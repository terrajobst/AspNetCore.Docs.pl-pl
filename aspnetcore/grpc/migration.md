---
title: Migrowanie usług gRPC podstawowe języka C do platformy ASP.NET Core
author: juntaoluo
description: Dowiedz się, jak przenieść istniejącą aplikację na podstawie gRPC podstawowe języka C do uruchamiania na szczycie stosu platformy ASP.NET Core.
monikerRange: '>= aspnetcore-3.0'
ms.author: johluo
ms.date: 03/08/2019
uid: grpc/migration
ms.openlocfilehash: 520318cfe87708cf5c453c88a5eb54027350db4b
ms.sourcegitcommit: a467828b5e4eaae291d961ffe2279a571900de23
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/18/2019
ms.locfileid: "58142610"
---
# <a name="migrating-grpc-services-from-c-core-to-aspnet-core"></a>Migrowanie usług gRPC podstawowe języka C do platformy ASP.NET Core

Przez [Luo Jan](https://github.com/juntaoluo)

Ze względu na implementację podstawowego stosu, nie wszystkie funkcje działają tak samo jak między [C-core na podstawie gRPC](https://grpc.io/blog/grpc-stacks) aplikacji opartych na aplikacji i ASP.NET Core. W tym dokumencie podświetla podstawowe różnice należy pamiętać podczas migracji między dwoma stosami.

## <a name="grpc-service-implementation-lifetime"></a>okres istnienia implementacji usługi gRPC

W stosie platformy ASP.NET Core gRPC services domyślnie, zostanie utworzony przy użyciu [okres istnienia polu](xref:fundamentals/dependency-injection). Z kolei gRPC C-core domyślnie wiąże się z usługą z okresem istnienia pojedynczego wystąpienia.

Okres istnienia polu umożliwia implementacji usługi można rozpoznać innych usług przy użyciu polu okresy istnienia. Na przykład może być także rozwiązać okres istnienia polu `DBContext`, z kontenera DI przy użyciu iniekcji konstruktora. Korzystanie z okresem istnienia polu:

* Nowe wystąpienie implementacji usługi jest tworzona dla każdego żądania.
* Nie jest możliwe udostępnianie stanu między żądaniami, za pośrednictwem składowych wystąpień dla typu wdrożenia.
* Oczekuje się, aby przechowywać udostępnionych stanów usługi Singleton w kontenerze DI. Przechowywane udostępnionych stanów są rozwiązywane w Konstruktorze gRPC implementacji usługi. 

Aby uzyskać więcej informacji o polu i pojedyncze okres istnienia, zobacz <xref:fundamentals/dependency-injection>.

### <a name="add-a-singleton-service"></a>Dodawanie usługi singleton

Aby ułatwić przejście od implementacji C-core gRPC do platformy ASP.NET Core, to można zmienić okres istnienia usługi implementacji usługi w polu do pojedynczego wystąpienia. Obejmuje to dodać do kontenera DI wystąpienie implementacji usługi:

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddGrpc();
    services.AddSingleton(new GreeterService());
}
```

Jednak implementacji usługi Singleton okres istnienia już będzie można rozwiązać polu usług za pomocą iniekcji konstruktora.

## <a name="configure-grpc-services-options"></a>Skonfiguruj opcje usługi gRPC

W języku C-core na podstawie aplikacji, ustawienia, takie jak `grpc.max_receive_message_length` i `grpc.max_send_message_length` są skonfigurowane przy użyciu `ChannelOption` podczas [konstruowanie `Server` wystąpienia](https://grpc.io/grpc/csharp/api/Grpc.Core.Server.html#Grpc_Core_Server__ctor_System_Collections_Generic_IEnumerable_Grpc_Core_ChannelOption__).

W programie ASP.NET Core `GrpcServiceOptions` zapewnia sposób, aby skonfigurować te ustawienia. Ustawienia mogą być stosowane globalnie do wszystkich usług gRPC lub typ implementacji poszczególnych usług. Opcje określone dla typów wdrożenia poszczególnych usług spowoduje przesłonięcie ustawień globalnych w przypadku skonfigurowania.

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

## <a name="logging"></a>Rejestrowanie

Aplikacje oparte na C-core polegają na `GrpcEnvironment` do [skonfigurować rejestratora](https://grpc.io/grpc/csharp/api/Grpc.Core.GrpcEnvironment.html?q=size#Grpc_Core_GrpcEnvironment_SetLogger_Grpc_Core_Logging_ILogger_) na potrzeby debugowania. Stos programu ASP.NET Core oferuje tę funkcję za pośrednictwem [interfejs API rejestrowania](xref:fundamentals/logging/index). Na przykład Rejestrator mogą być dodawane do usługi gRPC przy użyciu iniekcji konstruktora:

```csharp
public class GreeterService : Greeter.GreeterBase
{
    public GreeterService(ILogger<GreeterService> logger)
    {
    }
}
```

## <a name="https"></a>HTTPS

Aplikacje oparte na C-core Konfigurowanie protokołu HTTPS za pośrednictwem [ `Server.Ports` właściwość](https://grpc.io/grpc/csharp/api/Grpc.Core.Server.html#Grpc_Core_Server_Ports). Koncepcja podobne służy do konfigurowania serwerów w programie ASP.NET Core. Na przykład używa Kestrel [konfiguracji punktu końcowego](xref:fundamentals/servers/kestrel#endpoint-configuration) dla tej funkcji.

## <a name="interceptors-and-middlewares"></a>Interceptory i Middlewares

Platforma ASP.NET Core [middlewares](xref:fundamentals/middleware/index) gRPC aplikacji opartych na oferuje podobne funkcje w porównaniu do interceptory w języku C-core. Middlewares i interceptory są koncepcyjnie takie same jak oba są używane do konstruowania pipleline, która obsługuje żądania gRPC. Umożliwiają one zarówno pracy do wykonania przed lub po następny składnik w potoku. Middlewares platformy ASP.NET Core operują na podstawowej komunikaty HTTP/2 natomiast interceptory działają na gRPC warstwa abstrakcji za pomocą [ `ServerCallContext` ](https://grpc.io/grpc/csharp/api/Grpc.Core.ServerCallContext.html).

## <a name="additional-resources"></a>Dodatkowe zasoby

* <xref:grpc/index>
* <xref:grpc/basics>
* <xref:grpc/aspnetcore>
* <xref:tutorials/grpc/grpc-start>
