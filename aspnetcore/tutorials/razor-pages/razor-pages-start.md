---
title: 'Samouczek: Wprowadzenie do Razor Pages w ASP.NET Core'
author: rick-anderson
description: W tej serii samouczków pokazano, jak używać Razor Pages w ASP.NET Core. Dowiedz się, jak utworzyć model, wygenerować kod dla stron Razor, użyć Entity Framework Core i SQL Server na potrzeby dostępu do danych, dodać funkcję wyszukiwania, dodać sprawdzanie poprawności danych wejściowych i użyć migracji w celu zaktualizowania modelu.
ms.author: riande
ms.date: 07/25/2019
uid: tutorials/razor-pages/razor-pages-start
ms.openlocfilehash: 0cc00cb85b6054752417b82c783cfd4c306aeda5
ms.sourcegitcommit: 215954a638d24124f791024c66fd4fb9109fd380
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 09/18/2019
ms.locfileid: "71082575"
---
# <a name="tutorial-get-started-with-razor-pages-in-aspnet-core"></a><span data-ttu-id="7fe49-104">Samouczek: Wprowadzenie do Razor Pages w ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="7fe49-104">Tutorial: Get started with Razor Pages in ASP.NET Core</span></span>

<span data-ttu-id="7fe49-105">Przez [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="7fe49-105">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

::: moniker range=">= aspnetcore-3.0"
<span data-ttu-id="7fe49-106">Jest to pierwszy samouczek dotyczący serii, który uczy się podstaw tworzenia aplikacji sieci Web Razor Pages ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="7fe49-106">This is the first tutorial of a series that teaches the basics of building an ASP.NET Core Razor Pages web app.</span></span>

[!INCLUDE[](~/includes/advancedRP.md)]

<span data-ttu-id="7fe49-107">Na końcu serii będziesz mieć aplikację, która zarządza bazą danych filmów.</span><span class="sxs-lookup"><span data-stu-id="7fe49-107">At the end of the series, you'll have an app that manages a database of movies.</span></span>  

[!INCLUDE[View or download sample code](~/includes/rp/download.md)]

<span data-ttu-id="7fe49-108">W tym samouczku przedstawiono następujące instrukcje:</span><span class="sxs-lookup"><span data-stu-id="7fe49-108">In this tutorial, you:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="7fe49-109">Utwórz aplikację sieci Web Razor Pages.</span><span class="sxs-lookup"><span data-stu-id="7fe49-109">Create a Razor Pages web app.</span></span>
> * <span data-ttu-id="7fe49-110">Uruchom aplikację.</span><span class="sxs-lookup"><span data-stu-id="7fe49-110">Run the app.</span></span>
> * <span data-ttu-id="7fe49-111">Przejrzyj pliki projektu.</span><span class="sxs-lookup"><span data-stu-id="7fe49-111">Examine the project files.</span></span>

<span data-ttu-id="7fe49-112">Na końcu tego samouczka będziesz mieć działającą Razor Pagesową aplikację internetową, która zostanie wdrożona w kolejnych samouczkach.</span><span class="sxs-lookup"><span data-stu-id="7fe49-112">At the end of this tutorial, you'll have a working Razor Pages web app that you'll build on in later tutorials.</span></span>

![Strona główna lub indeks](razor-pages-start/_static/home2.2.png)

## <a name="prerequisites"></a><span data-ttu-id="7fe49-114">Wymagania wstępne</span><span class="sxs-lookup"><span data-stu-id="7fe49-114">Prerequisites</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="7fe49-115">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="7fe49-115">Visual Studio</span></span>](#tab/visual-studio)

[!INCLUDE[](~/includes/net-core-prereqs-vs-3.0.md)]

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="7fe49-116">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="7fe49-116">Visual Studio Code</span></span>](#tab/visual-studio-code)

[!INCLUDE[](~/includes/net-core-prereqs-vsc-3.0.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="7fe49-117">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="7fe49-117">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

[!INCLUDE[](~/includes/net-core-prereqs-mac-3.0.md)]

---

## <a name="create-a-razor-pages-web-app"></a><span data-ttu-id="7fe49-118">Tworzenie aplikacji sieci Web Razor Pages</span><span class="sxs-lookup"><span data-stu-id="7fe49-118">Create a Razor Pages web app</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="7fe49-119">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="7fe49-119">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="7fe49-120">W programie Visual Studio **pliku** menu, wybierz opcję **New** > **projektu**.</span><span class="sxs-lookup"><span data-stu-id="7fe49-120">From the Visual Studio **File** menu, select **New** > **Project**.</span></span>
* <span data-ttu-id="7fe49-121">Utwórz nową aplikację sieci Web ASP.NET Core a następnie wybierz pozycję **dalej**.</span><span class="sxs-lookup"><span data-stu-id="7fe49-121">Create a new ASP.NET Core Web Application and select **Next**.</span></span>
  <span data-ttu-id="7fe49-122">![Nowa aplikacja sieci Web ASP.NET Core](razor-pages-start/_static/np_2.1.png)</span><span class="sxs-lookup"><span data-stu-id="7fe49-122">![new ASP.NET Core Web Application](razor-pages-start/_static/np_2.1.png)</span></span>
* <span data-ttu-id="7fe49-123">Nazwij projekt **RazorPagesMovie**.</span><span class="sxs-lookup"><span data-stu-id="7fe49-123">Name the project **RazorPagesMovie**.</span></span> <span data-ttu-id="7fe49-124">Ważne jest, aby nazwa projektu *RazorPagesMovie* , tak aby przestrzenie nazw były zgodne podczas kopiowania i wklejania kodu.</span><span class="sxs-lookup"><span data-stu-id="7fe49-124">It's important to name the project *RazorPagesMovie* so the namespaces will match when you copy and paste code.</span></span>
  <span data-ttu-id="7fe49-125">![Nowa aplikacja sieci Web ASP.NET Core](razor-pages-start/_static/config.png)</span><span class="sxs-lookup"><span data-stu-id="7fe49-125">![new ASP.NET Core Web Application](razor-pages-start/_static/config.png)</span></span>

* <span data-ttu-id="7fe49-126">Wybierz pozycję **ASP.NET Core 3,0** na liście rozwijanej, **aplikacji sieci Web**, a następnie wybierz pozycję **Utwórz**.</span><span class="sxs-lookup"><span data-stu-id="7fe49-126">Select **ASP.NET Core 3.0** in the dropdown, **Web Application**, and then select **Create**.</span></span>

![Nowa aplikacja sieci Web ASP.NET Core](razor-pages-start/_static/3/npx.png)

  <span data-ttu-id="7fe49-128">Tworzony jest następujący projekt początkowy:</span><span class="sxs-lookup"><span data-stu-id="7fe49-128">The following starter project is created:</span></span>

  ![Eksplorator rozwiązań](razor-pages-start/_static/se2.2.png)

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="7fe49-130">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="7fe49-130">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="7fe49-131">Otwórz [zintegrowany terminal](https://code.visualstudio.com/docs/editor/integrated-terminal).</span><span class="sxs-lookup"><span data-stu-id="7fe49-131">Open the [integrated terminal](https://code.visualstudio.com/docs/editor/integrated-terminal).</span></span>

* <span data-ttu-id="7fe49-132">Przejdź do katalogu (`cd`), który będzie zawierać projekt.</span><span class="sxs-lookup"><span data-stu-id="7fe49-132">Change to the directory (`cd`) which will contain the project.</span></span>

* <span data-ttu-id="7fe49-133">Uruchom następujące polecenia:</span><span class="sxs-lookup"><span data-stu-id="7fe49-133">Run the following commands:</span></span>

  ```dotnetcli
  dotnet new webapp -o RazorPagesMovie
  code -r RazorPagesMovie
  ```

  * <span data-ttu-id="7fe49-134">Polecenie tworzy nowy projekt Razor Pages w folderze RazorPagesMovie. `dotnet new`</span><span class="sxs-lookup"><span data-stu-id="7fe49-134">The `dotnet new` command creates a new Razor Pages project in the *RazorPagesMovie* folder.</span></span>
  * <span data-ttu-id="7fe49-135">Polecenie otwiera folder RazorPagesMovie w bieżącym wystąpieniu Visual Studio Code. `code`</span><span class="sxs-lookup"><span data-stu-id="7fe49-135">The `code` command opens the *RazorPagesMovie* folder in the current instance of Visual Studio Code.</span></span>

* <span data-ttu-id="7fe49-136">Gdy ikona płomienia OmniSharp na pasku stanu zmieni kolor na zielony, w **oknie dialogowym zostanie wyświetlony monit o podanie wymaganych zasobów do skompilowania i debugowania z elementu "RazorPagesMovie". Dodać je?**</span><span class="sxs-lookup"><span data-stu-id="7fe49-136">After the status bar's OmniSharp flame icon turns green, a dialog asks **Required assets to build and debug are missing from 'RazorPagesMovie'. Add them?**</span></span> <span data-ttu-id="7fe49-137">Wybierz pozycję **tak**.</span><span class="sxs-lookup"><span data-stu-id="7fe49-137">Select **Yes**.</span></span>

  <span data-ttu-id="7fe49-138">Katalog *. programu vscode* , zawierający pliki *Launch. JSON* i *Tasks. JSON* , jest dodawany do katalogu głównego projektu.</span><span class="sxs-lookup"><span data-stu-id="7fe49-138">A *.vscode* directory, containing *launch.json* and *tasks.json* files, is added to the project's root directory.</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="7fe49-139">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="7fe49-139">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="7fe49-140">Wybierz pozycję **plik** > **nowe rozwiązanie**.</span><span class="sxs-lookup"><span data-stu-id="7fe49-140">Select **File** > **New Solution**.</span></span>

![Nowe rozwiązanie w systemie macOS](../first-mvc-app/start-mvc/_static/new_project_vsmac.png)

* <span data-ttu-id="7fe49-142">Wybierz pozycję > **aplikacja** > internetowa> aplikacji .NET Core.</span><span class="sxs-lookup"><span data-stu-id="7fe49-142">Select **.NET Core** > **App** > **Web Application** > **Next**.</span></span>

  ![okno dialogowe z systemem macOS nowego projektu](razor-pages-start/_static/webapp.png)

* <span data-ttu-id="7fe49-144">W oknie dialogowym **Konfigurowanie nowego interfejsu API sieci Web ASP.NET Core** Ustaw platformę **docelową** na **.NET Core 3,0**.</span><span class="sxs-lookup"><span data-stu-id="7fe49-144">In the **Configure your new ASP.NET Core Web API** dialog, set the  **Target Framework** to **.NET Core 3.0**.</span></span>

  ![wybór macOS .NET Core 3,0](razor-pages-start/_static/targetframework3.png)

* <span data-ttu-id="7fe49-146">Nazwij projekt **RazorPagesMovie**, a następnie wybierz pozycję **Utwórz**.</span><span class="sxs-lookup"><span data-stu-id="7fe49-146">Name the project **RazorPagesMovie**, and then select **Create**.</span></span>

  ![nameproj](razor-pages-start/_static/RazorPagesMovie.png)


## <a name="open-the-project"></a><span data-ttu-id="7fe49-148">Otwórz projekt</span><span class="sxs-lookup"><span data-stu-id="7fe49-148">Open the project</span></span>

<span data-ttu-id="7fe49-149">W programie Visual Studio wybierz pozycję **plik > Otwórz**, a następnie wybierz plik *RazorPagesMovie. csproj* .</span><span class="sxs-lookup"><span data-stu-id="7fe49-149">From Visual Studio, select **File > Open**, and then select the *RazorPagesMovie.csproj* file.</span></span>

<!-- End of VS tabs -->

---

## <a name="run-the-app"></a><span data-ttu-id="7fe49-150">Uruchamianie aplikacji</span><span class="sxs-lookup"><span data-stu-id="7fe49-150">Run the app</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="7fe49-151">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="7fe49-151">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="7fe49-152">Naciśnij klawisze CTRL + F5, aby uruchomić bez debugera.</span><span class="sxs-lookup"><span data-stu-id="7fe49-152">Press Ctrl+F5 to run without the debugger.</span></span>

  [!INCLUDE[](~/includes/trustCertVS.md)]

  <span data-ttu-id="7fe49-153">Program Visual Studio jest uruchamiany [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) i uruchomi aplikację.</span><span class="sxs-lookup"><span data-stu-id="7fe49-153">Visual Studio starts [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) and runs the app.</span></span> <span data-ttu-id="7fe49-154">Na pasku adresu są `localhost:port#` wyświetlane inne elementy, `example.com`takie jak.</span><span class="sxs-lookup"><span data-stu-id="7fe49-154">The address bar shows `localhost:port#` and not something like `example.com`.</span></span> <span data-ttu-id="7fe49-155">Wynika `localhost` to z tego, że jest standardową nazwą hosta dla komputera lokalnego.</span><span class="sxs-lookup"><span data-stu-id="7fe49-155">That's because `localhost` is the standard hostname for the local computer.</span></span> <span data-ttu-id="7fe49-156">Host lokalny obsługuje tylko żądania sieci Web z komputera lokalnego.</span><span class="sxs-lookup"><span data-stu-id="7fe49-156">Localhost only serves web requests from the local computer.</span></span> <span data-ttu-id="7fe49-157">Gdy program Visual Studio tworzy projekt sieci Web, dla serwera sieci Web jest używany port losowy.</span><span class="sxs-lookup"><span data-stu-id="7fe49-157">When Visual Studio creates a web project, a random port is used for the web server.</span></span>
 
# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="7fe49-158">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="7fe49-158">Visual Studio Code</span></span>](#tab/visual-studio-code)

  [!INCLUDE[](~/includes/trustCertVSC.md)]

* <span data-ttu-id="7fe49-159">Naciśnij **klawisze CTRL + F5** , aby uruchomić bez debugera.</span><span class="sxs-lookup"><span data-stu-id="7fe49-159">Press **Ctrl-F5** to run without the debugger.</span></span>

  <span data-ttu-id="7fe49-160">Visual Studio Code uruchamia [Kestrel](xref:fundamentals/servers/kestrel), uruchamia przeglądarkę i nawiguje do `http://localhost:5001`.</span><span class="sxs-lookup"><span data-stu-id="7fe49-160">Visual Studio Code starts [Kestrel](xref:fundamentals/servers/kestrel), launches a browser, and navigates to `http://localhost:5001`.</span></span> <span data-ttu-id="7fe49-161">Na pasku adresu są `localhost:port#` wyświetlane inne elementy, `example.com`takie jak.</span><span class="sxs-lookup"><span data-stu-id="7fe49-161">The address bar shows `localhost:port#` and not something like `example.com`.</span></span> <span data-ttu-id="7fe49-162">Wynika `localhost` to z tego, że jest standardową nazwą hosta dla komputera lokalnego.</span><span class="sxs-lookup"><span data-stu-id="7fe49-162">That's because `localhost` is the standard hostname for  local computer.</span></span> <span data-ttu-id="7fe49-163">Host lokalny obsługuje tylko żądania sieci Web z komputera lokalnego.</span><span class="sxs-lookup"><span data-stu-id="7fe49-163">Localhost only serves web requests from the local computer.</span></span>

  
# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="7fe49-164">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="7fe49-164">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

  [!INCLUDE[](~/includes/trustCertMac.md)]

* <span data-ttu-id="7fe49-165">Naciśnij klawisze **Alt-cmd-Enter** , aby uruchomić bez debugera.</span><span class="sxs-lookup"><span data-stu-id="7fe49-165">Press **Alt-Cmd-Enter** to run without the debugger.</span></span> <span data-ttu-id="7fe49-166">Alternatywnie przejdź do paska menu i przejdź do pozycji Uruchom > Uruchom bez debugowania.</span><span class="sxs-lookup"><span data-stu-id="7fe49-166">Alternatively, navigate to the menu bar and go to Run>Start Without Debugging.</span></span>

  <span data-ttu-id="7fe49-167">Program Visual Studio uruchamia [Kestrel](xref:fundamentals/servers/kestrel), uruchamia przeglądarkę i przechodzi do `http://localhost:5001`.</span><span class="sxs-lookup"><span data-stu-id="7fe49-167">Visual Studio starts [Kestrel](xref:fundamentals/servers/kestrel), launches a browser, and navigates to `http://localhost:5001`.</span></span>

<!-- End of VS tabs -->

---

## <a name="examine-the-project-files"></a><span data-ttu-id="7fe49-168">Sprawdzanie plików projektu</span><span class="sxs-lookup"><span data-stu-id="7fe49-168">Examine the project files</span></span>

<span data-ttu-id="7fe49-169">Poniżej przedstawiono Omówienie folderów i plików projektu głównego, z których będziesz korzystać w kolejnych samouczkach.</span><span class="sxs-lookup"><span data-stu-id="7fe49-169">Here's an overview of the main project folders and files that you'll work with in later tutorials.</span></span>

### <a name="pages-folder"></a><span data-ttu-id="7fe49-170">Folder stron</span><span class="sxs-lookup"><span data-stu-id="7fe49-170">Pages folder</span></span>

<span data-ttu-id="7fe49-171">Zawiera strony Razor i pliki pomocnicze.</span><span class="sxs-lookup"><span data-stu-id="7fe49-171">Contains Razor pages and supporting files.</span></span> <span data-ttu-id="7fe49-172">Każda Strona Razor to para plików:</span><span class="sxs-lookup"><span data-stu-id="7fe49-172">Each Razor page is a pair of files:</span></span>

* <span data-ttu-id="7fe49-173">Plik *. cshtml* , który zawiera znaczniki HTML z C# kodem przy użyciu składnia Razor.</span><span class="sxs-lookup"><span data-stu-id="7fe49-173">A *.cshtml* file that contains HTML markup with C# code using Razor syntax.</span></span>
* <span data-ttu-id="7fe49-174">Plik *. cshtml.cs* , który zawiera C# kod, który obsługuje zdarzenia strony.</span><span class="sxs-lookup"><span data-stu-id="7fe49-174">A *.cshtml.cs* file that contains C# code that handles page events.</span></span>

<span data-ttu-id="7fe49-175">Pliki pomocnicze mają nazwy zaczynające się od znaku podkreślenia.</span><span class="sxs-lookup"><span data-stu-id="7fe49-175">Supporting files have names that begin with an underscore.</span></span> <span data-ttu-id="7fe49-176">Na przykład plik *_Layout. cshtml* służy do konfigurowania elementów interfejsu użytkownika wspólnych dla wszystkich stron.</span><span class="sxs-lookup"><span data-stu-id="7fe49-176">For example, the *_Layout.cshtml* file configures UI elements common to all pages.</span></span> <span data-ttu-id="7fe49-177">Ten plik konfiguruje menu nawigacji w górnej części strony i informacje o prawach autorskich w dolnej części strony.</span><span class="sxs-lookup"><span data-stu-id="7fe49-177">This file sets up the navigation menu at the top of the page and the copyright notice at the bottom of the page.</span></span> <span data-ttu-id="7fe49-178">Aby uzyskać więcej informacji, zobacz <xref:mvc/views/layout>.</span><span class="sxs-lookup"><span data-stu-id="7fe49-178">For more information, see <xref:mvc/views/layout>.</span></span>

### <a name="wwwroot-folder"></a><span data-ttu-id="7fe49-179">folder wwwroot</span><span class="sxs-lookup"><span data-stu-id="7fe49-179">wwwroot folder</span></span>

<span data-ttu-id="7fe49-180">Zawiera pliki statyczne, takie jak pliki HTML, pliki JavaScript i pliki CSS.</span><span class="sxs-lookup"><span data-stu-id="7fe49-180">Contains static files, such as HTML files, JavaScript files, and CSS files.</span></span> <span data-ttu-id="7fe49-181">Aby uzyskać więcej informacji, zobacz <xref:fundamentals/static-files>.</span><span class="sxs-lookup"><span data-stu-id="7fe49-181">For more information, see <xref:fundamentals/static-files>.</span></span>

### <a name="appsettingsjson"></a><span data-ttu-id="7fe49-182">appSettings.json</span><span class="sxs-lookup"><span data-stu-id="7fe49-182">appSettings.json</span></span>

<span data-ttu-id="7fe49-183">Zawiera dane konfiguracyjne, takie jak parametry połączenia.</span><span class="sxs-lookup"><span data-stu-id="7fe49-183">Contains configuration data, such as connection strings.</span></span> <span data-ttu-id="7fe49-184">Aby uzyskać więcej informacji, zobacz <xref:fundamentals/configuration/index>.</span><span class="sxs-lookup"><span data-stu-id="7fe49-184">For more information, see <xref:fundamentals/configuration/index>.</span></span>

### <a name="programcs"></a><span data-ttu-id="7fe49-185">Program.cs</span><span class="sxs-lookup"><span data-stu-id="7fe49-185">Program.cs</span></span>

<span data-ttu-id="7fe49-186">Zawiera punkt wejścia dla programu.</span><span class="sxs-lookup"><span data-stu-id="7fe49-186">Contains the entry point for the program.</span></span> <span data-ttu-id="7fe49-187">Aby uzyskać więcej informacji, zobacz <xref:fundamentals/host/generic-host>.</span><span class="sxs-lookup"><span data-stu-id="7fe49-187">For more information, see <xref:fundamentals/host/generic-host>.</span></span>

### <a name="startupcs"></a><span data-ttu-id="7fe49-188">Startup.cs</span><span class="sxs-lookup"><span data-stu-id="7fe49-188">Startup.cs</span></span>

<span data-ttu-id="7fe49-189">Zawiera kod, który konfiguruje zachowanie aplikacji.</span><span class="sxs-lookup"><span data-stu-id="7fe49-189">Contains code that configures app behavior.</span></span> <span data-ttu-id="7fe49-190">Aby uzyskać więcej informacji, zobacz <xref:fundamentals/startup>.</span><span class="sxs-lookup"><span data-stu-id="7fe49-190">For more information, see <xref:fundamentals/startup>.</span></span>

## <a name="next-steps"></a><span data-ttu-id="7fe49-191">Następne kroki</span><span class="sxs-lookup"><span data-stu-id="7fe49-191">Next steps</span></span>

<span data-ttu-id="7fe49-192">Przejdź do następnego samouczka z serii:</span><span class="sxs-lookup"><span data-stu-id="7fe49-192">Advance to the next tutorial in the series:</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="7fe49-193">Dodawanie modelu</span><span class="sxs-lookup"><span data-stu-id="7fe49-193">Add a model</span></span>](xref:tutorials/razor-pages/model)

::: moniker-end

<!--::: moniker range=">= aspnetcore-3.0" -->

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="7fe49-194">Jest to pierwszy samouczek dotyczący serii.</span><span class="sxs-lookup"><span data-stu-id="7fe49-194">This is the first tutorial of a series.</span></span> <span data-ttu-id="7fe49-195">[Seria zawiera](xref:tutorials/razor-pages/index) podstawowe informacje na temat tworzenia aplikacji sieci web ASP.NET Core Razor Pages.</span><span class="sxs-lookup"><span data-stu-id="7fe49-195">[The series](xref:tutorials/razor-pages/index) teaches the basics of building an ASP.NET Core Razor Pages web app.</span></span>

[!INCLUDE[](~/includes/advancedRP.md)]

<span data-ttu-id="7fe49-196">Na końcu serii będziesz mieć aplikację, która zarządza bazą danych filmów.</span><span class="sxs-lookup"><span data-stu-id="7fe49-196">At the end of the series, you'll have an app that manages a database of movies.</span></span>  

[!INCLUDE[View or download sample code](~/includes/rp/download.md)]

<span data-ttu-id="7fe49-197">W tym samouczku przedstawiono następujące instrukcje:</span><span class="sxs-lookup"><span data-stu-id="7fe49-197">In this tutorial, you:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="7fe49-198">Utwórz aplikację sieci Web Razor Pages.</span><span class="sxs-lookup"><span data-stu-id="7fe49-198">Create a Razor Pages web app.</span></span>
> * <span data-ttu-id="7fe49-199">Uruchom aplikację.</span><span class="sxs-lookup"><span data-stu-id="7fe49-199">Run the app.</span></span>
> * <span data-ttu-id="7fe49-200">Przejrzyj pliki projektu.</span><span class="sxs-lookup"><span data-stu-id="7fe49-200">Examine the project files.</span></span>

<span data-ttu-id="7fe49-201">Na końcu tego samouczka będziesz mieć działającą Razor Pagesową aplikację internetową, która zostanie wdrożona w kolejnych samouczkach.</span><span class="sxs-lookup"><span data-stu-id="7fe49-201">At the end of this tutorial, you'll have a working Razor Pages web app that you'll build on in later tutorials.</span></span>

![Strona główna lub indeks](razor-pages-start/_static/home2.2.png)

## <a name="prerequisites"></a><span data-ttu-id="7fe49-203">Wymagania wstępne</span><span class="sxs-lookup"><span data-stu-id="7fe49-203">Prerequisites</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="7fe49-204">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="7fe49-204">Visual Studio</span></span>](#tab/visual-studio)

[!INCLUDE[](~/includes/net-core-prereqs-vs2019-2.2.md)]

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="7fe49-205">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="7fe49-205">Visual Studio Code</span></span>](#tab/visual-studio-code)

[!INCLUDE[](~/includes/net-core-prereqs-vsc-2.2.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="7fe49-206">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="7fe49-206">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

[!INCLUDE[](~/includes/net-core-prereqs-mac-2.2.md)]

---

## <a name="create-a-razor-pages-web-app"></a><span data-ttu-id="7fe49-207">Tworzenie aplikacji sieci Web Razor Pages</span><span class="sxs-lookup"><span data-stu-id="7fe49-207">Create a Razor Pages web app</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="7fe49-208">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="7fe49-208">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="7fe49-209">W programie Visual Studio **pliku** menu, wybierz opcję **New** > **projektu**.</span><span class="sxs-lookup"><span data-stu-id="7fe49-209">From the Visual Studio **File** menu, select **New** > **Project**.</span></span>

* <span data-ttu-id="7fe49-210">Utwórz nową aplikację sieci Web ASP.NET Core a następnie wybierz pozycję **dalej**.</span><span class="sxs-lookup"><span data-stu-id="7fe49-210">Create a new ASP.NET Core Web Application and select **Next**.</span></span>

  ![Nowa aplikacja sieci Web ASP.NET Core](razor-pages-start/_static/np_2.1.png)

* <span data-ttu-id="7fe49-212">Nazwij projekt **RazorPagesMovie**.</span><span class="sxs-lookup"><span data-stu-id="7fe49-212">Name the project **RazorPagesMovie**.</span></span> <span data-ttu-id="7fe49-213">Ważne jest, aby nazwa projektu *RazorPagesMovie* , tak aby przestrzenie nazw były zgodne podczas kopiowania i wklejania kodu.</span><span class="sxs-lookup"><span data-stu-id="7fe49-213">It's important to name the project *RazorPagesMovie* so the namespaces will match when you copy and paste code.</span></span>

  ![Nowa aplikacja sieci Web ASP.NET Core](razor-pages-start/_static/config.png)

* <span data-ttu-id="7fe49-215">Wybierz pozycję **ASP.NET Core 2,2** na liście rozwijanej, **aplikacji sieci Web**, a następnie wybierz pozycję **Utwórz**.</span><span class="sxs-lookup"><span data-stu-id="7fe49-215">Select **ASP.NET Core 2.2** in the dropdown, **Web Application**, and then select **Create**.</span></span>

![Nowa aplikacja sieci Web ASP.NET Core](razor-pages-start/_static/np_2_2.2.png)

  <span data-ttu-id="7fe49-217">Tworzony jest następujący projekt początkowy:</span><span class="sxs-lookup"><span data-stu-id="7fe49-217">The following starter project is created:</span></span>

  ![Eksplorator rozwiązań](razor-pages-start/_static/se2.2.png)

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="7fe49-219">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="7fe49-219">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="7fe49-220">Otwórz [zintegrowany terminal](https://code.visualstudio.com/docs/editor/integrated-terminal).</span><span class="sxs-lookup"><span data-stu-id="7fe49-220">Open the [integrated terminal](https://code.visualstudio.com/docs/editor/integrated-terminal).</span></span>

* <span data-ttu-id="7fe49-221">Przejdź do katalogu (`cd`), który będzie zawierać projekt.</span><span class="sxs-lookup"><span data-stu-id="7fe49-221">Change to the directory (`cd`) which will contain the project.</span></span>

* <span data-ttu-id="7fe49-222">Uruchom następujące polecenia:</span><span class="sxs-lookup"><span data-stu-id="7fe49-222">Run the following commands:</span></span>

  ```dotnetcli
  dotnet new webapp -o RazorPagesMovie
  code -r RazorPagesMovie
  ```

  * <span data-ttu-id="7fe49-223">Polecenie tworzy nowy projekt Razor Pages w folderze RazorPagesMovie. `dotnet new`</span><span class="sxs-lookup"><span data-stu-id="7fe49-223">The `dotnet new` command creates a new Razor Pages project in the *RazorPagesMovie* folder.</span></span>
  * <span data-ttu-id="7fe49-224">Polecenie otwiera folder RazorPagesMovie w bieżącym wystąpieniu Visual Studio Code. `code`</span><span class="sxs-lookup"><span data-stu-id="7fe49-224">The `code` command opens the *RazorPagesMovie* folder in the current instance of Visual Studio Code.</span></span>

* <span data-ttu-id="7fe49-225">Gdy ikona płomienia OmniSharp na pasku stanu zmieni kolor na zielony, w **oknie dialogowym zostanie wyświetlony monit o podanie wymaganych zasobów do skompilowania i debugowania z elementu "RazorPagesMovie". Dodać je?**</span><span class="sxs-lookup"><span data-stu-id="7fe49-225">After the status bar's OmniSharp flame icon turns green, a dialog asks **Required assets to build and debug are missing from 'RazorPagesMovie'. Add them?**</span></span> <span data-ttu-id="7fe49-226">Wybierz pozycję **tak**.</span><span class="sxs-lookup"><span data-stu-id="7fe49-226">Select **Yes**.</span></span>

  <span data-ttu-id="7fe49-227">Katalog *. programu vscode* , zawierający pliki *Launch. JSON* i *Tasks. JSON* , jest dodawany do katalogu głównego projektu.</span><span class="sxs-lookup"><span data-stu-id="7fe49-227">A *.vscode* directory, containing *launch.json* and *tasks.json* files, is added to the project's root directory.</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="7fe49-228">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="7fe49-228">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="7fe49-229">W terminalu uruchom następujące polecenie:</span><span class="sxs-lookup"><span data-stu-id="7fe49-229">From a terminal, run the following command:</span></span>

<!-- TODO: update these instruction once mac support 2.2 projects -->

```dotnetcli
dotnet new webapp -o RazorPagesMovie
```

<span data-ttu-id="7fe49-230">Poprzednie polecenia używają [interfejs wiersza polecenia platformy .NET Core](/dotnet/core/tools/dotnet) do tworzenia projektu Razor Pages.</span><span class="sxs-lookup"><span data-stu-id="7fe49-230">The preceding commands use the [.NET Core CLI](/dotnet/core/tools/dotnet) to create a Razor Pages project.</span></span>

## <a name="open-the-project"></a><span data-ttu-id="7fe49-231">Otwórz projekt</span><span class="sxs-lookup"><span data-stu-id="7fe49-231">Open the project</span></span>

<span data-ttu-id="7fe49-232">W programie Visual Studio wybierz pozycję **plik > Otwórz**, a następnie wybierz plik *RazorPagesMovie. csproj* .</span><span class="sxs-lookup"><span data-stu-id="7fe49-232">From Visual Studio, select **File > Open**, and then select the *RazorPagesMovie.csproj* file.</span></span>

<!-- End of VS tabs -->

---

## <a name="run-the-app"></a><span data-ttu-id="7fe49-233">Uruchamianie aplikacji</span><span class="sxs-lookup"><span data-stu-id="7fe49-233">Run the app</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="7fe49-234">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="7fe49-234">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="7fe49-235">Naciśnij klawisze CTRL + F5, aby uruchomić bez debugera.</span><span class="sxs-lookup"><span data-stu-id="7fe49-235">Press Ctrl+F5 to run without the debugger.</span></span>

  [!INCLUDE[](~/includes/trustCertVS.md)]

  <span data-ttu-id="7fe49-236">Program Visual Studio jest uruchamiany [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) i uruchomi aplikację.</span><span class="sxs-lookup"><span data-stu-id="7fe49-236">Visual Studio starts [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) and runs the app.</span></span> <span data-ttu-id="7fe49-237">Na pasku adresu są `localhost:port#` wyświetlane inne elementy, `example.com`takie jak.</span><span class="sxs-lookup"><span data-stu-id="7fe49-237">The address bar shows `localhost:port#` and not something like `example.com`.</span></span> <span data-ttu-id="7fe49-238">Wynika `localhost` to z tego, że jest standardową nazwą hosta dla komputera lokalnego.</span><span class="sxs-lookup"><span data-stu-id="7fe49-238">That's because `localhost` is the standard hostname for the local computer.</span></span> <span data-ttu-id="7fe49-239">Host lokalny obsługuje tylko żądania sieci Web z komputera lokalnego.</span><span class="sxs-lookup"><span data-stu-id="7fe49-239">Localhost only serves web requests from the local computer.</span></span> <span data-ttu-id="7fe49-240">Gdy program Visual Studio tworzy projekt sieci Web, dla serwera sieci Web jest używany port losowy.</span><span class="sxs-lookup"><span data-stu-id="7fe49-240">When Visual Studio creates a web project, a random port is used for the web server.</span></span>

* <span data-ttu-id="7fe49-241">Na stronie głównej aplikacji wybierz pozycję **Akceptuj** , aby wyrazić zgodę na śledzenie.</span><span class="sxs-lookup"><span data-stu-id="7fe49-241">On the app's home page, select **Accept** to consent to tracking.</span></span>

  <span data-ttu-id="7fe49-242">Ta aplikacja nie śledzi informacji osobistych, ale szablon projektu zawiera funkcję zgody na wypadek, gdyby była niezbędna do przestrzegania Ogólne rozporządzenie o ochronie danych Unii Europejskiej [(Rodo)](xref:security/gdpr).</span><span class="sxs-lookup"><span data-stu-id="7fe49-242">This app doesn't track personal information, but the project template includes the consent feature in case you need it to comply with the European Union's [General Data Protection Regulation (GDPR)](xref:security/gdpr).</span></span>

  ![Strona główna lub indeks](razor-pages-start/_static/homeGDPR2.2.png)

  <span data-ttu-id="7fe49-244">Na poniższej ilustracji przedstawiono aplikację po udzieleniu zgody na śledzenie:</span><span class="sxs-lookup"><span data-stu-id="7fe49-244">The following image shows the app after you give consent to tracking:</span></span>

  ![Strona główna lub indeks](razor-pages-start/_static/home2.2.png)
  
# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="7fe49-246">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="7fe49-246">Visual Studio Code</span></span>](#tab/visual-studio-code)

  [!INCLUDE[](~/includes/trustCertVSC.md)]

* <span data-ttu-id="7fe49-247">Naciśnij **klawisze CTRL + F5** , aby uruchomić bez debugera.</span><span class="sxs-lookup"><span data-stu-id="7fe49-247">Press **Ctrl-F5** to run without the debugger.</span></span>

  <span data-ttu-id="7fe49-248">Visual Studio Code uruchamia [Kestrel](xref:fundamentals/servers/kestrel), uruchamia przeglądarkę i nawiguje do `http://localhost:5001`.</span><span class="sxs-lookup"><span data-stu-id="7fe49-248">Visual Studio Code starts [Kestrel](xref:fundamentals/servers/kestrel), launches a browser, and navigates to `http://localhost:5001`.</span></span> <span data-ttu-id="7fe49-249">Na pasku adresu są `localhost:port#` wyświetlane inne elementy, `example.com`takie jak.</span><span class="sxs-lookup"><span data-stu-id="7fe49-249">The address bar shows `localhost:port#` and not something like `example.com`.</span></span> <span data-ttu-id="7fe49-250">Wynika `localhost` to z tego, że jest standardową nazwą hosta dla komputera lokalnego.</span><span class="sxs-lookup"><span data-stu-id="7fe49-250">That's because `localhost` is the standard hostname for  local computer.</span></span> <span data-ttu-id="7fe49-251">Host lokalny obsługuje tylko żądania sieci Web z komputera lokalnego.</span><span class="sxs-lookup"><span data-stu-id="7fe49-251">Localhost only serves web requests from the local computer.</span></span>

* <span data-ttu-id="7fe49-252">Na stronie głównej aplikacji wybierz pozycję **Akceptuj** , aby wyrazić zgodę na śledzenie.</span><span class="sxs-lookup"><span data-stu-id="7fe49-252">On the app's home page, select **Accept** to consent to tracking.</span></span>

  <span data-ttu-id="7fe49-253">Ta aplikacja nie śledzi informacji osobistych, ale szablon projektu zawiera funkcję zgody na wypadek, gdyby była niezbędna do przestrzegania Ogólne rozporządzenie o ochronie danych Unii Europejskiej [(Rodo)](xref:security/gdpr).</span><span class="sxs-lookup"><span data-stu-id="7fe49-253">This app doesn't track personal information, but the project template includes the consent feature in case you need it to comply with the European Union's [General Data Protection Regulation (GDPR)](xref:security/gdpr).</span></span>

  ![Strona główna lub indeks](razor-pages-start/_static/homeGDPR2.2.png)

  <span data-ttu-id="7fe49-255">Na poniższej ilustracji przedstawiono aplikację po udzieleniu zgody na śledzenie:</span><span class="sxs-lookup"><span data-stu-id="7fe49-255">The following image shows the app after you give consent to tracking:</span></span>

  ![Strona główna lub indeks](razor-pages-start/_static/home2.2.png)
  
# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="7fe49-257">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="7fe49-257">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

  [!INCLUDE[](~/includes/trustCertMac.md)]

* <span data-ttu-id="7fe49-258">Naciśnij **polecenie cmd-opt-F5** , aby uruchomić program bez debugera.</span><span class="sxs-lookup"><span data-stu-id="7fe49-258">Press **Cmd-Opt-F5** to run without the debugger.</span></span>

  <span data-ttu-id="7fe49-259">Program Visual Studio uruchamia [Kestrel](xref:fundamentals/servers/kestrel), uruchamia przeglądarkę i przechodzi do `http://localhost:5001`.</span><span class="sxs-lookup"><span data-stu-id="7fe49-259">Visual Studio starts [Kestrel](xref:fundamentals/servers/kestrel), launches a browser, and navigates to `http://localhost:5001`.</span></span>

* <span data-ttu-id="7fe49-260">Na stronie głównej aplikacji wybierz pozycję **Akceptuj** , aby wyrazić zgodę na śledzenie.</span><span class="sxs-lookup"><span data-stu-id="7fe49-260">On the app's home page, select **Accept** to consent to tracking.</span></span>

  <span data-ttu-id="7fe49-261">Ta aplikacja nie śledzi informacji osobistych, ale szablon projektu zawiera funkcję zgody na wypadek, gdyby była niezbędna do przestrzegania Ogólne rozporządzenie o ochronie danych Unii Europejskiej [(Rodo)](xref:security/gdpr).</span><span class="sxs-lookup"><span data-stu-id="7fe49-261">This app doesn't track personal information, but the project template includes the consent feature in case you need it to comply with the European Union's [General Data Protection Regulation (GDPR)](xref:security/gdpr).</span></span>

  ![Strona główna lub indeks](razor-pages-start/_static/homeGDPR2.2_safari.png)

  <span data-ttu-id="7fe49-263">Na poniższej ilustracji przedstawiono aplikację po udzieleniu zgody na śledzenie:</span><span class="sxs-lookup"><span data-stu-id="7fe49-263">The following image shows the app after you give consent to tracking:</span></span>

  ![Strona główna lub indeks](razor-pages-start/_static/home2.2_safari.png)

<!-- End of VS tabs -->

---

## <a name="examine-the-project-files"></a><span data-ttu-id="7fe49-265">Sprawdzanie plików projektu</span><span class="sxs-lookup"><span data-stu-id="7fe49-265">Examine the project files</span></span>

<span data-ttu-id="7fe49-266">Poniżej przedstawiono Omówienie folderów i plików projektu głównego, z których będziesz korzystać w kolejnych samouczkach.</span><span class="sxs-lookup"><span data-stu-id="7fe49-266">Here's an overview of the main project folders and files that you'll work with in later tutorials.</span></span>

### <a name="pages-folder"></a><span data-ttu-id="7fe49-267">Folder stron</span><span class="sxs-lookup"><span data-stu-id="7fe49-267">Pages folder</span></span>

<span data-ttu-id="7fe49-268">Zawiera strony Razor i pliki pomocnicze.</span><span class="sxs-lookup"><span data-stu-id="7fe49-268">Contains Razor pages and supporting files.</span></span> <span data-ttu-id="7fe49-269">Każda Strona Razor to para plików:</span><span class="sxs-lookup"><span data-stu-id="7fe49-269">Each Razor page is a pair of files:</span></span>

* <span data-ttu-id="7fe49-270">Plik *. cshtml* , który zawiera znaczniki HTML z C# kodem przy użyciu składnia Razor.</span><span class="sxs-lookup"><span data-stu-id="7fe49-270">A *.cshtml* file that contains HTML markup with C# code using Razor syntax.</span></span>
* <span data-ttu-id="7fe49-271">Plik *. cshtml.cs* , który zawiera C# kod, który obsługuje zdarzenia strony.</span><span class="sxs-lookup"><span data-stu-id="7fe49-271">A *.cshtml.cs* file that contains C# code that handles page events.</span></span>

<span data-ttu-id="7fe49-272">Pliki pomocnicze mają nazwy zaczynające się od znaku podkreślenia.</span><span class="sxs-lookup"><span data-stu-id="7fe49-272">Supporting files have names that begin with an underscore.</span></span> <span data-ttu-id="7fe49-273">Na przykład plik *_Layout. cshtml* służy do konfigurowania elementów interfejsu użytkownika wspólnych dla wszystkich stron.</span><span class="sxs-lookup"><span data-stu-id="7fe49-273">For example, the *_Layout.cshtml* file configures UI elements common to all pages.</span></span> <span data-ttu-id="7fe49-274">Ten plik konfiguruje menu nawigacji w górnej części strony i informacje o prawach autorskich w dolnej części strony.</span><span class="sxs-lookup"><span data-stu-id="7fe49-274">This file sets up the navigation menu at the top of the page and the copyright notice at the bottom of the page.</span></span> <span data-ttu-id="7fe49-275">Aby uzyskać więcej informacji, zobacz <xref:mvc/views/layout>.</span><span class="sxs-lookup"><span data-stu-id="7fe49-275">For more information, see <xref:mvc/views/layout>.</span></span>

### <a name="wwwroot-folder"></a><span data-ttu-id="7fe49-276">folder wwwroot</span><span class="sxs-lookup"><span data-stu-id="7fe49-276">wwwroot folder</span></span>

<span data-ttu-id="7fe49-277">Zawiera pliki statyczne, takie jak pliki HTML, pliki JavaScript i pliki CSS.</span><span class="sxs-lookup"><span data-stu-id="7fe49-277">Contains static files, such as HTML files, JavaScript files, and CSS files.</span></span> <span data-ttu-id="7fe49-278">Aby uzyskać więcej informacji, zobacz <xref:fundamentals/static-files>.</span><span class="sxs-lookup"><span data-stu-id="7fe49-278">For more information, see <xref:fundamentals/static-files>.</span></span>

### <a name="appsettingsjson"></a><span data-ttu-id="7fe49-279">appSettings.json</span><span class="sxs-lookup"><span data-stu-id="7fe49-279">appSettings.json</span></span>

<span data-ttu-id="7fe49-280">Zawiera dane konfiguracyjne, takie jak parametry połączenia.</span><span class="sxs-lookup"><span data-stu-id="7fe49-280">Contains configuration data, such as connection strings.</span></span> <span data-ttu-id="7fe49-281">Aby uzyskać więcej informacji, zobacz <xref:fundamentals/configuration/index>.</span><span class="sxs-lookup"><span data-stu-id="7fe49-281">For more information, see <xref:fundamentals/configuration/index>.</span></span>

### <a name="programcs"></a><span data-ttu-id="7fe49-282">Program.cs</span><span class="sxs-lookup"><span data-stu-id="7fe49-282">Program.cs</span></span>

<span data-ttu-id="7fe49-283">Zawiera punkt wejścia dla programu.</span><span class="sxs-lookup"><span data-stu-id="7fe49-283">Contains the entry point for the program.</span></span> <span data-ttu-id="7fe49-284">Aby uzyskać więcej informacji, zobacz <xref:fundamentals/host/generic-host>.</span><span class="sxs-lookup"><span data-stu-id="7fe49-284">For more information, see <xref:fundamentals/host/generic-host>.</span></span>

### <a name="startupcs"></a><span data-ttu-id="7fe49-285">Startup.cs</span><span class="sxs-lookup"><span data-stu-id="7fe49-285">Startup.cs</span></span>

<span data-ttu-id="7fe49-286">Zawiera kod, który konfiguruje zachowanie aplikacji, na przykład czy wymaga zgody na pliki cookie.</span><span class="sxs-lookup"><span data-stu-id="7fe49-286">Contains code that configures app behavior, such as whether it requires consent for cookies.</span></span> <span data-ttu-id="7fe49-287">Aby uzyskać więcej informacji, zobacz <xref:fundamentals/startup>.</span><span class="sxs-lookup"><span data-stu-id="7fe49-287">For more information, see <xref:fundamentals/startup>.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="7fe49-288">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="7fe49-288">Additional resources</span></span>

* [<span data-ttu-id="7fe49-289">Wersja tego samouczka usługi YouTube</span><span class="sxs-lookup"><span data-stu-id="7fe49-289">Youtube version of this tutorial</span></span>](https://www.youtube.com/watch?v=F0SP7Ry4flQ&feature=youtu.be)

## <a name="next-steps"></a><span data-ttu-id="7fe49-290">Następne kroki</span><span class="sxs-lookup"><span data-stu-id="7fe49-290">Next steps</span></span>

<span data-ttu-id="7fe49-291">Przejdź do następnego samouczka z serii:</span><span class="sxs-lookup"><span data-stu-id="7fe49-291">Advance to the next tutorial in the series:</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="7fe49-292">Dodawanie modelu</span><span class="sxs-lookup"><span data-stu-id="7fe49-292">Add a model</span></span>](xref:tutorials/razor-pages/model)

::: moniker-end
