---
title: Konfiguracja programu gRPC for .NET
author: jamesnk
description: Dowiedz się, jak skonfigurować gRPC dla aplikacji .NET.
monikerRange: '>= aspnetcore-3.0'
ms.author: jamesnk
ms.custom: mvc
ms.date: 09/05/2019
uid: grpc/configuration
ms.openlocfilehash: de478752f94713d7f3d55ed66e6410500c3a72e8
ms.sourcegitcommit: 2cb857f0de774df421e35289662ba92cfe56ffd1
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 12/25/2019
ms.locfileid: "75355732"
---
# <a name="grpc-for-net-configuration"></a>Konfiguracja programu gRPC for .NET

## <a name="configure-services-options"></a>Konfigurowanie opcji usług

usługi gRPC są konfigurowane przy użyciu `AddGrpc` w *Startup.cs*. W poniższej tabeli opisano opcje konfigurowania usług gRPC:

| Opcja | Wartość domyślna | Opis |
| ------ | ------------- | ----------- |
| `MaxSendMessageSize` | `null` | Maksymalny rozmiar wiadomości w bajtach, które mogą być wysyłane z serwera. Próba wysłania komunikatu, który przekracza skonfigurowany maksymalny rozmiar komunikatu, spowoduje wyjątek. |
| `MaxReceiveMessageSize` | 4 MB | Maksymalny rozmiar komunikatu w bajtach, który może zostać odebrany przez serwer. Jeśli serwer odbiera komunikat, który przekracza ten limit, zgłasza wyjątek. Zwiększenie tej wartości umożliwia serwerowi otrzymywanie większych komunikatów, ale może mieć negatywny wpływ na użycie pamięci. |
| `EnableDetailedErrors` | `false` | Jeśli `true`, szczegółowe komunikaty o wyjątkach są zwracane do klientów, gdy wyjątek jest zgłaszany w metodzie usługi. Wartość domyślna to `false`. Ustawienie `EnableDetailedErrors` `true` może wyciekować poufne informacje. |
| `CompressionProviders` | gzip | Kolekcja dostawców kompresji służąca do kompresowania i dekompresowania komunikatów. Niestandardowych dostawców kompresji można utworzyć i dodać do kolekcji. Domyślnie skonfigurowane dostawcy obsługują kompresję w formacie **gzip** . |
| `ResponseCompressionAlgorithm` | `null` | Algorytm kompresji używany do kompresowania komunikatów wysyłanych z serwera. Algorytm musi być zgodny z dostawcą kompresji w `CompressionProviders`. Aby algorytm był kompresowany odpowiedzi, klient musi wskazać, że obsługuje algorytm, wysyłając go w nagłówku **GRPC-Accept-Encoding** . |
| `ResponseCompressionLevel` | `null` | Poziom kompresji używany do kompresowania komunikatów wysyłanych z serwera. |
| `Interceptors` | Brak | Kolekcja przechwyceń, które są uruchamiane z każdym wywołaniem gRPC. Interceptory są uruchamiane w kolejności, w jakiej zostały zarejestrowane. Skonfigurowane globalnie Interceptory są uruchamiane przed przechwyceniami skonfigurowanymi dla jednej usługi. Aby uzyskać więcej informacji na temat interceptorów gRPC, zobacz [GRPC Interceptory i oprogramowanie pośredniczące](xref:grpc/migration#grpc-interceptors-vs-middleware). |

Opcje można skonfigurować dla wszystkich usług, dostarczając opcje delegata wywołania `AddGrpc` w `Startup.ConfigureServices`:

[!code-csharp[](~/grpc/configuration/sample/GrcpService/Startup.cs?name=snippet)]

Opcje pojedynczej usługi przesłaniają opcje globalne dostępne w `AddGrpc` i można je skonfigurować za pomocą `AddServiceOptions<TService>`:

[!code-csharp[](~/grpc/configuration/sample/GrcpService/Startup2.cs?name=snippet)]

## <a name="configure-client-options"></a>Konfigurowanie opcji klienta

Konfiguracja klienta gRPC jest ustawiona na `GrpcChannelOptions`. W poniższej tabeli opisano opcje konfigurowania kanałów gRPC:

| Opcja | Wartość domyślna | Opis |
| ------ | ------------- | ----------- |
| `HttpClient` | Nowe wystąpienie | `HttpClient` używany do wykonywania wywołań gRPC. Można ustawić klienta, aby skonfigurować niestandardowy `HttpClientHandler`lub dodać dodatkowe programy obsługi do potoku HTTP dla wywołań gRPC. Jeśli nie określono `HttpClient`, nowe wystąpienie `HttpClient` zostanie utworzone dla kanału. Zostanie on automatycznie usunięty. |
| `DisposeHttpClient` | `false` | Jeśli `true`i określono `HttpClient`, wystąpienie `HttpClient` zostanie usunięte po usunięciu `GrpcChannel`. |
| `LoggerFactory` | `null` | `LoggerFactory` używany przez klienta do rejestrowania informacji o wywołaniach gRPC. Wystąpienie `LoggerFactory` może zostać rozpoznane z iniekcji zależności lub utworzone przy użyciu `LoggerFactory.Create`. Przykłady konfigurowania rejestrowania znajdują się w temacie <xref:grpc/diagnostics#grpc-client-logging>. |
| `MaxSendMessageSize` | `null` | Maksymalny rozmiar wiadomości w bajtach, które mogą być wysyłane z klienta. Próba wysłania komunikatu, który przekracza skonfigurowany maksymalny rozmiar komunikatu, spowoduje wyjątek. |
| `MaxReceiveMessageSize` | 4 MB | Maksymalny rozmiar komunikatu w bajtach, który może zostać odebrany przez klienta. Jeśli klient odbiera komunikat, który przekracza ten limit, zgłasza wyjątek. Zwiększenie tej wartości umożliwia klientowi otrzymywanie większych komunikatów, ale może mieć negatywny wpływ na użycie pamięci. |
| `Credentials` | `null` | Wystąpienie `ChannelCredentials`. Poświadczenia służą do dodawania metadanych uwierzytelniania do wywołań gRPC. |
| `CompressionProviders` | gzip | Kolekcja dostawców kompresji służąca do kompresowania i dekompresowania komunikatów. Niestandardowych dostawców kompresji można utworzyć i dodać do kolekcji. Domyślnie skonfigurowane dostawcy obsługują kompresję w formacie **gzip** . |

Następujący kod:

* Ustawia maksymalny rozmiar wiadomości wysyłania i odbierania w kanale.
* Tworzy klienta.

[!code-csharp[](~/grpc/configuration/sample/Program.cs?name=snippet&highlight=3-8)]

[!INCLUDE[](~/includes/gRPCazure.md)]

## <a name="additional-resources"></a>Dodatkowe zasoby

* <xref:grpc/aspnetcore>
* <xref:grpc/client>
* <xref:grpc/diagnostics>
* <xref:tutorials/grpc/grpc-start>
