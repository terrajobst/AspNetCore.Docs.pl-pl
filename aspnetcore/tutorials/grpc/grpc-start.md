---
title: Tworzenie klienta i serwera platformy .NET Core gRPC w ASP.NET Core
author: juntaoluo
description: W tym samouczku pokazano, jak utworzyć usługę gRPC i klienta gRPC na ASP.NET Core. Dowiedz się, jak utworzyć projekt usługi gRPC, edytować plik PROTO i dodać wywołanie przesyłania strumieniowego dupleksu.
monikerRange: '>= aspnetcore-3.0'
ms.author: johluo
ms.date: 8/26/2019
uid: tutorials/grpc/grpc-start
ms.openlocfilehash: ffe0034f1d33fb9282b39c030421b9b307413cdb
ms.sourcegitcommit: de17150e5ec7507d7114dde0e5dbc2e45a66ef53
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/28/2019
ms.locfileid: "70113279"
---
# <a name="tutorial-create-a-grpc-client-and-server-in-aspnet-core"></a><span data-ttu-id="639b8-104">Samouczek: Utwórz klienta i serwer gRPC w ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="639b8-104">Tutorial: Create a gRPC client and server in ASP.NET Core</span></span>

<span data-ttu-id="639b8-105">Przez [Jan Luo](https://github.com/juntaoluo)</span><span class="sxs-lookup"><span data-stu-id="639b8-105">By [John Luo](https://github.com/juntaoluo)</span></span>

<span data-ttu-id="639b8-106">W tym samouczku pokazano, jak utworzyć klienta programu .NET Core [gRPC](https://grpc.io/docs/guides/) oraz serwer gRPC ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="639b8-106">This tutorial shows how to create a .NET Core [gRPC](https://grpc.io/docs/guides/) client and an ASP.NET Core gRPC Server.</span></span>

<span data-ttu-id="639b8-107">Na koniec będziesz mieć klienta gRPC, który komunikuje się z usługą gRPC Greeter.</span><span class="sxs-lookup"><span data-stu-id="639b8-107">At the end, you'll have a gRPC client that communicates with the gRPC Greeter service.</span></span>

<span data-ttu-id="639b8-108">[Wyświetlanie lub Pobieranie przykładowego kodu](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/tutorials/grpc/grpc-start/sample) ([jak pobrać](xref:index#how-to-download-a-sample)).</span><span class="sxs-lookup"><span data-stu-id="639b8-108">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/tutorials/grpc/grpc-start/sample) ([how to download](xref:index#how-to-download-a-sample)).</span></span>

<span data-ttu-id="639b8-109">W tym samouczku przedstawiono następujące instrukcje:</span><span class="sxs-lookup"><span data-stu-id="639b8-109">In this tutorial, you:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="639b8-110">Utwórz serwer gRPC.</span><span class="sxs-lookup"><span data-stu-id="639b8-110">Create a gRPC Server.</span></span>
> * <span data-ttu-id="639b8-111">Utwórz klienta gRPC.</span><span class="sxs-lookup"><span data-stu-id="639b8-111">Create a gRPC client.</span></span>
> * <span data-ttu-id="639b8-112">Przetestuj usługę klienta gRPC za pomocą usługi gRPC Greeter.</span><span class="sxs-lookup"><span data-stu-id="639b8-112">Test the gRPC client service with the gRPC Greeter service.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="639b8-113">Wymagania wstępne</span><span class="sxs-lookup"><span data-stu-id="639b8-113">Prerequisites</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="639b8-114">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="639b8-114">Visual Studio</span></span>](#tab/visual-studio)

[!INCLUDE[](~/includes/net-core-prereqs-vs-3.0.md)]

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="639b8-115">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="639b8-115">Visual Studio Code</span></span>](#tab/visual-studio-code)

[!INCLUDE[](~/includes/net-core-prereqs-vsc-3.0.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="639b8-116">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="639b8-116">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

[!INCLUDE[](~/includes/net-core-prereqs-mac-3.0.md)]

---

## <a name="create-a-grpc-service"></a><span data-ttu-id="639b8-117">Tworzenie usługi gRPC</span><span class="sxs-lookup"><span data-stu-id="639b8-117">Create a gRPC service</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="639b8-118">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="639b8-118">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="639b8-119">Uruchom program Visual Studio i wybierz pozycję **Utwórz nowy projekt**.</span><span class="sxs-lookup"><span data-stu-id="639b8-119">Start Visual Studio and select **Create a new project**.</span></span> <span data-ttu-id="639b8-120">Alternatywnie z menu **plik** programu Visual Studio wybierz pozycję **Nowy** > **projekt**.</span><span class="sxs-lookup"><span data-stu-id="639b8-120">Alternatively, from the Visual Studio **File** menu, select **New** > **Project**.</span></span>
* <span data-ttu-id="639b8-121">W oknie dialogowym **Tworzenie nowego projektu** wybierz pozycję **Usługa gPRC** i wybierz pozycję **dalej**:</span><span class="sxs-lookup"><span data-stu-id="639b8-121">In the **Create a new project** dialog, select **gPRC Service** and select **Next**:</span></span>

  ![\* \* Utwórz nowy projekt \* \* okno dialogowe](~/tutorials/grpc/grpc-start/static/cnp.png)

* <span data-ttu-id="639b8-123">Nazwij projekt **GrpcGreeter**.</span><span class="sxs-lookup"><span data-stu-id="639b8-123">Name the project **GrpcGreeter**.</span></span> <span data-ttu-id="639b8-124">Ważne jest, aby nazwa projektu *GrpcGreeter* , tak aby przestrzenie nazw były zgodne podczas kopiowania i wklejania kodu.</span><span class="sxs-lookup"><span data-stu-id="639b8-124">It's important to name the project *GrpcGreeter* so the namespaces will match when you copy and paste code.</span></span>
* <span data-ttu-id="639b8-125">Wybierz pozycję **Utwórz**.</span><span class="sxs-lookup"><span data-stu-id="639b8-125">Select **Create**.</span></span>
* <span data-ttu-id="639b8-126">W oknie dialogowym **Tworzenie nowej usługi gRPC** :</span><span class="sxs-lookup"><span data-stu-id="639b8-126">In the **Create a new gRPC service** dialog:</span></span>
  * <span data-ttu-id="639b8-127">Wybrano szablon **usługi gRPC** .</span><span class="sxs-lookup"><span data-stu-id="639b8-127">The **gRPC Service** template is selected.</span></span>
  * <span data-ttu-id="639b8-128">Wybierz pozycję **Utwórz**.</span><span class="sxs-lookup"><span data-stu-id="639b8-128">Select **Create**.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="639b8-129">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="639b8-129">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="639b8-130">Otwórz [zintegrowany terminal](https://code.visualstudio.com/docs/editor/integrated-terminal).</span><span class="sxs-lookup"><span data-stu-id="639b8-130">Open the [integrated terminal](https://code.visualstudio.com/docs/editor/integrated-terminal).</span></span>
* <span data-ttu-id="639b8-131">Zmień katalogi (`cd`) na folder, który będzie zawierać projekt.</span><span class="sxs-lookup"><span data-stu-id="639b8-131">Change directories (`cd`) to a folder which will contain the project.</span></span>
* <span data-ttu-id="639b8-132">Uruchom następujące polecenia:</span><span class="sxs-lookup"><span data-stu-id="639b8-132">Run the following commands:</span></span>

  ```console
  dotnet new grpc -o GrpcGreeter
  code -r GrpcGreeter
  ```

  * <span data-ttu-id="639b8-133">Polecenie tworzy nową usługę gRPC w folderze GrpcGreeter. `dotnet new`</span><span class="sxs-lookup"><span data-stu-id="639b8-133">The `dotnet new` command creates a new gRPC service in the *GrpcGreeter* folder.</span></span>
  * <span data-ttu-id="639b8-134">Polecenie otwiera folder GrpcGreeter w nowym wystąpieniu Visual Studio Code. `code`</span><span class="sxs-lookup"><span data-stu-id="639b8-134">The `code` command opens the *GrpcGreeter* folder in a new instance of Visual Studio Code.</span></span>

  <span data-ttu-id="639b8-135">Zostanie wyświetlone okno dialogowe z **wymaganymi zasobami do kompilowania i debugowania brakuje w "GrpcGreeter". Dodać je?**</span><span class="sxs-lookup"><span data-stu-id="639b8-135">A dialog box appears with **Required assets to build and debug are missing from 'GrpcGreeter'. Add them?**</span></span>
* <span data-ttu-id="639b8-136">Wybierz pozycję **tak**.</span><span class="sxs-lookup"><span data-stu-id="639b8-136">Select **Yes**.</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="639b8-137">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="639b8-137">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="639b8-138">W terminalu uruchom następujące polecenia:</span><span class="sxs-lookup"><span data-stu-id="639b8-138">From a terminal, run the following commands:</span></span>

```console
  dotnet new grpc -o GrpcGreeter
  cd GrpcGreeter
```

<span data-ttu-id="639b8-139">Poprzednie polecenia używają [interfejs wiersza polecenia platformy .NET Core](/dotnet/core/tools/dotnet) do tworzenia usługi gRPC.</span><span class="sxs-lookup"><span data-stu-id="639b8-139">The preceding commands use the [.NET Core CLI](/dotnet/core/tools/dotnet) to create a gRPC service.</span></span>

### <a name="open-the-project"></a><span data-ttu-id="639b8-140">Otwórz projekt</span><span class="sxs-lookup"><span data-stu-id="639b8-140">Open the project</span></span>

<span data-ttu-id="639b8-141">W programie Visual Studio wybierz pozycję **plik** > **Otwórz**, a następnie wybierz plik *GrpcGreeter. sln* .</span><span class="sxs-lookup"><span data-stu-id="639b8-141">From Visual Studio, select **File** > **Open**, and then select the *GrpcGreeter.sln* file.</span></span>

---

### <a name="run-the-service"></a><span data-ttu-id="639b8-142">Uruchamianie usługi</span><span class="sxs-lookup"><span data-stu-id="639b8-142">Run the service</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="639b8-143">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="639b8-143">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="639b8-144">Naciśnij `Ctrl+F5` klawisz, aby uruchomić usługę gRPC bez debugera.</span><span class="sxs-lookup"><span data-stu-id="639b8-144">Press `Ctrl+F5` to run the gRPC service without the debugger.</span></span>

  <span data-ttu-id="639b8-145">Program Visual Studio uruchamia usługę w wierszu polecenia.</span><span class="sxs-lookup"><span data-stu-id="639b8-145">Visual Studio runs the service in a command prompt.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="639b8-146">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="639b8-146">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="639b8-147">Uruchom gRPC Greeter projektu *GrpcGreeter* z wiersza polecenia przy użyciu `dotnet run`.</span><span class="sxs-lookup"><span data-stu-id="639b8-147">Run the gRPC Greeter project *GrpcGreeter* from the command line using `dotnet run`.</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="639b8-148">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="639b8-148">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="639b8-149">Uruchom gRPC Greeter projektu *GrpcGreeter* z wiersza polecenia przy użyciu `dotnet run`.</span><span class="sxs-lookup"><span data-stu-id="639b8-149">Run the gRPC Greeter project *GrpcGreeter* from the command line using `dotnet run`.</span></span>

---

<span data-ttu-id="639b8-150">Dzienniki pokazują, że usługa nasłuchuje `https://localhost:5001`.</span><span class="sxs-lookup"><span data-stu-id="639b8-150">The logs show the service listening on `https://localhost:5001`.</span></span>

```console
info: Microsoft.Hosting.Lifetime[0]
      Now listening on: https://localhost:5001
info: Microsoft.Hosting.Lifetime[0]
      Application started. Press Ctrl+C to shut down.
info: Microsoft.Hosting.Lifetime[0]
      Hosting environment: Development
```

> [!NOTE]
> <span data-ttu-id="639b8-151">Szablon gRPC jest skonfigurowany do korzystania z [Transport Layer Security (TLS)](https://tools.ietf.org/html/rfc5246).</span><span class="sxs-lookup"><span data-stu-id="639b8-151">The gRPC template is configured to use [Transport Layer Security (TLS)](https://tools.ietf.org/html/rfc5246).</span></span> <span data-ttu-id="639b8-152">Klienci gRPC muszą używać protokołu HTTPS, aby wywołać serwer.</span><span class="sxs-lookup"><span data-stu-id="639b8-152">gRPC clients need to use HTTPS to call the server.</span></span>
>
> <span data-ttu-id="639b8-153">Program macOS nie obsługuje ASP.NET Core gRPC z protokołem TLS.</span><span class="sxs-lookup"><span data-stu-id="639b8-153">macOS doesn't support ASP.NET Core gRPC with TLS.</span></span> <span data-ttu-id="639b8-154">Aby pomyślnie uruchomić usługi gRPC Services w usłudze macOS, wymagana jest dodatkowa konfiguracja.</span><span class="sxs-lookup"><span data-stu-id="639b8-154">Additional configuration is required to successfully run gRPC services on macOS.</span></span> <span data-ttu-id="639b8-155">Aby uzyskać więcej informacji, zobacz [nie można uruchomić aplikacji ASP.NET Core gRPC w witrynie macOS](xref:grpc/troubleshoot#unable-to-start-aspnet-core-grpc-app-on-macos).</span><span class="sxs-lookup"><span data-stu-id="639b8-155">For more information, see [Unable to start ASP.NET Core gRPC app on macOS](xref:grpc/troubleshoot#unable-to-start-aspnet-core-grpc-app-on-macos).</span></span>

### <a name="examine-the-project-files"></a><span data-ttu-id="639b8-156">Sprawdzanie plików projektu</span><span class="sxs-lookup"><span data-stu-id="639b8-156">Examine the project files</span></span>

<span data-ttu-id="639b8-157">*GrpcGreeter* pliki projektu:</span><span class="sxs-lookup"><span data-stu-id="639b8-157">*GrpcGreeter* project files:</span></span>

* <span data-ttu-id="639b8-158">*Greeting. proto* &ndash; `Greeter` plik *Protos/Greeting. proto* definiuje gRPC i służy do generowania zasobów serwera gRPC.</span><span class="sxs-lookup"><span data-stu-id="639b8-158">*greet.proto* &ndash; The *Protos/greet.proto* file defines the `Greeter` gRPC and is used to generate the gRPC server assets.</span></span> <span data-ttu-id="639b8-159">Aby uzyskać więcej informacji, zobacz [wprowadzenie do gRPC](xref:grpc/index).</span><span class="sxs-lookup"><span data-stu-id="639b8-159">For more information, see [Introduction to gRPC](xref:grpc/index).</span></span>
* <span data-ttu-id="639b8-160">Folder *usługi* : Zawiera implementację `Greeter` usługi.</span><span class="sxs-lookup"><span data-stu-id="639b8-160">*Services* folder: Contains the implementation of the `Greeter` service.</span></span>
* <span data-ttu-id="639b8-161">plik *appSettings. JSON* &ndash; zawiera dane konfiguracyjne, takie jak protokół używany przez Kestrel.</span><span class="sxs-lookup"><span data-stu-id="639b8-161">*appSettings.json* &ndash; Contains configuration data, such as protocol used by Kestrel.</span></span> <span data-ttu-id="639b8-162">Aby uzyskać więcej informacji, zobacz <xref:fundamentals/configuration/index>.</span><span class="sxs-lookup"><span data-stu-id="639b8-162">For more information, see <xref:fundamentals/configuration/index>.</span></span>
* <span data-ttu-id="639b8-163">Program.cs&ndash; zawiera punkt wejścia dla usługi gRPC.</span><span class="sxs-lookup"><span data-stu-id="639b8-163">*Program.cs* &ndash; Contains the entry point for the gRPC service.</span></span> <span data-ttu-id="639b8-164">Aby uzyskać więcej informacji, zobacz <xref:fundamentals/host/generic-host>.</span><span class="sxs-lookup"><span data-stu-id="639b8-164">For more information, see <xref:fundamentals/host/generic-host>.</span></span>
* <span data-ttu-id="639b8-165">Startup.cs&ndash; zawiera kod, który konfiguruje zachowanie aplikacji.</span><span class="sxs-lookup"><span data-stu-id="639b8-165">*Startup.cs* &ndash; Contains code that configures app behavior.</span></span> <span data-ttu-id="639b8-166">Aby uzyskać więcej informacji, zobacz [Uruchamianie aplikacji](xref:fundamentals/startup).</span><span class="sxs-lookup"><span data-stu-id="639b8-166">For more information, see [App startup](xref:fundamentals/startup).</span></span>

## <a name="create-the-grpc-client-in-a-net-console-app"></a><span data-ttu-id="639b8-167">Tworzenie klienta gRPC w aplikacji konsolowej .NET</span><span class="sxs-lookup"><span data-stu-id="639b8-167">Create the gRPC client in a .NET console app</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="639b8-168">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="639b8-168">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="639b8-169">Otwórz drugie wystąpienie programu Visual Studio i wybierz pozycję **Utwórz nowy projekt**.</span><span class="sxs-lookup"><span data-stu-id="639b8-169">Open a second instance of Visual Studio and select **Create a new project**.</span></span>
* <span data-ttu-id="639b8-170">W oknie dialogowym **Tworzenie nowego projektu** wybierz pozycję **aplikacja konsoli (.NET Core)** , a następnie wybierz pozycję **dalej**.</span><span class="sxs-lookup"><span data-stu-id="639b8-170">In the **Create a new project** dialog, select **Console App (.NET Core)** and select **Next**.</span></span>
* <span data-ttu-id="639b8-171">W polu tekstowym **Nazwa** wprowadź **GrpcGreeterClient** , a następnie wybierz pozycję **Utwórz**.</span><span class="sxs-lookup"><span data-stu-id="639b8-171">In the **Name** text box, enter **GrpcGreeterClient** and select **Create**.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="639b8-172">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="639b8-172">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="639b8-173">Otwórz [zintegrowany terminal](https://code.visualstudio.com/docs/editor/integrated-terminal).</span><span class="sxs-lookup"><span data-stu-id="639b8-173">Open the [integrated terminal](https://code.visualstudio.com/docs/editor/integrated-terminal).</span></span>
* <span data-ttu-id="639b8-174">Zmień katalogi (`cd`) na folder, który będzie zawierać projekt.</span><span class="sxs-lookup"><span data-stu-id="639b8-174">Change directories (`cd`) to a folder which will contain the project.</span></span>
* <span data-ttu-id="639b8-175">Uruchom następujące polecenia:</span><span class="sxs-lookup"><span data-stu-id="639b8-175">Run the following commands:</span></span>

  ```console
  dotnet new console -o GrpcGreeterClient
  code -r GrpcGreeterClient
  ```

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="639b8-176">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="639b8-176">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="639b8-177">Postępuj zgodnie z instrukcjami w temacie [Tworzenie kompletnego rozwiązania .NET Core w systemie macOS przy użyciu Visual Studio dla komputerów Mac](/dotnet/core/tutorials/using-on-mac-vs-full-solution) , aby utworzyć aplikację konsolową o nazwie *GrpcGreeterClient*.</span><span class="sxs-lookup"><span data-stu-id="639b8-177">Follow the instructions in [Building a complete .NET Core solution on macOS using Visual Studio for Mac](/dotnet/core/tutorials/using-on-mac-vs-full-solution) to create a console app with the name *GrpcGreeterClient*.</span></span>

---

### <a name="add-required-packages"></a><span data-ttu-id="639b8-178">Dodaj wymagane pakiety</span><span class="sxs-lookup"><span data-stu-id="639b8-178">Add required packages</span></span>

<span data-ttu-id="639b8-179">Projekt klienta gRPC wymaga następujących pakietów:</span><span class="sxs-lookup"><span data-stu-id="639b8-179">The gRPC client project requires the following packages:</span></span>

* <span data-ttu-id="639b8-180">[GRPC .NET. Client](https://www.nuget.org/packages/Grpc.Net.Client), który zawiera klienta .NET Core.</span><span class="sxs-lookup"><span data-stu-id="639b8-180">[Grpc.Net.Client](https://www.nuget.org/packages/Grpc.Net.Client), which contains the .NET Core client.</span></span>
* <span data-ttu-id="639b8-181">[Google. protobuf](https://www.nuget.org/packages/Google.Protobuf/), który zawiera interfejsy API komunikatów protobuf C#dla.</span><span class="sxs-lookup"><span data-stu-id="639b8-181">[Google.Protobuf](https://www.nuget.org/packages/Google.Protobuf/), which contains protobuf message APIs for C#.</span></span>
* <span data-ttu-id="639b8-182">[GRPC. Tools](https://www.nuget.org/packages/Grpc.Tools/), która zawiera C# obsługę narzędzi dla plików protobuf.</span><span class="sxs-lookup"><span data-stu-id="639b8-182">[Grpc.Tools](https://www.nuget.org/packages/Grpc.Tools/), which contains C# tooling support for protobuf files.</span></span> <span data-ttu-id="639b8-183">Pakiet narzędzi nie jest wymagany w czasie wykonywania, dlatego zależność jest oznaczona za pomocą `PrivateAssets="All"`.</span><span class="sxs-lookup"><span data-stu-id="639b8-183">The tooling package isn't required at runtime, so the dependency is marked with `PrivateAssets="All"`.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="639b8-184">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="639b8-184">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="639b8-185">Zainstaluj pakiety przy użyciu konsoli Menedżera pakietów (PMC) lub Zarządzaj pakietami NuGet.</span><span class="sxs-lookup"><span data-stu-id="639b8-185">Install the packages using either the Package Manager Console (PMC) or Manage NuGet Packages.</span></span>

#### <a name="pmc-option-to-install-packages"></a><span data-ttu-id="639b8-186">Opcja PMC, aby zainstalować pakiety</span><span class="sxs-lookup"><span data-stu-id="639b8-186">PMC option to install packages</span></span>

* <span data-ttu-id="639b8-187">W programie Visual Studio wybierz kolejno pozycje **Narzędzia** > Menedżer**pakietów** > NuGet**konsola Menedżera pakietów**</span><span class="sxs-lookup"><span data-stu-id="639b8-187">From Visual Studio, select **Tools** > **NuGet Package Manager** > **Package Manager Console**</span></span>
* <span data-ttu-id="639b8-188">W oknie **konsola Menedżera pakietów** Uruchom `cd GrpcGreeterClient` polecenie, aby zmienić katalogi na folder zawierający pliki *GrpcGreeterClient. csproj* .</span><span class="sxs-lookup"><span data-stu-id="639b8-188">From the **Package Manager Console** window, run `cd GrpcGreeterClient` to change directories to the folder containing the *GrpcGreeterClient.csproj* files.</span></span>
* <span data-ttu-id="639b8-189">Uruchom następujące polecenia:</span><span class="sxs-lookup"><span data-stu-id="639b8-189">Run the following commands:</span></span>

  ```powershell
  Install-Package Grpc.Net.Client -prerelease
  Install-Package Google.Protobuf -prerelease
  Install-Package Grpc.Tools -prerelease
  ```

#### <a name="manage-nuget-packages-option-to-install-packages"></a><span data-ttu-id="639b8-190">Opcja zarządzania pakietami NuGet w celu zainstalowania pakietów</span><span class="sxs-lookup"><span data-stu-id="639b8-190">Manage NuGet Packages option to install packages</span></span>

* <span data-ttu-id="639b8-191">Kliknij prawym przyciskiem myszy projekt w **Eksplorator rozwiązań** > **Zarządzanie pakietami NuGet**</span><span class="sxs-lookup"><span data-stu-id="639b8-191">Right-click the project in **Solution Explorer** > **Manage NuGet Packages**</span></span>
* <span data-ttu-id="639b8-192">Wybierz kartę **przeglądanie** .</span><span class="sxs-lookup"><span data-stu-id="639b8-192">Select the **Browse** tab.</span></span>
* <span data-ttu-id="639b8-193">W polu wyszukiwania wprowadź **GRPC .NET. Client** .</span><span class="sxs-lookup"><span data-stu-id="639b8-193">Enter **Grpc.Net.Client** in the search box.</span></span>
* <span data-ttu-id="639b8-194">Wybierz pakiet **GRPC .NET. Client** z karty **Przeglądaj** i wybierz pozycję **Zainstaluj**.</span><span class="sxs-lookup"><span data-stu-id="639b8-194">Select the **Grpc.Net.Client** package from the **Browse** tab and select **Install**.</span></span>
* <span data-ttu-id="639b8-195">Powtórz dla `Google.Protobuf` i `Grpc.Tools`.</span><span class="sxs-lookup"><span data-stu-id="639b8-195">Repeat for `Google.Protobuf` and `Grpc.Tools`.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="639b8-196">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="639b8-196">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="639b8-197">Uruchom następujące polecenia w zintegrowanym **terminalu**:</span><span class="sxs-lookup"><span data-stu-id="639b8-197">Run the following commands from the **Integrated Terminal**:</span></span>

```console
dotnet add GrpcGreeterClient.csproj package Grpc.Net.Client
dotnet add GrpcGreeterClient.csproj package Google.Protobuf
dotnet add GrpcGreeterClient.csproj package Grpc.Tools
```

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="639b8-198">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="639b8-198">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="639b8-199">Kliknij prawym przyciskiem myszy folder **pakiety** w **okienko rozwiązania** > **Dodaj pakiety**</span><span class="sxs-lookup"><span data-stu-id="639b8-199">Right-click the **Packages** folder in **Solution Pad** > **Add Packages**</span></span>
* <span data-ttu-id="639b8-200">W polu wyszukiwania wprowadź **GRPC .NET. Client** .</span><span class="sxs-lookup"><span data-stu-id="639b8-200">Enter **Grpc.Net.Client** in the search box.</span></span>
* <span data-ttu-id="639b8-201">Wybierz pakiet **GRPC .NET. Client** z okienka wyników, a następnie wybierz pozycję **Dodaj pakiet** .</span><span class="sxs-lookup"><span data-stu-id="639b8-201">Select the **Grpc.Net.Client** package from the results pane and select **Add Package**</span></span>
* <span data-ttu-id="639b8-202">Powtórz dla `Google.Protobuf` i `Grpc.Tools`.</span><span class="sxs-lookup"><span data-stu-id="639b8-202">Repeat for `Google.Protobuf` and `Grpc.Tools`.</span></span>

---

### <a name="add-greetproto"></a><span data-ttu-id="639b8-203">Dodaj Greeting. proto</span><span class="sxs-lookup"><span data-stu-id="639b8-203">Add greet.proto</span></span>

* <span data-ttu-id="639b8-204">Utwórz folder *Protos* w projekcie klienta gRPC.</span><span class="sxs-lookup"><span data-stu-id="639b8-204">Create a *Protos* folder in the gRPC client project.</span></span>
* <span data-ttu-id="639b8-205">Skopiuj plik *Protos\greet.proto* z usługi gRPC Greeter do projektu klienta gRPC.</span><span class="sxs-lookup"><span data-stu-id="639b8-205">Copy the *Protos\greet.proto* file from the gRPC Greeter service to the gRPC client project.</span></span>
* <span data-ttu-id="639b8-206">Edytuj plik projektu *GrpcGreeterClient. csproj* :</span><span class="sxs-lookup"><span data-stu-id="639b8-206">Edit the *GrpcGreeterClient.csproj* project file:</span></span>

  # <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="639b8-207">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="639b8-207">Visual Studio</span></span>](#tab/visual-studio)

  <span data-ttu-id="639b8-208">Kliknij prawym przyciskiem myszy projekt i wybierz polecenie **Edytuj plik projektu**.</span><span class="sxs-lookup"><span data-stu-id="639b8-208">Right-click the project and select **Edit Project File**.</span></span>

  # <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="639b8-209">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="639b8-209">Visual Studio Code</span></span>](#tab/visual-studio-code)

  <span data-ttu-id="639b8-210">Wybierz plik *GrpcGreeterClient. csproj* .</span><span class="sxs-lookup"><span data-stu-id="639b8-210">Select the *GrpcGreeterClient.csproj* file.</span></span>

  # <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="639b8-211">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="639b8-211">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

  <span data-ttu-id="639b8-212">Kliknij prawym przyciskiem myszy projekt i wybierz polecenie **Narzędzia** > **Edytuj plik**.</span><span class="sxs-lookup"><span data-stu-id="639b8-212">Right-click the project and select **Tools** > **Edit File**.</span></span>

  ---

* <span data-ttu-id="639b8-213">Dodaj grupę elementów z `<Protobuf>` elementem, który odwołuje się do pliku *Greeting. proto* :</span><span class="sxs-lookup"><span data-stu-id="639b8-213">Add an item group with a `<Protobuf>` element that refers to the *greet.proto* file:</span></span>

  ```xml
  <ItemGroup>
    <Protobuf Include="Protos\greet.proto" GrpcServices="Client" />
  </ItemGroup>
  ```

### <a name="create-the-greeter-client"></a><span data-ttu-id="639b8-214">Tworzenie klienta Greeter</span><span class="sxs-lookup"><span data-stu-id="639b8-214">Create the Greeter client</span></span>

<span data-ttu-id="639b8-215">Skompiluj projekt, aby utworzyć typy w `GrpcGreeter` przestrzeni nazw.</span><span class="sxs-lookup"><span data-stu-id="639b8-215">Build the project to create the types in the `GrpcGreeter` namespace.</span></span> <span data-ttu-id="639b8-216">`GrpcGreeter` Typy są generowane automatycznie przez proces kompilacji.</span><span class="sxs-lookup"><span data-stu-id="639b8-216">The `GrpcGreeter` types are generated automatically by the build process.</span></span>

<span data-ttu-id="639b8-217">Zaktualizuj plik *program.cs* klienta gRPC za pomocą następującego kodu:</span><span class="sxs-lookup"><span data-stu-id="639b8-217">Update the gRPC client *Program.cs* file with the following code:</span></span>

[!code-csharp[](~/tutorials/grpc/grpc-start/sample/GrpcGreeterClient/Program.cs?name=snippet2)]

<span data-ttu-id="639b8-218">*Program.cs* zawiera punkt wejścia i logikę dla klienta gRPC.</span><span class="sxs-lookup"><span data-stu-id="639b8-218">*Program.cs* contains the entry point and logic for the gRPC client.</span></span>

<span data-ttu-id="639b8-219">Klient Greeter jest tworzony przez:</span><span class="sxs-lookup"><span data-stu-id="639b8-219">The Greeter client is created by:</span></span>

* <span data-ttu-id="639b8-220">Utworzenie wystąpienia zawierającego informacje dotyczące tworzenia połączenia z usługą gRPC. `HttpClient`</span><span class="sxs-lookup"><span data-stu-id="639b8-220">Instantiating an `HttpClient` containing the information for creating the connection to the gRPC service.</span></span>
* <span data-ttu-id="639b8-221">Korzystanie z programu do konstruowania klienta Greeter: `HttpClient`</span><span class="sxs-lookup"><span data-stu-id="639b8-221">Using the `HttpClient` to construct the Greeter client:</span></span>

[!code-csharp[](~/tutorials/grpc/grpc-start/sample/GrpcGreeterClient/Program.cs?name=snippet&highlight=3-6)]

<span data-ttu-id="639b8-222">Klient Greeter wywołuje metodę asynchroniczną `SayHello` .</span><span class="sxs-lookup"><span data-stu-id="639b8-222">The Greeter client calls the asynchronous `SayHello` method.</span></span> <span data-ttu-id="639b8-223">Zostanie wyświetlony wynik `SayHello` wywołania:</span><span class="sxs-lookup"><span data-stu-id="639b8-223">The result of the `SayHello` call is displayed:</span></span>

[!code-csharp[](~/tutorials/grpc/grpc-start/sample/GrpcGreeterClient/Program.cs?name=snippet&highlight=7-9)]

## <a name="test-the-grpc-client-with-the-grpc-greeter-service"></a><span data-ttu-id="639b8-224">Testowanie klienta gRPC za pomocą usługi gRPC Greeter</span><span class="sxs-lookup"><span data-stu-id="639b8-224">Test the gRPC client with the gRPC Greeter service</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="639b8-225">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="639b8-225">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="639b8-226">W usłudze Greeter naciśnij klawisz `Ctrl+F5` , aby uruchomić serwer bez debugera.</span><span class="sxs-lookup"><span data-stu-id="639b8-226">In the Greeter service, press `Ctrl+F5` to start the server without the debugger.</span></span>
* <span data-ttu-id="639b8-227">W projekcie naciśnij klawisz `Ctrl+F5` , aby uruchomić klienta bez debugera. `GrpcGreeterClient`</span><span class="sxs-lookup"><span data-stu-id="639b8-227">In the `GrpcGreeterClient` project, press `Ctrl+F5` to start the client without the debugger.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="639b8-228">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="639b8-228">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="639b8-229">Uruchom usługę Greeter.</span><span class="sxs-lookup"><span data-stu-id="639b8-229">Start the Greeter service.</span></span>
* <span data-ttu-id="639b8-230">Uruchom klienta programu.</span><span class="sxs-lookup"><span data-stu-id="639b8-230">Start the client.</span></span>


# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="639b8-231">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="639b8-231">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="639b8-232">Uruchom usługę Greeter.</span><span class="sxs-lookup"><span data-stu-id="639b8-232">Start the Greeter service.</span></span>
* <span data-ttu-id="639b8-233">Uruchom klienta programu.</span><span class="sxs-lookup"><span data-stu-id="639b8-233">Start the client.</span></span>

---

<span data-ttu-id="639b8-234">Klient wysyła do usługi powitanie z komunikatem zawierającym nazwę *GreeterClient*.</span><span class="sxs-lookup"><span data-stu-id="639b8-234">The client sends a greeting to the service with a message containing its name, *GreeterClient*.</span></span> <span data-ttu-id="639b8-235">Usługa wysyła komunikat "Hello GreeterClient" jako odpowiedź.</span><span class="sxs-lookup"><span data-stu-id="639b8-235">The service sends the message "Hello GreeterClient" as a response.</span></span> <span data-ttu-id="639b8-236">Odpowiedź "Hello GreeterClient" jest wyświetlana w wierszu polecenia:</span><span class="sxs-lookup"><span data-stu-id="639b8-236">The "Hello GreeterClient" response is displayed in the command prompt:</span></span>

```console
Greeting: Hello GreeterClient
Press any key to exit...
```

<span data-ttu-id="639b8-237">Usługa gRPC rejestruje szczegóły pomyślnego wywołania w dziennikach, które zostały zapisane w wierszu polecenia:</span><span class="sxs-lookup"><span data-stu-id="639b8-237">The gRPC service records the details of the successful call in the logs written to the command prompt:</span></span>

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
> <span data-ttu-id="639b8-238">Kod w tym artykule wymaga certyfikatu programistycznego HTTPS ASP.NET Core do zabezpieczenia usługi gRPC.</span><span class="sxs-lookup"><span data-stu-id="639b8-238">The code in this article requires the ASP.NET Core HTTPS development certificate to secure the gRPC service.</span></span> <span data-ttu-id="639b8-239">Jeśli klient nie powiedzie się z `The remote certificate is invalid according to the validation procedure.`komunikatem, certyfikat deweloperski nie jest zaufany.</span><span class="sxs-lookup"><span data-stu-id="639b8-239">If the client fails with the message `The remote certificate is invalid according to the validation procedure.`, the development certificate is not trusted.</span></span> <span data-ttu-id="639b8-240">Aby uzyskać instrukcje dotyczące rozwiązania tego problemu, zobacz temat [ASP.NET Core ufanie certyfikatowi Deweloperskiemu protokołu HTTPS w systemie Windows i macOS](xref:security/enforcing-ssl#trust-the-aspnet-core-https-development-certificate-on-windows-and-macos).</span><span class="sxs-lookup"><span data-stu-id="639b8-240">For instructions to fix this issue, see [Trust the ASP.NET Core HTTPS development certificate on Windows and macOS](xref:security/enforcing-ssl#trust-the-aspnet-core-https-development-certificate-on-windows-and-macos).</span></span>

### <a name="next-steps"></a><span data-ttu-id="639b8-241">Następne kroki</span><span class="sxs-lookup"><span data-stu-id="639b8-241">Next steps</span></span>

* <xref:grpc/index>
* <xref:grpc/basics>
* <xref:grpc/migration>
