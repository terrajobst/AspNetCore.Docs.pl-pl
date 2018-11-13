---
title: Wprowadzenie do biblioteki SignalR platformy ASP.NET Core
author: tdykstra
description: W tym samouczku utworzysz aplikację rozmowy, która korzysta z biblioteki SignalR platformy ASP.NET Core.
monikerRange: '>= aspnetcore-2.1'
ms.author: tdykstra
ms.custom: mvc
ms.date: 11/13/2018
uid: tutorials/signalr
ms.openlocfilehash: b7414b1981508f2424eccb147a44023058c7f97c
ms.sourcegitcommit: 1d6ab43eed9cb3df6211c22b97bb3a9351ec4419
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/13/2018
ms.locfileid: "51597800"
---
# <a name="tutorial-get-started-with-aspnet-core-signalr"></a><span data-ttu-id="6e825-103">Samouczek: Rozpoczynanie pracy przy użyciu biblioteki SignalR platformy ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="6e825-103">Tutorial: Get started with ASP.NET Core SignalR</span></span>

<span data-ttu-id="6e825-104">W tym samouczku pokazano podstawowe informacje dotyczące tworzenia aplikacji w czasie rzeczywistym przy użyciu biblioteki SignalR.</span><span class="sxs-lookup"><span data-stu-id="6e825-104">This tutorial teaches the basics of building a real-time app using SignalR.</span></span> <span data-ttu-id="6e825-105">Dowiesz się, jak:</span><span class="sxs-lookup"><span data-stu-id="6e825-105">You learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="6e825-106">Utwórz projekt sieci web.</span><span class="sxs-lookup"><span data-stu-id="6e825-106">Create a web project.</span></span>
> * <span data-ttu-id="6e825-107">Dodaj bibliotekę klienta SignalR.</span><span class="sxs-lookup"><span data-stu-id="6e825-107">Add the SignalR client library.</span></span>
> * <span data-ttu-id="6e825-108">Utwórz Centrum SignalR.</span><span class="sxs-lookup"><span data-stu-id="6e825-108">Create a SignalR hub.</span></span>
> * <span data-ttu-id="6e825-109">Skonfiguruj projekt do korzystania z SignalR.</span><span class="sxs-lookup"><span data-stu-id="6e825-109">Configure the project to use SignalR.</span></span>
> * <span data-ttu-id="6e825-110">Dodaj kod, który wysyła komunikaty z dowolnego klienta do wszystkich połączonych klientów.</span><span class="sxs-lookup"><span data-stu-id="6e825-110">Add code that sends messages from any client to all connected clients.</span></span>

<span data-ttu-id="6e825-111">Po zakończeniu będziesz mieć działającą aplikację rozmowy:</span><span class="sxs-lookup"><span data-stu-id="6e825-111">At the end, you'll have a working chat app:</span></span>

![SignalR przykładowej aplikacji](signalr/_static/signalr-get-started-finished.png)

<span data-ttu-id="6e825-113">[Wyświetlanie lub pobieranie przykładowego kodu](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/signalr/sample) ([sposobu pobierania](xref:index#how-to-download-a-sample)).</span><span class="sxs-lookup"><span data-stu-id="6e825-113">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/signalr/sample) ([how to download](xref:index#how-to-download-a-sample)).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="6e825-114">Wymagania wstępne</span><span class="sxs-lookup"><span data-stu-id="6e825-114">Prerequisites</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="6e825-115">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="6e825-115">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="6e825-116">[Visual Studio 2017 w wersji należy zachować 15,8 lub nowszej](https://www.visualstudio.com/downloads/) z **ASP.NET i tworzenie aplikacji internetowych** obciążenia</span><span class="sxs-lookup"><span data-stu-id="6e825-116">[Visual Studio 2017 version 15.8 or later](https://www.visualstudio.com/downloads/) with the **ASP.NET and web development** workload</span></span>
* [<span data-ttu-id="6e825-117">.NET core SDK 2.1 lub nowszej</span><span class="sxs-lookup"><span data-stu-id="6e825-117">.NET Core SDK 2.1 or later</span></span>](https://www.microsoft.com/net/download/all)

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="6e825-118">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="6e825-118">Visual Studio Code</span></span>](#tab/visual-studio-code)

* [<span data-ttu-id="6e825-119">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="6e825-119">Visual Studio Code</span></span>](https://code.visualstudio.com/download)
* [<span data-ttu-id="6e825-120">.NET core SDK 2.1 lub nowszej</span><span class="sxs-lookup"><span data-stu-id="6e825-120">.NET Core SDK 2.1 or later</span></span>](https://www.microsoft.com/net/download/all)
* [<span data-ttu-id="6e825-121">Środowisko C# dla programu Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="6e825-121">C# for Visual Studio Code</span></span>](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp)

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="6e825-122">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="6e825-122">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* [<span data-ttu-id="6e825-123">Program Visual Studio dla komputerów Mac w wersji 7.5.4 lub nowszy</span><span class="sxs-lookup"><span data-stu-id="6e825-123">Visual Studio for Mac version 7.5.4 or later</span></span>](https://www.visualstudio.com/downloads/)
* <span data-ttu-id="6e825-124">[.NET core SDK 2.1 lub nowszej](https://www.microsoft.com/net/download/all) (dołączone do instalacji programu Visual Studio)</span><span class="sxs-lookup"><span data-stu-id="6e825-124">[.NET Core SDK 2.1 or later](https://www.microsoft.com/net/download/all) (included in the Visual Studio install)</span></span>

---

## <a name="create-a-web-project"></a><span data-ttu-id="6e825-125">Tworzenie projektu sieci web</span><span class="sxs-lookup"><span data-stu-id="6e825-125">Create a web project</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="6e825-126">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="6e825-126">Visual Studio</span></span>](#tab/visual-studio/)

* <span data-ttu-id="6e825-127">W menu, wybierz opcję **Plik > Nowy projekt**.</span><span class="sxs-lookup"><span data-stu-id="6e825-127">From the menu, select **File > New Project**.</span></span>

* <span data-ttu-id="6e825-128">W **nowy projekt** okno dialogowe, wybierz opcję **zainstalowane > Visual C# > sieci Web > Aplikacja sieci Web programu ASP.NET Core**.</span><span class="sxs-lookup"><span data-stu-id="6e825-128">In the **New Project** dialog, select **Installed > Visual C# > Web > ASP.NET Core Web Application**.</span></span> <span data-ttu-id="6e825-129">Nadaj projektowi nazwę *SignalRChat*.</span><span class="sxs-lookup"><span data-stu-id="6e825-129">Name the project *SignalRChat*.</span></span>

  ![Okno dialogowe nowego projektu w programie Visual Studio](signalr/_static/signalr-new-project-dialog.png)

* <span data-ttu-id="6e825-131">Wybierz **aplikacji sieci Web** do tworzenia projektu, który używa stron Razor.</span><span class="sxs-lookup"><span data-stu-id="6e825-131">Select **Web Application** to create a project that uses Razor Pages.</span></span>

* <span data-ttu-id="6e825-132">Wybierz docelową platformę **platformy .NET Core**, wybierz opcję **platformy ASP.NET Core 2.1**i kliknij przycisk **OK**.</span><span class="sxs-lookup"><span data-stu-id="6e825-132">Select a target framework of **.NET Core**, select **ASP.NET Core 2.1**, and click **OK**.</span></span>

  ![Okno dialogowe nowego projektu w programie Visual Studio](signalr/_static/signalr-new-project-choose-type.png)

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="6e825-134">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="6e825-134">Visual Studio Code</span></span>](#tab/visual-studio-code/)

* <span data-ttu-id="6e825-135">Otwórz [zintegrowany terminal](https://code.visualstudio.com/docs/editor/integrated-terminal) do folderu, w którym zostanie utworzony nowy folder projektu.</span><span class="sxs-lookup"><span data-stu-id="6e825-135">Open the [integrated terminal](https://code.visualstudio.com/docs/editor/integrated-terminal) to the folder in which the new project folder will be created.</span></span>

* <span data-ttu-id="6e825-136">Uruchom następujące polecenia:</span><span class="sxs-lookup"><span data-stu-id="6e825-136">Run the following commands:</span></span>

   ```console
   dotnet new webapp -o SignalRChat
   code -r SignalRChat
   ```

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="6e825-137">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="6e825-137">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="6e825-138">W menu, wybierz opcję **Plik > nowe rozwiązanie**.</span><span class="sxs-lookup"><span data-stu-id="6e825-138">From the menu, select **File > New Solution**.</span></span>

* <span data-ttu-id="6e825-139">Wybierz **platformy .NET Core > aplikacji > aplikacji internetowej ASP.NET Core** (nie wybieraj **ASP.NET Core Web App (MVC)**).</span><span class="sxs-lookup"><span data-stu-id="6e825-139">Select **.NET Core > App > ASP.NET Core Web App** (Don't select **ASP.NET Core Web App (MVC)**).</span></span>

* <span data-ttu-id="6e825-140">Wybierz **dalej**.</span><span class="sxs-lookup"><span data-stu-id="6e825-140">Select **Next**.</span></span>

* <span data-ttu-id="6e825-141">Nadaj projektowi nazwę *SignalRChat*, a następnie wybierz pozycję **Utwórz**.</span><span class="sxs-lookup"><span data-stu-id="6e825-141">Name the project *SignalRChat*, and then select **Create**.</span></span>

---

## <a name="add-the-signalr-client-library"></a><span data-ttu-id="6e825-142">Dodaj bibliotekę klienta SignalR</span><span class="sxs-lookup"><span data-stu-id="6e825-142">Add the SignalR client library</span></span>

<span data-ttu-id="6e825-143">Serwer biblioteki SignalR znajduje się w `Microsoft.AspNetCore.App` meta Microsoft.aspnetcore.all.</span><span class="sxs-lookup"><span data-stu-id="6e825-143">The SignalR server library is included in the `Microsoft.AspNetCore.App` metapackage.</span></span> <span data-ttu-id="6e825-144">Biblioteki klienta JavaScript automatycznie nie jest zawarty w projekcie.</span><span class="sxs-lookup"><span data-stu-id="6e825-144">The JavaScript client library isn't automatically included in the project.</span></span> <span data-ttu-id="6e825-145">W tym samouczku używasz Library Manager (LibMan) można pobrać z biblioteki klienta z *unpkg*.</span><span class="sxs-lookup"><span data-stu-id="6e825-145">For this tutorial, you use Library Manager (LibMan) to get the client library from *unpkg*.</span></span> <span data-ttu-id="6e825-146">unpkg jest usługa content delivery network (CDN)), można dostarczać niczego w Menedżer pakietów npm oraz Menedżera pakietów środowiska Node.js.</span><span class="sxs-lookup"><span data-stu-id="6e825-146">unpkg is a content delivery network (CDN)) that can deliver anything found in npm, the Node.js package manager.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="6e825-147">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="6e825-147">Visual Studio</span></span>](#tab/visual-studio/)

* <span data-ttu-id="6e825-148">W **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy projekt i wybierz **Dodaj** > **biblioteki po stronie klienta**.</span><span class="sxs-lookup"><span data-stu-id="6e825-148">In **Solution Explorer**, right-click the project, and select **Add** > **Client-Side Library**.</span></span>

* <span data-ttu-id="6e825-149">W **Dodaj biblioteki po stronie klienta** okno dialogowe, aby uzyskać **dostawcy** wybierz **unpkg**.</span><span class="sxs-lookup"><span data-stu-id="6e825-149">In the **Add Client-Side Library** dialog, for **Provider** select **unpkg**.</span></span> 

* <span data-ttu-id="6e825-150">Aby uzyskać **biblioteki**, wprowadź `@aspnet/signalr@1`i wybierz najnowszą wersję, która nie jest w wersji zapoznawczej.</span><span class="sxs-lookup"><span data-stu-id="6e825-150">For **Library**, enter `@aspnet/signalr@1`, and select the latest version that isn't preview.</span></span>

  ![Dodaj okno dialogowe biblioteki po stronie klienta — Wybieranie biblioteki](signalr/_static/libman1.png)

* <span data-ttu-id="6e825-152">Wybierz **wybierz konkretne pliki**, rozwiń węzeł *dist/przeglądarki* folder, a następnie wybierz *signalr.js* i *signalr.min.js*.</span><span class="sxs-lookup"><span data-stu-id="6e825-152">Select **Choose specific files**, expand the *dist/browser* folder, and select *signalr.js* and *signalr.min.js*.</span></span>

* <span data-ttu-id="6e825-153">Ustaw **lokalizacji docelowej** do *wwwroot/lib/signalr/* i wybierz **zainstalować**.</span><span class="sxs-lookup"><span data-stu-id="6e825-153">Set **Target Location** to *wwwroot/lib/signalr/*, and select **Install**.</span></span>

  ![Dodaj okno dialogowe z biblioteki klienta - wybranie plików i miejsce docelowe](signalr/_static/libman2.png)

  <span data-ttu-id="6e825-155">Tworzy LibMan *wwwroot/lib/signalr* folder i kopiuje wybrane pliki.</span><span class="sxs-lookup"><span data-stu-id="6e825-155">LibMan creates a *wwwroot/lib/signalr* folder and copies the selected files to it.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="6e825-156">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="6e825-156">Visual Studio Code</span></span>](#tab/visual-studio-code/)

* <span data-ttu-id="6e825-157">W zintegrowanym terminalu uruchom następujące polecenie, aby zainstalować LibMan.</span><span class="sxs-lookup"><span data-stu-id="6e825-157">In the integrated terminal, run the following command to install LibMan.</span></span>

  ```console
  dotnet tool install -g Microsoft.Web.LibraryManager.Cli
  ```

* <span data-ttu-id="6e825-158">Uruchom następujące polecenie, aby przy użyciu LibMan uzyskać biblioteki klienta SignalR.</span><span class="sxs-lookup"><span data-stu-id="6e825-158">Run the following command to get the SignalR client library by using LibMan.</span></span> <span data-ttu-id="6e825-159">Być może musisz odczekaj kilka sekund, zanim dane wyjściowe.</span><span class="sxs-lookup"><span data-stu-id="6e825-159">You might have to wait a few seconds before seeing output.</span></span>

  ```console
  libman install @aspnet/signalr -p unpkg -d wwwroot/lib/signalr --files dist/browser/signalr.js --files dist/browser/signalr.min.js
  ```

  <span data-ttu-id="6e825-160">Parametry określ następujące opcje:</span><span class="sxs-lookup"><span data-stu-id="6e825-160">The parameters specify the following options:</span></span>
  * <span data-ttu-id="6e825-161">Użyj dostawcy unpkg.</span><span class="sxs-lookup"><span data-stu-id="6e825-161">Use the unpkg provider.</span></span>
  * <span data-ttu-id="6e825-162">Skopiuj pliki do *wwwroot/lib/signalr* docelowego.</span><span class="sxs-lookup"><span data-stu-id="6e825-162">Copy files to the *wwwroot/lib/signalr* destination.</span></span>
  * <span data-ttu-id="6e825-163">Skopiuj tylko określonych plików.</span><span class="sxs-lookup"><span data-stu-id="6e825-163">Copy only the specified files.</span></span>

  <span data-ttu-id="6e825-164">Dane wyjściowe wyglądają jak w poniższym przykładzie:</span><span class="sxs-lookup"><span data-stu-id="6e825-164">The output looks like the following example:</span></span>

  ```console
  wwwroot/lib/signalr/dist/browser/signalr.js written to disk
  wwwroot/lib/signalr/dist/browser/signalr.min.js written to disk
  Installed library "@aspnet/signalr@1.0.3" to "wwwroot/lib/signalr"
  ```

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="6e825-165">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="6e825-165">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="6e825-166">W **terminalu**, uruchom następujące polecenie, aby zainstalować LibMan.</span><span class="sxs-lookup"><span data-stu-id="6e825-166">In the **Terminal**, run the following command to install LibMan.</span></span>

  ```console
  dotnet tool install -g Microsoft.Web.LibraryManager.Cli
  ```

* <span data-ttu-id="6e825-167">Przejdź do folderu projektu (zawierającego *SignalRChat.csproj* pliku).</span><span class="sxs-lookup"><span data-stu-id="6e825-167">Navigate to the project folder (the one that contains the *SignalRChat.csproj* file).</span></span>

* <span data-ttu-id="6e825-168">Uruchom następujące polecenie, aby przy użyciu LibMan uzyskać biblioteki klienta SignalR.</span><span class="sxs-lookup"><span data-stu-id="6e825-168">Run the following command to get the SignalR client library by using LibMan.</span></span>

  ```console
  libman install @aspnet/signalr -p unpkg -d wwwroot/lib/signalr --files dist/browser/signalr.js --files dist/browser/signalr.min.js
  ```

  <span data-ttu-id="6e825-169">Parametry określ następujące opcje:</span><span class="sxs-lookup"><span data-stu-id="6e825-169">The parameters specify the following options:</span></span>
  * <span data-ttu-id="6e825-170">Użyj dostawcy unpkg.</span><span class="sxs-lookup"><span data-stu-id="6e825-170">Use the unpkg provider.</span></span>
  * <span data-ttu-id="6e825-171">Skopiuj pliki do *wwwroot/lib/signalr* docelowego.</span><span class="sxs-lookup"><span data-stu-id="6e825-171">Copy files to the *wwwroot/lib/signalr* destination.</span></span>
  * <span data-ttu-id="6e825-172">Skopiuj tylko określonych plików.</span><span class="sxs-lookup"><span data-stu-id="6e825-172">Copy only the specified files.</span></span>

  <span data-ttu-id="6e825-173">Dane wyjściowe wyglądają jak w poniższym przykładzie:</span><span class="sxs-lookup"><span data-stu-id="6e825-173">The output looks like the following example:</span></span>

  ```console
  wwwroot/lib/signalr/dist/browser/signalr.js written to disk
  wwwroot/lib/signalr/dist/browser/signalr.min.js written to disk
  Installed library "@aspnet/signalr@1.0.3" to "wwwroot/lib/signalr"
  ```

---

## <a name="create-a-signalr-hub"></a><span data-ttu-id="6e825-174">Tworzenie Centrum SignalR</span><span class="sxs-lookup"><span data-stu-id="6e825-174">Create a SignalR hub</span></span>

<span data-ttu-id="6e825-175">A *Centrum* jest klasa, która służy jako ogólny potok, który obsługuje komunikację klient serwer.</span><span class="sxs-lookup"><span data-stu-id="6e825-175">A *hub* is a class that serves as a high-level pipeline that handles client-server communication.</span></span>

* <span data-ttu-id="6e825-176">W folderze projektu SignalRChat Utwórz *koncentratory* folderu.</span><span class="sxs-lookup"><span data-stu-id="6e825-176">In the SignalRChat project folder, create a *Hubs* folder.</span></span>

* <span data-ttu-id="6e825-177">W *Hubs* folderze utwórz *ChatHub.cs* pliku następującym kodem:</span><span class="sxs-lookup"><span data-stu-id="6e825-177">In the *Hubs* folder, create a *ChatHub.cs* file with the following code:</span></span>

  [!code-csharp[Startup](signalr/sample/Hubs/ChatHub.cs)]

  <span data-ttu-id="6e825-178">`ChatHub` Klasa dziedziczy z elementu SignalR `Hub` klasy.</span><span class="sxs-lookup"><span data-stu-id="6e825-178">The `ChatHub` class inherits from the SignalR `Hub` class.</span></span> <span data-ttu-id="6e825-179">`Hub` Klasa zarządza połączeń, grup i komunikatów.</span><span class="sxs-lookup"><span data-stu-id="6e825-179">The `Hub` class manages connections, groups, and messaging.</span></span>

  <span data-ttu-id="6e825-180">`SendMessage` Metoda może być wywoływana przez wszystkie połączone klienta.</span><span class="sxs-lookup"><span data-stu-id="6e825-180">The `SendMessage` method can be called by any connected client.</span></span> <span data-ttu-id="6e825-181">Wysyła odebranego komunikatu do wszystkich klientów.</span><span class="sxs-lookup"><span data-stu-id="6e825-181">It sends the received message to all clients.</span></span> <span data-ttu-id="6e825-182">Kod SignalR jest asynchroniczne w celu zapewnienia maksymalnej skalowalności.</span><span class="sxs-lookup"><span data-stu-id="6e825-182">SignalR code is asynchronous to provide maximum scalability.</span></span>

## <a name="configure-signalr"></a><span data-ttu-id="6e825-183">Konfigurowanie biblioteki SignalR</span><span class="sxs-lookup"><span data-stu-id="6e825-183">Configure SignalR</span></span>

<span data-ttu-id="6e825-184">Serwer biblioteki SignalR musi być skonfigurowany do przekazywania żądań SignalR z SignalR.</span><span class="sxs-lookup"><span data-stu-id="6e825-184">The SignalR server must be configured to pass SignalR requests to SignalR.</span></span>

* <span data-ttu-id="6e825-185">Dodaj następujący wyróżniony kod do *Startup.cs* pliku.</span><span class="sxs-lookup"><span data-stu-id="6e825-185">Add the following highlighted code to the *Startup.cs* file.</span></span>

  [!code-csharp[Startup](signalr/sample/Startup.cs?highlight=7,33,52-55)]

  <span data-ttu-id="6e825-186">Te zmiany dodanie SignalR systemu iniekcji zależności platformy ASP.NET Core i potoku oprogramowania pośredniczącego.</span><span class="sxs-lookup"><span data-stu-id="6e825-186">These changes add SignalR to the ASP.NET Core dependency injection system and the middleware pipeline.</span></span>

## <a name="add-signalr-client-code"></a><span data-ttu-id="6e825-187">Dodaj kod klienta SignalR</span><span class="sxs-lookup"><span data-stu-id="6e825-187">Add SignalR client code</span></span>

* <span data-ttu-id="6e825-188">Zastąp zawartość *Pages\Index.cshtml* następującym kodem:</span><span class="sxs-lookup"><span data-stu-id="6e825-188">Replace the content in *Pages\Index.cshtml* with the following code:</span></span>

  [!code-cshtml[Index](signalr/sample/Pages/Index.cshtml)]

  <span data-ttu-id="6e825-189">Powyższy kod:</span><span class="sxs-lookup"><span data-stu-id="6e825-189">The preceding code:</span></span>

  * <span data-ttu-id="6e825-190">Tworzy pola tekstowe nazwy i tekst wiadomość i przycisk Prześlij.</span><span class="sxs-lookup"><span data-stu-id="6e825-190">Creates text boxes for name and message text, and a submit button.</span></span>
  * <span data-ttu-id="6e825-191">Tworzy listę z `id="messagesList"` do wyświetlania komunikatów odebranych z Centrum SignalR.</span><span class="sxs-lookup"><span data-stu-id="6e825-191">Creates a list with `id="messagesList"` for displaying messages that are received from the SignalR hub.</span></span>
  * <span data-ttu-id="6e825-192">Zawiera odwołania do skryptu z SignalR i *chat.js* kod aplikacji, który zostanie utworzony w następnym kroku.</span><span class="sxs-lookup"><span data-stu-id="6e825-192">Includes script references to SignalR and the *chat.js* application code that you create in the next step.</span></span>

* <span data-ttu-id="6e825-193">W *wwwroot/js* folderze utwórz *chat.js* pliku następującym kodem:</span><span class="sxs-lookup"><span data-stu-id="6e825-193">In the *wwwroot/js* folder, create a *chat.js* file with the following code:</span></span>

  [!code-javascript[Index](signalr/sample/wwwroot/js/chat.js)]

  <span data-ttu-id="6e825-194">Powyższy kod:</span><span class="sxs-lookup"><span data-stu-id="6e825-194">The preceding code:</span></span>

  * <span data-ttu-id="6e825-195">Tworzy i uruchamia połączenie.</span><span class="sxs-lookup"><span data-stu-id="6e825-195">Creates and starts a connection.</span></span>
  * <span data-ttu-id="6e825-196">Dodaje przycisk przesyłania program obsługi, która wysyła komunikaty do Centrum.</span><span class="sxs-lookup"><span data-stu-id="6e825-196">Adds to the submit button a handler that sends messages to the hub.</span></span>
  * <span data-ttu-id="6e825-197">Dodaje do obiektu połączenia program obsługi, który odbiera komunikaty z Centrum i dodaje je do listy.</span><span class="sxs-lookup"><span data-stu-id="6e825-197">Adds to the connection object a handler that receives messages from the hub and adds them to the list.</span></span>

## <a name="run-the-app"></a><span data-ttu-id="6e825-198">Uruchamianie aplikacji</span><span class="sxs-lookup"><span data-stu-id="6e825-198">Run the app</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="6e825-199">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="6e825-199">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="6e825-200">Naciśnij klawisz **kombinację klawiszy CTRL + F5** Aby uruchomić aplikację bez debugowania.</span><span class="sxs-lookup"><span data-stu-id="6e825-200">Press **CTRL+F5** to run the app without debugging.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="6e825-201">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="6e825-201">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="6e825-202">W zintegrowanym terminalu uruchom następujące polecenie:</span><span class="sxs-lookup"><span data-stu-id="6e825-202">In the integrated terminal, run the following command:</span></span>

  ```console
  dotnet run -p SignalRChat
  ```
  
# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="6e825-203">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="6e825-203">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="6e825-204">W menu, wybierz opcję **Uruchom > Uruchom bez debugowania**.</span><span class="sxs-lookup"><span data-stu-id="6e825-204">From the menu, select **Run > Start Without Debugging**.</span></span>

---

* <span data-ttu-id="6e825-205">Skopiuj adres URL z paska adresu, a następnie otwórz innego wystąpienia przeglądarki lub karty i wklej adres URL w pasku adresu.</span><span class="sxs-lookup"><span data-stu-id="6e825-205">Copy the URL from the address bar, open another browser instance or tab, and paste the URL in the address bar.</span></span>

* <span data-ttu-id="6e825-206">Wybierz albo przeglądarki, wprowadź nazwę i komunikat, a następnie wybierz **wysyłania komunikatu** przycisku.</span><span class="sxs-lookup"><span data-stu-id="6e825-206">Choose either browser, enter a name and message, and select the **Send Message** button.</span></span>

  <span data-ttu-id="6e825-207">Nazwa i wiadomości są wyświetlane na obu stronach natychmiast.</span><span class="sxs-lookup"><span data-stu-id="6e825-207">The name and message are displayed on both pages instantly.</span></span>

  ![SignalR przykładowej aplikacji](signalr/_static/signalr-get-started-finished.png)

## <a name="next-steps"></a><span data-ttu-id="6e825-209">Następne kroki</span><span class="sxs-lookup"><span data-stu-id="6e825-209">Next steps</span></span>

<span data-ttu-id="6e825-210">W tym samouczku przedstawiono sposób:</span><span class="sxs-lookup"><span data-stu-id="6e825-210">In this tutorial, you learned how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="6e825-211">Tworzenie projektu aplikacji sieci web.</span><span class="sxs-lookup"><span data-stu-id="6e825-211">Create a web app project.</span></span>
> * <span data-ttu-id="6e825-212">Dodaj bibliotekę klienta SignalR.</span><span class="sxs-lookup"><span data-stu-id="6e825-212">Add the SignalR client library.</span></span>
> * <span data-ttu-id="6e825-213">Utwórz Centrum SignalR.</span><span class="sxs-lookup"><span data-stu-id="6e825-213">Create a SignalR hub.</span></span>
> * <span data-ttu-id="6e825-214">Skonfiguruj projekt do korzystania z SignalR.</span><span class="sxs-lookup"><span data-stu-id="6e825-214">Configure the project to use SignalR.</span></span>
> * <span data-ttu-id="6e825-215">Dodaj kod używający koncentratora, aby wysyłać komunikaty z dowolnego klienta do wszystkich połączonych klientów.</span><span class="sxs-lookup"><span data-stu-id="6e825-215">Add code that uses the hub to send messages from any client to all connected clients.</span></span>

<span data-ttu-id="6e825-216">Aby dowiedzieć się więcej na temat biblioteki SignalR, zobacz wprowadzenie:</span><span class="sxs-lookup"><span data-stu-id="6e825-216">To learn more about SignalR, see the introduction:</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="6e825-217">Wprowadzenie do SignalR platformy ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="6e825-217">Introduction to ASP.NET Core SignalR</span></span>](xref:signalr/introduction)
