---
title: Debuguj ASP.NET Core Blazor
author: guardrex
description: Dowiedz się, jak debugować aplikacje Blazor.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 08/22/2019
uid: blazor/debug
ms.openlocfilehash: e9477e504d32fd1dd5d6c87392386d1131f46e9f
ms.sourcegitcommit: 092061c4f6ef46ed2165fa84de6273d3786fb97e
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 09/13/2019
ms.locfileid: "70963995"
---
# <a name="debug-aspnet-core-blazor"></a><span data-ttu-id="97fae-103">Debuguj ASP.NET Core Blazor</span><span class="sxs-lookup"><span data-stu-id="97fae-103">Debug ASP.NET Core Blazor</span></span>

[<span data-ttu-id="97fae-104">Daniel Roth</span><span class="sxs-lookup"><span data-stu-id="97fae-104">Daniel Roth</span></span>](https://github.com/danroth27)

<span data-ttu-id="97fae-105">Istnieje *wczesna* pomoc techniczna dla debugowania Blazor aplikacje webassembly działające w zestawie webassembly w przeglądarce Chrome.</span><span class="sxs-lookup"><span data-stu-id="97fae-105">*Early* support exists for debugging Blazor WebAssembly apps running on WebAssembly in Chrome.</span></span>

<span data-ttu-id="97fae-106">Możliwości debugera są ograniczone.</span><span class="sxs-lookup"><span data-stu-id="97fae-106">Debugger capabilities are limited.</span></span> <span data-ttu-id="97fae-107">Dostępne scenariusze obejmują:</span><span class="sxs-lookup"><span data-stu-id="97fae-107">Available scenarios include:</span></span>

* <span data-ttu-id="97fae-108">Ustawianie i usuwanie punktów przerwania.</span><span class="sxs-lookup"><span data-stu-id="97fae-108">Set and remove breakpoints.</span></span>
* <span data-ttu-id="97fae-109">Pojedynczy krok (`F10`) za pomocą kodu lub wznowienia kodu`F8`().</span><span class="sxs-lookup"><span data-stu-id="97fae-109">Single-step (`F10`) through the code or resume (`F8`) code execution.</span></span>
* <span data-ttu-id="97fae-110">Na ekranie *Ustawienia lokalne* Obserwuj wartości wszelkich zmiennych lokalnych typu `int`, `string`, i `bool`.</span><span class="sxs-lookup"><span data-stu-id="97fae-110">In the *Locals* display, observe the values of any local variables of type `int`, `string`, and `bool`.</span></span>
* <span data-ttu-id="97fae-111">Zobacz stos wywołań, w tym łańcuchy wywołań, które pochodzą z języka JavaScript do platformy .NET i z programu .NET do języka JavaScript.</span><span class="sxs-lookup"><span data-stu-id="97fae-111">See the call stack, including call chains that go from JavaScript into .NET and from .NET to JavaScript.</span></span>

<span data-ttu-id="97fae-112">*Nie*można:</span><span class="sxs-lookup"><span data-stu-id="97fae-112">You *can't*:</span></span>

* <span data-ttu-id="97fae-113">Obserwuj wartości wszelkich zmiennych lokalnych `int`, które nie są, `string`lub `bool`.</span><span class="sxs-lookup"><span data-stu-id="97fae-113">Observe the values of any locals that aren't an `int`, `string`, or `bool`.</span></span>
* <span data-ttu-id="97fae-114">Obserwuj wartości właściwości lub pól klas.</span><span class="sxs-lookup"><span data-stu-id="97fae-114">Observe the values of any class properties or fields.</span></span>
* <span data-ttu-id="97fae-115">Umieść kursor nad zmiennymi, aby zobaczyć ich wartości.</span><span class="sxs-lookup"><span data-stu-id="97fae-115">Hover over variables to see their values.</span></span>
* <span data-ttu-id="97fae-116">Oceń wyrażenia w konsoli programu.</span><span class="sxs-lookup"><span data-stu-id="97fae-116">Evaluate expressions in the console.</span></span>
* <span data-ttu-id="97fae-117">Krok w ramach wywołań asynchronicznych.</span><span class="sxs-lookup"><span data-stu-id="97fae-117">Step across async calls.</span></span>
* <span data-ttu-id="97fae-118">Wykonuj większość innych typowych scenariuszy debugowania.</span><span class="sxs-lookup"><span data-stu-id="97fae-118">Perform most other ordinary debugging scenarios.</span></span>

<span data-ttu-id="97fae-119">Programowanie dalszych scenariuszy debugowania jest tym samym punktem zespołu inżynierów.</span><span class="sxs-lookup"><span data-stu-id="97fae-119">Development of further debugging scenarios is an on-going focus of the engineering team.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="97fae-120">Wymagania wstępne</span><span class="sxs-lookup"><span data-stu-id="97fae-120">Prerequisites</span></span>

<span data-ttu-id="97fae-121">Debugowanie wymaga jednej z następujących przeglądarek:</span><span class="sxs-lookup"><span data-stu-id="97fae-121">Debugging requires either of the following browsers:</span></span>

* <span data-ttu-id="97fae-122">Google Chrome (wersja 70 lub nowsza)</span><span class="sxs-lookup"><span data-stu-id="97fae-122">Google Chrome (version 70 or later)</span></span>
* <span data-ttu-id="97fae-123">Podgląd Microsoft Edge ([kanał dev Edge](https://www.microsoftedgeinsider.com))</span><span class="sxs-lookup"><span data-stu-id="97fae-123">Microsoft Edge preview ([Edge Dev Channel](https://www.microsoftedgeinsider.com))</span></span>

## <a name="procedure"></a><span data-ttu-id="97fae-124">Procedura</span><span class="sxs-lookup"><span data-stu-id="97fae-124">Procedure</span></span>

1. <span data-ttu-id="97fae-125">Uruchom aplikację webassembly Blazor w `Debug` konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="97fae-125">Run a Blazor WebAssembly app in `Debug` configuration.</span></span> <span data-ttu-id="97fae-126">Przekaż opcję do polecenia [Run dotnet](/dotnet/core/tools/dotnet-run) : `dotnet run --configuration Debug`. `--configuration Debug`</span><span class="sxs-lookup"><span data-stu-id="97fae-126">Pass the `--configuration Debug` option to the [dotnet run](/dotnet/core/tools/dotnet-run) command: `dotnet run --configuration Debug`.</span></span>
1. <span data-ttu-id="97fae-127">Uzyskaj dostęp do aplikacji w przeglądarce.</span><span class="sxs-lookup"><span data-stu-id="97fae-127">Access the app in the browser.</span></span>
1. <span data-ttu-id="97fae-128">Umieść fokus klawiatury w aplikacji, a nie na panelu Narzędzia deweloperskie.</span><span class="sxs-lookup"><span data-stu-id="97fae-128">Place the keyboard focus on the app, not the developer tools panel.</span></span> <span data-ttu-id="97fae-129">Po zainicjowaniu debugowania można zamknąć panel Narzędzia deweloperskie.</span><span class="sxs-lookup"><span data-stu-id="97fae-129">The developer tools panel can be closed when debugging is initiated.</span></span>
1. <span data-ttu-id="97fae-130">Wybierz następujący skrót klawiaturowy dotyczący Blazor:</span><span class="sxs-lookup"><span data-stu-id="97fae-130">Select the following Blazor-specific keyboard shortcut:</span></span>
   * <span data-ttu-id="97fae-131">`Shift+Alt+D`w systemie Windows/Linux</span><span class="sxs-lookup"><span data-stu-id="97fae-131">`Shift+Alt+D` on Windows/Linux</span></span>
   * <span data-ttu-id="97fae-132">`Shift+Cmd+D`na macOS</span><span class="sxs-lookup"><span data-stu-id="97fae-132">`Shift+Cmd+D` on macOS</span></span>
1. <span data-ttu-id="97fae-133">Postępuj zgodnie z instrukcjami wyświetlanymi na ekranie, aby ponownie uruchomić przeglądarkę z włączonym debugowaniem zdalnym.</span><span class="sxs-lookup"><span data-stu-id="97fae-133">Follow the steps listed on the screen to restart the browser with remote debugging enabled.</span></span>
1. <span data-ttu-id="97fae-134">Ponownie wybierz następujący skrót klawiaturowy dotyczący Blazor, aby rozpocząć sesję debugowania:</span><span class="sxs-lookup"><span data-stu-id="97fae-134">Select the following Blazor-specific keyboard shortcut once again to start the debug session:</span></span>
   * <span data-ttu-id="97fae-135">`Shift+Alt+D`w systemie Windows/Linux</span><span class="sxs-lookup"><span data-stu-id="97fae-135">`Shift+Alt+D` on Windows/Linux</span></span>
   * <span data-ttu-id="97fae-136">`Shift+Cmd+D`na macOS</span><span class="sxs-lookup"><span data-stu-id="97fae-136">`Shift+Cmd+D` on macOS</span></span>

## <a name="enable-remote-debugging"></a><span data-ttu-id="97fae-137">Włącz debugowanie zdalne</span><span class="sxs-lookup"><span data-stu-id="97fae-137">Enable remote debugging</span></span>

<span data-ttu-id="97fae-138">Jeśli debugowanie zdalne jest wyłączone, **nie można odnaleźć strony błędu karty przeglądarki możliwością debugowania w przeglądarce** Chrome.</span><span class="sxs-lookup"><span data-stu-id="97fae-138">If remote debugging is disabled, an **Unable to find debuggable browser tab** error page is generated by Chrome.</span></span> <span data-ttu-id="97fae-139">Strona błędu zawiera instrukcje dotyczące uruchamiania programu Chrome z otwartym portem debugowania, dzięki czemu serwer proxy debugowania Blazor może połączyć się z aplikacją.</span><span class="sxs-lookup"><span data-stu-id="97fae-139">The error page contains instructions for running Chrome with the debugging port open so that the Blazor debugging proxy can connect to the app.</span></span> <span data-ttu-id="97fae-140">*Zamknij wszystkie wystąpienia programu Chrome* i uruchom ponownie program Chrome zgodnie z instrukcją.</span><span class="sxs-lookup"><span data-stu-id="97fae-140">*Close all Chrome instances* and restart Chrome as instructed.</span></span>

## <a name="debug-the-app"></a><span data-ttu-id="97fae-141">Debugowanie aplikacji</span><span class="sxs-lookup"><span data-stu-id="97fae-141">Debug the app</span></span>

<span data-ttu-id="97fae-142">Gdy program Chrome jest uruchomiony z włączonym debugowaniem zdalnym, skrót klawiaturowy debugowania otwiera nową kartę debugera. Po chwili na karcie **źródła** zostanie wyświetlona lista zestawów .NET w aplikacji.</span><span class="sxs-lookup"><span data-stu-id="97fae-142">Once Chrome is running with remote debugging enabled, the debugging keyboard shortcut opens a new debugger tab. After a moment, the **Sources** tab shows a list of the .NET assemblies in the app.</span></span> <span data-ttu-id="97fae-143">Rozwiń każdy zestaw i Znajdź pliki źródłowe *CS*/ *. Razor* dostępne do debugowania.</span><span class="sxs-lookup"><span data-stu-id="97fae-143">Expand each assembly and find the *.cs*/*.razor* source files available for debugging.</span></span> <span data-ttu-id="97fae-144">Ustaw punkty przerwania, przełącz się z powrotem do karty aplikacji, a punkty przerwania są trafień, gdy kod jest wykonywany.</span><span class="sxs-lookup"><span data-stu-id="97fae-144">Set breakpoints, switch back to the app's tab, and the breakpoints are hit when the code executes.</span></span> <span data-ttu-id="97fae-145">Po trafieniu punktu przerwania pojedynczy krok (`F10`) przez wykonanie kodu kodu lub wznowienia (`F8`).</span><span class="sxs-lookup"><span data-stu-id="97fae-145">After a breakpoint is hit, single-step (`F10`) through the code or resume (`F8`) code execution normally.</span></span>

<span data-ttu-id="97fae-146">Blazor udostępnia serwer proxy debugowania, który implementuje [Protokół Chrome devtools](https://chromedevtools.github.io/devtools-protocol/) i rozszerza protokół z. Informacje specyficzne dla sieci.</span><span class="sxs-lookup"><span data-stu-id="97fae-146">Blazor provides a debugging proxy that implements the [Chrome DevTools Protocol](https://chromedevtools.github.io/devtools-protocol/) and augments the protocol with .NET-specific information.</span></span> <span data-ttu-id="97fae-147">Gdy skrót klawiaturowy debugowania zostanie naciśnięty, Blazor punkty Chrome DevTools na serwerze proxy.</span><span class="sxs-lookup"><span data-stu-id="97fae-147">When debugging keyboard shortcut is pressed, Blazor points the Chrome DevTools at the proxy.</span></span> <span data-ttu-id="97fae-148">Serwer proxy nawiązuje połączenie z oknem przeglądarki, które próbujesz debugować (w związku z tym trzeba włączyć debugowanie zdalne).</span><span class="sxs-lookup"><span data-stu-id="97fae-148">The proxy connects to the browser window you're seeking to debug (hence the need to enable remote debugging).</span></span>

## <a name="browser-source-maps"></a><span data-ttu-id="97fae-149">Mapy źródeł przeglądarki</span><span class="sxs-lookup"><span data-stu-id="97fae-149">Browser source maps</span></span>

<span data-ttu-id="97fae-150">Mapy źródeł przeglądarki umożliwiają przeglądarce mapowanie skompilowanych plików z powrotem do ich oryginalnych plików źródłowych i są często używane do debugowania po stronie klienta.</span><span class="sxs-lookup"><span data-stu-id="97fae-150">Browser source maps allow the browser to map compiled files back to their original source files and are commonly used for client-side debugging.</span></span> <span data-ttu-id="97fae-151">Jednak Blazor nie jest obecnie mapowany C# bezpośrednio na język JavaScript/WASM.</span><span class="sxs-lookup"><span data-stu-id="97fae-151">However, Blazor doesn't currently map C# directly to JavaScript/WASM.</span></span> <span data-ttu-id="97fae-152">Zamiast tego Blazor wykonuje interpretację IL w przeglądarce, dlatego mapy źródłowe nie są istotne.</span><span class="sxs-lookup"><span data-stu-id="97fae-152">Instead, Blazor does IL interpretation within the browser, so source maps aren't relevant.</span></span>

## <a name="troubleshooting-tip"></a><span data-ttu-id="97fae-153">Porady dotyczące rozwiązywania problemów</span><span class="sxs-lookup"><span data-stu-id="97fae-153">Troubleshooting tip</span></span>

<span data-ttu-id="97fae-154">Jeśli występują błędy, Poniższa Wskazówka może pomóc:</span><span class="sxs-lookup"><span data-stu-id="97fae-154">If you're running into errors, the following tip may help:</span></span>

<span data-ttu-id="97fae-155">Na karcie **debuger** Otwórz narzędzia deweloperskie w przeglądarce.</span><span class="sxs-lookup"><span data-stu-id="97fae-155">In the **Debugger** tab, open the developer tools in your browser.</span></span> <span data-ttu-id="97fae-156">W konsoli programu wykonaj `localStorage.clear()` polecenie, aby usunąć wszystkie punkty przerwania.</span><span class="sxs-lookup"><span data-stu-id="97fae-156">In the console, execute `localStorage.clear()` to remove any breakpoints.</span></span>
