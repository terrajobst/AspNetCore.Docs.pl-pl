---
uid: signalr/overview/getting-started/tutorial-getting-started-with-signalr
title: 'Samouczek: Rozmowy w czasie rzeczywistym przy użyciu SignalR 2 | Dokumentacja firmy Microsoft'
author: bradygaster
description: Ten samouczek pokazuje, jak utworzyć aplikację do obsługi rozmów w czasie rzeczywistym przy użyciu SignalR. Możesz dodać SignalR na pustą aplikację sieci web platformy ASP.NET.
ms.author: bradyg
ms.date: 01/22/2019
ms.assetid: a8b3b778-f009-4369-85c7-e90f9878d8b4
msc.legacyurl: /signalr/overview/getting-started/tutorial-getting-started-with-signalr
msc.type: authoredcontent
ms.topic: tutorial
ms.openlocfilehash: 90f2c03fbda522e3a46200bc0132cc74100ce70f
ms.sourcegitcommit: ebf4e5a7ca301af8494edf64f85d4a8deb61d641
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/24/2019
ms.locfileid: "54836795"
---
# <a name="tutorial-real-time-chat-with-signalr-2"></a><span data-ttu-id="efa6b-104">Samouczek: Rozmowa w czasie rzeczywistym przy użyciu usługi SignalR 2</span><span class="sxs-lookup"><span data-stu-id="efa6b-104">Tutorial: Real-time chat with SignalR 2</span></span>

<span data-ttu-id="efa6b-105">Ten samouczek pokazuje, jak utworzyć aplikację do obsługi rozmów w czasie rzeczywistym przy użyciu SignalR.</span><span class="sxs-lookup"><span data-stu-id="efa6b-105">This tutorial shows you how to use SignalR to create a real-time chat application.</span></span> <span data-ttu-id="efa6b-106">Dodaj SignalR do pustych aplikacji sieci web ASP.NET i Utwórz stronę HTML do wysyłania i wyświetla komunikaty.</span><span class="sxs-lookup"><span data-stu-id="efa6b-106">You add SignalR to an empty ASP.NET web application and create an HTML page to send and display messages.</span></span>

<span data-ttu-id="efa6b-107">W ramach tego samouczka możesz:</span><span class="sxs-lookup"><span data-stu-id="efa6b-107">In this tutorial, you:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="efa6b-108">Konfigurowanie projektu</span><span class="sxs-lookup"><span data-stu-id="efa6b-108">Set up the project</span></span>
> * <span data-ttu-id="efa6b-109">Uruchamianie aplikacji przykładowej</span><span class="sxs-lookup"><span data-stu-id="efa6b-109">Run the sample</span></span>
> * <span data-ttu-id="efa6b-110">Poszukaj w kodzie</span><span class="sxs-lookup"><span data-stu-id="efa6b-110">Examine the code</span></span>

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

## <a name="prerequisites"></a><span data-ttu-id="efa6b-111">Wymagania wstępne</span><span class="sxs-lookup"><span data-stu-id="efa6b-111">Prerequisites</span></span>

* <span data-ttu-id="efa6b-112">[Program Visual Studio 2017](https://visualstudio.microsoft.com/downloads/) z **ASP.NET i tworzenie aplikacji internetowych** obciążenia.</span><span class="sxs-lookup"><span data-stu-id="efa6b-112">[Visual Studio 2017](https://visualstudio.microsoft.com/downloads/) with the **ASP.NET and web development** workload.</span></span>

## <a name="set-up-the-project"></a><span data-ttu-id="efa6b-113">Konfigurowanie projektu</span><span class="sxs-lookup"><span data-stu-id="efa6b-113">Set up the Project</span></span>

<span data-ttu-id="efa6b-114">W tej sekcji pokazano, jak utworzyć pustą aplikację sieci web platformy ASP.NET, za pomocą programu Visual Studio 2017 i SignalR 2 Dodaj SignalR i tworzenie aplikacji czatu.</span><span class="sxs-lookup"><span data-stu-id="efa6b-114">This section shows how to use Visual Studio 2017 and SignalR 2 to create an empty ASP.NET web application, add SignalR, and create the chat application.</span></span>

1. <span data-ttu-id="efa6b-115">W programie Visual Studio należy utworzyć aplikację sieci Web platformy ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="efa6b-115">In Visual Studio, create an ASP.NET Web Application.</span></span>

    ![Tworzenie sieci web](tutorial-getting-started-with-signalr/_static/image2.png)

1. <span data-ttu-id="efa6b-117">W **nowy projekt ASP.NET - SignalRChat** okna, pozostaw **pusty** zaznaczone, a następnie wybierz **OK**.</span><span class="sxs-lookup"><span data-stu-id="efa6b-117">In the **New ASP.NET Project - SignalRChat** window, leave **Empty** selected and select **OK**.</span></span>

1. <span data-ttu-id="efa6b-118">W **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy projekt i wybierz **Dodaj** > **nowy element**.</span><span class="sxs-lookup"><span data-stu-id="efa6b-118">In **Solution Explorer**, right-click the project and select **Add** > **New Item**.</span></span>

1. <span data-ttu-id="efa6b-119">W **Dodaj nowy element - SignalRChat**, wybierz opcję **zainstalowane** > **Visual C#**   >  **Web**  >  **SignalR** , a następnie wybierz **klasa Centrum SignalR (v2)**.</span><span class="sxs-lookup"><span data-stu-id="efa6b-119">In **Add New Item - SignalRChat**, select **Installed** > **Visual C#** > **Web** > **SignalR**  and then select **SignalR Hub Class (v2)**.</span></span>

1. <span data-ttu-id="efa6b-120">Nazwa klasy *ChatHub* i dodaj go do projektu.</span><span class="sxs-lookup"><span data-stu-id="efa6b-120">Name the class *ChatHub* and add it to the project.</span></span>

    <span data-ttu-id="efa6b-121">Spowoduje to utworzenie *ChatHub.cs* plik i dodaje zestaw plików skryptów i odwołania do zestawów, obsługujące bibliotekę SignalR do projektu.</span><span class="sxs-lookup"><span data-stu-id="efa6b-121">This step creates the *ChatHub.cs* class file and adds a set of script files and assembly references that support SignalR to the project.</span></span>

1. <span data-ttu-id="efa6b-122">Zastąp kod w nowej *ChatHub.cs* plik klasy przy użyciu tego kodu:</span><span class="sxs-lookup"><span data-stu-id="efa6b-122">Replace the code in the new *ChatHub.cs* class file with this code:</span></span>

    [!code-csharp[Main](tutorial-getting-started-with-signalr/samples/sample1.cs)]

1. <span data-ttu-id="efa6b-123">W **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy projekt i wybierz **Dodaj** > **nowy element**.</span><span class="sxs-lookup"><span data-stu-id="efa6b-123">In **Solution Explorer**, right-click the project and select **Add** > **New Item**.</span></span>

1. <span data-ttu-id="efa6b-124">W **Dodaj nowy element - SignalRChat** wybierz **zainstalowane** > **Visual C#**   >  **Web** a Wybierz **klasy początkowej OWIN**.</span><span class="sxs-lookup"><span data-stu-id="efa6b-124">In **Add New Item - SignalRChat** select **Installed** > **Visual C#** > **Web**  and then select **OWIN Startup Class**.</span></span>

1. <span data-ttu-id="efa6b-125">Nazwa klasy *uruchamiania* i dodaj go do projektu.</span><span class="sxs-lookup"><span data-stu-id="efa6b-125">Name the class *Startup* and add it to the project.</span></span>

1. <span data-ttu-id="efa6b-126">W **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy projekt i wybierz **Dodaj** > **strony HTML**.</span><span class="sxs-lookup"><span data-stu-id="efa6b-126">In **Solution Explorer**, right-click the project and select **Add** > **HTML Page**.</span></span>

1. <span data-ttu-id="efa6b-127">Nadaj nowej stronie *indeksu* i wybierz **OK**.</span><span class="sxs-lookup"><span data-stu-id="efa6b-127">Name the new page *index* and select **OK**.</span></span>

1. <span data-ttu-id="efa6b-128">W **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy strony HTML został utworzony i wybierz **Ustaw jako strona startowa**.</span><span class="sxs-lookup"><span data-stu-id="efa6b-128">In **Solution Explorer**, right-click the HTML page you created and select **Set as Start Page**.</span></span>

1. <span data-ttu-id="efa6b-129">Zastąp kod domyślnej strony HTML przy użyciu tego kodu:</span><span class="sxs-lookup"><span data-stu-id="efa6b-129">Replace the default code in the HTML page with this code:</span></span>

    [!code-html[Main](tutorial-getting-started-with-signalr/samples/sample3.html)]

1. <span data-ttu-id="efa6b-130">W **Eksploratora rozwiązań**, rozwiń węzeł **skrypty**.</span><span class="sxs-lookup"><span data-stu-id="efa6b-130">In **Solution Explorer**, expand **Scripts**.</span></span>

    <span data-ttu-id="efa6b-131">Biblioteki skryptów, jQuery i SignalR są widoczne w projekcie.</span><span class="sxs-lookup"><span data-stu-id="efa6b-131">Script libraries for jQuery and SignalR are visible in the project.</span></span>

    > [!IMPORTANT]
    > <span data-ttu-id="efa6b-132">Menedżer pakietów może zainstalowano późniejszą wersję skrypty SignalR.</span><span class="sxs-lookup"><span data-stu-id="efa6b-132">The package manager may have installed a later version of the SignalR scripts.</span></span>

1. <span data-ttu-id="efa6b-133">Upewnij się, że odwołania do skryptów w blok kodu odpowiadają wersji plików skrypt w projekcie.</span><span class="sxs-lookup"><span data-stu-id="efa6b-133">Check that the script references in the code block correspond to the versions of the script files in the project.</span></span>

    <span data-ttu-id="efa6b-134">Odwołania do skryptu z oryginalnego bloku kodu:</span><span class="sxs-lookup"><span data-stu-id="efa6b-134">Script references from the original code block:</span></span>
    ```html
    <!--Script references. -->
    <!--Reference the jQuery library. -->
    <script src="Scripts/jquery-3.1.1.min.js" ></script>
    <!--Reference the SignalR library. -->
    <script src="Scripts/jquery.signalR-2.2.1.min.js"></script>
    ```

1. <span data-ttu-id="efa6b-135">Jeśli nie są zgodne, zaktualizuj *.html* pliku.</span><span class="sxs-lookup"><span data-stu-id="efa6b-135">If they don't match, update the *.html* file.</span></span>

1. <span data-ttu-id="efa6b-136">Na pasku menu wybierz **pliku** > **Zapisz wszystko**.</span><span class="sxs-lookup"><span data-stu-id="efa6b-136">From the menu bar, select **File** > **Save All**.</span></span>

## <a name="run-the-sample"></a><span data-ttu-id="efa6b-137">Uruchamianie aplikacji przykładowej</span><span class="sxs-lookup"><span data-stu-id="efa6b-137">Run the Sample</span></span>

1. <span data-ttu-id="efa6b-138">Na pasku narzędzi, Włącz **debugowanie skryptu** a następnie wybierz przycisk Odtwórz, aby uruchomić przykład w trybie debugowania.</span><span class="sxs-lookup"><span data-stu-id="efa6b-138">In the toolbar, turn on **Script Debugging** and then select the play button to run the sample in Debug mode.</span></span>

    ![Wprowadź nazwę użytkownika](tutorial-getting-started-with-signalr/_static/image3.png)

1. <span data-ttu-id="efa6b-140">Po otwarciu przeglądarki, wprowadź nazwę swojej rozmowy tożsamości.</span><span class="sxs-lookup"><span data-stu-id="efa6b-140">When the browser opens, enter a name for your chat identity.</span></span>

1. <span data-ttu-id="efa6b-141">Skopiuj adres URL w przeglądarce otwórz dwóch innych przeglądarek i wklejać adresy URL paski adresu.</span><span class="sxs-lookup"><span data-stu-id="efa6b-141">Copy the URL from the browser, open two other browsers, and paste the URLs into the address bars.</span></span>

1. <span data-ttu-id="efa6b-142">W każdej przeglądarce wprowadź unikatową nazwę.</span><span class="sxs-lookup"><span data-stu-id="efa6b-142">In each browser, enter a unique name.</span></span>

1. <span data-ttu-id="efa6b-143">Teraz Dodaj komentarz i wybierz pozycję **wysyłania**.</span><span class="sxs-lookup"><span data-stu-id="efa6b-143">Now, add a comment and select **Send**.</span></span> <span data-ttu-id="efa6b-144">Powtórz, w innych przeglądarkach.</span><span class="sxs-lookup"><span data-stu-id="efa6b-144">Repeat that in the other browsers.</span></span> <span data-ttu-id="efa6b-145">Komentarze są wyświetlane w czasie rzeczywistym.</span><span class="sxs-lookup"><span data-stu-id="efa6b-145">The comments appear in real-time.</span></span>

    > [!NOTE]
    > <span data-ttu-id="efa6b-146">Tej aplikacji rozmów prostego nie utrzymuje kontekst dyskusji na serwerze.</span><span class="sxs-lookup"><span data-stu-id="efa6b-146">This simple chat application does not maintain the discussion context on the server.</span></span> <span data-ttu-id="efa6b-147">Centrum emituje komentarzy do wszystkich bieżących użytkowników.</span><span class="sxs-lookup"><span data-stu-id="efa6b-147">The hub broadcasts comments to all current users.</span></span> <span data-ttu-id="efa6b-148">Użytkownicy, którzy dołączają do czatu później zostanie wyświetlony komunikat o dodane od czasu dołączenia.</span><span class="sxs-lookup"><span data-stu-id="efa6b-148">Users who join the chat later will see messages added from the time they join.</span></span>

    <span data-ttu-id="efa6b-149">Zobacz, jak działa aplikacji rozmów w trzech różnych przeglądarkach.</span><span class="sxs-lookup"><span data-stu-id="efa6b-149">See how the chat application runs in three different browsers.</span></span> <span data-ttu-id="efa6b-150">Tom Anand i Susan wysyłania wiadomości, wszystkie przeglądarki aktualizacji w czasie rzeczywistym:</span><span class="sxs-lookup"><span data-stu-id="efa6b-150">When Tom, Anand, and Susan send messages, all browsers update in real time:</span></span>

    ![Wszystkie trzy przeglądarki wyświetlenia tych samych Historia](tutorial-getting-started-with-signalr/_static/image4.png)

1. <span data-ttu-id="efa6b-152">W **Eksploratora rozwiązań**, zbadaj **dokumenty skryptów** węzła dla działającej aplikacji.</span><span class="sxs-lookup"><span data-stu-id="efa6b-152">In **Solution Explorer**, inspect the **Script Documents** node for the running application.</span></span> <span data-ttu-id="efa6b-153">Brak pliku skryptu o nazwie *koncentratory* generujący biblioteki SignalR w czasie wykonywania.</span><span class="sxs-lookup"><span data-stu-id="efa6b-153">There's a script file named *hubs* that the SignalR library generates at runtime.</span></span> <span data-ttu-id="efa6b-154">Ten plik zarządza komunikacji między jQuery skryptu i kod po stronie serwera.</span><span class="sxs-lookup"><span data-stu-id="efa6b-154">This file manages the communication between jQuery script and server-side code.</span></span>

    ![wygenerowany automatycznie koncentratory skryptu w węźle dokumentów skryptu](tutorial-getting-started-with-signalr/_static/image5.png)

## <a name="examine-the-code"></a><span data-ttu-id="efa6b-156">Poszukaj w kodzie</span><span class="sxs-lookup"><span data-stu-id="efa6b-156">Examine the Code</span></span>

<span data-ttu-id="efa6b-157">Aplikacja SignalRChat pokazuje dwa podstawowe zadania rozwoju SignalR.</span><span class="sxs-lookup"><span data-stu-id="efa6b-157">The SignalRChat application demonstrates two basic SignalR development tasks.</span></span> <span data-ttu-id="efa6b-158">Prezentuje sposób Utwórz koncentrator.</span><span class="sxs-lookup"><span data-stu-id="efa6b-158">It shows you how to create a hub.</span></span> <span data-ttu-id="efa6b-159">Serwer używa koncentratora jako obiekt główny koordynacji.</span><span class="sxs-lookup"><span data-stu-id="efa6b-159">The server uses that hub as the main coordination object.</span></span> <span data-ttu-id="efa6b-160">Centrum używa biblioteki jQuery SignalR do wysyłania i odbierania komunikatów.</span><span class="sxs-lookup"><span data-stu-id="efa6b-160">The hub uses the SignalR jQuery library to send and receive messages.</span></span>

### <a name="signalr-hubs-in-the-chathubcs"></a><span data-ttu-id="efa6b-161">Koncentratory SignalR w ChatHub.cs</span><span class="sxs-lookup"><span data-stu-id="efa6b-161">SignalR Hubs in the ChatHub.cs</span></span>

<span data-ttu-id="efa6b-162">W powyższym przykładowym kodzie `ChatHub` klasa pochodzi od `Microsoft.AspNet.SignalR.Hub` klasy.</span><span class="sxs-lookup"><span data-stu-id="efa6b-162">In the code sample above, the `ChatHub` class derives from the `Microsoft.AspNet.SignalR.Hub` class.</span></span> <span data-ttu-id="efa6b-163">Wyprowadzanie z `Hub` klasy jest to wygodny sposób, aby skompilować aplikację SignalR.</span><span class="sxs-lookup"><span data-stu-id="efa6b-163">Deriving from the `Hub` class is a useful way to build a SignalR application.</span></span> <span data-ttu-id="efa6b-164">Można utworzyć metody publiczne na klasie Centrum i następnie użyć tych metod, wywołując je ze skryptów na stronie sieci web.</span><span class="sxs-lookup"><span data-stu-id="efa6b-164">You can create public methods on your hub class and then use those methods by calling them from scripts in a web page.</span></span>

<span data-ttu-id="efa6b-165">W kodzie Rozmowa klienci wywołują `ChatHub.Send` metodę, aby wysyłać nowy komunikat.</span><span class="sxs-lookup"><span data-stu-id="efa6b-165">In the chat code, clients call the `ChatHub.Send` method to send a new message.</span></span> <span data-ttu-id="efa6b-166">Centrum wiadomość jest wysyłana do wszystkich klientów, wywołując `Clients.All.broadcastMessage`.</span><span class="sxs-lookup"><span data-stu-id="efa6b-166">The hub then sends the message to all clients by calling `Clients.All.broadcastMessage`.</span></span>

<span data-ttu-id="efa6b-167">`Send` Metoda pokazuje kilka koncepcji w Centrum:</span><span class="sxs-lookup"><span data-stu-id="efa6b-167">The `Send` method demonstrates several hub concepts:</span></span>

* <span data-ttu-id="efa6b-168">Tak, aby je wywoływać klientów do deklarowania metod publicznych w koncentratorze.</span><span class="sxs-lookup"><span data-stu-id="efa6b-168">Declare public methods on a hub so that clients can call them.</span></span>

* <span data-ttu-id="efa6b-169">Użyj `Microsoft.AspNet.SignalR.Hub.Clients` właściwości dynamicznych do komunikowania się ze wszystkimi klientami podłączone do tego koncentratora.</span><span class="sxs-lookup"><span data-stu-id="efa6b-169">Use the `Microsoft.AspNet.SignalR.Hub.Clients` dynamic property to communicate with all clients connected to this hub.</span></span>

* <span data-ttu-id="efa6b-170">Wywołania funkcji na komputerze klienckim (takich jak `broadcastMessage` funkcji) aby zaktualizować klientów.</span><span class="sxs-lookup"><span data-stu-id="efa6b-170">Call a function on the client (like the `broadcastMessage` function) to update clients.</span></span>

    [!code-csharp[Main](tutorial-getting-started-with-signalr/samples/sample4.cs)]

### <a name="signalr-and-jquery-in-the-indexhtml"></a><span data-ttu-id="efa6b-171">SignalR i jQuery w index.html</span><span class="sxs-lookup"><span data-stu-id="efa6b-171">SignalR and jQuery in the index.html</span></span>

<span data-ttu-id="efa6b-172">*Index.html* strony w przykładzie kodu pokazano, jak użyć biblioteki jQuery SignalR do komunikowania się z Centrum SignalR.</span><span class="sxs-lookup"><span data-stu-id="efa6b-172">The *index.html* page in the code sample shows how to use the SignalR jQuery library to communicate with a SignalR hub.</span></span> <span data-ttu-id="efa6b-173">Kod wykonuje wiele istotnych zadań.</span><span class="sxs-lookup"><span data-stu-id="efa6b-173">The code carries out many important tasks.</span></span> <span data-ttu-id="efa6b-174">Go deklaruje serwer proxy, aby odwoływać się do koncentratora, deklaruje funkcję, serwer można wywołać w celu wypychania zawartości do klientów i rozpoczyna połączenie do wysłania wiadomości do Centrum.</span><span class="sxs-lookup"><span data-stu-id="efa6b-174">It declares a proxy to reference the hub, declares a function that the server can call to push content to clients, and it starts a connection to send messages to the hub.</span></span>

[!code-javascript[Main](tutorial-getting-started-with-signalr/samples/sample5.js)]

> [!NOTE]
> <span data-ttu-id="efa6b-175">W języku JavaScript, musi być camelCase odwołanie do klasy serwera i jej elementów członkowskich.</span><span class="sxs-lookup"><span data-stu-id="efa6b-175">In JavaScript the reference to the server class and its members has to be camelCase.</span></span> <span data-ttu-id="efa6b-176">Odwołania do próbki kodu C# *ChatHub* klasy w języku JavaScript jako `chatHub`.</span><span class="sxs-lookup"><span data-stu-id="efa6b-176">The code sample references the C# *ChatHub* class in JavaScript as `chatHub`.</span></span>

<span data-ttu-id="efa6b-177">W tym bloku kodu utworzysz funkcję wywołania zwrotnego w skrypcie.</span><span class="sxs-lookup"><span data-stu-id="efa6b-177">In this code block, you create a callback function in the script.</span></span>

[!code-html[Main](tutorial-getting-started-with-signalr/samples/sample6.html)]

<span data-ttu-id="efa6b-178">Klasy koncentratora na serwerze wywołuje tę funkcję, aby wypchnąć aktualizacji zawartości dla każdego klienta.</span><span class="sxs-lookup"><span data-stu-id="efa6b-178">The hub class on the server calls this function to push content updates to each client.</span></span> <span data-ttu-id="efa6b-179">Dwa wiersze, że — kodowanie HTML zawartości przed wyświetleniem go są opcjonalne i Pokaż dobrym sposobem, aby uniemożliwić uruchomienie skryptu.</span><span class="sxs-lookup"><span data-stu-id="efa6b-179">The two lines that HTML-encode the content before displaying it are optional and show a good way to prevent script injection.</span></span>

<span data-ttu-id="efa6b-180">Ten kod otwiera połączenie z koncentratorem.</span><span class="sxs-lookup"><span data-stu-id="efa6b-180">This code opens a connection with the hub.</span></span>

[!code-javascript[Main](tutorial-getting-started-with-signalr/samples/sample7.js)]

> [!NOTE]
> <span data-ttu-id="efa6b-181">Takie podejście zapewnia, że kod nawiąże połączenie, przed wykonaniem programu obsługi zdarzeń.</span><span class="sxs-lookup"><span data-stu-id="efa6b-181">This approach ensures that the code establishes a connection before the event handler executes.</span></span>

<span data-ttu-id="efa6b-182">Kod uruchamia połączenie i przekazuje go po funkcji do obsługi zdarzenia click na **wysyłania** przycisk na stronie HTML.</span><span class="sxs-lookup"><span data-stu-id="efa6b-182">The code starts the connection and then passes it a function to handle the click event on the **Send** button in the HTML page.</span></span>

## <a name="get-the-code"></a><span data-ttu-id="efa6b-183">Pobierz kod</span><span class="sxs-lookup"><span data-stu-id="efa6b-183">Get the code</span></span>

[<span data-ttu-id="efa6b-184">Pobierz ukończony projekt</span><span class="sxs-lookup"><span data-stu-id="efa6b-184">Download Completed Project</span></span>](http://code.msdn.microsoft.com/SignalR-Getting-Started-b9d18aa9)

## <a name="additional-resources"></a><span data-ttu-id="efa6b-185">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="efa6b-185">Additional resources</span></span>

<span data-ttu-id="efa6b-186">Aby uzyskać więcej informacji na temat biblioteki SignalR zobacz następujące zasoby:</span><span class="sxs-lookup"><span data-stu-id="efa6b-186">For more about SignalR, see the following resources:</span></span>

* [<span data-ttu-id="efa6b-187">Projekt SignalR</span><span class="sxs-lookup"><span data-stu-id="efa6b-187">SignalR Project</span></span>](http://signalr.net)

* [<span data-ttu-id="efa6b-188">SignalR Github i przykłady</span><span class="sxs-lookup"><span data-stu-id="efa6b-188">SignalR Github and Samples</span></span>](https://github.com/SignalR/SignalR)

* [<span data-ttu-id="efa6b-189">Witryny typu Wiki biblioteki SignalR</span><span class="sxs-lookup"><span data-stu-id="efa6b-189">SignalR Wiki</span></span>](https://github.com/SignalR/SignalR/wiki)

## <a name="next-steps"></a><span data-ttu-id="efa6b-190">Następne kroki</span><span class="sxs-lookup"><span data-stu-id="efa6b-190">Next steps</span></span>

<span data-ttu-id="efa6b-191">W tym samouczku możesz:</span><span class="sxs-lookup"><span data-stu-id="efa6b-191">In this tutorial you:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="efa6b-192">Konfigurowanie projektu</span><span class="sxs-lookup"><span data-stu-id="efa6b-192">Set up the project</span></span>
> * <span data-ttu-id="efa6b-193">Uruchomiono próbki</span><span class="sxs-lookup"><span data-stu-id="efa6b-193">Ran the sample</span></span>
> * <span data-ttu-id="efa6b-194">Zbadanie kodu</span><span class="sxs-lookup"><span data-stu-id="efa6b-194">Examined the code</span></span>

<span data-ttu-id="efa6b-195">Przejdź do następnego artykułu, aby dowiedzieć się, jak używać biblioteki SignalR i MVC 5.</span><span class="sxs-lookup"><span data-stu-id="efa6b-195">Advance to the next article to learn how to use SignalR and MVC 5.</span></span>
> [!div class="nextstepaction"]
> [<span data-ttu-id="efa6b-196">SignalR 2 i MVC 5</span><span class="sxs-lookup"><span data-stu-id="efa6b-196">SignalR 2 and MVC 5</span></span>](tutorial-getting-started-with-signalr-and-mvc.md)