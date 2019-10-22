---
title: ASP.NET Core Blazor międzyoperacyjności JavaScript
author: guardrex
description: Dowiedz się, jak wywoływać funkcje języka JavaScript z technologii .NET i .NET z poziomu języka JavaScript w aplikacjach Blazor.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 10/16/2019
uid: blazor/javascript-interop
ms.openlocfilehash: b157e16918975cd522318a02f21824d9a0198b11
ms.sourcegitcommit: eb4fcdeb2f9e8413117624de42841a4997d1d82d
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/21/2019
ms.locfileid: "72697922"
---
# <a name="aspnet-core-blazor-javascript-interop"></a><span data-ttu-id="73142-103">ASP.NET Core Blazor międzyoperacyjności JavaScript</span><span class="sxs-lookup"><span data-stu-id="73142-103">ASP.NET Core Blazor JavaScript interop</span></span>

<span data-ttu-id="73142-104">[Javier Calvarro Nelson](https://github.com/javiercn), [Daniel Roth](https://github.com/danroth27)i [Luke](https://github.com/guardrex) Latham</span><span class="sxs-lookup"><span data-stu-id="73142-104">By [Javier Calvarro Nelson](https://github.com/javiercn), [Daniel Roth](https://github.com/danroth27), and [Luke Latham](https://github.com/guardrex)</span></span>

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

<span data-ttu-id="73142-105">Aplikacja Blazor może wywoływać funkcje języka JavaScript z technologii .NET i .NET z kodu JavaScript.</span><span class="sxs-lookup"><span data-stu-id="73142-105">A Blazor app can invoke JavaScript functions from .NET and .NET methods from JavaScript code.</span></span>

<span data-ttu-id="73142-106">[Wyświetlanie lub Pobieranie przykładowego kodu](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/blazor/common/samples/) ([jak pobrać](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="73142-106">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/blazor/common/samples/) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="invoke-javascript-functions-from-net-methods"></a><span data-ttu-id="73142-107">Wywoływanie funkcji języka JavaScript z metod .NET</span><span class="sxs-lookup"><span data-stu-id="73142-107">Invoke JavaScript functions from .NET methods</span></span>

<span data-ttu-id="73142-108">Istnieją przypadki, w których kod .NET jest wymagany do wywołania funkcji języka JavaScript.</span><span class="sxs-lookup"><span data-stu-id="73142-108">There are times when .NET code is required to call a JavaScript function.</span></span> <span data-ttu-id="73142-109">Na przykład wywołanie języka JavaScript może uwidaczniać możliwości przeglądarki lub funkcje z biblioteki JavaScript w aplikacji.</span><span class="sxs-lookup"><span data-stu-id="73142-109">For example, a JavaScript call can expose browser capabilities or functionality from a JavaScript library to the app.</span></span>

<span data-ttu-id="73142-110">Aby wywołać kod JavaScript z platformy .NET, użyj abstrakcji `IJSRuntime`.</span><span class="sxs-lookup"><span data-stu-id="73142-110">To call into JavaScript from .NET, use the `IJSRuntime` abstraction.</span></span> <span data-ttu-id="73142-111">Metoda `InvokeAsync<T>` przyjmuje identyfikator funkcji języka JavaScript, która ma zostać wywołana wraz z dowolną liczbą argumentów z możliwością serializacji JSON.</span><span class="sxs-lookup"><span data-stu-id="73142-111">The `InvokeAsync<T>` method takes an identifier for the JavaScript function that you wish to invoke along with any number of JSON-serializable arguments.</span></span> <span data-ttu-id="73142-112">Identyfikator funkcji jest względny w stosunku do zakresu globalnego (`window`).</span><span class="sxs-lookup"><span data-stu-id="73142-112">The function identifier is relative to the global scope (`window`).</span></span> <span data-ttu-id="73142-113">Jeśli chcesz wywołać `window.someScope.someFunction`, identyfikator jest `someScope.someFunction`.</span><span class="sxs-lookup"><span data-stu-id="73142-113">If you wish to call `window.someScope.someFunction`, the identifier is `someScope.someFunction`.</span></span> <span data-ttu-id="73142-114">Nie ma potrzeby rejestrowania funkcji przed jej wywołaniem.</span><span class="sxs-lookup"><span data-stu-id="73142-114">There's no need to register the function before it's called.</span></span> <span data-ttu-id="73142-115">Typ zwracany `T` musi być również możliwy do serializacji JSON.</span><span class="sxs-lookup"><span data-stu-id="73142-115">The return type `T` must also be JSON serializable.</span></span>

<span data-ttu-id="73142-116">Dla aplikacji serwera Blazor:</span><span class="sxs-lookup"><span data-stu-id="73142-116">For Blazor Server apps:</span></span>

* <span data-ttu-id="73142-117">Aplikacja serwera Blazor przetwarza wiele żądań użytkownika.</span><span class="sxs-lookup"><span data-stu-id="73142-117">Multiple user requests are processed by the Blazor Server app.</span></span> <span data-ttu-id="73142-118">Nie wywołuj `JSRuntime.Current` w składniku w celu wywołania funkcji JavaScript.</span><span class="sxs-lookup"><span data-stu-id="73142-118">Don't call `JSRuntime.Current` in a component to invoke JavaScript functions.</span></span>
* <span data-ttu-id="73142-119">Wstrzyknąć abstrakcyjność `IJSRuntime` i użyj wstrzykniętego obiektu do wystawienia wywołań międzyoperacyjnych języka JavaScript.</span><span class="sxs-lookup"><span data-stu-id="73142-119">Inject the `IJSRuntime` abstraction and use the injected object to issue JavaScript interop calls.</span></span>
* <span data-ttu-id="73142-120">Gdy aplikacja Blazor jest wstępnie renderowana, wywołanie do języka JavaScript nie jest możliwe, ponieważ połączenie z przeglądarką nie zostało nawiązane.</span><span class="sxs-lookup"><span data-stu-id="73142-120">While a Blazor app is prerendering, calling into JavaScript isn't possible because a connection with the browser hasn't been established.</span></span> <span data-ttu-id="73142-121">Aby uzyskać więcej informacji, zobacz sekcję [wykrywanie, kiedy aplikacja Blazor jest renderowana](#detect-when-a-blazor-app-is-prerendering) .</span><span class="sxs-lookup"><span data-stu-id="73142-121">For more information, see the [Detect when a Blazor app is prerendering](#detect-when-a-blazor-app-is-prerendering) section.</span></span>

<span data-ttu-id="73142-122">Poniższy przykład jest oparty na [dekoderze](https://developer.mozilla.org/docs/Web/API/TextDecoder), eksperymentalnym dekoderem JavaScript.</span><span class="sxs-lookup"><span data-stu-id="73142-122">The following example is based on [TextDecoder](https://developer.mozilla.org/docs/Web/API/TextDecoder), an experimental JavaScript-based decoder.</span></span> <span data-ttu-id="73142-123">W przykładzie pokazano, jak wywołać funkcję JavaScript z C# metody.</span><span class="sxs-lookup"><span data-stu-id="73142-123">The example demonstrates how to invoke a JavaScript function from a C# method.</span></span> <span data-ttu-id="73142-124">Funkcja JavaScript akceptuje tablicę bajtową z C# metody, dekoduje tablicę i zwraca tekst do składnika do wyświetlenia.</span><span class="sxs-lookup"><span data-stu-id="73142-124">The JavaScript function accepts a byte array from a C# method, decodes the array, and returns the text to the component for display.</span></span>

<span data-ttu-id="73142-125">Wewnątrz elementu `<head>` *wwwroot/index.html* (Blazor webassembly) lub *Pages/_Host. cshtml* (Blazor Server), podaj funkcję języka JavaScript, która używa `TextDecoder` do dekodowania przekazaną tablicę i zwracają zdekodowaną wartość:</span><span class="sxs-lookup"><span data-stu-id="73142-125">Inside the `<head>` element of *wwwroot/index.html* (Blazor WebAssembly) or *Pages/_Host.cshtml* (Blazor Server), provide a JavaScript function that uses `TextDecoder` to decode a passed array and return the decoded value:</span></span>

[!code-html[](javascript-interop/samples_snapshot/index-script-convertarray.html)]

<span data-ttu-id="73142-126">Kod JavaScript, taki jak kod przedstawiony w powyższym przykładzie, można również załadować z pliku JavaScript ( *. js*) z odwołaniem do pliku skryptu:</span><span class="sxs-lookup"><span data-stu-id="73142-126">JavaScript code, such as the code shown in the preceding example, can also be loaded from a JavaScript file (*.js*) with a reference to the script file:</span></span>

```html
<script src="exampleJsInterop.js"></script>
```

<span data-ttu-id="73142-127">Następujący składnik:</span><span class="sxs-lookup"><span data-stu-id="73142-127">The following component:</span></span>

* <span data-ttu-id="73142-128">Wywołuje funkcję `convertArray` JavaScript przy użyciu `JSRuntime`, gdy zostanie wybrany przycisk składnika (**Konwertuj tablicę**).</span><span class="sxs-lookup"><span data-stu-id="73142-128">Invokes the `convertArray` JavaScript function using `JSRuntime` when a component button (**Convert Array**) is selected.</span></span>
* <span data-ttu-id="73142-129">Po wywołaniu funkcji języka JavaScript przenoszona tablica jest konwertowana na ciąg.</span><span class="sxs-lookup"><span data-stu-id="73142-129">After the JavaScript function is called, the passed array is converted into a string.</span></span> <span data-ttu-id="73142-130">Ciąg jest zwracany do składnika do wyświetlenia.</span><span class="sxs-lookup"><span data-stu-id="73142-130">The string is returned to the component for display.</span></span>

[!code-cshtml[](javascript-interop/samples_snapshot/call-js-example.razor?highlight=2,34-35)]

##  <a name="use-of-ijsruntime"></a><span data-ttu-id="73142-131">Korzystanie z IJSRuntime</span><span class="sxs-lookup"><span data-stu-id="73142-131">Use of IJSRuntime</span></span>

<span data-ttu-id="73142-132">Aby użyć abstrakcji `IJSRuntime`, należy zastosować jedną z następujących metod:</span><span class="sxs-lookup"><span data-stu-id="73142-132">To use the `IJSRuntime` abstraction, adopt any of the following approaches:</span></span>

* <span data-ttu-id="73142-133">Wstrzyknąć abstrakcję `IJSRuntime` do składnika Razor ( *. Razor*):</span><span class="sxs-lookup"><span data-stu-id="73142-133">Inject the `IJSRuntime` abstraction into the Razor component (*.razor*):</span></span>

  [!code-cshtml[](javascript-interop/samples_snapshot/inject-abstraction.razor?highlight=1)]

  <span data-ttu-id="73142-134">Wewnątrz elementu `<head>` *wwwroot/index.html* (Blazor webassembly) lub *Pages/_Host. cshtml* (Blazor Server) podaj `handleTickerChanged` funkcję JavaScript.</span><span class="sxs-lookup"><span data-stu-id="73142-134">Inside the `<head>` element of *wwwroot/index.html* (Blazor WebAssembly) or *Pages/_Host.cshtml* (Blazor Server), provide a `handleTickerChanged` JavaScript function.</span></span> <span data-ttu-id="73142-135">Funkcja jest wywoływana z `IJSRuntime.InvokeVoidAsync` i nie zwraca wartości:</span><span class="sxs-lookup"><span data-stu-id="73142-135">The function is called with `IJSRuntime.InvokeVoidAsync` and doesn't return a value:</span></span>

  [!code-html[](javascript-interop/samples_snapshot/index-script-handleTickerChanged1.html)]

* <span data-ttu-id="73142-136">Wstrzyknąć abstrakcję `IJSRuntime` do klasy ( *. cs*):</span><span class="sxs-lookup"><span data-stu-id="73142-136">Inject the `IJSRuntime` abstraction into a class (*.cs*):</span></span>

  [!code-csharp[](javascript-interop/samples_snapshot/inject-abstraction-class.cs?highlight=5)]

  <span data-ttu-id="73142-137">Wewnątrz elementu `<head>` *wwwroot/index.html* (Blazor webassembly) lub *Pages/_Host. cshtml* (Blazor Server) podaj `handleTickerChanged` funkcję JavaScript.</span><span class="sxs-lookup"><span data-stu-id="73142-137">Inside the `<head>` element of *wwwroot/index.html* (Blazor WebAssembly) or *Pages/_Host.cshtml* (Blazor Server), provide a `handleTickerChanged` JavaScript function.</span></span> <span data-ttu-id="73142-138">Funkcja jest wywoływana z `JSRuntime.InvokeAsync` i zwraca wartość:</span><span class="sxs-lookup"><span data-stu-id="73142-138">The function is called with `JSRuntime.InvokeAsync` and returns a value:</span></span>

  [!code-html[](javascript-interop/samples_snapshot/index-script-handleTickerChanged2.html)]

* <span data-ttu-id="73142-139">Aby można było wygenerować zawartość dynamiczną przy użyciu [BuildRenderTree](xref:blazor/components#manual-rendertreebuilder-logic), użyj atrybutu `[Inject]`:</span><span class="sxs-lookup"><span data-stu-id="73142-139">For dynamic content generation with [BuildRenderTree](xref:blazor/components#manual-rendertreebuilder-logic), use the `[Inject]` attribute:</span></span>

  ```csharp
  [Inject]
  IJSRuntime JSRuntime { get; set; }
  ```

<span data-ttu-id="73142-140">W aplikacji przykładowej po stronie klienta, która jest dołączona do tego tematu, dostępne są dwie funkcje języka JavaScript, które współdziałają z modelem DOM, aby odbierać dane wejściowe użytkownika i wyświetlać komunikat powitalny:</span><span class="sxs-lookup"><span data-stu-id="73142-140">In the client-side sample app that accompanies this topic, two JavaScript functions are available to the app that interact with the DOM to receive user input and display a welcome message:</span></span>

* <span data-ttu-id="73142-141">`showPrompt` &ndash; generuje monit o zaakceptowanie danych wprowadzonych przez użytkownika (nazwę użytkownika) i zwraca nazwę obiektu wywołującego.</span><span class="sxs-lookup"><span data-stu-id="73142-141">`showPrompt` &ndash; Produces a prompt to accept user input (the user's name) and returns the name to the caller.</span></span>
* <span data-ttu-id="73142-142">`displayWelcome` &ndash; przypisuje Komunikat powitalny od wywołującego do obiektu DOM z `id` z `welcome`.</span><span class="sxs-lookup"><span data-stu-id="73142-142">`displayWelcome` &ndash; Assigns a welcome message from the caller to a DOM object with an `id` of `welcome`.</span></span>

<span data-ttu-id="73142-143">*wwwroot/exampleJsInterop. js*:</span><span class="sxs-lookup"><span data-stu-id="73142-143">*wwwroot/exampleJsInterop.js*:</span></span>

[!code-javascript[](./common/samples/3.x/BlazorWebAssemblySample/wwwroot/exampleJsInterop.js?highlight=2-7)]

<span data-ttu-id="73142-144">Umieść tag `<script>`, który odwołuje się do pliku JavaScript w pliku *wwwroot/index.html* (Blazor webassembly) lub *Pages/_Host. cshtml* (Blazor Server).</span><span class="sxs-lookup"><span data-stu-id="73142-144">Place the `<script>` tag that references the JavaScript file in the *wwwroot/index.html* file (Blazor WebAssembly) or *Pages/_Host.cshtml* file (Blazor Server).</span></span>

<span data-ttu-id="73142-145">*wwwroot/index.html* (Blazor webassembly):</span><span class="sxs-lookup"><span data-stu-id="73142-145">*wwwroot/index.html* (Blazor WebAssembly):</span></span>

[!code-html[](./common/samples/3.x/BlazorWebAssemblySample/wwwroot/index.html?highlight=15)]

<span data-ttu-id="73142-146">*Pages/_Host. cshtml* (Blazor Server):</span><span class="sxs-lookup"><span data-stu-id="73142-146">*Pages/_Host.cshtml* (Blazor Server):</span></span>

[!code-cshtml[](./common/samples/3.x/BlazorServerSample/Pages/_Host.cshtml?highlight=21)]

<span data-ttu-id="73142-147">Nie umieszczaj znacznika `<script>` w pliku składnika, ponieważ nie można dynamicznie zaktualizować tagu `<script>`.</span><span class="sxs-lookup"><span data-stu-id="73142-147">Don't place a `<script>` tag in a component file because the `<script>` tag can't be updated dynamically.</span></span>

<span data-ttu-id="73142-148">.NET metod współdziałania z funkcjami JavaScript w pliku *exampleJsInterop. js* przez wywołanie `IJSRuntime.InvokeAsync<T>`.</span><span class="sxs-lookup"><span data-stu-id="73142-148">.NET methods interop with the JavaScript functions in the *exampleJsInterop.js* file by calling `IJSRuntime.InvokeAsync<T>`.</span></span>

<span data-ttu-id="73142-149">Abstrakcja `IJSRuntime` jest asynchroniczna, aby umożliwić obsługę scenariuszy serwera Blazor.</span><span class="sxs-lookup"><span data-stu-id="73142-149">The `IJSRuntime` abstraction is asynchronous to allow for Blazor Server scenarios.</span></span> <span data-ttu-id="73142-150">Jeśli aplikacja jest aplikacją webassembly Blazor i chcesz wywołać funkcję JavaScript synchronicznie, downcast do `IJSInProcessRuntime` i Wywołaj `Invoke<T>`.</span><span class="sxs-lookup"><span data-stu-id="73142-150">If the app is a Blazor WebAssembly app and you want to invoke a JavaScript function synchronously, downcast to `IJSInProcessRuntime` and call `Invoke<T>` instead.</span></span> <span data-ttu-id="73142-151">Zalecamy, aby większość bibliotek międzyoperacyjnych języka JavaScript używała asynchronicznych interfejsów API, aby upewnić się, że biblioteki są dostępne we wszystkich scenariuszach.</span><span class="sxs-lookup"><span data-stu-id="73142-151">We recommend that most JavaScript interop libraries use the async APIs to ensure that the libraries are available in all scenarios.</span></span>

<span data-ttu-id="73142-152">Przykładowa aplikacja zawiera składnik demonstrujący międzyoperacyjność JavaScript.</span><span class="sxs-lookup"><span data-stu-id="73142-152">The sample app includes a component to demonstrate JavaScript interop.</span></span> <span data-ttu-id="73142-153">Składnik:</span><span class="sxs-lookup"><span data-stu-id="73142-153">The component:</span></span>

* <span data-ttu-id="73142-154">Odbiera dane wprowadzane przez użytkownika za pośrednictwem wiersza polecenia języka JavaScript.</span><span class="sxs-lookup"><span data-stu-id="73142-154">Receives user input via a JavaScript prompt.</span></span>
* <span data-ttu-id="73142-155">Zwraca tekst do składnika do przetworzenia.</span><span class="sxs-lookup"><span data-stu-id="73142-155">Returns the text to the component for processing.</span></span>
* <span data-ttu-id="73142-156">Wywołuje drugą funkcję języka JavaScript, która współdziała z modelem DOM, aby wyświetlić komunikat powitalny.</span><span class="sxs-lookup"><span data-stu-id="73142-156">Calls a second JavaScript function that interacts with the DOM to display a welcome message.</span></span>

<span data-ttu-id="73142-157">*Strony/JSInterop. Razor*:</span><span class="sxs-lookup"><span data-stu-id="73142-157">*Pages/JSInterop.razor*:</span></span>

[!code-cshtml[](./common/samples/3.x/BlazorWebAssemblySample/Pages/JsInterop.razor?name=snippet_JSInterop1&highlight=3,19-21,23-25)]

1. <span data-ttu-id="73142-158">Gdy `TriggerJsPrompt` jest wykonywane przez wybranie przycisku **wyzwalacza skryptu JavaScript** składnika, funkcja JavaScript `showPrompt` dostępna w pliku *wwwroot/exampleJsInterop. js* jest wywoływana.</span><span class="sxs-lookup"><span data-stu-id="73142-158">When `TriggerJsPrompt` is executed by selecting the component's **Trigger JavaScript Prompt** button, the JavaScript `showPrompt` function provided in the *wwwroot/exampleJsInterop.js* file is called.</span></span>
1. <span data-ttu-id="73142-159">Funkcja `showPrompt` akceptuje dane wejściowe użytkownika (nazwę użytkownika), które są kodowane w formacie HTML i zwracane do składnika.</span><span class="sxs-lookup"><span data-stu-id="73142-159">The `showPrompt` function accepts user input (the user's name), which is HTML-encoded and returned to the component.</span></span> <span data-ttu-id="73142-160">Składnik przechowuje nazwę użytkownika w zmiennej lokalnej, `name`.</span><span class="sxs-lookup"><span data-stu-id="73142-160">The component stores the user's name in a local variable, `name`.</span></span>
1. <span data-ttu-id="73142-161">Ciąg przechowywany w `name` jest zawarty w komunikacie powitalnym, który jest przesyłany do funkcji języka JavaScript, `displayWelcome`, która renderuje Komunikat powitalny do znacznika nagłówka.</span><span class="sxs-lookup"><span data-stu-id="73142-161">The string stored in `name` is incorporated into a welcome message, which is passed to a JavaScript function, `displayWelcome`, which renders the welcome message into a heading tag.</span></span>

## <a name="call-a-void-javascript-function"></a><span data-ttu-id="73142-162">Wywoływanie funkcji języka JavaScript typu void</span><span class="sxs-lookup"><span data-stu-id="73142-162">Call a void JavaScript function</span></span>

<span data-ttu-id="73142-163">Funkcje języka JavaScript zwracające [wartość void (0)/void 0](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Operators/void) lub [undefined](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/undefined) są wywoływane z `IJSRuntime.InvokeVoidAsync`.</span><span class="sxs-lookup"><span data-stu-id="73142-163">JavaScript functions that return [void(0)/void 0](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Operators/void) or [undefined](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/undefined) are called with `IJSRuntime.InvokeVoidAsync`.</span></span>

## <a name="detect-when-a-blazor-app-is-prerendering"></a><span data-ttu-id="73142-164">Wykryj, kiedy aplikacja Blazor jest renderowana</span><span class="sxs-lookup"><span data-stu-id="73142-164">Detect when a Blazor app is prerendering</span></span>
 
[!INCLUDE[](~/includes/blazor-prerendering.md)]

## <a name="capture-references-to-elements"></a><span data-ttu-id="73142-165">Przechwyć odwołania do elementów</span><span class="sxs-lookup"><span data-stu-id="73142-165">Capture references to elements</span></span>

<span data-ttu-id="73142-166">Niektóre scenariusze [międzyoperacyjności języka JavaScript](xref:blazor/javascript-interop) wymagają odwołań do elementów HTML.</span><span class="sxs-lookup"><span data-stu-id="73142-166">Some [JavaScript interop](xref:blazor/javascript-interop) scenarios require references to HTML elements.</span></span> <span data-ttu-id="73142-167">Na przykład Biblioteka interfejsu użytkownika może wymagać odwołania do elementu dla inicjalizacji lub może być konieczne wywołanie interfejsów API, takich jak `focus` lub `play`.</span><span class="sxs-lookup"><span data-stu-id="73142-167">For example, a UI library may require an element reference for initialization, or you might need to call command-like APIs on an element, such as `focus` or `play`.</span></span>

<span data-ttu-id="73142-168">Przechwyć odwołania do elementów HTML w składniku, korzystając z następującej metody:</span><span class="sxs-lookup"><span data-stu-id="73142-168">Capture references to HTML elements in a component using the following approach:</span></span>

* <span data-ttu-id="73142-169">Dodaj atrybut `@ref` do elementu HTML.</span><span class="sxs-lookup"><span data-stu-id="73142-169">Add an `@ref` attribute to the HTML element.</span></span>
* <span data-ttu-id="73142-170">Zdefiniuj pole typu `ElementReference`, którego nazwa pasuje do wartości atrybutu `@ref`.</span><span class="sxs-lookup"><span data-stu-id="73142-170">Define a field of type `ElementReference` whose name matches the value of the `@ref` attribute.</span></span>

<span data-ttu-id="73142-171">Poniższy przykład pokazuje przechwytywanie odwołania do elementu `username` `<input>`:</span><span class="sxs-lookup"><span data-stu-id="73142-171">The following example shows capturing a reference to the `username` `<input>` element:</span></span>

```cshtml
<input @ref="username" ... />

@code {
    ElementReference username;
}
```

> [!NOTE]
> <span data-ttu-id="73142-172">**Nie** używaj przechwyconych odwołań do elementów jako sposobu wypełniania modelu DOM.</span><span class="sxs-lookup"><span data-stu-id="73142-172">Do **not** use captured element references as a way of populating the DOM.</span></span> <span data-ttu-id="73142-173">Wykonanie tej czynności może zakłócać model renderowania deklaratywnego.</span><span class="sxs-lookup"><span data-stu-id="73142-173">Doing so may interfere with the declarative rendering model.</span></span>

<span data-ttu-id="73142-174">W odniesieniu do kodu platformy .NET `ElementReference` jest nieprzezroczystym uchwytem.</span><span class="sxs-lookup"><span data-stu-id="73142-174">As far as .NET code is concerned, an `ElementReference` is an opaque handle.</span></span> <span data-ttu-id="73142-175">*Jedyną* czynnością, którą można wykonać przy użyciu `ElementReference`, jest przekazanie go do kodu JavaScript za pośrednictwem międzyoperacyjności języka JavaScript.</span><span class="sxs-lookup"><span data-stu-id="73142-175">The *only* thing you can do with `ElementReference` is pass it through to JavaScript code via JavaScript interop.</span></span> <span data-ttu-id="73142-176">Gdy to zrobisz, kod po stronie JavaScript odbiera wystąpienie `HTMLElement`, które może być używane z normalnymi interfejsami API modelu DOM.</span><span class="sxs-lookup"><span data-stu-id="73142-176">When you do so, the JavaScript-side code receives an `HTMLElement` instance, which it can use with normal DOM APIs.</span></span>

<span data-ttu-id="73142-177">Na przykład poniższy kod definiuje metodę rozszerzenia .NET, która umożliwia ustawienie fokusu na elemencie:</span><span class="sxs-lookup"><span data-stu-id="73142-177">For example, the following code defines a .NET extension method that enables setting the focus on an element:</span></span>

<span data-ttu-id="73142-178">*exampleJsInterop. js*:</span><span class="sxs-lookup"><span data-stu-id="73142-178">*exampleJsInterop.js*:</span></span>

```javascript
window.exampleJsFunctions = {
  focusElement : function (element) {
    element.focus();
  }
}
```

<span data-ttu-id="73142-179">Użyj `IJSRuntime.InvokeAsync<T>` i Wywołaj `exampleJsFunctions.focusElement` z `ElementReference`, aby skoncentrować element:</span><span class="sxs-lookup"><span data-stu-id="73142-179">Use `IJSRuntime.InvokeAsync<T>` and call `exampleJsFunctions.focusElement` with an `ElementReference` to focus an element:</span></span>

[!code-cshtml[](javascript-interop/samples_snapshot/component1.razor?highlight=1,3,11-12)]

<span data-ttu-id="73142-180">Aby użyć metody rozszerzenia do skoncentrowania się na elemencie, Utwórz statyczną metodę rozszerzenia, która odbiera wystąpienie `IJSRuntime`:</span><span class="sxs-lookup"><span data-stu-id="73142-180">To use an extension method to focus an element, create a static extension method that receives the `IJSRuntime` instance:</span></span>

```csharp
public static Task Focus(this ElementReference elementRef, IJSRuntime jsRuntime)
{
    return jsRuntime.InvokeAsync<object>(
        "exampleJsFunctions.focusElement", elementRef);
}
```

<span data-ttu-id="73142-181">Metoda jest wywoływana bezpośrednio dla obiektu.</span><span class="sxs-lookup"><span data-stu-id="73142-181">The method is called directly on the object.</span></span> <span data-ttu-id="73142-182">W poniższym przykładzie przyjęto założenie, że metoda static `Focus` jest dostępna z przestrzeni nazw `JsInteropClasses`:</span><span class="sxs-lookup"><span data-stu-id="73142-182">The following example assumes that the static `Focus` method is available from the `JsInteropClasses` namespace:</span></span>

[!code-cshtml[](javascript-interop/samples_snapshot/component2.razor?highlight=1,4,12)]

> [!IMPORTANT]
> <span data-ttu-id="73142-183">Zmienna `username` jest wypełniana tylko po wyrenderowaniu składnika.</span><span class="sxs-lookup"><span data-stu-id="73142-183">The `username` variable is only populated after the component is rendered.</span></span> <span data-ttu-id="73142-184">W przypadku niewypełnienia `ElementReference` do kodu JavaScript kod JavaScript otrzymuje wartość `null`.</span><span class="sxs-lookup"><span data-stu-id="73142-184">If an unpopulated `ElementReference` is passed to JavaScript code, the JavaScript code receives a value of `null`.</span></span> <span data-ttu-id="73142-185">Aby manipulować odwołaniami do elementów po zakończeniu renderowania składnika (aby ustawić początkowy fokus w elemencie), użyj [metod cyklu życia składnika](xref:blazor/components#lifecycle-methods)`OnAfterRenderAsync` lub `OnAfterRender`.</span><span class="sxs-lookup"><span data-stu-id="73142-185">To manipulate element references after the component has finished rendering (to set the initial focus on an element) use the `OnAfterRenderAsync` or `OnAfterRender` [component lifecycle methods](xref:blazor/components#lifecycle-methods).</span></span>

## <a name="invoke-net-methods-from-javascript-functions"></a><span data-ttu-id="73142-186">Wywoływanie metod .NET przy użyciu funkcji języka JavaScript</span><span class="sxs-lookup"><span data-stu-id="73142-186">Invoke .NET methods from JavaScript functions</span></span>

### <a name="static-net-method-call"></a><span data-ttu-id="73142-187">Statyczne wywołanie metody .NET</span><span class="sxs-lookup"><span data-stu-id="73142-187">Static .NET method call</span></span>

<span data-ttu-id="73142-188">Aby wywołać statyczną metodę .NET z poziomu języka JavaScript, użyj funkcji `DotNet.invokeMethod` lub `DotNet.invokeMethodAsync`.</span><span class="sxs-lookup"><span data-stu-id="73142-188">To invoke a static .NET method from JavaScript, use the `DotNet.invokeMethod` or `DotNet.invokeMethodAsync` functions.</span></span> <span data-ttu-id="73142-189">Przekaż identyfikator metody statycznej, która ma być wywoływana, nazwę zestawu zawierającego funkcję i wszelkie argumenty.</span><span class="sxs-lookup"><span data-stu-id="73142-189">Pass in the identifier of the static method you wish to call, the name of the assembly containing the function, and any arguments.</span></span> <span data-ttu-id="73142-190">Wersja asynchroniczna jest preferowana do obsługi scenariuszy serwera Blazor.</span><span class="sxs-lookup"><span data-stu-id="73142-190">The asynchronous version is preferred to support Blazor Server scenarios.</span></span> <span data-ttu-id="73142-191">Aby wywołać metodę .NET z języka JavaScript, metoda .NET musi być publiczna, statyczna i mieć atrybut `[JSInvokable]`.</span><span class="sxs-lookup"><span data-stu-id="73142-191">To invoke a .NET method from JavaScript, the .NET method must be public, static, and have the `[JSInvokable]` attribute.</span></span> <span data-ttu-id="73142-192">Domyślnie identyfikator metody jest nazwą metody, ale można określić inny identyfikator przy użyciu konstruktora `JSInvokableAttribute`.</span><span class="sxs-lookup"><span data-stu-id="73142-192">By default, the method identifier is the method name, but you can specify a different identifier using the `JSInvokableAttribute` constructor.</span></span> <span data-ttu-id="73142-193">Wywoływanie otwartych metod ogólnych nie jest obecnie obsługiwane.</span><span class="sxs-lookup"><span data-stu-id="73142-193">Calling open generic methods isn't currently supported.</span></span>

<span data-ttu-id="73142-194">Przykładowa aplikacja zawiera C# metodę zwracającą tablicę `int`S.</span><span class="sxs-lookup"><span data-stu-id="73142-194">The sample app includes a C# method to return an array of `int`s.</span></span> <span data-ttu-id="73142-195">Atrybut `JSInvokable` jest stosowany do metody.</span><span class="sxs-lookup"><span data-stu-id="73142-195">The `JSInvokable` attribute is applied to the method.</span></span>

<span data-ttu-id="73142-196">*Strony/JsInterop. Razor*:</span><span class="sxs-lookup"><span data-stu-id="73142-196">*Pages/JsInterop.razor*:</span></span>

[!code-cshtml[](./common/samples/3.x/BlazorWebAssemblySample/Pages/JsInterop.razor?name=snippet_JSInterop2&highlight=7-11)]

<span data-ttu-id="73142-197">Kod JavaScript obsługiwany przez klienta wywołuje metodę C# .NET.</span><span class="sxs-lookup"><span data-stu-id="73142-197">JavaScript served to the client invokes the C# .NET method.</span></span>

<span data-ttu-id="73142-198">*wwwroot/exampleJsInterop. js*:</span><span class="sxs-lookup"><span data-stu-id="73142-198">*wwwroot/exampleJsInterop.js*:</span></span>

[!code-javascript[](./common/samples/3.x/BlazorWebAssemblySample/wwwroot/exampleJsInterop.js?highlight=8-14)]

<span data-ttu-id="73142-199">Gdy zostanie wybrany przycisk **ReturnArrayAsync Wyzwalaj metodę statyczną .NET** , sprawdź dane wyjściowe konsoli w narzędziach deweloperskich sieci Web w przeglądarce.</span><span class="sxs-lookup"><span data-stu-id="73142-199">When the **Trigger .NET static method ReturnArrayAsync** button is selected, examine the console output in the browser's web developer tools.</span></span>

<span data-ttu-id="73142-200">Dane wyjściowe konsoli są następujące:</span><span class="sxs-lookup"><span data-stu-id="73142-200">The console output is:</span></span>

```console
Array(4) [ 1, 2, 3, 4 ]
```

<span data-ttu-id="73142-201">Czwarta wartość tablicy jest wypychana do tablicy (`data.push(4);`) zwróconej przez `ReturnArrayAsync`.</span><span class="sxs-lookup"><span data-stu-id="73142-201">The fourth array value is pushed to the array (`data.push(4);`) returned by `ReturnArrayAsync`.</span></span>

### <a name="instance-method-call"></a><span data-ttu-id="73142-202">Wywołanie metody wystąpienia</span><span class="sxs-lookup"><span data-stu-id="73142-202">Instance method call</span></span>

<span data-ttu-id="73142-203">Można również wywołać metody wystąpienia platformy .NET z poziomu języka JavaScript.</span><span class="sxs-lookup"><span data-stu-id="73142-203">You can also call .NET instance methods from JavaScript.</span></span> <span data-ttu-id="73142-204">Aby wywołać metodę wystąpienia platformy .NET z poziomu języka JavaScript:</span><span class="sxs-lookup"><span data-stu-id="73142-204">To invoke a .NET instance method from JavaScript:</span></span>

* <span data-ttu-id="73142-205">Przekaż wystąpienie programu .NET do języka JavaScript, zawijając je w wystąpieniu `DotNetObjectReference`.</span><span class="sxs-lookup"><span data-stu-id="73142-205">Pass the .NET instance to JavaScript by wrapping it in a `DotNetObjectReference` instance.</span></span> <span data-ttu-id="73142-206">Wystąpienie programu .NET jest przesyłane przez odwołanie do języka JavaScript.</span><span class="sxs-lookup"><span data-stu-id="73142-206">The .NET instance is passed by reference to JavaScript.</span></span>
* <span data-ttu-id="73142-207">Wywołaj metody wystąpienia platformy .NET w wystąpieniu przy użyciu funkcji `invokeMethod` lub `invokeMethodAsync`.</span><span class="sxs-lookup"><span data-stu-id="73142-207">Invoke .NET instance methods on the instance using the `invokeMethod` or `invokeMethodAsync` functions.</span></span> <span data-ttu-id="73142-208">Wystąpienie programu .NET może być również przekazywać jako argument podczas wywoływania innych metod .NET z JavaScript.</span><span class="sxs-lookup"><span data-stu-id="73142-208">The .NET instance can also be passed as an argument when invoking other .NET methods from JavaScript.</span></span>

> [!NOTE]
> <span data-ttu-id="73142-209">Przykładowa aplikacja rejestruje komunikaty do konsoli po stronie klienta.</span><span class="sxs-lookup"><span data-stu-id="73142-209">The sample app logs messages to the client-side console.</span></span> <span data-ttu-id="73142-210">W poniższych przykładach zademonstrowanych przez przykładową aplikację można sprawdzić dane wyjściowe konsoli przeglądarki w narzędziach deweloperskich w przeglądarce.</span><span class="sxs-lookup"><span data-stu-id="73142-210">For the following examples demonstrated by the sample app, examine the browser's console output in the browser's developer tools.</span></span>

<span data-ttu-id="73142-211">Po wybraniu przycisku **wyzwalanie metody wystąpienia platformy .NET HelloHelper. sayHello** , `ExampleJsInterop.CallHelloHelperSayHello` jest wywoływana i przekazuje nazwę, `Blazor` do metody.</span><span class="sxs-lookup"><span data-stu-id="73142-211">When the **Trigger .NET instance method HelloHelper.SayHello** button is selected, `ExampleJsInterop.CallHelloHelperSayHello` is called and passes a name, `Blazor`, to the method.</span></span>

<span data-ttu-id="73142-212">*Strony/JsInterop. Razor*:</span><span class="sxs-lookup"><span data-stu-id="73142-212">*Pages/JsInterop.razor*:</span></span>

[!code-cshtml[](./common/samples/3.x/BlazorWebAssemblySample/Pages/JsInterop.razor?name=snippet_JSInterop3&highlight=8-9)]

<span data-ttu-id="73142-213">`CallHelloHelperSayHello` wywołuje funkcję JavaScript `sayHello` z nowym wystąpieniem `HelloHelper`.</span><span class="sxs-lookup"><span data-stu-id="73142-213">`CallHelloHelperSayHello` invokes the JavaScript function `sayHello` with a new instance of `HelloHelper`.</span></span>

<span data-ttu-id="73142-214">*JsInteropClasses/ExampleJsInterop. cs*:</span><span class="sxs-lookup"><span data-stu-id="73142-214">*JsInteropClasses/ExampleJsInterop.cs*:</span></span>

[!code-csharp[](./common/samples/3.x/BlazorWebAssemblySample/JsInteropClasses/ExampleJsInterop.cs?name=snippet1&highlight=10-16)]

<span data-ttu-id="73142-215">*wwwroot/exampleJsInterop. js*:</span><span class="sxs-lookup"><span data-stu-id="73142-215">*wwwroot/exampleJsInterop.js*:</span></span>

[!code-javascript[](./common/samples/3.x/BlazorWebAssemblySample/wwwroot/exampleJsInterop.js?highlight=15-18)]

<span data-ttu-id="73142-216">Nazwa jest przenoszona do konstruktora `HelloHelper`, który ustawia właściwość `HelloHelper.Name`.</span><span class="sxs-lookup"><span data-stu-id="73142-216">The name is passed to `HelloHelper`'s constructor, which sets the `HelloHelper.Name` property.</span></span> <span data-ttu-id="73142-217">Gdy funkcja JavaScript `sayHello` jest wykonywana, `HelloHelper.SayHello` zwróci komunikat `Hello, {Name}!`, który jest zapisywana w konsoli przez funkcję JavaScript.</span><span class="sxs-lookup"><span data-stu-id="73142-217">When the JavaScript function `sayHello` is executed, `HelloHelper.SayHello` returns the `Hello, {Name}!` message, which is written to the console by the JavaScript function.</span></span>

<span data-ttu-id="73142-218">*JsInteropClasses/HelloHelper. cs*:</span><span class="sxs-lookup"><span data-stu-id="73142-218">*JsInteropClasses/HelloHelper.cs*:</span></span>

[!code-csharp[](./common/samples/3.x/BlazorWebAssemblySample/JsInteropClasses/HelloHelper.cs?name=snippet1&highlight=5,10-11)]

<span data-ttu-id="73142-219">Dane wyjściowe konsoli w narzędziach deweloperskich sieci Web w przeglądarce:</span><span class="sxs-lookup"><span data-stu-id="73142-219">Console output in the browser's web developer tools:</span></span>

```console
Hello, Blazor!
```

## <a name="share-interop-code-in-a-class-library"></a><span data-ttu-id="73142-220">Udostępnianie kodu międzyoperacyjnego w bibliotece klas</span><span class="sxs-lookup"><span data-stu-id="73142-220">Share interop code in a class library</span></span>

<span data-ttu-id="73142-221">Kod międzyoperacyjny JavaScript może być uwzględniony w bibliotece klas, co umożliwia udostępnianie kodu w pakiecie NuGet.</span><span class="sxs-lookup"><span data-stu-id="73142-221">JavaScript interop code can be included in a class library, which allows you to share the code in a NuGet package.</span></span>

<span data-ttu-id="73142-222">Biblioteka klas obsługuje Osadzanie zasobów JavaScript w skompilowanym zestawie.</span><span class="sxs-lookup"><span data-stu-id="73142-222">The class library handles embedding JavaScript resources in the built assembly.</span></span> <span data-ttu-id="73142-223">Pliki JavaScript są umieszczane w folderze *wwwroot* .</span><span class="sxs-lookup"><span data-stu-id="73142-223">The JavaScript files are placed in the *wwwroot* folder.</span></span> <span data-ttu-id="73142-224">Narzędzia te zajmują się osadzaniem zasobów podczas kompilowania biblioteki.</span><span class="sxs-lookup"><span data-stu-id="73142-224">The tooling takes care of embedding the resources when the library is built.</span></span>

<span data-ttu-id="73142-225">Skompilowany pakiet NuGet jest przywoływany w pliku projektu aplikacji w taki sam sposób, w jaki jest przywoływany każdy pakiet NuGet.</span><span class="sxs-lookup"><span data-stu-id="73142-225">The built NuGet package is referenced in the app's project file the same way that any NuGet package is referenced.</span></span> <span data-ttu-id="73142-226">Po przywróceniu pakietu kod aplikacji może być wywoływany w języku JavaScript, tak jakby C#był.</span><span class="sxs-lookup"><span data-stu-id="73142-226">After the package is restored, app code can call into JavaScript as if it were C#.</span></span>

<span data-ttu-id="73142-227">Aby uzyskać więcej informacji, zobacz <xref:blazor/class-libraries>.</span><span class="sxs-lookup"><span data-stu-id="73142-227">For more information, see <xref:blazor/class-libraries>.</span></span>

## <a name="harden-js-interop-calls"></a><span data-ttu-id="73142-228">Zabezpieczenia wywołań międzyoperacyjnych w ramach funkcjonalności JS</span><span class="sxs-lookup"><span data-stu-id="73142-228">Harden JS interop calls</span></span>

<span data-ttu-id="73142-229">Usługa JS Interop może zakończyć się niepowodzeniem z powodu błędów sieci i powinna być traktowana jako niezawodna.</span><span class="sxs-lookup"><span data-stu-id="73142-229">JS interop may fail due to networking errors and should be treated as unreliable.</span></span> <span data-ttu-id="73142-230">Domyślnie aplikacja serwera Blazor przeprowadzi limit czasu wywołań międzyoperacyjnych JS na serwerze po jednej minucie.</span><span class="sxs-lookup"><span data-stu-id="73142-230">By default, a Blazor Server app times out JS interop calls on the server after one minute.</span></span> <span data-ttu-id="73142-231">Jeśli aplikacja może tolerować bardziej agresywny limit czasu, na przykład 10 sekund, należy ustawić limit czasu przy użyciu jednej z następujących metod:</span><span class="sxs-lookup"><span data-stu-id="73142-231">If an app can tolerate a more aggressive timeout, such as 10 seconds, set the timeout using one of the following approaches:</span></span>

* <span data-ttu-id="73142-232">Globalnie w `Startup.ConfigureServices` Określ limit czasu:</span><span class="sxs-lookup"><span data-stu-id="73142-232">Globally in `Startup.ConfigureServices`, specify the timeout:</span></span>

  ```csharp
  services.AddServerSideBlazor(
      options => options.JSInteropDefaultCallTimeout = TimeSpan.FromSeconds({SECONDS}));
  ```

* <span data-ttu-id="73142-233">Dla wywołania w kodzie składnika pojedyncze wywołanie może określać limit czasu:</span><span class="sxs-lookup"><span data-stu-id="73142-233">Per-invocation in component code, a single call can specify the timeout:</span></span>

  ```csharp
  var result = await JSRuntime.InvokeAsync<string>("MyJSOperation", 
      TimeSpan.FromSeconds({SECONDS}), new[] { "Arg1" });
  ```

<span data-ttu-id="73142-234">Aby uzyskać więcej informacji na temat wyczerpania zasobów, zobacz <xref:security/blazor/server>.</span><span class="sxs-lookup"><span data-stu-id="73142-234">For more information on resource exhaustion, see <xref:security/blazor/server>.</span></span>
