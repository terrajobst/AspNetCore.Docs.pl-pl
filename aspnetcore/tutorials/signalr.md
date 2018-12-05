---
title: Wprowadzenie do biblioteki SignalR platformy ASP.NET Core
author: tdykstra
description: W tym samouczku utworzysz aplikację rozmowy, która korzysta z biblioteki SignalR platformy ASP.NET Core.
monikerRange: '>= aspnetcore-2.1'
ms.author: tdykstra
ms.custom: mvc
ms.date: 11/30/2018
uid: tutorials/signalr
ms.openlocfilehash: c52041b34d6c9d1d8f06f980c900b805a0933293
ms.sourcegitcommit: 9bb58d7c8dad4bbd03419bcc183d027667fefa20
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 12/04/2018
ms.locfileid: "52861990"
---
# <a name="tutorial-get-started-with-aspnet-core-signalr"></a><span data-ttu-id="4c1ad-103">Samouczek: Rozpoczynanie pracy przy użyciu biblioteki SignalR platformy ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="4c1ad-103">Tutorial: Get started with ASP.NET Core SignalR</span></span>

<span data-ttu-id="4c1ad-104">W tym samouczku pokazano podstawowe informacje dotyczące tworzenia aplikacji w czasie rzeczywistym przy użyciu biblioteki SignalR.</span><span class="sxs-lookup"><span data-stu-id="4c1ad-104">This tutorial teaches the basics of building a real-time app using SignalR.</span></span> <span data-ttu-id="4c1ad-105">Dowiesz się, jak:</span><span class="sxs-lookup"><span data-stu-id="4c1ad-105">You learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="4c1ad-106">Utwórz projekt sieci web.</span><span class="sxs-lookup"><span data-stu-id="4c1ad-106">Create a web project.</span></span>
> * <span data-ttu-id="4c1ad-107">Dodaj bibliotekę klienta SignalR.</span><span class="sxs-lookup"><span data-stu-id="4c1ad-107">Add the SignalR client library.</span></span>
> * <span data-ttu-id="4c1ad-108">Utwórz Centrum SignalR.</span><span class="sxs-lookup"><span data-stu-id="4c1ad-108">Create a SignalR hub.</span></span>
> * <span data-ttu-id="4c1ad-109">Skonfiguruj projekt do korzystania z SignalR.</span><span class="sxs-lookup"><span data-stu-id="4c1ad-109">Configure the project to use SignalR.</span></span>
> * <span data-ttu-id="4c1ad-110">Dodaj kod, który wysyła komunikaty z dowolnego klienta do wszystkich połączonych klientów.</span><span class="sxs-lookup"><span data-stu-id="4c1ad-110">Add code that sends messages from any client to all connected clients.</span></span>

<span data-ttu-id="4c1ad-111">Po zakończeniu będziesz mieć działającą aplikację rozmowy:</span><span class="sxs-lookup"><span data-stu-id="4c1ad-111">At the end, you'll have a working chat app:</span></span>

![SignalR przykładowej aplikacji](signalr/_static/signalr-get-started-finished.png)

<span data-ttu-id="4c1ad-113">[Wyświetlanie lub pobieranie przykładowego kodu](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/signalr/sample) ([sposobu pobierania](xref:index#how-to-download-a-sample)).</span><span class="sxs-lookup"><span data-stu-id="4c1ad-113">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/signalr/sample) ([how to download](xref:index#how-to-download-a-sample)).</span></span>

> [!NOTE]
> <span data-ttu-id="4c1ad-114">W tej chwili testujemy użyteczność nowo zaproponowanej struktury spisu treści dla dokumentacji platformy ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="4c1ad-114">We’re testing the usability of a proposed new structure for the ASP.NET Core table of contents.</span></span>  <span data-ttu-id="4c1ad-115">Jeśli możesz poświęcić chwilę na wykonanie ćwiczenia polegającego na znalezieniu 7 różnych tematów w aktualnym lub zaproponowanym spisie treści, [kliknij tutaj i weź udział w badaniu](https://dpk4xbh5.optimalworkshop.com/treejack/aa11wn82).</span><span class="sxs-lookup"><span data-stu-id="4c1ad-115">If you have a few minutes to try an exercise of finding 7 different topics in the current or proposed table of contents, please [click here to participate in the study](https://dpk4xbh5.optimalworkshop.com/treejack/aa11wn82).</span></span>


[!INCLUDE [|Prerequisites](~/includes/net-core-prereqs-all-2.2.md)]

## <a name="create-a-web-project"></a><span data-ttu-id="4c1ad-116">Tworzenie projektu sieci web</span><span class="sxs-lookup"><span data-stu-id="4c1ad-116">Create a web project</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="4c1ad-117">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="4c1ad-117">Visual Studio</span></span>](#tab/visual-studio/)

* <span data-ttu-id="4c1ad-118">W menu, wybierz opcję **Plik > Nowy projekt**.</span><span class="sxs-lookup"><span data-stu-id="4c1ad-118">From the menu, select **File > New Project**.</span></span>

* <span data-ttu-id="4c1ad-119">W **nowy projekt** okno dialogowe, wybierz opcję **zainstalowane > Visual C# > sieci Web > Aplikacja sieci Web programu ASP.NET Core**.</span><span class="sxs-lookup"><span data-stu-id="4c1ad-119">In the **New Project** dialog, select **Installed > Visual C# > Web > ASP.NET Core Web Application**.</span></span> <span data-ttu-id="4c1ad-120">Nadaj projektowi nazwę *SignalRChat*.</span><span class="sxs-lookup"><span data-stu-id="4c1ad-120">Name the project *SignalRChat*.</span></span>

  ![Okno dialogowe nowego projektu w programie Visual Studio](signalr/_static/signalr-new-project-dialog.png)

* <span data-ttu-id="4c1ad-122">Wybierz **aplikacji sieci Web** do tworzenia projektu, który używa stron Razor.</span><span class="sxs-lookup"><span data-stu-id="4c1ad-122">Select **Web Application** to create a project that uses Razor Pages.</span></span>

* <span data-ttu-id="4c1ad-123">Wybierz docelową platformę **platformy .NET Core**, wybierz opcję **platformy ASP.NET Core 2.2**i kliknij przycisk **OK**.</span><span class="sxs-lookup"><span data-stu-id="4c1ad-123">Select a target framework of **.NET Core**, select **ASP.NET Core 2.2**, and click **OK**.</span></span>

  ![Okno dialogowe nowego projektu w programie Visual Studio](signalr/_static/signalr-new-project-choose-type.png)

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="4c1ad-125">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="4c1ad-125">Visual Studio Code</span></span>](#tab/visual-studio-code/)

* <span data-ttu-id="4c1ad-126">Otwórz [zintegrowany terminal](https://code.visualstudio.com/docs/editor/integrated-terminal) do folderu, w którym zostanie utworzony nowy folder projektu.</span><span class="sxs-lookup"><span data-stu-id="4c1ad-126">Open the [integrated terminal](https://code.visualstudio.com/docs/editor/integrated-terminal) to the folder in which the new project folder will be created.</span></span>

* <span data-ttu-id="4c1ad-127">Uruchom następujące polecenia:</span><span class="sxs-lookup"><span data-stu-id="4c1ad-127">Run the following commands:</span></span>

   ```console
   dotnet new webapp -o SignalRChat
   code -r SignalRChat
   ```

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="4c1ad-128">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="4c1ad-128">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="4c1ad-129">W menu, wybierz opcję **Plik > nowe rozwiązanie**.</span><span class="sxs-lookup"><span data-stu-id="4c1ad-129">From the menu, select **File > New Solution**.</span></span>

* <span data-ttu-id="4c1ad-130">Wybierz **platformy .NET Core > aplikacji > aplikacji internetowej ASP.NET Core** (nie wybieraj **ASP.NET Core Web App (MVC)**).</span><span class="sxs-lookup"><span data-stu-id="4c1ad-130">Select **.NET Core > App > ASP.NET Core Web App** (Don't select **ASP.NET Core Web App (MVC)**).</span></span>

* <span data-ttu-id="4c1ad-131">Wybierz **dalej**.</span><span class="sxs-lookup"><span data-stu-id="4c1ad-131">Select **Next**.</span></span>

* <span data-ttu-id="4c1ad-132">Nadaj projektowi nazwę *SignalRChat*, a następnie wybierz pozycję **Utwórz**.</span><span class="sxs-lookup"><span data-stu-id="4c1ad-132">Name the project *SignalRChat*, and then select **Create**.</span></span>

---

## <a name="add-the-signalr-client-library"></a><span data-ttu-id="4c1ad-133">Dodaj bibliotekę klienta SignalR</span><span class="sxs-lookup"><span data-stu-id="4c1ad-133">Add the SignalR client library</span></span>

<span data-ttu-id="4c1ad-134">Serwer biblioteki SignalR znajduje się w `Microsoft.AspNetCore.App` meta Microsoft.aspnetcore.all.</span><span class="sxs-lookup"><span data-stu-id="4c1ad-134">The SignalR server library is included in the `Microsoft.AspNetCore.App` metapackage.</span></span> <span data-ttu-id="4c1ad-135">Biblioteki klienta JavaScript automatycznie nie jest zawarty w projekcie.</span><span class="sxs-lookup"><span data-stu-id="4c1ad-135">The JavaScript client library isn't automatically included in the project.</span></span> <span data-ttu-id="4c1ad-136">W tym samouczku używasz Library Manager (LibMan) można pobrać z biblioteki klienta z *unpkg*.</span><span class="sxs-lookup"><span data-stu-id="4c1ad-136">For this tutorial, you use Library Manager (LibMan) to get the client library from *unpkg*.</span></span> <span data-ttu-id="4c1ad-137">unpkg jest usługa content delivery network (CDN)), można dostarczać niczego w Menedżer pakietów npm oraz Menedżera pakietów środowiska Node.js.</span><span class="sxs-lookup"><span data-stu-id="4c1ad-137">unpkg is a content delivery network (CDN)) that can deliver anything found in npm, the Node.js package manager.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="4c1ad-138">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="4c1ad-138">Visual Studio</span></span>](#tab/visual-studio/)

* <span data-ttu-id="4c1ad-139">W **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy projekt i wybierz **Dodaj** > **biblioteki po stronie klienta**.</span><span class="sxs-lookup"><span data-stu-id="4c1ad-139">In **Solution Explorer**, right-click the project, and select **Add** > **Client-Side Library**.</span></span>

* <span data-ttu-id="4c1ad-140">W **Dodaj biblioteki po stronie klienta** okno dialogowe, aby uzyskać **dostawcy** wybierz **unpkg**.</span><span class="sxs-lookup"><span data-stu-id="4c1ad-140">In the **Add Client-Side Library** dialog, for **Provider** select **unpkg**.</span></span> 

* <span data-ttu-id="4c1ad-141">Aby uzyskać **biblioteki**, wprowadź `@aspnet/signalr@1`i wybierz najnowszą wersję, która nie jest w wersji zapoznawczej.</span><span class="sxs-lookup"><span data-stu-id="4c1ad-141">For **Library**, enter `@aspnet/signalr@1`, and select the latest version that isn't preview.</span></span>

  ![Dodaj okno dialogowe biblioteki po stronie klienta — Wybieranie biblioteki](signalr/_static/libman1.png)

* <span data-ttu-id="4c1ad-143">Wybierz **wybierz konkretne pliki**, rozwiń węzeł *dist/przeglądarki* folder, a następnie wybierz *signalr.js* i *signalr.min.js*.</span><span class="sxs-lookup"><span data-stu-id="4c1ad-143">Select **Choose specific files**, expand the *dist/browser* folder, and select *signalr.js* and *signalr.min.js*.</span></span>

* <span data-ttu-id="4c1ad-144">Ustaw **lokalizacji docelowej** do *wwwroot/lib/signalr/* i wybierz **zainstalować**.</span><span class="sxs-lookup"><span data-stu-id="4c1ad-144">Set **Target Location** to *wwwroot/lib/signalr/*, and select **Install**.</span></span>

  ![Dodaj okno dialogowe z biblioteki klienta - wybranie plików i miejsce docelowe](signalr/_static/libman2.png)

  <span data-ttu-id="4c1ad-146">Tworzy LibMan *wwwroot/lib/signalr* folder i kopiuje wybrane pliki.</span><span class="sxs-lookup"><span data-stu-id="4c1ad-146">LibMan creates a *wwwroot/lib/signalr* folder and copies the selected files to it.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="4c1ad-147">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="4c1ad-147">Visual Studio Code</span></span>](#tab/visual-studio-code/)

* <span data-ttu-id="4c1ad-148">W zintegrowanym terminalu uruchom następujące polecenie, aby zainstalować LibMan.</span><span class="sxs-lookup"><span data-stu-id="4c1ad-148">In the integrated terminal, run the following command to install LibMan.</span></span>

  ```console
  dotnet tool install -g Microsoft.Web.LibraryManager.Cli
  ```

* <span data-ttu-id="4c1ad-149">Uruchom następujące polecenie, aby przy użyciu LibMan uzyskać biblioteki klienta SignalR.</span><span class="sxs-lookup"><span data-stu-id="4c1ad-149">Run the following command to get the SignalR client library by using LibMan.</span></span> <span data-ttu-id="4c1ad-150">Być może musisz odczekaj kilka sekund, zanim dane wyjściowe.</span><span class="sxs-lookup"><span data-stu-id="4c1ad-150">You might have to wait a few seconds before seeing output.</span></span>

  ```console
  libman install @aspnet/signalr -p unpkg -d wwwroot/lib/signalr --files dist/browser/signalr.js --files dist/browser/signalr.min.js
  ```

  <span data-ttu-id="4c1ad-151">Parametry określ następujące opcje:</span><span class="sxs-lookup"><span data-stu-id="4c1ad-151">The parameters specify the following options:</span></span>
  * <span data-ttu-id="4c1ad-152">Użyj dostawcy unpkg.</span><span class="sxs-lookup"><span data-stu-id="4c1ad-152">Use the unpkg provider.</span></span>
  * <span data-ttu-id="4c1ad-153">Skopiuj pliki do *wwwroot/lib/signalr* docelowego.</span><span class="sxs-lookup"><span data-stu-id="4c1ad-153">Copy files to the *wwwroot/lib/signalr* destination.</span></span>
  * <span data-ttu-id="4c1ad-154">Skopiuj tylko określonych plików.</span><span class="sxs-lookup"><span data-stu-id="4c1ad-154">Copy only the specified files.</span></span>

  <span data-ttu-id="4c1ad-155">Dane wyjściowe wyglądają jak w poniższym przykładzie:</span><span class="sxs-lookup"><span data-stu-id="4c1ad-155">The output looks like the following example:</span></span>

  ```console
  wwwroot/lib/signalr/dist/browser/signalr.js written to disk
  wwwroot/lib/signalr/dist/browser/signalr.min.js written to disk
  Installed library "@aspnet/signalr@1.0.3" to "wwwroot/lib/signalr"
  ```

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="4c1ad-156">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="4c1ad-156">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="4c1ad-157">W **terminalu**, uruchom następujące polecenie, aby zainstalować LibMan.</span><span class="sxs-lookup"><span data-stu-id="4c1ad-157">In the **Terminal**, run the following command to install LibMan.</span></span>

  ```console
  dotnet tool install -g Microsoft.Web.LibraryManager.Cli
  ```

* <span data-ttu-id="4c1ad-158">Przejdź do folderu projektu (zawierającego *SignalRChat.csproj* pliku).</span><span class="sxs-lookup"><span data-stu-id="4c1ad-158">Navigate to the project folder (the one that contains the *SignalRChat.csproj* file).</span></span>

* <span data-ttu-id="4c1ad-159">Uruchom następujące polecenie, aby przy użyciu LibMan uzyskać biblioteki klienta SignalR.</span><span class="sxs-lookup"><span data-stu-id="4c1ad-159">Run the following command to get the SignalR client library by using LibMan.</span></span>

  ```console
  libman install @aspnet/signalr -p unpkg -d wwwroot/lib/signalr --files dist/browser/signalr.js --files dist/browser/signalr.min.js
  ```

  <span data-ttu-id="4c1ad-160">Parametry określ następujące opcje:</span><span class="sxs-lookup"><span data-stu-id="4c1ad-160">The parameters specify the following options:</span></span>
  * <span data-ttu-id="4c1ad-161">Użyj dostawcy unpkg.</span><span class="sxs-lookup"><span data-stu-id="4c1ad-161">Use the unpkg provider.</span></span>
  * <span data-ttu-id="4c1ad-162">Skopiuj pliki do *wwwroot/lib/signalr* docelowego.</span><span class="sxs-lookup"><span data-stu-id="4c1ad-162">Copy files to the *wwwroot/lib/signalr* destination.</span></span>
  * <span data-ttu-id="4c1ad-163">Skopiuj tylko określonych plików.</span><span class="sxs-lookup"><span data-stu-id="4c1ad-163">Copy only the specified files.</span></span>

  <span data-ttu-id="4c1ad-164">Dane wyjściowe wyglądają jak w poniższym przykładzie:</span><span class="sxs-lookup"><span data-stu-id="4c1ad-164">The output looks like the following example:</span></span>

  ```console
  wwwroot/lib/signalr/dist/browser/signalr.js written to disk
  wwwroot/lib/signalr/dist/browser/signalr.min.js written to disk
  Installed library "@aspnet/signalr@1.0.3" to "wwwroot/lib/signalr"
  ```

---

## <a name="create-a-signalr-hub"></a><span data-ttu-id="4c1ad-165">Tworzenie Centrum SignalR</span><span class="sxs-lookup"><span data-stu-id="4c1ad-165">Create a SignalR hub</span></span>

<span data-ttu-id="4c1ad-166">A *Centrum* jest klasa, która służy jako ogólny potok, który obsługuje komunikację klient serwer.</span><span class="sxs-lookup"><span data-stu-id="4c1ad-166">A *hub* is a class that serves as a high-level pipeline that handles client-server communication.</span></span>

* <span data-ttu-id="4c1ad-167">W folderze projektu SignalRChat Utwórz *koncentratory* folderu.</span><span class="sxs-lookup"><span data-stu-id="4c1ad-167">In the SignalRChat project folder, create a *Hubs* folder.</span></span>

* <span data-ttu-id="4c1ad-168">W *Hubs* folderze utwórz *ChatHub.cs* pliku następującym kodem:</span><span class="sxs-lookup"><span data-stu-id="4c1ad-168">In the *Hubs* folder, create a *ChatHub.cs* file with the following code:</span></span>

  [!code-csharp[Startup](signalr/sample/Hubs/ChatHub.cs)]

  <span data-ttu-id="4c1ad-169">`ChatHub` Klasa dziedziczy z elementu SignalR `Hub` klasy.</span><span class="sxs-lookup"><span data-stu-id="4c1ad-169">The `ChatHub` class inherits from the SignalR `Hub` class.</span></span> <span data-ttu-id="4c1ad-170">`Hub` Klasa zarządza połączeń, grup i komunikatów.</span><span class="sxs-lookup"><span data-stu-id="4c1ad-170">The `Hub` class manages connections, groups, and messaging.</span></span>

  <span data-ttu-id="4c1ad-171">`SendMessage` Metoda może być wywoływana przez wszystkie połączone klienta.</span><span class="sxs-lookup"><span data-stu-id="4c1ad-171">The `SendMessage` method can be called by any connected client.</span></span> <span data-ttu-id="4c1ad-172">Wysyła odebranego komunikatu do wszystkich klientów.</span><span class="sxs-lookup"><span data-stu-id="4c1ad-172">It sends the received message to all clients.</span></span> <span data-ttu-id="4c1ad-173">Kod SignalR jest asynchroniczne w celu zapewnienia maksymalnej skalowalności.</span><span class="sxs-lookup"><span data-stu-id="4c1ad-173">SignalR code is asynchronous to provide maximum scalability.</span></span>

## <a name="configure-signalr"></a><span data-ttu-id="4c1ad-174">Konfigurowanie biblioteki SignalR</span><span class="sxs-lookup"><span data-stu-id="4c1ad-174">Configure SignalR</span></span>

<span data-ttu-id="4c1ad-175">Serwer biblioteki SignalR musi być skonfigurowany do przekazywania żądań SignalR z SignalR.</span><span class="sxs-lookup"><span data-stu-id="4c1ad-175">The SignalR server must be configured to pass SignalR requests to SignalR.</span></span>

* <span data-ttu-id="4c1ad-176">Dodaj następujący wyróżniony kod do *Startup.cs* pliku.</span><span class="sxs-lookup"><span data-stu-id="4c1ad-176">Add the following highlighted code to the *Startup.cs* file.</span></span>

  [!code-csharp[Startup](signalr/sample/Startup.cs?highlight=7,33,52-55)]

  <span data-ttu-id="4c1ad-177">Te zmiany dodanie SignalR systemu iniekcji zależności platformy ASP.NET Core i potoku oprogramowania pośredniczącego.</span><span class="sxs-lookup"><span data-stu-id="4c1ad-177">These changes add SignalR to the ASP.NET Core dependency injection system and the middleware pipeline.</span></span>

## <a name="add-signalr-client-code"></a><span data-ttu-id="4c1ad-178">Dodaj kod klienta SignalR</span><span class="sxs-lookup"><span data-stu-id="4c1ad-178">Add SignalR client code</span></span>

* <span data-ttu-id="4c1ad-179">Zastąp zawartość *Pages\Index.cshtml* następującym kodem:</span><span class="sxs-lookup"><span data-stu-id="4c1ad-179">Replace the content in *Pages\Index.cshtml* with the following code:</span></span>

  [!code-cshtml[Index](signalr/sample/Pages/Index.cshtml)]

  <span data-ttu-id="4c1ad-180">Powyższy kod:</span><span class="sxs-lookup"><span data-stu-id="4c1ad-180">The preceding code:</span></span>

  * <span data-ttu-id="4c1ad-181">Tworzy pola tekstowe nazwy i tekst wiadomość i przycisk Prześlij.</span><span class="sxs-lookup"><span data-stu-id="4c1ad-181">Creates text boxes for name and message text, and a submit button.</span></span>
  * <span data-ttu-id="4c1ad-182">Tworzy listę z `id="messagesList"` do wyświetlania komunikatów odebranych z Centrum SignalR.</span><span class="sxs-lookup"><span data-stu-id="4c1ad-182">Creates a list with `id="messagesList"` for displaying messages that are received from the SignalR hub.</span></span>
  * <span data-ttu-id="4c1ad-183">Zawiera odwołania do skryptu z SignalR i *chat.js* kod aplikacji, który zostanie utworzony w następnym kroku.</span><span class="sxs-lookup"><span data-stu-id="4c1ad-183">Includes script references to SignalR and the *chat.js* application code that you create in the next step.</span></span>

* <span data-ttu-id="4c1ad-184">W *wwwroot/js* folderze utwórz *chat.js* pliku następującym kodem:</span><span class="sxs-lookup"><span data-stu-id="4c1ad-184">In the *wwwroot/js* folder, create a *chat.js* file with the following code:</span></span>

  [!code-javascript[Index](signalr/sample/wwwroot/js/chat.js)]

  <span data-ttu-id="4c1ad-185">Powyższy kod:</span><span class="sxs-lookup"><span data-stu-id="4c1ad-185">The preceding code:</span></span>

  * <span data-ttu-id="4c1ad-186">Tworzy i uruchamia połączenie.</span><span class="sxs-lookup"><span data-stu-id="4c1ad-186">Creates and starts a connection.</span></span>
  * <span data-ttu-id="4c1ad-187">Dodaje przycisk przesyłania program obsługi, która wysyła komunikaty do Centrum.</span><span class="sxs-lookup"><span data-stu-id="4c1ad-187">Adds to the submit button a handler that sends messages to the hub.</span></span>
  * <span data-ttu-id="4c1ad-188">Dodaje do obiektu połączenia program obsługi, który odbiera komunikaty z Centrum i dodaje je do listy.</span><span class="sxs-lookup"><span data-stu-id="4c1ad-188">Adds to the connection object a handler that receives messages from the hub and adds them to the list.</span></span>

## <a name="run-the-app"></a><span data-ttu-id="4c1ad-189">Uruchamianie aplikacji</span><span class="sxs-lookup"><span data-stu-id="4c1ad-189">Run the app</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="4c1ad-190">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="4c1ad-190">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="4c1ad-191">Naciśnij klawisz **kombinację klawiszy CTRL + F5** Aby uruchomić aplikację bez debugowania.</span><span class="sxs-lookup"><span data-stu-id="4c1ad-191">Press **CTRL+F5** to run the app without debugging.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="4c1ad-192">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="4c1ad-192">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="4c1ad-193">W zintegrowanym terminalu uruchom następujące polecenie:</span><span class="sxs-lookup"><span data-stu-id="4c1ad-193">In the integrated terminal, run the following command:</span></span>

  ```console
  dotnet run -p SignalRChat.csproj
  ```
  
# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="4c1ad-194">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="4c1ad-194">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="4c1ad-195">W menu, wybierz opcję **Uruchom > Uruchom bez debugowania**.</span><span class="sxs-lookup"><span data-stu-id="4c1ad-195">From the menu, select **Run > Start Without Debugging**.</span></span>

---

* <span data-ttu-id="4c1ad-196">Skopiuj adres URL z paska adresu, a następnie otwórz innego wystąpienia przeglądarki lub karty i wklej adres URL w pasku adresu.</span><span class="sxs-lookup"><span data-stu-id="4c1ad-196">Copy the URL from the address bar, open another browser instance or tab, and paste the URL in the address bar.</span></span>

* <span data-ttu-id="4c1ad-197">Wybierz albo przeglądarki, wprowadź nazwę i komunikat, a następnie wybierz **wysyłania komunikatu** przycisku.</span><span class="sxs-lookup"><span data-stu-id="4c1ad-197">Choose either browser, enter a name and message, and select the **Send Message** button.</span></span>

  <span data-ttu-id="4c1ad-198">Nazwa i wiadomości są wyświetlane na obu stronach natychmiast.</span><span class="sxs-lookup"><span data-stu-id="4c1ad-198">The name and message are displayed on both pages instantly.</span></span>

  ![SignalR przykładowej aplikacji](signalr/_static/signalr-get-started-finished.png)

> [!TIP]
> <span data-ttu-id="4c1ad-200">Jeśli aplikacja nie działa, Otwórz swoje narzędzia deweloperskie przeglądarki (F12) i przejdź do konsoli.</span><span class="sxs-lookup"><span data-stu-id="4c1ad-200">If the app doesn't work, open your browser developer tools (F12) and go to the console.</span></span> <span data-ttu-id="4c1ad-201">Może pojawić się błędy związane z Twoim kodem HTML i JavaScript.</span><span class="sxs-lookup"><span data-stu-id="4c1ad-201">You might see errors related to your HTML and JavaScript code.</span></span> <span data-ttu-id="4c1ad-202">Załóżmy, że możesz umieścić *signalr.js* w innym folderze niż bezpośredniego.</span><span class="sxs-lookup"><span data-stu-id="4c1ad-202">For example, suppose you put *signalr.js* in a different folder than directed.</span></span> <span data-ttu-id="4c1ad-203">W takim przypadku odwołania do tego pliku nie będzie działać, a następnie zostanie wyświetlony błąd 404 w konsoli.</span><span class="sxs-lookup"><span data-stu-id="4c1ad-203">In that case the reference to that file won't work and you'll see a 404 error in the console.</span></span>
> <span data-ttu-id="4c1ad-204">![błąd — nie znaleziono signalr.js](signalr/_static/f12-console.png)</span><span class="sxs-lookup"><span data-stu-id="4c1ad-204">![signalr.js not found error](signalr/_static/f12-console.png)</span></span>

## <a name="next-steps"></a><span data-ttu-id="4c1ad-205">Następne kroki</span><span class="sxs-lookup"><span data-stu-id="4c1ad-205">Next steps</span></span>

<span data-ttu-id="4c1ad-206">W tym samouczku przedstawiono sposób:</span><span class="sxs-lookup"><span data-stu-id="4c1ad-206">In this tutorial, you learned how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="4c1ad-207">Tworzenie projektu aplikacji sieci web.</span><span class="sxs-lookup"><span data-stu-id="4c1ad-207">Create a web app project.</span></span>
> * <span data-ttu-id="4c1ad-208">Dodaj bibliotekę klienta SignalR.</span><span class="sxs-lookup"><span data-stu-id="4c1ad-208">Add the SignalR client library.</span></span>
> * <span data-ttu-id="4c1ad-209">Utwórz Centrum SignalR.</span><span class="sxs-lookup"><span data-stu-id="4c1ad-209">Create a SignalR hub.</span></span>
> * <span data-ttu-id="4c1ad-210">Skonfiguruj projekt do korzystania z SignalR.</span><span class="sxs-lookup"><span data-stu-id="4c1ad-210">Configure the project to use SignalR.</span></span>
> * <span data-ttu-id="4c1ad-211">Dodaj kod używający koncentratora, aby wysyłać komunikaty z dowolnego klienta do wszystkich połączonych klientów.</span><span class="sxs-lookup"><span data-stu-id="4c1ad-211">Add code that uses the hub to send messages from any client to all connected clients.</span></span>

<span data-ttu-id="4c1ad-212">Aby dowiedzieć się więcej na temat biblioteki SignalR, zobacz wprowadzenie:</span><span class="sxs-lookup"><span data-stu-id="4c1ad-212">To learn more about SignalR, see the introduction:</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="4c1ad-213">Wprowadzenie do SignalR platformy ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="4c1ad-213">Introduction to ASP.NET Core SignalR</span></span>](xref:signalr/introduction)
