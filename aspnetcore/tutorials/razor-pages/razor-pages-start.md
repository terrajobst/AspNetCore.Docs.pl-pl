---
title: 'Samouczek: Rozpoczynanie pracy ze stronami Razor w programie ASP.NET Core'
author: rick-anderson
description: W tej serii samouczków pokazano, jak używać stron Razor w programie ASP.NET Core. Dowiedz się, jak utworzyć model, generowanie kodu dla stron Razor, platformy Entity Framework Core i SQL Server na użytek dostęp do danych, dodać funkcje wyszukiwania, dodać sprawdzanie poprawności danych wejściowych i użyć migracje do aktualizacji modelu.
ms.author: riande
ms.date: 6/3/2019
uid: tutorials/razor-pages/razor-pages-start
ms.openlocfilehash: ee5ef572db8b3c4e152fd864177c0eea3edc1f20
ms.sourcegitcommit: f5762967df3be8b8c868229e679301f2f7954679
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/13/2019
ms.locfileid: "67048218"
---
# <a name="tutorial-get-started-with-razor-pages-in-aspnet-core"></a><span data-ttu-id="b226f-104">Samouczek: Rozpoczynanie pracy ze stronami Razor w programie ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="b226f-104">Tutorial: Get started with Razor Pages in ASP.NET Core</span></span>

<span data-ttu-id="b226f-105">Przez [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="b226f-105">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="b226f-106">To jest pierwszy samouczek serii.</span><span class="sxs-lookup"><span data-stu-id="b226f-106">This is the first tutorial of a series.</span></span> <span data-ttu-id="b226f-107">[Seria](xref:tutorials/razor-pages/index) uczy podstaw tworzenia aplikacji platformy ASP.NET Core Razor strony sieci web.</span><span class="sxs-lookup"><span data-stu-id="b226f-107">[The series](xref:tutorials/razor-pages/index) teaches the basics of building an ASP.NET Core Razor Pages web app.</span></span>

[!INCLUDE[](~/includes/advancedRP.md)]

<span data-ttu-id="b226f-108">Na końcu serii będziesz mieć aplikację, która zarządza bazy danych filmów.</span><span class="sxs-lookup"><span data-stu-id="b226f-108">At the end of the series, you'll have an app that manages a database of movies.</span></span>  

[!INCLUDE[View or download sample code](~/includes/rp/download.md)]

<span data-ttu-id="b226f-109">W ramach tego samouczka możesz:</span><span class="sxs-lookup"><span data-stu-id="b226f-109">In this tutorial, you:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="b226f-110">Tworzenie aplikacji internetowej stron Razor.</span><span class="sxs-lookup"><span data-stu-id="b226f-110">Create a Razor Pages web app.</span></span>
> * <span data-ttu-id="b226f-111">Uruchom aplikację.</span><span class="sxs-lookup"><span data-stu-id="b226f-111">Run the app.</span></span>
> * <span data-ttu-id="b226f-112">Sprawdź pliki projektu.</span><span class="sxs-lookup"><span data-stu-id="b226f-112">Examine the project files.</span></span>

<span data-ttu-id="b226f-113">Na końcu tego samouczka będziesz mieć działającą aplikację sieci web stron Razor, które utworzysz w kolejnych samouczkach.</span><span class="sxs-lookup"><span data-stu-id="b226f-113">At the end of this tutorial, you'll have a working Razor Pages web app that you'll build on in later tutorials.</span></span>

![Strona główna lub indeks](razor-pages-start/_static/home2.2.png)

[!INCLUDE[](~/includes/net-core-prereqs-all-2.2.md)]

## <a name="create-a-razor-pages-web-app"></a><span data-ttu-id="b226f-115">Tworzenie aplikacji sieci web stron Razor</span><span class="sxs-lookup"><span data-stu-id="b226f-115">Create a Razor Pages web app</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="b226f-116">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="b226f-116">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="b226f-117">W programie Visual Studio **pliku** menu, wybierz opcję **New** > **projektu**.</span><span class="sxs-lookup"><span data-stu-id="b226f-117">From the Visual Studio **File** menu, select **New** > **Project**.</span></span>

* <span data-ttu-id="b226f-118">Tworzenie nowej aplikacji sieci Web platformy ASP.NET Core i wybierz **dalej**.</span><span class="sxs-lookup"><span data-stu-id="b226f-118">Create a new ASP.NET Core Web Application and select **Next**.</span></span>

  ![Nowa aplikacja internetowa ASP.NET Core](razor-pages-start/_static/np_2.1.png)

* <span data-ttu-id="b226f-120">Nadaj projektowi nazwę **RazorPagesMovie**.</span><span class="sxs-lookup"><span data-stu-id="b226f-120">Name the project **RazorPagesMovie**.</span></span> <span data-ttu-id="b226f-121">Ważne jest, aby nadaj projektowi nazwę *RazorPagesMovie* , przestrzenie nazw będą zgodne po skopiuj i Wklej kod.</span><span class="sxs-lookup"><span data-stu-id="b226f-121">It's important to name the project *RazorPagesMovie* so the namespaces will match when you copy and paste code.</span></span>

  ![Nowa aplikacja internetowa ASP.NET Core](razor-pages-start/_static/config.png)

* <span data-ttu-id="b226f-123">Wybierz **platformy ASP.NET Core 2.2** na liście rozwijanej **aplikacji sieci Web**, a następnie wybierz pozycję **Utwórz**.</span><span class="sxs-lookup"><span data-stu-id="b226f-123">Select **ASP.NET Core 2.2** in the dropdown, **Web Application**, and then select **Create**.</span></span>

![Nowa aplikacja internetowa ASP.NET Core](razor-pages-start/_static/np_2_2.2.png)

  <span data-ttu-id="b226f-125">Utworzono następujący projekt startowy:</span><span class="sxs-lookup"><span data-stu-id="b226f-125">The following starter project is created:</span></span>

  ![Eksplorator rozwiązań](razor-pages-start/_static/se2.2.png)

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="b226f-127">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="b226f-127">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="b226f-128">Otwórz [zintegrowany terminal](https://code.visualstudio.com/docs/editor/integrated-terminal).</span><span class="sxs-lookup"><span data-stu-id="b226f-128">Open the [integrated terminal](https://code.visualstudio.com/docs/editor/integrated-terminal).</span></span>

* <span data-ttu-id="b226f-129">Przejdź do katalogu (`cd`) zawierający projekt.</span><span class="sxs-lookup"><span data-stu-id="b226f-129">Change to the directory (`cd`) which will contain the project.</span></span>

* <span data-ttu-id="b226f-130">Uruchom następujące polecenia:</span><span class="sxs-lookup"><span data-stu-id="b226f-130">Run the following commands:</span></span>

  ```console
  dotnet new webapp -o RazorPagesMovie
  code -r RazorPagesMovie
  ```

  * <span data-ttu-id="b226f-131">`dotnet new` Polecenie tworzy nowy projekt strony Razor w *RazorPagesMovie* folderu.</span><span class="sxs-lookup"><span data-stu-id="b226f-131">The `dotnet new` command creates a new Razor Pages project in the *RazorPagesMovie* folder.</span></span>
  * <span data-ttu-id="b226f-132">`code` Polecenia otwiera *RazorPagesMovie* folderu w bieżącym wystąpieniu programu Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="b226f-132">The `code` command opens the *RazorPagesMovie* folder in the current instance of Visual Studio Code.</span></span>

* <span data-ttu-id="b226f-133">Po technologię OmniSharp pasek stanu gaśniczego ikona zmieni kolor na zielony, okno dialogowe prosi **"RazorPagesMovie" brakuje wymagane zasoby do tworzenia i debugowania. Dodaj je?**</span><span class="sxs-lookup"><span data-stu-id="b226f-133">After the status bar's OmniSharp flame icon turns green, a dialog asks **Required assets to build and debug are missing from 'RazorPagesMovie'. Add them?**</span></span> <span data-ttu-id="b226f-134">Wybierz **tak**.</span><span class="sxs-lookup"><span data-stu-id="b226f-134">Select **Yes**.</span></span>

  <span data-ttu-id="b226f-135">A *.vscode* katalog zawierający *launch.json* i *tasks.json* pliki, zostanie dodany do katalogu głównego projektu.</span><span class="sxs-lookup"><span data-stu-id="b226f-135">A *.vscode* directory, containing *launch.json* and *tasks.json* files, is added to the project's root directory.</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="b226f-136">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="b226f-136">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="b226f-137">W terminalu uruchom następujące polecenie:</span><span class="sxs-lookup"><span data-stu-id="b226f-137">From a terminal, run the following command:</span></span>

<!-- TODO: update these instruction once mac support 2.2 projects -->

```console
dotnet new webapp -o RazorPagesMovie
```

<span data-ttu-id="b226f-138">Poprzednie polecenia użyj [interfejsu wiersza polecenia platformy .NET Core](/dotnet/core/tools/dotnet) do utworzenia projektu stron Razor.</span><span class="sxs-lookup"><span data-stu-id="b226f-138">The preceding commands use the [.NET Core CLI](/dotnet/core/tools/dotnet) to create a Razor Pages project.</span></span>

## <a name="open-the-project"></a><span data-ttu-id="b226f-139">Otwórz projekt</span><span class="sxs-lookup"><span data-stu-id="b226f-139">Open the project</span></span>

<span data-ttu-id="b226f-140">Z programu Visual Studio, wybierz **Plik > Otwórz**, a następnie wybierz pozycję *RazorPagesMovie.csproj* pliku.</span><span class="sxs-lookup"><span data-stu-id="b226f-140">From Visual Studio, select **File > Open**, and then select the *RazorPagesMovie.csproj* file.</span></span>

<!-- End of VS tabs -->

---

## <a name="run-the-app"></a><span data-ttu-id="b226f-141">Uruchamianie aplikacji</span><span class="sxs-lookup"><span data-stu-id="b226f-141">Run the app</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="b226f-142">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="b226f-142">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="b226f-143">Naciśnij klawisze Ctrl + F5, aby uruchomić bez debugowania.</span><span class="sxs-lookup"><span data-stu-id="b226f-143">Press Ctrl+F5 to run without the debugger.</span></span>

  [!INCLUDE[](~/includes/trustCertVS.md)]

  <span data-ttu-id="b226f-144">Program Visual Studio uruchamia [usług IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) i uruchomienie aplikacji.</span><span class="sxs-lookup"><span data-stu-id="b226f-144">Visual Studio starts [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) and runs the app.</span></span> <span data-ttu-id="b226f-145">Przedstawia pasek adresu `localhost:port#` i nie mielibyśmy mieć czegoś podobnego `example.com`.</span><span class="sxs-lookup"><span data-stu-id="b226f-145">The address bar shows `localhost:port#` and not something like `example.com`.</span></span> <span data-ttu-id="b226f-146">To dlatego, że `localhost` jest standardowa nazwa hosta komputera lokalnego.</span><span class="sxs-lookup"><span data-stu-id="b226f-146">That's because `localhost` is the standard hostname for the local computer.</span></span> <span data-ttu-id="b226f-147">Localhost obsługują tylko żądania sieci web z komputera lokalnego.</span><span class="sxs-lookup"><span data-stu-id="b226f-147">Localhost only serves web requests from the local computer.</span></span> <span data-ttu-id="b226f-148">Gdy program Visual Studio tworzy projekt sieci web, losowy port jest używany dla serwera sieci web.</span><span class="sxs-lookup"><span data-stu-id="b226f-148">When Visual Studio creates a web project, a random port is used for the web server.</span></span>

* <span data-ttu-id="b226f-149">Na stronie głównej aplikacji, wybierz **Akceptuj** do wyrażenia zgody na śledzenie.</span><span class="sxs-lookup"><span data-stu-id="b226f-149">On the app's home page, select **Accept** to consent to tracking.</span></span>

  <span data-ttu-id="b226f-150">Ta aplikacja nie może śledzić informacje osobiste, ale szablonu projektu obejmuje funkcję zgody, w przypadku, gdy będą potrzebne do wykonania w Unii Europejskiej [ogólne rozporządzenie o ochronie danych (RODO)](xref:security/gdpr).</span><span class="sxs-lookup"><span data-stu-id="b226f-150">This app doesn't track personal information, but the project template includes the consent feature in case you need it to comply with the European Union's [General Data Protection Regulation (GDPR)](xref:security/gdpr).</span></span>

  ![Strona główna lub indeks](razor-pages-start/_static/homeGDPR2.2.png)

  <span data-ttu-id="b226f-152">Na poniższej ilustracji przedstawiono aplikację po zgody śledzenia:</span><span class="sxs-lookup"><span data-stu-id="b226f-152">The following image shows the app after you give consent to tracking:</span></span>

  ![Strona główna lub indeks](razor-pages-start/_static/home2.2.png)
  
# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="b226f-154">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="b226f-154">Visual Studio Code</span></span>](#tab/visual-studio-code)

  [!INCLUDE[](~/includes/trustCertVSC.md)]

* <span data-ttu-id="b226f-155">Naciśnij klawisz **Ctrl-F5** do uruchomienia bez debugera.</span><span class="sxs-lookup"><span data-stu-id="b226f-155">Press **Ctrl-F5** to run without the debugger.</span></span>

  <span data-ttu-id="b226f-156">Uruchamia programu Visual Studio Code [Kestrel](xref:fundamentals/servers/kestrel)otworzy w przeglądarce i przechodzi do `http://localhost:5001`.</span><span class="sxs-lookup"><span data-stu-id="b226f-156">Visual Studio Code starts [Kestrel](xref:fundamentals/servers/kestrel), launches a browser, and navigates to `http://localhost:5001`.</span></span> <span data-ttu-id="b226f-157">Przedstawia pasek adresu `localhost:port#` i nie mielibyśmy mieć czegoś podobnego `example.com`.</span><span class="sxs-lookup"><span data-stu-id="b226f-157">The address bar shows `localhost:port#` and not something like `example.com`.</span></span> <span data-ttu-id="b226f-158">To dlatego, że `localhost` jest standardowa nazwa hosta na komputerze lokalnym.</span><span class="sxs-lookup"><span data-stu-id="b226f-158">That's because `localhost` is the standard hostname for  local computer.</span></span> <span data-ttu-id="b226f-159">Localhost obsługują tylko żądania sieci web z komputera lokalnego.</span><span class="sxs-lookup"><span data-stu-id="b226f-159">Localhost only serves web requests from the local computer.</span></span>

* <span data-ttu-id="b226f-160">Na stronie głównej aplikacji, wybierz **Akceptuj** do wyrażenia zgody na śledzenie.</span><span class="sxs-lookup"><span data-stu-id="b226f-160">On the app's home page, select **Accept** to consent to tracking.</span></span>

  <span data-ttu-id="b226f-161">Ta aplikacja nie może śledzić informacje osobiste, ale szablonu projektu obejmuje funkcję zgody, w przypadku, gdy będą potrzebne do wykonania w Unii Europejskiej [ogólne rozporządzenie o ochronie danych (RODO)](xref:security/gdpr).</span><span class="sxs-lookup"><span data-stu-id="b226f-161">This app doesn't track personal information, but the project template includes the consent feature in case you need it to comply with the European Union's [General Data Protection Regulation (GDPR)](xref:security/gdpr).</span></span>

  ![Strona główna lub indeks](razor-pages-start/_static/homeGDPR2.2.png)

  <span data-ttu-id="b226f-163">Na poniższej ilustracji przedstawiono aplikację po zgody śledzenia:</span><span class="sxs-lookup"><span data-stu-id="b226f-163">The following image shows the app after you give consent to tracking:</span></span>

  ![Strona główna lub indeks](razor-pages-start/_static/home2.2.png)
  
# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="b226f-165">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="b226f-165">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

  [!INCLUDE[](~/includes/trustCertMac.md)]

* <span data-ttu-id="b226f-166">Naciśnij klawisz **Cmd-Opt — F5** do uruchomienia bez debugera.</span><span class="sxs-lookup"><span data-stu-id="b226f-166">Press **Cmd-Opt-F5** to run without the debugger.</span></span>

  <span data-ttu-id="b226f-167">Program Visual Studio uruchamia [Kestrel](xref:fundamentals/servers/kestrel)otworzy w przeglądarce i przechodzi do `http://localhost:5001`.</span><span class="sxs-lookup"><span data-stu-id="b226f-167">Visual Studio starts [Kestrel](xref:fundamentals/servers/kestrel), launches a browser, and navigates to `http://localhost:5001`.</span></span>

* <span data-ttu-id="b226f-168">Na stronie głównej aplikacji, wybierz **Akceptuj** do wyrażenia zgody na śledzenie.</span><span class="sxs-lookup"><span data-stu-id="b226f-168">On the app's home page, select **Accept** to consent to tracking.</span></span>

  <span data-ttu-id="b226f-169">Ta aplikacja nie może śledzić informacje osobiste, ale szablonu projektu obejmuje funkcję zgody, w przypadku, gdy będą potrzebne do wykonania w Unii Europejskiej [ogólne rozporządzenie o ochronie danych (RODO)](xref:security/gdpr).</span><span class="sxs-lookup"><span data-stu-id="b226f-169">This app doesn't track personal information, but the project template includes the consent feature in case you need it to comply with the European Union's [General Data Protection Regulation (GDPR)](xref:security/gdpr).</span></span>

  ![Strona główna lub indeks](razor-pages-start/_static/homeGDPR2.2_safari.png)

  <span data-ttu-id="b226f-171">Na poniższej ilustracji przedstawiono aplikację po zgody śledzenia:</span><span class="sxs-lookup"><span data-stu-id="b226f-171">The following image shows the app after you give consent to tracking:</span></span>

  ![Strona główna lub indeks](razor-pages-start/_static/home2.2_safari.png)

<!-- End of VS tabs -->

---

## <a name="examine-the-project-files"></a><span data-ttu-id="b226f-173">Przejrzyj pliki projektu</span><span class="sxs-lookup"><span data-stu-id="b226f-173">Examine the project files</span></span>

<span data-ttu-id="b226f-174">Poniżej przedstawiono omówienie folderów głównego projektu i plików, które będziesz pracować w kolejnych samouczkach.</span><span class="sxs-lookup"><span data-stu-id="b226f-174">Here's an overview of the main project folders and files that you'll work with in later tutorials.</span></span>

### <a name="pages-folder"></a><span data-ttu-id="b226f-175">Folder stron</span><span class="sxs-lookup"><span data-stu-id="b226f-175">Pages folder</span></span>

<span data-ttu-id="b226f-176">Zawiera stronami Razor i pliki pomocnicze.</span><span class="sxs-lookup"><span data-stu-id="b226f-176">Contains Razor pages and supporting files.</span></span> <span data-ttu-id="b226f-177">Każda strona Razor jest parę plików:</span><span class="sxs-lookup"><span data-stu-id="b226f-177">Each Razor page is a pair of files:</span></span>

* <span data-ttu-id="b226f-178">A *.cshtml* pliku, który zawiera kod znaczników HTML za pomocą C# kodu przy użyciu składni Razor.</span><span class="sxs-lookup"><span data-stu-id="b226f-178">A *.cshtml* file that contains HTML markup with C# code using Razor syntax.</span></span>
* <span data-ttu-id="b226f-179">A *. cshtml.cs* pliku, który zawiera C# kod, który obsługuje zdarzenia strony.</span><span class="sxs-lookup"><span data-stu-id="b226f-179">A *.cshtml.cs* file that contains C# code that handles page events.</span></span>

<span data-ttu-id="b226f-180">Pliki obsługi mają nazwy rozpoczynające się od znaku podkreślenia.</span><span class="sxs-lookup"><span data-stu-id="b226f-180">Supporting files have names that begin with an underscore.</span></span> <span data-ttu-id="b226f-181">Na przykład *_Layout.cshtml* plik konfiguruje elementy interfejsu użytkownika dla wszystkich stron.</span><span class="sxs-lookup"><span data-stu-id="b226f-181">For example, the *_Layout.cshtml* file configures UI elements common to all pages.</span></span> <span data-ttu-id="b226f-182">Ten plik konfiguruje menu nawigacji w górnej części strony i informacje o prawach autorskich w dolnej części strony.</span><span class="sxs-lookup"><span data-stu-id="b226f-182">This file sets up the navigation menu at the top of the page and the copyright notice at the bottom of the page.</span></span> <span data-ttu-id="b226f-183">Aby uzyskać więcej informacji, zobacz <xref:mvc/views/layout>.</span><span class="sxs-lookup"><span data-stu-id="b226f-183">For more information, see <xref:mvc/views/layout>.</span></span>

### <a name="wwwroot-folder"></a><span data-ttu-id="b226f-184">Wwwroot folder</span><span class="sxs-lookup"><span data-stu-id="b226f-184">wwwroot folder</span></span>

<span data-ttu-id="b226f-185">Zawiera pliki statyczne, takie jak pliki HTML, plików JavaScript i plików CSS.</span><span class="sxs-lookup"><span data-stu-id="b226f-185">Contains static files, such as HTML files, JavaScript files, and CSS files.</span></span> <span data-ttu-id="b226f-186">Aby uzyskać więcej informacji, zobacz <xref:fundamentals/static-files>.</span><span class="sxs-lookup"><span data-stu-id="b226f-186">For more information, see <xref:fundamentals/static-files>.</span></span>

### <a name="appsettingsjson"></a><span data-ttu-id="b226f-187">appSettings.json</span><span class="sxs-lookup"><span data-stu-id="b226f-187">appSettings.json</span></span>

<span data-ttu-id="b226f-188">Zawiera dane konfiguracyjne, takie jak parametry połączenia.</span><span class="sxs-lookup"><span data-stu-id="b226f-188">Contains configuration data, such as connection strings.</span></span> <span data-ttu-id="b226f-189">Aby uzyskać więcej informacji, zobacz <xref:fundamentals/configuration/index>.</span><span class="sxs-lookup"><span data-stu-id="b226f-189">For more information, see <xref:fundamentals/configuration/index>.</span></span>

### <a name="programcs"></a><span data-ttu-id="b226f-190">Program.cs</span><span class="sxs-lookup"><span data-stu-id="b226f-190">Program.cs</span></span>

<span data-ttu-id="b226f-191">Zawiera punkt wejścia programu.</span><span class="sxs-lookup"><span data-stu-id="b226f-191">Contains the entry point for the program.</span></span> <span data-ttu-id="b226f-192">Aby uzyskać więcej informacji, zobacz <xref:fundamentals/host/generic-host>.</span><span class="sxs-lookup"><span data-stu-id="b226f-192">For more information, see <xref:fundamentals/host/generic-host>.</span></span>

### <a name="startupcs"></a><span data-ttu-id="b226f-193">Startup.cs</span><span class="sxs-lookup"><span data-stu-id="b226f-193">Startup.cs</span></span>

<span data-ttu-id="b226f-194">Zawiera kod, który konfiguruje zachowania aplikacji, na przykład tego, czy wymaga zgody na pliki cookie.</span><span class="sxs-lookup"><span data-stu-id="b226f-194">Contains code that configures app behavior, such as whether it requires consent for cookies.</span></span> <span data-ttu-id="b226f-195">Aby uzyskać więcej informacji, zobacz <xref:fundamentals/startup>.</span><span class="sxs-lookup"><span data-stu-id="b226f-195">For more information, see <xref:fundamentals/startup>.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="b226f-196">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="b226f-196">Additional resources</span></span>

* [<span data-ttu-id="b226f-197">Wersja usługi youtube w tym samouczku</span><span class="sxs-lookup"><span data-stu-id="b226f-197">Youtube version of this tutorial</span></span>](https://www.youtube.com/watch?v=F0SP7Ry4flQ&feature=youtu.be)

## <a name="next-steps"></a><span data-ttu-id="b226f-198">Następne kroki</span><span class="sxs-lookup"><span data-stu-id="b226f-198">Next steps</span></span>

<span data-ttu-id="b226f-199">W ramach tego samouczka możesz:</span><span class="sxs-lookup"><span data-stu-id="b226f-199">In this tutorial, you:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="b226f-200">Utworzona aplikacja internetowa ze stronami Razor.</span><span class="sxs-lookup"><span data-stu-id="b226f-200">Created a Razor Pages web app.</span></span>
> * <span data-ttu-id="b226f-201">Uruchomienia aplikacji.</span><span class="sxs-lookup"><span data-stu-id="b226f-201">Ran the app.</span></span>
> * <span data-ttu-id="b226f-202">Zbadane plików projektu.</span><span class="sxs-lookup"><span data-stu-id="b226f-202">Examined the project files.</span></span>

<span data-ttu-id="b226f-203">Przejdź do następnego samouczka w serii:</span><span class="sxs-lookup"><span data-stu-id="b226f-203">Advance to the next tutorial in the series:</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="b226f-204">Dodawanie modelu</span><span class="sxs-lookup"><span data-stu-id="b226f-204">Add a model</span></span>](xref:tutorials/razor-pages/model)
