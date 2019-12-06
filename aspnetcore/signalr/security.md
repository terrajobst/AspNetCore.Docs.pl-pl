---
title: Zagadnienia dotyczące zabezpieczeń w ASP.NET Core SignalR
author: bradygaster
description: Dowiedz się, jak używać uwierzytelniania i autoryzacji w ASP.NET Core SignalR.
monikerRange: '>= aspnetcore-2.1'
ms.author: anurse
ms.custom: mvc
ms.date: 12/05/2019
no-loc:
- SignalR
uid: signalr/security
ms.openlocfilehash: f443fe0fbaaa1facd09edc0878c048772895ecff
ms.sourcegitcommit: c0b72b344dadea835b0e7943c52463f13ab98dd1
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 12/06/2019
ms.locfileid: "74881182"
---
# <a name="security-considerations-in-aspnet-core-opno-locsignalr"></a>Zagadnienia dotyczące zabezpieczeń w ASP.NET Core SignalR

Według [Andrew Stanton-pielęgniarki](https://twitter.com/anurse)

Ten artykuł zawiera informacje dotyczące zabezpieczania SignalR.

## <a name="cross-origin-resource-sharing"></a>Współużytkowanie zasobów między źródłami

[Współużytkowanie zasobów między źródłami (CORS)](https://www.w3.org/TR/cors/) może służyć do zezwalania na połączenia SignalR między źródłami w przeglądarce. Jeśli kod JavaScript jest hostowany w innej domenie z aplikacji SignalR, należy włączyć [oprogramowanie](xref:security/cors) do obsługi mechanizmu CORS, aby umożliwić programowi JavaScript łączenie się z aplikacją SignalR. Zezwalaj na żądania między źródłami tylko z domen, które ufają lub kontrolują. Na przykład:

* Twoja witryna jest hostowana w `http://www.example.com`
* Aplikacja SignalR jest hostowana na `http://signalr.example.com`

Funkcję CORS należy skonfigurować w aplikacji SignalR tak, aby zezwalała na `www.example.com`źródła.

Aby uzyskać więcej informacji na temat konfigurowania mechanizmu CORS, zobacz [Włączanie żądań między źródłami (CORS)](xref:security/cors). SignalR **wymaga** następujących zasad CORS:

* Zezwalaj na określone oczekiwane źródła. Umożliwienie dowolnego pochodzenia jest możliwe, ale **nie** jest bezpieczne lub zalecane.
* Metody HTTP `GET` i `POST` muszą być dozwolone.
* Poświadczenia muszą być włączone, nawet jeśli uwierzytelnianie nie jest używane.

Na przykład następujące zasady CORS umożliwiają klientowi SignalR przeglądarki hostowanemu na `https://example.com` dostęp do aplikacji SignalR hostowanej w `https://signalr.example.com`:

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
> SignalR nie jest zgodna z wbudowaną funkcją CORS w programie Azure App Service.

## <a name="websocket-origin-restriction"></a>Ograniczenie pochodzenia obiektu WebSocket

::: moniker range=">= aspnetcore-2.2"

Ochrona zapewniana przez mechanizm CORS nie ma zastosowania do obiektów WebSockets. W przypadku ograniczenia pochodzenia dla obiektów WebSockets Odczytaj [ograniczenie pochodzenia elementu WebSockets](xref:fundamentals/websockets#websocket-origin-restriction).

::: moniker-end

::: moniker range="< aspnetcore-2.2"

Ochrona zapewniana przez mechanizm CORS nie ma zastosowania do obiektów WebSockets. Przeglądarki **nie**:

* Wykonaj żądania funkcji CORS przed inspekcją.
* Przestrzeganie ograniczeń określonych w `Access-Control` nagłówkach podczas wykonywania żądań WebSocket.

Jednak przeglądarki wysyłają nagłówek `Origin` podczas wystawiania żądań protokołu WebSocket. Aplikacje powinny być skonfigurowane do sprawdzania poprawności tych nagłówków, aby upewnić się, że dozwolone są tylko usługi WebSockets pochodzące z oczekiwanych źródeł.

W ASP.NET Core 2,1 i nowszych walidacji nagłówka można osiągnąć przy użyciu niestandardowego oprogramowania pośredniczącego umieszczonego **przed `UseSignalR`i uwierzytelniania oprogramowania pośredniczącego** w `Configure`:

[!code-csharp[Main](security/sample/Startup.cs?name=snippet2)]

> [!NOTE]
> Nagłówek `Origin` jest kontrolowany przez klienta, podobnie jak nagłówek `Referer`, może zostać sfałszowany. Tych nagłówków **nie** należy używać jako mechanizmu uwierzytelniania.

::: moniker-end

## <a name="access-token-logging"></a>Rejestrowanie tokenu dostępu

W przypadku korzystania z usługi WebSockets lub zdarzeń wysyłanych przez serwer klient przeglądarki wysyła token dostępu w ciągu zapytania. Uzyskiwanie tokenu dostępu za pośrednictwem ciągu zapytania jest ogólnie bezpieczne, jak w przypadku standardowego nagłówka `Authorization`. Należy zawsze używać protokołu HTTPS, aby zapewnić bezpieczne połączenie między klientem a serwerem. Wiele serwerów sieci Web rejestruje adres URL dla każdego żądania, w tym ciąg zapytania. Rejestrowanie adresów URL może rejestrować token dostępu. ASP.NET Core domyślnie rejestruje adres URL dla każdego żądania, który będzie zawierać ciąg zapytania. Na przykład:

```
info: Microsoft.AspNetCore.Hosting.Internal.WebHost[1]
      Request starting HTTP/1.1 GET http://localhost:5000/myhub?access_token=1234
```

Jeśli masz problemy z rejestrowaniem tych danych w dziennikach serwera, możesz wyłączyć to rejestrowanie w całości poprzez skonfigurowanie rejestratora `Microsoft.AspNetCore.Hosting` na poziomie `Warning` lub wyższym (te komunikaty są zapisywane na poziomie `Info`). Aby uzyskać więcej informacji, zobacz dokumentację dotyczącą [filtrowania dzienników](xref:fundamentals/logging/index#log-filtering) . Jeśli nadal chcesz rejestrować pewne informacje o żądaniu, możesz [napisać oprogramowanie pośredniczące](xref:fundamentals/middleware/write) do rejestrowania wymaganych danych i odfiltrować `access_token` wartość ciągu zapytania (jeśli istnieje).

## <a name="exceptions"></a>Wyjątki

Komunikaty o wyjątkach są zwykle uznawane za dane poufne, które nie powinny być ujawnione dla klienta. Domyślnie SignalR nie wysyła szczegółów wyjątku zgłoszonego przez metodę centrum do klienta programu. Zamiast tego klient otrzymuje komunikat ogólny informujący o wystąpieniu błędu. Dostarczenie komunikatu o wyjątku do klienta może zostać zastąpione (na przykład w przypadku programowania lub testowania) za pomocą [EnableDetailedErrors](xref:signalr/configuration#configure-server-options). Komunikaty o wyjątkach nie powinny być udostępniane klientowi w aplikacjach produkcyjnych.

## <a name="buffer-management"></a>Zarządzanie buforem

SignalR używa buforów dla połączeń do zarządzania wiadomościami przychodzącymi i wychodzącymi. Domyślnie SignalR ogranicza te bufory do 32 KB. Największym komunikatem, który klient lub serwer może wysłać, to 32 KB. Maksymalna ilość pamięci zużywanej przez połączenie dla komunikatów to 32 KB. Jeśli komunikaty są zawsze mniejsze niż 32 KB, można zmniejszyć limit, który:

* Uniemożliwia klientowi wysyłanie większej wiadomości.
* Serwer nigdy nie będzie musiał przydzielić dużych buforów, aby akceptować komunikaty.

Jeśli rozmiar komunikatów przekracza 32 KB, można zwiększyć limit. Zwiększenie tego limitu oznacza:

* Klient może spowodować, że serwer przydzieli bufory dużych pamięci.
* Alokacja serwerów dużych buforów może obniżyć liczbę jednoczesnych połączeń.

Istnieją limity dla wiadomości przychodzących i wychodzących, obie można skonfigurować na obiekcie [HttpConnectionDispatcherOptions](xref:signalr/configuration#configure-server-options) skonfigurowanym w `MapHub`:

* `ApplicationMaxBufferSize` reprezentuje maksymalną liczbę bajtów od klienta, które buforuje serwer. Jeśli klient próbuje wysłać komunikat przekraczający ten limit, połączenie może być zamknięte.
* `TransportMaxBufferSize` reprezentuje maksymalną liczbę bajtów, które może wysłać serwer. Jeśli serwer próbuje wysłać komunikat (uwzględniając wartości zwracane z metod centralnych) większy niż ten limit, zostanie zgłoszony wyjątek.

Ustawienie limitu `0` powoduje wyłączenie limitu. Usunięcie limitu umożliwia klientowi wysłanie komunikatu o dowolnym rozmiarze. Złośliwi klienci wysyłający duże wiadomości mogą spowodować przydzielenie nadmiernej ilości pamięci. Nadmierne użycie pamięci może znacznie zmniejszyć liczbę jednoczesnych połączeń.
