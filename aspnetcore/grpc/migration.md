---
title: Migrowanie usług gRPC z dysku C-Core do ASP.NET Core
author: juntaoluo
description: Dowiedz się, jak przenieść istniejącą aplikację gRPC opartą na procesorze C do uruchamiania na stosie ASP.NET Core.
monikerRange: '>= aspnetcore-3.0'
ms.author: johluo
ms.date: 09/25/2019
uid: grpc/migration
ms.openlocfilehash: 596eca0f510387a18472eb353672980e0a8e0d24
ms.sourcegitcommit: eb4fcdeb2f9e8413117624de42841a4997d1d82d
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/21/2019
ms.locfileid: "72697997"
---
# <a name="migrating-grpc-services-from-c-core-to-aspnet-core"></a><span data-ttu-id="935ef-103">Migrowanie usług gRPC z dysku C-Core do ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="935ef-103">Migrating gRPC services from C-core to ASP.NET Core</span></span>

<span data-ttu-id="935ef-104">Przez [Jan Luo](https://github.com/juntaoluo)</span><span class="sxs-lookup"><span data-stu-id="935ef-104">By [John Luo](https://github.com/juntaoluo)</span></span>

<span data-ttu-id="935ef-105">Ze względu na implementację bazowego stosu nie wszystkie funkcje działają w taki sam sposób między aplikacjami [gRPC opartymi na](https://grpc.io/blog/grpc-stacks) procesorze C i aplikacjami opartymi na ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="935ef-105">Due to the implementation of the underlying stack, not all features work in the same way between [C-core-based gRPC](https://grpc.io/blog/grpc-stacks) apps and ASP.NET Core-based apps.</span></span> <span data-ttu-id="935ef-106">W tym dokumencie przedstawiono najważniejsze różnice dotyczące migracji między dwoma stosami.</span><span class="sxs-lookup"><span data-stu-id="935ef-106">This document highlights the key differences for migrating between the two stacks.</span></span>

## <a name="grpc-service-implementation-lifetime"></a><span data-ttu-id="935ef-107">okres istnienia implementacji usługi gRPC</span><span class="sxs-lookup"><span data-stu-id="935ef-107">gRPC service implementation lifetime</span></span>

<span data-ttu-id="935ef-108">W stosie ASP.NET Core usługi gRPC są domyślnie tworzone z [okresem istnienia w zakresie](xref:fundamentals/dependency-injection#service-lifetimes).</span><span class="sxs-lookup"><span data-stu-id="935ef-108">In the ASP.NET Core stack, gRPC services, by default, are created with a [scoped lifetime](xref:fundamentals/dependency-injection#service-lifetimes).</span></span> <span data-ttu-id="935ef-109">Z kolei gRPC C-Core domyślnie wiąże się z usługą z [pojedynczym okresem istnienia](xref:fundamentals/dependency-injection#service-lifetimes).</span><span class="sxs-lookup"><span data-stu-id="935ef-109">In contrast, gRPC C-core by default binds to a service with a [singleton lifetime](xref:fundamentals/dependency-injection#service-lifetimes).</span></span>

<span data-ttu-id="935ef-110">Okres istnienia w zakresie pozwala implementacji usługi rozwiązywać inne usługi z okresami istnienia w zakresie.</span><span class="sxs-lookup"><span data-stu-id="935ef-110">A scoped lifetime allows the service implementation to resolve other services with scoped lifetimes.</span></span> <span data-ttu-id="935ef-111">Na przykład okres istnienia w zakresie może również rozwiązać `DbContext` z kontenera DI przez iniekcję konstruktora.</span><span class="sxs-lookup"><span data-stu-id="935ef-111">For example, a scoped lifetime can also resolve `DbContext` from the DI container through constructor injection.</span></span> <span data-ttu-id="935ef-112">Korzystanie z okresu istnienia w zakresie:</span><span class="sxs-lookup"><span data-stu-id="935ef-112">Using scoped lifetime:</span></span>

* <span data-ttu-id="935ef-113">Nowe wystąpienie implementacji usługi jest konstruowane dla każdego żądania.</span><span class="sxs-lookup"><span data-stu-id="935ef-113">A new instance of the service implementation is constructed for each request.</span></span>
* <span data-ttu-id="935ef-114">Nie jest możliwe udostępnianie stanu między żądaniami za pośrednictwem elementów członkowskich wystąpienia w typie implementacji.</span><span class="sxs-lookup"><span data-stu-id="935ef-114">It isn't possible to share state between requests via instance members on the implementation type.</span></span>
* <span data-ttu-id="935ef-115">Oczekuje się, że wszystkie Stany udostępnione są przechowywane w usłudze pojedynczej w kontenerze DI.</span><span class="sxs-lookup"><span data-stu-id="935ef-115">The expectation is to store shared states in a singleton service in the DI container.</span></span> <span data-ttu-id="935ef-116">Przechowywane udostępnione Stany są rozwiązywane w konstruktorze implementacji usługi gRPC.</span><span class="sxs-lookup"><span data-stu-id="935ef-116">The stored shared states are resolved in the constructor of the gRPC service implementation.</span></span>

<span data-ttu-id="935ef-117">Aby uzyskać więcej informacji na temat okresów istnienia usługi, zobacz <xref:fundamentals/dependency-injection#service-lifetimes>.</span><span class="sxs-lookup"><span data-stu-id="935ef-117">For more information on service lifetimes, see <xref:fundamentals/dependency-injection#service-lifetimes>.</span></span>

### <a name="add-a-singleton-service"></a><span data-ttu-id="935ef-118">Dodawanie pojedynczej usługi</span><span class="sxs-lookup"><span data-stu-id="935ef-118">Add a singleton service</span></span>

<span data-ttu-id="935ef-119">Aby ułatwić przejście z implementacji gRPC C-Core do ASP.NET Core, można zmienić okres istnienia usługi dla implementacji usługi z zakresu na pojedynczą.</span><span class="sxs-lookup"><span data-stu-id="935ef-119">To facilitate the transition from a gRPC C-core implementation to ASP.NET Core, it's possible to change the service lifetime of the service implementation from scoped to singleton.</span></span> <span data-ttu-id="935ef-120">Obejmuje to dodanie wystąpienia implementacji usługi do kontenera DI:</span><span class="sxs-lookup"><span data-stu-id="935ef-120">This involves adding an instance of the service implementation to the DI container:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddGrpc();
    services.AddSingleton(new GreeterService());
}
```

<span data-ttu-id="935ef-121">Jednak implementacja usługi z pojedynczym okresem istnienia nie jest już w stanie rozpoznać usług objętych zakresem przez iniekcję konstruktora.</span><span class="sxs-lookup"><span data-stu-id="935ef-121">However, a service implementation with a singleton lifetime is no longer able to resolve scoped services through constructor injection.</span></span>

## <a name="configure-grpc-services-options"></a><span data-ttu-id="935ef-122">Skonfiguruj opcje usług gRPC Services</span><span class="sxs-lookup"><span data-stu-id="935ef-122">Configure gRPC services options</span></span>

<span data-ttu-id="935ef-123">W przypadku aplikacji opartych na rdzeniu C ustawienia, takie jak `grpc.max_receive_message_length` i `grpc.max_send_message_length`, są konfigurowane przy użyciu `ChannelOption` podczas [konstruowania wystąpienia serwera](https://grpc.io/grpc/csharp/api/Grpc.Core.Server.html#Grpc_Core_Server__ctor_System_Collections_Generic_IEnumerable_Grpc_Core_ChannelOption__).</span><span class="sxs-lookup"><span data-stu-id="935ef-123">In C-core-based apps, settings such as `grpc.max_receive_message_length` and `grpc.max_send_message_length` are configured with `ChannelOption` when [constructing the Server instance](https://grpc.io/grpc/csharp/api/Grpc.Core.Server.html#Grpc_Core_Server__ctor_System_Collections_Generic_IEnumerable_Grpc_Core_ChannelOption__).</span></span>

<span data-ttu-id="935ef-124">W ASP.NET Core gRPC zapewnia konfigurację za pomocą typu `GrpcServiceOptions`.</span><span class="sxs-lookup"><span data-stu-id="935ef-124">In ASP.NET Core, gRPC provides configuration through the `GrpcServiceOptions` type.</span></span> <span data-ttu-id="935ef-125">Na przykład maksymalny rozmiar komunikatu przychodzącego usługi gRPC można skonfigurować za pośrednictwem `AddGrpc`.</span><span class="sxs-lookup"><span data-stu-id="935ef-125">For example, a gRPC service's the maximum incoming message size can be configured via `AddGrpc`.</span></span> <span data-ttu-id="935ef-126">Poniższy przykład zmienia domyślne `MaxReceiveMessageSize` z 4 MB na 16 MB:</span><span class="sxs-lookup"><span data-stu-id="935ef-126">The following example changes the default `MaxReceiveMessageSize` of 4 MB to 16 MB:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddGrpc(options =>
    {
        options.MaxReceiveMessageSize = 16 * 1024 * 1024; // 16 MB
    });
}
```

<span data-ttu-id="935ef-127">Aby uzyskać więcej informacji na temat konfiguracji, zobacz <xref:grpc/configuration>.</span><span class="sxs-lookup"><span data-stu-id="935ef-127">For more information on configuration, see <xref:grpc/configuration>.</span></span>

## <a name="logging"></a><span data-ttu-id="935ef-128">Rejestrowanie</span><span class="sxs-lookup"><span data-stu-id="935ef-128">Logging</span></span>

<span data-ttu-id="935ef-129">Aplikacje oparte na rdzeniu C są zależne od `GrpcEnvironment` [konfigurowania rejestratora](https://grpc.io/grpc/csharp/api/Grpc.Core.GrpcEnvironment.html?q=size#Grpc_Core_GrpcEnvironment_SetLogger_Grpc_Core_Logging_ILogger_) do celów debugowania.</span><span class="sxs-lookup"><span data-stu-id="935ef-129">C-core-based apps rely on the `GrpcEnvironment` to [configure the logger](https://grpc.io/grpc/csharp/api/Grpc.Core.GrpcEnvironment.html?q=size#Grpc_Core_GrpcEnvironment_SetLogger_Grpc_Core_Logging_ILogger_) for debugging purposes.</span></span> <span data-ttu-id="935ef-130">Stos ASP.NET Core udostępnia tę funkcję za pomocą [interfejsu API rejestrowania](xref:fundamentals/logging/index).</span><span class="sxs-lookup"><span data-stu-id="935ef-130">The ASP.NET Core stack provides this functionality through the [Logging API](xref:fundamentals/logging/index).</span></span> <span data-ttu-id="935ef-131">Na przykład można dodać Rejestrator do usługi gRPC za pośrednictwem iniekcji konstruktora:</span><span class="sxs-lookup"><span data-stu-id="935ef-131">For example, a logger can be added to the gRPC service via constructor injection:</span></span>

```csharp
public class GreeterService : Greeter.GreeterBase
{
    public GreeterService(ILogger<GreeterService> logger)
    {
    }
}
```

## <a name="https"></a><span data-ttu-id="935ef-132">HTTPS</span><span class="sxs-lookup"><span data-stu-id="935ef-132">HTTPS</span></span>

<span data-ttu-id="935ef-133">Aplikacje oparte na rdzeniu C konfigurują protokół HTTPS za pomocą [właściwości Server. Ports](https://grpc.io/grpc/csharp/api/Grpc.Core.Server.html#Grpc_Core_Server_Ports).</span><span class="sxs-lookup"><span data-stu-id="935ef-133">C-core-based apps configure HTTPS through the [Server.Ports property](https://grpc.io/grpc/csharp/api/Grpc.Core.Server.html#Grpc_Core_Server_Ports).</span></span> <span data-ttu-id="935ef-134">Podobne pojęcie służy do konfigurowania serwerów w ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="935ef-134">A similar concept is used to configure servers in ASP.NET Core.</span></span> <span data-ttu-id="935ef-135">Na przykład Kestrel używa [konfiguracji punktu końcowego](xref:fundamentals/servers/kestrel#endpoint-configuration) dla tej funkcji.</span><span class="sxs-lookup"><span data-stu-id="935ef-135">For example, Kestrel uses [endpoint configuration](xref:fundamentals/servers/kestrel#endpoint-configuration) for this functionality.</span></span>

## <a name="interceptors-and-middleware"></a><span data-ttu-id="935ef-136">Interceptory i oprogramowanie pośredniczące</span><span class="sxs-lookup"><span data-stu-id="935ef-136">Interceptors and Middleware</span></span>

<span data-ttu-id="935ef-137">ASP.NET Core [oprogramowanie pośredniczące](xref:fundamentals/middleware/index) oferuje podobne funkcje w porównaniu do przechwyceń w aplikacjach gRPC opartych na rdzeniach języka C.</span><span class="sxs-lookup"><span data-stu-id="935ef-137">ASP.NET Core [middleware](xref:fundamentals/middleware/index) offers similar functionalities compared to interceptors in C-core-based gRPC apps.</span></span> <span data-ttu-id="935ef-138">Oprogramowanie pośredniczące i Interceptory są koncepcyjnie takie same, jak w obu przypadkach są używane do konstruowania potoku, który obsługuje żądanie gRPC.</span><span class="sxs-lookup"><span data-stu-id="935ef-138">Middleware and interceptors are conceptually the same as both are used to construct a pipeline that handles a gRPC request.</span></span> <span data-ttu-id="935ef-139">Umożliwiają one wykonywanie zadań przed lub po następnym składniku w potoku.</span><span class="sxs-lookup"><span data-stu-id="935ef-139">They both allow work to be performed before or after the next component in the pipeline.</span></span> <span data-ttu-id="935ef-140">Jednak ASP.NET Core oprogramowanie pośredniczące działa na podstawowych komunikatach HTTP/2, podczas gdy Interceptory działają na warstwie abstrakcji gRPC przy użyciu [ServerCallContext](https://grpc.io/grpc/csharp/api/Grpc.Core.ServerCallContext.html).</span><span class="sxs-lookup"><span data-stu-id="935ef-140">However, ASP.NET Core middleware operates on the underlying HTTP/2 messages, while interceptors operate on the gRPC layer of abstraction using the [ServerCallContext](https://grpc.io/grpc/csharp/api/Grpc.Core.ServerCallContext.html).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="935ef-141">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="935ef-141">Additional resources</span></span>

* <xref:grpc/index>
* <xref:grpc/basics>
* <xref:grpc/aspnetcore>
* <xref:tutorials/grpc/grpc-start>
