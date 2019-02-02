---
title: Tworzenie i używanie składników Razor
author: guardrex
description: Informacje o sposobie tworzenia i używania składników Razor, w tym jak powiązać z danymi, obsługa zdarzeń i Zarządzaj cyklami życia składników.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 01/29/2019
uid: razor-components/components
ms.openlocfilehash: 550755d4cc2bd309846e1cc332c3bb1f0b812c25
ms.sourcegitcommit: ed76cc752966c604a795fbc56d5a71d16ded0b58
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/02/2019
ms.locfileid: "55668200"
---
# <a name="create-and-use-razor-components"></a>Tworzenie i używanie składników Razor

Przez [Luke Latham](https://github.com/guardrex), [Daniel Roth](https://github.com/danroth27), i [Morné Zaayman](https://github.com/MorneZaayman)

[!INCLUDE[](~/includes/razor-components-preview-notice.md)]

[Wyświetlanie lub pobieranie przykładowego kodu](https://github.com/aspnet/Docs/tree/master/aspnetcore/razor-components/common/samples/) ([sposobu pobierania](xref:index#how-to-download-a-sample)). Zobacz [wprowadzenie](xref:razor-components/get-started) tematu wstępnie wymaganych składników.

Razor składniki aplikacji są tworzone przy użyciu *składniki*. Składnik jest niezależna fragmentu interfejsu użytkownika (UI), takich jak strony, okno dialogowe lub formularza. Składnik zawiera znacznik HTML do renderowania wraz z logiki przetwarzania potrzebne do Wstawianie danych lub reagowania na zdarzenia interfejsu użytkownika. Składniki są elastyczne i uproszczone i może być zagnieżdżony, ponownie, a także współużytkowane między projektami.

## <a name="component-classes"></a>Klasy składników

Składniki są zazwyczaj implementowani w  *\*.cshtml* plików przy użyciu kombinacji C# i kod znaczników HTML. W interfejsie użytkownika dla składnika jest zdefiniowana za pomocą kodu HTML. Logika renderowania dynamicznego (na przykład pętli, warunkowych, wyrażeń) zostanie dodany przy użyciu osadzonych C# składni o nazwie [Razor](https://docs.microsoft.com/aspnet/core/mvc/views/razor). Podczas kompilowania aplikacji Razor składników, kod znaczników HTML i C# logiki renderowania są konwertowane na klasy składnika. Nazwa wygenerowanej klasy odpowiada nazwie pliku.

Elementy członkowskie klasy składników są zdefiniowane w `@functions` bloku (więcej niż jeden `@functions` bloku jest dozwolone). W `@functions` bloku, stan składników (właściwości, pola) została określona wraz z metody obsługi zdarzeń lub Definiowanie logiki innych składników.

Elementy członkowskie składnika mogą posłużyć jako część składnika przez renderowanie przy użyciu logiki C# wyrażeń, które zaczyna się `@`. Na przykład C# pole jest renderowane przez dodanie przedrostka `@` na nazwę pola. Poniższy przykład daje w wyniku i renderuje:

* `_headingFontStyle` wartości właściwości CSS dla `font-style`.
* `_headingText` w treści `<h1>` elementu.

```cshtml
<h1 style="font-style:@_headingFontStyle">@_headingText</h1>

@functions {
    private string _headingFontStyle = "italic";
    private string _headingText = "Put on your new Blazor!";
}
```

Po początkowo renderowania składnik składnika generuje jej drzewo renderowania w odpowiedzi na zdarzenia. Składniki razor następnie porównuje nowego drzewa renderowania względem poprzedniego i stosuje wszystkie zmiany do przeglądarki w modelu DOM (Document Object).

## <a name="using-components"></a>Używanie składników

Składniki mogą zawierać inne składniki, deklarując je przy użyciu składni elementu HTML. Znaczniki dla za pomocą składnika wygląda jak HTML tag, gdzie nazwa tagu jest typ składnika.

Renderuje następujące znaczniki `HeadingComponent` (*HeadingComponent.cshtml*) wystąpienie:

[!code-cshtml[](common/samples/3.x/BlazorSample/Pages/Index.cshtml?start=11&end=11)]

## <a name="component-parameters"></a>Parametry składnika

Składniki mogą mieć *Parametry składnika*, które są definiowane za pomocą *niepublicznych* ozdobione właściwości klasy składnika `[Parameter]`. Używanie atrybutów, aby określić argumenty dla składnika w znacznikach.

W poniższym przykładzie `ParentComponent` ustawia wartość `Title` właściwość `ChildComponent`:

*ParentComponent.cshtml*:

[!code-cshtml[](common/samples/3.x/BlazorSample/Pages/ParentComponent.cshtml?start=1&end=7&highlight=5)]

*ChildComponent.cshtml*:

[!code-cshtml[](common/samples/3.x/BlazorSample/Pages/ChildComponent.cshtml?highlight=7-8)]

## <a name="child-content"></a>Zawartość elementu podrzędnego

Składniki można ustawić zawartość w innym składniku. Przypisywanie składnik udostępnia zawartości między tagami, które określają odbieranie składnika. Na przykład `ParentComponent` można udostępnić zawartość, która ma być renderowany przy platformy `ChildComponent` , umieszczając zawartość wewnątrz  **\<ChildComponent >** tagów.

*ParentComponent.cshtml*:

[!code-cshtml[](common/samples/3.x/BlazorSample/Pages/ParentComponent.cshtml?start=1&end=7&highlight=6)]

Składnik podrzędny ma `ChildContent` właściwość, która reprezentuje `RenderFragment`. Wartość `ChildContent` jest umieszczony w znaczniku elementu podrzędnego, której zawartość ma być renderowany. W poniższym przykładzie wartość `ChildContent` jest otrzymane od składnika nadrzędnego i renderowania wewnątrz panelu Bootstrap `panel-body`.

*ChildComponent.cshtml*:

[!code-cshtml[](common/samples/3.x/BlazorSample/Pages/ChildComponent.cshtml?highlight=3,10-11)]

> [!NOTE]
> Odbieranie właściwość `RenderFragment` zawartość, musi nosić `ChildContent` przez Konwencję.

## <a name="data-binding"></a>Powiązanie danych

Powiązanie danych do składników i elementów DOM odbywa się za pomocą `bind` atrybutu. Poniższy przykład tworzy powiązanie `ItalicsCheck` właściwość z polem wyboru zaznaczone stanu:

```cshtml
<input type="checkbox" class="form-check-input" id="italicsCheck" bind="@_italicsCheck" />
```

Gdy pole wyboru jest zaznaczone, a następnie wyczyszczone, wartość właściwości jest aktualizowany do `true` i `false`, odpowiednio.

Pole wyboru jest aktualizowana w interfejsie użytkownika tylko wtedy, gdy składnik jest renderowany, nie w odpowiedzi na zmianę wartości właściwości. Ponieważ składniki renderować się po wykonaniu kod procedury obsługi zdarzeń, aktualizacje właściwości są zazwyczaj odzwierciedlane w interfejsie użytkownika od razu.

Za pomocą `bind` z `CurrentValue` właściwości (`<input bind="@CurrentValue" />`) jest zasadniczo odpowiednikiem następujących czynności:

```cshtml
<input value="@CurrentValue" 
    onchange="@((UIChangeEventArgs __e) => CurrentValue = __e.Value)" />
```

Po wyrenderowaniu składnika `value` danego elementu wejściowego pochodzi z `CurrentValue` właściwości. Gdy użytkownik wpisuje w polu tekstowym `onchange` jest uruchamiany i `CurrentValue` zostaje ustalona zmieniona wartość. W rzeczywistości generowania kodu jest nieco bardziej złożone, ponieważ `bind` zajmuje się kilka przypadków, w którym konwersje są wykonywane. W zasadzie `bind` kojarzy bieżącą wartość wyrażenia z `value` atrybutu i uchwytów zmiany przy użyciu zarejestrowanego programu obsługi.

**Ciągi formatujące**

Powiązanie danych w programach [daty/godziny](https://docs.microsoft.com/dotnet/api/system.datetime) formatowanie ciągów (ale nie inne wyrażenia w tej chwili, takich jak waluta lub liczba podzielony na fragmenty):

```cshtml
<input bind="@StartDate" format-value="yyyy-MM-dd" />

@functions {
    [Parameter]
    private DateTime StartDate { get; set; } = new DateTime(2020, 1, 1);
}
```

`format-value` Atrybut określa format daty do zastosowania do `value` z `input` elementu. Format umożliwia również przeanalizować wartość po `onchange` wystąpi zdarzenie.

**Parametry składnika**

Powiązanie rozpoznaje również Parametry składnika, gdzie `bind-{property}` można powiązać wartości właściwości między składnikami.

Używa następującego składnika `ChildComponent` i wiąże `ParentYear` parametru z obiektu `Year` parametru w składniku podrzędne:

Składnik nadrzędny:

```cshtml
@page "/ParentComponent"

<h1>Parent Component</h1>

<p>ParentYear: @ParentYear</p>

<ChildComponent bind-Year="@ParentYear" />

<button class="btn btn-primary" onclick="@ChangeTheYear">Change Year to 1986</button>

@functions {
    [Parameter]
    private int ParentYear { get; set; } = 1978;

    void ChangeTheYear()
    {
        ParentYear = 1986;
    }
}
```

Element podrzędny:

```cshtml
<h2>Child Component</h2>

<p>Year: @Year</p>

@functions {
    [Parameter]
    private int Year { get; set; }

    [Parameter]
    private Action<int> YearChanged { get; set; }
}
```

`Year` Parametr jest możliwej do wiązania, ponieważ ma ona towarzyszące `YearChanged` zdarzeń, który jest zgodny z typem `Year` parametru.

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

## <a name="event-handling"></a>Obsługa zdarzeń

Składniki razor udostępniają funkcje obsługi zdarzeń. Atrybut elementu HTML o nazwie `on<event>` (na przykład `onclick`, `onsubmit`) z wartością wpisane delegata składniki Razor traktuje wartość atrybutu jako program obsługi zdarzeń. Nazwa atrybutu zawsze zaczyna się od `on`.

Poniższy kod wywoła `UpdateHeading` metodę po wybraniu przycisku w interfejsie użytkownika:

```cshtml
<button class="btn btn-primary" onclick="@UpdateHeading">
    Update heading
</button>

@functions {
    void UpdateHeading(UIMouseEventArgs e)
    {
        ...
    }
}
```

Poniższy kod wywoła `CheckboxChanged` metody, gdy pole wyboru jest zmieniana w interfejsie użytkownika:

```cshtml
<input type="checkbox" class="form-check-input" onchange="@CheckboxChanged" />

@functions {
    void CheckboxChanged()
    {
        ...
    }
}
```

Programy obsługi zdarzeń można też asynchronicznego i zwracają `Task`. Nie ma konieczności ręcznego wywoływania `StateHasChanged()`. Wyjątki są rejestrowane w momencie ich wystąpienia.

```cshtml
<button class="btn btn-primary" onclick="@UpdateHeading">
    Update heading
</button>

@functions {
    async Task UpdateHeading(UIMouseEventArgs e)
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
<button onclick="@(e => Console.WriteLine("Hello, world!"))">Say hello</button>
```

Często jest to wygodne zamknąć dodatkowe wartości, takich jak podczas iteracji w zestawie elementów. Poniższy przykład tworzy trzy przyciski wszystkich, która wywołuje metodę `UpdateHeading` przekazywaniem argumentu zdarzenia (`UIMouseEventArgs`) i jego numer przycisku (`buttonNumber`) w przypadku wybrania w interfejsie użytkownika:

```cshtml
<h2>@message</h2>

@for (var i = 1; i < 4; i++)
{
    var buttonNumber = i;

    <button class="btn btn-primary" 
            onclick="@(e => UpdateHeading(e, buttonNumber))">
        Button #@i
    </button>
}

@functions {
    string message = "Select a button to learn its position.";

    void UpdateHeading(UIMouseEventArgs e, int buttonNumber)
    {
        message = $"You selected Button #{buttonNumber} at " +
            "mouse position: {e.ClientX} X {e.ClientY}.";
    }
}
```

## <a name="capture-references-to-components"></a>Przechwytywanie odwołania do składników

Składnik odwołuje się do zapewnienia get sposób odwołania do wystąpienia składnika, dzięki czemu można wysyłać polecenia do tego wystąpienia, takie jak `Show` lub `Reset`. Do przechwycenia odwołania do składników, należy dodać `ref` atrybutu do elementu podrzędnego, a następnie zdefiniować pole o tej samej nazwie i typie jako element podrzędny.

```cshtml
<MyLoginDialog ref="loginDialog" ... />

@functions {
    MyLoginDialog loginDialog;

    void OnSomething()
    {
        loginDialog.Show();
    }
}
```

Po wyrenderowaniu składnika `loginDialog` pole jest wypełniane `MyLoginDialog` wystąpienie składnika podrzędnego. Następnie można wywoływać metod .NET w instancji składnika.

> [!IMPORTANT]
> `loginDialog` Zmiennej tylko jest wypełniana po składnik jest renderowana i jego dane wyjściowe obejmują `MyLoginDialog` elementu, ponieważ do tego czasu nie ma nic do odwołania. Do manipulowania odwołania do składników, po zakończeniu renderowania składnik, należy użyć `OnAfterRenderAsync` lub `OnAfterRender` metody cyklu życia.

Podczas gdy przechwytywania odwołania do składników używa składni podobnie [przechwytywania odwołania do elementu](xref:razor-components/javascript-interop#capture-references-to-elements), nie jest [międzyoperacyjnego JavaScript](xref:razor-components/javascript-interop) funkcji. Odwołania do składników nie są przekazywane do kodu JavaScript; są one używane tylko w kodzie .NET.

> [!NOTE]
> Czy **nie** umożliwia odwołania do składników mutować stan składnikach podrzędnych. Zamiast tego należy zawsze używać normalnego parametry deklaratywne do przekazywania danych do elementów podrzędnych. To sprawia, że podrzędnych automatycznie rerender w właściwym czasie.

## <a name="lifecycle-methods"></a>Cykl życia metody

`OnInitAsync` i `OnInit` wykonanie kodu po zainicjowaniu składnika. Aby wykonać operację asynchroniczną, użyj `OnInitAsync` i użyj `await` — słowo kluczowe:

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

`OnParametersSetAsync` i `OnParametersSet` są wywoływane, gdy składnik, który otrzyma parametry od jego elementu nadrzędnego, a wartości są przypisywane do właściwości. Te metody są wykonywane po `OnInit` podczas inicjowania składnika.

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

`OnAfterRenderAsync` i `OnAfterRender` jest wywoływana za każdym razem po zakończeniu renderowania składnika. Odwołania do elementu, jak i składnika zostaną wypełnione na tym etapie. Aby wykonać kroki dodatkowe inicjowanie przy użyciu renderowanej zawartości, takich jak aktywacja bibliotek JavaScript innych firm, które działają na renderowanych elementów DOM, należy użyć tego etapu.

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

Jeśli składnik implementuje [interfejsu IDisposable](https://docs.microsoft.com/dotnet/api/system.idisposable), [Dispose — metoda](https://docs.microsoft.com/dotnet/standard/garbage-collection/implementing-dispose) jest wywoływana, gdy składnik zostanie usunięty z interfejsu użytkownika. Używa następującego składnika `@implements IDisposable` i `Dispose` metody:

```csharp
@using System
@implements IDisposable

...

@functions {
    public void Dispose()
    {
        ...
    }
}
```

## <a name="routing"></a>Routing

Routing w składnikach Razor odbywa się, podając szablonu trasy na każdej części dostępne w aplikacji.

Gdy  *\*.cshtml* plik z `@page` dyrektywa jest kompilowany, wygenerowana klasa otrzymuje [RouteAttribute](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.mvc.routeattribute) Określanie szablonu trasy. W czasie wykonywania, router szuka klasy składników za pomocą `RouteAttribute` i renderuje, niezależnie od składnik ma szablon trasy, który pasuje do żądanego adresu URL.

Wiele szablonów tras można zastosować do składnika. Następujący składnik, który będzie odpowiadał na żądania `/BlazorRoute` i `/DifferentBlazorRoute`:

[!code-cshtml[](common/samples/3.x/BlazorSample/Pages/BlazorRoute.cshtml?start=1&end=4)]

## <a name="route-parameters"></a>Parametry trasy

Składniki mogą odbierać parametrów trasy z szablonu trasy dostępnego w `@page` dyrektywy. Router używa parametrów trasy do wypełnienia odpowiednich parametrów składnika.

*RouteParameter.cshtml*:

[!code-cshtml[](common/samples/3.x/BlazorSample/Pages/RouteParameter.cshtml?start=1&end=9)]

Opcjonalne parametry nie są obsługiwane, dlatego dwie `@page` dyrektywy są stosowane w powyższym przykładzie. Pierwszy pozwala nawigacji do składnika bez parametrów. Drugi `@page` przyjmuje dyrektywy `{text}` kierowanie parametru, a następnie przypisuje wartość do `Text` właściwości.

## <a name="base-class-inheritance-for-a-code-behind-experience"></a>Dziedziczenie klasy bazowej dla środowiska "związane z kodem"

Pliki składników (*\*.cshtml*) mieszać kod znaczników HTML i C# przetwarzania kodu w tym samym pliku. `@inherits` Dyrektywy może służyć do zapewnienia Razor składniki aplikacji w środowisku "związane z kodem" oddzielający składnika znaczników, od przetwarzania kodu.

[Przykładową aplikację](https://github.com/aspnet/Docs/tree/master/aspnetcore/razor-components/common/samples/) pokazuje, jak składnik może dziedziczyć klasy bazowej, `BlazorRocksBase`w celu zapewnienia składnika właściwości i metody.

*BlazorRocks.cshtml*:

[!code-cshtml[](common/samples/3.x/BlazorSample/Pages/BlazorRocks.cshtml?start=1&end=8)]

*BlazorRocksBase.cs*:

[!code-csharp[](common/samples/3.x/BlazorSample/Pages/BlazorRocksBase.cs)]

Klasa bazowa powinien pochodzić od `BlazorComponent`.

## <a name="razor-support"></a>Obsługa razor

**Dyrektywy razor**

W poniższej tabeli przedstawiono dyrektywy razor.

| — Dyrektywa | Opis |
| --------- | ----------- |
| [@functions](https://docs.microsoft.com/aspnet/core/mvc/views/razor#functions) | Dodaje C# blok kodu do składnika. |
| `@implements` | Implementuje interfejs dla klasy wygenerowanej składnika. |
| [@inherits](https://docs.microsoft.com/aspnet/core/mvc/views/razor#inherits) | Zapewnia pełną kontrolę nad klasę, która dziedziczy składnika. |
| [@inject](https://docs.microsoft.com/aspnet/core/mvc/views/razor#inject) | Włącza usługi iniekcji z [kontener usługi](https://docs.microsoft.com/aspnet/core/fundamentals/dependency-injection). Aby uzyskać więcej informacji, zobacz [wstrzykiwanie zależności do widoków](https://docs.microsoft.com/aspnet/core/mvc/views/dependency-injection). |
| `@layout` | Określa składnik układu. Składniki układu są używane, aby uniknąć zduplikowania kodu i niespójności. |
| [@page](https://docs.microsoft.com/aspnet/core/mvc/razor-pages#razor-pages) | Określa, że składnik obsługi żądań bezpośrednio. `@page` Dyrektywy można określić za pomocą trasy i opcjonalnych parametrów. W przeciwieństwie do stron Razor `@page` dyrektywy nie musi być pierwszą dyrektywę w górnej części pliku. Aby uzyskać więcej informacji, zobacz [Routing](xref:razor-components/routing). |
| [@using](https://docs.microsoft.com/aspnet/core/mvc/views/razor#using) | Dodaje C# `using` dyrektywy do klasy wygenerowanej składnika. |
| [@addTagHelper](https://docs.microsoft.com/aspnet/core/mvc/views/razor#tag-helpers) | Użyj `@addTagHelper` użycie składnika w innym zestawie niż zestaw aplikacji. |

**Atrybuty warunkowe**

Atrybuty warunkowe są renderowane na podstawie wartości platformy .NET. Jeśli wartość jest `false` lub `null`, ten atrybut nie jest renderowany. Jeśli wartość jest `true`, ten atrybut jest renderowany zminimalizowane.

W poniższym przykładzie `IsCompleted` Określa, czy `checked` jest renderowany w znacznikach formantu.

```cshtml
<input type="checkbox" checked="@IsCompleted" />

@functions {
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

Aby uzyskać więcej informacji na temat Razor, zobacz [dokumentacja składni Razor](https://docs.microsoft.com/aspnet/core/mvc/views/razor). Należy pamiętać, że nie wszystkie funkcje aparatu Razor są dostępne w składnikach Razor w tej chwili.

## <a name="raw-html"></a>Surowy kod HTML

Ciągi są zwykle renderowane przy użyciu modelu DOM węzły tekstowe, co oznacza, że żadnych znaczników, które mogą zawierać jest ignorowany i traktowany jako literał tekstowy. Aby renderować kod HTML, opakowywanie zawartości HTML w `MarkupString` wartości, który następnie jest analizowany jako HTML lub SVG i wstawiony DOM.

> [!WARNING]
> Renderowanie surowe HTML skonstruowany z dowolnego niezaufanych źródło jest **zagrożenie dla bezpieczeństwa** i należy ich unikać!

Poniższy przykład pokazuje użycie `MarkupString` typu do dodania bloku zawartości statycznej HTML do wyniku renderowania składnika:

```html
@((MarkupString)myMarkup)

@functions {
    string myMarkup = "<p class='markup'>This is a <em>markup string</em>.</p>";
}
```

## <a name="templated-components"></a>Składniki oparte na szablonach

Oparte na szablonach składniki są składniki, które akceptują jeden lub więcej szablonów interfejsu użytkownika jako parametry, które następnie mogą być używane jako część logiki renderowania składnika. Oparte na szablonach składniki umożliwiają tworzenie składników wyższego poziomu, które są bardziej wielokrotnego użytku, niż regularne składników. Zawiera kilka przykładów:

* Składnik tabeli, który umożliwia użytkownikowi określenie szablonów dla nagłówka tabeli, wierszy i stopki.
* Składnik listy, który umożliwia użytkownikowi określić szablon służący do renderowania elementów na liście.

### <a name="template-parameters"></a>Parametry szablonu

Oparte na szablonach składnika jest zdefiniowany, określając jeden lub więcej parametrów składnika typu `RenderFragment` lub `RenderFragment<T>`. Fragment renderowania reprezentuje segment interfejsu użytkownika, który jest renderowany przez składnik. Fragment renderowania opcjonalnie przyjmuje parametr, który można określić, gdy jest wywoływany fragmentu renderowania.

*Components/TableTemplate.cshtml*:

[!code-cshtml[](common/samples/3.x/BlazorSample/Components/TableTemplate.cshtml)]

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

Oparte na szablonach składniki są często objęte wpisane. Na przykład element ListView ogólny może zostać użyty do renderowania `IEnumerable<T>` wartości. Aby zdefiniować element ogólny, należy użyć `@typeparam` dyrektywy, aby określić parametry typu.

*Components/ListViewTemplate.cshtml*:

[!code-cshtml[](common/samples/3.x/BlazorSample/Components/ListViewTemplate.cshtml?highlight=1)]

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

Składnik nadrzędny może zapewnić kaskadowych przy użyciu wartości `CascadingValue` składnika. `CascadingValue` Składnika otacza poddrzewo hierarchii składników i dostarcza pojedynczą wartość dla wszystkich składników w ramach tego poddrzewa.

Na przykład przykładowa aplikacja określa informacje o motywie (`ThemeInfo`) w jednej aplikacji układów jako parametr kaskadowych dla wszystkich składników, które tworzą treści układ `@Body` właściwości. `ButtonClass` jest przypisywana wartość `btn-success` w składniku układu. Dowolny składnik podrzędny mogą używać tej właściwości, za pośrednictwem `ThemeInfo` cascading obiektu.

*Shared/CascadingValuesParametersLayout.cshtml*:

```cshtml
@inherits BlazorLayoutComponent
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

@functions {
    ThemeInfo theme = new ThemeInfo { ButtonClass = "btn-success" };
}
```

Zapewnienie korzystania z wartości kaskadowych, składniki deklarować parametrów kaskadowych przy użyciu `[CascadingParameter]` atrybut lub na podstawie wartości ciągu nazwy:

```cshtml
<CascadingValue Value=@PermInfo Name="UserPermissions">...</CascadingValue>

[CascadingParameter(Name = "UserPermissions")] PermInfo Permissions { get; set; }
```

Powiązanie z wartością nazwy ciągu jest istotne, jeśli masz wiele kaskadowych wartości tego samego typu i potrzebujesz odróżnić je w ramach tej samej poddrzewo.

Kaskadowe wartości są powiązane kaskadowych parametry według typu.

W przykładowej aplikacji `CascadingValuesParametersTheme` składnika jest powiązana `ThemeInfo` kaskadowa wartość parametru kaskadowych. Parametr służy do ustawiania klasę CSS dla jednego z przyciski wyświetlane przez składnik.

*Pages/CascadingValuesParametersTheme.cshtml*:

```cshtml
@page "/cascadingvaluesparameterstheme"
@layout CascadingValuesParametersLayout
@using BlazorSample.UIThemeClasses

<h1>Cascading Values & Parameters</h1>

<p>Current count: @currentCount</p>

<p>
    <button class="btn" onclick="@IncrementCount">
        Increment Counter (Unthemed)
    </button>
</p>

<p>
    <button class="btn @ThemeInfo.ButtonClass" onclick="@IncrementCount">
        Increment Counter (Themed)
    </button>
</p>

@functions {
    int currentCount = 0;

    [CascadingParameter] protected ThemeInfo ThemeInfo { get; set; }

    void IncrementCount()
    {
        currentCount++;
    }
}
```

### <a name="tabset-example"></a>Przykład TabSet

Parametry kaskadowych również włączyć składników do współpracy w całej hierarchii składnika. Na przykład, należy wziąć pod uwagę następujące *TabSet* przykładu w przykładowej aplikacji.

Przykładowa aplikacja ma `ITab` interfejs, który karty implementacji:

[!code-cs[](common/samples/3.x/BlazorSample/UIInterfaces/ITab.cs)]

`CascadingValuesParametersTabSet` Składnik używa `TabSet` składnik, który zawiera kilka `Tab` składników:

[!code-cshtml[](common/samples/3.x/BlazorSample/Pages/CascadingValuesParametersTabSet.cshtml?name=snippet_TabSet)]

Element podrzędny `Tab` składniki jawnie nie są przekazywane jako parametry do `TabSet`. Zamiast tego elementu podrzędnego `Tab` składniki są częścią zawartość elementu podrzędnego `TabSet`. Jednak `TabSet` musi wiedzieć o każdym `Tab` , dzięki czemu można renderować, nagłówki i aktywną kartę. Można włączyć koordynacja bez konieczności dodatkowego kodu `TabSet` składnika *może zapewnić sama jako wartość kaskadowych* , następnie zostaje pobrana przez podrzędny `Tab` składników.

*Components/TabSet.cshtml*:

[!code-cshtml[](common/samples/3.x/BlazorSample/Components/TabSet.cshtml)]

Podrzędny `Tab` składniki przechwytywania, zawierający `TabSet` jako parametr kaskadowych, więc `Tab` składniki dodania użytkownika do `TabSet` i współrzędnych, na którym `Tab` jest aktywny.

*Components/Tab.cshtml*:

[!code-cshtml[](common/samples/3.x/BlazorSample/Components/Tab.cshtml)]

## <a name="razor-templates"></a>Szablony razor

Renderowanie fragmentów można zdefiniować przy użyciu składni szablonów Razor. Szablony razor służą do definiowania fragmentu interfejsu użytkownika i założono następujący format:

```cshtml
@<tag>...</tag>
```

Poniższy przykład ilustruje sposób określania `RenderFragment` i `RenderFragment<T>` wartości.

*RazorTemplates.cshtml*:

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
