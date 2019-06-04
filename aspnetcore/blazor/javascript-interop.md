---
title: Blazor JavaScript interop
author: guardrex
description: Dowiedz się, jak wywoływać funkcje języka JavaScript z .NET i .NET metody z kodu JavaScript w aplikacjach Blazor.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 05/29/2019
uid: blazor/javascript-interop
ms.openlocfilehash: 0e4f6640beb77602edf1abd5d03b27b8badefd0c
ms.sourcegitcommit: a04eb20e81243930ec829a9db5dd5de49f669450
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/03/2019
ms.locfileid: "66470254"
---
# <a name="blazor-javascript-interop"></a>Blazor JavaScript interop

Przez [Javier Calvarro Nelson](https://github.com/javiercn), [Daniel Roth](https://github.com/danroth27), i [Luke Latham](https://github.com/guardrex)

Aplikacja Blazor można wywoływać funkcje języka JavaScript z .NET i .NET metody z kodu JavaScript.

## <a name="invoke-javascript-functions-from-net-methods"></a>Wywoływanie funkcji języka JavaScript z metod .NET

Istnieją terminy, gdy jest wymagane do wywołania funkcji JavaScript kodu platformy .NET. Na przykład wywołanie JavaScript mogą uwidocznić możliwości przeglądarki lub funkcji z biblioteki JavaScript do aplikacji.

Aby wywołać na język JavaScript z platformy .NET, należy użyć `IJSRuntime` abstrakcji. `InvokeAsync<T>` Metoda pobiera identyfikator dla funkcji języka JavaScript, którą chcesz wywołać wraz z dowolnej liczby argumentów do serializacji JSON. Identyfikator funkcji jest określana względem zakresu globalnego (`window`). Jeśli chcesz wywołać `window.someScope.someFunction`, identyfikator ma `someScope.someFunction`. Nie ma potrzeby, aby zarejestrować funkcja jest wywoływana. Zwracany typ `T` musi być również JSON do serializacji.

W przypadku aplikacji po stronie serwera:

* Wiele żądań użytkownika są przetwarzane przez aplikację po stronie serwera. Nie wywołuj `JSRuntime.Current` w składniku do wywoływania funkcji języka JavaScript.
* Wstrzykiwanie `IJSRuntime` pozyskiwania i użycia wprowadzonego obiektu do wystawiania wywołań międzyoperacyjnych w języku JavaScript.
* Gdy aplikacja Blazor jest prerendering, wywołanie JavaScript nie jest możliwe, ponieważ nie został utworzony połączenia za pośrednictwem przeglądarki. Aby uzyskać więcej informacji, zobacz [Wykryj, kiedy aplikacja Blazor jest prerendering](#detect-when-a-blazor-app-is-prerendering) sekcji.

Poniższy przykład jest oparty na [TextDecoder](https://developer.mozilla.org/docs/Web/API/TextDecoder), eksperymentalne dekodera oparte na języku JavaScript. W przykładzie pokazano, jak wywołać funkcję JavaScript z C# metody. Funkcja języka JavaScript akceptuje tablicy bajtów z C# metody dekoduje tablicy i zwraca tekst do składnika do wyświetlenia.

Wewnątrz `<head>` elementu *wwwroot/index.html* (Blazor po stronie klienta) lub *stron /\_Host.cshtml* (Blazor po stronie serwera), zapewnia funkcję, która używa `TextDecoder` do dekodowanie przekazana tablica:

[!code-html[](javascript-interop/samples_snapshot/index-script.html)]

Kod JavaScript, taki jak kod przedstawiony w poprzednim przykładzie, można również załadować z pliku JavaScript (*js*) za pomocą odwołania do pliku skryptu:

```html
<script src="exampleJsInterop.js"></script>
```

Następujących składników:

* Wywołuje `ConvertArray` funkcję języka JavaScript za pomocą `JsRuntime` przycisku składnika (**konwersji tablicy**) jest zaznaczona.
* Po wywołaniu funkcji JavaScript przekazana tablica jest konwertowana na ciąg. Ten ciąg jest zwracany do składnika do wyświetlenia.

[!code-cshtml[](javascript-interop/samples_snapshot/call-js-example.razor?highlight=2,34-35)]

Aby użyć `IJSRuntime` abstrakcji, przyjmuje żadnego z następujących metod:

* Wstrzykiwanie `IJSRuntime` abstrakcji do składnika Razor ( *.razor*):

  [!code-cshtml[](javascript-interop/samples_snapshot/inject-abstraction.razor?highlight=1)]

* Wstrzykiwanie `IJSRuntime` abstrakcji do klasy ( *.cs*):

  [!code-csharp[](javascript-interop/samples_snapshot/inject-abstraction-class.cs?highlight=5)]

* Dla dynamicznego generowania zawartości za pomocą [BuildRenderTree](xref:blazor/components#manual-rendertreebuilder-logic), użyj `[Inject]` atrybutu:

  ```csharp
  [Inject]
  IJSRuntime JSRuntime { get; set; }
  ```

W aplikacji przykładowej po stronie klienta, znajdująca się w tym temacie dwie funkcje języka JavaScript są dostępne dla aplikacji po stronie klienta, który wchodzić w interakcje z modelu DOM do odbierania danych wejściowych użytkownika i wyświetlania komunikatu powitalnego:

* `showPrompt` &ndash; Wyświetla monit o podanie, aby akceptować dane wejściowe użytkownika (nazwa użytkownika) i zwraca nazwę do obiektu wywołującego.
* `displayWelcome` &ndash; Przypisuje komunikat powitalny wywołujący do obiektu modelu DOM przy użyciu `id` z `welcome`.

*wwwroot/exampleJsInterop.js*:

[!code-javascript[](./common/samples/3.x/BlazorSample/wwwroot/exampleJsInterop.js?highlight=2-7)]

Miejsce `<script>` tag, który odwołuje się do pliku JavaScript w *wwwroot/index.html* pliku (Blazor po stronie klienta) lub *stron /\_Host.cshtml* pliku (Blazor po stronie serwera).

*Wwwroot/index.HTML* (Blazor po stronie klienta):

[!code-html[](./common/samples/3.x/BlazorSample/wwwroot/index.html?highlight=15)]

*Strony /\_Host.cshtml* (Blazor po stronie serwera):

[!code-cshtml[](javascript-interop/samples_snapshot/_Host.cshtml?highlight=29)]

Nie należy umieszczać `<script>` tagów w pliku składnika, ponieważ `<script>` tagów nie można zaktualizować dynamicznie.

.NET współdziałanie metod za pomocą języka JavaScript działa w *exampleJsInterop.js* pliku przez wywołanie metody `IJSRuntime.InvokeAsync<T>`.

`IJSRuntime` Abstrakcją jest asynchroniczne, aby zezwolić na potrzeby scenariuszy po stronie serwera. Jeśli aplikacja jest uruchamiana po stronie klienta, a użytkownik chce synchronicznie, wywołaj funkcję JavaScript takiej `IJSInProcessRuntime` i wywołać `Invoke<T>` zamiast tego. Firma Microsoft zaleca, aby większość bibliotek międzyoperacyjnego JavaScript upewnij się, że biblioteki są dostępne we wszystkich scenariuszach, po stronie klienta i po stronie serwera za pomocą async interfejsów API.

Przykładowa aplikacja zawiera składnik do zademonstrowania międzyoperacyjnego JavaScript. Składnik:

* Odbiera dane wejściowe użytkownika za pośrednictwem wiersza kodu JavaScript.
* Zwraca tekst do składnika do przetworzenia.
* Wywołania drugiej funkcji języka JavaScript, która współdziała z modelu DOM w celu wyświetlania komunikatu powitalnego.

*Pages/JSInterop.razor*:

[!code-cshtml[](./common/samples/3.x/BlazorSample/Pages/JsInterop.razor?name=snippet_JSInterop1&highlight=3,19-21,23-25)]

1. Gdy `TriggerJsPrompt` jest wykonywana przez wybranie elementu **wyzwalacza JavaScript monitu** przycisk JavaScript `showPrompt` funkcja udostępniana w *wwwroot/exampleJsInterop.js* plik jest wywoływana.
1. `showPrompt` Funkcja akceptuje dane wejściowe użytkownika (nazwa użytkownika), który jest kodowany w formacie HTML i zwrócone do składnika. Składnik nazwy użytkownika są przechowywane w zmiennej lokalnej `name`.
1. Ten ciąg jest przechowywany w `name` jest włączona do komunikat powitalny, który jest przekazywany do funkcji języka JavaScript, `displayWelcome`, który renderuje wiadomość powitalna w tagu nagłówka.

## <a name="call-a-void-javascript-function"></a>Wywołanie funkcji JavaScript void

Funkcji w języku JavaScript zwracające [void (0) / void 0](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Operators/void) lub [niezdefiniowane](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/undefined) są wywoływane przy użyciu `IJSRuntime.InvokeAsync<object>`, co powoduje zwrócenie `null`.

## <a name="detect-when-a-blazor-app-is-prerendering"></a>Wykryj, kiedy aplikacja Blazor jest prerendering
 
[!INCLUDE[](~/includes/blazor-prerendering.md)]

## <a name="capture-references-to-elements"></a>Przechwytywanie odwołania do elementów

Niektóre [międzyoperacyjnego JavaScript](xref:blazor/javascript-interop) scenariusze wymagają odwołania do elementów HTML. Na przykład biblioteka interfejsów użytkownika może wymagać odwołanie do elementu inicjowania lub może być konieczne wywołania interfejsów API jak polecenie elementu, takie jak `focus` lub `play`.

Istnieje możliwość przechwytywania odwołania do elementów HTML w składniku przy użyciu następujących podejść:

* Dodaj `ref` atrybutu do elementu HTML.
* Definiowanie pola typu `ElementRef` którego nazwa odpowiada wartości `ref` atrybutu.

W poniższym przykładzie pokazano przechwytywania odwołania do `username` `<input>` elementu:

```cshtml
<input ref="username" ... />

@functions {
    ElementRef username;
}
```

> [!NOTE]
> Czy **nie** używać odwołań do elementu przechwyconych jako sposobu wypełniania lub manipulowanie modelu DOM, gdy Blazor korzysta z elementów, do których odwołuje się. Ten sposób może kolidować z modelu deklaratywne renderowania.

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

[!code-cshtml[](javascript-interop/samples_snapshot/component1.razor?highlight=1,3,7,11-12)]

Można użyć metody rozszerzenia do skoncentrowania się element, należy utworzyć metody statyczne rozszerzenie, który odbiera `IJSRuntime` wystąpienie:

```csharp
public static Task Focus(this ElementRef elementRef, IJSRuntime jsRuntime)
{
    return jsRuntime.InvokeAsync<object>(
        "exampleJsFunctions.focusElement", elementRef);
}
```

Metoda jest wywoływana bezpośrednio na obiekcie. W poniższym przykładzie założono, że statyczne `Focus` metoda jest dostępna z `JsInteropClasses` przestrzeni nazw:

[!code-cshtml[](javascript-interop/samples_snapshot/component2.razor?highlight=1,4,8,12)]

> [!IMPORTANT]
> `username` Zmiennej tylko jest wypełniana po renderuje składnika i jego dane wyjściowe obejmują `>` elementu. Jeśli zostanie podjęta próba przekazania unpopulated `ElementRef` do kodu JavaScript, otrzyma kod JavaScript `null`. Do manipulowania odwołania do elementu, po zakończeniu renderowania (w celu ustawienie początkowy fokus w elemencie) Użyj składnika `OnAfterRenderAsync` lub `OnAfterRender` [metody cyklu życia składników](xref:blazor/components#lifecycle-methods).

## <a name="invoke-net-methods-from-javascript-functions"></a>Wywoływanie metod .NET z funkcji języka JavaScript

### <a name="static-net-method-call"></a>Statyczne wywołanie metody .NET

Aby wywołać metodę statyczną .NET poziomu języka JavaScript, należy użyć `DotNet.invokeMethod` lub `DotNet.invokeMethodAsync` funkcji. Przekaż identyfikator statyczna metoda, którą chcesz wywołać, nazwa zestawu zawierającego funkcję i żadnych argumentów. Wersja asynchroniczna jest preferowane w scenariuszach, po stronie serwera. Aby wywołać metodę .NET poziomu języka JavaScript, metoda .NET muszą być publiczne, statyczne i `[JSInvokable]` atrybutu. Domyślnie identyfikator metody jest nazwa metody, ale można określić przy użyciu innego identyfikatora `JSInvokableAttribute` konstruktora. Wywoływanie metod ogólnych open nie jest obecnie obsługiwane.

Przykładowa aplikacja zawiera C# metodę, aby zwrócić tablicę `int`s. `JSInvokable` Atrybut jest stosowany do metody.

*Pages/JsInterop.razor*:

[!code-cshtml[](./common/samples/3.x/BlazorSample/Pages/JsInterop.razor?name=snippet_JSInterop2&highlight=7-11)]

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

Można również wywołać metody wystąpienia .NET poziomu języka JavaScript. Aby wywołać metodę wystąpienia .NET poziomu języka JavaScript:

* Przekaż wystąpienie platformy .NET w kodzie JavaScript, opakowując go w `DotNetObjectRef` wystąpienia. Wystąpienie platformy .NET jest przekazywany przez odwołanie w kodzie JavaScript.
* Wywoływanie metod wystąpienia platformy .NET przy użyciu wystąpienia `invokeMethod` lub `invokeMethodAsync` funkcji. Wystąpienie programu .NET może być przekazywany jako argument, podczas wywoływania innych metod .NET poziomu języka JavaScript.

> [!NOTE]
> Przykładowa aplikacja rejestruje komunikaty w konsoli po stronie klienta. W poniższych przykładach pokazano przez aplikację przykładową Sprawdź dane wyjściowe konsoli w przeglądarce w narzędzia dla deweloperów w przeglądarce.

Gdy **metodę wystąpienia .NET wyzwalacza HelloHelper.SayHello** przycisk jest zaznaczony, `ExampleJsInterop.CallHelloHelperSayHello` nosi nazwę i przekazuje nazwę `Blazor`, do metody.

*Pages/JsInterop.razor*:

[!code-cshtml[](./common/samples/3.x/BlazorSample/Pages/JsInterop.razor?name=snippet_JSInterop3&highlight=8-9)]

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

## <a name="share-interop-code-in-a-class-library"></a>Kód Interop SE udziału w bibliotece klas

Kód Interop SE JavaScript mogą być dołączane w bibliotece klas, co pozwala na udostępnianie kodu w pakiecie NuGet.

Biblioteka klas obsługuje osadzanie zasobów JavaScript skompilowany zestaw. Pliki JavaScript są umieszczane w *wwwroot* folderu. Osadzanie zasobów podczas kompilowania biblioteki zajmuje się narzędzi.

Skompilowany pakiet NuGet odwołuje się do pliku projektu aplikacji, tak samo, jak odwołuje się do dowolnego normalnego pakietu NuGet. Po przywróceniu aplikacji kod aplikacji może wywołać na język JavaScript, tak jakby C#.

Aby uzyskać więcej informacji, zobacz <xref:blazor/class-libraries>.
