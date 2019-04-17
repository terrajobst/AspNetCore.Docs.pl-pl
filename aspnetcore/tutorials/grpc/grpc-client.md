---
title: 'Samouczek: Tworzenie klienta gRPC platformy .NET Core'
author: juntaoluo
description: Tej serii samouczków pokazano, jak utworzyć gRPC usługi programu ASP.NET Core. Dowiedz się, jak utworzyć projekt usługi gRPC, Edytuj plik proto i dodać dupleks, przesyłanie strumieniowe wywołania.
monikerRange: '>= aspnetcore-3.0'
ms.author: johluo
ms.date: 4/10/2019
uid: tutorials/grpc/grpc-client
ms.openlocfilehash: 031afbfaf097c518a85400b0b6abbc135c1bc611
ms.sourcegitcommit: 57a974556acd09363a58f38c26f74dc21e0d4339
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/17/2019
ms.locfileid: "59675794"
---
# <a name="tutorial-create-a-net-core-grpc-client"></a>Samouczek: Tworzenie klienta gRPC platformy .NET Core

Przez [Luo Jan](https://github.com/juntaoluo)

W tym samouczku przedstawiono sposób tworzenia platformy .NET Core [gRPC](https://grpc.io/docs/guides/) klienta, który może komunikować się z usługami gRPC.

Po zakończeniu będziesz mieć klienta gRPC, który komunikuje się z gRPC Greeter usługi.

[!INCLUDE[View or download sample code](~/includes/grpc/downloadClient.md)]

W ramach tego samouczka możesz:

> [!div class="checklist"]
> * Tworzenie klienta gRPC.
> * Uruchom usługę względem gRPC usługi Greeter utworzony w poprzednim samouczku.
> * Sprawdź pliki projektu.

[!INCLUDE[](~/includes/net-core-prereqs-all-3.0.md)]

## <a name="create-a-net-console-application"></a>Utwórz aplikację konsolową .NET

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

Postępuj zgodnie z instrukcjami [tutaj](https://docs.microsoft.com/en-us/dotnet/core/tutorials/with-visual-studio) do tworzenia aplikacji konsolowej o nazwie *GrpcGreeterClient*.

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

Postępuj zgodnie z instrukcjami [tutaj](https://docs.microsoft.com/en-us/dotnet/core/tutorials/with-visual-studio-code) do tworzenia aplikacji konsolowej o nazwie *GrpcGreeterClient*.

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[Visual Studio for Mac](#tab/visual-studio-mac)

Postępuj zgodnie z instrukcjami [tutaj](https://docs.microsoft.com/en-us/dotnet/core/tutorials/using-on-mac-vs-full-solution) do tworzenia aplikacji konsolowej o nazwie *GrpcGreeterClient*.

<!-- End of VS tabs -->

---

## <a name="add-required-packages"></a>Dodawanie wymaganych pakietów

GRPC projekt klienta, należy dodać następujące pakiety:

* [Grpc.Core](https://www.nuget.org/packages/Grpc.Core), który zawiera C# interfejsu API klienta C-core.
* [Google.Protobuf](https://www.nuget.org/packages/Google.Protobuf/), który zawiera protobuf komunikatu interfejsów API na potrzeby C#.
* [Grpc.Tools](https://www.nuget.org/packages/Grpc.Tools/), który zawiera C# narzędzia do obsługi formatu protobuf plików. Pakiet narzędzi nie jest wymagane w czasie wykonywania, więc zależność jest oznaczona za pomocą `PrivateAssets="All"`.

Pakiety mogą być dodawane przy użyciu następujących metod:

### <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

### <a name="pmc-option-to-install-packages"></a>Konsolę zarządzania Pakietami opcję, aby zainstalować pakiety

* W programie Visual Studio, wybierz **narzędzia** > **Menedżera pakietów NuGet** > **Konsola Menedżera pakietów**
* Z **Konsola Menedżera pakietów** okna, przejdź do katalogu, w którym *GrpcGreeterClient.csproj* plik istnieje.
* Uruchom następujące polecenie:

    ```powershell
    Install-Package Grpc.Core
    ```

* Powtórz `Install-Package` Google.Protobuf i Grpc.Tools

<!-- Tutorials shouldn't have multiple options. Select what you think is the best approach. Recommend you removed this approach. -->

### <a name="manage-nuget-packages-option-to-install-packages"></a>Zarządzanie pakietami NuGet opcję, aby zainstalować pakiety

* Kliknij prawym przyciskiem myszy projekt w **Eksploratora rozwiązań** > **Zarządzaj pakietami NuGet**
* Ustaw **źródła pakietu** na stronie "nuget.org"
* W polu wyszukiwania wprowadź "Grpc.Core"
* Wybierz pakiet "Grpc.Core" z **Przeglądaj** kartę, a następnie kliknij przycisk **instalacji**
* Powtórz te czynności dla Google.Protobuf i Grpc.Tools

### <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

Uruchom następujące polecenie z **zintegrowany Terminal**:

```console
dotnet add TodoApi.csproj package Grpc.Core
```

Powtórz te czynności dla Google.Protobuf i Grpc.Tools

### <a name="visual-studio-for-mactabvisual-studio-mac"></a>[Visual Studio for Mac](#tab/visual-studio-mac)

* Kliknij prawym przyciskiem myszy *pakietów* folderu w **konsoli rozwiązania** > **Dodawanie pakietów...**
* Ustaw **Dodawanie pakietów** okna **źródła** menu rozwijane "nuget.org"
* W polu wyszukiwania wprowadź "Grpc.Core"
* Wybierz pakiet "Grpc.Core" w okienku wyników, a następnie kliknij przycisk **Dodaj pakiet**
* Powtórz te czynności dla Google.Protobuf i Grpc.Tools

---

## <a name="add-the-greetproto-file"></a>Dodaj plik greet.proto

Kopiuj **Protos\greet.proto** pliku z gRPC Greeter usługi do projektu klienta gRPC. Dodaj **greet.proto** plik `<Protobuf>` grupy elementów GrpcGreeterClient pliku projektu:

```XML
<Protobuf Include="Protos\greet.proto" GrpcServices="Client" />
```

> [!NOTE]
> Plik projektu GrpcGreeterClient można otworzyć, klikając prawym przyciskiem myszy projekt i wybierając polecenie **Edytuj GrpcGreeterClient.csproj** opcję z menu rozwijanego.
>
> ![Nowa aplikacja internetowa ASP.NET Core](grpc-start/_static/edit_csproj.png)
>
> Alternatywnie, przejdź do katalogu GrpcGreeterClient i edytować `GrpcGreeterClient.csproj` za pomocą ulubionego edytora.

`GrpcServices="Client"` Jest dodawany, aby tylko atrybut C# zasobów klienta są generowane dla pliku protobuf uwzględnione. Skompiluj projekt klienta, aby wyzwolić Generowanie C# zasobów klienta.

## <a name="create-a-greeterclient-and-invoke-the-sayhello-unary-call"></a>Tworzenie GreeterClient i wywoływanie SayHello wywołanie jednoargumentowy

Dodaj następujący kod do `Main` metody `Program.cs` pliku projektu klienta gRPC:

[!code-cs[](~/tutorials/grpc/grpc-start/samples/GrpcGreeterClient/Program.cs?name=snippet)]

Dostęp do wymaganych typów używając następujących instrukcji są wymagane do:

[!code-cs[](~/tutorials/grpc/grpc-start/samples/GrpcGreeterClient/Program.cs?name=using)]

GreeterClient jest tworzony przez utworzenie wystąpienia `Channel` zawierających informacje dotyczące tworzenia połączenia z usługą gRPC i używania go do konstruowania `GreeterClient`:

[!code-cs[](~/tutorials/grpc/grpc-start/samples/GrpcGreeterClient/Program.cs?name=snippet&highlight=4-5)]

GreeterClient zawiera wywołanie jednoargumentowe `SayHello` które mogą być wywoływane asynchronicznie:

[!code-cs[](~/tutorials/grpc/grpc-start/samples/GrpcGreeterClient/Program.cs?name=snippet&highlight=7-8)]

Wyniki `SayHello` wywołania są przechowywane w `reply` które następnie mogą być wyświetlane:

[!code-cs[](~/tutorials/grpc/grpc-start/samples/GrpcGreeterClient/Program.cs?name=snippet&highlight=9)]

`Channel` Używany przez klienta należy zamknąć po zakończeniu operacji zwolnić wszystkie zasoby:

[!code-cs[](~/tutorials/grpc/grpc-start/samples/GrpcGreeterClient/Program.cs?name=snippet&highlight=11)]

> [!NOTE]
> Będą potrzebne do tworzenia projektu przed typów w **Greeter** przestrzeni nazw mogą być rozwiązane. Te typy są generowane automatycznie podczas kompilacji i nie być dostępne przed uruchomieniem kompilacji.

## <a name="test-the-grpc-client-with-the-grpc-greeter-service"></a>Testowanie klienta gRPC gRPC Greeter usługi

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

* Upewnij się, Greeter utworzony w poprzednim samouczku jest uruchomiona.

* Gdy usługa jest uruchomiona, wróć do **GrpcGreeterClient** projektu jest ustawiony jako projekt startowy. Naciśnij klawisze Ctrl + F5, aby uruchomić klienta bez debugera.

  Klient wysyła pozdrowienia z usługą za pomocą komunikatu zawierającego jego nazwę "GreeterClient". Usługa wyśle komunikat "Hello GreeterClient" jako odpowiedzi, który jest wyświetlany w wierszu polecenia.

  ![Nowa aplikacja internetowa ASP.NET Core](grpc-start/_static/client.png)

  Usługa rejestruje szczegółowe informacje o pomyślnym wywołaniem w dziennikach, zapisywany wiersza polecenia.

  ![Nowa aplikacja internetowa ASP.NET Core](grpc-start/_static/server_complete.png)

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[Visual Studio Code / Visual Studio for Mac](#tab/visual-studio-code+visual-studio-mac)

* Upewnij się, Greeter utworzony w poprzednim samouczku jest uruchomiona.

* Uruchom projekt klienta, GrpcGreeter.Client w osobnym wierszu polecenia przy użyciu `dotnet run`.

Klient wysyła pozdrowienia z usługą za pomocą komunikatu zawierającego jego nazwę "GreeterClient". Usługa wyśle komunikat "Hello GreeterClient" jako odpowiedzi, który jest wyświetlany w wierszu polecenia.

```console
Greeting: Hello GreeterClient
Press any key to exit...
```

Usługa rejestruje szczegółowe informacje o pomyślnym wywołaniem w dziennikach, zapisywany wiersza polecenia.

```console
dbug: Grpc.AspNetCore.Server.Internal.GrpcServiceBinder[1]
      Added gRPC method 'SayHello' to service 'Greet.Greeter'. Method type: 'Unary', route pattern: '/Greet.Greeter/SayHello'.
info: Microsoft.Hosting.Lifetime[0]
      Now listening on: http://localhost:50051
info: Microsoft.Hosting.Lifetime[0]
      Application started. Press Ctrl+C to shut down.
info: Microsoft.Hosting.Lifetime[0]
      Hosting environment: Development
info: Microsoft.Hosting.Lifetime[0]
      Content root path: C:\gh\Docs\aspnetcore\tutorials\grpc\grpc-start\samples\GrpcGreeter
info: Microsoft.AspNetCore.Hosting.Internal.GenericWebHostService[1]
      Request starting HTTP/2 POST http://localhost:50051/Greet.Greeter/SayHello application/grpc
info: Microsoft.AspNetCore.Routing.EndpointMiddleware[0]
      Executing endpoint 'gRPC - /Greet.Greeter/SayHello'
info: Microsoft.AspNetCore.Routing.EndpointMiddleware[1]
      Executed endpoint 'gRPC - /Greet.Greeter/SayHello'
info: Microsoft.AspNetCore.Hosting.Internal.GenericWebHostService[2]
      Request finished in 194.5798ms 200 application/grpc
```

<!-- End of combined VS/Mac tabs -->

---

### <a name="examine-the-project-files-of-the-grpc-project"></a>Przejrzyj pliki projektu gRPC projektu

gRPC client GrpcGreeterClient file:

*Plik program.cs* zawiera punktu wejścia, a logika gRPC klienta.

## <a name="additional-resources"></a>Dodatkowe zasoby

W ramach tego samouczka możesz:

> [!div class="checklist"]
> * Tworzenie klienta gRPC.
> * Uruchom usługę względem gRPC usługi Greeter utworzony w poprzednim samouczku.
> * Sprawdź pliki projektu.

> [!div class="step-by-step"]
> [Poprzednie: Utwórz gRPC Greeter usługi](xref:tutorials/grpc/grpc-start)
