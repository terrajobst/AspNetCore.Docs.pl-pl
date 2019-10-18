---
title: ASP.NET Core Blazor międzyoperacyjności JavaScript
author: guardrex
description: Dowiedz się, jak wywoływać funkcje języka JavaScript z technologii .NET i .NET z poziomu języka JavaScript w aplikacjach Blazor.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 10/15/2019
uid: blazor/javascript-interop
ms.openlocfilehash: a8c3a0951761faab1c11507834aeef2507388d71
ms.sourcegitcommit: ce2bfb01f2cc7dd83f8a97da0689d232c71bcdc4
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/17/2019
ms.locfileid: "72531126"
---
# <a name="aspnet-core-blazor-javascript-interop"></a><span data-ttu-id="038d2-103">ASP.NET Core Blazor międzyoperacyjności JavaScript</span><span class="sxs-lookup"><span data-stu-id="038d2-103">ASP.NET Core Blazor JavaScript interop</span></span>

<span data-ttu-id="038d2-104">[Javier Calvarro Nelson](https://github.com/javiercn), [Daniel Roth](https://github.com/danroth27)i [Luke](https://github.com/guardrex) Latham</span><span class="sxs-lookup"><span data-stu-id="038d2-104">By [Javier Calvarro Nelson](https://github.com/javiercn), [Daniel Roth](https://github.com/danroth27), and [Luke Latham](https://github.com/guardrex)</span></span>

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

<span data-ttu-id="038d2-105">Aplikacja Blazor może wywoływać funkcje języka JavaScript z technologii .NET i .NET z kodu JavaScript.</span><span class="sxs-lookup"><span data-stu-id="038d2-105">A Blazor app can invoke JavaScript functions from .NET and .NET methods from JavaScript code.</span></span>

<span data-ttu-id="038d2-106">[Wyświetlanie lub Pobieranie przykładowego kodu](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/blazor/common/samples/) ([jak pobrać](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="038d2-106">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/blazor/common/samples/) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="invoke-javascript-functions-from-net-methods"></a><span data-ttu-id="038d2-107">Wywoływanie funkcji języka JavaScript z metod .NET</span><span class="sxs-lookup"><span data-stu-id="038d2-107">Invoke JavaScript functions from .NET methods</span></span>

<span data-ttu-id="038d2-108">Istnieją przypadki, w których kod .NET jest wymagany do wywołania funkcji języka JavaScript.</span><span class="sxs-lookup"><span data-stu-id="038d2-108">There are times when .NET code is required to call a JavaScript function.</span></span> <span data-ttu-id="038d2-109">Na przykład wywołanie języka JavaScript może uwidaczniać możliwości przeglądarki lub funkcje z biblioteki JavaScript w aplikacji.</span><span class="sxs-lookup"><span data-stu-id="038d2-109">For example, a JavaScript call can expose browser capabilities or functionality from a JavaScript library to the app.</span></span>

<span data-ttu-id="038d2-110">Aby wywołać kod JavaScript z platformy .NET, użyj abstrakcji `IJSRuntime`.</span><span class="sxs-lookup"><span data-stu-id="038d2-110">To call into JavaScript from .NET, use the `IJSRuntime` abstraction.</span></span> <span data-ttu-id="038d2-111">Metoda `InvokeAsync<T>` przyjmuje identyfikator funkcji języka JavaScript, która ma zostać wywołana wraz z dowolną liczbą argumentów z możliwością serializacji JSON.</span><span class="sxs-lookup"><span data-stu-id="038d2-111">The `InvokeAsync<T>` method takes an identifier for the JavaScript function that you wish to invoke along with any number of JSON-serializable arguments.</span></span> <span data-ttu-id="038d2-112">Identyfikator funkcji jest względny w stosunku do zakresu globalnego (`window`).</span><span class="sxs-lookup"><span data-stu-id="038d2-112">The function identifier is relative to the global scope (`window`).</span></span> <span data-ttu-id="038d2-113">Jeśli chcesz wywołać `window.someScope.someFunction`, identyfikator jest `someScope.someFunction`.</span><span class="sxs-lookup"><span data-stu-id="038d2-113">If you wish to call `window.someScope.someFunction`, the identifier is `someScope.someFunction`.</span></span> <span data-ttu-id="038d2-114">Nie ma potrzeby rejestrowania funkcji przed jej wywołaniem.</span><span class="sxs-lookup"><span data-stu-id="038d2-114">There's no need to register the function before it's called.</span></span> <span data-ttu-id="038d2-115">Typ zwracany `T` musi być również możliwy do serializacji JSON.</span><span class="sxs-lookup"><span data-stu-id="038d2-115">The return type `T` must also be JSON serializable.</span></span>

<span data-ttu-id="038d2-116">Dla aplikacji serwera Blazor:</span><span class="sxs-lookup"><span data-stu-id="038d2-116">For Blazor Server apps:</span></span>

* <span data-ttu-id="038d2-117">Aplikacja serwera Blazor przetwarza wiele żądań użytkownika.</span><span class="sxs-lookup"><span data-stu-id="038d2-117">Multiple user requests are processed by the Blazor Server app.</span></span> <span data-ttu-id="038d2-118">Nie wywołuj `JSRuntime.Current` w składniku w celu wywołania funkcji JavaScript.</span><span class="sxs-lookup"><span data-stu-id="038d2-118">Don't call `JSRuntime.Current` in a component to invoke JavaScript functions.</span></span>
* <span data-ttu-id="038d2-119">Wstrzyknąć abstrakcyjność `IJSRuntime` i użyj wstrzykniętego obiektu do wystawienia wywołań międzyoperacyjnych języka JavaScript.</span><span class="sxs-lookup"><span data-stu-id="038d2-119">Inject the `IJSRuntime` abstraction and use the injected object to issue JavaScript interop calls.</span></span>
* <span data-ttu-id="038d2-120">Gdy aplikacja Blazor jest wstępnie renderowana, wywołanie do języka JavaScript nie jest możliwe, ponieważ połączenie z przeglądarką nie zostało nawiązane.</span><span class="sxs-lookup"><span data-stu-id="038d2-120">While a Blazor app is prerendering, calling into JavaScript isn't possible because a connection with the browser hasn't been established.</span></span> <span data-ttu-id="038d2-121">Aby uzyskać więcej informacji, zobacz sekcję [wykrywanie, kiedy aplikacja Blazor jest renderowana](#detect-when-a-blazor-app-is-prerendering) .</span><span class="sxs-lookup"><span data-stu-id="038d2-121">For more information, see the [Detect when a Blazor app is prerendering](#detect-when-a-blazor-app-is-prerendering) section.</span></span>

<span data-ttu-id="038d2-122">Poniższy przykład jest oparty na [dekoderze](https://developer.mozilla.org/docs/Web/API/TextDecoder), eksperymentalnym dekoderem JavaScript.</span><span class="sxs-lookup"><span data-stu-id="038d2-122">The following example is based on [TextDecoder](https://developer.mozilla.org/docs/Web/API/TextDecoder), an experimental JavaScript-based decoder.</span></span> <span data-ttu-id="038d2-123">W przykładzie pokazano, jak wywołać funkcję JavaScript z C# metody.</span><span class="sxs-lookup"><span data-stu-id="038d2-123">The example demonstrates how to invoke a JavaScript function from a C# method.</span></span> <span data-ttu-id="038d2-124">Funkcja JavaScript akceptuje tablicę bajtową z C# metody, dekoduje tablicę i zwraca tekst do składnika do wyświetlenia.</span><span class="sxs-lookup"><span data-stu-id="038d2-124">The JavaScript function accepts a byte array from a C# method, decodes the array, and returns the text to the component for display.</span></span>

<span data-ttu-id="038d2-125">Wewnątrz elementu `<head>` *wwwroot/index.html* (Blazor webassembly) lub *Pages/_Host. cshtml* (Blazor Server) podaj funkcję, która używa `TextDecoder` do zdekodowania przekazaną tablicę:</span><span class="sxs-lookup"><span data-stu-id="038d2-125">Inside the `<head>` element of *wwwroot/index.html* (Blazor WebAssembly) or *Pages/_Host.cshtml* (Blazor Server), provide a function that uses `TextDecoder` to decode a passed array:</span></span>

[!code-html[](javascript-interop/samples_snapshot/index-script.html)]

<span data-ttu-id="038d2-126">Kod JavaScript, taki jak kod przedstawiony w powyższym przykładzie, można również załadować z pliku JavaScript ( *. js*) z odwołaniem do pliku skryptu:</span><span class="sxs-lookup"><span data-stu-id="038d2-126">JavaScript code, such as the code shown in the preceding example, can also be loaded from a JavaScript file (*.js*) with a reference to the script file:</span></span>

```html
<script src="exampleJsInterop.js"></script>
```

<span data-ttu-id="038d2-127">Następujący składnik:</span><span class="sxs-lookup"><span data-stu-id="038d2-127">The following component:</span></span>

* <span data-ttu-id="038d2-128">Wywołuje funkcję JavaScript `ConvertArray` przy użyciu `JsRuntime`, gdy zostanie wybrany przycisk składnika (**Tablica konwersji**).</span><span class="sxs-lookup"><span data-stu-id="038d2-128">Invokes the `ConvertArray` JavaScript function using `JsRuntime` when a component button (**Convert Array**) is selected.</span></span>
* <span data-ttu-id="038d2-129">Po wywołaniu funkcji języka JavaScript przenoszona tablica jest konwertowana na ciąg.</span><span class="sxs-lookup"><span data-stu-id="038d2-129">After the JavaScript function is called, the passed array is converted into a string.</span></span> <span data-ttu-id="038d2-130">Ciąg jest zwracany do składnika do wyświetlenia.</span><span class="sxs-lookup"><span data-stu-id="038d2-130">The string is returned to the component for display.</span></span>

[!code-cshtml[](javascript-interop/samples_snapshot/call-js-example.razor?highlight=2,34-35)]

<span data-ttu-id="038d2-131">Aby użyć abstrakcji `IJSRuntime`, należy zastosować jedną z następujących metod:</span><span class="sxs-lookup"><span data-stu-id="038d2-131">To use the `IJSRuntime` abstraction, adopt any of the following approaches:</span></span>

* <span data-ttu-id="038d2-132">Wstrzyknąć abstrakcję `IJSRuntime` do składnika Razor ( *. Razor*):</span><span class="sxs-lookup"><span data-stu-id="038d2-132">Inject the `IJSRuntime` abstraction into the Razor component (*.razor*):</span></span>

  [!code-cshtml[](javascript-interop/samples_snapshot/inject-abstraction.razor?highlight=1)]

* <span data-ttu-id="038d2-133">Wstrzyknąć abstrakcję `IJSRuntime` do klasy ( *. cs*):</span><span class="sxs-lookup"><span data-stu-id="038d2-133">Inject the `IJSRuntime` abstraction into a class (*.cs*):</span></span>

  [!code-csharp[](javascript-interop/samples_snapshot/inject-abstraction-class.cs?highlight=5)]

* <span data-ttu-id="038d2-134">Aby można było wygenerować zawartość dynamiczną przy użyciu [BuildRenderTree](xref:blazor/components#manual-rendertreebuilder-logic), użyj atrybutu `[Inject]`:</span><span class="sxs-lookup"><span data-stu-id="038d2-134">For dynamic content generation with [BuildRenderTree](xref:blazor/components#manual-rendertreebuilder-logic), use the `[Inject]` attribute:</span></span>

  ```csharp
  [Inject]
  IJSRuntime JSRuntime { get; set; }
  ```

<span data-ttu-id="038d2-135">W aplikacji przykładowej po stronie klienta, która jest dołączona do tego tematu, dostępne są dwie funkcje języka JavaScript, które współdziałają z modelem DOM, aby odbierać dane wejściowe użytkownika i wyświetlać komunikat powitalny:</span><span class="sxs-lookup"><span data-stu-id="038d2-135">In the client-side sample app that accompanies this topic, two JavaScript functions are available to the app that interact with the DOM to receive user input and display a welcome message:</span></span>

* <span data-ttu-id="038d2-136">`showPrompt` &ndash; generuje monit o zaakceptowanie danych wprowadzonych przez użytkownika (nazwę użytkownika) i zwraca nazwę obiektu wywołującego.</span><span class="sxs-lookup"><span data-stu-id="038d2-136">`showPrompt` &ndash; Produces a prompt to accept user input (the user's name) and returns the name to the caller.</span></span>
* <span data-ttu-id="038d2-137">`displayWelcome` &ndash; przypisuje Komunikat powitalny od wywołującego do obiektu DOM z `id` z `welcome`.</span><span class="sxs-lookup"><span data-stu-id="038d2-137">`displayWelcome` &ndash; Assigns a welcome message from the caller to a DOM object with an `id` of `welcome`.</span></span>

<span data-ttu-id="038d2-138">*wwwroot/exampleJsInterop. js*:</span><span class="sxs-lookup"><span data-stu-id="038d2-138">*wwwroot/exampleJsInterop.js*:</span></span>

[!code-javascript[](./common/samples/3.x/BlazorWebAssemblySample/wwwroot/exampleJsInterop.js?highlight=2-7)]

<span data-ttu-id="038d2-139">Umieść tag `<script>`, który odwołuje się do pliku JavaScript w pliku *wwwroot/index.html* (Blazor webassembly) lub *Pages/_Host. cshtml* (Blazor Server).</span><span class="sxs-lookup"><span data-stu-id="038d2-139">Place the `<script>` tag that references the JavaScript file in the *wwwroot/index.html* file (Blazor WebAssembly) or *Pages/_Host.cshtml* file (Blazor Server).</span></span>

<span data-ttu-id="038d2-140">*wwwroot/index.html* (Blazor webassembly):</span><span class="sxs-lookup"><span data-stu-id="038d2-140">*wwwroot/index.html* (Blazor WebAssembly):</span></span>

[!code-html[](./common/samples/3.x/BlazorWebAssemblySample/wwwroot/index.html?highlight=15)]

<span data-ttu-id="038d2-141">*Pages/_Host. cshtml* (Blazor Server):</span><span class="sxs-lookup"><span data-stu-id="038d2-141">*Pages/_Host.cshtml* (Blazor Server):</span></span>

[!code-cshtml[](./common/samples/3.x/BlazorServerSample/Pages/_Host.cshtml?highlight=21)]

<span data-ttu-id="038d2-142">Nie umieszczaj znacznika `<script>` w pliku składnika, ponieważ nie można dynamicznie zaktualizować tagu `<script>`.</span><span class="sxs-lookup"><span data-stu-id="038d2-142">Don't place a `<script>` tag in a component file because the `<script>` tag can't be updated dynamically.</span></span>

<span data-ttu-id="038d2-143">.NET metod współdziałania z funkcjami JavaScript w pliku *exampleJsInterop. js* przez wywołanie `IJSRuntime.InvokeAsync<T>`.</span><span class="sxs-lookup"><span data-stu-id="038d2-143">.NET methods interop with the JavaScript functions in the *exampleJsInterop.js* file by calling `IJSRuntime.InvokeAsync<T>`.</span></span>

<span data-ttu-id="038d2-144">Abstrakcja `IJSRuntime` jest asynchroniczna, aby umożliwić obsługę scenariuszy serwera Blazor.</span><span class="sxs-lookup"><span data-stu-id="038d2-144">The `IJSRuntime` abstraction is asynchronous to allow for Blazor Server scenarios.</span></span> <span data-ttu-id="038d2-145">Jeśli aplikacja jest aplikacją webassembly Blazor i chcesz wywołać funkcję JavaScript synchronicznie, downcast do `IJSInProcessRuntime` i Wywołaj `Invoke<T>`.</span><span class="sxs-lookup"><span data-stu-id="038d2-145">If the app is a Blazor WebAssembly app and you want to invoke a JavaScript function synchronously, downcast to `IJSInProcessRuntime` and call `Invoke<T>` instead.</span></span> <span data-ttu-id="038d2-146">Zalecamy, aby większość bibliotek międzyoperacyjnych języka JavaScript używała asynchronicznych interfejsów API, aby upewnić się, że biblioteki są dostępne we wszystkich scenariuszach.</span><span class="sxs-lookup"><span data-stu-id="038d2-146">We recommend that most JavaScript interop libraries use the async APIs to ensure that the libraries are available in all scenarios.</span></span>

<span data-ttu-id="038d2-147">Przykładowa aplikacja zawiera składnik demonstrujący międzyoperacyjność JavaScript.</span><span class="sxs-lookup"><span data-stu-id="038d2-147">The sample app includes a component to demonstrate JavaScript interop.</span></span> <span data-ttu-id="038d2-148">Składnik:</span><span class="sxs-lookup"><span data-stu-id="038d2-148">The component:</span></span>

* <span data-ttu-id="038d2-149">Odbiera dane wprowadzane przez użytkownika za pośrednictwem wiersza polecenia języka JavaScript.</span><span class="sxs-lookup"><span data-stu-id="038d2-149">Receives user input via a JavaScript prompt.</span></span>
* <span data-ttu-id="038d2-150">Zwraca tekst do składnika do przetworzenia.</span><span class="sxs-lookup"><span data-stu-id="038d2-150">Returns the text to the component for processing.</span></span>
* <span data-ttu-id="038d2-151">Wywołuje drugą funkcję języka JavaScript, która współdziała z modelem DOM, aby wyświetlić komunikat powitalny.</span><span class="sxs-lookup"><span data-stu-id="038d2-151">Calls a second JavaScript function that interacts with the DOM to display a welcome message.</span></span>

<span data-ttu-id="038d2-152">*Strony/JSInterop. Razor*:</span><span class="sxs-lookup"><span data-stu-id="038d2-152">*Pages/JSInterop.razor*:</span></span>

[!code-cshtml[](./common/samples/3.x/BlazorWebAssemblySample/Pages/JsInterop.razor?name=snippet_JSInterop1&highlight=3,19-21,23-25)]

1. <span data-ttu-id="038d2-153">Gdy `TriggerJsPrompt` jest wykonywane przez wybranie przycisku **wyzwalacza skryptu JavaScript** składnika, funkcja JavaScript `showPrompt` dostępna w pliku *wwwroot/exampleJsInterop. js* jest wywoływana.</span><span class="sxs-lookup"><span data-stu-id="038d2-153">When `TriggerJsPrompt` is executed by selecting the component's **Trigger JavaScript Prompt** button, the JavaScript `showPrompt` function provided in the *wwwroot/exampleJsInterop.js* file is called.</span></span>
1. <span data-ttu-id="038d2-154">Funkcja `showPrompt` akceptuje dane wejściowe użytkownika (nazwę użytkownika), które są kodowane w formacie HTML i zwracane do składnika.</span><span class="sxs-lookup"><span data-stu-id="038d2-154">The `showPrompt` function accepts user input (the user's name), which is HTML-encoded and returned to the component.</span></span> <span data-ttu-id="038d2-155">Składnik przechowuje nazwę użytkownika w zmiennej lokalnej, `name`.</span><span class="sxs-lookup"><span data-stu-id="038d2-155">The component stores the user's name in a local variable, `name`.</span></span>
1. <span data-ttu-id="038d2-156">Ciąg przechowywany w `name` jest zawarty w komunikacie powitalnym, który jest przesyłany do funkcji języka JavaScript, `displayWelcome`, która renderuje Komunikat powitalny do znacznika nagłówka.</span><span class="sxs-lookup"><span data-stu-id="038d2-156">The string stored in `name` is incorporated into a welcome message, which is passed to a JavaScript function, `displayWelcome`, which renders the welcome message into a heading tag.</span></span>

## <a name="call-a-void-javascript-function"></a><span data-ttu-id="038d2-157">Wywoływanie funkcji języka JavaScript typu void</span><span class="sxs-lookup"><span data-stu-id="038d2-157">Call a void JavaScript function</span></span>

<span data-ttu-id="038d2-158">Funkcje języka JavaScript zwracające [wartość void (0)/void 0](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Operators/void) lub [undefined](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/undefined) są wywoływane z `IJSRuntime.InvokeVoidAsync`.</span><span class="sxs-lookup"><span data-stu-id="038d2-158">JavaScript functions that return [void(0)/void 0](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Operators/void) or [undefined](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/undefined) are called with `IJSRuntime.InvokeVoidAsync`.</span></span>

## <a name="detect-when-a-blazor-app-is-prerendering"></a><span data-ttu-id="038d2-159">Wykryj, kiedy aplikacja Blazor jest renderowana</span><span class="sxs-lookup"><span data-stu-id="038d2-159">Detect when a Blazor app is prerendering</span></span>
 
[!INCLUDE[](~/includes/blazor-prerendering.md)]

## <a name="capture-references-to-elements"></a><span data-ttu-id="038d2-160">Przechwyć odwołania do elementów</span><span class="sxs-lookup"><span data-stu-id="038d2-160">Capture references to elements</span></span>

<span data-ttu-id="038d2-161">Niektóre scenariusze [międzyoperacyjności języka JavaScript](xref:blazor/javascript-interop) wymagają odwołań do elementów HTML.</span><span class="sxs-lookup"><span data-stu-id="038d2-161">Some [JavaScript interop](xref:blazor/javascript-interop) scenarios require references to HTML elements.</span></span> <span data-ttu-id="038d2-162">Na przykład Biblioteka interfejsu użytkownika może wymagać odwołania do elementu dla inicjalizacji lub może być konieczne wywołanie interfejsów API, takich jak `focus` lub `play`.</span><span class="sxs-lookup"><span data-stu-id="038d2-162">For example, a UI library may require an element reference for initialization, or you might need to call command-like APIs on an element, such as `focus` or `play`.</span></span>

<span data-ttu-id="038d2-163">Przechwyć odwołania do elementów HTML w składniku, korzystając z następującej metody:</span><span class="sxs-lookup"><span data-stu-id="038d2-163">Capture references to HTML elements in a component using the following approach:</span></span>

* <span data-ttu-id="038d2-164">Dodaj atrybut `@ref` do elementu HTML.</span><span class="sxs-lookup"><span data-stu-id="038d2-164">Add an `@ref` attribute to the HTML element.</span></span>
* <span data-ttu-id="038d2-165">Zdefiniuj pole typu `ElementReference`, którego nazwa pasuje do wartości atrybutu `@ref`.</span><span class="sxs-lookup"><span data-stu-id="038d2-165">Define a field of type `ElementReference` whose name matches the value of the `@ref` attribute.</span></span>

<span data-ttu-id="038d2-166">Poniższy przykład pokazuje przechwytywanie odwołania do elementu `username` `<input>`:</span><span class="sxs-lookup"><span data-stu-id="038d2-166">The following example shows capturing a reference to the `username` `<input>` element:</span></span>

```cshtml
<input @ref="username" ... />

@code {
    ElementReference username;
}
```

> [!NOTE]
> <span data-ttu-id="038d2-167">**Nie** używaj przechwyconych odwołań do elementów jako sposobu wypełniania modelu DOM.</span><span class="sxs-lookup"><span data-stu-id="038d2-167">Do **not** use captured element references as a way of populating the DOM.</span></span> <span data-ttu-id="038d2-168">Wykonanie tej czynności może zakłócać model renderowania deklaratywnego.</span><span class="sxs-lookup"><span data-stu-id="038d2-168">Doing so may interfere with the declarative rendering model.</span></span>

<span data-ttu-id="038d2-169">W odniesieniu do kodu platformy .NET `ElementReference` jest nieprzezroczystym uchwytem.</span><span class="sxs-lookup"><span data-stu-id="038d2-169">As far as .NET code is concerned, an `ElementReference` is an opaque handle.</span></span> <span data-ttu-id="038d2-170">*Jedyną* czynnością, którą można wykonać przy użyciu `ElementReference`, jest przekazanie go do kodu JavaScript za pośrednictwem międzyoperacyjności języka JavaScript.</span><span class="sxs-lookup"><span data-stu-id="038d2-170">The *only* thing you can do with `ElementReference` is pass it through to JavaScript code via JavaScript interop.</span></span> <span data-ttu-id="038d2-171">Gdy to zrobisz, kod po stronie JavaScript odbiera wystąpienie `HTMLElement`, które może być używane z normalnymi interfejsami API modelu DOM.</span><span class="sxs-lookup"><span data-stu-id="038d2-171">When you do so, the JavaScript-side code receives an `HTMLElement` instance, which it can use with normal DOM APIs.</span></span>

<span data-ttu-id="038d2-172">Na przykład poniższy kod definiuje metodę rozszerzenia .NET, która umożliwia ustawienie fokusu na elemencie:</span><span class="sxs-lookup"><span data-stu-id="038d2-172">For example, the following code defines a .NET extension method that enables setting the focus on an element:</span></span>

<span data-ttu-id="038d2-173">*exampleJsInterop. js*:</span><span class="sxs-lookup"><span data-stu-id="038d2-173">*exampleJsInterop.js*:</span></span>

```javascript
window.exampleJsFunctions = {
  focusElement : function (element) {
    element.focus();
  }
}
```

<span data-ttu-id="038d2-174">Użyj `IJSRuntime.InvokeAsync<T>` i Wywołaj `exampleJsFunctions.focusElement` z `ElementReference`, aby skoncentrować element:</span><span class="sxs-lookup"><span data-stu-id="038d2-174">Use `IJSRuntime.InvokeAsync<T>` and call `exampleJsFunctions.focusElement` with an `ElementReference` to focus an element:</span></span>

[!code-cshtml[](javascript-interop/samples_snapshot/component1.razor?highlight=1,3,11-12)]

<span data-ttu-id="038d2-175">Aby użyć metody rozszerzenia do skoncentrowania się na elemencie, Utwórz statyczną metodę rozszerzenia, która odbiera wystąpienie `IJSRuntime`:</span><span class="sxs-lookup"><span data-stu-id="038d2-175">To use an extension method to focus an element, create a static extension method that receives the `IJSRuntime` instance:</span></span>

```csharp
public static Task Focus(this ElementReference elementRef, IJSRuntime jsRuntime)
{
    return jsRuntime.InvokeAsync<object>(
        "exampleJsFunctions.focusElement", elementRef);
}
```

<span data-ttu-id="038d2-176">Metoda jest wywoływana bezpośrednio dla obiektu.</span><span class="sxs-lookup"><span data-stu-id="038d2-176">The method is called directly on the object.</span></span> <span data-ttu-id="038d2-177">W poniższym przykładzie przyjęto założenie, że metoda static `Focus` jest dostępna z przestrzeni nazw `JsInteropClasses`:</span><span class="sxs-lookup"><span data-stu-id="038d2-177">The following example assumes that the static `Focus` method is available from the `JsInteropClasses` namespace:</span></span>

[!code-cshtml[](javascript-interop/samples_snapshot/component2.razor?highlight=1,4,12)]

> [!IMPORTANT]
> <span data-ttu-id="038d2-178">Zmienna `username` jest wypełniana tylko po wyrenderowaniu składnika.</span><span class="sxs-lookup"><span data-stu-id="038d2-178">The `username` variable is only populated after the component is rendered.</span></span> <span data-ttu-id="038d2-179">W przypadku niewypełnienia `ElementReference` do kodu JavaScript kod JavaScript otrzymuje wartość `null`.</span><span class="sxs-lookup"><span data-stu-id="038d2-179">If an unpopulated `ElementReference` is passed to JavaScript code, the JavaScript code receives a value of `null`.</span></span> <span data-ttu-id="038d2-180">Aby manipulować odwołaniami do elementów po zakończeniu renderowania składnika (aby ustawić początkowy fokus w elemencie), użyj [metod cyklu życia składnika](xref:blazor/components#lifecycle-methods)`OnAfterRenderAsync` lub `OnAfterRender`.</span><span class="sxs-lookup"><span data-stu-id="038d2-180">To manipulate element references after the component has finished rendering (to set the initial focus on an element) use the `OnAfterRenderAsync` or `OnAfterRender` [component lifecycle methods](xref:blazor/components#lifecycle-methods).</span></span>

## <a name="invoke-net-methods-from-javascript-functions"></a><span data-ttu-id="038d2-181">Wywoływanie metod .NET przy użyciu funkcji języka JavaScript</span><span class="sxs-lookup"><span data-stu-id="038d2-181">Invoke .NET methods from JavaScript functions</span></span>

### <a name="static-net-method-call"></a><span data-ttu-id="038d2-182">Statyczne wywołanie metody .NET</span><span class="sxs-lookup"><span data-stu-id="038d2-182">Static .NET method call</span></span>

<span data-ttu-id="038d2-183">Aby wywołać statyczną metodę .NET z poziomu języka JavaScript, użyj funkcji `DotNet.invokeMethod` lub `DotNet.invokeMethodAsync`.</span><span class="sxs-lookup"><span data-stu-id="038d2-183">To invoke a static .NET method from JavaScript, use the `DotNet.invokeMethod` or `DotNet.invokeMethodAsync` functions.</span></span> <span data-ttu-id="038d2-184">Przekaż identyfikator metody statycznej, która ma być wywoływana, nazwę zestawu zawierającego funkcję i wszelkie argumenty.</span><span class="sxs-lookup"><span data-stu-id="038d2-184">Pass in the identifier of the static method you wish to call, the name of the assembly containing the function, and any arguments.</span></span> <span data-ttu-id="038d2-185">Wersja asynchroniczna jest preferowana do obsługi scenariuszy serwera Blazor.</span><span class="sxs-lookup"><span data-stu-id="038d2-185">The asynchronous version is preferred to support Blazor Server scenarios.</span></span> <span data-ttu-id="038d2-186">Aby wywołać metodę .NET z języka JavaScript, metoda .NET musi być publiczna, statyczna i mieć atrybut `[JSInvokable]`.</span><span class="sxs-lookup"><span data-stu-id="038d2-186">To invoke a .NET method from JavaScript, the .NET method must be public, static, and have the `[JSInvokable]` attribute.</span></span> <span data-ttu-id="038d2-187">Domyślnie identyfikator metody jest nazwą metody, ale można określić inny identyfikator przy użyciu konstruktora `JSInvokableAttribute`.</span><span class="sxs-lookup"><span data-stu-id="038d2-187">By default, the method identifier is the method name, but you can specify a different identifier using the `JSInvokableAttribute` constructor.</span></span> <span data-ttu-id="038d2-188">Wywoływanie otwartych metod ogólnych nie jest obecnie obsługiwane.</span><span class="sxs-lookup"><span data-stu-id="038d2-188">Calling open generic methods isn't currently supported.</span></span>

<span data-ttu-id="038d2-189">Przykładowa aplikacja zawiera C# metodę zwracającą tablicę `int`S.</span><span class="sxs-lookup"><span data-stu-id="038d2-189">The sample app includes a C# method to return an array of `int`s.</span></span> <span data-ttu-id="038d2-190">Atrybut `JSInvokable` jest stosowany do metody.</span><span class="sxs-lookup"><span data-stu-id="038d2-190">The `JSInvokable` attribute is applied to the method.</span></span>

<span data-ttu-id="038d2-191">*Strony/JsInterop. Razor*:</span><span class="sxs-lookup"><span data-stu-id="038d2-191">*Pages/JsInterop.razor*:</span></span>

[!code-cshtml[](./common/samples/3.x/BlazorWebAssemblySample/Pages/JsInterop.razor?name=snippet_JSInterop2&highlight=7-11)]

<span data-ttu-id="038d2-192">Kod JavaScript obsługiwany przez klienta wywołuje metodę C# .NET.</span><span class="sxs-lookup"><span data-stu-id="038d2-192">JavaScript served to the client invokes the C# .NET method.</span></span>

<span data-ttu-id="038d2-193">*wwwroot/exampleJsInterop. js*:</span><span class="sxs-lookup"><span data-stu-id="038d2-193">*wwwroot/exampleJsInterop.js*:</span></span>

[!code-javascript[](./common/samples/3.x/BlazorWebAssemblySample/wwwroot/exampleJsInterop.js?highlight=8-14)]

<span data-ttu-id="038d2-194">Gdy zostanie wybrany przycisk **ReturnArrayAsync Wyzwalaj metodę statyczną .NET** , sprawdź dane wyjściowe konsoli w narzędziach deweloperskich sieci Web w przeglądarce.</span><span class="sxs-lookup"><span data-stu-id="038d2-194">When the **Trigger .NET static method ReturnArrayAsync** button is selected, examine the console output in the browser's web developer tools.</span></span>

<span data-ttu-id="038d2-195">Dane wyjściowe konsoli są następujące:</span><span class="sxs-lookup"><span data-stu-id="038d2-195">The console output is:</span></span>

```console
Array(4) [ 1, 2, 3, 4 ]
```

<span data-ttu-id="038d2-196">Czwarta wartość tablicy jest wypychana do tablicy (`data.push(4);`) zwróconej przez `ReturnArrayAsync`.</span><span class="sxs-lookup"><span data-stu-id="038d2-196">The fourth array value is pushed to the array (`data.push(4);`) returned by `ReturnArrayAsync`.</span></span>

### <a name="instance-method-call"></a><span data-ttu-id="038d2-197">Wywołanie metody wystąpienia</span><span class="sxs-lookup"><span data-stu-id="038d2-197">Instance method call</span></span>

<span data-ttu-id="038d2-198">Można również wywołać metody wystąpienia platformy .NET z poziomu języka JavaScript.</span><span class="sxs-lookup"><span data-stu-id="038d2-198">You can also call .NET instance methods from JavaScript.</span></span> <span data-ttu-id="038d2-199">Aby wywołać metodę wystąpienia platformy .NET z poziomu języka JavaScript:</span><span class="sxs-lookup"><span data-stu-id="038d2-199">To invoke a .NET instance method from JavaScript:</span></span>

* <span data-ttu-id="038d2-200">Przekaż wystąpienie programu .NET do języka JavaScript, zawijając je w wystąpieniu `DotNetObjectReference`.</span><span class="sxs-lookup"><span data-stu-id="038d2-200">Pass the .NET instance to JavaScript by wrapping it in a `DotNetObjectReference` instance.</span></span> <span data-ttu-id="038d2-201">Wystąpienie programu .NET jest przesyłane przez odwołanie do języka JavaScript.</span><span class="sxs-lookup"><span data-stu-id="038d2-201">The .NET instance is passed by reference to JavaScript.</span></span>
* <span data-ttu-id="038d2-202">Wywołaj metody wystąpienia platformy .NET w wystąpieniu przy użyciu funkcji `invokeMethod` lub `invokeMethodAsync`.</span><span class="sxs-lookup"><span data-stu-id="038d2-202">Invoke .NET instance methods on the instance using the `invokeMethod` or `invokeMethodAsync` functions.</span></span> <span data-ttu-id="038d2-203">Wystąpienie programu .NET może być również przekazywać jako argument podczas wywoływania innych metod .NET z JavaScript.</span><span class="sxs-lookup"><span data-stu-id="038d2-203">The .NET instance can also be passed as an argument when invoking other .NET methods from JavaScript.</span></span>

> [!NOTE]
> <span data-ttu-id="038d2-204">Przykładowa aplikacja rejestruje komunikaty do konsoli po stronie klienta.</span><span class="sxs-lookup"><span data-stu-id="038d2-204">The sample app logs messages to the client-side console.</span></span> <span data-ttu-id="038d2-205">W poniższych przykładach zademonstrowanych przez przykładową aplikację można sprawdzić dane wyjściowe konsoli przeglądarki w narzędziach deweloperskich w przeglądarce.</span><span class="sxs-lookup"><span data-stu-id="038d2-205">For the following examples demonstrated by the sample app, examine the browser's console output in the browser's developer tools.</span></span>

<span data-ttu-id="038d2-206">Po wybraniu przycisku **wyzwalanie metody wystąpienia platformy .NET HelloHelper. sayHello** , `ExampleJsInterop.CallHelloHelperSayHello` jest wywoływana i przekazuje nazwę, `Blazor` do metody.</span><span class="sxs-lookup"><span data-stu-id="038d2-206">When the **Trigger .NET instance method HelloHelper.SayHello** button is selected, `ExampleJsInterop.CallHelloHelperSayHello` is called and passes a name, `Blazor`, to the method.</span></span>

<span data-ttu-id="038d2-207">*Strony/JsInterop. Razor*:</span><span class="sxs-lookup"><span data-stu-id="038d2-207">*Pages/JsInterop.razor*:</span></span>

[!code-cshtml[](./common/samples/3.x/BlazorWebAssemblySample/Pages/JsInterop.razor?name=snippet_JSInterop3&highlight=8-9)]

<span data-ttu-id="038d2-208">`CallHelloHelperSayHello` wywołuje funkcję JavaScript `sayHello` z nowym wystąpieniem `HelloHelper`.</span><span class="sxs-lookup"><span data-stu-id="038d2-208">`CallHelloHelperSayHello` invokes the JavaScript function `sayHello` with a new instance of `HelloHelper`.</span></span>

<span data-ttu-id="038d2-209">*JsInteropClasses/ExampleJsInterop. cs*:</span><span class="sxs-lookup"><span data-stu-id="038d2-209">*JsInteropClasses/ExampleJsInterop.cs*:</span></span>

[!code-csharp[](./common/samples/3.x/BlazorWebAssemblySample/JsInteropClasses/ExampleJsInterop.cs?name=snippet1&highlight=10-16)]

<span data-ttu-id="038d2-210">*wwwroot/exampleJsInterop. js*:</span><span class="sxs-lookup"><span data-stu-id="038d2-210">*wwwroot/exampleJsInterop.js*:</span></span>

[!code-javascript[](./common/samples/3.x/BlazorWebAssemblySample/wwwroot/exampleJsInterop.js?highlight=15-18)]

<span data-ttu-id="038d2-211">Nazwa jest przenoszona do konstruktora `HelloHelper`, który ustawia właściwość `HelloHelper.Name`.</span><span class="sxs-lookup"><span data-stu-id="038d2-211">The name is passed to `HelloHelper`'s constructor, which sets the `HelloHelper.Name` property.</span></span> <span data-ttu-id="038d2-212">Gdy funkcja JavaScript `sayHello` jest wykonywana, `HelloHelper.SayHello` zwróci komunikat `Hello, {Name}!`, który jest zapisywana w konsoli przez funkcję JavaScript.</span><span class="sxs-lookup"><span data-stu-id="038d2-212">When the JavaScript function `sayHello` is executed, `HelloHelper.SayHello` returns the `Hello, {Name}!` message, which is written to the console by the JavaScript function.</span></span>

<span data-ttu-id="038d2-213">*JsInteropClasses/HelloHelper. cs*:</span><span class="sxs-lookup"><span data-stu-id="038d2-213">*JsInteropClasses/HelloHelper.cs*:</span></span>

[!code-csharp[](./common/samples/3.x/BlazorWebAssemblySample/JsInteropClasses/HelloHelper.cs?name=snippet1&highlight=5,10-11)]

<span data-ttu-id="038d2-214">Dane wyjściowe konsoli w narzędziach deweloperskich sieci Web w przeglądarce:</span><span class="sxs-lookup"><span data-stu-id="038d2-214">Console output in the browser's web developer tools:</span></span>

```console
Hello, Blazor!
```

## <a name="share-interop-code-in-a-class-library"></a><span data-ttu-id="038d2-215">Udostępnianie kodu międzyoperacyjnego w bibliotece klas</span><span class="sxs-lookup"><span data-stu-id="038d2-215">Share interop code in a class library</span></span>

<span data-ttu-id="038d2-216">Kod międzyoperacyjny JavaScript może być uwzględniony w bibliotece klas, co umożliwia udostępnianie kodu w pakiecie NuGet.</span><span class="sxs-lookup"><span data-stu-id="038d2-216">JavaScript interop code can be included in a class library, which allows you to share the code in a NuGet package.</span></span>

<span data-ttu-id="038d2-217">Biblioteka klas obsługuje Osadzanie zasobów JavaScript w skompilowanym zestawie.</span><span class="sxs-lookup"><span data-stu-id="038d2-217">The class library handles embedding JavaScript resources in the built assembly.</span></span> <span data-ttu-id="038d2-218">Pliki JavaScript są umieszczane w folderze *wwwroot* .</span><span class="sxs-lookup"><span data-stu-id="038d2-218">The JavaScript files are placed in the *wwwroot* folder.</span></span> <span data-ttu-id="038d2-219">Narzędzia te zajmują się osadzaniem zasobów podczas kompilowania biblioteki.</span><span class="sxs-lookup"><span data-stu-id="038d2-219">The tooling takes care of embedding the resources when the library is built.</span></span>

<span data-ttu-id="038d2-220">Skompilowany pakiet NuGet jest przywoływany w pliku projektu aplikacji w taki sam sposób, w jaki jest przywoływany każdy pakiet NuGet.</span><span class="sxs-lookup"><span data-stu-id="038d2-220">The built NuGet package is referenced in the app's project file the same way that any NuGet package is referenced.</span></span> <span data-ttu-id="038d2-221">Po przywróceniu pakietu kod aplikacji może być wywoływany w języku JavaScript, tak jakby C#był.</span><span class="sxs-lookup"><span data-stu-id="038d2-221">After the package is restored, app code can call into JavaScript as if it were C#.</span></span>

<span data-ttu-id="038d2-222">Aby uzyskać więcej informacji, zobacz <xref:blazor/class-libraries>.</span><span class="sxs-lookup"><span data-stu-id="038d2-222">For more information, see <xref:blazor/class-libraries>.</span></span>

## <a name="harden-js-interop-calls"></a><span data-ttu-id="038d2-223">Zabezpieczenia wywołań międzyoperacyjnych w ramach funkcjonalności JS</span><span class="sxs-lookup"><span data-stu-id="038d2-223">Harden JS interop calls</span></span>

<span data-ttu-id="038d2-224">Usługa JS Interop może zakończyć się niepowodzeniem z powodu błędów sieci i powinna być traktowana jako niezawodna.</span><span class="sxs-lookup"><span data-stu-id="038d2-224">JS interop may fail due to networking errors and should be treated as unreliable.</span></span> <span data-ttu-id="038d2-225">Domyślnie aplikacja serwera Blazor przeprowadzi limit czasu wywołań międzyoperacyjnych JS na serwerze po jednej minucie.</span><span class="sxs-lookup"><span data-stu-id="038d2-225">By default, a Blazor Server app times out JS interop calls on the server after one minute.</span></span> <span data-ttu-id="038d2-226">Jeśli aplikacja może tolerować bardziej agresywny limit czasu, na przykład 10 sekund, należy ustawić limit czasu przy użyciu jednej z następujących metod:</span><span class="sxs-lookup"><span data-stu-id="038d2-226">If an app can tolerate a more aggressive timeout, such as 10 seconds, set the timeout using one of the following approaches:</span></span>

* <span data-ttu-id="038d2-227">Globalnie w `Startup.ConfigureServices` Określ limit czasu:</span><span class="sxs-lookup"><span data-stu-id="038d2-227">Globally in `Startup.ConfigureServices`, specify the timeout:</span></span>

  ```csharp
  services.AddServerSideBlazor(
      options => options.JSInteropDefaultCallTimeout = TimeSpan.FromSeconds({SECONDS}));
  ```

* <span data-ttu-id="038d2-228">Dla wywołania w kodzie składnika pojedyncze wywołanie może określać limit czasu:</span><span class="sxs-lookup"><span data-stu-id="038d2-228">Per-invocation in component code, a single call can specify the timeout:</span></span>

  ```csharp
  var result = await JSRuntime.InvokeAsync<string>("MyJSOperation", 
      TimeSpan.FromSeconds({SECONDS}), new[] { "Arg1" });
  ```

<span data-ttu-id="038d2-229">Aby uzyskać więcej informacji na temat wyczerpania zasobów, zobacz <xref:security/blazor/server>.</span><span class="sxs-lookup"><span data-stu-id="038d2-229">For more information on resource exhaustion, see <xref:security/blazor/server>.</span></span>
