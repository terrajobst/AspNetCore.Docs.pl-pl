---
page_type: sample
description: W tym samouczku pokazano, jak utworzyć usługę gRPC i klienta gRPC na ASP.NET Core.
languages:
- csharp
products:
- dotnet-core
- aspnet-core
- vs
urlFragment: create-grpc-client
ms.openlocfilehash: b9feb9eed62177358fffc0d7da582f625a431e32
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78660931"
---
# <a name="create-a-grpc-client-and-server-in-aspnet-core-30-using-visual-studio"></a>Tworzenie gRPC klienta i serwera w ASP.NET Core 3,0 przy użyciu programu Visual Studio

W tym samouczku pokazano, jak utworzyć klienta programu .NET Core [gRPC](https://grpc.io/docs/guides/) oraz serwer gRPC ASP.NET Core.

Na koniec będziesz mieć klienta gRPC, który komunikuje się z usługą gRPC Greeter.

W tym samouczku:

* Utwórz serwer gRPC.
* Utwórz klienta gRPC.
* Przetestuj usługę klienta gRPC za pomocą usługi gRPC Greeter.

## <a name="create-a-grpc-service"></a>Tworzenie usługi gRPC

* Z menu **plik** programu Visual Studio wybierz pozycję **Nowy** **projekt** > .
* W oknie dialogowym **Tworzenie nowego projektu** wybierz pozycję **ASP.NET Core aplikacji sieci Web**.
* Wybierz pozycję **Dalej**
* Nazwij projekt **GrpcGreeter**. Ważne jest, aby nazwa projektu *GrpcGreeter* , tak aby przestrzenie nazw były zgodne podczas kopiowania i wklejania kodu.
* Wybierz pozycję **Utwórz**
* W oknie dialogowym **Tworzenie nowej ASP.NET Core aplikacji sieci Web** :
  * W menu rozwijanym wybierz pozycję **.NET Core** i **ASP.NET Core 3,0** . 
  * Wybierz szablon **usługi gRPC** .
  * Wybierz pozycję **Utwórz**

### <a name="run-the-service"></a>Uruchamianie usługi

* Naciśnij `Ctrl+F5`, aby uruchomić usługę gRPC bez debugera.

  Program Visual Studio uruchamia usługę w wierszu polecenia.

Dzienniki pokazują, że usługa nasłuchuje na `https://localhost:5001`.

```console
info: Microsoft.Hosting.Lifetime[0]
      Now listening on: https://localhost:5001
info: Microsoft.Hosting.Lifetime[0]
      Application started. Press Ctrl+C to shut down.
info: Microsoft.Hosting.Lifetime[0]
      Hosting environment: Development
info: Microsoft.Hosting.Lifetime[0]
```

> [!NOTE]
> Szablon gRPC jest skonfigurowany do korzystania z [Transport Layer Security (TLS)](https://tools.ietf.org/html/rfc5246). Klienci gRPC muszą używać protokołu HTTPS, aby wywołać serwer.
>
> Program macOS nie obsługuje ASP.NET Core gRPC z protokołem TLS. Aby pomyślnie uruchomić usługi gRPC Services w usłudze macOS, wymagana jest dodatkowa konfiguracja. Aby uzyskać więcej informacji, zobacz [gRPC i ASP.NET Core on macOS](xref:grpc/aspnetcore#grpc-and-aspnet-core-on-macos).

### <a name="examine-the-project-files"></a>Sprawdzanie plików projektu

*GrpcGreeter* pliki projektu:

* *Greeting. proto*: plik *Protos/Greeting. proto* definiuje `Greeter` gRPC i służy do generowania zasobów serwera gRPC. Aby uzyskać więcej informacji, zobacz [wprowadzenie do gRPC](xref:grpc/index).
* Folder *usługi* : zawiera implementację usługi `Greeter`.
* *appSettings. JSON*: zawiera dane konfiguracyjne, takie jak protokół używany przez Kestrel. Aby uzyskać więcej informacji, zobacz <xref:fundamentals/configuration/index>.
* *Program.cs*: zawiera punkt wejścia dla usługi gRPC. Aby uzyskać więcej informacji, zobacz <xref:fundamentals/host/generic-host>.
* *Startup.cs*: zawiera kod, który konfiguruje zachowanie aplikacji. Aby uzyskać więcej informacji, zobacz [Uruchamianie aplikacji](xref:fundamentals/startup).

## <a name="create-the-grpc-client-in-a-net-console-app"></a>Tworzenie klienta gRPC w aplikacji konsolowej .NET

* Otwórz drugie wystąpienie programu Visual Studio.
* Wybierz pozycję **plik** > **Nowy** > **projekt** na pasku menu.
* W oknie dialogowym **Tworzenie nowego projektu** wybierz pozycję **Aplikacja konsolowa (.NET Core)** .
* Wybierz pozycję **Dalej**
* W polu tekstowym **Nazwa** wprowadź wartość "GrpcGreeterClient".
* Wybierz pozycję **Utwórz**.

### <a name="add-required-packages"></a>Dodaj wymagane pakiety

Projekt klienta gRPC wymaga następujących pakietów:

* [GRPC .NET. Client](https://www.nuget.org/packages/Grpc.Net.Client), który zawiera klienta .NET Core.
* [Google. protobuf](https://www.nuget.org/packages/Google.Protobuf/), który zawiera interfejsy API komunikatów protobuf C#dla.
* [GRPC. Tools](https://www.nuget.org/packages/Grpc.Tools/), która zawiera C# obsługę narzędzi dla plików protobuf. Pakiet narzędzi nie jest wymagany w czasie wykonywania, dlatego zależność jest oznaczona za pomocą `PrivateAssets="All"`.

Zainstaluj pakiety przy użyciu konsoli Menedżera pakietów (PMC) lub Zarządzaj pakietami NuGet.

#### <a name="pmc-option-to-install-packages"></a>Opcja PMC, aby zainstalować pakiety

* W programie Visual Studio wybierz kolejno pozycje **narzędzia** > **menedżer pakietów NuGet** > **konsola Menedżera pakietów**
* W oknie **konsola Menedżera pakietów** przejdź do katalogu, w którym znajduje się plik *GrpcGreeterClient. csproj* .
* Uruchom następujące polecenia:

 ```powershell
Install-Package Grpc.Net.Client
Install-Package Google.Protobuf
Install-Package Grpc.Tools
```

#### <a name="manage-nuget-packages-option-to-install-packages"></a>Opcja zarządzania pakietami NuGet w celu zainstalowania pakietów

* Kliknij prawym przyciskiem myszy projekt w **Eksplorator rozwiązań** > **Zarządzanie pakietami NuGet**
* Wybierz kartę **Przeglądaj**.
* W polu wyszukiwania wprowadź **GRPC .NET. Client** .
* Wybierz pakiet **GRPC .NET. Client** z karty **Przeglądaj** i wybierz pozycję **Zainstaluj**.
* Powtórz dla `Google.Protobuf` i `Grpc.Tools`.

### <a name="add-greetproto"></a>Dodaj Greeting. proto

* Utwórz folder **Protos** w projekcie klienta gRPC.
* Skopiuj plik **Protos\greet.proto** z usługi gRPC Greeter do projektu klienta gRPC.
* Edytuj plik projektu *GrpcGreeterClient. csproj* :

  Kliknij prawym przyciskiem myszy projekt i wybierz polecenie **Edytuj plik projektu**.

* Dodaj grupę elementów z elementem `<Protobuf>`, który odwołuje się do pliku **Greeting. proto** :

  ```xml
  <ItemGroup>
    <Protobuf Include="Protos\greet.proto" GrpcServices="Client" />
  </ItemGroup>
  ```

### <a name="create-the-greeter-client"></a>Tworzenie klienta Greeter

Skompiluj projekt, aby utworzyć typy w przestrzeni nazw `GrpcGreeter`. Typy `GrpcGreeter` są generowane automatycznie przez proces kompilacji.

Zaktualizuj plik *program.cs* klienta gRPC za pomocą następującego kodu:

```csharp
using System;
using System.Net.Http;
using System.Threading.Tasks;
using GrpcGreeter;
using Grpc.Net.Client;

namespace GrpcGreeterClient
{
    class Program
    {
        static async Task Main(string[] args)
        {
            // The port number(5001) must match the port of the gRPC server.
            var channel = GrpcChannel.ForAddress("https://localhost:5001");
            var client = new Greeter.GreeterClient(channel);
            var reply = await client.SayHelloAsync(
                              new HelloRequest { Name = "GreeterClient" });
            Console.WriteLine("Greeting: " + reply.Message);
            Console.WriteLine("Press any key to exit...");
            Console.ReadKey();
        }
    }
}
```

*Program.cs* zawiera punkt wejścia i logikę dla klienta gRPC.

Klient Greeter jest tworzony przez:

* Utworzenie wystąpienia `GrpcChannel` zawierającego informacje służące do tworzenia połączenia z usługą gRPC.
* Używanie `GrpcChannel` do konstruowania klienta Greeter.

## <a name="test-the-grpc-client-with-the-grpc-greeter-service"></a>Testowanie klienta gRPC za pomocą usługi gRPC Greeter

* W usłudze Greeter naciśnij `Ctrl+F5`, aby uruchomić serwer bez debugera.
* W projekcie `GrpcGreeterClient` naciśnij klawisz `Ctrl+F5`, aby uruchomić klienta bez debugera.

Klient wysyła powitanie do usługi z komunikatem zawierającym nazwę "GreeterClient". Usługa wysyła komunikat "Hello GreeterClient" jako odpowiedź. Odpowiedź "Hello GreeterClient" jest wyświetlana w wierszu polecenia:

```console
Greeting: Hello GreeterClient
Press any key to exit...
```

Usługa gRPC rejestruje szczegóły pomyślnego wywołania w dziennikach zapisanych w wierszu polecenia.

```console
info: Microsoft.Hosting.Lifetime[0]
      Now listening on: https://localhost:5001
info: Microsoft.Hosting.Lifetime[0]
      Application started. Press Ctrl+C to shut down.
info: Microsoft.Hosting.Lifetime[0]
      Hosting environment: Development
info: Microsoft.Hosting.Lifetime[0]
      Content root path: C:\GH\aspnet\docs\4\Docs\aspnetcore\tutorials\grpc\grpc-start\sample\GrpcGreeter
info: Microsoft.AspNetCore.Hosting.Diagnostics[1]
      Request starting HTTP/2 POST https://localhost:5001/Greet.Greeter/SayHello application/grpc
info: Microsoft.AspNetCore.Routing.EndpointMiddleware[0]
      Executing endpoint 'gRPC - /Greet.Greeter/SayHello'
info: Microsoft.AspNetCore.Routing.EndpointMiddleware[1]
      Executed endpoint 'gRPC - /Greet.Greeter/SayHello'
info: Microsoft.AspNetCore.Hosting.Diagnostics[2]
      Request finished in 78.32260000000001ms 200 application/grpc
```

### <a name="docs-help--next-steps-for-grpc"></a>Dokumentacja pomocy & następnych kroków dla gRPC

* [Wprowadzenie do gRPC na ASP.NET Core](https://docs.microsoft.com/aspnet/core/grpc/index?view=aspnetcore-3.0)
* [usługi gRPC zC#](https://docs.microsoft.com/aspnet/core/grpc/basics?view=aspnetcore-3.0)
* [Migrowanie usług gRPC z dysku C-Core do ASP.NET Core](https://docs.microsoft.com/aspnet/core/grpc/migration?view=aspnetcore-3.0)
