---
title: Usługi gRPC w środowisku C#
author: juntaoluo
description: Zapoznaj się z podstawowymi pojęciami dotyczącymi pisania usług gRPC Services za pomocą programu C#.
monikerRange: '>= aspnetcore-3.0'
ms.author: johluo
ms.date: 07/03/2019
uid: grpc/basics
ms.openlocfilehash: 8d99d036fd4b00fc4568e67ea5225dc006dea4b1
ms.sourcegitcommit: 73e255e846e414821b8cc20ffa3aec946735cd4e
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/03/2019
ms.locfileid: "71925187"
---
# <a name="grpc-services-with-c"></a>usługi gRPC w języku C\#

W tym dokumencie przedstawiono koncepcje niezbędne do pisania aplikacji [gRPC](https://grpc.io/docs/guides/) w programie C#. Tematy omówione tutaj dotyczą zarówno aplikacji gRPC opartych na procesorze [C](https://grpc.io/blog/grpc-stacks), jak i na ASP.NET Core.

## <a name="proto-file"></a>plik PROTO

gRPC używa podejścia pierwszego kontraktu do programowania interfejsu API. Bufory protokołu (protobuf) są domyślnie używane jako język projektowania interfejsu (IDL). Plik *@no__t -1. proto* zawiera następujące:

* Definicja usługi gRPC.
* Komunikaty wysyłane między klientami a serwerami.

Aby uzyskać więcej informacji na temat składni plików protobuf, zapoznaj się z [oficjalną dokumentacją (protobuf)](https://developers.google.com/protocol-buffers/docs/proto3).

Rozważmy na przykład plik *Greeting. proto* używany w temacie [Rozpoczynanie pracy z usługą gRPC](xref:tutorials/grpc/grpc-start):

* `Greeter` Definiuje usługę.
* `Greeter` Usługa`SayHello` definiuje wywołanie.
* `SayHello`wysyła komunikat i odbiera `HelloReply`komunikat: `HelloRequest`

[!code-protobuf[](~/tutorials/grpc/grpc-start/sample/GrpcGreeter/Protos/greet.proto)]

## <a name="add-a-proto-file-to-a-c-app"></a>Dodawanie pliku. proto do aplikacji C\#

Plik *@no__t -1. proto* jest zawarty w projekcie przez dodanie go do grupy elementów `<Protobuf>`:

[!code-xml[](~/tutorials/grpc/grpc-start/sample/GrpcGreeter/GrpcGreeter.csproj?highlight=2&range=7-9)]

## <a name="c-tooling-support-for-proto-files"></a>C#Obsługa narzędzi dla plików. proto

Pakiet narzędzi [GRPC. Tools](https://www.nuget.org/packages/Grpc.Tools/) jest wymagany do wygenerowania C# elementów zawartości z plików *@no__t -3. proto* . Wygenerowane zasoby (pliki):

* Są generowane w przypadku, gdy jest to wymagane, za każdym razem, gdy projekt jest skompilowany.
* Nie są dodawane do projektu ani zaewidencjonowane do kontroli źródła.
* Czy artefakt kompilacji znajduje się w katalogu *obj* .

Ten pakiet jest wymagany przez projekty serwera i klienta. Pakietbinding zawiera odwołanie do `Grpc.Tools`. `Grpc.AspNetCore` Projekty serwera mogą `<PackageReference>` być `Grpc.AspNetCore` dodawane przy użyciu Menedżera pakietów w programie Visual Studio lub poprzez dodanie do pliku projektu:

[!code-xml[](~/tutorials/grpc/grpc-start/sample/GrpcGreeter/GrpcGreeter.csproj?highlight=1&range=12)]

Projekty klientów powinny bezpośrednio odwoływać się do `Grpc.Tools` wraz z innymi pakietami wymaganymi do korzystania z klienta gRPC. Pakiet narzędzi nie jest wymagany w czasie wykonywania, dlatego zależność jest oznaczona przy użyciu `PrivateAssets="All"`:

[!code-xml[](~/tutorials/grpc/grpc-start/sample/GrpcGreeterClient/GrpcGreeterClient.csproj?highlight=3&range=9-11)]

## <a name="generated-c-assets"></a>Wygenerowane C# zasoby

Pakiet narzędzi generuje C# typy reprezentujące komunikaty zdefiniowane w dołączonych plikach *@no__t -2. proto* .

W przypadku zasobów po stronie serwera jest generowany abstrakcyjny typ podstawowy usługi. Typ podstawowy zawiera definicje wszystkich wywołań gRPC znajdujących się w pliku *. proto* . Utwórz konkretną implementację usługi, która pochodzi z tego typu podstawowego i implementuje logikę dla wywołań gRPC. W przypadku `GreeterBase` `SayHello` , przykładu opisanego wcześniej, generowany jest typ abstrakcyjny, który zawiera metodę wirtualną. `greet.proto` Konkretna implementacja `GreeterService` przesłania metodę i implementuje logikę obsługi wywołania gRPC.

[!code-csharp[](~/tutorials/grpc/grpc-start/sample/GrpcGreeter/Services/GreeterService.cs?name=snippet)]

Dla zasobów po stronie klienta jest generowany konkretny typ klienta. Wywołania gRPC w pliku *. proto* są tłumaczone na metody w konkretnym typie, który można wywołać. W przypadku `GreeterClient` , przykładu opisanego wcześniej, generowany jest konkretny typ. `greet.proto` Wywołaj `GreeterClient.SayHelloAsync` , aby zainicjować wywołanie gRPC na serwerze.

[!code-csharp[](~/tutorials/grpc/grpc-start/sample/GrpcGreeterClient/Program.cs?name=snippet)]

Domyślnie zasoby serwera i klienta są generowane dla każdego pliku *@no__t -1. proto* , który znajduje się w grupie elementów `<Protobuf>`. Aby upewnić się, że tylko zasoby serwera są generowane w projekcie serwera `GrpcServices` , atrybut jest ustawiany `Server`na.

[!code-xml[](~/tutorials/grpc/grpc-start/sample/GrpcGreeter/GrpcGreeter.csproj?highlight=2&range=7-9)]

Podobnie atrybut jest ustawiany na `Client` w projektach klientów.

[!INCLUDE[](~/includes/gRPCazure.md)]

## <a name="additional-resources"></a>Dodatkowe zasoby

* <xref:grpc/index>
* <xref:tutorials/grpc/grpc-start>
* <xref:grpc/aspnetcore>
* <xref:grpc/client>
