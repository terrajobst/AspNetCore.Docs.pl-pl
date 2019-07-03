---
title: Wprowadzenie do biblioteki SignalR platformy ASP.NET Core
author: bradygaster
description: W tym samouczku utworzysz aplikację rozmowy, która korzysta z biblioteki SignalR platformy ASP.NET Core.
ms.author: bradyg
ms.custom: mvc
ms.date: 11/30/2018
uid: tutorials/signalr
ms.openlocfilehash: 9a4296550a17ac2c348f2406e9f5b39877b02b59
ms.sourcegitcommit: d6e51c60439f03a8992bda70cc982ddb15d3f100
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/03/2019
ms.locfileid: "67555926"
---
# <a name="tutorial-get-started-with-aspnet-core-signalr"></a><span data-ttu-id="d8890-103">Samouczek: Wprowadzenie do biblioteki SignalR platformy ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="d8890-103">Tutorial: Get started with ASP.NET Core SignalR</span></span>

<span data-ttu-id="d8890-104">W tym samouczku pokazano podstawowe informacje dotyczące tworzenia aplikacji w czasie rzeczywistym przy użyciu biblioteki SignalR.</span><span class="sxs-lookup"><span data-stu-id="d8890-104">This tutorial teaches the basics of building a real-time app using SignalR.</span></span> <span data-ttu-id="d8890-105">Dowiesz się, jak:</span><span class="sxs-lookup"><span data-stu-id="d8890-105">You learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="d8890-106">Utwórz projekt sieci web.</span><span class="sxs-lookup"><span data-stu-id="d8890-106">Create a web project.</span></span>
> * <span data-ttu-id="d8890-107">Dodaj bibliotekę klienta SignalR.</span><span class="sxs-lookup"><span data-stu-id="d8890-107">Add the SignalR client library.</span></span>
> * <span data-ttu-id="d8890-108">Utwórz Centrum SignalR.</span><span class="sxs-lookup"><span data-stu-id="d8890-108">Create a SignalR hub.</span></span>
> * <span data-ttu-id="d8890-109">Skonfiguruj projekt do korzystania z SignalR.</span><span class="sxs-lookup"><span data-stu-id="d8890-109">Configure the project to use SignalR.</span></span>
> * <span data-ttu-id="d8890-110">Dodaj kod, który wysyła komunikaty z dowolnego klienta do wszystkich połączonych klientów.</span><span class="sxs-lookup"><span data-stu-id="d8890-110">Add code that sends messages from any client to all connected clients.</span></span>

<span data-ttu-id="d8890-111">Po zakończeniu będziesz mieć działającą aplikację rozmowy:</span><span class="sxs-lookup"><span data-stu-id="d8890-111">At the end, you'll have a working chat app:</span></span>

![SignalR przykładowej aplikacji](signalr/_static/signalr-get-started-finished.png)

<span data-ttu-id="d8890-113">[Wyświetlanie lub pobieranie przykładowego kodu](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/tutorials/signalr/sample) ([sposobu pobierania](xref:index#how-to-download-a-sample)).</span><span class="sxs-lookup"><span data-stu-id="d8890-113">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/tutorials/signalr/sample) ([how to download](xref:index#how-to-download-a-sample)).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="d8890-114">Wymagania wstępne</span><span class="sxs-lookup"><span data-stu-id="d8890-114">Prerequisites</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="d8890-115">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="d8890-115">Visual Studio</span></span>](#tab/visual-studio)

[!INCLUDE[](~/includes/net-core-prereqs-vs2017-2.2.md)]

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="d8890-116">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="d8890-116">Visual Studio Code</span></span>](#tab/visual-studio-code)

[!INCLUDE[](~/includes/net-core-prereqs-vsc-2.2.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="d8890-117">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="d8890-117">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

[!INCLUDE[](~/includes/net-core-prereqs-mac-2.2.md)]

---

## <a name="create-a-web-project"></a><span data-ttu-id="d8890-118">Tworzenie projektu sieci web</span><span class="sxs-lookup"><span data-stu-id="d8890-118">Create a web project</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="d8890-119">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="d8890-119">Visual Studio</span></span>](#tab/visual-studio/)

* <span data-ttu-id="d8890-120">W menu, wybierz opcję **Plik > Nowy projekt**.</span><span class="sxs-lookup"><span data-stu-id="d8890-120">From the menu, select **File > New Project**.</span></span>

* <span data-ttu-id="d8890-121">W **nowy projekt** okno dialogowe, wybierz opcję **zainstalowane > Visual C# > sieci Web > Aplikacja sieci Web programu ASP.NET Core**.</span><span class="sxs-lookup"><span data-stu-id="d8890-121">In the **New Project** dialog, select **Installed > Visual C# > Web > ASP.NET Core Web Application**.</span></span> <span data-ttu-id="d8890-122">Nadaj projektowi nazwę *SignalRChat*.</span><span class="sxs-lookup"><span data-stu-id="d8890-122">Name the project *SignalRChat*.</span></span>

  ![Okno dialogowe nowego projektu w programie Visual Studio](signalr/_static/signalr-new-project-dialog.png)

* <span data-ttu-id="d8890-124">Wybierz **aplikacji sieci Web** do tworzenia projektu, który używa stron Razor.</span><span class="sxs-lookup"><span data-stu-id="d8890-124">Select **Web Application** to create a project that uses Razor Pages.</span></span>

* <span data-ttu-id="d8890-125">Wybierz docelową platformę **platformy .NET Core**, wybierz opcję **platformy ASP.NET Core 2.2**i kliknij przycisk **OK**.</span><span class="sxs-lookup"><span data-stu-id="d8890-125">Select a target framework of **.NET Core**, select **ASP.NET Core 2.2**, and click **OK**.</span></span>

  ![Okno dialogowe nowego projektu w programie Visual Studio](signalr/_static/signalr-new-project-choose-type.png)

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="d8890-127">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="d8890-127">Visual Studio Code</span></span>](#tab/visual-studio-code/)

* <span data-ttu-id="d8890-128">Otwórz [zintegrowany terminal](https://code.visualstudio.com/docs/editor/integrated-terminal) do folderu, w którym zostanie utworzony nowy folder projektu.</span><span class="sxs-lookup"><span data-stu-id="d8890-128">Open the [integrated terminal](https://code.visualstudio.com/docs/editor/integrated-terminal) to the folder in which the new project folder will be created.</span></span>

* <span data-ttu-id="d8890-129">Uruchom następujące polecenia:</span><span class="sxs-lookup"><span data-stu-id="d8890-129">Run the following commands:</span></span>

   ```console
   dotnet new webapp -o SignalRChat
   code -r SignalRChat
   ```

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="d8890-130">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="d8890-130">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="d8890-131">W menu, wybierz opcję **Plik > nowe rozwiązanie**.</span><span class="sxs-lookup"><span data-stu-id="d8890-131">From the menu, select **File > New Solution**.</span></span>

* <span data-ttu-id="d8890-132">Wybierz **platformy .NET Core > aplikacji > aplikacji internetowej ASP.NET Core** (nie wybieraj **ASP.NET Core Web App (MVC)** ).</span><span class="sxs-lookup"><span data-stu-id="d8890-132">Select **.NET Core > App > ASP.NET Core Web App** (Don't select **ASP.NET Core Web App (MVC)**).</span></span>

* <span data-ttu-id="d8890-133">Wybierz opcję **Dalej**.</span><span class="sxs-lookup"><span data-stu-id="d8890-133">Select **Next**.</span></span>

* <span data-ttu-id="d8890-134">Nadaj projektowi nazwę *SignalRChat*, a następnie wybierz pozycję **Utwórz**.</span><span class="sxs-lookup"><span data-stu-id="d8890-134">Name the project *SignalRChat*, and then select **Create**.</span></span>

---

## <a name="add-the-signalr-client-library"></a><span data-ttu-id="d8890-135">Dodaj bibliotekę klienta SignalR</span><span class="sxs-lookup"><span data-stu-id="d8890-135">Add the SignalR client library</span></span>

<span data-ttu-id="d8890-136">Serwer biblioteki SignalR znajduje się w `Microsoft.AspNetCore.App` meta Microsoft.aspnetcore.all.</span><span class="sxs-lookup"><span data-stu-id="d8890-136">The SignalR server library is included in the `Microsoft.AspNetCore.App` metapackage.</span></span> <span data-ttu-id="d8890-137">Biblioteki klienta JavaScript automatycznie nie jest zawarty w projekcie.</span><span class="sxs-lookup"><span data-stu-id="d8890-137">The JavaScript client library isn't automatically included in the project.</span></span> <span data-ttu-id="d8890-138">W tym samouczku używasz Library Manager (LibMan) można pobrać z biblioteki klienta z *unpkg*.</span><span class="sxs-lookup"><span data-stu-id="d8890-138">For this tutorial, you use Library Manager (LibMan) to get the client library from *unpkg*.</span></span> <span data-ttu-id="d8890-139">unpkg jest usługa content delivery network (CDN)), można dostarczać niczego w Menedżer pakietów npm oraz Menedżera pakietów środowiska Node.js.</span><span class="sxs-lookup"><span data-stu-id="d8890-139">unpkg is a content delivery network (CDN)) that can deliver anything found in npm, the Node.js package manager.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="d8890-140">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="d8890-140">Visual Studio</span></span>](#tab/visual-studio/)

* <span data-ttu-id="d8890-141">W **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy projekt i wybierz **Dodaj** > **biblioteki po stronie klienta**.</span><span class="sxs-lookup"><span data-stu-id="d8890-141">In **Solution Explorer**, right-click the project, and select **Add** > **Client-Side Library**.</span></span>

* <span data-ttu-id="d8890-142">W **Dodaj biblioteki po stronie klienta** okno dialogowe, aby uzyskać **dostawcy** wybierz **unpkg**.</span><span class="sxs-lookup"><span data-stu-id="d8890-142">In the **Add Client-Side Library** dialog, for **Provider** select **unpkg**.</span></span>

* <span data-ttu-id="d8890-143">Aby uzyskać **biblioteki**, wprowadź `@aspnet/signalr@1`i wybierz najnowszą wersję, która nie jest w wersji zapoznawczej.</span><span class="sxs-lookup"><span data-stu-id="d8890-143">For **Library**, enter `@aspnet/signalr@1`, and select the latest version that isn't preview.</span></span>

  ![Dodaj okno dialogowe biblioteki po stronie klienta — Wybieranie biblioteki](signalr/_static/libman1.png)

* <span data-ttu-id="d8890-145">Wybierz **wybierz konkretne pliki**, rozwiń węzeł *dist/przeglądarki* folder, a następnie wybierz *signalr.js* i *signalr.min.js*.</span><span class="sxs-lookup"><span data-stu-id="d8890-145">Select **Choose specific files**, expand the *dist/browser* folder, and select *signalr.js* and *signalr.min.js*.</span></span>

* <span data-ttu-id="d8890-146">Ustaw **lokalizacji docelowej** do *wwwroot/lib/signalr/* i wybierz **zainstalować**.</span><span class="sxs-lookup"><span data-stu-id="d8890-146">Set **Target Location** to *wwwroot/lib/signalr/*, and select **Install**.</span></span>

  ![Dodaj okno dialogowe z biblioteki klienta - wybranie plików i miejsce docelowe](signalr/_static/libman2.png)

  <span data-ttu-id="d8890-148">Tworzy LibMan *wwwroot/lib/signalr* folder i kopiuje wybrane pliki.</span><span class="sxs-lookup"><span data-stu-id="d8890-148">LibMan creates a *wwwroot/lib/signalr* folder and copies the selected files to it.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="d8890-149">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="d8890-149">Visual Studio Code</span></span>](#tab/visual-studio-code/)

* <span data-ttu-id="d8890-150">W zintegrowanym terminalu uruchom następujące polecenie, aby zainstalować LibMan.</span><span class="sxs-lookup"><span data-stu-id="d8890-150">In the integrated terminal, run the following command to install LibMan.</span></span>

  ```console
  dotnet tool install -g Microsoft.Web.LibraryManager.Cli
  ```

* <span data-ttu-id="d8890-151">Uruchom następujące polecenie, aby przy użyciu LibMan uzyskać biblioteki klienta SignalR.</span><span class="sxs-lookup"><span data-stu-id="d8890-151">Run the following command to get the SignalR client library by using LibMan.</span></span> <span data-ttu-id="d8890-152">Być może musisz odczekaj kilka sekund, zanim dane wyjściowe.</span><span class="sxs-lookup"><span data-stu-id="d8890-152">You might have to wait a few seconds before seeing output.</span></span>

  ```console
  libman install @aspnet/signalr -p unpkg -d wwwroot/lib/signalr --files dist/browser/signalr.js --files dist/browser/signalr.min.js
  ```

  <span data-ttu-id="d8890-153">Parametry określ następujące opcje:</span><span class="sxs-lookup"><span data-stu-id="d8890-153">The parameters specify the following options:</span></span>
  * <span data-ttu-id="d8890-154">Użyj dostawcy unpkg.</span><span class="sxs-lookup"><span data-stu-id="d8890-154">Use the unpkg provider.</span></span>
  * <span data-ttu-id="d8890-155">Skopiuj pliki do *wwwroot/lib/signalr* docelowego.</span><span class="sxs-lookup"><span data-stu-id="d8890-155">Copy files to the *wwwroot/lib/signalr* destination.</span></span>
  * <span data-ttu-id="d8890-156">Skopiuj tylko określonych plików.</span><span class="sxs-lookup"><span data-stu-id="d8890-156">Copy only the specified files.</span></span>

  <span data-ttu-id="d8890-157">Dane wyjściowe wyglądają jak w poniższym przykładzie:</span><span class="sxs-lookup"><span data-stu-id="d8890-157">The output looks like the following example:</span></span>

  ```console
  wwwroot/lib/signalr/dist/browser/signalr.js written to disk
  wwwroot/lib/signalr/dist/browser/signalr.min.js written to disk
  Installed library "@aspnet/signalr@1.0.3" to "wwwroot/lib/signalr"
  ```

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="d8890-158">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="d8890-158">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="d8890-159">W **terminalu**, uruchom następujące polecenie, aby zainstalować LibMan.</span><span class="sxs-lookup"><span data-stu-id="d8890-159">In the **Terminal**, run the following command to install LibMan.</span></span>

  ```console
  dotnet tool install -g Microsoft.Web.LibraryManager.Cli
  ```

* <span data-ttu-id="d8890-160">Przejdź do folderu projektu (zawierającego *SignalRChat.csproj* pliku).</span><span class="sxs-lookup"><span data-stu-id="d8890-160">Navigate to the project folder (the one that contains the *SignalRChat.csproj* file).</span></span>

* <span data-ttu-id="d8890-161">Uruchom następujące polecenie, aby przy użyciu LibMan uzyskać biblioteki klienta SignalR.</span><span class="sxs-lookup"><span data-stu-id="d8890-161">Run the following command to get the SignalR client library by using LibMan.</span></span>

  ```console
  libman install @aspnet/signalr -p unpkg -d wwwroot/lib/signalr --files dist/browser/signalr.js --files dist/browser/signalr.min.js
  ```

  <span data-ttu-id="d8890-162">Parametry określ następujące opcje:</span><span class="sxs-lookup"><span data-stu-id="d8890-162">The parameters specify the following options:</span></span>
  * <span data-ttu-id="d8890-163">Użyj dostawcy unpkg.</span><span class="sxs-lookup"><span data-stu-id="d8890-163">Use the unpkg provider.</span></span>
  * <span data-ttu-id="d8890-164">Skopiuj pliki do *wwwroot/lib/signalr* docelowego.</span><span class="sxs-lookup"><span data-stu-id="d8890-164">Copy files to the *wwwroot/lib/signalr* destination.</span></span>
  * <span data-ttu-id="d8890-165">Skopiuj tylko określonych plików.</span><span class="sxs-lookup"><span data-stu-id="d8890-165">Copy only the specified files.</span></span>

  <span data-ttu-id="d8890-166">Dane wyjściowe wyglądają jak w poniższym przykładzie:</span><span class="sxs-lookup"><span data-stu-id="d8890-166">The output looks like the following example:</span></span>

  ```console
  wwwroot/lib/signalr/dist/browser/signalr.js written to disk
  wwwroot/lib/signalr/dist/browser/signalr.min.js written to disk
  Installed library "@aspnet/signalr@1.0.3" to "wwwroot/lib/signalr"
  ```

---

## <a name="create-a-signalr-hub"></a><span data-ttu-id="d8890-167">Tworzenie Centrum SignalR</span><span class="sxs-lookup"><span data-stu-id="d8890-167">Create a SignalR hub</span></span>

<span data-ttu-id="d8890-168">A *Centrum* jest klasa, która służy jako ogólny potok, który obsługuje komunikację klient serwer.</span><span class="sxs-lookup"><span data-stu-id="d8890-168">A *hub* is a class that serves as a high-level pipeline that handles client-server communication.</span></span>

* <span data-ttu-id="d8890-169">W folderze projektu SignalRChat Utwórz *koncentratory* folderu.</span><span class="sxs-lookup"><span data-stu-id="d8890-169">In the SignalRChat project folder, create a *Hubs* folder.</span></span>

* <span data-ttu-id="d8890-170">W *Hubs* folderze utwórz *ChatHub.cs* pliku następującym kodem:</span><span class="sxs-lookup"><span data-stu-id="d8890-170">In the *Hubs* folder, create a *ChatHub.cs* file with the following code:</span></span>

  [!code-csharp[Startup](signalr/sample/Hubs/ChatHub.cs)]

  <span data-ttu-id="d8890-171">`ChatHub` Klasa dziedziczy z elementu SignalR `Hub` klasy.</span><span class="sxs-lookup"><span data-stu-id="d8890-171">The `ChatHub` class inherits from the SignalR `Hub` class.</span></span> <span data-ttu-id="d8890-172">`Hub` Klasa zarządza połączeń, grup i komunikatów.</span><span class="sxs-lookup"><span data-stu-id="d8890-172">The `Hub` class manages connections, groups, and messaging.</span></span>

  <span data-ttu-id="d8890-173">`SendMessage` Metoda może być wywoływana przez połączone klienta, aby wysłać wiadomość do wszystkich klientów.</span><span class="sxs-lookup"><span data-stu-id="d8890-173">The `SendMessage` method can be called by a connected client to send a message to all clients.</span></span> <span data-ttu-id="d8890-174">Kod klienta JavaScript, który wywołuje metodę przedstawiono w dalszej części tego samouczka.</span><span class="sxs-lookup"><span data-stu-id="d8890-174">JavaScript client code that calls the method is shown later in the tutorial.</span></span> <span data-ttu-id="d8890-175">Kod SignalR jest asynchroniczne w celu zapewnienia maksymalnej skalowalności.</span><span class="sxs-lookup"><span data-stu-id="d8890-175">SignalR code is asynchronous to provide maximum scalability.</span></span>

## <a name="configure-signalr"></a><span data-ttu-id="d8890-176">Konfigurowanie biblioteki SignalR</span><span class="sxs-lookup"><span data-stu-id="d8890-176">Configure SignalR</span></span>

<span data-ttu-id="d8890-177">Serwer biblioteki SignalR musi być skonfigurowany do przekazywania żądań SignalR z SignalR.</span><span class="sxs-lookup"><span data-stu-id="d8890-177">The SignalR server must be configured to pass SignalR requests to SignalR.</span></span>

* <span data-ttu-id="d8890-178">Dodaj następujący wyróżniony kod do *Startup.cs* pliku.</span><span class="sxs-lookup"><span data-stu-id="d8890-178">Add the following highlighted code to the *Startup.cs* file.</span></span>

  [!code-csharp[Startup](signalr/sample/Startup.cs?highlight=7,33,52-55)]

  <span data-ttu-id="d8890-179">Te zmiany dodanie SignalR systemu iniekcji zależności platformy ASP.NET Core i potoku oprogramowania pośredniczącego.</span><span class="sxs-lookup"><span data-stu-id="d8890-179">These changes add SignalR to the ASP.NET Core dependency injection system and the middleware pipeline.</span></span>

## <a name="add-signalr-client-code"></a><span data-ttu-id="d8890-180">Dodaj kod klienta SignalR</span><span class="sxs-lookup"><span data-stu-id="d8890-180">Add SignalR client code</span></span>

* <span data-ttu-id="d8890-181">Zastąp zawartość *Pages\Index.cshtml* następującym kodem:</span><span class="sxs-lookup"><span data-stu-id="d8890-181">Replace the content in *Pages\Index.cshtml* with the following code:</span></span>

  [!code-cshtml[Index](signalr/sample/Pages/Index.cshtml)]

  <span data-ttu-id="d8890-182">Powyższy kod:</span><span class="sxs-lookup"><span data-stu-id="d8890-182">The preceding code:</span></span>

  * <span data-ttu-id="d8890-183">Tworzy pola tekstowe nazwy i tekst wiadomość i przycisk Prześlij.</span><span class="sxs-lookup"><span data-stu-id="d8890-183">Creates text boxes for name and message text, and a submit button.</span></span>
  * <span data-ttu-id="d8890-184">Tworzy listę z `id="messagesList"` do wyświetlania komunikatów odebranych z Centrum SignalR.</span><span class="sxs-lookup"><span data-stu-id="d8890-184">Creates a list with `id="messagesList"` for displaying messages that are received from the SignalR hub.</span></span>
  * <span data-ttu-id="d8890-185">Zawiera odwołania do skryptu z SignalR i *chat.js* kod aplikacji, który zostanie utworzony w następnym kroku.</span><span class="sxs-lookup"><span data-stu-id="d8890-185">Includes script references to SignalR and the *chat.js* application code that you create in the next step.</span></span>

* <span data-ttu-id="d8890-186">W *wwwroot/js* folderze utwórz *chat.js* pliku następującym kodem:</span><span class="sxs-lookup"><span data-stu-id="d8890-186">In the *wwwroot/js* folder, create a *chat.js* file with the following code:</span></span>

  [!code-javascript[Index](signalr/sample/wwwroot/js/chat.js)]

  <span data-ttu-id="d8890-187">Powyższy kod:</span><span class="sxs-lookup"><span data-stu-id="d8890-187">The preceding code:</span></span>

  * <span data-ttu-id="d8890-188">Tworzy i uruchamia połączenie.</span><span class="sxs-lookup"><span data-stu-id="d8890-188">Creates and starts a connection.</span></span>
  * <span data-ttu-id="d8890-189">Dodaje przycisk przesyłania program obsługi, która wysyła komunikaty do Centrum.</span><span class="sxs-lookup"><span data-stu-id="d8890-189">Adds to the submit button a handler that sends messages to the hub.</span></span>
  * <span data-ttu-id="d8890-190">Dodaje do obiektu połączenia program obsługi, który odbiera komunikaty z Centrum i dodaje je do listy.</span><span class="sxs-lookup"><span data-stu-id="d8890-190">Adds to the connection object a handler that receives messages from the hub and adds them to the list.</span></span>

## <a name="run-the-app"></a><span data-ttu-id="d8890-191">Uruchamianie aplikacji</span><span class="sxs-lookup"><span data-stu-id="d8890-191">Run the app</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="d8890-192">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="d8890-192">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="d8890-193">Naciśnij klawisz **kombinację klawiszy CTRL + F5** Aby uruchomić aplikację bez debugowania.</span><span class="sxs-lookup"><span data-stu-id="d8890-193">Press **CTRL+F5** to run the app without debugging.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="d8890-194">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="d8890-194">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="d8890-195">W zintegrowanym terminalu uruchom następujące polecenie:</span><span class="sxs-lookup"><span data-stu-id="d8890-195">In the integrated terminal, run the following command:</span></span>

  ```console
  dotnet run -p SignalRChat.csproj
  ```

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="d8890-196">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="d8890-196">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="d8890-197">W menu, wybierz opcję **Uruchom > Uruchom bez debugowania**.</span><span class="sxs-lookup"><span data-stu-id="d8890-197">From the menu, select **Run > Start Without Debugging**.</span></span>

---

* <span data-ttu-id="d8890-198">Skopiuj adres URL z paska adresu, a następnie otwórz innego wystąpienia przeglądarki lub karty i wklej adres URL w pasku adresu.</span><span class="sxs-lookup"><span data-stu-id="d8890-198">Copy the URL from the address bar, open another browser instance or tab, and paste the URL in the address bar.</span></span>

* <span data-ttu-id="d8890-199">Wybierz albo przeglądarki, wprowadź nazwę i komunikat, a następnie wybierz **wysyłania komunikatu** przycisku.</span><span class="sxs-lookup"><span data-stu-id="d8890-199">Choose either browser, enter a name and message, and select the **Send Message** button.</span></span>

  <span data-ttu-id="d8890-200">Nazwa i wiadomości są wyświetlane na obu stronach natychmiast.</span><span class="sxs-lookup"><span data-stu-id="d8890-200">The name and message are displayed on both pages instantly.</span></span>

  ![SignalR przykładowej aplikacji](signalr/_static/signalr-get-started-finished.png)

> [!TIP]
> <span data-ttu-id="d8890-202">Jeśli aplikacja nie działa, Otwórz swoje narzędzia deweloperskie przeglądarki (F12) i przejdź do konsoli.</span><span class="sxs-lookup"><span data-stu-id="d8890-202">If the app doesn't work, open your browser developer tools (F12) and go to the console.</span></span> <span data-ttu-id="d8890-203">Może pojawić się błędy związane z Twoim kodem HTML i JavaScript.</span><span class="sxs-lookup"><span data-stu-id="d8890-203">You might see errors related to your HTML and JavaScript code.</span></span> <span data-ttu-id="d8890-204">Załóżmy, że możesz umieścić *signalr.js* w innym folderze niż bezpośredniego.</span><span class="sxs-lookup"><span data-stu-id="d8890-204">For example, suppose you put *signalr.js* in a different folder than directed.</span></span> <span data-ttu-id="d8890-205">W takim przypadku odwołania do tego pliku nie będzie działać, a następnie zostanie wyświetlony błąd 404 w konsoli.</span><span class="sxs-lookup"><span data-stu-id="d8890-205">In that case the reference to that file won't work and you'll see a 404 error in the console.</span></span>
> <span data-ttu-id="d8890-206">![błąd — nie znaleziono signalr.js](signalr/_static/f12-console.png)</span><span class="sxs-lookup"><span data-stu-id="d8890-206">![signalr.js not found error](signalr/_static/f12-console.png)</span></span>

## <a name="next-steps"></a><span data-ttu-id="d8890-207">Następne kroki</span><span class="sxs-lookup"><span data-stu-id="d8890-207">Next steps</span></span>

<span data-ttu-id="d8890-208">W niniejszym samouczku zawarto informacje na temat wykonywania następujących czynności:</span><span class="sxs-lookup"><span data-stu-id="d8890-208">In this tutorial, you learned how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="d8890-209">Tworzenie projektu aplikacji sieci web.</span><span class="sxs-lookup"><span data-stu-id="d8890-209">Create a web app project.</span></span>
> * <span data-ttu-id="d8890-210">Dodaj bibliotekę klienta SignalR.</span><span class="sxs-lookup"><span data-stu-id="d8890-210">Add the SignalR client library.</span></span>
> * <span data-ttu-id="d8890-211">Utwórz Centrum SignalR.</span><span class="sxs-lookup"><span data-stu-id="d8890-211">Create a SignalR hub.</span></span>
> * <span data-ttu-id="d8890-212">Skonfiguruj projekt do korzystania z SignalR.</span><span class="sxs-lookup"><span data-stu-id="d8890-212">Configure the project to use SignalR.</span></span>
> * <span data-ttu-id="d8890-213">Dodaj kod używający koncentratora, aby wysyłać komunikaty z dowolnego klienta do wszystkich połączonych klientów.</span><span class="sxs-lookup"><span data-stu-id="d8890-213">Add code that uses the hub to send messages from any client to all connected clients.</span></span>

<span data-ttu-id="d8890-214">Aby dowiedzieć się więcej na temat biblioteki SignalR, zobacz wprowadzenie:</span><span class="sxs-lookup"><span data-stu-id="d8890-214">To learn more about SignalR, see the introduction:</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="d8890-215">Wprowadzenie do SignalR platformy ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="d8890-215">Introduction to ASP.NET Core SignalR</span></span>](xref:signalr/introduction)
