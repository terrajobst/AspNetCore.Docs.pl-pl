---
title: Części aplikacji w ASP.NET Core
author: ardalis
description: Dowiedz się, jak używać części aplikacji, które są abstrakcją za pośrednictwem zasobów aplikacji, aby odnaleźć lub uniknąć ładowania funkcji z zestawu.
ms.author: riande
ms.date: 01/04/2017
uid: mvc/extensibility/app-parts
ms.openlocfilehash: 4900ccf5589500db076f8cecd9da198c6a7ceea4
ms.sourcegitcommit: 41f2c1a6b316e6e368a4fd27a8b18d157cef91e1
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/21/2019
ms.locfileid: "69886465"
---
<!-- DO NOT MAKE CHANGES BEFORE https://github.com/aspnet/AspNetCore.Docs/pull/12376 Merges -->

# <a name="application-parts-in-aspnet-core"></a>Części aplikacji w ASP.NET Core

[Wyświetlanie lub pobieranie przykładowego kodu](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/advanced/app-parts/sample) ([sposobu pobierania](xref:index#how-to-download-a-sample))

*Część aplikacji* to Abstrakcja zasobów aplikacji, z której mogą zostać odnalezione funkcje MVC, takie jak kontrolery, składniki widoku lub pomocniki tagów. Przykładem części aplikacji jest AssemblyPart, który hermetyzuje odwołanie do zestawu i ujawnia typy i odwołania do kompilacji. *Dostawcy funkcji* pracują z częściami aplikacji, aby wypełnić funkcje aplikacji ASP.NET Core MVC. Głównym przypadkiem użycia części aplikacji jest umożliwienie konfigurowania aplikacji w celu odnajdywania (lub unikania ładowania) funkcji MVC z zestawu.

## <a name="introducing-application-parts"></a>Wprowadzenie do części aplikacji

Aplikacje MVC ładują swoje funkcje z [części aplikacji](/dotnet/api/microsoft.aspnetcore.mvc.applicationparts.applicationpart). W szczególności Klasa [AssemblyPart](/dotnet/api/microsoft.aspnetcore.mvc.applicationparts.assemblypart) reprezentuje część aplikacji, która jest obsługiwana przez zestaw. Tych klas można używać do odnajdywania i ładowania funkcji MVC, takich jak kontrolery, składniki widoku, pomocnicy tagów i źródła kompilacji Razor. [ApplicationPartManager](/dotnet/api/microsoft.aspnetcore.mvc.applicationparts.applicationpartmanager) jest odpowiedzialny za śledzenie składników aplikacji i dostawców funkcji dostępnych dla aplikacji MVC. Podczas konfigurowania MVC można korzystać `ApplicationPartManager` z `Startup` programu w programie:

```csharp
// create an assembly part from a class's assembly
var assembly = typeof(Startup).GetTypeInfo().Assembly;
services.AddMvc()
    .AddApplicationPart(assembly);

// OR
var assembly = typeof(Startup).GetTypeInfo().Assembly;
var part = new AssemblyPart(assembly);
services.AddMvc()
    .ConfigureApplicationPartManager(apm => apm.ApplicationParts.Add(part));
```

Domyślnie MVC przeszuka drzewo zależności i znajdzie kontrolery (nawet w innych zestawach). Do załadowania dowolnego zestawu (na przykład z wtyczki, do której nie odwołuje się w czasie kompilacji), można użyć części aplikacji.

Możesz użyć części aplikacji, aby *uniknąć* wyszukiwania kontrolerów w określonym zestawie lub lokalizacji. Można kontrolować, które części (lub zestawy) są dostępne dla aplikacji, modyfikując `ApplicationParts` kolekcję. `ApplicationPartManager` Kolejność wpisów w `ApplicationParts` kolekcji nie jest ważna. Ważne jest, `ApplicationPartManager` aby w pełni skonfigurować usługę przed jej użyciem do konfigurowania usług w kontenerze. Na przykład należy w pełni skonfigurować `ApplicationPartManager` przed wywołaniem. `AddControllersAsServices` W przeciwnym razie oznacza to, że nie będzie to miało wpływu na kontrolery w składnikach aplikacji dodanych po wywołaniu tej metody (nie zostaną one zarejestrowane jako usługi), co może spowodować nieprawidłowe zachowanie aplikacji.

Jeśli masz zestaw zawierający kontrolery, których nie chcesz używać, usuń go z `ApplicationPartManager`:

```csharp
services.AddMvc()
    .ConfigureApplicationPartManager(apm =>
    {
        var dependentLibrary = apm.ApplicationParts
            .FirstOrDefault(part => part.Name == "DependentLibrary");

        if (dependentLibrary != null)
        {
           apm.ApplicationParts.Remove(dependentLibrary);
        }
    })
```

Oprócz zestawu projektu i jego zestawów zależnych, `ApplicationPartManager` program będzie zawierać części dla `Microsoft.AspNetCore.Mvc.TagHelpers` i `Microsoft.AspNetCore.Mvc.Razor` domyślnie.

## <a name="application-feature-providers"></a>Dostawcy funkcji aplikacji

Dostawcy funkcji aplikacji badają części aplikacji i udostępniają funkcje dla tych części. Istnieją Wbudowani dostawcy funkcji dla następujących funkcji MVC:

* [Kontrolery](/dotnet/api/microsoft.aspnetcore.mvc.controllers.controllerfeatureprovider)
* [Odwołanie do metadanych](/dotnet/api/microsoft.aspnetcore.mvc.razor.compilation.metadatareferencefeatureprovider)
* [Pomocnicy tagów](/dotnet/api/microsoft.aspnetcore.mvc.razor.taghelpers.taghelperfeatureprovider)
* [Wyświetl składniki](/dotnet/api/microsoft.aspnetcore.mvc.viewcomponents.viewcomponentfeatureprovider)

Dostawcy funkcji dziedziczą `IApplicationFeatureProvider<T>`z, `T` gdzie jest typem funkcji. Możesz zaimplementować własnych dostawców funkcji dla dowolnego z wymienionych powyżej typów funkcji MVC. Kolejność dostawców funkcji w `ApplicationPartManager.FeatureProviders` kolekcji może być ważna, ponieważ później dostawcy mogą reagować na działania podejmowane przez wcześniejszych dostawców.

### <a name="sample-generic-controller-feature"></a>Northwind Funkcja kontrolera ogólnego

Domyślnie ASP.NET Core MVC ignoruje ogólne kontrolery (na przykład `SomeController<T>`). Ten przykład korzysta z dostawcy funkcji kontrolera, który jest uruchamiany po domyślnym dostawcy i dodaje wystąpienia kontrolera ogólnego dla określonej listy typów (zdefiniowane w `EntityTypes.Types`):

[!code-csharp[](./app-parts/sample/AppPartsSample/GenericControllerFeatureProvider.cs?highlight=13&range=18-36)]

Typy jednostek:

[!code-csharp[](./app-parts/sample/AppPartsSample/Model/EntityTypes.cs?range=6-16)]

Dostawca funkcji został dodany w `Startup`:

```csharp
services.AddMvc()
    .ConfigureApplicationPartManager(apm => 
        apm.FeatureProviders.Add(new GenericControllerFeatureProvider()));
```

Domyślnie nazwy kontrolerów ogólnych używane do routingu byłyby postać *GenericController ' 1 [widget]* zamiast *widgetu*. Następujący atrybut służy do modyfikowania nazwy, aby odpowiadała typowi ogólnemu używanemu przez kontroler:

[!code-csharp[](./app-parts/sample/AppPartsSample/GenericControllerNameConvention.cs)]

`GenericController` Klasa:

[!code-csharp[](./app-parts/sample/AppPartsSample/GenericController.cs?highlight=5-6)]

Wynik, gdy żąda się zgodnej trasy:

![Przykładowe dane wyjściowe z przykładowej aplikacji odczytuje "Hello z ogólnego kontrolera Sprocket".](app-parts/_static/generic-controller.png)

### <a name="sample-display-available-features"></a>Northwind Wyświetlanie dostępnych funkcji

Można wykonać iterację wypełnionych funkcji dostępnych dla aplikacji, żądając `ApplicationPartManager` od iniekcji [zależności](../../fundamentals/dependency-injection.md) i używając go do wypełniania wystąpień odpowiednich funkcji:

[!code-csharp[](./app-parts/sample/AppPartsSample/Controllers/FeaturesController.cs?highlight=16,25-27)]

Przykładowe dane wyjściowe:

![Przykładowe dane wyjściowe z przykładowej aplikacji](app-parts/_static/available-features.png)
