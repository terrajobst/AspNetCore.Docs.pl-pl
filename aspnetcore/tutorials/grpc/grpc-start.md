---
title: Tworzenie klienta i serwera platformy .NET Core gRPC w ASP.NET Core
author: juntaoluo
description: W tym samouczku pokazano, jak utworzyć usługę gRPC i klienta gRPC na ASP.NET Core. Dowiedz się, jak utworzyć projekt usługi gRPC, edytować plik PROTO i dodać wywołanie przesyłania strumieniowego dupleksu.
monikerRange: '>= aspnetcore-3.0'
ms.author: johluo
ms.date: 06/12/2019
uid: tutorials/grpc/grpc-start
ms.openlocfilehash: 3e90e3b17186757fe157fb6641888786bb7a0df2
ms.sourcegitcommit: f30b18442ed12831c7e86b0db249183ccd749f59
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/23/2019
ms.locfileid: "68412523"
---
# <a name="tutorial-create-a-grpc-client-and-server-in-aspnet-core"></a><span data-ttu-id="2d542-104">Samouczek: Utwórz klienta i serwer gRPC w ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="2d542-104">Tutorial: Create a gRPC client and server in ASP.NET Core</span></span>

<span data-ttu-id="2d542-105">Przez [Jan Luo](https://github.com/juntaoluo)</span><span class="sxs-lookup"><span data-stu-id="2d542-105">By [John Luo](https://github.com/juntaoluo)</span></span>

<span data-ttu-id="2d542-106">W tym samouczku pokazano, jak utworzyć klienta programu .NET Core [gRPC](https://grpc.io/docs/guides/) oraz serwer gRPC ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="2d542-106">This tutorial shows how to create a .NET Core [gRPC](https://grpc.io/docs/guides/) client and an ASP.NET Core gRPC Server.</span></span>

<span data-ttu-id="2d542-107">Na koniec będziesz mieć klienta gRPC, który komunikuje się z usługą gRPC Greeter.</span><span class="sxs-lookup"><span data-stu-id="2d542-107">At the end, you'll have a gRPC client that communicates with the gRPC Greeter service.</span></span>

<span data-ttu-id="2d542-108">[Wyświetlanie lub Pobieranie przykładowego kodu](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/tutorials/grpc/grpc-start/sample) ([jak pobrać](xref:index#how-to-download-a-sample)).</span><span class="sxs-lookup"><span data-stu-id="2d542-108">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/tutorials/grpc/grpc-start/sample) ([how to download](xref:index#how-to-download-a-sample)).</span></span>

<span data-ttu-id="2d542-109">W tym samouczku przedstawiono następujące instrukcje:</span><span class="sxs-lookup"><span data-stu-id="2d542-109">In this tutorial, you:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="2d542-110">Utwórz serwer gRPC.</span><span class="sxs-lookup"><span data-stu-id="2d542-110">Create a gRPC Server.</span></span>
> * <span data-ttu-id="2d542-111">Utwórz klienta gRPC.</span><span class="sxs-lookup"><span data-stu-id="2d542-111">Create a gRPC client.</span></span>
> * <span data-ttu-id="2d542-112">Przetestuj usługę klienta gRPC za pomocą usługi gRPC Greeter.</span><span class="sxs-lookup"><span data-stu-id="2d542-112">Test the gRPC client service with the gRPC Greeter service.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="2d542-113">Wymagania wstępne</span><span class="sxs-lookup"><span data-stu-id="2d542-113">Prerequisites</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="2d542-114">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="2d542-114">Visual Studio</span></span>](#tab/visual-studio)

[!INCLUDE[](~/includes/net-core-prereqs-vs-3.0.md)]

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="2d542-115">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="2d542-115">Visual Studio Code</span></span>](#tab/visual-studio-code)

[!INCLUDE[](~/includes/net-core-prereqs-vsc-3.0.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="2d542-116">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="2d542-116">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

[!INCLUDE[](~/includes/net-core-prereqs-mac-3.0.md)]

---

## <a name="create-a-grpc-service"></a><span data-ttu-id="2d542-117">Tworzenie usługi gRPC</span><span class="sxs-lookup"><span data-stu-id="2d542-117">Create a gRPC service</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="2d542-118">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="2d542-118">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="2d542-119">W programie Visual Studio **pliku** menu, wybierz opcję **New** > **projektu**.</span><span class="sxs-lookup"><span data-stu-id="2d542-119">From the Visual Studio **File** menu, select **New** > **Project**.</span></span>
* <span data-ttu-id="2d542-120">W oknie dialogowym **Tworzenie nowego projektu** wybierz pozycję **ASP.NET Core aplikacji sieci Web**.</span><span class="sxs-lookup"><span data-stu-id="2d542-120">In the **Create a new project** dialog, select **ASP.NET Core Web Application**.</span></span>
* <span data-ttu-id="2d542-121">Wybierz pozycję **dalej**</span><span class="sxs-lookup"><span data-stu-id="2d542-121">Select **Next**</span></span>
* <span data-ttu-id="2d542-122">Nazwij projekt **GrpcGreeter**.</span><span class="sxs-lookup"><span data-stu-id="2d542-122">Name the project **GrpcGreeter**.</span></span> <span data-ttu-id="2d542-123">Ważne jest, aby nazwa projektu *GrpcGreeter* , tak aby przestrzenie nazw były zgodne podczas kopiowania i wklejania kodu.</span><span class="sxs-lookup"><span data-stu-id="2d542-123">It's important to name the project *GrpcGreeter* so the namespaces will match when you copy and paste code.</span></span>
* <span data-ttu-id="2d542-124">Wybierz pozycję **Utwórz**</span><span class="sxs-lookup"><span data-stu-id="2d542-124">Select **Create**</span></span>
* <span data-ttu-id="2d542-125">W oknie dialogowym **Tworzenie nowej ASP.NET Core aplikacji sieci Web** :</span><span class="sxs-lookup"><span data-stu-id="2d542-125">In the **Create a new ASP.NET Core Web Application** dialog:</span></span>
  * <span data-ttu-id="2d542-126">W menu rozwijanym wybierz pozycję **.NET Core** i **ASP.NET Core 3,0** .</span><span class="sxs-lookup"><span data-stu-id="2d542-126">Select **.NET Core** and **ASP.NET Core 3.0** in the dropdown menus.</span></span> 
  * <span data-ttu-id="2d542-127">Wybierz szablon **usługi gRPC** .</span><span class="sxs-lookup"><span data-stu-id="2d542-127">Select the **gRPC Service** template.</span></span>
  * <span data-ttu-id="2d542-128">Wybierz pozycję **Utwórz**</span><span class="sxs-lookup"><span data-stu-id="2d542-128">Select **Create**</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="2d542-129">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="2d542-129">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="2d542-130">Otwórz [zintegrowany terminal](https://code.visualstudio.com/docs/editor/integrated-terminal).</span><span class="sxs-lookup"><span data-stu-id="2d542-130">Open the [integrated terminal](https://code.visualstudio.com/docs/editor/integrated-terminal).</span></span>
* <span data-ttu-id="2d542-131">Zmień katalogi (`cd`) na folder, który będzie zawierać projekt.</span><span class="sxs-lookup"><span data-stu-id="2d542-131">Change directories (`cd`) to a folder which will contain the project.</span></span>
* <span data-ttu-id="2d542-132">Uruchom następujące polecenia:</span><span class="sxs-lookup"><span data-stu-id="2d542-132">Run the following commands:</span></span>

  ```console
  dotnet new grpc -o GrpcGreeter
  code -r GrpcGreeter
  ```

  * <span data-ttu-id="2d542-133">Polecenie tworzy nową usługę gRPC w folderze GrpcGreeter.  `dotnet new`</span><span class="sxs-lookup"><span data-stu-id="2d542-133">The `dotnet new` command creates a new gRPC service in the *GrpcGreeter* folder.</span></span>
  * <span data-ttu-id="2d542-134">Polecenie otwiera folder GrpcGreeter w nowym wystąpieniu Visual Studio Code.  `code`</span><span class="sxs-lookup"><span data-stu-id="2d542-134">The `code` command opens the *GrpcGreeter* folder in a new instance of Visual Studio Code.</span></span>

  <span data-ttu-id="2d542-135">Zostanie wyświetlone okno dialogowe z **wymaganymi zasobami do kompilowania i debugowania brakuje w "GrpcGreeter". Dodać je?**</span><span class="sxs-lookup"><span data-stu-id="2d542-135">A dialog box appears with **Required assets to build and debug are missing from 'GrpcGreeter'. Add them?**</span></span>
* <span data-ttu-id="2d542-136">Wybierz opcję **tak**</span><span class="sxs-lookup"><span data-stu-id="2d542-136">Select **Yes**</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="2d542-137">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="2d542-137">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="2d542-138">W terminalu uruchom następujące polecenia:</span><span class="sxs-lookup"><span data-stu-id="2d542-138">From a terminal, run the following commands:</span></span>

```console
  dotnet new grpc -o GrpcGreeter
  cd GrpcGreeter
```

<span data-ttu-id="2d542-139">Poprzednie polecenia używają [interfejs wiersza polecenia platformy .NET Core](/dotnet/core/tools/dotnet) do tworzenia usługi gRPC.</span><span class="sxs-lookup"><span data-stu-id="2d542-139">The preceding commands use the [.NET Core CLI](/dotnet/core/tools/dotnet) to create a gRPC service.</span></span>

### <a name="open-the-project"></a><span data-ttu-id="2d542-140">Otwórz projekt</span><span class="sxs-lookup"><span data-stu-id="2d542-140">Open the project</span></span>

<span data-ttu-id="2d542-141">W programie Visual Studio wybierz pozycję **plik > Otwórz**, a następnie wybierz plik *GrpcGreeter. sln* .</span><span class="sxs-lookup"><span data-stu-id="2d542-141">From Visual Studio, select **File > Open**, and then select the *GrpcGreeter.sln* file.</span></span>

<!-- End of VS tabs -->

---

### <a name="run-the-service"></a><span data-ttu-id="2d542-142">Uruchamianie usługi</span><span class="sxs-lookup"><span data-stu-id="2d542-142">Run the service</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="2d542-143">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="2d542-143">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="2d542-144">Naciśnij `Ctrl+F5` klawisz, aby uruchomić usługę gRPC bez debugera.</span><span class="sxs-lookup"><span data-stu-id="2d542-144">Press `Ctrl+F5` to run the gRPC service without the debugger.</span></span>

  <span data-ttu-id="2d542-145">Program Visual Studio uruchamia usługę w wierszu polecenia.</span><span class="sxs-lookup"><span data-stu-id="2d542-145">Visual Studio runs the service in a command prompt.</span></span>

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[<span data-ttu-id="2d542-146">Visual Studio Code/Visual Studio dla komputerów Mac</span><span class="sxs-lookup"><span data-stu-id="2d542-146">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

* <span data-ttu-id="2d542-147">Uruchom gRPC Greeter projektu *GrpcGreeter* z wiersza polecenia przy użyciu `dotnet run`.</span><span class="sxs-lookup"><span data-stu-id="2d542-147">Run the gRPC Greeter project *GrpcGreeter* from the command line using `dotnet run`.</span></span>

<!-- End of combined VS/Mac tabs -->

---

<span data-ttu-id="2d542-148">Dzienniki pokazują, że usługa nasłuchuje `https://localhost:5001`.</span><span class="sxs-lookup"><span data-stu-id="2d542-148">The logs show the service listening on `https://localhost:5001`.</span></span>

```console
info: Microsoft.Hosting.Lifetime[0]
      Now listening on: https://localhost:5001
info: Microsoft.Hosting.Lifetime[0]
      Application started. Press Ctrl+C to shut down.
info: Microsoft.Hosting.Lifetime[0]
      Hosting environment: Development
info: Microsoft.Hosting.Lifetime[0]
```

### <a name="examine-the-project-files"></a><span data-ttu-id="2d542-149">Sprawdzanie plików projektu</span><span class="sxs-lookup"><span data-stu-id="2d542-149">Examine the project files</span></span>

<span data-ttu-id="2d542-150">*GrpcGreeter* pliki projektu:</span><span class="sxs-lookup"><span data-stu-id="2d542-150">*GrpcGreeter* project files:</span></span>

* <span data-ttu-id="2d542-151">*Greeting. proto*: Plik *Protos/Greeting. proto* definiuje `Greeter` gRPC i służy do generowania zasobów serwera gRPC.</span><span class="sxs-lookup"><span data-stu-id="2d542-151">*greet.proto*: The *Protos/greet.proto* file defines the `Greeter` gRPC and is used to generate the gRPC server assets.</span></span> <span data-ttu-id="2d542-152">Aby uzyskać więcej informacji, zobacz [wprowadzenie do gRPC](xref:grpc/index).</span><span class="sxs-lookup"><span data-stu-id="2d542-152">For more information, see [Introduction to gRPC](xref:grpc/index).</span></span>
* <span data-ttu-id="2d542-153">Folder *usługi* : Zawiera implementację `Greeter` usługi.</span><span class="sxs-lookup"><span data-stu-id="2d542-153">*Services* folder: Contains the implementation of the `Greeter` service.</span></span>
* <span data-ttu-id="2d542-154">*appSettings.json*: Zawiera dane konfiguracyjne, takie jak protokół używany przez Kestrel.</span><span class="sxs-lookup"><span data-stu-id="2d542-154">*appSettings.json*: Contains configuration data, such as protocol used by Kestrel.</span></span> <span data-ttu-id="2d542-155">Aby uzyskać więcej informacji, zobacz <xref:fundamentals/configuration/index>.</span><span class="sxs-lookup"><span data-stu-id="2d542-155">For more information, see <xref:fundamentals/configuration/index>.</span></span>
* <span data-ttu-id="2d542-156">*Program.cs*: Zawiera punkt wejścia dla usługi gRPC.</span><span class="sxs-lookup"><span data-stu-id="2d542-156">*Program.cs*: Contains the entry point for the gRPC service.</span></span> <span data-ttu-id="2d542-157">Aby uzyskać więcej informacji, zobacz <xref:fundamentals/host/generic-host>.</span><span class="sxs-lookup"><span data-stu-id="2d542-157">For more information, see <xref:fundamentals/host/generic-host>.</span></span>
* <span data-ttu-id="2d542-158">*Startup.cs*: Zawiera kod, który konfiguruje zachowanie aplikacji.</span><span class="sxs-lookup"><span data-stu-id="2d542-158">*Startup.cs*: Contains code that configures app behavior.</span></span> <span data-ttu-id="2d542-159">Aby uzyskać więcej informacji, zobacz [Uruchamianie aplikacji](xref:fundamentals/startup).</span><span class="sxs-lookup"><span data-stu-id="2d542-159">For more information, see [App startup](xref:fundamentals/startup).</span></span>

## <a name="create-the-grpc-client-in-a-net-console-app"></a><span data-ttu-id="2d542-160">Tworzenie klienta gRPC w aplikacji konsolowej .NET</span><span class="sxs-lookup"><span data-stu-id="2d542-160">Create the gRPC client in a .NET console app</span></span>

## <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="2d542-161">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="2d542-161">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="2d542-162">Otwórz drugie wystąpienie programu Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="2d542-162">Open a second instance of Visual Studio.</span></span>
* <span data-ttu-id="2d542-163">Na pasku menu wybierz pozycję **plik** > **Nowy** > **projekt** .</span><span class="sxs-lookup"><span data-stu-id="2d542-163">Select **File** > **New** > **Project** from the menu bar.</span></span>
* <span data-ttu-id="2d542-164">W oknie dialogowym **Tworzenie nowego projektu** wybierz pozycję **Aplikacja konsolowa (.NET Core)** .</span><span class="sxs-lookup"><span data-stu-id="2d542-164">In the **Create a new project** dialog, select **Console App (.NET Core)**.</span></span>
* <span data-ttu-id="2d542-165">Wybierz pozycję **dalej**</span><span class="sxs-lookup"><span data-stu-id="2d542-165">Select **Next**</span></span>
* <span data-ttu-id="2d542-166">W polu tekstowym **Nazwa** wprowadź wartość "GrpcGreeterClient".</span><span class="sxs-lookup"><span data-stu-id="2d542-166">In the **Name** text box, enter "GrpcGreeterClient".</span></span>
* <span data-ttu-id="2d542-167">Wybierz pozycję **Utwórz**.</span><span class="sxs-lookup"><span data-stu-id="2d542-167">Select **Create**.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="2d542-168">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="2d542-168">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="2d542-169">Otwórz [zintegrowany terminal](https://code.visualstudio.com/docs/editor/integrated-terminal).</span><span class="sxs-lookup"><span data-stu-id="2d542-169">Open the [integrated terminal](https://code.visualstudio.com/docs/editor/integrated-terminal).</span></span>
* <span data-ttu-id="2d542-170">Zmień katalogi (`cd`) na folder, który będzie zawierać projekt.</span><span class="sxs-lookup"><span data-stu-id="2d542-170">Change directories (`cd`) to a folder which will contain the project.</span></span>
* <span data-ttu-id="2d542-171">Uruchom następujące polecenia:</span><span class="sxs-lookup"><span data-stu-id="2d542-171">Run the following commands:</span></span>

```console
dotnet new console -o GrpcGreeterClient
code -r GrpcGreeterClient
```

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="2d542-172">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="2d542-172">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="2d542-173">Postępuj zgodnie z instrukcjami w [tym miejscu](/dotnet/core/tutorials/using-on-mac-vs-full-solution) , aby utworzyć aplikację konsolową o nazwie *GrpcGreeterClient*.</span><span class="sxs-lookup"><span data-stu-id="2d542-173">Follow the instructions [here](/dotnet/core/tutorials/using-on-mac-vs-full-solution) to create a console app with the name *GrpcGreeterClient*.</span></span>

<!-- End of VS tabs -->

---

### <a name="add-required-packages"></a><span data-ttu-id="2d542-174">Dodaj wymagane pakiety</span><span class="sxs-lookup"><span data-stu-id="2d542-174">Add required packages</span></span>

<span data-ttu-id="2d542-175">Projekt klienta gRPC wymaga następujących pakietów:</span><span class="sxs-lookup"><span data-stu-id="2d542-175">The gRPC client project requires the following packages:</span></span>

* <span data-ttu-id="2d542-176">[GRPC .NET. Client](https://www.nuget.org/packages/Grpc.Net.Client), który zawiera klienta .NET Core.</span><span class="sxs-lookup"><span data-stu-id="2d542-176">[Grpc.Net.Client](https://www.nuget.org/packages/Grpc.Net.Client), which contains the .NET Core client.</span></span>
* <span data-ttu-id="2d542-177">[Google. protobuf](https://www.nuget.org/packages/Google.Protobuf/), który zawiera interfejsy API komunikatów protobuf C#dla.</span><span class="sxs-lookup"><span data-stu-id="2d542-177">[Google.Protobuf](https://www.nuget.org/packages/Google.Protobuf/), which contains protobuf message APIs for C#.</span></span>
* <span data-ttu-id="2d542-178">[GRPC. Tools](https://www.nuget.org/packages/Grpc.Tools/), która zawiera C# obsługę narzędzi dla plików protobuf.</span><span class="sxs-lookup"><span data-stu-id="2d542-178">[Grpc.Tools](https://www.nuget.org/packages/Grpc.Tools/), which contains C# tooling support for protobuf files.</span></span> <span data-ttu-id="2d542-179">Pakiet narzędzi nie jest wymagany w czasie wykonywania, dlatego zależność jest oznaczona za pomocą `PrivateAssets="All"`.</span><span class="sxs-lookup"><span data-stu-id="2d542-179">The tooling package isn't required at runtime, so the dependency is marked with `PrivateAssets="All"`.</span></span>

### <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="2d542-180">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="2d542-180">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="2d542-181">Zainstaluj pakiety przy użyciu konsoli Menedżera pakietów (PMC) lub Zarządzaj pakietami NuGet.</span><span class="sxs-lookup"><span data-stu-id="2d542-181">Install the packages using either the Package Manager Console (PMC) or Manage NuGet Packages.</span></span>

#### <a name="pmc-option-to-install-packages"></a><span data-ttu-id="2d542-182">Opcja PMC, aby zainstalować pakiety</span><span class="sxs-lookup"><span data-stu-id="2d542-182">PMC option to install packages</span></span>

* <span data-ttu-id="2d542-183">W programie Visual Studio wybierz kolejno pozycje **Narzędzia** > Menedżer**pakietów** > NuGet**konsola Menedżera pakietów**</span><span class="sxs-lookup"><span data-stu-id="2d542-183">From Visual Studio, select **Tools** > **NuGet Package Manager** > **Package Manager Console**</span></span>
* <span data-ttu-id="2d542-184">W oknie **konsola Menedżera pakietów** przejdź do katalogu, w którym znajduje się plik *GrpcGreeterClient. csproj* .</span><span class="sxs-lookup"><span data-stu-id="2d542-184">From the **Package Manager Console** window, navigate to the directory in which the *GrpcGreeterClient.csproj* file exists.</span></span>
* <span data-ttu-id="2d542-185">Uruchom następujące polecenia:</span><span class="sxs-lookup"><span data-stu-id="2d542-185">Run the following commands:</span></span>

 ```powershell
Install-Package Grpc.Net.Client
Install-Package Google.Protobuf
Install-Package Grpc.Tools
```

#### <a name="manage-nuget-packages-option-to-install-packages"></a><span data-ttu-id="2d542-186">Opcja zarządzania pakietami NuGet w celu zainstalowania pakietów</span><span class="sxs-lookup"><span data-stu-id="2d542-186">Manage NuGet Packages option to install packages</span></span>

* <span data-ttu-id="2d542-187">Kliknij prawym przyciskiem myszy projekt w **Eksplorator rozwiązań** > **Zarządzanie pakietami NuGet**</span><span class="sxs-lookup"><span data-stu-id="2d542-187">Right-click the project in **Solution Explorer** > **Manage NuGet Packages**</span></span>
* <span data-ttu-id="2d542-188">Wybierz kartę **przeglądanie** .</span><span class="sxs-lookup"><span data-stu-id="2d542-188">Select the **Browse** tab.</span></span>
* <span data-ttu-id="2d542-189">W polu wyszukiwania wprowadź **GRPC. Core** .</span><span class="sxs-lookup"><span data-stu-id="2d542-189">Enter **Grpc.Core** in the search box.</span></span>
* <span data-ttu-id="2d542-190">Wybierz pakiet **GRPC. Core** z karty **Przeglądaj** i wybierz pozycję **Zainstaluj**.</span><span class="sxs-lookup"><span data-stu-id="2d542-190">Select the **Grpc.Core** package from the **Browse** tab and select **Install**.</span></span>
* <span data-ttu-id="2d542-191">Powtórz dla `Google.Protobuf` i `Grpc.Tools`.</span><span class="sxs-lookup"><span data-stu-id="2d542-191">Repeat for `Google.Protobuf` and `Grpc.Tools`.</span></span>

### <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="2d542-192">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="2d542-192">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="2d542-193">Uruchom następujące polecenia w zintegrowanym **terminalu**:</span><span class="sxs-lookup"><span data-stu-id="2d542-193">Run the following commands from the **Integrated Terminal**:</span></span>

```console
dotnet add GrpcGreeterClient.csproj package Grpc.Net.Client
dotnet add GrpcGreeterClient.csproj package Google.Protobuf
dotnet add GrpcGreeterClient.csproj package Grpc.Tools
```

### <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="2d542-194">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="2d542-194">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="2d542-195">Kliknij prawym przyciskiem myszy folder **pakiety** w **okienko rozwiązania** > **Dodaj pakiety**</span><span class="sxs-lookup"><span data-stu-id="2d542-195">Right-click the **Packages** folder in **Solution Pad** > **Add Packages**</span></span>
* <span data-ttu-id="2d542-196">W polu wyszukiwania wprowadź **GRPC .NET. Client** .</span><span class="sxs-lookup"><span data-stu-id="2d542-196">Enter **Grpc.Net.Client** in the search box.</span></span>
* <span data-ttu-id="2d542-197">Wybierz pakiet **GRPC .NET. Client** z okienka wyników, a następnie wybierz pozycję **Dodaj pakiet** .</span><span class="sxs-lookup"><span data-stu-id="2d542-197">Select the **Grpc.Net.Client** package from the results pane and select **Add Package**</span></span>
* <span data-ttu-id="2d542-198">Powtórz dla `Google.Protobuf` i `Grpc.Tools`.</span><span class="sxs-lookup"><span data-stu-id="2d542-198">Repeat for `Google.Protobuf` and `Grpc.Tools`.</span></span>

---

### <a name="add-greetproto"></a><span data-ttu-id="2d542-199">Dodaj Greeting. proto</span><span class="sxs-lookup"><span data-stu-id="2d542-199">Add greet.proto</span></span>

* <span data-ttu-id="2d542-200">Utwórz folder **Protos** w projekcie klienta gRPC.</span><span class="sxs-lookup"><span data-stu-id="2d542-200">Create a **Protos** folder in the gRPC client project.</span></span>
* <span data-ttu-id="2d542-201">Skopiuj plik **Protos\greet.proto** z usługi gRPC Greeter do projektu klienta gRPC.</span><span class="sxs-lookup"><span data-stu-id="2d542-201">Copy the **Protos\greet.proto** file from the gRPC Greeter service to the gRPC client project.</span></span>
* <span data-ttu-id="2d542-202">Edytuj plik projektu *GrpcGreeterClient. csproj* :</span><span class="sxs-lookup"><span data-stu-id="2d542-202">Edit the *GrpcGreeterClient.csproj* project file:</span></span>

  # <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="2d542-203">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="2d542-203">Visual Studio</span></span>](#tab/visual-studio) 

  <span data-ttu-id="2d542-204">Kliknij prawym przyciskiem myszy projekt i wybierz polecenie **Edytuj plik projektu**.</span><span class="sxs-lookup"><span data-stu-id="2d542-204">Right-click the project and select **Edit Project File**.</span></span>

  # <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="2d542-205">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="2d542-205">Visual Studio Code</span></span>](#tab/visual-studio-code) 

  <span data-ttu-id="2d542-206">Wybierz plik *GrpcGreeterClient. csproj* .</span><span class="sxs-lookup"><span data-stu-id="2d542-206">Select the *GrpcGreeterClient.csproj* file.</span></span>

  # <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="2d542-207">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="2d542-207">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

  <span data-ttu-id="2d542-208">Kliknij prawym przyciskiem myszy projekt, a następnie wybierz polecenie **narzędzia > Edytuj plik**.</span><span class="sxs-lookup"><span data-stu-id="2d542-208">Right-click the project and select **Tools > Edit File**.</span></span>

  ---

* <span data-ttu-id="2d542-209">Dodaj grupę elementów z `<Protobuf>` elementem, który odwołuje się do pliku **Greeting. proto** :</span><span class="sxs-lookup"><span data-stu-id="2d542-209">Add an item group with a `<Protobuf>` element that refers to the **greet.proto** file:</span></span>

  ```XML
  <ItemGroup>
    <Protobuf Include="Protos\greet.proto" GrpcServices="Client" />
  </ItemGroup>
  ```

### <a name="create-the-greeter-client"></a><span data-ttu-id="2d542-210">Tworzenie klienta Greeter</span><span class="sxs-lookup"><span data-stu-id="2d542-210">Create the Greeter client</span></span>

<span data-ttu-id="2d542-211">Skompiluj projekt, aby utworzyć typy w `GrpcGreeter` przestrzeni nazw.</span><span class="sxs-lookup"><span data-stu-id="2d542-211">Build the project to create the types in the `GrpcGreeter` namespace.</span></span> <span data-ttu-id="2d542-212">`GrpcGreeter` Typy są generowane automatycznie przez proces kompilacji.</span><span class="sxs-lookup"><span data-stu-id="2d542-212">The `GrpcGreeter` types are generated automatically by the build process.</span></span>

<span data-ttu-id="2d542-213">Zaktualizuj plik *program.cs* klienta gRPC za pomocą następującego kodu:</span><span class="sxs-lookup"><span data-stu-id="2d542-213">Update the gRPC client *Program.cs* file with the following code:</span></span>

[!code-cs[](~/tutorials/grpc/grpc-start/sample/GrpcGreeterClient/Program.cs?name=snippet2)]

<span data-ttu-id="2d542-214">*Program.cs* zawiera punkt wejścia i logikę dla klienta gRPC.</span><span class="sxs-lookup"><span data-stu-id="2d542-214">*Program.cs* contains the entry point and logic for the gRPC client.</span></span>

<span data-ttu-id="2d542-215">Klient Greeter jest tworzony przez:</span><span class="sxs-lookup"><span data-stu-id="2d542-215">The Greeter client is created by:</span></span>

* <span data-ttu-id="2d542-216">Utworzenie wystąpienia zawierającego informacje dotyczące tworzenia połączenia z usługą gRPC. `HttpClient`</span><span class="sxs-lookup"><span data-stu-id="2d542-216">Instantiating an `HttpClient` containing the information for creating the connection to the gRPC service.</span></span>
* <span data-ttu-id="2d542-217">Korzystanie z programu do konstruowania klienta Greeter: `HttpClient`</span><span class="sxs-lookup"><span data-stu-id="2d542-217">Using the `HttpClient` to construct the Greeter client:</span></span>

[!code-cs[](~/tutorials/grpc/grpc-start/sample/GrpcGreeterClient/Program.cs?name=snippet&highlight=3-6)]

<span data-ttu-id="2d542-218">Klient Greeter wywołuje metodę asynchroniczną `SayHello` .</span><span class="sxs-lookup"><span data-stu-id="2d542-218">The Greeter client calls the asynchronous `SayHello` method.</span></span> <span data-ttu-id="2d542-219">Zostanie wyświetlony wynik `SayHello` wywołania:</span><span class="sxs-lookup"><span data-stu-id="2d542-219">The result of the `SayHello` call is displayed:</span></span>

[!code-cs[](~/tutorials/grpc/grpc-start/sample/GrpcGreeterClient/Program.cs?name=snippet&highlight=7-9)]

## <a name="test-the-grpc-client-with-the-grpc-greeter-service"></a><span data-ttu-id="2d542-220">Testowanie klienta gRPC za pomocą usługi gRPC Greeter</span><span class="sxs-lookup"><span data-stu-id="2d542-220">Test the gRPC client with the gRPC Greeter service</span></span>

### <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="2d542-221">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="2d542-221">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="2d542-222">W usłudze Greeter naciśnij klawisz `Ctrl+F5` , aby uruchomić serwer bez debugera.</span><span class="sxs-lookup"><span data-stu-id="2d542-222">In the Greeter service, press `Ctrl+F5` to start the server without the debugger.</span></span>
* <span data-ttu-id="2d542-223">W projekcie naciśnij klawisz `Ctrl+F5` , aby uruchomić serwer bez debugera. `GrpcGreeterClient`</span><span class="sxs-lookup"><span data-stu-id="2d542-223">In the `GrpcGreeterClient` project, press `Ctrl+F5` to start the server without the debugger.</span></span>

### <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[<span data-ttu-id="2d542-224">Visual Studio Code/Visual Studio dla komputerów Mac</span><span class="sxs-lookup"><span data-stu-id="2d542-224">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

* <span data-ttu-id="2d542-225">Uruchom usługę Greeter.</span><span class="sxs-lookup"><span data-stu-id="2d542-225">Start the Greeter service.</span></span>
* <span data-ttu-id="2d542-226">Uruchom klienta programu.</span><span class="sxs-lookup"><span data-stu-id="2d542-226">Start the client.</span></span>

<!-- End of combined VS/Mac tabs -->

---

<span data-ttu-id="2d542-227">Klient wysyła powitanie do usługi z komunikatem zawierającym nazwę "GreeterClient".</span><span class="sxs-lookup"><span data-stu-id="2d542-227">The client sends a greeting to the service with a message containing its name "GreeterClient".</span></span> <span data-ttu-id="2d542-228">Usługa wysyła komunikat "Hello GreeterClient" jako odpowiedź.</span><span class="sxs-lookup"><span data-stu-id="2d542-228">The service sends the message "Hello GreeterClient" as a response.</span></span> <span data-ttu-id="2d542-229">Odpowiedź "Hello GreeterClient" jest wyświetlana w wierszu polecenia:</span><span class="sxs-lookup"><span data-stu-id="2d542-229">The "Hello GreeterClient" response is displayed in the command prompt:</span></span>

```console
Greeting: Hello GreeterClient
Press any key to exit...
```

<span data-ttu-id="2d542-230">Usługa gRPC rejestruje szczegóły pomyślnego wywołania w dziennikach zapisanych w wierszu polecenia.</span><span class="sxs-lookup"><span data-stu-id="2d542-230">The gRPC service records the details of the successful call in the logs written to the command prompt.</span></span>

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

### <a name="next-steps"></a><span data-ttu-id="2d542-231">Następne kroki</span><span class="sxs-lookup"><span data-stu-id="2d542-231">Next steps</span></span>

* <xref:grpc/index>
* <xref:grpc/basics>
* <xref:grpc/migration>
