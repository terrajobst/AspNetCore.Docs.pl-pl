---
title: Razor Components JavaScript interop
author: guardrex
description: Dowiedz się, jak wywoływać funkcje języka JavaScript z .NET i .NET metody z kodu JavaScript w aplikacjach Blazor i składniki Razor.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 01/29/2019
uid: razor-components/javascript-interop
ms.openlocfilehash: 9844bd5a63f1144867360ac4f31645d36710989c
ms.sourcegitcommit: d913bca90373c07f89b1d1df01af5fc01fc908ef
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/14/2019
ms.locfileid: "57978498"
---
# <a name="razor-components-javascript-interop"></a><span data-ttu-id="2ad6c-103">Razor Components JavaScript interop</span><span class="sxs-lookup"><span data-stu-id="2ad6c-103">Razor Components JavaScript interop</span></span>

<span data-ttu-id="2ad6c-104">Przez [Javier Calvarro Nelson](https://github.com/javiercn), [Daniel Roth](https://github.com/danroth27), i [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="2ad6c-104">By [Javier Calvarro Nelson](https://github.com/javiercn), [Daniel Roth](https://github.com/danroth27), and [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="2ad6c-105">Aplikacja składniki Razor można wywołać funkcji języka JavaScript z .NET i .NET metody z kodu JavaScript.</span><span class="sxs-lookup"><span data-stu-id="2ad6c-105">A Razor Components app can invoke JavaScript functions from .NET and .NET methods from JavaScript code.</span></span>

## <a name="invoke-javascript-functions-from-net-methods"></a><span data-ttu-id="2ad6c-106">Wywoływanie funkcji języka JavaScript z metod .NET</span><span class="sxs-lookup"><span data-stu-id="2ad6c-106">Invoke JavaScript functions from .NET methods</span></span>

<span data-ttu-id="2ad6c-107">Istnieją terminy, gdy jest wymagane do wywołania funkcji JavaScript kodu platformy .NET.</span><span class="sxs-lookup"><span data-stu-id="2ad6c-107">There are times when .NET code is required to call a JavaScript function.</span></span> <span data-ttu-id="2ad6c-108">Na przykład wywołanie JavaScript mogą uwidocznić możliwości przeglądarki lub funkcji z biblioteki JavaScript do aplikacji.</span><span class="sxs-lookup"><span data-stu-id="2ad6c-108">For example, a JavaScript call can expose browser capabilities or functionality from a JavaScript library to the app.</span></span>

<span data-ttu-id="2ad6c-109">Aby wywołać na język JavaScript z platformy .NET, należy użyć `IJSRuntime` abstrakcji.</span><span class="sxs-lookup"><span data-stu-id="2ad6c-109">To call into JavaScript from .NET, use the `IJSRuntime` abstraction.</span></span> <span data-ttu-id="2ad6c-110">`InvokeAsync<T>` Metoda pobiera identyfikator dla funkcji języka JavaScript, którą chcesz wywołać wraz z dowolnej liczby argumentów do serializacji JSON.</span><span class="sxs-lookup"><span data-stu-id="2ad6c-110">The `InvokeAsync<T>` method takes an identifier for the JavaScript function that you wish to invoke along with any number of JSON-serializable arguments.</span></span> <span data-ttu-id="2ad6c-111">Identyfikator funkcji jest określana względem zakresu globalnego (`window`).</span><span class="sxs-lookup"><span data-stu-id="2ad6c-111">The function identifier is relative to the global scope (`window`).</span></span> <span data-ttu-id="2ad6c-112">Jeśli chcesz wywołać `window.someScope.someFunction`, identyfikator ma `someScope.someFunction`.</span><span class="sxs-lookup"><span data-stu-id="2ad6c-112">If you wish to call `window.someScope.someFunction`, the identifier is `someScope.someFunction`.</span></span> <span data-ttu-id="2ad6c-113">Nie ma potrzeby, aby zarejestrować funkcja jest wywoływana.</span><span class="sxs-lookup"><span data-stu-id="2ad6c-113">There's no need to register the function before it's called.</span></span> <span data-ttu-id="2ad6c-114">Zwracany typ `T` musi być również JSON do serializacji.</span><span class="sxs-lookup"><span data-stu-id="2ad6c-114">The return type `T` must also be JSON serializable.</span></span>

<span data-ttu-id="2ad6c-115">W przypadku aplikacji ASP.NET Core Razor składniki po stronie serwera:</span><span class="sxs-lookup"><span data-stu-id="2ad6c-115">For server-side ASP.NET Core Razor Components apps:</span></span>

* <span data-ttu-id="2ad6c-116">Wiele żądań użytkownika są przetwarzane przez aplikację po stronie serwera.</span><span class="sxs-lookup"><span data-stu-id="2ad6c-116">Multiple user requests are processed by the server-side app.</span></span> <span data-ttu-id="2ad6c-117">Nie wywołuj `JSRuntime.Current` w składniku do wywoływania funkcji języka JavaScript.</span><span class="sxs-lookup"><span data-stu-id="2ad6c-117">Don't call `JSRuntime.Current` in a component to invoke JavaScript functions.</span></span>
* <span data-ttu-id="2ad6c-118">Wstrzykiwanie `IJSRuntime` pozyskiwania i użycia wprowadzonego obiektu do wystawiania wywołań międzyoperacyjnych w języku JavaScript.</span><span class="sxs-lookup"><span data-stu-id="2ad6c-118">Inject the `IJSRuntime` abstraction and use the injected object to issue JavaScript interop calls.</span></span>

<span data-ttu-id="2ad6c-119">Poniższy przykład jest oparty na [TextDecoder](https://developer.mozilla.org/docs/Web/API/TextDecoder), eksperymentalne dekodera oparte na języku JavaScript.</span><span class="sxs-lookup"><span data-stu-id="2ad6c-119">The following example is based on [TextDecoder](https://developer.mozilla.org/docs/Web/API/TextDecoder), an experimental JavaScript-based decoder.</span></span> <span data-ttu-id="2ad6c-120">W przykładzie pokazano, jak wywołać funkcję JavaScript z C# metody.</span><span class="sxs-lookup"><span data-stu-id="2ad6c-120">The example demonstrates how to invoke a JavaScript function from a C# method.</span></span> <span data-ttu-id="2ad6c-121">Funkcja języka JavaScript akceptuje tablicy bajtów z C# metody dekoduje tablicy i zwraca tekst do składnika do wyświetlenia.</span><span class="sxs-lookup"><span data-stu-id="2ad6c-121">The JavaScript function accepts a byte array from a C# method, decodes the array, and returns the text to the component for display.</span></span>

<span data-ttu-id="2ad6c-122">Wewnątrz `<head>` elementu *wwwroot/index.html*, zapewnia funkcję, która używa `TextDecoder` zdekodować tablicy przekazany:</span><span class="sxs-lookup"><span data-stu-id="2ad6c-122">Inside the `<head>` element of *wwwroot/index.html*, provide a function that uses `TextDecoder` to decode a passed array:</span></span>

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

<span data-ttu-id="2ad6c-123">Kod JavaScript, taki jak kod przedstawiony w poprzednim przykładzie, można również załadować z pliku JavaScript (*js*) za pomocą odwołania do pliku skryptu w *wwwroot/index.html* pliku:</span><span class="sxs-lookup"><span data-stu-id="2ad6c-123">JavaScript code, such as the code shown in the preceding example, can also be loaded from a JavaScript file (*.js*) with a reference to the script file in the *wwwroot/index.html* file:</span></span>

```html
<script src="exampleJsInterop.js"></script>
```

<span data-ttu-id="2ad6c-124">Następujących składników:</span><span class="sxs-lookup"><span data-stu-id="2ad6c-124">The following component:</span></span>

* <span data-ttu-id="2ad6c-125">Wywołuje `ConvertArray` funkcję języka JavaScript za pomocą `JsRuntime` przycisku składnika (**konwersji tablicy**) jest zaznaczona.</span><span class="sxs-lookup"><span data-stu-id="2ad6c-125">Invokes the `ConvertArray` JavaScript function using `JsRuntime` when a component button (**Convert Array**) is selected.</span></span>
* <span data-ttu-id="2ad6c-126">Po wywołaniu funkcji JavaScript przekazana tablica jest konwertowana na ciąg.</span><span class="sxs-lookup"><span data-stu-id="2ad6c-126">After the JavaScript function is called, the passed array is converted into a string.</span></span> <span data-ttu-id="2ad6c-127">Ten ciąg jest zwracany do składnika do wyświetlenia.</span><span class="sxs-lookup"><span data-stu-id="2ad6c-127">The string is returned to the component for display.</span></span>

```cshtml
@page "/"
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

<span data-ttu-id="2ad6c-128">Aby użyć `IJSRuntime` abstrakcji, przyjmuje żadnego z następujących metod:</span><span class="sxs-lookup"><span data-stu-id="2ad6c-128">To use the `IJSRuntime` abstraction, adopt any of the following approaches:</span></span>

* <span data-ttu-id="2ad6c-129">Wstrzykiwanie `IJSRuntime` abstrakcji w pliku Razor (*.cshtml*):</span><span class="sxs-lookup"><span data-stu-id="2ad6c-129">Inject the `IJSRuntime` abstraction into the Razor file (*.cshtml*):</span></span>

  ```cshtml
  @inject IJSRuntime JSRuntime

  @functions {
      public override void OnInit()
      {
          StocksService.OnStockTickerUpdated += stockUpdate =>
          {
              JSRuntime.InvokeAsync<object>(
                  "handleTickerChanged", 
                  stockUpdate.symbol, 
                  stockUpdate.price);
          };
      }
  }
  ```
* <span data-ttu-id="2ad6c-130">Wstrzykiwanie `IJSRuntime` abstrakcji do klasy (*.cs*):</span><span class="sxs-lookup"><span data-stu-id="2ad6c-130">Inject the `IJSRuntime` abstraction into a class (*.cs*):</span></span>

  ```csharp
  public class MyJsInterop
  {
      private readonly IJSRuntime _jsRuntime;

      public MyJsInterop(IJSRuntime jsRuntime)
      {
          _jsRuntime = jsRuntime;
      }

      public Task<string> DoSomething(string data)
      {
          // The doSomething JavaScript method is implemented 
          // in a JavaScript file, such as 'wwwroot/MyJsInterop.js'.
          return _jsRuntime.InvokeAsync<string>(
              "myJsFunctions.doSomething",
              data);
      }
  }
  ```
* <span data-ttu-id="2ad6c-131">Dla dynamicznego generowania zawartości za pomocą `BuildRenderTree`, użyj `[Inject]` atrybutu:</span><span class="sxs-lookup"><span data-stu-id="2ad6c-131">For dynamic content generation with `BuildRenderTree`, use the `[Inject]` attribute:</span></span>

  ```csharp
  [Inject] IJSRuntime JSRuntime { get; set; }
  ```

<span data-ttu-id="2ad6c-132">W aplikacji przykładowej po stronie klienta, znajdująca się w tym temacie dwie funkcje języka JavaScript są dostępne dla aplikacji po stronie klienta, który wchodzić w interakcje z modelu DOM do odbierania danych wejściowych użytkownika i wyświetlania komunikatu powitalnego:</span><span class="sxs-lookup"><span data-stu-id="2ad6c-132">In the client-side sample app that accompanies this topic, two JavaScript functions are available to the client-side app that interact with the DOM to receive user input and display a welcome message:</span></span>

* <span data-ttu-id="2ad6c-133">`showPrompt` &ndash; Wyświetla monit o podanie, aby akceptować dane wejściowe użytkownika (nazwa użytkownika) i zwraca nazwę do obiektu wywołującego.</span><span class="sxs-lookup"><span data-stu-id="2ad6c-133">`showPrompt` &ndash; Produces a prompt to accept user input (the user's name) and returns the name to the caller.</span></span>
* <span data-ttu-id="2ad6c-134">`displayWelcome` &ndash; Przypisuje komunikat powitalny wywołujący do obiektu modelu DOM przy użyciu `id` z `welcome`.</span><span class="sxs-lookup"><span data-stu-id="2ad6c-134">`displayWelcome` &ndash; Assigns a welcome message from the caller to a DOM object with an `id` of `welcome`.</span></span>

<span data-ttu-id="2ad6c-135">*wwwroot/exampleJsInterop.js*:</span><span class="sxs-lookup"><span data-stu-id="2ad6c-135">*wwwroot/exampleJsInterop.js*:</span></span>

[!code-javascript[](./common/samples/3.x/BlazorSample/wwwroot/exampleJsInterop.js?highlight=2-7)]

<span data-ttu-id="2ad6c-136">Miejsce `<script>` tag, który odwołuje się do pliku JavaScript w *wwwroot/index.html* pliku:</span><span class="sxs-lookup"><span data-stu-id="2ad6c-136">Place the `<script>` tag that references the JavaScript file in the *wwwroot/index.html* file:</span></span>

[!code-html[](./common/samples/3.x/BlazorSample/wwwroot/index.html?highlight=15)]

<span data-ttu-id="2ad6c-137">Nie należy umieszczać `<script>` tagów w pliku składnika, ponieważ `<script>` tagów nie można zaktualizować dynamicznie.</span><span class="sxs-lookup"><span data-stu-id="2ad6c-137">Don't place a `<script>` tag in a component file because the `<script>` tag can't be updated dynamically.</span></span>

<span data-ttu-id="2ad6c-138">.NET współdziałanie metod za pomocą języka JavaScript działa w *exampleJsInterop.js* pliku przez wywołanie metody `IJSRuntime.InvokeAsync<T>`.</span><span class="sxs-lookup"><span data-stu-id="2ad6c-138">.NET methods interop with the JavaScript functions in the *exampleJsInterop.js* file by calling `IJSRuntime.InvokeAsync<T>`.</span></span>

<span data-ttu-id="2ad6c-139">Przykładowa aplikacja korzysta z parą C# metod `Prompt` i `Display`, aby wywołać `showPrompt` i `displayWelcome` funkcje języka JavaScript:</span><span class="sxs-lookup"><span data-stu-id="2ad6c-139">The sample app uses a pair of C# methods, `Prompt` and `Display`, to invoke the `showPrompt` and `displayWelcome` JavaScript functions:</span></span>

<span data-ttu-id="2ad6c-140">*JsInteropClasses/ExampleJsInterop.cs*:</span><span class="sxs-lookup"><span data-stu-id="2ad6c-140">*JsInteropClasses/ExampleJsInterop.cs*:</span></span>

[!code-csharp[](./common/samples/3.x/BlazorSample/JsInteropClasses/ExampleJsInterop.cs?name=snippet1&highlight=13-15,21-23)]

<span data-ttu-id="2ad6c-141">`IJSRuntime` Abstrakcją jest asynchroniczne, aby zezwolić na potrzeby scenariuszy po stronie serwera.</span><span class="sxs-lookup"><span data-stu-id="2ad6c-141">The `IJSRuntime` abstraction is asynchronous to allow for server-side scenarios.</span></span> <span data-ttu-id="2ad6c-142">Jeśli aplikacja jest uruchamiana po stronie klienta, a użytkownik chce synchronicznie, wywołaj funkcję JavaScript takiej `IJSInProcessRuntime` i wywołać `Invoke<T>` zamiast tego.</span><span class="sxs-lookup"><span data-stu-id="2ad6c-142">If the app runs client-side and you want to invoke a JavaScript function synchronously, downcast to `IJSInProcessRuntime` and call `Invoke<T>` instead.</span></span> <span data-ttu-id="2ad6c-143">Firma Microsoft zaleca, aby większość bibliotek międzyoperacyjnego JavaScript upewnij się, że biblioteki są dostępne we wszystkich scenariuszach, po stronie klienta i po stronie serwera za pomocą async interfejsów API.</span><span class="sxs-lookup"><span data-stu-id="2ad6c-143">We recommend that most JavaScript interop libraries use the async APIs to ensure that the libraries are available in all scenarios, client-side or server-side.</span></span>

<span data-ttu-id="2ad6c-144">Przykładowa aplikacja zawiera składnik do zademonstrowania międzyoperacyjnego JavaScript.</span><span class="sxs-lookup"><span data-stu-id="2ad6c-144">The sample app includes a component to demonstrate JavaScript interop.</span></span> <span data-ttu-id="2ad6c-145">Składnik:</span><span class="sxs-lookup"><span data-stu-id="2ad6c-145">The component:</span></span>

* <span data-ttu-id="2ad6c-146">Odbiera dane wejściowe użytkownika za pośrednictwem wiersza kodu JavaScript.</span><span class="sxs-lookup"><span data-stu-id="2ad6c-146">Receives user input via a JavaScript prompt.</span></span>
* <span data-ttu-id="2ad6c-147">Zwraca tekst do składnika do przetworzenia.</span><span class="sxs-lookup"><span data-stu-id="2ad6c-147">Returns the text to the component for processing.</span></span>
* <span data-ttu-id="2ad6c-148">Wywołania drugiej funkcji języka JavaScript, która współdziała z modelu DOM w celu wyświetlania komunikatu powitalnego.</span><span class="sxs-lookup"><span data-stu-id="2ad6c-148">Calls a second JavaScript function that interacts with the DOM to display a welcome message.</span></span>

<span data-ttu-id="2ad6c-149">*Pages/JSInterop.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="2ad6c-149">*Pages/JSInterop.cshtml*:</span></span>

[!code-cshtml[](./common/samples/3.x/BlazorSample/Pages/JsInterop.cshtml?start=1&end=21)]

1. <span data-ttu-id="2ad6c-150">Podczas `TriggerJsPrompt` jest wykonywana przez wybranie elementu **wyzwalacza JavaScript monitu** przycisku `ExampleJsInterop.Prompt` method in Class metoda C# kodu jest wywoływana.</span><span class="sxs-lookup"><span data-stu-id="2ad6c-150">When `TriggerJsPrompt` is executed by selecting the component's **Trigger JavaScript Prompt** button, the `ExampleJsInterop.Prompt` method in C# code is called.</span></span>
1. <span data-ttu-id="2ad6c-151">`Prompt` Metoda wykonuje kod JavaScript `showPrompt` funkcji podawany *wwwroot/exampleJsInterop.js* pliku.</span><span class="sxs-lookup"><span data-stu-id="2ad6c-151">The `Prompt` method executes the JavaScript `showPrompt` function provided in the *wwwroot/exampleJsInterop.js* file.</span></span>
1. <span data-ttu-id="2ad6c-152">`showPrompt` Funkcja akceptuje dane wejściowe użytkownika (nazwa użytkownika), który jest kodowany w formacie HTML i zwrócone do `Prompt` metody i ostatecznie z powrotem do składnika.</span><span class="sxs-lookup"><span data-stu-id="2ad6c-152">The `showPrompt` function accepts user input (the user's name), which is HTML-encoded and returned to the `Prompt` method and ultimately back to the component.</span></span> <span data-ttu-id="2ad6c-153">Składnik nazwy użytkownika są przechowywane w zmiennej lokalnej `name`.</span><span class="sxs-lookup"><span data-stu-id="2ad6c-153">The component stores the user's name in a local variable, `name`.</span></span>
1. <span data-ttu-id="2ad6c-154">Ten ciąg jest przechowywany w `name` jest włączona do komunikat powitalny, który jest przekazywany do drugiej C# metody `ExampleJsInterop.Display`.</span><span class="sxs-lookup"><span data-stu-id="2ad6c-154">The string stored in `name` is incorporated into a welcome message, which is passed to a second C# method, `ExampleJsInterop.Display`.</span></span>
1. <span data-ttu-id="2ad6c-155">`Display` wywołuje funkcję JavaScript `displayWelcome`, która renderuje wiadomość powitalna w tagu nagłówka.</span><span class="sxs-lookup"><span data-stu-id="2ad6c-155">`Display` calls a JavaScript function, `displayWelcome`, which renders the welcome message into a heading tag.</span></span>

## <a name="capture-references-to-elements"></a><span data-ttu-id="2ad6c-156">Przechwytywanie odwołania do elementów</span><span class="sxs-lookup"><span data-stu-id="2ad6c-156">Capture references to elements</span></span>

<span data-ttu-id="2ad6c-157">Niektóre [międzyoperacyjnego JavaScript](xref:razor-components/javascript-interop) scenariusze wymagają odwołania do elementów HTML.</span><span class="sxs-lookup"><span data-stu-id="2ad6c-157">Some [JavaScript interop](xref:razor-components/javascript-interop) scenarios require references to HTML elements.</span></span> <span data-ttu-id="2ad6c-158">Na przykład biblioteka interfejsów użytkownika może wymagać odwołanie do elementu inicjowania lub może być konieczne wywołania interfejsów API jak polecenie elementu, takie jak `focus` lub `play`.</span><span class="sxs-lookup"><span data-stu-id="2ad6c-158">For example, a UI library may require an element reference for initialization, or you might need to call command-like APIs on an element, such as `focus` or `play`.</span></span>

<span data-ttu-id="2ad6c-159">Odwołania do elementów HTML w składniku można przechwycić poprzez dodanie `ref` atrybutu do elementu HTML, a następnie zdefiniowanie pola typu `ElementRef` którego nazwa odpowiada wartości `ref` atrybutu.</span><span class="sxs-lookup"><span data-stu-id="2ad6c-159">You can capture references to HTML elements in a component by adding a `ref` attribute to the HTML element and then defining a field of type `ElementRef` whose name matches the value of the `ref` attribute.</span></span>

<span data-ttu-id="2ad6c-160">Poniższy przykład pokazuje, przechwytywanie odwołanie do elementu wprowadzania nazwy użytkownika:</span><span class="sxs-lookup"><span data-stu-id="2ad6c-160">The following example shows capturing a reference to the username input element:</span></span>

```cshtml
<input ref="username" ... />

@functions {
    ElementRef username;
}
```

> [!NOTE]
> <span data-ttu-id="2ad6c-161">Czy **nie** używać odwołań do elementu przechwyconych jako sposobu wypełniania DOM.</span><span class="sxs-lookup"><span data-stu-id="2ad6c-161">Do **not** use captured element references as a way of populating the DOM.</span></span> <span data-ttu-id="2ad6c-162">Ten sposób może kolidować z modelu deklaratywne renderowania.</span><span class="sxs-lookup"><span data-stu-id="2ad6c-162">Doing so may interfere with the declarative rendering model.</span></span>

<span data-ttu-id="2ad6c-163">Jeśli chodzi o kodu platformy .NET, `ElementRef` jest nieprzezroczysta dojście.</span><span class="sxs-lookup"><span data-stu-id="2ad6c-163">As far as .NET code is concerned, an `ElementRef` is an opaque handle.</span></span> <span data-ttu-id="2ad6c-164">*Tylko* rzeczy można zrobić za pomocą `ElementRef` jest przekazywany do kodu w języku JavaScript za pomocą międzyoperacyjności języka JavaScript.</span><span class="sxs-lookup"><span data-stu-id="2ad6c-164">The *only* thing you can do with `ElementRef` is pass it through to JavaScript code via JavaScript interop.</span></span> <span data-ttu-id="2ad6c-165">Jeśli tak zrobisz, otrzyma kod JavaScript po stronie `HTMLElement` wystąpienia, którego można użyć z normalnym DOM interfejsów API.</span><span class="sxs-lookup"><span data-stu-id="2ad6c-165">When you do so, the JavaScript-side code receives an `HTMLElement` instance, which it can use with normal DOM APIs.</span></span>

<span data-ttu-id="2ad6c-166">Na przykład poniższy kod definiuje metody rozszerzenia platformy .NET, która umożliwia ustawienie fokusu w elemencie:</span><span class="sxs-lookup"><span data-stu-id="2ad6c-166">For example, the following code defines a .NET extension method that enables setting the focus on an element:</span></span>

<span data-ttu-id="2ad6c-167">*mylib.js*:</span><span class="sxs-lookup"><span data-stu-id="2ad6c-167">*mylib.js*:</span></span>

```javascript
window.myLib = {
  focusElement : function (element) {
    element.focus();
  }
}
```

<span data-ttu-id="2ad6c-168">*ElementRefExtensions.cs*:</span><span class="sxs-lookup"><span data-stu-id="2ad6c-168">*ElementRefExtensions.cs*:</span></span>

```csharp
using Microsoft.AspNetCore.Components;
using Microsoft.JSInterop;
using System.Threading.Tasks;

namespace MyLib
{
    public static class MyLibElementRefExtensions
    {
        private readonly IJSRuntime _jsRuntime;

        public MyJsInterop(IJSRuntime jsRuntime)
        {
            _jsRuntime = jsRuntime;
        }

        public static Task Focus(this ElementRef elementRef)
        {
            return _jsRuntime.InvokeAsync<object>(
                "myLib.focusElement", elementRef);
        }
    }
}
```

<span data-ttu-id="2ad6c-169">Użyj `MyLib` i wywołać `Focus` na `ElementRef` na dane wejściowe fokus, dowolny składnik:</span><span class="sxs-lookup"><span data-stu-id="2ad6c-169">Use `MyLib` and call `Focus` on an `ElementRef` to focus inputs in any component:</span></span>

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
> <span data-ttu-id="2ad6c-170">`username` Zmiennej tylko jest wypełniana po renderuje składnika i jego dane wyjściowe obejmują `<input>` elementu.</span><span class="sxs-lookup"><span data-stu-id="2ad6c-170">The `username` variable is only populated after the component renders and its output includes the `<input>` element.</span></span> <span data-ttu-id="2ad6c-171">Jeśli zostanie podjęta próba przekazania unpopulated `ElementRef` do kodu JavaScript, otrzyma kod JavaScript `null`.</span><span class="sxs-lookup"><span data-stu-id="2ad6c-171">If you try to pass an unpopulated `ElementRef` to JavaScript code, the JavaScript code receives `null`.</span></span> <span data-ttu-id="2ad6c-172">Do manipulowania odwołania do elementu, po zakończeniu renderowania (w celu ustawienie początkowy fokus w elemencie) Użyj składnika `OnAfterRenderAsync` lub `OnAfterRender` [metody cyklu życia składników](xref:razor-components/components#lifecycle-methods).</span><span class="sxs-lookup"><span data-stu-id="2ad6c-172">To manipulate element references after the component has finished rendering (to set the initial focus on an element) use the `OnAfterRenderAsync` or `OnAfterRender` [component lifecycle methods](xref:razor-components/components#lifecycle-methods).</span></span>

## <a name="invoke-net-methods-from-javascript-functions"></a><span data-ttu-id="2ad6c-173">Wywoływanie metod .NET z funkcji języka JavaScript</span><span class="sxs-lookup"><span data-stu-id="2ad6c-173">Invoke .NET methods from JavaScript functions</span></span>

### <a name="static-net-method-call"></a><span data-ttu-id="2ad6c-174">Statyczne wywołanie metody .NET</span><span class="sxs-lookup"><span data-stu-id="2ad6c-174">Static .NET method call</span></span>

<span data-ttu-id="2ad6c-175">Aby wywołać metodę statyczną .NET poziomu języka JavaScript, należy użyć `DotNet.invokeMethod` lub `DotNet.invokeMethodAsync` funkcji.</span><span class="sxs-lookup"><span data-stu-id="2ad6c-175">To invoke a static .NET method from JavaScript, use the `DotNet.invokeMethod` or `DotNet.invokeMethodAsync` functions.</span></span> <span data-ttu-id="2ad6c-176">Przekaż identyfikator statyczna metoda, którą chcesz wywołać, nazwa zestawu zawierającego funkcję i żadnych argumentów.</span><span class="sxs-lookup"><span data-stu-id="2ad6c-176">Pass in the identifier of the static method you wish to call, the name of the assembly containing the function, and any arguments.</span></span> <span data-ttu-id="2ad6c-177">Wersja asynchroniczna jest preferowane w scenariuszach, po stronie serwera.</span><span class="sxs-lookup"><span data-stu-id="2ad6c-177">The asynchronous version is preferred to support server-side scenarios.</span></span> <span data-ttu-id="2ad6c-178">Jako element wywoływalny poziomu języka JavaScript, metoda .NET musi być publiczne, statycznych i dekorowane za pomocą `[JSInvokable]`.</span><span class="sxs-lookup"><span data-stu-id="2ad6c-178">To be invokable from JavaScript, the .NET method must be public, static, and decorated with `[JSInvokable]`.</span></span> <span data-ttu-id="2ad6c-179">Domyślnie identyfikator metody jest nazwa metody, ale można określić przy użyciu innego identyfikatora `JSInvokableAttribute` konstruktora.</span><span class="sxs-lookup"><span data-stu-id="2ad6c-179">By default, the method identifier is the method name, but you can specify a different identifier using the `JSInvokableAttribute` constructor.</span></span> <span data-ttu-id="2ad6c-180">Wywoływanie metod ogólnych open nie jest obecnie obsługiwane.</span><span class="sxs-lookup"><span data-stu-id="2ad6c-180">Calling open generic methods isn't currently supported.</span></span>

<span data-ttu-id="2ad6c-181">Przykładowa aplikacja zawiera C# metodę, aby zwrócić tablicę `int`s.</span><span class="sxs-lookup"><span data-stu-id="2ad6c-181">The sample app includes a C# method to return an array of `int`s.</span></span> <span data-ttu-id="2ad6c-182">Metoda zostanie nadany `JSInvokable` atrybutu.</span><span class="sxs-lookup"><span data-stu-id="2ad6c-182">The method is decorated with the `JSInvokable` attribute.</span></span>

<span data-ttu-id="2ad6c-183">*Pages/JsInterop.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="2ad6c-183">*Pages/JsInterop.cshtml*:</span></span>

[!code-cshtml[](./common/samples/3.x/BlazorSample/Pages/JsInterop.cshtml?start=48&end=59&highlight=7-11)]

<span data-ttu-id="2ad6c-184">JavaScript obsłużonych klient wywołuje C# metoda .NET.</span><span class="sxs-lookup"><span data-stu-id="2ad6c-184">JavaScript served to the client invokes the C# .NET method.</span></span>

<span data-ttu-id="2ad6c-185">*wwwroot/exampleJsInterop.js*:</span><span class="sxs-lookup"><span data-stu-id="2ad6c-185">*wwwroot/exampleJsInterop.js*:</span></span>

[!code-javascript[](./common/samples/3.x/BlazorSample/wwwroot/exampleJsInterop.js?highlight=8-12)]

<span data-ttu-id="2ad6c-186">Gdy **.NET wyzwalacza statycznej metody ReturnArrayAsync** przycisk jest zaznaczony, sprawdź dane wyjściowe konsoli narzędzi dla deweloperów sieci web w przeglądarce.</span><span class="sxs-lookup"><span data-stu-id="2ad6c-186">When the **Trigger .NET static method ReturnArrayAsync** button is selected, examine the console output in the browser's web developer tools.</span></span>

<span data-ttu-id="2ad6c-187">Dane wyjściowe konsoli będą:</span><span class="sxs-lookup"><span data-stu-id="2ad6c-187">The console output is:</span></span>

```console
Array(4) [ 1, 2, 3, 4 ]
```

<span data-ttu-id="2ad6c-188">Czwarta wartość tablicy są wypychane do tablicy (`data.push(4);`) zwróciło `ReturnArrayAsync`.</span><span class="sxs-lookup"><span data-stu-id="2ad6c-188">The fourth array value is pushed to the array (`data.push(4);`) returned by `ReturnArrayAsync`.</span></span>

### <a name="instance-method-call"></a><span data-ttu-id="2ad6c-189">Wywołanie metody wystąpienia</span><span class="sxs-lookup"><span data-stu-id="2ad6c-189">Instance method call</span></span>

<span data-ttu-id="2ad6c-190">Można również wywołać metody wystąpienia .NET poziomu języka JavaScript.</span><span class="sxs-lookup"><span data-stu-id="2ad6c-190">You can also call .NET instance methods from JavaScript.</span></span> <span data-ttu-id="2ad6c-191">Aby wywołać metodę wystąpienia .NET poziomu języka JavaScript, najpierw przejść wystąpienia platformy .NET w kodzie JavaScript opakowując go w `DotNetObjectRef` wystąpienia.</span><span class="sxs-lookup"><span data-stu-id="2ad6c-191">To invoke a .NET instance method from JavaScript, first pass the .NET instance to JavaScript by wrapping it in a `DotNetObjectRef` instance.</span></span> <span data-ttu-id="2ad6c-192">Wystąpienie platformy .NET jest przekazywany przez odwołanie w kodzie JavaScript i można wywołać metody wystąpienia platformy .NET przy użyciu wystąpienia `invokeMethod` lub `invokeMethodAsync` funkcji.</span><span class="sxs-lookup"><span data-stu-id="2ad6c-192">The .NET instance is passed by reference to JavaScript, and you can invoke .NET instance methods on the instance using the `invokeMethod` or `invokeMethodAsync` functions.</span></span> <span data-ttu-id="2ad6c-193">Wystąpienie programu .NET może być przekazywany jako argument, podczas wywoływania innych metod .NET poziomu języka JavaScript.</span><span class="sxs-lookup"><span data-stu-id="2ad6c-193">The .NET instance can also be passed as an argument when invoking other .NET methods from JavaScript.</span></span>

> [!NOTE]
> <span data-ttu-id="2ad6c-194">Przykładowa aplikacja rejestruje komunikaty w konsoli po stronie klienta.</span><span class="sxs-lookup"><span data-stu-id="2ad6c-194">The sample app logs messages to the client-side console.</span></span> <span data-ttu-id="2ad6c-195">W poniższych przykładach pokazano przez aplikację przykładową Sprawdź dane wyjściowe konsoli w przeglądarce w narzędzia dla deweloperów w przeglądarce.</span><span class="sxs-lookup"><span data-stu-id="2ad6c-195">For the following examples demonstrated by the sample app, examine the browser's console output in the browser's developer tools.</span></span>

<span data-ttu-id="2ad6c-196">Gdy **metodę wystąpienia .NET wyzwalacza HelloHelper.SayHello** przycisk jest zaznaczony, `ExampleJsInterop.CallHelloHelperSayHello` nosi nazwę i przekazuje nazwę `Blazor`, do metody.</span><span class="sxs-lookup"><span data-stu-id="2ad6c-196">When the **Trigger .NET instance method HelloHelper.SayHello** button is selected, `ExampleJsInterop.CallHelloHelperSayHello` is called and passes a name, `Blazor`, to the method.</span></span>

<span data-ttu-id="2ad6c-197">*Pages/JsInterop.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="2ad6c-197">*Pages/JsInterop.cshtml*:</span></span>

[!code-cshtml[](./common/samples/3.x/BlazorSample/Pages/JsInterop.cshtml?start=61&end=71&highlight=8-9)]

<span data-ttu-id="2ad6c-198">`CallHelloHelperSayHello` wywołuje funkcję JavaScript `sayHello` o nowe wystąpienie klasy `HelloHelper`.</span><span class="sxs-lookup"><span data-stu-id="2ad6c-198">`CallHelloHelperSayHello` invokes the JavaScript function `sayHello` with a new instance of `HelloHelper`.</span></span>

<span data-ttu-id="2ad6c-199">*JsInteropClasses/ExampleJsInterop.cs*:</span><span class="sxs-lookup"><span data-stu-id="2ad6c-199">*JsInteropClasses/ExampleJsInterop.cs*:</span></span>

[!code-csharp[](./common/samples/3.x/BlazorSample/JsInteropClasses/ExampleJsInterop.cs?name=snippet1&highlight=26-32)]

<span data-ttu-id="2ad6c-200">*wwwroot/exampleJsInterop.js*:</span><span class="sxs-lookup"><span data-stu-id="2ad6c-200">*wwwroot/exampleJsInterop.js*:</span></span>

[!code-javascript[](./common/samples/3.x/BlazorSample/wwwroot/exampleJsInterop.js?highlight=15-17)]

<span data-ttu-id="2ad6c-201">Nazwa jest przekazywany do `HelloHelper`przez konstruktora, który ustawia `HelloHelper.Name` właściwości.</span><span class="sxs-lookup"><span data-stu-id="2ad6c-201">The name is passed to `HelloHelper`'s constructor, which sets the `HelloHelper.Name` property.</span></span> <span data-ttu-id="2ad6c-202">Gdy funkcja języka JavaScript `sayHello` jest wykonywane, `HelloHelper.SayHello` zwraca `Hello, {Name}!` komunikat jest wyświetlony w konsoli przez funkcję języka JavaScript.</span><span class="sxs-lookup"><span data-stu-id="2ad6c-202">When the JavaScript function `sayHello` is executed, `HelloHelper.SayHello` returns the `Hello, {Name}!` message, which is written to the console by the JavaScript function.</span></span>

<span data-ttu-id="2ad6c-203">*JsInteropClasses/HelloHelper.cs*:</span><span class="sxs-lookup"><span data-stu-id="2ad6c-203">*JsInteropClasses/HelloHelper.cs*:</span></span>

[!code-csharp[](./common/samples/3.x/BlazorSample/JsInteropClasses/HelloHelper.cs?name=snippet1&highlight=5,10-11)]

<span data-ttu-id="2ad6c-204">Konsola danych wyjściowych w narzędzia dla deweloperów sieci web w przeglądarce:</span><span class="sxs-lookup"><span data-stu-id="2ad6c-204">Console output in the browser's web developer tools:</span></span>

```console
Hello, Blazor!
```

## <a name="share-interop-code-in-a-razor-component-class-library"></a><span data-ttu-id="2ad6c-205">Kód Interop SE udziału w bibliotece klas składników Razor</span><span class="sxs-lookup"><span data-stu-id="2ad6c-205">Share interop code in a Razor Component class library</span></span>

<span data-ttu-id="2ad6c-206">Biblioteki klas Razor składnika mogą być dołączane międzyoperacyjnego kodu JavaScript (`dotnet new razorclasslib`), co pozwala na udostępnianie kodu w pakiecie NuGet.</span><span class="sxs-lookup"><span data-stu-id="2ad6c-206">JavaScript interop code can be included in a Razor Component class library (`dotnet new razorclasslib`), which allows you to share the code in a NuGet package.</span></span>

<span data-ttu-id="2ad6c-207">Biblioteki klas Razor składnik obsługuje osadzanie zasobów JavaScript skompilowany zestaw.</span><span class="sxs-lookup"><span data-stu-id="2ad6c-207">The Razor Component class library handles embedding JavaScript resources in the built assembly.</span></span> <span data-ttu-id="2ad6c-208">Pliki JavaScript są umieszczane w *wwwroot* folderu.</span><span class="sxs-lookup"><span data-stu-id="2ad6c-208">The JavaScript files are placed in the *wwwroot* folder.</span></span> <span data-ttu-id="2ad6c-209">Osadzanie zasobów podczas kompilowania biblioteki zajmuje się narzędzi.</span><span class="sxs-lookup"><span data-stu-id="2ad6c-209">The tooling takes care of embedding the resources when the library is built.</span></span>

<span data-ttu-id="2ad6c-210">Skompilowany pakiet NuGet odwołuje się do pliku projektu aplikacji, tak samo, jak odwołuje się do dowolnego normalnego pakietu NuGet.</span><span class="sxs-lookup"><span data-stu-id="2ad6c-210">The built NuGet package is referenced in the project file of the app just as any normal NuGet package is referenced.</span></span> <span data-ttu-id="2ad6c-211">Po przywróceniu aplikacji kod aplikacji może wywołać na język JavaScript, tak jakby C#.</span><span class="sxs-lookup"><span data-stu-id="2ad6c-211">After the app is restored, app code can call into JavaScript as if it were C#.</span></span>

<span data-ttu-id="2ad6c-212">Aby uzyskać więcej informacji, zobacz <xref:razor-components/class-libraries>.</span><span class="sxs-lookup"><span data-stu-id="2ad6c-212">For more information, see <xref:razor-components/class-libraries>.</span></span>
