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
# <a name="razor-components-javascript-interop"></a>Razor Components JavaScript interop

Przez [Javier Calvarro Nelson](https://github.com/javiercn), [Daniel Roth](https://github.com/danroth27), i [Luke Latham](https://github.com/guardrex)

Aplikacja składniki Razor można wywołać funkcji języka JavaScript z .NET i .NET metody z kodu JavaScript.

## <a name="invoke-javascript-functions-from-net-methods"></a>Wywoływanie funkcji języka JavaScript z metod .NET

Istnieją terminy, gdy jest wymagane do wywołania funkcji JavaScript kodu platformy .NET. Na przykład wywołanie JavaScript mogą uwidocznić możliwości przeglądarki lub funkcji z biblioteki JavaScript do aplikacji.

Aby wywołać na język JavaScript z platformy .NET, należy użyć `IJSRuntime` abstrakcji. `InvokeAsync<T>` Metody `IJSRuntime` przyjmuje identyfikator dla funkcji języka JavaScript do wywołania wraz z dowolnej liczby argumentów do serializacji JSON. Identyfikator funkcji jest określana względem zakresu globalnego (`window`). Jeśli chcesz wywołać `window.someScope.someFunction`, identyfikator ma `someScope.someFunction`. Nie ma potrzeby, aby zarejestrować funkcja jest wywoływana. Zwracany typ `T` musi być również JSON do serializacji.

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

Kod JavaScript, taki jak kod przedstawiony w poprzednim przykładzie, można również załadować plik JavaScript z odwołaniem do pliku skryptu w *wwwroot/index.html* pliku.

Następujących składników:

* Wywołuje `ConvertArray` funkcję języka JavaScript za pomocą `JsRuntime` przycisku składnika (**konwersji tablicy**) jest zaznaczona.
* Po wywołaniu funkcji JavaScript przekazana tablica jest konwertowana na ciąg. Ten ciąg jest zwracany do składnika do wyświetlenia.

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

W przypadku aplikacji po stronie klienta Blazor `IJSRuntime` abstrakcją jest dostępny z `JSRuntime.Current`, która odnosi się do bieżącego użytkownika żądania. Ponieważ istnieje tylko jeden użytkownik aplikacji Blazor po stronie klienta, za pomocą `JSRuntime.Current` do wywołania języka JavaScript działa normalnie. Używaj tylko `JSRuntime.Current` w aplikacjach Blazor po stronie klienta.

W aplikacji przykładowej po stronie klienta, znajdująca się w tym temacie dwie funkcje języka JavaScript są dostępne dla aplikacji po stronie klienta, który wchodzić w interakcje z modelu DOM do odbierania danych wejściowych użytkownika i wyświetlania komunikatu powitalnego:

* `showPrompt` &ndash; Wyświetla monit o podanie, aby akceptować dane wejściowe użytkownika (nazwa użytkownika) i zwraca nazwę do obiektu wywołującego.
* `displayWelcome` &ndash; Przypisuje komunikat powitalny wywołujący do obiektu modelu DOM przy użyciu `id` z `welcome`.

*wwwroot/exampleJsInterop.js*:

[!code-javascript[](./common/samples/3.x/BlazorSample/wwwroot/exampleJsInterop.js?highlight=2-7)]

Miejsce `<script>` tag, który odwołuje się do pliku JavaScript w *wwwroot/index.html* pliku:

[!code-html[](./common/samples/3.x/BlazorSample/wwwroot/index.html?highlight=16)]

Nie umieszczaj tag skryptu w pliku składnika, ponieważ tag skryptu nie można zaktualizować dynamicznie.

.NET współdziałanie metod z funkcji języka JavaScript, przez wywołanie metody `InvokeAsync<T>` metody `IJSRuntime`.

Przykładowa aplikacja korzysta z parą C# metod `Prompt` i `Display`, aby wywołać `showPrompt` i `displayWelcome` funkcje języka JavaScript:

*JsInteropClasses/ExampleJsInterop.cs*:

[!code-csharp[](./common/samples/3.x/BlazorSample/JsInteropClasses/ExampleJsInterop.cs?name=snippet1&highlight=6-8,14-16)]

`IJSRuntime` Abstrakcją jest asynchroniczne, aby zezwolić na potrzeby scenariuszy po stronie serwera. Jeśli aplikacja jest uruchamiana po stronie klienta, a użytkownik chce synchronicznie, wywołaj funkcję JavaScript takiej `IJSInProcessRuntime` i wywołać `Invoke<T>` zamiast tego. Firma Microsoft zaleca, większość JavaScript międzyoperacyjny bibliotek korzystające z asynchroniczne interfejsy API w celu zapewnienia biblioteki są dostępne we wszystkich scenariuszach, po stronie klienta i po stronie serwera.

Przykładowa aplikacja zawiera składnik do zademonstrowania JS interop. Składnik:

* Odbiera dane wejściowe użytkownika za pośrednictwem wiersza JS.
* Zwraca tekst do składnika do przetworzenia.
* Wywołania drugiej funkcji języka JS, która współdziała z modelu DOM w celu wyświetlania komunikatu powitalnego.

*Pages/JSInterop.cshtml*:

[!code-cshtml[](./common/samples/3.x/BlazorSample/Pages/JsInterop.cshtml?start=1&end=21&highlight=2-3,9-11,13,16-20)]

1. Podczas `TriggerJsPrompt` jest wykonywana przez wybranie elementu **wyzwalacza JavaScript monitu** przycisku `ExampleJsInterop.Prompt` method in Class metoda C# kodu jest wywoływana.
1. `Prompt` Metoda wykonuje kod JavaScript `showPrompt` funkcji podawany *wwwroot/exampleJsInterop.js* pliku.
1. `showPrompt` Funkcja akceptuje dane wejściowe użytkownika (nazwa użytkownika), który jest kodowany w formacie HTML i zwrócone do `Prompt` metody i ostatecznie z powrotem do składnika. Składnik nazwy użytkownika są przechowywane w zmiennej lokalnej `name`.
1. Ten ciąg jest przechowywany w `name` jest włączona do komunikat powitalny, który jest przekazywany do drugiej C# metody `ExampleJsInterop.Display`.
1. `Display` wywołuje funkcję JavaScript `displayWelcome`, która renderuje wiadomość powitalna w tagu nagłówka.

## <a name="capture-references-to-elements"></a>Przechwytywanie odwołania do elementów

Niektóre [międzyoperacyjnego JavaScript](xref:razor-components/javascript-interop) scenariusze wymagają odwołania do elementów HTML. Na przykład biblioteka interfejsów użytkownika może wymagać odwołanie do elementu inicjowania lub może być konieczne wywołania interfejsów API jak polecenie elementu, takie jak `focus` lub `play`.

Odwołania do elementów HTML w składniku można przechwycić poprzez dodanie `ref` atrybutu do elementu HTML, a następnie zdefiniowanie pola typu `ElementRef` którego nazwa odpowiada wartości `ref` atrybutu.

Poniższy przykład pokazuje, przechwytywanie odwołanie do elementu wprowadzania nazwy użytkownika:

```csharp
<input ref="username" ... />

@functions {
    ElementRef username;
}
```

> [!NOTE]
> Czy **nie** używać odwołań do elementu przechwyconych jako sposobu wypełniania DOM. Ten sposób może kolidować z modelu deklaratywne renderowania.

Jeśli chodzi o kodu platformy .NET, `ElementRef` jest nieprzezroczysta dojście. *Tylko* może z nim zrobić jest przekazywany do kodu w języku JavaScript za pomocą międzyoperacyjności języka JavaScript. Jeśli tak zrobisz, otrzyma kod JavaScript po stronie `HTMLElement` wystąpienia, którego można użyć z normalnym DOM interfejsów API.

Na przykład poniższy kod definiuje metody rozszerzenia platformy .NET, która umożliwia ustawienie fokusu w elemencie:

*mylib.js*:

```javascript
window.myLib = {
  focusElement : function (element) {
    element.focus();
  }
}
```

*ElementRefExtensions.cs*:

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

Teraz Ty możesz skoncentrować się dane wejściowe w żadnym składniki:

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
> `username` Zmiennej tylko jest wypełniana po renderuje składnika i jego dane wyjściowe obejmują `<input>` elementu. Jeśli zostanie podjęta próba przekazania unpopulated `ElementRef` do kodu JavaScript, otrzyma kod JavaScript `null`. Do manipulowania odwołania do elementu, po zakończeniu renderowania (w celu ustawienie początkowy fokus w elemencie) Użyj składnika `OnAfterRenderAsync` lub `OnAfterRender` [metody cyklu życia składników](xref:razor-components/components#lifecycle-methods).

## <a name="invoke-net-methods-from-javascript-functions"></a>Wywoływanie metod .NET z funkcji języka JavaScript

### <a name="static-net-method-call"></a>Statyczne wywołanie metody .NET

Aby wywołać metodę statyczną .NET poziomu języka JavaScript, należy użyć `DotNet.invokeMethod` lub `DotNet.invokeMethodAsync` funkcji. Przekaż identyfikator statyczna metoda, którą chcesz wywołać, nazwa zestawu zawierającego funkcję i żadnych argumentów. Ponownie wersji asynchronicznej jest preferowane w scenariuszach, po stronie serwera. Jako element wywoływalny poziomu języka JavaScript, metoda .NET musi być publiczne, statycznych i dekorowane za pomocą `[JSInvokable]`. Domyślnie identyfikator metody jest nazwa metody, ale można określić przy użyciu innego identyfikatora `JSInvokableAttribute` konstruktora. Wywoływanie metod ogólnych open nie jest obecnie obsługiwane.

Przykładowa aplikacja zawiera C# metodę, aby zwrócić tablicę `int`s. Metoda zostanie nadany `JSInvokable` atrybutu.

*Pages/JsInterop.cshtml*:

[!code-cshtml[](./common/samples/3.x/BlazorSample/Pages/JsInterop.cshtml?start=47&end=58&highlight=7-11)]

JavaScript obsłużonych klient wywołuje C# metoda .NET.

*wwwroot/exampleJsInterop.js*:

[!code-javascript[](./common/samples/3.x/BlazorSample/wwwroot/exampleJsInterop.js?highlight=8-12)]

Gdy **.NET wyzwalacza statycznej metody ReturnArrayAsync** przycisk jest zaznaczony, sprawdź dane wyjściowe konsoli narzędzi dla deweloperów sieci web w przeglądarce:

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

[!code-cshtml[](./common/samples/3.x/BlazorSample/Pages/JsInterop.cshtml?start=60&end=69&highlight=8)]

`CallHelloHelperSayHello` wywołuje funkcję JavaScript `sayHello` o nowe wystąpienie klasy `HelloHelper`.

*JsInteropClasses/ExampleJsInterop.cs*:

[!code-csharp[](./common/samples/3.x/BlazorSample/JsInteropClasses/ExampleJsInterop.cs?name=snippet1&highlight=19-25)]

*wwwroot/exampleJsInterop.js*:

[!code-javascript[](./common/samples/3.x/BlazorSample/wwwroot/exampleJsInterop.js?highlight=15-17)]

Nazwa jest przekazywany do `HelloHelper`przez konstruktora, który ustawia `HelloHelper.Name` właściwości. Gdy funkcja języka JavaScript `sayHello` jest wykonywane, `HelloHelper.SayHello` zwraca `Hello, {Name}!` komunikat jest wyświetlony w konsoli przez funkcję języka JavaScript.

*JsInteropClasses/HelloHelper.cs*:

[!code-csharp[](./common/samples/3.x/BlazorSample/JsInteropClasses/HelloHelper.cs?name=snippet1&highlight=5,10-11)]

Konsola danych wyjściowych w narzędzia dla deweloperów sieci web w przeglądarce:

```console
Hello, Blazor!
```

## <a name="share-interop-code-in-a-razor-component-class-library"></a>Kód Interop SE udziału w bibliotece klas składników Razor

Biblioteki klas Razor składnika mogą być dołączane międzyoperacyjnego kodu JavaScript (`dotnet new blazorlib`), co pozwala na udostępnianie kodu w pakiecie NuGet.

Biblioteki klas Razor składnik obsługuje osadzanie zasobów JavaScript skompilowany zestaw. Pliki JavaScript są umieszczane w *wwwroot* folder i narzędzi dba o osadzaniu zasobów podczas kompilowania biblioteki.

Skompilowany pakiet NuGet odwołuje się do pliku projektu aplikacji, tak samo, jak odwołuje się do dowolnego normalnego pakietu NuGet. Po przywróceniu aplikacji kod aplikacji może wywołać na język JavaScript, tak jakby C#.
