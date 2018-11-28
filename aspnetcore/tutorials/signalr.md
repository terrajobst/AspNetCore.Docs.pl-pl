---
title: Wprowadzenie do biblioteki SignalR platformy ASP.NET Core
author: tdykstra
description: W tym samouczku utworzysz aplikację rozmowy, która korzysta z biblioteki SignalR platformy ASP.NET Core.
monikerRange: '>= aspnetcore-2.1'
ms.author: tdykstra
ms.custom: mvc
ms.date: 11/13/2018
uid: tutorials/signalr
ms.openlocfilehash: 190717dc6e6f9f2766ba92aa7472f4cdea9b6827
ms.sourcegitcommit: e7fafb153b9de7595c2558a0133f8d1c33a3bddb
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/28/2018
ms.locfileid: "52458533"
---
# <a name="tutorial-get-started-with-aspnet-core-signalr"></a><span data-ttu-id="1415a-103">Samouczek: Rozpoczynanie pracy przy użyciu biblioteki SignalR platformy ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="1415a-103">Tutorial: Get started with ASP.NET Core SignalR</span></span>

<span data-ttu-id="1415a-104">W tym samouczku pokazano podstawowe informacje dotyczące tworzenia aplikacji w czasie rzeczywistym przy użyciu biblioteki SignalR.</span><span class="sxs-lookup"><span data-stu-id="1415a-104">This tutorial teaches the basics of building a real-time app using SignalR.</span></span> <span data-ttu-id="1415a-105">Dowiesz się, jak:</span><span class="sxs-lookup"><span data-stu-id="1415a-105">You learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="1415a-106">Utwórz projekt sieci web.</span><span class="sxs-lookup"><span data-stu-id="1415a-106">Create a web project.</span></span>
> * <span data-ttu-id="1415a-107">Dodaj bibliotekę klienta SignalR.</span><span class="sxs-lookup"><span data-stu-id="1415a-107">Add the SignalR client library.</span></span>
> * <span data-ttu-id="1415a-108">Utwórz Centrum SignalR.</span><span class="sxs-lookup"><span data-stu-id="1415a-108">Create a SignalR hub.</span></span>
> * <span data-ttu-id="1415a-109">Skonfiguruj projekt do korzystania z SignalR.</span><span class="sxs-lookup"><span data-stu-id="1415a-109">Configure the project to use SignalR.</span></span>
> * <span data-ttu-id="1415a-110">Dodaj kod, który wysyła komunikaty z dowolnego klienta do wszystkich połączonych klientów.</span><span class="sxs-lookup"><span data-stu-id="1415a-110">Add code that sends messages from any client to all connected clients.</span></span>

<span data-ttu-id="1415a-111">Po zakończeniu będziesz mieć działającą aplikację rozmowy:</span><span class="sxs-lookup"><span data-stu-id="1415a-111">At the end, you'll have a working chat app:</span></span>

![SignalR przykładowej aplikacji](signalr/_static/signalr-get-started-finished.png)

<span data-ttu-id="1415a-113">[Wyświetlanie lub pobieranie przykładowego kodu](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/signalr/sample) ([sposobu pobierania](xref:index#how-to-download-a-sample)).</span><span class="sxs-lookup"><span data-stu-id="1415a-113">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/signalr/sample) ([how to download](xref:index#how-to-download-a-sample)).</span></span>

> [!NOTE]
> <span data-ttu-id="1415a-114">W tej chwili testujemy użyteczność nowo zaproponowanej struktury spisu treści dla dokumentacji platformy ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="1415a-114">We’re testing the usability of a proposed new structure for the ASP.NET Core table of contents.</span></span>  <span data-ttu-id="1415a-115">Jeśli możesz poświęcić chwilę na wykonanie ćwiczenia polegającego na znalezieniu 7 różnych tematów w aktualnym lub zaproponowanym spisie treści, [kliknij tutaj i weź udział w badaniu](https://dpk4xbh5.optimalworkshop.com/treejack/rps16hd5).</span><span class="sxs-lookup"><span data-stu-id="1415a-115">If you have a few minutes to try an exercise of finding 7 different topics in the current or proposed table of contents, please [click here to participate in the study](https://dpk4xbh5.optimalworkshop.com/treejack/rps16hd5).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="1415a-116">Wymagania wstępne</span><span class="sxs-lookup"><span data-stu-id="1415a-116">Prerequisites</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="1415a-117">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="1415a-117">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="1415a-118">[Visual Studio 2017 w wersji należy zachować 15,8 lub nowszej](https://www.visualstudio.com/downloads/) z **ASP.NET i tworzenie aplikacji internetowych** obciążenia</span><span class="sxs-lookup"><span data-stu-id="1415a-118">[Visual Studio 2017 version 15.8 or later](https://www.visualstudio.com/downloads/) with the **ASP.NET and web development** workload</span></span>
* [<span data-ttu-id="1415a-119">.NET core SDK 2.1 lub nowszej</span><span class="sxs-lookup"><span data-stu-id="1415a-119">.NET Core SDK 2.1 or later</span></span>](https://www.microsoft.com/net/download/all)

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="1415a-120">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="1415a-120">Visual Studio Code</span></span>](#tab/visual-studio-code)

* [<span data-ttu-id="1415a-121">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="1415a-121">Visual Studio Code</span></span>](https://code.visualstudio.com/download)
* [<span data-ttu-id="1415a-122">.NET core SDK 2.1 lub nowszej</span><span class="sxs-lookup"><span data-stu-id="1415a-122">.NET Core SDK 2.1 or later</span></span>](https://www.microsoft.com/net/download/all)
* [<span data-ttu-id="1415a-123">Środowisko C# dla programu Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="1415a-123">C# for Visual Studio Code</span></span>](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp)

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="1415a-124">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="1415a-124">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* [<span data-ttu-id="1415a-125">Program Visual Studio dla komputerów Mac w wersji 7.5.4 lub nowszy</span><span class="sxs-lookup"><span data-stu-id="1415a-125">Visual Studio for Mac version 7.5.4 or later</span></span>](https://www.visualstudio.com/downloads/)
* <span data-ttu-id="1415a-126">[.NET core SDK 2.1 lub nowszej](https://www.microsoft.com/net/download/all) (dołączone do instalacji programu Visual Studio)</span><span class="sxs-lookup"><span data-stu-id="1415a-126">[.NET Core SDK 2.1 or later](https://www.microsoft.com/net/download/all) (included in the Visual Studio install)</span></span>

---

## <a name="create-a-web-project"></a><span data-ttu-id="1415a-127">Tworzenie projektu sieci web</span><span class="sxs-lookup"><span data-stu-id="1415a-127">Create a web project</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="1415a-128">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="1415a-128">Visual Studio</span></span>](#tab/visual-studio/)

* <span data-ttu-id="1415a-129">W menu, wybierz opcję **Plik > Nowy projekt**.</span><span class="sxs-lookup"><span data-stu-id="1415a-129">From the menu, select **File > New Project**.</span></span>

* <span data-ttu-id="1415a-130">W **nowy projekt** okno dialogowe, wybierz opcję **zainstalowane > Visual C# > sieci Web > Aplikacja sieci Web programu ASP.NET Core**.</span><span class="sxs-lookup"><span data-stu-id="1415a-130">In the **New Project** dialog, select **Installed > Visual C# > Web > ASP.NET Core Web Application**.</span></span> <span data-ttu-id="1415a-131">Nadaj projektowi nazwę *SignalRChat*.</span><span class="sxs-lookup"><span data-stu-id="1415a-131">Name the project *SignalRChat*.</span></span>

  ![Okno dialogowe nowego projektu w programie Visual Studio](signalr/_static/signalr-new-project-dialog.png)

* <span data-ttu-id="1415a-133">Wybierz **aplikacji sieci Web** do tworzenia projektu, który używa stron Razor.</span><span class="sxs-lookup"><span data-stu-id="1415a-133">Select **Web Application** to create a project that uses Razor Pages.</span></span>

* <span data-ttu-id="1415a-134">Wybierz docelową platformę **platformy .NET Core**, wybierz opcję **platformy ASP.NET Core 2.1**i kliknij przycisk **OK**.</span><span class="sxs-lookup"><span data-stu-id="1415a-134">Select a target framework of **.NET Core**, select **ASP.NET Core 2.1**, and click **OK**.</span></span>

  ![Okno dialogowe nowego projektu w programie Visual Studio](signalr/_static/signalr-new-project-choose-type.png)

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="1415a-136">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="1415a-136">Visual Studio Code</span></span>](#tab/visual-studio-code/)

* <span data-ttu-id="1415a-137">Otwórz [zintegrowany terminal](https://code.visualstudio.com/docs/editor/integrated-terminal) do folderu, w którym zostanie utworzony nowy folder projektu.</span><span class="sxs-lookup"><span data-stu-id="1415a-137">Open the [integrated terminal](https://code.visualstudio.com/docs/editor/integrated-terminal) to the folder in which the new project folder will be created.</span></span>

* <span data-ttu-id="1415a-138">Uruchom następujące polecenia:</span><span class="sxs-lookup"><span data-stu-id="1415a-138">Run the following commands:</span></span>

   ```console
   dotnet new webapp -o SignalRChat
   code -r SignalRChat
   ```

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="1415a-139">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="1415a-139">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="1415a-140">W menu, wybierz opcję **Plik > nowe rozwiązanie**.</span><span class="sxs-lookup"><span data-stu-id="1415a-140">From the menu, select **File > New Solution**.</span></span>

* <span data-ttu-id="1415a-141">Wybierz **platformy .NET Core > aplikacji > aplikacji internetowej ASP.NET Core** (nie wybieraj **ASP.NET Core Web App (MVC)**).</span><span class="sxs-lookup"><span data-stu-id="1415a-141">Select **.NET Core > App > ASP.NET Core Web App** (Don't select **ASP.NET Core Web App (MVC)**).</span></span>

* <span data-ttu-id="1415a-142">Wybierz **dalej**.</span><span class="sxs-lookup"><span data-stu-id="1415a-142">Select **Next**.</span></span>

* <span data-ttu-id="1415a-143">Nadaj projektowi nazwę *SignalRChat*, a następnie wybierz pozycję **Utwórz**.</span><span class="sxs-lookup"><span data-stu-id="1415a-143">Name the project *SignalRChat*, and then select **Create**.</span></span>

---

## <a name="add-the-signalr-client-library"></a><span data-ttu-id="1415a-144">Dodaj bibliotekę klienta SignalR</span><span class="sxs-lookup"><span data-stu-id="1415a-144">Add the SignalR client library</span></span>

<span data-ttu-id="1415a-145">Serwer biblioteki SignalR znajduje się w `Microsoft.AspNetCore.App` meta Microsoft.aspnetcore.all.</span><span class="sxs-lookup"><span data-stu-id="1415a-145">The SignalR server library is included in the `Microsoft.AspNetCore.App` metapackage.</span></span> <span data-ttu-id="1415a-146">Biblioteki klienta JavaScript automatycznie nie jest zawarty w projekcie.</span><span class="sxs-lookup"><span data-stu-id="1415a-146">The JavaScript client library isn't automatically included in the project.</span></span> <span data-ttu-id="1415a-147">W tym samouczku używasz Library Manager (LibMan) można pobrać z biblioteki klienta z *unpkg*.</span><span class="sxs-lookup"><span data-stu-id="1415a-147">For this tutorial, you use Library Manager (LibMan) to get the client library from *unpkg*.</span></span> <span data-ttu-id="1415a-148">unpkg jest usługa content delivery network (CDN)), można dostarczać niczego w Menedżer pakietów npm oraz Menedżera pakietów środowiska Node.js.</span><span class="sxs-lookup"><span data-stu-id="1415a-148">unpkg is a content delivery network (CDN)) that can deliver anything found in npm, the Node.js package manager.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="1415a-149">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="1415a-149">Visual Studio</span></span>](#tab/visual-studio/)

* <span data-ttu-id="1415a-150">W **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy projekt i wybierz **Dodaj** > **biblioteki po stronie klienta**.</span><span class="sxs-lookup"><span data-stu-id="1415a-150">In **Solution Explorer**, right-click the project, and select **Add** > **Client-Side Library**.</span></span>

* <span data-ttu-id="1415a-151">W **Dodaj biblioteki po stronie klienta** okno dialogowe, aby uzyskać **dostawcy** wybierz **unpkg**.</span><span class="sxs-lookup"><span data-stu-id="1415a-151">In the **Add Client-Side Library** dialog, for **Provider** select **unpkg**.</span></span> 

* <span data-ttu-id="1415a-152">Aby uzyskać **biblioteki**, wprowadź `@aspnet/signalr@1`i wybierz najnowszą wersję, która nie jest w wersji zapoznawczej.</span><span class="sxs-lookup"><span data-stu-id="1415a-152">For **Library**, enter `@aspnet/signalr@1`, and select the latest version that isn't preview.</span></span>

  ![Dodaj okno dialogowe biblioteki po stronie klienta — Wybieranie biblioteki](signalr/_static/libman1.png)

* <span data-ttu-id="1415a-154">Wybierz **wybierz konkretne pliki**, rozwiń węzeł *dist/przeglądarki* folder, a następnie wybierz *signalr.js* i *signalr.min.js*.</span><span class="sxs-lookup"><span data-stu-id="1415a-154">Select **Choose specific files**, expand the *dist/browser* folder, and select *signalr.js* and *signalr.min.js*.</span></span>

* <span data-ttu-id="1415a-155">Ustaw **lokalizacji docelowej** do *wwwroot/lib/signalr/* i wybierz **zainstalować**.</span><span class="sxs-lookup"><span data-stu-id="1415a-155">Set **Target Location** to *wwwroot/lib/signalr/*, and select **Install**.</span></span>

  ![Dodaj okno dialogowe z biblioteki klienta - wybranie plików i miejsce docelowe](signalr/_static/libman2.png)

  <span data-ttu-id="1415a-157">Tworzy LibMan *wwwroot/lib/signalr* folder i kopiuje wybrane pliki.</span><span class="sxs-lookup"><span data-stu-id="1415a-157">LibMan creates a *wwwroot/lib/signalr* folder and copies the selected files to it.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="1415a-158">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="1415a-158">Visual Studio Code</span></span>](#tab/visual-studio-code/)

* <span data-ttu-id="1415a-159">W zintegrowanym terminalu uruchom następujące polecenie, aby zainstalować LibMan.</span><span class="sxs-lookup"><span data-stu-id="1415a-159">In the integrated terminal, run the following command to install LibMan.</span></span>

  ```console
  dotnet tool install -g Microsoft.Web.LibraryManager.Cli
  ```

* <span data-ttu-id="1415a-160">Uruchom następujące polecenie, aby przy użyciu LibMan uzyskać biblioteki klienta SignalR.</span><span class="sxs-lookup"><span data-stu-id="1415a-160">Run the following command to get the SignalR client library by using LibMan.</span></span> <span data-ttu-id="1415a-161">Być może musisz odczekaj kilka sekund, zanim dane wyjściowe.</span><span class="sxs-lookup"><span data-stu-id="1415a-161">You might have to wait a few seconds before seeing output.</span></span>

  ```console
  libman install @aspnet/signalr -p unpkg -d wwwroot/lib/signalr --files dist/browser/signalr.js --files dist/browser/signalr.min.js
  ```

  <span data-ttu-id="1415a-162">Parametry określ następujące opcje:</span><span class="sxs-lookup"><span data-stu-id="1415a-162">The parameters specify the following options:</span></span>
  * <span data-ttu-id="1415a-163">Użyj dostawcy unpkg.</span><span class="sxs-lookup"><span data-stu-id="1415a-163">Use the unpkg provider.</span></span>
  * <span data-ttu-id="1415a-164">Skopiuj pliki do *wwwroot/lib/signalr* docelowego.</span><span class="sxs-lookup"><span data-stu-id="1415a-164">Copy files to the *wwwroot/lib/signalr* destination.</span></span>
  * <span data-ttu-id="1415a-165">Skopiuj tylko określonych plików.</span><span class="sxs-lookup"><span data-stu-id="1415a-165">Copy only the specified files.</span></span>

  <span data-ttu-id="1415a-166">Dane wyjściowe wyglądają jak w poniższym przykładzie:</span><span class="sxs-lookup"><span data-stu-id="1415a-166">The output looks like the following example:</span></span>

  ```console
  wwwroot/lib/signalr/dist/browser/signalr.js written to disk
  wwwroot/lib/signalr/dist/browser/signalr.min.js written to disk
  Installed library "@aspnet/signalr@1.0.3" to "wwwroot/lib/signalr"
  ```

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="1415a-167">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="1415a-167">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="1415a-168">W **terminalu**, uruchom następujące polecenie, aby zainstalować LibMan.</span><span class="sxs-lookup"><span data-stu-id="1415a-168">In the **Terminal**, run the following command to install LibMan.</span></span>

  ```console
  dotnet tool install -g Microsoft.Web.LibraryManager.Cli
  ```

* <span data-ttu-id="1415a-169">Przejdź do folderu projektu (zawierającego *SignalRChat.csproj* pliku).</span><span class="sxs-lookup"><span data-stu-id="1415a-169">Navigate to the project folder (the one that contains the *SignalRChat.csproj* file).</span></span>

* <span data-ttu-id="1415a-170">Uruchom następujące polecenie, aby przy użyciu LibMan uzyskać biblioteki klienta SignalR.</span><span class="sxs-lookup"><span data-stu-id="1415a-170">Run the following command to get the SignalR client library by using LibMan.</span></span>

  ```console
  libman install @aspnet/signalr -p unpkg -d wwwroot/lib/signalr --files dist/browser/signalr.js --files dist/browser/signalr.min.js
  ```

  <span data-ttu-id="1415a-171">Parametry określ następujące opcje:</span><span class="sxs-lookup"><span data-stu-id="1415a-171">The parameters specify the following options:</span></span>
  * <span data-ttu-id="1415a-172">Użyj dostawcy unpkg.</span><span class="sxs-lookup"><span data-stu-id="1415a-172">Use the unpkg provider.</span></span>
  * <span data-ttu-id="1415a-173">Skopiuj pliki do *wwwroot/lib/signalr* docelowego.</span><span class="sxs-lookup"><span data-stu-id="1415a-173">Copy files to the *wwwroot/lib/signalr* destination.</span></span>
  * <span data-ttu-id="1415a-174">Skopiuj tylko określonych plików.</span><span class="sxs-lookup"><span data-stu-id="1415a-174">Copy only the specified files.</span></span>

  <span data-ttu-id="1415a-175">Dane wyjściowe wyglądają jak w poniższym przykładzie:</span><span class="sxs-lookup"><span data-stu-id="1415a-175">The output looks like the following example:</span></span>

  ```console
  wwwroot/lib/signalr/dist/browser/signalr.js written to disk
  wwwroot/lib/signalr/dist/browser/signalr.min.js written to disk
  Installed library "@aspnet/signalr@1.0.3" to "wwwroot/lib/signalr"
  ```

---

## <a name="create-a-signalr-hub"></a><span data-ttu-id="1415a-176">Tworzenie Centrum SignalR</span><span class="sxs-lookup"><span data-stu-id="1415a-176">Create a SignalR hub</span></span>

<span data-ttu-id="1415a-177">A *Centrum* jest klasa, która służy jako ogólny potok, który obsługuje komunikację klient serwer.</span><span class="sxs-lookup"><span data-stu-id="1415a-177">A *hub* is a class that serves as a high-level pipeline that handles client-server communication.</span></span>

* <span data-ttu-id="1415a-178">W folderze projektu SignalRChat Utwórz *koncentratory* folderu.</span><span class="sxs-lookup"><span data-stu-id="1415a-178">In the SignalRChat project folder, create a *Hubs* folder.</span></span>

* <span data-ttu-id="1415a-179">W *Hubs* folderze utwórz *ChatHub.cs* pliku następującym kodem:</span><span class="sxs-lookup"><span data-stu-id="1415a-179">In the *Hubs* folder, create a *ChatHub.cs* file with the following code:</span></span>

  [!code-csharp[Startup](signalr/sample/Hubs/ChatHub.cs)]

  <span data-ttu-id="1415a-180">`ChatHub` Klasa dziedziczy z elementu SignalR `Hub` klasy.</span><span class="sxs-lookup"><span data-stu-id="1415a-180">The `ChatHub` class inherits from the SignalR `Hub` class.</span></span> <span data-ttu-id="1415a-181">`Hub` Klasa zarządza połączeń, grup i komunikatów.</span><span class="sxs-lookup"><span data-stu-id="1415a-181">The `Hub` class manages connections, groups, and messaging.</span></span>

  <span data-ttu-id="1415a-182">`SendMessage` Metoda może być wywoływana przez wszystkie połączone klienta.</span><span class="sxs-lookup"><span data-stu-id="1415a-182">The `SendMessage` method can be called by any connected client.</span></span> <span data-ttu-id="1415a-183">Wysyła odebranego komunikatu do wszystkich klientów.</span><span class="sxs-lookup"><span data-stu-id="1415a-183">It sends the received message to all clients.</span></span> <span data-ttu-id="1415a-184">Kod SignalR jest asynchroniczne w celu zapewnienia maksymalnej skalowalności.</span><span class="sxs-lookup"><span data-stu-id="1415a-184">SignalR code is asynchronous to provide maximum scalability.</span></span>

## <a name="configure-signalr"></a><span data-ttu-id="1415a-185">Konfigurowanie biblioteki SignalR</span><span class="sxs-lookup"><span data-stu-id="1415a-185">Configure SignalR</span></span>

<span data-ttu-id="1415a-186">Serwer biblioteki SignalR musi być skonfigurowany do przekazywania żądań SignalR z SignalR.</span><span class="sxs-lookup"><span data-stu-id="1415a-186">The SignalR server must be configured to pass SignalR requests to SignalR.</span></span>

* <span data-ttu-id="1415a-187">Dodaj następujący wyróżniony kod do *Startup.cs* pliku.</span><span class="sxs-lookup"><span data-stu-id="1415a-187">Add the following highlighted code to the *Startup.cs* file.</span></span>

  [!code-csharp[Startup](signalr/sample/Startup.cs?highlight=7,33,52-55)]

  <span data-ttu-id="1415a-188">Te zmiany dodanie SignalR systemu iniekcji zależności platformy ASP.NET Core i potoku oprogramowania pośredniczącego.</span><span class="sxs-lookup"><span data-stu-id="1415a-188">These changes add SignalR to the ASP.NET Core dependency injection system and the middleware pipeline.</span></span>

## <a name="add-signalr-client-code"></a><span data-ttu-id="1415a-189">Dodaj kod klienta SignalR</span><span class="sxs-lookup"><span data-stu-id="1415a-189">Add SignalR client code</span></span>

* <span data-ttu-id="1415a-190">Zastąp zawartość *Pages\Index.cshtml* następującym kodem:</span><span class="sxs-lookup"><span data-stu-id="1415a-190">Replace the content in *Pages\Index.cshtml* with the following code:</span></span>

  [!code-cshtml[Index](signalr/sample/Pages/Index.cshtml)]

  <span data-ttu-id="1415a-191">Powyższy kod:</span><span class="sxs-lookup"><span data-stu-id="1415a-191">The preceding code:</span></span>

  * <span data-ttu-id="1415a-192">Tworzy pola tekstowe nazwy i tekst wiadomość i przycisk Prześlij.</span><span class="sxs-lookup"><span data-stu-id="1415a-192">Creates text boxes for name and message text, and a submit button.</span></span>
  * <span data-ttu-id="1415a-193">Tworzy listę z `id="messagesList"` do wyświetlania komunikatów odebranych z Centrum SignalR.</span><span class="sxs-lookup"><span data-stu-id="1415a-193">Creates a list with `id="messagesList"` for displaying messages that are received from the SignalR hub.</span></span>
  * <span data-ttu-id="1415a-194">Zawiera odwołania do skryptu z SignalR i *chat.js* kod aplikacji, który zostanie utworzony w następnym kroku.</span><span class="sxs-lookup"><span data-stu-id="1415a-194">Includes script references to SignalR and the *chat.js* application code that you create in the next step.</span></span>

* <span data-ttu-id="1415a-195">W *wwwroot/js* folderze utwórz *chat.js* pliku następującym kodem:</span><span class="sxs-lookup"><span data-stu-id="1415a-195">In the *wwwroot/js* folder, create a *chat.js* file with the following code:</span></span>

  [!code-javascript[Index](signalr/sample/wwwroot/js/chat.js)]

  <span data-ttu-id="1415a-196">Powyższy kod:</span><span class="sxs-lookup"><span data-stu-id="1415a-196">The preceding code:</span></span>

  * <span data-ttu-id="1415a-197">Tworzy i uruchamia połączenie.</span><span class="sxs-lookup"><span data-stu-id="1415a-197">Creates and starts a connection.</span></span>
  * <span data-ttu-id="1415a-198">Dodaje przycisk przesyłania program obsługi, która wysyła komunikaty do Centrum.</span><span class="sxs-lookup"><span data-stu-id="1415a-198">Adds to the submit button a handler that sends messages to the hub.</span></span>
  * <span data-ttu-id="1415a-199">Dodaje do obiektu połączenia program obsługi, który odbiera komunikaty z Centrum i dodaje je do listy.</span><span class="sxs-lookup"><span data-stu-id="1415a-199">Adds to the connection object a handler that receives messages from the hub and adds them to the list.</span></span>

## <a name="run-the-app"></a><span data-ttu-id="1415a-200">Uruchamianie aplikacji</span><span class="sxs-lookup"><span data-stu-id="1415a-200">Run the app</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="1415a-201">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="1415a-201">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="1415a-202">Naciśnij klawisz **kombinację klawiszy CTRL + F5** Aby uruchomić aplikację bez debugowania.</span><span class="sxs-lookup"><span data-stu-id="1415a-202">Press **CTRL+F5** to run the app without debugging.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="1415a-203">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="1415a-203">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="1415a-204">W zintegrowanym terminalu uruchom następujące polecenie:</span><span class="sxs-lookup"><span data-stu-id="1415a-204">In the integrated terminal, run the following command:</span></span>

  ```console
  dotnet run -p SignalRChat.csproj
  ```
  
# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="1415a-205">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="1415a-205">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="1415a-206">W menu, wybierz opcję **Uruchom > Uruchom bez debugowania**.</span><span class="sxs-lookup"><span data-stu-id="1415a-206">From the menu, select **Run > Start Without Debugging**.</span></span>

---

* <span data-ttu-id="1415a-207">Skopiuj adres URL z paska adresu, a następnie otwórz innego wystąpienia przeglądarki lub karty i wklej adres URL w pasku adresu.</span><span class="sxs-lookup"><span data-stu-id="1415a-207">Copy the URL from the address bar, open another browser instance or tab, and paste the URL in the address bar.</span></span>

* <span data-ttu-id="1415a-208">Wybierz albo przeglądarki, wprowadź nazwę i komunikat, a następnie wybierz **wysyłania komunikatu** przycisku.</span><span class="sxs-lookup"><span data-stu-id="1415a-208">Choose either browser, enter a name and message, and select the **Send Message** button.</span></span>

  <span data-ttu-id="1415a-209">Nazwa i wiadomości są wyświetlane na obu stronach natychmiast.</span><span class="sxs-lookup"><span data-stu-id="1415a-209">The name and message are displayed on both pages instantly.</span></span>

  ![SignalR przykładowej aplikacji](signalr/_static/signalr-get-started-finished.png)

> [!TIP]
> <span data-ttu-id="1415a-211">Jeśli aplikacja nie działa, Otwórz swoje narzędzia deweloperskie przeglądarki (F12) i przejdź do konsoli.</span><span class="sxs-lookup"><span data-stu-id="1415a-211">If the app doesn't work, open your browser developer tools (F12) and go to the console.</span></span> <span data-ttu-id="1415a-212">Może pojawić się błędy związane z Twoim kodem HTML i JavaScript.</span><span class="sxs-lookup"><span data-stu-id="1415a-212">You might see errors related to your HTML and JavaScript code.</span></span> <span data-ttu-id="1415a-213">Załóżmy, że możesz umieścić *signalr.js* w innym folderze niż bezpośredniego.</span><span class="sxs-lookup"><span data-stu-id="1415a-213">For example, suppose you put *signalr.js* in a different folder than directed.</span></span> <span data-ttu-id="1415a-214">W takim przypadku odwołania do tego pliku nie będzie działać, a następnie zostanie wyświetlony błąd 404 w konsoli.</span><span class="sxs-lookup"><span data-stu-id="1415a-214">In that case the reference to that file won't work and you'll see a 404 error in the console.</span></span>
> <span data-ttu-id="1415a-215">![błąd — nie znaleziono signalr.js](signalr/_static/f12-console.png)</span><span class="sxs-lookup"><span data-stu-id="1415a-215">![signalr.js not found error](signalr/_static/f12-console.png)</span></span>

## <a name="next-steps"></a><span data-ttu-id="1415a-216">Następne kroki</span><span class="sxs-lookup"><span data-stu-id="1415a-216">Next steps</span></span>

<span data-ttu-id="1415a-217">W tym samouczku przedstawiono sposób:</span><span class="sxs-lookup"><span data-stu-id="1415a-217">In this tutorial, you learned how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="1415a-218">Tworzenie projektu aplikacji sieci web.</span><span class="sxs-lookup"><span data-stu-id="1415a-218">Create a web app project.</span></span>
> * <span data-ttu-id="1415a-219">Dodaj bibliotekę klienta SignalR.</span><span class="sxs-lookup"><span data-stu-id="1415a-219">Add the SignalR client library.</span></span>
> * <span data-ttu-id="1415a-220">Utwórz Centrum SignalR.</span><span class="sxs-lookup"><span data-stu-id="1415a-220">Create a SignalR hub.</span></span>
> * <span data-ttu-id="1415a-221">Skonfiguruj projekt do korzystania z SignalR.</span><span class="sxs-lookup"><span data-stu-id="1415a-221">Configure the project to use SignalR.</span></span>
> * <span data-ttu-id="1415a-222">Dodaj kod używający koncentratora, aby wysyłać komunikaty z dowolnego klienta do wszystkich połączonych klientów.</span><span class="sxs-lookup"><span data-stu-id="1415a-222">Add code that uses the hub to send messages from any client to all connected clients.</span></span>

<span data-ttu-id="1415a-223">Aby dowiedzieć się więcej na temat biblioteki SignalR, zobacz wprowadzenie:</span><span class="sxs-lookup"><span data-stu-id="1415a-223">To learn more about SignalR, see the introduction:</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="1415a-224">Wprowadzenie do SignalR platformy ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="1415a-224">Introduction to ASP.NET Core SignalR</span></span>](xref:signalr/introduction)
