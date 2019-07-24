---
title: Wprowadzenie do Razor Pages w ASP.NET Core
author: Rick-Anderson
description: Dowiedz się, jak stron Razor w programie ASP.NET Core umożliwia kodowania scenariuszy skoncentrowane na stronie łatwiejsze i bardziej wydajne niż przy użyciu platformy MVC.
monikerRange: '>= aspnetcore-2.0'
ms.author: riande
ms.date: 04/06/2019
uid: razor-pages/index
ms.openlocfilehash: 406e89c96ea63493091d0287077e244faee5f730
ms.sourcegitcommit: b40613c603d6f0cc71f3232c16df61550907f550
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/18/2019
ms.locfileid: "68308006"
---
# <a name="introduction-to-razor-pages-in-aspnet-core"></a>Wprowadzenie do Razor Pages w ASP.NET Core

[Rick Anderson](https://twitter.com/RickAndMSFT) i [Ryan Nowak](https://github.com/rynowak)

Razor Pages jest nowym aspektem ASP.NET Core MVC, który sprawia, że kodowanie scenariuszy ukierunkowanych na strony jest łatwiejsze i bardziej produktywne.

Jeśli szukasz samouczka korzystającego z podejścia Model-View-Controller, zobacz Wprowadzenie do [ASP.NET Core MVC](xref:tutorials/first-mvc-app/start-mvc).

Ten dokument zawiera wprowadzenie do Razor Pages. Nie jest to samouczek krok po kroku. Jeśli okaże się, że niektóre sekcje są zbyt zaawansowane, zobacz [wprowadzenie do Razor Pages](xref:tutorials/razor-pages/razor-pages-start). Aby zapoznać się z omówieniem ASP.NET Core, zobacz [wprowadzenie do ASP.NET Core](xref:index).

## <a name="prerequisites"></a>Wymagania wstępne

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

[!INCLUDE[](~/includes/net-core-prereqs-vs2019-2.2.md)]

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

[!INCLUDE[](~/includes/net-core-prereqs-vsc-2.2.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[Visual Studio for Mac](#tab/visual-studio-mac)

[!INCLUDE[](~/includes/net-core-prereqs-mac-2.2.md)]

---

<a name="rpvs17"></a>

## <a name="create-a-razor-pages-project"></a>Tworzenie projektu Razor Pages

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

Aby uzyskać szczegółowe instrukcje dotyczące sposobu tworzenia projektu Razor Pages, zobacz Wprowadzenie do [Razor Pages](xref:tutorials/razor-pages/razor-pages-start) .

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[Visual Studio for Mac](#tab/visual-studio-mac)

::: moniker range=">= aspnetcore-2.1"

Uruchom `dotnet new webapp` polecenie w wierszu polecenia.

::: moniker-end

::: moniker range="= aspnetcore-2.0"

Uruchom `dotnet new razor` polecenie w wierszu polecenia.

::: moniker-end

Otwórz wygenerowany plik *csproj* z Visual Studio dla komputerów Mac.

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

::: moniker range=">= aspnetcore-2.1"

Uruchom `dotnet new webapp` polecenie w wierszu polecenia.

::: moniker-end

::: moniker range="= aspnetcore-2.0"

Uruchom `dotnet new razor` polecenie w wierszu polecenia.

::: moniker-end

---

## <a name="razor-pages"></a>Razor Pages

Razor Pages jest włączona w *Startup.cs*:

[!code-cs[](index/sample/RazorPagesIntro/Startup.cs?name=snippet_Startup)]

Weź pod uwagę podstawową stronę:<a name="OnGet"></a>

[!code-cshtml[](index/sample/RazorPagesIntro/Pages/Index.cshtml)]

Poprzedni kod wygląda podobnie jak [plik widoku Razor](xref:tutorials/first-mvc-app/adding-view) używany w aplikacji ASP.NET Core z kontrolerami i widokami. Co sprawia, `@page` że jest to dyrektywa. `@page`sprawia, że plik jest akcją MVC, co oznacza, że obsługuje żądania bezpośrednio, bez przechodzenia przez kontroler. `@page`musi być pierwszą dyrektywą Razor na stronie. `@page`wpływa na zachowanie innych konstrukcji Razor.

Podobna Strona, za pomocą `PageModel` klasy, jest pokazana w następujących dwóch plikach. Plik *Pages/index2. cshtml* :

[!code-cshtml[](index/sample/RazorPagesIntro/Pages/Index2.cshtml)]

Model strony *stron/index2. cshtml. cs* :

[!code-cs[](index/sample/RazorPagesIntro/Pages/Index2.cshtml.cs)]

Zgodnie z Konwencją `PageModel` plik klasy ma taką samą nazwę jak plik stronicowania Razor z dołączonym rozszerzeniem *. cs* . Na przykład Poprzednia strona Razor to *Pages/index2. cshtml*. Plik zawierający `PageModel` klasę ma nazwę Pages */index2. cshtml. cs*.

Skojarzenia ścieżek adresów URL ze stronami są określane przez lokalizację strony w systemie plików. W poniższej tabeli przedstawiono ścieżkę strony Razor i pasujący adres URL:

| Nazwa i ścieżka pliku               | pasujący adres URL |
| ----------------- | ------------ |
| */Pages/Index.cshtml* | `/` lub `/Index` |
| */Pages/Contact.cshtml* | `/Contact` |
| */Pages/Store/Contact.cshtml* | `/Store/Contact` |
| */Pages/Store/Index.cshtml* | `/Store` lub `/Store/Index` |

Uwagi:

* Środowisko wykonawcze domyślnie wyszukuje pliki Razor Pages  w folderze Pages.
* `Index`jest stroną domyślną, gdy adres URL nie zawiera strony.

## <a name="write-a-basic-form"></a>Napisz podstawowy formularz

Razor Pages zaprojektowano tak, aby wspólne wzorce używane z przeglądarkami sieci Web były łatwe do wdrożenia podczas kompilowania aplikacji. [Powiązania modelu](xref:mvc/models/model-binding), [pomocników tagów](xref:mvc/views/tag-helpers/intro)i pomocników HTML same *działają* z właściwościami zdefiniowanymi w klasie strony Razor. Weź pod uwagę stronę implementującą podstawowy formularz "contact us" (kontakt `Contact` z nami) dla modelu:

Przykłady w tym dokumencie `DbContext` są inicjowane w pliku [Startup.cs](https://github.com/aspnet/AspNetCore.Docs/blob/master/aspnetcore/razor-pages/index/sample/RazorPagesContacts/Startup.cs#L15-L16) .

[!code-cs[](index/sample/RazorPagesContacts/Startup.cs?highlight=15-16)]

Model danych:

[!code-cs[](index/sample/RazorPagesContacts/Data/Customer.cs)]

Kontekst bazy danych:

[!code-cs[](index/sample/RazorPagesContacts/Data/AppDbContext.cs)]

Plik widoku *Pages/Create. cshtml* :

[!code-cshtml[](index/sample/RazorPagesContacts/Pages/Create.cshtml)]

Model strony *Pages/Create. cshtml. cs* :

[!code-cs[](index/sample/RazorPagesContacts/Pages/Create.cshtml.cs?name=snippet_ALL)]

Zgodnie z Konwencją `PageModel` Klasa jest wywoływana `<PageName>Model` i znajduje się w tej samej przestrzeni nazw co strona.

`PageModel` Klasa umożliwia rozdzielenie logiki strony od jej prezentacji. Definiuje procedury obsługi stron dla żądań wysyłanych do strony oraz dane używane do renderowania strony. To Separacja umożliwia zarządzanie zależnościami stron za pomocą [iniekcji zależności](xref:fundamentals/dependency-injection) oraz do [testowania jednostkowego](xref:test/razor-pages-tests) stron.

Strona ma `OnPostAsync` *metodę obsługi*, która jest uruchamiana na `POST` żądaniach (gdy użytkownik księguje formularz). Można dodać metody obsługi dla dowolnego zlecenia HTTP. Najczęstszymi obsłudze są:

* `OnGet`do zainicjowania stanu wymaganego dla strony. Przykład [OnGet](#OnGet) .
* `OnPost`do obsługi przesłanych formularzy.

Sufiks `Async` nazewnictwa jest opcjonalny, ale jest często używany przez Konwencję dla funkcji asynchronicznych. `OnPostAsync` Kod w poprzednim przykładzie wygląda podobnie jak w przypadku normalnego zapisu w kontrolerze. Poprzedni kod jest typowy dla Razor Pages. Większość elementów podstawowych MVC, takich jak [powiązanie modelu](xref:mvc/models/model-binding), [Walidacja](xref:mvc/models/validation)i wyniki akcji, jest udostępniana.  <!-- Review: Ryan, can we get a list of what is shared and what isn't? -->

Poprzednia `OnPostAsync` Metoda:

[!code-cs[](index/sample/RazorPagesContacts/Pages/Create.cshtml.cs?name=snippet_OnPostAsync)]

Podstawowy przepływ `OnPostAsync`:

Sprawdź, czy występują błędy walidacji.

* Jeśli nie ma żadnych błędów, Zapisz dane i Przekieruj.
* W przypadku wystąpienia błędów ponownie Wyświetl stronę z komunikatami walidacji. Walidacja po stronie klienta jest taka sama jak w przypadku tradycyjnych ASP.NET Core aplikacji MVC. W wielu przypadkach błędy sprawdzania poprawności zostaną wykryte na kliencie i nigdy nie przesłano ich do serwera.

Po pomyślnym `OnPostAsync` wprowadzeniu danych metoda obsługi `RedirectToPage` wywołuje metodę pomocnika zwracającą wystąpienie elementu `RedirectToPageResult`. `RedirectToPage`to nowy wynik akcji, podobny do `RedirectToAction` lub `RedirectToRoute`, ale dostosowany do stron. W powyższym przykładzie przekierowuje do strony indeksu głównego (`/Index`). `RedirectToPage`szczegółowo znajduje się w sekcji [generowanie adresów URL dla stron](#url_gen) .

Gdy przesłany formularz ma błędy walidacji (które są przekazywane do serwera),`OnPostAsync` metoda obsługi `Page` wywołuje metodę pomocnika. `Page`Zwraca wystąpienie elementu `PageResult`. Zwracanie `Page` jest podobne do sposobu, w jaki akcje `View`w kontrolerach zwracają. `PageResult`jest wartością domyślną <!-- Review  --> zwracany typ dla metody obsługi. Metoda obsługi, która zwraca `void` renderowanie strony.

`Customer` Właściwość używa`[BindProperty]` atrybutu, aby zrezygnować z powiązania modelu.

[!code-cs[](index/sample/RazorPagesContacts/Pages/Create.cshtml.cs?name=snippet_PageModel&highlight=10-11)]

Razor Pages domyślnie Powiąż właściwości tylko z`GET` niezleceniami. Powiązanie z właściwościami może zmniejszyć ilość kodu, który trzeba napisać. Powiązanie zmniejsza kod, używając tej samej właściwości do renderowania pól formularza (`<input asp-for="Customer.Name">`) i akceptuję dane wejściowe.

[!INCLUDE[](~/includes/bind-get.md)]

Strona główna (*index. cshtml*):

[!code-cshtml[](index/sample/RazorPagesContacts/Pages/Index.cshtml)]

Skojarzona `PageModel` Klasa (*index.cshtml.cs*):

[!code-cs[](index/sample/RazorPagesContacts/Pages/Index.cshtml.cs)]

Plik *index. cshtml* zawiera następujące znaczniki, aby utworzyć łącze do edycji dla każdego kontaktu:

[!code-cshtml[](index/sample/RazorPagesContacts/Pages/Index.cshtml?range=21)]

[Pomocnik tagu kotwicy](xref:mvc/views/tag-helpers/builtin-th/anchor-tag-helper) użył `asp-route-{value}` atrybutu w celu wygenerowania linku do strony edycji. Link zawiera dane trasy z IDENTYFIKATORem kontaktu. Na przykład `http://localhost:5000/Edit/1`. Użyj atrybutu `asp-area` , aby określić obszar. Aby uzyskać więcej informacji, zobacz <xref:mvc/controllers/areas>.

Plik *Pages/Edit. cshtml* :

[!code-cshtml[](index/sample/RazorPagesContacts/Pages/Edit.cshtml?highlight=1)]

Pierwszy wiersz zawiera `@page "{id:int}"` dyrektywę. Ograniczenie`"{id:int}"` routingu instruuje stronę, aby akceptowała żądania do strony `int` zawierającej dane trasy. Jeśli żądanie do strony nie zawiera danych trasy, które można przekonwertować na obiekt `int`, środowisko uruchomieniowe zwróci błąd HTTP 404 (nie znaleziono). Aby identyfikator był opcjonalny, Dołącz `?` do ograniczenia trasy:

 ```cshtml
@page "{id:int?}"
```

Plik *stron/Edytuj. cshtml. cs* :

[!code-cs[](index/sample/RazorPagesContacts/Pages/Edit.cshtml.cs)]

Plik *index. cshtml* zawiera również znaczniki umożliwiające utworzenie przycisku usuwania dla każdego kontaktu z klientem:

[!code-cshtml[](index/sample/RazorPagesContacts/Pages/Index.cshtml?range=22-23)]

Gdy przycisk Usuń jest renderowany w języku HTML, jego `formaction` parametry obejmują:

* Identyfikator osoby kontaktowej klienta określony przez `asp-route-id` atrybut.
* `handler` Określony`asp-page-handler` przez atrybut.

Oto przykład renderowanego przycisku usuwania z IDENTYFIKATORem `1`kontaktu klienta:

```html
<button type="submit" formaction="/?id=1&amp;handler=delete">delete</button>
```

Po wybraniu przycisku do serwera zostanie wysłane `POST` żądanie formularza. Według Konwencji, nazwa metody obsługi jest wybierana na podstawie wartości `handler` parametru zgodnie z schematem. `OnPost[handler]Async`

`handler` Ponieważ jest `delete` w`OnPostDeleteAsync` tym przykładzie, metoda obsługi jest używana do przetwarzania `POST` żądania. Jeśli jest ustawiona na inną wartość, na przykład `remove`, jest wybierana metoda obsługi o nazwie `OnPostRemoveAsync`. `asp-page-handler`

[!code-cs[](index/sample/RazorPagesContacts/Pages/Index.cshtml.cs?range=26-37)]

`OnPostDeleteAsync` Metody:

* `id` Akceptuje z ciągu zapytania.
* Wysyła zapytanie do bazy danych w celu skontaktowania się z klientem za pomocą `FindAsync`programu.
* Jeśli kontakt z klientem zostanie znaleziony, zostanie on usunięty z listy kontaktów klientów. Baza danych jest aktualizowana.
* Wywołania `RedirectToPage` do przekierowania na stronę indeksu głównego (`/Index`).

::: moniker range=">= aspnetcore-2.1"

## <a name="mark-page-properties-as-required"></a>Oznacz właściwości strony jako wymagane

Właściwości na serwerze `PageModel` mogą być dekoracyjne z [wymaganym](/dotnet/api/system.componentmodel.dataannotations.requiredattribute) atrybutem:

[!code-cs[](index/sample/Create.cshtml.cs?highlight=3,15-16)]

Aby uzyskać więcej informacji, zobacz [Walidacja modelu](xref:mvc/models/validation).

## <a name="handle-head-requests-with-an-onget-handler-fallback"></a>Obsługa żądań głównych przy użyciu rezerwy procedury obsługi OnGet

`HEAD`żądania umożliwiają pobranie nagłówków dla określonego zasobu. W przeciwieństwie `GET` do `HEAD` żądań, żądania nie zwracają treści odpowiedzi.

Zwykle program obsługi jest tworzony i wywoływany dla `HEAD` żądań: `OnHead` 

```csharp
public void OnHead()
{
    HttpContext.Response.Headers.Add("HandledBy", "Handled by OnHead!");
}
```

W ASP.NET Core 2,1 lub nowszej Razor Pages powracać do wywoływania procedury `OnGet` obsługi, jeśli `OnHead` nie zdefiniowano procedury obsługi. To zachowanie jest włączane przez wywołanie [SetCompatibilityVersion](xref:mvc/compatibility-version) w `Startup.ConfigureServices`:

```csharp
services.AddMvc()
    .SetCompatibilityVersion(CompatibilityVersion.Version_2_1);
```

Szablony domyślne generują `SetCompatibilityVersion` wywołanie w ASP.NET Core 2,1 i 2,2. `SetCompatibilityVersion`efektywnie ustawia opcję `AllowMappingHeadRequestsToGetHandler` Razor Pages na `true`.

Zamiast korzystać z wszystkich zachowań w programie `SetCompatibilityVersion`, można jawnie zrezygnować z *określonych* zachowań. Poniższy kod pozwala na umożliwienie `HEAD` mapowania żądań `OnGet` do programu obsługi:

```csharp
services.AddMvc()
    .AddRazorPagesOptions(options =>
    {
        options.AllowMappingHeadRequestsToGetHandler = true;
    });
```

::: moniker-end

<a name="xsrf"></a>

## <a name="xsrfcsrf-and-razor-pages"></a>XSRF/CSRF i Razor Pages

Nie trzeba pisać kodu do [weryfikacji](xref:security/anti-request-forgery)przed fałszerstwem. Generowanie i sprawdzanie poprawności tokenów antysfałszowanych są automatycznie dołączane do Razor Pages.

<a name="layout"></a>

## <a name="using-layouts-partials-templates-and-tag-helpers-with-razor-pages"></a>Używanie układów, częściowych, szablonów i pomocników tagów z Razor Pages

Strony współpracują ze wszystkimi funkcjami aparatu widoku Razor. Układy, części, szablony, pomocników tagów, *_ViewStart. cshtml*, *_ViewImports. cshtml* działają w taki sam sposób, jak w przypadku konwencjonalnych widoków Razor.

Zanotujmy Tę stronę, korzystając z zalet niektórych z tych funkcji.

::: moniker range=">= aspnetcore-2.1"

Dodaj [stronę układu](xref:mvc/views/layout) do *stron/Shared/_Layout. cshtml*:

::: moniker-end

::: moniker range="= aspnetcore-2.0"

Dodaj [stronę układu](xref:mvc/views/layout) do *stron/_Layout. cshtml*:

::: moniker-end

[!code-cshtml[](index/sample/RazorPagesContacts2/Pages/_LayoutSimple.cshtml)]

[Układ](xref:mvc/views/layout):

* Steruje układem każdej strony (chyba że strona nie jest częścią układu).
* Importuje struktury HTML, takie jak JavaScript i stylesheets.

Aby uzyskać więcej informacji, zobacz [stronę układu](xref:mvc/views/layout) .

Właściwość [układu](xref:mvc/views/layout#specifying-a-layout) jest ustawiana na *stronie/_ViewStart. cshtml*:

[!code-cshtml[](index/sample/RazorPagesContacts2/Pages/_ViewStart.cshtml)]

::: moniker range=">= aspnetcore-2.1"

Układ znajduje się w *stronie/* w folderze udostępnionym. Strony szukają innych widoków (układów, szablonów, częściowych) hierarchicznie, rozpoczynając w tym samym folderze, w którym znajduje się bieżąca strona. Układ na *stronach/* w folderze udostępnionym może być używany z dowolnej strony Razor w folderze *Pages* .

Plik układu powinien przejść do *stron/folderu udostępnionego* .

::: moniker-end

::: moniker range="= aspnetcore-2.0"

Układ znajduje się w folderze *strony* . Strony szukają innych widoków (układów, szablonów, częściowych) hierarchicznie, rozpoczynając w tym samym folderze, w którym znajduje się bieżąca strona. Układ w folderze *Pages* może być używany z dowolnej strony Razor w folderze *Pages* .

::: moniker-end

Zalecamy **umieszczenie pliku** układu w widokach */* folderze udostępnionym. *Widoki/udostępnione* są wzorcem widoków MVC. Razor Pages są przeznaczone do korzystania z hierarchii folderów, a nie Konwencji ścieżek.

Widok wyszukiwania na stronie Razor zawiera folder *strony* . Układy, szablony i częściowe, które są używane z kontrolerami MVC i konwencjonalnymi widokami Razor, *działają tylko*.

Dodaj plik *Pages/_ViewImports. cshtml* :

[!code-cshtml[](index/sample/RazorPagesContacts2/Pages/_ViewImports.cshtml)]

`@namespace`wyjaśniono w dalszej części tego samouczka. Dyrektywa `@addTagHelper` znajduje się w [wbudowanych pomocników tagów](xref:mvc/views/tag-helpers/builtin-th/Index) do wszystkich stron w folderze *strony* .

<a name="namespace"></a>

`@namespace` Gdy dyrektywa jest używana jawnie na stronie:

[!code-cshtml[](index/sample/RazorPagesIntro/Pages/Customers/Namespace2.cshtml?highlight=2)]

Dyrektywa ustawia przestrzeń nazw dla strony. `@model` Dyrektywa nie musi zawierać przestrzeni nazw.

Gdy dyrektywa jest zawarta w *_ViewImports. cshtml*, określona przestrzeń nazw udostępnia prefiks dla wygenerowanej przestrzeni nazw na `@namespace` stronie, która importuje dyrektywę. `@namespace` Pozostała część wygenerowanej przestrzeni nazw (część sufiksu) jest ścieżką względną oddzieloną kropką między folderem zawierającym *_ViewImports. cshtml* i folderem zawierającym stronę.

Na przykład `PageModel` Klasa Pages */Customers/Edit. cshtml. cs* jawnie ustawia przestrzeń nazw:

[!code-cs[](index/sample/RazorPagesContacts2/Pages/Customers/Edit.cshtml.cs?name=snippet_namespace)]

Plik *Pages/_ViewImports. cshtml* ustawia następującą przestrzeń nazw:

[!code-cshtml[](index/sample/RazorPagesContacts2/Pages/_ViewImports.cshtml?highlight=1)]

Wygenerowana przestrzeń nazw dla strony */Customers/Edit. cshtml* Razor jest taka sama jak `PageModel` Klasa.

`@namespace`*działa również z konwencjonalnymi widokami Razor.*

Oryginalne *strony/Utwórz plik widoku. cshtml* :

[!code-cshtml[](index/sample/RazorPagesContacts/Pages/Create.cshtml?highlight=2)]

Zaktualizowane *strony/Utwórz plik widoku. cshtml* :

[!code-cshtml[](index/sample/RazorPagesContacts2/Pages/Customers/Create.cshtml?highlight=2)]

[Razor Pages początkowy projekt](#rpvs17) zawiera *stronę/_ValidationScriptsPartial. cshtml*, który łączy weryfikację po stronie klienta.

Aby uzyskać więcej informacji o widokach częściowych, zobacz <xref:mvc/views/partial>.

<a name="url_gen"></a>

## <a name="url-generation-for-pages"></a>Generowanie adresu URL dla stron

Strona, pokazana wcześniej, używa `RedirectToPage`: `Create`

[!code-cs[](index/sample/RazorPagesContacts/Pages/Create.cshtml.cs?name=snippet_OnPostAsync&highlight=10)]

Aplikacja ma następującą strukturę plików/folderów:

* */Pages*

  * *Index.cshtml*
  * */Customers*

    * *Create.cshtml*
    * *Edit.cshtml*
    * *Index.cshtml*

Strony */Customers/Create. cshtml* i Pages/ *Customers/Edit. cshtml* przekierują do *stron/index. cshtml* po powodzeniu. Ciąg `/Index` jest częścią identyfikatora URI, aby uzyskać dostęp do poprzedniej strony. Ten ciąg `/Index` może służyć do generowania identyfikatorów URI na stronie *stron/index. cshtml* . Przykład:

* `Url.Page("/Index", ...)`
* `<a asp-page="/Index">My Index Page</a>`
* `RedirectToPage("/Index")`

Nazwa strony jest ścieżką do strony z folderu głównego */Pages* , włącznie z wiodącym `/` `/Index`(na przykład). Powyższe przykłady generowania adresów URL oferują ulepszone opcje i możliwości funkcjonalne w porównaniu z zakodowana adresem URL. Generowanie adresów URL używa [routingu](xref:mvc/controllers/routing) i może generować i kodować parametry zgodnie ze sposobem zdefiniowania trasy w ścieżce docelowej.

Generowanie adresów URL dla stron obsługuje nazwy względne. W poniższej tabeli przedstawiono, która strona indeksu została wybrana z `RedirectToPage` różnymi parametrami *stron/Customers/Create. cshtml*:

| RedirectToPage(x)| Stronic |
| ----------------- | ------------ |
| RedirectToPage("/Index") | *Strony/indeks* |
| RedirectToPage("./Index"); | *Strony/klienci/indeks* |
| RedirectToPage("../Index") | *Strony/indeks* |
| RedirectToPage("Index")  | *Strony/klienci/indeks* |

`RedirectToPage("Index")`, `RedirectToPage("./Index")`, i `RedirectToPage("../Index")` są *nazwami względnymi*. Parametr jest połączony ze ścieżką bieżącej strony, aby obliczyć nazwę strony docelowej.  `RedirectToPage`  <!-- Review: Original had The provided string is combined with the page name of the current page to compute the name of the destination page.  page name, not page path -->

Łączenie nazw względnych jest przydatne podczas kompilowania lokacji ze złożoną strukturą. Jeśli używasz nazw względnych do łączenia między stronami w folderze, możesz zmienić nazwę tego folderu. Wszystkie linki nadal działają (ponieważ nie zawierają nazwy folderu).

::: moniker range=">= aspnetcore-2.1"

Aby przekierować do strony w innym [obszarze](xref:mvc/controllers/areas), określ obszar:

```csharp
RedirectToPage("/Index", new { area = "Services" });
```

Aby uzyskać więcej informacji, zobacz <xref:mvc/controllers/areas>.

## <a name="viewdata-attribute"></a>ViewData — atrybut

Dane można przekazywać do strony z [ViewDataAttribute](/dotnet/api/microsoft.aspnetcore.mvc.viewdataattribute). Właściwości na kontrolerach lub modelach stron Razor, `[ViewData]` których wartości są przechowywane i ładowane z [ViewDataDictionary](/dotnet/api/microsoft.aspnetcore.mvc.viewfeatures.viewdatadictionary).

W poniższym przykładzie `AboutModel` `Title` zawiera właściwość z `[ViewData]`właściwością. `Title` Właściwość jest ustawiona na tytuł strony informacje:

```csharp
public class AboutModel : PageModel
{
    [ViewData]
    public string Title { get; } = "About";

    public void OnGet()
    {
    }
}
```

Na stronie informacje uzyskaj dostęp do `Title` właściwości jako właściwości modelu:

```cshtml
<h1>@Model.Title</h1>
```

W układzie tytuł jest odczytywany ze słownika ViewData:

```cshtml
<!DOCTYPE html>
<html lang="en">
<head>
    <title>@ViewData["Title"] - WebApplication</title>
    ...
```

::: moniker-end

## <a name="tempdata"></a>TempData

ASP.NET Core uwidacznia Właściwość [TempData](/dotnet/api/microsoft.aspnetcore.mvc.controller.tempdata?view=aspnetcore-2.0#Microsoft_AspNetCore_Mvc_Controller_TempData) na [kontrolerze](/dotnet/api/microsoft.aspnetcore.mvc.controller). Ta właściwość przechowuje dane, dopóki nie zostanie odczytana. Metody `Keep` i`Peek` mogą służyć do badania danych bez usuwania. `TempData`jest przydatne w przypadku przekierowania, gdy dane są potrzebne dla więcej niż jednego żądania.

Ten `[TempData]` atrybut jest nowy w ASP.NET Core 2,0 i jest obsługiwany na kontrolerach i stronach.

Poniższy kod ustawia wartość `Message` użycia: `TempData`

[!code-cs[](index/sample/RazorPagesContacts2/Pages/Customers/CreateDot.cshtml.cs?highlight=10-11,25&name=snippet_Temp)]

W poniższym znaczniku w pliku *Pages/Customers/index. cshtml* jest wyświetlana wartość `Message` using `TempData`.

```cshtml
<h3>Msg: @Model.Message</h3>
```

Model strony *Pages/Customers/index. cshtml. cs* stosuje `[TempData]` atrybut do `Message` właściwości.

```cs
[TempData]
public string Message { get; set; }
```

Aby uzyskać więcej informacji, zobacz [TempData](xref:fundamentals/app-state#tempdata) .

<a name="mhpp"></a>

## <a name="multiple-handlers-per-page"></a>Wiele programów obsługi na stronie

Poniższa Strona generuje znaczniki dla dwóch programów obsługi przy użyciu `asp-page-handler` pomocnika tagów:

[!code-cshtml[](index/sample/RazorPagesContacts2/Pages/Customers/CreateFATH.cshtml?highlight=12-13)]

<!-- Review: the FormActionTagHelper applies to all <form /> elements on a Razor page, even when there's no `asp-` attribute   -->

Formularz w poprzednim przykładzie ma dwa przyciski przesyłania, z których `FormActionTagHelper` każdy używa do przesłania do innego adresu URL. Ten `asp-page-handler` atrybut jest towarzyszący do `asp-page`. `asp-page-handler`generuje adresy URL, które przesyłają do każdej metody obsługi zdefiniowanej przez stronę. `asp-page`nie została określona, ponieważ próbka jest łączona z bieżącą stroną.

Model strony:

[!code-cs[](index/sample/RazorPagesContacts2/Pages/Customers/CreateFATH.cshtml.cs?highlight=20,32)]

Poprzedni kod używa *nazwanych metod obsługi*. Nazwane metody obsługi są tworzone przez pobranie tekstu w nazwie po `On<HTTP Verb>` i przed `Async` (jeśli istnieje). W poprzednim przykładzie metody strony są onpost**JoinList**Async i Onpost**JoinListUC**Async. Po  usunięciu funkcji onpost i *Async* nazwy programów obsługi `JoinList` są `JoinListUC`i.

[!code-cshtml[](index/sample/RazorPagesContacts2/Pages/Customers/CreateFATH.cshtml?range=12-13)]

Korzystając z powyższego kodu, ścieżka URL, która jest `OnPostJoinListAsync` przesyłana do usługi, to `http://localhost:5000/Customers/CreateFATH?handler=JoinList`. Ścieżka URL, która jest przesyłana `OnPostJoinListUCAsync` do `http://localhost:5000/Customers/CreateFATH?handler=JoinListUC`programu, to.

## <a name="custom-routes"></a>Trasy niestandardowe

Użyj dyrektywy `@page` , aby:

* Określ trasę niestandardową dla strony. Na przykład trasy do strony informacje można ustawić na `/Some/Other/Path` wartość z. `@page "/Some/Other/Path"`
* Dołącz segmenty do domyślnej trasy strony. Na przykład segment "Item" można dodać do domyślnej trasy strony przy użyciu `@page "item"`.
* Dołącz parametry do domyślnej trasy strony. Na przykład parametr ID, `id`, może być wymagany dla strony z. `@page "{id}"`

Ścieżka względna elementu głównego wypisana przez tyldę`~`() na początku ścieżki jest obsługiwana. Na przykład `@page "~/Some/Other/Path"` jest taka sama jak `@page "/Some/Other/Path"`.

Można zmienić ciąg `?handler=JoinList` zapytania w adresie URL na segment `/JoinList` trasy, określając szablon `@page "{handler?}"`trasy.

Jeśli nie podoba Ci się ciąg `?handler=JoinList` zapytania w adresie URL, możesz zmienić trasy, aby umieścić nazwę programu obsługi w części adresu URL. Możesz dostosować trasę, dodając szablon trasy ujęty w podwójne cudzysłowy po `@page` dyrektywie.

[!code-cshtml[](index/sample/RazorPagesContacts2/Pages/Customers/CreateRoute.cshtml?highlight=1)]

Korzystając z powyższego kodu, ścieżka URL, która jest `OnPostJoinListAsync` przesyłana do usługi, to `http://localhost:5000/Customers/CreateFATH/JoinList`. Ścieżka URL, która jest przesyłana `OnPostJoinListUCAsync` do `http://localhost:5000/Customers/CreateFATH/JoinListUC`programu, to.

Poniżej przedstawiono `handler` , że parametr trasy jest opcjonalny. `?`

## <a name="configuration-and-settings"></a>Konfiguracja i ustawienia

Aby skonfigurować opcje zaawansowane, użyj metody `AddRazorPagesOptions` rozszerzenia w konstruktorze MVC:

[!code-cs[](index/sample/RazorPagesContacts/StartupAdvanced.cs?name=snippet_1)]

Obecnie można użyć, `RazorPagesOptions` aby ustawić katalog główny dla stron lub dodać konwencje modelu aplikacji dla stron. W przyszłości włączysz więcej rozszerzeń w ten sposób.

Aby wstępnie skompilować widoki, zobacz [kompilacja widoku Razor](xref:mvc/views/view-compilation) .

[Pobierz lub Wyświetl przykładowy kod](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/razor-pages/index/sample).

Zobacz Rozpoczynanie [pracy z usługą Razor Pages](xref:tutorials/razor-pages/razor-pages-start), która kompiluje się w tym wprowadzeniu.

### <a name="specify-that-razor-pages-are-at-the-content-root"></a>Określ, że Razor Pages znajdują się w katalogu głównym zawartości

Domyślnie Razor Pages są umieszczane w katalogu */Pages* . Dodaj [WithRazorPagesAtContentRoot](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvcbuilderextensions.withrazorpagesatcontentroot) do [AddMvc](/dotnet/api/microsoft.extensions.dependencyinjection.mvcservicecollectionextensions.addmvc#Microsoft_Extensions_DependencyInjection_MvcServiceCollectionExtensions_AddMvc_Microsoft_Extensions_DependencyInjection_IServiceCollection_) , aby określić, że Razor Pages znajdują się w katalogu głównym zawartości ([ContentRootPath](/dotnet/api/microsoft.aspnetcore.hosting.ihostingenvironment.contentrootpath)) aplikacji:

```csharp
services.AddMvc()
    .AddRazorPagesOptions(options =>
    {
        ...
    })
    .WithRazorPagesAtContentRoot();
```

### <a name="specify-that-razor-pages-are-at-a-custom-root-directory"></a>Określ, że Razor Pages znajdują się w niestandardowym katalogu głównym

Dodaj [WithRazorPagesRoot](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvccorebuilderextensions.withrazorpagesroot) do [AddMvc](/dotnet/api/microsoft.extensions.dependencyinjection.mvcservicecollectionextensions.addmvc#Microsoft_Extensions_DependencyInjection_MvcServiceCollectionExtensions_AddMvc_Microsoft_Extensions_DependencyInjection_IServiceCollection_) , aby określić, że Razor Pages znajdują się w niestandardowym katalogu głównym w aplikacji (podaj ścieżkę względną):

```csharp
services.AddMvc()
    .AddRazorPagesOptions(options =>
    {
        ...
    })
    .WithRazorPagesRoot("/path/to/razor/pages");
```

## <a name="additional-resources"></a>Dodatkowe zasoby

* <xref:index>
* <xref:mvc/views/razor>
* <xref:mvc/controllers/areas>
* <xref:tutorials/razor-pages/razor-pages-start>
* <xref:security/authorization/razor-pages-authorization>
* <xref:razor-pages/razor-pages-conventions>
* <xref:test/razor-pages-tests>
* <xref:mvc/views/partial>
