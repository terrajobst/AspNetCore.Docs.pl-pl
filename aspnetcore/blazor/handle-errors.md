---
title: Obsługa błędów w aplikacjach Blazor ASP.NET Core
author: guardrex
description: Odkryj, jak ASP.NET Core Blazor sposób, w jaki Blazor zarządza nieobsługiwanymi wyjątkami i jak opracowywać aplikacje wykrywające i obsługujące błędy.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 11/21/2019
no-loc:
- Blazor
- SignalR
uid: blazor/handle-errors
ms.openlocfilehash: f2fa59259f1dd36f50e81256bddea265e347554b
ms.sourcegitcommit: 3e503ef510008e77be6dd82ee79213c9f7b97607
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/22/2019
ms.locfileid: "74317153"
---
# <a name="handle-errors-in-aspnet-core-opno-locblazor-apps"></a><span data-ttu-id="89167-103">Obsługa błędów w aplikacjach Blazor ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="89167-103">Handle errors in ASP.NET Core Blazor apps</span></span>

<span data-ttu-id="89167-104">[Steve Sanderson](https://github.com/SteveSandersonMS)</span><span class="sxs-lookup"><span data-stu-id="89167-104">By [Steve Sanderson](https://github.com/SteveSandersonMS)</span></span>

<span data-ttu-id="89167-105">W tym artykule opisano sposób, w jaki Blazor zarządza nieobsługiwanymi wyjątkami i jak opracowywać aplikacje wykrywające i obsługujące błędy.</span><span class="sxs-lookup"><span data-stu-id="89167-105">This article describes how Blazor manages unhandled exceptions and how to develop apps that detect and handle errors.</span></span>

::: moniker range=">= aspnetcore-3.1"

## <a name="detailed-errors-during-development"></a><span data-ttu-id="89167-106">Szczegóły błędów podczas opracowywania</span><span class="sxs-lookup"><span data-stu-id="89167-106">Detailed errors during development</span></span>

<span data-ttu-id="89167-107">Gdy aplikacja Blazor nie działa prawidłowo podczas opracowywania, otrzymywanie szczegółowych informacji o błędach z aplikacji pomaga w rozwiązywaniu problemów i rozwiązaniu problemu.</span><span class="sxs-lookup"><span data-stu-id="89167-107">When a Blazor app isn't functioning properly during development, receiving detailed error information from the app assists in troubleshooting and fixing the issue.</span></span> <span data-ttu-id="89167-108">Gdy wystąpi błąd, Blazor aplikacje wyświetlają złoty pasek u dołu ekranu:</span><span class="sxs-lookup"><span data-stu-id="89167-108">When an error occurs, Blazor apps display a gold bar at the bottom of the screen:</span></span>

* <span data-ttu-id="89167-109">W trakcie programowania złoty pasek kieruje użytkownika do konsoli przeglądarki, gdzie można zobaczyć wyjątek.</span><span class="sxs-lookup"><span data-stu-id="89167-109">During development, the gold bar directs you to the browser console, where you can see the exception.</span></span>
* <span data-ttu-id="89167-110">W środowisku produkcyjnym złoty pasek powiadamia użytkownika o wystąpieniu błędu i zaleca odświeżenie przeglądarki.</span><span class="sxs-lookup"><span data-stu-id="89167-110">In production, the gold bar notifies the user that an error has occurred and recommends refreshing the browser.</span></span>

<span data-ttu-id="89167-111">Interfejs użytkownika dla tego środowiska obsługi błędów jest częścią szablonów projektu Blazor.</span><span class="sxs-lookup"><span data-stu-id="89167-111">The UI for this error handling experience is part of the Blazor project templates.</span></span> <span data-ttu-id="89167-112">W aplikacji Blazor webassembly Dostosuj środowisko w pliku *wwwroot/index.html* :</span><span class="sxs-lookup"><span data-stu-id="89167-112">In a Blazor WebAssembly app, customize the experience in the *wwwroot/index.html* file:</span></span>

```html
<div id="blazor-error-ui">
    An unhandled error has occurred.
    <a href="" class="reload">Reload</a>
    <a class="dismiss">🗙</a>
</div>
```

<span data-ttu-id="89167-113">W aplikacji serwera Blazor Dostosuj środowisko w pliku *Pages/_Host. cshtml* :</span><span class="sxs-lookup"><span data-stu-id="89167-113">In a Blazor Server app, customize the experience in the *Pages/_Host.cshtml* file:</span></span>

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

<span data-ttu-id="89167-114">Element `blazor-error-ui` jest ukryty przez style dołączone do Blazor szablonów, a następnie pokazywany w przypadku wystąpienia błędu.</span><span class="sxs-lookup"><span data-stu-id="89167-114">The `blazor-error-ui` element is hidden by the styles included with the Blazor templates and then shown when an error occurs.</span></span>

::: moniker-end

## <a name="how-the-opno-locblazor-framework-reacts-to-unhandled-exceptions"></a><span data-ttu-id="89167-115">Jak struktura Blazor reaguje na Nieobsłużone wyjątki</span><span class="sxs-lookup"><span data-stu-id="89167-115">How the Blazor framework reacts to unhandled exceptions</span></span>

<span data-ttu-id="89167-116">Serwer Blazor jest strukturą stanową.</span><span class="sxs-lookup"><span data-stu-id="89167-116">Blazor Server is a stateful framework.</span></span> <span data-ttu-id="89167-117">Gdy użytkownicy współpracują z aplikacją, utrzymują połączenie z serwerem znanym jako *obwód*.</span><span class="sxs-lookup"><span data-stu-id="89167-117">While users interact with an app, they maintain a connection to the server known as a *circuit*.</span></span> <span data-ttu-id="89167-118">Obwód zawiera aktywne wystąpienia składnika, a także wiele innych aspektów stanu, takich jak:</span><span class="sxs-lookup"><span data-stu-id="89167-118">The circuit holds active component instances, plus many other aspects of state, such as:</span></span>

* <span data-ttu-id="89167-119">Najnowsze renderowane dane wyjściowe składników.</span><span class="sxs-lookup"><span data-stu-id="89167-119">The most recent rendered output of components.</span></span>
* <span data-ttu-id="89167-120">Bieżący zestaw delegatów obsługi zdarzeń, które mogą być wyzwalane przez zdarzenia po stronie klienta.</span><span class="sxs-lookup"><span data-stu-id="89167-120">The current set of event-handling delegates that could be triggered by client-side events.</span></span>

<span data-ttu-id="89167-121">Jeśli użytkownik otworzy aplikację na wielu kartach przeglądarki, mają one wiele niezależnych obwodów.</span><span class="sxs-lookup"><span data-stu-id="89167-121">If a user opens the app in multiple browser tabs, they have multiple independent circuits.</span></span>

Blazor<span data-ttu-id="89167-122"> traktuje większość nieobsłużonych wyjątków jako krytyczne dla obwodu, w którym występują.</span><span class="sxs-lookup"><span data-stu-id="89167-122"> treats most unhandled exceptions as fatal to the circuit where they occur.</span></span> <span data-ttu-id="89167-123">Jeśli obwód zostanie przerwany z powodu nieobsługiwanego wyjątku, użytkownik może nadal korzystać z aplikacji tylko przez ponowne załadowanie strony w celu utworzenia nowego obwodu.</span><span class="sxs-lookup"><span data-stu-id="89167-123">If a circuit is terminated due to an unhandled exception, the user can only continue to interact with the app by reloading the page to create a new circuit.</span></span> <span data-ttu-id="89167-124">Nie ma to wpływu na obwody, które zostały przerwane, które są obwody dla innych użytkowników lub kart przeglądarki.</span><span class="sxs-lookup"><span data-stu-id="89167-124">Circuits outside of the one that's terminated, which are circuits for other users or other browser tabs, aren't affected.</span></span> <span data-ttu-id="89167-125">Ten scenariusz jest podobny do aplikacji klasycznej, która uległa awarii,&mdash;należy ponownie uruchomić aplikację, ale nie ma to wpływu na inne aplikacje.</span><span class="sxs-lookup"><span data-stu-id="89167-125">This scenario is similar to a desktop app that crashes&mdash;the crashed app must be restarted, but other apps aren't affected.</span></span>

<span data-ttu-id="89167-126">Obwód jest zakończony, gdy wystąpił nieobsługiwany wyjątek z następujących powodów:</span><span class="sxs-lookup"><span data-stu-id="89167-126">A circuit is terminated when an unhandled exception occurs for the following reasons:</span></span>

* <span data-ttu-id="89167-127">Nieobsługiwany wyjątek często pozostawia obwód w niezdefiniowanym stanie.</span><span class="sxs-lookup"><span data-stu-id="89167-127">An unhandled exception often leaves the circuit in an undefined state.</span></span>
* <span data-ttu-id="89167-128">Nie można zagwarantować normalnej operacji aplikacji po wystąpieniu nieobsłużonego wyjątku.</span><span class="sxs-lookup"><span data-stu-id="89167-128">The app's normal operation can't be guaranteed after an unhandled exception.</span></span>
* <span data-ttu-id="89167-129">W przypadku kontynuowania obwodu w aplikacji mogą pojawić się luki w zabezpieczeniach.</span><span class="sxs-lookup"><span data-stu-id="89167-129">Security vulnerabilities may appear in the app if the circuit continues.</span></span>

## <a name="manage-unhandled-exceptions-in-developer-code"></a><span data-ttu-id="89167-130">Zarządzanie nieobsługiwanymi wyjątkami w kodzie dewelopera</span><span class="sxs-lookup"><span data-stu-id="89167-130">Manage unhandled exceptions in developer code</span></span>

<span data-ttu-id="89167-131">Aby aplikacja kontynuowała działanie po wystąpieniu błędu, aplikacja musi mieć logikę obsługi błędów.</span><span class="sxs-lookup"><span data-stu-id="89167-131">For an app to continue after an error, the app must have error handling logic.</span></span> <span data-ttu-id="89167-132">W dalszej części tego artykułu opisano potencjalne źródła nieobsłużonych wyjątków.</span><span class="sxs-lookup"><span data-stu-id="89167-132">Later sections of this article describe potential sources of unhandled exceptions.</span></span>

<span data-ttu-id="89167-133">W środowisku produkcyjnym nie Renderuj komunikatów wyjątków struktury ani śladów stosu w interfejsie użytkownika.</span><span class="sxs-lookup"><span data-stu-id="89167-133">In production, don't render framework exception messages or stack traces in the UI.</span></span> <span data-ttu-id="89167-134">Renderowanie komunikatów o wyjątkach lub śladów stosu może:</span><span class="sxs-lookup"><span data-stu-id="89167-134">Rendering exception messages or stack traces could:</span></span>

* <span data-ttu-id="89167-135">Ujawnianie poufnych informacji użytkownikom końcowym.</span><span class="sxs-lookup"><span data-stu-id="89167-135">Disclose sensitive information to end users.</span></span>
* <span data-ttu-id="89167-136">Pomóż złośliwemu użytkownikowi wykrywać słabe strony w aplikacji, która może naruszyć bezpieczeństwo aplikacji, serwera lub sieci.</span><span class="sxs-lookup"><span data-stu-id="89167-136">Help a malicious user discover weaknesses in an app that can compromise the security of the app, server, or network.</span></span>

## <a name="log-errors-with-a-persistent-provider"></a><span data-ttu-id="89167-137">Rejestrowanie błędów przez dostawcę trwałego</span><span class="sxs-lookup"><span data-stu-id="89167-137">Log errors with a persistent provider</span></span>

<span data-ttu-id="89167-138">Jeśli wystąpi nieobsługiwany wyjątek, wyjątek jest rejestrowany w celu <xref:Microsoft.Extensions.Logging.ILogger> wystąpień skonfigurowanych w kontenerze usługi.</span><span class="sxs-lookup"><span data-stu-id="89167-138">If an unhandled exception occurs, the exception is logged to <xref:Microsoft.Extensions.Logging.ILogger> instances configured in the service container.</span></span> <span data-ttu-id="89167-139">Domyślnie aplikacje Blazor rejestrować w danych wyjściowych konsoli za pomocą dostawcy rejestrowania konsoli.</span><span class="sxs-lookup"><span data-stu-id="89167-139">By default, Blazor apps log to console output with the Console Logging Provider.</span></span> <span data-ttu-id="89167-140">Należy rozważyć logowanie do bardziej trwałej lokalizacji z dostawcą, który zarządza rozmiarem dziennika i rotacją dzienników.</span><span class="sxs-lookup"><span data-stu-id="89167-140">Consider logging to a more permanent location with a provider that manages log size and log rotation.</span></span> <span data-ttu-id="89167-141">Aby uzyskać więcej informacji, zobacz <xref:fundamentals/logging/index>.</span><span class="sxs-lookup"><span data-stu-id="89167-141">For more information, see <xref:fundamentals/logging/index>.</span></span>

<span data-ttu-id="89167-142">Podczas opracowywania, Blazor zwykle wysyła pełne szczegóły wyjątków do konsoli przeglądarki w celu ułatwienia debugowania.</span><span class="sxs-lookup"><span data-stu-id="89167-142">During development, Blazor usually sends the full details of exceptions to the browser's console to aid in debugging.</span></span> <span data-ttu-id="89167-143">W środowisku produkcyjnym szczegółowe błędy w konsoli przeglądarki są domyślnie wyłączone, co oznacza, że błędy nie są wysyłane do klientów, ale wszystkie szczegóły wyjątku nadal są rejestrowane po stronie serwera.</span><span class="sxs-lookup"><span data-stu-id="89167-143">In production, detailed errors in the browser's console are disabled by default, which means that errors aren't sent to clients but the exception's full details are still logged server-side.</span></span> <span data-ttu-id="89167-144">Aby uzyskać więcej informacji, zobacz <xref:fundamentals/error-handling>.</span><span class="sxs-lookup"><span data-stu-id="89167-144">For more information, see <xref:fundamentals/error-handling>.</span></span>

<span data-ttu-id="89167-145">Należy zdecydować, które zdarzenia mają być rejestrowane, oraz poziom ważności zarejestrowanych zdarzeń.</span><span class="sxs-lookup"><span data-stu-id="89167-145">You must decide which incidents to log and the level of severity of logged incidents.</span></span> <span data-ttu-id="89167-146">Nieszkodliwi użytkownicy mogą być w stanie wyzwolić błędy w sposób celowy.</span><span class="sxs-lookup"><span data-stu-id="89167-146">Hostile users might be able to trigger errors deliberately.</span></span> <span data-ttu-id="89167-147">Na przykład nie należy rejestrować zdarzenia z błędu, gdzie w adresie URL składnika, który zawiera szczegóły produktu, jest podano nieznanego `ProductId`.</span><span class="sxs-lookup"><span data-stu-id="89167-147">For example, don't log an incident from an error where an unknown `ProductId` is supplied in the URL of a component that displays product details.</span></span> <span data-ttu-id="89167-148">Nie wszystkie błędy powinny być traktowane jako zdarzenia o wysokiej ważności do rejestrowania.</span><span class="sxs-lookup"><span data-stu-id="89167-148">Not all errors should be treated as high-severity incidents for logging.</span></span>

## <a name="places-where-errors-may-occur"></a><span data-ttu-id="89167-149">Miejsca, w których mogą wystąpić błędy</span><span class="sxs-lookup"><span data-stu-id="89167-149">Places where errors may occur</span></span>

<span data-ttu-id="89167-150">Kod struktury i aplikacji może wyzwolić Nieobsłużone wyjątki w jednej z następujących lokalizacji:</span><span class="sxs-lookup"><span data-stu-id="89167-150">Framework and app code may trigger unhandled exceptions in any of the following locations:</span></span>

* [<span data-ttu-id="89167-151">Tworzenie wystąpienia składnika</span><span class="sxs-lookup"><span data-stu-id="89167-151">Component instantiation</span></span>](#component-instantiation)
* [<span data-ttu-id="89167-152">Metody cyklu życia</span><span class="sxs-lookup"><span data-stu-id="89167-152">Lifecycle methods</span></span>](#lifecycle-methods)
* [<span data-ttu-id="89167-153">Logika renderowania</span><span class="sxs-lookup"><span data-stu-id="89167-153">Rendering logic</span></span>](#rendering-logic)
* [<span data-ttu-id="89167-154">Programy obsługi zdarzeń</span><span class="sxs-lookup"><span data-stu-id="89167-154">Event handlers</span></span>](#event-handlers)
* [<span data-ttu-id="89167-155">Usuwanie składników</span><span class="sxs-lookup"><span data-stu-id="89167-155">Component disposal</span></span>](#component-disposal)
* [<span data-ttu-id="89167-156">Międzyoperacyjność JavaScript</span><span class="sxs-lookup"><span data-stu-id="89167-156">JavaScript interop</span></span>](#javascript-interop)
* [<span data-ttu-id="89167-157">Programy obsługi obwodu</span><span class="sxs-lookup"><span data-stu-id="89167-157">Circuit handlers</span></span>](#circuit-handlers)
* [<span data-ttu-id="89167-158">Usuwanie obwodu</span><span class="sxs-lookup"><span data-stu-id="89167-158">Circuit disposal</span></span>](#circuit-disposal)
* [<span data-ttu-id="89167-159">Renderowanie prerenderingu</span><span class="sxs-lookup"><span data-stu-id="89167-159">Prerendering</span></span>](#prerendering)

<span data-ttu-id="89167-160">Poprzednie Nieobsłużone wyjątki zostały opisane w poniższych sekcjach tego artykułu.</span><span class="sxs-lookup"><span data-stu-id="89167-160">The preceding unhandled exceptions are described in the following sections of this article.</span></span>

### <a name="component-instantiation"></a><span data-ttu-id="89167-161">Tworzenie wystąpienia składnika</span><span class="sxs-lookup"><span data-stu-id="89167-161">Component instantiation</span></span>

<span data-ttu-id="89167-162">Gdy Blazor tworzy wystąpienie składnika:</span><span class="sxs-lookup"><span data-stu-id="89167-162">When Blazor creates an instance of a component:</span></span>

* <span data-ttu-id="89167-163">Konstruktor składnika jest wywoływany.</span><span class="sxs-lookup"><span data-stu-id="89167-163">The component's constructor is invoked.</span></span>
* <span data-ttu-id="89167-164">Są wywoływane konstruktory wszelkich niepojedynczych usług DI dostarczonych do konstruktora składnika za pośrednictwem dyrektywy [@inject](xref:blazor/dependency-injection#request-a-service-in-a-component) lub atrybutu [[wstrzyknięcie]](xref:blazor/dependency-injection#request-a-service-in-a-component) .</span><span class="sxs-lookup"><span data-stu-id="89167-164">The constructors of any non-singleton DI services supplied to the component's constructor via the [@inject](xref:blazor/dependency-injection#request-a-service-in-a-component) directive or the [[Inject]](xref:blazor/dependency-injection#request-a-service-in-a-component) attribute are invoked.</span></span> 

<span data-ttu-id="89167-165">Obwód kończy się niepowodzeniem, gdy dowolny wykonany Konstruktor lub setter dla każdej właściwości `[Inject]` zgłasza nieobsłużony wyjątek.</span><span class="sxs-lookup"><span data-stu-id="89167-165">A circuit fails when any executed constructor or a setter for any `[Inject]` property throws an unhandled exception.</span></span> <span data-ttu-id="89167-166">Wyjątek jest krytyczny, ponieważ struktura nie może utworzyć wystąpienia składnika.</span><span class="sxs-lookup"><span data-stu-id="89167-166">The exception is fatal because the framework can't instantiate the component.</span></span> <span data-ttu-id="89167-167">Jeśli logika konstruktora może generować wyjątki, aplikacja powinna zalewkować wyjątki przy użyciu instrukcji [try-catch](/dotnet/csharp/language-reference/keywords/try-catch) z obsługą błędów i rejestrowaniem.</span><span class="sxs-lookup"><span data-stu-id="89167-167">If constructor logic may throw exceptions, the app should trap the exceptions using a [try-catch](/dotnet/csharp/language-reference/keywords/try-catch) statement with error handling and logging.</span></span>

### <a name="lifecycle-methods"></a><span data-ttu-id="89167-168">Metody cyklu życia</span><span class="sxs-lookup"><span data-stu-id="89167-168">Lifecycle methods</span></span>

<span data-ttu-id="89167-169">W trakcie okresu istnienia składnika Blazor wywołuje metody cyklu życia:</span><span class="sxs-lookup"><span data-stu-id="89167-169">During the lifetime of a component, Blazor invokes lifecycle methods:</span></span>

* `OnInitialized` / `OnInitializedAsync`
* `OnParametersSet` / `OnParametersSetAsync`
* `ShouldRender` / `ShouldRenderAsync`
* `OnAfterRender` / `OnAfterRenderAsync`

<span data-ttu-id="89167-170">Jeśli jakakolwiek metoda cyklu życia zgłasza wyjątek, synchronicznie lub asynchronicznie, wyjątek jest krytyczny dla obwodu.</span><span class="sxs-lookup"><span data-stu-id="89167-170">If any lifecycle method throws an exception, synchronously or asynchronously, the exception is fatal to the circuit.</span></span> <span data-ttu-id="89167-171">Aby składniki zajmowały błędy w metodach cyklu życia, Dodaj logikę obsługi błędów.</span><span class="sxs-lookup"><span data-stu-id="89167-171">For components to deal with errors in lifecycle methods, add error handling logic.</span></span>

<span data-ttu-id="89167-172">W poniższym przykładzie, gdzie `OnParametersSetAsync` wywołuje metodę w celu uzyskania produktu:</span><span class="sxs-lookup"><span data-stu-id="89167-172">In the following example where `OnParametersSetAsync` calls a method to obtain a product:</span></span>

* <span data-ttu-id="89167-173">Wyjątek zgłoszony w metodzie `ProductRepository.GetProductByIdAsync` jest obsługiwany przez instrukcję `try-catch`.</span><span class="sxs-lookup"><span data-stu-id="89167-173">An exception thrown in the `ProductRepository.GetProductByIdAsync` method is handled by a `try-catch` statement.</span></span>
* <span data-ttu-id="89167-174">Gdy jest wykonywany blok `catch`:</span><span class="sxs-lookup"><span data-stu-id="89167-174">When the `catch` block is executed:</span></span>
  * <span data-ttu-id="89167-175">`loadFailed` jest ustawiony na `true`, który jest używany do wyświetlania komunikatu o błędzie dla użytkownika.</span><span class="sxs-lookup"><span data-stu-id="89167-175">`loadFailed` is set to `true`, which is used to display an error message to the user.</span></span>
  * <span data-ttu-id="89167-176">Błąd jest rejestrowany.</span><span class="sxs-lookup"><span data-stu-id="89167-176">The error is logged.</span></span>

[!code-cshtml[](handle-errors/samples_snapshot/3.x/product-details.razor?highlight=11,27-39)]

### <a name="rendering-logic"></a><span data-ttu-id="89167-177">Logika renderowania</span><span class="sxs-lookup"><span data-stu-id="89167-177">Rendering logic</span></span>

<span data-ttu-id="89167-178">Znaczniki deklaratywne w pliku składnika `.razor` są kompilowane do C# metody o nazwie `BuildRenderTree`.</span><span class="sxs-lookup"><span data-stu-id="89167-178">The declarative markup in a `.razor` component file is compiled into a C# method called `BuildRenderTree`.</span></span> <span data-ttu-id="89167-179">Gdy składnik jest renderowany, `BuildRenderTree` wykonuje i tworzy strukturę danych opisującą elementy, tekst i składniki podrzędne renderowanego składnika.</span><span class="sxs-lookup"><span data-stu-id="89167-179">When a component renders, `BuildRenderTree` executes and builds up a data structure describing the elements, text, and child components of the rendered component.</span></span>

<span data-ttu-id="89167-180">Logika renderowania może zgłosić wyjątek.</span><span class="sxs-lookup"><span data-stu-id="89167-180">Rendering logic can throw an exception.</span></span> <span data-ttu-id="89167-181">Przykład tego scenariusza występuje, gdy `@someObject.PropertyName` jest oceniane, ale `@someObject` `null`.</span><span class="sxs-lookup"><span data-stu-id="89167-181">An example of this scenario occurs when `@someObject.PropertyName` is evaluated but `@someObject` is `null`.</span></span> <span data-ttu-id="89167-182">Nieobsługiwany wyjątek zgłoszony przez logikę renderowania jest krytyczny dla obwodu.</span><span class="sxs-lookup"><span data-stu-id="89167-182">An unhandled exception thrown by rendering logic is fatal to the circuit.</span></span>

<span data-ttu-id="89167-183">Aby zapobiec wystąpieniu wyjątku odwołania o wartości null w logice renderowania, przed uzyskaniem dostępu do elementów członkowskich Sprawdź, czy `null` obiektu.</span><span class="sxs-lookup"><span data-stu-id="89167-183">To prevent a null reference exception in rendering logic, check for a `null` object before accessing its members.</span></span> <span data-ttu-id="89167-184">W poniższym przykładzie właściwości `person.Address` nie są dostępne w przypadku `null``person.Address`:</span><span class="sxs-lookup"><span data-stu-id="89167-184">In the following example, `person.Address` properties aren't accessed if `person.Address` is `null`:</span></span>

[!code-cshtml[](handle-errors/samples_snapshot/3.x/person-example.razor?highlight=1)]

<span data-ttu-id="89167-185">Poprzedni kod założono, że `person` nie `null`.</span><span class="sxs-lookup"><span data-stu-id="89167-185">The preceding code assumes that `person` isn't `null`.</span></span> <span data-ttu-id="89167-186">Często Struktura kodu gwarantuje, że obiekt istnieje w momencie renderowania składnika.</span><span class="sxs-lookup"><span data-stu-id="89167-186">Often, the structure of the code guarantees that an object exists at the time the component is rendered.</span></span> <span data-ttu-id="89167-187">W takich przypadkach nie trzeba sprawdzać `null` w logice renderowania.</span><span class="sxs-lookup"><span data-stu-id="89167-187">In those cases, it isn't necessary to check for `null` in rendering logic.</span></span> <span data-ttu-id="89167-188">W poprzednim przykładzie można zagwarantować, że istnieje `person`, ponieważ `person` jest tworzony podczas tworzenia wystąpienia składnika.</span><span class="sxs-lookup"><span data-stu-id="89167-188">In the prior example, `person` might be guaranteed to exist because `person` is created when the component is instantiated.</span></span>

### <a name="event-handlers"></a><span data-ttu-id="89167-189">Programy obsługi zdarzeń</span><span class="sxs-lookup"><span data-stu-id="89167-189">Event handlers</span></span>

<span data-ttu-id="89167-190">Kod po stronie klienta wyzwala wywołania kodu, C# gdy programy obsługi zdarzeń są tworzone przy użyciu:</span><span class="sxs-lookup"><span data-stu-id="89167-190">Client-side code triggers invocations of C# code when event handlers are created using:</span></span>

* `@onclick`
* `@onchange`
* <span data-ttu-id="89167-191">Inne atrybuty `@on...`</span><span class="sxs-lookup"><span data-stu-id="89167-191">Other `@on...` attributes</span></span>
* `@bind`

<span data-ttu-id="89167-192">Kod procedury obsługi zdarzeń może zgłosić nieobsługiwany wyjątek w tych scenariuszach.</span><span class="sxs-lookup"><span data-stu-id="89167-192">Event handler code might throw an unhandled exception in these scenarios.</span></span>

<span data-ttu-id="89167-193">Jeśli procedura obsługi zdarzeń zgłasza nieobsługiwany wyjątek (na przykład kwerenda bazy danych kończy się niepowodzeniem), wyjątek jest krytyczny dla obwodu.</span><span class="sxs-lookup"><span data-stu-id="89167-193">If an event handler throws an unhandled exception (for example, a database query fails), the exception is fatal to the circuit.</span></span> <span data-ttu-id="89167-194">Jeśli aplikacja wywołuje kod, który może zakończyć się niepowodzeniem z powodów zewnętrznych, należy zastosować wyjątek pułapki przy użyciu instrukcji [try-catch](/dotnet/csharp/language-reference/keywords/try-catch) z obsługą błędów i rejestrowaniem.</span><span class="sxs-lookup"><span data-stu-id="89167-194">If the app calls code that could fail for external reasons, trap exceptions using a [try-catch](/dotnet/csharp/language-reference/keywords/try-catch) statement with error handling and logging.</span></span>

<span data-ttu-id="89167-195">Jeśli kod użytkownika nie jest pułapk i nie obsługuje wyjątku, struktura rejestruje wyjątek i kończy obwód.</span><span class="sxs-lookup"><span data-stu-id="89167-195">If user code doesn't trap and handle the exception, the framework logs the exception and terminates the circuit.</span></span>

### <a name="component-disposal"></a><span data-ttu-id="89167-196">Usuwanie składników</span><span class="sxs-lookup"><span data-stu-id="89167-196">Component disposal</span></span>

<span data-ttu-id="89167-197">Składnik może zostać usunięty z interfejsu użytkownika, na przykład, ponieważ użytkownik przeszedł do innej strony.</span><span class="sxs-lookup"><span data-stu-id="89167-197">A component may be removed from the UI, for example, because the user has navigated to another page.</span></span> <span data-ttu-id="89167-198">Gdy składnik implementujący <xref:System.IDisposable?displayProperty=fullName> jest usuwany z interfejsu użytkownika, struktura wywołuje metodę <xref:System.IDisposable.Dispose*> składnika.</span><span class="sxs-lookup"><span data-stu-id="89167-198">When a component that implements <xref:System.IDisposable?displayProperty=fullName> is removed from the UI, the framework calls the component's <xref:System.IDisposable.Dispose*> method.</span></span> 

<span data-ttu-id="89167-199">Jeśli metoda `Dispose` składnika zgłasza nieobsługiwany wyjątek, wyjątek jest krytyczny dla obwodu.</span><span class="sxs-lookup"><span data-stu-id="89167-199">If the component's `Dispose` method throws an unhandled exception, the exception is fatal to the circuit.</span></span> <span data-ttu-id="89167-200">Jeśli logika usuwania może generować wyjątki, aplikacja powinna zalewkować wyjątki przy użyciu instrukcji [try-catch](/dotnet/csharp/language-reference/keywords/try-catch) z obsługą błędów i rejestrowaniem.</span><span class="sxs-lookup"><span data-stu-id="89167-200">If disposal logic may throw exceptions, the app should trap the exceptions using a [try-catch](/dotnet/csharp/language-reference/keywords/try-catch) statement with error handling and logging.</span></span>

<span data-ttu-id="89167-201">Aby uzyskać więcej informacji na temat usuwania składników, zobacz <xref:blazor/components#component-disposal-with-idisposable>.</span><span class="sxs-lookup"><span data-stu-id="89167-201">For more information on component disposal, see <xref:blazor/components#component-disposal-with-idisposable>.</span></span>

### <a name="javascript-interop"></a><span data-ttu-id="89167-202">Międzyoperacyjność w języku JavaScript</span><span class="sxs-lookup"><span data-stu-id="89167-202">JavaScript interop</span></span>

<span data-ttu-id="89167-203">`IJSRuntime.InvokeAsync<T>` umożliwia kodowi .NET wykonywanie wywołań asynchronicznych do środowiska uruchomieniowego JavaScript w przeglądarce użytkownika.</span><span class="sxs-lookup"><span data-stu-id="89167-203">`IJSRuntime.InvokeAsync<T>` allows .NET code to make asynchronous calls to the JavaScript runtime in the user's browser.</span></span>

<span data-ttu-id="89167-204">Poniższe warunki dotyczą obsługi błędów w `InvokeAsync<T>`:</span><span class="sxs-lookup"><span data-stu-id="89167-204">The following conditions apply to error handling with `InvokeAsync<T>`:</span></span>

* <span data-ttu-id="89167-205">Jeśli wywołanie `InvokeAsync<T>` nie powiedzie się synchronicznie, wystąpi wyjątek programu .NET.</span><span class="sxs-lookup"><span data-stu-id="89167-205">If a call to `InvokeAsync<T>` fails synchronously, a .NET exception occurs.</span></span> <span data-ttu-id="89167-206">Wywołanie `InvokeAsync<T>` może zakończyć się niepowodzeniem, na przykład dlatego, że nie można serializować dostarczonych argumentów.</span><span class="sxs-lookup"><span data-stu-id="89167-206">A call to `InvokeAsync<T>` may fail, for example, because the supplied arguments can't be serialized.</span></span> <span data-ttu-id="89167-207">Kod dewelopera musi przechwycić wyjątek.</span><span class="sxs-lookup"><span data-stu-id="89167-207">Developer code must catch the exception.</span></span> <span data-ttu-id="89167-208">Jeśli kod aplikacji w obsłudze zdarzeń lub metoda cyklu życia składnika nie obsługuje wyjątku, wynikający z nich wyjątek jest krytyczny dla obwodu.</span><span class="sxs-lookup"><span data-stu-id="89167-208">If app code in an event handler or component lifecycle method doesn't handle an exception, the resulting exception is fatal to the circuit.</span></span>
* <span data-ttu-id="89167-209">Jeśli wywołanie `InvokeAsync<T>` nie powiedzie się asynchronicznie, <xref:System.Threading.Tasks.Task> .NET kończy się niepowodzeniem.</span><span class="sxs-lookup"><span data-stu-id="89167-209">If a call to `InvokeAsync<T>` fails asynchronously, the .NET <xref:System.Threading.Tasks.Task> fails.</span></span> <span data-ttu-id="89167-210">Wywołanie `InvokeAsync<T>` może zakończyć się niepowodzeniem, na przykład ponieważ kod po stronie JavaScript zgłasza wyjątek lub zwraca `Promise`, który został ukończony jako `rejected`.</span><span class="sxs-lookup"><span data-stu-id="89167-210">A call to `InvokeAsync<T>` may fail, for example, because the JavaScript-side code throws an exception or returns a `Promise` that completed as `rejected`.</span></span> <span data-ttu-id="89167-211">Kod dewelopera musi przechwycić wyjątek.</span><span class="sxs-lookup"><span data-stu-id="89167-211">Developer code must catch the exception.</span></span> <span data-ttu-id="89167-212">W przypadku użycia operatora [await](/dotnet/csharp/language-reference/keywords/await) Rozważ zapakowanie wywołania metody w instrukcji [try-catch](/dotnet/csharp/language-reference/keywords/try-catch) z obsługą błędów i rejestrowaniem.</span><span class="sxs-lookup"><span data-stu-id="89167-212">If using the [await](/dotnet/csharp/language-reference/keywords/await) operator, consider wrapping the method call in a [try-catch](/dotnet/csharp/language-reference/keywords/try-catch) statement with error handling and logging.</span></span> <span data-ttu-id="89167-213">W przeciwnym razie niepowodzenie kodu spowoduje nieobsłużony wyjątek, który jest krytyczny dla obwodu.</span><span class="sxs-lookup"><span data-stu-id="89167-213">Otherwise, the failing code results in an unhandled exception that's fatal to the circuit.</span></span>
* <span data-ttu-id="89167-214">Domyślnie wywołania do `InvokeAsync<T>` muszą zakończyć się w określonym czasie lub w przeciwnym razie upłynął limit czasu połączenia. Domyślny limit czasu wynosi jedną minutę.</span><span class="sxs-lookup"><span data-stu-id="89167-214">By default, calls to `InvokeAsync<T>` must complete within a certain period or else the call times out. The default timeout period is one minute.</span></span> <span data-ttu-id="89167-215">Limit czasu chroni kod przed utratą połączenia sieciowego lub kodem JavaScript, który nigdy nie odsyła komunikat uzupełniający.</span><span class="sxs-lookup"><span data-stu-id="89167-215">The timeout protects the code against a loss in network connectivity or JavaScript code that never sends back a completion message.</span></span> <span data-ttu-id="89167-216">Jeśli wystąpiło przełączenie, wynikiem `Task` zakończy się niepowodzeniem z <xref:System.OperationCanceledException>.</span><span class="sxs-lookup"><span data-stu-id="89167-216">If the call times out, the resulting `Task` fails with an <xref:System.OperationCanceledException>.</span></span> <span data-ttu-id="89167-217">Zalewka i przetwórz wyjątek z rejestrowaniem.</span><span class="sxs-lookup"><span data-stu-id="89167-217">Trap and process the exception with logging.</span></span>

<span data-ttu-id="89167-218">Podobnie kod JavaScript może inicjować wywołania metod .NET wskazywanych przez [atrybut [JSInvokable]](xref:blazor/javascript-interop#invoke-net-methods-from-javascript-functions).</span><span class="sxs-lookup"><span data-stu-id="89167-218">Similarly, JavaScript code may initiate calls to .NET methods indicated by the [[JSInvokable] attribute](xref:blazor/javascript-interop#invoke-net-methods-from-javascript-functions).</span></span> <span data-ttu-id="89167-219">Jeśli te metody .NET zgłaszają nieobsługiwany wyjątek:</span><span class="sxs-lookup"><span data-stu-id="89167-219">If these .NET methods throw an unhandled exception:</span></span>

* <span data-ttu-id="89167-220">Wyjątek nie jest traktowany jako krytyczny dla obwodu.</span><span class="sxs-lookup"><span data-stu-id="89167-220">The exception isn't treated as fatal to the circuit.</span></span>
* <span data-ttu-id="89167-221">`Promise` po stronie JavaScript zostanie odrzucony.</span><span class="sxs-lookup"><span data-stu-id="89167-221">The JavaScript-side `Promise` is rejected.</span></span>

<span data-ttu-id="89167-222">Istnieje możliwość użycia kodu obsługi błędów po stronie .NET lub stronie JavaScript wywołania metody.</span><span class="sxs-lookup"><span data-stu-id="89167-222">You have the option of using error handling code on either the .NET side or the JavaScript side of the method call.</span></span>

<span data-ttu-id="89167-223">Aby uzyskać więcej informacji, zobacz <xref:blazor/javascript-interop>.</span><span class="sxs-lookup"><span data-stu-id="89167-223">For more information, see <xref:blazor/javascript-interop>.</span></span>

### <a name="circuit-handlers"></a><span data-ttu-id="89167-224">Programy obsługi obwodu</span><span class="sxs-lookup"><span data-stu-id="89167-224">Circuit handlers</span></span>

Blazor<span data-ttu-id="89167-225"> umożliwia kodowi Definiowanie *procedury obsługi obwodu*, która otrzymuje powiadomienia, gdy zmieni się stan obwodu użytkownika.</span><span class="sxs-lookup"><span data-stu-id="89167-225"> allows code to define a *circuit handler*, which receives notifications when the state of a user's circuit changes.</span></span> <span data-ttu-id="89167-226">Używane są następujące stany:</span><span class="sxs-lookup"><span data-stu-id="89167-226">The following states are used:</span></span>

* `initialized`
* `connected`
* `disconnected`
* `disposed`

<span data-ttu-id="89167-227">Powiadomienia są zarządzane przez zarejestrowanie usługi DI, która dziedziczy z `CircuitHandler` abstrakcyjnej klasy bazowej.</span><span class="sxs-lookup"><span data-stu-id="89167-227">Notifications are managed by registering a DI service that inherits from the `CircuitHandler` abstract base class.</span></span>

<span data-ttu-id="89167-228">Jeśli metody obsługi niestandardowego obwodu zgłaszają nieobsługiwany wyjątek, wyjątek jest krytyczny dla obwodu.</span><span class="sxs-lookup"><span data-stu-id="89167-228">If a custom circuit handler's methods throw an unhandled exception, the exception is fatal to the circuit.</span></span> <span data-ttu-id="89167-229">Aby tolerować wyjątki w kodzie programu obsługi lub metodach wywoływanych, zawiń kod w co najmniej jednej instrukcji [try-catch](/dotnet/csharp/language-reference/keywords/try-catch) z obsługą błędów i rejestrowaniem.</span><span class="sxs-lookup"><span data-stu-id="89167-229">To tolerate exceptions in a handler's code or called methods, wrap the code in one or more [try-catch](/dotnet/csharp/language-reference/keywords/try-catch) statements with error handling and logging.</span></span>

### <a name="circuit-disposal"></a><span data-ttu-id="89167-230">Usuwanie obwodu</span><span class="sxs-lookup"><span data-stu-id="89167-230">Circuit disposal</span></span>

<span data-ttu-id="89167-231">Gdy obwód kończy się, ponieważ użytkownik odłączył się i struktura czyści stan obwodu, struktura usuwa zakres DI obwodu.</span><span class="sxs-lookup"><span data-stu-id="89167-231">When a circuit ends because a user has disconnected and the framework is cleaning up the circuit state, the framework disposes of the circuit's DI scope.</span></span> <span data-ttu-id="89167-232">Oddysponowanie zakresu polega na usunięciu wszelkich usług w zakresie innych firm, które implementują <xref:System.IDisposable?displayProperty=fullName>.</span><span class="sxs-lookup"><span data-stu-id="89167-232">Disposing the scope disposes any circuit-scoped DI services that implement <xref:System.IDisposable?displayProperty=fullName>.</span></span> <span data-ttu-id="89167-233">Jeśli jakakolwiek usługa nie zgłasza nieobsłużonego wyjątku podczas usuwania, struktura rejestruje wyjątek.</span><span class="sxs-lookup"><span data-stu-id="89167-233">If any DI service throws an unhandled exception during disposal, the framework logs the exception.</span></span>

### <a name="prerendering"></a><span data-ttu-id="89167-234">Renderowanie prerenderingu</span><span class="sxs-lookup"><span data-stu-id="89167-234">Prerendering</span></span>

::: moniker range=">= aspnetcore-3.1"

<span data-ttu-id="89167-235">składniki Blazor mogą być wstępnie renderowane przy użyciu pomocnika tagów `Component`, tak że renderowane znaczniki HTML są zwracane jako część początkowego żądania HTTP użytkownika.</span><span class="sxs-lookup"><span data-stu-id="89167-235">Blazor components can be prerendered using the `Component` Tag Helper so that their rendered HTML markup is returned as part of the user's initial HTTP request.</span></span> <span data-ttu-id="89167-236">Działa to w następujący sposób:</span><span class="sxs-lookup"><span data-stu-id="89167-236">This works by:</span></span>

* <span data-ttu-id="89167-237">Tworzenie nowego obwodu dla wszystkich wstępnie renderowanych składników, które są częścią tej samej strony.</span><span class="sxs-lookup"><span data-stu-id="89167-237">Creating a new circuit for all of the prerendered components that are part of the same page.</span></span>
* <span data-ttu-id="89167-238">Generowanie początkowego kodu HTML.</span><span class="sxs-lookup"><span data-stu-id="89167-238">Generating the initial HTML.</span></span>
* <span data-ttu-id="89167-239">Podtraktowanie obwodu jako `disconnected`, dopóki przeglądarka użytkownika nie ustanowi połączenia SignalR z powrotem do tego samego serwera.</span><span class="sxs-lookup"><span data-stu-id="89167-239">Treating the circuit as `disconnected` until the user's browser establishes a SignalR connection back to the same server.</span></span> <span data-ttu-id="89167-240">Po nawiązaniu połączenia zostanie wznowione działanie międzydziałające w obwodzie i zostanie zaktualizowane oznaczenie HTML składników.</span><span class="sxs-lookup"><span data-stu-id="89167-240">When the connection is established, interactivity on the circuit is resumed and the components' HTML markup is updated.</span></span>

<span data-ttu-id="89167-241">Jeśli jakikolwiek składnik zgłasza nieobsłużony wyjątek podczas renderowania pre, na przykład podczas wykonywania metody cyklu życia lub logiki renderowania:</span><span class="sxs-lookup"><span data-stu-id="89167-241">If any component throws an unhandled exception during prerendering, for example, during a lifecycle method or in rendering logic:</span></span>

* <span data-ttu-id="89167-242">Wyjątek jest krytyczny dla obwodu.</span><span class="sxs-lookup"><span data-stu-id="89167-242">The exception is fatal to the circuit.</span></span>
* <span data-ttu-id="89167-243">Wyjątek jest generowany w stosie wywołań z pomocnika tagów `Component`.</span><span class="sxs-lookup"><span data-stu-id="89167-243">The exception is thrown up the call stack from the `Component` Tag Helper.</span></span> <span data-ttu-id="89167-244">W związku z tym całe żądanie HTTP kończy się niepowodzeniem, chyba że wyjątek jest jawnie przechwycony przez kod dewelopera.</span><span class="sxs-lookup"><span data-stu-id="89167-244">Therefore, the entire HTTP request fails unless the exception is explicitly caught by developer code.</span></span>

<span data-ttu-id="89167-245">W normalnych warunkach w przypadku niepowodzenia wstępnego renderowania kontynuowanie kompilowania i renderowania składnika nie ma sensu, ponieważ nie można renderować składnika roboczego.</span><span class="sxs-lookup"><span data-stu-id="89167-245">Under normal circumstances when prerendering fails, continuing to build and render the component doesn't make sense because a working component can't be rendered.</span></span>

<span data-ttu-id="89167-246">Aby tolerować błędy, które mogą wystąpić podczas renderowania prerenderingu, logika obsługi błędów musi być umieszczona wewnątrz składnika, który może zgłaszać wyjątki.</span><span class="sxs-lookup"><span data-stu-id="89167-246">To tolerate errors that may occur during prerendering, error handling logic must be placed inside a component that may throw exceptions.</span></span> <span data-ttu-id="89167-247">Używaj instrukcji [try-catch](/dotnet/csharp/language-reference/keywords/try-catch) z obsługą błędów i rejestrowaniem.</span><span class="sxs-lookup"><span data-stu-id="89167-247">Use [try-catch](/dotnet/csharp/language-reference/keywords/try-catch) statements with error handling and logging.</span></span> <span data-ttu-id="89167-248">Zamiast zawijania pomocnika tagów `Component` w instrukcji `try-catch`, umieść logikę obsługi błędów w składniku renderowanym przez pomocnika tagów `Component`.</span><span class="sxs-lookup"><span data-stu-id="89167-248">Instead of wrapping the `Component` Tag Helper in a `try-catch` statement, place error handling logic in the component rendered by the `Component` Tag Helper.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.1"

<span data-ttu-id="89167-249">składniki Blazor mogą być wstępnie renderowane przy użyciu `Html.RenderComponentAsync`, dzięki czemu renderowane znaczniki HTML są zwracane jako część początkowego żądania HTTP użytkownika.</span><span class="sxs-lookup"><span data-stu-id="89167-249">Blazor components can be prerendered using `Html.RenderComponentAsync` so that their rendered HTML markup is returned as part of the user's initial HTTP request.</span></span> <span data-ttu-id="89167-250">Działa to w następujący sposób:</span><span class="sxs-lookup"><span data-stu-id="89167-250">This works by:</span></span>

* <span data-ttu-id="89167-251">Tworzenie nowego obwodu dla wszystkich wstępnie renderowanych składników, które są częścią tej samej strony.</span><span class="sxs-lookup"><span data-stu-id="89167-251">Creating a new circuit for all of the prerendered components that are part of the same page.</span></span>
* <span data-ttu-id="89167-252">Generowanie początkowego kodu HTML.</span><span class="sxs-lookup"><span data-stu-id="89167-252">Generating the initial HTML.</span></span>
* <span data-ttu-id="89167-253">Podtraktowanie obwodu jako `disconnected`, dopóki przeglądarka użytkownika nie ustanowi połączenia SignalR z powrotem do tego samego serwera.</span><span class="sxs-lookup"><span data-stu-id="89167-253">Treating the circuit as `disconnected` until the user's browser establishes a SignalR connection back to the same server.</span></span> <span data-ttu-id="89167-254">Po nawiązaniu połączenia zostanie wznowione działanie międzydziałające w obwodzie i zostanie zaktualizowane oznaczenie HTML składników.</span><span class="sxs-lookup"><span data-stu-id="89167-254">When the connection is established, interactivity on the circuit is resumed and the components' HTML markup is updated.</span></span>

<span data-ttu-id="89167-255">Jeśli jakikolwiek składnik zgłasza nieobsłużony wyjątek podczas renderowania pre, na przykład podczas wykonywania metody cyklu życia lub logiki renderowania:</span><span class="sxs-lookup"><span data-stu-id="89167-255">If any component throws an unhandled exception during prerendering, for example, during a lifecycle method or in rendering logic:</span></span>

* <span data-ttu-id="89167-256">Wyjątek jest krytyczny dla obwodu.</span><span class="sxs-lookup"><span data-stu-id="89167-256">The exception is fatal to the circuit.</span></span>
* <span data-ttu-id="89167-257">Wyjątek jest generowany przez stos wywołań z wywołania `Html.RenderComponentAsync`.</span><span class="sxs-lookup"><span data-stu-id="89167-257">The exception is thrown up the call stack from the `Html.RenderComponentAsync` call.</span></span> <span data-ttu-id="89167-258">W związku z tym całe żądanie HTTP kończy się niepowodzeniem, chyba że wyjątek jest jawnie przechwycony przez kod dewelopera.</span><span class="sxs-lookup"><span data-stu-id="89167-258">Therefore, the entire HTTP request fails unless the exception is explicitly caught by developer code.</span></span>

<span data-ttu-id="89167-259">W normalnych warunkach w przypadku niepowodzenia wstępnego renderowania kontynuowanie kompilowania i renderowania składnika nie ma sensu, ponieważ nie można renderować składnika roboczego.</span><span class="sxs-lookup"><span data-stu-id="89167-259">Under normal circumstances when prerendering fails, continuing to build and render the component doesn't make sense because a working component can't be rendered.</span></span>

<span data-ttu-id="89167-260">Aby tolerować błędy, które mogą wystąpić podczas renderowania prerenderingu, logika obsługi błędów musi być umieszczona wewnątrz składnika, który może zgłaszać wyjątki.</span><span class="sxs-lookup"><span data-stu-id="89167-260">To tolerate errors that may occur during prerendering, error handling logic must be placed inside a component that may throw exceptions.</span></span> <span data-ttu-id="89167-261">Używaj instrukcji [try-catch](/dotnet/csharp/language-reference/keywords/try-catch) z obsługą błędów i rejestrowaniem.</span><span class="sxs-lookup"><span data-stu-id="89167-261">Use [try-catch](/dotnet/csharp/language-reference/keywords/try-catch) statements with error handling and logging.</span></span> <span data-ttu-id="89167-262">Zamiast zawijać wywołanie do `RenderComponentAsync` w instrukcji `try-catch`, umieść logikę obsługi błędów w składniku renderowanym przez `RenderComponentAsync`.</span><span class="sxs-lookup"><span data-stu-id="89167-262">Instead of wrapping the call to `RenderComponentAsync` in a `try-catch` statement, place error handling logic in the component rendered by `RenderComponentAsync`.</span></span>

::: moniker-end

## <a name="advanced-scenarios"></a><span data-ttu-id="89167-263">Scenariusze zaawansowane</span><span class="sxs-lookup"><span data-stu-id="89167-263">Advanced scenarios</span></span>

### <a name="recursive-rendering"></a><span data-ttu-id="89167-264">Renderowanie cykliczne</span><span class="sxs-lookup"><span data-stu-id="89167-264">Recursive rendering</span></span>

<span data-ttu-id="89167-265">Składniki można cyklicznie zagnieżdżać.</span><span class="sxs-lookup"><span data-stu-id="89167-265">Components can be nested recursively.</span></span> <span data-ttu-id="89167-266">Jest to przydatne do reprezentowania struktur danych rekursywnych.</span><span class="sxs-lookup"><span data-stu-id="89167-266">This is useful for representing recursive data structures.</span></span> <span data-ttu-id="89167-267">Na przykład składnik `TreeNode` może renderować więcej składników `TreeNode` dla każdego elementu podrzędnego węzła.</span><span class="sxs-lookup"><span data-stu-id="89167-267">For example, a `TreeNode` component can render more `TreeNode` components for each of the node's children.</span></span>

<span data-ttu-id="89167-268">W przypadku renderowania cyklicznego należy unikać tworzenia wzorców, które powodują nieskończoną rekursję:</span><span class="sxs-lookup"><span data-stu-id="89167-268">When rendering recursively, avoid coding patterns that result in infinite recursion:</span></span>

* <span data-ttu-id="89167-269">Nie Renderuj rekursywnie struktury danych zawierającej cykl.</span><span class="sxs-lookup"><span data-stu-id="89167-269">Don't recursively render a data structure that contains a cycle.</span></span> <span data-ttu-id="89167-270">Na przykład nie Renderuj węzła drzewa, którego elementy podrzędne zawierają sam siebie.</span><span class="sxs-lookup"><span data-stu-id="89167-270">For example, don't render a tree node whose children includes itself.</span></span>
* <span data-ttu-id="89167-271">Nie twórz łańcucha układów zawierających cykl.</span><span class="sxs-lookup"><span data-stu-id="89167-271">Don't create a chain of layouts that contain a cycle.</span></span> <span data-ttu-id="89167-272">Na przykład nie twórz układu, którego układ jest sam.</span><span class="sxs-lookup"><span data-stu-id="89167-272">For example, don't create a layout whose layout is itself.</span></span>
* <span data-ttu-id="89167-273">Nie Zezwalaj użytkownikowi końcowemu na naruszenie nieodmian rekursji (reguł) przy użyciu złośliwego wpisu danych lub wywołań międzyoperacyjnych języka JavaScript.</span><span class="sxs-lookup"><span data-stu-id="89167-273">Don't allow an end user to violate recursion invariants (rules) through malicious data entry or JavaScript interop calls.</span></span>

<span data-ttu-id="89167-274">Nieskończone pętle podczas renderowania:</span><span class="sxs-lookup"><span data-stu-id="89167-274">Infinite loops during rendering:</span></span>

* <span data-ttu-id="89167-275">Powoduje, że proces renderowania kontynuuje działanie zawsze.</span><span class="sxs-lookup"><span data-stu-id="89167-275">Causes the rendering process to continue forever.</span></span>
* <span data-ttu-id="89167-276">Jest równoznaczny z tworzeniem niezakończonej pętli.</span><span class="sxs-lookup"><span data-stu-id="89167-276">Is equivalent to creating an unterminated loop.</span></span>

<span data-ttu-id="89167-277">W tych scenariuszach obwód, którego to dotyczy, zawiesza się, a wątek zwykle próbuje wykonać:</span><span class="sxs-lookup"><span data-stu-id="89167-277">In these scenarios, the affected circuit hangs, and the thread usually attempts to:</span></span>

* <span data-ttu-id="89167-278">Zużywaj ilość czasu procesora CPU dozwoloną przez system operacyjny w nieskończoność.</span><span class="sxs-lookup"><span data-stu-id="89167-278">Consume as much CPU time as permitted by the operating system, indefinitely.</span></span>
* <span data-ttu-id="89167-279">Korzystaj z nieograniczonej ilości pamięci serwera.</span><span class="sxs-lookup"><span data-stu-id="89167-279">Consume an unlimited amount of server memory.</span></span> <span data-ttu-id="89167-280">Zużywanie nieograniczonej pamięci jest równoważne scenariuszowi, w którym niezakończona pętla dodaje wpisy do kolekcji na każdej iteracji.</span><span class="sxs-lookup"><span data-stu-id="89167-280">Consuming unlimited memory is equivalent to the scenario where an unterminated loop adds entries to a collection on every iteration.</span></span>

<span data-ttu-id="89167-281">Aby uniknąć nieskończonych wzorców rekursji, należy się upewnić, że kod renderowania cyklicznego zawiera odpowiednie warunki zatrzymania.</span><span class="sxs-lookup"><span data-stu-id="89167-281">To avoid infinite recursion patterns, ensure that recursive rendering code contains suitable stopping conditions.</span></span>

### <a name="custom-render-tree-logic"></a><span data-ttu-id="89167-282">Logika drzewa renderowania niestandardowego</span><span class="sxs-lookup"><span data-stu-id="89167-282">Custom render tree logic</span></span>

<span data-ttu-id="89167-283">Większość Blazor składników jest implementowana jako pliki *Razor* i są kompilowane do tworzenia logiki, która działa na `RenderTreeBuilder` w celu renderowania danych wyjściowych.</span><span class="sxs-lookup"><span data-stu-id="89167-283">Most Blazor components are implemented as *.razor* files and are compiled to produce logic that operates on a `RenderTreeBuilder` to render their output.</span></span> <span data-ttu-id="89167-284">Deweloper może ręcznie zaimplementować logikę `RenderTreeBuilder` przy użyciu C# kodu proceduralnego.</span><span class="sxs-lookup"><span data-stu-id="89167-284">A developer may manually implement `RenderTreeBuilder` logic using procedural C# code.</span></span> <span data-ttu-id="89167-285">Aby uzyskać więcej informacji, zobacz <xref:blazor/components#manual-rendertreebuilder-logic>.</span><span class="sxs-lookup"><span data-stu-id="89167-285">For more information, see <xref:blazor/components#manual-rendertreebuilder-logic>.</span></span>

> [!WARNING]
> <span data-ttu-id="89167-286">Korzystanie z logiki konstruktora drzewa renderowania ręcznego jest uznawane za zaawansowane i niebezpieczne scenariusze, które nie są zalecane do ogólnego tworzenia składników.</span><span class="sxs-lookup"><span data-stu-id="89167-286">Use of manual render tree builder logic is considered an advanced and unsafe scenario, not recommended for general component development.</span></span>

<span data-ttu-id="89167-287">Jeśli `RenderTreeBuilder` kod zostanie zapisany, Deweloper musi zagwarantować poprawność kodu.</span><span class="sxs-lookup"><span data-stu-id="89167-287">If `RenderTreeBuilder` code is written, the developer must guarantee the correctness of the code.</span></span> <span data-ttu-id="89167-288">Na przykład Deweloper musi upewnić się, że:</span><span class="sxs-lookup"><span data-stu-id="89167-288">For example, the developer must ensure that:</span></span>

* <span data-ttu-id="89167-289">Wywołania `OpenElement` i `CloseElement` są prawidłowo zrównoważone.</span><span class="sxs-lookup"><span data-stu-id="89167-289">Calls to `OpenElement` and `CloseElement` are correctly balanced.</span></span>
* <span data-ttu-id="89167-290">Atrybuty są dodawane tylko w prawidłowych miejscach.</span><span class="sxs-lookup"><span data-stu-id="89167-290">Attributes are only added in the correct places.</span></span>

<span data-ttu-id="89167-291">Nieprawidłowa ręczna logika konstruktora drzewa renderowania może spowodować dowolne niezdefiniowane zachowanie, w tym awarie, zawieszenie serwera i luki w zabezpieczeniach.</span><span class="sxs-lookup"><span data-stu-id="89167-291">Incorrect manual render tree builder logic can cause arbitrary undefined behavior, including crashes, server hangs, and security vulnerabilities.</span></span>

<span data-ttu-id="89167-292">Rozważ ręczne renderowanie logiki konstruktora drzew drzewa na tym samym poziomie złożoności i z tym samym poziomem *zagrożenia* co ręczne pisanie kodu zestawu lub instrukcji MSIL.</span><span class="sxs-lookup"><span data-stu-id="89167-292">Consider manual render tree builder logic on the same level of complexity and with the same level of *danger* as writing assembly code or MSIL instructions by hand.</span></span>
