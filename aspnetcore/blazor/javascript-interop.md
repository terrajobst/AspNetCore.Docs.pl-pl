---
title: ASP.NET Core Blazor JavaScript interop
author: guardrex
description: Dowiedz się, jak wywoływać funkcje języka JavaScript z .NET i .NET metody z kodu JavaScript w aplikacjach Blazor.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 07/02/2019
uid: blazor/javascript-interop
ms.openlocfilehash: 5d90a83acae3864c40ce38b6259d1938e56a2c54
ms.sourcegitcommit: 0b9e767a09beaaaa4301915cdda9ef69daaf3ff2
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/03/2019
ms.locfileid: "67538543"
---
# <a name="aspnet-core-blazor-javascript-interop"></a><span data-ttu-id="f9a44-103">ASP.NET Core Blazor JavaScript interop</span><span class="sxs-lookup"><span data-stu-id="f9a44-103">ASP.NET Core Blazor JavaScript interop</span></span>

<span data-ttu-id="f9a44-104">Przez [Javier Calvarro Nelson](https://github.com/javiercn), [Daniel Roth](https://github.com/danroth27), i [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="f9a44-104">By [Javier Calvarro Nelson](https://github.com/javiercn), [Daniel Roth](https://github.com/danroth27), and [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="f9a44-105">Aplikacja Blazor można wywoływać funkcje języka JavaScript z .NET i .NET metody z kodu JavaScript.</span><span class="sxs-lookup"><span data-stu-id="f9a44-105">A Blazor app can invoke JavaScript functions from .NET and .NET methods from JavaScript code.</span></span>

## <a name="invoke-javascript-functions-from-net-methods"></a><span data-ttu-id="f9a44-106">Wywoływanie funkcji języka JavaScript z metod .NET</span><span class="sxs-lookup"><span data-stu-id="f9a44-106">Invoke JavaScript functions from .NET methods</span></span>

<span data-ttu-id="f9a44-107">Istnieją terminy, gdy jest wymagane do wywołania funkcji JavaScript kodu platformy .NET.</span><span class="sxs-lookup"><span data-stu-id="f9a44-107">There are times when .NET code is required to call a JavaScript function.</span></span> <span data-ttu-id="f9a44-108">Na przykład wywołanie JavaScript mogą uwidocznić możliwości przeglądarki lub funkcji z biblioteki JavaScript do aplikacji.</span><span class="sxs-lookup"><span data-stu-id="f9a44-108">For example, a JavaScript call can expose browser capabilities or functionality from a JavaScript library to the app.</span></span>

<span data-ttu-id="f9a44-109">Aby wywołać na język JavaScript z platformy .NET, należy użyć `IJSRuntime` abstrakcji.</span><span class="sxs-lookup"><span data-stu-id="f9a44-109">To call into JavaScript from .NET, use the `IJSRuntime` abstraction.</span></span> <span data-ttu-id="f9a44-110">`InvokeAsync<T>` Metoda pobiera identyfikator dla funkcji języka JavaScript, którą chcesz wywołać wraz z dowolnej liczby argumentów do serializacji JSON.</span><span class="sxs-lookup"><span data-stu-id="f9a44-110">The `InvokeAsync<T>` method takes an identifier for the JavaScript function that you wish to invoke along with any number of JSON-serializable arguments.</span></span> <span data-ttu-id="f9a44-111">Identyfikator funkcji jest określana względem zakresu globalnego (`window`).</span><span class="sxs-lookup"><span data-stu-id="f9a44-111">The function identifier is relative to the global scope (`window`).</span></span> <span data-ttu-id="f9a44-112">Jeśli chcesz wywołać `window.someScope.someFunction`, identyfikator ma `someScope.someFunction`.</span><span class="sxs-lookup"><span data-stu-id="f9a44-112">If you wish to call `window.someScope.someFunction`, the identifier is `someScope.someFunction`.</span></span> <span data-ttu-id="f9a44-113">Nie ma potrzeby, aby zarejestrować funkcja jest wywoływana.</span><span class="sxs-lookup"><span data-stu-id="f9a44-113">There's no need to register the function before it's called.</span></span> <span data-ttu-id="f9a44-114">Zwracany typ `T` musi być również JSON do serializacji.</span><span class="sxs-lookup"><span data-stu-id="f9a44-114">The return type `T` must also be JSON serializable.</span></span>

<span data-ttu-id="f9a44-115">W przypadku aplikacji po stronie serwera:</span><span class="sxs-lookup"><span data-stu-id="f9a44-115">For server-side apps:</span></span>

* <span data-ttu-id="f9a44-116">Wiele żądań użytkownika są przetwarzane przez aplikację po stronie serwera.</span><span class="sxs-lookup"><span data-stu-id="f9a44-116">Multiple user requests are processed by the server-side app.</span></span> <span data-ttu-id="f9a44-117">Nie wywołuj `JSRuntime.Current` w składniku do wywoływania funkcji języka JavaScript.</span><span class="sxs-lookup"><span data-stu-id="f9a44-117">Don't call `JSRuntime.Current` in a component to invoke JavaScript functions.</span></span>
* <span data-ttu-id="f9a44-118">Wstrzykiwanie `IJSRuntime` pozyskiwania i użycia wprowadzonego obiektu do wystawiania wywołań międzyoperacyjnych w języku JavaScript.</span><span class="sxs-lookup"><span data-stu-id="f9a44-118">Inject the `IJSRuntime` abstraction and use the injected object to issue JavaScript interop calls.</span></span>
* <span data-ttu-id="f9a44-119">Gdy aplikacja Blazor jest prerendering, wywołanie JavaScript nie jest możliwe, ponieważ nie został utworzony połączenia za pośrednictwem przeglądarki.</span><span class="sxs-lookup"><span data-stu-id="f9a44-119">While a Blazor app is prerendering, calling into JavaScript isn't possible because a connection with the browser hasn't been established.</span></span> <span data-ttu-id="f9a44-120">Aby uzyskać więcej informacji, zobacz [Wykryj, kiedy aplikacja Blazor jest prerendering](#detect-when-a-blazor-app-is-prerendering) sekcji.</span><span class="sxs-lookup"><span data-stu-id="f9a44-120">For more information, see the [Detect when a Blazor app is prerendering](#detect-when-a-blazor-app-is-prerendering) section.</span></span>

<span data-ttu-id="f9a44-121">Poniższy przykład jest oparty na [TextDecoder](https://developer.mozilla.org/docs/Web/API/TextDecoder), eksperymentalne dekodera oparte na języku JavaScript.</span><span class="sxs-lookup"><span data-stu-id="f9a44-121">The following example is based on [TextDecoder](https://developer.mozilla.org/docs/Web/API/TextDecoder), an experimental JavaScript-based decoder.</span></span> <span data-ttu-id="f9a44-122">W przykładzie pokazano, jak wywołać funkcję JavaScript z C# metody.</span><span class="sxs-lookup"><span data-stu-id="f9a44-122">The example demonstrates how to invoke a JavaScript function from a C# method.</span></span> <span data-ttu-id="f9a44-123">Funkcja języka JavaScript akceptuje tablicy bajtów z C# metody dekoduje tablicy i zwraca tekst do składnika do wyświetlenia.</span><span class="sxs-lookup"><span data-stu-id="f9a44-123">The JavaScript function accepts a byte array from a C# method, decodes the array, and returns the text to the component for display.</span></span>

<span data-ttu-id="f9a44-124">Wewnątrz `<head>` elementu *wwwroot/index.html* (Blazor po stronie klienta) lub *Pages/_Host.cshtml* (Blazor po stronie serwera), zapewnia funkcję, która używa `TextDecoder` zdekodować przekazany Tablica:</span><span class="sxs-lookup"><span data-stu-id="f9a44-124">Inside the `<head>` element of *wwwroot/index.html* (Blazor client-side) or *Pages/_Host.cshtml* (Blazor server-side), provide a function that uses `TextDecoder` to decode a passed array:</span></span>

[!code-html[](javascript-interop/samples_snapshot/index-script.html)]

<span data-ttu-id="f9a44-125">Kod JavaScript, taki jak kod przedstawiony w poprzednim przykładzie, można również załadować z pliku JavaScript (*js*) za pomocą odwołania do pliku skryptu:</span><span class="sxs-lookup"><span data-stu-id="f9a44-125">JavaScript code, such as the code shown in the preceding example, can also be loaded from a JavaScript file (*.js*) with a reference to the script file:</span></span>

```html
<script src="exampleJsInterop.js"></script>
```

<span data-ttu-id="f9a44-126">Następujących składników:</span><span class="sxs-lookup"><span data-stu-id="f9a44-126">The following component:</span></span>

* <span data-ttu-id="f9a44-127">Wywołuje `ConvertArray` funkcję języka JavaScript za pomocą `JsRuntime` przycisku składnika (**konwersji tablicy**) jest zaznaczona.</span><span class="sxs-lookup"><span data-stu-id="f9a44-127">Invokes the `ConvertArray` JavaScript function using `JsRuntime` when a component button (**Convert Array**) is selected.</span></span>
* <span data-ttu-id="f9a44-128">Po wywołaniu funkcji JavaScript przekazana tablica jest konwertowana na ciąg.</span><span class="sxs-lookup"><span data-stu-id="f9a44-128">After the JavaScript function is called, the passed array is converted into a string.</span></span> <span data-ttu-id="f9a44-129">Ten ciąg jest zwracany do składnika do wyświetlenia.</span><span class="sxs-lookup"><span data-stu-id="f9a44-129">The string is returned to the component for display.</span></span>

[!code-cshtml[](javascript-interop/samples_snapshot/call-js-example.razor?highlight=2,34-35)]

<span data-ttu-id="f9a44-130">Aby użyć `IJSRuntime` abstrakcji, przyjmuje żadnego z następujących metod:</span><span class="sxs-lookup"><span data-stu-id="f9a44-130">To use the `IJSRuntime` abstraction, adopt any of the following approaches:</span></span>

* <span data-ttu-id="f9a44-131">Wstrzykiwanie `IJSRuntime` abstrakcji do składnika Razor ( *.razor*):</span><span class="sxs-lookup"><span data-stu-id="f9a44-131">Inject the `IJSRuntime` abstraction into the Razor component (*.razor*):</span></span>

  [!code-cshtml[](javascript-interop/samples_snapshot/inject-abstraction.razor?highlight=1)]

* <span data-ttu-id="f9a44-132">Wstrzykiwanie `IJSRuntime` abstrakcji do klasy ( *.cs*):</span><span class="sxs-lookup"><span data-stu-id="f9a44-132">Inject the `IJSRuntime` abstraction into a class (*.cs*):</span></span>

  [!code-csharp[](javascript-interop/samples_snapshot/inject-abstraction-class.cs?highlight=5)]

* <span data-ttu-id="f9a44-133">Dla dynamicznego generowania zawartości za pomocą [BuildRenderTree](xref:blazor/components#manual-rendertreebuilder-logic), użyj `[Inject]` atrybutu:</span><span class="sxs-lookup"><span data-stu-id="f9a44-133">For dynamic content generation with [BuildRenderTree](xref:blazor/components#manual-rendertreebuilder-logic), use the `[Inject]` attribute:</span></span>

  ```csharp
  [Inject]
  IJSRuntime JSRuntime { get; set; }
  ```

<span data-ttu-id="f9a44-134">W aplikacji przykładowej po stronie klienta, znajdująca się w tym temacie dwie funkcje języka JavaScript są dostępne dla aplikacji po stronie klienta, który wchodzić w interakcje z modelu DOM do odbierania danych wejściowych użytkownika i wyświetlania komunikatu powitalnego:</span><span class="sxs-lookup"><span data-stu-id="f9a44-134">In the client-side sample app that accompanies this topic, two JavaScript functions are available to the client-side app that interact with the DOM to receive user input and display a welcome message:</span></span>

* <span data-ttu-id="f9a44-135">`showPrompt` &ndash; Wyświetla monit o podanie, aby akceptować dane wejściowe użytkownika (nazwa użytkownika) i zwraca nazwę do obiektu wywołującego.</span><span class="sxs-lookup"><span data-stu-id="f9a44-135">`showPrompt` &ndash; Produces a prompt to accept user input (the user's name) and returns the name to the caller.</span></span>
* <span data-ttu-id="f9a44-136">`displayWelcome` &ndash; Przypisuje komunikat powitalny wywołujący do obiektu modelu DOM przy użyciu `id` z `welcome`.</span><span class="sxs-lookup"><span data-stu-id="f9a44-136">`displayWelcome` &ndash; Assigns a welcome message from the caller to a DOM object with an `id` of `welcome`.</span></span>

<span data-ttu-id="f9a44-137">*wwwroot/exampleJsInterop.js*:</span><span class="sxs-lookup"><span data-stu-id="f9a44-137">*wwwroot/exampleJsInterop.js*:</span></span>

[!code-javascript[](./common/samples/3.x/BlazorSample/wwwroot/exampleJsInterop.js?highlight=2-7)]

<span data-ttu-id="f9a44-138">Miejsce `<script>` tag, który odwołuje się do pliku JavaScript w *wwwroot/index.html* pliku (Blazor po stronie klienta) lub *Pages/_Host.cshtml* pliku (Blazor po stronie serwera).</span><span class="sxs-lookup"><span data-stu-id="f9a44-138">Place the `<script>` tag that references the JavaScript file in the *wwwroot/index.html* file (Blazor client-side) or *Pages/_Host.cshtml* file (Blazor server-side).</span></span>

<span data-ttu-id="f9a44-139">*Wwwroot/index.HTML* (Blazor po stronie klienta):</span><span class="sxs-lookup"><span data-stu-id="f9a44-139">*wwwroot/index.html* (Blazor client-side):</span></span>

[!code-html[](./common/samples/3.x/BlazorSample/wwwroot/index.html?highlight=15)]

<span data-ttu-id="f9a44-140">*Pages/_Host.cshtml* (Blazor po stronie serwera):</span><span class="sxs-lookup"><span data-stu-id="f9a44-140">*Pages/_Host.cshtml* (Blazor server-side):</span></span>

[!code-cshtml[](javascript-interop/samples_snapshot/_Host.cshtml?highlight=29)]

<span data-ttu-id="f9a44-141">Nie należy umieszczać `<script>` tagów w pliku składnika, ponieważ `<script>` tagów nie można zaktualizować dynamicznie.</span><span class="sxs-lookup"><span data-stu-id="f9a44-141">Don't place a `<script>` tag in a component file because the `<script>` tag can't be updated dynamically.</span></span>

<span data-ttu-id="f9a44-142">.NET współdziałanie metod za pomocą języka JavaScript działa w *exampleJsInterop.js* pliku przez wywołanie metody `IJSRuntime.InvokeAsync<T>`.</span><span class="sxs-lookup"><span data-stu-id="f9a44-142">.NET methods interop with the JavaScript functions in the *exampleJsInterop.js* file by calling `IJSRuntime.InvokeAsync<T>`.</span></span>

<span data-ttu-id="f9a44-143">`IJSRuntime` Abstrakcją jest asynchroniczne, aby zezwolić na potrzeby scenariuszy po stronie serwera.</span><span class="sxs-lookup"><span data-stu-id="f9a44-143">The `IJSRuntime` abstraction is asynchronous to allow for server-side scenarios.</span></span> <span data-ttu-id="f9a44-144">Jeśli aplikacja jest uruchamiana po stronie klienta, a użytkownik chce synchronicznie, wywołaj funkcję JavaScript takiej `IJSInProcessRuntime` i wywołać `Invoke<T>` zamiast tego.</span><span class="sxs-lookup"><span data-stu-id="f9a44-144">If the app runs client-side and you want to invoke a JavaScript function synchronously, downcast to `IJSInProcessRuntime` and call `Invoke<T>` instead.</span></span> <span data-ttu-id="f9a44-145">Firma Microsoft zaleca, aby większość bibliotek międzyoperacyjnego JavaScript upewnij się, że biblioteki są dostępne we wszystkich scenariuszach, po stronie klienta i po stronie serwera za pomocą async interfejsów API.</span><span class="sxs-lookup"><span data-stu-id="f9a44-145">We recommend that most JavaScript interop libraries use the async APIs to ensure that the libraries are available in all scenarios, client-side or server-side.</span></span>

<span data-ttu-id="f9a44-146">Przykładowa aplikacja zawiera składnik do zademonstrowania międzyoperacyjnego JavaScript.</span><span class="sxs-lookup"><span data-stu-id="f9a44-146">The sample app includes a component to demonstrate JavaScript interop.</span></span> <span data-ttu-id="f9a44-147">Składnik:</span><span class="sxs-lookup"><span data-stu-id="f9a44-147">The component:</span></span>

* <span data-ttu-id="f9a44-148">Odbiera dane wejściowe użytkownika za pośrednictwem wiersza kodu JavaScript.</span><span class="sxs-lookup"><span data-stu-id="f9a44-148">Receives user input via a JavaScript prompt.</span></span>
* <span data-ttu-id="f9a44-149">Zwraca tekst do składnika do przetworzenia.</span><span class="sxs-lookup"><span data-stu-id="f9a44-149">Returns the text to the component for processing.</span></span>
* <span data-ttu-id="f9a44-150">Wywołania drugiej funkcji języka JavaScript, która współdziała z modelu DOM w celu wyświetlania komunikatu powitalnego.</span><span class="sxs-lookup"><span data-stu-id="f9a44-150">Calls a second JavaScript function that interacts with the DOM to display a welcome message.</span></span>

<span data-ttu-id="f9a44-151">*Pages/JSInterop.razor*:</span><span class="sxs-lookup"><span data-stu-id="f9a44-151">*Pages/JSInterop.razor*:</span></span>

[!code-cshtml[](./common/samples/3.x/BlazorSample/Pages/JsInterop.razor?name=snippet_JSInterop1&highlight=3,19-21,23-25)]

1. <span data-ttu-id="f9a44-152">Gdy `TriggerJsPrompt` jest wykonywana przez wybranie elementu **wyzwalacza JavaScript monitu** przycisk JavaScript `showPrompt` funkcja udostępniana w *wwwroot/exampleJsInterop.js* plik jest wywoływana.</span><span class="sxs-lookup"><span data-stu-id="f9a44-152">When `TriggerJsPrompt` is executed by selecting the component's **Trigger JavaScript Prompt** button, the JavaScript `showPrompt` function provided in the *wwwroot/exampleJsInterop.js* file is called.</span></span>
1. <span data-ttu-id="f9a44-153">`showPrompt` Funkcja akceptuje dane wejściowe użytkownika (nazwa użytkownika), który jest kodowany w formacie HTML i zwrócone do składnika.</span><span class="sxs-lookup"><span data-stu-id="f9a44-153">The `showPrompt` function accepts user input (the user's name), which is HTML-encoded and returned to the component.</span></span> <span data-ttu-id="f9a44-154">Składnik nazwy użytkownika są przechowywane w zmiennej lokalnej `name`.</span><span class="sxs-lookup"><span data-stu-id="f9a44-154">The component stores the user's name in a local variable, `name`.</span></span>
1. <span data-ttu-id="f9a44-155">Ten ciąg jest przechowywany w `name` jest włączona do komunikat powitalny, który jest przekazywany do funkcji języka JavaScript, `displayWelcome`, który renderuje wiadomość powitalna w tagu nagłówka.</span><span class="sxs-lookup"><span data-stu-id="f9a44-155">The string stored in `name` is incorporated into a welcome message, which is passed to a JavaScript function, `displayWelcome`, which renders the welcome message into a heading tag.</span></span>

## <a name="call-a-void-javascript-function"></a><span data-ttu-id="f9a44-156">Wywołanie funkcji JavaScript void</span><span class="sxs-lookup"><span data-stu-id="f9a44-156">Call a void JavaScript function</span></span>

<span data-ttu-id="f9a44-157">Funkcji w języku JavaScript zwracające [void (0) / void 0](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Operators/void) lub [niezdefiniowane](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/undefined) są wywoływane przy użyciu `IJSRuntime.InvokeAsync<object>`, co powoduje zwrócenie `null`.</span><span class="sxs-lookup"><span data-stu-id="f9a44-157">JavaScript functions that return [void(0)/void 0](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Operators/void) or [undefined](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/undefined) are called with `IJSRuntime.InvokeAsync<object>`, which returns `null`.</span></span>

## <a name="detect-when-a-blazor-app-is-prerendering"></a><span data-ttu-id="f9a44-158">Wykryj, kiedy aplikacja Blazor jest prerendering</span><span class="sxs-lookup"><span data-stu-id="f9a44-158">Detect when a Blazor app is prerendering</span></span>
 
[!INCLUDE[](~/includes/blazor-prerendering.md)]

## <a name="capture-references-to-elements"></a><span data-ttu-id="f9a44-159">Przechwytywanie odwołania do elementów</span><span class="sxs-lookup"><span data-stu-id="f9a44-159">Capture references to elements</span></span>

<span data-ttu-id="f9a44-160">Niektóre [międzyoperacyjnego JavaScript](xref:blazor/javascript-interop) scenariusze wymagają odwołania do elementów HTML.</span><span class="sxs-lookup"><span data-stu-id="f9a44-160">Some [JavaScript interop](xref:blazor/javascript-interop) scenarios require references to HTML elements.</span></span> <span data-ttu-id="f9a44-161">Na przykład biblioteka interfejsów użytkownika może wymagać odwołanie do elementu inicjowania lub może być konieczne wywołania interfejsów API jak polecenie elementu, takie jak `focus` lub `play`.</span><span class="sxs-lookup"><span data-stu-id="f9a44-161">For example, a UI library may require an element reference for initialization, or you might need to call command-like APIs on an element, such as `focus` or `play`.</span></span>

<span data-ttu-id="f9a44-162">Istnieje możliwość przechwytywania odwołania do elementów HTML w składniku przy użyciu następujących podejść:</span><span class="sxs-lookup"><span data-stu-id="f9a44-162">You can capture references to HTML elements in a component using the following approach:</span></span>

* <span data-ttu-id="f9a44-163">Dodaj `@ref` atrybutu do elementu HTML.</span><span class="sxs-lookup"><span data-stu-id="f9a44-163">Add a `@ref` attribute to the HTML element.</span></span>
* <span data-ttu-id="f9a44-164">Definiowanie pola typu `ElementRef` którego nazwa odpowiada wartości `@ref` atrybutu.</span><span class="sxs-lookup"><span data-stu-id="f9a44-164">Define a field of type `ElementRef` whose name matches the value of the `@ref` attribute.</span></span>

<span data-ttu-id="f9a44-165">W poniższym przykładzie pokazano przechwytywania odwołania do `username` `<input>` elementu:</span><span class="sxs-lookup"><span data-stu-id="f9a44-165">The following example shows capturing a reference to the `username` `<input>` element:</span></span>

```cshtml
<input @ref="username" ... />

@code {
    ElementRef username;
}
```

> [!NOTE]
> <span data-ttu-id="f9a44-166">Czy **nie** używać odwołań do elementu przechwyconych jako sposobu wypełniania lub manipulowanie modelu DOM, gdy Blazor korzysta z elementów, do których odwołuje się.</span><span class="sxs-lookup"><span data-stu-id="f9a44-166">Do **not** use captured element references as a way of populating or manipulating the DOM when Blazor interacts with the elements referenced.</span></span> <span data-ttu-id="f9a44-167">Ten sposób może kolidować z modelu deklaratywne renderowania.</span><span class="sxs-lookup"><span data-stu-id="f9a44-167">Doing so may interfere with the declarative rendering model.</span></span>

<span data-ttu-id="f9a44-168">Jeśli chodzi o kodu platformy .NET, `ElementRef` jest nieprzezroczysta dojście.</span><span class="sxs-lookup"><span data-stu-id="f9a44-168">As far as .NET code is concerned, an `ElementRef` is an opaque handle.</span></span> <span data-ttu-id="f9a44-169">*Tylko* rzeczy można zrobić za pomocą `ElementRef` jest przekazywany do kodu w języku JavaScript za pomocą międzyoperacyjności języka JavaScript.</span><span class="sxs-lookup"><span data-stu-id="f9a44-169">The *only* thing you can do with `ElementRef` is pass it through to JavaScript code via JavaScript interop.</span></span> <span data-ttu-id="f9a44-170">Jeśli tak zrobisz, otrzyma kod JavaScript po stronie `HTMLElement` wystąpienia, którego można użyć z normalnym DOM interfejsów API.</span><span class="sxs-lookup"><span data-stu-id="f9a44-170">When you do so, the JavaScript-side code receives an `HTMLElement` instance, which it can use with normal DOM APIs.</span></span>

<span data-ttu-id="f9a44-171">Na przykład poniższy kod definiuje metody rozszerzenia platformy .NET, która umożliwia ustawienie fokusu w elemencie:</span><span class="sxs-lookup"><span data-stu-id="f9a44-171">For example, the following code defines a .NET extension method that enables setting the focus on an element:</span></span>

<span data-ttu-id="f9a44-172">*exampleJsInterop.js*:</span><span class="sxs-lookup"><span data-stu-id="f9a44-172">*exampleJsInterop.js*:</span></span>

```javascript
window.exampleJsFunctions = {
  focusElement : function (element) {
    element.focus();
  }
}
```

<span data-ttu-id="f9a44-173">Użyj `IJSRuntime.InvokeAsync<T>` i wywołać `exampleJsFunctions.focusElement` z `ElementRef` się elementu:</span><span class="sxs-lookup"><span data-stu-id="f9a44-173">Use `IJSRuntime.InvokeAsync<T>` and call `exampleJsFunctions.focusElement` with an `ElementRef` to focus an element:</span></span>

[!code-cshtml[](javascript-interop/samples_snapshot/component1.razor?highlight=1,3,7,11-12)]

<span data-ttu-id="f9a44-174">Można użyć metody rozszerzenia do skoncentrowania się element, należy utworzyć metody statyczne rozszerzenie, który odbiera `IJSRuntime` wystąpienie:</span><span class="sxs-lookup"><span data-stu-id="f9a44-174">To use an extension method to focus an element, create a static extension method that receives the `IJSRuntime` instance:</span></span>

```csharp
public static Task Focus(this ElementRef elementRef, IJSRuntime jsRuntime)
{
    return jsRuntime.InvokeAsync<object>(
        "exampleJsFunctions.focusElement", elementRef);
}
```

<span data-ttu-id="f9a44-175">Metoda jest wywoływana bezpośrednio na obiekcie.</span><span class="sxs-lookup"><span data-stu-id="f9a44-175">The method is called directly on the object.</span></span> <span data-ttu-id="f9a44-176">W poniższym przykładzie założono, że statyczne `Focus` metoda jest dostępna z `JsInteropClasses` przestrzeni nazw:</span><span class="sxs-lookup"><span data-stu-id="f9a44-176">The following example assumes that the static `Focus` method is available from the `JsInteropClasses` namespace:</span></span>

[!code-cshtml[](javascript-interop/samples_snapshot/component2.razor?highlight=1,4,8,12)]

> [!IMPORTANT]
> <span data-ttu-id="f9a44-177">`username` Zmiennej tylko jest wypełniana po składnik jest renderowany.</span><span class="sxs-lookup"><span data-stu-id="f9a44-177">The `username` variable is only populated after the component is rendered.</span></span> <span data-ttu-id="f9a44-178">Jeśli unpopulated `ElementRef` jest przekazywany do kodu JavaScript, JavaScript, kod otrzymuje wartość `null`.</span><span class="sxs-lookup"><span data-stu-id="f9a44-178">If an unpopulated `ElementRef` is passed to JavaScript code, the JavaScript code receives a value of `null`.</span></span> <span data-ttu-id="f9a44-179">Do manipulowania odwołania do elementu, po zakończeniu renderowania (w celu ustawienie początkowy fokus w elemencie) Użyj składnika `OnAfterRenderAsync` lub `OnAfterRender` [metody cyklu życia składników](xref:blazor/components#lifecycle-methods).</span><span class="sxs-lookup"><span data-stu-id="f9a44-179">To manipulate element references after the component has finished rendering (to set the initial focus on an element) use the `OnAfterRenderAsync` or `OnAfterRender` [component lifecycle methods](xref:blazor/components#lifecycle-methods).</span></span>

## <a name="invoke-net-methods-from-javascript-functions"></a><span data-ttu-id="f9a44-180">Wywoływanie metod .NET z funkcji języka JavaScript</span><span class="sxs-lookup"><span data-stu-id="f9a44-180">Invoke .NET methods from JavaScript functions</span></span>

### <a name="static-net-method-call"></a><span data-ttu-id="f9a44-181">Statyczne wywołanie metody .NET</span><span class="sxs-lookup"><span data-stu-id="f9a44-181">Static .NET method call</span></span>

<span data-ttu-id="f9a44-182">Aby wywołać metodę statyczną .NET poziomu języka JavaScript, należy użyć `DotNet.invokeMethod` lub `DotNet.invokeMethodAsync` funkcji.</span><span class="sxs-lookup"><span data-stu-id="f9a44-182">To invoke a static .NET method from JavaScript, use the `DotNet.invokeMethod` or `DotNet.invokeMethodAsync` functions.</span></span> <span data-ttu-id="f9a44-183">Przekaż identyfikator statyczna metoda, którą chcesz wywołać, nazwa zestawu zawierającego funkcję i żadnych argumentów.</span><span class="sxs-lookup"><span data-stu-id="f9a44-183">Pass in the identifier of the static method you wish to call, the name of the assembly containing the function, and any arguments.</span></span> <span data-ttu-id="f9a44-184">Wersja asynchroniczna jest preferowane w scenariuszach, po stronie serwera.</span><span class="sxs-lookup"><span data-stu-id="f9a44-184">The asynchronous version is preferred to support server-side scenarios.</span></span> <span data-ttu-id="f9a44-185">Aby wywołać metodę .NET poziomu języka JavaScript, metoda .NET muszą być publiczne, statyczne i `[JSInvokable]` atrybutu.</span><span class="sxs-lookup"><span data-stu-id="f9a44-185">To invoke a .NET method from JavaScript, the .NET method must be public, static, and have the `[JSInvokable]` attribute.</span></span> <span data-ttu-id="f9a44-186">Domyślnie identyfikator metody jest nazwa metody, ale można określić przy użyciu innego identyfikatora `JSInvokableAttribute` konstruktora.</span><span class="sxs-lookup"><span data-stu-id="f9a44-186">By default, the method identifier is the method name, but you can specify a different identifier using the `JSInvokableAttribute` constructor.</span></span> <span data-ttu-id="f9a44-187">Wywoływanie metod ogólnych open nie jest obecnie obsługiwane.</span><span class="sxs-lookup"><span data-stu-id="f9a44-187">Calling open generic methods isn't currently supported.</span></span>

<span data-ttu-id="f9a44-188">Przykładowa aplikacja zawiera C# metodę, aby zwrócić tablicę `int`s.</span><span class="sxs-lookup"><span data-stu-id="f9a44-188">The sample app includes a C# method to return an array of `int`s.</span></span> <span data-ttu-id="f9a44-189">`JSInvokable` Atrybut jest stosowany do metody.</span><span class="sxs-lookup"><span data-stu-id="f9a44-189">The `JSInvokable` attribute is applied to the method.</span></span>

<span data-ttu-id="f9a44-190">*Pages/JsInterop.razor*:</span><span class="sxs-lookup"><span data-stu-id="f9a44-190">*Pages/JsInterop.razor*:</span></span>

[!code-cshtml[](./common/samples/3.x/BlazorSample/Pages/JsInterop.razor?name=snippet_JSInterop2&highlight=7-11)]

<span data-ttu-id="f9a44-191">JavaScript obsłużonych klient wywołuje C# metoda .NET.</span><span class="sxs-lookup"><span data-stu-id="f9a44-191">JavaScript served to the client invokes the C# .NET method.</span></span>

<span data-ttu-id="f9a44-192">*wwwroot/exampleJsInterop.js*:</span><span class="sxs-lookup"><span data-stu-id="f9a44-192">*wwwroot/exampleJsInterop.js*:</span></span>

[!code-javascript[](./common/samples/3.x/BlazorSample/wwwroot/exampleJsInterop.js?highlight=8-14)]

<span data-ttu-id="f9a44-193">Gdy **.NET wyzwalacza statycznej metody ReturnArrayAsync** przycisk jest zaznaczony, sprawdź dane wyjściowe konsoli narzędzi dla deweloperów sieci web w przeglądarce.</span><span class="sxs-lookup"><span data-stu-id="f9a44-193">When the **Trigger .NET static method ReturnArrayAsync** button is selected, examine the console output in the browser's web developer tools.</span></span>

<span data-ttu-id="f9a44-194">Dane wyjściowe konsoli będą:</span><span class="sxs-lookup"><span data-stu-id="f9a44-194">The console output is:</span></span>

```console
Array(4) [ 1, 2, 3, 4 ]
```

<span data-ttu-id="f9a44-195">Czwarta wartość tablicy są wypychane do tablicy (`data.push(4);`) zwróciło `ReturnArrayAsync`.</span><span class="sxs-lookup"><span data-stu-id="f9a44-195">The fourth array value is pushed to the array (`data.push(4);`) returned by `ReturnArrayAsync`.</span></span>

### <a name="instance-method-call"></a><span data-ttu-id="f9a44-196">Wywołanie metody wystąpienia</span><span class="sxs-lookup"><span data-stu-id="f9a44-196">Instance method call</span></span>

<span data-ttu-id="f9a44-197">Można również wywołać metody wystąpienia .NET poziomu języka JavaScript.</span><span class="sxs-lookup"><span data-stu-id="f9a44-197">You can also call .NET instance methods from JavaScript.</span></span> <span data-ttu-id="f9a44-198">Aby wywołać metodę wystąpienia .NET poziomu języka JavaScript:</span><span class="sxs-lookup"><span data-stu-id="f9a44-198">To invoke a .NET instance method from JavaScript:</span></span>

* <span data-ttu-id="f9a44-199">Przekaż wystąpienie platformy .NET w kodzie JavaScript, opakowując go w `DotNetObjectRef` wystąpienia.</span><span class="sxs-lookup"><span data-stu-id="f9a44-199">Pass the .NET instance to JavaScript by wrapping it in a `DotNetObjectRef` instance.</span></span> <span data-ttu-id="f9a44-200">Wystąpienie platformy .NET jest przekazywany przez odwołanie w kodzie JavaScript.</span><span class="sxs-lookup"><span data-stu-id="f9a44-200">The .NET instance is passed by reference to JavaScript.</span></span>
* <span data-ttu-id="f9a44-201">Wywoływanie metod wystąpienia platformy .NET przy użyciu wystąpienia `invokeMethod` lub `invokeMethodAsync` funkcji.</span><span class="sxs-lookup"><span data-stu-id="f9a44-201">Invoke .NET instance methods on the instance using the `invokeMethod` or `invokeMethodAsync` functions.</span></span> <span data-ttu-id="f9a44-202">Wystąpienie programu .NET może być przekazywany jako argument, podczas wywoływania innych metod .NET poziomu języka JavaScript.</span><span class="sxs-lookup"><span data-stu-id="f9a44-202">The .NET instance can also be passed as an argument when invoking other .NET methods from JavaScript.</span></span>

> [!NOTE]
> <span data-ttu-id="f9a44-203">Przykładowa aplikacja rejestruje komunikaty w konsoli po stronie klienta.</span><span class="sxs-lookup"><span data-stu-id="f9a44-203">The sample app logs messages to the client-side console.</span></span> <span data-ttu-id="f9a44-204">W poniższych przykładach pokazano przez aplikację przykładową Sprawdź dane wyjściowe konsoli w przeglądarce w narzędzia dla deweloperów w przeglądarce.</span><span class="sxs-lookup"><span data-stu-id="f9a44-204">For the following examples demonstrated by the sample app, examine the browser's console output in the browser's developer tools.</span></span>

<span data-ttu-id="f9a44-205">Gdy **metodę wystąpienia .NET wyzwalacza HelloHelper.SayHello** przycisk jest zaznaczony, `ExampleJsInterop.CallHelloHelperSayHello` nosi nazwę i przekazuje nazwę `Blazor`, do metody.</span><span class="sxs-lookup"><span data-stu-id="f9a44-205">When the **Trigger .NET instance method HelloHelper.SayHello** button is selected, `ExampleJsInterop.CallHelloHelperSayHello` is called and passes a name, `Blazor`, to the method.</span></span>

<span data-ttu-id="f9a44-206">*Pages/JsInterop.razor*:</span><span class="sxs-lookup"><span data-stu-id="f9a44-206">*Pages/JsInterop.razor*:</span></span>

[!code-cshtml[](./common/samples/3.x/BlazorSample/Pages/JsInterop.razor?name=snippet_JSInterop3&highlight=8-9)]

<span data-ttu-id="f9a44-207">`CallHelloHelperSayHello` wywołuje funkcję JavaScript `sayHello` o nowe wystąpienie klasy `HelloHelper`.</span><span class="sxs-lookup"><span data-stu-id="f9a44-207">`CallHelloHelperSayHello` invokes the JavaScript function `sayHello` with a new instance of `HelloHelper`.</span></span>

<span data-ttu-id="f9a44-208">*JsInteropClasses/ExampleJsInterop.cs*:</span><span class="sxs-lookup"><span data-stu-id="f9a44-208">*JsInteropClasses/ExampleJsInterop.cs*:</span></span>

[!code-csharp[](./common/samples/3.x/BlazorSample/JsInteropClasses/ExampleJsInterop.cs?name=snippet1&highlight=10-16)]

<span data-ttu-id="f9a44-209">*wwwroot/exampleJsInterop.js*:</span><span class="sxs-lookup"><span data-stu-id="f9a44-209">*wwwroot/exampleJsInterop.js*:</span></span>

[!code-javascript[](./common/samples/3.x/BlazorSample/wwwroot/exampleJsInterop.js?highlight=15-18)]

<span data-ttu-id="f9a44-210">Nazwa jest przekazywany do `HelloHelper`przez konstruktora, który ustawia `HelloHelper.Name` właściwości.</span><span class="sxs-lookup"><span data-stu-id="f9a44-210">The name is passed to `HelloHelper`'s constructor, which sets the `HelloHelper.Name` property.</span></span> <span data-ttu-id="f9a44-211">Gdy funkcja języka JavaScript `sayHello` jest wykonywane, `HelloHelper.SayHello` zwraca `Hello, {Name}!` komunikat jest wyświetlony w konsoli przez funkcję języka JavaScript.</span><span class="sxs-lookup"><span data-stu-id="f9a44-211">When the JavaScript function `sayHello` is executed, `HelloHelper.SayHello` returns the `Hello, {Name}!` message, which is written to the console by the JavaScript function.</span></span>

<span data-ttu-id="f9a44-212">*JsInteropClasses/HelloHelper.cs*:</span><span class="sxs-lookup"><span data-stu-id="f9a44-212">*JsInteropClasses/HelloHelper.cs*:</span></span>

[!code-csharp[](./common/samples/3.x/BlazorSample/JsInteropClasses/HelloHelper.cs?name=snippet1&highlight=5,10-11)]

<span data-ttu-id="f9a44-213">Konsola danych wyjściowych w narzędzia dla deweloperów sieci web w przeglądarce:</span><span class="sxs-lookup"><span data-stu-id="f9a44-213">Console output in the browser's web developer tools:</span></span>

```console
Hello, Blazor!
```

## <a name="share-interop-code-in-a-class-library"></a><span data-ttu-id="f9a44-214">Kód Interop SE udziału w bibliotece klas</span><span class="sxs-lookup"><span data-stu-id="f9a44-214">Share interop code in a class library</span></span>

<span data-ttu-id="f9a44-215">Kód Interop SE JavaScript mogą być dołączane w bibliotece klas, co pozwala na udostępnianie kodu w pakiecie NuGet.</span><span class="sxs-lookup"><span data-stu-id="f9a44-215">JavaScript interop code can be included in a class library, which allows you to share the code in a NuGet package.</span></span>

<span data-ttu-id="f9a44-216">Biblioteka klas obsługuje osadzanie zasobów JavaScript skompilowany zestaw.</span><span class="sxs-lookup"><span data-stu-id="f9a44-216">The class library handles embedding JavaScript resources in the built assembly.</span></span> <span data-ttu-id="f9a44-217">Pliki JavaScript są umieszczane w *wwwroot* folderu.</span><span class="sxs-lookup"><span data-stu-id="f9a44-217">The JavaScript files are placed in the *wwwroot* folder.</span></span> <span data-ttu-id="f9a44-218">Osadzanie zasobów podczas kompilowania biblioteki zajmuje się narzędzi.</span><span class="sxs-lookup"><span data-stu-id="f9a44-218">The tooling takes care of embedding the resources when the library is built.</span></span>

<span data-ttu-id="f9a44-219">Skompilowany pakiet NuGet odwołuje się do pliku projektu aplikacji taki sam sposób, że odwołuje się do dowolnego pakietu NuGet.</span><span class="sxs-lookup"><span data-stu-id="f9a44-219">The built NuGet package is referenced in the app's project file the same way that any NuGet package is referenced.</span></span> <span data-ttu-id="f9a44-220">Po przywróceniu pakietu kodu aplikacji, można wywołać na język JavaScript, jak gdyby C#.</span><span class="sxs-lookup"><span data-stu-id="f9a44-220">After the package is restored, app code can call into JavaScript as if it were C#.</span></span>

<span data-ttu-id="f9a44-221">Aby uzyskać więcej informacji, zobacz <xref:blazor/class-libraries>.</span><span class="sxs-lookup"><span data-stu-id="f9a44-221">For more information, see <xref:blazor/class-libraries>.</span></span>
