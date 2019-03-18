---
title: 'Samouczek: Rozpoczynanie pracy z usługą gRPC w programie ASP.NET Core'
author: juntaoluo
description: Tej serii samouczków pokazano, jak utworzyć gRPC usługi programu ASP.NET Core. Dowiedz się, jak utworzyć projekt usługi gRPC, Edytuj plik proto i dodać dupleks przesyłania strumieniowego wywołania.
ms.author: johluo
ms.date: 2/26/2019
uid: tutorials/grpc/grpc-start
ms.openlocfilehash: 793d13d0fbac4f2f46b7cd8490ded294e597ca9d
ms.sourcegitcommit: a467828b5e4eaae291d961ffe2279a571900de23
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/18/2019
ms.locfileid: "58142611"
---
# <a name="tutorial-get-started-with-grpc-service-in-aspnet-core"></a>Samouczek: Rozpoczynanie pracy z usługą gRPC w programie ASP.NET Core

Przez [Luo Jan](https://github.com/juntaoluo)

W tym samouczku pokazano podstawy tworzenia usługi gRPC programu ASP.NET Core.

Po zakończeniu będziesz mieć usługa gRPC, która zwraca greetings.

[!INCLUDE[View or download sample code](~/includes/grpc/download.md)]

W ramach tego samouczka możesz:

> [!div class="checklist"]
> * Utwórz usługę gRPC.
> * Uruchom usługę.
> * Sprawdź pliki projektu.

[!INCLUDE[](~/includes/net-core-prereqs-all-3.0.md)]

## <a name="create-a-grpc-service"></a>Tworzenie usługi gRPC

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

* W programie Visual Studio **pliku** menu, wybierz opcję **New** > **projektu**.
* Tworzenie nowej aplikacji sieci Web platformy ASP.NET Core.
  ![Nowa aplikacja internetowa ASP.NET Core](grpc-start/_static/np_3_0.1.png)
* Nadaj projektowi nazwę **GrpcGreeter**. Ważne jest, aby nadaj projektowi nazwę *GrpcGreeter* , przestrzenie nazw będą zgodne po skopiuj i Wklej kod.
  ![Nowa aplikacja internetowa ASP.NET Core](grpc-start/_static/np_3_0.2.png)
* Wybierz **platformy .NET Core** i **platformy ASP.NET Core 3.0** na liście rozwijanej. Wybierz **gRPC usługi** szablonu.

  Utworzono następujący projekt startowy:

  ![Eksplorator rozwiązań](grpc-start/_static/se3.0.png)

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

### <a name="test-the-service"></a>Testowanie usługi

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

* Upewnij się, **GrpcGreeter.Server** jest ustawiony jako projekt startowy i naciśnij klawisze Ctrl + F5, aby uruchomić usługę gRPC bez debugera.

  Visual Studio uruchamia usługę w wierszu polecenia. Dzienniki pokazują, że usługa uruchomiono nasłuchiwanie protokołu `http://localhost:50051`.

  ![Nowa aplikacja internetowa ASP.NET Core](grpc-start/_static/server_start.png)

* Gdy usługa jest uruchomiona, należy ustawić **GrpcGreeter.Client** jest ustawiony jako projekt startowy, a następnie naciśnij klawisz Ctrl + F5, aby uruchomić klienta bez debugera.

  Klient wysyła pozdrowienia z usługą za pomocą komunikatu zawierającego jego nazwę "GreeterClient". Usługa wyśle komunikat "Hello GreeterClient" jako odpowiedzi, który jest wyświetlany w wierszu polecenia.

  ![Nowa aplikacja internetowa ASP.NET Core](grpc-start/_static/client.png)

  Usługa rejestruje szczegółowe informacje o pomyślnym wywołaniem w dziennikach, zapisywany wiersza polecenia.

  ![Nowa aplikacja internetowa ASP.NET Core](grpc-start/_static/server_complete.png)

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[Visual Studio Code / Visual Studio for Mac](#tab/visual-studio-code+visual-studio-mac)

* Uruchom projekt serwera GrpcGreeter.Server z wiersza polecenia przy użyciu `dotnet run`. Dzienniki pokazują, że usługa uruchomiono nasłuchiwanie protokołu `http://localhost:50051`.

```console
info: Microsoft.Hosting.Lifetime[0]
      Now listening on: http://localhost:50051
info: Microsoft.Hosting.Lifetime[0]
      Application started. Press Ctrl+C to shut down.
info: Microsoft.Hosting.Lifetime[0]
      Hosting environment: Development
info: Microsoft.Hosting.Lifetime[0]
      Content root path: C:\example\GrpcGreeter\GrpcGreeter.Server
```

* Uruchom projekt klienta, GrpcGreeter.Client w osobnym wierszu polecenia przy użyciu `dotnet run`.

Klient wysyła pozdrowienia z usługą za pomocą komunikatu zawierającego jego nazwę "GreeterClient". Usługa wyśle komunikat "Hello GreeterClient" jako odpowiedzi, który jest wyświetlany w wierszu polecenia.

```console
Greeting: Hello GreeterClient
Press any key to exit...
```

Usługa rejestruje szczegółowe informacje o pomyślnym wywołaniem w dziennikach, zapisywany wiersza polecenia.

```console
info: Microsoft.Hosting.Lifetime[0]
      Now listening on: http://localhost:50051
info: Microsoft.Hosting.Lifetime[0]
      Application started. Press Ctrl+C to shut down.
info: Microsoft.Hosting.Lifetime[0]
      Hosting environment: Development
info: Microsoft.Hosting.Lifetime[0]
      Content root path: C:\gh\tp\GrpcGreeter\GrpcGreeter.Server
info: Microsoft.AspNetCore.Hosting.Internal.GenericWebHostService[1]
      Request starting HTTP/2 POST http://localhost:50051/Greet.Greeter/SayHello application/grpc
info: Microsoft.AspNetCore.Hosting.Internal.GenericWebHostService[2]
      Request finished in 107.46730000000001ms 200 application/grpc
```

<!-- End of combined VS/Mac tabs -->

---

### <a name="examine-the-project-files-of-the-grpc-project"></a>Przejrzyj pliki projektu gRPC projektu

GrpcGreeter.Server files:

* greet.proto: *Protos/greet.proto* plik definiuje `Greeter` gRPC i służy do generowania gRPC zasoby serwera. Aby uzyskać więcej informacji, zobacz <xref:grpc/index>.
* *Usługi* folderu: Zawiera implementację `Greeter` usługi.
* *appSettings.json*: zawierająca dane konfiguracyjne, takie jak protokół używany przez Kestrel. Aby uzyskać więcej informacji, zobacz <xref:fundamentals/configuration/index>.
* *Program.cs*: Zawiera punkt wejścia dla usługi gRPC. Aby uzyskać więcej informacji, zobacz <xref:fundamentals/host/web-host>.
* Startup.cs

Zawiera kod, który konfiguruje zachowanie aplikacji. Aby uzyskać więcej informacji, zobacz <xref:fundamentals/startup>.

gRPC client GrpcGreeter.Client file:

*Plik program.cs* zawiera punktu wejścia, a logika gRPC klienta.

W ramach tego samouczka możesz:

> [!div class="checklist"]
> * Usługa gRPC utworzona.
> * Uruchomiono usługę i klienta, aby przetestować usługę.
> * Zbadane plików projektu.
