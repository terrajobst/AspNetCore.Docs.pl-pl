---
title: gRPC konfiguracji platformy ASP.NET Core
author: jamesnk
description: Dowiedz się, jak skonfigurować gRPC dla aplikacji platformy ASP.NET Core.
monikerRange: '>= aspnetcore-3.0'
ms.author: jamesnk
ms.custom: mvc
ms.date: 04/09/2019
uid: grpc/configuration
ms.openlocfilehash: 851c9ca1f7d62f6f368df66bb38eb4bbaf64bf32
ms.sourcegitcommit: 5d384db2fa9373a93b5d15e985fb34430e49ad7a
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 05/23/2019
ms.locfileid: "66041891"
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

## <a name="additional-resources"></a>Dodatkowe zasoby

* <xref:tutorials/grpc/grpc-start>
* <xref:grpc/index>
* <xref:grpc/basics>
* <xref:grpc/migration>
