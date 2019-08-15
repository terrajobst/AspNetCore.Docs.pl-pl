---
title: Usługi gRPC na platformie ASP.NET Core
author: juntaoluo
description: Zapoznaj się z podstawowymi pojęciami dotyczącymi pisania usług gRPC Services przy użyciu ASP.NET Core.
monikerRange: '>= aspnetcore-3.0'
ms.author: johluo
ms.date: 08/07/2019
uid: grpc/aspnetcore
ms.openlocfilehash: 38111c152c581c50767f9cd4e5fa257bd3fd804e
ms.sourcegitcommit: 476ea5ad86a680b7b017c6f32098acd3414c0f6c
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/14/2019
ms.locfileid: "69022312"
---
# <a name="grpc-services-with-aspnet-core"></a>Usługi gRPC na platformie ASP.NET Core

W tym dokumencie przedstawiono sposób rozpoczynania pracy z usługami gRPC Services przy użyciu ASP.NET Core.

## <a name="prerequisites"></a>Wymagania wstępne

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

[!INCLUDE[](~/includes/net-core-prereqs-vs-3.0.md)]

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

[!INCLUDE[](~/includes/net-core-prereqs-vsc-3.0.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[Visual Studio for Mac](#tab/visual-studio-mac)

[!INCLUDE[](~/includes/net-core-prereqs-mac-3.0.md)]

---

## <a name="get-started-with-grpc-service-in-aspnet-core"></a>Wprowadzenie do usługi gRPC na platformie ASP.NET Core

[Wyświetlanie lub Pobieranie przykładowego kodu](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/tutorials/grpc/grpc-start/sample) ([jak pobrać](xref:index#how-to-download-a-sample)).

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

Szczegółowe instrukcje dotyczące tworzenia projektu gRPC można znaleźć w temacie Wprowadzenie do [usług gRPC Services](xref:tutorials/grpc/grpc-start) .

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[Visual Studio Code/Visual Studio dla komputerów Mac](#tab/visual-studio-code+visual-studio-mac)

Uruchom `dotnet new grpc -o GrpcGreeter` polecenie w wierszu polecenia.

---

## <a name="add-grpc-services-to-an-aspnet-core-app"></a>Dodawanie usług gRPC do aplikacji ASP.NET Core

gRPC wymaga pakietu [gRPC. AspNetCore](https://www.nuget.org/packages/Grpc.AspNetCore) .

### <a name="configure-grpc"></a>Konfigurowanie gRPC

gRPC jest włączona za pomocą `AddGrpc` metody:

[!code-csharp[](~/tutorials/grpc/grpc-start/sample/GrpcGreeter/Startup.cs?name=snippet&highlight=7)]

Każda usługa gRPC jest dodawana do potoku routingu za `MapGrpcService` pomocą metody:

[!code-csharp[](~/tutorials/grpc/grpc-start/sample/GrpcGreeter/Startup.cs?name=snippet&highlight=24)]

ASP.NET Core middlewares i funkcje korzystają z potoku routingu, w związku z czym aplikacja może być skonfigurowana w taki sposób, aby obsługiwała dodatkowe procedury obsługi żądań. Dodatkowe programy obsługi żądań, takie jak kontrolery MVC, pracują równolegle ze skonfigurowanymi usługami gRPC.

## <a name="integration-with-aspnet-core-apis"></a>Integracja z interfejsami API ASP.NET Core

usługi gRPC mają pełny dostęp do funkcji ASP.NET Core, takich jak [iniekcja zależności](xref:fundamentals/dependency-injection) (di) i [Rejestrowanie](xref:fundamentals/logging/index). Na przykład implementacja usługi może rozpoznać usługę rejestratora z kontenera DI za pośrednictwem konstruktora:

```csharp
public class GreeterService : Greeter.GreeterBase
{
    public GreeterService(ILogger<GreeterService> logger)
    {
    }
}
```

Domyślnie implementacja usługi gRPC może rozwiązywać inne usługi DI z dowolnym okresem istnienia (pojedynczym, zakresowym lub przejściowym).

### <a name="resolve-httpcontext-in-grpc-methods"></a>Rozwiąż element HttpContext w metodach gRPC

Interfejs API gRPC zapewnia dostęp do niektórych danych komunikatów HTTP/2, takich jak metoda, host, nagłówek i przyczepy. Dostęp odbywa się za `ServerCallContext` pomocą argumentu przesłanego do każdej metody gRPC:

[!code-csharp[](~/grpc/aspnetcore/sample/GrcpService/GreeterService.cs?highlight=3-4&name=snippet)]

`ServerCallContext`nie zapewnia pełnego dostępu do `HttpContext` wszystkich interfejsów API ASP.NET. Metoda rozszerzająca zapewnia pełny dostęp `HttpContext` do reprezentowania podstawowego komunikatu http/2 w interfejsach API ASP.NET: `GetHttpContext`

[!code-csharp[](~/grpc/aspnetcore/sample/GrcpService/GreeterService2.cs?highlight=6-7&name=snippet)]

## <a name="additional-resources"></a>Dodatkowe zasoby

* <xref:tutorials/grpc/grpc-start>
* <xref:grpc/index>
* <xref:grpc/basics>
* <xref:grpc/migration>
