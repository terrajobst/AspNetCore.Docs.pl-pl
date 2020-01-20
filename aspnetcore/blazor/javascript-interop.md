---
title: ASP.NET Core Blazor JavaScript Interop
author: guardrex
description: Dowiedz się, jak wywoływać funkcje języka JavaScript z technologii .NET i .NET z poziomu języka JavaScript w aplikacjach Blazor.
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 12/18/2019
no-loc:
- Blazor
- SignalR
uid: blazor/javascript-interop
ms.openlocfilehash: e1b9c84dace193768c6f3fbb5636ef675d65a20d
ms.sourcegitcommit: 9ee99300a48c810ca6fd4f7700cd95c3ccb85972
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/17/2020
ms.locfileid: "76159908"
---
# <a name="aspnet-core-opno-locblazor-javascript-interop"></a><span data-ttu-id="e7e7f-103">ASP.NET Core Blazor JavaScript Interop</span><span class="sxs-lookup"><span data-stu-id="e7e7f-103">ASP.NET Core Blazor JavaScript interop</span></span>

<span data-ttu-id="e7e7f-104">[Javier Calvarro Nelson](https://github.com/javiercn), [Daniel Roth](https://github.com/danroth27)i [Luke](https://github.com/guardrex) Latham</span><span class="sxs-lookup"><span data-stu-id="e7e7f-104">By [Javier Calvarro Nelson](https://github.com/javiercn), [Daniel Roth](https://github.com/danroth27), and [Luke Latham](https://github.com/guardrex)</span></span>

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

<span data-ttu-id="e7e7f-105">Aplikacja Blazor może wywoływać funkcje języka JavaScript z technologii .NET i .NET z kodu JavaScript.</span><span class="sxs-lookup"><span data-stu-id="e7e7f-105">A Blazor app can invoke JavaScript functions from .NET and .NET methods from JavaScript code.</span></span>

<span data-ttu-id="e7e7f-106">[Wyświetl lub pobierz przykładowy kod](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/blazor/common/samples/) ([jak pobrać](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="e7e7f-106">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/blazor/common/samples/) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="invoke-javascript-functions-from-net-methods"></a><span data-ttu-id="e7e7f-107">Wywoływanie funkcji języka JavaScript z metod .NET</span><span class="sxs-lookup"><span data-stu-id="e7e7f-107">Invoke JavaScript functions from .NET methods</span></span>

<span data-ttu-id="e7e7f-108">Istnieją przypadki, w których kod .NET jest wymagany do wywołania funkcji języka JavaScript.</span><span class="sxs-lookup"><span data-stu-id="e7e7f-108">There are times when .NET code is required to call a JavaScript function.</span></span> <span data-ttu-id="e7e7f-109">Na przykład wywołanie języka JavaScript może uwidaczniać możliwości przeglądarki lub funkcje z biblioteki JavaScript w aplikacji.</span><span class="sxs-lookup"><span data-stu-id="e7e7f-109">For example, a JavaScript call can expose browser capabilities or functionality from a JavaScript library to the app.</span></span> <span data-ttu-id="e7e7f-110">Ten scenariusz jest nazywany *współdziałaniem języka JavaScript* (w programie*js Interop*).</span><span class="sxs-lookup"><span data-stu-id="e7e7f-110">This scenario is called *JavaScript interoperability* (*JS interop*).</span></span>

<span data-ttu-id="e7e7f-111">Aby wywołać kod JavaScript z platformy .NET, użyj abstrakcji `IJSRuntime`.</span><span class="sxs-lookup"><span data-stu-id="e7e7f-111">To call into JavaScript from .NET, use the `IJSRuntime` abstraction.</span></span> <span data-ttu-id="e7e7f-112">Aby wystawić wywołania w programie JS Interop, wstrzyknąć `IJSRuntime` abstrakcję w składniku.</span><span class="sxs-lookup"><span data-stu-id="e7e7f-112">To issue JS interop calls, inject the `IJSRuntime` abstraction in your component.</span></span> <span data-ttu-id="e7e7f-113">Metoda `InvokeAsync<T>` przyjmuje identyfikator funkcji języka JavaScript, która ma zostać wywołana wraz z dowolną liczbą argumentów z możliwością serializacji JSON.</span><span class="sxs-lookup"><span data-stu-id="e7e7f-113">The `InvokeAsync<T>` method takes an identifier for the JavaScript function that you wish to invoke along with any number of JSON-serializable arguments.</span></span> <span data-ttu-id="e7e7f-114">Identyfikator funkcji jest względny w stosunku do zakresu globalnego (`window`).</span><span class="sxs-lookup"><span data-stu-id="e7e7f-114">The function identifier is relative to the global scope (`window`).</span></span> <span data-ttu-id="e7e7f-115">Jeśli chcesz wywołać `window.someScope.someFunction`, identyfikator jest `someScope.someFunction`.</span><span class="sxs-lookup"><span data-stu-id="e7e7f-115">If you wish to call `window.someScope.someFunction`, the identifier is `someScope.someFunction`.</span></span> <span data-ttu-id="e7e7f-116">Nie ma potrzeby rejestrowania funkcji przed jej wywołaniem.</span><span class="sxs-lookup"><span data-stu-id="e7e7f-116">There's no need to register the function before it's called.</span></span> <span data-ttu-id="e7e7f-117">Typ zwracany `T` musi również być możliwy do serializacji kod JSON.</span><span class="sxs-lookup"><span data-stu-id="e7e7f-117">The return type `T` must also be JSON serializable.</span></span> <span data-ttu-id="e7e7f-118">`T` powinna być zgodna z typem .NET, który najlepiej jest mapowany do zwracanego typu JSON.</span><span class="sxs-lookup"><span data-stu-id="e7e7f-118">`T` should match the .NET type that best maps to the JSON type returned.</span></span>

<span data-ttu-id="e7e7f-119">W przypadku aplikacji Blazor Server z włączoną obsługą prerenderowania Wywoływanie kodu JavaScript nie jest możliwe podczas początkowego wstępnego renderowania.</span><span class="sxs-lookup"><span data-stu-id="e7e7f-119">For Blazor Server apps with prerendering enabled, calling into JavaScript isn't possible during the initial prerendering.</span></span> <span data-ttu-id="e7e7f-120">Wywołania międzyoperacyjne języka JavaScript muszą zostać odroczone do momentu ustanowienia połączenia z przeglądarką.</span><span class="sxs-lookup"><span data-stu-id="e7e7f-120">JavaScript interop calls must be deferred until after the connection with the browser is established.</span></span> <span data-ttu-id="e7e7f-121">Aby uzyskać więcej informacji, zobacz sekcję [wykrywanie, kiedy aplikacja Blazor jest renderowana](#detect-when-a-blazor-app-is-prerendering) .</span><span class="sxs-lookup"><span data-stu-id="e7e7f-121">For more information, see the [Detect when a Blazor app is prerendering](#detect-when-a-blazor-app-is-prerendering) section.</span></span>

<span data-ttu-id="e7e7f-122">Poniższy przykład jest oparty na [dekoderze](https://developer.mozilla.org/docs/Web/API/TextDecoder), eksperymentalnym dekoderem JavaScript.</span><span class="sxs-lookup"><span data-stu-id="e7e7f-122">The following example is based on [TextDecoder](https://developer.mozilla.org/docs/Web/API/TextDecoder), an experimental JavaScript-based decoder.</span></span> <span data-ttu-id="e7e7f-123">W przykładzie pokazano, jak wywołać funkcję JavaScript z C# metody.</span><span class="sxs-lookup"><span data-stu-id="e7e7f-123">The example demonstrates how to invoke a JavaScript function from a C# method.</span></span> <span data-ttu-id="e7e7f-124">Funkcja JavaScript akceptuje tablicę bajtową z C# metody, dekoduje tablicę i zwraca tekst do składnika do wyświetlenia.</span><span class="sxs-lookup"><span data-stu-id="e7e7f-124">The JavaScript function accepts a byte array from a C# method, decodes the array, and returns the text to the component for display.</span></span>

<span data-ttu-id="e7e7f-125">Wewnątrz elementu `<head>` *wwwroot/index.html* (Blazor webassembly) lub *pages/_Host. cshtml* (Blazor Server) podaj funkcję języka JavaScript, która używa `TextDecoder` do zdekodowania przekazaną tablicę i zwrócenie zdekodowanej wartości:</span><span class="sxs-lookup"><span data-stu-id="e7e7f-125">Inside the `<head>` element of *wwwroot/index.html* (Blazor WebAssembly) or *Pages/_Host.cshtml* (Blazor Server), provide a JavaScript function that uses `TextDecoder` to decode a passed array and return the decoded value:</span></span>

[!code-html[](javascript-interop/samples_snapshot/index-script-convertarray.html)]

<span data-ttu-id="e7e7f-126">Kod JavaScript, taki jak kod przedstawiony w powyższym przykładzie, można również załadować z pliku JavaScript ( *. js*) z odwołaniem do pliku skryptu:</span><span class="sxs-lookup"><span data-stu-id="e7e7f-126">JavaScript code, such as the code shown in the preceding example, can also be loaded from a JavaScript file (*.js*) with a reference to the script file:</span></span>

```html
<script src="exampleJsInterop.js"></script>
```

<span data-ttu-id="e7e7f-127">Następujący składnik:</span><span class="sxs-lookup"><span data-stu-id="e7e7f-127">The following component:</span></span>

* <span data-ttu-id="e7e7f-128">Wywołuje funkcję `convertArray` JavaScript przy użyciu `JSRuntime`, gdy zostanie wybrany przycisk składnika (**Konwertuj tablicę**).</span><span class="sxs-lookup"><span data-stu-id="e7e7f-128">Invokes the `convertArray` JavaScript function using `JSRuntime` when a component button (**Convert Array**) is selected.</span></span>
* <span data-ttu-id="e7e7f-129">Po wywołaniu funkcji języka JavaScript przenoszona tablica jest konwertowana na ciąg.</span><span class="sxs-lookup"><span data-stu-id="e7e7f-129">After the JavaScript function is called, the passed array is converted into a string.</span></span> <span data-ttu-id="e7e7f-130">Ciąg jest zwracany do składnika do wyświetlenia.</span><span class="sxs-lookup"><span data-stu-id="e7e7f-130">The string is returned to the component for display.</span></span>

[!code-razor[](javascript-interop/samples_snapshot/call-js-example.razor?highlight=2,34-35)]

## <a name="use-of-ijsruntime"></a><span data-ttu-id="e7e7f-131">Korzystanie z IJSRuntime</span><span class="sxs-lookup"><span data-stu-id="e7e7f-131">Use of IJSRuntime</span></span>

<span data-ttu-id="e7e7f-132">Aby użyć abstrakcji `IJSRuntime`, należy zastosować jedną z następujących metod:</span><span class="sxs-lookup"><span data-stu-id="e7e7f-132">To use the `IJSRuntime` abstraction, adopt any of the following approaches:</span></span>

* <span data-ttu-id="e7e7f-133">Wsuń `IJSRuntime` abstrakcję do składnika Razor ( *. Razor*):</span><span class="sxs-lookup"><span data-stu-id="e7e7f-133">Inject the `IJSRuntime` abstraction into the Razor component (*.razor*):</span></span>

  [!code-razor[](javascript-interop/samples_snapshot/inject-abstraction.razor?highlight=1)]

  <span data-ttu-id="e7e7f-134">Wewnątrz elementu `<head>` *wwwroot/index.html* (Blazor webassembly) lub *pages/_Host. cshtml* (Blazor Server) podaj funkcję JavaScript `handleTickerChanged`.</span><span class="sxs-lookup"><span data-stu-id="e7e7f-134">Inside the `<head>` element of *wwwroot/index.html* (Blazor WebAssembly) or *Pages/_Host.cshtml* (Blazor Server), provide a `handleTickerChanged` JavaScript function.</span></span> <span data-ttu-id="e7e7f-135">Funkcja jest wywoływana z `IJSRuntime.InvokeVoidAsync` i nie zwraca wartości:</span><span class="sxs-lookup"><span data-stu-id="e7e7f-135">The function is called with `IJSRuntime.InvokeVoidAsync` and doesn't return a value:</span></span>

  [!code-html[](javascript-interop/samples_snapshot/index-script-handleTickerChanged1.html)]

* <span data-ttu-id="e7e7f-136">Wstrzyknąć `IJSRuntime` abstrakcję do klasy ( *. cs*):</span><span class="sxs-lookup"><span data-stu-id="e7e7f-136">Inject the `IJSRuntime` abstraction into a class (*.cs*):</span></span>

  [!code-csharp[](javascript-interop/samples_snapshot/inject-abstraction-class.cs?highlight=5)]

  <span data-ttu-id="e7e7f-137">Wewnątrz elementu `<head>` *wwwroot/index.html* (Blazor webassembly) lub *pages/_Host. cshtml* (Blazor Server) podaj funkcję JavaScript `handleTickerChanged`.</span><span class="sxs-lookup"><span data-stu-id="e7e7f-137">Inside the `<head>` element of *wwwroot/index.html* (Blazor WebAssembly) or *Pages/_Host.cshtml* (Blazor Server), provide a `handleTickerChanged` JavaScript function.</span></span> <span data-ttu-id="e7e7f-138">Funkcja jest wywoływana z `JSRuntime.InvokeAsync` i zwraca wartość:</span><span class="sxs-lookup"><span data-stu-id="e7e7f-138">The function is called with `JSRuntime.InvokeAsync` and returns a value:</span></span>

  [!code-html[](javascript-interop/samples_snapshot/index-script-handleTickerChanged2.html)]

* <span data-ttu-id="e7e7f-139">Aby można było wygenerować zawartość dynamiczną przy użyciu [BuildRenderTree](xref:blazor/components#manual-rendertreebuilder-logic), użyj atrybutu `[Inject]`:</span><span class="sxs-lookup"><span data-stu-id="e7e7f-139">For dynamic content generation with [BuildRenderTree](xref:blazor/components#manual-rendertreebuilder-logic), use the `[Inject]` attribute:</span></span>

  ```razor
  [Inject]
  IJSRuntime JSRuntime { get; set; }
  ```

<span data-ttu-id="e7e7f-140">W aplikacji przykładowej po stronie klienta, która jest dołączona do tego tematu, dostępne są dwie funkcje języka JavaScript, które współdziałają z modelem DOM, aby odbierać dane wejściowe użytkownika i wyświetlać komunikat powitalny:</span><span class="sxs-lookup"><span data-stu-id="e7e7f-140">In the client-side sample app that accompanies this topic, two JavaScript functions are available to the app that interact with the DOM to receive user input and display a welcome message:</span></span>

* <span data-ttu-id="e7e7f-141">`showPrompt` &ndash; generuje monit o zaakceptowanie danych wprowadzonych przez użytkownika (nazwę użytkownika) i zwraca nazwę obiektu wywołującego.</span><span class="sxs-lookup"><span data-stu-id="e7e7f-141">`showPrompt` &ndash; Produces a prompt to accept user input (the user's name) and returns the name to the caller.</span></span>
* <span data-ttu-id="e7e7f-142">`displayWelcome` &ndash; przypisuje Komunikat powitalny od wywołującego do obiektu DOM z `id` `welcome`.</span><span class="sxs-lookup"><span data-stu-id="e7e7f-142">`displayWelcome` &ndash; Assigns a welcome message from the caller to a DOM object with an `id` of `welcome`.</span></span>

<span data-ttu-id="e7e7f-143">*wwwroot/exampleJsInterop. js*:</span><span class="sxs-lookup"><span data-stu-id="e7e7f-143">*wwwroot/exampleJsInterop.js*:</span></span>

[!code-javascript[](./common/samples/3.x/BlazorWebAssemblySample/wwwroot/exampleJsInterop.js?highlight=2-7)]

<span data-ttu-id="e7e7f-144">Umieść tag `<script>` odwołujący się do pliku JavaScript w pliku *wwwroot/index.html* (Blazor webassembly) lub *pages/_Host. cshtml* (serwerBlazor).</span><span class="sxs-lookup"><span data-stu-id="e7e7f-144">Place the `<script>` tag that references the JavaScript file in the *wwwroot/index.html* file (Blazor WebAssembly) or *Pages/_Host.cshtml* file (Blazor Server).</span></span>

<span data-ttu-id="e7e7f-145">*wwwroot/index.html* (Blazor webassembly):</span><span class="sxs-lookup"><span data-stu-id="e7e7f-145">*wwwroot/index.html* (Blazor WebAssembly):</span></span>

[!code-html[](./common/samples/3.x/BlazorWebAssemblySample/wwwroot/index.html?highlight=15)]

<span data-ttu-id="e7e7f-146">*Pages/_Host. cshtml* (Blazor Server):</span><span class="sxs-lookup"><span data-stu-id="e7e7f-146">*Pages/_Host.cshtml* (Blazor Server):</span></span>

[!code-cshtml[](./common/samples/3.x/BlazorServerSample/Pages/_Host.cshtml?highlight=21)]

<span data-ttu-id="e7e7f-147">Nie umieszczaj znacznika `<script>` w pliku składnika, ponieważ nie można dynamicznie zaktualizować tagu `<script>`.</span><span class="sxs-lookup"><span data-stu-id="e7e7f-147">Don't place a `<script>` tag in a component file because the `<script>` tag can't be updated dynamically.</span></span>

<span data-ttu-id="e7e7f-148">.NET metod współdziałania z funkcjami JavaScript w pliku *exampleJsInterop. js* przez wywołanie `IJSRuntime.InvokeAsync<T>`.</span><span class="sxs-lookup"><span data-stu-id="e7e7f-148">.NET methods interop with the JavaScript functions in the *exampleJsInterop.js* file by calling `IJSRuntime.InvokeAsync<T>`.</span></span>

<span data-ttu-id="e7e7f-149">Abstrakcja `IJSRuntime` jest asynchroniczna, aby umożliwić obsługę scenariuszy Blazor Server.</span><span class="sxs-lookup"><span data-stu-id="e7e7f-149">The `IJSRuntime` abstraction is asynchronous to allow for Blazor Server scenarios.</span></span> <span data-ttu-id="e7e7f-150">Jeśli aplikacja jest aplikacją Blazor webassembly i chcesz wywołać funkcję JavaScript synchronicznie, downcast do `IJSInProcessRuntime` i Wywołaj `Invoke<T>` zamiast tego.</span><span class="sxs-lookup"><span data-stu-id="e7e7f-150">If the app is a Blazor WebAssembly app and you want to invoke a JavaScript function synchronously, downcast to `IJSInProcessRuntime` and call `Invoke<T>` instead.</span></span> <span data-ttu-id="e7e7f-151">Zalecamy, aby większość bibliotek międzyoperacyjnych JS używała asynchronicznych interfejsów API, aby upewnić się, że biblioteki są dostępne we wszystkich scenariuszach.</span><span class="sxs-lookup"><span data-stu-id="e7e7f-151">We recommend that most JS interop libraries use the async APIs to ensure that the libraries are available in all scenarios.</span></span>

<span data-ttu-id="e7e7f-152">Przykładowa aplikacja zawiera składnik demonstrujący międzyoperacyjność JS.</span><span class="sxs-lookup"><span data-stu-id="e7e7f-152">The sample app includes a component to demonstrate JS interop.</span></span> <span data-ttu-id="e7e7f-153">Składnik:</span><span class="sxs-lookup"><span data-stu-id="e7e7f-153">The component:</span></span>

* <span data-ttu-id="e7e7f-154">Odbiera dane wprowadzane przez użytkownika za pośrednictwem wiersza polecenia języka JavaScript.</span><span class="sxs-lookup"><span data-stu-id="e7e7f-154">Receives user input via a JavaScript prompt.</span></span>
* <span data-ttu-id="e7e7f-155">Zwraca tekst do składnika do przetworzenia.</span><span class="sxs-lookup"><span data-stu-id="e7e7f-155">Returns the text to the component for processing.</span></span>
* <span data-ttu-id="e7e7f-156">Wywołuje drugą funkcję języka JavaScript, która współdziała z modelem DOM, aby wyświetlić komunikat powitalny.</span><span class="sxs-lookup"><span data-stu-id="e7e7f-156">Calls a second JavaScript function that interacts with the DOM to display a welcome message.</span></span>

<span data-ttu-id="e7e7f-157">*Strony/JSInterop. Razor*:</span><span class="sxs-lookup"><span data-stu-id="e7e7f-157">*Pages/JSInterop.razor*:</span></span>

```razor
@page "/JSInterop"
@using BlazorSample.JsInteropClasses
@inject IJSRuntime JSRuntime

<h1>JavaScript Interop</h1>

<h2>Invoke JavaScript functions from .NET methods</h2>

<button type="button" class="btn btn-primary" @onclick="TriggerJsPrompt">
    Trigger JavaScript Prompt
</button>

<h3 id="welcome" style="color:green;font-style:italic"></h3>

@code {
    public async Task TriggerJsPrompt()
    {
        // showPrompt is implemented in wwwroot/exampleJsInterop.js
        var name = await JSRuntime.InvokeAsync<string>(
                "exampleJsFunctions.showPrompt",
                "What's your name?");
        // displayWelcome is implemented in wwwroot/exampleJsInterop.js
        await JSRuntime.InvokeVoidAsync(
                "exampleJsFunctions.displayWelcome",
                $"Hello {name}! Welcome to Blazor!");
    }
}
```

1. <span data-ttu-id="e7e7f-158">Gdy `TriggerJsPrompt` jest wykonywane, wybierając przycisk **Monituj wyzwalacza JavaScript** składnika, funkcja JavaScript `showPrompt` dostępna w pliku *wwwroot/exampleJsInterop. js* jest wywoływana.</span><span class="sxs-lookup"><span data-stu-id="e7e7f-158">When `TriggerJsPrompt` is executed by selecting the component's **Trigger JavaScript Prompt** button, the JavaScript `showPrompt` function provided in the *wwwroot/exampleJsInterop.js* file is called.</span></span>
1. <span data-ttu-id="e7e7f-159">Funkcja `showPrompt` akceptuje dane wejściowe użytkownika (nazwę użytkownika), które są kodowane w formacie HTML i zwracane do składnika.</span><span class="sxs-lookup"><span data-stu-id="e7e7f-159">The `showPrompt` function accepts user input (the user's name), which is HTML-encoded and returned to the component.</span></span> <span data-ttu-id="e7e7f-160">Składnik przechowuje nazwę użytkownika w zmiennej lokalnej, `name`.</span><span class="sxs-lookup"><span data-stu-id="e7e7f-160">The component stores the user's name in a local variable, `name`.</span></span>
1. <span data-ttu-id="e7e7f-161">Ciąg przechowywany w `name` jest zawarty w komunikacie powitalnym, który jest przesyłany do funkcji języka JavaScript, `displayWelcome`, która renderuje Komunikat powitalny do znacznika nagłówka.</span><span class="sxs-lookup"><span data-stu-id="e7e7f-161">The string stored in `name` is incorporated into a welcome message, which is passed to a JavaScript function, `displayWelcome`, which renders the welcome message into a heading tag.</span></span>

## <a name="call-a-void-javascript-function"></a><span data-ttu-id="e7e7f-162">Wywoływanie funkcji języka JavaScript typu void</span><span class="sxs-lookup"><span data-stu-id="e7e7f-162">Call a void JavaScript function</span></span>

<span data-ttu-id="e7e7f-163">Funkcje języka JavaScript zwracające [wartość void (0)/void 0](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Operators/void) lub [undefined](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/undefined) są wywoływane z `IJSRuntime.InvokeVoidAsync`.</span><span class="sxs-lookup"><span data-stu-id="e7e7f-163">JavaScript functions that return [void(0)/void 0](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Operators/void) or [undefined](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/undefined) are called with `IJSRuntime.InvokeVoidAsync`.</span></span>

## <a name="detect-when-a-opno-locblazor-app-is-prerendering"></a><span data-ttu-id="e7e7f-164">Wykryj, kiedy aplikacja Blazora jest renderowana</span><span class="sxs-lookup"><span data-stu-id="e7e7f-164">Detect when a Blazor app is prerendering</span></span>
 
[!INCLUDE[](~/includes/blazor-prerendering.md)]

## <a name="capture-references-to-elements"></a><span data-ttu-id="e7e7f-165">Przechwyć odwołania do elementów</span><span class="sxs-lookup"><span data-stu-id="e7e7f-165">Capture references to elements</span></span>

<span data-ttu-id="e7e7f-166">Niektóre scenariusze międzyoperacyjności JS wymagają odwołań do elementów HTML.</span><span class="sxs-lookup"><span data-stu-id="e7e7f-166">Some JS interop scenarios require references to HTML elements.</span></span> <span data-ttu-id="e7e7f-167">Na przykład Biblioteka interfejsu użytkownika może wymagać odwołania do elementu dla inicjalizacji lub może być konieczne wywoływanie interfejsów API przypominających polecenia na elemencie, takich jak `focus` lub `play`.</span><span class="sxs-lookup"><span data-stu-id="e7e7f-167">For example, a UI library may require an element reference for initialization, or you might need to call command-like APIs on an element, such as `focus` or `play`.</span></span>

<span data-ttu-id="e7e7f-168">Przechwyć odwołania do elementów HTML w składniku, korzystając z następującej metody:</span><span class="sxs-lookup"><span data-stu-id="e7e7f-168">Capture references to HTML elements in a component using the following approach:</span></span>

* <span data-ttu-id="e7e7f-169">Dodaj atrybut `@ref` do elementu HTML.</span><span class="sxs-lookup"><span data-stu-id="e7e7f-169">Add an `@ref` attribute to the HTML element.</span></span>
* <span data-ttu-id="e7e7f-170">Zdefiniuj pole typu `ElementReference`, którego nazwa pasuje do wartości atrybutu `@ref`.</span><span class="sxs-lookup"><span data-stu-id="e7e7f-170">Define a field of type `ElementReference` whose name matches the value of the `@ref` attribute.</span></span>

<span data-ttu-id="e7e7f-171">W poniższym przykładzie pokazano przechwytywanie odwołania do `username` elementu `<input>`:</span><span class="sxs-lookup"><span data-stu-id="e7e7f-171">The following example shows capturing a reference to the `username` `<input>` element:</span></span>

```razor
<input @ref="username" ... />

@code {
    ElementReference username;
}
```

> [!WARNING]
> <span data-ttu-id="e7e7f-172">Użyj odwołania do elementu, aby zmodyfikować zawartość pustego elementu, który nie współdziała z Blazor.</span><span class="sxs-lookup"><span data-stu-id="e7e7f-172">Only use an element reference to mutate the contents of an empty element that doesn't interact with Blazor.</span></span> <span data-ttu-id="e7e7f-173">Ten scenariusz jest przydatny, gdy interfejs API innej firmy dostarcza zawartość do elementu.</span><span class="sxs-lookup"><span data-stu-id="e7e7f-173">This scenario is useful when a 3rd party API supplies content to the element.</span></span> <span data-ttu-id="e7e7f-174">Ponieważ Blazor nie współdziała z elementem, nie ma możliwości konfliktu między reprezentacją elementu a obiektem DOM przez Blazor.</span><span class="sxs-lookup"><span data-stu-id="e7e7f-174">Because Blazor doesn't interact with the element, there's no possibility of a conflict between Blazor's representation of the element and the DOM.</span></span>
>
> <span data-ttu-id="e7e7f-175">W poniższym przykładzie jest *niebezpieczne* do mutacji zawartości listy nieuporządkowanej (`ul`), ponieważ Blazor współdziała z modelem dom w celu wypełnienia elementów listy elementu (`<li>`):</span><span class="sxs-lookup"><span data-stu-id="e7e7f-175">In the following example, it's *dangerous* to mutate the contents of the unordered list (`ul`) because Blazor interacts with the DOM to populate this element's list items (`<li>`):</span></span>
>
> ```razor
> <ul ref="MyList">
>     @foreach (var item in Todos)
>     {
>         <li>@item.Text</li>
>     }
> </ul>
> ```
>
> <span data-ttu-id="e7e7f-176">Jeśli program JS Interop przyniesie zawartość elementu `MyList` i Blazor próbuje zastosować różnic do elementu, różnice nie będą zgodne z modelem DOM.</span><span class="sxs-lookup"><span data-stu-id="e7e7f-176">If JS interop mutates the contents of element `MyList` and Blazor attempts to apply diffs to the element, the diffs won't match the DOM.</span></span>

<span data-ttu-id="e7e7f-177">W odniesieniu do kodu platformy .NET `ElementReference` jest nieprzezroczystym uchwytem.</span><span class="sxs-lookup"><span data-stu-id="e7e7f-177">As far as .NET code is concerned, an `ElementReference` is an opaque handle.</span></span> <span data-ttu-id="e7e7f-178">*Jedyną* czynnością, którą można wykonać za pomocą `ElementReference`, jest przekazanie jej do kodu JavaScript za pośrednictwem międzyoperacyjnego js.</span><span class="sxs-lookup"><span data-stu-id="e7e7f-178">The *only* thing you can do with `ElementReference` is pass it through to JavaScript code via JS interop.</span></span> <span data-ttu-id="e7e7f-179">Gdy to zrobisz, kod po stronie JavaScript odbiera wystąpienie `HTMLElement`, które może być używane z normalnymi interfejsami API modelu DOM.</span><span class="sxs-lookup"><span data-stu-id="e7e7f-179">When you do so, the JavaScript-side code receives an `HTMLElement` instance, which it can use with normal DOM APIs.</span></span>

<span data-ttu-id="e7e7f-180">Na przykład poniższy kod definiuje metodę rozszerzenia .NET, która umożliwia ustawienie fokusu na elemencie:</span><span class="sxs-lookup"><span data-stu-id="e7e7f-180">For example, the following code defines a .NET extension method that enables setting the focus on an element:</span></span>

<span data-ttu-id="e7e7f-181">*exampleJsInterop.js*:</span><span class="sxs-lookup"><span data-stu-id="e7e7f-181">*exampleJsInterop.js*:</span></span>

```javascript
window.exampleJsFunctions = {
  focusElement : function (element) {
    element.focus();
  }
}
```

<span data-ttu-id="e7e7f-182">Aby wywołać funkcję języka JavaScript, która nie zwraca wartości, użyj `IJSRuntime.InvokeVoidAsync`.</span><span class="sxs-lookup"><span data-stu-id="e7e7f-182">To call a JavaScript function that doesn't return a value, use `IJSRuntime.InvokeVoidAsync`.</span></span> <span data-ttu-id="e7e7f-183">Poniższy kod ustawia fokus na wejściu do nazwy użytkownika, wywołując poprzednią funkcję JavaScript z przechwyconą `ElementReference`:</span><span class="sxs-lookup"><span data-stu-id="e7e7f-183">The following code sets the focus on the username input by calling the preceding JavaScript function with the captured `ElementReference`:</span></span>

[!code-razor[](javascript-interop/samples_snapshot/component1.razor?highlight=1,3,11-12)]

<span data-ttu-id="e7e7f-184">Aby użyć metody rozszerzenia, Utwórz statyczną metodę rozszerzenia, która odbiera wystąpienie `IJSRuntime`:</span><span class="sxs-lookup"><span data-stu-id="e7e7f-184">To use an extension method, create a static extension method that receives the `IJSRuntime` instance:</span></span>

```csharp
public static async Task Focus(this ElementReference elementRef, IJSRuntime jsRuntime)
{
    await jsRuntime.InvokeVoidAsync(
        "exampleJsFunctions.focusElement", elementRef);
}
```

<span data-ttu-id="e7e7f-185">Metoda `Focus` jest wywoływana bezpośrednio dla obiektu.</span><span class="sxs-lookup"><span data-stu-id="e7e7f-185">The `Focus` method is called directly on the object.</span></span> <span data-ttu-id="e7e7f-186">W poniższym przykładzie przyjęto założenie, że metoda `Focus` jest dostępna z przestrzeni nazw `JsInteropClasses`:</span><span class="sxs-lookup"><span data-stu-id="e7e7f-186">The following example assumes that the `Focus` method is available from the `JsInteropClasses` namespace:</span></span>

[!code-razor[](javascript-interop/samples_snapshot/component2.razor?highlight=1-4,12)]

> [!IMPORTANT]
> <span data-ttu-id="e7e7f-187">Zmienna `username` jest wypełniana tylko po wyrenderowaniu składnika.</span><span class="sxs-lookup"><span data-stu-id="e7e7f-187">The `username` variable is only populated after the component is rendered.</span></span> <span data-ttu-id="e7e7f-188">Jeśli niewypełniony `ElementReference` jest przekazywane do kodu JavaScript, kod JavaScript otrzymuje wartość `null`.</span><span class="sxs-lookup"><span data-stu-id="e7e7f-188">If an unpopulated `ElementReference` is passed to JavaScript code, the JavaScript code receives a value of `null`.</span></span> <span data-ttu-id="e7e7f-189">Aby manipulować odwołaniami do elementów po zakończeniu renderowania składnika (aby ustawić początkowy fokus w elemencie), użyj [metod cyklu życia składnika OnAfterRenderAsync lub OnAfterRender](xref:blazor/lifecycle#after-component-render).</span><span class="sxs-lookup"><span data-stu-id="e7e7f-189">To manipulate element references after the component has finished rendering (to set the initial focus on an element) use the [OnAfterRenderAsync or OnAfterRender component lifecycle methods](xref:blazor/lifecycle#after-component-render).</span></span>

<span data-ttu-id="e7e7f-190">Podczas pracy z typami ogólnymi i zwracania wartości należy użyć [ValueTask\<t >](xref:System.Threading.Tasks.ValueTask`1):</span><span class="sxs-lookup"><span data-stu-id="e7e7f-190">When working with generic types and returning a value, use [ValueTask\<T>](xref:System.Threading.Tasks.ValueTask`1):</span></span>

```csharp
public static ValueTask<T> GenericMethod<T>(this ElementReference elementRef, 
    IJSRuntime jsRuntime)
{
    return jsRuntime.InvokeAsync<T>(
        "exampleJsFunctions.doSomethingGeneric", elementRef);
}
```

<span data-ttu-id="e7e7f-191">`GenericMethod` jest wywoływana bezpośrednio w obiekcie z typem.</span><span class="sxs-lookup"><span data-stu-id="e7e7f-191">`GenericMethod` is called directly on the object with a type.</span></span> <span data-ttu-id="e7e7f-192">W poniższym przykładzie przyjęto założenie, że `GenericMethod` jest dostępny z przestrzeni nazw `JsInteropClasses`:</span><span class="sxs-lookup"><span data-stu-id="e7e7f-192">The following example assumes that the `GenericMethod` is available from the `JsInteropClasses` namespace:</span></span>

[!code-razor[](javascript-interop/samples_snapshot/component3.razor?highlight=17)]

## <a name="invoke-net-methods-from-javascript-functions"></a><span data-ttu-id="e7e7f-193">Wywoływanie metod .NET przy użyciu funkcji języka JavaScript</span><span class="sxs-lookup"><span data-stu-id="e7e7f-193">Invoke .NET methods from JavaScript functions</span></span>

### <a name="static-net-method-call"></a><span data-ttu-id="e7e7f-194">Statyczne wywołanie metody .NET</span><span class="sxs-lookup"><span data-stu-id="e7e7f-194">Static .NET method call</span></span>

<span data-ttu-id="e7e7f-195">Aby wywołać statyczną metodę .NET z poziomu języka JavaScript, użyj funkcji `DotNet.invokeMethod` lub `DotNet.invokeMethodAsync`.</span><span class="sxs-lookup"><span data-stu-id="e7e7f-195">To invoke a static .NET method from JavaScript, use the `DotNet.invokeMethod` or `DotNet.invokeMethodAsync` functions.</span></span> <span data-ttu-id="e7e7f-196">Przekaż identyfikator metody statycznej, która ma być wywoływana, nazwę zestawu zawierającego funkcję i wszelkie argumenty.</span><span class="sxs-lookup"><span data-stu-id="e7e7f-196">Pass in the identifier of the static method you wish to call, the name of the assembly containing the function, and any arguments.</span></span> <span data-ttu-id="e7e7f-197">Wersja asynchroniczna jest preferowana do obsługi scenariuszy serwera Blazor.</span><span class="sxs-lookup"><span data-stu-id="e7e7f-197">The asynchronous version is preferred to support Blazor Server scenarios.</span></span> <span data-ttu-id="e7e7f-198">Aby wywołać metodę .NET z języka JavaScript, metoda .NET musi być publiczna, statyczna i mieć atrybut `[JSInvokable]`.</span><span class="sxs-lookup"><span data-stu-id="e7e7f-198">To invoke a .NET method from JavaScript, the .NET method must be public, static, and have the `[JSInvokable]` attribute.</span></span> <span data-ttu-id="e7e7f-199">Domyślnie identyfikator metody jest nazwą metody, ale można określić inny identyfikator przy użyciu konstruktora `JSInvokableAttribute`.</span><span class="sxs-lookup"><span data-stu-id="e7e7f-199">By default, the method identifier is the method name, but you can specify a different identifier using the `JSInvokableAttribute` constructor.</span></span> <span data-ttu-id="e7e7f-200">Wywoływanie otwartych metod ogólnych nie jest obecnie obsługiwane.</span><span class="sxs-lookup"><span data-stu-id="e7e7f-200">Calling open generic methods isn't currently supported.</span></span>

<span data-ttu-id="e7e7f-201">Przykładowa aplikacja zawiera C# metodę zwracającą tablicę `int`s.</span><span class="sxs-lookup"><span data-stu-id="e7e7f-201">The sample app includes a C# method to return an array of `int`s.</span></span> <span data-ttu-id="e7e7f-202">Atrybut `JSInvokable` jest stosowany do metody.</span><span class="sxs-lookup"><span data-stu-id="e7e7f-202">The `JSInvokable` attribute is applied to the method.</span></span>

<span data-ttu-id="e7e7f-203">*Strony/JsInterop. Razor*:</span><span class="sxs-lookup"><span data-stu-id="e7e7f-203">*Pages/JsInterop.razor*:</span></span>

```razor
<button type="button" class="btn btn-primary"
        onclick="exampleJsFunctions.returnArrayAsyncJs()">
    Trigger .NET static method ReturnArrayAsync
</button>

@code {
    [JSInvokable]
    public static Task<int[]> ReturnArrayAsync()
    {
        return Task.FromResult(new int[] { 1, 2, 3 });
    }
}
```

<span data-ttu-id="e7e7f-204">Kod JavaScript obsługiwany przez klienta wywołuje metodę C# .NET.</span><span class="sxs-lookup"><span data-stu-id="e7e7f-204">JavaScript served to the client invokes the C# .NET method.</span></span>

<span data-ttu-id="e7e7f-205">*wwwroot/exampleJsInterop. js*:</span><span class="sxs-lookup"><span data-stu-id="e7e7f-205">*wwwroot/exampleJsInterop.js*:</span></span>

[!code-javascript[](./common/samples/3.x/BlazorWebAssemblySample/wwwroot/exampleJsInterop.js?highlight=8-14)]

<span data-ttu-id="e7e7f-206">Gdy zostanie wybrany przycisk **ReturnArrayAsync Wyzwalaj metodę statyczną .NET** , sprawdź dane wyjściowe konsoli w narzędziach deweloperskich sieci Web w przeglądarce.</span><span class="sxs-lookup"><span data-stu-id="e7e7f-206">When the **Trigger .NET static method ReturnArrayAsync** button is selected, examine the console output in the browser's web developer tools.</span></span>

<span data-ttu-id="e7e7f-207">Dane wyjściowe konsoli są następujące:</span><span class="sxs-lookup"><span data-stu-id="e7e7f-207">The console output is:</span></span>

```console
Array(4) [ 1, 2, 3, 4 ]
```

<span data-ttu-id="e7e7f-208">Czwarta wartość tablicy jest wypychana do tablicy (`data.push(4);`) zwróconej przez `ReturnArrayAsync`.</span><span class="sxs-lookup"><span data-stu-id="e7e7f-208">The fourth array value is pushed to the array (`data.push(4);`) returned by `ReturnArrayAsync`.</span></span>

### <a name="instance-method-call"></a><span data-ttu-id="e7e7f-209">Wywołanie metody wystąpienia</span><span class="sxs-lookup"><span data-stu-id="e7e7f-209">Instance method call</span></span>

<span data-ttu-id="e7e7f-210">Można również wywołać metody wystąpienia platformy .NET z poziomu języka JavaScript.</span><span class="sxs-lookup"><span data-stu-id="e7e7f-210">You can also call .NET instance methods from JavaScript.</span></span> <span data-ttu-id="e7e7f-211">Aby wywołać metodę wystąpienia platformy .NET z poziomu języka JavaScript:</span><span class="sxs-lookup"><span data-stu-id="e7e7f-211">To invoke a .NET instance method from JavaScript:</span></span>

* <span data-ttu-id="e7e7f-212">Przekaż wystąpienie programu .NET do języka JavaScript, zawijając je w wystąpieniu `DotNetObjectReference`.</span><span class="sxs-lookup"><span data-stu-id="e7e7f-212">Pass the .NET instance to JavaScript by wrapping it in a `DotNetObjectReference` instance.</span></span> <span data-ttu-id="e7e7f-213">Wystąpienie programu .NET jest przesyłane przez odwołanie do języka JavaScript.</span><span class="sxs-lookup"><span data-stu-id="e7e7f-213">The .NET instance is passed by reference to JavaScript.</span></span>
* <span data-ttu-id="e7e7f-214">Wywołaj metody wystąpienia platformy .NET w wystąpieniu przy użyciu funkcji `invokeMethod` lub `invokeMethodAsync`.</span><span class="sxs-lookup"><span data-stu-id="e7e7f-214">Invoke .NET instance methods on the instance using the `invokeMethod` or `invokeMethodAsync` functions.</span></span> <span data-ttu-id="e7e7f-215">Wystąpienie programu .NET może być również przekazywać jako argument podczas wywoływania innych metod .NET z JavaScript.</span><span class="sxs-lookup"><span data-stu-id="e7e7f-215">The .NET instance can also be passed as an argument when invoking other .NET methods from JavaScript.</span></span>

> [!NOTE]
> <span data-ttu-id="e7e7f-216">Przykładowa aplikacja rejestruje komunikaty do konsoli po stronie klienta.</span><span class="sxs-lookup"><span data-stu-id="e7e7f-216">The sample app logs messages to the client-side console.</span></span> <span data-ttu-id="e7e7f-217">W poniższych przykładach zademonstrowanych przez przykładową aplikację można sprawdzić dane wyjściowe konsoli przeglądarki w narzędziach deweloperskich w przeglądarce.</span><span class="sxs-lookup"><span data-stu-id="e7e7f-217">For the following examples demonstrated by the sample app, examine the browser's console output in the browser's developer tools.</span></span>

<span data-ttu-id="e7e7f-218">Po wybraniu przycisku **Wyzwalaj metodę wystąpienia .NET HelloHelper. sayHello** , `ExampleJsInterop.CallHelloHelperSayHello` jest wywoływana i przekazuje nazwę, `Blazor`, do metody.</span><span class="sxs-lookup"><span data-stu-id="e7e7f-218">When the **Trigger .NET instance method HelloHelper.SayHello** button is selected, `ExampleJsInterop.CallHelloHelperSayHello` is called and passes a name, `Blazor`, to the method.</span></span>

<span data-ttu-id="e7e7f-219">*Strony/JsInterop. Razor*:</span><span class="sxs-lookup"><span data-stu-id="e7e7f-219">*Pages/JsInterop.razor*:</span></span>

```razor
<button type="button" class="btn btn-primary" @onclick="TriggerNetInstanceMethod">
    Trigger .NET instance method HelloHelper.SayHello
</button>

@code {
    public async Task TriggerNetInstanceMethod()
    {
        var exampleJsInterop = new ExampleJsInterop(JSRuntime);
        await exampleJsInterop.CallHelloHelperSayHello("Blazor");
    }
}
```

<span data-ttu-id="e7e7f-220">`CallHelloHelperSayHello` wywołuje funkcję JavaScript `sayHello` z nowym wystąpieniem `HelloHelper`.</span><span class="sxs-lookup"><span data-stu-id="e7e7f-220">`CallHelloHelperSayHello` invokes the JavaScript function `sayHello` with a new instance of `HelloHelper`.</span></span>

<span data-ttu-id="e7e7f-221">*JsInteropClasses/ExampleJsInterop.cs*:</span><span class="sxs-lookup"><span data-stu-id="e7e7f-221">*JsInteropClasses/ExampleJsInterop.cs*:</span></span>

[!code-csharp[](./common/samples/3.x/BlazorWebAssemblySample/JsInteropClasses/ExampleJsInterop.cs?name=snippet1&highlight=10-16)]

<span data-ttu-id="e7e7f-222">*wwwroot/exampleJsInterop. js*:</span><span class="sxs-lookup"><span data-stu-id="e7e7f-222">*wwwroot/exampleJsInterop.js*:</span></span>

[!code-javascript[](./common/samples/3.x/BlazorWebAssemblySample/wwwroot/exampleJsInterop.js?highlight=15-18)]

<span data-ttu-id="e7e7f-223">Nazwa jest przenoszona do konstruktora `HelloHelper`, który ustawia właściwość `HelloHelper.Name`.</span><span class="sxs-lookup"><span data-stu-id="e7e7f-223">The name is passed to `HelloHelper`'s constructor, which sets the `HelloHelper.Name` property.</span></span> <span data-ttu-id="e7e7f-224">Gdy funkcja JavaScript `sayHello` jest wykonywana, `HelloHelper.SayHello` zwraca komunikat `Hello, {Name}!`, który jest zapisywana w konsoli przez funkcję JavaScript.</span><span class="sxs-lookup"><span data-stu-id="e7e7f-224">When the JavaScript function `sayHello` is executed, `HelloHelper.SayHello` returns the `Hello, {Name}!` message, which is written to the console by the JavaScript function.</span></span>

<span data-ttu-id="e7e7f-225">*JsInteropClasses/HelloHelper.cs*:</span><span class="sxs-lookup"><span data-stu-id="e7e7f-225">*JsInteropClasses/HelloHelper.cs*:</span></span>

[!code-csharp[](./common/samples/3.x/BlazorWebAssemblySample/JsInteropClasses/HelloHelper.cs?name=snippet1&highlight=5,10-11)]

<span data-ttu-id="e7e7f-226">Dane wyjściowe konsoli w narzędziach deweloperskich sieci Web w przeglądarce:</span><span class="sxs-lookup"><span data-stu-id="e7e7f-226">Console output in the browser's web developer tools:</span></span>

```console
Hello, Blazor!
```

## <a name="share-interop-code-in-a-class-library"></a><span data-ttu-id="e7e7f-227">Udostępnianie kodu międzyoperacyjnego w bibliotece klas</span><span class="sxs-lookup"><span data-stu-id="e7e7f-227">Share interop code in a class library</span></span>

<span data-ttu-id="e7e7f-228">Kod międzyoperacyjny JS można uwzględnić w bibliotece klas, co umożliwia udostępnianie kodu w pakiecie NuGet.</span><span class="sxs-lookup"><span data-stu-id="e7e7f-228">JS interop code can be included in a class library, which allows you to share the code in a NuGet package.</span></span>

<span data-ttu-id="e7e7f-229">Biblioteka klas obsługuje Osadzanie zasobów JavaScript w skompilowanym zestawie.</span><span class="sxs-lookup"><span data-stu-id="e7e7f-229">The class library handles embedding JavaScript resources in the built assembly.</span></span> <span data-ttu-id="e7e7f-230">Pliki JavaScript są umieszczane w folderze *wwwroot* .</span><span class="sxs-lookup"><span data-stu-id="e7e7f-230">The JavaScript files are placed in the *wwwroot* folder.</span></span> <span data-ttu-id="e7e7f-231">Narzędzia te zajmują się osadzaniem zasobów podczas kompilowania biblioteki.</span><span class="sxs-lookup"><span data-stu-id="e7e7f-231">The tooling takes care of embedding the resources when the library is built.</span></span>

<span data-ttu-id="e7e7f-232">Skompilowany pakiet NuGet jest przywoływany w pliku projektu aplikacji w taki sam sposób, w jaki jest przywoływany każdy pakiet NuGet.</span><span class="sxs-lookup"><span data-stu-id="e7e7f-232">The built NuGet package is referenced in the app's project file the same way that any NuGet package is referenced.</span></span> <span data-ttu-id="e7e7f-233">Po przywróceniu pakietu kod aplikacji może być wywoływany w języku JavaScript, tak jakby C#był.</span><span class="sxs-lookup"><span data-stu-id="e7e7f-233">After the package is restored, app code can call into JavaScript as if it were C#.</span></span>

<span data-ttu-id="e7e7f-234">Aby uzyskać więcej informacji, zobacz temat <xref:blazor/class-libraries>.</span><span class="sxs-lookup"><span data-stu-id="e7e7f-234">For more information, see <xref:blazor/class-libraries>.</span></span>

## <a name="harden-js-interop-calls"></a><span data-ttu-id="e7e7f-235">Zabezpieczenia wywołań międzyoperacyjnych w ramach funkcjonalności JS</span><span class="sxs-lookup"><span data-stu-id="e7e7f-235">Harden JS interop calls</span></span>

<span data-ttu-id="e7e7f-236">Usługa JS Interop może zakończyć się niepowodzeniem z powodu błędów sieci i powinna być traktowana jako niezawodna.</span><span class="sxs-lookup"><span data-stu-id="e7e7f-236">JS interop may fail due to networking errors and should be treated as unreliable.</span></span> <span data-ttu-id="e7e7f-237">Domyślnie aplikacja serwerowa Blazor przeprowadziła limit czasu wywołań międzyoperacyjnych JS na serwerze po jednej minucie.</span><span class="sxs-lookup"><span data-stu-id="e7e7f-237">By default, a Blazor Server app times out JS interop calls on the server after one minute.</span></span> <span data-ttu-id="e7e7f-238">Jeśli aplikacja może tolerować bardziej agresywny limit czasu, na przykład 10 sekund, należy ustawić limit czasu przy użyciu jednej z następujących metod:</span><span class="sxs-lookup"><span data-stu-id="e7e7f-238">If an app can tolerate a more aggressive timeout, such as 10 seconds, set the timeout using one of the following approaches:</span></span>

* <span data-ttu-id="e7e7f-239">Globalnie w `Startup.ConfigureServices`Określ limit czasu:</span><span class="sxs-lookup"><span data-stu-id="e7e7f-239">Globally in `Startup.ConfigureServices`, specify the timeout:</span></span>

  ```csharp
  services.AddServerSideBlazor(
      options => options.JSInteropDefaultCallTimeout = TimeSpan.FromSeconds({SECONDS}));
  ```

* <span data-ttu-id="e7e7f-240">Dla wywołania w kodzie składnika pojedyncze wywołanie może określać limit czasu:</span><span class="sxs-lookup"><span data-stu-id="e7e7f-240">Per-invocation in component code, a single call can specify the timeout:</span></span>

  ```csharp
  var result = await JSRuntime.InvokeAsync<string>("MyJSOperation", 
      TimeSpan.FromSeconds({SECONDS}), new[] { "Arg1" });
  ```

<span data-ttu-id="e7e7f-241">Aby uzyskać więcej informacji na temat wyczerpania zasobów, zobacz <xref:security/blazor/server>.</span><span class="sxs-lookup"><span data-stu-id="e7e7f-241">For more information on resource exhaustion, see <xref:security/blazor/server>.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="e7e7f-242">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="e7e7f-242">Additional resources</span></span>

* [<span data-ttu-id="e7e7f-243">InteropComponent. Razor — przykład (repozytorium dotnet/AspNetCore w witrynie GitHub, 3,1 gałąź wydania)</span><span class="sxs-lookup"><span data-stu-id="e7e7f-243">InteropComponent.razor example (dotnet/AspNetCore GitHub repository, 3.1 release branch)</span></span>](https://github.com/dotnet/AspNetCore/blob/release/3.1/src/Components/test/testassets/BasicTestApp/InteropComponent.razor)
