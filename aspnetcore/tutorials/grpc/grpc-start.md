---
title: Tworzenie platformy .NET Core gRPC klienta i serwera w programie ASP.NET Core
author: juntaoluo
description: W tym samouczku przedstawiono sposób tworzenia klienta usługi i gRPC gRPC programu ASP.NET Core. Dowiedz się, jak utworzyć projekt usługi gRPC, Edytuj plik proto i dodać dupleks, przesyłanie strumieniowe wywołania.
monikerRange: '>= aspnetcore-3.0'
ms.author: johluo
ms.date: 5/30/2019
uid: tutorials/grpc/grpc-start
ms.openlocfilehash: 2e1768e82f670750621257fec10457c11e9b601b
ms.sourcegitcommit: a1364109d11d414121a6337b611bee61d6e489e9
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/04/2019
ms.locfileid: "66491222"
---
# <a name="tutorial-create-a-grpc-client-and-server-in-aspnet-core"></a><span data-ttu-id="4f44b-104">Samouczek: Utworzenie gRPC klienta i serwera w programie ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="4f44b-104">Tutorial: Create a gRPC client and server in ASP.NET Core</span></span>

<span data-ttu-id="4f44b-105">Przez [Luo Jan](https://github.com/juntaoluo)</span><span class="sxs-lookup"><span data-stu-id="4f44b-105">By [John Luo](https://github.com/juntaoluo)</span></span>

<span data-ttu-id="4f44b-106">W tym samouczku przedstawiono sposób tworzenia platformy .NET Core [gRPC](https://grpc.io/docs/guides/) klienta i ASP.NET Core gRPC serwera.</span><span class="sxs-lookup"><span data-stu-id="4f44b-106">This tutorial shows how to create a .NET Core [gRPC](https://grpc.io/docs/guides/) client and an ASP.NET Core gRPC Server.</span></span>

<span data-ttu-id="4f44b-107">Po zakończeniu będziesz mieć klienta gRPC, który komunikuje się z gRPC Greeter usługi.</span><span class="sxs-lookup"><span data-stu-id="4f44b-107">At the end, you'll have a gRPC client that communicates with the gRPC Greeter service.</span></span>

<span data-ttu-id="4f44b-108">[Wyświetlanie lub pobieranie przykładowego kodu](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/tutorials/grpc/grpc-start/sample) ([sposobu pobierania](xref:index#how-to-download-a-sample)).</span><span class="sxs-lookup"><span data-stu-id="4f44b-108">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/tutorials/grpc/grpc-start/sample) ([how to download](xref:index#how-to-download-a-sample)).</span></span>

<span data-ttu-id="4f44b-109">W ramach tego samouczka możesz:</span><span class="sxs-lookup"><span data-stu-id="4f44b-109">In this tutorial, you:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="4f44b-110">Utwórz gRPC serwera.</span><span class="sxs-lookup"><span data-stu-id="4f44b-110">Create a gRPC Server.</span></span>
> * <span data-ttu-id="4f44b-111">Tworzenie klienta gRPC.</span><span class="sxs-lookup"><span data-stu-id="4f44b-111">Create a gRPC client.</span></span>
> * <span data-ttu-id="4f44b-112">Przetestuj usługę klienta gRPC z gRPC Greeter usługi.</span><span class="sxs-lookup"><span data-stu-id="4f44b-112">Test the gRPC client service with the gRPC Greeter service.</span></span>

[!INCLUDE[](~/includes/net-core-prereqs-all-3.0.md)]

## <a name="create-a-grpc-service"></a><span data-ttu-id="4f44b-113">Tworzenie usługi gRPC</span><span class="sxs-lookup"><span data-stu-id="4f44b-113">Create a gRPC service</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="4f44b-114">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="4f44b-114">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="4f44b-115">W programie Visual Studio **pliku** menu, wybierz opcję **New** > **projektu**.</span><span class="sxs-lookup"><span data-stu-id="4f44b-115">From the Visual Studio **File** menu, select **New** > **Project**.</span></span>
* <span data-ttu-id="4f44b-116">W **Utwórz nowy projekt** okno dialogowe, wybierz opcję **aplikacji sieci Web programu ASP.NET Core**.</span><span class="sxs-lookup"><span data-stu-id="4f44b-116">In the **Create a new project** dialog, select **ASP.NET Core Web Application**.</span></span>
* <span data-ttu-id="4f44b-117">Wybierz **dalej**</span><span class="sxs-lookup"><span data-stu-id="4f44b-117">Select **Next**</span></span>
* <span data-ttu-id="4f44b-118">Nadaj projektowi nazwę **GrpcGreeter**.</span><span class="sxs-lookup"><span data-stu-id="4f44b-118">Name the project **GrpcGreeter**.</span></span> <span data-ttu-id="4f44b-119">Ważne jest, aby nadaj projektowi nazwę *GrpcGreeter* , przestrzenie nazw będą zgodne po skopiuj i Wklej kod.</span><span class="sxs-lookup"><span data-stu-id="4f44b-119">It's important to name the project *GrpcGreeter* so the namespaces will match when you copy and paste code.</span></span>
* <span data-ttu-id="4f44b-120">Wybierz pozycję **Utwórz**</span><span class="sxs-lookup"><span data-stu-id="4f44b-120">Select **Create**</span></span>
* <span data-ttu-id="4f44b-121">W **Tworzenie nowej aplikacji sieci Web platformy ASP.NET Core** okno dialogowe:</span><span class="sxs-lookup"><span data-stu-id="4f44b-121">In the **Create a new ASP.NET Core Web Application** dialog:</span></span>
  * <span data-ttu-id="4f44b-122">Wybierz **platformy .NET Core** i **platformy ASP.NET Core 3.0** w menu rozwijanych.</span><span class="sxs-lookup"><span data-stu-id="4f44b-122">Select **.NET Core** and **ASP.NET Core 3.0** in the dropdown menus.</span></span> 
  * <span data-ttu-id="4f44b-123">Wybierz **gRPC usługi** szablonu.</span><span class="sxs-lookup"><span data-stu-id="4f44b-123">Select the **gRPC Service** template.</span></span>
  * <span data-ttu-id="4f44b-124">Wybierz pozycję **Utwórz**</span><span class="sxs-lookup"><span data-stu-id="4f44b-124">Select **Create**</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="4f44b-125">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="4f44b-125">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="4f44b-126">Otwórz [zintegrowany terminal](https://code.visualstudio.com/docs/editor/integrated-terminal).</span><span class="sxs-lookup"><span data-stu-id="4f44b-126">Open the [integrated terminal](https://code.visualstudio.com/docs/editor/integrated-terminal).</span></span>
* <span data-ttu-id="4f44b-127">Zmień katalog (`cd`) do folderu, który będzie zawierać projekt.</span><span class="sxs-lookup"><span data-stu-id="4f44b-127">Change directories (`cd`) to a folder which will contain the project.</span></span>
* <span data-ttu-id="4f44b-128">Uruchom następujące polecenia:</span><span class="sxs-lookup"><span data-stu-id="4f44b-128">Run the following commands:</span></span>

  ```console
  dotnet new grpc -o GrpcGreeter
  code -r GrpcGreeter
  ```

  * <span data-ttu-id="4f44b-129">`dotnet new` Polecenie tworzy nową usługę gRPC w *GrpcGreeter* folderu.</span><span class="sxs-lookup"><span data-stu-id="4f44b-129">The `dotnet new` command creates a new gRPC service in the *GrpcGreeter* folder.</span></span>
  * <span data-ttu-id="4f44b-130">`code` Polecenia otwiera *GrpcGreeter* folderu w nowym wystąpieniu programu Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="4f44b-130">The `code` command opens the *GrpcGreeter* folder in a new instance of Visual Studio Code.</span></span>

  <span data-ttu-id="4f44b-131">Zostanie wyświetlone okno dialogowe z **"GrpcGreeter" brakuje wymagane zasoby do tworzenia i debugowania. Dodaj je?**</span><span class="sxs-lookup"><span data-stu-id="4f44b-131">A dialog box appears with **Required assets to build and debug are missing from 'GrpcGreeter'. Add them?**</span></span>
* <span data-ttu-id="4f44b-132">Wybierz **tak**</span><span class="sxs-lookup"><span data-stu-id="4f44b-132">Select **Yes**</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="4f44b-133">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="4f44b-133">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="4f44b-134">W terminalu uruchom następujące polecenia:</span><span class="sxs-lookup"><span data-stu-id="4f44b-134">From a terminal, run the following commands:</span></span>

```console
  dotnet new grpc -o GrpcGreeter
  cd GrpcGreeter
```

<span data-ttu-id="4f44b-135">Poprzednie polecenia użyj [interfejsu wiersza polecenia platformy .NET Core](/dotnet/core/tools/dotnet) utworzyć usługę gRPC.</span><span class="sxs-lookup"><span data-stu-id="4f44b-135">The preceding commands use the [.NET Core CLI](/dotnet/core/tools/dotnet) to create a gRPC service.</span></span>

### <a name="open-the-project"></a><span data-ttu-id="4f44b-136">Otwórz projekt</span><span class="sxs-lookup"><span data-stu-id="4f44b-136">Open the project</span></span>

<span data-ttu-id="4f44b-137">Z programu Visual Studio, wybierz **Plik > Otwórz**, a następnie wybierz pozycję *GrpcGreeter.sln* pliku.</span><span class="sxs-lookup"><span data-stu-id="4f44b-137">From Visual Studio, select **File > Open**, and then select the *GrpcGreeter.sln* file.</span></span>

<!-- End of VS tabs -->

---

### <a name="run-the-service"></a><span data-ttu-id="4f44b-138">Uruchom usługę</span><span class="sxs-lookup"><span data-stu-id="4f44b-138">Run the service</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="4f44b-139">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="4f44b-139">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="4f44b-140">Naciśnij klawisze Ctrl + F5, aby uruchomić usługę gRPC bez debugera.</span><span class="sxs-lookup"><span data-stu-id="4f44b-140">Press Ctrl+F5 to run the gRPC service without the debugger.</span></span>

  <span data-ttu-id="4f44b-141">Visual Studio uruchamia usługę w wierszu polecenia.</span><span class="sxs-lookup"><span data-stu-id="4f44b-141">Visual Studio runs the service in a command prompt.</span></span>

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[<span data-ttu-id="4f44b-142">Visual Studio Code / Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="4f44b-142">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

* <span data-ttu-id="4f44b-143">Uruchom projekt Greeter gRPC GrpcGreeter z wiersza polecenia przy użyciu `dotnet run`.</span><span class="sxs-lookup"><span data-stu-id="4f44b-143">Run the gRPC Greeter project GrpcGreeter from the command line using `dotnet run`.</span></span>

<!-- End of combined VS/Mac tabs -->

---

<span data-ttu-id="4f44b-144">Dzienniki przedstawiają nasłuchiwanie usługi na `http://localhost:50051`.</span><span class="sxs-lookup"><span data-stu-id="4f44b-144">The logs show the service listening on `http://localhost:50051`.</span></span>

```console
info: Microsoft.Hosting.Lifetime[0]
      Now listening on: http://localhost:50051
info: Microsoft.Hosting.Lifetime[0]
      Application started. Press Ctrl+C to shut down.
info: Microsoft.Hosting.Lifetime[0]
      Hosting environment: Development
info: Microsoft.Hosting.Lifetime[0]
```

### <a name="examine-the-project-files"></a><span data-ttu-id="4f44b-145">Przejrzyj pliki projektu</span><span class="sxs-lookup"><span data-stu-id="4f44b-145">Examine the project files</span></span>

<span data-ttu-id="4f44b-146">Pliki GrpcGreeter:</span><span class="sxs-lookup"><span data-stu-id="4f44b-146">GrpcGreeter files:</span></span>

* <span data-ttu-id="4f44b-147">*greet.proto*: *Protos/greet.proto* plik definiuje `Greeter` gRPC i służy do generowania gRPC zasoby serwera.</span><span class="sxs-lookup"><span data-stu-id="4f44b-147">*greet.proto*: The *Protos/greet.proto* file defines the `Greeter` gRPC and is used to generate the gRPC server assets.</span></span> <span data-ttu-id="4f44b-148">Aby uzyskać więcej informacji, zobacz [wprowadzenie do gRPC](xref:grpc/index).</span><span class="sxs-lookup"><span data-stu-id="4f44b-148">For more information, see [Introduction to gRPC](xref:grpc/index).</span></span>
* <span data-ttu-id="4f44b-149">*Usługi* folderu: Zawiera implementację `Greeter` usługi.</span><span class="sxs-lookup"><span data-stu-id="4f44b-149">*Services* folder: Contains the implementation of the `Greeter` service.</span></span>
* <span data-ttu-id="4f44b-150">*appSettings.json*: Zawiera dane konfiguracyjne, takie jak protokół używany przez Kestrel.</span><span class="sxs-lookup"><span data-stu-id="4f44b-150">*appSettings.json*: Contains configuration data, such as protocol used by Kestrel.</span></span> <span data-ttu-id="4f44b-151">Aby uzyskać więcej informacji, zobacz <xref:fundamentals/configuration/index>.</span><span class="sxs-lookup"><span data-stu-id="4f44b-151">For more information, see <xref:fundamentals/configuration/index>.</span></span>
* <span data-ttu-id="4f44b-152">*Program.cs*: Zawiera punkt wejścia dla usługi gRPC.</span><span class="sxs-lookup"><span data-stu-id="4f44b-152">*Program.cs*: Contains the entry point for the gRPC service.</span></span> <span data-ttu-id="4f44b-153">Aby uzyskać więcej informacji, zobacz <xref:fundamentals/host/web-host>.</span><span class="sxs-lookup"><span data-stu-id="4f44b-153">For more information, see <xref:fundamentals/host/web-host>.</span></span>
* <span data-ttu-id="4f44b-154">*Startup.cs*: Zawiera kod, który konfiguruje zachowanie aplikacji.</span><span class="sxs-lookup"><span data-stu-id="4f44b-154">*Startup.cs*: Contains code that configures app behavior.</span></span> <span data-ttu-id="4f44b-155">Aby uzyskać więcej informacji, zobacz [uruchamiania aplikacji](xref:fundamentals/startup).</span><span class="sxs-lookup"><span data-stu-id="4f44b-155">For more information, see [App startup](xref:fundamentals/startup).</span></span>

## <a name="create-the-grpc-client-in-a-net-console-app"></a><span data-ttu-id="4f44b-156">Tworzenie klienta gRPC w aplikacji konsoli .NET</span><span class="sxs-lookup"><span data-stu-id="4f44b-156">Create the gRPC client in a .NET console app</span></span>

## <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="4f44b-157">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="4f44b-157">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="4f44b-158">Wybierz **pliku** > **New** > **projektu** z paska menu.</span><span class="sxs-lookup"><span data-stu-id="4f44b-158">Select **File** > **New** > **Project** from the menu bar.</span></span>
* <span data-ttu-id="4f44b-159">W **Utwórz nowy projekt** okno dialogowe, wybierz opcję **Aplikacja konsoli (.NET Core)** .</span><span class="sxs-lookup"><span data-stu-id="4f44b-159">In the **Create a new project** dialog, select **Console App (.NET Core)**.</span></span>
* <span data-ttu-id="4f44b-160">Wybierz **dalej**</span><span class="sxs-lookup"><span data-stu-id="4f44b-160">Select **Next**</span></span>
* <span data-ttu-id="4f44b-161">W **nazwa** tekstu wprowadź "GrpcGreeterClient".</span><span class="sxs-lookup"><span data-stu-id="4f44b-161">In the **Name** text box, enter "GrpcGreeterClient".</span></span>
* <span data-ttu-id="4f44b-162">Wybierz pozycję **Utwórz**.</span><span class="sxs-lookup"><span data-stu-id="4f44b-162">Select **Create**.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="4f44b-163">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="4f44b-163">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="4f44b-164">Otwórz [zintegrowany terminal](https://code.visualstudio.com/docs/editor/integrated-terminal).</span><span class="sxs-lookup"><span data-stu-id="4f44b-164">Open the [integrated terminal](https://code.visualstudio.com/docs/editor/integrated-terminal).</span></span>
* <span data-ttu-id="4f44b-165">Zmień katalog (`cd`) do folderu, który będzie zawierać projekt.</span><span class="sxs-lookup"><span data-stu-id="4f44b-165">Change directories (`cd`) to a folder which will contain the project.</span></span>
* <span data-ttu-id="4f44b-166">Uruchom następujące polecenia:</span><span class="sxs-lookup"><span data-stu-id="4f44b-166">Run the following commands:</span></span>

```console
dotnet new console -o GrpcGreeterClient
code -r GrpcGreeterClient
```

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="4f44b-167">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="4f44b-167">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="4f44b-168">Postępuj zgodnie z instrukcjami [tutaj](/dotnet/core/tutorials/using-on-mac-vs-full-solution) do tworzenia aplikacji konsolowej o nazwie *GrpcGreeterClient*.</span><span class="sxs-lookup"><span data-stu-id="4f44b-168">Follow the instructions [here](/dotnet/core/tutorials/using-on-mac-vs-full-solution) to create a console app with the name *GrpcGreeterClient*.</span></span>

<!-- End of VS tabs -->

---

### <a name="add-required-packages"></a><span data-ttu-id="4f44b-169">Dodawanie wymaganych pakietów</span><span class="sxs-lookup"><span data-stu-id="4f44b-169">Add required packages</span></span>

<span data-ttu-id="4f44b-170">GRPC projekt klienta, należy dodać następujące pakiety:</span><span class="sxs-lookup"><span data-stu-id="4f44b-170">Add the following packages to the gRPC client project:</span></span>

* <span data-ttu-id="4f44b-171">[Grpc.Core](https://www.nuget.org/packages/Grpc.Core), który zawiera C# interfejsu API klienta C-core.</span><span class="sxs-lookup"><span data-stu-id="4f44b-171">[Grpc.Core](https://www.nuget.org/packages/Grpc.Core), which contains the C# API for the C-core client.</span></span>
* <span data-ttu-id="4f44b-172">[Google.Protobuf](https://www.nuget.org/packages/Google.Protobuf/), który zawiera protobuf komunikatu interfejsów API na potrzeby C#.</span><span class="sxs-lookup"><span data-stu-id="4f44b-172">[Google.Protobuf](https://www.nuget.org/packages/Google.Protobuf/), which contains protobuf message APIs for C#.</span></span>
* <span data-ttu-id="4f44b-173">[Grpc.Tools](https://www.nuget.org/packages/Grpc.Tools/), który zawiera C# narzędzia do obsługi formatu protobuf plików.</span><span class="sxs-lookup"><span data-stu-id="4f44b-173">[Grpc.Tools](https://www.nuget.org/packages/Grpc.Tools/), which contains C# tooling support for protobuf files.</span></span> <span data-ttu-id="4f44b-174">Pakiet narzędzi nie jest wymagane w czasie wykonywania, więc zależność jest oznaczona za pomocą `PrivateAssets="All"`.</span><span class="sxs-lookup"><span data-stu-id="4f44b-174">The tooling package isn't required at runtime, so the dependency is marked with `PrivateAssets="All"`.</span></span>

### <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="4f44b-175">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="4f44b-175">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="4f44b-176">Instalowanie pakietów przy użyciu konsoli Menedżera pakietów (PMC) "lub" Zarządzaj pakietami NuGet</span><span class="sxs-lookup"><span data-stu-id="4f44b-176">Install the packages using either the Package Manager Console (PMC) or Manage NuGet Package</span></span>

#### <a name="pmc-option-to-install-packages"></a><span data-ttu-id="4f44b-177">Konsolę zarządzania Pakietami opcję, aby zainstalować pakiety</span><span class="sxs-lookup"><span data-stu-id="4f44b-177">PMC option to install packages</span></span>

* <span data-ttu-id="4f44b-178">W programie Visual Studio, wybierz **narzędzia** > **Menedżera pakietów NuGet** > **Konsola Menedżera pakietów**</span><span class="sxs-lookup"><span data-stu-id="4f44b-178">From Visual Studio, select **Tools** > **NuGet Package Manager** > **Package Manager Console**</span></span>
* <span data-ttu-id="4f44b-179">Z **Konsola Menedżera pakietów** okna, przejdź do katalogu, w którym *GrpcGreeterClient.csproj* plik istnieje.</span><span class="sxs-lookup"><span data-stu-id="4f44b-179">From the **Package Manager Console** window, navigate to the directory in which the *GrpcGreeterClient.csproj* file exists.</span></span>
* <span data-ttu-id="4f44b-180">Uruchom następujące polecenia:</span><span class="sxs-lookup"><span data-stu-id="4f44b-180">Run the following commands:</span></span>

 ```powershell
Install-Package Grpc.Core
Install-Package Google.Protobuf
Install-Package Grpc.Tools
```

#### <a name="manage-nuget-packages-option-to-install-packages"></a><span data-ttu-id="4f44b-181">Zarządzanie pakietami NuGet opcję, aby zainstalować pakiety</span><span class="sxs-lookup"><span data-stu-id="4f44b-181">Manage NuGet Packages option to install packages</span></span>

* <span data-ttu-id="4f44b-182">Kliknij prawym przyciskiem myszy projekt w **Eksploratora rozwiązań** > **Zarządzaj pakietami NuGet**</span><span class="sxs-lookup"><span data-stu-id="4f44b-182">Right-click the project in **Solution Explorer** > **Manage NuGet Packages**</span></span>
* <span data-ttu-id="4f44b-183">Wybierz **Przeglądaj** kartę.</span><span class="sxs-lookup"><span data-stu-id="4f44b-183">Select the **Browse** tab.</span></span>
* <span data-ttu-id="4f44b-184">Wprowadź **Grpc.Core** w polu wyszukiwania.</span><span class="sxs-lookup"><span data-stu-id="4f44b-184">Enter **Grpc.Core** in the search box.</span></span>
* <span data-ttu-id="4f44b-185">Wybierz **Grpc.Core** pakietu z **Przeglądaj** kartę, a następnie wybierz pozycję **zainstalować**.</span><span class="sxs-lookup"><span data-stu-id="4f44b-185">Select the **Grpc.Core** package from the **Browse** tab and select **Install**.</span></span>
* <span data-ttu-id="4f44b-186">Powtórz tę procedurę dla `Google.Protobuf` i `Grpc.Tools`.</span><span class="sxs-lookup"><span data-stu-id="4f44b-186">Repeat for `Google.Protobuf` and `Grpc.Tools`.</span></span>

### <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="4f44b-187">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="4f44b-187">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="4f44b-188">Uruchom następujące polecenia z **zintegrowany Terminal**:</span><span class="sxs-lookup"><span data-stu-id="4f44b-188">Run the following commands from the **Integrated Terminal**:</span></span>

```console
dotnet add GrpcGreeterClient.csproj package Grpc.Core
dotnet add GrpcGreeterClient.csproj package Google.Protobuf
dotnet add GrpcGreeterClient.csproj package Grpc.Tools
```

### <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="4f44b-189">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="4f44b-189">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="4f44b-190">Kliknij prawym przyciskiem myszy **pakietów** folderu w **konsoli rozwiązania** > **Dodawanie pakietów**</span><span class="sxs-lookup"><span data-stu-id="4f44b-190">Right-click the **Packages** folder in **Solution Pad** > **Add Packages**</span></span>
* <span data-ttu-id="4f44b-191">Wprowadź **Grpc.Core** w polu wyszukiwania.</span><span class="sxs-lookup"><span data-stu-id="4f44b-191">Enter **Grpc.Core** in the search box.</span></span>
* <span data-ttu-id="4f44b-192">Wybierz **Grpc.Core** pakietu w okienku wyników, a następnie wybierz pozycję **Dodaj pakiet**</span><span class="sxs-lookup"><span data-stu-id="4f44b-192">Select the **Grpc.Core** package from the results pane and select **Add Package**</span></span>
* <span data-ttu-id="4f44b-193">Powtórz tę procedurę dla `Google.Protobuf` i `Grpc.Tools`.</span><span class="sxs-lookup"><span data-stu-id="4f44b-193">Repeat for `Google.Protobuf` and `Grpc.Tools`.</span></span>

---

### <a name="add-greetproto"></a><span data-ttu-id="4f44b-194">Dodaj greet.proto</span><span class="sxs-lookup"><span data-stu-id="4f44b-194">Add greet.proto</span></span>

* <span data-ttu-id="4f44b-195">Tworzenie **Protos** folderu w projekcie gRPC klienta.</span><span class="sxs-lookup"><span data-stu-id="4f44b-195">Create a **Protos** folder in the gRPC client project.</span></span>
* <span data-ttu-id="4f44b-196">Kopiuj **Protos\greet.proto** pliku z gRPC Greeter usługi do projektu klienta gRPC.</span><span class="sxs-lookup"><span data-stu-id="4f44b-196">Copy the **Protos\greet.proto** file from the gRPC Greeter service to the gRPC client project.</span></span>
* <span data-ttu-id="4f44b-197">Edytuj *GrpcGreeterClient.csproj* pliku projektu:</span><span class="sxs-lookup"><span data-stu-id="4f44b-197">Edit the *GrpcGreeterClient.csproj* project file:</span></span>

  # <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="4f44b-198">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="4f44b-198">Visual Studio</span></span>](#tab/visual-studio) 

  <span data-ttu-id="4f44b-199">Kliknij prawym przyciskiem myszy projekt i wybierz **Edytuj GrpcGreeterClient.csproj**.</span><span class="sxs-lookup"><span data-stu-id="4f44b-199">Right-click the project and select the **Edit GrpcGreeterClient.csproj**.</span></span>

  # <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="4f44b-200">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="4f44b-200">Visual Studio Code</span></span>](#tab/visual-studio-code) 

  <span data-ttu-id="4f44b-201">Wybierz *GrpcGreeterClient.csproj* pliku.</span><span class="sxs-lookup"><span data-stu-id="4f44b-201">Select the *GrpcGreeterClient.csproj* file.</span></span>

  # <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="4f44b-202">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="4f44b-202">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

  <span data-ttu-id="4f44b-203">Kliknij prawym przyciskiem myszy projekt i wybierz **Narzędzia > Edytuj plik**.</span><span class="sxs-lookup"><span data-stu-id="4f44b-203">Right click the project and select **Tools > Edit File**.</span></span>

  ---

* <span data-ttu-id="4f44b-204">Dodaj **greet.proto** plik `<Protobuf>` grupy elementów GrpcGreeterClient pliku projektu:</span><span class="sxs-lookup"><span data-stu-id="4f44b-204">Add the **greet.proto** file to the `<Protobuf>` item group of the GrpcGreeterClient project file:</span></span>

  ```XML
  <ItemGroup>
    <Protobuf Include="Protos\greet.proto" GrpcServices="Client" />
  </ItemGroup>
  ```

<span data-ttu-id="4f44b-205">Skompiluj projekt klienta, aby wyzwolić Generowanie C# zasobów klienta.</span><span class="sxs-lookup"><span data-stu-id="4f44b-205">Build the client project to trigger the generation of the C# client assets.</span></span>

### <a name="create-the-greater-client"></a><span data-ttu-id="4f44b-206">Utwórz większe klienta</span><span class="sxs-lookup"><span data-stu-id="4f44b-206">Create the greater client</span></span>

<span data-ttu-id="4f44b-207">Skompiluj projekt, aby utworzyć typy w **Greeter** przestrzeni nazw.</span><span class="sxs-lookup"><span data-stu-id="4f44b-207">Build the project to create the types in the **Greeter** namespace.</span></span> <span data-ttu-id="4f44b-208">`Greeter` Typy są generowane automatycznie przez proces kompilacji.</span><span class="sxs-lookup"><span data-stu-id="4f44b-208">The `Greeter` types are generated automatically by the build process.</span></span>

<span data-ttu-id="4f44b-209">Aktualizacja klienta gRPC *Program.cs* pliku następującym kodem:</span><span class="sxs-lookup"><span data-stu-id="4f44b-209">Update the gRPC client *Program.cs* file with the following code:</span></span>

[!code-cs[](~/tutorials/grpc/grpc-start/sample/GrpcGreeterClient/Program.cs?name=snippet2)]

<span data-ttu-id="4f44b-210">*Plik program.cs* zawiera punktu wejścia, a logika gRPC klienta.</span><span class="sxs-lookup"><span data-stu-id="4f44b-210">*Program.cs* contains the entry point and logic for the gRPC client.</span></span>

<span data-ttu-id="4f44b-211">Większa klienta jest tworzony przez:</span><span class="sxs-lookup"><span data-stu-id="4f44b-211">The greater client is created by:</span></span>

* <span data-ttu-id="4f44b-212">Utworzenie wystąpienia `Channel` zawierające informacje dotyczące tworzenia połączenia z usługą gRPC.</span><span class="sxs-lookup"><span data-stu-id="4f44b-212">Instantiating a `Channel` containing the information for creating the connection to the gRPC service.</span></span>
* <span data-ttu-id="4f44b-213">Za pomocą `Channel` do konstruowania większa klienta:</span><span class="sxs-lookup"><span data-stu-id="4f44b-213">Using the `Channel` to construct the greater client:</span></span>

[!code-cs[](~/tutorials/grpc/grpc-start/sample/GrpcGreeterClient/Program.cs?name=snippet&highlight=4-6)]

<span data-ttu-id="4f44b-214">Większa klient wywołuje asynchroniczną `SayHello` metody.</span><span class="sxs-lookup"><span data-stu-id="4f44b-214">The greater client calls the asynchronous `SayHello` method.</span></span> <span data-ttu-id="4f44b-215">Wynik `SayHello` wywołań jest wyświetlany:</span><span class="sxs-lookup"><span data-stu-id="4f44b-215">The result of the `SayHello` call is displayed:</span></span>

[!code-cs[](~/tutorials/grpc/grpc-start/sample/GrpcGreeterClient/Program.cs?name=snippet&highlight=7-9)]

<span data-ttu-id="4f44b-216">Zamknij `Channel` użytą przez klienta po zakończeniu operacji zwolnić wszystkie zasoby.</span><span class="sxs-lookup"><span data-stu-id="4f44b-216">Shut down the `Channel` used by the client when operations have finished to release all resources.</span></span>

## <a name="test-the-grpc-client-with-the-grpc-greeter-service"></a><span data-ttu-id="4f44b-217">Testowanie klienta gRPC gRPC Greeter usługi</span><span class="sxs-lookup"><span data-stu-id="4f44b-217">Test the gRPC client with the gRPC Greeter service</span></span>

### <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="4f44b-218">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="4f44b-218">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="4f44b-219">W usłudze Greeter naciśnij klawisze Ctrl + F5, aby uruchomić serwer bez debugera.</span><span class="sxs-lookup"><span data-stu-id="4f44b-219">In the Greeter service, press Ctrl+F5 to start the server without the debugger.</span></span>
* <span data-ttu-id="4f44b-220">W projekcie GrpcGreeterClient naciśnij klawisze Ctrl + F5, aby uruchomić serwer bez debugera.</span><span class="sxs-lookup"><span data-stu-id="4f44b-220">In the GrpcGreeterClient project, press Ctrl+F5 to start the server without the debugger.</span></span>

### <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[<span data-ttu-id="4f44b-221">Visual Studio Code / Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="4f44b-221">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

* <span data-ttu-id="4f44b-222">Uruchom usługę Greeter.</span><span class="sxs-lookup"><span data-stu-id="4f44b-222">Start the Greeter service.</span></span>
* <span data-ttu-id="4f44b-223">Uruchom klienta.</span><span class="sxs-lookup"><span data-stu-id="4f44b-223">Start the client.</span></span>

<!-- End of combined VS/Mac tabs -->

---

<span data-ttu-id="4f44b-224">Klient wysyła pozdrowienia z usługą za pomocą komunikatu zawierającego jego nazwę "GreeterClient".</span><span class="sxs-lookup"><span data-stu-id="4f44b-224">The client sends a greeting to the service with a message containing its name "GreeterClient".</span></span> <span data-ttu-id="4f44b-225">Usługa wysyła komunikat "Hello GreeterClient", jako odpowiedź.</span><span class="sxs-lookup"><span data-stu-id="4f44b-225">The service sends the message "Hello GreeterClient" as a response.</span></span> <span data-ttu-id="4f44b-226">Odpowiedź "Hello GreeterClient" jest wyświetlana w wierszu polecenia:</span><span class="sxs-lookup"><span data-stu-id="4f44b-226">The "Hello GreeterClient" response is displayed in the command prompt:</span></span>

```console
Greeting: Hello GreeterClient
Press any key to exit...
```

<span data-ttu-id="4f44b-227">Usługa gRPC rejestrujący szczegóły dotyczące pomyślnego wywołania w dziennikach, zapisywany wiersza polecenia.</span><span class="sxs-lookup"><span data-stu-id="4f44b-227">The gRPC service records the details of the successful call in the logs written to the command prompt.</span></span>

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

### <a name="next-steps"></a><span data-ttu-id="4f44b-228">Następne kroki</span><span class="sxs-lookup"><span data-stu-id="4f44b-228">Next steps</span></span>

* <xref:grpc/index>
* <xref:grpc/basics>
* <xref:grpc/migration>
