---
title: Wprowadzenie do biblioteki SignalR platformy ASP.NET Core
author: bradygaster
description: W tym samouczku utworzysz aplikację rozmowy, która korzysta z biblioteki SignalR platformy ASP.NET Core.
ms.author: bradyg
ms.custom: mvc
ms.date: 07/08/2019
uid: tutorials/signalr
ms.openlocfilehash: fd3324dfa0731ae16747178d83bd93ed95dd15ce
ms.sourcegitcommit: bee530454ae2b3c25dc7ffebf93536f479a14460
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/10/2019
ms.locfileid: "67724475"
---
# <a name="tutorial-get-started-with-aspnet-core-signalr"></a><span data-ttu-id="0fef3-103">Samouczek: Wprowadzenie do biblioteki SignalR platformy ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="0fef3-103">Tutorial: Get started with ASP.NET Core SignalR</span></span>

<span data-ttu-id="0fef3-104">W tym samouczku pokazano podstawowe informacje dotyczące tworzenia aplikacji w czasie rzeczywistym przy użyciu biblioteki SignalR.</span><span class="sxs-lookup"><span data-stu-id="0fef3-104">This tutorial teaches the basics of building a real-time app using SignalR.</span></span> <span data-ttu-id="0fef3-105">Dowiesz się, jak:</span><span class="sxs-lookup"><span data-stu-id="0fef3-105">You learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="0fef3-106">Utwórz projekt sieci web.</span><span class="sxs-lookup"><span data-stu-id="0fef3-106">Create a web project.</span></span>
> * <span data-ttu-id="0fef3-107">Dodaj bibliotekę klienta SignalR.</span><span class="sxs-lookup"><span data-stu-id="0fef3-107">Add the SignalR client library.</span></span>
> * <span data-ttu-id="0fef3-108">Utwórz Centrum SignalR.</span><span class="sxs-lookup"><span data-stu-id="0fef3-108">Create a SignalR hub.</span></span>
> * <span data-ttu-id="0fef3-109">Skonfiguruj projekt do korzystania z SignalR.</span><span class="sxs-lookup"><span data-stu-id="0fef3-109">Configure the project to use SignalR.</span></span>
> * <span data-ttu-id="0fef3-110">Dodaj kod, który wysyła komunikaty z dowolnego klienta do wszystkich połączonych klientów.</span><span class="sxs-lookup"><span data-stu-id="0fef3-110">Add code that sends messages from any client to all connected clients.</span></span>

<span data-ttu-id="0fef3-111">Po zakończeniu będziesz mieć działającą aplikację rozmowy:</span><span class="sxs-lookup"><span data-stu-id="0fef3-111">At the end, you'll have a working chat app:</span></span>

![SignalR przykładowej aplikacji](signalr/_static/signalr-get-started-finished.png)

## <a name="prerequisites"></a><span data-ttu-id="0fef3-113">Wymagania wstępne</span><span class="sxs-lookup"><span data-stu-id="0fef3-113">Prerequisites</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="0fef3-114">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="0fef3-114">Visual Studio</span></span>](#tab/visual-studio)

[!INCLUDE[](~/includes/net-core-prereqs-vs-3.0.md)]

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="0fef3-115">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="0fef3-115">Visual Studio Code</span></span>](#tab/visual-studio-code)

[!INCLUDE[](~/includes/net-core-prereqs-vsc-3.0.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="0fef3-116">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="0fef3-116">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

[!INCLUDE[](~/includes/net-core-prereqs-mac-3.0.md)]

---

## <a name="create-a-web-app-project"></a><span data-ttu-id="0fef3-117">Tworzenie projektu aplikacji sieci web</span><span class="sxs-lookup"><span data-stu-id="0fef3-117">Create a web app project</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="0fef3-118">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="0fef3-118">Visual Studio</span></span>](#tab/visual-studio/)

* <span data-ttu-id="0fef3-119">W menu, wybierz opcję **Plik > Nowy projekt**.</span><span class="sxs-lookup"><span data-stu-id="0fef3-119">From the menu, select **File > New Project**.</span></span>

* <span data-ttu-id="0fef3-120">W **Utwórz nowy projekt** okno dialogowe, wybierz opcję **aplikacji sieci Web programu ASP.NET Core**, a następnie wybierz pozycję **dalej**.</span><span class="sxs-lookup"><span data-stu-id="0fef3-120">In the **Create a new project** dialog, select **ASP.NET Core Web Application**, and then select **Next**.</span></span>

* <span data-ttu-id="0fef3-121">W **konfigurowania nowego projektu** okno dialogowe, nazwę projektu *SignalRChat*, a następnie wybierz pozycję **Utwórz**.</span><span class="sxs-lookup"><span data-stu-id="0fef3-121">In the **Configure your new project** dialog, name the project *SignalRChat*, and then select **Create**.</span></span>

* <span data-ttu-id="0fef3-122">W **Tworzenie nowej aplikacji sieci Web platformy ASP.NET Core** okno dialogowe, wybierz opcję **platformy .NET Core** i **platformy ASP.NET Core 3.0**.</span><span class="sxs-lookup"><span data-stu-id="0fef3-122">In the **Create a new ASP.NET Core Web Application** dialog, select **.NET Core** and **ASP.NET Core 3.0**.</span></span> 

* <span data-ttu-id="0fef3-123">Wybierz **aplikacji sieci Web** utworzyć projekt, który korzysta ze stronami Razor, a następnie wybierz pozycję **Utwórz**.</span><span class="sxs-lookup"><span data-stu-id="0fef3-123">Select **Web Application** to create a project that uses Razor Pages, and then select **Create**.</span></span>

  ![Okno dialogowe nowego projektu w programie Visual Studio](signalr/_static/signalr-new-project-dialog.png)

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="0fef3-125">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="0fef3-125">Visual Studio Code</span></span>](#tab/visual-studio-code/)

* <span data-ttu-id="0fef3-126">Otwórz [zintegrowany terminal](https://code.visualstudio.com/docs/editor/integrated-terminal) do folderu, w którym zostanie utworzony nowy folder projektu.</span><span class="sxs-lookup"><span data-stu-id="0fef3-126">Open the [integrated terminal](https://code.visualstudio.com/docs/editor/integrated-terminal) to the folder in which the new project folder will be created.</span></span>

* <span data-ttu-id="0fef3-127">Uruchom następujące polecenia:</span><span class="sxs-lookup"><span data-stu-id="0fef3-127">Run the following commands:</span></span>

   ```console
   dotnet new webapp -o SignalRChat
   code -r SignalRChat
   ```

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="0fef3-128">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="0fef3-128">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="0fef3-129">W menu, wybierz opcję **Plik > nowe rozwiązanie**.</span><span class="sxs-lookup"><span data-stu-id="0fef3-129">From the menu, select **File > New Solution**.</span></span>

* <span data-ttu-id="0fef3-130">Wybierz **platformy .NET Core > aplikacji > Aplikacja sieci Web** (nie wybieraj **aplikacji sieci Web (Model-View-Controller)** ), a następnie wybierz pozycję **dalej**.</span><span class="sxs-lookup"><span data-stu-id="0fef3-130">Select **.NET Core > App > Web Application** (Don't select **Web Application (Model-View-Controller)**), and then select **Next**.</span></span>

* <span data-ttu-id="0fef3-131">Upewnij się, że **platformy docelowej** jest ustawiona na **.NET Core 3.0 to**, a następnie wybierz pozycję **dalej**.</span><span class="sxs-lookup"><span data-stu-id="0fef3-131">Make sure the **Target Framework** is set to **.NET Core 3.0**, and then select **Next**.</span></span>

* <span data-ttu-id="0fef3-132">Nadaj projektowi nazwę *SignalRChat*, a następnie wybierz pozycję **Utwórz**.</span><span class="sxs-lookup"><span data-stu-id="0fef3-132">Name the project *SignalRChat*, and then select **Create**.</span></span>

---

## <a name="add-the-signalr-client-library"></a><span data-ttu-id="0fef3-133">Dodaj bibliotekę klienta SignalR</span><span class="sxs-lookup"><span data-stu-id="0fef3-133">Add the SignalR client library</span></span>

<span data-ttu-id="0fef3-134">Serwer biblioteki SignalR znajduje się w udostępnionej platformy ASP.NET Core 3.0.</span><span class="sxs-lookup"><span data-stu-id="0fef3-134">The SignalR server library is included in the ASP.NET Core 3.0 shared framework.</span></span> <span data-ttu-id="0fef3-135">Biblioteki klienta JavaScript automatycznie nie jest zawarty w projekcie.</span><span class="sxs-lookup"><span data-stu-id="0fef3-135">The JavaScript client library isn't automatically included in the project.</span></span> <span data-ttu-id="0fef3-136">W tym samouczku używasz Library Manager (LibMan) można pobrać z biblioteki klienta z *unpkg*.</span><span class="sxs-lookup"><span data-stu-id="0fef3-136">For this tutorial, you use Library Manager (LibMan) to get the client library from *unpkg*.</span></span> <span data-ttu-id="0fef3-137">unpkg jest usługa content delivery network (CDN)), można dostarczać niczego w Menedżer pakietów npm oraz Menedżera pakietów środowiska Node.js.</span><span class="sxs-lookup"><span data-stu-id="0fef3-137">unpkg is a content delivery network (CDN)) that can deliver anything found in npm, the Node.js package manager.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="0fef3-138">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="0fef3-138">Visual Studio</span></span>](#tab/visual-studio/)

* <span data-ttu-id="0fef3-139">W **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy projekt i wybierz **Dodaj** > **biblioteki po stronie klienta**.</span><span class="sxs-lookup"><span data-stu-id="0fef3-139">In **Solution Explorer**, right-click the project, and select **Add** > **Client-Side Library**.</span></span>

* <span data-ttu-id="0fef3-140">W **Dodaj biblioteki po stronie klienta** okno dialogowe, aby uzyskać **dostawcy** wybierz **unpkg**.</span><span class="sxs-lookup"><span data-stu-id="0fef3-140">In the **Add Client-Side Library** dialog, for **Provider** select **unpkg**.</span></span>

* <span data-ttu-id="0fef3-141">Aby uzyskać **biblioteki**, wprowadź `@aspnet/signalr@next`.</span><span class="sxs-lookup"><span data-stu-id="0fef3-141">For **Library**, enter `@aspnet/signalr@next`.</span></span>
<!-- when 3.0 is released, change @next to @latest -->

* <span data-ttu-id="0fef3-142">Wybierz **wybierz konkretne pliki**, rozwiń węzeł *dist/przeglądarki* folder, a następnie wybierz *signalr.js* i *signalr.min.js*.</span><span class="sxs-lookup"><span data-stu-id="0fef3-142">Select **Choose specific files**, expand the *dist/browser* folder, and select *signalr.js* and *signalr.min.js*.</span></span>

* <span data-ttu-id="0fef3-143">Ustaw **lokalizacji docelowej** do *wwwroot/lib/signalr/* i wybierz **zainstalować**.</span><span class="sxs-lookup"><span data-stu-id="0fef3-143">Set **Target Location** to *wwwroot/lib/signalr/*, and select **Install**.</span></span>

  ![Dodaj okno dialogowe biblioteki po stronie klienta — Wybieranie biblioteki](signalr/_static/libman1.png)

  <span data-ttu-id="0fef3-145">Tworzy LibMan *wwwroot/lib/signalr* folder i kopiuje wybrane pliki.</span><span class="sxs-lookup"><span data-stu-id="0fef3-145">LibMan creates a *wwwroot/lib/signalr* folder and copies the selected files to it.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="0fef3-146">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="0fef3-146">Visual Studio Code</span></span>](#tab/visual-studio-code/)

* <span data-ttu-id="0fef3-147">W zintegrowanym terminalu uruchom następujące polecenie, aby zainstalować LibMan.</span><span class="sxs-lookup"><span data-stu-id="0fef3-147">In the integrated terminal, run the following command to install LibMan.</span></span>

  ```console
  dotnet tool install -g Microsoft.Web.LibraryManager.Cli
  ```

* <span data-ttu-id="0fef3-148">Uruchom następujące polecenie, aby przy użyciu LibMan uzyskać biblioteki klienta SignalR.</span><span class="sxs-lookup"><span data-stu-id="0fef3-148">Run the following command to get the SignalR client library by using LibMan.</span></span> <span data-ttu-id="0fef3-149">Być może musisz odczekaj kilka sekund, zanim dane wyjściowe.</span><span class="sxs-lookup"><span data-stu-id="0fef3-149">You might have to wait a few seconds before seeing output.</span></span>

  ```console
  libman install @aspnet/signalr@next -p unpkg -d wwwroot/lib/signalr --files dist/browser/signalr.js --files dist/browser/signalr.min.js
  ```

  <span data-ttu-id="0fef3-150">Parametry określ następujące opcje:</span><span class="sxs-lookup"><span data-stu-id="0fef3-150">The parameters specify the following options:</span></span>
  * <span data-ttu-id="0fef3-151">Użyj dostawcy unpkg.</span><span class="sxs-lookup"><span data-stu-id="0fef3-151">Use the unpkg provider.</span></span>
  * <span data-ttu-id="0fef3-152">Skopiuj pliki do *wwwroot/lib/signalr* docelowego.</span><span class="sxs-lookup"><span data-stu-id="0fef3-152">Copy files to the *wwwroot/lib/signalr* destination.</span></span>
  * <span data-ttu-id="0fef3-153">Skopiuj tylko określonych plików.</span><span class="sxs-lookup"><span data-stu-id="0fef3-153">Copy only the specified files.</span></span>

  <span data-ttu-id="0fef3-154">Dane wyjściowe wyglądają jak w poniższym przykładzie:</span><span class="sxs-lookup"><span data-stu-id="0fef3-154">The output looks like the following example:</span></span>

  ```console
  wwwroot/lib/signalr/dist/browser/signalr.js written to disk
  wwwroot/lib/signalr/dist/browser/signalr.min.js written to disk
  Installed library "@aspnet/signalr@next" to "wwwroot/lib/signalr"
  ```

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="0fef3-155">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="0fef3-155">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="0fef3-156">W **terminalu**, uruchom następujące polecenie, aby zainstalować LibMan.</span><span class="sxs-lookup"><span data-stu-id="0fef3-156">In the **Terminal**, run the following command to install LibMan.</span></span>

  ```console
  dotnet tool install -g Microsoft.Web.LibraryManager.Cli
  ```

* <span data-ttu-id="0fef3-157">Przejdź do folderu projektu (zawierającego *SignalRChat.csproj* pliku).</span><span class="sxs-lookup"><span data-stu-id="0fef3-157">Navigate to the project folder (the one that contains the *SignalRChat.csproj* file).</span></span>

* <span data-ttu-id="0fef3-158">Uruchom następujące polecenie, aby przy użyciu LibMan uzyskać biblioteki klienta SignalR.</span><span class="sxs-lookup"><span data-stu-id="0fef3-158">Run the following command to get the SignalR client library by using LibMan.</span></span>

  ```console
  libman install @aspnet/signalr@next -p unpkg -d wwwroot/lib/signalr --files dist/browser/signalr.js --files dist/browser/signalr.min.js
  ```

  <span data-ttu-id="0fef3-159">Parametry określ następujące opcje:</span><span class="sxs-lookup"><span data-stu-id="0fef3-159">The parameters specify the following options:</span></span>
  * <span data-ttu-id="0fef3-160">Użyj dostawcy unpkg.</span><span class="sxs-lookup"><span data-stu-id="0fef3-160">Use the unpkg provider.</span></span>
  * <span data-ttu-id="0fef3-161">Skopiuj pliki do *wwwroot/lib/signalr* docelowego.</span><span class="sxs-lookup"><span data-stu-id="0fef3-161">Copy files to the *wwwroot/lib/signalr* destination.</span></span>
  * <span data-ttu-id="0fef3-162">Skopiuj tylko określonych plików.</span><span class="sxs-lookup"><span data-stu-id="0fef3-162">Copy only the specified files.</span></span>

  <span data-ttu-id="0fef3-163">Dane wyjściowe wyglądają jak w poniższym przykładzie:</span><span class="sxs-lookup"><span data-stu-id="0fef3-163">The output looks like the following example:</span></span>

  ```console
  wwwroot/lib/signalr/dist/browser/signalr.js written to disk
  wwwroot/lib/signalr/dist/browser/signalr.min.js written to disk
  Installed library "@aspnet/signalr@next" to "wwwroot/lib/signalr"
  ```

---

## <a name="create-a-signalr-hub"></a><span data-ttu-id="0fef3-164">Tworzenie Centrum SignalR</span><span class="sxs-lookup"><span data-stu-id="0fef3-164">Create a SignalR hub</span></span>

<span data-ttu-id="0fef3-165">A *Centrum* jest klasa, która służy jako ogólny potok, który obsługuje komunikację klient serwer.</span><span class="sxs-lookup"><span data-stu-id="0fef3-165">A *hub* is a class that serves as a high-level pipeline that handles client-server communication.</span></span>

* <span data-ttu-id="0fef3-166">W folderze projektu SignalRChat Utwórz *koncentratory* folderu.</span><span class="sxs-lookup"><span data-stu-id="0fef3-166">In the SignalRChat project folder, create a *Hubs* folder.</span></span>

* <span data-ttu-id="0fef3-167">W *Hubs* folderze utwórz *ChatHub.cs* pliku następującym kodem:</span><span class="sxs-lookup"><span data-stu-id="0fef3-167">In the *Hubs* folder, create a *ChatHub.cs* file with the following code:</span></span>

  [!code-csharp[Startup](signalr/sample-snapshot/ChatHub.cs)]

  <span data-ttu-id="0fef3-168">`ChatHub` Klasa dziedziczy z elementu SignalR `Hub` klasy.</span><span class="sxs-lookup"><span data-stu-id="0fef3-168">The `ChatHub` class inherits from the SignalR `Hub` class.</span></span> <span data-ttu-id="0fef3-169">`Hub` Klasa zarządza połączeń, grup i komunikatów.</span><span class="sxs-lookup"><span data-stu-id="0fef3-169">The `Hub` class manages connections, groups, and messaging.</span></span>

  <span data-ttu-id="0fef3-170">`SendMessage` Metoda może być wywoływana przez połączone klienta, aby wysłać wiadomość do wszystkich klientów.</span><span class="sxs-lookup"><span data-stu-id="0fef3-170">The `SendMessage` method can be called by a connected client to send a message to all clients.</span></span> <span data-ttu-id="0fef3-171">Kod klienta JavaScript, który wywołuje metodę przedstawiono w dalszej części tego samouczka.</span><span class="sxs-lookup"><span data-stu-id="0fef3-171">JavaScript client code that calls the method is shown later in the tutorial.</span></span> <span data-ttu-id="0fef3-172">Kod SignalR jest asynchroniczne w celu zapewnienia maksymalnej skalowalności.</span><span class="sxs-lookup"><span data-stu-id="0fef3-172">SignalR code is asynchronous to provide maximum scalability.</span></span>

## <a name="configure-signalr"></a><span data-ttu-id="0fef3-173">Konfigurowanie biblioteki SignalR</span><span class="sxs-lookup"><span data-stu-id="0fef3-173">Configure SignalR</span></span>

<span data-ttu-id="0fef3-174">Serwer biblioteki SignalR musi być skonfigurowany do przekazywania żądań SignalR z SignalR.</span><span class="sxs-lookup"><span data-stu-id="0fef3-174">The SignalR server must be configured to pass SignalR requests to SignalR.</span></span>

* <span data-ttu-id="0fef3-175">Dodaj następujący wyróżniony kod do *Startup.cs* pliku.</span><span class="sxs-lookup"><span data-stu-id="0fef3-175">Add the following highlighted code to the *Startup.cs* file.</span></span>

  [!code-csharp[Startup](signalr/sample-snapshot/Startup.cs?highlight=6,30,58)]

  <span data-ttu-id="0fef3-176">Te zmiany dodanie SignalR do wstrzykiwania zależności platformy ASP.NET Core i routing systemów.</span><span class="sxs-lookup"><span data-stu-id="0fef3-176">These changes add SignalR to the ASP.NET Core dependency injection and routing systems.</span></span>

## <a name="add-signalr-client-code"></a><span data-ttu-id="0fef3-177">Dodaj kod klienta SignalR</span><span class="sxs-lookup"><span data-stu-id="0fef3-177">Add SignalR client code</span></span>

* <span data-ttu-id="0fef3-178">Zastąp zawartość *Pages\Index.cshtml* następującym kodem:</span><span class="sxs-lookup"><span data-stu-id="0fef3-178">Replace the content in *Pages\Index.cshtml* with the following code:</span></span>

  [!code-cshtml[Index](signalr/sample-snapshot/Index.cshtml)]

  <span data-ttu-id="0fef3-179">Powyższy kod:</span><span class="sxs-lookup"><span data-stu-id="0fef3-179">The preceding code:</span></span>

  * <span data-ttu-id="0fef3-180">Tworzy pola tekstowe nazwy i tekst wiadomość i przycisk Prześlij.</span><span class="sxs-lookup"><span data-stu-id="0fef3-180">Creates text boxes for name and message text, and a submit button.</span></span>
  * <span data-ttu-id="0fef3-181">Tworzy listę z `id="messagesList"` do wyświetlania komunikatów odebranych z Centrum SignalR.</span><span class="sxs-lookup"><span data-stu-id="0fef3-181">Creates a list with `id="messagesList"` for displaying messages that are received from the SignalR hub.</span></span>
  * <span data-ttu-id="0fef3-182">Zawiera odwołania do skryptu z SignalR i *chat.js* kod aplikacji, który zostanie utworzony w następnym kroku.</span><span class="sxs-lookup"><span data-stu-id="0fef3-182">Includes script references to SignalR and the *chat.js* application code that you create in the next step.</span></span>

* <span data-ttu-id="0fef3-183">W *wwwroot/js* folderze utwórz *chat.js* pliku następującym kodem:</span><span class="sxs-lookup"><span data-stu-id="0fef3-183">In the *wwwroot/js* folder, create a *chat.js* file with the following code:</span></span>

  [!code-javascript[Index](signalr/sample-snapshot/chat.js)]

  <span data-ttu-id="0fef3-184">Powyższy kod:</span><span class="sxs-lookup"><span data-stu-id="0fef3-184">The preceding code:</span></span>

  * <span data-ttu-id="0fef3-185">Tworzy i uruchamia połączenie.</span><span class="sxs-lookup"><span data-stu-id="0fef3-185">Creates and starts a connection.</span></span>
  * <span data-ttu-id="0fef3-186">Dodaje przycisk przesyłania program obsługi, która wysyła komunikaty do Centrum.</span><span class="sxs-lookup"><span data-stu-id="0fef3-186">Adds to the submit button a handler that sends messages to the hub.</span></span>
  * <span data-ttu-id="0fef3-187">Dodaje do obiektu połączenia program obsługi, który odbiera komunikaty z Centrum i dodaje je do listy.</span><span class="sxs-lookup"><span data-stu-id="0fef3-187">Adds to the connection object a handler that receives messages from the hub and adds them to the list.</span></span>

## <a name="run-the-app"></a><span data-ttu-id="0fef3-188">Uruchamianie aplikacji</span><span class="sxs-lookup"><span data-stu-id="0fef3-188">Run the app</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="0fef3-189">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="0fef3-189">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="0fef3-190">Naciśnij klawisz **kombinację klawiszy CTRL + F5** Aby uruchomić aplikację bez debugowania.</span><span class="sxs-lookup"><span data-stu-id="0fef3-190">Press **CTRL+F5** to run the app without debugging.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="0fef3-191">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="0fef3-191">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="0fef3-192">W zintegrowanym terminalu uruchom następujące polecenie:</span><span class="sxs-lookup"><span data-stu-id="0fef3-192">In the integrated terminal, run the following command:</span></span>

  ```console
  dotnet run -p SignalRChat.csproj
  ```

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="0fef3-193">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="0fef3-193">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="0fef3-194">W menu, wybierz opcję **Uruchom > Uruchom bez debugowania**.</span><span class="sxs-lookup"><span data-stu-id="0fef3-194">From the menu, select **Run > Start Without Debugging**.</span></span>

---

* <span data-ttu-id="0fef3-195">Skopiuj adres URL z paska adresu, a następnie otwórz innego wystąpienia przeglądarki lub karty i wklej adres URL w pasku adresu.</span><span class="sxs-lookup"><span data-stu-id="0fef3-195">Copy the URL from the address bar, open another browser instance or tab, and paste the URL in the address bar.</span></span>

* <span data-ttu-id="0fef3-196">Wybierz albo przeglądarki, wprowadź nazwę i komunikat, a następnie wybierz **wysyłania komunikatu** przycisku.</span><span class="sxs-lookup"><span data-stu-id="0fef3-196">Choose either browser, enter a name and message, and select the **Send Message** button.</span></span>

  <span data-ttu-id="0fef3-197">Nazwa i wiadomości są wyświetlane na obu stronach natychmiast.</span><span class="sxs-lookup"><span data-stu-id="0fef3-197">The name and message are displayed on both pages instantly.</span></span>

  ![SignalR przykładowej aplikacji](signalr/_static/signalr-get-started-finished.png)

> [!TIP]
> * <span data-ttu-id="0fef3-199">Jeśli aplikacja nie działa, Otwórz swoje narzędzia deweloperskie przeglądarki (F12) i przejdź do konsoli.</span><span class="sxs-lookup"><span data-stu-id="0fef3-199">If the app doesn't work, open your browser developer tools (F12) and go to the console.</span></span> <span data-ttu-id="0fef3-200">Może pojawić się błędy związane z Twoim kodem HTML i JavaScript.</span><span class="sxs-lookup"><span data-stu-id="0fef3-200">You might see errors related to your HTML and JavaScript code.</span></span> <span data-ttu-id="0fef3-201">Załóżmy, że możesz umieścić *signalr.js* w innym folderze niż bezpośredniego.</span><span class="sxs-lookup"><span data-stu-id="0fef3-201">For example, suppose you put *signalr.js* in a different folder than directed.</span></span> <span data-ttu-id="0fef3-202">W takim przypadku odwołania do tego pliku nie będzie działać, a następnie zostanie wyświetlony błąd 404 w konsoli.</span><span class="sxs-lookup"><span data-stu-id="0fef3-202">In that case the reference to that file won't work and you'll see a 404 error in the console.</span></span>
>   <span data-ttu-id="0fef3-203">![błąd — nie znaleziono signalr.js](signalr/_static/f12-console.png)</span><span class="sxs-lookup"><span data-stu-id="0fef3-203">![signalr.js not found error](signalr/_static/f12-console.png)</span></span>
> * <span data-ttu-id="0fef3-204">Jeśli wystąpi błąd, ERR_SPDY_INADEQUATE_TRANSPORT_SECURITY w przeglądarce Chrome lub NS_ERROR_NET_INADEQUATE_SECURITY w przeglądarce Firefox, uruchom następujące polecenia, aby zaktualizować certyfikat deweloperski:</span><span class="sxs-lookup"><span data-stu-id="0fef3-204">If you get the error ERR_SPDY_INADEQUATE_TRANSPORT_SECURITY in Chrome or NS_ERROR_NET_INADEQUATE_SECURITY in Firefox, run these commands to update your development certificate:</span></span>
>   ```
>   dotnet dev-certs https --clean
>   dotnet dev-certs https --trust
>   ```

## <a name="next-steps"></a><span data-ttu-id="0fef3-205">Następne kroki</span><span class="sxs-lookup"><span data-stu-id="0fef3-205">Next steps</span></span>

<span data-ttu-id="0fef3-206">Aby dowiedzieć się więcej na temat biblioteki SignalR, zobacz wprowadzenie:</span><span class="sxs-lookup"><span data-stu-id="0fef3-206">To learn more about SignalR, see the introduction:</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="0fef3-207">Wprowadzenie do SignalR platformy ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="0fef3-207">Introduction to ASP.NET Core SignalR</span></span>](xref:signalr/introduction)
