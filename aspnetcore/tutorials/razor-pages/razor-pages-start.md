---
title: 'Samouczek: Wprowadzenie do Razor Pages w ASP.NET Core'
author: rick-anderson
description: W tej serii samouczków pokazano, jak używać Razor Pages w ASP.NET Core. Dowiedz się, jak utworzyć model, wygenerować kod dla stron Razor, użyć Entity Framework Core i SQL Server na potrzeby dostępu do danych, dodać funkcję wyszukiwania, dodać sprawdzanie poprawności danych wejściowych i użyć migracji w celu zaktualizowania modelu.
ms.author: riande
ms.date: 07/25/2019
uid: tutorials/razor-pages/razor-pages-start
ms.openlocfilehash: 1605197188d97f27a884739a72400da2d5818b1a
ms.sourcegitcommit: 849af69ee3c94cdb9fd8fa1f1bb8f5a5dda7b9eb
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/22/2019
ms.locfileid: "68372001"
---
# <a name="tutorial-get-started-with-razor-pages-in-aspnet-core"></a><span data-ttu-id="5fa05-104">Samouczek: Wprowadzenie do Razor Pages w ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="5fa05-104">Tutorial: Get started with Razor Pages in ASP.NET Core</span></span>

<span data-ttu-id="5fa05-105">Przez [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="5fa05-105">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

::: moniker range=">= aspnetcore-3.0"
<span data-ttu-id="5fa05-106">Jest to pierwszy samouczek dotyczący serii, który uczy się podstaw tworzenia aplikacji sieci Web Razor Pages ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="5fa05-106">This is the first tutorial of a series that teaches the basics of building an ASP.NET Core Razor Pages web app.</span></span>

[!INCLUDE[](~/includes/advancedRP.md)]

<span data-ttu-id="5fa05-107">Na końcu serii będziesz mieć aplikację, która zarządza bazą danych filmów.</span><span class="sxs-lookup"><span data-stu-id="5fa05-107">At the end of the series, you'll have an app that manages a database of movies.</span></span>  

[!INCLUDE[View or download sample code](~/includes/rp/download.md)]

<span data-ttu-id="5fa05-108">W tym samouczku przedstawiono następujące instrukcje:</span><span class="sxs-lookup"><span data-stu-id="5fa05-108">In this tutorial, you:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="5fa05-109">Utwórz aplikację sieci Web Razor Pages.</span><span class="sxs-lookup"><span data-stu-id="5fa05-109">Create a Razor Pages web app.</span></span>
> * <span data-ttu-id="5fa05-110">Uruchom aplikację.</span><span class="sxs-lookup"><span data-stu-id="5fa05-110">Run the app.</span></span>
> * <span data-ttu-id="5fa05-111">Przejrzyj pliki projektu.</span><span class="sxs-lookup"><span data-stu-id="5fa05-111">Examine the project files.</span></span>

<span data-ttu-id="5fa05-112">Na końcu tego samouczka będziesz mieć działającą Razor Pagesową aplikację internetową, która zostanie wdrożona w kolejnych samouczkach.</span><span class="sxs-lookup"><span data-stu-id="5fa05-112">At the end of this tutorial, you'll have a working Razor Pages web app that you'll build on in later tutorials.</span></span>

![Strona główna lub indeks](razor-pages-start/_static/home2.2.png)

## <a name="prerequisites"></a><span data-ttu-id="5fa05-114">Wymagania wstępne</span><span class="sxs-lookup"><span data-stu-id="5fa05-114">Prerequisites</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="5fa05-115">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="5fa05-115">Visual Studio</span></span>](#tab/visual-studio)

[!INCLUDE[](~/includes/net-core-prereqs-vs-3.0.md)]

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="5fa05-116">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="5fa05-116">Visual Studio Code</span></span>](#tab/visual-studio-code)

[!INCLUDE[](~/includes/net-core-prereqs-vsc-3.0.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="5fa05-117">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="5fa05-117">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

[!INCLUDE[](~/includes/net-core-prereqs-mac-3.0.md)]

---

## <a name="create-a-razor-pages-web-app"></a><span data-ttu-id="5fa05-118">Tworzenie aplikacji sieci Web Razor Pages</span><span class="sxs-lookup"><span data-stu-id="5fa05-118">Create a Razor Pages web app</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="5fa05-119">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="5fa05-119">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="5fa05-120">W programie Visual Studio **pliku** menu, wybierz opcję **New** > **projektu**.</span><span class="sxs-lookup"><span data-stu-id="5fa05-120">From the Visual Studio **File** menu, select **New** > **Project**.</span></span>
* <span data-ttu-id="5fa05-121">Utwórz nową aplikację sieci Web ASP.NET Core a następnie wybierz pozycję **dalej**.</span><span class="sxs-lookup"><span data-stu-id="5fa05-121">Create a new ASP.NET Core Web Application and select **Next**.</span></span>
  <span data-ttu-id="5fa05-122">![Nowa aplikacja sieci Web ASP.NET Core](razor-pages-start/_static/np_2.1.png)</span><span class="sxs-lookup"><span data-stu-id="5fa05-122">![new ASP.NET Core Web Application](razor-pages-start/_static/np_2.1.png)</span></span>
* <span data-ttu-id="5fa05-123">Nazwij projekt **RazorPagesMovie**.</span><span class="sxs-lookup"><span data-stu-id="5fa05-123">Name the project **RazorPagesMovie**.</span></span> <span data-ttu-id="5fa05-124">Ważne jest, aby nazwa projektu *RazorPagesMovie* , tak aby przestrzenie nazw były zgodne podczas kopiowania i wklejania kodu.</span><span class="sxs-lookup"><span data-stu-id="5fa05-124">It's important to name the project *RazorPagesMovie* so the namespaces will match when you copy and paste code.</span></span>
  <span data-ttu-id="5fa05-125">![Nowa aplikacja sieci Web ASP.NET Core](razor-pages-start/_static/config.png)</span><span class="sxs-lookup"><span data-stu-id="5fa05-125">![new ASP.NET Core Web Application](razor-pages-start/_static/config.png)</span></span>

* <span data-ttu-id="5fa05-126">Wybierz pozycję **ASP.NET Core 3,0** na liście rozwijanej, **aplikacji sieci Web**, a następnie wybierz pozycję **Utwórz**.</span><span class="sxs-lookup"><span data-stu-id="5fa05-126">Select **ASP.NET Core 3.0** in the dropdown, **Web Application**, and then select **Create**.</span></span>

![Nowa aplikacja sieci Web ASP.NET Core](razor-pages-start/_static/3/npx.png)

  <span data-ttu-id="5fa05-128">Tworzony jest następujący projekt początkowy:</span><span class="sxs-lookup"><span data-stu-id="5fa05-128">The following starter project is created:</span></span>

  ![Eksplorator rozwiązań](razor-pages-start/_static/se2.2.png)

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="5fa05-130">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="5fa05-130">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="5fa05-131">Otwórz [zintegrowany terminal](https://code.visualstudio.com/docs/editor/integrated-terminal).</span><span class="sxs-lookup"><span data-stu-id="5fa05-131">Open the [integrated terminal](https://code.visualstudio.com/docs/editor/integrated-terminal).</span></span>

* <span data-ttu-id="5fa05-132">Przejdź do katalogu (`cd`), który będzie zawierać projekt.</span><span class="sxs-lookup"><span data-stu-id="5fa05-132">Change to the directory (`cd`) which will contain the project.</span></span>

* <span data-ttu-id="5fa05-133">Uruchom następujące polecenia:</span><span class="sxs-lookup"><span data-stu-id="5fa05-133">Run the following commands:</span></span>

  ```console
  dotnet new webapp -o RazorPagesMovie
  code -r RazorPagesMovie
  ```

  * <span data-ttu-id="5fa05-134">Polecenie tworzy nowy projekt Razor Pages w folderze RazorPagesMovie.  `dotnet new`</span><span class="sxs-lookup"><span data-stu-id="5fa05-134">The `dotnet new` command creates a new Razor Pages project in the *RazorPagesMovie* folder.</span></span>
  * <span data-ttu-id="5fa05-135">Polecenie otwiera folder RazorPagesMovie w bieżącym wystąpieniu Visual Studio Code.  `code`</span><span class="sxs-lookup"><span data-stu-id="5fa05-135">The `code` command opens the *RazorPagesMovie* folder in the current instance of Visual Studio Code.</span></span>

* <span data-ttu-id="5fa05-136">Gdy ikona płomienia OmniSharp na pasku stanu zmieni kolor na zielony, w **oknie dialogowym zostanie wyświetlony monit o podanie wymaganych zasobów do skompilowania i debugowania z elementu "RazorPagesMovie". Dodać je?**</span><span class="sxs-lookup"><span data-stu-id="5fa05-136">After the status bar's OmniSharp flame icon turns green, a dialog asks **Required assets to build and debug are missing from 'RazorPagesMovie'. Add them?**</span></span> <span data-ttu-id="5fa05-137">Wybierz pozycję **tak**.</span><span class="sxs-lookup"><span data-stu-id="5fa05-137">Select **Yes**.</span></span>

  <span data-ttu-id="5fa05-138">Katalog *. programu vscode* , zawierający pliki *Launch. JSON* i *Tasks. JSON* , jest dodawany do katalogu głównego projektu.</span><span class="sxs-lookup"><span data-stu-id="5fa05-138">A *.vscode* directory, containing *launch.json* and *tasks.json* files, is added to the project's root directory.</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="5fa05-139">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="5fa05-139">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="5fa05-140">W terminalu uruchom następujące polecenie:</span><span class="sxs-lookup"><span data-stu-id="5fa05-140">From a terminal, run the following command:</span></span>

<!-- TODO: update these instruction once mac support 2.2 projects -->

```console
dotnet new webapp -o RazorPagesMovie
```

<span data-ttu-id="5fa05-141">Poprzednie polecenia używają [interfejs wiersza polecenia platformy .NET Core](/dotnet/core/tools/dotnet) do tworzenia projektu Razor Pages.</span><span class="sxs-lookup"><span data-stu-id="5fa05-141">The preceding commands use the [.NET Core CLI](/dotnet/core/tools/dotnet) to create a Razor Pages project.</span></span>

## <a name="open-the-project"></a><span data-ttu-id="5fa05-142">Otwórz projekt</span><span class="sxs-lookup"><span data-stu-id="5fa05-142">Open the project</span></span>

<span data-ttu-id="5fa05-143">W programie Visual Studio wybierz pozycję **plik > Otwórz**, a następnie wybierz plik *RazorPagesMovie. csproj* .</span><span class="sxs-lookup"><span data-stu-id="5fa05-143">From Visual Studio, select **File > Open**, and then select the *RazorPagesMovie.csproj* file.</span></span>

<!-- End of VS tabs -->

---

## <a name="run-the-app"></a><span data-ttu-id="5fa05-144">Uruchamianie aplikacji</span><span class="sxs-lookup"><span data-stu-id="5fa05-144">Run the app</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="5fa05-145">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="5fa05-145">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="5fa05-146">Naciśnij klawisze CTRL + F5, aby uruchomić bez debugera.</span><span class="sxs-lookup"><span data-stu-id="5fa05-146">Press Ctrl+F5 to run without the debugger.</span></span>

  [!INCLUDE[](~/includes/trustCertVS.md)]

  <span data-ttu-id="5fa05-147">Program Visual Studio jest uruchamiany [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) i uruchomi aplikację.</span><span class="sxs-lookup"><span data-stu-id="5fa05-147">Visual Studio starts [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) and runs the app.</span></span> <span data-ttu-id="5fa05-148">Na pasku adresu są `localhost:port#` wyświetlane inne elementy, `example.com`takie jak.</span><span class="sxs-lookup"><span data-stu-id="5fa05-148">The address bar shows `localhost:port#` and not something like `example.com`.</span></span> <span data-ttu-id="5fa05-149">Wynika `localhost` to z tego, że jest standardową nazwą hosta dla komputera lokalnego.</span><span class="sxs-lookup"><span data-stu-id="5fa05-149">That's because `localhost` is the standard hostname for the local computer.</span></span> <span data-ttu-id="5fa05-150">Host lokalny obsługuje tylko żądania sieci Web z komputera lokalnego.</span><span class="sxs-lookup"><span data-stu-id="5fa05-150">Localhost only serves web requests from the local computer.</span></span> <span data-ttu-id="5fa05-151">Gdy program Visual Studio tworzy projekt sieci Web, dla serwera sieci Web jest używany port losowy.</span><span class="sxs-lookup"><span data-stu-id="5fa05-151">When Visual Studio creates a web project, a random port is used for the web server.</span></span>
 
# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="5fa05-152">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="5fa05-152">Visual Studio Code</span></span>](#tab/visual-studio-code)

  [!INCLUDE[](~/includes/trustCertVSC.md)]

* <span data-ttu-id="5fa05-153">Naciśnij **klawisze CTRL + F5** , aby uruchomić bez debugera.</span><span class="sxs-lookup"><span data-stu-id="5fa05-153">Press **Ctrl-F5** to run without the debugger.</span></span>

  <span data-ttu-id="5fa05-154">Visual Studio Code uruchamia [Kestrel](xref:fundamentals/servers/kestrel), uruchamia przeglądarkę i nawiguje do `http://localhost:5001`.</span><span class="sxs-lookup"><span data-stu-id="5fa05-154">Visual Studio Code starts [Kestrel](xref:fundamentals/servers/kestrel), launches a browser, and navigates to `http://localhost:5001`.</span></span> <span data-ttu-id="5fa05-155">Na pasku adresu są `localhost:port#` wyświetlane inne elementy, `example.com`takie jak.</span><span class="sxs-lookup"><span data-stu-id="5fa05-155">The address bar shows `localhost:port#` and not something like `example.com`.</span></span> <span data-ttu-id="5fa05-156">Wynika `localhost` to z tego, że jest standardową nazwą hosta dla komputera lokalnego.</span><span class="sxs-lookup"><span data-stu-id="5fa05-156">That's because `localhost` is the standard hostname for  local computer.</span></span> <span data-ttu-id="5fa05-157">Host lokalny obsługuje tylko żądania sieci Web z komputera lokalnego.</span><span class="sxs-lookup"><span data-stu-id="5fa05-157">Localhost only serves web requests from the local computer.</span></span>

  
# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="5fa05-158">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="5fa05-158">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

  [!INCLUDE[](~/includes/trustCertMac.md)]

* <span data-ttu-id="5fa05-159">Naciśnij **polecenie cmd-opt-F5** , aby uruchomić program bez debugera.</span><span class="sxs-lookup"><span data-stu-id="5fa05-159">Press **Cmd-Opt-F5** to run without the debugger.</span></span>

  <span data-ttu-id="5fa05-160">Program Visual Studio uruchamia [Kestrel](xref:fundamentals/servers/kestrel), uruchamia przeglądarkę i przechodzi do `http://localhost:5001`.</span><span class="sxs-lookup"><span data-stu-id="5fa05-160">Visual Studio starts [Kestrel](xref:fundamentals/servers/kestrel), launches a browser, and navigates to `http://localhost:5001`.</span></span>

<!-- End of VS tabs -->

---

## <a name="examine-the-project-files"></a><span data-ttu-id="5fa05-161">Sprawdzanie plików projektu</span><span class="sxs-lookup"><span data-stu-id="5fa05-161">Examine the project files</span></span>

<span data-ttu-id="5fa05-162">Poniżej przedstawiono Omówienie folderów i plików projektu głównego, z których będziesz korzystać w kolejnych samouczkach.</span><span class="sxs-lookup"><span data-stu-id="5fa05-162">Here's an overview of the main project folders and files that you'll work with in later tutorials.</span></span>

### <a name="pages-folder"></a><span data-ttu-id="5fa05-163">Folder stron</span><span class="sxs-lookup"><span data-stu-id="5fa05-163">Pages folder</span></span>

<span data-ttu-id="5fa05-164">Zawiera strony Razor i pliki pomocnicze.</span><span class="sxs-lookup"><span data-stu-id="5fa05-164">Contains Razor pages and supporting files.</span></span> <span data-ttu-id="5fa05-165">Każda Strona Razor to para plików:</span><span class="sxs-lookup"><span data-stu-id="5fa05-165">Each Razor page is a pair of files:</span></span>

* <span data-ttu-id="5fa05-166">Plik *. cshtml* , który zawiera znaczniki HTML z C# kodem przy użyciu składnia Razor.</span><span class="sxs-lookup"><span data-stu-id="5fa05-166">A *.cshtml* file that contains HTML markup with C# code using Razor syntax.</span></span>
* <span data-ttu-id="5fa05-167">Plik *. cshtml.cs* , który zawiera C# kod, który obsługuje zdarzenia strony.</span><span class="sxs-lookup"><span data-stu-id="5fa05-167">A *.cshtml.cs* file that contains C# code that handles page events.</span></span>

<span data-ttu-id="5fa05-168">Pliki pomocnicze mają nazwy zaczynające się od znaku podkreślenia.</span><span class="sxs-lookup"><span data-stu-id="5fa05-168">Supporting files have names that begin with an underscore.</span></span> <span data-ttu-id="5fa05-169">Na przykład plik *_Layout. cshtml* służy do konfigurowania elementów interfejsu użytkownika wspólnych dla wszystkich stron.</span><span class="sxs-lookup"><span data-stu-id="5fa05-169">For example, the *_Layout.cshtml* file configures UI elements common to all pages.</span></span> <span data-ttu-id="5fa05-170">Ten plik konfiguruje menu nawigacji w górnej części strony i informacje o prawach autorskich w dolnej części strony.</span><span class="sxs-lookup"><span data-stu-id="5fa05-170">This file sets up the navigation menu at the top of the page and the copyright notice at the bottom of the page.</span></span> <span data-ttu-id="5fa05-171">Aby uzyskać więcej informacji, zobacz <xref:mvc/views/layout>.</span><span class="sxs-lookup"><span data-stu-id="5fa05-171">For more information, see <xref:mvc/views/layout>.</span></span>

### <a name="wwwroot-folder"></a><span data-ttu-id="5fa05-172">folder wwwroot</span><span class="sxs-lookup"><span data-stu-id="5fa05-172">wwwroot folder</span></span>

<span data-ttu-id="5fa05-173">Zawiera pliki statyczne, takie jak pliki HTML, pliki JavaScript i pliki CSS.</span><span class="sxs-lookup"><span data-stu-id="5fa05-173">Contains static files, such as HTML files, JavaScript files, and CSS files.</span></span> <span data-ttu-id="5fa05-174">Aby uzyskać więcej informacji, zobacz <xref:fundamentals/static-files>.</span><span class="sxs-lookup"><span data-stu-id="5fa05-174">For more information, see <xref:fundamentals/static-files>.</span></span>

### <a name="appsettingsjson"></a><span data-ttu-id="5fa05-175">appSettings.json</span><span class="sxs-lookup"><span data-stu-id="5fa05-175">appSettings.json</span></span>

<span data-ttu-id="5fa05-176">Zawiera dane konfiguracyjne, takie jak parametry połączenia.</span><span class="sxs-lookup"><span data-stu-id="5fa05-176">Contains configuration data, such as connection strings.</span></span> <span data-ttu-id="5fa05-177">Aby uzyskać więcej informacji, zobacz <xref:fundamentals/configuration/index>.</span><span class="sxs-lookup"><span data-stu-id="5fa05-177">For more information, see <xref:fundamentals/configuration/index>.</span></span>

### <a name="programcs"></a><span data-ttu-id="5fa05-178">Program.cs</span><span class="sxs-lookup"><span data-stu-id="5fa05-178">Program.cs</span></span>

<span data-ttu-id="5fa05-179">Zawiera punkt wejścia dla programu.</span><span class="sxs-lookup"><span data-stu-id="5fa05-179">Contains the entry point for the program.</span></span> <span data-ttu-id="5fa05-180">Aby uzyskać więcej informacji, zobacz <xref:fundamentals/host/generic-host>.</span><span class="sxs-lookup"><span data-stu-id="5fa05-180">For more information, see <xref:fundamentals/host/generic-host>.</span></span>

### <a name="startupcs"></a><span data-ttu-id="5fa05-181">Startup.cs</span><span class="sxs-lookup"><span data-stu-id="5fa05-181">Startup.cs</span></span>

<span data-ttu-id="5fa05-182">Zawiera kod, który konfiguruje zachowanie aplikacji.</span><span class="sxs-lookup"><span data-stu-id="5fa05-182">Contains code that configures app behavior.</span></span> <span data-ttu-id="5fa05-183">Aby uzyskać więcej informacji, zobacz <xref:fundamentals/startup>.</span><span class="sxs-lookup"><span data-stu-id="5fa05-183">For more information, see <xref:fundamentals/startup>.</span></span>

## <a name="next-steps"></a><span data-ttu-id="5fa05-184">Następne kroki</span><span class="sxs-lookup"><span data-stu-id="5fa05-184">Next steps</span></span>

<span data-ttu-id="5fa05-185">Przejdź do następnego samouczka z serii:</span><span class="sxs-lookup"><span data-stu-id="5fa05-185">Advance to the next tutorial in the series:</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="5fa05-186">Dodawanie modelu</span><span class="sxs-lookup"><span data-stu-id="5fa05-186">Add a model</span></span>](xref:tutorials/razor-pages/model)

::: moniker-end

<!--::: moniker range=">= aspnetcore-3.0" -->

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="5fa05-187">Jest to pierwszy samouczek dotyczący serii.</span><span class="sxs-lookup"><span data-stu-id="5fa05-187">This is the first tutorial of a series.</span></span> <span data-ttu-id="5fa05-188">[Seria zawiera](xref:tutorials/razor-pages/index) podstawowe informacje na temat tworzenia aplikacji sieci web ASP.NET Core Razor Pages.</span><span class="sxs-lookup"><span data-stu-id="5fa05-188">[The series](xref:tutorials/razor-pages/index) teaches the basics of building an ASP.NET Core Razor Pages web app.</span></span>

[!INCLUDE[](~/includes/advancedRP.md)]

<span data-ttu-id="5fa05-189">Na końcu serii będziesz mieć aplikację, która zarządza bazą danych filmów.</span><span class="sxs-lookup"><span data-stu-id="5fa05-189">At the end of the series, you'll have an app that manages a database of movies.</span></span>  

[!INCLUDE[View or download sample code](~/includes/rp/download.md)]

<span data-ttu-id="5fa05-190">W tym samouczku przedstawiono następujące instrukcje:</span><span class="sxs-lookup"><span data-stu-id="5fa05-190">In this tutorial, you:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="5fa05-191">Utwórz aplikację sieci Web Razor Pages.</span><span class="sxs-lookup"><span data-stu-id="5fa05-191">Create a Razor Pages web app.</span></span>
> * <span data-ttu-id="5fa05-192">Uruchom aplikację.</span><span class="sxs-lookup"><span data-stu-id="5fa05-192">Run the app.</span></span>
> * <span data-ttu-id="5fa05-193">Przejrzyj pliki projektu.</span><span class="sxs-lookup"><span data-stu-id="5fa05-193">Examine the project files.</span></span>

<span data-ttu-id="5fa05-194">Na końcu tego samouczka będziesz mieć działającą Razor Pagesową aplikację internetową, która zostanie wdrożona w kolejnych samouczkach.</span><span class="sxs-lookup"><span data-stu-id="5fa05-194">At the end of this tutorial, you'll have a working Razor Pages web app that you'll build on in later tutorials.</span></span>

![Strona główna lub indeks](razor-pages-start/_static/home2.2.png)

## <a name="prerequisites"></a><span data-ttu-id="5fa05-196">Wymagania wstępne</span><span class="sxs-lookup"><span data-stu-id="5fa05-196">Prerequisites</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="5fa05-197">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="5fa05-197">Visual Studio</span></span>](#tab/visual-studio)

[!INCLUDE[](~/includes/net-core-prereqs-vs2019-2.2.md)]

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="5fa05-198">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="5fa05-198">Visual Studio Code</span></span>](#tab/visual-studio-code)

[!INCLUDE[](~/includes/net-core-prereqs-vsc-2.2.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="5fa05-199">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="5fa05-199">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

[!INCLUDE[](~/includes/net-core-prereqs-mac-2.2.md)]

---

## <a name="create-a-razor-pages-web-app"></a><span data-ttu-id="5fa05-200">Tworzenie aplikacji sieci Web Razor Pages</span><span class="sxs-lookup"><span data-stu-id="5fa05-200">Create a Razor Pages web app</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="5fa05-201">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="5fa05-201">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="5fa05-202">W programie Visual Studio **pliku** menu, wybierz opcję **New** > **projektu**.</span><span class="sxs-lookup"><span data-stu-id="5fa05-202">From the Visual Studio **File** menu, select **New** > **Project**.</span></span>

* <span data-ttu-id="5fa05-203">Utwórz nową aplikację sieci Web ASP.NET Core a następnie wybierz pozycję **dalej**.</span><span class="sxs-lookup"><span data-stu-id="5fa05-203">Create a new ASP.NET Core Web Application and select **Next**.</span></span>

  ![Nowa aplikacja sieci Web ASP.NET Core](razor-pages-start/_static/np_2.1.png)

* <span data-ttu-id="5fa05-205">Nazwij projekt **RazorPagesMovie**.</span><span class="sxs-lookup"><span data-stu-id="5fa05-205">Name the project **RazorPagesMovie**.</span></span> <span data-ttu-id="5fa05-206">Ważne jest, aby nazwa projektu *RazorPagesMovie* , tak aby przestrzenie nazw były zgodne podczas kopiowania i wklejania kodu.</span><span class="sxs-lookup"><span data-stu-id="5fa05-206">It's important to name the project *RazorPagesMovie* so the namespaces will match when you copy and paste code.</span></span>

  ![Nowa aplikacja sieci Web ASP.NET Core](razor-pages-start/_static/config.png)

* <span data-ttu-id="5fa05-208">Wybierz pozycję **ASP.NET Core 2,2** na liście rozwijanej, **aplikacji sieci Web**, a następnie wybierz pozycję **Utwórz**.</span><span class="sxs-lookup"><span data-stu-id="5fa05-208">Select **ASP.NET Core 2.2** in the dropdown, **Web Application**, and then select **Create**.</span></span>

![Nowa aplikacja sieci Web ASP.NET Core](razor-pages-start/_static/np_2_2.2.png)

  <span data-ttu-id="5fa05-210">Tworzony jest następujący projekt początkowy:</span><span class="sxs-lookup"><span data-stu-id="5fa05-210">The following starter project is created:</span></span>

  ![Eksplorator rozwiązań](razor-pages-start/_static/se2.2.png)

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="5fa05-212">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="5fa05-212">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="5fa05-213">Otwórz [zintegrowany terminal](https://code.visualstudio.com/docs/editor/integrated-terminal).</span><span class="sxs-lookup"><span data-stu-id="5fa05-213">Open the [integrated terminal](https://code.visualstudio.com/docs/editor/integrated-terminal).</span></span>

* <span data-ttu-id="5fa05-214">Przejdź do katalogu (`cd`), który będzie zawierać projekt.</span><span class="sxs-lookup"><span data-stu-id="5fa05-214">Change to the directory (`cd`) which will contain the project.</span></span>

* <span data-ttu-id="5fa05-215">Uruchom następujące polecenia:</span><span class="sxs-lookup"><span data-stu-id="5fa05-215">Run the following commands:</span></span>

  ```console
  dotnet new webapp -o RazorPagesMovie
  code -r RazorPagesMovie
  ```

  * <span data-ttu-id="5fa05-216">Polecenie tworzy nowy projekt Razor Pages w folderze RazorPagesMovie.  `dotnet new`</span><span class="sxs-lookup"><span data-stu-id="5fa05-216">The `dotnet new` command creates a new Razor Pages project in the *RazorPagesMovie* folder.</span></span>
  * <span data-ttu-id="5fa05-217">Polecenie otwiera folder RazorPagesMovie w bieżącym wystąpieniu Visual Studio Code.  `code`</span><span class="sxs-lookup"><span data-stu-id="5fa05-217">The `code` command opens the *RazorPagesMovie* folder in the current instance of Visual Studio Code.</span></span>

* <span data-ttu-id="5fa05-218">Gdy ikona płomienia OmniSharp na pasku stanu zmieni kolor na zielony, w **oknie dialogowym zostanie wyświetlony monit o podanie wymaganych zasobów do skompilowania i debugowania z elementu "RazorPagesMovie". Dodać je?**</span><span class="sxs-lookup"><span data-stu-id="5fa05-218">After the status bar's OmniSharp flame icon turns green, a dialog asks **Required assets to build and debug are missing from 'RazorPagesMovie'. Add them?**</span></span> <span data-ttu-id="5fa05-219">Wybierz pozycję **tak**.</span><span class="sxs-lookup"><span data-stu-id="5fa05-219">Select **Yes**.</span></span>

  <span data-ttu-id="5fa05-220">Katalog *. programu vscode* , zawierający pliki *Launch. JSON* i *Tasks. JSON* , jest dodawany do katalogu głównego projektu.</span><span class="sxs-lookup"><span data-stu-id="5fa05-220">A *.vscode* directory, containing *launch.json* and *tasks.json* files, is added to the project's root directory.</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="5fa05-221">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="5fa05-221">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="5fa05-222">W terminalu uruchom następujące polecenie:</span><span class="sxs-lookup"><span data-stu-id="5fa05-222">From a terminal, run the following command:</span></span>

<!-- TODO: update these instruction once mac support 2.2 projects -->

```console
dotnet new webapp -o RazorPagesMovie
```

<span data-ttu-id="5fa05-223">Poprzednie polecenia używają [interfejs wiersza polecenia platformy .NET Core](/dotnet/core/tools/dotnet) do tworzenia projektu Razor Pages.</span><span class="sxs-lookup"><span data-stu-id="5fa05-223">The preceding commands use the [.NET Core CLI](/dotnet/core/tools/dotnet) to create a Razor Pages project.</span></span>

## <a name="open-the-project"></a><span data-ttu-id="5fa05-224">Otwórz projekt</span><span class="sxs-lookup"><span data-stu-id="5fa05-224">Open the project</span></span>

<span data-ttu-id="5fa05-225">W programie Visual Studio wybierz pozycję **plik > Otwórz**, a następnie wybierz plik *RazorPagesMovie. csproj* .</span><span class="sxs-lookup"><span data-stu-id="5fa05-225">From Visual Studio, select **File > Open**, and then select the *RazorPagesMovie.csproj* file.</span></span>

<!-- End of VS tabs -->

---

## <a name="run-the-app"></a><span data-ttu-id="5fa05-226">Uruchamianie aplikacji</span><span class="sxs-lookup"><span data-stu-id="5fa05-226">Run the app</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="5fa05-227">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="5fa05-227">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="5fa05-228">Naciśnij klawisze CTRL + F5, aby uruchomić bez debugera.</span><span class="sxs-lookup"><span data-stu-id="5fa05-228">Press Ctrl+F5 to run without the debugger.</span></span>

  [!INCLUDE[](~/includes/trustCertVS.md)]

  <span data-ttu-id="5fa05-229">Program Visual Studio jest uruchamiany [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) i uruchomi aplikację.</span><span class="sxs-lookup"><span data-stu-id="5fa05-229">Visual Studio starts [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) and runs the app.</span></span> <span data-ttu-id="5fa05-230">Na pasku adresu są `localhost:port#` wyświetlane inne elementy, `example.com`takie jak.</span><span class="sxs-lookup"><span data-stu-id="5fa05-230">The address bar shows `localhost:port#` and not something like `example.com`.</span></span> <span data-ttu-id="5fa05-231">Wynika `localhost` to z tego, że jest standardową nazwą hosta dla komputera lokalnego.</span><span class="sxs-lookup"><span data-stu-id="5fa05-231">That's because `localhost` is the standard hostname for the local computer.</span></span> <span data-ttu-id="5fa05-232">Host lokalny obsługuje tylko żądania sieci Web z komputera lokalnego.</span><span class="sxs-lookup"><span data-stu-id="5fa05-232">Localhost only serves web requests from the local computer.</span></span> <span data-ttu-id="5fa05-233">Gdy program Visual Studio tworzy projekt sieci Web, dla serwera sieci Web jest używany port losowy.</span><span class="sxs-lookup"><span data-stu-id="5fa05-233">When Visual Studio creates a web project, a random port is used for the web server.</span></span>

* <span data-ttu-id="5fa05-234">Na stronie głównej aplikacji wybierz pozycję **Akceptuj** , aby wyrazić zgodę na śledzenie.</span><span class="sxs-lookup"><span data-stu-id="5fa05-234">On the app's home page, select **Accept** to consent to tracking.</span></span>

  <span data-ttu-id="5fa05-235">Ta aplikacja nie śledzi informacji osobistych, ale szablon projektu zawiera funkcję zgody na wypadek, gdyby była niezbędna do przestrzegania Ogólne rozporządzenie o ochronie danych Unii Europejskiej [(Rodo)](xref:security/gdpr).</span><span class="sxs-lookup"><span data-stu-id="5fa05-235">This app doesn't track personal information, but the project template includes the consent feature in case you need it to comply with the European Union's [General Data Protection Regulation (GDPR)](xref:security/gdpr).</span></span>

  ![Strona główna lub indeks](razor-pages-start/_static/homeGDPR2.2.png)

  <span data-ttu-id="5fa05-237">Na poniższej ilustracji przedstawiono aplikację po udzieleniu zgody na śledzenie:</span><span class="sxs-lookup"><span data-stu-id="5fa05-237">The following image shows the app after you give consent to tracking:</span></span>

  ![Strona główna lub indeks](razor-pages-start/_static/home2.2.png)
  
# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="5fa05-239">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="5fa05-239">Visual Studio Code</span></span>](#tab/visual-studio-code)

  [!INCLUDE[](~/includes/trustCertVSC.md)]

* <span data-ttu-id="5fa05-240">Naciśnij **klawisze CTRL + F5** , aby uruchomić bez debugera.</span><span class="sxs-lookup"><span data-stu-id="5fa05-240">Press **Ctrl-F5** to run without the debugger.</span></span>

  <span data-ttu-id="5fa05-241">Visual Studio Code uruchamia [Kestrel](xref:fundamentals/servers/kestrel), uruchamia przeglądarkę i nawiguje do `http://localhost:5001`.</span><span class="sxs-lookup"><span data-stu-id="5fa05-241">Visual Studio Code starts [Kestrel](xref:fundamentals/servers/kestrel), launches a browser, and navigates to `http://localhost:5001`.</span></span> <span data-ttu-id="5fa05-242">Na pasku adresu są `localhost:port#` wyświetlane inne elementy, `example.com`takie jak.</span><span class="sxs-lookup"><span data-stu-id="5fa05-242">The address bar shows `localhost:port#` and not something like `example.com`.</span></span> <span data-ttu-id="5fa05-243">Wynika `localhost` to z tego, że jest standardową nazwą hosta dla komputera lokalnego.</span><span class="sxs-lookup"><span data-stu-id="5fa05-243">That's because `localhost` is the standard hostname for  local computer.</span></span> <span data-ttu-id="5fa05-244">Host lokalny obsługuje tylko żądania sieci Web z komputera lokalnego.</span><span class="sxs-lookup"><span data-stu-id="5fa05-244">Localhost only serves web requests from the local computer.</span></span>

* <span data-ttu-id="5fa05-245">Na stronie głównej aplikacji wybierz pozycję **Akceptuj** , aby wyrazić zgodę na śledzenie.</span><span class="sxs-lookup"><span data-stu-id="5fa05-245">On the app's home page, select **Accept** to consent to tracking.</span></span>

  <span data-ttu-id="5fa05-246">Ta aplikacja nie śledzi informacji osobistych, ale szablon projektu zawiera funkcję zgody na wypadek, gdyby była niezbędna do przestrzegania Ogólne rozporządzenie o ochronie danych Unii Europejskiej [(Rodo)](xref:security/gdpr).</span><span class="sxs-lookup"><span data-stu-id="5fa05-246">This app doesn't track personal information, but the project template includes the consent feature in case you need it to comply with the European Union's [General Data Protection Regulation (GDPR)](xref:security/gdpr).</span></span>

  ![Strona główna lub indeks](razor-pages-start/_static/homeGDPR2.2.png)

  <span data-ttu-id="5fa05-248">Na poniższej ilustracji przedstawiono aplikację po udzieleniu zgody na śledzenie:</span><span class="sxs-lookup"><span data-stu-id="5fa05-248">The following image shows the app after you give consent to tracking:</span></span>

  ![Strona główna lub indeks](razor-pages-start/_static/home2.2.png)
  
# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="5fa05-250">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="5fa05-250">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

  [!INCLUDE[](~/includes/trustCertMac.md)]

* <span data-ttu-id="5fa05-251">Naciśnij **polecenie cmd-opt-F5** , aby uruchomić program bez debugera.</span><span class="sxs-lookup"><span data-stu-id="5fa05-251">Press **Cmd-Opt-F5** to run without the debugger.</span></span>

  <span data-ttu-id="5fa05-252">Program Visual Studio uruchamia [Kestrel](xref:fundamentals/servers/kestrel), uruchamia przeglądarkę i przechodzi do `http://localhost:5001`.</span><span class="sxs-lookup"><span data-stu-id="5fa05-252">Visual Studio starts [Kestrel](xref:fundamentals/servers/kestrel), launches a browser, and navigates to `http://localhost:5001`.</span></span>

* <span data-ttu-id="5fa05-253">Na stronie głównej aplikacji wybierz pozycję **Akceptuj** , aby wyrazić zgodę na śledzenie.</span><span class="sxs-lookup"><span data-stu-id="5fa05-253">On the app's home page, select **Accept** to consent to tracking.</span></span>

  <span data-ttu-id="5fa05-254">Ta aplikacja nie śledzi informacji osobistych, ale szablon projektu zawiera funkcję zgody na wypadek, gdyby była niezbędna do przestrzegania Ogólne rozporządzenie o ochronie danych Unii Europejskiej [(Rodo)](xref:security/gdpr).</span><span class="sxs-lookup"><span data-stu-id="5fa05-254">This app doesn't track personal information, but the project template includes the consent feature in case you need it to comply with the European Union's [General Data Protection Regulation (GDPR)](xref:security/gdpr).</span></span>

  ![Strona główna lub indeks](razor-pages-start/_static/homeGDPR2.2_safari.png)

  <span data-ttu-id="5fa05-256">Na poniższej ilustracji przedstawiono aplikację po udzieleniu zgody na śledzenie:</span><span class="sxs-lookup"><span data-stu-id="5fa05-256">The following image shows the app after you give consent to tracking:</span></span>

  ![Strona główna lub indeks](razor-pages-start/_static/home2.2_safari.png)

<!-- End of VS tabs -->

---

## <a name="examine-the-project-files"></a><span data-ttu-id="5fa05-258">Sprawdzanie plików projektu</span><span class="sxs-lookup"><span data-stu-id="5fa05-258">Examine the project files</span></span>

<span data-ttu-id="5fa05-259">Poniżej przedstawiono Omówienie folderów i plików projektu głównego, z których będziesz korzystać w kolejnych samouczkach.</span><span class="sxs-lookup"><span data-stu-id="5fa05-259">Here's an overview of the main project folders and files that you'll work with in later tutorials.</span></span>

### <a name="pages-folder"></a><span data-ttu-id="5fa05-260">Folder stron</span><span class="sxs-lookup"><span data-stu-id="5fa05-260">Pages folder</span></span>

<span data-ttu-id="5fa05-261">Zawiera strony Razor i pliki pomocnicze.</span><span class="sxs-lookup"><span data-stu-id="5fa05-261">Contains Razor pages and supporting files.</span></span> <span data-ttu-id="5fa05-262">Każda Strona Razor to para plików:</span><span class="sxs-lookup"><span data-stu-id="5fa05-262">Each Razor page is a pair of files:</span></span>

* <span data-ttu-id="5fa05-263">Plik *. cshtml* , który zawiera znaczniki HTML z C# kodem przy użyciu składnia Razor.</span><span class="sxs-lookup"><span data-stu-id="5fa05-263">A *.cshtml* file that contains HTML markup with C# code using Razor syntax.</span></span>
* <span data-ttu-id="5fa05-264">Plik *. cshtml.cs* , który zawiera C# kod, który obsługuje zdarzenia strony.</span><span class="sxs-lookup"><span data-stu-id="5fa05-264">A *.cshtml.cs* file that contains C# code that handles page events.</span></span>

<span data-ttu-id="5fa05-265">Pliki pomocnicze mają nazwy zaczynające się od znaku podkreślenia.</span><span class="sxs-lookup"><span data-stu-id="5fa05-265">Supporting files have names that begin with an underscore.</span></span> <span data-ttu-id="5fa05-266">Na przykład plik *_Layout. cshtml* służy do konfigurowania elementów interfejsu użytkownika wspólnych dla wszystkich stron.</span><span class="sxs-lookup"><span data-stu-id="5fa05-266">For example, the *_Layout.cshtml* file configures UI elements common to all pages.</span></span> <span data-ttu-id="5fa05-267">Ten plik konfiguruje menu nawigacji w górnej części strony i informacje o prawach autorskich w dolnej części strony.</span><span class="sxs-lookup"><span data-stu-id="5fa05-267">This file sets up the navigation menu at the top of the page and the copyright notice at the bottom of the page.</span></span> <span data-ttu-id="5fa05-268">Aby uzyskać więcej informacji, zobacz <xref:mvc/views/layout>.</span><span class="sxs-lookup"><span data-stu-id="5fa05-268">For more information, see <xref:mvc/views/layout>.</span></span>

### <a name="wwwroot-folder"></a><span data-ttu-id="5fa05-269">folder wwwroot</span><span class="sxs-lookup"><span data-stu-id="5fa05-269">wwwroot folder</span></span>

<span data-ttu-id="5fa05-270">Zawiera pliki statyczne, takie jak pliki HTML, pliki JavaScript i pliki CSS.</span><span class="sxs-lookup"><span data-stu-id="5fa05-270">Contains static files, such as HTML files, JavaScript files, and CSS files.</span></span> <span data-ttu-id="5fa05-271">Aby uzyskać więcej informacji, zobacz <xref:fundamentals/static-files>.</span><span class="sxs-lookup"><span data-stu-id="5fa05-271">For more information, see <xref:fundamentals/static-files>.</span></span>

### <a name="appsettingsjson"></a><span data-ttu-id="5fa05-272">appSettings.json</span><span class="sxs-lookup"><span data-stu-id="5fa05-272">appSettings.json</span></span>

<span data-ttu-id="5fa05-273">Zawiera dane konfiguracyjne, takie jak parametry połączenia.</span><span class="sxs-lookup"><span data-stu-id="5fa05-273">Contains configuration data, such as connection strings.</span></span> <span data-ttu-id="5fa05-274">Aby uzyskać więcej informacji, zobacz <xref:fundamentals/configuration/index>.</span><span class="sxs-lookup"><span data-stu-id="5fa05-274">For more information, see <xref:fundamentals/configuration/index>.</span></span>

### <a name="programcs"></a><span data-ttu-id="5fa05-275">Program.cs</span><span class="sxs-lookup"><span data-stu-id="5fa05-275">Program.cs</span></span>

<span data-ttu-id="5fa05-276">Zawiera punkt wejścia dla programu.</span><span class="sxs-lookup"><span data-stu-id="5fa05-276">Contains the entry point for the program.</span></span> <span data-ttu-id="5fa05-277">Aby uzyskać więcej informacji, zobacz <xref:fundamentals/host/generic-host>.</span><span class="sxs-lookup"><span data-stu-id="5fa05-277">For more information, see <xref:fundamentals/host/generic-host>.</span></span>

### <a name="startupcs"></a><span data-ttu-id="5fa05-278">Startup.cs</span><span class="sxs-lookup"><span data-stu-id="5fa05-278">Startup.cs</span></span>

<span data-ttu-id="5fa05-279">Zawiera kod, który konfiguruje zachowanie aplikacji, na przykład czy wymaga zgody na pliki cookie.</span><span class="sxs-lookup"><span data-stu-id="5fa05-279">Contains code that configures app behavior, such as whether it requires consent for cookies.</span></span> <span data-ttu-id="5fa05-280">Aby uzyskać więcej informacji, zobacz <xref:fundamentals/startup>.</span><span class="sxs-lookup"><span data-stu-id="5fa05-280">For more information, see <xref:fundamentals/startup>.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="5fa05-281">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="5fa05-281">Additional resources</span></span>

* [<span data-ttu-id="5fa05-282">Wersja tego samouczka usługi YouTube</span><span class="sxs-lookup"><span data-stu-id="5fa05-282">Youtube version of this tutorial</span></span>](https://www.youtube.com/watch?v=F0SP7Ry4flQ&feature=youtu.be)

## <a name="next-steps"></a><span data-ttu-id="5fa05-283">Następne kroki</span><span class="sxs-lookup"><span data-stu-id="5fa05-283">Next steps</span></span>

<span data-ttu-id="5fa05-284">Przejdź do następnego samouczka z serii:</span><span class="sxs-lookup"><span data-stu-id="5fa05-284">Advance to the next tutorial in the series:</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="5fa05-285">Dodawanie modelu</span><span class="sxs-lookup"><span data-stu-id="5fa05-285">Add a model</span></span>](xref:tutorials/razor-pages/model)

::: moniker-end