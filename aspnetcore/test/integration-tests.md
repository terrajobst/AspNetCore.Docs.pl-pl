---
title: Integracja z testów w ASP.NET Core
author: guardrex
description: Dowiedz się, jak testy integracji zapewnienia poprawnego działania składniki aplikacji na poziomie infrastruktury, włącznie z bazy danych, system plików i sieci.
manager: wpickett
ms.author: riande
ms.custom: mvc
ms.date: 05/30/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: test/integration-tests
ms.openlocfilehash: a402c3f5f6a75917eba4058e6cc6926f25b214d4
ms.sourcegitcommit: 726ffab258070b4fe6cf950bf030ce10c0c07bb4
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/04/2018
ms.locfileid: "34734719"
---
# <a name="integration-tests-in-aspnet-core"></a>Integracja z testów w ASP.NET Core

Przez [Luke Latham](https://github.com/guardrex) i [Steve Smith](https://ardalis.com/)

Testy integracji upewnij się, czy składniki aplikacji działać poprawnie na poziomie, który obejmuje obsługę infrastruktury aplikacji, takich jak bazy danych, system plików i sieci. Platformy ASP.NET Core obsługuje integrację testy za pomocą frameworka testów jednostkowych z hosta testu sieci web a serwerem testu w pamięci.

W tym temacie założono podstawową wiedzę na temat testów jednostkowych. Jeśli znasz koncepcji testu, zobacz [testów jednostkowych w .NET Core i .NET Standard](/dotnet/core/testing/) temat i jego połączonej zawartości.

[Wyświetlić lub pobrać przykładowy kod](https://github.com/aspnet/Docs/tree/master/aspnetcore/test/integration-tests/samples) ([sposobu pobierania](xref:tutorials/index#how-to-download-a-sample))

Przykładowa aplikacja jest aplikacją stron Razor przyjęto założenie, podstawową wiedzę na temat stron Razor. Jeśli znasz stron Razor, zobacz następujące tematy:

* [Wprowadzenie do stron Razor](xref:mvc/razor-pages/index)
* [Wprowadzenie do korzystania ze stron Razor](xref:tutorials/razor-pages/razor-pages-start)
* [Testy jednostkowe stron razor](xref:test/razor-pages-tests)

## <a name="introduction-to-integration-tests"></a>Wprowadzenie do integracji testów

Testy integracji ocenić składniki aplikacji w szerszym zakresie niż [testów jednostkowych](/dotnet/core/testing/). Testy jednostkowe są wykorzystywane do testowania oprogramowania izolowane składniki, takie jak poszczególnych klas, metod. Testy integracji upewnij się, co najmniej dwóch składników aplikacji współpracują ze sobą do produkcji oczekiwany wynik, może obejmować każdego składnika wymaganego do pełne przetworzenie żądania.

Testy szerszych są wykorzystywane do testowania aplikacji infrastruktury i całej platformy, często w tym następujących składników:

* Baza danych
* System plików
* Urządzenia sieciowe
* Potok żądań i odpowiedzi

Użyj wykonane składniki, nazywane testów jednostkowych *substytutów* lub *mock obiektów*, zamiast składników infrastruktury.

W przeciwieństwie do testów jednostkowych integracji testów:

* Użyj rzeczywistego składników, które korzysta z aplikacji w środowisku produkcyjnym.
* Wymagają więcej kodu i przetwarzania danych.
* Działał dłużej.

W związku z tym ograniczyć użycie integracji testów do najważniejszych scenariuszach infrastruktury. To zachowanie można przetestować przy użyciu testu jednostkowego lub testu integracji, jeśli test jednostkowy.

> [!TIP]
> Nie napisać testy integracji dla każdej permutacji możliwe dostępu do danych i pliku z bazami danych i systemy plików. Niezależnie od tego, ile miejsca w aplikacji interakcji z baz danych i systemy plików, ukierunkowanych zestaw odczytu, zapisu, aktualizacji i usuwania integracji testy są zazwyczaj można odpowiednio testowanie bazy danych i składników systemu plików. Testów jednostkowych używany dla procedury testów logikę metody wchodzące w interakcje z tych składników. W testach jednostkowych korzystanie z infrastruktury elementów sztucznych/mocks doprowadzi do szybszego wykonywania testu.

> [!NOTE]
> W dyskusjach ze specjalistami integracji testów, przetestowany projekt jest często nazywany *testowanym systemie*, lub "SUT" skrócie.

## <a name="aspnet-core-integration-tests"></a>Testy integracji platformy ASP.NET Core

Integracja z testów w ASP.NET Core wymagają, aby:

* Projekt testowy służy do zawierają i wykonywania testów. Projekt testowy zawiera odwołanie do projektu platformy ASP.NET Core przetestowane, o nazwie *testowanym systemie* (SUT). _"SUT" jest używana w tym temacie do odwoływania się do aplikacji przetestowane._
* Projekt testowy tworzy testu sieci web hosta dla SUT i używa klienta serwera testowego do obsługi żądań i odpowiedzi na SUT.
* Moduł uruchamiający służy do wykonywania testów i raportu wyników testu.

Testy integracji wykonaj sekwencję zdarzeń, które obejmują zwykle *Rozmieść*, *Act*, i *Assert* testowania czynności:

1. SUT hosta sieci web jest skonfigurowana.
1. Klient serwera testowy jest tworzony do przesyłania żądań do aplikacji.
1. *Rozmieść* krok testu jest wykonywana: przetestuj aplikację przygotowuje żądanie.
1. *Act* krok testu jest wykonywana: klient przesyła żądanie i odbiera odpowiedź.
1. *Assert* krok testu jest wykonywana: *rzeczywiste* odpowiedzi jest zweryfikowany jako *przekazać* lub *się nie powieść* na podstawie *oczekiwano*  odpowiedzi.
1. Proces jest kontynuowany, dopóki wszystkie testy są wykonywane.
1. Wyniki testu są zgłaszane.

Zazwyczaj hosta testów sieci web skonfigurowano inaczej niż aplikacji sieci web normalne dla testu jest uruchomiona. Na przykład ustawienia innej aplikacji lub innej bazy danych można użyć dla testów.

Składniki infrastruktury, takie jak hosta testów sieci web i testów w pamięci serwera ([elementu TestServer](/dotnet/api/microsoft.aspnetcore.testhost.testserver)), są podane lub są zarządzane przez [Microsoft.AspNetCore.Mvc.Testing](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Testing) pakietu. Użyj tego pakietu upraszcza tworzenie testu i wykonywania.

`Microsoft.AspNetCore.Mvc.Testing` Pakiet obsługuje następujące zadania:

* Kopiuje plik zależności (*\*.deps*) z SUT do projektu testowego *bin* folderu.
* Ustawia zawartości katalogu głównego projektu SUT tak, aby pliki statyczne i stron/widoki są dostępne, gdy testy są wykonywane.
* Udostępnia [WebApplicationFactory](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactory-1) klasę, aby usprawnić uruchamianie SUT z `TestServer`.

[Testów jednostkowych](/dotnet/articles/core/testing/unit-testing-with-dotnet-test) dokumentacji opisano sposób konfigurowania projektu i testowania uruchamiający, oraz szczegółowe informacje na temat do uruchomienia testów i zalecenia dotyczące do nazwy testów i testowania klasy.

> [!NOTE]
> Podczas tworzenia projektu testowego dla aplikacji, należy oddzielić testów jednostkowych z testów integracji do różnych projektów. Pomaga to zapewnić, że testowanie składników infrastruktury nie są ani przypadkowo uwzględnione w testach jednostkowych. Umożliwia również oddzielenie testy jednostkowe i integracji kontroli, przez który zestaw testów są uruchamiane.

Nie ma praktycznie różnic między konfiguracji testów aplikacji Razor stron i aplikacji MVC. Jedyną różnicą jest w sposób nazywania testy. W aplikacji stron Razor, testy strony punktów końcowych są najczęściej nosi klasy modelu strony (na przykład `IndexPageTests` do testowania składników integracji strony indeksu). W aplikacji MVC, testy są zazwyczaj są zorganizowane według klasy kontrolera i o nazwie po kontrolerów sprawdzają one (na przykład `HomeControllerTests` do testowania składników integracji dla kontrolera głównej).

## <a name="test-app-prerequisites"></a>Testowanie aplikacji wymagania wstępne

Projekt testowy musi:

* Odwołania do pakietu [Microsoft.AspNetCore.App](https://www.nuget.org/packages/Microsoft.AspNetCore.App/).
* Używanie zestawu SDK sieci Web w pliku projektu (`<Project Sdk="Microsoft.NET.Sdk.Web">`).

Te prerequesities są widoczne w [Przykładowa aplikacja](https://github.com/aspnet/Docs/tree/master/aspnetcore/test/integration-tests/samples/). Sprawdź *tests/RazorPagesProject.Tests/RazorPagesProject.Tests.csproj* pliku.

## <a name="basic-tests-with-the-default-webapplicationfactory"></a>Podstawowe testy przy użyciu domyślnego WebApplicationFactory

[WebApplicationFactory&lt;TEntryPoint&gt; ](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactory-1) służy do tworzenia [elementu TestServer](/dotnet/api/microsoft.aspnetcore.testhost.testserver) testów integracji. `TEntryPoint` jest klasą punktu wejścia SUT, zwykle `Startup` klasy.

Implementacja klasy testu *osprzętu klasy* interfejsu (`IClassFixture`) oznacza klasy zawiera testy i zapewniają wystąpień obiektu współużytkowanego przez testów w klasie.

### <a name="basic-test-of-app-endpoints"></a>Podstawowy test punktów końcowych aplikacji

Następującego testu, klasy, `BasicTests`, używa `WebApplicationFactory` bootstrap SUT i podaj [HttpClient](/dotnet/api/system.net.http.httpclient) do metody testowej, `Get_EndpointsReturnSuccessAndCorrectContentType`. Metoda sprawdza, czy kod stanu odpowiedzi jest pomyślne (kodów stanu zakresu 200 299) i `Content-Type` nagłówek jest `text/html; charset=utf-8` na wielu stronach w aplikacji.

[CreateClient](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactory-1.createclient) tworzy wystąpienie `HttpClient` który automatycznie wykonuje przekierowania i obsługuje pliki cookie.

[!code-csharp[](integration-tests/samples/2.x/IntegrationTestsSample/tests/RazorPagesProject.Tests/IntegrationTests/BasicTests.cs?name=snippet1)]

### <a name="test-a-secure-endpoint"></a>Testowanie bezpiecznego punktu końcowego

Innego testu w `BasicTests` klasa sprawdza, czy bezpieczny punkt końcowy przekierowuje nieuwierzytelniony użytkownik do strony logowania aplikacji.

W SUT `/SecurePage` strona używa [AuthorizePage](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizepage) Konwencji, aby zastosować [AuthorizeFilter](/dotnet/api/microsoft.aspnetcore.mvc.authorization.authorizefilter) do strony. Aby uzyskać więcej informacji, zobacz [konwencje autoryzacji stron Razor](xref:security/authorization/razor-pages-authorization#require-authorization-to-access-a-page).

[!code-csharp[](integration-tests/samples/2.x/IntegrationTestsSample/src/RazorPagesProject/Startup.cs?name=snippet1)]

W `Get_SecurePageRequiresAnAuthenticatedUser` przetestować, [WebApplicationFactoryClientOptions](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactoryclientoptions) ustawiono nie zezwalaj na przekierowuje przez ustawienie [AllowAutoRedirect](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactoryclientoptions.allowautoredirect) do `false`:

[!code-csharp[](integration-tests/samples/2.x/IntegrationTestsSample/tests/RazorPagesProject.Tests/IntegrationTests/BasicTests.cs?name=snippet2)]

Zezwalając na klientowi wykonaj przekierowania, może się następujące testy:

* Kod stanu zwrócony przez SUT można sprawdzić przed oczekiwanej [HttpStatusCode.Redirect](/dotnet/api/system.net.httpstatuscode) wynik, nie kod stanu końcowego po przekierowanie do strony logowania, która byłaby [HttpStatusCode.OK](/dotnet/api/system.net.httpstatuscode).
* `Location` Wartość nagłówka w nagłówkach odpowiedzi jest sprawdzane, aby potwierdzić, że rozpoczyna się od `http://localhost/Identity/Account/Login`, nie końcowego logowania czasu odpowiedzi strony, gdzie `Location` nagłówka nie będą obecne.

Aby uzyskać więcej informacji na temat `WebApplicationFactoryClientOptions`, zobacz [opcje klienta](#client-options) sekcji.

## <a name="customize-webapplicationfactory"></a>Dostosowywanie WebApplicationFactory

Konfiguracja hosta sieci Web można tworzyć niezależnie od klasy testu przez dziedziczenie z `WebApplicationFactory` można utworzyć co najmniej jeden niestandardowy fabryk:

1. Dziedzicz `WebApplicationFactory` i zastąpić [ConfigureWebHost](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactory-1.configurewebhost). [IWebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder) umożliwia skonfigurowanie kolekcji usługi z [ConfigureServices](/dotnet/api/microsoft.aspnetcore.hosting.istartup.configureservices):

   [!code-csharp[](integration-tests/samples/2.x/IntegrationTestsSample/tests/RazorPagesProject.Tests/CustomWebApplicationFactory.cs?name=snippet1)]

   Wstępne wypełnianie w bazie danych [Przykładowa aplikacja](https://github.com/aspnet/Docs/tree/master/aspnetcore/test/integration-tests/samples) odbywa się za pośrednictwem `InitializeDbForTests` metody. Metoda jest opisana w [integracji testów próbki: testowanie aplikacji organizacji](#test-app-organization) sekcji.

2. Użyj niestandardowego `CustomWebApplicationFactory` w klasach testu. W poniższym przykładzie użyto fabryce `IndexPageTests` klasy:

   [!code-csharp[](integration-tests/samples/2.x/IntegrationTestsSample/tests/RazorPagesProject.Tests/IntegrationTests/IndexPageTests.cs?name=snippet1)]

   Przykładowa aplikacja klient jest skonfigurowany do zapobiec `HttpClient` z następujących przekierowania. Zgodnie z objaśnieniem w [Test bezpieczny punkt końcowy](#test-a-secure-endpoint) sekcji pozwala testów, aby sprawdzić wynik pierwszej odpowiedzi z aplikacji. Pierwszą odpowiedź jest przekierowanie w wielu z tych testów z `Location` nagłówka.

3. Typowy przetestujesz `HttpClient` i metody pomocnicze do przetwarzania żądania i odpowiedzi:

   [!code-csharp[](integration-tests/samples/2.x/IntegrationTestsSample/tests/RazorPagesProject.Tests/IntegrationTests/IndexPageTests.cs?name=snippet2)]

Wszystkie żądania POST do SUT muszą spełniać antiforgery Sprawdź, czy staje się automatycznie za pomocą aplikacji [antiforgery systemu ochrony danych](xref:security/data-protection/introduction). Aby zorganizować dla żądania POST testu, przetestuj aplikację musi:

1. Tworzenie żądania strony.
1. Przeanalizować antiforgery plik cookie i token weryfikacji żądania z odpowiedzi.
1. Należy żądania POST z antiforgery weryfikacji pliku cookie i żądania tokenu w miejscu.

`SendAsync` Metody rozszerzenia pomocnika (*Helpers/HttpClientExtensions.cs*) i `GetDocumentAsync` metody pomocniczej (*Helpers/HtmlHelpers.cs*) w [Przykładowa aplikacja](https://github.com/aspnet/Docs/tree/master/aspnetcore/test/integration-tests/samples/) użyj [AngleSharp](https://anglesharp.github.io/) analizator w celu obsługi antiforgery wyboru z następujących metod:

* `GetDocumentAsync` &ndash; Odbiera [HttpResponseMessage](/dotnet/api/system.net.http.httpresponsemessage) i zwraca `IHtmlDocument`. `GetDocumentAsync` używa fabrykę przygotowuje *wirtualnego odpowiedzi* oparty na oryginalnym `HttpResponseMessage`. Aby uzyskać więcej informacji, zobacz [dokumentacji AngleSharp](https://github.com/AngleSharp/AngleSharp#documentation).
* `SendAsync` metody rozszerzenia dla `HttpClient` tworzą [HttpRequestMessage](/dotnet/api/system.net.http.httprequestmessage) i Wywołaj [SendAsync(HttpRequestMessage)](/dotnet/api/system.net.http.httpclient.sendasync#System_Net_Http_HttpClient_SendAsync_System_Net_Http_HttpRequestMessage_) do przesyłania żądań do SUT. Overloads dla `SendAsync` zaakceptować formularza HTML (`IHtmlFormElement`) oraz następujące:
  - Przedstawia przycisk formularza (`IHtmlElement`)
  - Kolekcja wartości formularza (`IEnumerable<KeyValuePair<string, string>>`)
  - Przycisk Prześlij (`IHtmlElement`) i tworzyć wartości (`IEnumerable<KeyValuePair<string, string>>`)

> [!NOTE]
> [AngleSharp](https://anglesharp.github.io/) jest analiza kodu innych firm biblioteki używane w celach demonstracyjnych, w tym temacie i przykładowej aplikacji. AngleSharp nie jest obsługiwany lub wymagana do integracji testowania aplikacji platformy ASP.NET Core. Inne analizatory składni mogą być używane, takich jak [pakiet elastyczność Html (HAP)](http://html-agility-pack.net/). Innym rozwiązaniem jest napisać kod, aby obsłużyć żądania tokenu weryfikacji antiforgery systemu i plik cookie antiforgery bezpośrednio.

## <a name="customize-the-client-with-withwebhostbuilder"></a>Dostosowywanie klienta z WithWebHostBuilder

Gdy w ramach metody testowej, wymagana jest dodatkowa konfiguracja [WithWebHostBuilder](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactory-1.withwebhostbuilder) tworzy nowy `WebApplicationFactory` z [IWebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder) dalsze dostosowaniu przez konfigurację.

`Post_DeleteMessageHandler_ReturnsRedirectToRoot` Test metody [Przykładowa aplikacja](https://github.com/aspnet/Docs/tree/master/aspnetcore/test/integration-tests/samples) zademonstrowano użycie `WithWebHostBuilder`. Ten test polega na Usuń rekord w bazie danych wyzwalając w SUT przesyłania formularza.

Ponieważ innego testu w `IndexPageTests` klasa wykonuje operację, która usuwa wszystkie rekordy w bazie danych i mogą być uruchamiane przed `Post_DeleteMessageHandler_ReturnsRedirectToRoot` metody, bazy danych jest obsługiwany w tej metodzie testu do zapewnienia, że rekord jest stosowany w przypadku SUT można usunąć. Wybieranie `deleteBtn1` przycisku `messages` formularza w SUT symulacji w żądaniu, aby SUT:

[!code-csharp[](integration-tests/samples/2.x/IntegrationTestsSample/tests/RazorPagesProject.Tests/IntegrationTests/IndexPageTests.cs?name=snippet3)]

## <a name="client-options"></a>Opcje klientów

W poniższej tabeli przedstawiono domyślne [WebApplicationFactoryClientOptions](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactoryclientoptions) dostępne podczas tworzenia `HttpClient` wystąpień.

| Opcja | Opis | Domyślny |
| ------ | ----------- | ------- |
| [AllowAutoRedirect](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactoryclientoptions.allowautoredirect) | Pobiera lub ustawia, czy `HttpClient` wystąpień automatycznie należy stosować odpowiedzi przekierowania. | `true` |
| [BaseAddress](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactoryclientoptions.baseaddress) | Pobiera lub ustawia adres bazowy `HttpClient` wystąpień. | `http://localhost` |
| [HandleCookies](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactoryclientoptions.handlecookies) | Pobiera lub ustawia czy `HttpClient` wystąpień powinna obsługiwać pliki cookie. | `true` |
| [MaxAutomaticRedirections](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactoryclientoptions.maxautomaticredirections) | Pobiera lub ustawia maksymalną liczbę odpowiedzi przekierowania, który `HttpClient` wystąpień powinien być zgodny. | 7 |

Utwórz `WebApplicationFactoryClientOptions` klasy i przekaż go do [CreateClient](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactory-1.createclient) — metoda (ustawienie domyślne wartości zostały przedstawione w przykładzie kodu):

```csharp
// Default client option values are shown
var clientOptions = new WebApplicationFactoryClientOptions();
clientOptions.AllowAutoRedirect = true;
clientOptions.BaseAddress = new Uri("http://localhost");
clientOptions.HandleCookies = true;
clientOptions.MaxAutomaticRedirections = 7;

_client = _factory.CreateClient(clientOptions);
```

## <a name="how-the-test-infrastructure-infers-the-app-content-root-path"></a>Jak infrastruktury testowego wnioskuje ścieżki zawartości katalogu głównego aplikacji

`WebApplicationFactory` Konstruktor wnioskuje ścieżki zawartości katalogu głównego aplikacji, wyszukując [WebApplicationFactoryContentRootAttribute](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactorycontentrootattribute) na zestaw zawierający testy integracji z kluczem równa `TEntryPoint` zestawu `System.Reflection.Assembly.FullName`. W przypadku, gdy atrybut przy użyciu poprawnego klucza nie zostanie odnaleziony, `WebApplicationFactory` powraca do wyszukiwania pliku rozwiązania (*\*.sln*) i dołącza `TEntryPoint` nazwy zestawu do katalogu rozwiązania. Katalog główny aplikacji (ścieżka zawartości katalogu głównego) jest używana do wykrywania, widoków i plików zawartości.

W większości przypadków nie trzeba jawnie ustaw główny zawartości aplikacji, jak logika wyszukiwania zazwyczaj znajduje poprawne zawartości katalogu głównego w czasie wykonywania. W scenariuszach specjalne, gdzie zawartości główny nie zostanie odnaleziony przy użyciu algorytmu wyszukiwania wbudowanych, zawartości jawnie lub przy użyciu niestandardowej logiki można określić katalogu głównego aplikacji. Aby ustawić zawartości katalogu głównego aplikacji w tych scenariuszach, wywołaj `UseSolutionRelativeContentRoot` — metoda rozszerzenia z [Microsoft.AspNetCore.TestHost](https://www.nuget.org/packages/Microsoft.AspNetCore.TestHost) pakietu. Podaj ścieżki względnej rozwiązania i wzorzec nazwy lub glob pliku rozwiązania opcjonalne (domyślne = `*.sln`).

Wywołanie [UseSolutionRelativeContentRoot](/dotnet/api/microsoft.aspnetcore.testhost.webhostbuilderextensions.usesolutionrelativecontentroot) przy użyciu metody rozszerzenia *jeden* z następujących metod:

* Podczas konfigurowania klas testu z `WebApplicationFactory`, podaj Konfiguracja niestandardowa z [IWebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder):

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

* Podczas konfigurowania klas testu z niestandardowego `WebApplicationFactory`, dziedziczą z `WebApplicationFactory` i zastąpić [ConfigureWebHost](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactory-1.configurewebhost):

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

## <a name="disable-shadow-copying"></a>Wyłącz kopiowanie w tle

Kopiowanie w tle powoduje, że testy do wykonania w innym folderze niż folder wyjściowy. Dla testów działało poprawnie musi zostać wyłączona kopiowanie w tle. [Przykładowa aplikacja](https://github.com/aspnet/Docs/tree/master/aspnetcore/test/integration-tests/samples) używa xUnit i wyłącza kopiowanie w tle dla xUnit przez dołączenie *xunit.runner.json* pliku z ustawieniem prawidłowej konfiguracji. Aby uzyskać więcej informacji, zobacz [Konfigurowanie xUnit.net z JSON](https://xunit.github.io/docs/configuring-with-json.html).

Dodaj *xunit.runner.json* plik do katalogu głównego projektu testowego o następującej treści:

```json
{
  "shadowCopy": false
}
```

## <a name="integration-tests-sample"></a>Przykładowe testy integracji

[Przykładowa aplikacja](https://github.com/aspnet/Docs/tree/master/aspnetcore/test/integration-tests/samples) składa się z dwóch aplikacji:

| Aplikacji | Folder projektu | Opis |
| --- | -------------- | ----------- |
| Komunikat aplikacji (SUT) | *src/RazorPagesProject* | Zezwala użytkownikowi na dodawanie, Usuń jedno, Usuń wszystkie i analizowania wiadomości. |
| Przetestuj aplikację | *tests/RazorPagesProject.Tests* | Umożliwia integrację testu SUT. |

Testy można uruchomić przy użyciu funkcji wbudowanych testów IDE, takich jak [programu Visual Studio](https://www.visualstudio.com/vs/). Jeśli przy użyciu [Visual Studio Code](https://code.visualstudio.com/) lub wiersza polecenia, uruchom następujące polecenie w wierszu polecenia *tests/RazorPagesProject.Tests* folderu:

```console
dotnet test
```

### <a name="message-app-sut-organization"></a>Komunikat aplikacji (SUT) organizacji

SUT jest systemem stron Razor komunikat o następującej charakterystyce:

* Strona indeksu aplikacji (*Pages/Index.cshtml* i *Pages/Index.cshtml.cs*) zapewnia interfejsu użytkownika i strony metody modelu kontroli Dodawanie, usuwanie i analizy wiadomości (średni słów na wiadomości) .
* Komunikat jest opisane przez `Message` klasy (*Data/Message.cs*) z dwóch właściwości: `Id` (klucz) i `Text` (komunikat). `Text` Właściwość jest wymagana i maksymalnie 200 znaków.
* Wiadomości są przechowywane przy użyciu [bazy danych programu Entity Framework w pamięci](/ef/core/providers/in-memory/)&#8224;.
* Ta aplikacja zawiera Warstwa dostępu do danych (DAL) w swojej klasie kontekst bazy danych `AppDbContext` (*Data/AppDbContext.cs*).
* Jeśli baza danych jest pusta podczas uruchamiania aplikacji, Magazyn komunikatu jest inicjowany z trzech wiadomości.
* Aplikacja zawiera `/SecurePage` który można uzyskać tylko przez uwierzytelnionego użytkownika.

&#8224;Temat EF [testu z InMemory](/ef/core/miscellaneous/testing/in-memory), opisano sposób korzystania z bazy danych w pamięci dla testów z użyciem MSTest. W tym temacie używa [xUnit](https://xunit.github.io/) struktury testowej. Badanie koncepcji i implementacje testu w innym testem struktury są podobne, lecz nie są identyczne.

Mimo że nie korzysta z aplikacji [wzorca repozytorium](http://martinfowler.com/eaaCatalog/repository.html) i nie jest skuteczne przykład [wzorzec jednostki pracy (UoW)](https://martinfowler.com/eaaCatalog/unitOfWork.html), stron Razor obsługuje te wzorce programowania. Aby uzyskać więcej informacji, zobacz [projektowania infrastruktury warstwę trwałości](/dotnet/standard/microservices-architecture/microservice-ddd-cqrs-patterns/infrastructure-persistence-layer-design), [wdrażanie repozytorium i jednostki pracy wzorce w aplikacji platformy ASP.NET MVC](/aspnet/mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application), i [kontrolera testów Logika](/aspnet/core/mvc/controllers/testing) (próbki implementuje wzorca repozytorium).

### <a name="test-app-organization"></a>Testowanie aplikacji organizacji

Test jest to aplikacja konsoli wewnątrz *tests/RazorPagesProject.Tests* folderu.

| Folder aplikacji testu | Opis |
| --------------- | ----------- |
| *BasicTests* | *BasicTests.cs* zawiera testu metod routingu, uzyskać dostęp do bezpiecznego strony nieuwierzytelniony użytkownik i uzyskiwania profilu użytkownika usługi GitHub i kontroli danych logowania użytkownika w profilu. |
| *IntegrationTests* | *IndexPageTests.cs* zawiera testy integracji strony indeksu przy użyciu niestandardowych `WebApplicationFactory` klasy. |
| *Pomocnicy/narzędzia* | <ul><li>*Utilities.cs* zawiera `InitializeDbForTests` metodę używaną do inicjatora bazy danych z danych testowych.</li><li>*HtmlHelpers.cs* udostępnia metodę, aby zwrócić AngleSharp `IHtmlDocument` do użycia przez metody testowe.</li><li>*HttpClientExtensions.cs* Podaj przeciążenia dla `SendAsync` do przesyłania żądań do SUT.</li></ul> |

Struktury testowej jest [xUnit](https://xunit.github.io/). Integracja testów przy użyciu [Microsoft.AspNetCore.TestHost](/dotnet/api/microsoft.aspnetcore.testhost), która obejmuje [elementu TestServer](/dotnet/api/microsoft.aspnetcore.testhost.testserver). Ponieważ [Microsoft.AspNetCore.Mvc.Testing](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Testing) pakietu jest używane do konfigurowania serwera hosta i testowania testu, `TestHost` i `TestServer` pakietów nie wymaga odwołania do pakietu bezpośrednio w pliku projektu aplikacji testów lub Konfiguracja deweloperów w aplikacji testu.

**Wstępne wypełnianie bazy danych do testowania**

Testy integracji zwykle wymagają małym zestawie danych w bazie danych przed wykonywania testu. Na przykład usunięcia testu wymaga usunięcie rekordu bazy danych, więc bazy danych musi zawierać co najmniej jeden rekord dla żądania usunięcia powiodło się.

Przykładowa aplikacja nasiona bazy danych z trzech wiadomości w *Utilities.cs* czy testy można użyć podczas wykonywania:

[!code-csharp[](integration-tests/samples/2.x/IntegrationTestsSample/tests/RazorPagesProject.Tests/Helpers/Utilities.cs?name=snippet1)]

## <a name="additional-resources"></a>Dodatkowe zasoby

* [Testy jednostkowe](/dotnet/articles/core/testing/unit-testing-with-dotnet-test)
* [Testy jednostkowe stron razor](xref:test/razor-pages-tests)
* [Oprogramowanie pośredniczące](xref:fundamentals/middleware/index)
* [Kontrolery testów](xref:mvc/controllers/testing)
