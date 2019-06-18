---
title: Tworzenie i używanie składników Razor
author: guardrex
description: Informacje o sposobie tworzenia i używania składników Razor, w tym jak powiązać z danymi, obsługa zdarzeń i Zarządzaj cyklami życia składników.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 06/12/2019
uid: blazor/components
ms.openlocfilehash: 25743550c60e1d027066cdefe137148de74b0715
ms.sourcegitcommit: 516f166c5f7cec54edf3d9c71e6e2ba53fb3b0e5
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/18/2019
ms.locfileid: "67196292"
---
# <a name="create-and-use-razor-components"></a>Tworzenie i używanie składników Razor

Przez [Luke Latham](https://github.com/guardrex), [Daniel Roth](https://github.com/danroth27), i [Morné Zaayman](https://github.com/MorneZaayman)

[Wyświetlanie lub pobieranie przykładowego kodu](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/blazor/common/samples/) ([sposobu pobierania](xref:index#how-to-download-a-sample))

Blazor aplikacje są tworzone przy użyciu *składniki*. Składnik jest niezależna fragmentu interfejsu użytkownika (UI), takich jak strony, okno dialogowe lub formularza. Składnik zawiera kod znaczników HTML i logika przetwarzania wymagane w celu wstrzyknięcia danych lub reagowania na zdarzenia interfejsu użytkownika. Składniki są elastyczne i uproszczone. Mogą być zagnieżdżone, ponownie i współużytkowane między projektami.

## <a name="component-classes"></a>Klasy składników

Składniki są implementowane w [Razor](xref:mvc/views/razor) pliki składników ( *.razor*) przy użyciu kombinacji C# i kod znaczników HTML.

Składniki mogą być tworzone za pomocą *.cshtml* rozszerzenie pliku, tak długo, jak pliki są identyfikowane jako pliki składnika Razor przy użyciu `_RazorComponentInclude` właściwości programu MSBuild. Na przykład aplikację, która określa, że wszystkie *.cshtml* plików w obszarze *stron* folder powinien być traktowany jako plików składników Razor:

```xml
<_RazorComponentInclude>Pages\**\*.cshtml</_RazorComponentInclude>
```

W interfejsie użytkownika dla składnika jest zdefiniowana za pomocą kodu HTML. Logika renderowania dynamicznego (na przykład pętli, warunkowych, wyrażeń) zostanie dodany przy użyciu osadzonych C# składni o nazwie [Razor](xref:mvc/views/razor). Gdy aplikacja jest kompilowana, kod znaczników HTML i C# logiki renderowania są konwertowane na klasy składnika. Nazwa wygenerowanej klasy odpowiada nazwie pliku.

Elementy członkowskie klasy składników są zdefiniowane w `@code` bloku. W `@code` bloku, stan składników (właściwości, pola) jest określony za pomocą metody obsługi zdarzeń lub Definiowanie logiki innych składników. Więcej niż jeden `@code` bloku jest dozwolone.

> [!NOTE]
> W poprzednich wersjach programu ASP.NET Core `@functions` bloki były używane dla tego samego celu co `@code` bloków. `@functions` bloki w dalszym ciągu działać, ale firma Microsoft zaleca używanie `@code` dyrektywy.

Elementy członkowskie składnika mogą posłużyć jako część składnika przez renderowanie przy użyciu logiki C# wyrażeń, które zaczyna się `@`. Na przykład C# pole jest renderowane przez dodanie przedrostka `@` na nazwę pola. Poniższy przykład daje w wyniku i renderuje:

* `_headingFontStyle` wartości właściwości CSS dla `font-style`.
* `_headingText` w treści `<h1>` elementu.

```cshtml
<h1 style="font-style:@_headingFontStyle">@_headingText</h1>

@code {
    private string _headingFontStyle = "italic";
    private string _headingText = "Put on your new Blazor!";
}
```

Po początkowo renderowania składnik składnika generuje jej drzewo renderowania w odpowiedzi na zdarzenia. Blazor następnie porównuje nowego drzewa renderowania względem poprzedniego i stosuje wszystkie zmiany do przeglądarki w modelu DOM (Document Object).

Składniki są zwykłe C# klasy i można umieścić w dowolnym miejscu w obrębie projektu. Składniki, które zwykle tworzą stron sieci Web znajdują się w *stron* folderu. Składniki strony inne niż często są umieszczane w *Shared* folder lub folder niestandardowy dodane do projektu. Aby korzystać z folderu niestandardowego, Dodaj niestandardowego folderu obszaru nazw do składnika nadrzędnego lub w aplikacji  *\_Imports.razor* pliku. Na przykład następująca przestrzeń nazw sprawia, że składniki *składniki* dostępne w przypadku aplikacji głównej przestrzeni nazw jest folder `WebApplication`:

```cshtml
@using WebApplication.Components
```

## <a name="integrate-components-into-razor-pages-and-mvc-apps"></a>Integrowanie składników aplikacji stronami Razor i programem MVC

Składniki za pomocą istniejących aplikacji stronami Razor i programem MVC. Nie ma potrzeby ponownego wpisywania istniejących stron lub widoków w celu używania składników Razor. Po wyrenderowaniu strony lub widoku składniki są prerendered&dagger; w tym samym czasie. 

> [!NOTE]
> &dagger;Prerendering po stronie serwera jest domyślnie włączona dla Blazor po stronie serwera aplikacji. Aplikacje Blazor po stronie klienta będzie obsługiwać prerendering w nadchodzącej wersji 5 (wersja zapoznawcza). Aby uzyskać więcej informacji, zobacz [aktualizacji szablonów/oprogramowanie pośredniczące MapFallbackToPage/pliku](https://github.com/aspnet/AspNetCore/issues/8852).

Aby renderować składnika ze strony lub widok, należy użyć `RenderComponentAsync<TComponent>` metody pomocnika kodu HTML:

```cshtml
<div id="Counter">
    @(await Html.RenderComponentAsync<Counter>(new { IncrementAmount = 10 }))
</div>
```

Gdy widoków i stron można użyć składników, prawdą nie dotyczy. Składniki nie można używać w scenariuszach specyficznych dla widoku i strony, takich jak widoki częściowe i sekcji. Aby użyć logiki z widoku częściowego w składniku, współczynnik logiki widoku częściowego do składnika.

Aby uzyskać więcej informacji na temat sposobu Wyrenderowana i składnika, stan składników odbywa się w aplikacji po stronie serwera Blazor, zobacz <xref:blazor/hosting-models> artykułu.

## <a name="using-components"></a>Używanie składników

Składniki mogą zawierać inne składniki, deklarując je przy użyciu składni elementu HTML. Znaczniki dla za pomocą składnika wygląda jak HTML tag, gdzie nazwa tagu jest typ składnika.

Następujące znaczniki w *Index.razor* renderuje `HeadingComponent` wystąpienie:

[!code-cshtml[](common/samples/3.x/BlazorSample/Pages/Index.razor?name=snippet_HeadingComponent)]

*Components/HeadingComponent.razor*:

[!code-cshtml[](common/samples/3.x/BlazorSample/Components/HeadingComponent.razor)]

## <a name="component-parameters"></a>Parametry składnika

Składniki mogą mieć *Parametry składnika*, które są definiowane za pomocą *niepublicznych* właściwości klasy składnika za pomocą `[Parameter]` atrybutu. Używanie atrybutów, aby określić argumenty dla składnika w znacznikach.

W poniższym przykładzie `ParentComponent` ustawia wartość `Title` właściwość `ChildComponent`.

*Pages/ParentComponent.razor*:

[!code-cshtml[](common/samples/3.x/BlazorSample/Pages/ParentComponent.razor?name=snippet_ParentComponent&highlight=5-6)]

*Components/ChildComponent.razor*:

[!code-cshtml[](common/samples/3.x/BlazorSample/Components/ChildComponent.razor?highlight=11-12)]

## <a name="child-content"></a>Zawartość elementu podrzędnego

Składniki można ustawić zawartości innego składnika. Przypisywanie składnik udostępnia zawartości między tagami, które określają odbieranie składnika. Na przykład `ParentComponent` może zapewnić zawartości dla renderowania przez składnik podrzędnych przez umieszczenie zawartość wewnątrz `<ChildComponent>` tagów.

*Pages/ParentComponent.razor*:

[!code-cshtml[](common/samples/3.x/BlazorSample/Pages/ParentComponent.razor?name=snippet_ParentComponent&highlight=7-8)]

Składnik podrzędny ma `ChildContent` właściwość, która reprezentuje `RenderFragment`. Wartość `ChildContent` jest umieszczony w znaczniku elementu podrzędnego, której zawartość ma być renderowany. W poniższym przykładzie wartość `ChildContent` jest otrzymane od składnika nadrzędnego i renderowania wewnątrz panelu Bootstrap `panel-body`.

*Components/ChildComponent.razor*:

[!code-cshtml[](common/samples/3.x/BlazorSample/Components/ChildComponent.razor?highlight=3,14-15)]

> [!NOTE]
> Odbieranie właściwość `RenderFragment` zawartość, musi nosić `ChildContent` przez Konwencję.

## <a name="data-binding"></a>Powiązanie danych

Powiązanie danych do składników i elementów DOM odbywa się za pomocą `@bind` atrybutu. Poniższy przykład tworzy powiązanie `_italicsCheck` pole do pola wyboru zaznaczone stanu:

```cshtml
<input type="checkbox" class="form-check-input" id="italicsCheck" 
    @bind="_italicsCheck" />
```

Gdy pole wyboru jest zaznaczone, a następnie wyczyszczone, wartość właściwości jest aktualizowany do `true` i `false`, odpowiednio.

Pole wyboru jest aktualizowana w interfejsie użytkownika tylko wtedy, gdy składnik jest renderowany, nie w odpowiedzi na zmianę wartości właściwości. Ponieważ składniki renderować się po wykonaniu kod procedury obsługi zdarzeń, aktualizacje właściwości są zazwyczaj odzwierciedlane w interfejsie użytkownika od razu.

Za pomocą `@bind` z `CurrentValue` właściwości (`<input @bind="CurrentValue" />`) jest zasadniczo odpowiednikiem następujących czynności:

```cshtml
<input value="@CurrentValue" 
    @onchange="@((UIChangeEventArgs __e) => CurrentValue = __e.Value)" />
```

Po wyrenderowaniu składnika `value` danego elementu wejściowego pochodzi z `CurrentValue` właściwości. Gdy użytkownik wpisuje w polu tekstowym `onchange` zdarzenie jest generowane i `CurrentValue` zostaje ustalona zmieniona wartość. W rzeczywistości generowania kodu jest nieco bardziej złożone, ponieważ `@bind` obsługuje kilka przypadków, w którym konwersje są wykonywane. W zasadzie `@bind` kojarzy bieżącą wartość wyrażenia z `value` atrybutu i uchwytów zmiany przy użyciu zarejestrowanego programu obsługi.

Oprócz `onchange`, właściwości mogą być powiązane przy użyciu innych zdarzeń, takich jak `oninput` , dodając `@bind` atrybutem `event` parametru:

```cshtml
<input type="text" @bind-value="@CurrentValue" @bind-value:event="oninput" />
```

W odróżnieniu od `onchange`, `oninput` generowane dla każdego znaku, która jest wprowadzana do pola tekstowego.

**Ciągi formatujące**

Powiązanie danych w programach <xref:System.DateTime> ciągi formatujące. Inne wyrażenia formatu, takie jak waluta lub formaty liczbowe nie są dostępne w tej chwili.

```cshtml
<input @bind="StartDate" @bind:format="yyyy-MM-dd" />

@code {
    [Parameter]
    private DateTime StartDate { get; set; } = new DateTime(2020, 1, 1);
}
```

`@bind:format` Atrybut określa format daty do zastosowania do `value` z `<input>` elementu. Format umożliwia również przeanalizować wartość po `onchange` wystąpi zdarzenie.

**Parametry składnika**

Powiązanie rozpoznaje również Parametry składnika, gdzie `@bind-{property}` można powiązać wartości właściwości między składnikami.

Używa następującego składnika `ChildComponent` i wiąże `ParentYear` parametru z obiektu `Year` parametru w składniku podrzędne:

Składnik nadrzędny:

```cshtml
@page "/ParentComponent"

<h1>Parent Component</h1>

<p>ParentYear: @ParentYear</p>

<ChildComponent @bind-Year="ParentYear" />

<button class="btn btn-primary" @onclick="@ChangeTheYear">
    Change Year to 1986
</button>

@code {
    [Parameter]
    private int ParentYear { get; set; } = 1978;

    private void ChangeTheYear()
    {
        ParentYear = 1986;
    }
}
```

Element podrzędny:

```cshtml
<h2>Child Component</h2>

<p>Year: @Year</p>

@code {
    [Parameter]
    private int Year { get; set; }

    [Parameter]
    private EventCallback<int> YearChanged { get; set; }
}
```

`EventCallback<T>` została wyjaśniona w [EventCallback](#eventcallback) sekcji.

Trwa ładowanie `ParentComponent` tworzy następujące znaczniki:

```html
<h1>Parent Component</h1>

<p>ParentYear: 1978</p>

<h2>Child Component</h2>

<p>Year: 1978</p>
```

Jeśli wartość `ParentYear` zmianie właściwości, wybierając przycisk w `ParentComponent`, `Year` właściwość `ChildComponent` jest aktualizowana. Nowa wartość `Year` jest wyświetlana w Interfejsie użytkownika po `ParentComponent` jest rerendered:

```html
<h1>Parent Component</h1>

<p>ParentYear: 1986</p>

<h2>Child Component</h2>

<p>Year: 1986</p>
```

`Year` Parametr jest możliwej do wiązania, ponieważ ma ona towarzyszące `YearChanged` zdarzeń, który jest zgodny z typem `Year` parametru.

Zgodnie z Konwencją `<ChildComponent @bind-Year="ParentYear" />` jest zasadniczo odpowiednikiem pisania,

```cshtml
<ChildComponent @bind-Year="ParentYear" @bind-Year:event="YearChanged" />
```

Ogólnie rzecz biorąc, można powiązać właściwości z odpowiedni program obsługi zdarzeń, za pomocą `@bind-property:event` atrybutu. Na przykład właściwość `MyProp` może być powiązana z `MyEventHandler` przy użyciu następujące atrybuty:

```cshtml
<FooComponent @bind-MyProp="MyValue" @bind-MyProp:event="MyEventHandler" />
```

## <a name="event-handling"></a>Obsługa zdarzeń

Składniki razor udostępniają funkcje obsługi zdarzeń. Atrybut elementu HTML o nazwie `on<event>` (na przykład `onclick`, `onsubmit`) z wartością wpisane delegata składniki Razor traktuje wartość atrybutu jako program obsługi zdarzeń. Nazwa atrybutu zawsze zaczyna się od `@on`.

Poniższy kod wywoła `UpdateHeading` metodę po wybraniu przycisku w interfejsie użytkownika:

```cshtml
<button class="btn btn-primary" @onclick="@UpdateHeading">
    Update heading
</button>

@code {
    private void UpdateHeading(UIMouseEventArgs e)
    {
        ...
    }
}
```

Poniższy kod wywoła `CheckboxChanged` metody, gdy pole wyboru jest zmieniana w interfejsie użytkownika:

```cshtml
<input type="checkbox" class="form-check-input" @onchange="@CheckboxChanged" />

@code {
    private void CheckboxChanged()
    {
        ...
    }
}
```

Programy obsługi zdarzeń można też asynchronicznego i zwracają <xref:System.Threading.Tasks.Task>. Nie ma konieczności ręcznego wywoływania `StateHasChanged()`. Wyjątki są rejestrowane w momencie ich wystąpienia.

```cshtml
<button class="btn btn-primary" @onclick="@UpdateHeading">
    Update heading
</button>

@code {
    private async Task UpdateHeading(UIMouseEventArgs e)
    {
        ...
    }
}
```

Niektóre zdarzenia są dozwolone typy argumentów zdarzenia określonego zdarzenia. Jeśli dostęp do jednego z następujących typów zdarzeń nie jest to konieczne, nie jest to wymagane w wywołaniu metody.

Lista argumentów zdarzeń obsługiwanych jest:

* UIEventArgs
* UIChangeEventArgs
* UIKeyboardEventArgs
* UIMouseEventArgs

Wyrażenia lambda może również służyć:

```cshtml
<button @onclick="@(e => Console.WriteLine("Hello, world!"))">Say hello</button>
```

Często jest to wygodne zamknąć dodatkowe wartości, takich jak podczas iteracji w zestawie elementów. Poniższy przykład tworzy trzy przyciski wszystkich, która wywołuje metodę `UpdateHeading` przekazywaniem argumentu zdarzenia (`UIMouseEventArgs`) i jego numer przycisku (`buttonNumber`) w przypadku wybrania w interfejsie użytkownika:

```cshtml
<h2>@message</h2>

@for (var i = 1; i < 4; i++)
{
    var buttonNumber = i;

    <button class="btn btn-primary"
            @onclick="@(e => UpdateHeading(e, buttonNumber))">
        Button #@i
    </button>
}

@code {
    private string message = "Select a button to learn its position.";

    private void UpdateHeading(UIMouseEventArgs e, int buttonNumber)
    {
        message = $"You selected Button #{buttonNumber} at " +
            $"mouse position: {e.ClientX} X {e.ClientY}.";
    }
}
```

> [!NOTE]
> Czy **nie** zmienna pętli (`i`) w `for` pętli bezpośrednio w wyrażeniu lambda. W przeciwnym razie tę samą zmienną jest używany przez wszystkie wyrażenia lambda, powodując `i`przez wartość, która ma być taka sama we wszystkich wyrażenia lambda. Zawsze odzwierciedla jej wartość w zmiennej lokalnej (`buttonNumber` w powyższym przykładzie), a następnie użyj go.

### <a name="eventcallback"></a>EventCallback

Typowy scenariusz w przypadku zagnieżdżonych składników jest wymaganą do uruchomienia elementu nadrzędnego metodę składnika, gdy wystąpi zdarzenie składnika podrzędnego&mdash;na przykład, gdy `onclick` wystąpi zdarzenie w podrzędnym. Aby udostępnić zdarzenia dotyczące składników, należy użyć `EventCallback`. Składnik nadrzędny można przypisać metodę wywołania zwrotnego do składnika podrzędnego `EventCallback`.

Składnik podrzędnych Przykładowa aplikacja pokazuje, jak przycisk `onclick` program obsługi jest skonfigurowany do otrzymywać `EventCallback` delegatem przykładowy składnik nadrzędny. `EventCallback` Jest wypełniana `UIMouseEventArgs`, która jest odpowiednia dla `onclick` zdarzeń za pomocą urządzenia peryferyjne:

[!code-cshtml[](common/samples/3.x/BlazorSample/Components/ChildComponent.razor?highlight=5-7,17-18)]

Składnik nadrzędny ustawia elementu podrzędnego `EventCallback<T>` do jego `ShowMessage` metody:

[!code-cshtml[](common/samples/3.x/BlazorSample/Pages/ParentComponent.razor?name=snippet_ParentComponent&highlight=6,16-19)]

Po wybraniu przycisku w składniku podrzędne:

* Składnik nadrzędny `ShowMessage` metoda jest wywoływana. `messageText` są aktualizowane i wyświetlane w składniku nadrzędnym.
* Wywołanie `StateHasChanged` nie jest wymagane w przypadku metody wywołania zwrotnego (`ShowMessage`). `StateHasChanged` wywoływana jest automatycznie rerender składnik nadrzędny tak samo, jak zdarzenia podrzędne wyzwolić składnika rerendering w procedurze obsługi zdarzeń, które są wykonywane w ramach elementu podrzędnego.

`EventCallback` i `EventCallback<T>` zezwolić delegatów asynchronicznych. `EventCallback<T>` Zdecydowanie jest wpisane i wymaga określonych argumentów. `EventCallback` słabo został wpisany oraz umożliwia dowolny typ argumentu.

```cshtml
<p><b>@messageText</b></p>

@{ var message = "Default Text"; }

<ChildComponent 
    OnClick="@(async () => { await Task.Yield(); messageText = "Blaze It!"; })" />

@code {
    private string messageText;
}
```

Wywoływanie `EventCallback` lub `EventCallback<T>` z `InvokeAsync` i await <xref:System.Threading.Tasks.Task>:

```csharp
await callback.InvokeAsync(arg);
```

Użyj `EventCallback` i `EventCallback<T>` dla zdarzenia, obsługi i wiązania parametrów na części.

Preferuj silnie typizowaną `EventCallback<T>` za pośrednictwem `EventCallback`. `EventCallback<T>` zapewniają lepszy błąd dla użytkowników składnika. Podobnie jak inne procedury obsługi zdarzeń interfejsu użytkownika, określając parametr zdarzeń jest opcjonalny. Użyj `EventCallback` po żadnej wartości przekazanej do wywołania zwrotnego.

## <a name="capture-references-to-components"></a>Przechwytywanie odwołania do składników

Odwołania do składników zapewnia sposób odwołania wystąpienie składnika, dzięki czemu można wysyłać polecenia do tego wystąpienia, takie jak `Show` lub `Reset`. Do przechwycenia odwołania do składników, należy dodać `@ref` atrybutu do elementu podrzędnego, a następnie zdefiniować pole o tej samej nazwie i typie jako element podrzędny.

```cshtml
<MyLoginDialog @ref="loginDialog" ... />

@code {
    private MyLoginDialog loginDialog;

    private void OnSomething()
    {
        loginDialog.Show();
    }
}
```

Po wyrenderowaniu składnika `loginDialog` pole jest wypełniane `MyLoginDialog` wystąpienie składnika podrzędnego. Następnie można wywoływać metod .NET w instancji składnika.

> [!IMPORTANT]
> `loginDialog` Zmiennej tylko jest wypełniana po składnik jest renderowana i jego dane wyjściowe obejmują `MyLoginDialog` elementu. Do tego momentu nie ma nic do odwołania. Do manipulowania odwołania do składników, po zakończeniu renderowania składnik, należy użyć `OnAfterRenderAsync` lub `OnAfterRender` metody.

Podczas przechwytywania odwołania do składników użyj podobnej składni do [przechwytywania odwołania do elementu](xref:blazor/javascript-interop#capture-references-to-elements), nie jest [międzyoperacyjnego JavaScript](xref:blazor/javascript-interop) funkcji. Odwołania do składników nie są przekazywane do kodu w języku JavaScript&mdash;są używane tylko w kodzie .NET.

> [!NOTE]
> Czy **nie** umożliwia odwołania do składników mutować stan składnikach podrzędnych. Zamiast tego należy użyć normalnego parametry deklaratywne, aby przekazać dane do elementów podrzędnych. To sprawia, że podrzędnych automatycznie rerender w właściwym czasie.

## <a name="use-key-to-control-the-preservation-of-elements-and-components"></a>Użyj @key do kontrolowania zachowania elementów i składniki

Podczas renderowania listę elementów lub składniki i elementy lub składniki później zmienić, algorytm porównywanie Blazor firmy należy zdecydować, które z poprzednich elementów lub składniki mogą być zachowywane przez i jak obiekty modelu powinny być mapowane do nich. Zwykle ten proces odbywa się automatycznie i można zignorować, ale istnieją przypadki, gdzie możesz chcieć kontrolować ten proces.

Rozważmy następujący przykład:

```csharp
@foreach (var person in People)
{
    <DetailsEditor Details="@person.Details" />
}

@code {
    [Parameter]
    private IEnumerable<Person> People { get; set; }
}
```

Zawartość `People` kolekcji mogą ulec zmianie z wstawiania, usunięty lub nowo porządkować wpisów. Gdy składnik rerenders, `<DetailsEditor>` składnika mogą ulec zmianie, aby otrzymać inny `Details` wartości parametrów. Może to spowodować rerendering bardziej skomplikowane niż oczekiwano. W niektórych przypadkach rerendering może prowadzić do widocznych różnicami, takie jak element utracone fokus.

Proces mapowania mogą być kontrolowane za pomocą `@key` atrybutu dyrektywy. `@key` powoduje, że algorytm porównywanie zagwarantowanie zachowywania elementów lub składników opartych na wartość klucza:

```csharp
@foreach (var person in People)
{
    <DetailsEditor @key="@person" Details="@person.Details" />
}

@code {
    [Parameter]
    private IEnumerable<Person> People { get; set; }
}
```

Gdy `People` zmiany kolekcji algorytm porównywanie zachowuje skojarzenie między `<DetailsEditor>` wystąpień i `person` wystąpień:

* Jeśli `Person` są usuwane z `People` listy, tylko odpowiednie `<DetailsEditor>` wystąpienie zostanie usunięte z interfejsu użytkownika. Pozostałe wystąpienia są pozostawione bez zmian.
* Jeśli `Person` zostanie wstawione w niektórych pozycji na liście, jeden nowy `<DetailsEditor>` wystąpienia zostanie wstawione w tym w odpowiednim miejscu. Pozostałe wystąpienia są pozostawione bez zmian.
* Jeśli `Person` wpisy są reorganizowane, odpowiedni `<DetailsEditor>` wystąpienia są zachowywane i nowo porządkować w interfejsie użytkownika.

W niektórych przypadkach użycie `@key` zmniejsza złożoność rerendering i pozwala uniknąć potencjalnych problemów za pomocą stanowe części DOM zmiany, takie jak pozycja fokus.

> [!IMPORTANT]
> Klucze są lokalne dla każdego elementu kontenera lub składnika. Klucze są *nie* porównane globalnie w dokumencie.

### <a name="when-to-use-key"></a>Kiedy należy używać @key

Zazwyczaj warto użyć `@key` zawsze, gdy lista jest renderowany (na przykład w `@foreach` bloku) oraz odpowiednią wartość istnieje w celu definiowania `@key`.

Można również użyć `@key` aby zapobiec Blazor zachowywanie poddrzewo elementu lub składnika, gdy ulegnie zmianie obiektu:

```cshtml
<div @key="@currentPerson">
    ... content that depends on @currentPerson ...
</div>
```

Jeśli `@currentPerson` zmian `@key` atrybutu dyrektywy wymusza Blazor można odrzucić cały `<div>` i jego elementy podrzędne, jak i ponownej kompilacji poddrzewo w Interfejsie użytkownika za pomocą nowych elementów i składników. Może to być przydatne, jeśli potrzebujesz gwarantuje nie stan interfejsu użytkownika są zachowywane podczas `@currentPerson` zmiany.

### <a name="when-not-to-use-key"></a>Kiedy nie należy używać @key

Występuje po negatywnie na wydajność, porównywanie za pomocą `@key`. Spadek wydajności nie jest duża, ale tylko określić `@key` Jeżeli kontrolowanie zachowania element lub składnika reguły korzystania z aplikacji.

Nawet wtedy, gdy `@key` nie jest używany, Blazor zachowuje wystąpienia elementu, jak i składnika podrzędnego możliwie. Tylko zaletą używania `@key` jest kontrola nad *jak* wystąpień modelu są mapowane na wystąpieniach zachowanych składnika, zamiast algorytm porównywanie, wybierając mapowania.

### <a name="what-values-to-use-for-key"></a>Wartości, których można użyć dla @key

Ogólnie rzecz biorąc, dobrym pomysłem będzie podać jedną z następujących rodzajów wartość `@key`:

* Model wystąpień obiektu (na przykład `Person` wystąpienia, tak jak w poprzednim przykładzie). Gwarantuje to zachowanie oparte na równość odwołań obiektu.
* Unikatowe identyfikatory (na przykład wartości klucza podstawowego typu `int`, `string`, lub `Guid`).

Należy unikać podawania wartość, która może zostać nieoczekiwanie kolidują. Jeśli `@key="@someObject.GetHashCode()"` jest podany, mogą wystąpić nieoczekiwane kolizji kody skrótów niepowiązanych obiektów mogą być takie same. W przypadku konfliktu `@key` wartości są żądane w ramach tego samego nadrzędnego `@key` wartości nie będą uznawane.

## <a name="lifecycle-methods"></a>Cykl życia metody

`OnInitAsync` i `OnInit` wykonanie kodu, aby zainicjować składnika. Aby wykonać operację asynchroniczną, użyj `OnInitAsync` i `await` — słowo kluczowe o nieudanej operacji:

```csharp
protected override async Task OnInitAsync()
{
    await ...
}
```

Operacja synchroniczna, można użyć `OnInit`:

```csharp
protected override void OnInit()
{
    ...
}
```

`OnParametersSetAsync` i `OnParametersSet` są wywoływane, gdy składnik, który otrzyma parametry od jego elementu nadrzędnego, a wartości są przypisywane do właściwości. Te metody są wykonywane po zainicjowaniu składnika i każdym składniku jest renderowany:

```csharp
protected override async Task OnParametersSetAsync()
{
    await ...
}
```

```csharp
protected override void OnParametersSet()
{
    ...
}
```

`OnAfterRenderAsync` i `OnAfterRender` są wywoływane po zakończeniu renderowania składnika. Odwołania do elementu, jak i składnika zostaną wypełnione na tym etapie. Aby wykonać kroki dodatkowe inicjowanie przy użyciu renderowanej zawartości, takich jak aktywacja bibliotek JavaScript innych firm, które działają na renderowanych elementów DOM, należy użyć tego etapu.

```csharp
protected override async Task OnAfterRenderAsync()
{
    await ...
}
```

```csharp
protected override void OnAfterRender()
{
    ...
}
```

`SetParameters` może zostać zastąpiona w celu wykonania kodu, zanim parametry są ustawione:

```csharp
public override void SetParameters(ParameterCollection parameters)
{
    ...

    base.SetParameters(parameters);
}
```

Jeśli `base.SetParameters` nie jest wywoływana, niestandardowy kod może interpretować przychodzących wartości parametrów w sposób wymagane. Na przykład przychodzące parametry nie są wymagane do przypisania do właściwości w klasie.

`ShouldRender` może zostać zastąpiona w celu pomijania odświeżanie interfejsu użytkownika. Jeśli implementacja zwraca `true`, interfejs użytkownika są odświeżane. Nawet wtedy, gdy `ShouldRender` jest została zastąpiona, składnik jest zawsze wstępnie renderowane.

```csharp
protected override bool ShouldRender()
{
    var renderUI = true;

    return renderUI;
}
```

## <a name="component-disposal-with-idisposable"></a>Usuwanie składnika z interfejsu IDisposable

Jeśli składnik implementuje <xref:System.IDisposable>, [Dispose — metoda](/dotnet/standard/garbage-collection/implementing-dispose) jest wywoływana, gdy składnik zostanie usunięty z interfejsu użytkownika. Używa następującego składnika `@implements IDisposable` i `Dispose` metody:

```csharp
@using System
@implements IDisposable

...

@code {
    public void Dispose()
    {
        ...
    }
}
```

## <a name="routing"></a>Routing

Routing w Blazor odbywa się, podając szablonu trasy na każdej części dostępne w aplikacji.

Gdy plik Razor za pomocą `@page` dyrektywa jest kompilowany, wygenerowana klasa otrzymuje <xref:Microsoft.AspNetCore.Mvc.RouteAttribute> Określanie szablonu trasy. W czasie wykonywania, router szuka klasy składników za pomocą `RouteAttribute` i renderuje, niezależnie od składnik ma szablon trasy, który pasuje do żądanego adresu URL.

Wiele szablonów tras można zastosować do składnika. Następujący składnik, który będzie odpowiadał na żądania `/BlazorRoute` i `/DifferentBlazorRoute`:

[!code-cshtml[](common/samples/3.x/BlazorSample/Pages/BlazorRoute.razor?name=snippet_BlazorRoute)]

## <a name="route-parameters"></a>Parametry trasy

Składniki mogą odbierać parametrów trasy z szablonu trasy dostępnego w `@page` dyrektywy. Router używa parametrów trasy do wypełnienia odpowiednich parametrów składnika.

*Składnik parametru trasy*:

[!code-cshtml[](common/samples/3.x/BlazorSample/Pages/RouteParameter.razor?name=snippet_RouteParameter)]

Opcjonalne parametry nie są obsługiwane, dlatego dwie `@page` dyrektywy są stosowane w powyższym przykładzie. Pierwszy pozwala nawigacji do składnika bez parametrów. Drugi `@page` przyjmuje dyrektywy `{text}` kierowanie parametru, a następnie przypisuje wartość do `Text` właściwości.

## <a name="base-class-inheritance-for-a-code-behind-experience"></a>Dziedziczenie klasy bazowej dla środowiska "związane z kodem"

Pliki składników mieszać kod znaczników HTML i C# przetwarzania kodu w tym samym pliku. `@inherits` Dyrektywy może służyć do zapewnienia Blazor aplikacji w środowisku "związane z kodem" oddzielający składnika znaczników, od przetwarzania kodu.

[Przykładową aplikację](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/blazor/common/samples/) pokazuje, jak składnik może dziedziczyć klasy bazowej, `BlazorRocksBase`w celu zapewnienia składnika właściwości i metody.

*Składnik Blazor Rocks*:

[!code-cshtml[](common/samples/3.x/BlazorSample/Pages/BlazorRocks.razor?name=snippet_BlazorRocks)]

*BlazorRocksBase.cs*:

[!code-csharp[](common/samples/3.x/BlazorSample/Pages/BlazorRocksBase.cs)]

Klasa bazowa powinien pochodzić od `ComponentBase`.

## <a name="import-components"></a>Importowanie elementów

Przestrzeń nazw składnika utworzone przy użyciu Razor opiera się na:

* Projekt `RootNamespace`.
* Ścieżka z katalogu głównego projektu do składnika. Na przykład `ComponentsSample/Pages/Index.razor` znajduje się w przestrzeni nazw `ComponentsSample.Pages`. Postępuj zgodnie z składniki C# nazwy zasad powiązania. W przypadku właściwości *Index.razor*, wszystkie składniki w tym samym folderze *stron*i folder nadrzędny *ComponentsSample*, znajdują się w zakresie.

Składniki zdefiniowane w innej przestrzeni nazw może być wprowadzana do zakresu przy użyciu firmy Razor [ \@przy użyciu](xref:mvc/views/razor#using) dyrektywy.

Jeśli inny składnik `NavMenu.razor`, istnieje w folderze `ComponentsSample/Shared/`, składnika mogą być używane w `Index.razor` następującym `@using` instrukcji:

```cshtml
@using ComponentsSample.Shared

This is the Index page.

<NavMenu></NavMenu>
```

Składniki mogą się też odwoływać przy użyciu ich w pełni kwalifikowanych nazw, która eliminuje potrzebę [ \@przy użyciu](xref:mvc/views/razor#using) dyrektywy:

```cshtml
This is the Index page.

<ComponentsSample.Shared.NavMenu></ComponentsSample.Shared.NavMenu>
```

> [!NOTE]
> `global::` Kwalifikacji nie jest obsługiwane.
>
> Importowanie elementów z aliasem `using` instrukcji (na przykład `@using Foo = Bar`) nie jest obsługiwane.
>
> Częściowo kwalifikowane nazwy nie są obsługiwane. Na przykład dodanie `@using ComponentsSample` i odwoływanie się do `NavMenu.razor` z `<Shared.NavMenu></Shared.NavMenu>` nie jest obsługiwane.

## <a name="razor-support"></a>Obsługa razor

**Dyrektywy razor**

W poniższej tabeli przedstawiono dyrektywy razor.

| — Dyrektywa | Opis |
| --------- | ----------- |
| [\@Kod](xref:mvc/views/razor#section-5) | Dodaje C# blok kodu do składnika. `@code` jest aliasem `@functions`. `@code` Zaleca się za pośrednictwem `@functions`. Więcej niż jeden `@code` bloku jest dozwolone. |
| [\@Funkcje](xref:mvc/views/razor#section-5) | Dodaje C# blok kodu do składnika. Wybierz `@code` za pośrednictwem `@functions` dla C# bloków kodu. |
| `@implements` | Implementuje interfejs dla klasy wygenerowanej składnika. |
| [\@Inherits](xref:mvc/views/razor#section-3) | Zapewnia pełną kontrolę nad klasę, która dziedziczy składnika. |
| [\@wstrzykiwanie](xref:mvc/views/razor#section-4) | Włącza usługi iniekcji z [kontener usługi](xref:fundamentals/dependency-injection). Aby uzyskać więcej informacji, zobacz [wstrzykiwanie zależności do widoków](xref:mvc/views/dependency-injection). |
| `@layout` | Określa składnik układu. Składniki układu są używane, aby uniknąć zduplikowania kodu i niespójności. |
| [\@page](xref:razor-pages/index#razor-pages) | Określa, że składnik obsługi żądań bezpośrednio. `@page` Dyrektywy można określić za pomocą trasy i opcjonalnych parametrów. W przeciwieństwie do stron Razor `@page` dyrektywy nie musi być pierwszą dyrektywę w górnej części pliku. Aby uzyskać więcej informacji, zobacz [Routing](xref:blazor/routing). |
| [\@za pomocą](xref:mvc/views/razor#using) | Dodaje C# `using` dyrektywy do klasy wygenerowanej składnika. Udostępniono też wszystkich składników, które są zdefiniowane w tej przestrzeni nazw do zakresu. |
| [\@Namespace](xref:mvc/views/razor#section-6) | Ustawia obszar nazw, klasy wygenerowanej składnika. |
| [\@Atrybut](xref:mvc/views/razor#section-7) | Dodaje atrybut do klasy wygenerowanej składnika. |

**Warunkowe atrybutów elementów HTML**

Atrybutów elementów HTML warunkowo są renderowane w oparciu o wartość .NET. Jeśli wartość jest `false` lub `null`, ten atrybut nie jest renderowany. Jeśli wartość jest `true`, ten atrybut jest renderowany zminimalizowane.

W poniższym przykładzie `IsCompleted` Określa, czy `checked` jest renderowany w znaczniku elementu:

```cshtml
<input type="checkbox" checked="@IsCompleted" />

@code {
    [Parameter]
    private bool IsCompleted { get; set; }
}
```

Jeśli `IsCompleted` jest `true`, pole jest renderowane jako:

```html
<input type="checkbox" checked />
```

Jeśli `IsCompleted` jest `false`, pole jest renderowane jako:

```html
<input type="checkbox" />
```

**Dodatkowe informacje na temat Razor**

Aby uzyskać więcej informacji na temat Razor, zobacz [dokumentacja składni Razor](xref:mvc/views/razor).

## <a name="raw-html"></a>Surowy kod HTML

Ciągi są zwykle renderowane przy użyciu modelu DOM węzły tekstowe, co oznacza, że żadnych znaczników, które mogą zawierać jest ignorowany i traktowany jako literał tekstowy. Aby renderować kod HTML, opakowywanie zawartości HTML w `MarkupString` wartość. Wartość jest analizowany jako HTML lub SVG i wstawiony DOM.

> [!WARNING]
> Renderowanie surowe HTML skonstruowany z dowolnego niezaufanych źródło jest **zagrożenie dla bezpieczeństwa** i należy ich unikać!

Poniższy przykład pokazuje użycie `MarkupString` typu do dodania bloku zawartości statycznej HTML do wyniku renderowania składnika:

```html
@((MarkupString)myMarkup)

@code {
    private string myMarkup = 
        "<p class='markup'>This is a <em>markup string</em>.</p>";
}
```

## <a name="templated-components"></a>Składniki oparte na szablonach

Oparte na szablonach składniki są składniki, które akceptują jeden lub więcej szablonów interfejsu użytkownika jako parametry, które następnie mogą być używane jako część logiki renderowania składnika. Oparte na szablonach składniki umożliwiają tworzenie składników wyższego poziomu, które są bardziej wielokrotnego użytku, niż regularne składników. Zawiera kilka przykładów:

* Składnik tabeli, który umożliwia użytkownikowi określenie szablonów dla nagłówka tabeli, wierszy i stopki.
* Składnik listy, który umożliwia użytkownikowi określić szablon służący do renderowania elementów na liście.

### <a name="template-parameters"></a>Parametry szablonu

Oparte na szablonach składnika jest zdefiniowany, określając jeden lub więcej parametrów składnika typu `RenderFragment` lub `RenderFragment<T>`. Fragment renderowania reprezentuje segment interfejsu użytkownika, który jest renderowany przez składnik. Fragment renderowania opcjonalnie przyjmuje parametr, który można określić, gdy jest wywoływany fragmentu renderowania.

*Części szablonu tabeli*:

[!code-cshtml[](common/samples/3.x/BlazorSample/Components/TableTemplate.razor)]

Podczas korzystania z szablonem składnik, można określić parametry szablonu przy użyciu elementy podrzędne, które pasują do nazw parametrów (`TableHeader` i `RowTemplate` w poniższym przykładzie):

```cshtml
<TableTemplate Items="@pets">
    <TableHeader>
        <th>ID</th>
        <th>Name</th>
    </TableHeader>
    <RowTemplate>
        <td>@context.PetId</td>
        <td>@context.Name</td>
    </RowTemplate>
</TableTemplate>
```

### <a name="template-context-parameters"></a>Parametry kontekstu szablonu

Składnik argumentów typu `RenderFragment<T>` przekazany jako elementy mają niejawny parametr o nazwie `context` (na przykład w poprzednim przykładzie kodu `@context.PetId`), można jednak zmienić przy użyciu nazwy parametru `Context` atrybutu w elemencie podrzędnym element. W poniższym przykładzie `RowTemplate` elementu `Context` Określa atrybut `pet` parametru:

```cshtml
<TableTemplate Items="@pets">
    <TableHeader>
        <th>ID</th>
        <th>Name</th>
    </TableHeader>
    <RowTemplate Context="pet">
        <td>@pet.PetId</td>
        <td>@pet.Name</td>
    </RowTemplate>
</TableTemplate>
```

Alternatywnie, można określić `Context` atrybutu w elemencie składnika. Określony `Context` atrybut ma zastosowanie do wszystkich parametrów określonego szablonu. Może to być przydatne, jeśli chcesz określić nazwę zawartości parametru zawartość elementu podrzędnego niejawne (bez żadnych zawijania elementu podrzędnego). W poniższym przykładzie `Context` atrybutu jest wyświetlany na `TableTemplate` elementu i ma zastosowanie do wszystkich parametrów szablonu:

```cshtml
<TableTemplate Items="@pets" Context="pet">
    <TableHeader>
        <th>ID</th>
        <th>Name</th>
    </TableHeader>
    <RowTemplate>
        <td>@pet.PetId</td>
        <td>@pet.Name</td>
    </RowTemplate>
</TableTemplate>
```

### <a name="generic-typed-components"></a>Składniki z kontrolą typów ogólnych

Oparte na szablonach składniki są często objęte wpisane. Na przykład ogólnego składnika szablon widoku listy może zostać użyty do renderowania `IEnumerable<T>` wartości. Aby zdefiniować element ogólny, należy użyć `@typeparam` dyrektywy, aby określić parametry typu.

*Części szablonu ListView*:

[!code-cshtml[](common/samples/3.x/BlazorSample/Components/ListViewTemplate.razor)]

Podczas korzystania z kontrolą typów ogólnych składników, jeśli jest to możliwe jest wnioskowany parametr typu:

```cshtml
<ListViewTemplate Items="@pets">
    <ItemTemplate Context="pet">
        <li>@pet.Name</li>
    </ItemTemplate>
</ListViewTemplate>
```

W przeciwnym razie parametru typu muszą być jawnie określone za pomocą atrybutu, który odpowiada nazwie parametru typu. W poniższym przykładzie `TItem="Pet"` Określa typ:

```cshtml
<ListViewTemplate Items="@pets" TItem="Pet">
    <ItemTemplate Context="pet">
        <li>@pet.Name</li>
    </ItemTemplate>
</ListViewTemplate>
```

## <a name="cascading-values-and-parameters"></a>Kaskadowe wartości i parametry

W niektórych przypadkach jest wygodne, przepływ danych ze składnika nadrzędnego za pomocą składnika podrzędny [Parametry składnika](#component-parameters), szczególnie w przypadku kilku warstw składnika. Kaskadowe wartości i parametry rozwiązują ten problem, zapewniając wygodny sposób w celu Podaj wartość, aby wszystkie jego elementy potomne składnika nadrzędnego. Kaskadowe wartości i parametry oferują również podejście składników do zapewnienia koordynacji.

### <a name="theme-example"></a>Przykład motywu

W następującym *motyw* przykład z przykładowej aplikacji `ThemeInfo` klasa określa informacje o motywie przepływ w dół hierarchii składnika tak, aby udostępnić wszystkie przyciski w ramach danego część aplikacji, ten styl.

*UIThemeClasses/ThemeInfo.cs*:

```csharp
public class ThemeInfo
{
    public string ButtonClass { get; set; }
}
```

Składnik nadrzędny można podać wartość kaskadowych za pomocą składnika kaskadowa wartość. Składnik wartości kaskadowych otacza poddrzewo hierarchii składników i dostarcza pojedynczą wartość dla wszystkich składników w ramach tego poddrzewa.

Na przykład przykładowa aplikacja określa informacje o motywie (`ThemeInfo`) w jednej aplikacji układów jako parametr kaskadowych dla wszystkich składników, które tworzą treści układ `@Body` właściwości. `ButtonClass` jest przypisywana wartość `btn-success` w składniku układu. Dowolny składnik podrzędny mogą używać tej właściwości, za pośrednictwem `ThemeInfo` cascading obiektu.

*Kaskadowe składnik wartości parametrów układ*:

```cshtml
@inherits LayoutComponentBase
@using BlazorSample.UIThemeClasses

<div class="container-fluid">
    <div class="row">
        <div class="col-sm-3">
            <NavMenu />
        </div>
        <div class="col-sm-9">
            <CascadingValue Value="@theme">
                <div class="content px-4">
                    @Body
                </div>
            </CascadingValue>
        </div>
    </div>
</div>

@code {
    private ThemeInfo theme = new ThemeInfo { ButtonClass = "btn-success" };
}
```

Zapewnienie korzystania z wartości kaskadowych, składniki deklarować parametrów kaskadowych przy użyciu `[CascadingParameter]` atrybut lub na podstawie wartości ciągu nazwy:

```cshtml
<CascadingValue Value=@PermInfo Name="UserPermissions">...</CascadingValue>

[CascadingParameter(Name = "UserPermissions")]
private PermInfo Permissions { get; set; }
```

Powiązanie z wartością nazwy ciągu jest istotne, jeśli masz wiele kaskadowych wartości tego samego typu i potrzebujesz odróżnić je w ramach tej samej poddrzewo.

Kaskadowe wartości są powiązane kaskadowych parametry według typu.

W przykładowej aplikacji wiąże składnika kaskadowych wartości parametrów motyw `ThemeInfo` kaskadowa wartość parametru kaskadowych. Parametr służy do ustawiania klasę CSS dla jednego z przyciski wyświetlane przez składnik.

*Kaskadowe składnik wartości parametrów motyw*:

```cshtml
@page "/cascadingvaluesparameterstheme"
@layout CascadingValuesParametersLayout
@using BlazorSample.UIThemeClasses

<h1>Cascading Values & Parameters</h1>

<p>Current count: @currentCount</p>

<p>
    <button class="btn" @onclick="@IncrementCount">
        Increment Counter (Unthemed)
    </button>
</p>

<p>
    <button class="btn @ThemeInfo.ButtonClass" @onclick="@IncrementCount">
        Increment Counter (Themed)
    </button>
</p>

@code {
    private int currentCount = 0;

    [CascadingParameter] protected ThemeInfo ThemeInfo { get; set; }

    private void IncrementCount()
    {
        currentCount++;
    }
}
```

### <a name="tabset-example"></a>Przykład TabSet

Parametry kaskadowych również włączyć składników do współpracy w całej hierarchii składnika. Na przykład, należy wziąć pod uwagę następujące *TabSet* przykładu w przykładowej aplikacji.

Przykładowa aplikacja ma `ITab` interfejs, który karty implementacji:

[!code-cs[](common/samples/3.x/BlazorSample/UIInterfaces/ITab.cs)]

Składnik kaskadowych wartości parametrów TabSet używa ustawienia karty składnik, który zawiera kilka składników karty:

[!code-cshtml[](common/samples/3.x/BlazorSample/Pages/CascadingValuesParametersTabSet.razor?name=snippet_TabSet)]

Składniki karty podrzędnej jawnie nie są przekazywane jako parametry można ustawić kartę. Zamiast tego składniki karty podrzędnej są częścią zawartość elementu podrzędnego zestawu kartę. Jednak ustawienia karty nadal musi wiedzieć o poszczególnych składników karty, dzięki czemu można renderować, nagłówki i aktywną kartę. Włączyć koordynacja bez konieczności dodatkowego kodu, Ustaw kartę składnika *może zapewnić sama jako wartość kaskadowych* , następnie zostaje pobrana przez podrzędny składniki kartę.

*Składnik TabSet*:

[!code-cshtml[](common/samples/3.x/BlazorSample/Components/TabSet.razor)]

Podrzędny przechwytywania składniki kartę zawierającego ustawienia karty jako parametr kaskadowych, więc składniki kartę dodają same siebie do ustawienia karty i współrzędnych karty, które jest aktywny.

*Karta składników*:

[!code-cshtml[](common/samples/3.x/BlazorSample/Components/Tab.razor)]

## <a name="razor-templates"></a>Szablony razor

Renderowanie fragmentów można zdefiniować przy użyciu składni szablonów Razor. Szablony razor służą do definiowania fragmentu interfejsu użytkownika i założono następujący format:

```cshtml
@<tag>...</tag>
```

Poniższy przykład ilustruje sposób określania `RenderFragment` i `RenderFragment<T>` wartości.

*Składnik Szablony razor*:

```cshtml
@{
    RenderFragment template = @<p>The time is @DateTime.Now.</p>;
    RenderFragment<Pet> petTemplate = (pet) => @<p>Your pet's name is @pet.Name.</p>;
}
```

Renderowanie fragmentów zdefiniowane przy użyciu Razor szablony mogą być przekazywane jako argumenty do składników oparte na szablonach lub renderowane bezpośrednio. Na przykład szablony poprzedniego bezpośrednio są renderowane w następującym kodem Razor:

```cshtml
@template

@petTemplate(new Pet { Name = "Rex" })
```

Wyniku renderowania:

```
The time is 10/04/2018 01:26:52.

Your pet's name is Rex.
```

## <a name="manual-rendertreebuilder-logic"></a>Ręczne logiki RenderTreeBuilder

`Microsoft.AspNetCore.Components.RenderTree` udostępnia metody do manipulowania składników i elementów, w tym ręcznie w kompilowanie składników C# kodu.

> [!NOTE]
> Korzystanie z `RenderTreeBuilder` do tworzenia składników to zaawansowany scenariusz. Źle sformułowane składników (na przykład tag niezamknięty znaczników) może spowodować niezdefiniowane zachowanie.

Należy wziąć pod uwagę następujący składnik Pet szczegóły mogą być ręcznie wbudowane w innym składniku:

```cshtml
<h2>Pet Details Component</h2>

<p>@PetDetailsQuote<p>

@code
{
    [Parameter]
    string PetDetailsQuote { get; set; }
}
```

W poniższym przykładzie pętli w `CreateComponent` metoda generuje trzy składniki Pet szczegóły. Podczas wywoływania `RenderTreeBuilder` metody tworzenia składników (`OpenComponent` i `AddAttribute`), numery sekwencyjne są numery wierszy kodu źródłowego. Algorytm różnica Blazor opiera się na numery sekwencyjne distinct wierszy kodu, a nie odrębne wywołania wywołania. Podczas tworzenia składnika za pomocą `RenderTreeBuilder` metody, umieszczaj argumenty dla numerów sekwencji. **Za pomocą obliczeń lub licznika do generowania numer sekwencyjny może prowadzić do pogorszenia wydajności.** Aby uzyskać więcej informacji, zobacz [sekwencji liczb odnoszą się do kolejności cyfry i nie wykonywania linii kodu](#sequence-numbers-relate-to-code-line-numbers-and-not-execution-order) sekcji.

*Wbudowane składnika zawartość*:

```cshtml
@page "/BuiltContent"

<h1>Build a component</h1>

@CustomRender

<button type="button" @onclick="@RenderComponent">
    Create three Pet Details components
</button>

@code {
    private RenderFragment CustomRender { get; set; }
    
    private RenderFragment CreateComponent() => builder =>
    {
        for (var i = 0; i < 3; i++) 
        {
            builder.OpenComponent(0, typeof(PetDetails));
            builder.AddAttribute(1, "PetDetailsQuote", "Someone's best friend!");
            builder.CloseComponent();
        }
    };    
    
    private void RenderComponent()
    {
        CustomRender = CreateComponent();
    }
}
```

### <a name="sequence-numbers-relate-to-code-line-numbers-and-not-execution-order"></a>Numery sekwencyjne odnoszą się do kolejności cyfry i nie wykonywania wiersza kodu

Blazor `.razor` pliki są zawsze kompilowane. Jest to potencjalnie dużą zaletą dla `.razor` kroku kompilacji można się posłużyć do dodania informacji, który zwiększa wydajność aplikacji w czasie wykonywania.

Przykład klawisza ulepszeniami obejmują *sekwencji numerów*. Numery sekwencyjne wskazuje środowiska uruchomieniowego, które dane wyjściowe pochodzą które odrębne i uporządkowanych wiersze kodu. Środowisko wykonawcze używa tych informacji do wygenerowania różnic wydajne drzewa liniowo, który jest znacznie szybsze niż zazwyczaj dla algorytmu diff drzewo Ogólne.

Należy wziąć pod uwagę następujące proste `.razor` pliku:

```cshtml
@if (someFlag)
{
    <text>First</text>
}

Second
```

To kompiluje, aby podobny do poniższego:

```csharp
if (someFlag)
{
    builder.AddContent(0, "First");
}

builder.AddContent(1, "Second");
```

Kiedy ten kod jest wykonywany po raz pierwszy, jeśli `someFlag` jest `true`, otrzymuje konstruktora:

| Sequence | Typ      | Dane   |
| :------: | --------- | :----: |
| 0        | Węzeł tekstowy | pierwszy  |
| 1        | Węzeł tekstowy | Sekunda |

Teraz załóżmy, że `someFlag` staje się `false`, i możemy ponownie renderowania. Tym razem odbiera konstruktora:

| Sequence | Typ       | Dane   |
| :------: | ---------- | :----: |
| 1        | Węzeł tekstowy  | Sekunda |

Gdy środowisko uruchomieniowe wykonuje różnic, widzi, elementu w sekwencji `0` została wyjęta, co generuje następujące proste *Przeprowadź edycję skryptu*:

* Usuń pierwszy węzeł tekstowy.

#### <a name="what-goes-wrong-if-you-generate-sequence-numbers-programmatically"></a>Co się nie uda, jeśli programowo wygenerować numery sekwencyjne

Sobie wyobrazić, autorem Poniższa logika konstruktora rendertree:

```csharp
var seq = 0;

if (someFlag)
{
    builder.AddContent(seq++, "First");
}

builder.AddContent(seq++, "Second");
```

Po pierwsze dane wyjściowe będą:

| Sekwencja | Typ | Dane || :------: | --------- | :--- : | | 0 | Węzeł tekstowy | Pierwszy || 1 | Węzeł tekstowy | Drugi |

Ten wynik jest identyczne z poprzednich przypadkiem, więc Brak problemów ujemna. W drugiej renderowanie, gdy `someFlag` jest `false`, dane wyjściowe to:

| Sequence | Typ      | Dane   |
| :------: | --------- | ------ |
| 0        | Węzeł tekstowy | Sekunda |

Tym razem algorytm diff widzi, który *dwóch* nastąpiły zmiany, a algorytm generuje poniższy skrypt edycji:

* Zmień wartość pierwszy węzeł tekstowy w celu `Second`.
* Usuń drugi węzeł tekstowy.

Generowanie numery sekwencyjne utracił przydatne informacje o tym, gdzie `if/else` gałęzie i pętle znajdowały się w kodzie oryginalnym. Skutkuje to różnic **dwa razy dłużej** tak jak poprzednio.

Jest to uproszczony przykład. W przypadku bardziej realistycznego o złożone i głęboko zagnieżdżonych struktur, a w szczególności z pętli przeprowadzanie bardziej dotkliwych jest spadek wydajności. Zamiast natychmiast identyfikowanie, które bloki pętli lub gałęzi zostało wstawionych lub usunięty, algorytm różnicowego musi recurse głęboko do drzewa renderowania i zazwyczaj skompilować znacznie dłużej edycji skryptów, ponieważ jest on misinformed o tym, jak starych i nowych struktur odnoszą się do siebie nawzajem.

#### <a name="guidance-and-conclusions"></a>Wskazówki i wniosków

* Wydajność aplikacji wystąpi, jeśli numerów sekwencji są generowane dynamicznie.
* Struktura nie można utworzyć liczby sekwencji automatycznie w czasie wykonywania, ponieważ nie istnieje niezbędne informacje, o ile nie są przechwytywane w czasie kompilacji.
* Nie zapisuj długie bloki konstrukcyjne ręcznie zaimplementowane `RenderTreeBuilder` logiki. Preferuj `.razor` pliki i umożliwić kompilatorowi do czynienia z numerami sekwencji.
* Jeśli numery sekwencyjne są zapisane na stałe, algorytm diff wymaga jedynie, czy numery sekwencyjne wzrost wartości. Wartość początkowa i luki są nieistotne. Jedną z opcji uzasadnione jest wykorzystanie numer wiersza kodu jako numer sekwencyjny lub zacznij od zera i zwiększenia z nich lub setki (lub dowolnym preferowanym interwale). 
* Blazor używa numerów sekwencji, podczas gdy innych platform tworzenia interfejsu użytkownika porównywanie drzewa nie są używane. Porównywanie jest znacznie szybszy, numery sekwencyjne są używane, gdy Blazor ma tę zaletę krok kompilacji, który dotyczy numerów sekwencyjnych automatycznie dla deweloperów autorstwa `.razor` plików.
