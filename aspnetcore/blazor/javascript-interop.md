---
title: ASP.NET Core Blazor JavaScript Interop
author: guardrex
description: Dowiedz się, jak wywoływać funkcje języka JavaScript z technologii .NET i .NET z poziomu języka JavaScript w aplikacjach Blazor.
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 02/12/2020
no-loc:
- Blazor
- SignalR
uid: blazor/javascript-interop
ms.openlocfilehash: d681eea5a5e876912bd614fba8ea45a464844496
ms.sourcegitcommit: 6645435fc8f5092fc7e923742e85592b56e37ada
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/19/2020
ms.locfileid: "77447168"
---
# <a name="aspnet-core-opno-locblazor-javascript-interop"></a><span data-ttu-id="48e35-103">ASP.NET Core Blazor JavaScript Interop</span><span class="sxs-lookup"><span data-stu-id="48e35-103">ASP.NET Core Blazor JavaScript interop</span></span>

<span data-ttu-id="48e35-104">[Javier Calvarro Nelson](https://github.com/javiercn), [Daniel Roth](https://github.com/danroth27)i [Luke](https://github.com/guardrex) Latham</span><span class="sxs-lookup"><span data-stu-id="48e35-104">By [Javier Calvarro Nelson](https://github.com/javiercn), [Daniel Roth](https://github.com/danroth27), and [Luke Latham](https://github.com/guardrex)</span></span>

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

<span data-ttu-id="48e35-105">Aplikacja Blazor może wywoływać funkcje języka JavaScript z technologii .NET i .NET z kodu JavaScript.</span><span class="sxs-lookup"><span data-stu-id="48e35-105">A Blazor app can invoke JavaScript functions from .NET and .NET methods from JavaScript code.</span></span>

<span data-ttu-id="48e35-106">[Wyświetl lub pobierz przykładowy kod](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/blazor/common/samples/) ([jak pobrać](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="48e35-106">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/blazor/common/samples/) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="invoke-javascript-functions-from-net-methods"></a><span data-ttu-id="48e35-107">Wywoływanie funkcji języka JavaScript z metod .NET</span><span class="sxs-lookup"><span data-stu-id="48e35-107">Invoke JavaScript functions from .NET methods</span></span>

<span data-ttu-id="48e35-108">Istnieją przypadki, w których kod .NET jest wymagany do wywołania funkcji języka JavaScript.</span><span class="sxs-lookup"><span data-stu-id="48e35-108">There are times when .NET code is required to call a JavaScript function.</span></span> <span data-ttu-id="48e35-109">Na przykład wywołanie języka JavaScript może uwidaczniać możliwości przeglądarki lub funkcje z biblioteki JavaScript w aplikacji.</span><span class="sxs-lookup"><span data-stu-id="48e35-109">For example, a JavaScript call can expose browser capabilities or functionality from a JavaScript library to the app.</span></span> <span data-ttu-id="48e35-110">Ten scenariusz jest nazywany *współdziałaniem języka JavaScript* (w programie*js Interop*).</span><span class="sxs-lookup"><span data-stu-id="48e35-110">This scenario is called *JavaScript interoperability* (*JS interop*).</span></span>

<span data-ttu-id="48e35-111">Aby wywołać kod JavaScript z platformy .NET, użyj abstrakcji `IJSRuntime`.</span><span class="sxs-lookup"><span data-stu-id="48e35-111">To call into JavaScript from .NET, use the `IJSRuntime` abstraction.</span></span> <span data-ttu-id="48e35-112">Aby wystawić wywołania w programie JS Interop, wstrzyknąć `IJSRuntime` abstrakcję w składniku.</span><span class="sxs-lookup"><span data-stu-id="48e35-112">To issue JS interop calls, inject the `IJSRuntime` abstraction in your component.</span></span> <span data-ttu-id="48e35-113">Metoda `InvokeAsync<T>` przyjmuje identyfikator funkcji języka JavaScript, która ma zostać wywołana wraz z dowolną liczbą argumentów z możliwością serializacji JSON.</span><span class="sxs-lookup"><span data-stu-id="48e35-113">The `InvokeAsync<T>` method takes an identifier for the JavaScript function that you wish to invoke along with any number of JSON-serializable arguments.</span></span> <span data-ttu-id="48e35-114">Identyfikator funkcji jest względny w stosunku do zakresu globalnego (`window`).</span><span class="sxs-lookup"><span data-stu-id="48e35-114">The function identifier is relative to the global scope (`window`).</span></span> <span data-ttu-id="48e35-115">Jeśli chcesz wywołać `window.someScope.someFunction`, identyfikator jest `someScope.someFunction`.</span><span class="sxs-lookup"><span data-stu-id="48e35-115">If you wish to call `window.someScope.someFunction`, the identifier is `someScope.someFunction`.</span></span> <span data-ttu-id="48e35-116">Nie ma potrzeby rejestrowania funkcji przed jej wywołaniem.</span><span class="sxs-lookup"><span data-stu-id="48e35-116">There's no need to register the function before it's called.</span></span> <span data-ttu-id="48e35-117">Typ zwracany `T` musi również być możliwy do serializacji kod JSON.</span><span class="sxs-lookup"><span data-stu-id="48e35-117">The return type `T` must also be JSON serializable.</span></span> <span data-ttu-id="48e35-118">`T` powinna być zgodna z typem .NET, który najlepiej jest mapowany do zwracanego typu JSON.</span><span class="sxs-lookup"><span data-stu-id="48e35-118">`T` should match the .NET type that best maps to the JSON type returned.</span></span>

<span data-ttu-id="48e35-119">W przypadku aplikacji Blazor Server z włączoną obsługą prerenderowania Wywoływanie kodu JavaScript nie jest możliwe podczas początkowego wstępnego renderowania.</span><span class="sxs-lookup"><span data-stu-id="48e35-119">For Blazor Server apps with prerendering enabled, calling into JavaScript isn't possible during the initial prerendering.</span></span> <span data-ttu-id="48e35-120">Wywołania międzyoperacyjne języka JavaScript muszą zostać odroczone do momentu ustanowienia połączenia z przeglądarką.</span><span class="sxs-lookup"><span data-stu-id="48e35-120">JavaScript interop calls must be deferred until after the connection with the browser is established.</span></span> <span data-ttu-id="48e35-121">Aby uzyskać więcej informacji, zobacz sekcję [wykrywanie, kiedy aplikacja Blazor jest renderowana](#detect-when-a-blazor-app-is-prerendering) .</span><span class="sxs-lookup"><span data-stu-id="48e35-121">For more information, see the [Detect when a Blazor app is prerendering](#detect-when-a-blazor-app-is-prerendering) section.</span></span>

<span data-ttu-id="48e35-122">Poniższy przykład jest oparty na [dekoderze](https://developer.mozilla.org/docs/Web/API/TextDecoder), eksperymentalnym dekoderem JavaScript.</span><span class="sxs-lookup"><span data-stu-id="48e35-122">The following example is based on [TextDecoder](https://developer.mozilla.org/docs/Web/API/TextDecoder), an experimental JavaScript-based decoder.</span></span> <span data-ttu-id="48e35-123">W przykładzie pokazano, jak wywołać funkcję JavaScript z C# metody.</span><span class="sxs-lookup"><span data-stu-id="48e35-123">The example demonstrates how to invoke a JavaScript function from a C# method.</span></span> <span data-ttu-id="48e35-124">Funkcja JavaScript akceptuje tablicę bajtową z C# metody, dekoduje tablicę i zwraca tekst do składnika do wyświetlenia.</span><span class="sxs-lookup"><span data-stu-id="48e35-124">The JavaScript function accepts a byte array from a C# method, decodes the array, and returns the text to the component for display.</span></span>

<span data-ttu-id="48e35-125">Wewnątrz elementu `<head>` *wwwroot/index.html* (Blazor webassembly) lub *pages/_Host. cshtml* (Blazor Server) podaj funkcję języka JavaScript, która używa `TextDecoder` do zdekodowania przekazaną tablicę i zwrócenie zdekodowanej wartości:</span><span class="sxs-lookup"><span data-stu-id="48e35-125">Inside the `<head>` element of *wwwroot/index.html* (Blazor WebAssembly) or *Pages/_Host.cshtml* (Blazor Server), provide a JavaScript function that uses `TextDecoder` to decode a passed array and return the decoded value:</span></span>

[!code-html[](javascript-interop/samples_snapshot/index-script-convertarray.html)]

<span data-ttu-id="48e35-126">Kod JavaScript, taki jak kod przedstawiony w powyższym przykładzie, można również załadować z pliku JavaScript ( *. js*) z odwołaniem do pliku skryptu:</span><span class="sxs-lookup"><span data-stu-id="48e35-126">JavaScript code, such as the code shown in the preceding example, can also be loaded from a JavaScript file (*.js*) with a reference to the script file:</span></span>

```html
<script src="exampleJsInterop.js"></script>
```

<span data-ttu-id="48e35-127">Następujący składnik:</span><span class="sxs-lookup"><span data-stu-id="48e35-127">The following component:</span></span>

* <span data-ttu-id="48e35-128">Wywołuje funkcję `convertArray` JavaScript przy użyciu `JSRuntime`, gdy zostanie wybrany przycisk składnika (**Konwertuj tablicę**).</span><span class="sxs-lookup"><span data-stu-id="48e35-128">Invokes the `convertArray` JavaScript function using `JSRuntime` when a component button (**Convert Array**) is selected.</span></span>
* <span data-ttu-id="48e35-129">Po wywołaniu funkcji języka JavaScript przenoszona tablica jest konwertowana na ciąg.</span><span class="sxs-lookup"><span data-stu-id="48e35-129">After the JavaScript function is called, the passed array is converted into a string.</span></span> <span data-ttu-id="48e35-130">Ciąg jest zwracany do składnika do wyświetlenia.</span><span class="sxs-lookup"><span data-stu-id="48e35-130">The string is returned to the component for display.</span></span>

[!code-razor[](javascript-interop/samples_snapshot/call-js-example.razor?highlight=2,34-35)]

## <a name="use-of-ijsruntime"></a><span data-ttu-id="48e35-131">Korzystanie z IJSRuntime</span><span class="sxs-lookup"><span data-stu-id="48e35-131">Use of IJSRuntime</span></span>

<span data-ttu-id="48e35-132">Aby użyć abstrakcji `IJSRuntime`, należy zastosować jedną z następujących metod:</span><span class="sxs-lookup"><span data-stu-id="48e35-132">To use the `IJSRuntime` abstraction, adopt any of the following approaches:</span></span>

* <span data-ttu-id="48e35-133">Wsuń `IJSRuntime` abstrakcję do składnika Razor ( *. Razor*):</span><span class="sxs-lookup"><span data-stu-id="48e35-133">Inject the `IJSRuntime` abstraction into the Razor component (*.razor*):</span></span>

  [!code-razor[](javascript-interop/samples_snapshot/inject-abstraction.razor?highlight=1)]

  <span data-ttu-id="48e35-134">Wewnątrz elementu `<head>` *wwwroot/index.html* (Blazor webassembly) lub *pages/_Host. cshtml* (Blazor Server) podaj funkcję JavaScript `handleTickerChanged`.</span><span class="sxs-lookup"><span data-stu-id="48e35-134">Inside the `<head>` element of *wwwroot/index.html* (Blazor WebAssembly) or *Pages/_Host.cshtml* (Blazor Server), provide a `handleTickerChanged` JavaScript function.</span></span> <span data-ttu-id="48e35-135">Funkcja jest wywoływana z `IJSRuntime.InvokeVoidAsync` i nie zwraca wartości:</span><span class="sxs-lookup"><span data-stu-id="48e35-135">The function is called with `IJSRuntime.InvokeVoidAsync` and doesn't return a value:</span></span>

  [!code-html[](javascript-interop/samples_snapshot/index-script-handleTickerChanged1.html)]

* <span data-ttu-id="48e35-136">Wstrzyknąć `IJSRuntime` abstrakcję do klasy ( *. cs*):</span><span class="sxs-lookup"><span data-stu-id="48e35-136">Inject the `IJSRuntime` abstraction into a class (*.cs*):</span></span>

  [!code-csharp[](javascript-interop/samples_snapshot/inject-abstraction-class.cs?highlight=5)]

  <span data-ttu-id="48e35-137">Wewnątrz elementu `<head>` *wwwroot/index.html* (Blazor webassembly) lub *pages/_Host. cshtml* (Blazor Server) podaj funkcję JavaScript `handleTickerChanged`.</span><span class="sxs-lookup"><span data-stu-id="48e35-137">Inside the `<head>` element of *wwwroot/index.html* (Blazor WebAssembly) or *Pages/_Host.cshtml* (Blazor Server), provide a `handleTickerChanged` JavaScript function.</span></span> <span data-ttu-id="48e35-138">Funkcja jest wywoływana z `JSRuntime.InvokeAsync` i zwraca wartość:</span><span class="sxs-lookup"><span data-stu-id="48e35-138">The function is called with `JSRuntime.InvokeAsync` and returns a value:</span></span>

  [!code-html[](javascript-interop/samples_snapshot/index-script-handleTickerChanged2.html)]

* <span data-ttu-id="48e35-139">Aby można było wygenerować zawartość dynamiczną przy użyciu [BuildRenderTree](xref:blazor/advanced-scenarios#manual-rendertreebuilder-logic), użyj atrybutu `[Inject]`:</span><span class="sxs-lookup"><span data-stu-id="48e35-139">For dynamic content generation with [BuildRenderTree](xref:blazor/advanced-scenarios#manual-rendertreebuilder-logic), use the `[Inject]` attribute:</span></span>

  ```razor
  [Inject]
  IJSRuntime JSRuntime { get; set; }
  ```

<span data-ttu-id="48e35-140">W aplikacji przykładowej po stronie klienta, która jest dołączona do tego tematu, dostępne są dwie funkcje języka JavaScript, które współdziałają z modelem DOM, aby odbierać dane wejściowe użytkownika i wyświetlać komunikat powitalny:</span><span class="sxs-lookup"><span data-stu-id="48e35-140">In the client-side sample app that accompanies this topic, two JavaScript functions are available to the app that interact with the DOM to receive user input and display a welcome message:</span></span>

* <span data-ttu-id="48e35-141">`showPrompt` &ndash; generuje monit o zaakceptowanie danych wprowadzonych przez użytkownika (nazwę użytkownika) i zwraca nazwę obiektu wywołującego.</span><span class="sxs-lookup"><span data-stu-id="48e35-141">`showPrompt` &ndash; Produces a prompt to accept user input (the user's name) and returns the name to the caller.</span></span>
* <span data-ttu-id="48e35-142">`displayWelcome` &ndash; przypisuje Komunikat powitalny od wywołującego do obiektu DOM z `id` `welcome`.</span><span class="sxs-lookup"><span data-stu-id="48e35-142">`displayWelcome` &ndash; Assigns a welcome message from the caller to a DOM object with an `id` of `welcome`.</span></span>

<span data-ttu-id="48e35-143">*wwwroot/exampleJsInterop. js*:</span><span class="sxs-lookup"><span data-stu-id="48e35-143">*wwwroot/exampleJsInterop.js*:</span></span>

[!code-javascript[](./common/samples/3.x/BlazorWebAssemblySample/wwwroot/exampleJsInterop.js?highlight=2-7)]

<span data-ttu-id="48e35-144">Umieść tag `<script>` odwołujący się do pliku JavaScript w pliku *wwwroot/index.html* (Blazor webassembly) lub *pages/_Host. cshtml* (serwerBlazor).</span><span class="sxs-lookup"><span data-stu-id="48e35-144">Place the `<script>` tag that references the JavaScript file in the *wwwroot/index.html* file (Blazor WebAssembly) or *Pages/_Host.cshtml* file (Blazor Server).</span></span>

<span data-ttu-id="48e35-145">*wwwroot/index.html* (Blazor webassembly):</span><span class="sxs-lookup"><span data-stu-id="48e35-145">*wwwroot/index.html* (Blazor WebAssembly):</span></span>

[!code-html[](./common/samples/3.x/BlazorWebAssemblySample/wwwroot/index.html?highlight=22)]

<span data-ttu-id="48e35-146">*Pages/_Host. cshtml* (Blazor Server):</span><span class="sxs-lookup"><span data-stu-id="48e35-146">*Pages/_Host.cshtml* (Blazor Server):</span></span>

[!code-cshtml[](./common/samples/3.x/BlazorServerSample/Pages/_Host.cshtml?highlight=35)]

<span data-ttu-id="48e35-147">Nie umieszczaj znacznika `<script>` w pliku składnika, ponieważ nie można dynamicznie zaktualizować tagu `<script>`.</span><span class="sxs-lookup"><span data-stu-id="48e35-147">Don't place a `<script>` tag in a component file because the `<script>` tag can't be updated dynamically.</span></span>

<span data-ttu-id="48e35-148">.NET metod współdziałania z funkcjami JavaScript w pliku *exampleJsInterop. js* przez wywołanie `IJSRuntime.InvokeAsync<T>`.</span><span class="sxs-lookup"><span data-stu-id="48e35-148">.NET methods interop with the JavaScript functions in the *exampleJsInterop.js* file by calling `IJSRuntime.InvokeAsync<T>`.</span></span>

<span data-ttu-id="48e35-149">Abstrakcja `IJSRuntime` jest asynchroniczna, aby umożliwić obsługę scenariuszy Blazor Server.</span><span class="sxs-lookup"><span data-stu-id="48e35-149">The `IJSRuntime` abstraction is asynchronous to allow for Blazor Server scenarios.</span></span> <span data-ttu-id="48e35-150">Jeśli aplikacja jest aplikacją Blazor webassembly i chcesz wywołać funkcję JavaScript synchronicznie, downcast do `IJSInProcessRuntime` i Wywołaj `Invoke<T>` zamiast tego.</span><span class="sxs-lookup"><span data-stu-id="48e35-150">If the app is a Blazor WebAssembly app and you want to invoke a JavaScript function synchronously, downcast to `IJSInProcessRuntime` and call `Invoke<T>` instead.</span></span> <span data-ttu-id="48e35-151">Zalecamy, aby większość bibliotek międzyoperacyjnych JS używała asynchronicznych interfejsów API, aby upewnić się, że biblioteki są dostępne we wszystkich scenariuszach.</span><span class="sxs-lookup"><span data-stu-id="48e35-151">We recommend that most JS interop libraries use the async APIs to ensure that the libraries are available in all scenarios.</span></span>

<span data-ttu-id="48e35-152">Przykładowa aplikacja zawiera składnik demonstrujący międzyoperacyjność JS.</span><span class="sxs-lookup"><span data-stu-id="48e35-152">The sample app includes a component to demonstrate JS interop.</span></span> <span data-ttu-id="48e35-153">Składnik:</span><span class="sxs-lookup"><span data-stu-id="48e35-153">The component:</span></span>

* <span data-ttu-id="48e35-154">Odbiera dane wprowadzane przez użytkownika za pośrednictwem wiersza polecenia języka JavaScript.</span><span class="sxs-lookup"><span data-stu-id="48e35-154">Receives user input via a JavaScript prompt.</span></span>
* <span data-ttu-id="48e35-155">Zwraca tekst do składnika do przetworzenia.</span><span class="sxs-lookup"><span data-stu-id="48e35-155">Returns the text to the component for processing.</span></span>
* <span data-ttu-id="48e35-156">Wywołuje drugą funkcję języka JavaScript, która współdziała z modelem DOM, aby wyświetlić komunikat powitalny.</span><span class="sxs-lookup"><span data-stu-id="48e35-156">Calls a second JavaScript function that interacts with the DOM to display a welcome message.</span></span>

<span data-ttu-id="48e35-157">*Strony/JSInterop. Razor*:</span><span class="sxs-lookup"><span data-stu-id="48e35-157">*Pages/JSInterop.razor*:</span></span>

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

1. <span data-ttu-id="48e35-158">Gdy `TriggerJsPrompt` jest wykonywane, wybierając przycisk **Monituj wyzwalacza JavaScript** składnika, funkcja JavaScript `showPrompt` dostępna w pliku *wwwroot/exampleJsInterop. js* jest wywoływana.</span><span class="sxs-lookup"><span data-stu-id="48e35-158">When `TriggerJsPrompt` is executed by selecting the component's **Trigger JavaScript Prompt** button, the JavaScript `showPrompt` function provided in the *wwwroot/exampleJsInterop.js* file is called.</span></span>
1. <span data-ttu-id="48e35-159">Funkcja `showPrompt` akceptuje dane wejściowe użytkownika (nazwę użytkownika), które są kodowane w formacie HTML i zwracane do składnika.</span><span class="sxs-lookup"><span data-stu-id="48e35-159">The `showPrompt` function accepts user input (the user's name), which is HTML-encoded and returned to the component.</span></span> <span data-ttu-id="48e35-160">Składnik przechowuje nazwę użytkownika w zmiennej lokalnej, `name`.</span><span class="sxs-lookup"><span data-stu-id="48e35-160">The component stores the user's name in a local variable, `name`.</span></span>
1. <span data-ttu-id="48e35-161">Ciąg przechowywany w `name` jest zawarty w komunikacie powitalnym, który jest przesyłany do funkcji języka JavaScript, `displayWelcome`, która renderuje Komunikat powitalny do znacznika nagłówka.</span><span class="sxs-lookup"><span data-stu-id="48e35-161">The string stored in `name` is incorporated into a welcome message, which is passed to a JavaScript function, `displayWelcome`, which renders the welcome message into a heading tag.</span></span>

## <a name="call-a-void-javascript-function"></a><span data-ttu-id="48e35-162">Wywoływanie funkcji języka JavaScript typu void</span><span class="sxs-lookup"><span data-stu-id="48e35-162">Call a void JavaScript function</span></span>

<span data-ttu-id="48e35-163">Funkcje języka JavaScript zwracające [wartość void (0)/void 0](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Operators/void) lub [undefined](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/undefined) są wywoływane z `IJSRuntime.InvokeVoidAsync`.</span><span class="sxs-lookup"><span data-stu-id="48e35-163">JavaScript functions that return [void(0)/void 0](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Operators/void) or [undefined](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/undefined) are called with `IJSRuntime.InvokeVoidAsync`.</span></span>

## <a name="detect-when-a-opno-locblazor-app-is-prerendering"></a><span data-ttu-id="48e35-164">Wykryj, kiedy aplikacja Blazora jest renderowana</span><span class="sxs-lookup"><span data-stu-id="48e35-164">Detect when a Blazor app is prerendering</span></span>
 
[!INCLUDE[](~/includes/blazor-prerendering.md)]

## <a name="capture-references-to-elements"></a><span data-ttu-id="48e35-165">Przechwyć odwołania do elementów</span><span class="sxs-lookup"><span data-stu-id="48e35-165">Capture references to elements</span></span>

<span data-ttu-id="48e35-166">Niektóre scenariusze międzyoperacyjności JS wymagają odwołań do elementów HTML.</span><span class="sxs-lookup"><span data-stu-id="48e35-166">Some JS interop scenarios require references to HTML elements.</span></span> <span data-ttu-id="48e35-167">Na przykład Biblioteka interfejsu użytkownika może wymagać odwołania do elementu dla inicjalizacji lub może być konieczne wywoływanie interfejsów API przypominających polecenia na elemencie, takich jak `focus` lub `play`.</span><span class="sxs-lookup"><span data-stu-id="48e35-167">For example, a UI library may require an element reference for initialization, or you might need to call command-like APIs on an element, such as `focus` or `play`.</span></span>

<span data-ttu-id="48e35-168">Przechwyć odwołania do elementów HTML w składniku, korzystając z następującej metody:</span><span class="sxs-lookup"><span data-stu-id="48e35-168">Capture references to HTML elements in a component using the following approach:</span></span>

* <span data-ttu-id="48e35-169">Dodaj atrybut `@ref` do elementu HTML.</span><span class="sxs-lookup"><span data-stu-id="48e35-169">Add an `@ref` attribute to the HTML element.</span></span>
* <span data-ttu-id="48e35-170">Zdefiniuj pole typu `ElementReference`, którego nazwa pasuje do wartości atrybutu `@ref`.</span><span class="sxs-lookup"><span data-stu-id="48e35-170">Define a field of type `ElementReference` whose name matches the value of the `@ref` attribute.</span></span>

<span data-ttu-id="48e35-171">W poniższym przykładzie pokazano przechwytywanie odwołania do `username` elementu `<input>`:</span><span class="sxs-lookup"><span data-stu-id="48e35-171">The following example shows capturing a reference to the `username` `<input>` element:</span></span>

```razor
<input @ref="username" ... />

@code {
    ElementReference username;
}
```

> [!WARNING]
> <span data-ttu-id="48e35-172">Użyj odwołania do elementu, aby zmodyfikować zawartość pustego elementu, który nie współdziała z Blazor.</span><span class="sxs-lookup"><span data-stu-id="48e35-172">Only use an element reference to mutate the contents of an empty element that doesn't interact with Blazor.</span></span> <span data-ttu-id="48e35-173">Ten scenariusz jest przydatny, gdy interfejs API innej firmy dostarcza zawartość do elementu.</span><span class="sxs-lookup"><span data-stu-id="48e35-173">This scenario is useful when a 3rd party API supplies content to the element.</span></span> <span data-ttu-id="48e35-174">Ponieważ Blazor nie współdziała z elementem, nie ma możliwości konfliktu między reprezentacją elementu a obiektem DOM przez Blazor.</span><span class="sxs-lookup"><span data-stu-id="48e35-174">Because Blazor doesn't interact with the element, there's no possibility of a conflict between Blazor's representation of the element and the DOM.</span></span>
>
> <span data-ttu-id="48e35-175">W poniższym przykładzie jest *niebezpieczne* do mutacji zawartości listy nieuporządkowanej (`ul`), ponieważ Blazor współdziała z modelem dom w celu wypełnienia elementów listy elementu (`<li>`):</span><span class="sxs-lookup"><span data-stu-id="48e35-175">In the following example, it's *dangerous* to mutate the contents of the unordered list (`ul`) because Blazor interacts with the DOM to populate this element's list items (`<li>`):</span></span>
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
> <span data-ttu-id="48e35-176">Jeśli program JS Interop przyniesie zawartość elementu `MyList` i Blazor próbuje zastosować różnic do elementu, różnice nie będą zgodne z modelem DOM.</span><span class="sxs-lookup"><span data-stu-id="48e35-176">If JS interop mutates the contents of element `MyList` and Blazor attempts to apply diffs to the element, the diffs won't match the DOM.</span></span>

<span data-ttu-id="48e35-177">W odniesieniu do kodu platformy .NET `ElementReference` jest nieprzezroczystym uchwytem.</span><span class="sxs-lookup"><span data-stu-id="48e35-177">As far as .NET code is concerned, an `ElementReference` is an opaque handle.</span></span> <span data-ttu-id="48e35-178">*Jedyną* czynnością, którą można wykonać za pomocą `ElementReference`, jest przekazanie jej do kodu JavaScript za pośrednictwem międzyoperacyjnego js.</span><span class="sxs-lookup"><span data-stu-id="48e35-178">The *only* thing you can do with `ElementReference` is pass it through to JavaScript code via JS interop.</span></span> <span data-ttu-id="48e35-179">Gdy to zrobisz, kod po stronie JavaScript odbiera wystąpienie `HTMLElement`, które może być używane z normalnymi interfejsami API modelu DOM.</span><span class="sxs-lookup"><span data-stu-id="48e35-179">When you do so, the JavaScript-side code receives an `HTMLElement` instance, which it can use with normal DOM APIs.</span></span>

<span data-ttu-id="48e35-180">Na przykład poniższy kod definiuje metodę rozszerzenia .NET, która umożliwia ustawienie fokusu na elemencie:</span><span class="sxs-lookup"><span data-stu-id="48e35-180">For example, the following code defines a .NET extension method that enables setting the focus on an element:</span></span>

<span data-ttu-id="48e35-181">*exampleJsInterop. js*:</span><span class="sxs-lookup"><span data-stu-id="48e35-181">*exampleJsInterop.js*:</span></span>

```javascript
window.exampleJsFunctions = {
  focusElement : function (element) {
    element.focus();
  }
}
```

<span data-ttu-id="48e35-182">Aby wywołać funkcję języka JavaScript, która nie zwraca wartości, użyj `IJSRuntime.InvokeVoidAsync`.</span><span class="sxs-lookup"><span data-stu-id="48e35-182">To call a JavaScript function that doesn't return a value, use `IJSRuntime.InvokeVoidAsync`.</span></span> <span data-ttu-id="48e35-183">Poniższy kod ustawia fokus na wejściu do nazwy użytkownika, wywołując poprzednią funkcję JavaScript z przechwyconą `ElementReference`:</span><span class="sxs-lookup"><span data-stu-id="48e35-183">The following code sets the focus on the username input by calling the preceding JavaScript function with the captured `ElementReference`:</span></span>

[!code-razor[](javascript-interop/samples_snapshot/component1.razor?highlight=1,3,11-12)]

<span data-ttu-id="48e35-184">Aby użyć metody rozszerzenia, Utwórz statyczną metodę rozszerzenia, która odbiera wystąpienie `IJSRuntime`:</span><span class="sxs-lookup"><span data-stu-id="48e35-184">To use an extension method, create a static extension method that receives the `IJSRuntime` instance:</span></span>

```csharp
public static async Task Focus(this ElementReference elementRef, IJSRuntime jsRuntime)
{
    await jsRuntime.InvokeVoidAsync(
        "exampleJsFunctions.focusElement", elementRef);
}
```

<span data-ttu-id="48e35-185">Metoda `Focus` jest wywoływana bezpośrednio dla obiektu.</span><span class="sxs-lookup"><span data-stu-id="48e35-185">The `Focus` method is called directly on the object.</span></span> <span data-ttu-id="48e35-186">W poniższym przykładzie przyjęto założenie, że metoda `Focus` jest dostępna z przestrzeni nazw `JsInteropClasses`:</span><span class="sxs-lookup"><span data-stu-id="48e35-186">The following example assumes that the `Focus` method is available from the `JsInteropClasses` namespace:</span></span>

[!code-razor[](javascript-interop/samples_snapshot/component2.razor?highlight=1-4,12)]

> [!IMPORTANT]
> <span data-ttu-id="48e35-187">Zmienna `username` jest wypełniana tylko po wyrenderowaniu składnika.</span><span class="sxs-lookup"><span data-stu-id="48e35-187">The `username` variable is only populated after the component is rendered.</span></span> <span data-ttu-id="48e35-188">Jeśli niewypełniony `ElementReference` jest przekazywane do kodu JavaScript, kod JavaScript otrzymuje wartość `null`.</span><span class="sxs-lookup"><span data-stu-id="48e35-188">If an unpopulated `ElementReference` is passed to JavaScript code, the JavaScript code receives a value of `null`.</span></span> <span data-ttu-id="48e35-189">Aby manipulować odwołaniami do elementów po zakończeniu renderowania składnika (aby ustawić początkowy fokus w elemencie), użyj [metod cyklu życia składnika OnAfterRenderAsync lub OnAfterRender](xref:blazor/lifecycle#after-component-render).</span><span class="sxs-lookup"><span data-stu-id="48e35-189">To manipulate element references after the component has finished rendering (to set the initial focus on an element) use the [OnAfterRenderAsync or OnAfterRender component lifecycle methods](xref:blazor/lifecycle#after-component-render).</span></span>

<span data-ttu-id="48e35-190">Podczas pracy z typami ogólnymi i zwracania wartości należy użyć [ValueTask\<t >](xref:System.Threading.Tasks.ValueTask`1):</span><span class="sxs-lookup"><span data-stu-id="48e35-190">When working with generic types and returning a value, use [ValueTask\<T>](xref:System.Threading.Tasks.ValueTask`1):</span></span>

```csharp
public static ValueTask<T> GenericMethod<T>(this ElementReference elementRef, 
    IJSRuntime jsRuntime)
{
    return jsRuntime.InvokeAsync<T>(
        "exampleJsFunctions.doSomethingGeneric", elementRef);
}
```

<span data-ttu-id="48e35-191">`GenericMethod` jest wywoływana bezpośrednio w obiekcie z typem.</span><span class="sxs-lookup"><span data-stu-id="48e35-191">`GenericMethod` is called directly on the object with a type.</span></span> <span data-ttu-id="48e35-192">W poniższym przykładzie przyjęto założenie, że `GenericMethod` jest dostępny z przestrzeni nazw `JsInteropClasses`:</span><span class="sxs-lookup"><span data-stu-id="48e35-192">The following example assumes that the `GenericMethod` is available from the `JsInteropClasses` namespace:</span></span>

[!code-razor[](javascript-interop/samples_snapshot/component3.razor?highlight=17)]

## <a name="reference-elements-across-components"></a><span data-ttu-id="48e35-193">Elementy odniesienia między składnikami</span><span class="sxs-lookup"><span data-stu-id="48e35-193">Reference elements across components</span></span>

<span data-ttu-id="48e35-194">`ElementReference` jest gwarantowany tylko w metodzie `OnAfterRender` składnika (a odwołanie elementu jest `struct`), dlatego nie można przekazywać odwołania do elementu między składnikami.</span><span class="sxs-lookup"><span data-stu-id="48e35-194">An `ElementReference` is only guaranteed valid in a component's `OnAfterRender` method (and an element reference is a `struct`), so an element reference can't be passed between components.</span></span>

<span data-ttu-id="48e35-195">Aby składnik nadrzędny mógł udostępnić odwołanie do elementu innym składnikom, składnik nadrzędny może:</span><span class="sxs-lookup"><span data-stu-id="48e35-195">For a parent component to make an element reference available to other components, the parent component can:</span></span>

* <span data-ttu-id="48e35-196">Zezwalaj składnikom podrzędnym na rejestrowanie wywołań zwrotnych.</span><span class="sxs-lookup"><span data-stu-id="48e35-196">Allow child components to register callbacks.</span></span>
* <span data-ttu-id="48e35-197">Wywołaj zarejestrowane wywołania zwrotne podczas zdarzenia `OnAfterRender` z odwołaniem do elementu.</span><span class="sxs-lookup"><span data-stu-id="48e35-197">Invoke the registered callbacks during the `OnAfterRender` event with the passed element reference.</span></span> <span data-ttu-id="48e35-198">Pośrednio takie podejście umożliwia składnikom podrzędnym współdziałanie z odwołaniem do elementu nadrzędnego.</span><span class="sxs-lookup"><span data-stu-id="48e35-198">Indirectly, this approach allows child components to interact with the parent's element reference.</span></span>

<span data-ttu-id="48e35-199">Poniższy przykład Blazor webassembly ilustruje podejście.</span><span class="sxs-lookup"><span data-stu-id="48e35-199">The following Blazor WebAssembly example illustrates the approach.</span></span>

<span data-ttu-id="48e35-200">W `<head>` *wwwroot/index.html*:</span><span class="sxs-lookup"><span data-stu-id="48e35-200">In the `<head>` of *wwwroot/index.html*:</span></span>

```html
<style>
    .red { color: red }
</style>
```

<span data-ttu-id="48e35-201">W `<body>` *wwwroot/index.html*:</span><span class="sxs-lookup"><span data-stu-id="48e35-201">In the `<body>` of *wwwroot/index.html*:</span></span>

```html
<script>
    function setElementClass(element, className) {
        /** @type {HTMLElement} **/
        var myElement = element;
        myElement.classList.add(className);
    }
</script>
```

<span data-ttu-id="48e35-202">*Pages/index. Razor* (składnik nadrzędny):</span><span class="sxs-lookup"><span data-stu-id="48e35-202">*Pages/Index.razor* (parent component):</span></span>

```razor
@page "/"

<h1 @ref="_title">Hello, world!</h1>

Welcome to your new app.

<SurveyPrompt Parent="this" Title="How is Blazor working for you?" />
```

<span data-ttu-id="48e35-203">*Pages/index. Razor. cs*:</span><span class="sxs-lookup"><span data-stu-id="48e35-203">*Pages/Index.razor.cs*:</span></span>

```csharp
using System;
using System.Collections.Generic;
using Microsoft.AspNetCore.Components;

namespace BlazorSample.Pages
{
    public partial class Index : 
        ComponentBase, IObservable<ElementReference>, IDisposable
    {
        private bool _disposing;
        private IList<IObserver<ElementReference>> _subscriptions = 
            new List<IObserver<ElementReference>>();
        private ElementReference _title;

        protected override void OnAfterRender(bool firstRender)
        {
            base.OnAfterRender(firstRender);

            foreach (var subscription in _subscriptions)
            {
                try
                {
                    subscription.OnNext(_title);
                }
                catch (Exception)
                {
                    throw;
                }
            }
        }

        public void Dispose()
        {
            _disposing = true;

            foreach (var subscription in _subscriptions)
            {
                try
                {
                    subscription.OnCompleted();
                }
                catch (Exception)
                {
                }
            }

            _subscriptions.Clear();
        }

        public IDisposable Subscribe(IObserver<ElementReference> observer)
        {
            if (_disposing)
            {
                throw new InvalidOperationException("Parent being disposed");
            }

            _subscriptions.Add(observer);

            return new Subscription(observer, this);
        }

        private class Subscription : IDisposable
        {
            public Subscription(IObserver<ElementReference> observer, Index self)
            {
                Observer = observer;
                Self = self;
            }

            public IObserver<ElementReference> Observer { get; }
            public Index Self { get; }

            public void Dispose()
            {
                Self._subscriptions.Remove(Observer);
            }
        }
    }
}
```

<span data-ttu-id="48e35-204">*Shared/SurveyPrompt. Razor* (składnik podrzędny):</span><span class="sxs-lookup"><span data-stu-id="48e35-204">*Shared/SurveyPrompt.razor* (child component):</span></span>

```razor
@inject IJSRuntime JS

<div class="alert alert-secondary mt-4" role="alert">
    <span class="oi oi-pencil mr-2" aria-hidden="true"></span>
    <strong>@Title</strong>

    <span class="text-nowrap">
        Please take our
        <a target="_blank" class="font-weight-bold" 
            href="https://go.microsoft.com/fwlink/?linkid=2109206">brief survey</a>
    </span>
    and tell us what you think.
</div>

@code {
    [Parameter]
    public string Title { get; set; }
}
```

<span data-ttu-id="48e35-205">*Shared/SurveyPrompt. Razor. cs*:</span><span class="sxs-lookup"><span data-stu-id="48e35-205">*Shared/SurveyPrompt.razor.cs*:</span></span>

```csharp
using System;
using Microsoft.AspNetCore.Components;

namespace BlazorSample.Shared
{
    public partial class SurveyPrompt : 
        ComponentBase, IObserver<ElementReference>, IDisposable
    {
        private IDisposable _subscription = null;

        [Parameter]
        public IObservable<ElementReference> Parent { get; set; }

        protected override void OnParametersSet()
        {
            base.OnParametersSet();

            if (_subscription != null)
            {
                _subscription.Dispose();
            }

            _subscription = Parent.Subscribe(this);
        }

        public void OnCompleted()
        {
            _subscription = null;
        }

        public void OnError(Exception error)
        {
            _subscription = null;
        }

        public void OnNext(ElementReference value)
        {
            JS.InvokeAsync<object>(
                "setElementClass", new object[] { value, "red" });
        }

        public void Dispose()
        {
            _subscription?.Dispose();
        }
    }
}
```

## <a name="invoke-net-methods-from-javascript-functions"></a><span data-ttu-id="48e35-206">Wywoływanie metod .NET przy użyciu funkcji języka JavaScript</span><span class="sxs-lookup"><span data-stu-id="48e35-206">Invoke .NET methods from JavaScript functions</span></span>

### <a name="static-net-method-call"></a><span data-ttu-id="48e35-207">Statyczne wywołanie metody .NET</span><span class="sxs-lookup"><span data-stu-id="48e35-207">Static .NET method call</span></span>

<span data-ttu-id="48e35-208">Aby wywołać statyczną metodę .NET z poziomu języka JavaScript, użyj funkcji `DotNet.invokeMethod` lub `DotNet.invokeMethodAsync`.</span><span class="sxs-lookup"><span data-stu-id="48e35-208">To invoke a static .NET method from JavaScript, use the `DotNet.invokeMethod` or `DotNet.invokeMethodAsync` functions.</span></span> <span data-ttu-id="48e35-209">Przekaż identyfikator metody statycznej, która ma być wywoływana, nazwę zestawu zawierającego funkcję i wszelkie argumenty.</span><span class="sxs-lookup"><span data-stu-id="48e35-209">Pass in the identifier of the static method you wish to call, the name of the assembly containing the function, and any arguments.</span></span> <span data-ttu-id="48e35-210">Wersja asynchroniczna jest preferowana do obsługi scenariuszy serwera Blazor.</span><span class="sxs-lookup"><span data-stu-id="48e35-210">The asynchronous version is preferred to support Blazor Server scenarios.</span></span> <span data-ttu-id="48e35-211">Metoda .NET musi być publiczna, statyczna i mieć atrybut `[JSInvokable]`.</span><span class="sxs-lookup"><span data-stu-id="48e35-211">The .NET method must be public, static, and have the `[JSInvokable]` attribute.</span></span> <span data-ttu-id="48e35-212">Wywoływanie otwartych metod ogólnych nie jest obecnie obsługiwane.</span><span class="sxs-lookup"><span data-stu-id="48e35-212">Calling open generic methods isn't currently supported.</span></span>

<span data-ttu-id="48e35-213">Przykładowa aplikacja zawiera C# metodę zwracającą tablicę `int`s.</span><span class="sxs-lookup"><span data-stu-id="48e35-213">The sample app includes a C# method to return an array of `int`s.</span></span> <span data-ttu-id="48e35-214">Atrybut `JSInvokable` jest stosowany do metody.</span><span class="sxs-lookup"><span data-stu-id="48e35-214">The `JSInvokable` attribute is applied to the method.</span></span>

<span data-ttu-id="48e35-215">*Strony/JsInterop. Razor*:</span><span class="sxs-lookup"><span data-stu-id="48e35-215">*Pages/JsInterop.razor*:</span></span>

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

<span data-ttu-id="48e35-216">Kod JavaScript obsługiwany przez klienta wywołuje metodę C# .NET.</span><span class="sxs-lookup"><span data-stu-id="48e35-216">JavaScript served to the client invokes the C# .NET method.</span></span>

<span data-ttu-id="48e35-217">*wwwroot/exampleJsInterop. js*:</span><span class="sxs-lookup"><span data-stu-id="48e35-217">*wwwroot/exampleJsInterop.js*:</span></span>

[!code-javascript[](./common/samples/3.x/BlazorWebAssemblySample/wwwroot/exampleJsInterop.js?highlight=8-14)]

<span data-ttu-id="48e35-218">Gdy zostanie wybrany przycisk **ReturnArrayAsync Wyzwalaj metodę statyczną .NET** , sprawdź dane wyjściowe konsoli w narzędziach deweloperskich sieci Web w przeglądarce.</span><span class="sxs-lookup"><span data-stu-id="48e35-218">When the **Trigger .NET static method ReturnArrayAsync** button is selected, examine the console output in the browser's web developer tools.</span></span>

<span data-ttu-id="48e35-219">Dane wyjściowe konsoli są następujące:</span><span class="sxs-lookup"><span data-stu-id="48e35-219">The console output is:</span></span>

```console
Array(4) [ 1, 2, 3, 4 ]
```

<span data-ttu-id="48e35-220">Czwarta wartość tablicy jest wypychana do tablicy (`data.push(4);`) zwróconej przez `ReturnArrayAsync`.</span><span class="sxs-lookup"><span data-stu-id="48e35-220">The fourth array value is pushed to the array (`data.push(4);`) returned by `ReturnArrayAsync`.</span></span>

<span data-ttu-id="48e35-221">Domyślnie identyfikator metody jest nazwą metody, ale można określić inny identyfikator przy użyciu konstruktora `JSInvokableAttribute`:</span><span class="sxs-lookup"><span data-stu-id="48e35-221">By default, the method identifier is the method name, but you can specify a different identifier using the `JSInvokableAttribute` constructor:</span></span>

```csharp
@code {
    [JSInvokable("DifferentMethodName")]
    public static Task<int[]> ReturnArrayAsync()
    {
        return Task.FromResult(new int[] { 1, 2, 3 });
    }
}
```

<span data-ttu-id="48e35-222">W pliku JavaScript po stronie klienta:</span><span class="sxs-lookup"><span data-stu-id="48e35-222">In the client-side JavaScript file:</span></span>

```javascript
returnArrayAsyncJs: function () {
  DotNet.invokeMethodAsync('BlazorSample', 'DifferentMethodName')
    .then(data => {
      data.push(4);
      console.log(data);
    });
}
```

### <a name="instance-method-call"></a><span data-ttu-id="48e35-223">Wywołanie metody wystąpienia</span><span class="sxs-lookup"><span data-stu-id="48e35-223">Instance method call</span></span>

<span data-ttu-id="48e35-224">Można również wywołać metody wystąpienia platformy .NET z poziomu języka JavaScript.</span><span class="sxs-lookup"><span data-stu-id="48e35-224">You can also call .NET instance methods from JavaScript.</span></span> <span data-ttu-id="48e35-225">Aby wywołać metodę wystąpienia platformy .NET z poziomu języka JavaScript:</span><span class="sxs-lookup"><span data-stu-id="48e35-225">To invoke a .NET instance method from JavaScript:</span></span>

* <span data-ttu-id="48e35-226">Przekaż wystąpienie platformy .NET przez odwołanie do języka JavaScript:</span><span class="sxs-lookup"><span data-stu-id="48e35-226">Pass the .NET instance by reference to JavaScript:</span></span>
  * <span data-ttu-id="48e35-227">Utwórz wywołanie statyczne do `DotNetObjectReference.Create`.</span><span class="sxs-lookup"><span data-stu-id="48e35-227">Make a static call to `DotNetObjectReference.Create`.</span></span>
  * <span data-ttu-id="48e35-228">Zawiń wystąpienie w wystąpieniu `DotNetObjectReference` i Wywołaj `Create` w wystąpieniu `DotNetObjectReference`.</span><span class="sxs-lookup"><span data-stu-id="48e35-228">Wrap the instance in a `DotNetObjectReference` instance and call `Create` on the `DotNetObjectReference` instance.</span></span> <span data-ttu-id="48e35-229">Usuwanie `DotNetObjectReference` obiektów (przykład pojawia się w dalszej części tej sekcji).</span><span class="sxs-lookup"><span data-stu-id="48e35-229">Dispose of `DotNetObjectReference` objects (an example appears later in this section).</span></span>
* <span data-ttu-id="48e35-230">Wywołaj metody wystąpienia platformy .NET w wystąpieniu przy użyciu funkcji `invokeMethod` lub `invokeMethodAsync`.</span><span class="sxs-lookup"><span data-stu-id="48e35-230">Invoke .NET instance methods on the instance using the `invokeMethod` or `invokeMethodAsync` functions.</span></span> <span data-ttu-id="48e35-231">Wystąpienie programu .NET może być również przekazywać jako argument podczas wywoływania innych metod .NET z JavaScript.</span><span class="sxs-lookup"><span data-stu-id="48e35-231">The .NET instance can also be passed as an argument when invoking other .NET methods from JavaScript.</span></span>

> [!NOTE]
> <span data-ttu-id="48e35-232">Przykładowa aplikacja rejestruje komunikaty do konsoli po stronie klienta.</span><span class="sxs-lookup"><span data-stu-id="48e35-232">The sample app logs messages to the client-side console.</span></span> <span data-ttu-id="48e35-233">W poniższych przykładach zademonstrowanych przez przykładową aplikację można sprawdzić dane wyjściowe konsoli przeglądarki w narzędziach deweloperskich w przeglądarce.</span><span class="sxs-lookup"><span data-stu-id="48e35-233">For the following examples demonstrated by the sample app, examine the browser's console output in the browser's developer tools.</span></span>

<span data-ttu-id="48e35-234">Po wybraniu przycisku **Wyzwalaj metodę wystąpienia .NET HelloHelper. sayHello** , `ExampleJsInterop.CallHelloHelperSayHello` jest wywoływana i przekazuje nazwę, `Blazor`, do metody.</span><span class="sxs-lookup"><span data-stu-id="48e35-234">When the **Trigger .NET instance method HelloHelper.SayHello** button is selected, `ExampleJsInterop.CallHelloHelperSayHello` is called and passes a name, `Blazor`, to the method.</span></span>

<span data-ttu-id="48e35-235">*Strony/JsInterop. Razor*:</span><span class="sxs-lookup"><span data-stu-id="48e35-235">*Pages/JsInterop.razor*:</span></span>

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

<span data-ttu-id="48e35-236">`CallHelloHelperSayHello` wywołuje funkcję JavaScript `sayHello` z nowym wystąpieniem `HelloHelper`.</span><span class="sxs-lookup"><span data-stu-id="48e35-236">`CallHelloHelperSayHello` invokes the JavaScript function `sayHello` with a new instance of `HelloHelper`.</span></span>

<span data-ttu-id="48e35-237">*JsInteropClasses/ExampleJsInterop. cs*:</span><span class="sxs-lookup"><span data-stu-id="48e35-237">*JsInteropClasses/ExampleJsInterop.cs*:</span></span>

[!code-csharp[](./common/samples/3.x/BlazorWebAssemblySample/JsInteropClasses/ExampleJsInterop.cs?name=snippet1&highlight=10-16)]

<span data-ttu-id="48e35-238">*wwwroot/exampleJsInterop. js*:</span><span class="sxs-lookup"><span data-stu-id="48e35-238">*wwwroot/exampleJsInterop.js*:</span></span>

[!code-javascript[](./common/samples/3.x/BlazorWebAssemblySample/wwwroot/exampleJsInterop.js?highlight=15-18)]

<span data-ttu-id="48e35-239">Nazwa jest przenoszona do konstruktora `HelloHelper`, który ustawia właściwość `HelloHelper.Name`.</span><span class="sxs-lookup"><span data-stu-id="48e35-239">The name is passed to `HelloHelper`'s constructor, which sets the `HelloHelper.Name` property.</span></span> <span data-ttu-id="48e35-240">Gdy funkcja JavaScript `sayHello` jest wykonywana, `HelloHelper.SayHello` zwraca komunikat `Hello, {Name}!`, który jest zapisywana w konsoli przez funkcję JavaScript.</span><span class="sxs-lookup"><span data-stu-id="48e35-240">When the JavaScript function `sayHello` is executed, `HelloHelper.SayHello` returns the `Hello, {Name}!` message, which is written to the console by the JavaScript function.</span></span>

<span data-ttu-id="48e35-241">*JsInteropClasses/HelloHelper. cs*:</span><span class="sxs-lookup"><span data-stu-id="48e35-241">*JsInteropClasses/HelloHelper.cs*:</span></span>

[!code-csharp[](./common/samples/3.x/BlazorWebAssemblySample/JsInteropClasses/HelloHelper.cs?name=snippet1&highlight=5,10-11)]

<span data-ttu-id="48e35-242">Dane wyjściowe konsoli w narzędziach deweloperskich sieci Web w przeglądarce:</span><span class="sxs-lookup"><span data-stu-id="48e35-242">Console output in the browser's web developer tools:</span></span>

```console
Hello, Blazor!
```

<span data-ttu-id="48e35-243">Aby uniknąć przecieków pamięci i zezwolić na wyrzucanie elementów bezużytecznych w składniku, który tworzy `DotNetObjectReference`, Usuń obiekt z klasy, która utworzyła wystąpienie `DotNetObjectReference`:</span><span class="sxs-lookup"><span data-stu-id="48e35-243">To avoid a memory leak and allow garbage collection on a component that creates a `DotNetObjectReference`, dispose of the object in the class that created the `DotNetObjectReference` instance:</span></span>

```csharp
public class ExampleJsInterop : IDisposable
{
    private readonly IJSRuntime _jsRuntime;
    private DotNetObjectReference<HelloHelper> _objRef;

    public ExampleJsInterop(IJSRuntime jsRuntime)
    {
        _jsRuntime = jsRuntime;
    }

    public ValueTask<string> CallHelloHelperSayHello(string name)
    {
        _objRef = DotNetObjectReference.Create(new HelloHelper(name));

        return _jsRuntime.InvokeAsync<string>(
            "exampleJsFunctions.sayHello",
            _objRef);
    }

    public void Dispose()
    {
        _objRef?.Dispose();
    }
}
```
  
<span data-ttu-id="48e35-244">Poprzedni wzorzec przedstawiony w klasie `ExampleJsInterop` można również zaimplementować w składniku:</span><span class="sxs-lookup"><span data-stu-id="48e35-244">The preceding pattern shown in the `ExampleJsInterop` class can also be implemented in a component:</span></span>
  
```razor
@page "/JSInteropComponent"
@using BlazorSample.JsInteropClasses
@implements IDisposable
@inject IJSRuntime JSRuntime

<h1>JavaScript Interop</h1>

<button type="button" class="btn btn-primary" @onclick="TriggerNetInstanceMethod">
    Trigger .NET instance method HelloHelper.SayHello
</button>

@code {
    private DotNetObjectReference<HelloHelper> _objRef;

    public async Task TriggerNetInstanceMethod()
    {
        _objRef = DotNetObjectReference.Create(new HelloHelper("Blazor"));

        await JSRuntime.InvokeAsync<string>(
            "exampleJsFunctions.sayHello",
            _objRef);
    }

    public void Dispose()
    {
        _objRef?.Dispose();
    }
}
```

## <a name="share-interop-code-in-a-class-library"></a><span data-ttu-id="48e35-245">Udostępnianie kodu międzyoperacyjnego w bibliotece klas</span><span class="sxs-lookup"><span data-stu-id="48e35-245">Share interop code in a class library</span></span>

<span data-ttu-id="48e35-246">Kod międzyoperacyjny JS można uwzględnić w bibliotece klas, co umożliwia udostępnianie kodu w pakiecie NuGet.</span><span class="sxs-lookup"><span data-stu-id="48e35-246">JS interop code can be included in a class library, which allows you to share the code in a NuGet package.</span></span>

<span data-ttu-id="48e35-247">Biblioteka klas obsługuje Osadzanie zasobów JavaScript w skompilowanym zestawie.</span><span class="sxs-lookup"><span data-stu-id="48e35-247">The class library handles embedding JavaScript resources in the built assembly.</span></span> <span data-ttu-id="48e35-248">Pliki JavaScript są umieszczane w folderze *wwwroot* .</span><span class="sxs-lookup"><span data-stu-id="48e35-248">The JavaScript files are placed in the *wwwroot* folder.</span></span> <span data-ttu-id="48e35-249">Narzędzia te zajmują się osadzaniem zasobów podczas kompilowania biblioteki.</span><span class="sxs-lookup"><span data-stu-id="48e35-249">The tooling takes care of embedding the resources when the library is built.</span></span>

<span data-ttu-id="48e35-250">Skompilowany pakiet NuGet jest przywoływany w pliku projektu aplikacji w taki sam sposób, w jaki jest przywoływany każdy pakiet NuGet.</span><span class="sxs-lookup"><span data-stu-id="48e35-250">The built NuGet package is referenced in the app's project file the same way that any NuGet package is referenced.</span></span> <span data-ttu-id="48e35-251">Po przywróceniu pakietu kod aplikacji może być wywoływany w języku JavaScript, tak jakby C#był.</span><span class="sxs-lookup"><span data-stu-id="48e35-251">After the package is restored, app code can call into JavaScript as if it were C#.</span></span>

<span data-ttu-id="48e35-252">Aby uzyskać więcej informacji, zobacz <xref:blazor/class-libraries>.</span><span class="sxs-lookup"><span data-stu-id="48e35-252">For more information, see <xref:blazor/class-libraries>.</span></span>

## <a name="harden-js-interop-calls"></a><span data-ttu-id="48e35-253">Zabezpieczenia wywołań międzyoperacyjnych w ramach funkcjonalności JS</span><span class="sxs-lookup"><span data-stu-id="48e35-253">Harden JS interop calls</span></span>

<span data-ttu-id="48e35-254">Usługa JS Interop może zakończyć się niepowodzeniem z powodu błędów sieci i powinna być traktowana jako niezawodna.</span><span class="sxs-lookup"><span data-stu-id="48e35-254">JS interop may fail due to networking errors and should be treated as unreliable.</span></span> <span data-ttu-id="48e35-255">Domyślnie aplikacja serwerowa Blazor przeprowadziła limit czasu wywołań międzyoperacyjnych JS na serwerze po jednej minucie.</span><span class="sxs-lookup"><span data-stu-id="48e35-255">By default, a Blazor Server app times out JS interop calls on the server after one minute.</span></span> <span data-ttu-id="48e35-256">Jeśli aplikacja może tolerować bardziej agresywny limit czasu, na przykład 10 sekund, należy ustawić limit czasu przy użyciu jednej z następujących metod:</span><span class="sxs-lookup"><span data-stu-id="48e35-256">If an app can tolerate a more aggressive timeout, such as 10 seconds, set the timeout using one of the following approaches:</span></span>

* <span data-ttu-id="48e35-257">Globalnie w `Startup.ConfigureServices`Określ limit czasu:</span><span class="sxs-lookup"><span data-stu-id="48e35-257">Globally in `Startup.ConfigureServices`, specify the timeout:</span></span>

  ```csharp
  services.AddServerSideBlazor(
      options => options.JSInteropDefaultCallTimeout = TimeSpan.FromSeconds({SECONDS}));
  ```

* <span data-ttu-id="48e35-258">Dla wywołania w kodzie składnika pojedyncze wywołanie może określać limit czasu:</span><span class="sxs-lookup"><span data-stu-id="48e35-258">Per-invocation in component code, a single call can specify the timeout:</span></span>

  ```csharp
  var result = await JSRuntime.InvokeAsync<string>("MyJSOperation", 
      TimeSpan.FromSeconds({SECONDS}), new[] { "Arg1" });
  ```

<span data-ttu-id="48e35-259">Aby uzyskać więcej informacji na temat wyczerpania zasobów, zobacz <xref:security/blazor/server>.</span><span class="sxs-lookup"><span data-stu-id="48e35-259">For more information on resource exhaustion, see <xref:security/blazor/server>.</span></span>

## <a name="perform-large-data-transfers-in-opno-locblazor-server-apps"></a><span data-ttu-id="48e35-260">Wykonywanie dużych transferów danych w aplikacjach Blazor Server</span><span class="sxs-lookup"><span data-stu-id="48e35-260">Perform large data transfers in Blazor Server apps</span></span>

<span data-ttu-id="48e35-261">W niektórych scenariuszach należy przenieść duże ilości danych między językami JavaScript i Blazor.</span><span class="sxs-lookup"><span data-stu-id="48e35-261">In some scenarios, large amounts of data must be transferred between JavaScript and Blazor.</span></span> <span data-ttu-id="48e35-262">Zwykle duże transfery danych odbywają się w przypadku:</span><span class="sxs-lookup"><span data-stu-id="48e35-262">Typically, large data transfers occur when:</span></span>

* <span data-ttu-id="48e35-263">Interfejsy API systemu plików przeglądarki służą do przekazywania lub pobierania pliku.</span><span class="sxs-lookup"><span data-stu-id="48e35-263">Browser file system APIs are used to upload or download a file.</span></span>
* <span data-ttu-id="48e35-264">Wymagana jest współdziałanie z biblioteką innej firmy.</span><span class="sxs-lookup"><span data-stu-id="48e35-264">Interop with a third party library is required.</span></span>

<span data-ttu-id="48e35-265">W programie Blazor Server ograniczenie jest stosowane w celu uniemożliwienia przekazywania pojedynczych dużych komunikatów, które mogą powodować problemy z wydajnością.</span><span class="sxs-lookup"><span data-stu-id="48e35-265">In Blazor Server, a limitation is in place to prevent passing single large messages that may result in performance issues.</span></span>

<span data-ttu-id="48e35-266">Podczas tworzenia kodu, który przesyła dane między językami JavaScript i Blazor, należy wziąć pod uwagę następujące wskazówki:</span><span class="sxs-lookup"><span data-stu-id="48e35-266">Consider the following guidance when developing code that transfers data between JavaScript and Blazor:</span></span>

* <span data-ttu-id="48e35-267">Wydziel dane na mniejsze fragmenty i Wyślij segmenty danych sekwencyjnie, dopóki wszystkie dane nie zostaną odebrane przez serwer.</span><span class="sxs-lookup"><span data-stu-id="48e35-267">Slice the data into smaller pieces, and send the data segments sequentially until all of the data is received by the server.</span></span>
* <span data-ttu-id="48e35-268">Nie przydzielaj dużych obiektów w języku C# JavaScript i kodzie.</span><span class="sxs-lookup"><span data-stu-id="48e35-268">Don't allocate large objects in JavaScript and C# code.</span></span>
* <span data-ttu-id="48e35-269">Nie blokuj głównego wątku interfejsu użytkownika przez długie okresy podczas wysyłania lub otrzymywania danych.</span><span class="sxs-lookup"><span data-stu-id="48e35-269">Don't block the main UI thread for long periods when sending or receiving data.</span></span>
* <span data-ttu-id="48e35-270">Zwolnij wszystkie używane pamięci, gdy proces zostanie ukończony lub anulowany.</span><span class="sxs-lookup"><span data-stu-id="48e35-270">Free any memory consumed when the process is completed or cancelled.</span></span>
* <span data-ttu-id="48e35-271">Wymuś następujące dodatkowe wymagania dotyczące zabezpieczeń:</span><span class="sxs-lookup"><span data-stu-id="48e35-271">Enforce the following additional requirements for security purposes:</span></span>
  * <span data-ttu-id="48e35-272">Zadeklaruj maksymalny rozmiar pliku lub danych, który może zostać przesłany.</span><span class="sxs-lookup"><span data-stu-id="48e35-272">Declare the maximum file or data size that can be passed.</span></span>
  * <span data-ttu-id="48e35-273">Zadeklaruj minimalną szybkość przekazywania z klienta na serwerze.</span><span class="sxs-lookup"><span data-stu-id="48e35-273">Declare the minimum upload rate from the client to the server.</span></span>
* <span data-ttu-id="48e35-274">Po odebraniu danych przez serwer dane mogą być następujące:</span><span class="sxs-lookup"><span data-stu-id="48e35-274">After the data is received by the server, the data can be:</span></span>
  * <span data-ttu-id="48e35-275">Tymczasowo przechowywane w buforze pamięci do momentu zebrania wszystkich segmentów.</span><span class="sxs-lookup"><span data-stu-id="48e35-275">Temporarily stored in a memory buffer until all of the segments are collected.</span></span>
  * <span data-ttu-id="48e35-276">Wykorzystano natychmiast.</span><span class="sxs-lookup"><span data-stu-id="48e35-276">Consumed immediately.</span></span> <span data-ttu-id="48e35-277">Na przykład dane mogą być przechowywane bezpośrednio w bazie danych lub zapisywane na dysku w miarę odbierania poszczególnych segmentów.</span><span class="sxs-lookup"><span data-stu-id="48e35-277">For example, the data can be stored immediately in a database or written to disk as each segment is received.</span></span>

<span data-ttu-id="48e35-278">Następująca Klasa obiektu przekazującego plików obsługuje program JS Interop z klientem programu.</span><span class="sxs-lookup"><span data-stu-id="48e35-278">The following file uploader class handles JS interop with the client.</span></span> <span data-ttu-id="48e35-279">Klasa obiektu przekazującego używa programu JS Interop do:</span><span class="sxs-lookup"><span data-stu-id="48e35-279">The uploader class uses JS interop to:</span></span>

* <span data-ttu-id="48e35-280">Przesondowaj klienta, aby wysłał segment danych.</span><span class="sxs-lookup"><span data-stu-id="48e35-280">Poll the client to send a data segment.</span></span>
* <span data-ttu-id="48e35-281">Przerywaj transakcję, jeśli trwa sondowanie.</span><span class="sxs-lookup"><span data-stu-id="48e35-281">Abort the transaction if polling times out.</span></span>

```csharp
using System;
using System.Buffers;
using System.Collections.Generic;
using System.IO;
using System.Threading.Tasks;
using Microsoft.JSInterop;

public class FileUploader : IDisposable
{
    private readonly IJSRuntime _jsRuntime;
    private readonly int _segmentSize = 6144;
    private readonly int _maxBase64SegmentSize = 8192;
    private readonly DotNetObjectReference<FileUploader> _thisReference;
    private List<IMemoryOwner<byte>> _uploadedSegments = 
        new List<IMemoryOwner<byte>>();

    public FileUploader(IJSRuntime jsRuntime)
    {
        _jsRuntime = jsRuntime;
    }

    public async Task<Stream> ReceiveFile(string selector, int maxSize)
    {
        var fileSize = 
            await _jsRuntime.InvokeAsync<int>("getFileSize", selector);

        if (fileSize > maxSize)
        {
            return null;
        }

        var numberOfSegments = Math.Floor(fileSize / (double)_segmentSize) + 1;
        var lastSegmentBytes = 0;
        string base64EncodedSegment;

        for (var i = 0; i < numberOfSegments; i++)
        {
            try
            {
                base64EncodedSegment = 
                    await _jsRuntime.InvokeAsync<string>(
                        "receiveSegment", i, selector);

                if (base64EncodedSegment.Length < _maxBase64SegmentSize && 
                    i < numberOfSegments - 1)
                {
                    return null;
                }
            }
            catch
            {
                return null;
            }

          var current = MemoryPool<byte>.Shared.Rent(_segmentSize);

          if (!Convert.TryFromBase64String(base64EncodedSegment, 
              current.Memory.Slice(0, _segmentSize).Span, out lastSegmentBytes))
          {
              return null;
          }

          _uploadedSegments.Add(current);
        }

        var segments = _uploadedSegments;
        _uploadedSegments = null;

        return new SegmentedStream(segments, _segmentSize, lastSegmentBytes);
    }

    public void Dispose()
    {
        if (_uploadedSegments != null)
        {
            foreach (var segment in _uploadedSegments)
            {
                segment.Dispose();
            }
        }
    }
}
```

<span data-ttu-id="48e35-282">W poprzednim przykładzie:</span><span class="sxs-lookup"><span data-stu-id="48e35-282">In the preceding example:</span></span>

* <span data-ttu-id="48e35-283">`_maxBase64SegmentSize` jest ustawiona na `8192`, która jest obliczana na podstawie `_maxBase64SegmentSize = _segmentSize * 4 / 3`.</span><span class="sxs-lookup"><span data-stu-id="48e35-283">The `_maxBase64SegmentSize` is set to `8192`, which is calculated from `_maxBase64SegmentSize = _segmentSize * 4 / 3`.</span></span>
* <span data-ttu-id="48e35-284">Interfejsy API zarządzania pamięcią na niskim poziomie są używane do przechowywania segmentów pamięci na serwerze w `_uploadedSegments`.</span><span class="sxs-lookup"><span data-stu-id="48e35-284">Low level .NET Core memory management APIs are used to store the memory segments on the server in `_uploadedSegments`.</span></span>
* <span data-ttu-id="48e35-285">Metoda `ReceiveFile` służy do obsługi przekazywania za pomocą narzędzia JS Interop:</span><span class="sxs-lookup"><span data-stu-id="48e35-285">A `ReceiveFile` method is used to handle the upload through JS interop:</span></span>
  * <span data-ttu-id="48e35-286">Rozmiar pliku jest określany w bajtach za pomocą narzędzia JS Interop z `_jsRuntime.InvokeAsync<FileInfo>('getFileSize', selector)`.</span><span class="sxs-lookup"><span data-stu-id="48e35-286">The file size is determined in bytes through JS interop with `_jsRuntime.InvokeAsync<FileInfo>('getFileSize', selector)`.</span></span>
  * <span data-ttu-id="48e35-287">Liczba segmentów do odebrania jest obliczana i przechowywana w `numberOfSegments`.</span><span class="sxs-lookup"><span data-stu-id="48e35-287">The number of segments to receive are calculated and stored in `numberOfSegments`.</span></span>
  * <span data-ttu-id="48e35-288">Segmenty są żądane w pętli `for` za pomocą programu JS Interop z `_jsRuntime.InvokeAsync<string>('receiveSegment', i, selector)`.</span><span class="sxs-lookup"><span data-stu-id="48e35-288">The segments are requested in a `for` loop through JS interop with `_jsRuntime.InvokeAsync<string>('receiveSegment', i, selector)`.</span></span> <span data-ttu-id="48e35-289">Wszystkie segmenty, ale ostatnie muszą mieć 8 192 bajtów przed dekodowaniem.</span><span class="sxs-lookup"><span data-stu-id="48e35-289">All segments but the last must be 8,192 bytes before decoding.</span></span> <span data-ttu-id="48e35-290">Klient jest zmuszony do wydajnego wysyłania danych.</span><span class="sxs-lookup"><span data-stu-id="48e35-290">The client is forced to send the data in an efficient manner.</span></span>
  * <span data-ttu-id="48e35-291">Podczas każdego odebranego segmentu sprawdzane są operacje przed dekodowaniem w <xref:System.Convert.TryFromBase64String*>.</span><span class="sxs-lookup"><span data-stu-id="48e35-291">For each segment received, checks are performed before decoding with <xref:System.Convert.TryFromBase64String*>.</span></span>
  * <span data-ttu-id="48e35-292">Strumień z danymi jest zwracany jako nowy <xref:System.IO.Stream> (`SegmentedStream`) po zakończeniu przekazywania.</span><span class="sxs-lookup"><span data-stu-id="48e35-292">A stream with the data is returned as a new <xref:System.IO.Stream> (`SegmentedStream`) after the upload is complete.</span></span>

<span data-ttu-id="48e35-293">Klasa łańcucha segmentów uwidacznia listę segmentów jako niemożliwy do przeszukiwania <xref:System.IO.Stream>:</span><span class="sxs-lookup"><span data-stu-id="48e35-293">The segmented stream class exposes the list of segments as a readonly non-seekable <xref:System.IO.Stream>:</span></span>

```csharp
using System;
using System.Buffers;
using System.Collections.Generic;
using System.IO;

public class SegmentedStream : Stream
{
    private readonly ReadOnlySequence<byte> _sequence;
    private long _currentPosition = 0;

    public SegmentedStream(IList<IMemoryOwner<byte>> segments, int segmentSize, 
        int lastSegmentSize)
    {
        if (segments.Count == 1)
        {
            _sequence = new ReadOnlySequence<byte>(
                segments[0].Memory.Slice(0, lastSegmentSize));
            return;
        }

        var sequenceSegment = new BufferSegment<byte>(
            segments[0].Memory.Slice(0, segmentSize));
        var lastSegment = sequenceSegment;

        for (int i = 1; i < segments.Count; i++)
        {
            var isLastSegment = i + 1 == segments.Count;
            lastSegment = lastSegment.Append(segments[i].Memory.Slice(
                0, isLastSegment ? lastSegmentSize : segmentSize));
        }

        _sequence = new ReadOnlySequence<byte>(
            sequenceSegment, 0, lastSegment, lastSegmentSize);
    }

    public override long Position
    {
        get => throw new NotImplementedException();
        set => throw new NotImplementedException();
    }

    public override int Read(byte[] buffer, int offset, int count)
    {
        var bytesToWrite = (int)(_currentPosition + count < _sequence.Length ? 
            count : _sequence.Length - _currentPosition);
        var data = _sequence.Slice(_currentPosition, bytesToWrite);
        data.CopyTo(buffer.AsSpan(offset, bytesToWrite));
        _currentPosition += bytesToWrite;

        return bytesToWrite;
    }

    private class BufferSegment<T> : ReadOnlySequenceSegment<T>
    {
        public BufferSegment(ReadOnlyMemory<T> memory)
        {
            Memory = memory;
        }

        public BufferSegment<T> Append(ReadOnlyMemory<T> memory)
        {
            var segment = new BufferSegment<T>(memory)
            {
                RunningIndex = RunningIndex + Memory.Length
            };

            Next = segment;

            return segment;
        }
    }

    public override bool CanRead => true;

    public override bool CanSeek => false;

    public override bool CanWrite => false;

    public override long Length => throw new NotImplementedException();

    public override void Flush() => throw new NotImplementedException();

    public override long Seek(long offset, SeekOrigin origin) => 
        throw new NotImplementedException();

    public override void SetLength(long value) => 
        throw new NotImplementedException();

    public override void Write(byte[] buffer, int offset, int count) => 
        throw new NotImplementedException();
}
```

<span data-ttu-id="48e35-294">Poniższy kod implementuje funkcje języka JavaScript do odbierania danych:</span><span class="sxs-lookup"><span data-stu-id="48e35-294">The following code implements JavaScript functions to receive the data:</span></span>

```javascript
function getFileSize(selector) {
  const file = getFile(selector);
  return file.size;
}

async function receiveSegment(segmentNumber, selector) {
  const file = getFile(selector);
  var segments = getFileSegments(file);
  var index = segmentNumber * 6144;
  return await getNextChunk(file, index);
}

function getFile(selector) {
  const element = document.querySelector(selector);
  if (!element) {
    throw new Error('Invalid selector');
  }
  const files = element.files;
  if (!files || files.length === 0) {
    throw new Error(`Element ${elementId} doesn't contain any files.`);
  }
  const file = files[0];
  return file;
}

function getFileSegments(file) {
  const segments = Math.floor(size % 6144 === 0 ? size / 6144 : 1 + size / 6144);
  return segments;
}

async function getNextChunk(file, index) {
  const length = file.size - index <= 6144 ? file.size - index : 6144;
  const chunk = file.slice(index, index + length);
  index += length;
  const base64Chunk = await this.base64EncodeAsync(chunk);
  return { base64Chunk, index };
}

async function base64EncodeAsync(chunk) {
  const reader = new FileReader();
  const result = new Promise((resolve, reject) => {
    reader.addEventListener('load',
      () => {
        const base64Chunk = reader.result;
        const cleanChunk = 
          base64Chunk.replace('data:application/octet-stream;base64,', '');
        resolve(cleanChunk);
      },
      false);
    reader.addEventListener('error', reject);
  });
  reader.readAsDataURL(chunk);
  return result;
}
```

## <a name="additional-resources"></a><span data-ttu-id="48e35-295">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="48e35-295">Additional resources</span></span>

* [<span data-ttu-id="48e35-296">InteropComponent. Razor — przykład (repozytorium dotnet/AspNetCore w witrynie GitHub, 3,1 gałąź wydania)</span><span class="sxs-lookup"><span data-stu-id="48e35-296">InteropComponent.razor example (dotnet/AspNetCore GitHub repository, 3.1 release branch)</span></span>](https://github.com/dotnet/AspNetCore/blob/release/3.1/src/Components/test/testassets/BasicTestApp/InteropComponent.razor)
