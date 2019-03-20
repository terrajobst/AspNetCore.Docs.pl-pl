---
title: Usługi gRPC w środowisku C#
author: juntaoluo
description: Podstawowe informacje na temat podczas zapisywania gRPC usług za pomocą C#.
monikerRange: '>= aspnetcore-3.0'
ms.author: johluo
ms.date: 03/08/2019
uid: grpc/basics
ms.openlocfilehash: 936561a3ad04183aff4c3ba1c9b0e8ab20dcbe12
ms.sourcegitcommit: 57792e5f594db1574742588017c708350958bdf0
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/20/2019
ms.locfileid: "58264773"
---
# <a name="grpc-services-with-c"></a><span data-ttu-id="aac56-103">gRPC usług przy użyciu języka C\#</span><span class="sxs-lookup"><span data-stu-id="aac56-103">gRPC services with C\#</span></span>

<span data-ttu-id="aac56-104">W tym dokumencie przedstawiono podstawowe pojęcia, które są potrzebne do zapisania [gRPC](https://grpc.io/docs/guides/) aplikacje w C#.</span><span class="sxs-lookup"><span data-stu-id="aac56-104">This document outlines the basic concepts needed to write [gRPC](https://grpc.io/docs/guides/) apps in C#.</span></span> <span data-ttu-id="aac56-105">Tematy omówione w tym miejscu dotyczą zarówno [C-core](https://grpc.io/blog/grpc-stacks) i ASP.NET Core gRPC aplikacji.</span><span class="sxs-lookup"><span data-stu-id="aac56-105">The topics covered here apply to both [C-core](https://grpc.io/blog/grpc-stacks) based and ASP.NET Core based gRPC apps.</span></span>

## <a name="proto-file"></a><span data-ttu-id="aac56-106">proto pliku</span><span class="sxs-lookup"><span data-stu-id="aac56-106">proto file</span></span>

<span data-ttu-id="aac56-107">gRPC korzysta z metody wymogiem wcześniejszego zawarcia kontraktu, do tworzenia interfejsu API.</span><span class="sxs-lookup"><span data-stu-id="aac56-107">gRPC uses a contract-first approach to API development.</span></span> <span data-ttu-id="aac56-108">Domyślnie protokół buforów (protobuf) są używane jako język projektu interfejsu (IDL).</span><span class="sxs-lookup"><span data-stu-id="aac56-108">Protocol buffers (protobuf) are used as the Interface Design Language (IDL) by default.</span></span> <span data-ttu-id="aac56-109">*.Proto* plik zawiera:</span><span class="sxs-lookup"><span data-stu-id="aac56-109">The *.proto* file contains:</span></span>

* <span data-ttu-id="aac56-110">Definicja usługi gRPC.</span><span class="sxs-lookup"><span data-stu-id="aac56-110">The definition of the gRPC service.</span></span>
* <span data-ttu-id="aac56-111">Komunikaty przesyłane między klientami a serwerami.</span><span class="sxs-lookup"><span data-stu-id="aac56-111">The  messages sent between clients and servers.</span></span>

<span data-ttu-id="aac56-112">Aby uzyskać więcej informacji o składni protobuf plików, zobacz [oficjalnych dokumentów (protobuf)](https://developers.google.com/protocol-buffers/docs/proto3).</span><span class="sxs-lookup"><span data-stu-id="aac56-112">For more information on the syntax of protobuf files, see the [official documentation (protobuf)](https://developers.google.com/protocol-buffers/docs/proto3).</span></span>

<span data-ttu-id="aac56-113">Na przykład, rozważmy *greet.proto* plik używany w [Rozpoczynanie pracy z usługą gRPC](xref:tutorials/grpc/grpc-start):</span><span class="sxs-lookup"><span data-stu-id="aac56-113">For example, consider the *greet.proto* file used in [Get started with gRPC service](xref:tutorials/grpc/grpc-start):</span></span>

* <span data-ttu-id="aac56-114">Definiuje `Greeter` usługi.</span><span class="sxs-lookup"><span data-stu-id="aac56-114">Defines a `Greeter` service.</span></span>
* <span data-ttu-id="aac56-115">`Greeter` Usługa zawiera definicję `SayHello` wywołania.</span><span class="sxs-lookup"><span data-stu-id="aac56-115">The `Greeter` service defines a `SayHello` call.</span></span>
* <span data-ttu-id="aac56-116">`SayHello` wysyła `HelloRequest` wiadomości i odbiera `HelloResponse` komunikat:</span><span class="sxs-lookup"><span data-stu-id="aac56-116">`SayHello` sends a `HelloRequest` message and receives a `HelloResponse` message:</span></span>

[!code-proto[](~/tutorials/grpc/grpc-start/samples/GrpcStart/Protos/greet.proto)]

## <a name="add-a-proto-file-to-a-c-app"></a><span data-ttu-id="aac56-117">Dodaj plik .proto do C# aplikacji</span><span class="sxs-lookup"><span data-stu-id="aac56-117">Add a .proto file to a C# app</span></span>

<span data-ttu-id="aac56-118">*.Proto* plik znajduje się w projekcie, dodając ją do `<Protobuf>` grupy elementów:</span><span class="sxs-lookup"><span data-stu-id="aac56-118">The *.proto* file is included in a project by adding it to the `<Protobuf>` item group:</span></span>

[!code-xml[](~/tutorials/grpc/grpc-start/samples/GrpcStart/GrpcGreeter.Server/GrpcGreeter.Server.csproj?highlight=2&range=7-10)]

## <a name="c-tooling-support-for-proto-files"></a><span data-ttu-id="aac56-119">C#Narzędzia do obsługi plików .proto</span><span class="sxs-lookup"><span data-stu-id="aac56-119">C# Tooling support for .proto files</span></span>

<span data-ttu-id="aac56-120">Pakiet narzędzi [Grpc.Tools](https://www.nuget.org/packages/Grpc.Tools/) jest wymagany do wygenerowania C# zasoby z *.proto* plików.</span><span class="sxs-lookup"><span data-stu-id="aac56-120">The tooling package [Grpc.Tools](https://www.nuget.org/packages/Grpc.Tools/) is required to generate the C# assets from *.proto* files.</span></span> <span data-ttu-id="aac56-121">Wygenerowanych elementów zawartości (plików):</span><span class="sxs-lookup"><span data-stu-id="aac56-121">The generated assets (files):</span></span>

* <span data-ttu-id="aac56-122">Są generowane na podstawie potrzeb jako każdym razem, gdy projekt jest kompilowany.</span><span class="sxs-lookup"><span data-stu-id="aac56-122">Are generated on a as-needed basis each time the project is built.</span></span>
* <span data-ttu-id="aac56-123">Nie są dodawane do projektu lub sprawdzone w formancie źródła.</span><span class="sxs-lookup"><span data-stu-id="aac56-123">Are not added to the project or checked into source control.</span></span>
* <span data-ttu-id="aac56-124">Czy artefakt kompilacji zawarte w *obj* katalogu.</span><span class="sxs-lookup"><span data-stu-id="aac56-124">Are a build artifact contained in the *obj* directory.</span></span>

<span data-ttu-id="aac56-125">Ten pakiet jest wymagane przez projekty serwera i klienta.</span><span class="sxs-lookup"><span data-stu-id="aac56-125">This package is required by both the server and client projects.</span></span> <span data-ttu-id="aac56-126">`Grpc.Tools` można dodać przy użyciu Menedżera pakietów w programie Visual Studio lub dodając `<PackageReference>` do pliku projektu:</span><span class="sxs-lookup"><span data-stu-id="aac56-126">`Grpc.Tools` can be added by using the Package Manager in Visual Studio or adding a `<PackageReference>` to the project file:</span></span>

[!code-xml[](~/tutorials/grpc/grpc-start/samples/GrpcStart/GrpcGreeter.Server/GrpcGreeter.Server.csproj?highlight=1&range=16)]

<span data-ttu-id="aac56-127">Pakiet narzędzi nie jest wymagany w czasie wykonywania, w związku z tym, zależność powinna być oznaczona jako za pomocą `PrivateAssets="All"`.</span><span class="sxs-lookup"><span data-stu-id="aac56-127">The tooling package is not required at runtime, therefore, the dependency should be marked with `PrivateAssets="All"`.</span></span>

## <a name="generated-c-assets"></a><span data-ttu-id="aac56-128">Wygenerowany C# zasobów</span><span class="sxs-lookup"><span data-stu-id="aac56-128">Generated C# assets</span></span>

<span data-ttu-id="aac56-129">Pakiet narzędzi wygeneruje C# typy reprezentujące komunikatów zdefiniowany w dołączonej *.proto* plików.</span><span class="sxs-lookup"><span data-stu-id="aac56-129">The tooling package will generate the C# types representing the messages defined in the included *.proto* files.</span></span>

<span data-ttu-id="aac56-130">Zasoby po stronie serwera generowany jest abstrakcyjny usługi typu podstawowego.</span><span class="sxs-lookup"><span data-stu-id="aac56-130">For server side assets, an abstract service base type is generated.</span></span> <span data-ttu-id="aac56-131">Typ podstawowy zawiera definicje wszystkich wywołań gRPC zawarte w *.proto* pliku.</span><span class="sxs-lookup"><span data-stu-id="aac56-131">The base type contains the definitions of all the gRPC calls contained in the *.proto* file.</span></span> <span data-ttu-id="aac56-132">Następnie utwórz konkretny implementacji usługi pochodzi od tego typu podstawowego i implementuje logikę wywołania gRPC.</span><span class="sxs-lookup"><span data-stu-id="aac56-132">You then create a concrete service implementation derives from this base type and implements the logic for the gRPC calls.</span></span> <span data-ttu-id="aac56-133">Dla `greet.proto` przykład opisanej powyżej, abstrakcyjną `GreeterBase` typu, który zawiera wirtualny `SayHello` ma generowaną metodę.</span><span class="sxs-lookup"><span data-stu-id="aac56-133">For the `greet.proto` example described previously, an abstract `GreeterBase` type that contains a virtual `SayHello` method is generated.</span></span> <span data-ttu-id="aac56-134">Konkretną implementację `GreeterService` zastępuje metodę i implementuje logikę obsługi wywołań gRPC.</span><span class="sxs-lookup"><span data-stu-id="aac56-134">A concrete implementation `GreeterService` overrides the method and implements the logic handling the gRPC call.</span></span>

[!code-cs[](~/tutorials/grpc/grpc-start/samples/GrpcStart/GrpcGreeter.Server/Services/GreeterService.cs?name=snippet)]

<span data-ttu-id="aac56-135">Zasoby po stronie klienta generowany jest typu konkretnego klienta.</span><span class="sxs-lookup"><span data-stu-id="aac56-135">For client side assets, a concrete client type is generated.</span></span> <span data-ttu-id="aac56-136">Wywołuje gRPC *.proto* pliku są tłumaczone na konkretny typ, który można wywoływać metod.</span><span class="sxs-lookup"><span data-stu-id="aac56-136">The gRPC calls in the *.proto* file are translated to methods on the concrete type which can be called.</span></span> <span data-ttu-id="aac56-137">Aby uzyskać `greet.proto` wcześniej opisywanym przykładzie konkretny `GreeterClient` typ jest generowany.</span><span class="sxs-lookup"><span data-stu-id="aac56-137">For the `greet.proto` example described previously, a concrete `GreeterClient` type is generated.</span></span> <span data-ttu-id="aac56-138">`GreeterClient` Typ zawiera `SayHello` metodę, która może być wywoływana na zainicjowanie połączenia gRPC z serwera.</span><span class="sxs-lookup"><span data-stu-id="aac56-138">The `GreeterClient` type contains a `SayHello` method that can be called to initiate a gRPC call to the server.</span></span>

[!code-cs[](~/tutorials/grpc/grpc-start/samples/GrpcStart/GrpcGreeter.Client/Program.cs?highlight=9-11&name=snippet)]

<span data-ttu-id="aac56-139">Domyślnie zasoby serwera i klienta są generowane dla każdego *.proto* plik dołączony `<Protobuf>` grupy elementów.</span><span class="sxs-lookup"><span data-stu-id="aac56-139">By default, both server and client assets are generated for each *.proto* file included in the `<Protobuf>` item group.</span></span> <span data-ttu-id="aac56-140">Aby upewnić się, tylko zasoby serwera są generowane w projekcie serwera `GrpcServices` ma ustawioną wartość atrybutu `Server`.</span><span class="sxs-lookup"><span data-stu-id="aac56-140">To ensure only the server assets are generated in a server project, the `GrpcServices` attribute is set to `Server`.</span></span>

[!code-xml[](~/tutorials/grpc/grpc-start/samples/GrpcStart/GrpcGreeter.Server/GrpcGreeter.Server.csproj?highlight=2&range=7-10)]

<span data-ttu-id="aac56-141">Podobnie, ma ustawioną wartość atrybutu `Client` w projektach klienta.</span><span class="sxs-lookup"><span data-stu-id="aac56-141">Similarly, the attribute is set to `Client` in client projects.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="aac56-142">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="aac56-142">Additional resources</span></span>

* <xref:grpc/index>
* <xref:tutorials/grpc/grpc-start>
* <xref:grpc/aspnetcore>
* <xref:grpc/migration>
