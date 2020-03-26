---
title: Debuguj ASP.NET Core Blazor webassembly
author: guardrex
description: Dowiedz się, jak debugować aplikacje Blazor.
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 03/26/2020
no-loc:
- Blazor
- SignalR
uid: blazor/debug
ms.openlocfilehash: 5dbc900ab68682068a7f9e3ffdaabef89a0c7798
ms.sourcegitcommit: 6ffb583991d6689326605a24565130083a28ef85
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/26/2020
ms.locfileid: "80306467"
---
# <a name="debug-aspnet-core-opno-locblazor-webassembly"></a><span data-ttu-id="bb5de-103">Debuguj ASP.NET Core Blazor webassembly</span><span class="sxs-lookup"><span data-stu-id="bb5de-103">Debug ASP.NET Core Blazor WebAssembly</span></span>

[<span data-ttu-id="bb5de-104">Daniel Roth</span><span class="sxs-lookup"><span data-stu-id="bb5de-104">Daniel Roth</span></span>](https://github.com/danroth27)

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

<span data-ttu-id="bb5de-105">aplikacje webassembly Blazor mogą być debugowane przy użyciu narzędzi deweloperskich przeglądarki w przeglądarkach opartych na chromie (Edge/Chrome).</span><span class="sxs-lookup"><span data-stu-id="bb5de-105">Blazor WebAssembly apps can be debugged using the browser dev tools in Chromium-based browsers (Edge/Chrome).</span></span>  <span data-ttu-id="bb5de-106">Alternatywnie możesz debugować aplikację przy użyciu programu Visual Studio lub Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="bb5de-106">Alternatively you can debug your app using Visual Studio or Visual Studio Code.</span></span>

<span data-ttu-id="bb5de-107">Dostępne scenariusze obejmują:</span><span class="sxs-lookup"><span data-stu-id="bb5de-107">Available scenarios include:</span></span>

* <span data-ttu-id="bb5de-108">Ustawianie i usuwanie punktów przerwania.</span><span class="sxs-lookup"><span data-stu-id="bb5de-108">Set and remove breakpoints.</span></span>
* <span data-ttu-id="bb5de-109">Uruchom aplikację z obsługą debugowania w programie Visual Studio i Visual Studio Code (Obsługa<kbd>F5</kbd> ).</span><span class="sxs-lookup"><span data-stu-id="bb5de-109">Run the app with debugging support in Visual Studio and Visual Studio Code (<kbd>F5</kbd> support).</span></span>
* <span data-ttu-id="bb5de-110">Jeden krok (<kbd>F10</kbd>) za pomocą kodu.</span><span class="sxs-lookup"><span data-stu-id="bb5de-110">Single-step (<kbd>F10</kbd>) through the code.</span></span>
* <span data-ttu-id="bb5de-111">Wznów wykonywanie kodu za pomocą <kbd>klawisza F8</kbd> w przeglądarce lub <kbd>F5</kbd> w programie Visual Studio lub Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="bb5de-111">Resume code execution with <kbd>F8</kbd> in a browser or <kbd>F5</kbd> in Visual Studio or Visual Studio Code.</span></span>
* <span data-ttu-id="bb5de-112">Na ekranie *Ustawienia lokalne* Obserwuj wartości zmiennych lokalnych.</span><span class="sxs-lookup"><span data-stu-id="bb5de-112">In the *Locals* display, observe the values of local variables.</span></span>
* <span data-ttu-id="bb5de-113">Zobacz stos wywołań, w tym łańcuchy wywołań, które pochodzą z języka JavaScript do platformy .NET i z programu .NET do języka JavaScript.</span><span class="sxs-lookup"><span data-stu-id="bb5de-113">See the call stack, including call chains that go from JavaScript into .NET and from .NET to JavaScript.</span></span>

<span data-ttu-id="bb5de-114">Obecnie *nie*można:</span><span class="sxs-lookup"><span data-stu-id="bb5de-114">For now, you *can't*:</span></span>

* <span data-ttu-id="bb5de-115">Inspekcja tablic.</span><span class="sxs-lookup"><span data-stu-id="bb5de-115">Inspect arrays.</span></span>
* <span data-ttu-id="bb5de-116">Umieść wskaźnik myszy, aby sprawdzić członków.</span><span class="sxs-lookup"><span data-stu-id="bb5de-116">Hover to inspect members.</span></span>
* <span data-ttu-id="bb5de-117">Wkrocz debugowanie do lub z kodu zarządzanego.</span><span class="sxs-lookup"><span data-stu-id="bb5de-117">Step debug into or out of managed code.</span></span>
* <span data-ttu-id="bb5de-118">Pełna obsługa inspekcji typów wartości.</span><span class="sxs-lookup"><span data-stu-id="bb5de-118">Have full support for inspecting value types.</span></span>
* <span data-ttu-id="bb5de-119">Przerwij w przypadku nieobsłużonych wyjątków.</span><span class="sxs-lookup"><span data-stu-id="bb5de-119">Break on unhandled exceptions.</span></span>
* <span data-ttu-id="bb5de-120">Trafij punkty przerwania podczas uruchamiania aplikacji.</span><span class="sxs-lookup"><span data-stu-id="bb5de-120">Hit breakpoints during app startup.</span></span>
* <span data-ttu-id="bb5de-121">Debugowanie aplikacji za pomocą procesu roboczego usługi.</span><span class="sxs-lookup"><span data-stu-id="bb5de-121">Debug an app with a service worker.</span></span>

<span data-ttu-id="bb5de-122">Będziemy nadal ulepszać środowisko debugowania w przyszłych wersjach.</span><span class="sxs-lookup"><span data-stu-id="bb5de-122">We will continue to improve the debugging experience in upcoming releases.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="bb5de-123">Wymagania wstępne</span><span class="sxs-lookup"><span data-stu-id="bb5de-123">Prerequisites</span></span>

<span data-ttu-id="bb5de-124">Debugowanie wymaga jednej z następujących przeglądarek:</span><span class="sxs-lookup"><span data-stu-id="bb5de-124">Debugging requires either of the following browsers:</span></span>

* <span data-ttu-id="bb5de-125">Microsoft Edge (wersja 80 lub nowsza)</span><span class="sxs-lookup"><span data-stu-id="bb5de-125">Microsoft Edge (version 80 or later)</span></span>
* <span data-ttu-id="bb5de-126">Google Chrome (wersja 70 lub nowsza)</span><span class="sxs-lookup"><span data-stu-id="bb5de-126">Google Chrome (version 70 or later)</span></span>

## <a name="enable-debugging-for-visual-studio-and-visual-studio-code"></a><span data-ttu-id="bb5de-127">Włącz debugowanie dla programu Visual Studio i Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="bb5de-127">Enable debugging for Visual Studio and Visual Studio Code</span></span>

<span data-ttu-id="bb5de-128">Debugowanie jest włączane automatycznie dla nowych projektów, które są tworzone przy użyciu ASP.NET Core 3,2 w wersji zapoznawczej 3 lub nowszej Blazor szablonu projektu webassembly.</span><span class="sxs-lookup"><span data-stu-id="bb5de-128">Debugging is enabled automatically for new projects that are created using the ASP.NET Core 3.2 Preview 3 or later Blazor WebAssembly project template.</span></span>

<span data-ttu-id="bb5de-129">Aby włączyć debugowanie istniejącej aplikacji Blazor webassembly, zaktualizuj plik *profilu launchsettings. JSON* w projekcie startowym w celu uwzględnienia następującej właściwości `inspectUri` w każdym profilu uruchamiania:</span><span class="sxs-lookup"><span data-stu-id="bb5de-129">To enable debugging for an existing Blazor WebAssembly app, update the *launchSettings.json* file in the startup project to include the following `inspectUri` property in each launch profile:</span></span>

```json
"inspectUri": "{wsProtocol}://{url.hostname}:{url.port}/_framework/debug/ws-proxy?browser={browserInspectUri}"
```

<span data-ttu-id="bb5de-130">Po zaktualizowaniu plik *profilu launchsettings. JSON* powinien wyglądać podobnie do poniższego przykładu:</span><span class="sxs-lookup"><span data-stu-id="bb5de-130">Once updated, the *launchSettings.json* file should look similar to the following example:</span></span>

[!code-json[](debug/launchSettings.json?highlight=14,22)]

<span data-ttu-id="bb5de-131">Właściwość `inspectUri`:</span><span class="sxs-lookup"><span data-stu-id="bb5de-131">The `inspectUri` property:</span></span>

* <span data-ttu-id="bb5de-132">Umożliwia środowisku IDE wykrywanie, czy aplikacja jest aplikacją Blazor webassembly.</span><span class="sxs-lookup"><span data-stu-id="bb5de-132">Enables the IDE to detect that the app is a Blazor WebAssembly app.</span></span>
* <span data-ttu-id="bb5de-133">Powoduje, że infrastruktura debugowania skryptów umożliwia łączenie się z przeglądarką za pomocą debugowania Blazorgo serwera proxy.</span><span class="sxs-lookup"><span data-stu-id="bb5de-133">Instructs the script debugging infrastructure to connect to the browser through Blazor's debugging proxy.</span></span>

## <a name="visual-studio"></a><span data-ttu-id="bb5de-134">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="bb5de-134">Visual Studio</span></span>

<span data-ttu-id="bb5de-135">Aby debugować aplikację webassembly Blazor w programie Visual Studio:</span><span class="sxs-lookup"><span data-stu-id="bb5de-135">To debug a Blazor WebAssembly app in Visual Studio:</span></span>

1. <span data-ttu-id="bb5de-136">Upewnij się [, że zainstalowano najnowszą wersję zapoznawczą programu Visual Studio 2019 16,6](https://visualstudio.com/preview) (wersja zapoznawcza 2 lub nowsza).</span><span class="sxs-lookup"><span data-stu-id="bb5de-136">Ensure you have [installed the latest preview release of Visual Studio 2019 16.6](https://visualstudio.com/preview) (Preview 2 or later).</span></span>
1. <span data-ttu-id="bb5de-137">Utwórz nową ASP.NET Core hostowanej Blazor aplikacji sieci webassembly.</span><span class="sxs-lookup"><span data-stu-id="bb5de-137">Create a new ASP.NET Core hosted Blazor WebAssembly app.</span></span>
1. <span data-ttu-id="bb5de-138">Naciśnij klawisz <kbd>F5</kbd> , aby uruchomić aplikację w debugerze.</span><span class="sxs-lookup"><span data-stu-id="bb5de-138">Press <kbd>F5</kbd> to run the app in the debugger.</span></span>
1. <span data-ttu-id="bb5de-139">Ustaw punkt przerwania w *liczniku Counter. Razor* w metodzie `IncrementCount`.</span><span class="sxs-lookup"><span data-stu-id="bb5de-139">Set a breakpoint in *Counter.razor* in the `IncrementCount` method.</span></span>
1. <span data-ttu-id="bb5de-140">Przejdź do karty **licznik** i wybierz przycisk, aby trafić w punkt przerwania:</span><span class="sxs-lookup"><span data-stu-id="bb5de-140">Browse to the **Counter** tab and select the button to hit the breakpoint:</span></span>

   ![Licznik debugowania](https://devblogs.microsoft.com/aspnet/wp-content/uploads/sites/16/2020/03/vs-debug-counter.png)

1. <span data-ttu-id="bb5de-142">Sprawdź wartość pola `currentCount` w oknie zmiennych lokalnych:</span><span class="sxs-lookup"><span data-stu-id="bb5de-142">Check out the value of the `currentCount` field in the locals window:</span></span>

   ![Wyświetl elementy lokalne](https://devblogs.microsoft.com/aspnet/wp-content/uploads/sites/16/2020/03/vs-debug-locals.png)

1. <span data-ttu-id="bb5de-144">Naciśnij klawisz <kbd>F5</kbd> , aby kontynuować wykonywanie.</span><span class="sxs-lookup"><span data-stu-id="bb5de-144">Press <kbd>F5</kbd> to continue execution.</span></span>

<span data-ttu-id="bb5de-145">Podczas debugowania aplikacji Blazor webassembly można także debugować kod serwera:</span><span class="sxs-lookup"><span data-stu-id="bb5de-145">While debugging your Blazor WebAssembly app, you can also debug your server code:</span></span>

1. <span data-ttu-id="bb5de-146">Ustaw punkt przerwania na stronie *FetchData. Razor* w `OnInitializedAsync`.</span><span class="sxs-lookup"><span data-stu-id="bb5de-146">Set a breakpoint in the *FetchData.razor* page in `OnInitializedAsync`.</span></span>
1. <span data-ttu-id="bb5de-147">Ustaw punkt przerwania w `WeatherForecastController` w metodzie `Get` akcji.</span><span class="sxs-lookup"><span data-stu-id="bb5de-147">Set a breakpoint in the `WeatherForecastController` in the `Get` action method.</span></span>
1. <span data-ttu-id="bb5de-148">Przejdź do karty **pobieranie danych** , aby trafić pierwszy punkt przerwania w składniku `FetchData` tuż przed wysłaniem żądania HTTP do serwera:</span><span class="sxs-lookup"><span data-stu-id="bb5de-148">Browse to the **Fetch Data** tab to hit the first breakpoint in the `FetchData` component just before it issues an HTTP request to the server:</span></span>

   ![Debuguj dane pobierania](https://devblogs.microsoft.com/aspnet/wp-content/uploads/sites/16/2020/03/vs-debug-fetch-data.png)

1. <span data-ttu-id="bb5de-150">Naciśnij klawisz <kbd>F5</kbd> , aby kontynuować wykonywanie, a następnie naciśnij punkt przerwania na serwerze w `WeatherForecastController`:</span><span class="sxs-lookup"><span data-stu-id="bb5de-150">Press <kbd>F5</kbd> to continue execution and then hit the breakpoint on the server in the `WeatherForecastController`:</span></span>

   ![Serwer debugowania](https://devblogs.microsoft.com/aspnet/wp-content/uploads/sites/16/2020/03/vs-debug-server.png)

1. <span data-ttu-id="bb5de-152">Naciśnij ponownie klawisz <kbd>F5</kbd> , aby umożliwić kontynuowanie wykonywania, a następnie zapoznaj się z renderowaną tabelą prognozy pogody.</span><span class="sxs-lookup"><span data-stu-id="bb5de-152">Press <kbd>F5</kbd> again to let execution continue and see the weather forecast table rendered.</span></span>

## <a name="visual-studio-code"></a><span data-ttu-id="bb5de-153">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="bb5de-153">Visual Studio Code</span></span>

<span data-ttu-id="bb5de-154">Aby debugować aplikację webassembly Blazor w programie Visual Studio Code:</span><span class="sxs-lookup"><span data-stu-id="bb5de-154">To debug a Blazor WebAssembly app in Visual Studio Code:</span></span>
 
1. <span data-ttu-id="bb5de-155">Zainstaluj [ C# rozszerzenie](https://marketplace.visualstudio.com/items?itemName=ms-dotnettools.csharp) oraz rozszerzenie [JavaScript Debugger (nocne)](https://marketplace.visualstudio.com/items?itemName=ms-vscode.js-debug-nightly) z `debug.javascript.usePreview` ustawionym na `true`.</span><span class="sxs-lookup"><span data-stu-id="bb5de-155">Install the [C# extension](https://marketplace.visualstudio.com/items?itemName=ms-dotnettools.csharp) and the [JavaScript Debugger (Nightly)](https://marketplace.visualstudio.com/items?itemName=ms-vscode.js-debug-nightly) extension with `debug.javascript.usePreview` set to `true`.</span></span>

   ![Rozszerzenia](https://devblogs.microsoft.com/aspnet/wp-content/uploads/sites/16/2020/03/vscode-extensions.png)

   ![Debuger podglądu JS](https://devblogs.microsoft.com/aspnet/wp-content/uploads/sites/16/2020/03/vscode-js-use-preview.png)

1. <span data-ttu-id="bb5de-158">Otwórz istniejącą aplikację Blazor webassembly z włączonym debugowaniem.</span><span class="sxs-lookup"><span data-stu-id="bb5de-158">Open an existing Blazor WebAssembly app with debugging enabled.</span></span>

   * <span data-ttu-id="bb5de-159">Jeśli otrzymujesz następujące powiadomienie, że do włączenia debugowania jest wymagana dodatkowa konfiguracja, upewnij się, że są zainstalowane odpowiednie rozszerzenia i włączono debugowanie JavaScript w wersji zapoznawczej, a następnie załaduj ponownie okno:</span><span class="sxs-lookup"><span data-stu-id="bb5de-159">If you get the following notification that additional setup is required to enable debugging, confirm that you have the correct extensions installed and JavaScript preview debugging enabled and then reload the window:</span></span>

     ![Dodatkowa konfiguracja wymagane](https://devblogs.microsoft.com/aspnet/wp-content/uploads/sites/16/2020/03/vscode-additional-setup.png)

   * <span data-ttu-id="bb5de-161">Powiadomienia umożliwiają dodanie wymaganych zasobów do aplikacji na potrzeby kompilowania i debugowania.</span><span class="sxs-lookup"><span data-stu-id="bb5de-161">A notification offers to add the required assets to the app for building and debugging.</span></span> <span data-ttu-id="bb5de-162">Wybierz opcję **tak**:</span><span class="sxs-lookup"><span data-stu-id="bb5de-162">Select **Yes**:</span></span>

     ![Dodaj wymagane zasoby](https://devblogs.microsoft.com/aspnet/wp-content/uploads/sites/16/2020/03/vscode-required-assets.png)

1. <span data-ttu-id="bb5de-164">Uruchamianie aplikacji w debugerze jest procesem dwuetapowym:</span><span class="sxs-lookup"><span data-stu-id="bb5de-164">Starting the app in the debugger is a two-step process:</span></span>

   <span data-ttu-id="bb5de-165">1\.</span><span class="sxs-lookup"><span data-stu-id="bb5de-165">1\.</span></span> <span data-ttu-id="bb5de-166">**Najpierw**Uruchom aplikację przy użyciu konfiguracji uruchamiania **uruchamiania programu .net Core (Blazor Standalone)** .</span><span class="sxs-lookup"><span data-stu-id="bb5de-166">**First**, start the app using the **.NET Core Launch (Blazor Standalone)** launch configuration.</span></span>

   <span data-ttu-id="bb5de-167">2\.</span><span class="sxs-lookup"><span data-stu-id="bb5de-167">2\.</span></span> <span data-ttu-id="bb5de-168">**Po rozpoczęciu uruchamiania aplikacji**Uruchom przeglądarkę, używając programu **.net core Debug Blazor zestawu sieci Web w** temacie Konfiguracja uruchamiania programu Chrome (wymaga programu Chrome).</span><span class="sxs-lookup"><span data-stu-id="bb5de-168">**After the app has started**, start the browser using the **.NET Core Debug Blazor Web Assembly in Chrome** launch configuration (requires Chrome).</span></span> <span data-ttu-id="bb5de-169">Aby użyć krawędzi zamiast przeglądarki Chrome, Zmień `type` konfiguracji uruchamiania w pliku *. programu vscode/Launch. JSON* z `pwa-chrome` na `pwa-edge`.</span><span class="sxs-lookup"><span data-stu-id="bb5de-169">To use Edge instead of Chrome, change the `type` of the launch configuration in *.vscode/launch.json* from `pwa-chrome` to `pwa-edge`.</span></span>

1. <span data-ttu-id="bb5de-170">Ustaw punkt przerwania w metodzie `IncrementCount` w składniku `Counter`, a następnie wybierz przycisk, aby trafić w punkt przerwania:</span><span class="sxs-lookup"><span data-stu-id="bb5de-170">Set a breakpoint in the `IncrementCount` method in the `Counter` component and then select the button to hit the breakpoint:</span></span>

   ![Debuguj licznik w VS Code](https://devblogs.microsoft.com/aspnet/wp-content/uploads/sites/16/2020/03/vscode-debug-counter.png)

## <a name="debug-in-the-browser"></a><span data-ttu-id="bb5de-172">Debuguj w przeglądarce</span><span class="sxs-lookup"><span data-stu-id="bb5de-172">Debug in the browser</span></span>

1. <span data-ttu-id="bb5de-173">Uruchom kompilację debugowania aplikacji w środowisku deweloperskim.</span><span class="sxs-lookup"><span data-stu-id="bb5de-173">Run a Debug build of the app in the Development environment.</span></span>

1. <span data-ttu-id="bb5de-174">Naciśnij klawisz <kbd>Shift</kbd>+<kbd>Alt</kbd>+<kbd>D</kbd>.</span><span class="sxs-lookup"><span data-stu-id="bb5de-174">Press <kbd>Shift</kbd>+<kbd>Alt</kbd>+<kbd>D</kbd>.</span></span>

1. <span data-ttu-id="bb5de-175">Przeglądarka musi być uruchomiona z włączonym debugowaniem zdalnym.</span><span class="sxs-lookup"><span data-stu-id="bb5de-175">The browser must be run with remote debugging enabled.</span></span> <span data-ttu-id="bb5de-176">Jeśli debugowanie zdalne jest wyłączone, **nie można odnaleźć strony błędu karty przeglądarki możliwością debugowania** .</span><span class="sxs-lookup"><span data-stu-id="bb5de-176">If remote debugging is disabled, an **Unable to find debuggable browser tab** error page is generated.</span></span> <span data-ttu-id="bb5de-177">Strona błędu zawiera instrukcje dotyczące uruchamiania przeglądarki z otwartym portem debugowania, dzięki czemu Blazor debugowania serwer proxy może połączyć się z aplikacją.</span><span class="sxs-lookup"><span data-stu-id="bb5de-177">The error page contains instructions for running the browser with the debugging port open so that the Blazor debugging proxy can connect to the app.</span></span> <span data-ttu-id="bb5de-178">*Zamknij wszystkie wystąpienia przeglądarki* i uruchom ponownie przeglądarkę zgodnie z instrukcjami.</span><span class="sxs-lookup"><span data-stu-id="bb5de-178">*Close all browser instances* and restart the browser as instructed.</span></span>

<span data-ttu-id="bb5de-179">Po uruchomieniu przeglądarki z włączonym debugowaniem zdalnym skrót klawiaturowy debugowania otwiera nową kartę debugera. Po chwili na karcie **źródła** zostanie wyświetlona lista zestawów .NET w aplikacji.</span><span class="sxs-lookup"><span data-stu-id="bb5de-179">Once the browser is running with remote debugging enabled, the debugging keyboard shortcut opens a new debugger tab. After a moment, the **Sources** tab shows a list of the .NET assemblies in the app.</span></span> <span data-ttu-id="bb5de-180">Rozwiń każdy zestaw i Znajdź *pliki źródłowe/.* *CS* dostępne do debugowania.</span><span class="sxs-lookup"><span data-stu-id="bb5de-180">Expand each assembly and find the *.cs*/*.razor* source files available for debugging.</span></span> <span data-ttu-id="bb5de-181">Ustaw punkty przerwania, przełącz się z powrotem do karty aplikacji, a punkty przerwania są trafień, gdy kod jest wykonywany.</span><span class="sxs-lookup"><span data-stu-id="bb5de-181">Set breakpoints, switch back to the app's tab, and the breakpoints are hit when the code executes.</span></span> <span data-ttu-id="bb5de-182">Po trafieniu punktu przerwania pojedynczy krok (<kbd>F10</kbd>) za pomocą kodu lub wznowienia kodu (<kbd>F8</kbd>) normalnie.</span><span class="sxs-lookup"><span data-stu-id="bb5de-182">After a breakpoint is hit, single-step (<kbd>F10</kbd>) through the code or resume (<kbd>F8</kbd>) code execution normally.</span></span>

Blazor<span data-ttu-id="bb5de-183"> udostępnia serwer proxy debugowania, który implementuje [Protokół Chrome devtools](https://chromedevtools.github.io/devtools-protocol/) i rozszerza protokół z. Informacje specyficzne dla sieci.</span><span class="sxs-lookup"><span data-stu-id="bb5de-183"> provides a debugging proxy that implements the [Chrome DevTools Protocol](https://chromedevtools.github.io/devtools-protocol/) and augments the protocol with .NET-specific information.</span></span> <span data-ttu-id="bb5de-184">Podczas naciskania skrótu klawiaturowego, Blazor wskazywał DevTools na serwerze proxy.</span><span class="sxs-lookup"><span data-stu-id="bb5de-184">When debugging keyboard shortcut is pressed, Blazor points the Chrome DevTools at the proxy.</span></span> <span data-ttu-id="bb5de-185">Serwer proxy nawiązuje połączenie z oknem przeglądarki, które próbujesz debugować (w związku z tym trzeba włączyć debugowanie zdalne).</span><span class="sxs-lookup"><span data-stu-id="bb5de-185">The proxy connects to the browser window you're seeking to debug (hence the need to enable remote debugging).</span></span>

## <a name="browser-source-maps"></a><span data-ttu-id="bb5de-186">Mapy źródeł przeglądarki</span><span class="sxs-lookup"><span data-stu-id="bb5de-186">Browser source maps</span></span>

<span data-ttu-id="bb5de-187">Mapy źródeł przeglądarki umożliwiają przeglądarce mapowanie skompilowanych plików z powrotem do ich oryginalnych plików źródłowych i są często używane do debugowania po stronie klienta.</span><span class="sxs-lookup"><span data-stu-id="bb5de-187">Browser source maps allow the browser to map compiled files back to their original source files and are commonly used for client-side debugging.</span></span> <span data-ttu-id="bb5de-188">Jednak Blazor nie są obecnie mapowane C# bezpośrednio do języka JavaScript/WASM.</span><span class="sxs-lookup"><span data-stu-id="bb5de-188">However, Blazor doesn't currently map C# directly to JavaScript/WASM.</span></span> <span data-ttu-id="bb5de-189">Zamiast tego Blazor wykonuje interpretację IL w przeglądarce, dlatego mapy źródłowe nie są istotne.</span><span class="sxs-lookup"><span data-stu-id="bb5de-189">Instead, Blazor does IL interpretation within the browser, so source maps aren't relevant.</span></span>

## <a name="troubleshoot"></a><span data-ttu-id="bb5de-190">Rozwiązywanie problemów</span><span class="sxs-lookup"><span data-stu-id="bb5de-190">Troubleshoot</span></span>

<span data-ttu-id="bb5de-191">Jeśli występują błędy, Poniższa Wskazówka może pomóc:</span><span class="sxs-lookup"><span data-stu-id="bb5de-191">If you're running into errors, the following tip may help:</span></span>

<span data-ttu-id="bb5de-192">Na karcie **debuger** Otwórz narzędzia deweloperskie w przeglądarce.</span><span class="sxs-lookup"><span data-stu-id="bb5de-192">In the **Debugger** tab, open the developer tools in your browser.</span></span> <span data-ttu-id="bb5de-193">W konsoli programu wykonaj `localStorage.clear()`, aby usunąć wszystkie punkty przerwania.</span><span class="sxs-lookup"><span data-stu-id="bb5de-193">In the console, execute `localStorage.clear()` to remove any breakpoints.</span></span>
