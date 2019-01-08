---
uid: signalr/overview/getting-started/tutorial-getting-started-with-signalr-and-mvc
title: 'Samouczek: Rozmowy w czasie rzeczywistym z SignalR 2 i MVC 5 | Dokumentacja firmy Microsoft'
author: pfletcher
description: W tym samouczku pokazano, jak używać signalr2 na platformie ASP.NET do tworzenia aplikacji rozmowy w czasie rzeczywistym. Biblioteki SignalR można dodać do aplikacji MVC 5.
ms.author: riande
ms.date: 01/02/2019
ms.assetid: 80bfe5fb-bdfc-41fe-ac43-2132e5d69fac
msc.legacyurl: /signalr/overview/getting-started/tutorial-getting-started-with-signalr-and-mvc
msc.type: authoredcontent
ms.topic: tutorial
ms.openlocfilehash: eb4b7e1403f4070d65702b756bf98c5294c7fb17
ms.sourcegitcommit: 97d7a00bd39c83a8f6bccb9daa44130a509f75ce
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/08/2019
ms.locfileid: "54098607"
---
# <a name="tutorial-real-time-chat-with-signalr-2-and-mvc-5"></a><span data-ttu-id="41ea2-104">Samouczek: Rozmowy w czasie rzeczywistym z SignalR 2 i MVC 5</span><span class="sxs-lookup"><span data-stu-id="41ea2-104">Tutorial: Real-time chat with SignalR 2 and MVC 5</span></span>

<span data-ttu-id="41ea2-105">W tym samouczku pokazano, jak używać signalr2 na platformie ASP.NET do tworzenia aplikacji rozmowy w czasie rzeczywistym.</span><span class="sxs-lookup"><span data-stu-id="41ea2-105">This tutorial shows how to use ASP.NET SignalR 2 to create a real-time chat application.</span></span> <span data-ttu-id="41ea2-106">Dodawanie SignalR do aplikacji MVC 5 i tworzenie widoku czatu do wysyłania i wyświetla komunikaty.</span><span class="sxs-lookup"><span data-stu-id="41ea2-106">You add SignalR to an MVC 5 application and create a chat view to send and display messages.</span></span>

<span data-ttu-id="41ea2-107">W ramach tego samouczka możesz:</span><span class="sxs-lookup"><span data-stu-id="41ea2-107">In this tutorial, you:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="41ea2-108">Konfigurowanie projektu</span><span class="sxs-lookup"><span data-stu-id="41ea2-108">Set up the project</span></span>
> * <span data-ttu-id="41ea2-109">Uruchamianie aplikacji przykładowej</span><span class="sxs-lookup"><span data-stu-id="41ea2-109">Run the sample</span></span>
> * <span data-ttu-id="41ea2-110">Poszukaj w kodzie</span><span class="sxs-lookup"><span data-stu-id="41ea2-110">Examine the code</span></span>

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

## <a name="prerequisites"></a><span data-ttu-id="41ea2-111">Wymagania wstępne</span><span class="sxs-lookup"><span data-stu-id="41ea2-111">Prerequisites</span></span>

* <span data-ttu-id="41ea2-112">[Program Visual Studio 2017](https://visualstudio.microsoft.com/downloads/) z **ASP.NET i tworzenie aplikacji internetowych** obciążenia.</span><span class="sxs-lookup"><span data-stu-id="41ea2-112">[Visual Studio 2017](https://visualstudio.microsoft.com/downloads/) with the **ASP.NET and web development** workload.</span></span>

## <a name="set-up-the-project"></a><span data-ttu-id="41ea2-113">Konfigurowanie projektu</span><span class="sxs-lookup"><span data-stu-id="41ea2-113">Set up the Project</span></span>

<span data-ttu-id="41ea2-114">W tej sekcji pokazano, jak używać programu Visual Studio 2017 i signalr2 do tworzenia pustej aplikacji ASP.NET MVC 5, dodawanie biblioteki SignalR i tworzenie aplikacji czatu.</span><span class="sxs-lookup"><span data-stu-id="41ea2-114">This section shows how to use Visual Studio 2017 and SignalR 2 to create an empty ASP.NET MVC 5 application, add the SignalR library, and create the chat application.</span></span>

1. <span data-ttu-id="41ea2-115">W programie Visual Studio tworzenie aplikacji C# ASP.NET przeznaczonych .NET Framework 4.5, nadaj jej nazwę SignalRChat, a następnie kliknij przycisk OK.</span><span class="sxs-lookup"><span data-stu-id="41ea2-115">In Visual Studio, create a C# ASP.NET application that targets .NET Framework 4.5, name it SignalRChat, and click OK.</span></span>

    ![Tworzenie sieci web](tutorial-getting-started-with-signalr-and-mvc/_static/image1.png)

1. <span data-ttu-id="41ea2-117">W **nowej aplikacji sieci Web ASP.NET - SignalRMvcChat**, wybierz opcję **MVC** , a następnie wybierz **Zmień uwierzytelnianie**.</span><span class="sxs-lookup"><span data-stu-id="41ea2-117">In **New ASP.NET Web Application - SignalRMvcChat**, select **MVC** and then select **Change Authentication**.</span></span>

1. <span data-ttu-id="41ea2-118">W **Zmień uwierzytelnianie**, wybierz opcję **bez uwierzytelniania** i kliknij przycisk **OK**.</span><span class="sxs-lookup"><span data-stu-id="41ea2-118">In **Change Authentication**, select **No Authentication** and click **OK**.</span></span>

    ![Wybierz pozycję bez uwierzytelniania](tutorial-getting-started-with-signalr-and-mvc/_static/image2.png)

1. <span data-ttu-id="41ea2-120">W **nowej aplikacji sieci Web ASP.NET - SignalRMvcChat**, wybierz opcję **OK**.</span><span class="sxs-lookup"><span data-stu-id="41ea2-120">In **New ASP.NET Web Application - SignalRMvcChat**, select **OK**.</span></span>

1. <span data-ttu-id="41ea2-121">W **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy projekt i wybierz **Dodaj** > **nowy element**.</span><span class="sxs-lookup"><span data-stu-id="41ea2-121">In **Solution Explorer**, right-click the project and select **Add** > **New Item**.</span></span>

1. <span data-ttu-id="41ea2-122">W **Dodaj nowy element - SignalRChat**, wybierz opcję **zainstalowane** > **Visual C#**   >  **Web**  >  **SignalR** , a następnie wybierz **klasa Centrum SignalR (v2)**.</span><span class="sxs-lookup"><span data-stu-id="41ea2-122">In **Add New Item - SignalRChat**, select **Installed** > **Visual C#** > **Web** > **SignalR**  and then select **SignalR Hub Class (v2)**.</span></span>

1. <span data-ttu-id="41ea2-123">Nazwa klasy *ChatHub* i dodaj go do projektu.</span><span class="sxs-lookup"><span data-stu-id="41ea2-123">Name the class *ChatHub* and add it to the project.</span></span>

    <span data-ttu-id="41ea2-124">Spowoduje to utworzenie *ChatHub.cs* plik i dodaje zestaw plików skryptów i odwołania do zestawów, obsługujące bibliotekę SignalR do projektu.</span><span class="sxs-lookup"><span data-stu-id="41ea2-124">This step creates the *ChatHub.cs* class file and adds a set of script files and assembly references that support SignalR to the project.</span></span>

1. <span data-ttu-id="41ea2-125">Zastąp kod w nowej *ChatHub.cs* plik klasy przy użyciu tego kodu:</span><span class="sxs-lookup"><span data-stu-id="41ea2-125">Replace the code in the new *ChatHub.cs* class file with this code:</span></span>

    [!code-csharp[Main](tutorial-getting-started-with-signalr-and-mvc/samples/sample1.cs)]

1. <span data-ttu-id="41ea2-126">W **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy projekt i wybierz **Dodaj** > **klasy**.</span><span class="sxs-lookup"><span data-stu-id="41ea2-126">In **Solution Explorer**, right-click the project and select **Add** > **Class**.</span></span>

1. <span data-ttu-id="41ea2-127">Nadaj nowej klasie *uruchamiania* i dodaj go do projektu.</span><span class="sxs-lookup"><span data-stu-id="41ea2-127">Name the new class *Startup* and add it to the project.</span></span>

1. <span data-ttu-id="41ea2-128">Zastąp kod w *Startup.cs* plik klasy przy użyciu tego kodu:</span><span class="sxs-lookup"><span data-stu-id="41ea2-128">Replace the code in the *Startup.cs* class file with this code:</span></span>

    [!code-csharp[Main](tutorial-getting-started-with-signalr-and-mvc/samples/sample2.cs)]

1. <span data-ttu-id="41ea2-129">W **Eksploratora rozwiązań**, wybierz opcję **kontrolerów** > **HomeController.cs**.</span><span class="sxs-lookup"><span data-stu-id="41ea2-129">In **Solution Explorer**, select **Controllers** > **HomeController.cs**.</span></span>

1. <span data-ttu-id="41ea2-130">Dodaj tę metodę w celu *HomeController.cs*.</span><span class="sxs-lookup"><span data-stu-id="41ea2-130">Add this method to the *HomeController.cs*.</span></span>

    [!code-csharp[Main](tutorial-getting-started-with-signalr-and-mvc/samples/sample3.cs)]

    <span data-ttu-id="41ea2-131">Ta metoda zwraca **Chat** widok, który zostanie utworzony w późniejszym kroku.</span><span class="sxs-lookup"><span data-stu-id="41ea2-131">This method returns the **Chat** view that you create in a later step.</span></span>

1. <span data-ttu-id="41ea2-132">W **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy **widoków** > **Home**i wybierz **Dodaj**  >    **Widok**.</span><span class="sxs-lookup"><span data-stu-id="41ea2-132">In **Solution Explorer**, right-click **Views** > **Home**, and select **Add** >  **View**.</span></span>

1. <span data-ttu-id="41ea2-133">W **Dodaj widok**, Nazwij nowy widok **Chat** i wybierz **Dodaj**.</span><span class="sxs-lookup"><span data-stu-id="41ea2-133">In **Add View**, name the new view **Chat** and select **Add**.</span></span>

1. <span data-ttu-id="41ea2-134">Zastąp zawartość **Chat.cshtml** przy użyciu tego kodu:</span><span class="sxs-lookup"><span data-stu-id="41ea2-134">Replace the contents of **Chat.cshtml** with this code:</span></span>

    [!code-cshtml[Main](tutorial-getting-started-with-signalr-and-mvc/samples/sample4.cshtml)]

1. <span data-ttu-id="41ea2-135">W **Eksploratora rozwiązań**, rozwiń węzeł **skrypty**.</span><span class="sxs-lookup"><span data-stu-id="41ea2-135">In **Solution Explorer**, expand **Scripts**.</span></span>

    <span data-ttu-id="41ea2-136">Biblioteki skryptów, jQuery i SignalR są widoczne w projekcie.</span><span class="sxs-lookup"><span data-stu-id="41ea2-136">Script libraries for jQuery and SignalR are visible in the project.</span></span>

    > [!IMPORTANT]
    > <span data-ttu-id="41ea2-137">Menedżer pakietów może zainstalowano późniejszą wersję skrypty SignalR.</span><span class="sxs-lookup"><span data-stu-id="41ea2-137">The package manager may have installed a later version of the SignalR scripts.</span></span>

1. <span data-ttu-id="41ea2-138">Upewnij się, że odwołania do skryptów w blok kodu odpowiadają wersji plików skrypt w projekcie.</span><span class="sxs-lookup"><span data-stu-id="41ea2-138">Check that the script references in the code block correspond to the versions of the script files in the project.</span></span>

    <span data-ttu-id="41ea2-139">Odwołania do skryptu z oryginalnego bloku kodu:</span><span class="sxs-lookup"><span data-stu-id="41ea2-139">Script references from the original code block:</span></span>

    ```cshtml
    <!--Script references. -->
    <!--The jQuery library is required and is referenced by default in _Layout.cshtml. -->
    <!--Reference the SignalR library. -->
    <script src="~/Scripts/jquery.signalR-2.1.0.min.js"></script>
    ```

1. <span data-ttu-id="41ea2-140">Jeśli nie są zgodne, zaktualizuj *.cshtml* pliku.</span><span class="sxs-lookup"><span data-stu-id="41ea2-140">If they don't match, update the *.cshtml* file.</span></span>

1. <span data-ttu-id="41ea2-141">Na pasku menu wybierz **pliku** > **Zapisz wszystko**.</span><span class="sxs-lookup"><span data-stu-id="41ea2-141">From the menu bar, select **File** > **Save All**.</span></span>

## <a name="run-the-sample"></a><span data-ttu-id="41ea2-142">Uruchamianie aplikacji przykładowej</span><span class="sxs-lookup"><span data-stu-id="41ea2-142">Run the Sample</span></span>

1. <span data-ttu-id="41ea2-143">Na pasku narzędzi, Włącz **debugowanie skryptu** a następnie wybierz przycisk Odtwórz, aby uruchomić przykład w trybie debugowania.</span><span class="sxs-lookup"><span data-stu-id="41ea2-143">In the toolbar, turn on **Script Debugging** and then select the play button to run the sample in Debug mode.</span></span>

    ![Wprowadź nazwę użytkownika](tutorial-getting-started-with-signalr-and-mvc/_static/image3.png)

1. <span data-ttu-id="41ea2-145">Po otwarciu przeglądarki, wprowadź nazwę swojej rozmowy tożsamości.</span><span class="sxs-lookup"><span data-stu-id="41ea2-145">When the browser opens, enter a name for your chat identity.</span></span>

1. <span data-ttu-id="41ea2-146">Skopiuj adres URL w przeglądarce otwórz dwóch innych przeglądarek i wklejać adresy URL paski adresu.</span><span class="sxs-lookup"><span data-stu-id="41ea2-146">Copy the URL from the browser, open two other browsers, and paste the URLs into the address bars.</span></span>

1. <span data-ttu-id="41ea2-147">W każdej przeglądarce wprowadź unikatową nazwę.</span><span class="sxs-lookup"><span data-stu-id="41ea2-147">In each browser, enter a unique name.</span></span>

1. <span data-ttu-id="41ea2-148">Teraz Dodaj komentarz i wybierz pozycję **wysyłania**.</span><span class="sxs-lookup"><span data-stu-id="41ea2-148">Now, add a comment and select **Send**.</span></span> <span data-ttu-id="41ea2-149">Powtórz, w innych przeglądarkach.</span><span class="sxs-lookup"><span data-stu-id="41ea2-149">Repeat that in the other browsers.</span></span> <span data-ttu-id="41ea2-150">Komentarze są wyświetlane w czasie rzeczywistym.</span><span class="sxs-lookup"><span data-stu-id="41ea2-150">The comments appear in real time.</span></span>

    > [!NOTE]
    > <span data-ttu-id="41ea2-151">Tej aplikacji rozmów prostego nie utrzymuje kontekst dyskusji na serwerze.</span><span class="sxs-lookup"><span data-stu-id="41ea2-151">This simple chat application does not maintain the discussion context on the server.</span></span> <span data-ttu-id="41ea2-152">Centrum emituje komentarzy do wszystkich bieżących użytkowników.</span><span class="sxs-lookup"><span data-stu-id="41ea2-152">The hub broadcasts comments to all current users.</span></span> <span data-ttu-id="41ea2-153">Użytkownicy, którzy dołączają do czatu później zostanie wyświetlony komunikat o dodane od czasu dołączenia.</span><span class="sxs-lookup"><span data-stu-id="41ea2-153">Users who join the chat later will see messages added from the time they join.</span></span>

    <span data-ttu-id="41ea2-154">Zobacz, jak działa aplikacji rozmów w trzech różnych przeglądarkach.</span><span class="sxs-lookup"><span data-stu-id="41ea2-154">See how the chat application runs in three different browsers.</span></span> <span data-ttu-id="41ea2-155">Tom Anand i Susan wysyłania wiadomości, wszystkie przeglądarki aktualizacji w czasie rzeczywistym:</span><span class="sxs-lookup"><span data-stu-id="41ea2-155">When Tom, Anand, and Susan send messages, all browsers update in real time:</span></span>

    ![Wszystkie trzy przeglądarki wyświetlenia tych samych Historia](tutorial-getting-started-with-signalr-and-mvc/_static/image4.png)

1. <span data-ttu-id="41ea2-157">W **Eksploratora rozwiązań**, zbadaj **dokumenty skryptów** węzła dla działającej aplikacji.</span><span class="sxs-lookup"><span data-stu-id="41ea2-157">In **Solution Explorer**, inspect the **Script Documents** node for the running application.</span></span> <span data-ttu-id="41ea2-158">Brak pliku skryptu o nazwie *koncentratory* generujący biblioteki SignalR w czasie wykonywania.</span><span class="sxs-lookup"><span data-stu-id="41ea2-158">There's a script file named *hubs* that the SignalR library generates at runtime.</span></span> <span data-ttu-id="41ea2-159">Ten plik zarządza komunikacji między jQuery skryptu i kod po stronie serwera.</span><span class="sxs-lookup"><span data-stu-id="41ea2-159">This file manages the communication between jQuery script and server-side code.</span></span>

    ![wygenerowany automatycznie koncentratory skryptu w węźle dokumentów skryptu](tutorial-getting-started-with-signalr-and-mvc/_static/image5.png)

## <a name="examine-the-code"></a><span data-ttu-id="41ea2-161">Poszukaj w kodzie</span><span class="sxs-lookup"><span data-stu-id="41ea2-161">Examine the Code</span></span>

<span data-ttu-id="41ea2-162">Aplikacji rozmów SignalR pokazuje dwa podstawowe zadania rozwoju SignalR.</span><span class="sxs-lookup"><span data-stu-id="41ea2-162">The SignalR chat application demonstrates two basic SignalR development tasks.</span></span> <span data-ttu-id="41ea2-163">Prezentuje sposób Utwórz koncentrator.</span><span class="sxs-lookup"><span data-stu-id="41ea2-163">It shows you how to create a hub.</span></span> <span data-ttu-id="41ea2-164">Serwer używa koncentratora jako obiekt główny koordynacji.</span><span class="sxs-lookup"><span data-stu-id="41ea2-164">The server uses that hub as the main coordination object.</span></span> <span data-ttu-id="41ea2-165">Centrum używa biblioteki jQuery SignalR do wysyłania i odbierania komunikatów.</span><span class="sxs-lookup"><span data-stu-id="41ea2-165">The hub uses the SignalR jQuery library to send and receive messages.</span></span>

### <a name="signalr-hubs-in-the-chathubcs"></a><span data-ttu-id="41ea2-166">Koncentratory SignalR w ChatHub.cs</span><span class="sxs-lookup"><span data-stu-id="41ea2-166">SignalR Hubs in the ChatHub.cs</span></span>

<span data-ttu-id="41ea2-167">W przykładowym kodzie `ChatHub` klasa pochodzi od `Microsoft.AspNet.SignalR.Hub` klasy.</span><span class="sxs-lookup"><span data-stu-id="41ea2-167">In the code sample, the `ChatHub` class derives from the `Microsoft.AspNet.SignalR.Hub` class.</span></span> <span data-ttu-id="41ea2-168">Wyprowadzanie z `Hub` klasy jest to wygodny sposób, aby skompilować aplikację SignalR.</span><span class="sxs-lookup"><span data-stu-id="41ea2-168">Deriving from the `Hub` class is a useful way to build a SignalR application.</span></span> <span data-ttu-id="41ea2-169">Można utworzyć metody publiczne na klasy koncentratora, a następnie uzyskać dostęp do tych metod, wywołując je ze skryptów na stronie sieci web.</span><span class="sxs-lookup"><span data-stu-id="41ea2-169">You can create public methods on your hub class and then access those methods by calling them from scripts in a web page.</span></span>

<span data-ttu-id="41ea2-170">W kodzie Rozmowa klienci wywołują `ChatHub.Send` metodę, aby wysyłać nowy komunikat.</span><span class="sxs-lookup"><span data-stu-id="41ea2-170">In the chat code, clients call the `ChatHub.Send` method to send a new message.</span></span> <span data-ttu-id="41ea2-171">Centrum z kolei wysyła wiadomość do wszystkich klientów, wywołując `Clients.All.addNewMessageToPage`.</span><span class="sxs-lookup"><span data-stu-id="41ea2-171">The hub in turn sends the message to all clients by calling `Clients.All.addNewMessageToPage`.</span></span>

<span data-ttu-id="41ea2-172">`Send` Metoda pokazuje kilka koncepcji w Centrum:</span><span class="sxs-lookup"><span data-stu-id="41ea2-172">The `Send` method demonstrates several hub concepts:</span></span>

* <span data-ttu-id="41ea2-173">Tak, aby je wywoływać klientów do deklarowania metod publicznych w koncentratorze.</span><span class="sxs-lookup"><span data-stu-id="41ea2-173">Declare public methods on a hub so that clients can call them.</span></span>

* <span data-ttu-id="41ea2-174">Użyj `Microsoft.AspNet.SignalR.Hub.Clients` właściwości dynamicznych do komunikowania się ze wszystkimi klientami podłączone do tego koncentratora.</span><span class="sxs-lookup"><span data-stu-id="41ea2-174">Use the `Microsoft.AspNet.SignalR.Hub.Clients` dynamic property to communicate with all clients connected to this hub.</span></span>

* <span data-ttu-id="41ea2-175">Wywołania funkcji na komputerze klienckim (takich jak `addNewMessageToPage` funkcji) aby zaktualizować klientów.</span><span class="sxs-lookup"><span data-stu-id="41ea2-175">Call a function on the client (like the `addNewMessageToPage` function) to update clients.</span></span>

    [!code-csharp[Main](tutorial-getting-started-with-signalr-and-mvc/samples/sample5.cs)]

### <a name="signalr-and-jquery-chatcshtml"></a><span data-ttu-id="41ea2-176">SignalR i jQuery Chat.cshtml</span><span class="sxs-lookup"><span data-stu-id="41ea2-176">SignalR and jQuery Chat.cshtml</span></span>

<span data-ttu-id="41ea2-177">*Chat.cshtml* plik widoku w przykładzie kodu pokazano, jak użyć biblioteki jQuery SignalR do komunikowania się z Centrum SignalR.</span><span class="sxs-lookup"><span data-stu-id="41ea2-177">The *Chat.cshtml* view file in the code sample shows how to use the SignalR jQuery library to communicate with a SignalR hub.</span></span>  <span data-ttu-id="41ea2-178">Kod wykonuje wiele istotnych zadań.</span><span class="sxs-lookup"><span data-stu-id="41ea2-178">The code carries out many important tasks.</span></span> <span data-ttu-id="41ea2-179">To tworzy odwołanie do serwera proxy automatycznie wygenerowana dla koncentratora, deklaruje funkcję, serwer można wywołać w celu wypychania zawartości do klientów i rozpoczyna połączenie do wysłania wiadomości do Centrum.</span><span class="sxs-lookup"><span data-stu-id="41ea2-179">It creates a reference to the autogenerated proxy for the hub, declares a function that the server can call to push content to clients, and it starts a connection to send messages to the hub.</span></span>

[!code-javascript[Main](tutorial-getting-started-with-signalr-and-mvc/samples/sample6.js)]

> [!NOTE]
> <span data-ttu-id="41ea2-180">Odwołanie do klasy serwera i jej elementów członkowskich w JavaScript, znajduje się w camelCase.</span><span class="sxs-lookup"><span data-stu-id="41ea2-180">In JavaScript, the reference to the server class and its members is in camelCase.</span></span> <span data-ttu-id="41ea2-181">Odwołania do próbki kodu C# `ChatHub` klasy w języku JavaScript jako `chatHub`.</span><span class="sxs-lookup"><span data-stu-id="41ea2-181">The code sample references the C# `ChatHub` class in JavaScript as `chatHub`.</span></span>

<span data-ttu-id="41ea2-182">W tym bloku kodu utworzysz funkcję wywołania zwrotnego w skrypcie.</span><span class="sxs-lookup"><span data-stu-id="41ea2-182">In this code block, you create a callback function in the script.</span></span>

[!code-html[Main](tutorial-getting-started-with-signalr-and-mvc/samples/sample7.html)]

<span data-ttu-id="41ea2-183">Klasy koncentratora na serwerze wywołuje tę funkcję, aby wypchnąć aktualizacji zawartości dla każdego klienta.</span><span class="sxs-lookup"><span data-stu-id="41ea2-183">The hub class on the server calls this function to push content updates to each client.</span></span> <span data-ttu-id="41ea2-184">Opcjonalne wywołanie `htmlEncode` funkcja przedstawiono sposób HTML zakodować zawartość komunikatu przed wyświetleniem go na stronie.</span><span class="sxs-lookup"><span data-stu-id="41ea2-184">The optional call to the `htmlEncode` function shows a way to HTML encode the message content before displaying it in the page.</span></span> <span data-ttu-id="41ea2-185">Jest to sposób, aby uniemożliwić uruchomienie skryptu.</span><span class="sxs-lookup"><span data-stu-id="41ea2-185">It's a way to prevent script injection.</span></span>

<span data-ttu-id="41ea2-186">Ten kod otwiera połączenie z koncentratorem.</span><span class="sxs-lookup"><span data-stu-id="41ea2-186">This code opens a connection with the hub.</span></span>

[!code-javascript[Main](tutorial-getting-started-with-signalr-and-mvc/samples/sample8.js)]

> [!NOTE]
> <span data-ttu-id="41ea2-187">Takie podejście zapewnia ustanowić połączenie przed wykonaniem programu obsługi zdarzeń.</span><span class="sxs-lookup"><span data-stu-id="41ea2-187">This approach ensures that you establish a connection before the event handler executes.</span></span>

<span data-ttu-id="41ea2-188">Kod uruchamia połączenie i przekazuje go po funkcji do obsługi zdarzenia click na **wysyłania** przycisk na stronie rozmowy.</span><span class="sxs-lookup"><span data-stu-id="41ea2-188">The code starts the connection and then passes it a function to handle the click event on the **Send** button in the Chat page.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="41ea2-189">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="41ea2-189">Additional resources</span></span>

<span data-ttu-id="41ea2-190">Aby uzyskać więcej informacji na temat biblioteki SignalR zobacz następujące zasoby:</span><span class="sxs-lookup"><span data-stu-id="41ea2-190">For more about SignalR, see the following resources:</span></span>

* [<span data-ttu-id="41ea2-191">Projekt SignalR</span><span class="sxs-lookup"><span data-stu-id="41ea2-191">SignalR Project</span></span>](http://signalr.net)

* [<span data-ttu-id="41ea2-192">SignalR GitHub i przykłady</span><span class="sxs-lookup"><span data-stu-id="41ea2-192">SignalR GitHub and Samples</span></span>](https://github.com/SignalR/SignalR)

* [<span data-ttu-id="41ea2-193">Witryny typu Wiki biblioteki SignalR</span><span class="sxs-lookup"><span data-stu-id="41ea2-193">SignalR Wiki</span></span>](https://github.com/SignalR/SignalR/wiki)

## <a name="next-steps"></a><span data-ttu-id="41ea2-194">Następne kroki</span><span class="sxs-lookup"><span data-stu-id="41ea2-194">Next steps</span></span>

<span data-ttu-id="41ea2-195">W ramach tego samouczka możesz:</span><span class="sxs-lookup"><span data-stu-id="41ea2-195">In this tutorial, you:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="41ea2-196">Konfigurowanie projektu</span><span class="sxs-lookup"><span data-stu-id="41ea2-196">Set up the project</span></span>
> * <span data-ttu-id="41ea2-197">Uruchomiono próbki</span><span class="sxs-lookup"><span data-stu-id="41ea2-197">Ran the sample</span></span>
> * <span data-ttu-id="41ea2-198">Zbadanie kodu</span><span class="sxs-lookup"><span data-stu-id="41ea2-198">Examined the code</span></span>

<span data-ttu-id="41ea2-199">Przejdź do następnego artykułu, aby dowiedzieć się, jak utworzyć aplikację sieci web korzystającą z signalr2 na platformie ASP.NET w celu zapewnienia funkcji obsługi wiadomości o wysokiej częstotliwości.</span><span class="sxs-lookup"><span data-stu-id="41ea2-199">Advance to the next article to learn how to create a web application that uses ASP.NET SignalR 2 to provide high-frequency messaging functionality.</span></span>
> [!div class="nextstepaction"]
> [<span data-ttu-id="41ea2-200">Aplikacja sieci Web z obsługą komunikatów o wysokiej częstotliwości</span><span class="sxs-lookup"><span data-stu-id="41ea2-200">Web app with high-frequency messaging</span></span>](tutorial-high-frequency-realtime-with-signalr.md)