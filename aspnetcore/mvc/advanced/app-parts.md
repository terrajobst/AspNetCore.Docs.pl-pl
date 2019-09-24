---
title: Części aplikacji w ASP.NET Core
author: rick-anderson
description: Udostępnianie kontrolerów, przeglądanie, Razor Pages i nie tylko za pomocą części aplikacji w ASP.NET Core
ms.author: riande
ms.date: 05/14/2019
uid: mvc/extensibility/app-parts
ms.openlocfilehash: 4b4c8c554a7045a180b56cf9998ab1a8496cde1b
ms.sourcegitcommit: 79eeb17604b536e8f34641d1e6b697fb9a2ee21f
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 09/24/2019
ms.locfileid: "71207351"
---
# <a name="share-controllers-views-razor-pages-and-more-with-application-parts-in-aspnet-core"></a>Udostępnianie kontrolerów, widoków, Razor Pages i innych elementów aplikacji w programie ASP.NET Core

Przez [Rick Anderson](https://twitter.com/RickAndMSFT)

[Wyświetlanie lub pobieranie przykładowego kodu](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/advanced/app-parts) ([sposobu pobierania](xref:index#how-to-download-a-sample))

*Część aplikacji* to Abstrakcja zasobów aplikacji. Części aplikacji umożliwiają ASP.NET Core odnajdywania kontrolerów, wyświetlania składników, pomocników tagów, Razor Pages, źródeł kompilacji Razor i nie tylko. [AssemblyPart](/dotnet/api/microsoft.aspnetcore.mvc.applicationparts.assemblypart#Microsoft_AspNetCore_Mvc_ApplicationParts_AssemblyPart) jest częścią aplikacji. `AssemblyPart`hermetyzuje odwołanie do zestawu i udostępnia typy i odwołania do kompilacji.

*Dostawcy funkcji* pracują z częściami aplikacji, aby wypełnić funkcje aplikacji ASP.NET Core. Głównym przypadkiem użycia części aplikacji jest skonfigurowanie aplikacji do odnajdywania (lub unikania ładowania) ASP.NET Core funkcji z zestawu. Na przykład może być konieczne udostępnienie typowych funkcji między wieloma aplikacjami. Korzystając z części aplikacji, można udostępnić zestaw (DLL) zawierający kontrolery, widoki, Razor Pages, źródła kompilacji Razor, pomocników tagów i wiele innych aplikacji. Udostępnianie zestawu jest preferowane do duplikowania kodu w wielu projektach.

ASP.NET Core aplikacje ładują funkcje <xref:System.Web.WebPages.ApplicationPart>z programu. <xref:Microsoft.AspNetCore.Mvc.ApplicationParts.AssemblyPart> Klasa reprezentuje część aplikacji, która jest obsługiwana przez zestaw.

## <a name="load-aspnet-core-features"></a>Załaduj funkcje ASP.NET Core

Użyj klas `AssemblyPart` i, aby odnajdywać i ładować funkcje ASP.NET Core (kontrolery, składniki widoku itp.). `ApplicationPart` <xref:Microsoft.AspNetCore.Mvc.ApplicationParts.ApplicationPartManager> Śledzi dostępne części aplikacji i dostawców funkcji. `ApplicationPartManager`jest skonfigurowany w `Startup.ConfigureServices`:

[!code-csharp[](./app-parts/sample1/WebAppParts/Startup.cs?name=snippet)]

Poniższy kod stanowi alternatywny sposób konfigurowania `ApplicationPartManager` użycia: `AssemblyPart`

[!code-csharp[](./app-parts/sample1/WebAppParts/Startup2.cs?name=snippet)]

Powyższe dwa przykłady kodu ładują `SharedController` z zestawu. Nie `SharedController` znajduje się w projekcie aplikacji. Zobacz Pobieranie przykładowego [rozwiązania WebAppParts](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/advanced/app-parts/sample1/WebAppParts) .

### <a name="include-views"></a>Dołącz widoki

Aby uwzględnić widoki w zestawie:

* Dodaj następujący znacznik do pliku projektu udostępnionego:

  ```csproj
  <ItemGroup>
      <EmbeddedResource Include="Views\**\*.cshtml" />
  </ItemGroup>
  ```

* <xref:Microsoft.Extensions.FileProviders.EmbeddedFileProvider> Dodaj<xref:Microsoft.AspNetCore.Mvc.Razor.RazorViewEngine>do:

  [!code-csharp[](./app-parts/sample1/WebAppParts/StartupViews.cs?name=snippet&highlight=3-7)]

### <a name="prevent-loading-resources"></a>Zapobiegaj ładowaniu zasobów

Części aplikacji mogą służyć do *uniknięcia* ładowania zasobów w określonym zestawie lub lokalizacji. Dodaj lub Usuń elementy członkowskie <xref:Microsoft.AspNetCore.Mvc.ApplicationParts> kolekcji, aby ukryć lub udostępnić dostępne zasoby. Kolejność wpisów w `ApplicationParts` kolekcji nie jest ważna. Skonfiguruj program `ApplicationPartManager` przed użyciem go do konfigurowania usług w kontenerze. Na przykład skonfiguruj `ApplicationPartManager` przed wywołaniem `AddControllersAsServices`. Wywołaj `Remove`kolekcję,abyusunąć zasób.`ApplicationParts`

Następujący kod używa <xref:Microsoft.AspNetCore.Mvc.ApplicationParts> do usunięcia `MyDependentLibrary` z aplikacji:[!code-csharp[](./app-parts/sample1/WebAppParts/StartupRm.cs?name=snippet)]

`ApplicationPartManager` Zawiera części dla:

* Zestaw aplikacji i zestawy zależne.
* `Microsoft.AspNetCore.Mvc.TagHelpers`.
* `Microsoft.AspNetCore.Mvc.Razor`.

## <a name="application-feature-providers"></a>Dostawcy funkcji aplikacji

Dostawcy funkcji aplikacji badają części aplikacji i udostępniają funkcje dla tych części. Istnieją Wbudowani dostawcy funkcji dla następujących funkcji ASP.NET Core:

* [Kontrolery](/dotnet/api/microsoft.aspnetcore.mvc.controllers.controllerfeatureprovider)
* [Pomocnicy tagów](/dotnet/api/microsoft.aspnetcore.mvc.razor.taghelpers.taghelperfeatureprovider)
* [Wyświetl składniki](/dotnet/api/microsoft.aspnetcore.mvc.viewcomponents.viewcomponentfeatureprovider)

Dostawcy funkcji dziedziczą <xref:Microsoft.AspNetCore.Mvc.ApplicationParts.IApplicationFeatureProvider`1>z, `T` gdzie jest typem funkcji. Dostawców funkcji można zaimplementować dla dowolnego z wcześniej wymienionych typów funkcji. Kolejność dostawców funkcji w programie `ApplicationPartManager.FeatureProviders` może mieć wpływ na zachowanie. Później dodani dostawcy mogą reagować na akcje podejmowane przez wcześniej dodanych dostawców.

### <a name="generic-controller-feature"></a>Funkcja kontrolera ogólnego

ASP.NET Core ignoruje [Ogólne kontrolery](/dotnet/csharp/programming-guide/generics/generic-classes). Kontroler generyczny ma parametr typu (na przykład `MyController<T>`). Poniższy przykład dodaje wystąpienia kontrolera ogólnego dla określonej listy typów:

[!code-csharp[](./app-parts/sample2/AppPartsSample/GenericControllerFeatureProvider.cs?name=snippet)]

Typy są zdefiniowane w `EntityTypes.Types`:

[!code-csharp[](./app-parts/sample2/AppPartsSample/Models/EntityTypes.cs?name=snippet)]

Dostawca funkcji został dodany w `Startup`:

[!code-csharp[](./app-parts/sample2/AppPartsSample/Startup.cs?name=snippet)]

Nazwy kontrolerów ogólnych używane do routingu mają postać *GenericController ' 1 [widget]* , a nie *widżetu*. Następujący atrybut modyfikuje nazwę odpowiadającą typowi ogólnemu używanemu przez kontroler:

[!code-csharp[](./app-parts/sample2/AppPartsSample/GenericControllerNameConvention.cs)]

`GenericController` Klasa:

[!code-csharp[](./app-parts/sample2/AppPartsSample/GenericController.cs)]

Na przykład żądanie adresu URL `https://localhost:5001/Sprocket` wyników w następującej odpowiedzi:

```text
Hello from a generic Sprocket controller.
```

### <a name="display-available-features"></a>Wyświetlanie dostępnych funkcji

Funkcje dostępne dla aplikacji można wyliczyć, żądając `ApplicationPartManager` od [iniekcji zależności](../../fundamentals/dependency-injection.md):

[!code-csharp[](./app-parts/sample2/AppPartsSample/Controllers/FeaturesController.cs?highlight=16,25-27)]

[Przykład pobierania](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/advanced/app-parts/sample2) używa powyższego kodu, aby wyświetlić funkcje aplikacji:

```text
Controllers:
    - FeaturesController
    - HomeController
    - HelloController
    - GenericController`1
    - GenericController`1
Tag Helpers:
    - PrerenderTagHelper
    - AnchorTagHelper
    - CacheTagHelper
    - DistributedCacheTagHelper
    - EnvironmentTagHelper
    - Additional Tag Helpers omitted for brevity.
View Components:
    - SampleViewComponent
```
