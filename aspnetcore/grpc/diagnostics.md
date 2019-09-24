---
title: Rejestrowanie i Diagnostyka w programie gRPC na platformie .NET
author: jamesnk
description: Dowiedz się, jak zbierać diagnostykę z aplikacji gRPC na platformie .NET.
monikerRange: '>= aspnetcore-3.0'
ms.author: jamesnk
ms.date: 09/23/2019
uid: grpc/diagnostics
ms.openlocfilehash: ce6ad96d9e26c9cd3844093536745f8f9bea4a76
ms.sourcegitcommit: 0365af91518004c4a44a30dc3a8ac324558a399b
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 09/23/2019
ms.locfileid: "71204346"
---
# <a name="logging-and-diagnostics-in-grpc-on-net"></a>Rejestrowanie i Diagnostyka w programie gRPC na platformie .NET

Przez [Kuba Kowalski-króla](https://twitter.com/jamesnk)

Ten artykuł zawiera wskazówki dotyczące zbierania danych diagnostycznych z aplikacji gRPC w celu ułatwienia rozwiązywania problemów.

## <a name="grpc-services-logging"></a>Rejestrowanie usług gRPC Services

> [!WARNING]
> Dzienniki po stronie serwera mogą zawierać poufne informacje z aplikacji. **Nigdy nie** Publikuj nieprzetworzonych dzienników z aplikacji produkcyjnych na forach publicznych, takich jak GitHub.

Ponieważ usługi gRPC Services są hostowane na ASP.NET Core, korzysta z systemu rejestrowania ASP.NET Core. W konfiguracji domyślnej gRPC rejestruje bardzo mało informacji, ale można je skonfigurować. Szczegółowe informacje na temat konfigurowania rejestrowania ASP.NET Core można znaleźć w dokumentacji dotyczącej [rejestrowania ASP.NET Core](xref:fundamentals/logging/index#configuration) .

gRPC dodaje dzienniki w `Grpc` kategorii. Aby włączyć `Grpc` szczegółowe dzienniki z gRPC, skonfiguruj prefiksy `Debug` do poziomu w pliku *appSettings. JSON* , dodając następujące elementy do `LogLevel` podsekcji w: `Logging`

[!code-json[](diagnostics/logging-config.json?highlight=7)]

Można to również skonfigurować w *Startup.cs* z `ConfigureLogging`:

[!code-csharp[](diagnostics/logging-config-code.cs?highlight=5)]

Jeśli nie korzystasz z konfiguracji opartej na notacji JSON, ustaw w systemie konfiguracji następującą wartość konfiguracji:

* `Logging:LogLevel:Grpc` = `Debug`

Zapoznaj się z dokumentacją systemu konfiguracyjnego, aby określić sposób określania zagnieżdżonych wartości konfiguracyjnych. Na przykład w przypadku używania zmiennych środowiskowych zamiast `_` `:` (na przykład `Logging__LogLevel__Grpc`) są używane dwa znaki.

Zalecamy użycie `Debug` poziomu podczas zbierania bardziej szczegółowych informacji diagnostycznych dla aplikacji. Na `Trace` poziomie powstaje Diagnostyka bardzo niskiego poziomu i jest rzadko wymagana do diagnozowania problemów w aplikacji.

### <a name="sample-logging-output"></a>Przykładowe dane wyjściowe rejestrowania

Oto przykład danych wyjściowych konsoli na `Debug` poziomie usługi gRPC:

```
info: Microsoft.AspNetCore.Hosting.Diagnostics[1]
      Request starting HTTP/2 POST https://localhost:5001/Greet.Greeter/SayHello application/grpc
info: Microsoft.AspNetCore.Routing.EndpointMiddleware[0]
      Executing endpoint 'gRPC - /Greet.Greeter/SayHello'
dbug: Grpc.AspNetCore.Server.ServerCallHandler[1]
      Reading message.
info: GrpcService.GreeterService[0]
      Hello World
dbug: Grpc.AspNetCore.Server.ServerCallHandler[6]
      Sending message.
info: Microsoft.AspNetCore.Routing.EndpointMiddleware[1]
      Executed endpoint 'gRPC - /Greet.Greeter/SayHello'
info: Microsoft.AspNetCore.Hosting.Diagnostics[2]
      Request finished in 1.4113ms 200 application/grpc
```

## <a name="access-server-side-logs"></a>Dostęp do dzienników po stronie serwera

Sposób dostępu do dzienników po stronie serwera zależy od środowiska, w którym jest uruchomiony program.

### <a name="as-a-console-app"></a>Jako Aplikacja konsolowa

Jeśli używasz programu w aplikacji konsolowej, [Rejestrator konsoli](xref:fundamentals/logging/index#console-provider) powinien być domyślnie włączony. Dzienniki gRPC będą wyświetlane w konsoli programu.

### <a name="other-environments"></a>Inne środowiska

Jeśli aplikacja jest wdrażana w innym środowisku (na przykład Docker, Kubernetes lub Windows Service), zobacz <xref:fundamentals/logging/index> , aby uzyskać więcej informacji na temat konfigurowania dostawców rejestrowania odpowiednie dla danego środowiska.

## <a name="grpc-client-logging"></a>Rejestrowanie klienta gRPC

> [!WARNING]
> Dzienniki po stronie klienta mogą zawierać poufne informacje z aplikacji. **Nigdy nie** Publikuj nieprzetworzonych dzienników z aplikacji produkcyjnych na forach publicznych, takich jak GitHub.

Aby pobrać dzienniki z klienta .NET, można ustawić `GrpcChannelOptions.LoggerFactory` właściwość podczas tworzenia kanału klienta. Jeśli wywołujesz usługę gRPC z aplikacji ASP.NET Core, fabryka rejestratorów może zostać rozpoznana z iniekcji zależności (DI):

[!code-csharp[](diagnostics/net-client-dependency-injection.cs?highlight=7,16)]

Alternatywnym sposobem włączenia rejestrowania klienta jest użycie [fabryki klienta gRPC](xref:grpc/clientfactory) do utworzenia klienta programu. Klient gRPC zarejestrowany w fabryce klienta i rozwiązany przez program DI automatycznie użyje skonfigurowanego rejestrowania aplikacji.

Jeśli aplikacja nie używa funkcji di, można utworzyć nowe `ILoggerFactory` wystąpienie za pomocą [LoggerFactory. Create](xref:Microsoft.Extensions.Logging.LoggerFactory.Create*). Aby uzyskać dostęp do tej metody, Dodaj pakiet [Microsoft. Extensions. Logging](https://www.nuget.org/packages/microsoft.extensions.logging/) do aplikacji.

[!code-csharp[](diagnostics/net-client-loggerfactory-create.cs?highlight=1,8)]

### <a name="sample-logging-output"></a>Przykładowe dane wyjściowe rejestrowania

Oto przykład danych wyjściowych konsoli na `Debug` poziomie klienta gRPC:

```
dbug: Grpc.Net.Client.Internal.GrpcCall[1]
      Starting gRPC call. Method type: 'Unary', URI: 'https://localhost:5001/Greet.Greeter/SayHello'.
dbug: Grpc.Net.Client.Internal.GrpcCall[6]
      Sending message.
dbug: Grpc.Net.Client.Internal.GrpcCall[1]
      Reading message.
dbug: Grpc.Net.Client.Internal.GrpcCall[4]
      Finished gRPC call.
```

## <a name="additional-resources"></a>Dodatkowe zasoby

* <xref:fundamentals/logging/index>
* <xref:grpc/configuration>
* <xref:grpc/clientfactory>
