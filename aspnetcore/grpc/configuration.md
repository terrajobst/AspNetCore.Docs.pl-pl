---
title: gRPC konfiguracji platformy ASP.NET Core
author: jamesnk
description: Dowiedz się, jak skonfigurować gRPC dla aplikacji platformy ASP.NET Core.
monikerRange: '>= aspnetcore-3.0'
ms.author: jamesnk
ms.custom: mvc
ms.date: 05/30/2019
uid: grpc/configuration
ms.openlocfilehash: e269d701f45c0b852a9006107f0162cc5af2c38a
ms.sourcegitcommit: 8516b586541e6ba402e57228e356639b85dfb2b9
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/11/2019
ms.locfileid: "67814924"
---
# <a name="grpc-for-aspnet-core-configuration"></a>gRPC konfiguracji platformy ASP.NET Core

## <a name="configure-services-options"></a>Skonfiguruj opcje usługi

W poniższej tabeli opisano opcje dotyczące konfigurowania usługi gRPC:

| Opcja | Wartość domyślna | Opis |
| ------ | ------------- | ----------- |
| `SendMaxMessageSize` | `null` | Maksymalny rozmiar wiadomości w bajtach, które mogą być wysyłane z serwera. Podjęto próbę wysłać komunikat, który przekracza skutkuje rozmiar skonfigurowany maksymalny komunikat o wyjątku. |
| `ReceiveMaxMessageSize` | 4 MB | Maksymalny rozmiar wiadomości w bajtów odebranych przez serwer. Jeśli serwer odbiera komunikat, który przekracza ten limit, zgłasza wyjątek. Zwiększenie tej wartości umożliwia serwer do odbierania komunikatów większy, ale może mieć negatywny wpływ na zużycie pamięci. |
| `EnableDetailedErrors` | `false` | Jeśli `true`, szczegółowe komunikaty o wyjątkach są zwracane do klientów, gdy wyjątek jest zgłaszany w metodzie usług. Wartość domyślna to `false`. Ustawienie `EnableDetailedErrors` do `true` można przecieku informacji poufnych. |
| `CompressionProviders` | Gzip | Kolekcja dostawców kompresji, które umożliwiają kompresję i dekompresję wiadomości. Można tworzyć i dodawać je do kolekcji dostawców niestandardowych kompresji. Domyślnie skonfigurowany dostawca obsługuje **gzip** kompresji. |
| `ResponseCompressionAlgorithm` | `null` | Algorytm kompresji używany do skompresowania komunikatów wysyłanych z serwera. Algorytm musi być zgodny dostawca kompresji w `CompressionProviders`. Dla algorytmu kompresowały odpowiedzi, klient musi wskazywać obsługuje algorytm przez wysłanie go **grpc zaakceptować encoding** nagłówka. |
| `ResponseCompressionLevel` | `null` | Poziom kompresji, używany do skompresowania komunikatów wysyłanych z serwera. |

Można skonfigurować opcje dla wszystkich usług, zapewniając delegat opcje do `AddGrpc` wywołania `Startup.ConfigureServices`:

[!code-csharp[](~/grpc/configuration/sample/GrcpService/Startup.cs?name=snippet)]

Opcje dla jednej usługi zastępują opcje globalne w `AddGrpc` i może być konfigurowana przy użyciu `AddServiceOptions<TService>`:

[!code-csharp[](~/grpc/configuration/sample/GrcpService/Startup2.cs?name=snippet)]

## <a name="configure-client-options"></a>Skonfiguruj opcje klienta

Poniższy kod ustawia maksymalna liczba wysyłanych klienta i wyświetlany rozmiar wiadomości:

[!code-csharp[](~/grpc/configuration/sample/Program.cs?name=snippet&highlight=3-6)]

## <a name="additional-resources"></a>Dodatkowe zasoby

* <xref:tutorials/grpc/grpc-start>
* <xref:grpc/index>
* <xref:grpc/basics>
* <xref:grpc/migration>
