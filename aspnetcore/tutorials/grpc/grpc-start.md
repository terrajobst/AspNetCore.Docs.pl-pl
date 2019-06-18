---
title: Tworzenie platformy .NET Core gRPC klienta i serwera w programie ASP.NET Core
author: juntaoluo
description: W tym samouczku przedstawiono sposób tworzenia klienta usługi i gRPC gRPC programu ASP.NET Core. Dowiedz się, jak utworzyć projekt usługi gRPC, Edytuj plik proto i dodać dupleks, przesyłanie strumieniowe wywołania.
monikerRange: '>= aspnetcore-3.0'
ms.author: johluo
ms.date: 06/12/2019
uid: tutorials/grpc/grpc-start
ms.openlocfilehash: 6aef56ecd61ad71e166c03c12b28b25b931cdd88
ms.sourcegitcommit: 4ef0362ef8b6e5426fc5af18f22734158fe587e1
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/17/2019
ms.locfileid: "67152933"
---
# <a name="tutorial-create-a-grpc-client-and-server-in-aspnet-core"></a><span data-ttu-id="f71a9-104">Samouczek: Utworzenie gRPC klienta i serwera w programie ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="f71a9-104">Tutorial: Create a gRPC client and server in ASP.NET Core</span></span>

<span data-ttu-id="f71a9-105">Przez [Luo Jan](https://github.com/juntaoluo)</span><span class="sxs-lookup"><span data-stu-id="f71a9-105">By [John Luo](https://github.com/juntaoluo)</span></span>

<span data-ttu-id="f71a9-106">W tym samouczku przedstawiono sposób tworzenia platformy .NET Core [gRPC](https://grpc.io/docs/guides/) klienta i ASP.NET Core gRPC serwera.</span><span class="sxs-lookup"><span data-stu-id="f71a9-106">This tutorial shows how to create a .NET Core [gRPC](https://grpc.io/docs/guides/) client and an ASP.NET Core gRPC Server.</span></span>

<span data-ttu-id="f71a9-107">Po zakończeniu będziesz mieć klienta gRPC, który komunikuje się z gRPC Greeter usługi.</span><span class="sxs-lookup"><span data-stu-id="f71a9-107">At the end, you'll have a gRPC client that communicates with the gRPC Greeter service.</span></span>

<span data-ttu-id="f71a9-108">[Wyświetlanie lub pobieranie przykładowego kodu](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/tutorials/grpc/grpc-start/sample) ([sposobu pobierania](xref:index#how-to-download-a-sample)).</span><span class="sxs-lookup"><span data-stu-id="f71a9-108">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/tutorials/grpc/grpc-start/sample) ([how to download](xref:index#how-to-download-a-sample)).</span></span>

<span data-ttu-id="f71a9-109">W ramach tego samouczka możesz:</span><span class="sxs-lookup"><span data-stu-id="f71a9-109">In this tutorial, you:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="f71a9-110">Utwórz gRPC serwera.</span><span class="sxs-lookup"><span data-stu-id="f71a9-110">Create a gRPC Server.</span></span>
> * <span data-ttu-id="f71a9-111">Tworzenie klienta gRPC.</span><span class="sxs-lookup"><span data-stu-id="f71a9-111">Create a gRPC client.</span></span>
> * <span data-ttu-id="f71a9-112">Przetestuj usługę klienta gRPC z gRPC Greeter usługi.</span><span class="sxs-lookup"><span data-stu-id="f71a9-112">Test the gRPC client service with the gRPC Greeter service.</span></span>

[!INCLUDE[](~/includes/net-core-prereqs-all-3.0.md)]

## <a name="create-a-grpc-service"></a><span data-ttu-id="f71a9-113">Tworzenie usługi gRPC</span><span class="sxs-lookup"><span data-stu-id="f71a9-113">Create a gRPC service</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="f71a9-114">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="f71a9-114">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="f71a9-115">W programie Visual Studio **pliku** menu, wybierz opcję **New** > **projektu**.</span><span class="sxs-lookup"><span data-stu-id="f71a9-115">From the Visual Studio **File** menu, select **New** > **Project**.</span></span>
* <span data-ttu-id="f71a9-116">W **Utwórz nowy projekt** okno dialogowe, wybierz opcję **aplikacji sieci Web programu ASP.NET Core**.</span><span class="sxs-lookup"><span data-stu-id="f71a9-116">In the **Create a new project** dialog, select **ASP.NET Core Web Application**.</span></span>
* <span data-ttu-id="f71a9-117">Wybierz **dalej**</span><span class="sxs-lookup"><span data-stu-id="f71a9-117">Select **Next**</span></span>
* <span data-ttu-id="f71a9-118">Nadaj projektowi nazwę **GrpcGreeter**.</span><span class="sxs-lookup"><span data-stu-id="f71a9-118">Name the project **GrpcGreeter**.</span></span> <span data-ttu-id="f71a9-119">Ważne jest, aby nadaj projektowi nazwę *GrpcGreeter* , przestrzenie nazw będą zgodne po skopiuj i Wklej kod.</span><span class="sxs-lookup"><span data-stu-id="f71a9-119">It's important to name the project *GrpcGreeter* so the namespaces will match when you copy and paste code.</span></span>
* <span data-ttu-id="f71a9-120">Wybierz pozycję **Utwórz**</span><span class="sxs-lookup"><span data-stu-id="f71a9-120">Select **Create**</span></span>
* <span data-ttu-id="f71a9-121">W **Tworzenie nowej aplikacji sieci Web platformy ASP.NET Core** okno dialogowe:</span><span class="sxs-lookup"><span data-stu-id="f71a9-121">In the **Create a new ASP.NET Core Web Application** dialog:</span></span>
  * <span data-ttu-id="f71a9-122">Wybierz **platformy .NET Core** i **platformy ASP.NET Core 3.0** w menu rozwijanych.</span><span class="sxs-lookup"><span data-stu-id="f71a9-122">Select **.NET Core** and **ASP.NET Core 3.0** in the dropdown menus.</span></span> 
  * <span data-ttu-id="f71a9-123">Wybierz **gRPC usługi** szablonu.</span><span class="sxs-lookup"><span data-stu-id="f71a9-123">Select the **gRPC Service** template.</span></span>
  * <span data-ttu-id="f71a9-124">Wybierz pozycję **Utwórz**</span><span class="sxs-lookup"><span data-stu-id="f71a9-124">Select **Create**</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="f71a9-125">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="f71a9-125">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="f71a9-126">Otwórz [zintegrowany terminal](https://code.visualstudio.com/docs/editor/integrated-terminal).</span><span class="sxs-lookup"><span data-stu-id="f71a9-126">Open the [integrated terminal](https://code.visualstudio.com/docs/editor/integrated-terminal).</span></span>
* <span data-ttu-id="f71a9-127">Zmień katalog (`cd`) do folderu, który będzie zawierać projekt.</span><span class="sxs-lookup"><span data-stu-id="f71a9-127">Change directories (`cd`) to a folder which will contain the project.</span></span>
* <span data-ttu-id="f71a9-128">Uruchom następujące polecenia:</span><span class="sxs-lookup"><span data-stu-id="f71a9-128">Run the following commands:</span></span>

  ```console
  dotnet new grpc -o GrpcGreeter
  code -r GrpcGreeter
  ```

  * <span data-ttu-id="f71a9-129">`dotnet new` Polecenie tworzy nową usługę gRPC w *GrpcGreeter* folderu.</span><span class="sxs-lookup"><span data-stu-id="f71a9-129">The `dotnet new` command creates a new gRPC service in the *GrpcGreeter* folder.</span></span>
  * <span data-ttu-id="f71a9-130">`code` Polecenia otwiera *GrpcGreeter* folderu w nowym wystąpieniu programu Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="f71a9-130">The `code` command opens the *GrpcGreeter* folder in a new instance of Visual Studio Code.</span></span>

  <span data-ttu-id="f71a9-131">Zostanie wyświetlone okno dialogowe z **"GrpcGreeter" brakuje wymagane zasoby do tworzenia i debugowania. Dodaj je?**</span><span class="sxs-lookup"><span data-stu-id="f71a9-131">A dialog box appears with **Required assets to build and debug are missing from 'GrpcGreeter'. Add them?**</span></span>
* <span data-ttu-id="f71a9-132">Wybierz **tak**</span><span class="sxs-lookup"><span data-stu-id="f71a9-132">Select **Yes**</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="f71a9-133">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="f71a9-133">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="f71a9-134">W terminalu uruchom następujące polecenia:</span><span class="sxs-lookup"><span data-stu-id="f71a9-134">From a terminal, run the following commands:</span></span>

```console
  dotnet new grpc -o GrpcGreeter
  cd GrpcGreeter
```

<span data-ttu-id="f71a9-135">Poprzednie polecenia użyj [interfejsu wiersza polecenia platformy .NET Core](/dotnet/core/tools/dotnet) utworzyć usługę gRPC.</span><span class="sxs-lookup"><span data-stu-id="f71a9-135">The preceding commands use the [.NET Core CLI](/dotnet/core/tools/dotnet) to create a gRPC service.</span></span>

### <a name="open-the-project"></a><span data-ttu-id="f71a9-136">Otwórz projekt</span><span class="sxs-lookup"><span data-stu-id="f71a9-136">Open the project</span></span>

<span data-ttu-id="f71a9-137">Z programu Visual Studio, wybierz **Plik > Otwórz**, a następnie wybierz pozycję *GrpcGreeter.sln* pliku.</span><span class="sxs-lookup"><span data-stu-id="f71a9-137">From Visual Studio, select **File > Open**, and then select the *GrpcGreeter.sln* file.</span></span>

<!-- End of VS tabs -->

---

### <a name="run-the-service"></a><span data-ttu-id="f71a9-138">Uruchom usługę</span><span class="sxs-lookup"><span data-stu-id="f71a9-138">Run the service</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="f71a9-139">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="f71a9-139">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="f71a9-140">Naciśnij klawisz `Ctrl+F5` do uruchamiania usługi gRPC bez debugera.</span><span class="sxs-lookup"><span data-stu-id="f71a9-140">Press `Ctrl+F5` to run the gRPC service without the debugger.</span></span>

  <span data-ttu-id="f71a9-141">Visual Studio uruchamia usługę w wierszu polecenia.</span><span class="sxs-lookup"><span data-stu-id="f71a9-141">Visual Studio runs the service in a command prompt.</span></span>

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[<span data-ttu-id="f71a9-142">Visual Studio Code / Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="f71a9-142">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

* <span data-ttu-id="f71a9-143">Uruchom projekt Greeter gRPC *GrpcGreeter* z wiersza polecenia przy użyciu `dotnet run`.</span><span class="sxs-lookup"><span data-stu-id="f71a9-143">Run the gRPC Greeter project *GrpcGreeter* from the command line using `dotnet run`.</span></span>

<!-- End of combined VS/Mac tabs -->

---

<span data-ttu-id="f71a9-144">Dzienniki przedstawiają nasłuchiwanie usługi na `http://localhost:50051`.</span><span class="sxs-lookup"><span data-stu-id="f71a9-144">The logs show the service listening on `http://localhost:50051`.</span></span>

```console
info: Microsoft.Hosting.Lifetime[0]
      Now listening on: http://localhost:50051
info: Microsoft.Hosting.Lifetime[0]
      Application started. Press Ctrl+C to shut down.
info: Microsoft.Hosting.Lifetime[0]
      Hosting environment: Development
info: Microsoft.Hosting.Lifetime[0]
```

### <a name="examine-the-project-files"></a><span data-ttu-id="f71a9-145">Przejrzyj pliki projektu</span><span class="sxs-lookup"><span data-stu-id="f71a9-145">Examine the project files</span></span>

<span data-ttu-id="f71a9-146">*GrpcGreeter* pliki projektu:</span><span class="sxs-lookup"><span data-stu-id="f71a9-146">*GrpcGreeter* project files:</span></span>

* <span data-ttu-id="f71a9-147">*greet.proto*: *Protos/greet.proto* plik definiuje `Greeter` gRPC i służy do generowania gRPC zasoby serwera.</span><span class="sxs-lookup"><span data-stu-id="f71a9-147">*greet.proto*: The *Protos/greet.proto* file defines the `Greeter` gRPC and is used to generate the gRPC server assets.</span></span> <span data-ttu-id="f71a9-148">Aby uzyskać więcej informacji, zobacz [wprowadzenie do gRPC](xref:grpc/index).</span><span class="sxs-lookup"><span data-stu-id="f71a9-148">For more information, see [Introduction to gRPC](xref:grpc/index).</span></span>
* <span data-ttu-id="f71a9-149">*Usługi* folderu: Zawiera implementację `Greeter` usługi.</span><span class="sxs-lookup"><span data-stu-id="f71a9-149">*Services* folder: Contains the implementation of the `Greeter` service.</span></span>
* <span data-ttu-id="f71a9-150">*appSettings.json*: Zawiera dane konfiguracyjne, takie jak protokół używany przez Kestrel.</span><span class="sxs-lookup"><span data-stu-id="f71a9-150">*appSettings.json*: Contains configuration data, such as protocol used by Kestrel.</span></span> <span data-ttu-id="f71a9-151">Aby uzyskać więcej informacji, zobacz <xref:fundamentals/configuration/index>.</span><span class="sxs-lookup"><span data-stu-id="f71a9-151">For more information, see <xref:fundamentals/configuration/index>.</span></span>
* <span data-ttu-id="f71a9-152">*Program.cs*: Zawiera punkt wejścia dla usługi gRPC.</span><span class="sxs-lookup"><span data-stu-id="f71a9-152">*Program.cs*: Contains the entry point for the gRPC service.</span></span> <span data-ttu-id="f71a9-153">Aby uzyskać więcej informacji, zobacz <xref:fundamentals/host/generic-host>.</span><span class="sxs-lookup"><span data-stu-id="f71a9-153">For more information, see <xref:fundamentals/host/generic-host>.</span></span>
* <span data-ttu-id="f71a9-154">*Startup.cs*: Zawiera kod, który konfiguruje zachowanie aplikacji.</span><span class="sxs-lookup"><span data-stu-id="f71a9-154">*Startup.cs*: Contains code that configures app behavior.</span></span> <span data-ttu-id="f71a9-155">Aby uzyskać więcej informacji, zobacz [uruchamiania aplikacji](xref:fundamentals/startup).</span><span class="sxs-lookup"><span data-stu-id="f71a9-155">For more information, see [App startup](xref:fundamentals/startup).</span></span>

## <a name="create-the-grpc-client-in-a-net-console-app"></a><span data-ttu-id="f71a9-156">Tworzenie klienta gRPC w aplikacji konsoli .NET</span><span class="sxs-lookup"><span data-stu-id="f71a9-156">Create the gRPC client in a .NET console app</span></span>

## <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="f71a9-157">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="f71a9-157">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="f71a9-158">Otwórz drugie wystąpienie programu Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="f71a9-158">Open a second instance of Visual Studio.</span></span>
* <span data-ttu-id="f71a9-159">Wybierz **pliku** > **New** > **projektu** z paska menu.</span><span class="sxs-lookup"><span data-stu-id="f71a9-159">Select **File** > **New** > **Project** from the menu bar.</span></span>
* <span data-ttu-id="f71a9-160">W **Utwórz nowy projekt** okno dialogowe, wybierz opcję **Aplikacja konsoli (.NET Core)** .</span><span class="sxs-lookup"><span data-stu-id="f71a9-160">In the **Create a new project** dialog, select **Console App (.NET Core)**.</span></span>
* <span data-ttu-id="f71a9-161">Wybierz **dalej**</span><span class="sxs-lookup"><span data-stu-id="f71a9-161">Select **Next**</span></span>
* <span data-ttu-id="f71a9-162">W **nazwa** tekstu wprowadź "GrpcGreeterClient".</span><span class="sxs-lookup"><span data-stu-id="f71a9-162">In the **Name** text box, enter "GrpcGreeterClient".</span></span>
* <span data-ttu-id="f71a9-163">Wybierz pozycję **Utwórz**.</span><span class="sxs-lookup"><span data-stu-id="f71a9-163">Select **Create**.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="f71a9-164">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="f71a9-164">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="f71a9-165">Otwórz [zintegrowany terminal](https://code.visualstudio.com/docs/editor/integrated-terminal).</span><span class="sxs-lookup"><span data-stu-id="f71a9-165">Open the [integrated terminal](https://code.visualstudio.com/docs/editor/integrated-terminal).</span></span>
* <span data-ttu-id="f71a9-166">Zmień katalog (`cd`) do folderu, który będzie zawierać projekt.</span><span class="sxs-lookup"><span data-stu-id="f71a9-166">Change directories (`cd`) to a folder which will contain the project.</span></span>
* <span data-ttu-id="f71a9-167">Uruchom następujące polecenia:</span><span class="sxs-lookup"><span data-stu-id="f71a9-167">Run the following commands:</span></span>

```console
dotnet new console -o GrpcGreeterClient
code -r GrpcGreeterClient
```

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="f71a9-168">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="f71a9-168">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="f71a9-169">Postępuj zgodnie z instrukcjami [tutaj](/dotnet/core/tutorials/using-on-mac-vs-full-solution) do tworzenia aplikacji konsolowej o nazwie *GrpcGreeterClient*.</span><span class="sxs-lookup"><span data-stu-id="f71a9-169">Follow the instructions [here](/dotnet/core/tutorials/using-on-mac-vs-full-solution) to create a console app with the name *GrpcGreeterClient*.</span></span>

<!-- End of VS tabs -->

---

### <a name="add-required-packages"></a><span data-ttu-id="f71a9-170">Dodawanie wymaganych pakietów</span><span class="sxs-lookup"><span data-stu-id="f71a9-170">Add required packages</span></span>

<span data-ttu-id="f71a9-171">GRPC projekt klienta wymaga następujących pakietów:</span><span class="sxs-lookup"><span data-stu-id="f71a9-171">The gRPC client project requires the following packages:</span></span>

* <span data-ttu-id="f71a9-172">[Grpc.Net.Client](https://www.nuget.org/packages/Grpc.Net.Client), który zawiera klienta programu .NET Core.</span><span class="sxs-lookup"><span data-stu-id="f71a9-172">[Grpc.Net.Client](https://www.nuget.org/packages/Grpc.Net.Client), which contains the .NET Core client.</span></span>
* <span data-ttu-id="f71a9-173">[Google.Protobuf](https://www.nuget.org/packages/Google.Protobuf/), który zawiera protobuf komunikatu interfejsów API na potrzeby C#.</span><span class="sxs-lookup"><span data-stu-id="f71a9-173">[Google.Protobuf](https://www.nuget.org/packages/Google.Protobuf/), which contains protobuf message APIs for C#.</span></span>
* <span data-ttu-id="f71a9-174">[Grpc.Tools](https://www.nuget.org/packages/Grpc.Tools/), który zawiera C# narzędzia do obsługi formatu protobuf plików.</span><span class="sxs-lookup"><span data-stu-id="f71a9-174">[Grpc.Tools](https://www.nuget.org/packages/Grpc.Tools/), which contains C# tooling support for protobuf files.</span></span> <span data-ttu-id="f71a9-175">Pakiet narzędzi nie jest wymagane w czasie wykonywania, więc zależność jest oznaczona za pomocą `PrivateAssets="All"`.</span><span class="sxs-lookup"><span data-stu-id="f71a9-175">The tooling package isn't required at runtime, so the dependency is marked with `PrivateAssets="All"`.</span></span>

### <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="f71a9-176">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="f71a9-176">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="f71a9-177">Zainstaluj pakiety przy użyciu konsoli Menedżera pakietów (PMC) "lub" Zarządzaj pakietami NuGet.</span><span class="sxs-lookup"><span data-stu-id="f71a9-177">Install the packages using either the Package Manager Console (PMC) or Manage NuGet Packages.</span></span>

#### <a name="pmc-option-to-install-packages"></a><span data-ttu-id="f71a9-178">Konsolę zarządzania Pakietami opcję, aby zainstalować pakiety</span><span class="sxs-lookup"><span data-stu-id="f71a9-178">PMC option to install packages</span></span>

* <span data-ttu-id="f71a9-179">W programie Visual Studio, wybierz **narzędzia** > **Menedżera pakietów NuGet** > **Konsola Menedżera pakietów**</span><span class="sxs-lookup"><span data-stu-id="f71a9-179">From Visual Studio, select **Tools** > **NuGet Package Manager** > **Package Manager Console**</span></span>
* <span data-ttu-id="f71a9-180">Z **Konsola Menedżera pakietów** okna, przejdź do katalogu, w którym *GrpcGreeterClient.csproj* plik istnieje.</span><span class="sxs-lookup"><span data-stu-id="f71a9-180">From the **Package Manager Console** window, navigate to the directory in which the *GrpcGreeterClient.csproj* file exists.</span></span>
* <span data-ttu-id="f71a9-181">Uruchom następujące polecenia:</span><span class="sxs-lookup"><span data-stu-id="f71a9-181">Run the following commands:</span></span>

 ```powershell
Install-Package Grpc.Net.Client
Install-Package Google.Protobuf
Install-Package Grpc.Tools
```

#### <a name="manage-nuget-packages-option-to-install-packages"></a><span data-ttu-id="f71a9-182">Zarządzanie pakietami NuGet opcję, aby zainstalować pakiety</span><span class="sxs-lookup"><span data-stu-id="f71a9-182">Manage NuGet Packages option to install packages</span></span>

* <span data-ttu-id="f71a9-183">Kliknij prawym przyciskiem myszy projekt w **Eksploratora rozwiązań** > **Zarządzaj pakietami NuGet**</span><span class="sxs-lookup"><span data-stu-id="f71a9-183">Right-click the project in **Solution Explorer** > **Manage NuGet Packages**</span></span>
* <span data-ttu-id="f71a9-184">Wybierz **Przeglądaj** kartę.</span><span class="sxs-lookup"><span data-stu-id="f71a9-184">Select the **Browse** tab.</span></span>
* <span data-ttu-id="f71a9-185">Wprowadź **Grpc.Core** w polu wyszukiwania.</span><span class="sxs-lookup"><span data-stu-id="f71a9-185">Enter **Grpc.Core** in the search box.</span></span>
* <span data-ttu-id="f71a9-186">Wybierz **Grpc.Core** pakietu z **Przeglądaj** kartę, a następnie wybierz pozycję **zainstalować**.</span><span class="sxs-lookup"><span data-stu-id="f71a9-186">Select the **Grpc.Core** package from the **Browse** tab and select **Install**.</span></span>
* <span data-ttu-id="f71a9-187">Powtórz tę procedurę dla `Google.Protobuf` i `Grpc.Tools`.</span><span class="sxs-lookup"><span data-stu-id="f71a9-187">Repeat for `Google.Protobuf` and `Grpc.Tools`.</span></span>

### <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="f71a9-188">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="f71a9-188">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="f71a9-189">Uruchom następujące polecenia z **zintegrowany Terminal**:</span><span class="sxs-lookup"><span data-stu-id="f71a9-189">Run the following commands from the **Integrated Terminal**:</span></span>

```console
dotnet add GrpcGreeterClient.csproj package Grpc.Net.Client
dotnet add GrpcGreeterClient.csproj package Google.Protobuf
dotnet add GrpcGreeterClient.csproj package Grpc.Tools
```

### <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="f71a9-190">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="f71a9-190">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="f71a9-191">Kliknij prawym przyciskiem myszy **pakietów** folderu w **konsoli rozwiązania** > **Dodawanie pakietów**</span><span class="sxs-lookup"><span data-stu-id="f71a9-191">Right-click the **Packages** folder in **Solution Pad** > **Add Packages**</span></span>
* <span data-ttu-id="f71a9-192">Wprowadź **Grpc.Net.Client** w polu wyszukiwania.</span><span class="sxs-lookup"><span data-stu-id="f71a9-192">Enter **Grpc.Net.Client** in the search box.</span></span>
* <span data-ttu-id="f71a9-193">Wybierz **Grpc.Net.Client** pakietu w okienku wyników, a następnie wybierz pozycję **Dodaj pakiet**</span><span class="sxs-lookup"><span data-stu-id="f71a9-193">Select the **Grpc.Net.Client** package from the results pane and select **Add Package**</span></span>
* <span data-ttu-id="f71a9-194">Powtórz tę procedurę dla `Google.Protobuf` i `Grpc.Tools`.</span><span class="sxs-lookup"><span data-stu-id="f71a9-194">Repeat for `Google.Protobuf` and `Grpc.Tools`.</span></span>

---

### <a name="add-greetproto"></a><span data-ttu-id="f71a9-195">Dodaj greet.proto</span><span class="sxs-lookup"><span data-stu-id="f71a9-195">Add greet.proto</span></span>

* <span data-ttu-id="f71a9-196">Tworzenie **Protos** folderu w projekcie gRPC klienta.</span><span class="sxs-lookup"><span data-stu-id="f71a9-196">Create a **Protos** folder in the gRPC client project.</span></span>
* <span data-ttu-id="f71a9-197">Kopiuj **Protos\greet.proto** pliku z gRPC Greeter usługi do projektu klienta gRPC.</span><span class="sxs-lookup"><span data-stu-id="f71a9-197">Copy the **Protos\greet.proto** file from the gRPC Greeter service to the gRPC client project.</span></span>
* <span data-ttu-id="f71a9-198">Edytuj *GrpcGreeterClient.csproj* pliku projektu:</span><span class="sxs-lookup"><span data-stu-id="f71a9-198">Edit the *GrpcGreeterClient.csproj* project file:</span></span>

  # <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="f71a9-199">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="f71a9-199">Visual Studio</span></span>](#tab/visual-studio) 

  <span data-ttu-id="f71a9-200">Kliknij prawym przyciskiem myszy projekt i wybierz **edycji pliku projektu**.</span><span class="sxs-lookup"><span data-stu-id="f71a9-200">Right-click the project and select **Edit Project File**.</span></span>

  # <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="f71a9-201">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="f71a9-201">Visual Studio Code</span></span>](#tab/visual-studio-code) 

  <span data-ttu-id="f71a9-202">Wybierz *GrpcGreeterClient.csproj* pliku.</span><span class="sxs-lookup"><span data-stu-id="f71a9-202">Select the *GrpcGreeterClient.csproj* file.</span></span>

  # <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="f71a9-203">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="f71a9-203">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

  <span data-ttu-id="f71a9-204">Kliknij prawym przyciskiem myszy projekt i wybierz **Narzędzia > Edytuj plik**.</span><span class="sxs-lookup"><span data-stu-id="f71a9-204">Right-click the project and select **Tools > Edit File**.</span></span>

  ---

* <span data-ttu-id="f71a9-205">Dodaj grupy elementów `<Protobuf>` element, który odwołuje się do **greet.proto** pliku:</span><span class="sxs-lookup"><span data-stu-id="f71a9-205">Add an item group with a `<Protobuf>` element that refers to the **greet.proto** file:</span></span>

  ```XML
  <ItemGroup>
    <Protobuf Include="Protos\greet.proto" GrpcServices="Client" />
  </ItemGroup>
  ```

### <a name="create-the-greeter-client"></a><span data-ttu-id="f71a9-206">Tworzenie klienta Greeter</span><span class="sxs-lookup"><span data-stu-id="f71a9-206">Create the Greeter client</span></span>

<span data-ttu-id="f71a9-207">Skompiluj projekt, aby utworzyć typy w `GrpcGreeter` przestrzeni nazw.</span><span class="sxs-lookup"><span data-stu-id="f71a9-207">Build the project to create the types in the `GrpcGreeter` namespace.</span></span> <span data-ttu-id="f71a9-208">`GrpcGreeter` Typy są generowane automatycznie przez proces kompilacji.</span><span class="sxs-lookup"><span data-stu-id="f71a9-208">The `GrpcGreeter` types are generated automatically by the build process.</span></span>

<span data-ttu-id="f71a9-209">Aktualizacja klienta gRPC *Program.cs* pliku następującym kodem:</span><span class="sxs-lookup"><span data-stu-id="f71a9-209">Update the gRPC client *Program.cs* file with the following code:</span></span>

[!code-cs[](~/tutorials/grpc/grpc-start/sample/GrpcGreeterClient/Program.cs?name=snippet2)]

<span data-ttu-id="f71a9-210">*Plik program.cs* zawiera punktu wejścia, a logika gRPC klienta.</span><span class="sxs-lookup"><span data-stu-id="f71a9-210">*Program.cs* contains the entry point and logic for the gRPC client.</span></span>

<span data-ttu-id="f71a9-211">Klient Greeter jest tworzony przez:</span><span class="sxs-lookup"><span data-stu-id="f71a9-211">The Greeter client is created by:</span></span>

* <span data-ttu-id="f71a9-212">Utworzenie wystąpienia `HttpClient` zawierające informacje dotyczące tworzenia połączenia z usługą gRPC.</span><span class="sxs-lookup"><span data-stu-id="f71a9-212">Instantiating an `HttpClient` containing the information for creating the connection to the gRPC service.</span></span>
* <span data-ttu-id="f71a9-213">Za pomocą `HttpClient` do konstruowania klienta Greeter:</span><span class="sxs-lookup"><span data-stu-id="f71a9-213">Using the `HttpClient` to construct the Greeter client:</span></span>

[!code-cs[](~/tutorials/grpc/grpc-start/sample/GrpcGreeterClient/Program.cs?name=snippet&highlight=3-9)]

<span data-ttu-id="f71a9-214">Klient Greeter wywołuje asynchroniczną `SayHello` metody.</span><span class="sxs-lookup"><span data-stu-id="f71a9-214">The Greeter client calls the asynchronous `SayHello` method.</span></span> <span data-ttu-id="f71a9-215">Wynik `SayHello` wywołań jest wyświetlany:</span><span class="sxs-lookup"><span data-stu-id="f71a9-215">The result of the `SayHello` call is displayed:</span></span>

[!code-cs[](~/tutorials/grpc/grpc-start/sample/GrpcGreeterClient/Program.cs?name=snippet&highlight=10-12)]

## <a name="test-the-grpc-client-with-the-grpc-greeter-service"></a><span data-ttu-id="f71a9-216">Testowanie klienta gRPC gRPC Greeter usługi</span><span class="sxs-lookup"><span data-stu-id="f71a9-216">Test the gRPC client with the gRPC Greeter service</span></span>

### <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="f71a9-217">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="f71a9-217">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="f71a9-218">W usłudze Greeter naciśnij `Ctrl+F5` można uruchomić serwera bez debugera.</span><span class="sxs-lookup"><span data-stu-id="f71a9-218">In the Greeter service, press `Ctrl+F5` to start the server without the debugger.</span></span>
* <span data-ttu-id="f71a9-219">W `GrpcGreeterClient` projektu, naciśnij klawisz `Ctrl+F5` można uruchomić serwera bez debugera.</span><span class="sxs-lookup"><span data-stu-id="f71a9-219">In the `GrpcGreeterClient` project, press `Ctrl+F5` to start the server without the debugger.</span></span>

### <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[<span data-ttu-id="f71a9-220">Visual Studio Code / Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="f71a9-220">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

* <span data-ttu-id="f71a9-221">Uruchom usługę Greeter.</span><span class="sxs-lookup"><span data-stu-id="f71a9-221">Start the Greeter service.</span></span>
* <span data-ttu-id="f71a9-222">Uruchom klienta.</span><span class="sxs-lookup"><span data-stu-id="f71a9-222">Start the client.</span></span>

<!-- End of combined VS/Mac tabs -->

---

<span data-ttu-id="f71a9-223">Klient wysyła pozdrowienia z usługą za pomocą komunikatu zawierającego jego nazwę "GreeterClient".</span><span class="sxs-lookup"><span data-stu-id="f71a9-223">The client sends a greeting to the service with a message containing its name "GreeterClient".</span></span> <span data-ttu-id="f71a9-224">Usługa wysyła komunikat "Hello GreeterClient", jako odpowiedź.</span><span class="sxs-lookup"><span data-stu-id="f71a9-224">The service sends the message "Hello GreeterClient" as a response.</span></span> <span data-ttu-id="f71a9-225">Odpowiedź "Hello GreeterClient" jest wyświetlana w wierszu polecenia:</span><span class="sxs-lookup"><span data-stu-id="f71a9-225">The "Hello GreeterClient" response is displayed in the command prompt:</span></span>

```console
Greeting: Hello GreeterClient
Press any key to exit...
```

<span data-ttu-id="f71a9-226">Usługa gRPC rejestrujący szczegóły dotyczące pomyślnego wywołania w dziennikach, zapisywany wiersza polecenia.</span><span class="sxs-lookup"><span data-stu-id="f71a9-226">The gRPC service records the details of the successful call in the logs written to the command prompt.</span></span>

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

### <a name="next-steps"></a><span data-ttu-id="f71a9-227">Następne kroki</span><span class="sxs-lookup"><span data-stu-id="f71a9-227">Next steps</span></span>

* <xref:grpc/index>
* <xref:grpc/basics>
* <xref:grpc/migration>
