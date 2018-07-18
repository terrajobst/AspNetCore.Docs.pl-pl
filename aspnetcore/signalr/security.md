---
title: Zagadnienia dotyczące zabezpieczeń w biblioteki SignalR platformy ASP.NET Core
author: tdykstra
description: Dowiedz się, jak używać uwierzytelniania i autoryzacji w biblioteki SignalR platformy ASP.NET Core.
monikerRange: '>= aspnetcore-2.1'
ms.author: anurse
ms.custom: mvc
ms.date: 06/29/2018
uid: signalr/security
ms.openlocfilehash: b66c7fbfbaee4c70a68f3132875fbc81018c3e20
ms.sourcegitcommit: 3ca527f27c88cfc9d04688db5499e372fbc2c775
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/17/2018
ms.locfileid: "39095135"
---
# <a name="security-considerations-in-aspnet-core-signalr"></a>Zagadnienia dotyczące zabezpieczeń w biblioteki SignalR platformy ASP.NET Core

Przez [Andrew Stanton pielęgniarki](https://twitter.com/anurse)

## <a name="overview"></a>Omówienie

Biblioteka SignalR udostępnia szereg zabezpieczenia domyślne. Należy zrozumieć, jak skonfigurować te zabezpieczenia.

### <a name="cross-origin-resource-sharing"></a>Współużytkowanie zasobów między źródłami

[Współużytkowanie zasobów między źródłami (cors)](https://en.wikipedia.org/wiki/Cross-origin_resource_sharing) może służyć do Zezwalaj na połączenia SignalR cross-origin w przeglądarce. Jeśli kod JavaScript jest hostowana w domenie o innej nazwie z aplikacji SignalR, należy włączyć [oprogramowanie pośredniczące ASP.NET Core CORS](xref:security/cors) aby umożliwić połączenia. Ogólnie rzecz biorąc umożliwiają żądań cross-origin tylko z domen, kontrolowanymi przez użytkownika. Na przykład, jeśli Twoja witryna jest hostowana pod `http://www.example.com` i aplikacji SignalR znajduje się na `http://signalr.example.com`, należy skonfigurować mechanizmu CORS w aplikacji SignalR, aby zezwalać tylko na pochodzenie `www.example.com`.

Aby uzyskać więcej informacji na temat konfigurowania mechanizmu CORS, zobacz [dokumentacji dotyczącej platformy ASP.NET Core CORS](xref:security/cors). SignalR wymaga następujących zasad CORS, aby działać poprawnie:

* Zasady muszą zezwalać na określonych źródeł oczekiwać lub zezwolić na wszystkie pochodzenia (niezalecane).
* Metody HTTP `GET` i `POST` muszą być dozwolone.
* Poświadczenia musi być włączona, nawet wtedy, gdy nie używasz uwierzytelniania.

Na przykład następujące zasady CORS umożliwia klient przeglądarki SignalR w serwisie `http://example.com` dostęp do aplikacji SignalR:

```csharp
public void Configure(IApplicationBuilder app)
{
    // ... other middleware ...

    // Make sure the CORS middleware is ahead of SignalR.
    app.UseCors(builder => {
        builder.WithOrigins("http://example.com")
            .AllowAnyHeader()
            .WithMethods("GET", "POST")
            .AllowCredentials();
    });

    // ... other middleware ...

    app.UseSignalR();

    // ... other middleware ...
}
```

> [!NOTE]
> SignalR nie jest zgodny z wbudowanej funkcji CORS w usłudze Azure App Service.

### <a name="access-token-logging"></a>Rejestrowanie tokenu dostępu

Korzystając z funkcji WebSockets lub Server-Sent zdarzeń, klient przeglądarki wysyła ten token dostępu w ciągu zapytania. Jest to zazwyczaj tak bezpieczne, jak przy użyciu standardu `Authorization` nagłówka, jednak wiele serwerów sieci web Zaloguj się adres URL dla każdego żądania, w tym ciągu zapytania. Oznacza to, że token dostępu mogą zostać zawarte w dziennikach. Należy wziąć pod uwagę, przeglądanie ustawień rejestrowania serwera sieci web, aby uniknąć rejestrowania tych informacji.

### <a name="exceptions"></a>Wyjątki

Komunikaty o wyjątkach są zazwyczaj uważane za dane poufne, które nie może uzyskać dostęp do klienta. Domyślnie SignalR nie wysyła szczegółowe informacje o wyjątku przez metodę koncentratora do klienta. Zamiast tego klient odbiera ogólny komunikat wskazujący, że wystąpił błąd. Zachowanie to można zastąpić, ustawiając [ `EnableDetailedErrors` ](xref:signalr/configuration#configure-server-options) ustawienie.

### <a name="buffer-management"></a>Zarządzanie buforem

SignalR używa buforów dla każdego połączenia, aby można było zarządzać wiadomości przychodzących i wychodzących. Domyślnie SignalR ogranicza bufory 32 KB. Oznacza to, że największych możliwych komunikat, który może wysyłać klienta lub serwera to 32 KB. Oznacza to, że maksymalna ilość pamięci używanej przez połączenie dla komunikatów to 32 KB. Jeśli wiesz, że wiadomości, zawsze są mniejsze niż to ograniczenie, można zmniejszyć ten rozmiar, aby uniemożliwić klientowi możliwość wysyłania wiadomości większych i wymusić można przydzielić pamięci, aby je zaakceptować. Podobnie jeśli wiesz, że wiadomości są większe niż to ograniczenie, można zwiększyć go. Należy jednak pamiętać, że zwiększenie tego limitu oznacza, że klient może spowodować, że serwer można przydzielić więcej pamięci może zmniejszyć liczbę jednoczesnych połączeń, które aplikacja może obsłużyć.

Istnieją oddzielne limity dla komunikatów przychodzących i wychodzących, skonfigurowania zarówno na [ `HttpConnectionDispatcherOptions` ](xref:signalr/configuration#configure-server-options) skonfigurowany w obiekcie `MapHub`:

* `ApplicationMaxBufferSize` przedstawia maksymalną liczbę bajtów z zakresu od klienta, bufory serwera. Jeśli klient próbuje wysłać wiadomość przekracza ten limit, połączenie może być zamknięte.
* `TransportMaxBufferSize` reprezentuje maksymalną liczbę bajtów, jaką serwer może wysyłać. Jeśli serwer próbuje wysłać wiadomość (w tym wartości zwracane metod koncentratora) przekracza ten limit, zostanie zgłoszony wyjątek.

Ustawienie wartości limitu `0` całkowicie wyłącza limit. Jednak ta powinna być podejmowana z najwyższą ostrożnością. Usunięcie limitu umożliwia klientowi wysłać komunikat o dowolnym rozmiarze. To może być używana przez złośliwego klienta powodują nadmierną ilość pamięci do przydzielenia, która może znacznie zmniejszyć liczbę jednoczesnych połączeń, które aplikacja może obsługiwać.
