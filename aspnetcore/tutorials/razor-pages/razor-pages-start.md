---
title: 'Samouczek: Wprowadzenie do Razor Pages w ASP.NET Core'
author: rick-anderson
description: W tej serii samouczków pokazano, jak używać Razor Pages w ASP.NET Core. Dowiedz się, jak utworzyć model, wygenerować kod dla stron Razor, użyć Entity Framework Core i SQL Server na potrzeby dostępu do danych, dodać funkcję wyszukiwania, dodać sprawdzanie poprawności danych wejściowych i użyć migracji w celu zaktualizowania modelu.
ms.author: riande
ms.date: 07/25/2019
uid: tutorials/razor-pages/razor-pages-start
ms.openlocfilehash: 57a10895c718c539ece280afcb27cb4033c7fb45
ms.sourcegitcommit: 979dbfc5e9ce09b9470789989cddfcfb57079d94
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/31/2019
ms.locfileid: "68682792"
---
# <a name="tutorial-get-started-with-razor-pages-in-aspnet-core"></a><span data-ttu-id="7793c-104">Samouczek: Wprowadzenie do Razor Pages w ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="7793c-104">Tutorial: Get started with Razor Pages in ASP.NET Core</span></span>

<span data-ttu-id="7793c-105">Przez [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="7793c-105">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

::: moniker range=">= aspnetcore-3.0"
<span data-ttu-id="7793c-106">Jest to pierwszy samouczek dotyczący serii, który uczy się podstaw tworzenia aplikacji sieci Web Razor Pages ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="7793c-106">This is the first tutorial of a series that teaches the basics of building an ASP.NET Core Razor Pages web app.</span></span>

[!INCLUDE[](~/includes/advancedRP.md)]

<span data-ttu-id="7793c-107">Na końcu serii będziesz mieć aplikację, która zarządza bazą danych filmów.</span><span class="sxs-lookup"><span data-stu-id="7793c-107">At the end of the series, you'll have an app that manages a database of movies.</span></span>  

[!INCLUDE[View or download sample code](~/includes/rp/download.md)]

<span data-ttu-id="7793c-108">W tym samouczku przedstawiono następujące instrukcje:</span><span class="sxs-lookup"><span data-stu-id="7793c-108">In this tutorial, you:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="7793c-109">Utwórz aplikację sieci Web Razor Pages.</span><span class="sxs-lookup"><span data-stu-id="7793c-109">Create a Razor Pages web app.</span></span>
> * <span data-ttu-id="7793c-110">Uruchom aplikację.</span><span class="sxs-lookup"><span data-stu-id="7793c-110">Run the app.</span></span>
> * <span data-ttu-id="7793c-111">Przejrzyj pliki projektu.</span><span class="sxs-lookup"><span data-stu-id="7793c-111">Examine the project files.</span></span>

<span data-ttu-id="7793c-112">Na końcu tego samouczka będziesz mieć działającą Razor Pagesową aplikację internetową, która zostanie wdrożona w kolejnych samouczkach.</span><span class="sxs-lookup"><span data-stu-id="7793c-112">At the end of this tutorial, you'll have a working Razor Pages web app that you'll build on in later tutorials.</span></span>

![Strona główna lub indeks](razor-pages-start/_static/home2.2.png)

## <a name="prerequisites"></a><span data-ttu-id="7793c-114">Wymagania wstępne</span><span class="sxs-lookup"><span data-stu-id="7793c-114">Prerequisites</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="7793c-115">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="7793c-115">Visual Studio</span></span>](#tab/visual-studio)

[!INCLUDE[](~/includes/net-core-prereqs-vs-3.0.md)]

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="7793c-116">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="7793c-116">Visual Studio Code</span></span>](#tab/visual-studio-code)

[!INCLUDE[](~/includes/net-core-prereqs-vsc-3.0.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="7793c-117">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="7793c-117">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

[!INCLUDE[](~/includes/net-core-prereqs-mac-3.0.md)]

---

## <a name="create-a-razor-pages-web-app"></a><span data-ttu-id="7793c-118">Tworzenie aplikacji sieci Web Razor Pages</span><span class="sxs-lookup"><span data-stu-id="7793c-118">Create a Razor Pages web app</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="7793c-119">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="7793c-119">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="7793c-120">W programie Visual Studio **pliku** menu, wybierz opcję **New** > **projektu**.</span><span class="sxs-lookup"><span data-stu-id="7793c-120">From the Visual Studio **File** menu, select **New** > **Project**.</span></span>
* <span data-ttu-id="7793c-121">Utwórz nową aplikację sieci Web ASP.NET Core a następnie wybierz pozycję **dalej**.</span><span class="sxs-lookup"><span data-stu-id="7793c-121">Create a new ASP.NET Core Web Application and select **Next**.</span></span>
  <span data-ttu-id="7793c-122">![Nowa aplikacja sieci Web ASP.NET Core](razor-pages-start/_static/np_2.1.png)</span><span class="sxs-lookup"><span data-stu-id="7793c-122">![new ASP.NET Core Web Application](razor-pages-start/_static/np_2.1.png)</span></span>
* <span data-ttu-id="7793c-123">Nazwij projekt **RazorPagesMovie**.</span><span class="sxs-lookup"><span data-stu-id="7793c-123">Name the project **RazorPagesMovie**.</span></span> <span data-ttu-id="7793c-124">Ważne jest, aby nazwa projektu *RazorPagesMovie* , tak aby przestrzenie nazw były zgodne podczas kopiowania i wklejania kodu.</span><span class="sxs-lookup"><span data-stu-id="7793c-124">It's important to name the project *RazorPagesMovie* so the namespaces will match when you copy and paste code.</span></span>
  <span data-ttu-id="7793c-125">![Nowa aplikacja sieci Web ASP.NET Core](razor-pages-start/_static/config.png)</span><span class="sxs-lookup"><span data-stu-id="7793c-125">![new ASP.NET Core Web Application](razor-pages-start/_static/config.png)</span></span>

* <span data-ttu-id="7793c-126">Wybierz pozycję **ASP.NET Core 3,0** na liście rozwijanej, **aplikacji sieci Web**, a następnie wybierz pozycję **Utwórz**.</span><span class="sxs-lookup"><span data-stu-id="7793c-126">Select **ASP.NET Core 3.0** in the dropdown, **Web Application**, and then select **Create**.</span></span>

![Nowa aplikacja sieci Web ASP.NET Core](razor-pages-start/_static/3/npx.png)

  <span data-ttu-id="7793c-128">Tworzony jest następujący projekt początkowy:</span><span class="sxs-lookup"><span data-stu-id="7793c-128">The following starter project is created:</span></span>

  ![Eksplorator rozwiązań](razor-pages-start/_static/se2.2.png)

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="7793c-130">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="7793c-130">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="7793c-131">Otwórz [zintegrowany terminal](https://code.visualstudio.com/docs/editor/integrated-terminal).</span><span class="sxs-lookup"><span data-stu-id="7793c-131">Open the [integrated terminal](https://code.visualstudio.com/docs/editor/integrated-terminal).</span></span>

* <span data-ttu-id="7793c-132">Przejdź do katalogu (`cd`), który będzie zawierać projekt.</span><span class="sxs-lookup"><span data-stu-id="7793c-132">Change to the directory (`cd`) which will contain the project.</span></span>

* <span data-ttu-id="7793c-133">Uruchom następujące polecenia:</span><span class="sxs-lookup"><span data-stu-id="7793c-133">Run the following commands:</span></span>

  ```console
  dotnet new webapp -o RazorPagesMovie
  code -r RazorPagesMovie
  ```

  * <span data-ttu-id="7793c-134">Polecenie tworzy nowy projekt Razor Pages w folderze RazorPagesMovie. `dotnet new`</span><span class="sxs-lookup"><span data-stu-id="7793c-134">The `dotnet new` command creates a new Razor Pages project in the *RazorPagesMovie* folder.</span></span>
  * <span data-ttu-id="7793c-135">Polecenie otwiera folder RazorPagesMovie w bieżącym wystąpieniu Visual Studio Code. `code`</span><span class="sxs-lookup"><span data-stu-id="7793c-135">The `code` command opens the *RazorPagesMovie* folder in the current instance of Visual Studio Code.</span></span>

* <span data-ttu-id="7793c-136">Gdy ikona płomienia OmniSharp na pasku stanu zmieni kolor na zielony, w **oknie dialogowym zostanie wyświetlony monit o podanie wymaganych zasobów do skompilowania i debugowania z elementu "RazorPagesMovie". Dodać je?**</span><span class="sxs-lookup"><span data-stu-id="7793c-136">After the status bar's OmniSharp flame icon turns green, a dialog asks **Required assets to build and debug are missing from 'RazorPagesMovie'. Add them?**</span></span> <span data-ttu-id="7793c-137">Wybierz pozycję **tak**.</span><span class="sxs-lookup"><span data-stu-id="7793c-137">Select **Yes**.</span></span>

  <span data-ttu-id="7793c-138">Katalog *. programu vscode* , zawierający pliki *Launch. JSON* i *Tasks. JSON* , jest dodawany do katalogu głównego projektu.</span><span class="sxs-lookup"><span data-stu-id="7793c-138">A *.vscode* directory, containing *launch.json* and *tasks.json* files, is added to the project's root directory.</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="7793c-139">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="7793c-139">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="7793c-140">W terminalu uruchom następujące polecenie:</span><span class="sxs-lookup"><span data-stu-id="7793c-140">From a terminal, run the following command:</span></span>

<!-- TODO: update these instruction once mac support 2.2 projects -->

```console
dotnet new webapp -o RazorPagesMovie
```

<span data-ttu-id="7793c-141">Poprzednie polecenia używają [interfejs wiersza polecenia platformy .NET Core](/dotnet/core/tools/dotnet) do tworzenia projektu Razor Pages.</span><span class="sxs-lookup"><span data-stu-id="7793c-141">The preceding commands use the [.NET Core CLI](/dotnet/core/tools/dotnet) to create a Razor Pages project.</span></span>

## <a name="open-the-project"></a><span data-ttu-id="7793c-142">Otwórz projekt</span><span class="sxs-lookup"><span data-stu-id="7793c-142">Open the project</span></span>

<span data-ttu-id="7793c-143">W programie Visual Studio wybierz pozycję **plik > Otwórz**, a następnie wybierz plik *RazorPagesMovie. csproj* .</span><span class="sxs-lookup"><span data-stu-id="7793c-143">From Visual Studio, select **File > Open**, and then select the *RazorPagesMovie.csproj* file.</span></span>

<!-- End of VS tabs -->

---

## <a name="run-the-app"></a><span data-ttu-id="7793c-144">Uruchamianie aplikacji</span><span class="sxs-lookup"><span data-stu-id="7793c-144">Run the app</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="7793c-145">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="7793c-145">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="7793c-146">Naciśnij klawisze CTRL + F5, aby uruchomić bez debugera.</span><span class="sxs-lookup"><span data-stu-id="7793c-146">Press Ctrl+F5 to run without the debugger.</span></span>

  [!INCLUDE[](~/includes/trustCertVS.md)]

  <span data-ttu-id="7793c-147">Program Visual Studio jest uruchamiany [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) i uruchomi aplikację.</span><span class="sxs-lookup"><span data-stu-id="7793c-147">Visual Studio starts [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) and runs the app.</span></span> <span data-ttu-id="7793c-148">Na pasku adresu są `localhost:port#` wyświetlane inne elementy, `example.com`takie jak.</span><span class="sxs-lookup"><span data-stu-id="7793c-148">The address bar shows `localhost:port#` and not something like `example.com`.</span></span> <span data-ttu-id="7793c-149">Wynika `localhost` to z tego, że jest standardową nazwą hosta dla komputera lokalnego.</span><span class="sxs-lookup"><span data-stu-id="7793c-149">That's because `localhost` is the standard hostname for the local computer.</span></span> <span data-ttu-id="7793c-150">Host lokalny obsługuje tylko żądania sieci Web z komputera lokalnego.</span><span class="sxs-lookup"><span data-stu-id="7793c-150">Localhost only serves web requests from the local computer.</span></span> <span data-ttu-id="7793c-151">Gdy program Visual Studio tworzy projekt sieci Web, dla serwera sieci Web jest używany port losowy.</span><span class="sxs-lookup"><span data-stu-id="7793c-151">When Visual Studio creates a web project, a random port is used for the web server.</span></span>
 
# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="7793c-152">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="7793c-152">Visual Studio Code</span></span>](#tab/visual-studio-code)

  [!INCLUDE[](~/includes/trustCertVSC.md)]

* <span data-ttu-id="7793c-153">Naciśnij **klawisze CTRL + F5** , aby uruchomić bez debugera.</span><span class="sxs-lookup"><span data-stu-id="7793c-153">Press **Ctrl-F5** to run without the debugger.</span></span>

  <span data-ttu-id="7793c-154">Visual Studio Code uruchamia [Kestrel](xref:fundamentals/servers/kestrel), uruchamia przeglądarkę i nawiguje do `http://localhost:5001`.</span><span class="sxs-lookup"><span data-stu-id="7793c-154">Visual Studio Code starts [Kestrel](xref:fundamentals/servers/kestrel), launches a browser, and navigates to `http://localhost:5001`.</span></span> <span data-ttu-id="7793c-155">Na pasku adresu są `localhost:port#` wyświetlane inne elementy, `example.com`takie jak.</span><span class="sxs-lookup"><span data-stu-id="7793c-155">The address bar shows `localhost:port#` and not something like `example.com`.</span></span> <span data-ttu-id="7793c-156">Wynika `localhost` to z tego, że jest standardową nazwą hosta dla komputera lokalnego.</span><span class="sxs-lookup"><span data-stu-id="7793c-156">That's because `localhost` is the standard hostname for  local computer.</span></span> <span data-ttu-id="7793c-157">Host lokalny obsługuje tylko żądania sieci Web z komputera lokalnego.</span><span class="sxs-lookup"><span data-stu-id="7793c-157">Localhost only serves web requests from the local computer.</span></span>

  
# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="7793c-158">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="7793c-158">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

  [!INCLUDE[](~/includes/trustCertMac.md)]

* <span data-ttu-id="7793c-159">Naciśnij klawisze **Alt-cmd-Enter** , aby uruchomić bez debugera.</span><span class="sxs-lookup"><span data-stu-id="7793c-159">Press **Alt-Cmd-Enter** to run without the debugger.</span></span> <span data-ttu-id="7793c-160">Alternatywnie przejdź do paska menu i przejdź do pozycji Uruchom > Uruchom bez debugowania.</span><span class="sxs-lookup"><span data-stu-id="7793c-160">Alternatively, navigate to the menu bar and go to Run>Start Without Debugging.</span></span>

  <span data-ttu-id="7793c-161">Program Visual Studio uruchamia [Kestrel](xref:fundamentals/servers/kestrel), uruchamia przeglądarkę i przechodzi do `http://localhost:5001`.</span><span class="sxs-lookup"><span data-stu-id="7793c-161">Visual Studio starts [Kestrel](xref:fundamentals/servers/kestrel), launches a browser, and navigates to `http://localhost:5001`.</span></span>

<!-- End of VS tabs -->

---

## <a name="examine-the-project-files"></a><span data-ttu-id="7793c-162">Sprawdzanie plików projektu</span><span class="sxs-lookup"><span data-stu-id="7793c-162">Examine the project files</span></span>

<span data-ttu-id="7793c-163">Poniżej przedstawiono Omówienie folderów i plików projektu głównego, z których będziesz korzystać w kolejnych samouczkach.</span><span class="sxs-lookup"><span data-stu-id="7793c-163">Here's an overview of the main project folders and files that you'll work with in later tutorials.</span></span>

### <a name="pages-folder"></a><span data-ttu-id="7793c-164">Folder stron</span><span class="sxs-lookup"><span data-stu-id="7793c-164">Pages folder</span></span>

<span data-ttu-id="7793c-165">Zawiera strony Razor i pliki pomocnicze.</span><span class="sxs-lookup"><span data-stu-id="7793c-165">Contains Razor pages and supporting files.</span></span> <span data-ttu-id="7793c-166">Każda Strona Razor to para plików:</span><span class="sxs-lookup"><span data-stu-id="7793c-166">Each Razor page is a pair of files:</span></span>

* <span data-ttu-id="7793c-167">Plik *. cshtml* , który zawiera znaczniki HTML z C# kodem przy użyciu składnia Razor.</span><span class="sxs-lookup"><span data-stu-id="7793c-167">A *.cshtml* file that contains HTML markup with C# code using Razor syntax.</span></span>
* <span data-ttu-id="7793c-168">Plik *. cshtml.cs* , który zawiera C# kod, który obsługuje zdarzenia strony.</span><span class="sxs-lookup"><span data-stu-id="7793c-168">A *.cshtml.cs* file that contains C# code that handles page events.</span></span>

<span data-ttu-id="7793c-169">Pliki pomocnicze mają nazwy zaczynające się od znaku podkreślenia.</span><span class="sxs-lookup"><span data-stu-id="7793c-169">Supporting files have names that begin with an underscore.</span></span> <span data-ttu-id="7793c-170">Na przykład plik *_Layout. cshtml* służy do konfigurowania elementów interfejsu użytkownika wspólnych dla wszystkich stron.</span><span class="sxs-lookup"><span data-stu-id="7793c-170">For example, the *_Layout.cshtml* file configures UI elements common to all pages.</span></span> <span data-ttu-id="7793c-171">Ten plik konfiguruje menu nawigacji w górnej części strony i informacje o prawach autorskich w dolnej części strony.</span><span class="sxs-lookup"><span data-stu-id="7793c-171">This file sets up the navigation menu at the top of the page and the copyright notice at the bottom of the page.</span></span> <span data-ttu-id="7793c-172">Aby uzyskać więcej informacji, zobacz <xref:mvc/views/layout>.</span><span class="sxs-lookup"><span data-stu-id="7793c-172">For more information, see <xref:mvc/views/layout>.</span></span>

### <a name="wwwroot-folder"></a><span data-ttu-id="7793c-173">folder wwwroot</span><span class="sxs-lookup"><span data-stu-id="7793c-173">wwwroot folder</span></span>

<span data-ttu-id="7793c-174">Zawiera pliki statyczne, takie jak pliki HTML, pliki JavaScript i pliki CSS.</span><span class="sxs-lookup"><span data-stu-id="7793c-174">Contains static files, such as HTML files, JavaScript files, and CSS files.</span></span> <span data-ttu-id="7793c-175">Aby uzyskać więcej informacji, zobacz <xref:fundamentals/static-files>.</span><span class="sxs-lookup"><span data-stu-id="7793c-175">For more information, see <xref:fundamentals/static-files>.</span></span>

### <a name="appsettingsjson"></a><span data-ttu-id="7793c-176">appSettings.json</span><span class="sxs-lookup"><span data-stu-id="7793c-176">appSettings.json</span></span>

<span data-ttu-id="7793c-177">Zawiera dane konfiguracyjne, takie jak parametry połączenia.</span><span class="sxs-lookup"><span data-stu-id="7793c-177">Contains configuration data, such as connection strings.</span></span> <span data-ttu-id="7793c-178">Aby uzyskać więcej informacji, zobacz <xref:fundamentals/configuration/index>.</span><span class="sxs-lookup"><span data-stu-id="7793c-178">For more information, see <xref:fundamentals/configuration/index>.</span></span>

### <a name="programcs"></a><span data-ttu-id="7793c-179">Program.cs</span><span class="sxs-lookup"><span data-stu-id="7793c-179">Program.cs</span></span>

<span data-ttu-id="7793c-180">Zawiera punkt wejścia dla programu.</span><span class="sxs-lookup"><span data-stu-id="7793c-180">Contains the entry point for the program.</span></span> <span data-ttu-id="7793c-181">Aby uzyskać więcej informacji, zobacz <xref:fundamentals/host/generic-host>.</span><span class="sxs-lookup"><span data-stu-id="7793c-181">For more information, see <xref:fundamentals/host/generic-host>.</span></span>

### <a name="startupcs"></a><span data-ttu-id="7793c-182">Startup.cs</span><span class="sxs-lookup"><span data-stu-id="7793c-182">Startup.cs</span></span>

<span data-ttu-id="7793c-183">Zawiera kod, który konfiguruje zachowanie aplikacji.</span><span class="sxs-lookup"><span data-stu-id="7793c-183">Contains code that configures app behavior.</span></span> <span data-ttu-id="7793c-184">Aby uzyskać więcej informacji, zobacz <xref:fundamentals/startup>.</span><span class="sxs-lookup"><span data-stu-id="7793c-184">For more information, see <xref:fundamentals/startup>.</span></span>

## <a name="next-steps"></a><span data-ttu-id="7793c-185">Następne kroki</span><span class="sxs-lookup"><span data-stu-id="7793c-185">Next steps</span></span>

<span data-ttu-id="7793c-186">Przejdź do następnego samouczka z serii:</span><span class="sxs-lookup"><span data-stu-id="7793c-186">Advance to the next tutorial in the series:</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="7793c-187">Dodawanie modelu</span><span class="sxs-lookup"><span data-stu-id="7793c-187">Add a model</span></span>](xref:tutorials/razor-pages/model)

::: moniker-end

<!--::: moniker range=">= aspnetcore-3.0" -->

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="7793c-188">Jest to pierwszy samouczek dotyczący serii.</span><span class="sxs-lookup"><span data-stu-id="7793c-188">This is the first tutorial of a series.</span></span> <span data-ttu-id="7793c-189">[Seria zawiera](xref:tutorials/razor-pages/index) podstawowe informacje na temat tworzenia aplikacji sieci web ASP.NET Core Razor Pages.</span><span class="sxs-lookup"><span data-stu-id="7793c-189">[The series](xref:tutorials/razor-pages/index) teaches the basics of building an ASP.NET Core Razor Pages web app.</span></span>

[!INCLUDE[](~/includes/advancedRP.md)]

<span data-ttu-id="7793c-190">Na końcu serii będziesz mieć aplikację, która zarządza bazą danych filmów.</span><span class="sxs-lookup"><span data-stu-id="7793c-190">At the end of the series, you'll have an app that manages a database of movies.</span></span>  

[!INCLUDE[View or download sample code](~/includes/rp/download.md)]

<span data-ttu-id="7793c-191">W tym samouczku przedstawiono następujące instrukcje:</span><span class="sxs-lookup"><span data-stu-id="7793c-191">In this tutorial, you:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="7793c-192">Utwórz aplikację sieci Web Razor Pages.</span><span class="sxs-lookup"><span data-stu-id="7793c-192">Create a Razor Pages web app.</span></span>
> * <span data-ttu-id="7793c-193">Uruchom aplikację.</span><span class="sxs-lookup"><span data-stu-id="7793c-193">Run the app.</span></span>
> * <span data-ttu-id="7793c-194">Przejrzyj pliki projektu.</span><span class="sxs-lookup"><span data-stu-id="7793c-194">Examine the project files.</span></span>

<span data-ttu-id="7793c-195">Na końcu tego samouczka będziesz mieć działającą Razor Pagesową aplikację internetową, która zostanie wdrożona w kolejnych samouczkach.</span><span class="sxs-lookup"><span data-stu-id="7793c-195">At the end of this tutorial, you'll have a working Razor Pages web app that you'll build on in later tutorials.</span></span>

![Strona główna lub indeks](razor-pages-start/_static/home2.2.png)

## <a name="prerequisites"></a><span data-ttu-id="7793c-197">Wymagania wstępne</span><span class="sxs-lookup"><span data-stu-id="7793c-197">Prerequisites</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="7793c-198">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="7793c-198">Visual Studio</span></span>](#tab/visual-studio)

[!INCLUDE[](~/includes/net-core-prereqs-vs2019-2.2.md)]

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="7793c-199">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="7793c-199">Visual Studio Code</span></span>](#tab/visual-studio-code)

[!INCLUDE[](~/includes/net-core-prereqs-vsc-2.2.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="7793c-200">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="7793c-200">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

[!INCLUDE[](~/includes/net-core-prereqs-mac-2.2.md)]

---

## <a name="create-a-razor-pages-web-app"></a><span data-ttu-id="7793c-201">Tworzenie aplikacji sieci Web Razor Pages</span><span class="sxs-lookup"><span data-stu-id="7793c-201">Create a Razor Pages web app</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="7793c-202">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="7793c-202">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="7793c-203">W programie Visual Studio **pliku** menu, wybierz opcję **New** > **projektu**.</span><span class="sxs-lookup"><span data-stu-id="7793c-203">From the Visual Studio **File** menu, select **New** > **Project**.</span></span>

* <span data-ttu-id="7793c-204">Utwórz nową aplikację sieci Web ASP.NET Core a następnie wybierz pozycję **dalej**.</span><span class="sxs-lookup"><span data-stu-id="7793c-204">Create a new ASP.NET Core Web Application and select **Next**.</span></span>

  ![Nowa aplikacja sieci Web ASP.NET Core](razor-pages-start/_static/np_2.1.png)

* <span data-ttu-id="7793c-206">Nazwij projekt **RazorPagesMovie**.</span><span class="sxs-lookup"><span data-stu-id="7793c-206">Name the project **RazorPagesMovie**.</span></span> <span data-ttu-id="7793c-207">Ważne jest, aby nazwa projektu *RazorPagesMovie* , tak aby przestrzenie nazw były zgodne podczas kopiowania i wklejania kodu.</span><span class="sxs-lookup"><span data-stu-id="7793c-207">It's important to name the project *RazorPagesMovie* so the namespaces will match when you copy and paste code.</span></span>

  ![Nowa aplikacja sieci Web ASP.NET Core](razor-pages-start/_static/config.png)

* <span data-ttu-id="7793c-209">Wybierz pozycję **ASP.NET Core 2,2** na liście rozwijanej, **aplikacji sieci Web**, a następnie wybierz pozycję **Utwórz**.</span><span class="sxs-lookup"><span data-stu-id="7793c-209">Select **ASP.NET Core 2.2** in the dropdown, **Web Application**, and then select **Create**.</span></span>

![Nowa aplikacja sieci Web ASP.NET Core](razor-pages-start/_static/np_2_2.2.png)

  <span data-ttu-id="7793c-211">Tworzony jest następujący projekt początkowy:</span><span class="sxs-lookup"><span data-stu-id="7793c-211">The following starter project is created:</span></span>

  ![Eksplorator rozwiązań](razor-pages-start/_static/se2.2.png)

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="7793c-213">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="7793c-213">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="7793c-214">Otwórz [zintegrowany terminal](https://code.visualstudio.com/docs/editor/integrated-terminal).</span><span class="sxs-lookup"><span data-stu-id="7793c-214">Open the [integrated terminal](https://code.visualstudio.com/docs/editor/integrated-terminal).</span></span>

* <span data-ttu-id="7793c-215">Przejdź do katalogu (`cd`), który będzie zawierać projekt.</span><span class="sxs-lookup"><span data-stu-id="7793c-215">Change to the directory (`cd`) which will contain the project.</span></span>

* <span data-ttu-id="7793c-216">Uruchom następujące polecenia:</span><span class="sxs-lookup"><span data-stu-id="7793c-216">Run the following commands:</span></span>

  ```console
  dotnet new webapp -o RazorPagesMovie
  code -r RazorPagesMovie
  ```

  * <span data-ttu-id="7793c-217">Polecenie tworzy nowy projekt Razor Pages w folderze RazorPagesMovie. `dotnet new`</span><span class="sxs-lookup"><span data-stu-id="7793c-217">The `dotnet new` command creates a new Razor Pages project in the *RazorPagesMovie* folder.</span></span>
  * <span data-ttu-id="7793c-218">Polecenie otwiera folder RazorPagesMovie w bieżącym wystąpieniu Visual Studio Code. `code`</span><span class="sxs-lookup"><span data-stu-id="7793c-218">The `code` command opens the *RazorPagesMovie* folder in the current instance of Visual Studio Code.</span></span>

* <span data-ttu-id="7793c-219">Gdy ikona płomienia OmniSharp na pasku stanu zmieni kolor na zielony, w **oknie dialogowym zostanie wyświetlony monit o podanie wymaganych zasobów do skompilowania i debugowania z elementu "RazorPagesMovie". Dodać je?**</span><span class="sxs-lookup"><span data-stu-id="7793c-219">After the status bar's OmniSharp flame icon turns green, a dialog asks **Required assets to build and debug are missing from 'RazorPagesMovie'. Add them?**</span></span> <span data-ttu-id="7793c-220">Wybierz pozycję **tak**.</span><span class="sxs-lookup"><span data-stu-id="7793c-220">Select **Yes**.</span></span>

  <span data-ttu-id="7793c-221">Katalog *. programu vscode* , zawierający pliki *Launch. JSON* i *Tasks. JSON* , jest dodawany do katalogu głównego projektu.</span><span class="sxs-lookup"><span data-stu-id="7793c-221">A *.vscode* directory, containing *launch.json* and *tasks.json* files, is added to the project's root directory.</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="7793c-222">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="7793c-222">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="7793c-223">W terminalu uruchom następujące polecenie:</span><span class="sxs-lookup"><span data-stu-id="7793c-223">From a terminal, run the following command:</span></span>

<!-- TODO: update these instruction once mac support 2.2 projects -->

```console
dotnet new webapp -o RazorPagesMovie
```

<span data-ttu-id="7793c-224">Poprzednie polecenia używają [interfejs wiersza polecenia platformy .NET Core](/dotnet/core/tools/dotnet) do tworzenia projektu Razor Pages.</span><span class="sxs-lookup"><span data-stu-id="7793c-224">The preceding commands use the [.NET Core CLI](/dotnet/core/tools/dotnet) to create a Razor Pages project.</span></span>

## <a name="open-the-project"></a><span data-ttu-id="7793c-225">Otwórz projekt</span><span class="sxs-lookup"><span data-stu-id="7793c-225">Open the project</span></span>

<span data-ttu-id="7793c-226">W programie Visual Studio wybierz pozycję **plik > Otwórz**, a następnie wybierz plik *RazorPagesMovie. csproj* .</span><span class="sxs-lookup"><span data-stu-id="7793c-226">From Visual Studio, select **File > Open**, and then select the *RazorPagesMovie.csproj* file.</span></span>

<!-- End of VS tabs -->

---

## <a name="run-the-app"></a><span data-ttu-id="7793c-227">Uruchamianie aplikacji</span><span class="sxs-lookup"><span data-stu-id="7793c-227">Run the app</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="7793c-228">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="7793c-228">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="7793c-229">Naciśnij klawisze CTRL + F5, aby uruchomić bez debugera.</span><span class="sxs-lookup"><span data-stu-id="7793c-229">Press Ctrl+F5 to run without the debugger.</span></span>

  [!INCLUDE[](~/includes/trustCertVS.md)]

  <span data-ttu-id="7793c-230">Program Visual Studio jest uruchamiany [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) i uruchomi aplikację.</span><span class="sxs-lookup"><span data-stu-id="7793c-230">Visual Studio starts [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) and runs the app.</span></span> <span data-ttu-id="7793c-231">Na pasku adresu są `localhost:port#` wyświetlane inne elementy, `example.com`takie jak.</span><span class="sxs-lookup"><span data-stu-id="7793c-231">The address bar shows `localhost:port#` and not something like `example.com`.</span></span> <span data-ttu-id="7793c-232">Wynika `localhost` to z tego, że jest standardową nazwą hosta dla komputera lokalnego.</span><span class="sxs-lookup"><span data-stu-id="7793c-232">That's because `localhost` is the standard hostname for the local computer.</span></span> <span data-ttu-id="7793c-233">Host lokalny obsługuje tylko żądania sieci Web z komputera lokalnego.</span><span class="sxs-lookup"><span data-stu-id="7793c-233">Localhost only serves web requests from the local computer.</span></span> <span data-ttu-id="7793c-234">Gdy program Visual Studio tworzy projekt sieci Web, dla serwera sieci Web jest używany port losowy.</span><span class="sxs-lookup"><span data-stu-id="7793c-234">When Visual Studio creates a web project, a random port is used for the web server.</span></span>

* <span data-ttu-id="7793c-235">Na stronie głównej aplikacji wybierz pozycję **Akceptuj** , aby wyrazić zgodę na śledzenie.</span><span class="sxs-lookup"><span data-stu-id="7793c-235">On the app's home page, select **Accept** to consent to tracking.</span></span>

  <span data-ttu-id="7793c-236">Ta aplikacja nie śledzi informacji osobistych, ale szablon projektu zawiera funkcję zgody na wypadek, gdyby była niezbędna do przestrzegania Ogólne rozporządzenie o ochronie danych Unii Europejskiej [(Rodo)](xref:security/gdpr).</span><span class="sxs-lookup"><span data-stu-id="7793c-236">This app doesn't track personal information, but the project template includes the consent feature in case you need it to comply with the European Union's [General Data Protection Regulation (GDPR)](xref:security/gdpr).</span></span>

  ![Strona główna lub indeks](razor-pages-start/_static/homeGDPR2.2.png)

  <span data-ttu-id="7793c-238">Na poniższej ilustracji przedstawiono aplikację po udzieleniu zgody na śledzenie:</span><span class="sxs-lookup"><span data-stu-id="7793c-238">The following image shows the app after you give consent to tracking:</span></span>

  ![Strona główna lub indeks](razor-pages-start/_static/home2.2.png)
  
# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="7793c-240">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="7793c-240">Visual Studio Code</span></span>](#tab/visual-studio-code)

  [!INCLUDE[](~/includes/trustCertVSC.md)]

* <span data-ttu-id="7793c-241">Naciśnij **klawisze CTRL + F5** , aby uruchomić bez debugera.</span><span class="sxs-lookup"><span data-stu-id="7793c-241">Press **Ctrl-F5** to run without the debugger.</span></span>

  <span data-ttu-id="7793c-242">Visual Studio Code uruchamia [Kestrel](xref:fundamentals/servers/kestrel), uruchamia przeglądarkę i nawiguje do `http://localhost:5001`.</span><span class="sxs-lookup"><span data-stu-id="7793c-242">Visual Studio Code starts [Kestrel](xref:fundamentals/servers/kestrel), launches a browser, and navigates to `http://localhost:5001`.</span></span> <span data-ttu-id="7793c-243">Na pasku adresu są `localhost:port#` wyświetlane inne elementy, `example.com`takie jak.</span><span class="sxs-lookup"><span data-stu-id="7793c-243">The address bar shows `localhost:port#` and not something like `example.com`.</span></span> <span data-ttu-id="7793c-244">Wynika `localhost` to z tego, że jest standardową nazwą hosta dla komputera lokalnego.</span><span class="sxs-lookup"><span data-stu-id="7793c-244">That's because `localhost` is the standard hostname for  local computer.</span></span> <span data-ttu-id="7793c-245">Host lokalny obsługuje tylko żądania sieci Web z komputera lokalnego.</span><span class="sxs-lookup"><span data-stu-id="7793c-245">Localhost only serves web requests from the local computer.</span></span>

* <span data-ttu-id="7793c-246">Na stronie głównej aplikacji wybierz pozycję **Akceptuj** , aby wyrazić zgodę na śledzenie.</span><span class="sxs-lookup"><span data-stu-id="7793c-246">On the app's home page, select **Accept** to consent to tracking.</span></span>

  <span data-ttu-id="7793c-247">Ta aplikacja nie śledzi informacji osobistych, ale szablon projektu zawiera funkcję zgody na wypadek, gdyby była niezbędna do przestrzegania Ogólne rozporządzenie o ochronie danych Unii Europejskiej [(Rodo)](xref:security/gdpr).</span><span class="sxs-lookup"><span data-stu-id="7793c-247">This app doesn't track personal information, but the project template includes the consent feature in case you need it to comply with the European Union's [General Data Protection Regulation (GDPR)](xref:security/gdpr).</span></span>

  ![Strona główna lub indeks](razor-pages-start/_static/homeGDPR2.2.png)

  <span data-ttu-id="7793c-249">Na poniższej ilustracji przedstawiono aplikację po udzieleniu zgody na śledzenie:</span><span class="sxs-lookup"><span data-stu-id="7793c-249">The following image shows the app after you give consent to tracking:</span></span>

  ![Strona główna lub indeks](razor-pages-start/_static/home2.2.png)
  
# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="7793c-251">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="7793c-251">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

  [!INCLUDE[](~/includes/trustCertMac.md)]

* <span data-ttu-id="7793c-252">Naciśnij **polecenie cmd-opt-F5** , aby uruchomić program bez debugera.</span><span class="sxs-lookup"><span data-stu-id="7793c-252">Press **Cmd-Opt-F5** to run without the debugger.</span></span>

  <span data-ttu-id="7793c-253">Program Visual Studio uruchamia [Kestrel](xref:fundamentals/servers/kestrel), uruchamia przeglądarkę i przechodzi do `http://localhost:5001`.</span><span class="sxs-lookup"><span data-stu-id="7793c-253">Visual Studio starts [Kestrel](xref:fundamentals/servers/kestrel), launches a browser, and navigates to `http://localhost:5001`.</span></span>

* <span data-ttu-id="7793c-254">Na stronie głównej aplikacji wybierz pozycję **Akceptuj** , aby wyrazić zgodę na śledzenie.</span><span class="sxs-lookup"><span data-stu-id="7793c-254">On the app's home page, select **Accept** to consent to tracking.</span></span>

  <span data-ttu-id="7793c-255">Ta aplikacja nie śledzi informacji osobistych, ale szablon projektu zawiera funkcję zgody na wypadek, gdyby była niezbędna do przestrzegania Ogólne rozporządzenie o ochronie danych Unii Europejskiej [(Rodo)](xref:security/gdpr).</span><span class="sxs-lookup"><span data-stu-id="7793c-255">This app doesn't track personal information, but the project template includes the consent feature in case you need it to comply with the European Union's [General Data Protection Regulation (GDPR)](xref:security/gdpr).</span></span>

  ![Strona główna lub indeks](razor-pages-start/_static/homeGDPR2.2_safari.png)

  <span data-ttu-id="7793c-257">Na poniższej ilustracji przedstawiono aplikację po udzieleniu zgody na śledzenie:</span><span class="sxs-lookup"><span data-stu-id="7793c-257">The following image shows the app after you give consent to tracking:</span></span>

  ![Strona główna lub indeks](razor-pages-start/_static/home2.2_safari.png)

<!-- End of VS tabs -->

---

## <a name="examine-the-project-files"></a><span data-ttu-id="7793c-259">Sprawdzanie plików projektu</span><span class="sxs-lookup"><span data-stu-id="7793c-259">Examine the project files</span></span>

<span data-ttu-id="7793c-260">Poniżej przedstawiono Omówienie folderów i plików projektu głównego, z których będziesz korzystać w kolejnych samouczkach.</span><span class="sxs-lookup"><span data-stu-id="7793c-260">Here's an overview of the main project folders and files that you'll work with in later tutorials.</span></span>

### <a name="pages-folder"></a><span data-ttu-id="7793c-261">Folder stron</span><span class="sxs-lookup"><span data-stu-id="7793c-261">Pages folder</span></span>

<span data-ttu-id="7793c-262">Zawiera strony Razor i pliki pomocnicze.</span><span class="sxs-lookup"><span data-stu-id="7793c-262">Contains Razor pages and supporting files.</span></span> <span data-ttu-id="7793c-263">Każda Strona Razor to para plików:</span><span class="sxs-lookup"><span data-stu-id="7793c-263">Each Razor page is a pair of files:</span></span>

* <span data-ttu-id="7793c-264">Plik *. cshtml* , który zawiera znaczniki HTML z C# kodem przy użyciu składnia Razor.</span><span class="sxs-lookup"><span data-stu-id="7793c-264">A *.cshtml* file that contains HTML markup with C# code using Razor syntax.</span></span>
* <span data-ttu-id="7793c-265">Plik *. cshtml.cs* , który zawiera C# kod, który obsługuje zdarzenia strony.</span><span class="sxs-lookup"><span data-stu-id="7793c-265">A *.cshtml.cs* file that contains C# code that handles page events.</span></span>

<span data-ttu-id="7793c-266">Pliki pomocnicze mają nazwy zaczynające się od znaku podkreślenia.</span><span class="sxs-lookup"><span data-stu-id="7793c-266">Supporting files have names that begin with an underscore.</span></span> <span data-ttu-id="7793c-267">Na przykład plik *_Layout. cshtml* służy do konfigurowania elementów interfejsu użytkownika wspólnych dla wszystkich stron.</span><span class="sxs-lookup"><span data-stu-id="7793c-267">For example, the *_Layout.cshtml* file configures UI elements common to all pages.</span></span> <span data-ttu-id="7793c-268">Ten plik konfiguruje menu nawigacji w górnej części strony i informacje o prawach autorskich w dolnej części strony.</span><span class="sxs-lookup"><span data-stu-id="7793c-268">This file sets up the navigation menu at the top of the page and the copyright notice at the bottom of the page.</span></span> <span data-ttu-id="7793c-269">Aby uzyskać więcej informacji, zobacz <xref:mvc/views/layout>.</span><span class="sxs-lookup"><span data-stu-id="7793c-269">For more information, see <xref:mvc/views/layout>.</span></span>

### <a name="wwwroot-folder"></a><span data-ttu-id="7793c-270">folder wwwroot</span><span class="sxs-lookup"><span data-stu-id="7793c-270">wwwroot folder</span></span>

<span data-ttu-id="7793c-271">Zawiera pliki statyczne, takie jak pliki HTML, pliki JavaScript i pliki CSS.</span><span class="sxs-lookup"><span data-stu-id="7793c-271">Contains static files, such as HTML files, JavaScript files, and CSS files.</span></span> <span data-ttu-id="7793c-272">Aby uzyskać więcej informacji, zobacz <xref:fundamentals/static-files>.</span><span class="sxs-lookup"><span data-stu-id="7793c-272">For more information, see <xref:fundamentals/static-files>.</span></span>

### <a name="appsettingsjson"></a><span data-ttu-id="7793c-273">appSettings.json</span><span class="sxs-lookup"><span data-stu-id="7793c-273">appSettings.json</span></span>

<span data-ttu-id="7793c-274">Zawiera dane konfiguracyjne, takie jak parametry połączenia.</span><span class="sxs-lookup"><span data-stu-id="7793c-274">Contains configuration data, such as connection strings.</span></span> <span data-ttu-id="7793c-275">Aby uzyskać więcej informacji, zobacz <xref:fundamentals/configuration/index>.</span><span class="sxs-lookup"><span data-stu-id="7793c-275">For more information, see <xref:fundamentals/configuration/index>.</span></span>

### <a name="programcs"></a><span data-ttu-id="7793c-276">Program.cs</span><span class="sxs-lookup"><span data-stu-id="7793c-276">Program.cs</span></span>

<span data-ttu-id="7793c-277">Zawiera punkt wejścia dla programu.</span><span class="sxs-lookup"><span data-stu-id="7793c-277">Contains the entry point for the program.</span></span> <span data-ttu-id="7793c-278">Aby uzyskać więcej informacji, zobacz <xref:fundamentals/host/generic-host>.</span><span class="sxs-lookup"><span data-stu-id="7793c-278">For more information, see <xref:fundamentals/host/generic-host>.</span></span>

### <a name="startupcs"></a><span data-ttu-id="7793c-279">Startup.cs</span><span class="sxs-lookup"><span data-stu-id="7793c-279">Startup.cs</span></span>

<span data-ttu-id="7793c-280">Zawiera kod, który konfiguruje zachowanie aplikacji, na przykład czy wymaga zgody na pliki cookie.</span><span class="sxs-lookup"><span data-stu-id="7793c-280">Contains code that configures app behavior, such as whether it requires consent for cookies.</span></span> <span data-ttu-id="7793c-281">Aby uzyskać więcej informacji, zobacz <xref:fundamentals/startup>.</span><span class="sxs-lookup"><span data-stu-id="7793c-281">For more information, see <xref:fundamentals/startup>.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="7793c-282">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="7793c-282">Additional resources</span></span>

* [<span data-ttu-id="7793c-283">Wersja tego samouczka usługi YouTube</span><span class="sxs-lookup"><span data-stu-id="7793c-283">Youtube version of this tutorial</span></span>](https://www.youtube.com/watch?v=F0SP7Ry4flQ&feature=youtu.be)

## <a name="next-steps"></a><span data-ttu-id="7793c-284">Następne kroki</span><span class="sxs-lookup"><span data-stu-id="7793c-284">Next steps</span></span>

<span data-ttu-id="7793c-285">Przejdź do następnego samouczka z serii:</span><span class="sxs-lookup"><span data-stu-id="7793c-285">Advance to the next tutorial in the series:</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="7793c-286">Dodawanie modelu</span><span class="sxs-lookup"><span data-stu-id="7793c-286">Add a model</span></span>](xref:tutorials/razor-pages/model)

::: moniker-end
