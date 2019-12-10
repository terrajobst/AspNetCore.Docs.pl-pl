---
title: 'Samouczek: wprowadzenie do Razor Pages w ASP.NET Core'
author: rick-anderson
description: W tej serii samouczków pokazano, jak używać Razor Pages w ASP.NET Core. Dowiedz się, jak utworzyć model, wygenerować kod dla stron Razor, użyć Entity Framework Core i SQL Server na potrzeby dostępu do danych, dodać funkcję wyszukiwania, dodać sprawdzanie poprawności danych wejściowych i użyć migracji w celu zaktualizowania modelu.
ms.author: riande
ms.date: 11/12/2019
uid: tutorials/razor-pages/razor-pages-start
ms.openlocfilehash: b651437b698d01310f90c5f14832616c1896e6c0
ms.sourcegitcommit: 4e3edff24ba6e43a103fee1b126c9826241bb37b
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 12/10/2019
ms.locfileid: "74959102"
---
# <a name="tutorial-get-started-with-razor-pages-in-aspnet-core"></a><span data-ttu-id="d5709-104">Samouczek: wprowadzenie do Razor Pages w ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="d5709-104">Tutorial: Get started with Razor Pages in ASP.NET Core</span></span>

<span data-ttu-id="d5709-105">Przez [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="d5709-105">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

::: moniker range=">= aspnetcore-3.0"
<span data-ttu-id="d5709-106">Jest to pierwszy samouczek dotyczący serii, który uczy się podstaw tworzenia aplikacji sieci Web Razor Pages ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="d5709-106">This is the first tutorial of a series that teaches the basics of building an ASP.NET Core Razor Pages web app.</span></span>

[!INCLUDE[](~/includes/advancedRP.md)]

<span data-ttu-id="d5709-107">Na końcu serii będziesz mieć aplikację, która zarządza bazą danych filmów.</span><span class="sxs-lookup"><span data-stu-id="d5709-107">At the end of the series, you'll have an app that manages a database of movies.</span></span>  

[!INCLUDE[View or download sample code](~/includes/rp/download.md)]

<span data-ttu-id="d5709-108">W tym samouczku zostaną wykonane następujące czynności:</span><span class="sxs-lookup"><span data-stu-id="d5709-108">In this tutorial, you:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="d5709-109">Utwórz aplikację sieci Web Razor Pages.</span><span class="sxs-lookup"><span data-stu-id="d5709-109">Create a Razor Pages web app.</span></span>
> * <span data-ttu-id="d5709-110">Uruchom aplikację.</span><span class="sxs-lookup"><span data-stu-id="d5709-110">Run the app.</span></span>
> * <span data-ttu-id="d5709-111">Przejrzyj pliki projektu.</span><span class="sxs-lookup"><span data-stu-id="d5709-111">Examine the project files.</span></span>

<span data-ttu-id="d5709-112">Na końcu tego samouczka będziesz mieć działającą Razor Pagesową aplikację internetową, która zostanie wdrożona w kolejnych samouczkach.</span><span class="sxs-lookup"><span data-stu-id="d5709-112">At the end of this tutorial, you'll have a working Razor Pages web app that you'll build on in later tutorials.</span></span>

![Strona główna lub indeks](razor-pages-start/_static/home2.2.png)

## <a name="prerequisites"></a><span data-ttu-id="d5709-114">Wymagania wstępne</span><span class="sxs-lookup"><span data-stu-id="d5709-114">Prerequisites</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="d5709-115">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="d5709-115">Visual Studio</span></span>](#tab/visual-studio)

[!INCLUDE[](~/includes/net-core-prereqs-vs-3.1.md)]

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="d5709-116">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="d5709-116">Visual Studio Code</span></span>](#tab/visual-studio-code)

[!INCLUDE[](~/includes/net-core-prereqs-vsc-3.1.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="d5709-117">Visual Studio dla komputerów Mac</span><span class="sxs-lookup"><span data-stu-id="d5709-117">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

[!INCLUDE[](~/includes/net-core-prereqs-mac-3.1.md)]

---

## <a name="create-a-razor-pages-web-app"></a><span data-ttu-id="d5709-118">Tworzenie aplikacji sieci Web Razor Pages</span><span class="sxs-lookup"><span data-stu-id="d5709-118">Create a Razor Pages web app</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="d5709-119">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="d5709-119">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="d5709-120">W programie Visual Studio **pliku** menu, wybierz opcję **New** > **projektu**.</span><span class="sxs-lookup"><span data-stu-id="d5709-120">From the Visual Studio **File** menu, select **New** > **Project**.</span></span>
* <span data-ttu-id="d5709-121">Utwórz nową aplikację sieci Web ASP.NET Core a następnie wybierz pozycję **dalej**.</span><span class="sxs-lookup"><span data-stu-id="d5709-121">Create a new ASP.NET Core Web Application and select **Next**.</span></span>
  <span data-ttu-id="d5709-122">![nową aplikację sieci Web ASP.NET Core](razor-pages-start/_static/np_2.1.png)</span><span class="sxs-lookup"><span data-stu-id="d5709-122">![new ASP.NET Core Web Application](razor-pages-start/_static/np_2.1.png)</span></span>
* <span data-ttu-id="d5709-123">Nazwij projekt **RazorPagesMovie**.</span><span class="sxs-lookup"><span data-stu-id="d5709-123">Name the project **RazorPagesMovie**.</span></span> <span data-ttu-id="d5709-124">Ważne jest, aby nazwa projektu *RazorPagesMovie* , tak aby przestrzenie nazw były zgodne podczas kopiowania i wklejania kodu.</span><span class="sxs-lookup"><span data-stu-id="d5709-124">It's important to name the project *RazorPagesMovie* so the namespaces will match when you copy and paste code.</span></span>
  <span data-ttu-id="d5709-125">![nową aplikację sieci Web ASP.NET Core](razor-pages-start/_static/config.png)</span><span class="sxs-lookup"><span data-stu-id="d5709-125">![new ASP.NET Core Web Application](razor-pages-start/_static/config.png)</span></span>

* <span data-ttu-id="d5709-126">Wybierz pozycję **ASP.NET Core 3,1** na liście rozwijanej, **aplikacji sieci Web**, a następnie wybierz pozycję **Utwórz**.</span><span class="sxs-lookup"><span data-stu-id="d5709-126">Select **ASP.NET Core 3.1** in the dropdown, **Web Application**, and then select **Create**.</span></span>

![Nowa aplikacja sieci Web ASP.NET Core](razor-pages-start/_static/3/npx.png)

  <span data-ttu-id="d5709-128">Tworzony jest następujący projekt początkowy:</span><span class="sxs-lookup"><span data-stu-id="d5709-128">The following starter project is created:</span></span>

  ![Eksplorator rozwiązań](razor-pages-start/_static/se2.2.png)

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="d5709-130">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="d5709-130">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="d5709-131">Otwórz [zintegrowany terminal](https://code.visualstudio.com/docs/editor/integrated-terminal).</span><span class="sxs-lookup"><span data-stu-id="d5709-131">Open the [integrated terminal](https://code.visualstudio.com/docs/editor/integrated-terminal).</span></span>

* <span data-ttu-id="d5709-132">Przejdź do katalogu (`cd`), który będzie zawierać projekt.</span><span class="sxs-lookup"><span data-stu-id="d5709-132">Change to the directory (`cd`) which will contain the project.</span></span>

* <span data-ttu-id="d5709-133">Uruchom następujące polecenia:</span><span class="sxs-lookup"><span data-stu-id="d5709-133">Run the following commands:</span></span>

  ```dotnetcli
  dotnet new webapp -o RazorPagesMovie
  code -r RazorPagesMovie
  ```

  * <span data-ttu-id="d5709-134">`dotnet new` polecenie tworzy nowy projekt Razor Pages w folderze *RazorPagesMovie*</span><span class="sxs-lookup"><span data-stu-id="d5709-134">The `dotnet new` command creates a new Razor Pages project in the *RazorPagesMovie* folder.</span></span>
  * <span data-ttu-id="d5709-135">`code` polecenie otwiera folder *RazorPagesMovie* w bieżącym wystąpieniu Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="d5709-135">The `code` command opens the *RazorPagesMovie* folder in the current instance of Visual Studio Code.</span></span>

* <span data-ttu-id="d5709-136">Gdy ikona płomienia OmniSharp na pasku stanu zmieni kolor na zielony, w oknie dialogowym zostanie wyświetlony monit **o podanie wymaganych zasobów do skompilowania i debugowania z elementu "RazorPagesMovie". Dodać je?**</span><span class="sxs-lookup"><span data-stu-id="d5709-136">After the status bar's OmniSharp flame icon turns green, a dialog asks **Required assets to build and debug are missing from 'RazorPagesMovie'. Add them?**</span></span> <span data-ttu-id="d5709-137">Wybierz pozycję **Yes**.</span><span class="sxs-lookup"><span data-stu-id="d5709-137">Select **Yes**.</span></span>

  <span data-ttu-id="d5709-138">Katalog *. programu vscode* , zawierający pliki *Launch. JSON* i *Tasks. JSON* , jest dodawany do katalogu głównego projektu.</span><span class="sxs-lookup"><span data-stu-id="d5709-138">A *.vscode* directory, containing *launch.json* and *tasks.json* files, is added to the project's root directory.</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="d5709-139">Visual Studio dla komputerów Mac</span><span class="sxs-lookup"><span data-stu-id="d5709-139">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="d5709-140">Wybierz pozycję **plik** > **nowe rozwiązanie**.</span><span class="sxs-lookup"><span data-stu-id="d5709-140">Select **File** > **New Solution**.</span></span>

![Nowe rozwiązanie w systemie macOS](../first-mvc-app/start-mvc/_static/new_project_vsmac.png)

* <span data-ttu-id="d5709-142">Wybierz pozycję **aplikacja** **sieci > Web** > **.NET Core** > **dalej**.</span><span class="sxs-lookup"><span data-stu-id="d5709-142">Select **.NET Core** > **App** > **Web Application** > **Next**.</span></span>

  ![okno dialogowe z systemem macOS nowego projektu](razor-pages-start/_static/webapp.png)

* <span data-ttu-id="d5709-144">W oknie dialogowym **Konfigurowanie nowego interfejsu API sieci Web ASP.NET Core** Ustaw platformę **docelową** na **.NET Core 3,1**.</span><span class="sxs-lookup"><span data-stu-id="d5709-144">In the **Configure your new ASP.NET Core Web API** dialog, set the  **Target Framework** to **.NET Core 3.1**.</span></span>

  ![wybór macOS .NET Core 3,0](razor-pages-start/_static/targetframework3.png)

* <span data-ttu-id="d5709-146">Nazwij projekt **RazorPagesMovie**, a następnie wybierz pozycję **Utwórz**.</span><span class="sxs-lookup"><span data-stu-id="d5709-146">Name the project **RazorPagesMovie**, and then select **Create**.</span></span>

  ![nameproj](razor-pages-start/_static/RazorPagesMovie.png)


## <a name="open-the-project"></a><span data-ttu-id="d5709-148">Otwórz projekt</span><span class="sxs-lookup"><span data-stu-id="d5709-148">Open the project</span></span>

<span data-ttu-id="d5709-149">W programie Visual Studio wybierz pozycję **plik > Otwórz**, a następnie wybierz plik *RazorPagesMovie. csproj* .</span><span class="sxs-lookup"><span data-stu-id="d5709-149">From Visual Studio, select **File > Open**, and then select the *RazorPagesMovie.csproj* file.</span></span>

<!-- End of VS tabs -->

---

## <a name="run-the-app"></a><span data-ttu-id="d5709-150">Uruchamianie aplikacji</span><span class="sxs-lookup"><span data-stu-id="d5709-150">Run the app</span></span>

  [!INCLUDE[](~/includes/run-the-app.md)]

## <a name="examine-the-project-files"></a><span data-ttu-id="d5709-151">Sprawdzanie plików projektu</span><span class="sxs-lookup"><span data-stu-id="d5709-151">Examine the project files</span></span>

<span data-ttu-id="d5709-152">Poniżej przedstawiono Omówienie folderów i plików projektu głównego, z których będziesz korzystać w kolejnych samouczkach.</span><span class="sxs-lookup"><span data-stu-id="d5709-152">Here's an overview of the main project folders and files that you'll work with in later tutorials.</span></span>

### <a name="pages-folder"></a><span data-ttu-id="d5709-153">Folder stron</span><span class="sxs-lookup"><span data-stu-id="d5709-153">Pages folder</span></span>

<span data-ttu-id="d5709-154">Zawiera strony Razor i pliki pomocnicze.</span><span class="sxs-lookup"><span data-stu-id="d5709-154">Contains Razor pages and supporting files.</span></span> <span data-ttu-id="d5709-155">Każda strona Razor to para plików:</span><span class="sxs-lookup"><span data-stu-id="d5709-155">Each Razor page is a pair of files:</span></span>

* <span data-ttu-id="d5709-156">Plik *. cshtml* , który zawiera znaczniki HTML z C# kodem przy użyciu składnia Razor.</span><span class="sxs-lookup"><span data-stu-id="d5709-156">A *.cshtml* file that contains HTML markup with C# code using Razor syntax.</span></span>
* <span data-ttu-id="d5709-157">Plik *. cshtml.cs* , który zawiera C# kod, który obsługuje zdarzenia strony.</span><span class="sxs-lookup"><span data-stu-id="d5709-157">A *.cshtml.cs* file that contains C# code that handles page events.</span></span>

<span data-ttu-id="d5709-158">Pliki pomocnicze mają nazwy zaczynające się od znaku podkreślenia.</span><span class="sxs-lookup"><span data-stu-id="d5709-158">Supporting files have names that begin with an underscore.</span></span> <span data-ttu-id="d5709-159">Na przykład plik *_Layout. cshtml* służy do konfigurowania elementów interfejsu użytkownika wspólnych dla wszystkich stron.</span><span class="sxs-lookup"><span data-stu-id="d5709-159">For example, the *_Layout.cshtml* file configures UI elements common to all pages.</span></span> <span data-ttu-id="d5709-160">Ten plik konfiguruje menu nawigacji w górnej części strony i informacje o prawach autorskich w dolnej części strony.</span><span class="sxs-lookup"><span data-stu-id="d5709-160">This file sets up the navigation menu at the top of the page and the copyright notice at the bottom of the page.</span></span> <span data-ttu-id="d5709-161">Aby uzyskać więcej informacji, zobacz temat <xref:mvc/views/layout>.</span><span class="sxs-lookup"><span data-stu-id="d5709-161">For more information, see <xref:mvc/views/layout>.</span></span>

### <a name="wwwroot-folder"></a><span data-ttu-id="d5709-162">folder wwwroot</span><span class="sxs-lookup"><span data-stu-id="d5709-162">wwwroot folder</span></span>

<span data-ttu-id="d5709-163">Zawiera pliki statyczne, takie jak pliki HTML, pliki JavaScript i pliki CSS.</span><span class="sxs-lookup"><span data-stu-id="d5709-163">Contains static files, such as HTML files, JavaScript files, and CSS files.</span></span> <span data-ttu-id="d5709-164">Aby uzyskać więcej informacji, zobacz temat <xref:fundamentals/static-files>.</span><span class="sxs-lookup"><span data-stu-id="d5709-164">For more information, see <xref:fundamentals/static-files>.</span></span>

### <a name="appsettingsjson"></a><span data-ttu-id="d5709-165">appSettings.json</span><span class="sxs-lookup"><span data-stu-id="d5709-165">appSettings.json</span></span>

<span data-ttu-id="d5709-166">Zawiera dane konfiguracyjne, takie jak parametry połączenia.</span><span class="sxs-lookup"><span data-stu-id="d5709-166">Contains configuration data, such as connection strings.</span></span> <span data-ttu-id="d5709-167">Aby uzyskać więcej informacji, zobacz temat <xref:fundamentals/configuration/index>.</span><span class="sxs-lookup"><span data-stu-id="d5709-167">For more information, see <xref:fundamentals/configuration/index>.</span></span>

### <a name="programcs"></a><span data-ttu-id="d5709-168">Program.cs</span><span class="sxs-lookup"><span data-stu-id="d5709-168">Program.cs</span></span>

<span data-ttu-id="d5709-169">Zawiera punkt wejścia dla programu.</span><span class="sxs-lookup"><span data-stu-id="d5709-169">Contains the entry point for the program.</span></span> <span data-ttu-id="d5709-170">Aby uzyskać więcej informacji, zobacz temat <xref:fundamentals/host/generic-host>.</span><span class="sxs-lookup"><span data-stu-id="d5709-170">For more information, see <xref:fundamentals/host/generic-host>.</span></span>

### <a name="startupcs"></a><span data-ttu-id="d5709-171">Startup.cs</span><span class="sxs-lookup"><span data-stu-id="d5709-171">Startup.cs</span></span>

<span data-ttu-id="d5709-172">Zawiera kod, który konfiguruje zachowanie aplikacji.</span><span class="sxs-lookup"><span data-stu-id="d5709-172">Contains code that configures app behavior.</span></span> <span data-ttu-id="d5709-173">Aby uzyskać więcej informacji, zobacz temat <xref:fundamentals/startup>.</span><span class="sxs-lookup"><span data-stu-id="d5709-173">For more information, see <xref:fundamentals/startup>.</span></span>

## <a name="next-steps"></a><span data-ttu-id="d5709-174">Następne kroki</span><span class="sxs-lookup"><span data-stu-id="d5709-174">Next steps</span></span>

<span data-ttu-id="d5709-175">Przejdź do następnego samouczka z serii:</span><span class="sxs-lookup"><span data-stu-id="d5709-175">Advance to the next tutorial in the series:</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="d5709-176">Dodawanie modelu</span><span class="sxs-lookup"><span data-stu-id="d5709-176">Add a model</span></span>](xref:tutorials/razor-pages/model)

::: moniker-end

<!--::: moniker range=">= aspnetcore-3.0" -->

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="d5709-177">Jest to pierwszy samouczek dotyczący serii.</span><span class="sxs-lookup"><span data-stu-id="d5709-177">This is the first tutorial of a series.</span></span> <span data-ttu-id="d5709-178">[Seria zawiera](xref:tutorials/razor-pages/index) podstawowe informacje na temat tworzenia aplikacji sieci web ASP.NET Core Razor Pages.</span><span class="sxs-lookup"><span data-stu-id="d5709-178">[The series](xref:tutorials/razor-pages/index) teaches the basics of building an ASP.NET Core Razor Pages web app.</span></span>

[!INCLUDE[](~/includes/advancedRP.md)]

<span data-ttu-id="d5709-179">Na końcu serii będziesz mieć aplikację, która zarządza bazą danych filmów.</span><span class="sxs-lookup"><span data-stu-id="d5709-179">At the end of the series, you'll have an app that manages a database of movies.</span></span>  

[!INCLUDE[View or download sample code](~/includes/rp/download.md)]

<span data-ttu-id="d5709-180">W tym samouczku zostaną wykonane następujące czynności:</span><span class="sxs-lookup"><span data-stu-id="d5709-180">In this tutorial, you:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="d5709-181">Utwórz aplikację sieci Web Razor Pages.</span><span class="sxs-lookup"><span data-stu-id="d5709-181">Create a Razor Pages web app.</span></span>
> * <span data-ttu-id="d5709-182">Uruchom aplikację.</span><span class="sxs-lookup"><span data-stu-id="d5709-182">Run the app.</span></span>
> * <span data-ttu-id="d5709-183">Przejrzyj pliki projektu.</span><span class="sxs-lookup"><span data-stu-id="d5709-183">Examine the project files.</span></span>

<span data-ttu-id="d5709-184">Na końcu tego samouczka będziesz mieć działającą Razor Pagesową aplikację internetową, która zostanie wdrożona w kolejnych samouczkach.</span><span class="sxs-lookup"><span data-stu-id="d5709-184">At the end of this tutorial, you'll have a working Razor Pages web app that you'll build on in later tutorials.</span></span>

![Strona główna lub indeks](razor-pages-start/_static/home2.2.png)

## <a name="prerequisites"></a><span data-ttu-id="d5709-186">Wymagania wstępne</span><span class="sxs-lookup"><span data-stu-id="d5709-186">Prerequisites</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="d5709-187">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="d5709-187">Visual Studio</span></span>](#tab/visual-studio)

[!INCLUDE[](~/includes/net-core-prereqs-vs2019-2.2.md)]

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="d5709-188">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="d5709-188">Visual Studio Code</span></span>](#tab/visual-studio-code)

[!INCLUDE[](~/includes/net-core-prereqs-vsc-2.2.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="d5709-189">Visual Studio dla komputerów Mac</span><span class="sxs-lookup"><span data-stu-id="d5709-189">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

[!INCLUDE[](~/includes/net-core-prereqs-mac-2.2.md)]

---

## <a name="create-a-razor-pages-web-app"></a><span data-ttu-id="d5709-190">Tworzenie aplikacji sieci Web Razor Pages</span><span class="sxs-lookup"><span data-stu-id="d5709-190">Create a Razor Pages web app</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="d5709-191">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="d5709-191">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="d5709-192">W programie Visual Studio **pliku** menu, wybierz opcję **New** > **projektu**.</span><span class="sxs-lookup"><span data-stu-id="d5709-192">From the Visual Studio **File** menu, select **New** > **Project**.</span></span>

* <span data-ttu-id="d5709-193">Utwórz nową aplikację sieci Web ASP.NET Core a następnie wybierz pozycję **dalej**.</span><span class="sxs-lookup"><span data-stu-id="d5709-193">Create a new ASP.NET Core Web Application and select **Next**.</span></span>

  ![Nowa aplikacja sieci Web ASP.NET Core](razor-pages-start/_static/np_2.1.png)

* <span data-ttu-id="d5709-195">Nazwij projekt **RazorPagesMovie**.</span><span class="sxs-lookup"><span data-stu-id="d5709-195">Name the project **RazorPagesMovie**.</span></span> <span data-ttu-id="d5709-196">Ważne jest, aby nazwa projektu *RazorPagesMovie* , tak aby przestrzenie nazw były zgodne podczas kopiowania i wklejania kodu.</span><span class="sxs-lookup"><span data-stu-id="d5709-196">It's important to name the project *RazorPagesMovie* so the namespaces will match when you copy and paste code.</span></span>

  ![Nowa aplikacja sieci Web ASP.NET Core](razor-pages-start/_static/config.png)

* <span data-ttu-id="d5709-198">Wybierz pozycję **ASP.NET Core 2,2** na liście rozwijanej, **aplikacji sieci Web**, a następnie wybierz pozycję **Utwórz**.</span><span class="sxs-lookup"><span data-stu-id="d5709-198">Select **ASP.NET Core 2.2** in the dropdown, **Web Application**, and then select **Create**.</span></span>

![Nowa aplikacja sieci Web ASP.NET Core](razor-pages-start/_static/np_2_2.2.png)

  <span data-ttu-id="d5709-200">Tworzony jest następujący projekt początkowy:</span><span class="sxs-lookup"><span data-stu-id="d5709-200">The following starter project is created:</span></span>

  ![Eksplorator rozwiązań](razor-pages-start/_static/se2.2.png)

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="d5709-202">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="d5709-202">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="d5709-203">Otwórz [zintegrowany terminal](https://code.visualstudio.com/docs/editor/integrated-terminal).</span><span class="sxs-lookup"><span data-stu-id="d5709-203">Open the [integrated terminal](https://code.visualstudio.com/docs/editor/integrated-terminal).</span></span>

* <span data-ttu-id="d5709-204">Przejdź do katalogu (`cd`), który będzie zawierać projekt.</span><span class="sxs-lookup"><span data-stu-id="d5709-204">Change to the directory (`cd`) which will contain the project.</span></span>

* <span data-ttu-id="d5709-205">Uruchom następujące polecenia:</span><span class="sxs-lookup"><span data-stu-id="d5709-205">Run the following commands:</span></span>

  ```dotnetcli
  dotnet new webapp -o RazorPagesMovie
  code -r RazorPagesMovie
  ```

  * <span data-ttu-id="d5709-206">`dotnet new` polecenie tworzy nowy projekt Razor Pages w folderze *RazorPagesMovie*</span><span class="sxs-lookup"><span data-stu-id="d5709-206">The `dotnet new` command creates a new Razor Pages project in the *RazorPagesMovie* folder.</span></span>
  * <span data-ttu-id="d5709-207">`code` polecenie otwiera folder *RazorPagesMovie* w bieżącym wystąpieniu Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="d5709-207">The `code` command opens the *RazorPagesMovie* folder in the current instance of Visual Studio Code.</span></span>

* <span data-ttu-id="d5709-208">Gdy ikona płomienia OmniSharp na pasku stanu zmieni kolor na zielony, w oknie dialogowym zostanie wyświetlony monit **o podanie wymaganych zasobów do skompilowania i debugowania z elementu "RazorPagesMovie". Dodać je?**</span><span class="sxs-lookup"><span data-stu-id="d5709-208">After the status bar's OmniSharp flame icon turns green, a dialog asks **Required assets to build and debug are missing from 'RazorPagesMovie'. Add them?**</span></span> <span data-ttu-id="d5709-209">Wybierz pozycję **Yes**.</span><span class="sxs-lookup"><span data-stu-id="d5709-209">Select **Yes**.</span></span>

  <span data-ttu-id="d5709-210">Katalog *. programu vscode* , zawierający pliki *Launch. JSON* i *Tasks. JSON* , jest dodawany do katalogu głównego projektu.</span><span class="sxs-lookup"><span data-stu-id="d5709-210">A *.vscode* directory, containing *launch.json* and *tasks.json* files, is added to the project's root directory.</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="d5709-211">Visual Studio dla komputerów Mac</span><span class="sxs-lookup"><span data-stu-id="d5709-211">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="d5709-212">W terminalu uruchom następujące polecenie:</span><span class="sxs-lookup"><span data-stu-id="d5709-212">From a terminal, run the following command:</span></span>

<!-- TODO: update these instruction once mac support 2.2 projects -->

```dotnetcli
dotnet new webapp -o RazorPagesMovie
```

<span data-ttu-id="d5709-213">Poprzednie polecenia używają [interfejs wiersza polecenia platformy .NET Core](/dotnet/core/tools/dotnet) do tworzenia projektu Razor Pages.</span><span class="sxs-lookup"><span data-stu-id="d5709-213">The preceding commands use the [.NET Core CLI](/dotnet/core/tools/dotnet) to create a Razor Pages project.</span></span>

## <a name="open-the-project"></a><span data-ttu-id="d5709-214">Otwórz projekt</span><span class="sxs-lookup"><span data-stu-id="d5709-214">Open the project</span></span>

<span data-ttu-id="d5709-215">W programie Visual Studio wybierz pozycję **plik > Otwórz**, a następnie wybierz plik *RazorPagesMovie. csproj* .</span><span class="sxs-lookup"><span data-stu-id="d5709-215">From Visual Studio, select **File > Open**, and then select the *RazorPagesMovie.csproj* file.</span></span>

<!-- End of VS tabs -->

---

## <a name="run-the-app"></a><span data-ttu-id="d5709-216">Uruchamianie aplikacji</span><span class="sxs-lookup"><span data-stu-id="d5709-216">Run the app</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="d5709-217">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="d5709-217">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="d5709-218">Naciśnij klawisze CTRL + F5, aby uruchomić bez debugera.</span><span class="sxs-lookup"><span data-stu-id="d5709-218">Press Ctrl+F5 to run without the debugger.</span></span>

  [!INCLUDE[](~/includes/trustCertVS.md)]

  <span data-ttu-id="d5709-219">Program Visual Studio jest uruchamiany [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) i uruchomi aplikację.</span><span class="sxs-lookup"><span data-stu-id="d5709-219">Visual Studio starts [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) and runs the app.</span></span> <span data-ttu-id="d5709-220">Na pasku adresu są wyświetlane `localhost:port#` a nie elementy, takie jak `example.com`.</span><span class="sxs-lookup"><span data-stu-id="d5709-220">The address bar shows `localhost:port#` and not something like `example.com`.</span></span> <span data-ttu-id="d5709-221">Wynika to z faktu, że `localhost` jest standardową nazwą hosta dla komputera lokalnego.</span><span class="sxs-lookup"><span data-stu-id="d5709-221">That's because `localhost` is the standard hostname for the local computer.</span></span> <span data-ttu-id="d5709-222">Host lokalny obsługuje tylko żądania sieci Web z komputera lokalnego.</span><span class="sxs-lookup"><span data-stu-id="d5709-222">Localhost only serves web requests from the local computer.</span></span> <span data-ttu-id="d5709-223">Podczas tworzenia projektu internetowego w programie Visual Studio dla serwera internetowego jest używany losowy port.</span><span class="sxs-lookup"><span data-stu-id="d5709-223">When Visual Studio creates a web project, a random port is used for the web server.</span></span>

* <span data-ttu-id="d5709-224">Na stronie głównej aplikacji wybierz pozycję **Akceptuj** , aby wyrazić zgodę na śledzenie.</span><span class="sxs-lookup"><span data-stu-id="d5709-224">On the app's home page, select **Accept** to consent to tracking.</span></span>

  <span data-ttu-id="d5709-225">Ta aplikacja nie śledzi informacji osobistych, ale szablon projektu zawiera funkcję zgody na wypadek, gdyby była niezbędna do przestrzegania Ogólne rozporządzenie o ochronie danych Unii Europejskiej [(Rodo)](xref:security/gdpr).</span><span class="sxs-lookup"><span data-stu-id="d5709-225">This app doesn't track personal information, but the project template includes the consent feature in case you need it to comply with the European Union's [General Data Protection Regulation (GDPR)](xref:security/gdpr).</span></span>

  ![Strona główna lub indeks](razor-pages-start/_static/homeGDPR2.2.png)

  <span data-ttu-id="d5709-227">Na poniższej ilustracji przedstawiono aplikację po udzieleniu zgody na śledzenie:</span><span class="sxs-lookup"><span data-stu-id="d5709-227">The following image shows the app after you give consent to tracking:</span></span>

  ![Strona główna lub indeks](razor-pages-start/_static/home2.2.png)
  
# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="d5709-229">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="d5709-229">Visual Studio Code</span></span>](#tab/visual-studio-code)

  [!INCLUDE[](~/includes/trustCertVSC.md)]

* <span data-ttu-id="d5709-230">Naciśnij **klawisze CTRL + F5** , aby uruchomić bez debugera.</span><span class="sxs-lookup"><span data-stu-id="d5709-230">Press **Ctrl-F5** to run without the debugger.</span></span>

  <span data-ttu-id="d5709-231">Visual Studio Code uruchamia [Kestrel](xref:fundamentals/servers/kestrel), uruchamia przeglądarkę i przechodzi do `http://localhost:5001`.</span><span class="sxs-lookup"><span data-stu-id="d5709-231">Visual Studio Code starts [Kestrel](xref:fundamentals/servers/kestrel), launches a browser, and navigates to `http://localhost:5001`.</span></span> <span data-ttu-id="d5709-232">Na pasku adresu są wyświetlane `localhost:port#` a nie elementy, takie jak `example.com`.</span><span class="sxs-lookup"><span data-stu-id="d5709-232">The address bar shows `localhost:port#` and not something like `example.com`.</span></span> <span data-ttu-id="d5709-233">Wynika to z faktu, że `localhost` jest standardową nazwą hosta dla komputera lokalnego.</span><span class="sxs-lookup"><span data-stu-id="d5709-233">That's because `localhost` is the standard hostname for  local computer.</span></span> <span data-ttu-id="d5709-234">Host lokalny obsługuje tylko żądania sieci Web z komputera lokalnego.</span><span class="sxs-lookup"><span data-stu-id="d5709-234">Localhost only serves web requests from the local computer.</span></span>

* <span data-ttu-id="d5709-235">Na stronie głównej aplikacji wybierz pozycję **Akceptuj** , aby wyrazić zgodę na śledzenie.</span><span class="sxs-lookup"><span data-stu-id="d5709-235">On the app's home page, select **Accept** to consent to tracking.</span></span>

  <span data-ttu-id="d5709-236">Ta aplikacja nie śledzi informacji osobistych, ale szablon projektu zawiera funkcję zgody na wypadek, gdyby była niezbędna do przestrzegania Ogólne rozporządzenie o ochronie danych Unii Europejskiej [(Rodo)](xref:security/gdpr).</span><span class="sxs-lookup"><span data-stu-id="d5709-236">This app doesn't track personal information, but the project template includes the consent feature in case you need it to comply with the European Union's [General Data Protection Regulation (GDPR)](xref:security/gdpr).</span></span>

  ![Strona główna lub indeks](razor-pages-start/_static/homeGDPR2.2.png)

  <span data-ttu-id="d5709-238">Na poniższej ilustracji przedstawiono aplikację po udzieleniu zgody na śledzenie:</span><span class="sxs-lookup"><span data-stu-id="d5709-238">The following image shows the app after you give consent to tracking:</span></span>

  ![Strona główna lub indeks](razor-pages-start/_static/home2.2.png)
  
# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="d5709-240">Visual Studio dla komputerów Mac</span><span class="sxs-lookup"><span data-stu-id="d5709-240">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

  [!INCLUDE[](~/includes/trustCertMac.md)]

* <span data-ttu-id="d5709-241">Naciśnij **polecenie cmd-opt-F5** , aby uruchomić program bez debugera.</span><span class="sxs-lookup"><span data-stu-id="d5709-241">Press **Cmd-Opt-F5** to run without the debugger.</span></span>

  <span data-ttu-id="d5709-242">Program Visual Studio uruchamia [Kestrel](xref:fundamentals/servers/kestrel), uruchamia przeglądarkę i przechodzi do `http://localhost:5001`.</span><span class="sxs-lookup"><span data-stu-id="d5709-242">Visual Studio starts [Kestrel](xref:fundamentals/servers/kestrel), launches a browser, and navigates to `http://localhost:5001`.</span></span>

* <span data-ttu-id="d5709-243">Na stronie głównej aplikacji wybierz pozycję **Akceptuj** , aby wyrazić zgodę na śledzenie.</span><span class="sxs-lookup"><span data-stu-id="d5709-243">On the app's home page, select **Accept** to consent to tracking.</span></span>

  <span data-ttu-id="d5709-244">Ta aplikacja nie śledzi informacji osobistych, ale szablon projektu zawiera funkcję zgody na wypadek, gdyby była niezbędna do przestrzegania Ogólne rozporządzenie o ochronie danych Unii Europejskiej [(Rodo)](xref:security/gdpr).</span><span class="sxs-lookup"><span data-stu-id="d5709-244">This app doesn't track personal information, but the project template includes the consent feature in case you need it to comply with the European Union's [General Data Protection Regulation (GDPR)](xref:security/gdpr).</span></span>

  ![Strona główna lub indeks](razor-pages-start/_static/homeGDPR2.2_safari.png)

  <span data-ttu-id="d5709-246">Na poniższej ilustracji przedstawiono aplikację po udzieleniu zgody na śledzenie:</span><span class="sxs-lookup"><span data-stu-id="d5709-246">The following image shows the app after you give consent to tracking:</span></span>

  ![Strona główna lub indeks](razor-pages-start/_static/home2.2_safari.png)

<!-- End of VS tabs -->

---

## <a name="examine-the-project-files"></a><span data-ttu-id="d5709-248">Sprawdzanie plików projektu</span><span class="sxs-lookup"><span data-stu-id="d5709-248">Examine the project files</span></span>

<span data-ttu-id="d5709-249">Poniżej przedstawiono Omówienie folderów i plików projektu głównego, z których będziesz korzystać w kolejnych samouczkach.</span><span class="sxs-lookup"><span data-stu-id="d5709-249">Here's an overview of the main project folders and files that you'll work with in later tutorials.</span></span>

### <a name="pages-folder"></a><span data-ttu-id="d5709-250">Folder stron</span><span class="sxs-lookup"><span data-stu-id="d5709-250">Pages folder</span></span>

<span data-ttu-id="d5709-251">Zawiera strony Razor i pliki pomocnicze.</span><span class="sxs-lookup"><span data-stu-id="d5709-251">Contains Razor pages and supporting files.</span></span> <span data-ttu-id="d5709-252">Każda strona Razor to para plików:</span><span class="sxs-lookup"><span data-stu-id="d5709-252">Each Razor page is a pair of files:</span></span>

* <span data-ttu-id="d5709-253">Plik *. cshtml* , który zawiera znaczniki HTML z C# kodem przy użyciu składnia Razor.</span><span class="sxs-lookup"><span data-stu-id="d5709-253">A *.cshtml* file that contains HTML markup with C# code using Razor syntax.</span></span>
* <span data-ttu-id="d5709-254">Plik *. cshtml.cs* , który zawiera C# kod, który obsługuje zdarzenia strony.</span><span class="sxs-lookup"><span data-stu-id="d5709-254">A *.cshtml.cs* file that contains C# code that handles page events.</span></span>

<span data-ttu-id="d5709-255">Pliki pomocnicze mają nazwy zaczynające się od znaku podkreślenia.</span><span class="sxs-lookup"><span data-stu-id="d5709-255">Supporting files have names that begin with an underscore.</span></span> <span data-ttu-id="d5709-256">Na przykład plik *_Layout. cshtml* służy do konfigurowania elementów interfejsu użytkownika wspólnych dla wszystkich stron.</span><span class="sxs-lookup"><span data-stu-id="d5709-256">For example, the *_Layout.cshtml* file configures UI elements common to all pages.</span></span> <span data-ttu-id="d5709-257">Ten plik konfiguruje menu nawigacji w górnej części strony i informacje o prawach autorskich w dolnej części strony.</span><span class="sxs-lookup"><span data-stu-id="d5709-257">This file sets up the navigation menu at the top of the page and the copyright notice at the bottom of the page.</span></span> <span data-ttu-id="d5709-258">Aby uzyskać więcej informacji, zobacz temat <xref:mvc/views/layout>.</span><span class="sxs-lookup"><span data-stu-id="d5709-258">For more information, see <xref:mvc/views/layout>.</span></span>

### <a name="wwwroot-folder"></a><span data-ttu-id="d5709-259">folder wwwroot</span><span class="sxs-lookup"><span data-stu-id="d5709-259">wwwroot folder</span></span>

<span data-ttu-id="d5709-260">Zawiera pliki statyczne, takie jak pliki HTML, pliki JavaScript i pliki CSS.</span><span class="sxs-lookup"><span data-stu-id="d5709-260">Contains static files, such as HTML files, JavaScript files, and CSS files.</span></span> <span data-ttu-id="d5709-261">Aby uzyskać więcej informacji, zobacz temat <xref:fundamentals/static-files>.</span><span class="sxs-lookup"><span data-stu-id="d5709-261">For more information, see <xref:fundamentals/static-files>.</span></span>

### <a name="appsettingsjson"></a><span data-ttu-id="d5709-262">appSettings.json</span><span class="sxs-lookup"><span data-stu-id="d5709-262">appSettings.json</span></span>

<span data-ttu-id="d5709-263">Zawiera dane konfiguracyjne, takie jak parametry połączenia.</span><span class="sxs-lookup"><span data-stu-id="d5709-263">Contains configuration data, such as connection strings.</span></span> <span data-ttu-id="d5709-264">Aby uzyskać więcej informacji, zobacz temat <xref:fundamentals/configuration/index>.</span><span class="sxs-lookup"><span data-stu-id="d5709-264">For more information, see <xref:fundamentals/configuration/index>.</span></span>

### <a name="programcs"></a><span data-ttu-id="d5709-265">Program.cs</span><span class="sxs-lookup"><span data-stu-id="d5709-265">Program.cs</span></span>

<span data-ttu-id="d5709-266">Zawiera punkt wejścia dla programu.</span><span class="sxs-lookup"><span data-stu-id="d5709-266">Contains the entry point for the program.</span></span> <span data-ttu-id="d5709-267">Aby uzyskać więcej informacji, zobacz temat <xref:fundamentals/host/generic-host>.</span><span class="sxs-lookup"><span data-stu-id="d5709-267">For more information, see <xref:fundamentals/host/generic-host>.</span></span>

### <a name="startupcs"></a><span data-ttu-id="d5709-268">Startup.cs</span><span class="sxs-lookup"><span data-stu-id="d5709-268">Startup.cs</span></span>

<span data-ttu-id="d5709-269">Zawiera kod, który konfiguruje zachowanie aplikacji, na przykład czy wymaga zgody na pliki cookie.</span><span class="sxs-lookup"><span data-stu-id="d5709-269">Contains code that configures app behavior, such as whether it requires consent for cookies.</span></span> <span data-ttu-id="d5709-270">Aby uzyskać więcej informacji, zobacz temat <xref:fundamentals/startup>.</span><span class="sxs-lookup"><span data-stu-id="d5709-270">For more information, see <xref:fundamentals/startup>.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="d5709-271">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="d5709-271">Additional resources</span></span>

* [<span data-ttu-id="d5709-272">Wersja tego samouczka usługi YouTube</span><span class="sxs-lookup"><span data-stu-id="d5709-272">Youtube version of this tutorial</span></span>](https://www.youtube.com/watch?v=F0SP7Ry4flQ&feature=youtu.be)

## <a name="next-steps"></a><span data-ttu-id="d5709-273">Następne kroki</span><span class="sxs-lookup"><span data-stu-id="d5709-273">Next steps</span></span>

<span data-ttu-id="d5709-274">Przejdź do następnego samouczka z serii:</span><span class="sxs-lookup"><span data-stu-id="d5709-274">Advance to the next tutorial in the series:</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="d5709-275">Dodawanie modelu</span><span class="sxs-lookup"><span data-stu-id="d5709-275">Add a model</span></span>](xref:tutorials/razor-pages/model)

::: moniker-end
