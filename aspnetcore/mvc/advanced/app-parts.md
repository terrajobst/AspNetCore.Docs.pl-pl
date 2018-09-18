---
title: Części aplikacji w programie ASP.NET Core
author: ardalis
description: Dowiedz się, jak za pomocą części aplikacji, które abstrakcje nad zasobami aplikacji, wykrywanie i uniknąć obciążania funkcji z zestawu.
ms.author: riande
ms.date: 01/04/2017
uid: mvc/extensibility/app-parts
ms.openlocfilehash: 41ae3fd4059844698ded4551dcedc8933ab8cff6
ms.sourcegitcommit: b2723654af4969a24545f09ebe32004cb5e84a96
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 09/18/2018
ms.locfileid: "46011316"
---
# <a name="application-parts-in-aspnet-core"></a>Części aplikacji w programie ASP.NET Core

[Wyświetlanie lub pobieranie przykładowego kodu](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/advanced/app-parts/sample) ([sposobu pobierania](xref:tutorials/index#how-to-download-a-sample))

*Aplikacji część* jest klasą abstrakcyjną nad zasobami aplikacji, z którego MVC funkcjami, jak kontrolerów, składniki widoków lub może zostać odnalezionych pomocników tagów. Jednym z przykładów aplikacji, część jest AssemblyPart, który hermetyzuje odwołania do zestawu i ujawnia typy i odwołania do kompilacji. *Funkcja dostawców* działają z części aplikacji, aby wypełnić funkcje aplikacji ASP.NET Core MVC. Głównie w przypadku części aplikacji jest umożliwienie skonfiguruj aplikację, aby odnaleźć (lub uniknąć ładowanie) funkcjami MVC z zestawu.

## <a name="introducing-application-parts"></a>Wprowadzenie do części aplikacji

Aplikacje MVC ładować swoje funkcje [części aplikacji](/dotnet/api/microsoft.aspnetcore.mvc.applicationparts.applicationpart). W szczególności [AssemblyPart](/dotnet/api/microsoft.aspnetcore.mvc.applicationparts.assemblypart#Microsoft_AspNetCore_Mvc_ApplicationParts_AssemblyPart) klasa reprezentuje część aplikacji, która jest wspierana przez zespół. W ramach tych zajęć umożliwia wykrycie i załadowanie funkcji MVC, takich jak kontrolery, składniki widoków, pomocników tagów i źródła kompilacji razor. [ApplicationPartManager](/dotnet/api/microsoft.aspnetcore.mvc.applicationparts.applicationpartmanager) jest odpowiedzialny za śledzenie części aplikacji i dostawców funkcji dostępnych w aplikacji MVC. Możesz wchodzić w interakcje z `ApplicationPartManager` w `Startup` po skonfigurowaniu MVC:

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

Domyślnie MVC przeszukać drzewo zależności i znaleźć kontrolery (nawet w przypadku innych zestawów). Aby załadować dowolny zestaw (na przykład z dodatek, który nie jest przywoływany w czasie kompilacji), można użyć części aplikacji.

Możesz użyć aplikacji składa się z części *uniknąć* szukasz kontrolerów z określonego zestawu lub lokalizacji. Można kontrolować, które części (lub zespoły) są dostępne dla aplikacji, modyfikując `ApplicationParts` zbiór `ApplicationPartManager`. Kolejność wpisy w `ApplicationParts` kolekcji nie są ważne. Jest ważne w pełni skonfigurować `ApplicationPartManager` przed jego użyciem, aby skonfigurować usługi w kontenerze. Na przykład w pełni należy skonfigurować `ApplicationPartManager` przed wywołaniem `AddControllersAsServices`. Kończy się niepowodzeniem to zrobić, oznacza, że kontrolerów w części aplikacji dodane po wywołanie metody nie ulegnie zmianie (nie będzie Zarejestruj się jako usługi) może spowodować niepoprawne działanie aplikacji.

Jeśli masz zestaw, który zawiera kontrolery nie chcesz być używane, usuń go z `ApplicationPartManager`:

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

Oprócz zestawu projektu i jego zestawów zależnych `ApplicationPartManager` będzie zawierać części do `Microsoft.AspNetCore.Mvc.TagHelpers` i `Microsoft.AspNetCore.Mvc.Razor` domyślnie.

## <a name="application-feature-providers"></a>Dostawcy funkcji aplikacji

Dostawcy funkcji aplikacji Sprawdź części aplikacji i udostępnia funkcje w tych częściach. Dostępne są następujące funkcje MVC dostawców wbudowanej funkcji:

* [Kontrolery](/dotnet/api/microsoft.aspnetcore.mvc.controllers.controllerfeatureprovider)
* [Odwołanie do metadanych](/dotnet/api/microsoft.aspnetcore.mvc.razor.compilation.metadatareferencefeatureprovider)
* [Pomocnicy tagów](/dotnet/api/microsoft.aspnetcore.mvc.razor.taghelpers.taghelperfeatureprovider)
* [Składniki widoków](/dotnet/api/microsoft.aspnetcore.mvc.viewcomponents.viewcomponentfeatureprovider)

Dziedzicz dostawców funkcji `IApplicationFeatureProvider<T>`, gdzie `T` jest typem funkcji. Można zaimplementować własną funkcji, którą dostawców dla dowolnego typu funkcji MVC wymienionych powyżej. Kolejność dostawców funkcji w `ApplicationPartManager.FeatureProviders` kolekcji może być istotne, od dostawców nowszego pozwala reagować na akcje wykonywane przez dostawców poprzedniego.

### <a name="sample-generic-controller-feature"></a>Próbki: Funkcja ogólna kontrolera

Domyślnie program ASP.NET Core MVC ignoruje ogólnego kontrolerów (na przykład `SomeController<T>`). W tym przykładzie użyto dostawcy funkcji kontrolera, który jest uruchamiany po domyślnego dostawcę i dodaje wystąpień ogólnego kontrolera dla określonej listy typów (zdefiniowane w `EntityTypes.Types`):

[!code-csharp[](./app-parts/sample/AppPartsSample/GenericControllerFeatureProvider.cs?highlight=13&range=18-36)]

Typy jednostek:

[!code-csharp[](./app-parts/sample/AppPartsSample/Model/EntityTypes.cs?range=6-16)]

Dostawca funkcji zostanie dodany do `Startup`:

```csharp
services.AddMvc()
    .ConfigureApplicationPartManager(apm => 
        apm.FeatureProviders.Add(new GenericControllerFeatureProvider()));
```

Domyślnie nazwy rodzajowe kontrolerów, używany do routingu będzie mieć postać *GenericController'1 [widżet]* zamiast *widżet*. Następujący atrybut jest używane do modyfikowania nazwy odpowiadają typu ogólnego, używane przez kontroler:

[!code-csharp[](./app-parts/sample/AppPartsSample/GenericControllerNameConvention.cs)]

`GenericController` Klasy:

[!code-csharp[](./app-parts/sample/AppPartsSample/GenericController.cs?highlight=5-6)]

Wynik, zleconą pasującej trasy:

![Przykładowe dane wyjściowe z przykładowej aplikacji odczytuje pozdrowienia z kontrolera Sproket ogólnego.](app-parts/_static/generic-controller.png)

### <a name="sample-display-available-features"></a>Przykład: Wyświetlanie dostępnych funkcji

Można wykonać iterację wypełnione funkcji dostępnych do swojej aplikacji, wysyłając żądanie `ApplicationPartManager` za pośrednictwem [wstrzykiwanie zależności](../../fundamentals/dependency-injection.md) i używanie ich do wystąpienia odpowiednie funkcje:

[!code-csharp[](./app-parts/sample/AppPartsSample/Controllers/FeaturesController.cs?highlight=16,25-27)]

Przykładowe dane wyjściowe:

![Przykładowe dane wyjściowe z przykładowej aplikacji](app-parts/_static/available-features.png)
