---
uid: mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-fundamentals
title: Podstawowe informacje na temat platformy ASP.NET MVC 4 | Dokumentacja firmy Microsoft
author: rick-anderson
description: W tym laboratorium Hands-On opiera się na magazynie utworów muzycznych MVC (Model View Controller), samouczek aplikacji, które wprowadzono i opisano krok po kroku, jak używać ASP.NET MV...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/18/2013
ms.topic: article
ms.assetid: b7dba543-73c3-4534-a9a0-ba70fa2c6a8a
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-fundamentals
msc.type: authoredcontent
ms.openlocfilehash: 225dff4663e0e556cfb8966f1078848b4c2b47a5
ms.sourcegitcommit: 3a893ae05f010656d99d6ddf55e82f1b5b6933bc
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 05/18/2018
ms.locfileid: "34306770"
---
# <a name="aspnet-mvc-4-fundamentals"></a><span data-ttu-id="fc507-103">Podstawowe informacje na temat platformy ASP.NET MVC 4</span><span class="sxs-lookup"><span data-stu-id="fc507-103">ASP.NET MVC 4 Fundamentals</span></span>

<span data-ttu-id="fc507-104">Przez [obozów sieci Web Team](https://twitter.com/webcamps)</span><span class="sxs-lookup"><span data-stu-id="fc507-104">By [Web Camps Team](https://twitter.com/webcamps)</span></span>

[<span data-ttu-id="fc507-105">Pobierz obozów sieci Web uczenie Kit</span><span class="sxs-lookup"><span data-stu-id="fc507-105">Download Web Camps Training Kit</span></span>](https://aka.ms/webcamps-training-kit)

<span data-ttu-id="fc507-106">W tym laboratorium Hands-On jest oparta na sklep muzyczny MVC (Model View Controller), samouczek aplikacji, które wprowadzono i opisano krok po kroku, jak używać programu ASP.NET MVC i Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="fc507-106">This Hands-On Lab is based on MVC (Model View Controller) Music Store, a tutorial application that introduces and explains step-by-step how to use ASP.NET MVC and Visual Studio.</span></span> <span data-ttu-id="fc507-107">W laboratorium dowiesz się prostotę, jeszcze mocy przy użyciu tych technologii jednocześnie.</span><span class="sxs-lookup"><span data-stu-id="fc507-107">Throughout the lab you will learn the simplicity, yet power of using these technologies together.</span></span> <span data-ttu-id="fc507-108">Rozpoczyna się od prostej aplikacji i zostanie on skompilowany, aż do uzyskania funkcjonalnej ASP.NET MVC 4 aplikacji sieci Web.</span><span class="sxs-lookup"><span data-stu-id="fc507-108">You will start with a simple application and will build it until you have a fully functional ASP.NET MVC 4 Web Application.</span></span>

<span data-ttu-id="fc507-109">W tym laboratorium współpracuje z platformy ASP.NET MVC 4.</span><span class="sxs-lookup"><span data-stu-id="fc507-109">This Lab works with ASP.NET MVC 4.</span></span>

<span data-ttu-id="fc507-110">Jeśli chcesz zapoznać się z wersji platformy ASP.NET MVC 3 samouczka aplikacji, można znaleźć w [MVC utworów muzycznych magazynu](https://github.com/evilDave/MVC-Music-Store).</span><span class="sxs-lookup"><span data-stu-id="fc507-110">If you wish to explore the ASP.NET MVC 3 version of the tutorial application, you can find it in [MVC-Music-Store](https://github.com/evilDave/MVC-Music-Store).</span></span>

<span data-ttu-id="fc507-111">W tym laboratorium Hands-On przyjęto założenie, że deweloper ma doświadczenie w sieci Web development technologii, takich jak HTML i JavaScript.</span><span class="sxs-lookup"><span data-stu-id="fc507-111">This Hands-On Lab assumes that the developer has experience in Web development technologies, such as HTML and JavaScript.</span></span>

> [!NOTE]
> <span data-ttu-id="fc507-112">Wszystkie przykładowy kod i fragmenty kodu są uwzględnione w sieci Web obozów zestaw szkoleniowy, dostępne pod adresem [wersje Microsoft-Web/WebCampTrainingKit](https://aka.ms/webcamps-training-kit).</span><span class="sxs-lookup"><span data-stu-id="fc507-112">All sample code and snippets are included in the Web Camps Training Kit, available at [Microsoft-Web/WebCampTrainingKit Releases](https://aka.ms/webcamps-training-kit).</span></span> <span data-ttu-id="fc507-113">Specyficzne dla tego laboratorium projektu jest dostępna na [podstawowe informacje na temat platformy ASP.NET MVC 4](https://github.com/Microsoft-Web/HOL-MVC4Fundamentals).</span><span class="sxs-lookup"><span data-stu-id="fc507-113">The project specific to this lab is available at [ASP.NET MVC 4 Fundamentals](https://github.com/Microsoft-Web/HOL-MVC4Fundamentals).</span></span>

<a id="The_Music_Store_application"></a>
### <a name="the-music-store-application"></a><span data-ttu-id="fc507-114">Aplikacji magazynu utworów muzycznych</span><span class="sxs-lookup"><span data-stu-id="fc507-114">The Music Store application</span></span>

<span data-ttu-id="fc507-115">Muzyka Magazyn aplikacji sieci web, który zostanie skompilowany w tym laboratorium obejmuje trzy główne części: zakupów, wyewidencjonowania i administracji.</span><span class="sxs-lookup"><span data-stu-id="fc507-115">The Music Store web application that will be built throughout this Lab comprises three main parts: shopping, checkout, and administration.</span></span> <span data-ttu-id="fc507-116">Osoby odwiedzające będzie Przeglądaj albumów według rodzaju, Dodaj do koszyka ich albumy, przejrzyj ich zaznaczenie i koniec przejść do wyewidencjonowania, aby się zalogować i ukończyć kolejności.</span><span class="sxs-lookup"><span data-stu-id="fc507-116">Visitors will be able to browse albums by genre, add albums to their cart, review their selection and finally proceed to checkout to login and complete the order.</span></span> <span data-ttu-id="fc507-117">Ponadto magazynu administratorzy będą mogli zarządzać albumów dostępne, a także ich głównych właściwości.</span><span class="sxs-lookup"><span data-stu-id="fc507-117">Additionally, store administrators will be able to manage the available albums as well as their main properties.</span></span>

<span data-ttu-id="fc507-118">![Ekrany magazynu utworów muzycznych](aspnet-mvc-4-fundamentals/_static/image1.png "ekrany magazynu utworów muzycznych")</span><span class="sxs-lookup"><span data-stu-id="fc507-118">![Music Store screens](aspnet-mvc-4-fundamentals/_static/image1.png "Music Store screens")</span></span>

<span data-ttu-id="fc507-119">*Ekrany magazynu utworów muzycznych*</span><span class="sxs-lookup"><span data-stu-id="fc507-119">*Music Store screens*</span></span>

<a id="ASPNET_MVC_4_Essentials"></a>
### <a name="aspnet-mvc-4-essentials"></a><span data-ttu-id="fc507-120">ASP.NET MVC 4 Essentials</span><span class="sxs-lookup"><span data-stu-id="fc507-120">ASP.NET MVC 4 Essentials</span></span>

<span data-ttu-id="fc507-121">Aplikację ze sklepu utworów muzycznych zostaną skompilowane z użyciem **kontrolera MVC (Model View)**, wzorzec architektury, która dzieli aplikację na trzy główne składniki:</span><span class="sxs-lookup"><span data-stu-id="fc507-121">Music Store application will be built using **Model View Controller (MVC)**, an architectural pattern that separates an application into three main components:</span></span>

- <span data-ttu-id="fc507-122">**Modele**: obiekty modelu są częściami aplikacji, które implementują logikę domeny.</span><span class="sxs-lookup"><span data-stu-id="fc507-122">**Models**: Model objects are the parts of the application that implement the domain logic.</span></span> <span data-ttu-id="fc507-123">Często obiekty modelu również pobrać i przechowywanie stanu modelu w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="fc507-123">Often, model objects also retrieve and store model state in a database.</span></span>
- <span data-ttu-id="fc507-124">**Widoki:** widoki są składnikami wyświetlającymi interfejs użytkownika aplikacji (UI).</span><span class="sxs-lookup"><span data-stu-id="fc507-124">**Views:** Views are the components that display the application's user interface (UI).</span></span> <span data-ttu-id="fc507-125">Zazwyczaj interfejs ten jest utworzone na podstawie danych modelu.</span><span class="sxs-lookup"><span data-stu-id="fc507-125">Typically, this UI is created from the model data.</span></span> <span data-ttu-id="fc507-126">Przykładem może być widok edycji albumów zawierający pola tekstowe i listy rozwijanej, w oparciu o bieżący stan obiektu albumu.</span><span class="sxs-lookup"><span data-stu-id="fc507-126">An example would be the edit view of Albums that displays text boxes and a drop-down list based on the current state of an Album object.</span></span>
- <span data-ttu-id="fc507-127">**Kontrolery:** kontrolery są składnikami, które obsługują interakcję z użytkownikiem, manipulować modelem i ostatecznie wybierają widok do renderowania interfejsu użytkownika.</span><span class="sxs-lookup"><span data-stu-id="fc507-127">**Controllers:** Controllers are the components that handle user interaction, manipulate the model, and ultimately select a view to render the UI.</span></span> <span data-ttu-id="fc507-128">W aplikacji MVC widoku wyświetlane są tylko informacje; Kontroler obsługuje i ma odpowiadać na dane wejściowe użytkownika i interakcja.</span><span class="sxs-lookup"><span data-stu-id="fc507-128">In an MVC application, the view only displays information; the controller handles and responds to user input and interaction.</span></span>

<span data-ttu-id="fc507-129">Wzorzec MVC pomaga tworzyć aplikacji połączonych różne aspekty aplikacji (logika danych wejściowych, logika biznesowa i logika interfejsu użytkownika), zapewniając luźne powiązanie między tymi elementami.</span><span class="sxs-lookup"><span data-stu-id="fc507-129">The MVC pattern helps you to create applications that separate the different aspects of the application (input logic, business logic, and UI logic), while providing a loose coupling between these elements.</span></span> <span data-ttu-id="fc507-130">Taki rozdział pomaga w zarządzaniu złożonością podczas tworzenia aplikacji, ponieważ pozwala skupić się na jednym aspekcie implementacji naraz.</span><span class="sxs-lookup"><span data-stu-id="fc507-130">This separation helps you manage complexity when you build an application, as it allows you to focus on one aspect of the implementation at a time.</span></span> <span data-ttu-id="fc507-131">Ponadto wzorzec MVC ułatwia testowanie aplikacji również wspieranie stosowania projektowanie oparte na (testach TDD) do tworzenia aplikacji.</span><span class="sxs-lookup"><span data-stu-id="fc507-131">In addition, the MVC pattern makes it easy to test applications, also encouraging the use of test-driven development (TDD) for creating applications.</span></span>

<span data-ttu-id="fc507-132">**ASP.NET MVC** framework stanowi alternatywę dla składnika ASP.NET Web Forms wzorzec służący do tworzenia aplikacji sieci Web opartych na platformie ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="fc507-132">The **ASP.NET MVC** framework provides an alternative to the ASP.NET Web Forms pattern for creating ASP.NET MVC-based Web applications.</span></span> <span data-ttu-id="fc507-133">**ASP.NET MVC** framework to struktura prezentacji niewielka, wysoce testowalna która (tak jak aplikacje oparte na formularzach sieci Web) jest zintegrowana z istniejącymi funkcjami platformy ASP.NET, takich jak strony wzorcowe i oparte na członkostwie uwierzytelnianie, aby pobrać wszystkie możliwości platformy .NET core.</span><span class="sxs-lookup"><span data-stu-id="fc507-133">The **ASP.NET MVC** framework is a lightweight, highly testable presentation framework that (as with Web-forms-based applications) is integrated with existing ASP.NET features, such as master pages and membership-based authentication so you get all the power of the core .NET framework.</span></span> <span data-ttu-id="fc507-134">Jest to przydatne, jeśli już znasz z formularzy sieci Web ASP.NET wszystkie biblioteki, które już używają nie są dostępne w programie ASP.NET MVC 4 oraz.</span><span class="sxs-lookup"><span data-stu-id="fc507-134">This is useful if you are already familiar with ASP.NET Web Forms because all the libraries that you already use are available in ASP.NET MVC 4 as well.</span></span>

<span data-ttu-id="fc507-135">Ponadto luźne powiązanie między trzema głównymi składnikami aplikacji MVC wspiera także Programowanie równoległe.</span><span class="sxs-lookup"><span data-stu-id="fc507-135">In addition, the loose coupling between the three main components of an MVC application also promotes parallel development.</span></span> <span data-ttu-id="fc507-136">Na przykład co może pracować w widoku, drugi może pracować nad logiką kontrolera i trzeci deweloper może skupić się na logiki biznesowej w modelu.</span><span class="sxs-lookup"><span data-stu-id="fc507-136">For instance, one developer can work on the view, a second developer can work on the controller logic, and a third developer can focus on the business logic in the model.</span></span>

<a id="Objectives"></a>

<a id="Objectives"></a>
### <a name="objectives"></a><span data-ttu-id="fc507-137">Cele</span><span class="sxs-lookup"><span data-stu-id="fc507-137">Objectives</span></span>

<span data-ttu-id="fc507-138">W tym laboratorium Hands-On przedstawiono sposób:</span><span class="sxs-lookup"><span data-stu-id="fc507-138">In this Hands-On Lab, you will learn how to:</span></span>

- <span data-ttu-id="fc507-139">Tworzenie aplikacji platformy ASP.NET MVC od podstaw oparte na samouczek aplikacji magazynu utworów muzycznych</span><span class="sxs-lookup"><span data-stu-id="fc507-139">Create an ASP.NET MVC application from scratch based on the Music Store Application tutorial</span></span>
- <span data-ttu-id="fc507-140">Dodaj kontrolery do obsługi adresów URL do strony głównej witryny oraz przeglądanie jej funkcji main</span><span class="sxs-lookup"><span data-stu-id="fc507-140">Add Controllers to handle URLs to the Home page of the site and for browsing its main functionality</span></span>
- <span data-ttu-id="fc507-141">Dodaj widok, aby dostosować zawartość wyświetlana wraz z jego styl</span><span class="sxs-lookup"><span data-stu-id="fc507-141">Add a View to customize the content displayed along with its style</span></span>
- <span data-ttu-id="fc507-142">Dodawanie klasy modelu zawierają i zarządzanie nimi danych i domeny logiki</span><span class="sxs-lookup"><span data-stu-id="fc507-142">Add Model classes to contain and manage data and domain logic</span></span>
- <span data-ttu-id="fc507-143">Użyj wzorca Model widoku do przekazywania informacji z akcji kontrolera do widoku szablonów</span><span class="sxs-lookup"><span data-stu-id="fc507-143">Use View Model pattern to pass information from controller actions to the view templates</span></span>
- <span data-ttu-id="fc507-144">Eksploruj nowego szablonu platformy ASP.NET MVC 4 dla aplikacji internetowych</span><span class="sxs-lookup"><span data-stu-id="fc507-144">Explore the ASP.NET MVC 4 new template for internet applications</span></span>

<a id="Prerequisites"></a>

<a id="Prerequisites"></a>
### <a name="prerequisites"></a><span data-ttu-id="fc507-145">Wymagania wstępne</span><span class="sxs-lookup"><span data-stu-id="fc507-145">Prerequisites</span></span>

<span data-ttu-id="fc507-146">Musi mieć następujące elementy do przygotowania tego laboratorium:</span><span class="sxs-lookup"><span data-stu-id="fc507-146">You must have the following items to complete this lab:</span></span>

- <span data-ttu-id="fc507-147">[Visual Studio Express 2012 for Web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web) (odczytu [dodatek a.](#AppendixA) instrukcje dotyczące sposobu jego instalacji)</span><span class="sxs-lookup"><span data-stu-id="fc507-147">[Visual Studio 2012 Express for Web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web) (read [Appendix A](#AppendixA) for instructions on how to install it)</span></span>

<a id="Setup"></a>

<a id="Setup"></a>
### <a name="setup"></a><span data-ttu-id="fc507-148">Konfiguracja</span><span class="sxs-lookup"><span data-stu-id="fc507-148">Setup</span></span>

<span data-ttu-id="fc507-149">**Instalowanie wstawki kodu**</span><span class="sxs-lookup"><span data-stu-id="fc507-149">**Installing Code Snippets**</span></span>

<span data-ttu-id="fc507-150">Dla wygody taki kod, który będzie zarządzany wzdłuż tego laboratorium jest dostępna jako wstawki kodu programu Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="fc507-150">For convenience, much of the code you will be managing along this lab is available as Visual Studio code snippets.</span></span> <span data-ttu-id="fc507-151">Aby zainstalować fragmenty kodu, uruchom **.\Source\Setup\CodeSnippets.vsi** pliku.</span><span class="sxs-lookup"><span data-stu-id="fc507-151">To install the code snippets run **.\Source\Setup\CodeSnippets.vsi** file.</span></span>

<span data-ttu-id="fc507-152">Jeśli nie masz doświadczenia z wstawki programu Visual Studio i chcesz dowiedzieć się, jak ich używać, można odwołać się do dodatku z tego dokumentu &quot; [wstawki kodu za pomocą programu dodatek C:](#AppendixC)&quot;.</span><span class="sxs-lookup"><span data-stu-id="fc507-152">If you are not familiar with the Visual Studio Code Snippets, and want to learn how to use them, you can refer to the appendix from this document &quot;[Appendix C: Using Code Snippets](#AppendixC)&quot;.</span></span>

<a id="Exercises"></a>

<a id="Exercises"></a>
## <a name="exercises"></a><span data-ttu-id="fc507-153">Ćwiczenia</span><span class="sxs-lookup"><span data-stu-id="fc507-153">Exercises</span></span>

<span data-ttu-id="fc507-154">W tym laboratorium Hands-On składa się przez następujących czynnościach:</span><span class="sxs-lookup"><span data-stu-id="fc507-154">This Hands-On Lab is comprised by the following exercises:</span></span>

1. [<span data-ttu-id="fc507-155">Ćwiczenie 1: Tworzenie projektu aplikacji sieci Web platformy ASP.NET MVC MusicStore</span><span class="sxs-lookup"><span data-stu-id="fc507-155">Exercise 1: Creating MusicStore ASP.NET MVC Web Application Project</span></span>](#Exercise1)
2. [<span data-ttu-id="fc507-156">Ćwiczenie 2: Tworzenie kontrolera</span><span class="sxs-lookup"><span data-stu-id="fc507-156">Exercise 2: Creating a Controller</span></span>](#Exercise2)
3. [<span data-ttu-id="fc507-157">Ćwiczenie 3: Przekazywanie parametrów do kontrolera</span><span class="sxs-lookup"><span data-stu-id="fc507-157">Exercise 3: Passing parameters to a Controller</span></span>](#Exercise3)
4. [<span data-ttu-id="fc507-158">Ćwiczenie 4: Tworzenie widoku</span><span class="sxs-lookup"><span data-stu-id="fc507-158">Exercise 4: Creating a View</span></span>](#Exercise4)
5. [<span data-ttu-id="fc507-159">Ćwiczenie 5: Tworzenie modelu widoku</span><span class="sxs-lookup"><span data-stu-id="fc507-159">Exercise 5: Creating a View Model</span></span>](#Exercise5)
6. [<span data-ttu-id="fc507-160">Ćwiczenie 6: W widoku przy użyciu parametrów</span><span class="sxs-lookup"><span data-stu-id="fc507-160">Exercise 6: Using parameters in View</span></span>](#Exercise6)
7. [<span data-ttu-id="fc507-161">Ćwiczenie 7: Laptop wokół nowego szablonu platformy ASP.NET MVC 4</span><span class="sxs-lookup"><span data-stu-id="fc507-161">Exercise 7: A lap around ASP.NET MVC 4 New Template</span></span>](#Exercise7)

> [!NOTE]
> <span data-ttu-id="fc507-162">Towarzyszy każdego wykonywania **zakończenia** folderu zawierającego wynikowy rozwiązanie, należy uzyskać po wykonaniu ćwiczeniach.</span><span class="sxs-lookup"><span data-stu-id="fc507-162">Each exercise is accompanied by an **End** folder containing the resulting solution you should obtain after completing the exercises.</span></span> <span data-ttu-id="fc507-163">Jeśli potrzebujesz dodatkowej pomocy pracuje nad ćwiczeniami, można użyć tego rozwiązania jako przewodnika.</span><span class="sxs-lookup"><span data-stu-id="fc507-163">You can use this solution as a guide if you need additional help working through the exercises.</span></span>


<span data-ttu-id="fc507-164">Szacowany czas trwania tego laboratorium: **60 minut**.</span><span class="sxs-lookup"><span data-stu-id="fc507-164">Estimated time to complete this lab: **60 minutes**.</span></span>

<a id="Exercise1"></a>

<a id="Exercise_1_Creating_MusicStore_ASPNET_MVC_Web_Application_Project"></a>
### <a name="exercise-1-creating-musicstore-aspnet-mvc-web-application-project"></a><span data-ttu-id="fc507-165">Ćwiczenie 1: Tworzenie projektu aplikacji sieci Web platformy ASP.NET MVC MusicStore</span><span class="sxs-lookup"><span data-stu-id="fc507-165">Exercise 1: Creating MusicStore ASP.NET MVC Web Application Project</span></span>

<span data-ttu-id="fc507-166">W tym ćwiczeniu dowiesz się, tworzenie aplikacji platformy ASP.NET MVC w programie Visual Studio 2012 Express dla sieci Web, a także jego organizacji folderu głównego.</span><span class="sxs-lookup"><span data-stu-id="fc507-166">In this exercise, you will learn how to create an ASP.NET MVC application in Visual Studio 2012 Express for Web as well as its main folder organization.</span></span> <span data-ttu-id="fc507-167">Ponadto dowiesz sposobu dodawania nowego kontrolera i zapewnić ich prostego ciągu wyświetlanego w strony głównej aplikacji.</span><span class="sxs-lookup"><span data-stu-id="fc507-167">Additionally, you will learn how to add a new Controller and make it display a simple string in the application's home page.</span></span>

<a id="Ex1Task1"></a>

<a id="Task_1_-_Creating_the_ASPNET_MVC_Web_Application_Project"></a>
#### <a name="task-1---creating-the-aspnet-mvc-web-application-project"></a><span data-ttu-id="fc507-168">Zadanie 1 — Tworzenie projektu aplikacji sieci Web platformy ASP.NET MVC</span><span class="sxs-lookup"><span data-stu-id="fc507-168">Task 1 - Creating the ASP.NET MVC Web Application Project</span></span>

1. <span data-ttu-id="fc507-169">W ramach tego zadania spowoduje utworzenie pusty projekt aplikacji platformy ASP.NET MVC przy użyciu szablonu MVC programu Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="fc507-169">In this task, you will create an empty ASP.NET MVC application project using the MVC Visual Studio template.</span></span> <span data-ttu-id="fc507-170">Uruchom **VS Express for Web**.</span><span class="sxs-lookup"><span data-stu-id="fc507-170">Start **VS Express for Web**.</span></span>
2. <span data-ttu-id="fc507-171">Na **pliku** menu, kliknij przycisk **nowy projekt**.</span><span class="sxs-lookup"><span data-stu-id="fc507-171">On the **File** menu, click **New Project**.</span></span>
3. <span data-ttu-id="fc507-172">W **nowy projekt** wybierz okno dialogowe **aplikacji sieci Web programu ASP.NET MVC 4** typu znajduje się w obszarze projektu **Visual C#,** **Web** szablonu Lista.</span><span class="sxs-lookup"><span data-stu-id="fc507-172">In the **New Project** dialog box select the **ASP.NET MVC 4 Web Application** project type, located under **Visual C#,** **Web** template list.</span></span>
4. <span data-ttu-id="fc507-173">Zmień **nazwa** do *MvcMusicStore*.</span><span class="sxs-lookup"><span data-stu-id="fc507-173">Change the **Name** to *MvcMusicStore*.</span></span>
5. <span data-ttu-id="fc507-174">Ustaw lokalizację rozwiązania wewnątrz nowy **rozpocząć** folder, w tym ćwiczeniu folder źródłowy, na przykład **\Source\Ex01-CreatingMusicStoreProject\Begin [YOUR-dzień HOL-PATH]**.</span><span class="sxs-lookup"><span data-stu-id="fc507-174">Set the location of the solution inside a new **Begin** folder in this Exercise's Source folder, for example **[YOUR-HOL-PATH]\Source\Ex01-CreatingMusicStoreProject\Begin**.</span></span> <span data-ttu-id="fc507-175">Kliknij przycisk **OK**.</span><span class="sxs-lookup"><span data-stu-id="fc507-175">Click **OK**.</span></span>

    <span data-ttu-id="fc507-176">![Utwórz okno dialogowe Nowy projekt](aspnet-mvc-4-fundamentals/_static/image2.png "utworzyć okno dialogowe Nowy projekt")</span><span class="sxs-lookup"><span data-stu-id="fc507-176">![Create New Project Dialog Box](aspnet-mvc-4-fundamentals/_static/image2.png "Create New Project Dialog Box")</span></span>

    <span data-ttu-id="fc507-177">*Utwórz okno dialogowe Nowy projekt*</span><span class="sxs-lookup"><span data-stu-id="fc507-177">*Create New Project Dialog Box*</span></span>
6. <span data-ttu-id="fc507-178">W **nowy projekt programu ASP.NET MVC 4** wybierz okno dialogowe **podstawowe** szablonu i upewnij się, że **aparat widoku** jest **Razor**.</span><span class="sxs-lookup"><span data-stu-id="fc507-178">In the **New ASP.NET MVC 4 Project** dialog box select the **Basic** template and make sure that the **View engine** selected is **Razor**.</span></span> <span data-ttu-id="fc507-179">Kliknij przycisk **OK**.</span><span class="sxs-lookup"><span data-stu-id="fc507-179">Click **OK**.</span></span>

    <span data-ttu-id="fc507-180">![Okno dialogowe nowego projektu platformy ASP.NET MVC 4](aspnet-mvc-4-fundamentals/_static/image3.png "okno dialogowe Nowy projekt ASP.NET MVC 4")</span><span class="sxs-lookup"><span data-stu-id="fc507-180">![New ASP.NET MVC 4 Project Dialog Box](aspnet-mvc-4-fundamentals/_static/image3.png "New ASP.NET MVC 4 Project Dialog Box")</span></span>

    <span data-ttu-id="fc507-181">*Okno dialogowe nowego projektu platformy ASP.NET MVC 4*</span><span class="sxs-lookup"><span data-stu-id="fc507-181">*New ASP.NET MVC 4 Project Dialog Box*</span></span>

<a id="Ex1Task2"></a>

<a id="Task_2_-_Exploring_the_Solution_Structure"></a>
#### <a name="task-2---exploring-the-solution-structure"></a><span data-ttu-id="fc507-182">Zadanie 2 — poznawanie struktury rozwiązania</span><span class="sxs-lookup"><span data-stu-id="fc507-182">Task 2 - Exploring the Solution Structure</span></span>

<span data-ttu-id="fc507-183">Platforma ASP.NET MVC zawiera szablon projektu Visual Studio, która ułatwia tworzenie aplikacji sieci Web obsługujące wzorzec MVC.</span><span class="sxs-lookup"><span data-stu-id="fc507-183">The ASP.NET MVC framework includes a Visual Studio project template that helps you create Web applications supporting the MVC pattern.</span></span> <span data-ttu-id="fc507-184">Ten szablon tworzy nową aplikację sieci Web programu ASP.NET MVC z wymagane foldery, szablonów elementów i we wpisach w plikach konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="fc507-184">This template creates a new ASP.NET MVC Web application with the required folders, item templates, and configuration-file entries.</span></span>

<span data-ttu-id="fc507-185">To zadanie należy zbadać struktury rozwiązania zrozumienie elementy, które są zaangażowane oraz ich relacji.</span><span class="sxs-lookup"><span data-stu-id="fc507-185">In this task, you will examine the solution structure to understand the elements that are involved and their relationships.</span></span> <span data-ttu-id="fc507-186">Następujące foldery są zawarte w aplikacji ASP.NET MVC, ponieważ platforma ASP.NET MVC domyślnie korzysta z &quot;Konwencji za pośrednictwem konfiguracji&quot; podejście i sprawia, że niektóre założenia domyślne oparte na folder nazewnictwa konwencje.</span><span class="sxs-lookup"><span data-stu-id="fc507-186">The following folders are included in all the ASP.NET MVC application because the ASP.NET MVC framework by default uses a &quot;convention over configuration&quot; approach, and makes some default assumptions based on folder naming conventions.</span></span>

1. <span data-ttu-id="fc507-187">Po utworzeniu projektu, przejrzyj struktury folderów, który został utworzony w Eksploratorze rozwiązań po prawej stronie:</span><span class="sxs-lookup"><span data-stu-id="fc507-187">Once the project is created, review the folder structure that has been created in the Solution Explorer on the right side:</span></span>

    <span data-ttu-id="fc507-188">![Struktura ASP.NET MVC folderów w Eksploratorze rozwiązań](aspnet-mvc-4-fundamentals/_static/image4.png "struktury ASP.NET MVC folderów w Eksploratorze rozwiązań")</span><span class="sxs-lookup"><span data-stu-id="fc507-188">![ASP.NET MVC Folder structure in Solution Explorer](aspnet-mvc-4-fundamentals/_static/image4.png "ASP.NET MVC Folder structure in Solution Explorer")</span></span>

    <span data-ttu-id="fc507-189">*Struktura ASP.NET MVC folderów w Eksploratorze rozwiązań*</span><span class="sxs-lookup"><span data-stu-id="fc507-189">*ASP.NET MVC Folder structure in Solution Explorer*</span></span>

   1. <span data-ttu-id="fc507-190">**Kontrolery**.</span><span class="sxs-lookup"><span data-stu-id="fc507-190">**Controllers**.</span></span> <span data-ttu-id="fc507-191">Ten folder będzie zawierać klasy kontrolera.</span><span class="sxs-lookup"><span data-stu-id="fc507-191">This folder will contain the controller classes.</span></span> <span data-ttu-id="fc507-192">W aplikacji MVC na podstawie kontrolery są zobowiązani do obsługi interakcji użytkownika końcowego, manipulowanie modelem i ostatecznie wybierając widoku do renderowania interfejsu użytkownika.</span><span class="sxs-lookup"><span data-stu-id="fc507-192">In an MVC based application, controllers are responsible for handling end user interaction, manipulating the model, and ultimately choosing a view to render the UI.</span></span>

       > [!NOTE]
       > <span data-ttu-id="fc507-193">Platforma MVC wymaga nazwy wszystkich kontrolerów kończącej się &quot;kontrolera&quot;— na przykład HomeController, LoginController lub ProductController.</span><span class="sxs-lookup"><span data-stu-id="fc507-193">The MVC framework requires the names of all controllers to end with &quot;Controller&quot;-for example, HomeController, LoginController, or ProductController.</span></span>
   2. <span data-ttu-id="fc507-194">**Modele**.</span><span class="sxs-lookup"><span data-stu-id="fc507-194">**Models**.</span></span> <span data-ttu-id="fc507-195">Ten folder jest dostępna dla klasy reprezentujące model aplikacji dla aplikacji sieci Web MVC.</span><span class="sxs-lookup"><span data-stu-id="fc507-195">This folder is provided for classes that represent the application model for the MVC Web application.</span></span> <span data-ttu-id="fc507-196">Obejmuje to zazwyczaj kod, który definiuje obiekty i logika interakcji z magazynu danych.</span><span class="sxs-lookup"><span data-stu-id="fc507-196">This usually includes code that defines objects and the logic for interacting with the data store.</span></span> <span data-ttu-id="fc507-197">Zazwyczaj rzeczywiste modelu obiektów jest w bibliotekach osobnej klasy.</span><span class="sxs-lookup"><span data-stu-id="fc507-197">Typically, the actual model objects will be in separate class libraries.</span></span> <span data-ttu-id="fc507-198">Jednak podczas tworzenia nowej aplikacji może zawierać klasy i przenieś je do biblioteki klas oddzielne w późniejszym czasie w cyklu tworzenia.</span><span class="sxs-lookup"><span data-stu-id="fc507-198">However, when you create a new application, you might include classes and then move them into separate class libraries at a later point in the development cycle.</span></span>
   3. <span data-ttu-id="fc507-199">**Widoki**.</span><span class="sxs-lookup"><span data-stu-id="fc507-199">**Views**.</span></span> <span data-ttu-id="fc507-200">Ten folder jest zalecana lokalizacja widoków, składniki odpowiada za wyświetlanie interfejsu użytkownika aplikacji.</span><span class="sxs-lookup"><span data-stu-id="fc507-200">This folder is the recommended location for views, the components responsible for displaying the application's user interface.</span></span> <span data-ttu-id="fc507-201">Widoki używają plików aspx, ascx, cshtml i .master, oprócz innych plików, które są powiązane z widokami renderowania.</span><span class="sxs-lookup"><span data-stu-id="fc507-201">Views use .aspx, .ascx, .cshtml and .master files, in addition to any other files that are related to rendering views.</span></span> <span data-ttu-id="fc507-202">Widoki folder zawiera folder dla każdego kontrolera; folder nosi nazwę z prefiksem nazwy kontrolera.</span><span class="sxs-lookup"><span data-stu-id="fc507-202">Views folder contains a folder for each controller; the folder is named with the controller-name prefix.</span></span> <span data-ttu-id="fc507-203">Na przykład, jeśli masz kontroler o nazwie **HomeController**, widoki folder zawiera folder o nazwie Home.</span><span class="sxs-lookup"><span data-stu-id="fc507-203">For example, if you have a controller named **HomeController**, the Views folder will contain a folder named Home.</span></span> <span data-ttu-id="fc507-204">Domyślnie, gdy platforma ASP.NET MVC ładuje widoku szuka plik .aspx o nazwie żądany widok w folderze Views\controllerName (**widoków [ControllerName] [Action] .aspx**) lub (**widoków [ControllerName] [Action] .cshtml**) dla widokami Razor.</span><span class="sxs-lookup"><span data-stu-id="fc507-204">By default, when the ASP.NET MVC framework loads a view, it looks for an .aspx file with the requested view name in the Views\controllerName folder (**Views[ControllerName][Action].aspx**) or (**Views[ControllerName][Action].cshtml**) for Razor Views.</span></span>

      > [!NOTE]
      > <span data-ttu-id="fc507-205">Oprócz folderów wymienionymi wcześniej, korzysta z aplikacji sieci Web MVC **Global.asax** plik routingu adresów URL globalne ustawienia domyślne i używa **Web.config** plik do skonfigurowania aplikacji.</span><span class="sxs-lookup"><span data-stu-id="fc507-205">In addition to the folders listed previously, an MVC Web application uses the **Global.asax** file to set global URL routing defaults, and it uses the **Web.config** file to configure the application.</span></span>

<a id="Ex1Task3"></a>

<a id="Task_3_-_Adding_a_HomeController"></a>
#### <a name="task-3---adding-a-homecontroller"></a><span data-ttu-id="fc507-206">Zadanie 3. Dodawanie HomeController</span><span class="sxs-lookup"><span data-stu-id="fc507-206">Task 3 - Adding a HomeController</span></span>

<span data-ttu-id="fc507-207">W aplikacjach ASP.NET, które nie korzystają z struktura MVC interakcji z użytkownikiem są zorganizowane wokół stron i wywoływanie zdarzeń i obsługa zdarzeń z tych stron.</span><span class="sxs-lookup"><span data-stu-id="fc507-207">In ASP.NET applications that do not use the MVC framework, user interaction is organized around pages, and around raising and handling events from those pages.</span></span> <span data-ttu-id="fc507-208">Z kolei interakcji użytkowników z aplikacji ASP.NET MVC skupiono kontrolerów i metod akcji.</span><span class="sxs-lookup"><span data-stu-id="fc507-208">In contrast, user interaction with ASP.NET MVC applications is organized around controllers and their action methods.</span></span>

<span data-ttu-id="fc507-209">Z drugiej strony platforma ASP.NET MVC mapuje adresy URL do klasy, które są określane jako kontrolery.</span><span class="sxs-lookup"><span data-stu-id="fc507-209">On the other hand, ASP.NET MVC framework maps URLs to classes that are referred to as controllers.</span></span> <span data-ttu-id="fc507-210">Kontrolery przetworzyć żądań przychodzących, obsługi danych wejściowych użytkownika i interakcji, wykonywanie logiki aplikacji odpowiednie i określić odpowiedzi do odesłania do klienta (wyświetlania kodu HTML, Pobierz plik, przekierowania do innego adresu URL itp.).</span><span class="sxs-lookup"><span data-stu-id="fc507-210">Controllers process incoming requests, handle user input and interactions, execute appropriate application logic and determine the response to send back to the client (display HTML, download a file, redirect to a different URL, etc.).</span></span> <span data-ttu-id="fc507-211">W przypadku wyświetlania kodu HTML, klasę kontrolera zwykle wywołuje składnik osobny widok, aby wygenerować kod znaczników HTML dla żądania.</span><span class="sxs-lookup"><span data-stu-id="fc507-211">In the case of displaying HTML, a controller class typically calls a separate view component to generate the HTML markup for the request.</span></span> <span data-ttu-id="fc507-212">W aplikacji MVC widoku wyświetlane są tylko informacje; Kontroler obsługuje i ma odpowiadać na dane wejściowe użytkownika i interakcja.</span><span class="sxs-lookup"><span data-stu-id="fc507-212">In an MVC application, the view only displays information; the controller handles and responds to user input and interaction.</span></span>

<span data-ttu-id="fc507-213">To zadanie spowoduje dodanie klasy kontrolera, która będzie obsługiwać adresy URL do strony głównej witryny sklep muzyczny.</span><span class="sxs-lookup"><span data-stu-id="fc507-213">In this task, you will add a Controller class that will handle URLs to the Home page of the Music Store site.</span></span>

1. <span data-ttu-id="fc507-214">Kliknij prawym przyciskiem myszy **kontrolerów** folder w Eksploratorze rozwiązań wybierz **Dodaj** , a następnie **kontrolera** polecenia:</span><span class="sxs-lookup"><span data-stu-id="fc507-214">Right-click **Controllers** folder within the Solution Explorer, select **Add** and then **Controller** command:</span></span>

    <span data-ttu-id="fc507-215">![Dodaj polecenie kontrolera](aspnet-mvc-4-fundamentals/_static/image5.png "Dodawanie polecenia kontrolera")</span><span class="sxs-lookup"><span data-stu-id="fc507-215">![Add a Controller Command](aspnet-mvc-4-fundamentals/_static/image5.png "Add a Controller Command")</span></span>

    <span data-ttu-id="fc507-216">*Dodaj polecenie kontrolera*</span><span class="sxs-lookup"><span data-stu-id="fc507-216">*Add Controller Command*</span></span>
2. <span data-ttu-id="fc507-217">**Dodaj kontroler** zostanie wyświetlone okno dialogowe.</span><span class="sxs-lookup"><span data-stu-id="fc507-217">The **Add Controller** dialog appears.</span></span> <span data-ttu-id="fc507-218">Nazwa kontrolera *HomeController* i naciśnij klawisz **Dodaj**.</span><span class="sxs-lookup"><span data-stu-id="fc507-218">Name the controller *HomeController* and press **Add**.</span></span>

    <span data-ttu-id="fc507-219">![Kontroler okno dialogowe Dodawanie](aspnet-mvc-4-fundamentals/_static/image6.png "kontrolera okno dialogowe Dodawanie")</span><span class="sxs-lookup"><span data-stu-id="fc507-219">![Add Controller Dialog](aspnet-mvc-4-fundamentals/_static/image6.png "Add Controller Dialog")</span></span>

    <span data-ttu-id="fc507-220">*Dodawanie kontrolera okna dialogowego*</span><span class="sxs-lookup"><span data-stu-id="fc507-220">*Add Controller Dialog*</span></span>
3. <span data-ttu-id="fc507-221">Plik **HomeController.cs** jest tworzony w **kontrolerów** folderu.</span><span class="sxs-lookup"><span data-stu-id="fc507-221">The file **HomeController.cs** is created in the **Controllers** folder.</span></span> <span data-ttu-id="fc507-222">Aby przypisać **HomeController** zwraca ciąg na jego akcji indeksu, Zastąp **indeksu** metodę z następującym kodem:</span><span class="sxs-lookup"><span data-stu-id="fc507-222">In order to have the **HomeController** return a string on its Index action, replace the **Index** method with the following code:</span></span>

    <span data-ttu-id="fc507-223">(Fragment - kodu *podstawy platformy ASP.NET MVC 4 - indeksu HomeController Ex1*)</span><span class="sxs-lookup"><span data-stu-id="fc507-223">(Code Snippet - *ASP.NET MVC 4 Fundamentals - Ex1 HomeController Index*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample1.cs)]

<a id="Ex1Task4"></a>

<a id="Task_4_-_Running_the_Application"></a>
#### <a name="task-4---running-the-application"></a><span data-ttu-id="fc507-224">Zadanie 4 — uruchamianie aplikacji</span><span class="sxs-lookup"><span data-stu-id="fc507-224">Task 4 - Running the Application</span></span>

<span data-ttu-id="fc507-225">To zadanie będzie Wypróbuj tę aplikację w przeglądarce sieci web.</span><span class="sxs-lookup"><span data-stu-id="fc507-225">In this task, you will try out the Application in a web browser.</span></span>

1. <span data-ttu-id="fc507-226">Naciśnij klawisz **F5** do uruchomienia aplikacji.</span><span class="sxs-lookup"><span data-stu-id="fc507-226">Press **F5** to run the Application.</span></span> <span data-ttu-id="fc507-227">Projekt jest kompilowany i uruchamia serwer sieci Web IIS lokalnej.</span><span class="sxs-lookup"><span data-stu-id="fc507-227">The project is compiled and the local IIS Web Server starts.</span></span> <span data-ttu-id="fc507-228">Lokalny serwer sieci Web usług IIS zostanie automatycznie uruchomiona przeglądarki sieci web wskazuje adres URL serwera sieci Web.</span><span class="sxs-lookup"><span data-stu-id="fc507-228">The local IIS Web Server will automatically open a web browser pointing to the URL of the Web server.</span></span>

    <span data-ttu-id="fc507-229">![Aplikacja uruchomiona w przeglądarce sieci web](aspnet-mvc-4-fundamentals/_static/image7.png "aplikacja była uruchomiona w przeglądarce sieci web")</span><span class="sxs-lookup"><span data-stu-id="fc507-229">![Application running in a web browser](aspnet-mvc-4-fundamentals/_static/image7.png "Application running in a web browser")</span></span>

    <span data-ttu-id="fc507-230">*Aplikacja uruchomiona w przeglądarce sieci web*</span><span class="sxs-lookup"><span data-stu-id="fc507-230">*Application running in a web browser*</span></span>

    > [!NOTE]
    > <span data-ttu-id="fc507-231">Lokalnego serwera sieci Web usług IIS będą uruchamiane witryny sieci Web na numer portu wolne losowych.</span><span class="sxs-lookup"><span data-stu-id="fc507-231">The local IIS Web Server will run the website on a random free port number.</span></span> <span data-ttu-id="fc507-232">Na powyższej ilustracji lokacji działa w `http://localhost:50103/`, więc używa portu 50103.</span><span class="sxs-lookup"><span data-stu-id="fc507-232">In the figure above, the site is running at `http://localhost:50103/`, so it's using port 50103.</span></span> <span data-ttu-id="fc507-233">Numer portu może się różnić.</span><span class="sxs-lookup"><span data-stu-id="fc507-233">Your port number may vary.</span></span>
2. <span data-ttu-id="fc507-234">Zamknij przeglądarkę.</span><span class="sxs-lookup"><span data-stu-id="fc507-234">Close the browser.</span></span>

<a id="Exercise2"></a>

<a id="Exercise_2_Creating_a_Controller"></a>
### <a name="exercise-2-creating-a-controller"></a><span data-ttu-id="fc507-235">Ćwiczenie 2: Tworzenie kontrolera</span><span class="sxs-lookup"><span data-stu-id="fc507-235">Exercise 2: Creating a Controller</span></span>

<span data-ttu-id="fc507-236">W tym ćwiczeniu dowiesz się, jak zaktualizować kontroler do zaimplementowania proste funkcje aplikacji utworów muzycznych magazynu.</span><span class="sxs-lookup"><span data-stu-id="fc507-236">In this exercise, you will learn how to update the controller to implement simple functionality of the Music Store application.</span></span> <span data-ttu-id="fc507-237">Ten kontroler określi metod akcji do każdego z następujących określone żądania obsługi:</span><span class="sxs-lookup"><span data-stu-id="fc507-237">That controller will define action methods to handle each of the following specific requests:</span></span>

- <span data-ttu-id="fc507-238">Strony listę gatunkami muzyki utworów muzycznych w magazynie utworów muzycznych</span><span class="sxs-lookup"><span data-stu-id="fc507-238">A listing page of the music genres in the Music Store</span></span>
- <span data-ttu-id="fc507-239">Strona przeglądania, która zawiera listę wszystkich albumów muzycznych dla określonego rodzaju</span><span class="sxs-lookup"><span data-stu-id="fc507-239">A browse page that lists all of the music albums for a particular genre</span></span>
- <span data-ttu-id="fc507-240">Strona szczegółów, która zawiera informacje o albumu określonych utworów muzycznych</span><span class="sxs-lookup"><span data-stu-id="fc507-240">A details page that shows information about a specific music album</span></span>

<span data-ttu-id="fc507-241">W zakresie tego ćwiczenia te akcje po prostu zwróci ciąg przez teraz.</span><span class="sxs-lookup"><span data-stu-id="fc507-241">For the scope of this exercise, those actions will simply return a string by now.</span></span>

<a id="Ex2Task1"></a>

<a id="Task_1_-_Adding_a_New_StoreController_Class"></a>
#### <a name="task-1---adding-a-new-storecontroller-class"></a><span data-ttu-id="fc507-242">Zadanie 1 — Dodawanie nowej klasy StoreController</span><span class="sxs-lookup"><span data-stu-id="fc507-242">Task 1 - Adding a New StoreController Class</span></span>

<span data-ttu-id="fc507-243">To zadanie spowoduje dodanie nowego kontrolera.</span><span class="sxs-lookup"><span data-stu-id="fc507-243">In this task, you will add a new Controller.</span></span>

1. <span data-ttu-id="fc507-244">Jeśli nie już otwarty, uruchom **VS Express for Web 2012**.</span><span class="sxs-lookup"><span data-stu-id="fc507-244">If not already open, start **VS Express for Web 2012**.</span></span>
2. <span data-ttu-id="fc507-245">W **pliku** menu, wybierz **Otwórz projekt**.</span><span class="sxs-lookup"><span data-stu-id="fc507-245">In the **File** menu, choose **Open Project**.</span></span> <span data-ttu-id="fc507-246">W oknie dialogowym Otwórz projekt, przejdź do **Source\Ex02 CreatingAController\Begin**, wybierz pozycję **Begin.sln** i kliknij przycisk **Otwórz**.</span><span class="sxs-lookup"><span data-stu-id="fc507-246">In the Open Project dialog, browse to **Source\Ex02-CreatingAController\Begin**, select **Begin.sln** and click **Open**.</span></span> <span data-ttu-id="fc507-247">Alternatywnie możesz nadal z rozwiązaniem uzyskany po zakończeniu pracy w poprzednim ćwiczeniu.</span><span class="sxs-lookup"><span data-stu-id="fc507-247">Alternatively, you may continue with the solution that you obtained after completing the previous exercise.</span></span>

   1. <span data-ttu-id="fc507-248">Po otwarciu dostarczonych **rozpocząć** rozwiązania, musisz pobrać niektórych brakujących pakietów NuGet aby kontynuować.</span><span class="sxs-lookup"><span data-stu-id="fc507-248">If you opened the provided **Begin** solution, you will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="fc507-249">Aby to zrobić, kliknij przycisk **projektu** menu i wybierz **Zarządzaj pakietami NuGet**.</span><span class="sxs-lookup"><span data-stu-id="fc507-249">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
   2. <span data-ttu-id="fc507-250">W **Zarządzaj pakietami NuGet** okna dialogowego, kliknij przycisk **przywrócić** celu pobieranie brakujących pakietów.</span><span class="sxs-lookup"><span data-stu-id="fc507-250">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
   3. <span data-ttu-id="fc507-251">Na koniec Skompiluj rozwiązanie, klikając **kompilacji** | **Kompiluj rozwiązanie**.</span><span class="sxs-lookup"><span data-stu-id="fc507-251">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

      > [!NOTE]
      > <span data-ttu-id="fc507-252">Jedną z zalet przy użyciu narzędzia NuGet jest, że nie masz do wysłania wszystkich bibliotek w projekcie, zmniejszenie jego rozmiar projektu.</span><span class="sxs-lookup"><span data-stu-id="fc507-252">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="fc507-253">Narzędzia Power NuGet określając wersje pakietów w pliku Packages.config, będzie można pobrać wymaganych bibliotek podczas pierwszego uruchomienia projektu.</span><span class="sxs-lookup"><span data-stu-id="fc507-253">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="fc507-254">Jest to, dlaczego konieczne będzie wykonanie tych kroków, po otwarciu istniejącego rozwiązania z tego laboratorium.</span><span class="sxs-lookup"><span data-stu-id="fc507-254">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
3. <span data-ttu-id="fc507-255">Dodaj nowy kontroler.</span><span class="sxs-lookup"><span data-stu-id="fc507-255">Add the new controller.</span></span> <span data-ttu-id="fc507-256">Aby to zrobić, kliknij prawym przyciskiem myszy **kontrolerów** folder w Eksploratorze rozwiązań wybierz **Dodaj** , a następnie **kontrolera** polecenia.</span><span class="sxs-lookup"><span data-stu-id="fc507-256">To do this, right-click the **Controllers** folder within the Solution Explorer, select **Add** and then the **Controller** command.</span></span> <span data-ttu-id="fc507-257">Zmień **nazwy kontrolera** do *StoreController*i kliknij przycisk **Dodaj**.</span><span class="sxs-lookup"><span data-stu-id="fc507-257">Change the **Controller Name** to *StoreController*, and click **Add**.</span></span>

    <span data-ttu-id="fc507-258">![Kontroler okno dialogowe Dodawanie](aspnet-mvc-4-fundamentals/_static/image8.png "kontrolera okno dialogowe Dodawanie")</span><span class="sxs-lookup"><span data-stu-id="fc507-258">![Add Controller Dialog](aspnet-mvc-4-fundamentals/_static/image8.png "Add Controller Dialog")</span></span>

    <span data-ttu-id="fc507-259">*Dodawanie kontrolera okna dialogowego*</span><span class="sxs-lookup"><span data-stu-id="fc507-259">*Add Controller Dialog*</span></span>

<a id="Ex2Task2"></a>

<a id="Task_2_-_Modifying_the_StoreControllers_Actions"></a>
#### <a name="task-2---modifying-the-storecontrollers-actions"></a><span data-ttu-id="fc507-260">Zadanie 2 - modyfikowanie StoreController akcje</span><span class="sxs-lookup"><span data-stu-id="fc507-260">Task 2 - Modifying the StoreController's Actions</span></span>

<span data-ttu-id="fc507-261">To zadanie, należy zmodyfikować metod kontrolera, które są nazywane **akcje**.</span><span class="sxs-lookup"><span data-stu-id="fc507-261">In this task, you will modify the Controller methods that are called **actions**.</span></span> <span data-ttu-id="fc507-262">Akcje są odpowiedzialne za obsługę żądań adresu URL i określanie zawartość, która ma zostać odesłany do przeglądarki lub użytkownik, który wywołał adres URL.</span><span class="sxs-lookup"><span data-stu-id="fc507-262">Actions are responsible for handling URL requests and determining the content that should be sent back to the browser or user that invoked the URL.</span></span>

1. <span data-ttu-id="fc507-263">**StoreController** klasa ma już **indeksu** metody.</span><span class="sxs-lookup"><span data-stu-id="fc507-263">The **StoreController** class already has an **Index** method.</span></span> <span data-ttu-id="fc507-264">Go w dalszej części tego laboratorium zostanie użyta do wdrożenia strony, który zawiera listę wszystkich genres magazynu utworów muzycznych.</span><span class="sxs-lookup"><span data-stu-id="fc507-264">You will use it later in this Lab to implement the page that lists all genres of the music store.</span></span> <span data-ttu-id="fc507-265">Teraz, po prostu zastąpić **indeksu** metodę z następującym kodem, które zwraca ciąg &quot;Hello z Store.Index()&quot;:</span><span class="sxs-lookup"><span data-stu-id="fc507-265">For now, just replace the **Index** method with the following code that returns a string &quot;Hello from Store.Index()&quot;:</span></span>

    <span data-ttu-id="fc507-266">(Fragment - kodu *podstawy platformy ASP.NET MVC 4 - indeksu StoreController Ex2*)</span><span class="sxs-lookup"><span data-stu-id="fc507-266">(Code Snippet - *ASP.NET MVC 4 Fundamentals - Ex2 StoreController Index*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample2.cs)]
2. <span data-ttu-id="fc507-267">Dodaj **Przeglądaj** i **szczegóły** metody.</span><span class="sxs-lookup"><span data-stu-id="fc507-267">Add **Browse** and **Details** methods.</span></span> <span data-ttu-id="fc507-268">Aby to zrobić, Dodaj następujący kod, aby **StoreController**:</span><span class="sxs-lookup"><span data-stu-id="fc507-268">To do this, add the following code to the **StoreController**:</span></span>

    <span data-ttu-id="fc507-269">(Fragment - kodu *podstawy platformy ASP.NET MVC 4 - Ex2 StoreController BrowseAndDetails*)</span><span class="sxs-lookup"><span data-stu-id="fc507-269">(Code Snippet - *ASP.NET MVC 4 Fundamentals - Ex2 StoreController BrowseAndDetails*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample3.cs)]

<a id="Ex2Task3"></a>

<a id="Task_3_-_Running_the_Application"></a>
#### <a name="task-3---running-the-application"></a><span data-ttu-id="fc507-270">Zadanie 3 — uruchamianie aplikacji</span><span class="sxs-lookup"><span data-stu-id="fc507-270">Task 3 - Running the Application</span></span>

<span data-ttu-id="fc507-271">To zadanie będzie Wypróbuj tę aplikację w przeglądarce sieci web.</span><span class="sxs-lookup"><span data-stu-id="fc507-271">In this task, you will try out the Application in a web browser.</span></span>

1. <span data-ttu-id="fc507-272">Naciśnij klawisz **F5** do uruchomienia aplikacji.</span><span class="sxs-lookup"><span data-stu-id="fc507-272">Press **F5** to run the Application.</span></span>
2. <span data-ttu-id="fc507-273">Projekt jest uruchamiany w **Home** strony.</span><span class="sxs-lookup"><span data-stu-id="fc507-273">The project starts in the **Home** page.</span></span> <span data-ttu-id="fc507-274">Zmień adres URL, aby sprawdzić implementacji każdej akcji.</span><span class="sxs-lookup"><span data-stu-id="fc507-274">Change the URL to verify each action's implementation.</span></span>

    1. <span data-ttu-id="fc507-275">**/ Przechowywania**.</span><span class="sxs-lookup"><span data-stu-id="fc507-275">**/Store**.</span></span> <span data-ttu-id="fc507-276">Zobaczysz  **&quot;Hello z Store.Index()&quot;**.</span><span class="sxs-lookup"><span data-stu-id="fc507-276">You will see **&quot;Hello from Store.Index()&quot;**.</span></span>
    2. <span data-ttu-id="fc507-277">**/ Magazynu/Przeglądaj**.</span><span class="sxs-lookup"><span data-stu-id="fc507-277">**/Store/Browse**.</span></span> <span data-ttu-id="fc507-278">Zobaczysz  **&quot;Hello z Store.Browse()&quot;**.</span><span class="sxs-lookup"><span data-stu-id="fc507-278">You will see **&quot;Hello from Store.Browse()&quot;**.</span></span>
    3. <span data-ttu-id="fc507-279">**/ / Szczegóły magazynu**.</span><span class="sxs-lookup"><span data-stu-id="fc507-279">**/Store/Details**.</span></span> <span data-ttu-id="fc507-280">Zobaczysz  **&quot;Hello z Store.Details()&quot;**.</span><span class="sxs-lookup"><span data-stu-id="fc507-280">You will see **&quot;Hello from Store.Details()&quot;**.</span></span>

        <span data-ttu-id="fc507-281">![Przeglądanie StoreBrowse](aspnet-mvc-4-fundamentals/_static/image9.png "przeglądanie StoreBrowse")</span><span class="sxs-lookup"><span data-stu-id="fc507-281">![Browsing StoreBrowse](aspnet-mvc-4-fundamentals/_static/image9.png "Browsing StoreBrowse")</span></span>

        <span data-ttu-id="fc507-282">*Przeglądanie /Store/Browse*</span><span class="sxs-lookup"><span data-stu-id="fc507-282">*Browsing /Store/Browse*</span></span>
3. <span data-ttu-id="fc507-283">Zamknij przeglądarkę.</span><span class="sxs-lookup"><span data-stu-id="fc507-283">Close the browser.</span></span>

<a id="Exercise3"></a>

<a id="Exercise_3_Passing_parameters_to_a_Controller"></a>
### <a name="exercise-3-passing-parameters-to-a-controller"></a><span data-ttu-id="fc507-284">Ćwiczenie 3: Przekazywanie parametrów do kontrolera</span><span class="sxs-lookup"><span data-stu-id="fc507-284">Exercise 3: Passing parameters to a Controller</span></span>

<span data-ttu-id="fc507-285">Do tej pory mieć zostały zwracanie stałej ciągów z kontrolerów.</span><span class="sxs-lookup"><span data-stu-id="fc507-285">Until now, you have been returning constant strings from the Controllers.</span></span> <span data-ttu-id="fc507-286">W tym ćwiczeniu dowiesz się, jak do przekazania parametrów do kontrolera przy użyciu adresu URL i querystring, a następnie sprawdzając akcje metody odpowiedzi z tekstem w przeglądarce.</span><span class="sxs-lookup"><span data-stu-id="fc507-286">In this exercise you will learn how to pass parameters to a Controller using the URL and querystring, and then making the method actions respond with text to the browser.</span></span>

<a id="Ex3Task1"></a>

<a id="Task_1_-_Adding_Genre_Parameter_to_StoreController"></a>
#### <a name="task-1---adding-genre-parameter-to-storecontroller"></a><span data-ttu-id="fc507-287">Zadanie 1 — Dodawanie parametru Genre StoreController</span><span class="sxs-lookup"><span data-stu-id="fc507-287">Task 1 - Adding Genre Parameter to StoreController</span></span>

<span data-ttu-id="fc507-288">W tym zadaniu użyjesz **querystring** wysłać parametry **Przeglądaj** metody akcji w **StoreController**.</span><span class="sxs-lookup"><span data-stu-id="fc507-288">In this task, you will use the **querystring** to send parameters to the **Browse** action method in the **StoreController**.</span></span>

1. <span data-ttu-id="fc507-289">Jeśli nie już otwarty, uruchom **VS Express for Web**.</span><span class="sxs-lookup"><span data-stu-id="fc507-289">If not already open, start **VS Express for Web**.</span></span>
2. <span data-ttu-id="fc507-290">W **pliku** menu, wybierz **Otwórz projekt**.</span><span class="sxs-lookup"><span data-stu-id="fc507-290">In the **File** menu, choose **Open Project**.</span></span> <span data-ttu-id="fc507-291">W oknie dialogowym Otwórz projekt, przejdź do **Source\Ex03 PassingParametersToAController\Begin**, wybierz pozycję **Begin.sln** i kliknij przycisk **Otwórz**.</span><span class="sxs-lookup"><span data-stu-id="fc507-291">In the Open Project dialog, browse to **Source\Ex03-PassingParametersToAController\Begin**, select **Begin.sln** and click **Open**.</span></span> <span data-ttu-id="fc507-292">Alternatywnie możesz nadal z rozwiązaniem uzyskany po zakończeniu pracy w poprzednim ćwiczeniu.</span><span class="sxs-lookup"><span data-stu-id="fc507-292">Alternatively, you may continue with the solution that you obtained after completing the previous exercise.</span></span>

   1. <span data-ttu-id="fc507-293">Po otwarciu dostarczonych **rozpocząć** rozwiązania, musisz pobrać niektórych brakujących pakietów NuGet aby kontynuować.</span><span class="sxs-lookup"><span data-stu-id="fc507-293">If you opened the provided **Begin** solution, you will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="fc507-294">Aby to zrobić, kliknij przycisk **projektu** menu i wybierz **Zarządzaj pakietami NuGet**.</span><span class="sxs-lookup"><span data-stu-id="fc507-294">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
   2. <span data-ttu-id="fc507-295">W **Zarządzaj pakietami NuGet** okna dialogowego, kliknij przycisk **przywrócić** celu pobieranie brakujących pakietów.</span><span class="sxs-lookup"><span data-stu-id="fc507-295">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
   3. <span data-ttu-id="fc507-296">Na koniec Skompiluj rozwiązanie, klikając **kompilacji** | **Kompiluj rozwiązanie**.</span><span class="sxs-lookup"><span data-stu-id="fc507-296">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

      > [!NOTE]
      > <span data-ttu-id="fc507-297">Jedną z zalet przy użyciu narzędzia NuGet jest, że nie masz do wysłania wszystkich bibliotek w projekcie, zmniejszenie jego rozmiar projektu.</span><span class="sxs-lookup"><span data-stu-id="fc507-297">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="fc507-298">Narzędzia Power NuGet określając wersje pakietów w pliku Packages.config, będzie można pobrać wymaganych bibliotek podczas pierwszego uruchomienia projektu.</span><span class="sxs-lookup"><span data-stu-id="fc507-298">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="fc507-299">Jest to, dlaczego konieczne będzie wykonanie tych kroków, po otwarciu istniejącego rozwiązania z tego laboratorium.</span><span class="sxs-lookup"><span data-stu-id="fc507-299">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
3. <span data-ttu-id="fc507-300">Otwórz **StoreController** klasy.</span><span class="sxs-lookup"><span data-stu-id="fc507-300">Open **StoreController** class.</span></span> <span data-ttu-id="fc507-301">Aby to zrobić, w **Eksploratora rozwiązań**, rozwiń węzeł **kontrolerów** folder i kliknij dwukrotnie **StoreController.cs**.</span><span class="sxs-lookup"><span data-stu-id="fc507-301">To do this, in the **Solution Explorer**, expand the **Controllers** folder and double-click **StoreController.cs**.</span></span>
4. <span data-ttu-id="fc507-302">Zmień **Przeglądaj** metody, dodając parametr typu string na żądanie dla określonego rodzaju.</span><span class="sxs-lookup"><span data-stu-id="fc507-302">Change the **Browse** method, adding a string parameter to request for a specific genre.</span></span> <span data-ttu-id="fc507-303">ASP.NET MVC zostanie automatycznie przekazać żadnych querystring lub przesłanie formularza parametrów o nazwie **genre** do tej metody akcji, gdy została wywołana.</span><span class="sxs-lookup"><span data-stu-id="fc507-303">ASP.NET MVC will automatically pass any querystring or form post parameters named **genre** to this action method when invoked.</span></span> <span data-ttu-id="fc507-304">Aby to zrobić, Zamień **Przeglądaj** metodę z następującym kodem:</span><span class="sxs-lookup"><span data-stu-id="fc507-304">To do this, replace the **Browse** method with the following code:</span></span>

    <span data-ttu-id="fc507-305">(Fragment - kodu *podstawy platformy ASP.NET MVC 4 - Ex3 StoreController BrowseMethod*)</span><span class="sxs-lookup"><span data-stu-id="fc507-305">(Code Snippet - *ASP.NET MVC 4 Fundamentals - Ex3 StoreController BrowseMethod*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample4.cs)]

> [!NOTE]
> <span data-ttu-id="fc507-306">Używasz **HttpUtility.HtmlEncode** metodę narzędzie uniemożliwia użytkownikom wstrzykiwania Javascript do widoku z łączem, takich jak   **/magazynu/Przeglądaj? Genre =&lt;skryptu&gt;window.location= "[http://hackersite.com](http://hackersite.com)"&lt;/script&gt;**.</span><span class="sxs-lookup"><span data-stu-id="fc507-306">You are using the **HttpUtility.HtmlEncode** utility method to prevents users from injecting Javascript into the View with a link like **/Store/Browse?Genre=&lt;script&gt;window.location='[http://hackersite.com](http://hackersite.com)'&lt;/script&gt;**.</span></span>
> 
> <span data-ttu-id="fc507-307">Aby uzyskać dokładniejsze objaśnienie, odwiedź stronę [ten artykuł w witrynie msdn](https://msdn.microsoft.com/library/a2a4yykt(v=VS.80).aspx).</span><span class="sxs-lookup"><span data-stu-id="fc507-307">For further explanation, please visit [this msdn article](https://msdn.microsoft.com/library/a2a4yykt(v=VS.80).aspx).</span></span>

<a id="Ex3Task2"></a>

<a id="Task_2_-_Running_the_Application"></a>
#### <a name="task-2---running-the-application"></a><span data-ttu-id="fc507-308">Zadanie 2 — w uruchomionej aplikacji</span><span class="sxs-lookup"><span data-stu-id="fc507-308">Task 2 - Running the Application</span></span>

<span data-ttu-id="fc507-309">W tym zadaniu zostanie wypróbowanie aplikacji w przeglądarce sieci web i używać **genre** parametru.</span><span class="sxs-lookup"><span data-stu-id="fc507-309">In this task, you will try out the Application in a web browser and use the **genre** parameter.</span></span>

1. <span data-ttu-id="fc507-310">Naciśnij klawisz **F5** do uruchomienia aplikacji.</span><span class="sxs-lookup"><span data-stu-id="fc507-310">Press **F5** to run the Application.</span></span>
2. <span data-ttu-id="fc507-311">Projekt jest uruchamiany w **Home** strony.</span><span class="sxs-lookup"><span data-stu-id="fc507-311">The project starts in the **Home** page.</span></span> <span data-ttu-id="fc507-312">Zmień adres URL do   */przechowywania/Przeglądaj? Genre = Disco* można zweryfikować, że akcja otrzyma parametru genre.</span><span class="sxs-lookup"><span data-stu-id="fc507-312">Change the URL to */Store/Browse?Genre=Disco* to verify that the action receives the genre parameter.</span></span>

    <span data-ttu-id="fc507-313">![Przeglądanie StoreBrowseGenre = Disco](aspnet-mvc-4-fundamentals/_static/image10.png "przeglądanie StoreBrowseGenre = Disco")</span><span class="sxs-lookup"><span data-stu-id="fc507-313">![Browsing StoreBrowseGenre=Disco](aspnet-mvc-4-fundamentals/_static/image10.png "Browsing StoreBrowseGenre=Disco")</span></span>

    <span data-ttu-id="fc507-314">*Przeglądanie /Store/Browse? Genre = Disco*</span><span class="sxs-lookup"><span data-stu-id="fc507-314">*Browsing /Store/Browse?Genre=Disco*</span></span>
3. <span data-ttu-id="fc507-315">Zamknij przeglądarkę.</span><span class="sxs-lookup"><span data-stu-id="fc507-315">Close the browser.</span></span>

<a id="Ex3Task3"></a>

<a id="Task_3_-_Adding_an_Id_Parameter_Embedded_in_the_URL"></a>
#### <a name="task-3---adding-an-id-parameter-embedded-in-the-url"></a><span data-ttu-id="fc507-316">Zadanie 3. Dodawanie parametru identyfikatora osadzone w adresie URL</span><span class="sxs-lookup"><span data-stu-id="fc507-316">Task 3 - Adding an Id Parameter Embedded in the URL</span></span>

<span data-ttu-id="fc507-317">W tym zadaniu użyjesz **adres URL** do przekazania **identyfikator** parametr **szczegóły** metody akcji **StoreController**.</span><span class="sxs-lookup"><span data-stu-id="fc507-317">In this task, you will use the **URL** to pass an **Id** parameter to the **Details** action method of the **StoreController**.</span></span> <span data-ttu-id="fc507-318">ASP.NET MVC domyślnej konwencji routingu jest Traktuj segment adresu URL po nazwy metody akcji, jako parametr o nazwie **identyfikator**. Jeśli metodę akcji ma parametr o nazwie identyfikator, ASP.NET MVC zostanie automatycznie przekazuj segment adresu URL do użytkownika jako parametr.</span><span class="sxs-lookup"><span data-stu-id="fc507-318">ASP.NET MVC's default routing convention is to treat the segment of a URL after the action method name as a parameter named **Id**. If your action method has parameter named Id, then ASP.NET MVC will automatically pass the URL segment to you as a parameter.</span></span> <span data-ttu-id="fc507-319">W adresie URL **5-magazynu/szczegóły**, **identyfikator** zostanie potraktowany jako **5**.</span><span class="sxs-lookup"><span data-stu-id="fc507-319">In the URL **Store/Details/5**, **Id** will be interpreted as **5**.</span></span>

1. <span data-ttu-id="fc507-320">Zmień **szczegóły** metody **StoreController**, dodając **int** parametr o nazwie **identyfikator**. Aby to zrobić, Zamień **szczegóły** metodę z następującym kodem:</span><span class="sxs-lookup"><span data-stu-id="fc507-320">Change the **Details** method of the **StoreController**, adding an **int** parameter called **id**. To do this, replace the **Details** method with the following code:</span></span>

    <span data-ttu-id="fc507-321">(Fragment - kodu *podstawy platformy ASP.NET MVC 4 - Ex3 StoreController DetailsMethod*)</span><span class="sxs-lookup"><span data-stu-id="fc507-321">(Code Snippet - *ASP.NET MVC 4 Fundamentals - Ex3 StoreController DetailsMethod*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample5.cs)]

<a id="Ex3Task4"></a>

<a id="Task_4_-_Running_the_Application"></a>
#### <a name="task-4---running-the-application"></a><span data-ttu-id="fc507-322">Zadanie 4 — uruchamianie aplikacji</span><span class="sxs-lookup"><span data-stu-id="fc507-322">Task 4 - Running the Application</span></span>

<span data-ttu-id="fc507-323">W tym zadaniu zostanie wypróbowanie aplikacji w przeglądarce sieci web i używać **identyfikator** parametru.</span><span class="sxs-lookup"><span data-stu-id="fc507-323">In this task, you will try out the Application in a web browser and use the **Id** parameter.</span></span>

1. <span data-ttu-id="fc507-324">Naciśnij klawisz **F5** do uruchomienia aplikacji.</span><span class="sxs-lookup"><span data-stu-id="fc507-324">Press **F5** to run the Application.</span></span>
2. <span data-ttu-id="fc507-325">Projekt jest uruchamiany w **Home** strony.</span><span class="sxs-lookup"><span data-stu-id="fc507-325">The project starts in the **Home** page.</span></span> <span data-ttu-id="fc507-326">Zmień adres URL do */Store/Details/5* można zweryfikować, że akcja otrzyma parametru identyfikatora.</span><span class="sxs-lookup"><span data-stu-id="fc507-326">Change the URL to */Store/Details/5* to verify that the action receives the id parameter.</span></span>

    <span data-ttu-id="fc507-327">![Przeglądanie StoreDetails5](aspnet-mvc-4-fundamentals/_static/image11.png "przeglądanie StoreDetails5")</span><span class="sxs-lookup"><span data-stu-id="fc507-327">![Browsing StoreDetails5](aspnet-mvc-4-fundamentals/_static/image11.png "Browsing StoreDetails5")</span></span>

    <span data-ttu-id="fc507-328">*Przeglądanie /Store/Details/5*</span><span class="sxs-lookup"><span data-stu-id="fc507-328">*Browsing /Store/Details/5*</span></span>

<a id="Exercise4"></a>

<a id="Exercise_4_Creating_a_View"></a>
### <a name="exercise-4-creating-a-view"></a><span data-ttu-id="fc507-329">Ćwiczenie 4: Tworzenie widoku</span><span class="sxs-lookup"><span data-stu-id="fc507-329">Exercise 4: Creating a View</span></span>

<span data-ttu-id="fc507-330">Do tej pory ma zostały zwracanie ciągów z akcji kontrolera.</span><span class="sxs-lookup"><span data-stu-id="fc507-330">So far you have been returning strings from controller actions.</span></span> <span data-ttu-id="fc507-331">Chociaż jest to wygodny sposób zrozumienia, jak działają kontrolery, jest nie jak rzeczywistych aplikacji sieci Web są tworzone.</span><span class="sxs-lookup"><span data-stu-id="fc507-331">Although that is a useful way of understanding how controllers work, it is not how your real Web applications are built.</span></span> <span data-ttu-id="fc507-332">Widoki są składnikami, które zapewniają lepszym rozwiązaniem dla generowania kodu HTML do przeglądarki przy użyciu plików szablonów.</span><span class="sxs-lookup"><span data-stu-id="fc507-332">Views are components that provide a better approach for generating HTML back to the browser with the use of template files.</span></span>

<span data-ttu-id="fc507-333">W tym ćwiczeniu dowiesz się, jak dodać układ strony wzorcowej można skonfigurować szablon dla typowych zawartość HTML, arkusz stylów w celu zwiększenia wyglądu i działania witryny, a na koniec szablon widoku, aby włączyć HomeController do zwrócenia HTML.</span><span class="sxs-lookup"><span data-stu-id="fc507-333">In this exercise you will learn how to add a layout master page to setup a template for common HTML content, a StyleSheet to enhance the look and feel of the site and finally a View template to enable HomeController to return HTML.</span></span>

<a id="Ex4Task1"></a>

<a id="Task_1_-_Modifying_the_file__layoutcshtml"></a>
#### <a name="task-1---modifying-the-file-layoutcshtml"></a><span data-ttu-id="fc507-334">Zadanie 1 - modyfikowania pliku \_layout.cshtml</span><span class="sxs-lookup"><span data-stu-id="fc507-334">Task 1 - Modifying the file \_layout.cshtml</span></span>

<span data-ttu-id="fc507-335">Plik **~/Views/Shared/\_layout.cshtml** pozwala skonfigurować szablon HTML wspólne korzystanie w całej witryny sieci Web.</span><span class="sxs-lookup"><span data-stu-id="fc507-335">The file **~/Views/Shared/\_layout.cshtml** allows you to setup a template for common HTML to use across the entire website.</span></span> <span data-ttu-id="fc507-336">W tym zadaniu doda układ strony wzorcowej przy użyciu wspólnej nagłówka z łączami do obszar strony głównej i magazynu.</span><span class="sxs-lookup"><span data-stu-id="fc507-336">In this task you will add a layout master page with a common header with links to the Home page and Store area.</span></span>

1. <span data-ttu-id="fc507-337">Jeśli nie już otwarty, uruchom **VS Express for Web**.</span><span class="sxs-lookup"><span data-stu-id="fc507-337">If not already open, start **VS Express for Web**.</span></span>
2. <span data-ttu-id="fc507-338">W **pliku** menu, wybierz **Otwórz projekt**.</span><span class="sxs-lookup"><span data-stu-id="fc507-338">In the **File** menu, choose **Open Project**.</span></span> <span data-ttu-id="fc507-339">W oknie dialogowym Otwórz projekt, przejdź do **Source\Ex04 CreatingAView\Begin**, wybierz pozycję **Begin.sln** i kliknij przycisk **Otwórz**.</span><span class="sxs-lookup"><span data-stu-id="fc507-339">In the Open Project dialog, browse to **Source\Ex04-CreatingAView\Begin**, select **Begin.sln** and click **Open**.</span></span> <span data-ttu-id="fc507-340">Alternatywnie możesz nadal z rozwiązaniem uzyskany po zakończeniu pracy w poprzednim ćwiczeniu.</span><span class="sxs-lookup"><span data-stu-id="fc507-340">Alternatively, you may continue with the solution that you obtained after completing the previous exercise.</span></span>

   1. <span data-ttu-id="fc507-341">Po otwarciu dostarczonych **rozpocząć** rozwiązania, musisz pobrać niektórych brakujących pakietów NuGet aby kontynuować.</span><span class="sxs-lookup"><span data-stu-id="fc507-341">If you opened the provided **Begin** solution, you will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="fc507-342">Aby to zrobić, kliknij przycisk **projektu** menu i wybierz **Zarządzaj pakietami NuGet**.</span><span class="sxs-lookup"><span data-stu-id="fc507-342">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
   2. <span data-ttu-id="fc507-343">W **Zarządzaj pakietami NuGet** okna dialogowego, kliknij przycisk **przywrócić** celu pobieranie brakujących pakietów.</span><span class="sxs-lookup"><span data-stu-id="fc507-343">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
   3. <span data-ttu-id="fc507-344">Na koniec Skompiluj rozwiązanie, klikając **kompilacji** | **Kompiluj rozwiązanie**.</span><span class="sxs-lookup"><span data-stu-id="fc507-344">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

      > [!NOTE]
      > <span data-ttu-id="fc507-345">Jedną z zalet przy użyciu narzędzia NuGet jest, że nie masz do wysłania wszystkich bibliotek w projekcie, zmniejszenie jego rozmiar projektu.</span><span class="sxs-lookup"><span data-stu-id="fc507-345">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="fc507-346">Narzędzia Power NuGet określając wersje pakietów w pliku Packages.config, będzie można pobrać wymaganych bibliotek podczas pierwszego uruchomienia projektu.</span><span class="sxs-lookup"><span data-stu-id="fc507-346">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="fc507-347">Jest to, dlaczego konieczne będzie wykonanie tych kroków, po otwarciu istniejącego rozwiązania z tego laboratorium.</span><span class="sxs-lookup"><span data-stu-id="fc507-347">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
3. <span data-ttu-id="fc507-348">Plik  <strong>\_layout.cshtml</strong> zawiera układ kontenera HTML dla wszystkich stron w witrynie.</span><span class="sxs-lookup"><span data-stu-id="fc507-348">The file <strong>\_layout.cshtml</strong> contains the HTML container layout for all pages on the site.</span></span> <span data-ttu-id="fc507-349">Obejmuje on <strong>&lt;html&gt;</strong> elementu w odpowiedzi HTML, jak również <strong>&lt;head&gt;</strong> i <strong>&lt;treści&gt;</strong> elementów.</span><span class="sxs-lookup"><span data-stu-id="fc507-349">It includes the <strong>&lt;html&gt;</strong> element for the HTML response, as well as the <strong>&lt;head&gt;</strong> and <strong>&lt;body&gt;</strong> elements.</span></span> <span data-ttu-id="fc507-350"><strong>@RenderBody()</strong> w kodzie HTML treści identyfikowania regionów tego widoku szablonów będzie można wypełnić się przy użyciu zawartości dynamicznej.</span><span class="sxs-lookup"><span data-stu-id="fc507-350"><strong>@RenderBody()</strong> within the HTML body identify regions that view templates will be able to fill in with dynamic content.</span></span>
   <span data-ttu-id="fc507-351">(C#)</span><span class="sxs-lookup"><span data-stu-id="fc507-351">(C#)</span></span>

    [!code-cshtml[Main](aspnet-mvc-4-fundamentals/samples/sample6.cshtml)]
4. <span data-ttu-id="fc507-352">Dodanie nagłówka wspólnego z łączami do obszar strony głównej i zapisać na wszystkich stron w witrynie.</span><span class="sxs-lookup"><span data-stu-id="fc507-352">Add a common header with links to the Home page and Store area on all pages in the site.</span></span> <span data-ttu-id="fc507-353">Aby to zrobić, Dodaj następujący kod poniżej &lt;treści&gt; instrukcji.</span><span class="sxs-lookup"><span data-stu-id="fc507-353">In order to do that, add the following code below &lt;body&gt; statement.</span></span>
   <span data-ttu-id="fc507-354">(C#)</span><span class="sxs-lookup"><span data-stu-id="fc507-354">(C#)</span></span>

    [!code-cshtml[Main](aspnet-mvc-4-fundamentals/samples/sample7.cshtml)]
5. <span data-ttu-id="fc507-355">Obejmują div renderowanie części treści każdej strony.</span><span class="sxs-lookup"><span data-stu-id="fc507-355">Include a div to render the body section of each page.</span></span> <span data-ttu-id="fc507-356">Zastąp  <strong>@RenderBody()</strong> następującym kodem higlighted: (C#)</span><span class="sxs-lookup"><span data-stu-id="fc507-356">Replace <strong>@RenderBody()</strong> with the following higlighted code: (C#)</span></span>

    [!code-cshtml[Main](aspnet-mvc-4-fundamentals/samples/sample8.cshtml)]

    > [!NOTE]
    > <span data-ttu-id="fc507-357">Czy wiesz?</span><span class="sxs-lookup"><span data-stu-id="fc507-357">Did you know?</span></span> <span data-ttu-id="fc507-358">Program Visual Studio 2012 ma fragmentów, które ułatwiają dodawania często używane kodu HTML i pliki kodu!</span><span class="sxs-lookup"><span data-stu-id="fc507-358">Visual Studio 2012 has snippets that make it easy to add commonly used code in HTML, code files and more!</span></span> <span data-ttu-id="fc507-359">Wypróbuj limit, wpisując **&lt;div&gt;** i naciskając klawisz **kartę** dwa razy, aby wstawić pełnego **div** tagu.</span><span class="sxs-lookup"><span data-stu-id="fc507-359">Try it out by typing **&lt;div&gt;** and pressing **TAB** twice to insert a complete **div** tag.</span></span>

<a id="Ex4Task2"></a>

<a id="Task_2_-_Adding_CSS_Stylesheet"></a>
#### <a name="task-2---adding-css-stylesheet"></a><span data-ttu-id="fc507-360">Zadanie 2 — Dodawanie arkusza stylów CSS</span><span class="sxs-lookup"><span data-stu-id="fc507-360">Task 2 - Adding CSS Stylesheet</span></span>

<span data-ttu-id="fc507-361">Pusty szablon projektu zawiera plik CSS bardzo prostsze, zawierający tylko stylów używany do wyświetlania podstawowych formularzy i komunikatów dotyczących sprawdzania poprawności.</span><span class="sxs-lookup"><span data-stu-id="fc507-361">The empty project template includes a very streamlined CSS file which just includes styles used to display basic forms and validation messages.</span></span> <span data-ttu-id="fc507-362">Aby poprawić wygląd i działanie witryny użyje dodatkowe CSS i obrazów (potencjalnie udostępnione przez projektanta).</span><span class="sxs-lookup"><span data-stu-id="fc507-362">You will use additional CSS and images (potentially provided by a designer) in order to enhance the look and feel of the site.</span></span>

<span data-ttu-id="fc507-363">To zadanie spowoduje dodanie arkusz stylów CSS do definiowania stylów witryny.</span><span class="sxs-lookup"><span data-stu-id="fc507-363">In this task, you will add a CSS stylesheet to define the styles of the site.</span></span>

1. <span data-ttu-id="fc507-364">Pliku CSS i obrazy są uwzględnione w **Source\Assets\Content** folder tego laboratorium.</span><span class="sxs-lookup"><span data-stu-id="fc507-364">The CSS file and images are included in the **Source\Assets\Content** folder of this Lab.</span></span> <span data-ttu-id="fc507-365">Aby dodać je do aplikacji, przeciągnij ich zawartość z **Eksploratora Windows** okna do **Eksploratora rozwiązań** w programie Visual Studio Express for Web, jak pokazano poniżej:</span><span class="sxs-lookup"><span data-stu-id="fc507-365">In order to add them to the application, drag their content from a **Windows Explorer** window into the **Solution Explorer** in Visual Studio Express for Web, as shown below:</span></span>

    <span data-ttu-id="fc507-366">![Przeciąganie styl treści](aspnet-mvc-4-fundamentals/_static/image12.png "przeciąganie styl treści")</span><span class="sxs-lookup"><span data-stu-id="fc507-366">![Dragging style contents](aspnet-mvc-4-fundamentals/_static/image12.png "Dragging style contents")</span></span>

    <span data-ttu-id="fc507-367">*Przeciąganie styl treści*</span><span class="sxs-lookup"><span data-stu-id="fc507-367">*Dragging style contents*</span></span>
2. <span data-ttu-id="fc507-368">Okno dialogowe z ostrzeżeniem pojawi się pytaniem o potwierdzenie zastąpić **Site.css** plików i niektórych istniejących obrazów.</span><span class="sxs-lookup"><span data-stu-id="fc507-368">A warning dialog will appear, asking for confirmation to replace **Site.css** file and some existing images.</span></span> <span data-ttu-id="fc507-369">Sprawdź **Zastosuj do wszystkich elementów** i kliknij przycisk **tak**.</span><span class="sxs-lookup"><span data-stu-id="fc507-369">Check **Apply to all items** and click **Yes**.</span></span>

<a id="Ex4Task3"></a>

<a id="Task_3_-_Adding_a_View_Template"></a>
#### <a name="task-3---adding-a-view-template"></a><span data-ttu-id="fc507-370">Zadanie 3. Dodawanie wyświetlanie szablonu</span><span class="sxs-lookup"><span data-stu-id="fc507-370">Task 3 - Adding a View Template</span></span>

<span data-ttu-id="fc507-371">To zadanie spowoduje dodanie szablonu widoku do generowania odpowiedzi HTML, który będzie używany przez układ strony wzorcowej i CSS dodane w tym ćwiczeniu.</span><span class="sxs-lookup"><span data-stu-id="fc507-371">In this task, you will add a View template to generate the HTML response that will use the layout master page and CSS added in this exercise.</span></span>

1. <span data-ttu-id="fc507-372">Aby użyć szablonu widoku podczas przeglądania strony głównej, najpierw musisz wskazują, że zamiast zwracać ciąg, **indeksu HomeController** metoda zwróci **widoku**.</span><span class="sxs-lookup"><span data-stu-id="fc507-372">To use a View template when browsing the site's home page, you will first need to indicate that instead of returning a string, the **HomeController Index** method will return a **View**.</span></span> <span data-ttu-id="fc507-373">Otwórz **HomeController** klasy i zmień jego **indeksu** metodę, aby zwrócić **ActionResult**, i zwróć **View()**.</span><span class="sxs-lookup"><span data-stu-id="fc507-373">Open **HomeController** class and change its **Index** method to return an **ActionResult**, and have it return **View()**.</span></span>

    <span data-ttu-id="fc507-374">(Fragment - kodu *podstawy platformy ASP.NET MVC 4 - indeksu HomeController Ex4*)</span><span class="sxs-lookup"><span data-stu-id="fc507-374">(Code Snippet - *ASP.NET MVC 4 Fundamentals - Ex4 HomeController Index*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample9.cs)]
2. <span data-ttu-id="fc507-375">Teraz musisz dodać odpowiedni szablon widoku.</span><span class="sxs-lookup"><span data-stu-id="fc507-375">Now, you need to add an appropriate View template.</span></span> <span data-ttu-id="fc507-376">Aby to zrobić, **kliknij prawym przyciskiem myszy** wewnątrz **indeksu** metody akcji i wybierz **Dodaj widok**.</span><span class="sxs-lookup"><span data-stu-id="fc507-376">To do this, **right-click** inside the **Index** action method and select **Add View**.</span></span> <span data-ttu-id="fc507-377">Zostanie wyświetlone okno **Dodaj widok** okna dialogowego.</span><span class="sxs-lookup"><span data-stu-id="fc507-377">This will bring up the **Add View** dialog.</span></span>

    <span data-ttu-id="fc507-378">![Dodawanie widoku z w metodzie indeksu](aspnet-mvc-4-fundamentals/_static/image13.png "Dodawanie widoku z wewnątrz Index — metoda")</span><span class="sxs-lookup"><span data-stu-id="fc507-378">![Adding a View from within the Index method](aspnet-mvc-4-fundamentals/_static/image13.png "Adding a View from within the Index method")</span></span>

    <span data-ttu-id="fc507-379">*Dodawanie widoku z wewnątrz Index — metoda*</span><span class="sxs-lookup"><span data-stu-id="fc507-379">*Adding a View from within the Index method*</span></span>
3. <span data-ttu-id="fc507-380">**Dodaj widok** wyświetli się okno dialogowe, aby wygenerować plik szablonu widoku.</span><span class="sxs-lookup"><span data-stu-id="fc507-380">The **Add View** Dialog will appear to generate a View template file.</span></span> <span data-ttu-id="fc507-381">Domyślnie to okno dialogowe wstępnie wypełnia nazwę szablonu widoku, aby odpowiadały one metody akcji, która będzie go używać.</span><span class="sxs-lookup"><span data-stu-id="fc507-381">By default, this dialog pre-populates the name of the View template so that it matches the action method that will use it.</span></span> <span data-ttu-id="fc507-382">Ponieważ użyto **Dodaj widok** menu kontekstowego w **indeksu** metody akcji w obrębie HomeController **Dodaj widok** okno dialogowe ma indeks jako domyślną nazwę widoku.</span><span class="sxs-lookup"><span data-stu-id="fc507-382">Because you used the **Add View** context menu within the **Index** action method within the HomeController, the **Add View** dialog has Index as the default view name.</span></span> <span data-ttu-id="fc507-383">Kliknij przycisk **Dodaj**.</span><span class="sxs-lookup"><span data-stu-id="fc507-383">Click **Add**.</span></span>

    <span data-ttu-id="fc507-384">![Okno dialogowe dodawania widoku](aspnet-mvc-4-fundamentals/_static/image14.png "okno dialogowe dodawania widoku")</span><span class="sxs-lookup"><span data-stu-id="fc507-384">![Add View Dialog](aspnet-mvc-4-fundamentals/_static/image14.png "Add View Dialog")</span></span>

    <span data-ttu-id="fc507-385">*Okno dialogowe dodawania widoku*</span><span class="sxs-lookup"><span data-stu-id="fc507-385">*Add View Dialog*</span></span>
4. <span data-ttu-id="fc507-386">Visual Studio generuje **Index.cshtml** Wyświetl szablon wewnątrz **Views\Home** folder, a następnie go otwiera.</span><span class="sxs-lookup"><span data-stu-id="fc507-386">Visual Studio generates an **Index.cshtml** view template inside the **Views\Home** folder and then opens it.</span></span>

    <span data-ttu-id="fc507-387">![Strona główna widok indeksu utworzony](aspnet-mvc-4-fundamentals/_static/image15.png "widok Home Indeks utworzony")</span><span class="sxs-lookup"><span data-stu-id="fc507-387">![Home Index view created](aspnet-mvc-4-fundamentals/_static/image15.png "Home Index view created")</span></span>

    <span data-ttu-id="fc507-388">*Macierzysty widok indeksu utworzony*</span><span class="sxs-lookup"><span data-stu-id="fc507-388">*Home Index view created*</span></span>

    > [!NOTE]
    > <span data-ttu-id="fc507-389">Nazwa i lokalizacja **Index.cshtml** plik ma zastosowanie i jest zgodna z domyślnych konwencji nazewnictwa platformy ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="fc507-389">name and location of the **Index.cshtml** file is relevant and follows the default ASP.NET MVC naming conventions.</span></span>
    > 
    > <span data-ttu-id="fc507-390">Folder \Views\**Home** zgodna z nazwą kontrolera (**Home** kontrolera).</span><span class="sxs-lookup"><span data-stu-id="fc507-390">The folder \Views\**Home** matches the controller name (**Home** Controller).</span></span> <span data-ttu-id="fc507-391">Nazwa szablonu widoku (**indeksu**), metoda akcji kontrolera, które będą wyświetlane w widoku jest zgodna.</span><span class="sxs-lookup"><span data-stu-id="fc507-391">The View template name (**Index**), matches the controller action method which will be displaying the View.</span></span>
    > 
    > <span data-ttu-id="fc507-392">W ten sposób ASP.NET MVC pozwala uniknąć konieczności jawnego określania nazwy lub lokalizacji szablonu widoku, używając tę konwencję nazewnictwa do zwrócenia widoku.</span><span class="sxs-lookup"><span data-stu-id="fc507-392">This way, ASP.NET MVC avoids having to explicitly specify the name or location of a View template when using this naming convention to return a View.</span></span>
5. <span data-ttu-id="fc507-393">Wygenerowany szablon widoku jest oparty na  **\_layout.cshtml** wcześniej zdefiniowanego szablonu.</span><span class="sxs-lookup"><span data-stu-id="fc507-393">The generated View template is based on the **\_layout.cshtml** template earlier defined.</span></span> <span data-ttu-id="fc507-394">Zaktualizuj właściwość ViewBag.Title do **Home**i zmień głównej zawartości do **to strona główna**, jak pokazano w poniższym kodzie:</span><span class="sxs-lookup"><span data-stu-id="fc507-394">Update the ViewBag.Title property to **Home**, and change the main content to **This is the Home Page**, as shown in the code below:</span></span>

    [!code-cshtml[Main](aspnet-mvc-4-fundamentals/samples/sample10.cshtml)]
6. <span data-ttu-id="fc507-395">Wybierz **MvcMusicStore** projekt w Eksploratorze rozwiązań i naciśnij klawisz **F5** do uruchomienia aplikacji.</span><span class="sxs-lookup"><span data-stu-id="fc507-395">Select **MvcMusicStore** project in the Solution Explorer and Press **F5** to run the Application.</span></span>

<a id="Ex4Task4"></a>

<a id="Task_4_Verification"></a>
#### <a name="task-4-verification"></a><span data-ttu-id="fc507-396">Zadanie 4: Weryfikacja</span><span class="sxs-lookup"><span data-stu-id="fc507-396">Task 4: Verification</span></span>

<span data-ttu-id="fc507-397">Aby sprawdzić, prawidłowo wykonane wszystkie kroki opisane w poprzednim ćwiczeniu, wykonać następujące czynności:</span><span class="sxs-lookup"><span data-stu-id="fc507-397">In order to verify that you have correctly performed all the steps in the previous exercise, proceed as follows:</span></span>

<span data-ttu-id="fc507-398">Z tą aplikacją otwarta w przeglądarce należy zauważyć, że:</span><span class="sxs-lookup"><span data-stu-id="fc507-398">With the application opened in a browser, you should note that:</span></span>

1. <span data-ttu-id="fc507-399">Metody akcji indeksu HomeController znaleźć i wyświetlić **\Views\Home\Index.cshtml** wyświetlić szablon, nawet jeśli kod wywołuje **zwracać View()**, ponieważ szablon widoku, a następnie standardowej konwencji nazewnictwa.</span><span class="sxs-lookup"><span data-stu-id="fc507-399">The HomeController's Index action method found and displayed the **\Views\Home\Index.cshtml** View template, even though the code called **return View()**, because the View template followed the standard naming convention.</span></span>
2. <span data-ttu-id="fc507-400">Strona główna wyświetla komunikat powitalny, zdefiniowanego w **\Views\Home\Index.cshtml** widok szablonu.</span><span class="sxs-lookup"><span data-stu-id="fc507-400">The Home Page displays the welcome message defined within the **\Views\Home\Index.cshtml** view template.</span></span>
3. <span data-ttu-id="fc507-401">Strona główna używa  **\_layout.cshtml** szablonu, co komunikat powitalny jest zawarty w układu standardowe witryny HTML.</span><span class="sxs-lookup"><span data-stu-id="fc507-401">The Home Page is using the **\_layout.cshtml** template, and so the welcome message is contained within the standard site HTML layout.</span></span>

    <span data-ttu-id="fc507-402">![Strona główna widoku indeksu przy użyciu zdefiniowanych LayoutPage i styl](aspnet-mvc-4-fundamentals/_static/image16.png "Home widoku indeksu przy użyciu zdefiniowanych LayoutPage i stylu")</span><span class="sxs-lookup"><span data-stu-id="fc507-402">![Home Index View using the defined LayoutPage and style](aspnet-mvc-4-fundamentals/_static/image16.png "Home Index View using the defined LayoutPage and style")</span></span>

    <span data-ttu-id="fc507-403">*Macierzysty widoku indeksu przy użyciu zdefiniowanych LayoutPage i stylu*</span><span class="sxs-lookup"><span data-stu-id="fc507-403">*Home Index View using the defined LayoutPage and style*</span></span>

<a id="Exercise5"></a>

<a id="Exercise_5_Creating_a_View_Model"></a>
### <a name="exercise-5-creating-a-view-model"></a><span data-ttu-id="fc507-404">Ćwiczenie 5: Tworzenie modelu widoku</span><span class="sxs-lookup"><span data-stu-id="fc507-404">Exercise 5: Creating a View Model</span></span>

<span data-ttu-id="fc507-405">Do tej pory wprowadzono widoków wyświetlić zapisane na stałe HTML, ale, aby można było utworzyć dynamicznych aplikacji sieci web, Wyświetl szablon powinien zostać wyświetlony informacji od kontrolera.</span><span class="sxs-lookup"><span data-stu-id="fc507-405">So far, you made your Views display hardcoded HTML, but, in order to create dynamic web applications, the View template should receive information from the Controller.</span></span> <span data-ttu-id="fc507-406">Jeden technika wspólnego do użycia w tym celu jest **ViewModel** wzorzec, dzięki czemu kontrolera spakować wszystkie informacje potrzebne do generowania odpowiednich odpowiedzi HTML.</span><span class="sxs-lookup"><span data-stu-id="fc507-406">One common technique to be used for that purpose is the **ViewModel** pattern, which allows a Controller to package up all the information needed to generate the appropriate HTML response.</span></span>

<span data-ttu-id="fc507-407">W tym ćwiczeniu dowiesz Tworzenie klasy ViewModel i dodać wymagane właściwości: numer gatunkami muzyki w magazynie i listę tych gatunkami muzyki.</span><span class="sxs-lookup"><span data-stu-id="fc507-407">In this exercise, you will learn how to create a ViewModel class and add the required properties: the number of genres in the store and a list of those genres.</span></span> <span data-ttu-id="fc507-408">Zostanie również zaktualizować StoreController do użycia ViewModel utworzony, a koniec utworzysz nowy szablon widoku, który spowoduje wyświetlenie właściwości wymienione na stronie.</span><span class="sxs-lookup"><span data-stu-id="fc507-408">You will also update the StoreController to use the created ViewModel, and finally, you will create a new View template that will display the mentioned properties in the page.</span></span>

<a id="Ex5Task1"></a>

<a id="Task_1_-_Creating_a_ViewModel_Class"></a>
#### <a name="task-1---creating-a-viewmodel-class"></a><span data-ttu-id="fc507-409">Zadanie 1 — Tworzenie klasy ViewModel</span><span class="sxs-lookup"><span data-stu-id="fc507-409">Task 1 - Creating a ViewModel Class</span></span>

<span data-ttu-id="fc507-410">W ramach tego zadania spowoduje utworzenie klasy ViewModel, która będzie wdrożyć scenariusz dla listy genre magazynu.</span><span class="sxs-lookup"><span data-stu-id="fc507-410">In this task, you will create a ViewModel class that will implement the Store genre listing scenario.</span></span>

1. <span data-ttu-id="fc507-411">Jeśli nie już otwarty, uruchom **VS Express for Web**.</span><span class="sxs-lookup"><span data-stu-id="fc507-411">If not already open, start **VS Express for Web**.</span></span>
2. <span data-ttu-id="fc507-412">W **pliku** menu, wybierz **Otwórz projekt**.</span><span class="sxs-lookup"><span data-stu-id="fc507-412">In the **File** menu, choose **Open Project**.</span></span> <span data-ttu-id="fc507-413">W oknie dialogowym Otwórz projekt, przejdź do **Source\Ex05 CreatingAViewModel\Begin**, wybierz pozycję **Begin.sln** i kliknij przycisk **Otwórz**.</span><span class="sxs-lookup"><span data-stu-id="fc507-413">In the Open Project dialog, browse to **Source\Ex05-CreatingAViewModel\Begin**, select **Begin.sln** and click **Open**.</span></span> <span data-ttu-id="fc507-414">Alternatywnie możesz nadal z rozwiązaniem uzyskany po zakończeniu pracy w poprzednim ćwiczeniu.</span><span class="sxs-lookup"><span data-stu-id="fc507-414">Alternatively, you may continue with the solution that you obtained after completing the previous exercise.</span></span>

   1. <span data-ttu-id="fc507-415">Po otwarciu dostarczonych **rozpocząć** rozwiązania, musisz pobrać niektórych brakujących pakietów NuGet aby kontynuować.</span><span class="sxs-lookup"><span data-stu-id="fc507-415">If you opened the provided **Begin** solution, you will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="fc507-416">Aby to zrobić, kliknij przycisk **projektu** menu i wybierz **Zarządzaj pakietami NuGet**.</span><span class="sxs-lookup"><span data-stu-id="fc507-416">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
   2. <span data-ttu-id="fc507-417">W **Zarządzaj pakietami NuGet** okna dialogowego, kliknij przycisk **przywrócić** celu pobieranie brakujących pakietów.</span><span class="sxs-lookup"><span data-stu-id="fc507-417">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
   3. <span data-ttu-id="fc507-418">Na koniec Skompiluj rozwiązanie, klikając **kompilacji** | **Kompiluj rozwiązanie**.</span><span class="sxs-lookup"><span data-stu-id="fc507-418">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

      > [!NOTE]
      > <span data-ttu-id="fc507-419">Jedną z zalet przy użyciu narzędzia NuGet jest, że nie masz do wysłania wszystkich bibliotek w projekcie, zmniejszenie jego rozmiar projektu.</span><span class="sxs-lookup"><span data-stu-id="fc507-419">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="fc507-420">Narzędzia Power NuGet określając wersje pakietów w pliku Packages.config, będzie można pobrać wymaganych bibliotek podczas pierwszego uruchomienia projektu.</span><span class="sxs-lookup"><span data-stu-id="fc507-420">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="fc507-421">Jest to, dlaczego konieczne będzie wykonanie tych kroków, po otwarciu istniejącego rozwiązania z tego laboratorium.</span><span class="sxs-lookup"><span data-stu-id="fc507-421">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
3. <span data-ttu-id="fc507-422">Utwórz **ViewModels** folder do przechowywania ViewModel.</span><span class="sxs-lookup"><span data-stu-id="fc507-422">Create a **ViewModels** folder to hold the ViewModel.</span></span> <span data-ttu-id="fc507-423">Aby to zrobić, kliknij prawym przyciskiem myszy najwyższego poziomu **MvcMusicStore** projektu, zaznacz **Dodaj** , a następnie **nowy Folder**.</span><span class="sxs-lookup"><span data-stu-id="fc507-423">To do this, right-click the top-level **MvcMusicStore** project, select **Add** and then **New Folder**.</span></span>

    <span data-ttu-id="fc507-424">![Dodawanie nowego folderu](aspnet-mvc-4-fundamentals/_static/image17.png "dodawanie nowy folder")</span><span class="sxs-lookup"><span data-stu-id="fc507-424">![Adding a new folder](aspnet-mvc-4-fundamentals/_static/image17.png "Adding a new folder")</span></span>

    <span data-ttu-id="fc507-425">*Dodawanie nowego folderu*</span><span class="sxs-lookup"><span data-stu-id="fc507-425">*Adding a new folder*</span></span>
4. <span data-ttu-id="fc507-426">Nazwa folderu *ViewModels*.</span><span class="sxs-lookup"><span data-stu-id="fc507-426">Name the folder *ViewModels*.</span></span>

    <span data-ttu-id="fc507-427">![ViewModels folder w Eksploratorze rozwiązań](aspnet-mvc-4-fundamentals/_static/image18.png "ViewModels folder w Eksploratorze rozwiązań")</span><span class="sxs-lookup"><span data-stu-id="fc507-427">![ViewModels folder in Solution Explorer](aspnet-mvc-4-fundamentals/_static/image18.png "ViewModels folder in Solution Explorer")</span></span>

    <span data-ttu-id="fc507-428">*ViewModels folder w Eksploratorze rozwiązań*</span><span class="sxs-lookup"><span data-stu-id="fc507-428">*ViewModels folder in Solution Explorer*</span></span>
5. <span data-ttu-id="fc507-429">Utwórz **ViewModel** klasy.</span><span class="sxs-lookup"><span data-stu-id="fc507-429">Create a **ViewModel** class.</span></span> <span data-ttu-id="fc507-430">Aby to zrobić, kliknij prawym przyciskiem myszy **ViewModels** folderu niedawno utworzona, wybierz **Dodaj** , a następnie **nowy element**.</span><span class="sxs-lookup"><span data-stu-id="fc507-430">To do this, right-click on the **ViewModels** folder recently created, select **Add** and then **New Item**.</span></span> <span data-ttu-id="fc507-431">W obszarze **kod**, wybierz **klasy** elementu i nazwij plik *StoreIndexViewModel.cs*, następnie kliknij przycisk **Dodaj**.</span><span class="sxs-lookup"><span data-stu-id="fc507-431">Under **Code**, choose the **Class** item and name the file *StoreIndexViewModel.cs*, then click **Add**.</span></span>

    <span data-ttu-id="fc507-432">![Dodawanie nowej klasy](aspnet-mvc-4-fundamentals/_static/image19.png "Dodawanie nowej klasy")</span><span class="sxs-lookup"><span data-stu-id="fc507-432">![Adding a new Class](aspnet-mvc-4-fundamentals/_static/image19.png "Adding a new Class")</span></span>

    <span data-ttu-id="fc507-433">*Dodawanie nowej klasy*</span><span class="sxs-lookup"><span data-stu-id="fc507-433">*Adding a new Class*</span></span>

    <span data-ttu-id="fc507-434">![Tworzenie klasy StoreIndexViewModel](aspnet-mvc-4-fundamentals/_static/image20.png "StoreIndexViewModel Tworzenie klasy")</span><span class="sxs-lookup"><span data-stu-id="fc507-434">![Creating StoreIndexViewModel class](aspnet-mvc-4-fundamentals/_static/image20.png "Creating StoreIndexViewModel class")</span></span>

    <span data-ttu-id="fc507-435">*Tworzenie klasy StoreIndexViewModel*</span><span class="sxs-lookup"><span data-stu-id="fc507-435">*Creating StoreIndexViewModel class*</span></span>

<a id="Ex5Task2"></a>

<a id="Task_2_-_Adding_Properties_to_the_ViewModel_class"></a>
#### <a name="task-2---adding-properties-to-the-viewmodel-class"></a><span data-ttu-id="fc507-436">Zadanie 2 — Dodawanie właściwości do klasy ViewModel</span><span class="sxs-lookup"><span data-stu-id="fc507-436">Task 2 - Adding Properties to the ViewModel class</span></span>

<span data-ttu-id="fc507-437">Istnieją dwa parametry do przekazania z StoreController do szablonu widoku w celu wygenerowania oczekiwanej odpowiedzi HTML: numer gatunkami muzyki w magazynie i listę tych gatunkami muzyki.</span><span class="sxs-lookup"><span data-stu-id="fc507-437">There are two parameters to be passed from the StoreController to the View template in order to generate the expected HTML response: the number of genres in the store and a list of those genres.</span></span>

<span data-ttu-id="fc507-438">To zadanie spowoduje dodanie tych właściwości 2 w celu **StoreIndexViewModel** klasy: **NumberOfGenres** (liczba całkowita) i **Genres** (lista ciągów).</span><span class="sxs-lookup"><span data-stu-id="fc507-438">In this task, you will add those 2 properties to the **StoreIndexViewModel** class: **NumberOfGenres** (an integer) and **Genres** (a list of strings).</span></span>

1. <span data-ttu-id="fc507-439">Dodaj **NumberOfGenres** i **Genres** właściwości **StoreIndexViewModel** klasy.</span><span class="sxs-lookup"><span data-stu-id="fc507-439">Add **NumberOfGenres** and **Genres** properties to the **StoreIndexViewModel** class.</span></span> <span data-ttu-id="fc507-440">Aby to zrobić, Dodaj następujące wiersze 2 do definicji klasy:</span><span class="sxs-lookup"><span data-stu-id="fc507-440">To do this, add the following 2 lines to the class definition:</span></span>

    <span data-ttu-id="fc507-441">(Fragment - kodu *ASP.NET MVC 4 podstawy — właściwości Ex5 StoreIndexViewModel*)</span><span class="sxs-lookup"><span data-stu-id="fc507-441">(Code Snippet - *ASP.NET MVC 4 Fundamentals - Ex5 StoreIndexViewModel properties*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample11.cs)]

> [!NOTE]
> <span data-ttu-id="fc507-442">**{Get; Ustaw;}**  notacji korzysta z C# w funkcji właściwości zaimplementowane automatycznie.</span><span class="sxs-lookup"><span data-stu-id="fc507-442">The **{ get; set; }** notation makes use of C#'s auto-implemented properties feature.</span></span> <span data-ttu-id="fc507-443">Zapewnia korzyści właściwości bez konieczności nam zadeklarować polem zapasowym.</span><span class="sxs-lookup"><span data-stu-id="fc507-443">It provides the benefits of a property without requiring us to declare a backing field.</span></span>

<a id="Ex5Task3"></a>

<a id="Task_3_-_Updating_StoreController_to_use_the_StoreIndexViewModel"></a>
#### <a name="task-3---updating-storecontroller-to-use-the-storeindexviewmodel"></a><span data-ttu-id="fc507-444">Zadanie 3 — aktualizowanie StoreController do użycia StoreIndexViewModel</span><span class="sxs-lookup"><span data-stu-id="fc507-444">Task 3 - Updating StoreController to use the StoreIndexViewModel</span></span>

<span data-ttu-id="fc507-445">**StoreIndexViewModel** klasa hermetyzuje informacje wymagane do przekazania z **StoreController**w **indeksu** metodę szablonu widoku w celu wygenerowania odpowiedzi .</span><span class="sxs-lookup"><span data-stu-id="fc507-445">The **StoreIndexViewModel** class encapsulates the information needed to pass from **StoreController**'s **Index** method to a View template in order to generate a response.</span></span>

<span data-ttu-id="fc507-446">To zadanie zaktualizuje **StoreController** do używania **StoreIndexViewModel**.</span><span class="sxs-lookup"><span data-stu-id="fc507-446">In this task, you will update the **StoreController** to use the **StoreIndexViewModel**.</span></span>

1. <span data-ttu-id="fc507-447">Otwórz **StoreController** klasy.</span><span class="sxs-lookup"><span data-stu-id="fc507-447">Open **StoreController** class.</span></span>

    <span data-ttu-id="fc507-448">![Otwieranie klasy StoreController](aspnet-mvc-4-fundamentals/_static/image21.png "StoreController otwierania — klasa")</span><span class="sxs-lookup"><span data-stu-id="fc507-448">![Opening StoreController class](aspnet-mvc-4-fundamentals/_static/image21.png "Opening StoreController class")</span></span>

    <span data-ttu-id="fc507-449">*Otwieranie StoreController klasy*</span><span class="sxs-lookup"><span data-stu-id="fc507-449">*Opening StoreController class*</span></span>
2. <span data-ttu-id="fc507-450">Aby można było używać **StoreIndexViewModel** klasę z **StoreController**, Dodaj następujący obszar nazw w górnej części **StoreController** kodu:</span><span class="sxs-lookup"><span data-stu-id="fc507-450">In order to use the **StoreIndexViewModel** class from the **StoreController**, add the following namespace at the top of the **StoreController** code:</span></span>

    <span data-ttu-id="fc507-451">(Fragment - kodu *ASP.NET MVC 4 podstawy — za pomocą ViewModels StoreIndexViewModel Ex5*)</span><span class="sxs-lookup"><span data-stu-id="fc507-451">(Code Snippet - *ASP.NET MVC 4 Fundamentals - Ex5 StoreIndexViewModel using ViewModels*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample12.cs)]
3. <span data-ttu-id="fc507-452">Zmień **StoreController**w **indeksu** metody akcji, którą tworzy i wypełnia **StoreIndexViewModel** obiektu i przekazuje ją do szablonu widoku w celu Generowanie odpowiedzi HTML z nim.</span><span class="sxs-lookup"><span data-stu-id="fc507-452">Change the **StoreController**'s **Index** action method so that it creates and populates a **StoreIndexViewModel** object and then passes it off to a View template to generate an HTML response with it.</span></span>

    > [!NOTE]
    > <span data-ttu-id="fc507-453">W laboratorium &quot;ASP.NET MVC modeli i dostęp do danych&quot; będzie pisania kodu, który pobiera listę gatunkami muzyki magazynu z bazy danych.</span><span class="sxs-lookup"><span data-stu-id="fc507-453">In Lab &quot;ASP.NET MVC Models and Data Access&quot; you will write code that retrieves the list of store genres from a database.</span></span> <span data-ttu-id="fc507-454">W poniższym kodzie utworzysz **listy** gatunkami muzyki fikcyjny danych, które wypełnia **StoreIndexViewModel**.</span><span class="sxs-lookup"><span data-stu-id="fc507-454">In the following code, you will create a **List** of dummy data genres that will populate the **StoreIndexViewModel**.</span></span>
    > 
    > <span data-ttu-id="fc507-455">Po utworzeniu i konfigurowanie **StoreIndexViewModel** obiektu, zostanie przekazany jako argument **widoku** metody.</span><span class="sxs-lookup"><span data-stu-id="fc507-455">After creating and setting up the **StoreIndexViewModel** object, it will be passed as an argument to the **View** method.</span></span> <span data-ttu-id="fc507-456">Oznacza to, że szablon widoku będzie używać tego obiektu do generowania odpowiedzi HTML z nim.</span><span class="sxs-lookup"><span data-stu-id="fc507-456">This indicates that the View template will use that object to generate an HTML response with it.</span></span>
4. <span data-ttu-id="fc507-457">Zastąp **indeksu** metodę z następującym kodem:</span><span class="sxs-lookup"><span data-stu-id="fc507-457">Replace the **Index** method with the following code:</span></span>

    <span data-ttu-id="fc507-458">(Fragment - kodu *ASP.NET MVC 4 podstawy — metoda indeksu StoreController Ex5*)</span><span class="sxs-lookup"><span data-stu-id="fc507-458">(Code Snippet - *ASP.NET MVC 4 Fundamentals - Ex5 StoreController Index method*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample13.cs)]

> [!NOTE]
> <span data-ttu-id="fc507-459">Jeśli znasz C#, może założyć to przy użyciu **var** oznacza, że **viewModel** zmienna jest późnym wiązaniem.</span><span class="sxs-lookup"><span data-stu-id="fc507-459">If you're unfamiliar with C#, you may assume that using **var** means that the **viewModel** variable is late-bound.</span></span> <span data-ttu-id="fc507-460">Nie jest to poprawna - kompilatora C# używa wnioskowanie typów w oparciu o przypisania do zmiennej do określenia, który **viewModel** jest typu **StoreIndexViewModel**.</span><span class="sxs-lookup"><span data-stu-id="fc507-460">That's not correct - the C# compiler is using type-inference based on what you assign to the variable to determine that **viewModel** is of type **StoreIndexViewModel**.</span></span> <span data-ttu-id="fc507-461">Ponadto uzyskując kompilowanie lokalnej **viewModel** zmiennej jako **StoreIndexViewModel** typ ma sprawdzanie kompilacji get i pomocy technicznej edytora kodu programu Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="fc507-461">Also, by compiling the local **viewModel** variable as a **StoreIndexViewModel** type you get compile-time checking and Visual Studio code-editor support.</span></span>

<a id="Ex5Task4"></a>

<a id="Task_4_-_Creating_a_View_Template_that_Uses_StoreIndexViewModel"></a>
#### <a name="task-4---creating-a-view-template-that-uses-storeindexviewmodel"></a><span data-ttu-id="fc507-462">Zadanie 4 — Tworzenie szablonu widoku, który używa StoreIndexViewModel</span><span class="sxs-lookup"><span data-stu-id="fc507-462">Task 4 - Creating a View Template that Uses StoreIndexViewModel</span></span>

<span data-ttu-id="fc507-463">W ramach tego zadania zostanie utworzony szablon widoku, który będzie używany obiekt StoreIndexViewModel przekazany z kontrolera do wyświetlenia listy gatunkami muzyki.</span><span class="sxs-lookup"><span data-stu-id="fc507-463">In this task, you will create a View template that will use a StoreIndexViewModel object passed from the Controller to display a list of genres.</span></span>

1. <span data-ttu-id="fc507-464">Przed utworzeniem nowego szablonu widoku, utworzymy projektu, aby **dialogowe dodawania widoku** zna **StoreIndexViewModel** klasy.</span><span class="sxs-lookup"><span data-stu-id="fc507-464">Before creating the new View template, let's build the project so that the **Add View Dialog** knows about the **StoreIndexViewModel** class.</span></span> <span data-ttu-id="fc507-465">Skompiluj projekt, wybierając **kompilacji** element menu, a następnie **kompilacji MvcMusicStore**.</span><span class="sxs-lookup"><span data-stu-id="fc507-465">Build the project by selecting the **Build** menu item and then **Build MvcMusicStore**.</span></span>

    <span data-ttu-id="fc507-466">![Tworzenie projektu](aspnet-mvc-4-fundamentals/_static/image22.png "skompilowanie projektu")</span><span class="sxs-lookup"><span data-stu-id="fc507-466">![Building the project](aspnet-mvc-4-fundamentals/_static/image22.png "Building the project")</span></span>

    <span data-ttu-id="fc507-467">*Tworzenie projektu*</span><span class="sxs-lookup"><span data-stu-id="fc507-467">*Building the project*</span></span>
2. <span data-ttu-id="fc507-468">Utwórz nowy szablon widoku.</span><span class="sxs-lookup"><span data-stu-id="fc507-468">Create a new View template.</span></span> <span data-ttu-id="fc507-469">W tym celu kliknij prawym przyciskiem myszy wewnątrz **indeksu** metodę i wybierz **Dodaj widok**.</span><span class="sxs-lookup"><span data-stu-id="fc507-469">To do that, right-click inside the **Index** method and select **Add View**.</span></span>

    <span data-ttu-id="fc507-470">![Dodawanie widoku](aspnet-mvc-4-fundamentals/_static/image23.png "dodawania widoku")</span><span class="sxs-lookup"><span data-stu-id="fc507-470">![Adding a View](aspnet-mvc-4-fundamentals/_static/image23.png "Adding a View")</span></span>

    <span data-ttu-id="fc507-471">*Dodawanie widoku*</span><span class="sxs-lookup"><span data-stu-id="fc507-471">*Adding a View*</span></span>
3. <span data-ttu-id="fc507-472">Ponieważ **dialogowe dodawania widoku** została wywołana z **StoreController**, spowoduje to dodanie Wyświetl szablon domyślny **\Views\Store\Index.cshtml** pliku.</span><span class="sxs-lookup"><span data-stu-id="fc507-472">Because the **Add View Dialog** was invoked from the **StoreController**, it will add the View template by default in a **\Views\Store\Index.cshtml** file.</span></span> <span data-ttu-id="fc507-473">Sprawdź **utworzyć silnie wpisane — widok** pole wyboru, a następnie wybierz **StoreIndexViewModel** jako **klasa modelu**.</span><span class="sxs-lookup"><span data-stu-id="fc507-473">Check the **Create a strongly-typed-view** checkbox and then select **StoreIndexViewModel** as the **Model class**.</span></span> <span data-ttu-id="fc507-474">Ponadto upewnij się, że wybrany aparat widoku jest **Razor**.</span><span class="sxs-lookup"><span data-stu-id="fc507-474">Also, make sure that the View engine selected is **Razor**.</span></span> <span data-ttu-id="fc507-475">Kliknij przycisk **Dodaj**.</span><span class="sxs-lookup"><span data-stu-id="fc507-475">Click **Add**.</span></span>

    <span data-ttu-id="fc507-476">![Okno dialogowe dodawania widoku](aspnet-mvc-4-fundamentals/_static/image24.png "okno dialogowe dodawania widoku")</span><span class="sxs-lookup"><span data-stu-id="fc507-476">![Add View Dialog](aspnet-mvc-4-fundamentals/_static/image24.png "Add View Dialog")</span></span>

    <span data-ttu-id="fc507-477">*Okno dialogowe dodawania widoku*</span><span class="sxs-lookup"><span data-stu-id="fc507-477">*Add View Dialog*</span></span>

    <span data-ttu-id="fc507-478">**\Views\Store\Index.cshtml** widoku pliku szablonu jest utworzony i otwarty.</span><span class="sxs-lookup"><span data-stu-id="fc507-478">The **\Views\Store\Index.cshtml** View template file is created and opened.</span></span> <span data-ttu-id="fc507-479">Na podstawie informacji do **Dodaj widok** okna dialogowego w ostatnim kroku, widok szablonu będzie oczekiwać **StoreIndexViewModel** wystąpienia jako danych używany do generowania odpowiedzi HTML.</span><span class="sxs-lookup"><span data-stu-id="fc507-479">Based on the information provided to the **Add View** dialog in the last step, the View template will expect a **StoreIndexViewModel** instance as the data to use to generate an HTML response.</span></span> <span data-ttu-id="fc507-480">Można zauważyć, że szablon dziedziczy `ViewPage<musicstore.viewmodels.storeindexviewmodel>` w języku C#.</span><span class="sxs-lookup"><span data-stu-id="fc507-480">You will notice that the template inherits a `ViewPage<musicstore.viewmodels.storeindexviewmodel>` in C#.</span></span>

<a id="Ex5Task5"></a>

<a id="Task_5_-_Updating_the_View_Template"></a>
#### <a name="task-5---updating-the-view-template"></a><span data-ttu-id="fc507-481">Zadanie 5 aktualizowanie szablonu widoku.</span><span class="sxs-lookup"><span data-stu-id="fc507-481">Task 5 - Updating the View Template</span></span>

<span data-ttu-id="fc507-482">To zadanie zaktualizuje Wyświetl szablon utworzony w ostatnim zadaniem, aby pobrać liczbę genres i ich nazwy na stronie.</span><span class="sxs-lookup"><span data-stu-id="fc507-482">In this task, you will update the View template created in the last task to retrieve the number of genres and their names within the page.</span></span>

> [!NOTE]
> <span data-ttu-id="fc507-483">Użyjesz @ składni (często określany jako &quot;kodu nuggets&quot;) do wykonywania kodu w szablonie widoku.</span><span class="sxs-lookup"><span data-stu-id="fc507-483">You will use @ syntax (often referred to as &quot;code nuggets&quot;) to execute code within the View template.</span></span>

1. <span data-ttu-id="fc507-484">W **Index.cshtml** pliku poziomu **magazynu** folderu, zastąp jego kod następującym kodem:</span><span class="sxs-lookup"><span data-stu-id="fc507-484">In the **Index.cshtml** file, within the **Store** folder, replace its code with the following:</span></span>

[!code-cshtml[Main](aspnet-mvc-4-fundamentals/samples/sample14.cshtml)]

    > [!NOTE]
    > As soon as you finish typing the period after the word **Model**, Visual Studio's Intellisense will show a list of possible properties and methods to choose from.
    > 
    > ![](aspnet-mvc-4-fundamentals/_static/image25.png)
    > 
    > *Getting Model properties and methods with Visual Studio's IntelliSense*
    > 
    > The **Model** property references the **StoreIndexViewModel** object that the Controller passed to the View template. This means that you can access all of the data passed from the Controller to the View template via the **Model** property, and format it into an appropriate HTML response within the View template.
    > 
    > You can just select the **NumberOfGenres** property from the Intellisense list rather than typing it in and then it will auto-complete it by pressing the **tab key**.
2. <span data-ttu-id="fc507-485">Pętla za pośrednictwem listy genre w **StoreIndexViewModel** i tworzenia kodu HTML **&lt;ul&gt;** listy przy użyciu **foreach** pętli.</span><span class="sxs-lookup"><span data-stu-id="fc507-485">Loop over the genre list in **StoreIndexViewModel** and create an HTML **&lt;ul&gt;** list using a **foreach** loop.</span></span>
   <span data-ttu-id="fc507-486">(C#)</span><span class="sxs-lookup"><span data-stu-id="fc507-486">(C#)</span></span>

    [!code-cshtml[Main](aspnet-mvc-4-fundamentals/samples/sample15.cshtml)]
3. <span data-ttu-id="fc507-487">Naciśnij klawisz **F5** uruchomić aplikację, a następnie przejdź **/magazynu**.</span><span class="sxs-lookup"><span data-stu-id="fc507-487">Press **F5** to run the Application and browse **/Store**.</span></span> <span data-ttu-id="fc507-488">Zostanie wyświetlona lista gatunkami muzyki przekazano **StoreIndexViewModel** obiekt z **StoreController** do szablonu widoku.</span><span class="sxs-lookup"><span data-stu-id="fc507-488">You will see the list of genres passed in the **StoreIndexViewModel** object from the **StoreController** to the View template.</span></span>

    <span data-ttu-id="fc507-489">![Wyświetlanie listy gatunkami muzyki widoku](aspnet-mvc-4-fundamentals/_static/image26.png "widoku wyświetlanie listy gatunkami muzyki")</span><span class="sxs-lookup"><span data-stu-id="fc507-489">![View displaying a list of genres](aspnet-mvc-4-fundamentals/_static/image26.png "View displaying a list of genres")</span></span>

    <span data-ttu-id="fc507-490">*Wyświetlanie listy gatunkami muzyki widoku*</span><span class="sxs-lookup"><span data-stu-id="fc507-490">*View displaying a list of genres*</span></span>
4. <span data-ttu-id="fc507-491">Zamknij przeglądarkę.</span><span class="sxs-lookup"><span data-stu-id="fc507-491">Close the browser.</span></span>

<a id="Exercise6"></a>

<a id="Exercise_6_Using_Parameters_in_View"></a>
### <a name="exercise-6-using-parameters-in-view"></a><span data-ttu-id="fc507-492">Ćwiczenie 6: W widoku przy użyciu parametrów</span><span class="sxs-lookup"><span data-stu-id="fc507-492">Exercise 6: Using Parameters in View</span></span>

<span data-ttu-id="fc507-493">Ćwiczenie 3 przedstawiono sposób przekazania parametrów do kontrolera.</span><span class="sxs-lookup"><span data-stu-id="fc507-493">In Exercise 3 you learned how to pass parameters to the Controller.</span></span> <span data-ttu-id="fc507-494">W tym ćwiczeniu dowiesz się, jak używać tych parametrów w szablonie widoku.</span><span class="sxs-lookup"><span data-stu-id="fc507-494">In this exercise, you will learn how to use those parameters in the View template.</span></span> <span data-ttu-id="fc507-495">W tym celu należy zostaną wprowadzone najpierw do klasy modeli, które ułatwiają zarządzanie logika danych i domeny.</span><span class="sxs-lookup"><span data-stu-id="fc507-495">For that purpose, you will be introduced first to Model classes that will help you to manage your data and domain logic.</span></span> <span data-ttu-id="fc507-496">Ponadto dowiesz sposobu tworzenia łączy do stron w aplikacji ASP.NET MVC bez obaw rzeczy, takich jak kodowanie ścieżki adresu URL.</span><span class="sxs-lookup"><span data-stu-id="fc507-496">Additionally, you will learn how to create links to pages inside the ASP.NET MVC application without worrying of things like URL paths encoding.</span></span>

<a id="Ex6Task1"></a>

<a id="Task_1_-_Adding_Model_Classes"></a>
#### <a name="task-1---adding-model-classes"></a><span data-ttu-id="fc507-497">Zadanie 1 — Dodawanie klasy modelu</span><span class="sxs-lookup"><span data-stu-id="fc507-497">Task 1 - Adding Model Classes</span></span>

<span data-ttu-id="fc507-498">W odróżnieniu od ViewModels, które są tworzone tylko w celu przekazywania informacji z kontrolera do widoku, klasy modeli są tworzone zawierają i zarządzanie nimi logiki danych i domeny.</span><span class="sxs-lookup"><span data-stu-id="fc507-498">Unlike ViewModels, which are created just to pass information from the Controller to the View, Model classes are built to contain and manage data and domain logic.</span></span> <span data-ttu-id="fc507-499">W ramach tego zadania zostaną dodane dwie klasy modelu do reprezentowania tych pojęć: **Genre** i **albumu**.</span><span class="sxs-lookup"><span data-stu-id="fc507-499">In this task you will add two model classes to represent these concepts: **Genre** and **Album**.</span></span>

1. <span data-ttu-id="fc507-500">Jeśli nie już otwarty, uruchom **VS Express for Web**</span><span class="sxs-lookup"><span data-stu-id="fc507-500">If not already open, start **VS Express for Web**</span></span>
2. <span data-ttu-id="fc507-501">W **pliku** menu, wybierz **Otwórz projekt**.</span><span class="sxs-lookup"><span data-stu-id="fc507-501">In the **File** menu, choose **Open Project**.</span></span> <span data-ttu-id="fc507-502">W oknie dialogowym Otwórz projekt, przejdź do **Source\Ex06 UsingParametersInView\Begin**, wybierz pozycję **Begin.sln** i kliknij przycisk **Otwórz**.</span><span class="sxs-lookup"><span data-stu-id="fc507-502">In the Open Project dialog, browse to **Source\Ex06-UsingParametersInView\Begin**, select **Begin.sln** and click **Open**.</span></span> <span data-ttu-id="fc507-503">Alternatywnie możesz nadal z rozwiązaniem uzyskany po zakończeniu pracy w poprzednim ćwiczeniu.</span><span class="sxs-lookup"><span data-stu-id="fc507-503">Alternatively, you may continue with the solution that you obtained after completing the previous exercise.</span></span>

   1. <span data-ttu-id="fc507-504">Po otwarciu dostarczonych **rozpocząć** rozwiązania, musisz pobrać niektórych brakujących pakietów NuGet aby kontynuować.</span><span class="sxs-lookup"><span data-stu-id="fc507-504">If you opened the provided **Begin** solution, you will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="fc507-505">Aby to zrobić, kliknij przycisk **projektu** menu i wybierz **Zarządzaj pakietami NuGet**.</span><span class="sxs-lookup"><span data-stu-id="fc507-505">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
   2. <span data-ttu-id="fc507-506">W **Zarządzaj pakietami NuGet** okna dialogowego, kliknij przycisk **przywrócić** celu pobieranie brakujących pakietów.</span><span class="sxs-lookup"><span data-stu-id="fc507-506">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
   3. <span data-ttu-id="fc507-507">Na koniec Skompiluj rozwiązanie, klikając **kompilacji** | **Kompiluj rozwiązanie**.</span><span class="sxs-lookup"><span data-stu-id="fc507-507">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

      > [!NOTE]
      > <span data-ttu-id="fc507-508">Jedną z zalet przy użyciu narzędzia NuGet jest, że nie masz do wysłania wszystkich bibliotek w projekcie, zmniejszenie jego rozmiar projektu.</span><span class="sxs-lookup"><span data-stu-id="fc507-508">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="fc507-509">Narzędzia Power NuGet określając wersje pakietów w pliku Packages.config, będzie można pobrać wymaganych bibliotek podczas pierwszego uruchomienia projektu.</span><span class="sxs-lookup"><span data-stu-id="fc507-509">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="fc507-510">Jest to, dlaczego konieczne będzie wykonanie tych kroków, po otwarciu istniejącego rozwiązania z tego laboratorium.</span><span class="sxs-lookup"><span data-stu-id="fc507-510">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
3. <span data-ttu-id="fc507-511">Dodaj **Genre** klasa modelu.</span><span class="sxs-lookup"><span data-stu-id="fc507-511">Add a **Genre** Model class.</span></span> <span data-ttu-id="fc507-512">Aby to zrobić, kliknij prawym przyciskiem myszy **modele** folderu w **Eksploratora rozwiązań**, wybierz pozycję **Dodaj** , a następnie **nowy element** opcji.</span><span class="sxs-lookup"><span data-stu-id="fc507-512">To do this, right-click the **Models** folder in the **Solution Explorer**, select **Add** and then the **New Item** option.</span></span> <span data-ttu-id="fc507-513">W obszarze **kod**, wybierz **klasy** elementu i nazwij plik *Genre.cs*, następnie kliknij przycisk **Dodaj**.</span><span class="sxs-lookup"><span data-stu-id="fc507-513">Under **Code**, choose the **Class** item and name the file *Genre.cs*, then click **Add**.</span></span>

    <span data-ttu-id="fc507-514">![Dodawanie klasy](aspnet-mvc-4-fundamentals/_static/image27.png "Dodawanie klasy")</span><span class="sxs-lookup"><span data-stu-id="fc507-514">![Adding a class](aspnet-mvc-4-fundamentals/_static/image27.png "Adding a class")</span></span>

    <span data-ttu-id="fc507-515">*Dodawanie nowego elementu*</span><span class="sxs-lookup"><span data-stu-id="fc507-515">*Adding a new item*</span></span>

    <span data-ttu-id="fc507-516">![Dodaj klasę modelu Genre](aspnet-mvc-4-fundamentals/_static/image28.png "Dodaj klasę modelu Genre")</span><span class="sxs-lookup"><span data-stu-id="fc507-516">![Add Genre Model Class](aspnet-mvc-4-fundamentals/_static/image28.png "Add Genre Model Class")</span></span>

    <span data-ttu-id="fc507-517">*Dodaj klasę modelu Genre*</span><span class="sxs-lookup"><span data-stu-id="fc507-517">*Add Genre Model Class*</span></span>
4. <span data-ttu-id="fc507-518">Dodaj **nazwa** właściwości do klasy Genre.</span><span class="sxs-lookup"><span data-stu-id="fc507-518">Add a **Name** property to the Genre class.</span></span> <span data-ttu-id="fc507-519">Aby to zrobić, Dodaj następujący kod:</span><span class="sxs-lookup"><span data-stu-id="fc507-519">To do this, add the following code:</span></span>

    <span data-ttu-id="fc507-520">(Fragment - kodu *podstawy platformy ASP.NET MVC 4 - Ex6 Genre*)</span><span class="sxs-lookup"><span data-stu-id="fc507-520">(Code Snippet - *ASP.NET MVC 4 Fundamentals - Ex6 Genre*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample16.cs)]
5. <span data-ttu-id="fc507-521">Procedury jak wcześniej, Dodaj **albumu** klasy.</span><span class="sxs-lookup"><span data-stu-id="fc507-521">Following the same procedure as before, add an **Album** class.</span></span> <span data-ttu-id="fc507-522">Aby to zrobić, kliknij prawym przyciskiem myszy **modele** folderu w **Eksploratora rozwiązań**, wybierz pozycję **Dodaj** , a następnie **nowy element** opcji.</span><span class="sxs-lookup"><span data-stu-id="fc507-522">To do this, right-click the **Models** folder in the **Solution Explorer**, select **Add** and then the **New Item** option.</span></span> <span data-ttu-id="fc507-523">W obszarze **kod**, wybierz **klasy** elementu i nazwij plik *Album.cs*, następnie kliknij przycisk **Dodaj**.</span><span class="sxs-lookup"><span data-stu-id="fc507-523">Under **Code**, choose the **Class** item and name the file *Album.cs*, then click **Add**.</span></span>
6. <span data-ttu-id="fc507-524">Dodaj dwie właściwości do klasy albumu: **Genre** i **tytuł**.</span><span class="sxs-lookup"><span data-stu-id="fc507-524">Add two properties to the Album class: **Genre** and **Title**.</span></span> <span data-ttu-id="fc507-525">Aby to zrobić, Dodaj następujący kod:</span><span class="sxs-lookup"><span data-stu-id="fc507-525">To do this, add the following code:</span></span>

    <span data-ttu-id="fc507-526">(Fragment - kodu *podstawy platformy ASP.NET MVC 4 - albumu Ex6*)</span><span class="sxs-lookup"><span data-stu-id="fc507-526">(Code Snippet - *ASP.NET MVC 4 Fundamentals - Ex6 Album*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample17.cs)]

<a id="Ex6Task2"></a>

<a id="Task_2_-_Adding_a_StoreBrowseViewModel"></a>
#### <a name="task-2---adding-a-storebrowseviewmodel"></a><span data-ttu-id="fc507-527">Zadanie 2 — Dodawanie StoreBrowseViewModel</span><span class="sxs-lookup"><span data-stu-id="fc507-527">Task 2 - Adding a StoreBrowseViewModel</span></span>

<span data-ttu-id="fc507-528">A **StoreBrowseViewModel** będzie używany w tym zadaniu pokazanie albumy, zgodne wybranego rodzaju.</span><span class="sxs-lookup"><span data-stu-id="fc507-528">A **StoreBrowseViewModel** will be used in this task to show the Albums that match a selected Genre.</span></span> <span data-ttu-id="fc507-529">To zadanie będzie utworzyć tę klasę, a następnie dodaj dwie właściwości do obsługi **Genre** i jego **albumu**na liście.</span><span class="sxs-lookup"><span data-stu-id="fc507-529">In this task, you will create this class and then add two properties to handle the **Genre** and its **Album**'s List.</span></span>

1. <span data-ttu-id="fc507-530">Dodaj **StoreBrowseViewModel** klasy.</span><span class="sxs-lookup"><span data-stu-id="fc507-530">Add a **StoreBrowseViewModel** class.</span></span> <span data-ttu-id="fc507-531">Aby to zrobić, kliknij prawym przyciskiem myszy **ViewModels** folderu w **Eksploratora rozwiązań**, wybierz pozycję **Dodaj** , a następnie **nowy element** opcji.</span><span class="sxs-lookup"><span data-stu-id="fc507-531">To do this, right-click the **ViewModels** folder in the **Solution Explorer**, select **Add** and then the **New Item** option.</span></span> <span data-ttu-id="fc507-532">W obszarze **kod**, wybierz **klasy** elementu i nazwij plik *StoreBrowseViewModel.cs*, następnie kliknij przycisk **Dodaj**.</span><span class="sxs-lookup"><span data-stu-id="fc507-532">Under **Code**, choose the **Class** item and name the file *StoreBrowseViewModel.cs*, then click **Add**.</span></span>
2. <span data-ttu-id="fc507-533">Dodaj odwołanie do modeli w **StoreBrowseViewModel** klasy.</span><span class="sxs-lookup"><span data-stu-id="fc507-533">Add a reference to the Models in **StoreBrowseViewModel** class.</span></span> <span data-ttu-id="fc507-534">Aby to zrobić, Dodaj następujący za pomocą przestrzeni nazw:</span><span class="sxs-lookup"><span data-stu-id="fc507-534">To do this, add the following using namespace:</span></span>

    <span data-ttu-id="fc507-535">(Fragment - kodu *podstawy platformy ASP.NET MVC 4 - Ex6 UsingModel*)</span><span class="sxs-lookup"><span data-stu-id="fc507-535">(Code Snippet - *ASP.NET MVC 4 Fundamentals - Ex6 UsingModel*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample18.cs)]
3. <span data-ttu-id="fc507-536">Dodaj dwie właściwości do **StoreBrowseViewModel** klasy: **Genre** i **albumów**.</span><span class="sxs-lookup"><span data-stu-id="fc507-536">Add two properties to **StoreBrowseViewModel** class: **Genre** and **Albums**.</span></span> <span data-ttu-id="fc507-537">Aby to zrobić, Dodaj następujący kod:</span><span class="sxs-lookup"><span data-stu-id="fc507-537">To do this, add the following code:</span></span>

    <span data-ttu-id="fc507-538">(Fragment - kodu *podstawy platformy ASP.NET MVC 4 - Ex6 ModelProperties*)</span><span class="sxs-lookup"><span data-stu-id="fc507-538">(Code Snippet - *ASP.NET MVC 4 Fundamentals - Ex6 ModelProperties*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample19.cs)]

> [!NOTE]
> <span data-ttu-id="fc507-539">Co to jest **listy&lt;albumu&gt;**  ?: używa tej definicji **listy&lt;T&gt;**  typu, których **T** ogranicza Typ, na których elementy tego **listy** należy w tym przypadku **albumu** (lub któregokolwiek z jej obiektów podrzędnych).</span><span class="sxs-lookup"><span data-stu-id="fc507-539">What is **List&lt;Album&gt;** ?: This definition is using the **List&lt;T&gt;** type, where **T** constrains the type to which elements of this **List** belong to, in this case **Album** (or any of its descendants).</span></span>
> 
> <span data-ttu-id="fc507-540">Ta możliwość projektowania klasy i metody, które mają być odroczone specyfikację jeden lub więcej typów, aż klasa lub metoda jest zadeklarowany i utworzone przez kod klienta jest funkcją języka C# o nazwie **ogólne**.</span><span class="sxs-lookup"><span data-stu-id="fc507-540">This ability to design classes and methods that defer the specification of one or more types until the class or method is declared and instantiated by client code is a feature of the C# language called **Generics**.</span></span>
> 
> <span data-ttu-id="fc507-541">**Lista&lt;T&gt;**  ogólny odpowiednik **ArrayList** typu i jest dostępny w **system.Collections.Generic —** przestrzeni nazw.</span><span class="sxs-lookup"><span data-stu-id="fc507-541">**List&lt;T&gt;** is the generic equivalent of the **ArrayList** type and is available in the **System.Collections.Generic** namespace.</span></span> <span data-ttu-id="fc507-542">Jedną z korzyści wynikające ze stosowania **ogólne** jest, że ponieważ typem jest określona, nie trzeba zajmie się sprawdzanie operacji, takich jak elementy do rzutowania typu **albumu** jak z **ArrayList**.</span><span class="sxs-lookup"><span data-stu-id="fc507-542">One of the benefits of using **generics** is that since the type is specified, you do not need to take care of type checking operations such as casting the elements into **Album** as you would do with an **ArrayList**.</span></span>

<a id="Ex6Task3"></a>

<a id="Task_3_-_Using_the_New_ViewModel_in_the_StoreController"></a>
#### <a name="task-3---using-the-new-viewmodel-in-the-storecontroller"></a><span data-ttu-id="fc507-543">Zadanie 3 - w StoreController przy użyciu nowego ViewModel</span><span class="sxs-lookup"><span data-stu-id="fc507-543">Task 3 - Using the New ViewModel in the StoreController</span></span>

<span data-ttu-id="fc507-544">To zadanie, należy zmodyfikować **StoreController**w **Przeglądaj** i **szczegóły** metod akcji do używania nowych **StoreBrowseViewModel** .</span><span class="sxs-lookup"><span data-stu-id="fc507-544">In this task, you will modify the **StoreController**'s **Browse** and **Details** action methods to use the new **StoreBrowseViewModel**.</span></span>

1. <span data-ttu-id="fc507-545">Dodaj odwołanie do folderu modeli w **StoreController** klasy.</span><span class="sxs-lookup"><span data-stu-id="fc507-545">Add a reference to the Models folder in **StoreController** class.</span></span> <span data-ttu-id="fc507-546">Aby to zrobić, rozwiń węzeł **kontrolerów** folderu w **Eksploratora rozwiązań** , a następnie otwórz **StoreController** klasy.</span><span class="sxs-lookup"><span data-stu-id="fc507-546">To do this, expand the **Controllers** folder in the **Solution Explorer** and open the **StoreController** class.</span></span> <span data-ttu-id="fc507-547">Następnie dodaj następujący kod:</span><span class="sxs-lookup"><span data-stu-id="fc507-547">Then add the following code:</span></span>

    <span data-ttu-id="fc507-548">(Fragment - kodu *podstawy platformy ASP.NET MVC 4 - Ex6 UsingModelInController*)</span><span class="sxs-lookup"><span data-stu-id="fc507-548">(Code Snippet - *ASP.NET MVC 4 Fundamentals - Ex6 UsingModelInController*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample20.cs)]
2. <span data-ttu-id="fc507-549">Zastąp **Przeglądaj** metody akcji, aby użyć **StoreViewBrowseController** klasy.</span><span class="sxs-lookup"><span data-stu-id="fc507-549">Replace the **Browse** action method to use the **StoreViewBrowseController** class.</span></span> <span data-ttu-id="fc507-550">Utworzysz określonego rodzaju i dwa nowe obiekty albumów fikcyjny danych (w następnej laboratorium Hands-on będą korzystać prawdziwe dane z bazy danych).</span><span class="sxs-lookup"><span data-stu-id="fc507-550">You will create a Genre and two new Albums objects with dummy data (in the next Hands-on Lab you will consume real data from a database).</span></span> <span data-ttu-id="fc507-551">Aby to zrobić, Zamień **Przeglądaj** metodę z następującym kodem:</span><span class="sxs-lookup"><span data-stu-id="fc507-551">To do this, replace the **Browse** method with the following code:</span></span>

    <span data-ttu-id="fc507-552">(Fragment - kodu *podstawy platformy ASP.NET MVC 4 - Ex6 BrowseMethod*)</span><span class="sxs-lookup"><span data-stu-id="fc507-552">(Code Snippet - *ASP.NET MVC 4 Fundamentals - Ex6 BrowseMethod*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample21.cs)]
3. <span data-ttu-id="fc507-553">Zastąp **szczegóły** metody akcji, aby użyć **StoreViewBrowseController** klasy.</span><span class="sxs-lookup"><span data-stu-id="fc507-553">Replace the **Details** action method to use the **StoreViewBrowseController** class.</span></span> <span data-ttu-id="fc507-554">Utworzy nowy **albumu** obiekt ma zostać zwrócona do **widoku**.</span><span class="sxs-lookup"><span data-stu-id="fc507-554">You will create a new **Album** object to be returned to the **View**.</span></span> <span data-ttu-id="fc507-555">Aby to zrobić, Zamień **szczegóły** metodę z następującym kodem:</span><span class="sxs-lookup"><span data-stu-id="fc507-555">To do this, replace the **Details** method with the following code:</span></span>

    <span data-ttu-id="fc507-556">(Fragment - kodu *podstawy platformy ASP.NET MVC 4 - Ex6 DetailsMethod*)</span><span class="sxs-lookup"><span data-stu-id="fc507-556">(Code Snippet - *ASP.NET MVC 4 Fundamentals - Ex6 DetailsMethod*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample22.cs)]

<a id="Ex6Task4"></a>

<a id="Task_4_-_Adding_a_Browse_View_Template"></a>
#### <a name="task-4---adding-a-browse-view-template"></a><span data-ttu-id="fc507-557">Zadanie 4 — Dodawanie szablonu widoku przeglądania</span><span class="sxs-lookup"><span data-stu-id="fc507-557">Task 4 - Adding a Browse View Template</span></span>

<span data-ttu-id="fc507-558">To zadanie spowoduje dodanie **Przeglądaj** widok, aby pokazać albumów znaleziono dla określonego rodzaju.</span><span class="sxs-lookup"><span data-stu-id="fc507-558">In this task, you will add a **Browse** View to show the Albums found for a specific Genre.</span></span>

1. <span data-ttu-id="fc507-559">Przed utworzeniem nowego szablonu widoku, należy skompilować projekt, aby **Dodaj widok** okna dialogowego zna **ViewModel** klasy do użycia.</span><span class="sxs-lookup"><span data-stu-id="fc507-559">Before creating the new View template, you should build the project so that the **Add View** Dialog knows about the **ViewModel** class to use.</span></span> <span data-ttu-id="fc507-560">Skompiluj projekt, wybierając **kompilacji** element menu, a następnie **kompilacji MvcMusicStore**.</span><span class="sxs-lookup"><span data-stu-id="fc507-560">Build the project by selecting the **Build** menu item and then **Build MvcMusicStore**.</span></span>
2. <span data-ttu-id="fc507-561">Dodaj **Przeglądaj** widoku.</span><span class="sxs-lookup"><span data-stu-id="fc507-561">Add a **Browse** View.</span></span> <span data-ttu-id="fc507-562">Aby to zrobić, kliknij prawym przyciskiem myszy w **Przeglądaj** metody akcji **StoreController** i kliknij przycisk **Dodaj widok**.</span><span class="sxs-lookup"><span data-stu-id="fc507-562">To do this, right-click in the **Browse** action method of the **StoreController** and click **Add View**.</span></span>
3. <span data-ttu-id="fc507-563">W **Dodaj widok** okna dialogowego upewnij się, że nazwa widoku jest **Przeglądaj**.</span><span class="sxs-lookup"><span data-stu-id="fc507-563">In the **Add View** dialog box, verify that the View Name is **Browse**.</span></span> <span data-ttu-id="fc507-564">Sprawdź **utworzyć widok jednoznacznie** wyboru i wybierz **StoreBrowseViewModel** z **klasa modelu** listy rozwijanej.</span><span class="sxs-lookup"><span data-stu-id="fc507-564">Check the **Create a strongly-typed view** checkbox and select **StoreBrowseViewModel** from the **Model class** dropdown.</span></span> <span data-ttu-id="fc507-565">Pozostaw wartości domyślne innych pól.</span><span class="sxs-lookup"><span data-stu-id="fc507-565">Leave the other fields with their default value.</span></span> <span data-ttu-id="fc507-566">Następnie kliknij przycisk **Dodaj**.</span><span class="sxs-lookup"><span data-stu-id="fc507-566">Then click **Add**.</span></span>

    <span data-ttu-id="fc507-567">![Dodawanie widoku przeglądania](aspnet-mvc-4-fundamentals/_static/image29.png "Dodawanie widoku przeglądania")</span><span class="sxs-lookup"><span data-stu-id="fc507-567">![Adding a Browse View](aspnet-mvc-4-fundamentals/_static/image29.png "Adding a Browse View")</span></span>

    <span data-ttu-id="fc507-568">*Dodawanie widoku przeglądania*</span><span class="sxs-lookup"><span data-stu-id="fc507-568">*Adding a Browse View*</span></span>
4. <span data-ttu-id="fc507-569">Modyfikowanie **Browse.cshtml** do wyświetlania informacji Genre, uzyskiwanie dostępu do **StoreBrowseViewModel** obiekt, który jest przekazywany do szablonu widoku.</span><span class="sxs-lookup"><span data-stu-id="fc507-569">Modify the **Browse.cshtml** to display the Genre's information, accessing the **StoreBrowseViewModel** object that is passed to the view template.</span></span> <span data-ttu-id="fc507-570">Aby to zrobić, Zastąp zawartość następującym kodem: (C#)</span><span class="sxs-lookup"><span data-stu-id="fc507-570">To do this, replace the content with the following: (C#)</span></span>

    [!code-cshtml[Main](aspnet-mvc-4-fundamentals/samples/sample23.cshtml)]

<a id="Ex6Task5"></a>

<a id="Task_5_-_Running_the_Application"></a>
#### <a name="task-5---running-the-application"></a><span data-ttu-id="fc507-571">Zadanie 5 działania aplikacji.</span><span class="sxs-lookup"><span data-stu-id="fc507-571">Task 5 - Running the Application</span></span>

<span data-ttu-id="fc507-572">W tym zadaniu zostanie sprawdzić, czy **Przeglądaj** metoda pobiera albumów z **Przeglądaj** metody akcji.</span><span class="sxs-lookup"><span data-stu-id="fc507-572">In this task, you will test that the **Browse** method retrieves Albums from the **Browse** method action.</span></span>

1. <span data-ttu-id="fc507-573">Naciśnij klawisz **F5** do uruchomienia aplikacji.</span><span class="sxs-lookup"><span data-stu-id="fc507-573">Press **F5** to run the Application.</span></span>
2. <span data-ttu-id="fc507-574">Na stronie głównej datę rozpoczęcia projektu.</span><span class="sxs-lookup"><span data-stu-id="fc507-574">The project starts in the Home page.</span></span> <span data-ttu-id="fc507-575">Zmień adres URL do   **/przechowywania/Przeglądaj? Genre = Disco** Aby sprawdzić, czy akcji zwraca dwa albumów.</span><span class="sxs-lookup"><span data-stu-id="fc507-575">Change the URL to **/Store/Browse?Genre=Disco** to verify that the action returns two Albums.</span></span>

    <span data-ttu-id="fc507-576">![Przeglądanie magazynu albumów Disco](aspnet-mvc-4-fundamentals/_static/image30.png "przeglądanie albumów Disco magazynu")</span><span class="sxs-lookup"><span data-stu-id="fc507-576">![Browsing Store Disco Albums](aspnet-mvc-4-fundamentals/_static/image30.png "Browsing Store Disco Albums")</span></span>

    <span data-ttu-id="fc507-577">*Przeglądanie albumów Disco magazynu*</span><span class="sxs-lookup"><span data-stu-id="fc507-577">*Browsing Store Disco Albums*</span></span>

<a id="Ex6Task6"></a>

<a id="Task_6_-_Displaying_information_About_a_Specific_Album"></a>
#### <a name="task-6---displaying-information-about-a-specific-album"></a><span data-ttu-id="fc507-578">Zadanie 6 — wyświetlanie informacji o określonych albumu</span><span class="sxs-lookup"><span data-stu-id="fc507-578">Task 6 - Displaying information About a Specific Album</span></span>

<span data-ttu-id="fc507-579">W tym zadaniu zostaną zaimplementowane **szczegóły dotyczące i magazynu** widok, aby wyświetlić informacje o określonym albumu.</span><span class="sxs-lookup"><span data-stu-id="fc507-579">In this task, you will implement the **Store/Details** view to display information about a specific album.</span></span> <span data-ttu-id="fc507-580">W tym laboratorium Hands-on wszystko, co spowoduje wyświetlenie o album znajduje się już w **widoku** szablonu.</span><span class="sxs-lookup"><span data-stu-id="fc507-580">In this Hands-on Lab, everything you will display about the album is already contained in the **View** template.</span></span> <span data-ttu-id="fc507-581">Tak, zamiast tworzyć **StoreDetailsViewModel** klasy, użyje bieżącego **StoreBrowseViewModel** szablonu przekazywanie Album do niego.</span><span class="sxs-lookup"><span data-stu-id="fc507-581">So, instead of creating a **StoreDetailsViewModel** class, you will use the current **StoreBrowseViewModel** template passing the Album to it.</span></span>

1. <span data-ttu-id="fc507-582">Zamknij przeglądarkę, jeśli to konieczne, aby powrócić do okna programu Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="fc507-582">Close the browser if needed, to return to the Visual Studio window.</span></span> <span data-ttu-id="fc507-583">Dodaj nową **szczegóły** wyświetlić **StoreController**w **szczegóły** metody akcji.</span><span class="sxs-lookup"><span data-stu-id="fc507-583">Add a new **Details** view for the **StoreController**'s **Details** action method.</span></span> <span data-ttu-id="fc507-584">Aby to zrobić, kliknij prawym przyciskiem myszy **szczegóły** metody w **StoreController** klasy, a następnie kliknij przycisk **Dodaj widok**.</span><span class="sxs-lookup"><span data-stu-id="fc507-584">To do this, right-click the **Details** method in the **StoreController** class and click **Add View**.</span></span>
2. <span data-ttu-id="fc507-585">W **Dodaj widok** okna dialogowego, upewnij się, że **nazwy widoku** jest **szczegóły**.</span><span class="sxs-lookup"><span data-stu-id="fc507-585">In the **Add View** dialog, verify that the **View Name** is **Details**.</span></span> <span data-ttu-id="fc507-586">Sprawdź **utworzyć widok jednoznacznie** wyboru i wybierz **albumu** z **klasa modelu** listy rozwijanej.</span><span class="sxs-lookup"><span data-stu-id="fc507-586">Check the **Create a strongly-typed view** checkbox and select **Album** from the **Model class** drop-down.</span></span> <span data-ttu-id="fc507-587">Pozostaw wartości domyślne innych pól.</span><span class="sxs-lookup"><span data-stu-id="fc507-587">Leave the other fields with their default value.</span></span> <span data-ttu-id="fc507-588">Następnie kliknij przycisk **Dodaj**.</span><span class="sxs-lookup"><span data-stu-id="fc507-588">Then click **Add**.</span></span> <span data-ttu-id="fc507-589">Spowoduje to utworzenie i otworzyć **\Views\Store\Details.cshtml** pliku.</span><span class="sxs-lookup"><span data-stu-id="fc507-589">This will create and open a **\Views\Store\Details.cshtml** file.</span></span>

    <span data-ttu-id="fc507-590">![Dodawanie widoku szczegółów](aspnet-mvc-4-fundamentals/_static/image31.png "Dodawanie widoku szczegółów")</span><span class="sxs-lookup"><span data-stu-id="fc507-590">![Adding a Details View](aspnet-mvc-4-fundamentals/_static/image31.png "Adding a Details View")</span></span>

    <span data-ttu-id="fc507-591">*Dodawanie widoku szczegółów*</span><span class="sxs-lookup"><span data-stu-id="fc507-591">*Adding a Details View*</span></span>
3. <span data-ttu-id="fc507-592">Modyfikowanie **Details.cshtml** plik, aby wyświetlić informacje albumu podczas uzyskiwania dostępu do **albumu** obiekt, który jest przekazywany do szablonu widoku.</span><span class="sxs-lookup"><span data-stu-id="fc507-592">Modify the **Details.cshtml** file to display the Album's information, accessing the **Album** object that is passed to the view template.</span></span> <span data-ttu-id="fc507-593">Aby to zrobić, Zastąp zawartość następującym kodem: (C#)</span><span class="sxs-lookup"><span data-stu-id="fc507-593">To do this, replace the content with the following: (C#)</span></span>

    [!code-cshtml[Main](aspnet-mvc-4-fundamentals/samples/sample24.cshtml)]

<a id="Ex6Task7"></a>

<a id="Task_7_-_Running_the_Application"></a>
#### <a name="task-7---running-the-application"></a><span data-ttu-id="fc507-594">Zadanie 7 - uruchamiania aplikacji</span><span class="sxs-lookup"><span data-stu-id="fc507-594">Task 7 - Running the Application</span></span>

<span data-ttu-id="fc507-595">W tym zadaniu zostanie sprawdzić, czy **szczegóły** widok pobiera informacje albumu z **szczegóły akcji** metody.</span><span class="sxs-lookup"><span data-stu-id="fc507-595">In this task, you will test that the **Details** View retrieves Album's information from the **Details action** method.</span></span>

1. <span data-ttu-id="fc507-596">Naciśnij klawisz **F5** do uruchomienia aplikacji.</span><span class="sxs-lookup"><span data-stu-id="fc507-596">Press **F5** to run the Application.</span></span>
2. <span data-ttu-id="fc507-597">Projekt jest uruchamiany w **Home** strony.</span><span class="sxs-lookup"><span data-stu-id="fc507-597">The project starts in the **Home** page.</span></span> <span data-ttu-id="fc507-598">Zmień adres URL do **/Store/Details/5** do sprawdzenia informacji o albumu.</span><span class="sxs-lookup"><span data-stu-id="fc507-598">Change the URL to **/Store/Details/5** to verify the album's information.</span></span>

    <span data-ttu-id="fc507-599">![Przeglądanie szczegółów albumów](aspnet-mvc-4-fundamentals/_static/image32.png "przeglądania szczegółów albumów")</span><span class="sxs-lookup"><span data-stu-id="fc507-599">![Browsing Albums Detail](aspnet-mvc-4-fundamentals/_static/image32.png "Browsing Albums Detail")</span></span>

    <span data-ttu-id="fc507-600">*Album przeglądania szczegółów*</span><span class="sxs-lookup"><span data-stu-id="fc507-600">*Browsing Album's Detail*</span></span>

<a id="Ex6Task8"></a>

<a id="Task_8_-_Adding_Links_Between_Pages"></a>
#### <a name="task-8---adding-links-between-pages"></a><span data-ttu-id="fc507-601">Zadanie 8 — Dodawanie łącza między stronami</span><span class="sxs-lookup"><span data-stu-id="fc507-601">Task 8 - Adding Links Between Pages</span></span>

<span data-ttu-id="fc507-602">To zadanie będzie dodanie łącza w widoku magazynu w celu zapewnienia łącze każdego rodzaju nazwy do odpowiedniego **/magazynu/Przeglądaj** adresu URL.</span><span class="sxs-lookup"><span data-stu-id="fc507-602">In this task, you will add a link in the Store View to have a link in every Genre name to the appropriate **/Store/Browse** URL.</span></span> <span data-ttu-id="fc507-603">Dzięki temu po kliknięciu określonego rodzaju, na przykład **Disco**, spowoduje przejście do **/magazynu/Przeglądaj? genre = Disco** adresu URL.</span><span class="sxs-lookup"><span data-stu-id="fc507-603">This way, when you click on a Genre, for instance **Disco**, it will navigate to **/Store/Browse?genre=Disco** URL.</span></span>

1. <span data-ttu-id="fc507-604">Zamknij przeglądarkę, jeśli to konieczne, aby powrócić do okna programu Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="fc507-604">Close the browser if needed, to return to the Visual Studio window.</span></span> <span data-ttu-id="fc507-605">Aktualizacja **indeksu** stronę, aby dodać łącze do **Przeglądaj** strony.</span><span class="sxs-lookup"><span data-stu-id="fc507-605">Update the **Index** page to add a link to the **Browse** page.</span></span> <span data-ttu-id="fc507-606">Aby to zrobić, w **Eksploratora rozwiązań** rozwiń węzeł **widoków** folderu, a następnie **magazynu** folder i kliknij dwukrotnie **Index.cshtml** strony.</span><span class="sxs-lookup"><span data-stu-id="fc507-606">To do this, in the **Solution Explorer** expand the **Views** folder, then the **Store** folder and double-click the **Index.cshtml** page.</span></span>
2. <span data-ttu-id="fc507-607">Dodaj łącze do widoku przeglądania wskazujący genre wybrane.</span><span class="sxs-lookup"><span data-stu-id="fc507-607">Add a link to the Browse view indicating the genre selected.</span></span> <span data-ttu-id="fc507-608">Aby to zrobić, Zamień następujące wyróżniony kod w **&lt;li&gt;** tagi: (C#)</span><span class="sxs-lookup"><span data-stu-id="fc507-608">To do this, replace the following highlighted code within the **&lt;li&gt;** tags: (C#)</span></span>

    [!code-cshtml[Main](aspnet-mvc-4-fundamentals/samples/sample25.cshtml)]

   > [!NOTE]
   > <span data-ttu-id="fc507-609">innym rozwiązaniem będzie łączenie bezpośrednio do strony, przy użyciu kodu podobne do poniższych:</span><span class="sxs-lookup"><span data-stu-id="fc507-609">another approach would be linking directly to the page, with a code like the following:</span></span>
   > 
   > <span data-ttu-id="fc507-610">&lt;href =&quot;/magazynu/Przeglądaj? genre =@genreName&quot;&gt;@genreName&lt;/a&gt;</span><span class="sxs-lookup"><span data-stu-id="fc507-610">&lt;a href=&quot;/Store/Browse?genre=@genreName&quot;&gt;@genreName&lt;/a&gt;</span></span>
   > 
   > <span data-ttu-id="fc507-611">Chociaż to rozwiązanie działa, jest zależna od ciągu zapisane na stałe.</span><span class="sxs-lookup"><span data-stu-id="fc507-611">Although this approach works, it depends on a hardcoded string.</span></span> <span data-ttu-id="fc507-612">Później w przypadku zmiany nazwy kontrolera, należy ręcznie zmienić tej instrukcji.</span><span class="sxs-lookup"><span data-stu-id="fc507-612">If you later rename the Controller, you will have to change this instruction manually.</span></span> <span data-ttu-id="fc507-613">Lepszym jest użycie **pomocnika kodu HTML** metody.</span><span class="sxs-lookup"><span data-stu-id="fc507-613">A better alternative is to use an **HTML Helper** method.</span></span> <span data-ttu-id="fc507-614">Platforma ASP.NET MVC zawiera metodę pomocnika kodu HTML, która jest dostępna dla takich zadań.</span><span class="sxs-lookup"><span data-stu-id="fc507-614">ASP.NET MVC includes an HTML Helper method which is available for such tasks.</span></span> <span data-ttu-id="fc507-615">**Html.ActionLink()** metody pomocnika ułatwia tworzenie HTML **&lt;&gt;** łącza, upewniając się, ścieżki adresu URL są poprawnie zakodowane w adresie URL.</span><span class="sxs-lookup"><span data-stu-id="fc507-615">The **Html.ActionLink()** helper method makes it easy to build HTML **&lt;a&gt;** links, making sure URL paths are properly URL encoded.</span></span>
   > 
   > <span data-ttu-id="fc507-616">Htlm.ActionLink ma kilka przeciążeń.</span><span class="sxs-lookup"><span data-stu-id="fc507-616">Htlm.ActionLink has several overloads.</span></span> <span data-ttu-id="fc507-617">W tym ćwiczeniu będzie używany, który przyjmuje trzy parametry:</span><span class="sxs-lookup"><span data-stu-id="fc507-617">In this exercise you will use one that takes three parameters:</span></span>
   > 
   > 1. <span data-ttu-id="fc507-618">Tekst łącza, która będzie wyświetlana nazwa Genre</span><span class="sxs-lookup"><span data-stu-id="fc507-618">Link text, which will display the Genre name</span></span>
   > 2. <span data-ttu-id="fc507-619">Nazwa akcji kontrolera (**Przeglądaj**)</span><span class="sxs-lookup"><span data-stu-id="fc507-619">Controller action name (**Browse**)</span></span>
   > 3. <span data-ttu-id="fc507-620">Wartości parametru, określając nazwę trasy (**Genre**) i wartość (**nazwa Genre**)</span><span class="sxs-lookup"><span data-stu-id="fc507-620">Route parameter values, specifying both the name (**Genre**) and the value (**Genre name**)</span></span>

<a id="Ex6Task9"></a>

<a id="Task_9_-_Running_the_Application"></a>
#### <a name="task-9---running-the-application"></a><span data-ttu-id="fc507-621">Zadanie 9 — uruchamianie aplikacji</span><span class="sxs-lookup"><span data-stu-id="fc507-621">Task 9 - Running the Application</span></span>

<span data-ttu-id="fc507-622">W tym zadaniu zostanie przetestować, czy każdego rodzaju są wyświetlane z łączem do odpowiedniej **/magazynu/Przeglądaj** adresu URL.</span><span class="sxs-lookup"><span data-stu-id="fc507-622">In this task, you will test that each Genre is displayed with a link to the appropriate **/Store/Browse** URL.</span></span>

1. <span data-ttu-id="fc507-623">Naciśnij klawisz **F5** do uruchomienia aplikacji.</span><span class="sxs-lookup"><span data-stu-id="fc507-623">Press **F5** to run the Application.</span></span>
2. <span data-ttu-id="fc507-624">Na stronie głównej datę rozpoczęcia projektu.</span><span class="sxs-lookup"><span data-stu-id="fc507-624">The project starts in the Home page.</span></span> <span data-ttu-id="fc507-625">Zmień adres URL do **/magazynu** można zweryfikować, że każdego rodzaju linki do odpowiednich **/magazynu/Przeglądaj** adresu URL.</span><span class="sxs-lookup"><span data-stu-id="fc507-625">Change the URL to **/Store** to verify that each Genre links to the appropriate **/Store/Browse** URL.</span></span>

    <span data-ttu-id="fc507-626">![Przeglądanie Genres wraz z łączami do przeglądania strony](aspnet-mvc-4-fundamentals/_static/image33.png "przeglądanie Genres wraz z łączami do przeglądania strony")</span><span class="sxs-lookup"><span data-stu-id="fc507-626">![Browsing Genres with links to Browse page](aspnet-mvc-4-fundamentals/_static/image33.png "Browsing Genres with links to Browse page")</span></span>

    <span data-ttu-id="fc507-627">*Przeglądanie Genres wraz z łączami do przeglądania strony*</span><span class="sxs-lookup"><span data-stu-id="fc507-627">*Browsing Genres with links to Browse page*</span></span>

<a id="Ex6Task10"></a>

<a id="Task_10_-_Using_Dynamic_ViewModel_Collection_to_Pass_Values"></a>
#### <a name="task-10---using-dynamic-viewmodel-collection-to-pass-values"></a><span data-ttu-id="fc507-628">Zadanie 10 - przy użyciu kolekcji dynamicznych ViewModel do przekazania wartości</span><span class="sxs-lookup"><span data-stu-id="fc507-628">Task 10 - Using Dynamic ViewModel Collection to Pass Values</span></span>

<span data-ttu-id="fc507-629">W tym zadaniu dowiesz się, prosta i skuteczna metoda przekazać wartości między kontrolerem a widokiem bez wprowadzania żadnych zmian w modelu.</span><span class="sxs-lookup"><span data-stu-id="fc507-629">In this task, you will learn a simple and powerful method to pass values between the Controller and the View without making any changes in the Model.</span></span> <span data-ttu-id="fc507-630">ASP.NET MVC 4 udostępnia kolekcję &quot;ViewModel&quot;, co przypisane do żadnej wartości dynamiczne i dostęp do wewnątrz również widoków i kontrolerów.</span><span class="sxs-lookup"><span data-stu-id="fc507-630">ASP.NET MVC 4 provides the collection &quot;ViewModel&quot;, which can be assigned to any dynamic value and accessed inside controllers and views as well.</span></span>

<span data-ttu-id="fc507-631">Teraz użyje kolekcji dynamicznych elementów ViewBag do przekazania listę &quot; **Starred genres** &quot; z kontrolera do widoku.</span><span class="sxs-lookup"><span data-stu-id="fc507-631">You will now use the ViewBag dynamic collection to pass a list of &quot;**Starred genres**&quot; from the controller to the view.</span></span> <span data-ttu-id="fc507-632">Widok indeksu magazynu będą uzyskiwać dostęp do **ViewModel** i wyświetlić informacje.</span><span class="sxs-lookup"><span data-stu-id="fc507-632">The Store Index view will access to **ViewModel** and display the information.</span></span>

1. <span data-ttu-id="fc507-633">Zamknij przeglądarkę, jeśli to konieczne, aby powrócić do okna programu Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="fc507-633">Close the browser if needed, to return to the Visual Studio window.</span></span> <span data-ttu-id="fc507-634">Otwórz **StoreController.cs** i zmodyfikuj **indeksu** metodę w celu utworzenia listy starred genres do kolekcji ViewModel:</span><span class="sxs-lookup"><span data-stu-id="fc507-634">Open **StoreController.cs** and modify **Index** method to create a list of starred genres into ViewModel collection :</span></span>

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample26.cs)]

    > [!NOTE]
    > <span data-ttu-id="fc507-635">Można również użyć składni **obiekt ViewBag [&quot;Starred&quot;]** uzyskać dostęp do właściwości.</span><span class="sxs-lookup"><span data-stu-id="fc507-635">You could also use the syntax **ViewBag[&quot;Starred&quot;]** to access the properties.</span></span>
2. <span data-ttu-id="fc507-636">Ikonę gwiazdki **&quot;starred.png&quot;** znajduje się w **Source\Assets\Images** folder tego laboratorium.</span><span class="sxs-lookup"><span data-stu-id="fc507-636">The star icon **&quot;starred.png&quot;** is included in the **Source\Assets\Images** folder of this lab.</span></span> <span data-ttu-id="fc507-637">Aby dodać go do aplikacji, przeciągnij ich zawartość z **Eksploratora Windows** okna do **Eksploratora rozwiązań** w Visual Web Developer Express, jak pokazano poniżej:</span><span class="sxs-lookup"><span data-stu-id="fc507-637">In order to add it to the application, drag their content from a **Windows Explorer** window into the **Solution Explorer** in Visual Web Developer Express, as shown below:</span></span>

    <span data-ttu-id="fc507-638">![Dodawanie obrazu gwiazdy do rozwiązania](aspnet-mvc-4-fundamentals/_static/image34.png "Dodawanie gwiazdy obrazu do rozwiązania")</span><span class="sxs-lookup"><span data-stu-id="fc507-638">![Adding star image to the solution](aspnet-mvc-4-fundamentals/_static/image34.png "Adding star image to the solution")</span></span>

    <span data-ttu-id="fc507-639">*Dodawanie obrazu gwiazdy do rozwiązania*</span><span class="sxs-lookup"><span data-stu-id="fc507-639">*Adding star image to the solution*</span></span>
3. <span data-ttu-id="fc507-640">Otwórz widok **Store/Index.cshtml** i modyfikować zawartość.</span><span class="sxs-lookup"><span data-stu-id="fc507-640">Open the view **Store/Index.cshtml** and modify the content.</span></span> <span data-ttu-id="fc507-641">Będzie odczytywał &quot;starred&quot; właściwości w **obiekt ViewBag** kolekcji i poproś, jeśli bieżąca nazwa genre znajduje się na liście.</span><span class="sxs-lookup"><span data-stu-id="fc507-641">You will read the &quot;starred&quot; property in the **ViewBag** collection, and ask if the current genre name is in the list.</span></span> <span data-ttu-id="fc507-642">W takim przypadku zostaną wyświetlone ikonę gwiazdki prawo do łącza rodzaju.</span><span class="sxs-lookup"><span data-stu-id="fc507-642">In that case you will show a star icon right to the genre link.</span></span>
   <span data-ttu-id="fc507-643">(C#)</span><span class="sxs-lookup"><span data-stu-id="fc507-643">(C#)</span></span>

    [!code-cshtml[Main](aspnet-mvc-4-fundamentals/samples/sample27.cshtml)]

<a id="Ex6Task11"></a>

<a id="Task_11_-_Running_the_Application"></a>
#### <a name="task-11---running-the-application"></a><span data-ttu-id="fc507-644">Zadanie 11 - uruchamiania aplikacji</span><span class="sxs-lookup"><span data-stu-id="fc507-644">Task 11 - Running the Application</span></span>

<span data-ttu-id="fc507-645">W tym zadaniu zostanie przetestować, że jak pokazano genres wyświetlać ikonę gwiazdki.</span><span class="sxs-lookup"><span data-stu-id="fc507-645">In this task, you will test that the starred genres display a star icon.</span></span>

1. <span data-ttu-id="fc507-646">Naciśnij klawisz **F5** do uruchomienia aplikacji.</span><span class="sxs-lookup"><span data-stu-id="fc507-646">Press **F5** to run the Application.</span></span>
2. <span data-ttu-id="fc507-647">Projekt jest uruchamiany w **Home** strony.</span><span class="sxs-lookup"><span data-stu-id="fc507-647">The project starts in the **Home** page.</span></span> <span data-ttu-id="fc507-648">Zmień adres URL do **/magazynu** do sprawdzenia, czy każdego rodzaju polecanych ma wagę do poszanowania etykiety:</span><span class="sxs-lookup"><span data-stu-id="fc507-648">Change the URL to **/Store** to verify that each featured genre has the respecting label:</span></span>

    <span data-ttu-id="fc507-649">![Przeglądanie Genres elementów jak pokazano na](aspnet-mvc-4-fundamentals/_static/image35.png "przeglądanie Genres elementów jak pokazano na")</span><span class="sxs-lookup"><span data-stu-id="fc507-649">![Browsing Genres with starred elements](aspnet-mvc-4-fundamentals/_static/image35.png "Browsing Genres with starred elements")</span></span>

    <span data-ttu-id="fc507-650">*Przeglądanie Genres elementów jak pokazano na*</span><span class="sxs-lookup"><span data-stu-id="fc507-650">*Browsing Genres with starred elements*</span></span>

<a id="Exercise7"></a>

<a id="Exercise_7_A_lap_around_ASPNET_MVC_4_new_template"></a>
### <a name="exercise-7-a-lap-around-aspnet-mvc-4-new-template"></a><span data-ttu-id="fc507-651">Ćwiczenie 7: Laptop wokół nowego szablonu platformy ASP.NET MVC 4</span><span class="sxs-lookup"><span data-stu-id="fc507-651">Exercise 7: A lap around ASP.NET MVC 4 new template</span></span>

<span data-ttu-id="fc507-652">W tym ćwiczeniu zostanie Poznaj ulepszenia w szablonach projektu programu ASP.NET MVC 4, biorąc spojrzeć na najbardziej istotne cechy nowy szablon.</span><span class="sxs-lookup"><span data-stu-id="fc507-652">In this exercise, you will explore the enhancements in the ASP.NET MVC 4 project templates, taking a look at the most relevant features of the new template.</span></span>

<a id="Ex7Task1"></a>

<a id="Task_1_Exploring_the_ASPNET_MVC_4_Internet_Application_Template"></a>
#### <a name="task-1-exploring-the-aspnet-mvc-4-internet-application-template"></a><span data-ttu-id="fc507-653">Zadanie 1: Eksploracji szablon aplikacji internetowych platformy ASP.NET MVC 4</span><span class="sxs-lookup"><span data-stu-id="fc507-653">Task 1: Exploring the ASP.NET MVC 4 Internet Application Template</span></span>

1. <span data-ttu-id="fc507-654">Jeśli nie już otwarty, uruchom **VS Express for Web**</span><span class="sxs-lookup"><span data-stu-id="fc507-654">If not already open, start **VS Express for Web**</span></span>
2. <span data-ttu-id="fc507-655">Wybierz **pliku | Nowy | Projekt** polecenia menu.</span><span class="sxs-lookup"><span data-stu-id="fc507-655">Select the **File | New | Project** menu command.</span></span> <span data-ttu-id="fc507-656">W **nowy projekt** okno dialogowe, wybierz opcję **Visual C# | Web** szablonu w lewym okienku drzewa, a następnie wybierz pozycję **aplikacji sieci Web programu ASP.NET MVC 4**.</span><span class="sxs-lookup"><span data-stu-id="fc507-656">In the **New Project** dialog, select the **Visual C#|Web** template on the left pane tree, and choose the **ASP.NET MVC 4 Web Application**.</span></span> <span data-ttu-id="fc507-657">**Nazwa** projektu *MusicStore* i zaktualizuj **Nazwa rozwiązania** do *rozpocząć*, następnie wybierz lokalizację (lub pozostaw wartość domyślną) i kliknij przycisk **OK** .</span><span class="sxs-lookup"><span data-stu-id="fc507-657">**Name** the project *MusicStore* and update the **solution name** to *Begin*, then select a location (or leave the default) and click **OK**.</span></span>

    <span data-ttu-id="fc507-658">![Tworzenie nowego projektu platformy ASP.NET MVC 4](aspnet-mvc-4-fundamentals/_static/image36.png "tworzenia nowego projektu platformy ASP.NET MVC 4")</span><span class="sxs-lookup"><span data-stu-id="fc507-658">![Creating a new ASP.NET MVC 4 Project](aspnet-mvc-4-fundamentals/_static/image36.png "Creating a new ASP.NET MVC 4 Project")</span></span>

    <span data-ttu-id="fc507-659">*Tworzenie nowego projektu platformy ASP.NET MVC 4*</span><span class="sxs-lookup"><span data-stu-id="fc507-659">*Creating a new ASP.NET MVC 4 Project*</span></span>
3. <span data-ttu-id="fc507-660">W **nowy projekt programu ASP.NET MVC 4** okno dialogowe, wybierz opcję **aplikacji internetowej** szablonu projektu i kliknij przycisk **OK**.</span><span class="sxs-lookup"><span data-stu-id="fc507-660">In the **New ASP.NET MVC 4 Project** dialog, select the **Internet Application** project template and click **OK**.</span></span> <span data-ttu-id="fc507-661">Należy zauważyć, że można wybrać jako aparat widoku Razor lub ASPX.</span><span class="sxs-lookup"><span data-stu-id="fc507-661">Notice you can select either Razor or ASPX as the view engine.</span></span>

    <span data-ttu-id="fc507-662">![Tworzenie nowej aplikacji internetowych 4 ASP.NET MVC](aspnet-mvc-4-fundamentals/_static/image37.png "Tworzenie nowej aplikacji internetowych 4 ASP.NET MVC")</span><span class="sxs-lookup"><span data-stu-id="fc507-662">![Creating a new ASP.NET MVC 4 Internet Application](aspnet-mvc-4-fundamentals/_static/image37.png "Creating a new ASP.NET MVC 4 Internet Application")</span></span>

    <span data-ttu-id="fc507-663">*Tworzenie nowej aplikacji internetowych 4 ASP.NET MVC*</span><span class="sxs-lookup"><span data-stu-id="fc507-663">*Creating a new ASP.NET MVC 4 Internet Application*</span></span>

    > [!NOTE]
    > <span data-ttu-id="fc507-664">Składnia razor została wprowadzona w programie ASP.NET MVC 3.</span><span class="sxs-lookup"><span data-stu-id="fc507-664">Razor syntax has been introduced in ASP.NET MVC 3.</span></span> <span data-ttu-id="fc507-665">Jego celem jest zminimalizowanie liczby znaków i naciśnięcia klawiszy wymagane w pliku, umożliwiające szybkie oraz płynu kodowania przepływu pracy.</span><span class="sxs-lookup"><span data-stu-id="fc507-665">Its goal is to minimize the number of characters and keystrokes required in a file, enabling a fast and fluid coding workflow.</span></span> <span data-ttu-id="fc507-666">Razor wykorzystuje istniejące C# /VB (lub innych) umiejętności język, a następnie dostarcza składnia znacznika szablonu, umożliwiającą świetny przepływu pracy konstrukcji kodu HTML.</span><span class="sxs-lookup"><span data-stu-id="fc507-666">Razor leverages existing C#/VB (or other) language skills and delivers a template markup syntax that enables an awesome HTML construction workflow.</span></span>
4. <span data-ttu-id="fc507-667">Naciśnij klawisz **F5** uruchomić rozwiązanie i zobaczyć odnowionego szablonu.</span><span class="sxs-lookup"><span data-stu-id="fc507-667">Press **F5** to run the solution and see the renewed template.</span></span> <span data-ttu-id="fc507-668">Można wyewidencjonować następujące funkcje:</span><span class="sxs-lookup"><span data-stu-id="fc507-668">You can check out the following features:</span></span>

    1. <span data-ttu-id="fc507-669">**Szablonów w stylu Modern**</span><span class="sxs-lookup"><span data-stu-id="fc507-669">**Modern-style templates**</span></span>

        <span data-ttu-id="fc507-670">Szablony odnowienia, zapewniając więcej stylów nowoczesny wygląd.</span><span class="sxs-lookup"><span data-stu-id="fc507-670">The templates have been renewed, providing more modern-looking styles.</span></span>

        <span data-ttu-id="fc507-671">![Szablony ASP.NET MVC 4 restyled](aspnet-mvc-4-fundamentals/_static/image38.png "restyled szablony programu ASP.NET MVC 4")</span><span class="sxs-lookup"><span data-stu-id="fc507-671">![ASP.NET MVC 4 restyled templates](aspnet-mvc-4-fundamentals/_static/image38.png "ASP.NET MVC 4 restyled templates")</span></span>

        <span data-ttu-id="fc507-672">*Szablony ASP.NET MVC 4 restyled*</span><span class="sxs-lookup"><span data-stu-id="fc507-672">*ASP.NET MVC 4 restyled templates*</span></span>
    2. <span data-ttu-id="fc507-673">**Renderowanie adaptacyjną**</span><span class="sxs-lookup"><span data-stu-id="fc507-673">**Adaptive Rendering**</span></span>

        <span data-ttu-id="fc507-674">Zapoznaj się z zmiany rozmiaru okna przeglądarki i zwróć uwagę, jak dynamicznie dostosowuje układ strony do nowego rozmiaru okna.</span><span class="sxs-lookup"><span data-stu-id="fc507-674">Check out resizing the browser window and notice how the page layout dynamically adapts to the new window size.</span></span> <span data-ttu-id="fc507-675">Te szablony używać techniki adaptacyjną renderowania mają być renderowane poprawnie w zarówno wersje desktop i mobile platform bez potrzeby dostosowywania.</span><span class="sxs-lookup"><span data-stu-id="fc507-675">These templates use the adaptive rendering technique to render properly in both desktop and mobile platforms without any customization.</span></span>

        <span data-ttu-id="fc507-676">![Szablon projektu programu ASP.NET MVC 4 w innej przeglądarce rozmiary](aspnet-mvc-4-fundamentals/_static/image39.png "szablonu projektu programu ASP.NET MVC 4 w innej przeglądarce rozmiarach")</span><span class="sxs-lookup"><span data-stu-id="fc507-676">![ASP.NET MVC 4 project template in different browser sizes](aspnet-mvc-4-fundamentals/_static/image39.png "ASP.NET MVC 4 project template in different browser sizes")</span></span>

        <span data-ttu-id="fc507-677">*Szablon projektu programu ASP.NET MVC 4 w innej przeglądarce rozmiarach*</span><span class="sxs-lookup"><span data-stu-id="fc507-677">*ASP.NET MVC 4 project template in different browser sizes*</span></span>
5. <span data-ttu-id="fc507-678">Zamknij przeglądarkę, aby zatrzymać debuger i powrócić do programu Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="fc507-678">Close the browser to stop the debugger and return to Visual Studio.</span></span>
6. <span data-ttu-id="fc507-679">Teraz masz możliwość Eksploruj rozwiązania i zapoznaj się z nowych funkcji wprowadzonych przez program ASP.NET MVC 4 w szablonie projektu.</span><span class="sxs-lookup"><span data-stu-id="fc507-679">Now you are able to explore the solution and check out some of the new features introduced by ASP.NET MVC 4 in the project template.</span></span>

    <span data-ttu-id="fc507-680">![ASP.NET MVC4-internet-— szablon projektu aplikacji-](aspnet-mvc-4-fundamentals/_static/image40.png "szablonu projektu aplikacji internetowych platformy ASP.NET MVC 4")</span><span class="sxs-lookup"><span data-stu-id="fc507-680">![ASP.NET MVC4-internet-application-project-template](aspnet-mvc-4-fundamentals/_static/image40.png "The ASP.NET MVC 4 Internet Application Project Template")</span></span>

    <span data-ttu-id="fc507-681">*Szablon projektu aplikacji internetowych platformy ASP.NET MVC 4*</span><span class="sxs-lookup"><span data-stu-id="fc507-681">*The ASP.NET MVC 4 Internet Application Project Template*</span></span>

   1. <span data-ttu-id="fc507-682">**HTML5 znaczników**</span><span class="sxs-lookup"><span data-stu-id="fc507-682">**HTML5 markup**</span></span>

       <span data-ttu-id="fc507-683">Przeglądaj widoków szablonu, aby dowiedzieć się nowego znacznika motywu, na przykład otwórz **About.cshtml** wyświetlić w **Home** folderu.</span><span class="sxs-lookup"><span data-stu-id="fc507-683">Browse template views to find out the new theme markup, for example open **About.cshtml** view within **Home** folder.</span></span>

       <span data-ttu-id="fc507-684">![Nowy szablon, przy użyciu znaczników Razor i HTML5](aspnet-mvc-4-fundamentals/_static/image41.png "nowego szablonu, za pomocą znacznika Razor i HTML5")</span><span class="sxs-lookup"><span data-stu-id="fc507-684">![New template, using Razor and HTML5 markup](aspnet-mvc-4-fundamentals/_static/image41.png "New template, using Razor and HTML5 markup")</span></span>

       <span data-ttu-id="fc507-685">*Nowy szablon, przy użyciu znaczników Razor i HTML5*</span><span class="sxs-lookup"><span data-stu-id="fc507-685">*New template, using Razor and HTML5 markup*</span></span>
   2. <span data-ttu-id="fc507-686">**JavaScript biblioteki dołączony**</span><span class="sxs-lookup"><span data-stu-id="fc507-686">**JavaScript libraries included**</span></span>

      1. <span data-ttu-id="fc507-687">**jQuery**: jQuery ułatwia przechodzenie dokumentu HTML, obsługa zdarzeń animacji i interakcje Ajax.</span><span class="sxs-lookup"><span data-stu-id="fc507-687">**jQuery**: jQuery simplifies HTML document traversing, event handling, animating, and Ajax interactions.</span></span>
      2. <span data-ttu-id="fc507-688">**interfejsu użytkownika jQuery**: Ta biblioteka zawiera obiektów abstrakcyjnych odpowiadających niskiego poziomu interakcji i zaawansowana efekty animacji i elementów WebParts elementy widget, rozszerzający jQuery biblioteka języka JavaScript.</span><span class="sxs-lookup"><span data-stu-id="fc507-688">**jQuery UI**: This library provides abstractions for low-level interaction and animation, advanced effects and themeable widgets, built on top of the jQuery JavaScript Library.</span></span>

         > [!NOTE]
         > <span data-ttu-id="fc507-689">Informacje na temat jQuery i jQuery interfejsu użytkownika w [ [ http://docs.jquery.com/ ](http://docs.jquery.com/) ](http://docs.jquery.com/).</span><span class="sxs-lookup"><span data-stu-id="fc507-689">You can learn about jQuery and jQuery UI in [[http://docs.jquery.com/](http://docs.jquery.com/)](http://docs.jquery.com/).</span></span>
      3. <span data-ttu-id="fc507-690">**Elementami KnockoutJS**: szablon domyślny ASP.NET MVC 4 zawiera teraz **elementami KnockoutJS**, platforma JavaScript MVVM, która umożliwia tworzenie aplikacji sieci web bogaty i elastyczny wysokiej przy użyciu języka JavaScript i HTML.</span><span class="sxs-lookup"><span data-stu-id="fc507-690">**KnockoutJS**: The ASP.NET MVC 4 default template now includes **KnockoutJS**, a JavaScript MVVM framework that lets you create rich and highly responsive web applications using JavaScript and HTML.</span></span> <span data-ttu-id="fc507-691">Podobnie jak w programie ASP.NET MVC 3, jQuery i jQuery biblioteki interfejsu użytkownika znajdują się również w technologii ASP.NET MVC 4.</span><span class="sxs-lookup"><span data-stu-id="fc507-691">Like in ASP.NET MVC 3, jQuery and jQuery UI libraries are also included in ASP.NET MVC 4.</span></span>

          > [!NOTE]
          > <span data-ttu-id="fc507-692">Możesz uzyskać więcej informacji na temat biblioteki elementami KnockOutJS w to łącze: [ http://learn.knockoutjs.com/ ](http://learn.knockoutjs.com/).</span><span class="sxs-lookup"><span data-stu-id="fc507-692">You can get more information about KnockOutJS library in this link: [http://learn.knockoutjs.com/](http://learn.knockoutjs.com/).</span></span>
      4. <span data-ttu-id="fc507-693">**Modernizr**: Ta biblioteka jest uruchamiana automatycznie, co zgodne ze starszych przeglądarek witryny przy użyciu technologii HTML5 i CSS3.</span><span class="sxs-lookup"><span data-stu-id="fc507-693">**Modernizr**: This library runs automatically, making your site compatible with older browsers when using HTML5 and CSS3 technologies.</span></span>

          > [!NOTE]
          > <span data-ttu-id="fc507-694">Możesz uzyskać więcej informacji na temat biblioteki Modernizr w to łącze: [ http://www.modernizr.com/ ](http://www.modernizr.com/).</span><span class="sxs-lookup"><span data-stu-id="fc507-694">You can get more information about Modernizr library in this link: [http://www.modernizr.com/](http://www.modernizr.com/).</span></span>
   3. <span data-ttu-id="fc507-695">**SimpleMembership zawarty w rozwiązaniu**</span><span class="sxs-lookup"><span data-stu-id="fc507-695">**SimpleMembership included in the solution**</span></span>

       <span data-ttu-id="fc507-696">SimpleMembership został zaprojektowany jako serwer zamienny dla poprzedniego systemu dostawcy ról ASP.NET i członkostwa.</span><span class="sxs-lookup"><span data-stu-id="fc507-696">SimpleMembership has been designed as a replacement for the previous ASP.NET Role and Membership provider system.</span></span> <span data-ttu-id="fc507-697">Ma wiele nowych funkcji, które ułatwiają deweloperom bezpiecznych stron sieci web w sposób bardziej elastyczne.</span><span class="sxs-lookup"><span data-stu-id="fc507-697">It has many new features that make it easier for the developer to secure web pages in a more flexible way.</span></span>

       <span data-ttu-id="fc507-698">Szablon Internet już ma skonfigurować kilka ustawień integracji SimpleMembership, na przykład elementu AccountController jest przygotowane do korzystania z OAuthWebSecurity (w przypadku rejestracji konta OAuth, logowania, zarządzania, itp.) i bezpieczeństwo sieci Web.</span><span class="sxs-lookup"><span data-stu-id="fc507-698">The Internet template already has set up a few things to integrate SimpleMembership, for example, the AccountController is prepared to use OAuthWebSecurity (for OAuth account registration, login, management, etc.) and Web Security.</span></span>

       <span data-ttu-id="fc507-699">![SimpleMembership zawarty w rozwiązaniu](aspnet-mvc-4-fundamentals/_static/image42.png "SimpleMembership zawarty w rozwiązaniu")</span><span class="sxs-lookup"><span data-stu-id="fc507-699">![SimpleMembership Included in the solution](aspnet-mvc-4-fundamentals/_static/image42.png "SimpleMembership Included in the solution")</span></span>

       <span data-ttu-id="fc507-700">*SimpleMembership zawarty w rozwiązaniu*</span><span class="sxs-lookup"><span data-stu-id="fc507-700">*SimpleMembership Included in the solution*</span></span>

       > [!NOTE]
       > <span data-ttu-id="fc507-701">Aby uzyskać więcej informacji o [OAuthWebSecurity](https://msdn.microsoft.com/library/jj158393(v=vs.111).aspx) w witrynie MSDN.</span><span class="sxs-lookup"><span data-stu-id="fc507-701">Find more information about [OAuthWebSecurity](https://msdn.microsoft.com/library/jj158393(v=vs.111).aspx) in MSDN.</span></span>

> [!NOTE]
> <span data-ttu-id="fc507-702">Ponadto można wdrożyć tę aplikację systemu Windows Azure Web Sites następujących [dodatek B: publikowania aplikacji ASP.NET MVC 4 przy użyciu narzędzia Web Deploy](#AppendixB).</span><span class="sxs-lookup"><span data-stu-id="fc507-702">Additionally, you can deploy this application to Windows Azure Web Sites following [Appendix B: Publishing an ASP.NET MVC 4 Application using Web Deploy](#AppendixB).</span></span>


* * *

<a id="Summary"></a>

<a id="Summary"></a>
## <a name="summary"></a><span data-ttu-id="fc507-703">Podsumowanie</span><span class="sxs-lookup"><span data-stu-id="fc507-703">Summary</span></span>

<span data-ttu-id="fc507-704">Wykonując tego laboratorium Hands-On znasz już podstawy ASP.NET MVC:</span><span class="sxs-lookup"><span data-stu-id="fc507-704">By completing this Hands-On Lab you have learned the fundamentals of ASP.NET MVC:</span></span>

- <span data-ttu-id="fc507-705">Elementy podstawowe aplikacji MVC i sposób ich interakcji</span><span class="sxs-lookup"><span data-stu-id="fc507-705">The core elements of an MVC application and how they interact</span></span>
- <span data-ttu-id="fc507-706">Tworzenie aplikacji platformy ASP.NET MVC</span><span class="sxs-lookup"><span data-stu-id="fc507-706">How to create an ASP.NET MVC Application</span></span>
- <span data-ttu-id="fc507-707">Jak dodać i skonfigurować kontrolerów, aby obsłużyć parametry przekazywane przy użyciu adresu URL i querystring</span><span class="sxs-lookup"><span data-stu-id="fc507-707">How to add and configure Controllers to handle parameters passed through the URL and querystring</span></span>
- <span data-ttu-id="fc507-708">Jak dodać układ strony wzorcowej można skonfigurować szablon dla typowych zawartość HTML, arkusz stylów, aby zwiększyć wyglądu i działania i szablonu widoku, aby wyświetlić zawartość HTML</span><span class="sxs-lookup"><span data-stu-id="fc507-708">How to add a layout master page to setup a template for common HTML content, a StyleSheet to enhance the look and feel and a View template to display HTML content</span></span>
- <span data-ttu-id="fc507-709">Sposób użycia wzorca ViewModel przekazywania właściwości do szablonu widoku do wyświetlenia informacji dynamicznych</span><span class="sxs-lookup"><span data-stu-id="fc507-709">How to use the ViewModel pattern for passing properties to the View template to display dynamic information</span></span>
- <span data-ttu-id="fc507-710">Jak używać parametrów przekazanych do kontrolerów w szablonie widoku</span><span class="sxs-lookup"><span data-stu-id="fc507-710">How to use parameters passed to Controllers in the View template</span></span>
- <span data-ttu-id="fc507-711">Dodawanie łączy do stron w aplikacji ASP.NET MVC</span><span class="sxs-lookup"><span data-stu-id="fc507-711">How to add links to pages inside the ASP.NET MVC application</span></span>
- <span data-ttu-id="fc507-712">Jak dodać i używania właściwości dynamicznych w widoku</span><span class="sxs-lookup"><span data-stu-id="fc507-712">How to add and use dynamic properties in a View</span></span>
- <span data-ttu-id="fc507-713">Ulepszenia w szablonach projektu programu ASP.NET MVC 4</span><span class="sxs-lookup"><span data-stu-id="fc507-713">The enhancements in the ASP.NET MVC 4 project templates</span></span>

<a id="AppendixA"></a>

<a id="Appendix_A_Installing_Visual_Studio_Express_2012_for_Web"></a>
## <a name="appendix-a-installing-visual-studio-express-2012-for-web"></a><span data-ttu-id="fc507-714">Dodatek A: Instalowanie programu Visual Studio Express 2012 for Web</span><span class="sxs-lookup"><span data-stu-id="fc507-714">Appendix A: Installing Visual Studio Express 2012 for Web</span></span>

<span data-ttu-id="fc507-715">Można zainstalować **Microsoft Visual Studio Express 2012 for Web** lub innym &quot;Express&quot; przy użyciu wersji **[Instalatora platformy sieci Web firmy Microsoft](https://www.microsoft.com/web/downloads/platform.aspx)**.</span><span class="sxs-lookup"><span data-stu-id="fc507-715">You can install **Microsoft Visual Studio Express 2012 for Web** or another &quot;Express&quot; version using the **[Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)**.</span></span> <span data-ttu-id="fc507-716">Poniższe instrukcje przedstawiono czynności wymagane do zainstalowania *programu Visual studio Express 2012 for Web* przy użyciu *Instalatora platformy sieci Web firmy Microsoft*.</span><span class="sxs-lookup"><span data-stu-id="fc507-716">The following instructions guide you through the steps required to install *Visual studio Express 2012 for Web* using *Microsoft Web Platform Installer*.</span></span>

1. <span data-ttu-id="fc507-717">Przejdź do [ [ https://go.microsoft.com/?linkid=9810169 ](https://go.microsoft.com/?linkid=9810169) ](https://go.microsoft.com/?linkid=9810169).</span><span class="sxs-lookup"><span data-stu-id="fc507-717">Go to [[https://go.microsoft.com/?linkid=9810169](https://go.microsoft.com/?linkid=9810169)](https://go.microsoft.com/?linkid=9810169).</span></span> <span data-ttu-id="fc507-718">Alternatywnie, jeśli została już zainstalowana Instalatora platformy sieci Web, można otworzyć go i Wyszukaj produkt &quot; <em>programu Visual Studio Express 2012 for Web z zestawem Windows Azure SDK</em>&quot;.</span><span class="sxs-lookup"><span data-stu-id="fc507-718">Alternatively, if you already have installed Web Platform Installer, you can open it and search for the product &quot;<em>Visual Studio Express 2012 for Web with Windows Azure SDK</em>&quot;.</span></span>
2. <span data-ttu-id="fc507-719">Polecenie **teraz zainstalować**.</span><span class="sxs-lookup"><span data-stu-id="fc507-719">Click on **Install Now**.</span></span> <span data-ttu-id="fc507-720">Jeśli nie masz **Instalatora platformy sieci Web** nastąpi przekierowanie do pobrania i zainstalowania go najpierw.</span><span class="sxs-lookup"><span data-stu-id="fc507-720">If you do not have **Web Platform Installer** you will be redirected to download and install it first.</span></span>
3. <span data-ttu-id="fc507-721">Raz **Instalatora platformy sieci Web** jest otwarty, kliknij przycisk **zainstalować** można uruchomić Instalatora.</span><span class="sxs-lookup"><span data-stu-id="fc507-721">Once **Web Platform Installer** is open, click **Install** to start the setup.</span></span>

    <span data-ttu-id="fc507-722">![Instalowanie programu Visual Studio Express](aspnet-mvc-4-fundamentals/_static/image43.png "instalacji programu Visual Studio Express")</span><span class="sxs-lookup"><span data-stu-id="fc507-722">![Install Visual Studio Express](aspnet-mvc-4-fundamentals/_static/image43.png "Install Visual Studio Express")</span></span>

    <span data-ttu-id="fc507-723">*Instalowanie programu Visual Studio Express*</span><span class="sxs-lookup"><span data-stu-id="fc507-723">*Install Visual Studio Express*</span></span>
4. <span data-ttu-id="fc507-724">Odczytywanie wszystkich produktów licencji i warunków, a następnie kliknij przycisk **akceptuję** aby kontynuować.</span><span class="sxs-lookup"><span data-stu-id="fc507-724">Read all the products' licenses and terms and click **I Accept** to continue.</span></span>

    ![Akceptowanie umowy licencyjnej](aspnet-mvc-4-fundamentals/_static/image44.png)

    <span data-ttu-id="fc507-726">*Akceptowanie umowy licencyjnej*</span><span class="sxs-lookup"><span data-stu-id="fc507-726">*Accepting the license terms*</span></span>
5. <span data-ttu-id="fc507-727">Poczekaj na zakończenie procesu pobierania i instalacji.</span><span class="sxs-lookup"><span data-stu-id="fc507-727">Wait until the downloading and installation process completes.</span></span>

    ![Postęp instalacji](aspnet-mvc-4-fundamentals/_static/image45.png)

    <span data-ttu-id="fc507-729">*Postęp instalacji*</span><span class="sxs-lookup"><span data-stu-id="fc507-729">*Installation progress*</span></span>
6. <span data-ttu-id="fc507-730">Po zakończeniu instalacji kliknij przycisk **Zakończ**.</span><span class="sxs-lookup"><span data-stu-id="fc507-730">When the installation completes, click **Finish**.</span></span>

    ![Instalacja została zakończona](aspnet-mvc-4-fundamentals/_static/image46.png)

    <span data-ttu-id="fc507-732">*Instalacja została zakończona*</span><span class="sxs-lookup"><span data-stu-id="fc507-732">*Installation completed*</span></span>
7. <span data-ttu-id="fc507-733">Kliknij przycisk **zakończenia** aby zamknąć Instalatora platformy sieci Web.</span><span class="sxs-lookup"><span data-stu-id="fc507-733">Click **Exit** to close Web Platform Installer.</span></span>
8. <span data-ttu-id="fc507-734">Aby otworzyć program Visual Studio Express for Web, przejdź do **Start** ekranu i zacznij pisać &quot; **VS Express**&quot;, następnie kliknij polecenie **VS Express for Web** Kafelek.</span><span class="sxs-lookup"><span data-stu-id="fc507-734">To open Visual Studio Express for Web, go to the **Start** screen and start writing &quot;**VS Express**&quot;, then click on the **VS Express for Web** tile.</span></span>

    ![VS Express for Web kafelka](aspnet-mvc-4-fundamentals/_static/image47.png)

    <span data-ttu-id="fc507-736">*VS Express for Web kafelka*</span><span class="sxs-lookup"><span data-stu-id="fc507-736">*VS Express for Web tile*</span></span>

<a id="AppendixB"></a>

<a id="Appendix_B_Publishing_an_ASPNET_MVC_4_Application_using_Web_Deploy"></a>
## <a name="appendix-b-publishing-an-aspnet-mvc-4-application-using-web-deploy"></a><span data-ttu-id="fc507-737">Dodatek B: publikowania aplikacji ASP.NET MVC 4 przy użyciu narzędzia Web Deploy</span><span class="sxs-lookup"><span data-stu-id="fc507-737">Appendix B: Publishing an ASP.NET MVC 4 Application using Web Deploy</span></span>

<span data-ttu-id="fc507-738">Ten dodatek opisano sposób tworzenia nowej witryny sieci web z portalu zarządzania pakietu Windows Azure i publikowanie aplikacji, uzyskane wykonując laboratorium, korzystając z funkcji publikowania narzędzia Web Deploy dostarczane przez Windows Azure.</span><span class="sxs-lookup"><span data-stu-id="fc507-738">This appendix will show you how to create a new web site from the Windows Azure Management Portal and publish the application you obtained by following the lab, taking advantage of the Web Deploy publishing feature provided by Windows Azure.</span></span>

<a id="ApxBTask1"></a>

<a id="Task_1_-_Creating_a_New_Web_Site_from_the_Windows_Azure_Portal"></a>
#### <a name="task-1---creating-a-new-web-site-from-the-windows-azure-portal"></a><span data-ttu-id="fc507-739">Zadanie 1 — Tworzenie nowej witryny sieci Web systemu Windows portalu Azure</span><span class="sxs-lookup"><span data-stu-id="fc507-739">Task 1 - Creating a New Web Site from the Windows Azure Portal</span></span>

1. <span data-ttu-id="fc507-740">Przejdź do [portalu zarządzania pakietu Windows Azure](https://manage.windowsazure.com/) i zaloguj się przy użyciu poświadczeń Microsoft skojarzonych z Twoją subskrypcją.</span><span class="sxs-lookup"><span data-stu-id="fc507-740">Go to the [Windows Azure Management Portal](https://manage.windowsazure.com/) and sign in using the Microsoft credentials associated with your subscription.</span></span>

    > [!NOTE]
    > <span data-ttu-id="fc507-741">Z systemu Windows Azure można udostępniać 10 witryn sieci Web platformy ASP.NET bezpłatnie i następnie Skaluj w miarę zwiększania się ruchu.</span><span class="sxs-lookup"><span data-stu-id="fc507-741">With Windows Azure you can host 10 ASP.NET Web Sites for free and then scale as your traffic grows.</span></span> <span data-ttu-id="fc507-742">Możesz utworzyć konto [tutaj](http://aka.ms/aspnet-hol-azure).</span><span class="sxs-lookup"><span data-stu-id="fc507-742">You can sign up [here](http://aka.ms/aspnet-hol-azure).</span></span>

    <span data-ttu-id="fc507-743">![Zaloguj się do portalu Windows Azure](aspnet-mvc-4-fundamentals/_static/image48.png "Zaloguj się do portalu Windows Azure")</span><span class="sxs-lookup"><span data-stu-id="fc507-743">![Log on to Windows Azure portal](aspnet-mvc-4-fundamentals/_static/image48.png "Log on to Windows Azure portal")</span></span>

    <span data-ttu-id="fc507-744">*Zaloguj się do portalu zarządzania platformy Azure z systemem Windows*</span><span class="sxs-lookup"><span data-stu-id="fc507-744">*Log on to Windows Azure Management Portal*</span></span>
2. <span data-ttu-id="fc507-745">Kliknij przycisk **nowy** na pasku poleceń.</span><span class="sxs-lookup"><span data-stu-id="fc507-745">Click **New** on the command bar.</span></span>

    <span data-ttu-id="fc507-746">![Tworzenie nowej witryny sieci Web](aspnet-mvc-4-fundamentals/_static/image49.png "tworzenia nowej witryny sieci Web")</span><span class="sxs-lookup"><span data-stu-id="fc507-746">![Creating a new Web Site](aspnet-mvc-4-fundamentals/_static/image49.png "Creating a new Web Site")</span></span>

    <span data-ttu-id="fc507-747">*Tworzenie nowej witryny sieci Web*</span><span class="sxs-lookup"><span data-stu-id="fc507-747">*Creating a new Web Site*</span></span>
3. <span data-ttu-id="fc507-748">Kliknij przycisk **obliczeniowe** | **witryny sieci Web**.</span><span class="sxs-lookup"><span data-stu-id="fc507-748">Click **Compute** | **Web Site**.</span></span> <span data-ttu-id="fc507-749">Następnie wybierz **szybkie tworzenie** opcji.</span><span class="sxs-lookup"><span data-stu-id="fc507-749">Then select **Quick Create** option.</span></span> <span data-ttu-id="fc507-750">Podaj dostępny adres URL dla nowej witryny sieci web, a następnie kliknij przycisk **tworzenie witryny sieci Web**.</span><span class="sxs-lookup"><span data-stu-id="fc507-750">Provide an available URL for the new web site and click **Create Web Site**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="fc507-751">Witryny sieci Web systemu Windows Azure jest hostem dla aplikacji sieci web w chmurze, które można kontrolować i zarządzanie nimi.</span><span class="sxs-lookup"><span data-stu-id="fc507-751">A Windows Azure Web Site is the host for a web application running in the cloud that you can control and manage.</span></span> <span data-ttu-id="fc507-752">Opcja szybkie tworzenie umożliwia wdrażanie ukończonej aplikacji sieci web do systemu Windows Azure witryny internetowej z spoza portalu.</span><span class="sxs-lookup"><span data-stu-id="fc507-752">The Quick Create option allows you to deploy a completed web application to the Windows Azure Web Site from outside the portal.</span></span> <span data-ttu-id="fc507-753">Nie obejmuje kroki konfigurowania bazy danych.</span><span class="sxs-lookup"><span data-stu-id="fc507-753">It does not include steps for setting up a database.</span></span>

    <span data-ttu-id="fc507-754">![Tworzenie nowej witryny sieci Web przy użyciu szybkie tworzenie](aspnet-mvc-4-fundamentals/_static/image50.png "tworzenia nowej witryny sieci Web przy użyciu szybkie tworzenie")</span><span class="sxs-lookup"><span data-stu-id="fc507-754">![Creating a new Web Site using Quick Create](aspnet-mvc-4-fundamentals/_static/image50.png "Creating a new Web Site using Quick Create")</span></span>

    <span data-ttu-id="fc507-755">*Tworzenie nowej witryny sieci Web przy użyciu szybkie tworzenie*</span><span class="sxs-lookup"><span data-stu-id="fc507-755">*Creating a new Web Site using Quick Create*</span></span>
4. <span data-ttu-id="fc507-756">Poczekaj na nowe **witryny sieci Web** jest tworzony.</span><span class="sxs-lookup"><span data-stu-id="fc507-756">Wait until the new **Web Site** is created.</span></span>
5. <span data-ttu-id="fc507-757">Po utworzeniu witryny sieci Web kliknij łącze w obszarze **adres URL** kolumny.</span><span class="sxs-lookup"><span data-stu-id="fc507-757">Once the Web Site is created click the link under the **URL** column.</span></span> <span data-ttu-id="fc507-758">Sprawdź, czy działa nowej witryny sieci Web.</span><span class="sxs-lookup"><span data-stu-id="fc507-758">Check that the new Web Site is working.</span></span>

    <span data-ttu-id="fc507-759">![Przeglądanie do nowej witryny sieci web](aspnet-mvc-4-fundamentals/_static/image51.png "przeglądanie do nowej witryny sieci web")</span><span class="sxs-lookup"><span data-stu-id="fc507-759">![Browsing to the new web site](aspnet-mvc-4-fundamentals/_static/image51.png "Browsing to the new web site")</span></span>

    <span data-ttu-id="fc507-760">*Przeglądanie do nowej witryny sieci web*</span><span class="sxs-lookup"><span data-stu-id="fc507-760">*Browsing to the new web site*</span></span>

    <span data-ttu-id="fc507-761">![Witryna sieci Web działa](aspnet-mvc-4-fundamentals/_static/image52.png "uruchamiania witryny sieci Web")</span><span class="sxs-lookup"><span data-stu-id="fc507-761">![Web site running](aspnet-mvc-4-fundamentals/_static/image52.png "Web site running")</span></span>

    <span data-ttu-id="fc507-762">*Witryna sieci Web uruchomiona*</span><span class="sxs-lookup"><span data-stu-id="fc507-762">*Web site running*</span></span>
6. <span data-ttu-id="fc507-763">Wróć do portalu i kliknij nazwę witryny sieci web w obszarze **nazwa** kolumny do wyświetlenia strony zarządzania.</span><span class="sxs-lookup"><span data-stu-id="fc507-763">Go back to the portal and click the name of the web site under the **Name** column to display the management pages.</span></span>

    <span data-ttu-id="fc507-764">![Otwieranie stron witryny sieci web zarządzania](aspnet-mvc-4-fundamentals/_static/image53.png "otwieranie stron zarządzania witryny sieci web")</span><span class="sxs-lookup"><span data-stu-id="fc507-764">![Opening the web site management pages](aspnet-mvc-4-fundamentals/_static/image53.png "Opening the web site management pages")</span></span>

    <span data-ttu-id="fc507-765">*Otwieranie stron zarządzania witryny sieci Web*</span><span class="sxs-lookup"><span data-stu-id="fc507-765">*Opening the Web Site management pages*</span></span>
7. <span data-ttu-id="fc507-766">W **pulpitu nawigacyjnego** w obszarze **szybkiego dostępu** kliknij **pobieranie profilu publikowania** łącza.</span><span class="sxs-lookup"><span data-stu-id="fc507-766">In the **Dashboard** page, under the **quick glance** section, click the **Download publish profile** link.</span></span>

    > [!NOTE]
    > <span data-ttu-id="fc507-767">*Profilu publikowania* zawiera wszystkie informacje wymagane do publikowania aplikacji sieci web do witryny sieci Web systemu Windows Azure dla każdej metody włączone publikacji.</span><span class="sxs-lookup"><span data-stu-id="fc507-767">The *publish profile* contains all of the information required to publish a web application to a Windows Azure website for each enabled publication method.</span></span> <span data-ttu-id="fc507-768">Profil publikowania zawiera adresy URL, poświadczenia użytkownika i parametry bazy danych wymagane do nawiązania połączenia i uwierzytelniania dla każdego z punktów końcowych, dla których włączono metoda publikacji.</span><span class="sxs-lookup"><span data-stu-id="fc507-768">The publish profile contains the URLs, user credentials and database strings required to connect to and authenticate against each of the endpoints for which a publication method is enabled.</span></span> <span data-ttu-id="fc507-769">**Microsoft WebMatrix 2**, **Microsoft Visual Studio Express for Web** i **programu Microsoft Visual Studio 2012** obsługują odczytywanie publikowanie profile do zautomatyzowania te programy Publikowanie aplikacji sieci web do witryn sieci Web systemu Windows Azure.</span><span class="sxs-lookup"><span data-stu-id="fc507-769">**Microsoft WebMatrix 2**, **Microsoft Visual Studio Express for Web** and **Microsoft Visual Studio 2012** support reading publish profiles to automate configuration of these programs for publishing web applications to Windows Azure websites.</span></span>

    <span data-ttu-id="fc507-770">![Pobieranie witryny sieci web profilu publikowania](aspnet-mvc-4-fundamentals/_static/image54.png "pobierania witryny sieci web profilu publikowania")</span><span class="sxs-lookup"><span data-stu-id="fc507-770">![Downloading the web site publish profile](aspnet-mvc-4-fundamentals/_static/image54.png "Downloading the web site publish profile")</span></span>

    <span data-ttu-id="fc507-771">*Pobieranie witryny sieci Web profilu publikowania*</span><span class="sxs-lookup"><span data-stu-id="fc507-771">*Downloading the Web Site publish profile*</span></span>
8. <span data-ttu-id="fc507-772">Pobierz profil publikowania w znanej lokalizacji.</span><span class="sxs-lookup"><span data-stu-id="fc507-772">Download the publish profile file to a known location.</span></span> <span data-ttu-id="fc507-773">Dodatkowo w tym ćwiczeniu zobaczysz jak opublikować aplikację sieci web do witryny sieci Web systemu Windows Azure w programie Visual Studio przy użyciu tego pliku.</span><span class="sxs-lookup"><span data-stu-id="fc507-773">Further in this exercise you will see how to use this file to publish a web application to a Windows Azure Web Sites from Visual Studio.</span></span>

    <span data-ttu-id="fc507-774">![Zapisywanie pliku profilu publikowania](aspnet-mvc-4-fundamentals/_static/image55.png "zapisywanie profilu publikowania")</span><span class="sxs-lookup"><span data-stu-id="fc507-774">![Saving the publish profile file](aspnet-mvc-4-fundamentals/_static/image55.png "Saving the publish profile")</span></span>

    <span data-ttu-id="fc507-775">*Zapisywanie pliku profilu publikowania*</span><span class="sxs-lookup"><span data-stu-id="fc507-775">*Saving the publish profile file*</span></span>

<a id="ApxBTask2"></a>

<a id="Task_2_-_Configuring_the_Database_Server"></a>
#### <a name="task-2---configuring-the-database-server"></a><span data-ttu-id="fc507-776">Zadanie 2 — Konfigurowanie serwera bazy danych</span><span class="sxs-lookup"><span data-stu-id="fc507-776">Task 2 - Configuring the Database Server</span></span>

<span data-ttu-id="fc507-777">Jeśli aplikacja korzysta z programu SQL Server baz danych, należy utworzyć serwer bazy danych SQL.</span><span class="sxs-lookup"><span data-stu-id="fc507-777">If your application makes use of SQL Server databases you will need to create a SQL Database server.</span></span> <span data-ttu-id="fc507-778">Jeśli chcesz wdrożyć prostą aplikację, która nie korzysta z programu SQL Server może pominąć to zadanie.</span><span class="sxs-lookup"><span data-stu-id="fc507-778">If you want to deploy a simple application that does not use SQL Server you might skip this task.</span></span>

1. <span data-ttu-id="fc507-779">Będzie potrzebny serwer bazy danych SQL do przechowywania bazy danych aplikacji.</span><span class="sxs-lookup"><span data-stu-id="fc507-779">You will need a SQL Database server for storing the application database.</span></span> <span data-ttu-id="fc507-780">Można wyświetlić serwery bazy danych SQL z subskrypcji usługi Windows Azure Management Portal pod adresem **baz danych Sql** | **serwerów** | **serwera Pulpit nawigacyjny**.</span><span class="sxs-lookup"><span data-stu-id="fc507-780">You can view the SQL Database servers from your subscription in the Windows Azure Management portal at **Sql Databases** | **Servers** | **Server's Dashboard**.</span></span> <span data-ttu-id="fc507-781">Jeśli nie masz serwer, który został utworzony, można utworzyć przy użyciu jednego **Dodaj** przycisk paska poleceń.</span><span class="sxs-lookup"><span data-stu-id="fc507-781">If you do not have a server created, you can create one using the **Add** button on the command bar.</span></span> <span data-ttu-id="fc507-782">Zwróć uwagę na **nazwę serwera i adres URL, nazwę logowania administratora i hasła**, jak będą używane w następnego zadania.</span><span class="sxs-lookup"><span data-stu-id="fc507-782">Take note of the **server name and URL, administrator login name and password**, as you will use them in the next tasks.</span></span> <span data-ttu-id="fc507-783">Nie należy tworzyć bazy danych jeszcze, jako zostaną utworzone w późniejszym terminie.</span><span class="sxs-lookup"><span data-stu-id="fc507-783">Do not create the database yet, as it will be created in a later stage.</span></span>

    <span data-ttu-id="fc507-784">![Pulpit nawigacyjny serwera bazy danych SQL](aspnet-mvc-4-fundamentals/_static/image56.png "pulpitu nawigacyjnego serwera bazy danych SQL")</span><span class="sxs-lookup"><span data-stu-id="fc507-784">![SQL Database Server Dashboard](aspnet-mvc-4-fundamentals/_static/image56.png "SQL Database Server Dashboard")</span></span>

    <span data-ttu-id="fc507-785">*Pulpit nawigacyjny serwera bazy danych SQL*</span><span class="sxs-lookup"><span data-stu-id="fc507-785">*SQL Database Server Dashboard*</span></span>
2. <span data-ttu-id="fc507-786">W następnym zadaniem Testuj połączenie z bazą danych z programu Visual Studio z tego powodu należy uwzględnić lokalny adres IP serwera liście **dozwolone adresy IP**.</span><span class="sxs-lookup"><span data-stu-id="fc507-786">In the next task you will test the database connection from Visual Studio, for that reason you need to include your local IP address in the server's list of **Allowed IP Addresses**.</span></span> <span data-ttu-id="fc507-787">Aby to zrobić, kliknij przycisk **Konfiguruj**, wybierz adres IP z **bieżącego adresu IP klienta** i wklej go na **początkowy adres IP** i **końcowy adres IP** pola tekstowe i kliknij przycisk ![add-client-ip-address-ok-button](aspnet-mvc-4-fundamentals/_static/image57.png) przycisku.</span><span class="sxs-lookup"><span data-stu-id="fc507-787">To do that, click **Configure**, select the IP address from **Current Client IP Address** and paste it on the **Start IP Address** and **End IP Address** text boxes and click the ![add-client-ip-address-ok-button](aspnet-mvc-4-fundamentals/_static/image57.png) button.</span></span>

    ![Dodawanie adresu IP klienta](aspnet-mvc-4-fundamentals/_static/image58.png)

    <span data-ttu-id="fc507-789">*Dodawanie adresu IP klienta*</span><span class="sxs-lookup"><span data-stu-id="fc507-789">*Adding Client IP Address*</span></span>
3. <span data-ttu-id="fc507-790">Raz **adres IP klienta** jest dodawany do dozwolonych adresów IP kliknij na **zapisać** o potwierdzenie zmian.</span><span class="sxs-lookup"><span data-stu-id="fc507-790">Once the **Client IP Address** is added to the allowed IP addresses list, click on **Save** to confirm the changes.</span></span>

    ![Potwierdzenie zmian](aspnet-mvc-4-fundamentals/_static/image59.png)

    <span data-ttu-id="fc507-792">*Potwierdzenie zmian*</span><span class="sxs-lookup"><span data-stu-id="fc507-792">*Confirm Changes*</span></span>

<a id="ApxBTask3"></a>

<a id="Task_3_-_Publishing_an_ASPNET_MVC_4_Application_using_Web_Deploy"></a>
#### <a name="task-3---publishing-an-aspnet-mvc-4-application-using-web-deploy"></a><span data-ttu-id="fc507-793">Zadanie 3 - publikowania aplikacji ASP.NET MVC 4 przy użyciu narzędzia Web Deploy</span><span class="sxs-lookup"><span data-stu-id="fc507-793">Task 3 - Publishing an ASP.NET MVC 4 Application using Web Deploy</span></span>

1. <span data-ttu-id="fc507-794">Wróć do rozwiązania ASP.NET MVC 4.</span><span class="sxs-lookup"><span data-stu-id="fc507-794">Go back to the ASP.NET MVC 4 solution.</span></span> <span data-ttu-id="fc507-795">W **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy projekt witryny sieci web i wybierz **publikowania**.</span><span class="sxs-lookup"><span data-stu-id="fc507-795">In the **Solution Explorer**, right-click the web site project and select **Publish**.</span></span>

    <span data-ttu-id="fc507-796">![Publikowanie aplikacji](aspnet-mvc-4-fundamentals/_static/image60.png "publikowania aplikacji")</span><span class="sxs-lookup"><span data-stu-id="fc507-796">![Publishing the Application](aspnet-mvc-4-fundamentals/_static/image60.png "Publishing the Application")</span></span>

    <span data-ttu-id="fc507-797">*Publikowanie witryny sieci web*</span><span class="sxs-lookup"><span data-stu-id="fc507-797">*Publishing the web site*</span></span>
2. <span data-ttu-id="fc507-798">Zaimportuj profil publikowania, zapisana w pierwszym zadaniu.</span><span class="sxs-lookup"><span data-stu-id="fc507-798">Import the publish profile you saved in the first task.</span></span>

    <span data-ttu-id="fc507-799">![Importowanie profilu publikowania](aspnet-mvc-4-fundamentals/_static/image61.png "Importowanie profilu publikowania")</span><span class="sxs-lookup"><span data-stu-id="fc507-799">![Importing the publish profile](aspnet-mvc-4-fundamentals/_static/image61.png "Importing the publish profile")</span></span>

    <span data-ttu-id="fc507-800">*Importowanie profilu publikowania*</span><span class="sxs-lookup"><span data-stu-id="fc507-800">*Importing publish profile*</span></span>
3. <span data-ttu-id="fc507-801">Kliknij przycisk **Weryfikacja połączenia z**.</span><span class="sxs-lookup"><span data-stu-id="fc507-801">Click **Validate Connection**.</span></span> <span data-ttu-id="fc507-802">Po zakończeniu sprawdzania kliknij **dalej**.</span><span class="sxs-lookup"><span data-stu-id="fc507-802">Once Validation is complete click **Next**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="fc507-803">Zakończeniu sprawdzania poprawności, gdy zostanie wyświetlony zielony znacznik wyboru są wyświetlane obok przycisku sprawdzania poprawności połączenia.</span><span class="sxs-lookup"><span data-stu-id="fc507-803">Validation is complete once you see a green checkmark appear next to the Validate Connection button.</span></span>

    <span data-ttu-id="fc507-804">![Sprawdzanie poprawności połączenia](aspnet-mvc-4-fundamentals/_static/image62.png "sprawdzanie poprawności połączenia")</span><span class="sxs-lookup"><span data-stu-id="fc507-804">![Validating connection](aspnet-mvc-4-fundamentals/_static/image62.png "Validating connection")</span></span>

    <span data-ttu-id="fc507-805">*Sprawdzanie poprawności połączenia*</span><span class="sxs-lookup"><span data-stu-id="fc507-805">*Validating connection*</span></span>
4. <span data-ttu-id="fc507-806">W **ustawienia** w obszarze **baz danych** sekcji, kliknij przycisk Dalej, aby textbox połączenia bazy danych (tj. **połączenia DefaultConnection**).</span><span class="sxs-lookup"><span data-stu-id="fc507-806">In the **Settings** page, under the **Databases** section, click the button next to your database connection's textbox (i.e. **DefaultConnection**).</span></span>

    <span data-ttu-id="fc507-807">![Konfiguracja narzędzia Web deploy](aspnet-mvc-4-fundamentals/_static/image63.png "Konfiguracja narzędzia Web deploy")</span><span class="sxs-lookup"><span data-stu-id="fc507-807">![Web deploy configuration](aspnet-mvc-4-fundamentals/_static/image63.png "Web deploy configuration")</span></span>

    <span data-ttu-id="fc507-808">*Konfiguracja narzędzia Web deploy*</span><span class="sxs-lookup"><span data-stu-id="fc507-808">*Web deploy configuration*</span></span>
5. <span data-ttu-id="fc507-809">Skonfiguruj połączenie z bazą danych w następujący sposób:</span><span class="sxs-lookup"><span data-stu-id="fc507-809">Configure the database connection as follows:</span></span>

   - <span data-ttu-id="fc507-810">W **nazwy serwera** wpisz swoją bazą danych SQL server adresu URL przy użyciu *tcp:* prefiks.</span><span class="sxs-lookup"><span data-stu-id="fc507-810">In the **Server name** type your SQL Database server URL using the *tcp:* prefix.</span></span>
   - <span data-ttu-id="fc507-811">W **nazwy użytkownika** wpisz nazwę logowania administratora serwera.</span><span class="sxs-lookup"><span data-stu-id="fc507-811">In **User name** type your server administrator login name.</span></span>
   - <span data-ttu-id="fc507-812">W **hasło** wpisz hasło logowania administratora serwera.</span><span class="sxs-lookup"><span data-stu-id="fc507-812">In **Password** type your server administrator login password.</span></span>
   - <span data-ttu-id="fc507-813">Wpisz nazwę nowej bazy danych, na przykład: *MVC4SampleDB*.</span><span class="sxs-lookup"><span data-stu-id="fc507-813">Type a new database name, for example: *MVC4SampleDB*.</span></span>

     <span data-ttu-id="fc507-814">![Konfigurowanie parametrów połączenia z lokalizacją docelową](aspnet-mvc-4-fundamentals/_static/image64.png "Konfigurowanie parametrów połączenia z lokalizacją docelową")</span><span class="sxs-lookup"><span data-stu-id="fc507-814">![Configuring destination connection string](aspnet-mvc-4-fundamentals/_static/image64.png "Configuring destination connection string")</span></span>

     <span data-ttu-id="fc507-815">*Konfigurowanie parametrów połączenia z lokalizacją docelową*</span><span class="sxs-lookup"><span data-stu-id="fc507-815">*Configuring destination connection string*</span></span>
6. <span data-ttu-id="fc507-816">Następnie kliknij przycisk **OK**.</span><span class="sxs-lookup"><span data-stu-id="fc507-816">Then click **OK**.</span></span> <span data-ttu-id="fc507-817">Po wyświetleniu monitu można utworzyć bazy danych kliknij **tak**.</span><span class="sxs-lookup"><span data-stu-id="fc507-817">When prompted to create the database click **Yes**.</span></span>

    <span data-ttu-id="fc507-818">![Tworzenie bazy danych](aspnet-mvc-4-fundamentals/_static/image65.png "tworzenie parametry bazy danych")</span><span class="sxs-lookup"><span data-stu-id="fc507-818">![Creating the database](aspnet-mvc-4-fundamentals/_static/image65.png "Creating the database string")</span></span>

    <span data-ttu-id="fc507-819">*Tworzenie bazy danych*</span><span class="sxs-lookup"><span data-stu-id="fc507-819">*Creating the database*</span></span>
7. <span data-ttu-id="fc507-820">Ciągu połączenia używanego do łączenia z bazą danych SQL w systemie Windows Azure jest wyświetlany w pole tekstowe domyślne połączenie.</span><span class="sxs-lookup"><span data-stu-id="fc507-820">The connection string you will use to connect to SQL Database in Windows Azure is shown within Default Connection textbox.</span></span> <span data-ttu-id="fc507-821">Następnie kliknij przycisk **Dalej**.</span><span class="sxs-lookup"><span data-stu-id="fc507-821">Then click **Next**.</span></span>

    <span data-ttu-id="fc507-822">![Parametry połączenia wskazujące bazę danych SQL](aspnet-mvc-4-fundamentals/_static/image66.png "ciąg połączenia wskazujące bazę danych SQL")</span><span class="sxs-lookup"><span data-stu-id="fc507-822">![Connection string pointing to SQL Database](aspnet-mvc-4-fundamentals/_static/image66.png "Connection string pointing to SQL Database")</span></span>

    <span data-ttu-id="fc507-823">*Parametry połączenia wskazujące bazę danych SQL*</span><span class="sxs-lookup"><span data-stu-id="fc507-823">*Connection string pointing to SQL Database*</span></span>
8. <span data-ttu-id="fc507-824">W **Podgląd** kliknij przycisk **publikowania**.</span><span class="sxs-lookup"><span data-stu-id="fc507-824">In the **Preview** page, click **Publish**.</span></span>

    <span data-ttu-id="fc507-825">![Publikowanie aplikacji sieci web](aspnet-mvc-4-fundamentals/_static/image67.png "publikowania aplikacji sieci web")</span><span class="sxs-lookup"><span data-stu-id="fc507-825">![Publishing the web application](aspnet-mvc-4-fundamentals/_static/image67.png "Publishing the web application")</span></span>

    <span data-ttu-id="fc507-826">*Publikowanie aplikacji sieci web*</span><span class="sxs-lookup"><span data-stu-id="fc507-826">*Publishing the web application*</span></span>
9. <span data-ttu-id="fc507-827">Po zakończeniu procesu publikowania domyślnej przeglądarce otworzy opublikowanej witryny sieci web.</span><span class="sxs-lookup"><span data-stu-id="fc507-827">Once the publishing process finishes, your default browser will open the published web site.</span></span>

    <span data-ttu-id="fc507-828">![Aplikacja opublikowana w systemie Windows Azure](aspnet-mvc-4-fundamentals/_static/image68.png "aplikacji publikowanych w systemie Windows Azure")</span><span class="sxs-lookup"><span data-stu-id="fc507-828">![Application published to Windows Azure](aspnet-mvc-4-fundamentals/_static/image68.png "Application published to Windows Azure")</span></span>

    <span data-ttu-id="fc507-829">*Aplikacji publikowanych w systemie Windows Azure*</span><span class="sxs-lookup"><span data-stu-id="fc507-829">*Application published to Windows Azure*</span></span>

<a id="AppendixC"></a>

<a id="Appendix_C_Using_Code_Snippets"></a>
## <a name="appendix-c-using-code-snippets"></a><span data-ttu-id="fc507-830">Dodatek C: korzystania z wstawek kodu</span><span class="sxs-lookup"><span data-stu-id="fc507-830">Appendix C: Using Code Snippets</span></span>

<span data-ttu-id="fc507-831">Wstawki kodu zapewniają całego kodu, które są potrzebne w zasięgu ręki.</span><span class="sxs-lookup"><span data-stu-id="fc507-831">With code snippets, you have all the code you need at your fingertips.</span></span> <span data-ttu-id="fc507-832">Dokument laboratorium informuje o dokładnie po ich użycia, jak pokazano na poniższej ilustracji.</span><span class="sxs-lookup"><span data-stu-id="fc507-832">The lab document will tell you exactly when you can use them, as shown in the following figure.</span></span>

<span data-ttu-id="fc507-833">![Aby wstawić kod do projektu przy użyciu wstawki kodu programu Visual Studio](aspnet-mvc-4-fundamentals/_static/image69.png "wstawki kodu za pomocą programu Visual Studio, aby wstawić kod do projektu")</span><span class="sxs-lookup"><span data-stu-id="fc507-833">![Using Visual Studio code snippets to insert code into your project](aspnet-mvc-4-fundamentals/_static/image69.png "Using Visual Studio code snippets to insert code into your project")</span></span>

<span data-ttu-id="fc507-834">*Aby wstawić kod do projektu przy użyciu wstawki kodu programu Visual Studio*</span><span class="sxs-lookup"><span data-stu-id="fc507-834">*Using Visual Studio code snippets to insert code into your project*</span></span>

<span data-ttu-id="fc507-835">***Aby dodać fragment kodu za pomocą klawiatury (C# tylko)***</span><span class="sxs-lookup"><span data-stu-id="fc507-835">***To add a code snippet using the keyboard (C# only)***</span></span>

1. <span data-ttu-id="fc507-836">Umieść kursor, w którym chcesz wstawić kod.</span><span class="sxs-lookup"><span data-stu-id="fc507-836">Place the cursor where you would like to insert the code.</span></span>
2. <span data-ttu-id="fc507-837">Zacznij wpisywać nazwę fragment (bez spacji i łączniki).</span><span class="sxs-lookup"><span data-stu-id="fc507-837">Start typing the snippet name (without spaces or hyphens).</span></span>
3. <span data-ttu-id="fc507-838">Obejrzyj jako IntelliSense wyświetla zgodne z nazwami wstawki.</span><span class="sxs-lookup"><span data-stu-id="fc507-838">Watch as IntelliSense displays matching snippets' names.</span></span>
4. <span data-ttu-id="fc507-839">Wybierz prawidłowe fragment (lub zachować wpisywanie do momentu wybrania fragment całą nazwę).</span><span class="sxs-lookup"><span data-stu-id="fc507-839">Select the correct snippet (or keep typing until the entire snippet's name is selected).</span></span>
5. <span data-ttu-id="fc507-840">Naciśnij klawisz Tab dwa razy, aby wstawić fragment kodu w lokalizacji kursora.</span><span class="sxs-lookup"><span data-stu-id="fc507-840">Press the Tab key twice to insert the snippet at the cursor location.</span></span>

<span data-ttu-id="fc507-841">![Rozpocznij wpisywanie nazwy fragment](aspnet-mvc-4-fundamentals/_static/image70.png "zacznij wpisywać nazwę wstawki programu")</span><span class="sxs-lookup"><span data-stu-id="fc507-841">![Start typing the snippet name](aspnet-mvc-4-fundamentals/_static/image70.png "Start typing the snippet name")</span></span>

<span data-ttu-id="fc507-842">*Rozpocznij wpisywanie nazwy fragment kodu*</span><span class="sxs-lookup"><span data-stu-id="fc507-842">*Start typing the snippet name*</span></span>

<span data-ttu-id="fc507-843">![Naciśnij klawisz Tab, aby wybrać wyróżnione fragment](aspnet-mvc-4-fundamentals/_static/image71.png "naciśnij klawisz Tab, aby wybrać wyróżnione fragment kodu")</span><span class="sxs-lookup"><span data-stu-id="fc507-843">![Press Tab to select the highlighted snippet](aspnet-mvc-4-fundamentals/_static/image71.png "Press Tab to select the highlighted snippet")</span></span>

<span data-ttu-id="fc507-844">*Naciśnij klawisz Tab, aby wybrać wyróżnione fragment kodu*</span><span class="sxs-lookup"><span data-stu-id="fc507-844">*Press Tab to select the highlighted snippet*</span></span>

<span data-ttu-id="fc507-845">![Ponownie naciśnij klawisz Tab i fragment rozwinie](aspnet-mvc-4-fundamentals/_static/image72.png "rozwinie ponownie naciśnij klawisz Tab i wstawki kodu")</span><span class="sxs-lookup"><span data-stu-id="fc507-845">![Press Tab again and the snippet will expand](aspnet-mvc-4-fundamentals/_static/image72.png "Press Tab again and the snippet will expand")</span></span>

<span data-ttu-id="fc507-846">*Rozwinie ponownie naciśnij klawisz Tab i wstawki kodu*</span><span class="sxs-lookup"><span data-stu-id="fc507-846">*Press Tab again and the snippet will expand*</span></span>

<span data-ttu-id="fc507-847">***Aby dodać fragment kodu za pomocą myszy (C#, Visual Basic i XML)*** 1.</span><span class="sxs-lookup"><span data-stu-id="fc507-847">***To add a code snippet using the mouse (C#, Visual Basic and XML)*** 1.</span></span> <span data-ttu-id="fc507-848">Kliknij prawym przyciskiem myszy, gdy chcesz wstawić fragment kodu.</span><span class="sxs-lookup"><span data-stu-id="fc507-848">Right-click where you want to insert the code snippet.</span></span>

1. <span data-ttu-id="fc507-849">Wybierz **wstawić fragment** następuje **Moje wstawki kodu**.</span><span class="sxs-lookup"><span data-stu-id="fc507-849">Select **Insert Snippet** followed by **My Code Snippets**.</span></span>
2. <span data-ttu-id="fc507-850">Wybierz odpowiedni fragment z listy, klikając go.</span><span class="sxs-lookup"><span data-stu-id="fc507-850">Pick the relevant snippet from the list, by clicking on it.</span></span>

<span data-ttu-id="fc507-851">![Kliknij prawym przyciskiem myszy, gdy chcesz wstawić fragment kodu i wybierz wstawić fragment](aspnet-mvc-4-fundamentals/_static/image73.png "kliknij prawym przyciskiem myszy, gdy chcesz wstawić fragment kodu i wybierz wstawić fragment kodu")</span><span class="sxs-lookup"><span data-stu-id="fc507-851">![Right-click where you want to insert the code snippet and select Insert Snippet](aspnet-mvc-4-fundamentals/_static/image73.png "Right-click where you want to insert the code snippet and select Insert Snippet")</span></span>

<span data-ttu-id="fc507-852">*Kliknij prawym przyciskiem myszy, gdy chcesz wstawić fragment kodu i wybierz wstawić fragment kodu*</span><span class="sxs-lookup"><span data-stu-id="fc507-852">*Right-click where you want to insert the code snippet and select Insert Snippet*</span></span>

<span data-ttu-id="fc507-853">![Wybierz odpowiedni fragment z listy, klikając go](aspnet-mvc-4-fundamentals/_static/image74.png "wybierz odpowiedni fragment z listy, klikając go")</span><span class="sxs-lookup"><span data-stu-id="fc507-853">![Pick the relevant snippet from the list, by clicking on it](aspnet-mvc-4-fundamentals/_static/image74.png "Pick the relevant snippet from the list, by clicking on it")</span></span>

<span data-ttu-id="fc507-854">*Wybierz odpowiedni fragment z listy, klikając go*</span><span class="sxs-lookup"><span data-stu-id="fc507-854">*Pick the relevant snippet from the list, by clicking on it*</span></span>
