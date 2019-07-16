---
title: Usługi gRPC w środowisku C#
author: juntaoluo
description: Podstawowe informacje na temat podczas zapisywania gRPC usług za pomocą C#.
monikerRange: '>= aspnetcore-3.0'
ms.author: johluo
ms.date: 03/31/2019
uid: grpc/basics
ms.openlocfilehash: 78d744d641396c449a142375c69730333f8183cd
ms.sourcegitcommit: 1bf80f4acd62151ff8cce517f03f6fa891136409
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/15/2019
ms.locfileid: "68223883"
---
# <a name="grpc-services-with-c"></a><span data-ttu-id="4c0ae-103">gRPC usług przy użyciu języka C\#</span><span class="sxs-lookup"><span data-stu-id="4c0ae-103">gRPC services with C\#</span></span>

<span data-ttu-id="4c0ae-104">W tym dokumencie opisano pojęcia są potrzebne do zapisania [gRPC](https://grpc.io/docs/guides/) aplikacje w C#.</span><span class="sxs-lookup"><span data-stu-id="4c0ae-104">This document outlines the concepts needed to write [gRPC](https://grpc.io/docs/guides/) apps in C#.</span></span> <span data-ttu-id="4c0ae-105">Tematy omówione w tym miejscu dotyczą zarówno [C-core](https://grpc.io/blog/grpc-stacks)-gRPC opartych na platformy ASP.NET Core i zależności aplikacji.</span><span class="sxs-lookup"><span data-stu-id="4c0ae-105">The topics covered here apply to both [C-core](https://grpc.io/blog/grpc-stacks)-based and ASP.NET Core-based gRPC apps.</span></span>

## <a name="proto-file"></a><span data-ttu-id="4c0ae-106">proto pliku</span><span class="sxs-lookup"><span data-stu-id="4c0ae-106">proto file</span></span>

<span data-ttu-id="4c0ae-107">gRPC korzysta z metody wymogiem wcześniejszego zawarcia kontraktu, do tworzenia interfejsu API.</span><span class="sxs-lookup"><span data-stu-id="4c0ae-107">gRPC uses a contract-first approach to API development.</span></span> <span data-ttu-id="4c0ae-108">Domyślnie protokół buforów (protobuf) są używane jako język projektu interfejsu (IDL).</span><span class="sxs-lookup"><span data-stu-id="4c0ae-108">Protocol buffers (protobuf) are used as the Interface Design Language (IDL) by default.</span></span> <span data-ttu-id="4c0ae-109">*.Proto* plik zawiera:</span><span class="sxs-lookup"><span data-stu-id="4c0ae-109">The *.proto* file contains:</span></span>

* <span data-ttu-id="4c0ae-110">Definicja usługi gRPC.</span><span class="sxs-lookup"><span data-stu-id="4c0ae-110">The definition of the gRPC service.</span></span>
* <span data-ttu-id="4c0ae-111">Komunikaty przesyłane między klientami a serwerami.</span><span class="sxs-lookup"><span data-stu-id="4c0ae-111">The messages sent between clients and servers.</span></span>

<span data-ttu-id="4c0ae-112">Aby uzyskać więcej informacji o składni protobuf plików, zobacz [oficjalnych dokumentów (protobuf)](https://developers.google.com/protocol-buffers/docs/proto3).</span><span class="sxs-lookup"><span data-stu-id="4c0ae-112">For more information on the syntax of protobuf files, see the [official documentation (protobuf)](https://developers.google.com/protocol-buffers/docs/proto3).</span></span>

<span data-ttu-id="4c0ae-113">Na przykład, rozważmy *greet.proto* plik używany w [Rozpoczynanie pracy z usługą gRPC](xref:tutorials/grpc/grpc-start):</span><span class="sxs-lookup"><span data-stu-id="4c0ae-113">For example, consider the *greet.proto* file used in [Get started with gRPC service](xref:tutorials/grpc/grpc-start):</span></span>

* <span data-ttu-id="4c0ae-114">Definiuje `Greeter` usługi.</span><span class="sxs-lookup"><span data-stu-id="4c0ae-114">Defines a `Greeter` service.</span></span>
* <span data-ttu-id="4c0ae-115">`Greeter` Usługa zawiera definicję `SayHello` wywołania.</span><span class="sxs-lookup"><span data-stu-id="4c0ae-115">The `Greeter` service defines a `SayHello` call.</span></span>
* <span data-ttu-id="4c0ae-116">`SayHello` wysyła `HelloRequest` wiadomości i odbiera `HelloReply` komunikat:</span><span class="sxs-lookup"><span data-stu-id="4c0ae-116">`SayHello` sends a `HelloRequest` message and receives a `HelloReply` message:</span></span>

[!code-protobuf[](~/tutorials//grpc/grpc-start/sample/GrpcGreeter/Protos/greet.proto)]

## <a name="add-a-proto-file-to-a-c-app"></a><span data-ttu-id="4c0ae-117">Dodaj plik .proto do C\# aplikacji</span><span class="sxs-lookup"><span data-stu-id="4c0ae-117">Add a .proto file to a C\# app</span></span>

<span data-ttu-id="4c0ae-118">*.Proto* plik znajduje się w projekcie, dodając ją do `<Protobuf>` grupy elementów:</span><span class="sxs-lookup"><span data-stu-id="4c0ae-118">The *.proto* file is included in a project by adding it to the `<Protobuf>` item group:</span></span>

[!code-xml[](~/tutorials/grpc/grpc-start/sample/GrpcGreeter/GrpcGreeter.csproj?highlight=2&range=7-9)]

## <a name="c-tooling-support-for-proto-files"></a><span data-ttu-id="4c0ae-119">C#Narzędzia do obsługi plików .proto</span><span class="sxs-lookup"><span data-stu-id="4c0ae-119">C# Tooling support for .proto files</span></span>

<span data-ttu-id="4c0ae-120">Pakiet narzędzi [Grpc.Tools](https://www.nuget.org/packages/Grpc.Tools/) jest wymagany do wygenerowania C# zasoby z *.proto* plików.</span><span class="sxs-lookup"><span data-stu-id="4c0ae-120">The tooling package [Grpc.Tools](https://www.nuget.org/packages/Grpc.Tools/) is required to generate the C# assets from *.proto* files.</span></span> <span data-ttu-id="4c0ae-121">Wygenerowanych elementów zawartości (plików):</span><span class="sxs-lookup"><span data-stu-id="4c0ae-121">The generated assets (files):</span></span>

* <span data-ttu-id="4c0ae-122">Są generowane na zgodnie z potrzebami każdorazowo, gdy projekt jest kompilowany.</span><span class="sxs-lookup"><span data-stu-id="4c0ae-122">Are generated on an as-needed basis each time the project is built.</span></span>
* <span data-ttu-id="4c0ae-123">Nie są dodawane do projektu lub sprawdzone w formancie źródła.</span><span class="sxs-lookup"><span data-stu-id="4c0ae-123">Aren't added to the project or checked into source control.</span></span>
* <span data-ttu-id="4c0ae-124">Czy artefakt kompilacji zawarte w *obj* katalogu.</span><span class="sxs-lookup"><span data-stu-id="4c0ae-124">Are a build artifact contained in the *obj* directory.</span></span>

<span data-ttu-id="4c0ae-125">Ten pakiet jest wymagane przez projekty serwera i klienta.</span><span class="sxs-lookup"><span data-stu-id="4c0ae-125">This package is required by both the server and client projects.</span></span> <span data-ttu-id="4c0ae-126">`Grpc.Tools` można dodać przy użyciu Menedżera pakietów w programie Visual Studio lub dodając `<PackageReference>` do pliku projektu:</span><span class="sxs-lookup"><span data-stu-id="4c0ae-126">`Grpc.Tools` can be added by using the Package Manager in Visual Studio or adding a `<PackageReference>` to the project file:</span></span>

[!code-xml[](~/tutorials/grpc/grpc-start/sample/GrpcGreeter/GrpcGreeter.csproj?highlight=1&range=15)]

<span data-ttu-id="4c0ae-127">Pakiet narzędzi nie jest wymagane w czasie wykonywania, więc zależność jest oznaczona za pomocą `PrivateAssets="All"`.</span><span class="sxs-lookup"><span data-stu-id="4c0ae-127">The tooling package isn't required at runtime, so the dependency is marked with `PrivateAssets="All"`.</span></span>

## <a name="generated-c-assets"></a><span data-ttu-id="4c0ae-128">Wygenerowany C# zasobów</span><span class="sxs-lookup"><span data-stu-id="4c0ae-128">Generated C# assets</span></span>

<span data-ttu-id="4c0ae-129">Generuje pakiet narzędzi C# typy reprezentujące komunikatów zdefiniowany w dołączonej *.proto* plików.</span><span class="sxs-lookup"><span data-stu-id="4c0ae-129">The tooling package generates the C# types representing the messages defined in the included *.proto* files.</span></span>

<span data-ttu-id="4c0ae-130">Zasobów po stronie serwera generowany jest abstrakcyjny usługi typu podstawowego.</span><span class="sxs-lookup"><span data-stu-id="4c0ae-130">For server-side assets, an abstract service base type is generated.</span></span> <span data-ttu-id="4c0ae-131">Typ podstawowy zawiera definicje wszystkich wywołań gRPC zawarte w *.proto* pliku.</span><span class="sxs-lookup"><span data-stu-id="4c0ae-131">The base type contains the definitions of all the gRPC calls contained in the *.proto* file.</span></span> <span data-ttu-id="4c0ae-132">Utwórz implementację konkretnych usług, pochodzi od tego typu podstawowego, który implementuje logikę dla wywołań gRPC.</span><span class="sxs-lookup"><span data-stu-id="4c0ae-132">Create a concrete service implementation that derives from this base type and implements the logic for the gRPC calls.</span></span> <span data-ttu-id="4c0ae-133">Dla `greet.proto`, przykład opisanej powyżej, abstrakcyjną `GreeterBase` typu, który zawiera wirtualny `SayHello` ma generowaną metodę.</span><span class="sxs-lookup"><span data-stu-id="4c0ae-133">For the `greet.proto`, the example described previously, an abstract `GreeterBase` type that contains a virtual `SayHello` method is generated.</span></span> <span data-ttu-id="4c0ae-134">Konkretną implementację `GreeterService` zastępuje metodę i implementuje logikę obsługi wywołań gRPC.</span><span class="sxs-lookup"><span data-stu-id="4c0ae-134">A concrete implementation `GreeterService` overrides the method and implements the logic handling the gRPC call.</span></span>

[!code-csharp[](~/tutorials//grpc/grpc-start/sample/GrpcGreeter/Services/GreeterService.cs?name=snippet)]

<span data-ttu-id="4c0ae-135">Dla zasobów po stronie klienta jest generowany typu konkretnego klienta.</span><span class="sxs-lookup"><span data-stu-id="4c0ae-135">For client-side assets, a concrete client type is generated.</span></span> <span data-ttu-id="4c0ae-136">Wywołuje gRPC *.proto* pliku są tłumaczone na konkretny typ, który można wywoływać metod.</span><span class="sxs-lookup"><span data-stu-id="4c0ae-136">The gRPC calls in the *.proto* file are translated into methods on the concrete type, which can be called.</span></span> <span data-ttu-id="4c0ae-137">Aby uzyskać `greet.proto`, wcześniej opisywanym przykładzie konkretny `GreeterClient` typ jest generowany.</span><span class="sxs-lookup"><span data-stu-id="4c0ae-137">For the `greet.proto`, the example described previously, a concrete `GreeterClient` type is generated.</span></span> <span data-ttu-id="4c0ae-138">Wywołaj `GreeterClient.SayHello` na zainicjowanie połączenia gRPC z serwera.</span><span class="sxs-lookup"><span data-stu-id="4c0ae-138">Call `GreeterClient.SayHello` to initiate a gRPC call to the server.</span></span>

[!code-csharp[](~/tutorials//grpc/grpc-start/sample/GrpcGreeterClient/Program.cs?highlight=5-8&name=snippet)]

<span data-ttu-id="4c0ae-139">Domyślnie zasoby serwera i klienta są generowane dla każdego *.proto* plik dołączony `<Protobuf>` grupy elementów.</span><span class="sxs-lookup"><span data-stu-id="4c0ae-139">By default, server and client assets are generated for each *.proto* file included in the `<Protobuf>` item group.</span></span> <span data-ttu-id="4c0ae-140">Aby upewnić się, tylko zasoby serwera są generowane w projekcie serwera `GrpcServices` ma ustawioną wartość atrybutu `Server`.</span><span class="sxs-lookup"><span data-stu-id="4c0ae-140">To ensure only the server assets are generated in a server project, the `GrpcServices` attribute is set to `Server`.</span></span>

[!code-xml[](~/tutorials//grpc/grpc-start/sample/GrpcGreeter/GrpcGreeter.csproj?highlight=2&range=7-9)]

<span data-ttu-id="4c0ae-141">Podobnie, ma ustawioną wartość atrybutu `Client` w projektach klienta.</span><span class="sxs-lookup"><span data-stu-id="4c0ae-141">Similarly, the attribute is set to `Client` in client projects.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="4c0ae-142">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="4c0ae-142">Additional resources</span></span>

* <xref:grpc/index>
* <xref:tutorials/grpc/grpc-start>
* <xref:grpc/aspnetcore>
* <xref:grpc/migration>
