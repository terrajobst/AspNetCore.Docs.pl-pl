---
title: Tworzenie klienta i serwera platformy .NET Core gRPC w ASP.NET Core
author: juntaoluo
description: W tym samouczku pokazano, jak utworzyć usługę gRPC i klienta gRPC na ASP.NET Core. Dowiedz się, jak utworzyć projekt usługi gRPC, edytować plik PROTO i dodać wywołanie przesyłania strumieniowego dupleksu.
monikerRange: '>= aspnetcore-3.0'
ms.author: johluo
ms.date: 08/07/2019
uid: tutorials/grpc/grpc-start
ms.openlocfilehash: 496f659bd51e2404a936bea8aad77e674e1a285d
ms.sourcegitcommit: 476ea5ad86a680b7b017c6f32098acd3414c0f6c
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/14/2019
ms.locfileid: "69022491"
---
# <a name="tutorial-create-a-grpc-client-and-server-in-aspnet-core"></a><span data-ttu-id="b7510-104">Samouczek: Utwórz klienta i serwer gRPC w ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="b7510-104">Tutorial: Create a gRPC client and server in ASP.NET Core</span></span>

<span data-ttu-id="b7510-105">Przez [Jan Luo](https://github.com/juntaoluo)</span><span class="sxs-lookup"><span data-stu-id="b7510-105">By [John Luo](https://github.com/juntaoluo)</span></span>

<span data-ttu-id="b7510-106">W tym samouczku pokazano, jak utworzyć klienta programu .NET Core [gRPC](https://grpc.io/docs/guides/) oraz serwer gRPC ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="b7510-106">This tutorial shows how to create a .NET Core [gRPC](https://grpc.io/docs/guides/) client and an ASP.NET Core gRPC Server.</span></span>

<span data-ttu-id="b7510-107">Na koniec będziesz mieć klienta gRPC, który komunikuje się z usługą gRPC Greeter.</span><span class="sxs-lookup"><span data-stu-id="b7510-107">At the end, you'll have a gRPC client that communicates with the gRPC Greeter service.</span></span>

<span data-ttu-id="b7510-108">[Wyświetlanie lub Pobieranie przykładowego kodu](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/tutorials/grpc/grpc-start/sample) ([jak pobrać](xref:index#how-to-download-a-sample)).</span><span class="sxs-lookup"><span data-stu-id="b7510-108">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/tutorials/grpc/grpc-start/sample) ([how to download](xref:index#how-to-download-a-sample)).</span></span>

<span data-ttu-id="b7510-109">W tym samouczku przedstawiono następujące instrukcje:</span><span class="sxs-lookup"><span data-stu-id="b7510-109">In this tutorial, you:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="b7510-110">Utwórz serwer gRPC.</span><span class="sxs-lookup"><span data-stu-id="b7510-110">Create a gRPC Server.</span></span>
> * <span data-ttu-id="b7510-111">Utwórz klienta gRPC.</span><span class="sxs-lookup"><span data-stu-id="b7510-111">Create a gRPC client.</span></span>
> * <span data-ttu-id="b7510-112">Przetestuj usługę klienta gRPC za pomocą usługi gRPC Greeter.</span><span class="sxs-lookup"><span data-stu-id="b7510-112">Test the gRPC client service with the gRPC Greeter service.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="b7510-113">Wymagania wstępne</span><span class="sxs-lookup"><span data-stu-id="b7510-113">Prerequisites</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="b7510-114">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="b7510-114">Visual Studio</span></span>](#tab/visual-studio)

[!INCLUDE[](~/includes/net-core-prereqs-vs-3.0.md)]

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="b7510-115">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="b7510-115">Visual Studio Code</span></span>](#tab/visual-studio-code)

[!INCLUDE[](~/includes/net-core-prereqs-vsc-3.0.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="b7510-116">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="b7510-116">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

[!INCLUDE[](~/includes/net-core-prereqs-mac-3.0.md)]

---

## <a name="create-a-grpc-service"></a><span data-ttu-id="b7510-117">Tworzenie usługi gRPC</span><span class="sxs-lookup"><span data-stu-id="b7510-117">Create a gRPC service</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="b7510-118">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="b7510-118">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="b7510-119">W programie Visual Studio **pliku** menu, wybierz opcję **New** > **projektu**.</span><span class="sxs-lookup"><span data-stu-id="b7510-119">From the Visual Studio **File** menu, select **New** > **Project**.</span></span>
* <span data-ttu-id="b7510-120">W oknie dialogowym **Tworzenie nowego projektu** wybierz pozycję **ASP.NET Core aplikacji sieci Web**.</span><span class="sxs-lookup"><span data-stu-id="b7510-120">In the **Create a new project** dialog, select **ASP.NET Core Web Application**.</span></span>
* <span data-ttu-id="b7510-121">Wybierz pozycję **dalej**</span><span class="sxs-lookup"><span data-stu-id="b7510-121">Select **Next**</span></span>
* <span data-ttu-id="b7510-122">Nazwij projekt **GrpcGreeter**.</span><span class="sxs-lookup"><span data-stu-id="b7510-122">Name the project **GrpcGreeter**.</span></span> <span data-ttu-id="b7510-123">Ważne jest, aby nazwa projektu *GrpcGreeter* , tak aby przestrzenie nazw były zgodne podczas kopiowania i wklejania kodu.</span><span class="sxs-lookup"><span data-stu-id="b7510-123">It's important to name the project *GrpcGreeter* so the namespaces will match when you copy and paste code.</span></span>
* <span data-ttu-id="b7510-124">Wybierz pozycję **Utwórz**</span><span class="sxs-lookup"><span data-stu-id="b7510-124">Select **Create**</span></span>
* <span data-ttu-id="b7510-125">W oknie dialogowym **Tworzenie nowej ASP.NET Core aplikacji sieci Web** :</span><span class="sxs-lookup"><span data-stu-id="b7510-125">In the **Create a new ASP.NET Core Web Application** dialog:</span></span>
  * <span data-ttu-id="b7510-126">W menu rozwijanym wybierz pozycję **.NET Core** i **ASP.NET Core 3,0** .</span><span class="sxs-lookup"><span data-stu-id="b7510-126">Select **.NET Core** and **ASP.NET Core 3.0** in the dropdown menus.</span></span> 
  * <span data-ttu-id="b7510-127">Wybierz szablon **usługi gRPC** .</span><span class="sxs-lookup"><span data-stu-id="b7510-127">Select the **gRPC Service** template.</span></span>
  * <span data-ttu-id="b7510-128">Wybierz pozycję **Utwórz**</span><span class="sxs-lookup"><span data-stu-id="b7510-128">Select **Create**</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="b7510-129">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="b7510-129">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="b7510-130">Otwórz [zintegrowany terminal](https://code.visualstudio.com/docs/editor/integrated-terminal).</span><span class="sxs-lookup"><span data-stu-id="b7510-130">Open the [integrated terminal](https://code.visualstudio.com/docs/editor/integrated-terminal).</span></span>
* <span data-ttu-id="b7510-131">Zmień katalogi (`cd`) na folder, który będzie zawierać projekt.</span><span class="sxs-lookup"><span data-stu-id="b7510-131">Change directories (`cd`) to a folder which will contain the project.</span></span>
* <span data-ttu-id="b7510-132">Uruchom następujące polecenia:</span><span class="sxs-lookup"><span data-stu-id="b7510-132">Run the following commands:</span></span>

  ```console
  dotnet new grpc -o GrpcGreeter
  code -r GrpcGreeter
  ```

  * <span data-ttu-id="b7510-133">Polecenie tworzy nową usługę gRPC w folderze GrpcGreeter. `dotnet new`</span><span class="sxs-lookup"><span data-stu-id="b7510-133">The `dotnet new` command creates a new gRPC service in the *GrpcGreeter* folder.</span></span>
  * <span data-ttu-id="b7510-134">Polecenie otwiera folder GrpcGreeter w nowym wystąpieniu Visual Studio Code. `code`</span><span class="sxs-lookup"><span data-stu-id="b7510-134">The `code` command opens the *GrpcGreeter* folder in a new instance of Visual Studio Code.</span></span>

  <span data-ttu-id="b7510-135">Zostanie wyświetlone okno dialogowe z **wymaganymi zasobami do kompilowania i debugowania brakuje w "GrpcGreeter". Dodać je?**</span><span class="sxs-lookup"><span data-stu-id="b7510-135">A dialog box appears with **Required assets to build and debug are missing from 'GrpcGreeter'. Add them?**</span></span>
* <span data-ttu-id="b7510-136">Wybierz opcję **tak**</span><span class="sxs-lookup"><span data-stu-id="b7510-136">Select **Yes**</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="b7510-137">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="b7510-137">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="b7510-138">W terminalu uruchom następujące polecenia:</span><span class="sxs-lookup"><span data-stu-id="b7510-138">From a terminal, run the following commands:</span></span>

```console
  dotnet new grpc -o GrpcGreeter
  cd GrpcGreeter
```

<span data-ttu-id="b7510-139">Poprzednie polecenia używają [interfejs wiersza polecenia platformy .NET Core](/dotnet/core/tools/dotnet) do tworzenia usługi gRPC.</span><span class="sxs-lookup"><span data-stu-id="b7510-139">The preceding commands use the [.NET Core CLI](/dotnet/core/tools/dotnet) to create a gRPC service.</span></span>

### <a name="open-the-project"></a><span data-ttu-id="b7510-140">Otwórz projekt</span><span class="sxs-lookup"><span data-stu-id="b7510-140">Open the project</span></span>

<span data-ttu-id="b7510-141">W programie Visual Studio wybierz pozycję **plik > Otwórz**, a następnie wybierz plik *GrpcGreeter. sln* .</span><span class="sxs-lookup"><span data-stu-id="b7510-141">From Visual Studio, select **File > Open**, and then select the *GrpcGreeter.sln* file.</span></span>

<!-- End of VS tabs -->

---

### <a name="run-the-service"></a><span data-ttu-id="b7510-142">Uruchamianie usługi</span><span class="sxs-lookup"><span data-stu-id="b7510-142">Run the service</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="b7510-143">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="b7510-143">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="b7510-144">Naciśnij `Ctrl+F5` klawisz, aby uruchomić usługę gRPC bez debugera.</span><span class="sxs-lookup"><span data-stu-id="b7510-144">Press `Ctrl+F5` to run the gRPC service without the debugger.</span></span>

  <span data-ttu-id="b7510-145">Program Visual Studio uruchamia usługę w wierszu polecenia.</span><span class="sxs-lookup"><span data-stu-id="b7510-145">Visual Studio runs the service in a command prompt.</span></span>

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[<span data-ttu-id="b7510-146">Visual Studio Code/Visual Studio dla komputerów Mac</span><span class="sxs-lookup"><span data-stu-id="b7510-146">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

* <span data-ttu-id="b7510-147">Uruchom gRPC Greeter projektu *GrpcGreeter* z wiersza polecenia przy użyciu `dotnet run`.</span><span class="sxs-lookup"><span data-stu-id="b7510-147">Run the gRPC Greeter project *GrpcGreeter* from the command line using `dotnet run`.</span></span>

<!-- End of combined VS/Mac tabs -->

---

<span data-ttu-id="b7510-148">Dzienniki pokazują, że usługa nasłuchuje `https://localhost:5001`.</span><span class="sxs-lookup"><span data-stu-id="b7510-148">The logs show the service listening on `https://localhost:5001`.</span></span>

```console
info: Microsoft.Hosting.Lifetime[0]
      Now listening on: https://localhost:5001
info: Microsoft.Hosting.Lifetime[0]
      Application started. Press Ctrl+C to shut down.
info: Microsoft.Hosting.Lifetime[0]
      Hosting environment: Development
```

> [!NOTE]
> <span data-ttu-id="b7510-149">Szablon gRPC jest skonfigurowany do korzystania z [Transport Layer Security (TLS)](https://tools.ietf.org/html/rfc5246).</span><span class="sxs-lookup"><span data-stu-id="b7510-149">The gRPC template is configured to use [Transport Layer Security (TLS)](https://tools.ietf.org/html/rfc5246).</span></span> <span data-ttu-id="b7510-150">Klienci gRPC muszą używać protokołu HTTPS, aby wywołać serwer.</span><span class="sxs-lookup"><span data-stu-id="b7510-150">gRPC clients need to use HTTPS to call the server.</span></span>
>
> <span data-ttu-id="b7510-151">Program macOS nie obsługuje ASP.NET Core gRPC z protokołem TLS.</span><span class="sxs-lookup"><span data-stu-id="b7510-151">macOS doesn't support ASP.NET Core gRPC with TLS.</span></span> <span data-ttu-id="b7510-152">Aby pomyślnie uruchomić usługi gRPC Services w usłudze macOS, wymagana jest dodatkowa konfiguracja.</span><span class="sxs-lookup"><span data-stu-id="b7510-152">Additional configuration is required to successfully run gRPC services on macOS.</span></span> <span data-ttu-id="b7510-153">Aby uzyskać więcej informacji, zobacz [nie można uruchomić aplikacji ASP.NET Core gRPC w witrynie macOS](xref:grpc/troubleshoot#unable-to-start-aspnet-core-grpc-app-on-macos).</span><span class="sxs-lookup"><span data-stu-id="b7510-153">For more information, see [Unable to start ASP.NET Core gRPC app on macOS](xref:grpc/troubleshoot#unable-to-start-aspnet-core-grpc-app-on-macos).</span></span>

### <a name="examine-the-project-files"></a><span data-ttu-id="b7510-154">Sprawdzanie plików projektu</span><span class="sxs-lookup"><span data-stu-id="b7510-154">Examine the project files</span></span>

<span data-ttu-id="b7510-155">*GrpcGreeter* pliki projektu:</span><span class="sxs-lookup"><span data-stu-id="b7510-155">*GrpcGreeter* project files:</span></span>

* <span data-ttu-id="b7510-156">*Greeting. proto*: Plik *Protos/Greeting. proto* definiuje `Greeter` gRPC i służy do generowania zasobów serwera gRPC.</span><span class="sxs-lookup"><span data-stu-id="b7510-156">*greet.proto*: The *Protos/greet.proto* file defines the `Greeter` gRPC and is used to generate the gRPC server assets.</span></span> <span data-ttu-id="b7510-157">Aby uzyskać więcej informacji, zobacz [wprowadzenie do gRPC](xref:grpc/index).</span><span class="sxs-lookup"><span data-stu-id="b7510-157">For more information, see [Introduction to gRPC](xref:grpc/index).</span></span>
* <span data-ttu-id="b7510-158">Folder *usługi* : Zawiera implementację `Greeter` usługi.</span><span class="sxs-lookup"><span data-stu-id="b7510-158">*Services* folder: Contains the implementation of the `Greeter` service.</span></span>
* <span data-ttu-id="b7510-159">*appSettings.json*: Zawiera dane konfiguracyjne, takie jak protokół używany przez Kestrel.</span><span class="sxs-lookup"><span data-stu-id="b7510-159">*appSettings.json*: Contains configuration data, such as protocol used by Kestrel.</span></span> <span data-ttu-id="b7510-160">Aby uzyskać więcej informacji, zobacz <xref:fundamentals/configuration/index>.</span><span class="sxs-lookup"><span data-stu-id="b7510-160">For more information, see <xref:fundamentals/configuration/index>.</span></span>
* <span data-ttu-id="b7510-161">*Program.cs*: Zawiera punkt wejścia dla usługi gRPC.</span><span class="sxs-lookup"><span data-stu-id="b7510-161">*Program.cs*: Contains the entry point for the gRPC service.</span></span> <span data-ttu-id="b7510-162">Aby uzyskać więcej informacji, zobacz <xref:fundamentals/host/generic-host>.</span><span class="sxs-lookup"><span data-stu-id="b7510-162">For more information, see <xref:fundamentals/host/generic-host>.</span></span>
* <span data-ttu-id="b7510-163">*Startup.cs*: Zawiera kod, który konfiguruje zachowanie aplikacji.</span><span class="sxs-lookup"><span data-stu-id="b7510-163">*Startup.cs*: Contains code that configures app behavior.</span></span> <span data-ttu-id="b7510-164">Aby uzyskać więcej informacji, zobacz [Uruchamianie aplikacji](xref:fundamentals/startup).</span><span class="sxs-lookup"><span data-stu-id="b7510-164">For more information, see [App startup](xref:fundamentals/startup).</span></span>

## <a name="create-the-grpc-client-in-a-net-console-app"></a><span data-ttu-id="b7510-165">Tworzenie klienta gRPC w aplikacji konsolowej .NET</span><span class="sxs-lookup"><span data-stu-id="b7510-165">Create the gRPC client in a .NET console app</span></span>

## <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="b7510-166">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="b7510-166">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="b7510-167">Otwórz drugie wystąpienie programu Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="b7510-167">Open a second instance of Visual Studio.</span></span>
* <span data-ttu-id="b7510-168">Na pasku menu wybierz pozycję **plik** > **Nowy** > **projekt** .</span><span class="sxs-lookup"><span data-stu-id="b7510-168">Select **File** > **New** > **Project** from the menu bar.</span></span>
* <span data-ttu-id="b7510-169">W oknie dialogowym **Tworzenie nowego projektu** wybierz pozycję **Aplikacja konsolowa (.NET Core)** .</span><span class="sxs-lookup"><span data-stu-id="b7510-169">In the **Create a new project** dialog, select **Console App (.NET Core)**.</span></span>
* <span data-ttu-id="b7510-170">Wybierz pozycję **dalej**</span><span class="sxs-lookup"><span data-stu-id="b7510-170">Select **Next**</span></span>
* <span data-ttu-id="b7510-171">W polu tekstowym **Nazwa** wprowadź wartość "GrpcGreeterClient".</span><span class="sxs-lookup"><span data-stu-id="b7510-171">In the **Name** text box, enter "GrpcGreeterClient".</span></span>
* <span data-ttu-id="b7510-172">Wybierz pozycję **Utwórz**.</span><span class="sxs-lookup"><span data-stu-id="b7510-172">Select **Create**.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="b7510-173">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="b7510-173">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="b7510-174">Otwórz [zintegrowany terminal](https://code.visualstudio.com/docs/editor/integrated-terminal).</span><span class="sxs-lookup"><span data-stu-id="b7510-174">Open the [integrated terminal](https://code.visualstudio.com/docs/editor/integrated-terminal).</span></span>
* <span data-ttu-id="b7510-175">Zmień katalogi (`cd`) na folder, który będzie zawierać projekt.</span><span class="sxs-lookup"><span data-stu-id="b7510-175">Change directories (`cd`) to a folder which will contain the project.</span></span>
* <span data-ttu-id="b7510-176">Uruchom następujące polecenia:</span><span class="sxs-lookup"><span data-stu-id="b7510-176">Run the following commands:</span></span>

```console
dotnet new console -o GrpcGreeterClient
code -r GrpcGreeterClient
```

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="b7510-177">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="b7510-177">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="b7510-178">Postępuj zgodnie z instrukcjami w [tym miejscu](/dotnet/core/tutorials/using-on-mac-vs-full-solution) , aby utworzyć aplikację konsolową o nazwie *GrpcGreeterClient*.</span><span class="sxs-lookup"><span data-stu-id="b7510-178">Follow the instructions [here](/dotnet/core/tutorials/using-on-mac-vs-full-solution) to create a console app with the name *GrpcGreeterClient*.</span></span>

<!-- End of VS tabs -->

---

### <a name="add-required-packages"></a><span data-ttu-id="b7510-179">Dodaj wymagane pakiety</span><span class="sxs-lookup"><span data-stu-id="b7510-179">Add required packages</span></span>

<span data-ttu-id="b7510-180">Projekt klienta gRPC wymaga następujących pakietów:</span><span class="sxs-lookup"><span data-stu-id="b7510-180">The gRPC client project requires the following packages:</span></span>

* <span data-ttu-id="b7510-181">[GRPC .NET. Client](https://www.nuget.org/packages/Grpc.Net.Client), który zawiera klienta .NET Core.</span><span class="sxs-lookup"><span data-stu-id="b7510-181">[Grpc.Net.Client](https://www.nuget.org/packages/Grpc.Net.Client), which contains the .NET Core client.</span></span>
* <span data-ttu-id="b7510-182">[Google. protobuf](https://www.nuget.org/packages/Google.Protobuf/), który zawiera interfejsy API komunikatów protobuf C#dla.</span><span class="sxs-lookup"><span data-stu-id="b7510-182">[Google.Protobuf](https://www.nuget.org/packages/Google.Protobuf/), which contains protobuf message APIs for C#.</span></span>
* <span data-ttu-id="b7510-183">[GRPC. Tools](https://www.nuget.org/packages/Grpc.Tools/), która zawiera C# obsługę narzędzi dla plików protobuf.</span><span class="sxs-lookup"><span data-stu-id="b7510-183">[Grpc.Tools](https://www.nuget.org/packages/Grpc.Tools/), which contains C# tooling support for protobuf files.</span></span> <span data-ttu-id="b7510-184">Pakiet narzędzi nie jest wymagany w czasie wykonywania, dlatego zależność jest oznaczona za pomocą `PrivateAssets="All"`.</span><span class="sxs-lookup"><span data-stu-id="b7510-184">The tooling package isn't required at runtime, so the dependency is marked with `PrivateAssets="All"`.</span></span>

### <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="b7510-185">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="b7510-185">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="b7510-186">Zainstaluj pakiety przy użyciu konsoli Menedżera pakietów (PMC) lub Zarządzaj pakietami NuGet.</span><span class="sxs-lookup"><span data-stu-id="b7510-186">Install the packages using either the Package Manager Console (PMC) or Manage NuGet Packages.</span></span>

#### <a name="pmc-option-to-install-packages"></a><span data-ttu-id="b7510-187">Opcja PMC, aby zainstalować pakiety</span><span class="sxs-lookup"><span data-stu-id="b7510-187">PMC option to install packages</span></span>

* <span data-ttu-id="b7510-188">W programie Visual Studio wybierz kolejno pozycje **Narzędzia** > Menedżer**pakietów** > NuGet**konsola Menedżera pakietów**</span><span class="sxs-lookup"><span data-stu-id="b7510-188">From Visual Studio, select **Tools** > **NuGet Package Manager** > **Package Manager Console**</span></span>
* <span data-ttu-id="b7510-189">W oknie **konsola Menedżera pakietów** przejdź do katalogu, w którym znajduje się plik *GrpcGreeterClient. csproj* .</span><span class="sxs-lookup"><span data-stu-id="b7510-189">From the **Package Manager Console** window, navigate to the directory in which the *GrpcGreeterClient.csproj* file exists.</span></span>
* <span data-ttu-id="b7510-190">Uruchom następujące polecenia:</span><span class="sxs-lookup"><span data-stu-id="b7510-190">Run the following commands:</span></span>

 ```powershell
Install-Package Grpc.Net.Client
Install-Package Google.Protobuf
Install-Package Grpc.Tools
```

#### <a name="manage-nuget-packages-option-to-install-packages"></a><span data-ttu-id="b7510-191">Opcja zarządzania pakietami NuGet w celu zainstalowania pakietów</span><span class="sxs-lookup"><span data-stu-id="b7510-191">Manage NuGet Packages option to install packages</span></span>

* <span data-ttu-id="b7510-192">Kliknij prawym przyciskiem myszy projekt w **Eksplorator rozwiązań** > **Zarządzanie pakietami NuGet**</span><span class="sxs-lookup"><span data-stu-id="b7510-192">Right-click the project in **Solution Explorer** > **Manage NuGet Packages**</span></span>
* <span data-ttu-id="b7510-193">Wybierz kartę **przeglądanie** .</span><span class="sxs-lookup"><span data-stu-id="b7510-193">Select the **Browse** tab.</span></span>
* <span data-ttu-id="b7510-194">W polu wyszukiwania wprowadź **GRPC .NET. Client** .</span><span class="sxs-lookup"><span data-stu-id="b7510-194">Enter **Grpc.Net.Client** in the search box.</span></span>
* <span data-ttu-id="b7510-195">Wybierz pakiet **GRPC .NET. Client** z karty **Przeglądaj** i wybierz pozycję **Zainstaluj**.</span><span class="sxs-lookup"><span data-stu-id="b7510-195">Select the **Grpc.Net.Client** package from the **Browse** tab and select **Install**.</span></span>
* <span data-ttu-id="b7510-196">Powtórz dla `Google.Protobuf` i `Grpc.Tools`.</span><span class="sxs-lookup"><span data-stu-id="b7510-196">Repeat for `Google.Protobuf` and `Grpc.Tools`.</span></span>

### <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="b7510-197">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="b7510-197">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="b7510-198">Uruchom następujące polecenia w zintegrowanym **terminalu**:</span><span class="sxs-lookup"><span data-stu-id="b7510-198">Run the following commands from the **Integrated Terminal**:</span></span>

```console
dotnet add GrpcGreeterClient.csproj package Grpc.Net.Client
dotnet add GrpcGreeterClient.csproj package Google.Protobuf
dotnet add GrpcGreeterClient.csproj package Grpc.Tools
```

### <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="b7510-199">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="b7510-199">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="b7510-200">Kliknij prawym przyciskiem myszy folder **pakiety** w **okienko rozwiązania** > **Dodaj pakiety**</span><span class="sxs-lookup"><span data-stu-id="b7510-200">Right-click the **Packages** folder in **Solution Pad** > **Add Packages**</span></span>
* <span data-ttu-id="b7510-201">W polu wyszukiwania wprowadź **GRPC .NET. Client** .</span><span class="sxs-lookup"><span data-stu-id="b7510-201">Enter **Grpc.Net.Client** in the search box.</span></span>
* <span data-ttu-id="b7510-202">Wybierz pakiet **GRPC .NET. Client** z okienka wyników, a następnie wybierz pozycję **Dodaj pakiet** .</span><span class="sxs-lookup"><span data-stu-id="b7510-202">Select the **Grpc.Net.Client** package from the results pane and select **Add Package**</span></span>
* <span data-ttu-id="b7510-203">Powtórz dla `Google.Protobuf` i `Grpc.Tools`.</span><span class="sxs-lookup"><span data-stu-id="b7510-203">Repeat for `Google.Protobuf` and `Grpc.Tools`.</span></span>

---

### <a name="add-greetproto"></a><span data-ttu-id="b7510-204">Dodaj Greeting. proto</span><span class="sxs-lookup"><span data-stu-id="b7510-204">Add greet.proto</span></span>

* <span data-ttu-id="b7510-205">Utwórz folder **Protos** w projekcie klienta gRPC.</span><span class="sxs-lookup"><span data-stu-id="b7510-205">Create a **Protos** folder in the gRPC client project.</span></span>
* <span data-ttu-id="b7510-206">Skopiuj plik **Protos\greet.proto** z usługi gRPC Greeter do projektu klienta gRPC.</span><span class="sxs-lookup"><span data-stu-id="b7510-206">Copy the **Protos\greet.proto** file from the gRPC Greeter service to the gRPC client project.</span></span>
* <span data-ttu-id="b7510-207">Edytuj plik projektu *GrpcGreeterClient. csproj* :</span><span class="sxs-lookup"><span data-stu-id="b7510-207">Edit the *GrpcGreeterClient.csproj* project file:</span></span>

  # <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="b7510-208">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="b7510-208">Visual Studio</span></span>](#tab/visual-studio) 

  <span data-ttu-id="b7510-209">Kliknij prawym przyciskiem myszy projekt i wybierz polecenie **Edytuj plik projektu**.</span><span class="sxs-lookup"><span data-stu-id="b7510-209">Right-click the project and select **Edit Project File**.</span></span>

  # <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="b7510-210">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="b7510-210">Visual Studio Code</span></span>](#tab/visual-studio-code) 

  <span data-ttu-id="b7510-211">Wybierz plik *GrpcGreeterClient. csproj* .</span><span class="sxs-lookup"><span data-stu-id="b7510-211">Select the *GrpcGreeterClient.csproj* file.</span></span>

  # <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="b7510-212">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="b7510-212">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

  <span data-ttu-id="b7510-213">Kliknij prawym przyciskiem myszy projekt, a następnie wybierz polecenie **narzędzia > Edytuj plik**.</span><span class="sxs-lookup"><span data-stu-id="b7510-213">Right-click the project and select **Tools > Edit File**.</span></span>

  ---

* <span data-ttu-id="b7510-214">Dodaj grupę elementów z `<Protobuf>` elementem, który odwołuje się do pliku **Greeting. proto** :</span><span class="sxs-lookup"><span data-stu-id="b7510-214">Add an item group with a `<Protobuf>` element that refers to the **greet.proto** file:</span></span>

  ```XML
  <ItemGroup>
    <Protobuf Include="Protos\greet.proto" GrpcServices="Client" />
  </ItemGroup>
  ```

### <a name="create-the-greeter-client"></a><span data-ttu-id="b7510-215">Tworzenie klienta Greeter</span><span class="sxs-lookup"><span data-stu-id="b7510-215">Create the Greeter client</span></span>

<span data-ttu-id="b7510-216">Skompiluj projekt, aby utworzyć typy w `GrpcGreeter` przestrzeni nazw.</span><span class="sxs-lookup"><span data-stu-id="b7510-216">Build the project to create the types in the `GrpcGreeter` namespace.</span></span> <span data-ttu-id="b7510-217">`GrpcGreeter` Typy są generowane automatycznie przez proces kompilacji.</span><span class="sxs-lookup"><span data-stu-id="b7510-217">The `GrpcGreeter` types are generated automatically by the build process.</span></span>

<span data-ttu-id="b7510-218">Zaktualizuj plik *program.cs* klienta gRPC za pomocą następującego kodu:</span><span class="sxs-lookup"><span data-stu-id="b7510-218">Update the gRPC client *Program.cs* file with the following code:</span></span>

[!code-cs[](~/tutorials/grpc/grpc-start/sample/GrpcGreeterClient/Program.cs?name=snippet2)]

<span data-ttu-id="b7510-219">*Program.cs* zawiera punkt wejścia i logikę dla klienta gRPC.</span><span class="sxs-lookup"><span data-stu-id="b7510-219">*Program.cs* contains the entry point and logic for the gRPC client.</span></span>

<span data-ttu-id="b7510-220">Klient Greeter jest tworzony przez:</span><span class="sxs-lookup"><span data-stu-id="b7510-220">The Greeter client is created by:</span></span>

* <span data-ttu-id="b7510-221">Utworzenie wystąpienia zawierającego informacje dotyczące tworzenia połączenia z usługą gRPC. `HttpClient`</span><span class="sxs-lookup"><span data-stu-id="b7510-221">Instantiating an `HttpClient` containing the information for creating the connection to the gRPC service.</span></span>
* <span data-ttu-id="b7510-222">Korzystanie z programu do konstruowania klienta Greeter: `HttpClient`</span><span class="sxs-lookup"><span data-stu-id="b7510-222">Using the `HttpClient` to construct the Greeter client:</span></span>

[!code-cs[](~/tutorials/grpc/grpc-start/sample/GrpcGreeterClient/Program.cs?name=snippet&highlight=3-6)]

<span data-ttu-id="b7510-223">Klient Greeter wywołuje metodę asynchroniczną `SayHello` .</span><span class="sxs-lookup"><span data-stu-id="b7510-223">The Greeter client calls the asynchronous `SayHello` method.</span></span> <span data-ttu-id="b7510-224">Zostanie wyświetlony wynik `SayHello` wywołania:</span><span class="sxs-lookup"><span data-stu-id="b7510-224">The result of the `SayHello` call is displayed:</span></span>

[!code-cs[](~/tutorials/grpc/grpc-start/sample/GrpcGreeterClient/Program.cs?name=snippet&highlight=7-9)]

## <a name="test-the-grpc-client-with-the-grpc-greeter-service"></a><span data-ttu-id="b7510-225">Testowanie klienta gRPC za pomocą usługi gRPC Greeter</span><span class="sxs-lookup"><span data-stu-id="b7510-225">Test the gRPC client with the gRPC Greeter service</span></span>

### <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="b7510-226">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="b7510-226">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="b7510-227">W usłudze Greeter naciśnij klawisz `Ctrl+F5` , aby uruchomić serwer bez debugera.</span><span class="sxs-lookup"><span data-stu-id="b7510-227">In the Greeter service, press `Ctrl+F5` to start the server without the debugger.</span></span>
* <span data-ttu-id="b7510-228">W projekcie naciśnij klawisz `Ctrl+F5` , aby uruchomić klienta bez debugera. `GrpcGreeterClient`</span><span class="sxs-lookup"><span data-stu-id="b7510-228">In the `GrpcGreeterClient` project, press `Ctrl+F5` to start the client without the debugger.</span></span>

### <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[<span data-ttu-id="b7510-229">Visual Studio Code/Visual Studio dla komputerów Mac</span><span class="sxs-lookup"><span data-stu-id="b7510-229">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

* <span data-ttu-id="b7510-230">Uruchom usługę Greeter.</span><span class="sxs-lookup"><span data-stu-id="b7510-230">Start the Greeter service.</span></span>
* <span data-ttu-id="b7510-231">Uruchom klienta programu.</span><span class="sxs-lookup"><span data-stu-id="b7510-231">Start the client.</span></span>

<!-- End of combined VS/Mac tabs -->

---

<span data-ttu-id="b7510-232">Klient wysyła powitanie do usługi z komunikatem zawierającym nazwę "GreeterClient".</span><span class="sxs-lookup"><span data-stu-id="b7510-232">The client sends a greeting to the service with a message containing its name "GreeterClient".</span></span> <span data-ttu-id="b7510-233">Usługa wysyła komunikat "Hello GreeterClient" jako odpowiedź.</span><span class="sxs-lookup"><span data-stu-id="b7510-233">The service sends the message "Hello GreeterClient" as a response.</span></span> <span data-ttu-id="b7510-234">Odpowiedź "Hello GreeterClient" jest wyświetlana w wierszu polecenia:</span><span class="sxs-lookup"><span data-stu-id="b7510-234">The "Hello GreeterClient" response is displayed in the command prompt:</span></span>

```console
Greeting: Hello GreeterClient
Press any key to exit...
```

<span data-ttu-id="b7510-235">Usługa gRPC rejestruje szczegóły pomyślnego wywołania w dziennikach zapisanych w wierszu polecenia.</span><span class="sxs-lookup"><span data-stu-id="b7510-235">The gRPC service records the details of the successful call in the logs written to the command prompt.</span></span>

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

### <a name="next-steps"></a><span data-ttu-id="b7510-236">Następne kroki</span><span class="sxs-lookup"><span data-stu-id="b7510-236">Next steps</span></span>

* <xref:grpc/index>
* <xref:grpc/basics>
* <xref:grpc/migration>
