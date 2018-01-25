---
uid: mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-fundamentals
title: Podstawowe informacje na temat platformy ASP.NET MVC 4 | Dokumentacja firmy Microsoft
author: rick-anderson
description: "W tym laboratorium Hands-On opiera się na magazynie utworów muzycznych MVC (Model View Controller), samouczek aplikacji, które wprowadzono i opisano krok po kroku, jak używać ASP.NET MV..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/18/2013
ms.topic: article
ms.assetid: b7dba543-73c3-4534-a9a0-ba70fa2c6a8a
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-fundamentals
msc.type: authoredcontent
ms.openlocfilehash: 468f6d5dabb645b1c005680dc5a1ffc4debd63b6
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/24/2018
---
<a name="aspnet-mvc-4-fundamentals"></a><span data-ttu-id="3507f-103">Podstawowe informacje na temat platformy ASP.NET MVC 4</span><span class="sxs-lookup"><span data-stu-id="3507f-103">ASP.NET MVC 4 Fundamentals</span></span>
====================
<span data-ttu-id="3507f-104">przez [obozów sieci Web Team](https://twitter.com/webcamps)</span><span class="sxs-lookup"><span data-stu-id="3507f-104">by [Web Camps Team](https://twitter.com/webcamps)</span></span>

> <span data-ttu-id="3507f-105">W tym laboratorium Hands-On jest oparta na sklep muzyczny MVC (Model View Controller), samouczek aplikacji, które wprowadzono i opisano krok po kroku, jak używać programu ASP.NET MVC i Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="3507f-105">This Hands-On Lab is based on MVC (Model View Controller) Music Store, a tutorial application that introduces and explains step-by-step how to use ASP.NET MVC and Visual Studio.</span></span> <span data-ttu-id="3507f-106">W laboratorium dowiesz się prostotę, jeszcze mocy przy użyciu tych technologii jednocześnie.</span><span class="sxs-lookup"><span data-stu-id="3507f-106">Throughout the lab you will learn the simplicity, yet power of using these technologies together.</span></span> <span data-ttu-id="3507f-107">Rozpoczyna się od prostej aplikacji i zostanie on skompilowany, aż do uzyskania funkcjonalnej ASP.NET MVC 4 aplikacji sieci Web.</span><span class="sxs-lookup"><span data-stu-id="3507f-107">You will start with a simple application and will build it until you have a fully functional ASP.NET MVC 4 Web Application.</span></span>
> 
> <span data-ttu-id="3507f-108">W tym laboratorium współpracuje z platformy ASP.NET MVC 4.</span><span class="sxs-lookup"><span data-stu-id="3507f-108">This Lab works with ASP.NET MVC 4.</span></span>
> 
> <span data-ttu-id="3507f-109">Jeśli chcesz zapoznać się z wersji platformy ASP.NET MVC 3 samouczka aplikacji, można znaleźć w [MVC utworów muzycznych magazynu](https://github.com/evilDave/MVC-Music-Store).</span><span class="sxs-lookup"><span data-stu-id="3507f-109">If you wish to explore the ASP.NET MVC 3 version of the tutorial application, you can find it in [MVC-Music-Store](https://github.com/evilDave/MVC-Music-Store).</span></span>
> 
> > [!NOTE]
> > <span data-ttu-id="3507f-110">W tym laboratorium Hands-On przyjęto założenie, że deweloper ma doświadczenie w sieci Web development technologii, takich jak HTML i JavaScript.</span><span class="sxs-lookup"><span data-stu-id="3507f-110">This Hands-On Lab assumes that the developer has experience in Web development technologies, such as HTML and JavaScript.</span></span>
> 
> 
> <span data-ttu-id="3507f-111">Wszystkie przykładowy kod i fragmenty kodu są uwzględnione w sieci Web obozów zestaw szkoleniowy, dostępne pod adresem [https://www.microsoft.com/download/29843](https://www.microsoft.com/download/29843).</span><span class="sxs-lookup"><span data-stu-id="3507f-111">All sample code and snippets are included in the Web Camps Training Kit, available at [https://www.microsoft.com/download/29843](https://www.microsoft.com/download/29843).</span></span>


<a id="The_Music_Store_application"></a>
### <a name="the-music-store-application"></a><span data-ttu-id="3507f-112">Aplikacji magazynu utworów muzycznych</span><span class="sxs-lookup"><span data-stu-id="3507f-112">The Music Store application</span></span>

<span data-ttu-id="3507f-113">Muzyka Magazyn aplikacji sieci web, który zostanie skompilowany w tym laboratorium obejmuje trzy główne części: zakupów, wyewidencjonowania i administracji.</span><span class="sxs-lookup"><span data-stu-id="3507f-113">The Music Store web application that will be built throughout this Lab comprises three main parts: shopping, checkout, and administration.</span></span> <span data-ttu-id="3507f-114">Osoby odwiedzające będzie Przeglądaj albumów według rodzaju, Dodaj do koszyka ich albumy, przejrzyj ich zaznaczenie i koniec przejść do wyewidencjonowania, aby się zalogować i ukończyć kolejności.</span><span class="sxs-lookup"><span data-stu-id="3507f-114">Visitors will be able to browse albums by genre, add albums to their cart, review their selection and finally proceed to checkout to login and complete the order.</span></span> <span data-ttu-id="3507f-115">Ponadto magazynu administratorzy będą mogli zarządzać albumów dostępne, a także ich głównych właściwości.</span><span class="sxs-lookup"><span data-stu-id="3507f-115">Additionally, store administrators will be able to manage the available albums as well as their main properties.</span></span>

<span data-ttu-id="3507f-116">![Ekrany magazynu utworów muzycznych](aspnet-mvc-4-fundamentals/_static/image1.png "ekrany magazynu utworów muzycznych")</span><span class="sxs-lookup"><span data-stu-id="3507f-116">![Music Store screens](aspnet-mvc-4-fundamentals/_static/image1.png "Music Store screens")</span></span>

<span data-ttu-id="3507f-117">*Ekrany magazynu utworów muzycznych*</span><span class="sxs-lookup"><span data-stu-id="3507f-117">*Music Store screens*</span></span>

<a id="ASPNET_MVC_4_Essentials"></a>
### <a name="aspnet-mvc-4-essentials"></a><span data-ttu-id="3507f-118">Podstawowe informacje dotyczące platformy ASP.NET MVC 4</span><span class="sxs-lookup"><span data-stu-id="3507f-118">ASP.NET MVC 4 Essentials</span></span>

<span data-ttu-id="3507f-119">Aplikację ze sklepu utworów muzycznych zostaną skompilowane z użyciem **kontrolera MVC (Model View)**, wzorzec architektury, która dzieli aplikację na trzy główne składniki:</span><span class="sxs-lookup"><span data-stu-id="3507f-119">Music Store application will be built using **Model View Controller (MVC)**, an architectural pattern that separates an application into three main components:</span></span>

- <span data-ttu-id="3507f-120">**Modele**: obiekty modelu są częściami aplikacji, które implementują logikę domeny.</span><span class="sxs-lookup"><span data-stu-id="3507f-120">**Models**: Model objects are the parts of the application that implement the domain logic.</span></span> <span data-ttu-id="3507f-121">Często obiekty modelu również pobrać i przechowywanie stanu modelu w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="3507f-121">Often, model objects also retrieve and store model state in a database.</span></span>
- <span data-ttu-id="3507f-122">**Widoki:** widoki są składnikami wyświetlającymi interfejs użytkownika aplikacji (UI).</span><span class="sxs-lookup"><span data-stu-id="3507f-122">**Views:** Views are the components that display the application's user interface (UI).</span></span> <span data-ttu-id="3507f-123">Zazwyczaj interfejs ten jest utworzone na podstawie danych modelu.</span><span class="sxs-lookup"><span data-stu-id="3507f-123">Typically, this UI is created from the model data.</span></span> <span data-ttu-id="3507f-124">Przykładem może być widok edycji albumów zawierający pola tekstowe i listy rozwijanej, w oparciu o bieżący stan obiektu albumu.</span><span class="sxs-lookup"><span data-stu-id="3507f-124">An example would be the edit view of Albums that displays text boxes and a drop-down list based on the current state of an Album object.</span></span>
- <span data-ttu-id="3507f-125">**Kontrolery:** kontrolery są składnikami, które obsługują interakcję z użytkownikiem, manipulować modelem i ostatecznie wybierają widok do renderowania interfejsu użytkownika.</span><span class="sxs-lookup"><span data-stu-id="3507f-125">**Controllers:** Controllers are the components that handle user interaction, manipulate the model, and ultimately select a view to render the UI.</span></span> <span data-ttu-id="3507f-126">W aplikacji MVC widoku wyświetlane są tylko informacje; Kontroler obsługuje i ma odpowiadać na dane wejściowe użytkownika i interakcja.</span><span class="sxs-lookup"><span data-stu-id="3507f-126">In an MVC application, the view only displays information; the controller handles and responds to user input and interaction.</span></span>

<span data-ttu-id="3507f-127">Wzorzec MVC pomaga tworzyć aplikacji połączonych różne aspekty aplikacji (logika danych wejściowych, logika biznesowa i logika interfejsu użytkownika), zapewniając luźne powiązanie między tymi elementami.</span><span class="sxs-lookup"><span data-stu-id="3507f-127">The MVC pattern helps you to create applications that separate the different aspects of the application (input logic, business logic, and UI logic), while providing a loose coupling between these elements.</span></span> <span data-ttu-id="3507f-128">Taki rozdział pomaga w zarządzaniu złożonością podczas tworzenia aplikacji, ponieważ pozwala skupić się na jednym aspekcie implementacji naraz.</span><span class="sxs-lookup"><span data-stu-id="3507f-128">This separation helps you manage complexity when you build an application, as it allows you to focus on one aspect of the implementation at a time.</span></span> <span data-ttu-id="3507f-129">Ponadto wzorzec MVC ułatwia testowanie aplikacji również wspieranie stosowania projektowanie oparte na (testach TDD) do tworzenia aplikacji.</span><span class="sxs-lookup"><span data-stu-id="3507f-129">In addition, the MVC pattern makes it easy to test applications, also encouraging the use of test-driven development (TDD) for creating applications.</span></span>

<span data-ttu-id="3507f-130">**ASP.NET MVC** framework stanowi alternatywę dla składnika ASP.NET Web Forms wzorzec służący do tworzenia aplikacji sieci Web opartych na platformie ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="3507f-130">The **ASP.NET MVC** framework provides an alternative to the ASP.NET Web Forms pattern for creating ASP.NET MVC-based Web applications.</span></span> <span data-ttu-id="3507f-131">**ASP.NET MVC** framework to struktura prezentacji niewielka, wysoce testowalna która (tak jak aplikacje oparte na formularzach sieci Web) jest zintegrowana z istniejącymi funkcjami platformy ASP.NET, takich jak strony wzorcowe i oparte na członkostwie uwierzytelnianie, aby pobrać wszystkie możliwości platformy .NET core.</span><span class="sxs-lookup"><span data-stu-id="3507f-131">The **ASP.NET MVC** framework is a lightweight, highly testable presentation framework that (as with Web-forms-based applications) is integrated with existing ASP.NET features, such as master pages and membership-based authentication so you get all the power of the core .NET framework.</span></span> <span data-ttu-id="3507f-132">Jest to przydatne, jeśli już znasz z formularzy sieci Web ASP.NET wszystkie biblioteki, które już używają nie są dostępne w programie ASP.NET MVC 4 oraz.</span><span class="sxs-lookup"><span data-stu-id="3507f-132">This is useful if you are already familiar with ASP.NET Web Forms because all the libraries that you already use are available in ASP.NET MVC 4 as well.</span></span>

<span data-ttu-id="3507f-133">Ponadto luźne powiązanie między trzema głównymi składnikami aplikacji MVC wspiera także Programowanie równoległe.</span><span class="sxs-lookup"><span data-stu-id="3507f-133">In addition, the loose coupling between the three main components of an MVC application also promotes parallel development.</span></span> <span data-ttu-id="3507f-134">Na przykład co może pracować w widoku, drugi może pracować nad logiką kontrolera i trzeci deweloper może skupić się na logiki biznesowej w modelu.</span><span class="sxs-lookup"><span data-stu-id="3507f-134">For instance, one developer can work on the view, a second developer can work on the controller logic, and a third developer can focus on the business logic in the model.</span></span>

<a id="Objectives"></a>

<a id="Objectives"></a>
### <a name="objectives"></a><span data-ttu-id="3507f-135">Cele</span><span class="sxs-lookup"><span data-stu-id="3507f-135">Objectives</span></span>

<span data-ttu-id="3507f-136">W tym laboratorium Hands-On przedstawiono sposób:</span><span class="sxs-lookup"><span data-stu-id="3507f-136">In this Hands-On Lab, you will learn how to:</span></span>

- <span data-ttu-id="3507f-137">Tworzenie aplikacji platformy ASP.NET MVC od podstaw oparte na samouczek aplikacji magazynu utworów muzycznych</span><span class="sxs-lookup"><span data-stu-id="3507f-137">Create an ASP.NET MVC application from scratch based on the Music Store Application tutorial</span></span>
- <span data-ttu-id="3507f-138">Dodaj kontrolery do obsługi adresów URL do strony głównej witryny oraz przeglądanie jej funkcji main</span><span class="sxs-lookup"><span data-stu-id="3507f-138">Add Controllers to handle URLs to the Home page of the site and for browsing its main functionality</span></span>
- <span data-ttu-id="3507f-139">Dodaj widok, aby dostosować zawartość wyświetlana wraz z jego styl</span><span class="sxs-lookup"><span data-stu-id="3507f-139">Add a View to customize the content displayed along with its style</span></span>
- <span data-ttu-id="3507f-140">Dodawanie klasy modelu zawierają i zarządzanie nimi danych i domeny logiki</span><span class="sxs-lookup"><span data-stu-id="3507f-140">Add Model classes to contain and manage data and domain logic</span></span>
- <span data-ttu-id="3507f-141">Użyj wzorca Model widoku do przekazywania informacji z akcji kontrolera do widoku szablonów</span><span class="sxs-lookup"><span data-stu-id="3507f-141">Use View Model pattern to pass information from controller actions to the view templates</span></span>
- <span data-ttu-id="3507f-142">Eksploruj nowego szablonu platformy ASP.NET MVC 4 dla aplikacji internetowych</span><span class="sxs-lookup"><span data-stu-id="3507f-142">Explore the ASP.NET MVC 4 new template for internet applications</span></span>

<a id="Prerequisites"></a>

<a id="Prerequisites"></a>
### <a name="prerequisites"></a><span data-ttu-id="3507f-143">Wymagania wstępne</span><span class="sxs-lookup"><span data-stu-id="3507f-143">Prerequisites</span></span>

<span data-ttu-id="3507f-144">Musi mieć następujące elementy do przygotowania tego laboratorium:</span><span class="sxs-lookup"><span data-stu-id="3507f-144">You must have the following items to complete this lab:</span></span>

- <span data-ttu-id="3507f-145">[Visual Studio Express 2012 for Web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web) (odczytu [dodatek a.](#AppendixA) instrukcje dotyczące sposobu jego instalacji)</span><span class="sxs-lookup"><span data-stu-id="3507f-145">[Visual Studio 2012 Express for Web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web) (read [Appendix A](#AppendixA) for instructions on how to install it)</span></span>

<a id="Setup"></a>

<a id="Setup"></a>
### <a name="setup"></a><span data-ttu-id="3507f-146">Konfiguracja</span><span class="sxs-lookup"><span data-stu-id="3507f-146">Setup</span></span>

<span data-ttu-id="3507f-147">**Instalowanie wstawki kodu**</span><span class="sxs-lookup"><span data-stu-id="3507f-147">**Installing Code Snippets**</span></span>

<span data-ttu-id="3507f-148">Dla wygody taki kod, który będzie zarządzany wzdłuż tego laboratorium jest dostępna jako wstawki kodu programu Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="3507f-148">For convenience, much of the code you will be managing along this lab is available as Visual Studio code snippets.</span></span> <span data-ttu-id="3507f-149">Aby zainstalować fragmenty kodu, uruchom **.\Source\Setup\CodeSnippets.vsi** pliku.</span><span class="sxs-lookup"><span data-stu-id="3507f-149">To install the code snippets run **.\Source\Setup\CodeSnippets.vsi** file.</span></span>

<span data-ttu-id="3507f-150">Jeśli nie masz doświadczenia z wstawki programu Visual Studio i chcesz dowiedzieć się, jak ich używać, można odwołać się do dodatku z tego dokumentu &quot; [wstawki kodu za pomocą programu dodatek C:](#AppendixC)&quot;.</span><span class="sxs-lookup"><span data-stu-id="3507f-150">If you are not familiar with the Visual Studio Code Snippets, and want to learn how to use them, you can refer to the appendix from this document &quot;[Appendix C: Using Code Snippets](#AppendixC)&quot;.</span></span>

<a id="Exercises"></a>

<a id="Exercises"></a>
## <a name="exercises"></a><span data-ttu-id="3507f-151">Ćwiczenia</span><span class="sxs-lookup"><span data-stu-id="3507f-151">Exercises</span></span>

<span data-ttu-id="3507f-152">W tym laboratorium Hands-On składa się przez następujących czynnościach:</span><span class="sxs-lookup"><span data-stu-id="3507f-152">This Hands-On Lab is comprised by the following exercises:</span></span>

1. [<span data-ttu-id="3507f-153">Ćwiczenie 1: Tworzenie projektu aplikacji sieci Web platformy ASP.NET MVC MusicStore</span><span class="sxs-lookup"><span data-stu-id="3507f-153">Exercise 1: Creating MusicStore ASP.NET MVC Web Application Project</span></span>](#Exercise1)
2. [<span data-ttu-id="3507f-154">Ćwiczenie 2: Tworzenie kontrolera</span><span class="sxs-lookup"><span data-stu-id="3507f-154">Exercise 2: Creating a Controller</span></span>](#Exercise2)
3. [<span data-ttu-id="3507f-155">Ćwiczenie 3: Przekazywanie parametrów do kontrolera</span><span class="sxs-lookup"><span data-stu-id="3507f-155">Exercise 3: Passing parameters to a Controller</span></span>](#Exercise3)
4. [<span data-ttu-id="3507f-156">Ćwiczenie 4: Tworzenie widoku</span><span class="sxs-lookup"><span data-stu-id="3507f-156">Exercise 4: Creating a View</span></span>](#Exercise4)
5. [<span data-ttu-id="3507f-157">Ćwiczenie 5: Tworzenie modelu widoku</span><span class="sxs-lookup"><span data-stu-id="3507f-157">Exercise 5: Creating a View Model</span></span>](#Exercise5)
6. [<span data-ttu-id="3507f-158">Ćwiczenie 6: W widoku przy użyciu parametrów</span><span class="sxs-lookup"><span data-stu-id="3507f-158">Exercise 6: Using parameters in View</span></span>](#Exercise6)
7. [<span data-ttu-id="3507f-159">Ćwiczenie 7: Laptop wokół nowego szablonu platformy ASP.NET MVC 4</span><span class="sxs-lookup"><span data-stu-id="3507f-159">Exercise 7: A lap around ASP.NET MVC 4 New Template</span></span>](#Exercise7)

> [!NOTE]
> <span data-ttu-id="3507f-160">Towarzyszy każdego wykonywania **zakończenia** folderu zawierającego wynikowy rozwiązanie, należy uzyskać po wykonaniu ćwiczeniach.</span><span class="sxs-lookup"><span data-stu-id="3507f-160">Each exercise is accompanied by an **End** folder containing the resulting solution you should obtain after completing the exercises.</span></span> <span data-ttu-id="3507f-161">Jeśli potrzebujesz dodatkowej pomocy pracuje nad ćwiczeniami, można użyć tego rozwiązania jako przewodnika.</span><span class="sxs-lookup"><span data-stu-id="3507f-161">You can use this solution as a guide if you need additional help working through the exercises.</span></span>


<span data-ttu-id="3507f-162">Szacowany czas trwania tego laboratorium: **60 minut**.</span><span class="sxs-lookup"><span data-stu-id="3507f-162">Estimated time to complete this lab: **60 minutes**.</span></span>

<a id="Exercise1"></a>

<a id="Exercise_1_Creating_MusicStore_ASPNET_MVC_Web_Application_Project"></a>
### <a name="exercise-1-creating-musicstore-aspnet-mvc-web-application-project"></a><span data-ttu-id="3507f-163">Ćwiczenie 1: Tworzenie projektu aplikacji sieci Web platformy ASP.NET MVC MusicStore</span><span class="sxs-lookup"><span data-stu-id="3507f-163">Exercise 1: Creating MusicStore ASP.NET MVC Web Application Project</span></span>

<span data-ttu-id="3507f-164">W tym ćwiczeniu dowiesz się, tworzenie aplikacji platformy ASP.NET MVC w programie Visual Studio 2012 Express dla sieci Web, a także jego organizacji folderu głównego.</span><span class="sxs-lookup"><span data-stu-id="3507f-164">In this exercise, you will learn how to create an ASP.NET MVC application in Visual Studio 2012 Express for Web as well as its main folder organization.</span></span> <span data-ttu-id="3507f-165">Ponadto dowiesz sposobu dodawania nowego kontrolera i zapewnić ich prostego ciągu wyświetlanego w strony głównej aplikacji.</span><span class="sxs-lookup"><span data-stu-id="3507f-165">Additionally, you will learn how to add a new Controller and make it display a simple string in the application's home page.</span></span>

<a id="Ex1Task1"></a>

<a id="Task_1_-_Creating_the_ASPNET_MVC_Web_Application_Project"></a>
#### <a name="task-1---creating-the-aspnet-mvc-web-application-project"></a><span data-ttu-id="3507f-166">Zadanie 1 — Tworzenie projektu aplikacji sieci Web platformy ASP.NET MVC</span><span class="sxs-lookup"><span data-stu-id="3507f-166">Task 1 - Creating the ASP.NET MVC Web Application Project</span></span>

1. <span data-ttu-id="3507f-167">W ramach tego zadania spowoduje utworzenie pusty projekt aplikacji platformy ASP.NET MVC przy użyciu szablonu MVC programu Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="3507f-167">In this task, you will create an empty ASP.NET MVC application project using the MVC Visual Studio template.</span></span> <span data-ttu-id="3507f-168">Uruchom **VS Express for Web**.</span><span class="sxs-lookup"><span data-stu-id="3507f-168">Start **VS Express for Web**.</span></span>
2. <span data-ttu-id="3507f-169">Na **pliku** menu, kliknij przycisk **nowy projekt**.</span><span class="sxs-lookup"><span data-stu-id="3507f-169">On the **File** menu, click **New Project**.</span></span>
3. <span data-ttu-id="3507f-170">W **nowy projekt** wybierz okno dialogowe **aplikacji sieci Web programu ASP.NET MVC 4** typu znajduje się w obszarze projektu **Visual C#,** **Web** szablonu Lista.</span><span class="sxs-lookup"><span data-stu-id="3507f-170">In the **New Project** dialog box select the **ASP.NET MVC 4 Web Application** project type, located under **Visual C#,** **Web** template list.</span></span>
4. <span data-ttu-id="3507f-171">Zmień **nazwa** do *MvcMusicStore*.</span><span class="sxs-lookup"><span data-stu-id="3507f-171">Change the **Name** to *MvcMusicStore*.</span></span>
5. <span data-ttu-id="3507f-172">Ustaw lokalizację rozwiązania wewnątrz nowy **rozpocząć** folder, w tym ćwiczeniu folder źródłowy, na przykład **\Source\Ex01-CreatingMusicStoreProject\Begin [YOUR-dzień HOL-PATH]**.</span><span class="sxs-lookup"><span data-stu-id="3507f-172">Set the location of the solution inside a new **Begin** folder in this Exercise's Source folder, for example **[YOUR-HOL-PATH]\Source\Ex01-CreatingMusicStoreProject\Begin**.</span></span> <span data-ttu-id="3507f-173">Kliknij przycisk **OK**.</span><span class="sxs-lookup"><span data-stu-id="3507f-173">Click **OK**.</span></span>

    <span data-ttu-id="3507f-174">![Utwórz okno dialogowe Nowy projekt](aspnet-mvc-4-fundamentals/_static/image2.png "utworzyć okno dialogowe Nowy projekt")</span><span class="sxs-lookup"><span data-stu-id="3507f-174">![Create New Project Dialog Box](aspnet-mvc-4-fundamentals/_static/image2.png "Create New Project Dialog Box")</span></span>

    <span data-ttu-id="3507f-175">*Utwórz okno dialogowe Nowy projekt*</span><span class="sxs-lookup"><span data-stu-id="3507f-175">*Create New Project Dialog Box*</span></span>
6. <span data-ttu-id="3507f-176">W **nowy projekt programu ASP.NET MVC 4** wybierz okno dialogowe **podstawowe** szablonu i upewnij się, że **aparat widoku** jest **Razor**.</span><span class="sxs-lookup"><span data-stu-id="3507f-176">In the **New ASP.NET MVC 4 Project** dialog box select the **Basic** template and make sure that the **View engine** selected is **Razor**.</span></span> <span data-ttu-id="3507f-177">Kliknij przycisk **OK**.</span><span class="sxs-lookup"><span data-stu-id="3507f-177">Click **OK**.</span></span>

    <span data-ttu-id="3507f-178">![Okno dialogowe nowego projektu platformy ASP.NET MVC 4](aspnet-mvc-4-fundamentals/_static/image3.png "okno dialogowe Nowy projekt ASP.NET MVC 4")</span><span class="sxs-lookup"><span data-stu-id="3507f-178">![New ASP.NET MVC 4 Project Dialog Box](aspnet-mvc-4-fundamentals/_static/image3.png "New ASP.NET MVC 4 Project Dialog Box")</span></span>

    <span data-ttu-id="3507f-179">*Okno dialogowe nowego projektu platformy ASP.NET MVC 4*</span><span class="sxs-lookup"><span data-stu-id="3507f-179">*New ASP.NET MVC 4 Project Dialog Box*</span></span>

<a id="Ex1Task2"></a>

<a id="Task_2_-_Exploring_the_Solution_Structure"></a>
#### <a name="task-2---exploring-the-solution-structure"></a><span data-ttu-id="3507f-180">Zadanie 2 — poznawanie struktury rozwiązania</span><span class="sxs-lookup"><span data-stu-id="3507f-180">Task 2 - Exploring the Solution Structure</span></span>

<span data-ttu-id="3507f-181">Platforma ASP.NET MVC zawiera szablon projektu Visual Studio, która ułatwia tworzenie aplikacji sieci Web obsługujące wzorzec MVC.</span><span class="sxs-lookup"><span data-stu-id="3507f-181">The ASP.NET MVC framework includes a Visual Studio project template that helps you create Web applications supporting the MVC pattern.</span></span> <span data-ttu-id="3507f-182">Ten szablon tworzy nową aplikację sieci Web programu ASP.NET MVC z wymagane foldery, szablonów elementów i we wpisach w plikach konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="3507f-182">This template creates a new ASP.NET MVC Web application with the required folders, item templates, and configuration-file entries.</span></span>

<span data-ttu-id="3507f-183">To zadanie należy zbadać struktury rozwiązania zrozumienie elementy, które są zaangażowane oraz ich relacji.</span><span class="sxs-lookup"><span data-stu-id="3507f-183">In this task, you will examine the solution structure to understand the elements that are involved and their relationships.</span></span> <span data-ttu-id="3507f-184">Następujące foldery są zawarte w aplikacji ASP.NET MVC, ponieważ platforma ASP.NET MVC domyślnie korzysta z &quot;Konwencji za pośrednictwem konfiguracji&quot; podejście i sprawia, że niektóre założenia domyślne oparte na folder nazewnictwa konwencje.</span><span class="sxs-lookup"><span data-stu-id="3507f-184">The following folders are included in all the ASP.NET MVC application because the ASP.NET MVC framework by default uses a &quot;convention over configuration&quot; approach, and makes some default assumptions based on folder naming conventions.</span></span>

1. <span data-ttu-id="3507f-185">Po utworzeniu projektu, przejrzyj struktury folderów, który został utworzony w Eksploratorze rozwiązań po prawej stronie:</span><span class="sxs-lookup"><span data-stu-id="3507f-185">Once the project is created, review the folder structure that has been created in the Solution Explorer on the right side:</span></span>

    <span data-ttu-id="3507f-186">![Struktura ASP.NET MVC folderów w Eksploratorze rozwiązań](aspnet-mvc-4-fundamentals/_static/image4.png "struktury ASP.NET MVC folderów w Eksploratorze rozwiązań")</span><span class="sxs-lookup"><span data-stu-id="3507f-186">![ASP.NET MVC Folder structure in Solution Explorer](aspnet-mvc-4-fundamentals/_static/image4.png "ASP.NET MVC Folder structure in Solution Explorer")</span></span>

    <span data-ttu-id="3507f-187">*Struktura ASP.NET MVC folderów w Eksploratorze rozwiązań*</span><span class="sxs-lookup"><span data-stu-id="3507f-187">*ASP.NET MVC Folder structure in Solution Explorer*</span></span>

    1. <span data-ttu-id="3507f-188">**Kontrolery**.</span><span class="sxs-lookup"><span data-stu-id="3507f-188">**Controllers**.</span></span> <span data-ttu-id="3507f-189">Ten folder będzie zawierać klasy kontrolera.</span><span class="sxs-lookup"><span data-stu-id="3507f-189">This folder will contain the controller classes.</span></span> <span data-ttu-id="3507f-190">W aplikacji MVC na podstawie kontrolery są zobowiązani do obsługi interakcji użytkownika końcowego, manipulowanie modelem i ostatecznie wybierając widoku do renderowania interfejsu użytkownika.</span><span class="sxs-lookup"><span data-stu-id="3507f-190">In an MVC based application, controllers are responsible for handling end user interaction, manipulating the model, and ultimately choosing a view to render the UI.</span></span>

        > [!NOTE]
        > <span data-ttu-id="3507f-191">Platforma MVC wymaga nazwy wszystkich kontrolerów kończącej się &quot;kontrolera&quot;— na przykład HomeController, LoginController lub ProductController.</span><span class="sxs-lookup"><span data-stu-id="3507f-191">The MVC framework requires the names of all controllers to end with &quot;Controller&quot;-for example, HomeController, LoginController, or ProductController.</span></span>
    2. <span data-ttu-id="3507f-192">**Modele**.</span><span class="sxs-lookup"><span data-stu-id="3507f-192">**Models**.</span></span> <span data-ttu-id="3507f-193">Ten folder jest dostępna dla klasy reprezentujące model aplikacji dla aplikacji sieci Web MVC.</span><span class="sxs-lookup"><span data-stu-id="3507f-193">This folder is provided for classes that represent the application model for the MVC Web application.</span></span> <span data-ttu-id="3507f-194">Obejmuje to zazwyczaj kod, który definiuje obiekty i logika interakcji z magazynu danych.</span><span class="sxs-lookup"><span data-stu-id="3507f-194">This usually includes code that defines objects and the logic for interacting with the data store.</span></span> <span data-ttu-id="3507f-195">Zazwyczaj rzeczywiste modelu obiektów jest w bibliotekach osobnej klasy.</span><span class="sxs-lookup"><span data-stu-id="3507f-195">Typically, the actual model objects will be in separate class libraries.</span></span> <span data-ttu-id="3507f-196">Jednak podczas tworzenia nowej aplikacji może zawierać klasy i przenieś je do biblioteki klas oddzielne w późniejszym czasie w cyklu tworzenia.</span><span class="sxs-lookup"><span data-stu-id="3507f-196">However, when you create a new application, you might include classes and then move them into separate class libraries at a later point in the development cycle.</span></span>
    3. <span data-ttu-id="3507f-197">**Widoki**.</span><span class="sxs-lookup"><span data-stu-id="3507f-197">**Views**.</span></span> <span data-ttu-id="3507f-198">Ten folder jest zalecana lokalizacja widoków, składniki odpowiada za wyświetlanie interfejsu użytkownika aplikacji.</span><span class="sxs-lookup"><span data-stu-id="3507f-198">This folder is the recommended location for views, the components responsible for displaying the application's user interface.</span></span> <span data-ttu-id="3507f-199">Widoki używają plików aspx, ascx, cshtml i .master, oprócz innych plików, które są powiązane z widokami renderowania.</span><span class="sxs-lookup"><span data-stu-id="3507f-199">Views use .aspx, .ascx, .cshtml and .master files, in addition to any other files that are related to rendering views.</span></span> <span data-ttu-id="3507f-200">Widoki folder zawiera folder dla każdego kontrolera; folder nosi nazwę z prefiksem nazwy kontrolera.</span><span class="sxs-lookup"><span data-stu-id="3507f-200">Views folder contains a folder for each controller; the folder is named with the controller-name prefix.</span></span> <span data-ttu-id="3507f-201">Na przykład, jeśli masz kontroler o nazwie **HomeController**, widoki folder zawiera folder o nazwie Home.</span><span class="sxs-lookup"><span data-stu-id="3507f-201">For example, if you have a controller named **HomeController**, the Views folder will contain a folder named Home.</span></span> <span data-ttu-id="3507f-202">Domyślnie, gdy platforma ASP.NET MVC ładuje widoku szuka plik .aspx o nazwie żądany widok w folderze Views\controllerName (**widoków [ControllerName] [Action] .aspx**) lub (**widoków [ControllerName] [Action] .cshtml**) dla widokami Razor.</span><span class="sxs-lookup"><span data-stu-id="3507f-202">By default, when the ASP.NET MVC framework loads a view, it looks for an .aspx file with the requested view name in the Views\controllerName folder (**Views[ControllerName][Action].aspx**) or (**Views[ControllerName][Action].cshtml**) for Razor Views.</span></span>

    > [!NOTE]
    > <span data-ttu-id="3507f-203">Oprócz folderów wymienionymi wcześniej, korzysta z aplikacji sieci Web MVC **Global.asax** plik routingu adresów URL globalne ustawienia domyślne i używa **Web.config** plik do skonfigurowania aplikacji.</span><span class="sxs-lookup"><span data-stu-id="3507f-203">In addition to the folders listed previously, an MVC Web application uses the **Global.asax** file to set global URL routing defaults, and it uses the **Web.config** file to configure the application.</span></span>

<a id="Ex1Task3"></a>

<a id="Task_3_-_Adding_a_HomeController"></a>
#### <a name="task-3---adding-a-homecontroller"></a><span data-ttu-id="3507f-204">Zadanie 3. Dodawanie HomeController</span><span class="sxs-lookup"><span data-stu-id="3507f-204">Task 3 - Adding a HomeController</span></span>

<span data-ttu-id="3507f-205">W aplikacjach ASP.NET, które nie korzystają z struktura MVC interakcji z użytkownikiem są zorganizowane wokół stron i wywoływanie zdarzeń i obsługa zdarzeń z tych stron.</span><span class="sxs-lookup"><span data-stu-id="3507f-205">In ASP.NET applications that do not use the MVC framework, user interaction is organized around pages, and around raising and handling events from those pages.</span></span> <span data-ttu-id="3507f-206">Z kolei interakcji użytkowników z aplikacji ASP.NET MVC skupiono kontrolerów i metod akcji.</span><span class="sxs-lookup"><span data-stu-id="3507f-206">In contrast, user interaction with ASP.NET MVC applications is organized around controllers and their action methods.</span></span>

<span data-ttu-id="3507f-207">Z drugiej strony platforma ASP.NET MVC mapuje adresy URL do klasy, które są określane jako kontrolery.</span><span class="sxs-lookup"><span data-stu-id="3507f-207">On the other hand, ASP.NET MVC framework maps URLs to classes that are referred to as controllers.</span></span> <span data-ttu-id="3507f-208">Kontrolery przetworzyć żądań przychodzących, obsługi danych wejściowych użytkownika i interakcji, wykonywanie logiki aplikacji odpowiednie i określić odpowiedzi do odesłania do klienta (wyświetlania kodu HTML, Pobierz plik, przekierowania do innego adresu URL itp.).</span><span class="sxs-lookup"><span data-stu-id="3507f-208">Controllers process incoming requests, handle user input and interactions, execute appropriate application logic and determine the response to send back to the client (display HTML, download a file, redirect to a different URL, etc.).</span></span> <span data-ttu-id="3507f-209">W przypadku wyświetlania kodu HTML, klasę kontrolera zwykle wywołuje składnik osobny widok, aby wygenerować kod znaczników HTML dla żądania.</span><span class="sxs-lookup"><span data-stu-id="3507f-209">In the case of displaying HTML, a controller class typically calls a separate view component to generate the HTML markup for the request.</span></span> <span data-ttu-id="3507f-210">W aplikacji MVC widoku wyświetlane są tylko informacje; Kontroler obsługuje i ma odpowiadać na dane wejściowe użytkownika i interakcja.</span><span class="sxs-lookup"><span data-stu-id="3507f-210">In an MVC application, the view only displays information; the controller handles and responds to user input and interaction.</span></span>

<span data-ttu-id="3507f-211">To zadanie spowoduje dodanie klasy kontrolera, która będzie obsługiwać adresy URL do strony głównej witryny sklep muzyczny.</span><span class="sxs-lookup"><span data-stu-id="3507f-211">In this task, you will add a Controller class that will handle URLs to the Home page of the Music Store site.</span></span>

1. <span data-ttu-id="3507f-212">Kliknij prawym przyciskiem myszy **kontrolerów** folder w Eksploratorze rozwiązań wybierz **Dodaj** , a następnie **kontrolera** polecenia:</span><span class="sxs-lookup"><span data-stu-id="3507f-212">Right-click **Controllers** folder within the Solution Explorer, select **Add** and then **Controller** command:</span></span>

    <span data-ttu-id="3507f-213">![Dodaj polecenie kontrolera](aspnet-mvc-4-fundamentals/_static/image5.png "Dodawanie polecenia kontrolera")</span><span class="sxs-lookup"><span data-stu-id="3507f-213">![Add a Controller Command](aspnet-mvc-4-fundamentals/_static/image5.png "Add a Controller Command")</span></span>

    <span data-ttu-id="3507f-214">*Dodaj polecenie kontrolera*</span><span class="sxs-lookup"><span data-stu-id="3507f-214">*Add Controller Command*</span></span>
2. <span data-ttu-id="3507f-215">**Dodaj kontroler** zostanie wyświetlone okno dialogowe.</span><span class="sxs-lookup"><span data-stu-id="3507f-215">The **Add Controller** dialog appears.</span></span> <span data-ttu-id="3507f-216">Nazwa kontrolera *HomeController* i naciśnij klawisz **Dodaj**.</span><span class="sxs-lookup"><span data-stu-id="3507f-216">Name the controller *HomeController* and press **Add**.</span></span>

    <span data-ttu-id="3507f-217">![Kontroler okno dialogowe Dodawanie](aspnet-mvc-4-fundamentals/_static/image6.png "kontrolera okno dialogowe Dodawanie")</span><span class="sxs-lookup"><span data-stu-id="3507f-217">![Add Controller Dialog](aspnet-mvc-4-fundamentals/_static/image6.png "Add Controller Dialog")</span></span>

    <span data-ttu-id="3507f-218">*Dodawanie kontrolera okna dialogowego*</span><span class="sxs-lookup"><span data-stu-id="3507f-218">*Add Controller Dialog*</span></span>
3. <span data-ttu-id="3507f-219">Plik **HomeController.cs** jest tworzony w **kontrolerów** folderu.</span><span class="sxs-lookup"><span data-stu-id="3507f-219">The file **HomeController.cs** is created in the **Controllers** folder.</span></span> <span data-ttu-id="3507f-220">Aby przypisać **HomeController** zwraca ciąg na jego akcji indeksu, Zastąp **indeksu** metodę z następującym kodem:</span><span class="sxs-lookup"><span data-stu-id="3507f-220">In order to have the **HomeController** return a string on its Index action, replace the **Index** method with the following code:</span></span>

    <span data-ttu-id="3507f-221">(Fragment - kodu *podstawy platformy ASP.NET MVC 4 - indeksu HomeController Ex1*)</span><span class="sxs-lookup"><span data-stu-id="3507f-221">(Code Snippet - *ASP.NET MVC 4 Fundamentals - Ex1 HomeController Index*)</span></span>


    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample1.cs)]

<a id="Ex1Task4"></a>

<a id="Task_4_-_Running_the_Application"></a>
#### <a name="task-4---running-the-application"></a><span data-ttu-id="3507f-222">Zadanie 4 — uruchamianie aplikacji</span><span class="sxs-lookup"><span data-stu-id="3507f-222">Task 4 - Running the Application</span></span>

<span data-ttu-id="3507f-223">To zadanie będzie Wypróbuj tę aplikację w przeglądarce sieci web.</span><span class="sxs-lookup"><span data-stu-id="3507f-223">In this task, you will try out the Application in a web browser.</span></span>

1. <span data-ttu-id="3507f-224">Naciśnij klawisz **F5** do uruchomienia aplikacji.</span><span class="sxs-lookup"><span data-stu-id="3507f-224">Press **F5** to run the Application.</span></span> <span data-ttu-id="3507f-225">Projekt jest kompilowany i uruchamia serwer sieci Web IIS lokalnej.</span><span class="sxs-lookup"><span data-stu-id="3507f-225">The project is compiled and the local IIS Web Server starts.</span></span> <span data-ttu-id="3507f-226">Lokalny serwer sieci Web usług IIS zostanie automatycznie uruchomiona przeglądarki sieci web wskazuje adres URL serwera sieci Web.</span><span class="sxs-lookup"><span data-stu-id="3507f-226">The local IIS Web Server will automatically open a web browser pointing to the URL of the Web server.</span></span>

    <span data-ttu-id="3507f-227">![Aplikacja uruchomiona w przeglądarce sieci web](aspnet-mvc-4-fundamentals/_static/image7.png "aplikacja była uruchomiona w przeglądarce sieci web")</span><span class="sxs-lookup"><span data-stu-id="3507f-227">![Application running in a web browser](aspnet-mvc-4-fundamentals/_static/image7.png "Application running in a web browser")</span></span>

    <span data-ttu-id="3507f-228">*Aplikacja uruchomiona w przeglądarce sieci web*</span><span class="sxs-lookup"><span data-stu-id="3507f-228">*Application running in a web browser*</span></span>

    > [!NOTE]
    > <span data-ttu-id="3507f-229">Lokalnego serwera sieci Web usług IIS będą uruchamiane witryny sieci Web na numer portu wolne losowych.</span><span class="sxs-lookup"><span data-stu-id="3507f-229">The local IIS Web Server will run the website on a random free port number.</span></span> <span data-ttu-id="3507f-230">Na powyższej ilustracji lokacji działa w `http://localhost:50103/`, więc używa portu 50103.</span><span class="sxs-lookup"><span data-stu-id="3507f-230">In the figure above, the site is running at `http://localhost:50103/`, so it's using port 50103.</span></span> <span data-ttu-id="3507f-231">Numer portu może się różnić.</span><span class="sxs-lookup"><span data-stu-id="3507f-231">Your port number may vary.</span></span>
2. <span data-ttu-id="3507f-232">Zamknij przeglądarkę.</span><span class="sxs-lookup"><span data-stu-id="3507f-232">Close the browser.</span></span>

<a id="Exercise2"></a>

<a id="Exercise_2_Creating_a_Controller"></a>
### <a name="exercise-2-creating-a-controller"></a><span data-ttu-id="3507f-233">Ćwiczenie 2: Tworzenie kontrolera</span><span class="sxs-lookup"><span data-stu-id="3507f-233">Exercise 2: Creating a Controller</span></span>

<span data-ttu-id="3507f-234">W tym ćwiczeniu dowiesz się, jak zaktualizować kontroler do zaimplementowania proste funkcje aplikacji utworów muzycznych magazynu.</span><span class="sxs-lookup"><span data-stu-id="3507f-234">In this exercise, you will learn how to update the controller to implement simple functionality of the Music Store application.</span></span> <span data-ttu-id="3507f-235">Ten kontroler określi metod akcji do każdego z następujących określone żądania obsługi:</span><span class="sxs-lookup"><span data-stu-id="3507f-235">That controller will define action methods to handle each of the following specific requests:</span></span>

- <span data-ttu-id="3507f-236">Strony listę gatunkami muzyki utworów muzycznych w magazynie utworów muzycznych</span><span class="sxs-lookup"><span data-stu-id="3507f-236">A listing page of the music genres in the Music Store</span></span>
- <span data-ttu-id="3507f-237">Strona przeglądania, która zawiera listę wszystkich albumów muzycznych dla określonego rodzaju</span><span class="sxs-lookup"><span data-stu-id="3507f-237">A browse page that lists all of the music albums for a particular genre</span></span>
- <span data-ttu-id="3507f-238">Strona szczegółów, która zawiera informacje o albumu określonych utworów muzycznych</span><span class="sxs-lookup"><span data-stu-id="3507f-238">A details page that shows information about a specific music album</span></span>

<span data-ttu-id="3507f-239">W zakresie tego ćwiczenia te akcje po prostu zwróci ciąg przez teraz.</span><span class="sxs-lookup"><span data-stu-id="3507f-239">For the scope of this exercise, those actions will simply return a string by now.</span></span>

<a id="Ex2Task1"></a>

<a id="Task_1_-_Adding_a_New_StoreController_Class"></a>
#### <a name="task-1---adding-a-new-storecontroller-class"></a><span data-ttu-id="3507f-240">Zadanie 1 — Dodawanie nowej klasy StoreController</span><span class="sxs-lookup"><span data-stu-id="3507f-240">Task 1 - Adding a New StoreController Class</span></span>

<span data-ttu-id="3507f-241">To zadanie spowoduje dodanie nowego kontrolera.</span><span class="sxs-lookup"><span data-stu-id="3507f-241">In this task, you will add a new Controller.</span></span>

1. <span data-ttu-id="3507f-242">Jeśli nie już otwarty, uruchom **VS Express for Web 2012**.</span><span class="sxs-lookup"><span data-stu-id="3507f-242">If not already open, start **VS Express for Web 2012**.</span></span>
2. <span data-ttu-id="3507f-243">W **pliku** menu, wybierz **Otwórz projekt**.</span><span class="sxs-lookup"><span data-stu-id="3507f-243">In the **File** menu, choose **Open Project**.</span></span> <span data-ttu-id="3507f-244">W oknie dialogowym Otwórz projekt, przejdź do **Source\Ex02 CreatingAController\Begin**, wybierz pozycję **Begin.sln** i kliknij przycisk **Otwórz**.</span><span class="sxs-lookup"><span data-stu-id="3507f-244">In the Open Project dialog, browse to **Source\Ex02-CreatingAController\Begin**, select **Begin.sln** and click **Open**.</span></span> <span data-ttu-id="3507f-245">Alternatywnie możesz nadal z rozwiązaniem uzyskany po zakończeniu pracy w poprzednim ćwiczeniu.</span><span class="sxs-lookup"><span data-stu-id="3507f-245">Alternatively, you may continue with the solution that you obtained after completing the previous exercise.</span></span>

    1. <span data-ttu-id="3507f-246">Po otwarciu dostarczonych **rozpocząć** rozwiązania, musisz pobrać niektórych brakujących pakietów NuGet aby kontynuować.</span><span class="sxs-lookup"><span data-stu-id="3507f-246">If you opened the provided **Begin** solution, you will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="3507f-247">Aby to zrobić, kliknij przycisk **projektu** menu i wybierz **Zarządzaj pakietami NuGet**.</span><span class="sxs-lookup"><span data-stu-id="3507f-247">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
    2. <span data-ttu-id="3507f-248">W **Zarządzaj pakietami NuGet** okna dialogowego, kliknij przycisk **przywrócić** celu pobieranie brakujących pakietów.</span><span class="sxs-lookup"><span data-stu-id="3507f-248">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
    3. <span data-ttu-id="3507f-249">Na koniec Skompiluj rozwiązanie, klikając **kompilacji** | **Kompiluj rozwiązanie**.</span><span class="sxs-lookup"><span data-stu-id="3507f-249">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="3507f-250">Jedną z zalet przy użyciu narzędzia NuGet jest, że nie masz do wysłania wszystkich bibliotek w projekcie, zmniejszenie jego rozmiar projektu.</span><span class="sxs-lookup"><span data-stu-id="3507f-250">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="3507f-251">Narzędzia Power NuGet określając wersje pakietów w pliku Packages.config, będzie można pobrać wymaganych bibliotek podczas pierwszego uruchomienia projektu.</span><span class="sxs-lookup"><span data-stu-id="3507f-251">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="3507f-252">Jest to, dlaczego konieczne będzie wykonanie tych kroków, po otwarciu istniejącego rozwiązania z tego laboratorium.</span><span class="sxs-lookup"><span data-stu-id="3507f-252">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
3. <span data-ttu-id="3507f-253">Dodaj nowy kontroler.</span><span class="sxs-lookup"><span data-stu-id="3507f-253">Add the new controller.</span></span> <span data-ttu-id="3507f-254">Aby to zrobić, kliknij prawym przyciskiem myszy **kontrolerów** folder w Eksploratorze rozwiązań wybierz **Dodaj** , a następnie **kontrolera** polecenia.</span><span class="sxs-lookup"><span data-stu-id="3507f-254">To do this, right-click the **Controllers** folder within the Solution Explorer, select **Add** and then the **Controller** command.</span></span> <span data-ttu-id="3507f-255">Zmień **nazwy kontrolera** do *StoreController*i kliknij przycisk **Dodaj**.</span><span class="sxs-lookup"><span data-stu-id="3507f-255">Change the **Controller Name** to *StoreController*, and click **Add**.</span></span>

    <span data-ttu-id="3507f-256">![Kontroler okno dialogowe Dodawanie](aspnet-mvc-4-fundamentals/_static/image8.png "kontrolera okno dialogowe Dodawanie")</span><span class="sxs-lookup"><span data-stu-id="3507f-256">![Add Controller Dialog](aspnet-mvc-4-fundamentals/_static/image8.png "Add Controller Dialog")</span></span>

    <span data-ttu-id="3507f-257">*Dodawanie kontrolera okna dialogowego*</span><span class="sxs-lookup"><span data-stu-id="3507f-257">*Add Controller Dialog*</span></span>

<a id="Ex2Task2"></a>

<a id="Task_2_-_Modifying_the_StoreControllers_Actions"></a>
#### <a name="task-2---modifying-the-storecontrollers-actions"></a><span data-ttu-id="3507f-258">Zadanie 2 - modyfikowanie StoreController akcje</span><span class="sxs-lookup"><span data-stu-id="3507f-258">Task 2 - Modifying the StoreController's Actions</span></span>

<span data-ttu-id="3507f-259">To zadanie, należy zmodyfikować metod kontrolera, które są nazywane **akcje**.</span><span class="sxs-lookup"><span data-stu-id="3507f-259">In this task, you will modify the Controller methods that are called **actions**.</span></span> <span data-ttu-id="3507f-260">Akcje są odpowiedzialne za obsługę żądań adresu URL i określanie zawartość, która ma zostać odesłany do przeglądarki lub użytkownik, który wywołał adres URL.</span><span class="sxs-lookup"><span data-stu-id="3507f-260">Actions are responsible for handling URL requests and determining the content that should be sent back to the browser or user that invoked the URL.</span></span>

1. <span data-ttu-id="3507f-261">**StoreController** klasa ma już **indeksu** metody.</span><span class="sxs-lookup"><span data-stu-id="3507f-261">The **StoreController** class already has an **Index** method.</span></span> <span data-ttu-id="3507f-262">Go w dalszej części tego laboratorium zostanie użyta do wdrożenia strony, który zawiera listę wszystkich genres magazynu utworów muzycznych.</span><span class="sxs-lookup"><span data-stu-id="3507f-262">You will use it later in this Lab to implement the page that lists all genres of the music store.</span></span> <span data-ttu-id="3507f-263">Teraz, po prostu zastąpić **indeksu** metodę z następującym kodem, które zwraca ciąg &quot;Hello z Store.Index()&quot;:</span><span class="sxs-lookup"><span data-stu-id="3507f-263">For now, just replace the **Index** method with the following code that returns a string &quot;Hello from Store.Index()&quot;:</span></span>

    <span data-ttu-id="3507f-264">(Fragment - kodu *podstawy platformy ASP.NET MVC 4 - indeksu StoreController Ex2*)</span><span class="sxs-lookup"><span data-stu-id="3507f-264">(Code Snippet - *ASP.NET MVC 4 Fundamentals - Ex2 StoreController Index*)</span></span>


    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample2.cs)]
2. <span data-ttu-id="3507f-265">Dodaj **Przeglądaj** i **szczegóły** metody.</span><span class="sxs-lookup"><span data-stu-id="3507f-265">Add **Browse** and **Details** methods.</span></span> <span data-ttu-id="3507f-266">Aby to zrobić, Dodaj następujący kod, aby **StoreController**:</span><span class="sxs-lookup"><span data-stu-id="3507f-266">To do this, add the following code to the **StoreController**:</span></span>

    <span data-ttu-id="3507f-267">(Fragment - kodu *podstawy platformy ASP.NET MVC 4 - Ex2 StoreController BrowseAndDetails*)</span><span class="sxs-lookup"><span data-stu-id="3507f-267">(Code Snippet - *ASP.NET MVC 4 Fundamentals - Ex2 StoreController BrowseAndDetails*)</span></span>


    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample3.cs)]

<a id="Ex2Task3"></a>

<a id="Task_3_-_Running_the_Application"></a>
#### <a name="task-3---running-the-application"></a><span data-ttu-id="3507f-268">Zadanie 3 — uruchamianie aplikacji</span><span class="sxs-lookup"><span data-stu-id="3507f-268">Task 3 - Running the Application</span></span>

<span data-ttu-id="3507f-269">To zadanie będzie Wypróbuj tę aplikację w przeglądarce sieci web.</span><span class="sxs-lookup"><span data-stu-id="3507f-269">In this task, you will try out the Application in a web browser.</span></span>

1. <span data-ttu-id="3507f-270">Naciśnij klawisz **F5** do uruchomienia aplikacji.</span><span class="sxs-lookup"><span data-stu-id="3507f-270">Press **F5** to run the Application.</span></span>
2. <span data-ttu-id="3507f-271">Projekt jest uruchamiany w **Home** strony.</span><span class="sxs-lookup"><span data-stu-id="3507f-271">The project starts in the **Home** page.</span></span> <span data-ttu-id="3507f-272">Zmień adres URL, aby sprawdzić implementacji każdej akcji.</span><span class="sxs-lookup"><span data-stu-id="3507f-272">Change the URL to verify each action's implementation.</span></span>

    1. <span data-ttu-id="3507f-273">**/ Przechowywania**.</span><span class="sxs-lookup"><span data-stu-id="3507f-273">**/Store**.</span></span> <span data-ttu-id="3507f-274">Zobaczysz  **&quot;Hello z Store.Index()&quot;**.</span><span class="sxs-lookup"><span data-stu-id="3507f-274">You will see **&quot;Hello from Store.Index()&quot;**.</span></span>
    2. <span data-ttu-id="3507f-275">**/ Magazynu/Przeglądaj**.</span><span class="sxs-lookup"><span data-stu-id="3507f-275">**/Store/Browse**.</span></span> <span data-ttu-id="3507f-276">Zobaczysz  **&quot;Hello z Store.Browse()&quot;**.</span><span class="sxs-lookup"><span data-stu-id="3507f-276">You will see **&quot;Hello from Store.Browse()&quot;**.</span></span>
    3. <span data-ttu-id="3507f-277">**/ / Szczegóły magazynu**.</span><span class="sxs-lookup"><span data-stu-id="3507f-277">**/Store/Details**.</span></span> <span data-ttu-id="3507f-278">Zobaczysz  **&quot;Hello z Store.Details()&quot;**.</span><span class="sxs-lookup"><span data-stu-id="3507f-278">You will see **&quot;Hello from Store.Details()&quot;**.</span></span>

        <span data-ttu-id="3507f-279">![Przeglądanie StoreBrowse](aspnet-mvc-4-fundamentals/_static/image9.png "przeglądanie StoreBrowse")</span><span class="sxs-lookup"><span data-stu-id="3507f-279">![Browsing StoreBrowse](aspnet-mvc-4-fundamentals/_static/image9.png "Browsing StoreBrowse")</span></span>

        <span data-ttu-id="3507f-280">*Przeglądanie /Store/Browse*</span><span class="sxs-lookup"><span data-stu-id="3507f-280">*Browsing /Store/Browse*</span></span>
3. <span data-ttu-id="3507f-281">Zamknij przeglądarkę.</span><span class="sxs-lookup"><span data-stu-id="3507f-281">Close the browser.</span></span>

<a id="Exercise3"></a>

<a id="Exercise_3_Passing_parameters_to_a_Controller"></a>
### <a name="exercise-3-passing-parameters-to-a-controller"></a><span data-ttu-id="3507f-282">Ćwiczenie 3: Przekazywanie parametrów do kontrolera</span><span class="sxs-lookup"><span data-stu-id="3507f-282">Exercise 3: Passing parameters to a Controller</span></span>

<span data-ttu-id="3507f-283">Do tej pory mieć zostały zwracanie stałej ciągów z kontrolerów.</span><span class="sxs-lookup"><span data-stu-id="3507f-283">Until now, you have been returning constant strings from the Controllers.</span></span> <span data-ttu-id="3507f-284">W tym ćwiczeniu dowiesz się, jak do przekazania parametrów do kontrolera przy użyciu adresu URL i querystring, a następnie sprawdzając akcje metody odpowiedzi z tekstem w przeglądarce.</span><span class="sxs-lookup"><span data-stu-id="3507f-284">In this exercise you will learn how to pass parameters to a Controller using the URL and querystring, and then making the method actions respond with text to the browser.</span></span>

<a id="Ex3Task1"></a>

<a id="Task_1_-_Adding_Genre_Parameter_to_StoreController"></a>
#### <a name="task-1---adding-genre-parameter-to-storecontroller"></a><span data-ttu-id="3507f-285">Zadanie 1 — Dodawanie parametru Genre StoreController</span><span class="sxs-lookup"><span data-stu-id="3507f-285">Task 1 - Adding Genre Parameter to StoreController</span></span>

<span data-ttu-id="3507f-286">W tym zadaniu użyjesz **querystring** wysłać parametry **Przeglądaj** metody akcji w **StoreController**.</span><span class="sxs-lookup"><span data-stu-id="3507f-286">In this task, you will use the **querystring** to send parameters to the **Browse** action method in the **StoreController**.</span></span>

1. <span data-ttu-id="3507f-287">Jeśli nie już otwarty, uruchom **VS Express for Web**.</span><span class="sxs-lookup"><span data-stu-id="3507f-287">If not already open, start **VS Express for Web**.</span></span>
2. <span data-ttu-id="3507f-288">W **pliku** menu, wybierz **Otwórz projekt**.</span><span class="sxs-lookup"><span data-stu-id="3507f-288">In the **File** menu, choose **Open Project**.</span></span> <span data-ttu-id="3507f-289">W oknie dialogowym Otwórz projekt, przejdź do **Source\Ex03 PassingParametersToAController\Begin**, wybierz pozycję **Begin.sln** i kliknij przycisk **Otwórz**.</span><span class="sxs-lookup"><span data-stu-id="3507f-289">In the Open Project dialog, browse to **Source\Ex03-PassingParametersToAController\Begin**, select **Begin.sln** and click **Open**.</span></span> <span data-ttu-id="3507f-290">Alternatywnie możesz nadal z rozwiązaniem uzyskany po zakończeniu pracy w poprzednim ćwiczeniu.</span><span class="sxs-lookup"><span data-stu-id="3507f-290">Alternatively, you may continue with the solution that you obtained after completing the previous exercise.</span></span>

    1. <span data-ttu-id="3507f-291">Po otwarciu dostarczonych **rozpocząć** rozwiązania, musisz pobrać niektórych brakujących pakietów NuGet aby kontynuować.</span><span class="sxs-lookup"><span data-stu-id="3507f-291">If you opened the provided **Begin** solution, you will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="3507f-292">Aby to zrobić, kliknij przycisk **projektu** menu i wybierz **Zarządzaj pakietami NuGet**.</span><span class="sxs-lookup"><span data-stu-id="3507f-292">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
    2. <span data-ttu-id="3507f-293">W **Zarządzaj pakietami NuGet** okna dialogowego, kliknij przycisk **przywrócić** celu pobieranie brakujących pakietów.</span><span class="sxs-lookup"><span data-stu-id="3507f-293">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
    3. <span data-ttu-id="3507f-294">Na koniec Skompiluj rozwiązanie, klikając **kompilacji** | **Kompiluj rozwiązanie**.</span><span class="sxs-lookup"><span data-stu-id="3507f-294">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="3507f-295">Jedną z zalet przy użyciu narzędzia NuGet jest, że nie masz do wysłania wszystkich bibliotek w projekcie, zmniejszenie jego rozmiar projektu.</span><span class="sxs-lookup"><span data-stu-id="3507f-295">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="3507f-296">Narzędzia Power NuGet określając wersje pakietów w pliku Packages.config, będzie można pobrać wymaganych bibliotek podczas pierwszego uruchomienia projektu.</span><span class="sxs-lookup"><span data-stu-id="3507f-296">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="3507f-297">Jest to, dlaczego konieczne będzie wykonanie tych kroków, po otwarciu istniejącego rozwiązania z tego laboratorium.</span><span class="sxs-lookup"><span data-stu-id="3507f-297">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
3. <span data-ttu-id="3507f-298">Otwórz **StoreController** klasy.</span><span class="sxs-lookup"><span data-stu-id="3507f-298">Open **StoreController** class.</span></span> <span data-ttu-id="3507f-299">Aby to zrobić, w **Eksploratora rozwiązań**, rozwiń węzeł **kontrolerów** folder i kliknij dwukrotnie **StoreController.cs**.</span><span class="sxs-lookup"><span data-stu-id="3507f-299">To do this, in the **Solution Explorer**, expand the **Controllers** folder and double-click **StoreController.cs**.</span></span>
4. <span data-ttu-id="3507f-300">Zmień **Przeglądaj** metody, dodając parametr typu string na żądanie dla określonego rodzaju.</span><span class="sxs-lookup"><span data-stu-id="3507f-300">Change the **Browse** method, adding a string parameter to request for a specific genre.</span></span> <span data-ttu-id="3507f-301">ASP.NET MVC zostanie automatycznie przekazać żadnych querystring lub przesłanie formularza parametrów o nazwie **genre** do tej metody akcji, gdy została wywołana.</span><span class="sxs-lookup"><span data-stu-id="3507f-301">ASP.NET MVC will automatically pass any querystring or form post parameters named **genre** to this action method when invoked.</span></span> <span data-ttu-id="3507f-302">Aby to zrobić, Zamień **Przeglądaj** metodę z następującym kodem:</span><span class="sxs-lookup"><span data-stu-id="3507f-302">To do this, replace the **Browse** method with the following code:</span></span>

    <span data-ttu-id="3507f-303">(Fragment - kodu *podstawy platformy ASP.NET MVC 4 - Ex3 StoreController BrowseMethod*)</span><span class="sxs-lookup"><span data-stu-id="3507f-303">(Code Snippet - *ASP.NET MVC 4 Fundamentals - Ex3 StoreController BrowseMethod*)</span></span>


    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample4.cs)]

    > [!NOTE]
    > <span data-ttu-id="3507f-304">Używasz **HttpUtility.HtmlEncode** metodę narzędzie uniemożliwia użytkownikom wstrzykiwania Javascript do widoku z łączem, takich jak   **/magazynu/Przeglądaj? Genre =&lt;skryptu&gt;window.location= "[http://hackersite.com](http://hackersite.com)"&lt;/script&gt;**.</span><span class="sxs-lookup"><span data-stu-id="3507f-304">You are using the **HttpUtility.HtmlEncode** utility method to prevents users from injecting Javascript into the View with a link like **/Store/Browse?Genre=&lt;script&gt;window.location='[http://hackersite.com](http://hackersite.com)'&lt;/script&gt;**.</span></span>
    > 
    > <span data-ttu-id="3507f-305">Aby uzyskać dokładniejsze objaśnienie, odwiedź stronę [ten artykuł w witrynie msdn](https://msdn.microsoft.com/library/a2a4yykt(v=VS.80).aspx).</span><span class="sxs-lookup"><span data-stu-id="3507f-305">For further explanation, please visit [this msdn article](https://msdn.microsoft.com/library/a2a4yykt(v=VS.80).aspx).</span></span>

<a id="Ex3Task2"></a>

<a id="Task_2_-_Running_the_Application"></a>
#### <a name="task-2---running-the-application"></a><span data-ttu-id="3507f-306">Zadanie 2 — w uruchomionej aplikacji</span><span class="sxs-lookup"><span data-stu-id="3507f-306">Task 2 - Running the Application</span></span>

<span data-ttu-id="3507f-307">W tym zadaniu zostanie wypróbowanie aplikacji w przeglądarce sieci web i używać **genre** parametru.</span><span class="sxs-lookup"><span data-stu-id="3507f-307">In this task, you will try out the Application in a web browser and use the **genre** parameter.</span></span>

1. <span data-ttu-id="3507f-308">Naciśnij klawisz **F5** do uruchomienia aplikacji.</span><span class="sxs-lookup"><span data-stu-id="3507f-308">Press **F5** to run the Application.</span></span>
2. <span data-ttu-id="3507f-309">Projekt jest uruchamiany w **Home** strony.</span><span class="sxs-lookup"><span data-stu-id="3507f-309">The project starts in the **Home** page.</span></span> <span data-ttu-id="3507f-310">Zmień adres URL do   */przechowywania/Przeglądaj? Genre = Disco* można zweryfikować, że akcja otrzyma parametru genre.</span><span class="sxs-lookup"><span data-stu-id="3507f-310">Change the URL to */Store/Browse?Genre=Disco* to verify that the action receives the genre parameter.</span></span>

    <span data-ttu-id="3507f-311">![Przeglądanie StoreBrowseGenre = Disco](aspnet-mvc-4-fundamentals/_static/image10.png "przeglądanie StoreBrowseGenre = Disco")</span><span class="sxs-lookup"><span data-stu-id="3507f-311">![Browsing StoreBrowseGenre=Disco](aspnet-mvc-4-fundamentals/_static/image10.png "Browsing StoreBrowseGenre=Disco")</span></span>

    <span data-ttu-id="3507f-312">*Przeglądanie /Store/Browse? Genre = Disco*</span><span class="sxs-lookup"><span data-stu-id="3507f-312">*Browsing /Store/Browse?Genre=Disco*</span></span>
3. <span data-ttu-id="3507f-313">Zamknij przeglądarkę.</span><span class="sxs-lookup"><span data-stu-id="3507f-313">Close the browser.</span></span>

<a id="Ex3Task3"></a>

<a id="Task_3_-_Adding_an_Id_Parameter_Embedded_in_the_URL"></a>
#### <a name="task-3---adding-an-id-parameter-embedded-in-the-url"></a><span data-ttu-id="3507f-314">Zadanie 3. Dodawanie parametru identyfikatora osadzone w adresie URL</span><span class="sxs-lookup"><span data-stu-id="3507f-314">Task 3 - Adding an Id Parameter Embedded in the URL</span></span>

<span data-ttu-id="3507f-315">W tym zadaniu użyjesz **adres URL** do przekazania **identyfikator** parametr **szczegóły** metody akcji **StoreController**.</span><span class="sxs-lookup"><span data-stu-id="3507f-315">In this task, you will use the **URL** to pass an **Id** parameter to the **Details** action method of the **StoreController**.</span></span> <span data-ttu-id="3507f-316">ASP.NET MVC domyślnej konwencji routingu jest Traktuj segment adresu URL po nazwy metody akcji, jako parametr o nazwie **identyfikator**. Jeśli metodę akcji ma parametr o nazwie identyfikator, ASP.NET MVC zostanie automatycznie przekazuj segment adresu URL do użytkownika jako parametr.</span><span class="sxs-lookup"><span data-stu-id="3507f-316">ASP.NET MVC's default routing convention is to treat the segment of a URL after the action method name as a parameter named **Id**. If your action method has parameter named Id, then ASP.NET MVC will automatically pass the URL segment to you as a parameter.</span></span> <span data-ttu-id="3507f-317">W adresie URL **5-magazynu/szczegóły**, **identyfikator** zostanie potraktowany jako **5**.</span><span class="sxs-lookup"><span data-stu-id="3507f-317">In the URL **Store/Details/5**, **Id** will be interpreted as **5**.</span></span>

1. <span data-ttu-id="3507f-318">Zmień **szczegóły** metody **StoreController**, dodając **int** parametr o nazwie **identyfikator**. Aby to zrobić, Zamień **szczegóły** metodę z następującym kodem:</span><span class="sxs-lookup"><span data-stu-id="3507f-318">Change the **Details** method of the **StoreController**, adding an **int** parameter called **id**. To do this, replace the **Details** method with the following code:</span></span>

    <span data-ttu-id="3507f-319">(Fragment - kodu *podstawy platformy ASP.NET MVC 4 - Ex3 StoreController DetailsMethod*)</span><span class="sxs-lookup"><span data-stu-id="3507f-319">(Code Snippet - *ASP.NET MVC 4 Fundamentals - Ex3 StoreController DetailsMethod*)</span></span>


    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample5.cs)]

<a id="Ex3Task4"></a>

<a id="Task_4_-_Running_the_Application"></a>
#### <a name="task-4---running-the-application"></a><span data-ttu-id="3507f-320">Zadanie 4 — uruchamianie aplikacji</span><span class="sxs-lookup"><span data-stu-id="3507f-320">Task 4 - Running the Application</span></span>

<span data-ttu-id="3507f-321">W tym zadaniu zostanie wypróbowanie aplikacji w przeglądarce sieci web i używać **identyfikator** parametru.</span><span class="sxs-lookup"><span data-stu-id="3507f-321">In this task, you will try out the Application in a web browser and use the **Id** parameter.</span></span>

1. <span data-ttu-id="3507f-322">Naciśnij klawisz **F5** do uruchomienia aplikacji.</span><span class="sxs-lookup"><span data-stu-id="3507f-322">Press **F5** to run the Application.</span></span>
2. <span data-ttu-id="3507f-323">Projekt jest uruchamiany w **Home** strony.</span><span class="sxs-lookup"><span data-stu-id="3507f-323">The project starts in the **Home** page.</span></span> <span data-ttu-id="3507f-324">Zmień adres URL do */Store/Details/5* można zweryfikować, że akcja otrzyma parametru identyfikatora.</span><span class="sxs-lookup"><span data-stu-id="3507f-324">Change the URL to */Store/Details/5* to verify that the action receives the id parameter.</span></span>

    <span data-ttu-id="3507f-325">![Przeglądanie StoreDetails5](aspnet-mvc-4-fundamentals/_static/image11.png "przeglądanie StoreDetails5")</span><span class="sxs-lookup"><span data-stu-id="3507f-325">![Browsing StoreDetails5](aspnet-mvc-4-fundamentals/_static/image11.png "Browsing StoreDetails5")</span></span>

    <span data-ttu-id="3507f-326">*Przeglądanie /Store/Details/5*</span><span class="sxs-lookup"><span data-stu-id="3507f-326">*Browsing /Store/Details/5*</span></span>

<a id="Exercise4"></a>

<a id="Exercise_4_Creating_a_View"></a>
### <a name="exercise-4-creating-a-view"></a><span data-ttu-id="3507f-327">Ćwiczenie 4: Tworzenie widoku</span><span class="sxs-lookup"><span data-stu-id="3507f-327">Exercise 4: Creating a View</span></span>

<span data-ttu-id="3507f-328">Do tej pory ma zostały zwracanie ciągów z akcji kontrolera.</span><span class="sxs-lookup"><span data-stu-id="3507f-328">So far you have been returning strings from controller actions.</span></span> <span data-ttu-id="3507f-329">Chociaż jest to wygodny sposób zrozumienia, jak działają kontrolery, jest nie jak rzeczywistych aplikacji sieci Web są tworzone.</span><span class="sxs-lookup"><span data-stu-id="3507f-329">Although that is a useful way of understanding how controllers work, it is not how your real Web applications are built.</span></span> <span data-ttu-id="3507f-330">Widoki są składnikami, które zapewniają lepszym rozwiązaniem dla generowania kodu HTML do przeglądarki przy użyciu plików szablonów.</span><span class="sxs-lookup"><span data-stu-id="3507f-330">Views are components that provide a better approach for generating HTML back to the browser with the use of template files.</span></span>

<span data-ttu-id="3507f-331">W tym ćwiczeniu dowiesz się, jak dodać układ strony wzorcowej można skonfigurować szablon dla typowych zawartość HTML, arkusz stylów w celu zwiększenia wyglądu i działania witryny, a na koniec szablon widoku, aby włączyć HomeController do zwrócenia HTML.</span><span class="sxs-lookup"><span data-stu-id="3507f-331">In this exercise you will learn how to add a layout master page to setup a template for common HTML content, a StyleSheet to enhance the look and feel of the site and finally a View template to enable HomeController to return HTML.</span></span>

<a id="Ex4Task1"></a>

<a id="Task_1_-_Modifying_the_file__layoutcshtml"></a>
#### <a name="task-1---modifying-the-file-layoutcshtml"></a><span data-ttu-id="3507f-332">Zadanie 1 - modyfikowania pliku \_layout.cshtml</span><span class="sxs-lookup"><span data-stu-id="3507f-332">Task 1 - Modifying the file \_layout.cshtml</span></span>

<span data-ttu-id="3507f-333">Plik **~/Views/Shared/\_layout.cshtml** pozwala skonfigurować szablon HTML wspólne korzystanie w całej witryny sieci Web.</span><span class="sxs-lookup"><span data-stu-id="3507f-333">The file **~/Views/Shared/\_layout.cshtml** allows you to setup a template for common HTML to use across the entire website.</span></span> <span data-ttu-id="3507f-334">W tym zadaniu doda układ strony wzorcowej przy użyciu wspólnej nagłówka z łączami do obszar strony głównej i magazynu.</span><span class="sxs-lookup"><span data-stu-id="3507f-334">In this task you will add a layout master page with a common header with links to the Home page and Store area.</span></span>

1. <span data-ttu-id="3507f-335">Jeśli nie już otwarty, uruchom **VS Express for Web**.</span><span class="sxs-lookup"><span data-stu-id="3507f-335">If not already open, start **VS Express for Web**.</span></span>
2. <span data-ttu-id="3507f-336">W **pliku** menu, wybierz **Otwórz projekt**.</span><span class="sxs-lookup"><span data-stu-id="3507f-336">In the **File** menu, choose **Open Project**.</span></span> <span data-ttu-id="3507f-337">W oknie dialogowym Otwórz projekt, przejdź do **Source\Ex04 CreatingAView\Begin**, wybierz pozycję **Begin.sln** i kliknij przycisk **Otwórz**.</span><span class="sxs-lookup"><span data-stu-id="3507f-337">In the Open Project dialog, browse to **Source\Ex04-CreatingAView\Begin**, select **Begin.sln** and click **Open**.</span></span> <span data-ttu-id="3507f-338">Alternatywnie możesz nadal z rozwiązaniem uzyskany po zakończeniu pracy w poprzednim ćwiczeniu.</span><span class="sxs-lookup"><span data-stu-id="3507f-338">Alternatively, you may continue with the solution that you obtained after completing the previous exercise.</span></span>

    1. <span data-ttu-id="3507f-339">Po otwarciu dostarczonych **rozpocząć** rozwiązania, musisz pobrać niektórych brakujących pakietów NuGet aby kontynuować.</span><span class="sxs-lookup"><span data-stu-id="3507f-339">If you opened the provided **Begin** solution, you will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="3507f-340">Aby to zrobić, kliknij przycisk **projektu** menu i wybierz **Zarządzaj pakietami NuGet**.</span><span class="sxs-lookup"><span data-stu-id="3507f-340">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
    2. <span data-ttu-id="3507f-341">W **Zarządzaj pakietami NuGet** okna dialogowego, kliknij przycisk **przywrócić** celu pobieranie brakujących pakietów.</span><span class="sxs-lookup"><span data-stu-id="3507f-341">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
    3. <span data-ttu-id="3507f-342">Na koniec Skompiluj rozwiązanie, klikając **kompilacji** | **Kompiluj rozwiązanie**.</span><span class="sxs-lookup"><span data-stu-id="3507f-342">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="3507f-343">Jedną z zalet przy użyciu narzędzia NuGet jest, że nie masz do wysłania wszystkich bibliotek w projekcie, zmniejszenie jego rozmiar projektu.</span><span class="sxs-lookup"><span data-stu-id="3507f-343">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="3507f-344">Narzędzia Power NuGet określając wersje pakietów w pliku Packages.config, będzie można pobrać wymaganych bibliotek podczas pierwszego uruchomienia projektu.</span><span class="sxs-lookup"><span data-stu-id="3507f-344">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="3507f-345">Jest to, dlaczego konieczne będzie wykonanie tych kroków, po otwarciu istniejącego rozwiązania z tego laboratorium.</span><span class="sxs-lookup"><span data-stu-id="3507f-345">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
3. <span data-ttu-id="3507f-346">Plik  **\_layout.cshtml** zawiera układ kontenera HTML dla wszystkich stron w witrynie.</span><span class="sxs-lookup"><span data-stu-id="3507f-346">The file **\_layout.cshtml** contains the HTML container layout for all pages on the site.</span></span> <span data-ttu-id="3507f-347">Obejmuje on  **&lt;html&gt;**  elementu w odpowiedzi HTML, jak również  **&lt;head&gt;**  i  **&lt;treści&gt;**  elementów.</span><span class="sxs-lookup"><span data-stu-id="3507f-347">It includes the **&lt;html&gt;** element for the HTML response, as well as the **&lt;head&gt;** and **&lt;body&gt;** elements.</span></span> <span data-ttu-id="3507f-348">**@RenderBody()** w kodzie HTML treści identyfikowania regionów tego widoku szablonów będzie można wypełnić się przy użyciu zawartości dynamicznej.</span><span class="sxs-lookup"><span data-stu-id="3507f-348">**@RenderBody()** within the HTML body identify regions that view templates will be able to fill in with dynamic content.</span></span>
<span data-ttu-id="3507f-349">(C#)</span><span class="sxs-lookup"><span data-stu-id="3507f-349">(C#)</span></span>

    [!code-cshtml[Main](aspnet-mvc-4-fundamentals/samples/sample6.cshtml)]
4. <span data-ttu-id="3507f-350">Dodanie nagłówka wspólnego z łączami do obszar strony głównej i zapisać na wszystkich stron w witrynie.</span><span class="sxs-lookup"><span data-stu-id="3507f-350">Add a common header with links to the Home page and Store area on all pages in the site.</span></span> <span data-ttu-id="3507f-351">Aby to zrobić, Dodaj następujący kod poniżej &lt;treści&gt; instrukcji.</span><span class="sxs-lookup"><span data-stu-id="3507f-351">In order to do that, add the following code below &lt;body&gt; statement.</span></span>
<span data-ttu-id="3507f-352">(C#)</span><span class="sxs-lookup"><span data-stu-id="3507f-352">(C#)</span></span>

    [!code-cshtml[Main](aspnet-mvc-4-fundamentals/samples/sample7.cshtml)]
5. <span data-ttu-id="3507f-353">Obejmują div renderowanie części treści każdej strony.</span><span class="sxs-lookup"><span data-stu-id="3507f-353">Include a div to render the body section of each page.</span></span> <span data-ttu-id="3507f-354">Zastąp  **@RenderBody()** następującym kodem higlighted: (C#)</span><span class="sxs-lookup"><span data-stu-id="3507f-354">Replace **@RenderBody()** with the following higlighted code: (C#)</span></span>

    [!code-cshtml[Main](aspnet-mvc-4-fundamentals/samples/sample8.cshtml)]

    > [!NOTE]
    > <span data-ttu-id="3507f-355">Czy wiesz?</span><span class="sxs-lookup"><span data-stu-id="3507f-355">Did you know?</span></span> <span data-ttu-id="3507f-356">Program Visual Studio 2012 ma fragmentów, które ułatwiają dodawania często używane kodu HTML i pliki kodu!</span><span class="sxs-lookup"><span data-stu-id="3507f-356">Visual Studio 2012 has snippets that make it easy to add commonly used code in HTML, code files and more!</span></span> <span data-ttu-id="3507f-357">Wypróbuj limit, wpisując  **&lt;div&gt;**  i naciskając klawisz **kartę** dwa razy, aby wstawić pełnego **div** tagu.</span><span class="sxs-lookup"><span data-stu-id="3507f-357">Try it out by typing **&lt;div&gt;** and pressing **TAB** twice to insert a complete **div** tag.</span></span>

<a id="Ex4Task2"></a>

<a id="Task_2_-_Adding_CSS_Stylesheet"></a>
#### <a name="task-2---adding-css-stylesheet"></a><span data-ttu-id="3507f-358">Zadanie 2 — Dodawanie arkusza stylów CSS</span><span class="sxs-lookup"><span data-stu-id="3507f-358">Task 2 - Adding CSS Stylesheet</span></span>

<span data-ttu-id="3507f-359">Pusty szablon projektu zawiera plik CSS bardzo prostsze, zawierający tylko stylów używany do wyświetlania podstawowych formularzy i komunikatów dotyczących sprawdzania poprawności.</span><span class="sxs-lookup"><span data-stu-id="3507f-359">The empty project template includes a very streamlined CSS file which just includes styles used to display basic forms and validation messages.</span></span> <span data-ttu-id="3507f-360">Aby poprawić wygląd i działanie witryny użyje dodatkowe CSS i obrazów (potencjalnie udostępnione przez projektanta).</span><span class="sxs-lookup"><span data-stu-id="3507f-360">You will use additional CSS and images (potentially provided by a designer) in order to enhance the look and feel of the site.</span></span>

<span data-ttu-id="3507f-361">To zadanie spowoduje dodanie arkusz stylów CSS do definiowania stylów witryny.</span><span class="sxs-lookup"><span data-stu-id="3507f-361">In this task, you will add a CSS stylesheet to define the styles of the site.</span></span>

1. <span data-ttu-id="3507f-362">Pliku CSS i obrazy są uwzględnione w **Source\Assets\Content** folder tego laboratorium.</span><span class="sxs-lookup"><span data-stu-id="3507f-362">The CSS file and images are included in the **Source\Assets\Content** folder of this Lab.</span></span> <span data-ttu-id="3507f-363">Aby dodać je do aplikacji, przeciągnij ich zawartość z **Eksploratora Windows** okna do **Eksploratora rozwiązań** w programie Visual Studio Express for Web, jak pokazano poniżej:</span><span class="sxs-lookup"><span data-stu-id="3507f-363">In order to add them to the application, drag their content from a **Windows Explorer** window into the **Solution Explorer** in Visual Studio Express for Web, as shown below:</span></span>

    <span data-ttu-id="3507f-364">![Przeciąganie styl treści](aspnet-mvc-4-fundamentals/_static/image12.png "przeciąganie styl treści")</span><span class="sxs-lookup"><span data-stu-id="3507f-364">![Dragging style contents](aspnet-mvc-4-fundamentals/_static/image12.png "Dragging style contents")</span></span>

    <span data-ttu-id="3507f-365">*Przeciąganie styl treści*</span><span class="sxs-lookup"><span data-stu-id="3507f-365">*Dragging style contents*</span></span>
2. <span data-ttu-id="3507f-366">Okno dialogowe z ostrzeżeniem pojawi się pytaniem o potwierdzenie zastąpić **Site.css** plików i niektórych istniejących obrazów.</span><span class="sxs-lookup"><span data-stu-id="3507f-366">A warning dialog will appear, asking for confirmation to replace **Site.css** file and some existing images.</span></span> <span data-ttu-id="3507f-367">Sprawdź **Zastosuj do wszystkich elementów** i kliknij przycisk **tak**.</span><span class="sxs-lookup"><span data-stu-id="3507f-367">Check **Apply to all items** and click **Yes**.</span></span>

<a id="Ex4Task3"></a>

<a id="Task_3_-_Adding_a_View_Template"></a>
#### <a name="task-3---adding-a-view-template"></a><span data-ttu-id="3507f-368">Zadanie 3. Dodawanie wyświetlanie szablonu</span><span class="sxs-lookup"><span data-stu-id="3507f-368">Task 3 - Adding a View Template</span></span>

<span data-ttu-id="3507f-369">To zadanie spowoduje dodanie szablonu widoku do generowania odpowiedzi HTML, który będzie używany przez układ strony wzorcowej i CSS dodane w tym ćwiczeniu.</span><span class="sxs-lookup"><span data-stu-id="3507f-369">In this task, you will add a View template to generate the HTML response that will use the layout master page and CSS added in this exercise.</span></span>

1. <span data-ttu-id="3507f-370">Aby użyć szablonu widoku podczas przeglądania strony głównej, najpierw musisz wskazują, że zamiast zwracać ciąg, **indeksu HomeController** metoda zwróci **widoku**.</span><span class="sxs-lookup"><span data-stu-id="3507f-370">To use a View template when browsing the site's home page, you will first need to indicate that instead of returning a string, the **HomeController Index** method will return a **View**.</span></span> <span data-ttu-id="3507f-371">Otwórz **HomeController** klasy i zmień jego **indeksu** metodę, aby zwrócić **ActionResult**, i zwróć **View()**.</span><span class="sxs-lookup"><span data-stu-id="3507f-371">Open **HomeController** class and change its **Index** method to return an **ActionResult**, and have it return **View()**.</span></span>

    <span data-ttu-id="3507f-372">(Fragment - kodu *podstawy platformy ASP.NET MVC 4 - indeksu HomeController Ex4*)</span><span class="sxs-lookup"><span data-stu-id="3507f-372">(Code Snippet - *ASP.NET MVC 4 Fundamentals - Ex4 HomeController Index*)</span></span>


    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample9.cs)]
2. <span data-ttu-id="3507f-373">Teraz musisz dodać odpowiedni szablon widoku.</span><span class="sxs-lookup"><span data-stu-id="3507f-373">Now, you need to add an appropriate View template.</span></span> <span data-ttu-id="3507f-374">Aby to zrobić, **kliknij prawym przyciskiem myszy** wewnątrz **indeksu** metody akcji i wybierz **Dodaj widok**.</span><span class="sxs-lookup"><span data-stu-id="3507f-374">To do this, **right-click** inside the **Index** action method and select **Add View**.</span></span> <span data-ttu-id="3507f-375">Zostanie wyświetlone okno **Dodaj widok** okna dialogowego.</span><span class="sxs-lookup"><span data-stu-id="3507f-375">This will bring up the **Add View** dialog.</span></span>

    <span data-ttu-id="3507f-376">![Dodawanie widoku z w metodzie indeksu](aspnet-mvc-4-fundamentals/_static/image13.png "Dodawanie widoku z wewnątrz Index — metoda")</span><span class="sxs-lookup"><span data-stu-id="3507f-376">![Adding a View from within the Index method](aspnet-mvc-4-fundamentals/_static/image13.png "Adding a View from within the Index method")</span></span>

    <span data-ttu-id="3507f-377">*Dodawanie widoku z wewnątrz Index — metoda*</span><span class="sxs-lookup"><span data-stu-id="3507f-377">*Adding a View from within the Index method*</span></span>
3. <span data-ttu-id="3507f-378">**Dodaj widok** wyświetli się okno dialogowe, aby wygenerować plik szablonu widoku.</span><span class="sxs-lookup"><span data-stu-id="3507f-378">The **Add View** Dialog will appear to generate a View template file.</span></span> <span data-ttu-id="3507f-379">Domyślnie to okno dialogowe wstępnie wypełnia nazwę szablonu widoku, aby odpowiadały one metody akcji, która będzie go używać.</span><span class="sxs-lookup"><span data-stu-id="3507f-379">By default, this dialog pre-populates the name of the View template so that it matches the action method that will use it.</span></span> <span data-ttu-id="3507f-380">Ponieważ użyto **Dodaj widok** menu kontekstowego w **indeksu** metody akcji w obrębie HomeController **Dodaj widok** okno dialogowe ma indeks jako domyślną nazwę widoku.</span><span class="sxs-lookup"><span data-stu-id="3507f-380">Because you used the **Add View** context menu within the **Index** action method within the HomeController, the **Add View** dialog has Index as the default view name.</span></span> <span data-ttu-id="3507f-381">Kliknij przycisk **Dodaj**.</span><span class="sxs-lookup"><span data-stu-id="3507f-381">Click **Add**.</span></span>

    <span data-ttu-id="3507f-382">![Okno dialogowe dodawania widoku](aspnet-mvc-4-fundamentals/_static/image14.png "okno dialogowe dodawania widoku")</span><span class="sxs-lookup"><span data-stu-id="3507f-382">![Add View Dialog](aspnet-mvc-4-fundamentals/_static/image14.png "Add View Dialog")</span></span>

    <span data-ttu-id="3507f-383">*Okno dialogowe dodawania widoku*</span><span class="sxs-lookup"><span data-stu-id="3507f-383">*Add View Dialog*</span></span>
4. <span data-ttu-id="3507f-384">Visual Studio generuje **Index.cshtml** Wyświetl szablon wewnątrz **Views\Home** folder, a następnie go otwiera.</span><span class="sxs-lookup"><span data-stu-id="3507f-384">Visual Studio generates an **Index.cshtml** view template inside the **Views\Home** folder and then opens it.</span></span>

    <span data-ttu-id="3507f-385">![Strona główna widok indeksu utworzony](aspnet-mvc-4-fundamentals/_static/image15.png "widok Home Indeks utworzony")</span><span class="sxs-lookup"><span data-stu-id="3507f-385">![Home Index view created](aspnet-mvc-4-fundamentals/_static/image15.png "Home Index view created")</span></span>

    <span data-ttu-id="3507f-386">*Macierzysty widok indeksu utworzony*</span><span class="sxs-lookup"><span data-stu-id="3507f-386">*Home Index view created*</span></span>

    > [!NOTE]
    > <span data-ttu-id="3507f-387">Nazwa i lokalizacja **Index.cshtml** plik ma zastosowanie i jest zgodna z domyślnych konwencji nazewnictwa platformy ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="3507f-387">name and location of the **Index.cshtml** file is relevant and follows the default ASP.NET MVC naming conventions.</span></span>
    > 
    > <span data-ttu-id="3507f-388">Folder \Views\**Home** zgodna z nazwą kontrolera (**Home** kontrolera).</span><span class="sxs-lookup"><span data-stu-id="3507f-388">The folder \Views\**Home** matches the controller name (**Home** Controller).</span></span> <span data-ttu-id="3507f-389">Nazwa szablonu widoku (**indeksu**), metoda akcji kontrolera, które będą wyświetlane w widoku jest zgodna.</span><span class="sxs-lookup"><span data-stu-id="3507f-389">The View template name (**Index**), matches the controller action method which will be displaying the View.</span></span>
    > 
    > <span data-ttu-id="3507f-390">W ten sposób ASP.NET MVC pozwala uniknąć konieczności jawnego określania nazwy lub lokalizacji szablonu widoku, używając tę konwencję nazewnictwa do zwrócenia widoku.</span><span class="sxs-lookup"><span data-stu-id="3507f-390">This way, ASP.NET MVC avoids having to explicitly specify the name or location of a View template when using this naming convention to return a View.</span></span>
5. <span data-ttu-id="3507f-391">Wygenerowany szablon widoku jest oparty na  **\_layout.cshtml** wcześniej zdefiniowanego szablonu.</span><span class="sxs-lookup"><span data-stu-id="3507f-391">The generated View template is based on the **\_layout.cshtml** template earlier defined.</span></span> <span data-ttu-id="3507f-392">Zaktualizuj właściwość ViewBag.Title do **Home**i zmień głównej zawartości do **to strona główna**, jak pokazano w poniższym kodzie:</span><span class="sxs-lookup"><span data-stu-id="3507f-392">Update the ViewBag.Title property to **Home**, and change the main content to **This is the Home Page**, as shown in the code below:</span></span>


    [!code-cshtml[Main](aspnet-mvc-4-fundamentals/samples/sample10.cshtml)]
6. <span data-ttu-id="3507f-393">Wybierz **MvcMusicStore** projekt w Eksploratorze rozwiązań i naciśnij klawisz **F5** do uruchomienia aplikacji.</span><span class="sxs-lookup"><span data-stu-id="3507f-393">Select **MvcMusicStore** project in the Solution Explorer and Press **F5** to run the Application.</span></span>

<a id="Ex4Task4"></a>

<a id="Task_4_Verification"></a>
#### <a name="task-4-verification"></a><span data-ttu-id="3507f-394">Zadanie 4: Weryfikacja</span><span class="sxs-lookup"><span data-stu-id="3507f-394">Task 4: Verification</span></span>

<span data-ttu-id="3507f-395">Aby sprawdzić, prawidłowo wykonane wszystkie kroki opisane w poprzednim ćwiczeniu, wykonać następujące czynności:</span><span class="sxs-lookup"><span data-stu-id="3507f-395">In order to verify that you have correctly performed all the steps in the previous exercise, proceed as follows:</span></span>

<span data-ttu-id="3507f-396">Z tą aplikacją otwarta w przeglądarce należy zauważyć, że:</span><span class="sxs-lookup"><span data-stu-id="3507f-396">With the application opened in a browser, you should note that:</span></span>

1. <span data-ttu-id="3507f-397">Metody akcji indeksu HomeController znaleźć i wyświetlić **\Views\Home\Index.cshtml** wyświetlić szablon, nawet jeśli kod wywołuje **zwracać View()**, ponieważ szablon widoku, a następnie standardowej konwencji nazewnictwa.</span><span class="sxs-lookup"><span data-stu-id="3507f-397">The HomeController's Index action method found and displayed the **\Views\Home\Index.cshtml** View template, even though the code called **return View()**, because the View template followed the standard naming convention.</span></span>
2. <span data-ttu-id="3507f-398">Strona główna wyświetla komunikat powitalny, zdefiniowanego w **\Views\Home\Index.cshtml** widok szablonu.</span><span class="sxs-lookup"><span data-stu-id="3507f-398">The Home Page displays the welcome message defined within the **\Views\Home\Index.cshtml** view template.</span></span>
3. <span data-ttu-id="3507f-399">Strona główna używa  **\_layout.cshtml** szablonu, co komunikat powitalny jest zawarty w układu standardowe witryny HTML.</span><span class="sxs-lookup"><span data-stu-id="3507f-399">The Home Page is using the **\_layout.cshtml** template, and so the welcome message is contained within the standard site HTML layout.</span></span>

    <span data-ttu-id="3507f-400">![Strona główna widoku indeksu przy użyciu zdefiniowanych LayoutPage i styl](aspnet-mvc-4-fundamentals/_static/image16.png "Home widoku indeksu przy użyciu zdefiniowanych LayoutPage i stylu")</span><span class="sxs-lookup"><span data-stu-id="3507f-400">![Home Index View using the defined LayoutPage and style](aspnet-mvc-4-fundamentals/_static/image16.png "Home Index View using the defined LayoutPage and style")</span></span>

    <span data-ttu-id="3507f-401">*Macierzysty widoku indeksu przy użyciu zdefiniowanych LayoutPage i stylu*</span><span class="sxs-lookup"><span data-stu-id="3507f-401">*Home Index View using the defined LayoutPage and style*</span></span>

<a id="Exercise5"></a>

<a id="Exercise_5_Creating_a_View_Model"></a>
### <a name="exercise-5-creating-a-view-model"></a><span data-ttu-id="3507f-402">Ćwiczenie 5: Tworzenie modelu widoku</span><span class="sxs-lookup"><span data-stu-id="3507f-402">Exercise 5: Creating a View Model</span></span>

<span data-ttu-id="3507f-403">Do tej pory wprowadzono widoków wyświetlić zapisane na stałe HTML, ale, aby można było utworzyć dynamicznych aplikacji sieci web, Wyświetl szablon powinien zostać wyświetlony informacji od kontrolera.</span><span class="sxs-lookup"><span data-stu-id="3507f-403">So far, you made your Views display hardcoded HTML, but, in order to create dynamic web applications, the View template should receive information from the Controller.</span></span> <span data-ttu-id="3507f-404">Jeden technika wspólnego do użycia w tym celu jest **ViewModel** wzorzec, dzięki czemu kontrolera spakować wszystkie informacje potrzebne do generowania odpowiednich odpowiedzi HTML.</span><span class="sxs-lookup"><span data-stu-id="3507f-404">One common technique to be used for that purpose is the **ViewModel** pattern, which allows a Controller to package up all the information needed to generate the appropriate HTML response.</span></span>

<span data-ttu-id="3507f-405">W tym ćwiczeniu dowiesz Tworzenie klasy ViewModel i dodać wymagane właściwości: numer gatunkami muzyki w magazynie i listę tych gatunkami muzyki.</span><span class="sxs-lookup"><span data-stu-id="3507f-405">In this exercise, you will learn how to create a ViewModel class and add the required properties: the number of genres in the store and a list of those genres.</span></span> <span data-ttu-id="3507f-406">Zostanie również zaktualizować StoreController do użycia ViewModel utworzony, a koniec utworzysz nowy szablon widoku, który spowoduje wyświetlenie właściwości wymienione na stronie.</span><span class="sxs-lookup"><span data-stu-id="3507f-406">You will also update the StoreController to use the created ViewModel, and finally, you will create a new View template that will display the mentioned properties in the page.</span></span>

<a id="Ex5Task1"></a>

<a id="Task_1_-_Creating_a_ViewModel_Class"></a>
#### <a name="task-1---creating-a-viewmodel-class"></a><span data-ttu-id="3507f-407">Zadanie 1 — Tworzenie klasy ViewModel</span><span class="sxs-lookup"><span data-stu-id="3507f-407">Task 1 - Creating a ViewModel Class</span></span>

<span data-ttu-id="3507f-408">W ramach tego zadania spowoduje utworzenie klasy ViewModel, która będzie wdrożyć scenariusz dla listy genre magazynu.</span><span class="sxs-lookup"><span data-stu-id="3507f-408">In this task, you will create a ViewModel class that will implement the Store genre listing scenario.</span></span>

1. <span data-ttu-id="3507f-409">Jeśli nie już otwarty, uruchom **VS Express for Web**.</span><span class="sxs-lookup"><span data-stu-id="3507f-409">If not already open, start **VS Express for Web**.</span></span>
2. <span data-ttu-id="3507f-410">W **pliku** menu, wybierz **Otwórz projekt**.</span><span class="sxs-lookup"><span data-stu-id="3507f-410">In the **File** menu, choose **Open Project**.</span></span> <span data-ttu-id="3507f-411">W oknie dialogowym Otwórz projekt, przejdź do **Source\Ex05 CreatingAViewModel\Begin**, wybierz pozycję **Begin.sln** i kliknij przycisk **Otwórz**.</span><span class="sxs-lookup"><span data-stu-id="3507f-411">In the Open Project dialog, browse to **Source\Ex05-CreatingAViewModel\Begin**, select **Begin.sln** and click **Open**.</span></span> <span data-ttu-id="3507f-412">Alternatywnie możesz nadal z rozwiązaniem uzyskany po zakończeniu pracy w poprzednim ćwiczeniu.</span><span class="sxs-lookup"><span data-stu-id="3507f-412">Alternatively, you may continue with the solution that you obtained after completing the previous exercise.</span></span>

    1. <span data-ttu-id="3507f-413">Po otwarciu dostarczonych **rozpocząć** rozwiązania, musisz pobrać niektórych brakujących pakietów NuGet aby kontynuować.</span><span class="sxs-lookup"><span data-stu-id="3507f-413">If you opened the provided **Begin** solution, you will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="3507f-414">Aby to zrobić, kliknij przycisk **projektu** menu i wybierz **Zarządzaj pakietami NuGet**.</span><span class="sxs-lookup"><span data-stu-id="3507f-414">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
    2. <span data-ttu-id="3507f-415">W **Zarządzaj pakietami NuGet** okna dialogowego, kliknij przycisk **przywrócić** celu pobieranie brakujących pakietów.</span><span class="sxs-lookup"><span data-stu-id="3507f-415">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
    3. <span data-ttu-id="3507f-416">Na koniec Skompiluj rozwiązanie, klikając **kompilacji** | **Kompiluj rozwiązanie**.</span><span class="sxs-lookup"><span data-stu-id="3507f-416">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="3507f-417">Jedną z zalet przy użyciu narzędzia NuGet jest, że nie masz do wysłania wszystkich bibliotek w projekcie, zmniejszenie jego rozmiar projektu.</span><span class="sxs-lookup"><span data-stu-id="3507f-417">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="3507f-418">Narzędzia Power NuGet określając wersje pakietów w pliku Packages.config, będzie można pobrać wymaganych bibliotek podczas pierwszego uruchomienia projektu.</span><span class="sxs-lookup"><span data-stu-id="3507f-418">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="3507f-419">Jest to, dlaczego konieczne będzie wykonanie tych kroków, po otwarciu istniejącego rozwiązania z tego laboratorium.</span><span class="sxs-lookup"><span data-stu-id="3507f-419">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
3. <span data-ttu-id="3507f-420">Utwórz **ViewModels** folder do przechowywania ViewModel.</span><span class="sxs-lookup"><span data-stu-id="3507f-420">Create a **ViewModels** folder to hold the ViewModel.</span></span> <span data-ttu-id="3507f-421">Aby to zrobić, kliknij prawym przyciskiem myszy najwyższego poziomu **MvcMusicStore** projektu, zaznacz **Dodaj** , a następnie **nowy Folder**.</span><span class="sxs-lookup"><span data-stu-id="3507f-421">To do this, right-click the top-level **MvcMusicStore** project, select **Add** and then **New Folder**.</span></span>

    <span data-ttu-id="3507f-422">![Dodawanie nowego folderu](aspnet-mvc-4-fundamentals/_static/image17.png "dodawanie nowy folder")</span><span class="sxs-lookup"><span data-stu-id="3507f-422">![Adding a new folder](aspnet-mvc-4-fundamentals/_static/image17.png "Adding a new folder")</span></span>

    <span data-ttu-id="3507f-423">*Dodawanie nowego folderu*</span><span class="sxs-lookup"><span data-stu-id="3507f-423">*Adding a new folder*</span></span>
4. <span data-ttu-id="3507f-424">Nazwa folderu *ViewModels*.</span><span class="sxs-lookup"><span data-stu-id="3507f-424">Name the folder *ViewModels*.</span></span>

    <span data-ttu-id="3507f-425">![ViewModels folder w Eksploratorze rozwiązań](aspnet-mvc-4-fundamentals/_static/image18.png "ViewModels folder w Eksploratorze rozwiązań")</span><span class="sxs-lookup"><span data-stu-id="3507f-425">![ViewModels folder in Solution Explorer](aspnet-mvc-4-fundamentals/_static/image18.png "ViewModels folder in Solution Explorer")</span></span>

    <span data-ttu-id="3507f-426">*ViewModels folder w Eksploratorze rozwiązań*</span><span class="sxs-lookup"><span data-stu-id="3507f-426">*ViewModels folder in Solution Explorer*</span></span>
5. <span data-ttu-id="3507f-427">Utwórz **ViewModel** klasy.</span><span class="sxs-lookup"><span data-stu-id="3507f-427">Create a **ViewModel** class.</span></span> <span data-ttu-id="3507f-428">Aby to zrobić, kliknij prawym przyciskiem myszy **ViewModels** folderu niedawno utworzona, wybierz **Dodaj** , a następnie **nowy element**.</span><span class="sxs-lookup"><span data-stu-id="3507f-428">To do this, right-click on the **ViewModels** folder recently created, select **Add** and then **New Item**.</span></span> <span data-ttu-id="3507f-429">W obszarze **kod**, wybierz **klasy** elementu i nazwij plik *StoreIndexViewModel.cs*, następnie kliknij przycisk **Dodaj**.</span><span class="sxs-lookup"><span data-stu-id="3507f-429">Under **Code**, choose the **Class** item and name the file *StoreIndexViewModel.cs*, then click **Add**.</span></span>

    <span data-ttu-id="3507f-430">![Dodawanie nowej klasy](aspnet-mvc-4-fundamentals/_static/image19.png "Dodawanie nowej klasy")</span><span class="sxs-lookup"><span data-stu-id="3507f-430">![Adding a new Class](aspnet-mvc-4-fundamentals/_static/image19.png "Adding a new Class")</span></span>

    <span data-ttu-id="3507f-431">*Dodawanie nowej klasy*</span><span class="sxs-lookup"><span data-stu-id="3507f-431">*Adding a new Class*</span></span>

    <span data-ttu-id="3507f-432">![Tworzenie klasy StoreIndexViewModel](aspnet-mvc-4-fundamentals/_static/image20.png "StoreIndexViewModel Tworzenie klasy")</span><span class="sxs-lookup"><span data-stu-id="3507f-432">![Creating StoreIndexViewModel class](aspnet-mvc-4-fundamentals/_static/image20.png "Creating StoreIndexViewModel class")</span></span>

    <span data-ttu-id="3507f-433">*Tworzenie klasy StoreIndexViewModel*</span><span class="sxs-lookup"><span data-stu-id="3507f-433">*Creating StoreIndexViewModel class*</span></span>

<a id="Ex5Task2"></a>

<a id="Task_2_-_Adding_Properties_to_the_ViewModel_class"></a>
#### <a name="task-2---adding-properties-to-the-viewmodel-class"></a><span data-ttu-id="3507f-434">Zadanie 2 — Dodawanie właściwości do klasy ViewModel</span><span class="sxs-lookup"><span data-stu-id="3507f-434">Task 2 - Adding Properties to the ViewModel class</span></span>

<span data-ttu-id="3507f-435">Istnieją dwa parametry do przekazania z StoreController do szablonu widoku w celu wygenerowania oczekiwanej odpowiedzi HTML: numer gatunkami muzyki w magazynie i listę tych gatunkami muzyki.</span><span class="sxs-lookup"><span data-stu-id="3507f-435">There are two parameters to be passed from the StoreController to the View template in order to generate the expected HTML response: the number of genres in the store and a list of those genres.</span></span>

<span data-ttu-id="3507f-436">To zadanie spowoduje dodanie tych właściwości 2 w celu **StoreIndexViewModel** klasy: **NumberOfGenres** (liczba całkowita) i **Genres** (lista ciągów).</span><span class="sxs-lookup"><span data-stu-id="3507f-436">In this task, you will add those 2 properties to the **StoreIndexViewModel** class: **NumberOfGenres** (an integer) and **Genres** (a list of strings).</span></span>

1. <span data-ttu-id="3507f-437">Dodaj **NumberOfGenres** i **Genres** właściwości **StoreIndexViewModel** klasy.</span><span class="sxs-lookup"><span data-stu-id="3507f-437">Add **NumberOfGenres** and **Genres** properties to the **StoreIndexViewModel** class.</span></span> <span data-ttu-id="3507f-438">Aby to zrobić, Dodaj następujące wiersze 2 do definicji klasy:</span><span class="sxs-lookup"><span data-stu-id="3507f-438">To do this, add the following 2 lines to the class definition:</span></span>

    <span data-ttu-id="3507f-439">(Fragment - kodu *ASP.NET MVC 4 podstawy — właściwości Ex5 StoreIndexViewModel*)</span><span class="sxs-lookup"><span data-stu-id="3507f-439">(Code Snippet - *ASP.NET MVC 4 Fundamentals - Ex5 StoreIndexViewModel properties*)</span></span>


    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample11.cs)]

    > [!NOTE]
    > <span data-ttu-id="3507f-440">**{Get; Ustaw;}**  notacji korzysta z C# w funkcji właściwości zaimplementowane automatycznie.</span><span class="sxs-lookup"><span data-stu-id="3507f-440">The **{ get; set; }** notation makes use of C#'s auto-implemented properties feature.</span></span> <span data-ttu-id="3507f-441">Zapewnia korzyści właściwości bez konieczności nam zadeklarować polem zapasowym.</span><span class="sxs-lookup"><span data-stu-id="3507f-441">It provides the benefits of a property without requiring us to declare a backing field.</span></span>

<a id="Ex5Task3"></a>

<a id="Task_3_-_Updating_StoreController_to_use_the_StoreIndexViewModel"></a>
#### <a name="task-3---updating-storecontroller-to-use-the-storeindexviewmodel"></a><span data-ttu-id="3507f-442">Zadanie 3 — aktualizowanie StoreController do użycia StoreIndexViewModel</span><span class="sxs-lookup"><span data-stu-id="3507f-442">Task 3 - Updating StoreController to use the StoreIndexViewModel</span></span>

<span data-ttu-id="3507f-443">**StoreIndexViewModel** klasa hermetyzuje informacje wymagane do przekazania z **StoreController**w **indeksu** metodę szablonu widoku w celu wygenerowania odpowiedzi .</span><span class="sxs-lookup"><span data-stu-id="3507f-443">The **StoreIndexViewModel** class encapsulates the information needed to pass from **StoreController**'s **Index** method to a View template in order to generate a response.</span></span>

<span data-ttu-id="3507f-444">To zadanie zaktualizuje **StoreController** do używania **StoreIndexViewModel**.</span><span class="sxs-lookup"><span data-stu-id="3507f-444">In this task, you will update the **StoreController** to use the **StoreIndexViewModel**.</span></span>

1. <span data-ttu-id="3507f-445">Otwórz **StoreController** klasy.</span><span class="sxs-lookup"><span data-stu-id="3507f-445">Open **StoreController** class.</span></span>

    <span data-ttu-id="3507f-446">![Otwieranie klasy StoreController](aspnet-mvc-4-fundamentals/_static/image21.png "StoreController otwierania — klasa")</span><span class="sxs-lookup"><span data-stu-id="3507f-446">![Opening StoreController class](aspnet-mvc-4-fundamentals/_static/image21.png "Opening StoreController class")</span></span>

    <span data-ttu-id="3507f-447">*Otwieranie StoreController klasy*</span><span class="sxs-lookup"><span data-stu-id="3507f-447">*Opening StoreController class*</span></span>
2. <span data-ttu-id="3507f-448">Aby można było używać **StoreIndexViewModel** klasę z **StoreController**, Dodaj następujący obszar nazw w górnej części **StoreController** kodu:</span><span class="sxs-lookup"><span data-stu-id="3507f-448">In order to use the **StoreIndexViewModel** class from the **StoreController**, add the following namespace at the top of the **StoreController** code:</span></span>

    <span data-ttu-id="3507f-449">(Fragment - kodu *ASP.NET MVC 4 podstawy — za pomocą ViewModels StoreIndexViewModel Ex5*)</span><span class="sxs-lookup"><span data-stu-id="3507f-449">(Code Snippet - *ASP.NET MVC 4 Fundamentals - Ex5 StoreIndexViewModel using ViewModels*)</span></span>


    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample12.cs)]
3. <span data-ttu-id="3507f-450">Zmień **StoreController**w **indeksu** metody akcji, którą tworzy i wypełnia **StoreIndexViewModel** obiektu i przekazuje ją do szablonu widoku w celu Generowanie odpowiedzi HTML z nim.</span><span class="sxs-lookup"><span data-stu-id="3507f-450">Change the **StoreController**'s **Index** action method so that it creates and populates a **StoreIndexViewModel** object and then passes it off to a View template to generate an HTML response with it.</span></span>

    > [!NOTE]
    > <span data-ttu-id="3507f-451">W laboratorium &quot;ASP.NET MVC modeli i dostęp do danych&quot; będzie pisania kodu, który pobiera listę gatunkami muzyki magazynu z bazy danych.</span><span class="sxs-lookup"><span data-stu-id="3507f-451">In Lab &quot;ASP.NET MVC Models and Data Access&quot; you will write code that retrieves the list of store genres from a database.</span></span> <span data-ttu-id="3507f-452">W poniższym kodzie utworzysz **listy** gatunkami muzyki fikcyjny danych, które wypełnia **StoreIndexViewModel**.</span><span class="sxs-lookup"><span data-stu-id="3507f-452">In the following code, you will create a **List** of dummy data genres that will populate the **StoreIndexViewModel**.</span></span>
    > 
    > <span data-ttu-id="3507f-453">Po utworzeniu i konfigurowanie **StoreIndexViewModel** obiektu, zostanie przekazany jako argument **widoku** metody.</span><span class="sxs-lookup"><span data-stu-id="3507f-453">After creating and setting up the **StoreIndexViewModel** object, it will be passed as an argument to the **View** method.</span></span> <span data-ttu-id="3507f-454">Oznacza to, że szablon widoku będzie używać tego obiektu do generowania odpowiedzi HTML z nim.</span><span class="sxs-lookup"><span data-stu-id="3507f-454">This indicates that the View template will use that object to generate an HTML response with it.</span></span>
4. <span data-ttu-id="3507f-455">Zastąp **indeksu** metodę z następującym kodem:</span><span class="sxs-lookup"><span data-stu-id="3507f-455">Replace the **Index** method with the following code:</span></span>

    <span data-ttu-id="3507f-456">(Fragment - kodu *ASP.NET MVC 4 podstawy — metoda indeksu StoreController Ex5*)</span><span class="sxs-lookup"><span data-stu-id="3507f-456">(Code Snippet - *ASP.NET MVC 4 Fundamentals - Ex5 StoreController Index method*)</span></span>


    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample13.cs)]

    > [!NOTE]
    > <span data-ttu-id="3507f-457">Jeśli znasz C#, może założyć to przy użyciu **var** oznacza, że **viewModel** zmienna jest późnym wiązaniem.</span><span class="sxs-lookup"><span data-stu-id="3507f-457">If you're unfamiliar with C#, you may assume that using **var** means that the **viewModel** variable is late-bound.</span></span> <span data-ttu-id="3507f-458">Nie jest to poprawna - kompilatora C# używa wnioskowanie typów w oparciu o przypisania do zmiennej do określenia, który **viewModel** jest typu **StoreIndexViewModel**.</span><span class="sxs-lookup"><span data-stu-id="3507f-458">That's not correct - the C# compiler is using type-inference based on what you assign to the variable to determine that **viewModel** is of type **StoreIndexViewModel**.</span></span> <span data-ttu-id="3507f-459">Ponadto uzyskując kompilowanie lokalnej **viewModel** zmiennej jako **StoreIndexViewModel** typ ma sprawdzanie kompilacji get i pomocy technicznej edytora kodu programu Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="3507f-459">Also, by compiling the local **viewModel** variable as a **StoreIndexViewModel** type you get compile-time checking and Visual Studio code-editor support.</span></span>

<a id="Ex5Task4"></a>

<a id="Task_4_-_Creating_a_View_Template_that_Uses_StoreIndexViewModel"></a>
#### <a name="task-4---creating-a-view-template-that-uses-storeindexviewmodel"></a><span data-ttu-id="3507f-460">Zadanie 4 — Tworzenie szablonu widoku, który używa StoreIndexViewModel</span><span class="sxs-lookup"><span data-stu-id="3507f-460">Task 4 - Creating a View Template that Uses StoreIndexViewModel</span></span>

<span data-ttu-id="3507f-461">W ramach tego zadania zostanie utworzony szablon widoku, który będzie używany obiekt StoreIndexViewModel przekazany z kontrolera do wyświetlenia listy gatunkami muzyki.</span><span class="sxs-lookup"><span data-stu-id="3507f-461">In this task, you will create a View template that will use a StoreIndexViewModel object passed from the Controller to display a list of genres.</span></span>

1. <span data-ttu-id="3507f-462">Przed utworzeniem nowego szablonu widoku, utworzymy projektu, aby **dialogowe dodawania widoku** zna **StoreIndexViewModel** klasy.</span><span class="sxs-lookup"><span data-stu-id="3507f-462">Before creating the new View template, let's build the project so that the **Add View Dialog** knows about the **StoreIndexViewModel** class.</span></span> <span data-ttu-id="3507f-463">Skompiluj projekt, wybierając **kompilacji** element menu, a następnie **kompilacji MvcMusicStore**.</span><span class="sxs-lookup"><span data-stu-id="3507f-463">Build the project by selecting the **Build** menu item and then **Build MvcMusicStore**.</span></span>

    <span data-ttu-id="3507f-464">![Tworzenie projektu](aspnet-mvc-4-fundamentals/_static/image22.png "skompilowanie projektu")</span><span class="sxs-lookup"><span data-stu-id="3507f-464">![Building the project](aspnet-mvc-4-fundamentals/_static/image22.png "Building the project")</span></span>

    <span data-ttu-id="3507f-465">*Tworzenie projektu*</span><span class="sxs-lookup"><span data-stu-id="3507f-465">*Building the project*</span></span>
2. <span data-ttu-id="3507f-466">Utwórz nowy szablon widoku.</span><span class="sxs-lookup"><span data-stu-id="3507f-466">Create a new View template.</span></span> <span data-ttu-id="3507f-467">W tym celu kliknij prawym przyciskiem myszy wewnątrz **indeksu** metodę i wybierz **Dodaj widok**.</span><span class="sxs-lookup"><span data-stu-id="3507f-467">To do that, right-click inside the **Index** method and select **Add View**.</span></span>

    <span data-ttu-id="3507f-468">![Dodawanie widoku](aspnet-mvc-4-fundamentals/_static/image23.png "dodawania widoku")</span><span class="sxs-lookup"><span data-stu-id="3507f-468">![Adding a View](aspnet-mvc-4-fundamentals/_static/image23.png "Adding a View")</span></span>

    <span data-ttu-id="3507f-469">*Dodawanie widoku*</span><span class="sxs-lookup"><span data-stu-id="3507f-469">*Adding a View*</span></span>
3. <span data-ttu-id="3507f-470">Ponieważ **dialogowe dodawania widoku** została wywołana z **StoreController**, spowoduje to dodanie Wyświetl szablon domyślny **\Views\Store\Index.cshtml** pliku.</span><span class="sxs-lookup"><span data-stu-id="3507f-470">Because the **Add View Dialog** was invoked from the **StoreController**, it will add the View template by default in a **\Views\Store\Index.cshtml** file.</span></span> <span data-ttu-id="3507f-471">Sprawdź **utworzyć silnie wpisane — widok** pole wyboru, a następnie wybierz **StoreIndexViewModel** jako **klasa modelu**.</span><span class="sxs-lookup"><span data-stu-id="3507f-471">Check the **Create a strongly-typed-view** checkbox and then select **StoreIndexViewModel** as the **Model class**.</span></span> <span data-ttu-id="3507f-472">Ponadto upewnij się, że wybrany aparat widoku jest **Razor**.</span><span class="sxs-lookup"><span data-stu-id="3507f-472">Also, make sure that the View engine selected is **Razor**.</span></span> <span data-ttu-id="3507f-473">Kliknij przycisk **Dodaj**.</span><span class="sxs-lookup"><span data-stu-id="3507f-473">Click **Add**.</span></span>

    <span data-ttu-id="3507f-474">![Okno dialogowe dodawania widoku](aspnet-mvc-4-fundamentals/_static/image24.png "okno dialogowe dodawania widoku")</span><span class="sxs-lookup"><span data-stu-id="3507f-474">![Add View Dialog](aspnet-mvc-4-fundamentals/_static/image24.png "Add View Dialog")</span></span>

    <span data-ttu-id="3507f-475">*Okno dialogowe dodawania widoku*</span><span class="sxs-lookup"><span data-stu-id="3507f-475">*Add View Dialog*</span></span>

    <span data-ttu-id="3507f-476">**\Views\Store\Index.cshtml** widoku pliku szablonu jest utworzony i otwarty.</span><span class="sxs-lookup"><span data-stu-id="3507f-476">The **\Views\Store\Index.cshtml** View template file is created and opened.</span></span> <span data-ttu-id="3507f-477">Na podstawie informacji do **Dodaj widok** okna dialogowego w ostatnim kroku, widok szablonu będzie oczekiwać **StoreIndexViewModel** wystąpienia jako danych używany do generowania odpowiedzi HTML.</span><span class="sxs-lookup"><span data-stu-id="3507f-477">Based on the information provided to the **Add View** dialog in the last step, the View template will expect a **StoreIndexViewModel** instance as the data to use to generate an HTML response.</span></span> <span data-ttu-id="3507f-478">Można zauważyć, że szablon dziedziczy `ViewPage<musicstore.viewmodels.storeindexviewmodel>` w języku C#.</span><span class="sxs-lookup"><span data-stu-id="3507f-478">You will notice that the template inherits a `ViewPage<musicstore.viewmodels.storeindexviewmodel>` in C#.</span></span>

<a id="Ex5Task5"></a>

<a id="Task_5_-_Updating_the_View_Template"></a>
#### <a name="task-5---updating-the-view-template"></a><span data-ttu-id="3507f-479">Zadanie 5 aktualizowanie szablonu widoku.</span><span class="sxs-lookup"><span data-stu-id="3507f-479">Task 5 - Updating the View Template</span></span>

<span data-ttu-id="3507f-480">To zadanie zaktualizuje Wyświetl szablon utworzony w ostatnim zadaniem, aby pobrać liczbę genres i ich nazwy na stronie.</span><span class="sxs-lookup"><span data-stu-id="3507f-480">In this task, you will update the View template created in the last task to retrieve the number of genres and their names within the page.</span></span>

> [!NOTE]
> <span data-ttu-id="3507f-481">Użyjesz @ składni (często określany jako &quot;kodu nuggets&quot;) do wykonywania kodu w szablonie widoku.</span><span class="sxs-lookup"><span data-stu-id="3507f-481">You will use @ syntax (often referred to as &quot;code nuggets&quot;) to execute code within the View template.</span></span>


1. <span data-ttu-id="3507f-482">W **Index.cshtml** pliku poziomu **magazynu** folderu, zastąp jego kod następującym kodem:</span><span class="sxs-lookup"><span data-stu-id="3507f-482">In the **Index.cshtml** file, within the **Store** folder, replace its code with the following:</span></span>


    [!code-cshtml[Main](aspnet-mvc-4-fundamentals/samples/sample14.cshtml)]

    > [!NOTE]
    > <span data-ttu-id="3507f-483">Zaraz po zakończeniu wpisywania okres, po słowie **modelu**, Intellisense programu Visual Studio wyświetli listę możliwych właściwości i metod do wyboru.</span><span class="sxs-lookup"><span data-stu-id="3507f-483">As soon as you finish typing the period after the word **Model**, Visual Studio's Intellisense will show a list of possible properties and methods to choose from.</span></span>
    > 
    > ![](aspnet-mvc-4-fundamentals/_static/image25.png)
    > 
    > <span data-ttu-id="3507f-484">*Pobieranie właściwości modelu i metody o IntelliSense programu Visual Studio*</span><span class="sxs-lookup"><span data-stu-id="3507f-484">*Getting Model properties and methods with Visual Studio's IntelliSense*</span></span>
    > 
    > <span data-ttu-id="3507f-485">**Modelu** odwołań do właściwości **StoreIndexViewModel** obiekt, który kontroler przekazanych do szablonu widoku.</span><span class="sxs-lookup"><span data-stu-id="3507f-485">The **Model** property references the **StoreIndexViewModel** object that the Controller passed to the View template.</span></span> <span data-ttu-id="3507f-486">Oznacza to, że można uzyskać dostęp do wszystkich danych przekazanych z kontrolera do widoku szablonu za pomocą **modelu** właściwości i sformatuj go do odpowiedniego odpowiedzi HTML w szablonie widoku.</span><span class="sxs-lookup"><span data-stu-id="3507f-486">This means that you can access all of the data passed from the Controller to the View template via the **Model** property, and format it into an appropriate HTML response within the View template.</span></span>
    > 
    > <span data-ttu-id="3507f-487">Można wybrać **NumberOfGenres** listy właściwości z funkcji Intellisense, zamiast wpisując je, a następnie go zostanie automatycznie uzupełniać go przez naciśnięcie przycisku **klawisza tab**.</span><span class="sxs-lookup"><span data-stu-id="3507f-487">You can just select the **NumberOfGenres** property from the Intellisense list rather than typing it in and then it will auto-complete it by pressing the **tab key**.</span></span>
2. <span data-ttu-id="3507f-488">Pętla za pośrednictwem listy genre w **StoreIndexViewModel** i tworzenia kodu HTML  **&lt;ul&gt;**  listy przy użyciu **foreach** pętli.</span><span class="sxs-lookup"><span data-stu-id="3507f-488">Loop over the genre list in **StoreIndexViewModel** and create an HTML **&lt;ul&gt;** list using a **foreach** loop.</span></span>
<span data-ttu-id="3507f-489">(C#)</span><span class="sxs-lookup"><span data-stu-id="3507f-489">(C#)</span></span>

    [!code-cshtml[Main](aspnet-mvc-4-fundamentals/samples/sample15.cshtml)]
3. <span data-ttu-id="3507f-490">Naciśnij klawisz **F5** uruchomić aplikację, a następnie przejdź **/magazynu**.</span><span class="sxs-lookup"><span data-stu-id="3507f-490">Press **F5** to run the Application and browse **/Store**.</span></span> <span data-ttu-id="3507f-491">Zostanie wyświetlona lista gatunkami muzyki przekazano **StoreIndexViewModel** obiekt z **StoreController** do szablonu widoku.</span><span class="sxs-lookup"><span data-stu-id="3507f-491">You will see the list of genres passed in the **StoreIndexViewModel** object from the **StoreController** to the View template.</span></span>

    <span data-ttu-id="3507f-492">![Wyświetlanie listy gatunkami muzyki widoku](aspnet-mvc-4-fundamentals/_static/image26.png "widoku wyświetlanie listy gatunkami muzyki")</span><span class="sxs-lookup"><span data-stu-id="3507f-492">![View displaying a list of genres](aspnet-mvc-4-fundamentals/_static/image26.png "View displaying a list of genres")</span></span>

    <span data-ttu-id="3507f-493">*Wyświetlanie listy gatunkami muzyki widoku*</span><span class="sxs-lookup"><span data-stu-id="3507f-493">*View displaying a list of genres*</span></span>
4. <span data-ttu-id="3507f-494">Zamknij przeglądarkę.</span><span class="sxs-lookup"><span data-stu-id="3507f-494">Close the browser.</span></span>

<a id="Exercise6"></a>

<a id="Exercise_6_Using_Parameters_in_View"></a>
### <a name="exercise-6-using-parameters-in-view"></a><span data-ttu-id="3507f-495">Ćwiczenie 6: W widoku przy użyciu parametrów</span><span class="sxs-lookup"><span data-stu-id="3507f-495">Exercise 6: Using Parameters in View</span></span>

<span data-ttu-id="3507f-496">Ćwiczenie 3 przedstawiono sposób przekazania parametrów do kontrolera.</span><span class="sxs-lookup"><span data-stu-id="3507f-496">In Exercise 3 you learned how to pass parameters to the Controller.</span></span> <span data-ttu-id="3507f-497">W tym ćwiczeniu dowiesz się, jak używać tych parametrów w szablonie widoku.</span><span class="sxs-lookup"><span data-stu-id="3507f-497">In this exercise, you will learn how to use those parameters in the View template.</span></span> <span data-ttu-id="3507f-498">W tym celu należy zostaną wprowadzone najpierw do klasy modeli, które ułatwiają zarządzanie logika danych i domeny.</span><span class="sxs-lookup"><span data-stu-id="3507f-498">For that purpose, you will be introduced first to Model classes that will help you to manage your data and domain logic.</span></span> <span data-ttu-id="3507f-499">Ponadto dowiesz sposobu tworzenia łączy do stron w aplikacji ASP.NET MVC bez obaw rzeczy, takich jak kodowanie ścieżki adresu URL.</span><span class="sxs-lookup"><span data-stu-id="3507f-499">Additionally, you will learn how to create links to pages inside the ASP.NET MVC application without worrying of things like URL paths encoding.</span></span>

<a id="Ex6Task1"></a>

<a id="Task_1_-_Adding_Model_Classes"></a>
#### <a name="task-1---adding-model-classes"></a><span data-ttu-id="3507f-500">Zadanie 1 — Dodawanie klasy modelu</span><span class="sxs-lookup"><span data-stu-id="3507f-500">Task 1 - Adding Model Classes</span></span>

<span data-ttu-id="3507f-501">W odróżnieniu od ViewModels, które są tworzone tylko w celu przekazywania informacji z kontrolera do widoku, klasy modeli są tworzone zawierają i zarządzanie nimi logiki danych i domeny.</span><span class="sxs-lookup"><span data-stu-id="3507f-501">Unlike ViewModels, which are created just to pass information from the Controller to the View, Model classes are built to contain and manage data and domain logic.</span></span> <span data-ttu-id="3507f-502">W ramach tego zadania zostaną dodane dwie klasy modelu do reprezentowania tych pojęć: **Genre** i **albumu**.</span><span class="sxs-lookup"><span data-stu-id="3507f-502">In this task you will add two model classes to represent these concepts: **Genre** and **Album**.</span></span>

1. <span data-ttu-id="3507f-503">Jeśli nie już otwarty, uruchom **VS Express for Web**</span><span class="sxs-lookup"><span data-stu-id="3507f-503">If not already open, start **VS Express for Web**</span></span>
2. <span data-ttu-id="3507f-504">W **pliku** menu, wybierz **Otwórz projekt**.</span><span class="sxs-lookup"><span data-stu-id="3507f-504">In the **File** menu, choose **Open Project**.</span></span> <span data-ttu-id="3507f-505">W oknie dialogowym Otwórz projekt, przejdź do **Source\Ex06 UsingParametersInView\Begin**, wybierz pozycję **Begin.sln** i kliknij przycisk **Otwórz**.</span><span class="sxs-lookup"><span data-stu-id="3507f-505">In the Open Project dialog, browse to **Source\Ex06-UsingParametersInView\Begin**, select **Begin.sln** and click **Open**.</span></span> <span data-ttu-id="3507f-506">Alternatywnie możesz nadal z rozwiązaniem uzyskany po zakończeniu pracy w poprzednim ćwiczeniu.</span><span class="sxs-lookup"><span data-stu-id="3507f-506">Alternatively, you may continue with the solution that you obtained after completing the previous exercise.</span></span>

    1. <span data-ttu-id="3507f-507">Po otwarciu dostarczonych **rozpocząć** rozwiązania, musisz pobrać niektórych brakujących pakietów NuGet aby kontynuować.</span><span class="sxs-lookup"><span data-stu-id="3507f-507">If you opened the provided **Begin** solution, you will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="3507f-508">Aby to zrobić, kliknij przycisk **projektu** menu i wybierz **Zarządzaj pakietami NuGet**.</span><span class="sxs-lookup"><span data-stu-id="3507f-508">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
    2. <span data-ttu-id="3507f-509">W **Zarządzaj pakietami NuGet** okna dialogowego, kliknij przycisk **przywrócić** celu pobieranie brakujących pakietów.</span><span class="sxs-lookup"><span data-stu-id="3507f-509">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
    3. <span data-ttu-id="3507f-510">Na koniec Skompiluj rozwiązanie, klikając **kompilacji** | **Kompiluj rozwiązanie**.</span><span class="sxs-lookup"><span data-stu-id="3507f-510">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="3507f-511">Jedną z zalet przy użyciu narzędzia NuGet jest, że nie masz do wysłania wszystkich bibliotek w projekcie, zmniejszenie jego rozmiar projektu.</span><span class="sxs-lookup"><span data-stu-id="3507f-511">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="3507f-512">Narzędzia Power NuGet określając wersje pakietów w pliku Packages.config, będzie można pobrać wymaganych bibliotek podczas pierwszego uruchomienia projektu.</span><span class="sxs-lookup"><span data-stu-id="3507f-512">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="3507f-513">Jest to, dlaczego konieczne będzie wykonanie tych kroków, po otwarciu istniejącego rozwiązania z tego laboratorium.</span><span class="sxs-lookup"><span data-stu-id="3507f-513">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
3. <span data-ttu-id="3507f-514">Dodaj **Genre** klasa modelu.</span><span class="sxs-lookup"><span data-stu-id="3507f-514">Add a **Genre** Model class.</span></span> <span data-ttu-id="3507f-515">Aby to zrobić, kliknij prawym przyciskiem myszy **modele** folderu w **Eksploratora rozwiązań**, wybierz pozycję **Dodaj** , a następnie **nowy element** opcji.</span><span class="sxs-lookup"><span data-stu-id="3507f-515">To do this, right-click the **Models** folder in the **Solution Explorer**, select **Add** and then the **New Item** option.</span></span> <span data-ttu-id="3507f-516">W obszarze **kod**, wybierz **klasy** elementu i nazwij plik *Genre.cs*, następnie kliknij przycisk **Dodaj**.</span><span class="sxs-lookup"><span data-stu-id="3507f-516">Under **Code**, choose the **Class** item and name the file *Genre.cs*, then click **Add**.</span></span>

    <span data-ttu-id="3507f-517">![Dodawanie klasy](aspnet-mvc-4-fundamentals/_static/image27.png "Dodawanie klasy")</span><span class="sxs-lookup"><span data-stu-id="3507f-517">![Adding a class](aspnet-mvc-4-fundamentals/_static/image27.png "Adding a class")</span></span>

    <span data-ttu-id="3507f-518">*Dodawanie nowego elementu*</span><span class="sxs-lookup"><span data-stu-id="3507f-518">*Adding a new item*</span></span>

    <span data-ttu-id="3507f-519">![Dodaj klasę modelu Genre](aspnet-mvc-4-fundamentals/_static/image28.png "Dodaj klasę modelu Genre")</span><span class="sxs-lookup"><span data-stu-id="3507f-519">![Add Genre Model Class](aspnet-mvc-4-fundamentals/_static/image28.png "Add Genre Model Class")</span></span>

    <span data-ttu-id="3507f-520">*Dodaj klasę modelu Genre*</span><span class="sxs-lookup"><span data-stu-id="3507f-520">*Add Genre Model Class*</span></span>
4. <span data-ttu-id="3507f-521">Dodaj **nazwa** właściwości do klasy Genre.</span><span class="sxs-lookup"><span data-stu-id="3507f-521">Add a **Name** property to the Genre class.</span></span> <span data-ttu-id="3507f-522">Aby to zrobić, Dodaj następujący kod:</span><span class="sxs-lookup"><span data-stu-id="3507f-522">To do this, add the following code:</span></span>

    <span data-ttu-id="3507f-523">(Fragment - kodu *podstawy platformy ASP.NET MVC 4 - Ex6 Genre*)</span><span class="sxs-lookup"><span data-stu-id="3507f-523">(Code Snippet - *ASP.NET MVC 4 Fundamentals - Ex6 Genre*)</span></span>


    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample16.cs)]
5. <span data-ttu-id="3507f-524">Procedury jak wcześniej, Dodaj **albumu** klasy.</span><span class="sxs-lookup"><span data-stu-id="3507f-524">Following the same procedure as before, add an **Album** class.</span></span> <span data-ttu-id="3507f-525">Aby to zrobić, kliknij prawym przyciskiem myszy **modele** folderu w **Eksploratora rozwiązań**, wybierz pozycję **Dodaj** , a następnie **nowy element** opcji.</span><span class="sxs-lookup"><span data-stu-id="3507f-525">To do this, right-click the **Models** folder in the **Solution Explorer**, select **Add** and then the **New Item** option.</span></span> <span data-ttu-id="3507f-526">W obszarze **kod**, wybierz **klasy** elementu i nazwij plik *Album.cs*, następnie kliknij przycisk **Dodaj**.</span><span class="sxs-lookup"><span data-stu-id="3507f-526">Under **Code**, choose the **Class** item and name the file *Album.cs*, then click **Add**.</span></span>
6. <span data-ttu-id="3507f-527">Dodaj dwie właściwości do klasy albumu: **Genre** i **tytuł**.</span><span class="sxs-lookup"><span data-stu-id="3507f-527">Add two properties to the Album class: **Genre** and **Title**.</span></span> <span data-ttu-id="3507f-528">Aby to zrobić, Dodaj następujący kod:</span><span class="sxs-lookup"><span data-stu-id="3507f-528">To do this, add the following code:</span></span>

    <span data-ttu-id="3507f-529">(Fragment - kodu *podstawy platformy ASP.NET MVC 4 - albumu Ex6*)</span><span class="sxs-lookup"><span data-stu-id="3507f-529">(Code Snippet - *ASP.NET MVC 4 Fundamentals - Ex6 Album*)</span></span>


    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample17.cs)]

<a id="Ex6Task2"></a>

<a id="Task_2_-_Adding_a_StoreBrowseViewModel"></a>
#### <a name="task-2---adding-a-storebrowseviewmodel"></a><span data-ttu-id="3507f-530">Zadanie 2 — Dodawanie StoreBrowseViewModel</span><span class="sxs-lookup"><span data-stu-id="3507f-530">Task 2 - Adding a StoreBrowseViewModel</span></span>

<span data-ttu-id="3507f-531">A **StoreBrowseViewModel** będzie używany w tym zadaniu pokazanie albumy, zgodne wybranego rodzaju.</span><span class="sxs-lookup"><span data-stu-id="3507f-531">A **StoreBrowseViewModel** will be used in this task to show the Albums that match a selected Genre.</span></span> <span data-ttu-id="3507f-532">To zadanie będzie utworzyć tę klasę, a następnie dodaj dwie właściwości do obsługi **Genre** i jego **albumu**na liście.</span><span class="sxs-lookup"><span data-stu-id="3507f-532">In this task, you will create this class and then add two properties to handle the **Genre** and its **Album**'s List.</span></span>

1. <span data-ttu-id="3507f-533">Dodaj **StoreBrowseViewModel** klasy.</span><span class="sxs-lookup"><span data-stu-id="3507f-533">Add a **StoreBrowseViewModel** class.</span></span> <span data-ttu-id="3507f-534">Aby to zrobić, kliknij prawym przyciskiem myszy **ViewModels** folderu w **Eksploratora rozwiązań**, wybierz pozycję **Dodaj** , a następnie **nowy element** opcji.</span><span class="sxs-lookup"><span data-stu-id="3507f-534">To do this, right-click the **ViewModels** folder in the **Solution Explorer**, select **Add** and then the **New Item** option.</span></span> <span data-ttu-id="3507f-535">W obszarze **kod**, wybierz **klasy** elementu i nazwij plik *StoreBrowseViewModel.cs*, następnie kliknij przycisk **Dodaj**.</span><span class="sxs-lookup"><span data-stu-id="3507f-535">Under **Code**, choose the **Class** item and name the file *StoreBrowseViewModel.cs*, then click **Add**.</span></span>
2. <span data-ttu-id="3507f-536">Dodaj odwołanie do modeli w **StoreBrowseViewModel** klasy.</span><span class="sxs-lookup"><span data-stu-id="3507f-536">Add a reference to the Models in **StoreBrowseViewModel** class.</span></span> <span data-ttu-id="3507f-537">Aby to zrobić, Dodaj następujący za pomocą przestrzeni nazw:</span><span class="sxs-lookup"><span data-stu-id="3507f-537">To do this, add the following using namespace:</span></span>

    <span data-ttu-id="3507f-538">(Fragment - kodu *podstawy platformy ASP.NET MVC 4 - Ex6 UsingModel*)</span><span class="sxs-lookup"><span data-stu-id="3507f-538">(Code Snippet - *ASP.NET MVC 4 Fundamentals - Ex6 UsingModel*)</span></span>


    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample18.cs)]
3. <span data-ttu-id="3507f-539">Dodaj dwie właściwości do **StoreBrowseViewModel** klasy: **Genre** i **albumów**.</span><span class="sxs-lookup"><span data-stu-id="3507f-539">Add two properties to **StoreBrowseViewModel** class: **Genre** and **Albums**.</span></span> <span data-ttu-id="3507f-540">Aby to zrobić, Dodaj następujący kod:</span><span class="sxs-lookup"><span data-stu-id="3507f-540">To do this, add the following code:</span></span>

    <span data-ttu-id="3507f-541">(Fragment - kodu *podstawy platformy ASP.NET MVC 4 - Ex6 ModelProperties*)</span><span class="sxs-lookup"><span data-stu-id="3507f-541">(Code Snippet - *ASP.NET MVC 4 Fundamentals - Ex6 ModelProperties*)</span></span>


    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample19.cs)]

    > [!NOTE]
    > <span data-ttu-id="3507f-542">Co to jest **listy&lt;albumu&gt;**  ?: używa tej definicji **listy&lt;T&gt;**  typu, których **T** ogranicza Typ, na których elementy tego **listy** należy w tym przypadku **albumu** (lub któregokolwiek z jej obiektów podrzędnych).</span><span class="sxs-lookup"><span data-stu-id="3507f-542">What is **List&lt;Album&gt;** ?: This definition is using the **List&lt;T&gt;** type, where **T** constrains the type to which elements of this **List** belong to, in this case **Album** (or any of its descendants).</span></span>
    > 
    > <span data-ttu-id="3507f-543">Ta możliwość projektowania klasy i metody, które mają być odroczone specyfikację jeden lub więcej typów, aż klasa lub metoda jest zadeklarowany i utworzone przez kod klienta jest funkcją języka C# o nazwie **ogólne**.</span><span class="sxs-lookup"><span data-stu-id="3507f-543">This ability to design classes and methods that defer the specification of one or more types until the class or method is declared and instantiated by client code is a feature of the C# language called **Generics**.</span></span>
    > 
    > <span data-ttu-id="3507f-544">**Lista&lt;T&gt;**  ogólny odpowiednik **ArrayList** typu i jest dostępny w **system.Collections.Generic —** przestrzeni nazw.</span><span class="sxs-lookup"><span data-stu-id="3507f-544">**List&lt;T&gt;** is the generic equivalent of the **ArrayList** type and is available in the **System.Collections.Generic** namespace.</span></span> <span data-ttu-id="3507f-545">Jedną z korzyści wynikające ze stosowania **ogólne** jest, że ponieważ typem jest określona, nie trzeba zajmie się sprawdzanie operacji, takich jak elementy do rzutowania typu **albumu** jak z **ArrayList**.</span><span class="sxs-lookup"><span data-stu-id="3507f-545">One of the benefits of using **generics** is that since the type is specified, you do not need to take care of type checking operations such as casting the elements into **Album** as you would do with an **ArrayList**.</span></span>

<a id="Ex6Task3"></a>

<a id="Task_3_-_Using_the_New_ViewModel_in_the_StoreController"></a>
#### <a name="task-3---using-the-new-viewmodel-in-the-storecontroller"></a><span data-ttu-id="3507f-546">Zadanie 3 - w StoreController przy użyciu nowego ViewModel</span><span class="sxs-lookup"><span data-stu-id="3507f-546">Task 3 - Using the New ViewModel in the StoreController</span></span>

<span data-ttu-id="3507f-547">To zadanie, należy zmodyfikować **StoreController**w **Przeglądaj** i **szczegóły** metod akcji do używania nowych **StoreBrowseViewModel** .</span><span class="sxs-lookup"><span data-stu-id="3507f-547">In this task, you will modify the **StoreController**'s **Browse** and **Details** action methods to use the new **StoreBrowseViewModel**.</span></span>

1. <span data-ttu-id="3507f-548">Dodaj odwołanie do folderu modeli w **StoreController** klasy.</span><span class="sxs-lookup"><span data-stu-id="3507f-548">Add a reference to the Models folder in **StoreController** class.</span></span> <span data-ttu-id="3507f-549">Aby to zrobić, rozwiń węzeł **kontrolerów** folderu w **Eksploratora rozwiązań** , a następnie otwórz **StoreController** klasy.</span><span class="sxs-lookup"><span data-stu-id="3507f-549">To do this, expand the **Controllers** folder in the **Solution Explorer** and open the **StoreController** class.</span></span> <span data-ttu-id="3507f-550">Następnie dodaj następujący kod:</span><span class="sxs-lookup"><span data-stu-id="3507f-550">Then add the following code:</span></span>

    <span data-ttu-id="3507f-551">(Fragment - kodu *podstawy platformy ASP.NET MVC 4 - Ex6 UsingModelInController*)</span><span class="sxs-lookup"><span data-stu-id="3507f-551">(Code Snippet - *ASP.NET MVC 4 Fundamentals - Ex6 UsingModelInController*)</span></span>


    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample20.cs)]
2. <span data-ttu-id="3507f-552">Zastąp **Przeglądaj** metody akcji, aby użyć **StoreViewBrowseController** klasy.</span><span class="sxs-lookup"><span data-stu-id="3507f-552">Replace the **Browse** action method to use the **StoreViewBrowseController** class.</span></span> <span data-ttu-id="3507f-553">Utworzysz określonego rodzaju i dwa nowe obiekty albumów fikcyjny danych (w następnej laboratorium Hands-on będą korzystać prawdziwe dane z bazy danych).</span><span class="sxs-lookup"><span data-stu-id="3507f-553">You will create a Genre and two new Albums objects with dummy data (in the next Hands-on Lab you will consume real data from a database).</span></span> <span data-ttu-id="3507f-554">Aby to zrobić, Zamień **Przeglądaj** metodę z następującym kodem:</span><span class="sxs-lookup"><span data-stu-id="3507f-554">To do this, replace the **Browse** method with the following code:</span></span>

    <span data-ttu-id="3507f-555">(Fragment - kodu *podstawy platformy ASP.NET MVC 4 - Ex6 BrowseMethod*)</span><span class="sxs-lookup"><span data-stu-id="3507f-555">(Code Snippet - *ASP.NET MVC 4 Fundamentals - Ex6 BrowseMethod*)</span></span>


    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample21.cs)]
3. <span data-ttu-id="3507f-556">Zastąp **szczegóły** metody akcji, aby użyć **StoreViewBrowseController** klasy.</span><span class="sxs-lookup"><span data-stu-id="3507f-556">Replace the **Details** action method to use the **StoreViewBrowseController** class.</span></span> <span data-ttu-id="3507f-557">Utworzy nowy **albumu** obiekt ma zostać zwrócona do **widoku**.</span><span class="sxs-lookup"><span data-stu-id="3507f-557">You will create a new **Album** object to be returned to the **View**.</span></span> <span data-ttu-id="3507f-558">Aby to zrobić, Zamień **szczegóły** metodę z następującym kodem:</span><span class="sxs-lookup"><span data-stu-id="3507f-558">To do this, replace the **Details** method with the following code:</span></span>

    <span data-ttu-id="3507f-559">(Fragment - kodu *podstawy platformy ASP.NET MVC 4 - Ex6 DetailsMethod*)</span><span class="sxs-lookup"><span data-stu-id="3507f-559">(Code Snippet - *ASP.NET MVC 4 Fundamentals - Ex6 DetailsMethod*)</span></span>


    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample22.cs)]

<a id="Ex6Task4"></a>

<a id="Task_4_-_Adding_a_Browse_View_Template"></a>
#### <a name="task-4---adding-a-browse-view-template"></a><span data-ttu-id="3507f-560">Zadanie 4 — Dodawanie szablonu widoku przeglądania</span><span class="sxs-lookup"><span data-stu-id="3507f-560">Task 4 - Adding a Browse View Template</span></span>

<span data-ttu-id="3507f-561">To zadanie spowoduje dodanie **Przeglądaj** widok, aby pokazać albumów znaleziono dla określonego rodzaju.</span><span class="sxs-lookup"><span data-stu-id="3507f-561">In this task, you will add a **Browse** View to show the Albums found for a specific Genre.</span></span>

1. <span data-ttu-id="3507f-562">Przed utworzeniem nowego szablonu widoku, należy skompilować projekt, aby **Dodaj widok** okna dialogowego zna **ViewModel** klasy do użycia.</span><span class="sxs-lookup"><span data-stu-id="3507f-562">Before creating the new View template, you should build the project so that the **Add View** Dialog knows about the **ViewModel** class to use.</span></span> <span data-ttu-id="3507f-563">Skompiluj projekt, wybierając **kompilacji** element menu, a następnie **kompilacji MvcMusicStore**.</span><span class="sxs-lookup"><span data-stu-id="3507f-563">Build the project by selecting the **Build** menu item and then **Build MvcMusicStore**.</span></span>
2. <span data-ttu-id="3507f-564">Dodaj **Przeglądaj** widoku.</span><span class="sxs-lookup"><span data-stu-id="3507f-564">Add a **Browse** View.</span></span> <span data-ttu-id="3507f-565">Aby to zrobić, kliknij prawym przyciskiem myszy w **Przeglądaj** metody akcji **StoreController** i kliknij przycisk **Dodaj widok**.</span><span class="sxs-lookup"><span data-stu-id="3507f-565">To do this, right-click in the **Browse** action method of the **StoreController** and click **Add View**.</span></span>
3. <span data-ttu-id="3507f-566">W **Dodaj widok** okna dialogowego upewnij się, że nazwa widoku jest **Przeglądaj**.</span><span class="sxs-lookup"><span data-stu-id="3507f-566">In the **Add View** dialog box, verify that the View Name is **Browse**.</span></span> <span data-ttu-id="3507f-567">Sprawdź **utworzyć widok jednoznacznie** wyboru i wybierz **StoreBrowseViewModel** z **klasa modelu** listy rozwijanej.</span><span class="sxs-lookup"><span data-stu-id="3507f-567">Check the **Create a strongly-typed view** checkbox and select **StoreBrowseViewModel** from the **Model class** dropdown.</span></span> <span data-ttu-id="3507f-568">Pozostaw wartości domyślne innych pól.</span><span class="sxs-lookup"><span data-stu-id="3507f-568">Leave the other fields with their default value.</span></span> <span data-ttu-id="3507f-569">Następnie kliknij przycisk **Dodaj**.</span><span class="sxs-lookup"><span data-stu-id="3507f-569">Then click **Add**.</span></span>

    <span data-ttu-id="3507f-570">![Dodawanie widoku przeglądania](aspnet-mvc-4-fundamentals/_static/image29.png "Dodawanie widoku przeglądania")</span><span class="sxs-lookup"><span data-stu-id="3507f-570">![Adding a Browse View](aspnet-mvc-4-fundamentals/_static/image29.png "Adding a Browse View")</span></span>

    <span data-ttu-id="3507f-571">*Dodawanie widoku przeglądania*</span><span class="sxs-lookup"><span data-stu-id="3507f-571">*Adding a Browse View*</span></span>
4. <span data-ttu-id="3507f-572">Modyfikowanie **Browse.cshtml** do wyświetlania informacji Genre, uzyskiwanie dostępu do **StoreBrowseViewModel** obiekt, który jest przekazywany do szablonu widoku.</span><span class="sxs-lookup"><span data-stu-id="3507f-572">Modify the **Browse.cshtml** to display the Genre's information, accessing the **StoreBrowseViewModel** object that is passed to the view template.</span></span> <span data-ttu-id="3507f-573">Aby to zrobić, Zastąp zawartość następującym kodem: (C#)</span><span class="sxs-lookup"><span data-stu-id="3507f-573">To do this, replace the content with the following: (C#)</span></span>

    [!code-cshtml[Main](aspnet-mvc-4-fundamentals/samples/sample23.cshtml)]

<a id="Ex6Task5"></a>

<a id="Task_5_-_Running_the_Application"></a>
#### <a name="task-5---running-the-application"></a><span data-ttu-id="3507f-574">Zadanie 5 działania aplikacji.</span><span class="sxs-lookup"><span data-stu-id="3507f-574">Task 5 - Running the Application</span></span>

<span data-ttu-id="3507f-575">W tym zadaniu zostanie sprawdzić, czy **Przeglądaj** metoda pobiera albumów z **Przeglądaj** metody akcji.</span><span class="sxs-lookup"><span data-stu-id="3507f-575">In this task, you will test that the **Browse** method retrieves Albums from the **Browse** method action.</span></span>

1. <span data-ttu-id="3507f-576">Naciśnij klawisz **F5** do uruchomienia aplikacji.</span><span class="sxs-lookup"><span data-stu-id="3507f-576">Press **F5** to run the Application.</span></span>
2. <span data-ttu-id="3507f-577">Na stronie głównej datę rozpoczęcia projektu.</span><span class="sxs-lookup"><span data-stu-id="3507f-577">The project starts in the Home page.</span></span> <span data-ttu-id="3507f-578">Zmień adres URL do   **/przechowywania/Przeglądaj? Genre = Disco** Aby sprawdzić, czy akcji zwraca dwa albumów.</span><span class="sxs-lookup"><span data-stu-id="3507f-578">Change the URL to **/Store/Browse?Genre=Disco** to verify that the action returns two Albums.</span></span>

    <span data-ttu-id="3507f-579">![Przeglądanie magazynu albumów Disco](aspnet-mvc-4-fundamentals/_static/image30.png "przeglądanie albumów Disco magazynu")</span><span class="sxs-lookup"><span data-stu-id="3507f-579">![Browsing Store Disco Albums](aspnet-mvc-4-fundamentals/_static/image30.png "Browsing Store Disco Albums")</span></span>

    <span data-ttu-id="3507f-580">*Przeglądanie albumów Disco magazynu*</span><span class="sxs-lookup"><span data-stu-id="3507f-580">*Browsing Store Disco Albums*</span></span>

<a id="Ex6Task6"></a>

<a id="Task_6_-_Displaying_information_About_a_Specific_Album"></a>
#### <a name="task-6---displaying-information-about-a-specific-album"></a><span data-ttu-id="3507f-581">Zadanie 6 — wyświetlanie informacji o określonych albumu</span><span class="sxs-lookup"><span data-stu-id="3507f-581">Task 6 - Displaying information About a Specific Album</span></span>

<span data-ttu-id="3507f-582">W tym zadaniu zostaną zaimplementowane **szczegóły dotyczące i magazynu** widok, aby wyświetlić informacje o określonym albumu.</span><span class="sxs-lookup"><span data-stu-id="3507f-582">In this task, you will implement the **Store/Details** view to display information about a specific album.</span></span> <span data-ttu-id="3507f-583">W tym laboratorium Hands-on wszystko, co spowoduje wyświetlenie o album znajduje się już w **widoku** szablonu.</span><span class="sxs-lookup"><span data-stu-id="3507f-583">In this Hands-on Lab, everything you will display about the album is already contained in the **View** template.</span></span> <span data-ttu-id="3507f-584">Tak, zamiast tworzyć **StoreDetailsViewModel** klasy, użyje bieżącego **StoreBrowseViewModel** szablonu przekazywanie Album do niego.</span><span class="sxs-lookup"><span data-stu-id="3507f-584">So, instead of creating a **StoreDetailsViewModel** class, you will use the current **StoreBrowseViewModel** template passing the Album to it.</span></span>

1. <span data-ttu-id="3507f-585">Zamknij przeglądarkę, jeśli to konieczne, aby powrócić do okna programu Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="3507f-585">Close the browser if needed, to return to the Visual Studio window.</span></span> <span data-ttu-id="3507f-586">Dodaj nową **szczegóły** wyświetlić **StoreController**w **szczegóły** metody akcji.</span><span class="sxs-lookup"><span data-stu-id="3507f-586">Add a new **Details** view for the **StoreController**'s **Details** action method.</span></span> <span data-ttu-id="3507f-587">Aby to zrobić, kliknij prawym przyciskiem myszy **szczegóły** metody w **StoreController** klasy, a następnie kliknij przycisk **Dodaj widok**.</span><span class="sxs-lookup"><span data-stu-id="3507f-587">To do this, right-click the **Details** method in the **StoreController** class and click **Add View**.</span></span>
2. <span data-ttu-id="3507f-588">W **Dodaj widok** okna dialogowego, upewnij się, że **nazwy widoku** jest **szczegóły**.</span><span class="sxs-lookup"><span data-stu-id="3507f-588">In the **Add View** dialog, verify that the **View Name** is **Details**.</span></span> <span data-ttu-id="3507f-589">Sprawdź **utworzyć widok jednoznacznie** wyboru i wybierz **albumu** z **klasa modelu** listy rozwijanej.</span><span class="sxs-lookup"><span data-stu-id="3507f-589">Check the **Create a strongly-typed view** checkbox and select **Album** from the **Model class** drop-down.</span></span> <span data-ttu-id="3507f-590">Pozostaw wartości domyślne innych pól.</span><span class="sxs-lookup"><span data-stu-id="3507f-590">Leave the other fields with their default value.</span></span> <span data-ttu-id="3507f-591">Następnie kliknij przycisk **Dodaj**.</span><span class="sxs-lookup"><span data-stu-id="3507f-591">Then click **Add**.</span></span> <span data-ttu-id="3507f-592">Spowoduje to utworzenie i otworzyć **\Views\Store\Details.cshtml** pliku.</span><span class="sxs-lookup"><span data-stu-id="3507f-592">This will create and open a **\Views\Store\Details.cshtml** file.</span></span>

    <span data-ttu-id="3507f-593">![Dodawanie widoku szczegółów](aspnet-mvc-4-fundamentals/_static/image31.png "Dodawanie widoku szczegółów")</span><span class="sxs-lookup"><span data-stu-id="3507f-593">![Adding a Details View](aspnet-mvc-4-fundamentals/_static/image31.png "Adding a Details View")</span></span>

    <span data-ttu-id="3507f-594">*Dodawanie widoku szczegółów*</span><span class="sxs-lookup"><span data-stu-id="3507f-594">*Adding a Details View*</span></span>
3. <span data-ttu-id="3507f-595">Modyfikowanie **Details.cshtml** plik, aby wyświetlić informacje albumu podczas uzyskiwania dostępu do **albumu** obiekt, który jest przekazywany do szablonu widoku.</span><span class="sxs-lookup"><span data-stu-id="3507f-595">Modify the **Details.cshtml** file to display the Album's information, accessing the **Album** object that is passed to the view template.</span></span> <span data-ttu-id="3507f-596">Aby to zrobić, Zastąp zawartość następującym kodem: (C#)</span><span class="sxs-lookup"><span data-stu-id="3507f-596">To do this, replace the content with the following: (C#)</span></span>

    [!code-cshtml[Main](aspnet-mvc-4-fundamentals/samples/sample24.cshtml)]

<a id="Ex6Task7"></a>

<a id="Task_7_-_Running_the_Application"></a>
#### <a name="task-7---running-the-application"></a><span data-ttu-id="3507f-597">Zadanie 7 - uruchamiania aplikacji</span><span class="sxs-lookup"><span data-stu-id="3507f-597">Task 7 - Running the Application</span></span>

<span data-ttu-id="3507f-598">W tym zadaniu zostanie sprawdzić, czy **szczegóły** widok pobiera informacje albumu z **szczegóły akcji** metody.</span><span class="sxs-lookup"><span data-stu-id="3507f-598">In this task, you will test that the **Details** View retrieves Album's information from the **Details action** method.</span></span>

1. <span data-ttu-id="3507f-599">Naciśnij klawisz **F5** do uruchomienia aplikacji.</span><span class="sxs-lookup"><span data-stu-id="3507f-599">Press **F5** to run the Application.</span></span>
2. <span data-ttu-id="3507f-600">Projekt jest uruchamiany w **Home** strony.</span><span class="sxs-lookup"><span data-stu-id="3507f-600">The project starts in the **Home** page.</span></span> <span data-ttu-id="3507f-601">Zmień adres URL do **/Store/Details/5** do sprawdzenia informacji o albumu.</span><span class="sxs-lookup"><span data-stu-id="3507f-601">Change the URL to **/Store/Details/5** to verify the album's information.</span></span>

    <span data-ttu-id="3507f-602">![Przeglądanie szczegółów albumów](aspnet-mvc-4-fundamentals/_static/image32.png "przeglądania szczegółów albumów")</span><span class="sxs-lookup"><span data-stu-id="3507f-602">![Browsing Albums Detail](aspnet-mvc-4-fundamentals/_static/image32.png "Browsing Albums Detail")</span></span>

    <span data-ttu-id="3507f-603">*Album przeglądania szczegółów*</span><span class="sxs-lookup"><span data-stu-id="3507f-603">*Browsing Album's Detail*</span></span>

<a id="Ex6Task8"></a>

<a id="Task_8_-_Adding_Links_Between_Pages"></a>
#### <a name="task-8---adding-links-between-pages"></a><span data-ttu-id="3507f-604">Zadanie 8 — Dodawanie łącza między stronami</span><span class="sxs-lookup"><span data-stu-id="3507f-604">Task 8 - Adding Links Between Pages</span></span>

<span data-ttu-id="3507f-605">To zadanie będzie dodanie łącza w widoku magazynu w celu zapewnienia łącze każdego rodzaju nazwy do odpowiedniego **/magazynu/Przeglądaj** adresu URL.</span><span class="sxs-lookup"><span data-stu-id="3507f-605">In this task, you will add a link in the Store View to have a link in every Genre name to the appropriate **/Store/Browse** URL.</span></span> <span data-ttu-id="3507f-606">Dzięki temu po kliknięciu określonego rodzaju, na przykład **Disco**, spowoduje przejście do **/magazynu/Przeglądaj? genre = Disco** adresu URL.</span><span class="sxs-lookup"><span data-stu-id="3507f-606">This way, when you click on a Genre, for instance **Disco**, it will navigate to **/Store/Browse?genre=Disco** URL.</span></span>

1. <span data-ttu-id="3507f-607">Zamknij przeglądarkę, jeśli to konieczne, aby powrócić do okna programu Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="3507f-607">Close the browser if needed, to return to the Visual Studio window.</span></span> <span data-ttu-id="3507f-608">Aktualizacja **indeksu** stronę, aby dodać łącze do **Przeglądaj** strony.</span><span class="sxs-lookup"><span data-stu-id="3507f-608">Update the **Index** page to add a link to the **Browse** page.</span></span> <span data-ttu-id="3507f-609">Aby to zrobić, w **Eksploratora rozwiązań** rozwiń węzeł **widoków** folderu, a następnie **magazynu** folder i kliknij dwukrotnie **Index.cshtml** strony.</span><span class="sxs-lookup"><span data-stu-id="3507f-609">To do this, in the **Solution Explorer** expand the **Views** folder, then the **Store** folder and double-click the **Index.cshtml** page.</span></span>
2. <span data-ttu-id="3507f-610">Dodaj łącze do widoku przeglądania wskazujący genre wybrane.</span><span class="sxs-lookup"><span data-stu-id="3507f-610">Add a link to the Browse view indicating the genre selected.</span></span> <span data-ttu-id="3507f-611">Aby to zrobić, Zamień następujące wyróżniony kod w  **&lt;li&gt;**  tagi: (C#)</span><span class="sxs-lookup"><span data-stu-id="3507f-611">To do this, replace the following highlighted code within the **&lt;li&gt;** tags: (C#)</span></span>

    [!code-cshtml[Main](aspnet-mvc-4-fundamentals/samples/sample25.cshtml)]

    > [!NOTE]
    > <span data-ttu-id="3507f-612">innym rozwiązaniem będzie łączenie bezpośrednio do strony, przy użyciu kodu podobne do poniższych:</span><span class="sxs-lookup"><span data-stu-id="3507f-612">another approach would be linking directly to the page, with a code like the following:</span></span>
    > 
    > <span data-ttu-id="3507f-613">&lt;href =&quot;/magazynu/Przeglądaj? genre =@genreName&quot;&gt;@genreName&lt;/a&gt;</span><span class="sxs-lookup"><span data-stu-id="3507f-613">&lt;a href=&quot;/Store/Browse?genre=@genreName&quot;&gt;@genreName&lt;/a&gt;</span></span>
    > 
    > <span data-ttu-id="3507f-614">Chociaż to rozwiązanie działa, jest zależna od ciągu zapisane na stałe.</span><span class="sxs-lookup"><span data-stu-id="3507f-614">Although this approach works, it depends on a hardcoded string.</span></span> <span data-ttu-id="3507f-615">Później w przypadku zmiany nazwy kontrolera, należy ręcznie zmienić tej instrukcji.</span><span class="sxs-lookup"><span data-stu-id="3507f-615">If you later rename the Controller, you will have to change this instruction manually.</span></span> <span data-ttu-id="3507f-616">Lepszym jest użycie **pomocnika kodu HTML** metody.</span><span class="sxs-lookup"><span data-stu-id="3507f-616">A better alternative is to use an **HTML Helper** method.</span></span> <span data-ttu-id="3507f-617">Platforma ASP.NET MVC zawiera metodę pomocnika kodu HTML, która jest dostępna dla takich zadań.</span><span class="sxs-lookup"><span data-stu-id="3507f-617">ASP.NET MVC includes an HTML Helper method which is available for such tasks.</span></span> <span data-ttu-id="3507f-618">**Html.ActionLink()** metody pomocnika ułatwia tworzenie HTML  **&lt;&gt;**  łącza, upewniając się, ścieżki adresu URL są poprawnie zakodowane w adresie URL.</span><span class="sxs-lookup"><span data-stu-id="3507f-618">The **Html.ActionLink()** helper method makes it easy to build HTML **&lt;a&gt;** links, making sure URL paths are properly URL encoded.</span></span>
    > 
    > <span data-ttu-id="3507f-619">Htlm.ActionLink ma kilka przeciążeń.</span><span class="sxs-lookup"><span data-stu-id="3507f-619">Htlm.ActionLink has several overloads.</span></span> <span data-ttu-id="3507f-620">W tym ćwiczeniu będzie używany, który przyjmuje trzy parametry:</span><span class="sxs-lookup"><span data-stu-id="3507f-620">In this exercise you will use one that takes three parameters:</span></span>
    > 
    > 1. <span data-ttu-id="3507f-621">Tekst łącza, która będzie wyświetlana nazwa Genre</span><span class="sxs-lookup"><span data-stu-id="3507f-621">Link text, which will display the Genre name</span></span>
    > 2. <span data-ttu-id="3507f-622">Nazwa akcji kontrolera (**Przeglądaj**)</span><span class="sxs-lookup"><span data-stu-id="3507f-622">Controller action name (**Browse**)</span></span>
    > 3. <span data-ttu-id="3507f-623">Wartości parametru, określając nazwę trasy (**Genre**) i wartość (**nazwa Genre**)</span><span class="sxs-lookup"><span data-stu-id="3507f-623">Route parameter values, specifying both the name (**Genre**) and the value (**Genre name**)</span></span>

<a id="Ex6Task9"></a>

<a id="Task_9_-_Running_the_Application"></a>
#### <a name="task-9---running-the-application"></a><span data-ttu-id="3507f-624">Zadanie 9 — uruchamianie aplikacji</span><span class="sxs-lookup"><span data-stu-id="3507f-624">Task 9 - Running the Application</span></span>

<span data-ttu-id="3507f-625">W tym zadaniu zostanie przetestować, czy każdego rodzaju są wyświetlane z łączem do odpowiedniej **/magazynu/Przeglądaj** adresu URL.</span><span class="sxs-lookup"><span data-stu-id="3507f-625">In this task, you will test that each Genre is displayed with a link to the appropriate **/Store/Browse** URL.</span></span>

1. <span data-ttu-id="3507f-626">Naciśnij klawisz **F5** do uruchomienia aplikacji.</span><span class="sxs-lookup"><span data-stu-id="3507f-626">Press **F5** to run the Application.</span></span>
2. <span data-ttu-id="3507f-627">Na stronie głównej datę rozpoczęcia projektu.</span><span class="sxs-lookup"><span data-stu-id="3507f-627">The project starts in the Home page.</span></span> <span data-ttu-id="3507f-628">Zmień adres URL do **/magazynu** można zweryfikować, że każdego rodzaju linki do odpowiednich **/magazynu/Przeglądaj** adresu URL.</span><span class="sxs-lookup"><span data-stu-id="3507f-628">Change the URL to **/Store** to verify that each Genre links to the appropriate **/Store/Browse** URL.</span></span>

    <span data-ttu-id="3507f-629">![Przeglądanie Genres wraz z łączami do przeglądania strony](aspnet-mvc-4-fundamentals/_static/image33.png "przeglądanie Genres wraz z łączami do przeglądania strony")</span><span class="sxs-lookup"><span data-stu-id="3507f-629">![Browsing Genres with links to Browse page](aspnet-mvc-4-fundamentals/_static/image33.png "Browsing Genres with links to Browse page")</span></span>

    <span data-ttu-id="3507f-630">*Przeglądanie Genres wraz z łączami do przeglądania strony*</span><span class="sxs-lookup"><span data-stu-id="3507f-630">*Browsing Genres with links to Browse page*</span></span>

<a id="Ex6Task10"></a>

<a id="Task_10_-_Using_Dynamic_ViewModel_Collection_to_Pass_Values"></a>
#### <a name="task-10---using-dynamic-viewmodel-collection-to-pass-values"></a><span data-ttu-id="3507f-631">Zadanie 10 - przy użyciu kolekcji dynamicznych ViewModel do przekazania wartości</span><span class="sxs-lookup"><span data-stu-id="3507f-631">Task 10 - Using Dynamic ViewModel Collection to Pass Values</span></span>

<span data-ttu-id="3507f-632">W tym zadaniu dowiesz się, prosta i skuteczna metoda przekazać wartości między kontrolerem a widokiem bez wprowadzania żadnych zmian w modelu.</span><span class="sxs-lookup"><span data-stu-id="3507f-632">In this task, you will learn a simple and powerful method to pass values between the Controller and the View without making any changes in the Model.</span></span> <span data-ttu-id="3507f-633">ASP.NET MVC 4 udostępnia kolekcję &quot;ViewModel&quot;, co przypisane do żadnej wartości dynamiczne i dostęp do wewnątrz również widoków i kontrolerów.</span><span class="sxs-lookup"><span data-stu-id="3507f-633">ASP.NET MVC 4 provides the collection &quot;ViewModel&quot;, which can be assigned to any dynamic value and accessed inside controllers and views as well.</span></span>

<span data-ttu-id="3507f-634">Teraz użyje kolekcji dynamicznych elementów ViewBag do przekazania listę &quot; **Starred genres** &quot; z kontrolera do widoku.</span><span class="sxs-lookup"><span data-stu-id="3507f-634">You will now use the ViewBag dynamic collection to pass a list of &quot;**Starred genres**&quot; from the controller to the view.</span></span> <span data-ttu-id="3507f-635">Widok indeksu magazynu będą uzyskiwać dostęp do **ViewModel** i wyświetlić informacje.</span><span class="sxs-lookup"><span data-stu-id="3507f-635">The Store Index view will access to **ViewModel** and display the information.</span></span>

1. <span data-ttu-id="3507f-636">Zamknij przeglądarkę, jeśli to konieczne, aby powrócić do okna programu Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="3507f-636">Close the browser if needed, to return to the Visual Studio window.</span></span> <span data-ttu-id="3507f-637">Otwórz **StoreController.cs** i zmodyfikuj **indeksu** metodę w celu utworzenia listy starred genres do kolekcji ViewModel:</span><span class="sxs-lookup"><span data-stu-id="3507f-637">Open **StoreController.cs** and modify **Index** method to create a list of starred genres into ViewModel collection :</span></span>


    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample26.cs)]

    > [!NOTE]
    > <span data-ttu-id="3507f-638">Można również użyć składni **obiekt ViewBag [&quot;Starred&quot;]** uzyskać dostęp do właściwości.</span><span class="sxs-lookup"><span data-stu-id="3507f-638">You could also use the syntax **ViewBag[&quot;Starred&quot;]** to access the properties.</span></span>
2. <span data-ttu-id="3507f-639">Ikonę gwiazdki  **&quot;starred.png&quot;**  znajduje się w **Source\Assets\Images** folder tego laboratorium.</span><span class="sxs-lookup"><span data-stu-id="3507f-639">The star icon **&quot;starred.png&quot;** is included in the **Source\Assets\Images** folder of this lab.</span></span> <span data-ttu-id="3507f-640">Aby dodać go do aplikacji, przeciągnij ich zawartość z **Eksploratora Windows** okna do **Eksploratora rozwiązań** w Visual Web Developer Express, jak pokazano poniżej:</span><span class="sxs-lookup"><span data-stu-id="3507f-640">In order to add it to the application, drag their content from a **Windows Explorer** window into the **Solution Explorer** in Visual Web Developer Express, as shown below:</span></span>

    <span data-ttu-id="3507f-641">![Dodawanie obrazu gwiazdy do rozwiązania](aspnet-mvc-4-fundamentals/_static/image34.png "Dodawanie gwiazdy obrazu do rozwiązania")</span><span class="sxs-lookup"><span data-stu-id="3507f-641">![Adding star image to the solution](aspnet-mvc-4-fundamentals/_static/image34.png "Adding star image to the solution")</span></span>

    <span data-ttu-id="3507f-642">*Dodawanie obrazu gwiazdy do rozwiązania*</span><span class="sxs-lookup"><span data-stu-id="3507f-642">*Adding star image to the solution*</span></span>
3. <span data-ttu-id="3507f-643">Otwórz widok **Store/Index.cshtml** i modyfikować zawartość.</span><span class="sxs-lookup"><span data-stu-id="3507f-643">Open the view **Store/Index.cshtml** and modify the content.</span></span> <span data-ttu-id="3507f-644">Będzie odczytywał &quot;starred&quot; właściwości w **obiekt ViewBag** kolekcji i poproś, jeśli bieżąca nazwa genre znajduje się na liście.</span><span class="sxs-lookup"><span data-stu-id="3507f-644">You will read the &quot;starred&quot; property in the **ViewBag** collection, and ask if the current genre name is in the list.</span></span> <span data-ttu-id="3507f-645">W takim przypadku zostaną wyświetlone ikonę gwiazdki prawo do łącza rodzaju.</span><span class="sxs-lookup"><span data-stu-id="3507f-645">In that case you will show a star icon right to the genre link.</span></span>
<span data-ttu-id="3507f-646">(C#)</span><span class="sxs-lookup"><span data-stu-id="3507f-646">(C#)</span></span>

    [!code-cshtml[Main](aspnet-mvc-4-fundamentals/samples/sample27.cshtml)]

<a id="Ex6Task11"></a>

<a id="Task_11_-_Running_the_Application"></a>
#### <a name="task-11---running-the-application"></a><span data-ttu-id="3507f-647">Zadanie 11 - uruchamiania aplikacji</span><span class="sxs-lookup"><span data-stu-id="3507f-647">Task 11 - Running the Application</span></span>

<span data-ttu-id="3507f-648">W tym zadaniu zostanie przetestować, że jak pokazano genres wyświetlać ikonę gwiazdki.</span><span class="sxs-lookup"><span data-stu-id="3507f-648">In this task, you will test that the starred genres display a star icon.</span></span>

1. <span data-ttu-id="3507f-649">Naciśnij klawisz **F5** do uruchomienia aplikacji.</span><span class="sxs-lookup"><span data-stu-id="3507f-649">Press **F5** to run the Application.</span></span>
2. <span data-ttu-id="3507f-650">Projekt jest uruchamiany w **Home** strony.</span><span class="sxs-lookup"><span data-stu-id="3507f-650">The project starts in the **Home** page.</span></span> <span data-ttu-id="3507f-651">Zmień adres URL do **/magazynu** do sprawdzenia, czy każdego rodzaju polecanych ma wagę do poszanowania etykiety:</span><span class="sxs-lookup"><span data-stu-id="3507f-651">Change the URL to **/Store** to verify that each featured genre has the respecting label:</span></span>

    <span data-ttu-id="3507f-652">![Przeglądanie Genres elementów jak pokazano na](aspnet-mvc-4-fundamentals/_static/image35.png "przeglądanie Genres elementów jak pokazano na")</span><span class="sxs-lookup"><span data-stu-id="3507f-652">![Browsing Genres with starred elements](aspnet-mvc-4-fundamentals/_static/image35.png "Browsing Genres with starred elements")</span></span>

    <span data-ttu-id="3507f-653">*Przeglądanie Genres elementów jak pokazano na*</span><span class="sxs-lookup"><span data-stu-id="3507f-653">*Browsing Genres with starred elements*</span></span>

<a id="Exercise7"></a>

<a id="Exercise_7_A_lap_around_ASPNET_MVC_4_new_template"></a>
### <a name="exercise-7-a-lap-around-aspnet-mvc-4-new-template"></a><span data-ttu-id="3507f-654">Ćwiczenie 7: Laptop wokół nowego szablonu platformy ASP.NET MVC 4</span><span class="sxs-lookup"><span data-stu-id="3507f-654">Exercise 7: A lap around ASP.NET MVC 4 new template</span></span>

<span data-ttu-id="3507f-655">W tym ćwiczeniu zostanie Poznaj ulepszenia w szablonach projektu programu ASP.NET MVC 4, biorąc spojrzeć na najbardziej istotne cechy nowy szablon.</span><span class="sxs-lookup"><span data-stu-id="3507f-655">In this exercise, you will explore the enhancements in the ASP.NET MVC 4 project templates, taking a look at the most relevant features of the new template.</span></span>

<a id="Ex7Task1"></a>

<a id="Task_1_Exploring_the_ASPNET_MVC_4_Internet_Application_Template"></a>
#### <a name="task-1-exploring-the-aspnet-mvc-4-internet-application-template"></a><span data-ttu-id="3507f-656">Zadanie 1: Eksploracji szablon aplikacji internetowych platformy ASP.NET MVC 4</span><span class="sxs-lookup"><span data-stu-id="3507f-656">Task 1: Exploring the ASP.NET MVC 4 Internet Application Template</span></span>

1. <span data-ttu-id="3507f-657">Jeśli nie już otwarty, uruchom **VS Express for Web**</span><span class="sxs-lookup"><span data-stu-id="3507f-657">If not already open, start **VS Express for Web**</span></span>
2. <span data-ttu-id="3507f-658">Wybierz **pliku | Nowy | Projekt** polecenia menu.</span><span class="sxs-lookup"><span data-stu-id="3507f-658">Select the **File | New | Project** menu command.</span></span> <span data-ttu-id="3507f-659">W **nowy projekt** okno dialogowe, wybierz opcję **Visual C# | Web** szablonu w lewym okienku drzewa, a następnie wybierz pozycję **aplikacji sieci Web programu ASP.NET MVC 4**.</span><span class="sxs-lookup"><span data-stu-id="3507f-659">In the **New Project** dialog, select the **Visual C#|Web** template on the left pane tree, and choose the **ASP.NET MVC 4 Web Application**.</span></span> <span data-ttu-id="3507f-660">**Nazwa** projektu *MusicStore* i zaktualizuj **Nazwa rozwiązania** do *rozpocząć*, następnie wybierz lokalizację (lub pozostaw wartość domyślną) i kliknij przycisk **OK**.</span><span class="sxs-lookup"><span data-stu-id="3507f-660">**Name** the project *MusicStore* and update the **solution name** to *Begin*, then select a location (or leave the default) and click **OK**.</span></span>

    <span data-ttu-id="3507f-661">![Tworzenie nowego projektu platformy ASP.NET MVC 4](aspnet-mvc-4-fundamentals/_static/image36.png "tworzenia nowego projektu platformy ASP.NET MVC 4")</span><span class="sxs-lookup"><span data-stu-id="3507f-661">![Creating a new ASP.NET MVC 4 Project](aspnet-mvc-4-fundamentals/_static/image36.png "Creating a new ASP.NET MVC 4 Project")</span></span>

    <span data-ttu-id="3507f-662">*Tworzenie nowego projektu platformy ASP.NET MVC 4*</span><span class="sxs-lookup"><span data-stu-id="3507f-662">*Creating a new ASP.NET MVC 4 Project*</span></span>
3. <span data-ttu-id="3507f-663">W **nowy projekt programu ASP.NET MVC 4** okno dialogowe, wybierz opcję **aplikacji internetowej** szablonu projektu i kliknij przycisk **OK**.</span><span class="sxs-lookup"><span data-stu-id="3507f-663">In the **New ASP.NET MVC 4 Project** dialog, select the **Internet Application** project template and click **OK**.</span></span> <span data-ttu-id="3507f-664">Należy zauważyć, że można wybrać jako aparat widoku Razor lub ASPX.</span><span class="sxs-lookup"><span data-stu-id="3507f-664">Notice you can select either Razor or ASPX as the view engine.</span></span>

    <span data-ttu-id="3507f-665">![Tworzenie nowej aplikacji internetowych 4 ASP.NET MVC](aspnet-mvc-4-fundamentals/_static/image37.png "Tworzenie nowej aplikacji internetowych 4 ASP.NET MVC")</span><span class="sxs-lookup"><span data-stu-id="3507f-665">![Creating a new ASP.NET MVC 4 Internet Application](aspnet-mvc-4-fundamentals/_static/image37.png "Creating a new ASP.NET MVC 4 Internet Application")</span></span>

    <span data-ttu-id="3507f-666">*Tworzenie nowej aplikacji internetowych 4 ASP.NET MVC*</span><span class="sxs-lookup"><span data-stu-id="3507f-666">*Creating a new ASP.NET MVC 4 Internet Application*</span></span>

    > [!NOTE]
    > <span data-ttu-id="3507f-667">Składnia razor została wprowadzona w programie ASP.NET MVC 3.</span><span class="sxs-lookup"><span data-stu-id="3507f-667">Razor syntax has been introduced in ASP.NET MVC 3.</span></span> <span data-ttu-id="3507f-668">Jego celem jest zminimalizowanie liczby znaków i naciśnięcia klawiszy wymagane w pliku, umożliwiające szybkie oraz płynu kodowania przepływu pracy.</span><span class="sxs-lookup"><span data-stu-id="3507f-668">Its goal is to minimize the number of characters and keystrokes required in a file, enabling a fast and fluid coding workflow.</span></span> <span data-ttu-id="3507f-669">Razor wykorzystuje istniejące C# /VB (lub innych) umiejętności język, a następnie dostarcza składnia znacznika szablonu, umożliwiającą świetny przepływu pracy konstrukcji kodu HTML.</span><span class="sxs-lookup"><span data-stu-id="3507f-669">Razor leverages existing C#/VB (or other) language skills and delivers a template markup syntax that enables an awesome HTML construction workflow.</span></span>
4. <span data-ttu-id="3507f-670">Naciśnij klawisz **F5** uruchomić rozwiązanie i zobaczyć odnowionego szablonu.</span><span class="sxs-lookup"><span data-stu-id="3507f-670">Press **F5** to run the solution and see the renewed template.</span></span> <span data-ttu-id="3507f-671">Można wyewidencjonować następujące funkcje:</span><span class="sxs-lookup"><span data-stu-id="3507f-671">You can check out the following features:</span></span>

    1. <span data-ttu-id="3507f-672">**Szablonów w stylu Modern**</span><span class="sxs-lookup"><span data-stu-id="3507f-672">**Modern-style templates**</span></span>

        <span data-ttu-id="3507f-673">Szablony odnowienia, zapewniając więcej stylów nowoczesny wygląd.</span><span class="sxs-lookup"><span data-stu-id="3507f-673">The templates have been renewed, providing more modern-looking styles.</span></span>

        <span data-ttu-id="3507f-674">![Szablony ASP.NET MVC 4 restyled](aspnet-mvc-4-fundamentals/_static/image38.png "restyled szablony programu ASP.NET MVC 4")</span><span class="sxs-lookup"><span data-stu-id="3507f-674">![ASP.NET MVC 4 restyled templates](aspnet-mvc-4-fundamentals/_static/image38.png "ASP.NET MVC 4 restyled templates")</span></span>

        <span data-ttu-id="3507f-675">*Szablony ASP.NET MVC 4 restyled*</span><span class="sxs-lookup"><span data-stu-id="3507f-675">*ASP.NET MVC 4 restyled templates*</span></span>
    2. <span data-ttu-id="3507f-676">**Renderowanie adaptacyjną**</span><span class="sxs-lookup"><span data-stu-id="3507f-676">**Adaptive Rendering**</span></span>

        <span data-ttu-id="3507f-677">Zapoznaj się z zmiany rozmiaru okna przeglądarki i zwróć uwagę, jak dynamicznie dostosowuje układ strony do nowego rozmiaru okna.</span><span class="sxs-lookup"><span data-stu-id="3507f-677">Check out resizing the browser window and notice how the page layout dynamically adapts to the new window size.</span></span> <span data-ttu-id="3507f-678">Te szablony używać techniki adaptacyjną renderowania mają być renderowane poprawnie w zarówno wersje desktop i mobile platform bez potrzeby dostosowywania.</span><span class="sxs-lookup"><span data-stu-id="3507f-678">These templates use the adaptive rendering technique to render properly in both desktop and mobile platforms without any customization.</span></span>

        <span data-ttu-id="3507f-679">![Szablon projektu programu ASP.NET MVC 4 w innej przeglądarce rozmiary](aspnet-mvc-4-fundamentals/_static/image39.png "szablonu projektu programu ASP.NET MVC 4 w innej przeglądarce rozmiarach")</span><span class="sxs-lookup"><span data-stu-id="3507f-679">![ASP.NET MVC 4 project template in different browser sizes](aspnet-mvc-4-fundamentals/_static/image39.png "ASP.NET MVC 4 project template in different browser sizes")</span></span>

        <span data-ttu-id="3507f-680">*Szablon projektu programu ASP.NET MVC 4 w innej przeglądarce rozmiarach*</span><span class="sxs-lookup"><span data-stu-id="3507f-680">*ASP.NET MVC 4 project template in different browser sizes*</span></span>
5. <span data-ttu-id="3507f-681">Zamknij przeglądarkę, aby zatrzymać debuger i powrócić do programu Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="3507f-681">Close the browser to stop the debugger and return to Visual Studio.</span></span>
6. <span data-ttu-id="3507f-682">Teraz masz możliwość Eksploruj rozwiązania i zapoznaj się z nowych funkcji wprowadzonych przez program ASP.NET MVC 4 w szablonie projektu.</span><span class="sxs-lookup"><span data-stu-id="3507f-682">Now you are able to explore the solution and check out some of the new features introduced by ASP.NET MVC 4 in the project template.</span></span>

    <span data-ttu-id="3507f-683">![ASP.NET MVC4-internet-— szablon projektu aplikacji-](aspnet-mvc-4-fundamentals/_static/image40.png "szablonu projektu aplikacji internetowych platformy ASP.NET MVC 4")</span><span class="sxs-lookup"><span data-stu-id="3507f-683">![ASP.NET MVC4-internet-application-project-template](aspnet-mvc-4-fundamentals/_static/image40.png "The ASP.NET MVC 4 Internet Application Project Template")</span></span>

    <span data-ttu-id="3507f-684">*Szablon projektu aplikacji internetowych platformy ASP.NET MVC 4*</span><span class="sxs-lookup"><span data-stu-id="3507f-684">*The ASP.NET MVC 4 Internet Application Project Template*</span></span>

    1. <span data-ttu-id="3507f-685">**HTML5 znaczników**</span><span class="sxs-lookup"><span data-stu-id="3507f-685">**HTML5 markup**</span></span>

        <span data-ttu-id="3507f-686">Przeglądaj widoków szablonu, aby dowiedzieć się nowego znacznika motywu, na przykład otwórz **About.cshtml** wyświetlić w **Home** folderu.</span><span class="sxs-lookup"><span data-stu-id="3507f-686">Browse template views to find out the new theme markup, for example open **About.cshtml** view within **Home** folder.</span></span>

        <span data-ttu-id="3507f-687">![Nowy szablon, przy użyciu znaczników Razor i HTML5](aspnet-mvc-4-fundamentals/_static/image41.png "nowego szablonu, za pomocą znacznika Razor i HTML5")</span><span class="sxs-lookup"><span data-stu-id="3507f-687">![New template, using Razor and HTML5 markup](aspnet-mvc-4-fundamentals/_static/image41.png "New template, using Razor and HTML5 markup")</span></span>

        <span data-ttu-id="3507f-688">*Nowy szablon, przy użyciu znaczników Razor i HTML5*</span><span class="sxs-lookup"><span data-stu-id="3507f-688">*New template, using Razor and HTML5 markup*</span></span>
    2. <span data-ttu-id="3507f-689">**JavaScript biblioteki dołączony**</span><span class="sxs-lookup"><span data-stu-id="3507f-689">**JavaScript libraries included**</span></span>

        1. <span data-ttu-id="3507f-690">**jQuery**: jQuery ułatwia przechodzenie dokumentu HTML, obsługa zdarzeń animacji i interakcje Ajax.</span><span class="sxs-lookup"><span data-stu-id="3507f-690">**jQuery**: jQuery simplifies HTML document traversing, event handling, animating, and Ajax interactions.</span></span>
        2. <span data-ttu-id="3507f-691">**interfejsu użytkownika jQuery**: Ta biblioteka zawiera obiektów abstrakcyjnych odpowiadających niskiego poziomu interakcji i zaawansowana efekty animacji i elementów WebParts elementy widget, rozszerzający jQuery biblioteka języka JavaScript.</span><span class="sxs-lookup"><span data-stu-id="3507f-691">**jQuery UI**: This library provides abstractions for low-level interaction and animation, advanced effects and themeable widgets, built on top of the jQuery JavaScript Library.</span></span>

            > [!NOTE]
            > <span data-ttu-id="3507f-692">Informacje na temat jQuery i jQuery interfejsu użytkownika w [ [http://docs.jquery.com/](http://docs.jquery.com/)](http://docs.jquery.com/).</span><span class="sxs-lookup"><span data-stu-id="3507f-692">You can learn about jQuery and jQuery UI in [[http://docs.jquery.com/](http://docs.jquery.com/)](http://docs.jquery.com/).</span></span>
        3. <span data-ttu-id="3507f-693">**Elementami KnockoutJS**: szablon domyślny ASP.NET MVC 4 zawiera teraz **elementami KnockoutJS**, platforma JavaScript MVVM, która umożliwia tworzenie aplikacji sieci web bogaty i elastyczny wysokiej przy użyciu języka JavaScript i HTML.</span><span class="sxs-lookup"><span data-stu-id="3507f-693">**KnockoutJS**: The ASP.NET MVC 4 default template now includes **KnockoutJS**, a JavaScript MVVM framework that lets you create rich and highly responsive web applications using JavaScript and HTML.</span></span> <span data-ttu-id="3507f-694">Podobnie jak w programie ASP.NET MVC 3, jQuery i jQuery biblioteki interfejsu użytkownika znajdują się również w technologii ASP.NET MVC 4.</span><span class="sxs-lookup"><span data-stu-id="3507f-694">Like in ASP.NET MVC 3, jQuery and jQuery UI libraries are also included in ASP.NET MVC 4.</span></span>

            > [!NOTE]
            > <span data-ttu-id="3507f-695">Możesz uzyskać więcej informacji na temat biblioteki elementami KnockOutJS w to łącze: [http://learn.knockoutjs.com/](http://learn.knockoutjs.com/).</span><span class="sxs-lookup"><span data-stu-id="3507f-695">You can get more information about KnockOutJS library in this link: [http://learn.knockoutjs.com/](http://learn.knockoutjs.com/).</span></span>
        4. <span data-ttu-id="3507f-696">**Modernizr**: Ta biblioteka jest uruchamiana automatycznie, co zgodne ze starszych przeglądarek witryny przy użyciu technologii HTML5 i CSS3.</span><span class="sxs-lookup"><span data-stu-id="3507f-696">**Modernizr**: This library runs automatically, making your site compatible with older browsers when using HTML5 and CSS3 technologies.</span></span>

            > [!NOTE]
            > <span data-ttu-id="3507f-697">Możesz uzyskać więcej informacji na temat biblioteki Modernizr w to łącze: [http://www.modernizr.com/](http://www.modernizr.com/).</span><span class="sxs-lookup"><span data-stu-id="3507f-697">You can get more information about Modernizr library in this link: [http://www.modernizr.com/](http://www.modernizr.com/).</span></span>
    3. <span data-ttu-id="3507f-698">**SimpleMembership zawarty w rozwiązaniu**</span><span class="sxs-lookup"><span data-stu-id="3507f-698">**SimpleMembership included in the solution**</span></span>

        <span data-ttu-id="3507f-699">SimpleMembership został zaprojektowany jako serwer zamienny dla poprzedniego systemu dostawcy ról ASP.NET i członkostwa.</span><span class="sxs-lookup"><span data-stu-id="3507f-699">SimpleMembership has been designed as a replacement for the previous ASP.NET Role and Membership provider system.</span></span> <span data-ttu-id="3507f-700">Ma wiele nowych funkcji, które ułatwiają deweloperom bezpiecznych stron sieci web w sposób bardziej elastyczne.</span><span class="sxs-lookup"><span data-stu-id="3507f-700">It has many new features that make it easier for the developer to secure web pages in a more flexible way.</span></span>

        <span data-ttu-id="3507f-701">Szablon Internet już ma skonfigurować kilka ustawień integracji SimpleMembership, na przykład elementu AccountController jest przygotowane do korzystania z OAuthWebSecurity (w przypadku rejestracji konta OAuth, logowania, zarządzania, itp.) i bezpieczeństwo sieci Web.</span><span class="sxs-lookup"><span data-stu-id="3507f-701">The Internet template already has set up a few things to integrate SimpleMembership, for example, the AccountController is prepared to use OAuthWebSecurity (for OAuth account registration, login, management, etc.) and Web Security.</span></span>

        <span data-ttu-id="3507f-702">![SimpleMembership zawarty w rozwiązaniu](aspnet-mvc-4-fundamentals/_static/image42.png "SimpleMembership zawarty w rozwiązaniu")</span><span class="sxs-lookup"><span data-stu-id="3507f-702">![SimpleMembership Included in the solution](aspnet-mvc-4-fundamentals/_static/image42.png "SimpleMembership Included in the solution")</span></span>

        <span data-ttu-id="3507f-703">*SimpleMembership zawarty w rozwiązaniu*</span><span class="sxs-lookup"><span data-stu-id="3507f-703">*SimpleMembership Included in the solution*</span></span>

        > [!NOTE]
        > <span data-ttu-id="3507f-704">Aby uzyskać więcej informacji o [OAuthWebSecurity](https://msdn.microsoft.com/library/jj158393(v=vs.111).aspx) w witrynie MSDN.</span><span class="sxs-lookup"><span data-stu-id="3507f-704">Find more information about [OAuthWebSecurity](https://msdn.microsoft.com/library/jj158393(v=vs.111).aspx) in MSDN.</span></span>

> [!NOTE]
> <span data-ttu-id="3507f-705">Ponadto można wdrożyć tę aplikację systemu Windows Azure Web Sites następujących [dodatek B: publikowania aplikacji ASP.NET MVC 4 przy użyciu narzędzia Web Deploy](#AppendixB).</span><span class="sxs-lookup"><span data-stu-id="3507f-705">Additionally, you can deploy this application to Windows Azure Web Sites following [Appendix B: Publishing an ASP.NET MVC 4 Application using Web Deploy](#AppendixB).</span></span>


* * *

<a id="Summary"></a>

<a id="Summary"></a>
## <a name="summary"></a><span data-ttu-id="3507f-706">Podsumowanie</span><span class="sxs-lookup"><span data-stu-id="3507f-706">Summary</span></span>

<span data-ttu-id="3507f-707">Wykonując tego laboratorium Hands-On znasz już podstawy ASP.NET MVC:</span><span class="sxs-lookup"><span data-stu-id="3507f-707">By completing this Hands-On Lab you have learned the fundamentals of ASP.NET MVC:</span></span>

- <span data-ttu-id="3507f-708">Elementy podstawowe aplikacji MVC i sposób ich interakcji</span><span class="sxs-lookup"><span data-stu-id="3507f-708">The core elements of an MVC application and how they interact</span></span>
- <span data-ttu-id="3507f-709">Tworzenie aplikacji platformy ASP.NET MVC</span><span class="sxs-lookup"><span data-stu-id="3507f-709">How to create an ASP.NET MVC Application</span></span>
- <span data-ttu-id="3507f-710">Jak dodać i skonfigurować kontrolerów, aby obsłużyć parametry przekazywane przy użyciu adresu URL i querystring</span><span class="sxs-lookup"><span data-stu-id="3507f-710">How to add and configure Controllers to handle parameters passed through the URL and querystring</span></span>
- <span data-ttu-id="3507f-711">Jak dodać układ strony wzorcowej można skonfigurować szablon dla typowych zawartość HTML, arkusz stylów, aby zwiększyć wyglądu i działania i szablonu widoku, aby wyświetlić zawartość HTML</span><span class="sxs-lookup"><span data-stu-id="3507f-711">How to add a layout master page to setup a template for common HTML content, a StyleSheet to enhance the look and feel and a View template to display HTML content</span></span>
- <span data-ttu-id="3507f-712">Sposób użycia wzorca ViewModel przekazywania właściwości do szablonu widoku do wyświetlenia informacji dynamicznych</span><span class="sxs-lookup"><span data-stu-id="3507f-712">How to use the ViewModel pattern for passing properties to the View template to display dynamic information</span></span>
- <span data-ttu-id="3507f-713">Jak używać parametrów przekazanych do kontrolerów w szablonie widoku</span><span class="sxs-lookup"><span data-stu-id="3507f-713">How to use parameters passed to Controllers in the View template</span></span>
- <span data-ttu-id="3507f-714">Dodawanie łączy do stron w aplikacji ASP.NET MVC</span><span class="sxs-lookup"><span data-stu-id="3507f-714">How to add links to pages inside the ASP.NET MVC application</span></span>
- <span data-ttu-id="3507f-715">Jak dodać i używania właściwości dynamicznych w widoku</span><span class="sxs-lookup"><span data-stu-id="3507f-715">How to add and use dynamic properties in a View</span></span>
- <span data-ttu-id="3507f-716">Ulepszenia w szablonach projektu programu ASP.NET MVC 4</span><span class="sxs-lookup"><span data-stu-id="3507f-716">The enhancements in the ASP.NET MVC 4 project templates</span></span>

<a id="AppendixA"></a>

<a id="Appendix_A_Installing_Visual_Studio_Express_2012_for_Web"></a>
## <a name="appendix-a-installing-visual-studio-express-2012-for-web"></a><span data-ttu-id="3507f-717">Dodatek A: Instalowanie programu Visual Studio Express 2012 for Web</span><span class="sxs-lookup"><span data-stu-id="3507f-717">Appendix A: Installing Visual Studio Express 2012 for Web</span></span>

<span data-ttu-id="3507f-718">Można zainstalować **Microsoft Visual Studio Express 2012 for Web** lub innym &quot;Express&quot; przy użyciu wersji  **[Instalatora platformy sieci Web firmy Microsoft](https://www.microsoft.com/web/downloads/platform.aspx)** .</span><span class="sxs-lookup"><span data-stu-id="3507f-718">You can install **Microsoft Visual Studio Express 2012 for Web** or another &quot;Express&quot; version using the **[Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)**.</span></span> <span data-ttu-id="3507f-719">Poniższe instrukcje przedstawiono czynności wymagane do zainstalowania *programu Visual studio Express 2012 for Web* przy użyciu *Instalatora platformy sieci Web firmy Microsoft*.</span><span class="sxs-lookup"><span data-stu-id="3507f-719">The following instructions guide you through the steps required to install *Visual studio Express 2012 for Web* using *Microsoft Web Platform Installer*.</span></span>

1. <span data-ttu-id="3507f-720">Przejdź do [ [https://go.microsoft.com/?linkid=9810169](https://go.microsoft.com/?linkid=9810169)](https://go.microsoft.com/?linkid=9810169).</span><span class="sxs-lookup"><span data-stu-id="3507f-720">Go to [[https://go.microsoft.com/?linkid=9810169](https://go.microsoft.com/?linkid=9810169)](https://go.microsoft.com/?linkid=9810169).</span></span> <span data-ttu-id="3507f-721">Alternatywnie, jeśli została już zainstalowana Instalatora platformy sieci Web, można otworzyć go i Wyszukaj produkt &quot; *programu Visual Studio Express 2012 for Web z zestawem Windows Azure SDK*&quot;.</span><span class="sxs-lookup"><span data-stu-id="3507f-721">Alternatively, if you already have installed Web Platform Installer, you can open it and search for the product &quot;*Visual Studio Express 2012 for Web with Windows Azure SDK*&quot;.</span></span>
2. <span data-ttu-id="3507f-722">Polecenie **teraz zainstalować**.</span><span class="sxs-lookup"><span data-stu-id="3507f-722">Click on **Install Now**.</span></span> <span data-ttu-id="3507f-723">Jeśli nie masz **Instalatora platformy sieci Web** nastąpi przekierowanie do pobrania i zainstalowania go najpierw.</span><span class="sxs-lookup"><span data-stu-id="3507f-723">If you do not have **Web Platform Installer** you will be redirected to download and install it first.</span></span>
3. <span data-ttu-id="3507f-724">Raz **Instalatora platformy sieci Web** jest otwarty, kliknij przycisk **zainstalować** można uruchomić Instalatora.</span><span class="sxs-lookup"><span data-stu-id="3507f-724">Once **Web Platform Installer** is open, click **Install** to start the setup.</span></span>

    <span data-ttu-id="3507f-725">![Instalowanie programu Visual Studio Express](aspnet-mvc-4-fundamentals/_static/image43.png "instalacji programu Visual Studio Express")</span><span class="sxs-lookup"><span data-stu-id="3507f-725">![Install Visual Studio Express](aspnet-mvc-4-fundamentals/_static/image43.png "Install Visual Studio Express")</span></span>

    <span data-ttu-id="3507f-726">*Instalowanie programu Visual Studio Express*</span><span class="sxs-lookup"><span data-stu-id="3507f-726">*Install Visual Studio Express*</span></span>
4. <span data-ttu-id="3507f-727">Odczytywanie wszystkich produktów licencji i warunków, a następnie kliknij przycisk **akceptuję** aby kontynuować.</span><span class="sxs-lookup"><span data-stu-id="3507f-727">Read all the products' licenses and terms and click **I Accept** to continue.</span></span>

    ![Akceptowanie umowy licencyjnej](aspnet-mvc-4-fundamentals/_static/image44.png)

    <span data-ttu-id="3507f-729">*Akceptowanie umowy licencyjnej*</span><span class="sxs-lookup"><span data-stu-id="3507f-729">*Accepting the license terms*</span></span>
5. <span data-ttu-id="3507f-730">Poczekaj na zakończenie procesu pobierania i instalacji.</span><span class="sxs-lookup"><span data-stu-id="3507f-730">Wait until the downloading and installation process completes.</span></span>

    ![Postęp instalacji](aspnet-mvc-4-fundamentals/_static/image45.png)

    <span data-ttu-id="3507f-732">*Postęp instalacji*</span><span class="sxs-lookup"><span data-stu-id="3507f-732">*Installation progress*</span></span>
6. <span data-ttu-id="3507f-733">Po zakończeniu instalacji kliknij przycisk **Zakończ**.</span><span class="sxs-lookup"><span data-stu-id="3507f-733">When the installation completes, click **Finish**.</span></span>

    ![Instalacja została zakończona](aspnet-mvc-4-fundamentals/_static/image46.png)

    <span data-ttu-id="3507f-735">*Instalacja została zakończona*</span><span class="sxs-lookup"><span data-stu-id="3507f-735">*Installation completed*</span></span>
7. <span data-ttu-id="3507f-736">Kliknij przycisk **zakończenia** aby zamknąć Instalatora platformy sieci Web.</span><span class="sxs-lookup"><span data-stu-id="3507f-736">Click **Exit** to close Web Platform Installer.</span></span>
8. <span data-ttu-id="3507f-737">Aby otworzyć program Visual Studio Express for Web, przejdź do **Start** ekranu i zacznij pisać &quot; **VS Express**&quot;, następnie kliknij polecenie **VS Express for Web** Kafelek.</span><span class="sxs-lookup"><span data-stu-id="3507f-737">To open Visual Studio Express for Web, go to the **Start** screen and start writing &quot;**VS Express**&quot;, then click on the **VS Express for Web** tile.</span></span>

    ![VS Express for Web kafelka](aspnet-mvc-4-fundamentals/_static/image47.png)

    <span data-ttu-id="3507f-739">*VS Express for Web kafelka*</span><span class="sxs-lookup"><span data-stu-id="3507f-739">*VS Express for Web tile*</span></span>

<a id="AppendixB"></a>

<a id="Appendix_B_Publishing_an_ASPNET_MVC_4_Application_using_Web_Deploy"></a>
## <a name="appendix-b-publishing-an-aspnet-mvc-4-application-using-web-deploy"></a><span data-ttu-id="3507f-740">Dodatek B: publikowania aplikacji ASP.NET MVC 4 przy użyciu narzędzia Web Deploy</span><span class="sxs-lookup"><span data-stu-id="3507f-740">Appendix B: Publishing an ASP.NET MVC 4 Application using Web Deploy</span></span>

<span data-ttu-id="3507f-741">Ten dodatek opisano sposób tworzenia nowej witryny sieci web z portalu zarządzania pakietu Windows Azure i publikowanie aplikacji, uzyskane wykonując laboratorium, korzystając z funkcji publikowania narzędzia Web Deploy dostarczane przez Windows Azure.</span><span class="sxs-lookup"><span data-stu-id="3507f-741">This appendix will show you how to create a new web site from the Windows Azure Management Portal and publish the application you obtained by following the lab, taking advantage of the Web Deploy publishing feature provided by Windows Azure.</span></span>

<a id="ApxBTask1"></a>

<a id="Task_1_-_Creating_a_New_Web_Site_from_the_Windows_Azure_Portal"></a>
#### <a name="task-1---creating-a-new-web-site-from-the-windows-azure-portal"></a><span data-ttu-id="3507f-742">Zadanie 1 — Tworzenie nowej witryny sieci Web systemu Windows portalu Azure</span><span class="sxs-lookup"><span data-stu-id="3507f-742">Task 1 - Creating a New Web Site from the Windows Azure Portal</span></span>

1. <span data-ttu-id="3507f-743">Przejdź do [portalu zarządzania pakietu Windows Azure](https://manage.windowsazure.com/) i zaloguj się przy użyciu poświadczeń Microsoft skojarzonych z Twoją subskrypcją.</span><span class="sxs-lookup"><span data-stu-id="3507f-743">Go to the [Windows Azure Management Portal](https://manage.windowsazure.com/) and sign in using the Microsoft credentials associated with your subscription.</span></span>

    > [!NOTE]
    > <span data-ttu-id="3507f-744">Z systemu Windows Azure można udostępniać 10 witryn sieci Web platformy ASP.NET bezpłatnie i następnie Skaluj w miarę zwiększania się ruchu.</span><span class="sxs-lookup"><span data-stu-id="3507f-744">With Windows Azure you can host 10 ASP.NET Web Sites for free and then scale as your traffic grows.</span></span> <span data-ttu-id="3507f-745">Możesz utworzyć konto [tutaj](http://aka.ms/aspnet-hol-azure).</span><span class="sxs-lookup"><span data-stu-id="3507f-745">You can sign up [here](http://aka.ms/aspnet-hol-azure).</span></span>

    <span data-ttu-id="3507f-746">![Zaloguj się do portalu Windows Azure](aspnet-mvc-4-fundamentals/_static/image48.png "Zaloguj się do portalu Windows Azure")</span><span class="sxs-lookup"><span data-stu-id="3507f-746">![Log on to Windows Azure portal](aspnet-mvc-4-fundamentals/_static/image48.png "Log on to Windows Azure portal")</span></span>

    <span data-ttu-id="3507f-747">*Zaloguj się do portalu zarządzania platformy Azure z systemem Windows*</span><span class="sxs-lookup"><span data-stu-id="3507f-747">*Log on to Windows Azure Management Portal*</span></span>
2. <span data-ttu-id="3507f-748">Kliknij przycisk **nowy** na pasku poleceń.</span><span class="sxs-lookup"><span data-stu-id="3507f-748">Click **New** on the command bar.</span></span>

    <span data-ttu-id="3507f-749">![Tworzenie nowej witryny sieci Web](aspnet-mvc-4-fundamentals/_static/image49.png "tworzenia nowej witryny sieci Web")</span><span class="sxs-lookup"><span data-stu-id="3507f-749">![Creating a new Web Site](aspnet-mvc-4-fundamentals/_static/image49.png "Creating a new Web Site")</span></span>

    <span data-ttu-id="3507f-750">*Tworzenie nowej witryny sieci Web*</span><span class="sxs-lookup"><span data-stu-id="3507f-750">*Creating a new Web Site*</span></span>
3. <span data-ttu-id="3507f-751">Kliknij przycisk **obliczeniowe** | **witryny sieci Web**.</span><span class="sxs-lookup"><span data-stu-id="3507f-751">Click **Compute** | **Web Site**.</span></span> <span data-ttu-id="3507f-752">Następnie wybierz **szybkie tworzenie** opcji.</span><span class="sxs-lookup"><span data-stu-id="3507f-752">Then select **Quick Create** option.</span></span> <span data-ttu-id="3507f-753">Podaj dostępny adres URL dla nowej witryny sieci web, a następnie kliknij przycisk **tworzenie witryny sieci Web**.</span><span class="sxs-lookup"><span data-stu-id="3507f-753">Provide an available URL for the new web site and click **Create Web Site**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="3507f-754">Witryny sieci Web systemu Windows Azure jest hostem dla aplikacji sieci web w chmurze, które można kontrolować i zarządzanie nimi.</span><span class="sxs-lookup"><span data-stu-id="3507f-754">A Windows Azure Web Site is the host for a web application running in the cloud that you can control and manage.</span></span> <span data-ttu-id="3507f-755">Opcja szybkie tworzenie umożliwia wdrażanie ukończonej aplikacji sieci web do systemu Windows Azure witryny internetowej z spoza portalu.</span><span class="sxs-lookup"><span data-stu-id="3507f-755">The Quick Create option allows you to deploy a completed web application to the Windows Azure Web Site from outside the portal.</span></span> <span data-ttu-id="3507f-756">Nie obejmuje kroki konfigurowania bazy danych.</span><span class="sxs-lookup"><span data-stu-id="3507f-756">It does not include steps for setting up a database.</span></span>

    <span data-ttu-id="3507f-757">![Tworzenie nowej witryny sieci Web przy użyciu szybkie tworzenie](aspnet-mvc-4-fundamentals/_static/image50.png "tworzenia nowej witryny sieci Web przy użyciu szybkie tworzenie")</span><span class="sxs-lookup"><span data-stu-id="3507f-757">![Creating a new Web Site using Quick Create](aspnet-mvc-4-fundamentals/_static/image50.png "Creating a new Web Site using Quick Create")</span></span>

    <span data-ttu-id="3507f-758">*Tworzenie nowej witryny sieci Web przy użyciu szybkie tworzenie*</span><span class="sxs-lookup"><span data-stu-id="3507f-758">*Creating a new Web Site using Quick Create*</span></span>
4. <span data-ttu-id="3507f-759">Poczekaj na nowe **witryny sieci Web** jest tworzony.</span><span class="sxs-lookup"><span data-stu-id="3507f-759">Wait until the new **Web Site** is created.</span></span>
5. <span data-ttu-id="3507f-760">Po utworzeniu witryny sieci Web kliknij łącze w obszarze **adres URL** kolumny.</span><span class="sxs-lookup"><span data-stu-id="3507f-760">Once the Web Site is created click the link under the **URL** column.</span></span> <span data-ttu-id="3507f-761">Sprawdź, czy działa nowej witryny sieci Web.</span><span class="sxs-lookup"><span data-stu-id="3507f-761">Check that the new Web Site is working.</span></span>

    <span data-ttu-id="3507f-762">![Przeglądanie do nowej witryny sieci web](aspnet-mvc-4-fundamentals/_static/image51.png "przeglądanie do nowej witryny sieci web")</span><span class="sxs-lookup"><span data-stu-id="3507f-762">![Browsing to the new web site](aspnet-mvc-4-fundamentals/_static/image51.png "Browsing to the new web site")</span></span>

    <span data-ttu-id="3507f-763">*Przeglądanie do nowej witryny sieci web*</span><span class="sxs-lookup"><span data-stu-id="3507f-763">*Browsing to the new web site*</span></span>

    <span data-ttu-id="3507f-764">![Witryna sieci Web działa](aspnet-mvc-4-fundamentals/_static/image52.png "uruchamiania witryny sieci Web")</span><span class="sxs-lookup"><span data-stu-id="3507f-764">![Web site running](aspnet-mvc-4-fundamentals/_static/image52.png "Web site running")</span></span>

    <span data-ttu-id="3507f-765">*Witryna sieci Web uruchomiona*</span><span class="sxs-lookup"><span data-stu-id="3507f-765">*Web site running*</span></span>
6. <span data-ttu-id="3507f-766">Wróć do portalu i kliknij nazwę witryny sieci web w obszarze **nazwa** kolumny do wyświetlenia strony zarządzania.</span><span class="sxs-lookup"><span data-stu-id="3507f-766">Go back to the portal and click the name of the web site under the **Name** column to display the management pages.</span></span>

    <span data-ttu-id="3507f-767">![Otwieranie stron witryny sieci web zarządzania](aspnet-mvc-4-fundamentals/_static/image53.png "otwieranie stron zarządzania witryny sieci web")</span><span class="sxs-lookup"><span data-stu-id="3507f-767">![Opening the web site management pages](aspnet-mvc-4-fundamentals/_static/image53.png "Opening the web site management pages")</span></span>

    <span data-ttu-id="3507f-768">*Otwieranie stron zarządzania witryny sieci Web*</span><span class="sxs-lookup"><span data-stu-id="3507f-768">*Opening the Web Site management pages*</span></span>
7. <span data-ttu-id="3507f-769">W **pulpitu nawigacyjnego** w obszarze **szybkiego dostępu** kliknij **pobieranie profilu publikowania** łącza.</span><span class="sxs-lookup"><span data-stu-id="3507f-769">In the **Dashboard** page, under the **quick glance** section, click the **Download publish profile** link.</span></span>

    > [!NOTE]
    > <span data-ttu-id="3507f-770">*Profilu publikowania* zawiera wszystkie informacje wymagane do publikowania aplikacji sieci web do witryny sieci Web systemu Windows Azure dla każdej metody włączone publikacji.</span><span class="sxs-lookup"><span data-stu-id="3507f-770">The *publish profile* contains all of the information required to publish a web application to a Windows Azure website for each enabled publication method.</span></span> <span data-ttu-id="3507f-771">Profil publikowania zawiera adresy URL, poświadczenia użytkownika i parametry bazy danych wymagane do nawiązania połączenia i uwierzytelniania dla każdego z punktów końcowych, dla których włączono metoda publikacji.</span><span class="sxs-lookup"><span data-stu-id="3507f-771">The publish profile contains the URLs, user credentials and database strings required to connect to and authenticate against each of the endpoints for which a publication method is enabled.</span></span> <span data-ttu-id="3507f-772">**Microsoft WebMatrix 2**, **Microsoft Visual Studio Express for Web** i **programu Microsoft Visual Studio 2012** obsługują odczytywanie publikowanie profile do zautomatyzowania te programy Publikowanie aplikacji sieci web do witryn sieci Web systemu Windows Azure.</span><span class="sxs-lookup"><span data-stu-id="3507f-772">**Microsoft WebMatrix 2**, **Microsoft Visual Studio Express for Web** and **Microsoft Visual Studio 2012** support reading publish profiles to automate configuration of these programs for publishing web applications to Windows Azure websites.</span></span>

    <span data-ttu-id="3507f-773">![Pobieranie witryny sieci web profilu publikowania](aspnet-mvc-4-fundamentals/_static/image54.png "pobierania witryny sieci web profilu publikowania")</span><span class="sxs-lookup"><span data-stu-id="3507f-773">![Downloading the web site publish profile](aspnet-mvc-4-fundamentals/_static/image54.png "Downloading the web site publish profile")</span></span>

    <span data-ttu-id="3507f-774">*Pobieranie witryny sieci Web profilu publikowania*</span><span class="sxs-lookup"><span data-stu-id="3507f-774">*Downloading the Web Site publish profile*</span></span>
8. <span data-ttu-id="3507f-775">Pobierz profil publikowania w znanej lokalizacji.</span><span class="sxs-lookup"><span data-stu-id="3507f-775">Download the publish profile file to a known location.</span></span> <span data-ttu-id="3507f-776">Dodatkowo w tym ćwiczeniu zobaczysz jak opublikować aplikację sieci web do witryny sieci Web systemu Windows Azure w programie Visual Studio przy użyciu tego pliku.</span><span class="sxs-lookup"><span data-stu-id="3507f-776">Further in this exercise you will see how to use this file to publish a web application to a Windows Azure Web Sites from Visual Studio.</span></span>

    <span data-ttu-id="3507f-777">![Zapisywanie pliku profilu publikowania](aspnet-mvc-4-fundamentals/_static/image55.png "zapisywanie profilu publikowania")</span><span class="sxs-lookup"><span data-stu-id="3507f-777">![Saving the publish profile file](aspnet-mvc-4-fundamentals/_static/image55.png "Saving the publish profile")</span></span>

    <span data-ttu-id="3507f-778">*Zapisywanie pliku profilu publikowania*</span><span class="sxs-lookup"><span data-stu-id="3507f-778">*Saving the publish profile file*</span></span>

<a id="ApxBTask2"></a>

<a id="Task_2_-_Configuring_the_Database_Server"></a>
#### <a name="task-2---configuring-the-database-server"></a><span data-ttu-id="3507f-779">Zadanie 2 — Konfigurowanie serwera bazy danych</span><span class="sxs-lookup"><span data-stu-id="3507f-779">Task 2 - Configuring the Database Server</span></span>

<span data-ttu-id="3507f-780">Jeśli aplikacja korzysta z programu SQL Server baz danych, należy utworzyć serwer bazy danych SQL.</span><span class="sxs-lookup"><span data-stu-id="3507f-780">If your application makes use of SQL Server databases you will need to create a SQL Database server.</span></span> <span data-ttu-id="3507f-781">Jeśli chcesz wdrożyć prostą aplikację, która nie korzysta z programu SQL Server może pominąć to zadanie.</span><span class="sxs-lookup"><span data-stu-id="3507f-781">If you want to deploy a simple application that does not use SQL Server you might skip this task.</span></span>

1. <span data-ttu-id="3507f-782">Będzie potrzebny serwer bazy danych SQL do przechowywania bazy danych aplikacji.</span><span class="sxs-lookup"><span data-stu-id="3507f-782">You will need a SQL Database server for storing the application database.</span></span> <span data-ttu-id="3507f-783">Można wyświetlić serwery bazy danych SQL z subskrypcji usługi Windows Azure Management Portal pod adresem **baz danych Sql** | **serwerów** | **serwera Pulpit nawigacyjny**.</span><span class="sxs-lookup"><span data-stu-id="3507f-783">You can view the SQL Database servers from your subscription in the Windows Azure Management portal at **Sql Databases** | **Servers** | **Server's Dashboard**.</span></span> <span data-ttu-id="3507f-784">Jeśli nie masz serwer, który został utworzony, można utworzyć przy użyciu jednego **Dodaj** przycisk paska poleceń.</span><span class="sxs-lookup"><span data-stu-id="3507f-784">If you do not have a server created, you can create one using the **Add** button on the command bar.</span></span> <span data-ttu-id="3507f-785">Zwróć uwagę na **nazwę serwera i adres URL, nazwę logowania administratora i hasła**, jak będą używane w następnego zadania.</span><span class="sxs-lookup"><span data-stu-id="3507f-785">Take note of the **server name and URL, administrator login name and password**, as you will use them in the next tasks.</span></span> <span data-ttu-id="3507f-786">Nie należy tworzyć bazy danych jeszcze, jako zostaną utworzone w późniejszym terminie.</span><span class="sxs-lookup"><span data-stu-id="3507f-786">Do not create the database yet, as it will be created in a later stage.</span></span>

    <span data-ttu-id="3507f-787">![Pulpit nawigacyjny serwera bazy danych SQL](aspnet-mvc-4-fundamentals/_static/image56.png "pulpitu nawigacyjnego serwera bazy danych SQL")</span><span class="sxs-lookup"><span data-stu-id="3507f-787">![SQL Database Server Dashboard](aspnet-mvc-4-fundamentals/_static/image56.png "SQL Database Server Dashboard")</span></span>

    <span data-ttu-id="3507f-788">*Pulpit nawigacyjny serwera bazy danych SQL*</span><span class="sxs-lookup"><span data-stu-id="3507f-788">*SQL Database Server Dashboard*</span></span>
2. <span data-ttu-id="3507f-789">W następnym zadaniem Testuj połączenie z bazą danych z programu Visual Studio z tego powodu należy uwzględnić lokalny adres IP serwera liście **dozwolone adresy IP**.</span><span class="sxs-lookup"><span data-stu-id="3507f-789">In the next task you will test the database connection from Visual Studio, for that reason you need to include your local IP address in the server's list of **Allowed IP Addresses**.</span></span> <span data-ttu-id="3507f-790">Aby to zrobić, kliknij przycisk **Konfiguruj**, wybierz adres IP z **bieżącego adresu IP klienta** i wklej go na **początkowy adres IP** i **końcowy adres IP** pola tekstowe i kliknij przycisk ![add-client-ip-address-ok-button](aspnet-mvc-4-fundamentals/_static/image57.png) przycisku.</span><span class="sxs-lookup"><span data-stu-id="3507f-790">To do that, click **Configure**, select the IP address from **Current Client IP Address** and paste it on the **Start IP Address** and **End IP Address** text boxes and click the ![add-client-ip-address-ok-button](aspnet-mvc-4-fundamentals/_static/image57.png) button.</span></span>

    ![Dodawanie adresu IP klienta](aspnet-mvc-4-fundamentals/_static/image58.png)

    <span data-ttu-id="3507f-792">*Dodawanie adresu IP klienta*</span><span class="sxs-lookup"><span data-stu-id="3507f-792">*Adding Client IP Address*</span></span>
3. <span data-ttu-id="3507f-793">Raz **adres IP klienta** jest dodawany do dozwolonych adresów IP kliknij na **zapisać** o potwierdzenie zmian.</span><span class="sxs-lookup"><span data-stu-id="3507f-793">Once the **Client IP Address** is added to the allowed IP addresses list, click on **Save** to confirm the changes.</span></span>

    ![Potwierdzenie zmian](aspnet-mvc-4-fundamentals/_static/image59.png)

    <span data-ttu-id="3507f-795">*Potwierdzenie zmian*</span><span class="sxs-lookup"><span data-stu-id="3507f-795">*Confirm Changes*</span></span>

<a id="ApxBTask3"></a>

<a id="Task_3_-_Publishing_an_ASPNET_MVC_4_Application_using_Web_Deploy"></a>
#### <a name="task-3---publishing-an-aspnet-mvc-4-application-using-web-deploy"></a><span data-ttu-id="3507f-796">Zadanie 3 - publikowania aplikacji ASP.NET MVC 4 przy użyciu narzędzia Web Deploy</span><span class="sxs-lookup"><span data-stu-id="3507f-796">Task 3 - Publishing an ASP.NET MVC 4 Application using Web Deploy</span></span>

1. <span data-ttu-id="3507f-797">Wróć do rozwiązania ASP.NET MVC 4.</span><span class="sxs-lookup"><span data-stu-id="3507f-797">Go back to the ASP.NET MVC 4 solution.</span></span> <span data-ttu-id="3507f-798">W **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy projekt witryny sieci web i wybierz **publikowania**.</span><span class="sxs-lookup"><span data-stu-id="3507f-798">In the **Solution Explorer**, right-click the web site project and select **Publish**.</span></span>

    <span data-ttu-id="3507f-799">![Publikowanie aplikacji](aspnet-mvc-4-fundamentals/_static/image60.png "publikowania aplikacji")</span><span class="sxs-lookup"><span data-stu-id="3507f-799">![Publishing the Application](aspnet-mvc-4-fundamentals/_static/image60.png "Publishing the Application")</span></span>

    <span data-ttu-id="3507f-800">*Publikowanie witryny sieci web*</span><span class="sxs-lookup"><span data-stu-id="3507f-800">*Publishing the web site*</span></span>
2. <span data-ttu-id="3507f-801">Zaimportuj profil publikowania, zapisana w pierwszym zadaniu.</span><span class="sxs-lookup"><span data-stu-id="3507f-801">Import the publish profile you saved in the first task.</span></span>

    <span data-ttu-id="3507f-802">![Importowanie profilu publikowania](aspnet-mvc-4-fundamentals/_static/image61.png "Importowanie profilu publikowania")</span><span class="sxs-lookup"><span data-stu-id="3507f-802">![Importing the publish profile](aspnet-mvc-4-fundamentals/_static/image61.png "Importing the publish profile")</span></span>

    <span data-ttu-id="3507f-803">*Importowanie profilu publikowania*</span><span class="sxs-lookup"><span data-stu-id="3507f-803">*Importing publish profile*</span></span>
3. <span data-ttu-id="3507f-804">Kliknij przycisk **Weryfikacja połączenia z**.</span><span class="sxs-lookup"><span data-stu-id="3507f-804">Click **Validate Connection**.</span></span> <span data-ttu-id="3507f-805">Po zakończeniu sprawdzania kliknij **dalej**.</span><span class="sxs-lookup"><span data-stu-id="3507f-805">Once Validation is complete click **Next**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="3507f-806">Zakończeniu sprawdzania poprawności, gdy zostanie wyświetlony zielony znacznik wyboru są wyświetlane obok przycisku sprawdzania poprawności połączenia.</span><span class="sxs-lookup"><span data-stu-id="3507f-806">Validation is complete once you see a green checkmark appear next to the Validate Connection button.</span></span>

    <span data-ttu-id="3507f-807">![Sprawdzanie poprawności połączenia](aspnet-mvc-4-fundamentals/_static/image62.png "sprawdzanie poprawności połączenia")</span><span class="sxs-lookup"><span data-stu-id="3507f-807">![Validating connection](aspnet-mvc-4-fundamentals/_static/image62.png "Validating connection")</span></span>

    <span data-ttu-id="3507f-808">*Sprawdzanie poprawności połączenia*</span><span class="sxs-lookup"><span data-stu-id="3507f-808">*Validating connection*</span></span>
4. <span data-ttu-id="3507f-809">W **ustawienia** w obszarze **baz danych** sekcji, kliknij przycisk Dalej, aby textbox połączenia bazy danych (tj. **połączenia DefaultConnection**).</span><span class="sxs-lookup"><span data-stu-id="3507f-809">In the **Settings** page, under the **Databases** section, click the button next to your database connection's textbox (i.e. **DefaultConnection**).</span></span>

    <span data-ttu-id="3507f-810">![Konfiguracja narzędzia Web deploy](aspnet-mvc-4-fundamentals/_static/image63.png "Konfiguracja narzędzia Web deploy")</span><span class="sxs-lookup"><span data-stu-id="3507f-810">![Web deploy configuration](aspnet-mvc-4-fundamentals/_static/image63.png "Web deploy configuration")</span></span>

    <span data-ttu-id="3507f-811">*Konfiguracja narzędzia Web deploy*</span><span class="sxs-lookup"><span data-stu-id="3507f-811">*Web deploy configuration*</span></span>
5. <span data-ttu-id="3507f-812">Skonfiguruj połączenie z bazą danych w następujący sposób:</span><span class="sxs-lookup"><span data-stu-id="3507f-812">Configure the database connection as follows:</span></span>

    - <span data-ttu-id="3507f-813">W **nazwy serwera** wpisz swoją bazą danych SQL server adresu URL przy użyciu *tcp:* prefiks.</span><span class="sxs-lookup"><span data-stu-id="3507f-813">In the **Server name** type your SQL Database server URL using the *tcp:* prefix.</span></span>
    - <span data-ttu-id="3507f-814">W **nazwy użytkownika** wpisz nazwę logowania administratora serwera.</span><span class="sxs-lookup"><span data-stu-id="3507f-814">In **User name** type your server administrator login name.</span></span>
    - <span data-ttu-id="3507f-815">W **hasło** wpisz hasło logowania administratora serwera.</span><span class="sxs-lookup"><span data-stu-id="3507f-815">In **Password** type your server administrator login password.</span></span>
    - <span data-ttu-id="3507f-816">Wpisz nazwę nowej bazy danych, na przykład: *MVC4SampleDB*.</span><span class="sxs-lookup"><span data-stu-id="3507f-816">Type a new database name, for example: *MVC4SampleDB*.</span></span>

    <span data-ttu-id="3507f-817">![Konfigurowanie parametrów połączenia z lokalizacją docelową](aspnet-mvc-4-fundamentals/_static/image64.png "Konfigurowanie parametrów połączenia z lokalizacją docelową")</span><span class="sxs-lookup"><span data-stu-id="3507f-817">![Configuring destination connection string](aspnet-mvc-4-fundamentals/_static/image64.png "Configuring destination connection string")</span></span>

    <span data-ttu-id="3507f-818">*Konfigurowanie parametrów połączenia z lokalizacją docelową*</span><span class="sxs-lookup"><span data-stu-id="3507f-818">*Configuring destination connection string*</span></span>
6. <span data-ttu-id="3507f-819">Następnie kliknij przycisk **OK**.</span><span class="sxs-lookup"><span data-stu-id="3507f-819">Then click **OK**.</span></span> <span data-ttu-id="3507f-820">Po wyświetleniu monitu można utworzyć bazy danych kliknij **tak**.</span><span class="sxs-lookup"><span data-stu-id="3507f-820">When prompted to create the database click **Yes**.</span></span>

    <span data-ttu-id="3507f-821">![Tworzenie bazy danych](aspnet-mvc-4-fundamentals/_static/image65.png "tworzenie parametry bazy danych")</span><span class="sxs-lookup"><span data-stu-id="3507f-821">![Creating the database](aspnet-mvc-4-fundamentals/_static/image65.png "Creating the database string")</span></span>

    <span data-ttu-id="3507f-822">*Tworzenie bazy danych*</span><span class="sxs-lookup"><span data-stu-id="3507f-822">*Creating the database*</span></span>
7. <span data-ttu-id="3507f-823">Ciągu połączenia używanego do łączenia z bazą danych SQL w systemie Windows Azure jest wyświetlany w pole tekstowe domyślne połączenie.</span><span class="sxs-lookup"><span data-stu-id="3507f-823">The connection string you will use to connect to SQL Database in Windows Azure is shown within Default Connection textbox.</span></span> <span data-ttu-id="3507f-824">Następnie kliknij przycisk **Dalej**.</span><span class="sxs-lookup"><span data-stu-id="3507f-824">Then click **Next**.</span></span>

    <span data-ttu-id="3507f-825">![Parametry połączenia wskazujące bazę danych SQL](aspnet-mvc-4-fundamentals/_static/image66.png "ciąg połączenia wskazujące bazę danych SQL")</span><span class="sxs-lookup"><span data-stu-id="3507f-825">![Connection string pointing to SQL Database](aspnet-mvc-4-fundamentals/_static/image66.png "Connection string pointing to SQL Database")</span></span>

    <span data-ttu-id="3507f-826">*Parametry połączenia wskazujące bazę danych SQL*</span><span class="sxs-lookup"><span data-stu-id="3507f-826">*Connection string pointing to SQL Database*</span></span>
8. <span data-ttu-id="3507f-827">W **Podgląd** kliknij przycisk **publikowania**.</span><span class="sxs-lookup"><span data-stu-id="3507f-827">In the **Preview** page, click **Publish**.</span></span>

    <span data-ttu-id="3507f-828">![Publikowanie aplikacji sieci web](aspnet-mvc-4-fundamentals/_static/image67.png "publikowania aplikacji sieci web")</span><span class="sxs-lookup"><span data-stu-id="3507f-828">![Publishing the web application](aspnet-mvc-4-fundamentals/_static/image67.png "Publishing the web application")</span></span>

    <span data-ttu-id="3507f-829">*Publikowanie aplikacji sieci web*</span><span class="sxs-lookup"><span data-stu-id="3507f-829">*Publishing the web application*</span></span>
9. <span data-ttu-id="3507f-830">Po zakończeniu procesu publikowania domyślnej przeglądarce otworzy opublikowanej witryny sieci web.</span><span class="sxs-lookup"><span data-stu-id="3507f-830">Once the publishing process finishes, your default browser will open the published web site.</span></span>

    <span data-ttu-id="3507f-831">![Aplikacja opublikowana w systemie Windows Azure](aspnet-mvc-4-fundamentals/_static/image68.png "aplikacji publikowanych w systemie Windows Azure")</span><span class="sxs-lookup"><span data-stu-id="3507f-831">![Application published to Windows Azure](aspnet-mvc-4-fundamentals/_static/image68.png "Application published to Windows Azure")</span></span>

    <span data-ttu-id="3507f-832">*Aplikacji publikowanych w systemie Windows Azure*</span><span class="sxs-lookup"><span data-stu-id="3507f-832">*Application published to Windows Azure*</span></span>

<a id="AppendixC"></a>

<a id="Appendix_C_Using_Code_Snippets"></a>
## <a name="appendix-c-using-code-snippets"></a><span data-ttu-id="3507f-833">Dodatek C: korzystania z wstawek kodu</span><span class="sxs-lookup"><span data-stu-id="3507f-833">Appendix C: Using Code Snippets</span></span>

<span data-ttu-id="3507f-834">Wstawki kodu zapewniają całego kodu, które są potrzebne w zasięgu ręki.</span><span class="sxs-lookup"><span data-stu-id="3507f-834">With code snippets, you have all the code you need at your fingertips.</span></span> <span data-ttu-id="3507f-835">Dokument laboratorium informuje o dokładnie po ich użycia, jak pokazano na poniższej ilustracji.</span><span class="sxs-lookup"><span data-stu-id="3507f-835">The lab document will tell you exactly when you can use them, as shown in the following figure.</span></span>

<span data-ttu-id="3507f-836">![Aby wstawić kod do projektu przy użyciu wstawki kodu programu Visual Studio](aspnet-mvc-4-fundamentals/_static/image69.png "wstawki kodu za pomocą programu Visual Studio, aby wstawić kod do projektu")</span><span class="sxs-lookup"><span data-stu-id="3507f-836">![Using Visual Studio code snippets to insert code into your project](aspnet-mvc-4-fundamentals/_static/image69.png "Using Visual Studio code snippets to insert code into your project")</span></span>

<span data-ttu-id="3507f-837">*Aby wstawić kod do projektu przy użyciu wstawki kodu programu Visual Studio*</span><span class="sxs-lookup"><span data-stu-id="3507f-837">*Using Visual Studio code snippets to insert code into your project*</span></span>

<span data-ttu-id="3507f-838">***Aby dodać fragment kodu za pomocą klawiatury (C# tylko)***</span><span class="sxs-lookup"><span data-stu-id="3507f-838">***To add a code snippet using the keyboard (C# only)***</span></span>

1. <span data-ttu-id="3507f-839">Umieść kursor, w którym chcesz wstawić kod.</span><span class="sxs-lookup"><span data-stu-id="3507f-839">Place the cursor where you would like to insert the code.</span></span>
2. <span data-ttu-id="3507f-840">Zacznij wpisywać nazwę fragment (bez spacji i łączniki).</span><span class="sxs-lookup"><span data-stu-id="3507f-840">Start typing the snippet name (without spaces or hyphens).</span></span>
3. <span data-ttu-id="3507f-841">Obejrzyj jako IntelliSense wyświetla zgodne z nazwami wstawki.</span><span class="sxs-lookup"><span data-stu-id="3507f-841">Watch as IntelliSense displays matching snippets' names.</span></span>
4. <span data-ttu-id="3507f-842">Wybierz prawidłowe fragment (lub zachować wpisywanie do momentu wybrania fragment całą nazwę).</span><span class="sxs-lookup"><span data-stu-id="3507f-842">Select the correct snippet (or keep typing until the entire snippet's name is selected).</span></span>
5. <span data-ttu-id="3507f-843">Naciśnij klawisz Tab dwa razy, aby wstawić fragment kodu w lokalizacji kursora.</span><span class="sxs-lookup"><span data-stu-id="3507f-843">Press the Tab key twice to insert the snippet at the cursor location.</span></span>

<span data-ttu-id="3507f-844">![Rozpocznij wpisywanie nazwy fragment](aspnet-mvc-4-fundamentals/_static/image70.png "zacznij wpisywać nazwę wstawki programu")</span><span class="sxs-lookup"><span data-stu-id="3507f-844">![Start typing the snippet name](aspnet-mvc-4-fundamentals/_static/image70.png "Start typing the snippet name")</span></span>

<span data-ttu-id="3507f-845">*Rozpocznij wpisywanie nazwy fragment kodu*</span><span class="sxs-lookup"><span data-stu-id="3507f-845">*Start typing the snippet name*</span></span>

<span data-ttu-id="3507f-846">![Naciśnij klawisz Tab, aby wybrać wyróżnione fragment](aspnet-mvc-4-fundamentals/_static/image71.png "naciśnij klawisz Tab, aby wybrać wyróżnione fragment kodu")</span><span class="sxs-lookup"><span data-stu-id="3507f-846">![Press Tab to select the highlighted snippet](aspnet-mvc-4-fundamentals/_static/image71.png "Press Tab to select the highlighted snippet")</span></span>

<span data-ttu-id="3507f-847">*Naciśnij klawisz Tab, aby wybrać wyróżnione fragment kodu*</span><span class="sxs-lookup"><span data-stu-id="3507f-847">*Press Tab to select the highlighted snippet*</span></span>

<span data-ttu-id="3507f-848">![Ponownie naciśnij klawisz Tab i fragment rozwinie](aspnet-mvc-4-fundamentals/_static/image72.png "rozwinie ponownie naciśnij klawisz Tab i wstawki kodu")</span><span class="sxs-lookup"><span data-stu-id="3507f-848">![Press Tab again and the snippet will expand](aspnet-mvc-4-fundamentals/_static/image72.png "Press Tab again and the snippet will expand")</span></span>

<span data-ttu-id="3507f-849">*Rozwinie ponownie naciśnij klawisz Tab i wstawki kodu*</span><span class="sxs-lookup"><span data-stu-id="3507f-849">*Press Tab again and the snippet will expand*</span></span>

<span data-ttu-id="3507f-850">***Aby dodać fragment kodu za pomocą myszy (C#, Visual Basic i XML)*** 1.</span><span class="sxs-lookup"><span data-stu-id="3507f-850">***To add a code snippet using the mouse (C#, Visual Basic and XML)*** 1.</span></span> <span data-ttu-id="3507f-851">Kliknij prawym przyciskiem myszy, gdy chcesz wstawić fragment kodu.</span><span class="sxs-lookup"><span data-stu-id="3507f-851">Right-click where you want to insert the code snippet.</span></span>

1. <span data-ttu-id="3507f-852">Wybierz **wstawić fragment** następuje **Moje wstawki kodu**.</span><span class="sxs-lookup"><span data-stu-id="3507f-852">Select **Insert Snippet** followed by **My Code Snippets**.</span></span>
2. <span data-ttu-id="3507f-853">Wybierz odpowiedni fragment z listy, klikając go.</span><span class="sxs-lookup"><span data-stu-id="3507f-853">Pick the relevant snippet from the list, by clicking on it.</span></span>

<span data-ttu-id="3507f-854">![Kliknij prawym przyciskiem myszy, gdy chcesz wstawić fragment kodu i wybierz wstawić fragment](aspnet-mvc-4-fundamentals/_static/image73.png "kliknij prawym przyciskiem myszy, gdy chcesz wstawić fragment kodu i wybierz wstawić fragment kodu")</span><span class="sxs-lookup"><span data-stu-id="3507f-854">![Right-click where you want to insert the code snippet and select Insert Snippet](aspnet-mvc-4-fundamentals/_static/image73.png "Right-click where you want to insert the code snippet and select Insert Snippet")</span></span>

<span data-ttu-id="3507f-855">*Kliknij prawym przyciskiem myszy, gdy chcesz wstawić fragment kodu i wybierz wstawić fragment kodu*</span><span class="sxs-lookup"><span data-stu-id="3507f-855">*Right-click where you want to insert the code snippet and select Insert Snippet*</span></span>

<span data-ttu-id="3507f-856">![Wybierz odpowiedni fragment z listy, klikając go](aspnet-mvc-4-fundamentals/_static/image74.png "wybierz odpowiedni fragment z listy, klikając go")</span><span class="sxs-lookup"><span data-stu-id="3507f-856">![Pick the relevant snippet from the list, by clicking on it](aspnet-mvc-4-fundamentals/_static/image74.png "Pick the relevant snippet from the list, by clicking on it")</span></span>

<span data-ttu-id="3507f-857">*Wybierz odpowiedni fragment z listy, klikając go*</span><span class="sxs-lookup"><span data-stu-id="3507f-857">*Pick the relevant snippet from the list, by clicking on it*</span></span>
