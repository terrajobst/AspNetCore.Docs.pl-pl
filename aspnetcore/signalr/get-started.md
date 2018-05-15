---
title: Rozpoczynanie pracy z SignalR platformy ASP.NET Core
author: rachelappel
description: W tym samouczku utworzysz aplikację dla platformy ASP.NET Core za pomocą biblioteki SignalR.
manager: wpickett
monikerRange: '>= aspnetcore-2.1'
ms.author: rachelap
ms.custom: mvc
ms.date: 05/09/2018
ms.prod: aspnet-core
ms.topic: tutorial
ms.technology: aspnet
uid: signalr/get-started
ms.openlocfilehash: 11c8cf079a0922e925060ad3d439ba5e095ae22e
ms.sourcegitcommit: 74be78285ea88772e7dad112f80146b6ed00e53e
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 05/10/2018
---
# <a name="get-started-with-signalr-on-aspnet-core"></a><span data-ttu-id="bfbc2-103">Rozpoczynanie pracy z SignalR platformy ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="bfbc2-103">Get started with SignalR on ASP.NET Core</span></span>

<span data-ttu-id="bfbc2-104">Przez [Rachel Appel](https://twitter.com/rachelappel)</span><span class="sxs-lookup"><span data-stu-id="bfbc2-104">By [Rachel Appel](https://twitter.com/rachelappel)</span></span>

<span data-ttu-id="bfbc2-105">Ten samouczek zawiera podstawowe informacje dotyczące tworzenia aplikacji w czasie rzeczywistym, za pomocą biblioteki SignalR dla platformy ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="bfbc2-105">This tutorial teaches the basics of building a real-time app using SignalR for ASP.NET Core.</span></span>

   ![Rozwiązanie](get-started/_static/signalr-get-started-finished.png)

<span data-ttu-id="bfbc2-107">W tym samouczku przedstawiono następujące SignalR zadań związanych z projektowaniem:</span><span class="sxs-lookup"><span data-stu-id="bfbc2-107">This tutorial demonstrates the following SignalR development tasks:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="bfbc2-108">Utwórz SignalR w aplikacji sieci web platformy ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="bfbc2-108">Create a SignalR on ASP.NET Core web app.</span></span>
> * <span data-ttu-id="bfbc2-109">Utwórz koncentratora SignalR do dystrybuowania zawartości do klientów.</span><span class="sxs-lookup"><span data-stu-id="bfbc2-109">Create a SignalR hub to push content to clients.</span></span>
> * <span data-ttu-id="bfbc2-110">Modyfikowanie `Startup` klasy i skonfigurowania aplikacji.</span><span class="sxs-lookup"><span data-stu-id="bfbc2-110">Modify the `Startup` class and configure the app.</span></span>

<span data-ttu-id="bfbc2-111">[Wyświetlić lub pobrać przykładowy kod](https://github.com/aspnet/Docs/tree/master/aspnetcore/signalr/get-started/sample/) ([sposobu pobierania](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="bfbc2-111">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/signalr/get-started/sample/) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

# <a name="prerequisites"></a><span data-ttu-id="bfbc2-112">Wymagania wstępne</span><span class="sxs-lookup"><span data-stu-id="bfbc2-112">Prerequisites</span></span>

<span data-ttu-id="bfbc2-113">Należy zainstalować następujące oprogramowanie:</span><span class="sxs-lookup"><span data-stu-id="bfbc2-113">Install the following software:</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="bfbc2-114">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="bfbc2-114">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="bfbc2-115">[Zestaw SDK programu .NET core 2.1.0 RC 1](https://www.microsoft.com/net/download/dotnet-core/sdk-2.1.300-rc1) lub nowszy</span><span class="sxs-lookup"><span data-stu-id="bfbc2-115">[.NET Core 2.1.0 RC 1 SDK](https://www.microsoft.com/net/download/dotnet-core/sdk-2.1.300-rc1) or later</span></span>
* <span data-ttu-id="bfbc2-116">[Visual Studio 2017](https://www.visualstudio.com/downloads/) wersji 15.7 lub nowszym z **ASP.NET i sieć web development** obciążenia</span><span class="sxs-lookup"><span data-stu-id="bfbc2-116">[Visual Studio 2017](https://www.visualstudio.com/downloads/) version 15.7 or later with the **ASP.NET and web development** workload</span></span>
* [<span data-ttu-id="bfbc2-117">npm</span><span class="sxs-lookup"><span data-stu-id="bfbc2-117">npm</span></span>](https://www.npmjs.com/get-npm)

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="bfbc2-118">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="bfbc2-118">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="bfbc2-119">[Zestaw SDK programu .NET core 2.1.0 RC 1](https://www.microsoft.com/net/download/dotnet-core/sdk-2.1.300-rc1) lub nowszy</span><span class="sxs-lookup"><span data-stu-id="bfbc2-119">[.NET Core 2.1.0 RC 1 SDK](https://www.microsoft.com/net/download/dotnet-core/sdk-2.1.300-rc1) or later</span></span>
* [<span data-ttu-id="bfbc2-120">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="bfbc2-120">Visual Studio Code</span></span>](https://code.visualstudio.com/download)
* [<span data-ttu-id="bfbc2-121">C# dla programu Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="bfbc2-121">C# for Visual Studio Code</span></span>](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp)
* [<span data-ttu-id="bfbc2-122">npm</span><span class="sxs-lookup"><span data-stu-id="bfbc2-122">npm</span></span>](https://www.npmjs.com/get-npm)

-----

## <a name="create-an-aspnet-core-project-that-hosts-signalr-client-and-server"></a><span data-ttu-id="bfbc2-123">Tworzenie projektu platformy ASP.NET Core obsługującego SignalR klienta i serwera</span><span class="sxs-lookup"><span data-stu-id="bfbc2-123">Create an ASP.NET Core project that hosts SignalR client and server</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="bfbc2-124">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="bfbc2-124">Visual Studio</span></span>](#tab/visual-studio/)

1. <span data-ttu-id="bfbc2-125">Użyj **pliku** > **nowy projekt** menu opcję i wybierz polecenie **aplikacji sieci Web platformy ASP.NET Core**.</span><span class="sxs-lookup"><span data-stu-id="bfbc2-125">Use the **File** > **New Project** menu option and choose **ASP.NET Core Web Application**.</span></span> <span data-ttu-id="bfbc2-126">Nazwij projekt *SignalRChat*.</span><span class="sxs-lookup"><span data-stu-id="bfbc2-126">Name the project *SignalRChat*.</span></span>

   ![Okno dialogowe nowego projektu w programie Visual Studio](get-started/_static/signalr-new-project-dialog.png)

2. <span data-ttu-id="bfbc2-128">Wybierz **aplikacji sieci Web** do tworzenia projektu za pomocą stron Razor.</span><span class="sxs-lookup"><span data-stu-id="bfbc2-128">Select **Web Application** to create a project using Razor Pages.</span></span> <span data-ttu-id="bfbc2-129">Następnie wybierz **OK**.</span><span class="sxs-lookup"><span data-stu-id="bfbc2-129">Then select **OK**.</span></span> <span data-ttu-id="bfbc2-130">Upewnij się, że **platformy ASP.NET Core 2.1** wybrano selektora framework, chociaż SignalR działa na starsze wersje programu .NET.</span><span class="sxs-lookup"><span data-stu-id="bfbc2-130">Be sure that **ASP.NET Core 2.1** is selected from the framework selector, though SignalR runs on older versions of .NET.</span></span>

   ![Okno dialogowe nowego projektu w programie Visual Studio](get-started/_static/signalr-new-project-choose-type.png)

<span data-ttu-id="bfbc2-132">Visual Studio zawiera `Microsoft.AspNetCore.SignalR` pakiet zawierający bibliotek serwer jako część jej **aplikacji sieci Web platformy ASP.NET Core** szablonu.</span><span class="sxs-lookup"><span data-stu-id="bfbc2-132">Visual Studio includes the `Microsoft.AspNetCore.SignalR` package containing its server libraries as part of its **ASP.NET Core Web Application** template.</span></span> <span data-ttu-id="bfbc2-133">Jednak biblioteka klienta języka JavaScript dla biblioteki SignalR musi być zainstalowany przy użyciu *npm*.</span><span class="sxs-lookup"><span data-stu-id="bfbc2-133">However, the JavaScript client library for SignalR must be installed using *npm*.</span></span>

3. <span data-ttu-id="bfbc2-134">Uruchom następujące polecenia **Konsola Menedżera pakietów** okno z katalogu głównego projektu:</span><span class="sxs-lookup"><span data-stu-id="bfbc2-134">Run the following commands in the **Package Manager Console** window, from the project root:</span></span>

    ```console
      npm init -y
      npm install @aspnet/signalr
    ```     

4. <span data-ttu-id="bfbc2-135">Kopiuj *signalr.js* plik z *node_modules\\ @aspnet\signalr\dist\browser*  do *lib* folder w projekcie.</span><span class="sxs-lookup"><span data-stu-id="bfbc2-135">Copy the *signalr.js* file from *node_modules\\@aspnet\signalr\dist\browser* to the *lib* folder in your project.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="bfbc2-136">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="bfbc2-136">Visual Studio Code</span></span>](#tab/visual-studio-code/)

1. <span data-ttu-id="bfbc2-137">Z **zintegrowane Terminal**, uruchom następujące polecenie:</span><span class="sxs-lookup"><span data-stu-id="bfbc2-137">From the **Integrated Terminal**, run the following command:</span></span>

    ```console
      dotnet new razor -o SignalRChat
    ```

2. <span data-ttu-id="bfbc2-138">Zainstaluj JavaScript klienta biblioteki przy użyciu *npm*.</span><span class="sxs-lookup"><span data-stu-id="bfbc2-138">Install the JavaScript client library using *npm*.</span></span>

    ```
      npm init -y
      npm install @aspnet/signalr
    ```

3. <span data-ttu-id="bfbc2-139">Kopiuj *signalr.js* plik z *node_modules\\ @aspnet\signalr\dist\browser*  do *lib* folder w projekcie.</span><span class="sxs-lookup"><span data-stu-id="bfbc2-139">Copy the *signalr.js* file from *node_modules\\@aspnet\signalr\dist\browser* to the *lib* folder in your project.</span></span>

-----

## <a name="create-the-signalr-hub"></a><span data-ttu-id="bfbc2-140">Tworzenie Centrum SignalR</span><span class="sxs-lookup"><span data-stu-id="bfbc2-140">Create the SignalR Hub</span></span>

<span data-ttu-id="bfbc2-141">Koncentrator jest klasa, która służy jako wysokiego poziomu potok, który umożliwia klienta i serwera, wywoływanie metod na siebie.</span><span class="sxs-lookup"><span data-stu-id="bfbc2-141">A hub is a class that serves as a high-level pipeline that allows the client and server to call methods on each other.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="bfbc2-142">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="bfbc2-142">Visual Studio</span></span>](#tab/visual-studio/)

1. <span data-ttu-id="bfbc2-143">Dodaj do projektu klasę, wybierając **pliku** > **nowy** > **pliku** i wybierając **Visual C# klasy**.</span><span class="sxs-lookup"><span data-stu-id="bfbc2-143">Add a class to the project by choosing **File** > **New** > **File** and selecting **Visual C# Class**.</span></span>

2. <span data-ttu-id="bfbc2-144">Dziedzicz `Microsoft.AspNetCore.SignalR.Hub`.</span><span class="sxs-lookup"><span data-stu-id="bfbc2-144">Inherit from `Microsoft.AspNetCore.SignalR.Hub`.</span></span> <span data-ttu-id="bfbc2-145">`Hub` Zawiera klasę, właściwości i zdarzeń związanych z zarządzaniem połączeń i grupy, a także wysyłania i odbierania danych.</span><span class="sxs-lookup"><span data-stu-id="bfbc2-145">The `Hub` class contains properties and events for managing connections and groups, as well as sending and receiving data.</span></span>

3. <span data-ttu-id="bfbc2-146">Utwórz `SendMessage` metodę, która wysyła komunikat do wszystkich klientów połączonych rozmów.</span><span class="sxs-lookup"><span data-stu-id="bfbc2-146">Create the `SendMessage` method that sends a message to all connected chat clients.</span></span> <span data-ttu-id="bfbc2-147">Zwróć uwagę, zwraca [zadań](https://msdn.microsoft.com/library/system.threading.tasks.task(v=vs.110).aspx), ponieważ asynchroniczny SignalR.</span><span class="sxs-lookup"><span data-stu-id="bfbc2-147">Notice it returns a [Task](https://msdn.microsoft.com/library/system.threading.tasks.task(v=vs.110).aspx), because SignalR is asynchronous.</span></span> <span data-ttu-id="bfbc2-148">Kod asynchroniczny skaluje się lepiej.</span><span class="sxs-lookup"><span data-stu-id="bfbc2-148">Asynchronous code scales better.</span></span>

   [!code-csharp[Startup](get-started/sample/Hubs/ChatHub.cs)]

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="bfbc2-149">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="bfbc2-149">Visual Studio Code</span></span>](#tab/visual-studio-code/)

1. <span data-ttu-id="bfbc2-150">Otwórz *SignalRChat* folderu w programie Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="bfbc2-150">Open the *SignalRChat* folder in Visual Studio Code.</span></span>

2. <span data-ttu-id="bfbc2-151">Dodaj klasę do projektu, wybierając **pliku** > **nowy plik** z menu.</span><span class="sxs-lookup"><span data-stu-id="bfbc2-151">Add a class to the project by selecting **File** > **New File** from the menu.</span></span>

3. <span data-ttu-id="bfbc2-152">Dziedzicz `Microsoft.AspNetCore.SignalR.Hub`.</span><span class="sxs-lookup"><span data-stu-id="bfbc2-152">Inherit from `Microsoft.AspNetCore.SignalR.Hub`.</span></span> <span data-ttu-id="bfbc2-153">`Hub` Zawiera klasę, właściwości i zdarzeń związanych z zarządzaniem połączeń i grupy, a także wysyłania i odbierania danych do klientów.</span><span class="sxs-lookup"><span data-stu-id="bfbc2-153">The `Hub` class contains properties and events for managing connections and groups, as well as sending and receiving data to clients.</span></span>

4. <span data-ttu-id="bfbc2-154">Dodaj `SendMessage` metodę do klasy.</span><span class="sxs-lookup"><span data-stu-id="bfbc2-154">Add a `SendMessage` method to the class.</span></span> <span data-ttu-id="bfbc2-155">`SendMessage` Metoda wysyła komunikat do wszystkich klientów połączonych rozmów.</span><span class="sxs-lookup"><span data-stu-id="bfbc2-155">The `SendMessage` method sends a message to all connected chat clients.</span></span> <span data-ttu-id="bfbc2-156">Zwróć uwagę, zwraca [zadań](/dotnet/api/system.threading.tasks.task), ponieważ asynchroniczny SignalR.</span><span class="sxs-lookup"><span data-stu-id="bfbc2-156">Notice it returns a [Task](/dotnet/api/system.threading.tasks.task), because SignalR is asynchronous.</span></span> <span data-ttu-id="bfbc2-157">Kod asynchroniczny skaluje się lepiej.</span><span class="sxs-lookup"><span data-stu-id="bfbc2-157">Asynchronous code scales better.</span></span>

   [!code-csharp[Startup](get-started/sample/Hubs/ChatHub.cs?range=6-12)]

-----

## <a name="configure-the-project-to-use-signalr"></a><span data-ttu-id="bfbc2-158">Konfigurowanie projektu do użycia biblioteki SignalR</span><span class="sxs-lookup"><span data-stu-id="bfbc2-158">Configure the project to use SignalR</span></span>

<span data-ttu-id="bfbc2-159">Serwer SignalR musi być skonfigurowany, tak aby wie, do przekazywania żądań do SignalR.</span><span class="sxs-lookup"><span data-stu-id="bfbc2-159">The SignalR server must be configured so that it knows to pass requests to SignalR.</span></span>

1. <span data-ttu-id="bfbc2-160">Aby skonfigurować projekt SignalR, zmodyfikuj projektu `Startup.ConfigureServices` metody.</span><span class="sxs-lookup"><span data-stu-id="bfbc2-160">To configure a SignalR project, modify the project's `Startup.ConfigureServices` method.</span></span>

   <span data-ttu-id="bfbc2-161">`services.AddSignalR` dodaje SignalR jako część [oprogramowanie pośredniczące](xref:fundamentals/middleware/index) potoku.</span><span class="sxs-lookup"><span data-stu-id="bfbc2-161">`services.AddSignalR` adds SignalR as part of the [middleware](xref:fundamentals/middleware/index) pipeline.</span></span>

2. <span data-ttu-id="bfbc2-162">Do konfigurowania tras sieci hubs przy użyciu `UseSignalR`.</span><span class="sxs-lookup"><span data-stu-id="bfbc2-162">Configure routes to your hubs using `UseSignalR`.</span></span>


   [!code-csharp[Startup](get-started/sample/Startup.cs?highlight=37,57-60)]


## <a name="create-the-signalr-client-code"></a><span data-ttu-id="bfbc2-163">Utwórz kod klienta SignalR</span><span class="sxs-lookup"><span data-stu-id="bfbc2-163">Create the SignalR client code</span></span>

1. <span data-ttu-id="bfbc2-164">Zamień zawartość *Pages\Index.cshtml* następującym kodem:</span><span class="sxs-lookup"><span data-stu-id="bfbc2-164">Replace the content in *Pages\Index.cshtml* with the following code:</span></span>

   [!code-cshtml[Index](get-started/sample/Pages/Index.cshtml)]

   <span data-ttu-id="bfbc2-165">Poprzedni kod HTML Wyświetla nazwę i pola wiadomości i przycisk przesyłania.</span><span class="sxs-lookup"><span data-stu-id="bfbc2-165">The preceding HTML displays name and message fields, and a submit button.</span></span> <span data-ttu-id="bfbc2-166">Zwróć uwagę, odwołań do skryptów u dołu: odwołanie do biblioteki SignalR i *chat.js*.</span><span class="sxs-lookup"><span data-stu-id="bfbc2-166">Notice the script references at the bottom: a reference to SignalR and *chat.js*.</span></span>

2. <span data-ttu-id="bfbc2-167">Dodaj plik JavaScript o nazwie *chat.js*, do *wwwroot\js* folderu.</span><span class="sxs-lookup"><span data-stu-id="bfbc2-167">Add a JavaScript file, named *chat.js*, to the *wwwroot\js* folder.</span></span> <span data-ttu-id="bfbc2-168">Dodaj do niej następujący kod:</span><span class="sxs-lookup"><span data-stu-id="bfbc2-168">Add the following code to it:</span></span>

   [!code-javascript[Index](get-started/sample/wwwroot/js/chat.js)]

## <a name="run-the-app"></a><span data-ttu-id="bfbc2-169">Uruchamianie aplikacji</span><span class="sxs-lookup"><span data-stu-id="bfbc2-169">Run the app</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="bfbc2-170">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="bfbc2-170">Visual Studio</span></span>](#tab/visual-studio)

1. <span data-ttu-id="bfbc2-171">Wybierz **debugowania** > **uruchomienie bez debugowania** przeglądarkę i załadować witryny sieci Web lokalnie.</span><span class="sxs-lookup"><span data-stu-id="bfbc2-171">Select **Debug** > **Start without debugging** to launch a browser and load the website locally.</span></span> <span data-ttu-id="bfbc2-172">Skopiuj adres URL na pasku adresu.</span><span class="sxs-lookup"><span data-stu-id="bfbc2-172">Copy the URL from the address bar.</span></span>

1. <span data-ttu-id="bfbc2-173">Otwórz inne wystąpienie przeglądarki (dowolnej przeglądarki) i wklej adres URL na pasku adresu.</span><span class="sxs-lookup"><span data-stu-id="bfbc2-173">Open another browser instance (any browser) and paste the URL in the address bar.</span></span>

1. <span data-ttu-id="bfbc2-174">Wybierz albo przeglądarkę, wprowadź nazwę i wiadomość, a następnie kliknij przycisk **wysyłania** przycisku.</span><span class="sxs-lookup"><span data-stu-id="bfbc2-174">Choose either browser, enter a name and message, and click the **Send** button.</span></span> <span data-ttu-id="bfbc2-175">Nazwa i wiadomości są wyświetlane na obu stronach natychmiast.</span><span class="sxs-lookup"><span data-stu-id="bfbc2-175">The name and message are displayed on both pages instantly.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="bfbc2-176">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="bfbc2-176">Visual Studio Code</span></span>](#tab/visual-studio-code)

1. <span data-ttu-id="bfbc2-177">Naciśnij klawisz **debugowania** (F5), aby skompilować i uruchomić program.</span><span class="sxs-lookup"><span data-stu-id="bfbc2-177">Press **Debug** (F5) to build and run the program.</span></span> <span data-ttu-id="bfbc2-178">Uruchomienie programu powoduje otwarcie okna przeglądarki.</span><span class="sxs-lookup"><span data-stu-id="bfbc2-178">Running the program opens a browser window.</span></span>

1. <span data-ttu-id="bfbc2-179">Otwiera inne okno przeglądarki i załadować witryny sieci Web lokalnie w nim.</span><span class="sxs-lookup"><span data-stu-id="bfbc2-179">Open another browser window and load the website locally in it.</span></span>

1. <span data-ttu-id="bfbc2-180">Wybierz albo przeglądarkę, wprowadź nazwę i wiadomość, a następnie kliknij przycisk **wysyłania** przycisku.</span><span class="sxs-lookup"><span data-stu-id="bfbc2-180">Choose either browser, enter a name and message, and click the **Send** button.</span></span> <span data-ttu-id="bfbc2-181">Nazwa i wiadomości są wyświetlane na obu stronach natychmiast.</span><span class="sxs-lookup"><span data-stu-id="bfbc2-181">The name and message are displayed on both pages instantly.</span></span>

-----

  ![Rozwiązanie](get-started/_static/signalr-get-started-finished.png)

## <a name="related-resources"></a><span data-ttu-id="bfbc2-183">Zasoby pokrewne</span><span class="sxs-lookup"><span data-stu-id="bfbc2-183">Related resources</span></span>

[<span data-ttu-id="bfbc2-184">Wprowadzenie do platformy ASP.NET Core SignalR</span><span class="sxs-lookup"><span data-stu-id="bfbc2-184">Introduction to ASP.NET Core SignalR</span></span>](introduction.md)