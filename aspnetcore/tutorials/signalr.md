---
title: 'Samouczek: Wprowadzenie do SignalR platformy ASP.NET Core'
author: tdykstra
description: W tym samouczku utworzysz aplikację rozmowy, która używa biblioteki SignalR platformy ASP.NET Core.
monikerRange: '>= aspnetcore-2.1'
ms.author: tdykstra
ms.custom: mvc
ms.date: 08/31/2018
uid: tutorials/signalr
ms.openlocfilehash: 6d96331a4630f766ca11edb056fd3e13b52b6ae4
ms.sourcegitcommit: 4cd8dce371d63a66d780e4af1baab2bcf9d61b24
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 09/06/2018
ms.locfileid: "43893168"
---
# <a name="tutorial-get-started-with-signalr-on-aspnet-core"></a><span data-ttu-id="65498-103">Samouczek: Wprowadzenie do SignalR platformy ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="65498-103">Tutorial: Get started with SignalR on ASP.NET Core</span></span>

<span data-ttu-id="65498-104">W tym samouczku pokazano podstawowe informacje dotyczące tworzenia aplikacji w czasie rzeczywistym przy użyciu biblioteki SignalR.</span><span class="sxs-lookup"><span data-stu-id="65498-104">This tutorial teaches the basics of building a real-time app using SignalR.</span></span> <span data-ttu-id="65498-105">Dowiesz się, jak:</span><span class="sxs-lookup"><span data-stu-id="65498-105">You learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="65498-106">Tworzenie aplikacji sieci web, która korzysta z biblioteki SignalR platformy ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="65498-106">Create a web app that uses SignalR on ASP.NET Core.</span></span>
> * <span data-ttu-id="65498-107">Utwórz Centrum SignalR na serwerze.</span><span class="sxs-lookup"><span data-stu-id="65498-107">Create a SignalR hub on the server.</span></span>
> * <span data-ttu-id="65498-108">Łączenie z Centrum SignalR poziomu klientów JavaScript.</span><span class="sxs-lookup"><span data-stu-id="65498-108">Connect to the SignalR hub from JavaScript clients.</span></span>
> * <span data-ttu-id="65498-109">Centrum umożliwia wysyłanie komunikatów za pomocą dowolnego klienta do wszystkich połączonych klientów.</span><span class="sxs-lookup"><span data-stu-id="65498-109">Use the hub to send messages from any client to all connected clients.</span></span>

<span data-ttu-id="65498-110">Po zakończeniu będziesz mieć działającą aplikację rozmowy:</span><span class="sxs-lookup"><span data-stu-id="65498-110">At the end, you'll have a working chat app:</span></span>

![SignalR przykładowej aplikacji](signalr/_static/signalr-get-started-finished.png)

<span data-ttu-id="65498-112">[Wyświetlanie lub pobieranie przykładowego kodu](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/signalr/sample) ([sposobu pobierania](xref:tutorials/index#how-to-download-a-sample)).</span><span class="sxs-lookup"><span data-stu-id="65498-112">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/signalr/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample)).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="65498-113">Wymagania wstępne</span><span class="sxs-lookup"><span data-stu-id="65498-113">Prerequisites</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="65498-114">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="65498-114">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="65498-115">[Visual Studio 2017 w wersji należy zachować 15,8 lub nowszej](https://www.visualstudio.com/downloads/) z **ASP.NET i tworzenie aplikacji internetowych** obciążenia</span><span class="sxs-lookup"><span data-stu-id="65498-115">[Visual Studio 2017 version 15.8 or later](https://www.visualstudio.com/downloads/) with the **ASP.NET and web development** workload</span></span>
* [<span data-ttu-id="65498-116">.NET core SDK 2.1 lub nowszej</span><span class="sxs-lookup"><span data-stu-id="65498-116">.NET Core SDK 2.1 or later</span></span>](https://www.microsoft.com/net/download/all)

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="65498-117">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="65498-117">Visual Studio Code</span></span>](#tab/visual-studio-code)

* [<span data-ttu-id="65498-118">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="65498-118">Visual Studio Code</span></span>](https://code.visualstudio.com/download)
* [<span data-ttu-id="65498-119">.NET core SDK 2.1 lub nowszej</span><span class="sxs-lookup"><span data-stu-id="65498-119">.NET Core SDK 2.1 or later</span></span>](https://www.microsoft.com/net/download/all)
* [<span data-ttu-id="65498-120">Środowisko C# dla programu Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="65498-120">C# for Visual Studio Code</span></span>](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp)

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="65498-121">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="65498-121">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* [<span data-ttu-id="65498-122">Program Visual Studio dla komputerów Mac w wersji 7.5.4 lub nowszy</span><span class="sxs-lookup"><span data-stu-id="65498-122">Visual Studio for Mac version 7.5.4 or later</span></span>](https://www.visualstudio.com/downloads/)
* <span data-ttu-id="65498-123">[.NET core SDK 2.1 lub nowszej](https://www.microsoft.com/net/download/all) (dołączone do instalacji programu Visual Studio)</span><span class="sxs-lookup"><span data-stu-id="65498-123">[.NET Core SDK 2.1 or later](https://www.microsoft.com/net/download/all) (included in the Visual Studio install)</span></span>

---

## <a name="create-the-project"></a><span data-ttu-id="65498-124">Utwórz projekt</span><span class="sxs-lookup"><span data-stu-id="65498-124">Create the project</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="65498-125">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="65498-125">Visual Studio</span></span>](#tab/visual-studio/)

* <span data-ttu-id="65498-126">W menu, wybierz opcję **Plik > Nowy projekt**.</span><span class="sxs-lookup"><span data-stu-id="65498-126">From the menu, select **File > New Project**.</span></span>

* <span data-ttu-id="65498-127">W **nowy projekt** okno dialogowe, wybierz opcję **zainstalowane > Visual C# > sieci Web > Aplikacja sieci Web programu ASP.NET Core**.</span><span class="sxs-lookup"><span data-stu-id="65498-127">In the **New Project** dialog, select **Installed > Visual C# > Web > ASP.NET Core Web Application**.</span></span> <span data-ttu-id="65498-128">Nadaj projektowi nazwę *SignalRChat*.</span><span class="sxs-lookup"><span data-stu-id="65498-128">Name the project *SignalRChat*.</span></span>

  ![Okno dialogowe nowego projektu w programie Visual Studio](signalr/_static/signalr-new-project-dialog.png)

* <span data-ttu-id="65498-130">Wybierz **aplikacji sieci Web** do tworzenia projektu, który używa stron Razor.</span><span class="sxs-lookup"><span data-stu-id="65498-130">Select **Web Application** to create a project that uses Razor Pages.</span></span>

* <span data-ttu-id="65498-131">Wybierz docelową platformę **platformy .NET Core**, wybierz opcję **platformy ASP.NET Core 2.1**i kliknij przycisk **OK**.</span><span class="sxs-lookup"><span data-stu-id="65498-131">Select a target framework of **.NET Core**, select **ASP.NET Core 2.1**, and click **OK**.</span></span>

  ![Okno dialogowe nowego projektu w programie Visual Studio](signalr/_static/signalr-new-project-choose-type.png)

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="65498-133">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="65498-133">Visual Studio Code</span></span>](#tab/visual-studio-code/)

* <span data-ttu-id="65498-134">Otwórz folder, który można użyć dla nowego projektu.</span><span class="sxs-lookup"><span data-stu-id="65498-134">Open a folder that you can use for a new project.</span></span>

* <span data-ttu-id="65498-135">W **zintegrowany Terminal**, uruchom następujące polecenie:</span><span class="sxs-lookup"><span data-stu-id="65498-135">In the **Integrated Terminal**, run the following command:</span></span>

   ```console
   dotnet new webapp -o SignalRChat
   ```

   [!INCLUDE[](~/includes/webapp-alias-notice.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="65498-136">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="65498-136">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="65498-137">W menu, wybierz opcję **Plik > nowe rozwiązanie**.</span><span class="sxs-lookup"><span data-stu-id="65498-137">From the menu, select **File > New Solution**.</span></span>

* <span data-ttu-id="65498-138">Wybierz **platformy .NET Core > aplikacji > aplikacji internetowej ASP.NET Core** (nie wybieraj **ASP.NET Core Web App (MVC)**).</span><span class="sxs-lookup"><span data-stu-id="65498-138">Select **.NET Core > App > ASP.NET Core Web App** (Don't select **ASP.NET Core Web App (MVC)**).</span></span>

* <span data-ttu-id="65498-139">Wybierz **dalej**.</span><span class="sxs-lookup"><span data-stu-id="65498-139">Select **Next**.</span></span>

* <span data-ttu-id="65498-140">Nadaj projektowi nazwę *SignalRChat*, a następnie wybierz pozycję **Utwórz**.</span><span class="sxs-lookup"><span data-stu-id="65498-140">Name the project *SignalRChat*, and then select **Create**.</span></span>

---

## <a name="add-the-signalr-client-library"></a><span data-ttu-id="65498-141">Dodaj bibliotekę klienta SignalR</span><span class="sxs-lookup"><span data-stu-id="65498-141">Add the SignalR client library</span></span>

<span data-ttu-id="65498-142">Serwer biblioteki SignalR znajduje się w [meta Microsoft.aspnetcore.all Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="65498-142">The SignalR server library is included in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span> <span data-ttu-id="65498-143">Biblioteki klienta JavaScript automatycznie nie jest zawarty w projekcie.</span><span class="sxs-lookup"><span data-stu-id="65498-143">The JavaScript client library isn't automatically included in the project.</span></span> <span data-ttu-id="65498-144">W tym samouczku użyjesz [Library Manager (LibMan)](xref:client-side/libman/index) można pobrać z biblioteki klienta z *unpkg*.</span><span class="sxs-lookup"><span data-stu-id="65498-144">For this tutorial, you use [Library Manager (LibMan)](xref:client-side/libman/index) to get the client library from *unpkg*.</span></span> <span data-ttu-id="65498-145">[unpkg](https://unpkg.com/#/) jest [usługa content delivery network](https://wikipedia.org/wiki/Content_delivery_network) , można dostarczać niczego w [npm, Menedżer pakietów Node.js](https://www.npmjs.com/get-npm).</span><span class="sxs-lookup"><span data-stu-id="65498-145">[unpkg](https://unpkg.com/#/) is a [content delivery network](https://wikipedia.org/wiki/Content_delivery_network) that can deliver anything found in [npm, the Node.js package manager](https://www.npmjs.com/get-npm).</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="65498-146">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="65498-146">Visual Studio</span></span>](#tab/visual-studio/)

* <span data-ttu-id="65498-147">W **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy projekt i wybierz **Dodaj** > **biblioteki po stronie klienta**.</span><span class="sxs-lookup"><span data-stu-id="65498-147">In **Solution Explorer**, right-click the project, and select **Add** > **Client-Side Library**.</span></span>

* <span data-ttu-id="65498-148">W **Dodaj biblioteki po stronie klienta** okno dialogowe, aby uzyskać **dostawcy** wybierz **unpkg**.</span><span class="sxs-lookup"><span data-stu-id="65498-148">In the **Add Client-Side Library** dialog, for **Provider** select **unpkg**.</span></span> 

* <span data-ttu-id="65498-149">Aby uzyskać **biblioteki**, wprowadź _@aspnet/signalr @1_i wybierz najnowszą wersję, która nie jest w wersji zapoznawczej.</span><span class="sxs-lookup"><span data-stu-id="65498-149">For **Library**, enter _@aspnet/signalr@1_, and select the latest version that isn't preview.</span></span>

  ![Dodaj okno dialogowe biblioteki po stronie klienta — Wybieranie biblioteki](signalr/_static/libman1.png)

* <span data-ttu-id="65498-151">Wybierz **wybierz konkretne pliki**, rozwiń węzeł *dist/przeglądarki* folder, a następnie wybierz *signalr.js* i *signalr.min.js*.</span><span class="sxs-lookup"><span data-stu-id="65498-151">Select **Choose specific files**, expand the *dist/browser* folder, and select *signalr.js* and *signalr.min.js*.</span></span>

* <span data-ttu-id="65498-152">Ustaw **lokalizacji docelowej** do *wwwroot/lib/signalr/* i wybierz **zainstalować**.</span><span class="sxs-lookup"><span data-stu-id="65498-152">Set **Target Location** to *wwwroot/lib/signalr/*, and select **Install**.</span></span>

  ![Dodaj okno dialogowe z biblioteki klienta - wybranie plików i miejsce docelowe](signalr/_static/libman2.png)

  <span data-ttu-id="65498-154">[LibMan](xref:client-side/libman/index) tworzy *wwwroot/lib/signalr* folder i kopiuje wybrane pliki.</span><span class="sxs-lookup"><span data-stu-id="65498-154">[LibMan](xref:client-side/libman/index) creates a *wwwroot/lib/signalr* folder and copies the selected files to it.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="65498-155">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="65498-155">Visual Studio Code</span></span>](#tab/visual-studio-code/)

* <span data-ttu-id="65498-156">W **zintegrowany Terminal**, uruchom następujące polecenie, aby zainstalować LibMan.</span><span class="sxs-lookup"><span data-stu-id="65498-156">In the **Integrated Terminal**, run the following command to install LibMan.</span></span>

  ```console
  dotnet tool install -g Microsoft.Web.LibraryManager.Cli
  ```

* <span data-ttu-id="65498-157">Przejdź do folderu projektu (zawierającego *SignalRChat.csproj* pliku).</span><span class="sxs-lookup"><span data-stu-id="65498-157">Navigate to the project folder (the one that contains the *SignalRChat.csproj* file).</span></span>

* <span data-ttu-id="65498-158">Uruchom następujące polecenie, aby przy użyciu LibMan uzyskać biblioteki klienta SignalR.</span><span class="sxs-lookup"><span data-stu-id="65498-158">Run the following command to get the SignalR client library by using LibMan.</span></span> <span data-ttu-id="65498-159">Być może musisz odczekaj kilka sekund, zanim dane wyjściowe.</span><span class="sxs-lookup"><span data-stu-id="65498-159">You might have to wait a few seconds before seeing output.</span></span>

  ```console
  libman install @aspnet/signalr -p unpkg -d wwwroot\lib\signalr --files dist/browser/signalr.js --files dist/browser/signalr.min.js
  ```

  <span data-ttu-id="65498-160">Parametry określ następujące opcje:</span><span class="sxs-lookup"><span data-stu-id="65498-160">The parameters specify the following options:</span></span>
  * <span data-ttu-id="65498-161">Użyj dostawcy unpkg.</span><span class="sxs-lookup"><span data-stu-id="65498-161">Use the unpkg provider.</span></span>
  * <span data-ttu-id="65498-162">Skopiuj pliki do *wwwroot/lib/signalr* docelowego.</span><span class="sxs-lookup"><span data-stu-id="65498-162">Copy files to the *wwwroot/lib/signalr* destination.</span></span>
  * <span data-ttu-id="65498-163">Skopiuj tylko określonych plików.</span><span class="sxs-lookup"><span data-stu-id="65498-163">Copy only the specified files.</span></span>

  <span data-ttu-id="65498-164">Dane wyjściowe wyglądają jak w poniższym przykładzie:</span><span class="sxs-lookup"><span data-stu-id="65498-164">The output looks like the following example:</span></span>

  ```console
  wwwroot/lib/signalr/dist/browser/signalr.js written to disk
  wwwroot/lib/signalr/dist/browser/signalr.min.js written to disk
  Installed library "@aspnet/signalr@1.0.3" to "wwwroot\lib\signalr"
  ```

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="65498-165">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="65498-165">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="65498-166">W **terminalu**, uruchom następujące polecenie, aby zainstalować LibMan.</span><span class="sxs-lookup"><span data-stu-id="65498-166">In the **Terminal**, run the following command to install LibMan.</span></span>

  ```console
  dotnet tool install -g Microsoft.Web.LibraryManager.Cli
  ```

* <span data-ttu-id="65498-167">Przejdź do folderu projektu (zawierającego *SignalRChat.csproj* pliku).</span><span class="sxs-lookup"><span data-stu-id="65498-167">Navigate to the project folder (the one that contains the *SignalRChat.csproj* file).</span></span>

* <span data-ttu-id="65498-168">Uruchom następujące polecenie, aby przy użyciu LibMan uzyskać biblioteki klienta SignalR.</span><span class="sxs-lookup"><span data-stu-id="65498-168">Run the following command to get the SignalR client library by using LibMan.</span></span>

  ```console
  libman install @aspnet/signalr -p unpkg -d wwwroot\lib\signalr --files dist/browser/signalr.js --files dist/browser/signalr.min.js
  ```

  <span data-ttu-id="65498-169">Parametry określ następujące opcje:</span><span class="sxs-lookup"><span data-stu-id="65498-169">The parameters specify the following options:</span></span>
  * <span data-ttu-id="65498-170">Użyj dostawcy unpkg.</span><span class="sxs-lookup"><span data-stu-id="65498-170">Use the unpkg provider.</span></span>
  * <span data-ttu-id="65498-171">Skopiuj pliki do *wwwroot/lib/signalr* docelowego.</span><span class="sxs-lookup"><span data-stu-id="65498-171">Copy files to the *wwwroot/lib/signalr* destination.</span></span>
  * <span data-ttu-id="65498-172">Skopiuj tylko określonych plików.</span><span class="sxs-lookup"><span data-stu-id="65498-172">Copy only the specified files.</span></span>

  <span data-ttu-id="65498-173">Dane wyjściowe wyglądają jak w poniższym przykładzie:</span><span class="sxs-lookup"><span data-stu-id="65498-173">The output looks like the following example:</span></span>

  ```console
  wwwroot/lib/signalr/dist/browser/signalr.js written to disk
  wwwroot/lib/signalr/dist/browser/signalr.min.js written to disk
  Installed library "@aspnet/signalr@1.0.3" to "wwwroot\lib\signalr"
  ```

---

## <a name="create-the-signalr-hub"></a><span data-ttu-id="65498-174">Tworzenie Centrum SignalR</span><span class="sxs-lookup"><span data-stu-id="65498-174">Create the SignalR hub</span></span>

<span data-ttu-id="65498-175">A [Centrum](xref:signalr/hubs) jest klasa, która służy jako ogólny potok, który obsługuje komunikację klient serwer.</span><span class="sxs-lookup"><span data-stu-id="65498-175">A [hub](xref:signalr/hubs) is a class that serves as a high-level pipeline that handles client-server communication.</span></span>

* <span data-ttu-id="65498-176">W folderze projektu SignalRChat Utwórz *koncentratory* folderu.</span><span class="sxs-lookup"><span data-stu-id="65498-176">In the SignalRChat project folder, create a *Hubs* folder.</span></span>

* <span data-ttu-id="65498-177">W *Hubs* folderze utwórz *ChatHub.cs* pliku następującym kodem:</span><span class="sxs-lookup"><span data-stu-id="65498-177">In the *Hubs* folder, create a *ChatHub.cs* file with the following code:</span></span>

  [!code-csharp[Startup](signalr/sample/Hubs/ChatHub.cs)]

  <span data-ttu-id="65498-178">`ChatHub` Klasa dziedziczy z elementu SignalR [Centrum](/dotnet/api/microsoft.aspnetcore.signalr.hub) klasy.</span><span class="sxs-lookup"><span data-stu-id="65498-178">The `ChatHub` class inherits from the SignalR [Hub](/dotnet/api/microsoft.aspnetcore.signalr.hub) class.</span></span> <span data-ttu-id="65498-179">`Hub` Klasa zarządza połączeń, grup i komunikatów.</span><span class="sxs-lookup"><span data-stu-id="65498-179">The `Hub` class manages connections, groups, and messaging.</span></span>

  <span data-ttu-id="65498-180">`SendMessage` Metoda może być wywoływana przez wszystkie połączone klienta.</span><span class="sxs-lookup"><span data-stu-id="65498-180">The `SendMessage` method can be called by any connected client.</span></span> <span data-ttu-id="65498-181">Wysyła odebranego komunikatu do wszystkich klientów.</span><span class="sxs-lookup"><span data-stu-id="65498-181">It sends the received message to all clients.</span></span> <span data-ttu-id="65498-182">Kod SignalR jest asynchroniczne w celu zapewnienia maksymalnej skalowalności.</span><span class="sxs-lookup"><span data-stu-id="65498-182">SignalR code is asynchronous to provide maximum scalability.</span></span>

## <a name="configure-the-project-to-use-signalr"></a><span data-ttu-id="65498-183">Konfigurowanie projektu do korzystania z biblioteki SignalR</span><span class="sxs-lookup"><span data-stu-id="65498-183">Configure the project to use SignalR</span></span>

<span data-ttu-id="65498-184">Serwer biblioteki SignalR musi być skonfigurowany do przekazywania żądań SignalR z SignalR.</span><span class="sxs-lookup"><span data-stu-id="65498-184">The SignalR server must be configured to pass SignalR requests to SignalR.</span></span>

* <span data-ttu-id="65498-185">Dodaj następujący wyróżniony kod do *Startup.cs* pliku.</span><span class="sxs-lookup"><span data-stu-id="65498-185">Add the following highlighted code to the *Startup.cs* file.</span></span>

  [!code-csharp[Startup](signalr/sample/Startup.cs?highlight=7,33,52-55)]

  <span data-ttu-id="65498-186">Te zmiany dodanie SignalR do [wstrzykiwanie zależności](xref:fundamentals/dependency-injection) systemu i [oprogramowania pośredniczącego](xref:fundamentals/middleware/index) potoku.</span><span class="sxs-lookup"><span data-stu-id="65498-186">These changes add SignalR to the [dependency injection](xref:fundamentals/dependency-injection) system and the [middleware](xref:fundamentals/middleware/index) pipeline.</span></span>

## <a name="create-the-signalr-client-code"></a><span data-ttu-id="65498-187">Tworzenie kodu klienta SignalR</span><span class="sxs-lookup"><span data-stu-id="65498-187">Create the SignalR client code</span></span>

* <span data-ttu-id="65498-188">Zastąp zawartość *Pages\Index.cshtml* następującym kodem:</span><span class="sxs-lookup"><span data-stu-id="65498-188">Replace the content in *Pages\Index.cshtml* with the following code:</span></span>

  [!code-cshtml[Index](signalr/sample/Pages/Index.cshtml)]

  <span data-ttu-id="65498-189">Powyższy kod:</span><span class="sxs-lookup"><span data-stu-id="65498-189">The preceding code:</span></span>

  * <span data-ttu-id="65498-190">Tworzy pola tekstowe nazwy i tekst wiadomość i przycisk Prześlij.</span><span class="sxs-lookup"><span data-stu-id="65498-190">Creates text boxes for name and message text, and a submit button.</span></span>
  * <span data-ttu-id="65498-191">Tworzy listę z `id="messagesList"` do wyświetlania komunikatów odebranych z Centrum SignalR.</span><span class="sxs-lookup"><span data-stu-id="65498-191">Creates a list with `id="messagesList"` for displaying messages that are received from the SignalR hub.</span></span>
  * <span data-ttu-id="65498-192">Zawiera odwołania do skryptu z SignalR i *chat.js* kod aplikacji, który zostanie utworzony w następnym kroku.</span><span class="sxs-lookup"><span data-stu-id="65498-192">Includes script references to SignalR and the *chat.js* application code that you create in the next step.</span></span>

* <span data-ttu-id="65498-193">W *wwwroot/js* folderze utwórz *chat.js* pliku następującym kodem:</span><span class="sxs-lookup"><span data-stu-id="65498-193">In the *wwwroot/js* folder, create a *chat.js* file with the following code:</span></span>

  [!code-javascript[Index](signalr/sample/wwwroot/js/chat.js)]

  <span data-ttu-id="65498-194">Powyższy kod:</span><span class="sxs-lookup"><span data-stu-id="65498-194">The preceding code:</span></span>

  * <span data-ttu-id="65498-195">Tworzy i uruchamia połączenie.</span><span class="sxs-lookup"><span data-stu-id="65498-195">Creates and starts a connection.</span></span>
  * <span data-ttu-id="65498-196">Dodaje przycisk przesyłania program obsługi, która wysyła komunikaty do Centrum.</span><span class="sxs-lookup"><span data-stu-id="65498-196">Adds to the submit button a handler that sends messages to the hub.</span></span>
  * <span data-ttu-id="65498-197">Dodaje do obiektu połączenia program obsługi, który odbiera komunikaty z Centrum i dodaje je do listy.</span><span class="sxs-lookup"><span data-stu-id="65498-197">Adds to the connection object a handler that receives messages from the hub and adds them to the list.</span></span>

## <a name="run-the-app"></a><span data-ttu-id="65498-198">Uruchamianie aplikacji</span><span class="sxs-lookup"><span data-stu-id="65498-198">Run the app</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="65498-199">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="65498-199">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="65498-200">Naciśnij klawisz **kombinację klawiszy CTRL + F5** Aby uruchomić aplikację bez debugowania.</span><span class="sxs-lookup"><span data-stu-id="65498-200">Press **CTRL+F5** to run the app without debugging.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="65498-201">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="65498-201">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="65498-202">Naciśnij klawisz **kombinację klawiszy CTRL + F5** Aby uruchomić aplikację bez debugowania.</span><span class="sxs-lookup"><span data-stu-id="65498-202">Press **CTRL+F5** to run the app without debugging.</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="65498-203">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="65498-203">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="65498-204">W menu, wybierz opcję **Uruchom > Uruchom bez debugowania**.</span><span class="sxs-lookup"><span data-stu-id="65498-204">From the menu, select **Run > Start Without Debugging**.</span></span>

---

* <span data-ttu-id="65498-205">Skopiuj adres URL z paska adresu, a następnie otwórz innego wystąpienia przeglądarki lub karty i wklej adres URL w pasku adresu.</span><span class="sxs-lookup"><span data-stu-id="65498-205">Copy the URL from the address bar, open another browser instance or tab, and paste the URL in the address bar.</span></span>

* <span data-ttu-id="65498-206">Wybierz albo przeglądarki, wprowadź nazwę i komunikat, a następnie wybierz **wysyłania** przycisku.</span><span class="sxs-lookup"><span data-stu-id="65498-206">Choose either browser, enter a name and message, and select the **Send** button.</span></span>

  <span data-ttu-id="65498-207">Nazwa i wiadomości są wyświetlane na obu stronach natychmiast.</span><span class="sxs-lookup"><span data-stu-id="65498-207">The name and message are displayed on both pages instantly.</span></span>

  ![SignalR przykładowej aplikacji](signalr/_static/signalr-get-started-finished.png)

> [!TIP]
> <span data-ttu-id="65498-209">Jeśli aplikacja nie działa, Otwórz swoje narzędzia deweloperskie przeglądarki (F12) i przejdź do konsoli.</span><span class="sxs-lookup"><span data-stu-id="65498-209">If the app doesn't work, open your browser developer tools (F12) and go to the console.</span></span> <span data-ttu-id="65498-210">Może pojawić się błędy związane z Twoim kodem HTML i JavaScript.</span><span class="sxs-lookup"><span data-stu-id="65498-210">You might see errors related to your HTML and JavaScript code.</span></span> <span data-ttu-id="65498-211">Załóżmy, że możesz umieścić *signalr.js* w innym folderze niż bezpośredniego.</span><span class="sxs-lookup"><span data-stu-id="65498-211">For example, suppose you put *signalr.js* in a different folder than directed.</span></span> <span data-ttu-id="65498-212">W takim przypadku odwołania do tego pliku nie będzie działać, a następnie zostanie wyświetlony błąd 404 w konsoli.</span><span class="sxs-lookup"><span data-stu-id="65498-212">In that case the reference to that file won't work and you'll see a 404 error in the console.</span></span>
> <span data-ttu-id="65498-213">![błąd — nie znaleziono signalr.js](signalr/_static/f12-console.png)</span><span class="sxs-lookup"><span data-stu-id="65498-213">![signalr.js not found error](signalr/_static/f12-console.png)</span></span>

## <a name="next-steps"></a><span data-ttu-id="65498-214">Następne kroki</span><span class="sxs-lookup"><span data-stu-id="65498-214">Next steps</span></span>

<span data-ttu-id="65498-215">Klientom na łączenie się z aplikacji SignalR w różnych domenach, należy włączyć udostępnianie zasobów między źródłami (CORS).</span><span class="sxs-lookup"><span data-stu-id="65498-215">If you want clients to connect to a SignalR app from different domains, you have to enable Cross-Origin Resource Sharing (CORS).</span></span> <span data-ttu-id="65498-216">Aby uzyskać więcej informacji, zobacz [Cross-origin resource sharing](xref:signalr/security?view=aspnetcore-2.1#cross-origin-resource-sharing).</span><span class="sxs-lookup"><span data-stu-id="65498-216">For more information, see [Cross-origin resource sharing](xref:signalr/security?view=aspnetcore-2.1#cross-origin-resource-sharing).</span></span>

<span data-ttu-id="65498-217">Aby dowiedzieć się więcej na temat biblioteki SignalR, koncentratorów i klientów języka JavaScript, zobacz następujące zasoby:</span><span class="sxs-lookup"><span data-stu-id="65498-217">To learn more about SignalR, hubs, and JavaScript clients, see these resources:</span></span>

* [<span data-ttu-id="65498-218">Wprowadzenie do SignalR dla platformy ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="65498-218">Introduction to SignalR for ASP.NET Core</span></span>](xref:signalr/introduction)
* [<span data-ttu-id="65498-219">Na użytek koncentratory w SignalR platformy ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="65498-219">Use hubs in SignalR for ASP.NET Core</span></span>](xref:signalr/hubs)
* [<span data-ttu-id="65498-220">ASP.NET Core SignalR JavaScript klienta</span><span class="sxs-lookup"><span data-stu-id="65498-220">ASP.NET Core SignalR JavaScript client</span></span>](xref:signalr/javascript-client)
