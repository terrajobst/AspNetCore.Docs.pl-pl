---
title: Obsługa błędów w aplikacjach Blazor ASP.NET Core
author: guardrex
description: Odkryj, jak ASP.NET Core Blazor sposób, w jaki Blazor zarządza nieobsługiwanymi wyjątkami i jak opracowywać aplikacje wykrywające i obsługujące błędy.
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 01/22/2020
no-loc:
- Blazor
- SignalR
uid: blazor/handle-errors
ms.openlocfilehash: b987513e5410e95ab632b9935d858b648838d94f
ms.sourcegitcommit: 0b0e485a8a6dfcc65a7a58b365622b3839f4d624
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/01/2020
ms.locfileid: "76928275"
---
# <a name="handle-errors-in-aspnet-core-opno-locblazor-apps"></a><span data-ttu-id="1eb6e-103">Obsługa błędów w aplikacjach Blazor ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="1eb6e-103">Handle errors in ASP.NET Core Blazor apps</span></span>

<span data-ttu-id="1eb6e-104">[Steve Sanderson](https://github.com/SteveSandersonMS)</span><span class="sxs-lookup"><span data-stu-id="1eb6e-104">By [Steve Sanderson](https://github.com/SteveSandersonMS)</span></span>

<span data-ttu-id="1eb6e-105">W tym artykule opisano sposób, w jaki Blazor zarządza nieobsługiwanymi wyjątkami i jak opracowywać aplikacje wykrywające i obsługujące błędy.</span><span class="sxs-lookup"><span data-stu-id="1eb6e-105">This article describes how Blazor manages unhandled exceptions and how to develop apps that detect and handle errors.</span></span>

## <a name="detailed-errors-during-development"></a><span data-ttu-id="1eb6e-106">Szczegóły błędów podczas opracowywania</span><span class="sxs-lookup"><span data-stu-id="1eb6e-106">Detailed errors during development</span></span>

<span data-ttu-id="1eb6e-107">Gdy aplikacja Blazor nie działa prawidłowo podczas opracowywania, otrzymywanie szczegółowych informacji o błędach z aplikacji pomaga w rozwiązywaniu problemów i rozwiązaniu problemu.</span><span class="sxs-lookup"><span data-stu-id="1eb6e-107">When a Blazor app isn't functioning properly during development, receiving detailed error information from the app assists in troubleshooting and fixing the issue.</span></span> <span data-ttu-id="1eb6e-108">Gdy wystąpi błąd, Blazor aplikacje wyświetlają złoty pasek u dołu ekranu:</span><span class="sxs-lookup"><span data-stu-id="1eb6e-108">When an error occurs, Blazor apps display a gold bar at the bottom of the screen:</span></span>

* <span data-ttu-id="1eb6e-109">W trakcie programowania złoty pasek kieruje użytkownika do konsoli przeglądarki, gdzie można zobaczyć wyjątek.</span><span class="sxs-lookup"><span data-stu-id="1eb6e-109">During development, the gold bar directs you to the browser console, where you can see the exception.</span></span>
* <span data-ttu-id="1eb6e-110">W środowisku produkcyjnym złoty pasek powiadamia użytkownika o wystąpieniu błędu i zaleca odświeżenie przeglądarki.</span><span class="sxs-lookup"><span data-stu-id="1eb6e-110">In production, the gold bar notifies the user that an error has occurred and recommends refreshing the browser.</span></span>

<span data-ttu-id="1eb6e-111">Interfejs użytkownika dla tego środowiska obsługi błędów jest częścią szablonów projektu Blazor.</span><span class="sxs-lookup"><span data-stu-id="1eb6e-111">The UI for this error handling experience is part of the Blazor project templates.</span></span>

<span data-ttu-id="1eb6e-112">W aplikacji Blazor webassembly Dostosuj środowisko w pliku *wwwroot/index.html* :</span><span class="sxs-lookup"><span data-stu-id="1eb6e-112">In a Blazor WebAssembly app, customize the experience in the *wwwroot/index.html* file:</span></span>

```html
<div id="blazor-error-ui">
    An unhandled error has occurred.
    <a href="" class="reload">Reload</a>
    <a class="dismiss">🗙</a>
</div>
```

<span data-ttu-id="1eb6e-113">W aplikacji serwera Blazor Dostosuj środowisko w pliku *Pages/_Host. cshtml* :</span><span class="sxs-lookup"><span data-stu-id="1eb6e-113">In a Blazor Server app, customize the experience in the *Pages/_Host.cshtml* file:</span></span>

```cshtml
<div id="blazor-error-ui">
    <environment include="Staging,Production">
        An error has occurred. This application may no longer respond until reloaded.
    </environment>
    <environment include="Development">
        An unhandled exception has occurred. See browser dev tools for details.
    </environment>
    <a href="" class="reload">Reload</a>
    <a class="dismiss">🗙</a>
</div>
```

<span data-ttu-id="1eb6e-114">Element `blazor-error-ui` jest ukryty przez style dołączone do Blazor szablonów, a następnie pokazywany w przypadku wystąpienia błędu.</span><span class="sxs-lookup"><span data-stu-id="1eb6e-114">The `blazor-error-ui` element is hidden by the styles included with the Blazor templates and then shown when an error occurs.</span></span>

## <a name="how-a-opno-locblazor-server-app-reacts-to-unhandled-exceptions"></a><span data-ttu-id="1eb6e-115">Jak aplikacja serwera Blazor reaguje na Nieobsłużone wyjątki</span><span class="sxs-lookup"><span data-stu-id="1eb6e-115">How a Blazor Server app reacts to unhandled exceptions</span></span>

<span data-ttu-id="1eb6e-116">Serwer Blazor jest strukturą stanową.</span><span class="sxs-lookup"><span data-stu-id="1eb6e-116">Blazor Server is a stateful framework.</span></span> <span data-ttu-id="1eb6e-117">Gdy użytkownicy współpracują z aplikacją, utrzymują połączenie z serwerem znanym jako *obwód*.</span><span class="sxs-lookup"><span data-stu-id="1eb6e-117">While users interact with an app, they maintain a connection to the server known as a *circuit*.</span></span> <span data-ttu-id="1eb6e-118">Obwód zawiera aktywne wystąpienia składnika, a także wiele innych aspektów stanu, takich jak:</span><span class="sxs-lookup"><span data-stu-id="1eb6e-118">The circuit holds active component instances, plus many other aspects of state, such as:</span></span>

* <span data-ttu-id="1eb6e-119">Najnowsze renderowane dane wyjściowe składników.</span><span class="sxs-lookup"><span data-stu-id="1eb6e-119">The most recent rendered output of components.</span></span>
* <span data-ttu-id="1eb6e-120">Bieżący zestaw delegatów obsługi zdarzeń, które mogą być wyzwalane przez zdarzenia po stronie klienta.</span><span class="sxs-lookup"><span data-stu-id="1eb6e-120">The current set of event-handling delegates that could be triggered by client-side events.</span></span>

<span data-ttu-id="1eb6e-121">Jeśli użytkownik otworzy aplikację na wielu kartach przeglądarki, mają one wiele niezależnych obwodów.</span><span class="sxs-lookup"><span data-stu-id="1eb6e-121">If a user opens the app in multiple browser tabs, they have multiple independent circuits.</span></span>

Blazor<span data-ttu-id="1eb6e-122"> traktuje większość nieobsłużonych wyjątków jako krytyczne dla obwodu, w którym występują.</span><span class="sxs-lookup"><span data-stu-id="1eb6e-122"> treats most unhandled exceptions as fatal to the circuit where they occur.</span></span> <span data-ttu-id="1eb6e-123">Jeśli obwód zostanie przerwany z powodu nieobsługiwanego wyjątku, użytkownik może nadal korzystać z aplikacji tylko przez ponowne załadowanie strony w celu utworzenia nowego obwodu.</span><span class="sxs-lookup"><span data-stu-id="1eb6e-123">If a circuit is terminated due to an unhandled exception, the user can only continue to interact with the app by reloading the page to create a new circuit.</span></span> <span data-ttu-id="1eb6e-124">Nie ma to wpływu na obwody, które zostały przerwane, które są obwody dla innych użytkowników lub kart przeglądarki.</span><span class="sxs-lookup"><span data-stu-id="1eb6e-124">Circuits outside of the one that's terminated, which are circuits for other users or other browser tabs, aren't affected.</span></span> <span data-ttu-id="1eb6e-125">Ten scenariusz jest podobny do aplikacji klasycznej, która uległa awarii,&mdash;należy ponownie uruchomić aplikację, ale nie ma to wpływu na inne aplikacje.</span><span class="sxs-lookup"><span data-stu-id="1eb6e-125">This scenario is similar to a desktop app that crashes&mdash;the crashed app must be restarted, but other apps aren't affected.</span></span>

<span data-ttu-id="1eb6e-126">Obwód jest zakończony, gdy wystąpił nieobsługiwany wyjątek z następujących powodów:</span><span class="sxs-lookup"><span data-stu-id="1eb6e-126">A circuit is terminated when an unhandled exception occurs for the following reasons:</span></span>

* <span data-ttu-id="1eb6e-127">Nieobsługiwany wyjątek często pozostawia obwód w niezdefiniowanym stanie.</span><span class="sxs-lookup"><span data-stu-id="1eb6e-127">An unhandled exception often leaves the circuit in an undefined state.</span></span>
* <span data-ttu-id="1eb6e-128">Nie można zagwarantować normalnej operacji aplikacji po wystąpieniu nieobsłużonego wyjątku.</span><span class="sxs-lookup"><span data-stu-id="1eb6e-128">The app's normal operation can't be guaranteed after an unhandled exception.</span></span>
* <span data-ttu-id="1eb6e-129">W przypadku kontynuowania obwodu w aplikacji mogą pojawić się luki w zabezpieczeniach.</span><span class="sxs-lookup"><span data-stu-id="1eb6e-129">Security vulnerabilities may appear in the app if the circuit continues.</span></span>

## <a name="manage-unhandled-exceptions-in-developer-code"></a><span data-ttu-id="1eb6e-130">Zarządzanie nieobsługiwanymi wyjątkami w kodzie dewelopera</span><span class="sxs-lookup"><span data-stu-id="1eb6e-130">Manage unhandled exceptions in developer code</span></span>

<span data-ttu-id="1eb6e-131">Aby aplikacja kontynuowała działanie po wystąpieniu błędu, aplikacja musi mieć logikę obsługi błędów.</span><span class="sxs-lookup"><span data-stu-id="1eb6e-131">For an app to continue after an error, the app must have error handling logic.</span></span> <span data-ttu-id="1eb6e-132">W dalszej części tego artykułu opisano potencjalne źródła nieobsłużonych wyjątków.</span><span class="sxs-lookup"><span data-stu-id="1eb6e-132">Later sections of this article describe potential sources of unhandled exceptions.</span></span>

<span data-ttu-id="1eb6e-133">W środowisku produkcyjnym nie Renderuj komunikatów wyjątków struktury ani śladów stosu w interfejsie użytkownika.</span><span class="sxs-lookup"><span data-stu-id="1eb6e-133">In production, don't render framework exception messages or stack traces in the UI.</span></span> <span data-ttu-id="1eb6e-134">Renderowanie komunikatów o wyjątkach lub śladów stosu może:</span><span class="sxs-lookup"><span data-stu-id="1eb6e-134">Rendering exception messages or stack traces could:</span></span>

* <span data-ttu-id="1eb6e-135">Ujawnianie poufnych informacji użytkownikom końcowym.</span><span class="sxs-lookup"><span data-stu-id="1eb6e-135">Disclose sensitive information to end users.</span></span>
* <span data-ttu-id="1eb6e-136">Pomóż złośliwemu użytkownikowi wykrywać słabe strony w aplikacji, która może naruszyć bezpieczeństwo aplikacji, serwera lub sieci.</span><span class="sxs-lookup"><span data-stu-id="1eb6e-136">Help a malicious user discover weaknesses in an app that can compromise the security of the app, server, or network.</span></span>

## <a name="log-errors-with-a-persistent-provider"></a><span data-ttu-id="1eb6e-137">Rejestrowanie błędów przez dostawcę trwałego</span><span class="sxs-lookup"><span data-stu-id="1eb6e-137">Log errors with a persistent provider</span></span>

<span data-ttu-id="1eb6e-138">Jeśli wystąpi nieobsługiwany wyjątek, wyjątek jest rejestrowany w celu <xref:Microsoft.Extensions.Logging.ILogger> wystąpień skonfigurowanych w kontenerze usługi.</span><span class="sxs-lookup"><span data-stu-id="1eb6e-138">If an unhandled exception occurs, the exception is logged to <xref:Microsoft.Extensions.Logging.ILogger> instances configured in the service container.</span></span> <span data-ttu-id="1eb6e-139">Domyślnie aplikacje Blazor rejestrować w danych wyjściowych konsoli za pomocą dostawcy rejestrowania konsoli.</span><span class="sxs-lookup"><span data-stu-id="1eb6e-139">By default, Blazor apps log to console output with the Console Logging Provider.</span></span> <span data-ttu-id="1eb6e-140">Należy rozważyć logowanie do bardziej trwałej lokalizacji z dostawcą, który zarządza rozmiarem dziennika i rotacją dzienników.</span><span class="sxs-lookup"><span data-stu-id="1eb6e-140">Consider logging to a more permanent location with a provider that manages log size and log rotation.</span></span> <span data-ttu-id="1eb6e-141">Aby uzyskać więcej informacji, zobacz temat <xref:fundamentals/logging/index>.</span><span class="sxs-lookup"><span data-stu-id="1eb6e-141">For more information, see <xref:fundamentals/logging/index>.</span></span>

<span data-ttu-id="1eb6e-142">Podczas opracowywania, Blazor zwykle wysyła pełne szczegóły wyjątków do konsoli przeglądarki w celu ułatwienia debugowania.</span><span class="sxs-lookup"><span data-stu-id="1eb6e-142">During development, Blazor usually sends the full details of exceptions to the browser's console to aid in debugging.</span></span> <span data-ttu-id="1eb6e-143">W środowisku produkcyjnym szczegółowe błędy w konsoli przeglądarki są domyślnie wyłączone, co oznacza, że błędy nie są wysyłane do klientów, ale wszystkie szczegóły wyjątku nadal są rejestrowane po stronie serwera.</span><span class="sxs-lookup"><span data-stu-id="1eb6e-143">In production, detailed errors in the browser's console are disabled by default, which means that errors aren't sent to clients but the exception's full details are still logged server-side.</span></span> <span data-ttu-id="1eb6e-144">Aby uzyskać więcej informacji, zobacz temat <xref:fundamentals/error-handling>.</span><span class="sxs-lookup"><span data-stu-id="1eb6e-144">For more information, see <xref:fundamentals/error-handling>.</span></span>

<span data-ttu-id="1eb6e-145">Należy zdecydować, które zdarzenia mają być rejestrowane, oraz poziom ważności zarejestrowanych zdarzeń.</span><span class="sxs-lookup"><span data-stu-id="1eb6e-145">You must decide which incidents to log and the level of severity of logged incidents.</span></span> <span data-ttu-id="1eb6e-146">Nieszkodliwi użytkownicy mogą być w stanie wyzwolić błędy w sposób celowy.</span><span class="sxs-lookup"><span data-stu-id="1eb6e-146">Hostile users might be able to trigger errors deliberately.</span></span> <span data-ttu-id="1eb6e-147">Na przykład nie należy rejestrować zdarzenia z błędu, gdzie w adresie URL składnika, który zawiera szczegóły produktu, jest podano nieznanego `ProductId`.</span><span class="sxs-lookup"><span data-stu-id="1eb6e-147">For example, don't log an incident from an error where an unknown `ProductId` is supplied in the URL of a component that displays product details.</span></span> <span data-ttu-id="1eb6e-148">Nie wszystkie błędy powinny być traktowane jako zdarzenia o wysokiej ważności do rejestrowania.</span><span class="sxs-lookup"><span data-stu-id="1eb6e-148">Not all errors should be treated as high-severity incidents for logging.</span></span>

## <a name="places-where-errors-may-occur"></a><span data-ttu-id="1eb6e-149">Miejsca, w których mogą wystąpić błędy</span><span class="sxs-lookup"><span data-stu-id="1eb6e-149">Places where errors may occur</span></span>

<span data-ttu-id="1eb6e-150">Kod struktury i aplikacji może wyzwolić Nieobsłużone wyjątki w jednej z następujących lokalizacji:</span><span class="sxs-lookup"><span data-stu-id="1eb6e-150">Framework and app code may trigger unhandled exceptions in any of the following locations:</span></span>

* [<span data-ttu-id="1eb6e-151">Tworzenie wystąpienia składnika</span><span class="sxs-lookup"><span data-stu-id="1eb6e-151">Component instantiation</span></span>](#component-instantiation)
* [<span data-ttu-id="1eb6e-152">Metody cyklu życia</span><span class="sxs-lookup"><span data-stu-id="1eb6e-152">Lifecycle methods</span></span>](#lifecycle-methods)
* [<span data-ttu-id="1eb6e-153">Logika renderowania</span><span class="sxs-lookup"><span data-stu-id="1eb6e-153">Rendering logic</span></span>](#rendering-logic)
* [<span data-ttu-id="1eb6e-154">Programy obsługi zdarzeń</span><span class="sxs-lookup"><span data-stu-id="1eb6e-154">Event handlers</span></span>](#event-handlers)
* [<span data-ttu-id="1eb6e-155">Usuwanie składników</span><span class="sxs-lookup"><span data-stu-id="1eb6e-155">Component disposal</span></span>](#component-disposal)
* [<span data-ttu-id="1eb6e-156">Międzyoperacyjność JavaScript</span><span class="sxs-lookup"><span data-stu-id="1eb6e-156">JavaScript interop</span></span>](#javascript-interop)
* <span data-ttu-id="1eb6e-157">[procedury obsługi obwodu serwera Blazor](#blazor-server-circuit-handlers)</span><span class="sxs-lookup"><span data-stu-id="1eb6e-157">[Blazor Server circuit handlers](#blazor-server-circuit-handlers)</span></span>
* <span data-ttu-id="1eb6e-158">[Usuwanie obwodu serwera Blazor](#blazor-server-circuit-disposal)</span><span class="sxs-lookup"><span data-stu-id="1eb6e-158">[Blazor Server circuit disposal](#blazor-server-circuit-disposal)</span></span>
* <span data-ttu-id="1eb6e-159">[Renderowanie serwera Blazor](#blazor-server-prerendering)</span><span class="sxs-lookup"><span data-stu-id="1eb6e-159">[Blazor Server rerendering](#blazor-server-prerendering)</span></span>

<span data-ttu-id="1eb6e-160">Poprzednie Nieobsłużone wyjątki zostały opisane w poniższych sekcjach tego artykułu.</span><span class="sxs-lookup"><span data-stu-id="1eb6e-160">The preceding unhandled exceptions are described in the following sections of this article.</span></span>

### <a name="component-instantiation"></a><span data-ttu-id="1eb6e-161">Tworzenie wystąpienia składnika</span><span class="sxs-lookup"><span data-stu-id="1eb6e-161">Component instantiation</span></span>

<span data-ttu-id="1eb6e-162">Gdy Blazor tworzy wystąpienie składnika:</span><span class="sxs-lookup"><span data-stu-id="1eb6e-162">When Blazor creates an instance of a component:</span></span>

* <span data-ttu-id="1eb6e-163">Konstruktor składnika jest wywoływany.</span><span class="sxs-lookup"><span data-stu-id="1eb6e-163">The component's constructor is invoked.</span></span>
* <span data-ttu-id="1eb6e-164">Konstruktory wszelkich niepojedynczych usług DI dostarczonych do konstruktora składnika za pośrednictwem dyrektywy [`@inject`](xref:blazor/dependency-injection#request-a-service-in-a-component) lub atrybutu [`[Inject]`](xref:blazor/dependency-injection#request-a-service-in-a-component) są wywoływane.</span><span class="sxs-lookup"><span data-stu-id="1eb6e-164">The constructors of any non-singleton DI services supplied to the component's constructor via the [`@inject`](xref:blazor/dependency-injection#request-a-service-in-a-component) directive or the [`[Inject]`](xref:blazor/dependency-injection#request-a-service-in-a-component) attribute are invoked.</span></span>

<span data-ttu-id="1eb6e-165">Obwód serwera Blazor kończy się niepowodzeniem, gdy dowolny wykonany Konstruktor lub Metoda ustawiająca dla żadnej właściwości `[Inject]` zgłasza nieobsłużony wyjątek.</span><span class="sxs-lookup"><span data-stu-id="1eb6e-165">A Blazor Server circuit fails when any executed constructor or a setter for any `[Inject]` property throws an unhandled exception.</span></span> <span data-ttu-id="1eb6e-166">Wyjątek jest krytyczny, ponieważ struktura nie może utworzyć wystąpienia składnika.</span><span class="sxs-lookup"><span data-stu-id="1eb6e-166">The exception is fatal because the framework can't instantiate the component.</span></span> <span data-ttu-id="1eb6e-167">Jeśli logika konstruktora może generować wyjątki, aplikacja powinna zalewkować wyjątki przy użyciu instrukcji [try-catch](/dotnet/csharp/language-reference/keywords/try-catch) z obsługą błędów i rejestrowaniem.</span><span class="sxs-lookup"><span data-stu-id="1eb6e-167">If constructor logic may throw exceptions, the app should trap the exceptions using a [try-catch](/dotnet/csharp/language-reference/keywords/try-catch) statement with error handling and logging.</span></span>

### <a name="lifecycle-methods"></a><span data-ttu-id="1eb6e-168">Metody cyklu życia</span><span class="sxs-lookup"><span data-stu-id="1eb6e-168">Lifecycle methods</span></span>

<span data-ttu-id="1eb6e-169">W trakcie okresu istnienia składnika Blazor wywołuje następujące [metody cyklu życia](xref:blazor/lifecycle):</span><span class="sxs-lookup"><span data-stu-id="1eb6e-169">During the lifetime of a component, Blazor invokes the following [lifecycle methods](xref:blazor/lifecycle):</span></span>

* `OnInitialized` / `OnInitializedAsync`
* `OnParametersSet` / `OnParametersSetAsync`
* `ShouldRender` / `ShouldRenderAsync`
* `OnAfterRender` / `OnAfterRenderAsync`

<span data-ttu-id="1eb6e-170">Jeśli jakakolwiek metoda cyklu życia zgłasza wyjątek, synchronicznie lub asynchronicznie, wyjątek jest krytyczny dla obwodu serwera Blazor.</span><span class="sxs-lookup"><span data-stu-id="1eb6e-170">If any lifecycle method throws an exception, synchronously or asynchronously, the exception is fatal to a Blazor Server circuit.</span></span> <span data-ttu-id="1eb6e-171">Aby składniki zajmowały błędy w metodach cyklu życia, Dodaj logikę obsługi błędów.</span><span class="sxs-lookup"><span data-stu-id="1eb6e-171">For components to deal with errors in lifecycle methods, add error handling logic.</span></span>

<span data-ttu-id="1eb6e-172">W poniższym przykładzie, gdzie `OnParametersSetAsync` wywołuje metodę w celu uzyskania produktu:</span><span class="sxs-lookup"><span data-stu-id="1eb6e-172">In the following example where `OnParametersSetAsync` calls a method to obtain a product:</span></span>

* <span data-ttu-id="1eb6e-173">Wyjątek zgłoszony w metodzie `ProductRepository.GetProductByIdAsync` jest obsługiwany przez instrukcję `try-catch`.</span><span class="sxs-lookup"><span data-stu-id="1eb6e-173">An exception thrown in the `ProductRepository.GetProductByIdAsync` method is handled by a `try-catch` statement.</span></span>
* <span data-ttu-id="1eb6e-174">Gdy jest wykonywany blok `catch`:</span><span class="sxs-lookup"><span data-stu-id="1eb6e-174">When the `catch` block is executed:</span></span>
  * <span data-ttu-id="1eb6e-175">`loadFailed` jest ustawiony na `true`, który jest używany do wyświetlania komunikatu o błędzie dla użytkownika.</span><span class="sxs-lookup"><span data-stu-id="1eb6e-175">`loadFailed` is set to `true`, which is used to display an error message to the user.</span></span>
  * <span data-ttu-id="1eb6e-176">Błąd jest rejestrowany.</span><span class="sxs-lookup"><span data-stu-id="1eb6e-176">The error is logged.</span></span>

[!code-razor[](handle-errors/samples_snapshot/3.x/product-details.razor?highlight=11,27-39)]

### <a name="rendering-logic"></a><span data-ttu-id="1eb6e-177">Logika renderowania</span><span class="sxs-lookup"><span data-stu-id="1eb6e-177">Rendering logic</span></span>

<span data-ttu-id="1eb6e-178">Znaczniki deklaratywne w pliku składnika `.razor` są kompilowane do C# metody o nazwie `BuildRenderTree`.</span><span class="sxs-lookup"><span data-stu-id="1eb6e-178">The declarative markup in a `.razor` component file is compiled into a C# method called `BuildRenderTree`.</span></span> <span data-ttu-id="1eb6e-179">Gdy składnik jest renderowany, `BuildRenderTree` wykonuje i tworzy strukturę danych opisującą elementy, tekst i składniki podrzędne renderowanego składnika.</span><span class="sxs-lookup"><span data-stu-id="1eb6e-179">When a component renders, `BuildRenderTree` executes and builds up a data structure describing the elements, text, and child components of the rendered component.</span></span>

<span data-ttu-id="1eb6e-180">Logika renderowania może zgłosić wyjątek.</span><span class="sxs-lookup"><span data-stu-id="1eb6e-180">Rendering logic can throw an exception.</span></span> <span data-ttu-id="1eb6e-181">Przykład tego scenariusza występuje, gdy `@someObject.PropertyName` jest oceniane, ale `@someObject` `null`.</span><span class="sxs-lookup"><span data-stu-id="1eb6e-181">An example of this scenario occurs when `@someObject.PropertyName` is evaluated but `@someObject` is `null`.</span></span> <span data-ttu-id="1eb6e-182">Nieobsługiwany wyjątek zgłoszony przez logikę renderowania jest krytyczny dla obwodu serwera Blazor.</span><span class="sxs-lookup"><span data-stu-id="1eb6e-182">An unhandled exception thrown by rendering logic is fatal to a Blazor Server circuit.</span></span>

<span data-ttu-id="1eb6e-183">Aby zapobiec wystąpieniu wyjątku odwołania o wartości null w logice renderowania, przed uzyskaniem dostępu do elementów członkowskich Sprawdź, czy `null` obiektu.</span><span class="sxs-lookup"><span data-stu-id="1eb6e-183">To prevent a null reference exception in rendering logic, check for a `null` object before accessing its members.</span></span> <span data-ttu-id="1eb6e-184">W poniższym przykładzie właściwości `person.Address` nie są dostępne w przypadku `null``person.Address`:</span><span class="sxs-lookup"><span data-stu-id="1eb6e-184">In the following example, `person.Address` properties aren't accessed if `person.Address` is `null`:</span></span>

[!code-razor[](handle-errors/samples_snapshot/3.x/person-example.razor?highlight=1)]

<span data-ttu-id="1eb6e-185">Poprzedni kod założono, że `person` nie `null`.</span><span class="sxs-lookup"><span data-stu-id="1eb6e-185">The preceding code assumes that `person` isn't `null`.</span></span> <span data-ttu-id="1eb6e-186">Często Struktura kodu gwarantuje, że obiekt istnieje w momencie renderowania składnika.</span><span class="sxs-lookup"><span data-stu-id="1eb6e-186">Often, the structure of the code guarantees that an object exists at the time the component is rendered.</span></span> <span data-ttu-id="1eb6e-187">W takich przypadkach nie trzeba sprawdzać `null` w logice renderowania.</span><span class="sxs-lookup"><span data-stu-id="1eb6e-187">In those cases, it isn't necessary to check for `null` in rendering logic.</span></span> <span data-ttu-id="1eb6e-188">W poprzednim przykładzie można zagwarantować, że istnieje `person`, ponieważ `person` jest tworzony podczas tworzenia wystąpienia składnika.</span><span class="sxs-lookup"><span data-stu-id="1eb6e-188">In the prior example, `person` might be guaranteed to exist because `person` is created when the component is instantiated.</span></span>

### <a name="event-handlers"></a><span data-ttu-id="1eb6e-189">Programy obsługi zdarzeń</span><span class="sxs-lookup"><span data-stu-id="1eb6e-189">Event handlers</span></span>

<span data-ttu-id="1eb6e-190">Kod po stronie klienta wyzwala wywołania kodu, C# gdy programy obsługi zdarzeń są tworzone przy użyciu:</span><span class="sxs-lookup"><span data-stu-id="1eb6e-190">Client-side code triggers invocations of C# code when event handlers are created using:</span></span>

* `@onclick`
* `@onchange`
* <span data-ttu-id="1eb6e-191">Inne atrybuty `@on...`</span><span class="sxs-lookup"><span data-stu-id="1eb6e-191">Other `@on...` attributes</span></span>
* `@bind`

<span data-ttu-id="1eb6e-192">Kod procedury obsługi zdarzeń może zgłosić nieobsługiwany wyjątek w tych scenariuszach.</span><span class="sxs-lookup"><span data-stu-id="1eb6e-192">Event handler code might throw an unhandled exception in these scenarios.</span></span>

<span data-ttu-id="1eb6e-193">Jeśli procedura obsługi zdarzeń zgłasza nieobsługiwany wyjątek (na przykład kwerenda bazy danych kończy się niepowodzeniem), wyjątek jest krytyczny dla obwodu serwera Blazor.</span><span class="sxs-lookup"><span data-stu-id="1eb6e-193">If an event handler throws an unhandled exception (for example, a database query fails), the exception is fatal to a Blazor Server circuit.</span></span> <span data-ttu-id="1eb6e-194">Jeśli aplikacja wywołuje kod, który może zakończyć się niepowodzeniem z powodów zewnętrznych, należy zastosować wyjątek pułapki przy użyciu instrukcji [try-catch](/dotnet/csharp/language-reference/keywords/try-catch) z obsługą błędów i rejestrowaniem.</span><span class="sxs-lookup"><span data-stu-id="1eb6e-194">If the app calls code that could fail for external reasons, trap exceptions using a [try-catch](/dotnet/csharp/language-reference/keywords/try-catch) statement with error handling and logging.</span></span>

<span data-ttu-id="1eb6e-195">Jeśli kod użytkownika nie jest pułapk i nie obsługuje wyjątku, struktura rejestruje wyjątek i kończy obwód.</span><span class="sxs-lookup"><span data-stu-id="1eb6e-195">If user code doesn't trap and handle the exception, the framework logs the exception and terminates the circuit.</span></span>

### <a name="component-disposal"></a><span data-ttu-id="1eb6e-196">Usuwanie składników</span><span class="sxs-lookup"><span data-stu-id="1eb6e-196">Component disposal</span></span>

<span data-ttu-id="1eb6e-197">Składnik może zostać usunięty z interfejsu użytkownika, na przykład, ponieważ użytkownik przeszedł do innej strony.</span><span class="sxs-lookup"><span data-stu-id="1eb6e-197">A component may be removed from the UI, for example, because the user has navigated to another page.</span></span> <span data-ttu-id="1eb6e-198">Gdy składnik implementujący <xref:System.IDisposable?displayProperty=fullName> jest usuwany z interfejsu użytkownika, struktura wywołuje metodę <xref:System.IDisposable.Dispose*> składnika.</span><span class="sxs-lookup"><span data-stu-id="1eb6e-198">When a component that implements <xref:System.IDisposable?displayProperty=fullName> is removed from the UI, the framework calls the component's <xref:System.IDisposable.Dispose*> method.</span></span>

<span data-ttu-id="1eb6e-199">Jeśli metoda `Dispose` składnika zgłasza nieobsługiwany wyjątek, wyjątek jest krytyczny dla obwodu serwera Blazor.</span><span class="sxs-lookup"><span data-stu-id="1eb6e-199">If the component's `Dispose` method throws an unhandled exception, the exception is fatal to a Blazor Server circuit.</span></span> <span data-ttu-id="1eb6e-200">Jeśli logika usuwania może generować wyjątki, aplikacja powinna zalewkować wyjątki przy użyciu instrukcji [try-catch](/dotnet/csharp/language-reference/keywords/try-catch) z obsługą błędów i rejestrowaniem.</span><span class="sxs-lookup"><span data-stu-id="1eb6e-200">If disposal logic may throw exceptions, the app should trap the exceptions using a [try-catch](/dotnet/csharp/language-reference/keywords/try-catch) statement with error handling and logging.</span></span>

<span data-ttu-id="1eb6e-201">Aby uzyskać więcej informacji na temat usuwania składników, zobacz <xref:blazor/lifecycle#component-disposal-with-idisposable>.</span><span class="sxs-lookup"><span data-stu-id="1eb6e-201">For more information on component disposal, see <xref:blazor/lifecycle#component-disposal-with-idisposable>.</span></span>

### <a name="javascript-interop"></a><span data-ttu-id="1eb6e-202">Międzyoperacyjność w języku JavaScript</span><span class="sxs-lookup"><span data-stu-id="1eb6e-202">JavaScript interop</span></span>

<span data-ttu-id="1eb6e-203">`IJSRuntime.InvokeAsync<T>` umożliwia kodowi .NET wykonywanie wywołań asynchronicznych do środowiska uruchomieniowego JavaScript w przeglądarce użytkownika.</span><span class="sxs-lookup"><span data-stu-id="1eb6e-203">`IJSRuntime.InvokeAsync<T>` allows .NET code to make asynchronous calls to the JavaScript runtime in the user's browser.</span></span>

<span data-ttu-id="1eb6e-204">Poniższe warunki dotyczą obsługi błędów w `InvokeAsync<T>`:</span><span class="sxs-lookup"><span data-stu-id="1eb6e-204">The following conditions apply to error handling with `InvokeAsync<T>`:</span></span>

* <span data-ttu-id="1eb6e-205">Jeśli wywołanie `InvokeAsync<T>` nie powiedzie się synchronicznie, wystąpi wyjątek programu .NET.</span><span class="sxs-lookup"><span data-stu-id="1eb6e-205">If a call to `InvokeAsync<T>` fails synchronously, a .NET exception occurs.</span></span> <span data-ttu-id="1eb6e-206">Wywołanie `InvokeAsync<T>` może zakończyć się niepowodzeniem, na przykład dlatego, że nie można serializować dostarczonych argumentów.</span><span class="sxs-lookup"><span data-stu-id="1eb6e-206">A call to `InvokeAsync<T>` may fail, for example, because the supplied arguments can't be serialized.</span></span> <span data-ttu-id="1eb6e-207">Kod dewelopera musi przechwycić wyjątek.</span><span class="sxs-lookup"><span data-stu-id="1eb6e-207">Developer code must catch the exception.</span></span> <span data-ttu-id="1eb6e-208">Jeśli kod aplikacji w obsłudze zdarzeń lub metodzie cyklu życia składnika nie obsługuje wyjątku, wynikający z nich wyjątek jest krytyczny dla obwodu serwera Blazor.</span><span class="sxs-lookup"><span data-stu-id="1eb6e-208">If app code in an event handler or component lifecycle method doesn't handle an exception, the resulting exception is fatal to a Blazor Server circuit.</span></span>
* <span data-ttu-id="1eb6e-209">Jeśli wywołanie `InvokeAsync<T>` nie powiedzie się asynchronicznie, <xref:System.Threading.Tasks.Task> .NET kończy się niepowodzeniem.</span><span class="sxs-lookup"><span data-stu-id="1eb6e-209">If a call to `InvokeAsync<T>` fails asynchronously, the .NET <xref:System.Threading.Tasks.Task> fails.</span></span> <span data-ttu-id="1eb6e-210">Wywołanie `InvokeAsync<T>` może zakończyć się niepowodzeniem, na przykład ponieważ kod po stronie JavaScript zgłasza wyjątek lub zwraca `Promise`, który został ukończony jako `rejected`.</span><span class="sxs-lookup"><span data-stu-id="1eb6e-210">A call to `InvokeAsync<T>` may fail, for example, because the JavaScript-side code throws an exception or returns a `Promise` that completed as `rejected`.</span></span> <span data-ttu-id="1eb6e-211">Kod dewelopera musi przechwycić wyjątek.</span><span class="sxs-lookup"><span data-stu-id="1eb6e-211">Developer code must catch the exception.</span></span> <span data-ttu-id="1eb6e-212">W przypadku użycia operatora [await](/dotnet/csharp/language-reference/keywords/await) Rozważ zapakowanie wywołania metody w instrukcji [try-catch](/dotnet/csharp/language-reference/keywords/try-catch) z obsługą błędów i rejestrowaniem.</span><span class="sxs-lookup"><span data-stu-id="1eb6e-212">If using the [await](/dotnet/csharp/language-reference/keywords/await) operator, consider wrapping the method call in a [try-catch](/dotnet/csharp/language-reference/keywords/try-catch) statement with error handling and logging.</span></span> <span data-ttu-id="1eb6e-213">W przeciwnym razie niepowodzenie kodu spowoduje nieobsłużony wyjątek krytyczny dla obwodu serwera Blazor.</span><span class="sxs-lookup"><span data-stu-id="1eb6e-213">Otherwise, the failing code results in an unhandled exception that's fatal to a Blazor Server circuit.</span></span>
* <span data-ttu-id="1eb6e-214">Domyślnie wywołania do `InvokeAsync<T>` muszą zakończyć się w określonym czasie lub w przeciwnym razie upłynął limit czasu połączenia. Domyślny limit czasu wynosi jedną minutę.</span><span class="sxs-lookup"><span data-stu-id="1eb6e-214">By default, calls to `InvokeAsync<T>` must complete within a certain period or else the call times out. The default timeout period is one minute.</span></span> <span data-ttu-id="1eb6e-215">Limit czasu chroni kod przed utratą połączenia sieciowego lub kodem JavaScript, który nigdy nie odsyła komunikat uzupełniający.</span><span class="sxs-lookup"><span data-stu-id="1eb6e-215">The timeout protects the code against a loss in network connectivity or JavaScript code that never sends back a completion message.</span></span> <span data-ttu-id="1eb6e-216">Jeśli wystąpiło przełączenie, wynikiem `Task` zakończy się niepowodzeniem z <xref:System.OperationCanceledException>.</span><span class="sxs-lookup"><span data-stu-id="1eb6e-216">If the call times out, the resulting `Task` fails with an <xref:System.OperationCanceledException>.</span></span> <span data-ttu-id="1eb6e-217">Zalewka i przetwórz wyjątek z rejestrowaniem.</span><span class="sxs-lookup"><span data-stu-id="1eb6e-217">Trap and process the exception with logging.</span></span>

<span data-ttu-id="1eb6e-218">Podobnie kod JavaScript może inicjować wywołania metod .NET wskazywanych przez atrybut [`[JSInvokable]`](xref:blazor/javascript-interop#invoke-net-methods-from-javascript-functions) .</span><span class="sxs-lookup"><span data-stu-id="1eb6e-218">Similarly, JavaScript code may initiate calls to .NET methods indicated by the [`[JSInvokable]`](xref:blazor/javascript-interop#invoke-net-methods-from-javascript-functions) attribute.</span></span> <span data-ttu-id="1eb6e-219">Jeśli te metody .NET zgłaszają nieobsługiwany wyjątek:</span><span class="sxs-lookup"><span data-stu-id="1eb6e-219">If these .NET methods throw an unhandled exception:</span></span>

* <span data-ttu-id="1eb6e-220">Wyjątek nie jest traktowany jako krytyczny dla obwodu serwera Blazor.</span><span class="sxs-lookup"><span data-stu-id="1eb6e-220">The exception isn't treated as fatal to a Blazor Server circuit.</span></span>
* <span data-ttu-id="1eb6e-221">`Promise` po stronie JavaScript zostanie odrzucony.</span><span class="sxs-lookup"><span data-stu-id="1eb6e-221">The JavaScript-side `Promise` is rejected.</span></span>

<span data-ttu-id="1eb6e-222">Istnieje możliwość użycia kodu obsługi błędów po stronie .NET lub stronie JavaScript wywołania metody.</span><span class="sxs-lookup"><span data-stu-id="1eb6e-222">You have the option of using error handling code on either the .NET side or the JavaScript side of the method call.</span></span>

<span data-ttu-id="1eb6e-223">Aby uzyskać więcej informacji, zobacz temat <xref:blazor/javascript-interop>.</span><span class="sxs-lookup"><span data-stu-id="1eb6e-223">For more information, see <xref:blazor/javascript-interop>.</span></span>

### <a name="opno-locblazor-server-circuit-handlers"></a><span data-ttu-id="1eb6e-224">procedury obsługi obwodu serwera Blazor</span><span class="sxs-lookup"><span data-stu-id="1eb6e-224">Blazor Server circuit handlers</span></span>

<span data-ttu-id="1eb6e-225">Serwer Blazor umożliwia kod definiujący *procedurę obsługi obwodu*, która umożliwia uruchamianie kodu na zmiany stanu obwodu użytkownika.</span><span class="sxs-lookup"><span data-stu-id="1eb6e-225">Blazor Server allows code to define a *circuit handler*, which allows running code on changes to the state of a user's circuit.</span></span> <span data-ttu-id="1eb6e-226">Program obsługi obwodu jest implementowany przez wyprowadzanie z `CircuitHandler` i rejestrowanie klasy w kontenerze usługi aplikacji.</span><span class="sxs-lookup"><span data-stu-id="1eb6e-226">A circuit handler is implemented by deriving from `CircuitHandler` and registering the class in the app's service container.</span></span> <span data-ttu-id="1eb6e-227">Poniższy przykład obsługi obwodu śledzi otwarte SignalR połączenia:</span><span class="sxs-lookup"><span data-stu-id="1eb6e-227">The following example of a circuit handler tracks open SignalR connections:</span></span>

```csharp
using System.Collections.Generic;
using System.Threading;
using System.Threading.Tasks;
using Microsoft.AspNetCore.Components.Server.Circuits;

public class TrackingCircuitHandler : CircuitHandler
{
    private HashSet<Circuit> _circuits = new HashSet<Circuit>();

    public override Task OnConnectionUpAsync(Circuit circuit, 
        CancellationToken cancellationToken)
    {
        _circuits.Add(circuit);

        return Task.CompletedTask;
    }

    public override Task OnConnectionDownAsync(Circuit circuit, 
        CancellationToken cancellationToken)
    {
        _circuits.Remove(circuit);

        return Task.CompletedTask;
    }

    public int ConnectedCircuits => _circuits.Count;
}
```

<span data-ttu-id="1eb6e-228">Procedury obsługi obwodu są rejestrowane przy użyciu funkcji DI.</span><span class="sxs-lookup"><span data-stu-id="1eb6e-228">Circuit handlers are registered using DI.</span></span> <span data-ttu-id="1eb6e-229">Wystąpienia w zakresie są tworzone na wystąpienie obwodu.</span><span class="sxs-lookup"><span data-stu-id="1eb6e-229">Scoped instances are created per instance of a circuit.</span></span> <span data-ttu-id="1eb6e-230">Korzystając z `TrackingCircuitHandler` w poprzednim przykładzie, tworzona jest usługa singleton, ponieważ stan wszystkich obwodów musi być śledzony:</span><span class="sxs-lookup"><span data-stu-id="1eb6e-230">Using the `TrackingCircuitHandler` in the preceding example, a singleton service is created because the state of all circuits must be tracked:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    ...
    services.AddSingleton<CircuitHandler, TrackingCircuitHandler>();
}
```

<span data-ttu-id="1eb6e-231">Jeśli metody obsługi niestandardowego obwodu zgłaszają nieobsługiwany wyjątek, wyjątek jest krytyczny dla obwodu serwera Blazor.</span><span class="sxs-lookup"><span data-stu-id="1eb6e-231">If a custom circuit handler's methods throw an unhandled exception, the exception is fatal to the Blazor Server circuit.</span></span> <span data-ttu-id="1eb6e-232">Aby tolerować wyjątki w kodzie programu obsługi lub metodach wywoływanych, zawiń kod w co najmniej jednej instrukcji [try-catch](/dotnet/csharp/language-reference/keywords/try-catch) z obsługą błędów i rejestrowaniem.</span><span class="sxs-lookup"><span data-stu-id="1eb6e-232">To tolerate exceptions in a handler's code or called methods, wrap the code in one or more [try-catch](/dotnet/csharp/language-reference/keywords/try-catch) statements with error handling and logging.</span></span>

### <a name="opno-locblazor-server-circuit-disposal"></a><span data-ttu-id="1eb6e-233">Usuwanie obwodu serwera Blazor</span><span class="sxs-lookup"><span data-stu-id="1eb6e-233">Blazor Server circuit disposal</span></span>

<span data-ttu-id="1eb6e-234">Gdy obwód kończy się, ponieważ użytkownik odłączył się i struktura czyści stan obwodu, struktura usuwa zakres DI obwodu.</span><span class="sxs-lookup"><span data-stu-id="1eb6e-234">When a circuit ends because a user has disconnected and the framework is cleaning up the circuit state, the framework disposes of the circuit's DI scope.</span></span> <span data-ttu-id="1eb6e-235">Oddysponowanie zakresu polega na usunięciu wszelkich usług w zakresie innych firm, które implementują <xref:System.IDisposable?displayProperty=fullName>.</span><span class="sxs-lookup"><span data-stu-id="1eb6e-235">Disposing the scope disposes any circuit-scoped DI services that implement <xref:System.IDisposable?displayProperty=fullName>.</span></span> <span data-ttu-id="1eb6e-236">Jeśli jakakolwiek usługa nie zgłasza nieobsłużonego wyjątku podczas usuwania, struktura rejestruje wyjątek.</span><span class="sxs-lookup"><span data-stu-id="1eb6e-236">If any DI service throws an unhandled exception during disposal, the framework logs the exception.</span></span>

### <a name="opno-locblazor-server-prerendering"></a><span data-ttu-id="1eb6e-237">Renderowanie preBlazor Server</span><span class="sxs-lookup"><span data-stu-id="1eb6e-237">Blazor Server prerendering</span></span>

<span data-ttu-id="1eb6e-238">składniki Blazor mogą być wstępnie renderowane przy użyciu pomocnika tagów `Component`, tak że renderowane znaczniki HTML są zwracane jako część początkowego żądania HTTP użytkownika.</span><span class="sxs-lookup"><span data-stu-id="1eb6e-238">Blazor components can be prerendered using the `Component` Tag Helper so that their rendered HTML markup is returned as part of the user's initial HTTP request.</span></span> <span data-ttu-id="1eb6e-239">Działa to w następujący sposób:</span><span class="sxs-lookup"><span data-stu-id="1eb6e-239">This works by:</span></span>

* <span data-ttu-id="1eb6e-240">Tworzenie nowego obwodu dla wszystkich wstępnie renderowanych składników, które są częścią tej samej strony.</span><span class="sxs-lookup"><span data-stu-id="1eb6e-240">Creating a new circuit for all of the prerendered components that are part of the same page.</span></span>
* <span data-ttu-id="1eb6e-241">Generowanie początkowego kodu HTML.</span><span class="sxs-lookup"><span data-stu-id="1eb6e-241">Generating the initial HTML.</span></span>
* <span data-ttu-id="1eb6e-242">Podtraktowanie obwodu jako `disconnected`, dopóki przeglądarka użytkownika nie ustanowi połączenia SignalR z powrotem do tego samego serwera.</span><span class="sxs-lookup"><span data-stu-id="1eb6e-242">Treating the circuit as `disconnected` until the user's browser establishes a SignalR connection back to the same server.</span></span> <span data-ttu-id="1eb6e-243">Po nawiązaniu połączenia zostanie wznowione działanie międzydziałające w obwodzie i zostanie zaktualizowane oznaczenie HTML składników.</span><span class="sxs-lookup"><span data-stu-id="1eb6e-243">When the connection is established, interactivity on the circuit is resumed and the components' HTML markup is updated.</span></span>

<span data-ttu-id="1eb6e-244">Jeśli jakikolwiek składnik zgłasza nieobsłużony wyjątek podczas renderowania pre, na przykład podczas wykonywania metody cyklu życia lub logiki renderowania:</span><span class="sxs-lookup"><span data-stu-id="1eb6e-244">If any component throws an unhandled exception during prerendering, for example, during a lifecycle method or in rendering logic:</span></span>

* <span data-ttu-id="1eb6e-245">Wyjątek jest krytyczny dla obwodu.</span><span class="sxs-lookup"><span data-stu-id="1eb6e-245">The exception is fatal to the circuit.</span></span>
* <span data-ttu-id="1eb6e-246">Wyjątek jest generowany w stosie wywołań z pomocnika tagów `Component`.</span><span class="sxs-lookup"><span data-stu-id="1eb6e-246">The exception is thrown up the call stack from the `Component` Tag Helper.</span></span> <span data-ttu-id="1eb6e-247">W związku z tym całe żądanie HTTP kończy się niepowodzeniem, chyba że wyjątek jest jawnie przechwycony przez kod dewelopera.</span><span class="sxs-lookup"><span data-stu-id="1eb6e-247">Therefore, the entire HTTP request fails unless the exception is explicitly caught by developer code.</span></span>

<span data-ttu-id="1eb6e-248">W normalnych warunkach w przypadku niepowodzenia wstępnego renderowania kontynuowanie kompilowania i renderowania składnika nie ma sensu, ponieważ nie można renderować składnika roboczego.</span><span class="sxs-lookup"><span data-stu-id="1eb6e-248">Under normal circumstances when prerendering fails, continuing to build and render the component doesn't make sense because a working component can't be rendered.</span></span>

<span data-ttu-id="1eb6e-249">Aby tolerować błędy, które mogą wystąpić podczas renderowania prerenderingu, logika obsługi błędów musi być umieszczona wewnątrz składnika, który może zgłaszać wyjątki.</span><span class="sxs-lookup"><span data-stu-id="1eb6e-249">To tolerate errors that may occur during prerendering, error handling logic must be placed inside a component that may throw exceptions.</span></span> <span data-ttu-id="1eb6e-250">Używaj instrukcji [try-catch](/dotnet/csharp/language-reference/keywords/try-catch) z obsługą błędów i rejestrowaniem.</span><span class="sxs-lookup"><span data-stu-id="1eb6e-250">Use [try-catch](/dotnet/csharp/language-reference/keywords/try-catch) statements with error handling and logging.</span></span> <span data-ttu-id="1eb6e-251">Zamiast zawijania pomocnika tagów `Component` w instrukcji `try-catch`, umieść logikę obsługi błędów w składniku renderowanym przez pomocnika tagów `Component`.</span><span class="sxs-lookup"><span data-stu-id="1eb6e-251">Instead of wrapping the `Component` Tag Helper in a `try-catch` statement, place error handling logic in the component rendered by the `Component` Tag Helper.</span></span>

## <a name="advanced-scenarios"></a><span data-ttu-id="1eb6e-252">Scenariusze zaawansowane</span><span class="sxs-lookup"><span data-stu-id="1eb6e-252">Advanced scenarios</span></span>

### <a name="recursive-rendering"></a><span data-ttu-id="1eb6e-253">Renderowanie cykliczne</span><span class="sxs-lookup"><span data-stu-id="1eb6e-253">Recursive rendering</span></span>

<span data-ttu-id="1eb6e-254">Składniki można cyklicznie zagnieżdżać.</span><span class="sxs-lookup"><span data-stu-id="1eb6e-254">Components can be nested recursively.</span></span> <span data-ttu-id="1eb6e-255">Jest to przydatne do reprezentowania struktur danych rekursywnych.</span><span class="sxs-lookup"><span data-stu-id="1eb6e-255">This is useful for representing recursive data structures.</span></span> <span data-ttu-id="1eb6e-256">Na przykład składnik `TreeNode` może renderować więcej składników `TreeNode` dla każdego elementu podrzędnego węzła.</span><span class="sxs-lookup"><span data-stu-id="1eb6e-256">For example, a `TreeNode` component can render more `TreeNode` components for each of the node's children.</span></span>

<span data-ttu-id="1eb6e-257">W przypadku renderowania cyklicznego należy unikać tworzenia wzorców, które powodują nieskończoną rekursję:</span><span class="sxs-lookup"><span data-stu-id="1eb6e-257">When rendering recursively, avoid coding patterns that result in infinite recursion:</span></span>

* <span data-ttu-id="1eb6e-258">Nie Renderuj rekursywnie struktury danych zawierającej cykl.</span><span class="sxs-lookup"><span data-stu-id="1eb6e-258">Don't recursively render a data structure that contains a cycle.</span></span> <span data-ttu-id="1eb6e-259">Na przykład nie Renderuj węzła drzewa, którego elementy podrzędne zawierają sam siebie.</span><span class="sxs-lookup"><span data-stu-id="1eb6e-259">For example, don't render a tree node whose children includes itself.</span></span>
* <span data-ttu-id="1eb6e-260">Nie twórz łańcucha układów zawierających cykl.</span><span class="sxs-lookup"><span data-stu-id="1eb6e-260">Don't create a chain of layouts that contain a cycle.</span></span> <span data-ttu-id="1eb6e-261">Na przykład nie twórz układu, którego układ jest sam.</span><span class="sxs-lookup"><span data-stu-id="1eb6e-261">For example, don't create a layout whose layout is itself.</span></span>
* <span data-ttu-id="1eb6e-262">Nie Zezwalaj użytkownikowi końcowemu na naruszenie nieodmian rekursji (reguł) przy użyciu złośliwego wpisu danych lub wywołań międzyoperacyjnych języka JavaScript.</span><span class="sxs-lookup"><span data-stu-id="1eb6e-262">Don't allow an end user to violate recursion invariants (rules) through malicious data entry or JavaScript interop calls.</span></span>

<span data-ttu-id="1eb6e-263">Nieskończone pętle podczas renderowania:</span><span class="sxs-lookup"><span data-stu-id="1eb6e-263">Infinite loops during rendering:</span></span>

* <span data-ttu-id="1eb6e-264">Powoduje, że proces renderowania kontynuuje działanie zawsze.</span><span class="sxs-lookup"><span data-stu-id="1eb6e-264">Causes the rendering process to continue forever.</span></span>
* <span data-ttu-id="1eb6e-265">Jest równoznaczny z tworzeniem niezakończonej pętli.</span><span class="sxs-lookup"><span data-stu-id="1eb6e-265">Is equivalent to creating an unterminated loop.</span></span>

<span data-ttu-id="1eb6e-266">W tych scenariuszach uszkodzony obwód serwera Blazor nie powiedzie się, a wątek zwykle próbuje wykonać:</span><span class="sxs-lookup"><span data-stu-id="1eb6e-266">In these scenarios, an affected Blazor Server circuit fails, and the thread usually attempts to:</span></span>

* <span data-ttu-id="1eb6e-267">Zużywaj ilość czasu procesora CPU dozwoloną przez system operacyjny w nieskończoność.</span><span class="sxs-lookup"><span data-stu-id="1eb6e-267">Consume as much CPU time as permitted by the operating system, indefinitely.</span></span>
* <span data-ttu-id="1eb6e-268">Korzystaj z nieograniczonej ilości pamięci serwera.</span><span class="sxs-lookup"><span data-stu-id="1eb6e-268">Consume an unlimited amount of server memory.</span></span> <span data-ttu-id="1eb6e-269">Zużywanie nieograniczonej pamięci jest równoważne scenariuszowi, w którym niezakończona pętla dodaje wpisy do kolekcji na każdej iteracji.</span><span class="sxs-lookup"><span data-stu-id="1eb6e-269">Consuming unlimited memory is equivalent to the scenario where an unterminated loop adds entries to a collection on every iteration.</span></span>

<span data-ttu-id="1eb6e-270">Aby uniknąć nieskończonych wzorców rekursji, należy się upewnić, że kod renderowania cyklicznego zawiera odpowiednie warunki zatrzymania.</span><span class="sxs-lookup"><span data-stu-id="1eb6e-270">To avoid infinite recursion patterns, ensure that recursive rendering code contains suitable stopping conditions.</span></span>

### <a name="custom-render-tree-logic"></a><span data-ttu-id="1eb6e-271">Logika drzewa renderowania niestandardowego</span><span class="sxs-lookup"><span data-stu-id="1eb6e-271">Custom render tree logic</span></span>

<span data-ttu-id="1eb6e-272">Większość Blazor składników jest implementowana jako pliki *Razor* i są kompilowane do tworzenia logiki, która działa na `RenderTreeBuilder` w celu renderowania danych wyjściowych.</span><span class="sxs-lookup"><span data-stu-id="1eb6e-272">Most Blazor components are implemented as *.razor* files and are compiled to produce logic that operates on a `RenderTreeBuilder` to render their output.</span></span> <span data-ttu-id="1eb6e-273">Deweloper może ręcznie zaimplementować logikę `RenderTreeBuilder` przy użyciu C# kodu proceduralnego.</span><span class="sxs-lookup"><span data-stu-id="1eb6e-273">A developer may manually implement `RenderTreeBuilder` logic using procedural C# code.</span></span> <span data-ttu-id="1eb6e-274">Aby uzyskać więcej informacji, zobacz temat <xref:blazor/components#manual-rendertreebuilder-logic>.</span><span class="sxs-lookup"><span data-stu-id="1eb6e-274">For more information, see <xref:blazor/components#manual-rendertreebuilder-logic>.</span></span>

> [!WARNING]
> <span data-ttu-id="1eb6e-275">Korzystanie z logiki konstruktora drzewa renderowania ręcznego jest uznawane za zaawansowane i niebezpieczne scenariusze, które nie są zalecane do ogólnego tworzenia składników.</span><span class="sxs-lookup"><span data-stu-id="1eb6e-275">Use of manual render tree builder logic is considered an advanced and unsafe scenario, not recommended for general component development.</span></span>

<span data-ttu-id="1eb6e-276">Jeśli `RenderTreeBuilder` kod zostanie zapisany, Deweloper musi zagwarantować poprawność kodu.</span><span class="sxs-lookup"><span data-stu-id="1eb6e-276">If `RenderTreeBuilder` code is written, the developer must guarantee the correctness of the code.</span></span> <span data-ttu-id="1eb6e-277">Na przykład Deweloper musi upewnić się, że:</span><span class="sxs-lookup"><span data-stu-id="1eb6e-277">For example, the developer must ensure that:</span></span>

* <span data-ttu-id="1eb6e-278">Wywołania `OpenElement` i `CloseElement` są prawidłowo zrównoważone.</span><span class="sxs-lookup"><span data-stu-id="1eb6e-278">Calls to `OpenElement` and `CloseElement` are correctly balanced.</span></span>
* <span data-ttu-id="1eb6e-279">Atrybuty są dodawane tylko w prawidłowych miejscach.</span><span class="sxs-lookup"><span data-stu-id="1eb6e-279">Attributes are only added in the correct places.</span></span>

<span data-ttu-id="1eb6e-280">Nieprawidłowa ręczna logika konstruktora drzewa renderowania może spowodować dowolne niezdefiniowane zachowanie, w tym awarie, zawieszenie serwera i luki w zabezpieczeniach.</span><span class="sxs-lookup"><span data-stu-id="1eb6e-280">Incorrect manual render tree builder logic can cause arbitrary undefined behavior, including crashes, server hangs, and security vulnerabilities.</span></span>

<span data-ttu-id="1eb6e-281">Rozważ ręczne renderowanie logiki konstruktora drzew drzewa na tym samym poziomie złożoności i z tym samym poziomem *zagrożenia* co ręczne pisanie kodu zestawu lub instrukcji MSIL.</span><span class="sxs-lookup"><span data-stu-id="1eb6e-281">Consider manual render tree builder logic on the same level of complexity and with the same level of *danger* as writing assembly code or MSIL instructions by hand.</span></span>
