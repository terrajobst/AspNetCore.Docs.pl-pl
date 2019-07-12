---
title: Wprowadzenie do biblioteki SignalR platformy ASP.NET Core
author: bradygaster
description: W tym samouczku utworzysz aplikację rozmowy, która korzysta z biblioteki SignalR platformy ASP.NET Core.
ms.author: bradyg
ms.custom: mvc
ms.date: 07/08/2019
uid: tutorials/signalr
ms.openlocfilehash: 5588cab1ebbe8fbd6d134025f07f9580ecafd89f
ms.sourcegitcommit: 7a40c56bf6a6aaa63a7ee83a2cac9b3a1d77555e
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/12/2019
ms.locfileid: "67855792"
---
# <a name="tutorial-get-started-with-aspnet-core-signalr"></a><span data-ttu-id="30475-103">Samouczek: Wprowadzenie do biblioteki SignalR platformy ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="30475-103">Tutorial: Get started with ASP.NET Core SignalR</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="30475-104">W tym samouczku pokazano podstawowe informacje dotyczące tworzenia aplikacji w czasie rzeczywistym przy użyciu biblioteki SignalR.</span><span class="sxs-lookup"><span data-stu-id="30475-104">This tutorial teaches the basics of building a real-time app using SignalR.</span></span> <span data-ttu-id="30475-105">Dowiesz się, jak:</span><span class="sxs-lookup"><span data-stu-id="30475-105">You learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="30475-106">Utwórz projekt sieci web.</span><span class="sxs-lookup"><span data-stu-id="30475-106">Create a web project.</span></span>
> * <span data-ttu-id="30475-107">Dodaj bibliotekę klienta SignalR.</span><span class="sxs-lookup"><span data-stu-id="30475-107">Add the SignalR client library.</span></span>
> * <span data-ttu-id="30475-108">Utwórz Centrum SignalR.</span><span class="sxs-lookup"><span data-stu-id="30475-108">Create a SignalR hub.</span></span>
> * <span data-ttu-id="30475-109">Skonfiguruj projekt do korzystania z SignalR.</span><span class="sxs-lookup"><span data-stu-id="30475-109">Configure the project to use SignalR.</span></span>
> * <span data-ttu-id="30475-110">Dodaj kod, który wysyła komunikaty z dowolnego klienta do wszystkich połączonych klientów.</span><span class="sxs-lookup"><span data-stu-id="30475-110">Add code that sends messages from any client to all connected clients.</span></span>

<span data-ttu-id="30475-111">Po zakończeniu będziesz mieć działającą aplikację rozmowy:</span><span class="sxs-lookup"><span data-stu-id="30475-111">At the end, you'll have a working chat app:</span></span>

![SignalR przykładowej aplikacji](signalr/_static/3.x/signalr-get-started-finished.png)

## <a name="prerequisites"></a><span data-ttu-id="30475-113">Wymagania wstępne</span><span class="sxs-lookup"><span data-stu-id="30475-113">Prerequisites</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="30475-114">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="30475-114">Visual Studio</span></span>](#tab/visual-studio)

[!INCLUDE[](~/includes/net-core-prereqs-vs-3.0.md)]

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="30475-115">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="30475-115">Visual Studio Code</span></span>](#tab/visual-studio-code)

[!INCLUDE[](~/includes/net-core-prereqs-vsc-3.0.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="30475-116">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="30475-116">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

[!INCLUDE[](~/includes/net-core-prereqs-mac-3.0.md)]

---

## <a name="create-a-web-app-project"></a><span data-ttu-id="30475-117">Tworzenie projektu aplikacji sieci web</span><span class="sxs-lookup"><span data-stu-id="30475-117">Create a web app project</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="30475-118">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="30475-118">Visual Studio</span></span>](#tab/visual-studio/)

* <span data-ttu-id="30475-119">W menu, wybierz opcję **Plik > Nowy projekt**.</span><span class="sxs-lookup"><span data-stu-id="30475-119">From the menu, select **File > New Project**.</span></span>

* <span data-ttu-id="30475-120">W **Utwórz nowy projekt** okno dialogowe, wybierz opcję **aplikacji sieci Web programu ASP.NET Core**, a następnie wybierz pozycję **dalej**.</span><span class="sxs-lookup"><span data-stu-id="30475-120">In the **Create a new project** dialog, select **ASP.NET Core Web Application**, and then select **Next**.</span></span>

* <span data-ttu-id="30475-121">W **konfigurowania nowego projektu** okno dialogowe, nazwę projektu *SignalRChat*, a następnie wybierz pozycję **Utwórz**.</span><span class="sxs-lookup"><span data-stu-id="30475-121">In the **Configure your new project** dialog, name the project *SignalRChat*, and then select **Create**.</span></span>

* <span data-ttu-id="30475-122">W **Tworzenie nowej aplikacji sieci Web platformy ASP.NET Core** okno dialogowe, wybierz opcję **platformy .NET Core** i **platformy ASP.NET Core 3.0**.</span><span class="sxs-lookup"><span data-stu-id="30475-122">In the **Create a new ASP.NET Core Web Application** dialog, select **.NET Core** and **ASP.NET Core 3.0**.</span></span> 

* <span data-ttu-id="30475-123">Wybierz **aplikacji sieci Web** utworzyć projekt, który korzysta ze stronami Razor, a następnie wybierz pozycję **Utwórz**.</span><span class="sxs-lookup"><span data-stu-id="30475-123">Select **Web Application** to create a project that uses Razor Pages, and then select **Create**.</span></span>

  ![Okno dialogowe nowego projektu w programie Visual Studio](signalr/_static/3.x/signalr-new-project-dialog.png)

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="30475-125">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="30475-125">Visual Studio Code</span></span>](#tab/visual-studio-code/)

* <span data-ttu-id="30475-126">Otwórz [zintegrowany terminal](https://code.visualstudio.com/docs/editor/integrated-terminal) do folderu, w którym zostanie utworzony nowy folder projektu.</span><span class="sxs-lookup"><span data-stu-id="30475-126">Open the [integrated terminal](https://code.visualstudio.com/docs/editor/integrated-terminal) to the folder in which the new project folder will be created.</span></span>

* <span data-ttu-id="30475-127">Uruchom następujące polecenia:</span><span class="sxs-lookup"><span data-stu-id="30475-127">Run the following commands:</span></span>

   ```console
   dotnet new webapp -o SignalRChat
   code -r SignalRChat
   ```

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="30475-128">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="30475-128">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="30475-129">W menu, wybierz opcję **Plik > nowe rozwiązanie**.</span><span class="sxs-lookup"><span data-stu-id="30475-129">From the menu, select **File > New Solution**.</span></span>

* <span data-ttu-id="30475-130">Wybierz **platformy .NET Core > aplikacji > Aplikacja sieci Web** (nie wybieraj **aplikacji sieci Web (Model-View-Controller)** ), a następnie wybierz pozycję **dalej**.</span><span class="sxs-lookup"><span data-stu-id="30475-130">Select **.NET Core > App > Web Application** (Don't select **Web Application (Model-View-Controller)**), and then select **Next**.</span></span>

* <span data-ttu-id="30475-131">Upewnij się, że **platformy docelowej** jest ustawiona na **.NET Core 3.0 to**, a następnie wybierz pozycję **dalej**.</span><span class="sxs-lookup"><span data-stu-id="30475-131">Make sure the **Target Framework** is set to **.NET Core 3.0**, and then select **Next**.</span></span>

* <span data-ttu-id="30475-132">Nadaj projektowi nazwę *SignalRChat*, a następnie wybierz pozycję **Utwórz**.</span><span class="sxs-lookup"><span data-stu-id="30475-132">Name the project *SignalRChat*, and then select **Create**.</span></span>

---

## <a name="add-the-signalr-client-library"></a><span data-ttu-id="30475-133">Dodaj bibliotekę klienta SignalR</span><span class="sxs-lookup"><span data-stu-id="30475-133">Add the SignalR client library</span></span>

<span data-ttu-id="30475-134">Serwer biblioteki SignalR znajduje się w udostępnionej platformy ASP.NET Core 3.0.</span><span class="sxs-lookup"><span data-stu-id="30475-134">The SignalR server library is included in the ASP.NET Core 3.0 shared framework.</span></span> <span data-ttu-id="30475-135">Biblioteki klienta JavaScript automatycznie nie jest zawarty w projekcie.</span><span class="sxs-lookup"><span data-stu-id="30475-135">The JavaScript client library isn't automatically included in the project.</span></span> <span data-ttu-id="30475-136">W tym samouczku używasz Library Manager (LibMan) można pobrać z biblioteki klienta z *unpkg*.</span><span class="sxs-lookup"><span data-stu-id="30475-136">For this tutorial, you use Library Manager (LibMan) to get the client library from *unpkg*.</span></span> <span data-ttu-id="30475-137">unpkg jest usługa content delivery network (CDN)), można dostarczać niczego w Menedżer pakietów npm oraz Menedżera pakietów środowiska Node.js.</span><span class="sxs-lookup"><span data-stu-id="30475-137">unpkg is a content delivery network (CDN)) that can deliver anything found in npm, the Node.js package manager.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="30475-138">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="30475-138">Visual Studio</span></span>](#tab/visual-studio/)

* <span data-ttu-id="30475-139">W **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy projekt i wybierz **Dodaj** > **biblioteki po stronie klienta**.</span><span class="sxs-lookup"><span data-stu-id="30475-139">In **Solution Explorer**, right-click the project, and select **Add** > **Client-Side Library**.</span></span>

* <span data-ttu-id="30475-140">W **Dodaj biblioteki po stronie klienta** okno dialogowe, aby uzyskać **dostawcy** wybierz **unpkg**.</span><span class="sxs-lookup"><span data-stu-id="30475-140">In the **Add Client-Side Library** dialog, for **Provider** select **unpkg**.</span></span>

* <span data-ttu-id="30475-141">Aby uzyskać **biblioteki**, wprowadź `@aspnet/signalr@next`.</span><span class="sxs-lookup"><span data-stu-id="30475-141">For **Library**, enter `@aspnet/signalr@next`.</span></span>
<!-- when 3.0 is released, change @next to @latest -->

* <span data-ttu-id="30475-142">Wybierz **wybierz konkretne pliki**, rozwiń węzeł *dist/przeglądarki* folder, a następnie wybierz *signalr.js* i *signalr.min.js*.</span><span class="sxs-lookup"><span data-stu-id="30475-142">Select **Choose specific files**, expand the *dist/browser* folder, and select *signalr.js* and *signalr.min.js*.</span></span>

* <span data-ttu-id="30475-143">Ustaw **lokalizacji docelowej** do *wwwroot/lib/signalr/* i wybierz **zainstalować**.</span><span class="sxs-lookup"><span data-stu-id="30475-143">Set **Target Location** to *wwwroot/lib/signalr/*, and select **Install**.</span></span>

  ![Dodaj okno dialogowe biblioteki po stronie klienta — Wybieranie biblioteki](signalr/_static/3.x/libman1.png)

  <span data-ttu-id="30475-145">Tworzy LibMan *wwwroot/lib/signalr* folder i kopiuje wybrane pliki.</span><span class="sxs-lookup"><span data-stu-id="30475-145">LibMan creates a *wwwroot/lib/signalr* folder and copies the selected files to it.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="30475-146">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="30475-146">Visual Studio Code</span></span>](#tab/visual-studio-code/)

* <span data-ttu-id="30475-147">W zintegrowanym terminalu uruchom następujące polecenie, aby zainstalować LibMan.</span><span class="sxs-lookup"><span data-stu-id="30475-147">In the integrated terminal, run the following command to install LibMan.</span></span>

  ```console
  dotnet tool install -g Microsoft.Web.LibraryManager.Cli
  ```

* <span data-ttu-id="30475-148">Uruchom następujące polecenie, aby przy użyciu LibMan uzyskać biblioteki klienta SignalR.</span><span class="sxs-lookup"><span data-stu-id="30475-148">Run the following command to get the SignalR client library by using LibMan.</span></span> <span data-ttu-id="30475-149">Być może musisz odczekaj kilka sekund, zanim dane wyjściowe.</span><span class="sxs-lookup"><span data-stu-id="30475-149">You might have to wait a few seconds before seeing output.</span></span>

  ```console
  libman install @aspnet/signalr@next -p unpkg -d wwwroot/lib/signalr --files dist/browser/signalr.js --files dist/browser/signalr.min.js
  ```

  <span data-ttu-id="30475-150">Parametry określ następujące opcje:</span><span class="sxs-lookup"><span data-stu-id="30475-150">The parameters specify the following options:</span></span>
  * <span data-ttu-id="30475-151">Użyj dostawcy unpkg.</span><span class="sxs-lookup"><span data-stu-id="30475-151">Use the unpkg provider.</span></span>
  * <span data-ttu-id="30475-152">Skopiuj pliki do *wwwroot/lib/signalr* docelowego.</span><span class="sxs-lookup"><span data-stu-id="30475-152">Copy files to the *wwwroot/lib/signalr* destination.</span></span>
  * <span data-ttu-id="30475-153">Skopiuj tylko określonych plików.</span><span class="sxs-lookup"><span data-stu-id="30475-153">Copy only the specified files.</span></span>

  <span data-ttu-id="30475-154">Dane wyjściowe wyglądają jak w poniższym przykładzie:</span><span class="sxs-lookup"><span data-stu-id="30475-154">The output looks like the following example:</span></span>

  ```console
  wwwroot/lib/signalr/dist/browser/signalr.js written to disk
  wwwroot/lib/signalr/dist/browser/signalr.min.js written to disk
  Installed library "@aspnet/signalr@next" to "wwwroot/lib/signalr"
  ```

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="30475-155">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="30475-155">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="30475-156">W **terminalu**, uruchom następujące polecenie, aby zainstalować LibMan.</span><span class="sxs-lookup"><span data-stu-id="30475-156">In the **Terminal**, run the following command to install LibMan.</span></span>

  ```console
  dotnet tool install -g Microsoft.Web.LibraryManager.Cli
  ```

* <span data-ttu-id="30475-157">Przejdź do folderu projektu (zawierającego *SignalRChat.csproj* pliku).</span><span class="sxs-lookup"><span data-stu-id="30475-157">Navigate to the project folder (the one that contains the *SignalRChat.csproj* file).</span></span>

* <span data-ttu-id="30475-158">Uruchom następujące polecenie, aby przy użyciu LibMan uzyskać biblioteki klienta SignalR.</span><span class="sxs-lookup"><span data-stu-id="30475-158">Run the following command to get the SignalR client library by using LibMan.</span></span>

  ```console
  libman install @aspnet/signalr@next -p unpkg -d wwwroot/lib/signalr --files dist/browser/signalr.js --files dist/browser/signalr.min.js
  ```

  <span data-ttu-id="30475-159">Parametry określ następujące opcje:</span><span class="sxs-lookup"><span data-stu-id="30475-159">The parameters specify the following options:</span></span>
  * <span data-ttu-id="30475-160">Użyj dostawcy unpkg.</span><span class="sxs-lookup"><span data-stu-id="30475-160">Use the unpkg provider.</span></span>
  * <span data-ttu-id="30475-161">Skopiuj pliki do *wwwroot/lib/signalr* docelowego.</span><span class="sxs-lookup"><span data-stu-id="30475-161">Copy files to the *wwwroot/lib/signalr* destination.</span></span>
  * <span data-ttu-id="30475-162">Skopiuj tylko określonych plików.</span><span class="sxs-lookup"><span data-stu-id="30475-162">Copy only the specified files.</span></span>

  <span data-ttu-id="30475-163">Dane wyjściowe wyglądają jak w poniższym przykładzie:</span><span class="sxs-lookup"><span data-stu-id="30475-163">The output looks like the following example:</span></span>

  ```console
  wwwroot/lib/signalr/dist/browser/signalr.js written to disk
  wwwroot/lib/signalr/dist/browser/signalr.min.js written to disk
  Installed library "@aspnet/signalr@next" to "wwwroot/lib/signalr"
  ```

---

## <a name="create-a-signalr-hub"></a><span data-ttu-id="30475-164">Tworzenie Centrum SignalR</span><span class="sxs-lookup"><span data-stu-id="30475-164">Create a SignalR hub</span></span>

<span data-ttu-id="30475-165">A *Centrum* jest klasa, która służy jako ogólny potok, który obsługuje komunikację klient serwer.</span><span class="sxs-lookup"><span data-stu-id="30475-165">A *hub* is a class that serves as a high-level pipeline that handles client-server communication.</span></span>

* <span data-ttu-id="30475-166">W folderze projektu SignalRChat Utwórz *koncentratory* folderu.</span><span class="sxs-lookup"><span data-stu-id="30475-166">In the SignalRChat project folder, create a *Hubs* folder.</span></span>

* <span data-ttu-id="30475-167">W *Hubs* folderze utwórz *ChatHub.cs* pliku następującym kodem:</span><span class="sxs-lookup"><span data-stu-id="30475-167">In the *Hubs* folder, create a *ChatHub.cs* file with the following code:</span></span>

  [!code-csharp[Startup](signalr/sample-snapshot/3.x/ChatHub.cs)]

  <span data-ttu-id="30475-168">`ChatHub` Klasa dziedziczy z elementu SignalR `Hub` klasy.</span><span class="sxs-lookup"><span data-stu-id="30475-168">The `ChatHub` class inherits from the SignalR `Hub` class.</span></span> <span data-ttu-id="30475-169">`Hub` Klasa zarządza połączeń, grup i komunikatów.</span><span class="sxs-lookup"><span data-stu-id="30475-169">The `Hub` class manages connections, groups, and messaging.</span></span>

  <span data-ttu-id="30475-170">`SendMessage` Metoda może być wywoływana przez połączone klienta, aby wysłać wiadomość do wszystkich klientów.</span><span class="sxs-lookup"><span data-stu-id="30475-170">The `SendMessage` method can be called by a connected client to send a message to all clients.</span></span> <span data-ttu-id="30475-171">Kod klienta JavaScript, który wywołuje metodę przedstawiono w dalszej części tego samouczka.</span><span class="sxs-lookup"><span data-stu-id="30475-171">JavaScript client code that calls the method is shown later in the tutorial.</span></span> <span data-ttu-id="30475-172">Kod SignalR jest asynchroniczne w celu zapewnienia maksymalnej skalowalności.</span><span class="sxs-lookup"><span data-stu-id="30475-172">SignalR code is asynchronous to provide maximum scalability.</span></span>

## <a name="configure-signalr"></a><span data-ttu-id="30475-173">Konfigurowanie biblioteki SignalR</span><span class="sxs-lookup"><span data-stu-id="30475-173">Configure SignalR</span></span>

<span data-ttu-id="30475-174">Serwer biblioteki SignalR musi być skonfigurowany do przekazywania żądań SignalR z SignalR.</span><span class="sxs-lookup"><span data-stu-id="30475-174">The SignalR server must be configured to pass SignalR requests to SignalR.</span></span>

* <span data-ttu-id="30475-175">Dodaj następujący wyróżniony kod do *Startup.cs* pliku.</span><span class="sxs-lookup"><span data-stu-id="30475-175">Add the following highlighted code to the *Startup.cs* file.</span></span>

  [!code-csharp[Startup](signalr/sample-snapshot/3.x/Startup.cs?highlight=6,30,58)]

  <span data-ttu-id="30475-176">Te zmiany dodanie SignalR do wstrzykiwania zależności platformy ASP.NET Core i routing systemów.</span><span class="sxs-lookup"><span data-stu-id="30475-176">These changes add SignalR to the ASP.NET Core dependency injection and routing systems.</span></span>

## <a name="add-signalr-client-code"></a><span data-ttu-id="30475-177">Dodaj kod klienta SignalR</span><span class="sxs-lookup"><span data-stu-id="30475-177">Add SignalR client code</span></span>

* <span data-ttu-id="30475-178">Zastąp zawartość *Pages\Index.cshtml* następującym kodem:</span><span class="sxs-lookup"><span data-stu-id="30475-178">Replace the content in *Pages\Index.cshtml* with the following code:</span></span>

  [!code-cshtml[Index](signalr/sample-snapshot/3.x/Index.cshtml)]

  <span data-ttu-id="30475-179">Powyższy kod:</span><span class="sxs-lookup"><span data-stu-id="30475-179">The preceding code:</span></span>

  * <span data-ttu-id="30475-180">Tworzy pola tekstowe nazwy i tekst wiadomość i przycisk Prześlij.</span><span class="sxs-lookup"><span data-stu-id="30475-180">Creates text boxes for name and message text, and a submit button.</span></span>
  * <span data-ttu-id="30475-181">Tworzy listę z `id="messagesList"` do wyświetlania komunikatów odebranych z Centrum SignalR.</span><span class="sxs-lookup"><span data-stu-id="30475-181">Creates a list with `id="messagesList"` for displaying messages that are received from the SignalR hub.</span></span>
  * <span data-ttu-id="30475-182">Zawiera odwołania do skryptu z SignalR i *chat.js* kod aplikacji, który zostanie utworzony w następnym kroku.</span><span class="sxs-lookup"><span data-stu-id="30475-182">Includes script references to SignalR and the *chat.js* application code that you create in the next step.</span></span>

* <span data-ttu-id="30475-183">W *wwwroot/js* folderze utwórz *chat.js* pliku następującym kodem:</span><span class="sxs-lookup"><span data-stu-id="30475-183">In the *wwwroot/js* folder, create a *chat.js* file with the following code:</span></span>

  [!code-javascript[Index](signalr/sample-snapshot/3.x/chat.js)]

  <span data-ttu-id="30475-184">Powyższy kod:</span><span class="sxs-lookup"><span data-stu-id="30475-184">The preceding code:</span></span>

  * <span data-ttu-id="30475-185">Tworzy i uruchamia połączenie.</span><span class="sxs-lookup"><span data-stu-id="30475-185">Creates and starts a connection.</span></span>
  * <span data-ttu-id="30475-186">Dodaje przycisk przesyłania program obsługi, która wysyła komunikaty do Centrum.</span><span class="sxs-lookup"><span data-stu-id="30475-186">Adds to the submit button a handler that sends messages to the hub.</span></span>
  * <span data-ttu-id="30475-187">Dodaje do obiektu połączenia program obsługi, który odbiera komunikaty z Centrum i dodaje je do listy.</span><span class="sxs-lookup"><span data-stu-id="30475-187">Adds to the connection object a handler that receives messages from the hub and adds them to the list.</span></span>

## <a name="run-the-app"></a><span data-ttu-id="30475-188">Uruchamianie aplikacji</span><span class="sxs-lookup"><span data-stu-id="30475-188">Run the app</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="30475-189">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="30475-189">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="30475-190">Naciśnij klawisz **kombinację klawiszy CTRL + F5** Aby uruchomić aplikację bez debugowania.</span><span class="sxs-lookup"><span data-stu-id="30475-190">Press **CTRL+F5** to run the app without debugging.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="30475-191">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="30475-191">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="30475-192">W zintegrowanym terminalu uruchom następujące polecenie:</span><span class="sxs-lookup"><span data-stu-id="30475-192">In the integrated terminal, run the following command:</span></span>

  ```console
  dotnet run -p SignalRChat.csproj
  ```

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="30475-193">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="30475-193">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="30475-194">W menu, wybierz opcję **Uruchom > Uruchom bez debugowania**.</span><span class="sxs-lookup"><span data-stu-id="30475-194">From the menu, select **Run > Start Without Debugging**.</span></span>

---

* <span data-ttu-id="30475-195">Skopiuj adres URL z paska adresu, a następnie otwórz innego wystąpienia przeglądarki lub karty i wklej adres URL w pasku adresu.</span><span class="sxs-lookup"><span data-stu-id="30475-195">Copy the URL from the address bar, open another browser instance or tab, and paste the URL in the address bar.</span></span>

* <span data-ttu-id="30475-196">Wybierz albo przeglądarki, wprowadź nazwę i komunikat, a następnie wybierz **wysyłania komunikatu** przycisku.</span><span class="sxs-lookup"><span data-stu-id="30475-196">Choose either browser, enter a name and message, and select the **Send Message** button.</span></span>

  <span data-ttu-id="30475-197">Nazwa i wiadomości są wyświetlane na obu stronach natychmiast.</span><span class="sxs-lookup"><span data-stu-id="30475-197">The name and message are displayed on both pages instantly.</span></span>

  ![SignalR przykładowej aplikacji](signalr/_static/3.x/signalr-get-started-finished.png)

> [!TIP]
> * <span data-ttu-id="30475-199">Jeśli aplikacja nie działa, Otwórz swoje narzędzia deweloperskie przeglądarki (F12) i przejdź do konsoli.</span><span class="sxs-lookup"><span data-stu-id="30475-199">If the app doesn't work, open your browser developer tools (F12) and go to the console.</span></span> <span data-ttu-id="30475-200">Może pojawić się błędy związane z Twoim kodem HTML i JavaScript.</span><span class="sxs-lookup"><span data-stu-id="30475-200">You might see errors related to your HTML and JavaScript code.</span></span> <span data-ttu-id="30475-201">Załóżmy, że możesz umieścić *signalr.js* w innym folderze niż bezpośredniego.</span><span class="sxs-lookup"><span data-stu-id="30475-201">For example, suppose you put *signalr.js* in a different folder than directed.</span></span> <span data-ttu-id="30475-202">W takim przypadku odwołania do tego pliku nie będzie działać, a następnie zostanie wyświetlony błąd 404 w konsoli.</span><span class="sxs-lookup"><span data-stu-id="30475-202">In that case the reference to that file won't work and you'll see a 404 error in the console.</span></span>
>   <span data-ttu-id="30475-203">![błąd — nie znaleziono signalr.js](signalr/_static/3.x/f12-console.png)</span><span class="sxs-lookup"><span data-stu-id="30475-203">![signalr.js not found error](signalr/_static/3.x/f12-console.png)</span></span>
> * <span data-ttu-id="30475-204">Jeśli wystąpi błąd, ERR_SPDY_INADEQUATE_TRANSPORT_SECURITY w przeglądarce Chrome lub NS_ERROR_NET_INADEQUATE_SECURITY w przeglądarce Firefox, uruchom następujące polecenia, aby zaktualizować certyfikat deweloperski:</span><span class="sxs-lookup"><span data-stu-id="30475-204">If you get the error ERR_SPDY_INADEQUATE_TRANSPORT_SECURITY in Chrome or NS_ERROR_NET_INADEQUATE_SECURITY in Firefox, run these commands to update your development certificate:</span></span>
>   ```
>   dotnet dev-certs https --clean
>   dotnet dev-certs https --trust
>   ```

## <a name="next-steps"></a><span data-ttu-id="30475-205">Następne kroki</span><span class="sxs-lookup"><span data-stu-id="30475-205">Next steps</span></span>

<span data-ttu-id="30475-206">Aby dowiedzieć się więcej na temat biblioteki SignalR, zobacz wprowadzenie:</span><span class="sxs-lookup"><span data-stu-id="30475-206">To learn more about SignalR, see the introduction:</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="30475-207">Wprowadzenie do SignalR platformy ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="30475-207">Introduction to ASP.NET Core SignalR</span></span>](xref:signalr/introduction)

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="30475-208">W tym samouczku pokazano podstawowe informacje dotyczące tworzenia aplikacji w czasie rzeczywistym przy użyciu biblioteki SignalR.</span><span class="sxs-lookup"><span data-stu-id="30475-208">This tutorial teaches the basics of building a real-time app using SignalR.</span></span> <span data-ttu-id="30475-209">Dowiesz się, jak:</span><span class="sxs-lookup"><span data-stu-id="30475-209">You learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="30475-210">Utwórz projekt sieci web.</span><span class="sxs-lookup"><span data-stu-id="30475-210">Create a web project.</span></span>
> * <span data-ttu-id="30475-211">Dodaj bibliotekę klienta SignalR.</span><span class="sxs-lookup"><span data-stu-id="30475-211">Add the SignalR client library.</span></span>
> * <span data-ttu-id="30475-212">Utwórz Centrum SignalR.</span><span class="sxs-lookup"><span data-stu-id="30475-212">Create a SignalR hub.</span></span>
> * <span data-ttu-id="30475-213">Skonfiguruj projekt do korzystania z SignalR.</span><span class="sxs-lookup"><span data-stu-id="30475-213">Configure the project to use SignalR.</span></span>
> * <span data-ttu-id="30475-214">Dodaj kod, który wysyła komunikaty z dowolnego klienta do wszystkich połączonych klientów.</span><span class="sxs-lookup"><span data-stu-id="30475-214">Add code that sends messages from any client to all connected clients.</span></span>

<span data-ttu-id="30475-215">Po zakończeniu będziesz mieć działającą aplikację rozmowy:</span><span class="sxs-lookup"><span data-stu-id="30475-215">At the end, you'll have a working chat app:</span></span>

![SignalR przykładowej aplikacji](signalr/_static/2.x/signalr-get-started-finished.png)

## <a name="prerequisites"></a><span data-ttu-id="30475-217">Wymagania wstępne</span><span class="sxs-lookup"><span data-stu-id="30475-217">Prerequisites</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="30475-218">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="30475-218">Visual Studio</span></span>](#tab/visual-studio)

[!INCLUDE[](~/includes/net-core-prereqs-vs2017-2.2.md)]

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="30475-219">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="30475-219">Visual Studio Code</span></span>](#tab/visual-studio-code)

[!INCLUDE[](~/includes/net-core-prereqs-vsc-2.2.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="30475-220">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="30475-220">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

[!INCLUDE[](~/includes/net-core-prereqs-mac-2.2.md)]

---

## <a name="create-a-web-project"></a><span data-ttu-id="30475-221">Tworzenie projektu sieci web</span><span class="sxs-lookup"><span data-stu-id="30475-221">Create a web project</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="30475-222">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="30475-222">Visual Studio</span></span>](#tab/visual-studio/)

* <span data-ttu-id="30475-223">W menu, wybierz opcję **Plik > Nowy projekt**.</span><span class="sxs-lookup"><span data-stu-id="30475-223">From the menu, select **File > New Project**.</span></span>

* <span data-ttu-id="30475-224">W **nowy projekt** okno dialogowe, wybierz opcję **zainstalowane > Visual C# > sieci Web > Aplikacja sieci Web programu ASP.NET Core**.</span><span class="sxs-lookup"><span data-stu-id="30475-224">In the **New Project** dialog, select **Installed > Visual C# > Web > ASP.NET Core Web Application**.</span></span> <span data-ttu-id="30475-225">Nadaj projektowi nazwę *SignalRChat*.</span><span class="sxs-lookup"><span data-stu-id="30475-225">Name the project *SignalRChat*.</span></span>

  ![Okno dialogowe nowego projektu w programie Visual Studio](signalr/_static/2.x/signalr-new-project-dialog.png)

* <span data-ttu-id="30475-227">Wybierz **aplikacji sieci Web** do tworzenia projektu, który używa stron Razor.</span><span class="sxs-lookup"><span data-stu-id="30475-227">Select **Web Application** to create a project that uses Razor Pages.</span></span>

* <span data-ttu-id="30475-228">Wybierz docelową platformę **platformy .NET Core**, wybierz opcję **platformy ASP.NET Core 2.2**i kliknij przycisk **OK**.</span><span class="sxs-lookup"><span data-stu-id="30475-228">Select a target framework of **.NET Core**, select **ASP.NET Core 2.2**, and click **OK**.</span></span>

  ![Okno dialogowe nowego projektu w programie Visual Studio](signalr/_static/2.x/signalr-new-project-choose-type.png)

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="30475-230">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="30475-230">Visual Studio Code</span></span>](#tab/visual-studio-code/)

* <span data-ttu-id="30475-231">Otwórz [zintegrowany terminal](https://code.visualstudio.com/docs/editor/integrated-terminal) do folderu, w którym zostanie utworzony nowy folder projektu.</span><span class="sxs-lookup"><span data-stu-id="30475-231">Open the [integrated terminal](https://code.visualstudio.com/docs/editor/integrated-terminal) to the folder in which the new project folder will be created.</span></span>

* <span data-ttu-id="30475-232">Uruchom następujące polecenia:</span><span class="sxs-lookup"><span data-stu-id="30475-232">Run the following commands:</span></span>

   ```console
   dotnet new webapp -o SignalRChat
   code -r SignalRChat
   ```

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="30475-233">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="30475-233">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="30475-234">W menu, wybierz opcję **Plik > nowe rozwiązanie**.</span><span class="sxs-lookup"><span data-stu-id="30475-234">From the menu, select **File > New Solution**.</span></span>

* <span data-ttu-id="30475-235">Wybierz **platformy .NET Core > aplikacji > aplikacji internetowej ASP.NET Core** (nie wybieraj **ASP.NET Core Web App (MVC)** ).</span><span class="sxs-lookup"><span data-stu-id="30475-235">Select **.NET Core > App > ASP.NET Core Web App** (Don't select **ASP.NET Core Web App (MVC)**).</span></span>

* <span data-ttu-id="30475-236">Wybierz opcję **Dalej**.</span><span class="sxs-lookup"><span data-stu-id="30475-236">Select **Next**.</span></span>

* <span data-ttu-id="30475-237">Nadaj projektowi nazwę *SignalRChat*, a następnie wybierz pozycję **Utwórz**.</span><span class="sxs-lookup"><span data-stu-id="30475-237">Name the project *SignalRChat*, and then select **Create**.</span></span>

---

## <a name="add-the-signalr-client-library"></a><span data-ttu-id="30475-238">Dodaj bibliotekę klienta SignalR</span><span class="sxs-lookup"><span data-stu-id="30475-238">Add the SignalR client library</span></span>

<span data-ttu-id="30475-239">Serwer biblioteki SignalR znajduje się w `Microsoft.AspNetCore.App` meta Microsoft.aspnetcore.all.</span><span class="sxs-lookup"><span data-stu-id="30475-239">The SignalR server library is included in the `Microsoft.AspNetCore.App` metapackage.</span></span> <span data-ttu-id="30475-240">Biblioteki klienta JavaScript automatycznie nie jest zawarty w projekcie.</span><span class="sxs-lookup"><span data-stu-id="30475-240">The JavaScript client library isn't automatically included in the project.</span></span> <span data-ttu-id="30475-241">W tym samouczku używasz Library Manager (LibMan) można pobrać z biblioteki klienta z *unpkg*.</span><span class="sxs-lookup"><span data-stu-id="30475-241">For this tutorial, you use Library Manager (LibMan) to get the client library from *unpkg*.</span></span> <span data-ttu-id="30475-242">unpkg jest usługa content delivery network (CDN)), można dostarczać niczego w Menedżer pakietów npm oraz Menedżera pakietów środowiska Node.js.</span><span class="sxs-lookup"><span data-stu-id="30475-242">unpkg is a content delivery network (CDN)) that can deliver anything found in npm, the Node.js package manager.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="30475-243">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="30475-243">Visual Studio</span></span>](#tab/visual-studio/)

* <span data-ttu-id="30475-244">W **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy projekt i wybierz **Dodaj** > **biblioteki po stronie klienta**.</span><span class="sxs-lookup"><span data-stu-id="30475-244">In **Solution Explorer**, right-click the project, and select **Add** > **Client-Side Library**.</span></span>

* <span data-ttu-id="30475-245">W **Dodaj biblioteki po stronie klienta** okno dialogowe, aby uzyskać **dostawcy** wybierz **unpkg**.</span><span class="sxs-lookup"><span data-stu-id="30475-245">In the **Add Client-Side Library** dialog, for **Provider** select **unpkg**.</span></span>

* <span data-ttu-id="30475-246">Aby uzyskać **biblioteki**, wprowadź `@aspnet/signalr@1`i wybierz najnowszą wersję, która nie jest w wersji zapoznawczej.</span><span class="sxs-lookup"><span data-stu-id="30475-246">For **Library**, enter `@aspnet/signalr@1`, and select the latest version that isn't preview.</span></span>

  ![Dodaj okno dialogowe biblioteki po stronie klienta — Wybieranie biblioteki](signalr/_static/2.x/libman1.png)

* <span data-ttu-id="30475-248">Wybierz **wybierz konkretne pliki**, rozwiń węzeł *dist/przeglądarki* folder, a następnie wybierz *signalr.js* i *signalr.min.js*.</span><span class="sxs-lookup"><span data-stu-id="30475-248">Select **Choose specific files**, expand the *dist/browser* folder, and select *signalr.js* and *signalr.min.js*.</span></span>

* <span data-ttu-id="30475-249">Ustaw **lokalizacji docelowej** do *wwwroot/lib/signalr/* i wybierz **zainstalować**.</span><span class="sxs-lookup"><span data-stu-id="30475-249">Set **Target Location** to *wwwroot/lib/signalr/*, and select **Install**.</span></span>

  ![Dodaj okno dialogowe z biblioteki klienta - wybranie plików i miejsce docelowe](signalr/_static/2.x/libman2.png)

  <span data-ttu-id="30475-251">Tworzy LibMan *wwwroot/lib/signalr* folder i kopiuje wybrane pliki.</span><span class="sxs-lookup"><span data-stu-id="30475-251">LibMan creates a *wwwroot/lib/signalr* folder and copies the selected files to it.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="30475-252">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="30475-252">Visual Studio Code</span></span>](#tab/visual-studio-code/)

* <span data-ttu-id="30475-253">W zintegrowanym terminalu uruchom następujące polecenie, aby zainstalować LibMan.</span><span class="sxs-lookup"><span data-stu-id="30475-253">In the integrated terminal, run the following command to install LibMan.</span></span>

  ```console
  dotnet tool install -g Microsoft.Web.LibraryManager.Cli
  ```

* <span data-ttu-id="30475-254">Uruchom następujące polecenie, aby przy użyciu LibMan uzyskać biblioteki klienta SignalR.</span><span class="sxs-lookup"><span data-stu-id="30475-254">Run the following command to get the SignalR client library by using LibMan.</span></span> <span data-ttu-id="30475-255">Być może musisz odczekaj kilka sekund, zanim dane wyjściowe.</span><span class="sxs-lookup"><span data-stu-id="30475-255">You might have to wait a few seconds before seeing output.</span></span>

  ```console
  libman install @aspnet/signalr -p unpkg -d wwwroot/lib/signalr --files dist/browser/signalr.js --files dist/browser/signalr.min.js
  ```

  <span data-ttu-id="30475-256">Parametry określ następujące opcje:</span><span class="sxs-lookup"><span data-stu-id="30475-256">The parameters specify the following options:</span></span>
  * <span data-ttu-id="30475-257">Użyj dostawcy unpkg.</span><span class="sxs-lookup"><span data-stu-id="30475-257">Use the unpkg provider.</span></span>
  * <span data-ttu-id="30475-258">Skopiuj pliki do *wwwroot/lib/signalr* docelowego.</span><span class="sxs-lookup"><span data-stu-id="30475-258">Copy files to the *wwwroot/lib/signalr* destination.</span></span>
  * <span data-ttu-id="30475-259">Skopiuj tylko określonych plików.</span><span class="sxs-lookup"><span data-stu-id="30475-259">Copy only the specified files.</span></span>

  <span data-ttu-id="30475-260">Dane wyjściowe wyglądają jak w poniższym przykładzie:</span><span class="sxs-lookup"><span data-stu-id="30475-260">The output looks like the following example:</span></span>

  ```console
  wwwroot/lib/signalr/dist/browser/signalr.js written to disk
  wwwroot/lib/signalr/dist/browser/signalr.min.js written to disk
  Installed library "@aspnet/signalr@1.0.3" to "wwwroot/lib/signalr"
  ```

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="30475-261">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="30475-261">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="30475-262">W **terminalu**, uruchom następujące polecenie, aby zainstalować LibMan.</span><span class="sxs-lookup"><span data-stu-id="30475-262">In the **Terminal**, run the following command to install LibMan.</span></span>

  ```console
  dotnet tool install -g Microsoft.Web.LibraryManager.Cli
  ```

* <span data-ttu-id="30475-263">Przejdź do folderu projektu (zawierającego *SignalRChat.csproj* pliku).</span><span class="sxs-lookup"><span data-stu-id="30475-263">Navigate to the project folder (the one that contains the *SignalRChat.csproj* file).</span></span>

* <span data-ttu-id="30475-264">Uruchom następujące polecenie, aby przy użyciu LibMan uzyskać biblioteki klienta SignalR.</span><span class="sxs-lookup"><span data-stu-id="30475-264">Run the following command to get the SignalR client library by using LibMan.</span></span>

  ```console
  libman install @aspnet/signalr -p unpkg -d wwwroot/lib/signalr --files dist/browser/signalr.js --files dist/browser/signalr.min.js
  ```

  <span data-ttu-id="30475-265">Parametry określ następujące opcje:</span><span class="sxs-lookup"><span data-stu-id="30475-265">The parameters specify the following options:</span></span>
  * <span data-ttu-id="30475-266">Użyj dostawcy unpkg.</span><span class="sxs-lookup"><span data-stu-id="30475-266">Use the unpkg provider.</span></span>
  * <span data-ttu-id="30475-267">Skopiuj pliki do *wwwroot/lib/signalr* docelowego.</span><span class="sxs-lookup"><span data-stu-id="30475-267">Copy files to the *wwwroot/lib/signalr* destination.</span></span>
  * <span data-ttu-id="30475-268">Skopiuj tylko określonych plików.</span><span class="sxs-lookup"><span data-stu-id="30475-268">Copy only the specified files.</span></span>

  <span data-ttu-id="30475-269">Dane wyjściowe wyglądają jak w poniższym przykładzie:</span><span class="sxs-lookup"><span data-stu-id="30475-269">The output looks like the following example:</span></span>

  ```console
  wwwroot/lib/signalr/dist/browser/signalr.js written to disk
  wwwroot/lib/signalr/dist/browser/signalr.min.js written to disk
  Installed library "@aspnet/signalr@1.0.3" to "wwwroot/lib/signalr"
  ```

---

## <a name="create-a-signalr-hub"></a><span data-ttu-id="30475-270">Tworzenie Centrum SignalR</span><span class="sxs-lookup"><span data-stu-id="30475-270">Create a SignalR hub</span></span>

<span data-ttu-id="30475-271">A *Centrum* jest klasa, która służy jako ogólny potok, który obsługuje komunikację klient serwer.</span><span class="sxs-lookup"><span data-stu-id="30475-271">A *hub* is a class that serves as a high-level pipeline that handles client-server communication.</span></span>

* <span data-ttu-id="30475-272">W folderze projektu SignalRChat Utwórz *koncentratory* folderu.</span><span class="sxs-lookup"><span data-stu-id="30475-272">In the SignalRChat project folder, create a *Hubs* folder.</span></span>

* <span data-ttu-id="30475-273">W *Hubs* folderze utwórz *ChatHub.cs* pliku następującym kodem:</span><span class="sxs-lookup"><span data-stu-id="30475-273">In the *Hubs* folder, create a *ChatHub.cs* file with the following code:</span></span>

  [!code-csharp[Startup](signalr/sample-snapshot/2.x/ChatHub.cs)]

  <span data-ttu-id="30475-274">`ChatHub` Klasa dziedziczy z elementu SignalR `Hub` klasy.</span><span class="sxs-lookup"><span data-stu-id="30475-274">The `ChatHub` class inherits from the SignalR `Hub` class.</span></span> <span data-ttu-id="30475-275">`Hub` Klasa zarządza połączeń, grup i komunikatów.</span><span class="sxs-lookup"><span data-stu-id="30475-275">The `Hub` class manages connections, groups, and messaging.</span></span>

  <span data-ttu-id="30475-276">`SendMessage` Metoda może być wywoływana przez połączone klienta, aby wysłać wiadomość do wszystkich klientów.</span><span class="sxs-lookup"><span data-stu-id="30475-276">The `SendMessage` method can be called by a connected client to send a message to all clients.</span></span> <span data-ttu-id="30475-277">Kod klienta JavaScript, który wywołuje metodę przedstawiono w dalszej części tego samouczka.</span><span class="sxs-lookup"><span data-stu-id="30475-277">JavaScript client code that calls the method is shown later in the tutorial.</span></span> <span data-ttu-id="30475-278">Kod SignalR jest asynchroniczne w celu zapewnienia maksymalnej skalowalności.</span><span class="sxs-lookup"><span data-stu-id="30475-278">SignalR code is asynchronous to provide maximum scalability.</span></span>

## <a name="configure-signalr"></a><span data-ttu-id="30475-279">Konfigurowanie biblioteki SignalR</span><span class="sxs-lookup"><span data-stu-id="30475-279">Configure SignalR</span></span>

<span data-ttu-id="30475-280">Serwer biblioteki SignalR musi być skonfigurowany do przekazywania żądań SignalR z SignalR.</span><span class="sxs-lookup"><span data-stu-id="30475-280">The SignalR server must be configured to pass SignalR requests to SignalR.</span></span>

* <span data-ttu-id="30475-281">Dodaj następujący wyróżniony kod do *Startup.cs* pliku.</span><span class="sxs-lookup"><span data-stu-id="30475-281">Add the following highlighted code to the *Startup.cs* file.</span></span>

  [!code-csharp[Startup](signalr/sample-snapshot/2.x/Startup.cs?highlight=7,33,52-55)]

  <span data-ttu-id="30475-282">Te zmiany dodanie SignalR systemu iniekcji zależności platformy ASP.NET Core i potoku oprogramowania pośredniczącego.</span><span class="sxs-lookup"><span data-stu-id="30475-282">These changes add SignalR to the ASP.NET Core dependency injection system and the middleware pipeline.</span></span>

## <a name="add-signalr-client-code"></a><span data-ttu-id="30475-283">Dodaj kod klienta SignalR</span><span class="sxs-lookup"><span data-stu-id="30475-283">Add SignalR client code</span></span>

* <span data-ttu-id="30475-284">Zastąp zawartość *Pages\Index.cshtml* następującym kodem:</span><span class="sxs-lookup"><span data-stu-id="30475-284">Replace the content in *Pages\Index.cshtml* with the following code:</span></span>

  [!code-cshtml[Index](signalr/sample-snapshot/2.x/Index.cshtml)]

  <span data-ttu-id="30475-285">Powyższy kod:</span><span class="sxs-lookup"><span data-stu-id="30475-285">The preceding code:</span></span>

  * <span data-ttu-id="30475-286">Tworzy pola tekstowe nazwy i tekst wiadomość i przycisk Prześlij.</span><span class="sxs-lookup"><span data-stu-id="30475-286">Creates text boxes for name and message text, and a submit button.</span></span>
  * <span data-ttu-id="30475-287">Tworzy listę z `id="messagesList"` do wyświetlania komunikatów odebranych z Centrum SignalR.</span><span class="sxs-lookup"><span data-stu-id="30475-287">Creates a list with `id="messagesList"` for displaying messages that are received from the SignalR hub.</span></span>
  * <span data-ttu-id="30475-288">Zawiera odwołania do skryptu z SignalR i *chat.js* kod aplikacji, który zostanie utworzony w następnym kroku.</span><span class="sxs-lookup"><span data-stu-id="30475-288">Includes script references to SignalR and the *chat.js* application code that you create in the next step.</span></span>

* <span data-ttu-id="30475-289">W *wwwroot/js* folderze utwórz *chat.js* pliku następującym kodem:</span><span class="sxs-lookup"><span data-stu-id="30475-289">In the *wwwroot/js* folder, create a *chat.js* file with the following code:</span></span>

  [!code-javascript[Index](signalr/sample-snapshot/2.x/chat.js)]

  <span data-ttu-id="30475-290">Powyższy kod:</span><span class="sxs-lookup"><span data-stu-id="30475-290">The preceding code:</span></span>

  * <span data-ttu-id="30475-291">Tworzy i uruchamia połączenie.</span><span class="sxs-lookup"><span data-stu-id="30475-291">Creates and starts a connection.</span></span>
  * <span data-ttu-id="30475-292">Dodaje przycisk przesyłania program obsługi, która wysyła komunikaty do Centrum.</span><span class="sxs-lookup"><span data-stu-id="30475-292">Adds to the submit button a handler that sends messages to the hub.</span></span>
  * <span data-ttu-id="30475-293">Dodaje do obiektu połączenia program obsługi, który odbiera komunikaty z Centrum i dodaje je do listy.</span><span class="sxs-lookup"><span data-stu-id="30475-293">Adds to the connection object a handler that receives messages from the hub and adds them to the list.</span></span>

## <a name="run-the-app"></a><span data-ttu-id="30475-294">Uruchamianie aplikacji</span><span class="sxs-lookup"><span data-stu-id="30475-294">Run the app</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="30475-295">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="30475-295">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="30475-296">Naciśnij klawisz **kombinację klawiszy CTRL + F5** Aby uruchomić aplikację bez debugowania.</span><span class="sxs-lookup"><span data-stu-id="30475-296">Press **CTRL+F5** to run the app without debugging.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="30475-297">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="30475-297">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="30475-298">W zintegrowanym terminalu uruchom następujące polecenie:</span><span class="sxs-lookup"><span data-stu-id="30475-298">In the integrated terminal, run the following command:</span></span>

  ```console
  dotnet run -p SignalRChat.csproj
  ```

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="30475-299">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="30475-299">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="30475-300">W menu, wybierz opcję **Uruchom > Uruchom bez debugowania**.</span><span class="sxs-lookup"><span data-stu-id="30475-300">From the menu, select **Run > Start Without Debugging**.</span></span>

---

* <span data-ttu-id="30475-301">Skopiuj adres URL z paska adresu, a następnie otwórz innego wystąpienia przeglądarki lub karty i wklej adres URL w pasku adresu.</span><span class="sxs-lookup"><span data-stu-id="30475-301">Copy the URL from the address bar, open another browser instance or tab, and paste the URL in the address bar.</span></span>

* <span data-ttu-id="30475-302">Wybierz albo przeglądarki, wprowadź nazwę i komunikat, a następnie wybierz **wysyłania komunikatu** przycisku.</span><span class="sxs-lookup"><span data-stu-id="30475-302">Choose either browser, enter a name and message, and select the **Send Message** button.</span></span>

  <span data-ttu-id="30475-303">Nazwa i wiadomości są wyświetlane na obu stronach natychmiast.</span><span class="sxs-lookup"><span data-stu-id="30475-303">The name and message are displayed on both pages instantly.</span></span>

  ![SignalR przykładowej aplikacji](signalr/_static/2.x/signalr-get-started-finished.png)

> [!TIP]
> <span data-ttu-id="30475-305">Jeśli aplikacja nie działa, Otwórz swoje narzędzia deweloperskie przeglądarki (F12) i przejdź do konsoli.</span><span class="sxs-lookup"><span data-stu-id="30475-305">If the app doesn't work, open your browser developer tools (F12) and go to the console.</span></span> <span data-ttu-id="30475-306">Może pojawić się błędy związane z Twoim kodem HTML i JavaScript.</span><span class="sxs-lookup"><span data-stu-id="30475-306">You might see errors related to your HTML and JavaScript code.</span></span> <span data-ttu-id="30475-307">Załóżmy, że możesz umieścić *signalr.js* w innym folderze niż bezpośredniego.</span><span class="sxs-lookup"><span data-stu-id="30475-307">For example, suppose you put *signalr.js* in a different folder than directed.</span></span> <span data-ttu-id="30475-308">W takim przypadku odwołania do tego pliku nie będzie działać, a następnie zostanie wyświetlony błąd 404 w konsoli.</span><span class="sxs-lookup"><span data-stu-id="30475-308">In that case the reference to that file won't work and you'll see a 404 error in the console.</span></span>
> <span data-ttu-id="30475-309">![błąd — nie znaleziono signalr.js](signalr/_static/2.x/f12-console.png)</span><span class="sxs-lookup"><span data-stu-id="30475-309">![signalr.js not found error](signalr/_static/2.x/f12-console.png)</span></span>

## <a name="next-steps"></a><span data-ttu-id="30475-310">Następne kroki</span><span class="sxs-lookup"><span data-stu-id="30475-310">Next steps</span></span>

<span data-ttu-id="30475-311">W niniejszym samouczku zawarto informacje na temat wykonywania następujących czynności:</span><span class="sxs-lookup"><span data-stu-id="30475-311">In this tutorial, you learned how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="30475-312">Tworzenie projektu aplikacji sieci web.</span><span class="sxs-lookup"><span data-stu-id="30475-312">Create a web app project.</span></span>
> * <span data-ttu-id="30475-313">Dodaj bibliotekę klienta SignalR.</span><span class="sxs-lookup"><span data-stu-id="30475-313">Add the SignalR client library.</span></span>
> * <span data-ttu-id="30475-314">Utwórz Centrum SignalR.</span><span class="sxs-lookup"><span data-stu-id="30475-314">Create a SignalR hub.</span></span>
> * <span data-ttu-id="30475-315">Skonfiguruj projekt do korzystania z SignalR.</span><span class="sxs-lookup"><span data-stu-id="30475-315">Configure the project to use SignalR.</span></span>
> * <span data-ttu-id="30475-316">Dodaj kod używający koncentratora, aby wysyłać komunikaty z dowolnego klienta do wszystkich połączonych klientów.</span><span class="sxs-lookup"><span data-stu-id="30475-316">Add code that uses the hub to send messages from any client to all connected clients.</span></span>

## <a name="next-steps"></a><span data-ttu-id="30475-317">Następne kroki</span><span class="sxs-lookup"><span data-stu-id="30475-317">Next steps</span></span>

<span data-ttu-id="30475-318">Aby dowiedzieć się więcej na temat biblioteki SignalR, zobacz wprowadzenie:</span><span class="sxs-lookup"><span data-stu-id="30475-318">To learn more about SignalR, see the introduction:</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="30475-319">Wprowadzenie do SignalR platformy ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="30475-319">Introduction to ASP.NET Core SignalR</span></span>](xref:signalr/introduction)

::: moniker-end