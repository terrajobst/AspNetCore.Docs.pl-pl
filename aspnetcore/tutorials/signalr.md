---
title: Wprowadzenie do biblioteki SignalR platformy ASP.NET Core
author: tdykstra
description: W tym samouczku utworzysz aplikację rozmowy, która korzysta z biblioteki SignalR platformy ASP.NET Core.
monikerRange: '>= aspnetcore-2.1'
ms.author: tdykstra
ms.custom: mvc
ms.date: 08/31/2018
uid: tutorials/signalr
ms.openlocfilehash: fcfe2fa6cc88b9eee1389e171fa5eb7711b4f14f
ms.sourcegitcommit: fc2486ddbeb15ab4969168d99b3fe0fbe91e8661
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/01/2018
ms.locfileid: "50758131"
---
# <a name="tutorial-get-started-with-aspnet-core-signalr"></a><span data-ttu-id="52f6c-103">Samouczek: Rozpoczynanie pracy przy użyciu biblioteki SignalR platformy ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="52f6c-103">Tutorial: Get started with ASP.NET Core SignalR</span></span>

<span data-ttu-id="52f6c-104">W tym samouczku pokazano podstawowe informacje dotyczące tworzenia aplikacji w czasie rzeczywistym przy użyciu biblioteki SignalR.</span><span class="sxs-lookup"><span data-stu-id="52f6c-104">This tutorial teaches the basics of building a real-time app using SignalR.</span></span> <span data-ttu-id="52f6c-105">Dowiesz się, jak:</span><span class="sxs-lookup"><span data-stu-id="52f6c-105">You learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="52f6c-106">Utwórz projekt sieci web.</span><span class="sxs-lookup"><span data-stu-id="52f6c-106">Create a web project.</span></span>
> * <span data-ttu-id="52f6c-107">Dodaj bibliotekę klienta SignalR.</span><span class="sxs-lookup"><span data-stu-id="52f6c-107">Add the SignalR client library.</span></span>
> * <span data-ttu-id="52f6c-108">Utwórz Centrum SignalR.</span><span class="sxs-lookup"><span data-stu-id="52f6c-108">Create a SignalR hub.</span></span>
> * <span data-ttu-id="52f6c-109">Skonfiguruj projekt do korzystania z SignalR.</span><span class="sxs-lookup"><span data-stu-id="52f6c-109">Configure the project to use SignalR.</span></span>
> * <span data-ttu-id="52f6c-110">Dodaj kod, który wysyła komunikaty z dowolnego klienta do wszystkich połączonych klientów.</span><span class="sxs-lookup"><span data-stu-id="52f6c-110">Add code that sends messages from any client to all connected clients.</span></span>

<span data-ttu-id="52f6c-111">Po zakończeniu będziesz mieć działającą aplikację rozmowy:</span><span class="sxs-lookup"><span data-stu-id="52f6c-111">At the end, you'll have a working chat app:</span></span>

![SignalR przykładowej aplikacji](signalr/_static/signalr-get-started-finished.png)

<span data-ttu-id="52f6c-113">[Wyświetlanie lub pobieranie przykładowego kodu](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/signalr/sample) ([sposobu pobierania](xref:index#how-to-download-a-sample)).</span><span class="sxs-lookup"><span data-stu-id="52f6c-113">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/signalr/sample) ([how to download](xref:index#how-to-download-a-sample)).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="52f6c-114">Wymagania wstępne</span><span class="sxs-lookup"><span data-stu-id="52f6c-114">Prerequisites</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="52f6c-115">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="52f6c-115">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="52f6c-116">[Visual Studio 2017 w wersji należy zachować 15,8 lub nowszej](https://www.visualstudio.com/downloads/) z **ASP.NET i tworzenie aplikacji internetowych** obciążenia</span><span class="sxs-lookup"><span data-stu-id="52f6c-116">[Visual Studio 2017 version 15.8 or later](https://www.visualstudio.com/downloads/) with the **ASP.NET and web development** workload</span></span>
* [<span data-ttu-id="52f6c-117">.NET core SDK 2.1 lub nowszej</span><span class="sxs-lookup"><span data-stu-id="52f6c-117">.NET Core SDK 2.1 or later</span></span>](https://www.microsoft.com/net/download/all)

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="52f6c-118">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="52f6c-118">Visual Studio Code</span></span>](#tab/visual-studio-code)

* [<span data-ttu-id="52f6c-119">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="52f6c-119">Visual Studio Code</span></span>](https://code.visualstudio.com/download)
* [<span data-ttu-id="52f6c-120">.NET core SDK 2.1 lub nowszej</span><span class="sxs-lookup"><span data-stu-id="52f6c-120">.NET Core SDK 2.1 or later</span></span>](https://www.microsoft.com/net/download/all)
* [<span data-ttu-id="52f6c-121">Środowisko C# dla programu Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="52f6c-121">C# for Visual Studio Code</span></span>](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp)

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="52f6c-122">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="52f6c-122">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* [<span data-ttu-id="52f6c-123">Program Visual Studio dla komputerów Mac w wersji 7.5.4 lub nowszy</span><span class="sxs-lookup"><span data-stu-id="52f6c-123">Visual Studio for Mac version 7.5.4 or later</span></span>](https://www.visualstudio.com/downloads/)
* <span data-ttu-id="52f6c-124">[.NET core SDK 2.1 lub nowszej](https://www.microsoft.com/net/download/all) (dołączone do instalacji programu Visual Studio)</span><span class="sxs-lookup"><span data-stu-id="52f6c-124">[.NET Core SDK 2.1 or later](https://www.microsoft.com/net/download/all) (included in the Visual Studio install)</span></span>

---

## <a name="create-a-web-project"></a><span data-ttu-id="52f6c-125">Tworzenie projektu sieci web</span><span class="sxs-lookup"><span data-stu-id="52f6c-125">Create a web project</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="52f6c-126">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="52f6c-126">Visual Studio</span></span>](#tab/visual-studio/)

* <span data-ttu-id="52f6c-127">W menu, wybierz opcję **Plik > Nowy projekt**.</span><span class="sxs-lookup"><span data-stu-id="52f6c-127">From the menu, select **File > New Project**.</span></span>

* <span data-ttu-id="52f6c-128">W **nowy projekt** okno dialogowe, wybierz opcję **zainstalowane > Visual C# > sieci Web > Aplikacja sieci Web programu ASP.NET Core**.</span><span class="sxs-lookup"><span data-stu-id="52f6c-128">In the **New Project** dialog, select **Installed > Visual C# > Web > ASP.NET Core Web Application**.</span></span> <span data-ttu-id="52f6c-129">Nadaj projektowi nazwę *SignalRChat*.</span><span class="sxs-lookup"><span data-stu-id="52f6c-129">Name the project *SignalRChat*.</span></span>

  ![Okno dialogowe nowego projektu w programie Visual Studio](signalr/_static/signalr-new-project-dialog.png)

* <span data-ttu-id="52f6c-131">Wybierz **aplikacji sieci Web** do tworzenia projektu, który używa stron Razor.</span><span class="sxs-lookup"><span data-stu-id="52f6c-131">Select **Web Application** to create a project that uses Razor Pages.</span></span>

* <span data-ttu-id="52f6c-132">Wybierz docelową platformę **platformy .NET Core**, wybierz opcję **platformy ASP.NET Core 2.1**i kliknij przycisk **OK**.</span><span class="sxs-lookup"><span data-stu-id="52f6c-132">Select a target framework of **.NET Core**, select **ASP.NET Core 2.1**, and click **OK**.</span></span>

  ![Okno dialogowe nowego projektu w programie Visual Studio](signalr/_static/signalr-new-project-choose-type.png)

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="52f6c-134">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="52f6c-134">Visual Studio Code</span></span>](#tab/visual-studio-code/)

* <span data-ttu-id="52f6c-135">Otwórz folder, który można użyć dla nowego projektu.</span><span class="sxs-lookup"><span data-stu-id="52f6c-135">Open a folder that you can use for a new project.</span></span>

* <span data-ttu-id="52f6c-136">W [zintegrowany Terminal](https://code.visualstudio.com/docs/editor/integrated-terminal), uruchom następujące polecenie:</span><span class="sxs-lookup"><span data-stu-id="52f6c-136">In the [Integrated Terminal](https://code.visualstudio.com/docs/editor/integrated-terminal), run the following command:</span></span>

   ```console
   dotnet new webapp -o SignalRChat
   ```

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="52f6c-137">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="52f6c-137">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="52f6c-138">W menu, wybierz opcję **Plik > nowe rozwiązanie**.</span><span class="sxs-lookup"><span data-stu-id="52f6c-138">From the menu, select **File > New Solution**.</span></span>

* <span data-ttu-id="52f6c-139">Wybierz **platformy .NET Core > aplikacji > aplikacji internetowej ASP.NET Core** (nie wybieraj **ASP.NET Core Web App (MVC)**).</span><span class="sxs-lookup"><span data-stu-id="52f6c-139">Select **.NET Core > App > ASP.NET Core Web App** (Don't select **ASP.NET Core Web App (MVC)**).</span></span>

* <span data-ttu-id="52f6c-140">Wybierz **dalej**.</span><span class="sxs-lookup"><span data-stu-id="52f6c-140">Select **Next**.</span></span>

* <span data-ttu-id="52f6c-141">Nadaj projektowi nazwę *SignalRChat*, a następnie wybierz pozycję **Utwórz**.</span><span class="sxs-lookup"><span data-stu-id="52f6c-141">Name the project *SignalRChat*, and then select **Create**.</span></span>

---

## <a name="add-the-signalr-client-library"></a><span data-ttu-id="52f6c-142">Dodaj bibliotekę klienta SignalR</span><span class="sxs-lookup"><span data-stu-id="52f6c-142">Add the SignalR client library</span></span>

<span data-ttu-id="52f6c-143">Serwer biblioteki SignalR znajduje się w `Microsoft.AspNetCore.App` meta Microsoft.aspnetcore.all.</span><span class="sxs-lookup"><span data-stu-id="52f6c-143">The SignalR server library is included in the `Microsoft.AspNetCore.App` metapackage.</span></span> <span data-ttu-id="52f6c-144">Biblioteki klienta JavaScript automatycznie nie jest zawarty w projekcie.</span><span class="sxs-lookup"><span data-stu-id="52f6c-144">The JavaScript client library isn't automatically included in the project.</span></span> <span data-ttu-id="52f6c-145">W tym samouczku używasz Library Manager (LibMan) można pobrać z biblioteki klienta z *unpkg*.</span><span class="sxs-lookup"><span data-stu-id="52f6c-145">For this tutorial, you use Library Manager (LibMan) to get the client library from *unpkg*.</span></span> <span data-ttu-id="52f6c-146">unpkg jest usługa content delivery network (CDN)), można dostarczać niczego w Menedżer pakietów npm oraz Menedżera pakietów środowiska Node.js.</span><span class="sxs-lookup"><span data-stu-id="52f6c-146">unpkg is a content delivery network (CDN)) that can deliver anything found in npm, the Node.js package manager.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="52f6c-147">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="52f6c-147">Visual Studio</span></span>](#tab/visual-studio/)

* <span data-ttu-id="52f6c-148">W **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy projekt i wybierz **Dodaj** > **biblioteki po stronie klienta**.</span><span class="sxs-lookup"><span data-stu-id="52f6c-148">In **Solution Explorer**, right-click the project, and select **Add** > **Client-Side Library**.</span></span>

* <span data-ttu-id="52f6c-149">W **Dodaj biblioteki po stronie klienta** okno dialogowe, aby uzyskać **dostawcy** wybierz **unpkg**.</span><span class="sxs-lookup"><span data-stu-id="52f6c-149">In the **Add Client-Side Library** dialog, for **Provider** select **unpkg**.</span></span> 

* <span data-ttu-id="52f6c-150">Aby uzyskać **biblioteki**, wprowadź `@aspnet/signalr@1`i wybierz najnowszą wersję, która nie jest w wersji zapoznawczej.</span><span class="sxs-lookup"><span data-stu-id="52f6c-150">For **Library**, enter `@aspnet/signalr@1`, and select the latest version that isn't preview.</span></span>

  ![Dodaj okno dialogowe biblioteki po stronie klienta — Wybieranie biblioteki](signalr/_static/libman1.png)

* <span data-ttu-id="52f6c-152">Wybierz **wybierz konkretne pliki**, rozwiń węzeł *dist/przeglądarki* folder, a następnie wybierz *signalr.js* i *signalr.min.js*.</span><span class="sxs-lookup"><span data-stu-id="52f6c-152">Select **Choose specific files**, expand the *dist/browser* folder, and select *signalr.js* and *signalr.min.js*.</span></span>

* <span data-ttu-id="52f6c-153">Ustaw **lokalizacji docelowej** do *wwwroot/lib/signalr/* i wybierz **zainstalować**.</span><span class="sxs-lookup"><span data-stu-id="52f6c-153">Set **Target Location** to *wwwroot/lib/signalr/*, and select **Install**.</span></span>

  ![Dodaj okno dialogowe z biblioteki klienta - wybranie plików i miejsce docelowe](signalr/_static/libman2.png)

  <span data-ttu-id="52f6c-155">Tworzy LibMan *wwwroot/lib/signalr* folder i kopiuje wybrane pliki.</span><span class="sxs-lookup"><span data-stu-id="52f6c-155">LibMan creates a *wwwroot/lib/signalr* folder and copies the selected files to it.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="52f6c-156">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="52f6c-156">Visual Studio Code</span></span>](#tab/visual-studio-code/)

* <span data-ttu-id="52f6c-157">W **zintegrowany Terminal**, uruchom następujące polecenie, aby zainstalować LibMan.</span><span class="sxs-lookup"><span data-stu-id="52f6c-157">In the **Integrated Terminal**, run the following command to install LibMan.</span></span>

  ```console
  dotnet tool install -g Microsoft.Web.LibraryManager.Cli
  ```

* <span data-ttu-id="52f6c-158">Przejdź do folderu projektu (zawierającego *SignalRChat.csproj* pliku).</span><span class="sxs-lookup"><span data-stu-id="52f6c-158">Navigate to the project folder (the one that contains the *SignalRChat.csproj* file).</span></span>

* <span data-ttu-id="52f6c-159">Uruchom następujące polecenie, aby przy użyciu LibMan uzyskać biblioteki klienta SignalR.</span><span class="sxs-lookup"><span data-stu-id="52f6c-159">Run the following command to get the SignalR client library by using LibMan.</span></span> <span data-ttu-id="52f6c-160">Być może musisz odczekaj kilka sekund, zanim dane wyjściowe.</span><span class="sxs-lookup"><span data-stu-id="52f6c-160">You might have to wait a few seconds before seeing output.</span></span>

  ```console
  libman install @aspnet/signalr -p unpkg -d wwwroot/lib/signalr --files dist/browser/signalr.js --files dist/browser/signalr.min.js
  ```

  <span data-ttu-id="52f6c-161">Parametry określ następujące opcje:</span><span class="sxs-lookup"><span data-stu-id="52f6c-161">The parameters specify the following options:</span></span>
  * <span data-ttu-id="52f6c-162">Użyj dostawcy unpkg.</span><span class="sxs-lookup"><span data-stu-id="52f6c-162">Use the unpkg provider.</span></span>
  * <span data-ttu-id="52f6c-163">Skopiuj pliki do *wwwroot/lib/signalr* docelowego.</span><span class="sxs-lookup"><span data-stu-id="52f6c-163">Copy files to the *wwwroot/lib/signalr* destination.</span></span>
  * <span data-ttu-id="52f6c-164">Skopiuj tylko określonych plików.</span><span class="sxs-lookup"><span data-stu-id="52f6c-164">Copy only the specified files.</span></span>

  <span data-ttu-id="52f6c-165">Dane wyjściowe wyglądają jak w poniższym przykładzie:</span><span class="sxs-lookup"><span data-stu-id="52f6c-165">The output looks like the following example:</span></span>

  ```console
  wwwroot/lib/signalr/dist/browser/signalr.js written to disk
  wwwroot/lib/signalr/dist/browser/signalr.min.js written to disk
  Installed library "@aspnet/signalr@1.0.3" to "wwwroot/lib/signalr"
  ```

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="52f6c-166">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="52f6c-166">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="52f6c-167">W **terminalu**, uruchom następujące polecenie, aby zainstalować LibMan.</span><span class="sxs-lookup"><span data-stu-id="52f6c-167">In the **Terminal**, run the following command to install LibMan.</span></span>

  ```console
  dotnet tool install -g Microsoft.Web.LibraryManager.Cli
  ```

* <span data-ttu-id="52f6c-168">Przejdź do folderu projektu (zawierającego *SignalRChat.csproj* pliku).</span><span class="sxs-lookup"><span data-stu-id="52f6c-168">Navigate to the project folder (the one that contains the *SignalRChat.csproj* file).</span></span>

* <span data-ttu-id="52f6c-169">Uruchom następujące polecenie, aby przy użyciu LibMan uzyskać biblioteki klienta SignalR.</span><span class="sxs-lookup"><span data-stu-id="52f6c-169">Run the following command to get the SignalR client library by using LibMan.</span></span>

  ```console
  libman install @aspnet/signalr -p unpkg -d wwwroot/lib/signalr --files dist/browser/signalr.js --files dist/browser/signalr.min.js
  ```

  <span data-ttu-id="52f6c-170">Parametry określ następujące opcje:</span><span class="sxs-lookup"><span data-stu-id="52f6c-170">The parameters specify the following options:</span></span>
  * <span data-ttu-id="52f6c-171">Użyj dostawcy unpkg.</span><span class="sxs-lookup"><span data-stu-id="52f6c-171">Use the unpkg provider.</span></span>
  * <span data-ttu-id="52f6c-172">Skopiuj pliki do *wwwroot/lib/signalr* docelowego.</span><span class="sxs-lookup"><span data-stu-id="52f6c-172">Copy files to the *wwwroot/lib/signalr* destination.</span></span>
  * <span data-ttu-id="52f6c-173">Skopiuj tylko określonych plików.</span><span class="sxs-lookup"><span data-stu-id="52f6c-173">Copy only the specified files.</span></span>

  <span data-ttu-id="52f6c-174">Dane wyjściowe wyglądają jak w poniższym przykładzie:</span><span class="sxs-lookup"><span data-stu-id="52f6c-174">The output looks like the following example:</span></span>

  ```console
  wwwroot/lib/signalr/dist/browser/signalr.js written to disk
  wwwroot/lib/signalr/dist/browser/signalr.min.js written to disk
  Installed library "@aspnet/signalr@1.0.3" to "wwwroot/lib/signalr"
  ```

---

## <a name="create-a-signalr-hub"></a><span data-ttu-id="52f6c-175">Tworzenie Centrum SignalR</span><span class="sxs-lookup"><span data-stu-id="52f6c-175">Create a SignalR hub</span></span>

<span data-ttu-id="52f6c-176">A *Centrum* jest klasa, która służy jako ogólny potok, który obsługuje komunikację klient serwer.</span><span class="sxs-lookup"><span data-stu-id="52f6c-176">A *hub* is a class that serves as a high-level pipeline that handles client-server communication.</span></span>

* <span data-ttu-id="52f6c-177">W folderze projektu SignalRChat Utwórz *koncentratory* folderu.</span><span class="sxs-lookup"><span data-stu-id="52f6c-177">In the SignalRChat project folder, create a *Hubs* folder.</span></span>

* <span data-ttu-id="52f6c-178">W *Hubs* folderze utwórz *ChatHub.cs* pliku następującym kodem:</span><span class="sxs-lookup"><span data-stu-id="52f6c-178">In the *Hubs* folder, create a *ChatHub.cs* file with the following code:</span></span>

  [!code-csharp[Startup](signalr/sample/Hubs/ChatHub.cs)]

  <span data-ttu-id="52f6c-179">`ChatHub` Klasa dziedziczy z elementu SignalR `Hub` klasy.</span><span class="sxs-lookup"><span data-stu-id="52f6c-179">The `ChatHub` class inherits from the SignalR `Hub` class.</span></span> <span data-ttu-id="52f6c-180">`Hub` Klasa zarządza połączeń, grup i komunikatów.</span><span class="sxs-lookup"><span data-stu-id="52f6c-180">The `Hub` class manages connections, groups, and messaging.</span></span>

  <span data-ttu-id="52f6c-181">`SendMessage` Metoda może być wywoływana przez wszystkie połączone klienta.</span><span class="sxs-lookup"><span data-stu-id="52f6c-181">The `SendMessage` method can be called by any connected client.</span></span> <span data-ttu-id="52f6c-182">Wysyła odebranego komunikatu do wszystkich klientów.</span><span class="sxs-lookup"><span data-stu-id="52f6c-182">It sends the received message to all clients.</span></span> <span data-ttu-id="52f6c-183">Kod SignalR jest asynchroniczne w celu zapewnienia maksymalnej skalowalności.</span><span class="sxs-lookup"><span data-stu-id="52f6c-183">SignalR code is asynchronous to provide maximum scalability.</span></span>

## <a name="configure-signalr"></a><span data-ttu-id="52f6c-184">Konfigurowanie biblioteki SignalR</span><span class="sxs-lookup"><span data-stu-id="52f6c-184">Configure SignalR</span></span>

<span data-ttu-id="52f6c-185">Serwer biblioteki SignalR musi być skonfigurowany do przekazywania żądań SignalR z SignalR.</span><span class="sxs-lookup"><span data-stu-id="52f6c-185">The SignalR server must be configured to pass SignalR requests to SignalR.</span></span>

* <span data-ttu-id="52f6c-186">Dodaj następujący wyróżniony kod do *Startup.cs* pliku.</span><span class="sxs-lookup"><span data-stu-id="52f6c-186">Add the following highlighted code to the *Startup.cs* file.</span></span>

  [!code-csharp[Startup](signalr/sample/Startup.cs?highlight=7,33,52-55)]

  <span data-ttu-id="52f6c-187">Te zmiany dodanie SignalR systemu iniekcji zależności platformy ASP.NET Core i potoku oprogramowania pośredniczącego.</span><span class="sxs-lookup"><span data-stu-id="52f6c-187">These changes add SignalR to the ASP.NET Core dependency injection system and the middleware pipeline.</span></span>

## <a name="add-signalr-client-code"></a><span data-ttu-id="52f6c-188">Dodaj kod klienta SignalR</span><span class="sxs-lookup"><span data-stu-id="52f6c-188">Add SignalR client code</span></span>

* <span data-ttu-id="52f6c-189">Zastąp zawartość *Pages\Index.cshtml* następującym kodem:</span><span class="sxs-lookup"><span data-stu-id="52f6c-189">Replace the content in *Pages\Index.cshtml* with the following code:</span></span>

  [!code-cshtml[Index](signalr/sample/Pages/Index.cshtml)]

  <span data-ttu-id="52f6c-190">Powyższy kod:</span><span class="sxs-lookup"><span data-stu-id="52f6c-190">The preceding code:</span></span>

  * <span data-ttu-id="52f6c-191">Tworzy pola tekstowe nazwy i tekst wiadomość i przycisk Prześlij.</span><span class="sxs-lookup"><span data-stu-id="52f6c-191">Creates text boxes for name and message text, and a submit button.</span></span>
  * <span data-ttu-id="52f6c-192">Tworzy listę z `id="messagesList"` do wyświetlania komunikatów odebranych z Centrum SignalR.</span><span class="sxs-lookup"><span data-stu-id="52f6c-192">Creates a list with `id="messagesList"` for displaying messages that are received from the SignalR hub.</span></span>
  * <span data-ttu-id="52f6c-193">Zawiera odwołania do skryptu z SignalR i *chat.js* kod aplikacji, który zostanie utworzony w następnym kroku.</span><span class="sxs-lookup"><span data-stu-id="52f6c-193">Includes script references to SignalR and the *chat.js* application code that you create in the next step.</span></span>

* <span data-ttu-id="52f6c-194">W *wwwroot/js* folderze utwórz *chat.js* pliku następującym kodem:</span><span class="sxs-lookup"><span data-stu-id="52f6c-194">In the *wwwroot/js* folder, create a *chat.js* file with the following code:</span></span>

  [!code-javascript[Index](signalr/sample/wwwroot/js/chat.js)]

  <span data-ttu-id="52f6c-195">Powyższy kod:</span><span class="sxs-lookup"><span data-stu-id="52f6c-195">The preceding code:</span></span>

  * <span data-ttu-id="52f6c-196">Tworzy i uruchamia połączenie.</span><span class="sxs-lookup"><span data-stu-id="52f6c-196">Creates and starts a connection.</span></span>
  * <span data-ttu-id="52f6c-197">Dodaje przycisk przesyłania program obsługi, która wysyła komunikaty do Centrum.</span><span class="sxs-lookup"><span data-stu-id="52f6c-197">Adds to the submit button a handler that sends messages to the hub.</span></span>
  * <span data-ttu-id="52f6c-198">Dodaje do obiektu połączenia program obsługi, który odbiera komunikaty z Centrum i dodaje je do listy.</span><span class="sxs-lookup"><span data-stu-id="52f6c-198">Adds to the connection object a handler that receives messages from the hub and adds them to the list.</span></span>

## <a name="run-the-app"></a><span data-ttu-id="52f6c-199">Uruchamianie aplikacji</span><span class="sxs-lookup"><span data-stu-id="52f6c-199">Run the app</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="52f6c-200">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="52f6c-200">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="52f6c-201">Naciśnij klawisz **kombinację klawiszy CTRL + F5** Aby uruchomić aplikację bez debugowania.</span><span class="sxs-lookup"><span data-stu-id="52f6c-201">Press **CTRL+F5** to run the app without debugging.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="52f6c-202">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="52f6c-202">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="52f6c-203">Naciśnij klawisz **kombinację klawiszy CTRL + F5** Aby uruchomić aplikację bez debugowania.</span><span class="sxs-lookup"><span data-stu-id="52f6c-203">Press **CTRL+F5** to run the app without debugging.</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="52f6c-204">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="52f6c-204">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="52f6c-205">W menu, wybierz opcję **Uruchom > Uruchom bez debugowania**.</span><span class="sxs-lookup"><span data-stu-id="52f6c-205">From the menu, select **Run > Start Without Debugging**.</span></span>

---

* <span data-ttu-id="52f6c-206">Skopiuj adres URL z paska adresu, a następnie otwórz innego wystąpienia przeglądarki lub karty i wklej adres URL w pasku adresu.</span><span class="sxs-lookup"><span data-stu-id="52f6c-206">Copy the URL from the address bar, open another browser instance or tab, and paste the URL in the address bar.</span></span>

* <span data-ttu-id="52f6c-207">Wybierz albo przeglądarki, wprowadź nazwę i komunikat, a następnie wybierz **wysyłania** przycisku.</span><span class="sxs-lookup"><span data-stu-id="52f6c-207">Choose either browser, enter a name and message, and select the **Send** button.</span></span>

  <span data-ttu-id="52f6c-208">Nazwa i wiadomości są wyświetlane na obu stronach natychmiast.</span><span class="sxs-lookup"><span data-stu-id="52f6c-208">The name and message are displayed on both pages instantly.</span></span>

  ![SignalR przykładowej aplikacji](signalr/_static/signalr-get-started-finished.png)

> [!TIP]
> <span data-ttu-id="52f6c-210">Jeśli aplikacja nie działa, Otwórz swoje narzędzia deweloperskie przeglądarki (F12) i przejdź do konsoli.</span><span class="sxs-lookup"><span data-stu-id="52f6c-210">If the app doesn't work, open your browser developer tools (F12) and go to the console.</span></span> <span data-ttu-id="52f6c-211">Może pojawić się błędy związane z Twoim kodem HTML i JavaScript.</span><span class="sxs-lookup"><span data-stu-id="52f6c-211">You might see errors related to your HTML and JavaScript code.</span></span> <span data-ttu-id="52f6c-212">Załóżmy, że możesz umieścić *signalr.js* w innym folderze niż bezpośredniego.</span><span class="sxs-lookup"><span data-stu-id="52f6c-212">For example, suppose you put *signalr.js* in a different folder than directed.</span></span> <span data-ttu-id="52f6c-213">W takim przypadku odwołania do tego pliku nie będzie działać, a następnie zostanie wyświetlony błąd 404 w konsoli.</span><span class="sxs-lookup"><span data-stu-id="52f6c-213">In that case the reference to that file won't work and you'll see a 404 error in the console.</span></span>
> <span data-ttu-id="52f6c-214">![błąd — nie znaleziono signalr.js](signalr/_static/f12-console.png)</span><span class="sxs-lookup"><span data-stu-id="52f6c-214">![signalr.js not found error](signalr/_static/f12-console.png)</span></span>

## <a name="next-steps"></a><span data-ttu-id="52f6c-215">Następne kroki</span><span class="sxs-lookup"><span data-stu-id="52f6c-215">Next steps</span></span>

<span data-ttu-id="52f6c-216">W tym samouczku przedstawiono sposób:</span><span class="sxs-lookup"><span data-stu-id="52f6c-216">In this tutorial, you learned how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="52f6c-217">Tworzenie projektu aplikacji sieci web.</span><span class="sxs-lookup"><span data-stu-id="52f6c-217">Create a web app project.</span></span>
> * <span data-ttu-id="52f6c-218">Dodaj bibliotekę klienta SignalR.</span><span class="sxs-lookup"><span data-stu-id="52f6c-218">Add the SignalR client library.</span></span>
> * <span data-ttu-id="52f6c-219">Utwórz Centrum SignalR.</span><span class="sxs-lookup"><span data-stu-id="52f6c-219">Create a SignalR hub.</span></span>
> * <span data-ttu-id="52f6c-220">Skonfiguruj projekt do korzystania z SignalR.</span><span class="sxs-lookup"><span data-stu-id="52f6c-220">Configure the project to use SignalR.</span></span>
> * <span data-ttu-id="52f6c-221">Dodaj kod używający koncentratora, aby wysyłać komunikaty z dowolnego klienta do wszystkich połączonych klientów.</span><span class="sxs-lookup"><span data-stu-id="52f6c-221">Add code that uses the hub to send messages from any client to all connected clients.</span></span>

<span data-ttu-id="52f6c-222">Aby dowiedzieć się więcej na temat biblioteki SignalR, zobacz wprowadzenie:</span><span class="sxs-lookup"><span data-stu-id="52f6c-222">To learn more about SignalR, see the introduction:</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="52f6c-223">Wprowadzenie do SignalR platformy ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="52f6c-223">Introduction to ASP.NET Core SignalR</span></span>](xref:signalr/introduction)
