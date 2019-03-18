---
title: gRPC usług za pomocąC#
author: juntaoluo
description: Podstawowe informacje na temat podczas zapisywania gRPC usług za pomocą C#.
monikerRange: '>= aspnetcore-3.0'
ms.author: johluo
ms.date: 03/08/2019
uid: grpc/basics
ms.openlocfilehash: d2ef9316c9bd8c551889c817403f7eb31f48eec6
ms.sourcegitcommit: a467828b5e4eaae291d961ffe2279a571900de23
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/18/2019
ms.locfileid: "58142633"
---
# <a name="grpc-services-with-c"></a>gRPC usług za pomocąC#

W tym dokumencie przedstawiono podstawowe pojęcia, które są potrzebne do zapisania [gRPC](https://grpc.io/docs/guides/) aplikacje w C#. Tematy omówione w tym miejscu dotyczą zarówno [C-core](https://grpc.io/blog/grpc-stacks) i ASP.NET Core gRPC aplikacji.

## <a name="proto-file"></a>proto pliku

gRPC korzysta z metody wymogiem wcześniejszego zawarcia kontraktu, do tworzenia interfejsu API. Domyślnie protokół buforów (protobuf) są używane jako język projektu interfejsu (IDL). *.Proto* plik zawiera:

* Definicja usługi gRPC.
* Komunikaty przesyłane między klientami a serwerami.

Aby uzyskać więcej informacji o składni protobuf plików, zobacz [oficjalnych dokumentów (protobuf)](https://developers.google.com/protocol-buffers/docs/proto3).

Na przykład, rozważmy *greet.proto* plik używany w [Rozpoczynanie pracy z usługą gRPC](xref:tutorials/grpc/grpc-start):

* Definiuje `Greeter` usługi.
* `Greeter` Usługa zawiera definicję `SayHello` wywołania.
* `SayHello` wysyła `HelloRequest` wiadomości i odbiera `HelloResponse` komunikat:

[!code-proto[](~/tutorials/grpc/grpc-start/samples/GrpcStart/Protos/greet.proto)]

## <a name="add-a-proto-file-to-a-c-app"></a>Dodaj plik .proto do C# aplikacji

*.Proto* plik znajduje się w projekcie, dodając ją do `<Protobuf>` grupy elementów:

[!code-xml[](~/tutorials/grpc/grpc-start/samples/GrpcStart/GrpcGreeter.Server/GrpcGreeter.Server.csproj?highlight=2&range=7-10)]

## <a name="c-tooling-support-for-proto-files"></a>C#Narzędzia do obsługi plików .proto

Pakiet narzędzi [Grpc.Tools](https://www.nuget.org/packages/Grpc.Tools/) jest wymagany do wygenerowania C# zasoby z *.proto* plików. Wygenerowanych elementów zawartości (plików):

* Są generowane na podstawie potrzeb jako każdym razem, gdy projekt jest kompilowany.
* Nie są dodawane do projektu lub sprawdzone w formancie źródła.
* Czy artefakt kompilacji zawarte w *obj* katalogu.

Ten pakiet jest wymagane przez projekty serwera i klienta. `Grpc.Tools` można dodać przy użyciu Menedżera pakietów w programie Visual Studio lub dodając `<PackageReference>` do pliku projektu:

[!code-xml[](~/tutorials/grpc/grpc-start/samples/GrpcStart/GrpcGreeter.Server/GrpcGreeter.Server.csproj?highlight=1&range=16)]

Pakiet narzędzi nie jest wymagany w czasie wykonywania, w związku z tym, zależność powinna być oznaczona jako za pomocą `PrivateAssets="All"`.

## <a name="generated-c-assets"></a>Wygenerowany C# zasobów

Pakiet narzędzi wygeneruje C# typy reprezentujące komunikatów zdefiniowany w dołączonej *.proto* plików.

Zasoby po stronie serwera generowany jest abstrakcyjny usługi typu podstawowego. Typ podstawowy zawiera definicje wszystkich wywołań gRPC zawarte w *.proto* pliku. Następnie utwórz konkretny implementacji usługi pochodzi od tego typu podstawowego i implementuje logikę wywołania gRPC. Dla `greet.proto` przykład opisanej powyżej, abstrakcyjną `GreeterBase` typu, który zawiera wirtualny `SayHello` ma generowaną metodę. Konkretną implementację `GreeterService` zastępuje metodę i implementuje logikę obsługi wywołań gRPC.

[!code-cs[](~/tutorials/grpc/grpc-start/samples/GrpcStart/GrpcGreeter.Server/Services/GreeterService.cs?name=snippet)]

Zasoby po stronie klienta generowany jest typu konkretnego klienta. Wywołuje gRPC *.proto* pliku są tłumaczone na konkretny typ, który można wywoływać metod. Aby uzyskać `greet.proto` wcześniej opisywanym przykładzie konkretny `GreeterClient` typ jest generowany. `GreeterClient` Typ zawiera `SayHello` metodę, która może być wywoływana na zainicjowanie połączenia gRPC z serwera.

[!code-cs[](~/tutorials/grpc/grpc-start/samples/GrpcStart/GrpcGreeter.Client/Program.cs?highlight=9-11&name=snippet)]

Domyślnie zasoby serwera i klienta są generowane dla każdego *.proto* plik dołączony `<Protobuf>` grupy elementów. Aby upewnić się, tylko zasoby serwera są generowane w projekcie serwera `GrpcServices` ma ustawioną wartość atrybutu `Server`.

[!code-xml[](~/tutorials/grpc/grpc-start/samples/GrpcStart/GrpcGreeter.Server/GrpcGreeter.Server.csproj?highlight=2&range=7-10)]

Podobnie, ma ustawioną wartość atrybutu `Client` w projektach klienta.

## <a name="additional-resources"></a>Dodatkowe zasoby

* <xref:grpc/index>
* <xref:tutorials/grpc/grpc-start>
* <xref:grpc/aspnetcore>
* <xref:grpc/migration>
