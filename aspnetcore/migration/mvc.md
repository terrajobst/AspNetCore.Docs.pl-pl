---
title: Migracja z programu ASP.NET MVC do platformy ASP.NET Core MVC
author: ardalis
description: Dowiedz się, jak rozpocząć pracę, migracji projektu MVC programu ASP.NET do ASP.NET Core MVC.
ms.author: riande
ms.date: 04/06/2019
uid: migration/mvc
ms.openlocfilehash: a9e2b41b933ed04a23515564892ed1694a4ac4f8
ms.sourcegitcommit: 5b0eca8c21550f95de3bb21096bd4fd4d9098026
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/27/2019
ms.locfileid: "64899728"
---
# <a name="migrate-from-aspnet-mvc-to-aspnet-core-mvc"></a><span data-ttu-id="b4e32-103">Migracja z programu ASP.NET MVC do platformy ASP.NET Core MVC</span><span class="sxs-lookup"><span data-stu-id="b4e32-103">Migrate from ASP.NET MVC to ASP.NET Core MVC</span></span>

<span data-ttu-id="b4e32-104">Przez [Rick Anderson](https://twitter.com/RickAndMSFT), [Daniel Roth](https://github.com/danroth27), [Steve Smith](https://ardalis.com/), i [Scott Addie](https://scottaddie.com)</span><span class="sxs-lookup"><span data-stu-id="b4e32-104">By [Rick Anderson](https://twitter.com/RickAndMSFT), [Daniel Roth](https://github.com/danroth27), [Steve Smith](https://ardalis.com/), and [Scott Addie](https://scottaddie.com)</span></span>

<span data-ttu-id="b4e32-105">W tym artykule pokazano, jak rozpocząć pracę w projekcie ASP.NET MVC do migrowania [ASP.NET Core MVC](../mvc/overview.md).</span><span class="sxs-lookup"><span data-stu-id="b4e32-105">This article shows how to get started migrating an ASP.NET MVC project to [ASP.NET Core MVC](../mvc/overview.md).</span></span> <span data-ttu-id="b4e32-106">W procesie jego wyróżnia wiele rzeczy, które zostały zmienione od platformy ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="b4e32-106">In the process, it highlights many of the things that have changed from ASP.NET MVC.</span></span> <span data-ttu-id="b4e32-107">Migracja z programu ASP.NET MVC jest wiele krok procesu, a w tym artykule opisano początkowej konfiguracji, podstawowe kontrolery i widoki, zawartość statyczną i zależności po stronie klienta.</span><span class="sxs-lookup"><span data-stu-id="b4e32-107">Migrating from ASP.NET MVC is a multiple step process and this article covers the initial setup, basic controllers and views, static content, and client-side dependencies.</span></span> <span data-ttu-id="b4e32-108">Dodatkowe artykuły obejmują Migrowanie konfiguracji i kodu tożsamości w wielu projektach ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="b4e32-108">Additional articles cover migrating configuration and identity code found in many ASP.NET MVC projects.</span></span>

> [!NOTE]
> <span data-ttu-id="b4e32-109">Numery wersji w przykładach, mogą być nieaktualne.</span><span class="sxs-lookup"><span data-stu-id="b4e32-109">The version numbers in the samples might not be current.</span></span> <span data-ttu-id="b4e32-110">Konieczne może być odpowiednio zaktualizować swoje projekty.</span><span class="sxs-lookup"><span data-stu-id="b4e32-110">You may need to update your projects accordingly.</span></span>

## <a name="create-the-starter-aspnet-mvc-project"></a><span data-ttu-id="b4e32-111">Tworzenie modułu uruchamiającego projekt składnika ASP.NET MVC</span><span class="sxs-lookup"><span data-stu-id="b4e32-111">Create the starter ASP.NET MVC project</span></span>

<span data-ttu-id="b4e32-112">Aby zademonstrować uaktualnienia, Zaczniemy od utworzenia aplikacji ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="b4e32-112">To demonstrate the upgrade, we'll start by creating a ASP.NET MVC app.</span></span> <span data-ttu-id="b4e32-113">Utwórz go o nazwie *WebApp1* tak pasujących przestrzeni nazw projektu ASP.NET Core, utworzymy w następnym kroku.</span><span class="sxs-lookup"><span data-stu-id="b4e32-113">Create it with the name *WebApp1* so the namespace matches the ASP.NET Core project we create in the next step.</span></span>

![Visual Studio okna dialogowego Nowy projekt](mvc/_static/new-project.png)

![Okno dialogowe Nowy aplikacji sieci Web: Szablon projektu MVC wybranego w panelu szablony platformy ASP.NET](mvc/_static/new-project-select-mvc-template.png)

<span data-ttu-id="b4e32-116">*Opcjonalnie:* Zmiana nazwy rozwiązania z *WebApp1* do *Mvc5*.</span><span class="sxs-lookup"><span data-stu-id="b4e32-116">*Optional:* Change the name of the Solution from *WebApp1* to *Mvc5*.</span></span> <span data-ttu-id="b4e32-117">Visual Studio Wyświetla nową nazwę rozwiązania (*Mvc5*), co pozwala łatwiej mówić tego projektu z projektu dalej.</span><span class="sxs-lookup"><span data-stu-id="b4e32-117">Visual Studio displays the new solution name (*Mvc5*), which makes it easier to tell this project from the next project.</span></span>

## <a name="create-the-aspnet-core-project"></a><span data-ttu-id="b4e32-118">Tworzenie projektu platformy ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="b4e32-118">Create the ASP.NET Core project</span></span>

<span data-ttu-id="b4e32-119">Utwórz nową *pusty* aplikacji internetowej ASP.NET Core z taką samą nazwę jak poprzedni projekt (*WebApp1*), dzięki czemu odpowiada przestrzeni nazw w dwóch projektów.</span><span class="sxs-lookup"><span data-stu-id="b4e32-119">Create a new *empty* ASP.NET Core web app with the same name as the previous project (*WebApp1*) so the namespaces in the two projects match.</span></span> <span data-ttu-id="b4e32-120">O tej samej przestrzeni nazw sprawia, że jest ona łatwiej będzie skopiować kod między dwa projekty.</span><span class="sxs-lookup"><span data-stu-id="b4e32-120">Having the same namespace makes it easier to copy code between the two projects.</span></span> <span data-ttu-id="b4e32-121">Będziesz mieć do tworzenia tego projektu w innym katalogu niż poprzednie projektu, aby użyć tej samej nazwie.</span><span class="sxs-lookup"><span data-stu-id="b4e32-121">You'll have to create this project in a different directory than the previous project to use the same name.</span></span>

![Okno dialogowe nowego projektu](mvc/_static/new_core.png)

![Okno dialogowe Nowy aplikacji sieci Web platformy ASP.NET: Pusty szablon projektu służący zaznaczonych w panelu szablony programu ASP.NET Core](mvc/_static/new-project-select-empty-aspnet5-template.png)

* <span data-ttu-id="b4e32-124">*Opcjonalnie:* Tworzenie nowej aplikacji platformy ASP.NET Core, za pomocą *aplikacji sieci Web* szablonu projektu.</span><span class="sxs-lookup"><span data-stu-id="b4e32-124">*Optional:* Create a new ASP.NET Core app using the *Web Application* project template.</span></span> <span data-ttu-id="b4e32-125">Nadaj projektowi nazwę *WebApp1*i wybierz opcję uwierzytelniania programu **indywidualne konta użytkowników**.</span><span class="sxs-lookup"><span data-stu-id="b4e32-125">Name the project *WebApp1*, and select an authentication option of **Individual User Accounts**.</span></span> <span data-ttu-id="b4e32-126">Zmień nazwę tej aplikacji do *FullAspNetCore*.</span><span class="sxs-lookup"><span data-stu-id="b4e32-126">Rename this app to *FullAspNetCore*.</span></span> <span data-ttu-id="b4e32-127">Tworzenie projektu oszczędza czas do konwersji.</span><span class="sxs-lookup"><span data-stu-id="b4e32-127">Creating this project saves you time in the conversion.</span></span> <span data-ttu-id="b4e32-128">Można sprawdzić kod wygenerowany szablon, aby zobaczyć efekt lub skopiować kod do projektu konwersji.</span><span class="sxs-lookup"><span data-stu-id="b4e32-128">You can look at the template-generated code to see the end result or to copy code to the conversion project.</span></span> <span data-ttu-id="b4e32-129">Jest również przydatne, gdy użytkownik zatrzymywane w kroku konwersji do porównania z wygenerowanych przez szablon projektu.</span><span class="sxs-lookup"><span data-stu-id="b4e32-129">It's also helpful when you get stuck on a conversion step to compare with the template-generated project.</span></span>

## <a name="configure-the-site-to-use-mvc"></a><span data-ttu-id="b4e32-130">Konfigurowanie lokacji w celu korzystania z aplikacji MVC</span><span class="sxs-lookup"><span data-stu-id="b4e32-130">Configure the site to use MVC</span></span>

::: moniker range=">= aspnetcore-2.1"

* <span data-ttu-id="b4e32-131">Podczas określania wartości docelowej platformy .NET Core [meta Microsoft.aspnetcore.all Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app) odwołuje się do domyślnego.</span><span class="sxs-lookup"><span data-stu-id="b4e32-131">When targeting .NET Core, the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) is referenced by default.</span></span> <span data-ttu-id="b4e32-132">Ten pakiet zawiera pakiety pakietów najczęściej używanych przez aplikacje platformy MVC.</span><span class="sxs-lookup"><span data-stu-id="b4e32-132">This package contains packages commonly used packages by MVC apps.</span></span> <span data-ttu-id="b4e32-133">Jeśli przeznaczony dla .NET Framework, odwołania do pakietu musi być indywidualnie wymieniony w pliku projektu.</span><span class="sxs-lookup"><span data-stu-id="b4e32-133">If targeting .NET Framework, package references must be listed individually in the project file.</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.0"

* <span data-ttu-id="b4e32-134">Podczas określania wartości docelowej platformy .NET Core [pakiet meta Microsoft.aspnetcore.all](xref:fundamentals/metapackage) odwołuje się do domyślnego.</span><span class="sxs-lookup"><span data-stu-id="b4e32-134">When targeting .NET Core, the [Microsoft.AspNetCore.All metapackage](xref:fundamentals/metapackage) is referenced by default.</span></span> <span data-ttu-id="b4e32-135">Ten pakiet zawiera pakiety pakietów najczęściej używanych przez aplikacje platformy MVC.</span><span class="sxs-lookup"><span data-stu-id="b4e32-135">This package contains packages commonly used packages by MVC apps.</span></span> <span data-ttu-id="b4e32-136">Jeśli przeznaczony dla .NET Framework, odwołania do pakietu musi być indywidualnie wymieniony w pliku projektu.</span><span class="sxs-lookup"><span data-stu-id="b4e32-136">If targeting .NET Framework, package references must be listed individually in the project file.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

* <span data-ttu-id="b4e32-137">Po przeznaczonych dla platformy .NET Core lub .NET Framework, pakiety pakietów najczęściej używanych przez aplikacje platformy MVC są wyświetlane indywidualnie w pliku projektu.</span><span class="sxs-lookup"><span data-stu-id="b4e32-137">When targeting .NET Core or .NET Framework, packages commonly used packages by MVC apps are listed individually in the project file.</span></span>

::: moniker-end

<span data-ttu-id="b4e32-138">`Microsoft.AspNetCore.Mvc` to platforma ASP.NET Core MVC.</span><span class="sxs-lookup"><span data-stu-id="b4e32-138">`Microsoft.AspNetCore.Mvc` is the ASP.NET Core MVC framework.</span></span> <span data-ttu-id="b4e32-139">`Microsoft.AspNetCore.StaticFiles` jest programem obsługi plików statycznych.</span><span class="sxs-lookup"><span data-stu-id="b4e32-139">`Microsoft.AspNetCore.StaticFiles` is the static file handler.</span></span> <span data-ttu-id="b4e32-140">Środowisko uruchomieniowe programu ASP.NET Core jest moduły i musi jawnie zgody na uczestnictwo w Obsługa plików statycznych (zobacz [pliki statyczne](xref:fundamentals/static-files)).</span><span class="sxs-lookup"><span data-stu-id="b4e32-140">The ASP.NET Core runtime is modular, and you must explicitly opt in to serve static files (see [Static files](xref:fundamentals/static-files)).</span></span>

* <span data-ttu-id="b4e32-141">Otwórz *Startup.cs* pliku i zmień kod, zgodnie z poniższym:</span><span class="sxs-lookup"><span data-stu-id="b4e32-141">Open the *Startup.cs* file and change the code to match the following:</span></span>

  [!code-csharp[](mvc/sample/Startup.cs?highlight=13,26-31)]

<span data-ttu-id="b4e32-142">`UseStaticFiles` — Metoda rozszerzenia dodaje procedurę obsługi plików statycznych.</span><span class="sxs-lookup"><span data-stu-id="b4e32-142">The `UseStaticFiles` extension method adds the static file handler.</span></span> <span data-ttu-id="b4e32-143">Jak wspomniano wcześniej, środowisko uruchomieniowe ASP.NET to moduły, a musi jawnie zgody na uczestnictwo w Obsługa plików statycznych.</span><span class="sxs-lookup"><span data-stu-id="b4e32-143">As mentioned previously, the ASP.NET runtime is modular, and you must explicitly opt in to serve static files.</span></span> <span data-ttu-id="b4e32-144">`UseMvc` — Metoda rozszerzenia dodaje routingu.</span><span class="sxs-lookup"><span data-stu-id="b4e32-144">The `UseMvc` extension method adds routing.</span></span> <span data-ttu-id="b4e32-145">Aby uzyskać więcej informacji, zobacz [uruchamiania aplikacji](xref:fundamentals/startup) i [Routing](xref:fundamentals/routing).</span><span class="sxs-lookup"><span data-stu-id="b4e32-145">For more information, see [Application Startup](xref:fundamentals/startup) and [Routing](xref:fundamentals/routing).</span></span>

## <a name="add-a-controller-and-view"></a><span data-ttu-id="b4e32-146">Dodaj kontroler i widok</span><span class="sxs-lookup"><span data-stu-id="b4e32-146">Add a controller and view</span></span>

<span data-ttu-id="b4e32-147">W tej sekcji dodasz minimalny kontroler i widok, aby służyć jako symbole zastępcze dla kontrolera ASP.NET MVC i widoki, że będzie migrowane w ramach następnej sekcji.</span><span class="sxs-lookup"><span data-stu-id="b4e32-147">In this section, you'll add a minimal controller and view to serve as placeholders for the ASP.NET MVC controller and views you'll migrate in the next section.</span></span>

* <span data-ttu-id="b4e32-148">Dodaj *kontrolerów* folderu.</span><span class="sxs-lookup"><span data-stu-id="b4e32-148">Add a *Controllers* folder.</span></span>

* <span data-ttu-id="b4e32-149">Dodaj **klasy kontrolera** o nazwie *HomeController.cs* do *kontrolerów* folderu.</span><span class="sxs-lookup"><span data-stu-id="b4e32-149">Add a **Controller Class** named *HomeController.cs* to the *Controllers* folder.</span></span>

![Dodaj okno dialogowe Nowy element](mvc/_static/add_mvc_ctl.png)

* <span data-ttu-id="b4e32-151">Dodaj *widoków* folderu.</span><span class="sxs-lookup"><span data-stu-id="b4e32-151">Add a *Views* folder.</span></span>

* <span data-ttu-id="b4e32-152">Dodaj *widoków domowych* folderu.</span><span class="sxs-lookup"><span data-stu-id="b4e32-152">Add a *Views/Home* folder.</span></span>

* <span data-ttu-id="b4e32-153">Dodaj **widoku Razor** o nazwie *Index.cshtml* do *widoków domowych* folderu.</span><span class="sxs-lookup"><span data-stu-id="b4e32-153">Add a **Razor View** named *Index.cshtml* to the *Views/Home* folder.</span></span>

![Dodaj okno dialogowe Nowy element](mvc/_static/view.png)

<span data-ttu-id="b4e32-155">Struktura projektu jest pokazany poniżej:</span><span class="sxs-lookup"><span data-stu-id="b4e32-155">The project structure is shown below:</span></span>

![Eksplorator rozwiązań pliki i foldery WebApp1](mvc/_static/project-structure-controller-view.png)

<span data-ttu-id="b4e32-157">Zastąp zawartość *Views/Home/Index.cshtml* pliku następującym kodem:</span><span class="sxs-lookup"><span data-stu-id="b4e32-157">Replace the contents of the *Views/Home/Index.cshtml* file with the following:</span></span>

```html
<h1>Hello world!</h1>
```

<span data-ttu-id="b4e32-158">Uruchom aplikację.</span><span class="sxs-lookup"><span data-stu-id="b4e32-158">Run the app.</span></span>

![Aplikacja sieci Web otwórz w programie Microsoft Edge](mvc/_static/hello-world.png)

<span data-ttu-id="b4e32-160">Zobacz [kontrolerów](xref:mvc/controllers/actions) i [widoków](xref:mvc/views/overview) Aby uzyskać więcej informacji.</span><span class="sxs-lookup"><span data-stu-id="b4e32-160">See [Controllers](xref:mvc/controllers/actions) and [Views](xref:mvc/views/overview) for more information.</span></span>

<span data-ttu-id="b4e32-161">Teraz, gdy minimalny projektu ASP.NET Core pracy, możemy rozpocząć migracji funkcji z projektu platformy ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="b4e32-161">Now that we have a minimal working ASP.NET Core project, we can start migrating functionality from the ASP.NET MVC project.</span></span> <span data-ttu-id="b4e32-162">Trzeba przenieść następujące czynności:</span><span class="sxs-lookup"><span data-stu-id="b4e32-162">We need to move the following:</span></span>

* <span data-ttu-id="b4e32-163">zawartość po stronie klienta (CSS, czcionki i skryptów)</span><span class="sxs-lookup"><span data-stu-id="b4e32-163">client-side content (CSS, fonts, and scripts)</span></span>

* <span data-ttu-id="b4e32-164">kontrolery</span><span class="sxs-lookup"><span data-stu-id="b4e32-164">controllers</span></span>

* <span data-ttu-id="b4e32-165">widoki</span><span class="sxs-lookup"><span data-stu-id="b4e32-165">views</span></span>

* <span data-ttu-id="b4e32-166">modele</span><span class="sxs-lookup"><span data-stu-id="b4e32-166">models</span></span>

* <span data-ttu-id="b4e32-167">Tworzenie pakietów</span><span class="sxs-lookup"><span data-stu-id="b4e32-167">bundling</span></span>

* <span data-ttu-id="b4e32-168">filtry</span><span class="sxs-lookup"><span data-stu-id="b4e32-168">filters</span></span>

* <span data-ttu-id="b4e32-169">Zaloguj się we/wy tożsamości (jest to wykonywane w następnym samouczku.)</span><span class="sxs-lookup"><span data-stu-id="b4e32-169">Log in/out, Identity (This is done in the next tutorial.)</span></span>

## <a name="controllers-and-views"></a><span data-ttu-id="b4e32-170">Kontrolery i widoki</span><span class="sxs-lookup"><span data-stu-id="b4e32-170">Controllers and views</span></span>

* <span data-ttu-id="b4e32-171">Skopiować każdą z metod z platformy ASP.NET MVC `HomeController` do nowego `HomeController`.</span><span class="sxs-lookup"><span data-stu-id="b4e32-171">Copy each of the methods from the ASP.NET MVC `HomeController` to the new `HomeController`.</span></span> <span data-ttu-id="b4e32-172">Należy pamiętać, że we wzorcu ASP.NET MVC, typ zwracany metody akcji kontrolera wbudowany szablon [ActionResult](https://msdn.microsoft.com/library/system.web.mvc.actionresult(v=vs.118).aspx); we wzorcu ASP.NET Core MVC, zwrotu metody akcji `IActionResult` zamiast tego.</span><span class="sxs-lookup"><span data-stu-id="b4e32-172">Note that in ASP.NET MVC, the built-in template's controller action method return type is [ActionResult](https://msdn.microsoft.com/library/system.web.mvc.actionresult(v=vs.118).aspx); in ASP.NET Core MVC, the action methods return `IActionResult` instead.</span></span> <span data-ttu-id="b4e32-173">`ActionResult` implementuje `IActionResult`, więc nie trzeba zmieniać zwracany typ metody akcji.</span><span class="sxs-lookup"><span data-stu-id="b4e32-173">`ActionResult` implements `IActionResult`, so there's no need to change the return type of your action methods.</span></span>

* <span data-ttu-id="b4e32-174">Kopiuj *About.cshtml*, *Contact.cshtml*, i *Index.cshtml* pliki widoku Razor z projektu platformy ASP.NET MVC do projektu programu ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="b4e32-174">Copy the *About.cshtml*, *Contact.cshtml*, and *Index.cshtml* Razor view files from the ASP.NET MVC project to the ASP.NET Core project.</span></span>

* <span data-ttu-id="b4e32-175">Uruchom aplikację ASP.NET Core, a każda metoda testowa.</span><span class="sxs-lookup"><span data-stu-id="b4e32-175">Run the ASP.NET Core app and test each method.</span></span> <span data-ttu-id="b4e32-176">Firma Microsoft nie jeszcze plik układu lub style jeszcze zmigrowane, więc renderowanych widoków zawierać tylko zawartości w plikach widoku.</span><span class="sxs-lookup"><span data-stu-id="b4e32-176">We haven't migrated the layout file or styles yet, so the rendered views only contain the content in the view files.</span></span> <span data-ttu-id="b4e32-177">Nie będziesz mieć linków do plików, wygenerowany układu dla `About` i `Contact` widoków, dzięki czemu będziesz mieć do wywołania, za pomocą przeglądarki (Zastąp **4492** przy użyciu numeru portu używanego w projekcie).</span><span class="sxs-lookup"><span data-stu-id="b4e32-177">You won't have the layout file generated links for the `About` and `Contact` views, so you'll have to invoke them from the browser (replace **4492** with the port number used in your project).</span></span>

  * `http://localhost:4492/home/about`

  * `http://localhost:4492/home/contact`

![Skontaktuj się z pomocą strony](mvc/_static/contact-page.png)

<span data-ttu-id="b4e32-179">Zwróć uwagę na Brak elementów menu i stylów.</span><span class="sxs-lookup"><span data-stu-id="b4e32-179">Note the lack of styling and menu items.</span></span> <span data-ttu-id="b4e32-180">Naprawimy, w następnej sekcji.</span><span class="sxs-lookup"><span data-stu-id="b4e32-180">We'll fix that in the next section.</span></span>

## <a name="static-content"></a><span data-ttu-id="b4e32-181">Zawartość statyczna</span><span class="sxs-lookup"><span data-stu-id="b4e32-181">Static content</span></span>

<span data-ttu-id="b4e32-182">W poprzednich wersjach programu ASP.NET MVC zawartość statyczna była hostowana w katalogu głównym projektu sieci web i został zmieszać z plikami po stronie serwera.</span><span class="sxs-lookup"><span data-stu-id="b4e32-182">In previous versions of ASP.NET MVC, static content was hosted from the root of the web project and was intermixed with server-side files.</span></span> <span data-ttu-id="b4e32-183">W programie ASP.NET Core zawartości statycznej znajduje się w *wwwroot* folderu.</span><span class="sxs-lookup"><span data-stu-id="b4e32-183">In ASP.NET Core, static content is hosted in the *wwwroot* folder.</span></span> <span data-ttu-id="b4e32-184">Należy skopiować zawartość statyczną z starych aplikacji ASP.NET MVC w celu *wwwroot* folder w projekcie platformy ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="b4e32-184">You'll want to copy the static content from your old ASP.NET MVC app to the *wwwroot* folder in your ASP.NET Core project.</span></span> <span data-ttu-id="b4e32-185">W tym konwersji próbki:</span><span class="sxs-lookup"><span data-stu-id="b4e32-185">In this sample conversion:</span></span>

* <span data-ttu-id="b4e32-186">Kopiuj *favicon.ico* plików ze starego projektu MVC do *wwwroot* folderu w projekcie platformy ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="b4e32-186">Copy the *favicon.ico* file from the old MVC project to the *wwwroot* folder in the ASP.NET Core project.</span></span>

<span data-ttu-id="b4e32-187">Stary platformy ASP.NET MVC projekt używa [Bootstrap](https://getbootstrap.com/) dla jego stylu i magazyny plików ładowania początkowego *zawartości* i *skrypty* folderów.</span><span class="sxs-lookup"><span data-stu-id="b4e32-187">The old ASP.NET MVC project uses [Bootstrap](https://getbootstrap.com/) for its styling and stores the Bootstrap files in the *Content* and *Scripts* folders.</span></span> <span data-ttu-id="b4e32-188">Szablon, który jest generowany starego projektu ASP.NET MVC, odwołuje się do ładowania początkowego w pliku układu (*Views/Shared/_Layout.cshtml*).</span><span class="sxs-lookup"><span data-stu-id="b4e32-188">The template, which generated the old ASP.NET MVC project, references Bootstrap in the layout file (*Views/Shared/_Layout.cshtml*).</span></span> <span data-ttu-id="b4e32-189">Można skopiować *bootstrap.js* i *bootstrap.css* projekt plików z platformy ASP.NET MVC *wwwroot* folderu w nowym projekcie.</span><span class="sxs-lookup"><span data-stu-id="b4e32-189">You could copy the *bootstrap.js* and *bootstrap.css* files from the ASP.NET MVC project to the *wwwroot* folder in the new project.</span></span> <span data-ttu-id="b4e32-190">Zamiast tego dodamy obsługę ładowania początkowego (i inne biblioteki po stronie klienta), przy użyciu usługi CDN w następnej sekcji.</span><span class="sxs-lookup"><span data-stu-id="b4e32-190">Instead, we'll add support for Bootstrap (and other client-side libraries) using CDNs in the next section.</span></span>

## <a name="migrate-the-layout-file"></a><span data-ttu-id="b4e32-191">Migrowanie pliku układu</span><span class="sxs-lookup"><span data-stu-id="b4e32-191">Migrate the layout file</span></span>

* <span data-ttu-id="b4e32-192">Kopia *_ViewStart.cshtml* plików ze starego projektu ASP.NET MVC *widoków* folderu do projektu programu ASP.NET Core *widoków* folderu.</span><span class="sxs-lookup"><span data-stu-id="b4e32-192">Copy the *_ViewStart.cshtml* file from the old ASP.NET MVC project's *Views* folder into the ASP.NET Core project's *Views* folder.</span></span> <span data-ttu-id="b4e32-193">*_ViewStart.cshtml* plik nie został zmieniony na platformie ASP.NET Core MVC.</span><span class="sxs-lookup"><span data-stu-id="b4e32-193">The *_ViewStart.cshtml* file has not changed in ASP.NET Core MVC.</span></span>

* <span data-ttu-id="b4e32-194">Tworzenie *widoków/Shared* folderu.</span><span class="sxs-lookup"><span data-stu-id="b4e32-194">Create a *Views/Shared* folder.</span></span>

* <span data-ttu-id="b4e32-195">*Opcjonalnie:* Kopiuj *_ViewImports.cshtml* z *FullAspNetCore* projektu MVC *widoków* folderu do projektu programu ASP.NET Core *widoków* folder.</span><span class="sxs-lookup"><span data-stu-id="b4e32-195">*Optional:* Copy *_ViewImports.cshtml* from the *FullAspNetCore* MVC project's *Views* folder into the ASP.NET Core project's *Views* folder.</span></span> <span data-ttu-id="b4e32-196">Usuń wszelkie deklarację przestrzeni nazw w *_ViewImports.cshtml* pliku.</span><span class="sxs-lookup"><span data-stu-id="b4e32-196">Remove any namespace declaration in the *_ViewImports.cshtml* file.</span></span> <span data-ttu-id="b4e32-197">*_ViewImports.cshtml* plik zawiera przestrzenie nazw dla wszystkich plików w widoku i wiąże [pomocników tagów](xref:mvc/views/tag-helpers/intro).</span><span class="sxs-lookup"><span data-stu-id="b4e32-197">The *_ViewImports.cshtml* file provides namespaces for all the view files and brings in [Tag Helpers](xref:mvc/views/tag-helpers/intro).</span></span> <span data-ttu-id="b4e32-198">Pomocnicy tagów są używane w nowym pliku układu.</span><span class="sxs-lookup"><span data-stu-id="b4e32-198">Tag Helpers are used in the new layout file.</span></span> <span data-ttu-id="b4e32-199">*_ViewImports.cshtml* nowego pliku, dla platformy ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="b4e32-199">The *_ViewImports.cshtml* file is new for ASP.NET Core.</span></span>

* <span data-ttu-id="b4e32-200">Kopia *_Layout.cshtml* plików ze starego projektu ASP.NET MVC *widoków/Shared* folderu do projektu programu ASP.NET Core *widoków/Shared* folderu.</span><span class="sxs-lookup"><span data-stu-id="b4e32-200">Copy the *_Layout.cshtml* file from the old ASP.NET MVC project's *Views/Shared* folder into the ASP.NET Core project's *Views/Shared* folder.</span></span>

<span data-ttu-id="b4e32-201">Otwórz *_Layout.cshtml* plik i dokonaj następujących zmian (kompletny kod znajduje się poniżej):</span><span class="sxs-lookup"><span data-stu-id="b4e32-201">Open *_Layout.cshtml* file and make the following changes (the completed code is shown below):</span></span>

* <span data-ttu-id="b4e32-202">Zastąp `@Styles.Render("~/Content/css")` z `<link>` elementu, aby załadować *bootstrap.css* (patrz poniżej).</span><span class="sxs-lookup"><span data-stu-id="b4e32-202">Replace `@Styles.Render("~/Content/css")` with a `<link>` element to load *bootstrap.css* (see below).</span></span>

* <span data-ttu-id="b4e32-203">Usuń `@Scripts.Render("~/bundles/modernizr")`.</span><span class="sxs-lookup"><span data-stu-id="b4e32-203">Remove `@Scripts.Render("~/bundles/modernizr")`.</span></span>

* <span data-ttu-id="b4e32-204">Komentarz `@Html.Partial("_LoginPartial")` wiersza (Otocz wiersz z `@*...*@`).</span><span class="sxs-lookup"><span data-stu-id="b4e32-204">Comment out the `@Html.Partial("_LoginPartial")` line (surround the line with `@*...*@`).</span></span> <span data-ttu-id="b4e32-205">Aby uzyskać więcej informacji, zobacz [migracji uwierzytelnianie i tożsamość do platformy ASP.NET Core](xref:migration/identity)</span><span class="sxs-lookup"><span data-stu-id="b4e32-205">For more information see [Migrate Authentication and Identity to ASP.NET Core](xref:migration/identity)</span></span>

* <span data-ttu-id="b4e32-206">Zastąp `@Scripts.Render("~/bundles/jquery")` z `<script>` — element (patrz poniżej).</span><span class="sxs-lookup"><span data-stu-id="b4e32-206">Replace `@Scripts.Render("~/bundles/jquery")` with a `<script>` element (see below).</span></span>

* <span data-ttu-id="b4e32-207">Zastąp `@Scripts.Render("~/bundles/bootstrap")` z `<script>` — element (patrz poniżej).</span><span class="sxs-lookup"><span data-stu-id="b4e32-207">Replace `@Scripts.Render("~/bundles/bootstrap")` with a `<script>` element (see below).</span></span>

<span data-ttu-id="b4e32-208">Znaczniki zastąpienia dla CSS szablonu Bootstrap dołączania:</span><span class="sxs-lookup"><span data-stu-id="b4e32-208">The replacement markup for Bootstrap CSS inclusion:</span></span>

```html
<link rel="stylesheet"
    href="https://maxcdn.bootstrapcdn.com/bootstrap/3.3.7/css/bootstrap.min.css"
    integrity="sha384-BVYiiSIFeK1dGmJRAkycuHAHRg32OmUcww7on3RYdg4Va+PmSTsz/K68vbdEjh4u"
    crossorigin="anonymous">
```

<span data-ttu-id="b4e32-209">Znaczniki zastąpienia dla jQuery i włączenia Bootstrap JavaScript:</span><span class="sxs-lookup"><span data-stu-id="b4e32-209">The replacement markup for jQuery and Bootstrap JavaScript inclusion:</span></span>

```html
<script src="https://code.jquery.com/jquery-3.3.1.min.js"></script>
<script src="https://maxcdn.bootstrapcdn.com/bootstrap/3.3.7/js/bootstrap.min.js"
    integrity="sha384-Tc5IQib027qvyjSMfHjOMaLkfuWVxZxUPnCJA7l2mCWNIpG9mGCD8wGNIcPD7Txa" crossorigin="anonymous"></script>
```

<span data-ttu-id="b4e32-210">Zaktualizowany interfejs *_Layout.cshtml* plików znajdują się poniżej:</span><span class="sxs-lookup"><span data-stu-id="b4e32-210">The updated *_Layout.cshtml* file is shown below:</span></span>

[!code-cshtml[](mvc/sample/Views/Shared/_Layout.cshtml?highlight=7-10,29,41-44)]

<span data-ttu-id="b4e32-211">Wyświetl witryny w przeglądarce.</span><span class="sxs-lookup"><span data-stu-id="b4e32-211">View the site in the browser.</span></span> <span data-ttu-id="b4e32-212">Teraz powinien on załadowany poprawnie, z oczekiwanym style w miejscu.</span><span class="sxs-lookup"><span data-stu-id="b4e32-212">It should now load correctly, with the expected styles in place.</span></span>

* <span data-ttu-id="b4e32-213">*Opcjonalnie:* Można spróbować użyć nowego pliku układu.</span><span class="sxs-lookup"><span data-stu-id="b4e32-213">*Optional:* You might want to try using the new layout file.</span></span> <span data-ttu-id="b4e32-214">Dla tego projektu można skopiować plik układu z *FullAspNetCore* projektu.</span><span class="sxs-lookup"><span data-stu-id="b4e32-214">For this project you can copy the layout file from the *FullAspNetCore* project.</span></span> <span data-ttu-id="b4e32-215">Nowy plik układu używa [pomocników tagów](xref:mvc/views/tag-helpers/intro) i ma inne ulepszenia.</span><span class="sxs-lookup"><span data-stu-id="b4e32-215">The new layout file uses [Tag Helpers](xref:mvc/views/tag-helpers/intro) and has other improvements.</span></span>

## <a name="configure-bundling-and-minification"></a><span data-ttu-id="b4e32-216">Skonfiguruj tworzenie pakietów i minimalizowanie</span><span class="sxs-lookup"><span data-stu-id="b4e32-216">Configure bundling and minification</span></span>

<span data-ttu-id="b4e32-217">Aby uzyskać informacje o sposobie konfigurowania tworzenie pakietów i minimalizowanie, zobacz [tworzenie pakietów i minimalizowanie](../client-side/bundling-and-minification.md).</span><span class="sxs-lookup"><span data-stu-id="b4e32-217">For information about how to configure bundling and minification, see [Bundling and Minification](../client-side/bundling-and-minification.md).</span></span>

## <a name="solve-http-500-errors"></a><span data-ttu-id="b4e32-218">Rozwiązuje błędy HTTP 500</span><span class="sxs-lookup"><span data-stu-id="b4e32-218">Solve HTTP 500 errors</span></span>

<span data-ttu-id="b4e32-219">Istnieje wiele problemów, które mogą spowodować, że komunikat o błędzie HTTP 500, które nie zawierają żadnych informacji do źródła problemu.</span><span class="sxs-lookup"><span data-stu-id="b4e32-219">There are many problems that can cause a HTTP 500 error message that contain no information on the source of the problem.</span></span> <span data-ttu-id="b4e32-220">Na przykład jeśli *Views/_ViewImports.cshtml* plik zawiera obszar nazw, który nie istnieje w projekcie, otrzymasz błąd HTTP 500.</span><span class="sxs-lookup"><span data-stu-id="b4e32-220">For example, if the *Views/_ViewImports.cshtml* file contains a namespace that doesn't exist in your project, you'll get a HTTP 500 error.</span></span> <span data-ttu-id="b4e32-221">Domyślnie w aplikacji platformy ASP.NET Core `UseDeveloperExceptionPage` rozszerzenia jest dodawany do `IApplicationBuilder` i wykonywany, gdy konfiguracja jest *rozwoju*.</span><span class="sxs-lookup"><span data-stu-id="b4e32-221">By default in ASP.NET Core apps, the `UseDeveloperExceptionPage` extension is added to the `IApplicationBuilder` and executed when the configuration is *Development*.</span></span> <span data-ttu-id="b4e32-222">Jest to szczegółowo w poniższym kodzie:</span><span class="sxs-lookup"><span data-stu-id="b4e32-222">This is detailed in the following code:</span></span>

[!code-csharp[](mvc/sample/Startup.cs?highlight=19-22)]

<span data-ttu-id="b4e32-223">Platforma ASP.NET Core konwertuje nieobsługiwanych wyjątków w aplikacji sieci web w odpowiedzi na błędy HTTP 500.</span><span class="sxs-lookup"><span data-stu-id="b4e32-223">ASP.NET Core converts unhandled exceptions in a web app into HTTP 500 error responses.</span></span> <span data-ttu-id="b4e32-224">Zwykle szczegóły błędu nie są uwzględniane w tych odpowiedzi, aby zapobiec ujawnieniu potencjalnie poufnych informacji o serwerze.</span><span class="sxs-lookup"><span data-stu-id="b4e32-224">Normally, error details aren't included in these responses to prevent disclosure of potentially sensitive information about the server.</span></span> <span data-ttu-id="b4e32-225">Zobacz **za pomocą strony wyjątek dla deweloperów** w [obsługi błędów](../fundamentals/error-handling.md) Aby uzyskać więcej informacji.</span><span class="sxs-lookup"><span data-stu-id="b4e32-225">See **Using the Developer Exception Page** in [Handle errors](../fundamentals/error-handling.md) for more information.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="b4e32-226">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="b4e32-226">Additional resources</span></span>

* <xref:blazor/index>
* <xref:mvc/views/tag-helpers/intro>
