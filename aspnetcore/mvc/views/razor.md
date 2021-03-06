---
title: Dokumentacja składni razor dla platformy ASP.NET Core
author: rick-anderson
description: Więcej informacji na temat składni znacznikowania Razor do osadzania kodu na serwerze do stron sieci Web.
ms.author: riande
ms.date: 02/12/2020
uid: mvc/views/razor
ms.openlocfilehash: e9d2e42ba3c36bc1661739f3b105ec8efe03de48
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78658719"
---
# <a name="razor-syntax-reference-for-aspnet-core"></a>Dokumentacja składni razor dla platformy ASP.NET Core

Autorzy [Rick Anderson](https://twitter.com/RickAndMSFT), [Taylor Mullen](https://twitter.com/ntaylormullen)i [Dan Vicarel](https://github.com/Rabadash8820)

Razor jest znaczników składnię osadzania kodu na serwerze w stronach sieci Web. Składnia Razor składa się z znaczników Razor C#, jak i HTML. Pliki zawierające plik Razor zazwyczaj mają rozszerzenie *. cshtml* . Razor również w plikach [składników Razor](xref:blazor/components) ( *. Razor*).

## <a name="rendering-html"></a>Renderowanie kodu HTML

Domyślny język Razor ma format HTML. Renderowanie kodu HTML, z kodu znaczników Razor nie różni się od renderowania kodu HTML z pliku HTML. Znaczniki HTML w plikach Razor *. cshtml* są renderowane przez serwer bez zmian.

## <a name="razor-syntax"></a>Składnia Razor

Razor obsługuje C# i używa symbolu `@`, aby przejść z formatu HTML C#do. Ocenia razor C# wyrażeń i renderuje je w danych wyjściowych HTML.

Gdy po symbolu `@` następuje [słowo kluczowe zarezerwowane Razor](#razor-reserved-keywords), przechodzi do znacznika specyficznego dla Razor. W przeciwnym razie nastąpi przejście do zwykłego C#.

Aby wypróbować symbol `@` w znaczniku Razor, użyj drugiego symbolu `@`:

```cshtml
<p>@@Username</p>
```

Kod jest renderowany w języku HTML z pojedynczym symbolem `@`:

```html
<p>@Username</p>
```

Atrybuty i zawartość HTML zawierające adresy e-mail nie traktują symbolu `@` jako znaku przejścia. Adresy e-mail w poniższym przykładzie są niezmienione, analiza kodu Razor:

```cshtml
<a href="mailto:Support@contoso.com">Support@contoso.com</a>
```

## <a name="implicit-razor-expressions"></a>Niejawne wyrażeń Razor

Niejawne wyrażenia Razor zaczynają się C# od `@` po którym następuje kod:

```cshtml
<p>@DateTime.Now</p>
<p>@DateTime.IsLeapYear(2016)</p>
```

Z wyjątkiem słowa kluczowego C# `await`, wyrażenia niejawne nie mogą zawierać spacji. Jeśli C# instrukcji posiada wyraźne zakończenie, miejsca do magazynowania można są:

```cshtml
<p>@await DoSomething("hello", "world")</p>
```

Wyrażenia niejawne C# **nie mogą** zawierać typów ogólnych, ponieważ znaki wewnątrz nawiasów (`<>`) są interpretowane jako tag HTML. Następujący **kod jest nieprawidłowy** :

```cshtml
<p>@GenericMethod<int>()</p>
```

Powyższy kod generuje błąd kompilatora podobny do jednego z następujących czynności:

* Element "int" nie został zamknięty. Wszystkie elementy muszą być albo samozamykający lub ma zgodnego tagu końcowego.
* Nie można przekonwertować grupy metod "GenericMethod" na typ "object" Niedelegowany. Czy zamierzane było wywołanie metody? "

Wywołania metody ogólnej muszą być opakowane w [jawne wyrażenie Razor](#explicit-razor-expressions) lub [blok kodu Razor](#razor-code-blocks).

## <a name="explicit-razor-expressions"></a>Jawne Razor wyrażeń

Jawne wyrażenia Razor składają się z `@` symbolu z wyrównanym nawiasem. Aby renderować czas ostatniego tygodnia, jest używany następujący kod Razor:

```cshtml
<p>Last week this time: @(DateTime.Now - TimeSpan.FromDays(7))</p>
```

Każda zawartość w nawiasach `@()` jest szacowana i renderowana w danych wyjściowych.

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

Bez wyrażenia jawnego `<p>Age@joe.Age</p>` jest traktowany jako adres e-mail, a `<p>Age@joe.Age</p>` jest renderowany. Gdy jest zapisywana jako wyrażenie jawne, `<p>Age33</p>` jest renderowany.

Wyrażenia jawne mogą służyć do renderowania danych wyjściowych z metod ogólnych w plikach *. cshtml* . Następujący kod pokazuje, jak rozwiązać błąd pokazany wcześniej spowodowane przez nawiasy z C# ogólnego. Kod jest zapisywany jako jawne wyrażenie:

```cshtml
<p>@(GenericMethod<int>())</p>
```

## <a name="expression-encoding"></a>Wyrażenie kodowania

C#wyrażenia, które obliczają do ciągu są kodowania HTML. C#wyrażenia, które mają być `IHtmlContent` są renderowane bezpośrednio przez `IHtmlContent.WriteTo`. C#wyrażenia, które nie są szacowane do `IHtmlContent`, są konwertowane na ciąg przez `ToString` i kodowane przed ich renderowaniem.

```cshtml
@("<span>Hello World</span>")
```

Ten kod Renderuje poniższy kod HTML:

```html
&lt;span&gt;Hello World&lt;/span&gt;
```

Kod HTML jest wyświetlany w przeglądarce jako:

```html
<span>Hello World</span>
```

dane wyjściowe `HtmlHelper.Raw` nie są kodowane, ale renderowane jako znaczniki HTML.

> [!WARNING]
> Używanie `HtmlHelper.Raw` w przypadku nieoczyszczonych danych wejściowych użytkownika jest ryzykowne dla bezpieczeństwa. Dane wejściowe użytkownika może zawierać złośliwego kodu JavaScript lub inne luki w zabezpieczeniach. Podczas czyszczenia danych wejściowych użytkownika jest trudne. Unikaj używania `HtmlHelper.Raw` z danymi wejściowymi użytkownika.

```cshtml
@Html.Raw("<span>Hello World</span>")
```

Ten kod Renderuje poniższy kod HTML:

```html
<span>Hello World</span>
```

## <a name="razor-code-blocks"></a>Bloki kodu razor

Bloki kodu Razor zaczynają się od `@` i są ujęte w `{}`. W przeciwieństwie do wyrażenia C# kod wewnątrz bloków kodu nie jest renderowany. Bloki kodu i wyrażenia w widoku udostępnianie tego samego zakresu i są definiowane w kolejności:

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

W blokach kodu Zadeklaruj [funkcje lokalne](/dotnet/csharp/programming-guide/classes-and-structs/local-functions) z adiustacją, aby zapewnić metody tworzenia szablonów:

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

Aby zdefiniować podsekcję bloku kodu, który powinien renderować kod HTML, należy ująć znaki do renderowania za pomocą tagu `<text>` Razor:

```cshtml
@for (var i = 0; i < people.Length; i++)
{
    var person = people[i];
    <text>Name: @person.Name</text>
}
```

Tej metody można użyć do renderowania elementów HTML, który nie jest otoczony potraktowane jak tag HTML. Bez tagu HTML lub Razor występuje błąd w czasie wykonywania Razor.

Tag `<text>` jest przydatny do kontrolowania odstępów podczas renderowania zawartości:

* Tylko zawartość między tagiem `<text>` jest renderowana.
* W danych wyjściowych HTML nie ma spacji przed ani po tagu `<text>`.

### <a name="explicit-line-transition"></a>Jawne przejście liniowe

Aby renderować resztę całego wiersza jako HTML wewnątrz bloku kodu, użyj składni `@:`:

```cshtml
@for (var i = 0; i < people.Length; i++)
{
    var person = people[i];
    @:Name: @person.Name
}
```

Bez `@:` w kodzie, generowany jest błąd środowiska uruchomieniowego Razor.

Dodatkowe `@` znaki w pliku Razor mogą spowodować błędy kompilatora w instrukcjach znajdujących się w dalszej części bloku. Te błędy kompilatora może być trudne do zrozumienia, ponieważ rzeczywista błąd wystąpi przed zgłoszonego błędu. Ten błąd jest wspólne po połączeniu wiele wyrażeń niejawnej/jawnej w jednym bloku kodu.

## <a name="control-structures"></a>Struktury sterujące

Struktury sterujące stanowią rozszerzenie bloków kodu. Wszystkie aspekty bloki kodu (przejście do znaczników, wbudowane C#) dotyczą również następujące struktury:

### <a name="conditionals-if-else-if-else-and-switch"></a>Warunkowa \@IF, else i \@Switch

`@if` kontrolki, gdy kod zostanie uruchomiony:

```cshtml
@if (value % 2 == 0)
{
    <p>The value was even.</p>
}
```

`else` i `else if` nie wymagają `@` symbolu:

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

### <a name="looping-for-foreach-while-and-do-while"></a>Zapętlenie \@dla, \@foreach, \@while, i \@do while

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

### <a name="compound-using"></a>\@złożone przy użyciu

W C#programie instrukcja `using` jest używana w celu upewnienia się, że obiekt został usunięty. W elemencie Razor ten sam mechanizm służy do tworzenia pomocników HTML, który zawiera dodatkową zawartość. W poniższym kodzie, pomocników HTML renderuje tag `<form>` przy użyciu instrukcji `@using`:

```cshtml
@using (Html.BeginForm())
{
    <div>
        Email: <input type="email" id="Email" value="">
        <button>Register</button>
    </div>
}
```

### <a name="try-catch-finally"></a>\@Wypróbuj, catch, finally

Obsługa wyjątków jest podobne do C#:

[!code-cshtml[](razor/sample/Views/Home/Contact7.cshtml)]

### <a name="lock"></a>Blokada \@

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

Komentarz razor są usuwane przez serwer przed wyświetleniem strony sieci Web. Razor używa `@*  *@` do ograniczania komentarzy. Poniższy kod jest zakomentowany, więc serwer nie renderuje żadnych znaczników:

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

Dyrektywy Razor są reprezentowane przez niejawne wyrażenia z zastrzeżonymi słowami kluczowymi po symbolu `@`. Dyrektywy zazwyczaj zmienia sposób, w widoku jest analizowany lub włącza inną funkcję.

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

W dalszej części tego artykułu w sekcji [Badanie klasy C# Razor wygenerowanej dla widoku](#inspect-the-razor-c-class-generated-for-a-view) wyjaśniono, jak wyświetlić wygenerowaną klasę.

### <a name="attribute"></a>atrybut \@

Dyrektywa `@attribute` dodaje dany atrybut do klasy wygenerowanej strony lub widoku. Poniższy przykład dodaje atrybut `[Authorize]`:

```cshtml
@attribute [Authorize]
```

::: moniker range=">= aspnetcore-3.0"

### <a name="code"></a>kod \@

*Ten scenariusz dotyczy tylko składników Razor (Razor).*

Blok `@code` umożliwia [składnikowi Razor](xref:blazor/components) Dodawanie C# elementów członkowskich (pól, właściwości i metod) do składnika:

```razor
@code {
    // C# members (fields, properties, and methods)
}
```

W przypadku składników Razor `@code` jest aliasem [`@functions`](#functions) i zalecanym względem `@functions`. Dozwolony jest więcej niż jeden blok `@code`.

::: moniker-end

### <a name="functions"></a>funkcje \@

Dyrektywa `@functions` umożliwia dodawanie C# elementów członkowskich (pól, właściwości i metod) do wygenerowanej klasy:

```cshtml
@functions {
    // C# members (fields, properties, and methods)
}
```

::: moniker range=">= aspnetcore-3.0"

W [składnikach Razor](xref:blazor/components)Użyj `@code` przez `@functions`, aby C# dodać członków.

::: moniker-end

Na przykład:

[!code-cshtml[](razor/sample/Views/Home/Contact6.cshtml)]

Kod generuje następujący kod znaczników HTML:

```html
<div>From method: Hello</div>
```

Poniższy kod jest wygenerowany Razor C# klasy:

[!code-csharp[](razor/sample/Classes/Views_Home_Test_cshtml.cs?range=1-19)]

::: moniker range=">= aspnetcore-3.0"

Metody `@functions` służą jako metody tworzenia szablonów, gdy mają znaczniki:

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

### <a name="implements"></a>\@implementuje

Dyrektywa `@implements` implementuje interfejs dla wygenerowanej klasy.

Poniższy przykład implementuje <xref:System.IDisposable?displayProperty=fullName>, aby można było wywołać metodę <xref:System.IDisposable.Dispose*>:

```cshtml
@implements IDisposable

<h1>Example</h1>

@functions {
    private bool _isDisposed;

    ...

    public void Dispose() => _isDisposed = true;
}
```

::: moniker-end

### <a name="inherits"></a>\@dziedziczy

Dyrektywa `@inherits` zapewnia pełną kontrolę nad klasą, którą dziedziczy widok:

```cshtml
@inherits TypeNameOfClassToInheritFrom
```

Poniższy kod jest niestandardowy typ strony Razor:

[!code-csharp[](razor/sample/Classes/CustomRazorPage.cs)]

`CustomText` jest wyświetlana w widoku:

[!code-cshtml[](razor/sample/Views/Home/Contact10.cshtml)]

Ten kod Renderuje poniższy kod HTML:

```html
<div>
    Custom text: Gardyloo! - A Scottish warning yelled from a window before dumping
    a slop bucket on the street below.
</div>
```

 `@model` i `@inherits` mogą być używane w tym samym widoku. `@inherits` może znajdować się w pliku *_ViewImports. cshtml* , który został zaimportowany przez widok:

[!code-cshtml[](razor/sample/Views/_ViewImportsModel.cshtml)]

Poniższy kod jest przykładem silnie typizowane widoku:

[!code-cshtml[](razor/sample/Views/Home/Login1.cshtml)]

W przypadku przekazywania "rick@contoso.com" w modelu widok generuje następujący znacznik HTML:

```html
<div>The Login Email: rick@contoso.com</div>
<div>
    Custom text: Gardyloo! - A Scottish warning yelled from a window before dumping
    a slop bucket on the street below.
</div>
```

### <a name="inject"></a>\@wstrzyknąć

Dyrektywa `@inject` umożliwia stronie Razor dodanie usługi z [kontenera usługi](xref:fundamentals/dependency-injection) do widoku. Aby uzyskać więcej informacji, zobacz [iniekcja zależności w widokach](xref:mvc/views/dependency-injection).

::: moniker range=">= aspnetcore-3.0"

### <a name="layout"></a>Układ \@

*Ten scenariusz dotyczy tylko składników Razor (Razor).*

Dyrektywa `@layout` określa układ składnika Razor. Składniki układu są używane do uniknięcia duplikowania kodu i niespójności. Aby uzyskać więcej informacji, zobacz <xref:blazor/layouts>.

::: moniker-end

### <a name="model"></a>Model \@

*Ten scenariusz dotyczy tylko widoków MVC i Razor Pages (. cshtml).*

Dyrektywa `@model` określa typ modelu przekazaną do widoku lub strony:

```cshtml
@model TypeNameOfModel
```

W aplikacji ASP.NET Core MVC lub Razor Pages utworzonej przy użyciu poszczególnych kont użytkowników, *widoki/konta/login. cshtml* zawierają następującą deklarację modelu:

```cshtml
@model LoginViewModel
```

Wygenerowana Klasa dziedziczy po `RazorPage<dynamic>`:

```csharp
public class _Views_Account_Login_cshtml : RazorPage<LoginViewModel>
```

Razor uwidacznia Właściwość `Model` do uzyskiwania dostępu do modelu przekazaną do widoku:

```cshtml
<div>The Login Email: @Model.Email</div>
```

Dyrektywa `@model` określa typ właściwości `Model`. Dyrektywa określa `T` w `RazorPage<T>` wygenerowanej klasy, z której pochodzi widok. Jeśli dyrektywa `@model` nie jest określona, właściwość `Model` jest typu `dynamic`. Aby uzyskać więcej informacji, zobacz [silnie wpisane modele i słowo kluczowe @model](xref:tutorials/first-mvc-app/adding-model#strongly-typed-models-and-the--keyword).

### <a name="namespace"></a>\@przestrzeń nazw

Dyrektywa `@namespace`:

* Ustawia przestrzeń nazw klasy wygenerowanej strony Razor, widoku MVC lub składnika Razor.
* Ustawia główne pochodne przestrzenie nazw stron, widoków lub klas składników z najbliższego pliku importów w drzewie katalogów, *_ViewImports. cshtml* (widoki lub strony) lub *_Imports. Razor* (składniki Razor).

```cshtml
@namespace Your.Namespace.Here
```

W przykładzie Razor Pages przedstawionym w poniższej tabeli:

* Każda Strona importuje *strony/_ViewImports. cshtml*.
* *Strony/_ViewImports. cshtml* zawierają `@namespace Hello.World`.
* Każda Strona ma `Hello.World` jako element główny przestrzeni nazw.

| Stronic                                        | Przestrzeń nazw                             |
| ------------------------------------------- | ------------------------------------- |
| *Pages/index. cshtml*                        | `Hello.World`                         |
| *Pages/MorePages/Page. cshtml*               | `Hello.World.MorePages`               |
| *Pages/MorePages/EvenMorePages/Page. cshtml* | `Hello.World.MorePages.EvenMorePages` |

Poprzednie relacje mają zastosowanie do plików importu używanych z widokami MVC i składnikami Razor.

Gdy wiele plików importu zawiera dyrektywę `@namespace`, plik znajdujący się najbliżej strony, widoku lub składnika w drzewie katalogów jest używany do ustawiania głównej przestrzeni nazw.

Jeśli folder *EvenMorePages* w poprzednim przykładzie zawiera plik Imports z `@namespace Another.Planet` (lub plik *Pages/MorePages/EvenMorePages/Page. cshtml* zawiera `@namespace Another.Planet`), wynik jest przedstawiony w poniższej tabeli.

| Stronic                                        | Przestrzeń nazw               |
| ------------------------------------------- | ----------------------- |
| *Pages/index. cshtml*                        | `Hello.World`           |
| *Pages/MorePages/Page. cshtml*               | `Hello.World.MorePages` |
| *Pages/MorePages/EvenMorePages/Page. cshtml* | `Another.Planet`        |

### <a name="page"></a>Strona \@

::: moniker range=">= aspnetcore-3.0"

Dyrektywa `@page` ma różne skutki w zależności od typu pliku, w którym występuje. Dyrektywa:

* W pliku *. cshtml* wskazuje, że plik jest stroną Razor. Aby uzyskać więcej informacji, zobacz [niestandardowe trasy](xref:razor-pages/index#custom-routes) i <xref:razor-pages/index>.
* Określa, że składnik Razor powinien obsługiwać żądania bezpośrednio. Aby uzyskać więcej informacji, zobacz <xref:blazor/routing>.

::: moniker-end

::: moniker range="< aspnetcore-3.0"

Dyrektywa `@page` w pierwszym wierszu pliku *. cshtml* wskazuje, że plik jest stroną Razor. Aby uzyskać więcej informacji, zobacz <xref:razor-pages/index>.

::: moniker-end

### <a name="section"></a>Sekcja \@

*Ten scenariusz dotyczy tylko widoków MVC i Razor Pages (. cshtml).*

Dyrektywa `@section` jest używana w połączeniu z [układem MVC i Razor Pages](xref:mvc/views/layout) , aby umożliwić widokom i stronom renderowanie zawartości w różnych częściach strony html. Aby uzyskać więcej informacji, zobacz <xref:mvc/views/layout>.

### <a name="using"></a>\@przy użyciu

Dyrektywa `@using` dodaje dyrektywę C# `using` do wygenerowanego widoku:

[!code-cshtml[](razor/sample/Views/Home/Contact9.cshtml)]

::: moniker range=">= aspnetcore-3.0"

W [składnikach Razor](xref:blazor/components)`@using` również kontroluje składniki, które znajdują się w zakresie.

::: moniker-end

::: moniker range=">= aspnetcore-3.0"

## <a name="directive-attributes"></a>Atrybuty dyrektywy

### <a name="attributes"></a>atrybuty \@

*Ten scenariusz dotyczy tylko składników Razor (Razor).*

`@attributes` zezwala składnikowi na renderowanie niezadeklarowanych atrybutów. Aby uzyskać więcej informacji, zobacz <xref:blazor/components#attribute-splatting-and-arbitrary-parameters>.

### <a name="bind"></a>powiązanie \@

*Ten scenariusz dotyczy tylko składników Razor (Razor).*

Powiązanie danych w składnikach jest realizowane przy użyciu atrybutu `@bind`. Aby uzyskać więcej informacji, zobacz <xref:blazor/data-binding>.

### <a name="onevent"></a>\@w {EVENT}

*Ten scenariusz dotyczy tylko składników Razor (Razor).*

Razor udostępnia funkcje obsługi zdarzeń dla składników. Aby uzyskać więcej informacji, zobacz <xref:blazor/event-handling>.

::: moniker-end

::: moniker range=">= aspnetcore-3.1"

### <a name="oneventpreventdefault"></a>\@w {EVENT}:p reventDefault

*Ten scenariusz dotyczy tylko składników Razor (Razor).*

Zapobiega domyślnej akcji dla zdarzenia.

### <a name="oneventstoppropagation"></a>\@w {EVENT}: stopPropagation

*Ten scenariusz dotyczy tylko składników Razor (Razor).*

Powoduje zatrzymanie propagacji zdarzenia dla zdarzenia.

::: moniker-end

::: moniker range=">= aspnetcore-3.0"

### <a name="key"></a>klucz \@

*Ten scenariusz dotyczy tylko składników Razor (Razor).*

Atrybut `@key` dyrektywie powoduje, że algorytmy różnicowe składników do zagwarantowania zachowywania elementów lub składników na podstawie wartości klucza. Aby uzyskać więcej informacji, zobacz <xref:blazor/components#use-key-to-control-the-preservation-of-elements-and-components>.

### <a name="ref"></a>\@ref

*Ten scenariusz dotyczy tylko składników Razor (Razor).*

Odwołania do składników (`@ref`) umożliwiają odwoływanie się do wystąpienia składnika, dzięki czemu można wydać polecenia do tego wystąpienia. Aby uzyskać więcej informacji, zobacz <xref:blazor/components#capture-references-to-components>.

### <a name="typeparam"></a>\@typeparam

*Ten scenariusz dotyczy tylko składników Razor (Razor).*

Dyrektywa `@typeparam` deklaruje parametr typu ogólnego dla wygenerowanej klasy składnika. Aby uzyskać więcej informacji, zobacz <xref:blazor/templated-components#generic-typed-components>.

::: moniker-end

## <a name="templated-razor-delegates"></a>Oparte na szablonach delegatów Razor

Szablony razor umożliwiają definiowanie fragmentu interfejsu użytkownika w następującym formacie:

```cshtml
@<tag>...</tag>
```

Poniższy przykład ilustruje, jak określić delegata Razor dla szablonu jako <xref:System.Func%602>. [Typ dynamiczny](/dotnet/csharp/programming-guide/types/using-type-dynamic) jest określony dla parametru metody, która jest hermetyzowana przez delegata. [Typ obiektu](/dotnet/csharp/language-reference/keywords/object) jest określony jako wartość zwracana delegata. Szablon jest używany z <xref:System.Collections.Generic.List%601> `Pet`, który ma właściwość `Name`.

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

Szablon jest renderowany przy użyciu `pets` dostarczonej przez instrukcję `foreach`:

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

Możesz również dostarczyć wbudowany szablon Razor, jako argument do metody. W poniższym przykładzie metoda `Repeat` otrzymuje szablon Razor. Metoda używa tego szablonu w celu wygenerowania zawartość HTML z powtórzeń elementy dostarczane z listy:

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

Korzystając z listy zwierząt domowych z poprzedniego przykładu, Metoda `Repeat` jest wywoływana z:

* <xref:System.Collections.Generic.List%601> `Pet`.
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

*Ten scenariusz dotyczy tylko widoków MVC i Razor Pages (. cshtml).*

Istnieją trzy dyrektywy, które odnoszą się do [pomocników tagów](xref:mvc/views/tag-helpers/intro).

| — Dyrektywa | Funkcja |
| --------- | -------- |
| [`@addTagHelper`](xref:mvc/views/tag-helpers/intro#add-helper-label) | Udostępnia pomocników tagów do widoku. |
| [`@removeTagHelper`](xref:mvc/views/tag-helpers/intro#remove-razor-directives-label) | Usuwa pomocnicy tagów, wcześniej dodany z widoku. |
| [`@tagHelperPrefix`](xref:mvc/views/tag-helpers/intro#prefix-razor-directives-label) | Określa prefiks tagu, aby włączyć obsługę pomocnika tagów i Pomocnik tagu użycie jawnego. |

## <a name="razor-reserved-keywords"></a>Razor zastrzeżone słowa kluczowe

### <a name="razor-keywords"></a>Słowa kluczowe razor

* Strona (wymaga ASP.NET Core 2,1 lub nowszej)
* przestrzeń nazw
* — funkcje
* Inherits
* model
* section
* Pomocnik (nie jest obecnie obsługiwane przez program ASP.NET Core)

Słowa kluczowe Razor są wyprowadzane przy użyciu `@(Razor Keyword)` (na przykład `@(functions)`).

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
* try
* CATCH
* finally
* korzystanie
* while

C#Słowa kluczowe Razor muszą mieć podwójną sekwencję przy użyciu `@(@C# Razor Keyword)` (na przykład `@(@case)`). Pierwszy `@` wyprowadza parser Razor. Druga `@` wyprowadza C# Analizator.

### <a name="reserved-keywords-not-used-by-razor"></a>Zastrzeżone słowa kluczowe nie użył ich Razor

* class

## <a name="inspect-the-razor-c-class-generated-for-a-view"></a>Zbadaj Razor C# klasa generowana dla widoku

::: moniker range=">= aspnetcore-2.1"

W zestaw .NET Core SDK 2,1 lub nowszej, [zestaw SDK Razor](xref:razor-pages/sdk) obsługuje kompilację plików Razor. Podczas kompilowania projektu zestaw SDK Razor generuje obiekt *obj/< build_configuration >/< target_framework_moniker > Directory/Razor* w katalogu głównym projektu. Struktura katalogów w katalogu *Razor* odzwierciedla strukturę katalogu projektu.

Rozważmy następującą strukturę katalogów w projekcie stron Razor programu ASP.NET Core 2.1 przeznaczonych dla platformy .NET Core 2.1:

* **Obszarach**
  * **Administratora**
    * **Page**
      * *Index. cshtml*
      * *Index.cshtml.cs*
* **Page**
  * **Udostępniać**
    * *_Layout. cshtml*
  * *_ViewImports. cshtml*
  * *_ViewStart. cshtml*
  * *Index. cshtml*
  * *Index.cshtml.cs*

Kompilowanie projektu w konfiguracji *debugowania* powoduje zwrócenie następującego katalogu *obj* :

* **obiektów**
  * **Rozpocząć**
    * **netcoreapp 2.1/**
      * **Razor**
        * **Obszarach**
          * **Administratora**
            * **Page**
              * *Index.g.cshtml.cs*
        * **Page**
          * **Udostępniać**
            * *_Layout. g. cshtml. cs*
          * *_ViewImports. g. cshtml. cs*
          * *_ViewStart. g. cshtml. cs*
          * *Index.g.cshtml.cs*

Aby wyświetlić wygenerowaną klasę dla *stron/index. cshtml*, Otwórz *obj/Debug/Netcoreapp 2.1/Razor/Pages/index. g. cshtml. cs*.

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

Dodaj poniższą klasę do projektu programu ASP.NET Core MVC:

[!code-csharp[](razor/sample/Utilities/CustomTemplateEngine.cs)]

W `Startup.ConfigureServices`Przesłoń `RazorTemplateEngine` dodany przez MVC z klasą `CustomTemplateEngine`:

[!code-csharp[](razor/sample/Startup.cs?highlight=4&range=10-14)]

Ustaw punkt przerwania na `return csharpDocument;` instrukcji `CustomTemplateEngine`. Gdy wykonanie programu zatrzyma się w punkcie przerwania, Wyświetl wartość `generatedCode`.

![Widok Wizualizator tekstu generatedCode](razor/_static/tvr.png)

::: moniker-end

## <a name="view-lookups-and-case-sensitivity"></a>Widok wyszukiwania i rozróżnianie wielkości liter

Aparat widoku Razor wykonuje wyszukiwanie z uwzględnieniem wielkości liter dla widoków. Jednak rzeczywista wyszukiwania jest określany przez sam system plików:

* Na podstawie źródła pliku:
  * W systemach operacyjnych z systemami plików bez uwzględniania wielkości liter (na przykład Windows) plik fizyczny dostawcy wyszukiwania jest rozróżniana wielkość liter. Na przykład `return View("Test")` wyniki są dopasowań dla */views/Home/test.cshtml*, */views/Home/test.cshtml*i innych wariantów wielkości liter.
  * W przypadku systemów plików z uwzględnieniem wielkości liter (na przykład Linux, OSX i with `EmbeddedFileProvider`) Wyszukiwanie jest rozróżniana wielkość liter. Na przykład `return View("Test")` są zgodne z */views/Home/test.cshtml*.
* Wstępnie skompilowany widoków: Platforma ASP.NET Core 2.0 i nowsze wersje wyszukiwania prekompilowanego widoków jest uwzględniana wielkość liter, we wszystkich systemach operacyjnych. Zachowanie jest taka sama jak zachowanie dostawcy plików fizycznych w Windows. Jeśli dwa widoki wstępnie skompilowanych różnią się tylko wielkością liter, wynikiem wyszukiwania jest niedeterministyczny.

Deweloperzy są zachęcani do pasuje do wielkości liter w nazwach plików i katalogów na wielkość liter w wyrazie:

* Nazwy obszaru, kontrolera i akcji.
* Strony razor.

Wielkości liter, gwarantuje, że wdrożeń Znajdź swoje opinie, niezależnie od tego, źródłowy system plików.
