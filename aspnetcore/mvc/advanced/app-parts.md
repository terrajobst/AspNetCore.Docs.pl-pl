---
title: Części aplikacji platformy ASP.NET Core
author: ardalis
description: Dowiedz się, jak używać części aplikacji, które są abstrakcje nad zasobami aplikacji, do odnajdowania lub uniknąć obciążania funkcji z zestawu.
manager: wpickett
ms.author: riande
ms.date: 01/04/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: mvc/extensibility/app-parts
ms.openlocfilehash: 8f7aeadc7a1218bf203575add8c82c95faf137b4
ms.sourcegitcommit: 5130b3034165f5cf49d829fe7475a84aa33d2693
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 05/03/2018
---
# <a name="application-parts-in-aspnet-core"></a>Części aplikacji platformy ASP.NET Core

[Wyświetlić lub pobrać przykładowy kod](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/advanced/app-parts/sample) ([sposobu pobierania](xref:tutorials/index#how-to-download-a-sample))

*Część aplikacji* jest abstrakcję przy użyciu zasobów aplikacji, z którego MVC funkcjami, takimi jak kontrolery, składniki widoku lub pomocników tagów może zostać odnalezione. Przykładem części aplikacji jest AssemblyPart, który hermetyzuje odwołania do zestawu i ujawnia typy i odwołuje się do kompilacji. *Funkcja dostawców* pracować z części aplikacji do wypełniania funkcji aplikacji ASP.NET Core MVC. Przypadek użycia głównego dla części aplikacji jest umożliwienie użytkownikom skonfiguruj aplikację, aby odnaleźć (lub Unikaj ładowania) z zestawu funkcji MVC.

## <a name="introducing-application-parts"></a>Wprowadzenie do części aplikacji

Aplikacji MVC załadować ich funkcje z [części aplikacji](/dotnet/api/microsoft.aspnetcore.mvc.applicationparts.applicationpart). W szczególności [AssemblyPart](/dotnet/api/microsoft.aspnetcore.mvc.applicationparts.assemblypart#Microsoft_AspNetCore_Mvc_ApplicationParts_AssemblyPart) klasa reprezentuje część aplikacji, która nie jest obsługiwana przez zestaw. Aby odnaleźć i załadować MVC funkcje, takie jak kontrolerów, wyświetlania składników pomocników tagów i źródeł kompilacji razor, można użyć tych klas. [ApplicationPartManager](/dotnet/api/microsoft.aspnetcore.mvc.applicationparts.applicationpartmanager) jest odpowiedzialny za śledzenia części aplikacji i dostawców funkcji dostępnych w aplikacji MVC. Użytkownik może interakcyjnie przeprowadzić `ApplicationPartManager` w `Startup` podczas konfigurowania MVC:

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

Domyślnie MVC przeszukać drzewo zależności i znaleźć kontrolery (nawet w innych zestawów). Aby załadować dowolnego zestawu (na przykład z wtyczkę, która nie jest przywoływany w czasie kompilacji), można użyć część aplikacji.

Można użyć części aplikacji do *uniknąć* wyszukiwanie kontrolerów z określonego zestawu lub lokalizacji. Można kontrolować, które części (lub zestawy) są dostępne dla aplikacji, modyfikując `ApplicationParts` kolekcji `ApplicationPartManager`. Kolejność pozycji w `ApplicationParts` kolekcji nie jest ważna. Ważne jest, aby skonfigurować w pełni `ApplicationPartManager` przed użyciem jej do skonfigurowania usług w kontenerze. Na przykład pełni należy skonfigurować `ApplicationPartManager` przed wywołaniem `AddControllersAsServices`. Można to zrobić, będzie oznaczać, że kontrolerów w części aplikacji dodane po wywołanie metody nie będzie mieć wpływ (nie pobrać zarejestrowany jako usługi) może spowodować niepoprawne bevavior aplikacji.

Jeśli masz zestaw, który zawiera kontrolery nie chcesz do użycia, usunąć go z `ApplicationPartManager`:

```csharp
services.AddMvc()
    .ConfigureApplicationPartManager(apm =>
    {
        var dependentLibrary = apm.ApplicationParts
            .FirstOrDefault(part => part.Name == "DependentLibrary");

        if (dependentLibrary != null)
        {
           p.ApplicationParts.Remove(dependentLibrary);
        }
    })
```

Oprócz zestawu projektu i jego zestawów zależnych `ApplicationPartManager` będzie zawierać części dla `Microsoft.AspNetCore.Mvc.TagHelpers` i `Microsoft.AspNetCore.Mvc.Razor` domyślnie.

## <a name="application-feature-providers"></a>Dostawcy funkcji aplikacji

Dostawcy funkcji aplikacji Sprawdź części aplikacji i udostępnia funkcje dla tych elementów. Brak dostawców wbudowanych funkcji MVC następujące funkcje:

* [Kontrolery](/dotnet/api/microsoft.aspnetcore.mvc.controllers.controllerfeatureprovider)
* [Odwołanie do metadanych](/dotnet/api/microsoft.aspnetcore.mvc.razor.compilation.metadatareferencefeatureprovider)
* [Pomocnicy tagów](/dotnet/api/microsoft.aspnetcore.mvc.razor.taghelpers.taghelperfeatureprovider)
* [Składniki w widoku](/dotnet/api/microsoft.aspnetcore.mvc.viewcomponents.viewcomponentfeatureprovider)

Dziedzicz dostawców funkcji `IApplicationFeatureProvider<T>`, gdzie `T` jest typem funkcji. Można zaimplementować własnych funkcji dostawców dla dowolnego typu funkcji MVC wymienionych powyżej. Kolejność dostawców funkcji w `ApplicationPartManager.FeatureProviders` kolekcji może być ważne, ponieważ nowsze dostawców można reagować na akcje wykonywane przez poprzednie dostawców.

### <a name="sample-generic-controller-feature"></a>Przykład: Funkcja ogólnego kontrolera

Domyślnie program ASP.NET Core MVC ignoruje ogólnego kontrolerów (na przykład `SomeController<T>`). W przykładzie użyto dostawcy funkcji kontrolera, który jest uruchamiany po domyślnego dostawcy i dodaje wystąpienia kontrolera ogólne dla określonej listy typów (zdefiniowany w `EntityTypes.Types`):

[!code-csharp[](./app-parts/sample/AppPartsSample/GenericControllerFeatureProvider.cs?highlight=13&range=18-36)]

Typy jednostek:

[!code-csharp[](./app-parts/sample/AppPartsSample/Model/EntityTypes.cs?range=6-16)]

Dostawca funkcji został dodany w `Startup`:

```csharp
services.AddMvc()
    .ConfigureApplicationPartManager(apm => 
        apm.FeatureProviders.Add(new GenericControllerFeatureProvider()));
```

Domyślnie nazwy kontrolera ogólnego używane do przesyłania będzie mieć postać *GenericController'1 [Widget]* zamiast *elementu Widget*. Następującego atrybutu służy do modyfikowania nazwy odpowiadającej typu ogólnego używane przez kontrolera:

[!code-csharp[](./app-parts/sample/AppPartsSample/GenericControllerNameConvention.cs)]

`GenericController` Klasy:

[!code-csharp[](./app-parts/sample/AppPartsSample/GenericController.cs?highlight=5-6)]

Wynik, gdy wymagane jest pasującej trasy:

![Przykładowe dane wyjściowe z przykładowej aplikacji odczytuje powitania od kontrolera Sproket ogólnego.](app-parts/_static/generic-controller.png)

### <a name="sample-display-available-features"></a>Przykład: Wyświetlanie dostępnych funkcji

Można wykonać iterację wypełnione funkcje dostępne do aplikacji przez zażądanie `ApplicationPartManager` za pośrednictwem [iniekcji zależności](../../fundamentals/dependency-injection.md) i używanie go można wypełnić wystąpień odpowiednie funkcje:

[!code-csharp[](./app-parts/sample/AppPartsSample/Controllers/FeaturesController.cs?highlight=16,25-27)]

Przykład danych wyjściowych:

![Przykładowe dane wyjściowe z przykładowej aplikacji](app-parts/_static/available-features.png)
