---
title: gRPC konfiguracji ASP.NET Core
author: jamesnk
description: Dowiedz się, jak skonfigurować gRPC dla aplikacji ASP.NET Core.
monikerRange: '>= aspnetcore-3.0'
ms.author: jamesnk
ms.custom: mvc
ms.date: 08/21/2019
uid: grpc/configuration
ms.openlocfilehash: 34eb598211c87fbb2c68ae5e041da50d02f543f7
ms.sourcegitcommit: 8b36f75b8931ae3f656e2a8e63572080adc78513
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 09/05/2019
ms.locfileid: "70310315"
---
# <a name="grpc-for-aspnet-core-configuration"></a>gRPC konfiguracji ASP.NET Core

## <a name="configure-services-options"></a>Konfigurowanie opcji usług

W poniższej tabeli opisano opcje konfigurowania usług gRPC:

| Opcja | Wartość domyślna | Opis |
| ------ | ------------- | ----------- |
| `MaxSendMessageSize` | `null` | Maksymalny rozmiar wiadomości w bajtach, które mogą być wysyłane z serwera. Próba wysłania komunikatu, który przekracza skonfigurowany maksymalny rozmiar komunikatu, spowoduje wyjątek. |
| `MaxReceiveMessageSize` | 4 MB | Maksymalny rozmiar komunikatu w bajtach, który może zostać odebrany przez serwer. Jeśli serwer odbiera komunikat, który przekracza ten limit, zgłasza wyjątek. Zwiększenie tej wartości umożliwia serwerowi otrzymywanie większych komunikatów, ale może mieć negatywny wpływ na użycie pamięci. |
| `EnableDetailedErrors` | `false` | Jeśli `true`szczegółowe komunikaty o wyjątkach są zwracane do klientów, gdy wyjątek jest zgłaszany w metodzie usługi. Wartość domyślna to `false`. Ustawienie `EnableDetailedErrors` , `true` aby można było wyciekować poufne informacje. |
| `CompressionProviders` | gzip, Wklęśnięcie | Kolekcja dostawców kompresji służąca do kompresowania i dekompresowania komunikatów. Niestandardowych dostawców kompresji można utworzyć i dodać do kolekcji. Domyślnie skonfigurowane dostawcy obsługują kompresję **gzip** i **Wklęśnięcie** . |
| `ResponseCompressionAlgorithm` | `null` | Algorytm kompresji używany do kompresowania komunikatów wysyłanych z serwera. Algorytm musi być zgodny z dostawcą kompresji w `CompressionProviders`. Aby algorytm był kompresowany odpowiedzi, klient musi wskazać, że obsługuje algorytm, wysyłając go w nagłówku **GRPC-Accept-Encoding** . |
| `ResponseCompressionLevel` | `null` | Poziom kompresji używany do kompresowania komunikatów wysyłanych z serwera. |

Opcje można skonfigurować dla wszystkich usług, dostarczając opcje delegata `AddGrpc` wywołania w: `Startup.ConfigureServices`

[!code-csharp[](~/grpc/configuration/sample/GrcpService/Startup.cs?name=snippet)]

Opcje pojedynczej usługi przesłaniają opcje globalne podane w `AddGrpc` i można je skonfigurować przy użyciu: `AddServiceOptions<TService>`

[!code-csharp[](~/grpc/configuration/sample/GrcpService/Startup2.cs?name=snippet)]

## <a name="configure-client-options"></a>Konfigurowanie opcji klienta

W poniższej tabeli opisano opcje konfigurowania kanałów gRPC:

| Opcja | Wartość domyślna | Opis |
| ------ | ------------- | ----------- |
| `HttpClient` | Nowe wystąpienie | `HttpClient` Używane do wykonywania wywołań gRPC. Można ustawić klienta, aby skonfigurować niestandardowe `HttpClientHandler`lub dodać dodatkowe programy obsługi do potoku HTTP dla wywołań gRPC. Domyślnie tworzone jest nowe `HttpClient` wystąpienie. |
| `MaxSendMessageSize` | `null` | Maksymalny rozmiar wiadomości w bajtach, które mogą być wysyłane z klienta. Próba wysłania komunikatu, który przekracza skonfigurowany maksymalny rozmiar komunikatu, spowoduje wyjątek. |
| `MaxReceiveMessageSize` | 4 MB | Maksymalny rozmiar komunikatu w bajtach, który może zostać odebrany przez klienta. Jeśli serwer odbiera komunikat, który przekracza ten limit, zgłasza wyjątek. Zwiększenie tej wartości umożliwia serwerowi otrzymywanie większych komunikatów, ale może mieć negatywny wpływ na użycie pamięci. |
| `TransportOptions` | `null` | Opcje transportu konfigurują sposób wywoływania usługi gRPC przez kanał. Obecnie jedyną implementacją są `HttpClientTransport` opcje pozwalające `HttpClient` określić użycie gRPC. |
| `Credentials` | `null` | `ChannelCredentials` Wystąpienie. Poświadczenia służą do dodawania metadanych uwierzytelniania do wywołań gRPC. |
| `CompressionProviders` | gzip, Wklęśnięcie | Kolekcja dostawców kompresji służąca do kompresowania i dekompresowania komunikatów. Niestandardowych dostawców kompresji można utworzyć i dodać do kolekcji. Domyślnie skonfigurowane dostawcy obsługują kompresję **gzip** i **Wklęśnięcie** . |

Następujący kod:

* Ustawia maksymalny rozmiar wiadomości wysyłania i odbierania w kanale.
* Tworzy klienta.

[!code-csharp[](~/grpc/configuration/sample/Program.cs?name=snippet&highlight=3-8)]

## <a name="additional-resources"></a>Dodatkowe zasoby

* <xref:grpc/aspnetcore>
* <xref:grpc/client>
* <xref:tutorials/grpc/grpc-start>
