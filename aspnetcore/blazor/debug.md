---
title: Debuguj ASP.NET Core Blazor
author: guardrex
description: Dowiedz się, jak debugować aplikacje Blazor.
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 01/16/2020
no-loc:
- Blazor
- SignalR
uid: blazor/debug
ms.openlocfilehash: 1b0035af48b82807a6ae14835a41a1ecbef06bb6
ms.sourcegitcommit: 9ee99300a48c810ca6fd4f7700cd95c3ccb85972
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/17/2020
ms.locfileid: "76159992"
---
# <a name="debug-aspnet-core-opno-locblazor"></a><span data-ttu-id="1be5b-103">Debuguj ASP.NET Core Blazor</span><span class="sxs-lookup"><span data-stu-id="1be5b-103">Debug ASP.NET Core Blazor</span></span>

[<span data-ttu-id="1be5b-104">Daniel Roth</span><span class="sxs-lookup"><span data-stu-id="1be5b-104">Daniel Roth</span></span>](https://github.com/danroth27)

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

<span data-ttu-id="1be5b-105">Istnieje *wczesna* obsługa debugowania Blazor webassembly przy użyciu narzędzi deweloperskich przeglądarki w przeglądarkach opartych na chromie (Chrome/Edge).</span><span class="sxs-lookup"><span data-stu-id="1be5b-105">*Early* support exists for debugging Blazor WebAssembly using the browser dev tools in Chromium-based browsers (Chrome/Edge).</span></span> <span data-ttu-id="1be5b-106">Prace są w toku do:</span><span class="sxs-lookup"><span data-stu-id="1be5b-106">Work is in progress to:</span></span>

* <span data-ttu-id="1be5b-107">W pełni Włącz debugowanie w programie Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="1be5b-107">Fully enable debugging in Visual Studio.</span></span>
* <span data-ttu-id="1be5b-108">Włącz debugowanie w Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="1be5b-108">Enable debugging in Visual Studio Code.</span></span>

<span data-ttu-id="1be5b-109">Możliwości debugera są ograniczone.</span><span class="sxs-lookup"><span data-stu-id="1be5b-109">Debugger capabilities are limited.</span></span> <span data-ttu-id="1be5b-110">Dostępne scenariusze obejmują:</span><span class="sxs-lookup"><span data-stu-id="1be5b-110">Available scenarios include:</span></span>

* <span data-ttu-id="1be5b-111">Ustawianie i usuwanie punktów przerwania.</span><span class="sxs-lookup"><span data-stu-id="1be5b-111">Set and remove breakpoints.</span></span>
* <span data-ttu-id="1be5b-112">Pojedynczy krok (`F10`) za pomocą kodu lub wznowienia kodu (`F8`).</span><span class="sxs-lookup"><span data-stu-id="1be5b-112">Single-step (`F10`) through the code or resume (`F8`) code execution.</span></span>
* <span data-ttu-id="1be5b-113">Na ekranie *Ustawienia lokalne* Obserwuj wartości wszelkich zmiennych lokalnych typu `int`, `string`i `bool`.</span><span class="sxs-lookup"><span data-stu-id="1be5b-113">In the *Locals* display, observe the values of any local variables of type `int`, `string`, and `bool`.</span></span>
* <span data-ttu-id="1be5b-114">Zobacz stos wywołań, w tym łańcuchy wywołań, które pochodzą z języka JavaScript do platformy .NET i z programu .NET do języka JavaScript.</span><span class="sxs-lookup"><span data-stu-id="1be5b-114">See the call stack, including call chains that go from JavaScript into .NET and from .NET to JavaScript.</span></span>

<span data-ttu-id="1be5b-115">*Nie*można:</span><span class="sxs-lookup"><span data-stu-id="1be5b-115">You *can't*:</span></span>

* <span data-ttu-id="1be5b-116">Obserwuj wartości dowolnych elementów lokalnych, które nie są `int`, `string`lub `bool`.</span><span class="sxs-lookup"><span data-stu-id="1be5b-116">Observe the values of any locals that aren't an `int`, `string`, or `bool`.</span></span>
* <span data-ttu-id="1be5b-117">Obserwuj wartości właściwości lub pól klas.</span><span class="sxs-lookup"><span data-stu-id="1be5b-117">Observe the values of any class properties or fields.</span></span>
* <span data-ttu-id="1be5b-118">Umieść kursor nad zmiennymi, aby zobaczyć ich wartości.</span><span class="sxs-lookup"><span data-stu-id="1be5b-118">Hover over variables to see their values.</span></span>
* <span data-ttu-id="1be5b-119">Oceń wyrażenia w konsoli programu.</span><span class="sxs-lookup"><span data-stu-id="1be5b-119">Evaluate expressions in the console.</span></span>
* <span data-ttu-id="1be5b-120">Krok w ramach wywołań asynchronicznych.</span><span class="sxs-lookup"><span data-stu-id="1be5b-120">Step across async calls.</span></span>
* <span data-ttu-id="1be5b-121">Wykonuj większość innych typowych scenariuszy debugowania.</span><span class="sxs-lookup"><span data-stu-id="1be5b-121">Perform most other ordinary debugging scenarios.</span></span>

<span data-ttu-id="1be5b-122">Programowanie dalszych scenariuszy debugowania jest tym samym punktem zespołu inżynierów.</span><span class="sxs-lookup"><span data-stu-id="1be5b-122">Development of further debugging scenarios is an on-going focus of the engineering team.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="1be5b-123">Wymagania wstępne</span><span class="sxs-lookup"><span data-stu-id="1be5b-123">Prerequisites</span></span>

<span data-ttu-id="1be5b-124">Debugowanie wymaga jednej z następujących przeglądarek:</span><span class="sxs-lookup"><span data-stu-id="1be5b-124">Debugging requires either of the following browsers:</span></span>

* <span data-ttu-id="1be5b-125">Google Chrome (wersja 70 lub nowsza)</span><span class="sxs-lookup"><span data-stu-id="1be5b-125">Google Chrome (version 70 or later)</span></span>
* <span data-ttu-id="1be5b-126">Podgląd Microsoft Edge ([kanał dev Edge](https://www.microsoftedgeinsider.com))</span><span class="sxs-lookup"><span data-stu-id="1be5b-126">Microsoft Edge preview ([Edge Dev Channel](https://www.microsoftedgeinsider.com))</span></span>

## <a name="procedure"></a><span data-ttu-id="1be5b-127">Procedura</span><span class="sxs-lookup"><span data-stu-id="1be5b-127">Procedure</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="1be5b-128">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="1be5b-128">Visual Studio</span></span>](#tab/visual-studio)

> [!WARNING]
> <span data-ttu-id="1be5b-129">Obsługa debugowania w programie Visual Studio jest wczesnym etapem opracowywania.</span><span class="sxs-lookup"><span data-stu-id="1be5b-129">Debugging support in Visual Studio is at an early stage of development.</span></span> <span data-ttu-id="1be5b-130">Debugowanie **F5** nie jest obecnie obsługiwane.</span><span class="sxs-lookup"><span data-stu-id="1be5b-130">**F5** debugging isn't currently supported.</span></span>

1. <span data-ttu-id="1be5b-131">Uruchom aplikację Blazor webassembly w konfiguracji `Debug` bez debugowania (**Ctrl**+**F5** zamiast **F5**).</span><span class="sxs-lookup"><span data-stu-id="1be5b-131">Run a Blazor WebAssembly app in `Debug` configuration without debugging (**Ctrl**+**F5** instead of **F5**).</span></span>
1. <span data-ttu-id="1be5b-132">Otwórz właściwości debugowania aplikacji (ostatni wpis w menu **debugowanie** ) i skopiuj **adres URL aplikacji**http.</span><span class="sxs-lookup"><span data-stu-id="1be5b-132">Open the Debug properties of the app (last entry in the **Debug** menu) and copy the HTTP **App URL**.</span></span> <span data-ttu-id="1be5b-133">Przejdź do adresu HTTP (nie adresu HTTPS) aplikacji przy użyciu przeglądarki opartej na formacie chrom (Edge beta lub Chrome).</span><span class="sxs-lookup"><span data-stu-id="1be5b-133">Browse to the HTTP address (not the HTTPS address) of the app using a Chromium-based browser (Edge Beta or Chrome).</span></span>
1. <span data-ttu-id="1be5b-134">Umieść fokus klawiatury w aplikacji w oknie przeglądarki, a nie na panelu Narzędzia deweloperskie.</span><span class="sxs-lookup"><span data-stu-id="1be5b-134">Place the keyboard focus on the app in the browser window, not the developer tools panel.</span></span> <span data-ttu-id="1be5b-135">Najlepiej jest pozostawić zamkniętą panel Narzędzia deweloperskie w celu wykonania tej procedury.</span><span class="sxs-lookup"><span data-stu-id="1be5b-135">It's best to keep the developer tools panel closed for this procedure.</span></span> <span data-ttu-id="1be5b-136">Po rozpoczęciu debugowania możesz ponownie otworzyć panel Narzędzia deweloperskie.</span><span class="sxs-lookup"><span data-stu-id="1be5b-136">After debugging has started, you can re-open the developer tools panel.</span></span>
1. <span data-ttu-id="1be5b-137">Wybierz następujący Blazorskrót klawiaturowy:</span><span class="sxs-lookup"><span data-stu-id="1be5b-137">Select the following Blazor-specific keyboard shortcut:</span></span>

   * <span data-ttu-id="1be5b-138">`Shift+Alt+D` w systemie Windows</span><span class="sxs-lookup"><span data-stu-id="1be5b-138">`Shift+Alt+D` on Windows</span></span>
   * <span data-ttu-id="1be5b-139">`Shift+Cmd+D` w macOS</span><span class="sxs-lookup"><span data-stu-id="1be5b-139">`Shift+Cmd+D` on macOS</span></span>

   <span data-ttu-id="1be5b-140">Jeśli zostanie wyświetlony komunikat **nie można znaleźć karty możliwością debugowania Browser**, zobacz [Włączanie debugowania zdalnego](#enable-remote-debugging).</span><span class="sxs-lookup"><span data-stu-id="1be5b-140">If you receive the **Unable to find debuggable browser tab**, see [Enable remote debugging](#enable-remote-debugging).</span></span>
   
   <span data-ttu-id="1be5b-141">Po włączeniu debugowania zdalnego:</span><span class="sxs-lookup"><span data-stu-id="1be5b-141">After enabling remote debugging:</span></span>
   
   <span data-ttu-id="1be5b-142">1\.</span><span class="sxs-lookup"><span data-stu-id="1be5b-142">1\.</span></span> <span data-ttu-id="1be5b-143">Zostanie otwarte nowe okno przeglądarki.</span><span class="sxs-lookup"><span data-stu-id="1be5b-143">A new browser window opens.</span></span> <span data-ttu-id="1be5b-144">Zamknij poprzednie okno.</span><span class="sxs-lookup"><span data-stu-id="1be5b-144">Close the prior window.</span></span>

   <span data-ttu-id="1be5b-145">2\.</span><span class="sxs-lookup"><span data-stu-id="1be5b-145">2\.</span></span> <span data-ttu-id="1be5b-146">Umieść fokus klawiatury w aplikacji w oknie przeglądarki.</span><span class="sxs-lookup"><span data-stu-id="1be5b-146">Place the keyboard focus on the app in the browser window.</span></span>

   <span data-ttu-id="1be5b-147">3 \.</span><span class="sxs-lookup"><span data-stu-id="1be5b-147">3\.</span></span> <span data-ttu-id="1be5b-148">Wybierz skrót klawiaturowy określony Blazorw nowym oknie przeglądarki: `Shift+Alt+D` w systemie Windows lub `Shift+Cmd+D` na macOS.</span><span class="sxs-lookup"><span data-stu-id="1be5b-148">Select the Blazor-specific keyboard shortcut in the new browser window: `Shift+Alt+D` on Windows or `Shift+Cmd+D` on macOS.</span></span>

   <span data-ttu-id="1be5b-149">4\.</span><span class="sxs-lookup"><span data-stu-id="1be5b-149">4\.</span></span> <span data-ttu-id="1be5b-150">Zostanie otwarta karta **devtools** w przeglądarce.</span><span class="sxs-lookup"><span data-stu-id="1be5b-150">The **DevTools** tab opens in the browser.</span></span> <span data-ttu-id="1be5b-151">**Wybierz kartę aplikacji w oknie przeglądarki.**</span><span class="sxs-lookup"><span data-stu-id="1be5b-151">**Reselect the app's tab in the browser window.**</span></span>

   <span data-ttu-id="1be5b-152">Aby dołączyć aplikację do programu Visual Studio, zobacz sekcję [dołączanie do procesu w programie Visual Studio](#attach-to-process-in-visual-studio) .</span><span class="sxs-lookup"><span data-stu-id="1be5b-152">To attach the app to Visual Studio, see the [Attach to process in Visual Studio](#attach-to-process-in-visual-studio) section.</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="1be5b-153">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="1be5b-153">.NET Core CLI</span></span>](#tab/netcore-cli/)

1. <span data-ttu-id="1be5b-154">Uruchom Blazor aplikację webassembly w konfiguracji `Debug`, przekazując opcję `--configuration Debug` do polecenia [Run dotnet](/dotnet/core/tools/dotnet-run) : `dotnet run --configuration Debug`.</span><span class="sxs-lookup"><span data-stu-id="1be5b-154">Run a Blazor WebAssembly app in `Debug` configuration by passing the `--configuration Debug` option to the [dotnet run](/dotnet/core/tools/dotnet-run) command: `dotnet run --configuration Debug`.</span></span>
1. <span data-ttu-id="1be5b-155">Przejdź do aplikacji w adresie URL protokołu HTTP pokazanej w oknie powłoki.</span><span class="sxs-lookup"><span data-stu-id="1be5b-155">Navigate to the app at the HTTP URL shown in the shell's window.</span></span>
1. <span data-ttu-id="1be5b-156">Umieść fokus klawiatury w aplikacji, a nie na panelu Narzędzia deweloperskie.</span><span class="sxs-lookup"><span data-stu-id="1be5b-156">Place the keyboard focus on the app, not the developer tools panel.</span></span> <span data-ttu-id="1be5b-157">Najlepiej jest pozostawić zamkniętą panel Narzędzia deweloperskie w celu wykonania tej procedury.</span><span class="sxs-lookup"><span data-stu-id="1be5b-157">It's best to keep the developer tools panel closed for this procedure.</span></span> <span data-ttu-id="1be5b-158">Po rozpoczęciu debugowania możesz ponownie otworzyć panel Narzędzia deweloperskie.</span><span class="sxs-lookup"><span data-stu-id="1be5b-158">After debugging has started, you can re-open the developer tools panel.</span></span>
1. <span data-ttu-id="1be5b-159">Wybierz następujący Blazorskrót klawiaturowy:</span><span class="sxs-lookup"><span data-stu-id="1be5b-159">Select the following Blazor-specific keyboard shortcut:</span></span>

   * <span data-ttu-id="1be5b-160">`Shift+Alt+D` w systemie Windows</span><span class="sxs-lookup"><span data-stu-id="1be5b-160">`Shift+Alt+D` on Windows</span></span>
   * <span data-ttu-id="1be5b-161">`Shift+Cmd+D` w macOS</span><span class="sxs-lookup"><span data-stu-id="1be5b-161">`Shift+Cmd+D` on macOS</span></span>

   <span data-ttu-id="1be5b-162">Jeśli zostanie wyświetlony komunikat **nie można znaleźć karty możliwością debugowania Browser**, zobacz [Włączanie debugowania zdalnego](#enable-remote-debugging).</span><span class="sxs-lookup"><span data-stu-id="1be5b-162">If you receive the **Unable to find debuggable browser tab**, see [Enable remote debugging](#enable-remote-debugging).</span></span>
   
   <span data-ttu-id="1be5b-163">Po włączeniu debugowania zdalnego:</span><span class="sxs-lookup"><span data-stu-id="1be5b-163">After enabling remote debugging:</span></span>
   
   <span data-ttu-id="1be5b-164">1\.</span><span class="sxs-lookup"><span data-stu-id="1be5b-164">1\.</span></span> <span data-ttu-id="1be5b-165">Zostanie otwarte nowe okno przeglądarki.</span><span class="sxs-lookup"><span data-stu-id="1be5b-165">A new browser window opens.</span></span> <span data-ttu-id="1be5b-166">Zamknij poprzednie okno.</span><span class="sxs-lookup"><span data-stu-id="1be5b-166">Close the prior window.</span></span>

   <span data-ttu-id="1be5b-167">2\.</span><span class="sxs-lookup"><span data-stu-id="1be5b-167">2\.</span></span> <span data-ttu-id="1be5b-168">Umieść fokus klawiatury w aplikacji w oknie przeglądarki, a nie na panelu Narzędzia deweloperskie.</span><span class="sxs-lookup"><span data-stu-id="1be5b-168">Place the keyboard focus on the app in the browser window, not the developer tools panel.</span></span>

   <span data-ttu-id="1be5b-169">3 \.</span><span class="sxs-lookup"><span data-stu-id="1be5b-169">3\.</span></span> <span data-ttu-id="1be5b-170">Wybierz skrót klawiaturowy określony Blazorw nowym oknie przeglądarki: `Shift+Alt+D` w systemie Windows lub `Shift+Cmd+D` na macOS.</span><span class="sxs-lookup"><span data-stu-id="1be5b-170">Select the Blazor-specific keyboard shortcut in the new browser window: `Shift+Alt+D` on Windows or `Shift+Cmd+D` on macOS.</span></span>

---

## <a name="enable-remote-debugging"></a><span data-ttu-id="1be5b-171">Włącz debugowanie zdalne</span><span class="sxs-lookup"><span data-stu-id="1be5b-171">Enable remote debugging</span></span>

<span data-ttu-id="1be5b-172">Jeśli debugowanie zdalne jest wyłączone, **nie można odnaleźć strony błędu karty przeglądarki możliwością debugowania w przeglądarce** Chrome.</span><span class="sxs-lookup"><span data-stu-id="1be5b-172">If remote debugging is disabled, an **Unable to find debuggable browser tab** error page is generated by Chrome.</span></span> <span data-ttu-id="1be5b-173">Strona błędu zawiera instrukcje dotyczące uruchamiania programu Chrome z otwartym portem debugowania, dzięki czemu serwer proxy debugowania Blazor może połączyć się z aplikacją.</span><span class="sxs-lookup"><span data-stu-id="1be5b-173">The error page contains instructions for running Chrome with the debugging port open so that the Blazor debugging proxy can connect to the app.</span></span> <span data-ttu-id="1be5b-174">*Zamknij wszystkie wystąpienia programu Chrome* i uruchom ponownie program Chrome zgodnie z instrukcją.</span><span class="sxs-lookup"><span data-stu-id="1be5b-174">*Close all Chrome instances* and restart Chrome as instructed.</span></span>

## <a name="debug-the-app"></a><span data-ttu-id="1be5b-175">Debugowanie aplikacji</span><span class="sxs-lookup"><span data-stu-id="1be5b-175">Debug the app</span></span>

<span data-ttu-id="1be5b-176">Gdy program Chrome jest uruchomiony z włączonym debugowaniem zdalnym, skrót klawiaturowy debugowania otwiera nową kartę debugera. Po chwili na karcie **źródła** zostanie wyświetlona lista zestawów .NET w aplikacji.</span><span class="sxs-lookup"><span data-stu-id="1be5b-176">Once Chrome is running with remote debugging enabled, the debugging keyboard shortcut opens a new debugger tab. After a moment, the **Sources** tab shows a list of the .NET assemblies in the app.</span></span> <span data-ttu-id="1be5b-177">Rozwiń każdy zestaw i Znajdź *pliki źródłowe/.* *CS* dostępne do debugowania.</span><span class="sxs-lookup"><span data-stu-id="1be5b-177">Expand each assembly and find the *.cs*/*.razor* source files available for debugging.</span></span> <span data-ttu-id="1be5b-178">Ustaw punkty przerwania, przełącz się z powrotem do karty aplikacji, a punkty przerwania są trafień, gdy kod jest wykonywany.</span><span class="sxs-lookup"><span data-stu-id="1be5b-178">Set breakpoints, switch back to the app's tab, and the breakpoints are hit when the code executes.</span></span> <span data-ttu-id="1be5b-179">Po trafieniu punktu przerwania pojedynczy krok (`F10`) za pomocą kodu lub wznowienia kodu (`F8`) normalnie.</span><span class="sxs-lookup"><span data-stu-id="1be5b-179">After a breakpoint is hit, single-step (`F10`) through the code or resume (`F8`) code execution normally.</span></span>

Blazor<span data-ttu-id="1be5b-180"> udostępnia serwer proxy debugowania, który implementuje [Protokół Chrome devtools](https://chromedevtools.github.io/devtools-protocol/) i rozszerza protokół z. Informacje specyficzne dla sieci.</span><span class="sxs-lookup"><span data-stu-id="1be5b-180"> provides a debugging proxy that implements the [Chrome DevTools Protocol](https://chromedevtools.github.io/devtools-protocol/) and augments the protocol with .NET-specific information.</span></span> <span data-ttu-id="1be5b-181">Podczas naciskania skrótu klawiaturowego, Blazor wskazywał DevTools na serwerze proxy.</span><span class="sxs-lookup"><span data-stu-id="1be5b-181">When debugging keyboard shortcut is pressed, Blazor points the Chrome DevTools at the proxy.</span></span> <span data-ttu-id="1be5b-182">Serwer proxy nawiązuje połączenie z oknem przeglądarki, które próbujesz debugować (w związku z tym trzeba włączyć debugowanie zdalne).</span><span class="sxs-lookup"><span data-stu-id="1be5b-182">The proxy connects to the browser window you're seeking to debug (hence the need to enable remote debugging).</span></span>

## <a name="attach-to-process-in-visual-studio"></a><span data-ttu-id="1be5b-183">Dołącz do procesu w programie Visual Studio</span><span class="sxs-lookup"><span data-stu-id="1be5b-183">Attach to process in Visual Studio</span></span>

<span data-ttu-id="1be5b-184">Dołączanie do procesu aplikacji w programie Visual Studio jest *tymczasowym* scenariuszem debugowania dla Blazor webassembly, podczas gdy debugowanie **F5** jest w trakcie projektowania.</span><span class="sxs-lookup"><span data-stu-id="1be5b-184">Attaching to the app's process in Visual Studio is a *temporary* debugging scenario for Blazor WebAssembly while **F5** debugging is in development.</span></span>

<span data-ttu-id="1be5b-185">Aby dołączyć proces uruchomionej aplikacji do programu Visual Studio:</span><span class="sxs-lookup"><span data-stu-id="1be5b-185">To attach the running app's process to Visual Studio:</span></span>

1. <span data-ttu-id="1be5b-186">W programie Visual Studio wybierz kolejno opcje **debuguj** > **Dołącz do procesu**.</span><span class="sxs-lookup"><span data-stu-id="1be5b-186">In Visual Studio, select **Debug** > **Attach to Process**.</span></span>
1. <span data-ttu-id="1be5b-187">W polu **Typ połączenia**wybierz pozycję **Chrome devtools Protocol WebSocket (bez uwierzytelniania)** .</span><span class="sxs-lookup"><span data-stu-id="1be5b-187">For the **Connection type**, select **Chrome devtools protocol websocket (no authentication)**.</span></span>
1. <span data-ttu-id="1be5b-188">W przypadku **celu połączenia**Wklej adres http (nie adres https) aplikacji.</span><span class="sxs-lookup"><span data-stu-id="1be5b-188">For the **Connection target**, paste in the HTTP address (not the HTTPS address) of the app.</span></span>
1. <span data-ttu-id="1be5b-189">Wybierz pozycję **Odśwież** , aby odświeżyć wpisy w obszarze **dostępne procesy**.</span><span class="sxs-lookup"><span data-stu-id="1be5b-189">Select **Refresh** to refresh the entries under **Available processes**.</span></span>
1. <span data-ttu-id="1be5b-190">Wybierz proces przeglądarki do debugowania i wybierz pozycję **Dołącz**.</span><span class="sxs-lookup"><span data-stu-id="1be5b-190">Select the browser process to debug and select **Attach**.</span></span>
1. <span data-ttu-id="1be5b-191">W oknie dialogowym **Wybieranie typu kodu** wybierz typ kodu dla konkretnej przeglądarki, do której dołączasz (Edge lub Chrome), a następnie wybierz **przycisk OK**.</span><span class="sxs-lookup"><span data-stu-id="1be5b-191">In the **Select Code Type** dialog, select the code type for the specific browser you're attaching to (Edge or Chrome) and then select **OK**.</span></span>

## <a name="browser-source-maps"></a><span data-ttu-id="1be5b-192">Mapy źródeł przeglądarki</span><span class="sxs-lookup"><span data-stu-id="1be5b-192">Browser source maps</span></span>

<span data-ttu-id="1be5b-193">Mapy źródeł przeglądarki umożliwiają przeglądarce mapowanie skompilowanych plików z powrotem do ich oryginalnych plików źródłowych i są często używane do debugowania po stronie klienta.</span><span class="sxs-lookup"><span data-stu-id="1be5b-193">Browser source maps allow the browser to map compiled files back to their original source files and are commonly used for client-side debugging.</span></span> <span data-ttu-id="1be5b-194">Jednak Blazor nie są obecnie mapowane C# bezpośrednio do języka JavaScript/WASM.</span><span class="sxs-lookup"><span data-stu-id="1be5b-194">However, Blazor doesn't currently map C# directly to JavaScript/WASM.</span></span> <span data-ttu-id="1be5b-195">Zamiast tego Blazor wykonuje interpretację IL w przeglądarce, dlatego mapy źródłowe nie są istotne.</span><span class="sxs-lookup"><span data-stu-id="1be5b-195">Instead, Blazor does IL interpretation within the browser, so source maps aren't relevant.</span></span>

## <a name="troubleshoot"></a><span data-ttu-id="1be5b-196">Rozwiązywanie problemów</span><span class="sxs-lookup"><span data-stu-id="1be5b-196">Troubleshoot</span></span>

<span data-ttu-id="1be5b-197">Jeśli występują błędy, Poniższa Wskazówka może pomóc:</span><span class="sxs-lookup"><span data-stu-id="1be5b-197">If you're running into errors, the following tip may help:</span></span>

<span data-ttu-id="1be5b-198">Na karcie **debuger** Otwórz narzędzia deweloperskie w przeglądarce.</span><span class="sxs-lookup"><span data-stu-id="1be5b-198">In the **Debugger** tab, open the developer tools in your browser.</span></span> <span data-ttu-id="1be5b-199">W konsoli programu wykonaj `localStorage.clear()`, aby usunąć wszystkie punkty przerwania.</span><span class="sxs-lookup"><span data-stu-id="1be5b-199">In the console, execute `localStorage.clear()` to remove any breakpoints.</span></span>
