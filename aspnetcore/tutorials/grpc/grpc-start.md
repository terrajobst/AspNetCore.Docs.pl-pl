---
title: Tworzenie klienta i serwera platformy .NET Core gRPC w ASP.NET Core
author: juntaoluo
description: W tym samouczku pokazano, jak utworzyć usługę gRPC i klienta gRPC na ASP.NET Core. Dowiedz się, jak utworzyć projekt usługi gRPC, edytować plik PROTO i dodać wywołanie przesyłania strumieniowego dupleksu.
monikerRange: '>= aspnetcore-3.0'
ms.author: johluo
ms.date: 10/10/2019
uid: tutorials/grpc/grpc-start
ms.openlocfilehash: 0da5a4cf0d9cc15fee6417d143cfc9e9f1e4509c
ms.sourcegitcommit: 9e85c2562df5e108d7933635c830297f484bb775
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/04/2019
ms.locfileid: "73463061"
---
# <a name="tutorial-create-a-grpc-client-and-server-in-aspnet-core"></a>Samouczek: Tworzenie gRPC klienta i serwera w ASP.NET Core

Przez [Jan Luo](https://github.com/juntaoluo)

W tym samouczku pokazano, jak utworzyć klienta programu .NET Core [gRPC](https://grpc.io/docs/guides/) oraz serwer gRPC ASP.NET Core.

Na koniec będziesz mieć klienta gRPC, który komunikuje się z usługą gRPC Greeter.

[Wyświetlanie lub Pobieranie przykładowego kodu](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/tutorials/grpc/grpc-start/sample) ([jak pobrać](xref:index#how-to-download-a-sample)).

W tym samouczku przedstawiono następujące instrukcje:

> [!div class="checklist"]
> * Utwórz serwer gRPC.
> * Utwórz klienta gRPC.
> * Przetestuj usługę klienta gRPC za pomocą usługi gRPC Greeter.

## <a name="prerequisites"></a>Wymagania wstępne

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

[!INCLUDE[](~/includes/net-core-prereqs-vs-3.0.md)]

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

[!INCLUDE[](~/includes/net-core-prereqs-vsc-3.0.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[Visual Studio dla komputerów Mac](#tab/visual-studio-mac)

[!INCLUDE[](~/includes/net-core-prereqs-mac-3.0.md)]

---

## <a name="create-a-grpc-service"></a>Tworzenie usługi gRPC

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

* Uruchom program Visual Studio i wybierz pozycję **Utwórz nowy projekt**. Alternatywnie z menu **plik** programu Visual Studio wybierz pozycję **Nowy** **projekt** > .
* W oknie dialogowym **Tworzenie nowego projektu** wybierz pozycję **Usługa gRPC** i wybierz pozycję **dalej**:

  ![\* * Utwórz nowy projekt * * okno dialogowe](~/tutorials/grpc/grpc-start/static/cnp.png)

* Nazwij projekt **GrpcGreeter**. Ważne jest, aby nazwa projektu *GrpcGreeter* , tak aby przestrzenie nazw były zgodne podczas kopiowania i wklejania kodu.
* Wybierz pozycję **Utwórz**.
* W oknie dialogowym **Tworzenie nowej usługi gRPC** :
  * Wybrano szablon **usługi gRPC** .
  * Wybierz pozycję **Utwórz**.

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

* Otwórz [zintegrowany terminal](https://code.visualstudio.com/docs/editor/integrated-terminal).
* Zmień katalog (`cd`) do folderu, który będzie zawierać projekt.
* Uruchom następujące polecenia:

  ```dotnetcli
  dotnet new grpc -o GrpcGreeter
  code -r GrpcGreeter
  ```

  * Polecenie `dotnet new` tworzy nową usługę gRPC w folderze *GrpcGreeter* .
  * Polecenie `code` otwiera folder *GrpcGreeter* w nowym wystąpieniu Visual Studio Code.

  Zostanie wyświetlone okno dialogowe z **wymaganymi zasobami do kompilowania i debugowania brakuje w "GrpcGreeter". Dodać je?**
* Wybierz pozycję **tak**.

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[Visual Studio dla komputerów Mac](#tab/visual-studio-mac)

W terminalu uruchom następujące polecenia:

```dotnetcli
dotnet new grpc -o GrpcGreeter
cd GrpcGreeter
```

Poprzednie polecenia używają [interfejs wiersza polecenia platformy .NET Core](/dotnet/core/tools/dotnet) do tworzenia usługi gRPC.

### <a name="open-the-project"></a>Otwórz projekt

W programie Visual Studio wybierz pozycję **plik** > **Otwórz**, a następnie wybierz plik *GrpcGreeter. csproj* .

---

### <a name="run-the-service"></a>Uruchamianie usługi

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

* Naciśnij `Ctrl+F5`, aby uruchomić usługę gRPC bez debugera.

  Program Visual Studio uruchamia usługę w wierszu polecenia.

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

* Uruchom gRPC Greeter projektu *GrpcGreeter* z wiersza polecenia przy użyciu `dotnet run`.

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[Visual Studio dla komputerów Mac](#tab/visual-studio-mac)

* Uruchom gRPC Greeter projektu *GrpcGreeter* z wiersza polecenia przy użyciu `dotnet run`.

---

Dzienniki pokazują, że usługa nasłuchuje na `https://localhost:5001`.

```console
info: Microsoft.Hosting.Lifetime[0]
      Now listening on: https://localhost:5001
info: Microsoft.Hosting.Lifetime[0]
      Application started. Press Ctrl+C to shut down.
info: Microsoft.Hosting.Lifetime[0]
      Hosting environment: Development
```

> [!NOTE]
> Szablon gRPC jest skonfigurowany do korzystania z [Transport Layer Security (TLS)](https://tools.ietf.org/html/rfc5246). Klienci gRPC muszą używać protokołu HTTPS, aby wywołać serwer.
>
> Program macOS nie obsługuje ASP.NET Core gRPC z protokołem TLS. Aby pomyślnie uruchomić usługi gRPC Services w usłudze macOS, wymagana jest dodatkowa konfiguracja. Aby uzyskać więcej informacji, zobacz [nie można uruchomić aplikacji ASP.NET Core gRPC w witrynie macOS](xref:grpc/troubleshoot#unable-to-start-aspnet-core-grpc-app-on-macos).

### <a name="examine-the-project-files"></a>Sprawdzanie plików projektu

*GrpcGreeter* pliki projektu:

* *Greeting. proto* &ndash; plik *Protos/Greeting. proto* definiuje `Greeter` gRPC i służy do generowania zasobów serwera gRPC. Aby uzyskać więcej informacji, zobacz [wprowadzenie do gRPC](xref:grpc/index).
* Folder *usługi* : zawiera implementację usługi `Greeter`.
* plik *appSettings. json* &ndash; zawiera dane konfiguracyjne, takie jak protokół używany przez Kestrel. Aby uzyskać więcej informacji, zobacz <xref:fundamentals/configuration/index>.
* *Program.cs* &ndash; zawiera punkt wejścia dla usługi gRPC. Aby uzyskać więcej informacji, zobacz <xref:fundamentals/host/generic-host>.
* *Startup.cs* &ndash; zawiera kod, który konfiguruje zachowanie aplikacji. Aby uzyskać więcej informacji, zobacz [Uruchamianie aplikacji](xref:fundamentals/startup).

## <a name="create-the-grpc-client-in-a-net-console-app"></a>Tworzenie klienta gRPC w aplikacji konsolowej .NET

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

* Otwórz drugie wystąpienie programu Visual Studio i wybierz pozycję **Utwórz nowy projekt**.
* W oknie dialogowym **Tworzenie nowego projektu** wybierz pozycję **aplikacja konsoli (.NET Core)** , a następnie wybierz pozycję **dalej**.
* W polu tekstowym **Nazwa** wprowadź **GrpcGreeterClient** , a następnie wybierz pozycję **Utwórz**.

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

* Otwórz [zintegrowany terminal](https://code.visualstudio.com/docs/editor/integrated-terminal).
* Zmień katalog (`cd`) do folderu, który będzie zawierać projekt.
* Uruchom następujące polecenia:

  ```dotnetcli
  dotnet new console -o GrpcGreeterClient
  code -r GrpcGreeterClient
  ```

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[Visual Studio dla komputerów Mac](#tab/visual-studio-mac)

Postępuj zgodnie z instrukcjami w temacie [Tworzenie kompletnego rozwiązania .NET Core w systemie macOS przy użyciu Visual Studio dla komputerów Mac](/dotnet/core/tutorials/using-on-mac-vs-full-solution) , aby utworzyć aplikację konsolową o nazwie *GrpcGreeterClient*.

---

### <a name="add-required-packages"></a>Dodaj wymagane pakiety

Projekt klienta gRPC wymaga następujących pakietów:

* [GRPC .NET. Client](https://www.nuget.org/packages/Grpc.Net.Client), który zawiera klienta .NET Core.
* [Google. protobuf](https://www.nuget.org/packages/Google.Protobuf/), który zawiera interfejsy API komunikatów protobuf C#dla.
* [GRPC. Tools](https://www.nuget.org/packages/Grpc.Tools/), która zawiera C# obsługę narzędzi dla plików protobuf. Pakiet narzędzi nie jest wymagany w czasie wykonywania, więc zależność jest oznaczona za pomocą `PrivateAssets="All"`.

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

Zainstaluj pakiety przy użyciu konsoli Menedżera pakietów (PMC) lub Zarządzaj pakietami NuGet.

#### <a name="pmc-option-to-install-packages"></a>Opcja PMC, aby zainstalować pakiety

* W programie Visual Studio wybierz kolejno pozycje **narzędzia** > **menedżer pakietów NuGet** > **konsola Menedżera pakietów**
* W oknie **konsola Menedżera pakietów** uruchom polecenie `cd GrpcGreeterClient`, aby zmienić katalogi na folder zawierający pliki *GrpcGreeterClient. csproj* .
* Uruchom następujące polecenia:

  ```powershell
  Install-Package Grpc.Net.Client
  Install-Package Google.Protobuf
  Install-Package Grpc.Tools
  ```

#### <a name="manage-nuget-packages-option-to-install-packages"></a>Opcja zarządzania pakietami NuGet w celu zainstalowania pakietów

* Kliknij prawym przyciskiem myszy projekt w **Eksplorator rozwiązań** > **Zarządzanie pakietami NuGet**
* Wybierz kartę **przeglądanie** .
* W polu wyszukiwania wprowadź **GRPC .NET. Client** .
* Wybierz pakiet **GRPC .NET. Client** z karty **Przeglądaj** i wybierz pozycję **Zainstaluj**.
* Powtórz dla `Google.Protobuf` i `Grpc.Tools`.

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

Uruchom następujące polecenia w **zintegrowanym terminalu**:

```dotnetcli
dotnet add GrpcGreeterClient.csproj package Grpc.Net.Client
dotnet add GrpcGreeterClient.csproj package Google.Protobuf
dotnet add GrpcGreeterClient.csproj package Grpc.Tools
```

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[Visual Studio dla komputerów Mac](#tab/visual-studio-mac)

* Kliknij prawym przyciskiem myszy folder **pakiety** w **okienko rozwiązania** > **Dodaj pakiety**
* W polu wyszukiwania wprowadź **GRPC .NET. Client** .
* Wybierz pakiet **GRPC .NET. Client** z okienka wyników, a następnie wybierz pozycję **Dodaj pakiet** .
* Powtórz dla `Google.Protobuf` i `Grpc.Tools`.

---

### <a name="add-greetproto"></a>Dodaj Greeting. proto

* Utwórz folder *Protos* w projekcie klienta gRPC.
* Skopiuj plik *Protos\greet.proto* z usługi gRPC Greeter do projektu klienta gRPC.
* Edytuj plik projektu *GrpcGreeterClient. csproj* :

  # <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

  Kliknij prawym przyciskiem myszy projekt i wybierz polecenie **Edytuj plik projektu**.

  # <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

  Wybierz plik *GrpcGreeterClient. csproj* .

  # <a name="visual-studio-for-mactabvisual-studio-mac"></a>[Visual Studio dla komputerów Mac](#tab/visual-studio-mac)

  Kliknij prawym przyciskiem myszy projekt i wybierz polecenie **narzędzia** > **Edytuj plik**.

  ---

* Dodaj grupę elementów z elementem `<Protobuf>`, który odwołuje się do pliku *Greeting. proto* :

  ```xml
  <ItemGroup>
    <Protobuf Include="Protos\greet.proto" GrpcServices="Client" />
  </ItemGroup>
  ```

### <a name="create-the-greeter-client"></a>Tworzenie klienta Greeter

Skompiluj projekt, aby utworzyć typy w przestrzeni nazw `GrpcGreeter`. Typy `GrpcGreeter` są generowane automatycznie przez proces kompilacji.

Zaktualizuj plik *program.cs* klienta gRPC za pomocą następującego kodu:

[!code-csharp[](~/tutorials/grpc/grpc-start/sample/GrpcGreeterClient/Program.cs?name=snippet2)]

*Program.cs* zawiera punkt wejścia i logikę dla klienta gRPC.

Klient Greeter jest tworzony przez:

* Utworzenie wystąpienia `GrpcChannel` zawierającego informacje służące do tworzenia połączenia z usługą gRPC.
* Używanie `GrpcChannel` do konstruowania klienta Greeter:

[!code-csharp[](~/tutorials/grpc/grpc-start/sample/GrpcGreeterClient/Program.cs?name=snippet&highlight=3-5)]

Klient Greeter wywołuje metodę asynchroniczną `SayHello`. Zostanie wyświetlony wynik wywołania `SayHello`:

[!code-csharp[](~/tutorials/grpc/grpc-start/sample/GrpcGreeterClient/Program.cs?name=snippet&highlight=6-8)]

## <a name="test-the-grpc-client-with-the-grpc-greeter-service"></a>Testowanie klienta gRPC za pomocą usługi gRPC Greeter

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

* W usłudze Greeter naciśnij `Ctrl+F5`, aby uruchomić serwer bez debugera.
* W projekcie `GrpcGreeterClient` naciśnij klawisz `Ctrl+F5`, aby uruchomić klienta bez debugera.

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

* Uruchom usługę Greeter.
* Uruchom klienta programu.


# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[Visual Studio dla komputerów Mac](#tab/visual-studio-mac)

* Uruchom usługę Greeter.
* Uruchom klienta programu.

---

Klient wysyła do usługi powitanie z komunikatem zawierającym nazwę *GreeterClient*. Usługa wysyła komunikat "Hello GreeterClient" jako odpowiedź. Odpowiedź "Hello GreeterClient" jest wyświetlana w wierszu polecenia:

```console
Greeting: Hello GreeterClient
Press any key to exit...
```

Usługa gRPC rejestruje szczegóły pomyślnego wywołania w dziennikach, które zostały zapisane w wierszu polecenia:

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

> [!NOTE]
> Kod w tym artykule wymaga certyfikatu programistycznego HTTPS ASP.NET Core do zabezpieczenia usługi gRPC. Jeśli klient kończy się niepowodzeniem z komunikatem `The remote certificate is invalid according to the validation procedure.`, certyfikat programistyczny nie jest zaufany. Aby uzyskać instrukcje dotyczące rozwiązania tego problemu, zobacz temat [ASP.NET Core ufanie certyfikatowi Deweloperskiemu protokołu HTTPS w systemie Windows i macOS](xref:security/enforcing-ssl#trust-the-aspnet-core-https-development-certificate-on-windows-and-macos).

[!INCLUDE[](~/includes/gRPCazure.md)]

### <a name="next-steps"></a>Następne kroki

* <xref:grpc/index>
* <xref:grpc/basics>
* <xref:grpc/migration>
