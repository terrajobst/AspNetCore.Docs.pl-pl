---
title: What's new in platformy ASP.NET Core 2.1
author: isaac2004
description: Dowiedz się więcej o nowych funkcjach w programie ASP.NET Core 2.1.
monikerRange: = aspnetcore-2.1
ms.author: riande
ms.custom: mvc
ms.date: 05/30/2018
uid: aspnetcore-2.1
ms.openlocfilehash: acbed75e2e894569816669e250795c95482bde2a
ms.sourcegitcommit: 5a2456cbf429069dc48aaa2823cde14100e4c438
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/22/2018
ms.locfileid: "41819475"
---
# <a name="whats-new-in-aspnet-core-21"></a>What's new in platformy ASP.NET Core 2.1

W tym artykule przedstawiono najbardziej znaczących zmian w programie ASP.NET Core 2.1 wraz z łączami do odpowiedniej dokumentacji.

## <a name="signalr"></a>SignalR

Ma został przepisany SignalR dla platformy ASP.NET Core 2.1. SignalR platformy ASP.NET Core zawiera szereg ulepszeń:

* Uproszczony model skalowalnego w poziomie.
* Nowy klient JavaScript za pomocą niezależne jQuery.
* Nowy compact binarne protokół oparte na MessagePack.
* Informacje dotyczące obsługi niestandardowych protokołów.
* Nowy model odpowiedzi przesyłania strumieniowego.
* Obsługa klientów zgodnie z ustawieniami systemu od zera funkcja WebSockets.

Aby uzyskać więcej informacji, zobacz [biblioteki SignalR platformy ASP.NET Core](xref:signalr/index).

## <a name="razor-class-libraries"></a>Biblioteki klas razor

ASP.NET Core 2.1 ułatwia tworzenie i obejmują interfejsu użytkownika opartego na Razor w bibliotece i udostępnić go w wielu projektach. Nowe umożliwia Razor SDK Kompilowanie plików Razor projekt biblioteki klas, które mogą być umieszczone w pakiet NuGet. Widoki stron w bibliotekach są automatycznie wykrywane i może być zastąpiona przez aplikację. Dzięki integracji kompilacji Razor do kompilacji:

* Czas uruchamiania aplikacji jest znacznie szybsze.
* Szybkie aktualizacje widokami Razor i stron w czasie wykonywania są nadal dostępne jako część przepływu pracy iteracyjne projektowanie.

Aby uzyskać więcej informacji, zobacz [tworzenia interfejsu użytkownika do wielokrotnego użytku, używając projektu biblioteki klas Razor](xref:razor-pages/ui-class).

## <a name="identity-ui-library--scaffolding"></a>Biblioteka interfejsów użytkownika tożsamości i tworzenia szkieletów

Platforma ASP.NET Core 2.1 udostępnia [tożsamości platformy ASP.NET Core](xref:security/authentication/identity) jako [biblioteki klas Razor](xref:razor-pages/ui-class). Aplikacje, które zawierają tożsamości można zastosować nowy generator szkieletu tożsamości, aby selektywnie Dodawanie kodu źródłowego, znajdujących się w bibliotece klas Razor tożsamości (RCL). Można wygenerować kod źródłowy, aby można było zmodyfikować kod i zmienić zachowanie. Na przykład można nakazać Generator szkieletu do generowania kodu, używane podczas rejestracji. Wygenerowany kod mają pierwszeństwo przed ten sam kod w RCL tożsamości.

Aplikacje, które wykonują **nie** obejmują uwierzytelniania można zastosować Generator szkieletu tożsamości, aby dodać pakiet RCL tożsamości. Masz możliwość wyboru tożsamości kodu do wygenerowania.

Aby uzyskać więcej informacji, zobacz [szkieletu tożsamość w projektach programu ASP.NET Core](xref:security/authentication/scaffold-identity).

## <a name="https"></a>HTTPS

Za pomocą lepiej skoncentrować się na zabezpieczeniach i prywatności ważne jest włączenie protokołu HTTPS dla aplikacji sieci web. Wymuszanie protokołu HTTPS staje się coraz bardziej rygorystyczne w sieci web. Lokacje, które nie używają protokołu HTTPS, są uważane za niebezpieczne. Przeglądarki (przeglądarki Chromium, Mozilla) coraz częściej wymuszania, że funkcje sieci web musi być używany z bezpiecznym kontekście. [RODO](xref:security/gdpr) wymaga użycia protokołu HTTPS, aby chronić prywatność użytkowników. Przy użyciu protokołu HTTPS w środowisku produkcyjnym jest krytyczna, przy użyciu protokołu HTTPS w rozwoju może zapobiec problemom we wdrożeniu (na przykład niezabezpieczone łącza). Platforma ASP.NET Core 2.1 zawiera szereg ulepszeń, które ułatwiają do używania protokołu HTTPS podczas tworzenia i konfigurowania protokołu HTTPS w środowisku produkcyjnym. Aby uzyskać więcej informacji, zobacz [Wymuszanie protokołu HTTPS](xref:security/enforcing-ssl).

### <a name="on-by-default"></a>Na domyślnie

W celu ułatwienia tworzenia bezpiecznej witryny sieci Web, HTTPS jest teraz włączony domyślnie. Począwszy od 2.1 Kestrel nasłuchuje na `https://localhost:5001` kiedy certyfikat rozwoju lokalnego znajduje się. Utworzono certyfikatu deweloperskiego:

* Jako część zestawu .NET Core SDK pierwszego uruchomienia komputera, gdy używasz zestawu SDK po raz pierwszy.
* Ręcznie przy użyciu nowego `dev-certs` narzędzia.

Uruchom `dotnet dev-certs https --trust` ufać certyfikatowi.

### <a name="https-redirection-and-enforcement"></a>Przekierowania protokołu HTTPS i wymuszania

Aplikacje internetowe zazwyczaj konieczne nasłuchiwania protokołów HTTP i HTTPS, ale następnie przekierowania całego ruchu HTTP na HTTPS. 2.1 wyspecjalizowanych pośredniczącym przekierowania protokołu HTTPS, inteligentnie przekierowującego na podstawie obecności konfiguracji lub powiązanych portów zostały wprowadzone.

Korzystanie z protokołu HTTPS można dodatkowo wymuszane, za pomocą [interfejsów HTTP Strict transportu zabezpieczeń protokołu (HSTS)](xref:security/enforcing-ssl#http-strict-transport-security-protocol-hsts). HSTS powoduje, że przeglądarek zawsze dostęp do witryny za pośrednictwem protokołu HTTPS. Platforma ASP.NET Core 2.1 dodaje oprogramowanie pośredniczące HSTS, który obsługuje opcje dla maksymalnego wieku, poddomen, i HSTS wstępnego ładowania listy.

### <a name="configuration-for-production"></a>Konfiguracja dla trybu produkcyjnego

W środowisku produkcyjnym należy jawnie skonfigurować protokół HTTPS. 2.1 domyślny schemat konfiguracji dotyczące konfigurowania protokołu HTTPS dla Kestrel został dodany. Aplikacje można skonfigurować do użycia:

* Wiele punktów końcowych, w tym adresy URL. Aby uzyskać więcej informacji, zobacz [implementacji serwera sieci web Kestrel: Konfiguracja punktu końcowego](xref:fundamentals/servers/kestrel#endpoint-configuration).
* Certyfikat używany do obsługi protokołu HTTPS z plikiem na dysku lub z magazynu certyfikatów.

## <a name="gdpr"></a>RODO

Platformy ASP.NET Core udostępnia interfejsów API i szablony, które ułatwiają korzystanie z niektórych [Unii Europejskiej ogólnego danych (GDPR Protection Regulation)](https://www.eugdpr.org/) wymagania. Aby uzyskać więcej informacji, zobacz [RODO obsługi w programie ASP.NET Core](xref:security/gdpr). A [przykładową aplikację](https://github.com/aspnet/Docs/tree/live/aspnetcore/security/gdpr/sample) pokazano, jak i umożliwia testowanie większość punkty rozszerzenia RODO i interfejsów API dodano szablony platformy ASP.NET Core 2.1.

## <a name="integration-tests"></a>Testy integracji

Nowy pakiet został wprowadzony przetestować upraszczają tworzenie i wykonywanie. [Microsoft.AspNetCore.Mvc.Testing](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Testing/) pakietu obsługuje następujące zadania:

* Kopiuje plik zależności (*\*.deps*) z przetestowanej aplikacji w projekcie testowym *bin* folderu.
* Ustawia zawartości głównego katalogu głównego projektu przetestowanej aplikacji tak, aby pliki statyczne i stron/widoki są dostępne, gdy testy są wykonywane.
* Udostępnia [WebApplicationFactory](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactory-1) klasy, aby usprawnić uruchamianie przetestowanej aplikacji za pomocą [elementu TestServer](/dotnet/api/microsoft.aspnetcore.testhost.testserver).

Następującego testu używa [xUnit](https://xunit.github.io/) do sprawdzenia, czy strony indeksu ładuje z pomyślny kod statusu i poprawny nagłówek Content-Type:

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

## <a name="apicontroller-actionresultt"></a>[Klasy ApiController], ActionResult\<T >

Platforma ASP.NET Core 2.1 dodaje nowe konwencje programowania, które ułatwiają tworzenie czyste i opisu interfejsów API sieci web. `ActionResult<T>` Nowy typ dodano na aplikację, aby zwracać typ odpowiedzi lub inny wynik akcji, (podobnie jak IActionResult), nadal wskazujące typ odpowiedzi. `[ApiController]` Atrybut został również dodany jako sposób korzystania z Konwencji specyficzne dla interfejsu API sieci Web i zachowania.

Aby uzyskać więcej informacji, zobacz [Tworzenie interfejsów API sieci Web za pomocą programu ASP.NET Core](xref:web-api/index).

## <a name="ihttpclientfactory"></a>IHttpClientFactory

Platforma ASP.NET Core 2.1 zawiera nową `IHttpClientFactory` usługa, która ułatwia konfigurowanie i używanie wystąpienia `HttpClient` w aplikacjach. `HttpClient` ma już pojęcie Delegowanie obsługi, które można połączyć ze sobą dla wychodzących żądań HTTP. Fabryka:

* Sprawia, że rejestrowanie wystąpień `HttpClient` na klienta o nazwie bardziej intuicyjne.
* Implementuje obsługi Polly, umożliwiająca Polly zasady służący do ponawiania, CircuitBreakers itp.

Aby uzyskać więcej informacji, zobacz [inicjowanie żądań HTTP](xref:fundamentals/http-requests).

## <a name="kestrel-transport-configuration"></a>Konfiguracja transportu kestrel

W wersji platformy ASP.NET Core 2.1 firmy Kestrel transportu domyślnego jest już oparte na Libuv, ale oparta na zarządzanych gniazda. Aby uzyskać więcej informacji, zobacz [implementacji serwera sieci web Kestrel: konfiguracja transportu](xref:fundamentals/servers/kestrel#transport-configuration).

## <a name="generic-host-builder"></a>Konstruktor ogólnego hosta

Ogólny konstruktora hosta (`HostBuilder`) została wprowadzona. Ten konstruktor może służyć do aplikacji, które nie przetwarzają żądania HTTP (Obsługa komunikatów, zadania w tle itp.).

Aby uzyskać więcej informacji, zobacz [hosta ogólnego .NET](xref:fundamentals/host/generic-host).

## <a name="updated-spa-templates"></a>Zaktualizowane szablony SPA

Szablony aplikacji jednostronicowej dla szablonów Angular, React i platformy React z kontenera Redux są aktualizowane, aby używać struktur standardowy projekt i tworzenie systemów dla każdej struktury.

Platformy Angular szablon jest oparty na interfejs wiersza polecenia usługi Angular i szablony platformy React są oparte na tworzenie platformy react aplikacji.
Aby uzyskać więcej informacji, zobacz [używania szablonów aplikacji jednostronicowej platformy ASP.NET Core](xref:spa/index).

## <a name="razor-pages-search-for-razor-assets"></a>Strony razor wyszukiwania zasobów Razor

2.1 stron Razor Wyszukaj Razor zasoby (na przykład układy i częściowych), w następujących katalogach w określonej kolejności:

1. Bieżący folder stron.
1. */Pages/udostępnione /*
1. */Views/udostępnione /*

## <a name="razor-pages-in-an-area"></a>Strony razor, w obszarze

Obsługują teraz stron razor [obszarów](xref:mvc/controllers/areas). Aby zobaczyć przykład obszarów, należy utworzyć nową aplikację sieci web dla stron Razor za pomocą indywidualnych kont użytkowników. Aplikacja internetowa ze stronami Razor za pomocą indywidualnych kont użytkowników zawiera */Areas/Identity/Pages*.

## <a name="mvc-compatibility-version"></a>Wersja zgodności MVC

<xref:Microsoft.Extensions.DependencyInjection.MvcCoreMvcBuilderExtensions.SetCompatibilityVersion*> Metody umożliwia aplikacji opcjonalnych lub zrezygnować z potencjalnie przełomowe zmiany zachowania wprowadzonych w programie ASP.NET Core MVC 2.1 lub nowszej.

Aby uzyskać więcej informacji, zobacz <xref:mvc/compatibility-version>.

## <a name="migrate-from-20-to-21"></a>Migrowanie z wersji 2.0 do wersji 2.1

Zobacz [migracji z programu ASP.NET Core 2.0 i 2.1](xref:migration/20_21).

## <a name="additional-information"></a>Dodatkowe informacje

Aby uzyskać pełną listę zmian, zobacz [platformy ASP.NET Core 2.1 — informacje o wersji](https://github.com/aspnet/Home/releases/tag/2.1.0).
