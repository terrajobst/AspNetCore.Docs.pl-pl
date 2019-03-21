---
title: Migrowanie usług gRPC podstawowe języka C do platformy ASP.NET Core
author: juntaoluo
description: Dowiedz się, jak przenieść istniejącą aplikację na podstawie gRPC podstawowe języka C do uruchamiania na szczycie stosu platformy ASP.NET Core.
monikerRange: '>= aspnetcore-3.0'
ms.author: johluo
ms.date: 03/08/2019
uid: grpc/migration
ms.openlocfilehash: 3c6e04694a33e953f6e1575f5ee9b0699cf1cdd3
ms.sourcegitcommit: 5f299daa7c8102d56a63b214b9a34cc4bc87bc42
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/19/2019
ms.locfileid: "58207956"
---
# <a name="migrating-grpc-services-from-c-core-to-aspnet-core"></a><span data-ttu-id="8f875-103">Migrowanie usług gRPC podstawowe języka C do platformy ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="8f875-103">Migrating gRPC services from C-core to ASP.NET Core</span></span>

<span data-ttu-id="8f875-104">Przez [Luo Jan](https://github.com/juntaoluo)</span><span class="sxs-lookup"><span data-stu-id="8f875-104">By [John Luo](https://github.com/juntaoluo)</span></span>

<span data-ttu-id="8f875-105">Ze względu na implementację podstawowego stosu, nie wszystkie funkcje działają tak samo jak między [C-core na podstawie gRPC](https://grpc.io/blog/grpc-stacks) aplikacji opartych na aplikacji i ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="8f875-105">Due to implementation of the underlying stack, not all features work in the same way between [C-core based gRPC](https://grpc.io/blog/grpc-stacks) apps and ASP.NET Core based apps.</span></span> <span data-ttu-id="8f875-106">W tym dokumencie podświetla podstawowe różnice należy pamiętać podczas migracji między dwoma stosami.</span><span class="sxs-lookup"><span data-stu-id="8f875-106">This document highlights the key differences to note when migrating between the two stacks.</span></span>

## <a name="grpc-service-implementation-lifetime"></a><span data-ttu-id="8f875-107">okres istnienia implementacji usługi gRPC</span><span class="sxs-lookup"><span data-stu-id="8f875-107">gRPC service implementation lifetime</span></span>

<span data-ttu-id="8f875-108">W stosie platformy ASP.NET Core gRPC services domyślnie, zostanie utworzony przy użyciu [okres istnienia polu](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="8f875-108">In the ASP.NET Core stack, gRPC services, by default, will be created with a [Scoped lifetime](xref:fundamentals/dependency-injection).</span></span> <span data-ttu-id="8f875-109">Z kolei gRPC C-core domyślnie wiąże się z usługą z okresem istnienia pojedynczego wystąpienia.</span><span class="sxs-lookup"><span data-stu-id="8f875-109">In contrast, gRPC C-core by default binds to a service with a Singleton lifetime.</span></span>

<span data-ttu-id="8f875-110">Okres istnienia polu umożliwia implementacji usługi można rozpoznać innych usług przy użyciu polu okresy istnienia.</span><span class="sxs-lookup"><span data-stu-id="8f875-110">A Scoped lifetime allows the service implementation to resolve other services with Scoped lifetimes.</span></span> <span data-ttu-id="8f875-111">Na przykład może być także rozwiązać okres istnienia polu `DBContext`, z kontenera DI przy użyciu iniekcji konstruktora.</span><span class="sxs-lookup"><span data-stu-id="8f875-111">For example Scoped lifetime can also resolve `DBContext`, from the DI container through constructor injection.</span></span> <span data-ttu-id="8f875-112">Korzystanie z okresem istnienia polu:</span><span class="sxs-lookup"><span data-stu-id="8f875-112">Using Scoped lifetime:</span></span>

* <span data-ttu-id="8f875-113">Nowe wystąpienie implementacji usługi jest tworzona dla każdego żądania.</span><span class="sxs-lookup"><span data-stu-id="8f875-113">A new instance of the service implementation is constructed for each request.</span></span>
* <span data-ttu-id="8f875-114">Nie jest możliwe udostępnianie stanu między żądaniami, za pośrednictwem składowych wystąpień dla typu wdrożenia.</span><span class="sxs-lookup"><span data-stu-id="8f875-114">It's not possible to share state between requests via instance members on the implementation type.</span></span>
* <span data-ttu-id="8f875-115">Oczekuje się, aby przechowywać udostępnionych stanów usługi Singleton w kontenerze DI.</span><span class="sxs-lookup"><span data-stu-id="8f875-115">The expectation is to store shared states in a Singleton service in the DI container.</span></span> <span data-ttu-id="8f875-116">Przechowywane udostępnionych stanów są rozwiązywane w Konstruktorze gRPC implementacji usługi.</span><span class="sxs-lookup"><span data-stu-id="8f875-116">The stored shared states are resolved in the constructor of the gRPC service implementation.</span></span> 

<span data-ttu-id="8f875-117">Aby uzyskać więcej informacji o polu i pojedyncze okres istnienia, zobacz <xref:fundamentals/dependency-injection>.</span><span class="sxs-lookup"><span data-stu-id="8f875-117">For more information on Scoped and Singleton lifetime, see <xref:fundamentals/dependency-injection>.</span></span>

### <a name="add-a-singleton-service"></a><span data-ttu-id="8f875-118">Dodawanie usługi singleton</span><span class="sxs-lookup"><span data-stu-id="8f875-118">Add a singleton service</span></span>

<span data-ttu-id="8f875-119">Aby ułatwić przejście od implementacji C-core gRPC do platformy ASP.NET Core, to można zmienić okres istnienia usługi implementacji usługi w polu do pojedynczego wystąpienia.</span><span class="sxs-lookup"><span data-stu-id="8f875-119">To facilitate the transition from gRPC C-core implementation to ASP.NET Core, it is possible to change the service lifetime of the service implementation from Scoped to Singleton.</span></span> <span data-ttu-id="8f875-120">Obejmuje to dodać do kontenera DI wystąpienie implementacji usługi:</span><span class="sxs-lookup"><span data-stu-id="8f875-120">This involves adding an instance of the service implementation to the DI container:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddGrpc();
    services.AddSingleton(new GreeterService());
}
```

<span data-ttu-id="8f875-121">Jednak implementacji usługi Singleton okres istnienia już będzie można rozwiązać polu usług za pomocą iniekcji konstruktora.</span><span class="sxs-lookup"><span data-stu-id="8f875-121">However, service implementation with Singleton lifetime will no longer be able to resolve Scoped services through constructor injection.</span></span>

## <a name="configure-grpc-services-options"></a><span data-ttu-id="8f875-122">Skonfiguruj opcje usługi gRPC</span><span class="sxs-lookup"><span data-stu-id="8f875-122">Configure gRPC services options</span></span>

<span data-ttu-id="8f875-123">W języku C-core na podstawie aplikacji, ustawienia, takie jak `grpc.max_receive_message_length` i `grpc.max_send_message_length` są skonfigurowane przy użyciu `ChannelOption` podczas [konstruowanie `Server` wystąpienia](https://grpc.io/grpc/csharp/api/Grpc.Core.Server.html#Grpc_Core_Server__ctor_System_Collections_Generic_IEnumerable_Grpc_Core_ChannelOption__).</span><span class="sxs-lookup"><span data-stu-id="8f875-123">In C-core based apps, settings such as `grpc.max_receive_message_length` and `grpc.max_send_message_length` are configured with `ChannelOption` when [constructing the `Server` instance](https://grpc.io/grpc/csharp/api/Grpc.Core.Server.html#Grpc_Core_Server__ctor_System_Collections_Generic_IEnumerable_Grpc_Core_ChannelOption__).</span></span>

<span data-ttu-id="8f875-124">W programie ASP.NET Core `GrpcServiceOptions` zapewnia sposób, aby skonfigurować te ustawienia.</span><span class="sxs-lookup"><span data-stu-id="8f875-124">In ASP.NET Core, `GrpcServiceOptions` provides a way to configure these settings.</span></span> <span data-ttu-id="8f875-125">Ustawienia mogą być stosowane globalnie do wszystkich usług gRPC lub typ implementacji poszczególnych usług.</span><span class="sxs-lookup"><span data-stu-id="8f875-125">The settings can be applied globally to all gRPC services or to an individual service implementation type.</span></span> <span data-ttu-id="8f875-126">Opcje określone dla typów wdrożenia poszczególnych usług spowoduje przesłonięcie ustawień globalnych w przypadku skonfigurowania.</span><span class="sxs-lookup"><span data-stu-id="8f875-126">Options specified for individual service implementation types will override global settings when configured.</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services
        .AddGrpc(globalOptions =>
        {
            // Global settings
            globalOptions.SendMaxMessageSize = 4096
            globalOptions.ReceiveMaxMessageSize = 4096
        })
        .AddServiceOptions<GreeterService>(greeterOptions =>
        {
            // GreeterService settings. These will override global settings
            globalOptions.SendMaxMessageSize = 2048
            globalOptions.ReceiveMaxMessageSize = 2048
        })
}
```

## <a name="logging"></a><span data-ttu-id="8f875-127">Rejestrowanie</span><span class="sxs-lookup"><span data-stu-id="8f875-127">Logging</span></span>

<span data-ttu-id="8f875-128">Aplikacje oparte na C-core polegają na `GrpcEnvironment` do [skonfigurować rejestratora](https://grpc.io/grpc/csharp/api/Grpc.Core.GrpcEnvironment.html?q=size#Grpc_Core_GrpcEnvironment_SetLogger_Grpc_Core_Logging_ILogger_) na potrzeby debugowania.</span><span class="sxs-lookup"><span data-stu-id="8f875-128">C-core based apps rely on the `GrpcEnvironment` to [configure the logger](https://grpc.io/grpc/csharp/api/Grpc.Core.GrpcEnvironment.html?q=size#Grpc_Core_GrpcEnvironment_SetLogger_Grpc_Core_Logging_ILogger_) for debugging purposes.</span></span> <span data-ttu-id="8f875-129">Stos programu ASP.NET Core oferuje tę funkcję za pośrednictwem [interfejs API rejestrowania](xref:fundamentals/logging/index).</span><span class="sxs-lookup"><span data-stu-id="8f875-129">The ASP.NET Core stack provides this functionality through the [logging API](xref:fundamentals/logging/index).</span></span> <span data-ttu-id="8f875-130">Na przykład Rejestrator mogą być dodawane do usługi gRPC przy użyciu iniekcji konstruktora:</span><span class="sxs-lookup"><span data-stu-id="8f875-130">For example a logger can be added to the gRPC service via constructor injection:</span></span>

```csharp
public class GreeterService : Greeter.GreeterBase
{
    public GreeterService(ILogger<GreeterService> logger)
    {
    }
}
```

## <a name="https"></a><span data-ttu-id="8f875-131">HTTPS</span><span class="sxs-lookup"><span data-stu-id="8f875-131">HTTPS</span></span>

<span data-ttu-id="8f875-132">Aplikacje oparte na C-core Konfigurowanie protokołu HTTPS za pośrednictwem [ `Server.Ports` właściwość](https://grpc.io/grpc/csharp/api/Grpc.Core.Server.html#Grpc_Core_Server_Ports).</span><span class="sxs-lookup"><span data-stu-id="8f875-132">C-core based apps configure HTTPS through the [`Server.Ports` property](https://grpc.io/grpc/csharp/api/Grpc.Core.Server.html#Grpc_Core_Server_Ports).</span></span> <span data-ttu-id="8f875-133">Koncepcja podobne służy do konfigurowania serwerów w programie ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="8f875-133">A similar concept is used to configure servers in ASP.NET Core.</span></span> <span data-ttu-id="8f875-134">Na przykład używa Kestrel [konfiguracji punktu końcowego](xref:fundamentals/servers/kestrel#endpoint-configuration) dla tej funkcji.</span><span class="sxs-lookup"><span data-stu-id="8f875-134">For example, Kestrel uses [endpoint configuration](xref:fundamentals/servers/kestrel#endpoint-configuration) for this functionality.</span></span>

## <a name="interceptors-and-middlewares"></a><span data-ttu-id="8f875-135">Interceptory i Middlewares</span><span class="sxs-lookup"><span data-stu-id="8f875-135">Interceptors and Middlewares</span></span>

<span data-ttu-id="8f875-136">Platforma ASP.NET Core [middlewares](xref:fundamentals/middleware/index) gRPC aplikacji opartych na oferuje podobne funkcje w porównaniu do interceptory w języku C-core.</span><span class="sxs-lookup"><span data-stu-id="8f875-136">ASP.NET Core [middlewares](xref:fundamentals/middleware/index) offers similar functionalities compared to interceptors in C-core based gRPC apps.</span></span> <span data-ttu-id="8f875-137">Middlewares i interceptory są koncepcyjnie takie same jak oba są używane do budowy potoku, który obsługuje żądania gRPC.</span><span class="sxs-lookup"><span data-stu-id="8f875-137">Middlewares and interceptors are conceptually the same as both are used to construct a pipeline that handles a gRPC request.</span></span> <span data-ttu-id="8f875-138">Umożliwiają one zarówno pracy do wykonania przed lub po następny składnik w potoku.</span><span class="sxs-lookup"><span data-stu-id="8f875-138">They both allow work to be performed before or after the next component in the pipeline.</span></span> <span data-ttu-id="8f875-139">Middlewares platformy ASP.NET Core operują na podstawowej komunikaty HTTP/2 natomiast interceptory działają na gRPC warstwa abstrakcji za pomocą [ `ServerCallContext` ](https://grpc.io/grpc/csharp/api/Grpc.Core.ServerCallContext.html).</span><span class="sxs-lookup"><span data-stu-id="8f875-139">However, ASP.NET Core middlewares operate on the underlying HTTP/2 messages whereas interceptors operate on the gRPC layer of abstraction using the [`ServerCallContext`](https://grpc.io/grpc/csharp/api/Grpc.Core.ServerCallContext.html).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="8f875-140">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="8f875-140">Additional resources</span></span>

* <xref:grpc/index>
* <xref:grpc/basics>
* <xref:grpc/aspnetcore>
* <xref:tutorials/grpc/grpc-start>
