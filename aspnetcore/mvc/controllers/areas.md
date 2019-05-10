---
title: Obszary w programie ASP.NET Core
author: rick-anderson
description: Dowiedz się, jak obszary są funkcją programu ASP.NET MVC, używane do organizowania powiązanych funkcji do grupy jako osobne przestrzeni nazw (w przypadku routingu) i struktury ich folderów (w przypadku widoków).
ms.author: riande
ms.date: 05/06/2019
uid: mvc/controllers/areas
ms.openlocfilehash: 35c7682861f7392b0bcda7326e4d7f5ccc356bda
ms.sourcegitcommit: b508b115107e0f8d7f62b25cfcc8ad45e1373459
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 05/07/2019
ms.locfileid: "65212588"
---
# <a name="areas-in-aspnet-core"></a>Obszary w programie ASP.NET Core

Przez [Kumara Dhananjay](https://twitter.com/debug_mode) i [Rick Anderson](https://twitter.com/RickAndMSFT)

Obszary są funkcją programu ASP.NET, używane do organizowania powiązanych funkcji do grupy jako osobne przestrzeni nazw (w przypadku routingu) i struktury ich folderów (w przypadku widoków). Za pomocą obszarów tworzą hierarchię na potrzeby routingu, dodając innego parametru trasy, `area`, `controller` i `action` lub strony Razor `page`.

Obszary umożliwiają partycji aplikacji sieci Web platformy ASP.NET Core na mniejsze grupy funkcjonalnej, każdy z swój własny zestaw stron Razor, kontrolerów, widoki i modele. Obszar skutecznie to struktura wewnątrz aplikacji. W projekcie sieci web platformy ASP.NET Core składników logicznych, takich jak strony, modelu, kontroler i Widok są przechowywane w różnych folderach. Środowisko uruchomieniowe programu ASP.NET Core używa konwencji nazewnictwa do utworzenia relacji między tymi składnikami. W przypadku dużych aplikacji może być korzystne podzielić ją na oddzielnych wysokiego poziomu obszary funkcji. Na przykład aplikacja handlu elektronicznego z wielu jednostek biznesowych, takich jak wyewidencjonowania, rozliczeń i wyszukiwania. Każda z tych jednostek ma swoje własne obszar zawiera widoki, kontrolery, stronami Razor i modeli.

Należy rozważyć użycie obszarów w projekcie po:

* Aplikacja składa się z wielu wysokiego poziomu funkcjonalności składników, które mogą zostać logicznie oddzielone.
* Chcesz podzielić na partycje aplikację tak, aby każdy obszar funkcjonalny może się opracowaniem niezależnie.

[Wyświetlanie lub pobieranie przykładowego kodu](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/controllers/areas/samples) ([sposobu pobierania](xref:index#how-to-download-a-sample)). Przykład pobierania zawiera podstawową aplikację do testowania obszarów.

Jeśli używasz stron Razor, zobacz [obszarów ze stronami Razor](#areas-with-razor-pages) w tym dokumencie.

## <a name="areas-for-controllers-with-views"></a>Obszary dla kontrolerów z widokami

Typowa aplikacja internetowa ASP.NET Core przy użyciu obszarów, widoków i kontrolerów zawiera następujące informacje:

* [Strukturę folderów obszaru](#area-folder-structure).
* Kontrolery ozdobione [ &lbrack;obszaru&rbrack; ](#attribute) atrybutu, aby skojarzyć kontroler z obszaru:

  [!code-csharp[](areas/samples/MVCareas/Areas/Products/Controllers/ManageController.cs?name=snippet2)]

* [Trasa obszaru dodana do uruchamiania](#add-area-route):

  [!code-csharp[](areas/samples/MVCareas/Startup.cs?name=snippet2&highlight=3-6)]

### <a name="area-folder-structure"></a>Struktura folderów obszaru

Należy wziąć pod uwagę aplikację, która ma dwa grup logicznych *produktów* i *usług*. Za pomocą obszarów, strukturę folderów będzie podobny do następującego:

* Project name (Nazwa projektu)
  * Obszary
    * Produkty
      * Kontrolery
        * HomeController.cs
        * ManageController.cs
      * Widoki
        * Home
          * Index.cshtml
        * Zarządzanie
          * Index.cshtml
          * About.cshtml
    * Usługi
      * Kontrolery
        * HomeController.cs
      * Widoki
        * Home
          * Index.cshtml

Podczas poprzedniego układ jest typowe w przypadku, gdy przy użyciu obszarów, Wyświetl pliki, należy użyć tej struktury folderów. Widok odnajdywania wyszukuje pasującego pliku widoku obszaru w następującej kolejności:

```text
/Areas/<Area-Name>/Views/<Controller-Name>/<Action-Name>.cshtml
/Areas/<Area-Name>/Views/Shared/<Action-Name>.cshtml
/Views/Shared/<Action-Name>.cshtml
/Pages/Shared/<Action-Name>.cshtml
   ```

Lokalizacji-view, folderów, takie jak *kontrolerów* i *modeli* jest **nie** znaczenia. Na przykład *kontrolerów* i *modeli* folderu nie są wymagane. Zawartość *kontrolerów* i *modeli* jest kod, który zostanie skompilowany w dll. Zawartość *widoków* nie jest kompilowana, dopóki nie wykonano żądania do tego widoku.

<a name="attribute"></a>

### <a name="associate-the-controller-with-an-area"></a>Skojarzyć kontroler z obszaru

Obszar kontrolerów zostały oznaczone za pomocą [ &lbrack;obszaru&rbrack; ](xref:Microsoft.AspNetCore.Mvc.AreaAttribute) atrybutu:

[!code-csharp[](areas/samples/MVCareas/Areas/Products/Controllers/ManageController.cs?highlight=5&name=snippet)]

### <a name="add-area-route"></a>Dodaj trasę obszaru

Obszar trasy używa się zazwyczaj do routingu konwencjonalna zamiast trasowanie atrybutów. Tradycyjnie routing jest zależna od kolejności. Ogólnie rzecz biorąc trasy z obszarami należy umieścić we wcześniejszej części tabeli tras, ponieważ są one bardziej szczegółowe niż trasy bez obszaru.

`{area:...}` może służyć jako token w szablonach tras Jeśli przestrzeń adresów url jest jednolita we wszystkich obszarach:

[!code-csharp[](areas/samples/MVCareas/Startup.cs?name=snippet&highlight=18-21)]

W poprzednim kodzie `exists` zastosuje ograniczenie, że trasy musi odpowiadać obszar. Za pomocą `{area:...}` jest najmniej skomplikowane mechanizm do dodawania routingu do obszarów.

Poniższy kod używa <xref:Microsoft.AspNetCore.Builder.MvcAreaRouteBuilderExtensions.MapAreaRoute*> utworzyć dwa o nazwie obszaru trasy:

[!code-csharp[](areas/samples/MVCareas/StartupMapAreaRoute.cs?name=snippet&highlight=18-27)]

Korzystając z `MapAreaRoute` za pomocą platformy ASP.NET Core 2.2, zobacz [problem w usłudze GitHub](https://github.com/aspnet/AspNetCore/issues/7772).

Aby uzyskać więcej informacji, zobacz [routingu obszaru](xref:mvc/controllers/routing#areas).

### <a name="link-generation-with-mvc-areas"></a>Generowanie konsolidacji z obszarami MVC

Poniższy kod z [pobrania próbki](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/controllers/areas/samples) pokazuje połączenie generacji z użyciem obszaru określony:

[!code-cshtml[](areas/samples/MVCareas/Views/Shared/_testLinksPartial.cshtml?name=snippet)]

Linki wygenerowane z poprzedniego kodu są prawidłowe w dowolnym miejscu aplikacji.

Przykładowe do pobrania obejmują programy [widoku częściowego](xref:mvc/views/partial) zawiera poprzednie linki i uruchamia łącze tak samo bez określenia obszaru. Odwołuje się widok częściowy [plik układu](xref:mvc/views/layout), więc co w aplikacji stronę Wyświetla łącza wygenerowany. Linki wygenerowany bez określenia obszaru są prawidłowe tylko gdy występuje do ze strony, w tym samym regionie i kontrolera.

Jeśli nie określono obszaru lub kontrolera, routing zależy od *otoczenia* wartości. Bieżące wartości trasy, bieżącego żądania są traktowane jako wartości otoczenia dotyczącymi generowania łączy. W wielu przypadkach dla przykładowej aplikacji przy użyciu wartości otoczenia generuje nieprawidłowe linki.

Aby uzyskać więcej informacji, zobacz [Routing do akcji kontrolera](xref:mvc/controllers/routing).

### <a name="shared-layout-for-areas-using-the-viewstartcshtml-file"></a>Udostępnione układu dla obszarów przy użyciu pliku _ViewStart.cshtml

Aby udostępnić typowych układu dla całej aplikacji, należy przenieść *_ViewStart.cshtml* do folderu głównego aplikacji.

<a name="rename"></a>

### <a name="change-default-area-folder-where-views-are-stored"></a>Zmień domyślny folder obszaru przechowywania widoków

Poniższy kod zmienia domyślny folder obszaru z `"Areas"` do `"MyAreas"`:

[!code-csharp[](areas/samples/MVCareas/Startup2.cs?name=snippet)]

<a name="arp"></a>

## <a name="areas-with-razor-pages"></a>Obszary ze stron Razor

Wymagaj obszarów ze stronami Razor i *obszarów /&lt;nazwy obszaru&gt;/strony* folder w katalogu głównym aplikacji. Następującą strukturę folderów jest używana z [pobrania próbki](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/controllers/areas/samples)

* Project name (Nazwa projektu)
  * Obszary
    * Produkty
      * Strony
        * _ViewImports
        * Informacje
        * Indeks
    * Usługi
      * Strony
        * Zarządzanie
          * Informacje
          * Indeks

### <a name="link-generation-with-razor-pages-and-areas"></a>Generowanie konsolidacji ze stronami Razor i obszary

Poniższy kod z [pobrania próbki](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/controllers/areas/samples/RPareas) pokazuje połączenie generacji z użyciem obszaru określony (na przykład `asp-area="Products"`):

[!code-cshtml[](areas/samples/RPareas/Pages/Shared/_testLinksPartial.cshtml?name=snippet)]

Linki wygenerowane z poprzedniego kodu są prawidłowe w dowolnym miejscu aplikacji.

Przykładowe do pobrania obejmują programy [widoku częściowego](xref:mvc/views/partial) zawiera poprzednie linki i uruchamia łącze tak samo bez określenia obszaru. Odwołuje się widok częściowy [plik układu](xref:mvc/views/layout), więc co w aplikacji stronę Wyświetla łącza wygenerowany. Linki wygenerowany bez określenia obszaru są prawidłowe tylko gdy występuje do ze strony, w tym samym regionie.

Jeśli nie określono obszaru, routing jest zależna od *otoczenia* wartości. Bieżące wartości trasy, bieżącego żądania są traktowane jako wartości otoczenia dotyczącymi generowania łączy. W wielu przypadkach dla przykładowej aplikacji przy użyciu wartości otoczenia generuje nieprawidłowe linki. Na przykład należy wziąć pod uwagę łącza wygenerowany na podstawie następującego kodu:

[!code-cshtml[](areas/samples/RPareas/Pages/Shared/_testLinksPartial.cshtml?name=snippet2)]

Dla poprzedniego kodu:

* Link wygenerowany na podstawie `<a asp-page="/Manage/About">` jest prawidłowy tylko podczas ostatniego żądania został dla strony w `Services` obszaru. Na przykład `/Services/Manage/`, `/Services/Manage/Index`, lub `/Services/Manage/About`.
* Link wygenerowany na podstawie `<a asp-page="/About">` jest prawidłowy tylko podczas ostatniego żądania został dla strony w `/Home`.
* Kod pochodzi z [pobrania próbki](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/controllers/areas/samples/RPareas).

### <a name="import-namespace-and-tag-helpers-with-viewimports-file"></a>Importowanie przy użyciu pliku _ViewImports przestrzeni nazw i pomocnicy tagów

A *_ViewImports* plik może zostać dodany do każdego obszaru *stron* folderu importowania przestrzeni nazw i pomocnicy tagów do każdej strony Razor, w tym folderze.

Należy wziąć pod uwagę *usług* obszaru przykładowy kod, który nie zawiera *_ViewImports* pliku. Ilustruje poniższy kod znaczników */Services/Manage/About* strona Razor:

[!code-cshtml[](areas/samples/RPareas/Areas/Services/Pages/Manage/About.cshtml)]

W poprzednim znaczników:

* W pełni kwalifikowana nazwa domeny może służyć do określania modelu (`@model RPareas.Areas.Services.Pages.Manage.AboutModel`).
* [Pomocników tagów](xref:mvc/views/tag-helpers/intro) są włączane przez `@addTagHelper *, Microsoft.AspNetCore.Mvc.TagHelpers`

Do pobrania próbki, w obszarze produktów zawiera następujące *_ViewImports* pliku:

[!code-cshtml[](areas/samples/RPareas/Areas/Products/Pages/_ViewImports.cshtml)]

Ilustruje poniższy kod znaczników */produkty/o* strona Razor:

[!code-cshtml[](areas/samples/RPareas/Areas/Products/Pages/About.cshtml)]

W poprzednim pliku przestrzeni nazw i `@addTagHelper` dyrektywy są importowane w pliku przez *Areas/Products/Pages/_ViewImports.cshtml* pliku.

Aby uzyskać więcej informacji, zobacz [Zarządzanie Pomocnik tagu zakresu](xref:mvc/views/tag-helpers/intro?view=aspnetcore-2.2#managing-tag-helper-scope) i [importowania dyrektywy udostępnione](xref:mvc/views/layout#importing-shared-directives).

### <a name="shared-layout-for-razor-pages-areas"></a>Układ udostępnionych obszarów stron Razor

Aby udostępnić typowych układu dla całej aplikacji, należy przenieść *_ViewStart.cshtml* do folderu głównego aplikacji.

### <a name="publishing-areas"></a>Obszary publikowania

Wszystkie `*.cshtml` i `wwwroot/**` plików, są publikowane dane wyjściowe, gdy `<Project Sdk="Microsoft.NET.Sdk.Web">` znajdują się w pliku the.csproj*.
