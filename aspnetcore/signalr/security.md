---
title: Zagadnienia dotyczące zabezpieczeń w usłudze ASP.NET Core sygnalizujący
author: bradygaster
description: Dowiedz się, jak używać uwierzytelniania i autoryzacji w usłudze ASP.NET Core sygnalizujący.
monikerRange: '>= aspnetcore-2.1'
ms.author: anurse
ms.custom: mvc
ms.date: 11/06/2018
uid: signalr/security
ms.openlocfilehash: a52db2ff51c55f7299d63aa3c7398f99727e0694
ms.sourcegitcommit: 387cf29f5d5addef2cbc70670a11d612806b36b2
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 09/06/2019
ms.locfileid: "70746558"
---
# <a name="security-considerations-in-aspnet-core-signalr"></a>Zagadnienia dotyczące zabezpieczeń w usłudze ASP.NET Core sygnalizujący

Według [Andrew Stanton-pielęgniarki](https://twitter.com/anurse)

Ten artykuł zawiera informacje dotyczące zabezpieczania sygnałów.

## <a name="cross-origin-resource-sharing"></a>Współużytkowanie zasobów między źródłami

[Współużytkowanie zasobów między źródłami (CORS)](https://www.w3.org/TR/cors/) może służyć do zezwalania na połączenia sygnałów między źródłami w przeglądarce. Jeśli kod JavaScript jest hostowany w innej domenie z aplikacji sygnalizującej, należy włączyć [oprogramowanie pośredniczące CORS](xref:security/cors) , aby umożliwić programowi JavaScript łączenie się z aplikacją sygnalizującej. Zezwalaj na żądania między źródłami tylko z domen, które ufają lub kontrolują. Na przykład:

* Twoja witryna jest hostowana`http://www.example.com`
* Twoja aplikacja sygnalizująca jest hostowana`http://signalr.example.com`

Funkcję CORS należy skonfigurować w aplikacji sygnalizującej tak, aby zezwalała na `www.example.com`źródło.

Aby uzyskać więcej informacji na temat konfigurowania mechanizmu CORS, zobacz [Włączanie żądań między źródłami (CORS)](xref:security/cors). Sygnalizujący **wymaga** następujących zasad CORS:

* Zezwalaj na określone oczekiwane źródła. Umożliwienie dowolnego pochodzenia jest możliwe, ale **nie** jest bezpieczne lub zalecane.
* Metody `GET` http i `POST` muszą być dozwolone.
* Poświadczenia muszą być włączone, nawet jeśli uwierzytelnianie nie jest używane.

Na przykład następujące zasady CORS umożliwiają klientowi przeglądarki sygnalizującemu na `https://example.com` dostęp do aplikacji sygnalizującej hostowanej w: `https://signalr.example.com`

::: moniker range=">= aspnetcore-3.0"

```csharp
public void Configure(IApplicationBuilder app, IHostingEnvironment env)
{
    // ... other middleware ...

    // Make sure the CORS middleware is ahead of SignalR.
    app.UseCors(builder =>
    {
        builder.WithOrigins("https://example.com")
            .AllowAnyHeader()
            .WithMethods("GET", "POST")
            .AllowCredentials();
    });

    // ... other middleware ...
    app.UseRouting();

    app.UseEndpoints(endpoints =>
    {
        endpoints.MapHub<ChatHub>("/chatHub");
    });

    // ... other middleware ...
}
```

::: moniker-end

::: moniker range="<= aspnetcore-2.2"

[!code-csharp[Main](security/sample/Startup.cs?name=snippet1)]

::: moniker-end

> [!NOTE]
> Program sygnalizujący nie jest zgodny z wbudowaną funkcją CORS w Azure App Service.

## <a name="websocket-origin-restriction"></a>Ograniczenie pochodzenia obiektu WebSocket

::: moniker range=">= aspnetcore-2.2"

Ochrona zapewniana przez mechanizm CORS nie ma zastosowania do obiektów WebSockets. W przypadku ograniczenia pochodzenia dla obiektów WebSockets Odczytaj [ograniczenie pochodzenia elementu WebSockets](xref:fundamentals/websockets#websocket-origin-restriction).

::: moniker-end

::: moniker range="< aspnetcore-2.2"

Ochrona zapewniana przez mechanizm CORS nie ma zastosowania do obiektów WebSockets. Przeglądarki **nie**:

* Wykonaj żądania funkcji CORS przed inspekcją.
* Przestrzeganie ograniczeń określonych w `Access-Control` nagłówkach podczas wykonywania żądań WebSocket.

Jednak przeglądarki wysyłają `Origin` nagłówek podczas wystawiania żądań protokołu WebSocket. Aplikacje powinny być skonfigurowane do sprawdzania poprawności tych nagłówków, aby upewnić się, że dozwolone są tylko usługi WebSockets pochodzące z oczekiwanych źródeł.

W ASP.NET Core 2,1 i nowszych walidacji nagłówka można osiągnąć przy użyciu niestandardowego oprogramowania pośredniczącego umieszczonego **przed `UseSignalR`i uwierzytelniania oprogramowania pośredniczącego** w programie `Configure`:

[!code-csharp[Main](security/sample/Startup.cs?name=snippet2)]

> [!NOTE]
> Nagłówek jest kontrolowany przez klienta i, `Referer` podobnie jak nagłówek, może być sfałszowany. `Origin` Tych nagłówków **nie** należy używać jako mechanizmu uwierzytelniania.

::: moniker-end

## <a name="access-token-logging"></a>Rejestrowanie tokenu dostępu

W przypadku korzystania z usługi WebSockets lub zdarzeń wysyłanych przez serwer klient przeglądarki wysyła token dostępu w ciągu zapytania. Uzyskiwanie tokenu dostępu za pośrednictwem ciągu zapytania jest ogólnie bezpieczne, jak przy `Authorization` użyciu standardowego nagłówka. Należy zawsze używać protokołu HTTPS, aby zapewnić bezpieczne połączenie między klientem a serwerem. Wiele serwerów sieci Web rejestruje adres URL dla każdego żądania, w tym ciąg zapytania. Rejestrowanie adresów URL może rejestrować token dostępu. ASP.NET Core domyślnie rejestruje adres URL dla każdego żądania, który będzie zawierać ciąg zapytania. Na przykład:

```
info: Microsoft.AspNetCore.Hosting.Internal.WebHost[1]
      Request starting HTTP/1.1 GET http://localhost:5000/myhub?access_token=1234
```

Jeśli masz problemy z rejestrowaniem tych danych w dziennikach serwera, możesz wyłączyć to rejestrowanie całkowicie przez skonfigurowanie `Microsoft.AspNetCore.Hosting` rejestratora `Warning` na poziomie lub wyższym (te komunikaty są zapisywane na `Info` poziomie). Aby uzyskać więcej informacji, zobacz dokumentację dotyczącą [filtrowania dzienników](xref:fundamentals/logging/index#log-filtering) . Jeśli nadal chcesz rejestrować pewne informacje o żądaniu, możesz [napisać oprogramowanie pośredniczące](xref:fundamentals/middleware/write) w celu zarejestrowania wymaganych danych i odfiltrować `access_token` wartość ciągu zapytania (jeśli istnieje).

## <a name="exceptions"></a>Wyjątki

Komunikaty o wyjątkach są zwykle uznawane za dane poufne, które nie powinny być ujawnione dla klienta. Domyślnie sygnalizujący nie wysyła szczegółów wyjątku zgłoszonego przez metodę centrum do klienta. Zamiast tego klient otrzymuje komunikat ogólny informujący o wystąpieniu błędu. Dostarczenie komunikatu o wyjątku do klienta może zostać zastąpione (na przykład w przypadku programowania lub [`EnableDetailedErrors`](xref:signalr/configuration#configure-server-options)testowania) za pomocą. Komunikaty o wyjątkach nie powinny być udostępniane klientowi w aplikacjach produkcyjnych.

## <a name="buffer-management"></a>Zarządzanie buforem

Sygnalizujący używa buforów dla poszczególnych połączeń do zarządzania wiadomościami przychodzącymi i wychodzącymi. Domyślnie program sygnalizujący ogranicza te bufory do 32 KB. Największym komunikatem, który klient lub serwer może wysłać, to 32 KB. Maksymalna ilość pamięci zużywanej przez połączenie dla komunikatów to 32 KB. Jeśli komunikaty są zawsze mniejsze niż 32 KB, można zmniejszyć limit, który:

* Uniemożliwia klientowi wysyłanie większej wiadomości.
* Serwer nigdy nie będzie musiał przydzielić dużych buforów, aby akceptować komunikaty.

Jeśli rozmiar komunikatów przekracza 32 KB, można zwiększyć limit. Zwiększenie tego limitu oznacza:

* Klient może spowodować, że serwer przydzieli bufory dużych pamięci.
* Alokacja serwerów dużych buforów może obniżyć liczbę jednoczesnych połączeń.

Istnieją limity dla wiadomości przychodzących i wychodzących, obie można skonfigurować na [`HttpConnectionDispatcherOptions`](xref:signalr/configuration#configure-server-options) obiekcie skonfigurowanym w: `MapHub`

* `ApplicationMaxBufferSize`reprezentuje maksymalną liczbę bajtów od klienta, które buforuje serwer. Jeśli klient próbuje wysłać komunikat przekraczający ten limit, połączenie może być zamknięte.
* `TransportMaxBufferSize`reprezentuje maksymalną liczbę bajtów, które może wysłać serwer. Jeśli serwer próbuje wysłać komunikat (uwzględniając wartości zwracane z metod centralnych) większy niż ten limit, zostanie zgłoszony wyjątek.

Ustawienie limitu `0` powoduje wyłączenie limitu. Usunięcie limitu umożliwia klientowi wysłanie komunikatu o dowolnym rozmiarze. Złośliwi klienci wysyłający duże wiadomości mogą spowodować przydzielenie nadmiernej ilości pamięci. Nadmierne użycie pamięci może znacznie zmniejszyć liczbę jednoczesnych połączeń.
