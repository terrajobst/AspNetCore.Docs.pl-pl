---
title: Obszary w programie ASP.NET Core
author: rick-anderson
description: Dowiedz się, jak obszary są funkcją programu ASP.NET MVC, używane do organizowania powiązanych funkcji do grupy jako osobne przestrzeni nazw (w przypadku routingu) i struktury ich folderów (w przypadku widoków).
ms.author: riande
ms.date: 02/14/2019
uid: mvc/controllers/areas
ms.openlocfilehash: c21eed04ea68512515da262b6b6895dc1a821039
ms.sourcegitcommit: 2c7ffe349eabdccf2ed748dd303ffd0ba6e1cfe3
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/26/2019
ms.locfileid: "56833530"
---
# <a name="areas-in-aspnet-core"></a>Obszary w programie ASP.NET Core

Przez [Kumara Dhananjay](https://twitter.com/debug_mode) i [Rick Anderson](https://twitter.com/RickAndMSFT)

Obszary są funkcją programu ASP.NET, używane do organizowania powiązanych funkcji do grupy jako osobne przestrzeni nazw (w przypadku routingu) i struktury ich folderów (w przypadku widoków). Za pomocą obszarów tworzą hierarchię na potrzeby routingu, dodając innego parametru trasy, `area`, `controller` i `action` lub strony Razor `page`.

Obszary umożliwiają partycji aplikacji sieci Web platformy ASP.NET Core na mniejsze grupy funkcjonalnej, każdy z swój własny zestaw stron Razor, kontrolerów, widoki i modele. Obszar skutecznie to struktura wewnątrz aplikacji. W projekcie sieci web platformy ASP.NET Core składników logicznych, takich jak strony, modelu, kontroler i Widok są przechowywane w różnych folderach. Środowisko uruchomieniowe programu ASP.NET Core używa konwencji nazewnictwa do utworzenia relacji między tymi składnikami. W przypadku dużych aplikacji może być korzystne podzielić ją na oddzielnych wysokiego poziomu obszary funkcji. Na przykład aplikacja handlu elektronicznego z wielu jednostek biznesowych, takich jak wyewidencjonowania, rozliczeń i wyszukiwania. Każda z tych jednostek ma swoje własne obszar zawiera widoki, kontrolery, stronami Razor i modeli.

Należy rozważyć użycie obszarów w projekcie po:

* Aplikacja składa się z wielu wysokiego poziomu funkcjonalności składników, które mogą zostać logicznie oddzielone.
* Chcesz podzielić na partycje aplikację tak, aby każdy obszar funkcjonalny może się opracowaniem niezależnie.

[Wyświetlanie lub pobieranie przykładowego kodu](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/controllers/areas/samples) ([sposobu pobierania](xref:index#how-to-download-a-sample)). Przykład pobierania zawiera podstawową aplikację do testowania obszarów.

## <a name="areas-for-controllers-with-views"></a>Obszary dla kontrolerów z widokami

Typowa aplikacja internetowa ASP.NET Core przy użyciu obszarów, widoków i kontrolerów zawiera następujące informacje:

* [Strukturę folderów obszaru](#area-folder-structure).
* Kontrolery ozdobione [ &lbrack;obszaru&rbrack; ](#attribute) atrybutu, aby skojarzyć kontroler z obszaru: [!code-csharp[](areas/samples/MVCareas/Areas/Products/Controllers/ManageController.cs?name=snippet2)]
* [Trasa obszaru dodana do uruchamiania](#add-area-route): [!code-csharp[](areas/samples/MVCareas/Startup.cs?name=snippet2&highlight=3-6)]

## <a name="area-folder-structure"></a>Struktura folderów obszaru
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

<!-- TODO review:
The content of the *Views* isn't compiled until a request to that view has been made.

What about precompiled views? 
 -->
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

### <a name="link-generation-with-areas"></a>Generowanie konsolidacji z obszarami

Poniższy kod z [pobrania próbki](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/controllers/areas/samples) pokazuje połączenie generacji z użyciem obszaru określony:

[!code-cshtml[](areas/samples/MVCareas/Views/Shared/_testLinksPartial.cshtml?name=snippet)]

Linki wygenerowane z poprzedniego kodu są prawidłowe w dowolnym miejscu aplikacji.

Przykładowe do pobrania obejmują programy [widoku częściowego](xref:mvc/views/partial) zawiera poprzednie linki i uruchamia łącze tak samo bez określenia obszaru. Odwołuje się widok częściowy [plik układu](), więc co w aplikacji stronę Wyświetla łącza wygenerowany. Linki wygenerowany bez określenia obszaru są prawidłowe tylko gdy występuje do ze strony, w tym samym regionie i kontrolera.

Jeśli nie określono obszaru lub kontrolera, routing zależy od *otoczenia* wartości. Bieżące wartości trasy, bieżącego żądania są traktowane jako wartości otoczenia dotyczącymi generowania łączy. W wielu przypadkach dla przykładowej aplikacji przy użyciu wartości otoczenia generuje nieprawidłowe linki.

Aby uzyskać więcej informacji, zobacz [Routing do akcji kontrolera](xref:mvc/controllers/routing).

### <a name="shared-layout-for-areas-using-the-viewstartcshtml-file"></a>Udostępnione układu dla obszarów przy użyciu pliku _ViewStart.cshtml

Aby udostępnić typowych układu dla całej aplikacji, należy przenieść *_ViewStart.cshtml* do folderu głównego aplikacji.

<!-- This section will be completed after https://github.com/aspnet/Docs/pull/10978 is merged.
<a name="arp"></a>

## Areas for Razor Pages
-->
<a name="rename"></a>

### <a name="change-default-area-folder-where-views-are-stored"></a>Zmień domyślny folder obszaru przechowywania widoków

Poniższy kod zmienia domyślny folder obszaru z `"Areas"` do `"MyAreas"`:

[!code-csharp[](areas/samples/MVCareas/Startup2.cs?name=snippet)]

<!-- TODO review - can we delete this. Areas doesn't change publishing - right? -->
### <a name="publishing-areas"></a>Obszary publikowania

Wszystkie `*.cshtml` i `wwwroot/**` plików, są publikowane dane wyjściowe, gdy `<Project Sdk="Microsoft.NET.Sdk.Web">` znajduje się w *.csproj* pliku.
