---
title: Razor Components JavaScript interop
author: guardrex
description: Dowiedz się, jak wywoływać funkcje języka JavaScript z .NET i .NET metody z kodu JavaScript w aplikacjach Blazor i składniki Razor.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 03/26/2019
uid: razor-components/javascript-interop
ms.openlocfilehash: c45c04d849ba4b3b017a65e79aa758effd5ba8eb
ms.sourcegitcommit: 6bde1fdf686326c080a7518a6725e56e56d8886e
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/08/2019
ms.locfileid: "59068120"
---
# <a name="razor-components-javascript-interop"></a>Razor Components JavaScript interop

Przez [Javier Calvarro Nelson](https://github.com/javiercn), [Daniel Roth](https://github.com/danroth27), i [Luke Latham](https://github.com/guardrex)

Aplikacja składniki Razor można wywołać funkcji języka JavaScript z .NET i .NET metody z kodu JavaScript.

## <a name="invoke-javascript-functions-from-net-methods"></a>Wywoływanie funkcji języka JavaScript z metod .NET

Istnieją terminy, gdy jest wymagane do wywołania funkcji JavaScript kodu platformy .NET. Na przykład wywołanie JavaScript mogą uwidocznić możliwości przeglądarki lub funkcji z biblioteki JavaScript do aplikacji.

Aby wywołać na język JavaScript z platformy .NET, należy użyć `IJSRuntime` abstrakcji. `InvokeAsync<T>` Metoda pobiera identyfikator dla funkcji języka JavaScript, którą chcesz wywołać wraz z dowolnej liczby argumentów do serializacji JSON. Identyfikator funkcji jest określana względem zakresu globalnego (`window`). Jeśli chcesz wywołać `window.someScope.someFunction`, identyfikator ma `someScope.someFunction`. Nie ma potrzeby, aby zarejestrować funkcja jest wywoływana. Zwracany typ `T` musi być również JSON do serializacji.

W przypadku aplikacji ASP.NET Core Razor składniki po stronie serwera:

* Wiele żądań użytkownika są przetwarzane przez aplikację po stronie serwera. Nie wywołuj `JSRuntime.Current` w składniku do wywoływania funkcji języka JavaScript.
* Wstrzykiwanie `IJSRuntime` pozyskiwania i użycia wprowadzonego obiektu do wystawiania wywołań międzyoperacyjnych w języku JavaScript.

Poniższy przykład jest oparty na [TextDecoder](https://developer.mozilla.org/docs/Web/API/TextDecoder), eksperymentalne dekodera oparte na języku JavaScript. W przykładzie pokazano, jak wywołać funkcję JavaScript z C# metody. Funkcja języka JavaScript akceptuje tablicy bajtów z C# metody dekoduje tablicy i zwraca tekst do składnika do wyświetlenia.

Wewnątrz `<head>` elementu *wwwroot/index.html*, zapewnia funkcję, która używa `TextDecoder` zdekodować tablicy przekazany:

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

Kod JavaScript, taki jak kod przedstawiony w poprzednim przykładzie, można również załadować z pliku JavaScript (*js*) za pomocą odwołania do pliku skryptu w *wwwroot/index.html* pliku:

```html
<script src="exampleJsInterop.js"></script>
```

Następujących składników:

* Wywołuje `ConvertArray` funkcję języka JavaScript za pomocą `JsRuntime` przycisku składnika (**konwersji tablicy**) jest zaznaczona.
* Po wywołaniu funkcji JavaScript przekazana tablica jest konwertowana na ciąg. Ten ciąg jest zwracany do składnika do wyświetlenia.

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

Aby użyć `IJSRuntime` abstrakcji, przyjmuje żadnego z następujących metod:

* Wstrzykiwanie `IJSRuntime` abstrakcji w pliku Razor (*.razor*, *.cshtml*):

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

* Wstrzykiwanie `IJSRuntime` abstrakcji do klasy (*.cs*):

  ```csharp
  public class JsInteropClasses
  {
      private readonly IJSRuntime _jsRuntime;

      public JsInteropClasses(IJSRuntime jsRuntime)
      {
          _jsRuntime = jsRuntime;
      }

      public Task<string> TickerChanged(string data)
      {
          // The handleTickerChanged JavaScript method is implemented
          // in a JavaScript file, such as 'wwwroot/tickerJsInterop.js'.
          return _jsRuntime.InvokeAsync<object>(
              "handleTickerChanged",
              stockUpdate.symbol,
              stockUpdate.price);
      }
  }
  ```

* Dla dynamicznego generowania zawartości za pomocą `BuildRenderTree`, użyj `[Inject]` atrybutu:

  ```csharp
  [Inject] IJSRuntime JSRuntime { get; set; }
  ```

W aplikacji przykładowej po stronie klienta, znajdująca się w tym temacie dwie funkcje języka JavaScript są dostępne dla aplikacji po stronie klienta, który wchodzić w interakcje z modelu DOM do odbierania danych wejściowych użytkownika i wyświetlania komunikatu powitalnego:

* `showPrompt` &ndash; Wyświetla monit o podanie, aby akceptować dane wejściowe użytkownika (nazwa użytkownika) i zwraca nazwę do obiektu wywołującego.
* `displayWelcome` &ndash; Przypisuje komunikat powitalny wywołujący do obiektu modelu DOM przy użyciu `id` z `welcome`.

*wwwroot/exampleJsInterop.js*:

[!code-javascript[](./common/samples/3.x/BlazorSample/wwwroot/exampleJsInterop.js?highlight=2-7)]

Miejsce `<script>` tag, który odwołuje się do pliku JavaScript w *wwwroot/index.html* pliku:

[!code-html[](./common/samples/3.x/BlazorSample/wwwroot/index.html?highlight=15)]

Nie należy umieszczać `<script>` tagów w pliku składnika, ponieważ `<script>` tagów nie można zaktualizować dynamicznie.

.NET współdziałanie metod za pomocą języka JavaScript działa w *exampleJsInterop.js* pliku przez wywołanie metody `IJSRuntime.InvokeAsync<T>`.

`IJSRuntime` Abstrakcją jest asynchroniczne, aby zezwolić na potrzeby scenariuszy po stronie serwera. Jeśli aplikacja jest uruchamiana po stronie klienta, a użytkownik chce synchronicznie, wywołaj funkcję JavaScript takiej `IJSInProcessRuntime` i wywołać `Invoke<T>` zamiast tego. Firma Microsoft zaleca, aby większość bibliotek międzyoperacyjnego JavaScript upewnij się, że biblioteki są dostępne we wszystkich scenariuszach, po stronie klienta i po stronie serwera za pomocą async interfejsów API.

Przykładowa aplikacja zawiera składnik do zademonstrowania międzyoperacyjnego JavaScript. Składnik:

* Odbiera dane wejściowe użytkownika za pośrednictwem wiersza kodu JavaScript.
* Zwraca tekst do składnika do przetworzenia.
* Wywołania drugiej funkcji języka JavaScript, która współdziała z modelu DOM w celu wyświetlania komunikatu powitalnego.

*Pages/JSInterop.cshtml*:

[!code-cshtml[](./common/samples/3.x/BlazorSample/Pages/JsInterop.cshtml?name=snippet_JSInterop1&highlight=3,19-21,23-25)]

1. Gdy `TriggerJsPrompt` jest wykonywana przez wybranie elementu **wyzwalacza JavaScript monitu** przycisk JavaScript `showPrompt` funkcja udostępniana w *wwwroot/exampleJsInterop.js* plik jest wywoływana.
1. `showPrompt` Funkcja akceptuje dane wejściowe użytkownika (nazwa użytkownika), który jest kodowany w formacie HTML i zwrócone do składnika. Składnik nazwy użytkownika są przechowywane w zmiennej lokalnej `name`.
1. Ten ciąg jest przechowywany w `name` jest włączona do komunikat powitalny, który jest przekazywany do funkcji języka JavaScript, `displayWelcome`, który renderuje wiadomość powitalna w tagu nagłówka.

## <a name="capture-references-to-elements"></a>Przechwytywanie odwołania do elementów

Niektóre [międzyoperacyjnego JavaScript](xref:razor-components/javascript-interop) scenariusze wymagają odwołania do elementów HTML. Na przykład biblioteka interfejsów użytkownika może wymagać odwołanie do elementu inicjowania lub może być konieczne wywołania interfejsów API jak polecenie elementu, takie jak `focus` lub `play`.

Odwołania do elementów HTML w składniku można przechwycić poprzez dodanie `ref` atrybutu do elementu HTML, a następnie zdefiniowanie pola typu `ElementRef` którego nazwa odpowiada wartości `ref` atrybutu.

Poniższy przykład pokazuje, przechwytywanie odwołanie do elementu wprowadzania nazwy użytkownika:

```cshtml
<input ref="username" ... />

@functions {
    ElementRef username;
}
```

> [!NOTE]
> Czy **nie** używać odwołań do elementu przechwyconych jako sposobu wypełniania DOM. Ten sposób może kolidować z modelu deklaratywne renderowania.

Jeśli chodzi o kodu platformy .NET, `ElementRef` jest nieprzezroczysta dojście. *Tylko* rzeczy można zrobić za pomocą `ElementRef` jest przekazywany do kodu w języku JavaScript za pomocą międzyoperacyjności języka JavaScript. Jeśli tak zrobisz, otrzyma kod JavaScript po stronie `HTMLElement` wystąpienia, którego można użyć z normalnym DOM interfejsów API.

Na przykład poniższy kod definiuje metody rozszerzenia platformy .NET, która umożliwia ustawienie fokusu w elemencie:

*exampleJsInterop.js*:

```javascript
window.exampleJsFunctions = {
  focusElement : function (element) {
    element.focus();
  }
}
```

Użyj `IJSRuntime.InvokeAsync<T>` i wywołać `exampleJsFunctions.focusElement` z `ElementRef` się elementu:

[!code-cshtml[](javascript-interop/samples_snapshot/component1.cshtml?highlight=1,3,7,11-12)]

Można użyć metody rozszerzenia do skoncentrowania się element, należy utworzyć metody statyczne rozszerzenie, który odbiera `IJSRuntime` wystąpienie:

```csharp
public static Task Focus(this ElementRef elementRef, IJSRuntime jsRuntime)
{
    return jsRuntime.InvokeAsync<object>(
        "exampleJsFunctions.focusElement", elementRef);
}
```

Metoda jest wywoływana bezpośrednio na obiekcie. W poniższym przykładzie założono, że statyczne `Focus` metoda jest dostępna z `JsInteropClasses` przestrzeni nazw:

[!code-cshtml[](javascript-interop/samples_snapshot/component2.cshtml?highlight=1,4,8,12)]

> [!IMPORTANT]
> `username` Zmiennej tylko jest wypełniana po renderuje składnika i jego dane wyjściowe obejmują `<input>` elementu. Jeśli zostanie podjęta próba przekazania unpopulated `ElementRef` do kodu JavaScript, otrzyma kod JavaScript `null`. Do manipulowania odwołania do elementu, po zakończeniu renderowania (w celu ustawienie początkowy fokus w elemencie) Użyj składnika `OnAfterRenderAsync` lub `OnAfterRender` [metody cyklu życia składników](xref:razor-components/components#lifecycle-methods).

## <a name="invoke-net-methods-from-javascript-functions"></a>Wywoływanie metod .NET z funkcji języka JavaScript

### <a name="static-net-method-call"></a>Statyczne wywołanie metody .NET

Aby wywołać metodę statyczną .NET poziomu języka JavaScript, należy użyć `DotNet.invokeMethod` lub `DotNet.invokeMethodAsync` funkcji. Przekaż identyfikator statyczna metoda, którą chcesz wywołać, nazwa zestawu zawierającego funkcję i żadnych argumentów. Wersja asynchroniczna jest preferowane w scenariuszach, po stronie serwera. Jako element wywoływalny poziomu języka JavaScript, metoda .NET musi być publiczne, statycznych i dekorowane za pomocą `[JSInvokable]`. Domyślnie identyfikator metody jest nazwa metody, ale można określić przy użyciu innego identyfikatora `JSInvokableAttribute` konstruktora. Wywoływanie metod ogólnych open nie jest obecnie obsługiwane.

Przykładowa aplikacja zawiera C# metodę, aby zwrócić tablicę `int`s. Metoda zostanie nadany `JSInvokable` atrybutu.

*Pages/JsInterop.cshtml*:

[!code-cshtml[](./common/samples/3.x/BlazorSample/Pages/JsInterop.cshtml?name=snippet_JSInterop2&highlight=7-11)]

JavaScript obsłużonych klient wywołuje C# metoda .NET.

*wwwroot/exampleJsInterop.js*:

[!code-javascript[](./common/samples/3.x/BlazorSample/wwwroot/exampleJsInterop.js?highlight=8-14)]

Gdy **.NET wyzwalacza statycznej metody ReturnArrayAsync** przycisk jest zaznaczony, sprawdź dane wyjściowe konsoli narzędzi dla deweloperów sieci web w przeglądarce.

Dane wyjściowe konsoli będą:

```console
Array(4) [ 1, 2, 3, 4 ]
```

Czwarta wartość tablicy są wypychane do tablicy (`data.push(4);`) zwróciło `ReturnArrayAsync`.

### <a name="instance-method-call"></a>Wywołanie metody wystąpienia

Można również wywołać metody wystąpienia .NET poziomu języka JavaScript. Aby wywołać metodę wystąpienia .NET poziomu języka JavaScript, najpierw przejść wystąpienia platformy .NET w kodzie JavaScript opakowując go w `DotNetObjectRef` wystąpienia. Wystąpienie platformy .NET jest przekazywany przez odwołanie w kodzie JavaScript i można wywołać metody wystąpienia platformy .NET przy użyciu wystąpienia `invokeMethod` lub `invokeMethodAsync` funkcji. Wystąpienie programu .NET może być przekazywany jako argument, podczas wywoływania innych metod .NET poziomu języka JavaScript.

> [!NOTE]
> Przykładowa aplikacja rejestruje komunikaty w konsoli po stronie klienta. W poniższych przykładach pokazano przez aplikację przykładową Sprawdź dane wyjściowe konsoli w przeglądarce w narzędzia dla deweloperów w przeglądarce.

Gdy **metodę wystąpienia .NET wyzwalacza HelloHelper.SayHello** przycisk jest zaznaczony, `ExampleJsInterop.CallHelloHelperSayHello` nosi nazwę i przekazuje nazwę `Blazor`, do metody.

*Pages/JsInterop.cshtml*:

[!code-cshtml[](./common/samples/3.x/BlazorSample/Pages/JsInterop.cshtml?name=snippet_JSInterop3&highlight=8-9)]

`CallHelloHelperSayHello` wywołuje funkcję JavaScript `sayHello` o nowe wystąpienie klasy `HelloHelper`.

*JsInteropClasses/ExampleJsInterop.cs*:

[!code-csharp[](./common/samples/3.x/BlazorSample/JsInteropClasses/ExampleJsInterop.cs?name=snippet1&highlight=10-16)]

*wwwroot/exampleJsInterop.js*:

[!code-javascript[](./common/samples/3.x/BlazorSample/wwwroot/exampleJsInterop.js?highlight=15-18)]

Nazwa jest przekazywany do `HelloHelper`przez konstruktora, który ustawia `HelloHelper.Name` właściwości. Gdy funkcja języka JavaScript `sayHello` jest wykonywane, `HelloHelper.SayHello` zwraca `Hello, {Name}!` komunikat jest wyświetlony w konsoli przez funkcję języka JavaScript.

*JsInteropClasses/HelloHelper.cs*:

[!code-csharp[](./common/samples/3.x/BlazorSample/JsInteropClasses/HelloHelper.cs?name=snippet1&highlight=5,10-11)]

Konsola danych wyjściowych w narzędzia dla deweloperów sieci web w przeglądarce:

```console
Hello, Blazor!
```

## <a name="share-interop-code-in-a-razor-component-class-library"></a>Kód Interop SE udziału w bibliotece klas składników Razor

Biblioteki klas Razor składnika mogą być dołączane międzyoperacyjnego kodu JavaScript (`dotnet new razorclasslib`), co pozwala na udostępnianie kodu w pakiecie NuGet.

Biblioteki klas Razor składnik obsługuje osadzanie zasobów JavaScript skompilowany zestaw. Pliki JavaScript są umieszczane w *wwwroot* folderu. Osadzanie zasobów podczas kompilowania biblioteki zajmuje się narzędzi.

Skompilowany pakiet NuGet odwołuje się do pliku projektu aplikacji, tak samo, jak odwołuje się do dowolnego normalnego pakietu NuGet. Po przywróceniu aplikacji kod aplikacji może wywołać na język JavaScript, tak jakby C#.

Aby uzyskać więcej informacji, zobacz <xref:razor-components/class-libraries>.
