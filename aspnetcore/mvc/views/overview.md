---
title: Widoki w programie ASP.NET Core MVC
author: ardalis
description: Dowiedz się, jak widoki obsługiwać prezentacji danych aplikacji i interakcji z użytkownikiem w aplikacji ASP.NET Core MVC.
ms.author: riande
ms.date: 04/03/2019
uid: mvc/views/overview
ms.openlocfilehash: 766996645bc6ef2b6be42d729baf5d57f55b6ddd
ms.sourcegitcommit: 78339e9891c8676db01a6e81e9cb0cdaa280162f
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/17/2019
ms.locfileid: "59012802"
---
# <a name="views-in-aspnet-core-mvc"></a>Widoki w programie ASP.NET Core MVC

Przez [Steve Smith](https://ardalis.com/) i [Luke Latham](https://github.com/guardrex)

W tym dokumencie wyjaśniono w widokach używanych w aplikacji ASP.NET Core MVC. Aby uzyskać informacji na temat stron Razor, zobacz [wprowadzenie do stron Razor](xref:razor-pages/index).

We wzorcu Model-View-Controller (MVC) *widoku* obsługuje interakcję prezentacji i użytkownika danych aplikacji. Widok jest szablon HTML z osadzonymi [znaczników Razor](xref:mvc/views/razor). Kod znaczników razor jest kod, który wchodzi w interakcję z kod znaczników HTML, aby utworzyć stronę sieci Web, które są wysyłane do klienta.

W aplikacji ASP.NET Core MVC, widoki są *.cshtml* pliki, które używają [języka programowania C#](/dotnet/csharp/) w znacznikach Razor. Zwykle, Wyświetl pliki są pogrupowane w foldery o nazwie dla każdej aplikacji [kontrolerów](xref:mvc/controllers/actions). Foldery są przechowywane w *widoków* folder w katalogu głównym aplikacji:

![Widoki folder w rozwiązaniu Explorer programu Visual Studio jest otwarty z folderu macierzystego otwarty, aby wyświetlić pliki About.cshtml, Contact.cshtml i Index.cshtml](overview/_static/views_solution_explorer.png)

*Home* kontrolera jest reprezentowany przez *Home* folder wewnątrz *widoków* folderu. *Home* folder zawiera widoki dla *o*, *skontaktuj się z pomocą*, i *indeksu* stron sieci Web (głównej). Gdy użytkownik zażąda, jeden z tych trzech stronach akcji kontrolera w *Home* kontrolera ustalić, które trzech widoków jest używane do tworzenia i zwrócić strony sieci Web do użytkownika.

Użyj [układy](xref:mvc/views/layout) sekcje spójne strony sieci Web i obniżyć powtarzających się kodów. Układy często zawierają nagłówka, elementy menu i nawigacji i stopka. Nagłówek i stopka zawierają zwykle standardowy kod znaczników dla wielu elementów metadanych i linki do zasobów skryptu i stylu. Układy pomóc w uniknięciu ten kod znaczników standardowy, w sekcji Widoki.

[Widoki częściowe](xref:mvc/views/partial) zmniejszyć zduplikowania kodu, umożliwiając zarządzanie wielokrotnego użytku części widoków. Na przykład widoku częściowego jest przydatne w przypadku biografia Autor na blogu witryny sieci Web w kilka widoków. Biografia autor jest wyświetlanie zwykłej zawartości i nie wymagają kodu do wykonania w celu utworzenia zawartości dla strony sieci Web. Autor biografia zawartość jest dostępna dla widoku przez powiązanie modelu samodzielnie, dzięki czemu przy użyciu widoku częściowego dla tego typu zawartości jest idealnym rozwiązaniem.

[Wyświetlanie składników](xref:mvc/views/view-components) są podobne do częściowe widoki, pozwalają one ograniczenia powtarzania kodu, ale są one odpowiednie dla zawartości widoku, który wymaga kod wymagany do uruchomienia na serwerze, aby było możliwe renderowanie strony sieci Web. Składniki widoków są przydatne, gdy renderowanej zawartości wymaga interakcji bazy danych, takie jak koszyk witryny sieci Web. Składniki widoków nie są ograniczone do wiązanie modelu w celu generowania danych wyjściowych strony sieci Web.

## <a name="benefits-of-using-views"></a>Korzyści z używania widoków

Widoki pomóc w ustaleniu [separacji](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#separation-of-concerns) w aplikacji MVC, oddzielając znaczników interfejsu użytkownika z innych części aplikacji. Następujące projektowania SoC sprawia, że aplikacja modułowe oprogramowanie, który zapewnia kilka korzyści:

* Aplikacja jest łatwiejsze w obsłudze, ponieważ jest lepiej zorganizowany. Widoki są ogólnie pogrupowane według funkcji aplikacji. Dzięki temu można łatwiej znaleźć widoki pokrewne podczas pracy z funkcją.
* Części aplikacji są luźno powiązane. Można tworzyć i aktualizować widoków aplikacji niezależnie od składniki dostępu logikę i dane biznesowe. Widoki aplikacji można modyfikować, bez konieczności aktualizacji innych części aplikacji.
* Jest to łatwiejsze testowanie części interfejsu użytkownika aplikacji, ponieważ opinie są oddzielne zespoły.
* Ze względu na lepsze organizacji jest mniej prawdopodobne, że należy powtórzyć przypadkowo części interfejsu użytkownika.

## <a name="creating-a-view"></a>Tworzenie widoku

Widoki, które są specyficzne dla kontrolera są tworzone w *widoków / [ControllerName]* folderu. Widoki, które są współużytkowane przez kontrolery są umieszczane w *widoków/Shared* folderu. Aby utworzyć widok, Dodaj nowy plik i nadaj mu taką samą nazwę jak jej działaniem kontrolera skojarzone z *.cshtml* rozszerzenie pliku. Aby utworzyć widok, który odpowiada za pomocą *o* akcji w *Home* kontroler, tworzyć *About.cshtml* w pliku *widoków domowych*folderu:

[!code-cshtml[](../../common/samples/WebApplication1/Views/Home/About.cshtml)]

*Razor* znaczników, który rozpoczyna się od `@` symboli. C# instrukcje uruchamiania, umieszczając C# kodu w ramach [bloków kodu Razor](xref:mvc/views/razor#razor-code-blocks) przez nawiasy klamrowe (`{ ... }`). Na przykład, zobacz przypisanie "About", aby `ViewData["Title"]` przedstawionych powyżej. Można wyświetlić wartości w pliku HTML, po prostu odwołując się do wartości z `@` symboli. Zobacz zawartość `<h2>` i `<h3>` elementów wymienionych powyżej.

Wyświetl zawartość powyżej jest tylko część całą stronę sieci Web, który jest renderowany do użytkownika. Resztę układu strony i innych typowych aspektów widoku są określone w innych plikach widoku. Aby dowiedzieć się więcej, zobacz [temat układu](xref:mvc/views/layout).

## <a name="how-controllers-specify-views"></a>Jak określić widoki, kontrolery

Widoki są zwykle zwracane z działań, jak [ViewResult](/dotnet/api/microsoft.aspnetcore.mvc.viewresult), który jest typem [ActionResult](/dotnet/api/microsoft.aspnetcore.mvc.actionresult). Metodzie akcji mogą tworzyć i zwracać `ViewResult` bezpośrednio, ale który nie jest często wykonywane. Ponieważ większość kontrolerów dziedziczy z [kontrolera](/dotnet/api/microsoft.aspnetcore.mvc.controller), po prostu użyć `View` metody pomocnika do zwrócenia `ViewResult`:

*HomeController.cs*

[!code-csharp[](../../common/samples/WebApplication1/Controllers/HomeController.cs?highlight=5&range=16-21)]

Po powrocie z tej akcji *About.cshtml* widoku wyświetlane w ostatniej sekcji jest renderowane jako następującej stronie sieci Web:

![Strona renderowane w przeglądarce Microsoft Edge — informacje](overview/_static/about-page.png)

`View` Metody pomocnika ma kilka przeciążeń. Opcjonalnie można określić:

* Jawne widoku do zwrócenia:

  ```csharp
  return View("Orders");
  ```

* A [modelu](xref:mvc/models/model-binding) do przekazania do widoku:

  ```csharp
  return View(Orders);
  ```

* Widoku i modelu:

  ```csharp
  return View("Orders", Orders);
  ```

### <a name="view-discovery"></a>Widok odnajdywania

Po powrocie z akcji widoku proces jest nazywany *widok odnajdywania* ma miejsce. Ten proces Określa plik, który widok jest używany, na podstawie nazwy widoku. 

Domyślne zachowanie `View` — metoda (`return View();`) jest do zwrócenia widoku z taką samą nazwę jak metody akcji, w którym jest wywoływana. Na przykład *o* `ActionResult` Nazwa metody kontrolera jest używana do wyszukiwania Wyświetl plik o nazwie *About.cshtml*. Po pierwsze, środowisko uruchomieniowe poszukuje *widoków / [ControllerName]* folder widoku. Jeśli nie znajdzie zgodnego widoku wyszukiwania *Shared* folder widoku.

Nie ma znaczenia, jeśli zwróci niejawnie `ViewResult` z `return View();` lub jawnie przekazać nazwę widoku, aby `View` metody z `return View("<ViewName>");`. W obu przypadkach widok odnajdywania wyszukuje pasującego pliku widoku w następującej kolejności:

   1. *Views/\[ControllerName]/\[ViewName].cshtml*
   1. *Widoki/udostępnione/\[Nazwa widoku] .cshtml*

Ścieżka pliku widoku można podać zamiast nazwy widoku. Jeśli przy użyciu ścieżką bezwzględną, zaczynając od katalogu głównego aplikacji (opcjonalnie, począwszy od "/" lub "~ /"), *.cshtml* rozszerzenia należy określić:

```csharp
return View("Views/Home/About.cshtml");
```

Ścieżki względnej można również użyć, możesz określić widoki w różnych katalogach bez *.cshtml* rozszerzenia. Wewnątrz `HomeController`, może zwrócić *indeksu* widoku Twojej *Zarządzaj* widoków ze ścieżką względną:

```csharp
return View("../Manage/Index");
```

Podobnie, można wskazać bieżący katalog specyficzne dla kontrolera, za pomocą ". /" prefiks:

```csharp
return View("./About");
```

[Widoki częściowe](xref:mvc/views/partial) i [wyświetlanie składników](xref:mvc/views/view-components) używać mechanizmu odnajdywania podobne (ale nie są identyczne).

Można dostosować domyślnej Konwencji dla jak widoki znajdują się w aplikacji przy użyciu niestandardowego [IViewLocationExpander](/dotnet/api/microsoft.aspnetcore.mvc.razor.iviewlocationexpander).

Widok odnajdywania zależy od tego, znajdowanie Wyświetl pliki według nazwy pliku. Jeśli źródłowy system plików jest uwzględniana wielkość liter, nazwy widoku są prawdopodobnie z uwzględnieniem wielkości liter. Zgodność systemów operacyjnych Uwzględnij wielkość liter, między kontrolera i nazwy akcji i skojarzony widok folderów i plików. Jeśli wystąpi błąd, którego nie można odnaleźć pliku widoku podczas pracy w systemie plików rozróżniana wielkość liter, upewnij się, że wielkość liter w wyrazie odpowiada między plikiem żądany widok i nazwa pliku rzeczywiste widoku.

Postępuj zgodnie z najlepszym rozwiązaniem jest organizowania struktury plików dla widoków w taki sposób uwzględnić relacje między kontrolerów, akcji i widoki, łatwość konserwacji i przejrzystości.

## <a name="passing-data-to-views"></a>Przekazywanie danych do widoków

Przekazywanie danych do widoków przy użyciu kilku metod:

* Silnie typizowane dane: viewmodel
* Ze słabą kontrolą typów danych
  * `ViewData` (`ViewDataAttribute`)
  * `ViewBag`

### <a name="strongly-typed-data-viewmodel"></a>Silnie typizowane dane (viewmodel)

Najbardziej niezawodną metodą jest określenie [modelu](xref:mvc/models/model-binding) typu w widoku. Ten model jest często określany jako *viewmodel*. Wystąpienie typu viewmodel są przekazywane do widoku akcji.

Aby przekazać dane do widoku przy użyciu viewmodel umożliwia widok móc korzystać z *silne* sprawdzania typu. *Silne wpisywanie* (lub *silnie typizowane*) oznacza, że każda zmienna i stała ma jawnie zdefiniowanych typów (na przykład `string`, `int`, lub `DateTime`). Poprawność typów używanych w widoku jest sprawdzany w czasie kompilacji.

[Program Visual Studio](https://www.visualstudio.com/vs/) i [programu Visual Studio Code](https://code.visualstudio.com/) listy elementów członkowskich silnie typizowanej klasy przy użyciu funkcji o nazwie [IntelliSense](/visualstudio/ide/using-intellisense). Jeśli chcesz wyświetlić właściwości viewmodel, wpisz nazwę zmiennej viewmodel następuje kropka (`.`). Dzięki temu można napisać kod szybciej z mniejszą liczbą błędów.

Określ model za pomocą `@model` dyrektywy. Użyj modelu z `@Model`:

```cshtml
@model WebApplication1.ViewModels.Address

<h2>Contact</h2>
<address>
    @Model.Street<br>
    @Model.City, @Model.State @Model.PostalCode<br>
    <abbr title="Phone">P:</abbr> 425.555.0100
</address>
```

Aby zapewnić model widoku, kontroler przekazuje go jako parametr:

```csharp
public IActionResult Contact()
{
    ViewData["Message"] = "Your contact page.";

    var viewModel = new Address()
    {
        Name = "Microsoft",
        Street = "One Microsoft Way",
        City = "Redmond",
        State = "WA",
        PostalCode = "98052-6399"
    };

    return View(viewModel);
}
```

Nie istnieją żadne ograniczenia dotyczące typów modeli, które można udostępnić widok. Zalecamy używanie modele widoków zwykłe stare CLR obiektu (— POCO) z działaniem niewielkiego lub żadnego (metod), zdefiniowane. Zazwyczaj klas viewmodel albo są przechowywane w *modeli* folderu lub osobnej *modele widoków* folder w katalogu głównym aplikacji. *Adres* viewmodel używane w powyższym przykładzie jest viewmodel POCO, przechowywane w pliku o nazwie *Address.cs*:

```csharp
namespace WebApplication1.ViewModels
{
    public class Address
    {
        public string Name { get; set; }
        public string Street { get; set; }
        public string City { get; set; }
        public string State { get; set; }
        public string PostalCode { get; set; }
    }
}
```

Nic nie uniemożliwia przy użyciu tych samych klas dla typów viewmodel i swoje typy modeli biznesowych. Jednak przy użyciu osobnych modeli umożliwia widoki, aby się nieznacznie różnić niezależnie od logiki biznesowej i danych dostęp do części aplikacji. Rozdzielenie modeli i modele widoków oferuje również korzyści w zakresie zabezpieczeń użycia modeli [wiązanie modelu](xref:mvc/models/model-binding) i [weryfikacji](xref:mvc/models/validation) dla danych przesyłanych do aplikacji przez użytkownika.

<a name="VD_VB"></a>

### <a name="weakly-typed-data-viewdata-viewdata-attribute-and-viewbag"></a>Słabo wpisanych danych (ViewData, atrybut ViewData i obiekt ViewBag)

`ViewBag` *nie jest dostępna w stron Razor.*

Oprócz silnie typizowane widoki, widoki mają dostęp do *słabo typizowaną* (nazywane również *luźno typizowanym*) zbierania danych. W przeciwieństwie do silnej typów *słabe typy* (lub *luźno typy*) oznacza, że nie jawnie deklarować typ danych jest używany. Kolekcja ze słabą kontrolą typów danych służy do przekazywania małe ilości danych do i z widoków i kontrolerów.

| Przekazywanie danych pomiędzy...                        | Przykład                                                                        |
| ------------------------------------------------- | ------------------------------------------------------------------------------ |
| Kontrolerem i widokiem                             | Wypełnianie listę rozwijaną z danymi.                                          |
| Wyświetl i [widoku układu](xref:mvc/views/layout)   | Ustawienie  **\<title >** zawartości elementu w widoku układu z plikiem widoku.  |
| [Widok częściowy](xref:mvc/views/partial) i widok | Element widget wyświetlający dane oparte na sieci Web, który użytkownik zażądał.      |

Ta kolekcja można się odwoływać przy użyciu jednej `ViewData` lub `ViewBag` właściwości dla widoków i kontrolerów. `ViewData` Właściwość jest słabo typizowanych obiektów ze słownika. `ViewBag` Właściwość jest otokę `ViewData` zapewniający właściwości dynamicznych bazowego `ViewData` kolekcji. Uwaga: Klucza wyszukiwania są bez uwzględniania wielkości liter dla obu `ViewData` i `ViewBag`.

`ViewData` i `ViewBag` są dynamicznie rozwiązane w czasie wykonywania. Ponieważ nie oferują typu w czasie kompilacji sprawdzania, oba są zwykle bardziej podatne na błędy niż przy użyciu viewmodel. Z tego powodu niektórzy deweloperzy wolą używać co najmniej lub nigdy nie `ViewData` i `ViewBag`.

<a name="VD"></a>

**ViewData**

`ViewData` jest [ViewDataDictionary](/dotnet/api/microsoft.aspnetcore.mvc.viewfeatures.viewdatadictionary) dostępne za pośrednictwem obiektu `string` kluczy. Dane ciągu może być przechowywane i używane bezpośrednio, bez konieczności rzutowania, ale należy rzutować innych `ViewData` obiektu wartości do określonych typów, kiedy ich wyodrębnić. Możesz użyć `ViewData` do przekazywania danych z kontrolerów, widoków i w obrębie widoków, w tym [widoki częściowe](xref:mvc/views/partial) i [układy](xref:mvc/views/layout).

Oto przykład, który ustawia wartości pozdrowienia i usługi za pomocą adresu `ViewData` w akcji:

```csharp
public IActionResult SomeAction()
{
    ViewData["Greeting"] = "Hello";
    ViewData["Address"]  = new Address()
    {
        Name = "Steve",
        Street = "123 Main St",
        City = "Hudson",
        State = "OH",
        PostalCode = "44236"
    };

    return View();
}
```

Praca z danymi w widoku:

```cshtml
@{
    // Since Address isn't a string, it requires a cast.
    var address = ViewData["Address"] as Address;
}

@ViewData["Greeting"] World!

<address>
    @address.Name<br>
    @address.Street<br>
    @address.City, @address.State @address.PostalCode
</address>
```

::: moniker range=">= aspnetcore-2.1"

**Atrybut viewData**

Innym rozwiązaniem, który używa [ViewDataDictionary](/dotnet/api/microsoft.aspnetcore.mvc.viewfeatures.viewdatadictionary) jest [ViewDataAttribute](/dotnet/api/microsoft.aspnetcore.mvc.viewdataattribute). Właściwości na kontrolerach ani w modelach strony Razor ozdobione `[ViewData]` ich wartości przechowywane i załadowany ze słownika.

W poniższym przykładzie zawiera kontrolera głównego `Title` ozdobione właściwość `[ViewData]`. `About` Metoda Ustawia tytuł dla tego widoku informacje:

```csharp
public class HomeController : Controller
{
    [ViewData]
    public string Title { get; set; }

    public IActionResult About()
    {
        Title = "About Us";
        ViewData["Message"] = "Your application description page.";

        return View();
    }
}
```

W widoku informacje dostępu `Title` właściwość jako właściwość modelu:

```cshtml
<h1>@Model.Title</h1>
```

W układzie tytuł jest do odczytu ze słownika ViewData:

```cshtml
<!DOCTYPE html>
<html lang="en">
<head>
    <title>@ViewData["Title"] - WebApplication</title>
    ...
```

::: moniker-end

**ViewBag**

`ViewBag` *nie jest dostępna w stron Razor.*

`ViewBag` jest [DynamicViewData](/dotnet/api/microsoft.aspnetcore.mvc.viewfeatures.internal.dynamicviewdata) obiektu, który dostarcza dynamiczny dostęp do obiektów przechowywanych w `ViewData`. `ViewBag` może być bardziej wygodne chcesz pracować, ponieważ nie wymaga rzutowania. Poniższy przykład pokazuje, jak używać `ViewBag` o ten sam wynik jako przy użyciu `ViewData` powyżej:

```csharp
public IActionResult SomeAction()
{
    ViewBag.Greeting = "Hello";
    ViewBag.Address  = new Address()
    {
        Name = "Steve",
        Street = "123 Main St",
        City = "Hudson",
        State = "OH",
        PostalCode = "44236"
    };

    return View();
}
```

```cshtml
@ViewBag.Greeting World!

<address>
    @ViewBag.Address.Name<br>
    @ViewBag.Address.Street<br>
    @ViewBag.Address.City, @ViewBag.Address.State @ViewBag.Address.PostalCode
</address>
```

**Korzystanie z podejścia ViewData i obiekt ViewBag jednocześnie**

`ViewBag` *nie jest dostępna w stron Razor.*

Ponieważ `ViewData` i `ViewBag` odnoszą się do tych samych podstawowych `ViewData` kolekcji, możesz używać ich obu `ViewData` i `ViewBag` mieszać i zgodne z nich podczas odczytywania i zapisywania wartości.

Można ustawić przy użyciu tytułu `ViewBag` i używanie opis `ViewData` w górnej części *About.cshtml* widoku:

```cshtml
@{
    Layout = "/Views/Shared/_Layout.cshtml";
    ViewBag.Title = "About Contoso";
    ViewData["Description"] = "Let us tell you about Contoso's philosophy and mission.";
}
```

Odczytane właściwości, ale odwrotnego użytkowania `ViewData` i `ViewBag`. W *_Layout.cshtml* plików, Uzyskaj za pomocą tytuł `ViewData` i Uzyskaj za pomocą opis `ViewBag`:

```cshtml
<!DOCTYPE html>
<html lang="en">
<head>
    <title>@ViewData["Title"]</title>
    <meta name="description" content="@ViewBag.Description">
    ...
```

Należy pamiętać, że ciągi nie wymagają oddanych na `ViewData`. Możesz użyć `@ViewData["Title"]` bez rzutowania.

Przy użyciu zarówno `ViewData` i `ViewBag` na tym samym działa czasu, co robi mieszanie i dopasowywanie odczytywanie i zapisywanie właściwości. Jest renderowany w niej następujące znaczniki:

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <title>About Contoso</title>
    <meta name="description" content="Let us tell you about Contoso's philosophy and mission.">
    ...
```

**Podsumowanie różnic między ViewData i obiekt ViewBag**

 `ViewBag` nie jest dostępna w stron Razor.

* `ViewData`
  * Pochodzi od klasy [ViewDataDictionary](/dotnet/api/microsoft.aspnetcore.mvc.viewfeatures.viewdatadictionary), więc posiada słownika właściwości, które mogą być przydatne, takie jak `ContainsKey`, `Add`, `Remove`, i `Clear`.
  * Klucze w słowniku są ciągami, umożliwić białe znaki. Przykład: `ViewData["Some Key With Whitespace"]`
  * Dowolny typ inny niż `string` musi być rzutowany w widoku, aby użyć `ViewData`.
* `ViewBag`
  * Pochodzi od klasy [DynamicViewData](/dotnet/api/microsoft.aspnetcore.mvc.viewfeatures.internal.dynamicviewdata), dzięki czemu umożliwia tworzenie właściwości dynamicznych, używając zapisu kropkowego (`@ViewBag.SomeKey = <value or object>`), a Rzutowanie nie jest wymagany. Składnia `ViewBag` ułatwia szybsze do dodania do widoków i kontrolerów.
  * Prostsze pod kątem wartości null. Przykład: `@ViewBag.Person?.Name`

**Kiedy używać ViewData i obiekt ViewBag**

Zarówno `ViewData` i `ViewBag` są równie prawidłowe podejścia do przekazywania małe ilości danych między widoków i kontrolerów. Wybór co do użycia zależy od preferencji. Można mieszać i dopasowywać `ViewData` i `ViewBag` obiektów, jednak ten kod jest łatwiej odczytywać i utrzymywać z jednym z podejść stosowane konsekwentnie. Oba podejścia są dynamicznie rozwiązane w czasie wykonywania i dlatego podatne na powodując błędy w czasie wykonywania. Niektóre zespoły deweloperów, unikaj ich.

### <a name="dynamic-views"></a>Widoki dynamiczne

Widoki, które nie deklarują modelu typu przy użyciu `@model` , ale mają wystąpienie modelu, przekazana do nich (na przykład `return View(Address);`) można odwoływać się do właściwości wystąpienia dynamicznie:

```cshtml
<address>
    @Model.Street<br>
    @Model.City, @Model.State @Model.PostalCode<br>
    <abbr title="Phone">P:</abbr> 425.555.0100
</address>
```

Ta funkcja zapewnia elastyczność, ale nie oferuje ochronę kompilacji i technologii IntelliSense. Jeśli właściwość nie istnieje, generowania strony sieci Web kończy się niepowodzeniem w czasie wykonywania.

## <a name="more-view-features"></a>Więcej funkcji widoku

[Pomocników tagów](xref:mvc/views/tag-helpers/intro) ułatwiają dodawanie zachowania po stronie serwera do istniejących tagów HTML. Używanie pomocników tagów eliminuje konieczność pisania niestandardowego kodu lub pomocników w obrębie widoków. Pomocnicy tagów są stosowane jako atrybuty do elementów kodu HTML i są ignorowane przez edytory, które nie może ich przetworzyć. Dzięki temu można edytować i renderowania kodu znaczników widoku w różnych narzędzi.

Generowanie niestandardowego kodu znaczników HTML może odbyć się z wielu wbudowanych pomocników HTML. Bardziej złożona logika interfejsu użytkownika może zostać obsłużony przez [składniki widoków](xref:mvc/views/view-components). Składniki widoków Podaj ten sam SoC tego kontrolerów, a widoki oferują. Mogą one wyeliminować potrzebę akcjami i widokami, które zajmują się dane używane przez wspólne elementy interfejsu użytkownika.

Podobnie jak wiele innych aspektów platformy ASP.NET Core, widoki pomocy technicznej [wstrzykiwanie zależności](xref:fundamentals/dependency-injection), dzięki czemu usługi [do widoków](xref:mvc/views/dependency-injection).
