---
title: Testy integracji w ASP.NET Core
author: guardrex
description: Dowiedz się, jak testy integracji zapewniają, że składniki aplikacji działają prawidłowo na poziomie infrastruktury, w tym bazy danych, systemu plików i sieci.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 08/23/2019
uid: test/integration-tests
ms.openlocfilehash: 195acd3e03f3de63ebd61767f2c86d1c0f38fca5
ms.sourcegitcommit: 983b31449fe398e6e922eb13e9eb6f4287ec91e8
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/24/2019
ms.locfileid: "70017434"
---
# <a name="integration-tests-in-aspnet-core"></a>Testy integracji w ASP.NET Core

Autorzy [Luke Latham](https://github.com/guardrex) i [Steve Smith](https://ardalis.com/)

Testy integracji zapewniają, że składniki aplikacji działają prawidłowo na poziomie, który obejmuje infrastrukturę pomocniczą aplikacji, taką jak baza danych, system plików i sieć. ASP.NET Core obsługuje testy integracji przy użyciu struktury testów jednostkowych z testowym hostem sieci Web i serwerem testowym w pamięci.

W tym temacie założono podstawową wiedzę na temat testów jednostkowych. Jeśli nie znasz pojęć testowych, zobacz [testy jednostkowe w programie .NET Core i w .NET Standard](/dotnet/core/testing/) tematu oraz jego połączonej zawartości.

[Wyświetlanie lub pobieranie przykładowego kodu](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/test/integration-tests/samples) ([sposobu pobierania](xref:index#how-to-download-a-sample))

Przykładowa aplikacja jest aplikacją Razor Pages i przyjmuje podstawową wiedzę na temat Razor Pages. Jeśli nie znasz Razor Pages, zobacz następujące tematy:

* [Wprowadzenie do produktu Razor Pages](xref:razor-pages/index)
* [Wprowadzenie do korzystania ze stron Razor](xref:tutorials/razor-pages/razor-pages-start)
* [Testy jednostkowe stron Razor](xref:test/razor-pages-tests)

> [!NOTE]
> W przypadku testowania aplikacji jednostronicowych zaleca się używanie narzędzia, takiego jak [selen](https://www.seleniumhq.org/), które umożliwia automatyzację przeglądarki.

## <a name="introduction-to-integration-tests"></a>Wprowadzenie do testów integracji

Testy integracji ocenią składniki aplikacji na szerszym poziomie niż [testy jednostkowe](/dotnet/core/testing/). Testy jednostkowe służą do testowania izolowanych składników oprogramowania, takich jak poszczególne metody klasy. Testy integracji potwierdzają, że co najmniej dwa składniki aplikacji współpracują ze sobą w celu uzyskania oczekiwanego wyniku, co może uwzględniać każdy składnik wymagany do pełnego przetworzenia żądania.

Te szersze testy są używane do testowania infrastruktury aplikacji i całego środowiska, często łącznie z następującymi składnikami:

* Baza danych
* System plików
* Urządzenia sieciowe
* Potok żądania-odpowiedź

Testy jednostkowe wykorzystują składniki, znane jako elementy sztucznelub makiety, zamiast składników infrastruktury.

W przeciwieństwie do testów jednostkowych, testy integracji:

* Użyj rzeczywistych składników używanych przez aplikację w środowisku produkcyjnym.
* Wymagaj większej ilości kodu i przetwarzania danych.
* Trwa dłużej.

W związku z tym Ogranicz korzystanie z testów integracji do najważniejszych scenariuszy infrastruktury. Jeśli można przetestować zachowanie przy użyciu testu jednostkowego lub testu integracji, wybierz test jednostkowy.

> [!TIP]
> Nie zapisuj testów integracji dla każdej możliwej permutacji danych i dostępu do plików z bazami danych i systemami plików. Bez względu na to, ile miejsc w aplikacji współdziała z bazami danych i systemami plików, skoncentrowany zestaw testów do odczytu, zapisu, aktualizacji i usuwania umożliwia zwykle testowanie składników bazy danych i systemu plików. Użyj testów jednostkowych do rutynowych testów logiki metod, które współpracują z tymi składnikami. W testach jednostkowych użycie sztucznych/imitacji infrastruktury powoduje szybsze wykonywanie testów.

> [!NOTE]
> W dyskusjach dotyczących testów integracji testowany projekt jest często określany jako testowany *system*lub "SUT".

## <a name="aspnet-core-integration-tests"></a>ASP.NET Core testy integracji

Testy integracji w ASP.NET Core wymagają następujących czynności:

* Projekt testowy służy do znajdowania i wykonywania testów. Projekt testowy ma odwołanie do testowanego projektu ASP.NET Core, zwanego *systemem testowym* (SUT). _"SUT" jest używany w tym temacie w celu odwoływania się do testowanej aplikacji._
* Projekt testowy tworzy testowy host sieci Web dla SUT i używa klienta serwera testowego do obsługi żądań i odpowiedzi do SUT.
* Moduł uruchamiający testy służy do wykonywania testów i raportujących wyniki testów.

Testy integracji są zgodne z sekwencją zdarzeń, które obejmujątypowe kroki testu rozmieszczenia, *działania*i potwierdzeń:

1. SUT hosta sieci Web.
1. Klient serwera testowego jest tworzony w celu przesyłania żądań do aplikacji.
1. Krok *Rozmieść* test jest wykonywany: Aplikacja testowa przygotowuje żądanie.
1. Krok testu *Act* jest wykonywany: Klient przesyła żądanie i otrzymuje odpowiedź.
1. Krok testu *potwierdzenia* jest wykonywany: Rzeczywista odpowiedź jest sprawdzana jako *przebieg* lub *Niepowodzenie* w zależności od *oczekiwanej* odpowiedzi.
1. Proces jest kontynuowany, dopóki wszystkie testy nie zostaną wykonane.
1. Wyniki testu są zgłaszane.

Test hosta sieci Web jest zwykle skonfigurowany inaczej niż normalny host sieci Web aplikacji dla przebiegów testowych. Na przykład dla testów można użyć innej bazy danych lub różnych ustawień aplikacji.

Składniki infrastruktury, takie jak test hosta sieci Web i serwer testu w pamięci ([TestServer](/dotnet/api/microsoft.aspnetcore.testhost.testserver)), są udostępniane lub zarządzane przez pakiet [Microsoft. AspNetCore. MVC. test](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Testing) . Użycie tego pakietu usprawnia tworzenie i wykonywanie testów.

`Microsoft.AspNetCore.Mvc.Testing` Pakiet obsługuje następujące zadania:

* Kopiuje plik zależności ( *\*. deps*) z SUT do katalogu *bin* projektu testowego.
* Ustawia katalog główny zawartości dla katalogu głównego projektu SUT, aby umożliwić znalezienie plików statycznych i stron/widoków podczas wykonywania testów.
* Udostępnia klasę [WebApplicationFactory](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactory-1) , aby usprawnić uruchamianie SUT przy użyciu `TestServer`.

Dokumentacja [testów jednostkowych](/dotnet/articles/core/testing/unit-testing-with-dotnet-test) opisuje sposób konfigurowania projektu testowego i modułu testowego, a także szczegółowe instrukcje dotyczące sposobu uruchamiania testów i zaleceń dotyczących sposobu określania nazw testów i klas testowych.

> [!NOTE]
> Podczas tworzenia projektu testowego dla aplikacji należy oddzielić testy jednostkowe od testów integracji do różnych projektów. Dzięki temu składniki do testowania infrastruktury nie są przypadkowo uwzględniane w testach jednostkowych. Rozdzielenie testów jednostkowych i integracyjnych umożliwia również kontrolę nad tym, który zestaw testów jest uruchamiany.

Nie istnieje praktycznie żadna różnica między konfiguracją testów aplikacji Razor Pages i aplikacji MVC. Jedyną różnicą jest to, jak te testy są nazywane. W aplikacji Razor Pages testy punktów końcowych stron są zwykle nazywane po klasie modelu strony (na przykład `IndexPageTests` w celu przetestowania integracji składników na stronie indeksu). W aplikacji MVC testy są zwykle zorganizowane według klas kontrolera i nazwane po testowaniu kontrolerów (na przykład `HomeControllerTests` w celu przetestowania integracji składników dla kontrolera macierzystego).

## <a name="test-app-prerequisites"></a>Wymagania wstępne aplikacji testowej

Projekt testowy musi:

* Odwołuje się do następujących pakietów:
  * [Microsoft.AspNetCore.App](https://www.nuget.org/packages/Microsoft.AspNetCore.App/)
  * [Microsoft.AspNetCore.Mvc.Testing](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Testing/)
* Określ zestaw SDK sieci Web w pliku projektu (`<Project Sdk="Microsoft.NET.Sdk.Web">`). Zestaw SDK sieci Web jest wymagany w przypadku odwoływania się do [pakietu Microsoft. AspNetCore. app](xref:fundamentals/metapackage-app).

Te wymagania wstępne można zobaczyć w [przykładowej aplikacji](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/test/integration-tests/samples/). Sprawdź plik *Tests/RazorPagesProject. Tests/RazorPagesProject. tests. csproj* . Przykładowa aplikacja używa środowiska testowego [xUnit](https://xunit.github.io/) i biblioteki analizatora [AngleSharp](https://anglesharp.github.io/) , dzięki czemu Przykładowa aplikacja również odwołuje się do:

* [xUnit](https://www.nuget.org/packages/xunit/)
* [xUnit. Runner. VisualStudio](https://www.nuget.org/packages/xunit.runner.visualstudio/)
* [AngleSharp](https://www.nuget.org/packages/AngleSharp/)

## <a name="sut-environment"></a>Środowisko SUT

Jeśli [środowisko](xref:fundamentals/environments) SUT nie jest ustawione, środowisko jest domyślnie stosowane do programowania.

## <a name="basic-tests-with-the-default-webapplicationfactory"></a>Podstawowe testy z domyślną WebApplicationFactory

[WebApplicationFactory&lt;TEntryPoint&gt; ](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactory-1) jest używany do tworzenia [TestServer](/dotnet/api/microsoft.aspnetcore.testhost.testserver) dla testów integracji. `TEntryPoint`jest klasą punktu wejścia SUT, zazwyczaj `Startup` klasy.

Klasy testowe implementują interfejs *armatury klasy* ([IClassFixture](https://xunit.github.io/docs/shared-context#class-fixture)) w celu wskazania, że Klasa zawiera testy i udostępnia wystąpienia obiektów udostępnionych w ramach testów w klasie.

### <a name="basic-test-of-app-endpoints"></a>Podstawowy test punktów końcowych aplikacji

Poniższa klasa testowa, `BasicTests`, `WebApplicationFactory` używa do ładowania początkowego SUT i zapewnienia `Get_EndpointsReturnSuccessAndCorrectContentType` [HttpClient](/dotnet/api/system.net.http.httpclient) do metody testowej. Metoda sprawdza, czy kod stanu odpowiedzi powiedzie się (kody stanu z zakresu 200-299), a `Content-Type` nagłówek jest `text/html; charset=utf-8` dla kilku stron aplikacji.

[Klient](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactory-1.createclient) tworzy wystąpienie `HttpClient` , które automatycznie następuje po przekierowaniu i obsłuży pliki cookie.

[!code-csharp[](integration-tests/samples/2.x/IntegrationTestsSample/tests/RazorPagesProject.Tests/IntegrationTests/BasicTests.cs?name=snippet1)]

Domyślnie nieistotne pliki cookie nie są zachowywane między żądaniami, gdy [zasady zgody Rodo](xref:security/gdpr) są włączone. Aby zachować nieistotne pliki cookie, takie jak te używane przez dostawcę TempData, oznacz je jako niezbędne w testach. Aby uzyskać instrukcje dotyczące oznaczania pliku cookie jako kluczowego, zobacz [podstawowe pliki cookie](xref:security/gdpr#essential-cookies).

### <a name="test-a-secure-endpoint"></a>Testowanie bezpiecznego punktu końcowego

Inny test w `BasicTests` klasie sprawdza, czy bezpieczny punkt końcowy przekierowuje nieuwierzytelnionego użytkownika do strony logowania aplikacji.

W SUT `/SecurePage` strona używa konwencji [AuthorizePage](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizepage) w celu zastosowania [AuthorizeFilter](/dotnet/api/microsoft.aspnetcore.mvc.authorization.authorizefilter) do strony. Aby uzyskać więcej informacji, zobacz [Razor Pages Konwencji autoryzacji](xref:security/authorization/razor-pages-authorization#require-authorization-to-access-a-page).

[!code-csharp[](integration-tests/samples/2.x/IntegrationTestsSample/src/RazorPagesProject/Startup.cs?name=snippet1)]

W teście [WebApplicationFactoryClientOptions](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactoryclientoptions) jest ustawiony tak, aby nie zezwalać na przekierowania przez ustawienie `false`AllowAutoRedirect na: [](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactoryclientoptions.allowautoredirect) `Get_SecurePageRequiresAnAuthenticatedUser`

[!code-csharp[](integration-tests/samples/2.x/IntegrationTestsSample/tests/RazorPagesProject.Tests/IntegrationTests/BasicTests.cs?name=snippet2)]

W przypadku niezezwalania klientowi na śledzenie przekierowania można wykonać następujące operacje:

* Kod stanu zwracany przez SUT można sprawdzić pod kątem oczekiwanego wyniku [HttpStatusCode. Redirect](/dotnet/api/system.net.httpstatuscode) , a nie do końcowego kodu stanu po przekierowaniu do strony logowania, która byłaby [HttpStatusCode. ok](/dotnet/api/system.net.httpstatuscode).
* Wartość nagłówka w nagłówkach odpowiedzi jest sprawdzana w celu potwierdzenia, że `http://localhost/Identity/Account/Login`zaczyna się od, a nie od końcowej odpowiedzi na `Location` stronę logowania, gdzie nagłówek nie może być obecny. `Location`

Aby uzyskać więcej informacji `WebApplicationFactoryClientOptions`na temat, zobacz sekcję [Opcje klienta](#client-options) .

## <a name="customize-webapplicationfactory"></a>Dostosuj WebApplicationFactory

Konfigurację hosta sieci Web można utworzyć niezależnie od klas testów przez dziedziczenie z `WebApplicationFactory` programu w celu utworzenia jednego lub kilku fabryk niestandardowych:

1. Dziedzicz z `WebApplicationFactory` i Zastąp [ConfigureWebHost](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactory-1.configurewebhost). [IWebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder) umożliwia konfigurację kolekcji usług z [ConfigureServices](/dotnet/api/microsoft.aspnetcore.hosting.istartup.configureservices):

   [!code-csharp[](integration-tests/samples/2.x/IntegrationTestsSample/tests/RazorPagesProject.Tests/CustomWebApplicationFactory.cs?name=snippet1)]

   Wypełnianie bazy danych w [aplikacji przykładowej](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/test/integration-tests/samples) jest wykonywane `InitializeDbForTests` przez metodę. Metoda jest opisana w [przykładzie testów integracji: Sekcja testowa](#test-app-organization) organizacja aplikacji.

2. Użyj niestandardowych `CustomWebApplicationFactory` klas testowych. W poniższym przykładzie zastosowano fabrykę w `IndexPageTests` klasie:

   [!code-csharp[](integration-tests/samples/2.x/IntegrationTestsSample/tests/RazorPagesProject.Tests/IntegrationTests/IndexPageTests.cs?name=snippet1)]

   Klient aplikacji przykładowej jest skonfigurowany tak, aby uniemożliwiać `HttpClient` następujące przekierowania. Zgodnie z opisem w sekcji [testowanie bezpiecznego punktu końcowego](#test-a-secure-endpoint) umożliwia ona testom sprawdzenie wyniku pierwszej odpowiedzi aplikacji. Pierwsza odpowiedź to przekierowanie w wielu z tych testów z `Location` nagłówkiem.

3. Typowy test używa `HttpClient` metod i, aby przetworzyć żądanie i odpowiedź:

   [!code-csharp[](integration-tests/samples/2.x/IntegrationTestsSample/tests/RazorPagesProject.Tests/IntegrationTests/IndexPageTests.cs?name=snippet2)]

Wszelkie żądania POST do SUT muszą być zgodne z sprawdzeniem, czy jest ono automatycznie wykonywane przez [system ochrony danych przed fałszerstwem](xref:security/data-protection/introduction). Aby można było rozmieścić żądanie POST testu, aplikacja testowa musi:

1. Utwórz żądanie dla strony.
1. Przeanalizuj plik cookie dotyczący fałszowania i token walidacji żądania z odpowiedzi.
1. Wprowadź żądanie POST przy użyciu pliku cookie służącego do fałszerstwa i tokenu walidacji żądania.

[](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/test/integration-tests/samples/) `GetDocumentAsync` [](https://anglesharp.github.io/)Metody rozszerzenia pomocnika(pomocnicys/HttpClientExtensions.cs)imetodapomocnika(pomocnicys/HtmlHelpers.cs)wprzykładowejaplikacjiużywająanalizatoraAngleSharpdoobsługiochronyprzedfałszerstwem`SendAsync` Sprawdź następujące metody:

* `GetDocumentAsync`Odbiera HttpResponseMessage [](/dotnet/api/system.net.http.httpresponsemessage) i zwraca `IHtmlDocument`. &ndash; `GetDocumentAsync`używa fabryki przygotowującej *odpowiedź wirtualną* na podstawie oryginału `HttpResponseMessage`. Aby uzyskać więcej informacji, zapoznaj się z [dokumentacją AngleSharp](https://github.com/AngleSharp/AngleSharp#documentation).
* `SendAsync`metody `HttpClient` rozszerzające [HttpRequestMessage](/dotnet/api/system.net.http.httprequestmessage) i Call [SendAsync (HttpRequestMessage)](/dotnet/api/system.net.http.httpclient.sendasync#System_Net_Http_HttpClient_SendAsync_System_Net_Http_HttpRequestMessage_) do przesyłania żądań do SUT. Przeciążenia dla `SendAsync` Zaakceptuj formularz HTML (`IHtmlFormElement`) i następujące:
  * Przycisk przesyłania formularza (`IHtmlElement`)
  * Kolekcja wartości formularza (`IEnumerable<KeyValuePair<string, string>>`)
  * Przycisk Prześlij (`IHtmlElement`) i wartości formularza (`IEnumerable<KeyValuePair<string, string>>`)

> [!NOTE]
> [AngleSharp](https://anglesharp.github.io/) to biblioteka analizy innej firmy używana do celów demonstracyjnych w tym temacie i Przykładowa aplikacja. AngleSharp nie jest obsługiwana ani wymagana do testowania integracji aplikacji ASP.NET Core. Można użyć innych analizatorów, takich jak [pakiet HAP (html)](https://html-agility-pack.net/). Innym podejściem jest napisać kod, który będzie obsługiwał token weryfikacji żądań systemu i bezfałszowający plik cookie.

## <a name="customize-the-client-with-withwebhostbuilder"></a>Dostosowywanie klienta przy użyciu WithWebHostBuilder

Gdy w metodzie testowej jest wymagana dodatkowa konfiguracja, [WithWebHostBuilder](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactory-1.withwebhostbuilder) tworzy nową `WebApplicationFactory` z [IWebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder) , który jest bardziej dostosowany przez konfigurację.

Metoda testowa przykładowej `WithWebHostBuilder`aplikacji pokazuje użycie. [](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/test/integration-tests/samples) `Post_DeleteMessageHandler_ReturnsRedirectToRoot` Ten test służy do usuwania rekordów w bazie danych przez wyzwolenie przesłania formularza w SUT.

Ponieważ inny test w `IndexPageTests` klasie wykonuje operację, która usuwa wszystkie rekordy z bazy danych i może być uruchomiona `Post_DeleteMessageHandler_ReturnsRedirectToRoot` przed metodą, baza danych jest umieszczana w tej metodzie testowej, aby upewnić się, że rekord jest obecny dla SUT do usunięcia. `deleteBtn1` Wybranie przycisku `messages` formularza w SUT jest symulowane w żądaniu do SUT:

[!code-csharp[](integration-tests/samples/2.x/IntegrationTestsSample/tests/RazorPagesProject.Tests/IntegrationTests/IndexPageTests.cs?name=snippet3)]

## <a name="client-options"></a>Opcje klienta

W poniższej tabeli przedstawiono domyślne [WebApplicationFactoryClientOptions](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactoryclientoptions) dostępne podczas tworzenia `HttpClient` wystąpień.

| Opcja | Opis | Domyślny |
| ------ | ----------- | ------- |
| [AllowAutoRedirect](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactoryclientoptions.allowautoredirect) | Pobiera lub ustawia, czy `HttpClient` wystąpienia powinny automatycznie śledzić odpowiedzi przekierowania. | `true` |
| [BaseAddress](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactoryclientoptions.baseaddress) | Pobiera lub ustawia podstawowy adres `HttpClient` wystąpień. | `http://localhost` |
| [HandleCookies](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactoryclientoptions.handlecookies) | Pobiera lub ustawia, `HttpClient` czy wystąpienia powinny obsługiwać pliki cookie. | `true` |
| [MaxAutomaticRedirections](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactoryclientoptions.maxautomaticredirections) | Pobiera lub ustawia maksymalną liczbę odpowiedzi przekierowań, które `HttpClient` powinny być zgodne z wystąpieniami. | 7 |

Utwórz klasę i przekaż ją do metody onclient (wartości domyślne są pokazane w przykładzie kodu): [](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactory-1.createclient) `WebApplicationFactoryClientOptions`

```csharp
// Default client option values are shown
var clientOptions = new WebApplicationFactoryClientOptions();
clientOptions.AllowAutoRedirect = true;
clientOptions.BaseAddress = new Uri("http://localhost");
clientOptions.HandleCookies = true;
clientOptions.MaxAutomaticRedirections = 7;

_client = _factory.CreateClient(clientOptions);
```

## <a name="inject-mock-services"></a>Wsuń usługi imitacji

Usługi można przesłaniać w teście, używając wywołania [ConfigureTestServices](/dotnet/api/microsoft.aspnetcore.testhost.webhostbuilderextensions.configuretestservices) na konstruktorze hosta. **Aby wstrzyknąć usługi makiety, SUT musi mieć `Startup` klasę `Startup.ConfigureServices` z metodą.**

Przykład SUT obejmuje usługę objętą zakresem zwracającą ofertę. Po zażądaniu strony indeksu oferta zostanie osadzona w ukrytym polu na stronie indeksu.

*Services/IQuoteService.cs*:

[!code-csharp[](integration-tests/samples/2.x/IntegrationTestsSample/src/RazorPagesProject/Services/IQuoteService.cs?name=snippet1)]

*Usługi/QuoteService. cs*:

[!code-csharp[](integration-tests/samples/2.x/IntegrationTestsSample/src/RazorPagesProject/Services/QuoteService.cs?name=snippet1)]

*Startup.cs*:

[!code-csharp[](integration-tests/samples/2.x/IntegrationTestsSample/src/RazorPagesProject/Startup.cs?name=snippet2)]

*Pages/index. cshtml. cs*:

[!code-csharp[](integration-tests/samples/2.x/IntegrationTestsSample/src/RazorPagesProject/Pages/Index.cshtml.cs?name=snippet1&highlight=4,9,20,26)]

*Strony/indeks. cs*:

[!code-cshtml[](integration-tests/samples/2.x/IntegrationTestsSample/src/RazorPagesProject/Pages/Index.cshtml?name=snippet_Quote)]

Po uruchomieniu aplikacji SUT jest generowany następujący znacznik:

```html
<input id="quote" type="hidden" value="Come on, Sarah. We&#x27;ve an appointment in 
    London, and we&#x27;re already 30,000 years late.">
```

W celu przetestowania usługi i iniekcji cytatu w teście integracji, usługa makiety jest wstrzykiwana do SUT przez test. Usługa makiety zastępuje aplikację `QuoteService` za pomocą usługi dostarczonej przez aplikację testową o nazwie: `TestQuoteService`

*IntegrationTests.IndexPageTests.cs*:

[!code-csharp[](integration-tests/samples/2.x/IntegrationTestsSample/tests/RazorPagesProject.Tests/IntegrationTests/IndexPageTests.cs?name=snippet4)]

`ConfigureTestServices`jest wywoływana, a usługa o określonym zakresie jest zarejestrowana:

[!code-csharp[](integration-tests/samples/2.x/IntegrationTestsSample/tests/RazorPagesProject.Tests/IntegrationTests/IndexPageTests.cs?name=snippet5&highlight=7-10,17,20-21)]

Adiustacje powstające podczas wykonywania testu odzwierciedlają tekst cytatu dostarczony przez `TestQuoteService`, w związku z czym potwierdzenie przebiega:

```html
<input id="quote" type="hidden" value="Something&#x27;s interfering with time, 
    Mr. Scarman, and time is my business.">
```

## <a name="how-the-test-infrastructure-infers-the-app-content-root-path"></a>Jak infrastruktura testowa wnioskuje ścieżkę katalogu głównego zawartości aplikacji

Konstruktor wnioskuje ścieżkę katalogu głównego zawartości aplikacji, wyszukując [WebApplicationFactoryContentRootAttribute](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactorycontentrootattribute) na zestawie zawierającym testy integracji z `TEntryPoint` kluczem równym zestawowi. `System.Reflection.Assembly.FullName` `WebApplicationFactory` W przypadku nieznalezienia `WebApplicationFactory` atrybutu o poprawnym kluczu Wróć do wyszukiwania pliku rozwiązania ( *\*.* `TEntryPoint` sln) i dołączaj nazwę zestawu do katalogu rozwiązania. Katalog główny aplikacji (ścieżka katalogu głównego zawartości) służy do odnajdywania widoków i plików zawartości.

## <a name="disable-shadow-copying"></a>Wyłącz kopiowanie w tle

Kopiowanie w tle powoduje, że testy są wykonywane w innym katalogu niż katalog wyjściowy. Aby testy działały prawidłowo, kopiowanie w tle musi być wyłączone. [Przykładowa aplikacja](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/test/integration-tests/samples) używa xUnit i wyłącza kopiowanie w tle dla xUnit, dołączając plik *xUnit. Runner. JSON* z prawidłowym ustawieniem konfiguracji. Aby uzyskać więcej informacji, zobacz [Configuring xUnit with JSON](https://xunit.github.io/docs/configuring-with-json.html).

Dodaj plik *xUnit. Runner. JSON* do katalogu głównego projektu testowego z następującą zawartością:

```json
{
  "shadowCopy": false
}
```

Jeśli używasz programu Visual Studio, ustaw wartość właściwości **Kopiuj do katalogu wyjściowego** na **zawsze Kopiuj**. Jeśli nie korzystasz z programu Visual Studio `Content` , Dodaj obiekt docelowy do pliku projektu aplikacji testowej:

```xml
<ItemGroup>
  <Content Update="xunit.runner.json">
    <CopyToOutputDirectory>Always</CopyToOutputDirectory>
  </Content>
</ItemGroup>
```

## <a name="disposal-of-objects"></a>Usuwanie obiektów

Po wykonaniu `IClassFixture` testów wdrożenia [TestServer](/dotnet/api/microsoft.aspnetcore.testhost.testserver) i [HttpClient](/dotnet/api/system.net.http.httpclient) są usuwane, gdy xUnit usuwa [WebApplicationFactory](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactory-1). Jeśli obiekty utworzone przez dewelopera wymagają usunięcia, usuń je w `IClassFixture` implementacji. Aby uzyskać więcej informacji, zobacz [implementowanie metody Dispose](/dotnet/standard/garbage-collection/implementing-dispose).

## <a name="integration-tests-sample"></a>Przykład testów integracji

[Przykładowa aplikacja](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/test/integration-tests/samples) składa się z dwóch aplikacji:

| Aplikacja | Katalog projektu | Opis |
| --- | ----------------- | ----------- |
| Aplikacja wiadomości (SUT) | *src/RazorPagesProject* | Zezwala użytkownikowi na dodawanie, usuwanie, usuwanie wszystkich i analizowanie komunikatów. |
| Aplikacja testowa | *tests/RazorPagesProject.Tests* | Służy do integracji testu SUT. |

Testy można uruchamiać przy użyciu wbudowanych funkcji testowych środowiska IDE, takich jak [Visual Studio](https://visualstudio.microsoft.com). W przypadku używania [Visual Studio Code](https://code.visualstudio.com/) lub wiersza polecenia wykonaj następujące polecenie w wierszu polecenia w katalogu *Tests/RazorPagesProject. Tests* :

```console
dotnet test
```

### <a name="message-app-sut-organization"></a>Organizacja aplikacji wiadomości (SUT)

SUT to system komunikatów Razor Pages o następujących cechach:

* Strona indeks aplikacji (*Pages/index. cshtml* i Pages */index. cshtml. cs*) zawiera metody interfejsu użytkownika i modelu strony umożliwiające sterowanie dodawaniem, usuwaniem i analizą komunikatów (średnia liczba wyrazów na komunikat).
* Komunikat jest opisywany `Message` przez klasę (*Data/Message. cs*) z dwiema właściwościami: `Id` (Key) i `Text` (Message). `Text` Właściwość jest wymagana i jest ograniczona do 200 znaków.
* Komunikaty są przechowywane przy użyciu&#8224; [bazy danych znajdującej się w pamięci Entity Framework](/ef/core/providers/in-memory/).
* Aplikacja zawiera warstwę dostępu do danych (dal) w swojej klasie `AppDbContext` kontekstu bazy danych (*Data/AppDbContext. cs*).
* Jeśli baza danych jest pusta podczas uruchamiania aplikacji, magazyn komunikatów zostanie zainicjowany przy użyciu trzech komunikatów.
* Aplikacja zawiera dostęp do `/SecurePage` programu, do którego jest dostępny tylko uwierzytelniony użytkownik.

&#8224;W temacie EF [test z](/ef/core/miscellaneous/testing/in-memory)niepamięcią, wyjaśniono, jak korzystać z bazy danych w pamięci dla testów z MSTest. W tym temacie jest stosowane środowisko testowe [xUnit](https://xunit.github.io/) . Koncepcje testowe i implementacje testów w różnych strukturach testów są podobne, ale nie są identyczne.

Mimo że aplikacja nie używa wzorca repozytorium i nie jest skutecznym przykładem [wzorca jednostki pracy](https://martinfowler.com/eaaCatalog/unitOfWork.html), Razor Pages obsługuje te wzorce rozwoju. Aby uzyskać więcej informacji, zobacz [projektowanie warstwy trwałości infrastruktury](/dotnet/standard/microservices-architecture/microservice-ddd-cqrs-patterns/infrastructure-persistence-layer-design) i [logiki kontrolera testów](/aspnet/core/mvc/controllers/testing) (przykład implementuje wzorzec repozytorium).

### <a name="test-app-organization"></a>Testuj organizację aplikacji

Aplikacja testowa to Aplikacja konsolowa w katalogu *Tests/RazorPagesProject. Tests* .

| Testuj katalog aplikacji | Opis |
| ------------------ | ----------- |
| *BasicTests* | *BasicTests.cs* zawiera metody testowe do routingu, uzyskiwania dostępu do bezpiecznej strony przez nieuwierzytelnionego użytkownika i uzyskiwania profilu użytkownika usługi GitHub oraz sprawdzania logowania użytkownika profilu. |
| *IntegrationTests* | *IndexPageTests.cs* zawiera testy integracji dla strony indeksu przy użyciu klasy niestandardowej `WebApplicationFactory` . |
| *Pomocnicy/narzędzia* | <ul><li>*Utilities.cs* zawiera `InitializeDbForTests` metodę używaną do wypełniania bazy danych danymi testowymi.</li><li>*HtmlHelpers.cs* zapewnia metodę, która zwraca AngleSharp `IHtmlDocument` do użycia przez metody testowe.</li><li>*HttpClientExtensions.cs* zapewniają przeciążenia dla `SendAsync` programu, aby przesyłać żądania do SUT.</li></ul> |

Platforma testowa jest [xUnit](https://xunit.github.io/). Testy integracji są przeprowadzane przy użyciu [programu Microsoft. AspNetCore. TestHost](/dotnet/api/microsoft.aspnetcore.testhost), który obejmuje [TestServer](/dotnet/api/microsoft.aspnetcore.testhost.testserver). Ponieważ pakiet [Microsoft. AspNetCore. MVC. test](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Testing) służy do konfigurowania hosta testowego i serwera testowego, `TestHost` pakiety i `TestServer` nie wymagają bezpośrednich odwołań do pakietu w pliku projektu lub deweloperze aplikacji testowej Konfiguracja w aplikacji testowej.

**Umieszczanie bazy danych do testowania**

Testy integracji zwykle wymagają małego zestawu danych w bazie danych przed wykonaniem testu. Na przykład można usunąć wywołania testu do usuwania rekordów bazy danych, więc baza danych musi mieć co najmniej jeden rekord, aby żądanie usunięcia zakończyło się pomyślnie.

Przykładowa aplikacja odziarnauje bazę danych z trzema komunikatami w *Utilities.cs* , że testy mogą być używane podczas wykonywania:

[!code-csharp[](integration-tests/samples/2.x/IntegrationTestsSample/tests/RazorPagesProject.Tests/Helpers/Utilities.cs?name=snippet1)]

## <a name="additional-resources"></a>Dodatkowe zasoby

* [Testy jednostkowe](/dotnet/articles/core/testing/unit-testing-with-dotnet-test)
* [Testy jednostkowe stron Razor](xref:test/razor-pages-tests)
* [Oprogramowanie pośredniczące](xref:fundamentals/middleware/index)
* [Kontrolery testów](xref:mvc/controllers/testing)
