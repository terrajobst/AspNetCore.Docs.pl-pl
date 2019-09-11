---
title: ASP.NET Core Blazor JavaScript interop
author: guardrex
description: Dowiedz się, jak wywoływać funkcje języka JavaScript z technologii .NET i .NET z poziomu języka JavaScript w aplikacjach Blazor.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 09/07/2019
uid: blazor/javascript-interop
ms.openlocfilehash: fa485420c01e6a6d4181f733d6848de08ffca730
ms.sourcegitcommit: e7c56e8da5419bbc20b437c2dd531dedf9b0dc6b
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 09/10/2019
ms.locfileid: "70878354"
---
# <a name="aspnet-core-blazor-javascript-interop"></a><span data-ttu-id="f78d9-103">ASP.NET Core Blazor JavaScript interop</span><span class="sxs-lookup"><span data-stu-id="f78d9-103">ASP.NET Core Blazor JavaScript interop</span></span>

<span data-ttu-id="f78d9-104">[Javier Calvarro Nelson](https://github.com/javiercn), [Daniel Roth](https://github.com/danroth27)i [Luke](https://github.com/guardrex) Latham</span><span class="sxs-lookup"><span data-stu-id="f78d9-104">By [Javier Calvarro Nelson](https://github.com/javiercn), [Daniel Roth](https://github.com/danroth27), and [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="f78d9-105">Aplikacja Blazor może wywoływać funkcje języka JavaScript z technologii .NET i .NET z kodu JavaScript.</span><span class="sxs-lookup"><span data-stu-id="f78d9-105">A Blazor app can invoke JavaScript functions from .NET and .NET methods from JavaScript code.</span></span>

<span data-ttu-id="f78d9-106">[Wyświetlanie lub pobieranie przykładowego kodu](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/blazor/common/samples/) ([sposobu pobierania](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="f78d9-106">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/blazor/common/samples/) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="invoke-javascript-functions-from-net-methods"></a><span data-ttu-id="f78d9-107">Wywoływanie funkcji języka JavaScript z metod .NET</span><span class="sxs-lookup"><span data-stu-id="f78d9-107">Invoke JavaScript functions from .NET methods</span></span>

<span data-ttu-id="f78d9-108">Istnieją przypadki, w których kod .NET jest wymagany do wywołania funkcji języka JavaScript.</span><span class="sxs-lookup"><span data-stu-id="f78d9-108">There are times when .NET code is required to call a JavaScript function.</span></span> <span data-ttu-id="f78d9-109">Na przykład wywołanie języka JavaScript może uwidaczniać możliwości przeglądarki lub funkcje z biblioteki JavaScript w aplikacji.</span><span class="sxs-lookup"><span data-stu-id="f78d9-109">For example, a JavaScript call can expose browser capabilities or functionality from a JavaScript library to the app.</span></span>

<span data-ttu-id="f78d9-110">Aby wywołać kod JavaScript z platformy .NET, użyj `IJSRuntime` abstrakcji.</span><span class="sxs-lookup"><span data-stu-id="f78d9-110">To call into JavaScript from .NET, use the `IJSRuntime` abstraction.</span></span> <span data-ttu-id="f78d9-111">`InvokeAsync<T>` Metoda przyjmuje identyfikator dla funkcji języka JavaScript, która ma zostać wywołana wraz z dowolną liczbą argumentów do serializacji JSON.</span><span class="sxs-lookup"><span data-stu-id="f78d9-111">The `InvokeAsync<T>` method takes an identifier for the JavaScript function that you wish to invoke along with any number of JSON-serializable arguments.</span></span> <span data-ttu-id="f78d9-112">Identyfikator funkcji jest względny w stosunku do zakresu globalnego`window`().</span><span class="sxs-lookup"><span data-stu-id="f78d9-112">The function identifier is relative to the global scope (`window`).</span></span> <span data-ttu-id="f78d9-113">Jeśli chcesz wywołać `window.someScope.someFunction`, identyfikator to `someScope.someFunction`.</span><span class="sxs-lookup"><span data-stu-id="f78d9-113">If you wish to call `window.someScope.someFunction`, the identifier is `someScope.someFunction`.</span></span> <span data-ttu-id="f78d9-114">Nie ma potrzeby rejestrowania funkcji przed jej wywołaniem.</span><span class="sxs-lookup"><span data-stu-id="f78d9-114">There's no need to register the function before it's called.</span></span> <span data-ttu-id="f78d9-115">Zwracanym typem `T` musi być również kod JSON możliwy do serializacji.</span><span class="sxs-lookup"><span data-stu-id="f78d9-115">The return type `T` must also be JSON serializable.</span></span>

<span data-ttu-id="f78d9-116">Dla aplikacji po stronie serwera:</span><span class="sxs-lookup"><span data-stu-id="f78d9-116">For server-side apps:</span></span>

* <span data-ttu-id="f78d9-117">Aplikacja po stronie serwera przetwarza wiele żądań użytkownika.</span><span class="sxs-lookup"><span data-stu-id="f78d9-117">Multiple user requests are processed by the server-side app.</span></span> <span data-ttu-id="f78d9-118">Nie wywołuj `JSRuntime.Current` w składniku w celu wywołania funkcji JavaScript.</span><span class="sxs-lookup"><span data-stu-id="f78d9-118">Don't call `JSRuntime.Current` in a component to invoke JavaScript functions.</span></span>
* <span data-ttu-id="f78d9-119">`IJSRuntime` Wstrzyknąć streszczenie i użyć wstrzykniętego obiektu do wystawienia wywołań międzyoperacyjnych języka JavaScript.</span><span class="sxs-lookup"><span data-stu-id="f78d9-119">Inject the `IJSRuntime` abstraction and use the injected object to issue JavaScript interop calls.</span></span>
* <span data-ttu-id="f78d9-120">Gdy aplikacja Blazor jest wstępnie renderowana, wywołanie do języka JavaScript nie jest możliwe, ponieważ połączenie z przeglądarką nie zostało nawiązane.</span><span class="sxs-lookup"><span data-stu-id="f78d9-120">While a Blazor app is prerendering, calling into JavaScript isn't possible because a connection with the browser hasn't been established.</span></span> <span data-ttu-id="f78d9-121">Aby uzyskać więcej informacji, zobacz sekcję [wykrywanie, kiedy aplikacja Blazor jest renderowana](#detect-when-a-blazor-app-is-prerendering) .</span><span class="sxs-lookup"><span data-stu-id="f78d9-121">For more information, see the [Detect when a Blazor app is prerendering](#detect-when-a-blazor-app-is-prerendering) section.</span></span>

<span data-ttu-id="f78d9-122">Poniższy przykład jest oparty na [dekoderze](https://developer.mozilla.org/docs/Web/API/TextDecoder), eksperymentalnym dekoderem JavaScript.</span><span class="sxs-lookup"><span data-stu-id="f78d9-122">The following example is based on [TextDecoder](https://developer.mozilla.org/docs/Web/API/TextDecoder), an experimental JavaScript-based decoder.</span></span> <span data-ttu-id="f78d9-123">W przykładzie pokazano, jak wywołać funkcję JavaScript z C# metody.</span><span class="sxs-lookup"><span data-stu-id="f78d9-123">The example demonstrates how to invoke a JavaScript function from a C# method.</span></span> <span data-ttu-id="f78d9-124">Funkcja JavaScript akceptuje tablicę bajtową z C# metody, dekoduje tablicę i zwraca tekst do składnika do wyświetlenia.</span><span class="sxs-lookup"><span data-stu-id="f78d9-124">The JavaScript function accepts a byte array from a C# method, decodes the array, and returns the text to the component for display.</span></span>

<span data-ttu-id="f78d9-125">Wewnątrz elementu wwwroot/index.html (Blazor po stronie klienta) lub *stron/_Host. cshtml* (Blazor po stronie serwera), podaj funkcję, która używa `TextDecoder` do zdekodowania przekazaną tablicę: `<head>`</span><span class="sxs-lookup"><span data-stu-id="f78d9-125">Inside the `<head>` element of *wwwroot/index.html* (Blazor client-side) or *Pages/_Host.cshtml* (Blazor server-side), provide a function that uses `TextDecoder` to decode a passed array:</span></span>

[!code-html[](javascript-interop/samples_snapshot/index-script.html)]

<span data-ttu-id="f78d9-126">Kod JavaScript, taki jak kod przedstawiony w powyższym przykładzie, można również załadować z pliku JavaScript ( *. js*) z odwołaniem do pliku skryptu:</span><span class="sxs-lookup"><span data-stu-id="f78d9-126">JavaScript code, such as the code shown in the preceding example, can also be loaded from a JavaScript file (*.js*) with a reference to the script file:</span></span>

```html
<script src="exampleJsInterop.js"></script>
```

<span data-ttu-id="f78d9-127">Następujący składnik:</span><span class="sxs-lookup"><span data-stu-id="f78d9-127">The following component:</span></span>

* <span data-ttu-id="f78d9-128">Wywołuje funkcję `JsRuntime` JavaScript przy użyciu przycisku składnika (**Konwertuj tablicę).** `ConvertArray`</span><span class="sxs-lookup"><span data-stu-id="f78d9-128">Invokes the `ConvertArray` JavaScript function using `JsRuntime` when a component button (**Convert Array**) is selected.</span></span>
* <span data-ttu-id="f78d9-129">Po wywołaniu funkcji języka JavaScript przenoszona tablica jest konwertowana na ciąg.</span><span class="sxs-lookup"><span data-stu-id="f78d9-129">After the JavaScript function is called, the passed array is converted into a string.</span></span> <span data-ttu-id="f78d9-130">Ciąg jest zwracany do składnika do wyświetlenia.</span><span class="sxs-lookup"><span data-stu-id="f78d9-130">The string is returned to the component for display.</span></span>

[!code-cshtml[](javascript-interop/samples_snapshot/call-js-example.razor?highlight=2,34-35)]

<span data-ttu-id="f78d9-131">Aby użyć `IJSRuntime` abstrakcji, należy zastosować jedną z następujących metod:</span><span class="sxs-lookup"><span data-stu-id="f78d9-131">To use the `IJSRuntime` abstraction, adopt any of the following approaches:</span></span>

* <span data-ttu-id="f78d9-132">Wstrzyknąć abstrakcję do składnika Razor ( *. Razor*): `IJSRuntime`</span><span class="sxs-lookup"><span data-stu-id="f78d9-132">Inject the `IJSRuntime` abstraction into the Razor component (*.razor*):</span></span>

  [!code-cshtml[](javascript-interop/samples_snapshot/inject-abstraction.razor?highlight=1)]

* <span data-ttu-id="f78d9-133">Wstrzyknąć streszczenie do klasy ( *. cs*): `IJSRuntime`</span><span class="sxs-lookup"><span data-stu-id="f78d9-133">Inject the `IJSRuntime` abstraction into a class (*.cs*):</span></span>

  [!code-csharp[](javascript-interop/samples_snapshot/inject-abstraction-class.cs?highlight=5)]

* <span data-ttu-id="f78d9-134">W przypadku generowania zawartości dynamicznej przy użyciu [BuildRenderTree](xref:blazor/components#manual-rendertreebuilder-logic)należy `[Inject]` użyć atrybutu:</span><span class="sxs-lookup"><span data-stu-id="f78d9-134">For dynamic content generation with [BuildRenderTree](xref:blazor/components#manual-rendertreebuilder-logic), use the `[Inject]` attribute:</span></span>

  ```csharp
  [Inject]
  IJSRuntime JSRuntime { get; set; }
  ```

<span data-ttu-id="f78d9-135">W aplikacji przykładowej po stronie klienta, która jest dołączona do tego tematu, dostępne są dwie funkcje JavaScript dla aplikacji po stronie klienta, które współdziałają z modelem DOM, aby odbierać dane wejściowe użytkownika i wyświetlać komunikat powitalny:</span><span class="sxs-lookup"><span data-stu-id="f78d9-135">In the client-side sample app that accompanies this topic, two JavaScript functions are available to the client-side app that interact with the DOM to receive user input and display a welcome message:</span></span>

* <span data-ttu-id="f78d9-136">`showPrompt`&ndash; Generuje monit o zaakceptowanie danych wprowadzonych przez użytkownika (nazwę użytkownika) i zwraca nazwę obiektu wywołującego.</span><span class="sxs-lookup"><span data-stu-id="f78d9-136">`showPrompt` &ndash; Produces a prompt to accept user input (the user's name) and returns the name to the caller.</span></span>
* <span data-ttu-id="f78d9-137">`displayWelcome`Przypisuje Komunikat powitalny od wywołującego do obiektu Dom `id` z `welcome`. &ndash;</span><span class="sxs-lookup"><span data-stu-id="f78d9-137">`displayWelcome` &ndash; Assigns a welcome message from the caller to a DOM object with an `id` of `welcome`.</span></span>

<span data-ttu-id="f78d9-138">*wwwroot/exampleJsInterop. js*:</span><span class="sxs-lookup"><span data-stu-id="f78d9-138">*wwwroot/exampleJsInterop.js*:</span></span>

[!code-javascript[](./common/samples/3.x/BlazorSample/wwwroot/exampleJsInterop.js?highlight=2-7)]

<span data-ttu-id="f78d9-139">Umieść tag odwołujący się do pliku JavaScript w pliku wwwroot/index.html (Blazor po stronie klienta) lub *strony/_Host. cshtml* (po stronie serwera Blazor). `<script>`</span><span class="sxs-lookup"><span data-stu-id="f78d9-139">Place the `<script>` tag that references the JavaScript file in the *wwwroot/index.html* file (Blazor client-side) or *Pages/_Host.cshtml* file (Blazor server-side).</span></span>

<span data-ttu-id="f78d9-140">*wwwroot/index.html* (Blazor po stronie klienta):</span><span class="sxs-lookup"><span data-stu-id="f78d9-140">*wwwroot/index.html* (Blazor client-side):</span></span>

[!code-html[](./common/samples/3.x/BlazorSample/wwwroot/index.html?highlight=15)]

<span data-ttu-id="f78d9-141">*Pages/_Host. cshtml* (Blazor po stronie serwera):</span><span class="sxs-lookup"><span data-stu-id="f78d9-141">*Pages/_Host.cshtml* (Blazor server-side):</span></span>

[!code-cshtml[](javascript-interop/samples_snapshot/_Host.cshtml?highlight=29)]

<span data-ttu-id="f78d9-142">Nie umieszczaj `<script>` znacznika w pliku składnika, `<script>` ponieważ nie można dynamicznie zaktualizować znacznika.</span><span class="sxs-lookup"><span data-stu-id="f78d9-142">Don't place a `<script>` tag in a component file because the `<script>` tag can't be updated dynamically.</span></span>

<span data-ttu-id="f78d9-143">.NET metod współdziałania z funkcjami JavaScript w pliku *exampleJsInterop. js* przez wywołanie `IJSRuntime.InvokeAsync<T>`.</span><span class="sxs-lookup"><span data-stu-id="f78d9-143">.NET methods interop with the JavaScript functions in the *exampleJsInterop.js* file by calling `IJSRuntime.InvokeAsync<T>`.</span></span>

<span data-ttu-id="f78d9-144">`IJSRuntime` Abstrakcja jest asynchroniczna, aby umożliwić obsługę scenariuszy po stronie serwera.</span><span class="sxs-lookup"><span data-stu-id="f78d9-144">The `IJSRuntime` abstraction is asynchronous to allow for server-side scenarios.</span></span> <span data-ttu-id="f78d9-145">Jeśli aplikacja działa po stronie klienta i chcesz wywołać funkcję JavaScript synchronicznie, downcast do `IJSInProcessRuntime` wywołania `Invoke<T>` i zamiast tego.</span><span class="sxs-lookup"><span data-stu-id="f78d9-145">If the app runs client-side and you want to invoke a JavaScript function synchronously, downcast to `IJSInProcessRuntime` and call `Invoke<T>` instead.</span></span> <span data-ttu-id="f78d9-146">Zalecamy, aby większość bibliotek międzyoperacyjnych języka JavaScript używała asynchronicznych interfejsów API, aby upewnić się, że biblioteki są dostępne we wszystkich scenariuszach, po stronie klienta lub po stronie serwera.</span><span class="sxs-lookup"><span data-stu-id="f78d9-146">We recommend that most JavaScript interop libraries use the async APIs to ensure that the libraries are available in all scenarios, client-side or server-side.</span></span>

<span data-ttu-id="f78d9-147">Przykładowa aplikacja zawiera składnik demonstrujący międzyoperacyjność JavaScript.</span><span class="sxs-lookup"><span data-stu-id="f78d9-147">The sample app includes a component to demonstrate JavaScript interop.</span></span> <span data-ttu-id="f78d9-148">Składnik:</span><span class="sxs-lookup"><span data-stu-id="f78d9-148">The component:</span></span>

* <span data-ttu-id="f78d9-149">Odbiera dane wprowadzane przez użytkownika za pośrednictwem wiersza polecenia języka JavaScript.</span><span class="sxs-lookup"><span data-stu-id="f78d9-149">Receives user input via a JavaScript prompt.</span></span>
* <span data-ttu-id="f78d9-150">Zwraca tekst do składnika do przetworzenia.</span><span class="sxs-lookup"><span data-stu-id="f78d9-150">Returns the text to the component for processing.</span></span>
* <span data-ttu-id="f78d9-151">Wywołuje drugą funkcję języka JavaScript, która współdziała z modelem DOM, aby wyświetlić komunikat powitalny.</span><span class="sxs-lookup"><span data-stu-id="f78d9-151">Calls a second JavaScript function that interacts with the DOM to display a welcome message.</span></span>

<span data-ttu-id="f78d9-152">*Strony/JSInterop. Razor*:</span><span class="sxs-lookup"><span data-stu-id="f78d9-152">*Pages/JSInterop.razor*:</span></span>

[!code-cshtml[](./common/samples/3.x/BlazorSample/Pages/JsInterop.razor?name=snippet_JSInterop1&highlight=3,19-21,23-25)]

1. <span data-ttu-id="f78d9-153">Gdy `TriggerJsPrompt` jest wykonywane, zaznaczając przycisk **Monituj wyzwalacza JavaScript** składnika, funkcja języka `showPrompt` JavaScript dostępna w pliku *wwwroot/exampleJsInterop. js* jest wywoływana.</span><span class="sxs-lookup"><span data-stu-id="f78d9-153">When `TriggerJsPrompt` is executed by selecting the component's **Trigger JavaScript Prompt** button, the JavaScript `showPrompt` function provided in the *wwwroot/exampleJsInterop.js* file is called.</span></span>
1. <span data-ttu-id="f78d9-154">`showPrompt` Funkcja akceptuje dane wejściowe użytkownika (nazwę użytkownika), które są kodowane w formacie HTML i zwracane do składnika.</span><span class="sxs-lookup"><span data-stu-id="f78d9-154">The `showPrompt` function accepts user input (the user's name), which is HTML-encoded and returned to the component.</span></span> <span data-ttu-id="f78d9-155">Składnik przechowuje nazwę użytkownika w zmiennej lokalnej, `name`.</span><span class="sxs-lookup"><span data-stu-id="f78d9-155">The component stores the user's name in a local variable, `name`.</span></span>
1. <span data-ttu-id="f78d9-156">Ciąg przechowywany w programie `name` jest zawarty w komunikacie powitalnym, który jest przesyłany do `displayWelcome`funkcji języka JavaScript, która renderuje Komunikat powitalny do znacznika nagłówka.</span><span class="sxs-lookup"><span data-stu-id="f78d9-156">The string stored in `name` is incorporated into a welcome message, which is passed to a JavaScript function, `displayWelcome`, which renders the welcome message into a heading tag.</span></span>

## <a name="call-a-void-javascript-function"></a><span data-ttu-id="f78d9-157">Wywoływanie funkcji języka JavaScript typu void</span><span class="sxs-lookup"><span data-stu-id="f78d9-157">Call a void JavaScript function</span></span>

<span data-ttu-id="f78d9-158">Funkcje języka JavaScript zwracające wartość [void (0)/void 0](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Operators/void) lub [undefined](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/undefined) są wywoływane `null`przy użyciu `IJSRuntime.InvokeAsync<object>`wartości, która zwraca wartość.</span><span class="sxs-lookup"><span data-stu-id="f78d9-158">JavaScript functions that return [void(0)/void 0](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Operators/void) or [undefined](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/undefined) are called with `IJSRuntime.InvokeAsync<object>`, which returns `null`.</span></span>

## <a name="detect-when-a-blazor-app-is-prerendering"></a><span data-ttu-id="f78d9-159">Wykryj, kiedy aplikacja Blazor jest renderowana</span><span class="sxs-lookup"><span data-stu-id="f78d9-159">Detect when a Blazor app is prerendering</span></span>
 
[!INCLUDE[](~/includes/blazor-prerendering.md)]

## <a name="capture-references-to-elements"></a><span data-ttu-id="f78d9-160">Przechwyć odwołania do elementów</span><span class="sxs-lookup"><span data-stu-id="f78d9-160">Capture references to elements</span></span>

<span data-ttu-id="f78d9-161">Niektóre scenariusze [międzyoperacyjności języka JavaScript](xref:blazor/javascript-interop) wymagają odwołań do elementów HTML.</span><span class="sxs-lookup"><span data-stu-id="f78d9-161">Some [JavaScript interop](xref:blazor/javascript-interop) scenarios require references to HTML elements.</span></span> <span data-ttu-id="f78d9-162">Na przykład Biblioteka interfejsu użytkownika może wymagać odwołania do elementu dla inicjalizacji lub może być konieczne wywołanie interfejsów API, takich jak `focus` lub. `play`</span><span class="sxs-lookup"><span data-stu-id="f78d9-162">For example, a UI library may require an element reference for initialization, or you might need to call command-like APIs on an element, such as `focus` or `play`.</span></span>

<span data-ttu-id="f78d9-163">Przechwyć odwołania do elementów HTML w składniku, korzystając z następującej metody:</span><span class="sxs-lookup"><span data-stu-id="f78d9-163">Capture references to HTML elements in a component using the following approach:</span></span>

* <span data-ttu-id="f78d9-164">`@ref` Dodaj atrybut do elementu HTML.</span><span class="sxs-lookup"><span data-stu-id="f78d9-164">Add an `@ref` attribute to the HTML element.</span></span>
* <span data-ttu-id="f78d9-165">Zdefiniuj pole typu `ElementReference` , którego nazwa pasuje do wartości `@ref` atrybutu.</span><span class="sxs-lookup"><span data-stu-id="f78d9-165">Define a field of type `ElementReference` whose name matches the value of the `@ref` attribute.</span></span>

<span data-ttu-id="f78d9-166">Poniższy przykład pokazuje przechwytywanie odwołania do `username` `<input>` elementu:</span><span class="sxs-lookup"><span data-stu-id="f78d9-166">The following example shows capturing a reference to the `username` `<input>` element:</span></span>

```cshtml
<input @ref="username" ... />

@code {
    ElementReference username;
}
```

> [!NOTE]
> <span data-ttu-id="f78d9-167">**Nie** używaj przechwyconych odwołań do elementów jako sposobu wypełniania lub manipulowania modelem dom, gdy Blazor współdziała z elementami, do których się odwołuje.</span><span class="sxs-lookup"><span data-stu-id="f78d9-167">Do **not** use captured element references as a way of populating or manipulating the DOM when Blazor interacts with the elements referenced.</span></span> <span data-ttu-id="f78d9-168">Wykonanie tej czynności może zakłócać model renderowania deklaratywnego.</span><span class="sxs-lookup"><span data-stu-id="f78d9-168">Doing so may interfere with the declarative rendering model.</span></span>

<span data-ttu-id="f78d9-169">W odniesieniu do kodu `ElementReference` platformy .NET jest to nieprzezroczyste dojście.</span><span class="sxs-lookup"><span data-stu-id="f78d9-169">As far as .NET code is concerned, an `ElementReference` is an opaque handle.</span></span> <span data-ttu-id="f78d9-170">*Jedyną* czynnością, którą można wykonać `ElementReference` , jest przekazanie jej do kodu JavaScript za pośrednictwem międzyoperacyjności języka JavaScript.</span><span class="sxs-lookup"><span data-stu-id="f78d9-170">The *only* thing you can do with `ElementReference` is pass it through to JavaScript code via JavaScript interop.</span></span> <span data-ttu-id="f78d9-171">Gdy to zrobisz, kod po stronie JavaScript odbiera `HTMLElement` wystąpienie, które może być używane z normalnymi interfejsami API modelu DOM.</span><span class="sxs-lookup"><span data-stu-id="f78d9-171">When you do so, the JavaScript-side code receives an `HTMLElement` instance, which it can use with normal DOM APIs.</span></span>

<span data-ttu-id="f78d9-172">Na przykład poniższy kod definiuje metodę rozszerzenia .NET, która umożliwia ustawienie fokusu na elemencie:</span><span class="sxs-lookup"><span data-stu-id="f78d9-172">For example, the following code defines a .NET extension method that enables setting the focus on an element:</span></span>

<span data-ttu-id="f78d9-173">*exampleJsInterop.js*:</span><span class="sxs-lookup"><span data-stu-id="f78d9-173">*exampleJsInterop.js*:</span></span>

```javascript
window.exampleJsFunctions = {
  focusElement : function (element) {
    element.focus();
  }
}
```

<span data-ttu-id="f78d9-174">Użyj `IJSRuntime.InvokeAsync<T>` i `exampleJsFunctions.focusElement` Wywołaj`ElementReference` , aby skoncentrować element:</span><span class="sxs-lookup"><span data-stu-id="f78d9-174">Use `IJSRuntime.InvokeAsync<T>` and call `exampleJsFunctions.focusElement` with an `ElementReference` to focus an element:</span></span>

[!code-cshtml[](javascript-interop/samples_snapshot/component1.razor?highlight=1,3,11-12)]

<span data-ttu-id="f78d9-175">Aby użyć metody rozszerzenia do skoncentrowania się na elemencie, Utwórz statyczną metodę rozszerzenia, która `IJSRuntime` odbiera wystąpienie:</span><span class="sxs-lookup"><span data-stu-id="f78d9-175">To use an extension method to focus an element, create a static extension method that receives the `IJSRuntime` instance:</span></span>

```csharp
public static Task Focus(this ElementReference elementRef, IJSRuntime jsRuntime)
{
    return jsRuntime.InvokeAsync<object>(
        "exampleJsFunctions.focusElement", elementRef);
}
```

<span data-ttu-id="f78d9-176">Metoda jest wywoływana bezpośrednio dla obiektu.</span><span class="sxs-lookup"><span data-stu-id="f78d9-176">The method is called directly on the object.</span></span> <span data-ttu-id="f78d9-177">W poniższym przykładzie przyjęto założenie `Focus` , że metoda statyczna `JsInteropClasses` jest dostępna z przestrzeni nazw:</span><span class="sxs-lookup"><span data-stu-id="f78d9-177">The following example assumes that the static `Focus` method is available from the `JsInteropClasses` namespace:</span></span>

[!code-cshtml[](javascript-interop/samples_snapshot/component2.razor?highlight=1,4,12)]

> [!IMPORTANT]
> <span data-ttu-id="f78d9-178">`username` Zmienna jest wypełniana tylko po wyrenderowaniu składnika.</span><span class="sxs-lookup"><span data-stu-id="f78d9-178">The `username` variable is only populated after the component is rendered.</span></span> <span data-ttu-id="f78d9-179">W przypadku przekazanie niewypełnionego `ElementReference` kodu JavaScript kod JavaScript otrzymuje `null`wartość.</span><span class="sxs-lookup"><span data-stu-id="f78d9-179">If an unpopulated `ElementReference` is passed to JavaScript code, the JavaScript code receives a value of `null`.</span></span> <span data-ttu-id="f78d9-180">Aby manipulować odwołaniami do elementów po zakończeniu renderowania składnika (aby ustawić początkowy fokus w elemencie), `OnAfterRenderAsync` Użyj metody lub [](xref:blazor/components#lifecycle-methods) `OnAfterRender` cyklu życia składnika.</span><span class="sxs-lookup"><span data-stu-id="f78d9-180">To manipulate element references after the component has finished rendering (to set the initial focus on an element) use the `OnAfterRenderAsync` or `OnAfterRender` [component lifecycle methods](xref:blazor/components#lifecycle-methods).</span></span>

## <a name="invoke-net-methods-from-javascript-functions"></a><span data-ttu-id="f78d9-181">Wywoływanie metod .NET przy użyciu funkcji języka JavaScript</span><span class="sxs-lookup"><span data-stu-id="f78d9-181">Invoke .NET methods from JavaScript functions</span></span>

### <a name="static-net-method-call"></a><span data-ttu-id="f78d9-182">Statyczne wywołanie metody .NET</span><span class="sxs-lookup"><span data-stu-id="f78d9-182">Static .NET method call</span></span>

<span data-ttu-id="f78d9-183">Aby wywołać statyczną metodę .NET z poziomu języka JavaScript, `DotNet.invokeMethod` Użyj `DotNet.invokeMethodAsync` funkcji or.</span><span class="sxs-lookup"><span data-stu-id="f78d9-183">To invoke a static .NET method from JavaScript, use the `DotNet.invokeMethod` or `DotNet.invokeMethodAsync` functions.</span></span> <span data-ttu-id="f78d9-184">Przekaż identyfikator metody statycznej, która ma być wywoływana, nazwę zestawu zawierającego funkcję i wszelkie argumenty.</span><span class="sxs-lookup"><span data-stu-id="f78d9-184">Pass in the identifier of the static method you wish to call, the name of the assembly containing the function, and any arguments.</span></span> <span data-ttu-id="f78d9-185">Wersja asynchroniczna jest preferowana do obsługi scenariuszy po stronie serwera.</span><span class="sxs-lookup"><span data-stu-id="f78d9-185">The asynchronous version is preferred to support server-side scenarios.</span></span> <span data-ttu-id="f78d9-186">Aby wywołać metodę .NET z języka JavaScript, metoda .NET musi być publiczna, statyczna i mieć `[JSInvokable]` atrybut.</span><span class="sxs-lookup"><span data-stu-id="f78d9-186">To invoke a .NET method from JavaScript, the .NET method must be public, static, and have the `[JSInvokable]` attribute.</span></span> <span data-ttu-id="f78d9-187">Domyślnie identyfikator metody jest nazwą metody, ale można określić inny identyfikator przy użyciu `JSInvokableAttribute` konstruktora.</span><span class="sxs-lookup"><span data-stu-id="f78d9-187">By default, the method identifier is the method name, but you can specify a different identifier using the `JSInvokableAttribute` constructor.</span></span> <span data-ttu-id="f78d9-188">Wywoływanie otwartych metod ogólnych nie jest obecnie obsługiwane.</span><span class="sxs-lookup"><span data-stu-id="f78d9-188">Calling open generic methods isn't currently supported.</span></span>

<span data-ttu-id="f78d9-189">Przykładowa aplikacja zawiera C# metodę zwracającą tablicę `int`s.</span><span class="sxs-lookup"><span data-stu-id="f78d9-189">The sample app includes a C# method to return an array of `int`s.</span></span> <span data-ttu-id="f78d9-190">Ten `JSInvokable` atrybut jest stosowany do metody.</span><span class="sxs-lookup"><span data-stu-id="f78d9-190">The `JSInvokable` attribute is applied to the method.</span></span>

<span data-ttu-id="f78d9-191">*Strony/JsInterop. Razor*:</span><span class="sxs-lookup"><span data-stu-id="f78d9-191">*Pages/JsInterop.razor*:</span></span>

[!code-cshtml[](./common/samples/3.x/BlazorSample/Pages/JsInterop.razor?name=snippet_JSInterop2&highlight=7-11)]

<span data-ttu-id="f78d9-192">Kod JavaScript obsługiwany przez klienta wywołuje metodę C# .NET.</span><span class="sxs-lookup"><span data-stu-id="f78d9-192">JavaScript served to the client invokes the C# .NET method.</span></span>

<span data-ttu-id="f78d9-193">*wwwroot/exampleJsInterop. js*:</span><span class="sxs-lookup"><span data-stu-id="f78d9-193">*wwwroot/exampleJsInterop.js*:</span></span>

[!code-javascript[](./common/samples/3.x/BlazorSample/wwwroot/exampleJsInterop.js?highlight=8-14)]

<span data-ttu-id="f78d9-194">Gdy zostanie wybrany przycisk **ReturnArrayAsync Wyzwalaj metodę statyczną .NET** , sprawdź dane wyjściowe konsoli w narzędziach deweloperskich sieci Web w przeglądarce.</span><span class="sxs-lookup"><span data-stu-id="f78d9-194">When the **Trigger .NET static method ReturnArrayAsync** button is selected, examine the console output in the browser's web developer tools.</span></span>

<span data-ttu-id="f78d9-195">Dane wyjściowe konsoli są następujące:</span><span class="sxs-lookup"><span data-stu-id="f78d9-195">The console output is:</span></span>

```console
Array(4) [ 1, 2, 3, 4 ]
```

<span data-ttu-id="f78d9-196">Czwarta wartość tablicy jest wypychana do tablicy (`data.push(4);`) zwracanej przez. `ReturnArrayAsync`</span><span class="sxs-lookup"><span data-stu-id="f78d9-196">The fourth array value is pushed to the array (`data.push(4);`) returned by `ReturnArrayAsync`.</span></span>

### <a name="instance-method-call"></a><span data-ttu-id="f78d9-197">Wywołanie metody wystąpienia</span><span class="sxs-lookup"><span data-stu-id="f78d9-197">Instance method call</span></span>

<span data-ttu-id="f78d9-198">Można również wywołać metody wystąpienia platformy .NET z poziomu języka JavaScript.</span><span class="sxs-lookup"><span data-stu-id="f78d9-198">You can also call .NET instance methods from JavaScript.</span></span> <span data-ttu-id="f78d9-199">Aby wywołać metodę wystąpienia platformy .NET z poziomu języka JavaScript:</span><span class="sxs-lookup"><span data-stu-id="f78d9-199">To invoke a .NET instance method from JavaScript:</span></span>

* <span data-ttu-id="f78d9-200">Przekaż wystąpienie programu .NET do języka JavaScript, zawijając je `DotNetObjectReference` w wystąpieniu.</span><span class="sxs-lookup"><span data-stu-id="f78d9-200">Pass the .NET instance to JavaScript by wrapping it in a `DotNetObjectReference` instance.</span></span> <span data-ttu-id="f78d9-201">Wystąpienie programu .NET jest przesyłane przez odwołanie do języka JavaScript.</span><span class="sxs-lookup"><span data-stu-id="f78d9-201">The .NET instance is passed by reference to JavaScript.</span></span>
* <span data-ttu-id="f78d9-202">Wywołaj metody wystąpienia platformy .NET w wystąpieniu przy `invokeMethod` użyciu `invokeMethodAsync` funkcji lub.</span><span class="sxs-lookup"><span data-stu-id="f78d9-202">Invoke .NET instance methods on the instance using the `invokeMethod` or `invokeMethodAsync` functions.</span></span> <span data-ttu-id="f78d9-203">Wystąpienie programu .NET może być również przekazywać jako argument podczas wywoływania innych metod .NET z JavaScript.</span><span class="sxs-lookup"><span data-stu-id="f78d9-203">The .NET instance can also be passed as an argument when invoking other .NET methods from JavaScript.</span></span>

> [!NOTE]
> <span data-ttu-id="f78d9-204">Przykładowa aplikacja rejestruje komunikaty do konsoli po stronie klienta.</span><span class="sxs-lookup"><span data-stu-id="f78d9-204">The sample app logs messages to the client-side console.</span></span> <span data-ttu-id="f78d9-205">W poniższych przykładach zademonstrowanych przez przykładową aplikację można sprawdzić dane wyjściowe konsoli przeglądarki w narzędziach deweloperskich w przeglądarce.</span><span class="sxs-lookup"><span data-stu-id="f78d9-205">For the following examples demonstrated by the sample app, examine the browser's console output in the browser's developer tools.</span></span>

<span data-ttu-id="f78d9-206">Po wybraniu `ExampleJsInterop.CallHelloHelperSayHello` przycisku **Wyzwalaj metodę wystąpienia .NET HelloHelper. sayHello** jest wywoływana i przekazuje nazwę, `Blazor`do metody.</span><span class="sxs-lookup"><span data-stu-id="f78d9-206">When the **Trigger .NET instance method HelloHelper.SayHello** button is selected, `ExampleJsInterop.CallHelloHelperSayHello` is called and passes a name, `Blazor`, to the method.</span></span>

<span data-ttu-id="f78d9-207">*Strony/JsInterop. Razor*:</span><span class="sxs-lookup"><span data-stu-id="f78d9-207">*Pages/JsInterop.razor*:</span></span>

[!code-cshtml[](./common/samples/3.x/BlazorSample/Pages/JsInterop.razor?name=snippet_JSInterop3&highlight=8-9)]

<span data-ttu-id="f78d9-208">`CallHelloHelperSayHello`wywołuje funkcję `sayHello` JavaScript z nowym `HelloHelper`wystąpieniem.</span><span class="sxs-lookup"><span data-stu-id="f78d9-208">`CallHelloHelperSayHello` invokes the JavaScript function `sayHello` with a new instance of `HelloHelper`.</span></span>

<span data-ttu-id="f78d9-209">*JsInteropClasses/ExampleJsInterop.cs*:</span><span class="sxs-lookup"><span data-stu-id="f78d9-209">*JsInteropClasses/ExampleJsInterop.cs*:</span></span>

[!code-csharp[](./common/samples/3.x/BlazorSample/JsInteropClasses/ExampleJsInterop.cs?name=snippet1&highlight=10-16)]

<span data-ttu-id="f78d9-210">*wwwroot/exampleJsInterop. js*:</span><span class="sxs-lookup"><span data-stu-id="f78d9-210">*wwwroot/exampleJsInterop.js*:</span></span>

[!code-javascript[](./common/samples/3.x/BlazorSample/wwwroot/exampleJsInterop.js?highlight=15-18)]

<span data-ttu-id="f78d9-211">Nazwa jest przenoszona do `HelloHelper`konstruktora, który `HelloHelper.Name` ustawia właściwość.</span><span class="sxs-lookup"><span data-stu-id="f78d9-211">The name is passed to `HelloHelper`'s constructor, which sets the `HelloHelper.Name` property.</span></span> <span data-ttu-id="f78d9-212">Gdy funkcja `sayHello` JavaScript jest wykonywana, `HelloHelper.SayHello` zwraca `Hello, {Name}!` komunikat, który jest zapisywana w konsoli przez funkcję JavaScript.</span><span class="sxs-lookup"><span data-stu-id="f78d9-212">When the JavaScript function `sayHello` is executed, `HelloHelper.SayHello` returns the `Hello, {Name}!` message, which is written to the console by the JavaScript function.</span></span>

<span data-ttu-id="f78d9-213">*JsInteropClasses/HelloHelper.cs*:</span><span class="sxs-lookup"><span data-stu-id="f78d9-213">*JsInteropClasses/HelloHelper.cs*:</span></span>

[!code-csharp[](./common/samples/3.x/BlazorSample/JsInteropClasses/HelloHelper.cs?name=snippet1&highlight=5,10-11)]

<span data-ttu-id="f78d9-214">Dane wyjściowe konsoli w narzędziach deweloperskich sieci Web w przeglądarce:</span><span class="sxs-lookup"><span data-stu-id="f78d9-214">Console output in the browser's web developer tools:</span></span>

```console
Hello, Blazor!
```

## <a name="share-interop-code-in-a-class-library"></a><span data-ttu-id="f78d9-215">Udostępnianie kodu międzyoperacyjnego w bibliotece klas</span><span class="sxs-lookup"><span data-stu-id="f78d9-215">Share interop code in a class library</span></span>

<span data-ttu-id="f78d9-216">Kod międzyoperacyjny JavaScript może być uwzględniony w bibliotece klas, co umożliwia udostępnianie kodu w pakiecie NuGet.</span><span class="sxs-lookup"><span data-stu-id="f78d9-216">JavaScript interop code can be included in a class library, which allows you to share the code in a NuGet package.</span></span>

<span data-ttu-id="f78d9-217">Biblioteka klas obsługuje Osadzanie zasobów JavaScript w skompilowanym zestawie.</span><span class="sxs-lookup"><span data-stu-id="f78d9-217">The class library handles embedding JavaScript resources in the built assembly.</span></span> <span data-ttu-id="f78d9-218">Pliki JavaScript są umieszczane w folderze *wwwroot* .</span><span class="sxs-lookup"><span data-stu-id="f78d9-218">The JavaScript files are placed in the *wwwroot* folder.</span></span> <span data-ttu-id="f78d9-219">Narzędzia te zajmują się osadzaniem zasobów podczas kompilowania biblioteki.</span><span class="sxs-lookup"><span data-stu-id="f78d9-219">The tooling takes care of embedding the resources when the library is built.</span></span>

<span data-ttu-id="f78d9-220">Skompilowany pakiet NuGet jest przywoływany w pliku projektu aplikacji w taki sam sposób, w jaki jest przywoływany każdy pakiet NuGet.</span><span class="sxs-lookup"><span data-stu-id="f78d9-220">The built NuGet package is referenced in the app's project file the same way that any NuGet package is referenced.</span></span> <span data-ttu-id="f78d9-221">Po przywróceniu pakietu kod aplikacji może być wywoływany w języku JavaScript, tak jakby C#był.</span><span class="sxs-lookup"><span data-stu-id="f78d9-221">After the package is restored, app code can call into JavaScript as if it were C#.</span></span>

<span data-ttu-id="f78d9-222">Aby uzyskać więcej informacji, zobacz <xref:blazor/class-libraries>.</span><span class="sxs-lookup"><span data-stu-id="f78d9-222">For more information, see <xref:blazor/class-libraries>.</span></span>

## <a name="harden-js-interop-calls"></a><span data-ttu-id="f78d9-223">Zabezpieczenia wywołań międzyoperacyjnych w ramach funkcjonalności JS</span><span class="sxs-lookup"><span data-stu-id="f78d9-223">Harden JS interop calls</span></span>

<span data-ttu-id="f78d9-224">Usługa JS Interop może zakończyć się niepowodzeniem z powodu błędów sieci i powinna być traktowana jako niezawodna.</span><span class="sxs-lookup"><span data-stu-id="f78d9-224">JS interop may fail due to networking errors and should be treated as unreliable.</span></span> <span data-ttu-id="f78d9-225">Domyślnie aplikacja serwera Blazor przeprowadzi limit czasu wywołań międzyoperacyjnych JS na serwerze po jednej minucie.</span><span class="sxs-lookup"><span data-stu-id="f78d9-225">By default, a Blazor Server app times out JS interop calls on the server after one minute.</span></span> <span data-ttu-id="f78d9-226">Jeśli aplikacja może tolerować bardziej agresywny limit czasu, na przykład 10 sekund, należy ustawić limit czasu przy użyciu jednej z następujących metod:</span><span class="sxs-lookup"><span data-stu-id="f78d9-226">If an app can tolerate a more aggressive timeout, such as 10 seconds, set the timeout using one of the following approaches:</span></span>

* <span data-ttu-id="f78d9-227">Globalnie w `Startup.ConfigureServices`programie Określ limit czasu:</span><span class="sxs-lookup"><span data-stu-id="f78d9-227">Globally in `Startup.ConfigureServices`, specify the timeout:</span></span>

  ```csharp
  services.AddServerSideBlazor(
      options => options.JSInteropDefaultCallTimeout = TimeSpan.FromSeconds({SECONDS}));
  ```

* <span data-ttu-id="f78d9-228">Dla wywołania w kodzie składnika pojedyncze wywołanie może określać limit czasu:</span><span class="sxs-lookup"><span data-stu-id="f78d9-228">Per-invocation in component code, a single call can specify the timeout:</span></span>

  ```csharp
  var result = await JSRuntime.InvokeAsync<string>("MyJSOperation", 
      TimeSpan.FromSeconds({SECONDS}), new[] { "Arg1" });
  ```

<span data-ttu-id="f78d9-229">Więcej informacji o wyczerpaniu zasobów znajduje się w <xref:security/blazor/server-side>temacie.</span><span class="sxs-lookup"><span data-stu-id="f78d9-229">For more information on resource exhaustion, see <xref:security/blazor/server-side>.</span></span>
