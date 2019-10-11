---
title: Tworzenie klienta i serwera platformy .NET Core gRPC w ASP.NET Core
author: juntaoluo
description: W tym samouczku pokazano, jak utworzyć usługę gRPC i klienta gRPC na ASP.NET Core. Dowiedz się, jak utworzyć projekt usługi gRPC, edytować plik PROTO i dodać wywołanie przesyłania strumieniowego dupleksu.
monikerRange: '>= aspnetcore-3.0'
ms.author: johluo
ms.date: 10/10/2019
uid: tutorials/grpc/grpc-start
ms.openlocfilehash: 61324cdd5b574ea8a12a1be5846a25c311ab4499
ms.sourcegitcommit: 7d3c6565dda6241eb13f9a8e1e1fd89b1cfe4d18
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2019
ms.locfileid: "72259669"
---
# <a name="tutorial-create-a-grpc-client-and-server-in-aspnet-core"></a><span data-ttu-id="a0e36-104">Samouczek: Tworzenie gRPC klienta i serwera w ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="a0e36-104">Tutorial: Create a gRPC client and server in ASP.NET Core</span></span>

<span data-ttu-id="a0e36-105">Przez [Jan Luo](https://github.com/juntaoluo)</span><span class="sxs-lookup"><span data-stu-id="a0e36-105">By [John Luo](https://github.com/juntaoluo)</span></span>

<span data-ttu-id="a0e36-106">W tym samouczku pokazano, jak utworzyć klienta programu .NET Core [gRPC](https://grpc.io/docs/guides/) oraz serwer gRPC ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="a0e36-106">This tutorial shows how to create a .NET Core [gRPC](https://grpc.io/docs/guides/) client and an ASP.NET Core gRPC Server.</span></span>

<span data-ttu-id="a0e36-107">Na koniec będziesz mieć klienta gRPC, który komunikuje się z usługą gRPC Greeter.</span><span class="sxs-lookup"><span data-stu-id="a0e36-107">At the end, you'll have a gRPC client that communicates with the gRPC Greeter service.</span></span>

<span data-ttu-id="a0e36-108">[Wyświetlanie lub Pobieranie przykładowego kodu](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/tutorials/grpc/grpc-start/sample) ([jak pobrać](xref:index#how-to-download-a-sample)).</span><span class="sxs-lookup"><span data-stu-id="a0e36-108">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/tutorials/grpc/grpc-start/sample) ([how to download](xref:index#how-to-download-a-sample)).</span></span>

<span data-ttu-id="a0e36-109">W tym samouczku zostaną wykonane następujące czynności:</span><span class="sxs-lookup"><span data-stu-id="a0e36-109">In this tutorial, you:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="a0e36-110">Utwórz serwer gRPC.</span><span class="sxs-lookup"><span data-stu-id="a0e36-110">Create a gRPC Server.</span></span>
> * <span data-ttu-id="a0e36-111">Utwórz klienta gRPC.</span><span class="sxs-lookup"><span data-stu-id="a0e36-111">Create a gRPC client.</span></span>
> * <span data-ttu-id="a0e36-112">Przetestuj usługę klienta gRPC za pomocą usługi gRPC Greeter.</span><span class="sxs-lookup"><span data-stu-id="a0e36-112">Test the gRPC client service with the gRPC Greeter service.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="a0e36-113">Wymagania wstępne</span><span class="sxs-lookup"><span data-stu-id="a0e36-113">Prerequisites</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="a0e36-114">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="a0e36-114">Visual Studio</span></span>](#tab/visual-studio)

[!INCLUDE[](~/includes/net-core-prereqs-vs-3.0.md)]

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="a0e36-115">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="a0e36-115">Visual Studio Code</span></span>](#tab/visual-studio-code)

[!INCLUDE[](~/includes/net-core-prereqs-vsc-3.0.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="a0e36-116">program Visual Studio dla komputerów Mac</span><span class="sxs-lookup"><span data-stu-id="a0e36-116">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

[!INCLUDE[](~/includes/net-core-prereqs-mac-3.0.md)]

---

## <a name="create-a-grpc-service"></a><span data-ttu-id="a0e36-117">Tworzenie usługi gRPC</span><span class="sxs-lookup"><span data-stu-id="a0e36-117">Create a gRPC service</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="a0e36-118">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="a0e36-118">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="a0e36-119">Uruchom program Visual Studio i wybierz pozycję **Utwórz nowy projekt**.</span><span class="sxs-lookup"><span data-stu-id="a0e36-119">Start Visual Studio and select **Create a new project**.</span></span> <span data-ttu-id="a0e36-120">Alternatywnie z menu **plik** programu Visual Studio wybierz pozycję **Nowy** **projekt** > .</span><span class="sxs-lookup"><span data-stu-id="a0e36-120">Alternatively, from the Visual Studio **File** menu, select **New** > **Project**.</span></span>
* <span data-ttu-id="a0e36-121">W oknie dialogowym **Tworzenie nowego projektu** wybierz pozycję **Usługa gRPC** i wybierz pozycję **dalej**:</span><span class="sxs-lookup"><span data-stu-id="a0e36-121">In the **Create a new project** dialog, select **gRPC Service** and select **Next**:</span></span>

  ![\* \* Utwórz nowy projekt \* \* okno dialogowe](~/tutorials/grpc/grpc-start/static/cnp.png)

* <span data-ttu-id="a0e36-123">Nazwij projekt **GrpcGreeter**.</span><span class="sxs-lookup"><span data-stu-id="a0e36-123">Name the project **GrpcGreeter**.</span></span> <span data-ttu-id="a0e36-124">Ważne jest, aby nazwa projektu *GrpcGreeter* , tak aby przestrzenie nazw były zgodne podczas kopiowania i wklejania kodu.</span><span class="sxs-lookup"><span data-stu-id="a0e36-124">It's important to name the project *GrpcGreeter* so the namespaces will match when you copy and paste code.</span></span>
* <span data-ttu-id="a0e36-125">Wybierz pozycję **Utwórz**.</span><span class="sxs-lookup"><span data-stu-id="a0e36-125">Select **Create**.</span></span>
* <span data-ttu-id="a0e36-126">W oknie dialogowym **Tworzenie nowej usługi gRPC** :</span><span class="sxs-lookup"><span data-stu-id="a0e36-126">In the **Create a new gRPC service** dialog:</span></span>
  * <span data-ttu-id="a0e36-127">Wybrano szablon **usługi gRPC** .</span><span class="sxs-lookup"><span data-stu-id="a0e36-127">The **gRPC Service** template is selected.</span></span>
  * <span data-ttu-id="a0e36-128">Wybierz pozycję **Utwórz**.</span><span class="sxs-lookup"><span data-stu-id="a0e36-128">Select **Create**.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="a0e36-129">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="a0e36-129">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="a0e36-130">Otwórz [zintegrowany terminal](https://code.visualstudio.com/docs/editor/integrated-terminal).</span><span class="sxs-lookup"><span data-stu-id="a0e36-130">Open the [integrated terminal](https://code.visualstudio.com/docs/editor/integrated-terminal).</span></span>
* <span data-ttu-id="a0e36-131">Zmień katalog (`cd`) do folderu, który będzie zawierać projekt.</span><span class="sxs-lookup"><span data-stu-id="a0e36-131">Change directories (`cd`) to a folder which will contain the project.</span></span>
* <span data-ttu-id="a0e36-132">Uruchom następujące polecenia:</span><span class="sxs-lookup"><span data-stu-id="a0e36-132">Run the following commands:</span></span>

  ```dotnetcli
  dotnet new grpc -o GrpcGreeter
  code -r GrpcGreeter
  ```

  * <span data-ttu-id="a0e36-133">Polecenie `dotnet new` tworzy nową usługę gRPC w folderze *GrpcGreeter* .</span><span class="sxs-lookup"><span data-stu-id="a0e36-133">The `dotnet new` command creates a new gRPC service in the *GrpcGreeter* folder.</span></span>
  * <span data-ttu-id="a0e36-134">Polecenie `code` otwiera folder *GrpcGreeter* w nowym wystąpieniu Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="a0e36-134">The `code` command opens the *GrpcGreeter* folder in a new instance of Visual Studio Code.</span></span>

  <span data-ttu-id="a0e36-135">Zostanie wyświetlone okno dialogowe z **wymaganymi zasobami do kompilowania i debugowania brakuje w "GrpcGreeter". Dodać je?**</span><span class="sxs-lookup"><span data-stu-id="a0e36-135">A dialog box appears with **Required assets to build and debug are missing from 'GrpcGreeter'. Add them?**</span></span>
* <span data-ttu-id="a0e36-136">Wybierz pozycję **tak**.</span><span class="sxs-lookup"><span data-stu-id="a0e36-136">Select **Yes**.</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="a0e36-137">program Visual Studio dla komputerów Mac</span><span class="sxs-lookup"><span data-stu-id="a0e36-137">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="a0e36-138">W terminalu uruchom następujące polecenia:</span><span class="sxs-lookup"><span data-stu-id="a0e36-138">From a terminal, run the following commands:</span></span>

```dotnetcli
dotnet new grpc -o GrpcGreeter
cd GrpcGreeter
```

<span data-ttu-id="a0e36-139">Poprzednie polecenia używają [interfejs wiersza polecenia platformy .NET Core](/dotnet/core/tools/dotnet) do tworzenia usługi gRPC.</span><span class="sxs-lookup"><span data-stu-id="a0e36-139">The preceding commands use the [.NET Core CLI](/dotnet/core/tools/dotnet) to create a gRPC service.</span></span>

### <a name="open-the-project"></a><span data-ttu-id="a0e36-140">Otwórz projekt</span><span class="sxs-lookup"><span data-stu-id="a0e36-140">Open the project</span></span>

<span data-ttu-id="a0e36-141">W programie Visual Studio wybierz pozycję **plik** > **Otwórz**, a następnie wybierz plik *GrpcGreeter. csproj* .</span><span class="sxs-lookup"><span data-stu-id="a0e36-141">From Visual Studio, select **File** > **Open**, and then select the *GrpcGreeter.csproj* file.</span></span>

---

### <a name="run-the-service"></a><span data-ttu-id="a0e36-142">Uruchamianie usługi</span><span class="sxs-lookup"><span data-stu-id="a0e36-142">Run the service</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="a0e36-143">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="a0e36-143">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="a0e36-144">Naciśnij `Ctrl+F5`, aby uruchomić usługę gRPC bez debugera.</span><span class="sxs-lookup"><span data-stu-id="a0e36-144">Press `Ctrl+F5` to run the gRPC service without the debugger.</span></span>

  <span data-ttu-id="a0e36-145">Program Visual Studio uruchamia usługę w wierszu polecenia.</span><span class="sxs-lookup"><span data-stu-id="a0e36-145">Visual Studio runs the service in a command prompt.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="a0e36-146">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="a0e36-146">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="a0e36-147">Uruchom gRPC Greeter projektu *GrpcGreeter* z wiersza polecenia przy użyciu `dotnet run`.</span><span class="sxs-lookup"><span data-stu-id="a0e36-147">Run the gRPC Greeter project *GrpcGreeter* from the command line using `dotnet run`.</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="a0e36-148">program Visual Studio dla komputerów Mac</span><span class="sxs-lookup"><span data-stu-id="a0e36-148">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="a0e36-149">Uruchom gRPC Greeter projektu *GrpcGreeter* z wiersza polecenia przy użyciu `dotnet run`.</span><span class="sxs-lookup"><span data-stu-id="a0e36-149">Run the gRPC Greeter project *GrpcGreeter* from the command line using `dotnet run`.</span></span>

---

<span data-ttu-id="a0e36-150">Dzienniki pokazują, że usługa nasłuchuje na `https://localhost:5001`.</span><span class="sxs-lookup"><span data-stu-id="a0e36-150">The logs show the service listening on `https://localhost:5001`.</span></span>

```console
info: Microsoft.Hosting.Lifetime[0]
      Now listening on: https://localhost:5001
info: Microsoft.Hosting.Lifetime[0]
      Application started. Press Ctrl+C to shut down.
info: Microsoft.Hosting.Lifetime[0]
      Hosting environment: Development
```

> [!NOTE]
> <span data-ttu-id="a0e36-151">Szablon gRPC jest skonfigurowany do korzystania z [Transport Layer Security (TLS)](https://tools.ietf.org/html/rfc5246).</span><span class="sxs-lookup"><span data-stu-id="a0e36-151">The gRPC template is configured to use [Transport Layer Security (TLS)](https://tools.ietf.org/html/rfc5246).</span></span> <span data-ttu-id="a0e36-152">Klienci gRPC muszą używać protokołu HTTPS, aby wywołać serwer.</span><span class="sxs-lookup"><span data-stu-id="a0e36-152">gRPC clients need to use HTTPS to call the server.</span></span>
>
> <span data-ttu-id="a0e36-153">Program macOS nie obsługuje ASP.NET Core gRPC z protokołem TLS.</span><span class="sxs-lookup"><span data-stu-id="a0e36-153">macOS doesn't support ASP.NET Core gRPC with TLS.</span></span> <span data-ttu-id="a0e36-154">Aby pomyślnie uruchomić usługi gRPC Services w usłudze macOS, wymagana jest dodatkowa konfiguracja.</span><span class="sxs-lookup"><span data-stu-id="a0e36-154">Additional configuration is required to successfully run gRPC services on macOS.</span></span> <span data-ttu-id="a0e36-155">Aby uzyskać więcej informacji, zobacz [nie można uruchomić aplikacji ASP.NET Core gRPC w witrynie macOS](xref:grpc/troubleshoot#unable-to-start-aspnet-core-grpc-app-on-macos).</span><span class="sxs-lookup"><span data-stu-id="a0e36-155">For more information, see [Unable to start ASP.NET Core gRPC app on macOS](xref:grpc/troubleshoot#unable-to-start-aspnet-core-grpc-app-on-macos).</span></span>

### <a name="examine-the-project-files"></a><span data-ttu-id="a0e36-156">Sprawdzanie plików projektu</span><span class="sxs-lookup"><span data-stu-id="a0e36-156">Examine the project files</span></span>

<span data-ttu-id="a0e36-157">*GrpcGreeter* pliki projektu:</span><span class="sxs-lookup"><span data-stu-id="a0e36-157">*GrpcGreeter* project files:</span></span>

* <span data-ttu-id="a0e36-158">*Greeting. proto* &ndash; plik *Protos/Greeting. proto* definiuje `Greeter` gRPC i służy do generowania zasobów serwera gRPC.</span><span class="sxs-lookup"><span data-stu-id="a0e36-158">*greet.proto* &ndash; The *Protos/greet.proto* file defines the `Greeter` gRPC and is used to generate the gRPC server assets.</span></span> <span data-ttu-id="a0e36-159">Aby uzyskać więcej informacji, zobacz [wprowadzenie do gRPC](xref:grpc/index).</span><span class="sxs-lookup"><span data-stu-id="a0e36-159">For more information, see [Introduction to gRPC](xref:grpc/index).</span></span>
* <span data-ttu-id="a0e36-160">Folder *usługi* : zawiera implementację usługi `Greeter`.</span><span class="sxs-lookup"><span data-stu-id="a0e36-160">*Services* folder: Contains the implementation of the `Greeter` service.</span></span>
* <span data-ttu-id="a0e36-161">plik *appSettings. json* &ndash; zawiera dane konfiguracyjne, takie jak protokół używany przez Kestrel.</span><span class="sxs-lookup"><span data-stu-id="a0e36-161">*appSettings.json* &ndash; Contains configuration data, such as protocol used by Kestrel.</span></span> <span data-ttu-id="a0e36-162">Aby uzyskać więcej informacji, zobacz <xref:fundamentals/configuration/index>.</span><span class="sxs-lookup"><span data-stu-id="a0e36-162">For more information, see <xref:fundamentals/configuration/index>.</span></span>
* <span data-ttu-id="a0e36-163">*Program.cs* &ndash; zawiera punkt wejścia dla usługi gRPC.</span><span class="sxs-lookup"><span data-stu-id="a0e36-163">*Program.cs* &ndash; Contains the entry point for the gRPC service.</span></span> <span data-ttu-id="a0e36-164">Aby uzyskać więcej informacji, zobacz <xref:fundamentals/host/generic-host>.</span><span class="sxs-lookup"><span data-stu-id="a0e36-164">For more information, see <xref:fundamentals/host/generic-host>.</span></span>
* <span data-ttu-id="a0e36-165">*Startup.cs* &ndash; zawiera kod, który konfiguruje zachowanie aplikacji.</span><span class="sxs-lookup"><span data-stu-id="a0e36-165">*Startup.cs* &ndash; Contains code that configures app behavior.</span></span> <span data-ttu-id="a0e36-166">Aby uzyskać więcej informacji, zobacz [Uruchamianie aplikacji](xref:fundamentals/startup).</span><span class="sxs-lookup"><span data-stu-id="a0e36-166">For more information, see [App startup](xref:fundamentals/startup).</span></span>

## <a name="create-the-grpc-client-in-a-net-console-app"></a><span data-ttu-id="a0e36-167">Tworzenie klienta gRPC w aplikacji konsolowej .NET</span><span class="sxs-lookup"><span data-stu-id="a0e36-167">Create the gRPC client in a .NET console app</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="a0e36-168">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="a0e36-168">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="a0e36-169">Otwórz drugie wystąpienie programu Visual Studio i wybierz pozycję **Utwórz nowy projekt**.</span><span class="sxs-lookup"><span data-stu-id="a0e36-169">Open a second instance of Visual Studio and select **Create a new project**.</span></span>
* <span data-ttu-id="a0e36-170">W oknie dialogowym **Tworzenie nowego projektu** wybierz pozycję **aplikacja konsoli (.NET Core)** , a następnie wybierz pozycję **dalej**.</span><span class="sxs-lookup"><span data-stu-id="a0e36-170">In the **Create a new project** dialog, select **Console App (.NET Core)** and select **Next**.</span></span>
* <span data-ttu-id="a0e36-171">W polu tekstowym **Nazwa** wprowadź **GrpcGreeterClient** , a następnie wybierz pozycję **Utwórz**.</span><span class="sxs-lookup"><span data-stu-id="a0e36-171">In the **Name** text box, enter **GrpcGreeterClient** and select **Create**.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="a0e36-172">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="a0e36-172">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="a0e36-173">Otwórz [zintegrowany terminal](https://code.visualstudio.com/docs/editor/integrated-terminal).</span><span class="sxs-lookup"><span data-stu-id="a0e36-173">Open the [integrated terminal](https://code.visualstudio.com/docs/editor/integrated-terminal).</span></span>
* <span data-ttu-id="a0e36-174">Zmień katalog (`cd`) do folderu, który będzie zawierać projekt.</span><span class="sxs-lookup"><span data-stu-id="a0e36-174">Change directories (`cd`) to a folder which will contain the project.</span></span>
* <span data-ttu-id="a0e36-175">Uruchom następujące polecenia:</span><span class="sxs-lookup"><span data-stu-id="a0e36-175">Run the following commands:</span></span>

  ```dotnetcli
  dotnet new console -o GrpcGreeterClient
  code -r GrpcGreeterClient
  ```

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="a0e36-176">program Visual Studio dla komputerów Mac</span><span class="sxs-lookup"><span data-stu-id="a0e36-176">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="a0e36-177">Postępuj zgodnie z instrukcjami w temacie [Tworzenie kompletnego rozwiązania .NET Core w systemie macOS przy użyciu Visual Studio dla komputerów Mac](/dotnet/core/tutorials/using-on-mac-vs-full-solution) , aby utworzyć aplikację konsolową o nazwie *GrpcGreeterClient*.</span><span class="sxs-lookup"><span data-stu-id="a0e36-177">Follow the instructions in [Building a complete .NET Core solution on macOS using Visual Studio for Mac](/dotnet/core/tutorials/using-on-mac-vs-full-solution) to create a console app with the name *GrpcGreeterClient*.</span></span>

---

### <a name="add-required-packages"></a><span data-ttu-id="a0e36-178">Dodaj wymagane pakiety</span><span class="sxs-lookup"><span data-stu-id="a0e36-178">Add required packages</span></span>

<span data-ttu-id="a0e36-179">Projekt klienta gRPC wymaga następujących pakietów:</span><span class="sxs-lookup"><span data-stu-id="a0e36-179">The gRPC client project requires the following packages:</span></span>

* <span data-ttu-id="a0e36-180">[GRPC .NET. Client](https://www.nuget.org/packages/Grpc.Net.Client), który zawiera klienta .NET Core.</span><span class="sxs-lookup"><span data-stu-id="a0e36-180">[Grpc.Net.Client](https://www.nuget.org/packages/Grpc.Net.Client), which contains the .NET Core client.</span></span>
* <span data-ttu-id="a0e36-181">[Google. protobuf](https://www.nuget.org/packages/Google.Protobuf/), który zawiera interfejsy API komunikatów protobuf C#dla.</span><span class="sxs-lookup"><span data-stu-id="a0e36-181">[Google.Protobuf](https://www.nuget.org/packages/Google.Protobuf/), which contains protobuf message APIs for C#.</span></span>
* <span data-ttu-id="a0e36-182">[GRPC. Tools](https://www.nuget.org/packages/Grpc.Tools/), która zawiera C# obsługę narzędzi dla plików protobuf.</span><span class="sxs-lookup"><span data-stu-id="a0e36-182">[Grpc.Tools](https://www.nuget.org/packages/Grpc.Tools/), which contains C# tooling support for protobuf files.</span></span> <span data-ttu-id="a0e36-183">Pakiet narzędzi nie jest wymagany w czasie wykonywania, więc zależność jest oznaczona za pomocą `PrivateAssets="All"`.</span><span class="sxs-lookup"><span data-stu-id="a0e36-183">The tooling package isn't required at runtime, so the dependency is marked with `PrivateAssets="All"`.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="a0e36-184">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="a0e36-184">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="a0e36-185">Zainstaluj pakiety przy użyciu konsoli Menedżera pakietów (PMC) lub Zarządzaj pakietami NuGet.</span><span class="sxs-lookup"><span data-stu-id="a0e36-185">Install the packages using either the Package Manager Console (PMC) or Manage NuGet Packages.</span></span>

#### <a name="pmc-option-to-install-packages"></a><span data-ttu-id="a0e36-186">Opcja PMC, aby zainstalować pakiety</span><span class="sxs-lookup"><span data-stu-id="a0e36-186">PMC option to install packages</span></span>

* <span data-ttu-id="a0e36-187">W programie Visual Studio wybierz kolejno pozycje **narzędzia**@no__t — 1**menedżer pakietów NuGet** > **konsola Menedżera pakietów**</span><span class="sxs-lookup"><span data-stu-id="a0e36-187">From Visual Studio, select **Tools** > **NuGet Package Manager** > **Package Manager Console**</span></span>
* <span data-ttu-id="a0e36-188">W oknie **konsola Menedżera pakietów** uruchom polecenie `cd GrpcGreeterClient`, aby zmienić katalogi na folder zawierający pliki *GrpcGreeterClient. csproj* .</span><span class="sxs-lookup"><span data-stu-id="a0e36-188">From the **Package Manager Console** window, run `cd GrpcGreeterClient` to change directories to the folder containing the *GrpcGreeterClient.csproj* files.</span></span>
* <span data-ttu-id="a0e36-189">Uruchom następujące polecenia:</span><span class="sxs-lookup"><span data-stu-id="a0e36-189">Run the following commands:</span></span>

  ```powershell
  Install-Package Grpc.Net.Client
  Install-Package Google.Protobuf
  Install-Package Grpc.Tools
  ```

#### <a name="manage-nuget-packages-option-to-install-packages"></a><span data-ttu-id="a0e36-190">Opcja zarządzania pakietami NuGet w celu zainstalowania pakietów</span><span class="sxs-lookup"><span data-stu-id="a0e36-190">Manage NuGet Packages option to install packages</span></span>

* <span data-ttu-id="a0e36-191">Kliknij prawym przyciskiem myszy projekt w **Eksplorator rozwiązań** > **Zarządzanie pakietami NuGet**</span><span class="sxs-lookup"><span data-stu-id="a0e36-191">Right-click the project in **Solution Explorer** > **Manage NuGet Packages**</span></span>
* <span data-ttu-id="a0e36-192">Wybierz kartę **Przeglądaj**.</span><span class="sxs-lookup"><span data-stu-id="a0e36-192">Select the **Browse** tab.</span></span>
* <span data-ttu-id="a0e36-193">W polu wyszukiwania wprowadź **GRPC .NET. Client** .</span><span class="sxs-lookup"><span data-stu-id="a0e36-193">Enter **Grpc.Net.Client** in the search box.</span></span>
* <span data-ttu-id="a0e36-194">Wybierz pakiet **GRPC .NET. Client** z karty **Przeglądaj** i wybierz pozycję **Zainstaluj**.</span><span class="sxs-lookup"><span data-stu-id="a0e36-194">Select the **Grpc.Net.Client** package from the **Browse** tab and select **Install**.</span></span>
* <span data-ttu-id="a0e36-195">Powtórz dla `Google.Protobuf` i `Grpc.Tools`.</span><span class="sxs-lookup"><span data-stu-id="a0e36-195">Repeat for `Google.Protobuf` and `Grpc.Tools`.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="a0e36-196">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="a0e36-196">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="a0e36-197">Uruchom następujące polecenia w **zintegrowanym terminalu**:</span><span class="sxs-lookup"><span data-stu-id="a0e36-197">Run the following commands from the **Integrated Terminal**:</span></span>

```dotnetcli
dotnet add GrpcGreeterClient.csproj package Grpc.Net.Client
dotnet add GrpcGreeterClient.csproj package Google.Protobuf
dotnet add GrpcGreeterClient.csproj package Grpc.Tools
```

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="a0e36-198">program Visual Studio dla komputerów Mac</span><span class="sxs-lookup"><span data-stu-id="a0e36-198">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="a0e36-199">Kliknij prawym przyciskiem myszy folder **pakiety** w **okienko rozwiązania** > **Dodaj pakiety**</span><span class="sxs-lookup"><span data-stu-id="a0e36-199">Right-click the **Packages** folder in **Solution Pad** > **Add Packages**</span></span>
* <span data-ttu-id="a0e36-200">W polu wyszukiwania wprowadź **GRPC .NET. Client** .</span><span class="sxs-lookup"><span data-stu-id="a0e36-200">Enter **Grpc.Net.Client** in the search box.</span></span>
* <span data-ttu-id="a0e36-201">Wybierz pakiet **GRPC .NET. Client** z okienka wyników, a następnie wybierz pozycję **Dodaj pakiet** .</span><span class="sxs-lookup"><span data-stu-id="a0e36-201">Select the **Grpc.Net.Client** package from the results pane and select **Add Package**</span></span>
* <span data-ttu-id="a0e36-202">Powtórz dla `Google.Protobuf` i `Grpc.Tools`.</span><span class="sxs-lookup"><span data-stu-id="a0e36-202">Repeat for `Google.Protobuf` and `Grpc.Tools`.</span></span>

---

### <a name="add-greetproto"></a><span data-ttu-id="a0e36-203">Dodaj Greeting. proto</span><span class="sxs-lookup"><span data-stu-id="a0e36-203">Add greet.proto</span></span>

* <span data-ttu-id="a0e36-204">Utwórz folder *Protos* w projekcie klienta gRPC.</span><span class="sxs-lookup"><span data-stu-id="a0e36-204">Create a *Protos* folder in the gRPC client project.</span></span>
* <span data-ttu-id="a0e36-205">Skopiuj plik *Protos\greet.proto* z usługi gRPC Greeter do projektu klienta gRPC.</span><span class="sxs-lookup"><span data-stu-id="a0e36-205">Copy the *Protos\greet.proto* file from the gRPC Greeter service to the gRPC client project.</span></span>
* <span data-ttu-id="a0e36-206">Edytuj plik projektu *GrpcGreeterClient. csproj* :</span><span class="sxs-lookup"><span data-stu-id="a0e36-206">Edit the *GrpcGreeterClient.csproj* project file:</span></span>

  # <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="a0e36-207">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="a0e36-207">Visual Studio</span></span>](#tab/visual-studio)

  <span data-ttu-id="a0e36-208">Kliknij prawym przyciskiem myszy projekt i wybierz polecenie **Edytuj plik projektu**.</span><span class="sxs-lookup"><span data-stu-id="a0e36-208">Right-click the project and select **Edit Project File**.</span></span>

  # <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="a0e36-209">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="a0e36-209">Visual Studio Code</span></span>](#tab/visual-studio-code)

  <span data-ttu-id="a0e36-210">Wybierz plik *GrpcGreeterClient. csproj* .</span><span class="sxs-lookup"><span data-stu-id="a0e36-210">Select the *GrpcGreeterClient.csproj* file.</span></span>

  # <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="a0e36-211">program Visual Studio dla komputerów Mac</span><span class="sxs-lookup"><span data-stu-id="a0e36-211">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

  <span data-ttu-id="a0e36-212">Kliknij prawym przyciskiem myszy projekt i wybierz polecenie **narzędzia** > **Edytuj plik**.</span><span class="sxs-lookup"><span data-stu-id="a0e36-212">Right-click the project and select **Tools** > **Edit File**.</span></span>

  ---

* <span data-ttu-id="a0e36-213">Dodaj grupę elementów z elementem `<Protobuf>`, który odwołuje się do pliku *Greeting. proto* :</span><span class="sxs-lookup"><span data-stu-id="a0e36-213">Add an item group with a `<Protobuf>` element that refers to the *greet.proto* file:</span></span>

  ```xml
  <ItemGroup>
    <Protobuf Include="Protos\greet.proto" GrpcServices="Client" />
  </ItemGroup>
  ```

### <a name="create-the-greeter-client"></a><span data-ttu-id="a0e36-214">Tworzenie klienta Greeter</span><span class="sxs-lookup"><span data-stu-id="a0e36-214">Create the Greeter client</span></span>

<span data-ttu-id="a0e36-215">Skompiluj projekt, aby utworzyć typy w przestrzeni nazw `GrpcGreeter`.</span><span class="sxs-lookup"><span data-stu-id="a0e36-215">Build the project to create the types in the `GrpcGreeter` namespace.</span></span> <span data-ttu-id="a0e36-216">Typy `GrpcGreeter` są generowane automatycznie przez proces kompilacji.</span><span class="sxs-lookup"><span data-stu-id="a0e36-216">The `GrpcGreeter` types are generated automatically by the build process.</span></span>

<span data-ttu-id="a0e36-217">Zaktualizuj plik *program.cs* klienta gRPC za pomocą następującego kodu:</span><span class="sxs-lookup"><span data-stu-id="a0e36-217">Update the gRPC client *Program.cs* file with the following code:</span></span>

[!code-csharp[](~/tutorials/grpc/grpc-start/sample/GrpcGreeterClient/Program.cs?name=snippet2)]

<span data-ttu-id="a0e36-218">*Program.cs* zawiera punkt wejścia i logikę dla klienta gRPC.</span><span class="sxs-lookup"><span data-stu-id="a0e36-218">*Program.cs* contains the entry point and logic for the gRPC client.</span></span>

<span data-ttu-id="a0e36-219">Klient Greeter jest tworzony przez:</span><span class="sxs-lookup"><span data-stu-id="a0e36-219">The Greeter client is created by:</span></span>

* <span data-ttu-id="a0e36-220">Utworzenie wystąpienia `HttpClient` zawierającego informacje służące do tworzenia połączenia z usługą gRPC.</span><span class="sxs-lookup"><span data-stu-id="a0e36-220">Instantiating an `HttpClient` containing the information for creating the connection to the gRPC service.</span></span>
* <span data-ttu-id="a0e36-221">Użycie `HttpClient` w celu skonstruowania kanału gRPC i klienta Greeter:</span><span class="sxs-lookup"><span data-stu-id="a0e36-221">Using the `HttpClient` to construct a gRPC channel and the Greeter client:</span></span>

[!code-csharp[](~/tutorials/grpc/grpc-start/sample/GrpcGreeterClient/Program.cs?name=snippet&highlight=3-5)]

<span data-ttu-id="a0e36-222">Klient Greeter wywołuje metodę asynchroniczną `SayHello`.</span><span class="sxs-lookup"><span data-stu-id="a0e36-222">The Greeter client calls the asynchronous `SayHello` method.</span></span> <span data-ttu-id="a0e36-223">Zostanie wyświetlony wynik wywołania `SayHello`:</span><span class="sxs-lookup"><span data-stu-id="a0e36-223">The result of the `SayHello` call is displayed:</span></span>

[!code-csharp[](~/tutorials/grpc/grpc-start/sample/GrpcGreeterClient/Program.cs?name=snippet&highlight=6-8)]

## <a name="test-the-grpc-client-with-the-grpc-greeter-service"></a><span data-ttu-id="a0e36-224">Testowanie klienta gRPC za pomocą usługi gRPC Greeter</span><span class="sxs-lookup"><span data-stu-id="a0e36-224">Test the gRPC client with the gRPC Greeter service</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="a0e36-225">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="a0e36-225">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="a0e36-226">W usłudze Greeter naciśnij `Ctrl+F5`, aby uruchomić serwer bez debugera.</span><span class="sxs-lookup"><span data-stu-id="a0e36-226">In the Greeter service, press `Ctrl+F5` to start the server without the debugger.</span></span>
* <span data-ttu-id="a0e36-227">W projekcie `GrpcGreeterClient` naciśnij klawisz `Ctrl+F5`, aby uruchomić klienta bez debugera.</span><span class="sxs-lookup"><span data-stu-id="a0e36-227">In the `GrpcGreeterClient` project, press `Ctrl+F5` to start the client without the debugger.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="a0e36-228">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="a0e36-228">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="a0e36-229">Uruchom usługę Greeter.</span><span class="sxs-lookup"><span data-stu-id="a0e36-229">Start the Greeter service.</span></span>
* <span data-ttu-id="a0e36-230">Uruchom klienta programu.</span><span class="sxs-lookup"><span data-stu-id="a0e36-230">Start the client.</span></span>


# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="a0e36-231">program Visual Studio dla komputerów Mac</span><span class="sxs-lookup"><span data-stu-id="a0e36-231">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="a0e36-232">Uruchom usługę Greeter.</span><span class="sxs-lookup"><span data-stu-id="a0e36-232">Start the Greeter service.</span></span>
* <span data-ttu-id="a0e36-233">Uruchom klienta programu.</span><span class="sxs-lookup"><span data-stu-id="a0e36-233">Start the client.</span></span>

---

<span data-ttu-id="a0e36-234">Klient wysyła do usługi powitanie z komunikatem zawierającym nazwę *GreeterClient*.</span><span class="sxs-lookup"><span data-stu-id="a0e36-234">The client sends a greeting to the service with a message containing its name, *GreeterClient*.</span></span> <span data-ttu-id="a0e36-235">Usługa wysyła komunikat "Hello GreeterClient" jako odpowiedź.</span><span class="sxs-lookup"><span data-stu-id="a0e36-235">The service sends the message "Hello GreeterClient" as a response.</span></span> <span data-ttu-id="a0e36-236">Odpowiedź "Hello GreeterClient" jest wyświetlana w wierszu polecenia:</span><span class="sxs-lookup"><span data-stu-id="a0e36-236">The "Hello GreeterClient" response is displayed in the command prompt:</span></span>

```console
Greeting: Hello GreeterClient
Press any key to exit...
```

<span data-ttu-id="a0e36-237">Usługa gRPC rejestruje szczegóły pomyślnego wywołania w dziennikach, które zostały zapisane w wierszu polecenia:</span><span class="sxs-lookup"><span data-stu-id="a0e36-237">The gRPC service records the details of the successful call in the logs written to the command prompt:</span></span>

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
> <span data-ttu-id="a0e36-238">Kod w tym artykule wymaga certyfikatu programistycznego HTTPS ASP.NET Core do zabezpieczenia usługi gRPC.</span><span class="sxs-lookup"><span data-stu-id="a0e36-238">The code in this article requires the ASP.NET Core HTTPS development certificate to secure the gRPC service.</span></span> <span data-ttu-id="a0e36-239">Jeśli klient kończy się niepowodzeniem z komunikatem `The remote certificate is invalid according to the validation procedure.`, certyfikat programistyczny nie jest zaufany.</span><span class="sxs-lookup"><span data-stu-id="a0e36-239">If the client fails with the message `The remote certificate is invalid according to the validation procedure.`, the development certificate is not trusted.</span></span> <span data-ttu-id="a0e36-240">Aby uzyskać instrukcje dotyczące rozwiązania tego problemu, zobacz temat [ASP.NET Core ufanie certyfikatowi Deweloperskiemu protokołu HTTPS w systemie Windows i macOS](xref:security/enforcing-ssl#trust-the-aspnet-core-https-development-certificate-on-windows-and-macos).</span><span class="sxs-lookup"><span data-stu-id="a0e36-240">For instructions to fix this issue, see [Trust the ASP.NET Core HTTPS development certificate on Windows and macOS](xref:security/enforcing-ssl#trust-the-aspnet-core-https-development-certificate-on-windows-and-macos).</span></span>

[!INCLUDE[](~/includes/gRPCazure.md)]

### <a name="next-steps"></a><span data-ttu-id="a0e36-241">Następne kroki</span><span class="sxs-lookup"><span data-stu-id="a0e36-241">Next steps</span></span>

* <xref:grpc/index>
* <xref:grpc/basics>
* <xref:grpc/migration>
