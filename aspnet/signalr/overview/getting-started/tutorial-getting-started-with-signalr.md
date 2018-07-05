---
uid: signalr/overview/getting-started/tutorial-getting-started-with-signalr
title: 'Samouczek: Wprowadzenie do SignalR 2 | Dokumentacja firmy Microsoft'
author: pfletcher
description: Ten samouczek pokazuje, jak utworzyć aplikację do obsługi rozmów w czasie rzeczywistym przy użyciu SignalR. Będzie dodać SignalR do pustych aplikacji sieci web ASP.NET i utworzyć pa HTML...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/10/2014
ms.topic: article
ms.assetid: a8b3b778-f009-4369-85c7-e90f9878d8b4
ms.technology: dotnet-signalr
msc.legacyurl: /signalr/overview/getting-started/tutorial-getting-started-with-signalr
msc.type: authoredcontent
ms.openlocfilehash: fcd00419de77a380e004cbe306eb46910655a355
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/03/2018
ms.locfileid: "37398187"
---
<a name="tutorial-getting-started-with-signalr-2"></a><span data-ttu-id="2bddd-104">Samouczek: Wprowadzenie do SignalR 2</span><span class="sxs-lookup"><span data-stu-id="2bddd-104">Tutorial: Getting Started with SignalR 2</span></span>
====================
<span data-ttu-id="2bddd-105">przez [Patrick Fletcher](https://github.com/pfletcher)</span><span class="sxs-lookup"><span data-stu-id="2bddd-105">by [Patrick Fletcher](https://github.com/pfletcher)</span></span>

[<span data-ttu-id="2bddd-106">Pobieranie ukończone projektu</span><span class="sxs-lookup"><span data-stu-id="2bddd-106">Download Completed Project</span></span>](http://code.msdn.microsoft.com/SignalR-Getting-Started-b9d18aa9)

> <span data-ttu-id="2bddd-107">Ten samouczek pokazuje, jak utworzyć aplikację do obsługi rozmów w czasie rzeczywistym przy użyciu SignalR.</span><span class="sxs-lookup"><span data-stu-id="2bddd-107">This tutorial shows how to use SignalR to create a real-time chat application.</span></span> <span data-ttu-id="2bddd-108">Będzie dodać SignalR do pustych aplikacji sieci web ASP.NET i Utwórz stronę HTML do wysyłania i wyświetla komunikaty.</span><span class="sxs-lookup"><span data-stu-id="2bddd-108">You will add SignalR to an empty ASP.NET web application and create an HTML page to send and display messages.</span></span> 
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="2bddd-109">Wersje oprogramowania używanego w tym samouczku</span><span class="sxs-lookup"><span data-stu-id="2bddd-109">Software versions used in the tutorial</span></span>
> 
> 
> - [<span data-ttu-id="2bddd-110">Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="2bddd-110">Visual Studio 2013</span></span>](https://www.microsoft.com/visualstudio/eng/2013-downloads)
> - <span data-ttu-id="2bddd-111">.NET 4.5</span><span class="sxs-lookup"><span data-stu-id="2bddd-111">.NET 4.5</span></span>
> - <span data-ttu-id="2bddd-112">SignalR w wersji 2</span><span class="sxs-lookup"><span data-stu-id="2bddd-112">SignalR version 2</span></span>
>   
> 
> 
> ## <a name="using-visual-studio-2012-with-this-tutorial"></a><span data-ttu-id="2bddd-113">Z tego samouczka przy użyciu programu Visual Studio 2012</span><span class="sxs-lookup"><span data-stu-id="2bddd-113">Using Visual Studio 2012 with this tutorial</span></span>
> 
> 
> <span data-ttu-id="2bddd-114">Aby użyć programu Visual Studio 2012 za pomocą tego samouczka, wykonaj następujące czynności:</span><span class="sxs-lookup"><span data-stu-id="2bddd-114">To use Visual Studio 2012 with this tutorial, do the following:</span></span>
> 
> - <span data-ttu-id="2bddd-115">Aktualizacja usługi [Menedżera pakietów](http://docs.nuget.org/docs/start-here/installing-nuget) do najnowszej wersji.</span><span class="sxs-lookup"><span data-stu-id="2bddd-115">Update your [Package Manager](http://docs.nuget.org/docs/start-here/installing-nuget) to the latest version.</span></span>
> - <span data-ttu-id="2bddd-116">Zainstaluj [Instalator platformy sieci Web](https://www.microsoft.com/web/downloads/platform.aspx).</span><span class="sxs-lookup"><span data-stu-id="2bddd-116">Install the [Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx).</span></span>
> - <span data-ttu-id="2bddd-117">Instalator platformy sieci Web, wyszukiwanie i instalowanie **platformy ASP.NET i Web Tools 2013.1 dla programu Visual Studio 2012**.</span><span class="sxs-lookup"><span data-stu-id="2bddd-117">In the Web Platform Installer, search for and install **ASP.NET and Web Tools 2013.1 for Visual Studio 2012**.</span></span> <span data-ttu-id="2bddd-118">Szablony programu Visual Studio dla klas SignalR spowoduje to zainstalowanie takich jak **Centrum**.</span><span class="sxs-lookup"><span data-stu-id="2bddd-118">This will install Visual Studio templates for SignalR classes such as **Hub**.</span></span>
> - <span data-ttu-id="2bddd-119">Niektóre szablony (takie jak **klasy początkowej OWIN**) nie są dostępne; w tym przypadku użyj pliku klasy.</span><span class="sxs-lookup"><span data-stu-id="2bddd-119">Some templates (such as **OWIN Startup Class**) will not be available; for these, use a Class file instead.</span></span>
> 
> 
> ## <a name="tutorial-versions"></a><span data-ttu-id="2bddd-120">Samouczek wersji</span><span class="sxs-lookup"><span data-stu-id="2bddd-120">Tutorial versions</span></span>
> 
> <span data-ttu-id="2bddd-121">Aby uzyskać informacje dotyczące starszych wersji biblioteki SignalR, zobacz [starsze wersje biblioteki SignalR](../older-versions/index.md).</span><span class="sxs-lookup"><span data-stu-id="2bddd-121">For information about earlier versions of SignalR, see [SignalR Older Versions](../older-versions/index.md).</span></span>
> 
> ## <a name="questions-and-comments"></a><span data-ttu-id="2bddd-122">Pytania i komentarze</span><span class="sxs-lookup"><span data-stu-id="2bddd-122">Questions and comments</span></span>
> 
> <span data-ttu-id="2bddd-123">Jak się podoba w tym samouczku, i co można było ulepszyć proces w komentarzach u dołu strony, wystaw opinię.</span><span class="sxs-lookup"><span data-stu-id="2bddd-123">Please leave feedback on how you liked this tutorial and what we could improve in the comments at the bottom of the page.</span></span> <span data-ttu-id="2bddd-124">Jeśli masz pytania, na które nie są bezpośrednio związane z tego samouczka, możesz zamieścić je do [forum ASP.NET SignalR](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) lub [StackOverflow.com](http://stackoverflow.com/).</span><span class="sxs-lookup"><span data-stu-id="2bddd-124">If you have questions that are not directly related to the tutorial, you can post them to the [ASP.NET SignalR forum](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) or [StackOverflow.com](http://stackoverflow.com/).</span></span>


## <a name="overview"></a><span data-ttu-id="2bddd-125">Omówienie</span><span class="sxs-lookup"><span data-stu-id="2bddd-125">Overview</span></span>

<span data-ttu-id="2bddd-126">W tym samouczku przedstawiono rozwoju SignalR, pokazując, jak utworzyć aplikację proste rozmowy opartej na przeglądarce.</span><span class="sxs-lookup"><span data-stu-id="2bddd-126">This tutorial introduces SignalR development by showing how to build a simple browser-based chat application.</span></span> <span data-ttu-id="2bddd-127">Będzie dodać biblioteki SignalR do pustą aplikację sieci web platformy ASP.NET, Utwórz klasę hub wysyłanie komunikatów do klientów i Utwórz stronę HTML, który umożliwia użytkownikom wysyłanie i odbieranie wiadomości rozmowy.</span><span class="sxs-lookup"><span data-stu-id="2bddd-127">You will add the SignalR library to an empty ASP.NET web application, create a hub class for sending messages to clients, and create an HTML page that lets users send and receive chat messages.</span></span> <span data-ttu-id="2bddd-128">Z podobnego samouczka dotyczącego pokazującym, jak utworzyć aplikację do obsługi rozmów w MVC 5 przy użyciu widoku MVC, zobacz [rozpoczęcie korzystania z SignalR 2 i MVC 5](tutorial-getting-started-with-signalr-and-mvc.md).</span><span class="sxs-lookup"><span data-stu-id="2bddd-128">For a similar tutorial that shows how to create a chat application in MVC 5 using an MVC view, see [Getting Started with SignalR 2 and MVC 5](tutorial-getting-started-with-signalr-and-mvc.md).</span></span>

> [!NOTE]
> <span data-ttu-id="2bddd-129">Ten samouczek przedstawia sposób tworzenia aplikacji SignalR w wersji 2.</span><span class="sxs-lookup"><span data-stu-id="2bddd-129">This tutorial demonstrates how to create SignalR applications in version 2.</span></span> <span data-ttu-id="2bddd-130">Aby uzyskać szczegółowe informacje na temat zmian między SignalR 1.x i 2, zobacz [projektów uaktualnianie SignalR 1.x](../releases/upgrading-signalr-1x-projects-to-20.md) i [Visual Studio 2013 Release Notes](../../../visual-studio/overview/2013/release-notes.md#TOC13).</span><span class="sxs-lookup"><span data-stu-id="2bddd-130">For details on changes between SignalR 1.x and 2, see [Upgrading SignalR 1.x Projects](../releases/upgrading-signalr-1x-projects-to-20.md) and [Visual Studio 2013 Release Notes](../../../visual-studio/overview/2013/release-notes.md#TOC13).</span></span>

<span data-ttu-id="2bddd-131">SignalR to biblioteki .NET typu open source do tworzenia aplikacji internetowych, które wymagają interakcji z użytkownikiem na żywo lub aktualizacji danych w czasie rzeczywistym.</span><span class="sxs-lookup"><span data-stu-id="2bddd-131">SignalR is an open-source .NET library for building web applications that require live user interaction or real-time data updates.</span></span> <span data-ttu-id="2bddd-132">Przykłady obejmują społecznościowe aplikacje, gry wielodostępnym, biznesowych współpracę i w nowościach, pogody lub finansowych aktualizacji aplikacji.</span><span class="sxs-lookup"><span data-stu-id="2bddd-132">Examples include social applications, multiuser games, business collaboration, and news, weather, or financial update applications.</span></span> <span data-ttu-id="2bddd-133">Są one często nazywane aplikacji w czasie rzeczywistym.</span><span class="sxs-lookup"><span data-stu-id="2bddd-133">These are often called real-time applications.</span></span>

<span data-ttu-id="2bddd-134">SignalR upraszcza proces tworzenia aplikacji w czasie rzeczywistym.</span><span class="sxs-lookup"><span data-stu-id="2bddd-134">SignalR simplifies the process of building real-time applications.</span></span> <span data-ttu-id="2bddd-135">Obejmuje ona bibliotekę serwera programu ASP.NET oraz bibliotekę kliencką JavaScript, aby ułatwić zarządzanie połączeniami klient serwer i wypychanie aktualizacji zawartości dla klientów.</span><span class="sxs-lookup"><span data-stu-id="2bddd-135">It includes an ASP.NET server library and a JavaScript client library to make it easier to manage client-server connections and push content updates to clients.</span></span> <span data-ttu-id="2bddd-136">Biblioteki SignalR można dodać do istniejącej aplikacji ASP.NET do uzyskania funkcji w czasie rzeczywistym.</span><span class="sxs-lookup"><span data-stu-id="2bddd-136">You can add the SignalR library to an existing ASP.NET application to gain real-time functionality.</span></span>

<span data-ttu-id="2bddd-137">Samouczek przedstawia następujące zadania deweloperskie SignalR:</span><span class="sxs-lookup"><span data-stu-id="2bddd-137">The tutorial demonstrates the following SignalR development tasks:</span></span>

- <span data-ttu-id="2bddd-138">Dodawanie biblioteki SignalR do aplikacji sieci web ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="2bddd-138">Adding the SignalR library to an ASP.NET web application.</span></span>
- <span data-ttu-id="2bddd-139">Tworzenie klasy koncentratora, aby wypchnąć zawartości do klientów.</span><span class="sxs-lookup"><span data-stu-id="2bddd-139">Creating a hub class to push content to clients.</span></span>
- <span data-ttu-id="2bddd-140">Tworzenie klasy początkowej OWIN do konfigurowania aplikacji.</span><span class="sxs-lookup"><span data-stu-id="2bddd-140">Creating an OWIN startup class to configure the application.</span></span>
- <span data-ttu-id="2bddd-141">Przy użyciu biblioteki jQuery SignalR na stronie sieci web umożliwiają przesyłanie komunikatów oraz wyświetlania aktualizacji z koncentratora.</span><span class="sxs-lookup"><span data-stu-id="2bddd-141">Using the SignalR jQuery library in a web page to send messages and display updates from the hub.</span></span>

<span data-ttu-id="2bddd-142">Poniższy zrzut ekranu przedstawia aplikacji rozmów w przeglądarce.</span><span class="sxs-lookup"><span data-stu-id="2bddd-142">The following screen shot shows the chat application running in a browser.</span></span> <span data-ttu-id="2bddd-143">Każdy nowy użytkownik może publikować komentarze i zobacz komentarze dodane po użytkownik nie przyłączy czatu.</span><span class="sxs-lookup"><span data-stu-id="2bddd-143">Each new user can post comments and see comments added after the user joins the chat.</span></span>

![Wystąpienia rozmowy](tutorial-getting-started-with-signalr/_static/image1.png)

<span data-ttu-id="2bddd-145">Sekcje:</span><span class="sxs-lookup"><span data-stu-id="2bddd-145">Sections:</span></span>

- [<span data-ttu-id="2bddd-146">Konfigurowanie projektu</span><span class="sxs-lookup"><span data-stu-id="2bddd-146">Set up the Project</span></span>](#setup)
- [<span data-ttu-id="2bddd-147">Uruchamianie aplikacji przykładowej</span><span class="sxs-lookup"><span data-stu-id="2bddd-147">Run the Sample</span></span>](#run)
- [<span data-ttu-id="2bddd-148">Poszukaj w kodzie</span><span class="sxs-lookup"><span data-stu-id="2bddd-148">Examine the Code</span></span>](#code)
- [<span data-ttu-id="2bddd-149">Następne kroki</span><span class="sxs-lookup"><span data-stu-id="2bddd-149">Next Steps</span></span>](#next)

<a id="setup"></a>

## <a name="set-up-the-project"></a><span data-ttu-id="2bddd-150">Konfigurowanie projektu</span><span class="sxs-lookup"><span data-stu-id="2bddd-150">Set up the Project</span></span>

<span data-ttu-id="2bddd-151">W tej sekcji pokazano, jak utworzyć pustą aplikację sieci web platformy ASP.NET, za pomocą programu Visual Studio 2013 i SignalR w wersji 2 Dodaj SignalR i tworzenie aplikacji czatu.</span><span class="sxs-lookup"><span data-stu-id="2bddd-151">This section shows how to use Visual Studio 2013 and SignalR version 2 to create an empty ASP.NET web application, add SignalR, and create the chat application.</span></span>

<span data-ttu-id="2bddd-152">Wymagania wstępne:</span><span class="sxs-lookup"><span data-stu-id="2bddd-152">Prerequisites:</span></span>

- <span data-ttu-id="2bddd-153">Visual Studio 2013.</span><span class="sxs-lookup"><span data-stu-id="2bddd-153">Visual Studio 2013.</span></span> <span data-ttu-id="2bddd-154">Jeśli nie masz programu Visual Studio, zobacz [ASP.NET pliki do pobrania](https://www.asp.net/downloads) można pobrać bezpłatny program Visual Studio 2013 Express narzędzia programistyczne.</span><span class="sxs-lookup"><span data-stu-id="2bddd-154">If you do not have Visual Studio, see [ASP.NET Downloads](https://www.asp.net/downloads) to get the free Visual Studio 2013 Express Development Tool.</span></span>

<span data-ttu-id="2bddd-155">Poniższe kroki Użyj programu Visual Studio 2013, aby utworzyć pustą aplikację sieci Web platformy ASP.NET i dodać biblioteki SignalR:</span><span class="sxs-lookup"><span data-stu-id="2bddd-155">The following steps use Visual Studio 2013 to create an ASP.NET Empty Web Application and add the SignalR library:</span></span>

1. <span data-ttu-id="2bddd-156">W programie Visual Studio należy utworzyć aplikację sieci Web platformy ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="2bddd-156">In Visual Studio, create an ASP.NET Web Application.</span></span>

    ![Tworzenie sieci web](tutorial-getting-started-with-signalr/_static/image2.png)
2. <span data-ttu-id="2bddd-158">W **nowy projekt ASP.NET** okna, pozostaw **pusty** zaznaczone, a następnie kliknij przycisk **Tworzenie projektu**.</span><span class="sxs-lookup"><span data-stu-id="2bddd-158">In the **New ASP.NET Project** window, leave **Empty** selected and click **Create Project**.</span></span>

    ![Tworzenie pustego sieci web](tutorial-getting-started-with-signalr/_static/image3.png)
3. <span data-ttu-id="2bddd-160">W **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy projekt, wybierz **Dodaj | Klasa Centrum SignalR (v2)**.</span><span class="sxs-lookup"><span data-stu-id="2bddd-160">In **Solution Explorer**, right-click the project, select **Add | SignalR Hub Class (v2)**.</span></span> <span data-ttu-id="2bddd-161">Nazwa klasy **ChatHub.cs** i dodaj go do projektu.</span><span class="sxs-lookup"><span data-stu-id="2bddd-161">Name the class **ChatHub.cs** and add it to the project.</span></span> <span data-ttu-id="2bddd-162">Spowoduje to utworzenie **ChatHub** klasy i dodaje do projektu zestawu plików skryptów i odwołania do zestawów, obsługujące bibliotekę SignalR.</span><span class="sxs-lookup"><span data-stu-id="2bddd-162">This step creates the **ChatHub** class and adds to the project a set of script files and assembly references that support SignalR.</span></span>

    > [!NOTE]
    > <span data-ttu-id="2bddd-163">Biblioteki SignalR można dodać do projektu, otwierając **narzędzia | Menedżer pakietów biblioteki | Konsola Menedżera pakietów** i uruchamiając polecenie:</span><span class="sxs-lookup"><span data-stu-id="2bddd-163">You can also add SignalR to a project by opening the **Tools | Library Package Manager | Package Manager Console** and running a command:</span></span>

    `install-package Microsoft.AspNet.SignalR`

    <span data-ttu-id="2bddd-164">Jeśli używasz konsoli można dodać SignalR, należy utworzyć klasa Centrum SignalR w osobnym kroku po dodaniu SignalR.</span><span class="sxs-lookup"><span data-stu-id="2bddd-164">If you use the console to add SignalR, create the SignalR hub class as a separate step after you add SignalR.</span></span>

    > [!NOTE]
    > <span data-ttu-id="2bddd-165">Jeśli używasz programu Visual Studio 2012, **klasa Centrum SignalR (v2)** szablonu nie będą dostępne.</span><span class="sxs-lookup"><span data-stu-id="2bddd-165">If you are using Visual Studio 2012, the **SignalR Hub Class (v2)** template will not be available.</span></span> <span data-ttu-id="2bddd-166">Możesz dodać zwykły **klasy** o nazwie `ChatHub` zamiast tego.</span><span class="sxs-lookup"><span data-stu-id="2bddd-166">You can add a plain **Class** called `ChatHub` instead.</span></span>
4. <span data-ttu-id="2bddd-167">W **Eksploratora rozwiązań**, rozwiń węzeł skryptów.</span><span class="sxs-lookup"><span data-stu-id="2bddd-167">In **Solution Explorer**, expand the Scripts node.</span></span> <span data-ttu-id="2bddd-168">Biblioteki skryptów, jQuery i SignalR są widoczne w projekcie.</span><span class="sxs-lookup"><span data-stu-id="2bddd-168">Script libraries for jQuery and SignalR are visible in the project.</span></span>
5. <span data-ttu-id="2bddd-169">Zastąp kod w nowej **ChatHub** klasy z następującym kodem.</span><span class="sxs-lookup"><span data-stu-id="2bddd-169">Replace the code in the new **ChatHub** class with the following code.</span></span>

    [!code-csharp[Main](tutorial-getting-started-with-signalr/samples/sample1.cs)]
6. <span data-ttu-id="2bddd-170">W **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy projekt, a następnie kliknij przycisk **Dodaj | Klasa początkowa OWIN**.</span><span class="sxs-lookup"><span data-stu-id="2bddd-170">In **Solution Explorer**, right-click the project, then click **Add | OWIN Startup Class**.</span></span> <span data-ttu-id="2bddd-171">Nadaj nowej klasie `Startup` i kliknij przycisk OK.</span><span class="sxs-lookup"><span data-stu-id="2bddd-171">Name the new class `Startup` and click OK.</span></span>

    > [!NOTE]
    > <span data-ttu-id="2bddd-172">Jeśli używasz programu Visual Studio 2012, **klasy początkowej OWIN** szablonu nie będą dostępne.</span><span class="sxs-lookup"><span data-stu-id="2bddd-172">If you are using Visual Studio 2012, the **OWIN Startup Class** template will not be available.</span></span> <span data-ttu-id="2bddd-173">Możesz dodać zwykły **klasy** o nazwie `Startup` zamiast tego.</span><span class="sxs-lookup"><span data-stu-id="2bddd-173">You can add a plain **Class** called `Startup` instead.</span></span>
7. <span data-ttu-id="2bddd-174">Zmień zawartość elementu nową klasę uruchamiania do następujących.</span><span class="sxs-lookup"><span data-stu-id="2bddd-174">Change the contents of the new Startup class to the following.</span></span>

    [!code-csharp[Main](tutorial-getting-started-with-signalr/samples/sample2.cs)]
8. <span data-ttu-id="2bddd-175">W **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy projekt, a następnie kliknij przycisk **Dodaj | Strona HTML**.</span><span class="sxs-lookup"><span data-stu-id="2bddd-175">In **Solution Explorer**, right-click the project, then click **Add | HTML Page**.</span></span> <span data-ttu-id="2bddd-176">Nadaj nowej stronie `index.html`.</span><span class="sxs-lookup"><span data-stu-id="2bddd-176">Name the new page `index.html`.</span></span>
    >[!NOTE]
    ><span data-ttu-id="2bddd-177">Czasami trzeba zmienić numery wersji dla odwołań do biblioteki JQuery i SignalR</span><span class="sxs-lookup"><span data-stu-id="2bddd-177">You might need to change the version numbers for the references to JQuery and SignalR libraries</span></span>
9. <span data-ttu-id="2bddd-178">W **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy właśnie utworzony strony HTML i kliknij przycisk **Ustaw jako strona startowa**.</span><span class="sxs-lookup"><span data-stu-id="2bddd-178">In **Solution Explorer**, right-click the HTML page you just created and click **Set as Start Page**.</span></span>
10. <span data-ttu-id="2bddd-179">Zastąp kod domyślnej strony HTML z następującym kodem.</span><span class="sxs-lookup"><span data-stu-id="2bddd-179">Replace the default code in the HTML page with the following code.</span></span>

    > [!NOTE]
    > <span data-ttu-id="2bddd-180">Może być zainstalowana nowsza wersja skryptów SignalR przez Menedżera pakietów.</span><span class="sxs-lookup"><span data-stu-id="2bddd-180">A later version of the SignalR scripts may be installed by the package manager.</span></span> <span data-ttu-id="2bddd-181">Sprawdź, czy odwołania do skryptu poniżej odpowiadają wersji plików skrypt w projekcie (będą różne, jeśli dodano SignalR za pomocą narzędzia NuGet, zamiast dodawać koncentrator.)</span><span class="sxs-lookup"><span data-stu-id="2bddd-181">Verify that the script references below correspond to the versions of the script files in the project (they will be different if you added SignalR using NuGet rather than adding a hub.)</span></span>

    [!code-html[Main](tutorial-getting-started-with-signalr/samples/sample3.html)]
11. <span data-ttu-id="2bddd-182">**Zapisz wszystko** dla projektu.</span><span class="sxs-lookup"><span data-stu-id="2bddd-182">**Save All** for the project.</span></span>

<a id="run"></a>

## <a name="run-the-sample"></a><span data-ttu-id="2bddd-183">Uruchamianie aplikacji przykładowej</span><span class="sxs-lookup"><span data-stu-id="2bddd-183">Run the Sample</span></span>

1. <span data-ttu-id="2bddd-184">Naciśnij klawisz F5, aby uruchomić projekt w trybie debugowania.</span><span class="sxs-lookup"><span data-stu-id="2bddd-184">Press F5 to run the project in debug mode.</span></span> <span data-ttu-id="2bddd-185">Strona HTML ładuje się w wystąpieniu przeglądarki i wyświetla monit o nazwę użytkownika.</span><span class="sxs-lookup"><span data-stu-id="2bddd-185">The HTML page loads in a browser instance and prompts for a user name.</span></span>

    ![Wprowadź nazwę użytkownika](tutorial-getting-started-with-signalr/_static/image4.png)
2. <span data-ttu-id="2bddd-187">Wprowadź nazwę użytkownika.</span><span class="sxs-lookup"><span data-stu-id="2bddd-187">Enter a user name.</span></span>
3. <span data-ttu-id="2bddd-188">Skopiuj adres URL w wierszu adresu przeglądarki i użyj go, aby otworzyć dwóch większej liczby wystąpień przeglądarki.</span><span class="sxs-lookup"><span data-stu-id="2bddd-188">Copy the URL from the address line of the browser and use it to open two more browser instances.</span></span> <span data-ttu-id="2bddd-189">W każdym wystąpieniu przeglądarki wprowadź unikatową nazwę użytkownika.</span><span class="sxs-lookup"><span data-stu-id="2bddd-189">In each browser instance, enter a unique user name.</span></span>
4. <span data-ttu-id="2bddd-190">W każdym wystąpieniu przeglądarki Dodaj komentarz, a następnie kliknij przycisk **wysyłania**.</span><span class="sxs-lookup"><span data-stu-id="2bddd-190">In each browser instance, add a comment and click **Send**.</span></span> <span data-ttu-id="2bddd-191">Komentarze powinien być wyświetlany we wszystkich wystąpieniach przeglądarki.</span><span class="sxs-lookup"><span data-stu-id="2bddd-191">The comments should display in all browser instances.</span></span>

    > [!NOTE]
    > <span data-ttu-id="2bddd-192">Tej aplikacji rozmów prostego nie utrzymuje kontekst dyskusji na serwerze.</span><span class="sxs-lookup"><span data-stu-id="2bddd-192">This simple chat application does not maintain the discussion context on the server.</span></span> <span data-ttu-id="2bddd-193">Centrum emituje komentarzy do wszystkich bieżących użytkowników.</span><span class="sxs-lookup"><span data-stu-id="2bddd-193">The hub broadcasts comments to all current users.</span></span> <span data-ttu-id="2bddd-194">Użytkownicy, którzy dołączają do czatu później zostanie wyświetlony komunikat o dodane od czasu dołączenia.</span><span class="sxs-lookup"><span data-stu-id="2bddd-194">Users who join the chat later will see messages added from the time they join.</span></span>

    <span data-ttu-id="2bddd-195">Poniższy zrzut ekranu przedstawia aplikacji rozmów w trzech wystąpień przeglądarki, z których wszystkie są aktualizowane w jedno wystąpienie jest wysyłana wiadomość:</span><span class="sxs-lookup"><span data-stu-id="2bddd-195">The following screen shot shows the chat application running in three browser instances, all of which are updated when one instance sends a message:</span></span>

    ![Rozmowa przeglądarki](tutorial-getting-started-with-signalr/_static/image5.png)
5. <span data-ttu-id="2bddd-197">W **Eksploratora rozwiązań**, zbadaj **dokumenty skryptów** węzła dla działającej aplikacji.</span><span class="sxs-lookup"><span data-stu-id="2bddd-197">In **Solution Explorer**, inspect the **Script Documents** node for the running application.</span></span> <span data-ttu-id="2bddd-198">Brak pliku skryptu o nazwie **koncentratory** generujący biblioteki SignalR dynamicznie w czasie wykonywania.</span><span class="sxs-lookup"><span data-stu-id="2bddd-198">There is a script file named **hubs** that the SignalR library dynamically generates at runtime.</span></span> <span data-ttu-id="2bddd-199">Ten plik zarządza komunikacji między jQuery skryptu i kod po stronie serwera.</span><span class="sxs-lookup"><span data-stu-id="2bddd-199">This file manages the communication between jQuery script and server-side code.</span></span>

    ![](tutorial-getting-started-with-signalr/_static/image6.png)

<a id="code"></a>

## <a name="examine-the-code"></a><span data-ttu-id="2bddd-200">Poszukaj w kodzie</span><span class="sxs-lookup"><span data-stu-id="2bddd-200">Examine the Code</span></span>

<span data-ttu-id="2bddd-201">Aplikacji rozmów SignalR pokazuje dwa podstawowe zadania rozwoju SignalR: tworzenie koncentratora jako obiekt główny koordynacji na serwerze i za pomocą biblioteki jQuery SignalR do wysyłania i odbierania komunikatów.</span><span class="sxs-lookup"><span data-stu-id="2bddd-201">The SignalR chat application demonstrates two basic SignalR development tasks: creating a hub as the main coordination object on the server, and using the SignalR jQuery library to send and receive messages.</span></span>

### <a name="signalr-hubs"></a><span data-ttu-id="2bddd-202">Koncentratory SignalR</span><span class="sxs-lookup"><span data-stu-id="2bddd-202">SignalR Hubs</span></span>

<span data-ttu-id="2bddd-203">W przykładowym kodzie **ChatHub** klasa pochodzi od **Microsoft.AspNet.SignalR.Hub** klasy.</span><span class="sxs-lookup"><span data-stu-id="2bddd-203">In the code sample the **ChatHub** class derives from the **Microsoft.AspNet.SignalR.Hub** class.</span></span> <span data-ttu-id="2bddd-204">Wyprowadzanie z **Centrum** klasy jest to wygodny sposób, aby skompilować aplikację SignalR.</span><span class="sxs-lookup"><span data-stu-id="2bddd-204">Deriving from the **Hub** class is a useful way to build a SignalR application.</span></span> <span data-ttu-id="2bddd-205">Można utworzyć metody publiczne na klasy koncentratora, a następnie uzyskać dostęp do tych metod, wywołując je ze skryptów na stronie sieci web.</span><span class="sxs-lookup"><span data-stu-id="2bddd-205">You can create public methods on your hub class and then access those methods by calling them from scripts in a web page.</span></span>

<span data-ttu-id="2bddd-206">W kodzie Rozmowa klienci wywołują **ChatHub.Send** metodę, aby wysyłać nowy komunikat.</span><span class="sxs-lookup"><span data-stu-id="2bddd-206">In the chat code, clients call the **ChatHub.Send** method to send a new message.</span></span> <span data-ttu-id="2bddd-207">Centrum z kolei wysyła wiadomość do wszystkich klientów, wywołując **Clients.All.broadcastMessage**.</span><span class="sxs-lookup"><span data-stu-id="2bddd-207">The hub in turn sends the message to all clients by calling **Clients.All.broadcastMessage**.</span></span>

<span data-ttu-id="2bddd-208">**Wysyłania** metoda pokazuje kilka koncepcji w Centrum:</span><span class="sxs-lookup"><span data-stu-id="2bddd-208">The **Send** method demonstrates several hub concepts :</span></span>

- <span data-ttu-id="2bddd-209">Tak, aby je wywoływać klientów do deklarowania metod publicznych w koncentratorze.</span><span class="sxs-lookup"><span data-stu-id="2bddd-209">Declare public methods on a hub so that clients can call them.</span></span>
- <span data-ttu-id="2bddd-210">Użyj **Microsoft.AspNet.SignalR.Hub.Clients** właściwość dynamiczna ma dostęp do wszystkich klientów podłączone do tego koncentratora.</span><span class="sxs-lookup"><span data-stu-id="2bddd-210">Use the **Microsoft.AspNet.SignalR.Hub.Clients** dynamic property to access all clients connected to this hub.</span></span>
- <span data-ttu-id="2bddd-211">Wywoływanie funkcji na komputerze klienckim (takich jak `broadcastMessage` funkcji) aby zaktualizować klientów.</span><span class="sxs-lookup"><span data-stu-id="2bddd-211">Call a function on the client (such as the `broadcastMessage` function) to update clients.</span></span>

    [!code-csharp[Main](tutorial-getting-started-with-signalr/samples/sample4.cs)]

### <a name="signalr-and-jquery"></a><span data-ttu-id="2bddd-212">SignalR i jQuery</span><span class="sxs-lookup"><span data-stu-id="2bddd-212">SignalR and jQuery</span></span>

<span data-ttu-id="2bddd-213">Strony HTML w przykładzie kodu pokazano, jak użyć biblioteki jQuery SignalR do komunikowania się z Centrum SignalR.</span><span class="sxs-lookup"><span data-stu-id="2bddd-213">The HTML page in the code sample shows how to use the SignalR jQuery library to communicate with a SignalR hub.</span></span> <span data-ttu-id="2bddd-214">Serwer proxy, aby odwoływać się do Centrum deklarowania funkcji, która serwera można wywołać w celu wypychania zawartości do klientów i od połączenia, aby wysyłać komunikaty do Centrum deklaruje podstawowych zadań w kodzie.</span><span class="sxs-lookup"><span data-stu-id="2bddd-214">The essential tasks in the code are declaring a proxy to reference the hub, declaring a function that the server can call to push content to clients, and starting a connection to send messages to the hub.</span></span>

<span data-ttu-id="2bddd-215">Poniższy kod deklaruje odwołanie do serwera proxy koncentratora.</span><span class="sxs-lookup"><span data-stu-id="2bddd-215">The following code declares a reference to a hub proxy.</span></span>

[!code-javascript[Main](tutorial-getting-started-with-signalr/samples/sample5.js)]

> [!NOTE]
> <span data-ttu-id="2bddd-216">Odwołanie do klasy serwera i jej elementów członkowskich w JavaScript znajduje się w camelcase.</span><span class="sxs-lookup"><span data-stu-id="2bddd-216">In JavaScript the reference to the server class and its members is in camel case.</span></span> <span data-ttu-id="2bddd-217">Przykładowy kod odwołuje się do języka C# **ChatHub** klasy w języku JavaScript jako **chatHub**.</span><span class="sxs-lookup"><span data-stu-id="2bddd-217">The code sample references the C# **ChatHub** class in JavaScript as **chatHub**.</span></span>


<span data-ttu-id="2bddd-218">Poniższy kod jest na tym, jak utworzyć funkcję wywołania zwrotnego w skrypcie.</span><span class="sxs-lookup"><span data-stu-id="2bddd-218">The following code is how you create a callback function in the script.</span></span> <span data-ttu-id="2bddd-219">Klasy koncentratora na serwerze wywołuje tę funkcję, aby wypchnąć aktualizacji zawartości dla każdego klienta.</span><span class="sxs-lookup"><span data-stu-id="2bddd-219">The hub class on the server calls this function to push content updates to each client.</span></span> <span data-ttu-id="2bddd-220">Dwa wiersze, czy kodowanie HTML zawartości przed wyświetleniem go są opcjonalne i Pokaż prostego sposobu zapobiegania uruchomienie skryptu.</span><span class="sxs-lookup"><span data-stu-id="2bddd-220">The two lines that HTML encode the content before displaying it are optional and show a simple way to prevent script injection.</span></span>

[!code-html[Main](tutorial-getting-started-with-signalr/samples/sample6.html)]

<span data-ttu-id="2bddd-221">Poniższy kod przedstawia sposób nawiązać połączenie z koncentratorem.</span><span class="sxs-lookup"><span data-stu-id="2bddd-221">The following code shows how to open a connection with the hub.</span></span> <span data-ttu-id="2bddd-222">Kod uruchamia połączenie i przekazuje go po funkcji do obsługi zdarzenia click na **wysyłania** przycisk na stronie HTML.</span><span class="sxs-lookup"><span data-stu-id="2bddd-222">The code starts the connection and then passes it a function to handle the click event on the **Send** button in the HTML page.</span></span>

> [!NOTE]
> <span data-ttu-id="2bddd-223">Takie podejście zapewnia, że połączenie zostało nawiązane przed wykonaniem programu obsługi zdarzeń.</span><span class="sxs-lookup"><span data-stu-id="2bddd-223">This approach ensures that the connection is established before the event handler executes.</span></span>


[!code-javascript[Main](tutorial-getting-started-with-signalr/samples/sample7.js)]

<a id="next"></a>

## <a name="next-steps"></a><span data-ttu-id="2bddd-224">Następne kroki</span><span class="sxs-lookup"><span data-stu-id="2bddd-224">Next Steps</span></span>

<span data-ttu-id="2bddd-225">Wiesz, że SignalR to architektura służąca do tworzenia aplikacji sieci web w czasie rzeczywistym.</span><span class="sxs-lookup"><span data-stu-id="2bddd-225">You learned that SignalR is a framework for building real-time web applications.</span></span> <span data-ttu-id="2bddd-226">Pokazaliśmy również kilka zadań programistycznych SignalR: jak dodać SignalR do aplikacji ASP.NET, sposób tworzenia klasy koncentratora oraz jak wysyłać i odbierać komunikaty z Centrum.</span><span class="sxs-lookup"><span data-stu-id="2bddd-226">You also learned several SignalR development tasks: how to add SignalR to an ASP.NET application, how to create a hub class, and how to send and receive messages from the hub.</span></span>

<span data-ttu-id="2bddd-227">Aby uzyskać wskazówki dotyczące sposobu wdrażania przykładowej aplikacji SignalR na platformie Azure, zobacz [przy użyciu SignalR z usługą Web Apps w usłudze Azure App Service](../deployment/using-signalr-with-azure-web-sites.md).</span><span class="sxs-lookup"><span data-stu-id="2bddd-227">For a walkthrough on how to deploy the sample SignalR application to Azure, see [Using SignalR with Web Apps in Azure App Service](../deployment/using-signalr-with-azure-web-sites.md).</span></span> <span data-ttu-id="2bddd-228">Aby uzyskać szczegółowe informacje o sposobie wdrażania projektu sieci web programu Visual Studio z witryny sieci Web do Windows Azure, zobacz [tworzenie aplikacji sieci web platformy ASP.NET w usłudze Azure App Service](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-get-started/).</span><span class="sxs-lookup"><span data-stu-id="2bddd-228">For detailed information about how to deploy a Visual Studio web project to a Windows Azure Web Site, see [Create an ASP.NET web app in Azure App Service](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-get-started/).</span></span>

<span data-ttu-id="2bddd-229">Aby dowiedzieć się bardziej zaawansowanych pojęciach zmiany SignalR, odwiedź następującą witrynę dla SignalR kod źródłowy i zasobów:</span><span class="sxs-lookup"><span data-stu-id="2bddd-229">To learn more advanced SignalR developments concepts, visit the following sites for SignalR source code and resources:</span></span>

- [<span data-ttu-id="2bddd-230">Projekt SignalR</span><span class="sxs-lookup"><span data-stu-id="2bddd-230">SignalR Project</span></span>](http://signalr.net)
- [<span data-ttu-id="2bddd-231">SignalR Github i przykłady</span><span class="sxs-lookup"><span data-stu-id="2bddd-231">SignalR Github and Samples</span></span>](https://github.com/SignalR/SignalR)
- [<span data-ttu-id="2bddd-232">Witryny typu Wiki biblioteki SignalR</span><span class="sxs-lookup"><span data-stu-id="2bddd-232">SignalR Wiki</span></span>](https://github.com/SignalR/SignalR/wiki)
