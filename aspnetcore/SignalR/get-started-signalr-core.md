---
title: Rozpoczynanie pracy z SignalR platformy ASP.NET Core
author: rachelappel
ms.author: rachelap
description: "W tym samouczku utworzysz aplikację dla platformy ASP.NET Core za pomocą biblioteki SignalR."
manager: wpickett
ms.date: 03/06/2018
ms.topic: tutorial
ms.technology: dotnet-signalr
ms.prod: aspnet-core
ms.custom: mvc
uid: signalr/get-started-signalr-core
ms.openlocfilehash: 4afb9785fc3d0f472226a745537acbc77adefb4c
ms.sourcegitcommit: 9622bdc6326c28c3322c70000468a80ef21ad376
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/12/2018
---
# <a name="tutorial-get-started-with-signalr-for-aspnet-core"></a><span data-ttu-id="9643a-103">Samouczek: Rozpoczynanie pracy z SignalR dla platformy ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="9643a-103">Tutorial: Get started with SignalR for ASP.NET Core</span></span>

<span data-ttu-id="9643a-104">Przez [Rachel Appel](https://twitter.com/rachelappel)</span><span class="sxs-lookup"><span data-stu-id="9643a-104">By [Rachel Appel](https://twitter.com/rachelappel)</span></span>

<span data-ttu-id="9643a-105">Ten samouczek zawiera podstawowe informacje dotyczące tworzenia aplikacji w czasie rzeczywistym, za pomocą biblioteki SignalR dla platformy ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="9643a-105">This tutorial teaches the basics of building a real-time app using SignalR for ASP.NET Core.</span></span>

   ![Rozwiązanie](get-started-signalr-core/_static/signalr-get-started-finished.png)

<span data-ttu-id="9643a-107">[Wyświetlić lub pobrać przykładowy kod](https://github.com/aspnet/Docs/tree/master/aspnetcore/signalr/get-started-signalr-core/sample/) ([sposobu pobierania](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="9643a-107">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/signalr/get-started-signalr-core/sample/) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

<span data-ttu-id="9643a-108">W tym samouczku przedstawiono następujące SignalR zadań związanych z projektowaniem:</span><span class="sxs-lookup"><span data-stu-id="9643a-108">This tutorial demonstrates the following SignalR development tasks:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="9643a-109">Tworzenie aplikacji sieci web platformy ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="9643a-109">Create an ASP.NET Core web app.</span></span>
> * <span data-ttu-id="9643a-110">Utwórz koncentratora SignalR do dystrybuowania zawartości do klientów.</span><span class="sxs-lookup"><span data-stu-id="9643a-110">Create a SignalR hub to push content to clients.</span></span>
> * <span data-ttu-id="9643a-111">Biblioteka SignalR JavaScript służy do wysyłania wiadomości i wyświetlać aktualizacje w Centrum.</span><span class="sxs-lookup"><span data-stu-id="9643a-111">Use the SignalR JavaScript library to send messages and display updates from the hub.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="9643a-112">Wymagania wstępne</span><span class="sxs-lookup"><span data-stu-id="9643a-112">Prerequisites</span></span>

<span data-ttu-id="9643a-113">Należy zainstalować następujące oprogramowanie:</span><span class="sxs-lookup"><span data-stu-id="9643a-113">Install the following software:</span></span>

* <span data-ttu-id="9643a-114">[1 — zestaw SDK w wersji Preview .NET core 2.1.0](https://www.microsoft.com/net/download/dotnet-core/sdk-2.1.300-preview1) lub nowszy</span><span class="sxs-lookup"><span data-stu-id="9643a-114">[.NET Core 2.1.0 Preview 1 SDK](https://www.microsoft.com/net/download/dotnet-core/sdk-2.1.300-preview1) or later</span></span>
* <span data-ttu-id="9643a-115">[Visual Studio 2017](https://www.visualstudio.com/downloads/) wersji 15.6 lub nowszej z obciążeniem, ASP.NET i sieć web development</span><span class="sxs-lookup"><span data-stu-id="9643a-115">[Visual Studio 2017](https://www.visualstudio.com/downloads/) version 15.6 or later with the ASP.NET and web development workload</span></span>
* [<span data-ttu-id="9643a-116">npm</span><span class="sxs-lookup"><span data-stu-id="9643a-116">npm</span></span>](https://www.npmjs.com/get-npm)

## <a name="create-an-aspnet-core-project-that-hosts-signalr-client-and-server"></a><span data-ttu-id="9643a-117">Tworzenie projektu platformy ASP.NET Core obsługującego SignalR klienta i serwera</span><span class="sxs-lookup"><span data-stu-id="9643a-117">Create an ASP.NET Core project that hosts SignalR client and server</span></span>

1. <span data-ttu-id="9643a-118">Użyj **pliku** > **nowy projekt** menu opcję i wybierz polecenie **aplikacji sieci Web platformy ASP.NET Core**.</span><span class="sxs-lookup"><span data-stu-id="9643a-118">Use the **File** > **New Project** menu option and choose **ASP.NET Core Web Application**.</span></span> <span data-ttu-id="9643a-119">Nazwij projekt `SignalRChat`.</span><span class="sxs-lookup"><span data-stu-id="9643a-119">Name the project `SignalRChat`.</span></span>

  ![Okno dialogowe nowego projektu w programie Visual Studio](get-started-signalr-core/_static/signalr-new-project-dialog.png)

2. <span data-ttu-id="9643a-121">Wybierz **aplikacji sieci Web** do tworzenia projektu za pomocą stron Razor.</span><span class="sxs-lookup"><span data-stu-id="9643a-121">Select **Web Application** to create a project using Razor Pages.</span></span> <span data-ttu-id="9643a-122">Następnie wybierz **Ok**.</span><span class="sxs-lookup"><span data-stu-id="9643a-122">Then select **Ok**.</span></span> <span data-ttu-id="9643a-123">Upewnij się, że **platformy ASP.NET Core 2.1** wybrano selektora framework, chociaż SignalR działa na starsze wersje programu .NET.</span><span class="sxs-lookup"><span data-stu-id="9643a-123">Be sure that **ASP.NET Core 2.1** is selected from the framework selector, though SignalR runs on older versions of .NET.</span></span>

  ![Okno dialogowe nowego projektu w programie Visual Studio](get-started-signalr-core/_static/signalr-new-project-choose-type.png)

  <span data-ttu-id="9643a-125">Biblioteki, w których hosta SignalR kod po stronie serwera są zawarte w szablonie projektu.</span><span class="sxs-lookup"><span data-stu-id="9643a-125">The libraries that host SignalR server-side code are included in the project template.</span></span> <span data-ttu-id="9643a-126">Zainstaluj JavaScript po stronie klienta oddzielnie z [npm](https://www.npmjs.com/).</span><span class="sxs-lookup"><span data-stu-id="9643a-126">Install the client-side JavaScript separately with [npm](https://www.npmjs.com/).</span></span>

  ```console
   npm install @aspnet/signalr
  ```

3. <span data-ttu-id="9643a-127">Kopiuj *signalr.js* z *node_modules\\ @aspnet\signalr\dist\browser*  do *wwwroot\lib* folder w projekcie.</span><span class="sxs-lookup"><span data-stu-id="9643a-127">Copy the *signalr.js* from *node_modules\\@aspnet\signalr\dist\browser* to the *wwwroot\lib* folder in your project.</span></span>

## <a name="create-the-signalr-hub"></a><span data-ttu-id="9643a-128">Tworzenie Centrum SignalR</span><span class="sxs-lookup"><span data-stu-id="9643a-128">Create the SignalR Hub</span></span>

<span data-ttu-id="9643a-129">Koncentrator jest klasa, która służy jako wysokiego poziomu potok, który umożliwia klienta i serwera, wywoływanie metod na siebie.</span><span class="sxs-lookup"><span data-stu-id="9643a-129">A hub is a class that serves as a high-level pipeline that allows the client and server to call methods on each other.</span></span>

1. <span data-ttu-id="9643a-130">Dodaj do projektu klasę, wybierając **pliku** > **nowy** > **pliku** i wybierając **Visual C# klasy**.</span><span class="sxs-lookup"><span data-stu-id="9643a-130">Add a class to the project by choosing **File** > **New** > **File** and selecting **Visual C# Class**.</span></span> 

1. <span data-ttu-id="9643a-131">Dziedzicz `Microsoft.AspNetCore.SignalR.Hub`.</span><span class="sxs-lookup"><span data-stu-id="9643a-131">Inherit from `Microsoft.AspNetCore.SignalR.Hub`.</span></span> <span data-ttu-id="9643a-132">`Hub` Zawiera klasę, właściwości i zdarzeń związanych z zarządzaniem połączeń i grupy, a także wysyłania i odbierania danych.</span><span class="sxs-lookup"><span data-stu-id="9643a-132">The `Hub` class contains properties and events for managing connections and groups, as well as sending and receiving data.</span></span>

1. <span data-ttu-id="9643a-133">Utwórz `Send` metodę, która wysyła komunikat do wszystkich klientów połączonych rozmów.</span><span class="sxs-lookup"><span data-stu-id="9643a-133">Create the `Send` method that sends a message to all connected chat clients.</span></span> <span data-ttu-id="9643a-134">Zwróć uwagę, zwraca `Task`, ponieważ asynchroniczny SignalR.</span><span class="sxs-lookup"><span data-stu-id="9643a-134">Notice it returns a `Task`, because SignalR is asynchronous.</span></span> <span data-ttu-id="9643a-135">Kod asynchroniczny skaluje się lepiej.</span><span class="sxs-lookup"><span data-stu-id="9643a-135">Asynchronous code scales better.</span></span>

  [!code-csharp[Startup](get-started-signalr-core/sample/Hubs/ChatHub.cs?range=7-14)]

## <a name="configure-the-project-to-use-signalr"></a><span data-ttu-id="9643a-136">Konfigurowanie projektu do użycia biblioteki SignalR</span><span class="sxs-lookup"><span data-stu-id="9643a-136">Configure the project to use SignalR</span></span>

<span data-ttu-id="9643a-137">Serwer SignalR musi być skonfigurowany, tak aby wie, do przekazywania żądań do SignalR.</span><span class="sxs-lookup"><span data-stu-id="9643a-137">The SignalR server must be configured so that it knows to pass requests to SignalR.</span></span>

1. <span data-ttu-id="9643a-138">Aby skonfigurować projekt SignalR, zmodyfikuj `ConfigureServices` metoda aplikacji `Startup` przy Wstawianie wywołania do `services.AddSignalR`.</span><span class="sxs-lookup"><span data-stu-id="9643a-138">To configure a SignalR project, modify the `ConfigureServices` method of the application's `Startup` class by inserting a call to `services.AddSignalR`.</span></span>

  <span data-ttu-id="9643a-139">`services.AddSignalR` dodaje SignalR jako część [oprogramowanie pośredniczące platformy ASP.NET Core](xref:fundamentals/middleware/index) potoku.</span><span class="sxs-lookup"><span data-stu-id="9643a-139">`services.AddSignalR` adds SignalR as part of the [ASP.NET Core middleware](xref:fundamentals/middleware/index) pipeline.</span></span>

1. <span data-ttu-id="9643a-140">Do konfigurowania tras sieci hubs przy użyciu `UseSignalR`.</span><span class="sxs-lookup"><span data-stu-id="9643a-140">Configure routes to your hubs using `UseSignalR`.</span></span>

  [!code-csharp[Startup](get-started-signalr-core/sample/Startup.cs?highlight=22,40-43)]

## <a name="create-the-signalr-client-code"></a><span data-ttu-id="9643a-141">Utwórz kod klienta SignalR</span><span class="sxs-lookup"><span data-stu-id="9643a-141">Create the SignalR client code</span></span>

1. <span data-ttu-id="9643a-142">Zamień zawartość *Pages\Index.cshtml* następującym kodem:</span><span class="sxs-lookup"><span data-stu-id="9643a-142">Replace the content in *Pages\Index.cshtml* with the following code:</span></span>

  [!code-cshtml[Index](get-started-signalr-core/sample/Pages/Index.cshtml)]

  <span data-ttu-id="9643a-143">Poprzedni kod HTML Wyświetla nazwę i pola wiadomości i przycisk przesyłania.</span><span class="sxs-lookup"><span data-stu-id="9643a-143">The preceding HTML displays name and message fields, and a submit button.</span></span> <span data-ttu-id="9643a-144">Zwróć uwagę, odwołań do skryptów u dołu: odwołanie do biblioteki SignalR i *chat.js*.</span><span class="sxs-lookup"><span data-stu-id="9643a-144">Notice the script references at the bottom: a reference to SignalR and *chat.js*.</span></span>

1. <span data-ttu-id="9643a-145">Dodaj plik JavaScript *wwwroot\js* folder o nazwie *chat.js* i Dodaj do niej następujący kod:</span><span class="sxs-lookup"><span data-stu-id="9643a-145">Add a JavaScript file to the *wwwroot\js* folder named *chat.js* and add the following code to it:</span></span>

  [!code-javascript[Index](get-started-signalr-core/sample/wwwroot/js/chat.js)]

## <a name="run-the-app"></a><span data-ttu-id="9643a-146">Uruchamianie aplikacji</span><span class="sxs-lookup"><span data-stu-id="9643a-146">Run the app</span></span>

1. <span data-ttu-id="9643a-147">Wybierz **debugowania** > **uruchomienie bez debugowania** przeglądarkę i załadować witryny sieci Web lokalnie.</span><span class="sxs-lookup"><span data-stu-id="9643a-147">Select **Debug** > **Start without debugging** to launch a browser and load the website locally.</span></span> <span data-ttu-id="9643a-148">Skopiuj adres URL na pasku adresu.</span><span class="sxs-lookup"><span data-stu-id="9643a-148">Copy the URL from the address bar.</span></span>

1. <span data-ttu-id="9643a-149">Otwórz inne wystąpienie przeglądarki (dowolnej przeglądarki) i wklej adres URL na pasku adresu.</span><span class="sxs-lookup"><span data-stu-id="9643a-149">Open another browser instance (any browser) and paste the URL in the address bar.</span></span>

1. <span data-ttu-id="9643a-150">Wybierz albo przeglądarkę, wprowadź nazwę i wiadomość, a następnie kliknij przycisk **wysyłania** przycisku.</span><span class="sxs-lookup"><span data-stu-id="9643a-150">Choose either browser, enter a name and message, and click the **Send** button.</span></span> <span data-ttu-id="9643a-151">Nazwa i wiadomości są wyświetlane na obu stronach natychmiast.</span><span class="sxs-lookup"><span data-stu-id="9643a-151">The name and message are displayed on both pages instantly.</span></span>

  ![Rozwiązanie](get-started-signalr-core/_static/signalr-get-started-finished.png)
