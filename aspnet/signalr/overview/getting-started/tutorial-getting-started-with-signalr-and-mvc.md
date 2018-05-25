---
uid: signalr/overview/getting-started/tutorial-getting-started-with-signalr-and-mvc
title: 'Samouczek: Wprowadzenie do korzystania z SignalR 2 i MVC 5 | Dokumentacja firmy Microsoft'
author: pfletcher
description: Ten samouczek pokazuje, jak używać ASP.NET SignalR 2 do tworzenia aplikacji rozmów w czasie rzeczywistym. Spowoduje dodanie SignalR do aplikacji MVC 5 i Utwórz widok rozmów...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/10/2014
ms.topic: article
ms.assetid: 80bfe5fb-bdfc-41fe-ac43-2132e5d69fac
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/getting-started/tutorial-getting-started-with-signalr-and-mvc
msc.type: authoredcontent
ms.openlocfilehash: ca0471114da7363c5df9d459308708e7ab4f8b84
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/24/2018
---
<a name="tutorial-getting-started-with-signalr-2-and-mvc-5"></a><span data-ttu-id="7d897-104">Samouczek: Wprowadzenie do korzystania z SignalR 2 i MVC 5</span><span class="sxs-lookup"><span data-stu-id="7d897-104">Tutorial: Getting Started with SignalR 2 and MVC 5</span></span>
====================
<span data-ttu-id="7d897-105">przez [Patrick Fletcher](https://github.com/pfletcher), [Teebken Timowi](https://github.com/timlt)</span><span class="sxs-lookup"><span data-stu-id="7d897-105">by [Patrick Fletcher](https://github.com/pfletcher), [Tim Teebken](https://github.com/timlt)</span></span>

[<span data-ttu-id="7d897-106">Pobieranie ukończone projektu</span><span class="sxs-lookup"><span data-stu-id="7d897-106">Download Completed Project</span></span>](http://code.msdn.microsoft.com/Getting-Started-with-c366b2f3)

> <span data-ttu-id="7d897-107">Ten samouczek pokazuje, jak używać ASP.NET SignalR 2 do tworzenia aplikacji rozmów w czasie rzeczywistym.</span><span class="sxs-lookup"><span data-stu-id="7d897-107">This tutorial shows how to use ASP.NET SignalR 2 to create a real-time chat application.</span></span> <span data-ttu-id="7d897-108">Spowoduje dodanie SignalR do aplikacji MVC 5 i utworzyć widok rozmów do wysyłania i wyświetlenie komunikatów.</span><span class="sxs-lookup"><span data-stu-id="7d897-108">You will add SignalR to an MVC 5 application and create a chat view to send and display messages.</span></span> 
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="7d897-109">Używane w samouczku wersje oprogramowania</span><span class="sxs-lookup"><span data-stu-id="7d897-109">Software versions used in the tutorial</span></span>
> 
> 
> - [<span data-ttu-id="7d897-110">Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="7d897-110">Visual Studio 2013</span></span>](https://www.microsoft.com/visualstudio/eng/2013-downloads)
> - <span data-ttu-id="7d897-111">.NET 4.5</span><span class="sxs-lookup"><span data-stu-id="7d897-111">.NET 4.5</span></span>
> - <span data-ttu-id="7d897-112">MVC 5</span><span class="sxs-lookup"><span data-stu-id="7d897-112">MVC 5</span></span>
> - <span data-ttu-id="7d897-113">SignalR w wersji 2</span><span class="sxs-lookup"><span data-stu-id="7d897-113">SignalR version 2</span></span>
>   
> 
> 
> ## <a name="using-visual-studio-2012-with-this-tutorial"></a><span data-ttu-id="7d897-114">Z tego samouczka przy użyciu programu Visual Studio 2012</span><span class="sxs-lookup"><span data-stu-id="7d897-114">Using Visual Studio 2012 with this tutorial</span></span>
> 
> 
> <span data-ttu-id="7d897-115">Aby używać programu Visual Studio 2012 z tego samouczka, wykonaj następujące czynności:</span><span class="sxs-lookup"><span data-stu-id="7d897-115">To use Visual Studio 2012 with this tutorial, do the following:</span></span>
> 
> - <span data-ttu-id="7d897-116">Aktualizacja Twojego [Menedżera pakietów](http://docs.nuget.org/docs/start-here/installing-nuget) do najnowszej wersji.</span><span class="sxs-lookup"><span data-stu-id="7d897-116">Update your [Package Manager](http://docs.nuget.org/docs/start-here/installing-nuget) to the latest version.</span></span>
> - <span data-ttu-id="7d897-117">Zainstaluj [sieci Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx).</span><span class="sxs-lookup"><span data-stu-id="7d897-117">Install the [Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx).</span></span>
> - <span data-ttu-id="7d897-118">Instalator platformy sieci Web, wyszukiwanie i instalowanie **ASP.NET i 2013.1 narzędzia sieci Web dla programu Visual Studio 2012**.</span><span class="sxs-lookup"><span data-stu-id="7d897-118">In the Web Platform Installer, search for and install **ASP.NET and Web Tools 2013.1 for Visual Studio 2012**.</span></span> <span data-ttu-id="7d897-119">Spowoduje to zainstalowanie szablony programu Visual Studio dla biblioteki SignalR klas takich jak **Centrum**.</span><span class="sxs-lookup"><span data-stu-id="7d897-119">This will install Visual Studio templates for SignalR classes such as **Hub**.</span></span>
> - <span data-ttu-id="7d897-120">Niektóre szablony (takich jak **klasy początkowej OWIN**) nie są dostępne; w tym przypadku powinien użyć pliku klasy.</span><span class="sxs-lookup"><span data-stu-id="7d897-120">Some templates (such as **OWIN Startup Class**) will not be available; for these, use a Class file instead.</span></span>
> 
> 
> ## <a name="tutorial-versions"></a><span data-ttu-id="7d897-121">Samouczek wersji</span><span class="sxs-lookup"><span data-stu-id="7d897-121">Tutorial Versions</span></span>
> 
> <span data-ttu-id="7d897-122">Aby uzyskać informacje dotyczące starszych wersji biblioteki signalr, zobacz [starsze wersje biblioteki SignalR](../older-versions/index.md).</span><span class="sxs-lookup"><span data-stu-id="7d897-122">For information about earlier versions of SignalR, see [SignalR Older Versions](../older-versions/index.md).</span></span>
> 
> ## <a name="questions-and-comments"></a><span data-ttu-id="7d897-123">Pytania i komentarze</span><span class="sxs-lookup"><span data-stu-id="7d897-123">Questions and comments</span></span>
> 
> <span data-ttu-id="7d897-124">Wystaw opinię na jak zbędne tego samouczka i jakie firma Microsoft może poprawić w komentarze u dołu strony.</span><span class="sxs-lookup"><span data-stu-id="7d897-124">Please leave feedback on how you liked this tutorial and what we could improve in the comments at the bottom of the page.</span></span> <span data-ttu-id="7d897-125">Jeśli masz pytania, które nie są bezpośrednio związane z tego samouczka możesz zamieścić je do [forum ASP.NET SignalR](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) lub [StackOverflow.com](http://stackoverflow.com/).</span><span class="sxs-lookup"><span data-stu-id="7d897-125">If you have questions that are not directly related to the tutorial, you can post them to the [ASP.NET SignalR forum](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) or [StackOverflow.com](http://stackoverflow.com/).</span></span>


## <a name="overview"></a><span data-ttu-id="7d897-126">Omówienie</span><span class="sxs-lookup"><span data-stu-id="7d897-126">Overview</span></span>

<span data-ttu-id="7d897-127">Ten samouczek stanowi wprowadzenie do programowania aplikacji sieci web w czasie rzeczywistym z 2 SignalR platformy ASP.NET i ASP.NET MVC 5.</span><span class="sxs-lookup"><span data-stu-id="7d897-127">This tutorial introduces you to real-time web application development with ASP.NET SignalR 2 and ASP.NET MVC 5.</span></span> <span data-ttu-id="7d897-128">W samouczku ten sam kod aplikacji rozmów jako [SignalR Wprowadzenie — samouczek](tutorial-getting-started-with-signalr.md), ale pokazano, jak dodać go do aplikacji MVC 5.</span><span class="sxs-lookup"><span data-stu-id="7d897-128">The tutorial uses the same chat application code as the [SignalR Getting Started tutorial](tutorial-getting-started-with-signalr.md), but shows how to add it to an MVC 5 application.</span></span>

<span data-ttu-id="7d897-129">W tym temacie przedstawiono następujące zadania programowanie SignalR:</span><span class="sxs-lookup"><span data-stu-id="7d897-129">In this topic you will learn the following SignalR development tasks:</span></span>

- <span data-ttu-id="7d897-130">Dodawanie biblioteki SignalR do aplikacji MVC 5.</span><span class="sxs-lookup"><span data-stu-id="7d897-130">Adding the SignalR library to an MVC 5 application.</span></span>
- <span data-ttu-id="7d897-131">Tworzenie klasy uruchamiania OWIN do dystrybuowania zawartości do klientów i koncentratora.</span><span class="sxs-lookup"><span data-stu-id="7d897-131">Creating hub and OWIN startup classes to push content to clients.</span></span>
- <span data-ttu-id="7d897-132">Za pomocą biblioteki jQuery SignalR na stronie sieci web można wysyłać wiadomości i wyświetlać aktualizacje z koncentratora.</span><span class="sxs-lookup"><span data-stu-id="7d897-132">Using the SignalR jQuery library in a web page to send messages and display updates from the hub.</span></span>

<span data-ttu-id="7d897-133">Poniższy zrzut ekranu przedstawia aplikacji rozmów ukończone działającego w przeglądarce.</span><span class="sxs-lookup"><span data-stu-id="7d897-133">The following screen shot shows the completed chat application running in a browser.</span></span>

![Rozmowa wystąpień](tutorial-getting-started-with-signalr-and-mvc/_static/image1.png)

<span data-ttu-id="7d897-135">Sekcje:</span><span class="sxs-lookup"><span data-stu-id="7d897-135">Sections:</span></span>

- [<span data-ttu-id="7d897-136">Konfigurowanie projektu</span><span class="sxs-lookup"><span data-stu-id="7d897-136">Set up the Project</span></span>](#setup)
- [<span data-ttu-id="7d897-137">Uruchom próbki</span><span class="sxs-lookup"><span data-stu-id="7d897-137">Run the Sample</span></span>](#run)
- [<span data-ttu-id="7d897-138">Sprawdź kod</span><span class="sxs-lookup"><span data-stu-id="7d897-138">Examine the Code</span></span>](#code)
- [<span data-ttu-id="7d897-139">Następne kroki</span><span class="sxs-lookup"><span data-stu-id="7d897-139">Next Steps</span></span>](#next)

<a id="setup"></a>

## <a name="set-up-the-project"></a><span data-ttu-id="7d897-140">Konfigurowanie projektu</span><span class="sxs-lookup"><span data-stu-id="7d897-140">Set up the Project</span></span>

<span data-ttu-id="7d897-141">Wymagania wstępne:</span><span class="sxs-lookup"><span data-stu-id="7d897-141">Prerequisites:</span></span>

- <span data-ttu-id="7d897-142">Visual Studio 2013.</span><span class="sxs-lookup"><span data-stu-id="7d897-142">Visual Studio 2013.</span></span> <span data-ttu-id="7d897-143">Jeśli nie masz programu Visual Studio, zobacz [pobiera ASP.NET](https://www.asp.net/downloads) uzyskać bezpłatne narzędzie Visual Studio 2013 Express programowanie.</span><span class="sxs-lookup"><span data-stu-id="7d897-143">If you do not have Visual Studio, see [ASP.NET Downloads](https://www.asp.net/downloads) to get the free Visual Studio 2013 Express Development Tool.</span></span>

<span data-ttu-id="7d897-144">W tej sekcji przedstawiono sposób tworzenia aplikacji ASP.NET MVC 5, Dodaj bibliotekę SignalR i tworzenie aplikacji czatu.</span><span class="sxs-lookup"><span data-stu-id="7d897-144">This section shows how to create an ASP.NET MVC 5 application, add the SignalR library, and create the chat application.</span></span>

1. <span data-ttu-id="7d897-145">W programie Visual Studio tworzenie aplikacji C# ASP.NET, którego element docelowy .NET Framework 4.5, nadaj jej nazwę SignalRChat i kliknij przycisk OK.</span><span class="sxs-lookup"><span data-stu-id="7d897-145">In Visual Studio, create a C# ASP.NET application that targets .NET Framework 4.5, name it SignalRChat, and click OK.</span></span>

    ![Tworzenie sieci web](tutorial-getting-started-with-signalr-and-mvc/_static/image2.png)
2. <span data-ttu-id="7d897-147">W `New ASP.NET Project` okna dialogowego, a następnie wybierz **MVC**i kliknij przycisk **Zmień uwierzytelnianie**.</span><span class="sxs-lookup"><span data-stu-id="7d897-147">In the `New ASP.NET Project` dialog, and select **MVC**, and click **Change Authentication**.</span></span>

    ![Tworzenie sieci web](tutorial-getting-started-with-signalr-and-mvc/_static/image3.png)
3. <span data-ttu-id="7d897-149">Wybierz **bez uwierzytelniania** w **Zmień uwierzytelnianie** okna dialogowego, a następnie kliknij przycisk **OK**.</span><span class="sxs-lookup"><span data-stu-id="7d897-149">Select **No Authentication** in the **Change Authentication** dialog, and click **OK**.</span></span>

    ![Wybierz bez uwierzytelniania](tutorial-getting-started-with-signalr-and-mvc/_static/image4.png)

    > [!NOTE]
    > <span data-ttu-id="7d897-151">W przypadku wybrania dostawcy uwierzytelniania innej aplikacji, `Startup.cs` klasy zostaną utworzone automatycznie; nie należy tworzyć własne `Startup.cs` klasy w kroku 10 poniżej.</span><span class="sxs-lookup"><span data-stu-id="7d897-151">If you select a different authentication provider for your application, a `Startup.cs` class will be created for you; you will not need to create your own `Startup.cs` class in step 10 below.</span></span>
4. <span data-ttu-id="7d897-152">Kliknij przycisk **OK** w **nowy projekt ASP.NET** okna dialogowego.</span><span class="sxs-lookup"><span data-stu-id="7d897-152">Click **OK** in the **New ASP.NET Project** dialog.</span></span>
5. <span data-ttu-id="7d897-153">Otwórz **narzędzia | Menedżer pakietów biblioteki | Konsola Menedżera pakietów** i uruchom następujące polecenie.</span><span class="sxs-lookup"><span data-stu-id="7d897-153">Open the **Tools | Library Package Manager | Package Manager Console** and run the following command.</span></span> <span data-ttu-id="7d897-154">Ten krok powoduje dodanie do projektu zestawu plików skryptów i włączyć funkcję SignalR odwołań do zestawu.</span><span class="sxs-lookup"><span data-stu-id="7d897-154">This step adds to the project a set of script files and assembly references that enable SignalR functionality.</span></span>

    `install-package Microsoft.AspNet.SignalR`
6. <span data-ttu-id="7d897-155">W **Eksploratora rozwiązań**, rozwiń folder skryptów.</span><span class="sxs-lookup"><span data-stu-id="7d897-155">In **Solution Explorer**, expand the Scripts folder.</span></span> <span data-ttu-id="7d897-156">Należy pamiętać, że skrypt biblioteki SignalR zostały dodane do projektu.</span><span class="sxs-lookup"><span data-stu-id="7d897-156">Note that script libraries for SignalR have been added to the project.</span></span>

    ![Folder skryptów](tutorial-getting-started-with-signalr-and-mvc/_static/image5.png)
7. <span data-ttu-id="7d897-158">W **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy projekt, wybierz **Dodaj | Nowy Folder**, i Dodaj nowy folder o nazwie **koncentratory**.</span><span class="sxs-lookup"><span data-stu-id="7d897-158">In **Solution Explorer**, right-click the project, select **Add | New Folder**, and add a new folder named **Hubs**.</span></span>
8. <span data-ttu-id="7d897-159">Kliknij prawym przyciskiem myszy **koncentratory** folderu, kliknij przycisk **Dodaj | Nowy element**, wybierz pozycję **Visual C# | Sieci Web | SignalR** w węźle **zainstalowana** okienku wybierz **klasy koncentratora SignalR (v2)** w środkowym okienku i Utwórz nowe Centrum o nazwie **ChatHub.cs**.</span><span class="sxs-lookup"><span data-stu-id="7d897-159">Right-click the **Hubs** folder, click **Add | New Item**, select the **Visual C# | Web | SignalR** node in the **Installed** pane, select **SignalR Hub Class (v2)** from the center pane, and create a new hub named **ChatHub.cs**.</span></span> <span data-ttu-id="7d897-160">Ta klasa będzie używany jako SignalR koncentratora serwera, który wysyła komunikaty do wszystkich klientów.</span><span class="sxs-lookup"><span data-stu-id="7d897-160">You will use this class as a SignalR server hub that sends messages to all clients.</span></span>

    ![Utwórz nowe Centrum](tutorial-getting-started-with-signalr-and-mvc/_static/image6.png)
9. <span data-ttu-id="7d897-162">Zastąp kod w **ChatHub** klasy następującym kodem.</span><span class="sxs-lookup"><span data-stu-id="7d897-162">Replace the code in the **ChatHub** class with the following code.</span></span>

    [!code-csharp[Main](tutorial-getting-started-with-signalr-and-mvc/samples/sample1.cs)]
10. <span data-ttu-id="7d897-163">Utwórz nową klasę o nazwie pliku Startup.cs.</span><span class="sxs-lookup"><span data-stu-id="7d897-163">Create a new class called Startup.cs.</span></span> <span data-ttu-id="7d897-164">Zmienić zawartość pliku do następującego.</span><span class="sxs-lookup"><span data-stu-id="7d897-164">Change the contents of the file to the following.</span></span>

    [!code-csharp[Main](tutorial-getting-started-with-signalr-and-mvc/samples/sample2.cs)]
11. <span data-ttu-id="7d897-165">Edytuj `HomeController` znaleziono klasy w **Controllers/HomeController.cs** i dodaj następującą metodę do klasy.</span><span class="sxs-lookup"><span data-stu-id="7d897-165">Edit the `HomeController` class found in **Controllers/HomeController.cs** and add the following method to the class.</span></span> <span data-ttu-id="7d897-166">Ta metoda zwraca **rozmowę** widoku, który zostanie utworzony w kolejnym kroku.</span><span class="sxs-lookup"><span data-stu-id="7d897-166">This method returns the **Chat** view that you will create in a later step.</span></span>

    [!code-csharp[Main](tutorial-getting-started-with-signalr-and-mvc/samples/sample3.cs)]
12. <span data-ttu-id="7d897-167">Kliknij prawym przyciskiem myszy **widoków domowych** folder, a następnie wybierz **Dodaj... | Widok**.</span><span class="sxs-lookup"><span data-stu-id="7d897-167">Right-click the **Views/Home** folder, and select **Add... | View**.</span></span>
13. <span data-ttu-id="7d897-168">W **Dodaj widok** okna dialogowego, nazwę nowego widoku **rozmowę**.</span><span class="sxs-lookup"><span data-stu-id="7d897-168">In the **Add View** dialog, name the new view **Chat**.</span></span>

    ![Dodawanie widoku](tutorial-getting-started-with-signalr-and-mvc/_static/image7.png)
14. <span data-ttu-id="7d897-170">Zastąp zawartość **Chat.cshtml** następującym kodem.</span><span class="sxs-lookup"><span data-stu-id="7d897-170">Replace the contents of **Chat.cshtml** with the following code.</span></span>

    > [!IMPORTANT]
    > <span data-ttu-id="7d897-171">Po dodaniu SignalR i innych bibliotek skryptu do projektu programu Visual Studio Menedżera pakietów mogą zainstalować wersję pliku skryptu SignalR, która jest nowsza niż wersja wyświetlane w tym temacie.</span><span class="sxs-lookup"><span data-stu-id="7d897-171">When you add SignalR and other script libraries to your Visual Studio project, the Package Manager might install a version of the SignalR script file that is more recent than the version shown in this topic.</span></span> <span data-ttu-id="7d897-172">Upewnij się, że odwołanie do skryptu w kodzie zgodna z wersją biblioteki skryptu zainstalowane w projekcie.</span><span class="sxs-lookup"><span data-stu-id="7d897-172">Make sure that the script reference in your code matches the version of the script library installed in your project.</span></span>

    [!code-cshtml[Main](tutorial-getting-started-with-signalr-and-mvc/samples/sample4.cshtml)]
15. <span data-ttu-id="7d897-173">**Zapisz wszystkie** dla projektu.</span><span class="sxs-lookup"><span data-stu-id="7d897-173">**Save All** for the project.</span></span>

<a id="run"></a>

## <a name="run-the-sample"></a><span data-ttu-id="7d897-174">Uruchom próbki</span><span class="sxs-lookup"><span data-stu-id="7d897-174">Run the Sample</span></span>

1. <span data-ttu-id="7d897-175">Naciśnij klawisz F5, aby uruchomić projekt w trybie debugowania.</span><span class="sxs-lookup"><span data-stu-id="7d897-175">Press F5 to run the project in debug mode.</span></span>
2. <span data-ttu-id="7d897-176">W wierszu adresu przeglądarki, dołącz **/home/rozmów** adres URL domyślnej strony dla projektu.</span><span class="sxs-lookup"><span data-stu-id="7d897-176">In the browser address line, append **/home/chat** to the URL of the default page for the project.</span></span> <span data-ttu-id="7d897-177">Ładuje strony rozmów w wystąpieniu przeglądarki i wyświetla monit o nazwę użytkownika.</span><span class="sxs-lookup"><span data-stu-id="7d897-177">The Chat page loads in a browser instance and prompts for a user name.</span></span>

    ![Wprowadź nazwę użytkownika](tutorial-getting-started-with-signalr-and-mvc/_static/image8.png)
3. <span data-ttu-id="7d897-179">Wprowadź nazwę użytkownika.</span><span class="sxs-lookup"><span data-stu-id="7d897-179">Enter a user name.</span></span>
4. <span data-ttu-id="7d897-180">Skopiuj adres URL w wierszu adresu przeglądarki i używać go otworzyć dwa kolejne wystąpienia przeglądarki.</span><span class="sxs-lookup"><span data-stu-id="7d897-180">Copy the URL from the address line of the browser and use it to open two more browser instances.</span></span> <span data-ttu-id="7d897-181">W każdym wystąpieniu przeglądarki wprowadź unikatową nazwę użytkownika.</span><span class="sxs-lookup"><span data-stu-id="7d897-181">In each browser instance, enter a unique user name.</span></span>
5. <span data-ttu-id="7d897-182">W każdym wystąpieniu przeglądarki dodać komentarz, a następnie kliknij przycisk **wysyłania**.</span><span class="sxs-lookup"><span data-stu-id="7d897-182">In each browser instance, add a comment and click **Send**.</span></span> <span data-ttu-id="7d897-183">Komentarze powinien być wyświetlany we wszystkich wystąpieniach przeglądarki.</span><span class="sxs-lookup"><span data-stu-id="7d897-183">The comments should display in all browser instances.</span></span>

    > [!NOTE]
    > <span data-ttu-id="7d897-184">Tej aplikacji rozmów proste kontekstu dyskusji na serwerze nie są zachowywane.</span><span class="sxs-lookup"><span data-stu-id="7d897-184">This simple chat application does not maintain the discussion context on the server.</span></span> <span data-ttu-id="7d897-185">Koncentrator emituje komentarze do wszystkich bieżących użytkowników.</span><span class="sxs-lookup"><span data-stu-id="7d897-185">The hub broadcasts comments to all current users.</span></span> <span data-ttu-id="7d897-186">Użytkownicy, którzy później dołączyć rozmowę zobaczą wiadomości dodane od czasu dołączenia.</span><span class="sxs-lookup"><span data-stu-id="7d897-186">Users who join the chat later will see messages added from the time they join.</span></span>
6. <span data-ttu-id="7d897-187">Poniższy zrzut ekranu przedstawia aplikacji czatu działającego w przeglądarce.</span><span class="sxs-lookup"><span data-stu-id="7d897-187">The following screen shot shows the chat application running in a browser.</span></span>

    ![Rozmowa przeglądarki](tutorial-getting-started-with-signalr-and-mvc/_static/image9.png)
7. <span data-ttu-id="7d897-189">W **Eksploratora rozwiązań**, sprawdź **dokumentów skryptu** węzła dla działającej aplikacji.</span><span class="sxs-lookup"><span data-stu-id="7d897-189">In **Solution Explorer**, inspect the **Script Documents** node for the running application.</span></span> <span data-ttu-id="7d897-190">Ten węzeł jest widoczny w trybie debugowania, jeśli używasz programu Internet Explorer jako przeglądarki.</span><span class="sxs-lookup"><span data-stu-id="7d897-190">This node is visible in debug mode if you are using Internet Explorer as your browser.</span></span> <span data-ttu-id="7d897-191">Brak pliku skryptu o nazwie **koncentratory** generujący biblioteki SignalR dynamicznie w czasie wykonywania.</span><span class="sxs-lookup"><span data-stu-id="7d897-191">There is a script file named **hubs** that the SignalR library dynamically generates at runtime.</span></span> <span data-ttu-id="7d897-192">Ten plik zarządza komunikacji między skryptu jQuery i kod po stronie serwera.</span><span class="sxs-lookup"><span data-stu-id="7d897-192">This file manages the communication between jQuery script and server-side code.</span></span> <span data-ttu-id="7d897-193">Jeśli używasz przeglądarki innej niż program Internet Explorer, można także przejść do dynamicznej **koncentratory** plików, przechodząc do niego bezpośrednio, na przykład http://mywebsite/signalr/hubs.</span><span class="sxs-lookup"><span data-stu-id="7d897-193">If you use a browser other than Internet Explorer, you can also access the dynamic **hubs** file by browsing to it directly, for example http://mywebsite/signalr/hubs.</span></span>

<a id="code"></a>

## <a name="examine-the-code"></a><span data-ttu-id="7d897-194">Sprawdź kod</span><span class="sxs-lookup"><span data-stu-id="7d897-194">Examine the Code</span></span>

<span data-ttu-id="7d897-195">SignalR aplikacji czatu pokazano dwa podstawowe SignalR zadań związanych z projektowaniem: tworzenie koncentratora jako obiekt główny koordynacji na serwerze i używanie biblioteki jQuery SignalR do wysyłania i odbierania wiadomości.</span><span class="sxs-lookup"><span data-stu-id="7d897-195">The SignalR chat application demonstrates two basic SignalR development tasks: creating a hub as the main coordination object on the server, and using the SignalR jQuery library to send and receive messages.</span></span>

### <a name="signalr-hubs"></a><span data-ttu-id="7d897-196">Koncentratory SignalR</span><span class="sxs-lookup"><span data-stu-id="7d897-196">SignalR Hubs</span></span>

<span data-ttu-id="7d897-197">W przykładowym kodzie **ChatHub** pochodną klasy **Microsoft.AspNet.SignalR.Hub** klasy.</span><span class="sxs-lookup"><span data-stu-id="7d897-197">In the code sample the **ChatHub** class derives from the **Microsoft.AspNet.SignalR.Hub** class.</span></span> <span data-ttu-id="7d897-198">Wyprowadzanie z **Centrum** klasy jest to wygodny sposób utworzyć aplikację SignalR.</span><span class="sxs-lookup"><span data-stu-id="7d897-198">Deriving from the **Hub** class is a useful way to build a SignalR application.</span></span> <span data-ttu-id="7d897-199">Można utworzyć metody publiczne na klasy koncentratora, a następnie uzyskać dostępu do tych metod, wywołując ze skryptów na stronie sieci web.</span><span class="sxs-lookup"><span data-stu-id="7d897-199">You can create public methods on your hub class and then access those methods by calling them from scripts in a web page.</span></span>

<span data-ttu-id="7d897-200">W kodzie rozmów wywołać klientów **ChatHub.Send** metody do wysłania nowej wiadomości.</span><span class="sxs-lookup"><span data-stu-id="7d897-200">In the chat code, clients call the **ChatHub.Send** method to send a new message.</span></span> <span data-ttu-id="7d897-201">Koncentrator z kolei wysyła wiadomość do wszystkich klientów przez wywołanie metody **Clients.All.addNewMessageToPage**.</span><span class="sxs-lookup"><span data-stu-id="7d897-201">The hub in turn sends the message to all clients by calling **Clients.All.addNewMessageToPage**.</span></span>

<span data-ttu-id="7d897-202">**Wysyłania** metody przedstawiono kilka pojęć Centrum:</span><span class="sxs-lookup"><span data-stu-id="7d897-202">The **Send** method demonstrates several hub concepts :</span></span>

- <span data-ttu-id="7d897-203">Metody publiczne należy zadeklarować w koncentratorze tak, aby klienci mogą połączeń telefonicznych z nimi.</span><span class="sxs-lookup"><span data-stu-id="7d897-203">Declare public methods on a hub so that clients can call them.</span></span>
- <span data-ttu-id="7d897-204">Użyj **Microsoft.AspNet.SignalR.Hub.Clients** właściwość, aby dostęp do wszystkich klientów podłączone do tego koncentratora.</span><span class="sxs-lookup"><span data-stu-id="7d897-204">Use the **Microsoft.AspNet.SignalR.Hub.Clients** property to access all clients connected to this hub.</span></span>
- <span data-ttu-id="7d897-205">Wywoływanie funkcji na kliencie (takich jak `addNewMessageToPage` funkcji) aby zaktualizować klientów.</span><span class="sxs-lookup"><span data-stu-id="7d897-205">Call a function on the client (such as the `addNewMessageToPage` function) to update clients.</span></span>

    [!code-csharp[Main](tutorial-getting-started-with-signalr-and-mvc/samples/sample5.cs)]

### <a name="signalr-and-jquery"></a><span data-ttu-id="7d897-206">SignalR i jQuery</span><span class="sxs-lookup"><span data-stu-id="7d897-206">SignalR and jQuery</span></span>

<span data-ttu-id="7d897-207">**Chat.cshtml** widoku plik przykładowy kod przedstawia sposób użycia biblioteki jQuery SignalR do komunikowania się przy użyciu koncentratora SignalR.</span><span class="sxs-lookup"><span data-stu-id="7d897-207">The **Chat.cshtml** view file in the code sample shows how to use the SignalR jQuery library to communicate with a SignalR hub.</span></span> <span data-ttu-id="7d897-208">Podstawowe zadania w kodzie utworzenie odwołania do automatycznie generowanej serwera proxy dla koncentratora, deklarowanie funkcję serwera można wywołać w celu wypychania zawartości do klientów, a następnie uruchomić połączenie do wysyłania komunikatów do koncentratora.</span><span class="sxs-lookup"><span data-stu-id="7d897-208">The essential tasks in the code are creating a reference to the auto-generated proxy for the hub, declaring a function that the server can call to push content to clients, and starting a connection to send messages to the hub.</span></span>

<span data-ttu-id="7d897-209">Poniższy kod deklaruje odwołanie do serwera proxy koncentratora.</span><span class="sxs-lookup"><span data-stu-id="7d897-209">The following code declares a reference to a hub proxy.</span></span>

[!code-javascript[Main](tutorial-getting-started-with-signalr-and-mvc/samples/sample6.js)]

> [!NOTE]
> <span data-ttu-id="7d897-210">W języku JavaScript odwołanie do klasy serwera i jej elementów członkowskich jest w camelcase.</span><span class="sxs-lookup"><span data-stu-id="7d897-210">In JavaScript the reference to the server class and its members is in camel case.</span></span> <span data-ttu-id="7d897-211">Przykładowy kod odwołuje się do języka C# **ChatHub** klasy w języku JavaScript jako **chatHub**.</span><span class="sxs-lookup"><span data-stu-id="7d897-211">The code sample references the C# **ChatHub** class in JavaScript as **chatHub**.</span></span> <span data-ttu-id="7d897-212">Jeśli chcesz odwołać `ChatHub` klasy jQuery z konwencjonalnej Pascal wielkość liter jak w języku C#, Edytuj plik klasy ChatHub.cs.</span><span class="sxs-lookup"><span data-stu-id="7d897-212">If you want to reference the `ChatHub` class in jQuery with conventional Pascal casing as you would in C#, edit the ChatHub.cs class file.</span></span> <span data-ttu-id="7d897-213">Dodaj `using` instrukcji, aby odwołać `Microsoft.AspNet.SignalR.Hubs` przestrzeni nazw.</span><span class="sxs-lookup"><span data-stu-id="7d897-213">Add a `using` statement to reference the `Microsoft.AspNet.SignalR.Hubs` namespace.</span></span> <span data-ttu-id="7d897-214">Następnie dodaj `HubName` atrybutu `ChatHub` klasy, na przykład `[HubName("ChatHub")]`.</span><span class="sxs-lookup"><span data-stu-id="7d897-214">Then add the `HubName` attribute to the `ChatHub` class, for example `[HubName("ChatHub")]`.</span></span> <span data-ttu-id="7d897-215">Na koniec zaktualizuj jQuery odwołania do `ChatHub` klasy.</span><span class="sxs-lookup"><span data-stu-id="7d897-215">Finally, update your jQuery reference to the `ChatHub` class.</span></span>


<span data-ttu-id="7d897-216">Poniższy kod przedstawia sposób tworzenia funkcję wywołania zwrotnego w skrypcie.</span><span class="sxs-lookup"><span data-stu-id="7d897-216">The following code shows how to create a callback function in the script.</span></span> <span data-ttu-id="7d897-217">Klasy koncentratora na serwerze wywołanie tej funkcji, aby wysyłać aktualizacje zawartości do każdego klienta.</span><span class="sxs-lookup"><span data-stu-id="7d897-217">The hub class on the server calls this function to push content updates to each client.</span></span> <span data-ttu-id="7d897-218">Opcjonalnie można wywołać `htmlEncode` funkcja pokazuje sposób HTML kodowanie zawartości komunikatu, przed wyświetleniem go na stronie, aby uniemożliwić uruchomienie skryptu.</span><span class="sxs-lookup"><span data-stu-id="7d897-218">The optional call to the `htmlEncode` function shows a way to HTML encode the message content before displaying it in the page, as a way to prevent script injection.</span></span>

[!code-html[Main](tutorial-getting-started-with-signalr-and-mvc/samples/sample7.html)]

<span data-ttu-id="7d897-219">Poniższy kod przedstawia sposób nawiązać połączenie z koncentratorem.</span><span class="sxs-lookup"><span data-stu-id="7d897-219">The following code shows how to open a connection with the hub.</span></span> <span data-ttu-id="7d897-220">Kod rozpoczyna połączenie, a następnie przekazuje on funkcję obsługi zdarzenia kliknij na **wysyłania** przycisku na stronie rozmów.</span><span class="sxs-lookup"><span data-stu-id="7d897-220">The code starts the connection and then passes it a function to handle the click event on the **Send** button in the Chat page.</span></span>

> [!NOTE]
> <span data-ttu-id="7d897-221">Takie podejście zapewnia, że połączenie zostanie nawiązane, przed wykonaniem programu obsługi zdarzeń.</span><span class="sxs-lookup"><span data-stu-id="7d897-221">This approach ensures that the connection is established before the event handler executes.</span></span>


[!code-javascript[Main](tutorial-getting-started-with-signalr-and-mvc/samples/sample8.js)]

<a id="next"></a>

## <a name="next-steps"></a><span data-ttu-id="7d897-222">Następne kroki</span><span class="sxs-lookup"><span data-stu-id="7d897-222">Next Steps</span></span>

<span data-ttu-id="7d897-223">Wiesz, że SignalR to struktura służąca do tworzenia aplikacji sieci web w czasie rzeczywistym.</span><span class="sxs-lookup"><span data-stu-id="7d897-223">You learned that SignalR is a framework for building real-time web applications.</span></span> <span data-ttu-id="7d897-224">Przedstawiono również kilka zadań związanych z projektowaniem SignalR: jak dodać do aplikacji ASP.NET SignalR, Tworzenie klasy koncentratora oraz sposobu wysyłania i odbierania wiadomości z koncentratora.</span><span class="sxs-lookup"><span data-stu-id="7d897-224">You also learned several SignalR development tasks: how to add SignalR to an ASP.NET application, how to create a hub class, and how to send and receive messages from the hub.</span></span>

<span data-ttu-id="7d897-225">Aby uzyskać wskazówki dotyczące sposobu wdrażania przykładowej aplikacji SignalR na platformie Azure, zobacz [SignalR korzystanie z aplikacji sieci Web w usłudze Azure App Service](../deployment/using-signalr-with-azure-web-sites.md).</span><span class="sxs-lookup"><span data-stu-id="7d897-225">For a walkthrough on how to deploy the sample SignalR application to Azure, see [Using SignalR with Web Apps in Azure App Service](../deployment/using-signalr-with-azure-web-sites.md).</span></span> <span data-ttu-id="7d897-226">Aby uzyskać szczegółowe informacje o sposobie wdrażania projektu sieci web programu Visual Studio do witryny sieci Web systemu Windows Azure, zobacz [tworzenie aplikacji sieci web platformy ASP.NET w usłudze Azure App Service](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-get-started/).</span><span class="sxs-lookup"><span data-stu-id="7d897-226">For detailed information about how to deploy a Visual Studio web project to a Windows Azure Web Site, see [Create an ASP.NET web app in Azure App Service](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-get-started/).</span></span>

<span data-ttu-id="7d897-227">Aby uzyskać bardziej zaawansowane pojęcia rozwój SignalR, odwiedź następującą witrynę SignalR kod źródłowy i zasobów:</span><span class="sxs-lookup"><span data-stu-id="7d897-227">To learn more advanced SignalR developments concepts, visit the following sites for SignalR source code and resources :</span></span>

- [<span data-ttu-id="7d897-228">Projekt SignalR</span><span class="sxs-lookup"><span data-stu-id="7d897-228">SignalR Project</span></span>](http://signalr.net)
- [<span data-ttu-id="7d897-229">SignalR Github i przykłady</span><span class="sxs-lookup"><span data-stu-id="7d897-229">SignalR Github and Samples</span></span>](https://github.com/SignalR/SignalR)
- [<span data-ttu-id="7d897-230">Witryna typu Wiki biblioteki SignalR</span><span class="sxs-lookup"><span data-stu-id="7d897-230">SignalR Wiki</span></span>](https://github.com/SignalR/SignalR/wiki)
