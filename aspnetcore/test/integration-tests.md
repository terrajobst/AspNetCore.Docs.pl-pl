---
title: Testy integracji w programie ASP.NET Core
author: guardrex
description: Dowiedz się, jak testy integracji upewnij się, że składniki aplikacji działać poprawnie na poziomie infrastruktury, w tym bazy danych, system plików i sieci.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 05/30/2018
uid: test/integration-tests
ms.openlocfilehash: a136a362cd8973b3684f9a70bd4792d75238eab0
ms.sourcegitcommit: 375e9a67f5e1f7b0faaa056b4b46294cc70f55b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/29/2018
ms.locfileid: "50207878"
---
# <a name="integration-tests-in-aspnet-core"></a>Testy integracji w programie ASP.NET Core

Przez [Luke Latham](https://github.com/guardrex) i [Steve Smith](https://ardalis.com/)

Testy integracji upewnij się, że składniki aplikacji działać poprawnie na poziomie, który obejmuje infrastrukturę obsługującą aplikacji, takich jak bazy danych, system plików i sieci. Platforma ASP.NET Core obsługuje testy integracji przy użyciu frameworka testów jednostkowych przy użyciu hosta testów sieci web a serwerem testu w pamięci.

W tym temacie założono podstawową wiedzę na temat testów jednostkowych. Jeśli znasz pojęć testów, zobacz [testów jednostkowych .NET Core i .NET Standard](/dotnet/core/testing/) temat i jego połączonej zawartości.

[Wyświetlanie lub pobieranie przykładowego kodu](https://github.com/aspnet/Docs/tree/master/aspnetcore/test/integration-tests/samples) ([sposobu pobierania](xref:index#how-to-download-a-sample))

Przykładowa aplikacja jest aplikacją stron Razor i zakłada podstawową wiedzę na temat stron Razor. Jeśli znasz stron Razor, zobacz następujące tematy:

* [Wprowadzenie do stron Razor](xref:razor-pages/index)
* [Wprowadzenie do korzystania ze stron Razor](xref:tutorials/razor-pages/razor-pages-start)
* [Testy jednostkowe stron Razor](xref:test/razor-pages-tests)

> [!NOTE]
> W przypadku testowania aplikacji jednostronicowych zalecane narzędzia, takie jak [Selenium](https://www.seleniumhq.org/), które można zautomatyzować przeglądarki.

## <a name="introduction-to-integration-tests"></a>Wprowadzenie do testów integracji

Testy integracji ocenić składniki aplikacji na wyższym poziomie niż [testów jednostkowych](/dotnet/core/testing/). Testy jednostkowe są używane do testowania składniki oprogramowania izolowane, takie jak metody poszczególne klasy. Testy integracji upewnij się, że co najmniej dwóch składników aplikacji współpracują ze sobą do produkcji oczekiwany wynik, prawdopodobnie każdy składnik wymagany na pełne przetworzenie żądania w tym.

Te testy szersze są używane do testowania aplikacji infrastruktury i całej struktury, często w tym następujących składników:

* Baza danych
* System plików
* Urządzenia sieciowe
* Potok żądań i odpowiedzi

Użyj wykonane składniki, nazywane testów jednostkowych *elementów sztucznych* lub *testowanie obiektów*, zamiast składników infrastruktury.

W przeciwieństwie do testów jednostkowych testów integracji:

* Użyj rzeczywistego składników, które aplikacja używa w środowisku produkcyjnym.
* Wymaga więcej kodu i przetwarzania danych.
* Działał dłużej.

W związku z tym można ograniczyć użycie testy integracji do najważniejszych scenariuszy infrastruktury. To zachowanie można przetestować za pomocą testu jednostkowego lub wiąże się test integracji, jeśli test jednostkowy.

> [!TIP]
> Nie zapisuj testy integracji dla każdej permutacji możliwe danych oraz pliku dostępu z bazami danych i systemy plików. Niezależnie od tego, ile miejsca w aplikacji można wchodzić w interakcje z baz danych i systemy plików, wąsko zdefiniowany zestaw odczytu, zapisu, aktualizacji i usuwania integracji testy są zazwyczaj można odpowiednio testowanie bazy danych i plików składników systemu. Użyj testów jednostki dla procedury testów logikę metody, wchodzących w interakcję z tych składników. W testach jednostkowych korzystanie z infrastruktury elementów sztucznych/mocks wynik na liście szybsze wykonywanie testów.

> [!NOTE]
> W dyskusjach dotyczących testów integracyjnych, przetestowane projekt jest często nazywany *badanego*, lub "SUT", w skrócie.

## <a name="aspnet-core-integration-tests"></a>Testy integracji platformy ASP.NET Core

Testy integracji w programie ASP.NET Core wymagają, aby:

* Projekt testowy służy do przechowywania i wykonywania testów. Projekt testowy zawiera odwołanie do przetestowany projekt platformy ASP.NET Core, o nazwie *badanego* (SUT). _"SUT" jest używany w tym temacie do odwoływania się do niej przetestowane._
* Projekt testowy tworzy testu sieci web hosta SUT i używa klienta serwera testowego do obsługi żądań i odpowiedzi SUT.
* Narzędzie test runner jest używany do wykonywania testów i raportu wyników testu.

Testy integracji wykonaj sekwencję zdarzeń, które zawierają zwykle *Rozmieść*, *Act*, i *Asercja* kroki testu:

1. SUT hosta sieci web jest skonfigurowana.
1. Klient serwera test jest tworzony do przesyłania żądań do aplikacji.
1. *Rozmieść* krok testu jest wykonywany: Aplikacja testowa przygotowuje żądania.
1. *Act* krok testu jest wykonywany: klient przesyła żądanie i odbiera odpowiedź.
1. *Asercja* krok testu jest wykonywany: *rzeczywiste* odpowiedzi jest zweryfikowany jako *przekazać* lub *się nie powieść* na podstawie *oczekiwano*  odpowiedzi.
1. Proces jest kontynuowany, dopóki wszystkie testy są wykonywane.
1. Wyniki testu są zgłaszane.

Zazwyczaj test hosta sieci web skonfigurowano inaczej niż uruchamia hosta normalne sieci web aplikacji dla testu. Na przykład inną bazę danych lub innej aplikacji ustawienia mogą być używane dla badań.

Składniki infrastruktury, takich jak test hosta sieci web i testu w pamięci serwera ([elementu TestServer](/dotnet/api/microsoft.aspnetcore.testhost.testserver)), są udostępniane lub zarządzana przez [Microsoft.AspNetCore.Mvc.Testing](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Testing) pakietu. Korzystanie z tego pakietu usprawnia tworzenie testów i wykonywania.

`Microsoft.AspNetCore.Mvc.Testing` Pakietu obsługuje następujące zadania:

* Kopiuje plik zależności (*\*.deps*) z SUT do projektu testowego *bin* folderu.
* Ustawia zawartości głównego katalogu głównego projektu SUT tak, aby pliki statyczne i stron/widoki są dostępne, gdy testy są wykonywane.
* Udostępnia [WebApplicationFactory](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactory-1) klasy, aby usprawnić uruchamianie SUT z `TestServer`.

[Testów jednostkowych](/dotnet/articles/core/testing/unit-testing-with-dotnet-test) dokumentacji opisano sposób konfigurowania projektu i testowania modułu uruchamiającego testy, oraz szczegółowe instrukcje dotyczące sposobu uruchamiania testów oraz zalecenia dotyczące nazwy testów i klas testów.

> [!NOTE]
> Podczas tworzenia projektu testowego dla aplikacji, należy oddzielić testy jednostkowe z testów integracji do różnych projektów. Pozwala to zagwarantować, że testowanie składników infrastruktury nie są przypadkowo uwzględnione w testach jednostkowych. Umożliwia także rozdzielenie testów jednostkowych i integracji kontroli, przez który zestaw testów są uruchamiane.

Praktycznie nie ma różnic między aplikacjami MVC i konfiguracji testów aplikacji stron Razor. Jedyną różnicą jest to w nazewnictwa testy. W aplikacji stron Razor testy punktów końcowych strony mają przeważnie nazwę po stronie klasy modelu (na przykład `IndexPageTests` się test integracji składnika strony indeksu). W aplikacji MVC testy są zwykle organizowane według klasy kontrolera i nazwana kontrolerów sprawdzają one, (na przykład `HomeControllerTests` się test integracji składnika dla kontrolera głównego).

## <a name="test-app-prerequisites"></a>Testowanie aplikacji wymagań wstępnych

Projekt testowy musi:

* Odwołać się do następujących pakietów:
  - [Microsoft.AspNetCore.App](https://www.nuget.org/packages/Microsoft.AspNetCore.App/)
  - [Microsoft.AspNetCore.Mvc.Testing](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Testing/)
* Określ zestaw SDK sieci Web w pliku projektu (`<Project Sdk="Microsoft.NET.Sdk.Web">`). Zestaw SDK sieci Web jest wymagany podczas odwoływania się do [meta Microsoft.aspnetcore.all Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app).

Te wymagania wstępne są widoczne w [przykładową aplikację](https://github.com/aspnet/Docs/tree/master/aspnetcore/test/integration-tests/samples/). Sprawdzanie *tests/RazorPagesProject.Tests/RazorPagesProject.Tests.csproj* pliku. Ta aplikacja używa przykładowych [xUnit](https://xunit.github.io/) struktury testowej i [AngleSharp](https://anglesharp.github.io/) Biblioteka analizator, więc Przykładowa aplikacja również odwołuje się do:

* [xunit](https://www.nuget.org/packages/xunit/)
* [xunit.Runner.VisualStudio](https://www.nuget.org/packages/xunit.runner.visualstudio/)
* [AngleSharp](https://www.nuget.org/packages/AngleSharp/)

## <a name="basic-tests-with-the-default-webapplicationfactory"></a>Podstawowe testy przy użyciu domyślnego WebApplicationFactory

[WebApplicationFactory&lt;TEntryPoint&gt; ](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactory-1) służy do tworzenia [elementu TestServer](/dotnet/api/microsoft.aspnetcore.testhost.testserver) testów integracji. `TEntryPoint` Zazwyczaj jest klasa punktu wejścia SUT `Startup` klasy.

Implementowanie klas testowych *początkowych klasy* interfejsu (`IClassFixture`) oznacza klasy zawiera testy i zapewnienia wystąpienia obiektów udostępnionych testów w klasie.

### <a name="basic-test-of-app-endpoints"></a>Podstawowy test punktów końcowych aplikacji

Następującego testu, klasy, `BasicTests`, używa `WebApplicationFactory` bootstrap SUT oraz zapewniając [HttpClient](/dotnet/api/system.net.http.httpclient) z metodą testową `Get_EndpointsReturnSuccessAndCorrectContentType`. Metoda sprawdza, czy kod stanu odpowiedzi to pomyślne (kody stanu w zakresie 200 299) i `Content-Type` nagłówek jest `text/html; charset=utf-8` kilka stron aplikacji.

[CreateClient](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactory-1.createclient) tworzy wystąpienie `HttpClient` , następuje przekierowania i automatycznie obsługuje pliki cookie.

[!code-csharp[](integration-tests/samples/2.x/IntegrationTestsSample/tests/RazorPagesProject.Tests/IntegrationTests/BasicTests.cs?name=snippet1)]

### <a name="test-a-secure-endpoint"></a>Testowanie bezpiecznego punktu końcowego

Kolejny test w `BasicTests` klasa sprawdza, czy bezpieczny punkt końcowy przekierowuje nieuwierzytelniony użytkownik zgłasza do strony logowania w aplikacji.

W SUT `/SecurePage` stronie używa [AuthorizePage](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizepage) Konwencji, aby zastosować [AuthorizeFilter](/dotnet/api/microsoft.aspnetcore.mvc.authorization.authorizefilter) ze stroną. Aby uzyskać więcej informacji, zobacz [konwencje autoryzacja stron Razor](xref:security/authorization/razor-pages-authorization#require-authorization-to-access-a-page).

[!code-csharp[](integration-tests/samples/2.x/IntegrationTestsSample/src/RazorPagesProject/Startup.cs?name=snippet1)]

W `Get_SecurePageRequiresAnAuthenticatedUser` przetestować, [WebApplicationFactoryClientOptions](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactoryclientoptions) jest ustawiona na nie zezwalaj na przekierowań, ustawiając [AllowAutoRedirect](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactoryclientoptions.allowautoredirect) do `false`:

[!code-csharp[](integration-tests/samples/2.x/IntegrationTestsSample/tests/RazorPagesProject.Tests/IntegrationTests/BasicTests.cs?name=snippet2)]

Nie można przydzielać wykonaj przekierowania klienta, może wprowadzone następujące testy:

* Można sprawdzić kod stanu zwrócony przez SUT względem oczekiwanej [HttpStatusCode.Redirect](/dotnet/api/system.net.httpstatuscode) wynik, nie kod stanu końcowego po przekierowanie do strony logowania, które byłyby [HttpStatusCode.OK](/dotnet/api/system.net.httpstatuscode).
* `Location` Wartość nagłówka w nagłówkach odpowiedzi jest sprawdzany w celu potwierdzenia, że rozpoczyna się od `http://localhost/Identity/Account/Login`, nie końcowej strony odpowiedź na logowanie, gdzie `Location` nagłówka nie ma być obecne.

Aby uzyskać więcej informacji na temat `WebApplicationFactoryClientOptions`, zobacz [opcje klienta](#client-options) sekcji.

## <a name="customize-webapplicationfactory"></a>Dostosowywanie WebApplicationFactory

Konfiguracja hosta sieci Web mogą być tworzone niezależnie od klas testowych przez dziedziczenie z `WebApplicationFactory` utworzenie co najmniej jeden fabryk niestandardowe:

1. Dziedzicz `WebApplicationFactory` i zastąpić [ConfigureWebHost](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactory-1.configurewebhost). [IWebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder) umożliwia konfigurowanie zbierania usługi za pomocą [ConfigureServices](/dotnet/api/microsoft.aspnetcore.hosting.istartup.configureservices):

   [!code-csharp[](integration-tests/samples/2.x/IntegrationTestsSample/tests/RazorPagesProject.Tests/CustomWebApplicationFactory.cs?name=snippet1)]

   Rozmieszczanie w bazie danych [przykładową aplikację](https://github.com/aspnet/Docs/tree/master/aspnetcore/test/integration-tests/samples) odbywa się przez `InitializeDbForTests` metody. Metoda została opisana w [integracji testy próbki: testowanie aplikacji organizacji](#test-app-organization) sekcji.

2. Użyj niestandardowego `CustomWebApplicationFactory` w klasach testowych. W poniższym przykładzie użyto fabryka w `IndexPageTests` klasy:

   [!code-csharp[](integration-tests/samples/2.x/IntegrationTestsSample/tests/RazorPagesProject.Tests/IntegrationTests/IndexPageTests.cs?name=snippet1)]

   Przykładowa aplikacja klient jest skonfigurowany do zapobiegania `HttpClient` z następujących przekierowania. Jak wyjaśniono w [Test bezpiecznego punktu końcowego](#test-a-secure-endpoint) sekcji pozwala testów, aby sprawdzić wynik pierwszej odpowiedzi z aplikacji. Pierwszą odpowiedzią jest przekierowanie w wielu z tych testów z `Location` nagłówka.

3. Korzysta z typowym teście `HttpClient` i metody pomocnicze do przetwarzania żądania i odpowiedzi:

   [!code-csharp[](integration-tests/samples/2.x/IntegrationTestsSample/tests/RazorPagesProject.Tests/IntegrationTests/IndexPageTests.cs?name=snippet2)]

Wszelkie żądania POST do SUT musi spełniać antiforgery upewnij się, że staje się automatycznie za pomocą aplikacji [system antiforgery ochrony danych](xref:security/data-protection/introduction). Aby rozmieścić dla żądania POST testu, aplikacja testowa musi:

1. Wyślij żądanie dla strony.
1. Przeanalizować antiforgery plik cookie i żądania tokenu weryfikacji z odpowiedzi.
1. Należy żądania POST z antiforgery sprawdzania poprawności plików cookie i żądania tokenu w miejscu.

`SendAsync` Metody rozszerzenia pomocnika (*Helpers/HttpClientExtensions.cs*) i `GetDocumentAsync` metodę pomocniczą (*Helpers/HtmlHelpers.cs*) w [przykładową aplikację](https://github.com/aspnet/Docs/tree/master/aspnetcore/test/integration-tests/samples/) użyj [AngleSharp](https://anglesharp.github.io/) Analizator do obsługi antiforgery wyboru przy użyciu następujących metod:

* `GetDocumentAsync` &ndash; Odbiera [obiektu HttpResponseMessage](/dotnet/api/system.net.http.httpresponsemessage) i zwraca `IHtmlDocument`. `GetDocumentAsync` używa fabryki, który przygotowuje *odpowiedzi wirtualnego* oparte na oryginalnym `HttpResponseMessage`. Aby uzyskać więcej informacji, zobacz [dokumentacji AngleSharp](https://github.com/AngleSharp/AngleSharp#documentation).
* `SendAsync` metody rozszerzenia dla `HttpClient` compose [HttpRequestMessage](/dotnet/api/system.net.http.httprequestmessage) i wywołać [SendAsync(HttpRequestMessage)](/dotnet/api/system.net.http.httpclient.sendasync#System_Net_Http_HttpClient_SendAsync_System_Net_Http_HttpRequestMessage_) do przesyłania żądań do SUT. Przeciążenia `SendAsync` zaakceptować formularza HTML (`IHtmlFormElement`) oraz następujące:
  - Przedstawia przycisk formularza (`IHtmlElement`)
  - Formularz wartości kolekcji (`IEnumerable<KeyValuePair<string, string>>`)
  - Przycisk Prześlij (`IHtmlElement`) i wartości pól (`IEnumerable<KeyValuePair<string, string>>`)

> [!NOTE]
> [AngleSharp](https://anglesharp.github.io/) jest analiza kodu innych firm używane w celach demonstracyjnych, w tym temacie i przykładowa aplikacja biblioteki. AngleSharp nie jest obsługiwana lub wymaganych do testowania integracji aplikacji platformy ASP.NET Core. Inne analizatory mogą być używane, takich jak [pakiet elastyczność Html (HAP)](http://html-agility-pack.net/). Innym rozwiązaniem jest napisać kod, aby obsłużyć żądania tokenu weryfikacji i plików cookie antiforgery antiforgery systemu bezpośrednio.

## <a name="customize-the-client-with-withwebhostbuilder"></a>Dostosowywanie klienta z WithWebHostBuilder

Gdy dodatkowa konfiguracja jest wymagana w metodzie badania, [WithWebHostBuilder](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactory-1.withwebhostbuilder) tworzy nową `WebApplicationFactory` z [IWebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder) który jest następnie dostosować przez konfigurację.

`Post_DeleteMessageHandler_ReturnsRedirectToRoot` Przetestować metodę [przykładową aplikację](https://github.com/aspnet/Docs/tree/master/aspnetcore/test/integration-tests/samples) demonstruje użycie `WithWebHostBuilder`. Ten test polega na usunięcie rekordu w bazie danych, wyzwalając przesyłania formularza, w SUT.

Ponieważ innego testu w `IndexPageTests` klasy wykonuje operację, która usuwa wszystkie rekordy w bazie danych i mogą być uruchamiane przed `Post_DeleteMessageHandler_ReturnsRedirectToRoot` metody, bazy danych jest obsługiwany w tej metodzie testowej, aby upewnić się, że rekord jest obecny dla SUT można usunąć. Wybieranie `deleteBtn1` przycisku `messages` formularza w SUT jest symulowane w żądaniu SUT:

[!code-csharp[](integration-tests/samples/2.x/IntegrationTestsSample/tests/RazorPagesProject.Tests/IntegrationTests/IndexPageTests.cs?name=snippet3)]

## <a name="client-options"></a>Opcje klienta

W poniższej tabeli przedstawiono domyślne [WebApplicationFactoryClientOptions](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactoryclientoptions) dostępne podczas tworzenia `HttpClient` wystąpień.

| Opcja | Opis | Domyślny |
| ------ | ----------- | ------- |
| [AllowAutoRedirect](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactoryclientoptions.allowautoredirect) | Pobiera lub ustawia informację, czy `HttpClient` wystąpień należy automatycznie stosować odpowiedzi przekierowania. | `true` |
| [BaseAddress](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactoryclientoptions.baseaddress) | Pobiera lub ustawia adres bazowy `HttpClient` wystąpień. | `http://localhost` |
| [HandleCookies](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactoryclientoptions.handlecookies) | Pobiera lub ustawia czy `HttpClient` wystąpień powinna obsługiwać pliki cookie. | `true` |
| [MaxAutomaticRedirections](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactoryclientoptions.maxautomaticredirections) | Pobiera lub ustawia maksymalną liczbę odpowiedzi przekierowania, który `HttpClient` wystąpień powinien być zgodny. | 7 |

Tworzenie `WebApplicationFactoryClientOptions` klasy i przekazać ją do [CreateClient](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactory-1.createclient) — metoda (ustawienie domyślne wartości są wyświetlane w przykładzie kodu):

```csharp
// Default client option values are shown
var clientOptions = new WebApplicationFactoryClientOptions();
clientOptions.AllowAutoRedirect = true;
clientOptions.BaseAddress = new Uri("http://localhost");
clientOptions.HandleCookies = true;
clientOptions.MaxAutomaticRedirections = 7;

_client = _factory.CreateClient(clientOptions);
```

## <a name="inject-mock-services"></a>Wstrzykiwanie makiety usług

Usługi mogą zostać zastąpione w teście z wywołaniem do [ConfigureTestServices](/dotnet/api/microsoft.aspnetcore.testhost.webhostbuilderextensions.configuretestservices) w Konstruktorze hosta. **Aby wstawić makiety usługi, musi mieć SUT `Startup` klasy `Startup.ConfigureServices` metody.**

Przykład SUT obejmuje usługi o określonym zakresie, która zwraca oferty. Oferta jest osadzony w ukryte pole na stronę indeksu, gdy wymagane jest stroną indeksu.

*Services/IQuoteService.cs*:

[!code-csharp[](integration-tests/samples/2.x/IntegrationTestsSample/src/RazorPagesProject/Services/IQuoteService.cs?name=snippet1)]

*Services/QuoteService.cs*:

[!code-csharp[](integration-tests/samples/2.x/IntegrationTestsSample/src/RazorPagesProject/Services/QuoteService.cs?name=snippet1)]

*Startup.cs*:

[!code-csharp[](integration-tests/samples/2.x/IntegrationTestsSample/src/RazorPagesProject/Startup.cs?name=snippet2)]

*Pages/Index.cshtml.cs*:

[!code-csharp[](integration-tests/samples/2.x/IntegrationTestsSample/src/RazorPagesProject/Pages/Index.cshtml.cs?name=snippet1&highlight=4,9,20,26)]

*Pages/Index.cs*:

[!code-cshtml[](integration-tests/samples/2.x/IntegrationTestsSample/src/RazorPagesProject/Pages/Index.cshtml?name=snippet_Quote)]

Następujący kod jest generowany po uruchomieniu aplikacji SUT:

```html
<input id="quote" type="hidden" value="Come on, Sarah. We&#x27;ve an appointment in 
    London, and we&#x27;re already 30,000 years late.">
```

Testowanie iniekcji usługi i oferty w wiąże się test integracji, makiety usługi są wstrzykiwane do SUT przez test. Usługa makiety zastępuje aplikacji `QuoteService` usługą zapewnianych przez aplikację testu o nazwie `TestQuoteService`:

*IntegrationTests.IndexPageTests.cs*:

[!code-csharp[](integration-tests/samples/2.x/IntegrationTestsSample/tests/RazorPagesProject.Tests/IntegrationTests/IndexPageTests.cs?name=snippet4)]

`ConfigureTestServices` jest wywoływana, i usługi o określonym zakresie nie jest zarejestrowany:

[!code-csharp[](integration-tests/samples/2.x/IntegrationTestsSample/tests/RazorPagesProject.Tests/IntegrationTests/IndexPageTests.cs?name=snippet5&highlight=7-10,17,20-21)]

Znaczniki generowane podczas wykonywania testów odzwierciedla tekst oferty, dostarczone przez `TestQuoteService`, w związku z tym przebiegów potwierdzenia:

```html
<input id="quote" type="hidden" value="Something&#x27;s interfering with time, 
    Mr. Scarman, and time is my business.">
```

## <a name="how-the-test-infrastructure-infers-the-app-content-root-path"></a>Jak infrastrukturę testowania wnioskuje ścieżka zawartości katalogu głównego aplikacji

`WebApplicationFactory` Konstruktor wnioskuje ścieżka zawartości katalogu głównego aplikacji, wyszukując [WebApplicationFactoryContentRootAttribute](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactorycontentrootattribute) na zestaw zawierający testy integracji przy użyciu klucza równą `TEntryPoint` zestawu `System.Reflection.Assembly.FullName`. W przypadku, gdy atrybut z poprawnym kluczem nie zostanie znaleziona, `WebApplicationFactory` powraca do wyszukiwania pliku rozwiązań (*\*.sln*) i dołącza `TEntryPoint` nazwy zestawu w katalogu rozwiązania. Katalog główny aplikacji (ścieżka katalogu głównego zawartości) służy do odnajdywania, widoki i plików zawartości.

W większości przypadków nie trzeba jawnie ustawić katalogu głównego zawartości aplikacji, ponieważ logika wyszukiwania zwykle znajduje poprawne zawartości katalogu głównego w czasie wykonywania. W scenariuszach specjalne, której nie odnaleziono katalogu głównego zawartości przy użyciu algorytmu wyszukiwania wbudowanych, zawartości, można określić katalogu głównego jawnie lub za pomocą niestandardowej logiki aplikacji. Aby ustawić katalogu głównego zawartości aplikacji w tych scenariuszach, należy wywołać `UseSolutionRelativeContentRoot` metoda rozszerzenia z [Microsoft.AspNetCore.TestHost](https://www.nuget.org/packages/Microsoft.AspNetCore.TestHost) pakietu. Podaj ścieżkę względną rozwiązania i wzorzec nazwy lub glob pliku rozwiązania opcjonalne (domyślny = `*.sln`).

Wywołaj [UseSolutionRelativeContentRoot](/dotnet/api/microsoft.aspnetcore.testhost.webhostbuilderextensions.usesolutionrelativecontentroot) przy użyciu metody rozszerzenia *jeden* z następujących metod:

* Podczas konfigurowania klas testowych za pomocą `WebApplicationFactory`, podaj niestandardowej konfiguracji za pomocą [IWebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder):

   ```csharp
   public IndexPageTests(
       WebApplicationFactory<RazorPagesProject.Startup> factory)
   {
       var _factory = factory.WithWebHostBuilder(builder =>
       {
           builder.UseSolutionRelativeContentRoot("<SOLUTION-RELATIVE-PATH>");

           ...
       });
   }
   ```

* Podczas konfigurowania klas testowych za pomocą niestandardowego `WebApplicationFactory`, dziedziczą z `WebApplicationFactory` i zastąpić [ConfigureWebHost](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactory-1.configurewebhost):

   ```csharp
   public class CustomWebApplicationFactory<TStartup>
       : WebApplicationFactory<RazorPagesProject.Startup>
   {
       protected override void ConfigureWebHost(IWebHostBuilder builder)
       {
           builder.ConfigureServices(services =>
           {
               builder.UseSolutionRelativeContentRoot("<SOLUTION-RELATIVE-PATH>");

               ...
           });
       }
   }
   ```

## <a name="disable-shadow-copying"></a>Wyłącz kopiowania w tle

Kopiowanie w tle powoduje, że testy do wykonania w innym folderze niż folder wyjściowy. W przypadku testów do poprawnego działania kopiowania w tle, należy wyłączyć. [Przykładową aplikację](https://github.com/aspnet/Docs/tree/master/aspnetcore/test/integration-tests/samples) używa rozwiązania xUnit i wyłącza kopiowania w tle dla xUnit, umieszczając *xunit.runner.json* pliku z ustawieniem prawidłowej konfiguracji. Aby uzyskać więcej informacji, zobacz [Konfigurowanie xUnit.net kodem JSON](https://xunit.github.io/docs/configuring-with-json.html).

Dodaj *xunit.runner.json* plik do katalogu głównego projektu testowego o następującej zawartości:

```json
{
  "shadowCopy": false
}
```

## <a name="integration-tests-sample"></a>Przykład testy integracji

[Przykładową aplikację](https://github.com/aspnet/Docs/tree/master/aspnetcore/test/integration-tests/samples) składa się z dwóch aplikacji:

| Aplikacja | Folder projektu | Opis |
| --- | -------------- | ----------- |
| Wiadomości w aplikacji (SUT) | *src/RazorPagesProject* | Umożliwia użytkownikowi Dodaj, Usuń jedno, Usuń wszystkie i analizowania komunikatów. |
| Testowanie aplikacji | *tests/RazorPagesProject.Tests* | Używany do testów integracji SUT. |

Testy mogą być uruchamiane przy użyciu funkcji wbudowanych testu środowisko IDE, takich jak [programu Visual Studio](https://www.visualstudio.com/vs/). Jeśli przy użyciu [programu Visual Studio Code](https://code.visualstudio.com/) lub wiersza polecenia, uruchom następujące polecenie w wierszu polecenia w *tests/RazorPagesProject.Tests* folderu:

```console
dotnet test
```

### <a name="message-app-sut-organization"></a>Komunikat aplikacji (SUT) organizacji

SUT to system stron Razor wiadomości o następującej charakterystyce:

* Strony indeksu aplikacji (*Pages/Index.cshtml* i *Pages/Index.cshtml.cs*) udostępnia interfejs użytkownika i strony metody modelu w celu kontrolowania dodawania, usuwania i analizy wiadomości (średni słowa na komunikat) .
* Komunikat jest opisana przez `Message` klasy (*Data/Message.cs*) za pomocą dwie właściwości: `Id` (klucz) i `Text` (komunikat). `Text` Właściwość jest wymagana i maksymalnie 200 znaków.
* Komunikaty są przechowywane przy użyciu [bazy danych w pamięci programu Entity Framework](/ef/core/providers/in-memory/)&#8224;.
* Ta aplikacja zawiera warstwy dostępu do danych (DAL) w swojej klasie kontekst bazy danych `AppDbContext` (*Data/AppDbContext.cs*).
* Jeśli baza danych jest pusta podczas uruchamiania aplikacji, Magazyn komunikatu jest inicjowany z trzech wiadomości.
* Aplikacja zawiera `/SecurePage` , może zostać oceniony jedynie przez uwierzytelnionego użytkownika.

&#8224;Temat EF [testu za pomocą InMemory](/ef/core/miscellaneous/testing/in-memory), wyjaśnia, jak użyć bazy danych w pamięci dla testów w narzędziu MSTest. W tym temacie używany [xUnit](https://xunit.github.io/) struktury testowej. Test pojęć i badanie implementacji różnych środowisk testowych różnych są podobne, ale nie są identyczne.

Mimo że aplikacja nie korzysta z wzorca repozytorium i nie jest skuteczne przykładem [wzorzec jednostki pracy (UoW)](https://martinfowler.com/eaaCatalog/unitOfWork.html), stron Razor obsługuje te wzorce programowania. Aby uzyskać więcej informacji, zobacz [projektowanie warstwy trwałości infrastruktury](/dotnet/standard/microservices-architecture/microservice-ddd-cqrs-patterns/infrastructure-persistence-layer-design) i [logikę kontrolera testu](/aspnet/core/mvc/controllers/testing) (przykład implementuje wzorzec repozytorium).

### <a name="test-app-organization"></a>Testowanie aplikacji organizacji

Aplikacja testowa jest aplikacją konsoli wewnątrz *tests/RazorPagesProject.Tests* folderu.

| Folder aplikacji testowego | Opis |
| --------------- | ----------- |
| *BasicTests* | *BasicTests.cs* zawiera metody testowe dla routingu, uzyskiwanie dostępu do bezpiecznego strony nieuwierzytelniony użytkownik i Uzyskiwanie profilu użytkownika usługi GitHub i sprawdzanie danych logowania użytkownika w profilu. |
| *IntegrationTests* | *IndexPageTests.cs* zawiera testy integracji strony indeksu przy użyciu niestandardowego `WebApplicationFactory` klasy. |
| *Pomocnicy/narzędzia* | <ul><li>*Utilities.cs* zawiera `InitializeDbForTests` metodę używaną do umieszczenia w bazie danych testowych.</li><li>*HtmlHelpers.cs* udostępnia metodę, aby zwrócić AngleSharp `IHtmlDocument` do użytku przez metod testowych.</li><li>*HttpClientExtensions.cs* , zapewnienia przeciążenia dla `SendAsync` do przesyłania żądań do SUT.</li></ul> |

Framework testów jest [xUnit](https://xunit.github.io/). Integracja z testów przy użyciu [Microsoft.AspNetCore.TestHost](/dotnet/api/microsoft.aspnetcore.testhost), która obejmuje [elementu TestServer](/dotnet/api/microsoft.aspnetcore.testhost.testserver). Ponieważ [Microsoft.AspNetCore.Mvc.Testing](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Testing) pakietu jest używany do konfigurowania testowego serwera hosta i testowania `TestHost` i `TestServer` pakietów nie wymagają odwołania do bezpośredniego pakietu w pliku projektu aplikacji testów lub Konfiguracja dla deweloperów w aplikacji testowych.

**Wstępne wypełnianie bazy danych do testowania**

Testy integracji zwykle wymagają małego zestawu danych w bazie danych, przed wykonaniem testów. Na przykład usuń testowe zaproszeń do usuwania rekordów w bazie danych, więc bazy danych musi zawierać co najmniej jeden rekord dla żądania usunięcia zakończyło się sukcesem.

Przykładowa aplikacja inicjowania inicjuje bazę danych z trzech wiadomości w *Utilities.cs* czy testów można użyć, gdy są wykonywane:

[!code-csharp[](integration-tests/samples/2.x/IntegrationTestsSample/tests/RazorPagesProject.Tests/Helpers/Utilities.cs?name=snippet1)]

## <a name="additional-resources"></a>Dodatkowe zasoby

* [Testy jednostkowe](/dotnet/articles/core/testing/unit-testing-with-dotnet-test)
* [Testy jednostkowe stron Razor](xref:test/razor-pages-tests)
* [Oprogramowanie pośredniczące](xref:fundamentals/middleware/index)
* [Kontrolery testów](xref:mvc/controllers/testing)
