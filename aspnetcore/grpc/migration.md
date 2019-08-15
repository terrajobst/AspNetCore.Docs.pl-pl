---
title: Migrowanie usług gRPC z dysku C-Core do ASP.NET Core
author: juntaoluo
description: Dowiedz się, jak przenieść istniejącą aplikację gRPC opartą na procesorze C do uruchamiania na stosie ASP.NET Core.
monikerRange: '>= aspnetcore-3.0'
ms.author: johluo
ms.date: 03/31/2019
uid: grpc/migration
ms.openlocfilehash: 39aa711a1a47cf11ec5b08903b4130c7caa1501c
ms.sourcegitcommit: 476ea5ad86a680b7b017c6f32098acd3414c0f6c
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/14/2019
ms.locfileid: "69022302"
---
# <a name="migrating-grpc-services-from-c-core-to-aspnet-core"></a><span data-ttu-id="f1191-103">Migrowanie usług gRPC z dysku C-Core do ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="f1191-103">Migrating gRPC services from C-core to ASP.NET Core</span></span>

<span data-ttu-id="f1191-104">Przez [Jan Luo](https://github.com/juntaoluo)</span><span class="sxs-lookup"><span data-stu-id="f1191-104">By [John Luo](https://github.com/juntaoluo)</span></span>

<span data-ttu-id="f1191-105">Ze względu na implementację bazowego stosu nie wszystkie funkcje działają w taki sam sposób między aplikacjami [gRPC opartymi na](https://grpc.io/blog/grpc-stacks) procesorze C i aplikacjami opartymi na ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="f1191-105">Due to the implementation of the underlying stack, not all features work in the same way between [C-core-based gRPC](https://grpc.io/blog/grpc-stacks) apps and ASP.NET Core-based apps.</span></span> <span data-ttu-id="f1191-106">W tym dokumencie przedstawiono najważniejsze różnice dotyczące migracji między dwoma stosami.</span><span class="sxs-lookup"><span data-stu-id="f1191-106">This document highlights the key differences for migrating between the two stacks.</span></span>

## <a name="grpc-service-implementation-lifetime"></a><span data-ttu-id="f1191-107">okres istnienia implementacji usługi gRPC</span><span class="sxs-lookup"><span data-stu-id="f1191-107">gRPC service implementation lifetime</span></span>

<span data-ttu-id="f1191-108">W stosie ASP.NET Core usługi gRPC są domyślnie tworzone z okresem istnienia w [zakresie](xref:fundamentals/dependency-injection#service-lifetimes).</span><span class="sxs-lookup"><span data-stu-id="f1191-108">In the ASP.NET Core stack, gRPC services, by default, are created with a [scoped lifetime](xref:fundamentals/dependency-injection#service-lifetimes).</span></span> <span data-ttu-id="f1191-109">Z kolei gRPC C-Core domyślnie wiąże się z usługą z [pojedynczym okresem istnienia](xref:fundamentals/dependency-injection#service-lifetimes).</span><span class="sxs-lookup"><span data-stu-id="f1191-109">In contrast, gRPC C-core by default binds to a service with a [singleton lifetime](xref:fundamentals/dependency-injection#service-lifetimes).</span></span>

<span data-ttu-id="f1191-110">Okres istnienia w zakresie pozwala implementacji usługi rozwiązywać inne usługi z okresami istnienia w zakresie.</span><span class="sxs-lookup"><span data-stu-id="f1191-110">A scoped lifetime allows the service implementation to resolve other services with scoped lifetimes.</span></span> <span data-ttu-id="f1191-111">Na przykład okres istnienia objęty zakresem można `DbContext` również rozwiązać z kontenera di przez iniekcję konstruktora.</span><span class="sxs-lookup"><span data-stu-id="f1191-111">For example, a scoped lifetime can also resolve `DbContext` from the DI container through constructor injection.</span></span> <span data-ttu-id="f1191-112">Korzystanie z okresu istnienia w zakresie:</span><span class="sxs-lookup"><span data-stu-id="f1191-112">Using scoped lifetime:</span></span>

* <span data-ttu-id="f1191-113">Nowe wystąpienie implementacji usługi jest konstruowane dla każdego żądania.</span><span class="sxs-lookup"><span data-stu-id="f1191-113">A new instance of the service implementation is constructed for each request.</span></span>
* <span data-ttu-id="f1191-114">Nie jest możliwe udostępnianie stanu między żądaniami za pośrednictwem elementów członkowskich wystąpienia w typie implementacji.</span><span class="sxs-lookup"><span data-stu-id="f1191-114">It isn't possible to share state between requests via instance members on the implementation type.</span></span>
* <span data-ttu-id="f1191-115">Oczekuje się, że wszystkie Stany udostępnione są przechowywane w usłudze pojedynczej w kontenerze DI.</span><span class="sxs-lookup"><span data-stu-id="f1191-115">The expectation is to store shared states in a singleton service in the DI container.</span></span> <span data-ttu-id="f1191-116">Przechowywane udostępnione Stany są rozwiązywane w konstruktorze implementacji usługi gRPC.</span><span class="sxs-lookup"><span data-stu-id="f1191-116">The stored shared states are resolved in the constructor of the gRPC service implementation.</span></span>

<span data-ttu-id="f1191-117">Aby uzyskać więcej informacji na temat okresów istnienia usługi, zobacz <xref:fundamentals/dependency-injection#service-lifetimes>.</span><span class="sxs-lookup"><span data-stu-id="f1191-117">For more information on service lifetimes, see <xref:fundamentals/dependency-injection#service-lifetimes>.</span></span>

### <a name="add-a-singleton-service"></a><span data-ttu-id="f1191-118">Dodawanie pojedynczej usługi</span><span class="sxs-lookup"><span data-stu-id="f1191-118">Add a singleton service</span></span>

<span data-ttu-id="f1191-119">Aby ułatwić przejście z implementacji gRPC C-Core do ASP.NET Core, można zmienić okres istnienia usługi dla implementacji usługi z zakresu na pojedynczą.</span><span class="sxs-lookup"><span data-stu-id="f1191-119">To facilitate the transition from a gRPC C-core implementation to ASP.NET Core, it's possible to change the service lifetime of the service implementation from scoped to singleton.</span></span> <span data-ttu-id="f1191-120">Obejmuje to dodanie wystąpienia implementacji usługi do kontenera DI:</span><span class="sxs-lookup"><span data-stu-id="f1191-120">This involves adding an instance of the service implementation to the DI container:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddGrpc();
    services.AddSingleton(new GreeterService());
}
```

<span data-ttu-id="f1191-121">Jednak implementacja usługi z pojedynczym okresem istnienia nie jest już w stanie rozpoznać usług objętych zakresem przez iniekcję konstruktora.</span><span class="sxs-lookup"><span data-stu-id="f1191-121">However, a service implementation with a singleton lifetime is no longer able to resolve scoped services through constructor injection.</span></span>

## <a name="configure-grpc-services-options"></a><span data-ttu-id="f1191-122">Skonfiguruj opcje usług gRPC Services</span><span class="sxs-lookup"><span data-stu-id="f1191-122">Configure gRPC services options</span></span>

<span data-ttu-id="f1191-123">W przypadku aplikacji opartych na rdzeniu C ustawienia takie `grpc.max_receive_message_length` jak `grpc.max_send_message_length` i są konfigurowane `ChannelOption` przy użyciu programu podczas [konstruowania wystąpienia serwera](https://grpc.io/grpc/csharp/api/Grpc.Core.Server.html#Grpc_Core_Server__ctor_System_Collections_Generic_IEnumerable_Grpc_Core_ChannelOption__).</span><span class="sxs-lookup"><span data-stu-id="f1191-123">In C-core-based apps, settings such as `grpc.max_receive_message_length` and `grpc.max_send_message_length` are configured with `ChannelOption` when [constructing the Server instance](https://grpc.io/grpc/csharp/api/Grpc.Core.Server.html#Grpc_Core_Server__ctor_System_Collections_Generic_IEnumerable_Grpc_Core_ChannelOption__).</span></span>

<span data-ttu-id="f1191-124">W ASP.NET Core gRPC zapewnia konfigurację przy użyciu `GrpcServiceOptions` typu.</span><span class="sxs-lookup"><span data-stu-id="f1191-124">In ASP.NET Core, gRPC provides configuration through the `GrpcServiceOptions` type.</span></span> <span data-ttu-id="f1191-125">Na przykład maksymalny rozmiar komunikatu przychodzącego usługi gRPC można skonfigurować za pomocą `AddGrpc`następujących funkcji:</span><span class="sxs-lookup"><span data-stu-id="f1191-125">For example, a gRPC service's the maximum incoming message size can be configured via `AddGrpc`:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddGrpc(options =>
    {
        options.ReceiveMaxMessageSize = 16384; // 16 MB
    });
}
```

<span data-ttu-id="f1191-126">Aby uzyskać więcej informacji na temat konfiguracji <xref:grpc/configuration>, zobacz.</span><span class="sxs-lookup"><span data-stu-id="f1191-126">For more information on configuration, see <xref:grpc/configuration>.</span></span>

## <a name="logging"></a><span data-ttu-id="f1191-127">Rejestrowanie</span><span class="sxs-lookup"><span data-stu-id="f1191-127">Logging</span></span>

<span data-ttu-id="f1191-128">Aplikacje oparte na rdzeniu C są zależne `GrpcEnvironment` od programu w celu [skonfigurowania rejestratora](https://grpc.io/grpc/csharp/api/Grpc.Core.GrpcEnvironment.html?q=size#Grpc_Core_GrpcEnvironment_SetLogger_Grpc_Core_Logging_ILogger_) do celów debugowania.</span><span class="sxs-lookup"><span data-stu-id="f1191-128">C-core-based apps rely on the `GrpcEnvironment` to [configure the logger](https://grpc.io/grpc/csharp/api/Grpc.Core.GrpcEnvironment.html?q=size#Grpc_Core_GrpcEnvironment_SetLogger_Grpc_Core_Logging_ILogger_) for debugging purposes.</span></span> <span data-ttu-id="f1191-129">Stos ASP.NET Core udostępnia tę funkcję za pomocą [interfejsu API rejestrowania](xref:fundamentals/logging/index).</span><span class="sxs-lookup"><span data-stu-id="f1191-129">The ASP.NET Core stack provides this functionality through the [Logging API](xref:fundamentals/logging/index).</span></span> <span data-ttu-id="f1191-130">Na przykład można dodać Rejestrator do usługi gRPC za pośrednictwem iniekcji konstruktora:</span><span class="sxs-lookup"><span data-stu-id="f1191-130">For example, a logger can be added to the gRPC service via constructor injection:</span></span>

```csharp
public class GreeterService : Greeter.GreeterBase
{
    public GreeterService(ILogger<GreeterService> logger)
    {
    }
}
```

## <a name="https"></a><span data-ttu-id="f1191-131">HTTPS</span><span class="sxs-lookup"><span data-stu-id="f1191-131">HTTPS</span></span>

<span data-ttu-id="f1191-132">Aplikacje oparte na rdzeniu C konfigurują protokół HTTPS za pomocą [właściwości Server. Ports](https://grpc.io/grpc/csharp/api/Grpc.Core.Server.html#Grpc_Core_Server_Ports).</span><span class="sxs-lookup"><span data-stu-id="f1191-132">C-core-based apps configure HTTPS through the [Server.Ports property](https://grpc.io/grpc/csharp/api/Grpc.Core.Server.html#Grpc_Core_Server_Ports).</span></span> <span data-ttu-id="f1191-133">Podobne pojęcie służy do konfigurowania serwerów w ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="f1191-133">A similar concept is used to configure servers in ASP.NET Core.</span></span> <span data-ttu-id="f1191-134">Na przykład Kestrel używa [konfiguracji punktu końcowego](xref:fundamentals/servers/kestrel#endpoint-configuration) dla tej funkcji.</span><span class="sxs-lookup"><span data-stu-id="f1191-134">For example, Kestrel uses [endpoint configuration](xref:fundamentals/servers/kestrel#endpoint-configuration) for this functionality.</span></span>

## <a name="interceptors-and-middleware"></a><span data-ttu-id="f1191-135">Interceptory i oprogramowanie pośredniczące</span><span class="sxs-lookup"><span data-stu-id="f1191-135">Interceptors and Middleware</span></span>

<span data-ttu-id="f1191-136">ASP.NET Core [oprogramowanie pośredniczące](xref:fundamentals/middleware/index) oferuje podobne funkcje w porównaniu do przechwyceń w aplikacjach gRPC opartych na rdzeniach języka C.</span><span class="sxs-lookup"><span data-stu-id="f1191-136">ASP.NET Core [middleware](xref:fundamentals/middleware/index) offers similar functionalities compared to interceptors in C-core-based gRPC apps.</span></span> <span data-ttu-id="f1191-137">Oprogramowanie pośredniczące i Interceptory są koncepcyjnie takie same, jak w obu przypadkach są używane do konstruowania potoku, który obsługuje żądanie gRPC.</span><span class="sxs-lookup"><span data-stu-id="f1191-137">Middleware and interceptors are conceptually the same as both are used to construct a pipeline that handles a gRPC request.</span></span> <span data-ttu-id="f1191-138">Umożliwiają one wykonywanie zadań przed lub po następnym składniku w potoku.</span><span class="sxs-lookup"><span data-stu-id="f1191-138">They both allow work to be performed before or after the next component in the pipeline.</span></span> <span data-ttu-id="f1191-139">Jednak ASP.NET Core oprogramowanie pośredniczące działa na podstawowych komunikatach HTTP/2, podczas gdy Interceptory działają na warstwie abstrakcji gRPC przy użyciu [ServerCallContext](https://grpc.io/grpc/csharp/api/Grpc.Core.ServerCallContext.html).</span><span class="sxs-lookup"><span data-stu-id="f1191-139">However, ASP.NET Core middleware operates on the underlying HTTP/2 messages, while interceptors operate on the gRPC layer of abstraction using the [ServerCallContext](https://grpc.io/grpc/csharp/api/Grpc.Core.ServerCallContext.html).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="f1191-140">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="f1191-140">Additional resources</span></span>

* <xref:grpc/index>
* <xref:grpc/basics>
* <xref:grpc/aspnetcore>
* <xref:tutorials/grpc/grpc-start>
