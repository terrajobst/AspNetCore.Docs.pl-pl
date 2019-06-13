---
title: Dokumentacja składni razor dla platformy ASP.NET Core
author: rick-anderson
description: Więcej informacji na temat składni znacznikowania Razor do osadzania kodu na serwerze do stron sieci Web.
ms.author: riande
ms.date: 06/12/2019
uid: mvc/views/razor
ms.openlocfilehash: 87c5b97a653c139b8b79f4270e0d9d0081815433
ms.sourcegitcommit: 335a88c1b6e7f0caa8a3a27db57c56664d676d34
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/12/2019
ms.locfileid: "67034940"
---
# <a name="razor-syntax-reference-for-aspnet-core"></a>Dokumentacja składni razor dla platformy ASP.NET Core

Przez [Rick Anderson](https://twitter.com/RickAndMSFT), [Luke Latham](https://github.com/guardrex), [MULLENA Taylora](https://twitter.com/ntaylormullen), i [Dan Vicarel](https://github.com/Rabadash8820)

Razor jest znaczników składnię osadzania kodu na serwerze w stronach sieci Web. Składnia Razor składa się z znaczników Razor C#, jak i HTML. Pliki zawierające Razor zazwyczaj mają *.cshtml* rozszerzenie pliku.

## <a name="rendering-html"></a>Renderowanie kodu HTML

Domyślny język Razor ma format HTML. Renderowanie kodu HTML, z kodu znaczników Razor nie różni się od renderowania kodu HTML z pliku HTML. Kod znaczników HTML w *.cshtml* plikach Razor jest renderowany przez serwer bez zmian.

## <a name="razor-syntax"></a>Składnia razor

Obsługuje razor C# i używa `@` symbol przejście z kodu HTML do C#. Ocenia razor C# wyrażeń i renderuje je w danych wyjściowych HTML.

Gdy `@` następuje symbol [Razor zastrzeżone słowa kluczowego](#razor-reserved-keywords), przechodzi do znaczników specyficzne dla aparatu Razor. W przeciwnym razie nastąpi przejście do zwykłego C#.

Wyjścia `@` symboli w znacznikach Razor, należy użyć drugiej `@` symbol:

```cshtml
<p>@@Username</p>
```

Renderowanie kodu HTML za pomocą jednego `@` symbol:

```html
<p>@Username</p>
```

Atrybuty kodu HTML i zawartość zawierająca adresy e-mail nie traktują `@` symbol znaku przejścia. Adresy e-mail w poniższym przykładzie są niezmienione, analiza kodu Razor:

```cshtml
<a href="mailto:Support@contoso.com">Support@contoso.com</a>
```

## <a name="implicit-razor-expressions"></a>Niejawne wyrażeń Razor

Niejawne wyrażeń Razor rozpoczynać `@` następuje C# kodu:

```cshtml
<p>@DateTime.Now</p>
<p>@DateTime.IsLeapYear(2016)</p>
```

Z wyjątkiem produktów C# `await` — słowo kluczowe, niejawnego wyrażenia nie może zawierać spacji. Jeśli C# instrukcji posiada wyraźne zakończenie, miejsca do magazynowania można są:

```cshtml
<p>@await DoSomething("hello", "world")</p>
```

Niejawne wyrażeń **nie** zawierają C# ogólne jako znaki w nawiasie (`<>`) są interpretowane jako potraktowane jak tag HTML. Poniższy kod jest **nie** prawidłowy:

```cshtml
<p>@GenericMethod<int>()</p>
```

Powyższy kod generuje błąd kompilatora podobny do jednego z następujących czynności:

* Element "int" nie został zamknięty. Wszystkie elementy muszą być albo samozamykający lub ma zgodnego tagu końcowego.
* Nie można przekonwertować grupy metod "GenericMethod" na typ "object" Niedelegowany. Czy zamierzane było wywołanie metody? "

Wywołania metody ogólnej musi być ujęte w [wyrażenie jawne Razor](#explicit-razor-expressions) lub [blok kodu Razor](#razor-code-blocks).

## <a name="explicit-razor-expressions"></a>Jawne Razor wyrażeń

Jawne Razor wyrażeń składają się z `@` symbol za pomocą nawiasów o zrównoważonym obciążeniu. Aby renderować czas ostatniego tygodnia, jest używany następujący kod Razor:

```cshtml
<p>Last week this time: @(DateTime.Now - TimeSpan.FromDays(7))</p>
```

Zawartości w obrębie `@()` nawiasów jest obliczane i renderowane w danych wyjściowych.

Niejawne wyrażeń, opisanego w poprzedniej sekcji, zazwyczaj nie może zawierać spacji. W poniższym kodzie jeden tydzień nie jest odejmowany od bieżącej godziny:

[!code-cshtml[](razor/sample/Views/Home/Contact.cshtml?range=17)]

Ten kod Renderuje poniższy kod HTML:

```html
<p>Last week: 7/7/2016 4:39:52 PM - TimeSpan.FromDays(7)</p>
```

Jawne wyrażeń może służyć do łączenia tekstu z wynikiem wyrażenia:

```cshtml
@{
    var joe = new Person("Joe", 33);
}

<p>Age@(joe.Age)</p>
```

Bez wyraźnej wyrażenia `<p>Age@joe.Age</p>` jest traktowany jak adres e-mail i `<p>Age@joe.Age</p>` jest renderowany. Podczas zapisywania jako wyrażenie jawne `<p>Age33</p>` jest renderowany.

Jawne wyrażeń może zostać użyty do renderowania dane wyjściowe z metod ogólnych w *.cshtml* plików. Następujący kod pokazuje, jak rozwiązać błąd pokazany wcześniej spowodowane przez nawiasy z C# ogólnego. Kod jest zapisywany jako jawne wyrażenie:

```cshtml
<p>@(GenericMethod<int>())</p>
```

## <a name="expression-encoding"></a>Wyrażenie kodowania

C#wyrażenia, które obliczają do ciągu są kodowania HTML. C#wyrażenia, które dają `IHtmlContent` — zostaną zrenderowane bezpośrednio za pomocą `IHtmlContent.WriteTo`. C#wyrażenia, które nie zwrócą `IHtmlContent` są konwertowane na ciąg przez `ToString` i kodowany przed są one renderowane.

```cshtml
@("<span>Hello World</span>")
```

Ten kod Renderuje poniższy kod HTML:

```html
&lt;span&gt;Hello World&lt;/span&gt;
```

Kod HTML jest wyświetlany w przeglądarce jako:

```
<span>Hello World</span>
```

`HtmlHelper.Raw` dane wyjściowe nie jest zakodowany, ale renderowany jako kod znaczników HTML.

> [!WARNING]
> Za pomocą `HtmlHelper.Raw` unsanitized użytkownika dane wejściowe to zagrożenie bezpieczeństwa. Dane wejściowe użytkownika może zawierać złośliwego kodu JavaScript lub inne luki w zabezpieczeniach. Podczas czyszczenia danych wejściowych użytkownika jest trudne. Unikaj używania `HtmlHelper.Raw` z danymi wejściowymi użytkownika.

```cshtml
@Html.Raw("<span>Hello World</span>")
```

Ten kod Renderuje poniższy kod HTML:

```html
<span>Hello World</span>
```

## <a name="razor-code-blocks"></a>Bloki kodu razor

Bloki kodu razor rozpoczynać `@` i są ujęte w `{}`. W przeciwieństwie do wyrażenia C# kod wewnątrz bloków kodu nie jest renderowany. Bloki kodu i wyrażenia w widoku udostępnianie tego samego zakresu i są definiowane w kolejności:

```cshtml
@{
    var quote = "The future depends on what you do today. - Mahatma Gandhi";
}

<p>@quote</p>

@{
    quote = "Hate cannot drive out hate, only love can do that. - Martin Luther King, Jr.";
}

<p>@quote</p>
```

Ten kod Renderuje poniższy kod HTML:

```html
<p>The future depends on what you do today. - Mahatma Gandhi</p>
<p>Hate cannot drive out hate, only love can do that. - Martin Luther King, Jr.</p>
```

::: moniker range=">= aspnetcore-3.0"

W blokach kodu, należy zadeklarować [funkcje lokalne](/dotnet/csharp/programming-guide/classes-and-structs/local-functions) przy użyciu znaczników, która będzie służyć jako metody tworzenia szablonów:

```cshtml
@{
    void RenderName(string name)
    {
        <p>Name: <strong>@name</strong></p>
    }

    RenderName("Mahatma Gandhi");
    RenderName("Martin Luther King, Jr.");
}
```

Ten kod Renderuje poniższy kod HTML:

```html
<p>Name: <strong>Mahatma Gandhi</strong></p>
<p>Name: <strong>Martin Luther King, Jr.</strong></p>
```

::: moniker-end

### <a name="implicit-transitions"></a>Niejawne przejścia

Jest to domyślny język, w bloku kodu C#, ale strona Razor można przejść do kodu HTML:

```cshtml
@{
    var inCSharp = true;
    <p>Now in HTML, was in C# @inCSharp</p>
}
```

### <a name="explicit-delimited-transition"></a>Jawne przejście rozdzielany

Aby zdefiniować podsekcję blok kodu, które mają renderować kod HTML, Otocz znaków do renderowania elementu Razor  **\<tekst >** tag:

```cshtml
@for (var i = 0; i < people.Length; i++)
{
    var person = people[i];
    <text>Name: @person.Name</text>
}
```

Tej metody można użyć do renderowania elementów HTML, który nie jest otoczony potraktowane jak tag HTML. Bez tagu HTML lub Razor występuje błąd w czasie wykonywania Razor.

**\<Tekst >** tag jest przydatne do kontroli odstępu, podczas renderowania zawartości:

* Tylko zawartości między  **\<tekst >** renderowanego tagu.
* Nie spacji przed ani po  **\<tekst >** tagów jest wyświetlana w danych wyjściowych HTML.

### <a name="explicit-line-transition-with-"></a>Jawne wiersz przejścia z @:

Aby renderować pozostałą część całego wiersza jako HTML wewnątrz bloku kodu, należy użyć `@:` składni:

```cshtml
@for (var i = 0; i < people.Length; i++)
{
    var person = people[i];
    @:Name: @person.Name
}
```

Bez `@:` w kodzie, jest generowany błąd czasu wykonywania Razor.

Ostrzeżenie: Dodatkowe `@` znaków w pliku Razor może spowodować błędy kompilatora w instrukcji w dalszej części tego bloku. Te błędy kompilatora może być trudne do zrozumienia, ponieważ rzeczywista błąd wystąpi przed zgłoszonego błędu. Ten błąd jest wspólne po połączeniu wiele wyrażeń niejawnej/jawnej w jednym bloku kodu.

## <a name="control-structures"></a>Struktury sterujące

Struktury sterujące stanowią rozszerzenie bloków kodu. Wszystkie aspekty bloki kodu (przejście do znaczników, wbudowane C#) dotyczą również następujące struktury:

### <a name="conditionals-if-else-if-else-and-switch"></a>Instrukcje warunkowe @if, jeśli nie, inny, i @switch

`@if` formanty, gdy kod jest wykonywany:

```cshtml
@if (value % 2 == 0)
{
    <p>The value was even.</p>
}
```

`else` i `else if` nie wymagają `@` symbol:

```cshtml
@if (value % 2 == 0)
{
    <p>The value was even.</p>
}
else if (value >= 1337)
{
    <p>The value is large.</p>
}
else
{
    <p>The value is odd and small.</p>
}
```

Następujący kod przedstawia sposób użycia instrukcji switch:

```cshtml
@switch (value)
{
    case 1:
        <p>The value is 1!</p>
        break;
    case 1337:
        <p>Your number is 1337!</p>
        break;
    default:
        <p>Your number wasn't 1 or 1337.</p>
        break;
}
```

### <a name="looping-for-foreach-while-and-do-while"></a>Tworzenie pętli @for, @foreach, @while, i @do podczas

Oparte na szablonach HTML może być renderowany przy użyciu pętli instrukcji sterowania. Aby renderować listy osób:

```cshtml
@{
    var people = new Person[]
    {
          new Person("Weston", 33),
          new Person("Johnathon", 41),
          ...
    };
}
```

Obsługiwane są następujące instrukcje pętli:

`@for`

```cshtml
@for (var i = 0; i < people.Length; i++)
{
    var person = people[i];
    <p>Name: @person.Name</p>
    <p>Age: @person.Age</p>
}
```

`@foreach`

```cshtml
@foreach (var person in people)
{
    <p>Name: @person.Name</p>
    <p>Age: @person.Age</p>
}
```

`@while`

```cshtml
@{ var i = 0; }
@while (i < people.Length)
{
    var person = people[i];
    <p>Name: @person.Name</p>
    <p>Age: @person.Age</p>

    i++;
}
```

`@do while`

```cshtml
@{ var i = 0; }
@do
{
    var person = people[i];
    <p>Name: @person.Name</p>
    <p>Age: @person.Age</p>

    i++;
} while (i < people.Length);
```

### <a name="compound-using"></a>Złożona (c) @using

W C#, `using` instrukcja jest używane, aby upewnić się, obiekt zostanie usunięty. W elemencie Razor ten sam mechanizm służy do tworzenia pomocników HTML, który zawiera dodatkową zawartość. W poniższym kodzie pomocników HTML, renderowania tag formularza z `@using` instrukcji:

```cshtml
@using (Html.BeginForm())
{
    <div>
        email:
        <input type="email" id="Email" value="">
        <button>Register</button>
    </div>
}
```

Można wykonywać działania na poziomie zakresu za pomocą [pomocników tagów](xref:mvc/views/tag-helpers/intro).

### <a name="try-catch-finally"></a>@try, catch finally

Obsługa wyjątków jest podobne do C#:

[!code-cshtml[](razor/sample/Views/Home/Contact7.cshtml)]

### <a name="lock"></a>@lock

Razor ma możliwość ochrony sekcje krytyczne w instrukcjach blokady:

```cshtml
@lock (SomeLock)
{
    // Do critical section work
}
```

### <a name="comments"></a>Komentarze

Obsługuje razor C# i komentarze HTML:

```cshtml
@{
    /* C# comment */
    // Another C# comment
}
<!-- HTML comment -->
```

Ten kod Renderuje poniższy kod HTML:

```html
<!-- HTML comment -->
```

Komentarz razor są usuwane przez serwer przed wyświetleniem strony sieci Web. Korzysta z aparatu razor `@*  *@` ograniczającej komentarzy. Poniższy kod jest zakomentowany, więc serwer nie renderuje żadnych znaczników:

```cshtml
@*
    @{
        /* C# comment */
        // Another C# comment
    }
    <!-- HTML comment -->
*@
```

## <a name="directives"></a>Dyrektyw

Dyrektywy razor są reprezentowane przez wyrażenia niejawne następujących zastrzeżonych słów kluczowych `@` symboli. Dyrektywy zazwyczaj zmienia sposób, w widoku jest analizowany lub włącza inną funkcję.

Zrozumienie, jak Razor generuje kod dla widoku ułatwia zrozumienie, jak działają dyrektywy.

[!code-cshtml[](razor/sample/Views/Home/Contact8.cshtml)]

Kod generuje klasę podobny do następującego:

```csharp
public class _Views_Something_cshtml : RazorPage<dynamic>
{
    public override async Task ExecuteAsync()
    {
        var output = "Getting old ain't for wimps! - Anonymous";

        WriteLiteral("/r/n<div>Quote of the Day: ");
        Write(output);
        WriteLiteral("</div>");
    }
}
```

W dalszej części tego artykułu sekcji [sprawdzić Razor C# klasy wygenerowanej w celu wyświetlenia](#inspect-the-razor-c-class-generated-for-a-view) opisano sposób wyświetlania tego wygenerowanej klasy.

<a name="using"></a>

### <a name="using"></a>@using

`@using` Dodaje dyrektywy C# `using` dyrektywy do wygenerowanego widoku:

[!code-cshtml[](razor/sample/Views/Home/Contact9.cshtml)]

### <a name="model"></a>@model

`@model` Dyrektywa określa typ modelu przekazywane do widoku:

```cshtml
@model TypeNameOfModel
```

W aplikacji ASP.NET Core MVC utworzonych za pomocą indywidualnych kont użytkowników *Views/Account/Login.cshtml* widok zawiera następującą deklarację modelu:

```cshtml
@model LoginViewModel
```

Dziedziczy po klasie, wygenerowany `RazorPage<dynamic>`:

```csharp
public class _Views_Account_Login_cshtml : RazorPage<LoginViewModel>
```

Udostępnia razor `Model` właściwości do uzyskiwania dostępu do modelu przekazywane do widoku:

```cshtml
<div>The Login Email: @Model.Email</div>
```

`@model` Dyrektywa określa typ tej właściwości. Dyrektywa określa `T` w `RazorPage<T>` czy wygenerowanej klasy, która widok pochodzi od klasy. Jeśli `@model` dyrektywy nie jest określona, `Model` właściwość jest typu `dynamic`. Wartość modelu jest przekazywany z kontrolera do widoku. Aby uzyskać więcej informacji, zobacz [silnie typizowane modeli i &commat;modelu — słowo kluczowe](xref:tutorials/first-mvc-app/adding-model#strongly-typed-models-and-the--keyword).

### <a name="inherits"></a>@inherits

`@inherits` Dyrektywy umożliwia pełną kontrolę nad klasa dziedziczy widoku:

```cshtml
@inherits TypeNameOfClassToInheritFrom
```

Poniższy kod jest niestandardowy typ strony Razor:

[!code-csharp[](razor/sample/Classes/CustomRazorPage.cs)]

`CustomText` Jest wyświetlany w widoku:

[!code-cshtml[](razor/sample/Views/Home/Contact10.cshtml)]

Ten kod Renderuje poniższy kod HTML:

```html
<div>Custom text: Gardyloo! - A Scottish warning yelled from a window before dumping a slop bucket on the street below.</div>
```

 `@model` i `@inherits` mogą być używane w jednym widoku. `@inherits` mogą znajdować się w *_ViewImports.cshtml* pliku, który importuje widoku:

[!code-cshtml[](razor/sample/Views/_ViewImportsModel.cshtml)]

Poniższy kod jest przykładem silnie typizowane widoku:

[!code-cshtml[](razor/sample/Views/Home/Login1.cshtml)]

Jeśli "rick@contoso.com" jest przekazywany w modelu widoku generuje następujący kod znaczników HTML:

```html
<div>The Login Email: rick@contoso.com</div>
<div>Custom text: Gardyloo! - A Scottish warning yelled from a window before dumping a slop bucket on the street below.</div>
```

### <a name="inject"></a>@inject

`@inject` Dyrektywy umożliwia strony Razor, aby wstawić usługi z [kontener usługi](xref:fundamentals/dependency-injection) w widoku. Aby uzyskać więcej informacji, zobacz [wstrzykiwanie zależności do widoków](xref:mvc/views/dependency-injection).

### <a name="functions"></a>@functions

`@functions` Dyrektywy umożliwia stronę Razor, aby dodać C# blok kodu do widoku:

```cshtml
@functions { // C# Code }
```

Na przykład:

[!code-cshtml[](razor/sample/Views/Home/Contact6.cshtml)]

Kod generuje następujący kod znaczników HTML:

```html
<div>From method: Hello</div>
```

Poniższy kod jest wygenerowany Razor C# klasy:

[!code-csharp[](razor/sample/Classes/Views_Home_Test_cshtml.cs?range=1-19)]

::: moniker range=">= aspnetcore-3.0"

`@functions` metody służą jako metody tworzenia szablonów, gdy kod znaczników:

```cshtml
@{
    RenderName("Mahatma Gandhi");
    RenderName("Martin Luther King, Jr.");
}

@functions {
    private void RenderName(string name)
    {
        <p>Name: <strong>@name</strong></p>
    }
}
```

Ten kod Renderuje poniższy kod HTML:

```html
<p>Name: <strong>Mahatma Gandhi</strong></p>
<p>Name: <strong>Martin Luther King, Jr.</strong></p>
```

::: moniker-end

### <a name="attribute"></a>@attribute

`@attribute` Dyrektywy dodaje nadano mu atrybut klasy wygenerowanej strony lub widoku. W poniższym przykładzie dodano `[Authorize]` atrybutu:

```cshtml
@attribute [Authorize]
```

> [!WARNING]
> W wersji platformy ASP.NET Core 3.0 w wersji zapoznawczej 6, jest to znany problem gdzie `@attribute` dyrektywy nie działają w  *\_Imports.razor* i  *\_ViewImports.cshtml* plików. Ten problem zostanie rozwiązany w wersji 7 (wersja zapoznawcza).

### <a name="namespace"></a>@namespace

`@namespace` Dyrektywa określa przestrzeń nazw, klasy wygenerowanej strony lub widoku:

```cshtml
@namespace Your.Namespace.Here
```

Jeśli strony lub widoku importuje interfejsu API za pomocą `@namespace` dyrektywy, przestrzeń nazw oryginalnego pliku ustawiono względem tej przestrzeni nazw. 

Jeśli *MyApp/stron/\_ViewImports.cshtml* zawiera `@namespace Hello.World`, stron lub widoki, które importują przestrzeń nazw `Hello.World` przestrzeni nazw jest ustawiona, jak pokazano w poniższej tabeli.

| Strona (lub widoku)                     | Przestrzeń nazw               |
| ---------------------------------- | ----------------------- |
| *MyApp/Pages/Index.cshtml*         | `Hello.World`           |
| *MyApp/Pages/MorePages/Bar.cshtml* | `Hello.World.MorePages` |

Jeśli masz wiele plików importu `@namespace` dyrektywy, zostanie użyty plik najbliższego widoku w łańcuchu katalogu lub strony.

### <a name="section"></a>@section

`@section` Dyrektywa jest używany w połączeniu z [układ](xref:mvc/views/layout) umożliwiające stron lub widoków w celu renderowania zawartości w różnych częściach strony HTML. Aby uzyskać więcej informacji, zobacz [sekcje](xref:mvc/views/layout#layout-sections-label).

## <a name="templated-razor-delegates"></a>Oparte na szablonach delegatów Razor

Szablony razor umożliwiają definiowanie fragmentu interfejsu użytkownika w następującym formacie:

```cshtml
@<tag>...</tag>
```

Poniższy przykład ilustruje sposób określania oparte na szablonach delegata Razor jako <xref:System.Func%602>. [Typu dynamicznego](/dotnet/csharp/programming-guide/types/using-type-dynamic) jest określona dla parametru metody, która hermetyzuje delegata. [Typ obiektu](/dotnet/csharp/language-reference/keywords/object) jest określony jako wartość zwracaną przez delegata. Ten szablon jest używany z <xref:System.Collections.Generic.List%601> z `Pet` zawierający `Name` właściwości.

```csharp
public class Pet
{
    public string Name { get; set; }
}
```

```cshtml
@{
    Func<dynamic, object> petTemplate = @<p>You have a pet named <strong>@item.Name</strong>.</p>;

    var pets = new List<Pet>
    {
        new Pet { Name = "Rin Tin Tin" },
        new Pet { Name = "Mr. Bigglesworth" },
        new Pet { Name = "K-9" }
    };
}
```

Szablon jest renderowany przy użyciu `pets` dostarczonych przez `foreach` instrukcji:

```cshtml
@foreach (var pet in pets)
{
    @petTemplate(pet)
}
```

Wyniku renderowania:

```html
<p>You have a pet named <strong>Rin Tin Tin</strong>.</p>
<p>You have a pet named <strong>Mr. Bigglesworth</strong>.</p>
<p>You have a pet named <strong>K-9</strong>.</p>
```

Możesz również dostarczyć wbudowany szablon Razor, jako argument do metody. W poniższym przykładzie `Repeat` metoda odbiera szablon Razor. Metoda używa tego szablonu w celu wygenerowania zawartość HTML z powtórzeń elementy dostarczane z listy:

```cshtml
@using Microsoft.AspNetCore.Html

@functions {
    public static IHtmlContent Repeat(IEnumerable<dynamic> items, int times,
        Func<dynamic, IHtmlContent> template)
    {
        var html = new HtmlContentBuilder();

        foreach (var item in items)
        {
            for (var i = 0; i < times; i++)
            {
                html.AppendHtml(template(item));
            }
        }

        return html;
    }
}
```

Za pomocą listę zwierząt domowych, z poprzedniego przykładu `Repeat` metoda jest wywoływana z:

* <xref:System.Collections.Generic.List%601> z `Pet`.
* Liczba powtórzeń każdego pet.
* Wbudowany szablon do użycia dla elementów listy, listy nieuporządkowane.

```cshtml
<ul>
    @Repeat(pets, 3, @<li>@item.Name</li>)
</ul>
```

Wyniku renderowania:

```html
<ul>
    <li>Rin Tin Tin</li>
    <li>Rin Tin Tin</li>
    <li>Rin Tin Tin</li>
    <li>Mr. Bigglesworth</li>
    <li>Mr. Bigglesworth</li>
    <li>Mr. Bigglesworth</li>
    <li>K-9</li>
    <li>K-9</li>
    <li>K-9</li>
</ul>
```

## <a name="tag-helpers"></a>Pomocnicy tagów

Istnieją trzy dyrektyw, które odnoszą się do [pomocników tagów](xref:mvc/views/tag-helpers/intro).

| — Dyrektywa | Funkcja |
| --------- | -------- |
| [&commat;addTagHelper](xref:mvc/views/tag-helpers/intro#add-helper-label) | Udostępnia pomocników tagów do widoku. |
| [&commat;removeTagHelper](xref:mvc/views/tag-helpers/intro#remove-razor-directives-label) | Usuwa pomocnicy tagów, wcześniej dodany z widoku. |
| [&commat;tagHelperPrefix](xref:mvc/views/tag-helpers/intro#prefix-razor-directives-label) | Określa prefiks tagu, aby włączyć obsługę pomocnika tagów i Pomocnik tagu użycie jawnego. |

## <a name="razor-reserved-keywords"></a>Razor zastrzeżone słowa kluczowe

### <a name="razor-keywords"></a>Słowa kluczowe razor

* Strona (wymaga platformy ASP.NET Core 2.0 i nowsze)
* — przestrzeń nazw
* — funkcje
* Inherits
* model
* sekcja
* Pomocnik (nie jest obecnie obsługiwane przez program ASP.NET Core)

Razor słowa kluczowe są poprzedzone znakiem zmiany znaczenia z `@(Razor Keyword)` (na przykład `@(functions)`).

### <a name="c-razor-keywords"></a>C#Słowa kluczowe razor

* case
* do
* default
* dla
* foreach
* if
* else
* lock
* — przełącznik
* Wypróbuj
* CATCH
* finally
* korzystanie
* while

C#Słowa kluczowe razor musi być double ucieczki `@(@C# Razor Keyword)` (na przykład `@(@case)`). Pierwszy `@` specjalne analizator Razor. Drugi `@` specjalne C# analizatora.

### <a name="reserved-keywords-not-used-by-razor"></a>Zastrzeżone słowa kluczowe nie użył ich Razor

* class

## <a name="inspect-the-razor-c-class-generated-for-a-view"></a>Zbadaj Razor C# klasa generowana dla widoku

::: moniker range=">= aspnetcore-2.1"

Za pomocą platformy .NET Core SDK 2.1 lub nowszej [Razor SDK](xref:razor-pages/sdk) obsługuje kompilacji plikach Razor. Podczas kompilowania projektu, generuje zestaw Razor SDK *obj / < build_configuration > / < target_framework_moniker > / Razor* katalogu w folderze głównym projektu. Struktura katalogów w ramach *Razor* katalogu odzwierciedla strukturze katalogu projektu.

Rozważmy następującą strukturę katalogów w projekcie stron Razor programu ASP.NET Core 2.1 przeznaczonych dla platformy .NET Core 2.1:

* **Obszary /**
  * **Administrator /**
    * **Strony /**
      * *Index.cshtml*
      * *Index.cshtml.CS*
* **Strony /**
  * **Udostępnione /**
    * *_Layout.cshtml*
  * *_ViewImports.cshtml*
  * *_ViewStart.cshtml*
  * *Index.cshtml*
  * *Index.cshtml.CS*

Tworzenie projektu na platformie *debugowania* konfiguracji daje następujące *obj* katalogu:

* **obj /**
  * **Debugowanie /**
    * **netcoreapp2.1 /**
      * **Razor /**
        * **Obszary /**
          * **Administrator /**
            * **Strony /**
              * *Index.g.cshtml.CS*
        * **Strony /**
          * **Udostępnione /**
            * *_Layout.g.cshtml.CS*
          * *_ViewImports.g.cshtml.CS*
          * *_ViewStart.g.cshtml.CS*
          * *Index.g.cshtml.CS*

Aby wyświetlić wygenerowanej klasy dla *Pages/Index.cshtml*, otwórz *obj/Debug/netcoreapp2.1/Razor/Pages/Index.g.cshtml.cs*.

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

Dodaj poniższą klasę do projektu programu ASP.NET Core MVC:

[!code-csharp[](razor/sample/Utilities/CustomTemplateEngine.cs)]

W `Startup.ConfigureServices`, Zastąp `RazorTemplateEngine` dodane przez wzorzec MVC z `CustomTemplateEngine` klasy:

[!code-csharp[](razor/sample/Startup.cs?highlight=4&range=10-14)]

Ustaw punkt przerwania na `return csharpDocument;` instrukcja `CustomTemplateEngine`. Po zatrzymaniu w punkcie przerwania wykonywania programu wyświetlić wartość `generatedCode`.

![Widok Wizualizator tekstu generatedCode](razor/_static/tvr.png)

::: moniker-end

## <a name="view-lookups-and-case-sensitivity"></a>Widok wyszukiwania i rozróżnianie wielkości liter

Aparat widoku Razor wykonuje wyszukiwanie z uwzględnieniem wielkości liter dla widoków. Jednak rzeczywista wyszukiwania jest określany przez sam system plików:

* Na podstawie źródła pliku:
  * W systemach operacyjnych z systemami plików bez uwzględniania wielkości liter (na przykład Windows) plik fizyczny dostawcy wyszukiwania jest rozróżniana wielkość liter. Na przykład `return View("Test")` skutkuje dopasowań dla */Views/Home/Test.cshtml*, */Views/home/test.cshtml*i inne wariant wielkość liter w wyrazie.
  * W systemach plików rozróżniana wielkość liter (na przykład, Linux, OSX i `EmbeddedFileProvider`), wyszukiwania jest rozróżniana wielkość liter. Na przykład `return View("Test")` specjalnie dopasowania */Views/Home/Test.cshtml*.
* Prekompilowany widoki: Za pomocą platformy ASP.NET Core 2.0 i nowszych wyszukiwania prekompilowanego widoków jest uwzględniana wielkość liter, we wszystkich systemach operacyjnych. Zachowanie jest taka sama jak zachowanie dostawcy plików fizycznych w Windows. Jeśli dwa widoki wstępnie skompilowanych różnią się tylko wielkością liter, wynikiem wyszukiwania jest niedeterministyczny.

Deweloperzy są zachęcani do pasuje do wielkości liter w nazwach plików i katalogów na wielkość liter w wyrazie:

* Nazwy obszaru, kontrolera i akcji.
* Strony razor.

Wielkości liter, gwarantuje, że wdrożeń Znajdź swoje opinie, niezależnie od tego, źródłowy system plików.
