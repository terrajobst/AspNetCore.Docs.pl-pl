---
title: Tworzenie platformy .NET Core gRPC klienta i serwera w programie ASP.NET Core
author: juntaoluo
description: W tym samouczku przedstawiono sposób tworzenia klienta usługi i gRPC gRPC programu ASP.NET Core. Dowiedz się, jak utworzyć projekt usługi gRPC, Edytuj plik proto i dodać dupleks, przesyłanie strumieniowe wywołania.
monikerRange: '>= aspnetcore-3.0'
ms.author: johluo
ms.date: 5/30/2019
uid: tutorials/grpc/grpc-start
ms.openlocfilehash: a0bb5c087a712ccd890344d2fc52cc58adc914ab
ms.sourcegitcommit: a04eb20e81243930ec829a9db5dd5de49f669450
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/03/2019
ms.locfileid: "66470421"
---
# <a name="tutorial-create-a-grpc-client-and-server-in-aspnet-core"></a>Samouczek: Utworzenie gRPC klienta i serwera w programie ASP.NET Core

Przez [Luo Jan](https://github.com/juntaoluo)

W tym samouczku przedstawiono sposób tworzenia platformy .NET Core [gRPC](https://grpc.io/docs/guides/) klienta i ASP.NET Core gRPC serwera.

Po zakończeniu będziesz mieć klienta gRPC, który komunikuje się z gRPC Greeter usługi.

[Wyświetlanie lub pobieranie przykładowego kodu](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/tutorials/grpc/grpc-start/sample) ([sposobu pobierania](xref:index#how-to-download-a-sample)).

W ramach tego samouczka możesz:

> [!div class="checklist"]
> * Utwórz gRPC serwera.
> * Tworzenie klienta gRPC.
> * Przetestuj usługę klienta gRPC z gRPC Greeter usługi.

[!INCLUDE[](~/includes/net-core-prereqs-all-3.0.md)]

## <a name="create-a-grpc-service"></a>Tworzenie usługi gRPC

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

* W programie Visual Studio **pliku** menu, wybierz opcję **New** > **projektu**.
* W **Utwórz nowy projekt** okno dialogowe, wybierz opcję **aplikacji sieci Web programu ASP.NET Core**.
* Wybierz **dalej**
* Nadaj projektowi nazwę **GrpcGreeter**. Ważne jest, aby nadaj projektowi nazwę *GrpcGreeter* , przestrzenie nazw będą zgodne po skopiuj i Wklej kod.
* Wybierz pozycję **Utwórz**
* W **Tworzenie nowej aplikacji sieci Web platformy ASP.NET Core** okno dialogowe:
  * Wybierz **platformy .NET Core** i **platformy ASP.NET Core 3.0** w menu rozwijanych. 
  * Wybierz **gRPC usługi** szablonu.
  * Wybierz pozycję **Utwórz**

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

* Otwórz [zintegrowany terminal](https://code.visualstudio.com/docs/editor/integrated-terminal).
* Zmień katalog (`cd`) do folderu, który będzie zawierać projekt.
* Uruchom następujące polecenia:

  ```console
  dotnet new grpc -o GrpcGreeter
  code -r GrpcGreeter
  ```

  * `dotnet new` Polecenie tworzy nową usługę gRPC w *GrpcGreeter* folderu.
  * `code` Polecenia otwiera *GrpcGreeter* folderu w nowym wystąpieniu programu Visual Studio Code.

  Zostanie wyświetlone okno dialogowe z **"GrpcGreeter" brakuje wymagane zasoby do tworzenia i debugowania. Dodaj je?**
* Wybierz **tak**

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[Visual Studio for Mac](#tab/visual-studio-mac)

W terminalu uruchom następujące polecenia:

```console
  dotnet new grpc -o GrpcGreeter
  cd GrpcGreeter
```

Poprzednie polecenia użyj [interfejsu wiersza polecenia platformy .NET Core](/dotnet/core/tools/dotnet) utworzyć usługę gRPC.

### <a name="open-the-project"></a>Otwórz projekt

Z programu Visual Studio, wybierz **Plik > Otwórz**, a następnie wybierz pozycję *GrpcGreeter.sln* pliku.

<!-- End of VS tabs -->

---

### <a name="run-the-service"></a>Uruchom usługę

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

* Naciśnij klawisze Ctrl + F5, aby uruchomić usługę gRPC bez debugera.

  Visual Studio uruchamia usługę w wierszu polecenia.

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[Visual Studio Code / Visual Studio for Mac](#tab/visual-studio-code+visual-studio-mac)

* Uruchom projekt Greeter gRPC GrpcGreeter z wiersza polecenia przy użyciu `dotnet run`.

<!-- End of combined VS/Mac tabs -->

---

Dzienniki przedstawiają nasłuchiwanie usługi na `http://localhost:50051`.

```console
info: Microsoft.Hosting.Lifetime[0]
      Now listening on: http://localhost:50051
info: Microsoft.Hosting.Lifetime[0]
      Application started. Press Ctrl+C to shut down.
info: Microsoft.Hosting.Lifetime[0]
      Hosting environment: Development
info: Microsoft.Hosting.Lifetime[0]
```

### <a name="examine-the-project-files"></a>Przejrzyj pliki projektu

Pliki GrpcGreeter:

* *greet.proto*: *Protos/greet.proto* plik definiuje `Greeter` gRPC i służy do generowania gRPC zasoby serwera. Aby uzyskać więcej informacji, zobacz [wprowadzenie do gRPC](xref:grpc/index).
* *Usługi* folderu: Zawiera implementację `Greeter` usługi.
* *appSettings.json*: Zawiera dane konfiguracyjne, takie jak protokół używany przez Kestrel. Aby uzyskać więcej informacji, zobacz <xref:fundamentals/configuration/index>.
* *Program.cs*: Zawiera punkt wejścia dla usługi gRPC. Aby uzyskać więcej informacji, zobacz <xref:fundamentals/host/web-host>.
* *Startup.cs*: Zawiera kod, który konfiguruje zachowanie aplikacji. Aby uzyskać więcej informacji, zobacz [uruchamiania aplikacji](xref:fundamentals/startup).

## <a name="create-the-grpc-client-in-a-net-console-app"></a>Tworzenie klienta gRPC w aplikacji konsoli .NET

## <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

* Wybierz **pliku** > **New** > **projektu** z paska menu.
* W **Utwórz nowy projekt** okno dialogowe, wybierz opcję **Aplikacja konsoli (.NET Core)** .
* Wybierz **dalej**
* W **nazwa** tekstu wprowadź "GrpcGreeterClient".
* Wybierz pozycję **Utwórz**.

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

* Otwórz [zintegrowany terminal](https://code.visualstudio.com/docs/editor/integrated-terminal).
* Zmień katalog (`cd`) do folderu, który będzie zawierać projekt.
* Uruchom następujące polecenia:

```console
dotnet new console -o GrpcGreeterClient
code -r GrpcGreeterClient
```

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[Visual Studio for Mac](#tab/visual-studio-mac)

Postępuj zgodnie z instrukcjami [tutaj](/dotnet/core/tutorials/using-on-mac-vs-full-solution) do tworzenia aplikacji konsolowej o nazwie *GrpcGreeterClient*.

<!-- End of VS tabs -->

---

### <a name="add-required-packages"></a>Dodawanie wymaganych pakietów

GRPC projekt klienta, należy dodać następujące pakiety:

* [Grpc.Core](https://www.nuget.org/packages/Grpc.Core), który zawiera C# interfejsu API klienta C-core.
* [Google.Protobuf](https://www.nuget.org/packages/Google.Protobuf/), który zawiera protobuf komunikatu interfejsów API na potrzeby C#.
* [Grpc.Tools](https://www.nuget.org/packages/Grpc.Tools/), który zawiera C# narzędzia do obsługi formatu protobuf plików. Pakiet narzędzi nie jest wymagane w czasie wykonywania, więc zależność jest oznaczona za pomocą `PrivateAssets="All"`.

### <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

Instalowanie pakietów przy użyciu konsoli Menedżera pakietów (PMC) "lub" Zarządzaj pakietami NuGet

#### <a name="pmc-option-to-install-packages"></a>Konsolę zarządzania Pakietami opcję, aby zainstalować pakiety

* W programie Visual Studio, wybierz **narzędzia** > **Menedżera pakietów NuGet** > **Konsola Menedżera pakietów**
* Z **Konsola Menedżera pakietów** okna, przejdź do katalogu, w którym *GrpcGreeterClient.csproj* plik istnieje.
* Uruchom następujące polecenia:

 ```powershell
Install-Package Grpc.Core
Install-Package Grpc.Protobuf
Install-Package Grpc.Tools
```

#### <a name="manage-nuget-packages-option-to-install-packages"></a>Zarządzanie pakietami NuGet opcję, aby zainstalować pakiety

* Kliknij prawym przyciskiem myszy projekt w **Eksploratora rozwiązań** > **Zarządzaj pakietami NuGet**
* Wybierz **Przeglądaj** kartę.
* Wprowadź **Grpc.Core** w polu wyszukiwania.
* Wybierz **Grpc.Core** pakietu z **Przeglądaj** kartę, a następnie wybierz pozycję **zainstalować**.
* Powtórz tę procedurę dla `Google.Protobuf` i `Grpc.Tools`.

### <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

Uruchom następujące polecenia z **zintegrowany Terminal**:

```console
dotnet add GrpcGreeterClient.csproj package Grpc.Core
dotnet add GrpcGreeterClient.csproj package Google.Protobuf
dotnet add GrpcGreeterClient.csproj package Grpc.Tools
```

### <a name="visual-studio-for-mactabvisual-studio-mac"></a>[Visual Studio for Mac](#tab/visual-studio-mac)

* Kliknij prawym przyciskiem myszy **pakietów** folderu w **konsoli rozwiązania** > **Dodawanie pakietów**
* Wprowadź **Grpc.Core** w polu wyszukiwania.
* Wybierz **Grpc.Core** pakietu w okienku wyników, a następnie wybierz pozycję **Dodaj pakiet**
* Powtórz tę procedurę dla `Google.Protobuf` i `Grpc.Tools`.

---

### <a name="add-greetproto"></a>Dodaj greet.proto

* Tworzenie **Protos** folderu w projekcie gRPC klienta.
* Kopiuj **Protos\greet.proto** pliku z gRPC Greeter usługi do projektu klienta gRPC.
* Edytuj *GrpcGreeterClient.csproj* pliku projektu:

  # <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio) 

  Kliknij prawym przyciskiem myszy projekt i wybierz **Edytuj GrpcGreeterClient.csproj**.

  # <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code) 

  Wybierz *GrpcGreeterClient.csproj* pliku.

  # <a name="visual-studio-for-mactabvisual-studio-mac"></a>[Visual Studio for Mac](#tab/visual-studio-mac)

  Kliknij prawym przyciskiem myszy projekt i wybierz **Narzędzia > Edytuj plik**.

  ---

* Dodaj **greet.proto** plik `<Protobuf>` grupy elementów GrpcGreeterClient pliku projektu:

  ```XML
  <ItemGroup>
    <Protobuf Include="Protos\greet.proto" GrpcServices="Client" />
  </ItemGroup>
  ```

Skompiluj projekt klienta, aby wyzwolić Generowanie C# zasobów klienta.

### <a name="create-the-greater-client"></a>Utwórz większe klienta

Skompiluj projekt, aby utworzyć typy w **Greeter** przestrzeni nazw. `Greeter` Typy są generowane automatycznie przez proces kompilacji.

Aktualizacja klienta gRPC *Program.cs* pliku następującym kodem:

[!code-cs[](~/tutorials/grpc/grpc-start/sample/GrpcGreeterClient/Program.cs?name=snippet2)]

*Plik program.cs* zawiera punktu wejścia, a logika gRPC klienta.

Większa klienta jest tworzony przez:

* Utworzenie wystąpienia `Channel` zawierające informacje dotyczące tworzenia połączenia z usługą gRPC.
* Za pomocą `Channel` do konstruowania większa klienta:

[!code-cs[](~/tutorials/grpc/grpc-start/sample/GrpcGreeterClient/Program.cs?name=snippet&highlight=4-6)]

Większa klient wywołuje asynchroniczną `SayHello` metody. Wynik `SayHello` wywołań jest wyświetlany:

[!code-cs[](~/tutorials/grpc/grpc-start/sample/GrpcGreeterClient/Program.cs?name=snippet&highlight=7-9)]

Zamknij `Channel` użytą przez klienta po zakończeniu operacji zwolnić wszystkie zasoby.

## <a name="test-the-grpc-client-with-the-grpc-greeter-service"></a>Testowanie klienta gRPC gRPC Greeter usługi

### <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

* W usłudze Greeter naciśnij klawisze Ctrl + F5, aby uruchomić serwer bez debugera.
* W projekcie GrpcGreeterClient naciśnij klawisze Ctrl + F5, aby uruchomić serwer bez debugera.

### <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[Visual Studio Code / Visual Studio for Mac](#tab/visual-studio-code+visual-studio-mac)

* Uruchom usługę Greeter.
* Uruchom klienta.

<!-- End of combined VS/Mac tabs -->

---

Klient wysyła pozdrowienia z usługą za pomocą komunikatu zawierającego jego nazwę "GreeterClient". Usługa wysyła komunikat "Hello GreeterClient", jako odpowiedź. Odpowiedź "Hello GreeterClient" jest wyświetlana w wierszu polecenia:

```console
Greeting: Hello GreeterClient
Press any key to exit...
```

Usługa gRPC rejestrujący szczegóły dotyczące pomyślnego wywołania w dziennikach, zapisywany wiersza polecenia.

```console
info: Microsoft.Hosting.Lifetime[0]
      Now listening on: http://localhost:50051
info: Microsoft.Hosting.Lifetime[0]
      Application started. Press Ctrl+C to shut down.
info: Microsoft.Hosting.Lifetime[0]
      Hosting environment: Development
info: Microsoft.Hosting.Lifetime[0]
      Content root path: C:\GH\aspnet\docs\4\Docs\aspnetcore\tutorials\grpc\grpc-start\sample\GrpcGreeter
info: Microsoft.AspNetCore.Hosting.Diagnostics[1]
      Request starting HTTP/2 POST http://localhost:50051/Greet.Greeter/SayHello application/grpc
info: Microsoft.AspNetCore.Routing.EndpointMiddleware[0]
      Executing endpoint 'gRPC - /Greet.Greeter/SayHello'
info: Microsoft.AspNetCore.Routing.EndpointMiddleware[1]
      Executed endpoint 'gRPC - /Greet.Greeter/SayHello'
info: Microsoft.AspNetCore.Hosting.Diagnostics[2]
      Request finished in 78.32260000000001ms 200 application/grpc
```

### <a name="next-steps"></a>Następne kroki

* <xref:grpc/index>
* <xref:grpc/basics>
* <xref:grpc/migration>
