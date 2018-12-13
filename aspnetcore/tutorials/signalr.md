---
title: Wprowadzenie do biblioteki SignalR platformy ASP.NET Core
author: tdykstra
description: W tym samouczku utworzysz aplikację rozmowy, która korzysta z biblioteki SignalR platformy ASP.NET Core.
ms.author: tdykstra
ms.custom: mvc
ms.date: 11/30/2018
uid: tutorials/signalr
ms.openlocfilehash: 904031c58b06f12d41902802f8ab3927b29ae94b
ms.sourcegitcommit: a16352c1c88a71770ab3922200a8cd148fb278a6
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 12/13/2018
ms.locfileid: "53335352"
---
# <a name="tutorial-get-started-with-aspnet-core-signalr"></a><span data-ttu-id="1503c-103">Samouczek: Wprowadzenie do biblioteki SignalR platformy ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="1503c-103">Tutorial: Get started with ASP.NET Core SignalR</span></span>

<span data-ttu-id="1503c-104">W tym samouczku pokazano podstawowe informacje dotyczące tworzenia aplikacji w czasie rzeczywistym przy użyciu biblioteki SignalR.</span><span class="sxs-lookup"><span data-stu-id="1503c-104">This tutorial teaches the basics of building a real-time app using SignalR.</span></span> <span data-ttu-id="1503c-105">Dowiesz się, jak:</span><span class="sxs-lookup"><span data-stu-id="1503c-105">You learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="1503c-106">Utwórz projekt sieci web.</span><span class="sxs-lookup"><span data-stu-id="1503c-106">Create a web project.</span></span>
> * <span data-ttu-id="1503c-107">Dodaj bibliotekę klienta SignalR.</span><span class="sxs-lookup"><span data-stu-id="1503c-107">Add the SignalR client library.</span></span>
> * <span data-ttu-id="1503c-108">Utwórz Centrum SignalR.</span><span class="sxs-lookup"><span data-stu-id="1503c-108">Create a SignalR hub.</span></span>
> * <span data-ttu-id="1503c-109">Skonfiguruj projekt do korzystania z SignalR.</span><span class="sxs-lookup"><span data-stu-id="1503c-109">Configure the project to use SignalR.</span></span>
> * <span data-ttu-id="1503c-110">Dodaj kod, który wysyła komunikaty z dowolnego klienta do wszystkich połączonych klientów.</span><span class="sxs-lookup"><span data-stu-id="1503c-110">Add code that sends messages from any client to all connected clients.</span></span>

<span data-ttu-id="1503c-111">Po zakończeniu będziesz mieć działającą aplikację rozmowy:</span><span class="sxs-lookup"><span data-stu-id="1503c-111">At the end, you'll have a working chat app:</span></span>

![SignalR przykładowej aplikacji](signalr/_static/signalr-get-started-finished.png)

<span data-ttu-id="1503c-113">[Wyświetlanie lub pobieranie przykładowego kodu](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/signalr/sample) ([sposobu pobierania](xref:index#how-to-download-a-sample)).</span><span class="sxs-lookup"><span data-stu-id="1503c-113">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/signalr/sample) ([how to download](xref:index#how-to-download-a-sample)).</span></span>

[!INCLUDE [|Prerequisites](~/includes/net-core-prereqs-all-2.2.md)]

## <a name="create-a-web-project"></a><span data-ttu-id="1503c-114">Tworzenie projektu sieci web</span><span class="sxs-lookup"><span data-stu-id="1503c-114">Create a web project</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="1503c-115">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="1503c-115">Visual Studio</span></span>](#tab/visual-studio/)

* <span data-ttu-id="1503c-116">W menu, wybierz opcję **Plik > Nowy projekt**.</span><span class="sxs-lookup"><span data-stu-id="1503c-116">From the menu, select **File > New Project**.</span></span>

* <span data-ttu-id="1503c-117">W **nowy projekt** okno dialogowe, wybierz opcję **zainstalowane > Visual C# > sieci Web > Aplikacja sieci Web programu ASP.NET Core**.</span><span class="sxs-lookup"><span data-stu-id="1503c-117">In the **New Project** dialog, select **Installed > Visual C# > Web > ASP.NET Core Web Application**.</span></span> <span data-ttu-id="1503c-118">Nadaj projektowi nazwę *SignalRChat*.</span><span class="sxs-lookup"><span data-stu-id="1503c-118">Name the project *SignalRChat*.</span></span>

  ![Okno dialogowe nowego projektu w programie Visual Studio](signalr/_static/signalr-new-project-dialog.png)

* <span data-ttu-id="1503c-120">Wybierz **aplikacji sieci Web** do tworzenia projektu, który używa stron Razor.</span><span class="sxs-lookup"><span data-stu-id="1503c-120">Select **Web Application** to create a project that uses Razor Pages.</span></span>

* <span data-ttu-id="1503c-121">Wybierz docelową platformę **platformy .NET Core**, wybierz opcję **platformy ASP.NET Core 2.2**i kliknij przycisk **OK**.</span><span class="sxs-lookup"><span data-stu-id="1503c-121">Select a target framework of **.NET Core**, select **ASP.NET Core 2.2**, and click **OK**.</span></span>

  ![Okno dialogowe nowego projektu w programie Visual Studio](signalr/_static/signalr-new-project-choose-type.png)

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="1503c-123">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="1503c-123">Visual Studio Code</span></span>](#tab/visual-studio-code/)

* <span data-ttu-id="1503c-124">Otwórz [zintegrowany terminal](https://code.visualstudio.com/docs/editor/integrated-terminal) do folderu, w którym zostanie utworzony nowy folder projektu.</span><span class="sxs-lookup"><span data-stu-id="1503c-124">Open the [integrated terminal](https://code.visualstudio.com/docs/editor/integrated-terminal) to the folder in which the new project folder will be created.</span></span>

* <span data-ttu-id="1503c-125">Uruchom następujące polecenia:</span><span class="sxs-lookup"><span data-stu-id="1503c-125">Run the following commands:</span></span>

   ```console
   dotnet new webapp -o SignalRChat
   code -r SignalRChat
   ```

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="1503c-126">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="1503c-126">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="1503c-127">W menu, wybierz opcję **Plik > nowe rozwiązanie**.</span><span class="sxs-lookup"><span data-stu-id="1503c-127">From the menu, select **File > New Solution**.</span></span>

* <span data-ttu-id="1503c-128">Wybierz **platformy .NET Core > aplikacji > aplikacji internetowej ASP.NET Core** (nie wybieraj **ASP.NET Core Web App (MVC)**).</span><span class="sxs-lookup"><span data-stu-id="1503c-128">Select **.NET Core > App > ASP.NET Core Web App** (Don't select **ASP.NET Core Web App (MVC)**).</span></span>

* <span data-ttu-id="1503c-129">Wybierz opcję **Dalej**.</span><span class="sxs-lookup"><span data-stu-id="1503c-129">Select **Next**.</span></span>

* <span data-ttu-id="1503c-130">Nadaj projektowi nazwę *SignalRChat*, a następnie wybierz pozycję **Utwórz**.</span><span class="sxs-lookup"><span data-stu-id="1503c-130">Name the project *SignalRChat*, and then select **Create**.</span></span>

---

## <a name="add-the-signalr-client-library"></a><span data-ttu-id="1503c-131">Dodaj bibliotekę klienta SignalR</span><span class="sxs-lookup"><span data-stu-id="1503c-131">Add the SignalR client library</span></span>

<span data-ttu-id="1503c-132">Serwer biblioteki SignalR znajduje się w `Microsoft.AspNetCore.App` meta Microsoft.aspnetcore.all.</span><span class="sxs-lookup"><span data-stu-id="1503c-132">The SignalR server library is included in the `Microsoft.AspNetCore.App` metapackage.</span></span> <span data-ttu-id="1503c-133">Biblioteki klienta JavaScript automatycznie nie jest zawarty w projekcie.</span><span class="sxs-lookup"><span data-stu-id="1503c-133">The JavaScript client library isn't automatically included in the project.</span></span> <span data-ttu-id="1503c-134">W tym samouczku używasz Library Manager (LibMan) można pobrać z biblioteki klienta z *unpkg*.</span><span class="sxs-lookup"><span data-stu-id="1503c-134">For this tutorial, you use Library Manager (LibMan) to get the client library from *unpkg*.</span></span> <span data-ttu-id="1503c-135">unpkg jest usługa content delivery network (CDN)), można dostarczać niczego w Menedżer pakietów npm oraz Menedżera pakietów środowiska Node.js.</span><span class="sxs-lookup"><span data-stu-id="1503c-135">unpkg is a content delivery network (CDN)) that can deliver anything found in npm, the Node.js package manager.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="1503c-136">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="1503c-136">Visual Studio</span></span>](#tab/visual-studio/)

* <span data-ttu-id="1503c-137">W **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy projekt i wybierz **Dodaj** > **biblioteki po stronie klienta**.</span><span class="sxs-lookup"><span data-stu-id="1503c-137">In **Solution Explorer**, right-click the project, and select **Add** > **Client-Side Library**.</span></span>

* <span data-ttu-id="1503c-138">W **Dodaj biblioteki po stronie klienta** okno dialogowe, aby uzyskać **dostawcy** wybierz **unpkg**.</span><span class="sxs-lookup"><span data-stu-id="1503c-138">In the **Add Client-Side Library** dialog, for **Provider** select **unpkg**.</span></span> 

* <span data-ttu-id="1503c-139">Aby uzyskać **biblioteki**, wprowadź `@aspnet/signalr@1`i wybierz najnowszą wersję, która nie jest w wersji zapoznawczej.</span><span class="sxs-lookup"><span data-stu-id="1503c-139">For **Library**, enter `@aspnet/signalr@1`, and select the latest version that isn't preview.</span></span>

  ![Dodaj okno dialogowe biblioteki po stronie klienta — Wybieranie biblioteki](signalr/_static/libman1.png)

* <span data-ttu-id="1503c-141">Wybierz **wybierz konkretne pliki**, rozwiń węzeł *dist/przeglądarki* folder, a następnie wybierz *signalr.js* i *signalr.min.js*.</span><span class="sxs-lookup"><span data-stu-id="1503c-141">Select **Choose specific files**, expand the *dist/browser* folder, and select *signalr.js* and *signalr.min.js*.</span></span>

* <span data-ttu-id="1503c-142">Ustaw **lokalizacji docelowej** do *wwwroot/lib/signalr/* i wybierz **zainstalować**.</span><span class="sxs-lookup"><span data-stu-id="1503c-142">Set **Target Location** to *wwwroot/lib/signalr/*, and select **Install**.</span></span>

  ![Dodaj okno dialogowe z biblioteki klienta - wybranie plików i miejsce docelowe](signalr/_static/libman2.png)

  <span data-ttu-id="1503c-144">Tworzy LibMan *wwwroot/lib/signalr* folder i kopiuje wybrane pliki.</span><span class="sxs-lookup"><span data-stu-id="1503c-144">LibMan creates a *wwwroot/lib/signalr* folder and copies the selected files to it.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="1503c-145">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="1503c-145">Visual Studio Code</span></span>](#tab/visual-studio-code/)

* <span data-ttu-id="1503c-146">W zintegrowanym terminalu uruchom następujące polecenie, aby zainstalować LibMan.</span><span class="sxs-lookup"><span data-stu-id="1503c-146">In the integrated terminal, run the following command to install LibMan.</span></span>

  ```console
  dotnet tool install -g Microsoft.Web.LibraryManager.Cli
  ```

* <span data-ttu-id="1503c-147">Uruchom następujące polecenie, aby przy użyciu LibMan uzyskać biblioteki klienta SignalR.</span><span class="sxs-lookup"><span data-stu-id="1503c-147">Run the following command to get the SignalR client library by using LibMan.</span></span> <span data-ttu-id="1503c-148">Być może musisz odczekaj kilka sekund, zanim dane wyjściowe.</span><span class="sxs-lookup"><span data-stu-id="1503c-148">You might have to wait a few seconds before seeing output.</span></span>

  ```console
  libman install @aspnet/signalr -p unpkg -d wwwroot/lib/signalr --files dist/browser/signalr.js --files dist/browser/signalr.min.js
  ```

  <span data-ttu-id="1503c-149">Parametry określ następujące opcje:</span><span class="sxs-lookup"><span data-stu-id="1503c-149">The parameters specify the following options:</span></span>
  * <span data-ttu-id="1503c-150">Użyj dostawcy unpkg.</span><span class="sxs-lookup"><span data-stu-id="1503c-150">Use the unpkg provider.</span></span>
  * <span data-ttu-id="1503c-151">Skopiuj pliki do *wwwroot/lib/signalr* docelowego.</span><span class="sxs-lookup"><span data-stu-id="1503c-151">Copy files to the *wwwroot/lib/signalr* destination.</span></span>
  * <span data-ttu-id="1503c-152">Skopiuj tylko określonych plików.</span><span class="sxs-lookup"><span data-stu-id="1503c-152">Copy only the specified files.</span></span>

  <span data-ttu-id="1503c-153">Dane wyjściowe wyglądają jak w poniższym przykładzie:</span><span class="sxs-lookup"><span data-stu-id="1503c-153">The output looks like the following example:</span></span>

  ```console
  wwwroot/lib/signalr/dist/browser/signalr.js written to disk
  wwwroot/lib/signalr/dist/browser/signalr.min.js written to disk
  Installed library "@aspnet/signalr@1.0.3" to "wwwroot/lib/signalr"
  ```

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="1503c-154">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="1503c-154">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="1503c-155">W **terminalu**, uruchom następujące polecenie, aby zainstalować LibMan.</span><span class="sxs-lookup"><span data-stu-id="1503c-155">In the **Terminal**, run the following command to install LibMan.</span></span>

  ```console
  dotnet tool install -g Microsoft.Web.LibraryManager.Cli
  ```

* <span data-ttu-id="1503c-156">Przejdź do folderu projektu (zawierającego *SignalRChat.csproj* pliku).</span><span class="sxs-lookup"><span data-stu-id="1503c-156">Navigate to the project folder (the one that contains the *SignalRChat.csproj* file).</span></span>

* <span data-ttu-id="1503c-157">Uruchom następujące polecenie, aby przy użyciu LibMan uzyskać biblioteki klienta SignalR.</span><span class="sxs-lookup"><span data-stu-id="1503c-157">Run the following command to get the SignalR client library by using LibMan.</span></span>

  ```console
  libman install @aspnet/signalr -p unpkg -d wwwroot/lib/signalr --files dist/browser/signalr.js --files dist/browser/signalr.min.js
  ```

  <span data-ttu-id="1503c-158">Parametry określ następujące opcje:</span><span class="sxs-lookup"><span data-stu-id="1503c-158">The parameters specify the following options:</span></span>
  * <span data-ttu-id="1503c-159">Użyj dostawcy unpkg.</span><span class="sxs-lookup"><span data-stu-id="1503c-159">Use the unpkg provider.</span></span>
  * <span data-ttu-id="1503c-160">Skopiuj pliki do *wwwroot/lib/signalr* docelowego.</span><span class="sxs-lookup"><span data-stu-id="1503c-160">Copy files to the *wwwroot/lib/signalr* destination.</span></span>
  * <span data-ttu-id="1503c-161">Skopiuj tylko określonych plików.</span><span class="sxs-lookup"><span data-stu-id="1503c-161">Copy only the specified files.</span></span>

  <span data-ttu-id="1503c-162">Dane wyjściowe wyglądają jak w poniższym przykładzie:</span><span class="sxs-lookup"><span data-stu-id="1503c-162">The output looks like the following example:</span></span>

  ```console
  wwwroot/lib/signalr/dist/browser/signalr.js written to disk
  wwwroot/lib/signalr/dist/browser/signalr.min.js written to disk
  Installed library "@aspnet/signalr@1.0.3" to "wwwroot/lib/signalr"
  ```

---

## <a name="create-a-signalr-hub"></a><span data-ttu-id="1503c-163">Tworzenie Centrum SignalR</span><span class="sxs-lookup"><span data-stu-id="1503c-163">Create a SignalR hub</span></span>

<span data-ttu-id="1503c-164">A *Centrum* jest klasa, która służy jako ogólny potok, który obsługuje komunikację klient serwer.</span><span class="sxs-lookup"><span data-stu-id="1503c-164">A *hub* is a class that serves as a high-level pipeline that handles client-server communication.</span></span>

* <span data-ttu-id="1503c-165">W folderze projektu SignalRChat Utwórz *koncentratory* folderu.</span><span class="sxs-lookup"><span data-stu-id="1503c-165">In the SignalRChat project folder, create a *Hubs* folder.</span></span>

* <span data-ttu-id="1503c-166">W *Hubs* folderze utwórz *ChatHub.cs* pliku następującym kodem:</span><span class="sxs-lookup"><span data-stu-id="1503c-166">In the *Hubs* folder, create a *ChatHub.cs* file with the following code:</span></span>

  [!code-csharp[Startup](signalr/sample/Hubs/ChatHub.cs)]

  <span data-ttu-id="1503c-167">`ChatHub` Klasa dziedziczy z elementu SignalR `Hub` klasy.</span><span class="sxs-lookup"><span data-stu-id="1503c-167">The `ChatHub` class inherits from the SignalR `Hub` class.</span></span> <span data-ttu-id="1503c-168">`Hub` Klasa zarządza połączeń, grup i komunikatów.</span><span class="sxs-lookup"><span data-stu-id="1503c-168">The `Hub` class manages connections, groups, and messaging.</span></span>

  <span data-ttu-id="1503c-169">`SendMessage` Metoda może być wywoływana przez wszystkie połączone klienta.</span><span class="sxs-lookup"><span data-stu-id="1503c-169">The `SendMessage` method can be called by any connected client.</span></span> <span data-ttu-id="1503c-170">Wysyła odebranego komunikatu do wszystkich klientów.</span><span class="sxs-lookup"><span data-stu-id="1503c-170">It sends the received message to all clients.</span></span> <span data-ttu-id="1503c-171">Kod SignalR jest asynchroniczne w celu zapewnienia maksymalnej skalowalności.</span><span class="sxs-lookup"><span data-stu-id="1503c-171">SignalR code is asynchronous to provide maximum scalability.</span></span>

## <a name="configure-signalr"></a><span data-ttu-id="1503c-172">Konfigurowanie biblioteki SignalR</span><span class="sxs-lookup"><span data-stu-id="1503c-172">Configure SignalR</span></span>

<span data-ttu-id="1503c-173">Serwer biblioteki SignalR musi być skonfigurowany do przekazywania żądań SignalR z SignalR.</span><span class="sxs-lookup"><span data-stu-id="1503c-173">The SignalR server must be configured to pass SignalR requests to SignalR.</span></span>

* <span data-ttu-id="1503c-174">Dodaj następujący wyróżniony kod do *Startup.cs* pliku.</span><span class="sxs-lookup"><span data-stu-id="1503c-174">Add the following highlighted code to the *Startup.cs* file.</span></span>

  [!code-csharp[Startup](signalr/sample/Startup.cs?highlight=7,33,52-55)]

  <span data-ttu-id="1503c-175">Te zmiany dodanie SignalR systemu iniekcji zależności platformy ASP.NET Core i potoku oprogramowania pośredniczącego.</span><span class="sxs-lookup"><span data-stu-id="1503c-175">These changes add SignalR to the ASP.NET Core dependency injection system and the middleware pipeline.</span></span>

## <a name="add-signalr-client-code"></a><span data-ttu-id="1503c-176">Dodaj kod klienta SignalR</span><span class="sxs-lookup"><span data-stu-id="1503c-176">Add SignalR client code</span></span>

* <span data-ttu-id="1503c-177">Zastąp zawartość *Pages\Index.cshtml* następującym kodem:</span><span class="sxs-lookup"><span data-stu-id="1503c-177">Replace the content in *Pages\Index.cshtml* with the following code:</span></span>

  [!code-cshtml[Index](signalr/sample/Pages/Index.cshtml)]

  <span data-ttu-id="1503c-178">Powyższy kod:</span><span class="sxs-lookup"><span data-stu-id="1503c-178">The preceding code:</span></span>

  * <span data-ttu-id="1503c-179">Tworzy pola tekstowe nazwy i tekst wiadomość i przycisk Prześlij.</span><span class="sxs-lookup"><span data-stu-id="1503c-179">Creates text boxes for name and message text, and a submit button.</span></span>
  * <span data-ttu-id="1503c-180">Tworzy listę z `id="messagesList"` do wyświetlania komunikatów odebranych z Centrum SignalR.</span><span class="sxs-lookup"><span data-stu-id="1503c-180">Creates a list with `id="messagesList"` for displaying messages that are received from the SignalR hub.</span></span>
  * <span data-ttu-id="1503c-181">Zawiera odwołania do skryptu z SignalR i *chat.js* kod aplikacji, który zostanie utworzony w następnym kroku.</span><span class="sxs-lookup"><span data-stu-id="1503c-181">Includes script references to SignalR and the *chat.js* application code that you create in the next step.</span></span>

* <span data-ttu-id="1503c-182">W *wwwroot/js* folderze utwórz *chat.js* pliku następującym kodem:</span><span class="sxs-lookup"><span data-stu-id="1503c-182">In the *wwwroot/js* folder, create a *chat.js* file with the following code:</span></span>

  [!code-javascript[Index](signalr/sample/wwwroot/js/chat.js)]

  <span data-ttu-id="1503c-183">Powyższy kod:</span><span class="sxs-lookup"><span data-stu-id="1503c-183">The preceding code:</span></span>

  * <span data-ttu-id="1503c-184">Tworzy i uruchamia połączenie.</span><span class="sxs-lookup"><span data-stu-id="1503c-184">Creates and starts a connection.</span></span>
  * <span data-ttu-id="1503c-185">Dodaje przycisk przesyłania program obsługi, która wysyła komunikaty do Centrum.</span><span class="sxs-lookup"><span data-stu-id="1503c-185">Adds to the submit button a handler that sends messages to the hub.</span></span>
  * <span data-ttu-id="1503c-186">Dodaje do obiektu połączenia program obsługi, który odbiera komunikaty z Centrum i dodaje je do listy.</span><span class="sxs-lookup"><span data-stu-id="1503c-186">Adds to the connection object a handler that receives messages from the hub and adds them to the list.</span></span>

## <a name="run-the-app"></a><span data-ttu-id="1503c-187">Uruchamianie aplikacji</span><span class="sxs-lookup"><span data-stu-id="1503c-187">Run the app</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="1503c-188">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="1503c-188">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="1503c-189">Naciśnij klawisz **kombinację klawiszy CTRL + F5** Aby uruchomić aplikację bez debugowania.</span><span class="sxs-lookup"><span data-stu-id="1503c-189">Press **CTRL+F5** to run the app without debugging.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="1503c-190">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="1503c-190">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="1503c-191">W zintegrowanym terminalu uruchom następujące polecenie:</span><span class="sxs-lookup"><span data-stu-id="1503c-191">In the integrated terminal, run the following command:</span></span>

  ```console
  dotnet run -p SignalRChat.csproj
  ```
  
# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="1503c-192">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="1503c-192">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="1503c-193">W menu, wybierz opcję **Uruchom > Uruchom bez debugowania**.</span><span class="sxs-lookup"><span data-stu-id="1503c-193">From the menu, select **Run > Start Without Debugging**.</span></span>

---

* <span data-ttu-id="1503c-194">Skopiuj adres URL z paska adresu, a następnie otwórz innego wystąpienia przeglądarki lub karty i wklej adres URL w pasku adresu.</span><span class="sxs-lookup"><span data-stu-id="1503c-194">Copy the URL from the address bar, open another browser instance or tab, and paste the URL in the address bar.</span></span>

* <span data-ttu-id="1503c-195">Wybierz albo przeglądarki, wprowadź nazwę i komunikat, a następnie wybierz **wysyłania komunikatu** przycisku.</span><span class="sxs-lookup"><span data-stu-id="1503c-195">Choose either browser, enter a name and message, and select the **Send Message** button.</span></span>

  <span data-ttu-id="1503c-196">Nazwa i wiadomości są wyświetlane na obu stronach natychmiast.</span><span class="sxs-lookup"><span data-stu-id="1503c-196">The name and message are displayed on both pages instantly.</span></span>

  ![SignalR przykładowej aplikacji](signalr/_static/signalr-get-started-finished.png)

> [!TIP]
> <span data-ttu-id="1503c-198">Jeśli aplikacja nie działa, Otwórz swoje narzędzia deweloperskie przeglądarki (F12) i przejdź do konsoli.</span><span class="sxs-lookup"><span data-stu-id="1503c-198">If the app doesn't work, open your browser developer tools (F12) and go to the console.</span></span> <span data-ttu-id="1503c-199">Może pojawić się błędy związane z Twoim kodem HTML i JavaScript.</span><span class="sxs-lookup"><span data-stu-id="1503c-199">You might see errors related to your HTML and JavaScript code.</span></span> <span data-ttu-id="1503c-200">Załóżmy, że możesz umieścić *signalr.js* w innym folderze niż bezpośredniego.</span><span class="sxs-lookup"><span data-stu-id="1503c-200">For example, suppose you put *signalr.js* in a different folder than directed.</span></span> <span data-ttu-id="1503c-201">W takim przypadku odwołania do tego pliku nie będzie działać, a następnie zostanie wyświetlony błąd 404 w konsoli.</span><span class="sxs-lookup"><span data-stu-id="1503c-201">In that case the reference to that file won't work and you'll see a 404 error in the console.</span></span>
> <span data-ttu-id="1503c-202">![błąd — nie znaleziono signalr.js](signalr/_static/f12-console.png)</span><span class="sxs-lookup"><span data-stu-id="1503c-202">![signalr.js not found error](signalr/_static/f12-console.png)</span></span>

## <a name="next-steps"></a><span data-ttu-id="1503c-203">Następne kroki</span><span class="sxs-lookup"><span data-stu-id="1503c-203">Next steps</span></span>

<span data-ttu-id="1503c-204">W niniejszym samouczku zawarto informacje na temat wykonywania następujących czynności:</span><span class="sxs-lookup"><span data-stu-id="1503c-204">In this tutorial, you learned how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="1503c-205">Tworzenie projektu aplikacji sieci web.</span><span class="sxs-lookup"><span data-stu-id="1503c-205">Create a web app project.</span></span>
> * <span data-ttu-id="1503c-206">Dodaj bibliotekę klienta SignalR.</span><span class="sxs-lookup"><span data-stu-id="1503c-206">Add the SignalR client library.</span></span>
> * <span data-ttu-id="1503c-207">Utwórz Centrum SignalR.</span><span class="sxs-lookup"><span data-stu-id="1503c-207">Create a SignalR hub.</span></span>
> * <span data-ttu-id="1503c-208">Skonfiguruj projekt do korzystania z SignalR.</span><span class="sxs-lookup"><span data-stu-id="1503c-208">Configure the project to use SignalR.</span></span>
> * <span data-ttu-id="1503c-209">Dodaj kod używający koncentratora, aby wysyłać komunikaty z dowolnego klienta do wszystkich połączonych klientów.</span><span class="sxs-lookup"><span data-stu-id="1503c-209">Add code that uses the hub to send messages from any client to all connected clients.</span></span>

<span data-ttu-id="1503c-210">Aby dowiedzieć się więcej na temat biblioteki SignalR, zobacz wprowadzenie:</span><span class="sxs-lookup"><span data-stu-id="1503c-210">To learn more about SignalR, see the introduction:</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="1503c-211">Wprowadzenie do SignalR platformy ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="1503c-211">Introduction to ASP.NET Core SignalR</span></span>](xref:signalr/introduction)
