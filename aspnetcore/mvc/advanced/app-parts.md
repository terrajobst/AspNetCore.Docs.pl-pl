---
title: Części aplikacji w ASP.NET Core
author: rick-anderson
description: Udostępnianie kontrolerów, przeglądanie, Razor Pages i nie tylko za pomocą części aplikacji w ASP.NET Core
ms.author: riande
ms.date: 11/10/2019
uid: mvc/extensibility/app-parts
ms.openlocfilehash: 92c408adfad8af3dfd2572b625ae28ef24f64948
ms.sourcegitcommit: a7bbe3890befead19440075b05b9674351f98872
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/10/2019
ms.locfileid: "73905764"
---
# <a name="share-controllers-views-razor-pages-and-more-with-application-parts-in-aspnet-core"></a>Udostępnianie kontrolerów, widoków, Razor Pages i innych elementów aplikacji w programie ASP.NET Core

::: moniker range=">= aspnetcore-3.0"

Autor [Rick Anderson](https://twitter.com/RickAndMSFT)

[Wyświetlanie lub Pobieranie przykładowego kodu](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/advanced/app-parts) ([jak pobrać](xref:index#how-to-download-a-sample))

*Część aplikacji* to Abstrakcja zasobów aplikacji. Części aplikacji umożliwiają ASP.NET Core odnajdywania kontrolerów, wyświetlania składników, pomocników tagów, Razor Pages, źródeł kompilacji Razor i nie tylko. [AssemblyPart](/dotnet/api/microsoft.aspnetcore.mvc.applicationparts.assemblypart#Microsoft_AspNetCore_Mvc_ApplicationParts_AssemblyPart) jest częścią aplikacji. `AssemblyPart` hermetyzuje odwołanie do zestawu i udostępnia typy i odwołania do kompilacji.

*Dostawcy funkcji* pracują z częściami aplikacji, aby wypełnić funkcje aplikacji ASP.NET Core. Głównym przypadkiem użycia części aplikacji jest skonfigurowanie aplikacji do odnajdywania (lub unikania ładowania) ASP.NET Core funkcji z zestawu. Na przykład może być konieczne udostępnienie typowych funkcji między wieloma aplikacjami. Korzystając z części aplikacji, można udostępnić zestaw (DLL) zawierający kontrolery, widoki, Razor Pages, źródła kompilacji Razor, pomocników tagów i wiele innych aplikacji. Udostępnianie zestawu jest preferowane do duplikowania kodu w wielu projektach.

ASP.NET Core aplikacje ładują funkcje z <xref:System.Web.WebPages.ApplicationPart>. Klasa <xref:Microsoft.AspNetCore.Mvc.ApplicationParts.AssemblyPart> reprezentuje część aplikacji, która jest obsługiwana przez zestaw.

## <a name="load-aspnet-core-features"></a>Załaduj funkcje ASP.NET Core

Użyj klas `ApplicationPart` i `AssemblyPart` do odnajdywania i ładowania funkcji ASP.NET Core (kontrolerów, składników widoku itp.). <xref:Microsoft.AspNetCore.Mvc.ApplicationParts.ApplicationPartManager> śledzi dostępne składniki aplikacji i dostawców funkcji. `ApplicationPartManager` jest skonfigurowany w `Startup.ConfigureServices`:

[!code-csharp[](./app-parts/sample1/WebAppParts/Startup.cs?name=snippet)]

Poniższy kod stanowi alternatywny sposób konfigurowania `ApplicationPartManager` przy użyciu `AssemblyPart`:

[!code-csharp[](./app-parts/sample1/WebAppParts/Startup2.cs?name=snippet)]

Powyższe dwa przykłady kodu ładują `SharedController` z zestawu. `SharedController` nie znajduje się w projekcie aplikacji. Zobacz Pobieranie przykładowego [rozwiązania WebAppParts](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/advanced/app-parts/sample1/WebAppParts) .

### <a name="include-views"></a>Dołącz widoki

Aby uwzględnić widoki w zestawie:

* Dodaj następujący znacznik do pliku projektu udostępnionego:

  ```csproj
  <ItemGroup>
      <EmbeddedResource Include="Views\**\*.cshtml" />
  </ItemGroup>
  ```

* Dodaj <xref:Microsoft.Extensions.FileProviders.EmbeddedFileProvider> do <xref:Microsoft.AspNetCore.Mvc.Razor.RazorViewEngine>:

  [!code-csharp[](./app-parts/sample1/WebAppParts/StartupViews.cs?name=snippet&highlight=3-7)]

### <a name="prevent-loading-resources"></a>Zapobiegaj ładowaniu zasobów

Części aplikacji mogą służyć do *uniknięcia* ładowania zasobów w określonym zestawie lub lokalizacji. Dodaj lub Usuń elementy członkowskie kolekcji <xref:Microsoft.AspNetCore.Mvc.ApplicationParts>, aby ukryć lub udostępnić dostępne zasoby. Kolejność wpisów w kolekcji `ApplicationParts` nie jest ważna. Skonfiguruj `ApplicationPartManager` przed użyciem go do konfigurowania usług w kontenerze. Na przykład skonfiguruj `ApplicationPartManager` przed wywołaniem `AddControllersAsServices`. Wywołaj `Remove` w kolekcji `ApplicationParts`, aby usunąć zasób.

Poniższy kod używa <xref:Microsoft.AspNetCore.Mvc.ApplicationParts> do usuwania `MyDependentLibrary` z aplikacji: [!code-csharp[](./app-parts/sample1/WebAppParts/StartupRm.cs?name=snippet)]

`ApplicationPartManager` zawiera części dla:

* Zestaw aplikacji i zestawy zależne.
* `Microsoft.AspNetCore.Mvc.TagHelpers`.,
* `Microsoft.AspNetCore.Mvc.Razor`.,

## <a name="application-feature-providers"></a>Dostawcy funkcji aplikacji

Dostawcy funkcji aplikacji badają części aplikacji i udostępniają funkcje dla tych części. Istnieją Wbudowani dostawcy funkcji dla następujących funkcji ASP.NET Core:

* [Kontrolery](/dotnet/api/microsoft.aspnetcore.mvc.controllers.controllerfeatureprovider)
* [Pomocnicy tagów](/dotnet/api/microsoft.aspnetcore.mvc.razor.taghelpers.taghelperfeatureprovider)
* [Wyświetl składniki](/dotnet/api/microsoft.aspnetcore.mvc.viewcomponents.viewcomponentfeatureprovider)

Dostawcy funkcji dziedziczą z <xref:Microsoft.AspNetCore.Mvc.ApplicationParts.IApplicationFeatureProvider`1>, gdzie `T` jest typem funkcji. Dostawców funkcji można zaimplementować dla dowolnego z wcześniej wymienionych typów funkcji. Kolejność dostawców funkcji w `ApplicationPartManager.FeatureProviders` może mieć wpływ na zachowanie w czasie wykonywania. Później dodani dostawcy mogą reagować na akcje podejmowane przez wcześniej dodanych dostawców.

### <a name="generic-controller-feature"></a>Funkcja kontrolera ogólnego

ASP.NET Core ignoruje [Ogólne kontrolery](/dotnet/csharp/programming-guide/generics/generic-classes). Kontroler generyczny ma parametr typu (na przykład `MyController<T>`). Poniższy przykład dodaje wystąpienia kontrolera ogólnego dla określonej listy typów:

[!code-csharp[](./app-parts/sample2/AppPartsSample/GenericControllerFeatureProvider.cs?name=snippet)]

Typy są zdefiniowane w `EntityTypes.Types`:

[!code-csharp[](./app-parts/sample2/AppPartsSample/Models/EntityTypes.cs?name=snippet)]

Dostawca funkcji jest dodawany w `Startup`:

[!code-csharp[](./app-parts/sample2/AppPartsSample/Startup.cs?name=snippet)]

Nazwy kontrolerów ogólnych używane do routingu mają postać *GenericController ' 1 [widget]* , a nie *widżetu*. Następujący atrybut modyfikuje nazwę odpowiadającą typowi ogólnemu używanemu przez kontroler:

[!code-csharp[](./app-parts/sample2/AppPartsSample/GenericControllerNameConvention.cs)]

Klasa `GenericController`:

[!code-csharp[](./app-parts/sample2/AppPartsSample/GenericController.cs)]

Na przykład żądanie adresu URL `https://localhost:5001/Sprocket` powoduje następujące odpowiedzi:

```text
Hello from a generic Sprocket controller.
```

### <a name="display-available-features"></a>Wyświetlanie dostępnych funkcji

Funkcje dostępne dla aplikacji można wyliczyć, żądając `ApplicationPartManager` za pomocą [iniekcji zależności](../../fundamentals/dependency-injection.md):

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

::: moniker-end

::: moniker range="< aspnetcore-3.0"

Autor [Rick Anderson](https://twitter.com/RickAndMSFT)

[Wyświetlanie lub Pobieranie przykładowego kodu](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/advanced/app-parts) ([jak pobrać](xref:index#how-to-download-a-sample))

*Część aplikacji* to Abstrakcja zasobów aplikacji. Części aplikacji umożliwiają ASP.NET Core odnajdywania kontrolerów, wyświetlania składników, pomocników tagów, Razor Pages, źródeł kompilacji Razor i nie tylko. [AssemblyPart](/dotnet/api/microsoft.aspnetcore.mvc.applicationparts.assemblypart#Microsoft_AspNetCore_Mvc_ApplicationParts_AssemblyPart) jest częścią aplikacji. `AssemblyPart` hermetyzuje odwołanie do zestawu i udostępnia typy i odwołania do kompilacji.

*Dostawcy funkcji* pracują z częściami aplikacji, aby wypełnić funkcje aplikacji ASP.NET Core. Głównym przypadkiem użycia części aplikacji jest skonfigurowanie aplikacji do odnajdywania (lub unikania ładowania) ASP.NET Core funkcji z zestawu. Na przykład może być konieczne udostępnienie typowych funkcji między wieloma aplikacjami. Korzystając z części aplikacji, można udostępnić zestaw (DLL) zawierający kontrolery, widoki, Razor Pages, źródła kompilacji Razor, pomocników tagów i wiele innych aplikacji. Udostępnianie zestawu jest preferowane do duplikowania kodu w wielu projektach.

ASP.NET Core aplikacje ładują funkcje z <xref:System.Web.WebPages.ApplicationPart>. Klasa <xref:Microsoft.AspNetCore.Mvc.ApplicationParts.AssemblyPart> reprezentuje część aplikacji, która jest obsługiwana przez zestaw.

## <a name="load-aspnet-core-features"></a>Załaduj funkcje ASP.NET Core

Użyj klas `ApplicationPart` i `AssemblyPart` do odnajdywania i ładowania funkcji ASP.NET Core (kontrolerów, składników widoku itp.). <xref:Microsoft.AspNetCore.Mvc.ApplicationParts.ApplicationPartManager> śledzi dostępne składniki aplikacji i dostawców funkcji. `ApplicationPartManager` jest skonfigurowany w `Startup.ConfigureServices`:

[!code-csharp[](./app-parts/sample1/WebAppParts/Startup.cs?name=snippet)]

Poniższy kod stanowi alternatywny sposób konfigurowania `ApplicationPartManager` przy użyciu `AssemblyPart`:

[!code-csharp[](./app-parts/sample1/WebAppParts/Startup2.cs?name=snippet)]

Powyższe dwa przykłady kodu ładują `SharedController` z zestawu. `SharedController` nie znajduje się w projekcie aplikacji. Zobacz Pobieranie przykładowego [rozwiązania WebAppParts](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/advanced/app-parts/sample1/WebAppParts) .

### <a name="include-views"></a>Dołącz widoki

Aby uwzględnić widoki w zestawie:

* Dodaj następujący znacznik do pliku projektu udostępnionego:

  ```csproj
  <ItemGroup>
      <EmbeddedResource Include="Views\**\*.cshtml" />
  </ItemGroup>
  ```

* Dodaj <xref:Microsoft.Extensions.FileProviders.EmbeddedFileProvider> do <xref:Microsoft.AspNetCore.Mvc.Razor.RazorViewEngine>:

  [!code-csharp[](./app-parts/sample1/WebAppParts/StartupViews.cs?name=snippet&highlight=3-7)]

### <a name="prevent-loading-resources"></a>Zapobiegaj ładowaniu zasobów

Części aplikacji mogą służyć do *uniknięcia* ładowania zasobów w określonym zestawie lub lokalizacji. Dodaj lub Usuń elementy członkowskie kolekcji <xref:Microsoft.AspNetCore.Mvc.ApplicationParts>, aby ukryć lub udostępnić dostępne zasoby. Kolejność wpisów w kolekcji `ApplicationParts` nie jest ważna. Skonfiguruj `ApplicationPartManager` przed użyciem go do konfigurowania usług w kontenerze. Na przykład skonfiguruj `ApplicationPartManager` przed wywołaniem `AddControllersAsServices`. Wywołaj `Remove` w kolekcji `ApplicationParts`, aby usunąć zasób.

Poniższy kod używa <xref:Microsoft.AspNetCore.Mvc.ApplicationParts> do usuwania `MyDependentLibrary` z aplikacji: [!code-csharp[](./app-parts/sample1/WebAppParts/StartupRm.cs?name=snippet)]

`ApplicationPartManager` zawiera części dla:

* Zestaw aplikacji i zestawy zależne.
* `Microsoft.AspNetCore.Mvc.TagHelpers`.,
* `Microsoft.AspNetCore.Mvc.Razor`.,

## <a name="application-feature-providers"></a>Dostawcy funkcji aplikacji

Dostawcy funkcji aplikacji badają części aplikacji i udostępniają funkcje dla tych części. Istnieją Wbudowani dostawcy funkcji dla następujących funkcji ASP.NET Core:

* [Kontrolery](/dotnet/api/microsoft.aspnetcore.mvc.controllers.controllerfeatureprovider)
* [Pomocnicy tagów](/dotnet/api/microsoft.aspnetcore.mvc.razor.taghelpers.taghelperfeatureprovider)
* [Wyświetl składniki](/dotnet/api/microsoft.aspnetcore.mvc.viewcomponents.viewcomponentfeatureprovider)

Dostawcy funkcji dziedziczą z <xref:Microsoft.AspNetCore.Mvc.ApplicationParts.IApplicationFeatureProvider`1>, gdzie `T` jest typem funkcji. Dostawców funkcji można zaimplementować dla dowolnego z wcześniej wymienionych typów funkcji. Kolejność dostawców funkcji w `ApplicationPartManager.FeatureProviders` może mieć wpływ na zachowanie w czasie wykonywania. Później dodani dostawcy mogą reagować na akcje podejmowane przez wcześniej dodanych dostawców.

### <a name="generic-controller-feature"></a>Funkcja kontrolera ogólnego

ASP.NET Core ignoruje [Ogólne kontrolery](/dotnet/csharp/programming-guide/generics/generic-classes). Kontroler generyczny ma parametr typu (na przykład `MyController<T>`). Poniższy przykład dodaje wystąpienia kontrolera ogólnego dla określonej listy typów:

[!code-csharp[](./app-parts/sample2/AppPartsSample/GenericControllerFeatureProvider.cs?name=snippet)]

Typy są zdefiniowane w `EntityTypes.Types`:

[!code-csharp[](./app-parts/sample2/AppPartsSample/Models/EntityTypes.cs?name=snippet)]

Dostawca funkcji jest dodawany w `Startup`:

[!code-csharp[](./app-parts/sample2/AppPartsSample/Startup.cs?name=snippet)]

Nazwy kontrolerów ogólnych używane do routingu mają postać *GenericController ' 1 [widget]* , a nie *widżetu*. Następujący atrybut modyfikuje nazwę odpowiadającą typowi ogólnemu używanemu przez kontroler:

[!code-csharp[](./app-parts/sample2/AppPartsSample/GenericControllerNameConvention.cs)]

Klasa `GenericController`:

[!code-csharp[](./app-parts/sample2/AppPartsSample/GenericController.cs)]

Na przykład żądanie adresu URL `https://localhost:5001/Sprocket` powoduje następujące odpowiedzi:

```text
Hello from a generic Sprocket controller.
```

### <a name="display-available-features"></a>Wyświetlanie dostępnych funkcji

Funkcje dostępne dla aplikacji można wyliczyć, żądając `ApplicationPartManager` za pomocą [iniekcji zależności](../../fundamentals/dependency-injection.md):

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

::: moniker-end
