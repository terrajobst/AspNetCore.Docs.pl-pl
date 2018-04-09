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
ms.openlocfilehash: a0dd32280321938aba84a2aed5273d80750ed774
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/06/2018
---
# <a name="aspnet-mvc-4-fundamentals"></a><span data-ttu-id="6e3e8-103">Podstawowe informacje na temat platformy ASP.NET MVC 4</span><span class="sxs-lookup"><span data-stu-id="6e3e8-103">ASP.NET MVC 4 Fundamentals</span></span>

<span data-ttu-id="6e3e8-104">Przez [obozów sieci Web Team](https://twitter.com/webcamps)</span><span class="sxs-lookup"><span data-stu-id="6e3e8-104">By [Web Camps Team](https://twitter.com/webcamps)</span></span>

[<span data-ttu-id="6e3e8-105">Pobierz obozów sieci Web uczenie Kit</span><span class="sxs-lookup"><span data-stu-id="6e3e8-105">Download Web Camps Training Kit</span></span>](https://aka.ms/webcamps-training-kit)

<span data-ttu-id="6e3e8-106">W tym laboratorium Hands-On jest oparta na sklep muzyczny MVC (Model View Controller), samouczek aplikacji, które wprowadzono i opisano krok po kroku, jak używać programu ASP.NET MVC i Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="6e3e8-106">This Hands-On Lab is based on MVC (Model View Controller) Music Store, a tutorial application that introduces and explains step-by-step how to use ASP.NET MVC and Visual Studio.</span></span> <span data-ttu-id="6e3e8-107">W laboratorium dowiesz się prostotę, jeszcze mocy przy użyciu tych technologii jednocześnie.</span><span class="sxs-lookup"><span data-stu-id="6e3e8-107">Throughout the lab you will learn the simplicity, yet power of using these technologies together.</span></span> <span data-ttu-id="6e3e8-108">Rozpoczyna się od prostej aplikacji i zostanie on skompilowany, aż do uzyskania funkcjonalnej ASP.NET MVC 4 aplikacji sieci Web.</span><span class="sxs-lookup"><span data-stu-id="6e3e8-108">You will start with a simple application and will build it until you have a fully functional ASP.NET MVC 4 Web Application.</span></span>

<span data-ttu-id="6e3e8-109">W tym laboratorium współpracuje z platformy ASP.NET MVC 4.</span><span class="sxs-lookup"><span data-stu-id="6e3e8-109">This Lab works with ASP.NET MVC 4.</span></span>

<span data-ttu-id="6e3e8-110">Jeśli chcesz zapoznać się z wersji platformy ASP.NET MVC 3 samouczka aplikacji, można znaleźć w [MVC utworów muzycznych magazynu](https://github.com/evilDave/MVC-Music-Store).</span><span class="sxs-lookup"><span data-stu-id="6e3e8-110">If you wish to explore the ASP.NET MVC 3 version of the tutorial application, you can find it in [MVC-Music-Store](https://github.com/evilDave/MVC-Music-Store).</span></span>

<span data-ttu-id="6e3e8-111">W tym laboratorium Hands-On przyjęto założenie, że deweloper ma doświadczenie w sieci Web development technologii, takich jak HTML i JavaScript.</span><span class="sxs-lookup"><span data-stu-id="6e3e8-111">This Hands-On Lab assumes that the developer has experience in Web development technologies, such as HTML and JavaScript.</span></span>

> [!NOTE]
> <span data-ttu-id="6e3e8-112">Wszystkie przykładowy kod i fragmenty kodu są uwzględnione w sieci Web obozów zestaw szkoleniowy, dostępne pod adresem [wersje Microsoft-Web/WebCampTrainingKit](https://aka.ms/webcamps-training-kit).</span><span class="sxs-lookup"><span data-stu-id="6e3e8-112">All sample code and snippets are included in the Web Camps Training Kit, available at [Microsoft-Web/WebCampTrainingKit Releases](https://aka.ms/webcamps-training-kit).</span></span> <span data-ttu-id="6e3e8-113">Specyficzne dla tego laboratorium projektu jest dostępna na [podstawowe informacje na temat platformy ASP.NET MVC 4](https://github.com/Microsoft-Web/HOL-MVC4Fundamentals).</span><span class="sxs-lookup"><span data-stu-id="6e3e8-113">The project specific to this lab is available at [ASP.NET MVC 4 Fundamentals](https://github.com/Microsoft-Web/HOL-MVC4Fundamentals).</span></span>

<a id="The_Music_Store_application"></a>
### <a name="the-music-store-application"></a><span data-ttu-id="6e3e8-114">Aplikacji magazynu utworów muzycznych</span><span class="sxs-lookup"><span data-stu-id="6e3e8-114">The Music Store application</span></span>

<span data-ttu-id="6e3e8-115">Muzyka Magazyn aplikacji sieci web, który zostanie skompilowany w tym laboratorium obejmuje trzy główne części: zakupów, wyewidencjonowania i administracji.</span><span class="sxs-lookup"><span data-stu-id="6e3e8-115">The Music Store web application that will be built throughout this Lab comprises three main parts: shopping, checkout, and administration.</span></span> <span data-ttu-id="6e3e8-116">Osoby odwiedzające będzie Przeglądaj albumów według rodzaju, Dodaj do koszyka ich albumy, przejrzyj ich zaznaczenie i koniec przejść do wyewidencjonowania, aby się zalogować i ukończyć kolejności.</span><span class="sxs-lookup"><span data-stu-id="6e3e8-116">Visitors will be able to browse albums by genre, add albums to their cart, review their selection and finally proceed to checkout to login and complete the order.</span></span> <span data-ttu-id="6e3e8-117">Ponadto magazynu administratorzy będą mogli zarządzać albumów dostępne, a także ich głównych właściwości.</span><span class="sxs-lookup"><span data-stu-id="6e3e8-117">Additionally, store administrators will be able to manage the available albums as well as their main properties.</span></span>

<span data-ttu-id="6e3e8-118">![Ekrany magazynu utworów muzycznych](aspnet-mvc-4-fundamentals/_static/image1.png "ekrany magazynu utworów muzycznych")</span><span class="sxs-lookup"><span data-stu-id="6e3e8-118">![Music Store screens](aspnet-mvc-4-fundamentals/_static/image1.png "Music Store screens")</span></span>

<span data-ttu-id="6e3e8-119">*Ekrany magazynu utworów muzycznych*</span><span class="sxs-lookup"><span data-stu-id="6e3e8-119">*Music Store screens*</span></span>

<a id="ASPNET_MVC_4_Essentials"></a>
### <a name="aspnet-mvc-4-essentials"></a><span data-ttu-id="6e3e8-120">ASP.NET MVC 4 Essentials</span><span class="sxs-lookup"><span data-stu-id="6e3e8-120">ASP.NET MVC 4 Essentials</span></span>

<span data-ttu-id="6e3e8-121">Aplikację ze sklepu utworów muzycznych zostaną skompilowane z użyciem **kontrolera MVC (Model View)**, wzorzec architektury, która dzieli aplikację na trzy główne składniki:</span><span class="sxs-lookup"><span data-stu-id="6e3e8-121">Music Store application will be built using **Model View Controller (MVC)**, an architectural pattern that separates an application into three main components:</span></span>

- <span data-ttu-id="6e3e8-122">**Modele**: obiekty modelu są częściami aplikacji, które implementują logikę domeny.</span><span class="sxs-lookup"><span data-stu-id="6e3e8-122">**Models**: Model objects are the parts of the application that implement the domain logic.</span></span> <span data-ttu-id="6e3e8-123">Często obiekty modelu również pobrać i przechowywanie stanu modelu w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="6e3e8-123">Often, model objects also retrieve and store model state in a database.</span></span>
- <span data-ttu-id="6e3e8-124">**Widoki:** widoki są składnikami wyświetlającymi interfejs użytkownika aplikacji (UI).</span><span class="sxs-lookup"><span data-stu-id="6e3e8-124">**Views:** Views are the components that display the application's user interface (UI).</span></span> <span data-ttu-id="6e3e8-125">Zazwyczaj interfejs ten jest utworzone na podstawie danych modelu.</span><span class="sxs-lookup"><span data-stu-id="6e3e8-125">Typically, this UI is created from the model data.</span></span> <span data-ttu-id="6e3e8-126">Przykładem może być widok edycji albumów zawierający pola tekstowe i listy rozwijanej, w oparciu o bieżący stan obiektu albumu.</span><span class="sxs-lookup"><span data-stu-id="6e3e8-126">An example would be the edit view of Albums that displays text boxes and a drop-down list based on the current state of an Album object.</span></span>
- <span data-ttu-id="6e3e8-127">**Kontrolery:** kontrolery są składnikami, które obsługują interakcję z użytkownikiem, manipulować modelem i ostatecznie wybierają widok do renderowania interfejsu użytkownika.</span><span class="sxs-lookup"><span data-stu-id="6e3e8-127">**Controllers:** Controllers are the components that handle user interaction, manipulate the model, and ultimately select a view to render the UI.</span></span> <span data-ttu-id="6e3e8-128">W aplikacji MVC widoku wyświetlane są tylko informacje; Kontroler obsługuje i ma odpowiadać na dane wejściowe użytkownika i interakcja.</span><span class="sxs-lookup"><span data-stu-id="6e3e8-128">In an MVC application, the view only displays information; the controller handles and responds to user input and interaction.</span></span>

<span data-ttu-id="6e3e8-129">Wzorzec MVC pomaga tworzyć aplikacji połączonych różne aspekty aplikacji (logika danych wejściowych, logika biznesowa i logika interfejsu użytkownika), zapewniając luźne powiązanie między tymi elementami.</span><span class="sxs-lookup"><span data-stu-id="6e3e8-129">The MVC pattern helps you to create applications that separate the different aspects of the application (input logic, business logic, and UI logic), while providing a loose coupling between these elements.</span></span> <span data-ttu-id="6e3e8-130">Taki rozdział pomaga w zarządzaniu złożonością podczas tworzenia aplikacji, ponieważ pozwala skupić się na jednym aspekcie implementacji naraz.</span><span class="sxs-lookup"><span data-stu-id="6e3e8-130">This separation helps you manage complexity when you build an application, as it allows you to focus on one aspect of the implementation at a time.</span></span> <span data-ttu-id="6e3e8-131">Ponadto wzorzec MVC ułatwia testowanie aplikacji również wspieranie stosowania projektowanie oparte na (testach TDD) do tworzenia aplikacji.</span><span class="sxs-lookup"><span data-stu-id="6e3e8-131">In addition, the MVC pattern makes it easy to test applications, also encouraging the use of test-driven development (TDD) for creating applications.</span></span>

<span data-ttu-id="6e3e8-132">**ASP.NET MVC** framework stanowi alternatywę dla składnika ASP.NET Web Forms wzorzec służący do tworzenia aplikacji sieci Web opartych na platformie ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="6e3e8-132">The **ASP.NET MVC** framework provides an alternative to the ASP.NET Web Forms pattern for creating ASP.NET MVC-based Web applications.</span></span> <span data-ttu-id="6e3e8-133">**ASP.NET MVC** framework to struktura prezentacji niewielka, wysoce testowalna która (tak jak aplikacje oparte na formularzach sieci Web) jest zintegrowana z istniejącymi funkcjami platformy ASP.NET, takich jak strony wzorcowe i oparte na członkostwie uwierzytelnianie, aby pobrać wszystkie możliwości platformy .NET core.</span><span class="sxs-lookup"><span data-stu-id="6e3e8-133">The **ASP.NET MVC** framework is a lightweight, highly testable presentation framework that (as with Web-forms-based applications) is integrated with existing ASP.NET features, such as master pages and membership-based authentication so you get all the power of the core .NET framework.</span></span> <span data-ttu-id="6e3e8-134">Jest to przydatne, jeśli już znasz z formularzy sieci Web ASP.NET wszystkie biblioteki, które już używają nie są dostępne w programie ASP.NET MVC 4 oraz.</span><span class="sxs-lookup"><span data-stu-id="6e3e8-134">This is useful if you are already familiar with ASP.NET Web Forms because all the libraries that you already use are available in ASP.NET MVC 4 as well.</span></span>

<span data-ttu-id="6e3e8-135">Ponadto luźne powiązanie między trzema głównymi składnikami aplikacji MVC wspiera także Programowanie równoległe.</span><span class="sxs-lookup"><span data-stu-id="6e3e8-135">In addition, the loose coupling between the three main components of an MVC application also promotes parallel development.</span></span> <span data-ttu-id="6e3e8-136">Na przykład co może pracować w widoku, drugi może pracować nad logiką kontrolera i trzeci deweloper może skupić się na logiki biznesowej w modelu.</span><span class="sxs-lookup"><span data-stu-id="6e3e8-136">For instance, one developer can work on the view, a second developer can work on the controller logic, and a third developer can focus on the business logic in the model.</span></span>

<a id="Objectives"></a>

<a id="Objectives"></a>
### <a name="objectives"></a><span data-ttu-id="6e3e8-137">Cele</span><span class="sxs-lookup"><span data-stu-id="6e3e8-137">Objectives</span></span>

<span data-ttu-id="6e3e8-138">W tym laboratorium Hands-On przedstawiono sposób:</span><span class="sxs-lookup"><span data-stu-id="6e3e8-138">In this Hands-On Lab, you will learn how to:</span></span>

- <span data-ttu-id="6e3e8-139">Tworzenie aplikacji platformy ASP.NET MVC od podstaw oparte na samouczek aplikacji magazynu utworów muzycznych</span><span class="sxs-lookup"><span data-stu-id="6e3e8-139">Create an ASP.NET MVC application from scratch based on the Music Store Application tutorial</span></span>
- <span data-ttu-id="6e3e8-140">Dodaj kontrolery do obsługi adresów URL do strony głównej witryny oraz przeglądanie jej funkcji main</span><span class="sxs-lookup"><span data-stu-id="6e3e8-140">Add Controllers to handle URLs to the Home page of the site and for browsing its main functionality</span></span>
- <span data-ttu-id="6e3e8-141">Dodaj widok, aby dostosować zawartość wyświetlana wraz z jego styl</span><span class="sxs-lookup"><span data-stu-id="6e3e8-141">Add a View to customize the content displayed along with its style</span></span>
- <span data-ttu-id="6e3e8-142">Dodawanie klasy modelu zawierają i zarządzanie nimi danych i domeny logiki</span><span class="sxs-lookup"><span data-stu-id="6e3e8-142">Add Model classes to contain and manage data and domain logic</span></span>
- <span data-ttu-id="6e3e8-143">Użyj wzorca Model widoku do przekazywania informacji z akcji kontrolera do widoku szablonów</span><span class="sxs-lookup"><span data-stu-id="6e3e8-143">Use View Model pattern to pass information from controller actions to the view templates</span></span>
- <span data-ttu-id="6e3e8-144">Eksploruj nowego szablonu platformy ASP.NET MVC 4 dla aplikacji internetowych</span><span class="sxs-lookup"><span data-stu-id="6e3e8-144">Explore the ASP.NET MVC 4 new template for internet applications</span></span>

<a id="Prerequisites"></a>

<a id="Prerequisites"></a>
### <a name="prerequisites"></a><span data-ttu-id="6e3e8-145">Wymagania wstępne</span><span class="sxs-lookup"><span data-stu-id="6e3e8-145">Prerequisites</span></span>

<span data-ttu-id="6e3e8-146">Musi mieć następujące elementy do przygotowania tego laboratorium:</span><span class="sxs-lookup"><span data-stu-id="6e3e8-146">You must have the following items to complete this lab:</span></span>

- <span data-ttu-id="6e3e8-147">[Visual Studio Express 2012 for Web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web) (odczytu [dodatek a.](#AppendixA) instrukcje dotyczące sposobu jego instalacji)</span><span class="sxs-lookup"><span data-stu-id="6e3e8-147">[Visual Studio 2012 Express for Web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web) (read [Appendix A](#AppendixA) for instructions on how to install it)</span></span>

<a id="Setup"></a>

<a id="Setup"></a>
### <a name="setup"></a><span data-ttu-id="6e3e8-148">Konfiguracja</span><span class="sxs-lookup"><span data-stu-id="6e3e8-148">Setup</span></span>

<span data-ttu-id="6e3e8-149">**Instalowanie wstawki kodu**</span><span class="sxs-lookup"><span data-stu-id="6e3e8-149">**Installing Code Snippets**</span></span>

<span data-ttu-id="6e3e8-150">Dla wygody taki kod, który będzie zarządzany wzdłuż tego laboratorium jest dostępna jako wstawki kodu programu Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="6e3e8-150">For convenience, much of the code you will be managing along this lab is available as Visual Studio code snippets.</span></span> <span data-ttu-id="6e3e8-151">Aby zainstalować fragmenty kodu, uruchom **.\Source\Setup\CodeSnippets.vsi** pliku.</span><span class="sxs-lookup"><span data-stu-id="6e3e8-151">To install the code snippets run **.\Source\Setup\CodeSnippets.vsi** file.</span></span>

<span data-ttu-id="6e3e8-152">Jeśli nie masz doświadczenia z wstawki programu Visual Studio i chcesz dowiedzieć się, jak ich używać, można odwołać się do dodatku z tego dokumentu &quot; [wstawki kodu za pomocą programu dodatek C:](#AppendixC)&quot;.</span><span class="sxs-lookup"><span data-stu-id="6e3e8-152">If you are not familiar with the Visual Studio Code Snippets, and want to learn how to use them, you can refer to the appendix from this document &quot;[Appendix C: Using Code Snippets](#AppendixC)&quot;.</span></span>

<a id="Exercises"></a>

<a id="Exercises"></a>
## <a name="exercises"></a><span data-ttu-id="6e3e8-153">Ćwiczenia</span><span class="sxs-lookup"><span data-stu-id="6e3e8-153">Exercises</span></span>

<span data-ttu-id="6e3e8-154">W tym laboratorium Hands-On składa się przez następujących czynnościach:</span><span class="sxs-lookup"><span data-stu-id="6e3e8-154">This Hands-On Lab is comprised by the following exercises:</span></span>

1. [<span data-ttu-id="6e3e8-155">Ćwiczenie 1: Tworzenie projektu aplikacji sieci Web platformy ASP.NET MVC MusicStore</span><span class="sxs-lookup"><span data-stu-id="6e3e8-155">Exercise 1: Creating MusicStore ASP.NET MVC Web Application Project</span></span>](#Exercise1)
2. [<span data-ttu-id="6e3e8-156">Ćwiczenie 2: Tworzenie kontrolera</span><span class="sxs-lookup"><span data-stu-id="6e3e8-156">Exercise 2: Creating a Controller</span></span>](#Exercise2)
3. [<span data-ttu-id="6e3e8-157">Ćwiczenie 3: Przekazywanie parametrów do kontrolera</span><span class="sxs-lookup"><span data-stu-id="6e3e8-157">Exercise 3: Passing parameters to a Controller</span></span>](#Exercise3)
4. [<span data-ttu-id="6e3e8-158">Ćwiczenie 4: Tworzenie widoku</span><span class="sxs-lookup"><span data-stu-id="6e3e8-158">Exercise 4: Creating a View</span></span>](#Exercise4)
5. [<span data-ttu-id="6e3e8-159">Ćwiczenie 5: Tworzenie modelu widoku</span><span class="sxs-lookup"><span data-stu-id="6e3e8-159">Exercise 5: Creating a View Model</span></span>](#Exercise5)
6. [<span data-ttu-id="6e3e8-160">Ćwiczenie 6: W widoku przy użyciu parametrów</span><span class="sxs-lookup"><span data-stu-id="6e3e8-160">Exercise 6: Using parameters in View</span></span>](#Exercise6)
7. [<span data-ttu-id="6e3e8-161">Ćwiczenie 7: Laptop wokół nowego szablonu platformy ASP.NET MVC 4</span><span class="sxs-lookup"><span data-stu-id="6e3e8-161">Exercise 7: A lap around ASP.NET MVC 4 New Template</span></span>](#Exercise7)

> [!NOTE]
> <span data-ttu-id="6e3e8-162">Towarzyszy każdego wykonywania **zakończenia** folderu zawierającego wynikowy rozwiązanie, należy uzyskać po wykonaniu ćwiczeniach.</span><span class="sxs-lookup"><span data-stu-id="6e3e8-162">Each exercise is accompanied by an **End** folder containing the resulting solution you should obtain after completing the exercises.</span></span> <span data-ttu-id="6e3e8-163">Jeśli potrzebujesz dodatkowej pomocy pracuje nad ćwiczeniami, można użyć tego rozwiązania jako przewodnika.</span><span class="sxs-lookup"><span data-stu-id="6e3e8-163">You can use this solution as a guide if you need additional help working through the exercises.</span></span>


<span data-ttu-id="6e3e8-164">Szacowany czas trwania tego laboratorium: **60 minut**.</span><span class="sxs-lookup"><span data-stu-id="6e3e8-164">Estimated time to complete this lab: **60 minutes**.</span></span>

<a id="Exercise1"></a>

<a id="Exercise_1_Creating_MusicStore_ASPNET_MVC_Web_Application_Project"></a>
### <a name="exercise-1-creating-musicstore-aspnet-mvc-web-application-project"></a><span data-ttu-id="6e3e8-165">Ćwiczenie 1: Tworzenie projektu aplikacji sieci Web platformy ASP.NET MVC MusicStore</span><span class="sxs-lookup"><span data-stu-id="6e3e8-165">Exercise 1: Creating MusicStore ASP.NET MVC Web Application Project</span></span>

<span data-ttu-id="6e3e8-166">W tym ćwiczeniu dowiesz się, tworzenie aplikacji platformy ASP.NET MVC w programie Visual Studio 2012 Express dla sieci Web, a także jego organizacji folderu głównego.</span><span class="sxs-lookup"><span data-stu-id="6e3e8-166">In this exercise, you will learn how to create an ASP.NET MVC application in Visual Studio 2012 Express for Web as well as its main folder organization.</span></span> <span data-ttu-id="6e3e8-167">Ponadto dowiesz sposobu dodawania nowego kontrolera i zapewnić ich prostego ciągu wyświetlanego w strony głównej aplikacji.</span><span class="sxs-lookup"><span data-stu-id="6e3e8-167">Additionally, you will learn how to add a new Controller and make it display a simple string in the application's home page.</span></span>

<a id="Ex1Task1"></a>

<a id="Task_1_-_Creating_the_ASPNET_MVC_Web_Application_Project"></a>
#### <a name="task-1---creating-the-aspnet-mvc-web-application-project"></a><span data-ttu-id="6e3e8-168">Zadanie 1 — Tworzenie projektu aplikacji sieci Web platformy ASP.NET MVC</span><span class="sxs-lookup"><span data-stu-id="6e3e8-168">Task 1 - Creating the ASP.NET MVC Web Application Project</span></span>

1. <span data-ttu-id="6e3e8-169">W ramach tego zadania spowoduje utworzenie pusty projekt aplikacji platformy ASP.NET MVC przy użyciu szablonu MVC programu Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="6e3e8-169">In this task, you will create an empty ASP.NET MVC application project using the MVC Visual Studio template.</span></span> <span data-ttu-id="6e3e8-170">Uruchom **VS Express for Web**.</span><span class="sxs-lookup"><span data-stu-id="6e3e8-170">Start **VS Express for Web**.</span></span>
2. <span data-ttu-id="6e3e8-171">Na **pliku** menu, kliknij przycisk **nowy projekt**.</span><span class="sxs-lookup"><span data-stu-id="6e3e8-171">On the **File** menu, click **New Project**.</span></span>
3. <span data-ttu-id="6e3e8-172">W **nowy projekt** wybierz okno dialogowe **aplikacji sieci Web programu ASP.NET MVC 4** typu znajduje się w obszarze projektu **Visual C#,** **Web** szablonu Lista.</span><span class="sxs-lookup"><span data-stu-id="6e3e8-172">In the **New Project** dialog box select the **ASP.NET MVC 4 Web Application** project type, located under **Visual C#,** **Web** template list.</span></span>
4. <span data-ttu-id="6e3e8-173">Zmień **nazwa** do *MvcMusicStore*.</span><span class="sxs-lookup"><span data-stu-id="6e3e8-173">Change the **Name** to *MvcMusicStore*.</span></span>
5. <span data-ttu-id="6e3e8-174">Ustaw lokalizację rozwiązania wewnątrz nowy **rozpocząć** folder, w tym ćwiczeniu folder źródłowy, na przykład **\Source\Ex01-CreatingMusicStoreProject\Begin [YOUR-dzień HOL-PATH]**.</span><span class="sxs-lookup"><span data-stu-id="6e3e8-174">Set the location of the solution inside a new **Begin** folder in this Exercise's Source folder, for example **[YOUR-HOL-PATH]\Source\Ex01-CreatingMusicStoreProject\Begin**.</span></span> <span data-ttu-id="6e3e8-175">Kliknij przycisk **OK**.</span><span class="sxs-lookup"><span data-stu-id="6e3e8-175">Click **OK**.</span></span>

    <span data-ttu-id="6e3e8-176">![Utwórz okno dialogowe Nowy projekt](aspnet-mvc-4-fundamentals/_static/image2.png "utworzyć okno dialogowe Nowy projekt")</span><span class="sxs-lookup"><span data-stu-id="6e3e8-176">![Create New Project Dialog Box](aspnet-mvc-4-fundamentals/_static/image2.png "Create New Project Dialog Box")</span></span>

    <span data-ttu-id="6e3e8-177">*Utwórz okno dialogowe Nowy projekt*</span><span class="sxs-lookup"><span data-stu-id="6e3e8-177">*Create New Project Dialog Box*</span></span>
6. <span data-ttu-id="6e3e8-178">W **nowy projekt programu ASP.NET MVC 4** wybierz okno dialogowe **podstawowe** szablonu i upewnij się, że **aparat widoku** jest **Razor**.</span><span class="sxs-lookup"><span data-stu-id="6e3e8-178">In the **New ASP.NET MVC 4 Project** dialog box select the **Basic** template and make sure that the **View engine** selected is **Razor**.</span></span> <span data-ttu-id="6e3e8-179">Kliknij przycisk **OK**.</span><span class="sxs-lookup"><span data-stu-id="6e3e8-179">Click **OK**.</span></span>

    <span data-ttu-id="6e3e8-180">![Okno dialogowe nowego projektu platformy ASP.NET MVC 4](aspnet-mvc-4-fundamentals/_static/image3.png "okno dialogowe Nowy projekt ASP.NET MVC 4")</span><span class="sxs-lookup"><span data-stu-id="6e3e8-180">![New ASP.NET MVC 4 Project Dialog Box](aspnet-mvc-4-fundamentals/_static/image3.png "New ASP.NET MVC 4 Project Dialog Box")</span></span>

    <span data-ttu-id="6e3e8-181">*Okno dialogowe nowego projektu platformy ASP.NET MVC 4*</span><span class="sxs-lookup"><span data-stu-id="6e3e8-181">*New ASP.NET MVC 4 Project Dialog Box*</span></span>

<a id="Ex1Task2"></a>

<a id="Task_2_-_Exploring_the_Solution_Structure"></a>
#### <a name="task-2---exploring-the-solution-structure"></a><span data-ttu-id="6e3e8-182">Zadanie 2 — poznawanie struktury rozwiązania</span><span class="sxs-lookup"><span data-stu-id="6e3e8-182">Task 2 - Exploring the Solution Structure</span></span>

<span data-ttu-id="6e3e8-183">Platforma ASP.NET MVC zawiera szablon projektu Visual Studio, która ułatwia tworzenie aplikacji sieci Web obsługujące wzorzec MVC.</span><span class="sxs-lookup"><span data-stu-id="6e3e8-183">The ASP.NET MVC framework includes a Visual Studio project template that helps you create Web applications supporting the MVC pattern.</span></span> <span data-ttu-id="6e3e8-184">Ten szablon tworzy nową aplikację sieci Web programu ASP.NET MVC z wymagane foldery, szablonów elementów i we wpisach w plikach konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="6e3e8-184">This template creates a new ASP.NET MVC Web application with the required folders, item templates, and configuration-file entries.</span></span>

<span data-ttu-id="6e3e8-185">To zadanie należy zbadać struktury rozwiązania zrozumienie elementy, które są zaangażowane oraz ich relacji.</span><span class="sxs-lookup"><span data-stu-id="6e3e8-185">In this task, you will examine the solution structure to understand the elements that are involved and their relationships.</span></span> <span data-ttu-id="6e3e8-186">Następujące foldery są zawarte w aplikacji ASP.NET MVC, ponieważ platforma ASP.NET MVC domyślnie korzysta z &quot;Konwencji za pośrednictwem konfiguracji&quot; podejście i sprawia, że niektóre założenia domyślne oparte na folder nazewnictwa konwencje.</span><span class="sxs-lookup"><span data-stu-id="6e3e8-186">The following folders are included in all the ASP.NET MVC application because the ASP.NET MVC framework by default uses a &quot;convention over configuration&quot; approach, and makes some default assumptions based on folder naming conventions.</span></span>

1. <span data-ttu-id="6e3e8-187">Po utworzeniu projektu, przejrzyj struktury folderów, który został utworzony w Eksploratorze rozwiązań po prawej stronie:</span><span class="sxs-lookup"><span data-stu-id="6e3e8-187">Once the project is created, review the folder structure that has been created in the Solution Explorer on the right side:</span></span>

    <span data-ttu-id="6e3e8-188">![Struktura ASP.NET MVC folderów w Eksploratorze rozwiązań](aspnet-mvc-4-fundamentals/_static/image4.png "struktury ASP.NET MVC folderów w Eksploratorze rozwiązań")</span><span class="sxs-lookup"><span data-stu-id="6e3e8-188">![ASP.NET MVC Folder structure in Solution Explorer](aspnet-mvc-4-fundamentals/_static/image4.png "ASP.NET MVC Folder structure in Solution Explorer")</span></span>

    <span data-ttu-id="6e3e8-189">*Struktura ASP.NET MVC folderów w Eksploratorze rozwiązań*</span><span class="sxs-lookup"><span data-stu-id="6e3e8-189">*ASP.NET MVC Folder structure in Solution Explorer*</span></span>

   1. <span data-ttu-id="6e3e8-190">**Kontrolery**.</span><span class="sxs-lookup"><span data-stu-id="6e3e8-190">**Controllers**.</span></span> <span data-ttu-id="6e3e8-191">Ten folder będzie zawierać klasy kontrolera.</span><span class="sxs-lookup"><span data-stu-id="6e3e8-191">This folder will contain the controller classes.</span></span> <span data-ttu-id="6e3e8-192">W aplikacji MVC na podstawie kontrolery są zobowiązani do obsługi interakcji użytkownika końcowego, manipulowanie modelem i ostatecznie wybierając widoku do renderowania interfejsu użytkownika.</span><span class="sxs-lookup"><span data-stu-id="6e3e8-192">In an MVC based application, controllers are responsible for handling end user interaction, manipulating the model, and ultimately choosing a view to render the UI.</span></span>

       > [!NOTE]
       > <span data-ttu-id="6e3e8-193">Platforma MVC wymaga nazwy wszystkich kontrolerów kończącej się &quot;kontrolera&quot;— na przykład HomeController, LoginController lub ProductController.</span><span class="sxs-lookup"><span data-stu-id="6e3e8-193">The MVC framework requires the names of all controllers to end with &quot;Controller&quot;-for example, HomeController, LoginController, or ProductController.</span></span>
   2. <span data-ttu-id="6e3e8-194">**Modele**.</span><span class="sxs-lookup"><span data-stu-id="6e3e8-194">**Models**.</span></span> <span data-ttu-id="6e3e8-195">Ten folder jest dostępna dla klasy reprezentujące model aplikacji dla aplikacji sieci Web MVC.</span><span class="sxs-lookup"><span data-stu-id="6e3e8-195">This folder is provided for classes that represent the application model for the MVC Web application.</span></span> <span data-ttu-id="6e3e8-196">Obejmuje to zazwyczaj kod, który definiuje obiekty i logika interakcji z magazynu danych.</span><span class="sxs-lookup"><span data-stu-id="6e3e8-196">This usually includes code that defines objects and the logic for interacting with the data store.</span></span> <span data-ttu-id="6e3e8-197">Zazwyczaj rzeczywiste modelu obiektów jest w bibliotekach osobnej klasy.</span><span class="sxs-lookup"><span data-stu-id="6e3e8-197">Typically, the actual model objects will be in separate class libraries.</span></span> <span data-ttu-id="6e3e8-198">Jednak podczas tworzenia nowej aplikacji może zawierać klasy i przenieś je do biblioteki klas oddzielne w późniejszym czasie w cyklu tworzenia.</span><span class="sxs-lookup"><span data-stu-id="6e3e8-198">However, when you create a new application, you might include classes and then move them into separate class libraries at a later point in the development cycle.</span></span>
   3. <span data-ttu-id="6e3e8-199">**Widoki**.</span><span class="sxs-lookup"><span data-stu-id="6e3e8-199">**Views**.</span></span> <span data-ttu-id="6e3e8-200">Ten folder jest zalecana lokalizacja widoków, składniki odpowiada za wyświetlanie interfejsu użytkownika aplikacji.</span><span class="sxs-lookup"><span data-stu-id="6e3e8-200">This folder is the recommended location for views, the components responsible for displaying the application's user interface.</span></span> <span data-ttu-id="6e3e8-201">Widoki używają plików aspx, ascx, cshtml i .master, oprócz innych plików, które są powiązane z widokami renderowania.</span><span class="sxs-lookup"><span data-stu-id="6e3e8-201">Views use .aspx, .ascx, .cshtml and .master files, in addition to any other files that are related to rendering views.</span></span> <span data-ttu-id="6e3e8-202">Widoki folder zawiera folder dla każdego kontrolera; folder nosi nazwę z prefiksem nazwy kontrolera.</span><span class="sxs-lookup"><span data-stu-id="6e3e8-202">Views folder contains a folder for each controller; the folder is named with the controller-name prefix.</span></span> <span data-ttu-id="6e3e8-203">Na przykład, jeśli masz kontroler o nazwie **HomeController**, widoki folder zawiera folder o nazwie Home.</span><span class="sxs-lookup"><span data-stu-id="6e3e8-203">For example, if you have a controller named **HomeController**, the Views folder will contain a folder named Home.</span></span> <span data-ttu-id="6e3e8-204">Domyślnie, gdy platforma ASP.NET MVC ładuje widoku szuka plik .aspx o nazwie żądany widok w folderze Views\controllerName (**widoków [ControllerName] [Action] .aspx**) lub (**widoków [ControllerName] [Action] .cshtml**) dla widokami Razor.</span><span class="sxs-lookup"><span data-stu-id="6e3e8-204">By default, when the ASP.NET MVC framework loads a view, it looks for an .aspx file with the requested view name in the Views\controllerName folder (**Views[ControllerName][Action].aspx**) or (**Views[ControllerName][Action].cshtml**) for Razor Views.</span></span>

      > [!NOTE]
      > <span data-ttu-id="6e3e8-205">Oprócz folderów wymienionymi wcześniej, korzysta z aplikacji sieci Web MVC **Global.asax** plik routingu adresów URL globalne ustawienia domyślne i używa **Web.config** plik do skonfigurowania aplikacji.</span><span class="sxs-lookup"><span data-stu-id="6e3e8-205">In addition to the folders listed previously, an MVC Web application uses the **Global.asax** file to set global URL routing defaults, and it uses the **Web.config** file to configure the application.</span></span>

<a id="Ex1Task3"></a>

<a id="Task_3_-_Adding_a_HomeController"></a>
#### <a name="task-3---adding-a-homecontroller"></a><span data-ttu-id="6e3e8-206">Zadanie 3. Dodawanie HomeController</span><span class="sxs-lookup"><span data-stu-id="6e3e8-206">Task 3 - Adding a HomeController</span></span>

<span data-ttu-id="6e3e8-207">W aplikacjach ASP.NET, które nie korzystają z struktura MVC interakcji z użytkownikiem są zorganizowane wokół stron i wywoływanie zdarzeń i obsługa zdarzeń z tych stron.</span><span class="sxs-lookup"><span data-stu-id="6e3e8-207">In ASP.NET applications that do not use the MVC framework, user interaction is organized around pages, and around raising and handling events from those pages.</span></span> <span data-ttu-id="6e3e8-208">Z kolei interakcji użytkowników z aplikacji ASP.NET MVC skupiono kontrolerów i metod akcji.</span><span class="sxs-lookup"><span data-stu-id="6e3e8-208">In contrast, user interaction with ASP.NET MVC applications is organized around controllers and their action methods.</span></span>

<span data-ttu-id="6e3e8-209">Z drugiej strony platforma ASP.NET MVC mapuje adresy URL do klasy, które są określane jako kontrolery.</span><span class="sxs-lookup"><span data-stu-id="6e3e8-209">On the other hand, ASP.NET MVC framework maps URLs to classes that are referred to as controllers.</span></span> <span data-ttu-id="6e3e8-210">Kontrolery przetworzyć żądań przychodzących, obsługi danych wejściowych użytkownika i interakcji, wykonywanie logiki aplikacji odpowiednie i określić odpowiedzi do odesłania do klienta (wyświetlania kodu HTML, Pobierz plik, przekierowania do innego adresu URL itp.).</span><span class="sxs-lookup"><span data-stu-id="6e3e8-210">Controllers process incoming requests, handle user input and interactions, execute appropriate application logic and determine the response to send back to the client (display HTML, download a file, redirect to a different URL, etc.).</span></span> <span data-ttu-id="6e3e8-211">W przypadku wyświetlania kodu HTML, klasę kontrolera zwykle wywołuje składnik osobny widok, aby wygenerować kod znaczników HTML dla żądania.</span><span class="sxs-lookup"><span data-stu-id="6e3e8-211">In the case of displaying HTML, a controller class typically calls a separate view component to generate the HTML markup for the request.</span></span> <span data-ttu-id="6e3e8-212">W aplikacji MVC widoku wyświetlane są tylko informacje; Kontroler obsługuje i ma odpowiadać na dane wejściowe użytkownika i interakcja.</span><span class="sxs-lookup"><span data-stu-id="6e3e8-212">In an MVC application, the view only displays information; the controller handles and responds to user input and interaction.</span></span>

<span data-ttu-id="6e3e8-213">To zadanie spowoduje dodanie klasy kontrolera, która będzie obsługiwać adresy URL do strony głównej witryny sklep muzyczny.</span><span class="sxs-lookup"><span data-stu-id="6e3e8-213">In this task, you will add a Controller class that will handle URLs to the Home page of the Music Store site.</span></span>

1. <span data-ttu-id="6e3e8-214">Kliknij prawym przyciskiem myszy **kontrolerów** folder w Eksploratorze rozwiązań wybierz **Dodaj** , a następnie **kontrolera** polecenia:</span><span class="sxs-lookup"><span data-stu-id="6e3e8-214">Right-click **Controllers** folder within the Solution Explorer, select **Add** and then **Controller** command:</span></span>

    <span data-ttu-id="6e3e8-215">![Dodaj polecenie kontrolera](aspnet-mvc-4-fundamentals/_static/image5.png "Dodawanie polecenia kontrolera")</span><span class="sxs-lookup"><span data-stu-id="6e3e8-215">![Add a Controller Command](aspnet-mvc-4-fundamentals/_static/image5.png "Add a Controller Command")</span></span>

    <span data-ttu-id="6e3e8-216">*Dodaj polecenie kontrolera*</span><span class="sxs-lookup"><span data-stu-id="6e3e8-216">*Add Controller Command*</span></span>
2. <span data-ttu-id="6e3e8-217">**Dodaj kontroler** zostanie wyświetlone okno dialogowe.</span><span class="sxs-lookup"><span data-stu-id="6e3e8-217">The **Add Controller** dialog appears.</span></span> <span data-ttu-id="6e3e8-218">Nazwa kontrolera *HomeController* i naciśnij klawisz **Dodaj**.</span><span class="sxs-lookup"><span data-stu-id="6e3e8-218">Name the controller *HomeController* and press **Add**.</span></span>

    <span data-ttu-id="6e3e8-219">![Kontroler okno dialogowe Dodawanie](aspnet-mvc-4-fundamentals/_static/image6.png "kontrolera okno dialogowe Dodawanie")</span><span class="sxs-lookup"><span data-stu-id="6e3e8-219">![Add Controller Dialog](aspnet-mvc-4-fundamentals/_static/image6.png "Add Controller Dialog")</span></span>

    <span data-ttu-id="6e3e8-220">*Dodawanie kontrolera okna dialogowego*</span><span class="sxs-lookup"><span data-stu-id="6e3e8-220">*Add Controller Dialog*</span></span>
3. <span data-ttu-id="6e3e8-221">Plik **HomeController.cs** jest tworzony w **kontrolerów** folderu.</span><span class="sxs-lookup"><span data-stu-id="6e3e8-221">The file **HomeController.cs** is created in the **Controllers** folder.</span></span> <span data-ttu-id="6e3e8-222">Aby przypisać **HomeController** zwraca ciąg na jego akcji indeksu, Zastąp **indeksu** metodę z następującym kodem:</span><span class="sxs-lookup"><span data-stu-id="6e3e8-222">In order to have the **HomeController** return a string on its Index action, replace the **Index** method with the following code:</span></span>

    <span data-ttu-id="6e3e8-223">(Fragment - kodu *podstawy platformy ASP.NET MVC 4 - indeksu HomeController Ex1*)</span><span class="sxs-lookup"><span data-stu-id="6e3e8-223">(Code Snippet - *ASP.NET MVC 4 Fundamentals - Ex1 HomeController Index*)</span></span>


~~~
[!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample1.cs)]
~~~

<a id="Ex1Task4"></a>

<a id="Task_4_-_Running_the_Application"></a>
#### <a name="task-4---running-the-application"></a><span data-ttu-id="6e3e8-224">Zadanie 4 — uruchamianie aplikacji</span><span class="sxs-lookup"><span data-stu-id="6e3e8-224">Task 4 - Running the Application</span></span>

<span data-ttu-id="6e3e8-225">To zadanie będzie Wypróbuj tę aplikację w przeglądarce sieci web.</span><span class="sxs-lookup"><span data-stu-id="6e3e8-225">In this task, you will try out the Application in a web browser.</span></span>

1. <span data-ttu-id="6e3e8-226">Naciśnij klawisz **F5** do uruchomienia aplikacji.</span><span class="sxs-lookup"><span data-stu-id="6e3e8-226">Press **F5** to run the Application.</span></span> <span data-ttu-id="6e3e8-227">Projekt jest kompilowany i uruchamia serwer sieci Web IIS lokalnej.</span><span class="sxs-lookup"><span data-stu-id="6e3e8-227">The project is compiled and the local IIS Web Server starts.</span></span> <span data-ttu-id="6e3e8-228">Lokalny serwer sieci Web usług IIS zostanie automatycznie uruchomiona przeglądarki sieci web wskazuje adres URL serwera sieci Web.</span><span class="sxs-lookup"><span data-stu-id="6e3e8-228">The local IIS Web Server will automatically open a web browser pointing to the URL of the Web server.</span></span>

    <span data-ttu-id="6e3e8-229">![Aplikacja uruchomiona w przeglądarce sieci web](aspnet-mvc-4-fundamentals/_static/image7.png "aplikacja była uruchomiona w przeglądarce sieci web")</span><span class="sxs-lookup"><span data-stu-id="6e3e8-229">![Application running in a web browser](aspnet-mvc-4-fundamentals/_static/image7.png "Application running in a web browser")</span></span>

    <span data-ttu-id="6e3e8-230">*Aplikacja uruchomiona w przeglądarce sieci web*</span><span class="sxs-lookup"><span data-stu-id="6e3e8-230">*Application running in a web browser*</span></span>

    > [!NOTE]
    > <span data-ttu-id="6e3e8-231">Lokalnego serwera sieci Web usług IIS będą uruchamiane witryny sieci Web na numer portu wolne losowych.</span><span class="sxs-lookup"><span data-stu-id="6e3e8-231">The local IIS Web Server will run the website on a random free port number.</span></span> <span data-ttu-id="6e3e8-232">Na powyższej ilustracji lokacji działa w `http://localhost:50103/`, więc używa portu 50103.</span><span class="sxs-lookup"><span data-stu-id="6e3e8-232">In the figure above, the site is running at `http://localhost:50103/`, so it's using port 50103.</span></span> <span data-ttu-id="6e3e8-233">Numer portu może się różnić.</span><span class="sxs-lookup"><span data-stu-id="6e3e8-233">Your port number may vary.</span></span>
2. <span data-ttu-id="6e3e8-234">Zamknij przeglądarkę.</span><span class="sxs-lookup"><span data-stu-id="6e3e8-234">Close the browser.</span></span>

<a id="Exercise2"></a>

<a id="Exercise_2_Creating_a_Controller"></a>
### <a name="exercise-2-creating-a-controller"></a><span data-ttu-id="6e3e8-235">Ćwiczenie 2: Tworzenie kontrolera</span><span class="sxs-lookup"><span data-stu-id="6e3e8-235">Exercise 2: Creating a Controller</span></span>

<span data-ttu-id="6e3e8-236">W tym ćwiczeniu dowiesz się, jak zaktualizować kontroler do zaimplementowania proste funkcje aplikacji utworów muzycznych magazynu.</span><span class="sxs-lookup"><span data-stu-id="6e3e8-236">In this exercise, you will learn how to update the controller to implement simple functionality of the Music Store application.</span></span> <span data-ttu-id="6e3e8-237">Ten kontroler określi metod akcji do każdego z następujących określone żądania obsługi:</span><span class="sxs-lookup"><span data-stu-id="6e3e8-237">That controller will define action methods to handle each of the following specific requests:</span></span>

- <span data-ttu-id="6e3e8-238">Strony listę gatunkami muzyki utworów muzycznych w magazynie utworów muzycznych</span><span class="sxs-lookup"><span data-stu-id="6e3e8-238">A listing page of the music genres in the Music Store</span></span>
- <span data-ttu-id="6e3e8-239">Strona przeglądania, która zawiera listę wszystkich albumów muzycznych dla określonego rodzaju</span><span class="sxs-lookup"><span data-stu-id="6e3e8-239">A browse page that lists all of the music albums for a particular genre</span></span>
- <span data-ttu-id="6e3e8-240">Strona szczegółów, która zawiera informacje o albumu określonych utworów muzycznych</span><span class="sxs-lookup"><span data-stu-id="6e3e8-240">A details page that shows information about a specific music album</span></span>

<span data-ttu-id="6e3e8-241">W zakresie tego ćwiczenia te akcje po prostu zwróci ciąg przez teraz.</span><span class="sxs-lookup"><span data-stu-id="6e3e8-241">For the scope of this exercise, those actions will simply return a string by now.</span></span>

<a id="Ex2Task1"></a>

<a id="Task_1_-_Adding_a_New_StoreController_Class"></a>
#### <a name="task-1---adding-a-new-storecontroller-class"></a><span data-ttu-id="6e3e8-242">Zadanie 1 — Dodawanie nowej klasy StoreController</span><span class="sxs-lookup"><span data-stu-id="6e3e8-242">Task 1 - Adding a New StoreController Class</span></span>

<span data-ttu-id="6e3e8-243">To zadanie spowoduje dodanie nowego kontrolera.</span><span class="sxs-lookup"><span data-stu-id="6e3e8-243">In this task, you will add a new Controller.</span></span>

1. <span data-ttu-id="6e3e8-244">Jeśli nie już otwarty, uruchom **VS Express for Web 2012**.</span><span class="sxs-lookup"><span data-stu-id="6e3e8-244">If not already open, start **VS Express for Web 2012**.</span></span>
2. <span data-ttu-id="6e3e8-245">W **pliku** menu, wybierz **Otwórz projekt**.</span><span class="sxs-lookup"><span data-stu-id="6e3e8-245">In the **File** menu, choose **Open Project**.</span></span> <span data-ttu-id="6e3e8-246">W oknie dialogowym Otwórz projekt, przejdź do **Source\Ex02 CreatingAController\Begin**, wybierz pozycję **Begin.sln** i kliknij przycisk **Otwórz**.</span><span class="sxs-lookup"><span data-stu-id="6e3e8-246">In the Open Project dialog, browse to **Source\Ex02-CreatingAController\Begin**, select **Begin.sln** and click **Open**.</span></span> <span data-ttu-id="6e3e8-247">Alternatywnie możesz nadal z rozwiązaniem uzyskany po zakończeniu pracy w poprzednim ćwiczeniu.</span><span class="sxs-lookup"><span data-stu-id="6e3e8-247">Alternatively, you may continue with the solution that you obtained after completing the previous exercise.</span></span>

   1. <span data-ttu-id="6e3e8-248">Po otwarciu dostarczonych **rozpocząć** rozwiązania, musisz pobrać niektórych brakujących pakietów NuGet aby kontynuować.</span><span class="sxs-lookup"><span data-stu-id="6e3e8-248">If you opened the provided **Begin** solution, you will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="6e3e8-249">Aby to zrobić, kliknij przycisk **projektu** menu i wybierz **Zarządzaj pakietami NuGet**.</span><span class="sxs-lookup"><span data-stu-id="6e3e8-249">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
   2. <span data-ttu-id="6e3e8-250">W **Zarządzaj pakietami NuGet** okna dialogowego, kliknij przycisk **przywrócić** celu pobieranie brakujących pakietów.</span><span class="sxs-lookup"><span data-stu-id="6e3e8-250">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
   3. <span data-ttu-id="6e3e8-251">Na koniec Skompiluj rozwiązanie, klikając **kompilacji** | **Kompiluj rozwiązanie**.</span><span class="sxs-lookup"><span data-stu-id="6e3e8-251">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

      > [!NOTE]
      > <span data-ttu-id="6e3e8-252">Jedną z zalet przy użyciu narzędzia NuGet jest, że nie masz do wysłania wszystkich bibliotek w projekcie, zmniejszenie jego rozmiar projektu.</span><span class="sxs-lookup"><span data-stu-id="6e3e8-252">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="6e3e8-253">Narzędzia Power NuGet określając wersje pakietów w pliku Packages.config, będzie można pobrać wymaganych bibliotek podczas pierwszego uruchomienia projektu.</span><span class="sxs-lookup"><span data-stu-id="6e3e8-253">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="6e3e8-254">Jest to, dlaczego konieczne będzie wykonanie tych kroków, po otwarciu istniejącego rozwiązania z tego laboratorium.</span><span class="sxs-lookup"><span data-stu-id="6e3e8-254">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
3. <span data-ttu-id="6e3e8-255">Dodaj nowy kontroler.</span><span class="sxs-lookup"><span data-stu-id="6e3e8-255">Add the new controller.</span></span> <span data-ttu-id="6e3e8-256">Aby to zrobić, kliknij prawym przyciskiem myszy **kontrolerów** folder w Eksploratorze rozwiązań wybierz **Dodaj** , a następnie **kontrolera** polecenia.</span><span class="sxs-lookup"><span data-stu-id="6e3e8-256">To do this, right-click the **Controllers** folder within the Solution Explorer, select **Add** and then the **Controller** command.</span></span> <span data-ttu-id="6e3e8-257">Zmień **nazwy kontrolera** do *StoreController*i kliknij przycisk **Dodaj**.</span><span class="sxs-lookup"><span data-stu-id="6e3e8-257">Change the **Controller Name** to *StoreController*, and click **Add**.</span></span>

    <span data-ttu-id="6e3e8-258">![Kontroler okno dialogowe Dodawanie](aspnet-mvc-4-fundamentals/_static/image8.png "kontrolera okno dialogowe Dodawanie")</span><span class="sxs-lookup"><span data-stu-id="6e3e8-258">![Add Controller Dialog](aspnet-mvc-4-fundamentals/_static/image8.png "Add Controller Dialog")</span></span>

    <span data-ttu-id="6e3e8-259">*Dodawanie kontrolera okna dialogowego*</span><span class="sxs-lookup"><span data-stu-id="6e3e8-259">*Add Controller Dialog*</span></span>

<a id="Ex2Task2"></a>

<a id="Task_2_-_Modifying_the_StoreControllers_Actions"></a>
#### <a name="task-2---modifying-the-storecontrollers-actions"></a><span data-ttu-id="6e3e8-260">Zadanie 2 - modyfikowanie StoreController akcje</span><span class="sxs-lookup"><span data-stu-id="6e3e8-260">Task 2 - Modifying the StoreController's Actions</span></span>

<span data-ttu-id="6e3e8-261">To zadanie, należy zmodyfikować metod kontrolera, które są nazywane **akcje**.</span><span class="sxs-lookup"><span data-stu-id="6e3e8-261">In this task, you will modify the Controller methods that are called **actions**.</span></span> <span data-ttu-id="6e3e8-262">Akcje są odpowiedzialne za obsługę żądań adresu URL i określanie zawartość, która ma zostać odesłany do przeglądarki lub użytkownik, który wywołał adres URL.</span><span class="sxs-lookup"><span data-stu-id="6e3e8-262">Actions are responsible for handling URL requests and determining the content that should be sent back to the browser or user that invoked the URL.</span></span>

1. <span data-ttu-id="6e3e8-263">**StoreController** klasa ma już **indeksu** metody.</span><span class="sxs-lookup"><span data-stu-id="6e3e8-263">The **StoreController** class already has an **Index** method.</span></span> <span data-ttu-id="6e3e8-264">Go w dalszej części tego laboratorium zostanie użyta do wdrożenia strony, który zawiera listę wszystkich genres magazynu utworów muzycznych.</span><span class="sxs-lookup"><span data-stu-id="6e3e8-264">You will use it later in this Lab to implement the page that lists all genres of the music store.</span></span> <span data-ttu-id="6e3e8-265">Teraz, po prostu zastąpić **indeksu** metodę z następującym kodem, które zwraca ciąg &quot;Hello z Store.Index()&quot;:</span><span class="sxs-lookup"><span data-stu-id="6e3e8-265">For now, just replace the **Index** method with the following code that returns a string &quot;Hello from Store.Index()&quot;:</span></span>

    <span data-ttu-id="6e3e8-266">(Fragment - kodu *podstawy platformy ASP.NET MVC 4 - indeksu StoreController Ex2*)</span><span class="sxs-lookup"><span data-stu-id="6e3e8-266">(Code Snippet - *ASP.NET MVC 4 Fundamentals - Ex2 StoreController Index*)</span></span>


~~~
[!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample2.cs)]
~~~
2. <span data-ttu-id="6e3e8-267">Dodaj **Przeglądaj** i **szczegóły** metody.</span><span class="sxs-lookup"><span data-stu-id="6e3e8-267">Add **Browse** and **Details** methods.</span></span> <span data-ttu-id="6e3e8-268">Aby to zrobić, Dodaj następujący kod, aby **StoreController**:</span><span class="sxs-lookup"><span data-stu-id="6e3e8-268">To do this, add the following code to the **StoreController**:</span></span>

    <span data-ttu-id="6e3e8-269">(Fragment - kodu *podstawy platformy ASP.NET MVC 4 - Ex2 StoreController BrowseAndDetails*)</span><span class="sxs-lookup"><span data-stu-id="6e3e8-269">(Code Snippet - *ASP.NET MVC 4 Fundamentals - Ex2 StoreController BrowseAndDetails*)</span></span>


~~~
[!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample3.cs)]
~~~

<a id="Ex2Task3"></a>

<a id="Task_3_-_Running_the_Application"></a>
#### <a name="task-3---running-the-application"></a><span data-ttu-id="6e3e8-270">Zadanie 3 — uruchamianie aplikacji</span><span class="sxs-lookup"><span data-stu-id="6e3e8-270">Task 3 - Running the Application</span></span>

<span data-ttu-id="6e3e8-271">To zadanie będzie Wypróbuj tę aplikację w przeglądarce sieci web.</span><span class="sxs-lookup"><span data-stu-id="6e3e8-271">In this task, you will try out the Application in a web browser.</span></span>

1. <span data-ttu-id="6e3e8-272">Naciśnij klawisz **F5** do uruchomienia aplikacji.</span><span class="sxs-lookup"><span data-stu-id="6e3e8-272">Press **F5** to run the Application.</span></span>
2. <span data-ttu-id="6e3e8-273">Projekt jest uruchamiany w **Home** strony.</span><span class="sxs-lookup"><span data-stu-id="6e3e8-273">The project starts in the **Home** page.</span></span> <span data-ttu-id="6e3e8-274">Zmień adres URL, aby sprawdzić implementacji każdej akcji.</span><span class="sxs-lookup"><span data-stu-id="6e3e8-274">Change the URL to verify each action's implementation.</span></span>

    1. <span data-ttu-id="6e3e8-275">**/ Przechowywania**.</span><span class="sxs-lookup"><span data-stu-id="6e3e8-275">**/Store**.</span></span> <span data-ttu-id="6e3e8-276">Zobaczysz  **&quot;Hello z Store.Index()&quot;**.</span><span class="sxs-lookup"><span data-stu-id="6e3e8-276">You will see **&quot;Hello from Store.Index()&quot;**.</span></span>
    2. <span data-ttu-id="6e3e8-277">**/ Magazynu/Przeglądaj**.</span><span class="sxs-lookup"><span data-stu-id="6e3e8-277">**/Store/Browse**.</span></span> <span data-ttu-id="6e3e8-278">Zobaczysz  **&quot;Hello z Store.Browse()&quot;**.</span><span class="sxs-lookup"><span data-stu-id="6e3e8-278">You will see **&quot;Hello from Store.Browse()&quot;**.</span></span>
    3. <span data-ttu-id="6e3e8-279">**/ / Szczegóły magazynu**.</span><span class="sxs-lookup"><span data-stu-id="6e3e8-279">**/Store/Details**.</span></span> <span data-ttu-id="6e3e8-280">Zobaczysz  **&quot;Hello z Store.Details()&quot;**.</span><span class="sxs-lookup"><span data-stu-id="6e3e8-280">You will see **&quot;Hello from Store.Details()&quot;**.</span></span>

        <span data-ttu-id="6e3e8-281">![Przeglądanie StoreBrowse](aspnet-mvc-4-fundamentals/_static/image9.png "przeglądanie StoreBrowse")</span><span class="sxs-lookup"><span data-stu-id="6e3e8-281">![Browsing StoreBrowse](aspnet-mvc-4-fundamentals/_static/image9.png "Browsing StoreBrowse")</span></span>

        <span data-ttu-id="6e3e8-282">*Przeglądanie /Store/Browse*</span><span class="sxs-lookup"><span data-stu-id="6e3e8-282">*Browsing /Store/Browse*</span></span>
3. <span data-ttu-id="6e3e8-283">Zamknij przeglądarkę.</span><span class="sxs-lookup"><span data-stu-id="6e3e8-283">Close the browser.</span></span>

<a id="Exercise3"></a>

<a id="Exercise_3_Passing_parameters_to_a_Controller"></a>
### <a name="exercise-3-passing-parameters-to-a-controller"></a><span data-ttu-id="6e3e8-284">Ćwiczenie 3: Przekazywanie parametrów do kontrolera</span><span class="sxs-lookup"><span data-stu-id="6e3e8-284">Exercise 3: Passing parameters to a Controller</span></span>

<span data-ttu-id="6e3e8-285">Do tej pory mieć zostały zwracanie stałej ciągów z kontrolerów.</span><span class="sxs-lookup"><span data-stu-id="6e3e8-285">Until now, you have been returning constant strings from the Controllers.</span></span> <span data-ttu-id="6e3e8-286">W tym ćwiczeniu dowiesz się, jak do przekazania parametrów do kontrolera przy użyciu adresu URL i querystring, a następnie sprawdzając akcje metody odpowiedzi z tekstem w przeglądarce.</span><span class="sxs-lookup"><span data-stu-id="6e3e8-286">In this exercise you will learn how to pass parameters to a Controller using the URL and querystring, and then making the method actions respond with text to the browser.</span></span>

<a id="Ex3Task1"></a>

<a id="Task_1_-_Adding_Genre_Parameter_to_StoreController"></a>
#### <a name="task-1---adding-genre-parameter-to-storecontroller"></a><span data-ttu-id="6e3e8-287">Zadanie 1 — Dodawanie parametru Genre StoreController</span><span class="sxs-lookup"><span data-stu-id="6e3e8-287">Task 1 - Adding Genre Parameter to StoreController</span></span>

<span data-ttu-id="6e3e8-288">W tym zadaniu użyjesz **querystring** wysłać parametry **Przeglądaj** metody akcji w **StoreController**.</span><span class="sxs-lookup"><span data-stu-id="6e3e8-288">In this task, you will use the **querystring** to send parameters to the **Browse** action method in the **StoreController**.</span></span>

1. <span data-ttu-id="6e3e8-289">Jeśli nie już otwarty, uruchom **VS Express for Web**.</span><span class="sxs-lookup"><span data-stu-id="6e3e8-289">If not already open, start **VS Express for Web**.</span></span>
2. <span data-ttu-id="6e3e8-290">W **pliku** menu, wybierz **Otwórz projekt**.</span><span class="sxs-lookup"><span data-stu-id="6e3e8-290">In the **File** menu, choose **Open Project**.</span></span> <span data-ttu-id="6e3e8-291">W oknie dialogowym Otwórz projekt, przejdź do **Source\Ex03 PassingParametersToAController\Begin**, wybierz pozycję **Begin.sln** i kliknij przycisk **Otwórz**.</span><span class="sxs-lookup"><span data-stu-id="6e3e8-291">In the Open Project dialog, browse to **Source\Ex03-PassingParametersToAController\Begin**, select **Begin.sln** and click **Open**.</span></span> <span data-ttu-id="6e3e8-292">Alternatywnie możesz nadal z rozwiązaniem uzyskany po zakończeniu pracy w poprzednim ćwiczeniu.</span><span class="sxs-lookup"><span data-stu-id="6e3e8-292">Alternatively, you may continue with the solution that you obtained after completing the previous exercise.</span></span>

   1. <span data-ttu-id="6e3e8-293">Po otwarciu dostarczonych **rozpocząć** rozwiązania, musisz pobrać niektórych brakujących pakietów NuGet aby kontynuować.</span><span class="sxs-lookup"><span data-stu-id="6e3e8-293">If you opened the provided **Begin** solution, you will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="6e3e8-294">Aby to zrobić, kliknij przycisk **projektu** menu i wybierz **Zarządzaj pakietami NuGet**.</span><span class="sxs-lookup"><span data-stu-id="6e3e8-294">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
   2. <span data-ttu-id="6e3e8-295">W **Zarządzaj pakietami NuGet** okna dialogowego, kliknij przycisk **przywrócić** celu pobieranie brakujących pakietów.</span><span class="sxs-lookup"><span data-stu-id="6e3e8-295">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
   3. <span data-ttu-id="6e3e8-296">Na koniec Skompiluj rozwiązanie, klikając **kompilacji** | **Kompiluj rozwiązanie**.</span><span class="sxs-lookup"><span data-stu-id="6e3e8-296">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

      > [!NOTE]
      > <span data-ttu-id="6e3e8-297">Jedną z zalet przy użyciu narzędzia NuGet jest, że nie masz do wysłania wszystkich bibliotek w projekcie, zmniejszenie jego rozmiar projektu.</span><span class="sxs-lookup"><span data-stu-id="6e3e8-297">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="6e3e8-298">Narzędzia Power NuGet określając wersje pakietów w pliku Packages.config, będzie można pobrać wymaganych bibliotek podczas pierwszego uruchomienia projektu.</span><span class="sxs-lookup"><span data-stu-id="6e3e8-298">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="6e3e8-299">Jest to, dlaczego konieczne będzie wykonanie tych kroków, po otwarciu istniejącego rozwiązania z tego laboratorium.</span><span class="sxs-lookup"><span data-stu-id="6e3e8-299">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
3. <span data-ttu-id="6e3e8-300">Otwórz **StoreController** klasy.</span><span class="sxs-lookup"><span data-stu-id="6e3e8-300">Open **StoreController** class.</span></span> <span data-ttu-id="6e3e8-301">Aby to zrobić, w **Eksploratora rozwiązań**, rozwiń węzeł **kontrolerów** folder i kliknij dwukrotnie **StoreController.cs**.</span><span class="sxs-lookup"><span data-stu-id="6e3e8-301">To do this, in the **Solution Explorer**, expand the **Controllers** folder and double-click **StoreController.cs**.</span></span>
4. <span data-ttu-id="6e3e8-302">Zmień **Przeglądaj** metody, dodając parametr typu string na żądanie dla określonego rodzaju.</span><span class="sxs-lookup"><span data-stu-id="6e3e8-302">Change the **Browse** method, adding a string parameter to request for a specific genre.</span></span> <span data-ttu-id="6e3e8-303">ASP.NET MVC zostanie automatycznie przekazać żadnych querystring lub przesłanie formularza parametrów o nazwie **genre** do tej metody akcji, gdy została wywołana.</span><span class="sxs-lookup"><span data-stu-id="6e3e8-303">ASP.NET MVC will automatically pass any querystring or form post parameters named **genre** to this action method when invoked.</span></span> <span data-ttu-id="6e3e8-304">Aby to zrobić, Zamień **Przeglądaj** metodę z następującym kodem:</span><span class="sxs-lookup"><span data-stu-id="6e3e8-304">To do this, replace the **Browse** method with the following code:</span></span>

    <span data-ttu-id="6e3e8-305">(Fragment - kodu *podstawy platformy ASP.NET MVC 4 - Ex3 StoreController BrowseMethod*)</span><span class="sxs-lookup"><span data-stu-id="6e3e8-305">(Code Snippet - *ASP.NET MVC 4 Fundamentals - Ex3 StoreController BrowseMethod*)</span></span>


~~~
[!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample4.cs)]

> [!NOTE]
> You are using the **HttpUtility.HtmlEncode** utility method to prevents users from injecting Javascript into the View with a link like **/Store/Browse?Genre=&lt;script&gt;window.location='[http://hackersite.com](http://hackersite.com)'&lt;/script&gt;**.
> 
> For further explanation, please visit [this msdn article](https://msdn.microsoft.com/library/a2a4yykt(v=VS.80).aspx).
~~~

<a id="Ex3Task2"></a>

<a id="Task_2_-_Running_the_Application"></a>
#### <a name="task-2---running-the-application"></a><span data-ttu-id="6e3e8-306">Zadanie 2 — w uruchomionej aplikacji</span><span class="sxs-lookup"><span data-stu-id="6e3e8-306">Task 2 - Running the Application</span></span>

<span data-ttu-id="6e3e8-307">W tym zadaniu zostanie wypróbowanie aplikacji w przeglądarce sieci web i używać **genre** parametru.</span><span class="sxs-lookup"><span data-stu-id="6e3e8-307">In this task, you will try out the Application in a web browser and use the **genre** parameter.</span></span>

1. <span data-ttu-id="6e3e8-308">Naciśnij klawisz **F5** do uruchomienia aplikacji.</span><span class="sxs-lookup"><span data-stu-id="6e3e8-308">Press **F5** to run the Application.</span></span>
2. <span data-ttu-id="6e3e8-309">Projekt jest uruchamiany w **Home** strony.</span><span class="sxs-lookup"><span data-stu-id="6e3e8-309">The project starts in the **Home** page.</span></span> <span data-ttu-id="6e3e8-310">Zmień adres URL do   */przechowywania/Przeglądaj? Genre = Disco* można zweryfikować, że akcja otrzyma parametru genre.</span><span class="sxs-lookup"><span data-stu-id="6e3e8-310">Change the URL to */Store/Browse?Genre=Disco* to verify that the action receives the genre parameter.</span></span>

    <span data-ttu-id="6e3e8-311">![Przeglądanie StoreBrowseGenre = Disco](aspnet-mvc-4-fundamentals/_static/image10.png "przeglądanie StoreBrowseGenre = Disco")</span><span class="sxs-lookup"><span data-stu-id="6e3e8-311">![Browsing StoreBrowseGenre=Disco](aspnet-mvc-4-fundamentals/_static/image10.png "Browsing StoreBrowseGenre=Disco")</span></span>

    <span data-ttu-id="6e3e8-312">*Przeglądanie /Store/Browse? Genre = Disco*</span><span class="sxs-lookup"><span data-stu-id="6e3e8-312">*Browsing /Store/Browse?Genre=Disco*</span></span>
3. <span data-ttu-id="6e3e8-313">Zamknij przeglądarkę.</span><span class="sxs-lookup"><span data-stu-id="6e3e8-313">Close the browser.</span></span>

<a id="Ex3Task3"></a>

<a id="Task_3_-_Adding_an_Id_Parameter_Embedded_in_the_URL"></a>
#### <a name="task-3---adding-an-id-parameter-embedded-in-the-url"></a><span data-ttu-id="6e3e8-314">Zadanie 3. Dodawanie parametru identyfikatora osadzone w adresie URL</span><span class="sxs-lookup"><span data-stu-id="6e3e8-314">Task 3 - Adding an Id Parameter Embedded in the URL</span></span>

<span data-ttu-id="6e3e8-315">W tym zadaniu użyjesz **adres URL** do przekazania **identyfikator** parametr **szczegóły** metody akcji **StoreController**.</span><span class="sxs-lookup"><span data-stu-id="6e3e8-315">In this task, you will use the **URL** to pass an **Id** parameter to the **Details** action method of the **StoreController**.</span></span> <span data-ttu-id="6e3e8-316">ASP.NET MVC domyślnej konwencji routingu jest Traktuj segment adresu URL po nazwy metody akcji, jako parametr o nazwie **identyfikator**. Jeśli metodę akcji ma parametr o nazwie identyfikator, ASP.NET MVC zostanie automatycznie przekazuj segment adresu URL do użytkownika jako parametr.</span><span class="sxs-lookup"><span data-stu-id="6e3e8-316">ASP.NET MVC's default routing convention is to treat the segment of a URL after the action method name as a parameter named **Id**. If your action method has parameter named Id, then ASP.NET MVC will automatically pass the URL segment to you as a parameter.</span></span> <span data-ttu-id="6e3e8-317">W adresie URL **5-magazynu/szczegóły**, **identyfikator** zostanie potraktowany jako **5**.</span><span class="sxs-lookup"><span data-stu-id="6e3e8-317">In the URL **Store/Details/5**, **Id** will be interpreted as **5**.</span></span>

1. <span data-ttu-id="6e3e8-318">Zmień **szczegóły** metody **StoreController**, dodając **int** parametr o nazwie **identyfikator**. Aby to zrobić, Zamień **szczegóły** metodę z następującym kodem:</span><span class="sxs-lookup"><span data-stu-id="6e3e8-318">Change the **Details** method of the **StoreController**, adding an **int** parameter called **id**. To do this, replace the **Details** method with the following code:</span></span>

    <span data-ttu-id="6e3e8-319">(Fragment - kodu *podstawy platformy ASP.NET MVC 4 - Ex3 StoreController DetailsMethod*)</span><span class="sxs-lookup"><span data-stu-id="6e3e8-319">(Code Snippet - *ASP.NET MVC 4 Fundamentals - Ex3 StoreController DetailsMethod*)</span></span>


~~~
[!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample5.cs)]
~~~

<a id="Ex3Task4"></a>

<a id="Task_4_-_Running_the_Application"></a>
#### <a name="task-4---running-the-application"></a><span data-ttu-id="6e3e8-320">Zadanie 4 — uruchamianie aplikacji</span><span class="sxs-lookup"><span data-stu-id="6e3e8-320">Task 4 - Running the Application</span></span>

<span data-ttu-id="6e3e8-321">W tym zadaniu zostanie wypróbowanie aplikacji w przeglądarce sieci web i używać **identyfikator** parametru.</span><span class="sxs-lookup"><span data-stu-id="6e3e8-321">In this task, you will try out the Application in a web browser and use the **Id** parameter.</span></span>

1. <span data-ttu-id="6e3e8-322">Naciśnij klawisz **F5** do uruchomienia aplikacji.</span><span class="sxs-lookup"><span data-stu-id="6e3e8-322">Press **F5** to run the Application.</span></span>
2. <span data-ttu-id="6e3e8-323">Projekt jest uruchamiany w **Home** strony.</span><span class="sxs-lookup"><span data-stu-id="6e3e8-323">The project starts in the **Home** page.</span></span> <span data-ttu-id="6e3e8-324">Zmień adres URL do */Store/Details/5* można zweryfikować, że akcja otrzyma parametru identyfikatora.</span><span class="sxs-lookup"><span data-stu-id="6e3e8-324">Change the URL to */Store/Details/5* to verify that the action receives the id parameter.</span></span>

    <span data-ttu-id="6e3e8-325">![Przeglądanie StoreDetails5](aspnet-mvc-4-fundamentals/_static/image11.png "przeglądanie StoreDetails5")</span><span class="sxs-lookup"><span data-stu-id="6e3e8-325">![Browsing StoreDetails5](aspnet-mvc-4-fundamentals/_static/image11.png "Browsing StoreDetails5")</span></span>

    <span data-ttu-id="6e3e8-326">*Przeglądanie /Store/Details/5*</span><span class="sxs-lookup"><span data-stu-id="6e3e8-326">*Browsing /Store/Details/5*</span></span>

<a id="Exercise4"></a>

<a id="Exercise_4_Creating_a_View"></a>
### <a name="exercise-4-creating-a-view"></a><span data-ttu-id="6e3e8-327">Ćwiczenie 4: Tworzenie widoku</span><span class="sxs-lookup"><span data-stu-id="6e3e8-327">Exercise 4: Creating a View</span></span>

<span data-ttu-id="6e3e8-328">Do tej pory ma zostały zwracanie ciągów z akcji kontrolera.</span><span class="sxs-lookup"><span data-stu-id="6e3e8-328">So far you have been returning strings from controller actions.</span></span> <span data-ttu-id="6e3e8-329">Chociaż jest to wygodny sposób zrozumienia, jak działają kontrolery, jest nie jak rzeczywistych aplikacji sieci Web są tworzone.</span><span class="sxs-lookup"><span data-stu-id="6e3e8-329">Although that is a useful way of understanding how controllers work, it is not how your real Web applications are built.</span></span> <span data-ttu-id="6e3e8-330">Widoki są składnikami, które zapewniają lepszym rozwiązaniem dla generowania kodu HTML do przeglądarki przy użyciu plików szablonów.</span><span class="sxs-lookup"><span data-stu-id="6e3e8-330">Views are components that provide a better approach for generating HTML back to the browser with the use of template files.</span></span>

<span data-ttu-id="6e3e8-331">W tym ćwiczeniu dowiesz się, jak dodać układ strony wzorcowej można skonfigurować szablon dla typowych zawartość HTML, arkusz stylów w celu zwiększenia wyglądu i działania witryny, a na koniec szablon widoku, aby włączyć HomeController do zwrócenia HTML.</span><span class="sxs-lookup"><span data-stu-id="6e3e8-331">In this exercise you will learn how to add a layout master page to setup a template for common HTML content, a StyleSheet to enhance the look and feel of the site and finally a View template to enable HomeController to return HTML.</span></span>

<a id="Ex4Task1"></a>

<a id="Task_1_-_Modifying_the_file__layoutcshtml"></a>
#### <a name="task-1---modifying-the-file-layoutcshtml"></a><span data-ttu-id="6e3e8-332">Zadanie 1 - modyfikowania pliku \_layout.cshtml</span><span class="sxs-lookup"><span data-stu-id="6e3e8-332">Task 1 - Modifying the file \_layout.cshtml</span></span>

<span data-ttu-id="6e3e8-333">Plik **~/Views/Shared/\_layout.cshtml** pozwala skonfigurować szablon HTML wspólne korzystanie w całej witryny sieci Web.</span><span class="sxs-lookup"><span data-stu-id="6e3e8-333">The file **~/Views/Shared/\_layout.cshtml** allows you to setup a template for common HTML to use across the entire website.</span></span> <span data-ttu-id="6e3e8-334">W tym zadaniu doda układ strony wzorcowej przy użyciu wspólnej nagłówka z łączami do obszar strony głównej i magazynu.</span><span class="sxs-lookup"><span data-stu-id="6e3e8-334">In this task you will add a layout master page with a common header with links to the Home page and Store area.</span></span>

1. <span data-ttu-id="6e3e8-335">Jeśli nie już otwarty, uruchom **VS Express for Web**.</span><span class="sxs-lookup"><span data-stu-id="6e3e8-335">If not already open, start **VS Express for Web**.</span></span>
2. <span data-ttu-id="6e3e8-336">W **pliku** menu, wybierz **Otwórz projekt**.</span><span class="sxs-lookup"><span data-stu-id="6e3e8-336">In the **File** menu, choose **Open Project**.</span></span> <span data-ttu-id="6e3e8-337">W oknie dialogowym Otwórz projekt, przejdź do **Source\Ex04 CreatingAView\Begin**, wybierz pozycję **Begin.sln** i kliknij przycisk **Otwórz**.</span><span class="sxs-lookup"><span data-stu-id="6e3e8-337">In the Open Project dialog, browse to **Source\Ex04-CreatingAView\Begin**, select **Begin.sln** and click **Open**.</span></span> <span data-ttu-id="6e3e8-338">Alternatywnie możesz nadal z rozwiązaniem uzyskany po zakończeniu pracy w poprzednim ćwiczeniu.</span><span class="sxs-lookup"><span data-stu-id="6e3e8-338">Alternatively, you may continue with the solution that you obtained after completing the previous exercise.</span></span>

   1. <span data-ttu-id="6e3e8-339">Po otwarciu dostarczonych **rozpocząć** rozwiązania, musisz pobrać niektórych brakujących pakietów NuGet aby kontynuować.</span><span class="sxs-lookup"><span data-stu-id="6e3e8-339">If you opened the provided **Begin** solution, you will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="6e3e8-340">Aby to zrobić, kliknij przycisk **projektu** menu i wybierz **Zarządzaj pakietami NuGet**.</span><span class="sxs-lookup"><span data-stu-id="6e3e8-340">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
   2. <span data-ttu-id="6e3e8-341">W **Zarządzaj pakietami NuGet** okna dialogowego, kliknij przycisk **przywrócić** celu pobieranie brakujących pakietów.</span><span class="sxs-lookup"><span data-stu-id="6e3e8-341">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
   3. <span data-ttu-id="6e3e8-342">Na koniec Skompiluj rozwiązanie, klikając **kompilacji** | **Kompiluj rozwiązanie**.</span><span class="sxs-lookup"><span data-stu-id="6e3e8-342">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

      > [!NOTE]
      > <span data-ttu-id="6e3e8-343">Jedną z zalet przy użyciu narzędzia NuGet jest, że nie masz do wysłania wszystkich bibliotek w projekcie, zmniejszenie jego rozmiar projektu.</span><span class="sxs-lookup"><span data-stu-id="6e3e8-343">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="6e3e8-344">Narzędzia Power NuGet określając wersje pakietów w pliku Packages.config, będzie można pobrać wymaganych bibliotek podczas pierwszego uruchomienia projektu.</span><span class="sxs-lookup"><span data-stu-id="6e3e8-344">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="6e3e8-345">Jest to, dlaczego konieczne będzie wykonanie tych kroków, po otwarciu istniejącego rozwiązania z tego laboratorium.</span><span class="sxs-lookup"><span data-stu-id="6e3e8-345">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
3. <span data-ttu-id="6e3e8-346">Plik  <strong>\_layout.cshtml</strong> zawiera układ kontenera HTML dla wszystkich stron w witrynie.</span><span class="sxs-lookup"><span data-stu-id="6e3e8-346">The file <strong>\_layout.cshtml</strong> contains the HTML container layout for all pages on the site.</span></span> <span data-ttu-id="6e3e8-347">Obejmuje on <strong>&lt;html&gt;</strong> elementu w odpowiedzi HTML, jak również <strong>&lt;head&gt;</strong> i <strong>&lt;treści&gt;</strong> elementów.</span><span class="sxs-lookup"><span data-stu-id="6e3e8-347">It includes the <strong>&lt;html&gt;</strong> element for the HTML response, as well as the <strong>&lt;head&gt;</strong> and <strong>&lt;body&gt;</strong> elements.</span></span> <span data-ttu-id="6e3e8-348"><strong>@RenderBody()</strong> w kodzie HTML treści identyfikowania regionów tego widoku szablonów będzie można wypełnić się przy użyciu zawartości dynamicznej.</span><span class="sxs-lookup"><span data-stu-id="6e3e8-348"><strong>@RenderBody()</strong> within the HTML body identify regions that view templates will be able to fill in with dynamic content.</span></span>
   <span data-ttu-id="6e3e8-349">(C#)</span><span class="sxs-lookup"><span data-stu-id="6e3e8-349">(C#)</span></span>

    [!code-cshtml[Main](aspnet-mvc-4-fundamentals/samples/sample6.cshtml)]
4. <span data-ttu-id="6e3e8-350">Dodanie nagłówka wspólnego z łączami do obszar strony głównej i zapisać na wszystkich stron w witrynie.</span><span class="sxs-lookup"><span data-stu-id="6e3e8-350">Add a common header with links to the Home page and Store area on all pages in the site.</span></span> <span data-ttu-id="6e3e8-351">Aby to zrobić, Dodaj następujący kod poniżej &lt;treści&gt; instrukcji.</span><span class="sxs-lookup"><span data-stu-id="6e3e8-351">In order to do that, add the following code below &lt;body&gt; statement.</span></span>
   <span data-ttu-id="6e3e8-352">(C#)</span><span class="sxs-lookup"><span data-stu-id="6e3e8-352">(C#)</span></span>

    [!code-cshtml[Main](aspnet-mvc-4-fundamentals/samples/sample7.cshtml)]
5. <span data-ttu-id="6e3e8-353">Obejmują div renderowanie części treści każdej strony.</span><span class="sxs-lookup"><span data-stu-id="6e3e8-353">Include a div to render the body section of each page.</span></span> <span data-ttu-id="6e3e8-354">Zastąp  <strong>@RenderBody()</strong> następującym kodem higlighted: (C#)</span><span class="sxs-lookup"><span data-stu-id="6e3e8-354">Replace <strong>@RenderBody()</strong> with the following higlighted code: (C#)</span></span>

    [!code-cshtml[Main](aspnet-mvc-4-fundamentals/samples/sample8.cshtml)]

    > [!NOTE]
    > <span data-ttu-id="6e3e8-355">Czy wiesz?</span><span class="sxs-lookup"><span data-stu-id="6e3e8-355">Did you know?</span></span> <span data-ttu-id="6e3e8-356">Program Visual Studio 2012 ma fragmentów, które ułatwiają dodawania często używane kodu HTML i pliki kodu!</span><span class="sxs-lookup"><span data-stu-id="6e3e8-356">Visual Studio 2012 has snippets that make it easy to add commonly used code in HTML, code files and more!</span></span> <span data-ttu-id="6e3e8-357">Wypróbuj limit, wpisując **&lt;div&gt;** i naciskając klawisz **kartę** dwa razy, aby wstawić pełnego **div** tagu.</span><span class="sxs-lookup"><span data-stu-id="6e3e8-357">Try it out by typing **&lt;div&gt;** and pressing **TAB** twice to insert a complete **div** tag.</span></span>

<a id="Ex4Task2"></a>

<a id="Task_2_-_Adding_CSS_Stylesheet"></a>
#### <a name="task-2---adding-css-stylesheet"></a><span data-ttu-id="6e3e8-358">Zadanie 2 — Dodawanie arkusza stylów CSS</span><span class="sxs-lookup"><span data-stu-id="6e3e8-358">Task 2 - Adding CSS Stylesheet</span></span>

<span data-ttu-id="6e3e8-359">Pusty szablon projektu zawiera plik CSS bardzo prostsze, zawierający tylko stylów używany do wyświetlania podstawowych formularzy i komunikatów dotyczących sprawdzania poprawności.</span><span class="sxs-lookup"><span data-stu-id="6e3e8-359">The empty project template includes a very streamlined CSS file which just includes styles used to display basic forms and validation messages.</span></span> <span data-ttu-id="6e3e8-360">Aby poprawić wygląd i działanie witryny użyje dodatkowe CSS i obrazów (potencjalnie udostępnione przez projektanta).</span><span class="sxs-lookup"><span data-stu-id="6e3e8-360">You will use additional CSS and images (potentially provided by a designer) in order to enhance the look and feel of the site.</span></span>

<span data-ttu-id="6e3e8-361">To zadanie spowoduje dodanie arkusz stylów CSS do definiowania stylów witryny.</span><span class="sxs-lookup"><span data-stu-id="6e3e8-361">In this task, you will add a CSS stylesheet to define the styles of the site.</span></span>

1. <span data-ttu-id="6e3e8-362">Pliku CSS i obrazy są uwzględnione w **Source\Assets\Content** folder tego laboratorium.</span><span class="sxs-lookup"><span data-stu-id="6e3e8-362">The CSS file and images are included in the **Source\Assets\Content** folder of this Lab.</span></span> <span data-ttu-id="6e3e8-363">Aby dodać je do aplikacji, przeciągnij ich zawartość z **Eksploratora Windows** okna do **Eksploratora rozwiązań** w programie Visual Studio Express for Web, jak pokazano poniżej:</span><span class="sxs-lookup"><span data-stu-id="6e3e8-363">In order to add them to the application, drag their content from a **Windows Explorer** window into the **Solution Explorer** in Visual Studio Express for Web, as shown below:</span></span>

    <span data-ttu-id="6e3e8-364">![Przeciąganie styl treści](aspnet-mvc-4-fundamentals/_static/image12.png "przeciąganie styl treści")</span><span class="sxs-lookup"><span data-stu-id="6e3e8-364">![Dragging style contents](aspnet-mvc-4-fundamentals/_static/image12.png "Dragging style contents")</span></span>

    <span data-ttu-id="6e3e8-365">*Przeciąganie styl treści*</span><span class="sxs-lookup"><span data-stu-id="6e3e8-365">*Dragging style contents*</span></span>
2. <span data-ttu-id="6e3e8-366">Okno dialogowe z ostrzeżeniem pojawi się pytaniem o potwierdzenie zastąpić **Site.css** plików i niektórych istniejących obrazów.</span><span class="sxs-lookup"><span data-stu-id="6e3e8-366">A warning dialog will appear, asking for confirmation to replace **Site.css** file and some existing images.</span></span> <span data-ttu-id="6e3e8-367">Sprawdź **Zastosuj do wszystkich elementów** i kliknij przycisk **tak**.</span><span class="sxs-lookup"><span data-stu-id="6e3e8-367">Check **Apply to all items** and click **Yes**.</span></span>

<a id="Ex4Task3"></a>

<a id="Task_3_-_Adding_a_View_Template"></a>
#### <a name="task-3---adding-a-view-template"></a><span data-ttu-id="6e3e8-368">Zadanie 3. Dodawanie wyświetlanie szablonu</span><span class="sxs-lookup"><span data-stu-id="6e3e8-368">Task 3 - Adding a View Template</span></span>

<span data-ttu-id="6e3e8-369">To zadanie spowoduje dodanie szablonu widoku do generowania odpowiedzi HTML, który będzie używany przez układ strony wzorcowej i CSS dodane w tym ćwiczeniu.</span><span class="sxs-lookup"><span data-stu-id="6e3e8-369">In this task, you will add a View template to generate the HTML response that will use the layout master page and CSS added in this exercise.</span></span>

1. <span data-ttu-id="6e3e8-370">Aby użyć szablonu widoku podczas przeglądania strony głównej, najpierw musisz wskazują, że zamiast zwracać ciąg, **indeksu HomeController** metoda zwróci **widoku**.</span><span class="sxs-lookup"><span data-stu-id="6e3e8-370">To use a View template when browsing the site's home page, you will first need to indicate that instead of returning a string, the **HomeController Index** method will return a **View**.</span></span> <span data-ttu-id="6e3e8-371">Otwórz **HomeController** klasy i zmień jego **indeksu** metodę, aby zwrócić **ActionResult**, i zwróć **View()**.</span><span class="sxs-lookup"><span data-stu-id="6e3e8-371">Open **HomeController** class and change its **Index** method to return an **ActionResult**, and have it return **View()**.</span></span>

    <span data-ttu-id="6e3e8-372">(Fragment - kodu *podstawy platformy ASP.NET MVC 4 - indeksu HomeController Ex4*)</span><span class="sxs-lookup"><span data-stu-id="6e3e8-372">(Code Snippet - *ASP.NET MVC 4 Fundamentals - Ex4 HomeController Index*)</span></span>


~~~
[!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample9.cs)]
~~~
2. <span data-ttu-id="6e3e8-373">Teraz musisz dodać odpowiedni szablon widoku.</span><span class="sxs-lookup"><span data-stu-id="6e3e8-373">Now, you need to add an appropriate View template.</span></span> <span data-ttu-id="6e3e8-374">Aby to zrobić, **kliknij prawym przyciskiem myszy** wewnątrz **indeksu** metody akcji i wybierz **Dodaj widok**.</span><span class="sxs-lookup"><span data-stu-id="6e3e8-374">To do this, **right-click** inside the **Index** action method and select **Add View**.</span></span> <span data-ttu-id="6e3e8-375">Zostanie wyświetlone okno **Dodaj widok** okna dialogowego.</span><span class="sxs-lookup"><span data-stu-id="6e3e8-375">This will bring up the **Add View** dialog.</span></span>

    <span data-ttu-id="6e3e8-376">![Dodawanie widoku z w metodzie indeksu](aspnet-mvc-4-fundamentals/_static/image13.png "Dodawanie widoku z wewnątrz Index — metoda")</span><span class="sxs-lookup"><span data-stu-id="6e3e8-376">![Adding a View from within the Index method](aspnet-mvc-4-fundamentals/_static/image13.png "Adding a View from within the Index method")</span></span>

    <span data-ttu-id="6e3e8-377">*Dodawanie widoku z wewnątrz Index — metoda*</span><span class="sxs-lookup"><span data-stu-id="6e3e8-377">*Adding a View from within the Index method*</span></span>
3. <span data-ttu-id="6e3e8-378">**Dodaj widok** wyświetli się okno dialogowe, aby wygenerować plik szablonu widoku.</span><span class="sxs-lookup"><span data-stu-id="6e3e8-378">The **Add View** Dialog will appear to generate a View template file.</span></span> <span data-ttu-id="6e3e8-379">Domyślnie to okno dialogowe wstępnie wypełnia nazwę szablonu widoku, aby odpowiadały one metody akcji, która będzie go używać.</span><span class="sxs-lookup"><span data-stu-id="6e3e8-379">By default, this dialog pre-populates the name of the View template so that it matches the action method that will use it.</span></span> <span data-ttu-id="6e3e8-380">Ponieważ użyto **Dodaj widok** menu kontekstowego w **indeksu** metody akcji w obrębie HomeController **Dodaj widok** okno dialogowe ma indeks jako domyślną nazwę widoku.</span><span class="sxs-lookup"><span data-stu-id="6e3e8-380">Because you used the **Add View** context menu within the **Index** action method within the HomeController, the **Add View** dialog has Index as the default view name.</span></span> <span data-ttu-id="6e3e8-381">Kliknij przycisk **Dodaj**.</span><span class="sxs-lookup"><span data-stu-id="6e3e8-381">Click **Add**.</span></span>

    <span data-ttu-id="6e3e8-382">![Okno dialogowe dodawania widoku](aspnet-mvc-4-fundamentals/_static/image14.png "okno dialogowe dodawania widoku")</span><span class="sxs-lookup"><span data-stu-id="6e3e8-382">![Add View Dialog](aspnet-mvc-4-fundamentals/_static/image14.png "Add View Dialog")</span></span>

    <span data-ttu-id="6e3e8-383">*Okno dialogowe dodawania widoku*</span><span class="sxs-lookup"><span data-stu-id="6e3e8-383">*Add View Dialog*</span></span>
4. <span data-ttu-id="6e3e8-384">Visual Studio generuje **Index.cshtml** Wyświetl szablon wewnątrz **Views\Home** folder, a następnie go otwiera.</span><span class="sxs-lookup"><span data-stu-id="6e3e8-384">Visual Studio generates an **Index.cshtml** view template inside the **Views\Home** folder and then opens it.</span></span>

    <span data-ttu-id="6e3e8-385">![Strona główna widok indeksu utworzony](aspnet-mvc-4-fundamentals/_static/image15.png "widok Home Indeks utworzony")</span><span class="sxs-lookup"><span data-stu-id="6e3e8-385">![Home Index view created](aspnet-mvc-4-fundamentals/_static/image15.png "Home Index view created")</span></span>

    <span data-ttu-id="6e3e8-386">*Macierzysty widok indeksu utworzony*</span><span class="sxs-lookup"><span data-stu-id="6e3e8-386">*Home Index view created*</span></span>

    > [!NOTE]
    > <span data-ttu-id="6e3e8-387">Nazwa i lokalizacja **Index.cshtml** plik ma zastosowanie i jest zgodna z domyślnych konwencji nazewnictwa platformy ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="6e3e8-387">name and location of the **Index.cshtml** file is relevant and follows the default ASP.NET MVC naming conventions.</span></span>
    > 
    > <span data-ttu-id="6e3e8-388">Folder \Views\**Home** zgodna z nazwą kontrolera (**Home** kontrolera).</span><span class="sxs-lookup"><span data-stu-id="6e3e8-388">The folder \Views\**Home** matches the controller name (**Home** Controller).</span></span> <span data-ttu-id="6e3e8-389">Nazwa szablonu widoku (**indeksu**), metoda akcji kontrolera, które będą wyświetlane w widoku jest zgodna.</span><span class="sxs-lookup"><span data-stu-id="6e3e8-389">The View template name (**Index**), matches the controller action method which will be displaying the View.</span></span>
    > 
    > <span data-ttu-id="6e3e8-390">W ten sposób ASP.NET MVC pozwala uniknąć konieczności jawnego określania nazwy lub lokalizacji szablonu widoku, używając tę konwencję nazewnictwa do zwrócenia widoku.</span><span class="sxs-lookup"><span data-stu-id="6e3e8-390">This way, ASP.NET MVC avoids having to explicitly specify the name or location of a View template when using this naming convention to return a View.</span></span>
5. <span data-ttu-id="6e3e8-391">Wygenerowany szablon widoku jest oparty na  **\_layout.cshtml** wcześniej zdefiniowanego szablonu.</span><span class="sxs-lookup"><span data-stu-id="6e3e8-391">The generated View template is based on the **\_layout.cshtml** template earlier defined.</span></span> <span data-ttu-id="6e3e8-392">Zaktualizuj właściwość ViewBag.Title do **Home**i zmień głównej zawartości do **to strona główna**, jak pokazano w poniższym kodzie:</span><span class="sxs-lookup"><span data-stu-id="6e3e8-392">Update the ViewBag.Title property to **Home**, and change the main content to **This is the Home Page**, as shown in the code below:</span></span>


~~~
[!code-cshtml[Main](aspnet-mvc-4-fundamentals/samples/sample10.cshtml)]
~~~
6. <span data-ttu-id="6e3e8-393">Wybierz **MvcMusicStore** projekt w Eksploratorze rozwiązań i naciśnij klawisz **F5** do uruchomienia aplikacji.</span><span class="sxs-lookup"><span data-stu-id="6e3e8-393">Select **MvcMusicStore** project in the Solution Explorer and Press **F5** to run the Application.</span></span>

<a id="Ex4Task4"></a>

<a id="Task_4_Verification"></a>
#### <a name="task-4-verification"></a><span data-ttu-id="6e3e8-394">Zadanie 4: Weryfikacja</span><span class="sxs-lookup"><span data-stu-id="6e3e8-394">Task 4: Verification</span></span>

<span data-ttu-id="6e3e8-395">Aby sprawdzić, prawidłowo wykonane wszystkie kroki opisane w poprzednim ćwiczeniu, wykonać następujące czynności:</span><span class="sxs-lookup"><span data-stu-id="6e3e8-395">In order to verify that you have correctly performed all the steps in the previous exercise, proceed as follows:</span></span>

<span data-ttu-id="6e3e8-396">Z tą aplikacją otwarta w przeglądarce należy zauważyć, że:</span><span class="sxs-lookup"><span data-stu-id="6e3e8-396">With the application opened in a browser, you should note that:</span></span>

1. <span data-ttu-id="6e3e8-397">Metody akcji indeksu HomeController znaleźć i wyświetlić **\Views\Home\Index.cshtml** wyświetlić szablon, nawet jeśli kod wywołuje **zwracać View()**, ponieważ szablon widoku, a następnie standardowej konwencji nazewnictwa.</span><span class="sxs-lookup"><span data-stu-id="6e3e8-397">The HomeController's Index action method found and displayed the **\Views\Home\Index.cshtml** View template, even though the code called **return View()**, because the View template followed the standard naming convention.</span></span>
2. <span data-ttu-id="6e3e8-398">Strona główna wyświetla komunikat powitalny, zdefiniowanego w **\Views\Home\Index.cshtml** widok szablonu.</span><span class="sxs-lookup"><span data-stu-id="6e3e8-398">The Home Page displays the welcome message defined within the **\Views\Home\Index.cshtml** view template.</span></span>
3. <span data-ttu-id="6e3e8-399">Strona główna używa  **\_layout.cshtml** szablonu, co komunikat powitalny jest zawarty w układu standardowe witryny HTML.</span><span class="sxs-lookup"><span data-stu-id="6e3e8-399">The Home Page is using the **\_layout.cshtml** template, and so the welcome message is contained within the standard site HTML layout.</span></span>

    <span data-ttu-id="6e3e8-400">![Strona główna widoku indeksu przy użyciu zdefiniowanych LayoutPage i styl](aspnet-mvc-4-fundamentals/_static/image16.png "Home widoku indeksu przy użyciu zdefiniowanych LayoutPage i stylu")</span><span class="sxs-lookup"><span data-stu-id="6e3e8-400">![Home Index View using the defined LayoutPage and style](aspnet-mvc-4-fundamentals/_static/image16.png "Home Index View using the defined LayoutPage and style")</span></span>

    <span data-ttu-id="6e3e8-401">*Macierzysty widoku indeksu przy użyciu zdefiniowanych LayoutPage i stylu*</span><span class="sxs-lookup"><span data-stu-id="6e3e8-401">*Home Index View using the defined LayoutPage and style*</span></span>

<a id="Exercise5"></a>

<a id="Exercise_5_Creating_a_View_Model"></a>
### <a name="exercise-5-creating-a-view-model"></a><span data-ttu-id="6e3e8-402">Ćwiczenie 5: Tworzenie modelu widoku</span><span class="sxs-lookup"><span data-stu-id="6e3e8-402">Exercise 5: Creating a View Model</span></span>

<span data-ttu-id="6e3e8-403">Do tej pory wprowadzono widoków wyświetlić zapisane na stałe HTML, ale, aby można było utworzyć dynamicznych aplikacji sieci web, Wyświetl szablon powinien zostać wyświetlony informacji od kontrolera.</span><span class="sxs-lookup"><span data-stu-id="6e3e8-403">So far, you made your Views display hardcoded HTML, but, in order to create dynamic web applications, the View template should receive information from the Controller.</span></span> <span data-ttu-id="6e3e8-404">Jeden technika wspólnego do użycia w tym celu jest **ViewModel** wzorzec, dzięki czemu kontrolera spakować wszystkie informacje potrzebne do generowania odpowiednich odpowiedzi HTML.</span><span class="sxs-lookup"><span data-stu-id="6e3e8-404">One common technique to be used for that purpose is the **ViewModel** pattern, which allows a Controller to package up all the information needed to generate the appropriate HTML response.</span></span>

<span data-ttu-id="6e3e8-405">W tym ćwiczeniu dowiesz Tworzenie klasy ViewModel i dodać wymagane właściwości: numer gatunkami muzyki w magazynie i listę tych gatunkami muzyki.</span><span class="sxs-lookup"><span data-stu-id="6e3e8-405">In this exercise, you will learn how to create a ViewModel class and add the required properties: the number of genres in the store and a list of those genres.</span></span> <span data-ttu-id="6e3e8-406">Zostanie również zaktualizować StoreController do użycia ViewModel utworzony, a koniec utworzysz nowy szablon widoku, który spowoduje wyświetlenie właściwości wymienione na stronie.</span><span class="sxs-lookup"><span data-stu-id="6e3e8-406">You will also update the StoreController to use the created ViewModel, and finally, you will create a new View template that will display the mentioned properties in the page.</span></span>

<a id="Ex5Task1"></a>

<a id="Task_1_-_Creating_a_ViewModel_Class"></a>
#### <a name="task-1---creating-a-viewmodel-class"></a><span data-ttu-id="6e3e8-407">Zadanie 1 — Tworzenie klasy ViewModel</span><span class="sxs-lookup"><span data-stu-id="6e3e8-407">Task 1 - Creating a ViewModel Class</span></span>

<span data-ttu-id="6e3e8-408">W ramach tego zadania spowoduje utworzenie klasy ViewModel, która będzie wdrożyć scenariusz dla listy genre magazynu.</span><span class="sxs-lookup"><span data-stu-id="6e3e8-408">In this task, you will create a ViewModel class that will implement the Store genre listing scenario.</span></span>

1. <span data-ttu-id="6e3e8-409">Jeśli nie już otwarty, uruchom **VS Express for Web**.</span><span class="sxs-lookup"><span data-stu-id="6e3e8-409">If not already open, start **VS Express for Web**.</span></span>
2. <span data-ttu-id="6e3e8-410">W **pliku** menu, wybierz **Otwórz projekt**.</span><span class="sxs-lookup"><span data-stu-id="6e3e8-410">In the **File** menu, choose **Open Project**.</span></span> <span data-ttu-id="6e3e8-411">W oknie dialogowym Otwórz projekt, przejdź do **Source\Ex05 CreatingAViewModel\Begin**, wybierz pozycję **Begin.sln** i kliknij przycisk **Otwórz**.</span><span class="sxs-lookup"><span data-stu-id="6e3e8-411">In the Open Project dialog, browse to **Source\Ex05-CreatingAViewModel\Begin**, select **Begin.sln** and click **Open**.</span></span> <span data-ttu-id="6e3e8-412">Alternatywnie możesz nadal z rozwiązaniem uzyskany po zakończeniu pracy w poprzednim ćwiczeniu.</span><span class="sxs-lookup"><span data-stu-id="6e3e8-412">Alternatively, you may continue with the solution that you obtained after completing the previous exercise.</span></span>

   1. <span data-ttu-id="6e3e8-413">Po otwarciu dostarczonych **rozpocząć** rozwiązania, musisz pobrać niektórych brakujących pakietów NuGet aby kontynuować.</span><span class="sxs-lookup"><span data-stu-id="6e3e8-413">If you opened the provided **Begin** solution, you will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="6e3e8-414">Aby to zrobić, kliknij przycisk **projektu** menu i wybierz **Zarządzaj pakietami NuGet**.</span><span class="sxs-lookup"><span data-stu-id="6e3e8-414">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
   2. <span data-ttu-id="6e3e8-415">W **Zarządzaj pakietami NuGet** okna dialogowego, kliknij przycisk **przywrócić** celu pobieranie brakujących pakietów.</span><span class="sxs-lookup"><span data-stu-id="6e3e8-415">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
   3. <span data-ttu-id="6e3e8-416">Na koniec Skompiluj rozwiązanie, klikając **kompilacji** | **Kompiluj rozwiązanie**.</span><span class="sxs-lookup"><span data-stu-id="6e3e8-416">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

      > [!NOTE]
      > <span data-ttu-id="6e3e8-417">Jedną z zalet przy użyciu narzędzia NuGet jest, że nie masz do wysłania wszystkich bibliotek w projekcie, zmniejszenie jego rozmiar projektu.</span><span class="sxs-lookup"><span data-stu-id="6e3e8-417">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="6e3e8-418">Narzędzia Power NuGet określając wersje pakietów w pliku Packages.config, będzie można pobrać wymaganych bibliotek podczas pierwszego uruchomienia projektu.</span><span class="sxs-lookup"><span data-stu-id="6e3e8-418">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="6e3e8-419">Jest to, dlaczego konieczne będzie wykonanie tych kroków, po otwarciu istniejącego rozwiązania z tego laboratorium.</span><span class="sxs-lookup"><span data-stu-id="6e3e8-419">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
3. <span data-ttu-id="6e3e8-420">Utwórz **ViewModels** folder do przechowywania ViewModel.</span><span class="sxs-lookup"><span data-stu-id="6e3e8-420">Create a **ViewModels** folder to hold the ViewModel.</span></span> <span data-ttu-id="6e3e8-421">Aby to zrobić, kliknij prawym przyciskiem myszy najwyższego poziomu **MvcMusicStore** projektu, zaznacz **Dodaj** , a następnie **nowy Folder**.</span><span class="sxs-lookup"><span data-stu-id="6e3e8-421">To do this, right-click the top-level **MvcMusicStore** project, select **Add** and then **New Folder**.</span></span>

    <span data-ttu-id="6e3e8-422">![Dodawanie nowego folderu](aspnet-mvc-4-fundamentals/_static/image17.png "dodawanie nowy folder")</span><span class="sxs-lookup"><span data-stu-id="6e3e8-422">![Adding a new folder](aspnet-mvc-4-fundamentals/_static/image17.png "Adding a new folder")</span></span>

    <span data-ttu-id="6e3e8-423">*Dodawanie nowego folderu*</span><span class="sxs-lookup"><span data-stu-id="6e3e8-423">*Adding a new folder*</span></span>
4. <span data-ttu-id="6e3e8-424">Nazwa folderu *ViewModels*.</span><span class="sxs-lookup"><span data-stu-id="6e3e8-424">Name the folder *ViewModels*.</span></span>

    <span data-ttu-id="6e3e8-425">![ViewModels folder w Eksploratorze rozwiązań](aspnet-mvc-4-fundamentals/_static/image18.png "ViewModels folder w Eksploratorze rozwiązań")</span><span class="sxs-lookup"><span data-stu-id="6e3e8-425">![ViewModels folder in Solution Explorer](aspnet-mvc-4-fundamentals/_static/image18.png "ViewModels folder in Solution Explorer")</span></span>

    <span data-ttu-id="6e3e8-426">*ViewModels folder w Eksploratorze rozwiązań*</span><span class="sxs-lookup"><span data-stu-id="6e3e8-426">*ViewModels folder in Solution Explorer*</span></span>
5. <span data-ttu-id="6e3e8-427">Utwórz **ViewModel** klasy.</span><span class="sxs-lookup"><span data-stu-id="6e3e8-427">Create a **ViewModel** class.</span></span> <span data-ttu-id="6e3e8-428">Aby to zrobić, kliknij prawym przyciskiem myszy **ViewModels** folderu niedawno utworzona, wybierz **Dodaj** , a następnie **nowy element**.</span><span class="sxs-lookup"><span data-stu-id="6e3e8-428">To do this, right-click on the **ViewModels** folder recently created, select **Add** and then **New Item**.</span></span> <span data-ttu-id="6e3e8-429">W obszarze **kod**, wybierz **klasy** elementu i nazwij plik *StoreIndexViewModel.cs*, następnie kliknij przycisk **Dodaj**.</span><span class="sxs-lookup"><span data-stu-id="6e3e8-429">Under **Code**, choose the **Class** item and name the file *StoreIndexViewModel.cs*, then click **Add**.</span></span>

    <span data-ttu-id="6e3e8-430">![Dodawanie nowej klasy](aspnet-mvc-4-fundamentals/_static/image19.png "Dodawanie nowej klasy")</span><span class="sxs-lookup"><span data-stu-id="6e3e8-430">![Adding a new Class](aspnet-mvc-4-fundamentals/_static/image19.png "Adding a new Class")</span></span>

    <span data-ttu-id="6e3e8-431">*Dodawanie nowej klasy*</span><span class="sxs-lookup"><span data-stu-id="6e3e8-431">*Adding a new Class*</span></span>

    <span data-ttu-id="6e3e8-432">![Tworzenie klasy StoreIndexViewModel](aspnet-mvc-4-fundamentals/_static/image20.png "StoreIndexViewModel Tworzenie klasy")</span><span class="sxs-lookup"><span data-stu-id="6e3e8-432">![Creating StoreIndexViewModel class](aspnet-mvc-4-fundamentals/_static/image20.png "Creating StoreIndexViewModel class")</span></span>

    <span data-ttu-id="6e3e8-433">*Tworzenie klasy StoreIndexViewModel*</span><span class="sxs-lookup"><span data-stu-id="6e3e8-433">*Creating StoreIndexViewModel class*</span></span>

<a id="Ex5Task2"></a>

<a id="Task_2_-_Adding_Properties_to_the_ViewModel_class"></a>
#### <a name="task-2---adding-properties-to-the-viewmodel-class"></a><span data-ttu-id="6e3e8-434">Zadanie 2 — Dodawanie właściwości do klasy ViewModel</span><span class="sxs-lookup"><span data-stu-id="6e3e8-434">Task 2 - Adding Properties to the ViewModel class</span></span>

<span data-ttu-id="6e3e8-435">Istnieją dwa parametry do przekazania z StoreController do szablonu widoku w celu wygenerowania oczekiwanej odpowiedzi HTML: numer gatunkami muzyki w magazynie i listę tych gatunkami muzyki.</span><span class="sxs-lookup"><span data-stu-id="6e3e8-435">There are two parameters to be passed from the StoreController to the View template in order to generate the expected HTML response: the number of genres in the store and a list of those genres.</span></span>

<span data-ttu-id="6e3e8-436">To zadanie spowoduje dodanie tych właściwości 2 w celu **StoreIndexViewModel** klasy: **NumberOfGenres** (liczba całkowita) i **Genres** (lista ciągów).</span><span class="sxs-lookup"><span data-stu-id="6e3e8-436">In this task, you will add those 2 properties to the **StoreIndexViewModel** class: **NumberOfGenres** (an integer) and **Genres** (a list of strings).</span></span>

1. <span data-ttu-id="6e3e8-437">Dodaj **NumberOfGenres** i **Genres** właściwości **StoreIndexViewModel** klasy.</span><span class="sxs-lookup"><span data-stu-id="6e3e8-437">Add **NumberOfGenres** and **Genres** properties to the **StoreIndexViewModel** class.</span></span> <span data-ttu-id="6e3e8-438">Aby to zrobić, Dodaj następujące wiersze 2 do definicji klasy:</span><span class="sxs-lookup"><span data-stu-id="6e3e8-438">To do this, add the following 2 lines to the class definition:</span></span>

    <span data-ttu-id="6e3e8-439">(Fragment - kodu *ASP.NET MVC 4 podstawy — właściwości Ex5 StoreIndexViewModel*)</span><span class="sxs-lookup"><span data-stu-id="6e3e8-439">(Code Snippet - *ASP.NET MVC 4 Fundamentals - Ex5 StoreIndexViewModel properties*)</span></span>


~~~
[!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample11.cs)]

> [!NOTE]
> The **{ get; set; }** notation makes use of C#'s auto-implemented properties feature. It provides the benefits of a property without requiring us to declare a backing field.
~~~

<a id="Ex5Task3"></a>

<a id="Task_3_-_Updating_StoreController_to_use_the_StoreIndexViewModel"></a>
#### <a name="task-3---updating-storecontroller-to-use-the-storeindexviewmodel"></a><span data-ttu-id="6e3e8-440">Zadanie 3 — aktualizowanie StoreController do użycia StoreIndexViewModel</span><span class="sxs-lookup"><span data-stu-id="6e3e8-440">Task 3 - Updating StoreController to use the StoreIndexViewModel</span></span>

<span data-ttu-id="6e3e8-441">**StoreIndexViewModel** klasa hermetyzuje informacje wymagane do przekazania z **StoreController**w **indeksu** metodę szablonu widoku w celu wygenerowania odpowiedzi .</span><span class="sxs-lookup"><span data-stu-id="6e3e8-441">The **StoreIndexViewModel** class encapsulates the information needed to pass from **StoreController**'s **Index** method to a View template in order to generate a response.</span></span>

<span data-ttu-id="6e3e8-442">To zadanie zaktualizuje **StoreController** do używania **StoreIndexViewModel**.</span><span class="sxs-lookup"><span data-stu-id="6e3e8-442">In this task, you will update the **StoreController** to use the **StoreIndexViewModel**.</span></span>

1. <span data-ttu-id="6e3e8-443">Otwórz **StoreController** klasy.</span><span class="sxs-lookup"><span data-stu-id="6e3e8-443">Open **StoreController** class.</span></span>

    <span data-ttu-id="6e3e8-444">![Otwieranie klasy StoreController](aspnet-mvc-4-fundamentals/_static/image21.png "StoreController otwierania — klasa")</span><span class="sxs-lookup"><span data-stu-id="6e3e8-444">![Opening StoreController class](aspnet-mvc-4-fundamentals/_static/image21.png "Opening StoreController class")</span></span>

    <span data-ttu-id="6e3e8-445">*Otwieranie StoreController klasy*</span><span class="sxs-lookup"><span data-stu-id="6e3e8-445">*Opening StoreController class*</span></span>
2. <span data-ttu-id="6e3e8-446">Aby można było używać **StoreIndexViewModel** klasę z **StoreController**, Dodaj następujący obszar nazw w górnej części **StoreController** kodu:</span><span class="sxs-lookup"><span data-stu-id="6e3e8-446">In order to use the **StoreIndexViewModel** class from the **StoreController**, add the following namespace at the top of the **StoreController** code:</span></span>

    <span data-ttu-id="6e3e8-447">(Fragment - kodu *ASP.NET MVC 4 podstawy — za pomocą ViewModels StoreIndexViewModel Ex5*)</span><span class="sxs-lookup"><span data-stu-id="6e3e8-447">(Code Snippet - *ASP.NET MVC 4 Fundamentals - Ex5 StoreIndexViewModel using ViewModels*)</span></span>


~~~
[!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample12.cs)]
~~~
3. <span data-ttu-id="6e3e8-448">Zmień **StoreController**w **indeksu** metody akcji, którą tworzy i wypełnia **StoreIndexViewModel** obiektu i przekazuje ją do szablonu widoku w celu Generowanie odpowiedzi HTML z nim.</span><span class="sxs-lookup"><span data-stu-id="6e3e8-448">Change the **StoreController**'s **Index** action method so that it creates and populates a **StoreIndexViewModel** object and then passes it off to a View template to generate an HTML response with it.</span></span>

    > [!NOTE]
    > <span data-ttu-id="6e3e8-449">W laboratorium &quot;ASP.NET MVC modeli i dostęp do danych&quot; będzie pisania kodu, który pobiera listę gatunkami muzyki magazynu z bazy danych.</span><span class="sxs-lookup"><span data-stu-id="6e3e8-449">In Lab &quot;ASP.NET MVC Models and Data Access&quot; you will write code that retrieves the list of store genres from a database.</span></span> <span data-ttu-id="6e3e8-450">W poniższym kodzie utworzysz **listy** gatunkami muzyki fikcyjny danych, które wypełnia **StoreIndexViewModel**.</span><span class="sxs-lookup"><span data-stu-id="6e3e8-450">In the following code, you will create a **List** of dummy data genres that will populate the **StoreIndexViewModel**.</span></span>
    > 
    > <span data-ttu-id="6e3e8-451">Po utworzeniu i konfigurowanie **StoreIndexViewModel** obiektu, zostanie przekazany jako argument **widoku** metody.</span><span class="sxs-lookup"><span data-stu-id="6e3e8-451">After creating and setting up the **StoreIndexViewModel** object, it will be passed as an argument to the **View** method.</span></span> <span data-ttu-id="6e3e8-452">Oznacza to, że szablon widoku będzie używać tego obiektu do generowania odpowiedzi HTML z nim.</span><span class="sxs-lookup"><span data-stu-id="6e3e8-452">This indicates that the View template will use that object to generate an HTML response with it.</span></span>
4. <span data-ttu-id="6e3e8-453">Zastąp **indeksu** metodę z następującym kodem:</span><span class="sxs-lookup"><span data-stu-id="6e3e8-453">Replace the **Index** method with the following code:</span></span>

    <span data-ttu-id="6e3e8-454">(Fragment - kodu *ASP.NET MVC 4 podstawy — metoda indeksu StoreController Ex5*)</span><span class="sxs-lookup"><span data-stu-id="6e3e8-454">(Code Snippet - *ASP.NET MVC 4 Fundamentals - Ex5 StoreController Index method*)</span></span>


~~~
[!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample13.cs)]

> [!NOTE]
> If you're unfamiliar with C#, you may assume that using **var** means that the **viewModel** variable is late-bound. That's not correct - the C# compiler is using type-inference based on what you assign to the variable to determine that **viewModel** is of type **StoreIndexViewModel**. Also, by compiling the local **viewModel** variable as a **StoreIndexViewModel** type you get compile-time checking and Visual Studio code-editor support.
~~~

<a id="Ex5Task4"></a>

<a id="Task_4_-_Creating_a_View_Template_that_Uses_StoreIndexViewModel"></a>
#### <a name="task-4---creating-a-view-template-that-uses-storeindexviewmodel"></a><span data-ttu-id="6e3e8-455">Zadanie 4 — Tworzenie szablonu widoku, który używa StoreIndexViewModel</span><span class="sxs-lookup"><span data-stu-id="6e3e8-455">Task 4 - Creating a View Template that Uses StoreIndexViewModel</span></span>

<span data-ttu-id="6e3e8-456">W ramach tego zadania zostanie utworzony szablon widoku, który będzie używany obiekt StoreIndexViewModel przekazany z kontrolera do wyświetlenia listy gatunkami muzyki.</span><span class="sxs-lookup"><span data-stu-id="6e3e8-456">In this task, you will create a View template that will use a StoreIndexViewModel object passed from the Controller to display a list of genres.</span></span>

1. <span data-ttu-id="6e3e8-457">Przed utworzeniem nowego szablonu widoku, utworzymy projektu, aby **dialogowe dodawania widoku** zna **StoreIndexViewModel** klasy.</span><span class="sxs-lookup"><span data-stu-id="6e3e8-457">Before creating the new View template, let's build the project so that the **Add View Dialog** knows about the **StoreIndexViewModel** class.</span></span> <span data-ttu-id="6e3e8-458">Skompiluj projekt, wybierając **kompilacji** element menu, a następnie **kompilacji MvcMusicStore**.</span><span class="sxs-lookup"><span data-stu-id="6e3e8-458">Build the project by selecting the **Build** menu item and then **Build MvcMusicStore**.</span></span>

    <span data-ttu-id="6e3e8-459">![Tworzenie projektu](aspnet-mvc-4-fundamentals/_static/image22.png "skompilowanie projektu")</span><span class="sxs-lookup"><span data-stu-id="6e3e8-459">![Building the project](aspnet-mvc-4-fundamentals/_static/image22.png "Building the project")</span></span>

    <span data-ttu-id="6e3e8-460">*Tworzenie projektu*</span><span class="sxs-lookup"><span data-stu-id="6e3e8-460">*Building the project*</span></span>
2. <span data-ttu-id="6e3e8-461">Utwórz nowy szablon widoku.</span><span class="sxs-lookup"><span data-stu-id="6e3e8-461">Create a new View template.</span></span> <span data-ttu-id="6e3e8-462">W tym celu kliknij prawym przyciskiem myszy wewnątrz **indeksu** metodę i wybierz **Dodaj widok**.</span><span class="sxs-lookup"><span data-stu-id="6e3e8-462">To do that, right-click inside the **Index** method and select **Add View**.</span></span>

    <span data-ttu-id="6e3e8-463">![Dodawanie widoku](aspnet-mvc-4-fundamentals/_static/image23.png "dodawania widoku")</span><span class="sxs-lookup"><span data-stu-id="6e3e8-463">![Adding a View](aspnet-mvc-4-fundamentals/_static/image23.png "Adding a View")</span></span>

    <span data-ttu-id="6e3e8-464">*Dodawanie widoku*</span><span class="sxs-lookup"><span data-stu-id="6e3e8-464">*Adding a View*</span></span>
3. <span data-ttu-id="6e3e8-465">Ponieważ **dialogowe dodawania widoku** została wywołana z **StoreController**, spowoduje to dodanie Wyświetl szablon domyślny **\Views\Store\Index.cshtml** pliku.</span><span class="sxs-lookup"><span data-stu-id="6e3e8-465">Because the **Add View Dialog** was invoked from the **StoreController**, it will add the View template by default in a **\Views\Store\Index.cshtml** file.</span></span> <span data-ttu-id="6e3e8-466">Sprawdź **utworzyć silnie wpisane — widok** pole wyboru, a następnie wybierz **StoreIndexViewModel** jako **klasa modelu**.</span><span class="sxs-lookup"><span data-stu-id="6e3e8-466">Check the **Create a strongly-typed-view** checkbox and then select **StoreIndexViewModel** as the **Model class**.</span></span> <span data-ttu-id="6e3e8-467">Ponadto upewnij się, że wybrany aparat widoku jest **Razor**.</span><span class="sxs-lookup"><span data-stu-id="6e3e8-467">Also, make sure that the View engine selected is **Razor**.</span></span> <span data-ttu-id="6e3e8-468">Kliknij przycisk **Dodaj**.</span><span class="sxs-lookup"><span data-stu-id="6e3e8-468">Click **Add**.</span></span>

    <span data-ttu-id="6e3e8-469">![Okno dialogowe dodawania widoku](aspnet-mvc-4-fundamentals/_static/image24.png "okno dialogowe dodawania widoku")</span><span class="sxs-lookup"><span data-stu-id="6e3e8-469">![Add View Dialog](aspnet-mvc-4-fundamentals/_static/image24.png "Add View Dialog")</span></span>

    <span data-ttu-id="6e3e8-470">*Okno dialogowe dodawania widoku*</span><span class="sxs-lookup"><span data-stu-id="6e3e8-470">*Add View Dialog*</span></span>

    <span data-ttu-id="6e3e8-471">**\Views\Store\Index.cshtml** widoku pliku szablonu jest utworzony i otwarty.</span><span class="sxs-lookup"><span data-stu-id="6e3e8-471">The **\Views\Store\Index.cshtml** View template file is created and opened.</span></span> <span data-ttu-id="6e3e8-472">Na podstawie informacji do **Dodaj widok** okna dialogowego w ostatnim kroku, widok szablonu będzie oczekiwać **StoreIndexViewModel** wystąpienia jako danych używany do generowania odpowiedzi HTML.</span><span class="sxs-lookup"><span data-stu-id="6e3e8-472">Based on the information provided to the **Add View** dialog in the last step, the View template will expect a **StoreIndexViewModel** instance as the data to use to generate an HTML response.</span></span> <span data-ttu-id="6e3e8-473">Można zauważyć, że szablon dziedziczy `ViewPage<musicstore.viewmodels.storeindexviewmodel>` w języku C#.</span><span class="sxs-lookup"><span data-stu-id="6e3e8-473">You will notice that the template inherits a `ViewPage<musicstore.viewmodels.storeindexviewmodel>` in C#.</span></span>

<a id="Ex5Task5"></a>

<a id="Task_5_-_Updating_the_View_Template"></a>
#### <a name="task-5---updating-the-view-template"></a><span data-ttu-id="6e3e8-474">Zadanie 5 aktualizowanie szablonu widoku.</span><span class="sxs-lookup"><span data-stu-id="6e3e8-474">Task 5 - Updating the View Template</span></span>

<span data-ttu-id="6e3e8-475">To zadanie zaktualizuje Wyświetl szablon utworzony w ostatnim zadaniem, aby pobrać liczbę genres i ich nazwy na stronie.</span><span class="sxs-lookup"><span data-stu-id="6e3e8-475">In this task, you will update the View template created in the last task to retrieve the number of genres and their names within the page.</span></span>

> [!NOTE]
> <span data-ttu-id="6e3e8-476">Użyjesz @ składni (często określany jako &quot;kodu nuggets&quot;) do wykonywania kodu w szablonie widoku.</span><span class="sxs-lookup"><span data-stu-id="6e3e8-476">You will use @ syntax (often referred to as &quot;code nuggets&quot;) to execute code within the View template.</span></span>


1. <span data-ttu-id="6e3e8-477">W **Index.cshtml** pliku poziomu **magazynu** folderu, zastąp jego kod następującym kodem:</span><span class="sxs-lookup"><span data-stu-id="6e3e8-477">In the **Index.cshtml** file, within the **Store** folder, replace its code with the following:</span></span>


~~~
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
~~~
2. <span data-ttu-id="6e3e8-478">Pętla za pośrednictwem listy genre w **StoreIndexViewModel** i tworzenia kodu HTML **&lt;ul&gt;** listy przy użyciu **foreach** pętli.</span><span class="sxs-lookup"><span data-stu-id="6e3e8-478">Loop over the genre list in **StoreIndexViewModel** and create an HTML **&lt;ul&gt;** list using a **foreach** loop.</span></span>
   <span data-ttu-id="6e3e8-479">(C#)</span><span class="sxs-lookup"><span data-stu-id="6e3e8-479">(C#)</span></span>

    [!code-cshtml[Main](aspnet-mvc-4-fundamentals/samples/sample15.cshtml)]
3. <span data-ttu-id="6e3e8-480">Naciśnij klawisz **F5** uruchomić aplikację, a następnie przejdź **/magazynu**.</span><span class="sxs-lookup"><span data-stu-id="6e3e8-480">Press **F5** to run the Application and browse **/Store**.</span></span> <span data-ttu-id="6e3e8-481">Zostanie wyświetlona lista gatunkami muzyki przekazano **StoreIndexViewModel** obiekt z **StoreController** do szablonu widoku.</span><span class="sxs-lookup"><span data-stu-id="6e3e8-481">You will see the list of genres passed in the **StoreIndexViewModel** object from the **StoreController** to the View template.</span></span>

    <span data-ttu-id="6e3e8-482">![Wyświetlanie listy gatunkami muzyki widoku](aspnet-mvc-4-fundamentals/_static/image26.png "widoku wyświetlanie listy gatunkami muzyki")</span><span class="sxs-lookup"><span data-stu-id="6e3e8-482">![View displaying a list of genres](aspnet-mvc-4-fundamentals/_static/image26.png "View displaying a list of genres")</span></span>

    <span data-ttu-id="6e3e8-483">*Wyświetlanie listy gatunkami muzyki widoku*</span><span class="sxs-lookup"><span data-stu-id="6e3e8-483">*View displaying a list of genres*</span></span>
4. <span data-ttu-id="6e3e8-484">Zamknij przeglądarkę.</span><span class="sxs-lookup"><span data-stu-id="6e3e8-484">Close the browser.</span></span>

<a id="Exercise6"></a>

<a id="Exercise_6_Using_Parameters_in_View"></a>
### <a name="exercise-6-using-parameters-in-view"></a><span data-ttu-id="6e3e8-485">Ćwiczenie 6: W widoku przy użyciu parametrów</span><span class="sxs-lookup"><span data-stu-id="6e3e8-485">Exercise 6: Using Parameters in View</span></span>

<span data-ttu-id="6e3e8-486">Ćwiczenie 3 przedstawiono sposób przekazania parametrów do kontrolera.</span><span class="sxs-lookup"><span data-stu-id="6e3e8-486">In Exercise 3 you learned how to pass parameters to the Controller.</span></span> <span data-ttu-id="6e3e8-487">W tym ćwiczeniu dowiesz się, jak używać tych parametrów w szablonie widoku.</span><span class="sxs-lookup"><span data-stu-id="6e3e8-487">In this exercise, you will learn how to use those parameters in the View template.</span></span> <span data-ttu-id="6e3e8-488">W tym celu należy zostaną wprowadzone najpierw do klasy modeli, które ułatwiają zarządzanie logika danych i domeny.</span><span class="sxs-lookup"><span data-stu-id="6e3e8-488">For that purpose, you will be introduced first to Model classes that will help you to manage your data and domain logic.</span></span> <span data-ttu-id="6e3e8-489">Ponadto dowiesz sposobu tworzenia łączy do stron w aplikacji ASP.NET MVC bez obaw rzeczy, takich jak kodowanie ścieżki adresu URL.</span><span class="sxs-lookup"><span data-stu-id="6e3e8-489">Additionally, you will learn how to create links to pages inside the ASP.NET MVC application without worrying of things like URL paths encoding.</span></span>

<a id="Ex6Task1"></a>

<a id="Task_1_-_Adding_Model_Classes"></a>
#### <a name="task-1---adding-model-classes"></a><span data-ttu-id="6e3e8-490">Zadanie 1 — Dodawanie klasy modelu</span><span class="sxs-lookup"><span data-stu-id="6e3e8-490">Task 1 - Adding Model Classes</span></span>

<span data-ttu-id="6e3e8-491">W odróżnieniu od ViewModels, które są tworzone tylko w celu przekazywania informacji z kontrolera do widoku, klasy modeli są tworzone zawierają i zarządzanie nimi logiki danych i domeny.</span><span class="sxs-lookup"><span data-stu-id="6e3e8-491">Unlike ViewModels, which are created just to pass information from the Controller to the View, Model classes are built to contain and manage data and domain logic.</span></span> <span data-ttu-id="6e3e8-492">W ramach tego zadania zostaną dodane dwie klasy modelu do reprezentowania tych pojęć: **Genre** i **albumu**.</span><span class="sxs-lookup"><span data-stu-id="6e3e8-492">In this task you will add two model classes to represent these concepts: **Genre** and **Album**.</span></span>

1. <span data-ttu-id="6e3e8-493">Jeśli nie już otwarty, uruchom **VS Express for Web**</span><span class="sxs-lookup"><span data-stu-id="6e3e8-493">If not already open, start **VS Express for Web**</span></span>
2. <span data-ttu-id="6e3e8-494">W **pliku** menu, wybierz **Otwórz projekt**.</span><span class="sxs-lookup"><span data-stu-id="6e3e8-494">In the **File** menu, choose **Open Project**.</span></span> <span data-ttu-id="6e3e8-495">W oknie dialogowym Otwórz projekt, przejdź do **Source\Ex06 UsingParametersInView\Begin**, wybierz pozycję **Begin.sln** i kliknij przycisk **Otwórz**.</span><span class="sxs-lookup"><span data-stu-id="6e3e8-495">In the Open Project dialog, browse to **Source\Ex06-UsingParametersInView\Begin**, select **Begin.sln** and click **Open**.</span></span> <span data-ttu-id="6e3e8-496">Alternatywnie możesz nadal z rozwiązaniem uzyskany po zakończeniu pracy w poprzednim ćwiczeniu.</span><span class="sxs-lookup"><span data-stu-id="6e3e8-496">Alternatively, you may continue with the solution that you obtained after completing the previous exercise.</span></span>

   1. <span data-ttu-id="6e3e8-497">Po otwarciu dostarczonych **rozpocząć** rozwiązania, musisz pobrać niektórych brakujących pakietów NuGet aby kontynuować.</span><span class="sxs-lookup"><span data-stu-id="6e3e8-497">If you opened the provided **Begin** solution, you will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="6e3e8-498">Aby to zrobić, kliknij przycisk **projektu** menu i wybierz **Zarządzaj pakietami NuGet**.</span><span class="sxs-lookup"><span data-stu-id="6e3e8-498">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
   2. <span data-ttu-id="6e3e8-499">W **Zarządzaj pakietami NuGet** okna dialogowego, kliknij przycisk **przywrócić** celu pobieranie brakujących pakietów.</span><span class="sxs-lookup"><span data-stu-id="6e3e8-499">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
   3. <span data-ttu-id="6e3e8-500">Na koniec Skompiluj rozwiązanie, klikając **kompilacji** | **Kompiluj rozwiązanie**.</span><span class="sxs-lookup"><span data-stu-id="6e3e8-500">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

      > [!NOTE]
      > <span data-ttu-id="6e3e8-501">Jedną z zalet przy użyciu narzędzia NuGet jest, że nie masz do wysłania wszystkich bibliotek w projekcie, zmniejszenie jego rozmiar projektu.</span><span class="sxs-lookup"><span data-stu-id="6e3e8-501">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="6e3e8-502">Narzędzia Power NuGet określając wersje pakietów w pliku Packages.config, będzie można pobrać wymaganych bibliotek podczas pierwszego uruchomienia projektu.</span><span class="sxs-lookup"><span data-stu-id="6e3e8-502">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="6e3e8-503">Jest to, dlaczego konieczne będzie wykonanie tych kroków, po otwarciu istniejącego rozwiązania z tego laboratorium.</span><span class="sxs-lookup"><span data-stu-id="6e3e8-503">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
3. <span data-ttu-id="6e3e8-504">Dodaj **Genre** klasa modelu.</span><span class="sxs-lookup"><span data-stu-id="6e3e8-504">Add a **Genre** Model class.</span></span> <span data-ttu-id="6e3e8-505">Aby to zrobić, kliknij prawym przyciskiem myszy **modele** folderu w **Eksploratora rozwiązań**, wybierz pozycję **Dodaj** , a następnie **nowy element** opcji.</span><span class="sxs-lookup"><span data-stu-id="6e3e8-505">To do this, right-click the **Models** folder in the **Solution Explorer**, select **Add** and then the **New Item** option.</span></span> <span data-ttu-id="6e3e8-506">W obszarze **kod**, wybierz **klasy** elementu i nazwij plik *Genre.cs*, następnie kliknij przycisk **Dodaj**.</span><span class="sxs-lookup"><span data-stu-id="6e3e8-506">Under **Code**, choose the **Class** item and name the file *Genre.cs*, then click **Add**.</span></span>

    <span data-ttu-id="6e3e8-507">![Dodawanie klasy](aspnet-mvc-4-fundamentals/_static/image27.png "Dodawanie klasy")</span><span class="sxs-lookup"><span data-stu-id="6e3e8-507">![Adding a class](aspnet-mvc-4-fundamentals/_static/image27.png "Adding a class")</span></span>

    <span data-ttu-id="6e3e8-508">*Dodawanie nowego elementu*</span><span class="sxs-lookup"><span data-stu-id="6e3e8-508">*Adding a new item*</span></span>

    <span data-ttu-id="6e3e8-509">![Dodaj klasę modelu Genre](aspnet-mvc-4-fundamentals/_static/image28.png "Dodaj klasę modelu Genre")</span><span class="sxs-lookup"><span data-stu-id="6e3e8-509">![Add Genre Model Class](aspnet-mvc-4-fundamentals/_static/image28.png "Add Genre Model Class")</span></span>

    <span data-ttu-id="6e3e8-510">*Dodaj klasę modelu Genre*</span><span class="sxs-lookup"><span data-stu-id="6e3e8-510">*Add Genre Model Class*</span></span>
4. <span data-ttu-id="6e3e8-511">Dodaj **nazwa** właściwości do klasy Genre.</span><span class="sxs-lookup"><span data-stu-id="6e3e8-511">Add a **Name** property to the Genre class.</span></span> <span data-ttu-id="6e3e8-512">Aby to zrobić, Dodaj następujący kod:</span><span class="sxs-lookup"><span data-stu-id="6e3e8-512">To do this, add the following code:</span></span>

    <span data-ttu-id="6e3e8-513">(Fragment - kodu *podstawy platformy ASP.NET MVC 4 - Ex6 Genre*)</span><span class="sxs-lookup"><span data-stu-id="6e3e8-513">(Code Snippet - *ASP.NET MVC 4 Fundamentals - Ex6 Genre*)</span></span>


~~~
[!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample16.cs)]
~~~
5. <span data-ttu-id="6e3e8-514">Procedury jak wcześniej, Dodaj **albumu** klasy.</span><span class="sxs-lookup"><span data-stu-id="6e3e8-514">Following the same procedure as before, add an **Album** class.</span></span> <span data-ttu-id="6e3e8-515">Aby to zrobić, kliknij prawym przyciskiem myszy **modele** folderu w **Eksploratora rozwiązań**, wybierz pozycję **Dodaj** , a następnie **nowy element** opcji.</span><span class="sxs-lookup"><span data-stu-id="6e3e8-515">To do this, right-click the **Models** folder in the **Solution Explorer**, select **Add** and then the **New Item** option.</span></span> <span data-ttu-id="6e3e8-516">W obszarze **kod**, wybierz **klasy** elementu i nazwij plik *Album.cs*, następnie kliknij przycisk **Dodaj**.</span><span class="sxs-lookup"><span data-stu-id="6e3e8-516">Under **Code**, choose the **Class** item and name the file *Album.cs*, then click **Add**.</span></span>
6. <span data-ttu-id="6e3e8-517">Dodaj dwie właściwości do klasy albumu: **Genre** i **tytuł**.</span><span class="sxs-lookup"><span data-stu-id="6e3e8-517">Add two properties to the Album class: **Genre** and **Title**.</span></span> <span data-ttu-id="6e3e8-518">Aby to zrobić, Dodaj następujący kod:</span><span class="sxs-lookup"><span data-stu-id="6e3e8-518">To do this, add the following code:</span></span>

    <span data-ttu-id="6e3e8-519">(Fragment - kodu *podstawy platformy ASP.NET MVC 4 - albumu Ex6*)</span><span class="sxs-lookup"><span data-stu-id="6e3e8-519">(Code Snippet - *ASP.NET MVC 4 Fundamentals - Ex6 Album*)</span></span>


~~~
[!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample17.cs)]
~~~

<a id="Ex6Task2"></a>

<a id="Task_2_-_Adding_a_StoreBrowseViewModel"></a>
#### <a name="task-2---adding-a-storebrowseviewmodel"></a><span data-ttu-id="6e3e8-520">Zadanie 2 — Dodawanie StoreBrowseViewModel</span><span class="sxs-lookup"><span data-stu-id="6e3e8-520">Task 2 - Adding a StoreBrowseViewModel</span></span>

<span data-ttu-id="6e3e8-521">A **StoreBrowseViewModel** będzie używany w tym zadaniu pokazanie albumy, zgodne wybranego rodzaju.</span><span class="sxs-lookup"><span data-stu-id="6e3e8-521">A **StoreBrowseViewModel** will be used in this task to show the Albums that match a selected Genre.</span></span> <span data-ttu-id="6e3e8-522">To zadanie będzie utworzyć tę klasę, a następnie dodaj dwie właściwości do obsługi **Genre** i jego **albumu**na liście.</span><span class="sxs-lookup"><span data-stu-id="6e3e8-522">In this task, you will create this class and then add two properties to handle the **Genre** and its **Album**'s List.</span></span>

1. <span data-ttu-id="6e3e8-523">Dodaj **StoreBrowseViewModel** klasy.</span><span class="sxs-lookup"><span data-stu-id="6e3e8-523">Add a **StoreBrowseViewModel** class.</span></span> <span data-ttu-id="6e3e8-524">Aby to zrobić, kliknij prawym przyciskiem myszy **ViewModels** folderu w **Eksploratora rozwiązań**, wybierz pozycję **Dodaj** , a następnie **nowy element** opcji.</span><span class="sxs-lookup"><span data-stu-id="6e3e8-524">To do this, right-click the **ViewModels** folder in the **Solution Explorer**, select **Add** and then the **New Item** option.</span></span> <span data-ttu-id="6e3e8-525">W obszarze **kod**, wybierz **klasy** elementu i nazwij plik *StoreBrowseViewModel.cs*, następnie kliknij przycisk **Dodaj**.</span><span class="sxs-lookup"><span data-stu-id="6e3e8-525">Under **Code**, choose the **Class** item and name the file *StoreBrowseViewModel.cs*, then click **Add**.</span></span>
2. <span data-ttu-id="6e3e8-526">Dodaj odwołanie do modeli w **StoreBrowseViewModel** klasy.</span><span class="sxs-lookup"><span data-stu-id="6e3e8-526">Add a reference to the Models in **StoreBrowseViewModel** class.</span></span> <span data-ttu-id="6e3e8-527">Aby to zrobić, Dodaj następujący za pomocą przestrzeni nazw:</span><span class="sxs-lookup"><span data-stu-id="6e3e8-527">To do this, add the following using namespace:</span></span>

    <span data-ttu-id="6e3e8-528">(Fragment - kodu *podstawy platformy ASP.NET MVC 4 - Ex6 UsingModel*)</span><span class="sxs-lookup"><span data-stu-id="6e3e8-528">(Code Snippet - *ASP.NET MVC 4 Fundamentals - Ex6 UsingModel*)</span></span>


~~~
[!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample18.cs)]
~~~
3. <span data-ttu-id="6e3e8-529">Dodaj dwie właściwości do **StoreBrowseViewModel** klasy: **Genre** i **albumów**.</span><span class="sxs-lookup"><span data-stu-id="6e3e8-529">Add two properties to **StoreBrowseViewModel** class: **Genre** and **Albums**.</span></span> <span data-ttu-id="6e3e8-530">Aby to zrobić, Dodaj następujący kod:</span><span class="sxs-lookup"><span data-stu-id="6e3e8-530">To do this, add the following code:</span></span>

    <span data-ttu-id="6e3e8-531">(Fragment - kodu *podstawy platformy ASP.NET MVC 4 - Ex6 ModelProperties*)</span><span class="sxs-lookup"><span data-stu-id="6e3e8-531">(Code Snippet - *ASP.NET MVC 4 Fundamentals - Ex6 ModelProperties*)</span></span>


~~~
[!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample19.cs)]

> [!NOTE]
> What is **List&lt;Album&gt;** ?: This definition is using the **List&lt;T&gt;** type, where **T** constrains the type to which elements of this **List** belong to, in this case **Album** (or any of its descendants).
> 
> This ability to design classes and methods that defer the specification of one or more types until the class or method is declared and instantiated by client code is a feature of the C# language called **Generics**.
> 
> **List&lt;T&gt;** is the generic equivalent of the **ArrayList** type and is available in the **System.Collections.Generic** namespace. One of the benefits of using **generics** is that since the type is specified, you do not need to take care of type checking operations such as casting the elements into **Album** as you would do with an **ArrayList**.
~~~

<a id="Ex6Task3"></a>

<a id="Task_3_-_Using_the_New_ViewModel_in_the_StoreController"></a>
#### <a name="task-3---using-the-new-viewmodel-in-the-storecontroller"></a><span data-ttu-id="6e3e8-532">Zadanie 3 - w StoreController przy użyciu nowego ViewModel</span><span class="sxs-lookup"><span data-stu-id="6e3e8-532">Task 3 - Using the New ViewModel in the StoreController</span></span>

<span data-ttu-id="6e3e8-533">To zadanie, należy zmodyfikować **StoreController**w **Przeglądaj** i **szczegóły** metod akcji do używania nowych **StoreBrowseViewModel** .</span><span class="sxs-lookup"><span data-stu-id="6e3e8-533">In this task, you will modify the **StoreController**'s **Browse** and **Details** action methods to use the new **StoreBrowseViewModel**.</span></span>

1. <span data-ttu-id="6e3e8-534">Dodaj odwołanie do folderu modeli w **StoreController** klasy.</span><span class="sxs-lookup"><span data-stu-id="6e3e8-534">Add a reference to the Models folder in **StoreController** class.</span></span> <span data-ttu-id="6e3e8-535">Aby to zrobić, rozwiń węzeł **kontrolerów** folderu w **Eksploratora rozwiązań** , a następnie otwórz **StoreController** klasy.</span><span class="sxs-lookup"><span data-stu-id="6e3e8-535">To do this, expand the **Controllers** folder in the **Solution Explorer** and open the **StoreController** class.</span></span> <span data-ttu-id="6e3e8-536">Następnie dodaj następujący kod:</span><span class="sxs-lookup"><span data-stu-id="6e3e8-536">Then add the following code:</span></span>

    <span data-ttu-id="6e3e8-537">(Fragment - kodu *podstawy platformy ASP.NET MVC 4 - Ex6 UsingModelInController*)</span><span class="sxs-lookup"><span data-stu-id="6e3e8-537">(Code Snippet - *ASP.NET MVC 4 Fundamentals - Ex6 UsingModelInController*)</span></span>


~~~
[!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample20.cs)]
~~~
2. <span data-ttu-id="6e3e8-538">Zastąp **Przeglądaj** metody akcji, aby użyć **StoreViewBrowseController** klasy.</span><span class="sxs-lookup"><span data-stu-id="6e3e8-538">Replace the **Browse** action method to use the **StoreViewBrowseController** class.</span></span> <span data-ttu-id="6e3e8-539">Utworzysz określonego rodzaju i dwa nowe obiekty albumów fikcyjny danych (w następnej laboratorium Hands-on będą korzystać prawdziwe dane z bazy danych).</span><span class="sxs-lookup"><span data-stu-id="6e3e8-539">You will create a Genre and two new Albums objects with dummy data (in the next Hands-on Lab you will consume real data from a database).</span></span> <span data-ttu-id="6e3e8-540">Aby to zrobić, Zamień **Przeglądaj** metodę z następującym kodem:</span><span class="sxs-lookup"><span data-stu-id="6e3e8-540">To do this, replace the **Browse** method with the following code:</span></span>

    <span data-ttu-id="6e3e8-541">(Fragment - kodu *podstawy platformy ASP.NET MVC 4 - Ex6 BrowseMethod*)</span><span class="sxs-lookup"><span data-stu-id="6e3e8-541">(Code Snippet - *ASP.NET MVC 4 Fundamentals - Ex6 BrowseMethod*)</span></span>


~~~
[!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample21.cs)]
~~~
3. <span data-ttu-id="6e3e8-542">Zastąp **szczegóły** metody akcji, aby użyć **StoreViewBrowseController** klasy.</span><span class="sxs-lookup"><span data-stu-id="6e3e8-542">Replace the **Details** action method to use the **StoreViewBrowseController** class.</span></span> <span data-ttu-id="6e3e8-543">Utworzy nowy **albumu** obiekt ma zostać zwrócona do **widoku**.</span><span class="sxs-lookup"><span data-stu-id="6e3e8-543">You will create a new **Album** object to be returned to the **View**.</span></span> <span data-ttu-id="6e3e8-544">Aby to zrobić, Zamień **szczegóły** metodę z następującym kodem:</span><span class="sxs-lookup"><span data-stu-id="6e3e8-544">To do this, replace the **Details** method with the following code:</span></span>

    <span data-ttu-id="6e3e8-545">(Fragment - kodu *podstawy platformy ASP.NET MVC 4 - Ex6 DetailsMethod*)</span><span class="sxs-lookup"><span data-stu-id="6e3e8-545">(Code Snippet - *ASP.NET MVC 4 Fundamentals - Ex6 DetailsMethod*)</span></span>


~~~
[!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample22.cs)]
~~~

<a id="Ex6Task4"></a>

<a id="Task_4_-_Adding_a_Browse_View_Template"></a>
#### <a name="task-4---adding-a-browse-view-template"></a><span data-ttu-id="6e3e8-546">Zadanie 4 — Dodawanie szablonu widoku przeglądania</span><span class="sxs-lookup"><span data-stu-id="6e3e8-546">Task 4 - Adding a Browse View Template</span></span>

<span data-ttu-id="6e3e8-547">To zadanie spowoduje dodanie **Przeglądaj** widok, aby pokazać albumów znaleziono dla określonego rodzaju.</span><span class="sxs-lookup"><span data-stu-id="6e3e8-547">In this task, you will add a **Browse** View to show the Albums found for a specific Genre.</span></span>

1. <span data-ttu-id="6e3e8-548">Przed utworzeniem nowego szablonu widoku, należy skompilować projekt, aby **Dodaj widok** okna dialogowego zna **ViewModel** klasy do użycia.</span><span class="sxs-lookup"><span data-stu-id="6e3e8-548">Before creating the new View template, you should build the project so that the **Add View** Dialog knows about the **ViewModel** class to use.</span></span> <span data-ttu-id="6e3e8-549">Skompiluj projekt, wybierając **kompilacji** element menu, a następnie **kompilacji MvcMusicStore**.</span><span class="sxs-lookup"><span data-stu-id="6e3e8-549">Build the project by selecting the **Build** menu item and then **Build MvcMusicStore**.</span></span>
2. <span data-ttu-id="6e3e8-550">Dodaj **Przeglądaj** widoku.</span><span class="sxs-lookup"><span data-stu-id="6e3e8-550">Add a **Browse** View.</span></span> <span data-ttu-id="6e3e8-551">Aby to zrobić, kliknij prawym przyciskiem myszy w **Przeglądaj** metody akcji **StoreController** i kliknij przycisk **Dodaj widok**.</span><span class="sxs-lookup"><span data-stu-id="6e3e8-551">To do this, right-click in the **Browse** action method of the **StoreController** and click **Add View**.</span></span>
3. <span data-ttu-id="6e3e8-552">W **Dodaj widok** okna dialogowego upewnij się, że nazwa widoku jest **Przeglądaj**.</span><span class="sxs-lookup"><span data-stu-id="6e3e8-552">In the **Add View** dialog box, verify that the View Name is **Browse**.</span></span> <span data-ttu-id="6e3e8-553">Sprawdź **utworzyć widok jednoznacznie** wyboru i wybierz **StoreBrowseViewModel** z **klasa modelu** listy rozwijanej.</span><span class="sxs-lookup"><span data-stu-id="6e3e8-553">Check the **Create a strongly-typed view** checkbox and select **StoreBrowseViewModel** from the **Model class** dropdown.</span></span> <span data-ttu-id="6e3e8-554">Pozostaw wartości domyślne innych pól.</span><span class="sxs-lookup"><span data-stu-id="6e3e8-554">Leave the other fields with their default value.</span></span> <span data-ttu-id="6e3e8-555">Następnie kliknij przycisk **Dodaj**.</span><span class="sxs-lookup"><span data-stu-id="6e3e8-555">Then click **Add**.</span></span>

    <span data-ttu-id="6e3e8-556">![Dodawanie widoku przeglądania](aspnet-mvc-4-fundamentals/_static/image29.png "Dodawanie widoku przeglądania")</span><span class="sxs-lookup"><span data-stu-id="6e3e8-556">![Adding a Browse View](aspnet-mvc-4-fundamentals/_static/image29.png "Adding a Browse View")</span></span>

    <span data-ttu-id="6e3e8-557">*Dodawanie widoku przeglądania*</span><span class="sxs-lookup"><span data-stu-id="6e3e8-557">*Adding a Browse View*</span></span>
4. <span data-ttu-id="6e3e8-558">Modyfikowanie **Browse.cshtml** do wyświetlania informacji Genre, uzyskiwanie dostępu do **StoreBrowseViewModel** obiekt, który jest przekazywany do szablonu widoku.</span><span class="sxs-lookup"><span data-stu-id="6e3e8-558">Modify the **Browse.cshtml** to display the Genre's information, accessing the **StoreBrowseViewModel** object that is passed to the view template.</span></span> <span data-ttu-id="6e3e8-559">Aby to zrobić, Zastąp zawartość następującym kodem: (C#)</span><span class="sxs-lookup"><span data-stu-id="6e3e8-559">To do this, replace the content with the following: (C#)</span></span>

    [!code-cshtml[Main](aspnet-mvc-4-fundamentals/samples/sample23.cshtml)]

<a id="Ex6Task5"></a>

<a id="Task_5_-_Running_the_Application"></a>
#### <a name="task-5---running-the-application"></a><span data-ttu-id="6e3e8-560">Zadanie 5 działania aplikacji.</span><span class="sxs-lookup"><span data-stu-id="6e3e8-560">Task 5 - Running the Application</span></span>

<span data-ttu-id="6e3e8-561">W tym zadaniu zostanie sprawdzić, czy **Przeglądaj** metoda pobiera albumów z **Przeglądaj** metody akcji.</span><span class="sxs-lookup"><span data-stu-id="6e3e8-561">In this task, you will test that the **Browse** method retrieves Albums from the **Browse** method action.</span></span>

1. <span data-ttu-id="6e3e8-562">Naciśnij klawisz **F5** do uruchomienia aplikacji.</span><span class="sxs-lookup"><span data-stu-id="6e3e8-562">Press **F5** to run the Application.</span></span>
2. <span data-ttu-id="6e3e8-563">Na stronie głównej datę rozpoczęcia projektu.</span><span class="sxs-lookup"><span data-stu-id="6e3e8-563">The project starts in the Home page.</span></span> <span data-ttu-id="6e3e8-564">Zmień adres URL do   **/przechowywania/Przeglądaj? Genre = Disco** Aby sprawdzić, czy akcji zwraca dwa albumów.</span><span class="sxs-lookup"><span data-stu-id="6e3e8-564">Change the URL to **/Store/Browse?Genre=Disco** to verify that the action returns two Albums.</span></span>

    <span data-ttu-id="6e3e8-565">![Przeglądanie magazynu albumów Disco](aspnet-mvc-4-fundamentals/_static/image30.png "przeglądanie albumów Disco magazynu")</span><span class="sxs-lookup"><span data-stu-id="6e3e8-565">![Browsing Store Disco Albums](aspnet-mvc-4-fundamentals/_static/image30.png "Browsing Store Disco Albums")</span></span>

    <span data-ttu-id="6e3e8-566">*Przeglądanie albumów Disco magazynu*</span><span class="sxs-lookup"><span data-stu-id="6e3e8-566">*Browsing Store Disco Albums*</span></span>

<a id="Ex6Task6"></a>

<a id="Task_6_-_Displaying_information_About_a_Specific_Album"></a>
#### <a name="task-6---displaying-information-about-a-specific-album"></a><span data-ttu-id="6e3e8-567">Zadanie 6 — wyświetlanie informacji o określonych albumu</span><span class="sxs-lookup"><span data-stu-id="6e3e8-567">Task 6 - Displaying information About a Specific Album</span></span>

<span data-ttu-id="6e3e8-568">W tym zadaniu zostaną zaimplementowane **szczegóły dotyczące i magazynu** widok, aby wyświetlić informacje o określonym albumu.</span><span class="sxs-lookup"><span data-stu-id="6e3e8-568">In this task, you will implement the **Store/Details** view to display information about a specific album.</span></span> <span data-ttu-id="6e3e8-569">W tym laboratorium Hands-on wszystko, co spowoduje wyświetlenie o album znajduje się już w **widoku** szablonu.</span><span class="sxs-lookup"><span data-stu-id="6e3e8-569">In this Hands-on Lab, everything you will display about the album is already contained in the **View** template.</span></span> <span data-ttu-id="6e3e8-570">Tak, zamiast tworzyć **StoreDetailsViewModel** klasy, użyje bieżącego **StoreBrowseViewModel** szablonu przekazywanie Album do niego.</span><span class="sxs-lookup"><span data-stu-id="6e3e8-570">So, instead of creating a **StoreDetailsViewModel** class, you will use the current **StoreBrowseViewModel** template passing the Album to it.</span></span>

1. <span data-ttu-id="6e3e8-571">Zamknij przeglądarkę, jeśli to konieczne, aby powrócić do okna programu Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="6e3e8-571">Close the browser if needed, to return to the Visual Studio window.</span></span> <span data-ttu-id="6e3e8-572">Dodaj nową **szczegóły** wyświetlić **StoreController**w **szczegóły** metody akcji.</span><span class="sxs-lookup"><span data-stu-id="6e3e8-572">Add a new **Details** view for the **StoreController**'s **Details** action method.</span></span> <span data-ttu-id="6e3e8-573">Aby to zrobić, kliknij prawym przyciskiem myszy **szczegóły** metody w **StoreController** klasy, a następnie kliknij przycisk **Dodaj widok**.</span><span class="sxs-lookup"><span data-stu-id="6e3e8-573">To do this, right-click the **Details** method in the **StoreController** class and click **Add View**.</span></span>
2. <span data-ttu-id="6e3e8-574">W **Dodaj widok** okna dialogowego, upewnij się, że **nazwy widoku** jest **szczegóły**.</span><span class="sxs-lookup"><span data-stu-id="6e3e8-574">In the **Add View** dialog, verify that the **View Name** is **Details**.</span></span> <span data-ttu-id="6e3e8-575">Sprawdź **utworzyć widok jednoznacznie** wyboru i wybierz **albumu** z **klasa modelu** listy rozwijanej.</span><span class="sxs-lookup"><span data-stu-id="6e3e8-575">Check the **Create a strongly-typed view** checkbox and select **Album** from the **Model class** drop-down.</span></span> <span data-ttu-id="6e3e8-576">Pozostaw wartości domyślne innych pól.</span><span class="sxs-lookup"><span data-stu-id="6e3e8-576">Leave the other fields with their default value.</span></span> <span data-ttu-id="6e3e8-577">Następnie kliknij przycisk **Dodaj**.</span><span class="sxs-lookup"><span data-stu-id="6e3e8-577">Then click **Add**.</span></span> <span data-ttu-id="6e3e8-578">Spowoduje to utworzenie i otworzyć **\Views\Store\Details.cshtml** pliku.</span><span class="sxs-lookup"><span data-stu-id="6e3e8-578">This will create and open a **\Views\Store\Details.cshtml** file.</span></span>

    <span data-ttu-id="6e3e8-579">![Dodawanie widoku szczegółów](aspnet-mvc-4-fundamentals/_static/image31.png "Dodawanie widoku szczegółów")</span><span class="sxs-lookup"><span data-stu-id="6e3e8-579">![Adding a Details View](aspnet-mvc-4-fundamentals/_static/image31.png "Adding a Details View")</span></span>

    <span data-ttu-id="6e3e8-580">*Dodawanie widoku szczegółów*</span><span class="sxs-lookup"><span data-stu-id="6e3e8-580">*Adding a Details View*</span></span>
3. <span data-ttu-id="6e3e8-581">Modyfikowanie **Details.cshtml** plik, aby wyświetlić informacje albumu podczas uzyskiwania dostępu do **albumu** obiekt, który jest przekazywany do szablonu widoku.</span><span class="sxs-lookup"><span data-stu-id="6e3e8-581">Modify the **Details.cshtml** file to display the Album's information, accessing the **Album** object that is passed to the view template.</span></span> <span data-ttu-id="6e3e8-582">Aby to zrobić, Zastąp zawartość następującym kodem: (C#)</span><span class="sxs-lookup"><span data-stu-id="6e3e8-582">To do this, replace the content with the following: (C#)</span></span>

    [!code-cshtml[Main](aspnet-mvc-4-fundamentals/samples/sample24.cshtml)]

<a id="Ex6Task7"></a>

<a id="Task_7_-_Running_the_Application"></a>
#### <a name="task-7---running-the-application"></a><span data-ttu-id="6e3e8-583">Zadanie 7 - uruchamiania aplikacji</span><span class="sxs-lookup"><span data-stu-id="6e3e8-583">Task 7 - Running the Application</span></span>

<span data-ttu-id="6e3e8-584">W tym zadaniu zostanie sprawdzić, czy **szczegóły** widok pobiera informacje albumu z **szczegóły akcji** metody.</span><span class="sxs-lookup"><span data-stu-id="6e3e8-584">In this task, you will test that the **Details** View retrieves Album's information from the **Details action** method.</span></span>

1. <span data-ttu-id="6e3e8-585">Naciśnij klawisz **F5** do uruchomienia aplikacji.</span><span class="sxs-lookup"><span data-stu-id="6e3e8-585">Press **F5** to run the Application.</span></span>
2. <span data-ttu-id="6e3e8-586">Projekt jest uruchamiany w **Home** strony.</span><span class="sxs-lookup"><span data-stu-id="6e3e8-586">The project starts in the **Home** page.</span></span> <span data-ttu-id="6e3e8-587">Zmień adres URL do **/Store/Details/5** do sprawdzenia informacji o albumu.</span><span class="sxs-lookup"><span data-stu-id="6e3e8-587">Change the URL to **/Store/Details/5** to verify the album's information.</span></span>

    <span data-ttu-id="6e3e8-588">![Przeglądanie szczegółów albumów](aspnet-mvc-4-fundamentals/_static/image32.png "przeglądania szczegółów albumów")</span><span class="sxs-lookup"><span data-stu-id="6e3e8-588">![Browsing Albums Detail](aspnet-mvc-4-fundamentals/_static/image32.png "Browsing Albums Detail")</span></span>

    <span data-ttu-id="6e3e8-589">*Album przeglądania szczegółów*</span><span class="sxs-lookup"><span data-stu-id="6e3e8-589">*Browsing Album's Detail*</span></span>

<a id="Ex6Task8"></a>

<a id="Task_8_-_Adding_Links_Between_Pages"></a>
#### <a name="task-8---adding-links-between-pages"></a><span data-ttu-id="6e3e8-590">Zadanie 8 — Dodawanie łącza między stronami</span><span class="sxs-lookup"><span data-stu-id="6e3e8-590">Task 8 - Adding Links Between Pages</span></span>

<span data-ttu-id="6e3e8-591">To zadanie będzie dodanie łącza w widoku magazynu w celu zapewnienia łącze każdego rodzaju nazwy do odpowiedniego **/magazynu/Przeglądaj** adresu URL.</span><span class="sxs-lookup"><span data-stu-id="6e3e8-591">In this task, you will add a link in the Store View to have a link in every Genre name to the appropriate **/Store/Browse** URL.</span></span> <span data-ttu-id="6e3e8-592">Dzięki temu po kliknięciu określonego rodzaju, na przykład **Disco**, spowoduje przejście do **/magazynu/Przeglądaj? genre = Disco** adresu URL.</span><span class="sxs-lookup"><span data-stu-id="6e3e8-592">This way, when you click on a Genre, for instance **Disco**, it will navigate to **/Store/Browse?genre=Disco** URL.</span></span>

1. <span data-ttu-id="6e3e8-593">Zamknij przeglądarkę, jeśli to konieczne, aby powrócić do okna programu Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="6e3e8-593">Close the browser if needed, to return to the Visual Studio window.</span></span> <span data-ttu-id="6e3e8-594">Aktualizacja **indeksu** stronę, aby dodać łącze do **Przeglądaj** strony.</span><span class="sxs-lookup"><span data-stu-id="6e3e8-594">Update the **Index** page to add a link to the **Browse** page.</span></span> <span data-ttu-id="6e3e8-595">Aby to zrobić, w **Eksploratora rozwiązań** rozwiń węzeł **widoków** folderu, a następnie **magazynu** folder i kliknij dwukrotnie **Index.cshtml** strony.</span><span class="sxs-lookup"><span data-stu-id="6e3e8-595">To do this, in the **Solution Explorer** expand the **Views** folder, then the **Store** folder and double-click the **Index.cshtml** page.</span></span>
2. <span data-ttu-id="6e3e8-596">Dodaj łącze do widoku przeglądania wskazujący genre wybrane.</span><span class="sxs-lookup"><span data-stu-id="6e3e8-596">Add a link to the Browse view indicating the genre selected.</span></span> <span data-ttu-id="6e3e8-597">Aby to zrobić, Zamień następujące wyróżniony kod w **&lt;li&gt;** tagi: (C#)</span><span class="sxs-lookup"><span data-stu-id="6e3e8-597">To do this, replace the following highlighted code within the **&lt;li&gt;** tags: (C#)</span></span>

    [!code-cshtml[Main](aspnet-mvc-4-fundamentals/samples/sample25.cshtml)]

   > [!NOTE]
   > <span data-ttu-id="6e3e8-598">innym rozwiązaniem będzie łączenie bezpośrednio do strony, przy użyciu kodu podobne do poniższych:</span><span class="sxs-lookup"><span data-stu-id="6e3e8-598">another approach would be linking directly to the page, with a code like the following:</span></span>
   > 
   > <span data-ttu-id="6e3e8-599">&lt;href =&quot;/magazynu/Przeglądaj? genre =@genreName&quot;&gt;@genreName&lt;/a&gt;</span><span class="sxs-lookup"><span data-stu-id="6e3e8-599">&lt;a href=&quot;/Store/Browse?genre=@genreName&quot;&gt;@genreName&lt;/a&gt;</span></span>
   > 
   > <span data-ttu-id="6e3e8-600">Chociaż to rozwiązanie działa, jest zależna od ciągu zapisane na stałe.</span><span class="sxs-lookup"><span data-stu-id="6e3e8-600">Although this approach works, it depends on a hardcoded string.</span></span> <span data-ttu-id="6e3e8-601">Później w przypadku zmiany nazwy kontrolera, należy ręcznie zmienić tej instrukcji.</span><span class="sxs-lookup"><span data-stu-id="6e3e8-601">If you later rename the Controller, you will have to change this instruction manually.</span></span> <span data-ttu-id="6e3e8-602">Lepszym jest użycie **pomocnika kodu HTML** metody.</span><span class="sxs-lookup"><span data-stu-id="6e3e8-602">A better alternative is to use an **HTML Helper** method.</span></span> <span data-ttu-id="6e3e8-603">Platforma ASP.NET MVC zawiera metodę pomocnika kodu HTML, która jest dostępna dla takich zadań.</span><span class="sxs-lookup"><span data-stu-id="6e3e8-603">ASP.NET MVC includes an HTML Helper method which is available for such tasks.</span></span> <span data-ttu-id="6e3e8-604">**Html.ActionLink()** metody pomocnika ułatwia tworzenie HTML **&lt;&gt;** łącza, upewniając się, ścieżki adresu URL są poprawnie zakodowane w adresie URL.</span><span class="sxs-lookup"><span data-stu-id="6e3e8-604">The **Html.ActionLink()** helper method makes it easy to build HTML **&lt;a&gt;** links, making sure URL paths are properly URL encoded.</span></span>
   > 
   > <span data-ttu-id="6e3e8-605">Htlm.ActionLink ma kilka przeciążeń.</span><span class="sxs-lookup"><span data-stu-id="6e3e8-605">Htlm.ActionLink has several overloads.</span></span> <span data-ttu-id="6e3e8-606">W tym ćwiczeniu będzie używany, który przyjmuje trzy parametry:</span><span class="sxs-lookup"><span data-stu-id="6e3e8-606">In this exercise you will use one that takes three parameters:</span></span>
   > 
   > 1. <span data-ttu-id="6e3e8-607">Tekst łącza, która będzie wyświetlana nazwa Genre</span><span class="sxs-lookup"><span data-stu-id="6e3e8-607">Link text, which will display the Genre name</span></span>
   > 2. <span data-ttu-id="6e3e8-608">Nazwa akcji kontrolera (**Przeglądaj**)</span><span class="sxs-lookup"><span data-stu-id="6e3e8-608">Controller action name (**Browse**)</span></span>
   > 3. <span data-ttu-id="6e3e8-609">Wartości parametru, określając nazwę trasy (**Genre**) i wartość (**nazwa Genre**)</span><span class="sxs-lookup"><span data-stu-id="6e3e8-609">Route parameter values, specifying both the name (**Genre**) and the value (**Genre name**)</span></span>

<a id="Ex6Task9"></a>

<a id="Task_9_-_Running_the_Application"></a>
#### <a name="task-9---running-the-application"></a><span data-ttu-id="6e3e8-610">Zadanie 9 — uruchamianie aplikacji</span><span class="sxs-lookup"><span data-stu-id="6e3e8-610">Task 9 - Running the Application</span></span>

<span data-ttu-id="6e3e8-611">W tym zadaniu zostanie przetestować, czy każdego rodzaju są wyświetlane z łączem do odpowiedniej **/magazynu/Przeglądaj** adresu URL.</span><span class="sxs-lookup"><span data-stu-id="6e3e8-611">In this task, you will test that each Genre is displayed with a link to the appropriate **/Store/Browse** URL.</span></span>

1. <span data-ttu-id="6e3e8-612">Naciśnij klawisz **F5** do uruchomienia aplikacji.</span><span class="sxs-lookup"><span data-stu-id="6e3e8-612">Press **F5** to run the Application.</span></span>
2. <span data-ttu-id="6e3e8-613">Na stronie głównej datę rozpoczęcia projektu.</span><span class="sxs-lookup"><span data-stu-id="6e3e8-613">The project starts in the Home page.</span></span> <span data-ttu-id="6e3e8-614">Zmień adres URL do **/magazynu** można zweryfikować, że każdego rodzaju linki do odpowiednich **/magazynu/Przeglądaj** adresu URL.</span><span class="sxs-lookup"><span data-stu-id="6e3e8-614">Change the URL to **/Store** to verify that each Genre links to the appropriate **/Store/Browse** URL.</span></span>

    <span data-ttu-id="6e3e8-615">![Przeglądanie Genres wraz z łączami do przeglądania strony](aspnet-mvc-4-fundamentals/_static/image33.png "przeglądanie Genres wraz z łączami do przeglądania strony")</span><span class="sxs-lookup"><span data-stu-id="6e3e8-615">![Browsing Genres with links to Browse page](aspnet-mvc-4-fundamentals/_static/image33.png "Browsing Genres with links to Browse page")</span></span>

    <span data-ttu-id="6e3e8-616">*Przeglądanie Genres wraz z łączami do przeglądania strony*</span><span class="sxs-lookup"><span data-stu-id="6e3e8-616">*Browsing Genres with links to Browse page*</span></span>

<a id="Ex6Task10"></a>

<a id="Task_10_-_Using_Dynamic_ViewModel_Collection_to_Pass_Values"></a>
#### <a name="task-10---using-dynamic-viewmodel-collection-to-pass-values"></a><span data-ttu-id="6e3e8-617">Zadanie 10 - przy użyciu kolekcji dynamicznych ViewModel do przekazania wartości</span><span class="sxs-lookup"><span data-stu-id="6e3e8-617">Task 10 - Using Dynamic ViewModel Collection to Pass Values</span></span>

<span data-ttu-id="6e3e8-618">W tym zadaniu dowiesz się, prosta i skuteczna metoda przekazać wartości między kontrolerem a widokiem bez wprowadzania żadnych zmian w modelu.</span><span class="sxs-lookup"><span data-stu-id="6e3e8-618">In this task, you will learn a simple and powerful method to pass values between the Controller and the View without making any changes in the Model.</span></span> <span data-ttu-id="6e3e8-619">ASP.NET MVC 4 udostępnia kolekcję &quot;ViewModel&quot;, co przypisane do żadnej wartości dynamiczne i dostęp do wewnątrz również widoków i kontrolerów.</span><span class="sxs-lookup"><span data-stu-id="6e3e8-619">ASP.NET MVC 4 provides the collection &quot;ViewModel&quot;, which can be assigned to any dynamic value and accessed inside controllers and views as well.</span></span>

<span data-ttu-id="6e3e8-620">Teraz użyje kolekcji dynamicznych elementów ViewBag do przekazania listę &quot; **Starred genres** &quot; z kontrolera do widoku.</span><span class="sxs-lookup"><span data-stu-id="6e3e8-620">You will now use the ViewBag dynamic collection to pass a list of &quot;**Starred genres**&quot; from the controller to the view.</span></span> <span data-ttu-id="6e3e8-621">Widok indeksu magazynu będą uzyskiwać dostęp do **ViewModel** i wyświetlić informacje.</span><span class="sxs-lookup"><span data-stu-id="6e3e8-621">The Store Index view will access to **ViewModel** and display the information.</span></span>

1. <span data-ttu-id="6e3e8-622">Zamknij przeglądarkę, jeśli to konieczne, aby powrócić do okna programu Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="6e3e8-622">Close the browser if needed, to return to the Visual Studio window.</span></span> <span data-ttu-id="6e3e8-623">Otwórz **StoreController.cs** i zmodyfikuj **indeksu** metodę w celu utworzenia listy starred genres do kolekcji ViewModel:</span><span class="sxs-lookup"><span data-stu-id="6e3e8-623">Open **StoreController.cs** and modify **Index** method to create a list of starred genres into ViewModel collection :</span></span>


~~~
[!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample26.cs)]

> [!NOTE]
> You could also use the syntax **ViewBag[&quot;Starred&quot;]** to access the properties.
~~~
2. <span data-ttu-id="6e3e8-624">Ikonę gwiazdki **&quot;starred.png&quot;** znajduje się w **Source\Assets\Images** folder tego laboratorium.</span><span class="sxs-lookup"><span data-stu-id="6e3e8-624">The star icon **&quot;starred.png&quot;** is included in the **Source\Assets\Images** folder of this lab.</span></span> <span data-ttu-id="6e3e8-625">Aby dodać go do aplikacji, przeciągnij ich zawartość z **Eksploratora Windows** okna do **Eksploratora rozwiązań** w Visual Web Developer Express, jak pokazano poniżej:</span><span class="sxs-lookup"><span data-stu-id="6e3e8-625">In order to add it to the application, drag their content from a **Windows Explorer** window into the **Solution Explorer** in Visual Web Developer Express, as shown below:</span></span>

    <span data-ttu-id="6e3e8-626">![Dodawanie obrazu gwiazdy do rozwiązania](aspnet-mvc-4-fundamentals/_static/image34.png "Dodawanie gwiazdy obrazu do rozwiązania")</span><span class="sxs-lookup"><span data-stu-id="6e3e8-626">![Adding star image to the solution](aspnet-mvc-4-fundamentals/_static/image34.png "Adding star image to the solution")</span></span>

    <span data-ttu-id="6e3e8-627">*Dodawanie obrazu gwiazdy do rozwiązania*</span><span class="sxs-lookup"><span data-stu-id="6e3e8-627">*Adding star image to the solution*</span></span>
3. <span data-ttu-id="6e3e8-628">Otwórz widok **Store/Index.cshtml** i modyfikować zawartość.</span><span class="sxs-lookup"><span data-stu-id="6e3e8-628">Open the view **Store/Index.cshtml** and modify the content.</span></span> <span data-ttu-id="6e3e8-629">Będzie odczytywał &quot;starred&quot; właściwości w **obiekt ViewBag** kolekcji i poproś, jeśli bieżąca nazwa genre znajduje się na liście.</span><span class="sxs-lookup"><span data-stu-id="6e3e8-629">You will read the &quot;starred&quot; property in the **ViewBag** collection, and ask if the current genre name is in the list.</span></span> <span data-ttu-id="6e3e8-630">W takim przypadku zostaną wyświetlone ikonę gwiazdki prawo do łącza rodzaju.</span><span class="sxs-lookup"><span data-stu-id="6e3e8-630">In that case you will show a star icon right to the genre link.</span></span>
   <span data-ttu-id="6e3e8-631">(C#)</span><span class="sxs-lookup"><span data-stu-id="6e3e8-631">(C#)</span></span>

    [!code-cshtml[Main](aspnet-mvc-4-fundamentals/samples/sample27.cshtml)]

<a id="Ex6Task11"></a>

<a id="Task_11_-_Running_the_Application"></a>
#### <a name="task-11---running-the-application"></a><span data-ttu-id="6e3e8-632">Zadanie 11 - uruchamiania aplikacji</span><span class="sxs-lookup"><span data-stu-id="6e3e8-632">Task 11 - Running the Application</span></span>

<span data-ttu-id="6e3e8-633">W tym zadaniu zostanie przetestować, że jak pokazano genres wyświetlać ikonę gwiazdki.</span><span class="sxs-lookup"><span data-stu-id="6e3e8-633">In this task, you will test that the starred genres display a star icon.</span></span>

1. <span data-ttu-id="6e3e8-634">Naciśnij klawisz **F5** do uruchomienia aplikacji.</span><span class="sxs-lookup"><span data-stu-id="6e3e8-634">Press **F5** to run the Application.</span></span>
2. <span data-ttu-id="6e3e8-635">Projekt jest uruchamiany w **Home** strony.</span><span class="sxs-lookup"><span data-stu-id="6e3e8-635">The project starts in the **Home** page.</span></span> <span data-ttu-id="6e3e8-636">Zmień adres URL do **/magazynu** do sprawdzenia, czy każdego rodzaju polecanych ma wagę do poszanowania etykiety:</span><span class="sxs-lookup"><span data-stu-id="6e3e8-636">Change the URL to **/Store** to verify that each featured genre has the respecting label:</span></span>

    <span data-ttu-id="6e3e8-637">![Przeglądanie Genres elementów jak pokazano na](aspnet-mvc-4-fundamentals/_static/image35.png "przeglądanie Genres elementów jak pokazano na")</span><span class="sxs-lookup"><span data-stu-id="6e3e8-637">![Browsing Genres with starred elements](aspnet-mvc-4-fundamentals/_static/image35.png "Browsing Genres with starred elements")</span></span>

    <span data-ttu-id="6e3e8-638">*Przeglądanie Genres elementów jak pokazano na*</span><span class="sxs-lookup"><span data-stu-id="6e3e8-638">*Browsing Genres with starred elements*</span></span>

<a id="Exercise7"></a>

<a id="Exercise_7_A_lap_around_ASPNET_MVC_4_new_template"></a>
### <a name="exercise-7-a-lap-around-aspnet-mvc-4-new-template"></a><span data-ttu-id="6e3e8-639">Ćwiczenie 7: Laptop wokół nowego szablonu platformy ASP.NET MVC 4</span><span class="sxs-lookup"><span data-stu-id="6e3e8-639">Exercise 7: A lap around ASP.NET MVC 4 new template</span></span>

<span data-ttu-id="6e3e8-640">W tym ćwiczeniu zostanie Poznaj ulepszenia w szablonach projektu programu ASP.NET MVC 4, biorąc spojrzeć na najbardziej istotne cechy nowy szablon.</span><span class="sxs-lookup"><span data-stu-id="6e3e8-640">In this exercise, you will explore the enhancements in the ASP.NET MVC 4 project templates, taking a look at the most relevant features of the new template.</span></span>

<a id="Ex7Task1"></a>

<a id="Task_1_Exploring_the_ASPNET_MVC_4_Internet_Application_Template"></a>
#### <a name="task-1-exploring-the-aspnet-mvc-4-internet-application-template"></a><span data-ttu-id="6e3e8-641">Zadanie 1: Eksploracji szablon aplikacji internetowych platformy ASP.NET MVC 4</span><span class="sxs-lookup"><span data-stu-id="6e3e8-641">Task 1: Exploring the ASP.NET MVC 4 Internet Application Template</span></span>

1. <span data-ttu-id="6e3e8-642">Jeśli nie już otwarty, uruchom **VS Express for Web**</span><span class="sxs-lookup"><span data-stu-id="6e3e8-642">If not already open, start **VS Express for Web**</span></span>
2. <span data-ttu-id="6e3e8-643">Wybierz **pliku | Nowy | Projekt** polecenia menu.</span><span class="sxs-lookup"><span data-stu-id="6e3e8-643">Select the **File | New | Project** menu command.</span></span> <span data-ttu-id="6e3e8-644">W **nowy projekt** okno dialogowe, wybierz opcję **Visual C# | Web** szablonu w lewym okienku drzewa, a następnie wybierz pozycję **aplikacji sieci Web programu ASP.NET MVC 4**.</span><span class="sxs-lookup"><span data-stu-id="6e3e8-644">In the **New Project** dialog, select the **Visual C#|Web** template on the left pane tree, and choose the **ASP.NET MVC 4 Web Application**.</span></span> <span data-ttu-id="6e3e8-645">**Nazwa** projektu *MusicStore* i zaktualizuj **Nazwa rozwiązania** do *rozpocząć*, następnie wybierz lokalizację (lub pozostaw wartość domyślną) i kliknij przycisk **OK** .</span><span class="sxs-lookup"><span data-stu-id="6e3e8-645">**Name** the project *MusicStore* and update the **solution name** to *Begin*, then select a location (or leave the default) and click **OK**.</span></span>

    <span data-ttu-id="6e3e8-646">![Tworzenie nowego projektu platformy ASP.NET MVC 4](aspnet-mvc-4-fundamentals/_static/image36.png "tworzenia nowego projektu platformy ASP.NET MVC 4")</span><span class="sxs-lookup"><span data-stu-id="6e3e8-646">![Creating a new ASP.NET MVC 4 Project](aspnet-mvc-4-fundamentals/_static/image36.png "Creating a new ASP.NET MVC 4 Project")</span></span>

    <span data-ttu-id="6e3e8-647">*Tworzenie nowego projektu platformy ASP.NET MVC 4*</span><span class="sxs-lookup"><span data-stu-id="6e3e8-647">*Creating a new ASP.NET MVC 4 Project*</span></span>
3. <span data-ttu-id="6e3e8-648">W **nowy projekt programu ASP.NET MVC 4** okno dialogowe, wybierz opcję **aplikacji internetowej** szablonu projektu i kliknij przycisk **OK**.</span><span class="sxs-lookup"><span data-stu-id="6e3e8-648">In the **New ASP.NET MVC 4 Project** dialog, select the **Internet Application** project template and click **OK**.</span></span> <span data-ttu-id="6e3e8-649">Należy zauważyć, że można wybrać jako aparat widoku Razor lub ASPX.</span><span class="sxs-lookup"><span data-stu-id="6e3e8-649">Notice you can select either Razor or ASPX as the view engine.</span></span>

    <span data-ttu-id="6e3e8-650">![Tworzenie nowej aplikacji internetowych 4 ASP.NET MVC](aspnet-mvc-4-fundamentals/_static/image37.png "Tworzenie nowej aplikacji internetowych 4 ASP.NET MVC")</span><span class="sxs-lookup"><span data-stu-id="6e3e8-650">![Creating a new ASP.NET MVC 4 Internet Application](aspnet-mvc-4-fundamentals/_static/image37.png "Creating a new ASP.NET MVC 4 Internet Application")</span></span>

    <span data-ttu-id="6e3e8-651">*Tworzenie nowej aplikacji internetowych 4 ASP.NET MVC*</span><span class="sxs-lookup"><span data-stu-id="6e3e8-651">*Creating a new ASP.NET MVC 4 Internet Application*</span></span>

    > [!NOTE]
    > <span data-ttu-id="6e3e8-652">Składnia razor została wprowadzona w programie ASP.NET MVC 3.</span><span class="sxs-lookup"><span data-stu-id="6e3e8-652">Razor syntax has been introduced in ASP.NET MVC 3.</span></span> <span data-ttu-id="6e3e8-653">Jego celem jest zminimalizowanie liczby znaków i naciśnięcia klawiszy wymagane w pliku, umożliwiające szybkie oraz płynu kodowania przepływu pracy.</span><span class="sxs-lookup"><span data-stu-id="6e3e8-653">Its goal is to minimize the number of characters and keystrokes required in a file, enabling a fast and fluid coding workflow.</span></span> <span data-ttu-id="6e3e8-654">Razor wykorzystuje istniejące C# /VB (lub innych) umiejętności język, a następnie dostarcza składnia znacznika szablonu, umożliwiającą świetny przepływu pracy konstrukcji kodu HTML.</span><span class="sxs-lookup"><span data-stu-id="6e3e8-654">Razor leverages existing C#/VB (or other) language skills and delivers a template markup syntax that enables an awesome HTML construction workflow.</span></span>
4. <span data-ttu-id="6e3e8-655">Naciśnij klawisz **F5** uruchomić rozwiązanie i zobaczyć odnowionego szablonu.</span><span class="sxs-lookup"><span data-stu-id="6e3e8-655">Press **F5** to run the solution and see the renewed template.</span></span> <span data-ttu-id="6e3e8-656">Można wyewidencjonować następujące funkcje:</span><span class="sxs-lookup"><span data-stu-id="6e3e8-656">You can check out the following features:</span></span>

    1. <span data-ttu-id="6e3e8-657">**Szablonów w stylu Modern**</span><span class="sxs-lookup"><span data-stu-id="6e3e8-657">**Modern-style templates**</span></span>

        <span data-ttu-id="6e3e8-658">Szablony odnowienia, zapewniając więcej stylów nowoczesny wygląd.</span><span class="sxs-lookup"><span data-stu-id="6e3e8-658">The templates have been renewed, providing more modern-looking styles.</span></span>

        <span data-ttu-id="6e3e8-659">![Szablony ASP.NET MVC 4 restyled](aspnet-mvc-4-fundamentals/_static/image38.png "restyled szablony programu ASP.NET MVC 4")</span><span class="sxs-lookup"><span data-stu-id="6e3e8-659">![ASP.NET MVC 4 restyled templates](aspnet-mvc-4-fundamentals/_static/image38.png "ASP.NET MVC 4 restyled templates")</span></span>

        <span data-ttu-id="6e3e8-660">*Szablony ASP.NET MVC 4 restyled*</span><span class="sxs-lookup"><span data-stu-id="6e3e8-660">*ASP.NET MVC 4 restyled templates*</span></span>
    2. <span data-ttu-id="6e3e8-661">**Renderowanie adaptacyjną**</span><span class="sxs-lookup"><span data-stu-id="6e3e8-661">**Adaptive Rendering**</span></span>

        <span data-ttu-id="6e3e8-662">Zapoznaj się z zmiany rozmiaru okna przeglądarki i zwróć uwagę, jak dynamicznie dostosowuje układ strony do nowego rozmiaru okna.</span><span class="sxs-lookup"><span data-stu-id="6e3e8-662">Check out resizing the browser window and notice how the page layout dynamically adapts to the new window size.</span></span> <span data-ttu-id="6e3e8-663">Te szablony używać techniki adaptacyjną renderowania mają być renderowane poprawnie w zarówno wersje desktop i mobile platform bez potrzeby dostosowywania.</span><span class="sxs-lookup"><span data-stu-id="6e3e8-663">These templates use the adaptive rendering technique to render properly in both desktop and mobile platforms without any customization.</span></span>

        <span data-ttu-id="6e3e8-664">![Szablon projektu programu ASP.NET MVC 4 w innej przeglądarce rozmiary](aspnet-mvc-4-fundamentals/_static/image39.png "szablonu projektu programu ASP.NET MVC 4 w innej przeglądarce rozmiarach")</span><span class="sxs-lookup"><span data-stu-id="6e3e8-664">![ASP.NET MVC 4 project template in different browser sizes](aspnet-mvc-4-fundamentals/_static/image39.png "ASP.NET MVC 4 project template in different browser sizes")</span></span>

        <span data-ttu-id="6e3e8-665">*Szablon projektu programu ASP.NET MVC 4 w innej przeglądarce rozmiarach*</span><span class="sxs-lookup"><span data-stu-id="6e3e8-665">*ASP.NET MVC 4 project template in different browser sizes*</span></span>
5. <span data-ttu-id="6e3e8-666">Zamknij przeglądarkę, aby zatrzymać debuger i powrócić do programu Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="6e3e8-666">Close the browser to stop the debugger and return to Visual Studio.</span></span>
6. <span data-ttu-id="6e3e8-667">Teraz masz możliwość Eksploruj rozwiązania i zapoznaj się z nowych funkcji wprowadzonych przez program ASP.NET MVC 4 w szablonie projektu.</span><span class="sxs-lookup"><span data-stu-id="6e3e8-667">Now you are able to explore the solution and check out some of the new features introduced by ASP.NET MVC 4 in the project template.</span></span>

    <span data-ttu-id="6e3e8-668">![ASP.NET MVC4-internet-— szablon projektu aplikacji-](aspnet-mvc-4-fundamentals/_static/image40.png "szablonu projektu aplikacji internetowych platformy ASP.NET MVC 4")</span><span class="sxs-lookup"><span data-stu-id="6e3e8-668">![ASP.NET MVC4-internet-application-project-template](aspnet-mvc-4-fundamentals/_static/image40.png "The ASP.NET MVC 4 Internet Application Project Template")</span></span>

    <span data-ttu-id="6e3e8-669">*Szablon projektu aplikacji internetowych platformy ASP.NET MVC 4*</span><span class="sxs-lookup"><span data-stu-id="6e3e8-669">*The ASP.NET MVC 4 Internet Application Project Template*</span></span>

   1. <span data-ttu-id="6e3e8-670">**HTML5 znaczników**</span><span class="sxs-lookup"><span data-stu-id="6e3e8-670">**HTML5 markup**</span></span>

       <span data-ttu-id="6e3e8-671">Przeglądaj widoków szablonu, aby dowiedzieć się nowego znacznika motywu, na przykład otwórz **About.cshtml** wyświetlić w **Home** folderu.</span><span class="sxs-lookup"><span data-stu-id="6e3e8-671">Browse template views to find out the new theme markup, for example open **About.cshtml** view within **Home** folder.</span></span>

       <span data-ttu-id="6e3e8-672">![Nowy szablon, przy użyciu znaczników Razor i HTML5](aspnet-mvc-4-fundamentals/_static/image41.png "nowego szablonu, za pomocą znacznika Razor i HTML5")</span><span class="sxs-lookup"><span data-stu-id="6e3e8-672">![New template, using Razor and HTML5 markup](aspnet-mvc-4-fundamentals/_static/image41.png "New template, using Razor and HTML5 markup")</span></span>

       <span data-ttu-id="6e3e8-673">*Nowy szablon, przy użyciu znaczników Razor i HTML5*</span><span class="sxs-lookup"><span data-stu-id="6e3e8-673">*New template, using Razor and HTML5 markup*</span></span>
   2. <span data-ttu-id="6e3e8-674">**JavaScript biblioteki dołączony**</span><span class="sxs-lookup"><span data-stu-id="6e3e8-674">**JavaScript libraries included**</span></span>

      1. <span data-ttu-id="6e3e8-675">**jQuery**: jQuery ułatwia przechodzenie dokumentu HTML, obsługa zdarzeń animacji i interakcje Ajax.</span><span class="sxs-lookup"><span data-stu-id="6e3e8-675">**jQuery**: jQuery simplifies HTML document traversing, event handling, animating, and Ajax interactions.</span></span>
      2. <span data-ttu-id="6e3e8-676">**interfejsu użytkownika jQuery**: Ta biblioteka zawiera obiektów abstrakcyjnych odpowiadających niskiego poziomu interakcji i zaawansowana efekty animacji i elementów WebParts elementy widget, rozszerzający jQuery biblioteka języka JavaScript.</span><span class="sxs-lookup"><span data-stu-id="6e3e8-676">**jQuery UI**: This library provides abstractions for low-level interaction and animation, advanced effects and themeable widgets, built on top of the jQuery JavaScript Library.</span></span>

         > [!NOTE]
         > <span data-ttu-id="6e3e8-677">Informacje na temat jQuery i jQuery interfejsu użytkownika w [ [ http://docs.jquery.com/ ](http://docs.jquery.com/) ](http://docs.jquery.com/).</span><span class="sxs-lookup"><span data-stu-id="6e3e8-677">You can learn about jQuery and jQuery UI in [[http://docs.jquery.com/](http://docs.jquery.com/)](http://docs.jquery.com/).</span></span>
      3. <span data-ttu-id="6e3e8-678">**Elementami KnockoutJS**: szablon domyślny ASP.NET MVC 4 zawiera teraz **elementami KnockoutJS**, platforma JavaScript MVVM, która umożliwia tworzenie aplikacji sieci web bogaty i elastyczny wysokiej przy użyciu języka JavaScript i HTML.</span><span class="sxs-lookup"><span data-stu-id="6e3e8-678">**KnockoutJS**: The ASP.NET MVC 4 default template now includes **KnockoutJS**, a JavaScript MVVM framework that lets you create rich and highly responsive web applications using JavaScript and HTML.</span></span> <span data-ttu-id="6e3e8-679">Podobnie jak w programie ASP.NET MVC 3, jQuery i jQuery biblioteki interfejsu użytkownika znajdują się również w technologii ASP.NET MVC 4.</span><span class="sxs-lookup"><span data-stu-id="6e3e8-679">Like in ASP.NET MVC 3, jQuery and jQuery UI libraries are also included in ASP.NET MVC 4.</span></span>

          > [!NOTE]
          > <span data-ttu-id="6e3e8-680">Możesz uzyskać więcej informacji na temat biblioteki elementami KnockOutJS w to łącze: [ http://learn.knockoutjs.com/ ](http://learn.knockoutjs.com/).</span><span class="sxs-lookup"><span data-stu-id="6e3e8-680">You can get more information about KnockOutJS library in this link: [http://learn.knockoutjs.com/](http://learn.knockoutjs.com/).</span></span>
      4. <span data-ttu-id="6e3e8-681">**Modernizr**: Ta biblioteka jest uruchamiana automatycznie, co zgodne ze starszych przeglądarek witryny przy użyciu technologii HTML5 i CSS3.</span><span class="sxs-lookup"><span data-stu-id="6e3e8-681">**Modernizr**: This library runs automatically, making your site compatible with older browsers when using HTML5 and CSS3 technologies.</span></span>

          > [!NOTE]
          > <span data-ttu-id="6e3e8-682">Możesz uzyskać więcej informacji na temat biblioteki Modernizr w to łącze: [ http://www.modernizr.com/ ](http://www.modernizr.com/).</span><span class="sxs-lookup"><span data-stu-id="6e3e8-682">You can get more information about Modernizr library in this link: [http://www.modernizr.com/](http://www.modernizr.com/).</span></span>
   3. <span data-ttu-id="6e3e8-683">**SimpleMembership zawarty w rozwiązaniu**</span><span class="sxs-lookup"><span data-stu-id="6e3e8-683">**SimpleMembership included in the solution**</span></span>

       <span data-ttu-id="6e3e8-684">SimpleMembership został zaprojektowany jako serwer zamienny dla poprzedniego systemu dostawcy ról ASP.NET i członkostwa.</span><span class="sxs-lookup"><span data-stu-id="6e3e8-684">SimpleMembership has been designed as a replacement for the previous ASP.NET Role and Membership provider system.</span></span> <span data-ttu-id="6e3e8-685">Ma wiele nowych funkcji, które ułatwiają deweloperom bezpiecznych stron sieci web w sposób bardziej elastyczne.</span><span class="sxs-lookup"><span data-stu-id="6e3e8-685">It has many new features that make it easier for the developer to secure web pages in a more flexible way.</span></span>

       <span data-ttu-id="6e3e8-686">Szablon Internet już ma skonfigurować kilka ustawień integracji SimpleMembership, na przykład elementu AccountController jest przygotowane do korzystania z OAuthWebSecurity (w przypadku rejestracji konta OAuth, logowania, zarządzania, itp.) i bezpieczeństwo sieci Web.</span><span class="sxs-lookup"><span data-stu-id="6e3e8-686">The Internet template already has set up a few things to integrate SimpleMembership, for example, the AccountController is prepared to use OAuthWebSecurity (for OAuth account registration, login, management, etc.) and Web Security.</span></span>

       <span data-ttu-id="6e3e8-687">![SimpleMembership zawarty w rozwiązaniu](aspnet-mvc-4-fundamentals/_static/image42.png "SimpleMembership zawarty w rozwiązaniu")</span><span class="sxs-lookup"><span data-stu-id="6e3e8-687">![SimpleMembership Included in the solution](aspnet-mvc-4-fundamentals/_static/image42.png "SimpleMembership Included in the solution")</span></span>

       <span data-ttu-id="6e3e8-688">*SimpleMembership zawarty w rozwiązaniu*</span><span class="sxs-lookup"><span data-stu-id="6e3e8-688">*SimpleMembership Included in the solution*</span></span>

       > [!NOTE]
       > <span data-ttu-id="6e3e8-689">Aby uzyskać więcej informacji o [OAuthWebSecurity](https://msdn.microsoft.com/library/jj158393(v=vs.111).aspx) w witrynie MSDN.</span><span class="sxs-lookup"><span data-stu-id="6e3e8-689">Find more information about [OAuthWebSecurity](https://msdn.microsoft.com/library/jj158393(v=vs.111).aspx) in MSDN.</span></span>

> [!NOTE]
> <span data-ttu-id="6e3e8-690">Ponadto można wdrożyć tę aplikację systemu Windows Azure Web Sites następujących [dodatek B: publikowania aplikacji ASP.NET MVC 4 przy użyciu narzędzia Web Deploy](#AppendixB).</span><span class="sxs-lookup"><span data-stu-id="6e3e8-690">Additionally, you can deploy this application to Windows Azure Web Sites following [Appendix B: Publishing an ASP.NET MVC 4 Application using Web Deploy](#AppendixB).</span></span>


* * *

<a id="Summary"></a>

<a id="Summary"></a>
## <a name="summary"></a><span data-ttu-id="6e3e8-691">Podsumowanie</span><span class="sxs-lookup"><span data-stu-id="6e3e8-691">Summary</span></span>

<span data-ttu-id="6e3e8-692">Wykonując tego laboratorium Hands-On znasz już podstawy ASP.NET MVC:</span><span class="sxs-lookup"><span data-stu-id="6e3e8-692">By completing this Hands-On Lab you have learned the fundamentals of ASP.NET MVC:</span></span>

- <span data-ttu-id="6e3e8-693">Elementy podstawowe aplikacji MVC i sposób ich interakcji</span><span class="sxs-lookup"><span data-stu-id="6e3e8-693">The core elements of an MVC application and how they interact</span></span>
- <span data-ttu-id="6e3e8-694">Tworzenie aplikacji platformy ASP.NET MVC</span><span class="sxs-lookup"><span data-stu-id="6e3e8-694">How to create an ASP.NET MVC Application</span></span>
- <span data-ttu-id="6e3e8-695">Jak dodać i skonfigurować kontrolerów, aby obsłużyć parametry przekazywane przy użyciu adresu URL i querystring</span><span class="sxs-lookup"><span data-stu-id="6e3e8-695">How to add and configure Controllers to handle parameters passed through the URL and querystring</span></span>
- <span data-ttu-id="6e3e8-696">Jak dodać układ strony wzorcowej można skonfigurować szablon dla typowych zawartość HTML, arkusz stylów, aby zwiększyć wyglądu i działania i szablonu widoku, aby wyświetlić zawartość HTML</span><span class="sxs-lookup"><span data-stu-id="6e3e8-696">How to add a layout master page to setup a template for common HTML content, a StyleSheet to enhance the look and feel and a View template to display HTML content</span></span>
- <span data-ttu-id="6e3e8-697">Sposób użycia wzorca ViewModel przekazywania właściwości do szablonu widoku do wyświetlenia informacji dynamicznych</span><span class="sxs-lookup"><span data-stu-id="6e3e8-697">How to use the ViewModel pattern for passing properties to the View template to display dynamic information</span></span>
- <span data-ttu-id="6e3e8-698">Jak używać parametrów przekazanych do kontrolerów w szablonie widoku</span><span class="sxs-lookup"><span data-stu-id="6e3e8-698">How to use parameters passed to Controllers in the View template</span></span>
- <span data-ttu-id="6e3e8-699">Dodawanie łączy do stron w aplikacji ASP.NET MVC</span><span class="sxs-lookup"><span data-stu-id="6e3e8-699">How to add links to pages inside the ASP.NET MVC application</span></span>
- <span data-ttu-id="6e3e8-700">Jak dodać i używania właściwości dynamicznych w widoku</span><span class="sxs-lookup"><span data-stu-id="6e3e8-700">How to add and use dynamic properties in a View</span></span>
- <span data-ttu-id="6e3e8-701">Ulepszenia w szablonach projektu programu ASP.NET MVC 4</span><span class="sxs-lookup"><span data-stu-id="6e3e8-701">The enhancements in the ASP.NET MVC 4 project templates</span></span>

<a id="AppendixA"></a>

<a id="Appendix_A_Installing_Visual_Studio_Express_2012_for_Web"></a>
## <a name="appendix-a-installing-visual-studio-express-2012-for-web"></a><span data-ttu-id="6e3e8-702">Dodatek A: Instalowanie programu Visual Studio Express 2012 for Web</span><span class="sxs-lookup"><span data-stu-id="6e3e8-702">Appendix A: Installing Visual Studio Express 2012 for Web</span></span>

<span data-ttu-id="6e3e8-703">Można zainstalować **Microsoft Visual Studio Express 2012 for Web** lub innym &quot;Express&quot; przy użyciu wersji **[Instalatora platformy sieci Web firmy Microsoft](https://www.microsoft.com/web/downloads/platform.aspx)**.</span><span class="sxs-lookup"><span data-stu-id="6e3e8-703">You can install **Microsoft Visual Studio Express 2012 for Web** or another &quot;Express&quot; version using the **[Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)**.</span></span> <span data-ttu-id="6e3e8-704">Poniższe instrukcje przedstawiono czynności wymagane do zainstalowania *programu Visual studio Express 2012 for Web* przy użyciu *Instalatora platformy sieci Web firmy Microsoft*.</span><span class="sxs-lookup"><span data-stu-id="6e3e8-704">The following instructions guide you through the steps required to install *Visual studio Express 2012 for Web* using *Microsoft Web Platform Installer*.</span></span>

1. <span data-ttu-id="6e3e8-705">Przejdź do [ [ https://go.microsoft.com/?linkid=9810169 ](https://go.microsoft.com/?linkid=9810169) ](https://go.microsoft.com/?linkid=9810169).</span><span class="sxs-lookup"><span data-stu-id="6e3e8-705">Go to [[https://go.microsoft.com/?linkid=9810169](https://go.microsoft.com/?linkid=9810169)](https://go.microsoft.com/?linkid=9810169).</span></span> <span data-ttu-id="6e3e8-706">Alternatywnie, jeśli została już zainstalowana Instalatora platformy sieci Web, można otworzyć go i Wyszukaj produkt &quot; <em>programu Visual Studio Express 2012 for Web z zestawem Windows Azure SDK</em>&quot;.</span><span class="sxs-lookup"><span data-stu-id="6e3e8-706">Alternatively, if you already have installed Web Platform Installer, you can open it and search for the product &quot;<em>Visual Studio Express 2012 for Web with Windows Azure SDK</em>&quot;.</span></span>
2. <span data-ttu-id="6e3e8-707">Polecenie **teraz zainstalować**.</span><span class="sxs-lookup"><span data-stu-id="6e3e8-707">Click on **Install Now**.</span></span> <span data-ttu-id="6e3e8-708">Jeśli nie masz **Instalatora platformy sieci Web** nastąpi przekierowanie do pobrania i zainstalowania go najpierw.</span><span class="sxs-lookup"><span data-stu-id="6e3e8-708">If you do not have **Web Platform Installer** you will be redirected to download and install it first.</span></span>
3. <span data-ttu-id="6e3e8-709">Raz **Instalatora platformy sieci Web** jest otwarty, kliknij przycisk **zainstalować** można uruchomić Instalatora.</span><span class="sxs-lookup"><span data-stu-id="6e3e8-709">Once **Web Platform Installer** is open, click **Install** to start the setup.</span></span>

    <span data-ttu-id="6e3e8-710">![Instalowanie programu Visual Studio Express](aspnet-mvc-4-fundamentals/_static/image43.png "instalacji programu Visual Studio Express")</span><span class="sxs-lookup"><span data-stu-id="6e3e8-710">![Install Visual Studio Express](aspnet-mvc-4-fundamentals/_static/image43.png "Install Visual Studio Express")</span></span>

    <span data-ttu-id="6e3e8-711">*Instalowanie programu Visual Studio Express*</span><span class="sxs-lookup"><span data-stu-id="6e3e8-711">*Install Visual Studio Express*</span></span>
4. <span data-ttu-id="6e3e8-712">Odczytywanie wszystkich produktów licencji i warunków, a następnie kliknij przycisk **akceptuję** aby kontynuować.</span><span class="sxs-lookup"><span data-stu-id="6e3e8-712">Read all the products' licenses and terms and click **I Accept** to continue.</span></span>

    ![Akceptowanie umowy licencyjnej](aspnet-mvc-4-fundamentals/_static/image44.png)

    <span data-ttu-id="6e3e8-714">*Akceptowanie umowy licencyjnej*</span><span class="sxs-lookup"><span data-stu-id="6e3e8-714">*Accepting the license terms*</span></span>
5. <span data-ttu-id="6e3e8-715">Poczekaj na zakończenie procesu pobierania i instalacji.</span><span class="sxs-lookup"><span data-stu-id="6e3e8-715">Wait until the downloading and installation process completes.</span></span>

    ![Postęp instalacji](aspnet-mvc-4-fundamentals/_static/image45.png)

    <span data-ttu-id="6e3e8-717">*Postęp instalacji*</span><span class="sxs-lookup"><span data-stu-id="6e3e8-717">*Installation progress*</span></span>
6. <span data-ttu-id="6e3e8-718">Po zakończeniu instalacji kliknij przycisk **Zakończ**.</span><span class="sxs-lookup"><span data-stu-id="6e3e8-718">When the installation completes, click **Finish**.</span></span>

    ![Instalacja została zakończona](aspnet-mvc-4-fundamentals/_static/image46.png)

    <span data-ttu-id="6e3e8-720">*Instalacja została zakończona*</span><span class="sxs-lookup"><span data-stu-id="6e3e8-720">*Installation completed*</span></span>
7. <span data-ttu-id="6e3e8-721">Kliknij przycisk **zakończenia** aby zamknąć Instalatora platformy sieci Web.</span><span class="sxs-lookup"><span data-stu-id="6e3e8-721">Click **Exit** to close Web Platform Installer.</span></span>
8. <span data-ttu-id="6e3e8-722">Aby otworzyć program Visual Studio Express for Web, przejdź do **Start** ekranu i zacznij pisać &quot; **VS Express**&quot;, następnie kliknij polecenie **VS Express for Web** Kafelek.</span><span class="sxs-lookup"><span data-stu-id="6e3e8-722">To open Visual Studio Express for Web, go to the **Start** screen and start writing &quot;**VS Express**&quot;, then click on the **VS Express for Web** tile.</span></span>

    ![VS Express for Web kafelka](aspnet-mvc-4-fundamentals/_static/image47.png)

    <span data-ttu-id="6e3e8-724">*VS Express for Web kafelka*</span><span class="sxs-lookup"><span data-stu-id="6e3e8-724">*VS Express for Web tile*</span></span>

<a id="AppendixB"></a>

<a id="Appendix_B_Publishing_an_ASPNET_MVC_4_Application_using_Web_Deploy"></a>
## <a name="appendix-b-publishing-an-aspnet-mvc-4-application-using-web-deploy"></a><span data-ttu-id="6e3e8-725">Dodatek B: publikowania aplikacji ASP.NET MVC 4 przy użyciu narzędzia Web Deploy</span><span class="sxs-lookup"><span data-stu-id="6e3e8-725">Appendix B: Publishing an ASP.NET MVC 4 Application using Web Deploy</span></span>

<span data-ttu-id="6e3e8-726">Ten dodatek opisano sposób tworzenia nowej witryny sieci web z portalu zarządzania pakietu Windows Azure i publikowanie aplikacji, uzyskane wykonując laboratorium, korzystając z funkcji publikowania narzędzia Web Deploy dostarczane przez Windows Azure.</span><span class="sxs-lookup"><span data-stu-id="6e3e8-726">This appendix will show you how to create a new web site from the Windows Azure Management Portal and publish the application you obtained by following the lab, taking advantage of the Web Deploy publishing feature provided by Windows Azure.</span></span>

<a id="ApxBTask1"></a>

<a id="Task_1_-_Creating_a_New_Web_Site_from_the_Windows_Azure_Portal"></a>
#### <a name="task-1---creating-a-new-web-site-from-the-windows-azure-portal"></a><span data-ttu-id="6e3e8-727">Zadanie 1 — Tworzenie nowej witryny sieci Web systemu Windows portalu Azure</span><span class="sxs-lookup"><span data-stu-id="6e3e8-727">Task 1 - Creating a New Web Site from the Windows Azure Portal</span></span>

1. <span data-ttu-id="6e3e8-728">Przejdź do [portalu zarządzania pakietu Windows Azure](https://manage.windowsazure.com/) i zaloguj się przy użyciu poświadczeń Microsoft skojarzonych z Twoją subskrypcją.</span><span class="sxs-lookup"><span data-stu-id="6e3e8-728">Go to the [Windows Azure Management Portal](https://manage.windowsazure.com/) and sign in using the Microsoft credentials associated with your subscription.</span></span>

    > [!NOTE]
    > <span data-ttu-id="6e3e8-729">Z systemu Windows Azure można udostępniać 10 witryn sieci Web platformy ASP.NET bezpłatnie i następnie Skaluj w miarę zwiększania się ruchu.</span><span class="sxs-lookup"><span data-stu-id="6e3e8-729">With Windows Azure you can host 10 ASP.NET Web Sites for free and then scale as your traffic grows.</span></span> <span data-ttu-id="6e3e8-730">Możesz utworzyć konto [tutaj](http://aka.ms/aspnet-hol-azure).</span><span class="sxs-lookup"><span data-stu-id="6e3e8-730">You can sign up [here](http://aka.ms/aspnet-hol-azure).</span></span>

    <span data-ttu-id="6e3e8-731">![Zaloguj się do portalu Windows Azure](aspnet-mvc-4-fundamentals/_static/image48.png "Zaloguj się do portalu Windows Azure")</span><span class="sxs-lookup"><span data-stu-id="6e3e8-731">![Log on to Windows Azure portal](aspnet-mvc-4-fundamentals/_static/image48.png "Log on to Windows Azure portal")</span></span>

    <span data-ttu-id="6e3e8-732">*Zaloguj się do portalu zarządzania platformy Azure z systemem Windows*</span><span class="sxs-lookup"><span data-stu-id="6e3e8-732">*Log on to Windows Azure Management Portal*</span></span>
2. <span data-ttu-id="6e3e8-733">Kliknij przycisk **nowy** na pasku poleceń.</span><span class="sxs-lookup"><span data-stu-id="6e3e8-733">Click **New** on the command bar.</span></span>

    <span data-ttu-id="6e3e8-734">![Tworzenie nowej witryny sieci Web](aspnet-mvc-4-fundamentals/_static/image49.png "tworzenia nowej witryny sieci Web")</span><span class="sxs-lookup"><span data-stu-id="6e3e8-734">![Creating a new Web Site](aspnet-mvc-4-fundamentals/_static/image49.png "Creating a new Web Site")</span></span>

    <span data-ttu-id="6e3e8-735">*Tworzenie nowej witryny sieci Web*</span><span class="sxs-lookup"><span data-stu-id="6e3e8-735">*Creating a new Web Site*</span></span>
3. <span data-ttu-id="6e3e8-736">Kliknij przycisk **obliczeniowe** | **witryny sieci Web**.</span><span class="sxs-lookup"><span data-stu-id="6e3e8-736">Click **Compute** | **Web Site**.</span></span> <span data-ttu-id="6e3e8-737">Następnie wybierz **szybkie tworzenie** opcji.</span><span class="sxs-lookup"><span data-stu-id="6e3e8-737">Then select **Quick Create** option.</span></span> <span data-ttu-id="6e3e8-738">Podaj dostępny adres URL dla nowej witryny sieci web, a następnie kliknij przycisk **tworzenie witryny sieci Web**.</span><span class="sxs-lookup"><span data-stu-id="6e3e8-738">Provide an available URL for the new web site and click **Create Web Site**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="6e3e8-739">Witryny sieci Web systemu Windows Azure jest hostem dla aplikacji sieci web w chmurze, które można kontrolować i zarządzanie nimi.</span><span class="sxs-lookup"><span data-stu-id="6e3e8-739">A Windows Azure Web Site is the host for a web application running in the cloud that you can control and manage.</span></span> <span data-ttu-id="6e3e8-740">Opcja szybkie tworzenie umożliwia wdrażanie ukończonej aplikacji sieci web do systemu Windows Azure witryny internetowej z spoza portalu.</span><span class="sxs-lookup"><span data-stu-id="6e3e8-740">The Quick Create option allows you to deploy a completed web application to the Windows Azure Web Site from outside the portal.</span></span> <span data-ttu-id="6e3e8-741">Nie obejmuje kroki konfigurowania bazy danych.</span><span class="sxs-lookup"><span data-stu-id="6e3e8-741">It does not include steps for setting up a database.</span></span>

    <span data-ttu-id="6e3e8-742">![Tworzenie nowej witryny sieci Web przy użyciu szybkie tworzenie](aspnet-mvc-4-fundamentals/_static/image50.png "tworzenia nowej witryny sieci Web przy użyciu szybkie tworzenie")</span><span class="sxs-lookup"><span data-stu-id="6e3e8-742">![Creating a new Web Site using Quick Create](aspnet-mvc-4-fundamentals/_static/image50.png "Creating a new Web Site using Quick Create")</span></span>

    <span data-ttu-id="6e3e8-743">*Tworzenie nowej witryny sieci Web przy użyciu szybkie tworzenie*</span><span class="sxs-lookup"><span data-stu-id="6e3e8-743">*Creating a new Web Site using Quick Create*</span></span>
4. <span data-ttu-id="6e3e8-744">Poczekaj na nowe **witryny sieci Web** jest tworzony.</span><span class="sxs-lookup"><span data-stu-id="6e3e8-744">Wait until the new **Web Site** is created.</span></span>
5. <span data-ttu-id="6e3e8-745">Po utworzeniu witryny sieci Web kliknij łącze w obszarze **adres URL** kolumny.</span><span class="sxs-lookup"><span data-stu-id="6e3e8-745">Once the Web Site is created click the link under the **URL** column.</span></span> <span data-ttu-id="6e3e8-746">Sprawdź, czy działa nowej witryny sieci Web.</span><span class="sxs-lookup"><span data-stu-id="6e3e8-746">Check that the new Web Site is working.</span></span>

    <span data-ttu-id="6e3e8-747">![Przeglądanie do nowej witryny sieci web](aspnet-mvc-4-fundamentals/_static/image51.png "przeglądanie do nowej witryny sieci web")</span><span class="sxs-lookup"><span data-stu-id="6e3e8-747">![Browsing to the new web site](aspnet-mvc-4-fundamentals/_static/image51.png "Browsing to the new web site")</span></span>

    <span data-ttu-id="6e3e8-748">*Przeglądanie do nowej witryny sieci web*</span><span class="sxs-lookup"><span data-stu-id="6e3e8-748">*Browsing to the new web site*</span></span>

    <span data-ttu-id="6e3e8-749">![Witryna sieci Web działa](aspnet-mvc-4-fundamentals/_static/image52.png "uruchamiania witryny sieci Web")</span><span class="sxs-lookup"><span data-stu-id="6e3e8-749">![Web site running](aspnet-mvc-4-fundamentals/_static/image52.png "Web site running")</span></span>

    <span data-ttu-id="6e3e8-750">*Witryna sieci Web uruchomiona*</span><span class="sxs-lookup"><span data-stu-id="6e3e8-750">*Web site running*</span></span>
6. <span data-ttu-id="6e3e8-751">Wróć do portalu i kliknij nazwę witryny sieci web w obszarze **nazwa** kolumny do wyświetlenia strony zarządzania.</span><span class="sxs-lookup"><span data-stu-id="6e3e8-751">Go back to the portal and click the name of the web site under the **Name** column to display the management pages.</span></span>

    <span data-ttu-id="6e3e8-752">![Otwieranie stron witryny sieci web zarządzania](aspnet-mvc-4-fundamentals/_static/image53.png "otwieranie stron zarządzania witryny sieci web")</span><span class="sxs-lookup"><span data-stu-id="6e3e8-752">![Opening the web site management pages](aspnet-mvc-4-fundamentals/_static/image53.png "Opening the web site management pages")</span></span>

    <span data-ttu-id="6e3e8-753">*Otwieranie stron zarządzania witryny sieci Web*</span><span class="sxs-lookup"><span data-stu-id="6e3e8-753">*Opening the Web Site management pages*</span></span>
7. <span data-ttu-id="6e3e8-754">W **pulpitu nawigacyjnego** w obszarze **szybkiego dostępu** kliknij **pobieranie profilu publikowania** łącza.</span><span class="sxs-lookup"><span data-stu-id="6e3e8-754">In the **Dashboard** page, under the **quick glance** section, click the **Download publish profile** link.</span></span>

    > [!NOTE]
    > <span data-ttu-id="6e3e8-755">*Profilu publikowania* zawiera wszystkie informacje wymagane do publikowania aplikacji sieci web do witryny sieci Web systemu Windows Azure dla każdej metody włączone publikacji.</span><span class="sxs-lookup"><span data-stu-id="6e3e8-755">The *publish profile* contains all of the information required to publish a web application to a Windows Azure website for each enabled publication method.</span></span> <span data-ttu-id="6e3e8-756">Profil publikowania zawiera adresy URL, poświadczenia użytkownika i parametry bazy danych wymagane do nawiązania połączenia i uwierzytelniania dla każdego z punktów końcowych, dla których włączono metoda publikacji.</span><span class="sxs-lookup"><span data-stu-id="6e3e8-756">The publish profile contains the URLs, user credentials and database strings required to connect to and authenticate against each of the endpoints for which a publication method is enabled.</span></span> <span data-ttu-id="6e3e8-757">**Microsoft WebMatrix 2**, **Microsoft Visual Studio Express for Web** i **programu Microsoft Visual Studio 2012** obsługują odczytywanie publikowanie profile do zautomatyzowania te programy Publikowanie aplikacji sieci web do witryn sieci Web systemu Windows Azure.</span><span class="sxs-lookup"><span data-stu-id="6e3e8-757">**Microsoft WebMatrix 2**, **Microsoft Visual Studio Express for Web** and **Microsoft Visual Studio 2012** support reading publish profiles to automate configuration of these programs for publishing web applications to Windows Azure websites.</span></span>

    <span data-ttu-id="6e3e8-758">![Pobieranie witryny sieci web profilu publikowania](aspnet-mvc-4-fundamentals/_static/image54.png "pobierania witryny sieci web profilu publikowania")</span><span class="sxs-lookup"><span data-stu-id="6e3e8-758">![Downloading the web site publish profile](aspnet-mvc-4-fundamentals/_static/image54.png "Downloading the web site publish profile")</span></span>

    <span data-ttu-id="6e3e8-759">*Pobieranie witryny sieci Web profilu publikowania*</span><span class="sxs-lookup"><span data-stu-id="6e3e8-759">*Downloading the Web Site publish profile*</span></span>
8. <span data-ttu-id="6e3e8-760">Pobierz profil publikowania w znanej lokalizacji.</span><span class="sxs-lookup"><span data-stu-id="6e3e8-760">Download the publish profile file to a known location.</span></span> <span data-ttu-id="6e3e8-761">Dodatkowo w tym ćwiczeniu zobaczysz jak opublikować aplikację sieci web do witryny sieci Web systemu Windows Azure w programie Visual Studio przy użyciu tego pliku.</span><span class="sxs-lookup"><span data-stu-id="6e3e8-761">Further in this exercise you will see how to use this file to publish a web application to a Windows Azure Web Sites from Visual Studio.</span></span>

    <span data-ttu-id="6e3e8-762">![Zapisywanie pliku profilu publikowania](aspnet-mvc-4-fundamentals/_static/image55.png "zapisywanie profilu publikowania")</span><span class="sxs-lookup"><span data-stu-id="6e3e8-762">![Saving the publish profile file](aspnet-mvc-4-fundamentals/_static/image55.png "Saving the publish profile")</span></span>

    <span data-ttu-id="6e3e8-763">*Zapisywanie pliku profilu publikowania*</span><span class="sxs-lookup"><span data-stu-id="6e3e8-763">*Saving the publish profile file*</span></span>

<a id="ApxBTask2"></a>

<a id="Task_2_-_Configuring_the_Database_Server"></a>
#### <a name="task-2---configuring-the-database-server"></a><span data-ttu-id="6e3e8-764">Zadanie 2 — Konfigurowanie serwera bazy danych</span><span class="sxs-lookup"><span data-stu-id="6e3e8-764">Task 2 - Configuring the Database Server</span></span>

<span data-ttu-id="6e3e8-765">Jeśli aplikacja korzysta z programu SQL Server baz danych, należy utworzyć serwer bazy danych SQL.</span><span class="sxs-lookup"><span data-stu-id="6e3e8-765">If your application makes use of SQL Server databases you will need to create a SQL Database server.</span></span> <span data-ttu-id="6e3e8-766">Jeśli chcesz wdrożyć prostą aplikację, która nie korzysta z programu SQL Server może pominąć to zadanie.</span><span class="sxs-lookup"><span data-stu-id="6e3e8-766">If you want to deploy a simple application that does not use SQL Server you might skip this task.</span></span>

1. <span data-ttu-id="6e3e8-767">Będzie potrzebny serwer bazy danych SQL do przechowywania bazy danych aplikacji.</span><span class="sxs-lookup"><span data-stu-id="6e3e8-767">You will need a SQL Database server for storing the application database.</span></span> <span data-ttu-id="6e3e8-768">Można wyświetlić serwery bazy danych SQL z subskrypcji usługi Windows Azure Management Portal pod adresem **baz danych Sql** | **serwerów** | **serwera Pulpit nawigacyjny**.</span><span class="sxs-lookup"><span data-stu-id="6e3e8-768">You can view the SQL Database servers from your subscription in the Windows Azure Management portal at **Sql Databases** | **Servers** | **Server's Dashboard**.</span></span> <span data-ttu-id="6e3e8-769">Jeśli nie masz serwer, który został utworzony, można utworzyć przy użyciu jednego **Dodaj** przycisk paska poleceń.</span><span class="sxs-lookup"><span data-stu-id="6e3e8-769">If you do not have a server created, you can create one using the **Add** button on the command bar.</span></span> <span data-ttu-id="6e3e8-770">Zwróć uwagę na **nazwę serwera i adres URL, nazwę logowania administratora i hasła**, jak będą używane w następnego zadania.</span><span class="sxs-lookup"><span data-stu-id="6e3e8-770">Take note of the **server name and URL, administrator login name and password**, as you will use them in the next tasks.</span></span> <span data-ttu-id="6e3e8-771">Nie należy tworzyć bazy danych jeszcze, jako zostaną utworzone w późniejszym terminie.</span><span class="sxs-lookup"><span data-stu-id="6e3e8-771">Do not create the database yet, as it will be created in a later stage.</span></span>

    <span data-ttu-id="6e3e8-772">![Pulpit nawigacyjny serwera bazy danych SQL](aspnet-mvc-4-fundamentals/_static/image56.png "pulpitu nawigacyjnego serwera bazy danych SQL")</span><span class="sxs-lookup"><span data-stu-id="6e3e8-772">![SQL Database Server Dashboard](aspnet-mvc-4-fundamentals/_static/image56.png "SQL Database Server Dashboard")</span></span>

    <span data-ttu-id="6e3e8-773">*Pulpit nawigacyjny serwera bazy danych SQL*</span><span class="sxs-lookup"><span data-stu-id="6e3e8-773">*SQL Database Server Dashboard*</span></span>
2. <span data-ttu-id="6e3e8-774">W następnym zadaniem Testuj połączenie z bazą danych z programu Visual Studio z tego powodu należy uwzględnić lokalny adres IP serwera liście **dozwolone adresy IP**.</span><span class="sxs-lookup"><span data-stu-id="6e3e8-774">In the next task you will test the database connection from Visual Studio, for that reason you need to include your local IP address in the server's list of **Allowed IP Addresses**.</span></span> <span data-ttu-id="6e3e8-775">Aby to zrobić, kliknij przycisk **Konfiguruj**, wybierz adres IP z **bieżącego adresu IP klienta** i wklej go na **początkowy adres IP** i **końcowy adres IP** pola tekstowe i kliknij przycisk ![add-client-ip-address-ok-button](aspnet-mvc-4-fundamentals/_static/image57.png) przycisku.</span><span class="sxs-lookup"><span data-stu-id="6e3e8-775">To do that, click **Configure**, select the IP address from **Current Client IP Address** and paste it on the **Start IP Address** and **End IP Address** text boxes and click the ![add-client-ip-address-ok-button](aspnet-mvc-4-fundamentals/_static/image57.png) button.</span></span>

    ![Dodawanie adresu IP klienta](aspnet-mvc-4-fundamentals/_static/image58.png)

    <span data-ttu-id="6e3e8-777">*Dodawanie adresu IP klienta*</span><span class="sxs-lookup"><span data-stu-id="6e3e8-777">*Adding Client IP Address*</span></span>
3. <span data-ttu-id="6e3e8-778">Raz **adres IP klienta** jest dodawany do dozwolonych adresów IP kliknij na **zapisać** o potwierdzenie zmian.</span><span class="sxs-lookup"><span data-stu-id="6e3e8-778">Once the **Client IP Address** is added to the allowed IP addresses list, click on **Save** to confirm the changes.</span></span>

    ![Potwierdzenie zmian](aspnet-mvc-4-fundamentals/_static/image59.png)

    <span data-ttu-id="6e3e8-780">*Potwierdzenie zmian*</span><span class="sxs-lookup"><span data-stu-id="6e3e8-780">*Confirm Changes*</span></span>

<a id="ApxBTask3"></a>

<a id="Task_3_-_Publishing_an_ASPNET_MVC_4_Application_using_Web_Deploy"></a>
#### <a name="task-3---publishing-an-aspnet-mvc-4-application-using-web-deploy"></a><span data-ttu-id="6e3e8-781">Zadanie 3 - publikowania aplikacji ASP.NET MVC 4 przy użyciu narzędzia Web Deploy</span><span class="sxs-lookup"><span data-stu-id="6e3e8-781">Task 3 - Publishing an ASP.NET MVC 4 Application using Web Deploy</span></span>

1. <span data-ttu-id="6e3e8-782">Wróć do rozwiązania ASP.NET MVC 4.</span><span class="sxs-lookup"><span data-stu-id="6e3e8-782">Go back to the ASP.NET MVC 4 solution.</span></span> <span data-ttu-id="6e3e8-783">W **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy projekt witryny sieci web i wybierz **publikowania**.</span><span class="sxs-lookup"><span data-stu-id="6e3e8-783">In the **Solution Explorer**, right-click the web site project and select **Publish**.</span></span>

    <span data-ttu-id="6e3e8-784">![Publikowanie aplikacji](aspnet-mvc-4-fundamentals/_static/image60.png "publikowania aplikacji")</span><span class="sxs-lookup"><span data-stu-id="6e3e8-784">![Publishing the Application](aspnet-mvc-4-fundamentals/_static/image60.png "Publishing the Application")</span></span>

    <span data-ttu-id="6e3e8-785">*Publikowanie witryny sieci web*</span><span class="sxs-lookup"><span data-stu-id="6e3e8-785">*Publishing the web site*</span></span>
2. <span data-ttu-id="6e3e8-786">Zaimportuj profil publikowania, zapisana w pierwszym zadaniu.</span><span class="sxs-lookup"><span data-stu-id="6e3e8-786">Import the publish profile you saved in the first task.</span></span>

    <span data-ttu-id="6e3e8-787">![Importowanie profilu publikowania](aspnet-mvc-4-fundamentals/_static/image61.png "Importowanie profilu publikowania")</span><span class="sxs-lookup"><span data-stu-id="6e3e8-787">![Importing the publish profile](aspnet-mvc-4-fundamentals/_static/image61.png "Importing the publish profile")</span></span>

    <span data-ttu-id="6e3e8-788">*Importowanie profilu publikowania*</span><span class="sxs-lookup"><span data-stu-id="6e3e8-788">*Importing publish profile*</span></span>
3. <span data-ttu-id="6e3e8-789">Kliknij przycisk **Weryfikacja połączenia z**.</span><span class="sxs-lookup"><span data-stu-id="6e3e8-789">Click **Validate Connection**.</span></span> <span data-ttu-id="6e3e8-790">Po zakończeniu sprawdzania kliknij **dalej**.</span><span class="sxs-lookup"><span data-stu-id="6e3e8-790">Once Validation is complete click **Next**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="6e3e8-791">Zakończeniu sprawdzania poprawności, gdy zostanie wyświetlony zielony znacznik wyboru są wyświetlane obok przycisku sprawdzania poprawności połączenia.</span><span class="sxs-lookup"><span data-stu-id="6e3e8-791">Validation is complete once you see a green checkmark appear next to the Validate Connection button.</span></span>

    <span data-ttu-id="6e3e8-792">![Sprawdzanie poprawności połączenia](aspnet-mvc-4-fundamentals/_static/image62.png "sprawdzanie poprawności połączenia")</span><span class="sxs-lookup"><span data-stu-id="6e3e8-792">![Validating connection](aspnet-mvc-4-fundamentals/_static/image62.png "Validating connection")</span></span>

    <span data-ttu-id="6e3e8-793">*Sprawdzanie poprawności połączenia*</span><span class="sxs-lookup"><span data-stu-id="6e3e8-793">*Validating connection*</span></span>
4. <span data-ttu-id="6e3e8-794">W **ustawienia** w obszarze **baz danych** sekcji, kliknij przycisk Dalej, aby textbox połączenia bazy danych (tj. **połączenia DefaultConnection**).</span><span class="sxs-lookup"><span data-stu-id="6e3e8-794">In the **Settings** page, under the **Databases** section, click the button next to your database connection's textbox (i.e. **DefaultConnection**).</span></span>

    <span data-ttu-id="6e3e8-795">![Konfiguracja narzędzia Web deploy](aspnet-mvc-4-fundamentals/_static/image63.png "Konfiguracja narzędzia Web deploy")</span><span class="sxs-lookup"><span data-stu-id="6e3e8-795">![Web deploy configuration](aspnet-mvc-4-fundamentals/_static/image63.png "Web deploy configuration")</span></span>

    <span data-ttu-id="6e3e8-796">*Konfiguracja narzędzia Web deploy*</span><span class="sxs-lookup"><span data-stu-id="6e3e8-796">*Web deploy configuration*</span></span>
5. <span data-ttu-id="6e3e8-797">Skonfiguruj połączenie z bazą danych w następujący sposób:</span><span class="sxs-lookup"><span data-stu-id="6e3e8-797">Configure the database connection as follows:</span></span>

   - <span data-ttu-id="6e3e8-798">W **nazwy serwera** wpisz swoją bazą danych SQL server adresu URL przy użyciu *tcp:* prefiks.</span><span class="sxs-lookup"><span data-stu-id="6e3e8-798">In the **Server name** type your SQL Database server URL using the *tcp:* prefix.</span></span>
   - <span data-ttu-id="6e3e8-799">W **nazwy użytkownika** wpisz nazwę logowania administratora serwera.</span><span class="sxs-lookup"><span data-stu-id="6e3e8-799">In **User name** type your server administrator login name.</span></span>
   - <span data-ttu-id="6e3e8-800">W **hasło** wpisz hasło logowania administratora serwera.</span><span class="sxs-lookup"><span data-stu-id="6e3e8-800">In **Password** type your server administrator login password.</span></span>
   - <span data-ttu-id="6e3e8-801">Wpisz nazwę nowej bazy danych, na przykład: *MVC4SampleDB*.</span><span class="sxs-lookup"><span data-stu-id="6e3e8-801">Type a new database name, for example: *MVC4SampleDB*.</span></span>

     <span data-ttu-id="6e3e8-802">![Konfigurowanie parametrów połączenia z lokalizacją docelową](aspnet-mvc-4-fundamentals/_static/image64.png "Konfigurowanie parametrów połączenia z lokalizacją docelową")</span><span class="sxs-lookup"><span data-stu-id="6e3e8-802">![Configuring destination connection string](aspnet-mvc-4-fundamentals/_static/image64.png "Configuring destination connection string")</span></span>

     <span data-ttu-id="6e3e8-803">*Konfigurowanie parametrów połączenia z lokalizacją docelową*</span><span class="sxs-lookup"><span data-stu-id="6e3e8-803">*Configuring destination connection string*</span></span>
6. <span data-ttu-id="6e3e8-804">Następnie kliknij przycisk **OK**.</span><span class="sxs-lookup"><span data-stu-id="6e3e8-804">Then click **OK**.</span></span> <span data-ttu-id="6e3e8-805">Po wyświetleniu monitu można utworzyć bazy danych kliknij **tak**.</span><span class="sxs-lookup"><span data-stu-id="6e3e8-805">When prompted to create the database click **Yes**.</span></span>

    <span data-ttu-id="6e3e8-806">![Tworzenie bazy danych](aspnet-mvc-4-fundamentals/_static/image65.png "tworzenie parametry bazy danych")</span><span class="sxs-lookup"><span data-stu-id="6e3e8-806">![Creating the database](aspnet-mvc-4-fundamentals/_static/image65.png "Creating the database string")</span></span>

    <span data-ttu-id="6e3e8-807">*Tworzenie bazy danych*</span><span class="sxs-lookup"><span data-stu-id="6e3e8-807">*Creating the database*</span></span>
7. <span data-ttu-id="6e3e8-808">Ciągu połączenia używanego do łączenia z bazą danych SQL w systemie Windows Azure jest wyświetlany w pole tekstowe domyślne połączenie.</span><span class="sxs-lookup"><span data-stu-id="6e3e8-808">The connection string you will use to connect to SQL Database in Windows Azure is shown within Default Connection textbox.</span></span> <span data-ttu-id="6e3e8-809">Następnie kliknij przycisk **Dalej**.</span><span class="sxs-lookup"><span data-stu-id="6e3e8-809">Then click **Next**.</span></span>

    <span data-ttu-id="6e3e8-810">![Parametry połączenia wskazujące bazę danych SQL](aspnet-mvc-4-fundamentals/_static/image66.png "ciąg połączenia wskazujące bazę danych SQL")</span><span class="sxs-lookup"><span data-stu-id="6e3e8-810">![Connection string pointing to SQL Database](aspnet-mvc-4-fundamentals/_static/image66.png "Connection string pointing to SQL Database")</span></span>

    <span data-ttu-id="6e3e8-811">*Parametry połączenia wskazujące bazę danych SQL*</span><span class="sxs-lookup"><span data-stu-id="6e3e8-811">*Connection string pointing to SQL Database*</span></span>
8. <span data-ttu-id="6e3e8-812">W **Podgląd** kliknij przycisk **publikowania**.</span><span class="sxs-lookup"><span data-stu-id="6e3e8-812">In the **Preview** page, click **Publish**.</span></span>

    <span data-ttu-id="6e3e8-813">![Publikowanie aplikacji sieci web](aspnet-mvc-4-fundamentals/_static/image67.png "publikowania aplikacji sieci web")</span><span class="sxs-lookup"><span data-stu-id="6e3e8-813">![Publishing the web application](aspnet-mvc-4-fundamentals/_static/image67.png "Publishing the web application")</span></span>

    <span data-ttu-id="6e3e8-814">*Publikowanie aplikacji sieci web*</span><span class="sxs-lookup"><span data-stu-id="6e3e8-814">*Publishing the web application*</span></span>
9. <span data-ttu-id="6e3e8-815">Po zakończeniu procesu publikowania domyślnej przeglądarce otworzy opublikowanej witryny sieci web.</span><span class="sxs-lookup"><span data-stu-id="6e3e8-815">Once the publishing process finishes, your default browser will open the published web site.</span></span>

    <span data-ttu-id="6e3e8-816">![Aplikacja opublikowana w systemie Windows Azure](aspnet-mvc-4-fundamentals/_static/image68.png "aplikacji publikowanych w systemie Windows Azure")</span><span class="sxs-lookup"><span data-stu-id="6e3e8-816">![Application published to Windows Azure](aspnet-mvc-4-fundamentals/_static/image68.png "Application published to Windows Azure")</span></span>

    <span data-ttu-id="6e3e8-817">*Aplikacji publikowanych w systemie Windows Azure*</span><span class="sxs-lookup"><span data-stu-id="6e3e8-817">*Application published to Windows Azure*</span></span>

<a id="AppendixC"></a>

<a id="Appendix_C_Using_Code_Snippets"></a>
## <a name="appendix-c-using-code-snippets"></a><span data-ttu-id="6e3e8-818">Dodatek C: korzystania z wstawek kodu</span><span class="sxs-lookup"><span data-stu-id="6e3e8-818">Appendix C: Using Code Snippets</span></span>

<span data-ttu-id="6e3e8-819">Wstawki kodu zapewniają całego kodu, które są potrzebne w zasięgu ręki.</span><span class="sxs-lookup"><span data-stu-id="6e3e8-819">With code snippets, you have all the code you need at your fingertips.</span></span> <span data-ttu-id="6e3e8-820">Dokument laboratorium informuje o dokładnie po ich użycia, jak pokazano na poniższej ilustracji.</span><span class="sxs-lookup"><span data-stu-id="6e3e8-820">The lab document will tell you exactly when you can use them, as shown in the following figure.</span></span>

<span data-ttu-id="6e3e8-821">![Aby wstawić kod do projektu przy użyciu wstawki kodu programu Visual Studio](aspnet-mvc-4-fundamentals/_static/image69.png "wstawki kodu za pomocą programu Visual Studio, aby wstawić kod do projektu")</span><span class="sxs-lookup"><span data-stu-id="6e3e8-821">![Using Visual Studio code snippets to insert code into your project](aspnet-mvc-4-fundamentals/_static/image69.png "Using Visual Studio code snippets to insert code into your project")</span></span>

<span data-ttu-id="6e3e8-822">*Aby wstawić kod do projektu przy użyciu wstawki kodu programu Visual Studio*</span><span class="sxs-lookup"><span data-stu-id="6e3e8-822">*Using Visual Studio code snippets to insert code into your project*</span></span>

<span data-ttu-id="6e3e8-823">***Aby dodać fragment kodu za pomocą klawiatury (C# tylko)***</span><span class="sxs-lookup"><span data-stu-id="6e3e8-823">***To add a code snippet using the keyboard (C# only)***</span></span>

1. <span data-ttu-id="6e3e8-824">Umieść kursor, w którym chcesz wstawić kod.</span><span class="sxs-lookup"><span data-stu-id="6e3e8-824">Place the cursor where you would like to insert the code.</span></span>
2. <span data-ttu-id="6e3e8-825">Zacznij wpisywać nazwę fragment (bez spacji i łączniki).</span><span class="sxs-lookup"><span data-stu-id="6e3e8-825">Start typing the snippet name (without spaces or hyphens).</span></span>
3. <span data-ttu-id="6e3e8-826">Obejrzyj jako IntelliSense wyświetla zgodne z nazwami wstawki.</span><span class="sxs-lookup"><span data-stu-id="6e3e8-826">Watch as IntelliSense displays matching snippets' names.</span></span>
4. <span data-ttu-id="6e3e8-827">Wybierz prawidłowe fragment (lub zachować wpisywanie do momentu wybrania fragment całą nazwę).</span><span class="sxs-lookup"><span data-stu-id="6e3e8-827">Select the correct snippet (or keep typing until the entire snippet's name is selected).</span></span>
5. <span data-ttu-id="6e3e8-828">Naciśnij klawisz Tab dwa razy, aby wstawić fragment kodu w lokalizacji kursora.</span><span class="sxs-lookup"><span data-stu-id="6e3e8-828">Press the Tab key twice to insert the snippet at the cursor location.</span></span>

<span data-ttu-id="6e3e8-829">![Rozpocznij wpisywanie nazwy fragment](aspnet-mvc-4-fundamentals/_static/image70.png "zacznij wpisywać nazwę wstawki programu")</span><span class="sxs-lookup"><span data-stu-id="6e3e8-829">![Start typing the snippet name](aspnet-mvc-4-fundamentals/_static/image70.png "Start typing the snippet name")</span></span>

<span data-ttu-id="6e3e8-830">*Rozpocznij wpisywanie nazwy fragment kodu*</span><span class="sxs-lookup"><span data-stu-id="6e3e8-830">*Start typing the snippet name*</span></span>

<span data-ttu-id="6e3e8-831">![Naciśnij klawisz Tab, aby wybrać wyróżnione fragment](aspnet-mvc-4-fundamentals/_static/image71.png "naciśnij klawisz Tab, aby wybrać wyróżnione fragment kodu")</span><span class="sxs-lookup"><span data-stu-id="6e3e8-831">![Press Tab to select the highlighted snippet](aspnet-mvc-4-fundamentals/_static/image71.png "Press Tab to select the highlighted snippet")</span></span>

<span data-ttu-id="6e3e8-832">*Naciśnij klawisz Tab, aby wybrać wyróżnione fragment kodu*</span><span class="sxs-lookup"><span data-stu-id="6e3e8-832">*Press Tab to select the highlighted snippet*</span></span>

<span data-ttu-id="6e3e8-833">![Ponownie naciśnij klawisz Tab i fragment rozwinie](aspnet-mvc-4-fundamentals/_static/image72.png "rozwinie ponownie naciśnij klawisz Tab i wstawki kodu")</span><span class="sxs-lookup"><span data-stu-id="6e3e8-833">![Press Tab again and the snippet will expand](aspnet-mvc-4-fundamentals/_static/image72.png "Press Tab again and the snippet will expand")</span></span>

<span data-ttu-id="6e3e8-834">*Rozwinie ponownie naciśnij klawisz Tab i wstawki kodu*</span><span class="sxs-lookup"><span data-stu-id="6e3e8-834">*Press Tab again and the snippet will expand*</span></span>

<span data-ttu-id="6e3e8-835">***Aby dodać fragment kodu za pomocą myszy (C#, Visual Basic i XML)*** 1.</span><span class="sxs-lookup"><span data-stu-id="6e3e8-835">***To add a code snippet using the mouse (C#, Visual Basic and XML)*** 1.</span></span> <span data-ttu-id="6e3e8-836">Kliknij prawym przyciskiem myszy, gdy chcesz wstawić fragment kodu.</span><span class="sxs-lookup"><span data-stu-id="6e3e8-836">Right-click where you want to insert the code snippet.</span></span>

1. <span data-ttu-id="6e3e8-837">Wybierz **wstawić fragment** następuje **Moje wstawki kodu**.</span><span class="sxs-lookup"><span data-stu-id="6e3e8-837">Select **Insert Snippet** followed by **My Code Snippets**.</span></span>
2. <span data-ttu-id="6e3e8-838">Wybierz odpowiedni fragment z listy, klikając go.</span><span class="sxs-lookup"><span data-stu-id="6e3e8-838">Pick the relevant snippet from the list, by clicking on it.</span></span>

<span data-ttu-id="6e3e8-839">![Kliknij prawym przyciskiem myszy, gdy chcesz wstawić fragment kodu i wybierz wstawić fragment](aspnet-mvc-4-fundamentals/_static/image73.png "kliknij prawym przyciskiem myszy, gdy chcesz wstawić fragment kodu i wybierz wstawić fragment kodu")</span><span class="sxs-lookup"><span data-stu-id="6e3e8-839">![Right-click where you want to insert the code snippet and select Insert Snippet](aspnet-mvc-4-fundamentals/_static/image73.png "Right-click where you want to insert the code snippet and select Insert Snippet")</span></span>

<span data-ttu-id="6e3e8-840">*Kliknij prawym przyciskiem myszy, gdy chcesz wstawić fragment kodu i wybierz wstawić fragment kodu*</span><span class="sxs-lookup"><span data-stu-id="6e3e8-840">*Right-click where you want to insert the code snippet and select Insert Snippet*</span></span>

<span data-ttu-id="6e3e8-841">![Wybierz odpowiedni fragment z listy, klikając go](aspnet-mvc-4-fundamentals/_static/image74.png "wybierz odpowiedni fragment z listy, klikając go")</span><span class="sxs-lookup"><span data-stu-id="6e3e8-841">![Pick the relevant snippet from the list, by clicking on it](aspnet-mvc-4-fundamentals/_static/image74.png "Pick the relevant snippet from the list, by clicking on it")</span></span>

<span data-ttu-id="6e3e8-842">*Wybierz odpowiedni fragment z listy, klikając go*</span><span class="sxs-lookup"><span data-stu-id="6e3e8-842">*Pick the relevant snippet from the list, by clicking on it*</span></span>
