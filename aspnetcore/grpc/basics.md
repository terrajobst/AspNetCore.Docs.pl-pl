---
title: Usługi gRPC w środowisku C#
author: juntaoluo
description: Zapoznaj się z podstawowymi pojęciami dotyczącymi pisania usług gRPC Services za pomocą programu C#.
monikerRange: '>= aspnetcore-3.0'
ms.author: johluo
ms.date: 07/03/2019
uid: grpc/basics
ms.openlocfilehash: 59257449e211ddf9c7faa5f21a3986caba4eebc6
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78664207"
---
# <a name="grpc-services-with-c"></a>usługi gRPC z usługą C\#

W tym dokumencie przedstawiono koncepcje niezbędne do pisania aplikacji [gRPC](https://grpc.io/docs/guides/) w programie C#. Tematy omówione tutaj dotyczą zarówno aplikacji gRPC opartych na procesorze [C](https://grpc.io/blog/grpc-stacks), jak i na ASP.NET Core.

## <a name="proto-file"></a>plik PROTO

gRPC używa podejścia pierwszego kontraktu do programowania interfejsu API. Bufory protokołu (protobuf) są domyślnie używane jako język projektowania interfejsu (IDL). Plik *\*. proto* zawiera:

* Definicja usługi gRPC.
* Komunikaty wysyłane między klientami a serwerami.

Aby uzyskać więcej informacji na temat składni plików protobuf, zapoznaj się z [oficjalną dokumentacją (protobuf)](https://developers.google.com/protocol-buffers/docs/proto3).

Rozważmy na przykład plik *Greeting. proto* używany w temacie [Rozpoczynanie pracy z usługą gRPC](xref:tutorials/grpc/grpc-start):

* Definiuje usługę `Greeter`.
* Usługa `Greeter` definiuje wywołanie `SayHello`.
* `SayHello` wysyła komunikat `HelloRequest` i odbiera komunikat `HelloReply`:

[!code-protobuf[](~/tutorials/grpc/grpc-start/sample/GrpcGreeter/Protos/greet.proto)]
[!INCLUDE[about the series](~/includes/code-comments-loc.md)]

## <a name="add-a-proto-file-to-a-c-app"></a>Dodawanie pliku. proto do aplikacji C\#

Plik *\*. proto* jest dołączany do projektu przez dodanie go do grupy elementów `<Protobuf>`:

[!code-xml[](~/tutorials/grpc/grpc-start/sample/GrpcGreeter/GrpcGreeter.csproj?highlight=2&range=7-9)]

## <a name="c-tooling-support-for-proto-files"></a>C#Obsługa narzędzi dla plików. proto

Pakiet narzędzi [GRPC. Tools](https://www.nuget.org/packages/Grpc.Tools/) jest wymagany do wygenerowania C# elementów zawartości z plików *\*. proto* . Wygenerowane zasoby (pliki):

* Są generowane w przypadku, gdy jest to wymagane, za każdym razem, gdy projekt jest skompilowany.
* Nie są dodawane do projektu ani zaewidencjonowane do kontroli źródła.
* Czy artefakt kompilacji znajduje się w katalogu *obj* .

Ten pakiet jest wymagany przez projekty serwera i klienta. Pakiet `Grpc.AspNetCore`binding zawiera odwołanie do `Grpc.Tools`. Projekty serwera mogą dodawać `Grpc.AspNetCore` przy użyciu Menedżera pakietów w programie Visual Studio lub poprzez dodanie `<PackageReference>` do pliku projektu:

[!code-xml[](~/tutorials/grpc/grpc-start/sample/GrpcGreeter/GrpcGreeter.csproj?highlight=1&range=12)]

Projekty klienta powinny bezpośrednio odwoływać się `Grpc.Tools` wraz z innymi pakietami wymaganymi do korzystania z klienta gRPC. Pakiet narzędzi nie jest wymagany w czasie wykonywania, więc zależność jest oznaczona `PrivateAssets="All"`:

[!code-xml[](~/tutorials/grpc/grpc-start/sample/GrpcGreeterClient/GrpcGreeterClient.csproj?highlight=3&range=9-11)]

## <a name="generated-c-assets"></a>Wygenerowane C# zasoby

Pakiet narzędzi generuje C# typy reprezentujące komunikaty zdefiniowane w dołączonych plikach *\*. proto* .

W przypadku zasobów po stronie serwera jest generowany abstrakcyjny typ podstawowy usługi. Typ podstawowy zawiera definicje wszystkich wywołań gRPC znajdujących się w pliku *. proto* . Utwórz konkretną implementację usługi, która pochodzi z tego typu podstawowego i implementuje logikę dla wywołań gRPC. W przypadku `greet.proto`opisany wcześniej przykład jest typem abstrakcyjnym `GreeterBase` zawierającym metodę `SayHello` wirtualnej. Konkretna implementacja `GreeterService` przesłania metodę i implementuje logikę obsługi wywołania gRPC.

[!code-csharp[](~/tutorials/grpc/grpc-start/sample/GrpcGreeter/Services/GreeterService.cs?name=snippet)]

Dla zasobów po stronie klienta jest generowany konkretny typ klienta. Wywołania gRPC w pliku *. proto* są tłumaczone na metody w konkretnym typie, który można wywołać. W przypadku `greet.proto`przedstawiony wcześniej przykład jest generowany konkretny `GreeterClient` typ. Wywołaj `GreeterClient.SayHelloAsync`, aby zainicjować wywołanie gRPC na serwerze.

[!code-csharp[](~/tutorials/grpc/grpc-start/sample/GrpcGreeterClient/Program.cs?name=snippet)]

Domyślnie zasoby serwera i klienta są generowane dla każdego pliku *\*. proto* znajdującego się w grupie `<Protobuf>` elementów. Aby upewnić się, że tylko zasoby serwera są generowane w projekcie serwera, atrybut `GrpcServices` jest ustawiony na `Server`.

[!code-xml[](~/tutorials/grpc/grpc-start/sample/GrpcGreeter/GrpcGreeter.csproj?highlight=2&range=7-9)]

Podobnie atrybut jest ustawiany na `Client` w projektach klientów.

[!INCLUDE[](~/includes/gRPCazure.md)]

## <a name="additional-resources"></a>Dodatkowe zasoby

* <xref:grpc/index>
* <xref:tutorials/grpc/grpc-start>
* <xref:grpc/aspnetcore>
* <xref:grpc/client>
