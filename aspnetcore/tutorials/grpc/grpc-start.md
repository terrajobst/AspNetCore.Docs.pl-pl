---
title: 'Samouczek: Rozpoczynanie pracy z usługą gRPC w programie ASP.NET Core'
author: juntaoluo
description: Tej serii samouczków pokazano, jak utworzyć gRPC usługi programu ASP.NET Core. Dowiedz się, jak utworzyć projekt usługi gRPC, Edytuj plik proto i dodać dupleks przesyłania strumieniowego wywołania.
monikerRange: '>= aspnetcore-3.0'
ms.author: johluo
ms.date: 2/26/2019
uid: tutorials/grpc/grpc-start
ms.openlocfilehash: a67050331cc8563b1baf5312b92910c1bbdfbd69
ms.sourcegitcommit: 57a974556acd09363a58f38c26f74dc21e0d4339
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/17/2019
ms.locfileid: "59672570"
---
# <a name="tutorial-get-started-with-grpc-service-in-aspnet-core"></a><span data-ttu-id="dafcb-104">Samouczek: Wprowadzenie do usługi gRPC na platformie ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="dafcb-104">Tutorial: Get started with gRPC service in ASP.NET Core</span></span>

<span data-ttu-id="dafcb-105">Przez [Luo Jan](https://github.com/juntaoluo)</span><span class="sxs-lookup"><span data-stu-id="dafcb-105">By [John Luo](https://github.com/juntaoluo)</span></span>

<span data-ttu-id="dafcb-106">W tym samouczku pokazano podstawy tworzenia usługi gRPC programu ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="dafcb-106">This tutorial teaches the basics of building a gRPC service on ASP.NET Core.</span></span>

<span data-ttu-id="dafcb-107">Po zakończeniu będziesz mieć usługa gRPC, która zwraca greetings.</span><span class="sxs-lookup"><span data-stu-id="dafcb-107">At the end, you'll have a gRPC service that echoes greetings.</span></span>

[!INCLUDE[View or download sample code](~/includes/grpc/download.md)]

<span data-ttu-id="dafcb-108">W ramach tego samouczka możesz:</span><span class="sxs-lookup"><span data-stu-id="dafcb-108">In this tutorial, you:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="dafcb-109">Utwórz usługę gRPC.</span><span class="sxs-lookup"><span data-stu-id="dafcb-109">Create a gRPC service.</span></span>
> * <span data-ttu-id="dafcb-110">Uruchom usługę gRPC.</span><span class="sxs-lookup"><span data-stu-id="dafcb-110">Run the gRPC service.</span></span>
> * <span data-ttu-id="dafcb-111">Sprawdź pliki projektu.</span><span class="sxs-lookup"><span data-stu-id="dafcb-111">Examine the project files.</span></span>

[!INCLUDE[](~/includes/net-core-prereqs-all-3.0.md)]

## <a name="create-a-grpc-service"></a><span data-ttu-id="dafcb-112">Tworzenie usługi gRPC</span><span class="sxs-lookup"><span data-stu-id="dafcb-112">Create a gRPC service</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="dafcb-113">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="dafcb-113">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="dafcb-114">W programie Visual Studio **pliku** menu, wybierz opcję **New** > **projektu**.</span><span class="sxs-lookup"><span data-stu-id="dafcb-114">From the Visual Studio **File** menu, select **New** > **Project**.</span></span>
* <span data-ttu-id="dafcb-115">Tworzenie nowej aplikacji sieci Web platformy ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="dafcb-115">Create a new ASP.NET Core Web Application.</span></span>
  <span data-ttu-id="dafcb-116">![Nowa aplikacja internetowa ASP.NET Core](grpc-start/_static/np_3_0.1.png)</span><span class="sxs-lookup"><span data-stu-id="dafcb-116">![new ASP.NET Core Web Application](grpc-start/_static/np_3_0.1.png)</span></span>
* <span data-ttu-id="dafcb-117">Nadaj projektowi nazwę **GrpcGreeter**.</span><span class="sxs-lookup"><span data-stu-id="dafcb-117">Name the project **GrpcGreeter**.</span></span> <span data-ttu-id="dafcb-118">Ważne jest, aby nadaj projektowi nazwę *GrpcGreeter* , przestrzenie nazw będą zgodne po skopiuj i Wklej kod.</span><span class="sxs-lookup"><span data-stu-id="dafcb-118">It's important to name the project *GrpcGreeter* so the namespaces will match when you copy and paste code.</span></span>
  <span data-ttu-id="dafcb-119">![Nowa aplikacja internetowa ASP.NET Core](grpc-start/_static/np_3_0.2.png)</span><span class="sxs-lookup"><span data-stu-id="dafcb-119">![new ASP.NET Core Web Application](grpc-start/_static/np_3_0.2.png)</span></span>
* <span data-ttu-id="dafcb-120">Wybierz **platformy .NET Core** i **platformy ASP.NET Core 3.0** na liście rozwijanej.</span><span class="sxs-lookup"><span data-stu-id="dafcb-120">Select **.NET Core** and **ASP.NET Core 3.0** in the dropdown.</span></span> <span data-ttu-id="dafcb-121">Wybierz **gRPC usługi** szablonu.</span><span class="sxs-lookup"><span data-stu-id="dafcb-121">Choose the **gRPC Service** template.</span></span>

  <span data-ttu-id="dafcb-122">Utworzono następujący projekt startowy:</span><span class="sxs-lookup"><span data-stu-id="dafcb-122">The following starter project is created:</span></span>

  ![Eksplorator rozwiązań](grpc-start/_static/se3.0.png)

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="dafcb-124">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="dafcb-124">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="dafcb-125">Otwórz [zintegrowany terminal](https://code.visualstudio.com/docs/editor/integrated-terminal).</span><span class="sxs-lookup"><span data-stu-id="dafcb-125">Open the [integrated terminal](https://code.visualstudio.com/docs/editor/integrated-terminal).</span></span>
* <span data-ttu-id="dafcb-126">Zmień katalog (`cd`) do folderu, który będzie zawierać projekt.</span><span class="sxs-lookup"><span data-stu-id="dafcb-126">Change directories (`cd`) to a folder which will contain the project.</span></span>
* <span data-ttu-id="dafcb-127">Uruchom następujące polecenia:</span><span class="sxs-lookup"><span data-stu-id="dafcb-127">Run the following commands:</span></span>

  ```console
  dotnet new grpc -o GrpcGreeter
  code -r GrpcGreeter
  ```

  * <span data-ttu-id="dafcb-128">`dotnet new` Polecenie tworzy nową usługę gRPC w *GrpcGreeter* folderu.</span><span class="sxs-lookup"><span data-stu-id="dafcb-128">The `dotnet new` command creates a new gRPC service in the *GrpcGreeter* folder.</span></span>
  * <span data-ttu-id="dafcb-129">`code` Polecenia otwiera *GrpcGreeter* folderu w nowym wystąpieniu programu Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="dafcb-129">The `code` command opens the *GrpcGreeter* folder in a new instance of Visual Studio Code.</span></span>

  <span data-ttu-id="dafcb-130">Zostanie wyświetlone okno dialogowe z **"GrpcGreeter" brakuje wymagane zasoby do tworzenia i debugowania. Dodaj je?**</span><span class="sxs-lookup"><span data-stu-id="dafcb-130">A dialog box appears with **Required assets to build and debug are missing from 'GrpcGreeter'. Add them?**</span></span>
* <span data-ttu-id="dafcb-131">Wybierz **tak**</span><span class="sxs-lookup"><span data-stu-id="dafcb-131">Select **Yes**</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="dafcb-132">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="dafcb-132">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="dafcb-133">W terminalu uruchom następujące polecenia:</span><span class="sxs-lookup"><span data-stu-id="dafcb-133">From a terminal, run the following commands:</span></span>

```console
  dotnet new grpc -o GrpcGreeter
  cd GrpcGreeter
```

<span data-ttu-id="dafcb-134">Poprzednie polecenia użyj [interfejsu wiersza polecenia platformy .NET Core](/dotnet/core/tools/dotnet) utworzyć usługę gRPC.</span><span class="sxs-lookup"><span data-stu-id="dafcb-134">The preceding commands use the [.NET Core CLI](/dotnet/core/tools/dotnet) to create a gRPC service.</span></span>

### <a name="open-the-project"></a><span data-ttu-id="dafcb-135">Otwórz projekt</span><span class="sxs-lookup"><span data-stu-id="dafcb-135">Open the project</span></span>

<span data-ttu-id="dafcb-136">Z programu Visual Studio, wybierz **Plik > Otwórz**, a następnie wybierz pozycję *GrpcGreeter.sln* pliku.</span><span class="sxs-lookup"><span data-stu-id="dafcb-136">From Visual Studio, select **File > Open**, and then select the *GrpcGreeter.sln* file.</span></span>

<!-- End of VS tabs -->

---

### <a name="run-the-service"></a><span data-ttu-id="dafcb-137">Uruchom usługę</span><span class="sxs-lookup"><span data-stu-id="dafcb-137">Run the service</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="dafcb-138">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="dafcb-138">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="dafcb-139">Naciśnij klawisze Ctrl + F5, aby uruchomić usługę gRPC bez debugera.</span><span class="sxs-lookup"><span data-stu-id="dafcb-139">Press Ctrl+F5 to run the gRPC service without the debugger.</span></span>

  <span data-ttu-id="dafcb-140">Visual Studio uruchamia usługę w wierszu polecenia.</span><span class="sxs-lookup"><span data-stu-id="dafcb-140">Visual Studio runs the service in a command prompt.</span></span> <span data-ttu-id="dafcb-141">Dzienniki pokazują, że usługa uruchomiono nasłuchiwanie protokołu `http://localhost:50051`.</span><span class="sxs-lookup"><span data-stu-id="dafcb-141">The logs show that the service started listening on `http://localhost:50051`.</span></span>

  ![Nowa aplikacja internetowa ASP.NET Core](grpc-start/_static/server_start.png)

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[<span data-ttu-id="dafcb-143">Visual Studio Code / Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="dafcb-143">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

* <span data-ttu-id="dafcb-144">Uruchom projekt Greeter gRPC GrpcGreeter z wiersza polecenia przy użyciu `dotnet run`.</span><span class="sxs-lookup"><span data-stu-id="dafcb-144">Run the gRPC Greeter project GrpcGreeter from the command line using `dotnet run`.</span></span> <span data-ttu-id="dafcb-145">Dzienniki pokazują, że usługa uruchomiono nasłuchiwanie protokołu `http://localhost:50051`.</span><span class="sxs-lookup"><span data-stu-id="dafcb-145">The logs show that the service started listening on `http://localhost:50051`.</span></span>

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
```

<!-- End of combined VS/Mac tabs -->

---

<span data-ttu-id="dafcb-146">Następnego samouczka pokazują, jak tworzyć gRPC klienta, który jest wymagany do testowania usługi Greeter.</span><span class="sxs-lookup"><span data-stu-id="dafcb-146">The next tutorial will demonstrate how to build a gRPC client, which is required to test the Greeter service.</span></span>

### <a name="examine-the-project-files-of-the-grpc-project"></a><span data-ttu-id="dafcb-147">Przejrzyj pliki projektu gRPC projektu</span><span class="sxs-lookup"><span data-stu-id="dafcb-147">Examine the project files of the gRPC project</span></span>

<span data-ttu-id="dafcb-148">Pliki GrpcGreeter:</span><span class="sxs-lookup"><span data-stu-id="dafcb-148">GrpcGreeter files:</span></span>

* <span data-ttu-id="dafcb-149">greet.proto: *Protos/greet.proto* plik definiuje `Greeter` gRPC i służy do generowania gRPC zasoby serwera.</span><span class="sxs-lookup"><span data-stu-id="dafcb-149">greet.proto: The *Protos/greet.proto* file defines the `Greeter` gRPC and is used to generate the gRPC server assets.</span></span> <span data-ttu-id="dafcb-150">Aby uzyskać więcej informacji, zobacz <xref:grpc/index>.</span><span class="sxs-lookup"><span data-stu-id="dafcb-150">For more information, see <xref:grpc/index>.</span></span>
* <span data-ttu-id="dafcb-151">*Usługi* folderu: Zawiera implementację `Greeter` usługi.</span><span class="sxs-lookup"><span data-stu-id="dafcb-151">*Services* folder: Contains the implementation of the `Greeter` service.</span></span>
* <span data-ttu-id="dafcb-152">*appSettings.json*: zawierająca dane konfiguracyjne, takie jak protokół używany przez Kestrel.</span><span class="sxs-lookup"><span data-stu-id="dafcb-152">*appSettings.json*:Contains configuration data, such as protocol used by Kestrel.</span></span> <span data-ttu-id="dafcb-153">Aby uzyskać więcej informacji, zobacz <xref:fundamentals/configuration/index>.</span><span class="sxs-lookup"><span data-stu-id="dafcb-153">For more information, see <xref:fundamentals/configuration/index>.</span></span>
* <span data-ttu-id="dafcb-154">*Program.cs*: Zawiera punkt wejścia dla usługi gRPC.</span><span class="sxs-lookup"><span data-stu-id="dafcb-154">*Program.cs*: Contains the entry point for the gRPC service.</span></span> <span data-ttu-id="dafcb-155">Aby uzyskać więcej informacji, zobacz <xref:fundamentals/host/web-host>.</span><span class="sxs-lookup"><span data-stu-id="dafcb-155">For more information, see <xref:fundamentals/host/web-host>.</span></span>
* <span data-ttu-id="dafcb-156">Startup.cs: Zawiera kod, który konfiguruje zachowanie aplikacji.</span><span class="sxs-lookup"><span data-stu-id="dafcb-156">Startup.cs: Contains code that configures app behavior.</span></span> <span data-ttu-id="dafcb-157">Aby uzyskać więcej informacji, zobacz <xref:fundamentals/startup>.</span><span class="sxs-lookup"><span data-stu-id="dafcb-157">For more information, see <xref:fundamentals/startup>.</span></span>

### <a name="test-the-service"></a><span data-ttu-id="dafcb-158">Testowanie usługi</span><span class="sxs-lookup"><span data-stu-id="dafcb-158">Test the service</span></span>

## <a name="additional-resources"></a><span data-ttu-id="dafcb-159">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="dafcb-159">Additional resources</span></span>

<span data-ttu-id="dafcb-160">W ramach tego samouczka możesz:</span><span class="sxs-lookup"><span data-stu-id="dafcb-160">In this tutorial, you:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="dafcb-161">Usługa gRPC utworzona.</span><span class="sxs-lookup"><span data-stu-id="dafcb-161">Created a gRPC service.</span></span>
> * <span data-ttu-id="dafcb-162">Uruchomiono usługę gRPC.</span><span class="sxs-lookup"><span data-stu-id="dafcb-162">Ran the gRPC service.</span></span>
> * <span data-ttu-id="dafcb-163">Zbadane plików projektu.</span><span class="sxs-lookup"><span data-stu-id="dafcb-163">Examined the project files.</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="dafcb-164">Dalej: Tworzenie klienta gRPC platformy .NET Core</span><span class="sxs-lookup"><span data-stu-id="dafcb-164">Next: Create a .NET Core gRPC client</span></span>](xref:tutorials/grpc/grpc-client)