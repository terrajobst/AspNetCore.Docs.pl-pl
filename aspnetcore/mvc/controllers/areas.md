---
title: Obszary platformy ASP.NET Core
author: rick-anderson
description: Dowiedz się, jak kwestie funkcji ASP.NET MVC, używane do organizowania funkcje w grupie jako osobne w obszarze nazw (routing) i struktury folderów (dla widoków).
manager: wpickett
ms.author: riande
ms.date: 02/14/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: mvc/controllers/areas
ms.openlocfilehash: 61527eb350b5aba9cb37b1de5acdeae1287bf073
ms.sourcegitcommit: 48beecfe749ddac52bc79aa3eb246a2dcdaa1862
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/22/2018
ms.locfileid: "30072724"
---
# <a name="areas-in-aspnet-core"></a>Obszary platformy ASP.NET Core

Przez [Dhananjay Kumar](https://twitter.com/debug_mode) i [Rick Anderson](https://twitter.com/RickAndMSFT)

Kwestie funkcji ASP.NET MVC, używane do organizowania funkcje w grupie jako osobne w obszarze nazw (routing) i struktury folderów (dla widoków). Za pomocą obszarów tworzy hierarchię na potrzeby routingu, dodając inny parametru trasy, `area`, do `controller` i `action`.

Obszary zapewniają sposób podziału dużych aplikacji sieci Web platformy ASP.NET Core MVC na mniejsze funkcjonalności grupowania. Obszar skutecznie to struktura MVC wewnątrz aplikacji. W projekcie MVC składników logicznych, takich jak Model, kontrolera i widoku są przechowywane w różnych folderach i MVC używa konwencji nazewnictwa do utworzenia relacji między tymi składnikami. W przypadku dużych aplikacji może być korzystne partycji aplikacji na oddzielnych wysokiej obszary poziomu funkcjonalności. Na przykład aplikacji handlu elektronicznego z wiele jednostek biznesowych, takich jak wyewidencjonowania, rozliczeń i wyszukiwania itd. Każdy z tych jednostek ma własne logiczny składnik widoki, kontrolery i modeli. W tym scenariuszu można obszarów fizycznie partycji składników biznesowej w tym samym projekcie.

Obszar może być zdefiniowany jako mniejsze jednostki organizacyjne w projekcie platformy ASP.NET Core MVC z zestawem kontrolerów, widoków i modeli.

Należy rozważyć użycie obszarów MVC projektu, gdy:

* Aplikacja składa się z wielu wysokiego poziomu funkcjonalności składników, które powinny być oddzielone logicznie

* Aby partycji projektu MVC, dzięki czemu obszarów funkcjonalnych mogą wykonywać niezależnie

Funkcje obszaru:

* Aplikacja ASP.NET Core MVC może mieć dowolną liczbę obszarów

* Każdy obszar ma swoją własną kontrolerów, modeli i widoków

* Umożliwia organizowanie dużych projektów MVC w wielu składników wysokiego poziomu, które mogą wykonywać niezależnie

* Obsługuje wiele kontrolerów o takiej samej nazwie — tak długo, jak długo mają różne *obszarów*

Spójrzmy na przykład ilustrujący sposób obszarów są tworzone i używane. Załóżmy, że masz aplikację sklepu, która ma dwa oddzielne grupy kontrolery i widoki: produktów i usług. Typowy folder struktury czy za pomocą obszarów MVC wygląda jak poniżej:

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

Gdy do renderowania widoku w obszarze domyślnie podejmie próbę MVC, próbuje znaleźć w następujących lokalizacjach:

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

Jedyną operacją, należy pamiętać, jest strukturą *widoków* folder jest tylko jeden, który jest uznawany za ważny w tym miejscu i zawartości pozostałej części foldery, takich jak *kontrolerów* i *modele* jest **nie** znaczenia. Na przykład można nie wymagają *kontrolerów* i *modele* folderu wcale. To działa, ponieważ zawartość *kontrolerów* i *modeli* jest tylko kod, który pobiera skompilowany w Biblioteka DLL, gdzie jako zawartość *widoków* nie jest do żądania z kodem Widok zostały wprowadzone.

Po zdefiniowaniu hierarchię folderów, należy sprawdzić MVC, że każdy kontroler jest skojarzony z obszarem. Można to zrobić, nazwy kontrolera z dekoracji `[Area]` atrybutu.

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

Skonfiguruj definicję trasy, która współdziała z nowo utworzonego obszarach. [Trasy do akcji kontrolera](routing.md) artykułu przechodzi do szczegółów dotyczących sposobu tworzenia definicji trasy, w tym o korzystaniu z konwencjonalnej tras i tras atrybutów. W tym przykładzie użyjemy konwencjonalnej trasy. Aby to zrobić, otwórz *Startup.cs* pliku, a następnie zmodyfikować go przez dodanie `areaRoute` o nazwie poniżej definicji trasy.

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

Przeglądanie do `http://<yourApp>/products`, `Index` metody akcji `HomeController` w `Products` obszaru zostanie wywołany.

## <a name="link-generation"></a>Generowanie konsolidacji

* Generowania łączy z akcji wewnątrz obszaru na podstawie kontrolera innego działania w ramach tego samego kontrolera.

  Załóżmy, że ścieżka bieżącego żądania przypomina `/Products/Home/Create`

  Składnia HtmlHelper: `@Html.ActionLink("Go to Product's Home Page", "Index")`

  Składnia pomocnika tagów: `<a asp-action="Index">Go to Product's Home Page</a>`

  Należy pamiętać, że, firma Microsoft nie musi dostarczać wartości "obszar" i "controller" poniżej, ponieważ są one już dostępne w kontekście bieżącego żądania. Tymi rodzaj wartości są nazywane `ambient` wartości.

* Generowania łączy z akcji wewnątrz obszaru na podstawie kontrolera do innej akcji na innym kontrolerze

  Załóżmy, że ścieżka bieżącego żądania przypomina `/Products/Home/Create`

  Składnia HtmlHelper: `@Html.ActionLink("Go to Manage Products Home Page", "Index", "Manage")`

  Składnia pomocnika tagów: `<a asp-controller="Manage" asp-action="Index">Go to Manage Products Home Page</a>`

  Pamiętaj, że w tym miejscu zostanie użyta wartość otoczenia obszaru, ale jawnie określona wartość "Administrator" powyżej.

* Generowania łączy z akcji w obrębie kontrolera do innej akcji na podstawie innego kontrolera i inny obszar.

  Załóżmy, że ścieżka bieżącego żądania przypomina `/Products/Home/Create`

  Składnia HtmlHelper: `@Html.ActionLink("Go to Services Home Page", "Index", "Home", new { area = "Services" })`

  Składnia pomocnika tagów: `<a asp-area="Services" asp-controller="Home" asp-action="Index">Go to Services Home Page</a>`

  Należy pamiętać, że w tym miejscu są używane nie wartości otoczenia.

* Generowanie łącza akcji w obrębie kontrolera obszaru na podstawie innej akcji na innym kontrolerze i **nie** w obszarze.

  Składnia HtmlHelper: `@Html.ActionLink("Go to Manage Products  Home Page", "Index", "Home", new { area = "" })`

  Składnia pomocnika tagów: `<a asp-area="" asp-controller="Manage" asp-action="Index">Go to Manage Products Home Page</a>`

  Ponieważ chcemy, aby wygenerować łączy do innych niż obszaru na podstawie akcji kontrolera, możemy pusty otoczenia wartość "obszar" w tym miejscu.

## <a name="publishing-areas"></a>Obszary publikowania

Wszystkie `*.cshtml` i `wwwroot/**` pliki są publikowane dane wyjściowe kiedy `<Project Sdk="Microsoft.NET.Sdk.Web">` znajduje się w *.csproj* pliku.
