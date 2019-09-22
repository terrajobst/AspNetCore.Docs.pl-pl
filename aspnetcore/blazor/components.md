---
title: Tworzenie i używanie składników ASP.NET Core Razor
author: guardrex
description: Dowiedz się, jak tworzyć i używać składników Razor, w tym jak powiązać z danymi, obsługiwać zdarzenia i zarządzać cyklem życia składników.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 09/21/2019
uid: blazor/components
ms.openlocfilehash: cf12be950043095b7e3e5eab897dd626021cb982
ms.sourcegitcommit: 04ce94b3c1b01d167f30eed60c1c95446dfe759d
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 09/21/2019
ms.locfileid: "71176390"
---
# <a name="create-and-use-aspnet-core-razor-components"></a>Tworzenie i używanie składników ASP.NET Core Razor

Autorzy [Luke Latham](https://github.com/guardrex) i [Daniel Roth](https://github.com/danroth27)

[Wyświetlanie lub pobieranie przykładowego kodu](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/blazor/common/samples/) ([sposobu pobierania](xref:index#how-to-download-a-sample))

Aplikacje Blazor są kompilowane przy użyciu *składników*programu. Składnik jest niezależnym fragmentem interfejsu użytkownika (UI), takim jak strona, okno dialogowe lub formularz. Składnik zawiera znaczniki HTML i logikę przetwarzania wymagane do iniekcji danych lub reagowania na zdarzenia interfejsu użytkownika. Składniki są elastyczne i lekkie. Mogą być zagnieżdżane, ponownie używane i udostępniane między projektami.

## <a name="component-classes"></a>Klasy składników

Składniki są zaimplementowane w plikach składników [Razor](xref:mvc/views/razor) ( *. Razor*) przy użyciu kombinacji C# i znaczników HTML. Składnik w Blazor jest formalnie określany jako *składnik Razor*.

Nazwa składnika musi rozpoczynać się wielką literą. Na przykład *MyCoolComponent. Razor* jest prawidłowy, a *MyCoolComponent. Razor* jest nieprawidłowy.

Interfejs użytkownika dla składnika jest definiowany przy użyciu języka HTML. Logika renderowania dynamicznego (na przykład pętle, warunkowe, wyrażenia) jest dodawana przy C# użyciu osadzonej składni o nazwie [Razor](xref:mvc/views/razor). Po skompilowaniu aplikacji logika znaczników HTML i C# renderowania jest konwertowana na klasę składnika. Nazwa wygenerowanej klasy jest zgodna z nazwą pliku.

Elementy członkowskie klasy składnika są zdefiniowane w `@code` bloku. `@code` W bloku stan składnika (właściwości, pola) jest określany przy użyciu metod obsługi zdarzeń lub definiowania innej logiki składnika. Dozwolony jest więcej `@code` niż jeden blok.

> [!NOTE]
> W poprzednich wersjach ASP.NET Core 3,0 `@functions` bloki zostały użyte do tego samego celu co `@code` bloki w składnikach Razor. `@functions`bloki kontynuują działanie w składnikach Razor, ale zalecamy używanie `@code` bloku w ASP.NET Core 3,0 wersja zapoznawcza 6 lub nowsza.

Składowe składnika mogą być używane jako część logiki renderowania składnika przy użyciu C# wyrażeń, które zaczynają `@`się od. Na przykład C# pole jest renderowane przez utworzenie prefiksu `@` do nazwy pola. Poniższy przykład szacuje i renderuje:

* `_headingFontStyle`na wartość właściwości CSS dla elementu `font-style`.
* `_headingText`do zawartości `<h1>` elementu.

```cshtml
<h1 style="font-style:@_headingFontStyle">@_headingText</h1>

@code {
    private string _headingFontStyle = "italic";
    private string _headingText = "Put on your new Blazor!";
}
```

Po pierwszym wyrenderowaniu składnika składnik generuje jego drzewo renderowania w odpowiedzi na zdarzenia. Blazor następnie porównuje nowe drzewo renderowania z poprzednią i zastosuje wszelkie modyfikacje Document Object Model przeglądarki (DOM).

Składniki są zwykłymi C# klasami i mogą być umieszczane w dowolnym miejscu w projekcie. Składniki, które generują strony sieci Web, zwykle znajdują się w folderze *strony* . Składniki niestronicowe są często umieszczane w folderze udostępnionym lub w folderze niestandardowym dodanym do projektu. Aby użyć folderu niestandardowego, należy dodać przestrzeń nazw folderu niestandardowego do składnika nadrzędnego lub do pliku *_Imports. Razor* aplikacji. Na przykład następująca przestrzeń nazw sprawia, że składniki w folderze Components są dostępne, gdy główna przestrzeń nazw `WebApplication`aplikacji to:

```cshtml
@using WebApplication.Components
```

## <a name="integrate-components-into-razor-pages-and-mvc-apps"></a>Integrowanie składników w aplikacjach Razor Pages i MVC

Używaj składników z istniejącymi aplikacjami Razor Pages i MVC. Nie ma potrzeby ponownego zapisywania istniejących stron lub widoków w celu używania składników Razor. Gdy strona lub widok są renderowane, składniki są wstępnie renderowane w tym samym czasie.

Aby renderować składnik ze strony lub widoku, użyj `RenderComponentAsync<TComponent>` metody pomocnika HTML:

```cshtml
<div id="MyComponent">
    @(await Html.RenderComponentAsync<MyComponent>(RenderMode.ServerPrerendered))
</div>
```

Podczas gdy strony i widoki mogą korzystać ze składników, wartość nie jest równa "true". Składniki nie mogą używać scenariuszy dotyczących widoków i stron, takich jak częściowe widoki i sekcje. Aby użyć logiki z widoku częściowego w składniku, należy rozłożyć logikę widoku częściowego na składnik.

Aby uzyskać więcej informacji na temat sposobu renderowania składników i zarządzania stanem składnika w aplikacjach serwera Blazor, zobacz <xref:blazor/hosting-models> artykuł.

## <a name="use-components"></a>Używanie składników

Składniki mogą zawierać inne składniki, deklarując je za pomocą składni elementu HTML. Znaczniki użycia składnika wyglądają jak tag HTML, gdzie nazwa znacznika jest typem składnika.

W powiązaniu atrybutu rozróżniana jest wielkość liter. Na przykład `@bind` prawidłowe i `@Bind` jest nieprawidłowe.

Poniższy znacznik w *indeksie. Razor* renderuje `HeadingComponent` wystąpienie:

[!code-cshtml[](common/samples/3.x/BlazorSample/Pages/Index.razor?name=snippet_HeadingComponent)]

*Składniki/HeadingComponent. Razor*:

[!code-cshtml[](common/samples/3.x/BlazorSample/Components/HeadingComponent.razor)]

Jeśli składnik zawiera element HTML z wielką literą, która nie jest zgodna z nazwą składnika, jest emitowane ostrzeżenie wskazujące, że element ma nieoczekiwaną nazwę. `@using` Dodanie instrukcji dla przestrzeni nazw składnika sprawia, że składnik jest dostępny, co spowoduje usunięcie tego ostrzeżenia.

## <a name="component-parameters"></a>Parametry składnika

Składniki mogą mieć *Parametry składnika*, które są zdefiniowane przy użyciu właściwości publicznych w klasie składnika z `[Parameter]` atrybutem. Użyj atrybutów, aby określić argumenty dla składnika w znaczniku.

*Składniki/ChildComponent. Razor*:

[!code-cshtml[](common/samples/3.x/BlazorSample/Components/ChildComponent.razor?highlight=11-12)]

W poniższym przykładzie `ParentComponent` `Title` Właściwość ustawia wartość właściwości `ChildComponent`.

*Strony/ParentComponent. Razor*:

[!code-cshtml[](common/samples/3.x/BlazorSample/Pages/ParentComponent.razor?name=snippet_ParentComponent&highlight=5-6)]

## <a name="child-content"></a>Zawartość podrzędna

Składniki mogą ustawiać zawartość innego składnika. Składnik Assigner zawiera zawartość między tagami, które określają składnik do odbioru.

W poniższym przykładzie `ChildComponent` `ChildContent` ma właściwość, która reprezentuje element `RenderFragment`, który reprezentuje segment interfejsu użytkownika do renderowania. Wartość `ChildContent` jest umieszczana w znacznikach składnika, gdzie zawartość powinna być renderowana. Wartość `ChildContent` jest odbierana ze składnika nadrzędnego i renderowany w `panel-body`panelu uruchamiania.

*Składniki/ChildComponent. Razor*:

[!code-cshtml[](common/samples/3.x/BlazorSample/Components/ChildComponent.razor?highlight=3,14-15)]

> [!NOTE]
> Właściwość otrzymująca `RenderFragment` zawartość musi być nazywana `ChildContent` Konwencją.

Poniższe elementy `ParentComponent` mogą zapewnić zawartość do `ChildComponent` renderowania przez `<ChildComponent>` umieszczenie zawartości wewnątrz tagów.

*Strony/ParentComponent. Razor*:

[!code-cshtml[](common/samples/3.x/BlazorSample/Pages/ParentComponent.razor?name=snippet_ParentComponent&highlight=7-8)]

## <a name="attribute-splatting-and-arbitrary-parameters"></a>Korzystając atrybutów i dowolne parametry

Składniki mogą przechwytywać i renderować dodatkowe atrybuty oprócz zadeklarowanych parametrów składnika. Dodatkowe atrybuty mogą być przechwytywane w słowniku, a następnie *splatted* do elementu, gdy składnik jest renderowany przy [@attributes](xref:mvc/views/razor#attributes) użyciu dyrektywy Razor. Ten scenariusz jest przydatny podczas definiowania składnika, który generuje element znaczników, który obsługuje różne dostosowania. Na przykład może być żmudnym do definiowania atrybutów oddzielnie dla `<input>` , który obsługuje wiele parametrów.

W `<input>` poniższym przykładzie pierwszy element (`id="useIndividualParams"`) używa pojedynczych parametrów składnika, podczas gdy drugi `<input>` element (`id="useAttributesDict"`) używa atrybutu korzystając:

```cshtml
<input id="useIndividualParams"
       maxlength="@Maxlength"
       placeholder="@Placeholder"
       required="@Required"
       size="@Size" />

<input id="useAttributesDict"
       @attributes="InputAttributes" />

@code {
    [Parameter]
    public string Maxlength { get; set; } = "10";

    [Parameter]
    public string Placeholder { get; set; } = "Input placeholder text";

    [Parameter]
    public string Required { get; set; } = "required";

    [Parameter]
    public string Size { get; set; } = "50";

    [Parameter]
    public Dictionary<string, object> InputAttributes { get; set; } =
        new Dictionary<string, object>()
        {
            { "maxlength", "10" },
            { "placeholder", "Input placeholder text" },
            { "required", "required" },
            { "size", "50" }
        };
}
```

Typ parametru musi być zaimplementowany `IEnumerable<KeyValuePair<string, object>>` przy użyciu kluczy ciągu. Korzystanie `IReadOnlyDictionary<string, object>` z programu jest również opcją w tym scenariuszu.

Renderowane `<input>` elementy korzystające z obu metod są identyczne:

```html
<input id="useIndividualParams"
       maxlength="10"
       placeholder="Input placeholder text"
       required="required"
       size="50">

<input id="useAttributesDict"
       maxlength="10"
       placeholder="Input placeholder text"
       required="required"
       size="50">
```

Aby zaakceptować dowolne atrybuty, zdefiniuj parametr składnika przy użyciu `[Parameter]` atrybutu `CaptureUnmatchedValues` z właściwością ustawioną na `true`:

```cshtml
@code {
    [Parameter(CaptureUnmatchedValues = true)]
    public Dictionary<string, object> InputAttributes { get; set; }
}
```

`CaptureUnmatchedValues` Właściwość on`[Parameter]` umożliwia dopasowanie parametru do wszystkich atrybutów, które nie są zgodne z żadnym innym parametrem. Składnik może definiować tylko jeden parametr z `CaptureUnmatchedValues`. Typ właściwości używany z elementem `CaptureUnmatchedValues` musi być możliwy do przypisania `Dictionary<string, object>` z klucza ciągu. `IEnumerable<KeyValuePair<string, object>>`lub `IReadOnlyDictionary<string, object>` są również opcje w tym scenariuszu.

## <a name="data-binding"></a>Powiązanie danych

Powiązanie danych zarówno ze składnikami, jak i elementami modelu dom jest [@bind](xref:mvc/views/razor#bind) realizowane przy użyciu atrybutu. Poniższy przykład wiąże `_italicsCheck` pole z zaznaczonym stanem pola wyboru:

```cshtml
<input type="checkbox" class="form-check-input" id="italicsCheck" 
    @bind="_italicsCheck" />
```

Gdy to pole wyboru jest zaznaczone i wyczyszczone, wartość właściwości jest aktualizowana `true` odpowiednio `false`do i.

To pole wyboru jest aktualizowane w interfejsie użytkownika tylko wtedy, gdy składnik jest renderowany, a nie w odpowiedzi na zmianę wartości właściwości. Ze względu na to, że składniki są renderowane po wykonaniu kodu procedury obsługi zdarzeń, aktualizacje właściwości są zwykle odzwierciedlane natychmiast w interfejsie użytkownika.

Używanie `@bind` `<input @bind="CurrentValue" />`z właściwością () jest zasadniczo równoważne z następującymi: `CurrentValue`

```cshtml
<input value="@CurrentValue"
    @onchange="@((ChangeEventArgs __e) => CurrentValue = __e.Value)" />
```

Gdy składnik jest renderowany, `value` element wejściowy pochodzi `CurrentValue` z właściwości. Gdy użytkownik wpisze w polu tekstowym, `onchange` zdarzenie jest wyzwalane, `CurrentValue` a właściwość jest ustawiona na wartość zmieniona. W rzeczywistości generowanie kodu jest nieco bardziej skomplikowane, ponieważ `@bind` obsługuje kilka przypadków, w których są wykonywane konwersje typów. W zasadzie `@bind` kojarzy bieżącą wartość wyrażenia `value` z atrybutem i obsługuje zmiany przy użyciu zarejestrowanej procedury obsługi.

`onchange` Oprócz obsługi[@bind-value:event](xref:mvc/views/razor#bind)zdarzeń ze `@bind` składnią właściwość lub pole można powiązać przy użyciu [@bind-value](xref:mvc/views/razor#bind) innych zdarzeń, `event` określając atrybut z parametrem (). Poniższy przykład wiąże się z `CurrentValue` właściwością `oninput` zdarzenia:

```cshtml
<input @bind-value="CurrentValue" @bind-value:event="oninput" />
```

W przeciwieństwie do `onchange`, które jest wyzwalane, gdy `oninput` element utraci fokus, jest uruchamiany po zmianie wartości pola tekstowego.

**Wartości niemożliwy do przeanalizowania**

Gdy użytkownik dostarczy wartość niemożliwy do przeanalizowania do elementu powiązanego z danymi, wartość niemożliwy do przeanalizowania jest automatycznie przywracana do poprzedniej wartości po wyzwoleniu zdarzenia bind.

Rozważmy następujący scenariusz:

* Element jest powiązany `int` z typem `123`z początkową wartością: `<input>`

  ```cshtml
  <input @bind="MyProperty" />

  @code {
      [Parameter]
      public int MyProperty { get; set; } = 123;
  }
  ```
* Użytkownik aktualizuje wartość elementu `123.45` na liście na stronie i zmienia fokus elementu.

W poprzednim scenariuszu wartość elementu jest przywracana `123`. Gdy wartość `123.45` zostanie odrzucona na korzyść oryginalnej `123`wartości, użytkownik rozumie, że ich wartość nie została zaakceptowana.

Domyślnie powiązanie dotyczy `onchange` zdarzenia elementu (`@bind="{PROPERTY OR FIELD}"`). Użyj `@bind-value="{PROPERTY OR FIELD}" @bind-value:event={EVENT}` , aby ustawić inne zdarzenie. W przypadku`@bind-value:event="oninput"`zdarzenia () jego wersja następuje po naciśnięciu klawisza, które wprowadza niemożliwy do przeanalizowania wartość. `oninput` Podczas określania wartości `oninput` docelowej dla zdarzenia `int`z typem związanym użytkownik nie jest w trakcie wpisywania `.` znaku. `.` Znak zostanie natychmiast usunięty, więc użytkownik otrzymuje natychmiastową opinię, że dozwolone są tylko liczby całkowite. Istnieją scenariusze, w których przywrócenie wartości `oninput` zdarzenia nie jest idealne, na przykład wtedy, gdy użytkownik powinien mieć możliwość wyczyszczenia `<input>` wartości, która nie może być przewidziana. Alternatywy obejmują:

* Nie używaj `oninput` zdarzenia. Użyj zdarzenia domyślnego `onchange` (`@bind="{PROPERTY OR FIELD}"`), gdzie nieprawidłowa wartość nie jest przywracana, dopóki element nie utraci fokusu.
* Powiąż z typem dopuszczającym wartość `int?` null `string`, taką jak lub, i podaj logikę niestandardową do obsługi nieprawidłowych wpisów.
* Użyj [składnika walidacji formularza](xref:blazor/forms-validation), takiego jak `InputNumber` lub `InputDate`. Składniki walidacji formularza mają wbudowaną obsługę zarządzania nieprawidłowymi danymi wejściowymi. Składniki walidacji formularza:
  * Zezwalaj użytkownikowi na dostarczenie nieprawidłowych danych wejściowych i odbieranie błędów walidacji w skojarzonym `EditContext`.
  * Wyświetlaj błędy walidacji w interfejsie użytkownika bez zakłócania wprowadzania dodatkowych danych przez użytkownika.

**Globalizacja**

`@bind`wartości są sformatowane do wyświetlania i analizowane przy użyciu reguł bieżącej kultury.

Dla bieżącej kultury można uzyskać dostęp z <xref:System.Globalization.CultureInfo.CurrentCulture?displayProperty=fullName> właściwości.

[CultureInfo. InvariantCulture](xref:System.Globalization.CultureInfo.InvariantCulture) jest używany dla następujących typów pól (`<input type="{TYPE}" />`):

* `date`
* `number`

Poprzednie typy pól:

* Są wyświetlane przy użyciu odpowiednich reguł formatowania opartych na przeglądarce.
* Nie może zawierać tekstu o dowolnym formacie.
* Podaj charakterystykę interakcji użytkownika w oparciu o implementację przeglądarki.

Następujące typy pól mają określone wymagania dotyczące formatowania i nie są obecnie obsługiwane przez Blazor, ponieważ nie są obsługiwane przez wszystkie główne przeglądarki:

* `datetime-local`
* `month`
* `week`

`@bind`obsługuje parametr, aby zapewnić analizę i formatowanie wartości. <xref:System.Globalization.CultureInfo?displayProperty=fullName> `@bind:culture` Określanie kultury nie jest zalecane w `date` przypadku używania typów pól i. `number` `date`i `number` ma wbudowaną obsługę Blazor, która dostarcza wymaganą kulturę.

Aby uzyskać informacje na temat sposobu ustawiania kultury użytkownika, zobacz sekcję [Lokalizacja](#localization) .

**Ciągi formatujące**

Powiązanie danych działa z <xref:System.DateTime> ciągami formatu [@bind:format](xref:mvc/views/razor#bind)przy użyciu. W tej chwili nie są dostępne inne wyrażenia formatu, takie jak formaty walutowe lub liczbowe.

```cshtml
<input @bind="StartDate" @bind:format="yyyy-MM-dd" />

@code {
    [Parameter]
    public DateTime StartDate { get; set; } = new DateTime(2020, 1, 1);
}
```

W poprzednim kodzie `<input>` typ pola (`type`) elementu jest wartością domyślną `text`. `@bind:format`jest obsługiwana w celu powiązania następujących typów .NET:

* <xref:System.DateTime?displayProperty=fullName>
* <xref:System.DateTime?displayProperty=fullName>?
* <xref:System.DateTimeOffset?displayProperty=fullName>
* <xref:System.DateTimeOffset?displayProperty=fullName>?

Ten `@bind:format` atrybut określa format daty, który ma zostać zastosowany `value` do `<input>` elementu. Format jest również używany do analizowania wartości w przypadku `onchange` wystąpienia zdarzenia.

Określanie formatu dla `date` typu pola nie jest zalecane, ponieważ Blazor ma wbudowaną obsługę formatowania dat.

**Parametry składnika**

Powiązanie rozpoznaje parametry składnika, gdzie `@bind-{property}` można powiązać wartość właściwości między składnikami.

Następujący składnik podrzędny (`ChildComponent`) `Year` ma parametr składnika i `YearChanged` wywołanie zwrotne:

```cshtml
<h2>Child Component</h2>

<p>Year: @Year</p>

@code {
    [Parameter]
    public int Year { get; set; }

    [Parameter]
    public EventCallback<int> YearChanged { get; set; }
}
```

`EventCallback<T>`wyjaśniono w sekcji [EventCallback](#eventcallback) .

Poniższy składnik nadrzędny używa `ChildComponent` i wiąże `ParentYear` parametr `Year` z elementu nadrzędnego z parametrem w składniku podrzędnym:

```cshtml
@page "/ParentComponent"

<h1>Parent Component</h1>

<p>ParentYear: @ParentYear</p>

<ChildComponent @bind-Year="ParentYear" />

<button class="btn btn-primary" @onclick="ChangeTheYear">
    Change Year to 1986
</button>

@code {
    [Parameter]
    public int ParentYear { get; set; } = 1978;

    private void ChangeTheYear()
    {
        ParentYear = 1986;
    }
}
```

Załadowanie `ParentComponent` powoduje utworzenie następującej adjustacji:

```html
<h1>Parent Component</h1>

<p>ParentYear: 1978</p>

<h2>Child Component</h2>

<p>Year: 1978</p>
```

Jeśli wartość `ParentYear` właściwości zostanie zmieniona przez wybranie przycisku `ParentComponent`w `ChildComponent` , `Year` właściwość jest aktualizowana. Nowa wartość `Year` jest renderowana w interfejsie użytkownika podczas jego `ParentComponent` renderowania:

```html
<h1>Parent Component</h1>

<p>ParentYear: 1986</p>

<h2>Child Component</h2>

<p>Year: 1986</p>
```

Parametr jest możliwy do powiązania, ponieważ ma zdarzenie towarzyszące `YearChanged` `Year` pasujące do typu parametru. `Year`

Zgodnie z Konwencją, `<ChildComponent @bind-Year="ParentYear" />` jest zasadniczo równoważne zapisowi:

```cshtml
<ChildComponent @bind-Year="ParentYear" @bind-Year:event="YearChanged" />
```

Ogólnie rzecz biorąc, właściwość może być powiązana z odpowiednią obsługą zdarzeń `@bind-property:event` przy użyciu atrybutu. Na przykład właściwość `MyProp` może być powiązana z `MyEventHandler` użyciem następujących dwóch atrybutów:

```cshtml
<MyComponent @bind-MyProp="MyValue" @bind-MyProp:event="MyEventHandler" />
```

## <a name="event-handling"></a>Obsługa zdarzeń

Składniki Razor zapewniają funkcje obsługi zdarzeń. Dla atrybutu elementu HTML o nazwie `on{event}` (na `onclick` przykład i `onsubmit`) z wartością typu delegata składniki Razor traktują wartość atrybutu jako procedurę obsługi zdarzeń. Nazwa atrybutu jest zawsze sformatowana [ @on{Event}](xref:mvc/views/razor#onevent).

Poniższy kod wywołuje metodę, `UpdateHeading` gdy przycisk zostanie wybrany w interfejsie użytkownika:

```cshtml
<button class="btn btn-primary" @onclick="UpdateHeading">
    Update heading
</button>

@code {
    private void UpdateHeading(MouseEventArgs e)
    {
        ...
    }
}
```

Poniższy kod wywołuje metodę, `CheckChanged` gdy pole wyboru zostanie zmienione w interfejsie użytkownika:

```cshtml
<input type="checkbox" class="form-check-input" @onchange="CheckChanged" />

@code {
    private void CheckChanged()
    {
        ...
    }
}
```

Procedury obsługi zdarzeń mogą również być asynchroniczne i zwracać <xref:System.Threading.Tasks.Task>. Nie ma potrzeby ręcznego wywoływania `StateHasChanged()`. Wyjątki są rejestrowane, gdy wystąpią.

W poniższym przykładzie `UpdateHeading` jest wywoływana asynchronicznie po wybraniu przycisku:

```cshtml
<button class="btn btn-primary" @onclick="UpdateHeading">
    Update heading
</button>

@code {
    private async Task UpdateHeading(MouseEventArgs e)
    {
        ...
    }
}
```

### <a name="event-argument-types"></a>Typy argumentów zdarzeń

W przypadku niektórych zdarzeń dozwolone są typy argumentów zdarzeń. Jeśli dostęp do jednego z tych typów zdarzeń nie jest konieczny, nie jest to wymagane w wywołaniu metody.

Obsługiwane [EventArgs](https://github.com/aspnet/AspNetCore/tree/release/3.0-preview9/src/Components/Web/src/Web) są przedstawione w poniższej tabeli.

| Zdarzenie | Class |
| ----- | ----- |
| Schowek        | `ClipboardEventArgs` |
| Przeciągnij             | `DragEventArgs`&ndash; iprzytrzymaj`DataTransferItem` przeciągane dane elementu. `DataTransfer` |
| Błąd            | `ErrorEventArgs` |
| Fokus            | `FocusEventArgs`Nie obejmuje obsługi dla `relatedTarget`. &ndash; |
| `<input>`stąp | `ChangeEventArgs` |
| Klawiatura         | `KeyboardEventArgs` |
| Wskaźnik            | `MouseEventArgs` |
| Wskaźnik myszy    | `PointerEventArgs` |
| Kółko myszy      | `WheelEventArgs` |
| Postęp         | `ProgressEventArgs` |
| Dotyk            | `TouchEventArgs`&ndash; reprezentujepojedynczypunktkontaktunaurządzeniuz`TouchPoint` wrażliwym dotknięciem. |

Aby uzyskać informacje o zachowaniu właściwości i obsłudze zdarzeń zdarzeń w powyższej tabeli, zobacz [EventArgs Classes in source Reference (ASPNET/AspNetCore Release/3.0-preview9)](https://github.com/aspnet/AspNetCore/tree/release/3.0-preview9/src/Components/Web/src/Web).

### <a name="lambda-expressions"></a>Wyrażenia lambda

Wyrażenia lambda mogą być również używane:

```cshtml
<button @onclick="@(e => Console.WriteLine("Hello, world!"))">Say hello</button>
```

Często wygodnie jest blisko dodatkowych wartości, na przykład podczas iteracji na zestawie elementów. Poniższy przykład tworzy trzy przyciski, z których każdy wywołuje `UpdateHeading` przekazanie argumentu zdarzenia (`MouseEventArgs`) i jego numer przycisku (`buttonNumber`), po wybraniu w interfejsie użytkownika:

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

    private void UpdateHeading(MouseEventArgs e, int buttonNumber)
    {
        message = $"You selected Button #{buttonNumber} at " +
            $"mouse position: {e.ClientX} X {e.ClientY}.";
    }
}
```

> [!NOTE]
> **Nie** używaj zmiennej Loop (`i`) w `for` pętli bezpośrednio w wyrażeniu lambda. W przeciwnym razie ta sama zmienna jest używana przez wszystkie wyrażenia `i`lambda, co sprawia, że wartość jest taka sama we wszystkich lambdach. Zawsze Przechwytuj wartość w zmiennej lokalnej (`buttonNumber` w poprzednim przykładzie), a następnie użyj jej.

### <a name="eventcallback"></a>EventCallback

Typowym scenariuszem ze składnikami zagnieżdżonymi jest uruchomienie metody składnika nadrzędnego, gdy występuje&mdash;zdarzenie składnika podrzędnego, gdy wystąpi zdarzenie w elemencie `onclick` podrzędnym. Aby uwidocznić zdarzenia między składnikami, `EventCallback`Użyj. Składnik nadrzędny może przypisać metodę wywołania zwrotnego do składnika `EventCallback`podrzędnego.

W przykładowej aplikacji pokazano, jak `onclick` program obsługi przycisku został `EventCallback` skonfigurowany tak, aby otrzymać delegata z przykładu `ParentComponent`. `ChildComponent` Typ ma wartość, która jest odpowiednia dla `onclick` zdarzenia z urządzenia peryferyjnego: `MouseEventArgs` `EventCallback`

[!code-cshtml[](common/samples/3.x/BlazorSample/Components/ChildComponent.razor?highlight=5-7,17-18)]

Ustawia element podrzędny `ShowMessage`dojegometody: `EventCallback<T>` `ParentComponent`

[!code-cshtml[](common/samples/3.x/BlazorSample/Pages/ParentComponent.razor?name=snippet_ParentComponent&highlight=6,16-19)]

Gdy przycisk zostanie wybrany w `ChildComponent`:

* `ParentComponent` Metoda`ShowMessage` jest wywoływana. `messageText`jest aktualizowany i wyświetlany w `ParentComponent`.
* Wywołanie `StateHasChanged` nie jest wymagane w metodzie wywołania zwrotnego (`ShowMessage`). `StateHasChanged`jest automatycznie wywoływana w celu odrenderowania `ParentComponent`elementu, podobnie jak zdarzenia podrzędne wyzwala ponowne renderowanie składnika w obsłudze zdarzeń, które są wykonywane w ramach elementu podrzędnego.

`EventCallback`i `EventCallback<T>` Zezwalaj na asynchroniczne Delegaty. `EventCallback<T>`jest silnie wpisana i wymaga określonego typu argumentu. `EventCallback`jest słabo wpisywany i zezwala na dowolny typ argumentu.

```cshtml
<p><b>@messageText</b></p>

@{ var message = "Default Text"; }

<ChildComponent 
    OnClick="@(async () => { await Task.Yield(); messageText = "Blaze It!"; })" />

@code {
    private string messageText;
}
```

`EventCallback<T>` Wywołaj `EventCallback` lub z`InvokeAsync` i await <xref:System.Threading.Tasks.Task>:

```csharp
await callback.InvokeAsync(arg);
```

Używaj `EventCallback` i`EventCallback<T>` dla parametrów składnika Obsługa zdarzeń i powiązania.

Preferuj silnie wpisaną `EventCallback<T>` `EventCallback`wartość. `EventCallback<T>`zapewnia lepszą opinię o błędach dla użytkowników składnika. Podobnie jak w przypadku innych programów obsługi zdarzeń interfejsu użytkownika, określenie parametru zdarzenia jest opcjonalne. Użyj `EventCallback` w przypadku braku wartości przekazywania do wywołania zwrotnego.

## <a name="chained-bind"></a>Powiązanie łańcuchowe

Typowy scenariusz polega na łańcuchu parametru powiązanego z danymi do elementu strony w danych wyjściowych składnika. Ten scenariusz jest nazywany *powiązaniem łańcuchowym* , ponieważ wiele poziomów powiązań występuje jednocześnie.

Nie można zaimplementować powiązania łańcuchowego przy użyciu `@bind` składni w elemencie strony. Program obsługi zdarzeń i wartość muszą być określone osobno. Składnik nadrzędny, jednak może używać `@bind` składni z parametrem składnika.

Następujący `PasswordField` składnik (*PasswordField. Razor*):

* Ustawia wartość `Password` elementu na właściwość. `<input>`
* Uwidacznia zmiany `Password` właściwości w składniku nadrzędnym z [EventCallback](#eventcallback).

```cshtml
Password: 

<input @oninput="OnPasswordChanged" 
       required 
       type="@(showPassword ? "text" : "password")" 
       value="@Password" />

<button class="btn btn-primary" @onclick="ToggleShowPassword">
    Show password
</button>

@code {
    private bool showPassword;

    [Parameter]
    public string Password { get; set; }

    [Parameter]
    public EventCallback<string> PasswordChanged { get; set; }

    private Task OnPasswordChanged(ChangeEventArgs e)
    {
        Password = e.Value.ToString();

        return PasswordChanged.InvokeAsync(Password);
    }

    private void ToggleShowPassword()
    {
        showPassword = !showPassword;
    }
}
```

`PasswordField` Składnik jest używany w innym składniku:

```cshtml
<PasswordField @bind-Password="password" />

@code {
    private string password;
}
```

Aby przeprowadzić sprawdzenia lub błędy pułapki dla hasła w poprzednim przykładzie:

* Utwórz pole zapasowe dla `Password` (`password` w poniższym przykładowym kodzie).
* Wykonaj testy lub błędy pułapek w `Password` metodzie ustawiającej.

Poniższy przykład przedstawia natychmiastową opinię dla użytkownika, jeśli w wartości hasła jest używana spacja:

```cshtml
Password: 

<input @oninput="OnPasswordChanged" 
       required 
       type="@(showPassword ? "text" : "password")" 
       value="@Password" />

<button class="btn btn-primary" @onclick="ToggleShowPassword">
    Show password
</button>

<span class="text-danger">@validationMessage</span>

@code {
    private bool showPassword;
    private string password;
    private string validationMessage;

    [Parameter]
    public string Password
    {
        get { return password ?? string.Empty; }
        set
        {
            if (password != value)
            {
                if (value.Contains(' '))
                {
                    validationMessage = "Spaces not allowed!";
                }
                else
                {
                    password = value;
                    validationMessage = string.Empty;
                }
            }
        }
    }

    [Parameter]
    public EventCallback<string> PasswordChanged { get; set; }

    private Task OnPasswordChanged(ChangeEventArgs e)
    {
        Password = e.Value.ToString();

        return PasswordChanged.InvokeAsync(Password);
    }

    private void ToggleShowPassword()
    {
        showPassword = !showPassword;
    }
}
```

## <a name="capture-references-to-components"></a>Przechwyć odwołania do składników

Odwołania do składników zapewniają sposób odwoływania się do wystąpienia składnika, dzięki czemu można wydać polecenia do tego wystąpienia, takie `Show` jak `Reset`lub. Aby przechwycić odwołanie do składnika:

* [@ref](xref:mvc/views/razor#ref) Dodaj atrybut do składnika podrzędnego.
* Zdefiniuj pole z tym samym typem co składnik podrzędny.

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

Gdy składnik jest renderowany, `loginDialog` pole zostanie wypełnione `MyLoginDialog` wystąpieniem składnika podrzędnego. Następnie można wywołać metody .NET w wystąpieniu składnika.

> [!IMPORTANT]
> Zmienna jest wypełniana tylko po wyrenderowaniu składnika, a jego wyjście `MyLoginDialog` zawiera element. `loginDialog` Do tego momentu nie ma niczego do odwołania. Aby manipulować odwołaniami do składników po zakończeniu renderowania składnika, użyj `OnAfterRenderAsync` metod lub. `OnAfterRender`

Podczas przechwytywania odwołań do składników użycie podobnej składni do [przechwytywania odwołań do elementów](xref:blazor/javascript-interop#capture-references-to-elements)nie jest funkcją międzyoperacyjności [języka JavaScript](xref:blazor/javascript-interop) . Odwołania do składników nie są przenoszone&mdash;do kodu JavaScript, są używane tylko w kodzie platformy .NET.

> [!NOTE]
> **Nie** należy używać odwołań do składników do mutacji stanu składników podrzędnych. Zamiast tego należy używać zwykłych parametrów deklaratywnych do przekazywania danych do składników podrzędnych. Użycie normalnych parametrów deklaratywnych powoduje, że składniki podrzędne, które automatycznie uruchamiają się w prawidłowym czasie.

## <a name="invoke-component-methods-externally-to-update-state"></a>Wywołaj metody składnika zewnętrznie, aby zaktualizować stan

Blazor używa w `SynchronizationContext` celu wymuszenia pojedynczego wątku logicznego wykonywania. W tym `SynchronizationContext`przypadku są wykonywane metody cyklu życia składnika i wszystkie wywołania zwrotne zdarzeń wywoływane przez Blazor. W przypadku zdarzenia składnik należy zaktualizować w oparciu o zdarzenie zewnętrzne, takie jak czasomierz lub inne powiadomienia, przy użyciu `InvokeAsync` metody, która będzie wysyłana do `SynchronizationContext`Blazor.

Rozważmy na przykład *usługę powiadamiania* , która może powiadomić dowolny składnik nasłuchujący zaktualizowanego stanu:

```csharp
public class NotifierService
{
    // Can be called from anywhere
    public async Task Update(string key, int value)
    {
        if (Notify != null)
        {
            await Notify.Invoke(key, value);
        }
    }

    public event Func<string, int, Task> Notify;
}
```

Użycie programu w celu zaktualizowania składnika: `NotifierService`

```cshtml
@page "/"
@inject NotifierService Notifier
@implements IDisposable

<p>Last update: @lastNotification.key = @lastNotification.value</p>

@code {
    private (string key, int value) lastNotification;

    protected override void OnInitialized()
    {
        Notifier.Notify += OnNotify;
    }

    public async Task OnNotify(string key, int value)
    {
        await InvokeAsync(() =>
        {
            lastNotification = (key, value);
            StateHasChanged();
        });
    }

    public void Dispose()
    {
        Notifier.Notify -= OnNotify;
    }
}
```

W poprzednim przykładzie `NotifierService` wywołuje `OnNotify` metodę składnika poza Blazor. `SynchronizationContext` `InvokeAsync`służy do przełączania do poprawnego kontekstu i kolejki renderowania.

## <a name="use-key-to-control-the-preservation-of-elements-and-components"></a>Użyj \@klawisza, aby kontrolować zachowywanie elementów i składników

Podczas renderowania listy elementów lub składników oraz elementów lub składników, które następnie zmieniają się, algorytm diff Blazor musi zdecydować, które z poprzednich elementów lub składników mogą być zachowywane i jak obiekty modelu powinny być mapowane na nie. Zwykle ten proces jest automatyczny i można go zignorować, ale istnieją przypadki, w których może być konieczne sterowanie procesem.

Rozważmy następujący przykład:

```csharp
@foreach (var person in People)
{
    <DetailsEditor Details="person.Details" />
}

@code {
    [Parameter]
    public IEnumerable<Person> People { get; set; }
}
```

Zawartość `People` kolekcji może ulec zmianie z wstawionymi, usuniętymi lub z ponownymi zamówieniami. Gdy składnik jest przerenderowany, `<DetailsEditor>` składnik może ulec zmianie, aby otrzymywać różne `Details` wartości parametrów. Może to spowodować bardziej złożone odwzorowanie niż oczekiwano. W niektórych przypadkach odzyskanie może prowadzić do zauważalnych różnic w zachowaniu, takich jak brak fokusu elementu.

Proces mapowania można kontrolować przy użyciu `@key` atrybutu dyrektywy. `@key`powoduje, że algorytm różnicowego gwarantuje zachowywanie elementów lub składników na podstawie wartości klucza:

```csharp
@foreach (var person in People)
{
    <DetailsEditor @key="person" Details="person.Details" />
}

@code {
    [Parameter]
    public IEnumerable<Person> People { get; set; }
}
```

`<DetailsEditor>` `person` Po zmianie `People` kolekcji algorytm różnicowy zachowuje skojarzenie między wystąpieniami i wystąpieniami:

* Jeśli element `Person` zostanie usunięty `People` z listy, tylko odpowiednie `<DetailsEditor>` wystąpienie zostanie usunięte z interfejsu użytkownika. Inne wystąpienia pozostaną bez zmian.
* Jeśli na liście `<DetailsEditor>` zostaniewstawionywpewnymmiejscu,jednonowewystąpieniezostaniewstawionew`Person` odpowiednim położeniu. Inne wystąpienia pozostaną bez zmian.
* W `Person` przypadku ponownego uporządkowania wpisów odpowiednie `<DetailsEditor>` wystąpienia są zachowywane i uporządkowane w interfejsie użytkownika.

W niektórych scenariuszach użycie programu `@key` minimalizuje złożoność operacji renderowania i pozwala uniknąć potencjalnych problemów związanych ze stanem częściowych elementów modelu dom, takich jak pozycja fokusu.

> [!IMPORTANT]
> Klucze są lokalne dla każdego elementu kontenera lub składnika. Klucze nie są porównywane globalnie w całym dokumencie.

### <a name="when-to-use-key"></a>Kiedy używać \@klucza

Zazwyczaj warto używać `@key` zawsze, gdy lista jest renderowana (na przykład `@foreach` w bloku) i odpowiednia `@key`wartość istnieje do zdefiniowania.

Można również użyć `@key` , aby uniemożliwić Blazor z zachowaniem poddrzewa elementu lub składnika, gdy zmieniany jest obiekt:

```cshtml
<div @key="currentPerson">
    ... content that depends on currentPerson ...
</div>
```

Jeśli `@currentPerson` zmiany `<div>` , dyrektywa Blazor wymusza odrzucanie całości i jego obiektów podrzędnych oraz ponowne skompilowanie poddrzewa w interfejsie użytkownika za pomocą nowych elementów i składników. `@key` Może to być przydatne, jeśli zachodzi konieczność zagwarantowania, że stan `@currentPerson` interfejsu użytkownika nie jest zachowywany w przypadku zmiany.

### <a name="when-not-to-use-key"></a>Kiedy nie używać \@klucza

W przypadku różnicowania w programie `@key`występuje koszt wydajności. Koszt wydajności nie jest duży, ale określa `@key` tylko, czy kontrolowanie reguł utrwalania elementu lub składnika przynosi korzyści dla aplikacji.

Nawet jeśli `@key` nie jest używany, Blazor zachowuje elementy podrzędne i wystąpienia składników tak dużo, jak to możliwe. Jedyną zaletą korzystania z `@key` programu jest kontrola nad sposobem, w *jaki* wystąpienia modelu są mapowane na zachowane wystąpienia składników, zamiast algorytmu różnicowego, wybierając mapowanie.

### <a name="what-values-to-use-for-key"></a>Wartości, które mają być \@używane dla klucza

Ogólnie rzecz biorąc, warto podać jeden z następujących rodzajów wartości dla `@key`:

* Wystąpienia obiektów modelu (na przykład `Person` wystąpienie takie jak w poprzednim przykładzie). Zapewnia to zachowywanie na podstawie równości odwołań do obiektów.
* Unikatowe identyfikatory (na przykład wartości klucza podstawowego typu `int`, `string`lub `Guid`).

Upewnij się, że wartości `@key` używane do nie kolidują. Jeśli w tym samym elemencie nadrzędnym zostaną wykryte wartości powodujące konflikt, Blazor zgłasza wyjątek, ponieważ nie może on w sposób jednoznaczny mapować starych elementów lub składników na nowe elementy lub składniki. Używaj tylko odrębnych wartości, takich jak wystąpienia obiektów lub wartości klucza podstawowego.

## <a name="lifecycle-methods"></a>Metody cyklu życia

`OnInitializedAsync`i `OnInitialized` wykonaj kod w celu zainicjowania składnika. Aby wykonać operację asynchroniczną, użyj `OnInitializedAsync` `await` słowa kluczowego i dla operacji:

```csharp
protected override async Task OnInitializedAsync()
{
    await ...
}
```

W przypadku operacji synchronicznych należy `OnInitialized`użyć:

```csharp
protected override void OnInitialized()
{
    ...
}
```

`OnParametersSetAsync`i `OnParametersSet` są wywoływane, gdy składnik otrzymał parametry od jego elementu nadrzędnego i wartości są przypisywane do właściwości. Te metody są wykonywane po zainicjowaniu składnika i za każdym razem, gdy składnik jest renderowany:

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

`OnAfterRenderAsync`i `OnAfterRender` są wywoływane po zakończeniu renderowania składnika. Odwołania do elementów i składników są wypełniane w tym momencie. Ten etap służy do wykonywania dodatkowych kroków inicjowania przy użyciu renderowanej zawartości, takiej jak aktywacja bibliotek języka JavaScript innych firm, które działają na renderowanych elementach DOM.

`OnAfterRender`*nie jest wywoływana podczas renderowania na serwerze.*

Parametr dla `OnAfterRenderAsync` i`OnAfterRender` jest: `firstRender`

* `true` Ustawiana przy pierwszym wywołaniu wystąpienia składnika.
* Zapewnia, że operacja inicjowania jest wykonywana tylko raz.

```csharp
protected override async Task OnAfterRenderAsync(bool firstRender)
{
    if (firstRender)
    {
        await ...
    }
}
```

```csharp
protected override void OnAfterRender(bool firstRender)
{
    if (firstRender)
    {
        ...
    }
}
```

### <a name="handle-incomplete-async-actions-at-render"></a>Obsługuj niekompletne akcje asynchroniczne podczas renderowania

Akcje asynchroniczne wykonane w zdarzeniach cyklu życia mogą nie zostać zakończone przed renderowaniem składnika. Obiekty mogą być `null` lub uzupełniane wypełniane danymi podczas wykonywania metody cyklu życia. Zapewnianie logiki renderowania w celu potwierdzenia, że obiekty są inicjowane. Renderuj zastępcze elementy interfejsu użytkownika (na przykład komunikat ładowania), gdy obiekty `null`są.

W składniku `OnInitializedAsync` szablonów Blazor został zastąpiony do asynchronicznie odbierania danych prognozy (`forecasts`). `FetchData` Gdy `forecasts` tak `null`jest, zostanie wyświetlony komunikat ładowania użytkownika. Po zakończeniu `OnInitializedAsync` zwracany przez program składnik zostanie przerenderowany ze zaktualizowanym stanem. `Task`

*Strony/FetchData. Razor*:

[!code-cshtml[](components/samples_snapshot/3.x/FetchData.razor?highlight=9)]

### <a name="execute-code-before-parameters-are-set"></a>Wykonaj kod przed ustawieniem parametrów

`SetParameters`można zastąpić, aby wykonać kod przed ustawieniem parametrów:

```csharp
public override void SetParameters(ParameterView parameters)
{
    ...

    base.SetParameters(parameters);
}
```

Jeśli `base.SetParameters` nie zostanie wywołana, kod niestandardowy może interpretować wartość parametrów przychodzących w dowolny sposób. Na przykład parametry przychodzące nie są wymagane do przypisania do właściwości w klasie.

### <a name="suppress-refreshing-of-the-ui"></a>Pomiń odświeżanie interfejsu użytkownika

`ShouldRender`można zastąpić, aby pominąć odświeżanie interfejsu użytkownika. Jeśli implementacja zwraca `true`, interfejs użytkownika zostanie odświeżony. Nawet jeśli `ShouldRender` jest zastępowany, składnik jest zawsze początkowo renderowany.

```csharp
protected override bool ShouldRender()
{
    var renderUI = true;

    return renderUI;
}
```

## <a name="component-disposal-with-idisposable"></a>Usuwanie składnika z interfejsem IDisposable

W przypadku zaimplementowania <xref:System.IDisposable>składnika [Metoda Dispose](/dotnet/standard/garbage-collection/implementing-dispose) jest wywoływana, gdy składnik zostanie usunięty z interfejsu użytkownika. Poniższy składnik używa `@implements IDisposable` `Dispose` i metody:

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

Routing w Blazor jest realizowany przez dostarczenie szablonu trasy do każdego dostępnego składnika w aplikacji.

Gdy plik Razor z `@page` dyrektywą jest kompilowany, wygenerowana Klasa ma <xref:Microsoft.AspNetCore.Mvc.RouteAttribute> określony szablon trasy. W czasie wykonywania router szuka klas składników za pomocą `RouteAttribute` i renderuje, w zależności od tego, który składnik ma szablon trasy zgodny z żądanym adresem URL.

Do składnika można zastosować wiele szablonów tras. Poniższy składnik odpowiada na żądania dla `/BlazorRoute` i: `/DifferentBlazorRoute`

[!code-cshtml[](common/samples/3.x/BlazorSample/Pages/BlazorRoute.razor?name=snippet_BlazorRoute)]

## <a name="route-parameters"></a>Parametry trasy

Składniki mogą odbierać parametry trasy z szablonu trasy dostarczonego w `@page` dyrektywie. Router używa parametrów trasy, aby wypełnić odpowiednie parametry składnika.

*Składnik parametru trasy*:

[!code-cshtml[](common/samples/3.x/BlazorSample/Pages/RouteParameter.razor?name=snippet_RouteParameter)]

Parametry opcjonalne nie są obsługiwane, więc `@page` dwie dyrektywy są stosowane w powyższym przykładzie. Pierwszy zezwala na nawigowanie do składnika bez parametru. Druga `@page` dyrektywa `Text` przyjmuje parametr Route i przypisuje wartość do właściwości. `{text}`

## <a name="base-class-inheritance-for-a-code-behind-experience"></a>Dziedziczenie klasy podstawowej dla środowiska "powiązanego z kodem"

Pliki składników mieszają znaczniki HTML C# i przetwarzają kod w tym samym pliku. `@inherits` Dyrektywa może służyć do udostępniania aplikacji Blazor z interfejsem "związanym z kodem", który oddziela oznakowanie składników od przetwarzania kodu.

[Przykładowa aplikacja](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/blazor/common/samples/) pokazuje, jak składnik może dziedziczyć klasę bazową `BlazorRocksBase`,, w celu zapewnienia właściwości i metod składnika.

*Strony/BlazorRocks. Razor*:

[!code-cshtml[](common/samples/3.x/BlazorSample/Pages/BlazorRocks.razor?name=snippet_BlazorRocks)]

*BlazorRocksBase.cs*:

[!code-csharp[](common/samples/3.x/BlazorSample/Pages/BlazorRocksBase.cs)]

Klasa bazowa powinna pochodzić od `ComponentBase`.

## <a name="import-components"></a>Importuj składniki

Przestrzeń nazw składnika utworzone przy użyciu Razor jest oparta na:

* Projekt `RootNamespace`.
* Ścieżka z katalogu głównego projektu do składnika. Na przykład, `ComponentsSample/Pages/Index.razor` znajduje się w przestrzeni `ComponentsSample.Pages`nazw. Składniki przestrzegają C# reguł powiązań nazw. W przypadku elementu *index. Razor*wszystkie składniki znajdujące się w tym samym folderze, *stronach*i folderze nadrzędnym ( *ComponentsSample*) znajdują się w zakresie.

Składniki zdefiniowane w innej przestrzeni nazw można wprowadzić do zakresu przy użyciu dyrektywy [ \@using używającej](xref:mvc/views/razor#using) Razor.

Jeśli inny składnik, `NavMenu.razor`,,, istnieje w `ComponentsSample/Shared/`folderze, składnik może być używany w `Index.razor` programie z następującą `@using` instrukcją:

```cshtml
@using ComponentsSample.Shared

This is the Index page.

<NavMenu></NavMenu>
```

Do składników można także odwoływać się za pomocą ich w pełni kwalifikowanych nazw, co [ \@](xref:mvc/views/razor#using) eliminuje konieczność stosowania dyrektywy using:

```cshtml
This is the Index page.

<ComponentsSample.Shared.NavMenu></ComponentsSample.Shared.NavMenu>
```

> [!NOTE]
> `global::` Kwalifikacja nie jest obsługiwana.
>
> Importowanie składników przy użyciu `using` instrukcji z aliasami ( `@using Foo = Bar`np.) nie jest obsługiwane.
>
> Częściowo kwalifikowane nazwy nie są obsługiwane. Na przykład dodawanie `@using ComponentsSample` i odwoływanie `NavMenu.razor` się `<Shared.NavMenu></Shared.NavMenu>` za pomocą nie jest obsługiwane.

## <a name="conditional-html-element-attributes"></a>Warunkowe atrybuty elementu HTML

Atrybuty elementu HTML są warunkowo renderowane na podstawie wartości .NET. Jeśli wartość jest `false` lub `null`, atrybut nie jest renderowany. Jeśli wartość to `true`, atrybut jest renderowany jako zminimalizowany.

W poniższym przykładzie, określa `IsCompleted` , czy `checked` jest renderowany w znacznikach elementu:

```cshtml
<input type="checkbox" checked="@IsCompleted" />

@code {
    [Parameter]
    public bool IsCompleted { get; set; }
}
```

Jeśli `IsCompleted` jest`true`, pole wyboru jest renderowane jako:

```html
<input type="checkbox" checked />
```

Jeśli `IsCompleted` jest`false`, pole wyboru jest renderowane jako:

```html
<input type="checkbox" />
```

Aby uzyskać więcej informacji, zobacz <xref:mvc/views/razor>.

> [!WARNING]
> Niektóre atrybuty HTML, takie jak [Aria](https://developer.mozilla.org/docs/Web/Accessibility/ARIA/Roles/button_role#Toggle_buttons), nie działają prawidłowo, gdy typem .NET jest `bool`. W takich przypadkach należy użyć `string` typu zamiast. `bool`

## <a name="raw-html"></a>Nieprzetworzony kod HTML

Ciągi są zwykle renderowane przy użyciu węzłów tekstowych DOM, co oznacza, że wszystkie znaczniki, które mogą zawierać, są ignorowane i traktowane jako tekst literału. Aby renderować nieprzetworzony kod HTML, zawiń zawartość HTML w `MarkupString` wartości. Wartość jest analizowana jako plik HTML lub SVG i wstawiona do modelu DOM.

> [!WARNING]
> Renderowanie nieprzetworzonego kodu HTML zbudowanego z dowolnego niezaufanego źródła stanowi **zagrożenie bezpieczeństwa** i należy je unikać!

W poniższym przykładzie pokazano, `MarkupString` jak za pomocą typu dodać blok statycznej zawartości HTML do renderowanego danych wyjściowych składnika:

```html
@((MarkupString)myMarkup)

@code {
    private string myMarkup = 
        "<p class='markup'>This is a <em>markup string</em>.</p>";
}
```

## <a name="templated-components"></a>Składniki z szablonami

Składniki z szablonami są składnikami, które akceptują jeden lub więcej szablonów interfejsu użytkownika jako parametry, które mogą być następnie używane jako część logiki renderowania składnika. Składniki z szablonami umożliwiają tworzenie składników wyższego poziomu, które są większe niż zwykłe składniki. Oto kilka przykładów:

* Składnik tabeli, który umożliwia użytkownikowi określenie szablonów dla nagłówka, wierszy i stopki tabeli.
* Składnik listy, który umożliwia użytkownikowi określenie szablonu do renderowania elementów na liście.

### <a name="template-parameters"></a>Parametry szablonu

Składnik szablonu jest definiowany przez określenie co najmniej jednego parametru składnika typu `RenderFragment` lub. `RenderFragment<T>` Fragment renderowania reprezentuje segment interfejsu użytkownika do renderowania. `RenderFragment<T>`przyjmuje parametr typu, który można określić podczas wywoływania fragmentu renderowania.

`TableTemplate`składnika

[!code-cshtml[](common/samples/3.x/BlazorSample/Components/TableTemplate.razor)]

W przypadku korzystania z składnika z szablonem parametry szablonu można określić za pomocą elementów podrzędnych, które pasują do nazw parametrów (`TableHeader` i `RowTemplate` w poniższym przykładzie):

```cshtml
<TableTemplate Items="pets">
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

Argumenty składnika `RenderFragment<T>` typu przekazane jako elementy mają niejawny parametr o `context` nazwie (na przykład z poprzedniego przykładu `@context.PetId`kodu), ale można zmienić nazwę parametru przy użyciu `Context` atrybutu w elemencie podrzędnym postaci. W poniższym przykładzie `RowTemplate` `Context` atrybut elementu określa `pet` parametr:

```cshtml
<TableTemplate Items="pets">
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

Alternatywnie można określić `Context` atrybut dla elementu składnika. Określony `Context` atrybut ma zastosowanie do wszystkich parametrów określonego szablonu. Może to być przydatne, jeśli chcesz określić nazwę parametru zawartości dla niejawnej zawartości podrzędnej (bez żadnego elementu podrzędnego otoki). W poniższym przykładzie `Context` atrybut pojawia się `TableTemplate` na elemencie i ma zastosowanie do wszystkich parametrów szablonu:

```cshtml
<TableTemplate Items="pets" Context="pet">
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

### <a name="generic-typed-components"></a>Składniki typu rodzajowego

Składniki z szablonami są często wpisywane ogólnie. Na przykład, składnik ogólny `ListViewTemplate` może służyć do renderowania `IEnumerable<T>` wartości. Aby zdefiniować składnik ogólny, użyj [@typeparam](xref:mvc/views/razor#typeparam) dyrektywy do określenia parametrów typu:

[!code-cshtml[](common/samples/3.x/BlazorSample/Components/ListViewTemplate.razor)]

W przypadku używania składników o typie ogólnym parametr typu jest wnioskowany, jeśli jest to możliwe:

```cshtml
<ListViewTemplate Items="pets">
    <ItemTemplate Context="pet">
        <li>@pet.Name</li>
    </ItemTemplate>
</ListViewTemplate>
```

W przeciwnym razie parametr typu musi być jawnie określony przy użyciu atrybutu, który jest zgodny z nazwą parametru typu. W poniższym przykładzie `TItem="Pet"` określa typ:

```cshtml
<ListViewTemplate Items="pets" TItem="Pet">
    <ItemTemplate Context="pet">
        <li>@pet.Name</li>
    </ItemTemplate>
</ListViewTemplate>
```

## <a name="cascading-values-and-parameters"></a>Wartości kaskadowe i parametry

W niektórych scenariuszach nie można przepływać danych z składnika nadrzędnego do składnika potomnego przy użyciu [parametrów składnika](#component-parameters), zwłaszcza gdy istnieje kilka warstw składników. Wartości kaskadowe i parametry rozwiązują ten problem, zapewniając wygodną metodę dla składnika nadrzędnego, aby zapewnić wartość wszystkim jej składnikom potomnym. Kaskadowe wartości i parametry również zapewniają podejście do współrzędnych składników.

### <a name="theme-example"></a>Przykład motywu

W poniższym przykładzie z przykładowej aplikacji `ThemeInfo` Klasa określa informacje o motywie, aby przetworzyć hierarchię składników w taki sposób, aby wszystkie przyciski w danej części aplikacji miały ten sam styl.

*UIThemeClasses/themeinfo wskazuje. cs*:

```csharp
public class ThemeInfo
{
    public string ButtonClass { get; set; }
}
```

Składnik nadrzędny może zapewnić kaskadową wartość przy użyciu składnika wartości kaskadowych. `CascadingValue` Składnik otacza poddrzewo hierarchii składników i dostarcza jedną wartość do wszystkich składników w tym poddrzewie.

Przykładowo aplikacja Przykładowa określa informacje o motywie`ThemeInfo`() w jednej z układów aplikacji jako parametr kaskadowy dla wszystkich składników, które tworzą treść `@Body` układu właściwości. `ButtonClass`ma przypisaną wartość `btn-success` w składniku układu. Każdy składnik podrzędny może wykorzystać tę właściwość za pomocą `ThemeInfo` obiektu kaskadowego.

`CascadingValuesParametersLayout`składnika

```cshtml
@inherits LayoutComponentBase
@using BlazorSample.UIThemeClasses

<div class="container-fluid">
    <div class="row">
        <div class="col-sm-3">
            <NavMenu />
        </div>
        <div class="col-sm-9">
            <CascadingValue Value="theme">
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

Aby korzystać z wartości kaskadowych, składniki deklarują kaskadowe parametry przy użyciu `[CascadingParameter]` atrybutu. Wartości kaskadowe są powiązane z parametrami kaskadowymi według typu.

W przykładowej aplikacji `CascadingValuesParametersTheme` składnik wiąże `ThemeInfo` wartość kaskadową z parametrem kaskadowym. Parametr służy do ustawiania klasy CSS dla jednego z przycisków wyświetlanych przez składnik.

`CascadingValuesParametersTheme`składnika

```cshtml
@page "/cascadingvaluesparameterstheme"
@layout CascadingValuesParametersLayout
@using BlazorSample.UIThemeClasses

<h1>Cascading Values & Parameters</h1>

<p>Current count: @currentCount</p>

<p>
    <button class="btn" @onclick="IncrementCount">
        Increment Counter (Unthemed)
    </button>
</p>

<p>
    <button class="btn @ThemeInfo.ButtonClass" @onclick="IncrementCount">
        Increment Counter (Themed)
    </button>
</p>

@code {
    private int currentCount = 0;

    [CascadingParameter]
    protected ThemeInfo ThemeInfo { get; set; }

    private void IncrementCount()
    {
        currentCount++;
    }
}
```

Aby przetworzyć kaskadowo wiele wartości tego samego typu w ramach tego samego poddrzewa `Name` , podaj unikatowy `CascadingValue` ciąg dla każdego składnika `CascadingParameter`i odpowiadający mu. W poniższym przykładzie dwa `CascadingValue` składniki kaskaduje różne wystąpienia o `MyCascadingType` nazwie:

```cshtml
<CascadingValue Value=@ParentCascadeParameter1 Name="CascadeParam1">
    <CascadingValue Value=@ParentCascadeParameter2 Name="CascadeParam2">
        ...
    </CascadingValue>
</CascadingValue>

@code {
    private MyCascadingType ParentCascadeParameter1;

    [Parameter]
    public MyCascadingType ParentCascadeParameter2 { get; set; }

    ...
}
```

W składniku potomnym, kaskadowe parametry odbierają swoje wartości z odpowiednich kaskadowych wartości w składniku nadrzędnym według nazwy:

```cshtml
...

@code {
    [CascadingParameter(Name = "CascadeParam1")]
    protected MyCascadingType ChildCascadeParameter1 { get; set; }
    
    [CascadingParameter(Name = "CascadeParam2")]
    protected MyCascadingType ChildCascadeParameter2 { get; set; }
}
```

### <a name="tabset-example"></a>Przykład TabSet

Parametry kaskadowe umożliwiają również współdziałanie składników w hierarchii składników. Rozważmy na przykład następujący przykład *TabSet* w aplikacji przykładowej.

Przykładowa aplikacja ma `ITab` interfejs, który implementuje karty:

[!code-csharp[](common/samples/3.x/BlazorSample/UIInterfaces/ITab.cs)]

Składnik używa składnika, który zawiera kilka `Tab` składników: `TabSet` `CascadingValuesParametersTabSet`

[!code-cshtml[](common/samples/3.x/BlazorSample/Pages/CascadingValuesParametersTabSet.razor?name=snippet_TabSet)]

Składniki podrzędne `Tab` nie są jawnie przenoszone jako parametry `TabSet`do. Zamiast tego składniki podrzędne `Tab` są częścią zawartości `TabSet`elementu podrzędnego. Jednak nadal musi wiedzieć o każdym `Tab` składniku, aby można było renderować nagłówki i aktywną kartę. `TabSet` Aby umożliwić tę koordynację bez konieczności stosowania dodatkowego kodu `TabSet` , składnik *może stanowić wartość kaskadową* , która następnie jest wybierana przez składniki potomne `Tab` .

`TabSet`składnika

[!code-cshtml[](common/samples/3.x/BlazorSample/Components/TabSet.razor)]

Składniki potomne `Tab` przechwytują zawierający `TabSet` jako parametr kaskadowy, więc `Tab` składniki dodają się do `TabSet` i koordynują, na której karcie jest aktywna.

`Tab`składnika

[!code-cshtml[](common/samples/3.x/BlazorSample/Components/Tab.razor)]

## <a name="razor-templates"></a>Szablony Razor

Fragmenty renderowania można definiować przy użyciu składni szablonu Razor. Szablony Razor są sposobem definiowania fragmentu interfejsu użytkownika i przyjmuje następujący format:

```cshtml
@<{HTML tag}>...</{HTML tag}>
```

Poniższy przykład ilustruje sposób określania `RenderFragment` i `RenderFragment<T>` wartości oraz renderowania szablonów bezpośrednio w składniku. Fragmenty renderowania mogą być również przekazane jako argumenty do [składników](#templated-components)z szablonem.

```cshtml
@timeTemplate

@petTemplate(new Pet { Name = "Rex" })

@code {
    private RenderFragment timeTemplate = @<p>The time is @DateTime.Now.</p>;
    private RenderFragment<Pet> petTemplate = 
        (pet) => @<p>Your pet's name is @pet.Name.</p>;

    private class Pet
    {
        public string Name { get; set; }
    }
}
```

Renderowane dane wyjściowe poprzedniego kodu:

```html
<p>The time is 10/04/2018 01:26:52.</p>

<p>Your pet's name is Rex.</p>
```

## <a name="manual-rendertreebuilder-logic"></a>Ręczna logika RenderTreeBuilder

`Microsoft.AspNetCore.Components.Rendering.RenderTreeBuilder`zapewnia metody manipulowania składnikami i elementami, w tym ręczne Kompilowanie C# składników w kodzie.

> [!NOTE]
> Korzystanie z `RenderTreeBuilder` programu do tworzenia składników jest zaawansowanym scenariuszem. Nieprawidłowo sformułowany składnik (na przykład niezamknięty tag znacznika) może spowodować niezdefiniowane zachowanie.

Rozważmy następujący `PetDetails` składnik, który można ręcznie utworzyć w innym składniku:

```cshtml
<h2>Pet Details Component</h2>

<p>@PetDetailsQuote</p>

@code
{
    [Parameter]
    public string PetDetailsQuote { get; set; }
}
```

W poniższym przykładzie pętla w `CreateComponent` metodzie generuje trzy `PetDetails` składniki. Podczas wywoływania `RenderTreeBuilder` metod tworzenia składników (`OpenComponent` i `AddAttribute`) numery sekwencji są numerami wierszy kodu źródłowego. Algorytm Blazor różnica polega na numerach sekwencji odpowiadających odrębnym wierszom kodu, a nie odrębnym wywoływaniu wywołań. Podczas tworzenia składnika przy użyciu `RenderTreeBuilder` metod umieszczaj argumenty dla numerów sekwencji. **Użycie obliczenia lub licznika do wygenerowania numeru sekwencji może prowadzić do niskiej wydajności.** Aby uzyskać więcej informacji, zobacz sekcję [numery sekwencji powiązane z numerami wierszy kodu i kolejnością niewykonania](#sequence-numbers-relate-to-code-line-numbers-and-not-execution-order) .

`BuiltContent`składnika

```cshtml
@page "/BuiltContent"

<h1>Build a component</h1>

@CustomRender

<button type="button" @onclick="RenderComponent">
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

> ! WYŚWIETLANIA Typy w programie `Microsoft.AspNetCore.Components.RenderTree` umożliwiają przetwarzanie *wyników* operacji renderowania. Są to wewnętrzne szczegóły implementacji platformy Blazor Framework. Te typy powinny być uznawane za *niestabilne* i mogą ulec zmianie w przyszłych wersjach.

### <a name="sequence-numbers-relate-to-code-line-numbers-and-not-execution-order"></a>Numery sekwencji odnoszą się do numerów wierszy kodu, a nie kolejności wykonywania

Pliki `.razor` Blazor są zawsze kompilowane. Jest to znakomita korzyść `.razor` , ponieważ krok kompilowania może służyć do iniekcji informacji, które zwiększają wydajność aplikacji w czasie wykonywania.

Najważniejszym przykładem tych ulepszeń są *numery sekwencji*. Numery sekwencji wskazują na środowisko uruchomieniowe, które pochodzą z różnych i uporządkowanych wierszy kodu. Środowisko uruchomieniowe używa tych informacji do generowania wydajnych różnic drzewa w czasie liniowym, które są znacznie szybsze niż zwykle jest to możliwe dla algorytmu różnicowego drzewa ogólnego.

Rozważmy następujący prosty `.razor` plik:

```cshtml
@if (someFlag)
{
    <text>First</text>
}

Second
```

Poprzedni kod kompiluje się w taki sposób, aby wyglądał następująco:

```csharp
if (someFlag)
{
    builder.AddContent(0, "First");
}

builder.AddContent(1, "Second");
```

Gdy kod jest wykonywany po raz pierwszy, jeśli `someFlag` jest `true`, Konstruktor odbiera:

| Sequence | Typ      | Dane   |
| :------: | --------- | :----: |
| 0        | Węzeł tekstu | pierwszego  |
| 1        | Węzeł tekstu | Sekunda |

Wyobraź sobie `someFlag` , `false`że zostanie ona przerenderowana, a znaczniki są renderowane ponownie. Tym razem Konstruktor odbiera:

| Sequence | Typ       | Dane   |
| :------: | ---------- | :----: |
| 1        | Węzeł tekstu  | Sekunda |

Gdy środowisko uruchomieniowe wykonuje porównanie, zobaczy, że element w sekwencji `0` został usunięty, więc generuje następujący skrypt uproszczonej *edycji*:

* Usuń pierwszy węzeł tekstu.

#### <a name="what-goes-wrong-if-you-generate-sequence-numbers-programmatically"></a>Co się stało z błędami w przypadku wygenerowania numerów sekwencyjnych

Wyobraź sobie, że została zapisana następująca logika konstruktora drzewa renderowania:

```csharp
var seq = 0;

if (someFlag)
{
    builder.AddContent(seq++, "First");
}

builder.AddContent(seq++, "Second");
```

Teraz pierwsze dane wyjściowe to:

| Sequence | Typ      | Dane   |
| :------: | --------- | :----: |
| 0        | Węzeł tekstu | pierwszego  |
| 1        | Węzeł tekstu | Sekunda |

Ten wynik jest identyczny z poprzednim przypadkiem, dlatego nie istnieją żadne negatywne problemy. `someFlag`znajduje `false` się na drugim renderingu, a dane wyjściowe:

| Sequence | Typ      | Dane   |
| :------: | --------- | ------ |
| 0        | Węzeł tekstu | Sekunda |

Tym razem algorytm diff widzi, że pojawiły się *dwie* zmiany, a algorytm generuje następujący skrypt edycji:

* Zmień wartość pierwszego węzła tekstowego na `Second`.
* Usuń drugi węzeł tekstu.

Generowanie numerów sekwencji utraciło wszystkie przydatne informacje o tym, gdzie znajdują się `if/else` gałęzie i pętle w oryginalnym kodzie. Wynikiem tego jest różnica **dwa razy** , tak długo, jak wcześniej.

Jest to prosty przykład. W bardziej realistycznych przypadkach ze złożonymi i głęboko zagnieżdżonymi strukturami, szczególnie w przypadku pętli, koszt wydajności jest bardziej poważny. Zamiast natychmiastowego identyfikowania, które bloki lub gałęzie pętli zostały wstawione lub usunięte, algorytm różnicowy musi reprezentować się w drzewach renderowania i zwykle tworzyć dużo dłużej edytowane skrypty, ponieważ nie są w nim poinformowani o sposobie starych i nowych struktur odnoszą się do siebie nawzajem.

#### <a name="guidance-and-conclusions"></a>Wskazówki i wnioski

* Wydajność aplikacji ma wpływ na to, że numery sekwencji są generowane dynamicznie.
* Struktura nie może automatycznie tworzyć własnych numerów sekwencji w czasie wykonywania, ponieważ niezbędne informacje nie istnieją, chyba że są przechwytywane w czasie kompilacji.
* Nie zapisuj długich bloków logiki wykonywanej `RenderTreeBuilder` ręcznie. Preferuj `.razor` pliki i Zezwalaj kompilatorowi na rozpatruje numery sekwencji. Jeśli `RenderTreeBuilder` nie możesz uniknąć ręcznej logiki, Podziel długie bloki kodu na mniejsze fragmenty `OpenRegion` / `CloseRegion` opakowane. Każdy region ma własne oddzielne miejsce numerów sekwencyjnych, więc można uruchomić ponownie od zera (lub dowolnego innego numeru) w każdym regionie.
* Jeśli numery sekwencji są stałee, algorytm diff wymaga tylko zwiększenia wartości sekwencji. Początkowa wartość i przerwy są nieistotne. Jedną z wiarygodnych opcji jest użycie numeru wiersza kodu jako numeru sekwencyjnego lub rozpoczęcie od zera i zwiększenie według wartości lub setek (lub dowolnego preferowanego interwału). 
* Blazor używa numerów sekwencji, podczas gdy inne struktury interfejsu użytkownika porównujące drzewa nie są używane. Różnica jest znacznie szybsza, gdy są używane numery sekwencji, a Blazor ma zalety kroku kompilacji, który zajmuje się automatycznie numerami sekwencyjnymi dla deweloperów tworzących `.razor` pliki.

## <a name="localization"></a>Lokalizacja

Aplikacje serwera Blazor są zlokalizowane przy użyciu [oprogramowania pośredniczącego](xref:fundamentals/localization#localization-middleware). Oprogramowanie pośredniczące wybiera odpowiednią kulturę dla użytkowników żądających zasobów z aplikacji.

Kulturę można ustawić przy użyciu jednej z następujących metod:

* [Plik cookie](#cookies)
* [Podaj interfejs użytkownika, aby wybrać kulturę](#provide-ui-to-choose-the-culture)

Aby uzyskać więcej informacji i przykładów, <xref:fundamentals/localization>Zobacz.

### <a name="cookies"></a>Cookie

Plik cookie kultury lokalizacji może utrzymywać kulturę użytkownika. Plik cookie jest tworzony przez `OnGet` metodę strony hosta aplikacji (*strony/host. cshtml. cs*). Oprogramowanie pośredniczące lokalizacji odczytuje plik cookie na kolejnych żądaniach, aby ustawić kulturę użytkownika. 

Użycie pliku cookie zapewnia, że połączenie z użyciem protokołu WebSocket może prawidłowo propagować kulturę. Jeśli schematy lokalizacji są oparte na ścieżce URL lub ciągu zapytania, schemat może nie być w stanie współdziałać z usługą WebSockets, więc nie będzie można zachować kultury. W związku z tym zalecanym podejściem jest użycie pliku cookie kultury lokalizacji.

Każda technika może służyć do przypisywania kultury, jeśli kultura jest utrwalona w pliku cookie lokalizacji. Jeśli aplikacja ma już ustalony schemat lokalizacji dla ASP.NET Core po stronie serwera, Kontynuuj korzystanie z istniejącej infrastruktury lokalizacji aplikacji i Ustaw plik cookie kultury lokalizacji w schemacie aplikacji.

Poniższy przykład pokazuje, jak ustawić bieżącą kulturę w pliku cookie, który może zostać odczytany przez oprogramowanie pośredniczące lokalizacji. Utwórz plik *Pages/hosta. cshtml. cs* z następującą zawartością w aplikacji Blazor Server:

```csharp
public class HostModel : PageModel
{
    public void OnGet()
    {
        HttpContext.Response.Cookies.Append(
            CookieRequestCultureProvider.DefaultCookieName,
            CookieRequestCultureProvider.MakeCookieValue(
                new RequestCulture(
                    CultureInfo.CurrentCulture,
                    CultureInfo.CurrentUICulture)));
    }
}
```

Lokalizacja jest obsługiwana w aplikacji:

1. Przeglądarka wysyła początkowe żądanie HTTP do aplikacji.
1. Kultura jest przypisana przez oprogramowanie pośredniczące lokalizacji.
1. Metoda w *_Host. cshtml. cs* utrzymuje kulturę w pliku cookie jako część odpowiedzi. `OnGet`
1. Przeglądarka otwiera połączenie WebSocket, aby utworzyć interaktywną sesję serwera Blazor.
1. Oprogramowanie pośredniczące lokalizacji odczytuje plik cookie i przypisuje kulturę.
1. Sesja serwera Blazor jest rozpoczynana z poprawną kulturą.

## <a name="provide-ui-to-choose-the-culture"></a>Podaj interfejs użytkownika, aby wybrać kulturę

Aby zapewnić interfejs użytkownika, aby umożliwić użytkownikowi wybranie kultury, zalecane jest *podejście oparte na przekierowaniu* . Ten proces jest podobny do tego, co się dzieje w aplikacji sieci Web, gdy użytkownik próbuje uzyskać&mdash;dostęp do bezpiecznego zasobu, użytkownik zostanie przekierowany do strony logowania, a następnie przekierowany z powrotem do oryginalnego zasobu. 

Aplikacja utrzymuje wybraną kulturę użytkownika za pośrednictwem przekierowania do kontrolera. Kontroler ustawia wybraną kulturę użytkownika na plik cookie i przekierowuje użytkownika z powrotem do oryginalnego identyfikatora URI.

Ustanów punkt końcowy HTTP na serwerze, aby ustawić kulturę wybraną przez użytkownika w pliku cookie i wykonać przekierowanie z powrotem do oryginalnego identyfikatora URI:

```csharp
[Route("[controller]/[action]")]
public class CultureController : Controller
{
    public IActionResult SetCulture(string culture, string redirectUri)
    {
        if (culture != null)
        {
            HttpContext.Response.Cookies.Append(
                CookieRequestCultureProvider.DefaultCookieName,
                CookieRequestCultureProvider.MakeCookieValue(
                    new RequestCulture(culture)));
        }

        return LocalRedirect(redirectUri);
    }
}
```

> [!WARNING]
> Użyj wyniku `LocalRedirect` działania, aby zapobiec atakom typu Open redirect. Aby uzyskać więcej informacji, zobacz <xref:security/preventing-open-redirects>.

Poniższy składnik przedstawia przykład sposobu wykonywania wstępnego przekierowania, gdy użytkownik wybierze kulturę:

```cshtml
@inject NavigationManager NavigationManager

<h3>Select your language</h3>

<select @onchange="OnSelected">
    <option>Select...</option>
    <option value="en-US">English</option>
    <option value="fr-FR">Français</option>
</select>

@code {
    private double textNumber;

    private void OnSelected(ChangeEventArgs e)
    {
        var culture = (string)e.Value;
        var uri = new Uri(NavigationManager.Uri())
            .GetComponents(UriComponents.PathAndQuery, UriFormat.Unescaped);
        var query = $"?culture={Uri.EscapeDataString(culture)}&" +
            $"redirectUri={Uri.EscapeDataString(uri)}";

        NavigationManager.NavigateTo("/Culture/SetCulture" + query, forceLoad: true);
    }
}
```

### <a name="use-net-localization-scenarios-in-blazor-apps"></a>Korzystanie z scenariuszy lokalizacji platformy .NET w aplikacjach Blazor

W aplikacjach Blazor dostępne są następujące scenariusze dotyczące lokalizacji i globalizacji platformy .NET:

* . System zasobów netto
* Formatowanie liczb i dat specyficznych dla kultury

`@bind` Funkcja Blazor wykonuje globalizację w oparciu o bieżącą kulturę użytkownika. Aby uzyskać więcej informacji, zobacz sekcję [powiązanie danych](#data-binding) .

Obecnie obsługiwane są ograniczone zestawy ASP.NET Core scenariuszy lokalizacji:

* `IStringLocalizer<>`*jest obsługiwany* w aplikacjach Blazor.
* `IHtmlLocalizer<>`Lokalizacje adnotacji danych są ASP.NET Core w scenariuszach MVC i nie są obsługiwane w aplikacjach Blazor. `IViewLocalizer<>`

Aby uzyskać więcej informacji, zobacz <xref:fundamentals/localization>.

## <a name="scalable-vector-graphics-svg-images"></a>Skalowalne obrazy wektorowe (SVG)

Ponieważ Blazor renderuje HTML, obrazy obsługiwane przez przeglądarkę, w tym obrazy*SVG (Scalable*Vector Graphics), są obsługiwane przez `<img>` Tag:

```html
<img alt="Example image" src="some-image.svg" />
```

Podobnie Obrazy SVG są obsługiwane w regułach CSS pliku arkusza stylów (*CSS*):

```css
.my-element {
    background-image: url("some-image.svg");
}
```

Jednak wbudowane znaczniki SVG nie są obsługiwane we wszystkich scenariuszach. Jeśli umieścisz `<svg>` tag bezpośrednio w pliku składnika ( *. Razor*), podstawowe renderowanie obrazu jest obsługiwane, ale wiele scenariuszy zaawansowanych nie jest jeszcze obsługiwanych. Na przykład `<use>` Tagi nie są obecnie przestrzegane i `@bind` nie mogą być używane z niektórymi tagami SVG. Oczekujemy, że te ograniczenia są opisane w przyszłej wersji.

## <a name="additional-resources"></a>Dodatkowe zasoby

* <xref:security/blazor/server>&ndash; Zawiera wskazówki dotyczące tworzenia aplikacji serwera Blazor, które muszą będą konkurować o z wyczerpaniem zasobów.
