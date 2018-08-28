---
title: 'Samouczek: Wprowadzenie do SignalR platformy ASP.NET Core'
author: tdykstra
description: W tym samouczku utworzysz aplikację rozmowy, która używa biblioteki SignalR platformy ASP.NET Core.
monikerRange: '>= aspnetcore-2.1'
ms.author: tdykstra
ms.custom: mvc
ms.date: 08/20/2018
uid: tutorials/signalr
ms.openlocfilehash: a2573e2817a2d8921954264ca17bc3a7e2a010a8
ms.sourcegitcommit: 847cc1de5526ff42a7303491e6336c2dbdb45de4
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/27/2018
ms.locfileid: "43055835"
---
# <a name="tutorial-get-started-with-signalr-on-aspnet-core"></a><span data-ttu-id="efb4e-103">Samouczek: Wprowadzenie do SignalR platformy ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="efb4e-103">Tutorial: Get started with SignalR on ASP.NET Core</span></span>

<span data-ttu-id="efb4e-104">W tym samouczku pokazano podstawowe informacje dotyczące tworzenia aplikacji w czasie rzeczywistym przy użyciu biblioteki SignalR.</span><span class="sxs-lookup"><span data-stu-id="efb4e-104">This tutorial teaches the basics of building a real-time app using SignalR.</span></span> <span data-ttu-id="efb4e-105">Dowiesz się, jak:</span><span class="sxs-lookup"><span data-stu-id="efb4e-105">You learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="efb4e-106">Tworzenie aplikacji sieci web, która korzysta z biblioteki SignalR platformy ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="efb4e-106">Create a web app that uses SignalR on ASP.NET Core.</span></span>
> * <span data-ttu-id="efb4e-107">Utwórz Centrum SignalR na serwerze.</span><span class="sxs-lookup"><span data-stu-id="efb4e-107">Create a SignalR hub on the server.</span></span>
> * <span data-ttu-id="efb4e-108">Łączenie z Centrum SignalR poziomu klientów JavaScript.</span><span class="sxs-lookup"><span data-stu-id="efb4e-108">Connect to the SignalR hub from JavaScript clients.</span></span>
> * <span data-ttu-id="efb4e-109">Centrum umożliwia wysyłanie komunikatów za pomocą dowolnego klienta do wszystkich połączonych klientów.</span><span class="sxs-lookup"><span data-stu-id="efb4e-109">Use the hub to send messages from any client to all connected clients.</span></span>

<span data-ttu-id="efb4e-110">Po zakończeniu będziesz mieć działającą aplikację rozmowy:</span><span class="sxs-lookup"><span data-stu-id="efb4e-110">At the end, you'll have a working chat app:</span></span>

![SignalR przykładowej aplikacji](signalr/_static/signalr-get-started-finished.png)

<span data-ttu-id="efb4e-112">[Wyświetlanie lub pobieranie przykładowego kodu](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/signalr/sample) ([sposobu pobierania](xref:tutorials/index#how-to-download-a-sample)).</span><span class="sxs-lookup"><span data-stu-id="efb4e-112">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/signalr/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample)).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="efb4e-113">Wymagania wstępne</span><span class="sxs-lookup"><span data-stu-id="efb4e-113">Prerequisites</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="efb4e-114">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="efb4e-114">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="efb4e-115">[Visual Studio 2017 w wersji 15.7.3 lub nowszej](https://www.visualstudio.com/downloads/) z **ASP.NET i tworzenie aplikacji internetowych** obciążenia</span><span class="sxs-lookup"><span data-stu-id="efb4e-115">[Visual Studio 2017 version 15.7.3 or later](https://www.visualstudio.com/downloads/) with the **ASP.NET and web development** workload</span></span>
* [<span data-ttu-id="efb4e-116">.NET core SDK 2.1 lub nowszej</span><span class="sxs-lookup"><span data-stu-id="efb4e-116">.NET Core SDK 2.1 or later</span></span>](https://www.microsoft.com/net/download/all)
* <span data-ttu-id="efb4e-117">[npm](https://www.npmjs.com/get-npm) (Menedżer pakietów dla środowiska Node.js, używany dla biblioteki klienta SignalR JavaScript).</span><span class="sxs-lookup"><span data-stu-id="efb4e-117">[npm](https://www.npmjs.com/get-npm) (Package manager for Node.js, used for the SignalR JavaScript client library.)</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="efb4e-118">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="efb4e-118">Visual Studio Code</span></span>](#tab/visual-studio-code)

* [<span data-ttu-id="efb4e-119">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="efb4e-119">Visual Studio Code</span></span>](https://code.visualstudio.com/download)
* [<span data-ttu-id="efb4e-120">.NET core SDK 2.1 lub nowszej</span><span class="sxs-lookup"><span data-stu-id="efb4e-120">.NET Core SDK 2.1 or later</span></span>](https://www.microsoft.com/net/download/all)
* [<span data-ttu-id="efb4e-121">Środowisko C# dla programu Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="efb4e-121">C# for Visual Studio Code</span></span>](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp)
* <span data-ttu-id="efb4e-122">[npm](https://www.npmjs.com/get-npm) (Menedżer pakietów dla środowiska Node.js, używany dla biblioteki klienta SignalR JavaScript).</span><span class="sxs-lookup"><span data-stu-id="efb4e-122">[npm](https://www.npmjs.com/get-npm) (Package manager for Node.js, used for the SignalR JavaScript client library.)</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="efb4e-123">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="efb4e-123">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* [<span data-ttu-id="efb4e-124">Program Visual Studio dla komputerów Mac w wersji 7.5.4 lub nowszy</span><span class="sxs-lookup"><span data-stu-id="efb4e-124">Visual Studio for Mac version 7.5.4 or later</span></span>](https://www.visualstudio.com/downloads/)
* <span data-ttu-id="efb4e-125">[.NET core SDK 2.1 lub nowszej](https://www.microsoft.com/net/download/all) (dołączone do instalacji programu Visual Studio)</span><span class="sxs-lookup"><span data-stu-id="efb4e-125">[.NET Core SDK 2.1 or later](https://www.microsoft.com/net/download/all) (included in the Visual Studio install)</span></span>
* <span data-ttu-id="efb4e-126">[npm](https://www.npmjs.com/get-npm) (Menedżer pakietów dla środowiska Node.js, używany dla biblioteki klienta SignalR JavaScript).</span><span class="sxs-lookup"><span data-stu-id="efb4e-126">[npm](https://www.npmjs.com/get-npm) (Package manager for Node.js, used for the SignalR JavaScript client library.)</span></span>

---

## <a name="create-the-project"></a><span data-ttu-id="efb4e-127">Utwórz projekt</span><span class="sxs-lookup"><span data-stu-id="efb4e-127">Create the project</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="efb4e-128">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="efb4e-128">Visual Studio</span></span>](#tab/visual-studio/)

* <span data-ttu-id="efb4e-129">W menu, wybierz opcję **Plik > Nowy projekt**.</span><span class="sxs-lookup"><span data-stu-id="efb4e-129">From the menu, select **File > New Project**.</span></span>

* <span data-ttu-id="efb4e-130">W **nowy projekt** okno dialogowe, wybierz opcję **zainstalowane > Visual C# > sieci Web > Aplikacja sieci Web programu ASP.NET Core**.</span><span class="sxs-lookup"><span data-stu-id="efb4e-130">In the **New Project** dialog, select **Installed > Visual C# > Web > ASP.NET Core Web Application**.</span></span> <span data-ttu-id="efb4e-131">Nadaj projektowi nazwę *SignalRChat*.</span><span class="sxs-lookup"><span data-stu-id="efb4e-131">Name the project *SignalRChat*.</span></span>

  ![Okno dialogowe nowego projektu w programie Visual Studio](signalr/_static/signalr-new-project-dialog.png)

* <span data-ttu-id="efb4e-133">Wybierz **aplikacji sieci Web** do tworzenia projektu, który używa stron Razor.</span><span class="sxs-lookup"><span data-stu-id="efb4e-133">Select **Web Application** to create a project that uses Razor Pages.</span></span>

* <span data-ttu-id="efb4e-134">Wybierz docelową platformę **platformy .NET Core**, wybierz opcję **platformy ASP.NET Core 2.1**i kliknij przycisk **OK**.</span><span class="sxs-lookup"><span data-stu-id="efb4e-134">Select a target framework of **.NET Core**, select **ASP.NET Core 2.1**, and click **OK**.</span></span>

  ![Okno dialogowe nowego projektu w programie Visual Studio](signalr/_static/signalr-new-project-choose-type.png)

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="efb4e-136">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="efb4e-136">Visual Studio Code</span></span>](#tab/visual-studio-code/)

* <span data-ttu-id="efb4e-137">Otwórz folder, który można użyć dla nowego projektu.</span><span class="sxs-lookup"><span data-stu-id="efb4e-137">Open a folder that you can use for a new project.</span></span>

* <span data-ttu-id="efb4e-138">W **zintegrowany Terminal**, uruchom następujące polecenie:</span><span class="sxs-lookup"><span data-stu-id="efb4e-138">In the **Integrated Terminal**, run the following command:</span></span>

   ```console
   dotnet new webapp -o SignalRChat
   ```

   [!INCLUDE[](~/includes/webapp-alias-notice.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="efb4e-139">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="efb4e-139">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="efb4e-140">W menu, wybierz opcję **Plik > nowe rozwiązanie**.</span><span class="sxs-lookup"><span data-stu-id="efb4e-140">From the menu, select **File > New Solution**.</span></span>

* <span data-ttu-id="efb4e-141">Wybierz **platformy .NET Core > aplikacji > aplikacji internetowej ASP.NET Core** (nie wybieraj **ASP.NET Core Web App (MVC)**).</span><span class="sxs-lookup"><span data-stu-id="efb4e-141">Select **.NET Core > App > ASP.NET Core Web App** (Don't select **ASP.NET Core Web App (MVC)**).</span></span>

* <span data-ttu-id="efb4e-142">Wybierz **dalej**.</span><span class="sxs-lookup"><span data-stu-id="efb4e-142">Select **Next**.</span></span>

* <span data-ttu-id="efb4e-143">Nadaj projektowi nazwę *SignalRChat*, a następnie wybierz pozycję **Utwórz**.</span><span class="sxs-lookup"><span data-stu-id="efb4e-143">Name the project *SignalRChat*, and then select **Create**.</span></span>

---

## <a name="add-the-signalr-client-library"></a><span data-ttu-id="efb4e-144">Dodaj bibliotekę klienta SignalR</span><span class="sxs-lookup"><span data-stu-id="efb4e-144">Add the SignalR client library</span></span>

<span data-ttu-id="efb4e-145">Serwer biblioteki SignalR znajduje się w [meta Microsoft.aspnetcore.all Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="efb4e-145">The SignalR server library is included in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span> <span data-ttu-id="efb4e-146">Ale trzeba uzyskać biblioteki klienckiej JavaScript z [npm, Menedżer pakietów Node.js](https://www.npmjs.com/get-npm).</span><span class="sxs-lookup"><span data-stu-id="efb4e-146">But you have to get the JavaScript client library from [npm, the Node.js package manager](https://www.npmjs.com/get-npm).</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="efb4e-147">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="efb4e-147">Visual Studio</span></span>](#tab/visual-studio/)

* <span data-ttu-id="efb4e-148">W **Konsola Menedżera pakietów** (PMC), przejdź do folderu projektu (zawierającego *SignalRChat.csproj* pliku).</span><span class="sxs-lookup"><span data-stu-id="efb4e-148">In **Package Manager Console** (PMC), change to the project folder (the one that contains the *SignalRChat.csproj* file).</span></span>

  ```console
  cd SignalRChat
  ```

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="efb4e-149">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="efb4e-149">Visual Studio Code</span></span>](#tab/visual-studio-code/)

2. <span data-ttu-id="efb4e-150">Przejdź do nowego folderu projektu.</span><span class="sxs-lookup"><span data-stu-id="efb4e-150">Change to the new project folder.</span></span>

  ```console
  cd SignalRChat
  ``` 

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="efb4e-151">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="efb4e-151">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="efb4e-152">W **terminalu**, przejdź do folderu projektu (zawierającego *SignalRChat.csproj* pliku).</span><span class="sxs-lookup"><span data-stu-id="efb4e-152">In the **Terminal**, navigate to the project folder (the one that contains the *SignalRChat.csproj* file).</span></span>

---

* <span data-ttu-id="efb4e-153">Uruchom inicjator npm, aby utworzyć *package.json* pliku:</span><span class="sxs-lookup"><span data-stu-id="efb4e-153">Run the npm initializer to create a *package.json* file:</span></span>

  ```console
  npm init -y
  ```

  <span data-ttu-id="efb4e-154">Polecenie tworzy dane wyjściowe podobne do poniższego przykładu:</span><span class="sxs-lookup"><span data-stu-id="efb4e-154">The command creates output similar to the following example:</span></span>

  ```console
  Wrote to C:\tmp\SignalRChat\package.json:
  {
    "name": "SignalRChat",
    "version": "1.0.0",
    "description": "",
    "main": "index.js",
    "scripts": {
      "test": "echo \"Error: no test specified\" && exit 1"
    },
    "keywords": [],
    "author": "",
    "license": "ISC"0
  }
  ```

* <span data-ttu-id="efb4e-155">Instalowanie pakietu biblioteki klienta:</span><span class="sxs-lookup"><span data-stu-id="efb4e-155">Install the client library package:</span></span>

  ```console
  npm install @aspnet/signalr
  ```

  <span data-ttu-id="efb4e-156">Polecenie tworzy dane wyjściowe podobne do poniższego przykładu:</span><span class="sxs-lookup"><span data-stu-id="efb4e-156">The command creates output similar to the following example:</span></span>

  ```
  npm notice created a lockfile as package-lock.json. You should commit this file.
  npm WARN signalrchat@1.0.0 No description
  npm WARN signalrchat@1.0.0 No repository field.

  + @aspnet/signalr@1.0.2
  added 1 package in 0.98s
  ```

<span data-ttu-id="efb4e-157">`npm install` Polecenia pobrane z biblioteki klienta JavaScript w podfolderze *node_modules*.</span><span class="sxs-lookup"><span data-stu-id="efb4e-157">The `npm install` command downloaded the JavaScript client library to a subfolder under *node_modules*.</span></span> <span data-ttu-id="efb4e-158">Skopiuj go stamtąd do folderu, w obszarze *wwwroot* odwołujący ze strony sieci web aplikacji rozmów.</span><span class="sxs-lookup"><span data-stu-id="efb4e-158">Copy it from there to a folder under *wwwroot* that you can reference from the chat app web page.</span></span>

* <span data-ttu-id="efb4e-159">Tworzenie *signalr* folderu w *wwwroot/lib*.</span><span class="sxs-lookup"><span data-stu-id="efb4e-159">Create a *signalr* folder in *wwwroot/lib*.</span></span>

* <span data-ttu-id="efb4e-160">Kopiuj *signalr.js* plik wchodzącej w skład *node_modules/@aspnet/signalr/dist/browser* do nowego *wwwroot/lib/signalr* folderu.</span><span class="sxs-lookup"><span data-stu-id="efb4e-160">Copy the *signalr.js* file from *node_modules/@aspnet/signalr/dist/browser* to the new *wwwroot/lib/signalr* folder.</span></span>

## <a name="create-the-signalr-hub"></a><span data-ttu-id="efb4e-161">Tworzenie Centrum SignalR</span><span class="sxs-lookup"><span data-stu-id="efb4e-161">Create the SignalR hub</span></span>

<span data-ttu-id="efb4e-162">A [Centrum](xref:signalr/hubs) jest klasa, która służy jako ogólny potok, który obsługuje komunikację klient serwer.</span><span class="sxs-lookup"><span data-stu-id="efb4e-162">A [hub](xref:signalr/hubs) is a class that serves as a high-level pipeline that handles client-server communication.</span></span>

* <span data-ttu-id="efb4e-163">W folderze projektu SignalRChat Utwórz *koncentratory* folderu.</span><span class="sxs-lookup"><span data-stu-id="efb4e-163">In the SignalRChat project folder, create a *Hubs* folder.</span></span>

* <span data-ttu-id="efb4e-164">W *Hubs* folderze utwórz *ChatHub.cs* pliku następującym kodem:</span><span class="sxs-lookup"><span data-stu-id="efb4e-164">In the *Hubs* folder, create a *ChatHub.cs* file with the following code:</span></span>

  [!code-csharp[Startup](signalr/sample/Hubs/ChatHub.cs)]

  <span data-ttu-id="efb4e-165">`ChatHub` Klasa dziedziczy z elementu SignalR [Centrum](/dotnet/api/microsoft.aspnetcore.signalr.hub) klasy.</span><span class="sxs-lookup"><span data-stu-id="efb4e-165">The `ChatHub` class inherits from the SignalR [Hub](/dotnet/api/microsoft.aspnetcore.signalr.hub) class.</span></span> <span data-ttu-id="efb4e-166">`Hub` Klasa zarządza połączeń, grup i komunikatów.</span><span class="sxs-lookup"><span data-stu-id="efb4e-166">The `Hub` class manages connections, groups, and messaging.</span></span>

  <span data-ttu-id="efb4e-167">`SendMessage` Metoda może być wywoływana przez wszystkie połączone klienta.</span><span class="sxs-lookup"><span data-stu-id="efb4e-167">The `SendMessage` method can be called by any connected client.</span></span> <span data-ttu-id="efb4e-168">Wysyła odebranego komunikatu do wszystkich klientów.</span><span class="sxs-lookup"><span data-stu-id="efb4e-168">It sends the received message to all clients.</span></span> <span data-ttu-id="efb4e-169">Kod SignalR jest asynchroniczne w celu zapewnienia maksymalnej skalowalności.</span><span class="sxs-lookup"><span data-stu-id="efb4e-169">SignalR code is asynchronous to provide maximum scalability.</span></span>

## <a name="configure-the-project-to-use-signalr"></a><span data-ttu-id="efb4e-170">Konfigurowanie projektu do korzystania z biblioteki SignalR</span><span class="sxs-lookup"><span data-stu-id="efb4e-170">Configure the project to use SignalR</span></span>

<span data-ttu-id="efb4e-171">Serwer biblioteki SignalR musi być skonfigurowany do przekazywania żądań SignalR z SignalR.</span><span class="sxs-lookup"><span data-stu-id="efb4e-171">The SignalR server must be configured to pass SignalR requests to SignalR.</span></span>

* <span data-ttu-id="efb4e-172">Dodaj następujący wyróżniony kod do *Startup.cs* pliku.</span><span class="sxs-lookup"><span data-stu-id="efb4e-172">Add the following highlighted code to the *Startup.cs* file.</span></span>

  [!code-csharp[Startup](signalr/sample/Startup.cs?highlight=7,33,52-55)]

  <span data-ttu-id="efb4e-173">Te zmiany dodanie SignalR do [wstrzykiwanie zależności](xref:fundamentals/dependency-injection) systemu i [oprogramowania pośredniczącego](xref:fundamentals/middleware/index) potoku.</span><span class="sxs-lookup"><span data-stu-id="efb4e-173">These changes add SignalR to the [dependency injection](xref:fundamentals/dependency-injection) system and the [middleware](xref:fundamentals/middleware/index) pipeline.</span></span>

## <a name="create-the-signalr-client-code"></a><span data-ttu-id="efb4e-174">Tworzenie kodu klienta SignalR</span><span class="sxs-lookup"><span data-stu-id="efb4e-174">Create the SignalR client code</span></span>

* <span data-ttu-id="efb4e-175">Zastąp zawartość *Pages\Index.cshtml* następującym kodem:</span><span class="sxs-lookup"><span data-stu-id="efb4e-175">Replace the content in *Pages\Index.cshtml* with the following:</span></span>

  [!code-cshtml[Index](signalr/sample/Pages/Index.cshtml)]

  <span data-ttu-id="efb4e-176">Powyższy kod:</span><span class="sxs-lookup"><span data-stu-id="efb4e-176">The preceding code:</span></span>

  * <span data-ttu-id="efb4e-177">Tworzy pola tekstowe nazwy i tekst wiadomość i przycisk Prześlij.</span><span class="sxs-lookup"><span data-stu-id="efb4e-177">Creates text boxes for name and message text, and a submit button.</span></span>
  * <span data-ttu-id="efb4e-178">Tworzy listę z `id="messagesList"` do wyświetlania komunikatów odebranych z Centrum SignalR.</span><span class="sxs-lookup"><span data-stu-id="efb4e-178">Creates a list with `id="messagesList"` for displaying messages that are received from the SignalR hub.</span></span>
  * <span data-ttu-id="efb4e-179">Zawiera odwołania do skryptu z SignalR i *chat.js* kod aplikacji, który zostanie utworzony w następnym kroku.</span><span class="sxs-lookup"><span data-stu-id="efb4e-179">Includes script references to SignalR and the *chat.js* application code that you create in the next step.</span></span>

* <span data-ttu-id="efb4e-180">W *wwwroot/js* folderze utwórz *chat.js* pliku następującym kodem:</span><span class="sxs-lookup"><span data-stu-id="efb4e-180">In the *wwwroot/js* folder, create a *chat.js* file with the following code:</span></span>

  [!code-javascript[Index](signalr/sample/wwwroot/js/chat.js)]

  <span data-ttu-id="efb4e-181">Powyższy kod:</span><span class="sxs-lookup"><span data-stu-id="efb4e-181">The preceding code:</span></span>

  * <span data-ttu-id="efb4e-182">Tworzy i uruchamia połączenie.</span><span class="sxs-lookup"><span data-stu-id="efb4e-182">Creates and starts a connection.</span></span>
  * <span data-ttu-id="efb4e-183">Dodaje przycisk przesyłania program obsługi, która wysyła komunikaty do Centrum.</span><span class="sxs-lookup"><span data-stu-id="efb4e-183">Adds to the submit button a handler that sends messages to the hub.</span></span>
  * <span data-ttu-id="efb4e-184">Dodaje do obiektu połączenia program obsługi, który odbiera komunikaty z Centrum i dodaje je do listy.</span><span class="sxs-lookup"><span data-stu-id="efb4e-184">Adds to the connection object a handler that receives messages from the hub and adds them to the list.</span></span>

## <a name="run-the-app"></a><span data-ttu-id="efb4e-185">Uruchamianie aplikacji</span><span class="sxs-lookup"><span data-stu-id="efb4e-185">Run the app</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="efb4e-186">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="efb4e-186">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="efb4e-187">Naciśnij klawisz **kombinację klawiszy CTRL + F5** Aby uruchomić aplikację bez debugowania.</span><span class="sxs-lookup"><span data-stu-id="efb4e-187">Press **CTRL+F5** to run the app without debugging.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="efb4e-188">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="efb4e-188">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="efb4e-189">Naciśnij klawisz **kombinację klawiszy CTRL + F5** Aby uruchomić aplikację bez debugowania.</span><span class="sxs-lookup"><span data-stu-id="efb4e-189">Press **CTRL+F5** to run the app without debugging.</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="efb4e-190">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="efb4e-190">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="efb4e-191">W menu, wybierz opcję **Uruchom > Uruchom bez debugowania**.</span><span class="sxs-lookup"><span data-stu-id="efb4e-191">From the menu, select **Run > Start Without Debugging**.</span></span>

---

* <span data-ttu-id="efb4e-192">Skopiuj adres URL z paska adresu, a następnie otwórz innego wystąpienia przeglądarki lub karty i wklej adres URL w pasku adresu.</span><span class="sxs-lookup"><span data-stu-id="efb4e-192">Copy the URL from the address bar, open another browser instance or tab, and paste the URL in the address bar.</span></span>

* <span data-ttu-id="efb4e-193">Wybierz albo przeglądarki, wprowadź nazwę i komunikat, a następnie wybierz **wysyłania** przycisku.</span><span class="sxs-lookup"><span data-stu-id="efb4e-193">Choose either browser, enter a name and message, and select the **Send** button.</span></span>

  <span data-ttu-id="efb4e-194">Nazwa i wiadomości są wyświetlane na obu stronach natychmiast.</span><span class="sxs-lookup"><span data-stu-id="efb4e-194">The name and message are displayed on both pages instantly.</span></span>

  ![SignalR przykładowej aplikacji](signalr/_static/signalr-get-started-finished.png)

> [!TIP]
> <span data-ttu-id="efb4e-196">Jeśli aplikacja nie działa, Otwórz swoje narzędzia deweloperskie przeglądarki (F12) i przejdź do konsoli.</span><span class="sxs-lookup"><span data-stu-id="efb4e-196">If the app doesn't work, open your browser developer tools (F12) and go to the console.</span></span> <span data-ttu-id="efb4e-197">Może pojawić się błędy związane z Twoim kodem HTML i JavaScript.</span><span class="sxs-lookup"><span data-stu-id="efb4e-197">You might see errors related to your HTML and JavaScript code.</span></span> <span data-ttu-id="efb4e-198">Załóżmy, że możesz umieścić *signalr.js* w innym folderze niż bezpośredniego.</span><span class="sxs-lookup"><span data-stu-id="efb4e-198">For example, suppose you put *signalr.js* in a different folder than directed.</span></span> <span data-ttu-id="efb4e-199">W takim przypadku odwołania do tego pliku nie będzie działać, a następnie zostanie wyświetlony błąd 404 w konsoli.</span><span class="sxs-lookup"><span data-stu-id="efb4e-199">In that case the reference to that file won't work and you'll see a 404 error in the console.</span></span>
> <span data-ttu-id="efb4e-200">![błąd — nie znaleziono signalr.js](signalr/_static/f12-console.png)</span><span class="sxs-lookup"><span data-stu-id="efb4e-200">![signalr.js not found error](signalr/_static/f12-console.png)</span></span>

## <a name="next-steps"></a><span data-ttu-id="efb4e-201">Następne kroki</span><span class="sxs-lookup"><span data-stu-id="efb4e-201">Next steps</span></span>

<span data-ttu-id="efb4e-202">Klientom na łączenie się z aplikacji SignalR w różnych domenach, należy włączyć udostępnianie zasobów między źródłami (CORS).</span><span class="sxs-lookup"><span data-stu-id="efb4e-202">If you want clients to connect to a SignalR app from different domains, you have to enable Cross-Origin Resource Sharing (CORS).</span></span> <span data-ttu-id="efb4e-203">Aby uzyskać więcej informacji, zobacz [Cross-origin resource sharing](xref:signalr/security?view=aspnetcore-2.1#cross-origin-resource-sharing).</span><span class="sxs-lookup"><span data-stu-id="efb4e-203">For more information, see [Cross-origin resource sharing](xref:signalr/security?view=aspnetcore-2.1#cross-origin-resource-sharing).</span></span>

<span data-ttu-id="efb4e-204">Aby dowiedzieć się więcej na temat biblioteki SignalR, koncentratorów i klientów języka JavaScript, zobacz następujące zasoby:</span><span class="sxs-lookup"><span data-stu-id="efb4e-204">To learn more about SignalR, hubs, and JavaScript clients, see these resources:</span></span>

* [<span data-ttu-id="efb4e-205">Wprowadzenie do SignalR dla platformy ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="efb4e-205">Introduction to SignalR for ASP.NET Core</span></span>](xref:signalr/introduction)
* [<span data-ttu-id="efb4e-206">Na użytek koncentratory w SignalR platformy ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="efb4e-206">Use hubs in SignalR for ASP.NET Core</span></span>](xref:signalr/hubs)
* [<span data-ttu-id="efb4e-207">ASP.NET Core SignalR JavaScript klienta</span><span class="sxs-lookup"><span data-stu-id="efb4e-207">ASP.NET Core SignalR JavaScript client</span></span>](xref:signalr/javascript-client)
