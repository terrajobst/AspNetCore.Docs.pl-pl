---
title: Tworzenie platformy .NET Core gRPC klienta i serwera w programie ASP.NET Core
author: juntaoluo
description: W tym samouczku przedstawiono sposób tworzenia klienta usługi i gRPC gRPC programu ASP.NET Core. Dowiedz się, jak utworzyć projekt usługi gRPC, Edytuj plik proto i dodać dupleks, przesyłanie strumieniowe wywołania.
monikerRange: '>= aspnetcore-3.0'
ms.author: johluo
ms.date: 06/12/2019
uid: tutorials/grpc/grpc-start
ms.openlocfilehash: 76c40d108bc0ce9dec7099b10812c896f0b7777d
ms.sourcegitcommit: f5762967df3be8b8c868229e679301f2f7954679
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/13/2019
ms.locfileid: "67048200"
---
# <a name="tutorial-create-a-grpc-client-and-server-in-aspnet-core"></a><span data-ttu-id="624cc-104">Samouczek: Utworzenie gRPC klienta i serwera w programie ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="624cc-104">Tutorial: Create a gRPC client and server in ASP.NET Core</span></span>

<span data-ttu-id="624cc-105">Przez [Luo Jan](https://github.com/juntaoluo)</span><span class="sxs-lookup"><span data-stu-id="624cc-105">By [John Luo](https://github.com/juntaoluo)</span></span>

<span data-ttu-id="624cc-106">W tym samouczku przedstawiono sposób tworzenia platformy .NET Core [gRPC](https://grpc.io/docs/guides/) klienta i ASP.NET Core gRPC serwera.</span><span class="sxs-lookup"><span data-stu-id="624cc-106">This tutorial shows how to create a .NET Core [gRPC](https://grpc.io/docs/guides/) client and an ASP.NET Core gRPC Server.</span></span>

<span data-ttu-id="624cc-107">Po zakończeniu będziesz mieć klienta gRPC, który komunikuje się z gRPC Greeter usługi.</span><span class="sxs-lookup"><span data-stu-id="624cc-107">At the end, you'll have a gRPC client that communicates with the gRPC Greeter service.</span></span>

<span data-ttu-id="624cc-108">[Wyświetlanie lub pobieranie przykładowego kodu](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/tutorials/grpc/grpc-start/sample) ([sposobu pobierania](xref:index#how-to-download-a-sample)).</span><span class="sxs-lookup"><span data-stu-id="624cc-108">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/tutorials/grpc/grpc-start/sample) ([how to download](xref:index#how-to-download-a-sample)).</span></span>

<span data-ttu-id="624cc-109">W ramach tego samouczka możesz:</span><span class="sxs-lookup"><span data-stu-id="624cc-109">In this tutorial, you:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="624cc-110">Utwórz gRPC serwera.</span><span class="sxs-lookup"><span data-stu-id="624cc-110">Create a gRPC Server.</span></span>
> * <span data-ttu-id="624cc-111">Tworzenie klienta gRPC.</span><span class="sxs-lookup"><span data-stu-id="624cc-111">Create a gRPC client.</span></span>
> * <span data-ttu-id="624cc-112">Przetestuj usługę klienta gRPC z gRPC Greeter usługi.</span><span class="sxs-lookup"><span data-stu-id="624cc-112">Test the gRPC client service with the gRPC Greeter service.</span></span>

[!INCLUDE[](~/includes/net-core-prereqs-all-3.0.md)]

## <a name="create-a-grpc-service"></a><span data-ttu-id="624cc-113">Tworzenie usługi gRPC</span><span class="sxs-lookup"><span data-stu-id="624cc-113">Create a gRPC service</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="624cc-114">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="624cc-114">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="624cc-115">W programie Visual Studio **pliku** menu, wybierz opcję **New** > **projektu**.</span><span class="sxs-lookup"><span data-stu-id="624cc-115">From the Visual Studio **File** menu, select **New** > **Project**.</span></span>
* <span data-ttu-id="624cc-116">W **Utwórz nowy projekt** okno dialogowe, wybierz opcję **aplikacji sieci Web programu ASP.NET Core**.</span><span class="sxs-lookup"><span data-stu-id="624cc-116">In the **Create a new project** dialog, select **ASP.NET Core Web Application**.</span></span>
* <span data-ttu-id="624cc-117">Wybierz **dalej**</span><span class="sxs-lookup"><span data-stu-id="624cc-117">Select **Next**</span></span>
* <span data-ttu-id="624cc-118">Nadaj projektowi nazwę **GrpcGreeter**.</span><span class="sxs-lookup"><span data-stu-id="624cc-118">Name the project **GrpcGreeter**.</span></span> <span data-ttu-id="624cc-119">Ważne jest, aby nadaj projektowi nazwę *GrpcGreeter* , przestrzenie nazw będą zgodne po skopiuj i Wklej kod.</span><span class="sxs-lookup"><span data-stu-id="624cc-119">It's important to name the project *GrpcGreeter* so the namespaces will match when you copy and paste code.</span></span>
* <span data-ttu-id="624cc-120">Wybierz pozycję **Utwórz**</span><span class="sxs-lookup"><span data-stu-id="624cc-120">Select **Create**</span></span>
* <span data-ttu-id="624cc-121">W **Tworzenie nowej aplikacji sieci Web platformy ASP.NET Core** okno dialogowe:</span><span class="sxs-lookup"><span data-stu-id="624cc-121">In the **Create a new ASP.NET Core Web Application** dialog:</span></span>
  * <span data-ttu-id="624cc-122">Wybierz **platformy .NET Core** i **platformy ASP.NET Core 3.0** w menu rozwijanych.</span><span class="sxs-lookup"><span data-stu-id="624cc-122">Select **.NET Core** and **ASP.NET Core 3.0** in the dropdown menus.</span></span> 
  * <span data-ttu-id="624cc-123">Wybierz **gRPC usługi** szablonu.</span><span class="sxs-lookup"><span data-stu-id="624cc-123">Select the **gRPC Service** template.</span></span>
  * <span data-ttu-id="624cc-124">Wybierz pozycję **Utwórz**</span><span class="sxs-lookup"><span data-stu-id="624cc-124">Select **Create**</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="624cc-125">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="624cc-125">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="624cc-126">Otwórz [zintegrowany terminal](https://code.visualstudio.com/docs/editor/integrated-terminal).</span><span class="sxs-lookup"><span data-stu-id="624cc-126">Open the [integrated terminal](https://code.visualstudio.com/docs/editor/integrated-terminal).</span></span>
* <span data-ttu-id="624cc-127">Zmień katalog (`cd`) do folderu, który będzie zawierać projekt.</span><span class="sxs-lookup"><span data-stu-id="624cc-127">Change directories (`cd`) to a folder which will contain the project.</span></span>
* <span data-ttu-id="624cc-128">Uruchom następujące polecenia:</span><span class="sxs-lookup"><span data-stu-id="624cc-128">Run the following commands:</span></span>

  ```console
  dotnet new grpc -o GrpcGreeter
  code -r GrpcGreeter
  ```

  * <span data-ttu-id="624cc-129">`dotnet new` Polecenie tworzy nową usługę gRPC w *GrpcGreeter* folderu.</span><span class="sxs-lookup"><span data-stu-id="624cc-129">The `dotnet new` command creates a new gRPC service in the *GrpcGreeter* folder.</span></span>
  * <span data-ttu-id="624cc-130">`code` Polecenia otwiera *GrpcGreeter* folderu w nowym wystąpieniu programu Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="624cc-130">The `code` command opens the *GrpcGreeter* folder in a new instance of Visual Studio Code.</span></span>

  <span data-ttu-id="624cc-131">Zostanie wyświetlone okno dialogowe z **"GrpcGreeter" brakuje wymagane zasoby do tworzenia i debugowania. Dodaj je?**</span><span class="sxs-lookup"><span data-stu-id="624cc-131">A dialog box appears with **Required assets to build and debug are missing from 'GrpcGreeter'. Add them?**</span></span>
* <span data-ttu-id="624cc-132">Wybierz **tak**</span><span class="sxs-lookup"><span data-stu-id="624cc-132">Select **Yes**</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="624cc-133">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="624cc-133">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="624cc-134">W terminalu uruchom następujące polecenia:</span><span class="sxs-lookup"><span data-stu-id="624cc-134">From a terminal, run the following commands:</span></span>

```console
  dotnet new grpc -o GrpcGreeter
  cd GrpcGreeter
```

<span data-ttu-id="624cc-135">Poprzednie polecenia użyj [interfejsu wiersza polecenia platformy .NET Core](/dotnet/core/tools/dotnet) utworzyć usługę gRPC.</span><span class="sxs-lookup"><span data-stu-id="624cc-135">The preceding commands use the [.NET Core CLI](/dotnet/core/tools/dotnet) to create a gRPC service.</span></span>

### <a name="open-the-project"></a><span data-ttu-id="624cc-136">Otwórz projekt</span><span class="sxs-lookup"><span data-stu-id="624cc-136">Open the project</span></span>

<span data-ttu-id="624cc-137">Z programu Visual Studio, wybierz **Plik > Otwórz**, a następnie wybierz pozycję *GrpcGreeter.sln* pliku.</span><span class="sxs-lookup"><span data-stu-id="624cc-137">From Visual Studio, select **File > Open**, and then select the *GrpcGreeter.sln* file.</span></span>

<!-- End of VS tabs -->

---

### <a name="run-the-service"></a><span data-ttu-id="624cc-138">Uruchom usługę</span><span class="sxs-lookup"><span data-stu-id="624cc-138">Run the service</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="624cc-139">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="624cc-139">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="624cc-140">Naciśnij klawisz `Ctrl+F5` do uruchamiania usługi gRPC bez debugera.</span><span class="sxs-lookup"><span data-stu-id="624cc-140">Press `Ctrl+F5` to run the gRPC service without the debugger.</span></span>

  <span data-ttu-id="624cc-141">Visual Studio uruchamia usługę w wierszu polecenia.</span><span class="sxs-lookup"><span data-stu-id="624cc-141">Visual Studio runs the service in a command prompt.</span></span>

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[<span data-ttu-id="624cc-142">Visual Studio Code / Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="624cc-142">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

* <span data-ttu-id="624cc-143">Uruchom projekt Greeter gRPC *GrpcGreeter* z wiersza polecenia przy użyciu `dotnet run`.</span><span class="sxs-lookup"><span data-stu-id="624cc-143">Run the gRPC Greeter project *GrpcGreeter* from the command line using `dotnet run`.</span></span>

<!-- End of combined VS/Mac tabs -->

---

<span data-ttu-id="624cc-144">Dzienniki przedstawiają nasłuchiwanie usługi na `http://localhost:50051`.</span><span class="sxs-lookup"><span data-stu-id="624cc-144">The logs show the service listening on `http://localhost:50051`.</span></span>

```console
info: Microsoft.Hosting.Lifetime[0]
      Now listening on: http://localhost:50051
info: Microsoft.Hosting.Lifetime[0]
      Application started. Press Ctrl+C to shut down.
info: Microsoft.Hosting.Lifetime[0]
      Hosting environment: Development
info: Microsoft.Hosting.Lifetime[0]
```

### <a name="examine-the-project-files"></a><span data-ttu-id="624cc-145">Przejrzyj pliki projektu</span><span class="sxs-lookup"><span data-stu-id="624cc-145">Examine the project files</span></span>

<span data-ttu-id="624cc-146">*GrpcGreeter* pliki projektu:</span><span class="sxs-lookup"><span data-stu-id="624cc-146">*GrpcGreeter* project files:</span></span>

* <span data-ttu-id="624cc-147">*greet.proto*: *Protos/greet.proto* plik definiuje `Greeter` gRPC i służy do generowania gRPC zasoby serwera.</span><span class="sxs-lookup"><span data-stu-id="624cc-147">*greet.proto*: The *Protos/greet.proto* file defines the `Greeter` gRPC and is used to generate the gRPC server assets.</span></span> <span data-ttu-id="624cc-148">Aby uzyskać więcej informacji, zobacz [wprowadzenie do gRPC](xref:grpc/index).</span><span class="sxs-lookup"><span data-stu-id="624cc-148">For more information, see [Introduction to gRPC](xref:grpc/index).</span></span>
* <span data-ttu-id="624cc-149">*Usługi* folderu: Zawiera implementację `Greeter` usługi.</span><span class="sxs-lookup"><span data-stu-id="624cc-149">*Services* folder: Contains the implementation of the `Greeter` service.</span></span>
* <span data-ttu-id="624cc-150">*appSettings.json*: Zawiera dane konfiguracyjne, takie jak protokół używany przez Kestrel.</span><span class="sxs-lookup"><span data-stu-id="624cc-150">*appSettings.json*: Contains configuration data, such as protocol used by Kestrel.</span></span> <span data-ttu-id="624cc-151">Aby uzyskać więcej informacji, zobacz <xref:fundamentals/configuration/index>.</span><span class="sxs-lookup"><span data-stu-id="624cc-151">For more information, see <xref:fundamentals/configuration/index>.</span></span>
* <span data-ttu-id="624cc-152">*Program.cs*: Zawiera punkt wejścia dla usługi gRPC.</span><span class="sxs-lookup"><span data-stu-id="624cc-152">*Program.cs*: Contains the entry point for the gRPC service.</span></span> <span data-ttu-id="624cc-153">Aby uzyskać więcej informacji, zobacz <xref:fundamentals/host/generic-host>.</span><span class="sxs-lookup"><span data-stu-id="624cc-153">For more information, see <xref:fundamentals/host/generic-host>.</span></span>
* <span data-ttu-id="624cc-154">*Startup.cs*: Zawiera kod, który konfiguruje zachowanie aplikacji.</span><span class="sxs-lookup"><span data-stu-id="624cc-154">*Startup.cs*: Contains code that configures app behavior.</span></span> <span data-ttu-id="624cc-155">Aby uzyskać więcej informacji, zobacz [uruchamiania aplikacji](xref:fundamentals/startup).</span><span class="sxs-lookup"><span data-stu-id="624cc-155">For more information, see [App startup](xref:fundamentals/startup).</span></span>

## <a name="create-the-grpc-client-in-a-net-console-app"></a><span data-ttu-id="624cc-156">Tworzenie klienta gRPC w aplikacji konsoli .NET</span><span class="sxs-lookup"><span data-stu-id="624cc-156">Create the gRPC client in a .NET console app</span></span>

## <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="624cc-157">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="624cc-157">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="624cc-158">Wybierz **pliku** > **New** > **projektu** z paska menu.</span><span class="sxs-lookup"><span data-stu-id="624cc-158">Select **File** > **New** > **Project** from the menu bar.</span></span>
* <span data-ttu-id="624cc-159">W **Utwórz nowy projekt** okno dialogowe, wybierz opcję **Aplikacja konsoli (.NET Core)** .</span><span class="sxs-lookup"><span data-stu-id="624cc-159">In the **Create a new project** dialog, select **Console App (.NET Core)**.</span></span>
* <span data-ttu-id="624cc-160">Wybierz **dalej**</span><span class="sxs-lookup"><span data-stu-id="624cc-160">Select **Next**</span></span>
* <span data-ttu-id="624cc-161">W **nazwa** tekstu wprowadź "GrpcGreeterClient".</span><span class="sxs-lookup"><span data-stu-id="624cc-161">In the **Name** text box, enter "GrpcGreeterClient".</span></span>
* <span data-ttu-id="624cc-162">Wybierz pozycję **Utwórz**.</span><span class="sxs-lookup"><span data-stu-id="624cc-162">Select **Create**.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="624cc-163">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="624cc-163">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="624cc-164">Otwórz [zintegrowany terminal](https://code.visualstudio.com/docs/editor/integrated-terminal).</span><span class="sxs-lookup"><span data-stu-id="624cc-164">Open the [integrated terminal](https://code.visualstudio.com/docs/editor/integrated-terminal).</span></span>
* <span data-ttu-id="624cc-165">Zmień katalog (`cd`) do folderu, który będzie zawierać projekt.</span><span class="sxs-lookup"><span data-stu-id="624cc-165">Change directories (`cd`) to a folder which will contain the project.</span></span>
* <span data-ttu-id="624cc-166">Uruchom następujące polecenia:</span><span class="sxs-lookup"><span data-stu-id="624cc-166">Run the following commands:</span></span>

```console
dotnet new console -o GrpcGreeterClient
code -r GrpcGreeterClient
```

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="624cc-167">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="624cc-167">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="624cc-168">Postępuj zgodnie z instrukcjami [tutaj](/dotnet/core/tutorials/using-on-mac-vs-full-solution) do tworzenia aplikacji konsolowej o nazwie *GrpcGreeterClient*.</span><span class="sxs-lookup"><span data-stu-id="624cc-168">Follow the instructions [here](/dotnet/core/tutorials/using-on-mac-vs-full-solution) to create a console app with the name *GrpcGreeterClient*.</span></span>

<!-- End of VS tabs -->

---

### <a name="add-required-packages"></a><span data-ttu-id="624cc-169">Dodawanie wymaganych pakietów</span><span class="sxs-lookup"><span data-stu-id="624cc-169">Add required packages</span></span>

<span data-ttu-id="624cc-170">GRPC projekt klienta, należy dodać następujące pakiety:</span><span class="sxs-lookup"><span data-stu-id="624cc-170">Add the following packages to the gRPC client project:</span></span>

* <span data-ttu-id="624cc-171">[Grpc.Net.Client](https://www.nuget.org/packages/Grpc.Net.Client), który zawiera klienta programu .NET Core.</span><span class="sxs-lookup"><span data-stu-id="624cc-171">[Grpc.Net.Client](https://www.nuget.org/packages/Grpc.Net.Client), which contains the .NET Core client.</span></span>
* <span data-ttu-id="624cc-172">[Google.Protobuf](https://www.nuget.org/packages/Google.Protobuf/), który zawiera protobuf komunikatu interfejsów API na potrzeby C#.</span><span class="sxs-lookup"><span data-stu-id="624cc-172">[Google.Protobuf](https://www.nuget.org/packages/Google.Protobuf/), which contains protobuf message APIs for C#.</span></span>
* <span data-ttu-id="624cc-173">[Grpc.Tools](https://www.nuget.org/packages/Grpc.Tools/), który zawiera C# narzędzia do obsługi formatu protobuf plików.</span><span class="sxs-lookup"><span data-stu-id="624cc-173">[Grpc.Tools](https://www.nuget.org/packages/Grpc.Tools/), which contains C# tooling support for protobuf files.</span></span> <span data-ttu-id="624cc-174">Pakiet narzędzi nie jest wymagane w czasie wykonywania, więc zależność jest oznaczona za pomocą `PrivateAssets="All"`.</span><span class="sxs-lookup"><span data-stu-id="624cc-174">The tooling package isn't required at runtime, so the dependency is marked with `PrivateAssets="All"`.</span></span>

### <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="624cc-175">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="624cc-175">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="624cc-176">Zainstaluj pakiety przy użyciu konsoli Menedżera pakietów (PMC) "lub" Zarządzaj pakietami NuGet.</span><span class="sxs-lookup"><span data-stu-id="624cc-176">Install the packages using either the Package Manager Console (PMC) or Manage NuGet Packages.</span></span>

#### <a name="pmc-option-to-install-packages"></a><span data-ttu-id="624cc-177">Konsolę zarządzania Pakietami opcję, aby zainstalować pakiety</span><span class="sxs-lookup"><span data-stu-id="624cc-177">PMC option to install packages</span></span>

* <span data-ttu-id="624cc-178">W programie Visual Studio, wybierz **narzędzia** > **Menedżera pakietów NuGet** > **Konsola Menedżera pakietów**</span><span class="sxs-lookup"><span data-stu-id="624cc-178">From Visual Studio, select **Tools** > **NuGet Package Manager** > **Package Manager Console**</span></span>
* <span data-ttu-id="624cc-179">Z **Konsola Menedżera pakietów** okna, przejdź do katalogu, w którym *GrpcGreeterClient.csproj* plik istnieje.</span><span class="sxs-lookup"><span data-stu-id="624cc-179">From the **Package Manager Console** window, navigate to the directory in which the *GrpcGreeterClient.csproj* file exists.</span></span>
* <span data-ttu-id="624cc-180">Uruchom następujące polecenia:</span><span class="sxs-lookup"><span data-stu-id="624cc-180">Run the following commands:</span></span>

 ```powershell
Install-Package Grpc.Net.Client
Install-Package Google.Protobuf
Install-Package Grpc.Tools
```

#### <a name="manage-nuget-packages-option-to-install-packages"></a><span data-ttu-id="624cc-181">Zarządzanie pakietami NuGet opcję, aby zainstalować pakiety</span><span class="sxs-lookup"><span data-stu-id="624cc-181">Manage NuGet Packages option to install packages</span></span>

* <span data-ttu-id="624cc-182">Kliknij prawym przyciskiem myszy projekt w **Eksploratora rozwiązań** > **Zarządzaj pakietami NuGet**</span><span class="sxs-lookup"><span data-stu-id="624cc-182">Right-click the project in **Solution Explorer** > **Manage NuGet Packages**</span></span>
* <span data-ttu-id="624cc-183">Wybierz **Przeglądaj** kartę.</span><span class="sxs-lookup"><span data-stu-id="624cc-183">Select the **Browse** tab.</span></span>
* <span data-ttu-id="624cc-184">Wprowadź **Grpc.Core** w polu wyszukiwania.</span><span class="sxs-lookup"><span data-stu-id="624cc-184">Enter **Grpc.Core** in the search box.</span></span>
* <span data-ttu-id="624cc-185">Wybierz **Grpc.Core** pakietu z **Przeglądaj** kartę, a następnie wybierz pozycję **zainstalować**.</span><span class="sxs-lookup"><span data-stu-id="624cc-185">Select the **Grpc.Core** package from the **Browse** tab and select **Install**.</span></span>
* <span data-ttu-id="624cc-186">Powtórz tę procedurę dla `Google.Protobuf` i `Grpc.Tools`.</span><span class="sxs-lookup"><span data-stu-id="624cc-186">Repeat for `Google.Protobuf` and `Grpc.Tools`.</span></span>

### <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="624cc-187">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="624cc-187">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="624cc-188">Uruchom następujące polecenia z **zintegrowany Terminal**:</span><span class="sxs-lookup"><span data-stu-id="624cc-188">Run the following commands from the **Integrated Terminal**:</span></span>

```console
dotnet add GrpcGreeterClient.csproj package Grpc.Net.Client
dotnet add GrpcGreeterClient.csproj package Google.Protobuf
dotnet add GrpcGreeterClient.csproj package Grpc.Tools
```

### <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="624cc-189">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="624cc-189">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="624cc-190">Kliknij prawym przyciskiem myszy **pakietów** folderu w **konsoli rozwiązania** > **Dodawanie pakietów**</span><span class="sxs-lookup"><span data-stu-id="624cc-190">Right-click the **Packages** folder in **Solution Pad** > **Add Packages**</span></span>
* <span data-ttu-id="624cc-191">Wprowadź **Grpc.Net.Client** w polu wyszukiwania.</span><span class="sxs-lookup"><span data-stu-id="624cc-191">Enter **Grpc.Net.Client** in the search box.</span></span>
* <span data-ttu-id="624cc-192">Wybierz **Grpc.Net.Client** pakietu w okienku wyników, a następnie wybierz pozycję **Dodaj pakiet**</span><span class="sxs-lookup"><span data-stu-id="624cc-192">Select the **Grpc.Net.Client** package from the results pane and select **Add Package**</span></span>
* <span data-ttu-id="624cc-193">Powtórz tę procedurę dla `Google.Protobuf` i `Grpc.Tools`.</span><span class="sxs-lookup"><span data-stu-id="624cc-193">Repeat for `Google.Protobuf` and `Grpc.Tools`.</span></span>

---

### <a name="add-greetproto"></a><span data-ttu-id="624cc-194">Dodaj greet.proto</span><span class="sxs-lookup"><span data-stu-id="624cc-194">Add greet.proto</span></span>

* <span data-ttu-id="624cc-195">Tworzenie **Protos** folderu w projekcie gRPC klienta.</span><span class="sxs-lookup"><span data-stu-id="624cc-195">Create a **Protos** folder in the gRPC client project.</span></span>
* <span data-ttu-id="624cc-196">Kopiuj **Protos\greet.proto** pliku z gRPC Greeter usługi do projektu klienta gRPC.</span><span class="sxs-lookup"><span data-stu-id="624cc-196">Copy the **Protos\greet.proto** file from the gRPC Greeter service to the gRPC client project.</span></span>
* <span data-ttu-id="624cc-197">Edytuj *GrpcGreeterClient.csproj* pliku projektu:</span><span class="sxs-lookup"><span data-stu-id="624cc-197">Edit the *GrpcGreeterClient.csproj* project file:</span></span>

  # <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="624cc-198">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="624cc-198">Visual Studio</span></span>](#tab/visual-studio) 

  <span data-ttu-id="624cc-199">Kliknij prawym przyciskiem myszy projekt i wybierz **Edytuj GrpcGreeterClient.csproj**.</span><span class="sxs-lookup"><span data-stu-id="624cc-199">Right-click the project and select the **Edit GrpcGreeterClient.csproj**.</span></span>

  # <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="624cc-200">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="624cc-200">Visual Studio Code</span></span>](#tab/visual-studio-code) 

  <span data-ttu-id="624cc-201">Wybierz *GrpcGreeterClient.csproj* pliku.</span><span class="sxs-lookup"><span data-stu-id="624cc-201">Select the *GrpcGreeterClient.csproj* file.</span></span>

  # <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="624cc-202">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="624cc-202">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

  <span data-ttu-id="624cc-203">Kliknij prawym przyciskiem myszy projekt i wybierz **Narzędzia > Edytuj plik**.</span><span class="sxs-lookup"><span data-stu-id="624cc-203">Right-click the project and select **Tools > Edit File**.</span></span>

  ---

* <span data-ttu-id="624cc-204">Dodaj **greet.proto** plik `<Protobuf>` grupy elementów GrpcGreeterClient pliku projektu:</span><span class="sxs-lookup"><span data-stu-id="624cc-204">Add the **greet.proto** file to the `<Protobuf>` item group of the GrpcGreeterClient project file:</span></span>

  ```XML
  <ItemGroup>
    <Protobuf Include="Protos\greet.proto" GrpcServices="Client" />
  </ItemGroup>
  ```

<span data-ttu-id="624cc-205">Skompiluj projekt klienta, aby wyzwolić Generowanie C# zasobów klienta.</span><span class="sxs-lookup"><span data-stu-id="624cc-205">Build the client project to trigger the generation of the C# client assets.</span></span>

### <a name="create-the-greeter-client"></a><span data-ttu-id="624cc-206">Tworzenie klienta Greeter</span><span class="sxs-lookup"><span data-stu-id="624cc-206">Create the Greeter client</span></span>

<span data-ttu-id="624cc-207">Skompiluj projekt, aby utworzyć typy w **Greeter** przestrzeni nazw.</span><span class="sxs-lookup"><span data-stu-id="624cc-207">Build the project to create the types in the **Greeter** namespace.</span></span> <span data-ttu-id="624cc-208">`Greeter` Typy są generowane automatycznie przez proces kompilacji.</span><span class="sxs-lookup"><span data-stu-id="624cc-208">The `Greeter` types are generated automatically by the build process.</span></span>

<span data-ttu-id="624cc-209">Aktualizacja klienta gRPC *Program.cs* pliku następującym kodem:</span><span class="sxs-lookup"><span data-stu-id="624cc-209">Update the gRPC client *Program.cs* file with the following code:</span></span>

[!code-cs[](~/tutorials/grpc/grpc-start/sample/GrpcGreeterClient/Program.cs?name=snippet2)]

<span data-ttu-id="624cc-210">*Plik program.cs* zawiera punktu wejścia, a logika gRPC klienta.</span><span class="sxs-lookup"><span data-stu-id="624cc-210">*Program.cs* contains the entry point and logic for the gRPC client.</span></span>

<span data-ttu-id="624cc-211">Klient Greeter jest tworzony przez:</span><span class="sxs-lookup"><span data-stu-id="624cc-211">The Greeter client is created by:</span></span>

* <span data-ttu-id="624cc-212">Utworzenie wystąpienia `HttpClient` zawierające informacje dotyczące tworzenia połączenia z usługą gRPC.</span><span class="sxs-lookup"><span data-stu-id="624cc-212">Instantiating an `HttpClient` containing the information for creating the connection to the gRPC service.</span></span>
* <span data-ttu-id="624cc-213">Za pomocą `HttpClient` do konstruowania klienta Greeter:</span><span class="sxs-lookup"><span data-stu-id="624cc-213">Using the `HttpClient` to construct the Greeter client:</span></span>

[!code-cs[](~/tutorials/grpc/grpc-start/sample/GrpcGreeterClient/Program.cs?name=snippet&highlight=3-9)]

<span data-ttu-id="624cc-214">Klient Greeter wywołuje asynchroniczną `SayHello` metody.</span><span class="sxs-lookup"><span data-stu-id="624cc-214">The Greeter client calls the asynchronous `SayHello` method.</span></span> <span data-ttu-id="624cc-215">Wynik `SayHello` wywołań jest wyświetlany:</span><span class="sxs-lookup"><span data-stu-id="624cc-215">The result of the `SayHello` call is displayed:</span></span>

[!code-cs[](~/tutorials/grpc/grpc-start/sample/GrpcGreeterClient/Program.cs?name=snippet&highlight=10-12)]

## <a name="test-the-grpc-client-with-the-grpc-greeter-service"></a><span data-ttu-id="624cc-216">Testowanie klienta gRPC gRPC Greeter usługi</span><span class="sxs-lookup"><span data-stu-id="624cc-216">Test the gRPC client with the gRPC Greeter service</span></span>

### <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="624cc-217">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="624cc-217">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="624cc-218">W usłudze Greeter naciśnij `Ctrl+F5` można uruchomić serwera bez debugera.</span><span class="sxs-lookup"><span data-stu-id="624cc-218">In the Greeter service, press `Ctrl+F5` to start the server without the debugger.</span></span>
* <span data-ttu-id="624cc-219">W `GrpcGreeterClient` projektu, naciśnij klawisz `Ctrl+F5` można uruchomić serwera bez debugera.</span><span class="sxs-lookup"><span data-stu-id="624cc-219">In the `GrpcGreeterClient` project, press `Ctrl+F5` to start the server without the debugger.</span></span>

### <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[<span data-ttu-id="624cc-220">Visual Studio Code / Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="624cc-220">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

* <span data-ttu-id="624cc-221">Uruchom usługę Greeter.</span><span class="sxs-lookup"><span data-stu-id="624cc-221">Start the Greeter service.</span></span>
* <span data-ttu-id="624cc-222">Uruchom klienta.</span><span class="sxs-lookup"><span data-stu-id="624cc-222">Start the client.</span></span>

<!-- End of combined VS/Mac tabs -->

---

<span data-ttu-id="624cc-223">Klient wysyła pozdrowienia z usługą za pomocą komunikatu zawierającego jego nazwę "GreeterClient".</span><span class="sxs-lookup"><span data-stu-id="624cc-223">The client sends a greeting to the service with a message containing its name "GreeterClient".</span></span> <span data-ttu-id="624cc-224">Usługa wysyła komunikat "Hello GreeterClient", jako odpowiedź.</span><span class="sxs-lookup"><span data-stu-id="624cc-224">The service sends the message "Hello GreeterClient" as a response.</span></span> <span data-ttu-id="624cc-225">Odpowiedź "Hello GreeterClient" jest wyświetlana w wierszu polecenia:</span><span class="sxs-lookup"><span data-stu-id="624cc-225">The "Hello GreeterClient" response is displayed in the command prompt:</span></span>

```console
Greeting: Hello GreeterClient
Press any key to exit...
```

<span data-ttu-id="624cc-226">Usługa gRPC rejestrujący szczegóły dotyczące pomyślnego wywołania w dziennikach, zapisywany wiersza polecenia.</span><span class="sxs-lookup"><span data-stu-id="624cc-226">The gRPC service records the details of the successful call in the logs written to the command prompt.</span></span>

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

### <a name="next-steps"></a><span data-ttu-id="624cc-227">Następne kroki</span><span class="sxs-lookup"><span data-stu-id="624cc-227">Next steps</span></span>

* <xref:grpc/index>
* <xref:grpc/basics>
* <xref:grpc/migration>
