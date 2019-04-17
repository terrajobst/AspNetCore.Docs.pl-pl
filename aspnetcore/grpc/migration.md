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
# <a name="migrating-grpc-services-from-c-core-to-aspnet-core"></a>Migrowanie usług gRPC podstawowe języka C do platformy ASP.NET Core

Przez [Luo Jan](https://github.com/juntaoluo)

Ze względu na implementację podstawowego stosu, nie wszystkie funkcje działają tak samo jak między [oparte na C-core gRPC](https://grpc.io/blog/grpc-stacks) aplikacje i aplikacje korzystające z platformy ASP.NET Core. W tym dokumencie podświetla podstawowe różnice dotyczące migracji między dwoma stosami.

## <a name="grpc-service-implementation-lifetime"></a>okres istnienia implementacji usługi gRPC

W stosie platformy ASP.NET Core gRPC services domyślnie są tworzone przy użyciu [o określonym zakresie okres istnienia](xref:fundamentals/dependency-injection#service-lifetimes). Z kolei gRPC C-core domyślnie wiąże się z usługą za pomocą [okres istnienia pojedyncze](xref:fundamentals/dependency-injection#service-lifetimes).

O określonym zakresie okres istnienia umożliwia implementacji usługi do rozpoznania innych usług z zakresu okresy istnienia. Na przykład, można również rozwiązać o określonym zakresie okres istnienia `DBContext` z kontenera DI przy użyciu iniekcji konstruktora. Korzystanie z zakresami okres istnienia:

* Nowe wystąpienie implementacji usługi jest tworzona dla każdego żądania.
* Nie jest możliwe udostępnianie stanu między żądaniami, za pośrednictwem składowych wystąpień dla typu wdrożenia.
* Oczekuje się, aby przechowywać udostępnionych stanów usługi singleton w kontenerze DI. Przechowywane udostępnionych stanów są rozwiązywane w Konstruktorze gRPC implementacji usługi.

Aby uzyskać więcej informacji na temat usługi okresy istnienia, zobacz <xref:fundamentals/dependency-injection#service-lifetimes>.

### <a name="add-a-singleton-service"></a>Dodawanie usługi singleton

Aby ułatwić przejście od implementacji C-core gRPC do platformy ASP.NET Core, jest można zmienić okres istnienia usługi implementacji usługi z ograniczone do pojedynczego wystąpienia. Obejmuje to dodać do kontenera DI wystąpienie implementacji usługi:

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddGrpc();
    services.AddSingleton(new GreeterService());
}
```

Jednak z implementacją usługi o okresie istnienia pojedynczego wystąpienia nie jest już możliwość rozpoznania o określonym zakresie usług za pomocą iniekcji konstruktora.

## <a name="configure-grpc-services-options"></a>Skonfiguruj opcje usługi gRPC

W aplikacjach na podstawie podstawowej C, ustawienia, takie jak `grpc.max_receive_message_length` i `grpc.max_send_message_length` są skonfigurowane przy użyciu `ChannelOption` podczas [konstruowanie wystąpienie serwera](https://grpc.io/grpc/csharp/api/Grpc.Core.Server.html#Grpc_Core_Server__ctor_System_Collections_Generic_IEnumerable_Grpc_Core_ChannelOption__).

W programie ASP.NET Core `GrpcServiceOptions` zapewnia sposób, aby skonfigurować te ustawienia. Ustawienia mogą być stosowane globalnie do wszystkich usług gRPC lub typ implementacji poszczególnych usług. Opcje określone dla typów wdrożenia poszczególnych usług zastępują ustawienia globalne, w przypadku skonfigurowania.

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

Aplikacje oparte na podstawowej C polegają na `GrpcEnvironment` do [skonfigurować rejestratora](https://grpc.io/grpc/csharp/api/Grpc.Core.GrpcEnvironment.html?q=size#Grpc_Core_GrpcEnvironment_SetLogger_Grpc_Core_Logging_ILogger_) na potrzeby debugowania. Stos programu ASP.NET Core oferuje tę funkcję za pośrednictwem [rejestrowanie interfejsu API](xref:fundamentals/logging/index). Na przykład Rejestrator mogą być dodawane do usługi gRPC przy użyciu iniekcji konstruktora:

```csharp
public class GreeterService : Greeter.GreeterBase
{
    public GreeterService(ILogger<GreeterService> logger)
    {
    }
}
```

## <a name="https"></a>HTTPS

Aplikacje oparte na podstawowej C Konfigurowanie protokołu HTTPS za pośrednictwem [właściwość Server.Ports](https://grpc.io/grpc/csharp/api/Grpc.Core.Server.html#Grpc_Core_Server_Ports). Koncepcja podobne służy do konfigurowania serwerów w programie ASP.NET Core. Na przykład używa Kestrel [konfiguracji punktu końcowego](xref:fundamentals/servers/kestrel#endpoint-configuration) dla tej funkcji.

## <a name="interceptors-and-middleware"></a>Interceptory i oprogramowanie pośredniczące

Platforma ASP.NET Core [oprogramowania pośredniczącego](xref:fundamentals/middleware/index) oferuje podobne funkcje w porównaniu do interceptory w aplikacjach gRPC oparte na C-core. Oprogramowanie pośredniczące i interceptory są koncepcyjnie takie same jak oba są używane do budowy potoku, który obsługuje żądania gRPC. Umożliwiają one zarówno pracy do wykonania przed lub po następny składnik w potoku. Jednak oprogramowanie pośredniczące platformy ASP.NET Core działa w podstawowej komunikaty w protokole HTTP/2, podczas interceptory działają na gRPC warstwa abstrakcji za pomocą [ServerCallContext](https://grpc.io/grpc/csharp/api/Grpc.Core.ServerCallContext.html).

## <a name="additional-resources"></a>Dodatkowe zasoby

* <xref:grpc/index>
* <xref:grpc/basics>
* <xref:grpc/aspnetcore>
* <xref:tutorials/grpc/grpc-start>
