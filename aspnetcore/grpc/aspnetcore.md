---
title: Usługi gRPC na platformie ASP.NET Core
author: juntaoluo
description: Podstawowe informacje na temat podczas zapisywania gRPC usług z platformą ASP.NET Core.
monikerRange: '>= aspnetcore-3.0'
ms.author: johluo
ms.date: 03/08/2019
uid: grpc/aspnetcore
ms.openlocfilehash: ca06478e6168c59d9abf43d99213fa8a7091e178
ms.sourcegitcommit: 756114cab5e24e99ec23de62b0c1c16b15197ac2
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/18/2019
ms.locfileid: "67169522"
---
# <a name="grpc-services-with-aspnet-core"></a>Usługi gRPC na platformie ASP.NET Core

W tym dokumencie pokazano, jak rozpocząć pracę z usługami gRPC przy użyciu platformy ASP.NET Core.

[!INCLUDE[](~/includes/net-core-prereqs-all-3.0.md)]

## <a name="get-started-with-grpc-service-in-aspnet-core"></a>Wprowadzenie do usługi gRPC na platformie ASP.NET Core

[Wyświetlanie lub pobieranie przykładowego kodu](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/tutorials/grpc/grpc-start/sample) ([sposobu pobierania](xref:index#how-to-download-a-sample)).

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

Zobacz [Rozpocznij pracę z usługami gRPC](xref:tutorials/grpc/grpc-start) szczegółowe instrukcje dotyczące sposobu tworzenia projektu gRPC.

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[Visual Studio Code / Visual Studio for Mac](#tab/visual-studio-code+visual-studio-mac)

Uruchom `dotnet new grpc -o GrpcGreeter` z wiersza polecenia.

---

## <a name="add-grpc-services-to-an-aspnet-core-app"></a>Dodawanie usług gRPC do aplikacji ASP.NET Core

gRPC wymaga następujących pakietów:

* [Grpc.AspNetCore.Server](https://www.nuget.org/packages/Grpc.AspNetCore.Server)
* [Google.Protobuf](https://www.nuget.org/packages/Google.Protobuf/) dla formatu protobuf komunikatu interfejsów API.
* [Grpc.Tools](https://www.nuget.org/packages/Grpc.Tools/)

### <a name="configure-grpc"></a>Konfigurowanie gRPC

włączono gRPC `AddGrpc` metody:

[!code-cs[](~/tutorials/grpc/grpc-start/sample/GrpcGreeter/Startup.cs?name=snippet&highlight=7)]

Każda usługa gRPC jest dodawany do potoku routingu za pomocą `MapGrpcService` metody:

[!code-cs[](~/tutorials/grpc/grpc-start/sample/GrpcGreeter/Startup.cs?name=snippet&highlight=24)]

Funkcje i platformy ASP.NET Core middlewares Udostępnianie routingu potoku, w związku z tym można skonfigurować aplikację do obsługi dodatkowych żądania obsługi. Procedury obsługi dodatkowych żądań, takich jak kontrolerów MVC równoległe Pracowanie z usług gRPC skonfigurowany.

## <a name="integration-with-aspnet-core-apis"></a>Integracja za pomocą interfejsów API platformy ASP.NET Core

gRPC usługi mają pełny dostęp do funkcji programu ASP.NET Core, takich jak [wstrzykiwanie zależności](xref:fundamentals/dependency-injection) (DI) i [rejestrowania](xref:fundamentals/logging/index). Na przykład implementacji usługi można usunąć usługi rejestratora z kontenera DI za pośrednictwem konstruktora:

```csharp
public class GreeterService : Greeter.GreeterBase
{
    public GreeterService(ILogger<GreeterService> logger)
    {
    }
}
```

Domyślnie implementacji usługi gRPC można rozwiązać, innych usług DI przy użyciu dowolnego okresu istnienia (pojedyncze, polu lub niepowodzenie tymczasowe).

### <a name="resolve-httpcontext-in-grpc-methods"></a>Rozwiąż HttpContext w metodach gRPC

GRPC interfejs API zapewnia dostęp do niektórych danych komunikatu protokołu HTTP/2, takie jak metody, host, nagłówek i przyczepy. Dostęp jest za pośrednictwem `ServerCallContext` argument przekazany do każdej metody gRPC:

[!code-cs[](~/tutorials/grpc/grpc-start/sample/GrpcGreeter/Services/GreeterService.cs?highlight=3-4&name=snippet)]

`ServerCallContext` nie zapewnia pełny dostęp do `HttpContext` w wszystkich interfejsów API platformy ASP.NET. `GetHttpContext` — Metoda rozszerzenia zapewnia pełny dostęp do `HttpContext` reprezentujący komunikat o podstawowym protokołu HTTP/2 w interfejsach API platformy ASP.NET:

[!code-cs[](~/tutorials/grpc/grpc-start/sample/GrpcGreeter/Services/GreeterService.cs?name=snippet)]

## <a name="additional-resources"></a>Dodatkowe zasoby

* <xref:tutorials/grpc/grpc-start>
* <xref:grpc/index>
* <xref:grpc/basics>
* <xref:grpc/migration>
