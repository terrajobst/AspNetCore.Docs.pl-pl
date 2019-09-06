---
title: Usługi gRPC w środowisku C#
author: juntaoluo
description: Zapoznaj się z podstawowymi pojęciami dotyczącymi pisania usług gRPC Services za pomocą programu C#.
monikerRange: '>= aspnetcore-3.0'
ms.author: johluo
ms.date: 07/03/2019
uid: grpc/basics
ms.openlocfilehash: e17a4561f2d4f8ceccb293a8a8c237de58e4ee3c
ms.sourcegitcommit: 8b36f75b8931ae3f656e2a8e63572080adc78513
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 09/05/2019
ms.locfileid: "70310421"
---
# <a name="grpc-services-with-c"></a><span data-ttu-id="70b44-103">usługi gRPC w języku C\#</span><span class="sxs-lookup"><span data-stu-id="70b44-103">gRPC services with C\#</span></span>

<span data-ttu-id="70b44-104">W tym dokumencie przedstawiono koncepcje niezbędne do pisania aplikacji [gRPC](https://grpc.io/docs/guides/) w programie C#.</span><span class="sxs-lookup"><span data-stu-id="70b44-104">This document outlines the concepts needed to write [gRPC](https://grpc.io/docs/guides/) apps in C#.</span></span> <span data-ttu-id="70b44-105">Tematy omówione tutaj dotyczą zarówno aplikacji gRPC opartych na procesorze [C](https://grpc.io/blog/grpc-stacks), jak i na ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="70b44-105">The topics covered here apply to both [C-core](https://grpc.io/blog/grpc-stacks)-based and ASP.NET Core-based gRPC apps.</span></span>

## <a name="proto-file"></a><span data-ttu-id="70b44-106">plik PROTO</span><span class="sxs-lookup"><span data-stu-id="70b44-106">proto file</span></span>

<span data-ttu-id="70b44-107">gRPC używa podejścia pierwszego kontraktu do programowania interfejsu API.</span><span class="sxs-lookup"><span data-stu-id="70b44-107">gRPC uses a contract-first approach to API development.</span></span> <span data-ttu-id="70b44-108">Bufory protokołu (protobuf) są domyślnie używane jako język projektowania interfejsu (IDL).</span><span class="sxs-lookup"><span data-stu-id="70b44-108">Protocol buffers (protobuf) are used as the Interface Design Language (IDL) by default.</span></span> <span data-ttu-id="70b44-109">Plik  *.protozawiera\** :</span><span class="sxs-lookup"><span data-stu-id="70b44-109">The *\*.proto* file contains:</span></span>

* <span data-ttu-id="70b44-110">Definicja usługi gRPC.</span><span class="sxs-lookup"><span data-stu-id="70b44-110">The definition of the gRPC service.</span></span>
* <span data-ttu-id="70b44-111">Komunikaty wysyłane między klientami a serwerami.</span><span class="sxs-lookup"><span data-stu-id="70b44-111">The messages sent between clients and servers.</span></span>

<span data-ttu-id="70b44-112">Aby uzyskać więcej informacji na temat składni plików protobuf, zapoznaj się z [oficjalną dokumentacją (protobuf)](https://developers.google.com/protocol-buffers/docs/proto3).</span><span class="sxs-lookup"><span data-stu-id="70b44-112">For more information on the syntax of protobuf files, see the [official documentation (protobuf)](https://developers.google.com/protocol-buffers/docs/proto3).</span></span>

<span data-ttu-id="70b44-113">Rozważmy na przykład plik *Greeting. proto* używany w temacie [Rozpoczynanie pracy z usługą gRPC](xref:tutorials/grpc/grpc-start):</span><span class="sxs-lookup"><span data-stu-id="70b44-113">For example, consider the *greet.proto* file used in [Get started with gRPC service](xref:tutorials/grpc/grpc-start):</span></span>

* <span data-ttu-id="70b44-114">`Greeter` Definiuje usługę.</span><span class="sxs-lookup"><span data-stu-id="70b44-114">Defines a `Greeter` service.</span></span>
* <span data-ttu-id="70b44-115">`Greeter` Usługa`SayHello` definiuje wywołanie.</span><span class="sxs-lookup"><span data-stu-id="70b44-115">The `Greeter` service defines a `SayHello` call.</span></span>
* <span data-ttu-id="70b44-116">`SayHello`wysyła komunikat i odbiera `HelloReply`komunikat: `HelloRequest`</span><span class="sxs-lookup"><span data-stu-id="70b44-116">`SayHello` sends a `HelloRequest` message and receives a `HelloReply` message:</span></span>

[!code-protobuf[](~/tutorials/grpc/grpc-start/sample/GrpcGreeter/Protos/greet.proto)]

## <a name="add-a-proto-file-to-a-c-app"></a><span data-ttu-id="70b44-117">Dodawanie pliku. proto do aplikacji C\#</span><span class="sxs-lookup"><span data-stu-id="70b44-117">Add a .proto file to a C\# app</span></span>

<span data-ttu-id="70b44-118">Plik PROTO jest dołączany do projektu przez `<Protobuf>` dodanie go do grupy Items:  *\**</span><span class="sxs-lookup"><span data-stu-id="70b44-118">The *\*.proto* file is included in a project by adding it to the `<Protobuf>` item group:</span></span>

[!code-xml[](~/tutorials/grpc/grpc-start/sample/GrpcGreeter/GrpcGreeter.csproj?highlight=2&range=7-9)]

## <a name="c-tooling-support-for-proto-files"></a><span data-ttu-id="70b44-119">C#Obsługa narzędzi dla plików. proto</span><span class="sxs-lookup"><span data-stu-id="70b44-119">C# Tooling support for .proto files</span></span>

<span data-ttu-id="70b44-120">Pakiet narzędzi [GRPC. Tools](https://www.nuget.org/packages/Grpc.Tools/) jest wymagany do wygenerowania C# elementów zawartości z  *\*plików. proto* .</span><span class="sxs-lookup"><span data-stu-id="70b44-120">The tooling package [Grpc.Tools](https://www.nuget.org/packages/Grpc.Tools/) is required to generate the C# assets from *\*.proto* files.</span></span> <span data-ttu-id="70b44-121">Wygenerowane zasoby (pliki):</span><span class="sxs-lookup"><span data-stu-id="70b44-121">The generated assets (files):</span></span>

* <span data-ttu-id="70b44-122">Są generowane w przypadku, gdy jest to wymagane, za każdym razem, gdy projekt jest skompilowany.</span><span class="sxs-lookup"><span data-stu-id="70b44-122">Are generated on an as-needed basis each time the project is built.</span></span>
* <span data-ttu-id="70b44-123">Nie są dodawane do projektu ani zaewidencjonowane do kontroli źródła.</span><span class="sxs-lookup"><span data-stu-id="70b44-123">Aren't added to the project or checked into source control.</span></span>
* <span data-ttu-id="70b44-124">Czy artefakt kompilacji znajduje się w katalogu *obj* .</span><span class="sxs-lookup"><span data-stu-id="70b44-124">Are a build artifact contained in the *obj* directory.</span></span>

<span data-ttu-id="70b44-125">Ten pakiet jest wymagany przez projekty serwera i klienta.</span><span class="sxs-lookup"><span data-stu-id="70b44-125">This package is required by both the server and client projects.</span></span> <span data-ttu-id="70b44-126">Pakietbinding zawiera odwołanie do `Grpc.Tools`. `Grpc.AspNetCore`</span><span class="sxs-lookup"><span data-stu-id="70b44-126">The `Grpc.AspNetCore` metapackage includes a reference to `Grpc.Tools`.</span></span> <span data-ttu-id="70b44-127">Projekty serwera mogą `<PackageReference>` być `Grpc.AspNetCore` dodawane przy użyciu Menedżera pakietów w programie Visual Studio lub poprzez dodanie do pliku projektu:</span><span class="sxs-lookup"><span data-stu-id="70b44-127">Server projects can add `Grpc.AspNetCore` using the Package Manager in Visual Studio or by adding a `<PackageReference>` to the project file:</span></span>

[!code-xml[](~/tutorials/grpc/grpc-start/sample/GrpcGreeter/GrpcGreeter.csproj?highlight=1&range=12)]

<span data-ttu-id="70b44-128">Projekty klienta należy bezpośrednio odwoływać `Grpc.Tools` się do innych pakietów wymaganych do korzystania z klienta gRPC.</span><span class="sxs-lookup"><span data-stu-id="70b44-128">Client projects should directly reference `Grpc.Tools` alongside the other packages required to use the gRPC client.</span></span> <span data-ttu-id="70b44-129">Pakiet narzędzi nie jest wymagany w czasie wykonywania, dlatego zależność jest oznaczona przy użyciu `PrivateAssets="All"`:</span><span class="sxs-lookup"><span data-stu-id="70b44-129">The tooling package isn't required at runtime, so the dependency is marked with `PrivateAssets="All"`:</span></span>

[!code-xml[](~/tutorials/grpc/grpc-start/sample/GrpcGreeterClient/GrpcGreeterClient.csproj?highlight=3&range=9-11)]

## <a name="generated-c-assets"></a><span data-ttu-id="70b44-130">Wygenerowane C# zasoby</span><span class="sxs-lookup"><span data-stu-id="70b44-130">Generated C# assets</span></span>

<span data-ttu-id="70b44-131">Pakiet narzędzi generuje C# typy reprezentujące komunikaty zdefiniowane w zawartych  *\*plikach. proto* .</span><span class="sxs-lookup"><span data-stu-id="70b44-131">The tooling package generates the C# types representing the messages defined in the included *\*.proto* files.</span></span>

<span data-ttu-id="70b44-132">W przypadku zasobów po stronie serwera jest generowany abstrakcyjny typ podstawowy usługi.</span><span class="sxs-lookup"><span data-stu-id="70b44-132">For server-side assets, an abstract service base type is generated.</span></span> <span data-ttu-id="70b44-133">Typ podstawowy zawiera definicje wszystkich wywołań gRPC znajdujących się w pliku *. proto* .</span><span class="sxs-lookup"><span data-stu-id="70b44-133">The base type contains the definitions of all the gRPC calls contained in the *.proto* file.</span></span> <span data-ttu-id="70b44-134">Utwórz konkretną implementację usługi, która pochodzi z tego typu podstawowego i implementuje logikę dla wywołań gRPC.</span><span class="sxs-lookup"><span data-stu-id="70b44-134">Create a concrete service implementation that derives from this base type and implements the logic for the gRPC calls.</span></span> <span data-ttu-id="70b44-135">W przypadku `GreeterBase` `SayHello` , przykładu opisanego wcześniej, generowany jest typ abstrakcyjny, który zawiera metodę wirtualną. `greet.proto`</span><span class="sxs-lookup"><span data-stu-id="70b44-135">For the `greet.proto`, the example described previously, an abstract `GreeterBase` type that contains a virtual `SayHello` method is generated.</span></span> <span data-ttu-id="70b44-136">Konkretna implementacja `GreeterService` przesłania metodę i implementuje logikę obsługi wywołania gRPC.</span><span class="sxs-lookup"><span data-stu-id="70b44-136">A concrete implementation `GreeterService` overrides the method and implements the logic handling the gRPC call.</span></span>

[!code-csharp[](~/tutorials/grpc/grpc-start/sample/GrpcGreeter/Services/GreeterService.cs?name=snippet)]

<span data-ttu-id="70b44-137">Dla zasobów po stronie klienta jest generowany konkretny typ klienta.</span><span class="sxs-lookup"><span data-stu-id="70b44-137">For client-side assets, a concrete client type is generated.</span></span> <span data-ttu-id="70b44-138">Wywołania gRPC w pliku *. proto* są tłumaczone na metody w konkretnym typie, który można wywołać.</span><span class="sxs-lookup"><span data-stu-id="70b44-138">The gRPC calls in the *.proto* file are translated into methods on the concrete type, which can be called.</span></span> <span data-ttu-id="70b44-139">W przypadku `GreeterClient` , przykładu opisanego wcześniej, generowany jest konkretny typ. `greet.proto`</span><span class="sxs-lookup"><span data-stu-id="70b44-139">For the `greet.proto`, the example described previously, a concrete `GreeterClient` type is generated.</span></span> <span data-ttu-id="70b44-140">Wywołaj `GreeterClient.SayHelloAsync` , aby zainicjować wywołanie gRPC na serwerze.</span><span class="sxs-lookup"><span data-stu-id="70b44-140">Call `GreeterClient.SayHelloAsync` to initiate a gRPC call to the server.</span></span>

[!code-csharp[](~/tutorials/grpc/grpc-start/sample/GrpcGreeterClient/Program.cs?name=snippet)]

<span data-ttu-id="70b44-141">Domyślnie zasoby serwera i klienta są generowane dla każdego `<Protobuf>`  *\*pliku. proto* , który znajduje się w grupie elementów.</span><span class="sxs-lookup"><span data-stu-id="70b44-141">By default, server and client assets are generated for each *\*.proto* file included in the `<Protobuf>` item group.</span></span> <span data-ttu-id="70b44-142">Aby upewnić się, że tylko zasoby serwera są generowane w projekcie serwera `GrpcServices` , atrybut jest ustawiany `Server`na.</span><span class="sxs-lookup"><span data-stu-id="70b44-142">To ensure only the server assets are generated in a server project, the `GrpcServices` attribute is set to `Server`.</span></span>

[!code-xml[](~/tutorials/grpc/grpc-start/sample/GrpcGreeter/GrpcGreeter.csproj?highlight=2&range=7-9)]

<span data-ttu-id="70b44-143">Podobnie atrybut jest ustawiany na `Client` w projektach klientów.</span><span class="sxs-lookup"><span data-stu-id="70b44-143">Similarly, the attribute is set to `Client` in client projects.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="70b44-144">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="70b44-144">Additional resources</span></span>

* <xref:grpc/index>
* <xref:tutorials/grpc/grpc-start>
* <xref:grpc/aspnetcore>
* <xref:grpc/client>
