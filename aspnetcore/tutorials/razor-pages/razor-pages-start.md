---
title: Rozpoczynanie pracy ze stronami Razor w programie ASP.NET Core
author: rick-anderson
monikerRange: '>= aspnetcore-2.2'
description: W tej serii samouczków pokazano, jak używać stron Razor w programie ASP.NET Core. Dowiedz się, jak utworzyć model, generowanie kodu dla stron Razor, platformy Entity Framework Core i SQL Server na użytek dostęp do danych, dodać funkcje wyszukiwania, dodać sprawdzanie poprawności danych wejściowych i użyć migracje do aktualizacji modelu.
ms.author: riande
ms.date: 12/5/2018
uid: tutorials/razor-pages/razor-pages-start
ms.openlocfilehash: 1152ebfcee48a46ecd28c941fce32d3fc1e05c41
ms.sourcegitcommit: 9bb58d7c8dad4bbd03419bcc183d027667fefa20
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 12/04/2018
ms.locfileid: "52861631"
---
# <a name="tutorial-get-started-with-razor-pages-in-aspnet-core"></a><span data-ttu-id="1c6a6-104">Samouczek: Rozpoczynanie pracy ze stronami Razor w programie ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="1c6a6-104">Tutorial: Get started with Razor Pages in ASP.NET Core</span></span>

<span data-ttu-id="1c6a6-105">Przez [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="1c6a6-105">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="1c6a6-106">W tym samouczku pokazano podstawy tworzenia aplikacji sieci web programu ASP.NET Core Razor strony.</span><span class="sxs-lookup"><span data-stu-id="1c6a6-106">This tutorial teaches the basics of building an ASP.NET Core Razor Pages web app.</span></span>

<span data-ttu-id="1c6a6-107">Aplikacja zarządza bazę tytułów filmów.</span><span class="sxs-lookup"><span data-stu-id="1c6a6-107">The app manages a database of movie titles.</span></span> <span data-ttu-id="1c6a6-108">Dowiesz się, jak:</span><span class="sxs-lookup"><span data-stu-id="1c6a6-108">You learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="1c6a6-109">Tworzenie aplikacji internetowej stron Razor.</span><span class="sxs-lookup"><span data-stu-id="1c6a6-109">Create a Razor Pages web app.</span></span>
> * <span data-ttu-id="1c6a6-110">Dodaj i tworzenia szkieletu modelu.</span><span class="sxs-lookup"><span data-stu-id="1c6a6-110">Add and scaffold a model.</span></span>
> * <span data-ttu-id="1c6a6-111">Praca z bazą danych.</span><span class="sxs-lookup"><span data-stu-id="1c6a6-111">Work with a database.</span></span>
> * <span data-ttu-id="1c6a6-112">Dodaj wyszukiwanie i sprawdzania poprawności.</span><span class="sxs-lookup"><span data-stu-id="1c6a6-112">Add search and validation.</span></span>

<span data-ttu-id="1c6a6-113">Na końcu masz aplikację, która może zarządzać i wyświetlania filmu tytułów elementów.</span><span class="sxs-lookup"><span data-stu-id="1c6a6-113">At the end, you have an app that can manage and display movie titles items.</span></span>

[!INCLUDE[](~/includes/rp/download.md)]

## <a name="prerequisites"></a><span data-ttu-id="1c6a6-114">Wymagania wstępne</span><span class="sxs-lookup"><span data-stu-id="1c6a6-114">Prerequisites</span></span>

[!INCLUDE[](~/includes/net-core-prereqs-all-2.2.md)]

## <a name="create-a-razor-web-app"></a><span data-ttu-id="1c6a6-115">Tworzenie aplikacji sieci web Razor</span><span class="sxs-lookup"><span data-stu-id="1c6a6-115">Create a Razor web app</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="1c6a6-116">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="1c6a6-116">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="1c6a6-117">W programie Visual Studio **pliku** menu, wybierz opcję **New** > **projektu**.</span><span class="sxs-lookup"><span data-stu-id="1c6a6-117">From the Visual Studio **File** menu, select **New** > **Project**.</span></span>
* <span data-ttu-id="1c6a6-118">Tworzenie nowej aplikacji sieci Web platformy ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="1c6a6-118">Create a new ASP.NET Core Web Application.</span></span> <span data-ttu-id="1c6a6-119">Nadaj projektowi nazwę **RazorPagesMovie**.</span><span class="sxs-lookup"><span data-stu-id="1c6a6-119">Name the project **RazorPagesMovie**.</span></span> <span data-ttu-id="1c6a6-120">Ważne jest, aby nadaj projektowi nazwę *RazorPagesMovie* , przestrzenie nazw będą zgodne, jeśli kopiujesz/wklejasz kod.</span><span class="sxs-lookup"><span data-stu-id="1c6a6-120">It's important to name the project *RazorPagesMovie* so the namespaces will match when you copy/paste code.</span></span>
 <span data-ttu-id="1c6a6-121">![Nowa aplikacja internetowa ASP.NET Core](razor-pages-start/_static/np_2.1.png)</span><span class="sxs-lookup"><span data-stu-id="1c6a6-121">![new ASP.NET Core Web Application](razor-pages-start/_static/np_2.1.png)</span></span>

* <span data-ttu-id="1c6a6-122">Wybierz **platformy ASP.NET Core 2.2** w listy rozwijanej, a następnie wybierz pozycję **aplikacji sieci Web**.</span><span class="sxs-lookup"><span data-stu-id="1c6a6-122">Select **ASP.NET Core 2.2** in the dropdown, and then select **Web Application**.</span></span>

  ![Nowa aplikacja internetowa ASP.NET Core](razor-pages-start/_static/np_2_2.2.png)

  <span data-ttu-id="1c6a6-124">Utworzono następujący projekt startowy:</span><span class="sxs-lookup"><span data-stu-id="1c6a6-124">The following starter project is created:</span></span>

  ![Eksplorator rozwiązań](razor-pages-start/_static/se2.2.png)

* <span data-ttu-id="1c6a6-126">Naciśnij klawisz **Ctrl-F5** do uruchomienia bez debugera.</span><span class="sxs-lookup"><span data-stu-id="1c6a6-126">Press **Ctrl-F5** to run without the debugger.</span></span>

  <span data-ttu-id="1c6a6-127">Program Visual Studio uruchamia [usług IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) i uruchomienie aplikacji.</span><span class="sxs-lookup"><span data-stu-id="1c6a6-127">Visual Studio starts [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) and runs the app.</span></span> <span data-ttu-id="1c6a6-128">Przedstawia pasek adresu `localhost:port#` i nie mielibyśmy mieć czegoś podobnego `example.com`.</span><span class="sxs-lookup"><span data-stu-id="1c6a6-128">The address bar shows `localhost:port#` and not something like `example.com`.</span></span> <span data-ttu-id="1c6a6-129">To dlatego, że `localhost` jest standardowa nazwa hosta komputera lokalnego.</span><span class="sxs-lookup"><span data-stu-id="1c6a6-129">That's because `localhost` is the standard hostname for the local computer.</span></span> <span data-ttu-id="1c6a6-130">Localhost obsługują tylko żądania sieci web z komputera lokalnego.</span><span class="sxs-lookup"><span data-stu-id="1c6a6-130">Localhost only serves web requests from the local computer.</span></span> <span data-ttu-id="1c6a6-131">Gdy program Visual Studio tworzy projekt sieci web, losowy port jest używany dla serwera sieci web.</span><span class="sxs-lookup"><span data-stu-id="1c6a6-131">When Visual Studio creates a web project, a random port is used for the web server.</span></span> <span data-ttu-id="1c6a6-132">Na wcześniejszej ilustracji numer portu to 5001.</span><span class="sxs-lookup"><span data-stu-id="1c6a6-132">In the preceding image, the port number is 5001.</span></span> <span data-ttu-id="1c6a6-133">Po uruchomieniu aplikacji, zobaczysz inny numer portu.</span><span class="sxs-lookup"><span data-stu-id="1c6a6-133">When you run the app, you'll see a different port number.</span></span>

  <span data-ttu-id="1c6a6-134">Uruchamianie aplikacji za pomocą **kombinację klawiszy Ctrl + F5** (bez debugowania w trybie) pozwala wprowadzać zmiany kodu, Zapisz plik, Odśwież przeglądarkę i zobaczyć zmiany kodu.</span><span class="sxs-lookup"><span data-stu-id="1c6a6-134">Launching the app with **Ctrl+F5** (non-debug mode) allows you to make code changes, save the file, refresh the browser, and see the code changes.</span></span> <span data-ttu-id="1c6a6-135">Wielu programistów łatwiej jest w trybie bez debugowania odświeżenie strony i wyświetlić zmiany.</span><span class="sxs-lookup"><span data-stu-id="1c6a6-135">Many developers prefer to use non-debug mode to refresh the page and view changes.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="1c6a6-136">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="1c6a6-136">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="1c6a6-137">Otwórz [zintegrowany terminal](https://code.visualstudio.com/docs/editor/integrated-terminal).</span><span class="sxs-lookup"><span data-stu-id="1c6a6-137">Open the [integrated terminal](https://code.visualstudio.com/docs/editor/integrated-terminal).</span></span>
* <span data-ttu-id="1c6a6-138">Zmień katalog (`cd`) do folderu, który będzie zawierać projekt.</span><span class="sxs-lookup"><span data-stu-id="1c6a6-138">Change directories (`cd`) to a folder which will contain the project.</span></span>
* <span data-ttu-id="1c6a6-139">Uruchom następujące polecenie:</span><span class="sxs-lookup"><span data-stu-id="1c6a6-139">Run the following command:</span></span>

   ```console
   dotnet new webapp -o RazorPagesMovie
   code -r RazorPagesMovie
   ```

  * <span data-ttu-id="1c6a6-140">Zostanie wyświetlone okno dialogowe z **"RazorPagesMovie" brakuje wymagane zasoby do tworzenia i debugowania. Dodaj je?**</span><span class="sxs-lookup"><span data-stu-id="1c6a6-140">A dialog box appears with **Required assets to build and debug are missing from 'RazorPagesMovie'. Add them?**</span></span>  <span data-ttu-id="1c6a6-141">Wybierz **tak**</span><span class="sxs-lookup"><span data-stu-id="1c6a6-141">Select **Yes**</span></span>

  * <span data-ttu-id="1c6a6-142">`dotnet new webapp -o RazorPagesMovie`: tworzy nowy projekt strony Razor w *RazorPagesMovie* folderu.</span><span class="sxs-lookup"><span data-stu-id="1c6a6-142">`dotnet new webapp -o RazorPagesMovie`: creates a new Razor Pages project in the *RazorPagesMovie* folder.</span></span>
  * <span data-ttu-id="1c6a6-143">`code -r RazorPagesMovie`: Ładuje *RazorPagesMovie.csproj* plik projektu w programie Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="1c6a6-143">`code -r RazorPagesMovie`: Loads the *RazorPagesMovie.csproj* project file in Visual Studio Code.</span></span>

### <a name="launch-the-app"></a><span data-ttu-id="1c6a6-144">Uruchom aplikację</span><span class="sxs-lookup"><span data-stu-id="1c6a6-144">Launch the app</span></span>

* <span data-ttu-id="1c6a6-145">Naciśnij klawisz **Ctrl-F5** do uruchomienia bez debugera.</span><span class="sxs-lookup"><span data-stu-id="1c6a6-145">Press **Ctrl-F5** to run without the debugger.</span></span>

  <span data-ttu-id="1c6a6-146">Visual Studio Code rozpoczyna się rozpoczyna się [Kestrel](xref:fundamentals/servers/kestrel)otworzy w przeglądarce i przechodzi do `http://localhost:5001`.</span><span class="sxs-lookup"><span data-stu-id="1c6a6-146">Visual Studio Code starts starts [Kestrel](xref:fundamentals/servers/kestrel), launches a browser, and navigates to `http://localhost:5001`.</span></span> <span data-ttu-id="1c6a6-147">Przedstawia pasek adresu `localhost:port:5001` i nie mielibyśmy mieć czegoś podobnego `example.com`.</span><span class="sxs-lookup"><span data-stu-id="1c6a6-147">The address bar shows `localhost:port:5001` and not something like `example.com`.</span></span> <span data-ttu-id="1c6a6-148">To dlatego, że `localhost` jest standardowa nazwa hosta na komputerze lokalnym.</span><span class="sxs-lookup"><span data-stu-id="1c6a6-148">That's because `localhost` is the standard hostname for  local computer.</span></span> <span data-ttu-id="1c6a6-149">Localhost obsługują tylko żądania sieci web z komputera lokalnego.</span><span class="sxs-lookup"><span data-stu-id="1c6a6-149">Localhost only serves web requests from the local computer.</span></span>

  <span data-ttu-id="1c6a6-150">Uruchamianie aplikacji za pomocą **kombinację klawiszy Ctrl + F5** (bez debugowania w trybie) pozwala wprowadzać zmiany kodu, Zapisz plik, Odśwież przeglądarkę i zobaczyć zmiany kodu.</span><span class="sxs-lookup"><span data-stu-id="1c6a6-150">Launching the app with **Ctrl+F5** (non-debug mode) allows you to make code changes, save the file, refresh the browser, and see the code changes.</span></span> <span data-ttu-id="1c6a6-151">Wielu programistów łatwiej jest w trybie bez debugowania odświeżenie strony i wyświetlić zmiany.</span><span class="sxs-lookup"><span data-stu-id="1c6a6-151">Many developers prefer to use non-debug mode to refresh the page and view changes.</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="1c6a6-152">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="1c6a6-152">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="1c6a6-153">W terminalu uruchom następujące polecenia:</span><span class="sxs-lookup"><span data-stu-id="1c6a6-153">From a terminal, run the following commands:</span></span>

<!-- TODO: update these instruction once mac support 2.2 projects -->

```console
dotnet new webapp -o RazorPagesMovie
cd RazorPagesMovie
dotnet run
```

<span data-ttu-id="1c6a6-154">Poprzednie polecenia użyj [interfejsu wiersza polecenia platformy .NET Core](/dotnet/core/tools/dotnet) do tworzenia i uruchamiania projektów stron Razor.</span><span class="sxs-lookup"><span data-stu-id="1c6a6-154">The preceding commands use the [.NET Core CLI](/dotnet/core/tools/dotnet) to create and run a Razor Pages project.</span></span> <span data-ttu-id="1c6a6-155">Otwórz w przeglądarce http://localhost:5000 Aby wyświetlić aplikację.</span><span class="sxs-lookup"><span data-stu-id="1c6a6-155">Open a browser to http://localhost:5000 to view the application.</span></span>

## <a name="open-the-project"></a><span data-ttu-id="1c6a6-156">Otwórz projekt</span><span class="sxs-lookup"><span data-stu-id="1c6a6-156">Open the project</span></span>

<span data-ttu-id="1c6a6-157">Naciśnij klawisze Ctrl + C, aby zamknąć aplikację.</span><span class="sxs-lookup"><span data-stu-id="1c6a6-157">Press Ctrl+C to shut down the application.</span></span>

<span data-ttu-id="1c6a6-158">Z programu Visual Studio, wybierz **Plik > Otwórz**, a następnie wybierz pozycję *RazorPagesMovie.csproj* pliku.</span><span class="sxs-lookup"><span data-stu-id="1c6a6-158">From Visual Studio, select **File > Open**, and then select the *RazorPagesMovie.csproj* file.</span></span>

### <a name="launch-the-app"></a><span data-ttu-id="1c6a6-159">Uruchom aplikację</span><span class="sxs-lookup"><span data-stu-id="1c6a6-159">Launch the app</span></span>

<span data-ttu-id="1c6a6-160">Wybierz **Uruchom > Uruchom bez debugowania** do uruchomienia aplikacji.</span><span class="sxs-lookup"><span data-stu-id="1c6a6-160">Select **Run > Start Without Debugging** to launch the app.</span></span> <span data-ttu-id="1c6a6-161">Program Visual Studio uruchamia [Kestrel](xref:fundamentals/servers/kestrel)otworzy w przeglądarce i przechodzi do `http://localhost:5001`.</span><span class="sxs-lookup"><span data-stu-id="1c6a6-161">Visual Studio starts [Kestrel](xref:fundamentals/servers/kestrel), launches a browser, and navigates to `http://localhost:5001`.</span></span>

<!-- End of VS tabs -->

---

* <span data-ttu-id="1c6a6-162">Wybierz **Akceptuj** do wyrażenia zgody na śledzenie.</span><span class="sxs-lookup"><span data-stu-id="1c6a6-162">Select **Accept** to consent to tracking.</span></span> <span data-ttu-id="1c6a6-163">Ta aplikacja nie może śledzić informacje osobiste.</span><span class="sxs-lookup"><span data-stu-id="1c6a6-163">This app doesn't track personal information.</span></span> <span data-ttu-id="1c6a6-164">Kod wygenerowany szablon zawiera zasoby, które ułatwiają korzystanie z [ogólne rozporządzenie o ochronie danych (RODO)](xref:security/gdpr).</span><span class="sxs-lookup"><span data-stu-id="1c6a6-164">The template generated code includes assets to help meet [General Data Protection Regulation (GDPR)](xref:security/gdpr).</span></span>

  ![Strona główna lub indeks](razor-pages-start/_static/homeGDPR2.2.png)

  <span data-ttu-id="1c6a6-166">Na poniższej ilustracji przedstawiono aplikację po zaakceptowaniu śledzenia:</span><span class="sxs-lookup"><span data-stu-id="1c6a6-166">The following image shows the app after accepting tracking:</span></span>

  ![Strona główna lub indeks](razor-pages-start/_static/home2.2.png)

## <a name="project-files-and-folders"></a><span data-ttu-id="1c6a6-168">Pliki projektu i foldery</span><span class="sxs-lookup"><span data-stu-id="1c6a6-168">Project files and folders</span></span>

<span data-ttu-id="1c6a6-169">W poniższej tabeli wymieniono pliki i foldery w projekcie.</span><span class="sxs-lookup"><span data-stu-id="1c6a6-169">The following table lists the files and folders in the project.</span></span> <span data-ttu-id="1c6a6-170">W tym punkcie, w tym samouczku *Startup.cs* plik jest najważniejsze informacje.</span><span class="sxs-lookup"><span data-stu-id="1c6a6-170">At this point in the tutorial, the *Startup.cs* file is the most important to understand.</span></span> <span data-ttu-id="1c6a6-171">Nie potrzebujesz zapoznać się z każdym linku podanego poniżej.</span><span class="sxs-lookup"><span data-stu-id="1c6a6-171">You don't need to review each link provided below.</span></span> <span data-ttu-id="1c6a6-172">Linki są dostarczane jako odwołanie, jeśli potrzebujesz więcej informacji na temat pliku lub folderu w projekcie.</span><span class="sxs-lookup"><span data-stu-id="1c6a6-172">The links are provided as a reference when you need more information on a file or folder in the project.</span></span>

| <span data-ttu-id="1c6a6-173">Plik lub folder</span><span class="sxs-lookup"><span data-stu-id="1c6a6-173">File or folder</span></span>              | <span data-ttu-id="1c6a6-174">Cel</span><span class="sxs-lookup"><span data-stu-id="1c6a6-174">Purpose</span></span> |
| ----------------- | ------------ |
| <span data-ttu-id="1c6a6-175">*wwwroot*</span><span class="sxs-lookup"><span data-stu-id="1c6a6-175">*wwwroot*</span></span> | <span data-ttu-id="1c6a6-176">Zawiera pliki statyczne.</span><span class="sxs-lookup"><span data-stu-id="1c6a6-176">Contains static files.</span></span> <span data-ttu-id="1c6a6-177">Zobacz [pliki statyczne](xref:fundamentals/static-files).</span><span class="sxs-lookup"><span data-stu-id="1c6a6-177">See [Static files](xref:fundamentals/static-files).</span></span> |
| <span data-ttu-id="1c6a6-178">*Strony*</span><span class="sxs-lookup"><span data-stu-id="1c6a6-178">*Pages*</span></span> | <span data-ttu-id="1c6a6-179">Folder [stron Razor](xref:razor-pages/index).</span><span class="sxs-lookup"><span data-stu-id="1c6a6-179">Folder for [Razor Pages](xref:razor-pages/index).</span></span> |
| <span data-ttu-id="1c6a6-180">*appsettings.json*</span><span class="sxs-lookup"><span data-stu-id="1c6a6-180">*appsettings.json*</span></span> | [<span data-ttu-id="1c6a6-181">Konfiguracja</span><span class="sxs-lookup"><span data-stu-id="1c6a6-181">Configuration</span></span>](xref:fundamentals/configuration/index) |
| <span data-ttu-id="1c6a6-182">*Program.cs*</span><span class="sxs-lookup"><span data-stu-id="1c6a6-182">*Program.cs*</span></span> | <span data-ttu-id="1c6a6-183">[Hosty](xref:fundamentals/host/index) aplikacji ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="1c6a6-183">[Hosts](xref:fundamentals/host/index) the ASP.NET Core app.</span></span>|
| <span data-ttu-id="1c6a6-184">*Startup.cs*</span><span class="sxs-lookup"><span data-stu-id="1c6a6-184">*Startup.cs*</span></span> | <span data-ttu-id="1c6a6-185">Umożliwia skonfigurowanie usług i potok żądań.</span><span class="sxs-lookup"><span data-stu-id="1c6a6-185">Configures services and the request pipeline.</span></span> <span data-ttu-id="1c6a6-186">Zobacz [uruchamiania](xref:fundamentals/startup).</span><span class="sxs-lookup"><span data-stu-id="1c6a6-186">See [Startup](xref:fundamentals/startup).</span></span>|

### <a name="the-pages-folder"></a><span data-ttu-id="1c6a6-187">Folder stron</span><span class="sxs-lookup"><span data-stu-id="1c6a6-187">The Pages folder</span></span>

<span data-ttu-id="1c6a6-188">*_Layout.cshtml* plik zawiera wspólne elementy HTML (skrypty i arkusze stylów) i ustawia układ dla aplikacji.</span><span class="sxs-lookup"><span data-stu-id="1c6a6-188">The *_Layout.cshtml* file contains common HTML elements (scripts and stylesheets) and sets the layout for the application.</span></span> <span data-ttu-id="1c6a6-189">Na przykład po kliknięciu **RazorPagesMovie**, **Home**, lub **zachowania**, zobacz te same elementy.</span><span class="sxs-lookup"><span data-stu-id="1c6a6-189">For example, when you click on **RazorPagesMovie**, **Home**, or **Privacy**, you see the same elements.</span></span> <span data-ttu-id="1c6a6-190">Wspólne elementy obejmują menu nawigacji górnej i nagłówek w dolnej części okna.</span><span class="sxs-lookup"><span data-stu-id="1c6a6-190">The common elements include the navigation menu on the top and the header on the bottom of the window.</span></span> <span data-ttu-id="1c6a6-191">Zobacz [układ](xref:mvc/views/layout) Aby uzyskać więcej informacji.</span><span class="sxs-lookup"><span data-stu-id="1c6a6-191">See [Layout](xref:mvc/views/layout) for more information.</span></span>

<span data-ttu-id="1c6a6-192">*_ViewImports.cshtml* plik zawiera dyrektywy Razor, które są importowane do każdej strony Razor.</span><span class="sxs-lookup"><span data-stu-id="1c6a6-192">The *_ViewImports.cshtml* file contains Razor directives that are imported into each Razor Page.</span></span> <span data-ttu-id="1c6a6-193">Zobacz [importowania dyrektywy udostępnione](xref:mvc/views/layout#importing-shared-directives) Aby uzyskać więcej informacji.</span><span class="sxs-lookup"><span data-stu-id="1c6a6-193">See [Importing Shared Directives](xref:mvc/views/layout#importing-shared-directives) for more information.</span></span>

<span data-ttu-id="1c6a6-194">*_ViewStart.cshtml* ustawia stron Razor `Layout` właściwości, aby korzystała *_Layout.cshtml* pliku.</span><span class="sxs-lookup"><span data-stu-id="1c6a6-194">The *_ViewStart.cshtml* sets the Razor Pages `Layout` property to use the *_Layout.cshtml* file.</span></span> <span data-ttu-id="1c6a6-195">Zobacz [układ](xref:mvc/views/layout) Aby uzyskać więcej informacji.</span><span class="sxs-lookup"><span data-stu-id="1c6a6-195">See [Layout](xref:mvc/views/layout) for more information.</span></span>

<span data-ttu-id="1c6a6-196">*_ValidationScriptsPartial.cshtml* plik zawiera odwołanie do [jQuery](https://jquery.com/) skrypty sprawdzania poprawności.</span><span class="sxs-lookup"><span data-stu-id="1c6a6-196">The *_ValidationScriptsPartial.cshtml* file provides a reference to [jQuery](https://jquery.com/) validation scripts.</span></span> <span data-ttu-id="1c6a6-197">Gdy `Create` i `Edit` strony są dodawane w dalszej części tego samouczka *_ValidationScriptsPartial.cshtml* plik będzie używany.</span><span class="sxs-lookup"><span data-stu-id="1c6a6-197">When the `Create` and `Edit` pages are added later in the tutorial, the *_ValidationScriptsPartial.cshtml* file will be used.</span></span>

<span data-ttu-id="1c6a6-198">`Index`, `Error`, i `Privacy` strony znajdują się na:</span><span class="sxs-lookup"><span data-stu-id="1c6a6-198">`Index`, `Error`, and `Privacy` pages are provided to:</span></span>

* <span data-ttu-id="1c6a6-199">`Index`: Uruchom aplikację.</span><span class="sxs-lookup"><span data-stu-id="1c6a6-199">`Index`: Start an app.</span></span>
* <span data-ttu-id="1c6a6-200">`Error`: Służy do wyświetlania informacji o błędzie.</span><span class="sxs-lookup"><span data-stu-id="1c6a6-200">`Error`: Display error information.</span></span>
* <span data-ttu-id="1c6a6-201">`Privacy`: Określ szczegóły dotyczące zasad ochrony prywatności witryny.</span><span class="sxs-lookup"><span data-stu-id="1c6a6-201">`Privacy`: Specify details about the site's privacy policy.</span></span>

<span data-ttu-id="1c6a6-202">W tym samouczku poprzedniej strony nie są używane.</span><span class="sxs-lookup"><span data-stu-id="1c6a6-202">For this tutorial, the preceding pages are not used.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="1c6a6-203">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="1c6a6-203">Visual Studio</span></span>](#tab/visual-studio)

<a name="f7"></a>
### <a name="use-f7-to-toggle-between-a-razor-page-and-the-pagemodel"></a><span data-ttu-id="1c6a6-204">F7 umożliwia przełączanie się między strony Razor i PageModel</span><span class="sxs-lookup"><span data-stu-id="1c6a6-204">Use F7 to toggle between a Razor Page and the PageModel</span></span>

<span data-ttu-id="1c6a6-205">F7 przełącza między stronami Razor (*\*.cshtml* pliku) i C# pliku (*\*. cshtml.cs*).</span><span class="sxs-lookup"><span data-stu-id="1c6a6-205">F7 toggles between a Razor Page (*\*.cshtml* file) and the C# file (*\*.cshtml.cs*).</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="1c6a6-206">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="1c6a6-206">Visual Studio Code</span></span>](#tab/visual-studio-code)

<!-- TODO review  Need something in these tabs -->

<span data-ttu-id="1c6a6-207">Zgodnie z Konwencją, strona Razor (*\*.cshtml* pliku) oraz skojarzonych z nimi `PageModel` mają taką samą nazwę pliku głównego.</span><span class="sxs-lookup"><span data-stu-id="1c6a6-207">By convention, the Razor Page (*\*.cshtml* file) and the associated `PageModel` have the same root file name.</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="1c6a6-208">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="1c6a6-208">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="1c6a6-209">Zgodnie z Konwencją, strona Razor (*\*.cshtml* pliku) oraz skojarzonych z nimi `PageModel` mają taką samą nazwę pliku głównego.</span><span class="sxs-lookup"><span data-stu-id="1c6a6-209">By convention, the Razor Page (*\*.cshtml* file) and the associated `PageModel` have the same root file name.</span></span>

---

> [!div class="step-by-step"]
> [<span data-ttu-id="1c6a6-210">Następnie: Dodawanie modelu</span><span class="sxs-lookup"><span data-stu-id="1c6a6-210">Next: Adding a model</span></span>](xref:tutorials/razor-pages/model)