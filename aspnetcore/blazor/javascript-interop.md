---
title: ASP.NET Core Blazor JavaScript Interop
author: guardrex
description: Dowiedz się, jak wywoływać funkcje języka JavaScript z technologii .NET i .NET z poziomu języka JavaScript w aplikacjach Blazor.
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 01/23/2020
no-loc:
- Blazor
- SignalR
uid: blazor/javascript-interop
ms.openlocfilehash: c4f2444b60fc2d3a8af893df379cf62636a7bdd5
ms.sourcegitcommit: d2ba66023884f0dca115ff010bd98d5ed6459283
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/14/2020
ms.locfileid: "77213366"
---
# <a name="aspnet-core-opno-locblazor-javascript-interop"></a>ASP.NET Core Blazor JavaScript Interop

[Javier Calvarro Nelson](https://github.com/javiercn), [Daniel Roth](https://github.com/danroth27)i [Luke](https://github.com/guardrex) Latham

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

Aplikacja Blazor może wywoływać funkcje języka JavaScript z technologii .NET i .NET z kodu JavaScript.

[Wyświetl lub pobierz przykładowy kod](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/blazor/common/samples/) ([jak pobrać](xref:index#how-to-download-a-sample))

## <a name="invoke-javascript-functions-from-net-methods"></a>Wywoływanie funkcji języka JavaScript z metod .NET

Istnieją przypadki, w których kod .NET jest wymagany do wywołania funkcji języka JavaScript. Na przykład wywołanie języka JavaScript może uwidaczniać możliwości przeglądarki lub funkcje z biblioteki JavaScript w aplikacji. Ten scenariusz jest nazywany *współdziałaniem języka JavaScript* (w programie*js Interop*).

Aby wywołać kod JavaScript z platformy .NET, użyj abstrakcji `IJSRuntime`. Aby wystawić wywołania w programie JS Interop, wstrzyknąć `IJSRuntime` abstrakcję w składniku. Metoda `InvokeAsync<T>` przyjmuje identyfikator funkcji języka JavaScript, która ma zostać wywołana wraz z dowolną liczbą argumentów z możliwością serializacji JSON. Identyfikator funkcji jest względny w stosunku do zakresu globalnego (`window`). Jeśli chcesz wywołać `window.someScope.someFunction`, identyfikator jest `someScope.someFunction`. Nie ma potrzeby rejestrowania funkcji przed jej wywołaniem. Typ zwracany `T` musi również być możliwy do serializacji kod JSON. `T` powinna być zgodna z typem .NET, który najlepiej jest mapowany do zwracanego typu JSON.

W przypadku aplikacji Blazor Server z włączoną obsługą prerenderowania Wywoływanie kodu JavaScript nie jest możliwe podczas początkowego wstępnego renderowania. Wywołania międzyoperacyjne języka JavaScript muszą zostać odroczone do momentu ustanowienia połączenia z przeglądarką. Aby uzyskać więcej informacji, zobacz sekcję [wykrywanie, kiedy aplikacja Blazor jest renderowana](#detect-when-a-blazor-app-is-prerendering) .

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

[!code-html[](./common/samples/3.x/BlazorWebAssemblySample/wwwroot/index.html?highlight=22)]

*Pages/_Host. cshtml* (Blazor Server):

[!code-cshtml[](./common/samples/3.x/BlazorServerSample/Pages/_Host.cshtml?highlight=35)]

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

*exampleJsInterop. js*:

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

## <a name="reference-elements-across-components"></a>Elementy odniesienia między składnikami

`ElementReference` jest gwarantowany tylko w metodzie `OnAfterRender` składnika (a odwołanie elementu jest `struct`), dlatego nie można przekazywać odwołania do elementu między składnikami.

Aby składnik nadrzędny mógł udostępnić odwołanie do elementu innym składnikom, składnik nadrzędny może:

* Zezwalaj składnikom podrzędnym na rejestrowanie wywołań zwrotnych.
* Wywołaj zarejestrowane wywołania zwrotne podczas zdarzenia `OnAfterRender` z odwołaniem do elementu. Pośrednio takie podejście umożliwia składnikom podrzędnym współdziałanie z odwołaniem do elementu nadrzędnego.

Poniższy przykład Blazor webassembly ilustruje podejście.

W `<head>` *wwwroot/index.html*:

```html
<style>
    .red { color: red }
</style>
```

W `<body>` *wwwroot/index.html*:

```html
<script>
    function setElementClass(element, className) {
        /** @type {HTMLElement} **/
        var myElement = element;
        myElement.classList.add(className);
    }
</script>
```

*Pages/index. Razor* (składnik nadrzędny):

```razor
@page "/"

<h1 @ref="_title">Hello, world!</h1>

Welcome to your new app.

<SurveyPrompt Parent="this" Title="How is Blazor working for you?" />
```

*Pages/index. Razor. cs*:

```csharp
using System;
using System.Collections.Generic;
using Microsoft.AspNetCore.Components;

namespace BlazorSample.Pages
{
    public partial class Index : 
        ComponentBase, IObservable<ElementReference>, IDisposable
    {
        private bool _disposing;
        private IList<IObserver<ElementReference>> _subscriptions = 
            new List<IObserver<ElementReference>>();
        private ElementReference _title;

        protected override void OnAfterRender(bool firstRender)
        {
            base.OnAfterRender(firstRender);

            foreach (var subscription in _subscriptions)
            {
                try
                {
                    subscription.OnNext(_title);
                }
                catch (Exception)
                {
                    throw;
                }
            }
        }

        public void Dispose()
        {
            _disposing = true;

            foreach (var subscription in _subscriptions)
            {
                try
                {
                    subscription.OnCompleted();
                }
                catch (Exception)
                {
                }
            }

            _subscriptions.Clear();
        }

        public IDisposable Subscribe(IObserver<ElementReference> observer)
        {
            if (_disposing)
            {
                throw new InvalidOperationException("Parent being disposed");
            }

            _subscriptions.Add(observer);

            return new Subscription(observer, this);
        }

        private class Subscription : IDisposable
        {
            public Subscription(IObserver<ElementReference> observer, Index self)
            {
                Observer = observer;
                Self = self;
            }

            public IObserver<ElementReference> Observer { get; }
            public Index Self { get; }

            public void Dispose()
            {
                Self._subscriptions.Remove(Observer);
            }
        }
    }
}
```

*Shared/SurveyPrompt. Razor* (składnik podrzędny):

```razor
@inject IJSRuntime JS

<div class="alert alert-secondary mt-4" role="alert">
    <span class="oi oi-pencil mr-2" aria-hidden="true"></span>
    <strong>@Title</strong>

    <span class="text-nowrap">
        Please take our
        <a target="_blank" class="font-weight-bold" 
            href="https://go.microsoft.com/fwlink/?linkid=2109206">brief survey</a>
    </span>
    and tell us what you think.
</div>

@code {
    [Parameter]
    public string Title { get; set; }
}
```

*Shared/SurveyPrompt. Razor. cs*:

```csharp
using System;
using Microsoft.AspNetCore.Components;

namespace BlazorSample.Shared
{
    public partial class SurveyPrompt : 
        ComponentBase, IObserver<ElementReference>, IDisposable
    {
        private IDisposable _subscription = null;

        [Parameter]
        public IObservable<ElementReference> Parent { get; set; }

        protected override void OnParametersSet()
        {
            base.OnParametersSet();

            if (_subscription != null)
            {
                _subscription.Dispose();
            }

            _subscription = Parent.Subscribe(this);
        }

        public void OnCompleted()
        {
            _subscription = null;
        }

        public void OnError(Exception error)
        {
            _subscription = null;
        }

        public void OnNext(ElementReference value)
        {
            JS.InvokeAsync<object>(
                "setElementClass", new object[] { value, "red" });
        }

        public void Dispose()
        {
            _subscription?.Dispose();
        }
    }
}
```

## <a name="invoke-net-methods-from-javascript-functions"></a>Wywoływanie metod .NET przy użyciu funkcji języka JavaScript

### <a name="static-net-method-call"></a>Statyczne wywołanie metody .NET

Aby wywołać statyczną metodę .NET z poziomu języka JavaScript, użyj funkcji `DotNet.invokeMethod` lub `DotNet.invokeMethodAsync`. Przekaż identyfikator metody statycznej, która ma być wywoływana, nazwę zestawu zawierającego funkcję i wszelkie argumenty. Wersja asynchroniczna jest preferowana do obsługi scenariuszy serwera Blazor. Metoda .NET musi być publiczna, statyczna i mieć atrybut `[JSInvokable]`. Wywoływanie otwartych metod ogólnych nie jest obecnie obsługiwane.

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

Domyślnie identyfikator metody jest nazwą metody, ale można określić inny identyfikator przy użyciu konstruktora `JSInvokableAttribute`:

```csharp
@code {
    [JSInvokable("DifferentMethodName")]
    public static Task<int[]> ReturnArrayAsync()
    {
        return Task.FromResult(new int[] { 1, 2, 3 });
    }
}
```

W pliku JavaScript po stronie klienta:

```javascript
returnArrayAsyncJs: function () {
  DotNet.invokeMethodAsync('BlazorSample', 'DifferentMethodName')
    .then(data => {
      data.push(4);
      console.log(data);
    });
}
```

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

*JsInteropClasses/ExampleJsInterop. cs*:

[!code-csharp[](./common/samples/3.x/BlazorWebAssemblySample/JsInteropClasses/ExampleJsInterop.cs?name=snippet1&highlight=10-16)]

*wwwroot/exampleJsInterop. js*:

[!code-javascript[](./common/samples/3.x/BlazorWebAssemblySample/wwwroot/exampleJsInterop.js?highlight=15-18)]

Nazwa jest przenoszona do konstruktora `HelloHelper`, który ustawia właściwość `HelloHelper.Name`. Gdy funkcja JavaScript `sayHello` jest wykonywana, `HelloHelper.SayHello` zwraca komunikat `Hello, {Name}!`, który jest zapisywana w konsoli przez funkcję JavaScript.

*JsInteropClasses/HelloHelper. cs*:

[!code-csharp[](./common/samples/3.x/BlazorWebAssemblySample/JsInteropClasses/HelloHelper.cs?name=snippet1&highlight=5,10-11)]

Dane wyjściowe konsoli w narzędziach deweloperskich sieci Web w przeglądarce:

```console
Hello, Blazor!
```

## <a name="share-interop-code-in-a-class-library"></a>Udostępnianie kodu międzyoperacyjnego w bibliotece klas

Kod międzyoperacyjny JS można uwzględnić w bibliotece klas, co umożliwia udostępnianie kodu w pakiecie NuGet.

Biblioteka klas obsługuje Osadzanie zasobów JavaScript w skompilowanym zestawie. Pliki JavaScript są umieszczane w folderze *wwwroot* . Narzędzia te zajmują się osadzaniem zasobów podczas kompilowania biblioteki.

Skompilowany pakiet NuGet jest przywoływany w pliku projektu aplikacji w taki sam sposób, w jaki jest przywoływany każdy pakiet NuGet. Po przywróceniu pakietu kod aplikacji może być wywoływany w języku JavaScript, tak jakby C#był.

Aby uzyskać więcej informacji, zobacz <xref:blazor/class-libraries>.

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

## <a name="perform-large-data-transfers-in-opno-locblazor-server-apps"></a>Wykonywanie dużych transferów danych w aplikacjach Blazor Server

W niektórych scenariuszach należy przenieść duże ilości danych między językami JavaScript i Blazor. Zwykle duże transfery danych odbywają się w przypadku:

* Interfejsy API systemu plików przeglądarki służą do przekazywania lub pobierania pliku.
* Wymagana jest współdziałanie z biblioteką innej firmy.

W programie Blazor Server ograniczenie jest stosowane w celu uniemożliwienia przekazywania pojedynczych dużych komunikatów, które mogą powodować problemy z wydajnością.

Podczas tworzenia kodu, który przesyła dane między językami JavaScript i Blazor, należy wziąć pod uwagę następujące wskazówki:

* Wydziel dane na mniejsze fragmenty i Wyślij segmenty danych sekwencyjnie, dopóki wszystkie dane nie zostaną odebrane przez serwer.
* Nie przydzielaj dużych obiektów w języku C# JavaScript i kodzie.
* Nie blokuj głównego wątku interfejsu użytkownika przez długie okresy podczas wysyłania lub otrzymywania danych.
* Zwolnij wszystkie używane pamięci, gdy proces zostanie ukończony lub anulowany.
* Wymuś następujące dodatkowe wymagania dotyczące zabezpieczeń:
  * Zadeklaruj maksymalny rozmiar pliku lub danych, który może zostać przesłany.
  * Zadeklaruj minimalną szybkość przekazywania z klienta na serwerze.
* Po odebraniu danych przez serwer dane mogą być następujące:
  * Tymczasowo przechowywane w buforze pamięci do momentu zebrania wszystkich segmentów.
  * Wykorzystano natychmiast. Na przykład dane mogą być przechowywane bezpośrednio w bazie danych lub zapisywane na dysku w miarę odbierania poszczególnych segmentów.

Następująca Klasa obiektu przekazującego plików obsługuje program JS Interop z klientem programu. Klasa obiektu przekazującego używa programu JS Interop do:

* Przesondowaj klienta, aby wysłał segment danych.
* Przerywaj transakcję, jeśli trwa sondowanie.

```csharp
using System;
using System.Buffers;
using System.Collections.Generic;
using System.IO;
using System.Threading.Tasks;
using Microsoft.JSInterop;

public class FileUploader : IDisposable
{
    private readonly IJSRuntime _jsRuntime;
    private readonly int _segmentSize = 6144;
    private readonly int _maxBase64SegmentSize = 8192;
    private readonly DotNetObjectReference<FileUploader> _thisReference;
    private List<IMemoryOwner<byte>> _uploadedSegments = 
        new List<IMemoryOwner<byte>>();

    public FileUploader(IJSRuntime jsRuntime)
    {
        _jsRuntime = jsRuntime;
    }

    public async Task<Stream> ReceiveFile(string selector, int maxSize)
    {
        var fileSize = 
            await _jsRuntime.InvokeAsync<int>("getFileSize", selector);

        if (fileSize > maxSize)
        {
            return null;
        }

        var numberOfSegments = Math.Floor(fileSize / (double)_segmentSize) + 1;
        var lastSegmentBytes = 0;
        string base64EncodedSegment;

        for (var i = 0; i < numberOfSegments; i++)
        {
            try
            {
                base64EncodedSegment = 
                    await _jsRuntime.InvokeAsync<string>(
                        "receiveSegment", i, selector);

                if (base64EncodedSegment.Length < _maxBase64SegmentSize && 
                    i < numberOfSegments - 1)
                {
                    return null;
                }
            }
            catch
            {
                return null;
            }

          var current = MemoryPool<byte>.Shared.Rent(_segmentSize);

          if (!Convert.TryFromBase64String(base64EncodedSegment, 
              current.Memory.Slice(0, _segmentSize).Span, out lastSegmentBytes))
          {
              return null;
          }

          _uploadedSegments.Add(current);
        }

        var segments = _uploadedSegments;
        _uploadedSegments = null;

        return new SegmentedStream(segments, _segmentSize, lastSegmentBytes);
    }

    public void Dispose()
    {
        if (_uploadedSegments != null)
        {
            foreach (var segment in _uploadedSegments)
            {
                segment.Dispose();
            }
        }
    }
}
```

W poprzednim przykładzie:

* `_maxBase64SegmentSize` jest ustawiona na `8192`, która jest obliczana na podstawie `_maxBase64SegmentSize = _segmentSize * 4 / 3`.
* Interfejsy API zarządzania pamięcią na niskim poziomie są używane do przechowywania segmentów pamięci na serwerze w `_uploadedSegments`.
* Metoda `ReceiveFile` służy do obsługi przekazywania za pomocą narzędzia JS Interop:
  * Rozmiar pliku jest określany w bajtach za pomocą narzędzia JS Interop z `_jsRuntime.InvokeAsync<FileInfo>('getFileSize', selector)`.
  * Liczba segmentów do odebrania jest obliczana i przechowywana w `numberOfSegments`.
  * Segmenty są żądane w pętli `for` za pomocą programu JS Interop z `_jsRuntime.InvokeAsync<string>('receiveSegment', i, selector)`. Wszystkie segmenty, ale ostatnie muszą mieć 8 192 bajtów przed dekodowaniem. Klient jest zmuszony do wydajnego wysyłania danych.
  * Podczas każdego odebranego segmentu sprawdzane są operacje przed dekodowaniem w <xref:System.Convert.TryFromBase64String*>.
  * Strumień z danymi jest zwracany jako nowy <xref:System.IO.Stream> (`SegmentedStream`) po zakończeniu przekazywania.

Klasa łańcucha segmentów uwidacznia listę segmentów jako niemożliwy do przeszukiwania <xref:System.IO.Stream>:

```csharp
using System;
using System.Buffers;
using System.Collections.Generic;
using System.IO;

public class SegmentedStream : Stream
{
    private readonly ReadOnlySequence<byte> _sequence;
    private long _currentPosition = 0;

    public SegmentedStream(IList<IMemoryOwner<byte>> segments, int segmentSize, 
        int lastSegmentSize)
    {
        if (segments.Count == 1)
        {
            _sequence = new ReadOnlySequence<byte>(
                segments[0].Memory.Slice(0, lastSegmentSize));
            return;
        }

        var sequenceSegment = new BufferSegment<byte>(
            segments[0].Memory.Slice(0, segmentSize));
        var lastSegment = sequenceSegment;

        for (int i = 1; i < segments.Count; i++)
        {
            var isLastSegment = i + 1 == segments.Count;
            lastSegment = lastSegment.Append(segments[i].Memory.Slice(
                0, isLastSegment ? lastSegmentSize : segmentSize));
        }

        _sequence = new ReadOnlySequence<byte>(
            sequenceSegment, 0, lastSegment, lastSegmentSize);
    }

    public override long Position
    {
        get => throw new NotImplementedException();
        set => throw new NotImplementedException();
    }

    public override int Read(byte[] buffer, int offset, int count)
    {
        var bytesToWrite = (int)(_currentPosition + count < _sequence.Length ? 
            count : _sequence.Length - _currentPosition);
        var data = _sequence.Slice(_currentPosition, bytesToWrite);
        data.CopyTo(buffer.AsSpan(offset, bytesToWrite));
        _currentPosition += bytesToWrite;

        return bytesToWrite;
    }

    private class BufferSegment<T> : ReadOnlySequenceSegment<T>
    {
        public BufferSegment(ReadOnlyMemory<T> memory)
        {
            Memory = memory;
        }

        public BufferSegment<T> Append(ReadOnlyMemory<T> memory)
        {
            var segment = new BufferSegment<T>(memory)
            {
                RunningIndex = RunningIndex + Memory.Length
            };

            Next = segment;

            return segment;
        }
    }

    public override bool CanRead => true;

    public override bool CanSeek => false;

    public override bool CanWrite => false;

    public override long Length => throw new NotImplementedException();

    public override void Flush() => throw new NotImplementedException();

    public override long Seek(long offset, SeekOrigin origin) => 
        throw new NotImplementedException();

    public override void SetLength(long value) => 
        throw new NotImplementedException();

    public override void Write(byte[] buffer, int offset, int count) => 
        throw new NotImplementedException();
}
```

Poniższy kod implementuje funkcje języka JavaScript do odbierania danych:

```javascript
function getFileSize(selector) {
  const file = getFile(selector);
  return file.size;
}

async function receiveSegment(segmentNumber, selector) {
  const file = getFile(selector);
  var segments = getFileSegments(file);
  var index = segmentNumber * 6144;
  return await getNextChunk(file, index);
}

function getFile(selector) {
  const element = document.querySelector(selector);
  if (!element) {
    throw new Error('Invalid selector');
  }
  const files = element.files;
  if (!files || files.length === 0) {
    throw new Error(`Element ${elementId} doesn't contain any files.`);
  }
  const file = files[0];
  return file;
}

function getFileSegments(file) {
  const segments = Math.floor(size % 6144 === 0 ? size / 6144 : 1 + size / 6144);
  return segments;
}

async function getNextChunk(file, index) {
  const length = file.size - index <= 6144 ? file.size - index : 6144;
  const chunk = file.slice(index, index + length);
  index += length;
  const base64Chunk = await this.base64EncodeAsync(chunk);
  return { base64Chunk, index };
}

async function base64EncodeAsync(chunk) {
  const reader = new FileReader();
  const result = new Promise((resolve, reject) => {
    reader.addEventListener('load',
      () => {
        const base64Chunk = reader.result;
        const cleanChunk = 
          base64Chunk.replace('data:application/octet-stream;base64,', '');
        resolve(cleanChunk);
      },
      false);
    reader.addEventListener('error', reject);
  });
  reader.readAsDataURL(chunk);
  return result;
}
```

## <a name="additional-resources"></a>Dodatkowe zasoby

* [InteropComponent. Razor — przykład (repozytorium dotnet/AspNetCore w witrynie GitHub, 3,1 gałąź wydania)](https://github.com/dotnet/AspNetCore/blob/release/3.1/src/Components/test/testassets/BasicTestApp/InteropComponent.razor)
