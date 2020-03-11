---
title: 'Samouczek: wprowadzenie do Razor Pages w ASP.NET Core'
author: rick-anderson
description: W tej serii samouczków pokazano, jak używać Razor Pages w ASP.NET Core. Dowiedz się, jak utworzyć model, wygenerować kod dla stron Razor, użyć Entity Framework Core i SQL Server na potrzeby dostępu do danych, dodać funkcję wyszukiwania, dodać sprawdzanie poprawności danych wejściowych i użyć migracji w celu zaktualizowania modelu.
ms.author: riande
ms.date: 11/12/2019
uid: tutorials/razor-pages/razor-pages-start
ms.openlocfilehash: 6e1d58ccd83f7d7c1083dc2bf9ce7476650812a1
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78658544"
---
# <a name="tutorial-get-started-with-razor-pages-in-aspnet-core"></a><span data-ttu-id="2628e-104">Samouczek: wprowadzenie do Razor Pages w ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="2628e-104">Tutorial: Get started with Razor Pages in ASP.NET Core</span></span>

<span data-ttu-id="2628e-105">Autor: [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="2628e-105">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

::: moniker range=">= aspnetcore-3.0"
<span data-ttu-id="2628e-106">Jest to pierwszy samouczek dotyczący serii, który uczy się podstaw tworzenia aplikacji sieci Web Razor Pages ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="2628e-106">This is the first tutorial of a series that teaches the basics of building an ASP.NET Core Razor Pages web app.</span></span>

[!INCLUDE[](~/includes/advancedRP.md)]

<span data-ttu-id="2628e-107">Na końcu serii będziesz mieć aplikację, która zarządza bazą danych filmów.</span><span class="sxs-lookup"><span data-stu-id="2628e-107">At the end of the series, you'll have an app that manages a database of movies.</span></span>  

[!INCLUDE[View or download sample code](~/includes/rp/download.md)]

<span data-ttu-id="2628e-108">W tym samouczku zostaną wykonane następujące czynności:</span><span class="sxs-lookup"><span data-stu-id="2628e-108">In this tutorial, you:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="2628e-109">Utwórz aplikację sieci Web Razor Pages.</span><span class="sxs-lookup"><span data-stu-id="2628e-109">Create a Razor Pages web app.</span></span>
> * <span data-ttu-id="2628e-110">Uruchom aplikację.</span><span class="sxs-lookup"><span data-stu-id="2628e-110">Run the app.</span></span>
> * <span data-ttu-id="2628e-111">Przejrzyj pliki projektu.</span><span class="sxs-lookup"><span data-stu-id="2628e-111">Examine the project files.</span></span>

<span data-ttu-id="2628e-112">Na końcu tego samouczka będziesz mieć działającą Razor Pagesową aplikację internetową, która zostanie wdrożona w kolejnych samouczkach.</span><span class="sxs-lookup"><span data-stu-id="2628e-112">At the end of this tutorial, you'll have a working Razor Pages web app that you'll build on in later tutorials.</span></span>

![Strona główna lub indeks](razor-pages-start/_static/home2.2.png)

## <a name="prerequisites"></a><span data-ttu-id="2628e-114">Wymagania wstępne</span><span class="sxs-lookup"><span data-stu-id="2628e-114">Prerequisites</span></span>

# <a name="visual-studio"></a>[<span data-ttu-id="2628e-115">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="2628e-115">Visual Studio</span></span>](#tab/visual-studio)

[!INCLUDE[](~/includes/net-core-prereqs-vs-3.1.md)]

# <a name="visual-studio-code"></a>[<span data-ttu-id="2628e-116">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="2628e-116">Visual Studio Code</span></span>](#tab/visual-studio-code)

[!INCLUDE[](~/includes/net-core-prereqs-vsc-3.1.md)]

# <a name="visual-studio-for-mac"></a>[<span data-ttu-id="2628e-117">Visual Studio dla komputerów Mac</span><span class="sxs-lookup"><span data-stu-id="2628e-117">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

[!INCLUDE[](~/includes/net-core-prereqs-mac-3.1.md)]

---

## <a name="create-a-razor-pages-web-app"></a><span data-ttu-id="2628e-118">Tworzenie aplikacji sieci Web Razor Pages</span><span class="sxs-lookup"><span data-stu-id="2628e-118">Create a Razor Pages web app</span></span>

# <a name="visual-studio"></a>[<span data-ttu-id="2628e-119">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="2628e-119">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="2628e-120">Z menu **plik** programu Visual Studio wybierz pozycję **Nowy** **projekt**>.</span><span class="sxs-lookup"><span data-stu-id="2628e-120">From the Visual Studio **File** menu, select **New** > **Project**.</span></span>
* <span data-ttu-id="2628e-121">Utwórz nową aplikację sieci Web ASP.NET Core a następnie wybierz pozycję **dalej**.</span><span class="sxs-lookup"><span data-stu-id="2628e-121">Create a new ASP.NET Core Web Application and select **Next**.</span></span>
  <span data-ttu-id="2628e-122">![nową aplikację sieci Web ASP.NET Core](razor-pages-start/_static/np_2.1.png)</span><span class="sxs-lookup"><span data-stu-id="2628e-122">![new ASP.NET Core Web Application](razor-pages-start/_static/np_2.1.png)</span></span>
* <span data-ttu-id="2628e-123">Nazwij projekt **RazorPagesMovie**.</span><span class="sxs-lookup"><span data-stu-id="2628e-123">Name the project **RazorPagesMovie**.</span></span> <span data-ttu-id="2628e-124">Ważne jest, aby nazwa projektu *RazorPagesMovie* , tak aby przestrzenie nazw były zgodne podczas kopiowania i wklejania kodu.</span><span class="sxs-lookup"><span data-stu-id="2628e-124">It's important to name the project *RazorPagesMovie* so the namespaces will match when you copy and paste code.</span></span>
  <span data-ttu-id="2628e-125">![nową aplikację sieci Web ASP.NET Core](razor-pages-start/_static/config.png)</span><span class="sxs-lookup"><span data-stu-id="2628e-125">![new ASP.NET Core Web Application](razor-pages-start/_static/config.png)</span></span>

* <span data-ttu-id="2628e-126">Wybierz pozycję **ASP.NET Core 3,1** na liście rozwijanej, **aplikacji sieci Web**, a następnie wybierz pozycję **Utwórz**.</span><span class="sxs-lookup"><span data-stu-id="2628e-126">Select **ASP.NET Core 3.1** in the dropdown, **Web Application**, and then select **Create**.</span></span>

![Nowa aplikacja sieci Web ASP.NET Core](razor-pages-start/_static/3/npx.png)

  <span data-ttu-id="2628e-128">Tworzony jest następujący projekt początkowy:</span><span class="sxs-lookup"><span data-stu-id="2628e-128">The following starter project is created:</span></span>

  ![Eksplorator rozwiązań](razor-pages-start/_static/se2.2.png)

# <a name="visual-studio-code"></a>[<span data-ttu-id="2628e-130">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="2628e-130">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="2628e-131">Otwórz [zintegrowany terminal](https://code.visualstudio.com/docs/editor/integrated-terminal).</span><span class="sxs-lookup"><span data-stu-id="2628e-131">Open the [integrated terminal](https://code.visualstudio.com/docs/editor/integrated-terminal).</span></span>

* <span data-ttu-id="2628e-132">Przejdź do katalogu (`cd`), który będzie zawierać projekt.</span><span class="sxs-lookup"><span data-stu-id="2628e-132">Change to the directory (`cd`) which will contain the project.</span></span>

* <span data-ttu-id="2628e-133">Uruchom następujące polecenia:</span><span class="sxs-lookup"><span data-stu-id="2628e-133">Run the following commands:</span></span>

  ```dotnetcli
  dotnet new webapp -o RazorPagesMovie
  code -r RazorPagesMovie
  ```

  * <span data-ttu-id="2628e-134">`dotnet new` polecenie tworzy nowy projekt Razor Pages w folderze *RazorPagesMovie*</span><span class="sxs-lookup"><span data-stu-id="2628e-134">The `dotnet new` command creates a new Razor Pages project in the *RazorPagesMovie* folder.</span></span>
  * <span data-ttu-id="2628e-135">`code` polecenie otwiera folder *RazorPagesMovie* w bieżącym wystąpieniu Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="2628e-135">The `code` command opens the *RazorPagesMovie* folder in the current instance of Visual Studio Code.</span></span>

* <span data-ttu-id="2628e-136">Gdy ikona płomienia OmniSharp na pasku stanu zmieni kolor na zielony, w oknie dialogowym zostanie wyświetlony monit **o podanie wymaganych zasobów do skompilowania i debugowania z elementu "RazorPagesMovie". Dodać je?**</span><span class="sxs-lookup"><span data-stu-id="2628e-136">After the status bar's OmniSharp flame icon turns green, a dialog asks **Required assets to build and debug are missing from 'RazorPagesMovie'. Add them?**</span></span> <span data-ttu-id="2628e-137">Wybierz pozycję **Tak**.</span><span class="sxs-lookup"><span data-stu-id="2628e-137">Select **Yes**.</span></span>

  <span data-ttu-id="2628e-138">Katalog *. programu vscode* , zawierający pliki *Launch. JSON* i *Tasks. JSON* , jest dodawany do katalogu głównego projektu.</span><span class="sxs-lookup"><span data-stu-id="2628e-138">A *.vscode* directory, containing *launch.json* and *tasks.json* files, is added to the project's root directory.</span></span>

# <a name="visual-studio-for-mac"></a>[<span data-ttu-id="2628e-139">Visual Studio dla komputerów Mac</span><span class="sxs-lookup"><span data-stu-id="2628e-139">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="2628e-140">Wybierz pozycję **plik** > **nowe rozwiązanie**.</span><span class="sxs-lookup"><span data-stu-id="2628e-140">Select **File** > **New Solution**.</span></span>

![Nowe rozwiązanie w systemie macOS](../first-mvc-app/start-mvc/_static/new_project_vsmac.png)

* <span data-ttu-id="2628e-142">Wybierz pozycję **aplikacja** **sieci > Web** > **.NET Core** > **dalej**.</span><span class="sxs-lookup"><span data-stu-id="2628e-142">Select **.NET Core** > **App** > **Web Application** > **Next**.</span></span>

  ![okno dialogowe z systemem macOS nowego projektu](razor-pages-start/_static/webapp.png)

* <span data-ttu-id="2628e-144">W oknie dialogowym **Konfigurowanie nowej aplikacji sieci Web** Ustaw platformę **docelową** na **platformę .NET Core 3,1**.</span><span class="sxs-lookup"><span data-stu-id="2628e-144">In the **Configure your new Web Application** dialog, set the  **Target Framework** to **.NET Core 3.1**.</span></span>

  ![wybór macOS .NET Core 3,1](razor-pages-start/_static/targetframework3.png)

* <span data-ttu-id="2628e-146">Nazwij projekt **RazorPagesMovie**, a następnie wybierz pozycję **Utwórz**.</span><span class="sxs-lookup"><span data-stu-id="2628e-146">Name the project **RazorPagesMovie**, and then select **Create**.</span></span>

  ![nameproj](razor-pages-start/_static/RazorPagesMovie.png)

<!-- End of VS tabs -->

---

## <a name="run-the-app"></a><span data-ttu-id="2628e-148">Uruchamianie aplikacji</span><span class="sxs-lookup"><span data-stu-id="2628e-148">Run the app</span></span>

  [!INCLUDE[](~/includes/run-the-app.md)]

## <a name="examine-the-project-files"></a><span data-ttu-id="2628e-149">Sprawdzanie plików projektu</span><span class="sxs-lookup"><span data-stu-id="2628e-149">Examine the project files</span></span>

<span data-ttu-id="2628e-150">Poniżej przedstawiono Omówienie folderów i plików projektu głównego, z których będziesz korzystać w kolejnych samouczkach.</span><span class="sxs-lookup"><span data-stu-id="2628e-150">Here's an overview of the main project folders and files that you'll work with in later tutorials.</span></span>

### <a name="pages-folder"></a><span data-ttu-id="2628e-151">Folder stron</span><span class="sxs-lookup"><span data-stu-id="2628e-151">Pages folder</span></span>

<span data-ttu-id="2628e-152">Zawiera strony Razor i pliki pomocnicze.</span><span class="sxs-lookup"><span data-stu-id="2628e-152">Contains Razor pages and supporting files.</span></span> <span data-ttu-id="2628e-153">Każda Strona Razor to para plików:</span><span class="sxs-lookup"><span data-stu-id="2628e-153">Each Razor page is a pair of files:</span></span>

* <span data-ttu-id="2628e-154">Plik *. cshtml* , który zawiera znaczniki HTML z C# kodem przy użyciu składnia Razor.</span><span class="sxs-lookup"><span data-stu-id="2628e-154">A *.cshtml* file that contains HTML markup with C# code using Razor syntax.</span></span>
* <span data-ttu-id="2628e-155">Plik *. cshtml.cs* , który zawiera C# kod, który obsługuje zdarzenia strony.</span><span class="sxs-lookup"><span data-stu-id="2628e-155">A *.cshtml.cs* file that contains C# code that handles page events.</span></span>

<span data-ttu-id="2628e-156">Pliki pomocnicze mają nazwy zaczynające się od znaku podkreślenia.</span><span class="sxs-lookup"><span data-stu-id="2628e-156">Supporting files have names that begin with an underscore.</span></span> <span data-ttu-id="2628e-157">Na przykład plik *_Layout. cshtml* służy do konfigurowania elementów interfejsu użytkownika wspólnych dla wszystkich stron.</span><span class="sxs-lookup"><span data-stu-id="2628e-157">For example, the *_Layout.cshtml* file configures UI elements common to all pages.</span></span> <span data-ttu-id="2628e-158">Ten plik konfiguruje menu nawigacji w górnej części strony i informacje o prawach autorskich w dolnej części strony.</span><span class="sxs-lookup"><span data-stu-id="2628e-158">This file sets up the navigation menu at the top of the page and the copyright notice at the bottom of the page.</span></span> <span data-ttu-id="2628e-159">Aby uzyskać więcej informacji, zobacz <xref:mvc/views/layout>.</span><span class="sxs-lookup"><span data-stu-id="2628e-159">For more information, see <xref:mvc/views/layout>.</span></span>

### <a name="wwwroot-folder"></a><span data-ttu-id="2628e-160">folder wwwroot</span><span class="sxs-lookup"><span data-stu-id="2628e-160">wwwroot folder</span></span>

<span data-ttu-id="2628e-161">Zawiera pliki statyczne, takie jak pliki HTML, pliki JavaScript i pliki CSS.</span><span class="sxs-lookup"><span data-stu-id="2628e-161">Contains static files, such as HTML files, JavaScript files, and CSS files.</span></span> <span data-ttu-id="2628e-162">Aby uzyskać więcej informacji, zobacz <xref:fundamentals/static-files>.</span><span class="sxs-lookup"><span data-stu-id="2628e-162">For more information, see <xref:fundamentals/static-files>.</span></span>

### <a name="appsettingsjson"></a><span data-ttu-id="2628e-163">appSettings.json</span><span class="sxs-lookup"><span data-stu-id="2628e-163">appSettings.json</span></span>

<span data-ttu-id="2628e-164">Zawiera dane konfiguracyjne, takie jak parametry połączenia.</span><span class="sxs-lookup"><span data-stu-id="2628e-164">Contains configuration data, such as connection strings.</span></span> <span data-ttu-id="2628e-165">Aby uzyskać więcej informacji, zobacz <xref:fundamentals/configuration/index>.</span><span class="sxs-lookup"><span data-stu-id="2628e-165">For more information, see <xref:fundamentals/configuration/index>.</span></span>

### <a name="programcs"></a><span data-ttu-id="2628e-166">Program.cs</span><span class="sxs-lookup"><span data-stu-id="2628e-166">Program.cs</span></span>

<span data-ttu-id="2628e-167">Zawiera punkt wejścia dla programu.</span><span class="sxs-lookup"><span data-stu-id="2628e-167">Contains the entry point for the program.</span></span> <span data-ttu-id="2628e-168">Aby uzyskać więcej informacji, zobacz <xref:fundamentals/host/generic-host>.</span><span class="sxs-lookup"><span data-stu-id="2628e-168">For more information, see <xref:fundamentals/host/generic-host>.</span></span>

### <a name="startupcs"></a><span data-ttu-id="2628e-169">Startup.cs</span><span class="sxs-lookup"><span data-stu-id="2628e-169">Startup.cs</span></span>

<span data-ttu-id="2628e-170">Zawiera kod, który konfiguruje zachowanie aplikacji.</span><span class="sxs-lookup"><span data-stu-id="2628e-170">Contains code that configures app behavior.</span></span> <span data-ttu-id="2628e-171">Aby uzyskać więcej informacji, zobacz <xref:fundamentals/startup>.</span><span class="sxs-lookup"><span data-stu-id="2628e-171">For more information, see <xref:fundamentals/startup>.</span></span>

## <a name="next-steps"></a><span data-ttu-id="2628e-172">Następne kroki</span><span class="sxs-lookup"><span data-stu-id="2628e-172">Next steps</span></span>

<span data-ttu-id="2628e-173">Przejdź do następnego samouczka z serii:</span><span class="sxs-lookup"><span data-stu-id="2628e-173">Advance to the next tutorial in the series:</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="2628e-174">Dodawanie modelu</span><span class="sxs-lookup"><span data-stu-id="2628e-174">Add a model</span></span>](xref:tutorials/razor-pages/model)

::: moniker-end

<!--::: moniker range=">= aspnetcore-3.0" -->

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="2628e-175">Jest to pierwszy samouczek dotyczący serii.</span><span class="sxs-lookup"><span data-stu-id="2628e-175">This is the first tutorial of a series.</span></span> <span data-ttu-id="2628e-176">[Seria zawiera](xref:tutorials/razor-pages/index) podstawowe informacje na temat tworzenia aplikacji sieci web ASP.NET Core Razor Pages.</span><span class="sxs-lookup"><span data-stu-id="2628e-176">[The series](xref:tutorials/razor-pages/index) teaches the basics of building an ASP.NET Core Razor Pages web app.</span></span>

[!INCLUDE[](~/includes/advancedRP.md)]

<span data-ttu-id="2628e-177">Na końcu serii będziesz mieć aplikację, która zarządza bazą danych filmów.</span><span class="sxs-lookup"><span data-stu-id="2628e-177">At the end of the series, you'll have an app that manages a database of movies.</span></span>  

[!INCLUDE[View or download sample code](~/includes/rp/download.md)]

<span data-ttu-id="2628e-178">W tym samouczku zostaną wykonane następujące czynności:</span><span class="sxs-lookup"><span data-stu-id="2628e-178">In this tutorial, you:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="2628e-179">Utwórz aplikację sieci Web Razor Pages.</span><span class="sxs-lookup"><span data-stu-id="2628e-179">Create a Razor Pages web app.</span></span>
> * <span data-ttu-id="2628e-180">Uruchom aplikację.</span><span class="sxs-lookup"><span data-stu-id="2628e-180">Run the app.</span></span>
> * <span data-ttu-id="2628e-181">Przejrzyj pliki projektu.</span><span class="sxs-lookup"><span data-stu-id="2628e-181">Examine the project files.</span></span>

<span data-ttu-id="2628e-182">Na końcu tego samouczka będziesz mieć działającą Razor Pagesową aplikację internetową, która zostanie wdrożona w kolejnych samouczkach.</span><span class="sxs-lookup"><span data-stu-id="2628e-182">At the end of this tutorial, you'll have a working Razor Pages web app that you'll build on in later tutorials.</span></span>

![Strona główna lub indeks](razor-pages-start/_static/home2.2.png)

## <a name="prerequisites"></a><span data-ttu-id="2628e-184">Wymagania wstępne</span><span class="sxs-lookup"><span data-stu-id="2628e-184">Prerequisites</span></span>

# <a name="visual-studio"></a>[<span data-ttu-id="2628e-185">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="2628e-185">Visual Studio</span></span>](#tab/visual-studio)

[!INCLUDE[](~/includes/net-core-prereqs-vs2019-2.2.md)]

# <a name="visual-studio-code"></a>[<span data-ttu-id="2628e-186">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="2628e-186">Visual Studio Code</span></span>](#tab/visual-studio-code)

[!INCLUDE[](~/includes/net-core-prereqs-vsc-2.2.md)]

# <a name="visual-studio-for-mac"></a>[<span data-ttu-id="2628e-187">Visual Studio dla komputerów Mac</span><span class="sxs-lookup"><span data-stu-id="2628e-187">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

[!INCLUDE[](~/includes/net-core-prereqs-mac-2.2.md)]

---

## <a name="create-a-razor-pages-web-app"></a><span data-ttu-id="2628e-188">Tworzenie aplikacji sieci Web Razor Pages</span><span class="sxs-lookup"><span data-stu-id="2628e-188">Create a Razor Pages web app</span></span>

# <a name="visual-studio"></a>[<span data-ttu-id="2628e-189">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="2628e-189">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="2628e-190">Z menu **plik** programu Visual Studio wybierz pozycję **Nowy** **projekt**>.</span><span class="sxs-lookup"><span data-stu-id="2628e-190">From the Visual Studio **File** menu, select **New** > **Project**.</span></span>

* <span data-ttu-id="2628e-191">Utwórz nową aplikację sieci Web ASP.NET Core a następnie wybierz pozycję **dalej**.</span><span class="sxs-lookup"><span data-stu-id="2628e-191">Create a new ASP.NET Core Web Application and select **Next**.</span></span>

  ![Nowa aplikacja sieci Web ASP.NET Core](razor-pages-start/_static/np_2.1.png)

* <span data-ttu-id="2628e-193">Nazwij projekt **RazorPagesMovie**.</span><span class="sxs-lookup"><span data-stu-id="2628e-193">Name the project **RazorPagesMovie**.</span></span> <span data-ttu-id="2628e-194">Ważne jest, aby nazwa projektu *RazorPagesMovie* , tak aby przestrzenie nazw były zgodne podczas kopiowania i wklejania kodu.</span><span class="sxs-lookup"><span data-stu-id="2628e-194">It's important to name the project *RazorPagesMovie* so the namespaces will match when you copy and paste code.</span></span>

  ![Nowa aplikacja sieci Web ASP.NET Core](razor-pages-start/_static/config.png)

* <span data-ttu-id="2628e-196">Wybierz pozycję **ASP.NET Core 2,2** na liście rozwijanej, **aplikacji sieci Web**, a następnie wybierz pozycję **Utwórz**.</span><span class="sxs-lookup"><span data-stu-id="2628e-196">Select **ASP.NET Core 2.2** in the dropdown, **Web Application**, and then select **Create**.</span></span>

![Nowa aplikacja sieci Web ASP.NET Core](razor-pages-start/_static/np_2_2.2.png)

  <span data-ttu-id="2628e-198">Tworzony jest następujący projekt początkowy:</span><span class="sxs-lookup"><span data-stu-id="2628e-198">The following starter project is created:</span></span>

  ![Eksplorator rozwiązań](razor-pages-start/_static/se2.2.png)

# <a name="visual-studio-code"></a>[<span data-ttu-id="2628e-200">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="2628e-200">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="2628e-201">Otwórz [zintegrowany terminal](https://code.visualstudio.com/docs/editor/integrated-terminal).</span><span class="sxs-lookup"><span data-stu-id="2628e-201">Open the [integrated terminal](https://code.visualstudio.com/docs/editor/integrated-terminal).</span></span>

* <span data-ttu-id="2628e-202">Przejdź do katalogu (`cd`), który będzie zawierać projekt.</span><span class="sxs-lookup"><span data-stu-id="2628e-202">Change to the directory (`cd`) which will contain the project.</span></span>

* <span data-ttu-id="2628e-203">Uruchom następujące polecenia:</span><span class="sxs-lookup"><span data-stu-id="2628e-203">Run the following commands:</span></span>

  ```dotnetcli
  dotnet new webapp -o RazorPagesMovie
  code -r RazorPagesMovie
  ```

  * <span data-ttu-id="2628e-204">`dotnet new` polecenie tworzy nowy projekt Razor Pages w folderze *RazorPagesMovie*</span><span class="sxs-lookup"><span data-stu-id="2628e-204">The `dotnet new` command creates a new Razor Pages project in the *RazorPagesMovie* folder.</span></span>
  * <span data-ttu-id="2628e-205">`code` polecenie otwiera folder *RazorPagesMovie* w bieżącym wystąpieniu Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="2628e-205">The `code` command opens the *RazorPagesMovie* folder in the current instance of Visual Studio Code.</span></span>

* <span data-ttu-id="2628e-206">Gdy ikona płomienia OmniSharp na pasku stanu zmieni kolor na zielony, w oknie dialogowym zostanie wyświetlony monit **o podanie wymaganych zasobów do skompilowania i debugowania z elementu "RazorPagesMovie". Dodać je?**</span><span class="sxs-lookup"><span data-stu-id="2628e-206">After the status bar's OmniSharp flame icon turns green, a dialog asks **Required assets to build and debug are missing from 'RazorPagesMovie'. Add them?**</span></span> <span data-ttu-id="2628e-207">Wybierz pozycję **Tak**.</span><span class="sxs-lookup"><span data-stu-id="2628e-207">Select **Yes**.</span></span>

  <span data-ttu-id="2628e-208">Katalog *. programu vscode* , zawierający pliki *Launch. JSON* i *Tasks. JSON* , jest dodawany do katalogu głównego projektu.</span><span class="sxs-lookup"><span data-stu-id="2628e-208">A *.vscode* directory, containing *launch.json* and *tasks.json* files, is added to the project's root directory.</span></span>

# <a name="visual-studio-for-mac"></a>[<span data-ttu-id="2628e-209">Visual Studio dla komputerów Mac</span><span class="sxs-lookup"><span data-stu-id="2628e-209">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="2628e-210">Wybierz pozycję **plik** > **nowe rozwiązanie**.</span><span class="sxs-lookup"><span data-stu-id="2628e-210">Select **File** > **New Solution**.</span></span>

![Nowe rozwiązanie w systemie macOS](../first-mvc-app/start-mvc/_static/new_project_vsmac.png)

* <span data-ttu-id="2628e-212">Wybierz pozycję **aplikacja** **sieci > Web** > **.NET Core** > **dalej**.</span><span class="sxs-lookup"><span data-stu-id="2628e-212">Select **.NET Core** > **App** > **Web Application** > **Next**.</span></span>

  ![okno dialogowe z systemem macOS nowego projektu](razor-pages-start/_static/webapp.png)

* <span data-ttu-id="2628e-214">W oknie dialogowym **Konfigurowanie nowego interfejsu API sieci Web ASP.NET Core** Ustaw platformę **docelową** na **.NET Core 3,1**.</span><span class="sxs-lookup"><span data-stu-id="2628e-214">In the **Configure your new ASP.NET Core Web API** dialog, set the  **Target Framework** to **.NET Core 3.1**.</span></span>

  ![wybór macOS .NET Core 3,0](razor-pages-start/_static/targetframework3.png)

* <span data-ttu-id="2628e-216">Nazwij projekt **RazorPagesMovie**, a następnie wybierz pozycję **Utwórz**.</span><span class="sxs-lookup"><span data-stu-id="2628e-216">Name the project **RazorPagesMovie**, and then select **Create**.</span></span>

  ![nameproj](razor-pages-start/_static/RazorPagesMovie.png)

<!-- End of VS tabs -->

---

## <a name="run-the-app"></a><span data-ttu-id="2628e-218">Uruchamianie aplikacji</span><span class="sxs-lookup"><span data-stu-id="2628e-218">Run the app</span></span>

# <a name="visual-studio"></a>[<span data-ttu-id="2628e-219">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="2628e-219">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="2628e-220">Naciśnij klawisze CTRL + F5, aby uruchomić bez debugera.</span><span class="sxs-lookup"><span data-stu-id="2628e-220">Press Ctrl+F5 to run without the debugger.</span></span>

  [!INCLUDE[](~/includes/trustCertVS.md)]

  <span data-ttu-id="2628e-221">Program Visual Studio jest uruchamiany [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) i uruchomi aplikację.</span><span class="sxs-lookup"><span data-stu-id="2628e-221">Visual Studio starts [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) and runs the app.</span></span> <span data-ttu-id="2628e-222">Na pasku adresu są wyświetlane `localhost:port#` a nie elementy, takie jak `example.com`.</span><span class="sxs-lookup"><span data-stu-id="2628e-222">The address bar shows `localhost:port#` and not something like `example.com`.</span></span> <span data-ttu-id="2628e-223">Wynika to z faktu, że `localhost` jest standardową nazwą hosta dla komputera lokalnego.</span><span class="sxs-lookup"><span data-stu-id="2628e-223">That's because `localhost` is the standard hostname for the local computer.</span></span> <span data-ttu-id="2628e-224">Host lokalny obsługuje tylko żądania sieci Web z komputera lokalnego.</span><span class="sxs-lookup"><span data-stu-id="2628e-224">Localhost only serves web requests from the local computer.</span></span> <span data-ttu-id="2628e-225">Gdy program Visual Studio tworzy projekt sieci Web, dla serwera sieci Web jest używany port losowy.</span><span class="sxs-lookup"><span data-stu-id="2628e-225">When Visual Studio creates a web project, a random port is used for the web server.</span></span>

* <span data-ttu-id="2628e-226">Na stronie głównej aplikacji wybierz pozycję **Akceptuj** , aby wyrazić zgodę na śledzenie.</span><span class="sxs-lookup"><span data-stu-id="2628e-226">On the app's home page, select **Accept** to consent to tracking.</span></span>

  <span data-ttu-id="2628e-227">Ta aplikacja nie śledzi informacji osobistych, ale szablon projektu zawiera funkcję zgody na wypadek, gdyby była niezbędna do przestrzegania Ogólne rozporządzenie o ochronie danych Unii Europejskiej [(Rodo)](xref:security/gdpr).</span><span class="sxs-lookup"><span data-stu-id="2628e-227">This app doesn't track personal information, but the project template includes the consent feature in case you need it to comply with the European Union's [General Data Protection Regulation (GDPR)](xref:security/gdpr).</span></span>

  ![Strona główna lub indeks](razor-pages-start/_static/homeGDPR2.2.png)

  <span data-ttu-id="2628e-229">Na poniższej ilustracji przedstawiono aplikację po udzieleniu zgody na śledzenie:</span><span class="sxs-lookup"><span data-stu-id="2628e-229">The following image shows the app after you give consent to tracking:</span></span>

  ![Strona główna lub indeks](razor-pages-start/_static/home2.2.png)
  
# <a name="visual-studio-code"></a>[<span data-ttu-id="2628e-231">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="2628e-231">Visual Studio Code</span></span>](#tab/visual-studio-code)

  [!INCLUDE[](~/includes/trustCertVSC.md)]

* <span data-ttu-id="2628e-232">Naciśnij **klawisze CTRL + F5** , aby uruchomić bez debugera.</span><span class="sxs-lookup"><span data-stu-id="2628e-232">Press **Ctrl-F5** to run without the debugger.</span></span>

  <span data-ttu-id="2628e-233">Visual Studio Code uruchamia [Kestrel](xref:fundamentals/servers/kestrel), uruchamia przeglądarkę i przechodzi do `http://localhost:5001`.</span><span class="sxs-lookup"><span data-stu-id="2628e-233">Visual Studio Code starts [Kestrel](xref:fundamentals/servers/kestrel), launches a browser, and navigates to `http://localhost:5001`.</span></span> <span data-ttu-id="2628e-234">Na pasku adresu są wyświetlane `localhost:port#` a nie elementy, takie jak `example.com`.</span><span class="sxs-lookup"><span data-stu-id="2628e-234">The address bar shows `localhost:port#` and not something like `example.com`.</span></span> <span data-ttu-id="2628e-235">Wynika to z faktu, że `localhost` jest standardową nazwą hosta dla komputera lokalnego.</span><span class="sxs-lookup"><span data-stu-id="2628e-235">That's because `localhost` is the standard hostname for  local computer.</span></span> <span data-ttu-id="2628e-236">Host lokalny obsługuje tylko żądania sieci Web z komputera lokalnego.</span><span class="sxs-lookup"><span data-stu-id="2628e-236">Localhost only serves web requests from the local computer.</span></span>

* <span data-ttu-id="2628e-237">Na stronie głównej aplikacji wybierz pozycję **Akceptuj** , aby wyrazić zgodę na śledzenie.</span><span class="sxs-lookup"><span data-stu-id="2628e-237">On the app's home page, select **Accept** to consent to tracking.</span></span>

  <span data-ttu-id="2628e-238">Ta aplikacja nie śledzi informacji osobistych, ale szablon projektu zawiera funkcję zgody na wypadek, gdyby była niezbędna do przestrzegania Ogólne rozporządzenie o ochronie danych Unii Europejskiej [(Rodo)](xref:security/gdpr).</span><span class="sxs-lookup"><span data-stu-id="2628e-238">This app doesn't track personal information, but the project template includes the consent feature in case you need it to comply with the European Union's [General Data Protection Regulation (GDPR)](xref:security/gdpr).</span></span>

  ![Strona główna lub indeks](razor-pages-start/_static/homeGDPR2.2.png)

  <span data-ttu-id="2628e-240">Na poniższej ilustracji przedstawiono aplikację po udzieleniu zgody na śledzenie:</span><span class="sxs-lookup"><span data-stu-id="2628e-240">The following image shows the app after you give consent to tracking:</span></span>

  ![Strona główna lub indeks](razor-pages-start/_static/home2.2.png)
  
# <a name="visual-studio-for-mac"></a>[<span data-ttu-id="2628e-242">Visual Studio dla komputerów Mac</span><span class="sxs-lookup"><span data-stu-id="2628e-242">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

  [!INCLUDE[](~/includes/trustCertMac.md)]

* <span data-ttu-id="2628e-243">Naciśnij **polecenie cmd-opt-F5** , aby uruchomić program bez debugera.</span><span class="sxs-lookup"><span data-stu-id="2628e-243">Press **Cmd-Opt-F5** to run without the debugger.</span></span>

  <span data-ttu-id="2628e-244">Program Visual Studio uruchamia [Kestrel](xref:fundamentals/servers/kestrel), uruchamia przeglądarkę i przechodzi do `http://localhost:5001`.</span><span class="sxs-lookup"><span data-stu-id="2628e-244">Visual Studio starts [Kestrel](xref:fundamentals/servers/kestrel), launches a browser, and navigates to `http://localhost:5001`.</span></span>

* <span data-ttu-id="2628e-245">Na stronie głównej aplikacji wybierz pozycję **Akceptuj** , aby wyrazić zgodę na śledzenie.</span><span class="sxs-lookup"><span data-stu-id="2628e-245">On the app's home page, select **Accept** to consent to tracking.</span></span>

  <span data-ttu-id="2628e-246">Ta aplikacja nie śledzi informacji osobistych, ale szablon projektu zawiera funkcję zgody na wypadek, gdyby była niezbędna do przestrzegania Ogólne rozporządzenie o ochronie danych Unii Europejskiej [(Rodo)](xref:security/gdpr).</span><span class="sxs-lookup"><span data-stu-id="2628e-246">This app doesn't track personal information, but the project template includes the consent feature in case you need it to comply with the European Union's [General Data Protection Regulation (GDPR)](xref:security/gdpr).</span></span>

  ![Strona główna lub indeks](razor-pages-start/_static/homeGDPR2.2_safari.png)

  <span data-ttu-id="2628e-248">Na poniższej ilustracji przedstawiono aplikację po udzieleniu zgody na śledzenie:</span><span class="sxs-lookup"><span data-stu-id="2628e-248">The following image shows the app after you give consent to tracking:</span></span>

  ![Strona główna lub indeks](razor-pages-start/_static/home2.2_safari.png)

<!-- End of VS tabs -->

---

## <a name="examine-the-project-files"></a><span data-ttu-id="2628e-250">Sprawdzanie plików projektu</span><span class="sxs-lookup"><span data-stu-id="2628e-250">Examine the project files</span></span>

<span data-ttu-id="2628e-251">Poniżej przedstawiono Omówienie folderów i plików projektu głównego, z których będziesz korzystać w kolejnych samouczkach.</span><span class="sxs-lookup"><span data-stu-id="2628e-251">Here's an overview of the main project folders and files that you'll work with in later tutorials.</span></span>

### <a name="pages-folder"></a><span data-ttu-id="2628e-252">Folder stron</span><span class="sxs-lookup"><span data-stu-id="2628e-252">Pages folder</span></span>

<span data-ttu-id="2628e-253">Zawiera strony Razor i pliki pomocnicze.</span><span class="sxs-lookup"><span data-stu-id="2628e-253">Contains Razor pages and supporting files.</span></span> <span data-ttu-id="2628e-254">Każda Strona Razor to para plików:</span><span class="sxs-lookup"><span data-stu-id="2628e-254">Each Razor page is a pair of files:</span></span>

* <span data-ttu-id="2628e-255">Plik *. cshtml* , który zawiera znaczniki HTML z C# kodem przy użyciu składnia Razor.</span><span class="sxs-lookup"><span data-stu-id="2628e-255">A *.cshtml* file that contains HTML markup with C# code using Razor syntax.</span></span>
* <span data-ttu-id="2628e-256">Plik *. cshtml.cs* , który zawiera C# kod, który obsługuje zdarzenia strony.</span><span class="sxs-lookup"><span data-stu-id="2628e-256">A *.cshtml.cs* file that contains C# code that handles page events.</span></span>

<span data-ttu-id="2628e-257">Pliki pomocnicze mają nazwy zaczynające się od znaku podkreślenia.</span><span class="sxs-lookup"><span data-stu-id="2628e-257">Supporting files have names that begin with an underscore.</span></span> <span data-ttu-id="2628e-258">Na przykład plik *_Layout. cshtml* służy do konfigurowania elementów interfejsu użytkownika wspólnych dla wszystkich stron.</span><span class="sxs-lookup"><span data-stu-id="2628e-258">For example, the *_Layout.cshtml* file configures UI elements common to all pages.</span></span> <span data-ttu-id="2628e-259">Ten plik konfiguruje menu nawigacji w górnej części strony i informacje o prawach autorskich w dolnej części strony.</span><span class="sxs-lookup"><span data-stu-id="2628e-259">This file sets up the navigation menu at the top of the page and the copyright notice at the bottom of the page.</span></span> <span data-ttu-id="2628e-260">Aby uzyskać więcej informacji, zobacz <xref:mvc/views/layout>.</span><span class="sxs-lookup"><span data-stu-id="2628e-260">For more information, see <xref:mvc/views/layout>.</span></span>

### <a name="wwwroot-folder"></a><span data-ttu-id="2628e-261">folder wwwroot</span><span class="sxs-lookup"><span data-stu-id="2628e-261">wwwroot folder</span></span>

<span data-ttu-id="2628e-262">Zawiera pliki statyczne, takie jak pliki HTML, pliki JavaScript i pliki CSS.</span><span class="sxs-lookup"><span data-stu-id="2628e-262">Contains static files, such as HTML files, JavaScript files, and CSS files.</span></span> <span data-ttu-id="2628e-263">Aby uzyskać więcej informacji, zobacz <xref:fundamentals/static-files>.</span><span class="sxs-lookup"><span data-stu-id="2628e-263">For more information, see <xref:fundamentals/static-files>.</span></span>

### <a name="appsettingsjson"></a><span data-ttu-id="2628e-264">appSettings.json</span><span class="sxs-lookup"><span data-stu-id="2628e-264">appSettings.json</span></span>

<span data-ttu-id="2628e-265">Zawiera dane konfiguracyjne, takie jak parametry połączenia.</span><span class="sxs-lookup"><span data-stu-id="2628e-265">Contains configuration data, such as connection strings.</span></span> <span data-ttu-id="2628e-266">Aby uzyskać więcej informacji, zobacz <xref:fundamentals/configuration/index>.</span><span class="sxs-lookup"><span data-stu-id="2628e-266">For more information, see <xref:fundamentals/configuration/index>.</span></span>

### <a name="programcs"></a><span data-ttu-id="2628e-267">Program.cs</span><span class="sxs-lookup"><span data-stu-id="2628e-267">Program.cs</span></span>

<span data-ttu-id="2628e-268">Zawiera punkt wejścia dla programu.</span><span class="sxs-lookup"><span data-stu-id="2628e-268">Contains the entry point for the program.</span></span> <span data-ttu-id="2628e-269">Aby uzyskać więcej informacji, zobacz <xref:fundamentals/host/generic-host>.</span><span class="sxs-lookup"><span data-stu-id="2628e-269">For more information, see <xref:fundamentals/host/generic-host>.</span></span>

### <a name="startupcs"></a><span data-ttu-id="2628e-270">Startup.cs</span><span class="sxs-lookup"><span data-stu-id="2628e-270">Startup.cs</span></span>

<span data-ttu-id="2628e-271">Zawiera kod, który konfiguruje zachowanie aplikacji, na przykład czy wymaga zgody na pliki cookie.</span><span class="sxs-lookup"><span data-stu-id="2628e-271">Contains code that configures app behavior, such as whether it requires consent for cookies.</span></span> <span data-ttu-id="2628e-272">Aby uzyskać więcej informacji, zobacz <xref:fundamentals/startup>.</span><span class="sxs-lookup"><span data-stu-id="2628e-272">For more information, see <xref:fundamentals/startup>.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="2628e-273">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="2628e-273">Additional resources</span></span>

* [<span data-ttu-id="2628e-274">Wersja tego samouczka usługi YouTube</span><span class="sxs-lookup"><span data-stu-id="2628e-274">Youtube version of this tutorial</span></span>](https://www.youtube.com/watch?v=F0SP7Ry4flQ&feature=youtu.be)

## <a name="next-steps"></a><span data-ttu-id="2628e-275">Następne kroki</span><span class="sxs-lookup"><span data-stu-id="2628e-275">Next steps</span></span>

<span data-ttu-id="2628e-276">Przejdź do następnego samouczka z serii:</span><span class="sxs-lookup"><span data-stu-id="2628e-276">Advance to the next tutorial in the series:</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="2628e-277">Dodawanie modelu</span><span class="sxs-lookup"><span data-stu-id="2628e-277">Add a model</span></span>](xref:tutorials/razor-pages/model)

::: moniker-end
