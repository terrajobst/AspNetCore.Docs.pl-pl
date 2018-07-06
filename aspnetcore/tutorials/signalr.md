---
title: Wprowadzenie do SignalR platformy ASP.NET Core
author: rachelappel
description: W tym samouczku utworzysz aplikację dla platformy ASP.NET Core przy użyciu SignalR.
monikerRange: '>= aspnetcore-2.1'
ms.author: rachelap
ms.custom: mvc
ms.date: 05/22/2018
uid: tutorials/signalr
ms.openlocfilehash: 6b8222ee04573ca7157b4e1125ed5a4453b2b9a9
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/05/2018
ms.locfileid: "37830558"
---
# <a name="get-started-with-signalr-on-aspnet-core"></a><span data-ttu-id="a9233-103">Wprowadzenie do SignalR platformy ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="a9233-103">Get started with SignalR on ASP.NET Core</span></span>

<span data-ttu-id="a9233-104">Przez [Rachel Appel](https://twitter.com/rachelappel)</span><span class="sxs-lookup"><span data-stu-id="a9233-104">By [Rachel Appel](https://twitter.com/rachelappel)</span></span>

<span data-ttu-id="a9233-105">W tym samouczku pokazano podstawowe informacje dotyczące tworzenia aplikacji w czasie rzeczywistym przy użyciu biblioteki SignalR dla platformy ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="a9233-105">This tutorial teaches the basics of building a real-time app using SignalR for ASP.NET Core.</span></span>

   ![Rozwiązanie](signalr/_static/signalr-get-started-finished.png)

<span data-ttu-id="a9233-107">W tym samouczku przedstawiono następujące zadania deweloperskie SignalR:</span><span class="sxs-lookup"><span data-stu-id="a9233-107">This tutorial demonstrates the following SignalR development tasks:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="a9233-108">Utwórz SignalR w aplikacji internetowej ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="a9233-108">Create a SignalR on ASP.NET Core web app.</span></span>
> * <span data-ttu-id="a9233-109">Utwórz Centrum SignalR, aby wypchnąć zawartości do klientów.</span><span class="sxs-lookup"><span data-stu-id="a9233-109">Create a SignalR hub to push content to clients.</span></span>
> * <span data-ttu-id="a9233-110">Modyfikowanie `Startup` klasy i skonfigurowania aplikacji.</span><span class="sxs-lookup"><span data-stu-id="a9233-110">Modify the `Startup` class and configure the app.</span></span>

<span data-ttu-id="a9233-111">[Wyświetlanie lub pobieranie przykładowego kodu](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/signalr/sample) ([sposobu pobierania](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="a9233-111">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/signalr/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="prerequisites"></a><span data-ttu-id="a9233-112">Wymagania wstępne</span><span class="sxs-lookup"><span data-stu-id="a9233-112">Prerequisites</span></span>

<span data-ttu-id="a9233-113">Zainstaluj następujące oprogramowanie:</span><span class="sxs-lookup"><span data-stu-id="a9233-113">Install the following software:</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="a9233-114">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="a9233-114">Visual Studio</span></span>](#tab/visual-studio)

* [<span data-ttu-id="a9233-115">.NET core SDK 2.1 lub nowszej</span><span class="sxs-lookup"><span data-stu-id="a9233-115">.NET Core SDK 2.1 or later</span></span>](https://www.microsoft.com/net/download/all)
* <span data-ttu-id="a9233-116">[Program Visual Studio 2017](https://www.visualstudio.com/downloads/) wersji 15.7.3 lub nowszy z **ASP.NET i tworzenie aplikacji internetowych** obciążenia</span><span class="sxs-lookup"><span data-stu-id="a9233-116">[Visual Studio 2017](https://www.visualstudio.com/downloads/) version 15.7.3 or later with the **ASP.NET and web development** workload</span></span>
* [<span data-ttu-id="a9233-117">npm</span><span class="sxs-lookup"><span data-stu-id="a9233-117">npm</span></span>](https://www.npmjs.com/get-npm)

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="a9233-118">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="a9233-118">Visual Studio Code</span></span>](#tab/visual-studio-code)

* [<span data-ttu-id="a9233-119">.NET core SDK 2.1 lub nowszej</span><span class="sxs-lookup"><span data-stu-id="a9233-119">.NET Core SDK 2.1 or later</span></span>](https://www.microsoft.com/net/download/all)
* [<span data-ttu-id="a9233-120">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="a9233-120">Visual Studio Code</span></span>](https://code.visualstudio.com/download)
* [<span data-ttu-id="a9233-121">Środowisko C# dla programu Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="a9233-121">C# for Visual Studio Code</span></span>](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp)
* [<span data-ttu-id="a9233-122">npm</span><span class="sxs-lookup"><span data-stu-id="a9233-122">npm</span></span>](https://www.npmjs.com/get-npm)

-----

## <a name="create-an-aspnet-core-project-that-hosts-signalr-client-and-server"></a><span data-ttu-id="a9233-123">Tworzenie projektu platformy ASP.NET Core, który jest hostem SignalR klienta i serwera</span><span class="sxs-lookup"><span data-stu-id="a9233-123">Create an ASP.NET Core project that hosts SignalR client and server</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="a9233-124">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="a9233-124">Visual Studio</span></span>](#tab/visual-studio/)

1. <span data-ttu-id="a9233-125">Użyj **pliku** > **nowy projekt** menu opcji, a następnie wybierz **aplikacji sieci Web programu ASP.NET Core**.</span><span class="sxs-lookup"><span data-stu-id="a9233-125">Use the **File** > **New Project** menu option and choose **ASP.NET Core Web Application**.</span></span> <span data-ttu-id="a9233-126">Nadaj projektowi nazwę *SignalRChat*.</span><span class="sxs-lookup"><span data-stu-id="a9233-126">Name the project *SignalRChat*.</span></span>

   ![Okno dialogowe nowego projektu w programie Visual Studio](signalr/_static/signalr-new-project-dialog.png)

2. <span data-ttu-id="a9233-128">Wybierz **aplikacji sieci Web** do utworzenia projektu przy użyciu stron Razor.</span><span class="sxs-lookup"><span data-stu-id="a9233-128">Select **Web Application** to create a project using Razor Pages.</span></span> <span data-ttu-id="a9233-129">Następnie wybierz pozycję **OK**.</span><span class="sxs-lookup"><span data-stu-id="a9233-129">Then select **OK**.</span></span> <span data-ttu-id="a9233-130">Upewnij się, że **platformy ASP.NET Core 2.1** wybrane z selektora framework, chociaż SignalR działa w starszej wersji platformy .NET.</span><span class="sxs-lookup"><span data-stu-id="a9233-130">Be sure that **ASP.NET Core 2.1** is selected from the framework selector, though SignalR runs on older versions of .NET.</span></span>

   ![Okno dialogowe nowego projektu w programie Visual Studio](signalr/_static/signalr-new-project-choose-type.png)

<span data-ttu-id="a9233-132">Program Visual Studio obejmuje `Microsoft.AspNetCore.SignalR` pakietu zawierającego jego serwera biblioteki jako część jej **aplikacji sieci Web programu ASP.NET Core** szablonu.</span><span class="sxs-lookup"><span data-stu-id="a9233-132">Visual Studio includes the `Microsoft.AspNetCore.SignalR` package containing its server libraries as part of its **ASP.NET Core Web Application** template.</span></span> <span data-ttu-id="a9233-133">Jednak z biblioteki klienta JavaScript dla elementu SignalR musi być zainstalowany przy użyciu *npm*.</span><span class="sxs-lookup"><span data-stu-id="a9233-133">However, the JavaScript client library for SignalR must be installed using *npm*.</span></span>

3. <span data-ttu-id="a9233-134">Uruchom następujące polecenia **Konsola Menedżera pakietów** okna z katalogu głównego projektu:</span><span class="sxs-lookup"><span data-stu-id="a9233-134">Run the following commands in the **Package Manager Console** window, from the project root:</span></span>

    ```console
    npm init -y
    npm install @aspnet/signalr
    ```

4. <span data-ttu-id="a9233-135">Utwórz nowy folder o nazwie "signalr" wewnątrz *lib* folder w projekcie.</span><span class="sxs-lookup"><span data-stu-id="a9233-135">Create a new folder named "signalr" inside the  *lib* folder in your project.</span></span> <span data-ttu-id="a9233-136">Kopiuj *signalr.js* plik wchodzącej w skład *node_modules\\ @aspnet\signalr\dist\browser*  do tego folderu.</span><span class="sxs-lookup"><span data-stu-id="a9233-136">Copy the *signalr.js* file from *node_modules\\@aspnet\signalr\dist\browser* to this folder.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="a9233-137">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="a9233-137">Visual Studio Code</span></span>](#tab/visual-studio-code/)

1. <span data-ttu-id="a9233-138">Z **zintegrowany Terminal**, uruchom następujące polecenie:</span><span class="sxs-lookup"><span data-stu-id="a9233-138">From the **Integrated Terminal**, run the following command:</span></span>

    ```console
    dotnet new webapp -o SignalRChat
    ```

    [!INCLUDE[](~/includes/webapp-alias-notice.md)]

2. <span data-ttu-id="a9233-139">Zainstaluj język JavaScript klienta biblioteki przy użyciu *npm*.</span><span class="sxs-lookup"><span data-stu-id="a9233-139">Install the JavaScript client library using *npm*.</span></span>

    ```console
    npm init -y
    npm install @aspnet/signalr
    ```

3. <span data-ttu-id="a9233-140">Utwórz nowy folder o nazwie "signalr" wewnątrz *lib* folder w projekcie.</span><span class="sxs-lookup"><span data-stu-id="a9233-140">Create a new folder named "signalr" inside the  *lib* folder in your project.</span></span> <span data-ttu-id="a9233-141">Kopiuj *signalr.js* plik wchodzącej w skład *node_modules\\ @aspnet\signalr\dist\browser*  do tego folderu.</span><span class="sxs-lookup"><span data-stu-id="a9233-141">Copy the *signalr.js* file from *node_modules\\@aspnet\signalr\dist\browser* to this folder.</span></span>

---

## <a name="create-the-signalr-hub"></a><span data-ttu-id="a9233-142">Tworzenie Centrum SignalR</span><span class="sxs-lookup"><span data-stu-id="a9233-142">Create the SignalR Hub</span></span>

<span data-ttu-id="a9233-143">Koncentrator, to klasa, która służy jako ogólny potok, który umożliwia klienta i serwera, wywoływanie metod na siebie nawzajem.</span><span class="sxs-lookup"><span data-stu-id="a9233-143">A hub is a class that serves as a high-level pipeline that allows the client and server to call methods on each other.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="a9233-144">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="a9233-144">Visual Studio</span></span>](#tab/visual-studio/)

1. <span data-ttu-id="a9233-145">Dodaj klasę do projektu, wybierając **pliku** > **nowy** > **pliku** i wybierając polecenie **klasy Visual C#**.</span><span class="sxs-lookup"><span data-stu-id="a9233-145">Add a class to the project by choosing **File** > **New** > **File** and selecting **Visual C# Class**.</span></span> <span data-ttu-id="a9233-146">Nazwa klasy `ChatHub` i plik *ChatHub.cs*.</span><span class="sxs-lookup"><span data-stu-id="a9233-146">Name the class `ChatHub` and the file *ChatHub.cs*.</span></span>

2. <span data-ttu-id="a9233-147">Dziedzicz `Microsoft.AspNetCore.SignalR.Hub`.</span><span class="sxs-lookup"><span data-stu-id="a9233-147">Inherit from `Microsoft.AspNetCore.SignalR.Hub`.</span></span> <span data-ttu-id="a9233-148">`Hub` Klasa zawiera właściwości i zdarzeń zarządzania połączeniami i grup, a także wysyłania i odbierania danych.</span><span class="sxs-lookup"><span data-stu-id="a9233-148">The `Hub` class contains properties and events for managing connections and groups, as well as sending and receiving data.</span></span>

3. <span data-ttu-id="a9233-149">Utwórz `SendMessage` metodę, która wysyła wiadomość do wszystkich klientów połączonych rozmowy.</span><span class="sxs-lookup"><span data-stu-id="a9233-149">Create the `SendMessage` method that sends a message to all connected chat clients.</span></span> <span data-ttu-id="a9233-150">Zwróć uwagę, funkcja zwraca [zadań](https://msdn.microsoft.com/library/system.threading.tasks.task(v=vs.110).aspx), ponieważ SignalR jest asynchroniczna.</span><span class="sxs-lookup"><span data-stu-id="a9233-150">Notice it returns a [Task](https://msdn.microsoft.com/library/system.threading.tasks.task(v=vs.110).aspx), because SignalR is asynchronous.</span></span> <span data-ttu-id="a9233-151">Kod asynchroniczny skaluje się lepiej.</span><span class="sxs-lookup"><span data-stu-id="a9233-151">Asynchronous code scales better.</span></span>

   [!code-csharp[Startup](signalr/sample/Hubs/ChatHub.cs)]

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="a9233-152">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="a9233-152">Visual Studio Code</span></span>](#tab/visual-studio-code/)

1. <span data-ttu-id="a9233-153">Otwórz *SignalRChat* folderu w programie Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="a9233-153">Open the *SignalRChat* folder in Visual Studio Code.</span></span>

2. <span data-ttu-id="a9233-154">Dodaj klasę do projektu, wybierając **pliku** > **nowy plik** z menu.</span><span class="sxs-lookup"><span data-stu-id="a9233-154">Add a class to the project by selecting **File** > **New File** from the menu.</span></span> <span data-ttu-id="a9233-155">Nazwa klasy `ChatHub` i plik *ChatHub.cs*.</span><span class="sxs-lookup"><span data-stu-id="a9233-155">Name the class `ChatHub` and the file *ChatHub.cs*.</span></span>

3. <span data-ttu-id="a9233-156">Dziedzicz `Microsoft.AspNetCore.SignalR.Hub`.</span><span class="sxs-lookup"><span data-stu-id="a9233-156">Inherit from `Microsoft.AspNetCore.SignalR.Hub`.</span></span> <span data-ttu-id="a9233-157">`Hub` Klasa zawiera właściwości i zdarzeń związanych z zarządzaniem połączeń i grupy, a także wysyłania i odbierania danych do klientów.</span><span class="sxs-lookup"><span data-stu-id="a9233-157">The `Hub` class contains properties and events for managing connections and groups, as well as sending and receiving data to clients.</span></span>

4. <span data-ttu-id="a9233-158">Dodaj `SendMessage` metodę do klasy.</span><span class="sxs-lookup"><span data-stu-id="a9233-158">Add a `SendMessage` method to the class.</span></span> <span data-ttu-id="a9233-159">`SendMessage` Metoda wysyła wiadomość do wszystkich klientów połączonych rozmowy.</span><span class="sxs-lookup"><span data-stu-id="a9233-159">The `SendMessage` method sends a message to all connected chat clients.</span></span> <span data-ttu-id="a9233-160">Zwróć uwagę, funkcja zwraca [zadań](/dotnet/api/system.threading.tasks.task), ponieważ SignalR jest asynchroniczna.</span><span class="sxs-lookup"><span data-stu-id="a9233-160">Notice it returns a [Task](/dotnet/api/system.threading.tasks.task), because SignalR is asynchronous.</span></span> <span data-ttu-id="a9233-161">Kod asynchroniczny skaluje się lepiej.</span><span class="sxs-lookup"><span data-stu-id="a9233-161">Asynchronous code scales better.</span></span>

   [!code-csharp[Startup](signalr/sample/Hubs/ChatHub.cs)]

-----

## <a name="configure-the-project-to-use-signalr"></a><span data-ttu-id="a9233-162">Konfigurowanie projektu do korzystania z biblioteki SignalR</span><span class="sxs-lookup"><span data-stu-id="a9233-162">Configure the project to use SignalR</span></span>

<span data-ttu-id="a9233-163">Serwer biblioteki SignalR musi być skonfigurowany, tak aby wie, aby przekazywać żądania do SignalR.</span><span class="sxs-lookup"><span data-stu-id="a9233-163">The SignalR server must be configured so that it knows to pass requests to SignalR.</span></span>

1. <span data-ttu-id="a9233-164">Aby skonfigurować projekt SignalR, zmodyfikuj projektu `Startup.ConfigureServices` metody.</span><span class="sxs-lookup"><span data-stu-id="a9233-164">To configure a SignalR project, modify the project's `Startup.ConfigureServices` method.</span></span>

   <span data-ttu-id="a9233-165">`services.AddSignalR` udostępnia usługi SignalR [wstrzykiwanie zależności](xref:fundamentals/dependency-injection) systemu.</span><span class="sxs-lookup"><span data-stu-id="a9233-165">`services.AddSignalR` makes the SignalR services available to the [dependency injection](xref:fundamentals/dependency-injection) system.</span></span>

1. <span data-ttu-id="a9233-166">Konfigurowanie tras do Twojej koncentratorów, za pomocą `UseSignalR` w `Configure` metody.</span><span class="sxs-lookup"><span data-stu-id="a9233-166">Configure routes to your hubs with `UseSignalR` in the `Configure` method.</span></span> <span data-ttu-id="a9233-167">`app.UseSignalR` dodaje SignalR do [oprogramowania pośredniczącego](xref:fundamentals/middleware/index) potoku.</span><span class="sxs-lookup"><span data-stu-id="a9233-167">`app.UseSignalR` adds SignalR to the [middleware](xref:fundamentals/middleware/index) pipeline.</span></span>

   [!code-csharp[Startup](signalr/sample/Startup.cs?highlight=37,57-60)]

## <a name="create-the-signalr-client-code"></a><span data-ttu-id="a9233-168">Tworzenie kodu klienta SignalR</span><span class="sxs-lookup"><span data-stu-id="a9233-168">Create the SignalR client code</span></span>

1. <span data-ttu-id="a9233-169">Dodaj plik języka JavaScript o nazwie *chat.js*, *wwwroot\js* folderu.</span><span class="sxs-lookup"><span data-stu-id="a9233-169">Add a JavaScript file, named *chat.js*, to the *wwwroot\js* folder.</span></span> <span data-ttu-id="a9233-170">Dodaj następujący kod do niego:</span><span class="sxs-lookup"><span data-stu-id="a9233-170">Add the following code to it:</span></span>

   [!code-javascript[Index](signalr/sample/wwwroot/js/chat.js)]

2. <span data-ttu-id="a9233-171">Zastąp zawartość *Pages\Index.cshtml* następującym kodem:</span><span class="sxs-lookup"><span data-stu-id="a9233-171">Replace the content in *Pages\Index.cshtml* with the following code:</span></span>

   [!code-cshtml[Index](signalr/sample/Pages/Index.cshtml)]

   <span data-ttu-id="a9233-172">Poprzedni kod HTML Wyświetla nazwę i pola wiadomości i przycisk Prześlij.</span><span class="sxs-lookup"><span data-stu-id="a9233-172">The preceding HTML displays name and message fields, and a submit button.</span></span> <span data-ttu-id="a9233-173">Zwróć uwagę, odwołania do skryptu, u dołu: odwołanie do biblioteki SignalR i *chat.js*.</span><span class="sxs-lookup"><span data-stu-id="a9233-173">Notice the script references at the bottom: a reference to SignalR and *chat.js*.</span></span>

## <a name="run-the-app"></a><span data-ttu-id="a9233-174">Uruchamianie aplikacji</span><span class="sxs-lookup"><span data-stu-id="a9233-174">Run the app</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="a9233-175">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="a9233-175">Visual Studio</span></span>](#tab/visual-studio)

1. <span data-ttu-id="a9233-176">Wybierz **debugowania** > **Uruchom bez debugowania** Uruchom przeglądarkę i załadować witryny sieci Web lokalnie.</span><span class="sxs-lookup"><span data-stu-id="a9233-176">Select **Debug** > **Start without debugging** to launch a browser and load the website locally.</span></span> <span data-ttu-id="a9233-177">Skopiuj adres URL z paska adresu.</span><span class="sxs-lookup"><span data-stu-id="a9233-177">Copy the URL from the address bar.</span></span>

1. <span data-ttu-id="a9233-178">Otwórz inne wystąpienie przeglądarki (w dowolnej przeglądarce) i wklej adres URL w pasku adresu.</span><span class="sxs-lookup"><span data-stu-id="a9233-178">Open another browser instance (any browser) and paste the URL in the address bar.</span></span>

1. <span data-ttu-id="a9233-179">Wybierz albo przeglądarki, wprowadź nazwę i komunikat, a następnie kliknij przycisk **wysyłania** przycisku.</span><span class="sxs-lookup"><span data-stu-id="a9233-179">Choose either browser, enter a name and message, and click the **Send** button.</span></span> <span data-ttu-id="a9233-180">Nazwa i wiadomości są wyświetlane na obu stronach natychmiast.</span><span class="sxs-lookup"><span data-stu-id="a9233-180">The name and message are displayed on both pages instantly.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="a9233-181">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="a9233-181">Visual Studio Code</span></span>](#tab/visual-studio-code)

1. <span data-ttu-id="a9233-182">Naciśnij klawisz **debugowania** (F5), aby skompilować i uruchomić program.</span><span class="sxs-lookup"><span data-stu-id="a9233-182">Press **Debug** (F5) to build and run the program.</span></span> <span data-ttu-id="a9233-183">Uruchomienie programu powoduje otwarcie okna przeglądarki.</span><span class="sxs-lookup"><span data-stu-id="a9233-183">Running the program opens a browser window.</span></span>

1. <span data-ttu-id="a9233-184">Otwarte inne okno przeglądarki i obciążenia witryny sieci Web lokalnie w nim.</span><span class="sxs-lookup"><span data-stu-id="a9233-184">Open another browser window and load the website locally in it.</span></span>

1. <span data-ttu-id="a9233-185">Wybierz albo przeglądarki, wprowadź nazwę i komunikat, a następnie kliknij przycisk **wysyłania** przycisku.</span><span class="sxs-lookup"><span data-stu-id="a9233-185">Choose either browser, enter a name and message, and click the **Send** button.</span></span> <span data-ttu-id="a9233-186">Nazwa i wiadomości są wyświetlane na obu stronach natychmiast.</span><span class="sxs-lookup"><span data-stu-id="a9233-186">The name and message are displayed on both pages instantly.</span></span>

---

  ![Rozwiązanie](signalr/_static/signalr-get-started-finished.png)

## <a name="related-resources"></a><span data-ttu-id="a9233-188">Powiązane zasoby</span><span class="sxs-lookup"><span data-stu-id="a9233-188">Related resources</span></span>

[<span data-ttu-id="a9233-189">Wprowadzenie do SignalR platformy ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="a9233-189">Introduction to ASP.NET Core SignalR</span></span>](xref:signalr/introduction)
