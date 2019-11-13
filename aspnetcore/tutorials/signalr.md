---
title: Wprowadzenie do ASP.NET Core SignalR
author: bradygaster
description: W tym samouczku utworzysz aplikację czatu korzystającą SignalRASP.NET Core.
ms.author: bradyg
ms.custom: mvc
ms.date: 11/12/2019
no-loc:
- SignalR
uid: tutorials/signalr
ms.openlocfilehash: 962cc0318ebbfc7fac16ca0947a2e3e83e51665c
ms.sourcegitcommit: 3fc3020961e1289ee5bf5f3c365ce8304d8ebf19
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/12/2019
ms.locfileid: "73964030"
---
# <a name="tutorial-get-started-with-aspnet-core-opno-locsignalr"></a><span data-ttu-id="2bb3e-103">Samouczek: Rozpoczynanie pracy z ASP.NET Core SignalR</span><span class="sxs-lookup"><span data-stu-id="2bb3e-103">Tutorial: Get started with ASP.NET Core SignalR</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="2bb3e-104">Ten samouczek uczy się podstaw tworzenia aplikacji w czasie rzeczywistym przy użyciu SignalR.</span><span class="sxs-lookup"><span data-stu-id="2bb3e-104">This tutorial teaches the basics of building a real-time app using SignalR.</span></span> <span data-ttu-id="2bb3e-105">Dowiesz się, jak:</span><span class="sxs-lookup"><span data-stu-id="2bb3e-105">You learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="2bb3e-106">Utwórz projekt sieci Web.</span><span class="sxs-lookup"><span data-stu-id="2bb3e-106">Create a web project.</span></span>
> * <span data-ttu-id="2bb3e-107">Dodaj SignalRą bibliotekę kliencką.</span><span class="sxs-lookup"><span data-stu-id="2bb3e-107">Add the SignalR client library.</span></span>
> * <span data-ttu-id="2bb3e-108">Utwórz centrum SignalR.</span><span class="sxs-lookup"><span data-stu-id="2bb3e-108">Create a SignalR hub.</span></span>
> * <span data-ttu-id="2bb3e-109">Skonfiguruj projekt do użycia SignalR.</span><span class="sxs-lookup"><span data-stu-id="2bb3e-109">Configure the project to use SignalR.</span></span>
> * <span data-ttu-id="2bb3e-110">Dodaj kod, który wysyła komunikaty z dowolnego klienta do wszystkich połączonych klientów.</span><span class="sxs-lookup"><span data-stu-id="2bb3e-110">Add code that sends messages from any client to all connected clients.</span></span>

<span data-ttu-id="2bb3e-111">Na końcu będziesz mieć działającą aplikację czatu:</span><span class="sxs-lookup"><span data-stu-id="2bb3e-111">At the end, you'll have a working chat app:</span></span>

![[! OP. Aplikacja Przykładowa NO-LOC (sygnalizująca)]](signalr/_static/3.x/signalr-get-started-finished.png)

## <a name="prerequisites"></a><span data-ttu-id="2bb3e-113">Wymagania wstępne</span><span class="sxs-lookup"><span data-stu-id="2bb3e-113">Prerequisites</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="2bb3e-114">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="2bb3e-114">Visual Studio</span></span>](#tab/visual-studio)

[!INCLUDE[](~/includes/net-core-prereqs-vs-3.0.md)]

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="2bb3e-115">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="2bb3e-115">Visual Studio Code</span></span>](#tab/visual-studio-code)

[!INCLUDE[](~/includes/net-core-prereqs-vsc-3.0.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="2bb3e-116">Visual Studio dla komputerów Mac</span><span class="sxs-lookup"><span data-stu-id="2bb3e-116">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

[!INCLUDE[](~/includes/net-core-prereqs-mac-3.0.md)]

---

## <a name="create-a-web-app-project"></a><span data-ttu-id="2bb3e-117">Tworzenie projektu aplikacji sieci Web</span><span class="sxs-lookup"><span data-stu-id="2bb3e-117">Create a web app project</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="2bb3e-118">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="2bb3e-118">Visual Studio</span></span>](#tab/visual-studio/)

* <span data-ttu-id="2bb3e-119">Z menu wybierz pozycję **plik > nowy projekt**.</span><span class="sxs-lookup"><span data-stu-id="2bb3e-119">From the menu, select **File > New Project**.</span></span>

* <span data-ttu-id="2bb3e-120">W oknie dialogowym **Tworzenie nowego projektu** wybierz pozycję **ASP.NET Core aplikacja sieci Web**, a następnie wybierz przycisk **dalej**.</span><span class="sxs-lookup"><span data-stu-id="2bb3e-120">In the **Create a new project** dialog, select **ASP.NET Core Web Application**, and then select **Next**.</span></span>

* <span data-ttu-id="2bb3e-121">W oknie dialogowym **Konfigurowanie nowego projektu** Nazwij projekt *SignalRChat*, a następnie wybierz pozycję **Utwórz**.</span><span class="sxs-lookup"><span data-stu-id="2bb3e-121">In the **Configure your new project** dialog, name the project *SignalRChat*, and then select **Create**.</span></span>

* <span data-ttu-id="2bb3e-122">W oknie dialogowym **Tworzenie nowej ASP.NET Core aplikacji sieci Web** wybierz pozycję **.net Core** i **ASP.NET Core 3,0**.</span><span class="sxs-lookup"><span data-stu-id="2bb3e-122">In the **Create a new ASP.NET Core web Application** dialog, select **.NET Core** and **ASP.NET Core 3.0**.</span></span> 

* <span data-ttu-id="2bb3e-123">Wybierz pozycję **aplikacja sieci Web** , aby utworzyć projekt, który używa Razor Pages, a następnie wybierz pozycję **Utwórz**.</span><span class="sxs-lookup"><span data-stu-id="2bb3e-123">Select **Web Application** to create a project that uses Razor Pages, and then select **Create**.</span></span>

  ![Okno dialogowe nowego projektu w programie Visual Studio](signalr/_static/3.x/signalr-new-project-dialog.png)

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="2bb3e-125">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="2bb3e-125">Visual Studio Code</span></span>](#tab/visual-studio-code/)

* <span data-ttu-id="2bb3e-126">Otwórz [zintegrowany terminal](https://code.visualstudio.com/docs/editor/integrated-terminal) do folderu, w którym zostanie utworzony nowy folder projektu.</span><span class="sxs-lookup"><span data-stu-id="2bb3e-126">Open the [integrated terminal](https://code.visualstudio.com/docs/editor/integrated-terminal) to the folder in which the new project folder will be created.</span></span>

* <span data-ttu-id="2bb3e-127">Uruchom następujące polecenia:</span><span class="sxs-lookup"><span data-stu-id="2bb3e-127">Run the following commands:</span></span>

   ```dotnetcli
   dotnet new webapp -o SignalRChat
   code -r SignalRChat
   ```

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="2bb3e-128">Visual Studio dla komputerów Mac</span><span class="sxs-lookup"><span data-stu-id="2bb3e-128">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="2bb3e-129">Z menu wybierz pozycję **plik > nowe rozwiązanie**.</span><span class="sxs-lookup"><span data-stu-id="2bb3e-129">From the menu, select **File > New Solution**.</span></span>

* <span data-ttu-id="2bb3e-130">Wybierz pozycję **.NET Core > App > aplikacji sieci Web** (nie zaznaczaj **aplikacji sieci Web (Model-View-Controller)** ), a następnie wybierz przycisk **dalej**.</span><span class="sxs-lookup"><span data-stu-id="2bb3e-130">Select **.NET Core > App > Web Application** (Don't select **Web Application (Model-View-Controller)**), and then select **Next**.</span></span>

* <span data-ttu-id="2bb3e-131">Upewnij się, że **platforma docelowa** jest ustawiona na **platformę .NET Core 3,0**, a następnie wybierz przycisk **dalej**.</span><span class="sxs-lookup"><span data-stu-id="2bb3e-131">Make sure the **Target Framework** is set to **.NET Core 3.0**, and then select **Next**.</span></span>

* <span data-ttu-id="2bb3e-132">Nazwij projekt *SignalRChat*, a następnie wybierz pozycję **Utwórz**.</span><span class="sxs-lookup"><span data-stu-id="2bb3e-132">Name the project *SignalRChat*, and then select **Create**.</span></span>

---

## <a name="add-the-opno-locsignalr-client-library"></a><span data-ttu-id="2bb3e-133">Dodawanie SignalRej biblioteki klienta</span><span class="sxs-lookup"><span data-stu-id="2bb3e-133">Add the SignalR client library</span></span>

<span data-ttu-id="2bb3e-134">Biblioteka SignalR Server jest dołączona do udostępnionej struktury ASP.NET Core 3,0.</span><span class="sxs-lookup"><span data-stu-id="2bb3e-134">The SignalR server library is included in the ASP.NET Core 3.0 shared framework.</span></span> <span data-ttu-id="2bb3e-135">Biblioteka klienta JavaScript nie jest automatycznie dołączana do projektu.</span><span class="sxs-lookup"><span data-stu-id="2bb3e-135">The JavaScript client library isn't automatically included in the project.</span></span> <span data-ttu-id="2bb3e-136">W tym samouczku użyjesz programu Library Manager (LibMan), aby uzyskać bibliotekę kliencką z *unpkg*.</span><span class="sxs-lookup"><span data-stu-id="2bb3e-136">For this tutorial, you use Library Manager (LibMan) to get the client library from *unpkg*.</span></span> <span data-ttu-id="2bb3e-137">unpkg to usługa Content Delivery Network (CDN), która umożliwia dostarczanie elementów znalezionych w programie npm, w Menedżerze pakietów środowiska Node. js.</span><span class="sxs-lookup"><span data-stu-id="2bb3e-137">unpkg is a content delivery network (CDN)) that can deliver anything found in npm, the Node.js package manager.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="2bb3e-138">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="2bb3e-138">Visual Studio</span></span>](#tab/visual-studio/)

* <span data-ttu-id="2bb3e-139">W **Eksplorator rozwiązań**kliknij prawym przyciskiem myszy projekt, a następnie wybierz polecenie **Dodaj** > **biblioteki po stronie klienta**.</span><span class="sxs-lookup"><span data-stu-id="2bb3e-139">In **Solution Explorer**, right-click the project, and select **Add** > **Client-Side Library**.</span></span>

* <span data-ttu-id="2bb3e-140">W oknie dialogowym **Dodawanie biblioteki po stronie klienta** dla **dostawcy** wybierz pozycję **unpkg**.</span><span class="sxs-lookup"><span data-stu-id="2bb3e-140">In the **Add Client-Side Library** dialog, for **Provider** select **unpkg**.</span></span>

* <span data-ttu-id="2bb3e-141">W obszarze **Biblioteka**wprowadź `@microsoft/signalr@latest`.</span><span class="sxs-lookup"><span data-stu-id="2bb3e-141">For **Library**, enter `@microsoft/signalr@latest`.</span></span>

* <span data-ttu-id="2bb3e-142">Wybierz pozycję **Wybierz określone pliki**, rozwiń folder *dist/przeglądarka* , a następnie wybierz pozycję *sygnalizujące. js* i *sygnalizujący. min. js*.</span><span class="sxs-lookup"><span data-stu-id="2bb3e-142">Select **Choose specific files**, expand the *dist/browser* folder, and select *signalr.js* and *signalr.min.js*.</span></span>

* <span data-ttu-id="2bb3e-143">Ustaw **lokalizację docelową** na plik *wwwroot/js/signaler/* , a następnie wybierz pozycję **Zainstaluj**.</span><span class="sxs-lookup"><span data-stu-id="2bb3e-143">Set **Target Location** to *wwwroot/js/signalr/*, and select **Install**.</span></span>

  ![Okno dialogowe Dodawanie biblioteki po stronie klienta — wybór biblioteki](signalr/_static/3.x/find-signalr-client-libs-select-files.png)

  <span data-ttu-id="2bb3e-145">LibMan tworzy folder *wwwroot/js/sygnalizujący* i kopiuje do niego wybrane pliki.</span><span class="sxs-lookup"><span data-stu-id="2bb3e-145">LibMan creates a *wwwroot/js/signalr* folder and copies the selected files to it.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="2bb3e-146">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="2bb3e-146">Visual Studio Code</span></span>](#tab/visual-studio-code/)

* <span data-ttu-id="2bb3e-147">W zintegrowanym terminalu uruchom następujące polecenie, aby zainstalować LibMan.</span><span class="sxs-lookup"><span data-stu-id="2bb3e-147">In the integrated terminal, run the following command to install LibMan.</span></span>

  ```dotnetcli
  dotnet tool install -g Microsoft.Web.LibraryManager.Cli
  ```

* <span data-ttu-id="2bb3e-148">Uruchom następujące polecenie, aby uzyskać SignalR bibliotekę kliencką przy użyciu LibMan.</span><span class="sxs-lookup"><span data-stu-id="2bb3e-148">Run the following command to get the SignalR client library by using LibMan.</span></span> <span data-ttu-id="2bb3e-149">Może być konieczne odczekanie kilku sekund przed wyświetleniem danych wyjściowych.</span><span class="sxs-lookup"><span data-stu-id="2bb3e-149">You might have to wait a few seconds before seeing output.</span></span>

  ```console
  libman install @microsoft/signalr@latest -p unpkg -d wwwroot/js/signalr --files dist/browser/signalr.js --files dist/browser/signalr.min.js
  ```

  <span data-ttu-id="2bb3e-150">Parametry określają następujące opcje:</span><span class="sxs-lookup"><span data-stu-id="2bb3e-150">The parameters specify the following options:</span></span>
  * <span data-ttu-id="2bb3e-151">Użyj dostawcy unpkg.</span><span class="sxs-lookup"><span data-stu-id="2bb3e-151">Use the unpkg provider.</span></span>
  * <span data-ttu-id="2bb3e-152">Kopiuj pliki do lokalizacji *wwwroot/js/sygnalizującer* .</span><span class="sxs-lookup"><span data-stu-id="2bb3e-152">Copy files to the *wwwroot/js/signalr* destination.</span></span>
  * <span data-ttu-id="2bb3e-153">Skopiuj tylko określone pliki.</span><span class="sxs-lookup"><span data-stu-id="2bb3e-153">Copy only the specified files.</span></span>

  <span data-ttu-id="2bb3e-154">Dane wyjściowe wyglądają podobnie do poniższego przykładu:</span><span class="sxs-lookup"><span data-stu-id="2bb3e-154">The output looks like the following example:</span></span>

  ```console
  wwwroot/js/signalr/dist/browser/signalr.js written to disk
  wwwroot/js/signalr/dist/browser/signalr.min.js written to disk
  Installed library "@microsoft/signalr@latest" to "wwwroot/js/signalr"
  ```

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="2bb3e-155">Visual Studio dla komputerów Mac</span><span class="sxs-lookup"><span data-stu-id="2bb3e-155">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="2bb3e-156">W **terminalu**Uruchom następujące polecenie, aby zainstalować LibMan.</span><span class="sxs-lookup"><span data-stu-id="2bb3e-156">In the **Terminal**, run the following command to install LibMan.</span></span>

  ```dotnetcli
  dotnet tool install -g Microsoft.Web.LibraryManager.Cli
  ```

* <span data-ttu-id="2bb3e-157">Przejdź do folderu projektu (taki, który zawiera plik *SignalRChat. csproj* ).</span><span class="sxs-lookup"><span data-stu-id="2bb3e-157">Navigate to the project folder (the one that contains the *SignalRChat.csproj* file).</span></span>

* <span data-ttu-id="2bb3e-158">Uruchom następujące polecenie, aby uzyskać SignalR bibliotekę kliencką przy użyciu LibMan.</span><span class="sxs-lookup"><span data-stu-id="2bb3e-158">Run the following command to get the SignalR client library by using LibMan.</span></span>

  ```console
  libman install @microsoft/signalr@latest -p unpkg -d wwwroot/js/signalr --files dist/browser/signalr.js --files dist/browser/signalr.min.js
  ```

  <span data-ttu-id="2bb3e-159">Parametry określają następujące opcje:</span><span class="sxs-lookup"><span data-stu-id="2bb3e-159">The parameters specify the following options:</span></span>
  * <span data-ttu-id="2bb3e-160">Użyj dostawcy unpkg.</span><span class="sxs-lookup"><span data-stu-id="2bb3e-160">Use the unpkg provider.</span></span>
  * <span data-ttu-id="2bb3e-161">Kopiuj pliki do lokalizacji *wwwroot/js/sygnalizującer* .</span><span class="sxs-lookup"><span data-stu-id="2bb3e-161">Copy files to the *wwwroot/js/signalr* destination.</span></span>
  * <span data-ttu-id="2bb3e-162">Skopiuj tylko określone pliki.</span><span class="sxs-lookup"><span data-stu-id="2bb3e-162">Copy only the specified files.</span></span>

  <span data-ttu-id="2bb3e-163">Dane wyjściowe wyglądają podobnie do poniższego przykładu:</span><span class="sxs-lookup"><span data-stu-id="2bb3e-163">The output looks like the following example:</span></span>

  ```console
  wwwroot/js/signalr/dist/browser/signalr.js written to disk
  wwwroot/js/signalr/dist/browser/signalr.min.js written to disk
  Installed library "@microsoft/signalr@latest" to "wwwroot/js/signalr"
  ```

---

## <a name="create-a-opno-locsignalr-hub"></a><span data-ttu-id="2bb3e-164">Tworzenie centrum SignalR</span><span class="sxs-lookup"><span data-stu-id="2bb3e-164">Create a SignalR hub</span></span>

<span data-ttu-id="2bb3e-165">*Koncentrator* jest klasą, która służy jako potok wysokiego poziomu, który obsługuje komunikację klient-serwer.</span><span class="sxs-lookup"><span data-stu-id="2bb3e-165">A *hub* is a class that serves as a high-level pipeline that handles client-server communication.</span></span>

* <span data-ttu-id="2bb3e-166">W folderze projektu SignalRChat Utwórz folder *Hubs* .</span><span class="sxs-lookup"><span data-stu-id="2bb3e-166">In the SignalRChat project folder, create a *Hubs* folder.</span></span>

* <span data-ttu-id="2bb3e-167">W folderze *Hubs* utwórz plik *ChatHub.cs* o następującym kodzie:</span><span class="sxs-lookup"><span data-stu-id="2bb3e-167">In the *Hubs* folder, create a *ChatHub.cs* file with the following code:</span></span>

  [!code-csharp[ChatHub](signalr/sample-snapshot/3.x/ChatHub.cs)]

  <span data-ttu-id="2bb3e-168">Klasa `ChatHub` dziedziczy z klasy SignalR `Hub`.</span><span class="sxs-lookup"><span data-stu-id="2bb3e-168">The `ChatHub` class inherits from the SignalR `Hub` class.</span></span> <span data-ttu-id="2bb3e-169">Klasa `Hub` zarządza połączeniami, grupami i obsługą komunikatów.</span><span class="sxs-lookup"><span data-stu-id="2bb3e-169">The `Hub` class manages connections, groups, and messaging.</span></span>

  <span data-ttu-id="2bb3e-170">Metoda `SendMessage` może być wywoływana przez połączonego klienta w celu wysłania komunikatu do wszystkich klientów.</span><span class="sxs-lookup"><span data-stu-id="2bb3e-170">The `SendMessage` method can be called by a connected client to send a message to all clients.</span></span> <span data-ttu-id="2bb3e-171">Kod klienta JavaScript, który wywołuje metodę, jest wyświetlany w dalszej części samouczka.</span><span class="sxs-lookup"><span data-stu-id="2bb3e-171">JavaScript client code that calls the method is shown later in the tutorial.</span></span> <span data-ttu-id="2bb3e-172">kod SignalR jest asynchroniczny w celu zapewnienia maksymalnej skalowalności.</span><span class="sxs-lookup"><span data-stu-id="2bb3e-172">SignalR code is asynchronous to provide maximum scalability.</span></span>

## <a name="configure-opno-locsignalr"></a><span data-ttu-id="2bb3e-173">Konfigurowanie SignalR</span><span class="sxs-lookup"><span data-stu-id="2bb3e-173">Configure SignalR</span></span>

<span data-ttu-id="2bb3e-174">Serwer SignalR musi być skonfigurowany do przekazywania żądań SignalR do SignalR.</span><span class="sxs-lookup"><span data-stu-id="2bb3e-174">The SignalR server must be configured to pass SignalR requests to SignalR.</span></span>

* <span data-ttu-id="2bb3e-175">Dodaj następujący wyróżniony kod do pliku *Startup.cs* .</span><span class="sxs-lookup"><span data-stu-id="2bb3e-175">Add the following highlighted code to the *Startup.cs* file.</span></span>

  [!code-csharp[Startup](signalr/sample-snapshot/3.x/Startup.cs?highlight=11,28,55)]

  <span data-ttu-id="2bb3e-176">Te zmiany dodają SignalR do ASP.NET Corenia zależności i systemów routingu.</span><span class="sxs-lookup"><span data-stu-id="2bb3e-176">These changes add SignalR to the ASP.NET Core dependency injection and routing systems.</span></span>

## <a name="add-opno-locsignalr-client-code"></a><span data-ttu-id="2bb3e-177">Dodawanie kodu klienta SignalR</span><span class="sxs-lookup"><span data-stu-id="2bb3e-177">Add SignalR client code</span></span>

* <span data-ttu-id="2bb3e-178">Zastąp zawartość w *Pages\Index.cshtml* następującym kodem:</span><span class="sxs-lookup"><span data-stu-id="2bb3e-178">Replace the content in *Pages\Index.cshtml* with the following code:</span></span>

  [!code-cshtml[Index](signalr/sample-snapshot/3.x/Index.cshtml)]

  <span data-ttu-id="2bb3e-179">Poprzedni kod:</span><span class="sxs-lookup"><span data-stu-id="2bb3e-179">The preceding code:</span></span>

  * <span data-ttu-id="2bb3e-180">Tworzy pola tekstowe dla nazwy i tekstu komunikatu oraz przycisk Prześlij.</span><span class="sxs-lookup"><span data-stu-id="2bb3e-180">Creates text boxes for name and message text, and a submit button.</span></span>
  * <span data-ttu-id="2bb3e-181">Tworzy listę z `id="messagesList"` do wyświetlania komunikatów odebranych z centrum SignalR.</span><span class="sxs-lookup"><span data-stu-id="2bb3e-181">Creates a list with `id="messagesList"` for displaying messages that are received from the SignalR hub.</span></span>
  * <span data-ttu-id="2bb3e-182">Zawiera odwołania do skryptów do SignalR i kod aplikacji *czatu. js* , który tworzysz w następnym kroku.</span><span class="sxs-lookup"><span data-stu-id="2bb3e-182">Includes script references to SignalR and the *chat.js* application code that you create in the next step.</span></span>

* <span data-ttu-id="2bb3e-183">W folderze *wwwroot/js* Utwórz plik *czatu. js* o następującym kodzie:</span><span class="sxs-lookup"><span data-stu-id="2bb3e-183">In the *wwwroot/js* folder, create a *chat.js* file with the following code:</span></span>

  [!code-javascript[chat](signalr/sample-snapshot/3.x/chat.js)]

  <span data-ttu-id="2bb3e-184">Poprzedni kod:</span><span class="sxs-lookup"><span data-stu-id="2bb3e-184">The preceding code:</span></span>

  * <span data-ttu-id="2bb3e-185">Tworzy i uruchamia połączenie.</span><span class="sxs-lookup"><span data-stu-id="2bb3e-185">Creates and starts a connection.</span></span>
  * <span data-ttu-id="2bb3e-186">Dodaje do przycisku Prześlij procedurę obsługi, która wysyła komunikaty do centrum.</span><span class="sxs-lookup"><span data-stu-id="2bb3e-186">Adds to the submit button a handler that sends messages to the hub.</span></span>
  * <span data-ttu-id="2bb3e-187">Dodaje do obiektu Connection program obsługi, który odbiera komunikaty z centrum i dodaje je do listy.</span><span class="sxs-lookup"><span data-stu-id="2bb3e-187">Adds to the connection object a handler that receives messages from the hub and adds them to the list.</span></span>

## <a name="run-the-app"></a><span data-ttu-id="2bb3e-188">Uruchamianie aplikacji</span><span class="sxs-lookup"><span data-stu-id="2bb3e-188">Run the app</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="2bb3e-189">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="2bb3e-189">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="2bb3e-190">Naciśnij **klawisze CTRL + F5** , aby uruchomić aplikację bez debugowania.</span><span class="sxs-lookup"><span data-stu-id="2bb3e-190">Press **CTRL+F5** to run the app without debugging.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="2bb3e-191">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="2bb3e-191">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="2bb3e-192">W zintegrowanym terminalu uruchom następujące polecenie:</span><span class="sxs-lookup"><span data-stu-id="2bb3e-192">In the integrated terminal, run the following command:</span></span>

  ```dotnetcli
  dotnet watch run -p SignalRChat.csproj
  ```

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="2bb3e-193">Visual Studio dla komputerów Mac</span><span class="sxs-lookup"><span data-stu-id="2bb3e-193">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="2bb3e-194">Z menu wybierz polecenie **uruchom > Uruchom bez debugowania**.</span><span class="sxs-lookup"><span data-stu-id="2bb3e-194">From the menu, select **Run > Start Without Debugging**.</span></span>

---

* <span data-ttu-id="2bb3e-195">Skopiuj adres URL z paska adresu, Otwórz inne wystąpienie przeglądarki lub kartę, a następnie wklej adres URL na pasku adresu.</span><span class="sxs-lookup"><span data-stu-id="2bb3e-195">Copy the URL from the address bar, open another browser instance or tab, and paste the URL in the address bar.</span></span>

* <span data-ttu-id="2bb3e-196">Wybierz opcję przeglądarka, wprowadź nazwę i komunikat, a następnie wybierz przycisk **Wyślij wiadomość** .</span><span class="sxs-lookup"><span data-stu-id="2bb3e-196">Choose either browser, enter a name and message, and select the **Send Message** button.</span></span>

  <span data-ttu-id="2bb3e-197">Nazwa i komunikat są natychmiast wyświetlane na obu stronach.</span><span class="sxs-lookup"><span data-stu-id="2bb3e-197">The name and message are displayed on both pages instantly.</span></span>

  ![[! OP. Aplikacja Przykładowa NO-LOC (sygnalizująca)]](signalr/_static/3.x/signalr-get-started-finished.png)

> [!TIP]
> * <span data-ttu-id="2bb3e-199">Jeśli aplikacja nie działa, Otwórz narzędzia deweloperskie przeglądarki (F12) i przejdź do konsoli programu.</span><span class="sxs-lookup"><span data-stu-id="2bb3e-199">If the app doesn't work, open your browser developer tools (F12) and go to the console.</span></span> <span data-ttu-id="2bb3e-200">Mogą pojawić się błędy związane z kodem HTML i JavaScript.</span><span class="sxs-lookup"><span data-stu-id="2bb3e-200">You might see errors related to your HTML and JavaScript code.</span></span> <span data-ttu-id="2bb3e-201">Załóżmy na przykład, że umieścisz polecenie *signaler. js* w innym folderze niż skierowany.</span><span class="sxs-lookup"><span data-stu-id="2bb3e-201">For example, suppose you put *signalr.js* in a different folder than directed.</span></span> <span data-ttu-id="2bb3e-202">W takim przypadku odwołanie do tego pliku nie będzie działało i zobaczysz błąd 404 w konsoli.</span><span class="sxs-lookup"><span data-stu-id="2bb3e-202">In that case the reference to that file won't work and you'll see a 404 error in the console.</span></span>
>   <span data-ttu-id="2bb3e-203">Wystąpił błąd ![sygnalizującer. nie znaleziono błędu](signalr/_static/3.x/f12-console.png)</span><span class="sxs-lookup"><span data-stu-id="2bb3e-203">![signalr.js not found error](signalr/_static/3.x/f12-console.png)</span></span>
> * <span data-ttu-id="2bb3e-204">Jeśli zostanie wyświetlony komunikat o błędzie ERR_SPDY_INADEQUATE_TRANSPORT_SECURITY w programie Chrome, uruchom następujące polecenia, aby zaktualizować certyfikat deweloperski:</span><span class="sxs-lookup"><span data-stu-id="2bb3e-204">If you get the error ERR_SPDY_INADEQUATE_TRANSPORT_SECURITY in Chrome, run these commands to update your development certificate:</span></span>
>
>   ```dotnetcli
>   dotnet dev-certs https --clean
>   dotnet dev-certs https --trust
>   ```

## <a name="next-steps"></a><span data-ttu-id="2bb3e-205">Następne kroki</span><span class="sxs-lookup"><span data-stu-id="2bb3e-205">Next steps</span></span>

<span data-ttu-id="2bb3e-206">Aby dowiedzieć się więcej na temat SignalR, zobacz Wprowadzenie:</span><span class="sxs-lookup"><span data-stu-id="2bb3e-206">To learn more about SignalR, see the introduction:</span></span>

> [!div class="nextstepaction"]
> <span data-ttu-id="2bb3e-207">[Wprowadzenie do ASP.NET Core SignalR](xref:signalr/introduction)</span><span class="sxs-lookup"><span data-stu-id="2bb3e-207">[Introduction to ASP.NET Core SignalR](xref:signalr/introduction)</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="2bb3e-208">Ten samouczek uczy się podstaw tworzenia aplikacji w czasie rzeczywistym przy użyciu SignalR.</span><span class="sxs-lookup"><span data-stu-id="2bb3e-208">This tutorial teaches the basics of building a real-time app using SignalR.</span></span> <span data-ttu-id="2bb3e-209">Dowiesz się, jak:</span><span class="sxs-lookup"><span data-stu-id="2bb3e-209">You learn how to:</span></span> 

> [!div class="checklist"]  
> * <span data-ttu-id="2bb3e-210">Utwórz projekt sieci Web.</span><span class="sxs-lookup"><span data-stu-id="2bb3e-210">Create a web project.</span></span>   
> * <span data-ttu-id="2bb3e-211">Dodaj SignalRą bibliotekę kliencką.</span><span class="sxs-lookup"><span data-stu-id="2bb3e-211">Add the SignalR client library.</span></span>   
> * <span data-ttu-id="2bb3e-212">Utwórz centrum SignalR.</span><span class="sxs-lookup"><span data-stu-id="2bb3e-212">Create a SignalR hub.</span></span> 
> * <span data-ttu-id="2bb3e-213">Skonfiguruj projekt do użycia SignalR.</span><span class="sxs-lookup"><span data-stu-id="2bb3e-213">Configure the project to use SignalR.</span></span> 
> * <span data-ttu-id="2bb3e-214">Dodaj kod, który wysyła komunikaty z dowolnego klienta do wszystkich połączonych klientów.</span><span class="sxs-lookup"><span data-stu-id="2bb3e-214">Add code that sends messages from any client to all connected clients.</span></span>  
<span data-ttu-id="2bb3e-215">Na końcu będziesz mieć działającą aplikację czatu: ![[! OP. NO-LOC (Signaler)] Przykładowa aplikacja](signalr/_static/2.x/signalr-get-started-finished.png)</span><span class="sxs-lookup"><span data-stu-id="2bb3e-215">At the end, you'll have a working chat app: ![SignalR sample app](signalr/_static/2.x/signalr-get-started-finished.png)</span></span>   

## <a name="prerequisites"></a><span data-ttu-id="2bb3e-216">Wymagania wstępne</span><span class="sxs-lookup"><span data-stu-id="2bb3e-216">Prerequisites</span></span>    

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="2bb3e-217">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="2bb3e-217">Visual Studio</span></span>](#tab/visual-studio)   

[!INCLUDE[](~/includes/net-core-prereqs-vs2017-2.2.md)] 

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="2bb3e-218">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="2bb3e-218">Visual Studio Code</span></span>](#tab/visual-studio-code) 

[!INCLUDE[](~/includes/net-core-prereqs-vsc-2.2.md)]    

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="2bb3e-219">Visual Studio dla komputerów Mac</span><span class="sxs-lookup"><span data-stu-id="2bb3e-219">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)   

[!INCLUDE[](~/includes/net-core-prereqs-mac-2.2.md)]    

--- 

## <a name="create-a-web-project"></a><span data-ttu-id="2bb3e-220">Tworzenie projektu sieci Web</span><span class="sxs-lookup"><span data-stu-id="2bb3e-220">Create a web project</span></span> 

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="2bb3e-221">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="2bb3e-221">Visual Studio</span></span>](#tab/visual-studio/)  

* <span data-ttu-id="2bb3e-222">Z menu wybierz pozycję **plik > nowy projekt**.</span><span class="sxs-lookup"><span data-stu-id="2bb3e-222">From the menu, select **File > New Project**.</span></span> 

* <span data-ttu-id="2bb3e-223">W oknie dialogowym **Nowy projekt** wybierz pozycję **zainstalowane > Visual C# > Web > ASP.NET Core aplikacji sieci Web**.</span><span class="sxs-lookup"><span data-stu-id="2bb3e-223">In the **New Project** dialog, select **Installed > Visual C# > Web > ASP.NET Core Web Application**.</span></span> <span data-ttu-id="2bb3e-224">Nazwij projekt *SignalRChat*.</span><span class="sxs-lookup"><span data-stu-id="2bb3e-224">Name the project *SignalRChat*.</span></span> 

  ![Okno dialogowe nowego projektu w programie Visual Studio](signalr/_static/2.x/signalr-new-project-dialog.png)    

* <span data-ttu-id="2bb3e-226">Wybierz pozycję **aplikacja sieci Web** , aby utworzyć projekt, który używa Razor Pages.</span><span class="sxs-lookup"><span data-stu-id="2bb3e-226">Select **Web Application** to create a project that uses Razor Pages.</span></span> 

* <span data-ttu-id="2bb3e-227">Wybierz platformę docelową programu **.NET Core**, wybierz pozycję **ASP.NET Core 2,2**, a następnie kliknij przycisk **OK**.</span><span class="sxs-lookup"><span data-stu-id="2bb3e-227">Select a target framework of **.NET Core**, select **ASP.NET Core 2.2**, and click **OK**.</span></span>    

  ![Okno dialogowe nowego projektu w programie Visual Studio](signalr/_static/2.x/signalr-new-project-choose-type.png)   

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="2bb3e-229">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="2bb3e-229">Visual Studio Code</span></span>](#tab/visual-studio-code/)    

* <span data-ttu-id="2bb3e-230">Otwórz [zintegrowany terminal](https://code.visualstudio.com/docs/editor/integrated-terminal) do folderu, w którym zostanie utworzony nowy folder projektu.</span><span class="sxs-lookup"><span data-stu-id="2bb3e-230">Open the [integrated terminal](https://code.visualstudio.com/docs/editor/integrated-terminal) to the folder in which the new project folder will be created.</span></span>  

* <span data-ttu-id="2bb3e-231">Uruchom następujące polecenia:</span><span class="sxs-lookup"><span data-stu-id="2bb3e-231">Run the following commands:</span></span>   

   ```dotnetcli 
   dotnet new webapp -o SignalRChat 
   code -r SignalRChat  
   ```  

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="2bb3e-232">Visual Studio dla komputerów Mac</span><span class="sxs-lookup"><span data-stu-id="2bb3e-232">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)   

* <span data-ttu-id="2bb3e-233">Z menu wybierz pozycję **plik > nowe rozwiązanie**.</span><span class="sxs-lookup"><span data-stu-id="2bb3e-233">From the menu, select **File > New Solution**.</span></span>    

* <span data-ttu-id="2bb3e-234">Wybierz pozycję **.NET Core > app > ASP.NET Core Web App** (nie wybieraj **ASP.NET Core Web App (MVC)** ).</span><span class="sxs-lookup"><span data-stu-id="2bb3e-234">Select **.NET Core > App > ASP.NET Core Web App** (Don't select **ASP.NET Core Web App (MVC)**).</span></span>  

* <span data-ttu-id="2bb3e-235">Wybierz pozycję **dalej**.</span><span class="sxs-lookup"><span data-stu-id="2bb3e-235">Select **Next**.</span></span>  

* <span data-ttu-id="2bb3e-236">Nazwij projekt *SignalRChat*, a następnie wybierz pozycję **Utwórz**.</span><span class="sxs-lookup"><span data-stu-id="2bb3e-236">Name the project *SignalRChat*, and then select **Create**.</span></span>   

--- 

## <a name="add-the-opno-locsignalr-client-library"></a><span data-ttu-id="2bb3e-237">Dodawanie SignalRej biblioteki klienta</span><span class="sxs-lookup"><span data-stu-id="2bb3e-237">Add the SignalR client library</span></span> 

<span data-ttu-id="2bb3e-238">Biblioteka SignalR Server jest dołączona do `Microsoft.AspNetCore.App` pakietu.</span><span class="sxs-lookup"><span data-stu-id="2bb3e-238">The SignalR server library is included in the `Microsoft.AspNetCore.App` metapackage.</span></span> <span data-ttu-id="2bb3e-239">Biblioteka klienta JavaScript nie jest automatycznie dołączana do projektu.</span><span class="sxs-lookup"><span data-stu-id="2bb3e-239">The JavaScript client library isn't automatically included in the project.</span></span> <span data-ttu-id="2bb3e-240">W tym samouczku użyjesz programu Library Manager (LibMan), aby uzyskać bibliotekę kliencką z *unpkg*.</span><span class="sxs-lookup"><span data-stu-id="2bb3e-240">For this tutorial, you use Library Manager (LibMan) to get the client library from *unpkg*.</span></span> <span data-ttu-id="2bb3e-241">unpkg to usługa Content Delivery Network (CDN), która umożliwia dostarczanie elementów znalezionych w programie npm, w Menedżerze pakietów środowiska Node. js.</span><span class="sxs-lookup"><span data-stu-id="2bb3e-241">unpkg is a content delivery network (CDN)) that can deliver anything found in npm, the Node.js package manager.</span></span>  

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="2bb3e-242">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="2bb3e-242">Visual Studio</span></span>](#tab/visual-studio/)  

* <span data-ttu-id="2bb3e-243">W **Eksplorator rozwiązań**kliknij prawym przyciskiem myszy projekt, a następnie wybierz polecenie **Dodaj** > **biblioteki po stronie klienta**.</span><span class="sxs-lookup"><span data-stu-id="2bb3e-243">In **Solution Explorer**, right-click the project, and select **Add** > **Client-Side Library**.</span></span>  

* <span data-ttu-id="2bb3e-244">W oknie dialogowym **Dodawanie biblioteki po stronie klienta** dla **dostawcy** wybierz pozycję **unpkg**.</span><span class="sxs-lookup"><span data-stu-id="2bb3e-244">In the **Add Client-Side Library** dialog, for **Provider** select **unpkg**.</span></span> 

* <span data-ttu-id="2bb3e-245">W obszarze **Biblioteka**wprowadź wartość `@aspnet/signalr@1` i wybierz najnowszą wersję, która nie jest zapoznawcza.</span><span class="sxs-lookup"><span data-stu-id="2bb3e-245">For **Library**, enter `@aspnet/signalr@1`, and select the latest version that isn't preview.</span></span> 

  ![Okno dialogowe Dodawanie biblioteki po stronie klienta — wybór biblioteki](signalr/_static/2.x/libman1.png)   

* <span data-ttu-id="2bb3e-247">Wybierz pozycję **Wybierz określone pliki**, rozwiń folder *dist/przeglądarka* , a następnie wybierz pozycję *sygnalizujące. js* i *sygnalizujący. min. js*.</span><span class="sxs-lookup"><span data-stu-id="2bb3e-247">Select **Choose specific files**, expand the *dist/browser* folder, and select *signalr.js* and *signalr.min.js*.</span></span> 

* <span data-ttu-id="2bb3e-248">Ustaw **lokalizację docelową** na plik *wwwroot/lib/sygnalizujący/* , a następnie wybierz pozycję **Zainstaluj**.</span><span class="sxs-lookup"><span data-stu-id="2bb3e-248">Set **Target Location** to *wwwroot/lib/signalr/*, and select **Install**.</span></span>    

  ![Okno dialogowe Dodawanie biblioteki po stronie klienta — Wybieranie plików i lokalizacji docelowej](signalr/_static/2.x/libman2.png) 

  <span data-ttu-id="2bb3e-250">LibMan tworzy folder *wwwroot/lib/sygnalizujący* i kopiuje do niego wybrane pliki.</span><span class="sxs-lookup"><span data-stu-id="2bb3e-250">LibMan creates a *wwwroot/lib/signalr* folder and copies the selected files to it.</span></span>    

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="2bb3e-251">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="2bb3e-251">Visual Studio Code</span></span>](#tab/visual-studio-code/)    

* <span data-ttu-id="2bb3e-252">W zintegrowanym terminalu uruchom następujące polecenie, aby zainstalować LibMan.</span><span class="sxs-lookup"><span data-stu-id="2bb3e-252">In the integrated terminal, run the following command to install LibMan.</span></span>  

  ```dotnetcli  
  dotnet tool install -g Microsoft.Web.LibraryManager.Cli   
  ```   

* <span data-ttu-id="2bb3e-253">Uruchom następujące polecenie, aby uzyskać SignalR bibliotekę kliencką przy użyciu LibMan.</span><span class="sxs-lookup"><span data-stu-id="2bb3e-253">Run the following command to get the SignalR client library by using LibMan.</span></span> <span data-ttu-id="2bb3e-254">Może być konieczne odczekanie kilku sekund przed wyświetleniem danych wyjściowych.</span><span class="sxs-lookup"><span data-stu-id="2bb3e-254">You might have to wait a few seconds before seeing output.</span></span> 

  ```console    
  libman install @aspnet/signalr -p unpkg -d wwwroot/lib/signalr --files dist/browser/signalr.js --files dist/browser/signalr.min.js    
  ```   

  <span data-ttu-id="2bb3e-255">Parametry określają następujące opcje:</span><span class="sxs-lookup"><span data-stu-id="2bb3e-255">The parameters specify the following options:</span></span> 
  * <span data-ttu-id="2bb3e-256">Użyj dostawcy unpkg.</span><span class="sxs-lookup"><span data-stu-id="2bb3e-256">Use the unpkg provider.</span></span> 
  * <span data-ttu-id="2bb3e-257">Kopiuj pliki do lokalizacji *wwwroot/lib/sygnalizującej* .</span><span class="sxs-lookup"><span data-stu-id="2bb3e-257">Copy files to the *wwwroot/lib/signalr* destination.</span></span>    
  * <span data-ttu-id="2bb3e-258">Skopiuj tylko określone pliki.</span><span class="sxs-lookup"><span data-stu-id="2bb3e-258">Copy only the specified files.</span></span>  

  <span data-ttu-id="2bb3e-259">Dane wyjściowe wyglądają podobnie do poniższego przykładu:</span><span class="sxs-lookup"><span data-stu-id="2bb3e-259">The output looks like the following example:</span></span>  

  ```console    
  wwwroot/lib/signalr/dist/browser/signalr.js written to disk   
  wwwroot/lib/signalr/dist/browser/signalr.min.js written to disk   
  Installed library "@aspnet/signalr@1.0.3" to "wwwroot/lib/signalr"    
  ```   

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="2bb3e-260">Visual Studio dla komputerów Mac</span><span class="sxs-lookup"><span data-stu-id="2bb3e-260">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)   

* <span data-ttu-id="2bb3e-261">W **terminalu**Uruchom następujące polecenie, aby zainstalować LibMan.</span><span class="sxs-lookup"><span data-stu-id="2bb3e-261">In the **Terminal**, run the following command to install LibMan.</span></span> 

  ```dotnetcli  
  dotnet tool install -g Microsoft.Web.LibraryManager.Cli   
  ```   

* <span data-ttu-id="2bb3e-262">Przejdź do folderu projektu (taki, który zawiera plik *SignalRChat. csproj* ).</span><span class="sxs-lookup"><span data-stu-id="2bb3e-262">Navigate to the project folder (the one that contains the *SignalRChat.csproj* file).</span></span> 

* <span data-ttu-id="2bb3e-263">Uruchom następujące polecenie, aby uzyskać SignalR bibliotekę kliencką przy użyciu LibMan.</span><span class="sxs-lookup"><span data-stu-id="2bb3e-263">Run the following command to get the SignalR client library by using LibMan.</span></span>    

  ```console    
  libman install @aspnet/signalr -p unpkg -d wwwroot/lib/signalr --files dist/browser/signalr.js --files dist/browser/signalr.min.js    
  ```   

  <span data-ttu-id="2bb3e-264">Parametry określają następujące opcje:</span><span class="sxs-lookup"><span data-stu-id="2bb3e-264">The parameters specify the following options:</span></span> 
  * <span data-ttu-id="2bb3e-265">Użyj dostawcy unpkg.</span><span class="sxs-lookup"><span data-stu-id="2bb3e-265">Use the unpkg provider.</span></span> 
  * <span data-ttu-id="2bb3e-266">Kopiuj pliki do lokalizacji *wwwroot/lib/sygnalizującej* .</span><span class="sxs-lookup"><span data-stu-id="2bb3e-266">Copy files to the *wwwroot/lib/signalr* destination.</span></span>    
  * <span data-ttu-id="2bb3e-267">Skopiuj tylko określone pliki.</span><span class="sxs-lookup"><span data-stu-id="2bb3e-267">Copy only the specified files.</span></span>  

  <span data-ttu-id="2bb3e-268">Dane wyjściowe wyglądają podobnie do poniższego przykładu:</span><span class="sxs-lookup"><span data-stu-id="2bb3e-268">The output looks like the following example:</span></span>  

  ```console    
  wwwroot/lib/signalr/dist/browser/signalr.js written to disk   
  wwwroot/lib/signalr/dist/browser/signalr.min.js written to disk   
  Installed library "@aspnet/signalr@1.0.3" to "wwwroot/lib/signalr"    
  ```   

--- 

## <a name="create-a-opno-locsignalr-hub"></a><span data-ttu-id="2bb3e-269">Tworzenie centrum SignalR</span><span class="sxs-lookup"><span data-stu-id="2bb3e-269">Create a SignalR hub</span></span>   

<span data-ttu-id="2bb3e-270">*Koncentrator* jest klasą, która służy jako potok wysokiego poziomu, który obsługuje komunikację klient-serwer.</span><span class="sxs-lookup"><span data-stu-id="2bb3e-270">A *hub* is a class that serves as a high-level pipeline that handles client-server communication.</span></span>   

* <span data-ttu-id="2bb3e-271">W folderze projektu SignalRChat Utwórz folder *Hubs* .</span><span class="sxs-lookup"><span data-stu-id="2bb3e-271">In the SignalRChat project folder, create a *Hubs* folder.</span></span>    

* <span data-ttu-id="2bb3e-272">W folderze *Hubs* utwórz plik *ChatHub.cs* o następującym kodzie:</span><span class="sxs-lookup"><span data-stu-id="2bb3e-272">In the *Hubs* folder, create a *ChatHub.cs* file with the following code:</span></span> 

  [!code-csharp[Startup](signalr/sample-snapshot/2.x/ChatHub.cs)]   

  <span data-ttu-id="2bb3e-273">Klasa `ChatHub` dziedziczy z klasy SignalR `Hub`.</span><span class="sxs-lookup"><span data-stu-id="2bb3e-273">The `ChatHub` class inherits from the SignalR `Hub` class.</span></span> <span data-ttu-id="2bb3e-274">Klasa `Hub` zarządza połączeniami, grupami i obsługą komunikatów.</span><span class="sxs-lookup"><span data-stu-id="2bb3e-274">The `Hub` class manages connections, groups, and messaging.</span></span>  

  <span data-ttu-id="2bb3e-275">Metoda `SendMessage` może być wywoływana przez połączonego klienta w celu wysłania komunikatu do wszystkich klientów.</span><span class="sxs-lookup"><span data-stu-id="2bb3e-275">The `SendMessage` method can be called by a connected client to send a message to all clients.</span></span> <span data-ttu-id="2bb3e-276">Kod klienta JavaScript, który wywołuje metodę, jest wyświetlany w dalszej części samouczka.</span><span class="sxs-lookup"><span data-stu-id="2bb3e-276">JavaScript client code that calls the method is shown later in the tutorial.</span></span> <span data-ttu-id="2bb3e-277">kod SignalR jest asynchroniczny w celu zapewnienia maksymalnej skalowalności.</span><span class="sxs-lookup"><span data-stu-id="2bb3e-277">SignalR code is asynchronous to provide maximum scalability.</span></span>    

## <a name="configure-opno-locsignalr"></a><span data-ttu-id="2bb3e-278">Konfigurowanie SignalR</span><span class="sxs-lookup"><span data-stu-id="2bb3e-278">Configure SignalR</span></span>  

<span data-ttu-id="2bb3e-279">Serwer SignalR musi być skonfigurowany do przekazywania żądań SignalR do SignalR.</span><span class="sxs-lookup"><span data-stu-id="2bb3e-279">The SignalR server must be configured to pass SignalR requests to SignalR.</span></span>    

* <span data-ttu-id="2bb3e-280">Dodaj następujący wyróżniony kod do pliku *Startup.cs* .</span><span class="sxs-lookup"><span data-stu-id="2bb3e-280">Add the following highlighted code to the *Startup.cs* file.</span></span>  

  [!code-csharp[Startup](signalr/sample-snapshot/2.x/Startup.cs?highlight=7,33,52-55)]  

  <span data-ttu-id="2bb3e-281">Te zmiany umożliwiają dodanie SignalR do ASP.NET Core systemu iniekcji zależności oraz potoku oprogramowania pośredniczącego.</span><span class="sxs-lookup"><span data-stu-id="2bb3e-281">These changes add SignalR to the ASP.NET Core dependency injection system and the middleware pipeline.</span></span>  

## <a name="add-opno-locsignalr-client-code"></a><span data-ttu-id="2bb3e-282">Dodawanie kodu klienta SignalR</span><span class="sxs-lookup"><span data-stu-id="2bb3e-282">Add SignalR client code</span></span>    

* <span data-ttu-id="2bb3e-283">Zastąp zawartość w *Pages\Index.cshtml* następującym kodem:</span><span class="sxs-lookup"><span data-stu-id="2bb3e-283">Replace the content in *Pages\Index.cshtml* with the following code:</span></span>  

  [!code-cshtml[Index](signalr/sample-snapshot/2.x/Index.cshtml)]   

  <span data-ttu-id="2bb3e-284">Poprzedni kod:</span><span class="sxs-lookup"><span data-stu-id="2bb3e-284">The preceding code:</span></span>   

  * <span data-ttu-id="2bb3e-285">Tworzy pola tekstowe dla nazwy i tekstu komunikatu oraz przycisk Prześlij.</span><span class="sxs-lookup"><span data-stu-id="2bb3e-285">Creates text boxes for name and message text, and a submit button.</span></span>  
  * <span data-ttu-id="2bb3e-286">Tworzy listę z `id="messagesList"` do wyświetlania komunikatów odebranych z centrum SignalR.</span><span class="sxs-lookup"><span data-stu-id="2bb3e-286">Creates a list with `id="messagesList"` for displaying messages that are received from the SignalR hub.</span></span>   
  * <span data-ttu-id="2bb3e-287">Zawiera odwołania do skryptów do SignalR i kod aplikacji *czatu. js* , który tworzysz w następnym kroku.</span><span class="sxs-lookup"><span data-stu-id="2bb3e-287">Includes script references to SignalR and the *chat.js* application code that you create in the next step.</span></span>    

* <span data-ttu-id="2bb3e-288">W folderze *wwwroot/js* Utwórz plik *czatu. js* o następującym kodzie:</span><span class="sxs-lookup"><span data-stu-id="2bb3e-288">In the *wwwroot/js* folder, create a *chat.js* file with the following code:</span></span>  

  [!code-javascript[Index](signalr/sample-snapshot/2.x/chat.js)]    

  <span data-ttu-id="2bb3e-289">Poprzedni kod:</span><span class="sxs-lookup"><span data-stu-id="2bb3e-289">The preceding code:</span></span>   

  * <span data-ttu-id="2bb3e-290">Tworzy i uruchamia połączenie.</span><span class="sxs-lookup"><span data-stu-id="2bb3e-290">Creates and starts a connection.</span></span>    
  * <span data-ttu-id="2bb3e-291">Dodaje do przycisku Prześlij procedurę obsługi, która wysyła komunikaty do centrum.</span><span class="sxs-lookup"><span data-stu-id="2bb3e-291">Adds to the submit button a handler that sends messages to the hub.</span></span> 
  * <span data-ttu-id="2bb3e-292">Dodaje do obiektu Connection program obsługi, który odbiera komunikaty z centrum i dodaje je do listy.</span><span class="sxs-lookup"><span data-stu-id="2bb3e-292">Adds to the connection object a handler that receives messages from the hub and adds them to the list.</span></span>  

## <a name="run-the-app"></a><span data-ttu-id="2bb3e-293">Uruchamianie aplikacji</span><span class="sxs-lookup"><span data-stu-id="2bb3e-293">Run the app</span></span>  

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="2bb3e-294">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="2bb3e-294">Visual Studio</span></span>](#tab/visual-studio)   

* <span data-ttu-id="2bb3e-295">Naciśnij **klawisze CTRL + F5** , aby uruchomić aplikację bez debugowania.</span><span class="sxs-lookup"><span data-stu-id="2bb3e-295">Press **CTRL+F5** to run the app without debugging.</span></span>   

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="2bb3e-296">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="2bb3e-296">Visual Studio Code</span></span>](#tab/visual-studio-code) 

* <span data-ttu-id="2bb3e-297">W zintegrowanym terminalu uruchom następujące polecenie:</span><span class="sxs-lookup"><span data-stu-id="2bb3e-297">In the integrated terminal, run the following command:</span></span>    

  ```dotnetcli  
  dotnet run -p SignalRChat.csproj  
  ```   

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="2bb3e-298">Visual Studio dla komputerów Mac</span><span class="sxs-lookup"><span data-stu-id="2bb3e-298">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)   

* <span data-ttu-id="2bb3e-299">Z menu wybierz polecenie **uruchom > Uruchom bez debugowania**.</span><span class="sxs-lookup"><span data-stu-id="2bb3e-299">From the menu, select **Run > Start Without Debugging**.</span></span>  

--- 

* <span data-ttu-id="2bb3e-300">Skopiuj adres URL z paska adresu, Otwórz inne wystąpienie przeglądarki lub kartę, a następnie wklej adres URL na pasku adresu.</span><span class="sxs-lookup"><span data-stu-id="2bb3e-300">Copy the URL from the address bar, open another browser instance or tab, and paste the URL in the address bar.</span></span>    

* <span data-ttu-id="2bb3e-301">Wybierz opcję przeglądarka, wprowadź nazwę i komunikat, a następnie wybierz przycisk **Wyślij wiadomość** .</span><span class="sxs-lookup"><span data-stu-id="2bb3e-301">Choose either browser, enter a name and message, and select the **Send Message** button.</span></span>  

  <span data-ttu-id="2bb3e-302">Nazwa i komunikat są natychmiast wyświetlane na obu stronach.</span><span class="sxs-lookup"><span data-stu-id="2bb3e-302">The name and message are displayed on both pages instantly.</span></span>   

  ![[! OP. Aplikacja Przykładowa NO-LOC (sygnalizująca)]](signalr/_static/2.x/signalr-get-started-finished.png) 

> [!TIP]    
> <span data-ttu-id="2bb3e-304">Jeśli aplikacja nie działa, Otwórz narzędzia deweloperskie przeglądarki (F12) i przejdź do konsoli programu.</span><span class="sxs-lookup"><span data-stu-id="2bb3e-304">If the app doesn't work, open your browser developer tools (F12) and go to the console.</span></span> <span data-ttu-id="2bb3e-305">Mogą pojawić się błędy związane z kodem HTML i JavaScript.</span><span class="sxs-lookup"><span data-stu-id="2bb3e-305">You might see errors related to your HTML and JavaScript code.</span></span> <span data-ttu-id="2bb3e-306">Załóżmy na przykład, że umieścisz polecenie *signaler. js* w innym folderze niż skierowany.</span><span class="sxs-lookup"><span data-stu-id="2bb3e-306">For example, suppose you put *signalr.js* in a different folder than directed.</span></span> <span data-ttu-id="2bb3e-307">W takim przypadku odwołanie do tego pliku nie będzie działało i zobaczysz błąd 404 w konsoli.</span><span class="sxs-lookup"><span data-stu-id="2bb3e-307">In that case the reference to that file won't work and you'll see a 404 error in the console.</span></span>   
> <span data-ttu-id="2bb3e-308">Wystąpił błąd ![sygnalizującer. nie znaleziono błędu](signalr/_static/2.x/f12-console.png)</span><span class="sxs-lookup"><span data-stu-id="2bb3e-308">![signalr.js not found error](signalr/_static/2.x/f12-console.png)</span></span>    
## <a name="additional-resources"></a><span data-ttu-id="2bb3e-309">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="2bb3e-309">Additional resources</span></span> 
* [<span data-ttu-id="2bb3e-310">Wersja tego samouczka usługi YouTube</span><span class="sxs-lookup"><span data-stu-id="2bb3e-310">Youtube version of this tutorial</span></span>](https://www.youtube.com/watch?v=iKlVmu-r0JQ)   

## <a name="next-steps"></a><span data-ttu-id="2bb3e-311">Następne kroki</span><span class="sxs-lookup"><span data-stu-id="2bb3e-311">Next steps</span></span>   

<span data-ttu-id="2bb3e-312">W tym samouczku przedstawiono sposób wykonywania tych instrukcji:</span><span class="sxs-lookup"><span data-stu-id="2bb3e-312">In this tutorial, you learned how to:</span></span>   

> [!div class="checklist"]  
> * <span data-ttu-id="2bb3e-313">Utwórz projekt aplikacji sieci Web.</span><span class="sxs-lookup"><span data-stu-id="2bb3e-313">Create a web app project.</span></span>   
> * <span data-ttu-id="2bb3e-314">Dodaj SignalRą bibliotekę kliencką.</span><span class="sxs-lookup"><span data-stu-id="2bb3e-314">Add the SignalR client library.</span></span>   
> * <span data-ttu-id="2bb3e-315">Utwórz centrum SignalR.</span><span class="sxs-lookup"><span data-stu-id="2bb3e-315">Create a SignalR hub.</span></span> 
> * <span data-ttu-id="2bb3e-316">Skonfiguruj projekt do użycia SignalR.</span><span class="sxs-lookup"><span data-stu-id="2bb3e-316">Configure the project to use SignalR.</span></span> 
> * <span data-ttu-id="2bb3e-317">Dodaj kod używający centrum do wysyłania komunikatów z dowolnego klienta do wszystkich podłączonych klientów.</span><span class="sxs-lookup"><span data-stu-id="2bb3e-317">Add code that uses the hub to send messages from any client to all connected clients.</span></span>   
<span data-ttu-id="2bb3e-318">Aby dowiedzieć się więcej na temat SignalR, zobacz Wprowadzenie:</span><span class="sxs-lookup"><span data-stu-id="2bb3e-318">To learn more about SignalR, see the introduction:</span></span>    
> [!div class="nextstepaction"] 
> <span data-ttu-id="2bb3e-319">[Wprowadzenie do ASP.NET Core SignalR](xref:signalr/introduction)</span><span class="sxs-lookup"><span data-stu-id="2bb3e-319">[Introduction to ASP.NET Core SignalR](xref:signalr/introduction)</span></span>   
::: moniker-end

