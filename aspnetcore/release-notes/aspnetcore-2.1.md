---
title: Co nowego w programie ASP.NET Core 2.1
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
# <a name="whats-new-in-aspnet-core-21"></a>Co nowego w programie ASP.NET Core 2.1

W tym artykule przedstawiono najbardziej znaczące zmiany w programie ASP.NET Core 2.1 wraz z łączami do odpowiedniej dokumentacji.

## <a name="signalr"></a>SignalR

SignalR został przepisany dla platformy ASP.NET Core 2.1. SignalR dla ASP.NET Core zawiera szereg ulepszeń:

* Uproszczony model skalowania w poziomie.
* Nowy klient JavaScript bez zależności od jQuery.
* Nowy kompaktowy protokół binarny oparty na MessagePack.
* Informacje dotyczące obsługi niestandardowych protokołów.
* Nowy model odpowiedzi przesyłania strumieniowego.
* Obsługa klientów opartych bezpośrednio na WebSockets.

Aby uzyskać więcej informacji, zobacz [biblioteki SignalR platformy ASP.NET Core](xref:signalr/index).

## <a name="razor-class-libraries"></a>Biblioteki klas Razor

ASP.NET Core 2.1 ułatwia tworzenie i dołączanie interfejsu użytkownika opartego na składni Razor w bibliotece i udostępnianie go w wielu projektach. Nowy zestaw SDK Razor umożliwia kompilowanie plików Razor do projektów bibliotek klas, które mogą być umieszczone w pakietach NuGet. Widoki i strony w bibliotekach są automatycznie wykrywane i mogą być nadpisane w aplikacji. Dzięki integracji kompilacji Razor w kompilacji:

* Czas uruchamiania aplikacji jest znacznie krótszy.
* Szybkie aktualizacje widoków i stron Razor w czasie wykonywania są nadal dostępne jako część przepływu pracy iteracyjnego programowania.

Aby uzyskać więcej informacji, zobacz [tworzenia interfejsu użytkownika do wielokrotnego użytku, używając projektu biblioteki klas Razor](xref:razor-pages/ui-class).

## <a name="identity-ui-library--scaffolding"></a>Biblioteka interfejsów użytkownika tożsamości i tworzenia szkieletów

Platforma ASP.NET Core 2.1 udostępnia mechanizm [tożsamości platformy ASP.NET Core](xref:security/authentication/identity) jako [bibliotekę klas Razor](xref:razor-pages/ui-class). Aplikacje, które używają mechanizmu tożsamości, mogą zastosować nowy generator szkieletu tożsamości, aby wybiórczo dodawać kod źródłowy, znajdujący się w bibliotece klas Razor tożsamości (RCL). Przydatne może być wygenerowanie kodu źródłowego, aby można było zmodyfikować kod i zmienić zachowanie. Na przykład można nakazać Generatorowi szkieletu wygenerowanie kodu używanego podczas rejestracji. Wygenerowany kod ma pierwszeństwo przed tym samym kodem w bibliotece RCL tożsamości.

Aplikacje, które **nie** obejmują uwierzytelniania, mogą zastosować Generator szkieletu tożsamości, aby dodać pakiet RCL tożsamości. Masz możliwość wyboru kodu tożsamości do wygenerowania.

Aby uzyskać więcej informacji, zobacz [szkieletu tożsamość w projektach programu ASP.NET Core](xref:security/authentication/scaffold-identity).

## <a name="https"></a>HTTPS

Aby lepiej skoncentrować się na zabezpieczeniach i prywatności, ważne jest włączenie protokołu HTTPS dla aplikacji sieci Web. Wymuszanie protokołu HTTPS staje się coraz bardziej rygorystyczne w sieci Web. Lokacje, które nie używają protokołu HTTPS, są uważane za niebezpieczne. Przeglądarki (Chromium, Mozilla) coraz częściej wymuszają, aby funkcje sieci Web były używane w bezpiecznym kontekście. [RODO](xref:security/gdpr) wymaga użycia protokołu HTTPS, aby chronić prywatność użytkowników. Użycie protokołu HTTPS w środowisku produkcyjnym jest krytyczne, natomiast w środowisku programistycznym może zapobiec problemom we wdrożeniu (takim jak niezabezpieczone linki). Platforma ASP.NET Core 2.1 zawiera szereg ulepszeń, które ułatwiają wykorzystanie protokołu HTTPS podczas programowania i konfigurowanie protokołu HTTPS w środowisku produkcyjnym. Aby uzyskać więcej informacji, zobacz [Wymuszanie protokołu HTTPS](xref:security/enforcing-ssl).

### <a name="on-by-default"></a>Włączony domyślnie

W celu ułatwienia tworzenia bezpiecznej witryny sieci Web protokół HTTPS jest teraz włączony domyślnie. Począwszy od wersji 2.1 serwer Kestrel nasłuchuje na porcie `https://localhost:5001`, kiedy lokalny certyfikat programistyczny jest obecny. Certyfikat programistyczny jest tworzony:

* Jako część zestawu .NET Core SDK pierwszego uruchomienia komputera, gdy używasz zestawu SDK po raz pierwszy.
* Ręcznie przy użyciu nowego narzędzia `dev-certs`.

Uruchom polecenie `dotnet dev-certs https --trust`, aby zaufać certyfikatowi.

### <a name="https-redirection-and-enforcement"></a>Przekierowania i wymuszenia protokołu HTTPS

Aplikacje internetowe zazwyczaj wymagają nasłuchiwania protokołów HTTP i HTTPS, ale następnie przekierowują cały ruchu HTTP na HTTPS. W wersji 2.1 zostało wprowadzone wyspecjalizowane oprogramowanie pośredniczące przekierowania protokołu HTTPS inteligentnie przekierowujące na podstawie obecności konfiguracji lub powiązanych portów serwera.

Korzystanie z protokołu HTTPS może być dodatkowo wymuszane za pomocą [protokołu HTTP Strict Transport Security (HSTS)](xref:security/enforcing-ssl#http-strict-transport-security-protocol-hsts). HSTS powoduje, że przeglądarka zawsze uzyskuje dostęp do witryny za pośrednictwem protokołu HTTPS. Platforma ASP.NET Core 2.1 dodaje oprogramowanie pośredniczące HSTS, które obsługuje opcje dla maksymalnego wieku, poddomen i listy wstępnego ładowania HSTS.

### <a name="configuration-for-production"></a>Konfiguracja dla trybu produkcyjnego

W środowisku produkcyjnym należy jawnie skonfigurować protokół HTTPS. W wersji 2.1 został dodany domyślny schemat konfiguracji dotyczący konfigurowania protokołu HTTPS dla serwera Kestrel. Aplikacje można skonfigurować do użycia:

* Wielu punktów końcowych, w tym adresów URL. Aby uzyskać więcej informacji, zobacz [Implementacja serwera sieci web Kestrel: Konfiguracja punktu końcowego](xref:fundamentals/servers/kestrel#endpoint-configuration).
* Certyfikatu używanego do obsługi protokołu HTTPS z pliku na dysku lub z magazynu certyfikatów.

## <a name="gdpr"></a>RODO

Platforma ASP.NET Core udostępnia interfejsy API i szablony, które ułatwiają spełnienie niektórych wymagań [Ogólnego rozporządzenia o ochronie danych (RODO)](https://www.eugdpr.org/). Aby uzyskać więcej informacji, zobacz [Obsługa RODO w programie ASP.NET Core](xref:security/gdpr). W [przykładowej aplikacji](https://github.com/aspnet/Docs/tree/live/aspnetcore/security/gdpr/sample) pokazano sposób użycia umożliwiono przetestowanie większości punktów rozszerzenia i interfejsów API związanych z RODO, które dodano w szablonach ASP.NET Core 2.1.

## <a name="integration-tests"></a>Testy integracyjne

Został wprowadzony nowy pakiet upraszczający tworzenie i wykonywanie testów. Pakiet [Microsoft.AspNetCore.Mvc.Testing](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Testing/) obsługuje następujące zadania:

* Kopiuje plik zależności (*\*.deps*) z przetestowanej aplikacji w projekcie testowym *bin* folderu.
* Ustawia katalog główny zawartości na katalog główny projektu testowanej aplikacji, aby można było znaleźć pliki statyczne i strony/widoki podczas wykonywania testów.
* Udostępnia klasę [WebApplicationFactory](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactory-1), aby usprawnić uruchamianie testowanej aplikacji za pomocą [elementu TestServer](/dotnet/api/microsoft.aspnetcore.testhost.testserver).

Następujący test używa narzędzia [xUnit](https://xunit.github.io/) do sprawdzenia, czy strona indeksu ładuje się z pomyślnym kodem statusu i poprawnym nagłówkiem Content-Type:

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

Platforma ASP.NET Core 2.1 dodaje nowe konwencje programowania, które ułatwiają tworzenie czystych i bardziej opisowych interfejsów API sieci Web. Dodano nowy typ `ActionResult<T>`, aby zwracać albo typ odpowiedzi albo inny wynik akcji (podobnie jak w przypadku IActionResult), ale nadal wskazywać typ odpowiedzi. Atrybut `[ApiController]` został również dodany jako sposób korzystania z konwencji i zachowań specyficznych dla interfejsu API sieci Web.

Aby uzyskać więcej informacji, zobacz [Tworzenie interfejsów API sieci Web za pomocą programu ASP.NET Core](xref:web-api/index).

## <a name="ihttpclientfactory"></a>IHttpClientFactory

Platforma ASP.NET Core 2.1 zawiera nową usługę `IHttpClientFactory`, która ułatwia konfigurowanie i używanie wystąpień `HttpClient` w aplikacjach. Klasa `HttpClient` ma już pojęcie delegowania programów obsługi, które można połączyć ze sobą dla wychodzących żądań HTTP. Fabryka:

* Sprawia, że rejestrowanie wystąpień `HttpClient` na klienta o nazwie bardziej intuicyjne.
* Implementuje obsługi Polly, umożliwiająca Polly zasady służący do ponawiania, CircuitBreakers itp.

Aby uzyskać więcej informacji, zobacz [inicjowanie żądań HTTP](xref:fundamentals/http-requests).

## <a name="kestrel-transport-configuration"></a>Konfiguracja transportu Kestrel

W wersji platformy ASP.NET Core 2.1 transport domyślny serwera Kestrel nie jest już oparty na bibliotece libuv, ale na zarządzanych gniazdach. Aby uzyskać więcej informacji, zobacz [Implementacja serwera sieci web Kestrel: konfiguracja transportu](xref:fundamentals/servers/kestrel#transport-configuration).

## <a name="generic-host-builder"></a>Ogólny konstruktor hosta

Został wprowadzony ogólny konstruktor hosta (`HostBuilder`). Ten konstruktor może służyć do aplikacji, które nie przetwarzają żądań HTTP (Obsługa komunikatów, zadania w tle itp.).

Aby uzyskać więcej informacji, zobacz [Host ogólny .NET](xref:fundamentals/host/generic-host).

## <a name="updated-spa-templates"></a>Zaktualizowane szablony SPA

Szablony aplikacji jednostronicowej dla platform Angular, React i React z Redux zostały zaktualizowane, aby używać standardowych struktur projektów i systemów budowania dla każdej z platform.

Platformy Angular szablon jest oparty na interfejs wiersza polecenia usługi Angular i szablony platformy React są oparte na tworzenie platformy react aplikacji.
Aby uzyskać więcej informacji, zobacz [używania szablonów aplikacji jednostronicowej platformy ASP.NET Core](xref:spa/index).

## <a name="razor-pages-search-for-razor-assets"></a>Wyszukiwanie zasobów Razor w stronach Razor

W wersji 2.1 w programie Razor Pages wyszukiwanie zasobów Razor (na przykład układów i widoków częściowych) odbywa się w następujących katalogach w podanej kolejności:

1. Bieżący folder stron.
1. */Pages/Shared/*
1. */Views/Shared /*

## <a name="razor-pages-in-an-area"></a>Strony razor, w obszarze

Narzędzie Razor Pages obsługuje teraz [obszary](xref:mvc/controllers/areas). Aby zobaczyć przykład wykorzystania obszarów, należy utworzyć nową aplikację internetową Razor Pages z indywidualnymi kontami użytkowników. Aplikacja internetowa Razor Pages z indywidualnymi kontami użytkowników zawiera element */Areas/Identity/Pages*.

## <a name="mvc-compatibility-version"></a>Wersja zgodności MVC

Metoda <xref:Microsoft.Extensions.DependencyInjection.MvcCoreMvcBuilderExtensions.SetCompatibilityVersion*> umożliwia aplikacji włączenie wykorzystania lub rezygnację ze zmian zachowania wprowadzanych w programie ASP.NET Core MVC w wersji 2.1 lub nowszej, które potencjalnie mogą prowadzić do awarii.

Aby uzyskać więcej informacji, zobacz <xref:mvc/compatibility-version>.

## <a name="migrate-from-20-to-21"></a>Migracja z wersji 2.0 do wersji 2.1

Zobacz [migracji z programu ASP.NET Core 2.0 i 2.1](xref:migration/20_21).

## <a name="additional-information"></a>Dodatkowe informacje

Aby uzyskać pełną listę zmian, zobacz [platformy ASP.NET Core 2.1 — informacje o wersji](https://github.com/aspnet/Home/releases/tag/2.1.0).
