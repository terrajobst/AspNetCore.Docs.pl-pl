---
title: "Odwołania do składni razor dla platformy ASP.NET Core"
author: rick-anderson
description: "Więcej informacji o składni Razor znaczników do osadzania kodu na serwerze do stron sieci Web."
keywords: Dyrektywy Razor platformy ASP.NET Core, Razor,
ms.author: riande
manager: wpickett
ms.date: 10/18/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: mvc/views/razor
ms.openlocfilehash: e3c3149254d602db1fcc6d42360690be026189a5
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/10/2017
---
# <a name="razor-syntax-for-aspnet-core"></a>Składnia razor dla platformy ASP.NET Core

Przez [Rick Anderson](https://twitter.com/RickAndMSFT), [Luke Latham](https://github.com/guardrex), [MULLENA Taylora](https://twitter.com/ntaylormullen), i [Dan Vicarel](https://github.com/Rabadash8820)

Razor jest składnię znaczników osadzanie kodu na serwerze w witrynach sieci Web. Składnia Razor składa się z Razor znaczników, C# i HTML. Pliki zawierające Razor zazwyczaj mają *.cshtml* rozszerzenia pliku.

## <a name="rendering-html"></a>Renderowanie kodu HTML

Domyślny język Razor jest HTML. Renderowanie kodu HTML, z znaczników Razor nie różni się od renderowania kodu HTML z pliku HTML.  Kod znaczników HTML w *.cshtml* pliki Razor jest renderowany na serwerze bez zmian.

## <a name="razor-syntax"></a>Składnia razor

Razor obsługuje C# i używa `@` symbol przejście z kodu HTML dla C#. Razor ocenia wyrażeń C# i renderuje je w danych wyjściowych HTML.

Gdy `@` następuje symbol [Razor zastrzeżone słowa kluczowego](#razor-reserved-keywords), jego przejścia do znaczników specyficzne dla elementu Razor. W przeciwnym wypadku przejścia do zwykłego C#.

Wyjścia `@` symboli w znaczniku Razor, należy użyć drugiej `@` symboli:

```cshtml
<p>@@Username</p>
```

Renderowanie kodu HTML za pomocą jednej `@` symboli:

```html
<p>@Username</p>
```

Atrybuty HTML i zawartości zawierającego adresy e-mail nie Traktuj `@` symbol jako znak przejścia. W poniższym przykładzie adresy e-mail są niezmienione przez Razor analizy:

```cshtml
<a href="mailto:Support@contoso.com">Support@contoso.com</a>
```

## <a name="implicit-razor-expressions"></a>Niejawne wyrażenia Razor

Niejawne wyrażenia Razor rozpoczynać `@` następuje kod w języku C#:

```cshtml
<p>@DateTime.Now</p>
<p>@DateTime.IsLeapYear(2016)</p>
```

Z wyjątkiem C# `await` — słowo kluczowe, niejawnego wyrażenia nie może zawierać spacji. Jeśli wyczyść Kończenie instrukcji C#, mogą wymieszaniu spacji:

```cshtml
<p>@await DoSomething("hello", "world")</p>
```

Wyrażenia niejawnego **nie** zawiera typami ogólnymi C#, jako znak wewnątrz nawiasów (`<>`) są interpretowane jako tagu HTML. Następujący kod jest **nie** prawidłowy:

```cshtml
<p>@GenericMethod<int>()</p>
```

Poprzedni kod generowany jest błąd kompilatora podobny do jednego z następujących czynności:

 * Element "int" nie został zamknięty.  Wszystkie elementy muszą być albo samodzielnie zamknięcie lub ma zgodnego tagu końcowego.
 *  Nie można przekonwertować grupy metod "GenericMethod" na typ "object" Niedelegowany. Czy zamierzasz wywołać metodę? " 
 
Wywołania metody rodzajowe muszą być ujęte w [jawne wyrażenie Razor](#explicit-razor-expressions) lub [blok kodu Razor](#razor-code-blocks). To ograniczenie nie dotyczy *.vbhtml* Razor plików, ponieważ składnia języka Visual Basic umieszcza nawiasy parametry typu ogólnego zamiast nawiasy.

## <a name="explicit-razor-expressions"></a>Jawne wyrażenia Razor

Jawne Razor wyrażeń składają się z `@` symbol o zrównoważonym nawiasu. Do renderowania czas ostatniego tygodnia, jest używany następujący kod Razor:

```cshtml
<p>Last week this time: @(DateTime.Now - TimeSpan.FromDays(7))</p>
```

Zawartość w `@()` nawias jest obliczany i renderowania danych wyjściowych.

Ogólnie wyrażenia niejawnego, opisane w poprzedniej sekcji, nie może zawierać spacji. W poniższym kodzie tydzień nie jest odejmowany od bieżącego czasu:

[!code-cshtml[Main](razor/sample/Views/Home/Contact.cshtml?range=17)]

Kod Renderuje poniższy kod HTML:

```html
<p>Last week: 7/7/2016 4:39:52 PM - TimeSpan.FromDays(7)</p>
```

Jawne wyrażenia może służyć do łączenia tekstu w wyniku wyrażenia:

```cshtml
@{
    var joe = new Person("Joe", 33);
}

<p>Age@(joe.Age)</p>
```

Bez jawnego wyrażenia `<p>Age@joe.Age</p>` jest traktowany jak adres e-mail i `<p>Age@joe.Age</p>` jest renderowany. Gdy zapisywane jako wyrażenie jawne `<p>Age33</p>` jest renderowany.


Jawne wyrażenia może zostać użyty do renderowania dane wyjściowe metody rodzajowe w *.cshtml* plików. W wyrażeniu niejawne znak wewnątrz nawiasów (`<>`) są interpretowane jako tagu HTML. Następujący kod znaczników jest **nie** Razor prawidłowy:

```cshtml
<p>@GenericMethod<int>()</p>
```

Poprzedni kod generowany jest błąd kompilatora podobny do jednego z następujących czynności:

 * Element "int" nie został zamknięty.  Wszystkie elementy muszą być albo samodzielnie zamknięcie lub ma zgodnego tagu końcowego.
 *  Nie można przekonwertować grupy metod "GenericMethod" na typ "object" Niedelegowany. Czy zamierzasz wywołać metodę? " 
 
 Następujący kod przedstawia sposób poprawne zapisu tego kodu.  Kod jest zapisywany jako jawne wyrażenie:

```cshtml
<p>@(GenericMethod<int>())</p>
```

Uwaga: to ograniczenie nie dotyczy *.vbhtml* pliki Razor.  Z *.vbhtml* nawiasy parametry typu ogólnego zamiast nawiasy umieszcza pliki Razor, składnia języka Visual Basic.

## <a name="expression-encoding"></a>Kodowanie wyrażenia

Wyrażenia języka C#, określających ciąg są kodowania HTML. C# wyrażeń określających `IHtmlContent` są renderowane za pomocą `IHtmlContent.WriteTo`. C# wyrażeń, które nie zwrócą `IHtmlContent` jest konwertowana na ciąg przez `ToString` i kodowany przed ich są renderowane.

```cshtml
@("<span>Hello World</span>")
```

Kod Renderuje poniższy kod HTML:

```html
&lt;span&gt;Hello World&lt;/span&gt;
```

Kod HTML jest wyświetlany w przeglądarce jako:

```
<span>Hello World</span>
```

`HtmlHelper.Raw`dane wyjściowe nie jest zakodowany, ale renderowany jako kod znaczników HTML.

> [!WARNING]
> Przy użyciu `HtmlHelper.Raw` unsanitized użytkownika dane wejściowe stanowi zagrożenie bezpieczeństwa. Dane wejściowe użytkownika może zawierać złośliwego kodu JavaScript lub inne luki w zabezpieczeniach. Trudno jest oczyszczania danych wejściowych użytkownika. Unikaj używania `HtmlHelper.Raw` z danych wejściowych użytkownika.

```cshtml
@Html.Raw("<span>Hello World</span>")
```

Kod Renderuje poniższy kod HTML:

```html
<span>Hello World</span>
```

## <a name="razor-code-blocks"></a>Bloki kodu razor

Bloki kodu razor rozpoczynać `@` i znajdują się wewnątrz `{}`. W przeciwieństwie do wyrażenia nie jest renderowany kodu C# wewnątrz bloków kodu. Bloki kodu i wyrażenia w widoku o tym samym zakresie i są zdefiniowane w kolejności:

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

Kod Renderuje poniższy kod HTML:

```html
<p>The future depends on what you do today. - Mahatma Gandhi</p>
<p>Hate cannot drive out hate, only love can do that. - Martin Luther King, Jr.</p>
```

### <a name="implicit-transitions"></a>Niejawne przejścia

Jest to domyślny język w bloku kodu C#, ale strona Razor można przejść do HTML:

```cshtml
@{
    var inCSharp = true;
    <p>Now in HTML, was in C# @inCSharp</p>
}
```

### <a name="explicit-delimited-transition"></a>Jawne przejścia rozdzielanego

Aby zdefiniować podsekcji bloku kodu, które mają renderować kod HTML, Otocz znaków do renderowania elementu Razor  **\<tekst >** tagu:

```cshtml
@for (var i = 0; i < people.Length; i++)
{
    var person = people[i];
    <text>Name: @person.Name</text>
}
```

Tej metody można użyć do renderowania kodu HTML, który nie jest ujęta w tagu HTML. Bez tagu HTML lub Razor występuje błąd w czasie wykonywania Razor.

 **\<Tekst >** tag jest przydatne do kontroli odstępu podczas renderowania zawartości:

* Tylko zawartość między  **\<tekst >** renderowania tagu. 
* Nie spacji przed lub po  **\<tekst >** tag jest wyświetlany w danych wyjściowych HTML.

### <a name="explicit-line-transition-with-"></a>Jawne wiersz przejścia z @:

Aby renderować rest całego wiersza jako HTML w bloku kodu, należy użyć `@:` składni:

```cshtml
@for (var i = 0; i < people.Length; i++)
{
    var person = people[i];
    @:Name: @person.Name
}
```

Bez `@:` w kodzie, generowany jest błąd w czasie wykonywania Razor.

Ostrzeżenie: Dodatkowy `@` znaki w pliku Razor może spowodować błędy kompilatora Przyczyna w instrukcji w dalszej części tego bloku. Te błędy kompilatora może być trudne do zrozumienia, ponieważ błędu występuje przed zgłoszonego błędu.  Ten błąd jest typowe po łączenie wielu niejawnej/jawnej wyrażenia w bloku kodu pojedynczego.

## <a name="control-structures"></a>Struktury sterujące

Struktury sterujące stanowią rozszerzenie bloków kodu. Wszystkie aspekty bloki kodu (przechodzenia do znaczników, wbudowane C#) również dotyczą następujące struktury:

### <a name="conditionals-if-else-if-else-and-switch"></a>Warunkowe instrukcje @if, else if, #else i@switch

`@if`Formanty po uruchomieniu kodu:

```cshtml
@if (value % 2 == 0)
{
    <p>The value was even.</p>
}
```

`else`i `else if` nie wymagają `@` symboli:

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

### <a name="looping-for-foreach-while-and-do-while"></a>Zapętlenie @for, @foreach, @while, i @do podczas

HTML opartego na szablonie można renderować z pętli instrukcji sterowania.  Do renderowania listy osób:

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

### <a name="compound-using"></a>Złożone@using

W języku C# `using` instrukcji służy do zapewnienia obiekt jest usunięty. W elemencie Razor ten sam mechanizm służy do tworzenia pomocników HTML, która zawiera dodatkową zawartość. W poniższym kodzie pomocników HTML renderowania tagu form z `@using` instrukcji:


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

### <a name="try-catch-finally"></a>@try, catch, finally

Obsługa wyjątków jest podobny do języka C#:

[!code-cshtml[Main](razor/sample/Views/Home/Contact7.cshtml)]

### <a name="lock"></a>@lock

Razor ma możliwość ochrony sekcje krytyczne w instrukcjach blokady:

```cshtml
@lock (SomeLock)
{
    // Do critical section work
}
```

### <a name="comments"></a>Komentarze

Razor obsługuje komentarze języka C# i HTML:

```cshtml
@{
    /* C# comment */
    // Another C# comment
}
<!-- HTML comment -->
```

Kod Renderuje poniższy kod HTML:

```html
<!-- HTML comment -->
```

Komentarz razor są usuwane przez serwer przed wyświetleniem strony sieci Web. Używa razor `@*  *@` ograniczającej komentarze. Poniższy kod jest oznaczone jako komentarz, dlatego serwer nie renderowania żadnych znaczników:

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

Dyrektywy razor są reprezentowane przez niejawnego wyrażenia następujące zastrzeżone słowa kluczowe `@` symbolu. Dyrektywy zwykle zmienia sposób widok jest analizowana lub umożliwia korzystanie z różnych funkcji.

Opis sposobu Razor generuje kod dla widoku ułatwia zrozumienie, jak działają dyrektywy.

[!code-html[Main](razor/sample/Views/Home/Contact8.cshtml)]

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

Dalszej części tego artykułu, w sekcji [wyświetlanie klasy Razor C# wygenerowany dla widoku](#viewing-the-razor-c-class-generated-for-a-view) opisano sposób wyświetlania tego wygenerowanej klasy.

### <a name="using"></a>@using

`@using` Dodaje dyrektywy języka C# `using` dyrektywy do wygenerowanego widoku:

[!code-cshtml[Main](razor/sample/Views/Home/Contact9.cshtml)]

### <a name="model"></a>@model

`@model` Dyrektywa określa typ modelu przekazywane do widoku:

```cshtml
@model TypeNameOfModel
```

W aplikacji ASP.NET Core MVC utworzone za pomocą poszczególnych kont użytkowników *Views/Account/Login.cshtml* widok zawiera następujące oświadczenie modelu:

```cshtml
@model LoginViewModel
```

Wygenerowana klasa dziedziczy `RazorPage<dynamic>`:

```csharp
public class _Views_Account_Login_cshtml : RazorPage<LoginViewModel>
```

Przedstawia razor `Model` właściwości do uzyskiwania dostępu do modelu przekazywane do widoku:

```cshtml
<div>The Login Email: @Model.Email</div>
```

`@model` Dyrektywa określa typ tej właściwości. Określa dyrektywy `T` w `RazorPage<T>` czy wygenerowanej klasy który widok jest pochodną. Jeśli `@model` dyrektywy jest określony, `Model` właściwość jest typu `dynamic`. Wartość modelu jest przekazywany z kontrolera do widoku. Aby uzyskać więcej informacji, zobacz [silnie typizowane modeli i @model — słowo kluczowe.

### <a name="inherits"></a>@inherits

`@inherits` Dyrektywy umożliwia pełną kontrolę nad klasa dziedziczy widoku:

```cshtml
@inherits TypeNameOfClassToInheritFrom
```

Następujący kod jest typu niestandardowego Razor strony:

[!code-csharp[Main](razor/sample/Classes/CustomRazorPage.cs)]

`CustomText` Jest wyświetlany w widoku:

[!code-cshtml[Main](razor/sample/Views/Home/Contact10.cshtml)]

Kod Renderuje poniższy kod HTML:

```html
<div>Custom text: Gardyloo! - A Scottish warning yelled from a window before dumping a slop bucket on the street below.</div>
```

 `@model`i `@inherits` mogą być używane w jednym widoku.  `@inherits`mogą znajdować się w *_ViewImports.cshtml* pliku, który importuje widoku:

[!code-cshtml[Main](razor/sample/Views/_ViewImportsModel.cshtml)]

Następujący kod jest przykładem silnie typizowanego widoku:

[!code-cshtml[Main](razor/sample/Views/Home/Login1.cshtml)]

Jeśli "rick@contoso.com" jest przekazywany w modelu widoku generuje następujący kod znaczników HTML:

```html
<div>The Login Email: rick@contoso.com</div>
<div>Custom text: Gardyloo! - A Scottish warning yelled from a window before dumping a slop bucket on the street below.</div>
```

### <a name="inject"></a>@inject


`@inject` Dyrektywy umożliwia stronę Razor do dodania usługi z [kontenera usług](xref:fundamentals/dependency-injection) w widoku. Aby uzyskać więcej informacji, zobacz [iniekcji zależności do widoków](xref:mvc/views/dependency-injection).

### <a name="functions"></a>@functions

`@functions` Dyrektywy umożliwia Razor strony do dodania do widoku zawartości na poziomie funkcji:

```cshtml
@functions { // C# Code }
```

Na przykład:

[!code-cshtml[Main](razor/sample/Views/Home/Contact6.cshtml)]

Kod generuje następujący kod znaczników HTML:

```html
<div>From method: Hello</div>
```

Następujący kod jest wygenerowana klasa Razor C#:

[!code-csharp[Main](razor/sample/Classes/Views_Home_Test_cshtml.cs?range=1-19)]

### <a name="section"></a>@section

`@section` Dyrektywa jest używany w połączeniu z [układu](xref:mvc/views/layout) umożliwiające widoków do renderowania zawartości w innej części strony HTML. Aby uzyskać więcej informacji, zobacz [sekcje](xref:mvc/views/layout#layout-sections-label).

## <a name="tag-helpers"></a>Pomocników tagów

Istnieją trzy dyrektywy, które odnoszą się do [pomocników tagów](xref:mvc/views/tag-helpers/intro).

| Dyrektywy | Funkcja |
| --------- | -------- |
| [@addTagHelper](xref:mvc/views/tag-helpers/intro#add-helper-label) | Udostępnia pomocników tagów do widoku. |
| [@removeTagHelper](xref:mvc/views/tag-helpers/intro#remove-razor-directives-label) | Usuwa pomocników tagów, które wcześniej dodano z widoku. |
| [@tagHelperPrefix](xref:mvc/views/tag-helpers/intro#prefix-razor-directives-label) | Określa prefiks tagu, aby włączyć obsługę pomocnika tagów i użycia Pomocnika tagów jawnego. |

## <a name="razor-reserved-keywords"></a>Razor zastrzeżone słowa kluczowe

### <a name="razor-keywords"></a>Słowa kluczowe razor

* Strona (wymaga platformy ASP.NET Core 2.0 i nowsze)
* — funkcje
* dziedziczy
* model
* sekcja
* Pomocnik (nie są obecnie obsługiwane przez program ASP.NET Core)

Słowa kluczowe razor będą miały zmienione znaczenie z `@(Razor Keyword)` (na przykład `@(functions)`).

### <a name="c-razor-keywords"></a>Słowa kluczowe języka C# Razor

* case
* do
* default
* dla
* foreach
* if
* else
* lock
* — przełącznik
* Spróbuj
* CATCH
* finally
* korzystanie
* while

Słowa kluczowe języka C# Razor musi być podwójne anulowanie z `@(@C# Razor Keyword)` (na przykład `@(@case)`). Pierwszy `@` specjalne analizator Razor. Drugi `@` specjalne analizatora składni języka C#.

### <a name="reserved-keywords-not-used-by-razor"></a>Zastrzeżone słowa kluczowe, które nie są używane przez Razor

* — przestrzeń nazw
* class

## <a name="viewing-the-razor-c-class-generated-for-a-view"></a>Wyświetlanie klasy Razor C# wygenerowany dla widoku

Dodaj następujące klasy do projektu programu ASP.NET MVC rdzeni:

[!code-csharp[Main](razor/sample/Utilities/CustomTemplateEngine.cs)]

Zastąpienie `RazorTemplateEngine` dodane przez MVC z `CustomTemplateEngine` klasy:

[!code-csharp[Main](razor/sample/Startup.cs?highlight=4&range=10-14)]

Ustaw punkt przerwania na `return csharpDocument` instrukcja `CustomTemplateEngine`. Po przerwaniu wykonywania programu w punkcie przerwania, Wyświetl wartość `generatedCode`.

![Widok wizualizatora tekst generatedCode](razor/_static/tvr.png)

## <a name="view-lookups-and-case-sensitivity"></a>Widok wyszukiwania i rozróżniania wielkości liter.

Aparat widoku Razor wykonuje wyszukiwanie z uwzględnieniem wielkości liter dla widoków. Jednak rzeczywista wyszukiwania jest określany przez źródłowy system plików:

* Źródło opartych na pliku: 
  * W systemach operacyjnych w systemach plików bez uwzględniania wielkości liter (na przykład system Windows) plik fizyczny dostawcy wyszukiwania są bez uwzględniania wielkości liter. Na przykład `return View("Test")` powoduje dopasowań dla */Views/Home/Test.cshtml*, */Views/home/test.cshtml*i inne wariant wielkości liter.
  * W systemach plików z uwzględnieniem wielkości liter (na przykład, Linux, OS x i z `EmbeddedFileProvider`), wyszukiwanie jest rozróżniana wielkość liter. Na przykład `return View("Test")` dopasowań w szczególności */Views/Home/Test.cshtml*.
* Wstępnie skompilowana widoków: podstawowych ASP.NET 2.0 i nowsze, wyszukiwania prekompilowanego widoków nie uwzględnia wielkości liter we wszystkich systemach operacyjnych. Zachowanie jest takie same jak zachowanie dostawcy plików fizycznych w systemie Windows. Jeśli dwa widoki prekompilowany różnią się tylko wielkością liter, wynik wyszukiwania jest deterministyczna.

Deweloperzy są zachęcani do pasuje do wielkości liter nazwy plików i katalogów, aby wielkość liter w wyrazie:

    * Nazwy obszaru, kontrolera i akcji. 
    * Stron razor.
    
Wielkości liter, gwarantuje, że wdrożeń znaleźć swoje opinie, niezależnie od źródłowy system plików.
