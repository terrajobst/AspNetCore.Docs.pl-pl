---
uid: signalr/overview/older-versions/tutorial-getting-started-with-signalr
title: 'Samouczek: Wprowadzenie do SignalR 1.x | Dokumentacja firmy Microsoft'
author: pfletcher
description: Użyj biblioteki SignalR platformy ASP.NET, aby utworzyć aplikację do rozmów w czasie rzeczywistym na stronie HTML.
ms.author: aspnetcontent
ms.date: 02/18/2013
ms.assetid: fdc3599a-5217-44c1-951f-0eec9812dce7
msc.legacyurl: /signalr/overview/older-versions/tutorial-getting-started-with-signalr
msc.type: authoredcontent
ms.openlocfilehash: 13d33ff7e3cfff996a9849cfccfcc43754c8234e
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/05/2018
ms.locfileid: "37838630"
---
<a name="tutorial-getting-started-with-signalr-1x"></a><span data-ttu-id="bdf3a-103">Samouczek: Wprowadzenie do SignalR 1.x</span><span class="sxs-lookup"><span data-stu-id="bdf3a-103">Tutorial: Getting Started with SignalR 1.x</span></span>
====================
<span data-ttu-id="bdf3a-104">przez [Patrick Fletcher](https://github.com/pfletcher), [Tim Teebken](https://github.com/timlt)</span><span class="sxs-lookup"><span data-stu-id="bdf3a-104">by [Patrick Fletcher](https://github.com/pfletcher), [Tim Teebken](https://github.com/timlt)</span></span>

> <span data-ttu-id="bdf3a-105">Ten samouczek pokazuje, jak utworzyć aplikację do obsługi rozmów w czasie rzeczywistym przy użyciu SignalR.</span><span class="sxs-lookup"><span data-stu-id="bdf3a-105">This tutorial shows how to use SignalR to create a real-time chat application.</span></span> <span data-ttu-id="bdf3a-106">Będzie dodać SignalR do pustych aplikacji sieci web ASP.NET i Utwórz stronę HTML do wysyłania i wyświetla komunikaty.</span><span class="sxs-lookup"><span data-stu-id="bdf3a-106">You will add SignalR to an empty ASP.NET web application and create an HTML page to send and display messages.</span></span>


## <a name="overview"></a><span data-ttu-id="bdf3a-107">Omówienie</span><span class="sxs-lookup"><span data-stu-id="bdf3a-107">Overview</span></span>

<span data-ttu-id="bdf3a-108">W tym samouczku przedstawiono rozwoju SignalR, pokazując, jak utworzyć aplikację proste rozmowy opartej na przeglądarce.</span><span class="sxs-lookup"><span data-stu-id="bdf3a-108">This tutorial introduces SignalR development by showing how to build a simple browser-based chat application.</span></span> <span data-ttu-id="bdf3a-109">Będzie dodać biblioteki SignalR do pustą aplikację sieci web platformy ASP.NET, Utwórz klasę hub wysyłanie komunikatów do klientów i Utwórz stronę HTML, który umożliwia użytkownikom wysyłanie i odbieranie wiadomości rozmowy.</span><span class="sxs-lookup"><span data-stu-id="bdf3a-109">You will add the SignalR library to an empty ASP.NET web application, create a hub class for sending messages to clients, and create an HTML page that lets users send and receive chat messages.</span></span> <span data-ttu-id="bdf3a-110">Z podobnego samouczka dotyczącego pokazującym, jak utworzyć aplikację do obsługi rozmów w MVC 4 przy użyciu widoku MVC, zobacz [wprowadzenie do SignalR i MVC 4](index.md).</span><span class="sxs-lookup"><span data-stu-id="bdf3a-110">For a similar tutorial that shows how to create a chat application in MVC 4 using an MVC view, see [Getting Started with SignalR and MVC 4](index.md).</span></span>

> [!NOTE]
> <span data-ttu-id="bdf3a-111">W tym samouczku korzysta z wersji release (1.x) SignalR.</span><span class="sxs-lookup"><span data-stu-id="bdf3a-111">This tutorial uses the release (1.x) version of SignalR.</span></span> <span data-ttu-id="bdf3a-112">Aby uzyskać szczegółowe informacje na temat zmian między SignalR 1.x i 2.0, zobacz [projektów uaktualnianie SignalR 1.x](../releases/upgrading-signalr-1x-projects-to-20.md).</span><span class="sxs-lookup"><span data-stu-id="bdf3a-112">For details on changes between SignalR 1.x and 2.0, see [Upgrading SignalR 1.x Projects](../releases/upgrading-signalr-1x-projects-to-20.md).</span></span>

<span data-ttu-id="bdf3a-113">SignalR to biblioteki .NET typu open source do tworzenia aplikacji internetowych, które wymagają interakcji z użytkownikiem na żywo lub aktualizacji danych w czasie rzeczywistym.</span><span class="sxs-lookup"><span data-stu-id="bdf3a-113">SignalR is an open-source .NET library for building web applications that require live user interaction or real-time data updates.</span></span> <span data-ttu-id="bdf3a-114">Przykłady obejmują społecznościowe aplikacje, gry wielodostępnym, biznesowych współpracę i w nowościach, pogody lub finansowych aktualizacji aplikacji.</span><span class="sxs-lookup"><span data-stu-id="bdf3a-114">Examples include social applications, multiuser games, business collaboration, and news, weather, or financial update applications.</span></span> <span data-ttu-id="bdf3a-115">Są one często nazywane aplikacji w czasie rzeczywistym.</span><span class="sxs-lookup"><span data-stu-id="bdf3a-115">These are often called real-time applications.</span></span>

<span data-ttu-id="bdf3a-116">SignalR upraszcza proces tworzenia aplikacji w czasie rzeczywistym.</span><span class="sxs-lookup"><span data-stu-id="bdf3a-116">SignalR simplifies the process of building real-time applications.</span></span> <span data-ttu-id="bdf3a-117">Obejmuje ona bibliotekę serwera programu ASP.NET oraz bibliotekę kliencką JavaScript, aby ułatwić zarządzanie połączeniami klient serwer i wypychanie aktualizacji zawartości dla klientów.</span><span class="sxs-lookup"><span data-stu-id="bdf3a-117">It includes an ASP.NET server library and a JavaScript client library to make it easier to manage client-server connections and push content updates to clients.</span></span> <span data-ttu-id="bdf3a-118">Biblioteki SignalR można dodać do istniejącej aplikacji ASP.NET do uzyskania funkcji w czasie rzeczywistym.</span><span class="sxs-lookup"><span data-stu-id="bdf3a-118">You can add the SignalR library to an existing ASP.NET application to gain real-time functionality.</span></span>

<span data-ttu-id="bdf3a-119">Samouczek przedstawia następujące zadania deweloperskie SignalR:</span><span class="sxs-lookup"><span data-stu-id="bdf3a-119">The tutorial demonstrates the following SignalR development tasks:</span></span>

- <span data-ttu-id="bdf3a-120">Dodawanie biblioteki SignalR do aplikacji sieci web ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="bdf3a-120">Adding the SignalR library to an ASP.NET web application.</span></span>
- <span data-ttu-id="bdf3a-121">Tworzenie klasy koncentratora, aby wypchnąć zawartości do klientów.</span><span class="sxs-lookup"><span data-stu-id="bdf3a-121">Creating a hub class to push content to clients.</span></span>
- <span data-ttu-id="bdf3a-122">Przy użyciu biblioteki jQuery SignalR na stronie sieci web umożliwiają przesyłanie komunikatów oraz wyświetlania aktualizacji z koncentratora.</span><span class="sxs-lookup"><span data-stu-id="bdf3a-122">Using the SignalR jQuery library in a web page to send messages and display updates from the hub.</span></span>

<span data-ttu-id="bdf3a-123">Poniższy zrzut ekranu przedstawia aplikacji rozmów w przeglądarce.</span><span class="sxs-lookup"><span data-stu-id="bdf3a-123">The following screen shot shows the chat application running in a browser.</span></span> <span data-ttu-id="bdf3a-124">Każdy nowy użytkownik może publikować komentarze i zobacz komentarze dodane po użytkownik nie przyłączy czatu.</span><span class="sxs-lookup"><span data-stu-id="bdf3a-124">Each new user can post comments and see comments added after the user joins the chat.</span></span>

![Wystąpienia rozmowy](tutorial-getting-started-with-signalr/_static/image1.png)

<span data-ttu-id="bdf3a-126">Sekcje:</span><span class="sxs-lookup"><span data-stu-id="bdf3a-126">Sections:</span></span>

- [<span data-ttu-id="bdf3a-127">Konfigurowanie projektu</span><span class="sxs-lookup"><span data-stu-id="bdf3a-127">Set up the Project</span></span>](#setup)
- [<span data-ttu-id="bdf3a-128">Uruchamianie aplikacji przykładowej</span><span class="sxs-lookup"><span data-stu-id="bdf3a-128">Run the Sample</span></span>](#run)
- [<span data-ttu-id="bdf3a-129">Poszukaj w kodzie</span><span class="sxs-lookup"><span data-stu-id="bdf3a-129">Examine the Code</span></span>](#code)
- [<span data-ttu-id="bdf3a-130">Następne kroki</span><span class="sxs-lookup"><span data-stu-id="bdf3a-130">Next Steps</span></span>](#next)

<a id="setup"></a>

## <a name="set-up-the-project"></a><span data-ttu-id="bdf3a-131">Konfigurowanie projektu</span><span class="sxs-lookup"><span data-stu-id="bdf3a-131">Set up the Project</span></span>

<span data-ttu-id="bdf3a-132">W tej sekcji pokazano, jak utworzyć pustą aplikację sieci web platformy ASP.NET, Dodaj SignalR i tworzenie aplikacji czatu.</span><span class="sxs-lookup"><span data-stu-id="bdf3a-132">This section shows how to create an empty ASP.NET web application, add SignalR, and create the chat application.</span></span>

<span data-ttu-id="bdf3a-133">Wymagania wstępne:</span><span class="sxs-lookup"><span data-stu-id="bdf3a-133">Prerequisites:</span></span>

- <span data-ttu-id="bdf3a-134">Visual Studio 2010 z dodatkiem SP1 lub 2012.</span><span class="sxs-lookup"><span data-stu-id="bdf3a-134">Visual Studio 2010 SP1 or 2012.</span></span> <span data-ttu-id="bdf3a-135">Jeśli nie masz programu Visual Studio, zobacz [ASP.NET pliki do pobrania](https://www.asp.net/downloads) można pobrać bezpłatny program Visual Studio 2012 Express narzędzia programistyczne.</span><span class="sxs-lookup"><span data-stu-id="bdf3a-135">If you do not have Visual Studio, see [ASP.NET Downloads](https://www.asp.net/downloads) to get the free Visual Studio 2012 Express Development Tool.</span></span>
- <span data-ttu-id="bdf3a-136">[Microsoft ASP.NET and Web Tools 2012.2](https://go.microsoft.com/fwlink/?LinkId=279941).</span><span class="sxs-lookup"><span data-stu-id="bdf3a-136">[Microsoft ASP.NET and Web Tools 2012.2](https://go.microsoft.com/fwlink/?LinkId=279941).</span></span> <span data-ttu-id="bdf3a-137">Dla programu Visual Studio 2012 Instalator dodaje nowe funkcje platformy ASP.NET, w tym szablony SignalR do programu Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="bdf3a-137">For Visual Studio 2012, this installer adds new ASP.NET features including SignalR templates to Visual Studio.</span></span> <span data-ttu-id="bdf3a-138">Dla programu Visual Studio 2010 z dodatkiem SP1 Instalator nie jest dostępny, ale zakończysz pracę z samouczkiem, instalując pakiet NuGet biblioteki SignalR, zgodnie z opisem w ramach kroków konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="bdf3a-138">For Visual Studio 2010 SP1, an installer is not available but you can complete the tutorial by installing the SignalR NuGet package as described in the setup steps.</span></span>

<span data-ttu-id="bdf3a-139">Poniższe kroki Użyj programu Visual Studio 2012, aby utworzyć pustą aplikację sieci Web platformy ASP.NET i dodać biblioteki SignalR:</span><span class="sxs-lookup"><span data-stu-id="bdf3a-139">The following steps use Visual Studio 2012 to create an ASP.NET Empty Web Application and add the SignalR library:</span></span>

1. <span data-ttu-id="bdf3a-140">W programie Visual Studio Utwórz pustą aplikację sieci Web platformy ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="bdf3a-140">In Visual Studio create an ASP.NET Empty Web Application.</span></span>

    ![Tworzenie pustego sieci web](tutorial-getting-started-with-signalr/_static/image2.png)
2. <span data-ttu-id="bdf3a-142">Otwórz **Konsola Menedżera pakietów** , wybierając **narzędzia | Menedżer pakietów biblioteki | Konsola Menedżera pakietów**.</span><span class="sxs-lookup"><span data-stu-id="bdf3a-142">Open the **Package Manager Console** by selecting **Tools | Library Package Manager | Package Manager Console**.</span></span> <span data-ttu-id="bdf3a-143">Wprowadź następujące polecenie w oknie konsoli:</span><span class="sxs-lookup"><span data-stu-id="bdf3a-143">Enter the following command into the console window:</span></span>

    `Install-Package Microsoft.AspNet.SignalR -Version 1.1.3`

    <span data-ttu-id="bdf3a-144">To polecenie powoduje zainstalowanie najnowszej wersji biblioteki SignalR 1.x.</span><span class="sxs-lookup"><span data-stu-id="bdf3a-144">This command installs the latest version of SignalR 1.x.</span></span>
3. <span data-ttu-id="bdf3a-145">W **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy projekt, wybierz **Dodaj | Klasa**.</span><span class="sxs-lookup"><span data-stu-id="bdf3a-145">In **Solution Explorer**, right-click the project, select **Add | Class**.</span></span> <span data-ttu-id="bdf3a-146">Nadaj nowej klasie **ChatHub**.</span><span class="sxs-lookup"><span data-stu-id="bdf3a-146">Name the new class **ChatHub**.</span></span>
4. <span data-ttu-id="bdf3a-147">W **Eksploratora rozwiązań** rozwiń węzeł skryptów.</span><span class="sxs-lookup"><span data-stu-id="bdf3a-147">In **Solution Explorer** expand the Scripts node.</span></span> <span data-ttu-id="bdf3a-148">Biblioteki skryptów, jQuery i SignalR są widoczne w projekcie.</span><span class="sxs-lookup"><span data-stu-id="bdf3a-148">Script libraries for jQuery and SignalR are visible in the project.</span></span>

    ![Odwołań do biblioteki](tutorial-getting-started-with-signalr/_static/image3.png)
5. <span data-ttu-id="bdf3a-150">Zastąp kod w **ChatHub** klasy z następującym kodem.</span><span class="sxs-lookup"><span data-stu-id="bdf3a-150">Replace the code in the **ChatHub** class with the following code.</span></span>

    [!code-csharp[Main](tutorial-getting-started-with-signalr/samples/sample1.cs)]
6. <span data-ttu-id="bdf3a-151">W **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy projekt, a następnie kliknij przycisk **Dodaj | Nowy element**.</span><span class="sxs-lookup"><span data-stu-id="bdf3a-151">In **Solution Explorer**, right-click the project, then click **Add | New Item**.</span></span> <span data-ttu-id="bdf3a-152">W **Dodaj nowy element** okno dialogowe, wybierz opcję **globalna klasa aplikacji** i kliknij przycisk **Dodaj**.</span><span class="sxs-lookup"><span data-stu-id="bdf3a-152">In the **Add New Item** dialog, select **Global Application Class** and click **Add**.</span></span>

    ![Dodaj globalne](tutorial-getting-started-with-signalr/_static/image4.png)
7. <span data-ttu-id="bdf3a-154">Dodaj następujący kod `using` instrukcji po podane `using` instrukcji w klasie Global.asax.cs.</span><span class="sxs-lookup"><span data-stu-id="bdf3a-154">Add the following `using` statements after the provided `using` statements in the Global.asax.cs class.</span></span>

    [!code-csharp[Main](tutorial-getting-started-with-signalr/samples/sample2.cs)]
8. <span data-ttu-id="bdf3a-155">Dodaj następujący wiersz kodu w `Application_Start` metody klasy globalny można zarejestrować domyślną trasę dla centrów SignalR.</span><span class="sxs-lookup"><span data-stu-id="bdf3a-155">Add the following line of code in the `Application_Start` method of the Global class to register the default route for SignalR hubs.</span></span>

    [!code-csharp[Main](tutorial-getting-started-with-signalr/samples/sample3.cs)]
9. <span data-ttu-id="bdf3a-156">W **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy projekt, a następnie kliknij przycisk **Dodaj | Nowy element**.</span><span class="sxs-lookup"><span data-stu-id="bdf3a-156">In **Solution Explorer**, right-click the project, then click **Add | New Item**.</span></span> <span data-ttu-id="bdf3a-157">W **Dodaj nowy element** okno dialogowe, wybierz stronę Html i kliknij przycisk **Dodaj**.</span><span class="sxs-lookup"><span data-stu-id="bdf3a-157">In the **Add New Item** dialog, select Html Page and click **Add**.</span></span>
10. <span data-ttu-id="bdf3a-158">W **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy właśnie utworzony strony HTML i kliknij przycisk **Ustaw jako strona startowa**.</span><span class="sxs-lookup"><span data-stu-id="bdf3a-158">In **Solution Explorer**, right-click the HTML page you just created and click **Set as Start Page**.</span></span>
11. <span data-ttu-id="bdf3a-159">Zastąp kod domyślnej strony HTML z następującym kodem.</span><span class="sxs-lookup"><span data-stu-id="bdf3a-159">Replace the default code in the HTML page with the following code.</span></span>

    [!code-html[Main](tutorial-getting-started-with-signalr/samples/sample4.html)]
12. <span data-ttu-id="bdf3a-160">**Zapisz wszystko** dla projektu.</span><span class="sxs-lookup"><span data-stu-id="bdf3a-160">**Save All** for the project.</span></span>

<a id="run"></a>

## <a name="run-the-sample"></a><span data-ttu-id="bdf3a-161">Uruchamianie aplikacji przykładowej</span><span class="sxs-lookup"><span data-stu-id="bdf3a-161">Run the Sample</span></span>

1. <span data-ttu-id="bdf3a-162">Naciśnij klawisz F5, aby uruchomić projekt w trybie debugowania.</span><span class="sxs-lookup"><span data-stu-id="bdf3a-162">Press F5 to run the project in debug mode.</span></span> <span data-ttu-id="bdf3a-163">Strona HTML ładuje się w wystąpieniu przeglądarki i wyświetla monit o nazwę użytkownika.</span><span class="sxs-lookup"><span data-stu-id="bdf3a-163">The HTML page loads in a browser instance and prompts for a user name.</span></span>

    ![Wprowadź nazwę użytkownika](tutorial-getting-started-with-signalr/_static/image5.png)
2. <span data-ttu-id="bdf3a-165">Wprowadź nazwę użytkownika.</span><span class="sxs-lookup"><span data-stu-id="bdf3a-165">Enter a user name.</span></span>
3. <span data-ttu-id="bdf3a-166">Skopiuj adres URL w wierszu adresu przeglądarki i użyj go, aby otworzyć dwóch większej liczby wystąpień przeglądarki.</span><span class="sxs-lookup"><span data-stu-id="bdf3a-166">Copy the URL from the address line of the browser and use it to open two more browser instances.</span></span> <span data-ttu-id="bdf3a-167">W każdym wystąpieniu przeglądarki wprowadź unikatową nazwę użytkownika.</span><span class="sxs-lookup"><span data-stu-id="bdf3a-167">In each browser instance, enter a unique user name.</span></span>
4. <span data-ttu-id="bdf3a-168">W każdym wystąpieniu przeglądarki Dodaj komentarz, a następnie kliknij przycisk **wysyłania**.</span><span class="sxs-lookup"><span data-stu-id="bdf3a-168">In each browser instance, add a comment and click **Send**.</span></span> <span data-ttu-id="bdf3a-169">Komentarze powinien być wyświetlany we wszystkich wystąpieniach przeglądarki.</span><span class="sxs-lookup"><span data-stu-id="bdf3a-169">The comments should display in all browser instances.</span></span>

    > [!NOTE]
    > <span data-ttu-id="bdf3a-170">Tej aplikacji rozmów prostego nie utrzymuje kontekst dyskusji na serwerze.</span><span class="sxs-lookup"><span data-stu-id="bdf3a-170">This simple chat application does not maintain the discussion context on the server.</span></span> <span data-ttu-id="bdf3a-171">Centrum emituje komentarzy do wszystkich bieżących użytkowników.</span><span class="sxs-lookup"><span data-stu-id="bdf3a-171">The hub broadcasts comments to all current users.</span></span> <span data-ttu-id="bdf3a-172">Użytkownicy, którzy dołączają do czatu później zostanie wyświetlony komunikat o dodane od czasu dołączenia.</span><span class="sxs-lookup"><span data-stu-id="bdf3a-172">Users who join the chat later will see messages added from the time they join.</span></span>

    <span data-ttu-id="bdf3a-173">Poniższy zrzut ekranu przedstawia aplikacji rozmów w trzech wystąpień przeglądarki, z których wszystkie są aktualizowane w jedno wystąpienie jest wysyłana wiadomość:</span><span class="sxs-lookup"><span data-stu-id="bdf3a-173">The following screen shot shows the chat application running in three browser instances, all of which are updated when one instance sends a message:</span></span>

    ![Rozmowa przeglądarki](tutorial-getting-started-with-signalr/_static/image6.png)
5. <span data-ttu-id="bdf3a-175">W **Eksploratora rozwiązań**, zbadaj **dokumenty skryptów** węzła dla działającej aplikacji.</span><span class="sxs-lookup"><span data-stu-id="bdf3a-175">In **Solution Explorer**, inspect the **Script Documents** node for the running application.</span></span> <span data-ttu-id="bdf3a-176">Brak pliku skryptu o nazwie **koncentratory** generujący biblioteki SignalR dynamicznie w czasie wykonywania.</span><span class="sxs-lookup"><span data-stu-id="bdf3a-176">There is a script file named **hubs** that the SignalR library dynamically generates at runtime.</span></span> <span data-ttu-id="bdf3a-177">Ten plik zarządza komunikacji między jQuery skryptu i kod po stronie serwera.</span><span class="sxs-lookup"><span data-stu-id="bdf3a-177">This file manages the communication between jQuery script and server-side code.</span></span>

    ![Wygenerowany Centrum skryptów](tutorial-getting-started-with-signalr/_static/image7.png)

<a id="code"></a>

## <a name="examine-the-code"></a><span data-ttu-id="bdf3a-179">Poszukaj w kodzie</span><span class="sxs-lookup"><span data-stu-id="bdf3a-179">Examine the Code</span></span>

<span data-ttu-id="bdf3a-180">Aplikacji rozmów SignalR pokazuje dwa podstawowe zadania rozwoju SignalR: tworzenie koncentratora jako obiekt główny koordynacji na serwerze i za pomocą biblioteki jQuery SignalR do wysyłania i odbierania komunikatów.</span><span class="sxs-lookup"><span data-stu-id="bdf3a-180">The SignalR chat application demonstrates two basic SignalR development tasks: creating a hub as the main coordination object on the server, and using the SignalR jQuery library to send and receive messages.</span></span>

### <a name="signalr-hubs"></a><span data-ttu-id="bdf3a-181">Koncentratory SignalR</span><span class="sxs-lookup"><span data-stu-id="bdf3a-181">SignalR Hubs</span></span>

<span data-ttu-id="bdf3a-182">W przykładowym kodzie **ChatHub** klasa pochodzi od **Microsoft.AspNet.SignalR.Hub** klasy.</span><span class="sxs-lookup"><span data-stu-id="bdf3a-182">In the code sample the **ChatHub** class derives from the **Microsoft.AspNet.SignalR.Hub** class.</span></span> <span data-ttu-id="bdf3a-183">Wyprowadzanie z **Centrum** klasy jest to wygodny sposób, aby skompilować aplikację SignalR.</span><span class="sxs-lookup"><span data-stu-id="bdf3a-183">Deriving from the **Hub** class is a useful way to build a SignalR application.</span></span> <span data-ttu-id="bdf3a-184">Można utworzyć metody publiczne na klasy koncentratora, a następnie uzyskać dostęp do tych metod, wywołując je z jQuery skrypty na stronie sieci web.</span><span class="sxs-lookup"><span data-stu-id="bdf3a-184">You can create public methods on your hub class and then access those methods by calling them from jQuery scripts in a web page.</span></span>

<span data-ttu-id="bdf3a-185">W kodzie Rozmowa klienci wywołują **ChatHub.Send** metodę, aby wysyłać nowy komunikat.</span><span class="sxs-lookup"><span data-stu-id="bdf3a-185">In the chat code, clients call the **ChatHub.Send** method to send a new message.</span></span> <span data-ttu-id="bdf3a-186">Centrum z kolei wysyła wiadomość do wszystkich klientów, wywołując **Clients.All.broadcastMessage**.</span><span class="sxs-lookup"><span data-stu-id="bdf3a-186">The hub in turn sends the message to all clients by calling **Clients.All.broadcastMessage**.</span></span>

<span data-ttu-id="bdf3a-187">**Wysyłania** metoda pokazuje kilka koncepcji w Centrum:</span><span class="sxs-lookup"><span data-stu-id="bdf3a-187">The **Send** method demonstrates several hub concepts :</span></span>

- <span data-ttu-id="bdf3a-188">Tak, aby je wywoływać klientów do deklarowania metod publicznych w koncentratorze.</span><span class="sxs-lookup"><span data-stu-id="bdf3a-188">Declare public methods on a hub so that clients can call them.</span></span>
- <span data-ttu-id="bdf3a-189">Użyj **Microsoft.AspNet.SignalR.Hub.Clients** właściwość dynamiczna ma dostęp do wszystkich klientów podłączone do tego koncentratora.</span><span class="sxs-lookup"><span data-stu-id="bdf3a-189">Use the **Microsoft.AspNet.SignalR.Hub.Clients** dynamic property to access all clients connected to this hub.</span></span>
- <span data-ttu-id="bdf3a-190">Wywołaj funkcję jQuery na kliencie (takich jak `broadcastMessage` funkcji) aby zaktualizować klientów.</span><span class="sxs-lookup"><span data-stu-id="bdf3a-190">Call a jQuery function on the client (such as the `broadcastMessage` function) to update clients.</span></span>

    [!code-csharp[Main](tutorial-getting-started-with-signalr/samples/sample5.cs)]

### <a name="signalr-and-jquery"></a><span data-ttu-id="bdf3a-191">SignalR i jQuery</span><span class="sxs-lookup"><span data-stu-id="bdf3a-191">SignalR and jQuery</span></span>

<span data-ttu-id="bdf3a-192">Strony HTML w przykładzie kodu pokazano, jak użyć biblioteki jQuery SignalR do komunikowania się z Centrum SignalR.</span><span class="sxs-lookup"><span data-stu-id="bdf3a-192">The HTML page in the code sample shows how to use the SignalR jQuery library to communicate with a SignalR hub.</span></span> <span data-ttu-id="bdf3a-193">Serwer proxy, aby odwoływać się do Centrum deklarowania funkcji, która serwera można wywołać w celu wypychania zawartości do klientów i od połączenia, aby wysyłać komunikaty do Centrum deklaruje podstawowych zadań w kodzie.</span><span class="sxs-lookup"><span data-stu-id="bdf3a-193">The essential tasks in the code are declaring a proxy to reference the hub, declaring a function that the server can call to push content to clients, and starting a connection to send messages to the hub.</span></span>

<span data-ttu-id="bdf3a-194">Poniższy kod deklaruje serwera proxy koncentratora.</span><span class="sxs-lookup"><span data-stu-id="bdf3a-194">The following code declares a proxy for a hub.</span></span>

[!code-javascript[Main](tutorial-getting-started-with-signalr/samples/sample6.js)]

> [!NOTE]
> <span data-ttu-id="bdf3a-195">Odwołanie do klasy serwera i jej elementów członkowskich w jQuery znajduje się w camelcase.</span><span class="sxs-lookup"><span data-stu-id="bdf3a-195">In jQuery the reference to the server class and its members is in camel case.</span></span> <span data-ttu-id="bdf3a-196">Przykładowy kod odwołuje się do języka C# **ChatHub** klasy w jQuery jako **chatHub**.</span><span class="sxs-lookup"><span data-stu-id="bdf3a-196">The code sample references the C# **ChatHub** class in jQuery as **chatHub**.</span></span>


<span data-ttu-id="bdf3a-197">Poniższy kod jest na tym, jak utworzyć funkcję wywołania zwrotnego w skrypcie.</span><span class="sxs-lookup"><span data-stu-id="bdf3a-197">The following code is how you create a callback function in the script.</span></span> <span data-ttu-id="bdf3a-198">Klasy koncentratora na serwerze wywołuje tę funkcję, aby wypchnąć aktualizacji zawartości dla każdego klienta.</span><span class="sxs-lookup"><span data-stu-id="bdf3a-198">The hub class on the server calls this function to push content updates to each client.</span></span> <span data-ttu-id="bdf3a-199">Dwa wiersze, czy kodowanie HTML zawartości przed wyświetleniem go są opcjonalne i Pokaż prostego sposobu zapobiegania uruchomienie skryptu.</span><span class="sxs-lookup"><span data-stu-id="bdf3a-199">The two lines that HTML encode the content before displaying it are optional and show a simple way to prevent script injection.</span></span>

[!code-html[Main](tutorial-getting-started-with-signalr/samples/sample7.html)]

<span data-ttu-id="bdf3a-200">Poniższy kod przedstawia sposób nawiązać połączenie z koncentratorem.</span><span class="sxs-lookup"><span data-stu-id="bdf3a-200">The following code shows how to open a connection with the hub.</span></span> <span data-ttu-id="bdf3a-201">Kod uruchamia połączenie i przekazuje go po funkcji do obsługi zdarzenia click na **wysyłania** przycisk na stronie HTML.</span><span class="sxs-lookup"><span data-stu-id="bdf3a-201">The code starts the connection and then passes it a function to handle the click event on the **Send** button in the HTML page.</span></span>

> [!NOTE]
> <span data-ttu-id="bdf3a-202">To podejście ubezpieczycielom, że połączenie zostało nawiązane przed wykonaniem programu obsługi zdarzeń.</span><span class="sxs-lookup"><span data-stu-id="bdf3a-202">This approach insures that the connection is established before the event handler executes.</span></span>


[!code-javascript[Main](tutorial-getting-started-with-signalr/samples/sample8.js)]

<a id="next"></a>

## <a name="next-steps"></a><span data-ttu-id="bdf3a-203">Następne kroki</span><span class="sxs-lookup"><span data-stu-id="bdf3a-203">Next Steps</span></span>

<span data-ttu-id="bdf3a-204">Wiesz, że SignalR to architektura służąca do tworzenia aplikacji sieci web w czasie rzeczywistym.</span><span class="sxs-lookup"><span data-stu-id="bdf3a-204">You learned that SignalR is a framework for building real-time web applications.</span></span> <span data-ttu-id="bdf3a-205">Pokazaliśmy również kilka zadań programistycznych SignalR: jak dodać SignalR do aplikacji ASP.NET, sposób tworzenia klasy koncentratora oraz jak wysyłać i odbierać komunikaty z Centrum.</span><span class="sxs-lookup"><span data-stu-id="bdf3a-205">You also learned several SignalR development tasks: how to add SignalR to an ASP.NET application, how to create a hub class, and how to send and receive messages from the hub.</span></span>

<span data-ttu-id="bdf3a-206">Możesz udostępnić przykładowej aplikacji w tym samouczku lub innych aplikacji SignalR w Internecie, wdrażając je do dostawcy usług hostingowych.</span><span class="sxs-lookup"><span data-stu-id="bdf3a-206">You can make the sample application in this tutorial or other SignalR applications available over the Internet by deploying them to a hosting provider.</span></span> <span data-ttu-id="bdf3a-207">Firma Microsoft oferuje bezpłatny internetowy hostowanie do 10 witryn sieci web w ramach bezpłatnego [konta wersji próbnej usługi Windows Azure](https://www.windowsazure.com/pricing/free-trial/?WT.mc_id=A443DD604).</span><span class="sxs-lookup"><span data-stu-id="bdf3a-207">Microsoft offers free web hosting for up to 10 web sites in a free [Windows Azure trial account](https://www.windowsazure.com/pricing/free-trial/?WT.mc_id=A443DD604).</span></span> <span data-ttu-id="bdf3a-208">Aby uzyskać wskazówki dotyczące sposobu wdrażania przykładowej aplikacji SignalR, zobacz [publikowania SignalR wprowadzenie do przykładu jako witryny sieci Web Azure Windows](https://blogs.msdn.com/b/timlee/archive/2013/02/27/deploy-the-signalr-getting-started-sample-as-a-windows-azure-web-site.aspx).</span><span class="sxs-lookup"><span data-stu-id="bdf3a-208">For a walkthrough on how to deploy the sample SignalR application, see [Publish the SignalR Getting Started Sample as a Windows Azure Web Site](https://blogs.msdn.com/b/timlee/archive/2013/02/27/deploy-the-signalr-getting-started-sample-as-a-windows-azure-web-site.aspx).</span></span> <span data-ttu-id="bdf3a-209">Aby uzyskać szczegółowe informacje o sposobie wdrażania projektu sieci web programu Visual Studio z witryny sieci Web do Windows Azure, zobacz [wdrażanie aplikacji ASP.NET do witryny sieci Web Windows Azure](https://docs.microsoft.com/azure/app-service-web/app-service-web-get-started-dotnet).</span><span class="sxs-lookup"><span data-stu-id="bdf3a-209">For detailed information about how to deploy a Visual Studio web project to a Windows Azure Web Site, see [Deploying an ASP.NET Application to a Windows Azure Web Site](https://docs.microsoft.com/azure/app-service-web/app-service-web-get-started-dotnet).</span></span> <span data-ttu-id="bdf3a-210">(Uwaga: transportu WebSocket nie jest obecnie obsługiwane dla witryn sieci Web Windows Azure.</span><span class="sxs-lookup"><span data-stu-id="bdf3a-210">(Note: The WebSocket transport is not currently supported for Windows Azure Web Sites.</span></span> <span data-ttu-id="bdf3a-211">WebSocket podczas transportu nie jest dostępna, SignalR używa innych dostępnych transportu, zgodnie z opisem w sekcji transportów [wprowadzenie do SignalR tematu](index.md).)</span><span class="sxs-lookup"><span data-stu-id="bdf3a-211">When WebSocket transport is not available, SignalR uses the other available transports as described in the Transports section of the [Introduction to SignalR topic](index.md).)</span></span>

<span data-ttu-id="bdf3a-212">Aby dowiedzieć się bardziej zaawansowanych pojęciach zmiany SignalR, odwiedź następującą witrynę dla SignalR kod źródłowy i zasobów:</span><span class="sxs-lookup"><span data-stu-id="bdf3a-212">To learn more advanced SignalR developments concepts, visit the following sites for SignalR source code and resources:</span></span>

- [<span data-ttu-id="bdf3a-213">Projekt SignalR</span><span class="sxs-lookup"><span data-stu-id="bdf3a-213">SignalR Project</span></span>](http://signalr.net)
- [<span data-ttu-id="bdf3a-214">SignalR Github i przykłady</span><span class="sxs-lookup"><span data-stu-id="bdf3a-214">SignalR Github and Samples</span></span>](https://github.com/SignalR/SignalR)
- [<span data-ttu-id="bdf3a-215">Witryny typu Wiki biblioteki SignalR</span><span class="sxs-lookup"><span data-stu-id="bdf3a-215">SignalR Wiki</span></span>](https://github.com/SignalR/SignalR/wiki)
