---
title: Usługi gRPC w środowisku C#
author: juntaoluo
description: Podstawowe informacje na temat podczas zapisywania gRPC usług za pomocą C#.
monikerRange: '>= aspnetcore-3.0'
ms.author: johluo
ms.date: 03/31/2019
uid: grpc/basics
ms.openlocfilehash: 00772144cb484b78a256f178642463577d316be2
ms.sourcegitcommit: 516f166c5f7cec54edf3d9c71e6e2ba53fb3b0e5
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/18/2019
ms.locfileid: "67196352"
---
# <a name="grpc-services-with-c"></a>gRPC usług przy użyciu języka C\#

W tym dokumencie opisano pojęcia są potrzebne do zapisania [gRPC](https://grpc.io/docs/guides/) aplikacje w C#. Tematy omówione w tym miejscu dotyczą zarówno [C-core](https://grpc.io/blog/grpc-stacks)-gRPC opartych na platformy ASP.NET Core i zależności aplikacji.

## <a name="proto-file"></a>proto pliku

gRPC korzysta z metody wymogiem wcześniejszego zawarcia kontraktu, do tworzenia interfejsu API. Domyślnie protokół buforów (protobuf) są używane jako język projektu interfejsu (IDL). *.Proto* plik zawiera:

* Definicja usługi gRPC.
* Komunikaty przesyłane między klientami a serwerami.

Aby uzyskać więcej informacji o składni protobuf plików, zobacz [oficjalnych dokumentów (protobuf)](https://developers.google.com/protocol-buffers/docs/proto3).

Na przykład, rozważmy *greet.proto* plik używany w [Rozpoczynanie pracy z usługą gRPC](xref:tutorials/grpc/grpc-start):

* Definiuje `Greeter` usługi.
* `Greeter` Usługa zawiera definicję `SayHello` wywołania.
* `SayHello` wysyła `HelloRequest` wiadomości i odbiera `HelloResponse` komunikat:

[!code-proto[](~/tutorials//grpc/grpc-start/sample/GrpcGreeter/Protos/greet.proto)]

## <a name="add-a-proto-file-to-a-c-app"></a>Dodaj plik .proto do C\# aplikacji

*.Proto* plik znajduje się w projekcie, dodając ją do `<Protobuf>` grupy elementów:

[!code-xml[](~/tutorials/grpc/grpc-start/sample/GrpcGreeter/GrpcGreeter.csproj?highlight=2&range=7-9)]

## <a name="c-tooling-support-for-proto-files"></a>C#Narzędzia do obsługi plików .proto

Pakiet narzędzi [Grpc.Tools](https://www.nuget.org/packages/Grpc.Tools/) jest wymagany do wygenerowania C# zasoby z *.proto* plików. Wygenerowanych elementów zawartości (plików):

* Są generowane na zgodnie z potrzebami każdorazowo, gdy projekt jest kompilowany.
* Nie są dodawane do projektu lub sprawdzone w formancie źródła.
* Czy artefakt kompilacji zawarte w *obj* katalogu.

Ten pakiet jest wymagane przez projekty serwera i klienta. `Grpc.Tools` można dodać przy użyciu Menedżera pakietów w programie Visual Studio lub dodając `<PackageReference>` do pliku projektu:

[!code-xml[](~/tutorials/grpc/grpc-start/sample/GrpcGreeter/GrpcGreeter.csproj?highlight=1&range=15)]

Pakiet narzędzi nie jest wymagane w czasie wykonywania, więc zależność jest oznaczona za pomocą `PrivateAssets="All"`.

## <a name="generated-c-assets"></a>Wygenerowany C# zasobów

Generuje pakiet narzędzi C# typy reprezentujące komunikatów zdefiniowany w dołączonej *.proto* plików.

Zasobów po stronie serwera generowany jest abstrakcyjny usługi typu podstawowego. Typ podstawowy zawiera definicje wszystkich wywołań gRPC zawarte w *.proto* pliku. Utwórz implementację konkretnych usług, pochodzi od tego typu podstawowego, który implementuje logikę dla wywołań gRPC. Dla `greet.proto`, przykład opisanej powyżej, abstrakcyjną `GreeterBase` typu, który zawiera wirtualny `SayHello` ma generowaną metodę. Konkretną implementację `GreeterService` zastępuje metodę i implementuje logikę obsługi wywołań gRPC.

[!code-csharp[](~/tutorials//grpc/grpc-start/sample/GrpcGreeter/Services/GreeterService.cs?name=snippet)]

Dla zasobów po stronie klienta jest generowany typu konkretnego klienta. Wywołuje gRPC *.proto* pliku są tłumaczone na konkretny typ, który można wywoływać metod. Aby uzyskać `greet.proto`, wcześniej opisywanym przykładzie konkretny `GreeterClient` typ jest generowany. Wywołaj `GreeterClient.SayHello` na zainicjowanie połączenia gRPC z serwera.

[!code-csharp[](~/tutorials//grpc/grpc-start/sample/GrpcGreeterClient/Program.cs?highlight=5-8&name=snippet)]

Domyślnie zasoby serwera i klienta są generowane dla każdego *.proto* plik dołączony `<Protobuf>` grupy elementów. Aby upewnić się, tylko zasoby serwera są generowane w projekcie serwera `GrpcServices` ma ustawioną wartość atrybutu `Server`.

[!code-xml[](~/tutorials//grpc/grpc-start/sample/GrpcGreeter/GrpcGreeter.csproj?highlight=2&range=7-9)]

Podobnie, ma ustawioną wartość atrybutu `Client` w projektach klienta.

## <a name="additional-resources"></a>Dodatkowe zasoby

* <xref:grpc/index>
* <xref:tutorials/grpc/grpc-start>
* <xref:grpc/aspnetcore>
* <xref:grpc/migration>
