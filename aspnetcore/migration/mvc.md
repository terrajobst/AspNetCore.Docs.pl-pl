---
title: Migrowanie z programu ASP.NET MVC do podstawowej platformy ASP.NET MVC
author: ardalis
description: "Dowiedz się, jak rozpocząć Migrowanie projektu programu ASP.NET MVC do platformy ASP.NET Core MVC."
manager: wpickett
ms.author: riande
ms.date: 03/07/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: migration/mvc
ms.openlocfilehash: 447b13eccf523cab81590405740bb194112b0dad
ms.sourcegitcommit: 18d1dc86770f2e272d93c7e1cddfc095c5995d9e
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/30/2018
---
# <a name="migrating-from-aspnet-mvc-to-aspnet-core-mvc"></a><span data-ttu-id="588fe-103">Migrowanie z programu ASP.NET MVC do podstawowej platformy ASP.NET MVC</span><span class="sxs-lookup"><span data-stu-id="588fe-103">Migrating from ASP.NET MVC to ASP.NET Core MVC</span></span>

<span data-ttu-id="588fe-104">Przez [Rick Anderson](https://twitter.com/RickAndMSFT), [Roth Danielowi](https://github.com/danroth27), [Steve Smith](https://ardalis.com/), i [Scott Addie](https://scottaddie.com)</span><span class="sxs-lookup"><span data-stu-id="588fe-104">By [Rick Anderson](https://twitter.com/RickAndMSFT), [Daniel Roth](https://github.com/danroth27), [Steve Smith](https://ardalis.com/), and [Scott Addie](https://scottaddie.com)</span></span>

<span data-ttu-id="588fe-105">W tym artykule pokazano, jak rozpocząć Migrowanie projektu programu ASP.NET MVC do [platformy ASP.NET Core MVC](../mvc/overview.md).</span><span class="sxs-lookup"><span data-stu-id="588fe-105">This article shows how to get started migrating an ASP.NET MVC project to [ASP.NET Core MVC](../mvc/overview.md).</span></span> <span data-ttu-id="588fe-106">W procesie zawiera opis wiele czynności, które zostały zmienione z platformy ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="588fe-106">In the process, it highlights many of the things that have changed from ASP.NET MVC.</span></span> <span data-ttu-id="588fe-107">Migrowanie z programu ASP.NET MVC jest wiele procesów kroku i w tym artykule omówiono początkowej konfiguracji, podstawowe kontrolery i widoki, zawartość statyczna i zależności po stronie klienta.</span><span class="sxs-lookup"><span data-stu-id="588fe-107">Migrating from ASP.NET MVC is a multiple step process and this article covers the initial setup, basic controllers and views, static content, and client-side dependencies.</span></span> <span data-ttu-id="588fe-108">Dodatkowe artykuły obejmuje Migrowanie konfiguracji i kod tożsamość w wielu projektów platformy ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="588fe-108">Additional articles cover migrating configuration and identity code found in many ASP.NET MVC projects.</span></span>

> [!NOTE]
> <span data-ttu-id="588fe-109">Numery wersji w przykładach, mogą być nieaktualne.</span><span class="sxs-lookup"><span data-stu-id="588fe-109">The version numbers in the samples might not be current.</span></span> <span data-ttu-id="588fe-110">Konieczne może być odpowiednio zaktualizować swoje projekty.</span><span class="sxs-lookup"><span data-stu-id="588fe-110">You may need to update your projects accordingly.</span></span>

## <a name="create-the-starter-aspnet-mvc-project"></a><span data-ttu-id="588fe-111">Utwórz początkowy projektu programu ASP.NET MVC</span><span class="sxs-lookup"><span data-stu-id="588fe-111">Create the starter ASP.NET MVC project</span></span>

<span data-ttu-id="588fe-112">Aby zademonstrować uaktualnienia, Zaczniemy przez tworzenie aplikacji ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="588fe-112">To demonstrate the upgrade, we'll start by creating a ASP.NET MVC app.</span></span> <span data-ttu-id="588fe-113">Utwórz go o nazwie *WebApp1* , przestrzeń nazw będzie odpowiadała projektu platformy ASP.NET Core, utworzymy w następnym kroku.</span><span class="sxs-lookup"><span data-stu-id="588fe-113">Create it with the name *WebApp1* so the namespace will match the ASP.NET Core project we create in the next step.</span></span>

![Visual Studio okno dialogowe Nowy projekt](mvc/_static/new-project.png)

![Okno dialogowe nowego aplikacji sieci Web: szablonu projektu MVC wybranego w panelu szablony ASP.NET](mvc/_static/new-project-select-mvc-template.png)

<span data-ttu-id="588fe-116">*Opcjonalnie:* zmienić nazwy rozwiązania z *WebApp1* do *Mvc5*.</span><span class="sxs-lookup"><span data-stu-id="588fe-116">*Optional:* Change the name of the Solution from *WebApp1* to *Mvc5*.</span></span> <span data-ttu-id="588fe-117">Visual Studio spowoduje wyświetlenie Nowa nazwa rozwiązania (*Mvc5*), który ułatwi mówić tego projektu z projektu dalej.</span><span class="sxs-lookup"><span data-stu-id="588fe-117">Visual Studio will display the new solution name (*Mvc5*), which will make it easier to tell this project from the next project.</span></span>

## <a name="create-the-aspnet-core-project"></a><span data-ttu-id="588fe-118">Tworzenie projektu platformy ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="588fe-118">Create the ASP.NET Core project</span></span>

<span data-ttu-id="588fe-119">Utwórz nową *pusty* aplikacji sieci web platformy ASP.NET Core z taką samą nazwę jak poprzednie projektu (*WebApp1*), odpowiada przestrzeni nazw w dwóch projektów.</span><span class="sxs-lookup"><span data-stu-id="588fe-119">Create a new *empty* ASP.NET Core web app with the same name as the previous project (*WebApp1*) so the namespaces in the two projects match.</span></span> <span data-ttu-id="588fe-120">O tej samej przestrzeni nazw ułatwia skopiować kod między dwa projekty.</span><span class="sxs-lookup"><span data-stu-id="588fe-120">Having the same namespace makes it easier to copy code between the two projects.</span></span> <span data-ttu-id="588fe-121">Musisz utworzyć ten projekt w katalogu innego niż poprzednie projektu do tej samej nazwie.</span><span class="sxs-lookup"><span data-stu-id="588fe-121">You'll have to create this project in a different directory than the previous project to use the same name.</span></span>

![Okno dialogowe nowego projektu](mvc/_static/new_core.png)

![Okno dialogowe nowego aplikacji sieci Web platformy ASP.NET: pusty szablon projektu wybrany w panelu platformy ASP.NET Core szablonów](mvc/_static/new-project-select-empty-aspnet5-template.png)

* <span data-ttu-id="588fe-124">*Opcjonalnie:* Tworzenie nowej aplikacji platformy ASP.NET Core, za pomocą *aplikacji sieci Web* szablonu projektu.</span><span class="sxs-lookup"><span data-stu-id="588fe-124">*Optional:* Create a new ASP.NET Core app using the *Web Application* project template.</span></span> <span data-ttu-id="588fe-125">Nazwij projekt *WebApp1*i wybierz opcję uwierzytelniania programu **indywidualnych kont użytkowników**.</span><span class="sxs-lookup"><span data-stu-id="588fe-125">Name the project *WebApp1*, and select an authentication option of **Individual User Accounts**.</span></span> <span data-ttu-id="588fe-126">Zmień nazwę tej aplikacji były *FullAspNetCore*.</span><span class="sxs-lookup"><span data-stu-id="588fe-126">Rename this app to *FullAspNetCore*.</span></span> <span data-ttu-id="588fe-127">Tworzenie projektu będzie zaoszczędzić czas podczas konwersji.</span><span class="sxs-lookup"><span data-stu-id="588fe-127">Creating this project will save you time in the conversion.</span></span> <span data-ttu-id="588fe-128">Można przyjrzeć się szablon wygenerowany kod, aby zobaczyć wynik końcowy lub skopiuj kod do konwersji projektu.</span><span class="sxs-lookup"><span data-stu-id="588fe-128">You can look at the template-generated code to see the end result or to copy code to the conversion project.</span></span> <span data-ttu-id="588fe-129">Jest również przydatne, gdy zostać zablokowane w kroku konwersji do porównania z wygenerowane z szablonu projektu.</span><span class="sxs-lookup"><span data-stu-id="588fe-129">It's also helpful when you get stuck on a conversion step to compare with the template-generated project.</span></span>

## <a name="configure-the-site-to-use-mvc"></a><span data-ttu-id="588fe-130">Konfigurowanie lokacji do używania MVC</span><span class="sxs-lookup"><span data-stu-id="588fe-130">Configure the site to use MVC</span></span>

* <span data-ttu-id="588fe-131">Zainstaluj `Microsoft.AspNetCore.Mvc` i `Microsoft.AspNetCore.StaticFiles` pakietów NuGet.</span><span class="sxs-lookup"><span data-stu-id="588fe-131">Install the `Microsoft.AspNetCore.Mvc` and `Microsoft.AspNetCore.StaticFiles` NuGet packages.</span></span>

  <span data-ttu-id="588fe-132">`Microsoft.AspNetCore.Mvc`to platforma ASP.NET Core MVC.</span><span class="sxs-lookup"><span data-stu-id="588fe-132">`Microsoft.AspNetCore.Mvc` is the ASP.NET Core MVC framework.</span></span> <span data-ttu-id="588fe-133">`Microsoft.AspNetCore.StaticFiles`Umożliwia to obsługę plików statycznych.</span><span class="sxs-lookup"><span data-stu-id="588fe-133">`Microsoft.AspNetCore.StaticFiles` is the static file handler.</span></span> <span data-ttu-id="588fe-134">Środowiska uruchomieniowego ASP.NET jest moduły i musi jawnie zgłosić się do obsługi plików statycznych (zobacz [Praca z pliki statyczne](../fundamentals/static-files.md)).</span><span class="sxs-lookup"><span data-stu-id="588fe-134">The ASP.NET runtime is modular, and you must explicitly opt in to serve static files (see [Working with Static Files](../fundamentals/static-files.md)).</span></span>

* <span data-ttu-id="588fe-135">Otwórz *.csproj* pliku (kliknij prawym przyciskiem myszy projekt w **Eksploratora rozwiązań** i wybierz **Edytuj WebApp1.csproj**) i Dodaj `PrepareForPublish` docelowych:</span><span class="sxs-lookup"><span data-stu-id="588fe-135">Open the *.csproj* file (right-click the project in **Solution Explorer** and select **Edit WebApp1.csproj**) and add a `PrepareForPublish` target:</span></span>

  [!code-xml[Main](mvc/sample/WebApp1.csproj?range=21-23)]

  <span data-ttu-id="588fe-136">`PrepareForPublish` Docelowy jest wymagany dla pobierania biblioteki po stronie klienta za pomocą rozwiązania Bower.</span><span class="sxs-lookup"><span data-stu-id="588fe-136">The `PrepareForPublish` target is needed for acquiring client-side libraries via Bower.</span></span> <span data-ttu-id="588fe-137">Będzie omawianiu który później.</span><span class="sxs-lookup"><span data-stu-id="588fe-137">We'll talk about that later.</span></span>

* <span data-ttu-id="588fe-138">Otwórz *Startup.cs* plików i zmień kod zgodnie z poniższym:</span><span class="sxs-lookup"><span data-stu-id="588fe-138">Open the *Startup.cs* file and change the code to match the following:</span></span>

  [!code-csharp[Main](mvc/sample/Startup.cs?highlight=14,27-34)]

  <span data-ttu-id="588fe-139">`UseStaticFiles` — Metoda rozszerzenia dodaje obsługę plików statycznych.</span><span class="sxs-lookup"><span data-stu-id="588fe-139">The `UseStaticFiles` extension method adds the static file handler.</span></span> <span data-ttu-id="588fe-140">Jak wspomniano wcześniej, środowiska uruchomieniowego ASP.NET jest moduły i musi jawnie zgłosić się do obsługi plików statycznych.</span><span class="sxs-lookup"><span data-stu-id="588fe-140">As mentioned previously, the ASP.NET runtime is modular, and you must explicitly opt in to serve static files.</span></span> <span data-ttu-id="588fe-141">`UseMvc` — Metoda rozszerzenia dodaje routingu.</span><span class="sxs-lookup"><span data-stu-id="588fe-141">The `UseMvc` extension method adds routing.</span></span> <span data-ttu-id="588fe-142">Aby uzyskać więcej informacji, zobacz [uruchamiania aplikacji](../fundamentals/startup.md) i [Routing](../fundamentals/routing.md).</span><span class="sxs-lookup"><span data-stu-id="588fe-142">For more information, see [Application Startup](../fundamentals/startup.md) and [Routing](../fundamentals/routing.md).</span></span>

## <a name="add-a-controller-and-view"></a><span data-ttu-id="588fe-143">Dodawanie kontrolera i widoku</span><span class="sxs-lookup"><span data-stu-id="588fe-143">Add a controller and view</span></span>

<span data-ttu-id="588fe-144">W tej sekcji możesz dodać minimalny kontrolera i widoku jako symbole zastępcze dla kontrolera ASP.NET MVC i widoki, będzie migracji w następnej sekcji.</span><span class="sxs-lookup"><span data-stu-id="588fe-144">In this section, you'll add a minimal controller and view to serve as placeholders for the ASP.NET MVC controller and views you'll migrate in the next section.</span></span>

* <span data-ttu-id="588fe-145">Dodaj *kontrolerów* folderu.</span><span class="sxs-lookup"><span data-stu-id="588fe-145">Add a *Controllers* folder.</span></span>

* <span data-ttu-id="588fe-146">Dodaj **klasy kontrolera MVC** o nazwie *HomeController.cs* do *kontrolerów* folderu.</span><span class="sxs-lookup"><span data-stu-id="588fe-146">Add an **MVC controller class** with the name *HomeController.cs* to the *Controllers* folder.</span></span>

![Dodaj nowy element okna dialogowego](mvc/_static/add_mvc_ctl.png)

* <span data-ttu-id="588fe-148">Dodaj *widoków* folderu.</span><span class="sxs-lookup"><span data-stu-id="588fe-148">Add a *Views* folder.</span></span>

* <span data-ttu-id="588fe-149">Dodaj *widoków domowych* folderu.</span><span class="sxs-lookup"><span data-stu-id="588fe-149">Add a *Views/Home* folder.</span></span>

* <span data-ttu-id="588fe-150">Dodaj *Index.cshtml* MVC — Wyświetl stronę do *widoków domowych* folderu.</span><span class="sxs-lookup"><span data-stu-id="588fe-150">Add an *Index.cshtml* MVC view page to the *Views/Home* folder.</span></span>

![Dodaj nowy element okna dialogowego](mvc/_static/view.png)

<span data-ttu-id="588fe-152">Poniżej przedstawiono struktury projektu:</span><span class="sxs-lookup"><span data-stu-id="588fe-152">The project structure is shown below:</span></span>

![Eksploratora rozwiązań przedstawiający pliki i foldery WebApp1](mvc/_static/project-structure-controller-view.png)

<span data-ttu-id="588fe-154">Zastąp zawartość *Views/Home/Index.cshtml* pliku następującym kodem:</span><span class="sxs-lookup"><span data-stu-id="588fe-154">Replace the contents of the *Views/Home/Index.cshtml* file with the following:</span></span>

```html
<h1>Hello world!</h1>
```

<span data-ttu-id="588fe-155">Uruchom aplikację.</span><span class="sxs-lookup"><span data-stu-id="588fe-155">Run the app.</span></span>

![Otwórz w programie Microsoft Edge aplikacji sieci Web](mvc/_static/hello-world.png)

<span data-ttu-id="588fe-157">Zobacz [kontrolerów](xref:mvc/controllers/actions) i [widoków](xref:mvc/views/overview) Aby uzyskać więcej informacji.</span><span class="sxs-lookup"><span data-stu-id="588fe-157">See [Controllers](xref:mvc/controllers/actions) and [Views](xref:mvc/views/overview) for more information.</span></span>

<span data-ttu-id="588fe-158">Teraz, gdy mamy minimalnego projektu platformy ASP.NET Core pracy, możemy rozpocząć Migrowanie funkcja z projektu programu ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="588fe-158">Now that we have a minimal working ASP.NET Core project, we can start migrating functionality from the ASP.NET MVC project.</span></span> <span data-ttu-id="588fe-159">Będzie trzeba przenieść następujące czynności:</span><span class="sxs-lookup"><span data-stu-id="588fe-159">We will need to move the following:</span></span>

* <span data-ttu-id="588fe-160">zawartość po stronie klienta (CSS, czcionki i skrypty)</span><span class="sxs-lookup"><span data-stu-id="588fe-160">client-side content (CSS, fonts, and scripts)</span></span>

* <span data-ttu-id="588fe-161">kontrolery</span><span class="sxs-lookup"><span data-stu-id="588fe-161">controllers</span></span>

* <span data-ttu-id="588fe-162">widoki</span><span class="sxs-lookup"><span data-stu-id="588fe-162">views</span></span>

* <span data-ttu-id="588fe-163">modele</span><span class="sxs-lookup"><span data-stu-id="588fe-163">models</span></span>

* <span data-ttu-id="588fe-164">Tworzenie pakietów</span><span class="sxs-lookup"><span data-stu-id="588fe-164">bundling</span></span>

* <span data-ttu-id="588fe-165">filtry</span><span class="sxs-lookup"><span data-stu-id="588fe-165">filters</span></span>

* <span data-ttu-id="588fe-166">Zaloguj się we/wy tożsamości (będzie to w następnym samouczku.)</span><span class="sxs-lookup"><span data-stu-id="588fe-166">Log in/out, identity (This will be done in the next tutorial.)</span></span>

## <a name="controllers-and-views"></a><span data-ttu-id="588fe-167">Kontrolery i widoki</span><span class="sxs-lookup"><span data-stu-id="588fe-167">Controllers and views</span></span>

* <span data-ttu-id="588fe-168">Skopiuj każdej z metod z platformy ASP.NET MVC `HomeController` do nowego `HomeController`.</span><span class="sxs-lookup"><span data-stu-id="588fe-168">Copy each of the methods from the ASP.NET MVC `HomeController` to the new `HomeController`.</span></span> <span data-ttu-id="588fe-169">Należy pamiętać, że w programie ASP.NET MVC, typ zwracany metody akcji kontrolera wbudowanych szablonów [ActionResult](https://msdn.microsoft.com/library/system.web.mvc.actionresult(v=vs.118).aspx); na platformie ASP.NET MVC Core, zwracany metody akcji `IActionResult` zamiast tego.</span><span class="sxs-lookup"><span data-stu-id="588fe-169">Note that in ASP.NET MVC, the built-in template's controller action method return type is [ActionResult](https://msdn.microsoft.com/library/system.web.mvc.actionresult(v=vs.118).aspx); in ASP.NET Core MVC, the action methods return `IActionResult` instead.</span></span> <span data-ttu-id="588fe-170">`ActionResult`implementuje `IActionResult`, więc nie trzeba zmienić zwracany typ metody akcji.</span><span class="sxs-lookup"><span data-stu-id="588fe-170">`ActionResult` implements `IActionResult`, so there's no need to change the return type of your action methods.</span></span>

* <span data-ttu-id="588fe-171">Kopiuj *About.cshtml*, *Contact.cshtml*, i *Index.cshtml* pliki widoku Razor z projektu programu ASP.NET MVC do projektu platformy ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="588fe-171">Copy the *About.cshtml*, *Contact.cshtml*, and *Index.cshtml* Razor view files from the ASP.NET MVC project to the ASP.NET Core project.</span></span>

* <span data-ttu-id="588fe-172">Uruchamianie aplikacji platformy ASP.NET Core i testowanie każdej metody.</span><span class="sxs-lookup"><span data-stu-id="588fe-172">Run the ASP.NET Core app and test each method.</span></span> <span data-ttu-id="588fe-173">Firma Microsoft nie migracji pliku układu i stylów jeszcze, więc renderowanych widoków będzie zawierać tylko zawartości w plikach widoku.</span><span class="sxs-lookup"><span data-stu-id="588fe-173">We haven't migrated the layout file or styles yet, so the rendered views will only contain the content in the view files.</span></span> <span data-ttu-id="588fe-174">Nie będziesz mieć łącza plik wygenerowany układu `About` i `Contact` widoków, więc musisz wywołać je za pomocą przeglądarki (Zastąp **4492** numer portu używany w projekcie).</span><span class="sxs-lookup"><span data-stu-id="588fe-174">You won't have the layout file generated links for the `About` and `Contact` views, so you'll have to invoke them from the browser (replace **4492** with the port number used in your project).</span></span>

  * `http://localhost:4492/home/about`

  * `http://localhost:4492/home/contact`

![Skontaktuj się z pomocą strony](mvc/_static/contact-page.png)

<span data-ttu-id="588fe-176">Zwróć uwagę na Brak elementów stylami i menu.</span><span class="sxs-lookup"><span data-stu-id="588fe-176">Note the lack of styling and menu items.</span></span> <span data-ttu-id="588fe-177">Firma Microsoft będzie rozwiązać ten problem w następnej sekcji.</span><span class="sxs-lookup"><span data-stu-id="588fe-177">We'll fix that in the next section.</span></span>

## <a name="static-content"></a><span data-ttu-id="588fe-178">Zawartość statyczna</span><span class="sxs-lookup"><span data-stu-id="588fe-178">Static content</span></span>

<span data-ttu-id="588fe-179">W poprzednich wersjach programu ASP.NET MVC zawartość statyczną hostowanej z katalogu głównego projektu sieci web i został zmieszać z plikami po stronie serwera.</span><span class="sxs-lookup"><span data-stu-id="588fe-179">In previous versions of  ASP.NET MVC, static content was hosted from the root of the web project and was intermixed with server-side files.</span></span> <span data-ttu-id="588fe-180">W przypadku platformy ASP.NET Core zawartość statyczną znajduje się w *wwwroot* folderu.</span><span class="sxs-lookup"><span data-stu-id="588fe-180">In ASP.NET Core, static content is hosted in the *wwwroot* folder.</span></span> <span data-ttu-id="588fe-181">Należy skopiować zawartość statyczną z aplikacji ASP.NET MVC starego *wwwroot* folder w projekcie platformy ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="588fe-181">You'll want to copy the static content from your old ASP.NET MVC app to the *wwwroot* folder in your ASP.NET Core project.</span></span> <span data-ttu-id="588fe-182">W tej konwersji próbkowania:</span><span class="sxs-lookup"><span data-stu-id="588fe-182">In this sample conversion:</span></span>

* <span data-ttu-id="588fe-183">Kopiuj *favicon.ico* pliku starego projektu MVC *wwwroot* folderu w projekcie platformy ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="588fe-183">Copy the *favicon.ico* file from the old MVC project to the *wwwroot* folder in the ASP.NET Core project.</span></span>

<span data-ttu-id="588fe-184">ASP.NET MVC stary projekt używa [Bootstrap](http://getbootstrap.com/) stylów i magazyny plików ładowania początkowego programu *zawartości* i *skryptów* folderów.</span><span class="sxs-lookup"><span data-stu-id="588fe-184">The old ASP.NET MVC project uses [Bootstrap](http://getbootstrap.com/) for its styling and stores the Bootstrap files in the *Content* and *Scripts* folders.</span></span> <span data-ttu-id="588fe-185">Szablon, który wygenerować stary projekt platformy ASP.NET MVC, odwołuje się do ładowania początkowego w pliku układu (*Views/Shared/_Layout.cshtml*).</span><span class="sxs-lookup"><span data-stu-id="588fe-185">The template, which generated the old ASP.NET MVC project, references Bootstrap in the layout file (*Views/Shared/_Layout.cshtml*).</span></span> <span data-ttu-id="588fe-186">Można skopiować *bootstrap.js* i *bootstrap.css* pliki z platformy ASP.NET MVC projektu do *wwwroot* nie korzysta z folderu w nowym projekcie, ale tego podejścia Ulepszone mechanizm zarządzania zależności po stronie klienta w ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="588fe-186">You could copy the *bootstrap.js* and *bootstrap.css* files from the ASP.NET MVC project to the *wwwroot* folder in the new project, but that approach doesn't use the improved mechanism for managing client-side dependencies in ASP.NET Core.</span></span>

<span data-ttu-id="588fe-187">W nowym projekcie dodamy obsługę ładowania początkowego (i innych bibliotek po stronie klienta), za pomocą [Bower](https://bower.io/):</span><span class="sxs-lookup"><span data-stu-id="588fe-187">In the new project, we'll add support for Bootstrap (and other client-side libraries) using [Bower](https://bower.io/):</span></span>

* <span data-ttu-id="588fe-188">Dodaj [Bower](https://bower.io/) pliku konfiguracji o nazwie *bower.json* do katalogu głównego projektu (kliknij prawym przyciskiem myszy na projekt, a następnie **Dodaj > Nowy element > pliku konfiguracji Bower**).</span><span class="sxs-lookup"><span data-stu-id="588fe-188">Add a [Bower](https://bower.io/) configuration file named *bower.json* to the project root (Right-click on the project, and then **Add > New Item > Bower Configuration File**).</span></span> <span data-ttu-id="588fe-189">Dodaj [Bootstrap](http://getbootstrap.com/) i [jQuery](https://jquery.com/) do pliku (zobacz wyróżnione wiersze poniżej).</span><span class="sxs-lookup"><span data-stu-id="588fe-189">Add [Bootstrap](http://getbootstrap.com/) and [jQuery](https://jquery.com/) to the file (see the highlighted lines below).</span></span>

  [!code-json[Main](mvc/sample/bower.json?highlight=5-6)]

<span data-ttu-id="588fe-190">Podczas zapisywania pliku, Bower będzie automatycznie pobierał zależności, aby *wwwroot/lib* folderu.</span><span class="sxs-lookup"><span data-stu-id="588fe-190">Upon saving the file, Bower will automatically download the dependencies to the *wwwroot/lib* folder.</span></span> <span data-ttu-id="588fe-191">Można użyć **Eksploratora rozwiązań wyszukiwania** pole można znaleźć ścieżki zasoby:</span><span class="sxs-lookup"><span data-stu-id="588fe-191">You can use the **Search Solution Explorer** box to find the path of the assets:</span></span>

![zasoby jquery wyświetlane w wynikach wyszukiwania w Eksploratorze rozwiązań](mvc/_static/search.png)

<span data-ttu-id="588fe-193">Zobacz [Zarządzaj pakietami po stronie klienta z Bower](../client-side/bower.md) Aby uzyskać więcej informacji.</span><span class="sxs-lookup"><span data-stu-id="588fe-193">See [Manage Client-Side Packages with Bower](../client-side/bower.md) for more information.</span></span>

<a name="migrate-layout-file"></a>

## <a name="migrate-the-layout-file"></a><span data-ttu-id="588fe-194">Migracja pliku układu</span><span class="sxs-lookup"><span data-stu-id="588fe-194">Migrate the layout file</span></span>

* <span data-ttu-id="588fe-195">Kopiuj *_ViewStart.cshtml* pliku starego projektu programu ASP.NET MVC *widoków* folderu do projektu platformy ASP.NET Core *widoków* folderu.</span><span class="sxs-lookup"><span data-stu-id="588fe-195">Copy the *_ViewStart.cshtml* file from the old ASP.NET MVC project's *Views* folder into the ASP.NET Core project's *Views* folder.</span></span> <span data-ttu-id="588fe-196">*_ViewStart.cshtml* plik nie został zmieniony na platformie ASP.NET Core MVC.</span><span class="sxs-lookup"><span data-stu-id="588fe-196">The *_ViewStart.cshtml* file has not changed in ASP.NET Core MVC.</span></span>

* <span data-ttu-id="588fe-197">Utwórz *widoków/Shared* folderu.</span><span class="sxs-lookup"><span data-stu-id="588fe-197">Create a *Views/Shared* folder.</span></span>

* <span data-ttu-id="588fe-198">*Opcjonalnie:* kopiowania *_ViewImports.cshtml* z *FullAspNetCore* projektu MVC *widoków* folderu do projektu platformy ASP.NET Core *Widoków* folderu.</span><span class="sxs-lookup"><span data-stu-id="588fe-198">*Optional:* Copy *_ViewImports.cshtml* from the *FullAspNetCore* MVC project's *Views* folder into the ASP.NET Core project's *Views* folder.</span></span> <span data-ttu-id="588fe-199">Usuń wszelkie deklaracji przestrzeni nazw w *_ViewImports.cshtml* pliku.</span><span class="sxs-lookup"><span data-stu-id="588fe-199">Remove any namespace declaration in the *_ViewImports.cshtml* file.</span></span> <span data-ttu-id="588fe-200">*_ViewImports.cshtml* plików zawiera przestrzenie nazw dla wszystkich plików widoku i w przypadku [pomocników tagów](xref:mvc/views/tag-helpers/intro).</span><span class="sxs-lookup"><span data-stu-id="588fe-200">The *_ViewImports.cshtml* file provides namespaces for all the view files and brings in [Tag Helpers](xref:mvc/views/tag-helpers/intro).</span></span> <span data-ttu-id="588fe-201">Pomocników tagów są używane w nowym pliku układu.</span><span class="sxs-lookup"><span data-stu-id="588fe-201">Tag Helpers are used in the new layout file.</span></span> <span data-ttu-id="588fe-202">*_ViewImports.cshtml* pliku jest nowa dla platformy ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="588fe-202">The *_ViewImports.cshtml* file is new for ASP.NET Core.</span></span>

* <span data-ttu-id="588fe-203">Kopia *_Layout.cshtml* pliku starego projektu programu ASP.NET MVC *widoków/Shared* folderu do projektu platformy ASP.NET Core *widoków/Shared* folderu.</span><span class="sxs-lookup"><span data-stu-id="588fe-203">Copy the *_Layout.cshtml* file from the old ASP.NET MVC project's *Views/Shared* folder into the ASP.NET Core project's *Views/Shared* folder.</span></span>

<span data-ttu-id="588fe-204">Otwórz *_Layout.cshtml* plik, a następnie dokonaj następujących zmian (kompletny kod przedstawiony poniżej):</span><span class="sxs-lookup"><span data-stu-id="588fe-204">Open *_Layout.cshtml* file and make the following changes (the completed code is shown below):</span></span>

   * <span data-ttu-id="588fe-205">Zastąp `@Styles.Render("~/Content/css")` z `<link>` elementu, aby załadować *bootstrap.css* (patrz poniżej).</span><span class="sxs-lookup"><span data-stu-id="588fe-205">Replace `@Styles.Render("~/Content/css")` with a `<link>` element to load *bootstrap.css* (see below).</span></span>

   * <span data-ttu-id="588fe-206">Usuń `@Scripts.Render("~/bundles/modernizr")`.</span><span class="sxs-lookup"><span data-stu-id="588fe-206">Remove `@Scripts.Render("~/bundles/modernizr")`.</span></span>

   * <span data-ttu-id="588fe-207">Komentarz `@Html.Partial("_LoginPartial")` wiersza (przestrzenny wiersz z `@*...*@`).</span><span class="sxs-lookup"><span data-stu-id="588fe-207">Comment out the `@Html.Partial("_LoginPartial")` line (surround the line with `@*...*@`).</span></span> <span data-ttu-id="588fe-208">Wrócimy do niego w przyszłości samouczka.</span><span class="sxs-lookup"><span data-stu-id="588fe-208">We'll return to it in a future tutorial.</span></span>

   * <span data-ttu-id="588fe-209">Zastąp `@Scripts.Render("~/bundles/jquery")` z `<script>` elementu (patrz poniżej).</span><span class="sxs-lookup"><span data-stu-id="588fe-209">Replace `@Scripts.Render("~/bundles/jquery")` with a `<script>` element (see below).</span></span>

   * <span data-ttu-id="588fe-210">Zastąp `@Scripts.Render("~/bundles/bootstrap")` z `<script>` elementu (patrz poniżej).</span><span class="sxs-lookup"><span data-stu-id="588fe-210">Replace `@Scripts.Render("~/bundles/bootstrap")` with a `<script>` element (see below)..</span></span>

<span data-ttu-id="588fe-211">Zastąpienie łączy CSS:</span><span class="sxs-lookup"><span data-stu-id="588fe-211">The replacement CSS link:</span></span>

```html
<link rel="stylesheet" href="~/lib/bootstrap/dist/css/bootstrap.css" />
```

<span data-ttu-id="588fe-212">Zastąpienie tagów skryptu:</span><span class="sxs-lookup"><span data-stu-id="588fe-212">The replacement script tags:</span></span>

```html
<script src="~/lib/jquery/dist/jquery.js"></script>
<script src="~/lib/bootstrap/dist/js/bootstrap.js"></script>
```

<span data-ttu-id="588fe-213">Zaktualizowany interfejs *_Layout.cshtml* pliku przedstawiono poniżej:</span><span class="sxs-lookup"><span data-stu-id="588fe-213">The updated *_Layout.cshtml* file is shown below:</span></span>

[!code-html[Main](mvc/sample/Views/Shared/_Layout.cshtml?highlight=7,27,39-40)]

<span data-ttu-id="588fe-214">Wyświetlać witrynę w przeglądarce.</span><span class="sxs-lookup"><span data-stu-id="588fe-214">View the site in the browser.</span></span> <span data-ttu-id="588fe-215">Teraz powinien on załadowany poprawnie, z oczekiwanym style w miejscu.</span><span class="sxs-lookup"><span data-stu-id="588fe-215">It should now load correctly, with the expected styles in place.</span></span>

* <span data-ttu-id="588fe-216">*Opcjonalnie:* możesz chcieć użyć nowego pliku układu.</span><span class="sxs-lookup"><span data-stu-id="588fe-216">*Optional:* You might want to try using the new layout file.</span></span> <span data-ttu-id="588fe-217">Dla tego projektu można skopiować pliku układu z *FullAspNetCore* projektu.</span><span class="sxs-lookup"><span data-stu-id="588fe-217">For this project you can copy the layout file from the *FullAspNetCore* project.</span></span> <span data-ttu-id="588fe-218">Używa nowego pliku układu [pomocników tagów](xref:mvc/views/tag-helpers/intro) i ma inne ulepszenia.</span><span class="sxs-lookup"><span data-stu-id="588fe-218">The new layout file uses [Tag Helpers](xref:mvc/views/tag-helpers/intro) and has other improvements.</span></span>

## <a name="configure-bundling-and-minification"></a><span data-ttu-id="588fe-219">Konfigurowanie, tworzenie pakietów i minimalizowanie</span><span class="sxs-lookup"><span data-stu-id="588fe-219">Configure bundling and minification</span></span>

<span data-ttu-id="588fe-220">Aby uzyskać informacje o sposobie konfigurowania tworzenie pakietów i minimalizowanie, zobacz [tworzenie pakietów i minimalizowanie](../client-side/bundling-and-minification.md).</span><span class="sxs-lookup"><span data-stu-id="588fe-220">For information about how to configure bundling and minification, see [Bundling and Minification](../client-side/bundling-and-minification.md).</span></span>

## <a name="solving-http-500-errors"></a><span data-ttu-id="588fe-221">Rozwiązywanie błędów HTTP 500</span><span class="sxs-lookup"><span data-stu-id="588fe-221">Solving HTTP 500 errors</span></span>

<span data-ttu-id="588fe-222">Istnieje wiele problemów, które mogą spowodować, że komunikat o błędzie HTTP 500, które nie zawierają żadnych informacji o źródło problemu.</span><span class="sxs-lookup"><span data-stu-id="588fe-222">There are many problems that can cause a HTTP 500 error message that contain no information on the source of the problem.</span></span> <span data-ttu-id="588fe-223">Na przykład jeśli *Views/_ViewImports.cshtml* plik zawiera przestrzeni nazw, która nie istnieje w projekcie, zostanie wyświetlony komunikat o błędzie HTTP 500.</span><span class="sxs-lookup"><span data-stu-id="588fe-223">For example, if the *Views/_ViewImports.cshtml* file contains a namespace that doesn't exist in your project, you'll get a HTTP 500 error.</span></span> <span data-ttu-id="588fe-224">Aby uzyskać szczegółowy komunikat o błędzie, Dodaj następujący kod:</span><span class="sxs-lookup"><span data-stu-id="588fe-224">To get a detailed error message, add the following code:</span></span>

```csharp
public void Configure(IApplicationBuilder app, IHostingEnvironment env, ILoggerFactory loggerFactory)
{
    if (env.IsDevelopment())
    {
         app.UseDeveloperExceptionPage();
    }

    app.UseStaticFiles();

    app.UseMvc(routes =>
    {
        routes.MapRoute(
            name: "default",
            template: "{controller=Home}/{action=Index}/{id?}");
    });
}
```

<span data-ttu-id="588fe-225">Zobacz **za pomocą strony wyjątek Developer** w [obsługi błędu](../fundamentals/error-handling.md) Aby uzyskać więcej informacji.</span><span class="sxs-lookup"><span data-stu-id="588fe-225">See **Using the Developer Exception Page** in [Error Handling](../fundamentals/error-handling.md) for more information.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="588fe-226">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="588fe-226">Additional resources</span></span>

* [<span data-ttu-id="588fe-227">Programowanie po stronie klienta</span><span class="sxs-lookup"><span data-stu-id="588fe-227">Client-Side Development</span></span>](xref:client-side/index)
* [<span data-ttu-id="588fe-228">Pomocnicy tagów</span><span class="sxs-lookup"><span data-stu-id="588fe-228">Tag Helpers</span></span>](xref:mvc/views/tag-helpers/intro)
