---
title: 'Samouczek: Rozpoczynanie pracy ze stronami Razor w programie ASP.NET Core'
author: rick-anderson
description: W tej serii samouczków pokazano, jak używać stron Razor w programie ASP.NET Core. Dowiedz się, jak utworzyć model, generowanie kodu dla stron Razor, platformy Entity Framework Core i SQL Server na użytek dostęp do danych, dodać funkcje wyszukiwania, dodać sprawdzanie poprawności danych wejściowych i użyć migracje do aktualizacji modelu.
ms.author: riande
ms.date: 06/3/2019
uid: tutorials/razor-pages/razor-pages-start
ms.openlocfilehash: b0cbb4dda5328d305686f21a0b3e8f8e83575a39
ms.sourcegitcommit: 8516b586541e6ba402e57228e356639b85dfb2b9
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/11/2019
ms.locfileid: "67815259"
---
# <a name="tutorial-get-started-with-razor-pages-in-aspnet-core"></a><span data-ttu-id="dfe60-104">Samouczek: Rozpoczynanie pracy ze stronami Razor w programie ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="dfe60-104">Tutorial: Get started with Razor Pages in ASP.NET Core</span></span>

<span data-ttu-id="dfe60-105">Przez [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="dfe60-105">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="dfe60-106">To jest pierwszy samouczek serii.</span><span class="sxs-lookup"><span data-stu-id="dfe60-106">This is the first tutorial of a series.</span></span> <span data-ttu-id="dfe60-107">[Seria](xref:tutorials/razor-pages/index) uczy podstaw tworzenia aplikacji platformy ASP.NET Core Razor strony sieci web.</span><span class="sxs-lookup"><span data-stu-id="dfe60-107">[The series](xref:tutorials/razor-pages/index) teaches the basics of building an ASP.NET Core Razor Pages web app.</span></span>

[!INCLUDE[](~/includes/advancedRP.md)]

<span data-ttu-id="dfe60-108">Na końcu serii będziesz mieć aplikację, która zarządza bazy danych filmów.</span><span class="sxs-lookup"><span data-stu-id="dfe60-108">At the end of the series, you'll have an app that manages a database of movies.</span></span>  

[!INCLUDE[View or download sample code](~/includes/rp/download.md)]

<span data-ttu-id="dfe60-109">W ramach tego samouczka możesz:</span><span class="sxs-lookup"><span data-stu-id="dfe60-109">In this tutorial, you:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="dfe60-110">Tworzenie aplikacji internetowej stron Razor.</span><span class="sxs-lookup"><span data-stu-id="dfe60-110">Create a Razor Pages web app.</span></span>
> * <span data-ttu-id="dfe60-111">Uruchom aplikację.</span><span class="sxs-lookup"><span data-stu-id="dfe60-111">Run the app.</span></span>
> * <span data-ttu-id="dfe60-112">Sprawdź pliki projektu.</span><span class="sxs-lookup"><span data-stu-id="dfe60-112">Examine the project files.</span></span>

<span data-ttu-id="dfe60-113">Na końcu tego samouczka będziesz mieć działającą aplikację sieci web stron Razor, które utworzysz w kolejnych samouczkach.</span><span class="sxs-lookup"><span data-stu-id="dfe60-113">At the end of this tutorial, you'll have a working Razor Pages web app that you'll build on in later tutorials.</span></span>

![Strona główna lub indeks](razor-pages-start/_static/home2.2.png)

## <a name="prerequisites"></a><span data-ttu-id="dfe60-115">Wymagania wstępne</span><span class="sxs-lookup"><span data-stu-id="dfe60-115">Prerequisites</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="dfe60-116">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="dfe60-116">Visual Studio</span></span>](#tab/visual-studio)

[!INCLUDE[](~/includes/net-core-prereqs-vs2019-2.2.md)]

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="dfe60-117">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="dfe60-117">Visual Studio Code</span></span>](#tab/visual-studio-code)

[!INCLUDE[](~/includes/net-core-prereqs-vsc-2.2.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="dfe60-118">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="dfe60-118">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

[!INCLUDE[](~/includes/net-core-prereqs-mac-2.2.md)]

---

## <a name="create-a-razor-pages-web-app"></a><span data-ttu-id="dfe60-119">Tworzenie aplikacji sieci web stron Razor</span><span class="sxs-lookup"><span data-stu-id="dfe60-119">Create a Razor Pages web app</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="dfe60-120">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="dfe60-120">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="dfe60-121">W programie Visual Studio **pliku** menu, wybierz opcję **New** > **projektu**.</span><span class="sxs-lookup"><span data-stu-id="dfe60-121">From the Visual Studio **File** menu, select **New** > **Project**.</span></span>

* <span data-ttu-id="dfe60-122">Tworzenie nowej aplikacji sieci Web platformy ASP.NET Core i wybierz **dalej**.</span><span class="sxs-lookup"><span data-stu-id="dfe60-122">Create a new ASP.NET Core Web Application and select **Next**.</span></span>

  ![Nowa aplikacja internetowa ASP.NET Core](razor-pages-start/_static/np_2.1.png)

* <span data-ttu-id="dfe60-124">Nadaj projektowi nazwę **RazorPagesMovie**.</span><span class="sxs-lookup"><span data-stu-id="dfe60-124">Name the project **RazorPagesMovie**.</span></span> <span data-ttu-id="dfe60-125">Ważne jest, aby nadaj projektowi nazwę *RazorPagesMovie* , przestrzenie nazw będą zgodne po skopiuj i Wklej kod.</span><span class="sxs-lookup"><span data-stu-id="dfe60-125">It's important to name the project *RazorPagesMovie* so the namespaces will match when you copy and paste code.</span></span>

  ![Nowa aplikacja internetowa ASP.NET Core](razor-pages-start/_static/config.png)

* <span data-ttu-id="dfe60-127">Wybierz **platformy ASP.NET Core 2.2** na liście rozwijanej **aplikacji sieci Web**, a następnie wybierz pozycję **Utwórz**.</span><span class="sxs-lookup"><span data-stu-id="dfe60-127">Select **ASP.NET Core 2.2** in the dropdown, **Web Application**, and then select **Create**.</span></span>

![Nowa aplikacja internetowa ASP.NET Core](razor-pages-start/_static/np_2_2.2.png)

  <span data-ttu-id="dfe60-129">Utworzono następujący projekt startowy:</span><span class="sxs-lookup"><span data-stu-id="dfe60-129">The following starter project is created:</span></span>

  ![Eksplorator rozwiązań](razor-pages-start/_static/se2.2.png)

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="dfe60-131">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="dfe60-131">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="dfe60-132">Otwórz [zintegrowany terminal](https://code.visualstudio.com/docs/editor/integrated-terminal).</span><span class="sxs-lookup"><span data-stu-id="dfe60-132">Open the [integrated terminal](https://code.visualstudio.com/docs/editor/integrated-terminal).</span></span>

* <span data-ttu-id="dfe60-133">Przejdź do katalogu (`cd`) zawierający projekt.</span><span class="sxs-lookup"><span data-stu-id="dfe60-133">Change to the directory (`cd`) which will contain the project.</span></span>

* <span data-ttu-id="dfe60-134">Uruchom następujące polecenia:</span><span class="sxs-lookup"><span data-stu-id="dfe60-134">Run the following commands:</span></span>

  ```console
  dotnet new webapp -o RazorPagesMovie
  code -r RazorPagesMovie
  ```

  * <span data-ttu-id="dfe60-135">`dotnet new` Polecenie tworzy nowy projekt strony Razor w *RazorPagesMovie* folderu.</span><span class="sxs-lookup"><span data-stu-id="dfe60-135">The `dotnet new` command creates a new Razor Pages project in the *RazorPagesMovie* folder.</span></span>
  * <span data-ttu-id="dfe60-136">`code` Polecenia otwiera *RazorPagesMovie* folderu w bieżącym wystąpieniu programu Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="dfe60-136">The `code` command opens the *RazorPagesMovie* folder in the current instance of Visual Studio Code.</span></span>

* <span data-ttu-id="dfe60-137">Po technologię OmniSharp pasek stanu gaśniczego ikona zmieni kolor na zielony, okno dialogowe prosi **"RazorPagesMovie" brakuje wymagane zasoby do tworzenia i debugowania. Dodaj je?**</span><span class="sxs-lookup"><span data-stu-id="dfe60-137">After the status bar's OmniSharp flame icon turns green, a dialog asks **Required assets to build and debug are missing from 'RazorPagesMovie'. Add them?**</span></span> <span data-ttu-id="dfe60-138">Wybierz **tak**.</span><span class="sxs-lookup"><span data-stu-id="dfe60-138">Select **Yes**.</span></span>

  <span data-ttu-id="dfe60-139">A *.vscode* katalog zawierający *launch.json* i *tasks.json* pliki, zostanie dodany do katalogu głównego projektu.</span><span class="sxs-lookup"><span data-stu-id="dfe60-139">A *.vscode* directory, containing *launch.json* and *tasks.json* files, is added to the project's root directory.</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="dfe60-140">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="dfe60-140">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="dfe60-141">W terminalu uruchom następujące polecenie:</span><span class="sxs-lookup"><span data-stu-id="dfe60-141">From a terminal, run the following command:</span></span>

<!-- TODO: update these instruction once mac support 2.2 projects -->

```console
dotnet new webapp -o RazorPagesMovie
```

<span data-ttu-id="dfe60-142">Poprzednie polecenia użyj [interfejsu wiersza polecenia platformy .NET Core](/dotnet/core/tools/dotnet) do utworzenia projektu stron Razor.</span><span class="sxs-lookup"><span data-stu-id="dfe60-142">The preceding commands use the [.NET Core CLI](/dotnet/core/tools/dotnet) to create a Razor Pages project.</span></span>

## <a name="open-the-project"></a><span data-ttu-id="dfe60-143">Otwórz projekt</span><span class="sxs-lookup"><span data-stu-id="dfe60-143">Open the project</span></span>

<span data-ttu-id="dfe60-144">Z programu Visual Studio, wybierz **Plik > Otwórz**, a następnie wybierz pozycję *RazorPagesMovie.csproj* pliku.</span><span class="sxs-lookup"><span data-stu-id="dfe60-144">From Visual Studio, select **File > Open**, and then select the *RazorPagesMovie.csproj* file.</span></span>

<!-- End of VS tabs -->

---

## <a name="run-the-app"></a><span data-ttu-id="dfe60-145">Uruchamianie aplikacji</span><span class="sxs-lookup"><span data-stu-id="dfe60-145">Run the app</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="dfe60-146">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="dfe60-146">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="dfe60-147">Naciśnij klawisze Ctrl + F5, aby uruchomić bez debugowania.</span><span class="sxs-lookup"><span data-stu-id="dfe60-147">Press Ctrl+F5 to run without the debugger.</span></span>

  [!INCLUDE[](~/includes/trustCertVS.md)]

  <span data-ttu-id="dfe60-148">Program Visual Studio uruchamia [usług IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) i uruchomienie aplikacji.</span><span class="sxs-lookup"><span data-stu-id="dfe60-148">Visual Studio starts [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) and runs the app.</span></span> <span data-ttu-id="dfe60-149">Przedstawia pasek adresu `localhost:port#` i nie mielibyśmy mieć czegoś podobnego `example.com`.</span><span class="sxs-lookup"><span data-stu-id="dfe60-149">The address bar shows `localhost:port#` and not something like `example.com`.</span></span> <span data-ttu-id="dfe60-150">To dlatego, że `localhost` jest standardowa nazwa hosta komputera lokalnego.</span><span class="sxs-lookup"><span data-stu-id="dfe60-150">That's because `localhost` is the standard hostname for the local computer.</span></span> <span data-ttu-id="dfe60-151">Localhost obsługują tylko żądania sieci web z komputera lokalnego.</span><span class="sxs-lookup"><span data-stu-id="dfe60-151">Localhost only serves web requests from the local computer.</span></span> <span data-ttu-id="dfe60-152">Gdy program Visual Studio tworzy projekt sieci web, losowy port jest używany dla serwera sieci web.</span><span class="sxs-lookup"><span data-stu-id="dfe60-152">When Visual Studio creates a web project, a random port is used for the web server.</span></span>

* <span data-ttu-id="dfe60-153">Na stronie głównej aplikacji, wybierz **Akceptuj** do wyrażenia zgody na śledzenie.</span><span class="sxs-lookup"><span data-stu-id="dfe60-153">On the app's home page, select **Accept** to consent to tracking.</span></span>

  <span data-ttu-id="dfe60-154">Ta aplikacja nie może śledzić informacje osobiste, ale szablonu projektu obejmuje funkcję zgody, w przypadku, gdy będą potrzebne do wykonania w Unii Europejskiej [ogólne rozporządzenie o ochronie danych (RODO)](xref:security/gdpr).</span><span class="sxs-lookup"><span data-stu-id="dfe60-154">This app doesn't track personal information, but the project template includes the consent feature in case you need it to comply with the European Union's [General Data Protection Regulation (GDPR)](xref:security/gdpr).</span></span>

  ![Strona główna lub indeks](razor-pages-start/_static/homeGDPR2.2.png)

  <span data-ttu-id="dfe60-156">Na poniższej ilustracji przedstawiono aplikację po zgody śledzenia:</span><span class="sxs-lookup"><span data-stu-id="dfe60-156">The following image shows the app after you give consent to tracking:</span></span>

  ![Strona główna lub indeks](razor-pages-start/_static/home2.2.png)
  
# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="dfe60-158">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="dfe60-158">Visual Studio Code</span></span>](#tab/visual-studio-code)

  [!INCLUDE[](~/includes/trustCertVSC.md)]

* <span data-ttu-id="dfe60-159">Naciśnij klawisz **Ctrl-F5** do uruchomienia bez debugera.</span><span class="sxs-lookup"><span data-stu-id="dfe60-159">Press **Ctrl-F5** to run without the debugger.</span></span>

  <span data-ttu-id="dfe60-160">Uruchamia programu Visual Studio Code [Kestrel](xref:fundamentals/servers/kestrel)otworzy w przeglądarce i przechodzi do `http://localhost:5001`.</span><span class="sxs-lookup"><span data-stu-id="dfe60-160">Visual Studio Code starts [Kestrel](xref:fundamentals/servers/kestrel), launches a browser, and navigates to `http://localhost:5001`.</span></span> <span data-ttu-id="dfe60-161">Przedstawia pasek adresu `localhost:port#` i nie mielibyśmy mieć czegoś podobnego `example.com`.</span><span class="sxs-lookup"><span data-stu-id="dfe60-161">The address bar shows `localhost:port#` and not something like `example.com`.</span></span> <span data-ttu-id="dfe60-162">To dlatego, że `localhost` jest standardowa nazwa hosta na komputerze lokalnym.</span><span class="sxs-lookup"><span data-stu-id="dfe60-162">That's because `localhost` is the standard hostname for  local computer.</span></span> <span data-ttu-id="dfe60-163">Localhost obsługują tylko żądania sieci web z komputera lokalnego.</span><span class="sxs-lookup"><span data-stu-id="dfe60-163">Localhost only serves web requests from the local computer.</span></span>

* <span data-ttu-id="dfe60-164">Na stronie głównej aplikacji, wybierz **Akceptuj** do wyrażenia zgody na śledzenie.</span><span class="sxs-lookup"><span data-stu-id="dfe60-164">On the app's home page, select **Accept** to consent to tracking.</span></span>

  <span data-ttu-id="dfe60-165">Ta aplikacja nie może śledzić informacje osobiste, ale szablonu projektu obejmuje funkcję zgody, w przypadku, gdy będą potrzebne do wykonania w Unii Europejskiej [ogólne rozporządzenie o ochronie danych (RODO)](xref:security/gdpr).</span><span class="sxs-lookup"><span data-stu-id="dfe60-165">This app doesn't track personal information, but the project template includes the consent feature in case you need it to comply with the European Union's [General Data Protection Regulation (GDPR)](xref:security/gdpr).</span></span>

  ![Strona główna lub indeks](razor-pages-start/_static/homeGDPR2.2.png)

  <span data-ttu-id="dfe60-167">Na poniższej ilustracji przedstawiono aplikację po zgody śledzenia:</span><span class="sxs-lookup"><span data-stu-id="dfe60-167">The following image shows the app after you give consent to tracking:</span></span>

  ![Strona główna lub indeks](razor-pages-start/_static/home2.2.png)
  
# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="dfe60-169">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="dfe60-169">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

  [!INCLUDE[](~/includes/trustCertMac.md)]

* <span data-ttu-id="dfe60-170">Naciśnij klawisz **Cmd-Opt — F5** do uruchomienia bez debugera.</span><span class="sxs-lookup"><span data-stu-id="dfe60-170">Press **Cmd-Opt-F5** to run without the debugger.</span></span>

  <span data-ttu-id="dfe60-171">Program Visual Studio uruchamia [Kestrel](xref:fundamentals/servers/kestrel)otworzy w przeglądarce i przechodzi do `http://localhost:5001`.</span><span class="sxs-lookup"><span data-stu-id="dfe60-171">Visual Studio starts [Kestrel](xref:fundamentals/servers/kestrel), launches a browser, and navigates to `http://localhost:5001`.</span></span>

* <span data-ttu-id="dfe60-172">Na stronie głównej aplikacji, wybierz **Akceptuj** do wyrażenia zgody na śledzenie.</span><span class="sxs-lookup"><span data-stu-id="dfe60-172">On the app's home page, select **Accept** to consent to tracking.</span></span>

  <span data-ttu-id="dfe60-173">Ta aplikacja nie może śledzić informacje osobiste, ale szablonu projektu obejmuje funkcję zgody, w przypadku, gdy będą potrzebne do wykonania w Unii Europejskiej [ogólne rozporządzenie o ochronie danych (RODO)](xref:security/gdpr).</span><span class="sxs-lookup"><span data-stu-id="dfe60-173">This app doesn't track personal information, but the project template includes the consent feature in case you need it to comply with the European Union's [General Data Protection Regulation (GDPR)](xref:security/gdpr).</span></span>

  ![Strona główna lub indeks](razor-pages-start/_static/homeGDPR2.2_safari.png)

  <span data-ttu-id="dfe60-175">Na poniższej ilustracji przedstawiono aplikację po zgody śledzenia:</span><span class="sxs-lookup"><span data-stu-id="dfe60-175">The following image shows the app after you give consent to tracking:</span></span>

  ![Strona główna lub indeks](razor-pages-start/_static/home2.2_safari.png)

<!-- End of VS tabs -->

---

## <a name="examine-the-project-files"></a><span data-ttu-id="dfe60-177">Przejrzyj pliki projektu</span><span class="sxs-lookup"><span data-stu-id="dfe60-177">Examine the project files</span></span>

<span data-ttu-id="dfe60-178">Poniżej przedstawiono omówienie folderów głównego projektu i plików, które będziesz pracować w kolejnych samouczkach.</span><span class="sxs-lookup"><span data-stu-id="dfe60-178">Here's an overview of the main project folders and files that you'll work with in later tutorials.</span></span>

### <a name="pages-folder"></a><span data-ttu-id="dfe60-179">Folder stron</span><span class="sxs-lookup"><span data-stu-id="dfe60-179">Pages folder</span></span>

<span data-ttu-id="dfe60-180">Zawiera stronami Razor i pliki pomocnicze.</span><span class="sxs-lookup"><span data-stu-id="dfe60-180">Contains Razor pages and supporting files.</span></span> <span data-ttu-id="dfe60-181">Każda strona Razor jest parę plików:</span><span class="sxs-lookup"><span data-stu-id="dfe60-181">Each Razor page is a pair of files:</span></span>

* <span data-ttu-id="dfe60-182">A *.cshtml* pliku, który zawiera kod znaczników HTML za pomocą C# kodu przy użyciu składni Razor.</span><span class="sxs-lookup"><span data-stu-id="dfe60-182">A *.cshtml* file that contains HTML markup with C# code using Razor syntax.</span></span>
* <span data-ttu-id="dfe60-183">A *. cshtml.cs* pliku, który zawiera C# kod, który obsługuje zdarzenia strony.</span><span class="sxs-lookup"><span data-stu-id="dfe60-183">A *.cshtml.cs* file that contains C# code that handles page events.</span></span>

<span data-ttu-id="dfe60-184">Pliki obsługi mają nazwy rozpoczynające się od znaku podkreślenia.</span><span class="sxs-lookup"><span data-stu-id="dfe60-184">Supporting files have names that begin with an underscore.</span></span> <span data-ttu-id="dfe60-185">Na przykład *_Layout.cshtml* plik konfiguruje elementy interfejsu użytkownika dla wszystkich stron.</span><span class="sxs-lookup"><span data-stu-id="dfe60-185">For example, the *_Layout.cshtml* file configures UI elements common to all pages.</span></span> <span data-ttu-id="dfe60-186">Ten plik konfiguruje menu nawigacji w górnej części strony i informacje o prawach autorskich w dolnej części strony.</span><span class="sxs-lookup"><span data-stu-id="dfe60-186">This file sets up the navigation menu at the top of the page and the copyright notice at the bottom of the page.</span></span> <span data-ttu-id="dfe60-187">Aby uzyskać więcej informacji, zobacz <xref:mvc/views/layout>.</span><span class="sxs-lookup"><span data-stu-id="dfe60-187">For more information, see <xref:mvc/views/layout>.</span></span>

### <a name="wwwroot-folder"></a><span data-ttu-id="dfe60-188">Wwwroot folder</span><span class="sxs-lookup"><span data-stu-id="dfe60-188">wwwroot folder</span></span>

<span data-ttu-id="dfe60-189">Zawiera pliki statyczne, takie jak pliki HTML, plików JavaScript i plików CSS.</span><span class="sxs-lookup"><span data-stu-id="dfe60-189">Contains static files, such as HTML files, JavaScript files, and CSS files.</span></span> <span data-ttu-id="dfe60-190">Aby uzyskać więcej informacji, zobacz <xref:fundamentals/static-files>.</span><span class="sxs-lookup"><span data-stu-id="dfe60-190">For more information, see <xref:fundamentals/static-files>.</span></span>

### <a name="appsettingsjson"></a><span data-ttu-id="dfe60-191">appSettings.json</span><span class="sxs-lookup"><span data-stu-id="dfe60-191">appSettings.json</span></span>

<span data-ttu-id="dfe60-192">Zawiera dane konfiguracyjne, takie jak parametry połączenia.</span><span class="sxs-lookup"><span data-stu-id="dfe60-192">Contains configuration data, such as connection strings.</span></span> <span data-ttu-id="dfe60-193">Aby uzyskać więcej informacji, zobacz <xref:fundamentals/configuration/index>.</span><span class="sxs-lookup"><span data-stu-id="dfe60-193">For more information, see <xref:fundamentals/configuration/index>.</span></span>

### <a name="programcs"></a><span data-ttu-id="dfe60-194">Program.cs</span><span class="sxs-lookup"><span data-stu-id="dfe60-194">Program.cs</span></span>

<span data-ttu-id="dfe60-195">Zawiera punkt wejścia programu.</span><span class="sxs-lookup"><span data-stu-id="dfe60-195">Contains the entry point for the program.</span></span> <span data-ttu-id="dfe60-196">Aby uzyskać więcej informacji, zobacz <xref:fundamentals/host/generic-host>.</span><span class="sxs-lookup"><span data-stu-id="dfe60-196">For more information, see <xref:fundamentals/host/generic-host>.</span></span>

### <a name="startupcs"></a><span data-ttu-id="dfe60-197">Startup.cs</span><span class="sxs-lookup"><span data-stu-id="dfe60-197">Startup.cs</span></span>

<span data-ttu-id="dfe60-198">Zawiera kod, który konfiguruje zachowania aplikacji, na przykład tego, czy wymaga zgody na pliki cookie.</span><span class="sxs-lookup"><span data-stu-id="dfe60-198">Contains code that configures app behavior, such as whether it requires consent for cookies.</span></span> <span data-ttu-id="dfe60-199">Aby uzyskać więcej informacji, zobacz <xref:fundamentals/startup>.</span><span class="sxs-lookup"><span data-stu-id="dfe60-199">For more information, see <xref:fundamentals/startup>.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="dfe60-200">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="dfe60-200">Additional resources</span></span>

* [<span data-ttu-id="dfe60-201">Wersja usługi youtube w tym samouczku</span><span class="sxs-lookup"><span data-stu-id="dfe60-201">Youtube version of this tutorial</span></span>](https://www.youtube.com/watch?v=F0SP7Ry4flQ&feature=youtu.be)

## <a name="next-steps"></a><span data-ttu-id="dfe60-202">Następne kroki</span><span class="sxs-lookup"><span data-stu-id="dfe60-202">Next steps</span></span>

<span data-ttu-id="dfe60-203">W ramach tego samouczka możesz:</span><span class="sxs-lookup"><span data-stu-id="dfe60-203">In this tutorial, you:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="dfe60-204">Utworzona aplikacja internetowa ze stronami Razor.</span><span class="sxs-lookup"><span data-stu-id="dfe60-204">Created a Razor Pages web app.</span></span>
> * <span data-ttu-id="dfe60-205">Uruchomienia aplikacji.</span><span class="sxs-lookup"><span data-stu-id="dfe60-205">Ran the app.</span></span>
> * <span data-ttu-id="dfe60-206">Zbadane plików projektu.</span><span class="sxs-lookup"><span data-stu-id="dfe60-206">Examined the project files.</span></span>

<span data-ttu-id="dfe60-207">Przejdź do następnego samouczka w serii:</span><span class="sxs-lookup"><span data-stu-id="dfe60-207">Advance to the next tutorial in the series:</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="dfe60-208">Dodawanie modelu</span><span class="sxs-lookup"><span data-stu-id="dfe60-208">Add a model</span></span>](xref:tutorials/razor-pages/model)
