---
title: Razor Components JavaScript interop
author: guardrex
description: Dowiedz się, jak wywoływać funkcje języka JavaScript z .NET i .NET metody z kodu JavaScript w aplikacjach Blazor i składniki Razor.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 01/29/2019
uid: razor-components/javascript-interop
ms.openlocfilehash: 9f822fee8990b03ff15ffa9857a2ddd95328a6ce
ms.sourcegitcommit: af8a6eb5375ef547a52ffae22465e265837aa82b
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/12/2019
ms.locfileid: "56159267"
---
# <a name="razor-components-javascript-interop"></a><span data-ttu-id="0e2c1-103">Razor Components JavaScript interop</span><span class="sxs-lookup"><span data-stu-id="0e2c1-103">Razor Components JavaScript interop</span></span>

<span data-ttu-id="0e2c1-104">Przez [Javier Calvarro Nelson](https://github.com/javiercn), [Daniel Roth](https://github.com/danroth27), i [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="0e2c1-104">By [Javier Calvarro Nelson](https://github.com/javiercn), [Daniel Roth](https://github.com/danroth27), and [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="0e2c1-105">Aplikacja składniki Razor można wywołać funkcji języka JavaScript z .NET i .NET metody z kodu JavaScript.</span><span class="sxs-lookup"><span data-stu-id="0e2c1-105">A Razor Components app can invoke JavaScript functions from .NET and .NET methods from JavaScript code.</span></span>

## <a name="invoke-javascript-functions-from-net-methods"></a><span data-ttu-id="0e2c1-106">Wywoływanie funkcji języka JavaScript z metod .NET</span><span class="sxs-lookup"><span data-stu-id="0e2c1-106">Invoke JavaScript functions from .NET methods</span></span>

<span data-ttu-id="0e2c1-107">Istnieją terminy, gdy jest wymagane do wywołania funkcji JavaScript kodu platformy .NET.</span><span class="sxs-lookup"><span data-stu-id="0e2c1-107">There are times when .NET code is required to call a JavaScript function.</span></span> <span data-ttu-id="0e2c1-108">Na przykład wywołanie JavaScript mogą uwidocznić możliwości przeglądarki lub funkcji z biblioteki JavaScript do aplikacji.</span><span class="sxs-lookup"><span data-stu-id="0e2c1-108">For example, a JavaScript call can expose browser capabilities or functionality from a JavaScript library to the app.</span></span>

<span data-ttu-id="0e2c1-109">Aby wywołać na język JavaScript z platformy .NET, należy użyć `IJSRuntime` abstrakcji.</span><span class="sxs-lookup"><span data-stu-id="0e2c1-109">To call into JavaScript from .NET, use the `IJSRuntime` abstraction.</span></span> <span data-ttu-id="0e2c1-110">`InvokeAsync<T>` Metody `IJSRuntime` przyjmuje identyfikator dla funkcji języka JavaScript do wywołania wraz z dowolnej liczby argumentów do serializacji JSON.</span><span class="sxs-lookup"><span data-stu-id="0e2c1-110">The `InvokeAsync<T>` method on `IJSRuntime` takes an identifier for the JavaScript function you wish to invoke along with any number of JSON-serializable arguments.</span></span> <span data-ttu-id="0e2c1-111">Identyfikator funkcji jest określana względem zakresu globalnego (`window`).</span><span class="sxs-lookup"><span data-stu-id="0e2c1-111">The function identifier is relative to the global scope (`window`).</span></span> <span data-ttu-id="0e2c1-112">Jeśli chcesz wywołać `window.someScope.someFunction`, identyfikator ma `someScope.someFunction`.</span><span class="sxs-lookup"><span data-stu-id="0e2c1-112">If you wish to call `window.someScope.someFunction`, the identifier is `someScope.someFunction`.</span></span> <span data-ttu-id="0e2c1-113">Nie ma potrzeby, aby zarejestrować funkcja jest wywoływana.</span><span class="sxs-lookup"><span data-stu-id="0e2c1-113">There's no need to register the function before it's called.</span></span> <span data-ttu-id="0e2c1-114">Zwracany typ `T` musi być również JSON do serializacji.</span><span class="sxs-lookup"><span data-stu-id="0e2c1-114">The return type `T` must also be JSON serializable.</span></span>

<span data-ttu-id="0e2c1-115">W przypadku aplikacji ASP.NET Core Razor składniki po stronie serwera:</span><span class="sxs-lookup"><span data-stu-id="0e2c1-115">For server-side ASP.NET Core Razor Components apps:</span></span>

* <span data-ttu-id="0e2c1-116">Wiele żądań użytkownika są przetwarzane przez aplikację po stronie serwera.</span><span class="sxs-lookup"><span data-stu-id="0e2c1-116">Multiple user requests are processed by the server-side app.</span></span> <span data-ttu-id="0e2c1-117">Nie wywołuj `JSRuntime.Current` w składniku do wywoływania funkcji języka JavaScript.</span><span class="sxs-lookup"><span data-stu-id="0e2c1-117">Don't call `JSRuntime.Current` in a component to invoke JavaScript functions.</span></span>
* <span data-ttu-id="0e2c1-118">Wstrzykiwanie `IJSRuntime` pozyskiwania i użycia wprowadzonego obiektu do wystawiania wywołań międzyoperacyjnych w języku JavaScript.</span><span class="sxs-lookup"><span data-stu-id="0e2c1-118">Inject the `IJSRuntime` abstraction and use the injected object to issue JavaScript interop calls.</span></span>

<span data-ttu-id="0e2c1-119">Poniższy przykład jest oparty na [TextDecoder](https://developer.mozilla.org/docs/Web/API/TextDecoder), eksperymentalne dekodera oparte na języku JavaScript.</span><span class="sxs-lookup"><span data-stu-id="0e2c1-119">The following example is based on [TextDecoder](https://developer.mozilla.org/docs/Web/API/TextDecoder), an experimental JavaScript-based decoder.</span></span> <span data-ttu-id="0e2c1-120">W przykładzie pokazano, jak wywołać funkcję JavaScript z C# metody.</span><span class="sxs-lookup"><span data-stu-id="0e2c1-120">The example demonstrates how to invoke a JavaScript function from a C# method.</span></span> <span data-ttu-id="0e2c1-121">Funkcja języka JavaScript akceptuje tablicy bajtów z C# metody dekoduje tablicy i zwraca tekst do składnika do wyświetlenia.</span><span class="sxs-lookup"><span data-stu-id="0e2c1-121">The JavaScript function accepts a byte array from a C# method, decodes the array, and returns the text to the component for display.</span></span>

<span data-ttu-id="0e2c1-122">Wewnątrz `<head>` elementu *wwwroot/index.html*, zapewnia funkcję, która używa `TextDecoder` zdekodować tablicy przekazany:</span><span class="sxs-lookup"><span data-stu-id="0e2c1-122">Inside the `<head>` element of *wwwroot/index.html*, provide a function that uses `TextDecoder` to decode a passed array:</span></span>

```html
<script>
  window.ConvertArray = (win1251Array) => {
    var win1251decoder = new TextDecoder('windows-1251');
    var bytes = new Uint8Array(win1251Array);
    var decodedArray = win1251decoder.decode(bytes);
    console.log(decodedArray);
    return decodedArray;
  };
</script>
```

<span data-ttu-id="0e2c1-123">Kod JavaScript, taki jak kod przedstawiony w poprzednim przykładzie, można również załadować plik JavaScript z odwołaniem do pliku skryptu w *wwwroot/index.html* pliku.</span><span class="sxs-lookup"><span data-stu-id="0e2c1-123">JavaScript code, such as the code shown in the preceding example, can also be loaded by a JavaScript file with a reference to the script file in the *wwwroot/index.html* file.</span></span>

<span data-ttu-id="0e2c1-124">Następujących składników:</span><span class="sxs-lookup"><span data-stu-id="0e2c1-124">The following component:</span></span>

* <span data-ttu-id="0e2c1-125">Wywołuje `ConvertArray` funkcję języka JavaScript za pomocą `JsRuntime` przycisku składnika (**konwersji tablicy**) jest zaznaczona.</span><span class="sxs-lookup"><span data-stu-id="0e2c1-125">Invokes the `ConvertArray` JavaScript function using `JsRuntime` when a component button (**Convert Array**) is selected.</span></span>
* <span data-ttu-id="0e2c1-126">Po wywołaniu funkcji JavaScript przekazana tablica jest konwertowana na ciąg.</span><span class="sxs-lookup"><span data-stu-id="0e2c1-126">After the JavaScript function is called, the passed array is converted into a string.</span></span> <span data-ttu-id="0e2c1-127">Ten ciąg jest zwracany do składnika do wyświetlenia.</span><span class="sxs-lookup"><span data-stu-id="0e2c1-127">The string is returned to the component for display.</span></span>

```cshtml
@page "/"
@using Microsoft.JSInterop;
@inject IJSRuntime JsRuntime;

<h1>Call JavaScript Function Example</h1>

<button type="button" class="btn btn-primary" onclick="@ConvertArray">
    Convert Array
</button>

<p class="mt-2" style="font-size:1.6em">
    <span class="badge badge-success">
        @ConvertedText
    </span>
</p>

@functions {
    // Quote (c)2005 Universal Pictures: Serenity
    // https://www.uphe.com/movies/serenity
    // David Krumholtz on IMDB: https://www.imdb.com/name/nm0472710/

    private MarkupString ConvertedText =
        new MarkupString("Select the <b>Convert Array</b> button.");

    private uint[] QuoteArray = new uint[]
        {
            60, 101, 109, 62, 67, 97, 110, 39, 116, 32, 115, 116, 111, 112, 32,
            116, 104, 101, 32, 115, 105, 103, 110, 97, 108, 44, 32, 77, 97,
            108, 46, 60, 47, 101, 109, 62, 32, 45, 32, 77, 114, 46, 32, 85, 110,
            105, 118, 101, 114, 115, 101, 10, 10,
        };

    async void ConvertArray()
    {
        var text =
            await JsRuntime.InvokeAsync<string>("ConvertArray", QuoteArray);

        ConvertedText = new MarkupString(text);

        StateHasChanged();
    }
}
```

<span data-ttu-id="0e2c1-128">W przypadku aplikacji po stronie klienta Blazor `IJSRuntime` abstrakcją jest dostępny z `JSRuntime.Current`, która odnosi się do bieżącego użytkownika żądania.</span><span class="sxs-lookup"><span data-stu-id="0e2c1-128">For client-side Blazor apps, the `IJSRuntime` abstraction is accessible from `JSRuntime.Current`, which refers to the current user's request.</span></span> <span data-ttu-id="0e2c1-129">Ponieważ istnieje tylko jeden użytkownik aplikacji Blazor po stronie klienta, za pomocą `JSRuntime.Current` do wywołania języka JavaScript działa normalnie.</span><span class="sxs-lookup"><span data-stu-id="0e2c1-129">Because there's only a single user of a client-side Blazor app, using `JSRuntime.Current` to invoke a JavaScript function works normally.</span></span> <span data-ttu-id="0e2c1-130">Używaj tylko `JSRuntime.Current` w aplikacjach Blazor po stronie klienta.</span><span class="sxs-lookup"><span data-stu-id="0e2c1-130">Only use `JSRuntime.Current` in client-side Blazor apps.</span></span>

<span data-ttu-id="0e2c1-131">W aplikacji przykładowej po stronie klienta, znajdująca się w tym temacie dwie funkcje języka JavaScript są dostępne dla aplikacji po stronie klienta, który wchodzić w interakcje z modelu DOM do odbierania danych wejściowych użytkownika i wyświetlania komunikatu powitalnego:</span><span class="sxs-lookup"><span data-stu-id="0e2c1-131">In the client-side sample app that accompanies this topic, two JavaScript functions are available to the client-side app that interact with the DOM to receive user input and display a welcome message:</span></span>

* <span data-ttu-id="0e2c1-132">`showPrompt` &ndash; Wyświetla monit o podanie, aby akceptować dane wejściowe użytkownika (nazwa użytkownika) i zwraca nazwę do obiektu wywołującego.</span><span class="sxs-lookup"><span data-stu-id="0e2c1-132">`showPrompt` &ndash; Produces a prompt to accept user input (the user's name) and returns the name to the caller.</span></span>
* <span data-ttu-id="0e2c1-133">`displayWelcome` &ndash; Przypisuje komunikat powitalny wywołujący do obiektu modelu DOM przy użyciu `id` z `welcome`.</span><span class="sxs-lookup"><span data-stu-id="0e2c1-133">`displayWelcome` &ndash; Assigns a welcome message from the caller to a DOM object with an `id` of `welcome`.</span></span>

<span data-ttu-id="0e2c1-134">*wwwroot/exampleJsInterop.js*:</span><span class="sxs-lookup"><span data-stu-id="0e2c1-134">*wwwroot/exampleJsInterop.js*:</span></span>

[!code-javascript[](./common/samples/3.x/BlazorSample/wwwroot/exampleJsInterop.js?highlight=2-7)]

<span data-ttu-id="0e2c1-135">Miejsce `<script>` tag, który odwołuje się do pliku JavaScript w *wwwroot/index.html* pliku:</span><span class="sxs-lookup"><span data-stu-id="0e2c1-135">Place the `<script>` tag that references the JavaScript file in the *wwwroot/index.html* file:</span></span>

[!code-html[](./common/samples/3.x/BlazorSample/wwwroot/index.html?highlight=16)]

<span data-ttu-id="0e2c1-136">Nie umieszczaj tag skryptu w pliku składnika, ponieważ tag skryptu nie można zaktualizować dynamicznie.</span><span class="sxs-lookup"><span data-stu-id="0e2c1-136">Don't place a script tag in a component file because the script tag can't be updated dynamically.</span></span>

<span data-ttu-id="0e2c1-137">.NET współdziałanie metod z funkcji języka JavaScript, przez wywołanie metody `InvokeAsync<T>` metody `IJSRuntime`.</span><span class="sxs-lookup"><span data-stu-id="0e2c1-137">.NET methods interop with the JavaScript functions by calling `InvokeAsync<T>` method on `IJSRuntime`.</span></span>

<span data-ttu-id="0e2c1-138">Przykładowa aplikacja korzysta z parą C# metod `Prompt` i `Display`, aby wywołać `showPrompt` i `displayWelcome` funkcje języka JavaScript:</span><span class="sxs-lookup"><span data-stu-id="0e2c1-138">The sample app uses a pair of C# methods, `Prompt` and `Display`, to invoke the `showPrompt` and `displayWelcome` JavaScript functions:</span></span>

<span data-ttu-id="0e2c1-139">*JsInteropClasses/ExampleJsInterop.cs*:</span><span class="sxs-lookup"><span data-stu-id="0e2c1-139">*JsInteropClasses/ExampleJsInterop.cs*:</span></span>

[!code-csharp[](./common/samples/3.x/BlazorSample/JsInteropClasses/ExampleJsInterop.cs?name=snippet1&highlight=6-8,14-16)]

<span data-ttu-id="0e2c1-140">`IJSRuntime` Abstrakcją jest asynchroniczne, aby zezwolić na potrzeby scenariuszy po stronie serwera.</span><span class="sxs-lookup"><span data-stu-id="0e2c1-140">The `IJSRuntime` abstraction is asynchronous to allow for server-side scenarios.</span></span> <span data-ttu-id="0e2c1-141">Jeśli aplikacja jest uruchamiana po stronie klienta, a użytkownik chce synchronicznie, wywołaj funkcję JavaScript takiej `IJSInProcessRuntime` i wywołać `Invoke<T>` zamiast tego.</span><span class="sxs-lookup"><span data-stu-id="0e2c1-141">If the app runs client-side and you want to invoke a JavaScript function synchronously, downcast to `IJSInProcessRuntime` and call `Invoke<T>` instead.</span></span> <span data-ttu-id="0e2c1-142">Firma Microsoft zaleca, większość JavaScript międzyoperacyjny bibliotek korzystające z asynchroniczne interfejsy API w celu zapewnienia biblioteki są dostępne we wszystkich scenariuszach, po stronie klienta i po stronie serwera.</span><span class="sxs-lookup"><span data-stu-id="0e2c1-142">We recommend that most JavaScript interop libraries use the async APIs to ensure the libraries are available in all scenarios, client-side or server-side.</span></span>

<span data-ttu-id="0e2c1-143">Przykładowa aplikacja zawiera składnik do zademonstrowania JS interop.</span><span class="sxs-lookup"><span data-stu-id="0e2c1-143">The sample app includes a component to demonstrate JS interop.</span></span> <span data-ttu-id="0e2c1-144">Składnik:</span><span class="sxs-lookup"><span data-stu-id="0e2c1-144">The component:</span></span>

* <span data-ttu-id="0e2c1-145">Odbiera dane wejściowe użytkownika za pośrednictwem wiersza JS.</span><span class="sxs-lookup"><span data-stu-id="0e2c1-145">Receives user input via a JS prompt.</span></span>
* <span data-ttu-id="0e2c1-146">Zwraca tekst do składnika do przetworzenia.</span><span class="sxs-lookup"><span data-stu-id="0e2c1-146">Returns the text to the component for processing.</span></span>
* <span data-ttu-id="0e2c1-147">Wywołania drugiej funkcji języka JS, która współdziała z modelu DOM w celu wyświetlania komunikatu powitalnego.</span><span class="sxs-lookup"><span data-stu-id="0e2c1-147">Calls a second JS function that interacts with the DOM to display a welcome message.</span></span>

<span data-ttu-id="0e2c1-148">*Pages/JSInterop.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="0e2c1-148">*Pages/JSInterop.cshtml*:</span></span>

[!code-cshtml[](./common/samples/3.x/BlazorSample/Pages/JsInterop.cshtml?start=1&end=21&highlight=2-3,9-11,13,16-20)]

1. <span data-ttu-id="0e2c1-149">Podczas `TriggerJsPrompt` jest wykonywana przez wybranie elementu **wyzwalacza JavaScript monitu** przycisku `ExampleJsInterop.Prompt` method in Class metoda C# kodu jest wywoływana.</span><span class="sxs-lookup"><span data-stu-id="0e2c1-149">When `TriggerJsPrompt` is executed by selecting the component's **Trigger JavaScript Prompt** button, the `ExampleJsInterop.Prompt` method in C# code is called.</span></span>
1. <span data-ttu-id="0e2c1-150">`Prompt` Metoda wykonuje kod JavaScript `showPrompt` funkcji podawany *wwwroot/exampleJsInterop.js* pliku.</span><span class="sxs-lookup"><span data-stu-id="0e2c1-150">The `Prompt` method executes the JavaScript `showPrompt` function provided in the *wwwroot/exampleJsInterop.js* file.</span></span>
1. <span data-ttu-id="0e2c1-151">`showPrompt` Funkcja akceptuje dane wejściowe użytkownika (nazwa użytkownika), który jest kodowany w formacie HTML i zwrócone do `Prompt` metody i ostatecznie z powrotem do składnika.</span><span class="sxs-lookup"><span data-stu-id="0e2c1-151">The `showPrompt` function accepts user input (the user's name), which is HTML-encoded and returned to the `Prompt` method and ultimately back to the component.</span></span> <span data-ttu-id="0e2c1-152">Składnik nazwy użytkownika są przechowywane w zmiennej lokalnej `name`.</span><span class="sxs-lookup"><span data-stu-id="0e2c1-152">The component stores the user's name in a local variable, `name`.</span></span>
1. <span data-ttu-id="0e2c1-153">Ten ciąg jest przechowywany w `name` jest włączona do komunikat powitalny, który jest przekazywany do drugiej C# metody `ExampleJsInterop.Display`.</span><span class="sxs-lookup"><span data-stu-id="0e2c1-153">The string stored in `name` is incorporated into a welcome message, which is passed to a second C# method, `ExampleJsInterop.Display`.</span></span>
1. <span data-ttu-id="0e2c1-154">`Display` wywołuje funkcję JavaScript `displayWelcome`, która renderuje wiadomość powitalna w tagu nagłówka.</span><span class="sxs-lookup"><span data-stu-id="0e2c1-154">`Display` calls a JavaScript function, `displayWelcome`, which renders the welcome message into a heading tag.</span></span>

## <a name="capture-references-to-elements"></a><span data-ttu-id="0e2c1-155">Przechwytywanie odwołania do elementów</span><span class="sxs-lookup"><span data-stu-id="0e2c1-155">Capture references to elements</span></span>

<span data-ttu-id="0e2c1-156">Niektóre [międzyoperacyjnego JavaScript](xref:razor-components/javascript-interop) scenariusze wymagają odwołania do elementów HTML.</span><span class="sxs-lookup"><span data-stu-id="0e2c1-156">Some [JavaScript interop](xref:razor-components/javascript-interop) scenarios require references to HTML elements.</span></span> <span data-ttu-id="0e2c1-157">Na przykład biblioteka interfejsów użytkownika może wymagać odwołanie do elementu inicjowania lub może być konieczne wywołania interfejsów API jak polecenie elementu, takie jak `focus` lub `play`.</span><span class="sxs-lookup"><span data-stu-id="0e2c1-157">For example, a UI library may require an element reference for initialization, or you might need to call command-like APIs on an element, such as `focus` or `play`.</span></span>

<span data-ttu-id="0e2c1-158">Odwołania do elementów HTML w składniku można przechwycić poprzez dodanie `ref` atrybutu do elementu HTML, a następnie zdefiniowanie pola typu `ElementRef` którego nazwa odpowiada wartości `ref` atrybutu.</span><span class="sxs-lookup"><span data-stu-id="0e2c1-158">You can capture references to HTML elements in a component by adding a `ref` attribute to the HTML element and then defining a field of type `ElementRef` whose name matches the value of the `ref` attribute.</span></span>

<span data-ttu-id="0e2c1-159">Poniższy przykład pokazuje, przechwytywanie odwołanie do elementu wprowadzania nazwy użytkownika:</span><span class="sxs-lookup"><span data-stu-id="0e2c1-159">The following example shows capturing a reference to the username input element:</span></span>

```csharp
<input ref="username" ... />

@functions {
    ElementRef username;
}
```

> [!NOTE]
> <span data-ttu-id="0e2c1-160">Czy **nie** używać odwołań do elementu przechwyconych jako sposobu wypełniania DOM.</span><span class="sxs-lookup"><span data-stu-id="0e2c1-160">Do **not** use captured element references as a way of populating the DOM.</span></span> <span data-ttu-id="0e2c1-161">Ten sposób może kolidować z modelu deklaratywne renderowania.</span><span class="sxs-lookup"><span data-stu-id="0e2c1-161">Doing so may interfere with the declarative rendering model.</span></span>

<span data-ttu-id="0e2c1-162">Jeśli chodzi o kodu platformy .NET, `ElementRef` jest nieprzezroczysta dojście.</span><span class="sxs-lookup"><span data-stu-id="0e2c1-162">As far as .NET code is concerned, an `ElementRef` is an opaque handle.</span></span> <span data-ttu-id="0e2c1-163">*Tylko* może z nim zrobić jest przekazywany do kodu w języku JavaScript za pomocą międzyoperacyjności języka JavaScript.</span><span class="sxs-lookup"><span data-stu-id="0e2c1-163">The *only* thing you can do with it is pass it through to JavaScript code via JavaScript interop.</span></span> <span data-ttu-id="0e2c1-164">Jeśli tak zrobisz, otrzyma kod JavaScript po stronie `HTMLElement` wystąpienia, którego można użyć z normalnym DOM interfejsów API.</span><span class="sxs-lookup"><span data-stu-id="0e2c1-164">When you do so, the JavaScript-side code receives an `HTMLElement` instance, which it can use with normal DOM APIs.</span></span>

<span data-ttu-id="0e2c1-165">Na przykład poniższy kod definiuje metody rozszerzenia platformy .NET, która umożliwia ustawienie fokusu w elemencie:</span><span class="sxs-lookup"><span data-stu-id="0e2c1-165">For example, the following code defines a .NET extension method that enables setting the focus on an element:</span></span>

<span data-ttu-id="0e2c1-166">*mylib.js*:</span><span class="sxs-lookup"><span data-stu-id="0e2c1-166">*mylib.js*:</span></span>

```javascript
window.myLib = {
  focusElement : function (element) {
    element.focus();
  }
}
```

<span data-ttu-id="0e2c1-167">*ElementRefExtensions.cs*:</span><span class="sxs-lookup"><span data-stu-id="0e2c1-167">*ElementRefExtensions.cs*:</span></span>

```csharp
using Microsoft.AspNetCore.Blazor;
using Microsoft.JSInterop;
using System.Threading.Tasks;

namespace MyLib
{
    public static class MyLibElementRefExtensions
    {
        public static Task Focus(this ElementRef elementRef)
        {
            return JSRuntime.Current.InvokeAsync<object>("myLib.focusElement", elementRef);
        }
    }
}
```

<span data-ttu-id="0e2c1-168">Teraz Ty możesz skoncentrować się dane wejściowe w żadnym składniki:</span><span class="sxs-lookup"><span data-stu-id="0e2c1-168">Now you can focus inputs in any of your components:</span></span>

```cshtml
@using MyLib

<input ref="username" />
<button onclick="@SetFocus">Set focus</button>

@functions {
    ElementRef username;

    void SetFocus()
    {
        username.Focus();
    }
}
```

> [!IMPORTANT]
> <span data-ttu-id="0e2c1-169">`username` Zmiennej tylko jest wypełniana po renderuje składnika i jego dane wyjściowe obejmują `<input>` elementu.</span><span class="sxs-lookup"><span data-stu-id="0e2c1-169">The `username` variable is only populated after the component renders and its output includes the `<input>` element.</span></span> <span data-ttu-id="0e2c1-170">Jeśli zostanie podjęta próba przekazania unpopulated `ElementRef` do kodu JavaScript, otrzyma kod JavaScript `null`.</span><span class="sxs-lookup"><span data-stu-id="0e2c1-170">If you try to pass an unpopulated `ElementRef` to JavaScript code, the JavaScript code receives `null`.</span></span> <span data-ttu-id="0e2c1-171">Do manipulowania odwołania do elementu, po zakończeniu renderowania (w celu ustawienie początkowy fokus w elemencie) Użyj składnika `OnAfterRenderAsync` lub `OnAfterRender` [metody cyklu życia składników](xref:razor-components/components#lifecycle-methods).</span><span class="sxs-lookup"><span data-stu-id="0e2c1-171">To manipulate element references after the component has finished rendering (to set the initial focus on an element) use the `OnAfterRenderAsync` or `OnAfterRender` [component lifecycle methods](xref:razor-components/components#lifecycle-methods).</span></span>

## <a name="invoke-net-methods-from-javascript-functions"></a><span data-ttu-id="0e2c1-172">Wywoływanie metod .NET z funkcji języka JavaScript</span><span class="sxs-lookup"><span data-stu-id="0e2c1-172">Invoke .NET methods from JavaScript functions</span></span>

### <a name="static-net-method-call"></a><span data-ttu-id="0e2c1-173">Statyczne wywołanie metody .NET</span><span class="sxs-lookup"><span data-stu-id="0e2c1-173">Static .NET method call</span></span>

<span data-ttu-id="0e2c1-174">Aby wywołać metodę statyczną .NET poziomu języka JavaScript, należy użyć `DotNet.invokeMethod` lub `DotNet.invokeMethodAsync` funkcji.</span><span class="sxs-lookup"><span data-stu-id="0e2c1-174">To invoke a static .NET method from JavaScript, use the `DotNet.invokeMethod` or `DotNet.invokeMethodAsync` functions.</span></span> <span data-ttu-id="0e2c1-175">Przekaż identyfikator statyczna metoda, którą chcesz wywołać, nazwa zestawu zawierającego funkcję i żadnych argumentów.</span><span class="sxs-lookup"><span data-stu-id="0e2c1-175">Pass in the identifier of the static method you wish to call, the name of the assembly containing the function, and any arguments.</span></span> <span data-ttu-id="0e2c1-176">Ponownie wersji asynchronicznej jest preferowane w scenariuszach, po stronie serwera.</span><span class="sxs-lookup"><span data-stu-id="0e2c1-176">Again, the async version is preferred to support server-side scenarios.</span></span> <span data-ttu-id="0e2c1-177">Jako element wywoływalny poziomu języka JavaScript, metoda .NET musi być publiczne, statycznych i dekorowane za pomocą `[JSInvokable]`.</span><span class="sxs-lookup"><span data-stu-id="0e2c1-177">To be invokable from JavaScript, the .NET method must be public, static, and decorated with `[JSInvokable]`.</span></span> <span data-ttu-id="0e2c1-178">Domyślnie identyfikator metody jest nazwa metody, ale można określić przy użyciu innego identyfikatora `JSInvokableAttribute` konstruktora.</span><span class="sxs-lookup"><span data-stu-id="0e2c1-178">By default, the method identifier is the method name, but you can specify a different identifier using the `JSInvokableAttribute` constructor.</span></span> <span data-ttu-id="0e2c1-179">Wywoływanie metod ogólnych open nie jest obecnie obsługiwane.</span><span class="sxs-lookup"><span data-stu-id="0e2c1-179">Calling open generic methods isn't currently supported.</span></span>

<span data-ttu-id="0e2c1-180">Przykładowa aplikacja zawiera C# metodę, aby zwrócić tablicę `int`s.</span><span class="sxs-lookup"><span data-stu-id="0e2c1-180">The sample app includes a C# method to return an array of `int`s.</span></span> <span data-ttu-id="0e2c1-181">Metoda zostanie nadany `JSInvokable` atrybutu.</span><span class="sxs-lookup"><span data-stu-id="0e2c1-181">The method is decorated with the `JSInvokable` attribute.</span></span>

<span data-ttu-id="0e2c1-182">*Pages/JsInterop.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="0e2c1-182">*Pages/JsInterop.cshtml*:</span></span>

[!code-cshtml[](./common/samples/3.x/BlazorSample/Pages/JsInterop.cshtml?start=47&end=58&highlight=7-11)]

<span data-ttu-id="0e2c1-183">JavaScript obsłużonych klient wywołuje C# metoda .NET.</span><span class="sxs-lookup"><span data-stu-id="0e2c1-183">JavaScript served to the client invokes the C# .NET method.</span></span>

<span data-ttu-id="0e2c1-184">*wwwroot/exampleJsInterop.js*:</span><span class="sxs-lookup"><span data-stu-id="0e2c1-184">*wwwroot/exampleJsInterop.js*:</span></span>

[!code-javascript[](./common/samples/3.x/BlazorSample/wwwroot/exampleJsInterop.js?highlight=8-12)]

<span data-ttu-id="0e2c1-185">Gdy **.NET wyzwalacza statycznej metody ReturnArrayAsync** przycisk jest zaznaczony, sprawdź dane wyjściowe konsoli narzędzi dla deweloperów sieci web w przeglądarce:</span><span class="sxs-lookup"><span data-stu-id="0e2c1-185">When the **Trigger .NET static method ReturnArrayAsync** button is selected, examine the console output in the browser's web developer tools:</span></span>

```console
Array(4) [ 1, 2, 3, 4 ]
```

<span data-ttu-id="0e2c1-186">Czwarta wartość tablicy są wypychane do tablicy (`data.push(4);`) zwróciło `ReturnArrayAsync`.</span><span class="sxs-lookup"><span data-stu-id="0e2c1-186">The fourth array value is pushed to the array (`data.push(4);`) returned by `ReturnArrayAsync`.</span></span>

### <a name="instance-method-call"></a><span data-ttu-id="0e2c1-187">Wywołanie metody wystąpienia</span><span class="sxs-lookup"><span data-stu-id="0e2c1-187">Instance method call</span></span>

<span data-ttu-id="0e2c1-188">Można również wywołać metody wystąpienia .NET poziomu języka JavaScript.</span><span class="sxs-lookup"><span data-stu-id="0e2c1-188">You can also call .NET instance methods from JavaScript.</span></span> <span data-ttu-id="0e2c1-189">Aby wywołać metodę wystąpienia .NET poziomu języka JavaScript, najpierw przejść wystąpienia platformy .NET w kodzie JavaScript opakowując go w `DotNetObjectRef` wystąpienia.</span><span class="sxs-lookup"><span data-stu-id="0e2c1-189">To invoke a .NET instance method from JavaScript, first pass the .NET instance to JavaScript by wrapping it in a `DotNetObjectRef` instance.</span></span> <span data-ttu-id="0e2c1-190">Wystąpienie platformy .NET jest przekazywany przez odwołanie w kodzie JavaScript i można wywołać metody wystąpienia platformy .NET przy użyciu wystąpienia `invokeMethod` lub `invokeMethodAsync` funkcji.</span><span class="sxs-lookup"><span data-stu-id="0e2c1-190">The .NET instance is passed by reference to JavaScript, and you can invoke .NET instance methods on the instance using the `invokeMethod` or `invokeMethodAsync` functions.</span></span> <span data-ttu-id="0e2c1-191">Wystąpienie programu .NET może być przekazywany jako argument, podczas wywoływania innych metod .NET poziomu języka JavaScript.</span><span class="sxs-lookup"><span data-stu-id="0e2c1-191">The .NET instance can also be passed as an argument when invoking other .NET methods from JavaScript.</span></span>

> [!NOTE]
> <span data-ttu-id="0e2c1-192">Przykładowa aplikacja rejestruje komunikaty w konsoli po stronie klienta.</span><span class="sxs-lookup"><span data-stu-id="0e2c1-192">The sample app logs messages to the client-side console.</span></span> <span data-ttu-id="0e2c1-193">W poniższych przykładach pokazano przez aplikację przykładową Sprawdź dane wyjściowe konsoli w przeglądarce w narzędzia dla deweloperów w przeglądarce.</span><span class="sxs-lookup"><span data-stu-id="0e2c1-193">For the following examples demonstrated by the sample app, examine the browser's console output in the browser's developer tools.</span></span>

<span data-ttu-id="0e2c1-194">Gdy **metodę wystąpienia .NET wyzwalacza HelloHelper.SayHello** przycisk jest zaznaczony, `ExampleJsInterop.CallHelloHelperSayHello` nosi nazwę i przekazuje nazwę `Blazor`, do metody.</span><span class="sxs-lookup"><span data-stu-id="0e2c1-194">When the **Trigger .NET instance method HelloHelper.SayHello** button is selected, `ExampleJsInterop.CallHelloHelperSayHello` is called and passes a name, `Blazor`, to the method.</span></span>

<span data-ttu-id="0e2c1-195">*Pages/JsInterop.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="0e2c1-195">*Pages/JsInterop.cshtml*:</span></span>

[!code-cshtml[](./common/samples/3.x/BlazorSample/Pages/JsInterop.cshtml?start=60&end=69&highlight=8)]

<span data-ttu-id="0e2c1-196">`CallHelloHelperSayHello` wywołuje funkcję JavaScript `sayHello` o nowe wystąpienie klasy `HelloHelper`.</span><span class="sxs-lookup"><span data-stu-id="0e2c1-196">`CallHelloHelperSayHello` invokes the JavaScript function `sayHello` with a new instance of `HelloHelper`.</span></span>

<span data-ttu-id="0e2c1-197">*JsInteropClasses/ExampleJsInterop.cs*:</span><span class="sxs-lookup"><span data-stu-id="0e2c1-197">*JsInteropClasses/ExampleJsInterop.cs*:</span></span>

[!code-csharp[](./common/samples/3.x/BlazorSample/JsInteropClasses/ExampleJsInterop.cs?name=snippet1&highlight=19-25)]

<span data-ttu-id="0e2c1-198">*wwwroot/exampleJsInterop.js*:</span><span class="sxs-lookup"><span data-stu-id="0e2c1-198">*wwwroot/exampleJsInterop.js*:</span></span>

[!code-javascript[](./common/samples/3.x/BlazorSample/wwwroot/exampleJsInterop.js?highlight=15-17)]

<span data-ttu-id="0e2c1-199">Nazwa jest przekazywany do `HelloHelper`przez konstruktora, który ustawia `HelloHelper.Name` właściwości.</span><span class="sxs-lookup"><span data-stu-id="0e2c1-199">The name is passed to `HelloHelper`'s constructor, which sets the `HelloHelper.Name` property.</span></span> <span data-ttu-id="0e2c1-200">Gdy funkcja języka JavaScript `sayHello` jest wykonywane, `HelloHelper.SayHello` zwraca `Hello, {Name}!` komunikat jest wyświetlony w konsoli przez funkcję języka JavaScript.</span><span class="sxs-lookup"><span data-stu-id="0e2c1-200">When the JavaScript function `sayHello` is executed, `HelloHelper.SayHello` returns the `Hello, {Name}!` message, which is written to the console by the JavaScript function.</span></span>

<span data-ttu-id="0e2c1-201">*JsInteropClasses/HelloHelper.cs*:</span><span class="sxs-lookup"><span data-stu-id="0e2c1-201">*JsInteropClasses/HelloHelper.cs*:</span></span>

[!code-csharp[](./common/samples/3.x/BlazorSample/JsInteropClasses/HelloHelper.cs?name=snippet1&highlight=5,10-11)]

<span data-ttu-id="0e2c1-202">Konsola danych wyjściowych w narzędzia dla deweloperów sieci web w przeglądarce:</span><span class="sxs-lookup"><span data-stu-id="0e2c1-202">Console output in the browser's web developer tools:</span></span>

```console
Hello, Blazor!
```

## <a name="share-interop-code-in-a-razor-component-class-library"></a><span data-ttu-id="0e2c1-203">Kód Interop SE udziału w bibliotece klas składników Razor</span><span class="sxs-lookup"><span data-stu-id="0e2c1-203">Share interop code in a Razor Component class library</span></span>

<span data-ttu-id="0e2c1-204">Biblioteki klas Razor składnika mogą być dołączane międzyoperacyjnego kodu JavaScript (`dotnet new blazorlib`), co pozwala na udostępnianie kodu w pakiecie NuGet.</span><span class="sxs-lookup"><span data-stu-id="0e2c1-204">JavaScript interop code can be included in a Razor Component class library (`dotnet new blazorlib`), which allows you to share the code in a NuGet package.</span></span>

<span data-ttu-id="0e2c1-205">Biblioteki klas Razor składnik obsługuje osadzanie zasobów JavaScript skompilowany zestaw.</span><span class="sxs-lookup"><span data-stu-id="0e2c1-205">The Razor Component class library handles embedding JavaScript resources in the built assembly.</span></span> <span data-ttu-id="0e2c1-206">Pliki JavaScript są umieszczane w *wwwroot* folder i narzędzi dba o osadzaniu zasobów podczas kompilowania biblioteki.</span><span class="sxs-lookup"><span data-stu-id="0e2c1-206">The JavaScript files are placed in the *wwwroot* folder, and the tooling takes care of embedding the resources when the library is built.</span></span>

<span data-ttu-id="0e2c1-207">Skompilowany pakiet NuGet odwołuje się do pliku projektu aplikacji, tak samo, jak odwołuje się do dowolnego normalnego pakietu NuGet.</span><span class="sxs-lookup"><span data-stu-id="0e2c1-207">The built NuGet package is referenced in the project file of the app just as any normal NuGet package is referenced.</span></span> <span data-ttu-id="0e2c1-208">Po przywróceniu aplikacji kod aplikacji może wywołać na język JavaScript, tak jakby C#.</span><span class="sxs-lookup"><span data-stu-id="0e2c1-208">After the app has been restored, app code can call into JavaScript as if it were C#.</span></span>
