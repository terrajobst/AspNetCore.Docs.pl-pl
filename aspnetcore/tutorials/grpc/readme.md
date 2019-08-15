---
page_type: sample
description: W tym samouczku pokazano, jak utworzyć usługę gRPC i klienta gRPC na ASP.NET Core.
languages:
- csharp
products:
- dotnet-core
- aspnet-core
- vs
urlFragment: create-grpc-client
ms.openlocfilehash: a281adc3b1fe90eeb32c1185750f911af683af83
ms.sourcegitcommit: 476ea5ad86a680b7b017c6f32098acd3414c0f6c
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/14/2019
ms.locfileid: "69029094"
---
# <a name="create-a-grpc-client-and-server-in-aspnet-core-30-using-visual-studio"></a><span data-ttu-id="d38a1-102">Tworzenie gRPC klienta i serwera w ASP.NET Core 3,0 przy użyciu programu Visual Studio</span><span class="sxs-lookup"><span data-stu-id="d38a1-102">Create a gRPC client and server in ASP.NET Core 3.0 using Visual Studio</span></span>

<span data-ttu-id="d38a1-103">W tym samouczku pokazano, jak utworzyć klienta programu .NET Core [gRPC](https://grpc.io/docs/guides/) oraz serwer gRPC ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="d38a1-103">This tutorial shows how to create a .NET Core [gRPC](https://grpc.io/docs/guides/) client and an ASP.NET Core gRPC Server.</span></span>

<span data-ttu-id="d38a1-104">Na koniec będziesz mieć klienta gRPC, który komunikuje się z usługą gRPC Greeter.</span><span class="sxs-lookup"><span data-stu-id="d38a1-104">At the end, you'll have a gRPC client that communicates with the gRPC Greeter service.</span></span>

<span data-ttu-id="d38a1-105">W tym samouczku:</span><span class="sxs-lookup"><span data-stu-id="d38a1-105">In this tutorial, you;</span></span>

* <span data-ttu-id="d38a1-106">Utwórz serwer gRPC.</span><span class="sxs-lookup"><span data-stu-id="d38a1-106">Create a gRPC Server.</span></span>
* <span data-ttu-id="d38a1-107">Utwórz klienta gRPC.</span><span class="sxs-lookup"><span data-stu-id="d38a1-107">Create a gRPC client.</span></span>
* <span data-ttu-id="d38a1-108">Przetestuj usługę klienta gRPC za pomocą usługi gRPC Greeter.</span><span class="sxs-lookup"><span data-stu-id="d38a1-108">Test the gRPC client service with the gRPC Greeter service.</span></span>

## <a name="create-a-grpc-service"></a><span data-ttu-id="d38a1-109">Tworzenie usługi gRPC</span><span class="sxs-lookup"><span data-stu-id="d38a1-109">Create a gRPC service</span></span>

* <span data-ttu-id="d38a1-110">W programie Visual Studio **pliku** menu, wybierz opcję **New** > **projektu**.</span><span class="sxs-lookup"><span data-stu-id="d38a1-110">From the Visual Studio **File** menu, select **New** > **Project**.</span></span>
* <span data-ttu-id="d38a1-111">W oknie dialogowym **Tworzenie nowego projektu** wybierz pozycję **ASP.NET Core aplikacji sieci Web**.</span><span class="sxs-lookup"><span data-stu-id="d38a1-111">In the **Create a new project** dialog, select **ASP.NET Core Web Application**.</span></span>
* <span data-ttu-id="d38a1-112">Wybierz pozycję **dalej**</span><span class="sxs-lookup"><span data-stu-id="d38a1-112">Select **Next**</span></span>
* <span data-ttu-id="d38a1-113">Nazwij projekt **GrpcGreeter**.</span><span class="sxs-lookup"><span data-stu-id="d38a1-113">Name the project **GrpcGreeter**.</span></span> <span data-ttu-id="d38a1-114">Ważne jest, aby nazwa projektu *GrpcGreeter* , tak aby przestrzenie nazw były zgodne podczas kopiowania i wklejania kodu.</span><span class="sxs-lookup"><span data-stu-id="d38a1-114">It's important to name the project *GrpcGreeter* so the namespaces will match when you copy and paste code.</span></span>
* <span data-ttu-id="d38a1-115">Wybierz pozycję **Utwórz**</span><span class="sxs-lookup"><span data-stu-id="d38a1-115">Select **Create**</span></span>
* <span data-ttu-id="d38a1-116">W oknie dialogowym **Tworzenie nowej ASP.NET Core aplikacji sieci Web** :</span><span class="sxs-lookup"><span data-stu-id="d38a1-116">In the **Create a new ASP.NET Core Web Application** dialog:</span></span>
  * <span data-ttu-id="d38a1-117">W menu rozwijanym wybierz pozycję **.NET Core** i **ASP.NET Core 3,0** .</span><span class="sxs-lookup"><span data-stu-id="d38a1-117">Select **.NET Core** and **ASP.NET Core 3.0** in the dropdown menus.</span></span> 
  * <span data-ttu-id="d38a1-118">Wybierz szablon **usługi gRPC** .</span><span class="sxs-lookup"><span data-stu-id="d38a1-118">Select the **gRPC Service** template.</span></span>
  * <span data-ttu-id="d38a1-119">Wybierz pozycję **Utwórz**</span><span class="sxs-lookup"><span data-stu-id="d38a1-119">Select **Create**</span></span>

### <a name="run-the-service"></a><span data-ttu-id="d38a1-120">Uruchamianie usługi</span><span class="sxs-lookup"><span data-stu-id="d38a1-120">Run the service</span></span>

* <span data-ttu-id="d38a1-121">Naciśnij `Ctrl+F5` klawisz, aby uruchomić usługę gRPC bez debugera.</span><span class="sxs-lookup"><span data-stu-id="d38a1-121">Press `Ctrl+F5` to run the gRPC service without the debugger.</span></span>

  <span data-ttu-id="d38a1-122">Program Visual Studio uruchamia usługę w wierszu polecenia.</span><span class="sxs-lookup"><span data-stu-id="d38a1-122">Visual Studio runs the service in a command prompt.</span></span>

<span data-ttu-id="d38a1-123">Dzienniki pokazują, że usługa nasłuchuje `https://localhost:5001`.</span><span class="sxs-lookup"><span data-stu-id="d38a1-123">The logs show the service listening on `https://localhost:5001`.</span></span>

```console
info: Microsoft.Hosting.Lifetime[0]
      Now listening on: https://localhost:5001
info: Microsoft.Hosting.Lifetime[0]
      Application started. Press Ctrl+C to shut down.
info: Microsoft.Hosting.Lifetime[0]
      Hosting environment: Development
info: Microsoft.Hosting.Lifetime[0]
```

> [!NOTE]
> <span data-ttu-id="d38a1-124">Szablon gRPC jest skonfigurowany do korzystania z [Transport Layer Security (TLS)](https://tools.ietf.org/html/rfc5246).</span><span class="sxs-lookup"><span data-stu-id="d38a1-124">The gRPC template is configured to use [Transport Layer Security (TLS)](https://tools.ietf.org/html/rfc5246).</span></span> <span data-ttu-id="d38a1-125">Klienci gRPC muszą używać protokołu HTTPS, aby wywołać serwer.</span><span class="sxs-lookup"><span data-stu-id="d38a1-125">gRPC clients need to use HTTPS to call the server.</span></span>
>
> <span data-ttu-id="d38a1-126">Program macOS nie obsługuje ASP.NET Core gRPC z protokołem TLS.</span><span class="sxs-lookup"><span data-stu-id="d38a1-126">macOS doesn't support ASP.NET Core gRPC with TLS.</span></span> <span data-ttu-id="d38a1-127">Aby pomyślnie uruchomić usługi gRPC Services w usłudze macOS, wymagana jest dodatkowa konfiguracja.</span><span class="sxs-lookup"><span data-stu-id="d38a1-127">Additional configuration is required to successfully run gRPC services on macOS.</span></span> <span data-ttu-id="d38a1-128">Aby uzyskać więcej informacji, zobacz [gRPC i ASP.NET Core on macOS](xref:grpc/aspnetcore#grpc-and-aspnet-core-on-macos).</span><span class="sxs-lookup"><span data-stu-id="d38a1-128">For more information, see [gRPC and ASP.NET Core on macOS](xref:grpc/aspnetcore#grpc-and-aspnet-core-on-macos).</span></span>

### <a name="examine-the-project-files"></a><span data-ttu-id="d38a1-129">Sprawdzanie plików projektu</span><span class="sxs-lookup"><span data-stu-id="d38a1-129">Examine the project files</span></span>

<span data-ttu-id="d38a1-130">*GrpcGreeter* pliki projektu:</span><span class="sxs-lookup"><span data-stu-id="d38a1-130">*GrpcGreeter* project files:</span></span>

* <span data-ttu-id="d38a1-131">*Greeting. proto*: Plik *Protos/Greeting. proto* definiuje `Greeter` gRPC i służy do generowania zasobów serwera gRPC.</span><span class="sxs-lookup"><span data-stu-id="d38a1-131">*greet.proto*: The *Protos/greet.proto* file defines the `Greeter` gRPC and is used to generate the gRPC server assets.</span></span> <span data-ttu-id="d38a1-132">Aby uzyskać więcej informacji, zobacz [wprowadzenie do gRPC](xref:grpc/index).</span><span class="sxs-lookup"><span data-stu-id="d38a1-132">For more information, see [Introduction to gRPC](xref:grpc/index).</span></span>
* <span data-ttu-id="d38a1-133">Folder *usługi* : Zawiera implementację `Greeter` usługi.</span><span class="sxs-lookup"><span data-stu-id="d38a1-133">*Services* folder: Contains the implementation of the `Greeter` service.</span></span>
* <span data-ttu-id="d38a1-134">*appSettings.json*: Zawiera dane konfiguracyjne, takie jak protokół używany przez Kestrel.</span><span class="sxs-lookup"><span data-stu-id="d38a1-134">*appSettings.json*: Contains configuration data, such as protocol used by Kestrel.</span></span> <span data-ttu-id="d38a1-135">Aby uzyskać więcej informacji, zobacz <xref:fundamentals/configuration/index>.</span><span class="sxs-lookup"><span data-stu-id="d38a1-135">For more information, see <xref:fundamentals/configuration/index>.</span></span>
* <span data-ttu-id="d38a1-136">*Program.cs*: Zawiera punkt wejścia dla usługi gRPC.</span><span class="sxs-lookup"><span data-stu-id="d38a1-136">*Program.cs*: Contains the entry point for the gRPC service.</span></span> <span data-ttu-id="d38a1-137">Aby uzyskać więcej informacji, zobacz <xref:fundamentals/host/generic-host>.</span><span class="sxs-lookup"><span data-stu-id="d38a1-137">For more information, see <xref:fundamentals/host/generic-host>.</span></span>
* <span data-ttu-id="d38a1-138">*Startup.cs*: Zawiera kod, który konfiguruje zachowanie aplikacji.</span><span class="sxs-lookup"><span data-stu-id="d38a1-138">*Startup.cs*: Contains code that configures app behavior.</span></span> <span data-ttu-id="d38a1-139">Aby uzyskać więcej informacji, zobacz [Uruchamianie aplikacji](xref:fundamentals/startup).</span><span class="sxs-lookup"><span data-stu-id="d38a1-139">For more information, see [App startup](xref:fundamentals/startup).</span></span>

## <a name="create-the-grpc-client-in-a-net-console-app"></a><span data-ttu-id="d38a1-140">Tworzenie klienta gRPC w aplikacji konsolowej .NET</span><span class="sxs-lookup"><span data-stu-id="d38a1-140">Create the gRPC client in a .NET console app</span></span>

* <span data-ttu-id="d38a1-141">Otwórz drugie wystąpienie programu Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="d38a1-141">Open a second instance of Visual Studio.</span></span>
* <span data-ttu-id="d38a1-142">Na pasku menu wybierz pozycję **plik** > **Nowy** > **projekt** .</span><span class="sxs-lookup"><span data-stu-id="d38a1-142">Select **File** > **New** > **Project** from the menu bar.</span></span>
* <span data-ttu-id="d38a1-143">W oknie dialogowym **Tworzenie nowego projektu** wybierz pozycję **Aplikacja konsolowa (.NET Core)** .</span><span class="sxs-lookup"><span data-stu-id="d38a1-143">In the **Create a new project** dialog, select **Console App (.NET Core)**.</span></span>
* <span data-ttu-id="d38a1-144">Wybierz pozycję **dalej**</span><span class="sxs-lookup"><span data-stu-id="d38a1-144">Select **Next**</span></span>
* <span data-ttu-id="d38a1-145">W polu tekstowym **Nazwa** wprowadź wartość "GrpcGreeterClient".</span><span class="sxs-lookup"><span data-stu-id="d38a1-145">In the **Name** text box, enter "GrpcGreeterClient".</span></span>
* <span data-ttu-id="d38a1-146">Wybierz pozycję **Utwórz**.</span><span class="sxs-lookup"><span data-stu-id="d38a1-146">Select **Create**.</span></span>

### <a name="add-required-packages"></a><span data-ttu-id="d38a1-147">Dodaj wymagane pakiety</span><span class="sxs-lookup"><span data-stu-id="d38a1-147">Add required packages</span></span>

<span data-ttu-id="d38a1-148">Projekt klienta gRPC wymaga następujących pakietów:</span><span class="sxs-lookup"><span data-stu-id="d38a1-148">The gRPC client project requires the following packages:</span></span>

* <span data-ttu-id="d38a1-149">[GRPC .NET. Client](https://www.nuget.org/packages/Grpc.Net.Client), który zawiera klienta .NET Core.</span><span class="sxs-lookup"><span data-stu-id="d38a1-149">[Grpc.Net.Client](https://www.nuget.org/packages/Grpc.Net.Client), which contains the .NET Core client.</span></span>
* <span data-ttu-id="d38a1-150">[Google. protobuf](https://www.nuget.org/packages/Google.Protobuf/), który zawiera interfejsy API komunikatów protobuf C#dla.</span><span class="sxs-lookup"><span data-stu-id="d38a1-150">[Google.Protobuf](https://www.nuget.org/packages/Google.Protobuf/), which contains protobuf message APIs for C#.</span></span>
* <span data-ttu-id="d38a1-151">[GRPC. Tools](https://www.nuget.org/packages/Grpc.Tools/), która zawiera C# obsługę narzędzi dla plików protobuf.</span><span class="sxs-lookup"><span data-stu-id="d38a1-151">[Grpc.Tools](https://www.nuget.org/packages/Grpc.Tools/), which contains C# tooling support for protobuf files.</span></span> <span data-ttu-id="d38a1-152">Pakiet narzędzi nie jest wymagany w czasie wykonywania, dlatego zależność jest oznaczona za pomocą `PrivateAssets="All"`.</span><span class="sxs-lookup"><span data-stu-id="d38a1-152">The tooling package isn't required at runtime, so the dependency is marked with `PrivateAssets="All"`.</span></span>

<span data-ttu-id="d38a1-153">Zainstaluj pakiety przy użyciu konsoli Menedżera pakietów (PMC) lub Zarządzaj pakietami NuGet.</span><span class="sxs-lookup"><span data-stu-id="d38a1-153">Install the packages using either the Package Manager Console (PMC) or Manage NuGet Packages.</span></span>

#### <a name="pmc-option-to-install-packages"></a><span data-ttu-id="d38a1-154">Opcja PMC, aby zainstalować pakiety</span><span class="sxs-lookup"><span data-stu-id="d38a1-154">PMC option to install packages</span></span>

* <span data-ttu-id="d38a1-155">W programie Visual Studio wybierz kolejno pozycje **Narzędzia** > Menedżer**pakietów** > NuGet**konsola Menedżera pakietów**</span><span class="sxs-lookup"><span data-stu-id="d38a1-155">From Visual Studio, select **Tools** > **NuGet Package Manager** > **Package Manager Console**</span></span>
* <span data-ttu-id="d38a1-156">W oknie **konsola Menedżera pakietów** przejdź do katalogu, w którym znajduje się plik *GrpcGreeterClient. csproj* .</span><span class="sxs-lookup"><span data-stu-id="d38a1-156">From the **Package Manager Console** window, navigate to the directory in which the *GrpcGreeterClient.csproj* file exists.</span></span>
* <span data-ttu-id="d38a1-157">Uruchom następujące polecenia:</span><span class="sxs-lookup"><span data-stu-id="d38a1-157">Run the following commands:</span></span>

 ```powershell
Install-Package Grpc.Net.Client
Install-Package Google.Protobuf
Install-Package Grpc.Tools
```

#### <a name="manage-nuget-packages-option-to-install-packages"></a><span data-ttu-id="d38a1-158">Opcja zarządzania pakietami NuGet w celu zainstalowania pakietów</span><span class="sxs-lookup"><span data-stu-id="d38a1-158">Manage NuGet Packages option to install packages</span></span>

* <span data-ttu-id="d38a1-159">Kliknij prawym przyciskiem myszy projekt w **Eksplorator rozwiązań** > **Zarządzanie pakietami NuGet**</span><span class="sxs-lookup"><span data-stu-id="d38a1-159">Right-click the project in **Solution Explorer** > **Manage NuGet Packages**</span></span>
* <span data-ttu-id="d38a1-160">Wybierz kartę **przeglądanie** .</span><span class="sxs-lookup"><span data-stu-id="d38a1-160">Select the **Browse** tab.</span></span>
* <span data-ttu-id="d38a1-161">W polu wyszukiwania wprowadź **GRPC .NET. Client** .</span><span class="sxs-lookup"><span data-stu-id="d38a1-161">Enter **Grpc.Net.Client** in the search box.</span></span>
* <span data-ttu-id="d38a1-162">Wybierz pakiet **GRPC .NET. Client** z karty **Przeglądaj** i wybierz pozycję **Zainstaluj**.</span><span class="sxs-lookup"><span data-stu-id="d38a1-162">Select the **Grpc.Net.Client** package from the **Browse** tab and select **Install**.</span></span>
* <span data-ttu-id="d38a1-163">Powtórz dla `Google.Protobuf` i `Grpc.Tools`.</span><span class="sxs-lookup"><span data-stu-id="d38a1-163">Repeat for `Google.Protobuf` and `Grpc.Tools`.</span></span>

### <a name="add-greetproto"></a><span data-ttu-id="d38a1-164">Dodaj Greeting. proto</span><span class="sxs-lookup"><span data-stu-id="d38a1-164">Add greet.proto</span></span>

* <span data-ttu-id="d38a1-165">Utwórz folder **Protos** w projekcie klienta gRPC.</span><span class="sxs-lookup"><span data-stu-id="d38a1-165">Create a **Protos** folder in the gRPC client project.</span></span>
* <span data-ttu-id="d38a1-166">Skopiuj plik **Protos\greet.proto** z usługi gRPC Greeter do projektu klienta gRPC.</span><span class="sxs-lookup"><span data-stu-id="d38a1-166">Copy the **Protos\greet.proto** file from the gRPC Greeter service to the gRPC client project.</span></span>
* <span data-ttu-id="d38a1-167">Edytuj plik projektu *GrpcGreeterClient. csproj* :</span><span class="sxs-lookup"><span data-stu-id="d38a1-167">Edit the *GrpcGreeterClient.csproj* project file:</span></span>

  <span data-ttu-id="d38a1-168">Kliknij prawym przyciskiem myszy projekt i wybierz polecenie **Edytuj plik projektu**.</span><span class="sxs-lookup"><span data-stu-id="d38a1-168">Right-click the project and select **Edit Project File**.</span></span>

* <span data-ttu-id="d38a1-169">Dodaj grupę elementów z `<Protobuf>` elementem, który odwołuje się do pliku **Greeting. proto** :</span><span class="sxs-lookup"><span data-stu-id="d38a1-169">Add an item group with a `<Protobuf>` element that refers to the **greet.proto** file:</span></span>

  ```xml
  <ItemGroup>
    <Protobuf Include="Protos\greet.proto" GrpcServices="Client" />
  </ItemGroup>
  ```

### <a name="create-the-greeter-client"></a><span data-ttu-id="d38a1-170">Tworzenie klienta Greeter</span><span class="sxs-lookup"><span data-stu-id="d38a1-170">Create the Greeter client</span></span>

<span data-ttu-id="d38a1-171">Skompiluj projekt, aby utworzyć typy w `GrpcGreeter` przestrzeni nazw.</span><span class="sxs-lookup"><span data-stu-id="d38a1-171">Build the project to create the types in the `GrpcGreeter` namespace.</span></span> <span data-ttu-id="d38a1-172">`GrpcGreeter` Typy są generowane automatycznie przez proces kompilacji.</span><span class="sxs-lookup"><span data-stu-id="d38a1-172">The `GrpcGreeter` types are generated automatically by the build process.</span></span>

<span data-ttu-id="d38a1-173">Zaktualizuj plik *program.cs* klienta gRPC za pomocą następującego kodu:</span><span class="sxs-lookup"><span data-stu-id="d38a1-173">Update the gRPC client *Program.cs* file with the following code:</span></span>

```csharp
using System;
using System.Net.Http;
using System.Threading.Tasks;
using GrpcGreeter;
using Grpc.Net.Client;

namespace GrpcGreeterClient
{
    class Program
    {
        static async Task Main(string[] args)
        {
            var httpClient = new HttpClient();
            // The port number(5001) must match the port of the gRPC server.
            httpClient.BaseAddress = new Uri("https://localhost:5001");
            var client = GrpcClient.Create<Greeter.GreeterClient>(httpClient);
            var reply = await client.SayHelloAsync(
                              new HelloRequest { Name = "GreeterClient" });
            Console.WriteLine("Greeting: " + reply.Message);
            Console.WriteLine("Press any key to exit...");
            Console.ReadKey();
        }
    }
}
```

<span data-ttu-id="d38a1-174">*Program.cs* zawiera punkt wejścia i logikę dla klienta gRPC.</span><span class="sxs-lookup"><span data-stu-id="d38a1-174">*Program.cs* contains the entry point and logic for the gRPC client.</span></span>

<span data-ttu-id="d38a1-175">Klient Greeter jest tworzony przez:</span><span class="sxs-lookup"><span data-stu-id="d38a1-175">The Greeter client is created by:</span></span>

* <span data-ttu-id="d38a1-176">Utworzenie wystąpienia zawierającego informacje dotyczące tworzenia połączenia z usługą gRPC. `HttpClient`</span><span class="sxs-lookup"><span data-stu-id="d38a1-176">Instantiating an `HttpClient` containing the information for creating the connection to the gRPC service.</span></span>
* <span data-ttu-id="d38a1-177">Korzystanie z programu do konstruowania klienta Greeter: `HttpClient`</span><span class="sxs-lookup"><span data-stu-id="d38a1-177">Using the `HttpClient` to construct the Greeter client:</span></span>

```csharp
static async Task Main(string[] args)
{
    var httpClient = new HttpClient();
    // The port number(5001) must match the port of the gRPC server.
    httpClient.BaseAddress = new Uri("https://localhost:5001");
    var client = GrpcClient.Create<Greeter.GreeterClient>(httpClient);
    var reply = await client.SayHelloAsync(
                      new HelloRequest { Name = "GreeterClient" });
    Console.WriteLine("Greeting: " + reply.Message);
    Console.WriteLine("Press any key to exit...");
    Console.ReadKey();
}
```

<span data-ttu-id="d38a1-178">Klient Greeter wywołuje metodę asynchroniczną `SayHello` .</span><span class="sxs-lookup"><span data-stu-id="d38a1-178">The Greeter client calls the asynchronous `SayHello` method.</span></span> <span data-ttu-id="d38a1-179">Zostanie wyświetlony wynik `SayHello` wywołania:</span><span class="sxs-lookup"><span data-stu-id="d38a1-179">The result of the `SayHello` call is displayed:</span></span>

```csharp
static async Task Main(string[] args)
{
    var httpClient = new HttpClient();
    // The port number(5001) must match the port of the gRPC server.
    httpClient.BaseAddress = new Uri("https://localhost:5001");
    var client = GrpcClient.Create<Greeter.GreeterClient>(httpClient);
    var reply = await client.SayHelloAsync(
                      new HelloRequest { Name = "GreeterClient" });
    Console.WriteLine("Greeting: " + reply.Message);
    Console.WriteLine("Press any key to exit...");
    Console.ReadKey();
}
```

## <a name="test-the-grpc-client-with-the-grpc-greeter-service"></a><span data-ttu-id="d38a1-180">Testowanie klienta gRPC za pomocą usługi gRPC Greeter</span><span class="sxs-lookup"><span data-stu-id="d38a1-180">Test the gRPC client with the gRPC Greeter service</span></span>

* <span data-ttu-id="d38a1-181">W usłudze Greeter naciśnij klawisz `Ctrl+F5` , aby uruchomić serwer bez debugera.</span><span class="sxs-lookup"><span data-stu-id="d38a1-181">In the Greeter service, press `Ctrl+F5` to start the server without the debugger.</span></span>
* <span data-ttu-id="d38a1-182">W projekcie naciśnij klawisz `Ctrl+F5` , aby uruchomić klienta bez debugera. `GrpcGreeterClient`</span><span class="sxs-lookup"><span data-stu-id="d38a1-182">In the `GrpcGreeterClient` project, press `Ctrl+F5` to start the client without the debugger.</span></span>

<span data-ttu-id="d38a1-183">Klient wysyła powitanie do usługi z komunikatem zawierającym nazwę "GreeterClient".</span><span class="sxs-lookup"><span data-stu-id="d38a1-183">The client sends a greeting to the service with a message containing its name "GreeterClient".</span></span> <span data-ttu-id="d38a1-184">Usługa wysyła komunikat "Hello GreeterClient" jako odpowiedź.</span><span class="sxs-lookup"><span data-stu-id="d38a1-184">The service sends the message "Hello GreeterClient" as a response.</span></span> <span data-ttu-id="d38a1-185">Odpowiedź "Hello GreeterClient" jest wyświetlana w wierszu polecenia:</span><span class="sxs-lookup"><span data-stu-id="d38a1-185">The "Hello GreeterClient" response is displayed in the command prompt:</span></span>

```console
Greeting: Hello GreeterClient
Press any key to exit...
```

<span data-ttu-id="d38a1-186">Usługa gRPC rejestruje szczegóły pomyślnego wywołania w dziennikach zapisanych w wierszu polecenia.</span><span class="sxs-lookup"><span data-stu-id="d38a1-186">The gRPC service records the details of the successful call in the logs written to the command prompt.</span></span>

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

### <a name="docs-help--next-steps-for-grpc"></a><span data-ttu-id="d38a1-187">Dokumentacja pomocy & następnych kroków dla gRPC</span><span class="sxs-lookup"><span data-stu-id="d38a1-187">Docs help & next steps for gRPC</span></span>

* [<span data-ttu-id="d38a1-188">Wprowadzenie do gRPC na ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="d38a1-188">Introduction to gRPC on ASP.NET Core</span></span>](https://docs.microsoft.com/aspnet/core/grpc/index?view=aspnetcore-3.0)
* [<span data-ttu-id="d38a1-189">usługi gRPC zC#</span><span class="sxs-lookup"><span data-stu-id="d38a1-189">gRPC services with C#</span></span>](https://docs.microsoft.com/aspnet/core/grpc/basics?view=aspnetcore-3.0)
* [<span data-ttu-id="d38a1-190">Migrowanie usług gRPC z dysku C-Core do ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="d38a1-190">Migrating gRPC services from C-core to ASP.NET Core</span></span>](https://docs.microsoft.com/aspnet/core/grpc/migration?view=aspnetcore-3.0)
