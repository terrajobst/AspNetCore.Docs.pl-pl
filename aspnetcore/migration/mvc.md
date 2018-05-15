---
title: Migracja z programu ASP.NET MVC do podstawowej platformy ASP.NET MVC
author: ardalis
description: Dowiedz się, jak rozpocząć Migrowanie projektu programu ASP.NET MVC do platformy ASP.NET Core MVC.
manager: wpickett
ms.author: riande
ms.date: 03/07/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: migration/mvc
ms.openlocfilehash: b8c913c0a6f47a1c993d508f9baae54981327957
ms.sourcegitcommit: 477d38e33530a305405eaf19faa29c6d805273aa
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 05/08/2018
---
# <a name="migrate-from-aspnet-mvc-to-aspnet-core-mvc"></a><span data-ttu-id="c517f-103">Migracja z programu ASP.NET MVC do podstawowej platformy ASP.NET MVC</span><span class="sxs-lookup"><span data-stu-id="c517f-103">Migrate from ASP.NET MVC to ASP.NET Core MVC</span></span>

<span data-ttu-id="c517f-104">Przez [Rick Anderson](https://twitter.com/RickAndMSFT), [Roth Danielowi](https://github.com/danroth27), [Steve Smith](https://ardalis.com/), i [Scott Addie](https://scottaddie.com)</span><span class="sxs-lookup"><span data-stu-id="c517f-104">By [Rick Anderson](https://twitter.com/RickAndMSFT), [Daniel Roth](https://github.com/danroth27), [Steve Smith](https://ardalis.com/), and [Scott Addie](https://scottaddie.com)</span></span>

<span data-ttu-id="c517f-105">W tym artykule pokazano, jak rozpocząć Migrowanie projektu programu ASP.NET MVC do [platformy ASP.NET Core MVC](../mvc/overview.md).</span><span class="sxs-lookup"><span data-stu-id="c517f-105">This article shows how to get started migrating an ASP.NET MVC project to [ASP.NET Core MVC](../mvc/overview.md).</span></span> <span data-ttu-id="c517f-106">W procesie zawiera opis wiele czynności, które zostały zmienione z platformy ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="c517f-106">In the process, it highlights many of the things that have changed from ASP.NET MVC.</span></span> <span data-ttu-id="c517f-107">Migrowanie z programu ASP.NET MVC jest wiele procesów kroku i w tym artykule omówiono początkowej konfiguracji, podstawowe kontrolery i widoki, zawartość statyczna i zależności po stronie klienta.</span><span class="sxs-lookup"><span data-stu-id="c517f-107">Migrating from ASP.NET MVC is a multiple step process and this article covers the initial setup, basic controllers and views, static content, and client-side dependencies.</span></span> <span data-ttu-id="c517f-108">Dodatkowe artykuły obejmuje Migrowanie konfiguracji i kod tożsamość w wielu projektów platformy ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="c517f-108">Additional articles cover migrating configuration and identity code found in many ASP.NET MVC projects.</span></span>

> [!NOTE]
> <span data-ttu-id="c517f-109">Numery wersji w przykładach, mogą być nieaktualne.</span><span class="sxs-lookup"><span data-stu-id="c517f-109">The version numbers in the samples might not be current.</span></span> <span data-ttu-id="c517f-110">Konieczne może być odpowiednio zaktualizować swoje projekty.</span><span class="sxs-lookup"><span data-stu-id="c517f-110">You may need to update your projects accordingly.</span></span>

## <a name="create-the-starter-aspnet-mvc-project"></a><span data-ttu-id="c517f-111">Utwórz początkowy projektu programu ASP.NET MVC</span><span class="sxs-lookup"><span data-stu-id="c517f-111">Create the starter ASP.NET MVC project</span></span>

<span data-ttu-id="c517f-112">Aby zademonstrować uaktualnienia, Zaczniemy przez tworzenie aplikacji ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="c517f-112">To demonstrate the upgrade, we'll start by creating a ASP.NET MVC app.</span></span> <span data-ttu-id="c517f-113">Utwórz go o nazwie *WebApp1* tak pasujących przestrzeni nazw projektu platformy ASP.NET Core, utworzymy w następnym kroku.</span><span class="sxs-lookup"><span data-stu-id="c517f-113">Create it with the name *WebApp1* so the namespace matches the ASP.NET Core project we create in the next step.</span></span>

![Visual Studio okno dialogowe Nowy projekt](mvc/_static/new-project.png)

![Okno dialogowe nowego aplikacji sieci Web: szablonu projektu MVC wybranego w panelu szablony ASP.NET](mvc/_static/new-project-select-mvc-template.png)

<span data-ttu-id="c517f-116">*Opcjonalnie:* zmienić nazwy rozwiązania z *WebApp1* do *Mvc5*.</span><span class="sxs-lookup"><span data-stu-id="c517f-116">*Optional:* Change the name of the Solution from *WebApp1* to *Mvc5*.</span></span> <span data-ttu-id="c517f-117">Visual Studio wyświetlana nazwa nowego rozwiązania (*Mvc5*), ułatwia mówić tego projektu z projektu dalej.</span><span class="sxs-lookup"><span data-stu-id="c517f-117">Visual Studio displays the new solution name (*Mvc5*), which makes it easier to tell this project from the next project.</span></span>

## <a name="create-the-aspnet-core-project"></a><span data-ttu-id="c517f-118">Tworzenie projektu platformy ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="c517f-118">Create the ASP.NET Core project</span></span>

<span data-ttu-id="c517f-119">Utwórz nową *pusty* aplikacji sieci web platformy ASP.NET Core z taką samą nazwę jak poprzednie projektu (*WebApp1*), odpowiada przestrzeni nazw w dwóch projektów.</span><span class="sxs-lookup"><span data-stu-id="c517f-119">Create a new *empty* ASP.NET Core web app with the same name as the previous project (*WebApp1*) so the namespaces in the two projects match.</span></span> <span data-ttu-id="c517f-120">O tej samej przestrzeni nazw ułatwia skopiować kod między dwa projekty.</span><span class="sxs-lookup"><span data-stu-id="c517f-120">Having the same namespace makes it easier to copy code between the two projects.</span></span> <span data-ttu-id="c517f-121">Musisz utworzyć ten projekt w katalogu innego niż poprzednie projektu do tej samej nazwie.</span><span class="sxs-lookup"><span data-stu-id="c517f-121">You'll have to create this project in a different directory than the previous project to use the same name.</span></span>

![Okno dialogowe nowego projektu](mvc/_static/new_core.png)

![Okno dialogowe nowego aplikacji sieci Web platformy ASP.NET: pusty szablon projektu wybrany w panelu platformy ASP.NET Core szablonów](mvc/_static/new-project-select-empty-aspnet5-template.png)

* <span data-ttu-id="c517f-124">*Opcjonalnie:* Tworzenie nowej aplikacji platformy ASP.NET Core, za pomocą *aplikacji sieci Web* szablonu projektu.</span><span class="sxs-lookup"><span data-stu-id="c517f-124">*Optional:* Create a new ASP.NET Core app using the *Web Application* project template.</span></span> <span data-ttu-id="c517f-125">Nazwij projekt *WebApp1*i wybierz opcję uwierzytelniania programu **indywidualnych kont użytkowników**.</span><span class="sxs-lookup"><span data-stu-id="c517f-125">Name the project *WebApp1*, and select an authentication option of **Individual User Accounts**.</span></span> <span data-ttu-id="c517f-126">Zmień nazwę tej aplikacji były *FullAspNetCore*.</span><span class="sxs-lookup"><span data-stu-id="c517f-126">Rename this app to *FullAspNetCore*.</span></span> <span data-ttu-id="c517f-127">Tworzenie projektu oszczędza czas w konwersji.</span><span class="sxs-lookup"><span data-stu-id="c517f-127">Creating this project saves you time in the conversion.</span></span> <span data-ttu-id="c517f-128">Można przyjrzeć się szablon wygenerowany kod, aby zobaczyć wynik końcowy lub skopiuj kod do konwersji projektu.</span><span class="sxs-lookup"><span data-stu-id="c517f-128">You can look at the template-generated code to see the end result or to copy code to the conversion project.</span></span> <span data-ttu-id="c517f-129">Jest również przydatne, gdy zostać zablokowane w kroku konwersji do porównania z wygenerowane z szablonu projektu.</span><span class="sxs-lookup"><span data-stu-id="c517f-129">It's also helpful when you get stuck on a conversion step to compare with the template-generated project.</span></span>

## <a name="configure-the-site-to-use-mvc"></a><span data-ttu-id="c517f-130">Konfigurowanie lokacji do używania MVC</span><span class="sxs-lookup"><span data-stu-id="c517f-130">Configure the site to use MVC</span></span>

* <span data-ttu-id="c517f-131">Gdy przeznaczonych dla platformy .NET Core, metapackage platformy ASP.NET Core zostanie dodane do projektu o nazwie `Microsoft.AspNetCore.All` domyślnie.</span><span class="sxs-lookup"><span data-stu-id="c517f-131">When targeting .NET Core, the ASP.NET Core metapackage is added to the project, called `Microsoft.AspNetCore.All` by default.</span></span> <span data-ttu-id="c517f-132">Ten pakiet zawiera pakiety, takich jak `Microsoft.AspNetCore.Mvc` i `Microsoft.AspNetCore.StaticFiles`.</span><span class="sxs-lookup"><span data-stu-id="c517f-132">This package contains packages like `Microsoft.AspNetCore.Mvc` and `Microsoft.AspNetCore.StaticFiles`.</span></span> <span data-ttu-id="c517f-133">Jeśli przeznaczonych dla platformy .NET Framework, odwołania do pakietu należy wymienione oddzielnie w pliku \*.csproj.</span><span class="sxs-lookup"><span data-stu-id="c517f-133">If targeting .NET Framework, package references need to be listed individually in the \*.csproj file.</span></span>

<span data-ttu-id="c517f-134">`Microsoft.AspNetCore.Mvc` to platforma ASP.NET Core MVC.</span><span class="sxs-lookup"><span data-stu-id="c517f-134">`Microsoft.AspNetCore.Mvc` is the ASP.NET Core MVC framework.</span></span> <span data-ttu-id="c517f-135">`Microsoft.AspNetCore.StaticFiles` Umożliwia to obsługę plików statycznych.</span><span class="sxs-lookup"><span data-stu-id="c517f-135">`Microsoft.AspNetCore.StaticFiles` is the static file handler.</span></span> <span data-ttu-id="c517f-136">Środowisko uruchomieniowe platformy ASP.NET Core jest moduły i musi jawnie zgłosić się do obsługi plików statycznych (zobacz [pliki statyczne](xref:fundamentals/static-files)).</span><span class="sxs-lookup"><span data-stu-id="c517f-136">The ASP.NET Core runtime is modular, and you must explicitly opt in to serve static files (see [Static files](xref:fundamentals/static-files)).</span></span>

* <span data-ttu-id="c517f-137">Otwórz *Startup.cs* plików i zmień kod zgodnie z poniższym:</span><span class="sxs-lookup"><span data-stu-id="c517f-137">Open the *Startup.cs* file and change the code to match the following:</span></span>

  [!code-csharp[](mvc/sample/Startup.cs?highlight=13,26-31)]

<span data-ttu-id="c517f-138">`UseStaticFiles` — Metoda rozszerzenia dodaje obsługę plików statycznych.</span><span class="sxs-lookup"><span data-stu-id="c517f-138">The `UseStaticFiles` extension method adds the static file handler.</span></span> <span data-ttu-id="c517f-139">Jak wspomniano wcześniej, środowiska uruchomieniowego ASP.NET jest moduły i musi jawnie zgłosić się do obsługi plików statycznych.</span><span class="sxs-lookup"><span data-stu-id="c517f-139">As mentioned previously, the ASP.NET runtime is modular, and you must explicitly opt in to serve static files.</span></span> <span data-ttu-id="c517f-140">`UseMvc` — Metoda rozszerzenia dodaje routingu.</span><span class="sxs-lookup"><span data-stu-id="c517f-140">The `UseMvc` extension method adds routing.</span></span> <span data-ttu-id="c517f-141">Aby uzyskać więcej informacji, zobacz [uruchamiania aplikacji](xref:fundamentals/startup) i [Routing](xref:fundamentals/routing).</span><span class="sxs-lookup"><span data-stu-id="c517f-141">For more information, see [Application Startup](xref:fundamentals/startup) and [Routing](xref:fundamentals/routing).</span></span>

## <a name="add-a-controller-and-view"></a><span data-ttu-id="c517f-142">Dodawanie kontrolera i widoku</span><span class="sxs-lookup"><span data-stu-id="c517f-142">Add a controller and view</span></span>

<span data-ttu-id="c517f-143">W tej sekcji możesz dodać minimalny kontrolera i widoku jako symbole zastępcze dla kontrolera ASP.NET MVC i widoki, będzie migracji w następnej sekcji.</span><span class="sxs-lookup"><span data-stu-id="c517f-143">In this section, you'll add a minimal controller and view to serve as placeholders for the ASP.NET MVC controller and views you'll migrate in the next section.</span></span>

* <span data-ttu-id="c517f-144">Dodaj *kontrolerów* folderu.</span><span class="sxs-lookup"><span data-stu-id="c517f-144">Add a *Controllers* folder.</span></span>

* <span data-ttu-id="c517f-145">Dodaj **klasy kontrolera** o nazwie *HomeController.cs* do *kontrolerów* folderu.</span><span class="sxs-lookup"><span data-stu-id="c517f-145">Add a **Controller Class** named *HomeController.cs* to the *Controllers* folder.</span></span>

![Dodaj nowy element okna dialogowego](mvc/_static/add_mvc_ctl.png)

* <span data-ttu-id="c517f-147">Dodaj *widoków* folderu.</span><span class="sxs-lookup"><span data-stu-id="c517f-147">Add a *Views* folder.</span></span>

* <span data-ttu-id="c517f-148">Dodaj *widoków domowych* folderu.</span><span class="sxs-lookup"><span data-stu-id="c517f-148">Add a *Views/Home* folder.</span></span>

* <span data-ttu-id="c517f-149">Dodaj **widoku Razor** o nazwie *Index.cshtml* do *widoków domowych* folderu.</span><span class="sxs-lookup"><span data-stu-id="c517f-149">Add a **Razor View** named *Index.cshtml* to the *Views/Home* folder.</span></span>

![Dodaj nowy element okna dialogowego](mvc/_static/view.png)

<span data-ttu-id="c517f-151">Poniżej przedstawiono struktury projektu:</span><span class="sxs-lookup"><span data-stu-id="c517f-151">The project structure is shown below:</span></span>

![Eksploratora rozwiązań przedstawiający pliki i foldery WebApp1](mvc/_static/project-structure-controller-view.png)

<span data-ttu-id="c517f-153">Zastąp zawartość *Views/Home/Index.cshtml* pliku następującym kodem:</span><span class="sxs-lookup"><span data-stu-id="c517f-153">Replace the contents of the *Views/Home/Index.cshtml* file with the following:</span></span>

```html
<h1>Hello world!</h1>
```

<span data-ttu-id="c517f-154">Uruchom aplikację.</span><span class="sxs-lookup"><span data-stu-id="c517f-154">Run the app.</span></span>

![Otwórz w programie Microsoft Edge aplikacji sieci Web](mvc/_static/hello-world.png)

<span data-ttu-id="c517f-156">Zobacz [kontrolerów](xref:mvc/controllers/actions) i [widoków](xref:mvc/views/overview) Aby uzyskać więcej informacji.</span><span class="sxs-lookup"><span data-stu-id="c517f-156">See [Controllers](xref:mvc/controllers/actions) and [Views](xref:mvc/views/overview) for more information.</span></span>

<span data-ttu-id="c517f-157">Teraz, gdy mamy minimalnego projektu platformy ASP.NET Core pracy, możemy rozpocząć Migrowanie funkcja z projektu programu ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="c517f-157">Now that we have a minimal working ASP.NET Core project, we can start migrating functionality from the ASP.NET MVC project.</span></span> <span data-ttu-id="c517f-158">Trzeba przenieść następujące czynności:</span><span class="sxs-lookup"><span data-stu-id="c517f-158">We need to move the following:</span></span>

* <span data-ttu-id="c517f-159">zawartość po stronie klienta (CSS, czcionki i skrypty)</span><span class="sxs-lookup"><span data-stu-id="c517f-159">client-side content (CSS, fonts, and scripts)</span></span>

* <span data-ttu-id="c517f-160">kontrolery</span><span class="sxs-lookup"><span data-stu-id="c517f-160">controllers</span></span>

* <span data-ttu-id="c517f-161">widoki</span><span class="sxs-lookup"><span data-stu-id="c517f-161">views</span></span>

* <span data-ttu-id="c517f-162">modele</span><span class="sxs-lookup"><span data-stu-id="c517f-162">models</span></span>

* <span data-ttu-id="c517f-163">Tworzenie pakietów</span><span class="sxs-lookup"><span data-stu-id="c517f-163">bundling</span></span>

* <span data-ttu-id="c517f-164">filtry</span><span class="sxs-lookup"><span data-stu-id="c517f-164">filters</span></span>

* <span data-ttu-id="c517f-165">Zaloguj się we/wy tożsamości (jest to realizowane w następnym samouczku.)</span><span class="sxs-lookup"><span data-stu-id="c517f-165">Log in/out, Identity (This is done in the next tutorial.)</span></span>

## <a name="controllers-and-views"></a><span data-ttu-id="c517f-166">Kontrolery i widoki</span><span class="sxs-lookup"><span data-stu-id="c517f-166">Controllers and views</span></span>

* <span data-ttu-id="c517f-167">Skopiuj każdej z metod z platformy ASP.NET MVC `HomeController` do nowego `HomeController`.</span><span class="sxs-lookup"><span data-stu-id="c517f-167">Copy each of the methods from the ASP.NET MVC `HomeController` to the new `HomeController`.</span></span> <span data-ttu-id="c517f-168">Należy pamiętać, że w programie ASP.NET MVC, typ zwracany metody akcji kontrolera wbudowanych szablonów [ActionResult](https://msdn.microsoft.com/library/system.web.mvc.actionresult(v=vs.118).aspx); na platformie ASP.NET MVC Core, zwracany metody akcji `IActionResult` zamiast tego.</span><span class="sxs-lookup"><span data-stu-id="c517f-168">Note that in ASP.NET MVC, the built-in template's controller action method return type is [ActionResult](https://msdn.microsoft.com/library/system.web.mvc.actionresult(v=vs.118).aspx); in ASP.NET Core MVC, the action methods return `IActionResult` instead.</span></span> <span data-ttu-id="c517f-169">`ActionResult` implementuje `IActionResult`, więc nie trzeba zmienić zwracany typ metody akcji.</span><span class="sxs-lookup"><span data-stu-id="c517f-169">`ActionResult` implements `IActionResult`, so there's no need to change the return type of your action methods.</span></span>

* <span data-ttu-id="c517f-170">Kopiuj *About.cshtml*, *Contact.cshtml*, i *Index.cshtml* pliki widoku Razor z projektu programu ASP.NET MVC do projektu platformy ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="c517f-170">Copy the *About.cshtml*, *Contact.cshtml*, and *Index.cshtml* Razor view files from the ASP.NET MVC project to the ASP.NET Core project.</span></span>

* <span data-ttu-id="c517f-171">Uruchamianie aplikacji platformy ASP.NET Core i testowanie każdej metody.</span><span class="sxs-lookup"><span data-stu-id="c517f-171">Run the ASP.NET Core app and test each method.</span></span> <span data-ttu-id="c517f-172">Firma Microsoft nie migracji pliku układu i stylów jeszcze, więc renderowanych widoków tylko z zawartością w plikach widoku.</span><span class="sxs-lookup"><span data-stu-id="c517f-172">We haven't migrated the layout file or styles yet, so the rendered views only contain the content in the view files.</span></span> <span data-ttu-id="c517f-173">Nie będziesz mieć łącza plik wygenerowany układu `About` i `Contact` widoków, więc musisz wywołać je za pomocą przeglądarki (Zastąp **4492** numer portu używany w projekcie).</span><span class="sxs-lookup"><span data-stu-id="c517f-173">You won't have the layout file generated links for the `About` and `Contact` views, so you'll have to invoke them from the browser (replace **4492** with the port number used in your project).</span></span>

  * `http://localhost:4492/home/about`

  * `http://localhost:4492/home/contact`

![Skontaktuj się z pomocą strony](mvc/_static/contact-page.png)

<span data-ttu-id="c517f-175">Zwróć uwagę na Brak elementów stylami i menu.</span><span class="sxs-lookup"><span data-stu-id="c517f-175">Note the lack of styling and menu items.</span></span> <span data-ttu-id="c517f-176">Firma Microsoft będzie rozwiązać ten problem w następnej sekcji.</span><span class="sxs-lookup"><span data-stu-id="c517f-176">We'll fix that in the next section.</span></span>

## <a name="static-content"></a><span data-ttu-id="c517f-177">Zawartość statyczna</span><span class="sxs-lookup"><span data-stu-id="c517f-177">Static content</span></span>

<span data-ttu-id="c517f-178">W poprzednich wersjach programu ASP.NET MVC zawartość statyczną hostowanej z katalogu głównego projektu sieci web i został zmieszać z plikami po stronie serwera.</span><span class="sxs-lookup"><span data-stu-id="c517f-178">In previous versions of ASP.NET MVC, static content was hosted from the root of the web project and was intermixed with server-side files.</span></span> <span data-ttu-id="c517f-179">W przypadku platformy ASP.NET Core zawartość statyczną znajduje się w *wwwroot* folderu.</span><span class="sxs-lookup"><span data-stu-id="c517f-179">In ASP.NET Core, static content is hosted in the *wwwroot* folder.</span></span> <span data-ttu-id="c517f-180">Należy skopiować zawartość statyczną z aplikacji ASP.NET MVC starego *wwwroot* folder w projekcie platformy ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="c517f-180">You'll want to copy the static content from your old ASP.NET MVC app to the *wwwroot* folder in your ASP.NET Core project.</span></span> <span data-ttu-id="c517f-181">W tej konwersji próbkowania:</span><span class="sxs-lookup"><span data-stu-id="c517f-181">In this sample conversion:</span></span>

* <span data-ttu-id="c517f-182">Kopiuj *favicon.ico* pliku starego projektu MVC *wwwroot* folderu w projekcie platformy ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="c517f-182">Copy the *favicon.ico* file from the old MVC project to the *wwwroot* folder in the ASP.NET Core project.</span></span>

<span data-ttu-id="c517f-183">ASP.NET MVC stary projekt używa [Bootstrap](https://getbootstrap.com/) stylów i magazyny plików ładowania początkowego programu *zawartości* i *skryptów* folderów.</span><span class="sxs-lookup"><span data-stu-id="c517f-183">The old ASP.NET MVC project uses [Bootstrap](https://getbootstrap.com/) for its styling and stores the Bootstrap files in the *Content* and *Scripts* folders.</span></span> <span data-ttu-id="c517f-184">Szablon, który wygenerować stary projekt platformy ASP.NET MVC, odwołuje się do ładowania początkowego w pliku układu (*Views/Shared/_Layout.cshtml*).</span><span class="sxs-lookup"><span data-stu-id="c517f-184">The template, which generated the old ASP.NET MVC project, references Bootstrap in the layout file (*Views/Shared/_Layout.cshtml*).</span></span> <span data-ttu-id="c517f-185">Można skopiować *bootstrap.js* i *bootstrap.css* pliki z platformy ASP.NET MVC projektu do *wwwroot* folderu w nowym projekcie.</span><span class="sxs-lookup"><span data-stu-id="c517f-185">You could copy the *bootstrap.js* and *bootstrap.css* files from the ASP.NET MVC project to the *wwwroot* folder in the new project.</span></span> <span data-ttu-id="c517f-186">Zamiast tego zostanie dodany obsługę ładowania początkowego (i innych bibliotek po stronie klienta), za pomocą CDN w następnej sekcji.</span><span class="sxs-lookup"><span data-stu-id="c517f-186">Instead, we'll add support for Bootstrap (and other client-side libraries) using CDNs in the next section.</span></span>

## <a name="migrate-the-layout-file"></a><span data-ttu-id="c517f-187">Migracja pliku układu</span><span class="sxs-lookup"><span data-stu-id="c517f-187">Migrate the layout file</span></span>

* <span data-ttu-id="c517f-188">Kopiuj *_ViewStart.cshtml* pliku starego projektu programu ASP.NET MVC *widoków* folderu do projektu platformy ASP.NET Core *widoków* folderu.</span><span class="sxs-lookup"><span data-stu-id="c517f-188">Copy the *_ViewStart.cshtml* file from the old ASP.NET MVC project's *Views* folder into the ASP.NET Core project's *Views* folder.</span></span> <span data-ttu-id="c517f-189">*_ViewStart.cshtml* plik nie został zmieniony na platformie ASP.NET Core MVC.</span><span class="sxs-lookup"><span data-stu-id="c517f-189">The *_ViewStart.cshtml* file has not changed in ASP.NET Core MVC.</span></span>

* <span data-ttu-id="c517f-190">Utwórz *widoków/Shared* folderu.</span><span class="sxs-lookup"><span data-stu-id="c517f-190">Create a *Views/Shared* folder.</span></span>

* <span data-ttu-id="c517f-191">*Opcjonalnie:* kopiowania *_ViewImports.cshtml* z *FullAspNetCore* projektu MVC *widoków* folderu do projektu platformy ASP.NET Core  *Widoki* folderu.</span><span class="sxs-lookup"><span data-stu-id="c517f-191">*Optional:* Copy *_ViewImports.cshtml* from the *FullAspNetCore* MVC project's *Views* folder into the ASP.NET Core project's *Views* folder.</span></span> <span data-ttu-id="c517f-192">Usuń wszelkie deklaracji przestrzeni nazw w *_ViewImports.cshtml* pliku.</span><span class="sxs-lookup"><span data-stu-id="c517f-192">Remove any namespace declaration in the *_ViewImports.cshtml* file.</span></span> <span data-ttu-id="c517f-193">*_ViewImports.cshtml* plików zawiera przestrzenie nazw dla wszystkich plików widoku i w przypadku [pomocników tagów](xref:mvc/views/tag-helpers/intro).</span><span class="sxs-lookup"><span data-stu-id="c517f-193">The *_ViewImports.cshtml* file provides namespaces for all the view files and brings in [Tag Helpers](xref:mvc/views/tag-helpers/intro).</span></span> <span data-ttu-id="c517f-194">Pomocników tagów są używane w nowym pliku układu.</span><span class="sxs-lookup"><span data-stu-id="c517f-194">Tag Helpers are used in the new layout file.</span></span> <span data-ttu-id="c517f-195">*_ViewImports.cshtml* pliku jest nowa dla platformy ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="c517f-195">The *_ViewImports.cshtml* file is new for ASP.NET Core.</span></span>

* <span data-ttu-id="c517f-196">Kopia *_Layout.cshtml* pliku starego projektu programu ASP.NET MVC *widoków/Shared* folderu do projektu platformy ASP.NET Core *widoków/Shared* folderu.</span><span class="sxs-lookup"><span data-stu-id="c517f-196">Copy the *_Layout.cshtml* file from the old ASP.NET MVC project's *Views/Shared* folder into the ASP.NET Core project's *Views/Shared* folder.</span></span>

<span data-ttu-id="c517f-197">Otwórz *_Layout.cshtml* plik, a następnie dokonaj następujących zmian (kompletny kod przedstawiony poniżej):</span><span class="sxs-lookup"><span data-stu-id="c517f-197">Open *_Layout.cshtml* file and make the following changes (the completed code is shown below):</span></span>

* <span data-ttu-id="c517f-198">Zastąp `@Styles.Render("~/Content/css")` z `<link>` elementu, aby załadować *bootstrap.css* (patrz poniżej).</span><span class="sxs-lookup"><span data-stu-id="c517f-198">Replace `@Styles.Render("~/Content/css")` with a `<link>` element to load *bootstrap.css* (see below).</span></span>

* <span data-ttu-id="c517f-199">Usuń `@Scripts.Render("~/bundles/modernizr")`.</span><span class="sxs-lookup"><span data-stu-id="c517f-199">Remove `@Scripts.Render("~/bundles/modernizr")`.</span></span>

* <span data-ttu-id="c517f-200">Komentarz `@Html.Partial("_LoginPartial")` wiersza (przestrzenny wiersz z `@*...*@`).</span><span class="sxs-lookup"><span data-stu-id="c517f-200">Comment out the `@Html.Partial("_LoginPartial")` line (surround the line with `@*...*@`).</span></span> <span data-ttu-id="c517f-201">Wrócimy do niego w przyszłości samouczka.</span><span class="sxs-lookup"><span data-stu-id="c517f-201">We'll return to it in a future tutorial.</span></span>

* <span data-ttu-id="c517f-202">Zastąp `@Scripts.Render("~/bundles/jquery")` z `<script>` elementu (patrz poniżej).</span><span class="sxs-lookup"><span data-stu-id="c517f-202">Replace `@Scripts.Render("~/bundles/jquery")` with a `<script>` element (see below).</span></span>

* <span data-ttu-id="c517f-203">Zastąp `@Scripts.Render("~/bundles/bootstrap")` z `<script>` elementu (patrz poniżej).</span><span class="sxs-lookup"><span data-stu-id="c517f-203">Replace `@Scripts.Render("~/bundles/bootstrap")` with a `<script>` element (see below).</span></span>

<span data-ttu-id="c517f-204">Kod znaczników zastępczy włączenie ładowania początkowego CSS:</span><span class="sxs-lookup"><span data-stu-id="c517f-204">The replacement markup for Bootstrap CSS inclusion:</span></span>

```html
<link rel="stylesheet"
    href="https://maxcdn.bootstrapcdn.com/bootstrap/3.3.7/css/bootstrap.min.css"
    integrity="sha384-BVYiiSIFeK1dGmJRAkycuHAHRg32OmUcww7on3RYdg4Va+PmSTsz/K68vbdEjh4u"
    crossorigin="anonymous">
```

<span data-ttu-id="c517f-205">Kod znaczników zastępczy jQuery i włączenie ładowania początkowego JavaScript:</span><span class="sxs-lookup"><span data-stu-id="c517f-205">The replacement markup for jQuery and Bootstrap JavaScript inclusion:</span></span>

```html
<script src="https://code.jquery.com/jquery-3.3.1.min.js"></script>
<script src="https://maxcdn.bootstrapcdn.com/bootstrap/3.3.7/js/bootstrap.min.js"
    integrity="sha384-Tc5IQib027qvyjSMfHjOMaLkfuWVxZxUPnCJA7l2mCWNIpG9mGCD8wGNIcPD7Txa" crossorigin="anonymous"></script>
```

<span data-ttu-id="c517f-206">Zaktualizowany interfejs *_Layout.cshtml* pliku przedstawiono poniżej:</span><span class="sxs-lookup"><span data-stu-id="c517f-206">The updated *_Layout.cshtml* file is shown below:</span></span>

[!code-cshtml[](mvc/sample/Views/Shared/_Layout.cshtml?highlight=7-10,29,41-44)]

<span data-ttu-id="c517f-207">Wyświetlać witrynę w przeglądarce.</span><span class="sxs-lookup"><span data-stu-id="c517f-207">View the site in the browser.</span></span> <span data-ttu-id="c517f-208">Teraz powinien on załadowany poprawnie, z oczekiwanym style w miejscu.</span><span class="sxs-lookup"><span data-stu-id="c517f-208">It should now load correctly, with the expected styles in place.</span></span>

* <span data-ttu-id="c517f-209">*Opcjonalnie:* możesz chcieć użyć nowego pliku układu.</span><span class="sxs-lookup"><span data-stu-id="c517f-209">*Optional:* You might want to try using the new layout file.</span></span> <span data-ttu-id="c517f-210">Dla tego projektu można skopiować pliku układu z *FullAspNetCore* projektu.</span><span class="sxs-lookup"><span data-stu-id="c517f-210">For this project you can copy the layout file from the *FullAspNetCore* project.</span></span> <span data-ttu-id="c517f-211">Używa nowego pliku układu [pomocników tagów](xref:mvc/views/tag-helpers/intro) i ma inne ulepszenia.</span><span class="sxs-lookup"><span data-stu-id="c517f-211">The new layout file uses [Tag Helpers](xref:mvc/views/tag-helpers/intro) and has other improvements.</span></span>

## <a name="configure-bundling-and-minification"></a><span data-ttu-id="c517f-212">Konfigurowanie, tworzenie pakietów i minimalizowanie</span><span class="sxs-lookup"><span data-stu-id="c517f-212">Configure bundling and minification</span></span>

<span data-ttu-id="c517f-213">Aby uzyskać informacje o sposobie konfigurowania tworzenie pakietów i minimalizowanie, zobacz [tworzenie pakietów i minimalizowanie](../client-side/bundling-and-minification.md).</span><span class="sxs-lookup"><span data-stu-id="c517f-213">For information about how to configure bundling and minification, see [Bundling and Minification](../client-side/bundling-and-minification.md).</span></span>

## <a name="solve-http-500-errors"></a><span data-ttu-id="c517f-214">Rozwiązywanie błędów HTTP 500</span><span class="sxs-lookup"><span data-stu-id="c517f-214">Solve HTTP 500 errors</span></span>

<span data-ttu-id="c517f-215">Istnieje wiele problemów, które mogą spowodować, że komunikat o błędzie HTTP 500, które nie zawierają żadnych informacji o źródło problemu.</span><span class="sxs-lookup"><span data-stu-id="c517f-215">There are many problems that can cause a HTTP 500 error message that contain no information on the source of the problem.</span></span> <span data-ttu-id="c517f-216">Na przykład jeśli *Views/_ViewImports.cshtml* plik zawiera przestrzeni nazw, która nie istnieje w projekcie, zostanie wyświetlony komunikat o błędzie HTTP 500.</span><span class="sxs-lookup"><span data-stu-id="c517f-216">For example, if the *Views/_ViewImports.cshtml* file contains a namespace that doesn't exist in your project, you'll get a HTTP 500 error.</span></span> <span data-ttu-id="c517f-217">Domyślnie w aplikacji platformy ASP.NET Core `UseDeveloperExceptionPage` rozszerzenie zostanie automatycznie dodane do `IApplicationBuilder` i wykonywać, gdy konfiguracja jest *programowanie*.</span><span class="sxs-lookup"><span data-stu-id="c517f-217">By default in ASP.NET Core apps, the `UseDeveloperExceptionPage` extension is added to the `IApplicationBuilder` and executed when the configuration is *Development*.</span></span> <span data-ttu-id="c517f-218">To jest szczegółowo w następującym kodem:</span><span class="sxs-lookup"><span data-stu-id="c517f-218">This is detailed in the following code:</span></span>

[!code-csharp[](mvc/sample/Startup.cs?highlight=19-22)]

<span data-ttu-id="c517f-219">Nieobsługiwanych wyjątków w aplikacji sieci web platformy ASP.NET Core konwertuje odpowiedzi HTTP 500.</span><span class="sxs-lookup"><span data-stu-id="c517f-219">ASP.NET Core converts unhandled exceptions in a web app into HTTP 500 error responses.</span></span> <span data-ttu-id="c517f-220">Zwykle szczegóły błędu nie są uwzględniane w tych odpowiedzi, aby zapobiec ujawnieniu potencjalnie poufnych informacji o serwerze.</span><span class="sxs-lookup"><span data-stu-id="c517f-220">Normally, error details aren't included in these responses to prevent disclosure of potentially sensitive information about the server.</span></span> <span data-ttu-id="c517f-221">Zobacz **za pomocą strony wyjątek Developer** w [obsługi błędów](../fundamentals/error-handling.md) Aby uzyskać więcej informacji.</span><span class="sxs-lookup"><span data-stu-id="c517f-221">See **Using the Developer Exception Page** in [Handle errors](../fundamentals/error-handling.md) for more information.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="c517f-222">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="c517f-222">Additional resources</span></span>

* [<span data-ttu-id="c517f-223">Programowanie po stronie klienta</span><span class="sxs-lookup"><span data-stu-id="c517f-223">Client-side development</span></span>](xref:client-side/index)
* [<span data-ttu-id="c517f-224">Pomocnicy tagów</span><span class="sxs-lookup"><span data-stu-id="c517f-224">Tag Helpers</span></span>](xref:mvc/views/tag-helpers/intro)
