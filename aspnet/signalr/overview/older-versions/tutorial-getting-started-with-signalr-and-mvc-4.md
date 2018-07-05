---
uid: signalr/overview/older-versions/tutorial-getting-started-with-signalr-and-mvc-4
title: 'Samouczek: Wprowadzenie do SignalR 1.x i MVC 4 | Dokumentacja firmy Microsoft'
author: pfletcher
description: Użyj biblioteki SignalR platformy ASP.NET i ASP.NET MVC 4, aby utworzyć aplikację do rozmów w czasie rzeczywistym.
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/29/2013
ms.topic: article
ms.assetid: eeef9f73-6de3-49f9-b50b-9af22108f2ce
ms.technology: dotnet-signalr
msc.legacyurl: /signalr/overview/older-versions/tutorial-getting-started-with-signalr-and-mvc-4
msc.type: authoredcontent
ms.openlocfilehash: cced40f65700eff073aa4cff4560a137a50bbc5b
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/03/2018
ms.locfileid: "37399967"
---
<a name="tutorial-getting-started-with-signalr-1x-and-mvc-4"></a><span data-ttu-id="c13d1-103">Samouczek: Wprowadzenie do SignalR 1.x i MVC 4</span><span class="sxs-lookup"><span data-stu-id="c13d1-103">Tutorial: Getting Started with SignalR 1.x and MVC 4</span></span>
====================
<span data-ttu-id="c13d1-104">przez [Patrick Fletcher](https://github.com/pfletcher), [Tim Teebken](https://github.com/timlt)</span><span class="sxs-lookup"><span data-stu-id="c13d1-104">by [Patrick Fletcher](https://github.com/pfletcher), [Tim Teebken](https://github.com/timlt)</span></span>

> <span data-ttu-id="c13d1-105">W tym samouczku pokazano, jak używać biblioteki SignalR platformy ASP.NET do tworzenia aplikacji rozmowy w czasie rzeczywistym.</span><span class="sxs-lookup"><span data-stu-id="c13d1-105">This tutorial shows how to use ASP.NET SignalR to create a real-time chat application.</span></span> <span data-ttu-id="c13d1-106">Będzie dodać SignalR do aplikacji MVC 4 i utworzyć widok czatu do wysyłania i wyświetla komunikaty.</span><span class="sxs-lookup"><span data-stu-id="c13d1-106">You will add SignalR to an MVC 4 application and create a chat view to send and display messages.</span></span>


## <a name="overview"></a><span data-ttu-id="c13d1-107">Omówienie</span><span class="sxs-lookup"><span data-stu-id="c13d1-107">Overview</span></span>

<span data-ttu-id="c13d1-108">Ten samouczek zawiera wprowadzenie do tworzenia aplikacji sieci web w czasie rzeczywistym przy użyciu biblioteki SignalR platformy ASP.NET i ASP.NET MVC 4.</span><span class="sxs-lookup"><span data-stu-id="c13d1-108">This tutorial introduces you to real-time web application development with ASP.NET SignalR and ASP.NET MVC 4.</span></span> <span data-ttu-id="c13d1-109">W tym samouczku użyto tego samego kodu aplikacji rozmów jako [samouczka Wprowadzenie do SignalR](tutorial-getting-started-with-signalr.md), ale pokazuje, jak dodać go do aplikacji MVC 4, na podstawie szablonu Internet.</span><span class="sxs-lookup"><span data-stu-id="c13d1-109">The tutorial uses the same chat application code as the [SignalR Getting Started tutorial](tutorial-getting-started-with-signalr.md), but shows how to add it to an MVC 4 application based on the Internet template.</span></span>

<span data-ttu-id="c13d1-110">W tym temacie dowiesz się, następujące zadania deweloperskie SignalR:</span><span class="sxs-lookup"><span data-stu-id="c13d1-110">In this topic you will learn the following SignalR development tasks:</span></span>

- <span data-ttu-id="c13d1-111">Dodawanie biblioteki SignalR do aplikacji MVC 4.</span><span class="sxs-lookup"><span data-stu-id="c13d1-111">Adding the SignalR library to an MVC 4 application.</span></span>
- <span data-ttu-id="c13d1-112">Tworzenie klasy koncentratora, aby wypchnąć zawartości do klientów.</span><span class="sxs-lookup"><span data-stu-id="c13d1-112">Creating a hub class to push content to clients.</span></span>
- <span data-ttu-id="c13d1-113">Przy użyciu biblioteki jQuery SignalR na stronie sieci web umożliwiają przesyłanie komunikatów oraz wyświetlania aktualizacji z koncentratora.</span><span class="sxs-lookup"><span data-stu-id="c13d1-113">Using the SignalR jQuery library in a web page to send messages and display updates from the hub.</span></span>

<span data-ttu-id="c13d1-114">Poniższy zrzut ekranu przedstawia zakończonych rozmów uruchomieniu aplikacji w przeglądarce.</span><span class="sxs-lookup"><span data-stu-id="c13d1-114">The following screen shot shows the completed chat application running in a browser.</span></span>

![Wystąpienia rozmowy](tutorial-getting-started-with-signalr-and-mvc-4/_static/image2.png)

<span data-ttu-id="c13d1-116">Sekcje:</span><span class="sxs-lookup"><span data-stu-id="c13d1-116">Sections:</span></span>

- [<span data-ttu-id="c13d1-117">Konfigurowanie projektu</span><span class="sxs-lookup"><span data-stu-id="c13d1-117">Set up the Project</span></span>](#setup)
- [<span data-ttu-id="c13d1-118">Uruchamianie aplikacji przykładowej</span><span class="sxs-lookup"><span data-stu-id="c13d1-118">Run the Sample</span></span>](#run)
- [<span data-ttu-id="c13d1-119">Poszukaj w kodzie</span><span class="sxs-lookup"><span data-stu-id="c13d1-119">Examine the Code</span></span>](#code)
- [<span data-ttu-id="c13d1-120">Następne kroki</span><span class="sxs-lookup"><span data-stu-id="c13d1-120">Next Steps</span></span>](#next)

<a id="setup"></a>

## <a name="set-up-the-project"></a><span data-ttu-id="c13d1-121">Konfigurowanie projektu</span><span class="sxs-lookup"><span data-stu-id="c13d1-121">Set up the Project</span></span>

<span data-ttu-id="c13d1-122">Wymagania wstępne:</span><span class="sxs-lookup"><span data-stu-id="c13d1-122">Prerequisites:</span></span>

- <span data-ttu-id="c13d1-123">Visual Studio 2010 SP1, program Visual Studio 2012 lub Visual Studio 2012 Express.</span><span class="sxs-lookup"><span data-stu-id="c13d1-123">Visual Studio 2010 SP1, Visual Studio 2012, or Visual Studio 2012 Express.</span></span> <span data-ttu-id="c13d1-124">Jeśli nie masz programu Visual Studio, zobacz [ASP.NET pliki do pobrania](https://www.asp.net/downloads) można pobrać bezpłatny program Visual Studio 2012 Express narzędzia programistyczne.</span><span class="sxs-lookup"><span data-stu-id="c13d1-124">If you do not have Visual Studio, see [ASP.NET Downloads](https://www.asp.net/downloads) to get the free Visual Studio 2012 Express Development Tool.</span></span>
- <span data-ttu-id="c13d1-125">Dla programu Visual Studio 2010, należy zainstalować [platformy ASP.NET MVC 4](https://www.microsoft.com/download/details.aspx?id=30683).</span><span class="sxs-lookup"><span data-stu-id="c13d1-125">For Visual Studio 2010, install [ASP.NET MVC 4](https://www.microsoft.com/download/details.aspx?id=30683).</span></span>

<span data-ttu-id="c13d1-126">W tej sekcji przedstawiono sposób tworzenia aplikacji ASP.NET MVC 4, dodawanie biblioteki SignalR i tworzenie aplikacji czatu.</span><span class="sxs-lookup"><span data-stu-id="c13d1-126">This section shows how to create an ASP.NET MVC 4 application, add the SignalR library, and create the chat application.</span></span>

1. 1. <span data-ttu-id="c13d1-127">W programie Visual Studio tworzenie aplikacji ASP.NET MVC 4, nadaj jej nazwę SignalRChat, a następnie kliknij przycisk OK.</span><span class="sxs-lookup"><span data-stu-id="c13d1-127">In Visual Studio create an ASP.NET MVC 4 application, name it SignalRChat, and click OK.</span></span>

        > [!NOTE]
        > <span data-ttu-id="c13d1-128">W programie VS 2010, wybrać **.NET Framework 4** z rozwijanej listy formantów wersji Framework.</span><span class="sxs-lookup"><span data-stu-id="c13d1-128">In VS 2010, select **.NET Framework 4** in the Framework version dropdown control.</span></span> <span data-ttu-id="c13d1-129">Wersje programu .NET Framework 4 i 4.5 działa kod SignalR.</span><span class="sxs-lookup"><span data-stu-id="c13d1-129">SignalR code runs on .NET Framework versions 4 and 4.5.</span></span>

        ![Tworzenie sieci web mvc](tutorial-getting-started-with-signalr-and-mvc-4/_static/image3.png)
      2. <span data-ttu-id="c13d1-131">Wybierz szablon aplikacji internetowych, wyczyść opcję **Tworzenie projektu testu jednostkowego**i kliknij przycisk OK.</span><span class="sxs-lookup"><span data-stu-id="c13d1-131">Select the Internet Application template, clear the option to **Create a unit test project**, and click OK.</span></span>

         ![Tworzenie witryny internetowej mvc](tutorial-getting-started-with-signalr-and-mvc-4/_static/image4.png)
      3. <span data-ttu-id="c13d1-133">Otwórz **narzędzia | Menedżer pakietów biblioteki | Konsola Menedżera pakietów** i uruchom następujące polecenie.</span><span class="sxs-lookup"><span data-stu-id="c13d1-133">Open the **Tools | Library Package Manager | Package Manager Console** and run the following command.</span></span> <span data-ttu-id="c13d1-134">Ten krok powoduje dodanie do projektu, zbiór plików skryptów i odwołania do zestawów, które umożliwiają funkcji SignalR.</span><span class="sxs-lookup"><span data-stu-id="c13d1-134">This step adds to the project a set of script files and assembly references that enable SignalR functionality.</span></span>

         `install-package Microsoft.AspNet.SignalR -Version 1.1.3`
      4. <span data-ttu-id="c13d1-135">W **Eksploratora rozwiązań** rozwiń folder skryptów.</span><span class="sxs-lookup"><span data-stu-id="c13d1-135">In **Solution Explorer** expand the Scripts folder.</span></span> <span data-ttu-id="c13d1-136">Należy pamiętać, że biblioteki skryptów dla elementu SignalR zostały dodane do projektu.</span><span class="sxs-lookup"><span data-stu-id="c13d1-136">Note that script libraries for SignalR have been added to the project.</span></span>

         ![Odwołań do biblioteki](tutorial-getting-started-with-signalr-and-mvc-4/_static/image6.png)
      5. <span data-ttu-id="c13d1-138">W **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy projekt, wybierz **Dodaj | Nowy Folder**, i Dodaj nowy folder o nazwie **koncentratory**.</span><span class="sxs-lookup"><span data-stu-id="c13d1-138">In **Solution Explorer**, right-click the project, select **Add | New Folder**, and add a new folder named **Hubs**.</span></span>
      6. <span data-ttu-id="c13d1-139">Kliknij prawym przyciskiem myszy **Hubs** folderu, kliknij przycisk **Dodaj | Klasa**i Utwórz nową klasę C# o nazwie **ChatHub.cs**.</span><span class="sxs-lookup"><span data-stu-id="c13d1-139">Right-click the **Hubs** folder, click **Add | Class**, and create a new C# class named **ChatHub.cs**.</span></span> <span data-ttu-id="c13d1-140">Ta klasa będzie używany jako koncentrator serwera SignalR, która wysyła komunikaty do wszystkich klientów.</span><span class="sxs-lookup"><span data-stu-id="c13d1-140">You will use this class as a SignalR server hub that sends messages to all clients.</span></span>

> [!NOTE]
> <span data-ttu-id="c13d1-141">Jeśli używasz programu Visual Studio 2012 i zainstalowano [aktualizacji platformy ASP.NET i Web Tools 2012.2](../../../visual-studio/overview/2012/aspnet-and-web-tools-20122-release-notes-rtw.md#_Installation), nowy szablon elementu SignalR można użyć do utworzenia klasy koncentratora.</span><span class="sxs-lookup"><span data-stu-id="c13d1-141">If you use Visual Studio 2012 and have installed the [ASP.NET and Web Tools 2012.2 update](../../../visual-studio/overview/2012/aspnet-and-web-tools-20122-release-notes-rtw.md#_Installation), you can use the new SignalR item template to create the hub class.</span></span> <span data-ttu-id="c13d1-142">Aby to zrobić, kliknij prawym przyciskiem myszy **Hubs** folderu, kliknij przycisk **Dodaj | Nowy element**, wybierz opcję **klasa Centrum SignalR (v1)** i nazwę klasy **ChatHub.cs**.</span><span class="sxs-lookup"><span data-stu-id="c13d1-142">To do that, right-click the **Hubs** folder, click **Add | New Item**, select **SignalR Hub Class (v1)**, and name the class **ChatHub.cs**.</span></span>


1. <span data-ttu-id="c13d1-143">Zastąp kod w **ChatHub** klasy z następującym kodem.</span><span class="sxs-lookup"><span data-stu-id="c13d1-143">Replace the code in the **ChatHub** class with the following code.</span></span>

    [!code-csharp[Main](tutorial-getting-started-with-signalr-and-mvc-4/samples/sample1.cs)]
2. <span data-ttu-id="c13d1-144">Otwórz **Global.asax** pliku projektu i dodaj wywołanie do metody `RouteTable.Routes.MapHubs();` jako pierwszy wiersz kodu w `Application_Start` metody.</span><span class="sxs-lookup"><span data-stu-id="c13d1-144">Open the **Global.asax** file for the project, and add a call to the method `RouteTable.Routes.MapHubs();` as the first line of code in the `Application_Start` method.</span></span> <span data-ttu-id="c13d1-145">Ten kod rejestruje domyślną trasę dla centrów SignalR i musi zostać wywołana przed zarejestrowaniem jakichkolwiek innych tras.</span><span class="sxs-lookup"><span data-stu-id="c13d1-145">This code registers the default route for SignalR hubs and must be called before you register any other routes.</span></span> <span data-ttu-id="c13d1-146">Gotowy `Application_Start` metoda wygląda podobnie jak w poniższym przykładzie.</span><span class="sxs-lookup"><span data-stu-id="c13d1-146">The completed `Application_Start` method looks like the following example.</span></span>

    [!code-csharp[Main](tutorial-getting-started-with-signalr-and-mvc-4/samples/sample2.cs)]
3. <span data-ttu-id="c13d1-147">Edytuj `HomeController` klasy znalezione w **Controllers/HomeController.cs** i dodaj następującą metodę do klasy.</span><span class="sxs-lookup"><span data-stu-id="c13d1-147">Edit the `HomeController` class found in **Controllers/HomeController.cs** and add the following method to the class.</span></span> <span data-ttu-id="c13d1-148">Ta metoda zwraca **Chat** widok, który spowoduje utworzenie w późniejszym kroku.</span><span class="sxs-lookup"><span data-stu-id="c13d1-148">This method returns the **Chat** view that you will create in a later step.</span></span>

    [!code-csharp[Main](tutorial-getting-started-with-signalr-and-mvc-4/samples/sample3.cs)]
4. <span data-ttu-id="c13d1-149">Kliknij prawym przyciskiem myszy w obrębie `Chat` metoda właśnie utworzony i kliknij przycisk **Dodaj widok** do utworzenia nowego pliku widoku.</span><span class="sxs-lookup"><span data-stu-id="c13d1-149">Right-click within the `Chat` method you just created, and click **Add View** to create a new view file.</span></span>
5. <span data-ttu-id="c13d1-150">W **Dodaj widok** okna dialogowego, upewnij się, że zaznaczone jest pole wyboru do **Użyj układ strony wzorcowej** (Usuń zaznaczenie pola wyboru), a następnie kliknij przycisk **Dodaj**.</span><span class="sxs-lookup"><span data-stu-id="c13d1-150">In the **Add View** dialog, make sure the check box is selected to **Use a layout or master page** (clear the other check boxes), and then click **Add**.</span></span>

    ![Dodawanie widoku](tutorial-getting-started-with-signalr-and-mvc-4/_static/image8.png)
6. <span data-ttu-id="c13d1-152">Edytuj nowy plik widoku o nazwie **Chat.cshtml**.</span><span class="sxs-lookup"><span data-stu-id="c13d1-152">Edit the new view file named **Chat.cshtml**.</span></span> <span data-ttu-id="c13d1-153">Po &lt;h2&gt; tag, wklej następujący kod &lt;div&gt; sekcji i `@section scripts` blok kodu do strony.</span><span class="sxs-lookup"><span data-stu-id="c13d1-153">After the &lt;h2&gt; tag, paste the following &lt;div&gt; section and `@section scripts` code block into the page.</span></span> <span data-ttu-id="c13d1-154">Ten skrypt umożliwia strony Aby wysyłać wiadomości rozmów i wyświetlenie komunikatów z serwera.</span><span class="sxs-lookup"><span data-stu-id="c13d1-154">This script enables the page to send chat messages and display messages from the server.</span></span> <span data-ttu-id="c13d1-155">Kompletny kod dla widoku rozmowy pojawia się w następującym fragmencie kodu.</span><span class="sxs-lookup"><span data-stu-id="c13d1-155">The complete code for the chat view appears in the following code block.</span></span>

    > [!IMPORTANT]
    > <span data-ttu-id="c13d1-156">Po dodaniu SignalR i inne biblioteki skryptu do projektu programu Visual Studio, Menedżera pakietów może zainstalować wersje skrypty, które są nowsze niż wersje przedstawione w tym temacie.</span><span class="sxs-lookup"><span data-stu-id="c13d1-156">When you add SignalR and other script libraries to your Visual Studio project, the Package Manager might install versions of the scripts that are more recent than the versions shown in this topic.</span></span> <span data-ttu-id="c13d1-157">Upewnij się, że odwołania do skryptu w kodzie są takie same wersje bibliotek skryptów zainstalowana w twoim projekcie.</span><span class="sxs-lookup"><span data-stu-id="c13d1-157">Make sure that the script references in your code match the versions of the script libraries installed in your project.</span></span>

    [!code-cshtml[Main](tutorial-getting-started-with-signalr-and-mvc-4/samples/sample4.cshtml)]
7. <span data-ttu-id="c13d1-158">**Zapisz wszystko** dla projektu.</span><span class="sxs-lookup"><span data-stu-id="c13d1-158">**Save All** for the project.</span></span>

<a id="run"></a>

## <a name="run-the-sample"></a><span data-ttu-id="c13d1-159">Uruchamianie aplikacji przykładowej</span><span class="sxs-lookup"><span data-stu-id="c13d1-159">Run the Sample</span></span>

1. <span data-ttu-id="c13d1-160">Naciśnij klawisz F5, aby uruchomić projekt w trybie debugowania.</span><span class="sxs-lookup"><span data-stu-id="c13d1-160">Press F5 to run the project in debug mode.</span></span>
2. <span data-ttu-id="c13d1-161">W wierszu adresu przeglądarki, dołącz **/home/rozmowy** adres URL domyślnej strony dla projektu.</span><span class="sxs-lookup"><span data-stu-id="c13d1-161">In the browser address line, append **/home/chat** to the URL of the default page for the project.</span></span> <span data-ttu-id="c13d1-162">Rozmowa strona ładuje się w wystąpieniu przeglądarki i wyświetla monit o nazwę użytkownika.</span><span class="sxs-lookup"><span data-stu-id="c13d1-162">The Chat page loads in a browser instance and prompts for a user name.</span></span>

    ![Wprowadź nazwę użytkownika](tutorial-getting-started-with-signalr-and-mvc-4/_static/image9.png)
3. <span data-ttu-id="c13d1-164">Wprowadź nazwę użytkownika.</span><span class="sxs-lookup"><span data-stu-id="c13d1-164">Enter a user name.</span></span>
4. <span data-ttu-id="c13d1-165">Skopiuj adres URL w wierszu adresu przeglądarki i użyj go, aby otworzyć dwóch większej liczby wystąpień przeglądarki.</span><span class="sxs-lookup"><span data-stu-id="c13d1-165">Copy the URL from the address line of the browser and use it to open two more browser instances.</span></span> <span data-ttu-id="c13d1-166">W każdym wystąpieniu przeglądarki wprowadź unikatową nazwę użytkownika.</span><span class="sxs-lookup"><span data-stu-id="c13d1-166">In each browser instance, enter a unique user name.</span></span>
5. <span data-ttu-id="c13d1-167">W każdym wystąpieniu przeglądarki Dodaj komentarz, a następnie kliknij przycisk **wysyłania**.</span><span class="sxs-lookup"><span data-stu-id="c13d1-167">In each browser instance, add a comment and click **Send**.</span></span> <span data-ttu-id="c13d1-168">Komentarze powinien być wyświetlany we wszystkich wystąpieniach przeglądarki.</span><span class="sxs-lookup"><span data-stu-id="c13d1-168">The comments should display in all browser instances.</span></span>

    > [!NOTE]
    > <span data-ttu-id="c13d1-169">Tej aplikacji rozmów prostego nie utrzymuje kontekst dyskusji na serwerze.</span><span class="sxs-lookup"><span data-stu-id="c13d1-169">This simple chat application does not maintain the discussion context on the server.</span></span> <span data-ttu-id="c13d1-170">Centrum emituje komentarzy do wszystkich bieżących użytkowników.</span><span class="sxs-lookup"><span data-stu-id="c13d1-170">The hub broadcasts comments to all current users.</span></span> <span data-ttu-id="c13d1-171">Użytkownicy, którzy dołączają do czatu później zostanie wyświetlony komunikat o dodane od czasu dołączenia.</span><span class="sxs-lookup"><span data-stu-id="c13d1-171">Users who join the chat later will see messages added from the time they join.</span></span>
6. <span data-ttu-id="c13d1-172">Poniższy zrzut ekranu przedstawia aplikacji rozmów w przeglądarce.</span><span class="sxs-lookup"><span data-stu-id="c13d1-172">The following screen shot shows the chat application running in a browser.</span></span>

    ![Rozmowa przeglądarki](tutorial-getting-started-with-signalr-and-mvc-4/_static/image11.png)
7. <span data-ttu-id="c13d1-174">W **Eksploratora rozwiązań**, zbadaj **dokumenty skryptów** węzła dla działającej aplikacji.</span><span class="sxs-lookup"><span data-stu-id="c13d1-174">In **Solution Explorer**, inspect the **Script Documents** node for the running application.</span></span> <span data-ttu-id="c13d1-175">Ten węzeł jest widoczny w trybie debugowania, jeśli używasz programu Internet Explorer jako przeglądarki.</span><span class="sxs-lookup"><span data-stu-id="c13d1-175">This node is visible in debug mode if you are using Internet Explorer as your browser.</span></span> <span data-ttu-id="c13d1-176">Brak pliku skryptu o nazwie **koncentratory** generujący biblioteki SignalR dynamicznie w czasie wykonywania.</span><span class="sxs-lookup"><span data-stu-id="c13d1-176">There is a script file named **hubs** that the SignalR library dynamically generates at runtime.</span></span> <span data-ttu-id="c13d1-177">Ten plik zarządza komunikacji między jQuery skryptu i kod po stronie serwera.</span><span class="sxs-lookup"><span data-stu-id="c13d1-177">This file manages the communication between jQuery script and server-side code.</span></span> <span data-ttu-id="c13d1-178">Jeśli używasz przeglądarki innej niż Internet Explorer, można także przejść do dynamicznego **hubs** plików, przechodząc do niego bezpośrednio, na przykład http://mywebsite/signalr/hubs.</span><span class="sxs-lookup"><span data-stu-id="c13d1-178">If you use a browser other than Internet Explorer, you can also access the dynamic **hubs** file by browsing to it directly, for example http://mywebsite/signalr/hubs.</span></span>

    ![Wygenerowany Centrum skryptów](tutorial-getting-started-with-signalr-and-mvc-4/_static/image13.png)

<a id="code"></a>

## <a name="examine-the-code"></a><span data-ttu-id="c13d1-180">Poszukaj w kodzie</span><span class="sxs-lookup"><span data-stu-id="c13d1-180">Examine the Code</span></span>

<span data-ttu-id="c13d1-181">Aplikacji rozmów SignalR pokazuje dwa podstawowe zadania rozwoju SignalR: tworzenie koncentratora jako obiekt główny koordynacji na serwerze i za pomocą biblioteki jQuery SignalR do wysyłania i odbierania komunikatów.</span><span class="sxs-lookup"><span data-stu-id="c13d1-181">The SignalR chat application demonstrates two basic SignalR development tasks: creating a hub as the main coordination object on the server, and using the SignalR jQuery library to send and receive messages.</span></span>

### <a name="signalr-hubs"></a><span data-ttu-id="c13d1-182">Koncentratory SignalR</span><span class="sxs-lookup"><span data-stu-id="c13d1-182">SignalR Hubs</span></span>

<span data-ttu-id="c13d1-183">W przykładowym kodzie **ChatHub** klasa pochodzi od **Microsoft.AspNet.SignalR.Hub** klasy.</span><span class="sxs-lookup"><span data-stu-id="c13d1-183">In the code sample the **ChatHub** class derives from the **Microsoft.AspNet.SignalR.Hub** class.</span></span> <span data-ttu-id="c13d1-184">Wyprowadzanie z **Centrum** klasy jest to wygodny sposób, aby skompilować aplikację SignalR.</span><span class="sxs-lookup"><span data-stu-id="c13d1-184">Deriving from the **Hub** class is a useful way to build a SignalR application.</span></span> <span data-ttu-id="c13d1-185">Można utworzyć metody publiczne na klasy koncentratora, a następnie uzyskać dostęp do tych metod, wywołując je z jQuery skrypty na stronie sieci web.</span><span class="sxs-lookup"><span data-stu-id="c13d1-185">You can create public methods on your hub class and then access those methods by calling them from jQuery scripts in a web page.</span></span>

<span data-ttu-id="c13d1-186">W kodzie Rozmowa klienci wywołują **ChatHub.Send** metodę, aby wysyłać nowy komunikat.</span><span class="sxs-lookup"><span data-stu-id="c13d1-186">In the chat code, clients call the **ChatHub.Send** method to send a new message.</span></span> <span data-ttu-id="c13d1-187">Centrum z kolei wysyła wiadomość do wszystkich klientów, wywołując **Clients.All.addNewMessageToPage**.</span><span class="sxs-lookup"><span data-stu-id="c13d1-187">The hub in turn sends the message to all clients by calling **Clients.All.addNewMessageToPage**.</span></span>

<span data-ttu-id="c13d1-188">**Wysyłania** metoda pokazuje kilka koncepcji w Centrum:</span><span class="sxs-lookup"><span data-stu-id="c13d1-188">The **Send** method demonstrates several hub concepts :</span></span>

- <span data-ttu-id="c13d1-189">Tak, aby je wywoływać klientów do deklarowania metod publicznych w koncentratorze.</span><span class="sxs-lookup"><span data-stu-id="c13d1-189">Declare public methods on a hub so that clients can call them.</span></span>
- <span data-ttu-id="c13d1-190">Użyj **Microsoft.AspNet.SignalR.Hub.Clients** właściwość dostęp do wszystkich klientów podłączone do tego koncentratora.</span><span class="sxs-lookup"><span data-stu-id="c13d1-190">Use the **Microsoft.AspNet.SignalR.Hub.Clients** property to access all clients connected to this hub.</span></span>
- <span data-ttu-id="c13d1-191">Wywołaj funkcję jQuery na kliencie (takich jak `addNewMessageToPage` funkcji) aby zaktualizować klientów.</span><span class="sxs-lookup"><span data-stu-id="c13d1-191">Call a jQuery function on the client (such as the `addNewMessageToPage` function) to update clients.</span></span>

    [!code-csharp[Main](tutorial-getting-started-with-signalr-and-mvc-4/samples/sample5.cs)]

### <a name="signalr-and-jquery"></a><span data-ttu-id="c13d1-192">SignalR i jQuery</span><span class="sxs-lookup"><span data-stu-id="c13d1-192">SignalR and jQuery</span></span>

<span data-ttu-id="c13d1-193">**Chat.cshtml** plik widoku w przykładzie kodu pokazano, jak użyć biblioteki jQuery SignalR do komunikowania się z Centrum SignalR.</span><span class="sxs-lookup"><span data-stu-id="c13d1-193">The **Chat.cshtml** view file in the code sample shows how to use the SignalR jQuery library to communicate with a SignalR hub.</span></span> <span data-ttu-id="c13d1-194">Podstawowe zadania w kodzie do utworzenia odwołania do automatycznego generowania serwera proxy koncentratora, deklarowania funkcji, która serwera można wywołać w celu wypychania zawartości do klientów i od połączenia do wysyłania komunikatów do Centrum.</span><span class="sxs-lookup"><span data-stu-id="c13d1-194">The essential tasks in the code are creating a reference to the auto-generated proxy for the hub, declaring a function that the server can call to push content to clients, and starting a connection to send messages to the hub.</span></span>

<span data-ttu-id="c13d1-195">Poniższy kod deklaruje serwera proxy koncentratora.</span><span class="sxs-lookup"><span data-stu-id="c13d1-195">The following code declares a proxy for a hub.</span></span>

[!code-javascript[Main](tutorial-getting-started-with-signalr-and-mvc-4/samples/sample6.js)]

> [!NOTE]
> <span data-ttu-id="c13d1-196">Odwołanie do klasy serwera i jej elementów członkowskich w jQuery znajduje się w camelcase.</span><span class="sxs-lookup"><span data-stu-id="c13d1-196">In jQuery the reference to the server class and its members is in camel case.</span></span> <span data-ttu-id="c13d1-197">Przykładowy kod odwołuje się do języka C# **ChatHub** klasy w jQuery jako **chatHub**.</span><span class="sxs-lookup"><span data-stu-id="c13d1-197">The code sample references the C# **ChatHub** class in jQuery as **chatHub**.</span></span> <span data-ttu-id="c13d1-198">Jeśli chcesz, aby odwołać się do `ChatHub` klasy jQuery za pomocą konwencjonalnych Pascal wielkość liter jak w języku C#, Edytuj plik klasy ChatHub.cs.</span><span class="sxs-lookup"><span data-stu-id="c13d1-198">If you want to reference the `ChatHub` class in jQuery with conventional Pascal casing as you would in C#, edit the ChatHub.cs class file.</span></span> <span data-ttu-id="c13d1-199">Dodaj `using` instrukcję, aby odwoływać się do `Microsoft.AspNet.SignalR.Hubs` przestrzeni nazw.</span><span class="sxs-lookup"><span data-stu-id="c13d1-199">Add a `using` statement to reference the `Microsoft.AspNet.SignalR.Hubs` namespace.</span></span> <span data-ttu-id="c13d1-200">Następnie dodaj `HubName` atrybutu `ChatHub` klasy, na przykład `[HubName("ChatHub")]`.</span><span class="sxs-lookup"><span data-stu-id="c13d1-200">Then add the `HubName` attribute to the `ChatHub` class, for example `[HubName("ChatHub")]`.</span></span> <span data-ttu-id="c13d1-201">Na koniec zaktualizuj użytkownikowi jQuery do `ChatHub` klasy.</span><span class="sxs-lookup"><span data-stu-id="c13d1-201">Finally, update your jQuery reference to the `ChatHub` class.</span></span>


<span data-ttu-id="c13d1-202">Poniższy kod przedstawia sposób tworzenia funkcji wywołania zwrotnego w skrypcie.</span><span class="sxs-lookup"><span data-stu-id="c13d1-202">The following code shows how to create a callback function in the script.</span></span> <span data-ttu-id="c13d1-203">Klasy koncentratora na serwerze wywołuje tę funkcję, aby wypchnąć aktualizacji zawartości dla każdego klienta.</span><span class="sxs-lookup"><span data-stu-id="c13d1-203">The hub class on the server calls this function to push content updates to each client.</span></span> <span data-ttu-id="c13d1-204">Opcjonalne wywołanie `htmlEncode` funkcja przedstawiono sposób HTML zakodować zawartość komunikatu przed wyświetleniem go na stronie, aby uniemożliwić uruchomienie skryptu.</span><span class="sxs-lookup"><span data-stu-id="c13d1-204">The optional call to the `htmlEncode` function shows a way to HTML encode the message content before displaying it in the page, as a way to prevent script injection.</span></span>

[!code-html[Main](tutorial-getting-started-with-signalr-and-mvc-4/samples/sample7.html)]

<span data-ttu-id="c13d1-205">Poniższy kod przedstawia sposób nawiązać połączenie z koncentratorem.</span><span class="sxs-lookup"><span data-stu-id="c13d1-205">The following code shows how to open a connection with the hub.</span></span> <span data-ttu-id="c13d1-206">Kod uruchamia połączenie i przekazuje go po funkcji do obsługi zdarzenia click na **wysyłania** przycisk na stronie rozmowy.</span><span class="sxs-lookup"><span data-stu-id="c13d1-206">The code starts the connection and then passes it a function to handle the click event on the **Send** button in the Chat page.</span></span>

> [!NOTE]
> <span data-ttu-id="c13d1-207">Takie podejście zapewnia, że połączenie zostało nawiązane przed wykonaniem programu obsługi zdarzeń.</span><span class="sxs-lookup"><span data-stu-id="c13d1-207">This approach ensures that the connection is established before the event handler executes.</span></span>


[!code-javascript[Main](tutorial-getting-started-with-signalr-and-mvc-4/samples/sample8.js)]

<a id="next"></a>

## <a name="next-steps"></a><span data-ttu-id="c13d1-208">Następne kroki</span><span class="sxs-lookup"><span data-stu-id="c13d1-208">Next Steps</span></span>

<span data-ttu-id="c13d1-209">Wiesz, że SignalR to architektura służąca do tworzenia aplikacji sieci web w czasie rzeczywistym.</span><span class="sxs-lookup"><span data-stu-id="c13d1-209">You learned that SignalR is a framework for building real-time web applications.</span></span> <span data-ttu-id="c13d1-210">Pokazaliśmy również kilka zadań programistycznych SignalR: jak dodać SignalR do aplikacji ASP.NET, sposób tworzenia klasy koncentratora oraz jak wysyłać i odbierać komunikaty z Centrum.</span><span class="sxs-lookup"><span data-stu-id="c13d1-210">You also learned several SignalR development tasks: how to add SignalR to an ASP.NET application, how to create a hub class, and how to send and receive messages from the hub.</span></span>

<span data-ttu-id="c13d1-211">Aby dowiedzieć się bardziej zaawansowanych pojęciach zmiany SignalR, odwiedź następującą witrynę dla SignalR kod źródłowy i zasobów:</span><span class="sxs-lookup"><span data-stu-id="c13d1-211">To learn more advanced SignalR developments concepts, visit the following sites for SignalR source code and resources :</span></span>

- [<span data-ttu-id="c13d1-212">Projekt SignalR</span><span class="sxs-lookup"><span data-stu-id="c13d1-212">SignalR Project</span></span>](http://signalr.net)
- [<span data-ttu-id="c13d1-213">SignalR Github i przykłady</span><span class="sxs-lookup"><span data-stu-id="c13d1-213">SignalR Github and Samples</span></span>](https://github.com/SignalR/SignalR)
- [<span data-ttu-id="c13d1-214">Witryny typu Wiki biblioteki SignalR</span><span class="sxs-lookup"><span data-stu-id="c13d1-214">SignalR Wiki</span></span>](https://github.com/SignalR/SignalR/wiki)
