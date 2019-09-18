---
title: Wprowadzenie do ASP.NET Core sygnalizującego
author: bradygaster
description: W tym samouczku utworzysz aplikację czatu korzystającą z ASP.NET Core sygnalizującego.
ms.author: bradyg
ms.custom: mvc
ms.date: 07/08/2019
uid: tutorials/signalr
ms.openlocfilehash: 2dfa994b9763a0139cb70cbf9847ac3b02b568e4
ms.sourcegitcommit: 215954a638d24124f791024c66fd4fb9109fd380
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 09/18/2019
ms.locfileid: "71081970"
---
# <a name="tutorial-get-started-with-aspnet-core-signalr"></a><span data-ttu-id="9ffb9-103">Samouczek: Wprowadzenie do ASP.NET Core sygnalizującego</span><span class="sxs-lookup"><span data-stu-id="9ffb9-103">Tutorial: Get started with ASP.NET Core SignalR</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="9ffb9-104">W tym samouczku przedstawiono podstawy tworzenia aplikacji w czasie rzeczywistym przy użyciu usługi sygnalizującej.</span><span class="sxs-lookup"><span data-stu-id="9ffb9-104">This tutorial teaches the basics of building a real-time app using SignalR.</span></span> <span data-ttu-id="9ffb9-105">Dowiesz się, jak:</span><span class="sxs-lookup"><span data-stu-id="9ffb9-105">You learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="9ffb9-106">Utwórz projekt sieci web.</span><span class="sxs-lookup"><span data-stu-id="9ffb9-106">Create a web project.</span></span>
> * <span data-ttu-id="9ffb9-107">Dodaj bibliotekę klienta sygnalizującego.</span><span class="sxs-lookup"><span data-stu-id="9ffb9-107">Add the SignalR client library.</span></span>
> * <span data-ttu-id="9ffb9-108">Utwórz centrum sygnałów.</span><span class="sxs-lookup"><span data-stu-id="9ffb9-108">Create a SignalR hub.</span></span>
> * <span data-ttu-id="9ffb9-109">Skonfiguruj projekt do używania sygnalizującego.</span><span class="sxs-lookup"><span data-stu-id="9ffb9-109">Configure the project to use SignalR.</span></span>
> * <span data-ttu-id="9ffb9-110">Dodaj kod, który wysyła komunikaty z dowolnego klienta do wszystkich połączonych klientów.</span><span class="sxs-lookup"><span data-stu-id="9ffb9-110">Add code that sends messages from any client to all connected clients.</span></span>

<span data-ttu-id="9ffb9-111">Na końcu będziesz mieć działającą aplikację czatu:</span><span class="sxs-lookup"><span data-stu-id="9ffb9-111">At the end, you'll have a working chat app:</span></span>

![Przykładowa aplikacja sygnalizująca](signalr/_static/3.x/signalr-get-started-finished.png)

## <a name="prerequisites"></a><span data-ttu-id="9ffb9-113">Wymagania wstępne</span><span class="sxs-lookup"><span data-stu-id="9ffb9-113">Prerequisites</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="9ffb9-114">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="9ffb9-114">Visual Studio</span></span>](#tab/visual-studio)

[!INCLUDE[](~/includes/net-core-prereqs-vs-3.0.md)]

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="9ffb9-115">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="9ffb9-115">Visual Studio Code</span></span>](#tab/visual-studio-code)

[!INCLUDE[](~/includes/net-core-prereqs-vsc-3.0.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="9ffb9-116">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="9ffb9-116">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

[!INCLUDE[](~/includes/net-core-prereqs-mac-3.0.md)]

---

## <a name="create-a-web-app-project"></a><span data-ttu-id="9ffb9-117">Tworzenie projektu aplikacji sieci Web</span><span class="sxs-lookup"><span data-stu-id="9ffb9-117">Create a web app project</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="9ffb9-118">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="9ffb9-118">Visual Studio</span></span>](#tab/visual-studio/)

* <span data-ttu-id="9ffb9-119">Z menu wybierz pozycję **plik > nowy projekt**.</span><span class="sxs-lookup"><span data-stu-id="9ffb9-119">From the menu, select **File > New Project**.</span></span>

* <span data-ttu-id="9ffb9-120">W oknie dialogowym **Tworzenie nowego projektu** wybierz pozycję **ASP.NET Core aplikacja sieci Web**, a następnie wybierz przycisk **dalej**.</span><span class="sxs-lookup"><span data-stu-id="9ffb9-120">In the **Create a new project** dialog, select **ASP.NET Core Web Application**, and then select **Next**.</span></span>

* <span data-ttu-id="9ffb9-121">W oknie dialogowym **Konfigurowanie nowego projektu** Nazwij projekt *SignalRChat*, a następnie wybierz pozycję **Utwórz**.</span><span class="sxs-lookup"><span data-stu-id="9ffb9-121">In the **Configure your new project** dialog, name the project *SignalRChat*, and then select **Create**.</span></span>

* <span data-ttu-id="9ffb9-122">W oknie dialogowym **Tworzenie nowej ASP.NET Core aplikacji sieci Web** wybierz pozycję **.net Core** i **ASP.NET Core 3,0**.</span><span class="sxs-lookup"><span data-stu-id="9ffb9-122">In the **Create a new ASP.NET Core Web Application** dialog, select **.NET Core** and **ASP.NET Core 3.0**.</span></span> 

* <span data-ttu-id="9ffb9-123">Wybierz pozycję **aplikacja sieci Web** , aby utworzyć projekt, który używa Razor Pages, a następnie wybierz pozycję **Utwórz**.</span><span class="sxs-lookup"><span data-stu-id="9ffb9-123">Select **Web Application** to create a project that uses Razor Pages, and then select **Create**.</span></span>

  ![Okno dialogowe nowego projektu w programie Visual Studio](signalr/_static/3.x/signalr-new-project-dialog.png)

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="9ffb9-125">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="9ffb9-125">Visual Studio Code</span></span>](#tab/visual-studio-code/)

* <span data-ttu-id="9ffb9-126">Otwórz [zintegrowany terminal](https://code.visualstudio.com/docs/editor/integrated-terminal) do folderu, w którym zostanie utworzony nowy folder projektu.</span><span class="sxs-lookup"><span data-stu-id="9ffb9-126">Open the [integrated terminal](https://code.visualstudio.com/docs/editor/integrated-terminal) to the folder in which the new project folder will be created.</span></span>

* <span data-ttu-id="9ffb9-127">Uruchom następujące polecenia:</span><span class="sxs-lookup"><span data-stu-id="9ffb9-127">Run the following commands:</span></span>

   ```dotnetcli
   dotnet new webapp -o SignalRChat
   code -r SignalRChat
   ```

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="9ffb9-128">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="9ffb9-128">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="9ffb9-129">Z menu wybierz pozycję **plik > nowe rozwiązanie**.</span><span class="sxs-lookup"><span data-stu-id="9ffb9-129">From the menu, select **File > New Solution**.</span></span>

* <span data-ttu-id="9ffb9-130">Wybierz pozycję **.NET Core > App > aplikacji sieci Web** (nie zaznaczaj **aplikacji sieci Web (Model-View-Controller)** ), a następnie wybierz przycisk **dalej**.</span><span class="sxs-lookup"><span data-stu-id="9ffb9-130">Select **.NET Core > App > Web Application** (Don't select **Web Application (Model-View-Controller)**), and then select **Next**.</span></span>

* <span data-ttu-id="9ffb9-131">Upewnij się, że **platforma docelowa** jest ustawiona na **platformę .NET Core 3,0**, a następnie wybierz przycisk **dalej**.</span><span class="sxs-lookup"><span data-stu-id="9ffb9-131">Make sure the **Target Framework** is set to **.NET Core 3.0**, and then select **Next**.</span></span>

* <span data-ttu-id="9ffb9-132">Nazwij projekt *SignalRChat*, a następnie wybierz pozycję **Utwórz**.</span><span class="sxs-lookup"><span data-stu-id="9ffb9-132">Name the project *SignalRChat*, and then select **Create**.</span></span>

---

## <a name="add-the-signalr-client-library"></a><span data-ttu-id="9ffb9-133">Dodawanie biblioteki klienta sygnalizującego</span><span class="sxs-lookup"><span data-stu-id="9ffb9-133">Add the SignalR client library</span></span>

<span data-ttu-id="9ffb9-134">Biblioteka serwera sygnalizującego jest dołączona do struktury udostępnionej ASP.NET Core 3,0.</span><span class="sxs-lookup"><span data-stu-id="9ffb9-134">The SignalR server library is included in the ASP.NET Core 3.0 shared framework.</span></span> <span data-ttu-id="9ffb9-135">Biblioteka klienta JavaScript nie jest automatycznie dołączana do projektu.</span><span class="sxs-lookup"><span data-stu-id="9ffb9-135">The JavaScript client library isn't automatically included in the project.</span></span> <span data-ttu-id="9ffb9-136">W tym samouczku użyjesz programu Library Manager (LibMan), aby uzyskać bibliotekę kliencką z *unpkg*.</span><span class="sxs-lookup"><span data-stu-id="9ffb9-136">For this tutorial, you use Library Manager (LibMan) to get the client library from *unpkg*.</span></span> <span data-ttu-id="9ffb9-137">unpkg to usługa Content Delivery Network (CDN), która umożliwia dostarczanie elementów znalezionych w programie npm, w Menedżerze pakietów środowiska Node. js.</span><span class="sxs-lookup"><span data-stu-id="9ffb9-137">unpkg is a content delivery network (CDN)) that can deliver anything found in npm, the Node.js package manager.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="9ffb9-138">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="9ffb9-138">Visual Studio</span></span>](#tab/visual-studio/)

* <span data-ttu-id="9ffb9-139">W **Eksplorator rozwiązań**kliknij prawym przyciskiem myszy projekt, a następnie wybierz polecenie **Dodaj** > **bibliotekę po stronie klienta**.</span><span class="sxs-lookup"><span data-stu-id="9ffb9-139">In **Solution Explorer**, right-click the project, and select **Add** > **Client-Side Library**.</span></span>

* <span data-ttu-id="9ffb9-140">W oknie dialogowym **Dodawanie biblioteki po stronie klienta** dla **dostawcy** wybierz pozycję **unpkg**.</span><span class="sxs-lookup"><span data-stu-id="9ffb9-140">In the **Add Client-Side Library** dialog, for **Provider** select **unpkg**.</span></span>

* <span data-ttu-id="9ffb9-141">W obszarze **Biblioteka**wprowadź `@aspnet/signalr@next`.</span><span class="sxs-lookup"><span data-stu-id="9ffb9-141">For **Library**, enter `@aspnet/signalr@next`.</span></span>
<!-- when 3.0 is released, change @next to @latest -->

* <span data-ttu-id="9ffb9-142">Wybierz pozycję **Wybierz określone pliki**, rozwiń folder *dist/przeglądarka* , a następnie wybierz pozycję *sygnalizujące. js* i *sygnalizujący. min. js*.</span><span class="sxs-lookup"><span data-stu-id="9ffb9-142">Select **Choose specific files**, expand the *dist/browser* folder, and select *signalr.js* and *signalr.min.js*.</span></span>

* <span data-ttu-id="9ffb9-143">Ustaw **lokalizację docelową** na plik *wwwroot/lib/sygnalizujący/* , a następnie wybierz pozycję **Zainstaluj**.</span><span class="sxs-lookup"><span data-stu-id="9ffb9-143">Set **Target Location** to *wwwroot/lib/signalr/*, and select **Install**.</span></span>

  ![Okno dialogowe Dodawanie biblioteki po stronie klienta — wybór biblioteki](signalr/_static/3.x/libman1.png)

  <span data-ttu-id="9ffb9-145">LibMan tworzy folder *wwwroot/lib/sygnalizujący* i kopiuje do niego wybrane pliki.</span><span class="sxs-lookup"><span data-stu-id="9ffb9-145">LibMan creates a *wwwroot/lib/signalr* folder and copies the selected files to it.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="9ffb9-146">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="9ffb9-146">Visual Studio Code</span></span>](#tab/visual-studio-code/)

* <span data-ttu-id="9ffb9-147">W zintegrowanym terminalu uruchom następujące polecenie, aby zainstalować LibMan.</span><span class="sxs-lookup"><span data-stu-id="9ffb9-147">In the integrated terminal, run the following command to install LibMan.</span></span>

  ```dotnetcli
  dotnet tool install -g Microsoft.Web.LibraryManager.Cli
  ```

* <span data-ttu-id="9ffb9-148">Uruchom następujące polecenie, aby uzyskać bibliotekę klienta sygnalizującego za pomocą LibMan.</span><span class="sxs-lookup"><span data-stu-id="9ffb9-148">Run the following command to get the SignalR client library by using LibMan.</span></span> <span data-ttu-id="9ffb9-149">Może być konieczne odczekanie kilku sekund przed wyświetleniem danych wyjściowych.</span><span class="sxs-lookup"><span data-stu-id="9ffb9-149">You might have to wait a few seconds before seeing output.</span></span>

  ```console
  libman install @aspnet/signalr@next -p unpkg -d wwwroot/lib/signalr --files dist/browser/signalr.js --files dist/browser/signalr.min.js
  ```

  <span data-ttu-id="9ffb9-150">Parametry określają następujące opcje:</span><span class="sxs-lookup"><span data-stu-id="9ffb9-150">The parameters specify the following options:</span></span>
  * <span data-ttu-id="9ffb9-151">Użyj dostawcy unpkg.</span><span class="sxs-lookup"><span data-stu-id="9ffb9-151">Use the unpkg provider.</span></span>
  * <span data-ttu-id="9ffb9-152">Kopiuj pliki do lokalizacji *wwwroot/lib/sygnalizującej* .</span><span class="sxs-lookup"><span data-stu-id="9ffb9-152">Copy files to the *wwwroot/lib/signalr* destination.</span></span>
  * <span data-ttu-id="9ffb9-153">Skopiuj tylko określone pliki.</span><span class="sxs-lookup"><span data-stu-id="9ffb9-153">Copy only the specified files.</span></span>

  <span data-ttu-id="9ffb9-154">Dane wyjściowe wyglądają podobnie do poniższego przykładu:</span><span class="sxs-lookup"><span data-stu-id="9ffb9-154">The output looks like the following example:</span></span>

  ```console
  wwwroot/lib/signalr/dist/browser/signalr.js written to disk
  wwwroot/lib/signalr/dist/browser/signalr.min.js written to disk
  Installed library "@aspnet/signalr@next" to "wwwroot/lib/signalr"
  ```

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="9ffb9-155">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="9ffb9-155">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="9ffb9-156">W **terminalu**Uruchom następujące polecenie, aby zainstalować LibMan.</span><span class="sxs-lookup"><span data-stu-id="9ffb9-156">In the **Terminal**, run the following command to install LibMan.</span></span>

  ```dotnetcli
  dotnet tool install -g Microsoft.Web.LibraryManager.Cli
  ```

* <span data-ttu-id="9ffb9-157">Przejdź do folderu projektu (taki, który zawiera plik *SignalRChat. csproj* ).</span><span class="sxs-lookup"><span data-stu-id="9ffb9-157">Navigate to the project folder (the one that contains the *SignalRChat.csproj* file).</span></span>

* <span data-ttu-id="9ffb9-158">Uruchom następujące polecenie, aby uzyskać bibliotekę klienta sygnalizującego za pomocą LibMan.</span><span class="sxs-lookup"><span data-stu-id="9ffb9-158">Run the following command to get the SignalR client library by using LibMan.</span></span>

  ```console
  libman install @aspnet/signalr@next -p unpkg -d wwwroot/lib/signalr --files dist/browser/signalr.js --files dist/browser/signalr.min.js
  ```

  <span data-ttu-id="9ffb9-159">Parametry określają następujące opcje:</span><span class="sxs-lookup"><span data-stu-id="9ffb9-159">The parameters specify the following options:</span></span>
  * <span data-ttu-id="9ffb9-160">Użyj dostawcy unpkg.</span><span class="sxs-lookup"><span data-stu-id="9ffb9-160">Use the unpkg provider.</span></span>
  * <span data-ttu-id="9ffb9-161">Kopiuj pliki do lokalizacji *wwwroot/lib/sygnalizującej* .</span><span class="sxs-lookup"><span data-stu-id="9ffb9-161">Copy files to the *wwwroot/lib/signalr* destination.</span></span>
  * <span data-ttu-id="9ffb9-162">Skopiuj tylko określone pliki.</span><span class="sxs-lookup"><span data-stu-id="9ffb9-162">Copy only the specified files.</span></span>

  <span data-ttu-id="9ffb9-163">Dane wyjściowe wyglądają podobnie do poniższego przykładu:</span><span class="sxs-lookup"><span data-stu-id="9ffb9-163">The output looks like the following example:</span></span>

  ```console
  wwwroot/lib/signalr/dist/browser/signalr.js written to disk
  wwwroot/lib/signalr/dist/browser/signalr.min.js written to disk
  Installed library "@aspnet/signalr@next" to "wwwroot/lib/signalr"
  ```

---

## <a name="create-a-signalr-hub"></a><span data-ttu-id="9ffb9-164">Tworzenie centrum sygnałów</span><span class="sxs-lookup"><span data-stu-id="9ffb9-164">Create a SignalR hub</span></span>

<span data-ttu-id="9ffb9-165">*Koncentrator* jest klasą, która służy jako potok wysokiego poziomu, który obsługuje komunikację klient-serwer.</span><span class="sxs-lookup"><span data-stu-id="9ffb9-165">A *hub* is a class that serves as a high-level pipeline that handles client-server communication.</span></span>

* <span data-ttu-id="9ffb9-166">W folderze projektu SignalRChat Utwórz folder *Hubs* .</span><span class="sxs-lookup"><span data-stu-id="9ffb9-166">In the SignalRChat project folder, create a *Hubs* folder.</span></span>

* <span data-ttu-id="9ffb9-167">W folderze *Hubs* utwórz plik *ChatHub.cs* o następującym kodzie:</span><span class="sxs-lookup"><span data-stu-id="9ffb9-167">In the *Hubs* folder, create a *ChatHub.cs* file with the following code:</span></span>

  [!code-csharp[Startup](signalr/sample-snapshot/3.x/ChatHub.cs)]

  <span data-ttu-id="9ffb9-168">Klasa dziedziczy od `Hub` klasy sygnalizującej. `ChatHub`</span><span class="sxs-lookup"><span data-stu-id="9ffb9-168">The `ChatHub` class inherits from the SignalR `Hub` class.</span></span> <span data-ttu-id="9ffb9-169">`Hub` Klasa zarządza połączeniami, grupami i obsługą wiadomości.</span><span class="sxs-lookup"><span data-stu-id="9ffb9-169">The `Hub` class manages connections, groups, and messaging.</span></span>

  <span data-ttu-id="9ffb9-170">`SendMessage` Metoda może być wywoływana przez połączonego klienta w celu wysłania komunikatu do wszystkich klientów.</span><span class="sxs-lookup"><span data-stu-id="9ffb9-170">The `SendMessage` method can be called by a connected client to send a message to all clients.</span></span> <span data-ttu-id="9ffb9-171">Kod klienta JavaScript, który wywołuje metodę, jest wyświetlany w dalszej części samouczka.</span><span class="sxs-lookup"><span data-stu-id="9ffb9-171">JavaScript client code that calls the method is shown later in the tutorial.</span></span> <span data-ttu-id="9ffb9-172">Kod sygnalizujący jest asynchroniczny, aby zapewnić maksymalną skalowalność.</span><span class="sxs-lookup"><span data-stu-id="9ffb9-172">SignalR code is asynchronous to provide maximum scalability.</span></span>

## <a name="configure-signalr"></a><span data-ttu-id="9ffb9-173">Konfigurowanie sygnalizującego</span><span class="sxs-lookup"><span data-stu-id="9ffb9-173">Configure SignalR</span></span>

<span data-ttu-id="9ffb9-174">Serwer sygnalizujący musi być skonfigurowany tak, aby przekazywać żądania sygnałów do sygnalizującego.</span><span class="sxs-lookup"><span data-stu-id="9ffb9-174">The SignalR server must be configured to pass SignalR requests to SignalR.</span></span>

* <span data-ttu-id="9ffb9-175">Dodaj następujący wyróżniony kod do pliku *Startup.cs* .</span><span class="sxs-lookup"><span data-stu-id="9ffb9-175">Add the following highlighted code to the *Startup.cs* file.</span></span>

  [!code-csharp[Startup](signalr/sample-snapshot/3.x/Startup.cs?highlight=6,30,58)]

  <span data-ttu-id="9ffb9-176">Te zmiany umożliwiają dodanie sygnalizacji do ASP.NET Core systemów iniekcji i routingu.</span><span class="sxs-lookup"><span data-stu-id="9ffb9-176">These changes add SignalR to the ASP.NET Core dependency injection and routing systems.</span></span>

## <a name="add-signalr-client-code"></a><span data-ttu-id="9ffb9-177">Dodaj kod klienta sygnalizującego</span><span class="sxs-lookup"><span data-stu-id="9ffb9-177">Add SignalR client code</span></span>

* <span data-ttu-id="9ffb9-178">Zastąp zawartość w *Pages\Index.cshtml* następującym kodem:</span><span class="sxs-lookup"><span data-stu-id="9ffb9-178">Replace the content in *Pages\Index.cshtml* with the following code:</span></span>

  [!code-cshtml[Index](signalr/sample-snapshot/3.x/Index.cshtml)]

  <span data-ttu-id="9ffb9-179">Powyższy kod:</span><span class="sxs-lookup"><span data-stu-id="9ffb9-179">The preceding code:</span></span>

  * <span data-ttu-id="9ffb9-180">Tworzy pola tekstowe dla nazwy i tekstu komunikatu oraz przycisk Prześlij.</span><span class="sxs-lookup"><span data-stu-id="9ffb9-180">Creates text boxes for name and message text, and a submit button.</span></span>
  * <span data-ttu-id="9ffb9-181">Tworzy listę z `id="messagesList"` do wyświetlania komunikatów odbieranych z centrum sygnałów.</span><span class="sxs-lookup"><span data-stu-id="9ffb9-181">Creates a list with `id="messagesList"` for displaying messages that are received from the SignalR hub.</span></span>
  * <span data-ttu-id="9ffb9-182">Zawiera odwołania do skryptów do programu Sygnalizującer i kod aplikacji *czatu. js* , który tworzysz w następnym kroku.</span><span class="sxs-lookup"><span data-stu-id="9ffb9-182">Includes script references to SignalR and the *chat.js* application code that you create in the next step.</span></span>

* <span data-ttu-id="9ffb9-183">W folderze *wwwroot/js* Utwórz plik *czatu. js* o następującym kodzie:</span><span class="sxs-lookup"><span data-stu-id="9ffb9-183">In the *wwwroot/js* folder, create a *chat.js* file with the following code:</span></span>

  [!code-javascript[Index](signalr/sample-snapshot/3.x/chat.js)]

  <span data-ttu-id="9ffb9-184">Powyższy kod:</span><span class="sxs-lookup"><span data-stu-id="9ffb9-184">The preceding code:</span></span>

  * <span data-ttu-id="9ffb9-185">Tworzy i uruchamia połączenie.</span><span class="sxs-lookup"><span data-stu-id="9ffb9-185">Creates and starts a connection.</span></span>
  * <span data-ttu-id="9ffb9-186">Dodaje do przycisku Prześlij procedurę obsługi, która wysyła komunikaty do centrum.</span><span class="sxs-lookup"><span data-stu-id="9ffb9-186">Adds to the submit button a handler that sends messages to the hub.</span></span>
  * <span data-ttu-id="9ffb9-187">Dodaje do obiektu Connection program obsługi, który odbiera komunikaty z centrum i dodaje je do listy.</span><span class="sxs-lookup"><span data-stu-id="9ffb9-187">Adds to the connection object a handler that receives messages from the hub and adds them to the list.</span></span>

## <a name="run-the-app"></a><span data-ttu-id="9ffb9-188">Uruchamianie aplikacji</span><span class="sxs-lookup"><span data-stu-id="9ffb9-188">Run the app</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="9ffb9-189">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="9ffb9-189">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="9ffb9-190">Naciśnij **klawisze CTRL + F5** , aby uruchomić aplikację bez debugowania.</span><span class="sxs-lookup"><span data-stu-id="9ffb9-190">Press **CTRL+F5** to run the app without debugging.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="9ffb9-191">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="9ffb9-191">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="9ffb9-192">W zintegrowanym terminalu uruchom następujące polecenie:</span><span class="sxs-lookup"><span data-stu-id="9ffb9-192">In the integrated terminal, run the following command:</span></span>

  ```dotnetcli
  dotnet run -p SignalRChat.csproj
  ```

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="9ffb9-193">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="9ffb9-193">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="9ffb9-194">Z menu wybierz polecenie **uruchom > Uruchom bez debugowania**.</span><span class="sxs-lookup"><span data-stu-id="9ffb9-194">From the menu, select **Run > Start Without Debugging**.</span></span>

---

* <span data-ttu-id="9ffb9-195">Skopiuj adres URL z paska adresu, Otwórz inne wystąpienie przeglądarki lub kartę, a następnie wklej adres URL na pasku adresu.</span><span class="sxs-lookup"><span data-stu-id="9ffb9-195">Copy the URL from the address bar, open another browser instance or tab, and paste the URL in the address bar.</span></span>

* <span data-ttu-id="9ffb9-196">Wybierz opcję przeglądarka, wprowadź nazwę i komunikat, a następnie wybierz przycisk **Wyślij wiadomość** .</span><span class="sxs-lookup"><span data-stu-id="9ffb9-196">Choose either browser, enter a name and message, and select the **Send Message** button.</span></span>

  <span data-ttu-id="9ffb9-197">Nazwa i komunikat są natychmiast wyświetlane na obu stronach.</span><span class="sxs-lookup"><span data-stu-id="9ffb9-197">The name and message are displayed on both pages instantly.</span></span>

  ![Przykładowa aplikacja sygnalizująca](signalr/_static/3.x/signalr-get-started-finished.png)

> [!TIP]
> * <span data-ttu-id="9ffb9-199">Jeśli aplikacja nie działa, Otwórz narzędzia deweloperskie przeglądarki (F12) i przejdź do konsoli programu.</span><span class="sxs-lookup"><span data-stu-id="9ffb9-199">If the app doesn't work, open your browser developer tools (F12) and go to the console.</span></span> <span data-ttu-id="9ffb9-200">Mogą pojawić się błędy związane z kodem HTML i JavaScript.</span><span class="sxs-lookup"><span data-stu-id="9ffb9-200">You might see errors related to your HTML and JavaScript code.</span></span> <span data-ttu-id="9ffb9-201">Załóżmy na przykład, że umieścisz polecenie *signaler. js* w innym folderze niż skierowany.</span><span class="sxs-lookup"><span data-stu-id="9ffb9-201">For example, suppose you put *signalr.js* in a different folder than directed.</span></span> <span data-ttu-id="9ffb9-202">W takim przypadku odwołanie do tego pliku nie będzie działało i zobaczysz błąd 404 w konsoli.</span><span class="sxs-lookup"><span data-stu-id="9ffb9-202">In that case the reference to that file won't work and you'll see a 404 error in the console.</span></span>
>   <span data-ttu-id="9ffb9-203">![błąd podczas znajdowania sygnalizującer. js](signalr/_static/3.x/f12-console.png)</span><span class="sxs-lookup"><span data-stu-id="9ffb9-203">![signalr.js not found error](signalr/_static/3.x/f12-console.png)</span></span>
> * <span data-ttu-id="9ffb9-204">Jeśli wystąpi błąd ERR_SPDY_INADEQUATE_TRANSPORT_SECURITY w przeglądarce Chrome lub NS_ERROR_NET_INADEQUATE_SECURITY w programie Firefox, uruchom następujące polecenia, aby zaktualizować certyfikat deweloperski:</span><span class="sxs-lookup"><span data-stu-id="9ffb9-204">If you get the error ERR_SPDY_INADEQUATE_TRANSPORT_SECURITY in Chrome or NS_ERROR_NET_INADEQUATE_SECURITY in Firefox, run these commands to update your development certificate:</span></span>
>
>   ```dotnetcli
>   dotnet dev-certs https --clean
>   dotnet dev-certs https --trust
>   ```

## <a name="next-steps"></a><span data-ttu-id="9ffb9-205">Następne kroki</span><span class="sxs-lookup"><span data-stu-id="9ffb9-205">Next steps</span></span>

<span data-ttu-id="9ffb9-206">Aby dowiedzieć się więcej na temat sygnalizacji, zobacz Wprowadzenie:</span><span class="sxs-lookup"><span data-stu-id="9ffb9-206">To learn more about SignalR, see the introduction:</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="9ffb9-207">Wprowadzenie do ASP.NET Core sygnalizującego</span><span class="sxs-lookup"><span data-stu-id="9ffb9-207">Introduction to ASP.NET Core SignalR</span></span>](xref:signalr/introduction)

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="9ffb9-208">W tym samouczku przedstawiono podstawy tworzenia aplikacji w czasie rzeczywistym przy użyciu usługi sygnalizującej.</span><span class="sxs-lookup"><span data-stu-id="9ffb9-208">This tutorial teaches the basics of building a real-time app using SignalR.</span></span> <span data-ttu-id="9ffb9-209">Dowiesz się, jak:</span><span class="sxs-lookup"><span data-stu-id="9ffb9-209">You learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="9ffb9-210">Utwórz projekt sieci web.</span><span class="sxs-lookup"><span data-stu-id="9ffb9-210">Create a web project.</span></span>
> * <span data-ttu-id="9ffb9-211">Dodaj bibliotekę klienta sygnalizującego.</span><span class="sxs-lookup"><span data-stu-id="9ffb9-211">Add the SignalR client library.</span></span>
> * <span data-ttu-id="9ffb9-212">Utwórz centrum sygnałów.</span><span class="sxs-lookup"><span data-stu-id="9ffb9-212">Create a SignalR hub.</span></span>
> * <span data-ttu-id="9ffb9-213">Skonfiguruj projekt do używania sygnalizującego.</span><span class="sxs-lookup"><span data-stu-id="9ffb9-213">Configure the project to use SignalR.</span></span>
> * <span data-ttu-id="9ffb9-214">Dodaj kod, który wysyła komunikaty z dowolnego klienta do wszystkich połączonych klientów.</span><span class="sxs-lookup"><span data-stu-id="9ffb9-214">Add code that sends messages from any client to all connected clients.</span></span>

<span data-ttu-id="9ffb9-215">Na końcu będziesz mieć działającą aplikację czatu:</span><span class="sxs-lookup"><span data-stu-id="9ffb9-215">At the end, you'll have a working chat app:</span></span>

![Przykładowa aplikacja sygnalizująca](signalr/_static/2.x/signalr-get-started-finished.png)

## <a name="prerequisites"></a><span data-ttu-id="9ffb9-217">Wymagania wstępne</span><span class="sxs-lookup"><span data-stu-id="9ffb9-217">Prerequisites</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="9ffb9-218">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="9ffb9-218">Visual Studio</span></span>](#tab/visual-studio)

[!INCLUDE[](~/includes/net-core-prereqs-vs2017-2.2.md)]

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="9ffb9-219">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="9ffb9-219">Visual Studio Code</span></span>](#tab/visual-studio-code)

[!INCLUDE[](~/includes/net-core-prereqs-vsc-2.2.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="9ffb9-220">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="9ffb9-220">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

[!INCLUDE[](~/includes/net-core-prereqs-mac-2.2.md)]

---

## <a name="create-a-web-project"></a><span data-ttu-id="9ffb9-221">Tworzenie projektu sieci web</span><span class="sxs-lookup"><span data-stu-id="9ffb9-221">Create a web project</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="9ffb9-222">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="9ffb9-222">Visual Studio</span></span>](#tab/visual-studio/)

* <span data-ttu-id="9ffb9-223">Z menu wybierz pozycję **plik > nowy projekt**.</span><span class="sxs-lookup"><span data-stu-id="9ffb9-223">From the menu, select **File > New Project**.</span></span>

* <span data-ttu-id="9ffb9-224">W oknie dialogowym **Nowy projekt** wybierz pozycję **zainstalowane > Visual C# > Web > ASP.NET Core aplikacji sieci Web**.</span><span class="sxs-lookup"><span data-stu-id="9ffb9-224">In the **New Project** dialog, select **Installed > Visual C# > Web > ASP.NET Core Web Application**.</span></span> <span data-ttu-id="9ffb9-225">Nazwij projekt *SignalRChat*.</span><span class="sxs-lookup"><span data-stu-id="9ffb9-225">Name the project *SignalRChat*.</span></span>

  ![Okno dialogowe nowego projektu w programie Visual Studio](signalr/_static/2.x/signalr-new-project-dialog.png)

* <span data-ttu-id="9ffb9-227">Wybierz pozycję **aplikacja sieci Web** , aby utworzyć projekt, który używa Razor Pages.</span><span class="sxs-lookup"><span data-stu-id="9ffb9-227">Select **Web Application** to create a project that uses Razor Pages.</span></span>

* <span data-ttu-id="9ffb9-228">Wybierz platformę docelową programu **.NET Core**, wybierz pozycję **ASP.NET Core 2,2**, a następnie kliknij przycisk **OK**.</span><span class="sxs-lookup"><span data-stu-id="9ffb9-228">Select a target framework of **.NET Core**, select **ASP.NET Core 2.2**, and click **OK**.</span></span>

  ![Okno dialogowe nowego projektu w programie Visual Studio](signalr/_static/2.x/signalr-new-project-choose-type.png)

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="9ffb9-230">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="9ffb9-230">Visual Studio Code</span></span>](#tab/visual-studio-code/)

* <span data-ttu-id="9ffb9-231">Otwórz [zintegrowany terminal](https://code.visualstudio.com/docs/editor/integrated-terminal) do folderu, w którym zostanie utworzony nowy folder projektu.</span><span class="sxs-lookup"><span data-stu-id="9ffb9-231">Open the [integrated terminal](https://code.visualstudio.com/docs/editor/integrated-terminal) to the folder in which the new project folder will be created.</span></span>

* <span data-ttu-id="9ffb9-232">Uruchom następujące polecenia:</span><span class="sxs-lookup"><span data-stu-id="9ffb9-232">Run the following commands:</span></span>

   ```dotnetcli
   dotnet new webapp -o SignalRChat
   code -r SignalRChat
   ```

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="9ffb9-233">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="9ffb9-233">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="9ffb9-234">Z menu wybierz pozycję **plik > nowe rozwiązanie**.</span><span class="sxs-lookup"><span data-stu-id="9ffb9-234">From the menu, select **File > New Solution**.</span></span>

* <span data-ttu-id="9ffb9-235">Wybierz pozycję **.NET Core > app > ASP.NET Core Web App** (nie wybieraj **ASP.NET Core Web App (MVC)** ).</span><span class="sxs-lookup"><span data-stu-id="9ffb9-235">Select **.NET Core > App > ASP.NET Core Web App** (Don't select **ASP.NET Core Web App (MVC)**).</span></span>

* <span data-ttu-id="9ffb9-236">Wybierz opcję **Dalej**.</span><span class="sxs-lookup"><span data-stu-id="9ffb9-236">Select **Next**.</span></span>

* <span data-ttu-id="9ffb9-237">Nazwij projekt *SignalRChat*, a następnie wybierz pozycję **Utwórz**.</span><span class="sxs-lookup"><span data-stu-id="9ffb9-237">Name the project *SignalRChat*, and then select **Create**.</span></span>

---

## <a name="add-the-signalr-client-library"></a><span data-ttu-id="9ffb9-238">Dodawanie biblioteki klienta sygnalizującego</span><span class="sxs-lookup"><span data-stu-id="9ffb9-238">Add the SignalR client library</span></span>

<span data-ttu-id="9ffb9-239">Biblioteka serwera sygnalizującego jest dołączona do `Microsoft.AspNetCore.App` pakietu.</span><span class="sxs-lookup"><span data-stu-id="9ffb9-239">The SignalR server library is included in the `Microsoft.AspNetCore.App` metapackage.</span></span> <span data-ttu-id="9ffb9-240">Biblioteka klienta JavaScript nie jest automatycznie dołączana do projektu.</span><span class="sxs-lookup"><span data-stu-id="9ffb9-240">The JavaScript client library isn't automatically included in the project.</span></span> <span data-ttu-id="9ffb9-241">W tym samouczku użyjesz programu Library Manager (LibMan), aby uzyskać bibliotekę kliencką z *unpkg*.</span><span class="sxs-lookup"><span data-stu-id="9ffb9-241">For this tutorial, you use Library Manager (LibMan) to get the client library from *unpkg*.</span></span> <span data-ttu-id="9ffb9-242">unpkg to usługa Content Delivery Network (CDN), która umożliwia dostarczanie elementów znalezionych w programie npm, w Menedżerze pakietów środowiska Node. js.</span><span class="sxs-lookup"><span data-stu-id="9ffb9-242">unpkg is a content delivery network (CDN)) that can deliver anything found in npm, the Node.js package manager.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="9ffb9-243">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="9ffb9-243">Visual Studio</span></span>](#tab/visual-studio/)

* <span data-ttu-id="9ffb9-244">W **Eksplorator rozwiązań**kliknij prawym przyciskiem myszy projekt, a następnie wybierz polecenie **Dodaj** > **bibliotekę po stronie klienta**.</span><span class="sxs-lookup"><span data-stu-id="9ffb9-244">In **Solution Explorer**, right-click the project, and select **Add** > **Client-Side Library**.</span></span>

* <span data-ttu-id="9ffb9-245">W oknie dialogowym **Dodawanie biblioteki po stronie klienta** dla **dostawcy** wybierz pozycję **unpkg**.</span><span class="sxs-lookup"><span data-stu-id="9ffb9-245">In the **Add Client-Side Library** dialog, for **Provider** select **unpkg**.</span></span>

* <span data-ttu-id="9ffb9-246">W polu **Biblioteka**wprowadź `@aspnet/signalr@1`i wybierz najnowszą wersję, która nie jest w wersji zapoznawczej.</span><span class="sxs-lookup"><span data-stu-id="9ffb9-246">For **Library**, enter `@aspnet/signalr@1`, and select the latest version that isn't preview.</span></span>

  ![Okno dialogowe Dodawanie biblioteki po stronie klienta — wybór biblioteki](signalr/_static/2.x/libman1.png)

* <span data-ttu-id="9ffb9-248">Wybierz pozycję **Wybierz określone pliki**, rozwiń folder *dist/przeglądarka* , a następnie wybierz pozycję *sygnalizujące. js* i *sygnalizujący. min. js*.</span><span class="sxs-lookup"><span data-stu-id="9ffb9-248">Select **Choose specific files**, expand the *dist/browser* folder, and select *signalr.js* and *signalr.min.js*.</span></span>

* <span data-ttu-id="9ffb9-249">Ustaw **lokalizację docelową** na plik *wwwroot/lib/sygnalizujący/* , a następnie wybierz pozycję **Zainstaluj**.</span><span class="sxs-lookup"><span data-stu-id="9ffb9-249">Set **Target Location** to *wwwroot/lib/signalr/*, and select **Install**.</span></span>

  ![Okno dialogowe Dodawanie biblioteki po stronie klienta — Wybieranie plików i lokalizacji docelowej](signalr/_static/2.x/libman2.png)

  <span data-ttu-id="9ffb9-251">LibMan tworzy folder *wwwroot/lib/sygnalizujący* i kopiuje do niego wybrane pliki.</span><span class="sxs-lookup"><span data-stu-id="9ffb9-251">LibMan creates a *wwwroot/lib/signalr* folder and copies the selected files to it.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="9ffb9-252">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="9ffb9-252">Visual Studio Code</span></span>](#tab/visual-studio-code/)

* <span data-ttu-id="9ffb9-253">W zintegrowanym terminalu uruchom następujące polecenie, aby zainstalować LibMan.</span><span class="sxs-lookup"><span data-stu-id="9ffb9-253">In the integrated terminal, run the following command to install LibMan.</span></span>

  ```dotnetcli
  dotnet tool install -g Microsoft.Web.LibraryManager.Cli
  ```

* <span data-ttu-id="9ffb9-254">Uruchom następujące polecenie, aby uzyskać bibliotekę klienta sygnalizującego za pomocą LibMan.</span><span class="sxs-lookup"><span data-stu-id="9ffb9-254">Run the following command to get the SignalR client library by using LibMan.</span></span> <span data-ttu-id="9ffb9-255">Może być konieczne odczekanie kilku sekund przed wyświetleniem danych wyjściowych.</span><span class="sxs-lookup"><span data-stu-id="9ffb9-255">You might have to wait a few seconds before seeing output.</span></span>

  ```console
  libman install @aspnet/signalr -p unpkg -d wwwroot/lib/signalr --files dist/browser/signalr.js --files dist/browser/signalr.min.js
  ```

  <span data-ttu-id="9ffb9-256">Parametry określają następujące opcje:</span><span class="sxs-lookup"><span data-stu-id="9ffb9-256">The parameters specify the following options:</span></span>
  * <span data-ttu-id="9ffb9-257">Użyj dostawcy unpkg.</span><span class="sxs-lookup"><span data-stu-id="9ffb9-257">Use the unpkg provider.</span></span>
  * <span data-ttu-id="9ffb9-258">Kopiuj pliki do lokalizacji *wwwroot/lib/sygnalizującej* .</span><span class="sxs-lookup"><span data-stu-id="9ffb9-258">Copy files to the *wwwroot/lib/signalr* destination.</span></span>
  * <span data-ttu-id="9ffb9-259">Skopiuj tylko określone pliki.</span><span class="sxs-lookup"><span data-stu-id="9ffb9-259">Copy only the specified files.</span></span>

  <span data-ttu-id="9ffb9-260">Dane wyjściowe wyglądają podobnie do poniższego przykładu:</span><span class="sxs-lookup"><span data-stu-id="9ffb9-260">The output looks like the following example:</span></span>

  ```console
  wwwroot/lib/signalr/dist/browser/signalr.js written to disk
  wwwroot/lib/signalr/dist/browser/signalr.min.js written to disk
  Installed library "@aspnet/signalr@1.0.3" to "wwwroot/lib/signalr"
  ```

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="9ffb9-261">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="9ffb9-261">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="9ffb9-262">W **terminalu**Uruchom następujące polecenie, aby zainstalować LibMan.</span><span class="sxs-lookup"><span data-stu-id="9ffb9-262">In the **Terminal**, run the following command to install LibMan.</span></span>

  ```dotnetcli
  dotnet tool install -g Microsoft.Web.LibraryManager.Cli
  ```

* <span data-ttu-id="9ffb9-263">Przejdź do folderu projektu (taki, który zawiera plik *SignalRChat. csproj* ).</span><span class="sxs-lookup"><span data-stu-id="9ffb9-263">Navigate to the project folder (the one that contains the *SignalRChat.csproj* file).</span></span>

* <span data-ttu-id="9ffb9-264">Uruchom następujące polecenie, aby uzyskać bibliotekę klienta sygnalizującego za pomocą LibMan.</span><span class="sxs-lookup"><span data-stu-id="9ffb9-264">Run the following command to get the SignalR client library by using LibMan.</span></span>

  ```console
  libman install @aspnet/signalr -p unpkg -d wwwroot/lib/signalr --files dist/browser/signalr.js --files dist/browser/signalr.min.js
  ```

  <span data-ttu-id="9ffb9-265">Parametry określają następujące opcje:</span><span class="sxs-lookup"><span data-stu-id="9ffb9-265">The parameters specify the following options:</span></span>
  * <span data-ttu-id="9ffb9-266">Użyj dostawcy unpkg.</span><span class="sxs-lookup"><span data-stu-id="9ffb9-266">Use the unpkg provider.</span></span>
  * <span data-ttu-id="9ffb9-267">Kopiuj pliki do lokalizacji *wwwroot/lib/sygnalizującej* .</span><span class="sxs-lookup"><span data-stu-id="9ffb9-267">Copy files to the *wwwroot/lib/signalr* destination.</span></span>
  * <span data-ttu-id="9ffb9-268">Skopiuj tylko określone pliki.</span><span class="sxs-lookup"><span data-stu-id="9ffb9-268">Copy only the specified files.</span></span>

  <span data-ttu-id="9ffb9-269">Dane wyjściowe wyglądają podobnie do poniższego przykładu:</span><span class="sxs-lookup"><span data-stu-id="9ffb9-269">The output looks like the following example:</span></span>

  ```console
  wwwroot/lib/signalr/dist/browser/signalr.js written to disk
  wwwroot/lib/signalr/dist/browser/signalr.min.js written to disk
  Installed library "@aspnet/signalr@1.0.3" to "wwwroot/lib/signalr"
  ```

---

## <a name="create-a-signalr-hub"></a><span data-ttu-id="9ffb9-270">Tworzenie centrum sygnałów</span><span class="sxs-lookup"><span data-stu-id="9ffb9-270">Create a SignalR hub</span></span>

<span data-ttu-id="9ffb9-271">*Koncentrator* jest klasą, która służy jako potok wysokiego poziomu, który obsługuje komunikację klient-serwer.</span><span class="sxs-lookup"><span data-stu-id="9ffb9-271">A *hub* is a class that serves as a high-level pipeline that handles client-server communication.</span></span>

* <span data-ttu-id="9ffb9-272">W folderze projektu SignalRChat Utwórz folder *Hubs* .</span><span class="sxs-lookup"><span data-stu-id="9ffb9-272">In the SignalRChat project folder, create a *Hubs* folder.</span></span>

* <span data-ttu-id="9ffb9-273">W folderze *Hubs* utwórz plik *ChatHub.cs* o następującym kodzie:</span><span class="sxs-lookup"><span data-stu-id="9ffb9-273">In the *Hubs* folder, create a *ChatHub.cs* file with the following code:</span></span>

  [!code-csharp[Startup](signalr/sample-snapshot/2.x/ChatHub.cs)]

  <span data-ttu-id="9ffb9-274">Klasa dziedziczy od `Hub` klasy sygnalizującej. `ChatHub`</span><span class="sxs-lookup"><span data-stu-id="9ffb9-274">The `ChatHub` class inherits from the SignalR `Hub` class.</span></span> <span data-ttu-id="9ffb9-275">`Hub` Klasa zarządza połączeniami, grupami i obsługą wiadomości.</span><span class="sxs-lookup"><span data-stu-id="9ffb9-275">The `Hub` class manages connections, groups, and messaging.</span></span>

  <span data-ttu-id="9ffb9-276">`SendMessage` Metoda może być wywoływana przez połączonego klienta w celu wysłania komunikatu do wszystkich klientów.</span><span class="sxs-lookup"><span data-stu-id="9ffb9-276">The `SendMessage` method can be called by a connected client to send a message to all clients.</span></span> <span data-ttu-id="9ffb9-277">Kod klienta JavaScript, który wywołuje metodę, jest wyświetlany w dalszej części samouczka.</span><span class="sxs-lookup"><span data-stu-id="9ffb9-277">JavaScript client code that calls the method is shown later in the tutorial.</span></span> <span data-ttu-id="9ffb9-278">Kod sygnalizujący jest asynchroniczny, aby zapewnić maksymalną skalowalność.</span><span class="sxs-lookup"><span data-stu-id="9ffb9-278">SignalR code is asynchronous to provide maximum scalability.</span></span>

## <a name="configure-signalr"></a><span data-ttu-id="9ffb9-279">Konfigurowanie sygnalizującego</span><span class="sxs-lookup"><span data-stu-id="9ffb9-279">Configure SignalR</span></span>

<span data-ttu-id="9ffb9-280">Serwer sygnalizujący musi być skonfigurowany tak, aby przekazywać żądania sygnałów do sygnalizującego.</span><span class="sxs-lookup"><span data-stu-id="9ffb9-280">The SignalR server must be configured to pass SignalR requests to SignalR.</span></span>

* <span data-ttu-id="9ffb9-281">Dodaj następujący wyróżniony kod do pliku *Startup.cs* .</span><span class="sxs-lookup"><span data-stu-id="9ffb9-281">Add the following highlighted code to the *Startup.cs* file.</span></span>

  [!code-csharp[Startup](signalr/sample-snapshot/2.x/Startup.cs?highlight=7,33,52-55)]

  <span data-ttu-id="9ffb9-282">Te zmiany umożliwiają dodanie sygnału do ASP.NET Core systemu iniekcji zależności oraz potoku oprogramowania pośredniczącego.</span><span class="sxs-lookup"><span data-stu-id="9ffb9-282">These changes add SignalR to the ASP.NET Core dependency injection system and the middleware pipeline.</span></span>

## <a name="add-signalr-client-code"></a><span data-ttu-id="9ffb9-283">Dodaj kod klienta sygnalizującego</span><span class="sxs-lookup"><span data-stu-id="9ffb9-283">Add SignalR client code</span></span>

* <span data-ttu-id="9ffb9-284">Zastąp zawartość w *Pages\Index.cshtml* następującym kodem:</span><span class="sxs-lookup"><span data-stu-id="9ffb9-284">Replace the content in *Pages\Index.cshtml* with the following code:</span></span>

  [!code-cshtml[Index](signalr/sample-snapshot/2.x/Index.cshtml)]

  <span data-ttu-id="9ffb9-285">Powyższy kod:</span><span class="sxs-lookup"><span data-stu-id="9ffb9-285">The preceding code:</span></span>

  * <span data-ttu-id="9ffb9-286">Tworzy pola tekstowe dla nazwy i tekstu komunikatu oraz przycisk Prześlij.</span><span class="sxs-lookup"><span data-stu-id="9ffb9-286">Creates text boxes for name and message text, and a submit button.</span></span>
  * <span data-ttu-id="9ffb9-287">Tworzy listę z `id="messagesList"` do wyświetlania komunikatów odbieranych z centrum sygnałów.</span><span class="sxs-lookup"><span data-stu-id="9ffb9-287">Creates a list with `id="messagesList"` for displaying messages that are received from the SignalR hub.</span></span>
  * <span data-ttu-id="9ffb9-288">Zawiera odwołania do skryptów do programu Sygnalizującer i kod aplikacji *czatu. js* , który tworzysz w następnym kroku.</span><span class="sxs-lookup"><span data-stu-id="9ffb9-288">Includes script references to SignalR and the *chat.js* application code that you create in the next step.</span></span>

* <span data-ttu-id="9ffb9-289">W folderze *wwwroot/js* Utwórz plik *czatu. js* o następującym kodzie:</span><span class="sxs-lookup"><span data-stu-id="9ffb9-289">In the *wwwroot/js* folder, create a *chat.js* file with the following code:</span></span>

  [!code-javascript[Index](signalr/sample-snapshot/2.x/chat.js)]

  <span data-ttu-id="9ffb9-290">Powyższy kod:</span><span class="sxs-lookup"><span data-stu-id="9ffb9-290">The preceding code:</span></span>

  * <span data-ttu-id="9ffb9-291">Tworzy i uruchamia połączenie.</span><span class="sxs-lookup"><span data-stu-id="9ffb9-291">Creates and starts a connection.</span></span>
  * <span data-ttu-id="9ffb9-292">Dodaje do przycisku Prześlij procedurę obsługi, która wysyła komunikaty do centrum.</span><span class="sxs-lookup"><span data-stu-id="9ffb9-292">Adds to the submit button a handler that sends messages to the hub.</span></span>
  * <span data-ttu-id="9ffb9-293">Dodaje do obiektu Connection program obsługi, który odbiera komunikaty z centrum i dodaje je do listy.</span><span class="sxs-lookup"><span data-stu-id="9ffb9-293">Adds to the connection object a handler that receives messages from the hub and adds them to the list.</span></span>

## <a name="run-the-app"></a><span data-ttu-id="9ffb9-294">Uruchamianie aplikacji</span><span class="sxs-lookup"><span data-stu-id="9ffb9-294">Run the app</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="9ffb9-295">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="9ffb9-295">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="9ffb9-296">Naciśnij **klawisze CTRL + F5** , aby uruchomić aplikację bez debugowania.</span><span class="sxs-lookup"><span data-stu-id="9ffb9-296">Press **CTRL+F5** to run the app without debugging.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="9ffb9-297">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="9ffb9-297">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="9ffb9-298">W zintegrowanym terminalu uruchom następujące polecenie:</span><span class="sxs-lookup"><span data-stu-id="9ffb9-298">In the integrated terminal, run the following command:</span></span>

  ```dotnetcli
  dotnet run -p SignalRChat.csproj
  ```

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="9ffb9-299">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="9ffb9-299">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="9ffb9-300">Z menu wybierz polecenie **uruchom > Uruchom bez debugowania**.</span><span class="sxs-lookup"><span data-stu-id="9ffb9-300">From the menu, select **Run > Start Without Debugging**.</span></span>

---

* <span data-ttu-id="9ffb9-301">Skopiuj adres URL z paska adresu, Otwórz inne wystąpienie przeglądarki lub kartę, a następnie wklej adres URL na pasku adresu.</span><span class="sxs-lookup"><span data-stu-id="9ffb9-301">Copy the URL from the address bar, open another browser instance or tab, and paste the URL in the address bar.</span></span>

* <span data-ttu-id="9ffb9-302">Wybierz opcję przeglądarka, wprowadź nazwę i komunikat, a następnie wybierz przycisk **Wyślij wiadomość** .</span><span class="sxs-lookup"><span data-stu-id="9ffb9-302">Choose either browser, enter a name and message, and select the **Send Message** button.</span></span>

  <span data-ttu-id="9ffb9-303">Nazwa i komunikat są natychmiast wyświetlane na obu stronach.</span><span class="sxs-lookup"><span data-stu-id="9ffb9-303">The name and message are displayed on both pages instantly.</span></span>

  ![Przykładowa aplikacja sygnalizująca](signalr/_static/2.x/signalr-get-started-finished.png)

> [!TIP]
> <span data-ttu-id="9ffb9-305">Jeśli aplikacja nie działa, Otwórz narzędzia deweloperskie przeglądarki (F12) i przejdź do konsoli programu.</span><span class="sxs-lookup"><span data-stu-id="9ffb9-305">If the app doesn't work, open your browser developer tools (F12) and go to the console.</span></span> <span data-ttu-id="9ffb9-306">Mogą pojawić się błędy związane z kodem HTML i JavaScript.</span><span class="sxs-lookup"><span data-stu-id="9ffb9-306">You might see errors related to your HTML and JavaScript code.</span></span> <span data-ttu-id="9ffb9-307">Załóżmy na przykład, że umieścisz polecenie *signaler. js* w innym folderze niż skierowany.</span><span class="sxs-lookup"><span data-stu-id="9ffb9-307">For example, suppose you put *signalr.js* in a different folder than directed.</span></span> <span data-ttu-id="9ffb9-308">W takim przypadku odwołanie do tego pliku nie będzie działało i zobaczysz błąd 404 w konsoli.</span><span class="sxs-lookup"><span data-stu-id="9ffb9-308">In that case the reference to that file won't work and you'll see a 404 error in the console.</span></span>
> <span data-ttu-id="9ffb9-309">![błąd podczas znajdowania sygnalizującer. js](signalr/_static/2.x/f12-console.png)</span><span class="sxs-lookup"><span data-stu-id="9ffb9-309">![signalr.js not found error](signalr/_static/2.x/f12-console.png)</span></span>

## <a name="next-steps"></a><span data-ttu-id="9ffb9-310">Następne kroki</span><span class="sxs-lookup"><span data-stu-id="9ffb9-310">Next steps</span></span>

<span data-ttu-id="9ffb9-311">W niniejszym samouczku zawarto informacje na temat wykonywania następujących czynności:</span><span class="sxs-lookup"><span data-stu-id="9ffb9-311">In this tutorial, you learned how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="9ffb9-312">Utwórz projekt aplikacji sieci Web.</span><span class="sxs-lookup"><span data-stu-id="9ffb9-312">Create a web app project.</span></span>
> * <span data-ttu-id="9ffb9-313">Dodaj bibliotekę klienta sygnalizującego.</span><span class="sxs-lookup"><span data-stu-id="9ffb9-313">Add the SignalR client library.</span></span>
> * <span data-ttu-id="9ffb9-314">Utwórz centrum sygnałów.</span><span class="sxs-lookup"><span data-stu-id="9ffb9-314">Create a SignalR hub.</span></span>
> * <span data-ttu-id="9ffb9-315">Skonfiguruj projekt do używania sygnalizującego.</span><span class="sxs-lookup"><span data-stu-id="9ffb9-315">Configure the project to use SignalR.</span></span>
> * <span data-ttu-id="9ffb9-316">Dodaj kod używający centrum do wysyłania komunikatów z dowolnego klienta do wszystkich podłączonych klientów.</span><span class="sxs-lookup"><span data-stu-id="9ffb9-316">Add code that uses the hub to send messages from any client to all connected clients.</span></span>

<span data-ttu-id="9ffb9-317">Aby dowiedzieć się więcej na temat sygnalizacji, zobacz Wprowadzenie:</span><span class="sxs-lookup"><span data-stu-id="9ffb9-317">To learn more about SignalR, see the introduction:</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="9ffb9-318">Wprowadzenie do ASP.NET Core sygnalizującego</span><span class="sxs-lookup"><span data-stu-id="9ffb9-318">Introduction to ASP.NET Core SignalR</span></span>](xref:signalr/introduction)

::: moniker-end
