---
title: ASP.NET Core Blazor międzyoperacyjności JavaScript
author: guardrex
description: Dowiedz się, jak wywoływać funkcje języka JavaScript z technologii .NET i .NET z poziomu języka JavaScript w aplikacjach Blazor.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 10/15/2019
uid: blazor/javascript-interop
ms.openlocfilehash: b4776a20c6da6c722d2c057d19863c570f530a21
ms.sourcegitcommit: 35a86ce48041caaf6396b1e88b0472578ba24483
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/16/2019
ms.locfileid: "72391066"
---
# <a name="aspnet-core-blazor-javascript-interop"></a>ASP.NET Core Blazor międzyoperacyjności JavaScript

[Javier Calvarro Nelson](https://github.com/javiercn), [Daniel Roth](https://github.com/danroth27)i [Luke](https://github.com/guardrex) Latham

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

Aplikacja Blazor może wywoływać funkcje języka JavaScript z technologii .NET i .NET z kodu JavaScript.

[Wyświetlanie lub Pobieranie przykładowego kodu](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/blazor/common/samples/) ([jak pobrać](xref:index#how-to-download-a-sample))

## <a name="invoke-javascript-functions-from-net-methods"></a>Wywoływanie funkcji języka JavaScript z metod .NET

Istnieją przypadki, w których kod .NET jest wymagany do wywołania funkcji języka JavaScript. Na przykład wywołanie języka JavaScript może uwidaczniać możliwości przeglądarki lub funkcje z biblioteki JavaScript w aplikacji.

Aby wywołać kod JavaScript z platformy .NET, użyj abstrakcji `IJSRuntime`. Metoda `InvokeAsync<T>` przyjmuje identyfikator funkcji języka JavaScript, która ma zostać wywołana wraz z dowolną liczbą argumentów z możliwością serializacji JSON. Identyfikator funkcji jest względny w stosunku do zakresu globalnego (`window`). Jeśli chcesz wywołać `window.someScope.someFunction`, identyfikator jest `someScope.someFunction`. Nie ma potrzeby rejestrowania funkcji przed jej wywołaniem. Typ zwracany `T` musi być również możliwy do serializacji JSON.

Dla aplikacji serwera Blazor:

* Aplikacja serwera Blazor przetwarza wiele żądań użytkownika. Nie wywołuj `JSRuntime.Current` w składniku w celu wywołania funkcji JavaScript.
* Wstrzyknąć abstrakcyjność `IJSRuntime` i użyj wstrzykniętego obiektu do wystawienia wywołań międzyoperacyjnych języka JavaScript.
* Gdy aplikacja Blazor jest wstępnie renderowana, wywołanie do języka JavaScript nie jest możliwe, ponieważ połączenie z przeglądarką nie zostało nawiązane. Aby uzyskać więcej informacji, zobacz sekcję [wykrywanie, kiedy aplikacja Blazor jest renderowana](#detect-when-a-blazor-app-is-prerendering) .

Poniższy przykład jest oparty na [dekoderze](https://developer.mozilla.org/docs/Web/API/TextDecoder), eksperymentalnym dekoderem JavaScript. W przykładzie pokazano, jak wywołać funkcję JavaScript z C# metody. Funkcja JavaScript akceptuje tablicę bajtową z C# metody, dekoduje tablicę i zwraca tekst do składnika do wyświetlenia.

Wewnątrz elementu `<head>` *wwwroot/index.html* (Blazor webassembly) lub *Pages/_Host. cshtml* (Blazor Server) podaj funkcję, która używa `TextDecoder` do zdekodowania przekazaną tablicę:

[!code-html[](javascript-interop/samples_snapshot/index-script.html)]

Kod JavaScript, taki jak kod przedstawiony w powyższym przykładzie, można również załadować z pliku JavaScript ( *. js*) z odwołaniem do pliku skryptu:

```html
<script src="exampleJsInterop.js"></script>
```

Następujący składnik:

* Wywołuje funkcję JavaScript `ConvertArray` przy użyciu `JsRuntime`, gdy zostanie wybrany przycisk składnika (**Tablica konwersji**).
* Po wywołaniu funkcji języka JavaScript przenoszona tablica jest konwertowana na ciąg. Ciąg jest zwracany do składnika do wyświetlenia.

[!code-cshtml[](javascript-interop/samples_snapshot/call-js-example.razor?highlight=2,34-35)]

Aby użyć abstrakcji `IJSRuntime`, należy zastosować jedną z następujących metod:

* Wstrzyknąć abstrakcję `IJSRuntime` do składnika Razor ( *. Razor*):

  [!code-cshtml[](javascript-interop/samples_snapshot/inject-abstraction.razor?highlight=1)]

* Wstrzyknąć abstrakcję `IJSRuntime` do klasy ( *. cs*):

  [!code-csharp[](javascript-interop/samples_snapshot/inject-abstraction-class.cs?highlight=5)]

* W przypadku generowania zawartości dynamicznej przy użyciu [BuildRenderTree](xref:blazor/components#manual-rendertreebuilder-logic)należy użyć atrybutu `[Inject]`:

  ```csharp
  [Inject]
  IJSRuntime JSRuntime { get; set; }
  ```

W aplikacji przykładowej po stronie klienta, która jest dołączona do tego tematu, dostępne są dwie funkcje języka JavaScript, które współdziałają z modelem DOM, aby odbierać dane wejściowe użytkownika i wyświetlać komunikat powitalny:

* `showPrompt` &ndash; generuje monit o zaakceptowanie danych wprowadzonych przez użytkownika (nazwę użytkownika) i zwraca nazwę obiektu wywołującego.
* `displayWelcome` &ndash; przypisuje Komunikat powitalny od wywołującego do obiektu DOM z `id` z `welcome`.

*wwwroot/exampleJsInterop. js*:

[!code-javascript[](./common/samples/3.x/BlazorSample/wwwroot/exampleJsInterop.js?highlight=2-7)]

Umieść tag `<script>` odwołujący się do pliku JavaScript w pliku *wwwroot/index.html* (Blazor webassembly) lub *Pages/_Host. cshtml* (Blazor Server).

*wwwroot/index.html* (Blazor webassembly):

[!code-html[](./common/samples/3.x/BlazorSample/wwwroot/index.html?highlight=15)]

*Pages/_Host. cshtml* (Blazor Server):

[!code-cshtml[](javascript-interop/samples_snapshot/_Host.cshtml?highlight=29)]

Nie umieszczaj znacznika `<script>` w pliku składnika, ponieważ nie można dynamicznie zaktualizować tagu `<script>`.

.NET metod współdziałania z funkcjami JavaScript w pliku *exampleJsInterop. js* przez wywołanie `IJSRuntime.InvokeAsync<T>`.

Abstrakcja `IJSRuntime` jest asynchroniczna, aby umożliwić obsługę scenariuszy serwera Blazor. Jeśli aplikacja jest aplikacją webassembly Blazor i chcesz wywołać funkcję JavaScript synchronicznie, downcast do `IJSInProcessRuntime` i Wywołaj `Invoke<T>`. Zalecamy, aby większość bibliotek międzyoperacyjnych języka JavaScript używała asynchronicznych interfejsów API, aby upewnić się, że biblioteki są dostępne we wszystkich scenariuszach.

Przykładowa aplikacja zawiera składnik demonstrujący międzyoperacyjność JavaScript. Składnik:

* Odbiera dane wprowadzane przez użytkownika za pośrednictwem wiersza polecenia języka JavaScript.
* Zwraca tekst do składnika do przetworzenia.
* Wywołuje drugą funkcję języka JavaScript, która współdziała z modelem DOM, aby wyświetlić komunikat powitalny.

*Strony/JSInterop. Razor*:

[!code-cshtml[](./common/samples/3.x/BlazorSample/Pages/JsInterop.razor?name=snippet_JSInterop1&highlight=3,19-21,23-25)]

1. Gdy `TriggerJsPrompt` jest wykonywane przez wybranie przycisku **wyzwalacza skryptu JavaScript** składnika, funkcja JavaScript `showPrompt` dostępna w pliku *wwwroot/exampleJsInterop. js* jest wywoływana.
1. Funkcja `showPrompt` akceptuje dane wejściowe użytkownika (nazwę użytkownika), które są kodowane w formacie HTML i zwracane do składnika. Składnik przechowuje nazwę użytkownika w zmiennej lokalnej, `name`.
1. Ciąg przechowywany w `name` jest zawarty w komunikacie powitalnym, który jest przesyłany do funkcji języka JavaScript, `displayWelcome`, która renderuje Komunikat powitalny do znacznika nagłówka.

## <a name="call-a-void-javascript-function"></a>Wywoływanie funkcji języka JavaScript typu void

Funkcje języka JavaScript zwracające [wartość void (0)/void 0](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Operators/void) lub [undefined](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/undefined) są wywoływane z `IJSRuntime.InvokeVoidAsync`.

## <a name="detect-when-a-blazor-app-is-prerendering"></a>Wykryj, kiedy aplikacja Blazor jest renderowana
 
[!INCLUDE[](~/includes/blazor-prerendering.md)]

## <a name="capture-references-to-elements"></a>Przechwyć odwołania do elementów

Niektóre scenariusze [międzyoperacyjności języka JavaScript](xref:blazor/javascript-interop) wymagają odwołań do elementów HTML. Na przykład Biblioteka interfejsu użytkownika może wymagać odwołania do elementu dla inicjalizacji lub może być konieczne wywołanie interfejsów API, takich jak `focus` lub `play`.

Przechwyć odwołania do elementów HTML w składniku, korzystając z następującej metody:

* Dodaj atrybut `@ref` do elementu HTML.
* Zdefiniuj pole typu `ElementReference`, którego nazwa pasuje do wartości atrybutu `@ref`.

Poniższy przykład pokazuje przechwytywanie odwołania do elementu `username` `<input>`:

```cshtml
<input @ref="username" ... />

@code {
    ElementReference username;
}
```

> [!NOTE]
> **Nie** używaj przechwyconych odwołań do elementów jako sposobu wypełniania modelu DOM. Wykonanie tej czynności może zakłócać model renderowania deklaratywnego.

W odniesieniu do kodu platformy .NET `ElementReference` jest nieprzezroczystym uchwytem. *Jedyną* czynnością, którą można wykonać przy użyciu `ElementReference`, jest przekazanie go do kodu JavaScript za pośrednictwem międzyoperacyjności języka JavaScript. Gdy to zrobisz, kod po stronie JavaScript odbiera wystąpienie `HTMLElement`, które może być używane z normalnymi interfejsami API modelu DOM.

Na przykład poniższy kod definiuje metodę rozszerzenia .NET, która umożliwia ustawienie fokusu na elemencie:

*exampleJsInterop. js*:

```javascript
window.exampleJsFunctions = {
  focusElement : function (element) {
    element.focus();
  }
}
```

Użyj `IJSRuntime.InvokeAsync<T>` i Wywołaj `exampleJsFunctions.focusElement` z `ElementReference`, aby skoncentrować element:

[!code-cshtml[](javascript-interop/samples_snapshot/component1.razor?highlight=1,3,11-12)]

Aby użyć metody rozszerzenia do skoncentrowania się na elemencie, Utwórz statyczną metodę rozszerzenia, która odbiera wystąpienie `IJSRuntime`:

```csharp
public static Task Focus(this ElementReference elementRef, IJSRuntime jsRuntime)
{
    return jsRuntime.InvokeAsync<object>(
        "exampleJsFunctions.focusElement", elementRef);
}
```

Metoda jest wywoływana bezpośrednio dla obiektu. W poniższym przykładzie przyjęto założenie, że metoda static `Focus` jest dostępna z przestrzeni nazw `JsInteropClasses`:

[!code-cshtml[](javascript-interop/samples_snapshot/component2.razor?highlight=1,4,12)]

> [!IMPORTANT]
> Zmienna `username` jest wypełniana tylko po wyrenderowaniu składnika. W przypadku niewypełnienia `ElementReference` do kodu JavaScript kod JavaScript otrzymuje wartość `null`. Aby manipulować odwołaniami do elementów po zakończeniu renderowania składnika (aby ustawić początkowy fokus w elemencie), użyj [metod cyklu życia składnika](xref:blazor/components#lifecycle-methods)`OnAfterRenderAsync` lub `OnAfterRender`.

## <a name="invoke-net-methods-from-javascript-functions"></a>Wywoływanie metod .NET przy użyciu funkcji języka JavaScript

### <a name="static-net-method-call"></a>Statyczne wywołanie metody .NET

Aby wywołać statyczną metodę .NET z poziomu języka JavaScript, użyj funkcji `DotNet.invokeMethod` lub `DotNet.invokeMethodAsync`. Przekaż identyfikator metody statycznej, która ma być wywoływana, nazwę zestawu zawierającego funkcję i wszelkie argumenty. Wersja asynchroniczna jest preferowana do obsługi scenariuszy serwera Blazor. Aby wywołać metodę .NET z języka JavaScript, metoda .NET musi być publiczna, statyczna i mieć atrybut `[JSInvokable]`. Domyślnie identyfikator metody jest nazwą metody, ale można określić inny identyfikator przy użyciu konstruktora `JSInvokableAttribute`. Wywoływanie otwartych metod ogólnych nie jest obecnie obsługiwane.

Przykładowa aplikacja zawiera C# metodę zwracającą tablicę `int`S. Atrybut `JSInvokable` jest stosowany do metody.

*Strony/JsInterop. Razor*:

[!code-cshtml[](./common/samples/3.x/BlazorSample/Pages/JsInterop.razor?name=snippet_JSInterop2&highlight=7-11)]

Kod JavaScript obsługiwany przez klienta wywołuje metodę C# .NET.

*wwwroot/exampleJsInterop. js*:

[!code-javascript[](./common/samples/3.x/BlazorSample/wwwroot/exampleJsInterop.js?highlight=8-14)]

Gdy zostanie wybrany przycisk **ReturnArrayAsync Wyzwalaj metodę statyczną .NET** , sprawdź dane wyjściowe konsoli w narzędziach deweloperskich sieci Web w przeglądarce.

Dane wyjściowe konsoli są następujące:

```console
Array(4) [ 1, 2, 3, 4 ]
```

Czwarta wartość tablicy jest wypychana do tablicy (`data.push(4);`) zwróconej przez `ReturnArrayAsync`.

### <a name="instance-method-call"></a>Wywołanie metody wystąpienia

Można również wywołać metody wystąpienia platformy .NET z poziomu języka JavaScript. Aby wywołać metodę wystąpienia platformy .NET z poziomu języka JavaScript:

* Przekaż wystąpienie programu .NET do języka JavaScript, zawijając je w wystąpieniu `DotNetObjectReference`. Wystąpienie programu .NET jest przesyłane przez odwołanie do języka JavaScript.
* Wywołaj metody wystąpienia platformy .NET w wystąpieniu przy użyciu funkcji `invokeMethod` lub `invokeMethodAsync`. Wystąpienie programu .NET może być również przekazywać jako argument podczas wywoływania innych metod .NET z JavaScript.

> [!NOTE]
> Przykładowa aplikacja rejestruje komunikaty do konsoli po stronie klienta. W poniższych przykładach zademonstrowanych przez przykładową aplikację można sprawdzić dane wyjściowe konsoli przeglądarki w narzędziach deweloperskich w przeglądarce.

Po wybraniu przycisku **wyzwalanie metody wystąpienia platformy .NET HelloHelper. sayHello** , `ExampleJsInterop.CallHelloHelperSayHello` jest wywoływana i przekazuje nazwę, `Blazor` do metody.

*Strony/JsInterop. Razor*:

[!code-cshtml[](./common/samples/3.x/BlazorSample/Pages/JsInterop.razor?name=snippet_JSInterop3&highlight=8-9)]

`CallHelloHelperSayHello` wywołuje funkcję JavaScript `sayHello` z nowym wystąpieniem `HelloHelper`.

*JsInteropClasses/ExampleJsInterop. cs*:

[!code-csharp[](./common/samples/3.x/BlazorSample/JsInteropClasses/ExampleJsInterop.cs?name=snippet1&highlight=10-16)]

*wwwroot/exampleJsInterop. js*:

[!code-javascript[](./common/samples/3.x/BlazorSample/wwwroot/exampleJsInterop.js?highlight=15-18)]

Nazwa jest przenoszona do konstruktora `HelloHelper`, który ustawia właściwość `HelloHelper.Name`. Gdy funkcja JavaScript `sayHello` jest wykonywana, `HelloHelper.SayHello` zwróci komunikat `Hello, {Name}!`, który jest zapisywana w konsoli przez funkcję JavaScript.

*JsInteropClasses/HelloHelper. cs*:

[!code-csharp[](./common/samples/3.x/BlazorSample/JsInteropClasses/HelloHelper.cs?name=snippet1&highlight=5,10-11)]

Dane wyjściowe konsoli w narzędziach deweloperskich sieci Web w przeglądarce:

```console
Hello, Blazor!
```

## <a name="share-interop-code-in-a-class-library"></a>Udostępnianie kodu międzyoperacyjnego w bibliotece klas

Kod międzyoperacyjny JavaScript może być uwzględniony w bibliotece klas, co umożliwia udostępnianie kodu w pakiecie NuGet.

Biblioteka klas obsługuje Osadzanie zasobów JavaScript w skompilowanym zestawie. Pliki JavaScript są umieszczane w folderze *wwwroot* . Narzędzia te zajmują się osadzaniem zasobów podczas kompilowania biblioteki.

Skompilowany pakiet NuGet jest przywoływany w pliku projektu aplikacji w taki sam sposób, w jaki jest przywoływany każdy pakiet NuGet. Po przywróceniu pakietu kod aplikacji może być wywoływany w języku JavaScript, tak jakby C#był.

Aby uzyskać więcej informacji, zobacz <xref:blazor/class-libraries>.

## <a name="harden-js-interop-calls"></a>Zabezpieczenia wywołań międzyoperacyjnych w ramach funkcjonalności JS

Usługa JS Interop może zakończyć się niepowodzeniem z powodu błędów sieci i powinna być traktowana jako niezawodna. Domyślnie aplikacja serwera Blazor przeprowadzi limit czasu wywołań międzyoperacyjnych JS na serwerze po jednej minucie. Jeśli aplikacja może tolerować bardziej agresywny limit czasu, na przykład 10 sekund, należy ustawić limit czasu przy użyciu jednej z następujących metod:

* Globalnie w `Startup.ConfigureServices` Określ limit czasu:

  ```csharp
  services.AddServerSideBlazor(
      options => options.JSInteropDefaultCallTimeout = TimeSpan.FromSeconds({SECONDS}));
  ```

* Dla wywołania w kodzie składnika pojedyncze wywołanie może określać limit czasu:

  ```csharp
  var result = await JSRuntime.InvokeAsync<string>("MyJSOperation", 
      TimeSpan.FromSeconds({SECONDS}), new[] { "Arg1" });
  ```

Aby uzyskać więcej informacji na temat wyczerpania zasobów, zobacz <xref:security/blazor/server>.
