---
title: gRPC konfiguracji platformy ASP.NET Core
author: jamesnk
description: Dowiedz się, jak skonfigurować gRPC dla aplikacji platformy ASP.NET Core.
monikerRange: '>= aspnetcore-3.0'
ms.author: jamesnk
ms.custom: mvc
ms.date: 04/09/2019
uid: grpc/configuration
ms.openlocfilehash: 66dfb9ec136616f10c1b7aaad766e18813b87de4
ms.sourcegitcommit: eb784a68219b4829d8e50c8a334c38d4b94e0cfa
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/22/2019
ms.locfileid: "59984524"
---
# <a name="grpc-for-aspnet-core-configuration"></a>gRPC konfiguracji platformy ASP.NET Core

## <a name="configure-services-options"></a>Skonfiguruj opcje usługi

W poniższej tabeli opisano opcje dotyczące konfigurowania usługi gRPC:

| Opcja | Wartość domyślna | Opis |
| ------ | ------------- | ----------- |
| `SendMaxMessageSize` | `null` | Maksymalny rozmiar wiadomości w bajtach, które mogą być wysyłane z serwera. Podjęto próbę wysłać komunikat, który przekracza skutkuje rozmiar skonfigurowany maksymalny komunikat o wyjątku. |
| `ReceiveMaxMessageSize` | 4 MB | Maksymalny rozmiar wiadomości w bajtów odebranych przez serwer. Jeśli serwer odbiera komunikat, który przekracza ten limit, zgłasza wyjątek. Zwiększenie tej wartości umożliwia serwer do odbierania komunikatów większy, ale może mieć negatywny wpływ na zużycie pamięci. |
| `EnableDetailedErrors` | `false` | Jeśli `true`, szczegółowe komunikaty o wyjątkach są zwracane do klientów, gdy wyjątek jest zgłaszany w metodzie usług. Wartość domyślna to `false`. Ustawienie tej opcji na `true` można przecieku informacji poufnych. |
| `CompressionProviders` | Gzip | Kolekcja dostawców kompresji, które umożliwiają kompresję i dekompresję wiadomości. Można tworzyć i dodawać je do kolekcji dostawców niestandardowych kompresji. Domyślnie skonfigurowany dostawca obsługuje **gzip** kompresji. |
| `ResponseCompressionAlgorithm` | `null` | Algorytm kompresji używany do skompresowania komunikatów wysyłanych z serwera. Algorytm musi być zgodny dostawca kompresji w `CompressionProviders`. Dla algorthm kompresowały odpowiedzi klienta musi wskazywać obsługuje algorytm przez wysłanie go **grpc zaakceptować encoding** nagłówka. |
| `ResponseCompressionLevel` | `null` | Poziom kompresji, używany do skompresowania komunikatów wysyłanych z serwera. |

Można skonfigurować opcje dla wszystkich usług, zapewniając delegat opcje do `AddGrpc` wywołania `Startup.ConfigureServices`.

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddGrpc(options =>
    {
        options.EnableDetailedErrors = true;
    });
}
```

Opcje dla jednej usługi zastępują opcje globalne w `AddGrpc` i może być konfigurowana przy użyciu `AddServiceOptions<TService>`:

```csharp
services.AddGrpc().AddServiceOptions<MyService>(options =>
{
    options.ReceiveMaxMessageSize = 10 * 1024 * 1024; // 10 megabytes
});
```

## <a name="configure-kestrel-options"></a>Skonfiguruj opcje Kestrel

Serwer kestrel ma opcji konfiguracji, które wpływają na zachowanie gRPC dla platformy ASP.NET.

### <a name="request-body-data-rate-limit"></a>Limit szybkości danych treści żądania

Domyślnie serwer Kestrel nakłada [szybkość danych treści żądania minimalne](
<xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits.MinRequestBodyDataRate>). Dla klienta przesyłania strumieniowego i przesyłania strumieniowego wywołania dupleks stawki nie może zostać wykonane i może zostać przekroczony limit czasu połączenia. Minimalna żądania ograniczania liczby wywołań danych musi być wyłączona, gdy usługa gRPC obejmuje klienta przesyłania strumieniowego i przesyłania strumieniowego wywołania dupleks treści:

```csharp
public class Program
{
    public static void Main(string[] args)
    {
        CreateHostBuilder(args).Build().Run();
    }

    public static IHostBuilder CreateHostBuilder(string[] args) =>
         Host.CreateDefaultBuilder(args)
    .ConfigureWebHostDefaults(webBuilder =>
    {
        webBuilder.UseStartup<Startup>();
        webBuilder.ConfigureKestrel((context, options) =>
        {
            options.Limits.MinRequestBodyDataRate = null;
        });
    });
}
```

## <a name="additional-resources"></a>Dodatkowe zasoby

* <xref:tutorials/grpc/grpc-start>
* <xref:grpc/index>
* <xref:grpc/basics>
* <xref:grpc/migration>
