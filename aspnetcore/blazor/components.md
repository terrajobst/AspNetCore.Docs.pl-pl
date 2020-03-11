---
title: Tworzenie i używanie składników ASP.NET Core Razor
author: guardrex
description: Dowiedz się, jak tworzyć i używać składników Razor, w tym jak powiązać z danymi, obsługiwać zdarzenia i zarządzać cyklem życia składników.
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 02/25/2020
no-loc:
- Blazor
- SignalR
uid: blazor/components
ms.openlocfilehash: e444ebfef5143a6c33ed2d122933903ad3a4f4a7
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78660700"
---
# <a name="create-and-use-aspnet-core-razor-components"></a>Tworzenie i używanie składników ASP.NET Core Razor

Autorzy [Luke Latham](https://github.com/guardrex) i [Daniel Roth](https://github.com/danroth27)

[Wyświetl lub pobierz przykładowy kod](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/blazor/common/samples/) ([jak pobrać](xref:index#how-to-download-a-sample))

aplikacje Blazor są kompilowane przy użyciu *składników*programu. Składnik jest niezależnym fragmentem interfejsu użytkownika (UI), takim jak strona, okno dialogowe lub formularz. Składnik zawiera znaczniki HTML i logikę przetwarzania wymagane do iniekcji danych lub reagowania na zdarzenia interfejsu użytkownika. Składniki są elastyczne i lekkie. Mogą być zagnieżdżane, ponownie używane i udostępniane między projektami.

## <a name="component-classes"></a>Klasy składników

Składniki są zaimplementowane w plikach składników [Razor](xref:mvc/views/razor) ( *. Razor*) przy użyciu kombinacji C# i znaczników HTML. Składnik Blazor jest formalnie określany jako *składnik Razor*.

Nazwa składnika musi rozpoczynać się wielką literą. Na przykład *MyCoolComponent. Razor* jest prawidłowy, a *MyCoolComponent. Razor* jest nieprawidłowy.

Interfejs użytkownika dla składnika jest definiowany przy użyciu języka HTML. Logika renderowania dynamicznego (na przykład pętle, warunkowe, wyrażenia) jest dodawana przy C# użyciu osadzonej składni o nazwie [Razor](xref:mvc/views/razor). Po skompilowaniu aplikacji logika znaczników HTML i C# renderowania jest konwertowana na klasę składnika. Nazwa wygenerowanej klasy jest zgodna z nazwą pliku.

Elementy członkowskie klasy składnika są zdefiniowane w bloku `@code`. W bloku `@code` stan składnika (właściwości, pola) jest określany przy użyciu metod obsługi zdarzeń lub definiowania innej logiki składnika. Dozwolony jest więcej niż jeden blok `@code`.

Składowe składnika mogą być używane jako część logiki renderowania składnika przy użyciu C# wyrażeń, które zaczynają się od `@`. Na przykład C# pole jest renderowane przez utworzenie prefiksu `@` na nazwę pola. Poniższy przykład szacuje i renderuje:

* `_headingFontStyle` wartość właściwości CSS dla `font-style`.
* `_headingText` do zawartości elementu `<h1>`.

```razor
<h1 style="font-style:@_headingFontStyle">@_headingText</h1>

@code {
    private string _headingFontStyle = "italic";
    private string _headingText = "Put on your new Blazor!";
}
```

Po pierwszym wyrenderowaniu składnika składnik generuje jego drzewo renderowania w odpowiedzi na zdarzenia. Blazor następnie porównuje nowe drzewo renderowania z poprzednią i zastosuje wszelkie modyfikacje Document Object Model (DOM) przeglądarki.

Składniki są zwykłymi C# klasami i mogą być umieszczane w dowolnym miejscu w projekcie. Składniki, które generują strony sieci Web, zwykle znajdują się w folderze *strony* . Składniki niestronicowe są często umieszczane w folderze *udostępnionym* lub w folderze niestandardowym dodanym do projektu.

Zazwyczaj przestrzeń nazw składnika pochodzi od głównej przestrzeni nazw aplikacji i lokalizacji składnika (folderu) w aplikacji. Jeśli główna przestrzeń nazw aplikacji jest `BlazorApp` a składnik `Counter` znajduje się w folderze *strony* :

* Przestrzeń nazw składnika `Counter` jest `BlazorApp.Pages`.
* W pełni kwalifikowana nazwa typu składnika jest `BlazorApp.Pages.Counter`.

Aby uzyskać więcej informacji, zobacz sekcję [Importowanie składników](#import-components) .

Aby użyć folderu niestandardowego, należy dodać przestrzeń nazw folderu niestandardowego do składnika nadrzędnego lub pliku *_Imports. Razor* aplikacji. Na przykład następująca przestrzeń nazw sprawia, że składniki w folderze *Components* są dostępne, gdy główna przestrzeń nazw aplikacji jest `BlazorApp`:

```razor
@using BlazorApp.Components
```

## <a name="static-assets"></a>Statyczne zasoby

Blazor jest zgodna z Konwencją ASP.NET Core aplikacji umieszczających statyczne zasoby w [folderze głównym katalogu głównego (wwwroot)](xref:fundamentals/index#web-root)projektu.

Użyj ścieżki względnej podstawowe (`/`), aby odwołać się do katalogu głównego sieci Web dla statycznego elementu zawartości. W poniższym przykładzie *logo. png* znajduje się fizycznie w folderze *{Project root}/wwwroot/images* :

```razor
<img alt="Company logo" src="/images/logo.png" />
```

Składniki Razor **nie** obsługują notacji z ukośnikiem (`~/`).

Aby uzyskać informacje na temat ustawiania ścieżki podstawowej aplikacji, zobacz <xref:host-and-deploy/blazor/index#app-base-path>.

## <a name="tag-helpers-arent-supported-in-components"></a>Pomocnicy tagów nie są obsługiwani w składnikach

[Pomocnicy tagów](xref:mvc/views/tag-helpers/intro) nie są obsługiwani w składnikach Razor (pliki *. Razor* ). Aby zapewnić funkcje podobne do pomocnika tagów w Blazor, Utwórz składnik o tej samej funkcji, co pomocnik tagów i użyj składnika zamiast.

## <a name="use-components"></a>Używanie składników

Składniki mogą zawierać inne składniki, deklarując je za pomocą składni elementu HTML. Znaczniki użycia składnika wyglądają jak tag HTML, gdzie nazwa znacznika jest typem składnika.

W powiązaniu atrybutu rozróżniana jest wielkość liter. Na przykład `@bind` jest prawidłowy, a `@Bind` jest nieprawidłowy.

Poniższy znacznik w *indeksie. Razor* renderuje wystąpienie `HeadingComponent`:

```razor
<HeadingComponent />
```

*Składniki/HeadingComponent. Razor*:

[!code-razor[](common/samples/3.x/BlazorWebAssemblySample/Components/HeadingComponent.razor)]

Jeśli składnik zawiera element HTML z wielką literą, która nie jest zgodna z nazwą składnika, jest emitowane ostrzeżenie wskazujące, że element ma nieoczekiwaną nazwę. Dodanie `@using` dyrektywy dla przestrzeni nazw składnika sprawia, że składnik jest dostępny, co rozwiązuje ostrzeżenie.

## <a name="routing"></a>Routing

Routing w Blazor jest realizowany przez dostarczenie szablonu trasy do każdego dostępnego składnika w aplikacji.

Po skompilowaniu pliku Razor z dyrektywą `@page`, wygenerowana Klasa otrzymuje <xref:Microsoft.AspNetCore.Mvc.RouteAttribute> określania szablonu trasy. W czasie wykonywania router szuka klas składników przy użyciu `RouteAttribute` i renderuje niezależnie składnik ma szablon trasy pasujący do żądanego adresu URL.

```razor
@page "/ParentComponent"

...
```

Aby uzyskać więcej informacji, zobacz <xref:blazor/routing>.

## <a name="parameters"></a>Parametry

### <a name="route-parameters"></a>Parametry trasy

Składniki mogą odbierać parametry tras z szablonu trasy dostarczonego w dyrektywie `@page`. Router używa parametrów trasy, aby wypełnić odpowiednie parametry składnika.

*Strony/RouteParameter. Razor*:

[!code-razor[](components/samples_snapshot/RouteParameter.razor?highlight=2,7-8)]

Parametry opcjonalne nie są obsługiwane, więc dwie dyrektywy `@page` są stosowane w poprzednim przykładzie. Pierwszy zezwala na nawigowanie do składnika bez parametru. Druga dyrektywa `@page` otrzymuje parametr trasy `{text}` i przypisuje wartość do właściwości `Text`.

Składnia *catch-all* (`*`/`**`), która przechwytuje ścieżkę między wieloma granicami folderów, **nie** jest obsługiwana w składnikach Razor ( *. Razor*).

### <a name="component-parameters"></a>Parametry składnika

Składniki mogą mieć *Parametry składnika*, które są zdefiniowane przy użyciu właściwości publicznych w klasie składnika z atrybutem `[Parameter]`. Użyj atrybutów, aby określić argumenty dla składnika w znaczniku.

*Składniki/ChildComponent. Razor*:

[!code-razor[](common/samples/3.x/BlazorWebAssemblySample/Components/ChildComponent.razor?highlight=2,11-12)]

W poniższym przykładzie z przykładowej aplikacji `ParentComponent` ustawia wartość właściwości `Title` `ChildComponent`.

*Strony/ParentComponent. Razor*:

[!code-razor[](components/samples_snapshot/ParentComponent.razor?highlight=5-6)]

## <a name="child-content"></a>Zawartość podrzędna

Składniki mogą ustawiać zawartość innego składnika. Składnik Assigner zawiera zawartość między tagami, które określają składnik do odbioru.

W poniższym przykładzie `ChildComponent` ma właściwość `ChildContent`, która reprezentuje `RenderFragment`, która reprezentuje segment interfejsu użytkownika do renderowania. Wartość `ChildContent` jest umieszczana w znacznikach składnika, w którym powinna być renderowana zawartość. Wartość `ChildContent` jest odbierana ze składnika nadrzędnego i renderowany w `panel-body`panelu uruchamiania.

*Składniki/ChildComponent. Razor*:

[!code-razor[](common/samples/3.x/BlazorWebAssemblySample/Components/ChildComponent.razor?highlight=3,14-15)]

> [!NOTE]
> Właściwość, która otrzymuje `RenderFragment` zawartość, musi mieć nazwę `ChildContent` według Konwencji.

`ParentComponent` w przykładowej aplikacji może udostępniać zawartość w celu renderowania `ChildComponent` przez umieszczenie zawartości wewnątrz tagów `<ChildComponent>`.

*Strony/ParentComponent. Razor*:

[!code-razor[](components/samples_snapshot/ParentComponent.razor?highlight=7-8)]

## <a name="attribute-splatting-and-arbitrary-parameters"></a>Korzystając atrybutów i dowolne parametry

Składniki mogą przechwytywać i renderować dodatkowe atrybuty oprócz zadeklarowanych parametrów składnika. Dodatkowe atrybuty mogą być przechwytywane w słowniku, a następnie *splatted* na element, gdy składnik jest renderowany przy użyciu dyrektywy [`@attributes`](xref:mvc/views/razor#attributes) Razor. Ten scenariusz jest przydatny podczas definiowania składnika, który generuje element znaczników, który obsługuje różne dostosowania. Na przykład można żmudnym definiować atrybuty oddzielnie dla `<input>`, które obsługują wiele parametrów.

W poniższym przykładzie pierwszy element `<input>` (`id="useIndividualParams"`) używa pojedynczych parametrów składnika, podczas gdy drugi `<input>` elementu (`id="useAttributesDict"`) używa atrybutu korzystając:

```razor
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

Typ parametru musi implementować `IEnumerable<KeyValuePair<string, object>>` za pomocą kluczy ciągu. Używanie `IReadOnlyDictionary<string, object>` jest również opcją w tym scenariuszu.

Renderowane `<input>` elementy używające obu metod są identyczne:

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

Aby zaakceptować dowolne atrybuty, zdefiniuj parametr składnika przy użyciu atrybutu `[Parameter]` z właściwością `CaptureUnmatchedValues` ustawioną na `true`:

```razor
@code {
    [Parameter(CaptureUnmatchedValues = true)]
    public Dictionary<string, object> InputAttributes { get; set; }
}
```

Właściwość `CaptureUnmatchedValues` na `[Parameter]` umożliwia dopasowanie parametru do wszystkich atrybutów, które nie są zgodne z żadnym innym parametrem. Składnik może definiować tylko jeden parametr z `CaptureUnmatchedValues`. Typ właściwości używany z `CaptureUnmatchedValues` musi być możliwy do przypisania z `Dictionary<string, object>` z kluczami ciągu. w tym scenariuszu są również dostępne opcje `IEnumerable<KeyValuePair<string, object>>` lub `IReadOnlyDictionary<string, object>`.

Pozycja `@attributes` odnosząca się do pozycji atrybutów elementu jest ważna. Gdy `@attributes` są splatted na elemencie, atrybuty są przetwarzane od prawej do lewej (Ostatnia do). Rozważmy następujący przykład składnika, który zużywa składnik `Child`:

*ParentComponent. Razor*:

```razor
<ChildComponent extra="10" />
```

*ChildComponent. Razor*:

```razor
<div @attributes="AdditionalAttributes" extra="5" />

[Parameter(CaptureUnmatchedValues = true)]
public IDictionary<string, object> AdditionalAttributes { get; set; }
```

Atrybut `extra` składnika `Child` jest ustawiony na prawo od `@attributes`. `<div>` renderowanego składnika `Parent` zawiera `extra="5"`, gdy jest on przeszukiwany za pomocą dodatkowego atrybutu, ponieważ atrybuty są przetwarzane od prawej do lewej (od ostatni do pierwszego):

```html
<div extra="5" />
```

W poniższym przykładzie porządek `extra` i `@attributes` jest odwrócony w `<div>`składnika `Child`:

*ParentComponent. Razor*:

```razor
<ChildComponent extra="10" />
```

*ChildComponent. Razor*:

```razor
<div extra="5" @attributes="AdditionalAttributes" />

[Parameter(CaptureUnmatchedValues = true)]
public IDictionary<string, object> AdditionalAttributes { get; set; }
```

Renderowane `<div>` w składniku `Parent` zawiera `extra="10"` w przypadku przechodzenia przez dodatkowy atrybut:

```html
<div extra="10" />
```

## <a name="capture-references-to-components"></a>Przechwyć odwołania do składników

Odwołania do składników zapewniają sposób odwoływania się do wystąpienia składnika, dzięki czemu można wydać polecenia do tego wystąpienia, takie jak `Show` lub `Reset`. Aby przechwycić odwołanie do składnika:

* Dodaj atrybut [`@ref`](xref:mvc/views/razor#ref) do składnika podrzędnego.
* Zdefiniuj pole z tym samym typem co składnik podrzędny.

```razor
<MyLoginDialog @ref="_loginDialog" ... />

@code {
    private MyLoginDialog _loginDialog;

    private void OnSomething()
    {
        _loginDialog.Show();
    }
}
```

Gdy składnik jest renderowany, pole `_loginDialog` jest wypełniane za pomocą `MyLoginDialog` podrzędnego wystąpienia składnika. Następnie można wywołać metody .NET w wystąpieniu składnika.

> [!IMPORTANT]
> Zmienna `_loginDialog` jest wypełniana tylko po wyrenderowaniu składnika, a jego wyjście zawiera element `MyLoginDialog`. Do tego momentu nie ma niczego do odwołania. Aby manipulować odwołaniami do składników po zakończeniu renderowania składnika, należy użyć [metody OnAfterRenderAsync lub OnAfterRender](xref:blazor/lifecycle#after-component-render).

Podczas przechwytywania odwołań do składników użycie podobnej składni do [przechwytywania odwołań do elementów](xref:blazor/call-javascript-from-dotnet#capture-references-to-elements)nie jest funkcją międzyoperacyjności języka JavaScript. Odwołania do składników nie są przesyłane do kodu JavaScript,&mdash;są używane tylko w kodzie platformy .NET.

> [!NOTE]
> **Nie** należy używać odwołań do składników do mutacji stanu składników podrzędnych. Zamiast tego należy używać zwykłych parametrów deklaratywnych do przekazywania danych do składników podrzędnych. Użycie normalnych parametrów deklaratywnych powoduje, że składniki podrzędne, które automatycznie uruchamiają się w prawidłowym czasie.

## <a name="invoke-component-methods-externally-to-update-state"></a>Wywołaj metody składnika zewnętrznie, aby zaktualizować stan

Blazor używa `SynchronizationContext` w celu wymuszenia pojedynczego wątku logicznego wykonywania. W tym `SynchronizationContext`są wykonywane [metody cyklu życia](xref:blazor/lifecycle) składnika i wszystkie wywołania zwrotne zdarzeń, które są wywoływane przez Blazor. W przypadku zdarzenia składnika należy zaktualizować na podstawie zdarzenia zewnętrznego, takiego jak czasomierz lub inne powiadomienia, użyj metody `InvokeAsync`, która będzie wysyłana do `SynchronizationContext`Blazor.

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

Zarejestruj `NotifierService` jako singletion:

* W programie Blazor webassembly Zarejestruj usługę w `Program.Main`:

  ```csharp
  builder.Services.AddSingleton<NotifierService>();
  ```

* W programie Blazor Server Zarejestruj usługę w `Startup.ConfigureServices`:

  ```csharp
  services.AddSingleton<NotifierService>();
  ```

Użyj `NotifierService`, aby zaktualizować składnik:

```razor
@page "/"
@inject NotifierService Notifier
@implements IDisposable

<p>Last update: @_lastNotification.key = @_lastNotification.value</p>

@code {
    private (string key, int value) _lastNotification;

    protected override void OnInitialized()
    {
        Notifier.Notify += OnNotify;
    }

    public async Task OnNotify(string key, int value)
    {
        await InvokeAsync(() =>
        {
            _lastNotification = (key, value);
            StateHasChanged();
        });
    }

    public void Dispose()
    {
        Notifier.Notify -= OnNotify;
    }
}
```

W poprzednim przykładzie `NotifierService` wywołuje metodę `OnNotify` składnika poza `SynchronizationContext`Blazor. `InvokeAsync` jest używany do przełączania do poprawnego kontekstu i renderowania kolejki.

## <a name="use-key-to-control-the-preservation-of-elements-and-components"></a>Użyj klucza \@, aby kontrolować zachowywanie elementów i składników

Podczas renderowania listy elementów lub składników oraz elementów lub składników, które następnie zmieniają się, algorytm różnicowania Blazormusi zdecydować, które z poprzednich elementów lub składników mogą być zachowywane i jak obiekty modelu powinny być mapowane na nie. Zwykle ten proces jest automatyczny i można go zignorować, ale istnieją przypadki, w których może być konieczne sterowanie procesem.

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

Zawartość kolekcji `People` może ulec zmianie z wstawionymi, usuniętymi lub z ponownymi zamówieniami. Gdy składnik jest przerenderowany, składnik `<DetailsEditor>` może zmienić, aby otrzymywać różne `Details` wartości parametrów. Może to spowodować bardziej złożone odwzorowanie niż oczekiwano. W niektórych przypadkach odzyskanie może prowadzić do zauważalnych różnic w zachowaniu, takich jak brak fokusu elementu.

Proces mapowania można kontrolować przy użyciu atrybutu dyrektywy `@key`. `@key` powoduje, że algorytm różnicowania gwarantuje zachowywanie elementów lub składników na podstawie wartości klucza:

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

W przypadku zmiany kolekcji `People`, algorytm różnicowania zachowuje skojarzenie między wystąpieniami `<DetailsEditor>` i wystąpieniami `person`:

* Jeśli `Person` zostanie usunięty z listy `People`, tylko odpowiednie wystąpienie `<DetailsEditor>` zostanie usunięte z interfejsu użytkownika. Inne wystąpienia pozostaną bez zmian.
* Jeśli na liście zostanie wstawiony `Person`, jedno nowe wystąpienie `<DetailsEditor>` zostanie wstawione do odpowiedniego położenia. Inne wystąpienia pozostaną bez zmian.
* W przypadku ponownego uporządkowania wpisów `Person` odpowiednie wystąpienia `<DetailsEditor>` są zachowywane i uporządkowane w interfejsie użytkownika.

W niektórych scenariuszach użycie `@key` minimalizuje złożoność operacji renderowania i pozwala uniknąć potencjalnych problemów związanych z zmianami stanowymi modelu DOM, takich jak pozycja fokusu.

> [!IMPORTANT]
> Klucze są lokalne dla każdego elementu kontenera lub składnika. Klucze nie są porównywane globalnie w całym dokumencie.

### <a name="when-to-use-key"></a>Kiedy używać klucza \@

Zazwyczaj warto używać `@key` zawsze, gdy lista jest renderowana (na przykład w bloku `@foreach`), a odpowiednia wartość istnieje do zdefiniowania `@key`.

Można również użyć `@key`, aby uniemożliwić Blazor zachowywania poddrzewa elementu lub składnika, gdy zmieniany jest obiekt:

```razor
<div @key="currentPerson">
    ... content that depends on currentPerson ...
</div>
```

W przypadku zmiany `@currentPerson` dyrektywa `@key` wymusza Blazor odrzucania całego `<div>` i jego obiektów podrzędnych i ponownej kompilacji poddrzewa w interfejsie użytkownika z nowymi elementami i składnikami. Może to być przydatne, jeśli zachodzi konieczność zagwarantowania, że stan interfejsu użytkownika nie jest zachowywany po zmianie `@currentPerson`.

### <a name="when-not-to-use-key"></a>Kiedy nie używać klucza \@

Różnica między `@key`ami jest kosztem wydajności. Koszt wydajności nie jest duży, ale Określ `@key` tylko wtedy, gdy kontrolowanie reguł utrwalania elementów i składników korzyści dla aplikacji.

Nawet jeśli `@key` nie jest używany, Blazor zachowuje elementy podrzędne i wystąpienia składników tak dużo, jak to możliwe. Jedyną zaletą korzystania z `@key` jest kontrola nad *sposobem* , w jaki wystąpienia modelu są mapowane na zachowane wystąpienia składników, zamiast algorytmu różnicowego, wybierając mapowanie.

### <a name="what-values-to-use-for-key"></a>Wartości, które mają być używane dla klucza \@

Ogólnie rzecz biorąc, warto podać jeden z następujących rodzajów wartości dla `@key`:

* Wystąpienia obiektów modelu (na przykład wystąpienie `Person` jak w poprzednim przykładzie). Zapewnia to zachowywanie na podstawie równości odwołań do obiektów.
* Unikatowe identyfikatory (na przykład wartości klucza podstawowego typu `int`, `string`lub `Guid`).

Upewnij się, że wartości używane dla `@key` nie kolidują. Jeśli w tym samym elemencie nadrzędnym zostaną wykryte wartości powodujące konflikt, Blazor zgłasza wyjątek, ponieważ nie może on w sposób jednoznaczny mapować starych elementów lub składników na nowe elementy lub składniki. Używaj tylko odrębnych wartości, takich jak wystąpienia obiektów lub wartości klucza podstawowego.

## <a name="partial-class-support"></a>Obsługa klasy częściowej

Składniki Razor są generowane jako klasy częściowe. Składniki Razor są tworzone przy użyciu jednej z następujących metod:

* C#kod jest zdefiniowany w bloku [`@code`](xref:mvc/views/razor#code) przy użyciu znaczników HTML i kodu Razor w pojedynczym pliku. Szablony Blazor definiują ich składniki Razor przy użyciu tego podejścia.
* C#kod jest umieszczany w pliku związanym z kodem zdefiniowanym jako Klasa częściowa.

Poniższy przykład pokazuje domyślny składnik `Counter` z blokiem `@code` w aplikacji wygenerowanej na podstawie szablonu Blazor. Znaczniki HTML, kod Razor i C# kod są w tym samym pliku:

*Counter. Razor*:

```razor
@page "/counter"

<h1>Counter</h1>

<p>Current count: @_currentCount</p>

<button class="btn btn-primary" @onclick="IncrementCount">Click me</button>

@code {
    private int _currentCount = 0;

    void IncrementCount()
    {
        _currentCount++;
    }
}
```

Składnik `Counter` można również utworzyć przy użyciu pliku związanego z kodem z klasą częściową:

*Counter. Razor*:

```razor
@page "/counter"

<h1>Counter</h1>

<p>Current count: @_currentCount</p>

<button class="btn btn-primary" @onclick="IncrementCount">Click me</button>
```

*Counter.Razor.cs*:

```csharp
namespace BlazorApp.Pages
{
    public partial class Counter
    {
        private int _currentCount = 0;

        void IncrementCount()
        {
            _currentCount++;
        }
    }
}
```

W razie potrzeby dodaj wszystkie wymagane przestrzenie nazw do pliku klasy częściowej. Typowe przestrzenie nazw używane przez składniki Razor obejmują:

```csharp
using Microsoft.AspNetCore.Authorization;
using Microsoft.AspNetCore.Components;
using Microsoft.AspNetCore.Components.Authorization;
using Microsoft.AspNetCore.Components.Forms;
using Microsoft.AspNetCore.Components.Routing;
using Microsoft.AspNetCore.Components.Web;
```

## <a name="specify-a-base-class"></a>Określ klasę bazową

Dyrektywa [`@inherits`](xref:mvc/views/razor#inherits) może służyć do określania klasy bazowej dla składnika. Poniższy przykład pokazuje, jak składnik może dziedziczyć klasę bazową `BlazorRocksBase`, aby zapewnić właściwości i metody składnika. Klasa bazowa powinna pochodzić od `ComponentBase`.

*Strony/BlazorRocks. Razor*:

```razor
@page "/BlazorRocks"
@inherits BlazorRocksBase

<h1>@BlazorRocksText</h1>
```

*BlazorRocksBase.cs*:

```csharp
using Microsoft.AspNetCore.Components;

namespace BlazorSample
{
    public class BlazorRocksBase : ComponentBase
    {
        public string BlazorRocksText { get; set; } = 
            "Blazor rocks the browser!";
    }
}
```

## <a name="specify-an-attribute"></a>Określ atrybut

Atrybuty można określić w składnikach Razor za pomocą dyrektywy [`@attribute`](xref:mvc/views/razor#attribute) . Poniższy przykład stosuje atrybut `[Authorize]` do klasy składnika:

```razor
@page "/"
@attribute [Authorize]
```

## <a name="import-components"></a>Importuj składniki

Przestrzeń nazw składnika utworzone przy użyciu Razor jest oparta na (w kolejności priorytetu):

* Wyznaczanie [`@namespace`](xref:mvc/views/razor#namespace) w znaczniku pliku*Razor (`@namespace BlazorSample.MyNamespace`* ).
* `RootNamespace` projektu w pliku projektu (`<RootNamespace>BlazorSample</RootNamespace>`).
* Nazwa projektu, pobrana z nazwy pliku projektu ( *. csproj*) i ścieżka z katalogu głównego projektu do składnika. Na przykład struktura rozpoznaje *{Project root}/Pages/index.Razor* (*BlazorSample. csproj*) do przestrzeni nazw `BlazorSample.Pages`. Składniki przestrzegają C# reguł powiązań nazw. Dla składnika `Index` w tym przykładzie składniki należące do zakresu są wszystkich składników:
  * W tym samym folderze *strony*.
  * Składniki w katalogu głównym projektu, które nie określają jawnie innej przestrzeni nazw.

Składniki zdefiniowane w innej przestrzeni nazw są wprowadzane do zakresu za pomocą dyrektywy [`@using`](xref:mvc/views/razor#using) Razor.

Jeśli inny składnik, `NavMenu.razor`, istnieje w *BlazorSample/Shared/* folder, składnik może być używany w `Index.razor` z następującą instrukcją `@using`:

```razor
@using BlazorSample.Shared

This is the Index page.

<NavMenu></NavMenu>
```

Do składników można także odwoływać się za pomocą ich w pełni kwalifikowanych nazw, które nie wymagają dyrektywy [`@using`](xref:mvc/views/razor#using) :

```razor
This is the Index page.

<BlazorSample.Shared.NavMenu></BlazorSample.Shared.NavMenu>
```

> [!NOTE]
> Kwalifikacja `global::` nie jest obsługiwana.
>
> Importowanie składników za pomocą instrukcji `using` z aliasami (na przykład `@using Foo = Bar`) nie jest obsługiwane.
>
> Częściowo kwalifikowane nazwy nie są obsługiwane. Na przykład dodawanie `@using BlazorSample` i odwoływanie się do `NavMenu.razor` za pomocą `<Shared.NavMenu></Shared.NavMenu>` nie jest obsługiwane.

## <a name="conditional-html-element-attributes"></a>Warunkowe atrybuty elementu HTML

Atrybuty elementu HTML są warunkowo renderowane na podstawie wartości .NET. Jeśli wartość jest `false` lub `null`, atrybut nie jest renderowany. Jeśli wartość jest `true`, atrybut jest renderowany jako zminimalizowany.

W poniższym przykładzie `IsCompleted` określa, czy `checked` jest renderowany w znacznikach elementu:

```razor
<input type="checkbox" checked="@IsCompleted" />

@code {
    [Parameter]
    public bool IsCompleted { get; set; }
}
```

Jeśli `IsCompleted` jest `true`, pole wyboru jest renderowane jako:

```html
<input type="checkbox" checked />
```

Jeśli `IsCompleted` jest `false`, pole wyboru jest renderowane jako:

```html
<input type="checkbox" />
```

Aby uzyskać więcej informacji, zobacz <xref:mvc/views/razor>.

> [!WARNING]
> Niektóre atrybuty HTML, takie jak [Aria](https://developer.mozilla.org/docs/Web/Accessibility/ARIA/Roles/button_role#Toggle_buttons), nie działają prawidłowo, gdy typem .net jest `bool`. W tych przypadkach Użyj typu `string` zamiast `bool`.

## <a name="raw-html"></a>Nieprzetworzony kod HTML

Ciągi są zwykle renderowane przy użyciu węzłów tekstowych DOM, co oznacza, że wszystkie znaczniki, które mogą zawierać, są ignorowane i traktowane jako tekst literału. Aby renderować nieprzetworzony kod HTML, zawiń zawartość HTML w `MarkupString` wartość. Wartość jest analizowana jako plik HTML lub SVG i wstawiona do modelu DOM.

> [!WARNING]
> Renderowanie nieprzetworzonego kodu HTML zbudowanego z dowolnego niezaufanego źródła stanowi **zagrożenie bezpieczeństwa** i należy je unikać!

Poniższy przykład ilustruje użycie typu `MarkupString`, aby dodać blok statycznej zawartości HTML do renderowanego danych wyjściowych składnika:

```html
@((MarkupString)_myMarkup)

@code {
    private string _myMarkup = 
        "<p class='markup'>This is a <em>markup string</em>.</p>";
}
```

## <a name="cascading-values-and-parameters"></a>Wartości kaskadowe i parametry

W niektórych scenariuszach nie można przepływać danych z składnika nadrzędnego do składnika potomnego przy użyciu [parametrów składnika](#component-parameters), zwłaszcza gdy istnieje kilka warstw składników. Wartości kaskadowe i parametry rozwiązują ten problem, zapewniając wygodną metodę dla składnika nadrzędnego, aby zapewnić wartość wszystkim jej składnikom potomnym. Kaskadowe wartości i parametry również zapewniają podejście do współrzędnych składników.

### <a name="theme-example"></a>Przykład motywu

W poniższym przykładzie z przykładowej aplikacji Klasa `ThemeInfo` określa informacje o motywie, aby przetworzyć hierarchię składników w taki sposób, aby wszystkie przyciski w danej części aplikacji miały ten sam styl.

*UIThemeClasses/themeinfo wskazuje. cs*:

```csharp
public class ThemeInfo
{
    public string ButtonClass { get; set; }
}
```

Składnik nadrzędny może zapewnić kaskadową wartość przy użyciu składnika wartości kaskadowych. Składnik `CascadingValue` zawija poddrzewo hierarchii składników i dostarcza jedną wartość do wszystkich składników w tym poddrzewie.

Przykładowo aplikacja Przykładowa określa informacje o motywie (`ThemeInfo`) w jednej z układów aplikacji jako parametr kaskadowy dla wszystkich składników, które tworzą treść układu właściwości `@Body`. `ButtonClass` ma przypisaną wartość `btn-success` w składniku układu. Każdy składnik podrzędny może wykorzystać tę właściwość za pomocą obiektu kaskadowego `ThemeInfo`.

składnik `CascadingValuesParametersLayout`:

```razor
@inherits LayoutComponentBase
@using BlazorSample.UIThemeClasses

<div class="container-fluid">
    <div class="row">
        <div class="col-sm-3">
            <NavMenu />
        </div>
        <div class="col-sm-9">
            <CascadingValue Value="_theme">
                <div class="content px-4">
                    @Body
                </div>
            </CascadingValue>
        </div>
    </div>
</div>

@code {
    private ThemeInfo _theme = new ThemeInfo { ButtonClass = "btn-success" };
}
```

Aby korzystać z wartości kaskadowych, składniki deklarują kaskadowe parametry przy użyciu atrybutu `[CascadingParameter]`. Wartości kaskadowe są powiązane z parametrami kaskadowymi według typu.

W przykładowej aplikacji składnik `CascadingValuesParametersTheme` wiąże `ThemeInfo` wartość kaskadową z parametrem kaskadowym. Parametr służy do ustawiania klasy CSS dla jednego z przycisków wyświetlanych przez składnik.

składnik `CascadingValuesParametersTheme`:

```razor
@page "/cascadingvaluesparameterstheme"
@layout CascadingValuesParametersLayout
@using BlazorSample.UIThemeClasses

<h1>Cascading Values & Parameters</h1>

<p>Current count: @_currentCount</p>

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
    private int _currentCount = 0;

    [CascadingParameter]
    protected ThemeInfo ThemeInfo { get; set; }

    private void IncrementCount()
    {
        _currentCount++;
    }
}
```

Aby przetworzyć kaskadowo wiele wartości tego samego typu w ramach tego samego poddrzewa, podaj unikatowy ciąg `Name` do każdego składnika `CascadingValue` i odpowiadający mu `CascadingParameter`. W poniższym przykładzie dwa składniki `CascadingValue` są kaskadowo różne wystąpienia `MyCascadingType` według nazwy:

```razor
<CascadingValue Value=@_parentCascadeParameter1 Name="CascadeParam1">
    <CascadingValue Value=@ParentCascadeParameter2 Name="CascadeParam2">
        ...
    </CascadingValue>
</CascadingValue>

@code {
    private MyCascadingType _parentCascadeParameter1;

    [Parameter]
    public MyCascadingType ParentCascadeParameter2 { get; set; }

    ...
}
```

W składniku potomnym, kaskadowe parametry odbierają swoje wartości z odpowiednich kaskadowych wartości w składniku nadrzędnym według nazwy:

```razor
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

Przykładowa aplikacja ma interfejs `ITab`, w którym znajdują się karty implementacji:

[!code-csharp[](common/samples/3.x/BlazorWebAssemblySample/UIInterfaces/ITab.cs)]

Składnik `CascadingValuesParametersTabSet` używa składnika `TabSet`, który zawiera kilka `Tab` składników:

```razor
<TabSet>
    <Tab Title="First tab">
        <h4>Greetings from the first tab!</h4>

        <label>
            <input type="checkbox" @bind="showThirdTab" />
            Toggle third tab
        </label>
    </Tab>
    <Tab Title="Second tab">
        <h4>The second tab says Hello World!</h4>
    </Tab>

    @if (showThirdTab)
    {
        <Tab Title="Third tab">
            <h4>Welcome to the disappearing third tab!</h4>
            <p>Toggle this tab from the first tab.</p>
        </Tab>
    }
</TabSet>
```

Podrzędne składniki `Tab` nie są jawnie przenoszone jako parametry do `TabSet`. Zamiast tego podrzędne składniki `Tab` są częścią zawartości podrzędnej `TabSet`. Jednak `TabSet` nadal muszą znać każdy składnik `Tab`, aby można było renderować nagłówki i aktywną kartę. Aby umożliwić tę koordynację bez konieczności stosowania dodatkowego kodu, składnik `TabSet` *może sam określić jako wartość kaskadową* , która jest następnie pobierana przez składniki `Tab` potomnych.

składnik `TabSet`:

[!code-razor[](common/samples/3.x/BlazorWebAssemblySample/Components/TabSet.razor)]

Składniki `Tab` potomne przechwytują `TabSet` zawierający jako parametr kaskadowy, więc składniki `Tab` dodają same do `TabSet` i koordynują, na której karcie jest aktywna.

składnik `Tab`:

[!code-razor[](common/samples/3.x/BlazorWebAssemblySample/Components/Tab.razor)]

## <a name="razor-templates"></a>Szablony Razor

Fragmenty renderowania można definiować przy użyciu składni szablonu Razor. Szablony Razor są sposobem definiowania fragmentu interfejsu użytkownika i przyjmuje następujący format:

```razor
@<{HTML tag}>...</{HTML tag}>
```

Poniższy przykład ilustruje sposób określania wartości `RenderFragment` i `RenderFragment<T>` oraz renderowania szablonów bezpośrednio w składniku. Fragmenty renderowania mogą być również przekazane jako argumenty do [składników z szablonem](xref:blazor/templated-components).

```razor
@_timeTemplate

@_petTemplate(new Pet { Name = "Rex" })

@code {
    private RenderFragment _timeTemplate = @<p>The time is @DateTime.Now.</p>;
    private RenderFragment<Pet> _petTemplate = (pet) => @<p>Pet: @pet.Name</p>;

    private class Pet
    {
        public string Name { get; set; }
    }
}
```

Renderowane dane wyjściowe poprzedniego kodu:

```html
<p>The time is 10/04/2018 01:26:52.</p>

<p>Pet: Rex</p>
```

## <a name="scalable-vector-graphics-svg-images"></a>Skalowalne obrazy wektorowe (SVG)

Ponieważ Blazor renderuje HTML, obrazy obsługiwane przez przeglądarkę, w tym obrazy*SVG (Scalable*Vector Graphics), są obsługiwane za pośrednictwem tagu `<img>`:

```html
<img alt="Example image" src="some-image.svg" />
```

Podobnie Obrazy SVG są obsługiwane w regułach CSS pliku arkusza stylów (*CSS*):

```css
.my-element {
    background-image: url("some-image.svg");
}
```

Jednak wbudowane znaczniki SVG nie są obsługiwane we wszystkich scenariuszach. Jeśli umieścisz tag `<svg>` bezpośrednio w pliku składnika ( *. Razor*), podstawowe renderowanie obrazu jest obsługiwane, ale wiele scenariuszy zaawansowanych nie jest jeszcze obsługiwanych. Na przykład Tagi `<use>` nie są obecnie przestrzegane i `@bind` nie mogą być używane w przypadku niektórych tagów SVG. Oczekujemy, że te ograniczenia są opisane w przyszłej wersji.

## <a name="additional-resources"></a>Dodatkowe zasoby

* <xref:security/blazor/server> &ndash; zawiera wskazówki dotyczące tworzenia aplikacji Blazor Server, które muszą będą konkurować o z wyczerpaniem zasobów.
