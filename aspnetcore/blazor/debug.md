---
title: Debuguj ASP.NET Core Blazor
author: guardrex
description: Dowiedz się, jak debugować aplikacje Blazor.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 10/15/2019
no-loc:
- Blazor
uid: blazor/debug
ms.openlocfilehash: 3096ad9b3a6904804f239d61f374adcd4d851978
ms.sourcegitcommit: 3fc3020961e1289ee5bf5f3c365ce8304d8ebf19
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/12/2019
ms.locfileid: "73963145"
---
# <a name="debug-aspnet-core-opno-locblazor"></a><span data-ttu-id="44b1f-103">Debuguj ASP.NET Core Blazor</span><span class="sxs-lookup"><span data-stu-id="44b1f-103">Debug ASP.NET Core Blazor</span></span>

[<span data-ttu-id="44b1f-104">Daniel Roth</span><span class="sxs-lookup"><span data-stu-id="44b1f-104">Daniel Roth</span></span>](https://github.com/danroth27)

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

<span data-ttu-id="44b1f-105">Istnieje *wczesna* pomoc techniczna dla debugowania Blazor aplikacje webassembly działające w zestawie webassembly w przeglądarce Chrome.</span><span class="sxs-lookup"><span data-stu-id="44b1f-105">*Early* support exists for debugging Blazor WebAssembly apps running on WebAssembly in Chrome.</span></span>

<span data-ttu-id="44b1f-106">Możliwości debugera są ograniczone.</span><span class="sxs-lookup"><span data-stu-id="44b1f-106">Debugger capabilities are limited.</span></span> <span data-ttu-id="44b1f-107">Dostępne scenariusze obejmują:</span><span class="sxs-lookup"><span data-stu-id="44b1f-107">Available scenarios include:</span></span>

* <span data-ttu-id="44b1f-108">Ustawianie i usuwanie punktów przerwania.</span><span class="sxs-lookup"><span data-stu-id="44b1f-108">Set and remove breakpoints.</span></span>
* <span data-ttu-id="44b1f-109">Pojedynczy krok (`F10`) przez wykonanie kodu lub wznowienie (`F8`).</span><span class="sxs-lookup"><span data-stu-id="44b1f-109">Single-step (`F10`) through the code or resume (`F8`) code execution.</span></span>
* <span data-ttu-id="44b1f-110">Na ekranie *Ustawienia lokalne* Obserwuj wartości wszelkich zmiennych lokalnych typu `int`, `string` i `bool`.</span><span class="sxs-lookup"><span data-stu-id="44b1f-110">In the *Locals* display, observe the values of any local variables of type `int`, `string`, and `bool`.</span></span>
* <span data-ttu-id="44b1f-111">Zobacz stos wywołań, w tym łańcuchy wywołań, które pochodzą z języka JavaScript do platformy .NET i z programu .NET do języka JavaScript.</span><span class="sxs-lookup"><span data-stu-id="44b1f-111">See the call stack, including call chains that go from JavaScript into .NET and from .NET to JavaScript.</span></span>

<span data-ttu-id="44b1f-112">*Nie*można:</span><span class="sxs-lookup"><span data-stu-id="44b1f-112">You *can't*:</span></span>

* <span data-ttu-id="44b1f-113">Zwróć uwagę na wartości dowolnych ustawień regionalnych, które nie są `int`, `string` lub `bool`.</span><span class="sxs-lookup"><span data-stu-id="44b1f-113">Observe the values of any locals that aren't an `int`, `string`, or `bool`.</span></span>
* <span data-ttu-id="44b1f-114">Obserwuj wartości właściwości lub pól klas.</span><span class="sxs-lookup"><span data-stu-id="44b1f-114">Observe the values of any class properties or fields.</span></span>
* <span data-ttu-id="44b1f-115">Umieść kursor nad zmiennymi, aby zobaczyć ich wartości.</span><span class="sxs-lookup"><span data-stu-id="44b1f-115">Hover over variables to see their values.</span></span>
* <span data-ttu-id="44b1f-116">Oceń wyrażenia w konsoli programu.</span><span class="sxs-lookup"><span data-stu-id="44b1f-116">Evaluate expressions in the console.</span></span>
* <span data-ttu-id="44b1f-117">Krok w ramach wywołań asynchronicznych.</span><span class="sxs-lookup"><span data-stu-id="44b1f-117">Step across async calls.</span></span>
* <span data-ttu-id="44b1f-118">Wykonuj większość innych typowych scenariuszy debugowania.</span><span class="sxs-lookup"><span data-stu-id="44b1f-118">Perform most other ordinary debugging scenarios.</span></span>

<span data-ttu-id="44b1f-119">Programowanie dalszych scenariuszy debugowania jest tym samym punktem zespołu inżynierów.</span><span class="sxs-lookup"><span data-stu-id="44b1f-119">Development of further debugging scenarios is an on-going focus of the engineering team.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="44b1f-120">Wymagania wstępne</span><span class="sxs-lookup"><span data-stu-id="44b1f-120">Prerequisites</span></span>

<span data-ttu-id="44b1f-121">Debugowanie wymaga jednej z następujących przeglądarek:</span><span class="sxs-lookup"><span data-stu-id="44b1f-121">Debugging requires either of the following browsers:</span></span>

* <span data-ttu-id="44b1f-122">Google Chrome (wersja 70 lub nowsza)</span><span class="sxs-lookup"><span data-stu-id="44b1f-122">Google Chrome (version 70 or later)</span></span>
* <span data-ttu-id="44b1f-123">Podgląd Microsoft Edge ([kanał dev Edge](https://www.microsoftedgeinsider.com))</span><span class="sxs-lookup"><span data-stu-id="44b1f-123">Microsoft Edge preview ([Edge Dev Channel](https://www.microsoftedgeinsider.com))</span></span>

## <a name="procedure"></a><span data-ttu-id="44b1f-124">Procedura</span><span class="sxs-lookup"><span data-stu-id="44b1f-124">Procedure</span></span>

1. <span data-ttu-id="44b1f-125">Uruchom aplikację Blazor webassembly w `Debug` Configuration.</span><span class="sxs-lookup"><span data-stu-id="44b1f-125">Run a Blazor WebAssembly app in `Debug` configuration.</span></span> <span data-ttu-id="44b1f-126">Przekaż opcję `--configuration Debug` do polecenia [Run dotnet](/dotnet/core/tools/dotnet-run) : `dotnet run --configuration Debug`.</span><span class="sxs-lookup"><span data-stu-id="44b1f-126">Pass the `--configuration Debug` option to the [dotnet run](/dotnet/core/tools/dotnet-run) command: `dotnet run --configuration Debug`.</span></span>
1. <span data-ttu-id="44b1f-127">Uzyskaj dostęp do aplikacji w przeglądarce.</span><span class="sxs-lookup"><span data-stu-id="44b1f-127">Access the app in the browser.</span></span>
1. <span data-ttu-id="44b1f-128">Umieść fokus klawiatury w aplikacji, a nie na panelu Narzędzia deweloperskie.</span><span class="sxs-lookup"><span data-stu-id="44b1f-128">Place the keyboard focus on the app, not the developer tools panel.</span></span> <span data-ttu-id="44b1f-129">Po zainicjowaniu debugowania można zamknąć panel Narzędzia deweloperskie.</span><span class="sxs-lookup"><span data-stu-id="44b1f-129">The developer tools panel can be closed when debugging is initiated.</span></span>
1. <span data-ttu-id="44b1f-130">Wybierz następujący Blazorskrót klawiaturowy:</span><span class="sxs-lookup"><span data-stu-id="44b1f-130">Select the following Blazor-specific keyboard shortcut:</span></span>
   * <span data-ttu-id="44b1f-131">`Shift+Alt+D` w systemie Windows/Linux</span><span class="sxs-lookup"><span data-stu-id="44b1f-131">`Shift+Alt+D` on Windows/Linux</span></span>
   * <span data-ttu-id="44b1f-132">`Shift+Cmd+D` w macOS</span><span class="sxs-lookup"><span data-stu-id="44b1f-132">`Shift+Cmd+D` on macOS</span></span>
1. <span data-ttu-id="44b1f-133">Postępuj zgodnie z instrukcjami wyświetlanymi na ekranie, aby ponownie uruchomić przeglądarkę z włączonym debugowaniem zdalnym.</span><span class="sxs-lookup"><span data-stu-id="44b1f-133">Follow the steps listed on the screen to restart the browser with remote debugging enabled.</span></span>
1. <span data-ttu-id="44b1f-134">Ponownie wybierz następujący skrót klawiaturowy dla Blazor, aby rozpocząć sesję debugowania:</span><span class="sxs-lookup"><span data-stu-id="44b1f-134">Select the following Blazor-specific keyboard shortcut once again to start the debug session:</span></span>
   * <span data-ttu-id="44b1f-135">`Shift+Alt+D` w systemie Windows/Linux</span><span class="sxs-lookup"><span data-stu-id="44b1f-135">`Shift+Alt+D` on Windows/Linux</span></span>
   * <span data-ttu-id="44b1f-136">`Shift+Cmd+D` w macOS</span><span class="sxs-lookup"><span data-stu-id="44b1f-136">`Shift+Cmd+D` on macOS</span></span>

## <a name="enable-remote-debugging"></a><span data-ttu-id="44b1f-137">Włącz debugowanie zdalne</span><span class="sxs-lookup"><span data-stu-id="44b1f-137">Enable remote debugging</span></span>

<span data-ttu-id="44b1f-138">Jeśli debugowanie zdalne jest wyłączone, **nie można odnaleźć strony błędu karty przeglądarki możliwością debugowania w przeglądarce** Chrome.</span><span class="sxs-lookup"><span data-stu-id="44b1f-138">If remote debugging is disabled, an **Unable to find debuggable browser tab** error page is generated by Chrome.</span></span> <span data-ttu-id="44b1f-139">Strona błędu zawiera instrukcje dotyczące uruchamiania programu Chrome z otwartym portem debugowania, dzięki czemu serwer proxy debugowania Blazor może połączyć się z aplikacją.</span><span class="sxs-lookup"><span data-stu-id="44b1f-139">The error page contains instructions for running Chrome with the debugging port open so that the Blazor debugging proxy can connect to the app.</span></span> <span data-ttu-id="44b1f-140">*Zamknij wszystkie wystąpienia programu Chrome* i uruchom ponownie program Chrome zgodnie z instrukcją.</span><span class="sxs-lookup"><span data-stu-id="44b1f-140">*Close all Chrome instances* and restart Chrome as instructed.</span></span>

## <a name="debug-the-app"></a><span data-ttu-id="44b1f-141">Debugowanie aplikacji</span><span class="sxs-lookup"><span data-stu-id="44b1f-141">Debug the app</span></span>

<span data-ttu-id="44b1f-142">Gdy program Chrome jest uruchomiony z włączonym debugowaniem zdalnym, skrót klawiaturowy debugowania otwiera nową kartę debugera. Po chwili na karcie **źródła** zostanie wyświetlona lista zestawów .NET w aplikacji.</span><span class="sxs-lookup"><span data-stu-id="44b1f-142">Once Chrome is running with remote debugging enabled, the debugging keyboard shortcut opens a new debugger tab. After a moment, the **Sources** tab shows a list of the .NET assemblies in the app.</span></span> <span data-ttu-id="44b1f-143">Rozwiń każdy zestaw i Znajdź plik *. cs*/ *.* pliki źródłowe Razor dostępne do debugowania.</span><span class="sxs-lookup"><span data-stu-id="44b1f-143">Expand each assembly and find the *.cs*/*.razor* source files available for debugging.</span></span> <span data-ttu-id="44b1f-144">Ustaw punkty przerwania, przełącz się z powrotem do karty aplikacji, a punkty przerwania są trafień, gdy kod jest wykonywany.</span><span class="sxs-lookup"><span data-stu-id="44b1f-144">Set breakpoints, switch back to the app's tab, and the breakpoints are hit when the code executes.</span></span> <span data-ttu-id="44b1f-145">Po trafieniu punktu przerwania pojedynczy krok (`F10`) za pomocą kodu lub wznowienia kodu (`F8`) normalnie.</span><span class="sxs-lookup"><span data-stu-id="44b1f-145">After a breakpoint is hit, single-step (`F10`) through the code or resume (`F8`) code execution normally.</span></span>

Blazor<span data-ttu-id="44b1f-146"> udostępnia serwer proxy debugowania, który implementuje [Protokół Chrome devtools](https://chromedevtools.github.io/devtools-protocol/) i rozszerza protokół z. Informacje specyficzne dla sieci.</span><span class="sxs-lookup"><span data-stu-id="44b1f-146"> provides a debugging proxy that implements the [Chrome DevTools Protocol](https://chromedevtools.github.io/devtools-protocol/) and augments the protocol with .NET-specific information.</span></span> <span data-ttu-id="44b1f-147">Podczas naciskania skrótu klawiaturowego, Blazor wskazywał DevTools na serwerze proxy.</span><span class="sxs-lookup"><span data-stu-id="44b1f-147">When debugging keyboard shortcut is pressed, Blazor points the Chrome DevTools at the proxy.</span></span> <span data-ttu-id="44b1f-148">Serwer proxy nawiązuje połączenie z oknem przeglądarki, które próbujesz debugować (w związku z tym trzeba włączyć debugowanie zdalne).</span><span class="sxs-lookup"><span data-stu-id="44b1f-148">The proxy connects to the browser window you're seeking to debug (hence the need to enable remote debugging).</span></span>

## <a name="browser-source-maps"></a><span data-ttu-id="44b1f-149">Mapy źródeł przeglądarki</span><span class="sxs-lookup"><span data-stu-id="44b1f-149">Browser source maps</span></span>

<span data-ttu-id="44b1f-150">Mapy źródeł przeglądarki umożliwiają przeglądarce mapowanie skompilowanych plików z powrotem do ich oryginalnych plików źródłowych i są często używane do debugowania po stronie klienta.</span><span class="sxs-lookup"><span data-stu-id="44b1f-150">Browser source maps allow the browser to map compiled files back to their original source files and are commonly used for client-side debugging.</span></span> <span data-ttu-id="44b1f-151">Jednak Blazor nie są obecnie mapowane C# bezpośrednio do języka JavaScript/WASM.</span><span class="sxs-lookup"><span data-stu-id="44b1f-151">However, Blazor doesn't currently map C# directly to JavaScript/WASM.</span></span> <span data-ttu-id="44b1f-152">Zamiast tego Blazor wykonuje interpretację IL w przeglądarce, dlatego mapy źródłowe nie są istotne.</span><span class="sxs-lookup"><span data-stu-id="44b1f-152">Instead, Blazor does IL interpretation within the browser, so source maps aren't relevant.</span></span>

## <a name="troubleshooting-tip"></a><span data-ttu-id="44b1f-153">Porady dotyczące rozwiązywania problemów</span><span class="sxs-lookup"><span data-stu-id="44b1f-153">Troubleshooting tip</span></span>

<span data-ttu-id="44b1f-154">Jeśli występują błędy, Poniższa Wskazówka może pomóc:</span><span class="sxs-lookup"><span data-stu-id="44b1f-154">If you're running into errors, the following tip may help:</span></span>

<span data-ttu-id="44b1f-155">Na karcie **debuger** Otwórz narzędzia deweloperskie w przeglądarce.</span><span class="sxs-lookup"><span data-stu-id="44b1f-155">In the **Debugger** tab, open the developer tools in your browser.</span></span> <span data-ttu-id="44b1f-156">W konsoli programu wykonaj `localStorage.clear()`, aby usunąć wszystkie punkty przerwania.</span><span class="sxs-lookup"><span data-stu-id="44b1f-156">In the console, execute `localStorage.clear()` to remove any breakpoints.</span></span>
