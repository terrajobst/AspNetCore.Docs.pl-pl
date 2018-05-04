---
title: Wprowadzenie do platformy ASP.NET Core stron Razor
author: Rick-Anderson
description: Dowiedz się, jak Razor strony platformy ASP.NET Core umożliwia kodowania scenariusze strony łatwiejsze i bardziej wydajnej pracy niż przy użyciu platformy MVC.
manager: wpickett
monikerRange: '>= aspnetcore-2.0'
ms.author: riande
ms.date: 09/12/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: get-started-article
uid: mvc/razor-pages/index
ms.openlocfilehash: 08866543d5b510b86c6af1896a9bd41ae0053ecf
ms.sourcegitcommit: 5130b3034165f5cf49d829fe7475a84aa33d2693
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 05/03/2018
---
# <a name="introduction-to-razor-pages-in-aspnet-core"></a>Wprowadzenie do platformy ASP.NET Core stron Razor

Przez [Rick Anderson](https://twitter.com/RickAndMSFT) i [Ryan Nowak](https://github.com/rynowak)

Stron razor to nowa funkcja platformy ASP.NET Core MVC umożliwia kodowanie strony scenariusze łatwiejsze i bardziej wydajnej pracy.

Jeśli szukasz samouczka, który korzysta z podejścia Model-View-Controller, zobacz [Rozpoczynanie pracy z platformą ASP.NET MVC Core](xref:tutorials/first-mvc-app/start-mvc).

Ten dokument zawiera wprowadzenie do stron Razor. Nie jest samouczek krok po kroku. Jeśli możesz znaleźć sekcje zbyt zaawansowanych, zobacz [wprowadzenie stron Razor](xref:tutorials/razor-pages/razor-pages-start). Omówienie platformy ASP.NET Core, zobacz [wprowadzenie do platformy ASP.NET Core](xref:index).

## <a name="prerequisites"></a>Wymagania wstępne

[!INCLUDE [](~/includes/net-core-prereqs.md)]

<a name="rpvs17"></a>

## <a name="creating-a-razor-pages-project"></a>Tworzenie projektu stron Razor

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio) 

Zobacz [wprowadzenie stron Razor](xref:tutorials/razor-pages/razor-pages-start) szczegółowe informacje dotyczące sposobu tworzenia stron Razor projektu za pomocą programu Visual Studio.

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[Visual Studio for Mac](#tab/visual-studio-mac)

Uruchom `dotnet new razor` z wiersza polecenia.

Otwórz wygenerowany *.csproj* plików z programu Visual Studio dla komputerów Mac.

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code) 

Uruchom `dotnet new razor` z wiersza polecenia.

# <a name="net-core-clitabnetcore-cli"></a>[.NET Core CLI](#tab/netcore-cli) 

Uruchom `dotnet new razor` z wiersza polecenia.

---

## <a name="razor-pages"></a>Stron razor

Stron razor jest włączone w *Startup.cs*:

[!code-cs[](index/sample/RazorPagesIntro/Startup.cs?name=snippet_Startup)]

Należy wziąć pod uwagę strony podstawowej: <a name="OnGet"></a>

[!code-cshtml[](index/sample/RazorPagesIntro/Pages/Index.cshtml)]

Poprzedni kod znacznie wygląda jak plik widoku Razor. Jest to, co ułatwia różnych `@page` dyrektywy. `@page` powoduje, że plik na akcję MVC — czyli obsługi żądań bezpośrednio, bez przechodzenia przez kontroler. `@page` musi być pierwszym dyrektywy Razor na stronie. `@page` wpływa na działanie innych konstrukcji Razor.

Podobne strony przy użyciu `PageModel` klasa, jest wyświetlany w obszarze następujące dwa pliki. *Pages/Index2.cshtml* pliku:

[!code-cshtml[](index/sample/RazorPagesIntro/Pages/Index2.cshtml)]

*Pages/Index2.cshtml.cs* modelu strony:

[!code-cs[](index/sample/RazorPagesIntro/Pages/Index2.cshtml.cs)]

Według konwencji `PageModel` pliku klasy ma taką samą nazwę jak plik Razor strony z *.cs* dołączane. Na przykład na poprzedniej stronie aparatu Razor jest *Pages/Index2.cshtml*. Plik zawierający `PageModel` nosi nazwę klasy *Pages/Index2.cshtml.cs*.

Skojarzenia ścieżki adresu URL do strony zależą od lokalizacji strony w systemie plików. W poniższej tabeli przedstawiono ścieżki Razor strony i dopasowywania adresu URL:

| Nazwa i ścieżka pliku               | Dopasowywanie adresu URL |
| ----------------- | ------------ |
| */Pages/Index.cshtml* | `/` lub `/Index` |
| */Pages/Contact.cshtml* | `/Contact` |
| */Pages/Store/Contact.cshtml* | `/Store/Contact` |
| */Pages/Store/Index.cshtml* | `/Store` lub `/Store/Index` |

Uwagi:

* Środowisko uruchomieniowe wyszukuje pliki stron Razor w *stron* folderu domyślnie.
* `Index` jest domyślną stronę, gdy adres URL nie zawiera strony.

## <a name="writing-a-basic-form"></a>Zapisywanie formularza podstawowego

Funkcje stron razor ułatwiają typowe wzorce używane w przeglądarkach łatwe. [Model powiązania](xref:mvc/models/model-binding), [pomocników tagów](xref:mvc/views/tag-helpers/intro)i wszystkich pomocników HTML *tylko pracy* z właściwościami zdefiniowana w klasie Razor strony. Należy wziąć pod uwagę strona, która implementuje podstawowego "Skontaktuj się z nami" tworzą dla `Contact` modelu:

Aby wyświetlić przykłady w tym dokumencie `DbContext` został zainicjowany w [Startup.cs](https://github.com/aspnet/Docs/blob/master/aspnetcore/mvc/razor-pages/index/sample/RazorPagesContacts/Startup.cs#L15-L16) pliku.

[!code-cs[](index/sample/RazorPagesContacts/Startup.cs?highlight=15-16)]

Model danych:

[!code-cs[](index/sample/RazorPagesContacts/Data/Customer.cs)]

Kontekst bazy danych:

[!code-cs[](index/sample/RazorPagesContacts/Data/AppDbContext.cs)]

*Pages/Create.cshtml* pliku widoku:

[!code-cshtml[](index/sample/RazorPagesContacts/Pages/Create.cshtml)]

*Pages/Create.cshtml.cs* modelu strony:

[!code-cs[](index/sample/RazorPagesContacts/Pages/Create.cshtml.cs?name=snippet_ALL)]

Według konwencji `PageModel` nosi nazwę klasy `<PageName>Model` i znajduje się w tej samej przestrzeni nazw jako strony.

`PageModel` Klasa umożliwia oddzielenie logiki strony z jego prezentacji. Definiuje stronę obsługi żądania wysyłane na stronie i dane używane do renderowania strony. Ta separacja umożliwia zarządzanie zależności strony za pomocą [iniekcji zależności](xref:fundamentals/dependency-injection) i [testu jednostkowego](xref:testing/razor-pages-testing) stron.

Na stronie `OnPostAsync` *metoda obsługi*, która działa w `POST` żąda (gdy użytkownik zapisuje formularz). Można dodać obsługi metod dla dowolnej zlecenie HTTP. Są najczęściej programów obsługi:

* `OnGet` Aby zainicjować stanu potrzebne dla strony. [OnGet](#OnGet) próbki.
* `OnPost` do obsługi przesyłania formularza.

`Async` Sufiks nazwy jest opcjonalne, ale często używane przez Konwencję dla funkcji asynchronicznej. `OnPostAsync` Kodu w poprzednim przykładzie wygląda podobnie do co zwykle piszesz w kontrolerze. Poprzedni kod jest typowe dla stron Razor. Większość podstawowych MVC, takich jak [modelu powiązania](xref:mvc/models/model-binding), [weryfikacji](xref:mvc/models/validation), i są współdzielone wyników akcji.  <!-- Review: Ryan, can we get a list of what is shared and what isn't? -->

Poprzedni `OnPostAsync` metody:

[!code-cs[](index/sample/RazorPagesContacts/Pages/Create.cshtml.cs?name=snippet_OnPostAsync)]

Podstawowy przepływ `OnPostAsync`:

Sprawdź błędy sprawdzania poprawności.

*  Jeśli nie ma żadnych błędów, Zapisz dane i przekierowanie.
*  Jeśli wystąpią błędy, Wyświetl stronę ponownie, podając komunikatów dotyczących sprawdzania poprawności. Weryfikacja po stronie klienta jest identyczna jak tradycyjne aplikacje platformy ASP.NET Core MVC. W wielu przypadkach błędy sprawdzania poprawności może być wykryte na komputerze klienckim i nigdy nie zostały przekazane do serwera.

Po pomyślnym wprowadzeniu danych `OnPostAsync` wywołań metody obsługi `RedirectToPage` metody pomocnika do zwrócenia wystąpienia klasy `RedirectToPageResult`. `RedirectToPage` jest nowy wynik akcji, podobnie jak `RedirectToAction` lub `RedirectToRoute`, ale dostosowanych stron. W poprzednim przykładzie przekierowuje do strony indeksu głównego (`/Index`). `RedirectToPage` została szczegółowo opisana w [generowania adresu URL dla stron](#url_gen) sekcji.

Jeśli przesłanego formularza zawiera błędy sprawdzania poprawności (które są przekazywane do serwera),`OnPostAsync` wywołań metody obsługi `Page` metody pomocnika. `Page` Zwraca wystąpienie klasy `PageResult`. Zwracanie `Page` jest podobny do sposób zwrócenia akcje w kontrolerach `View`. `PageResult` Wartość domyślna to <!-- Review  --> zwracany typ metody obsługi. Metoda obsługi, która zwraca `void` renderuje stronę.

`Customer` Używa właściwości `[BindProperty]` atrybutu korzystania z wiązania modelu.

[!code-cs[](index/sample/RazorPagesContacts/Pages/Create.cshtml.cs?name=snippet_PageModel&highlight=10-11)]

Stron razor domyślnie powiązania właściwości tylko z innych niż GET zleceń. Powiązanie właściwości może zmniejszyć ilość kodu, które trzeba zapisać. Powiązanie zmniejsza kodu przy użyciu tej samej właściwości do renderowania pól formularza (`<input asp-for="Customer.Name" />`) i akceptuje dane wejściowe.

> [!NOTE]
> Ze względów bezpieczeństwa należy zgadzaj się na wiązanie danych żądania GET do strony właściwości modelu. Sprawdź dane wejściowe użytkownika przed zamapowaniem ją do właściwości. Zgody na korzystanie z to zachowanie jest przydatne podczas kompilowania funkcji, które zależą od wartości ciągu lub trasy kwerendy.
>
> Aby powiązać właściwość na żądania GET, ustaw `[BindProperty]` atrybutu `SupportsGet` właściwości `true`: `[BindProperty(SupportsGet = true)]`

Strona główna (*Index.cshtml*):

[!code-cshtml[](index/sample/RazorPagesContacts/Pages/Index.cshtml)]

Kod związany z *Index.cshtml.cs* pliku:

[!code-cs[](index/sample/RazorPagesContacts/Pages/Index.cshtml.cs)]

*Index.cshtml* pliku zawiera następujące znaczniki, aby utworzyć link edycji dla każdego kontakt:

[!code-cshtml[](index/sample/RazorPagesContacts/Pages/Index.cshtml?range=21)]

[Pomocnika Tag kotwicy](xref:mvc/views/tag-helpers/builtin-th/anchor-tag-helper) używane `asp-route-{value}` atrybut do generowania łącza do edycji strony. Ten link zawiera dane trasy, o kontakt identyfikatora. Na przykład `http://localhost:5000/Edit/1`.

*Pages/Edit.cshtml* pliku:

[!code-cshtml[](index/sample/RazorPagesContacts/Pages/Edit.cshtml?highlight=1)]

Pierwszy wiersz zawiera `@page "{id:int}"` dyrektywy. Ograniczenie routingu`"{id:int}"` informuje strony do akceptowania żądań do strony, które zawierają `int` danych trasy. Jeśli żądanie do strony nie zawiera danych trasy, który może zostać przekonwertowany na `int`, środowisko uruchomieniowe zwraca błąd HTTP 404 (nie znaleziono). Aby wprowadzić identyfikator opcjonalne, dołącz `?` ograniczenia trasy:

 ```cshtml
@page "{id:int?}"
```

*Pages/Edit.cshtml.cs* pliku:

[!code-cs[](index/sample/RazorPagesContacts/Pages/Edit.cshtml.cs)]

*Index.cshtml* plik zawiera także znaczników do utworzenia przycisk Usuń widoczny dla każdego kontakt klienta:

[!code-cshtml[](index/sample/RazorPagesContacts/Pages/Index.cshtml?range=22-23)]

Przycisk usuwania jest renderowany w języku HTML, jego `formaction` zawiera parametry:

* Określony przez identyfikator kontaktu klienta z `asp-route-id` atrybutu.
* `handler` Określonego przez `asp-page-handler` atrybutu.

Poniżej przedstawiono przykładowy przycisk usuwania renderowany z klientem skontaktuj się z Identyfikatorem `1`:

```html
<button type="submit" formaction="/?id=1&amp;handler=delete">delete</button>
```

Jeśli przycisk jest zaznaczony, formularz `POST` wysłaniu żądania do serwera. Według Konwencji wybrano nazwę metody obsługi na podstawie wartości `handler` parametru zgodnie ze schematem `OnPost[handler]Async`.

Ponieważ `handler` jest `delete` w tym przykładzie `OnPostDeleteAsync` metoda obsługi jest używany do procesu `POST` żądania. Jeśli `asp-page-handler` ustawiono innej wartości, takich jak `remove`, strona metody obsługi o nazwie `OnPostRemoveAsync` jest zaznaczone.

[!code-cs[](index/sample/RazorPagesContacts/Pages/Index.cshtml.cs?range=26-37)]

`OnPostDeleteAsync` Metody:

* Akceptuje `id` z ciągu zapytania.
* Wysyła zapytanie do bazy danych dla klienta kontakt z `FindAsync`.
* Jeśli zostanie znaleziony kontaktowe klienta, są one usunięte z listy kontaktów klienta. Baza danych jest aktualizowana.
* Wywołania `RedirectToPage` ma nastąpić przekierowanie do strony indeksu głównego (`/Index`).

::: moniker range=">= aspnetcore-2.1"
## <a name="manage-head-requests-with-the-onget-handler"></a>Zarządzaj żądaniami HEAD z obsługą OnGet

Zwykle HEAD program obsługi jest tworzony i wywołana dla żądania HEAD:

```csharp
public void OnHead()
{
    HttpContext.Response.Headers.Add("HandledBy", "Handled by OnHead!");
}
```

Jeśli bez obsługi HEAD (`OnHead`) jest zdefiniowany, stron Razor nastąpi powrót do wywoływania obsługi strony GET (`OnGet`) platformy ASP.NET Core 2.1 lub nowszej. Zgódź się na zachowanie w przypadku [metody SetCompatibilityVersion](xref:fundamentals/startup#setcompatibilityversion-for-aspnet-core-mvc) w `Startup.Configure` dla platformy ASP.NET Core 2.1 2.x:

```csharp
services.AddMvc()
    .SetCompatibilityVersion(Microsoft.AspNetCore.Mvc.CompatibilityVersion.Version_2_1);
```

`SetCompatibilityVersion` efektywnie ustawia opcję stron Razor `AllowMappingHeadRequestsToGetHandler` do `true`. Zachowanie jest zdecydować się na do wersji platformy ASP.NET Core 3.0 w wersji zapoznawczej 1 lub nowszym. Każda wersja główna platformy ASP.NET Core przyjmuje wszystkie zachowania wersji poprawki poprzedniej wersji.

Globalne zdecydować się na zachowanie poprawki wersje 2.1 do 2.x można uniknąć z konfiguracją aplikacji, która mapuje żądania HEAD do obsługi GET. Ustaw `AllowMappingHeadRequestsToGetHandler` stron Razor opcji w celu `true` bez wywoływania elementu `SetCompatibilityVersion` w `Startup.Configure`:

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

Nie masz do pisania kodu dla [antiforgery weryfikacji](xref:security/anti-request-forgery). Antiforgery generowania tokenów i weryfikacja są automatycznie umieszczane w stron Razor.

<a name="layout"></a>
## <a name="using-layouts-partials-templates-and-tag-helpers-with-razor-pages"></a>Za pomocą układów, częściowe, szablonów i pomocników tagów Razor strony

Strony działać z wszystkimi funkcjami aparatu widoku Razor. Układy, częściowe, szablony, pomocników tagów *_ViewStart.cshtml*, *_ViewImports.cshtml* pracy w taki sam sposób jak w przypadku konwencjonalnych widokami Razor.

Korzystając z tych funkcji umożliwia declutter tej strony.

Dodaj [układ strony](xref:mvc/views/layout) do *Pages/_Layout.cshtml*:

[!code-cshtml[](index/sample/RazorPagesContacts2/Pages/_LayoutSimple.cshtml)]

[Układu](xref:mvc/views/layout):

* Określa układ każdej strony (o ile nie zdecyduje strony poza układ).
* Importuje struktury HTML, takich jak JavaScript i arkusze stylów.

Zobacz [układ strony](xref:mvc/views/layout) Aby uzyskać więcej informacji.

[Układu](xref:mvc/views/layout#specifying-a-layout) właściwość jest ustawiona *Pages/_ViewStart.cshtml*:

[!code-cshtml[](index/sample/RazorPagesContacts2/Pages/_ViewStart.cshtml)]

**Uwaga:** układ jest *stron* folderu. Strony wyszukać innych widoków (układy, szablony, częściowe) hierarchicznie, uruchamianie w tym samym folderze co bieżąca strona. Układ w *stron* folderu można używać z dowolnej strony Razor, w obszarze *stron* folderu.

Firma Microsoft zaleca **nie** umieścić plik układu w *widoków/Shared* folderu. *Widoki/Shared* jest wzorzec widoków MVC. Stron razor są przeznaczone do zależą od hierarchii folderów, nie ścieżkę Konwencji.

Widok wyszukiwania ze strony Razor obejmuje *stron* folderu. Układy, szablonów i częściowe jest używany z konwencjonalnej Razor widoków i kontrolerów MVC *tylko pracy*.

Dodaj *Pages/_ViewImports.cshtml* pliku:

[!code-cshtml[](index/sample/RazorPagesContacts2/Pages/_ViewImports.cshtml)]

`@namespace` znajduje się w dalszej części tego samouczka. `@addTagHelper` Dyrektywy dostarcza [wbudowanych pomocników tagów](xref:mvc/views/tag-helpers/builtin-th/Index) dla wszystkich stron w *stron* folderu.

<a name="namespace"></a>

Gdy `@namespace` dyrektywa jest używana jawnie na stronie:

[!code-cshtml[](index/sample/RazorPagesIntro/Pages/Customers/Namespace2.cshtml?highlight=2)]

Dyrektywa ustawia obszar nazw dla strony. `@model` Dyrektywy nie musi obejmować przestrzeni nazw.

Gdy `@namespace` dyrektywy znajduje się w *_ViewImports.cshtml*, określonego obszaru nazw dostarcza prefiksu dla przestrzeni nazw wygenerowane na stronie, który importuje `@namespace` dyrektywy. Pozostała część wygenerowanej (część sufiks) jest oddzielona kropkami ścieżki względnej między folder zawierający *_ViewImports.cshtml* i folderu zawierającego strony.

Na przykład kodzie pliku *Pages/Customers/Edit.cshtml.cs* jawnie Ustawia obszar nazw:

[!code-cs[](index/sample/RazorPagesContacts2/Pages/Customers/Edit.cshtml.cs?name=snippet_namespace)]

*Pages/_ViewImports.cshtml* pliku ustawia następujących nazw:

[!code-cshtml[](index/sample/RazorPagesContacts2/Pages/_ViewImports.cshtml?highlight=1)]

Wygenerowany obszar nazw dla *Pages/Customers/Edit.cshtml* Razor strony jest taka sama jak pliku CodeBehind. `@namespace` Dyrektywa została zaprojektowana tak klasy C# dodane do projektu i stron wygenerowany kod *tylko pracy* bez dodawania `@using` dyrektywy do pliku CodeBehind.

**Uwaga:** `@namespace` współdziała również z konwencjonalnej widokami Razor.

Oryginalna *Pages/Create.cshtml* pliku widoku:

[!code-cshtml[](index/sample/RazorPagesContacts/Pages/Create.cshtml?highlight=2)]

Zaktualizowany interfejs *Pages/Create.cshtml* pliku widoku:

[!code-cshtml[](index/sample/RazorPagesContacts2/Pages/Customers/Create.cshtml?highlight=2)]

[Projektu starter stron Razor](#rpvs17) zawiera *Pages/_ValidationScriptsPartial.cshtml*, który przechwytuje sprawdzania poprawności po stronie klienta.

<a name="url_gen"></a>

## <a name="url-generation-for-pages"></a>Generowania adresu URL dla stron

`Create` Strony pokazana wcześniej, używa `RedirectToPage`:

[!code-cs[](index/sample/RazorPagesContacts/Pages/Create.cshtml.cs?name=snippet_OnPostAsync&highlight=10)]

Aplikacja ma następującą strukturę plik lub folder:

* */ Stron*

  * *Index.cshtml*
  * */ Klientów*

    * *Create.cshtml*
    * *Edit.cshtml*
    * *Index.cshtml*

*Pages/Customers/Create.cshtml* i *Pages/Customers/Edit.cshtml* stron przekierowania do *Pages/Index.cshtml* po pomyślnym. Ciąg `/Index` jest częścią identyfikatora URI uzyskać dostęp do poprzedniej strony. Ciąg `/Index` może służyć do generowania identyfikatorów URI *Pages/Index.cshtml* strony. Na przykład:

* `Url.Page("/Index", ...)`
* `<a asp-page="/Index">My Index Page</a>`
* `RedirectToPage("/Index")`

Nazwa strony jest ścieżka do strony z katalogu głównego */strony* folderze (w tym na początku `/`, na przykład `/Index`). Poprzedniej próbki generowania adresu URL są bardziej zaawansowanej funkcji niż hardcoding tylko adres URL. Używa generowania adresu URL [routingu](xref:mvc/controllers/routing) i generowanie i kodowanie parametry zgodnie z sposób definiowania trasy w ścieżce docelowej.

Generowania adresu URL dla stron obsługuje nazw względnych. W poniższej tabeli przedstawiono stronę indeksu, która jest zaznaczone z różnych `RedirectToPage` parametry z *Pages/Customers/Create.cshtml*:

| RedirectToPage(x)| Strona |
| ----------------- | ------------ |
| RedirectToPage("/Index") | *Strony/indeksu* |
| RedirectToPage("./Index"); | *Strony/klientów/indeksu* |
| RedirectToPage("../Index") | *Strony/indeksu* |
| RedirectToPage("Index")  | *Strony/klientów/indeksu* |

`RedirectToPage("Index")`, `RedirectToPage("./Index")`, i `RedirectToPage("../Index")` są <em>względne nazwy</em>. `RedirectToPage` Parametr jest <em>łączyć</em> ze ścieżką bieżącej strony do obliczenia nazwę strony docelowej.  <!-- Review: Original had The provided string is combined with the page name of the current page to compute the name of the destination page.  page name, not page path -->

Nazwa względna konsolidacji jest przydatne, gdy tworzenie witryn ze strukturą złożonych. Jeśli używasz nazwy względne do połączenia między stronami w folderze, można zmienić nazwę tego folderu. Wszystkie linki nadal działać (ponieważ one nie obejmować nazwę folderu).

## <a name="tempdata"></a>TempData

Przedstawia platformy ASP.NET Core [TempData](/dotnet/api/microsoft.aspnetcore.mvc.controller.tempdata?view=aspnetcore-2.0#Microsoft_AspNetCore_Mvc_Controller_TempData) właściwość [kontrolera](/dotnet/api/microsoft.aspnetcore.mvc.controller). Ta właściwość przechowuje dane, dopóki nie jest do odczytu. `Keep` i `Peek` metod można użyć do sprawdzenia danych bez usuwania. `TempData` jest przydatne w przypadku przekierowania, gdy dane są potrzebne dla więcej niż jednego żądania.

`[TempData]` Jest nowy w programie ASP.NET 2.0 Core i jest obsługiwane na stronach i kontrolerów.

Poniższy kod ustawia wartość `Message` przy użyciu `TempData`:

[!code-cs[](index/sample/RazorPagesContacts2/Pages/Customers/CreateDot.cshtml.cs?highlight=10-11,25&name=snippet_Temp)]

Następujący kod w *Pages/Customers/Index.cshtml* plik zawiera wartość `Message` przy użyciu `TempData`.

```cshtml
<h3>Msg: @Model.Message</h3>
```

*Pages/Customers/Index.cshtml.cs* stosuje modelu strony `[TempData]` atrybutu `Message` właściwości.

```cs
[TempData]
public string Message { get; set; }
```

Zobacz [TempData](xref:fundamentals/app-state#temp) Aby uzyskać więcej informacji.

<a name="mhpp"></a>
## <a name="multiple-handlers-per-page"></a>Wielu obsług na stronie

Następująca strona generuje kod znaczników dla dwie strony przy użyciu programów obsługi `asp-page-handler` pomocnika tagów:

[!code-cshtml[](index/sample/RazorPagesContacts2/Pages/Customers/CreateFATH.cshtml?highlight=12-13)]

<!-- Review: the FormActionTagHelper applies to all <form /> elements on a Razor page, even when there's no `asp-` attribute   -->

Formularz w poprzednim przykładzie ma dwa przesłać przyciski, za pomocą każdej `FormActionTagHelper` do przesłania do innego adresu URL. `asp-page-handler` Atrybut jest dodatek do `asp-page`. `asp-page-handler` generuje adresów URL, które przesłania do każdej z metod obsługi zdefiniowane przez stronę. `asp-page` nie jest określony, ponieważ próbki jest konsolidacja do bieżącej strony.

Model strony:

[!code-cs[](index/sample/RazorPagesContacts2/Pages/Customers/CreateFATH.cshtml.cs?highlight=20,32)]

W poprzednim kodzie użyto *o nazwie metod obsługi*. Metody o nazwie procedury obsługi są tworzone przez pobieranie tekstu w nazwie po `On<HTTP Verb>` i przed `Async` (jeśli istnieje). W powyższym przykładzie metody strony są OnPost**JoinList**Async i OnPost**JoinListUC**asynchronicznego. Z *OnPost* i *Async* usunięta, nazwy programu obsługi są `JoinList` i `JoinListUC`.

[!code-cshtml[](index/sample/RazorPagesContacts2/Pages/Customers/CreateFATH.cshtml?range=12-13)]

Przy użyciu poprzedniego kodu ścieżkę adresu URL, który przesyła do `OnPostJoinListAsync` jest `http://localhost:5000/Customers/CreateFATH?handler=JoinList`. Ścieżka adresu URL, który przesyła do `OnPostJoinListUCAsync` jest `http://localhost:5000/Customers/CreateFATH?handler=JoinListUC`.



## <a name="customizing-routing"></a>Dostosowywanie routingu

Jeśli nie chcesz, ciąg zapytania `?handler=JoinList` w adresie URL, można zmienić trasy, aby umieścić nazwę programu obsługi w części ścieżki adresu URL. Trasy można dostosować, dodając szablonu trasy ująć w podwójny cudzysłów po `@page` dyrektywy.

[!code-cshtml[](index/sample/RazorPagesContacts2/Pages/Customers/CreateRoute.cshtml?highlight=1)]

Trasa poprzedniego umieszcza nazwa programu obsługi ścieżki adresu URL zamiast ciągu zapytania. `?` Następujące `handler` oznacza, że parametr trasy jest opcjonalny.

Można użyć `@page` dodać dodatkowe segmenty i parametrów do strony trasy. Niezależnie od istnieje już ma **dołączany** do trasy domyślnej strony. Zmiana trasy strony przy użyciu ścieżki bezwzględnej lub wirtualnych (takie jak `"~/Some/Other/Path"`) nie jest obsługiwany.

## <a name="configuration-and-settings"></a>Konfiguracja i ustawienia

Aby skonfigurować opcje zaawansowane, użyj metody rozszerzenia `AddRazorPagesOptions` w Konstruktorze MVC:

[!code-cs[](index/sample/RazorPagesContacts/StartupAdvanced.cs?name=snippet_1)]

Obecnie można użyć `RazorPagesOptions` Ustaw katalog główny strony lub Dodaj konwencje modelu aplikacji dla stron. Firma Microsoft będzie włączyć więcej rozszerzeń w ten sposób w przyszłości.

Wstępnej kompilacji widoków, zobacz [kompilacji widoku Razor](xref:mvc/views/view-compilation) .

[Pobrania lub wyświetlenia przykładowy kod](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/razor-pages/index/sample).

Zobacz [wprowadzenie stron Razor](xref:tutorials/razor-pages/razor-pages-start), która opiera się na to wprowadzenie.

### <a name="specify-that-razor-pages-are-at-the-content-root"></a>Określ, czy stron Razor zawartości katalogu głównego

Domyślnie stron Razor są umieszczone w */strony* katalogu. Dodaj [WithRazorPagesAtContentRoot](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvcbuilderextensions.withrazorpagesatcontentroot) do [AddMvc](/dotnet/api/microsoft.extensions.dependencyinjection.mvcservicecollectionextensions.addmvc#Microsoft_Extensions_DependencyInjection_MvcServiceCollectionExtensions_AddMvc_Microsoft_Extensions_DependencyInjection_IServiceCollection_) stron Razor są zawartości katalogu głównego ([ContentRootPath](/dotnet/api/microsoft.aspnetcore.hosting.ihostingenvironment.contentrootpath)) aplikacji:

```csharp
services.AddMvc()
    .AddRazorPagesOptions(options =>
    {
        ...
    })
    .WithRazorPagesAtContentRoot();
```

### <a name="specify-that-razor-pages-are-at-a-custom-root-directory"></a>Określ, czy w katalogu głównym niestandardowych stron Razor

Dodaj [WithRazorPagesRoot](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvccorebuilderextensions.withrazorpagesroot) do [AddMvc](/dotnet/api/microsoft.extensions.dependencyinjection.mvcservicecollectionextensions.addmvc#Microsoft_Extensions_DependencyInjection_MvcServiceCollectionExtensions_AddMvc_Microsoft_Extensions_DependencyInjection_IServiceCollection_) do określenia, czy stron Razor w katalogu głównym niestandardowych w aplikacji (podaj ścieżkę względną):

```csharp
services.AddMvc()
    .AddRazorPagesOptions(options =>
    {
        ...
    })
    .WithRazorPagesRoot("/path/to/razor/pages");
```

## <a name="see-also"></a>Zobacz także

* [Wprowadzenie do platformy ASP.NET Core](xref:index)
* [Składnia Razor](xref:mvc/views/razor)
* [Wprowadzenie do korzystania ze stron Razor](xref:tutorials/razor-pages/razor-pages-start)
* [Konwencje autoryzacji stron razor](xref:security/authorization/razor-pages-authorization)
* [Razor strony trasy i strony modelu dostawców niestandardowych](xref:mvc/razor-pages/razor-pages-convention-features)
* [Testy jednostkowe i integracja z stron razor](xref:testing/razor-pages-testing)
