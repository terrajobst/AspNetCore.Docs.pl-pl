---
title: Debug ASP.NET Core Blazor
author: guardrex
description: Dowiedz się, jak można debugować aplikacje Blazor.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 06/14/2019
uid: blazor/debug
ms.openlocfilehash: 887edcd1db6942ba163857d48adfcf8efc8d7f5a
ms.sourcegitcommit: 4ef0362ef8b6e5426fc5af18f22734158fe587e1
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/17/2019
ms.locfileid: "67152704"
---
# <a name="debug-aspnet-core-blazor"></a><span data-ttu-id="1adc7-103">Debug ASP.NET Core Blazor</span><span class="sxs-lookup"><span data-stu-id="1adc7-103">Debug ASP.NET Core Blazor</span></span>

[<span data-ttu-id="1adc7-104">Daniel Roth</span><span class="sxs-lookup"><span data-stu-id="1adc7-104">Daniel Roth</span></span>](https://github.com/danroth27)

<span data-ttu-id="1adc7-105">*Wczesne* Obsługa istnieje, do debugowania po stronie klienta Blazor aplikacji uruchomionych na format WebAssembly w przeglądarce Chrome.</span><span class="sxs-lookup"><span data-stu-id="1adc7-105">*Early* support exists for debugging client-side Blazor apps running on WebAssembly in Chrome.</span></span>

<span data-ttu-id="1adc7-106">Debuger możliwości są ograniczone.</span><span class="sxs-lookup"><span data-stu-id="1adc7-106">Debugger capabilities are limited.</span></span> <span data-ttu-id="1adc7-107">Dostępne scenariusze obejmują:</span><span class="sxs-lookup"><span data-stu-id="1adc7-107">Available scenarios include:</span></span>

* <span data-ttu-id="1adc7-108">Ustaw i usuwanie punktów przerwania.</span><span class="sxs-lookup"><span data-stu-id="1adc7-108">Set and remove breakpoints.</span></span>
* <span data-ttu-id="1adc7-109">Pojedynczy krok (`F10`) za pomocą kodu lub Wznów (`F8`) wykonanie kodu.</span><span class="sxs-lookup"><span data-stu-id="1adc7-109">Single-step (`F10`) through the code or resume (`F8`) code execution.</span></span>
* <span data-ttu-id="1adc7-110">W *lokalne* wyświetlane, sprawdź wartości wszelkich zmiennych lokalnych typu `int`, `string`, i `bool`.</span><span class="sxs-lookup"><span data-stu-id="1adc7-110">In the *Locals* display, observe the values of any local variables of type `int`, `string`, and `bool`.</span></span>
* <span data-ttu-id="1adc7-111">Zobaczyć stos wywołań, w tym łańcuchy wywołania, które bardziej szczegółowo w kodzie JavaScript z języka JavaScript w środowisku .NET i .NET.</span><span class="sxs-lookup"><span data-stu-id="1adc7-111">See the call stack, including call chains that go from JavaScript into .NET and from .NET to JavaScript.</span></span>

<span data-ttu-id="1adc7-112">Możesz *nie*:</span><span class="sxs-lookup"><span data-stu-id="1adc7-112">You *can't*:</span></span>

* <span data-ttu-id="1adc7-113">Sprawdź wartości wszelkich zmiennych lokalnych, które nie są `int`, `string`, lub `bool`.</span><span class="sxs-lookup"><span data-stu-id="1adc7-113">Observe the values of any locals that aren't an `int`, `string`, or `bool`.</span></span>
* <span data-ttu-id="1adc7-114">Sprawdź wartości właściwości klasy lub pola.</span><span class="sxs-lookup"><span data-stu-id="1adc7-114">Observe the values of any class properties or fields.</span></span>
* <span data-ttu-id="1adc7-115">Umieść kursor nad zmienne, aby zobaczyć ich wartości.</span><span class="sxs-lookup"><span data-stu-id="1adc7-115">Hover over variables to see their values.</span></span>
* <span data-ttu-id="1adc7-116">Ocena wyrażeń w konsoli.</span><span class="sxs-lookup"><span data-stu-id="1adc7-116">Evaluate expressions in the console.</span></span>
* <span data-ttu-id="1adc7-117">Przejść przez wywołania asynchronicznego.</span><span class="sxs-lookup"><span data-stu-id="1adc7-117">Step across async calls.</span></span>
* <span data-ttu-id="1adc7-118">Wykonywać większości innych zwykłych scenariuszy debugowania.</span><span class="sxs-lookup"><span data-stu-id="1adc7-118">Perform most other ordinary debugging scenarios.</span></span>

<span data-ttu-id="1adc7-119">Rozwój dalszego debugowania scenariusze jest w toku celem zespołu inżynieryjnego.</span><span class="sxs-lookup"><span data-stu-id="1adc7-119">Development of further debugging scenarios is an on-going focus of the engineering team.</span></span>

## <a name="procedure"></a><span data-ttu-id="1adc7-120">Procedura</span><span class="sxs-lookup"><span data-stu-id="1adc7-120">Procedure</span></span>

<span data-ttu-id="1adc7-121">Aby debugować aplikację Blazor po stronie klienta w przeglądarce Chrome:</span><span class="sxs-lookup"><span data-stu-id="1adc7-121">To debug a client-side Blazor app in Chrome:</span></span>

* <span data-ttu-id="1adc7-122">Tworzenie aplikacji Blazor w `Debug` konfiguracji (domyślnie dla nieopublikowane aplikacje).</span><span class="sxs-lookup"><span data-stu-id="1adc7-122">Build a Blazor app in `Debug` configuration (the default for unpublished apps).</span></span>
* <span data-ttu-id="1adc7-123">Uruchom aplikację Blazor w przeglądarce Chrome (wersja 70 lub nowszej).</span><span class="sxs-lookup"><span data-stu-id="1adc7-123">Run the Blazor app in Chrome (version 70 or later).</span></span>
* <span data-ttu-id="1adc7-124">Fokus klawiatury w aplikacji (nie w panelu Narzędzia dla deweloperów, które prawdopodobnie należy zamknąć mniej mylące środowisko debugowania) wybierz następujący skrót klawiaturowy specyficzne dla Blazor:</span><span class="sxs-lookup"><span data-stu-id="1adc7-124">With the keyboard focus on the app (not in the developer tools panel, which you should probably close for a less confusing debugging experience), select the following Blazor-specific keyboard shortcut:</span></span>
  * <span data-ttu-id="1adc7-125">`Shift+Alt+D` w systemie Windows/Linux</span><span class="sxs-lookup"><span data-stu-id="1adc7-125">`Shift+Alt+D` on Windows/Linux</span></span>
  * <span data-ttu-id="1adc7-126">`Shift+Cmd+D` W systemie macOS</span><span class="sxs-lookup"><span data-stu-id="1adc7-126">`Shift+Cmd+D` on macOS</span></span>

## <a name="enable-remote-debugging"></a><span data-ttu-id="1adc7-127">Włączanie debugowania zdalnego</span><span class="sxs-lookup"><span data-stu-id="1adc7-127">Enable remote debugging</span></span>

<span data-ttu-id="1adc7-128">Jeśli zdalne debugowanie jest wyłączone, **nie można odnaleźć karty przeglądarki debugowania** strony błędu jest generowany przez przeglądarki Chrome.</span><span class="sxs-lookup"><span data-stu-id="1adc7-128">If remote debugging is disabled, an **Unable to find debuggable browser tab** error page is generated by Chrome.</span></span> <span data-ttu-id="1adc7-129">Strona błędu zawiera instrukcje na temat uruchamiania dla programu Chrome z portem debugowania Otwórz tak, aby serwer proxy debugowania Blazor podłączeniem do aplikacji.</span><span class="sxs-lookup"><span data-stu-id="1adc7-129">The error page contains instructions for running Chrome with the debugging port open so that the Blazor debugging proxy can connect to the app.</span></span> <span data-ttu-id="1adc7-130">*Zamknij wszystkie wystąpienia dla programu Chrome* i ponownie uruchomić przeglądarkę Chrome, zgodnie z instrukcjami.</span><span class="sxs-lookup"><span data-stu-id="1adc7-130">*Close all Chrome instances* and restart Chrome as instructed.</span></span>

## <a name="debug-the-app"></a><span data-ttu-id="1adc7-131">Debugowanie aplikacji</span><span class="sxs-lookup"><span data-stu-id="1adc7-131">Debug the app</span></span>

<span data-ttu-id="1adc7-132">Gdy program Chrome jest uruchomiony z włączonym debugowaniem zdalnym, debugowania skrótu klawiaturowego zostanie otwarta nowa karta debugera. Po chwili **źródeł** karta zawiera listę zestawów .NET w aplikacji.</span><span class="sxs-lookup"><span data-stu-id="1adc7-132">Once Chrome is running with remote debugging enabled, the debugging keyboard shortcut opens a new debugger tab. After a moment, the **Sources** tab shows a list of the .NET assemblies in the app.</span></span> <span data-ttu-id="1adc7-133">Rozwiń każdy zestaw i Znajdź *.cs*/ *.razor* dostępna do debugowania plików źródłowych.</span><span class="sxs-lookup"><span data-stu-id="1adc7-133">Expand each assembly and find the *.cs*/*.razor* source files available for debugging.</span></span> <span data-ttu-id="1adc7-134">Ustawianie punktów przerwania, wrócić do karty aplikacji, a punkty przerwania są osiągane, gdy kod jest wykonywany.</span><span class="sxs-lookup"><span data-stu-id="1adc7-134">Set breakpoints, switch back to the app's tab, and the breakpoints are hit when the code executes.</span></span> <span data-ttu-id="1adc7-135">Punkt przerwania — po trafienie w jednym kroku (`F10`) za pomocą kodu lub Wznów (`F8`) wykonanie kodu w zwykły sposób.</span><span class="sxs-lookup"><span data-stu-id="1adc7-135">After a breakpoint is hit, single-step (`F10`) through the code or resume (`F8`) code execution normally.</span></span>

<span data-ttu-id="1adc7-136">Blazor udostępnia serwer proxy debugowania, który implementuje [Chrome DevTools Protocol](https://chromedevtools.github.io/devtools-protocol/) i rozszerzają protokołu. Informacje dotyczące sieci.</span><span class="sxs-lookup"><span data-stu-id="1adc7-136">Blazor provides a debugging proxy that implements the [Chrome DevTools Protocol](https://chromedevtools.github.io/devtools-protocol/) and augments the protocol with .NET-specific information.</span></span> <span data-ttu-id="1adc7-137">Po naciśnięciu klawisza skrótu klawiaturowego debugowania Blazor wskazuje Chrome DevTools serwera proxy.</span><span class="sxs-lookup"><span data-stu-id="1adc7-137">When debugging keyboard shortcut is pressed, Blazor points the Chrome DevTools at the proxy.</span></span> <span data-ttu-id="1adc7-138">Serwer proxy łączy do okna przeglądarki, w przypadku znalezienia do debugowania (dlatego trzeba włączyć zdalne debugowanie).</span><span class="sxs-lookup"><span data-stu-id="1adc7-138">The proxy connects to the browser window you're seeking to debug (hence the need to enable remote debugging).</span></span>

## <a name="browser-source-maps"></a><span data-ttu-id="1adc7-139">Mapy źródła przeglądarki</span><span class="sxs-lookup"><span data-stu-id="1adc7-139">Browser source maps</span></span>

<span data-ttu-id="1adc7-140">Mapy źródła przeglądarki Zezwalaj na używanie przeglądarki zamapować skompilowane pliki do ich oryginalnych plików źródłowych i są często używane do debugowania po stronie klienta.</span><span class="sxs-lookup"><span data-stu-id="1adc7-140">Browser source maps allow the browser to map compiled files back to their original source files and are commonly used for client-side debugging.</span></span> <span data-ttu-id="1adc7-141">Jednak nie jest obecnie mapowany Blazor C# bezpośrednio do języka JavaScript/WASM.</span><span class="sxs-lookup"><span data-stu-id="1adc7-141">However, Blazor doesn't currently map C# directly to JavaScript/WASM.</span></span> <span data-ttu-id="1adc7-142">Zamiast tego Blazor wykonuje interpretacji IL w przeglądarce, więc map źródeł nie są istotne.</span><span class="sxs-lookup"><span data-stu-id="1adc7-142">Instead, Blazor does IL interpretation within the browser, so source maps aren't relevant.</span></span>

## <a name="troubleshooting-tip"></a><span data-ttu-id="1adc7-143">Porada dotyczącą rozwiązywania problemów</span><span class="sxs-lookup"><span data-stu-id="1adc7-143">Troubleshooting tip</span></span>

<span data-ttu-id="1adc7-144">Jeśli pracujesz w błędy, poniższe porady mogą pomóc:</span><span class="sxs-lookup"><span data-stu-id="1adc7-144">If you're running into errors, the following tip may help:</span></span>

<span data-ttu-id="1adc7-145">W **debugera** karcie, Otwórz narzędzia deweloperskie w przeglądarce.</span><span class="sxs-lookup"><span data-stu-id="1adc7-145">In the **Debugger** tab, open the developer tools in your browser.</span></span> <span data-ttu-id="1adc7-146">W konsoli, należy wykonać `localStorage.clear()` do usunięcia żadnych punktów przerwania.</span><span class="sxs-lookup"><span data-stu-id="1adc7-146">In the console, execute `localStorage.clear()` to remove any breakpoints.</span></span>
