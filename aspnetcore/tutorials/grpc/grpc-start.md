---
title: 'Samouczek: Rozpoczynanie pracy z usługą gRPC w programie ASP.NET Core'
author: juntaoluo
description: Tej serii samouczków pokazano, jak utworzyć gRPC usługi programu ASP.NET Core. Dowiedz się, jak utworzyć projekt usługi gRPC, Edytuj plik proto i dodać dupleks przesyłania strumieniowego wywołania.
monikerRange: '>= aspnetcore-3.0'
ms.author: johluo
ms.date: 2/26/2019
uid: tutorials/grpc/grpc-start
ms.openlocfilehash: 5f7a2f6b57804b3295b23c322dcbac553b05528b
ms.sourcegitcommit: 088e6744cd67a62f214f25146313a53949b17d35
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/21/2019
ms.locfileid: "58320059"
---
# <a name="tutorial-get-started-with-grpc-service-in-aspnet-core"></a><span data-ttu-id="07beb-104">Samouczek: Wprowadzenie do usługi gRPC na platformie ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="07beb-104">Tutorial: Get started with gRPC service in ASP.NET Core</span></span>

<span data-ttu-id="07beb-105">Przez [Luo Jan](https://github.com/juntaoluo)</span><span class="sxs-lookup"><span data-stu-id="07beb-105">By [John Luo](https://github.com/juntaoluo)</span></span>

<span data-ttu-id="07beb-106">W tym samouczku pokazano podstawy tworzenia usługi gRPC programu ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="07beb-106">This tutorial teaches the basics of building a gRPC service on ASP.NET Core.</span></span>

<span data-ttu-id="07beb-107">Po zakończeniu będziesz mieć usługa gRPC, która zwraca greetings.</span><span class="sxs-lookup"><span data-stu-id="07beb-107">At the end, you'll have a gRPC service that echoes greetings.</span></span>

[!INCLUDE[View or download sample code](~/includes/grpc/download.md)]

<span data-ttu-id="07beb-108">W ramach tego samouczka możesz:</span><span class="sxs-lookup"><span data-stu-id="07beb-108">In this tutorial, you:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="07beb-109">Utwórz usługę gRPC.</span><span class="sxs-lookup"><span data-stu-id="07beb-109">Create a gRPC service.</span></span>
> * <span data-ttu-id="07beb-110">Uruchom usługę.</span><span class="sxs-lookup"><span data-stu-id="07beb-110">Run the service.</span></span>
> * <span data-ttu-id="07beb-111">Sprawdź pliki projektu.</span><span class="sxs-lookup"><span data-stu-id="07beb-111">Examine the project files.</span></span>

[!INCLUDE[](~/includes/net-core-prereqs-all-3.0.md)]

## <a name="create-a-grpc-service"></a><span data-ttu-id="07beb-112">Tworzenie usługi gRPC</span><span class="sxs-lookup"><span data-stu-id="07beb-112">Create a gRPC service</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="07beb-113">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="07beb-113">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="07beb-114">W programie Visual Studio **pliku** menu, wybierz opcję **New** > **projektu**.</span><span class="sxs-lookup"><span data-stu-id="07beb-114">From the Visual Studio **File** menu, select **New** > **Project**.</span></span>
* <span data-ttu-id="07beb-115">Tworzenie nowej aplikacji sieci Web platformy ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="07beb-115">Create a new ASP.NET Core Web Application.</span></span>
  <span data-ttu-id="07beb-116">![Nowa aplikacja internetowa ASP.NET Core](grpc-start/_static/np_3_0.1.png)</span><span class="sxs-lookup"><span data-stu-id="07beb-116">![new ASP.NET Core Web Application](grpc-start/_static/np_3_0.1.png)</span></span>
* <span data-ttu-id="07beb-117">Nadaj projektowi nazwę **GrpcGreeter**.</span><span class="sxs-lookup"><span data-stu-id="07beb-117">Name the project **GrpcGreeter**.</span></span> <span data-ttu-id="07beb-118">Ważne jest, aby nadaj projektowi nazwę *GrpcGreeter* , przestrzenie nazw będą zgodne po skopiuj i Wklej kod.</span><span class="sxs-lookup"><span data-stu-id="07beb-118">It's important to name the project *GrpcGreeter* so the namespaces will match when you copy and paste code.</span></span>
  <span data-ttu-id="07beb-119">![Nowa aplikacja internetowa ASP.NET Core](grpc-start/_static/np_3_0.2.png)</span><span class="sxs-lookup"><span data-stu-id="07beb-119">![new ASP.NET Core Web Application](grpc-start/_static/np_3_0.2.png)</span></span>
* <span data-ttu-id="07beb-120">Wybierz **platformy .NET Core** i **platformy ASP.NET Core 3.0** na liście rozwijanej.</span><span class="sxs-lookup"><span data-stu-id="07beb-120">Select **.NET Core** and **ASP.NET Core 3.0** in the dropdown.</span></span> <span data-ttu-id="07beb-121">Wybierz **gRPC usługi** szablonu.</span><span class="sxs-lookup"><span data-stu-id="07beb-121">Choose the **gRPC Service** template.</span></span>

  <span data-ttu-id="07beb-122">Utworzono następujący projekt startowy:</span><span class="sxs-lookup"><span data-stu-id="07beb-122">The following starter project is created:</span></span>

  ![Eksplorator rozwiązań](grpc-start/_static/se3.0.png)

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="07beb-124">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="07beb-124">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="07beb-125">Otwórz [zintegrowany terminal](https://code.visualstudio.com/docs/editor/integrated-terminal).</span><span class="sxs-lookup"><span data-stu-id="07beb-125">Open the [integrated terminal](https://code.visualstudio.com/docs/editor/integrated-terminal).</span></span>
* <span data-ttu-id="07beb-126">Zmień katalog (`cd`) do folderu, który będzie zawierać projekt.</span><span class="sxs-lookup"><span data-stu-id="07beb-126">Change directories (`cd`) to a folder which will contain the project.</span></span>
* <span data-ttu-id="07beb-127">Uruchom następujące polecenia:</span><span class="sxs-lookup"><span data-stu-id="07beb-127">Run the following commands:</span></span>

  ```console
  dotnet new grpc -o GrpcGreeter
  code -r GrpcGreeter
  ```

  * <span data-ttu-id="07beb-128">`dotnet new` Polecenie tworzy nową usługę gRPC w *GrpcGreeter* folderu.</span><span class="sxs-lookup"><span data-stu-id="07beb-128">The `dotnet new` command creates a new gRPC service in the *GrpcGreeter* folder.</span></span>
  * <span data-ttu-id="07beb-129">`code` Polecenia otwiera *GrpcGreeter* folderu w nowym wystąpieniu programu Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="07beb-129">The `code` command opens the *GrpcGreeter* folder in a new instance of Visual Studio Code.</span></span>

  <span data-ttu-id="07beb-130">Zostanie wyświetlone okno dialogowe z **"GrpcGreeter" brakuje wymagane zasoby do tworzenia i debugowania. Dodaj je?**</span><span class="sxs-lookup"><span data-stu-id="07beb-130">A dialog box appears with **Required assets to build and debug are missing from 'GrpcGreeter'. Add them?**</span></span>
* <span data-ttu-id="07beb-131">Wybierz **tak**</span><span class="sxs-lookup"><span data-stu-id="07beb-131">Select **Yes**</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="07beb-132">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="07beb-132">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="07beb-133">W terminalu uruchom następujące polecenia:</span><span class="sxs-lookup"><span data-stu-id="07beb-133">From a terminal, run the following commands:</span></span>

```console
  dotnet new grpc -o GrpcGreeter
  cd GrpcGreeter
```

<span data-ttu-id="07beb-134">Poprzednie polecenia użyj [interfejsu wiersza polecenia platformy .NET Core](/dotnet/core/tools/dotnet) utworzyć usługę gRPC.</span><span class="sxs-lookup"><span data-stu-id="07beb-134">The preceding commands use the [.NET Core CLI](/dotnet/core/tools/dotnet) to create a gRPC service.</span></span>

### <a name="open-the-project"></a><span data-ttu-id="07beb-135">Otwórz projekt</span><span class="sxs-lookup"><span data-stu-id="07beb-135">Open the project</span></span>

<span data-ttu-id="07beb-136">Z programu Visual Studio, wybierz **Plik > Otwórz**, a następnie wybierz pozycję *GrpcGreeter.sln* pliku.</span><span class="sxs-lookup"><span data-stu-id="07beb-136">From Visual Studio, select **File > Open**, and then select the *GrpcGreeter.sln* file.</span></span>

<!-- End of VS tabs -->

---

### <a name="test-the-service"></a><span data-ttu-id="07beb-137">Testowanie usługi</span><span class="sxs-lookup"><span data-stu-id="07beb-137">Test the service</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="07beb-138">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="07beb-138">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="07beb-139">Upewnij się, **GrpcGreeter.Server** jest ustawiony jako projekt startowy i naciśnij klawisze Ctrl + F5, aby uruchomić usługę gRPC bez debugera.</span><span class="sxs-lookup"><span data-stu-id="07beb-139">Ensure the **GrpcGreeter.Server** is set as the Startup Project and press Ctrl+F5 to run the gRPC service without the debugger.</span></span>

  <span data-ttu-id="07beb-140">Visual Studio uruchamia usługę w wierszu polecenia.</span><span class="sxs-lookup"><span data-stu-id="07beb-140">Visual Studio runs the service in a command prompt.</span></span> <span data-ttu-id="07beb-141">Dzienniki pokazują, że usługa uruchomiono nasłuchiwanie protokołu `http://localhost:50051`.</span><span class="sxs-lookup"><span data-stu-id="07beb-141">The logs show that the service started listening on `http://localhost:50051`.</span></span>

  ![Nowa aplikacja internetowa ASP.NET Core](grpc-start/_static/server_start.png)

* <span data-ttu-id="07beb-143">Gdy usługa jest uruchomiona, należy ustawić **GrpcGreeter.Client** jest ustawiony jako projekt startowy, a następnie naciśnij klawisz Ctrl + F5, aby uruchomić klienta bez debugera.</span><span class="sxs-lookup"><span data-stu-id="07beb-143">Once the service is running, set the **GrpcGreeter.Client** is set as the Startup Project and press Ctrl+F5 to run the client without the debugger.</span></span>

  <span data-ttu-id="07beb-144">Klient wysyła pozdrowienia z usługą za pomocą komunikatu zawierającego jego nazwę "GreeterClient".</span><span class="sxs-lookup"><span data-stu-id="07beb-144">The client sends a greeting to the service with a message containing its name "GreeterClient".</span></span> <span data-ttu-id="07beb-145">Usługa wyśle komunikat "Hello GreeterClient" jako odpowiedzi, który jest wyświetlany w wierszu polecenia.</span><span class="sxs-lookup"><span data-stu-id="07beb-145">The service will send a message "Hello GreeterClient" as a response that is displayed in the command prompt.</span></span>

  ![Nowa aplikacja internetowa ASP.NET Core](grpc-start/_static/client.png)

  <span data-ttu-id="07beb-147">Usługa rejestruje szczegółowe informacje o pomyślnym wywołaniem w dziennikach, zapisywany wiersza polecenia.</span><span class="sxs-lookup"><span data-stu-id="07beb-147">The service records the details of the successful call in the logs written to the command prompt.</span></span>

  ![Nowa aplikacja internetowa ASP.NET Core](grpc-start/_static/server_complete.png)

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[<span data-ttu-id="07beb-149">Visual Studio Code / Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="07beb-149">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

* <span data-ttu-id="07beb-150">Uruchom projekt serwera GrpcGreeter.Server z wiersza polecenia przy użyciu `dotnet run`.</span><span class="sxs-lookup"><span data-stu-id="07beb-150">Run the Server project GrpcGreeter.Server from the command line using `dotnet run`.</span></span> <span data-ttu-id="07beb-151">Dzienniki pokazują, że usługa uruchomiono nasłuchiwanie protokołu `http://localhost:50051`.</span><span class="sxs-lookup"><span data-stu-id="07beb-151">The logs show that the service started listening on `http://localhost:50051`.</span></span>

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

* <span data-ttu-id="07beb-152">Uruchom projekt klienta, GrpcGreeter.Client w osobnym wierszu polecenia przy użyciu `dotnet run`.</span><span class="sxs-lookup"><span data-stu-id="07beb-152">Run the Client project GrpcGreeter.Client from the separate command line using `dotnet run`.</span></span>

<span data-ttu-id="07beb-153">Klient wysyła pozdrowienia z usługą za pomocą komunikatu zawierającego jego nazwę "GreeterClient".</span><span class="sxs-lookup"><span data-stu-id="07beb-153">The client sends a greeting to the service with a message containing its name "GreeterClient".</span></span> <span data-ttu-id="07beb-154">Usługa wyśle komunikat "Hello GreeterClient" jako odpowiedzi, który jest wyświetlany w wierszu polecenia.</span><span class="sxs-lookup"><span data-stu-id="07beb-154">The service will send a message "Hello GreeterClient" as a response that is displayed in the command prompt.</span></span>

```console
Greeting: Hello GreeterClient
Press any key to exit...
```

<span data-ttu-id="07beb-155">Usługa rejestruje szczegółowe informacje o pomyślnym wywołaniem w dziennikach, zapisywany wiersza polecenia.</span><span class="sxs-lookup"><span data-stu-id="07beb-155">The service records the details of the successful call in the logs written to the command prompt.</span></span>

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

### <a name="examine-the-project-files-of-the-grpc-project"></a><span data-ttu-id="07beb-156">Przejrzyj pliki projektu gRPC projektu</span><span class="sxs-lookup"><span data-stu-id="07beb-156">Examine the project files of the gRPC project</span></span>

<span data-ttu-id="07beb-157">GrpcGreeter.Server files:</span><span class="sxs-lookup"><span data-stu-id="07beb-157">GrpcGreeter.Server files:</span></span>

* <span data-ttu-id="07beb-158">greet.proto: *Protos/greet.proto* plik definiuje `Greeter` gRPC i służy do generowania gRPC zasoby serwera.</span><span class="sxs-lookup"><span data-stu-id="07beb-158">greet.proto: The *Protos/greet.proto* file defines the `Greeter` gRPC and is used to generate the gRPC server assets.</span></span> <span data-ttu-id="07beb-159">Aby uzyskać więcej informacji, zobacz <xref:grpc/index>.</span><span class="sxs-lookup"><span data-stu-id="07beb-159">For more information, see <xref:grpc/index>.</span></span>
* <span data-ttu-id="07beb-160">*Usługi* folderu: Zawiera implementację `Greeter` usługi.</span><span class="sxs-lookup"><span data-stu-id="07beb-160">*Services* folder: Contains the implementation of the `Greeter` service.</span></span>
* <span data-ttu-id="07beb-161">*appSettings.json*: zawierająca dane konfiguracyjne, takie jak protokół używany przez Kestrel.</span><span class="sxs-lookup"><span data-stu-id="07beb-161">*appSettings.json*:Contains configuration data, such as protocol used by Kestrel.</span></span> <span data-ttu-id="07beb-162">Aby uzyskać więcej informacji, zobacz <xref:fundamentals/configuration/index>.</span><span class="sxs-lookup"><span data-stu-id="07beb-162">For more information, see <xref:fundamentals/configuration/index>.</span></span>
* <span data-ttu-id="07beb-163">*Program.cs*: Zawiera punkt wejścia dla usługi gRPC.</span><span class="sxs-lookup"><span data-stu-id="07beb-163">*Program.cs*: Contains the entry point for the gRPC service.</span></span> <span data-ttu-id="07beb-164">Aby uzyskać więcej informacji, zobacz <xref:fundamentals/host/web-host>.</span><span class="sxs-lookup"><span data-stu-id="07beb-164">For more information, see <xref:fundamentals/host/web-host>.</span></span>
* <span data-ttu-id="07beb-165">Startup.cs</span><span class="sxs-lookup"><span data-stu-id="07beb-165">Startup.cs</span></span>

<span data-ttu-id="07beb-166">Zawiera kod, który konfiguruje zachowanie aplikacji.</span><span class="sxs-lookup"><span data-stu-id="07beb-166">Contains code that configures app behavior.</span></span> <span data-ttu-id="07beb-167">Aby uzyskać więcej informacji, zobacz <xref:fundamentals/startup>.</span><span class="sxs-lookup"><span data-stu-id="07beb-167">For more information, see <xref:fundamentals/startup>.</span></span>

<span data-ttu-id="07beb-168">gRPC client GrpcGreeter.Client file:</span><span class="sxs-lookup"><span data-stu-id="07beb-168">gRPC client GrpcGreeter.Client file:</span></span>

<span data-ttu-id="07beb-169">*Plik program.cs* zawiera punktu wejścia, a logika gRPC klienta.</span><span class="sxs-lookup"><span data-stu-id="07beb-169">*Program.cs* contains the entry point and logic for the gRPC client.</span></span>

<span data-ttu-id="07beb-170">W ramach tego samouczka możesz:</span><span class="sxs-lookup"><span data-stu-id="07beb-170">In this tutorial, you:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="07beb-171">Usługa gRPC utworzona.</span><span class="sxs-lookup"><span data-stu-id="07beb-171">Created a gRPC service.</span></span>
> * <span data-ttu-id="07beb-172">Uruchomiono usługę i klienta, aby przetestować usługę.</span><span class="sxs-lookup"><span data-stu-id="07beb-172">Ran the service and a client to test the service.</span></span>
> * <span data-ttu-id="07beb-173">Zbadane plików projektu.</span><span class="sxs-lookup"><span data-stu-id="07beb-173">Examined the project files.</span></span>
