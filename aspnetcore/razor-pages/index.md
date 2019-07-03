---
title: Wprowadzenie do stron Razor programu ASP.NET Core
author: Rick-Anderson
description: Dowiedz się, jak stron Razor w programie ASP.NET Core umożliwia kodowania scenariuszy skoncentrowane na stronie łatwiejsze i bardziej wydajne niż przy użyciu platformy MVC.
monikerRange: '>= aspnetcore-2.0'
ms.author: riande
ms.date: 04/06/2019
uid: razor-pages/index
ms.openlocfilehash: 419355d670536fef1a38fbcb8ce1fd880c0e9b0d
ms.sourcegitcommit: d6e51c60439f03a8992bda70cc982ddb15d3f100
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/03/2019
ms.locfileid: "67555729"
---
# <a name="introduction-to-razor-pages-in-aspnet-core"></a>Wprowadzenie do stron Razor programu ASP.NET Core

Przez [Rick Anderson](https://twitter.com/RickAndMSFT) i [Ryan Nowak](https://github.com/rynowak)

Strony razor jest nowy aspekt ASP.NET Core MVC, która sprawia, że kodowania skoncentrowane na stronie scenariuszy łatwiejsze i bardziej wydajne.

Jeśli szukasz samouczka, który korzysta z metody Model-View-Controller [Rozpoczynanie pracy z platformą ASP.NET Core MVC](xref:tutorials/first-mvc-app/start-mvc).

Ten dokument zawiera wprowadzenie do stron Razor. Nie jest samouczek krok po kroku. Jeśli znajdziesz niektóre sekcje zbyt zaawansowany, zobacz [Rozpoczynanie pracy ze stronami Razor](xref:tutorials/razor-pages/razor-pages-start). Aby uzyskać omówienie platformy ASP.NET Core, zobacz [wprowadzenie do platformy ASP.NET Core](xref:index).

## <a name="prerequisites"></a>Wymagania wstępne

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

[!INCLUDE[](~/includes/net-core-prereqs-vs2019-2.2.md)]

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

[!INCLUDE[](~/includes/net-core-prereqs-vsc-2.2.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[Visual Studio for Mac](#tab/visual-studio-mac)

[!INCLUDE[](~/includes/net-core-prereqs-mac-2.2.md)]

---

<a name="rpvs17"></a>

## <a name="create-a-razor-pages-project"></a>Utwórz projekt stron Razor

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

Zobacz [Rozpoczynanie pracy ze stronami Razor](xref:tutorials/razor-pages/razor-pages-start) szczegółowe instrukcje dotyczące sposobu tworzenia projektu stron Razor.

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[Visual Studio for Mac](#tab/visual-studio-mac)

::: moniker range=">= aspnetcore-2.1"

Uruchom `dotnet new webapp` z wiersza polecenia.

::: moniker-end

::: moniker range="= aspnetcore-2.0"

Uruchom `dotnet new razor` z wiersza polecenia.

::: moniker-end

Otwórz wygenerowany *.csproj* pliku z programu Visual Studio dla komputerów Mac.

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

::: moniker range=">= aspnetcore-2.1"

Uruchom `dotnet new webapp` z wiersza polecenia.

::: moniker-end

::: moniker range="= aspnetcore-2.0"

Uruchom `dotnet new razor` z wiersza polecenia.

::: moniker-end

---

## <a name="razor-pages"></a>Razor Pages

Strony razor jest włączone w *Startup.cs*:

[!code-cs[](index/sample/RazorPagesIntro/Startup.cs?name=snippet_Startup)]

Należy wziąć pod uwagę strona podstawowa: <a name="OnGet"></a>

[!code-cshtml[](index/sample/RazorPagesIntro/Pages/Index.cshtml)]

Powyższy kod wygląda o wiele [plik widoku Razor](xref:tutorials/first-mvc-app/adding-view) używane w aplikacji ASP.NET Core za pomocą kontrolerów i widoków. Czym różni się on jest `@page` dyrektywy. `@page` sprawia, że plik do Akcja kontrolera MVC — oznacza to, obsługi żądań bezpośrednio, bez przechodzenia przez kontrolera. `@page` musi być pierwszym dyrektywy Razor na stronie. `@page` wpływa na działanie innych konstrukcji Razor.

Podobny strony, `PageModel` klasy, jest wyświetlany w następujące dwa pliki. *Pages/Index2.cshtml* pliku:

[!code-cshtml[](index/sample/RazorPagesIntro/Pages/Index2.cshtml)]

*Pages/Index2.cshtml.cs* modelu strony:

[!code-cs[](index/sample/RazorPagesIntro/Pages/Index2.cshtml.cs)]

Zgodnie z Konwencją `PageModel` plik klasy ma taką samą nazwę jak plik strony Razor za pomocą *.cs* dołączane. Na przykład jest poprzedniej strony Razor *Pages/Index2.cshtml*. Plik zawierający `PageModel` nosi nazwę klasy *Pages/Index2.cshtml.cs*.

Skojarzenia ścieżki adresu URL do strony są określane według lokalizacji strony w systemie plików. W poniższej tabeli przedstawiono ścieżki strony Razor i dopasowywania adresu URL:

| Nazwa pliku i ścieżka               | dopasowanie adresu URL |
| ----------------- | ------------ |
| */Pages/Index.cshtml* | `/` lub `/Index` |
| */Pages/Contact.cshtml* | `/Contact` |
| */Pages/Store/Contact.cshtml* | `/Store/Contact` |
| */Pages/Store/Index.cshtml* | `/Store` lub `/Store/Index` |

Uwagi:

* Środowisko uruchomieniowe szuka plików stron Razor w *stron* folderu domyślnie.
* `Index` jest domyślną stroną, podczas gdy adres URL nie zawiera strony.

## <a name="write-a-basic-form"></a>Zapis formularza podstawowego

Strony razor zaprojektowaną do podejmowania typowe wzorce używane w przeglądarkach, łatwe do wdrożenia podczas kompilowania aplikacji. [Wiązanie modelu](xref:mvc/models/model-binding), [pomocników tagów](xref:mvc/views/tag-helpers/intro)i wszystkich pomocników HTML *po prostu działają* z właściwościami, zdefiniowanej w klasie strony Razor. Należy wziąć pod uwagę strona, która implementuje podstawową "Skontaktuj się z nami" formularza dla `Contact` modelu:

Aby wyświetlić przykłady w niniejszym dokumencie `DbContext` jest inicjowana w [Startup.cs](https://github.com/aspnet/AspNetCore.Docs/blob/master/aspnetcore/razor-pages/index/sample/RazorPagesContacts/Startup.cs#L15-L16) pliku.

[!code-cs[](index/sample/RazorPagesContacts/Startup.cs?highlight=15-16)]

Model danych:

[!code-cs[](index/sample/RazorPagesContacts/Data/Customer.cs)]

Kontekst bazy danych:

[!code-cs[](index/sample/RazorPagesContacts/Data/AppDbContext.cs)]

*Pages/Create.cshtml* Wyświetl plik:

[!code-cshtml[](index/sample/RazorPagesContacts/Pages/Create.cshtml)]

*Pages/Create.cshtml.cs* modelu strony:

[!code-cs[](index/sample/RazorPagesContacts/Pages/Create.cshtml.cs?name=snippet_ALL)]

Zgodnie z Konwencją `PageModel` nosi nazwę klasy `<PageName>Model` i znajduje się w tej samej przestrzeni nazw, gdy ta strona.

`PageModel` Klasa umożliwia oddzielenie logiki strony z jej prezentacji. Definiuje stronę obsługi żądań wysyłanych do strony i dane używane do renderowania strony. Ta separacja pozwala na zarządzanie zależnościami strony za pomocą [wstrzykiwanie zależności](xref:fundamentals/dependency-injection) i [testu jednostkowego](xref:test/razor-pages-tests) stron.

Strona ta zawiera `OnPostAsync` *metody obsługi*, która działa w `POST` żądania (po użytkownik wpisy formularza). Możesz dodać metody obsługi dla dowolnego zlecenie HTTP. Najbardziej typowe procedury obsługi są:

* `OnGet` Aby zainicjować stanu potrzebne dla strony. [OnGet](#OnGet) próbki.
* `OnPost` do obsługi przesyłania formularza.

`Async` Sufiks nazwy jest opcjonalne, ale jest często używana przez Konwencję w funkcji asynchronicznej. `OnPostAsync` Kodu w powyższym przykładzie jest podobny do będzie zwykle napisany w kontrolerze. Powyższy kod jest typowe dla stron Razor. Większość elementów podstawowych MVC, takie jak [wiązanie modelu](xref:mvc/models/model-binding), [weryfikacji](xref:mvc/models/validation), wyniki akcji są współdzielone.  <!-- Review: Ryan, can we get a list of what is shared and what isn't? -->

Poprzedni `OnPostAsync` metody:

[!code-cs[](index/sample/RazorPagesContacts/Pages/Create.cshtml.cs?name=snippet_OnPostAsync)]

Podstawowy przepływ `OnPostAsync`:

Sprawdź, czy błędy sprawdzania poprawności.

* Jeśli nie ma żadnych błędów, Zapisz dane i przekierować.
* Jeśli występują błędy, wyświetlenie strony ponownie przy użyciu komunikatów dotyczących sprawdzania poprawności. Weryfikacja po stronie klienta jest identyczna z tradycyjnych aplikacji ASP.NET Core MVC. W wielu przypadkach błędy sprawdzania poprawności będzie być wykryte na komputerze klienckim i nigdy nie zostały przekazane do serwera.

Po pomyślnym wprowadzeniu danych `OnPostAsync` wywołań metody obsługi `RedirectToPage` metody pomocnika do zwrócenia wystąpienia `RedirectToPageResult`. `RedirectToPage` to nowy wynik akcji, podobnie jak `RedirectToAction` lub `RedirectToRoute`, ale dostosowane dla stron. W poprzednim przykładzie, zostanie przekierowany do strony indeksu głównego (`/Index`). `RedirectToPage` została szczegółowo opisana w [Generowanie adresu URL dla stron](#url_gen) sekcji.

Kiedy przesłanego formularza ma błędy sprawdzania poprawności, (które są przekazywane do serwera),`OnPostAsync` wywołań metody obsługi `Page` metody pomocnika. `Page` Zwraca wystąpienie `PageResult`. Zwracanie `Page` jest podobny do sposobu Zwróć akcje w kontrolerach `View`. `PageResult` jest wartością domyślną <!-- Review  --> zwracany typ metody programu obsługi. Metody obsługi, która zwraca `void` renderuje stronę.

`Customer` Używa właściwości `[BindProperty]` atrybutu zgadzaj się na wiązania modelu.

[!code-cs[](index/sample/RazorPagesContacts/Pages/Create.cshtml.cs?name=snippet_PageModel&highlight=10-11)]

Strony razor domyślnie powiązania właściwości tylko zlecenia-GET. Powiązanie z właściwościami może zmniejszyć ilość kodu, który trzeba napisać. Powiązanie zmniejsza kodu przy użyciu tej samej właściwości do renderowania pól formularza (`<input asp-for="Customer.Name">`) i akceptuje dane wejściowe.

[!INCLUDE[](~/includes/bind-get.md)]

Strona główna (*Index.cshtml*):

[!code-cshtml[](index/sample/RazorPagesContacts/Pages/Index.cshtml)]

Skojarzone `PageModel` klasy (*Index.cshtml.cs*):

[!code-cs[](index/sample/RazorPagesContacts/Pages/Index.cshtml.cs)]

*Index.cshtml* plik zawiera następujące znaczniki, aby utworzyć link edycji dla każdego kontaktu:

[!code-cshtml[](index/sample/RazorPagesContacts/Pages/Index.cshtml?range=21)]

[Pomocnik tagu kotwicy](xref:mvc/views/tag-helpers/builtin-th/anchor-tag-helper) używane `asp-route-{value}` atrybutu, aby wygenerować łącze do strony edytowania. Ten link zawiera dane trasy z kontaktem identyfikatora. Na przykład `http://localhost:5000/Edit/1`. Użyj `asp-area` atrybutu, aby określić obszar. Aby uzyskać więcej informacji, zobacz <xref:mvc/controllers/areas>.

*Pages/Edit.cshtml* pliku:

[!code-cshtml[](index/sample/RazorPagesContacts/Pages/Edit.cshtml?highlight=1)]

Pierwszy wiersz zawiera `@page "{id:int}"` dyrektywy. Ograniczenia routingu`"{id:int}"` informuje strony aby akceptował żądania do strony, które zawierają `int` przekierowywanie danych. Jeśli żądanie strony nie zawiera danych trasy, który może zostać przekonwertowany na `int`, środowisko wykonawcze zwraca błąd HTTP 404 (nie znaleziono). Aby wprowadzić identyfikator jest opcjonalne, należy dołączyć `?` do ograniczenia trasy:

 ```cshtml
@page "{id:int?}"
```

*Pages/Edit.cshtml.cs* pliku:

[!code-cs[](index/sample/RazorPagesContacts/Pages/Edit.cshtml.cs)]

*Index.cshtml* plik zawiera także znaczników, aby utworzyć przycisk Usuń, dla każdego kontaktu klienta:

[!code-cshtml[](index/sample/RazorPagesContacts/Pages/Index.cshtml?range=22-23)]

Gdy przycisk Usuń jest renderowany w języku HTML, jego `formaction` obejmuje parametry:

* Określony przez identyfikator kontaktu klienta z `asp-route-id` atrybutu.
* `handler` Określony przez `asp-page-handler` atrybutu.

Oto przykład przycisku Usuń renderowany przy użyciu klienta skontaktować się z Identyfikatora `1`:

```html
<button type="submit" formaction="/?id=1&amp;handler=delete">delete</button>
```

Po wybraniu przycisku, formularz `POST` żądanie jest wysyłane do serwera. Zgodnie z Konwencją Nazwa metody obsługi jest zaznaczona, na podstawie wartości z `handler` parametru, zgodnie ze schematem `OnPost[handler]Async`.

Ponieważ `handler` jest `delete` w tym przykładzie `OnPostDeleteAsync` metody obsługi jest używany do procesu `POST` żądania. Jeśli `asp-page-handler` jest ustawiona na inną wartość, takich jak `remove`, metodę programu obsługi strony o nazwie `OnPostRemoveAsync` jest zaznaczone.

[!code-cs[](index/sample/RazorPagesContacts/Pages/Index.cshtml.cs?range=26-37)]

`OnPostDeleteAsync` Metody:

* Akceptuje `id` z ciągu zapytania.
* Zapytania bazy danych dla kontaktu klienta z `FindAsync`.
* Jeśli zostanie znaleziony kontakt z klientem, zostali oni usunięci z listy kontaktów klienta. Baza danych jest aktualizowana.
* Wywołania `RedirectToPage` do przekierowania do strony indeksu głównego (`/Index`).

::: moniker range=">= aspnetcore-2.1"

## <a name="mark-page-properties-as-required"></a>Oznacz właściwości strony zgodnie z potrzebami

Właściwości `PageModel` być dekorowane za pomocą [wymagane](/dotnet/api/system.componentmodel.dataannotations.requiredattribute) atrybutu:

[!code-cs[](index/sample/Create.cshtml.cs?highlight=3,15-16)]

Aby uzyskać więcej informacji, zobacz [Walidacja modelu](xref:mvc/models/validation).

## <a name="manage-head-requests-with-the-onget-handler"></a>Zarządzanie żądaniami HEAD za pomocą obsługi OnGet

Żądania HEAD umożliwiają pobieranie nagłówki dla określonego zasobu. Inaczej niż w przypadku żądania GET HEAD żądań nie zwracają treść odpowiedzi.

Zazwyczaj obsługi HEAD jest tworzony i wywołana dla żądania HEAD: 

```csharp
public void OnHead()
{
    HttpContext.Response.Headers.Add("HandledBy", "Handled by OnHead!");
}
```

Jeśli żadna procedura obsługi HEAD (`OnHead`) jest zdefiniowany, stron Razor powraca do wywoływania procedury obsługi strony GET (`OnGet`) w programie ASP.NET Core 2.1 lub nowszej. W programie ASP.NET Core 2.1 i 2.2 to zachowanie dotyczy [SetCompatibilityVersion](xref:mvc/compatibility-version) w `Startup.Configure`:

```csharp
services.AddMvc()
    .SetCompatibilityVersion(Microsoft.AspNetCore.Mvc.CompatibilityVersion.Version_2_1);
```

Generowanie szablonów domyślnych `SetCompatibilityVersion` wywołania platformy ASP.NET Core 2.1 i 2.2.

`SetCompatibilityVersion` efektywnie ustawia opcję stron Razor `AllowMappingHeadRequestsToGetHandler` do `true`.

Zamiast włączenie wszystkich 2.1 zachowania `SetCompatibilityVersion`, użytkownik może jawnie wyrazić zgodę na konkretne zachowania. Poniższy kod zdecyduje się na żądania HEAD mapowanie programu obsługi GET.

```csharp
services.AddMvc()
    .AddRazorPagesOptions(options =>
    {
        options.AllowMappingHeadRequestsToGetHandler = true;
    });
```

::: moniker-end

<a name="xsrf"></a>

## <a name="xsrfcsrf-and-razor-pages"></a>XSRF/CSRF i stron Razor

Nie trzeba pisać kodu dla [antiforgery weryfikacji](xref:security/anti-request-forgery). Antiforgery generowania tokenu i sprawdzanie poprawności są automatycznie uwzględniane stron Razor.

<a name="layout"></a>

## <a name="using-layouts-partials-templates-and-tag-helpers-with-razor-pages"></a>Za pomocą układów, częściowe, szablonów i pomocnicy tagów ze stron Razor

Strony pracować ze wszystkimi funkcjami aparatu widoku Razor. Układy, częściowe, szablony, pomocników tagów *_ViewStart.cshtml*, *_ViewImports.cshtml* działają w taki sam sposób jak w przypadku konwencjonalnych widokami Razor.

Teraz declutter tę stronę, wykorzystując niektóre z tych możliwości.

::: moniker range=">= aspnetcore-2.1"

Dodaj [stronę układu](xref:mvc/views/layout) do *Pages/Shared/_Layout.cshtml*:

::: moniker-end

::: moniker range="= aspnetcore-2.0"

Dodaj [stronę układu](xref:mvc/views/layout) do *Pages/_Layout.cshtml*:

::: moniker-end

[!code-cshtml[](index/sample/RazorPagesContacts2/Pages/_LayoutSimple.cshtml)]

[Układ](xref:mvc/views/layout):

* Kontroluje układ każdej strony (chyba że strona oznacza brak zgody na układ).
* Importuje HTML struktur, takich jak JavaScript i arkusze stylów.

Zobacz [stronę układu](xref:mvc/views/layout) Aby uzyskać więcej informacji.

[Układ](xref:mvc/views/layout#specifying-a-layout) właściwość jest ustawiona *Pages/_ViewStart.cshtml*:

[!code-cshtml[](index/sample/RazorPagesContacts2/Pages/_ViewStart.cshtml)]

::: moniker range=">= aspnetcore-2.1"

Układ jest *stron/Shared* folderu. Strony wyszukać inne widoki (układów, szablony, częściowe) hierarchiczny, w tym samym folderze co bieżącej strony. Układ w *stron/Shared* folderu można używać na dowolnej strony Razor, w obszarze *stron* folderu.

Plik układu powinny przejść *stron/Shared* folderu.

::: moniker-end

::: moniker range="= aspnetcore-2.0"

Układ jest *stron* folderu. Strony wyszukać inne widoki (układów, szablony, częściowe) hierarchiczny, w tym samym folderze co bieżącej strony. Układ w *stron* folderu można używać na dowolnej strony Razor, w obszarze *stron* folderu.

::: moniker-end

Firma Microsoft zaleca **nie** umieścić ten plik układu w *widoków/Shared* folderu. *Widoki/Shared* jest wzorzec widoków MVC. Strony razor są przeznaczone do zależą od hierarchii folderów, nie ścieżka Konwencji.

Wyszukiwanie w widoku ze strony Razor obejmuje *stron* folderu. Układy, szablony i częściowych używasz konwencjonalnych Razor widoków i kontrolerów MVC *po prostu działają*.

Dodaj *Pages/_ViewImports.cshtml* pliku:

[!code-cshtml[](index/sample/RazorPagesContacts2/Pages/_ViewImports.cshtml)]

`@namespace` zostało wyjaśnione w dalszej części tego samouczka. `@addTagHelper` Wiąże dyrektywy [wbudowanych pomocników tagów](xref:mvc/views/tag-helpers/builtin-th/Index) do wszystkich stron w *stron* folderu.

<a name="namespace"></a>

Gdy `@namespace` dyrektywa jest używany jawnie na stronie:

[!code-cshtml[](index/sample/RazorPagesIntro/Pages/Customers/Namespace2.cshtml?highlight=2)]

Dyrektywa ustawia obszar nazw dla strony. `@model` Dyrektywy nie musi zawierać przestrzeń nazw.

Gdy `@namespace` dyrektywy znajduje się w *_ViewImports.cshtml*, określonego obszaru nazw dostarcza prefiksu dla przestrzeni nazw wygenerowanej na stronie, który importuje `@namespace` dyrektywy. Pozostała część wygenerowana przestrzeń nazw (sufiks fragment) jest oddzielona względnej ścieżki między folder zawierający *_ViewImports.cshtml* i folderu zawierającego strony.

Na przykład `PageModel` klasy *Pages/Customers/Edit.cshtml.cs* jawnie określa przestrzeń nazw:

[!code-cs[](index/sample/RazorPagesContacts2/Pages/Customers/Edit.cshtml.cs?name=snippet_namespace)]

*Pages/_ViewImports.cshtml* pliku ustawia następująca przestrzeń nazw:

[!code-cshtml[](index/sample/RazorPagesContacts2/Pages/_ViewImports.cshtml?highlight=1)]

Wygenerowany obszar nazw dla *Pages/Customers/Edit.cshtml* strona Razor jest taka sama jak `PageModel` klasy.

`@namespace` *współpracuje również z konwencjonalnych widokami Razor.*

Oryginalny *Pages/Create.cshtml* Wyświetl plik:

[!code-cshtml[](index/sample/RazorPagesContacts/Pages/Create.cshtml?highlight=2)]

Zaktualizowany interfejs *Pages/Create.cshtml* Wyświetl plik:

[!code-cshtml[](index/sample/RazorPagesContacts2/Pages/Customers/Create.cshtml?highlight=2)]

[Projekt startowy stron Razor](#rpvs17) zawiera *Pages/_ValidationScriptsPartial.cshtml*, która przechwytuje sprawdzania poprawności po stronie klienta.

Aby uzyskać więcej informacji na temat widoki częściowe, zobacz <xref:mvc/views/partial>.

<a name="url_gen"></a>

## <a name="url-generation-for-pages"></a>Generowanie adresu URL dla stron

`Create` Strona, pokazana wcześniej, używa `RedirectToPage`:

[!code-cs[](index/sample/RazorPagesContacts/Pages/Create.cshtml.cs?name=snippet_OnPostAsync&highlight=10)]

Aplikacja ma następującą strukturę pliku/folderu:

* */ Stron*

  * *Index.cshtml*
  * */ Klientów*

    * *Create.cshtml*
    * *Edit.cshtml*
    * *Index.cshtml*

*Pages/Customers/Create.cshtml* i *Pages/Customers/Edit.cshtml* strony przekierowania do *Pages/Index.cshtml* po pomyślnym zakończeniu. Ciąg `/Index` jest częścią identyfikatora URI, aby uzyskać dostęp do poprzedniej strony. Ciąg `/Index` może służyć do generowania identyfikatorów URI *Pages/Index.cshtml* strony. Na przykład:

* `Url.Page("/Index", ...)`
* `<a asp-page="/Index">My Index Page</a>`
* `RedirectToPage("/Index")`

Nazwa strony jest ścieżką do strony z katalogu głównego */strony* folder, w tym wiodący `/` (na przykład `/Index`). Poprzednich przykładów generowania adresu URL oferują rozszerzone opcje i funkcji za pośrednictwem hardcoding adresu URL. Generowanie adresu URL używa [routingu](xref:mvc/controllers/routing) można wygenerować i kodowanie parametrów, zgodnie z jak trasy jest zdefiniowany w ścieżce docelowej.

Generowanie adresu URL dla stron obsługuje nazwy względnej. W poniższej tabeli przedstawiono strony indeksu, który wybrano z różnymi `RedirectToPage` parametry z *Pages/Customers/Create.cshtml*:

| RedirectToPage(x)| Strona |
| ----------------- | ------------ |
| RedirectToPage("/Index") | *Indeks strony* |
| RedirectToPage("./Index"); | *Strony/klientów/indeksu* |
| RedirectToPage("../Index") | *Indeks strony* |
| RedirectToPage("Index")  | *Strony/klientów/indeksu* |

`RedirectToPage("Index")`, `RedirectToPage("./Index")`, i `RedirectToPage("../Index")` są *względne nazwy*. `RedirectToPage` Parametr jest *połączone* ze ścieżką bieżącej strony, aby obliczyć nazwę strony docelowej.  <!-- Review: Original had The provided string is combined with the page name of the current page to compute the name of the destination page.  page name, not page path -->

Nazwa względna łączenia jest przydatne w przypadku, gdy tworzenia witryn z złożonej strukturze. Jeśli używasz względne nazwy do połączenia między stronami w folderze, można zmienić nazwę tego folderu. Wszystkie łącza nadal działać (ponieważ nie obejmują one nazwę folderu).

::: moniker range=">= aspnetcore-2.1"

Przekierowywanie do strony w innej [obszaru](xref:mvc/controllers/areas), określ obszar:

```csharp
RedirectToPage("/Index", new { area = "Services" });
```

Aby uzyskać więcej informacji, zobacz <xref:mvc/controllers/areas>.

## <a name="viewdata-attribute"></a>Atrybut viewData

Dane mogą być przekazywane do stron z [ViewDataAttribute](/dotnet/api/microsoft.aspnetcore.mvc.viewdataattribute). Właściwości na kontrolerach ani w modelach strony Razor ozdobione `[ViewData]` ich wartości przechowywane i ładowane z [ViewDataDictionary](/dotnet/api/microsoft.aspnetcore.mvc.viewfeatures.viewdatadictionary).

W poniższym przykładzie `AboutModel` zawiera `Title` ozdobione właściwość `[ViewData]`. `Title` Właściwość jest ustawiona na tytuł strony informacje:

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

Na stronie informacje dostępu `Title` właściwość jako właściwość modelu:

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

## <a name="tempdata"></a>TempData

Udostępnia platformy ASP.NET Core [TempData](/dotnet/api/microsoft.aspnetcore.mvc.controller.tempdata?view=aspnetcore-2.0#Microsoft_AspNetCore_Mvc_Controller_TempData) właściwość [kontrolera](/dotnet/api/microsoft.aspnetcore.mvc.controller). Ta właściwość przechowuje dane, dopóki nie jest do odczytu. `Keep` i `Peek` metody może służyć do sprawdzenia danych bez usuwania. `TempData` jest przydatne w przypadku przekierowania, gdy dane są potrzebne dla więcej niż jedno żądanie.

`[TempData]` Atrybut jest nowego w programie ASP.NET Core 2.0 i jest obsługiwane na stronach i kontrolerów.

Poniższy kod ustawia wartość `Message` przy użyciu `TempData`:

[!code-cs[](index/sample/RazorPagesContacts2/Pages/Customers/CreateDot.cshtml.cs?highlight=10-11,25&name=snippet_Temp)]

Następujące znaczniki w *Pages/Customers/Index.cshtml* pliku wyświetlana jest wartość `Message` przy użyciu `TempData`.

```cshtml
<h3>Msg: @Model.Message</h3>
```

*Pages/Customers/Index.cshtml.cs* stosowany model strony `[TempData]` atrybutu `Message` właściwości.

```cs
[TempData]
public string Message { get; set; }
```

Aby uzyskać więcej informacji, zobacz [TempData](xref:fundamentals/app-state#tempdata) .

<a name="mhpp"></a>

## <a name="multiple-handlers-per-page"></a>Procedury obsługi wielu na stronę

Następująca strona generuje kod znaczników dla dwóch stron przy użyciu programów obsługi `asp-page-handler` Pomocnik tagu:

[!code-cshtml[](index/sample/RazorPagesContacts2/Pages/Customers/CreateFATH.cshtml?highlight=12-13)]

<!-- Review: the FormActionTagHelper applies to all <form /> elements on a Razor page, even when there's no `asp-` attribute   -->

Formularz w poprzednim przykładzie ma dwa przesłać przyciski, za pomocą każdego `FormActionTagHelper` do przesłania do innego adresu URL. `asp-page-handler` Atrybutu jest uzupełnieniem do `asp-page`. `asp-page-handler` generuje adresy URL, którzy przesłali do każdej metody obsługi zdefiniowane przez stronę. `asp-page` nie jest określona, ponieważ plik jest tworzenie łączy do bieżącej strony.

Model strony:

[!code-cs[](index/sample/RazorPagesContacts2/Pages/Customers/CreateFATH.cshtml.cs?highlight=20,32)]

W poprzednim kodzie użyto *o nazwie metody obsługi*. Metody obsługi o nazwie są tworzone przez przyjęcie tekst nazwę po `On<HTTP Verb>` i przed `Async` (jeśli istnieje). W poprzednim przykładzie, metody strony są OnPost**JoinList**Async i OnPost**JoinListUC**Async. Za pomocą *OnPost* i *Async* usunięty, są nazwy programów obsługi `JoinList` i `JoinListUC`.

[!code-cshtml[](index/sample/RazorPagesContacts2/Pages/Customers/CreateFATH.cshtml?range=12-13)]

Za pomocą poprzedniego kodu Ścieżka adresu URL, które są przesyłane `OnPostJoinListAsync` jest `http://localhost:5000/Customers/CreateFATH?handler=JoinList`. Ścieżka adresu URL, które są przesyłane `OnPostJoinListUCAsync` jest `http://localhost:5000/Customers/CreateFATH?handler=JoinListUC`.

## <a name="custom-routes"></a>Trasy niestandardowe

Użyj `@page` dyrektywę:

* Określ niestandardowe trasy do strony. Na przykład można ustawić trasy do strony informacje `/Some/Other/Path` z `@page "/Some/Other/Path"`.
* Dołącz segmentów do trasy domyślnej strony. Na przykład "item" segmentu można dodać do trasy domyślnej strony z `@page "item"`.
* Dodaj parametry do trasy domyślnej strony. Na przykład parametru ID, `id`, może być wymagane dla strony z `@page "{id}"`.

Ścieżka względem katalogu głównego, w wyznaczonym przez tyldy (`~`) na początku ścieżki jest obsługiwana. Na przykład `@page "~/Some/Other/Path"` jest taka sama jak `@page "/Some/Other/Path"`.

Możesz zmienić ciągu zapytania `?handler=JoinList` w adresie URL do segmentu trasy `/JoinList` , określając szablon trasy `@page "{handler?}"`.

Jeśli nie chcesz, aby ciąg zapytania `?handler=JoinList` w adresie URL, możesz zmienić trasy, aby umieścić nazwę programu obsługi w część ścieżki adresu URL. Trasy można dostosować, dodając szablon trasy, ujęte w podwójny cudzysłów po `@page` dyrektywy.

[!code-cshtml[](index/sample/RazorPagesContacts2/Pages/Customers/CreateRoute.cshtml?highlight=1)]

Za pomocą poprzedniego kodu Ścieżka adresu URL, które są przesyłane `OnPostJoinListAsync` jest `http://localhost:5000/Customers/CreateFATH/JoinList`. Ścieżka adresu URL, które są przesyłane `OnPostJoinListUCAsync` jest `http://localhost:5000/Customers/CreateFATH/JoinListUC`.

`?` Następujące `handler` oznacza, że parametr trasy jest opcjonalny.

## <a name="configuration-and-settings"></a>Konfiguracja i ustawienia

Aby skonfigurować opcje zaawansowane, należy użyć metody rozszerzenia `AddRazorPagesOptions` w Konstruktorze MVC:

[!code-cs[](index/sample/RazorPagesContacts/StartupAdvanced.cs?name=snippet_1)]

Obecnie możesz używać `RazorPagesOptions` katalog główny strony lub Dodaj konwencje modelu aplikacji dla stron. Firma Microsoft będzie włączyć więcej rozszerzeń w ten sposób w przyszłości.

Przeprowadzać prekompilowanie widoków, zobacz [kompilacja widoku Razor](xref:mvc/views/view-compilation) .

[Pobieranie i wyświetlanie przykładowy kod](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/razor-pages/index/sample).

Zobacz [Rozpoczynanie pracy ze stronami Razor](xref:tutorials/razor-pages/razor-pages-start), która jest oparta na wprowadzeniu.

### <a name="specify-that-razor-pages-are-at-the-content-root"></a>Określ, czy w katalogu głównym zawartości stron Razor

Domyślnie strony Razor są umieszczone w */strony* katalogu. Dodaj [WithRazorPagesAtContentRoot](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvcbuilderextensions.withrazorpagesatcontentroot) do [AddMvc](/dotnet/api/microsoft.extensions.dependencyinjection.mvcservicecollectionextensions.addmvc#Microsoft_Extensions_DependencyInjection_MvcServiceCollectionExtensions_AddMvc_Microsoft_Extensions_DependencyInjection_IServiceCollection_) do określenia, czy strony Razor zawartości katalogu głównego ([ContentRootPath](/dotnet/api/microsoft.aspnetcore.hosting.ihostingenvironment.contentrootpath)) aplikacji:

```csharp
services.AddMvc()
    .AddRazorPagesOptions(options =>
    {
        ...
    })
    .WithRazorPagesAtContentRoot();
```

### <a name="specify-that-razor-pages-are-at-a-custom-root-directory"></a>Określ, czy w katalogu głównym niestandardowych stron Razor

Dodaj [WithRazorPagesRoot](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvccorebuilderextensions.withrazorpagesroot) do [AddMvc](/dotnet/api/microsoft.extensions.dependencyinjection.mvcservicecollectionextensions.addmvc#Microsoft_Extensions_DependencyInjection_MvcServiceCollectionExtensions_AddMvc_Microsoft_Extensions_DependencyInjection_IServiceCollection_) do określenia, czy strony Razor w katalogu głównym niestandardowego w aplikacji (podaj ścieżkę względną):

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
