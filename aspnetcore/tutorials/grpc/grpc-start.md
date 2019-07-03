---
title: Tworzenie platformy .NET Core gRPC klienta i serwera w programie ASP.NET Core
author: juntaoluo
description: W tym samouczku przedstawiono sposób tworzenia klienta usługi i gRPC gRPC programu ASP.NET Core. Dowiedz się, jak utworzyć projekt usługi gRPC, Edytuj plik proto i dodać dupleks, przesyłanie strumieniowe wywołania.
monikerRange: '>= aspnetcore-3.0'
ms.author: johluo
ms.date: 06/12/2019
uid: tutorials/grpc/grpc-start
ms.openlocfilehash: 8507c3dcefeb61a4dd34a1ff967c082d287cf646
ms.sourcegitcommit: d6e51c60439f03a8992bda70cc982ddb15d3f100
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/03/2019
ms.locfileid: "67555888"
---
# <a name="tutorial-create-a-grpc-client-and-server-in-aspnet-core"></a><span data-ttu-id="e7896-104">Samouczek: Utworzenie gRPC klienta i serwera w programie ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="e7896-104">Tutorial: Create a gRPC client and server in ASP.NET Core</span></span>

<span data-ttu-id="e7896-105">Przez [Luo Jan](https://github.com/juntaoluo)</span><span class="sxs-lookup"><span data-stu-id="e7896-105">By [John Luo](https://github.com/juntaoluo)</span></span>

<span data-ttu-id="e7896-106">W tym samouczku przedstawiono sposób tworzenia platformy .NET Core [gRPC](https://grpc.io/docs/guides/) klienta i ASP.NET Core gRPC serwera.</span><span class="sxs-lookup"><span data-stu-id="e7896-106">This tutorial shows how to create a .NET Core [gRPC](https://grpc.io/docs/guides/) client and an ASP.NET Core gRPC Server.</span></span>

<span data-ttu-id="e7896-107">Po zakończeniu będziesz mieć klienta gRPC, który komunikuje się z gRPC Greeter usługi.</span><span class="sxs-lookup"><span data-stu-id="e7896-107">At the end, you'll have a gRPC client that communicates with the gRPC Greeter service.</span></span>

<span data-ttu-id="e7896-108">[Wyświetlanie lub pobieranie przykładowego kodu](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/tutorials/grpc/grpc-start/sample) ([sposobu pobierania](xref:index#how-to-download-a-sample)).</span><span class="sxs-lookup"><span data-stu-id="e7896-108">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/tutorials/grpc/grpc-start/sample) ([how to download](xref:index#how-to-download-a-sample)).</span></span>

<span data-ttu-id="e7896-109">W ramach tego samouczka możesz:</span><span class="sxs-lookup"><span data-stu-id="e7896-109">In this tutorial, you:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="e7896-110">Utwórz gRPC serwera.</span><span class="sxs-lookup"><span data-stu-id="e7896-110">Create a gRPC Server.</span></span>
> * <span data-ttu-id="e7896-111">Tworzenie klienta gRPC.</span><span class="sxs-lookup"><span data-stu-id="e7896-111">Create a gRPC client.</span></span>
> * <span data-ttu-id="e7896-112">Przetestuj usługę klienta gRPC z gRPC Greeter usługi.</span><span class="sxs-lookup"><span data-stu-id="e7896-112">Test the gRPC client service with the gRPC Greeter service.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="e7896-113">Wymagania wstępne</span><span class="sxs-lookup"><span data-stu-id="e7896-113">Prerequisites</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="e7896-114">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="e7896-114">Visual Studio</span></span>](#tab/visual-studio)

[!INCLUDE[](~/includes/net-core-prereqs-vs-3.0.md)]

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="e7896-115">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="e7896-115">Visual Studio Code</span></span>](#tab/visual-studio-code)

[!INCLUDE[](~/includes/net-core-prereqs-vsc-3.0.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="e7896-116">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="e7896-116">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

[!INCLUDE[](~/includes/net-core-prereqs-mac-3.0.md)]

---

## <a name="create-a-grpc-service"></a><span data-ttu-id="e7896-117">Tworzenie usługi gRPC</span><span class="sxs-lookup"><span data-stu-id="e7896-117">Create a gRPC service</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="e7896-118">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="e7896-118">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="e7896-119">W programie Visual Studio **pliku** menu, wybierz opcję **New** > **projektu**.</span><span class="sxs-lookup"><span data-stu-id="e7896-119">From the Visual Studio **File** menu, select **New** > **Project**.</span></span>
* <span data-ttu-id="e7896-120">W **Utwórz nowy projekt** okno dialogowe, wybierz opcję **aplikacji sieci Web programu ASP.NET Core**.</span><span class="sxs-lookup"><span data-stu-id="e7896-120">In the **Create a new project** dialog, select **ASP.NET Core Web Application**.</span></span>
* <span data-ttu-id="e7896-121">Wybierz **dalej**</span><span class="sxs-lookup"><span data-stu-id="e7896-121">Select **Next**</span></span>
* <span data-ttu-id="e7896-122">Nadaj projektowi nazwę **GrpcGreeter**.</span><span class="sxs-lookup"><span data-stu-id="e7896-122">Name the project **GrpcGreeter**.</span></span> <span data-ttu-id="e7896-123">Ważne jest, aby nadaj projektowi nazwę *GrpcGreeter* , przestrzenie nazw będą zgodne po skopiuj i Wklej kod.</span><span class="sxs-lookup"><span data-stu-id="e7896-123">It's important to name the project *GrpcGreeter* so the namespaces will match when you copy and paste code.</span></span>
* <span data-ttu-id="e7896-124">Wybierz pozycję **Utwórz**</span><span class="sxs-lookup"><span data-stu-id="e7896-124">Select **Create**</span></span>
* <span data-ttu-id="e7896-125">W **Tworzenie nowej aplikacji sieci Web platformy ASP.NET Core** okno dialogowe:</span><span class="sxs-lookup"><span data-stu-id="e7896-125">In the **Create a new ASP.NET Core Web Application** dialog:</span></span>
  * <span data-ttu-id="e7896-126">Wybierz **platformy .NET Core** i **platformy ASP.NET Core 3.0** w menu rozwijanych.</span><span class="sxs-lookup"><span data-stu-id="e7896-126">Select **.NET Core** and **ASP.NET Core 3.0** in the dropdown menus.</span></span> 
  * <span data-ttu-id="e7896-127">Wybierz **gRPC usługi** szablonu.</span><span class="sxs-lookup"><span data-stu-id="e7896-127">Select the **gRPC Service** template.</span></span>
  * <span data-ttu-id="e7896-128">Wybierz pozycję **Utwórz**</span><span class="sxs-lookup"><span data-stu-id="e7896-128">Select **Create**</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="e7896-129">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="e7896-129">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="e7896-130">Otwórz [zintegrowany terminal](https://code.visualstudio.com/docs/editor/integrated-terminal).</span><span class="sxs-lookup"><span data-stu-id="e7896-130">Open the [integrated terminal](https://code.visualstudio.com/docs/editor/integrated-terminal).</span></span>
* <span data-ttu-id="e7896-131">Zmień katalog (`cd`) do folderu, który będzie zawierać projekt.</span><span class="sxs-lookup"><span data-stu-id="e7896-131">Change directories (`cd`) to a folder which will contain the project.</span></span>
* <span data-ttu-id="e7896-132">Uruchom następujące polecenia:</span><span class="sxs-lookup"><span data-stu-id="e7896-132">Run the following commands:</span></span>

  ```console
  dotnet new grpc -o GrpcGreeter
  code -r GrpcGreeter
  ```

  * <span data-ttu-id="e7896-133">`dotnet new` Polecenie tworzy nową usługę gRPC w *GrpcGreeter* folderu.</span><span class="sxs-lookup"><span data-stu-id="e7896-133">The `dotnet new` command creates a new gRPC service in the *GrpcGreeter* folder.</span></span>
  * <span data-ttu-id="e7896-134">`code` Polecenia otwiera *GrpcGreeter* folderu w nowym wystąpieniu programu Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="e7896-134">The `code` command opens the *GrpcGreeter* folder in a new instance of Visual Studio Code.</span></span>

  <span data-ttu-id="e7896-135">Zostanie wyświetlone okno dialogowe z **"GrpcGreeter" brakuje wymagane zasoby do tworzenia i debugowania. Dodaj je?**</span><span class="sxs-lookup"><span data-stu-id="e7896-135">A dialog box appears with **Required assets to build and debug are missing from 'GrpcGreeter'. Add them?**</span></span>
* <span data-ttu-id="e7896-136">Wybierz **tak**</span><span class="sxs-lookup"><span data-stu-id="e7896-136">Select **Yes**</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="e7896-137">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="e7896-137">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="e7896-138">W terminalu uruchom następujące polecenia:</span><span class="sxs-lookup"><span data-stu-id="e7896-138">From a terminal, run the following commands:</span></span>

```console
  dotnet new grpc -o GrpcGreeter
  cd GrpcGreeter
```

<span data-ttu-id="e7896-139">Poprzednie polecenia użyj [interfejsu wiersza polecenia platformy .NET Core](/dotnet/core/tools/dotnet) utworzyć usługę gRPC.</span><span class="sxs-lookup"><span data-stu-id="e7896-139">The preceding commands use the [.NET Core CLI](/dotnet/core/tools/dotnet) to create a gRPC service.</span></span>

### <a name="open-the-project"></a><span data-ttu-id="e7896-140">Otwórz projekt</span><span class="sxs-lookup"><span data-stu-id="e7896-140">Open the project</span></span>

<span data-ttu-id="e7896-141">Z programu Visual Studio, wybierz **Plik > Otwórz**, a następnie wybierz pozycję *GrpcGreeter.sln* pliku.</span><span class="sxs-lookup"><span data-stu-id="e7896-141">From Visual Studio, select **File > Open**, and then select the *GrpcGreeter.sln* file.</span></span>

<!-- End of VS tabs -->

---

### <a name="run-the-service"></a><span data-ttu-id="e7896-142">Uruchom usługę</span><span class="sxs-lookup"><span data-stu-id="e7896-142">Run the service</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="e7896-143">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="e7896-143">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="e7896-144">Naciśnij klawisz `Ctrl+F5` do uruchamiania usługi gRPC bez debugera.</span><span class="sxs-lookup"><span data-stu-id="e7896-144">Press `Ctrl+F5` to run the gRPC service without the debugger.</span></span>

  <span data-ttu-id="e7896-145">Visual Studio uruchamia usługę w wierszu polecenia.</span><span class="sxs-lookup"><span data-stu-id="e7896-145">Visual Studio runs the service in a command prompt.</span></span>

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[<span data-ttu-id="e7896-146">Visual Studio Code / Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="e7896-146">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

* <span data-ttu-id="e7896-147">Uruchom projekt Greeter gRPC *GrpcGreeter* z wiersza polecenia przy użyciu `dotnet run`.</span><span class="sxs-lookup"><span data-stu-id="e7896-147">Run the gRPC Greeter project *GrpcGreeter* from the command line using `dotnet run`.</span></span>

<!-- End of combined VS/Mac tabs -->

---

<span data-ttu-id="e7896-148">Dzienniki przedstawiają nasłuchiwanie usługi na `http://localhost:50051`.</span><span class="sxs-lookup"><span data-stu-id="e7896-148">The logs show the service listening on `http://localhost:50051`.</span></span>

```console
info: Microsoft.Hosting.Lifetime[0]
      Now listening on: http://localhost:50051
info: Microsoft.Hosting.Lifetime[0]
      Application started. Press Ctrl+C to shut down.
info: Microsoft.Hosting.Lifetime[0]
      Hosting environment: Development
info: Microsoft.Hosting.Lifetime[0]
```

### <a name="examine-the-project-files"></a><span data-ttu-id="e7896-149">Przejrzyj pliki projektu</span><span class="sxs-lookup"><span data-stu-id="e7896-149">Examine the project files</span></span>

<span data-ttu-id="e7896-150">*GrpcGreeter* pliki projektu:</span><span class="sxs-lookup"><span data-stu-id="e7896-150">*GrpcGreeter* project files:</span></span>

* <span data-ttu-id="e7896-151">*greet.proto*: *Protos/greet.proto* plik definiuje `Greeter` gRPC i służy do generowania gRPC zasoby serwera.</span><span class="sxs-lookup"><span data-stu-id="e7896-151">*greet.proto*: The *Protos/greet.proto* file defines the `Greeter` gRPC and is used to generate the gRPC server assets.</span></span> <span data-ttu-id="e7896-152">Aby uzyskać więcej informacji, zobacz [wprowadzenie do gRPC](xref:grpc/index).</span><span class="sxs-lookup"><span data-stu-id="e7896-152">For more information, see [Introduction to gRPC](xref:grpc/index).</span></span>
* <span data-ttu-id="e7896-153">*Usługi* folderu: Zawiera implementację `Greeter` usługi.</span><span class="sxs-lookup"><span data-stu-id="e7896-153">*Services* folder: Contains the implementation of the `Greeter` service.</span></span>
* <span data-ttu-id="e7896-154">*appSettings.json*: Zawiera dane konfiguracyjne, takie jak protokół używany przez Kestrel.</span><span class="sxs-lookup"><span data-stu-id="e7896-154">*appSettings.json*: Contains configuration data, such as protocol used by Kestrel.</span></span> <span data-ttu-id="e7896-155">Aby uzyskać więcej informacji, zobacz <xref:fundamentals/configuration/index>.</span><span class="sxs-lookup"><span data-stu-id="e7896-155">For more information, see <xref:fundamentals/configuration/index>.</span></span>
* <span data-ttu-id="e7896-156">*Program.cs*: Zawiera punkt wejścia dla usługi gRPC.</span><span class="sxs-lookup"><span data-stu-id="e7896-156">*Program.cs*: Contains the entry point for the gRPC service.</span></span> <span data-ttu-id="e7896-157">Aby uzyskać więcej informacji, zobacz <xref:fundamentals/host/generic-host>.</span><span class="sxs-lookup"><span data-stu-id="e7896-157">For more information, see <xref:fundamentals/host/generic-host>.</span></span>
* <span data-ttu-id="e7896-158">*Startup.cs*: Zawiera kod, który konfiguruje zachowanie aplikacji.</span><span class="sxs-lookup"><span data-stu-id="e7896-158">*Startup.cs*: Contains code that configures app behavior.</span></span> <span data-ttu-id="e7896-159">Aby uzyskać więcej informacji, zobacz [uruchamiania aplikacji](xref:fundamentals/startup).</span><span class="sxs-lookup"><span data-stu-id="e7896-159">For more information, see [App startup](xref:fundamentals/startup).</span></span>

## <a name="create-the-grpc-client-in-a-net-console-app"></a><span data-ttu-id="e7896-160">Tworzenie klienta gRPC w aplikacji konsoli .NET</span><span class="sxs-lookup"><span data-stu-id="e7896-160">Create the gRPC client in a .NET console app</span></span>

## <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="e7896-161">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="e7896-161">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="e7896-162">Otwórz drugie wystąpienie programu Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="e7896-162">Open a second instance of Visual Studio.</span></span>
* <span data-ttu-id="e7896-163">Wybierz **pliku** > **New** > **projektu** z paska menu.</span><span class="sxs-lookup"><span data-stu-id="e7896-163">Select **File** > **New** > **Project** from the menu bar.</span></span>
* <span data-ttu-id="e7896-164">W **Utwórz nowy projekt** okno dialogowe, wybierz opcję **Aplikacja konsoli (.NET Core)** .</span><span class="sxs-lookup"><span data-stu-id="e7896-164">In the **Create a new project** dialog, select **Console App (.NET Core)**.</span></span>
* <span data-ttu-id="e7896-165">Wybierz **dalej**</span><span class="sxs-lookup"><span data-stu-id="e7896-165">Select **Next**</span></span>
* <span data-ttu-id="e7896-166">W **nazwa** tekstu wprowadź "GrpcGreeterClient".</span><span class="sxs-lookup"><span data-stu-id="e7896-166">In the **Name** text box, enter "GrpcGreeterClient".</span></span>
* <span data-ttu-id="e7896-167">Wybierz pozycję **Utwórz**.</span><span class="sxs-lookup"><span data-stu-id="e7896-167">Select **Create**.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="e7896-168">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="e7896-168">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="e7896-169">Otwórz [zintegrowany terminal](https://code.visualstudio.com/docs/editor/integrated-terminal).</span><span class="sxs-lookup"><span data-stu-id="e7896-169">Open the [integrated terminal](https://code.visualstudio.com/docs/editor/integrated-terminal).</span></span>
* <span data-ttu-id="e7896-170">Zmień katalog (`cd`) do folderu, który będzie zawierać projekt.</span><span class="sxs-lookup"><span data-stu-id="e7896-170">Change directories (`cd`) to a folder which will contain the project.</span></span>
* <span data-ttu-id="e7896-171">Uruchom następujące polecenia:</span><span class="sxs-lookup"><span data-stu-id="e7896-171">Run the following commands:</span></span>

```console
dotnet new console -o GrpcGreeterClient
code -r GrpcGreeterClient
```

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="e7896-172">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="e7896-172">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="e7896-173">Postępuj zgodnie z instrukcjami [tutaj](/dotnet/core/tutorials/using-on-mac-vs-full-solution) do tworzenia aplikacji konsolowej o nazwie *GrpcGreeterClient*.</span><span class="sxs-lookup"><span data-stu-id="e7896-173">Follow the instructions [here](/dotnet/core/tutorials/using-on-mac-vs-full-solution) to create a console app with the name *GrpcGreeterClient*.</span></span>

<!-- End of VS tabs -->

---

### <a name="add-required-packages"></a><span data-ttu-id="e7896-174">Dodawanie wymaganych pakietów</span><span class="sxs-lookup"><span data-stu-id="e7896-174">Add required packages</span></span>

<span data-ttu-id="e7896-175">GRPC projekt klienta wymaga następujących pakietów:</span><span class="sxs-lookup"><span data-stu-id="e7896-175">The gRPC client project requires the following packages:</span></span>

* <span data-ttu-id="e7896-176">[Grpc.Net.Client](https://www.nuget.org/packages/Grpc.Net.Client), który zawiera klienta programu .NET Core.</span><span class="sxs-lookup"><span data-stu-id="e7896-176">[Grpc.Net.Client](https://www.nuget.org/packages/Grpc.Net.Client), which contains the .NET Core client.</span></span>
* <span data-ttu-id="e7896-177">[Google.Protobuf](https://www.nuget.org/packages/Google.Protobuf/), który zawiera protobuf komunikatu interfejsów API na potrzeby C#.</span><span class="sxs-lookup"><span data-stu-id="e7896-177">[Google.Protobuf](https://www.nuget.org/packages/Google.Protobuf/), which contains protobuf message APIs for C#.</span></span>
* <span data-ttu-id="e7896-178">[Grpc.Tools](https://www.nuget.org/packages/Grpc.Tools/), który zawiera C# narzędzia do obsługi formatu protobuf plików.</span><span class="sxs-lookup"><span data-stu-id="e7896-178">[Grpc.Tools](https://www.nuget.org/packages/Grpc.Tools/), which contains C# tooling support for protobuf files.</span></span> <span data-ttu-id="e7896-179">Pakiet narzędzi nie jest wymagane w czasie wykonywania, więc zależność jest oznaczona za pomocą `PrivateAssets="All"`.</span><span class="sxs-lookup"><span data-stu-id="e7896-179">The tooling package isn't required at runtime, so the dependency is marked with `PrivateAssets="All"`.</span></span>

### <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="e7896-180">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="e7896-180">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="e7896-181">Zainstaluj pakiety przy użyciu konsoli Menedżera pakietów (PMC) "lub" Zarządzaj pakietami NuGet.</span><span class="sxs-lookup"><span data-stu-id="e7896-181">Install the packages using either the Package Manager Console (PMC) or Manage NuGet Packages.</span></span>

#### <a name="pmc-option-to-install-packages"></a><span data-ttu-id="e7896-182">Konsolę zarządzania Pakietami opcję, aby zainstalować pakiety</span><span class="sxs-lookup"><span data-stu-id="e7896-182">PMC option to install packages</span></span>

* <span data-ttu-id="e7896-183">W programie Visual Studio, wybierz **narzędzia** > **Menedżera pakietów NuGet** > **Konsola Menedżera pakietów**</span><span class="sxs-lookup"><span data-stu-id="e7896-183">From Visual Studio, select **Tools** > **NuGet Package Manager** > **Package Manager Console**</span></span>
* <span data-ttu-id="e7896-184">Z **Konsola Menedżera pakietów** okna, przejdź do katalogu, w którym *GrpcGreeterClient.csproj* plik istnieje.</span><span class="sxs-lookup"><span data-stu-id="e7896-184">From the **Package Manager Console** window, navigate to the directory in which the *GrpcGreeterClient.csproj* file exists.</span></span>
* <span data-ttu-id="e7896-185">Uruchom następujące polecenia:</span><span class="sxs-lookup"><span data-stu-id="e7896-185">Run the following commands:</span></span>

 ```powershell
Install-Package Grpc.Net.Client
Install-Package Google.Protobuf
Install-Package Grpc.Tools
```

#### <a name="manage-nuget-packages-option-to-install-packages"></a><span data-ttu-id="e7896-186">Zarządzanie pakietami NuGet opcję, aby zainstalować pakiety</span><span class="sxs-lookup"><span data-stu-id="e7896-186">Manage NuGet Packages option to install packages</span></span>

* <span data-ttu-id="e7896-187">Kliknij prawym przyciskiem myszy projekt w **Eksploratora rozwiązań** > **Zarządzaj pakietami NuGet**</span><span class="sxs-lookup"><span data-stu-id="e7896-187">Right-click the project in **Solution Explorer** > **Manage NuGet Packages**</span></span>
* <span data-ttu-id="e7896-188">Wybierz **Przeglądaj** kartę.</span><span class="sxs-lookup"><span data-stu-id="e7896-188">Select the **Browse** tab.</span></span>
* <span data-ttu-id="e7896-189">Wprowadź **Grpc.Core** w polu wyszukiwania.</span><span class="sxs-lookup"><span data-stu-id="e7896-189">Enter **Grpc.Core** in the search box.</span></span>
* <span data-ttu-id="e7896-190">Wybierz **Grpc.Core** pakietu z **Przeglądaj** kartę, a następnie wybierz pozycję **zainstalować**.</span><span class="sxs-lookup"><span data-stu-id="e7896-190">Select the **Grpc.Core** package from the **Browse** tab and select **Install**.</span></span>
* <span data-ttu-id="e7896-191">Powtórz tę procedurę dla `Google.Protobuf` i `Grpc.Tools`.</span><span class="sxs-lookup"><span data-stu-id="e7896-191">Repeat for `Google.Protobuf` and `Grpc.Tools`.</span></span>

### <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="e7896-192">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="e7896-192">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="e7896-193">Uruchom następujące polecenia z **zintegrowany Terminal**:</span><span class="sxs-lookup"><span data-stu-id="e7896-193">Run the following commands from the **Integrated Terminal**:</span></span>

```console
dotnet add GrpcGreeterClient.csproj package Grpc.Net.Client
dotnet add GrpcGreeterClient.csproj package Google.Protobuf
dotnet add GrpcGreeterClient.csproj package Grpc.Tools
```

### <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="e7896-194">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="e7896-194">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="e7896-195">Kliknij prawym przyciskiem myszy **pakietów** folderu w **konsoli rozwiązania** > **Dodawanie pakietów**</span><span class="sxs-lookup"><span data-stu-id="e7896-195">Right-click the **Packages** folder in **Solution Pad** > **Add Packages**</span></span>
* <span data-ttu-id="e7896-196">Wprowadź **Grpc.Net.Client** w polu wyszukiwania.</span><span class="sxs-lookup"><span data-stu-id="e7896-196">Enter **Grpc.Net.Client** in the search box.</span></span>
* <span data-ttu-id="e7896-197">Wybierz **Grpc.Net.Client** pakietu w okienku wyników, a następnie wybierz pozycję **Dodaj pakiet**</span><span class="sxs-lookup"><span data-stu-id="e7896-197">Select the **Grpc.Net.Client** package from the results pane and select **Add Package**</span></span>
* <span data-ttu-id="e7896-198">Powtórz tę procedurę dla `Google.Protobuf` i `Grpc.Tools`.</span><span class="sxs-lookup"><span data-stu-id="e7896-198">Repeat for `Google.Protobuf` and `Grpc.Tools`.</span></span>

---

### <a name="add-greetproto"></a><span data-ttu-id="e7896-199">Dodaj greet.proto</span><span class="sxs-lookup"><span data-stu-id="e7896-199">Add greet.proto</span></span>

* <span data-ttu-id="e7896-200">Tworzenie **Protos** folderu w projekcie gRPC klienta.</span><span class="sxs-lookup"><span data-stu-id="e7896-200">Create a **Protos** folder in the gRPC client project.</span></span>
* <span data-ttu-id="e7896-201">Kopiuj **Protos\greet.proto** pliku z gRPC Greeter usługi do projektu klienta gRPC.</span><span class="sxs-lookup"><span data-stu-id="e7896-201">Copy the **Protos\greet.proto** file from the gRPC Greeter service to the gRPC client project.</span></span>
* <span data-ttu-id="e7896-202">Edytuj *GrpcGreeterClient.csproj* pliku projektu:</span><span class="sxs-lookup"><span data-stu-id="e7896-202">Edit the *GrpcGreeterClient.csproj* project file:</span></span>

  # <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="e7896-203">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="e7896-203">Visual Studio</span></span>](#tab/visual-studio) 

  <span data-ttu-id="e7896-204">Kliknij prawym przyciskiem myszy projekt i wybierz **edycji pliku projektu**.</span><span class="sxs-lookup"><span data-stu-id="e7896-204">Right-click the project and select **Edit Project File**.</span></span>

  # <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="e7896-205">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="e7896-205">Visual Studio Code</span></span>](#tab/visual-studio-code) 

  <span data-ttu-id="e7896-206">Wybierz *GrpcGreeterClient.csproj* pliku.</span><span class="sxs-lookup"><span data-stu-id="e7896-206">Select the *GrpcGreeterClient.csproj* file.</span></span>

  # <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="e7896-207">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="e7896-207">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

  <span data-ttu-id="e7896-208">Kliknij prawym przyciskiem myszy projekt i wybierz **Narzędzia > Edytuj plik**.</span><span class="sxs-lookup"><span data-stu-id="e7896-208">Right-click the project and select **Tools > Edit File**.</span></span>

  ---

* <span data-ttu-id="e7896-209">Dodaj grupy elementów `<Protobuf>` element, który odwołuje się do **greet.proto** pliku:</span><span class="sxs-lookup"><span data-stu-id="e7896-209">Add an item group with a `<Protobuf>` element that refers to the **greet.proto** file:</span></span>

  ```XML
  <ItemGroup>
    <Protobuf Include="Protos\greet.proto" GrpcServices="Client" />
  </ItemGroup>
  ```

### <a name="create-the-greeter-client"></a><span data-ttu-id="e7896-210">Tworzenie klienta Greeter</span><span class="sxs-lookup"><span data-stu-id="e7896-210">Create the Greeter client</span></span>

<span data-ttu-id="e7896-211">Skompiluj projekt, aby utworzyć typy w `GrpcGreeter` przestrzeni nazw.</span><span class="sxs-lookup"><span data-stu-id="e7896-211">Build the project to create the types in the `GrpcGreeter` namespace.</span></span> <span data-ttu-id="e7896-212">`GrpcGreeter` Typy są generowane automatycznie przez proces kompilacji.</span><span class="sxs-lookup"><span data-stu-id="e7896-212">The `GrpcGreeter` types are generated automatically by the build process.</span></span>

<span data-ttu-id="e7896-213">Aktualizacja klienta gRPC *Program.cs* pliku następującym kodem:</span><span class="sxs-lookup"><span data-stu-id="e7896-213">Update the gRPC client *Program.cs* file with the following code:</span></span>

[!code-cs[](~/tutorials/grpc/grpc-start/sample/GrpcGreeterClient/Program.cs?name=snippet2)]

<span data-ttu-id="e7896-214">*Plik program.cs* zawiera punktu wejścia, a logika gRPC klienta.</span><span class="sxs-lookup"><span data-stu-id="e7896-214">*Program.cs* contains the entry point and logic for the gRPC client.</span></span>

<span data-ttu-id="e7896-215">Klient Greeter jest tworzony przez:</span><span class="sxs-lookup"><span data-stu-id="e7896-215">The Greeter client is created by:</span></span>

* <span data-ttu-id="e7896-216">Utworzenie wystąpienia `HttpClient` zawierające informacje dotyczące tworzenia połączenia z usługą gRPC.</span><span class="sxs-lookup"><span data-stu-id="e7896-216">Instantiating an `HttpClient` containing the information for creating the connection to the gRPC service.</span></span>
* <span data-ttu-id="e7896-217">Za pomocą `HttpClient` do konstruowania klienta Greeter:</span><span class="sxs-lookup"><span data-stu-id="e7896-217">Using the `HttpClient` to construct the Greeter client:</span></span>

[!code-cs[](~/tutorials/grpc/grpc-start/sample/GrpcGreeterClient/Program.cs?name=snippet&highlight=3-9)]

<span data-ttu-id="e7896-218">Klient Greeter wywołuje asynchroniczną `SayHello` metody.</span><span class="sxs-lookup"><span data-stu-id="e7896-218">The Greeter client calls the asynchronous `SayHello` method.</span></span> <span data-ttu-id="e7896-219">Wynik `SayHello` wywołań jest wyświetlany:</span><span class="sxs-lookup"><span data-stu-id="e7896-219">The result of the `SayHello` call is displayed:</span></span>

[!code-cs[](~/tutorials/grpc/grpc-start/sample/GrpcGreeterClient/Program.cs?name=snippet&highlight=10-12)]

## <a name="test-the-grpc-client-with-the-grpc-greeter-service"></a><span data-ttu-id="e7896-220">Testowanie klienta gRPC gRPC Greeter usługi</span><span class="sxs-lookup"><span data-stu-id="e7896-220">Test the gRPC client with the gRPC Greeter service</span></span>

### <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="e7896-221">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="e7896-221">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="e7896-222">W usłudze Greeter naciśnij `Ctrl+F5` można uruchomić serwera bez debugera.</span><span class="sxs-lookup"><span data-stu-id="e7896-222">In the Greeter service, press `Ctrl+F5` to start the server without the debugger.</span></span>
* <span data-ttu-id="e7896-223">W `GrpcGreeterClient` projektu, naciśnij klawisz `Ctrl+F5` można uruchomić serwera bez debugera.</span><span class="sxs-lookup"><span data-stu-id="e7896-223">In the `GrpcGreeterClient` project, press `Ctrl+F5` to start the server without the debugger.</span></span>

### <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[<span data-ttu-id="e7896-224">Visual Studio Code / Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="e7896-224">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

* <span data-ttu-id="e7896-225">Uruchom usługę Greeter.</span><span class="sxs-lookup"><span data-stu-id="e7896-225">Start the Greeter service.</span></span>
* <span data-ttu-id="e7896-226">Uruchom klienta.</span><span class="sxs-lookup"><span data-stu-id="e7896-226">Start the client.</span></span>

<!-- End of combined VS/Mac tabs -->

---

<span data-ttu-id="e7896-227">Klient wysyła pozdrowienia z usługą za pomocą komunikatu zawierającego jego nazwę "GreeterClient".</span><span class="sxs-lookup"><span data-stu-id="e7896-227">The client sends a greeting to the service with a message containing its name "GreeterClient".</span></span> <span data-ttu-id="e7896-228">Usługa wysyła komunikat "Hello GreeterClient", jako odpowiedź.</span><span class="sxs-lookup"><span data-stu-id="e7896-228">The service sends the message "Hello GreeterClient" as a response.</span></span> <span data-ttu-id="e7896-229">Odpowiedź "Hello GreeterClient" jest wyświetlana w wierszu polecenia:</span><span class="sxs-lookup"><span data-stu-id="e7896-229">The "Hello GreeterClient" response is displayed in the command prompt:</span></span>

```console
Greeting: Hello GreeterClient
Press any key to exit...
```

<span data-ttu-id="e7896-230">Usługa gRPC rejestrujący szczegóły dotyczące pomyślnego wywołania w dziennikach, zapisywany wiersza polecenia.</span><span class="sxs-lookup"><span data-stu-id="e7896-230">The gRPC service records the details of the successful call in the logs written to the command prompt.</span></span>

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

### <a name="next-steps"></a><span data-ttu-id="e7896-231">Następne kroki</span><span class="sxs-lookup"><span data-stu-id="e7896-231">Next steps</span></span>

* <xref:grpc/index>
* <xref:grpc/basics>
* <xref:grpc/migration>
