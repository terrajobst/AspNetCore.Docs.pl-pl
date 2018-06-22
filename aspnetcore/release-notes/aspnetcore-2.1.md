---
title: What's new in ASP.NET Core 2.1
author: isaac2004
description: Dowiedz się więcej na temat nowych funkcji programu ASP.NET Core 2.1.
manager: wpickett
monikerRange: = aspnetcore-2.1
ms.custom: mvc
ms.date: 05/30/2018
uid: aspnetcore-2.1
ms.openlocfilehash: 98e867ec77a79102bc536fe5580c8796cf142feb
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/20/2018
ms.locfileid: "36291652"
---
# <a name="whats-new-in-aspnet-core-21"></a>What's new in ASP.NET Core 2.1

W tym artykule omówiono najbardziej znaczących zmian w ASP.NET Core 2.1, wraz z łączami do odpowiedniej dokumentacji.

## <a name="signalr"></a>SignalR

Dla platformy ASP.NET Core 2.1 została ponownie napisana SignalR. SignalR platformy ASP.NET Core zawiera wiele ulepszeń:

* Uproszczony model skalowalnego w poziomie.
* Nowy klient JavaScript z zależności nie jQuery.
* Nowy compact binarne protokół oparte na MessagePack.
* Obsługa protokołów niestandardowych.
* Nowy model odpowiedzi przesyłania strumieniowego.
* Obsługa klientów zgodnie z Websocket systemu od zera.

Aby uzyskać więcej informacji, zobacz [ASP.NET Core SignalR](xref:signalr/index).

## <a name="razor-class-libraries"></a>Biblioteki klas razor

Platformy ASP.NET Core 2.1 ułatwia kompilacji i umieścić Razor na podstawie interfejsu użytkownika w bibliotece i udostępnić go wielu projektach. Nowe umożliwia Razor SDK tworzenia plików Razor do projektu biblioteki klas, które mogą być umieszczone w pakietu NuGet. Widoki i stron w bibliotekach są odnajdowane automatycznie i może zostać zastąpiona przez aplikację. Przez włączenie Razor kompilacji do kompilacji:

* Czas uruchamiania aplikacji jest znacznie szybsze.
* Szybkie aktualizacje widokami Razor i stron w czasie wykonywania są nadal dostępne w ramach przepływu pracy programowania iteracji.

Aby uzyskać więcej informacji, zobacz [tworzyć wielokrotnego użytku interfejsu użytkownika przy użyciu projektu biblioteki klas Razor](xref:razor-pages/ui-class).

## <a name="identity-ui-library--scaffolding"></a>Biblioteka interfejsów użytkownika tożsamości & szkieletów

Platformy ASP.NET Core 2.1 udostępnia [ASP.NET Core Identity](xref:security/authentication/identity) jako [biblioteki klas Razor](xref:razor-pages/ui-class). Aplikacje, które obejmują tożsamości można stosować nowe tworzenia szkieletu tożsamości można selektywnie dodać kod źródłowy zawartych w bibliotece klasy Razor tożsamości (RCL). Można wygenerować kodu źródłowego, aby można było zmodyfikować kod i zmiany zachowania. Na przykład można nakazać tworzenia szkieletu, aby wygenerować kod używany w rejestracji. Wygenerowany kod mają pierwszeństwo przed ten sam kod w RCL tożsamości.

Aplikacje, które wykonują **nie** obejmują uwierzytelniania można zastosować tworzenia szkieletu tożsamości, aby dodać pakiet RCL tożsamości. Istnieje możliwość wybrania tożsamości kod zostanie wygenerowany.

Aby uzyskać więcej informacji, zobacz [szkieletu tożsamości w projektach platformy ASP.NET Core](xref:security/authentication/scaffold-identity).

## <a name="https"></a>HTTPS

Zwiększona fokus na bezpieczeństwo i ochrona prywatności ważne jest włączenie protokołu HTTPS dla aplikacji sieci web. Wymuszanie protokołu HTTPS staje się coraz bardziej ściśle w sieci web. Lokacje, które nie używają protokołu HTTPS są uważane za niebezpieczne. Przeglądarki (chromu, Mozilla) coraz wymuszania, że funkcje sieci web musi być używany z bezpiecznym kontekście. [GDPR](xref:security/gdpr) wymaga użycia protokołu HTTPS, aby chronić prywatność użytkowników. Gdy w środowisku produkcyjnym przy użyciu protokołu HTTPS jest szczególnie ważne, przy użyciu protokołu HTTPS do rozwoju mogą pomagać w zapobieganiu problemy we wdrożeniu (na przykład niezabezpieczonych łącza). Platformy ASP.NET Core 2.1 zawiera wiele ulepszeń, które ułatwiają do używania protokołu HTTPS do rozwoju i konfigurowania protokołu HTTPS w środowisku produkcyjnym. Aby uzyskać więcej informacji, zobacz [wymusić HTTPS](xref:security/enforcing-ssl).

### <a name="on-by-default"></a>Na domyślnie

Aby ułatwić projektowanie bezpiecznej witryny internetowej, HTTPS teraz jest domyślnie włączona. Począwszy od 2.1 Kestrel nasłuchuje na `https://localhost:5001` podczas rozwoju lokalnego certyfikat znajduje się. Utworzono certyfikatu deweloperskiego:

* Jako część zestawu SDK programu .NET Core pierwszego uruchomienia komputera, gdy używasz zestawu SDK po raz pierwszy.
* Ręcznie przy użyciu nowego `dev-certs` narzędzia.

Uruchom `dotnet dev-certs https --trust` ufać certyfikatowi.

### <a name="https-redirection-and-enforcement"></a>Przekierowania protokołu HTTPS i wymuszania

Aplikacje sieci Web muszą zwykle nasłuchiwania protokołów HTTP i HTTPS, ale następnie przekierować cały ruch HTTP, HTTPS. W 2.1 specjalne pośredniczącym przekierowania HTTPS inteligentnie przekierowuje na podstawie obecności konfiguracji lub powiązane portów zostały wprowadzone.

Korzystanie z protokołu HTTPS można dodatkowo wymuszane, za pomocą [interfejsów HTTP Strict transportu zabezpieczeń protokołu (HSTS)](xref:security/enforcing-ssl#http-strict-transport-security-protocol-hsts). HSTS nakazuje przeglądarki zawsze dostęp do witryny za pośrednictwem protokołu HTTPS. Platformy ASP.NET Core 2.1 dodaje oprogramowanie pośredniczące HSTS, obsługująca opcje maksymalny wiek, podrzędne, a HSTS wstępnego ładowania listy.

### <a name="configuration-for-production"></a>Konfiguracja do produkcji

W środowisku produkcyjnym należy jawnie skonfigurować HTTPS. 2.1 domyślny schemat konfiguracji związane z konfigurowaniem protokołu HTTPS dla Kestrel został dodany. Aplikacje mogą być skonfigurowana do używania:

* Wiele punktów końcowych, w tym adresy URL. Aby uzyskać więcej informacji, zobacz [implementacją serwera sieci web Kestrel: Konfiguracja punktu końcowego](xref:fundamentals/servers/kestrel#endpoint-configuration).
* Certyfikat do użycia na potrzeby protokołu HTTPS z pliku na dysku lub z magazynu certyfikatów.

## <a name="gdpr"></a>GDPR

Platformy ASP.NET Core udostępnia interfejsy API i szablony, które ułatwiają korzystanie z niektórych [interfejsów UE ogólne dane ochrony rozporządzenia (GDPR)](https://www.eugdpr.org/) wymagania. Aby uzyskać więcej informacji, zobacz [GDPR obsługuje w ASP.NET Core](xref:security/gdpr). A [Przykładowa aplikacja](https://github.com/aspnet/Docs/tree/live/aspnetcore/security/gdpr/sample) przedstawia sposób użycia i umożliwia testowanie większość punktów rozszerzenia GDPR i dodane do szablonów platformy ASP.NET Core 2.1 interfejsów API.

## <a name="integration-tests"></a>Testy integracji

Wprowadzono nowy pakiet czy usprawnia przetestować tworzenie i wykonywanie. [Microsoft.AspNetCore.Mvc.Testing](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Testing/) pakietu obsługuje następujące zadania:

* Kopiuje plik zależności (*\*.deps*) z przetestowanej aplikacji do projektu testowego *bin* folderu.
* Ustawia zawartości katalogu głównym projektu przetestowanej aplikacji tak, aby pliki statyczne i stron/widoki są dostępne, gdy testy są wykonywane.
* Udostępnia [WebApplicationFactory](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactory-1) klasę, aby usprawnić uruchamianie przetestowanej aplikacji z [elementu TestServer](/dotnet/api/microsoft.aspnetcore.testhost.testserver).

Następującego testu używa [xUnit](https://xunit.github.io/) do sprawdzenia, czy strona indeksu ładuje z kodem stanu powodzenia i poprawne nagłówka Content-Type:

```csharp
public class BasicTests
    : IClassFixture<WebApplicationFactory<RazorPagesProject.Startup>>
{
    private readonly HttpClient _client;

    public BasicTests(WebApplicationFactory<RazorPagesProject.Startup> factory)
    {
        _client = factory.CreateClient();
    }

    [Fact]
    public async Task GetHomePage()
    {
        // Act
        var response = await _client.GetAsync("/");

        // Assert
        response.EnsureSuccessStatusCode(); // Status Code 200-299
        Assert.Equal("text/html; charset=utf-8",
            response.Content.Headers.ContentType.ToString());
    }
}
```

Aby uzyskać więcej informacji, zobacz [testy integracji](xref:test/integration-tests) tematu.

## <a name="apicontroller-actionresult"></a>[Klasy ApiController], ActionResult

Platformy ASP.NET Core 2.1 dodaje nowe konwencje programowania, które ułatwiają tworzenie czystą i opisowy interfejsów API sieci web. `ActionResult<T>` Nowy typ jest dodane do Zezwalaj aplikacji na zwracany typ odpowiedzi lub innych wynik akcji, (podobnie jak IActionResult), nadal wskazujące typ odpowiedzi. `[ApiController]` Atrybut dodano także sposób korzystania z Konwencji danego interfejsu API sieci Web i zachowania.

Aby uzyskać więcej informacji, zobacz [Tworzenie interfejsów API sieci Web z platformy ASP.NET Core](xref:web-api/index).

## <a name="ihttpclientfactory"></a>IHttpClientFactory

Platformy ASP.NET Core 2.1 zawiera nową `IHttpClientFactory` usługi, który ułatwia konfigurowanie i używanie wystąpienia `HttpClient` w aplikacjach. `HttpClient` jest już pojęcie Delegowanie obsługi, które można połączyć ze sobą dla wychodzących żądań HTTP. Fabryka:

* Umożliwia rejestrowanie wystąpień `HttpClient` na bardziej intuicyjne nazwanego klienta.
* Implementuje obsługi Polly, która umożliwia Polly zasady do zastosowania w przypadku ponownych prób, CircuitBreakers itp.

Aby uzyskać więcej informacji, zobacz [inicjowania żądań HTTP](xref:fundamentals/http-requests).

## <a name="kestrel-transport-configuration"></a>Konfiguracja transportu kestrel

Od wersji platformy ASP.NET Core 2.1 Kestrel przez domyślny transport jest już oparta na Libuv, ale zamiast tego oparty na zarządzanych gniazda. Aby uzyskać więcej informacji, zobacz [implementacją serwera sieci web Kestrel: konfiguracja transportu](xref:fundamentals/servers/kestrel#transport-configuration).

## <a name="generic-host-builder"></a>Konstruktor rodzajowego hosta

Ogólny konstruktora hosta (`HostBuilder`) została wprowadzona. Ten konstruktor może służyć do aplikacji, które nie przetwarzają żądań HTTP (wiadomości, zadania w tle itp.).

Aby uzyskać więcej informacji, zobacz [.NET rodzajowego hosta](xref:fundamentals/host/generic-host).

## <a name="updated-spa-templates"></a>Zaktualizowane szablony SPA

Szablony jednej strony aplikacji dla kątową, platformy React, i platformy React z Redux zostaną zaktualizowane, aby używać struktur standardowe projektu i stworzyć systemy dla każdej platformy.

Dyrektywy Angular szablon jest oparty na kątowego interfejsu wiersza polecenia i szablonów platformy React są oparte na tworzenie platformy react aplikacji.
Aby uzyskać więcej informacji, zobacz [szablony jednej strony aplikacji za pomocą platformy ASP.NET Core](xref:spa/index).

## <a name="razor-pages-search-for-razor-assets"></a>Wyszukiwanie stron razor zasoby Razor

2.1 stron Razor Wyszukaj Razor zasoby (na przykład układy i częściowe) w następujących katalogów w określonej kolejności:

1. Bieżący folder strony.
1. */Pages/udostępnione /*
1. */Views/udostępnione /*

## <a name="razor-pages-in-an-area"></a>Stron razor w obszarze

Obsługa teraz stron razor [obszarów](xref:mvc/controllers/areas). Aby zapoznać się z przykładem obszarów, Utwórz nową aplikację sieci web Razor strony z indywidualnych kont użytkowników. Obejmuje stron Razor aplikacji sieci web z indywidualne konta użytkowników */Areas/Identity/Pages*.

## <a name="migrate-from-20-to-21"></a>Migracja z 2.0 do 2.1

Zobacz [migracji z platformy ASP.NET Core 2.0 lub 2.1](xref:migration/20_21).

## <a name="additional-information"></a>Dodatkowe informacje

Aby uzyskać pełną listę zmian, zobacz [2.1 informacje o wersji platformy ASP.NET Core](https://github.com/aspnet/Home/releases/tag/2.1.0).
