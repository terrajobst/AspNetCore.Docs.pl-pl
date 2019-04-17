---
title: Migrowanie usług gRPC podstawowe języka C do platformy ASP.NET Core
author: juntaoluo
description: Dowiedz się, jak przenieść istniejącą aplikację na podstawie gRPC podstawowe języka C do uruchamiania na szczycie stosu platformy ASP.NET Core.
monikerRange: '>= aspnetcore-3.0'
ms.author: johluo
ms.date: 03/31/2019
uid: grpc/migration
ms.openlocfilehash: 4d489b5aecf2e15fbbe3ac472b991a4365cd47c1
ms.sourcegitcommit: 57a974556acd09363a58f38c26f74dc21e0d4339
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/17/2019
ms.locfileid: "59672622"
---
# <a name="migrating-grpc-services-from-c-core-to-aspnet-core"></a><span data-ttu-id="0b443-103">Migrowanie usług gRPC podstawowe języka C do platformy ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="0b443-103">Migrating gRPC services from C-core to ASP.NET Core</span></span>

<span data-ttu-id="0b443-104">Przez [Luo Jan](https://github.com/juntaoluo)</span><span class="sxs-lookup"><span data-stu-id="0b443-104">By [John Luo](https://github.com/juntaoluo)</span></span>

<span data-ttu-id="0b443-105">Ze względu na implementację podstawowego stosu, nie wszystkie funkcje działają tak samo jak między [oparte na C-core gRPC](https://grpc.io/blog/grpc-stacks) aplikacje i aplikacje korzystające z platformy ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="0b443-105">Due to the implementation of the underlying stack, not all features work in the same way between [C-core-based gRPC](https://grpc.io/blog/grpc-stacks) apps and ASP.NET Core-based apps.</span></span> <span data-ttu-id="0b443-106">W tym dokumencie podświetla podstawowe różnice dotyczące migracji między dwoma stosami.</span><span class="sxs-lookup"><span data-stu-id="0b443-106">This document highlights the key differences for migrating between the two stacks.</span></span>

## <a name="grpc-service-implementation-lifetime"></a><span data-ttu-id="0b443-107">okres istnienia implementacji usługi gRPC</span><span class="sxs-lookup"><span data-stu-id="0b443-107">gRPC service implementation lifetime</span></span>

<span data-ttu-id="0b443-108">W stosie platformy ASP.NET Core gRPC services domyślnie są tworzone przy użyciu [o określonym zakresie okres istnienia](xref:fundamentals/dependency-injection#service-lifetimes).</span><span class="sxs-lookup"><span data-stu-id="0b443-108">In the ASP.NET Core stack, gRPC services, by default, are created with a [scoped lifetime](xref:fundamentals/dependency-injection#service-lifetimes).</span></span> <span data-ttu-id="0b443-109">Z kolei gRPC C-core domyślnie wiąże się z usługą za pomocą [okres istnienia pojedyncze](xref:fundamentals/dependency-injection#service-lifetimes).</span><span class="sxs-lookup"><span data-stu-id="0b443-109">In contrast, gRPC C-core by default binds to a service with a [singleton lifetime](xref:fundamentals/dependency-injection#service-lifetimes).</span></span>

<span data-ttu-id="0b443-110">O określonym zakresie okres istnienia umożliwia implementacji usługi do rozpoznania innych usług z zakresu okresy istnienia.</span><span class="sxs-lookup"><span data-stu-id="0b443-110">A scoped lifetime allows the service implementation to resolve other services with scoped lifetimes.</span></span> <span data-ttu-id="0b443-111">Na przykład, można również rozwiązać o określonym zakresie okres istnienia `DBContext` z kontenera DI przy użyciu iniekcji konstruktora.</span><span class="sxs-lookup"><span data-stu-id="0b443-111">For example, a scoped lifetime can also resolve `DBContext` from the DI container through constructor injection.</span></span> <span data-ttu-id="0b443-112">Korzystanie z zakresami okres istnienia:</span><span class="sxs-lookup"><span data-stu-id="0b443-112">Using scoped lifetime:</span></span>

* <span data-ttu-id="0b443-113">Nowe wystąpienie implementacji usługi jest tworzona dla każdego żądania.</span><span class="sxs-lookup"><span data-stu-id="0b443-113">A new instance of the service implementation is constructed for each request.</span></span>
* <span data-ttu-id="0b443-114">Nie jest możliwe udostępnianie stanu między żądaniami, za pośrednictwem składowych wystąpień dla typu wdrożenia.</span><span class="sxs-lookup"><span data-stu-id="0b443-114">It isn't possible to share state between requests via instance members on the implementation type.</span></span>
* <span data-ttu-id="0b443-115">Oczekuje się, aby przechowywać udostępnionych stanów usługi singleton w kontenerze DI.</span><span class="sxs-lookup"><span data-stu-id="0b443-115">The expectation is to store shared states in a singleton service in the DI container.</span></span> <span data-ttu-id="0b443-116">Przechowywane udostępnionych stanów są rozwiązywane w Konstruktorze gRPC implementacji usługi.</span><span class="sxs-lookup"><span data-stu-id="0b443-116">The stored shared states are resolved in the constructor of the gRPC service implementation.</span></span>

<span data-ttu-id="0b443-117">Aby uzyskać więcej informacji na temat usługi okresy istnienia, zobacz <xref:fundamentals/dependency-injection#service-lifetimes>.</span><span class="sxs-lookup"><span data-stu-id="0b443-117">For more information on service lifetimes, see <xref:fundamentals/dependency-injection#service-lifetimes>.</span></span>

### <a name="add-a-singleton-service"></a><span data-ttu-id="0b443-118">Dodawanie usługi singleton</span><span class="sxs-lookup"><span data-stu-id="0b443-118">Add a singleton service</span></span>

<span data-ttu-id="0b443-119">Aby ułatwić przejście od implementacji C-core gRPC do platformy ASP.NET Core, jest można zmienić okres istnienia usługi implementacji usługi z ograniczone do pojedynczego wystąpienia.</span><span class="sxs-lookup"><span data-stu-id="0b443-119">To facilitate the transition from a gRPC C-core implementation to ASP.NET Core, it's possible to change the service lifetime of the service implementation from scoped to singleton.</span></span> <span data-ttu-id="0b443-120">Obejmuje to dodać do kontenera DI wystąpienie implementacji usługi:</span><span class="sxs-lookup"><span data-stu-id="0b443-120">This involves adding an instance of the service implementation to the DI container:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddGrpc();
    services.AddSingleton(new GreeterService());
}
```

<span data-ttu-id="0b443-121">Jednak z implementacją usługi o okresie istnienia pojedynczego wystąpienia nie jest już możliwość rozpoznania o określonym zakresie usług za pomocą iniekcji konstruktora.</span><span class="sxs-lookup"><span data-stu-id="0b443-121">However, a service implementation with a singleton lifetime is no longer able to resolve scoped services through constructor injection.</span></span>

## <a name="configure-grpc-services-options"></a><span data-ttu-id="0b443-122">Skonfiguruj opcje usługi gRPC</span><span class="sxs-lookup"><span data-stu-id="0b443-122">Configure gRPC services options</span></span>

<span data-ttu-id="0b443-123">W aplikacjach na podstawie podstawowej C, ustawienia, takie jak `grpc.max_receive_message_length` i `grpc.max_send_message_length` są skonfigurowane przy użyciu `ChannelOption` podczas [konstruowanie wystąpienie serwera](https://grpc.io/grpc/csharp/api/Grpc.Core.Server.html#Grpc_Core_Server__ctor_System_Collections_Generic_IEnumerable_Grpc_Core_ChannelOption__).</span><span class="sxs-lookup"><span data-stu-id="0b443-123">In C-core-based apps, settings such as `grpc.max_receive_message_length` and `grpc.max_send_message_length` are configured with `ChannelOption` when [constructing the Server instance](https://grpc.io/grpc/csharp/api/Grpc.Core.Server.html#Grpc_Core_Server__ctor_System_Collections_Generic_IEnumerable_Grpc_Core_ChannelOption__).</span></span>

<span data-ttu-id="0b443-124">W programie ASP.NET Core `GrpcServiceOptions` zapewnia sposób, aby skonfigurować te ustawienia.</span><span class="sxs-lookup"><span data-stu-id="0b443-124">In ASP.NET Core, `GrpcServiceOptions` provides a way to configure these settings.</span></span> <span data-ttu-id="0b443-125">Ustawienia mogą być stosowane globalnie do wszystkich usług gRPC lub typ implementacji poszczególnych usług.</span><span class="sxs-lookup"><span data-stu-id="0b443-125">The settings can be applied globally to all gRPC services or to an individual service implementation type.</span></span> <span data-ttu-id="0b443-126">Opcje określone dla typów wdrożenia poszczególnych usług zastępują ustawienia globalne, w przypadku skonfigurowania.</span><span class="sxs-lookup"><span data-stu-id="0b443-126">Options specified for individual service implementation types override global settings when configured.</span></span>

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

## <a name="logging"></a><span data-ttu-id="0b443-127">Rejestrowanie</span><span class="sxs-lookup"><span data-stu-id="0b443-127">Logging</span></span>

<span data-ttu-id="0b443-128">Aplikacje oparte na podstawowej C polegają na `GrpcEnvironment` do [skonfigurować rejestratora](https://grpc.io/grpc/csharp/api/Grpc.Core.GrpcEnvironment.html?q=size#Grpc_Core_GrpcEnvironment_SetLogger_Grpc_Core_Logging_ILogger_) na potrzeby debugowania.</span><span class="sxs-lookup"><span data-stu-id="0b443-128">C-core-based apps rely on the `GrpcEnvironment` to [configure the logger](https://grpc.io/grpc/csharp/api/Grpc.Core.GrpcEnvironment.html?q=size#Grpc_Core_GrpcEnvironment_SetLogger_Grpc_Core_Logging_ILogger_) for debugging purposes.</span></span> <span data-ttu-id="0b443-129">Stos programu ASP.NET Core oferuje tę funkcję za pośrednictwem [rejestrowanie interfejsu API](xref:fundamentals/logging/index).</span><span class="sxs-lookup"><span data-stu-id="0b443-129">The ASP.NET Core stack provides this functionality through the [Logging API](xref:fundamentals/logging/index).</span></span> <span data-ttu-id="0b443-130">Na przykład Rejestrator mogą być dodawane do usługi gRPC przy użyciu iniekcji konstruktora:</span><span class="sxs-lookup"><span data-stu-id="0b443-130">For example, a logger can be added to the gRPC service via constructor injection:</span></span>

```csharp
public class GreeterService : Greeter.GreeterBase
{
    public GreeterService(ILogger<GreeterService> logger)
    {
    }
}
```

## <a name="https"></a><span data-ttu-id="0b443-131">HTTPS</span><span class="sxs-lookup"><span data-stu-id="0b443-131">HTTPS</span></span>

<span data-ttu-id="0b443-132">Aplikacje oparte na podstawowej C Konfigurowanie protokołu HTTPS za pośrednictwem [właściwość Server.Ports](https://grpc.io/grpc/csharp/api/Grpc.Core.Server.html#Grpc_Core_Server_Ports).</span><span class="sxs-lookup"><span data-stu-id="0b443-132">C-core-based apps configure HTTPS through the [Server.Ports property](https://grpc.io/grpc/csharp/api/Grpc.Core.Server.html#Grpc_Core_Server_Ports).</span></span> <span data-ttu-id="0b443-133">Koncepcja podobne służy do konfigurowania serwerów w programie ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="0b443-133">A similar concept is used to configure servers in ASP.NET Core.</span></span> <span data-ttu-id="0b443-134">Na przykład używa Kestrel [konfiguracji punktu końcowego](xref:fundamentals/servers/kestrel#endpoint-configuration) dla tej funkcji.</span><span class="sxs-lookup"><span data-stu-id="0b443-134">For example, Kestrel uses [endpoint configuration](xref:fundamentals/servers/kestrel#endpoint-configuration) for this functionality.</span></span>

## <a name="interceptors-and-middleware"></a><span data-ttu-id="0b443-135">Interceptory i oprogramowanie pośredniczące</span><span class="sxs-lookup"><span data-stu-id="0b443-135">Interceptors and Middleware</span></span>

<span data-ttu-id="0b443-136">Platforma ASP.NET Core [oprogramowania pośredniczącego](xref:fundamentals/middleware/index) oferuje podobne funkcje w porównaniu do interceptory w aplikacjach gRPC oparte na C-core.</span><span class="sxs-lookup"><span data-stu-id="0b443-136">ASP.NET Core [middleware](xref:fundamentals/middleware/index) offers similar functionalities compared to interceptors in C-core-based gRPC apps.</span></span> <span data-ttu-id="0b443-137">Oprogramowanie pośredniczące i interceptory są koncepcyjnie takie same jak oba są używane do budowy potoku, który obsługuje żądania gRPC.</span><span class="sxs-lookup"><span data-stu-id="0b443-137">Middleware and interceptors are conceptually the same as both are used to construct a pipeline that handles a gRPC request.</span></span> <span data-ttu-id="0b443-138">Umożliwiają one zarówno pracy do wykonania przed lub po następny składnik w potoku.</span><span class="sxs-lookup"><span data-stu-id="0b443-138">They both allow work to be performed before or after the next component in the pipeline.</span></span> <span data-ttu-id="0b443-139">Jednak oprogramowanie pośredniczące platformy ASP.NET Core działa w podstawowej komunikaty w protokole HTTP/2, podczas interceptory działają na gRPC warstwa abstrakcji za pomocą [ServerCallContext](https://grpc.io/grpc/csharp/api/Grpc.Core.ServerCallContext.html).</span><span class="sxs-lookup"><span data-stu-id="0b443-139">However, ASP.NET Core middleware operates on the underlying HTTP/2 messages, while interceptors operate on the gRPC layer of abstraction using the [ServerCallContext](https://grpc.io/grpc/csharp/api/Grpc.Core.ServerCallContext.html).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="0b443-140">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="0b443-140">Additional resources</span></span>

* <xref:grpc/index>
* <xref:grpc/basics>
* <xref:grpc/aspnetcore>
* <xref:tutorials/grpc/grpc-start>
