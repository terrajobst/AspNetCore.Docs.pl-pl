---
title: ASP.NET Core Blazor JavaScript Interop
author: guardrex
description: Dowiedz się, jak wywoływać funkcje języka JavaScript z technologii .NET i .NET z poziomu języka JavaScript w aplikacjach Blazor.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 12/05/2019
no-loc:
- Blazor
uid: blazor/javascript-interop
ms.openlocfilehash: 05225b86701b7a5d5c84dd43afbef70dd1ece228
ms.sourcegitcommit: 851b921080fe8d719f54871770ccf6f78052584e
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 12/09/2019
ms.locfileid: "74944073"
---
# <a name="aspnet-core-opno-locblazor-javascript-interop"></a>ASP.NET Core Blazor JavaScript Interop

[Javier Calvarro Nelson](https://github.com/javiercn), [Daniel Roth](https://github.com/danroth27)i [Luke](https://github.com/guardrex) Latham

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

Aplikacja Blazor może wywoływać funkcje języka JavaScript z technologii .NET i .NET z kodu JavaScript.

[Wyświetlanie lub pobieranie przykładowego kodu](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/blazor/common/samples/) ([sposobu pobierania](xref:index#how-to-download-a-sample))

## <a name="invoke-javascript-functions-from-net-methods"></a>Wywoływanie funkcji języka JavaScript z metod .NET

Istnieją przypadki, w których kod .NET jest wymagany do wywołania funkcji języka JavaScript. Na przykład wywołanie języka JavaScript może uwidaczniać możliwości przeglądarki lub funkcje z biblioteki JavaScript w aplikacji. Ten scenariusz jest nazywany *współdziałaniem języka JavaScript* (w programie*js Interop*).

Aby wywołać kod JavaScript z platformy .NET, użyj abstrakcji `IJSRuntime`. Metoda `InvokeAsync<T>` przyjmuje identyfikator funkcji języka JavaScript, która ma zostać wywołana wraz z dowolną liczbą argumentów z możliwością serializacji JSON. Identyfikator funkcji jest względny w stosunku do zakresu globalnego (`window`). Jeśli chcesz wywołać `window.someScope.someFunction`, identyfikator jest `someScope.someFunction`. Nie ma potrzeby rejestrowania funkcji przed jej wywołaniem. Typ zwracany `T` musi również być możliwy do serializacji kod JSON. `T` powinna być zgodna z typem .NET, który najlepiej jest mapowany do zwracanego typu JSON.

W przypadku aplikacji Blazor Server:

* Aplikacja serwera Blazor przetwarza wiele żądań użytkowników. Nie wywołuj `JSRuntime.Current` w składniku w celu wywołania funkcji JavaScript.
* Wstrzyknąć `IJSRuntime` abstrakcję i użyj wstrzykniętego obiektu do wystawienia wywołań międzyoperacyjnych JS.
* Gdy aplikacja Blazor jest wstępnie renderowana, wywołanie do języka JavaScript nie jest możliwe, ponieważ połączenie z przeglądarką nie zostało nawiązane. Aby uzyskać więcej informacji, zobacz sekcję [wykrywanie, kiedy aplikacja Blazor jest renderowana](#detect-when-a-blazor-app-is-prerendering) .

Poniższy przykład jest oparty na [dekoderze](https://developer.mozilla.org/docs/Web/API/TextDecoder), eksperymentalnym dekoderem JavaScript. W przykładzie pokazano, jak wywołać funkcję JavaScript z C# metody. Funkcja JavaScript akceptuje tablicę bajtową z C# metody, dekoduje tablicę i zwraca tekst do składnika do wyświetlenia.

Wewnątrz elementu `<head>` *wwwroot/index.html* (Blazor webassembly) lub *pages/_Host. cshtml* (Blazor Server) podaj funkcję języka JavaScript, która używa `TextDecoder` do zdekodowania przekazaną tablicę i zwrócenie zdekodowanej wartości:

[!code-html[](javascript-interop/samples_snapshot/index-script-convertarray.html)]

Kod JavaScript, taki jak kod przedstawiony w powyższym przykładzie, można również załadować z pliku JavaScript ( *. js*) z odwołaniem do pliku skryptu:

```html
<script src="exampleJsInterop.js"></script>
```

Następujący składnik:

* Wywołuje funkcję `convertArray` JavaScript przy użyciu `JSRuntime`, gdy zostanie wybrany przycisk składnika (**Konwertuj tablicę**).
* Po wywołaniu funkcji języka JavaScript przenoszona tablica jest konwertowana na ciąg. Ciąg jest zwracany do składnika do wyświetlenia.

[!code-razor[](javascript-interop/samples_snapshot/call-js-example.razor?highlight=2,34-35)]

## <a name="use-of-ijsruntime"></a>Korzystanie z IJSRuntime

Aby użyć abstrakcji `IJSRuntime`, należy zastosować jedną z następujących metod:

* Wsuń `IJSRuntime` abstrakcję do składnika Razor ( *. Razor*):

  [!code-razor[](javascript-interop/samples_snapshot/inject-abstraction.razor?highlight=1)]

  Wewnątrz elementu `<head>` *wwwroot/index.html* (Blazor webassembly) lub *pages/_Host. cshtml* (Blazor Server) podaj funkcję JavaScript `handleTickerChanged`. Funkcja jest wywoływana z `IJSRuntime.InvokeVoidAsync` i nie zwraca wartości:

  [!code-html[](javascript-interop/samples_snapshot/index-script-handleTickerChanged1.html)]

* Wstrzyknąć `IJSRuntime` abstrakcję do klasy ( *. cs*):

  [!code-csharp[](javascript-interop/samples_snapshot/inject-abstraction-class.cs?highlight=5)]

  Wewnątrz elementu `<head>` *wwwroot/index.html* (Blazor webassembly) lub *pages/_Host. cshtml* (Blazor Server) podaj funkcję JavaScript `handleTickerChanged`. Funkcja jest wywoływana z `JSRuntime.InvokeAsync` i zwraca wartość:

  [!code-html[](javascript-interop/samples_snapshot/index-script-handleTickerChanged2.html)]

* Aby można było wygenerować zawartość dynamiczną przy użyciu [BuildRenderTree](xref:blazor/components#manual-rendertreebuilder-logic), użyj atrybutu `[Inject]`:

  ```razor
  [Inject]
  IJSRuntime JSRuntime { get; set; }
  ```

W aplikacji przykładowej po stronie klienta, która jest dołączona do tego tematu, dostępne są dwie funkcje języka JavaScript, które współdziałają z modelem DOM, aby odbierać dane wejściowe użytkownika i wyświetlać komunikat powitalny:

* `showPrompt` &ndash; generuje monit o zaakceptowanie danych wprowadzonych przez użytkownika (nazwę użytkownika) i zwraca nazwę obiektu wywołującego.
* `displayWelcome` &ndash; przypisuje Komunikat powitalny od wywołującego do obiektu DOM z `id` `welcome`.

*wwwroot/exampleJsInterop. js*:

[!code-javascript[](./common/samples/3.x/BlazorWebAssemblySample/wwwroot/exampleJsInterop.js?highlight=2-7)]

Umieść tag `<script>` odwołujący się do pliku JavaScript w pliku *wwwroot/index.html* (Blazor webassembly) lub *pages/_Host. cshtml* (serwerBlazor).

*wwwroot/index.html* (Blazor webassembly):

[!code-html[](./common/samples/3.x/BlazorWebAssemblySample/wwwroot/index.html?highlight=15)]

*Pages/_Host. cshtml* (Blazor Server):

[!code-cshtml[](./common/samples/3.x/BlazorServerSample/Pages/_Host.cshtml?highlight=21)]

Nie umieszczaj znacznika `<script>` w pliku składnika, ponieważ nie można dynamicznie zaktualizować tagu `<script>`.

.NET metod współdziałania z funkcjami JavaScript w pliku *exampleJsInterop. js* przez wywołanie `IJSRuntime.InvokeAsync<T>`.

Abstrakcja `IJSRuntime` jest asynchroniczna, aby umożliwić obsługę scenariuszy Blazor Server. Jeśli aplikacja jest aplikacją Blazor webassembly i chcesz wywołać funkcję JavaScript synchronicznie, downcast do `IJSInProcessRuntime` i Wywołaj `Invoke<T>` zamiast tego. Zalecamy, aby większość bibliotek międzyoperacyjnych JS używała asynchronicznych interfejsów API, aby upewnić się, że biblioteki są dostępne we wszystkich scenariuszach.

Przykładowa aplikacja zawiera składnik demonstrujący międzyoperacyjność JS. Składnik:

* Odbiera dane wprowadzane przez użytkownika za pośrednictwem wiersza polecenia języka JavaScript.
* Zwraca tekst do składnika do przetworzenia.
* Wywołuje drugą funkcję języka JavaScript, która współdziała z modelem DOM, aby wyświetlić komunikat powitalny.

*Strony/JSInterop. Razor*:

```razor
@page "/JSInterop"
@using BlazorSample.JsInteropClasses
@inject IJSRuntime JSRuntime

<h1>JavaScript Interop</h1>

<h2>Invoke JavaScript functions from .NET methods</h2>

<button type="button" class="btn btn-primary" @onclick="TriggerJsPrompt">
    Trigger JavaScript Prompt
</button>

<h3 id="welcome" style="color:green;font-style:italic"></h3>

@code {
    public async Task TriggerJsPrompt()
    {
        // showPrompt is implemented in wwwroot/exampleJsInterop.js
        var name = await JSRuntime.InvokeAsync<string>(
                "exampleJsFunctions.showPrompt",
                "What's your name?");
        // displayWelcome is implemented in wwwroot/exampleJsInterop.js
        await JSRuntime.InvokeVoidAsync(
                "exampleJsFunctions.displayWelcome",
                $"Hello {name}! Welcome to Blazor!");
    }
}
```

1. Gdy `TriggerJsPrompt` jest wykonywane, wybierając przycisk **Monituj wyzwalacza JavaScript** składnika, funkcja JavaScript `showPrompt` dostępna w pliku *wwwroot/exampleJsInterop. js* jest wywoływana.
1. Funkcja `showPrompt` akceptuje dane wejściowe użytkownika (nazwę użytkownika), które są kodowane w formacie HTML i zwracane do składnika. Składnik przechowuje nazwę użytkownika w zmiennej lokalnej, `name`.
1. Ciąg przechowywany w `name` jest zawarty w komunikacie powitalnym, który jest przesyłany do funkcji języka JavaScript, `displayWelcome`, która renderuje Komunikat powitalny do znacznika nagłówka.

## <a name="call-a-void-javascript-function"></a>Wywoływanie funkcji języka JavaScript typu void

Funkcje języka JavaScript zwracające [wartość void (0)/void 0](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Operators/void) lub [undefined](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/undefined) są wywoływane z `IJSRuntime.InvokeVoidAsync`.

## <a name="detect-when-a-opno-locblazor-app-is-prerendering"></a>Wykryj, kiedy aplikacja Blazora jest renderowana
 
[!INCLUDE[](~/includes/blazor-prerendering.md)]

## <a name="capture-references-to-elements"></a>Przechwyć odwołania do elementów

Niektóre scenariusze międzyoperacyjności JS wymagają odwołań do elementów HTML. Na przykład Biblioteka interfejsu użytkownika może wymagać odwołania do elementu dla inicjalizacji lub może być konieczne wywoływanie interfejsów API przypominających polecenia na elemencie, takich jak `focus` lub `play`.

Przechwyć odwołania do elementów HTML w składniku, korzystając z następującej metody:

* Dodaj atrybut `@ref` do elementu HTML.
* Zdefiniuj pole typu `ElementReference`, którego nazwa pasuje do wartości atrybutu `@ref`.

W poniższym przykładzie pokazano przechwytywanie odwołania do `username` elementu `<input>`:

```razor
<input @ref="username" ... />

@code {
    ElementReference username;
}
```

> [!WARNING]
> Użyj odwołania do elementu, aby zmodyfikować zawartość pustego elementu, który nie współdziała z Blazor. Ten scenariusz jest przydatny, gdy interfejs API innej firmy dostarcza zawartość do elementu. Ponieważ Blazor nie współdziała z elementem, nie ma możliwości konfliktu między reprezentacją elementu a obiektem DOM przez Blazor.
>
> W poniższym przykładzie jest *niebezpieczne* do mutacji zawartości listy nieuporządkowanej (`ul`), ponieważ Blazor współdziała z modelem dom w celu wypełnienia elementów listy elementu (`<li>`):
>
> ```razor
> <ul ref="MyList">
>     @foreach (var item in Todos)
>     {
>         <li>@item.Text</li>
>     }
> </ul>
> ```
>
> Jeśli program JS Interop przyniesie zawartość elementu `MyList` i Blazor próbuje zastosować różnic do elementu, różnice nie będą zgodne z modelem DOM.

W odniesieniu do kodu platformy .NET `ElementReference` jest nieprzezroczystym uchwytem. *Jedyną* czynnością, którą można wykonać za pomocą `ElementReference`, jest przekazanie jej do kodu JavaScript za pośrednictwem międzyoperacyjnego js. Gdy to zrobisz, kod po stronie JavaScript odbiera wystąpienie `HTMLElement`, które może być używane z normalnymi interfejsami API modelu DOM.

Na przykład poniższy kod definiuje metodę rozszerzenia .NET, która umożliwia ustawienie fokusu na elemencie:

*exampleJsInterop.js*:

```javascript
window.exampleJsFunctions = {
  focusElement : function (element) {
    element.focus();
  }
}
```

Aby wywołać funkcję języka JavaScript, która nie zwraca wartości, użyj `IJSRuntime.InvokeVoidAsync`. Poniższy kod ustawia fokus na wejściu do nazwy użytkownika, wywołując poprzednią funkcję JavaScript z przechwyconą `ElementReference`:

[!code-razor[](javascript-interop/samples_snapshot/component1.razor?highlight=1,3,11-12)]

Aby użyć metody rozszerzenia, Utwórz statyczną metodę rozszerzenia, która odbiera wystąpienie `IJSRuntime`:

```csharp
public static async Task Focus(this ElementReference elementRef, IJSRuntime jsRuntime)
{
    await jsRuntime.InvokeVoidAsync(
        "exampleJsFunctions.focusElement", elementRef);
}
```

Metoda `Focus` jest wywoływana bezpośrednio dla obiektu. W poniższym przykładzie przyjęto założenie, że metoda `Focus` jest dostępna z przestrzeni nazw `JsInteropClasses`:

[!code-razor[](javascript-interop/samples_snapshot/component2.razor?highlight=1-4,12)]

> [!IMPORTANT]
> Zmienna `username` jest wypełniana tylko po wyrenderowaniu składnika. Jeśli niewypełniony `ElementReference` jest przekazywane do kodu JavaScript, kod JavaScript otrzymuje wartość `null`. Aby manipulować odwołaniami do elementów po zakończeniu renderowania składnika (aby ustawić początkowy fokus w elemencie), użyj [metod cyklu życia składnika OnAfterRenderAsync lub OnAfterRender](xref:blazor/lifecycle#after-component-render).

Podczas pracy z typami ogólnymi i zwracania wartości należy użyć [ValueTask\<t >](xref:System.Threading.Tasks.ValueTask`1):

```csharp
public static ValueTask<T> GenericMethod<T>(this ElementReference elementRef, 
    IJSRuntime jsRuntime)
{
    return jsRuntime.InvokeAsync<T>(
        "exampleJsFunctions.doSomethingGeneric", elementRef);
}
```

`GenericMethod` jest wywoływana bezpośrednio w obiekcie z typem. W poniższym przykładzie przyjęto założenie, że `GenericMethod` jest dostępny z przestrzeni nazw `JsInteropClasses`:

[!code-razor[](javascript-interop/samples_snapshot/component3.razor?highlight=17)]

## <a name="invoke-net-methods-from-javascript-functions"></a>Wywoływanie metod .NET przy użyciu funkcji języka JavaScript

### <a name="static-net-method-call"></a>Statyczne wywołanie metody .NET

Aby wywołać statyczną metodę .NET z poziomu języka JavaScript, użyj funkcji `DotNet.invokeMethod` lub `DotNet.invokeMethodAsync`. Przekaż identyfikator metody statycznej, która ma być wywoływana, nazwę zestawu zawierającego funkcję i wszelkie argumenty. Wersja asynchroniczna jest preferowana do obsługi scenariuszy serwera Blazor. Aby wywołać metodę .NET z języka JavaScript, metoda .NET musi być publiczna, statyczna i mieć atrybut `[JSInvokable]`. Domyślnie identyfikator metody jest nazwą metody, ale można określić inny identyfikator przy użyciu konstruktora `JSInvokableAttribute`. Wywoływanie otwartych metod ogólnych nie jest obecnie obsługiwane.

Przykładowa aplikacja zawiera C# metodę zwracającą tablicę `int`s. Atrybut `JSInvokable` jest stosowany do metody.

*Strony/JsInterop. Razor*:

```razor
<button type="button" class="btn btn-primary"
        onclick="exampleJsFunctions.returnArrayAsyncJs()">
    Trigger .NET static method ReturnArrayAsync
</button>

@code {
    [JSInvokable]
    public static Task<int[]> ReturnArrayAsync()
    {
        return Task.FromResult(new int[] { 1, 2, 3 });
    }
}
```

Kod JavaScript obsługiwany przez klienta wywołuje metodę C# .NET.

*wwwroot/exampleJsInterop. js*:

[!code-javascript[](./common/samples/3.x/BlazorWebAssemblySample/wwwroot/exampleJsInterop.js?highlight=8-14)]

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

Po wybraniu przycisku **Wyzwalaj metodę wystąpienia .NET HelloHelper. sayHello** , `ExampleJsInterop.CallHelloHelperSayHello` jest wywoływana i przekazuje nazwę, `Blazor`, do metody.

*Strony/JsInterop. Razor*:

```razor
<button type="button" class="btn btn-primary" @onclick="TriggerNetInstanceMethod">
    Trigger .NET instance method HelloHelper.SayHello
</button>

@code {
    public async Task TriggerNetInstanceMethod()
    {
        var exampleJsInterop = new ExampleJsInterop(JSRuntime);
        await exampleJsInterop.CallHelloHelperSayHello("Blazor");
    }
}
```

`CallHelloHelperSayHello` wywołuje funkcję JavaScript `sayHello` z nowym wystąpieniem `HelloHelper`.

*JsInteropClasses/ExampleJsInterop.cs*:

[!code-csharp[](./common/samples/3.x/BlazorWebAssemblySample/JsInteropClasses/ExampleJsInterop.cs?name=snippet1&highlight=10-16)]

*wwwroot/exampleJsInterop. js*:

[!code-javascript[](./common/samples/3.x/BlazorWebAssemblySample/wwwroot/exampleJsInterop.js?highlight=15-18)]

Nazwa jest przenoszona do konstruktora `HelloHelper`, który ustawia właściwość `HelloHelper.Name`. Gdy funkcja JavaScript `sayHello` jest wykonywana, `HelloHelper.SayHello` zwraca komunikat `Hello, {Name}!`, który jest zapisywana w konsoli przez funkcję JavaScript.

*JsInteropClasses/HelloHelper.cs*:

[!code-csharp[](./common/samples/3.x/BlazorWebAssemblySample/JsInteropClasses/HelloHelper.cs?name=snippet1&highlight=5,10-11)]

Dane wyjściowe konsoli w narzędziach deweloperskich sieci Web w przeglądarce:

```console
Hello, Blazor!
```

## <a name="share-interop-code-in-a-class-library"></a>Udostępnianie kodu międzyoperacyjnego w bibliotece klas

Kod międzyoperacyjny JS można uwzględnić w bibliotece klas, co umożliwia udostępnianie kodu w pakiecie NuGet.

Biblioteka klas obsługuje Osadzanie zasobów JavaScript w skompilowanym zestawie. Pliki JavaScript są umieszczane w folderze *wwwroot* . Narzędzia te zajmują się osadzaniem zasobów podczas kompilowania biblioteki.

Skompilowany pakiet NuGet jest przywoływany w pliku projektu aplikacji w taki sam sposób, w jaki jest przywoływany każdy pakiet NuGet. Po przywróceniu pakietu kod aplikacji może być wywoływany w języku JavaScript, tak jakby C#był.

Aby uzyskać więcej informacji, zobacz temat <xref:blazor/class-libraries>.

## <a name="harden-js-interop-calls"></a>Zabezpieczenia wywołań międzyoperacyjnych w ramach funkcjonalności JS

Usługa JS Interop może zakończyć się niepowodzeniem z powodu błędów sieci i powinna być traktowana jako niezawodna. Domyślnie aplikacja serwerowa Blazor przeprowadziła limit czasu wywołań międzyoperacyjnych JS na serwerze po jednej minucie. Jeśli aplikacja może tolerować bardziej agresywny limit czasu, na przykład 10 sekund, należy ustawić limit czasu przy użyciu jednej z następujących metod:

* Globalnie w `Startup.ConfigureServices`Określ limit czasu:

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

## <a name="additional-resources"></a>Dodatkowe zasoby

* [Przykład InteropComponent. Razor (ASPNET/AspNetCore repozytorium GitHub, 3,0 gałąź wydania)](https://github.com/aspnet/AspNetCore/blob/release/3.0/src/Components/test/testassets/BasicTestApp/InteropComponent.razor)
