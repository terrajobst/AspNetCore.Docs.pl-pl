---
uid: signalr/overview/older-versions/tutorial-getting-started-with-signalr
title: 'Samouczek: Wprowadzenie do korzystania z SignalR 1.x | Dokumentacja firmy Microsoft'
author: pfletcher
description: Umożliwia tworzenie aplikacji czatu w czasie rzeczywistym na stronie HTML ASP.NET SignalR.
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/18/2013
ms.topic: article
ms.assetid: fdc3599a-5217-44c1-951f-0eec9812dce7
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/older-versions/tutorial-getting-started-with-signalr
msc.type: authoredcontent
ms.openlocfilehash: ce4953a0abf64af28ef4dbc5a62bb2d989343d99
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/24/2018
---
<a name="tutorial-getting-started-with-signalr-1x"></a><span data-ttu-id="c90fc-103">Samouczek: Wprowadzenie do korzystania z SignalR 1.x</span><span class="sxs-lookup"><span data-stu-id="c90fc-103">Tutorial: Getting Started with SignalR 1.x</span></span>
====================
<span data-ttu-id="c90fc-104">przez [Patrick Fletcher](https://github.com/pfletcher), [Teebken Timowi](https://github.com/timlt)</span><span class="sxs-lookup"><span data-stu-id="c90fc-104">by [Patrick Fletcher](https://github.com/pfletcher), [Tim Teebken](https://github.com/timlt)</span></span>

> <span data-ttu-id="c90fc-105">Ten samouczek pokazuje, jak utworzyć aplikację do obsługi rozmów w czasie rzeczywistym przy użyciu SignalR.</span><span class="sxs-lookup"><span data-stu-id="c90fc-105">This tutorial shows how to use SignalR to create a real-time chat application.</span></span> <span data-ttu-id="c90fc-106">Spowoduje dodanie SignalR do pustą aplikację sieci web ASP.NET i Utwórz stronę HTML do wysyłania i wyświetlenie komunikatów.</span><span class="sxs-lookup"><span data-stu-id="c90fc-106">You will add SignalR to an empty ASP.NET web application and create an HTML page to send and display messages.</span></span>


## <a name="overview"></a><span data-ttu-id="c90fc-107">Omówienie</span><span class="sxs-lookup"><span data-stu-id="c90fc-107">Overview</span></span>

<span data-ttu-id="c90fc-108">W tym samouczku wprowadzenie programowanie SignalR przez przedstawiający sposób tworzenia aplikacji proste rozmów bazujące na przeglądarce.</span><span class="sxs-lookup"><span data-stu-id="c90fc-108">This tutorial introduces SignalR development by showing how to build a simple browser-based chat application.</span></span> <span data-ttu-id="c90fc-109">Zostanie dodać biblioteki SignalR do pusta aplikacja sieci web platformy ASP.NET, Utwórz klasę koncentratora do wysyłania wiadomości do klientów i utworzyć strona HTML, która umożliwia użytkownikom wysyłanie i odbieranie wiadomości.</span><span class="sxs-lookup"><span data-stu-id="c90fc-109">You will add the SignalR library to an empty ASP.NET web application, create a hub class for sending messages to clients, and create an HTML page that lets users send and receive chat messages.</span></span> <span data-ttu-id="c90fc-110">Podobne samouczek przedstawia sposób tworzenia aplikacji rozmów w MVC 4 przy użyciu widoku MVC, zobacz [wprowadzenie SignalR i MVC 4](index.md).</span><span class="sxs-lookup"><span data-stu-id="c90fc-110">For a similar tutorial that shows how to create a chat application in MVC 4 using an MVC view, see [Getting Started with SignalR and MVC 4](index.md).</span></span>

> [!NOTE]
> <span data-ttu-id="c90fc-111">Ten samouczek używa (1.x) wersji biblioteki signalr.</span><span class="sxs-lookup"><span data-stu-id="c90fc-111">This tutorial uses the release (1.x) version of SignalR.</span></span> <span data-ttu-id="c90fc-112">Aby uzyskać szczegółowe informacje o zmianach między SignalR 1.x i 2.0, zobacz [SignalR Uaktualnianie projektów 1.x](../releases/upgrading-signalr-1x-projects-to-20.md).</span><span class="sxs-lookup"><span data-stu-id="c90fc-112">For details on changes between SignalR 1.x and 2.0, see [Upgrading SignalR 1.x Projects](../releases/upgrading-signalr-1x-projects-to-20.md).</span></span>

<span data-ttu-id="c90fc-113">SignalR to open source biblioteki .NET do tworzenia aplikacji sieci web, które wymagają interakcji z użytkownikiem na żywo lub aktualizacji danych w czasie rzeczywistym.</span><span class="sxs-lookup"><span data-stu-id="c90fc-113">SignalR is an open-source .NET library for building web applications that require live user interaction or real-time data updates.</span></span> <span data-ttu-id="c90fc-114">Przykładami społecznościowych aplikacji, wielodostępnym gry, business współpracy i grup dyskusyjnych, pogody lub finansowych aktualizacji aplikacji.</span><span class="sxs-lookup"><span data-stu-id="c90fc-114">Examples include social applications, multiuser games, business collaboration, and news, weather, or financial update applications.</span></span> <span data-ttu-id="c90fc-115">Są one często nazywane aplikacji w czasie rzeczywistym.</span><span class="sxs-lookup"><span data-stu-id="c90fc-115">These are often called real-time applications.</span></span>

<span data-ttu-id="c90fc-116">SignalR upraszcza proces tworzenia aplikacji w czasie rzeczywistym.</span><span class="sxs-lookup"><span data-stu-id="c90fc-116">SignalR simplifies the process of building real-time applications.</span></span> <span data-ttu-id="c90fc-117">Zawiera bibliotekę serwera programu ASP.NET i biblioteki klienckiej JavaScript, aby ułatwić zarządzanie połączeniami klient serwer i wypychanie aktualizacji zawartości do klientów.</span><span class="sxs-lookup"><span data-stu-id="c90fc-117">It includes an ASP.NET server library and a JavaScript client library to make it easier to manage client-server connections and push content updates to clients.</span></span> <span data-ttu-id="c90fc-118">Biblioteka SignalR można dodać do istniejącej aplikacji ASP.NET do uzyskania funkcjonalność w czasie rzeczywistym.</span><span class="sxs-lookup"><span data-stu-id="c90fc-118">You can add the SignalR library to an existing ASP.NET application to gain real-time functionality.</span></span>

<span data-ttu-id="c90fc-119">Samouczek przedstawia następujące SignalR zadań związanych z projektowaniem:</span><span class="sxs-lookup"><span data-stu-id="c90fc-119">The tutorial demonstrates the following SignalR development tasks:</span></span>

- <span data-ttu-id="c90fc-120">Dodawanie biblioteki SignalR do aplikacji sieci web ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="c90fc-120">Adding the SignalR library to an ASP.NET web application.</span></span>
- <span data-ttu-id="c90fc-121">Tworzenie klasy koncentratora do dystrybuowania zawartości do klientów.</span><span class="sxs-lookup"><span data-stu-id="c90fc-121">Creating a hub class to push content to clients.</span></span>
- <span data-ttu-id="c90fc-122">Za pomocą biblioteki jQuery SignalR na stronie sieci web można wysyłać wiadomości i wyświetlać aktualizacje z koncentratora.</span><span class="sxs-lookup"><span data-stu-id="c90fc-122">Using the SignalR jQuery library in a web page to send messages and display updates from the hub.</span></span>

<span data-ttu-id="c90fc-123">Poniższy zrzut ekranu przedstawia aplikacji czatu działającego w przeglądarce.</span><span class="sxs-lookup"><span data-stu-id="c90fc-123">The following screen shot shows the chat application running in a browser.</span></span> <span data-ttu-id="c90fc-124">Każdy nowy użytkownik można publikować komentarze i wyświetlić komentarze, dodawane po użytkownik nie przyłączy rozmowę.</span><span class="sxs-lookup"><span data-stu-id="c90fc-124">Each new user can post comments and see comments added after the user joins the chat.</span></span>

![Rozmowa wystąpień](tutorial-getting-started-with-signalr/_static/image1.png)

<span data-ttu-id="c90fc-126">Sekcje:</span><span class="sxs-lookup"><span data-stu-id="c90fc-126">Sections:</span></span>

- [<span data-ttu-id="c90fc-127">Konfigurowanie projektu</span><span class="sxs-lookup"><span data-stu-id="c90fc-127">Set up the Project</span></span>](#setup)
- [<span data-ttu-id="c90fc-128">Uruchom próbki</span><span class="sxs-lookup"><span data-stu-id="c90fc-128">Run the Sample</span></span>](#run)
- [<span data-ttu-id="c90fc-129">Sprawdź kod</span><span class="sxs-lookup"><span data-stu-id="c90fc-129">Examine the Code</span></span>](#code)
- [<span data-ttu-id="c90fc-130">Następne kroki</span><span class="sxs-lookup"><span data-stu-id="c90fc-130">Next Steps</span></span>](#next)

<a id="setup"></a>

## <a name="set-up-the-project"></a><span data-ttu-id="c90fc-131">Konfigurowanie projektu</span><span class="sxs-lookup"><span data-stu-id="c90fc-131">Set up the Project</span></span>

<span data-ttu-id="c90fc-132">W tej sekcji pokazano, jak utworzyć pustą aplikację sieci web ASP.NET, Dodaj SignalR i tworzenie aplikacji czatu.</span><span class="sxs-lookup"><span data-stu-id="c90fc-132">This section shows how to create an empty ASP.NET web application, add SignalR, and create the chat application.</span></span>

<span data-ttu-id="c90fc-133">Wymagania wstępne:</span><span class="sxs-lookup"><span data-stu-id="c90fc-133">Prerequisites:</span></span>

- <span data-ttu-id="c90fc-134">Visual Studio 2010 z dodatkiem SP1 lub 2012.</span><span class="sxs-lookup"><span data-stu-id="c90fc-134">Visual Studio 2010 SP1 or 2012.</span></span> <span data-ttu-id="c90fc-135">Jeśli nie masz programu Visual Studio, zobacz [pobiera ASP.NET](https://www.asp.net/downloads) uzyskać bezpłatne narzędzie Visual Studio 2012 Express programowanie.</span><span class="sxs-lookup"><span data-stu-id="c90fc-135">If you do not have Visual Studio, see [ASP.NET Downloads](https://www.asp.net/downloads) to get the free Visual Studio 2012 Express Development Tool.</span></span>
- <span data-ttu-id="c90fc-136">[Microsoft ASP.NET i sieć Web narzędzi 2012.2](https://go.microsoft.com/fwlink/?LinkId=279941).</span><span class="sxs-lookup"><span data-stu-id="c90fc-136">[Microsoft ASP.NET and Web Tools 2012.2](https://go.microsoft.com/fwlink/?LinkId=279941).</span></span> <span data-ttu-id="c90fc-137">Dla programu Visual Studio 2012 ten Instalator dodaje nowe funkcje platformy ASP.NET w tym szablony SignalR dla programu Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="c90fc-137">For Visual Studio 2012, this installer adds new ASP.NET features including SignalR templates to Visual Studio.</span></span> <span data-ttu-id="c90fc-138">Dla programu Visual Studio 2010 z dodatkiem SP1 Instalator nie jest dostępny, ale można Ukończ samouczek, instalując pakiet SignalR NuGet, zgodnie z opisem w ramach kroków konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="c90fc-138">For Visual Studio 2010 SP1, an installer is not available but you can complete the tutorial by installing the SignalR NuGet package as described in the setup steps.</span></span>

<span data-ttu-id="c90fc-139">Następujące kroki Użyj programu Visual Studio 2012, aby utworzyć pustą aplikację sieci Web ASP.NET i dodać biblioteki SignalR:</span><span class="sxs-lookup"><span data-stu-id="c90fc-139">The following steps use Visual Studio 2012 to create an ASP.NET Empty Web Application and add the SignalR library:</span></span>

1. <span data-ttu-id="c90fc-140">W programie Visual Studio Utwórz pustą aplikację sieci Web ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="c90fc-140">In Visual Studio create an ASP.NET Empty Web Application.</span></span>

    ![Utwórz pusty sieci web](tutorial-getting-started-with-signalr/_static/image2.png)
2. <span data-ttu-id="c90fc-142">Otwórz **Konsola Menedżera pakietów** wybierając **narzędzia | Menedżer pakietów biblioteki | Konsola Menedżera pakietów**.</span><span class="sxs-lookup"><span data-stu-id="c90fc-142">Open the **Package Manager Console** by selecting **Tools | Library Package Manager | Package Manager Console**.</span></span> <span data-ttu-id="c90fc-143">Wprowadź następujące polecenie w oknie konsoli:</span><span class="sxs-lookup"><span data-stu-id="c90fc-143">Enter the following command into the console window:</span></span>

    `Install-Package Microsoft.AspNet.SignalR -Version 1.1.3`

    <span data-ttu-id="c90fc-144">To polecenie powoduje zainstalowanie najnowszej wersji biblioteki signalr 1.x.</span><span class="sxs-lookup"><span data-stu-id="c90fc-144">This command installs the latest version of SignalR 1.x.</span></span>
3. <span data-ttu-id="c90fc-145">W **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy projekt, wybierz **Dodaj | Klasa**.</span><span class="sxs-lookup"><span data-stu-id="c90fc-145">In **Solution Explorer**, right-click the project, select **Add | Class**.</span></span> <span data-ttu-id="c90fc-146">Nazwa nowej klasy **ChatHub**.</span><span class="sxs-lookup"><span data-stu-id="c90fc-146">Name the new class **ChatHub**.</span></span>
4. <span data-ttu-id="c90fc-147">W **Eksploratora rozwiązań** rozwiń węzeł skryptów.</span><span class="sxs-lookup"><span data-stu-id="c90fc-147">In **Solution Explorer** expand the Scripts node.</span></span> <span data-ttu-id="c90fc-148">Biblioteki skryptów, jQuery i SignalR są widoczne w projekcie.</span><span class="sxs-lookup"><span data-stu-id="c90fc-148">Script libraries for jQuery and SignalR are visible in the project.</span></span>

    ![Odwołania do biblioteki](tutorial-getting-started-with-signalr/_static/image3.png)
5. <span data-ttu-id="c90fc-150">Zastąp kod w **ChatHub** klasy następującym kodem.</span><span class="sxs-lookup"><span data-stu-id="c90fc-150">Replace the code in the **ChatHub** class with the following code.</span></span>

    [!code-csharp[Main](tutorial-getting-started-with-signalr/samples/sample1.cs)]
6. <span data-ttu-id="c90fc-151">W **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy projekt, a następnie kliknij przycisk **Dodaj | Nowy element**.</span><span class="sxs-lookup"><span data-stu-id="c90fc-151">In **Solution Explorer**, right-click the project, then click **Add | New Item**.</span></span> <span data-ttu-id="c90fc-152">W **Dodaj nowy element** okno dialogowe, wybierz opcję **globalnej klasy aplikacji** i kliknij przycisk **Dodaj**.</span><span class="sxs-lookup"><span data-stu-id="c90fc-152">In the **Add New Item** dialog, select **Global Application Class** and click **Add**.</span></span>

    ![Dodaj globalne](tutorial-getting-started-with-signalr/_static/image4.png)
7. <span data-ttu-id="c90fc-154">Dodaj następujące `using` instrukcje po dostarczonych `using` instrukcje w klasie Global.asax.cs.</span><span class="sxs-lookup"><span data-stu-id="c90fc-154">Add the following `using` statements after the provided `using` statements in the Global.asax.cs class.</span></span>

    [!code-csharp[Main](tutorial-getting-started-with-signalr/samples/sample2.cs)]
8. <span data-ttu-id="c90fc-155">Dodaj następujący wiersz kodu w `Application_Start` metody klasy globalny można zarejestrować trasy domyślnej koncentratorów SignalR.</span><span class="sxs-lookup"><span data-stu-id="c90fc-155">Add the following line of code in the `Application_Start` method of the Global class to register the default route for SignalR hubs.</span></span>

    [!code-csharp[Main](tutorial-getting-started-with-signalr/samples/sample3.cs)]
9. <span data-ttu-id="c90fc-156">W **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy projekt, a następnie kliknij przycisk **Dodaj | Nowy element**.</span><span class="sxs-lookup"><span data-stu-id="c90fc-156">In **Solution Explorer**, right-click the project, then click **Add | New Item**.</span></span> <span data-ttu-id="c90fc-157">W **Dodaj nowy element** okno dialogowe, wybierz strony Html i kliknij przycisk **Dodaj**.</span><span class="sxs-lookup"><span data-stu-id="c90fc-157">In the **Add New Item** dialog, select Html Page and click **Add**.</span></span>
10. <span data-ttu-id="c90fc-158">W **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy właśnie utworzony strony HTML i kliknij przycisk **Ustaw jako stronę startową**.</span><span class="sxs-lookup"><span data-stu-id="c90fc-158">In **Solution Explorer**, right-click the HTML page you just created and click **Set as Start Page**.</span></span>
11. <span data-ttu-id="c90fc-159">Zastąp następujący kod w kodzie domyślnym na stronie HTML.</span><span class="sxs-lookup"><span data-stu-id="c90fc-159">Replace the default code in the HTML page with the following code.</span></span>

    [!code-html[Main](tutorial-getting-started-with-signalr/samples/sample4.html)]
12. <span data-ttu-id="c90fc-160">**Zapisz wszystkie** dla projektu.</span><span class="sxs-lookup"><span data-stu-id="c90fc-160">**Save All** for the project.</span></span>

<a id="run"></a>

## <a name="run-the-sample"></a><span data-ttu-id="c90fc-161">Uruchom próbki</span><span class="sxs-lookup"><span data-stu-id="c90fc-161">Run the Sample</span></span>

1. <span data-ttu-id="c90fc-162">Naciśnij klawisz F5, aby uruchomić projekt w trybie debugowania.</span><span class="sxs-lookup"><span data-stu-id="c90fc-162">Press F5 to run the project in debug mode.</span></span> <span data-ttu-id="c90fc-163">Ładuje strony HTML w wystąpieniu przeglądarki i wyświetla monit o nazwę użytkownika.</span><span class="sxs-lookup"><span data-stu-id="c90fc-163">The HTML page loads in a browser instance and prompts for a user name.</span></span>

    ![Wprowadź nazwę użytkownika](tutorial-getting-started-with-signalr/_static/image5.png)
2. <span data-ttu-id="c90fc-165">Wprowadź nazwę użytkownika.</span><span class="sxs-lookup"><span data-stu-id="c90fc-165">Enter a user name.</span></span>
3. <span data-ttu-id="c90fc-166">Skopiuj adres URL w wierszu adresu przeglądarki i używać go otworzyć dwa kolejne wystąpienia przeglądarki.</span><span class="sxs-lookup"><span data-stu-id="c90fc-166">Copy the URL from the address line of the browser and use it to open two more browser instances.</span></span> <span data-ttu-id="c90fc-167">W każdym wystąpieniu przeglądarki wprowadź unikatową nazwę użytkownika.</span><span class="sxs-lookup"><span data-stu-id="c90fc-167">In each browser instance, enter a unique user name.</span></span>
4. <span data-ttu-id="c90fc-168">W każdym wystąpieniu przeglądarki dodać komentarz, a następnie kliknij przycisk **wysyłania**.</span><span class="sxs-lookup"><span data-stu-id="c90fc-168">In each browser instance, add a comment and click **Send**.</span></span> <span data-ttu-id="c90fc-169">Komentarze powinien być wyświetlany we wszystkich wystąpieniach przeglądarki.</span><span class="sxs-lookup"><span data-stu-id="c90fc-169">The comments should display in all browser instances.</span></span>

    > [!NOTE]
    > <span data-ttu-id="c90fc-170">Tej aplikacji rozmów proste kontekstu dyskusji na serwerze nie są zachowywane.</span><span class="sxs-lookup"><span data-stu-id="c90fc-170">This simple chat application does not maintain the discussion context on the server.</span></span> <span data-ttu-id="c90fc-171">Koncentrator emituje komentarze do wszystkich bieżących użytkowników.</span><span class="sxs-lookup"><span data-stu-id="c90fc-171">The hub broadcasts comments to all current users.</span></span> <span data-ttu-id="c90fc-172">Użytkownicy, którzy później dołączyć rozmowę zobaczą wiadomości dodane od czasu dołączenia.</span><span class="sxs-lookup"><span data-stu-id="c90fc-172">Users who join the chat later will see messages added from the time they join.</span></span>

    <span data-ttu-id="c90fc-173">Poniższy zrzut ekranu przedstawia rozmów aplikacja była uruchomiona w trzech przypadkach przeglądarki, które są aktualizowane w jedno wystąpienie wysyła komunikat:</span><span class="sxs-lookup"><span data-stu-id="c90fc-173">The following screen shot shows the chat application running in three browser instances, all of which are updated when one instance sends a message:</span></span>

    ![Rozmowa przeglądarki](tutorial-getting-started-with-signalr/_static/image6.png)
5. <span data-ttu-id="c90fc-175">W **Eksploratora rozwiązań**, sprawdź **dokumentów skryptu** węzła dla działającej aplikacji.</span><span class="sxs-lookup"><span data-stu-id="c90fc-175">In **Solution Explorer**, inspect the **Script Documents** node for the running application.</span></span> <span data-ttu-id="c90fc-176">Brak pliku skryptu o nazwie **koncentratory** generujący biblioteki SignalR dynamicznie w czasie wykonywania.</span><span class="sxs-lookup"><span data-stu-id="c90fc-176">There is a script file named **hubs** that the SignalR library dynamically generates at runtime.</span></span> <span data-ttu-id="c90fc-177">Ten plik zarządza komunikacji między skryptu jQuery i kod po stronie serwera.</span><span class="sxs-lookup"><span data-stu-id="c90fc-177">This file manages the communication between jQuery script and server-side code.</span></span>

    ![Wygenerowany Centrum skryptów](tutorial-getting-started-with-signalr/_static/image7.png)

<a id="code"></a>

## <a name="examine-the-code"></a><span data-ttu-id="c90fc-179">Sprawdź kod</span><span class="sxs-lookup"><span data-stu-id="c90fc-179">Examine the Code</span></span>

<span data-ttu-id="c90fc-180">SignalR aplikacji czatu pokazano dwa podstawowe SignalR zadań związanych z projektowaniem: tworzenie koncentratora jako obiekt główny koordynacji na serwerze i używanie biblioteki jQuery SignalR do wysyłania i odbierania wiadomości.</span><span class="sxs-lookup"><span data-stu-id="c90fc-180">The SignalR chat application demonstrates two basic SignalR development tasks: creating a hub as the main coordination object on the server, and using the SignalR jQuery library to send and receive messages.</span></span>

### <a name="signalr-hubs"></a><span data-ttu-id="c90fc-181">Koncentratory SignalR</span><span class="sxs-lookup"><span data-stu-id="c90fc-181">SignalR Hubs</span></span>

<span data-ttu-id="c90fc-182">W przykładowym kodzie **ChatHub** pochodną klasy **Microsoft.AspNet.SignalR.Hub** klasy.</span><span class="sxs-lookup"><span data-stu-id="c90fc-182">In the code sample the **ChatHub** class derives from the **Microsoft.AspNet.SignalR.Hub** class.</span></span> <span data-ttu-id="c90fc-183">Wyprowadzanie z **Centrum** klasy jest to wygodny sposób utworzyć aplikację SignalR.</span><span class="sxs-lookup"><span data-stu-id="c90fc-183">Deriving from the **Hub** class is a useful way to build a SignalR application.</span></span> <span data-ttu-id="c90fc-184">Można utworzyć metody publiczne na klasy koncentratora, a następnie te metody dostępu wywołując ze skryptów jQuery na stronie sieci web.</span><span class="sxs-lookup"><span data-stu-id="c90fc-184">You can create public methods on your hub class and then access those methods by calling them from jQuery scripts in a web page.</span></span>

<span data-ttu-id="c90fc-185">W kodzie rozmów wywołać klientów **ChatHub.Send** metody do wysłania nowej wiadomości.</span><span class="sxs-lookup"><span data-stu-id="c90fc-185">In the chat code, clients call the **ChatHub.Send** method to send a new message.</span></span> <span data-ttu-id="c90fc-186">Koncentrator z kolei wysyła wiadomość do wszystkich klientów przez wywołanie metody **Clients.All.broadcastMessage**.</span><span class="sxs-lookup"><span data-stu-id="c90fc-186">The hub in turn sends the message to all clients by calling **Clients.All.broadcastMessage**.</span></span>

<span data-ttu-id="c90fc-187">**Wysyłania** metody przedstawiono kilka pojęć Centrum:</span><span class="sxs-lookup"><span data-stu-id="c90fc-187">The **Send** method demonstrates several hub concepts :</span></span>

- <span data-ttu-id="c90fc-188">Metody publiczne należy zadeklarować w koncentratorze tak, aby klienci mogą połączeń telefonicznych z nimi.</span><span class="sxs-lookup"><span data-stu-id="c90fc-188">Declare public methods on a hub so that clients can call them.</span></span>
- <span data-ttu-id="c90fc-189">Użyj **Microsoft.AspNet.SignalR.Hub.Clients** właściwość dynamiczna ma dostęp do wszystkich klientów podłączone do tego koncentratora.</span><span class="sxs-lookup"><span data-stu-id="c90fc-189">Use the **Microsoft.AspNet.SignalR.Hub.Clients** dynamic property to access all clients connected to this hub.</span></span>
- <span data-ttu-id="c90fc-190">Wywoływanie funkcji jQuery na kliencie (takich jak `broadcastMessage` funkcji) aby zaktualizować klientów.</span><span class="sxs-lookup"><span data-stu-id="c90fc-190">Call a jQuery function on the client (such as the `broadcastMessage` function) to update clients.</span></span>

    [!code-csharp[Main](tutorial-getting-started-with-signalr/samples/sample5.cs)]

### <a name="signalr-and-jquery"></a><span data-ttu-id="c90fc-191">SignalR i jQuery</span><span class="sxs-lookup"><span data-stu-id="c90fc-191">SignalR and jQuery</span></span>

<span data-ttu-id="c90fc-192">Strony HTML w przykładowy kod przedstawia sposób użycia biblioteki jQuery SignalR do komunikowania się przy użyciu koncentratora SignalR.</span><span class="sxs-lookup"><span data-stu-id="c90fc-192">The HTML page in the code sample shows how to use the SignalR jQuery library to communicate with a SignalR hub.</span></span> <span data-ttu-id="c90fc-193">Podstawowe zadania w kodzie są deklarowanie serwer proxy, aby odwołać koncentratora, deklarowanie funkcję serwera można wywołać w celu wypychania zawartości do klientów, a następnie uruchomić połączenie do wysłania wiadomości do koncentratora.</span><span class="sxs-lookup"><span data-stu-id="c90fc-193">The essential tasks in the code are declaring a proxy to reference the hub, declaring a function that the server can call to push content to clients, and starting a connection to send messages to the hub.</span></span>

<span data-ttu-id="c90fc-194">Poniższy kod deklaruje serwer proxy koncentratora.</span><span class="sxs-lookup"><span data-stu-id="c90fc-194">The following code declares a proxy for a hub.</span></span>

[!code-javascript[Main](tutorial-getting-started-with-signalr/samples/sample6.js)]

> [!NOTE]
> <span data-ttu-id="c90fc-195">Odwołanie do klasy serwera i jej elementów członkowskich w jQuery znajduje camelcase.</span><span class="sxs-lookup"><span data-stu-id="c90fc-195">In jQuery the reference to the server class and its members is in camel case.</span></span> <span data-ttu-id="c90fc-196">Przykładowy kod odwołuje się do języka C# **ChatHub** klasy w jQuery jako **chatHub**.</span><span class="sxs-lookup"><span data-stu-id="c90fc-196">The code sample references the C# **ChatHub** class in jQuery as **chatHub**.</span></span>


<span data-ttu-id="c90fc-197">Następujący kod jest, jak utworzyć funkcję wywołania zwrotnego w skrypcie.</span><span class="sxs-lookup"><span data-stu-id="c90fc-197">The following code is how you create a callback function in the script.</span></span> <span data-ttu-id="c90fc-198">Klasy koncentratora na serwerze wywołanie tej funkcji, aby wysyłać aktualizacje zawartości do każdego klienta.</span><span class="sxs-lookup"><span data-stu-id="c90fc-198">The hub class on the server calls this function to push content updates to each client.</span></span> <span data-ttu-id="c90fc-199">Dwa wiersze, czy kodowanie HTML zawartości przed wyświetleniem go są opcjonalne i Pokaż prosty sposób, aby uniemożliwić uruchomienie skryptu.</span><span class="sxs-lookup"><span data-stu-id="c90fc-199">The two lines that HTML encode the content before displaying it are optional and show a simple way to prevent script injection.</span></span>

[!code-html[Main](tutorial-getting-started-with-signalr/samples/sample7.html)]

<span data-ttu-id="c90fc-200">Poniższy kod przedstawia sposób nawiązać połączenie z koncentratorem.</span><span class="sxs-lookup"><span data-stu-id="c90fc-200">The following code shows how to open a connection with the hub.</span></span> <span data-ttu-id="c90fc-201">Kod rozpoczyna połączenie, a następnie przekazuje on funkcję obsługi zdarzenia kliknij na **wysyłania** przycisku na stronie HTML.</span><span class="sxs-lookup"><span data-stu-id="c90fc-201">The code starts the connection and then passes it a function to handle the click event on the **Send** button in the HTML page.</span></span>

> [!NOTE]
> <span data-ttu-id="c90fc-202">Takie podejście temu, że połączenie zostanie nawiązane, przed wykonaniem programu obsługi zdarzeń.</span><span class="sxs-lookup"><span data-stu-id="c90fc-202">This approach insures that the connection is established before the event handler executes.</span></span>


[!code-javascript[Main](tutorial-getting-started-with-signalr/samples/sample8.js)]

<a id="next"></a>

## <a name="next-steps"></a><span data-ttu-id="c90fc-203">Następne kroki</span><span class="sxs-lookup"><span data-stu-id="c90fc-203">Next Steps</span></span>

<span data-ttu-id="c90fc-204">Wiesz, że SignalR to struktura służąca do tworzenia aplikacji sieci web w czasie rzeczywistym.</span><span class="sxs-lookup"><span data-stu-id="c90fc-204">You learned that SignalR is a framework for building real-time web applications.</span></span> <span data-ttu-id="c90fc-205">Przedstawiono również kilka zadań związanych z projektowaniem SignalR: jak dodać do aplikacji ASP.NET SignalR, Tworzenie klasy koncentratora oraz sposobu wysyłania i odbierania wiadomości z koncentratora.</span><span class="sxs-lookup"><span data-stu-id="c90fc-205">You also learned several SignalR development tasks: how to add SignalR to an ASP.NET application, how to create a hub class, and how to send and receive messages from the hub.</span></span>

<span data-ttu-id="c90fc-206">Można udostępnić przykładowej aplikacji w tym samouczku lub innych aplikacji SignalR za pośrednictwem Internetu przez wdrożenie ich do dostawcy hostingu.</span><span class="sxs-lookup"><span data-stu-id="c90fc-206">You can make the sample application in this tutorial or other SignalR applications available over the Internet by deploying them to a hosting provider.</span></span> <span data-ttu-id="c90fc-207">Firma Microsoft oferuje usługi hostingu sieci web wolnego maksymalnie 10 witryn sieci web w bezpłatny [systemu Windows Azure, konto próbne](https://www.windowsazure.com/pricing/free-trial/?WT.mc_id=A443DD604).</span><span class="sxs-lookup"><span data-stu-id="c90fc-207">Microsoft offers free web hosting for up to 10 web sites in a free [Windows Azure trial account](https://www.windowsazure.com/pricing/free-trial/?WT.mc_id=A443DD604).</span></span> <span data-ttu-id="c90fc-208">Aby uzyskać wskazówki dotyczące sposobu wdrażania przykładowej aplikacji SignalR, zobacz [publikowania SignalR pobieranie rozpoczęto próbki jako witryny sieci Web systemu Windows Azure](https://blogs.msdn.com/b/timlee/archive/2013/02/27/deploy-the-signalr-getting-started-sample-as-a-windows-azure-web-site.aspx).</span><span class="sxs-lookup"><span data-stu-id="c90fc-208">For a walkthrough on how to deploy the sample SignalR application, see [Publish the SignalR Getting Started Sample as a Windows Azure Web Site](https://blogs.msdn.com/b/timlee/archive/2013/02/27/deploy-the-signalr-getting-started-sample-as-a-windows-azure-web-site.aspx).</span></span> <span data-ttu-id="c90fc-209">Aby uzyskać szczegółowe informacje o sposobie wdrażania projektu sieci web programu Visual Studio do witryny sieci Web systemu Windows Azure, zobacz [wdrażanie aplikacji ASP.NET do witryny sieci Web systemu Windows Azure](https://docs.microsoft.com/azure/app-service-web/app-service-web-get-started-dotnet).</span><span class="sxs-lookup"><span data-stu-id="c90fc-209">For detailed information about how to deploy a Visual Studio web project to a Windows Azure Web Site, see [Deploying an ASP.NET Application to a Windows Azure Web Site](https://docs.microsoft.com/azure/app-service-web/app-service-web-get-started-dotnet).</span></span> <span data-ttu-id="c90fc-210">(Uwaga: transportu protokołu WebSocket nie jest obecnie obsługiwana dla witryn sieci Web systemu Windows Azure.</span><span class="sxs-lookup"><span data-stu-id="c90fc-210">(Note: The WebSocket transport is not currently supported for Windows Azure Web Sites.</span></span> <span data-ttu-id="c90fc-211">Protokół WebSocket podczas transportu nie jest dostępna, SignalR używa innych dostępnych transportów zgodnie z opisem w sekcji transportów [wprowadzenie do tematu SignalR](index.md).)</span><span class="sxs-lookup"><span data-stu-id="c90fc-211">When WebSocket transport is not available, SignalR uses the other available transports as described in the Transports section of the [Introduction to SignalR topic](index.md).)</span></span>

<span data-ttu-id="c90fc-212">Aby uzyskać bardziej zaawansowane pojęcia rozwój SignalR, odwiedź następującą witrynę SignalR kod źródłowy i zasobów:</span><span class="sxs-lookup"><span data-stu-id="c90fc-212">To learn more advanced SignalR developments concepts, visit the following sites for SignalR source code and resources:</span></span>

- [<span data-ttu-id="c90fc-213">Projekt SignalR</span><span class="sxs-lookup"><span data-stu-id="c90fc-213">SignalR Project</span></span>](http://signalr.net)
- [<span data-ttu-id="c90fc-214">SignalR Github i przykłady</span><span class="sxs-lookup"><span data-stu-id="c90fc-214">SignalR Github and Samples</span></span>](https://github.com/SignalR/SignalR)
- [<span data-ttu-id="c90fc-215">Witryna typu Wiki biblioteki SignalR</span><span class="sxs-lookup"><span data-stu-id="c90fc-215">SignalR Wiki</span></span>](https://github.com/SignalR/SignalR/wiki)
