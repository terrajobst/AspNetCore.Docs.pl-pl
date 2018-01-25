---
uid: signalr/overview/getting-started/tutorial-getting-started-with-signalr
title: 'Samouczek: Wprowadzenie do korzystania z SignalR 2 | Dokumentacja firmy Microsoft'
author: pfletcher
description: "Ten samouczek pokazuje, jak utworzyć aplikację do obsługi rozmów w czasie rzeczywistym przy użyciu SignalR. Spowoduje dodanie SignalR do pustą aplikację sieci web ASP.NET i utworzyć pa HTML..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/10/2014
ms.topic: article
ms.assetid: a8b3b778-f009-4369-85c7-e90f9878d8b4
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/getting-started/tutorial-getting-started-with-signalr
msc.type: authoredcontent
ms.openlocfilehash: 8be851f5a2b1cca39f5f8f284ff1c002c486d7e8
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/24/2018
---
<a name="tutorial-getting-started-with-signalr-2"></a><span data-ttu-id="4dfcb-104">Samouczek: Wprowadzenie do korzystania z SignalR 2</span><span class="sxs-lookup"><span data-stu-id="4dfcb-104">Tutorial: Getting Started with SignalR 2</span></span>
====================
<span data-ttu-id="4dfcb-105">przez [Patrick Fletcher](https://github.com/pfletcher)</span><span class="sxs-lookup"><span data-stu-id="4dfcb-105">by [Patrick Fletcher](https://github.com/pfletcher)</span></span>

[<span data-ttu-id="4dfcb-106">Pobieranie ukończone projektu</span><span class="sxs-lookup"><span data-stu-id="4dfcb-106">Download Completed Project</span></span>](http://code.msdn.microsoft.com/SignalR-Getting-Started-b9d18aa9)

> <span data-ttu-id="4dfcb-107">Ten samouczek pokazuje, jak utworzyć aplikację do obsługi rozmów w czasie rzeczywistym przy użyciu SignalR.</span><span class="sxs-lookup"><span data-stu-id="4dfcb-107">This tutorial shows how to use SignalR to create a real-time chat application.</span></span> <span data-ttu-id="4dfcb-108">Spowoduje dodanie SignalR do pustą aplikację sieci web ASP.NET i Utwórz stronę HTML do wysyłania i wyświetlenie komunikatów.</span><span class="sxs-lookup"><span data-stu-id="4dfcb-108">You will add SignalR to an empty ASP.NET web application and create an HTML page to send and display messages.</span></span> 
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="4dfcb-109">Używane w samouczku wersje oprogramowania</span><span class="sxs-lookup"><span data-stu-id="4dfcb-109">Software versions used in the tutorial</span></span>
> 
> 
> - [<span data-ttu-id="4dfcb-110">Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="4dfcb-110">Visual Studio 2013</span></span>](https://www.microsoft.com/visualstudio/eng/2013-downloads)
> - <span data-ttu-id="4dfcb-111">.NET 4.5</span><span class="sxs-lookup"><span data-stu-id="4dfcb-111">.NET 4.5</span></span>
> - <span data-ttu-id="4dfcb-112">SignalR w wersji 2</span><span class="sxs-lookup"><span data-stu-id="4dfcb-112">SignalR version 2</span></span>
>   
> 
> 
> ## <a name="using-visual-studio-2012-with-this-tutorial"></a><span data-ttu-id="4dfcb-113">Z tego samouczka przy użyciu programu Visual Studio 2012</span><span class="sxs-lookup"><span data-stu-id="4dfcb-113">Using Visual Studio 2012 with this tutorial</span></span>
> 
> 
> <span data-ttu-id="4dfcb-114">Aby używać programu Visual Studio 2012 z tego samouczka, wykonaj następujące czynności:</span><span class="sxs-lookup"><span data-stu-id="4dfcb-114">To use Visual Studio 2012 with this tutorial, do the following:</span></span>
> 
> - <span data-ttu-id="4dfcb-115">Aktualizacja Twojego [Menedżera pakietów](http://docs.nuget.org/docs/start-here/installing-nuget) do najnowszej wersji.</span><span class="sxs-lookup"><span data-stu-id="4dfcb-115">Update your [Package Manager](http://docs.nuget.org/docs/start-here/installing-nuget) to the latest version.</span></span>
> - <span data-ttu-id="4dfcb-116">Zainstaluj [sieci Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx).</span><span class="sxs-lookup"><span data-stu-id="4dfcb-116">Install the [Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx).</span></span>
> - <span data-ttu-id="4dfcb-117">Instalator platformy sieci Web, wyszukiwanie i instalowanie **ASP.NET i 2013.1 narzędzia sieci Web dla programu Visual Studio 2012**.</span><span class="sxs-lookup"><span data-stu-id="4dfcb-117">In the Web Platform Installer, search for and install **ASP.NET and Web Tools 2013.1 for Visual Studio 2012**.</span></span> <span data-ttu-id="4dfcb-118">Spowoduje to zainstalowanie szablony programu Visual Studio dla biblioteki SignalR klas takich jak **Centrum**.</span><span class="sxs-lookup"><span data-stu-id="4dfcb-118">This will install Visual Studio templates for SignalR classes such as **Hub**.</span></span>
> - <span data-ttu-id="4dfcb-119">Niektóre szablony (takich jak **klasy początkowej OWIN**) nie są dostępne; w tym przypadku powinien użyć pliku klasy.</span><span class="sxs-lookup"><span data-stu-id="4dfcb-119">Some templates (such as **OWIN Startup Class**) will not be available; for these, use a Class file instead.</span></span>
> 
> 
> ## <a name="tutorial-versions"></a><span data-ttu-id="4dfcb-120">Samouczek wersji</span><span class="sxs-lookup"><span data-stu-id="4dfcb-120">Tutorial versions</span></span>
> 
> <span data-ttu-id="4dfcb-121">Aby uzyskać informacje dotyczące starszych wersji biblioteki signalr, zobacz [starsze wersje biblioteki SignalR](../older-versions/index.md).</span><span class="sxs-lookup"><span data-stu-id="4dfcb-121">For information about earlier versions of SignalR, see [SignalR Older Versions](../older-versions/index.md).</span></span>
> 
> ## <a name="questions-and-comments"></a><span data-ttu-id="4dfcb-122">Pytania i komentarze</span><span class="sxs-lookup"><span data-stu-id="4dfcb-122">Questions and comments</span></span>
> 
> <span data-ttu-id="4dfcb-123">Wystaw opinię na jak zbędne tego samouczka i jakie firma Microsoft może poprawić w komentarze u dołu strony.</span><span class="sxs-lookup"><span data-stu-id="4dfcb-123">Please leave feedback on how you liked this tutorial and what we could improve in the comments at the bottom of the page.</span></span> <span data-ttu-id="4dfcb-124">Jeśli masz pytania, które nie są bezpośrednio związane z tego samouczka możesz zamieścić je do [forum ASP.NET SignalR](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) lub [StackOverflow.com](http://stackoverflow.com/).</span><span class="sxs-lookup"><span data-stu-id="4dfcb-124">If you have questions that are not directly related to the tutorial, you can post them to the [ASP.NET SignalR forum](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) or [StackOverflow.com](http://stackoverflow.com/).</span></span>


## <a name="overview"></a><span data-ttu-id="4dfcb-125">Omówienie</span><span class="sxs-lookup"><span data-stu-id="4dfcb-125">Overview</span></span>

<span data-ttu-id="4dfcb-126">W tym samouczku wprowadzenie programowanie SignalR przez przedstawiający sposób tworzenia aplikacji proste rozmów bazujące na przeglądarce.</span><span class="sxs-lookup"><span data-stu-id="4dfcb-126">This tutorial introduces SignalR development by showing how to build a simple browser-based chat application.</span></span> <span data-ttu-id="4dfcb-127">Zostanie dodać biblioteki SignalR do pusta aplikacja sieci web platformy ASP.NET, Utwórz klasę koncentratora do wysyłania wiadomości do klientów i utworzyć strona HTML, która umożliwia użytkownikom wysyłanie i odbieranie wiadomości.</span><span class="sxs-lookup"><span data-stu-id="4dfcb-127">You will add the SignalR library to an empty ASP.NET web application, create a hub class for sending messages to clients, and create an HTML page that lets users send and receive chat messages.</span></span> <span data-ttu-id="4dfcb-128">Podobne samouczek przedstawia sposób tworzenia aplikacji rozmów w MVC 5 widoku MVC, zobacz [wprowadzenie SignalR 2 i MVC 5](tutorial-getting-started-with-signalr-and-mvc.md).</span><span class="sxs-lookup"><span data-stu-id="4dfcb-128">For a similar tutorial that shows how to create a chat application in MVC 5 using an MVC view, see [Getting Started with SignalR 2 and MVC 5](tutorial-getting-started-with-signalr-and-mvc.md).</span></span>

> [!NOTE]
> <span data-ttu-id="4dfcb-129">Ten samouczek przedstawia sposób tworzenia aplikacji SignalR w wersji 2.</span><span class="sxs-lookup"><span data-stu-id="4dfcb-129">This tutorial demonstrates how to create SignalR applications in version 2.</span></span> <span data-ttu-id="4dfcb-130">Aby uzyskać szczegółowe informacje o zmianach między SignalR 1.x i 2, zobacz [SignalR Uaktualnianie projektów 1.x](../releases/upgrading-signalr-1x-projects-to-20.md) i [programu Visual Studio 2013 informacje o wersji](../../../visual-studio/overview/2013/release-notes.md#TOC13).</span><span class="sxs-lookup"><span data-stu-id="4dfcb-130">For details on changes between SignalR 1.x and 2, see [Upgrading SignalR 1.x Projects](../releases/upgrading-signalr-1x-projects-to-20.md) and [Visual Studio 2013 Release Notes](../../../visual-studio/overview/2013/release-notes.md#TOC13).</span></span>

<span data-ttu-id="4dfcb-131">SignalR to open source biblioteki .NET do tworzenia aplikacji sieci web, które wymagają interakcji z użytkownikiem na żywo lub aktualizacji danych w czasie rzeczywistym.</span><span class="sxs-lookup"><span data-stu-id="4dfcb-131">SignalR is an open-source .NET library for building web applications that require live user interaction or real-time data updates.</span></span> <span data-ttu-id="4dfcb-132">Przykładami społecznościowych aplikacji, wielodostępnym gry, business współpracy i grup dyskusyjnych, pogody lub finansowych aktualizacji aplikacji.</span><span class="sxs-lookup"><span data-stu-id="4dfcb-132">Examples include social applications, multiuser games, business collaboration, and news, weather, or financial update applications.</span></span> <span data-ttu-id="4dfcb-133">Są one często nazywane aplikacji w czasie rzeczywistym.</span><span class="sxs-lookup"><span data-stu-id="4dfcb-133">These are often called real-time applications.</span></span>

<span data-ttu-id="4dfcb-134">SignalR upraszcza proces tworzenia aplikacji w czasie rzeczywistym.</span><span class="sxs-lookup"><span data-stu-id="4dfcb-134">SignalR simplifies the process of building real-time applications.</span></span> <span data-ttu-id="4dfcb-135">Zawiera bibliotekę serwera programu ASP.NET i biblioteki klienckiej JavaScript, aby ułatwić zarządzanie połączeniami klient serwer i wypychanie aktualizacji zawartości do klientów.</span><span class="sxs-lookup"><span data-stu-id="4dfcb-135">It includes an ASP.NET server library and a JavaScript client library to make it easier to manage client-server connections and push content updates to clients.</span></span> <span data-ttu-id="4dfcb-136">Biblioteka SignalR można dodać do istniejącej aplikacji ASP.NET do uzyskania funkcjonalność w czasie rzeczywistym.</span><span class="sxs-lookup"><span data-stu-id="4dfcb-136">You can add the SignalR library to an existing ASP.NET application to gain real-time functionality.</span></span>

<span data-ttu-id="4dfcb-137">Samouczek przedstawia następujące SignalR zadań związanych z projektowaniem:</span><span class="sxs-lookup"><span data-stu-id="4dfcb-137">The tutorial demonstrates the following SignalR development tasks:</span></span>

- <span data-ttu-id="4dfcb-138">Dodawanie biblioteki SignalR do aplikacji sieci web ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="4dfcb-138">Adding the SignalR library to an ASP.NET web application.</span></span>
- <span data-ttu-id="4dfcb-139">Tworzenie klasy koncentratora do dystrybuowania zawartości do klientów.</span><span class="sxs-lookup"><span data-stu-id="4dfcb-139">Creating a hub class to push content to clients.</span></span>
- <span data-ttu-id="4dfcb-140">Tworzenie klasy początkowej OWIN do skonfigurowania aplikacji.</span><span class="sxs-lookup"><span data-stu-id="4dfcb-140">Creating an OWIN startup class to configure the application.</span></span>
- <span data-ttu-id="4dfcb-141">Za pomocą biblioteki jQuery SignalR na stronie sieci web można wysyłać wiadomości i wyświetlać aktualizacje z koncentratora.</span><span class="sxs-lookup"><span data-stu-id="4dfcb-141">Using the SignalR jQuery library in a web page to send messages and display updates from the hub.</span></span>

<span data-ttu-id="4dfcb-142">Poniższy zrzut ekranu przedstawia aplikacji czatu działającego w przeglądarce.</span><span class="sxs-lookup"><span data-stu-id="4dfcb-142">The following screen shot shows the chat application running in a browser.</span></span> <span data-ttu-id="4dfcb-143">Każdy nowy użytkownik można publikować komentarze i wyświetlić komentarze, dodawane po użytkownik nie przyłączy rozmowę.</span><span class="sxs-lookup"><span data-stu-id="4dfcb-143">Each new user can post comments and see comments added after the user joins the chat.</span></span>

![Rozmowa wystąpień](tutorial-getting-started-with-signalr/_static/image1.png)

<span data-ttu-id="4dfcb-145">Sekcje:</span><span class="sxs-lookup"><span data-stu-id="4dfcb-145">Sections:</span></span>

- [<span data-ttu-id="4dfcb-146">Konfigurowanie projektu</span><span class="sxs-lookup"><span data-stu-id="4dfcb-146">Set up the Project</span></span>](#setup)
- [<span data-ttu-id="4dfcb-147">Uruchom próbki</span><span class="sxs-lookup"><span data-stu-id="4dfcb-147">Run the Sample</span></span>](#run)
- [<span data-ttu-id="4dfcb-148">Sprawdź kod</span><span class="sxs-lookup"><span data-stu-id="4dfcb-148">Examine the Code</span></span>](#code)
- [<span data-ttu-id="4dfcb-149">Następne kroki</span><span class="sxs-lookup"><span data-stu-id="4dfcb-149">Next Steps</span></span>](#next)

<a id="setup"></a>

## <a name="set-up-the-project"></a><span data-ttu-id="4dfcb-150">Konfigurowanie projektu</span><span class="sxs-lookup"><span data-stu-id="4dfcb-150">Set up the Project</span></span>

<span data-ttu-id="4dfcb-151">W tej sekcji przedstawiono sposób Użyj programu Visual Studio 2013 i SignalR w wersji 2, aby utworzyć pustą aplikację sieci web ASP.NET, Dodaj SignalR i tworzenie aplikacji czatu.</span><span class="sxs-lookup"><span data-stu-id="4dfcb-151">This section shows how to use Visual Studio 2013 and SignalR version 2 to create an empty ASP.NET web application, add SignalR, and create the chat application.</span></span>

<span data-ttu-id="4dfcb-152">Wymagania wstępne:</span><span class="sxs-lookup"><span data-stu-id="4dfcb-152">Prerequisites:</span></span>

- <span data-ttu-id="4dfcb-153">Visual Studio 2013.</span><span class="sxs-lookup"><span data-stu-id="4dfcb-153">Visual Studio 2013.</span></span> <span data-ttu-id="4dfcb-154">Jeśli nie masz programu Visual Studio, zobacz [pobiera ASP.NET](https://www.asp.net/downloads) uzyskać bezpłatne narzędzie Visual Studio 2013 Express programowanie.</span><span class="sxs-lookup"><span data-stu-id="4dfcb-154">If you do not have Visual Studio, see [ASP.NET Downloads](https://www.asp.net/downloads) to get the free Visual Studio 2013 Express Development Tool.</span></span>

<span data-ttu-id="4dfcb-155">Następujące kroki Użyj programu Visual Studio 2013, aby utworzyć pustą aplikację sieci Web ASP.NET i dodać biblioteki SignalR:</span><span class="sxs-lookup"><span data-stu-id="4dfcb-155">The following steps use Visual Studio 2013 to create an ASP.NET Empty Web Application and add the SignalR library:</span></span>

1. <span data-ttu-id="4dfcb-156">W programie Visual Studio Utwórz aplikację sieci Web platformy ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="4dfcb-156">In Visual Studio, create an ASP.NET Web Application.</span></span>

    ![Tworzenie sieci web](tutorial-getting-started-with-signalr/_static/image2.png)
2. <span data-ttu-id="4dfcb-158">W **nowy projekt ASP.NET** okna, pozostaw **pusty** zaznaczone, a następnie kliknij przycisk **tworzenia projektu**.</span><span class="sxs-lookup"><span data-stu-id="4dfcb-158">In the **New ASP.NET Project** window, leave **Empty** selected and click **Create Project**.</span></span>

    ![Utwórz pusty sieci web](tutorial-getting-started-with-signalr/_static/image3.png)
3. <span data-ttu-id="4dfcb-160">W **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy projekt, wybierz **Dodaj | Klasy koncentratora SignalR (v2)**.</span><span class="sxs-lookup"><span data-stu-id="4dfcb-160">In **Solution Explorer**, right-click the project, select **Add | SignalR Hub Class (v2)**.</span></span> <span data-ttu-id="4dfcb-161">Nazwa klasy **ChatHub.cs** i dodaj go do projektu.</span><span class="sxs-lookup"><span data-stu-id="4dfcb-161">Name the class **ChatHub.cs** and add it to the project.</span></span> <span data-ttu-id="4dfcb-162">Spowoduje to utworzenie **ChatHub** i dodaje do projektu zestawu plików skryptów i odwołania do zestawów, które obsługują SignalR.</span><span class="sxs-lookup"><span data-stu-id="4dfcb-162">This step creates the **ChatHub** class and adds to the project a set of script files and assembly references that support SignalR.</span></span>

    > [!NOTE]
    > <span data-ttu-id="4dfcb-163">Można również dodać SignalR na projekt, otwierając **narzędzia | Menedżer pakietów biblioteki | Konsola Menedżera pakietów** i uruchamiając polecenie:</span><span class="sxs-lookup"><span data-stu-id="4dfcb-163">You can also add SignalR to a project by opening the **Tools | Library Package Manager | Package Manager Console** and running a command:</span></span>

    `install-package Microsoft.AspNet.SignalR`

    <span data-ttu-id="4dfcb-164">Jeśli używasz konsoli można dodać SignalR, należy utworzyć klasę koncentratora SignalR w osobnym kroku po dodaniu SignalR.</span><span class="sxs-lookup"><span data-stu-id="4dfcb-164">If you use the console to add SignalR, create the SignalR hub class as a separate step after you add SignalR.</span></span>

    > [!NOTE]
    > <span data-ttu-id="4dfcb-165">Jeśli używasz programu Visual Studio 2012, **klasy koncentratora SignalR (v2)** szablonu nie będą dostępne.</span><span class="sxs-lookup"><span data-stu-id="4dfcb-165">If you are using Visual Studio 2012, the **SignalR Hub Class (v2)** template will not be available.</span></span> <span data-ttu-id="4dfcb-166">Możesz dodać zwykły **klasy** o nazwie `ChatHub` zamiast tego.</span><span class="sxs-lookup"><span data-stu-id="4dfcb-166">You can add a plain **Class** called `ChatHub` instead.</span></span>
4. <span data-ttu-id="4dfcb-167">W **Eksploratora rozwiązań**, rozwiń węzeł skryptów.</span><span class="sxs-lookup"><span data-stu-id="4dfcb-167">In **Solution Explorer**, expand the Scripts node.</span></span> <span data-ttu-id="4dfcb-168">Biblioteki skryptów, jQuery i SignalR są widoczne w projekcie.</span><span class="sxs-lookup"><span data-stu-id="4dfcb-168">Script libraries for jQuery and SignalR are visible in the project.</span></span>
5. <span data-ttu-id="4dfcb-169">Zastąp kod w nowej **ChatHub** klasy następującym kodem.</span><span class="sxs-lookup"><span data-stu-id="4dfcb-169">Replace the code in the new **ChatHub** class with the following code.</span></span>

    [!code-csharp[Main](tutorial-getting-started-with-signalr/samples/sample1.cs)]
6. <span data-ttu-id="4dfcb-170">W **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy projekt, a następnie kliknij przycisk **Dodaj | Klasy początkowej OWIN**.</span><span class="sxs-lookup"><span data-stu-id="4dfcb-170">In **Solution Explorer**, right-click the project, then click **Add | OWIN Startup Class**.</span></span> <span data-ttu-id="4dfcb-171">Nazwa nowej klasy `Startup` i kliknij przycisk OK.</span><span class="sxs-lookup"><span data-stu-id="4dfcb-171">Name the new class `Startup` and click OK.</span></span>

    > [!NOTE]
    > <span data-ttu-id="4dfcb-172">Jeśli używasz programu Visual Studio 2012, **klasy początkowej OWIN** szablonu nie będą dostępne.</span><span class="sxs-lookup"><span data-stu-id="4dfcb-172">If you are using Visual Studio 2012, the **OWIN Startup Class** template will not be available.</span></span> <span data-ttu-id="4dfcb-173">Możesz dodać zwykły **klasy** o nazwie `Startup` zamiast tego.</span><span class="sxs-lookup"><span data-stu-id="4dfcb-173">You can add a plain **Class** called `Startup` instead.</span></span>
7. <span data-ttu-id="4dfcb-174">Zmiany zawartości nowa klasa uruchamiania do następującego.</span><span class="sxs-lookup"><span data-stu-id="4dfcb-174">Change the contents of the new Startup class to the following.</span></span>

    [!code-csharp[Main](tutorial-getting-started-with-signalr/samples/sample2.cs)]
8. <span data-ttu-id="4dfcb-175">W **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy projekt, a następnie kliknij przycisk **Dodaj | Strona HTML**.</span><span class="sxs-lookup"><span data-stu-id="4dfcb-175">In **Solution Explorer**, right-click the project, then click **Add | HTML Page**.</span></span> <span data-ttu-id="4dfcb-176">Nazwa nowej strony `index.html`.</span><span class="sxs-lookup"><span data-stu-id="4dfcb-176">Name the new page `index.html`.</span></span>
    >[!NOTE]
    ><span data-ttu-id="4dfcb-177">Może być konieczna zmiana numery wersji dla odwołania do biblioteki JQuery i SignalR</span><span class="sxs-lookup"><span data-stu-id="4dfcb-177">You might need to change the version numbers for the references to JQuery and SignalR libraries</span></span>
9. <span data-ttu-id="4dfcb-178">W **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy właśnie utworzony strony HTML i kliknij przycisk **Ustaw jako stronę startową**.</span><span class="sxs-lookup"><span data-stu-id="4dfcb-178">In **Solution Explorer**, right-click the HTML page you just created and click **Set as Start Page**.</span></span>
10. <span data-ttu-id="4dfcb-179">Zastąp następujący kod w kodzie domyślnym na stronie HTML.</span><span class="sxs-lookup"><span data-stu-id="4dfcb-179">Replace the default code in the HTML page with the following code.</span></span>

    > [!NOTE]
    > <span data-ttu-id="4dfcb-180">Może być zainstalowana nowsza wersja skryptów SignalR przez Menedżera pakietów.</span><span class="sxs-lookup"><span data-stu-id="4dfcb-180">A later version of the SignalR scripts may be installed by the package manager.</span></span> <span data-ttu-id="4dfcb-181">Sprawdź, czy poniżej odwołań do skryptów odpowiadają wersji plików skryptów w projekcie (będą różne, jeśli dodano SignalR przy użyciu narzędzia NuGet, a nie dodaje koncentratora.)</span><span class="sxs-lookup"><span data-stu-id="4dfcb-181">Verify that the script references below correspond to the versions of the script files in the project (they will be different if you added SignalR using NuGet rather than adding a hub.)</span></span>

    [!code-html[Main](tutorial-getting-started-with-signalr/samples/sample3.html)]
11. <span data-ttu-id="4dfcb-182">**Zapisz wszystkie** dla projektu.</span><span class="sxs-lookup"><span data-stu-id="4dfcb-182">**Save All** for the project.</span></span>

<a id="run"></a>

## <a name="run-the-sample"></a><span data-ttu-id="4dfcb-183">Uruchom próbki</span><span class="sxs-lookup"><span data-stu-id="4dfcb-183">Run the Sample</span></span>

1. <span data-ttu-id="4dfcb-184">Naciśnij klawisz F5, aby uruchomić projekt w trybie debugowania.</span><span class="sxs-lookup"><span data-stu-id="4dfcb-184">Press F5 to run the project in debug mode.</span></span> <span data-ttu-id="4dfcb-185">Ładuje strony HTML w wystąpieniu przeglądarki i wyświetla monit o nazwę użytkownika.</span><span class="sxs-lookup"><span data-stu-id="4dfcb-185">The HTML page loads in a browser instance and prompts for a user name.</span></span>

    ![Wprowadź nazwę użytkownika](tutorial-getting-started-with-signalr/_static/image4.png)
2. <span data-ttu-id="4dfcb-187">Wprowadź nazwę użytkownika.</span><span class="sxs-lookup"><span data-stu-id="4dfcb-187">Enter a user name.</span></span>
3. <span data-ttu-id="4dfcb-188">Skopiuj adres URL w wierszu adresu przeglądarki i używać go otworzyć dwa kolejne wystąpienia przeglądarki.</span><span class="sxs-lookup"><span data-stu-id="4dfcb-188">Copy the URL from the address line of the browser and use it to open two more browser instances.</span></span> <span data-ttu-id="4dfcb-189">W każdym wystąpieniu przeglądarki wprowadź unikatową nazwę użytkownika.</span><span class="sxs-lookup"><span data-stu-id="4dfcb-189">In each browser instance, enter a unique user name.</span></span>
4. <span data-ttu-id="4dfcb-190">W każdym wystąpieniu przeglądarki dodać komentarz, a następnie kliknij przycisk **wysyłania**.</span><span class="sxs-lookup"><span data-stu-id="4dfcb-190">In each browser instance, add a comment and click **Send**.</span></span> <span data-ttu-id="4dfcb-191">Komentarze powinien być wyświetlany we wszystkich wystąpieniach przeglądarki.</span><span class="sxs-lookup"><span data-stu-id="4dfcb-191">The comments should display in all browser instances.</span></span>

    > [!NOTE]
    > <span data-ttu-id="4dfcb-192">Tej aplikacji rozmów proste kontekstu dyskusji na serwerze nie są zachowywane.</span><span class="sxs-lookup"><span data-stu-id="4dfcb-192">This simple chat application does not maintain the discussion context on the server.</span></span> <span data-ttu-id="4dfcb-193">Koncentrator emituje komentarze do wszystkich bieżących użytkowników.</span><span class="sxs-lookup"><span data-stu-id="4dfcb-193">The hub broadcasts comments to all current users.</span></span> <span data-ttu-id="4dfcb-194">Użytkownicy, którzy później dołączyć rozmowę zobaczą wiadomości dodane od czasu dołączenia.</span><span class="sxs-lookup"><span data-stu-id="4dfcb-194">Users who join the chat later will see messages added from the time they join.</span></span>

    <span data-ttu-id="4dfcb-195">Poniższy zrzut ekranu przedstawia rozmów aplikacja była uruchomiona w trzech przypadkach przeglądarki, które są aktualizowane w jedno wystąpienie wysyła komunikat:</span><span class="sxs-lookup"><span data-stu-id="4dfcb-195">The following screen shot shows the chat application running in three browser instances, all of which are updated when one instance sends a message:</span></span>

    ![Rozmowa przeglądarki](tutorial-getting-started-with-signalr/_static/image5.png)
5. <span data-ttu-id="4dfcb-197">W **Eksploratora rozwiązań**, sprawdź **dokumentów skryptu** węzła dla działającej aplikacji.</span><span class="sxs-lookup"><span data-stu-id="4dfcb-197">In **Solution Explorer**, inspect the **Script Documents** node for the running application.</span></span> <span data-ttu-id="4dfcb-198">Brak pliku skryptu o nazwie **koncentratory** generujący biblioteki SignalR dynamicznie w czasie wykonywania.</span><span class="sxs-lookup"><span data-stu-id="4dfcb-198">There is a script file named **hubs** that the SignalR library dynamically generates at runtime.</span></span> <span data-ttu-id="4dfcb-199">Ten plik zarządza komunikacji między skryptu jQuery i kod po stronie serwera.</span><span class="sxs-lookup"><span data-stu-id="4dfcb-199">This file manages the communication between jQuery script and server-side code.</span></span>

    ![](tutorial-getting-started-with-signalr/_static/image6.png)

<a id="code"></a>

## <a name="examine-the-code"></a><span data-ttu-id="4dfcb-200">Sprawdź kod</span><span class="sxs-lookup"><span data-stu-id="4dfcb-200">Examine the Code</span></span>

<span data-ttu-id="4dfcb-201">SignalR aplikacji czatu pokazano dwa podstawowe SignalR zadań związanych z projektowaniem: tworzenie koncentratora jako obiekt główny koordynacji na serwerze i używanie biblioteki jQuery SignalR do wysyłania i odbierania wiadomości.</span><span class="sxs-lookup"><span data-stu-id="4dfcb-201">The SignalR chat application demonstrates two basic SignalR development tasks: creating a hub as the main coordination object on the server, and using the SignalR jQuery library to send and receive messages.</span></span>

### <a name="signalr-hubs"></a><span data-ttu-id="4dfcb-202">Koncentratory SignalR</span><span class="sxs-lookup"><span data-stu-id="4dfcb-202">SignalR Hubs</span></span>

<span data-ttu-id="4dfcb-203">W przykładowym kodzie **ChatHub** pochodną klasy **Microsoft.AspNet.SignalR.Hub** klasy.</span><span class="sxs-lookup"><span data-stu-id="4dfcb-203">In the code sample the **ChatHub** class derives from the **Microsoft.AspNet.SignalR.Hub** class.</span></span> <span data-ttu-id="4dfcb-204">Wyprowadzanie z **Centrum** klasy jest to wygodny sposób utworzyć aplikację SignalR.</span><span class="sxs-lookup"><span data-stu-id="4dfcb-204">Deriving from the **Hub** class is a useful way to build a SignalR application.</span></span> <span data-ttu-id="4dfcb-205">Można utworzyć metody publiczne na klasy koncentratora, a następnie uzyskać dostępu do tych metod, wywołując ze skryptów na stronie sieci web.</span><span class="sxs-lookup"><span data-stu-id="4dfcb-205">You can create public methods on your hub class and then access those methods by calling them from scripts in a web page.</span></span>

<span data-ttu-id="4dfcb-206">W kodzie rozmów wywołać klientów **ChatHub.Send** metody do wysłania nowej wiadomości.</span><span class="sxs-lookup"><span data-stu-id="4dfcb-206">In the chat code, clients call the **ChatHub.Send** method to send a new message.</span></span> <span data-ttu-id="4dfcb-207">Koncentrator z kolei wysyła wiadomość do wszystkich klientów przez wywołanie metody **Clients.All.broadcastMessage**.</span><span class="sxs-lookup"><span data-stu-id="4dfcb-207">The hub in turn sends the message to all clients by calling **Clients.All.broadcastMessage**.</span></span>

<span data-ttu-id="4dfcb-208">**Wysyłania** metody przedstawiono kilka pojęć Centrum:</span><span class="sxs-lookup"><span data-stu-id="4dfcb-208">The **Send** method demonstrates several hub concepts :</span></span>

- <span data-ttu-id="4dfcb-209">Metody publiczne należy zadeklarować w koncentratorze tak, aby klienci mogą połączeń telefonicznych z nimi.</span><span class="sxs-lookup"><span data-stu-id="4dfcb-209">Declare public methods on a hub so that clients can call them.</span></span>
- <span data-ttu-id="4dfcb-210">Użyj **Microsoft.AspNet.SignalR.Hub.Clients** właściwość dynamiczna ma dostęp do wszystkich klientów podłączone do tego koncentratora.</span><span class="sxs-lookup"><span data-stu-id="4dfcb-210">Use the **Microsoft.AspNet.SignalR.Hub.Clients** dynamic property to access all clients connected to this hub.</span></span>
- <span data-ttu-id="4dfcb-211">Wywoływanie funkcji na kliencie (takich jak `broadcastMessage` funkcji) aby zaktualizować klientów.</span><span class="sxs-lookup"><span data-stu-id="4dfcb-211">Call a function on the client (such as the `broadcastMessage` function) to update clients.</span></span>

    [!code-csharp[Main](tutorial-getting-started-with-signalr/samples/sample4.cs)]

### <a name="signalr-and-jquery"></a><span data-ttu-id="4dfcb-212">SignalR i jQuery</span><span class="sxs-lookup"><span data-stu-id="4dfcb-212">SignalR and jQuery</span></span>

<span data-ttu-id="4dfcb-213">Strony HTML w przykładowy kod przedstawia sposób użycia biblioteki jQuery SignalR do komunikowania się przy użyciu koncentratora SignalR.</span><span class="sxs-lookup"><span data-stu-id="4dfcb-213">The HTML page in the code sample shows how to use the SignalR jQuery library to communicate with a SignalR hub.</span></span> <span data-ttu-id="4dfcb-214">Podstawowe zadania w kodzie są deklarowanie serwer proxy, aby odwołać koncentratora, deklarowanie funkcję serwera można wywołać w celu wypychania zawartości do klientów, a następnie uruchomić połączenie do wysłania wiadomości do koncentratora.</span><span class="sxs-lookup"><span data-stu-id="4dfcb-214">The essential tasks in the code are declaring a proxy to reference the hub, declaring a function that the server can call to push content to clients, and starting a connection to send messages to the hub.</span></span>

<span data-ttu-id="4dfcb-215">Poniższy kod deklaruje odwołanie do serwera proxy koncentratora.</span><span class="sxs-lookup"><span data-stu-id="4dfcb-215">The following code declares a reference to a hub proxy.</span></span>

[!code-javascript[Main](tutorial-getting-started-with-signalr/samples/sample5.js)]

> [!NOTE]
> <span data-ttu-id="4dfcb-216">W języku JavaScript odwołanie do klasy serwera i jej elementów członkowskich jest w camelcase.</span><span class="sxs-lookup"><span data-stu-id="4dfcb-216">In JavaScript the reference to the server class and its members is in camel case.</span></span> <span data-ttu-id="4dfcb-217">Przykładowy kod odwołuje się do języka C# **ChatHub** klasy w języku JavaScript jako **chatHub**.</span><span class="sxs-lookup"><span data-stu-id="4dfcb-217">The code sample references the C# **ChatHub** class in JavaScript as **chatHub**.</span></span>


<span data-ttu-id="4dfcb-218">Następujący kod jest, jak utworzyć funkcję wywołania zwrotnego w skrypcie.</span><span class="sxs-lookup"><span data-stu-id="4dfcb-218">The following code is how you create a callback function in the script.</span></span> <span data-ttu-id="4dfcb-219">Klasy koncentratora na serwerze wywołanie tej funkcji, aby wysyłać aktualizacje zawartości do każdego klienta.</span><span class="sxs-lookup"><span data-stu-id="4dfcb-219">The hub class on the server calls this function to push content updates to each client.</span></span> <span data-ttu-id="4dfcb-220">Dwa wiersze, czy kodowanie HTML zawartości przed wyświetleniem go są opcjonalne i Pokaż prosty sposób, aby uniemożliwić uruchomienie skryptu.</span><span class="sxs-lookup"><span data-stu-id="4dfcb-220">The two lines that HTML encode the content before displaying it are optional and show a simple way to prevent script injection.</span></span>

[!code-html[Main](tutorial-getting-started-with-signalr/samples/sample6.html)]

<span data-ttu-id="4dfcb-221">Poniższy kod przedstawia sposób nawiązać połączenie z koncentratorem.</span><span class="sxs-lookup"><span data-stu-id="4dfcb-221">The following code shows how to open a connection with the hub.</span></span> <span data-ttu-id="4dfcb-222">Kod rozpoczyna połączenie, a następnie przekazuje on funkcję obsługi zdarzenia kliknij na **wysyłania** przycisku na stronie HTML.</span><span class="sxs-lookup"><span data-stu-id="4dfcb-222">The code starts the connection and then passes it a function to handle the click event on the **Send** button in the HTML page.</span></span>

> [!NOTE]
> <span data-ttu-id="4dfcb-223">Takie podejście zapewnia, że połączenie zostanie nawiązane, przed wykonaniem programu obsługi zdarzeń.</span><span class="sxs-lookup"><span data-stu-id="4dfcb-223">This approach ensures that the connection is established before the event handler executes.</span></span>


[!code-javascript[Main](tutorial-getting-started-with-signalr/samples/sample7.js)]

<a id="next"></a>

## <a name="next-steps"></a><span data-ttu-id="4dfcb-224">Następne kroki</span><span class="sxs-lookup"><span data-stu-id="4dfcb-224">Next Steps</span></span>

<span data-ttu-id="4dfcb-225">Wiesz, że SignalR to struktura służąca do tworzenia aplikacji sieci web w czasie rzeczywistym.</span><span class="sxs-lookup"><span data-stu-id="4dfcb-225">You learned that SignalR is a framework for building real-time web applications.</span></span> <span data-ttu-id="4dfcb-226">Przedstawiono również kilka zadań związanych z projektowaniem SignalR: jak dodać do aplikacji ASP.NET SignalR, Tworzenie klasy koncentratora oraz sposobu wysyłania i odbierania wiadomości z koncentratora.</span><span class="sxs-lookup"><span data-stu-id="4dfcb-226">You also learned several SignalR development tasks: how to add SignalR to an ASP.NET application, how to create a hub class, and how to send and receive messages from the hub.</span></span>

<span data-ttu-id="4dfcb-227">Aby uzyskać wskazówki dotyczące sposobu wdrażania przykładowej aplikacji SignalR na platformie Azure, zobacz [SignalR korzystanie z aplikacji sieci Web w usłudze Azure App Service](../deployment/using-signalr-with-azure-web-sites.md).</span><span class="sxs-lookup"><span data-stu-id="4dfcb-227">For a walkthrough on how to deploy the sample SignalR application to Azure, see [Using SignalR with Web Apps in Azure App Service](../deployment/using-signalr-with-azure-web-sites.md).</span></span> <span data-ttu-id="4dfcb-228">Aby uzyskać szczegółowe informacje o sposobie wdrażania projektu sieci web programu Visual Studio do witryny sieci Web systemu Windows Azure, zobacz [tworzenie aplikacji sieci web platformy ASP.NET w usłudze Azure App Service](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-get-started/).</span><span class="sxs-lookup"><span data-stu-id="4dfcb-228">For detailed information about how to deploy a Visual Studio web project to a Windows Azure Web Site, see [Create an ASP.NET web app in Azure App Service](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-get-started/).</span></span>

<span data-ttu-id="4dfcb-229">Aby uzyskać bardziej zaawansowane pojęcia rozwój SignalR, odwiedź następującą witrynę SignalR kod źródłowy i zasobów:</span><span class="sxs-lookup"><span data-stu-id="4dfcb-229">To learn more advanced SignalR developments concepts, visit the following sites for SignalR source code and resources:</span></span>

- [<span data-ttu-id="4dfcb-230">Projekt SignalR</span><span class="sxs-lookup"><span data-stu-id="4dfcb-230">SignalR Project</span></span>](http://signalr.net)
- [<span data-ttu-id="4dfcb-231">SignalR Github i przykłady</span><span class="sxs-lookup"><span data-stu-id="4dfcb-231">SignalR Github and Samples</span></span>](https://github.com/SignalR/SignalR)
- [<span data-ttu-id="4dfcb-232">Witryna typu Wiki biblioteki SignalR</span><span class="sxs-lookup"><span data-stu-id="4dfcb-232">SignalR Wiki</span></span>](https://github.com/SignalR/SignalR/wiki)
