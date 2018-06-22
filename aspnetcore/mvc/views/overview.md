---
title: Widoki w podstawowej platformy ASP.NET MVC
author: ardalis
description: Dowiedz się, jak widoki obsługi aplikacji prezentacji danych i interakcji z użytkownikiem w nazwie wzorca MVC ASP.NET Core.
ms.author: riande
ms.date: 12/12/2017
uid: mvc/views/overview
ms.openlocfilehash: 4d5cb6288711cdef145ebb0b52e4e645c535bdf2
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/20/2018
ms.locfileid: "36278352"
---
# <a name="views-in-aspnet-core-mvc"></a>Widoki w podstawowej platformy ASP.NET MVC

Przez [Steve Smith](https://ardalis.com/) i [Luke Latham](https://github.com/guardrex)

W tym dokumencie opisano widoki używane w aplikacjach ASP.NET Core MVC. Aby uzyskać informacje na stronach Razor, zobacz [wprowadzenie do stron Razor](xref:razor-pages/index).

We wzorcu Model-widok-kontroler (MVC) *widoku* obsługuje interakcję danych aplikacji prezentacji i użytkownika. Widok jest szablonu HTML z osadzonych [znaczników Razor](xref:mvc/views/razor). Kod znaczników razor jest kod, który współdziała z kod znaczników HTML do tworzenia strony sieci Web, które są wysyłane do klienta.

W programie ASP.NET Core MVC, widoki są *.cshtml* pliki, które używają [język programowania C#](/dotnet/csharp/) w znaczniku Razor. Zwykle, Wyświetl pliki są podzielone na foldery o nazwie dla każdej aplikacji [kontrolerów](xref:mvc/controllers/actions). Foldery są przechowywane w *widoków* folder w katalogu głównym aplikacji:

![Widoki folder w rozwiązaniu Explorer programu Visual Studio jest otwarty z folderu głównej otwarte, aby wyświetlić pliki About.cshtml, Contact.cshtml i Index.cshtml](overview/_static/views_solution_explorer.png)

*Home* kontrolera jest reprezentowana przez *Home* folder wewnątrz *widoków* folderu. *Home* folder zawiera widoki dla *o*, *skontaktuj się z*, i *indeksu* stron sieci Web (strona główna). Gdy użytkownik zażąda jeden z tych trzech stron internetowych akcji kontrolera w *Home* kontrolera ustalić, który z trzech widoków jest używane do tworzenia i zwraca go użytkownikowi strony sieci Web.

Użyj [układów](xref:mvc/views/layout) sekcje spójne strony sieci Web i obniżyć powtarzania kodu. Układy często zawierają nagłówka, elementy nawigacji i menu i stopki. Nagłówku i stopce zazwyczaj zawiera schematyczny znaczników dla wielu elementów metadanych i linki do zasobów skryptu i stylu. Układy pomóc w uniknięciu tego znacznika umożliwiającego w widoków.

[Widoki częściowe](xref:mvc/views/partial) ograniczyć zduplikowania kodu za pomocą wielokrotnego użytku części widoków zarządzania. Na przykład widoku częściowego jest przydatne w przypadku autora biografię na blogu witryny sieci Web wyświetlaną w kilka widoków. Biografię autora jest zwykłej Wyświetl zawartość i nie wymaga kod do wykonania w celu uzyskania zawartości strony sieci Web. Autor biografię zawartość jest dostępna do widoku przez powiązanie modelu samodzielnie, więc używanie widoku częściowego dla tego typu zawartości jest idealnym rozwiązaniem.

[Wyświetlanie składników](xref:mvc/views/view-components) są podobne do częściowego widoki, ponieważ umożliwiają zmniejszenie powtarzających się kod, ale są one odpowiednie dla widoku zawartości, która wymaga kod wymagany do uruchomienia na serwerze w celu renderowania strony sieci Web. Widok składniki są przydatne, gdy renderowanej zawartości wymaga interakcji z bazy danych, takich jak koszyka zakupów witryna sieci Web. Widok składniki nie są ograniczone do modelu powiązania w celu generowania danych wyjściowych strony sieci Web.

## <a name="benefits-of-using-views"></a>Korzyści wynikające z korzystania z widoków

Widoki pomóc w ustaleniu [projektu separacji dotyczy (SoC)](http://deviq.com/separation-of-concerns/) w aplikacji MVC, oddzielając znaczników interfejsu użytkownika z innych części aplikacji. Następującego projektu SoC sprawia, że aplikacja moduły, który zapewnia kilka korzyści:

* Aplikacja jest łatwiejsze w obsłudze, ponieważ jest lepiej zorganizowany. Widoki zazwyczaj są pogrupowane według funkcji aplikacji. Dzięki temu można łatwiej znaleźć widoki pokrewne podczas pracy z funkcją.
* Są luźno powiązane z części aplikacji. Możesz skompilować i aktualizacji aplikacji widoków niezależnie od składniki dostępu logikę i dane biznesowe. Widoki aplikacji można modyfikować, bez konieczności aktualizacji innych części aplikacji.
* Możliwe jest łatwiejsze testowanie części interfejsu użytkownika aplikacji, ponieważ widoki są osobne jednostki.
* Z powodu zapewnienia lepszej organizacji jest mniej prawdopodobne, że zostanie przypadkowo sekcje powtarzania interfejsu użytkownika.

## <a name="creating-a-view"></a>Tworzenie widoku

Widoki, które są specyficzne dla kontrolera są tworzone w *widoków / [ControllerName]* folderu. Widoki, które są współużytkowane przez kontrolery są umieszczane w *widoków/Shared* folderu. Aby utworzyć widok, Dodaj nowy plik i nadaj mu taką samą nazwę jak jego akcji kontrolera skojarzone z *.cshtml* rozszerzenia pliku. Aby utworzyć widok, który odpowiada *o* akcji w *Home* kontroler, tworzyć *About.cshtml* w pliku *widoków domowych*folderu:

[!code-cshtml[](../../common/samples/WebApplication1/Views/Home/About.cshtml)]

*Razor* znaczników rozpoczyna się od `@` symbolu. Kod C# instrukcje uruchamiania przez umieszczenie C# w [bloki kodu Razor](xref:mvc/views/razor#razor-code-blocks) przez nawiasy klamrowe (`{ ... }`). Na przykład znajdują się w sekcji przypisania "Informacje o" `ViewData["Title"]` przedstawionych powyżej. Możesz wyświetlić wartości w pliku HTML po prostu odwołuje się do wartości o `@` symbolu. Wyświetlanie zawartości `<h2>` i `<h3>` elementów wymienionych powyżej.

Wyświetl zawartość pokazanym powyżej jest tylko część całą stronę sieci Web, który jest renderowany do użytkownika. Resztę układu strony i innych typowych aspektów widoku są określone w innych plików widoku. Aby dowiedzieć się więcej, zobacz [tematu układu](xref:mvc/views/layout).

## <a name="how-controllers-specify-views"></a>Jak określić widoki, kontrolery

Widoki są zazwyczaj zwracane z akcji jako [ViewResult](/dotnet/api/microsoft.aspnetcore.mvc.viewresult), który jest typem [ActionResult](/dotnet/api/microsoft.aspnetcore.mvc.actionresult). Metodę akcji można tworzyć i zwracać `ViewResult` bezpośrednio, ale często nie jest stosowana. Ponieważ większość kontrolerów dziedziczyć [kontrolera](/dotnet/api/microsoft.aspnetcore.mvc.controller), po prostu `View` metody pomocnika do zwrócenia `ViewResult`:

*HomeController.cs*

[!code-csharp[](../../common/samples/WebApplication1/Controllers/HomeController.cs?highlight=5&range=16-21)]

Po powrocie z tej akcji *About.cshtml* renderowania widoku w ostatniej sekcji jako następujące strony sieci Web:

![Strona renderowane w przeglądarce Microsoft Edge — informacje](overview/_static/about-page.png)

`View` Metoda pomocnika ma kilka przeciążeń. Opcjonalnie można określić:

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

Po powrocie z operacji widoku proces jest nazywany *widok odnajdywania* ma miejsce. Ten proces Określa plik widoku, który jest używany na podstawie nazwy widoku. 

Domyślne zachowanie `View` — metoda (`return View();`) jest do zwrócenia widoku z taką samą nazwę jak metody akcji, z którego jest wywoływana. Na przykład *o* `ActionResult` nazwę metody kontrolera jest używana do wyszukiwania pliku widok o nazwie *About.cshtml*. Najpierw środowiska uruchomieniowego jest wyszukiwany w *widoków / [ControllerName]* folderu dla widoku. Jeśli nie znajdzie pasującego widoku wyszukuje *Shared* folderu dla widoku.

Nie ma znaczenia, gdy zwracają niejawnie `ViewResult` z `return View();` lub jawnego przesłania nazwy widoku, aby `View` metody z `return View("<ViewName>");`. W obu przypadkach widok odnajdywania wyszukuje odpowiedniego pliku widoku w następującej kolejności:

   1. *Views/\[ControllerName]/\[ViewName].cshtml*
   1. *Widoki/udostępnione/\[ViewName] .cshtml*

Ścieżka pliku widoku można podać zamiast nazwy widoku. Jeśli przy użyciu ścieżką bezwzględną, zaczynając od katalogu głównego aplikacji (opcjonalnie rozpoczynających się od "/" lub "~ /"), *.cshtml* rozszerzenia musi być określona:

```csharp
return View("Views/Home/About.cshtml");
```

Umożliwia także ścieżkę względną do określenia widoków w różnych katalogach bez *.cshtml* rozszerzenia. Wewnątrz `HomeController`, można powrócić *indeksu* widok z *Zarządzaj* widoków ze ścieżką względną:

```csharp
return View("../Manage/Index");
```

Podobnie można określić bieżącego katalogu kontrolera specyficznych z ". /" prefiksu:

```csharp
return View("./About");
```

[Widoki częściowe](xref:mvc/views/partial) i [wyświetlić składniki](xref:mvc/views/view-components) używa mechanizmów odnajdywania podobne (ale nie są identyczne).

Można dostosować domyślnej Konwencji dla jak widoki znajdują się w aplikacji przy użyciu niestandardowego [IViewLocationExpander](/dotnet/api/microsoft.aspnetcore.mvc.razor.iviewlocationexpander).

Widok odnajdywania zależy od tego, wyszukiwanie Wyświetl pliki według nazwy pliku. Jeśli źródłowy system plików jest rozróżniana wielkość liter, nazwy widoku są prawdopodobnie wielkość liter. Zgodność w różnych systemach operacyjnych wielkość liter między kontrolera i nazwy akcji i skojarzony widok folderów i plików. Jeśli wystąpi błąd, którego nie można odnaleźć pliku widoku podczas pracy z systemem plików z uwzględnieniem wielkości liter, upewnij się, zgodność między plikiem żądany widok i nazwa pliku rzeczywista widok wielkość liter.

Postępuj zgodnie z najlepszym rozwiązaniem organizowania struktury plików widoki, aby odzwierciedlić relacje widoków utrzymanie i przejrzystości, akcje i kontrolery.

## <a name="passing-data-to-views"></a>Przekazywanie danych do widoków

Przekazywanie danych do widoków przy użyciu kilku metod:

* Dane jednoznacznie: viewmodel
* Słabą danych
  - `ViewData` (`ViewDataAttribute`)
  - `ViewBag`

### <a name="strongly-typed-data-viewmodel"></a>Silnie typizowanych danych (viewmodel)

Najbardziej niezawodna podejściem jest określenie [modelu](xref:mvc/models/model-binding) typu w widoku. Ten model jest często określana jako *viewmodel*. Wystąpienie typu viewmodel są przekazywane do widoku z akcji.

Aby przekazać dane do widoku przy użyciu viewmodel umożliwia widok, aby móc korzystać z *silne* sprawdzania typu. *Silne wpisywanie* (lub *jednoznacznie*) oznacza, że każdy zmiennej i stałej ma jawnie zdefiniowanych typów (na przykład `string`, `int`, lub `DateTime`). Ważność typy używane w widoku jest sprawdzany w czasie kompilacji.

[Visual Studio](https://www.visualstudio.com/vs/) i [Visual Studio Code](https://code.visualstudio.com/) członków klasy jednoznacznie przy użyciu funkcji o nazwie [IntelliSense](/visualstudio/ide/using-intellisense). Aby wyświetlić właściwości viewmodel, należy wpisać nazwę zmiennej viewmodel, a następnie kropki (`.`). Dzięki temu można napisać kod szybciej z mniejszą liczbą błędów.

Określ model przy użyciu `@model` dyrektywy. Użyj modelu o `@Model`:

```cshtml
@model WebApplication1.ViewModels.Address

<h2>Contact</h2>
<address>
    @Model.Street<br>
    @Model.City, @Model.State @Model.PostalCode<br>
    <abbr title="Phone">P:</abbr> 425.555.0100
</address>
```

Aby zapewnić modelu widoku, kontrolera przekazuje ją jako parametr:

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

Nie ma żadnych ograniczeń na typy modelu, umożliwiające do widoku. Zalecamy używanie viewmodels zwykłego obiektu CLR stary (POCO) z zachowaniem żadnych (metody), zdefiniowane. Zazwyczaj klasy viewmodel albo są przechowywane w *modele* folderu lub oddzielnej *ViewModels* folder w katalogu głównym aplikacji. *Adres* viewmodel używane w powyższym przykładzie jest viewmodel POCO, przechowywane w pliku o nazwie *Address.cs*:

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

Nic nie uniemożliwia przy użyciu tej samej klasy dla użytkownika typy viewmodel i z typów modelu biznesowych. Jednak przy użyciu osobnych modeli umożliwia widoków się różnić, niezależnie od logiki biznesowej i dane dostępu do części aplikacji. Rozdzielenie modeli i viewmodels oferuje również korzyści w zakresie zabezpieczeń używania modeli [modelu powiązania](xref:mvc/models/model-binding) i [weryfikacji](xref:mvc/models/validation) dla danych przesyłanych do aplikacji przez użytkownika.

<a name="VD_VB"></a>

### <a name="weakly-typed-data-viewdata-viewdata-attribute-and-viewbag"></a>Słabą danych (ViewData, atrybut ViewData i obiekt ViewBag)

`ViewBag` *nie jest dostępna w stron Razor.*

Oprócz widoków z silnie typizowanych widoki mają dostęp do *słabą kontrolą* (nazywane również *typowaniem luźnym*) zbierania danych. W odróżnieniu od typów silne *słabe typy* (lub *luźno typy*) oznacza, że nie jawnie zadeklarować typ danych. Zbieranie danych, słabą kontrolą służy do przekazywania niewielkich ilości danych do i z widoków i kontrolerów.

| Przekazywanie danych pomiędzy...                        | Przykład                                                                        |
| ------------------------------------------------- | ------------------------------------------------------------------------------ |
| Kontrolerem i widokiem                             | Wypełnianie listy rozwijanej z danymi.                                          |
| Wyświetl i [widoku układu](xref:mvc/views/layout)   | Ustawienie  **\<title >** zawartości elementu w widoku układu z widoku pliku.  |
| [Widok częściowy](xref:mvc/views/partial) i widoku | Element widget wyświetlający dane oparte na sieci Web, który użytkownik zażądał.      |

Ta kolekcja można odwoływać się przy użyciu jednej `ViewData` lub `ViewBag` właściwości kontrolery i widoki. `ViewData` Właściwości jest słownikiem słabą kontrolą obiektów. `ViewBag` Właściwość jest otokę `ViewData` zapewnia właściwości dynamicznych odpowiadającego `ViewData` kolekcji.

`ViewData` i `ViewBag` są dynamicznie rozwiązane w czasie wykonywania. Ponieważ nie oferują sprawdzanie typów w czasie kompilacji, są zazwyczaj bardziej podatnych niż przy użyciu viewmodel. Z tego powodu niektórzy deweloperzy wolą minimalny zestaw lub nigdy nie należy używać `ViewData` i `ViewBag`.

<a name="VD"></a>

**ViewData**

`ViewData` jest [ViewDataDictionary](/dotnet/api/microsoft.aspnetcore.mvc.viewfeatures.viewdatadictionary) dostępne za pośrednictwem obiektu `string` kluczy. Danych dotyczących ciągu mogą być przechowywane i używane bezpośrednio, bez konieczności rzutowanie, ale należy rzutować innych `ViewData` obiektu wartości do określonych typów, po ich wyodrębnieniu. Można użyć `ViewData` do przekazywania danych z kontrolerów, widoków i w obrębie widoków, w tym [widoki częściowe](xref:mvc/views/partial) i [układów](xref:mvc/views/layout).

Poniżej przedstawiono przykład, która ustawia wartości pozdrowienia i przy użyciu adresu `ViewData` w akcji:

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

Innym rozwiązaniem, która używa [ViewDataDictionary](/dotnet/api/microsoft.aspnetcore.mvc.viewfeatures.viewdatadictionary) jest [ViewDataAttribute](/dotnet/api/microsoft.aspnetcore.mvc.viewdataattribute). Właściwości w kontrolerach ani w modelach Razor strony ozdobione `[ViewData]` ich wartości przechowywane i załadować ze słownika.

W poniższym przykładzie zawiera kontrolera głównej `Title` ozdobione właściwości `[ViewData]`. `About` Metoda Ustawia tytuł dla tego widoku informacje:

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

W widoku informacje dostępu `Title` właściwości jako właściwość modelu:

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

**Obiekt ViewBag**

`ViewBag` *nie jest dostępna w stron Razor.*

`ViewBag` jest [DynamicViewData](/dotnet/api/microsoft.aspnetcore.mvc.viewfeatures.internal.dynamicviewdata) obiekt, który umożliwia dynamiczne dostęp do obiektów przechowywanych w `ViewData`. `ViewBag` może być bardziej wygodne do pracy, ponieważ nie wymaga rzutowania. Poniższy przykład przedstawia użycie `ViewBag` z takiego samego wyniku jako przy użyciu `ViewData` powyżej:

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

**Przy użyciu elementów ViewBag, ViewData a jednocześnie**

`ViewBag` *nie jest dostępna w stron Razor.*

Ponieważ `ViewData` i `ViewBag` odwoływać się do tego samego podstawowego `ViewData` kolekcji, można użyć zarówno `ViewData` i `ViewBag` i mieszać i dopasowywać między nimi podczas odczytywania i zapisywania wartości.

Ustawić za pomocą tytuł `ViewBag` i przy użyciu opisu `ViewData` w górnej części *About.cshtml* widoku:

```cshtml
@{
    Layout = "/Views/Shared/_Layout.cshtml";
    ViewBag.Title = "About Contoso";
    ViewData["Description"] = "Let us tell you about Contoso's philosophy and mission.";
}
```

Właściwości do odczytu, ale wstecznego stosowania `ViewData` i `ViewBag`. W *_Layout.cshtml* plików, Uzyskaj za pomocą tytuł `ViewData` i Uzyskaj za pomocą opis `ViewBag`:

```cshtml
<!DOCTYPE html>
<html lang="en">
<head>
    <title>@ViewData["Title"]</title>
    <meta name="description" content="@ViewBag.Description">
    ...
```

Należy pamiętać, że ciągów nie wymagają rzutowanie dla `ViewData`. Można użyć `@ViewData["Title"]` bez rzutowania.

Za pomocą obu `ViewData` i `ViewBag` w tej samej pracy czas, jak mieszania i dopasowywanie odczytywanie i zapisywanie właściwości. Następujący kod znaczników jest renderowany:

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <title>About Contoso</title>
    <meta name="description" content="Let us tell you about Contoso's philosophy and mission.">
    ...
```

**Podsumowanie różnic między ViewData i obiekt ViewBag**

 `ViewBag` nie jest dostępna na stronach Razor.

* `ViewData`
  * Pochodną [ViewDataDictionary](/dotnet/api/microsoft.aspnetcore.mvc.viewfeatures.viewdatadictionary), co powoduje słownika właściwości, które mogą być przydatne, takich jak `ContainsKey`, `Add`, `Remove`, i `Clear`.
  * Klucze w słowniku są ciągi, więc spacji jest dozwolony. Przykład: `ViewData["Some Key With Whitespace"]`
  * Dowolny typ innych niż `string` musi być rzutowane w widoku, aby użyć `ViewData`.
* `ViewBag`
  * Pochodną [DynamicViewData](/dotnet/api/microsoft.aspnetcore.mvc.viewfeatures.internal.dynamicviewdata), więc umożliwia tworzenie dynamicznych właściwości, używając zapisu kropkowego (`@ViewBag.SomeKey = <value or object>`), a Rzutowanie nie jest wymagana. Składnia `ViewBag` umożliwia szybsze do dodania do widoków i kontrolerów.
  * Łatwiejsze do sprawdzenia wartości null. Przykład: `@ViewBag.Person?.Name`

**Kiedy należy używać ViewData lub obiekt ViewBag**

Zarówno `ViewData` i `ViewBag` są równie prawidłowy podejścia do przekazywania niewielkich ilości danych między kontrolery i widoki. Wybór który bazuje na preferencji. Można mieszać i dopasowywać `ViewData` i `ViewBag` obiektów, jednak kod jest łatwiej odczytywać i obsługa o jeden z nich korzystać. W obu przypadkach efekt są dynamicznie rozpoznane w czasie wykonywania i w związku z tym podatne na powodujące błędy podczas wykonywania. Niektóre zespoły rozwoju ich uniknięcie.

### <a name="dynamic-views"></a>Widoki dynamiczne

Widoki, które nie deklaruje modelu typu przy użyciu `@model` , ale mają wystąpienie modelu, przekazany do nich (na przykład `return View(Address);`) można odwoływać się właściwości wystąpienia dynamicznie:

```cshtml
<address>
    @Model.Street<br>
    @Model.City, @Model.State @Model.PostalCode<br>
    <abbr title="Phone">P:</abbr> 425.555.0100
</address>
```

Ta funkcja zapewnia elastyczność, ale nie oferuje ochronę kompilacji i technologii IntelliSense. Jeśli właściwość nie istnieje, generowania strony sieci Web nie powiedzie się w czasie wykonywania.

## <a name="more-view-features"></a>Więcej funkcji widoku

[Pomocników tagów](xref:mvc/views/tag-helpers/intro) ułatwiają dodawanie zachowania po stronie serwera do istniejącego tagów HTML. Przy użyciu pomocników tagów uniknąć konieczności pisanie kodu niestandardowego lub pomocników w obrębie widoków. Pomocników tagów dotyczą jako atrybuty elementów HTML i są ignorowane przez edytory, które nie może ich przetworzyć. Dzięki temu można edytować i renderowania kodu znaczników widoku w różnych narzędzi.

Generowanie niestandardowego kodu znaczników HTML może zostać osiągnięty przy wiele wbudowanych pomocników HTML. Bardziej złożonej logice interfejsu użytkownika mogą być obsługiwane przez [wyświetlania składników](xref:mvc/views/view-components). Składniki w widoku Podaj tego samego SoC tego kontrolery i widoki oferty. Mogą one, eliminując konieczność stosowania akcjami i widokami, które zajmują się dane używane przez wspólne elementy interfejsu użytkownika.

Podobnie jak wiele innych aspektów platformy ASP.NET Core, widoki pomocy technicznej [iniekcji zależności](xref:fundamentals/dependency-injection), tak aby być usługi [do widoków](xref:mvc/views/dependency-injection).
