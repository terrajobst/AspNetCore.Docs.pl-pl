---
title: Obszary w programie ASP.NET Core
author: rick-anderson
description: Dowiedz się, jak obszary są funkcją programu ASP.NET MVC, używane do organizowania powiązanych funkcji do grupy jako osobne przestrzeni nazw (w przypadku routingu) i struktury ich folderów (w przypadku widoków).
ms.author: riande
ms.date: 02/14/2017
uid: mvc/controllers/areas
ms.openlocfilehash: b78bb5146f1ab9039fa9ff015471654510718ed6
ms.sourcegitcommit: ecf2cd4e0613569025b28e12de3baa21d86d4258
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/30/2018
ms.locfileid: "43312221"
---
# <a name="areas-in-aspnet-core"></a>Obszary w programie ASP.NET Core

Przez [Kumara Dhananjay](https://twitter.com/debug_mode) i [Rick Anderson](https://twitter.com/RickAndMSFT)

Obszary są funkcją programu ASP.NET MVC, używane do organizowania powiązanych funkcji do grupy jako osobne przestrzeni nazw (w przypadku routingu) i struktury ich folderów (w przypadku widoków). Za pomocą obszarów tworzą hierarchię na potrzeby routingu, dodając innego parametru trasy, `area`, `controller` i `action`.

Obszary zapewniają sposób dzielenia dużych aplikacji sieci Web platformy ASP.NET Core MVC na mniejsze grupy funkcjonalnej. Obszar jest skutecznie strukturę MVC w aplikacji. W projekcie MVC logiczne składniki, takie jak Model, kontroler i Widok są przechowywane w różnych folderach, i są używane konwencje nazewnictwa do utworzenia relacji między tymi składnikami. W przypadku dużych aplikacji może być korzystne podzielić ją na oddzielnych wysokiego poziomu obszary funkcji. Na przykład aplikacja handlu elektronicznego z wielu jednostek biznesowych, takich jak wyszukiwanie itp wyewidencjonowanie i rozliczeniami. Każda z tych jednostek ma swoje własne widoki logiczny składnik, kontrolery i modeli. W tym scenariuszu można użyć obszarów do partycjonowania fizycznie składniki biznesowej, w tym samym projekcie.

Obszar mogą być definiowane jako mniejsze jednostki organizacyjne w projekcie programu ASP.NET Core MVC za pomocą swój własny zestaw kontrolerów, widoki i modele.

Należy rozważyć użycie obszarów w MVC projektu, gdy:

* Aplikacja składa się z wielu wysokiego poziomu funkcjonalności składników, które powinny zostać logicznie oddzielone

* Chcesz podzielić projektu MVC tak, aby każdy obszar funkcjonalny może się opracowaniem niezależnie

Funkcje obszaru:

* Aplikacja ASP.NET Core MVC może mieć dowolną liczbę obszarów.

* Każdy obszar ma swój własny, modeli, widoków i kontrolerów.

* Obszary umożliwiają organizowanie dużymi projektami MVC w wielu składników wysokiego poziomu, które mogą być realizowane niezależnie.

* Obszary obsługuje wiele kontrolerów o takiej samej nazwie, tak długo, jak długo mają różne *obszarów*.

Spójrzmy na przykład aby zilustrować, jak obszary są tworzone i używane. Załóżmy, że masz aplikację ze sklepu, która ma dwa oddzielne grupy widoków i kontrolerów: produktów i usług. Typowy folder struktury dla, że przy użyciu obszarów MVC wygląda jak poniżej:

* Nazwa projektu

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

    * Usługi

      * Kontrolery

        * HomeController.cs

      * Widoki

        * Home

          * Index.cshtml

Gdy do renderowania widoku w obszarze domyślnie podejmie próbę MVC, próbuje Szukaj w następujących lokalizacjach:

```text
/Areas/<Area-Name>/Views/<Controller-Name>/<Action-Name>.cshtml
   /Areas/<Area-Name>/Views/Shared/<Action-Name>.cshtml
   /Views/Shared/<Action-Name>.cshtml
   ```

Są to domyślne lokalizacje, które można zmienić za pośrednictwem `AreaViewLocationFormats` na `Microsoft.AspNetCore.Mvc.Razor.RazorViewEngineOptions`.

Na przykład w poniższego kodu, zamiast nazwy folderu jako "Obszary", został zmieniony na "Kategorie".

```csharp
services.Configure<RazorViewEngineOptions>(options =>
   {
       options.AreaViewLocationFormats.Clear();
       options.AreaViewLocationFormats.Add("/Categories/{2}/Views/{1}/{0}.cshtml");
       options.AreaViewLocationFormats.Add("/Categories/{2}/Views/Shared/{0}.cshtml");
       options.AreaViewLocationFormats.Add("/Views/Shared/{0}.cshtml");
   });
   ```

Jedno, należy pamiętać, jest strukturą *widoków* folder jest tylko jeden, co jest uznawane za ważne tutaj i zawartość w pozostałej części folderów, takich jak *kontrolerów* i *modeli* jest **nie** znaczenia. Na przykład, użytkownik nie musi mieć *kontrolerów* i *modeli* folderu w ogóle. To działa, ponieważ zawartość *kontrolerów* i *modeli* jest po prostu kod, który pobiera skompilowany w dll, gdzie jako zawartość *widoków* nie jest do żądania, Wyświetl zostały wprowadzone.

Po zdefiniowaniu hierarchii folderów należy MVC stwierdzić, że każdy kontroler jest skojarzony z obszarem. Można to zrobić, dekoracji nazwy kontrolera, za pomocą `[Area]` atrybutu.

```csharp
...
   namespace MyStore.Areas.Products.Controllers
   {
       [Area("Products")]
       public class HomeController : Controller
       {
           // GET: /Products/Home/Index
           public IActionResult Index()
           {
               return View();
           }

           // GET: /Products/Home/Create
           public IActionResult Create()
           {
               return View();
           }
       }
   }
   ```

Skonfiguruj definicję trasy, która współdziała z nowo utworzoną obszary. [Trasy do akcji kontrolera](routing.md) artykułu przechodzi do szczegółowych informacji dotyczących sposobu tworzenia definicji trasy, w tym o korzystaniu z konwencjonalnych trasy w porównaniu z trasami atrybutów. W tym przykładzie użyjemy konwencjonalne trasy. Aby to zrobić, otwórz *Startup.cs* plików i zmodyfikuj go, dodając `areaRoute` o nazwie definicji trasy.

```csharp
...
   app.UseMvc(routes =>
   {
     routes.MapRoute(
         name: "areaRoute",
         template: "{area:exists}/{controller=Home}/{action=Index}/{id?}");

     routes.MapRoute(
         name: "default",
         template: "{controller=Home}/{action=Index}/{id?}");
   });
   ```

Przechodzenie do `http://<yourApp>/products`, `Index` metody akcji `HomeController` w `Products` obszar, który zostanie wywołany.

## <a name="link-generation"></a>Generowanie konsolidacji

* Podczas generowania łączy z akcji wewnątrz obszaru na podstawie kontrolera do innej akcji w obrębie tego samego kontrolera.

  Załóżmy, że ścieżka bieżącego żądania jest podobne `/Products/Home/Create`

  Składnia HtmlHelper: `@Html.ActionLink("Go to Product's Home Page", "Index")`

  Składnia pomocnika tagów: `<a asp-action="Index">Go to Product's Home Page</a>`

  Należy pamiętać, że, firma Microsoft nie musi dostarczać wartości "obszar" i "controller" tutaj, ponieważ są one już dostępne w kontekście bieżącego żądania. Ten rodzaj wartości są nazywane `ambient` wartości.

* Podczas generowania łączy z akcji wewnątrz obszaru na podstawie kontrolera do kolejnej akcji na innym kontrolerze

  Załóżmy, że ścieżka bieżącego żądania jest podobne `/Products/Home/Create`

  Składnia HtmlHelper: `@Html.ActionLink("Go to Manage Products Home Page", "Index", "Manage")`

  Składnia pomocnika tagów: `<a asp-controller="Manage" asp-action="Index">Go to Manage Products Home Page</a>`

  Pamiętaj, że w tym miejscu zostanie użyta wartość otoczenia obszaru, ale jawnie określono wartość "controller", powyżej.

* Podczas generowania łączy z akcji w obrębie kontrolera do kolejnej akcji na podstawie innego kontrolera i innego obszaru.

  Załóżmy, że ścieżka bieżącego żądania jest podobne `/Products/Home/Create`

  Składnia HtmlHelper: `@Html.ActionLink("Go to Services Home Page", "Index", "Home", new { area = "Services" })`

  Składnia pomocnika tagów: `<a asp-area="Services" asp-controller="Home" asp-action="Index">Go to Services Home Page</a>`

  Należy pamiętać, że w tym miejscu są używane żadne wartości otoczenia.

* Podczas generowania łączy z akcji w kontrolerze obszaru na podstawie innej akcji na innym kontrolerze i **nie** w obszarze.

  Składnia HtmlHelper: `@Html.ActionLink("Go to Manage Products  Home Page", "Index", "Home", new { area = "" })`

  Składnia pomocnika tagów: `<a asp-area="" asp-controller="Manage" asp-action="Index">Go to Manage Products Home Page</a>`

  Ponieważ chcemy wygenerować łączy do innych obszaru na podstawie akcji kontrolera, możemy pusty otoczenia wartość "obszar" w tym miejscu.

## <a name="publishing-areas"></a>Obszary publikowania

Wszystkie `*.cshtml` i `wwwroot/**` plików, są publikowane dane wyjściowe, gdy `<Project Sdk="Microsoft.NET.Sdk.Web">` znajduje się w *.csproj* pliku.
