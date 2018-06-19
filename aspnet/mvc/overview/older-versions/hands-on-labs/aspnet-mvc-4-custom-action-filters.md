---
uid: mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-custom-action-filters
title: Filtry akcji niestandardowych platformy ASP.NET MVC 4 | Dokumentacja firmy Microsoft
author: rick-anderson
description: ASP.NET MVC udostępnia filtry akcji wykonywania logiki filtrowania przed lub po nosi nazwę metody akcji. Filtry akcji są określona atrybutów niestandardowych...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/18/2013
ms.topic: article
ms.assetid: 969ab824-1b98-4552-81fe-b60ef5fc6887
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-custom-action-filters
msc.type: authoredcontent
ms.openlocfilehash: 8b135b23aea64b0c7c7d4368eef9ee80914159e4
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/06/2018
ms.locfileid: "30877713"
---
# <a name="aspnet-mvc-4-custom-action-filters"></a><span data-ttu-id="b2aff-104">Filtry akcji niestandardowych platformy ASP.NET MVC 4</span><span class="sxs-lookup"><span data-stu-id="b2aff-104">ASP.NET MVC 4 Custom Action Filters</span></span>

<span data-ttu-id="b2aff-105">Przez [obozów sieci Web Team](https://twitter.com/webcamps)</span><span class="sxs-lookup"><span data-stu-id="b2aff-105">By [Web Camps Team](https://twitter.com/webcamps)</span></span>

[<span data-ttu-id="b2aff-106">Pobierz obozów sieci Web uczenie Kit</span><span class="sxs-lookup"><span data-stu-id="b2aff-106">Download Web Camps Training Kit</span></span>](https://aka.ms/webcamps-training-kit)

<span data-ttu-id="b2aff-107">ASP.NET MVC udostępnia filtry akcji wykonywania logiki filtrowania przed lub po nosi nazwę metody akcji.</span><span class="sxs-lookup"><span data-stu-id="b2aff-107">ASP.NET MVC provides Action Filters for executing filtering logic either before or after an action method is called.</span></span> <span data-ttu-id="b2aff-108">Filtry akcji są atrybutów niestandardowych, zawierających deklaratywne oznacza dodanie akcji przed i po zachowania do metod akcji kontrolera.</span><span class="sxs-lookup"><span data-stu-id="b2aff-108">Action Filters are custom attributes that provide declarative means to add pre-action and post-action behavior to the controller's action methods.</span></span>

<span data-ttu-id="b2aff-109">W tym laboratorium Hands-on spowoduje utworzenie atrybutu filtru akcji niestandardowej do rozwiązania MvcMusicStore catch kontrolera żądań i rejestrowanie aktywności lokacji do tabeli bazy danych.</span><span class="sxs-lookup"><span data-stu-id="b2aff-109">In this Hands-on Lab you will create a custom action filter attribute into MvcMusicStore solution to catch controller's requests and log the activity of a site into a database table.</span></span> <span data-ttu-id="b2aff-110">Będzie można dodać filtr rejestrowania przez uruchomienie kontrolera lub akcji.</span><span class="sxs-lookup"><span data-stu-id="b2aff-110">You will be able to add your logging filter by injection to any controller or action.</span></span> <span data-ttu-id="b2aff-111">Na koniec zostanie wyświetlony widok dziennika z listą użytkowników.</span><span class="sxs-lookup"><span data-stu-id="b2aff-111">Finally, you will see the log view that shows the list of visitors.</span></span>

<span data-ttu-id="b2aff-112">W tym laboratorium Hands-on zakłada mieć podstawową wiedzę na temat **ASP.NET MVC**.</span><span class="sxs-lookup"><span data-stu-id="b2aff-112">This Hands-on Lab assumes you have basic knowledge of **ASP.NET MVC**.</span></span> <span data-ttu-id="b2aff-113">Jeśli nie używasz **ASP.NET MVC** przed, zalecamy zapoznać się z **podstawowe informacje na temat platformy ASP.NET MVC 4** Hands-on laboratorium.</span><span class="sxs-lookup"><span data-stu-id="b2aff-113">If you have not used **ASP.NET MVC** before, we recommend you to go over **ASP.NET MVC 4 Fundamentals** Hands-on Lab.</span></span>

> [!NOTE]
> <span data-ttu-id="b2aff-114">Wszystkie przykładowy kod i fragmenty kodu są uwzględnione w sieci Web obozów zestaw szkoleniowy, pod [wersje Microsoft-Web/WebCampTrainingKit](https://aka.ms/webcamps-training-kit).</span><span class="sxs-lookup"><span data-stu-id="b2aff-114">All sample code and snippets are included in the Web Camps Training Kit, available from at [Microsoft-Web/WebCampTrainingKit Releases](https://aka.ms/webcamps-training-kit).</span></span> <span data-ttu-id="b2aff-115">Specyficzne dla tego laboratorium projektu jest dostępna na [filtry akcji niestandardowych platformy ASP.NET MVC 4](https://github.com/Microsoft-Web/HOL-MVC4CustomActionFilters).</span><span class="sxs-lookup"><span data-stu-id="b2aff-115">The project specific to this lab is available at [ASP.NET MVC 4 Custom Action Filters](https://github.com/Microsoft-Web/HOL-MVC4CustomActionFilters).</span></span>

<a id="Objectives"></a>
### <a name="objectives"></a><span data-ttu-id="b2aff-116">Cele</span><span class="sxs-lookup"><span data-stu-id="b2aff-116">Objectives</span></span>

<span data-ttu-id="b2aff-117">W tym laboratorium Hands-On przedstawiono sposób:</span><span class="sxs-lookup"><span data-stu-id="b2aff-117">In this Hands-On Lab, you will learn how to:</span></span>

- <span data-ttu-id="b2aff-118">Tworzenie atrybutu filtru akcji niestandardowej, aby rozszerzyć możliwości filtrowania</span><span class="sxs-lookup"><span data-stu-id="b2aff-118">Create a custom action filter attribute to extend filtering capabilities</span></span>
- <span data-ttu-id="b2aff-119">Zastosuj atrybut niestandardowy filtr przez wstrzyknięcie do określonego poziomu</span><span class="sxs-lookup"><span data-stu-id="b2aff-119">Apply a custom filter attribute by injection to a specific level</span></span>
- <span data-ttu-id="b2aff-120">Zarejestruj globalnie filtry akcji niestandardowej</span><span class="sxs-lookup"><span data-stu-id="b2aff-120">Register a custom action filters globally</span></span>

<a id="Prerequisites"></a>

<a id="Prerequisites"></a>
### <a name="prerequisites"></a><span data-ttu-id="b2aff-121">Wymagania wstępne</span><span class="sxs-lookup"><span data-stu-id="b2aff-121">Prerequisites</span></span>

<span data-ttu-id="b2aff-122">Musi mieć następujące elementy do przygotowania tego laboratorium:</span><span class="sxs-lookup"><span data-stu-id="b2aff-122">You must have the following items to complete this lab:</span></span>

- <span data-ttu-id="b2aff-123">[Microsoft Visual Studio Express 2012 for Web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web) lub wyższego poziomu (odczytu [dodatek a.](#AppendixA) instrukcje dotyczące sposobu jego instalacji).</span><span class="sxs-lookup"><span data-stu-id="b2aff-123">[Microsoft Visual Studio Express 2012 for Web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web) or superior (read [Appendix A](#AppendixA) for instructions on how to install it).</span></span>

<a id="Setup"></a>

<a id="Setup"></a>
### <a name="setup"></a><span data-ttu-id="b2aff-124">Konfiguracja</span><span class="sxs-lookup"><span data-stu-id="b2aff-124">Setup</span></span>

<span data-ttu-id="b2aff-125">**Instalowanie wstawki kodu**</span><span class="sxs-lookup"><span data-stu-id="b2aff-125">**Installing Code Snippets**</span></span>

<span data-ttu-id="b2aff-126">Dla wygody taki kod, który będzie zarządzany wzdłuż tego laboratorium jest dostępna jako wstawki kodu programu Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="b2aff-126">For convenience, much of the code you will be managing along this lab is available as Visual Studio code snippets.</span></span> <span data-ttu-id="b2aff-127">Aby zainstalować fragmenty kodu, uruchom **.\Source\Setup\CodeSnippets.vsi** pliku.</span><span class="sxs-lookup"><span data-stu-id="b2aff-127">To install the code snippets run **.\Source\Setup\CodeSnippets.vsi** file.</span></span>

<span data-ttu-id="b2aff-128">Jeśli nie masz doświadczenia z wstawki programu Visual Studio i chcesz dowiedzieć się, jak ich używać, można odwołać się do dodatku z tego dokumentu &quot; [wstawki kodu za pomocą programu dodatek C:](#AppendixC)&quot;.</span><span class="sxs-lookup"><span data-stu-id="b2aff-128">If you are not familiar with the Visual Studio Code Snippets, and want to learn how to use them, you can refer to the appendix from this document &quot;[Appendix C: Using Code Snippets](#AppendixC)&quot;.</span></span>

* * *

<a id="Exercises"></a>

<a id="Exercises"></a>
## <a name="exercises"></a><span data-ttu-id="b2aff-129">Ćwiczenia</span><span class="sxs-lookup"><span data-stu-id="b2aff-129">Exercises</span></span>

<span data-ttu-id="b2aff-130">W tym laboratorium Hands-On składa się przez następujących czynnościach:</span><span class="sxs-lookup"><span data-stu-id="b2aff-130">This Hands-On Lab is comprised by the following exercises:</span></span>

1. [<span data-ttu-id="b2aff-131">Ćwiczenie 1: Rejestrowanie akcji</span><span class="sxs-lookup"><span data-stu-id="b2aff-131">Exercise 1: Logging actions</span></span>](#Exercise1)
2. [<span data-ttu-id="b2aff-132">Ćwiczenie 2: Zarządzanie wielu filtrów akcji</span><span class="sxs-lookup"><span data-stu-id="b2aff-132">Exercise 2: Managing Multiple Action Filters</span></span>](#Exercise2)

<span data-ttu-id="b2aff-133">Szacowany czas trwania tego laboratorium: **30 minut**.</span><span class="sxs-lookup"><span data-stu-id="b2aff-133">Estimated time to complete this lab: **30 minutes**.</span></span>

> [!NOTE]
> <span data-ttu-id="b2aff-134">Towarzyszy każdego wykonywania **zakończenia** folderu zawierającego wynikowy rozwiązanie, należy uzyskać po wykonaniu ćwiczeniach.</span><span class="sxs-lookup"><span data-stu-id="b2aff-134">Each exercise is accompanied by an **End** folder containing the resulting solution you should obtain after completing the exercises.</span></span> <span data-ttu-id="b2aff-135">Jeśli potrzebujesz dodatkowej pomocy pracuje nad ćwiczeniami, można użyć tego rozwiązania jako przewodnika.</span><span class="sxs-lookup"><span data-stu-id="b2aff-135">You can use this solution as a guide if you need additional help working through the exercises.</span></span>


<a id="Exercise1"></a>

<a id="Exercise_1_Logging_Actions"></a>
### <a name="exercise-1-logging-actions"></a><span data-ttu-id="b2aff-136">Ćwiczenie 1: Rejestrowanie akcji</span><span class="sxs-lookup"><span data-stu-id="b2aff-136">Exercise 1: Logging Actions</span></span>

<span data-ttu-id="b2aff-137">W tym ćwiczeniu dowiesz się, jak utworzyć filtr dziennika akcji niestandardowej przy użyciu dostawców filtrów programu ASP.NET MVC 4.</span><span class="sxs-lookup"><span data-stu-id="b2aff-137">In this exercise, you will learn how to create a custom action log filter by using ASP.NET MVC 4 Filter Providers.</span></span> <span data-ttu-id="b2aff-138">W tym celu filtr rejestrowania będą dotyczyć lokacji MusicStore, który będzie rejestrować wszystkie działania w wybranych kontrolerów.</span><span class="sxs-lookup"><span data-stu-id="b2aff-138">For that purpose you will apply a logging filter to the MusicStore site that will record all the activities in the selected controllers.</span></span>

<span data-ttu-id="b2aff-139">Rozszerzenie filtr **ActionFilterAttributeClass** i zastąpić **OnActionExecuting** metodę przechwytuje każde żądanie, a następnie wykonać akcje rejestrowania.</span><span class="sxs-lookup"><span data-stu-id="b2aff-139">The filter will extend **ActionFilterAttributeClass** and override **OnActionExecuting** method to catch each request and then perform the logging actions.</span></span> <span data-ttu-id="b2aff-140">Informacje o kontekście dotyczące żądania HTTP, wykonywanie metod, wyniki i parametry będą udostępniane przez program ASP.NET MVC **ActionExecutingContext** klasy **.**</span><span class="sxs-lookup"><span data-stu-id="b2aff-140">The context information about HTTP requests, executing methods, results and parameters will be provided by ASP.NET MVC **ActionExecutingContext** class **.**</span></span>

> [!NOTE]
> <span data-ttu-id="b2aff-141">ASP.NET MVC 4 ma również domyślnych dostawców filtrów, których można używać bez tworzenia niestandardowego filtru.</span><span class="sxs-lookup"><span data-stu-id="b2aff-141">ASP.NET MVC 4 also has default filters providers you can use without creating a custom filter.</span></span> <span data-ttu-id="b2aff-142">ASP.NET MVC 4 zawiera następujące typy filtrów:</span><span class="sxs-lookup"><span data-stu-id="b2aff-142">ASP.NET MVC 4 provides the following types of filters:</span></span>
> 
> - <span data-ttu-id="b2aff-143">**Autoryzacji** filtrować, co czyni zabezpieczeń decyzji o tym, czy można wykonać metody akcji, takich jak uwierzytelniania lub sprawdzania poprawności właściwości żądania.</span><span class="sxs-lookup"><span data-stu-id="b2aff-143">**Authorization** filter, which makes security decisions about whether to execute an action method, such as performing authentication or validating properties of the request.</span></span>
> - <span data-ttu-id="b2aff-144">**Akcja** filtr, który opakowuje wykonanie metody akcji.</span><span class="sxs-lookup"><span data-stu-id="b2aff-144">**Action** filter, which wraps the action method execution.</span></span> <span data-ttu-id="b2aff-145">Ten filtr można wykonać przetwarzania dodatkowe, takie jak dostarczanie dodatkowe dane do metody akcji, sprawdzania wartości zwracanej lub anulowanie wykonywania metody akcji</span><span class="sxs-lookup"><span data-stu-id="b2aff-145">This filter can perform additional processing, such as providing extra data to the action method, inspecting the return value, or canceling execution of the action method</span></span>
> - <span data-ttu-id="b2aff-146">**Wynik** filtr, który opakowuje wykonanie obiektu ActionResult.</span><span class="sxs-lookup"><span data-stu-id="b2aff-146">**Result** filter, which wraps execution of the ActionResult object.</span></span> <span data-ttu-id="b2aff-147">Ten filtr może wykonywać dodatkowego przetwarzania wyniku, takie jak modyfikowanie odpowiedzi HTTP.</span><span class="sxs-lookup"><span data-stu-id="b2aff-147">This filter can perform additional processing of the result, such as modifying the HTTP response.</span></span>
> - <span data-ttu-id="b2aff-148">**Wyjątek** filtr, który wykonuje przypadku nieobsługiwany wyjątek gdzieś w metodzie akcji, począwszy od filtry autoryzacji i kończy wykonywanie wyniku.</span><span class="sxs-lookup"><span data-stu-id="b2aff-148">**Exception** filter, which executes if there is an unhandled exception thrown somewhere in action method, starting with the authorization filters and ending with the execution of the result.</span></span> <span data-ttu-id="b2aff-149">Filtry wyjątków może służyć do zadań, takich jak rejestrowanie lub wyświetlanie stronę błędu.</span><span class="sxs-lookup"><span data-stu-id="b2aff-149">Exception filters can be used for tasks such as logging or displaying an error page.</span></span>
> 
> <span data-ttu-id="b2aff-150">Aby uzyskać więcej informacji o dostawcach filtry odwiedź linkiem MSDN: ([https://msdn.microsoft.com/library/dd410209.aspx](https://msdn.microsoft.com/library/dd410209.aspx)).</span><span class="sxs-lookup"><span data-stu-id="b2aff-150">For more information about Filters Providers please visit this MSDN link: ([https://msdn.microsoft.com/library/dd410209.aspx](https://msdn.microsoft.com/library/dd410209.aspx)) .</span></span>


<a id="AboutLoggingFeature"></a>

<a id="About_MVC_Music_Store_Application_logging_feature"></a>
#### <a name="about-mvc-music-store-application-logging-feature"></a><span data-ttu-id="b2aff-151">Informacje o funkcji rejestrowania aplikacji programu MVC utworów muzycznych magazynu</span><span class="sxs-lookup"><span data-stu-id="b2aff-151">About MVC Music Store Application logging feature</span></span>

<span data-ttu-id="b2aff-152">To rozwiązanie magazynu utworów muzycznych ma nowej tabeli modelu danych do rejestrowania lokacji **ActionLog**, z następującymi polami: Nazwa kontrolera, który odebrał żądanie, wywoływanej akcji, IP klienta i sygnaturę czasową.</span><span class="sxs-lookup"><span data-stu-id="b2aff-152">This Music Store solution has a new data model table for site logging, **ActionLog**, with the following fields: Name of the controller that received a request, Called action, Client IP and Time stamp.</span></span>

<span data-ttu-id="b2aff-153">![Model danych. Tabela ActionLog. ](aspnet-mvc-4-custom-action-filters/_static/image1.png "Modelu danych. Tabela ActionLog.")</span><span class="sxs-lookup"><span data-stu-id="b2aff-153">![Data model. ActionLog table.](aspnet-mvc-4-custom-action-filters/_static/image1.png "Data model. ActionLog table.")</span></span>

<span data-ttu-id="b2aff-154">*Model danych - ActionLog tabeli*</span><span class="sxs-lookup"><span data-stu-id="b2aff-154">*Data model - ActionLog table*</span></span>

<span data-ttu-id="b2aff-155">Rozwiązanie zawiera widok MVC ASP.NET do dziennika akcji, który znajduje się w temacie **ActionLog-MvcMusicStores/widoków**:</span><span class="sxs-lookup"><span data-stu-id="b2aff-155">The solution provides an ASP.NET MVC View for the Action log that can be found at **MvcMusicStores/Views/ActionLog**:</span></span>

<span data-ttu-id="b2aff-156">![Widok dziennika akcji](aspnet-mvc-4-custom-action-filters/_static/image2.png "widok dziennika akcji")</span><span class="sxs-lookup"><span data-stu-id="b2aff-156">![Action Log view](aspnet-mvc-4-custom-action-filters/_static/image2.png "Action Log view")</span></span>

<span data-ttu-id="b2aff-157">*Widok dziennika akcji*</span><span class="sxs-lookup"><span data-stu-id="b2aff-157">*Action Log view*</span></span>

<span data-ttu-id="b2aff-158">Z tym podane struktury całą pracę koncentrować się na przerywania żądania kontrolera, a następnie wykonać rejestrowanie przy użyciu niestandardowych filtrowania.</span><span class="sxs-lookup"><span data-stu-id="b2aff-158">With this given structure, all the work will be focused on interrupting controller's request and performing the logging by using custom filtering.</span></span>

<a id="Ex1Task1"></a>

<a id="Task_1_-_Creating_a_Custom_Filter_to_Catch_a_Controllers_Request"></a>
#### <a name="task-1---creating-a-custom-filter-to-catch-a-controllers-request"></a><span data-ttu-id="b2aff-159">Zadanie 1 — Tworzenie niestandardowego filtru do wychwytywania żądania kontrolera</span><span class="sxs-lookup"><span data-stu-id="b2aff-159">Task 1 - Creating a Custom Filter to Catch a Controller's Request</span></span>

<span data-ttu-id="b2aff-160">W ramach tego zadania spowoduje utworzenie klasy atrybutu niestandardowego filtru, która będzie zawierać logikę rejestrowania.</span><span class="sxs-lookup"><span data-stu-id="b2aff-160">In this task you will create a custom filter attribute class that will contain the logging logic.</span></span> <span data-ttu-id="b2aff-161">W tym celu można rozszerzyć ASP.NET MVC **ActionFilterAttribute** klasy i zaimplementuj interfejs **IActionFilter**.</span><span class="sxs-lookup"><span data-stu-id="b2aff-161">For that purpose you will extend ASP.NET MVC **ActionFilterAttribute** Class and implement the interface **IActionFilter**.</span></span>

> [!NOTE]
> <span data-ttu-id="b2aff-162">**ActionFilterAttribute** jest klasa podstawowa dla wszystkich filtrów atrybutu.</span><span class="sxs-lookup"><span data-stu-id="b2aff-162">The **ActionFilterAttribute** is the base class for all the attribute filters.</span></span> <span data-ttu-id="b2aff-163">Udostępnia ona następujące metody umożliwiające wykonanie konkretnej logiki po i przed wykonaniem akcji kontrolera:</span><span class="sxs-lookup"><span data-stu-id="b2aff-163">It provides the following methods to execute a specific logic after and before controller action's execution:</span></span>
> 
> - <span data-ttu-id="b2aff-164">**OnActionExecuting**(ActionExecutingContext filterContext): tuż przed akcji jest wywoływana metoda.</span><span class="sxs-lookup"><span data-stu-id="b2aff-164">**OnActionExecuting**(ActionExecutingContext filterContext): Just before the action method is called.</span></span>
> - <span data-ttu-id="b2aff-165">**OnActionExecuted**(ActionExecutedContext filterContext): po wywołaniu metody akcji i przed (przed renderowania widoku) jest wykonywany wynik.</span><span class="sxs-lookup"><span data-stu-id="b2aff-165">**OnActionExecuted**(ActionExecutedContext filterContext): After the action method is called and before the result is executed (before view render).</span></span>
> - <span data-ttu-id="b2aff-166">**OnResultExecuting**(ResultExecutingContext filterContext): bezpośrednio przed (przed renderowania widoku) jest wykonywany wynik.</span><span class="sxs-lookup"><span data-stu-id="b2aff-166">**OnResultExecuting**(ResultExecutingContext filterContext): Just before the result is executed (before view render).</span></span>
> - <span data-ttu-id="b2aff-167">**OnResultExecuted**(ResultExecutedContext filterContext): po wykonaniu wyniku (po renderowania widoku).</span><span class="sxs-lookup"><span data-stu-id="b2aff-167">**OnResultExecuted**(ResultExecutedContext filterContext): After the result is executed (after the view is rendered).</span></span>
> 
> <span data-ttu-id="b2aff-168">Przez zastąpienie jednej z tych metod w klasie pochodnej, można wykonać filtrowania kodu.</span><span class="sxs-lookup"><span data-stu-id="b2aff-168">By overriding any of these methods into a derived class, you can execute your own filtering code.</span></span>


1. <span data-ttu-id="b2aff-169">Otwórz **rozpocząć** rozwiązania, znajdujących się na **\Source\Ex01-LoggingActions\Begin** folderu.</span><span class="sxs-lookup"><span data-stu-id="b2aff-169">Open the **Begin** solution located at **\Source\Ex01-LoggingActions\Begin** folder.</span></span>

   1. <span data-ttu-id="b2aff-170">Należy pobrać niektórych brakujących pakietów NuGet, przed kontynuowaniem.</span><span class="sxs-lookup"><span data-stu-id="b2aff-170">You will need to download some missing NuGet packages before you continue.</span></span> <span data-ttu-id="b2aff-171">Aby to zrobić, kliknij przycisk **projektu** menu i wybierz **Zarządzaj pakietami NuGet**.</span><span class="sxs-lookup"><span data-stu-id="b2aff-171">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
   2. <span data-ttu-id="b2aff-172">W **Zarządzaj pakietami NuGet** okna dialogowego, kliknij przycisk **przywrócić** celu pobieranie brakujących pakietów.</span><span class="sxs-lookup"><span data-stu-id="b2aff-172">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
   3. <span data-ttu-id="b2aff-173">Na koniec Skompiluj rozwiązanie, klikając **kompilacji** | **Kompiluj rozwiązanie**.</span><span class="sxs-lookup"><span data-stu-id="b2aff-173">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

      > [!NOTE]
      > <span data-ttu-id="b2aff-174">Jedną z zalet przy użyciu narzędzia NuGet jest, że nie masz do wysłania wszystkich bibliotek w projekcie, zmniejszenie jego rozmiar projektu.</span><span class="sxs-lookup"><span data-stu-id="b2aff-174">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="b2aff-175">Narzędzia Power NuGet określając wersje pakietów w pliku Packages.config, będzie można pobrać wymaganych bibliotek podczas pierwszego uruchomienia projektu.</span><span class="sxs-lookup"><span data-stu-id="b2aff-175">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="b2aff-176">Jest to, dlaczego konieczne będzie wykonanie tych kroków, po otwarciu istniejącego rozwiązania z tego laboratorium.</span><span class="sxs-lookup"><span data-stu-id="b2aff-176">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
      > 
      > <span data-ttu-id="b2aff-177">Aby uzyskać więcej informacji, zobacz ten artykuł: [ http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages ](http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages).</span><span class="sxs-lookup"><span data-stu-id="b2aff-177">For more information, see this article: [http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages](http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages).</span></span>
2. <span data-ttu-id="b2aff-178">Dodaj nową klasę C# do **filtry** folder i nadaj mu nazwę *CustomActionFilter.cs*.</span><span class="sxs-lookup"><span data-stu-id="b2aff-178">Add a new C# class into the **Filters** folder and name it *CustomActionFilter.cs*.</span></span> <span data-ttu-id="b2aff-179">Ten folder będzie przechowywać wszystkie filtry niestandardowe.</span><span class="sxs-lookup"><span data-stu-id="b2aff-179">This folder will store all the custom filters.</span></span>
3. <span data-ttu-id="b2aff-180">Otwórz **CustomActionFilter.cs** i Dodaj odwołanie do **System.Web.Mvc** i **MvcMusicStore.Models** przestrzeni nazw:</span><span class="sxs-lookup"><span data-stu-id="b2aff-180">Open **CustomActionFilter.cs** and add a reference to **System.Web.Mvc** and **MvcMusicStore.Models** namespaces:</span></span>

    <span data-ttu-id="b2aff-181">(Fragment - kodu *filtry akcji niestandardowych platformy ASP.NET MVC 4 - Ex1 CustomActionFilterNamespaces*)</span><span class="sxs-lookup"><span data-stu-id="b2aff-181">(Code Snippet - *ASP.NET MVC 4 Custom Action Filters - Ex1-CustomActionFilterNamespaces*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample1.cs)]
4. <span data-ttu-id="b2aff-182">Dziedzicz **CustomActionFilter** klasę z **ActionFilterAttribute** , a następnie wprowadź **CustomActionFilter** implementacji klasy **IActionFilter** interfejsu.</span><span class="sxs-lookup"><span data-stu-id="b2aff-182">Inherit the **CustomActionFilter** class from **ActionFilterAttribute** and then make **CustomActionFilter** class implement **IActionFilter** interface.</span></span>

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample2.cs)]
5. <span data-ttu-id="b2aff-183">Wprowadź **CustomActionFilter** klasy przesłonić metodę **OnActionExecuting** i Dodaj logikę niezbędne do logowania się wykonanie filtru.</span><span class="sxs-lookup"><span data-stu-id="b2aff-183">Make **CustomActionFilter** class override the method **OnActionExecuting** and add the necessary logic to log the filter's execution.</span></span> <span data-ttu-id="b2aff-184">Aby to zrobić, Dodaj następujący wyróżniony kod w **CustomActionFilter** klasy.</span><span class="sxs-lookup"><span data-stu-id="b2aff-184">To do this, add the following highlighted code within **CustomActionFilter** class.</span></span>

    <span data-ttu-id="b2aff-185">(Fragment - kodu *filtry akcji niestandardowych platformy ASP.NET MVC 4 - Ex1 LoggingActions*)</span><span class="sxs-lookup"><span data-stu-id="b2aff-185">(Code Snippet - *ASP.NET MVC 4 Custom Action Filters - Ex1-LoggingActions*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample3.cs#Highlight)]

    > [!NOTE]
    > <span data-ttu-id="b2aff-186">**OnActionExecuting** używa metody **Entity Framework** można dodać nowego rejestru ActionLog.</span><span class="sxs-lookup"><span data-stu-id="b2aff-186">**OnActionExecuting** method is using **Entity Framework** to add a new ActionLog register.</span></span> <span data-ttu-id="b2aff-187">Tworzy i wypełnia nowe wystąpienie jednostki informacje o kontekście z **filterContext**.</span><span class="sxs-lookup"><span data-stu-id="b2aff-187">It creates and fills a new entity instance with the context information from **filterContext**.</span></span>
    > 
    > <span data-ttu-id="b2aff-188">Możesz przeczytać dodatkowe informacje **kontekstem ControllerContext** klasy w [msdn](https://msdn.microsoft.com/library/system.web.mvc.controllercontext.aspx).</span><span class="sxs-lookup"><span data-stu-id="b2aff-188">You can read more about **ControllerContext** class at [msdn](https://msdn.microsoft.com/library/system.web.mvc.controllercontext.aspx).</span></span>

<a id="Ex1Task2"></a>

<a id="Task_2_-_Injecting_a_Code_Interceptor_into_the_Store_Controller_Class"></a>
#### <a name="task-2---injecting-a-code-interceptor-into-the-store-controller-class"></a><span data-ttu-id="b2aff-189">Zadanie 2 — wstrzykiwania interceptora kodu do klasy kontrolera magazynu</span><span class="sxs-lookup"><span data-stu-id="b2aff-189">Task 2 - Injecting a Code Interceptor into the Store Controller Class</span></span>

<span data-ttu-id="b2aff-190">To zadanie spowoduje dodanie niestandardowego filtru przez wstrzykiwanie go do wszystkich klas kontrolera i akcji kontrolera, które będą rejestrowane.</span><span class="sxs-lookup"><span data-stu-id="b2aff-190">In this task you will add the custom filter by injecting it to all controller classes and controller actions that will be logged.</span></span> <span data-ttu-id="b2aff-191">Na potrzeby tego ćwiczenia klasy kontrolera magazynu ma dziennika.</span><span class="sxs-lookup"><span data-stu-id="b2aff-191">For the purpose of this exercise, the Store Controller class will have a log.</span></span>

<span data-ttu-id="b2aff-192">Metoda **OnActionExecuting** z **ActionLogFilterAttribute** niestandardowy filtr jest uruchamiany, gdy jest wywoływana wprowadzony element.</span><span class="sxs-lookup"><span data-stu-id="b2aff-192">The method **OnActionExecuting** from **ActionLogFilterAttribute** custom filter runs when an injected element is called.</span></span>

<span data-ttu-id="b2aff-193">Istnieje również możliwość przechwycenia metody określonego kontrolera.</span><span class="sxs-lookup"><span data-stu-id="b2aff-193">It is also possible to intercept a specific controller method.</span></span>

1. <span data-ttu-id="b2aff-194">Otwórz **StoreController** w **MvcMusicStore\Controllers** i Dodaj odwołanie do **filtry** przestrzeni nazw:</span><span class="sxs-lookup"><span data-stu-id="b2aff-194">Open the **StoreController** at **MvcMusicStore\Controllers** and add a reference to the **Filters** namespace:</span></span>

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample4.cs)]
2. <span data-ttu-id="b2aff-195">Wstaw niestandardowy filtr **CustomActionFilter** do **StoreController** klasy, dodając **[CustomActionFilter]** atrybutu przed deklaracją klasy.</span><span class="sxs-lookup"><span data-stu-id="b2aff-195">Inject the custom filter **CustomActionFilter** into **StoreController** class by adding **[CustomActionFilter]** attribute before the class declaration.</span></span>

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample5.cs)]

   > [!NOTE]
   > <span data-ttu-id="b2aff-196">Gdy filtr jest wstrzykiwane do klasy kontrolera, również są wprowadzić swoje działania.</span><span class="sxs-lookup"><span data-stu-id="b2aff-196">When a filter is injected into a controller class, all its actions are also injected.</span></span> <span data-ttu-id="b2aff-197">Jeśli chcesz zastosować filtr tylko dla działań, konieczne będzie przeprowadzenie wstrzyknąć **[CustomActionFilter]** do każdej z nich z nich:</span><span class="sxs-lookup"><span data-stu-id="b2aff-197">If you would like to apply the filter only for a set of actions, you would have to inject **[CustomActionFilter]** to each one of them:</span></span>
   > 
   > [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample6.cs)]

<a id="Ex1Task3"></a>

<a id="Task_3_-_Running_the_Application"></a>
#### <a name="task-3---running-the-application"></a><span data-ttu-id="b2aff-198">Zadanie 3 — uruchamianie aplikacji</span><span class="sxs-lookup"><span data-stu-id="b2aff-198">Task 3 - Running the Application</span></span>

<span data-ttu-id="b2aff-199">W tym zadaniu zostanie testu działa filtr rejestrowania.</span><span class="sxs-lookup"><span data-stu-id="b2aff-199">In this task, you will test that the logging filter is working.</span></span> <span data-ttu-id="b2aff-200">Będzie uruchomić aplikację i przejdź do sklepu, a następnie będzie sprawdzać rejestrowane działania.</span><span class="sxs-lookup"><span data-stu-id="b2aff-200">You will start the application and visit the store, and then you will check logged activities.</span></span>

1. <span data-ttu-id="b2aff-201">Naciśnij klawisz **F5** do uruchomienia aplikacji.</span><span class="sxs-lookup"><span data-stu-id="b2aff-201">Press **F5** to run the application.</span></span>
2. <span data-ttu-id="b2aff-202">Przejdź do **/ActionLog** aby zobaczyć stan początkowy widok dziennika:</span><span class="sxs-lookup"><span data-stu-id="b2aff-202">Browse to **/ActionLog** to see log view initial state:</span></span>

    <span data-ttu-id="b2aff-203">![Rejestrowanie śledzenia stanu przed działania strony](aspnet-mvc-4-custom-action-filters/_static/image3.png "rejestrowania śledzenia stanu przed działania strony")</span><span class="sxs-lookup"><span data-stu-id="b2aff-203">![Log tracker status before page activity](aspnet-mvc-4-custom-action-filters/_static/image3.png "Log tracker status before page activity")</span></span>

    <span data-ttu-id="b2aff-204">*Dziennik śledzenia stanu przed działania strony*</span><span class="sxs-lookup"><span data-stu-id="b2aff-204">*Log tracker status before page activity*</span></span>

   > [!NOTE]
   > <span data-ttu-id="b2aff-205">Domyślnie go będzie zawsze pokazuj elementu, który jest generowany podczas pobierania istniejących genres menu.</span><span class="sxs-lookup"><span data-stu-id="b2aff-205">By default, it will always show one item that is generated when retrieving the existing genres for the menu.</span></span>
   > 
   > <span data-ttu-id="b2aff-206">Dla uproszczenia możemy jest czyszczony **ActionLog** tabeli każdym uruchomieniu aplikacji, będzie wyświetlana tylko dzienniki weryfikacji każdego określonego zadania.</span><span class="sxs-lookup"><span data-stu-id="b2aff-206">For simplicity purposes we're cleaning up the **ActionLog** table each time the application runs so it will only show the logs of each particular task's verification.</span></span>
   > 
   > <span data-ttu-id="b2aff-207">Konieczne może być usuń poniższy kod z **sesji\_Start** — metoda (w **Global.asax** klasy), aby zapisać dziennik historyczny dla wszystkich akcji, które są wykonywane w magazynie Kontroler.</span><span class="sxs-lookup"><span data-stu-id="b2aff-207">You might need to remove the following code from the **Session\_Start** method (in the **Global.asax** class), in order to save an historical log for all the actions executed within the Store Controller.</span></span>
   > 
   > [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample7.cs)]
3. <span data-ttu-id="b2aff-208">Kliknij jeden z **Genres** z menu i wykonywania pewnych działań, takich jak przeglądanie dostępnych albumu.</span><span class="sxs-lookup"><span data-stu-id="b2aff-208">Click one of the **Genres** from the menu and perform some actions there, like browsing an available album.</span></span>
4. <span data-ttu-id="b2aff-209">Przejdź do **/ActionLog** i jeśli dziennik jest pusty naciśnij **F5** odświeżenie strony.</span><span class="sxs-lookup"><span data-stu-id="b2aff-209">Browse to **/ActionLog** and if the log is empty press **F5** to refresh the page.</span></span> <span data-ttu-id="b2aff-210">Sprawdź, czy zostały śledzone wizyty:</span><span class="sxs-lookup"><span data-stu-id="b2aff-210">Check that your visits were tracked:</span></span>

    <span data-ttu-id="b2aff-211">![Dziennik akcji z działaniem rejestrowane](aspnet-mvc-4-custom-action-filters/_static/image4.png "dziennika akcji z działaniem rejestrowane")</span><span class="sxs-lookup"><span data-stu-id="b2aff-211">![Action log with activity logged](aspnet-mvc-4-custom-action-filters/_static/image4.png "Action log with activity logged")</span></span>

    <span data-ttu-id="b2aff-212">*Dziennik akcji z działaniem rejestrowane*</span><span class="sxs-lookup"><span data-stu-id="b2aff-212">*Action log with activity logged*</span></span>

<a id="Exercise2"></a>

<a id="Exercise_2_Managing_Multiple_Action_Filters"></a>
### <a name="exercise-2-managing-multiple-action-filters"></a><span data-ttu-id="b2aff-213">Ćwiczenie 2: Zarządzanie wielu filtrów akcji</span><span class="sxs-lookup"><span data-stu-id="b2aff-213">Exercise 2: Managing Multiple Action Filters</span></span>

<span data-ttu-id="b2aff-214">W tym ćwiczeniu zostanie dodać drugi niestandardowy filtr akcji do klasy StoreController i zdefiniuj określonej kolejności, w której zostanie wykonana obu filtrów.</span><span class="sxs-lookup"><span data-stu-id="b2aff-214">In this exercise you will add a second Custom Action Filter to the StoreController class and define the specific order in which both filters will be executed.</span></span> <span data-ttu-id="b2aff-215">Następnie zostanie zaktualizuj kod, aby zarejestrować filtr globalny.</span><span class="sxs-lookup"><span data-stu-id="b2aff-215">Then you will update the code to register the filter Globally.</span></span>

<span data-ttu-id="b2aff-216">Istnieją różne opcje wziąć pod uwagę podczas definiowania kolejność wykonywania filtrów.</span><span class="sxs-lookup"><span data-stu-id="b2aff-216">There are different options to take into account when defining the Filters' execution order.</span></span> <span data-ttu-id="b2aff-217">Na przykład właściwość kolejności i zakres te filtry:</span><span class="sxs-lookup"><span data-stu-id="b2aff-217">For example, the Order property and the Filters' scope:</span></span>

<span data-ttu-id="b2aff-218">Można zdefiniować **zakres** dla każdego z filtrów, na przykład można zakresu wszystkie filtry akcji do uruchomienia w ramach **zakresu kontrolera**oraz wszystkie filtry autoryzacji do uruchamiania w **zakresie globalnym** .</span><span class="sxs-lookup"><span data-stu-id="b2aff-218">You can define a **Scope** for each of the Filters, for example, you could scope all the Action Filters to run within the **Controller Scope**, and all Authorization Filters to run in **Global scope**.</span></span> <span data-ttu-id="b2aff-219">Zakresy mają kolejność wykonywania zdefiniowane.</span><span class="sxs-lookup"><span data-stu-id="b2aff-219">The scopes have a defined execution order.</span></span>

<span data-ttu-id="b2aff-220">Ponadto każdy filtr akcji ma właściwość kolejność, która służy do określania kolejności wykonywania w zakresie filtru.</span><span class="sxs-lookup"><span data-stu-id="b2aff-220">Additionally, each action filter has an Order property which is used to determine the execution order in the scope of the filter.</span></span>

<span data-ttu-id="b2aff-221">Aby uzyskać więcej informacji na temat kolejność wykonywania filtrów akcji niestandardowej, odwiedź ten artykuł w witrynie MSDN: ([https://msdn.microsoft.com/library/dd381609(v=vs.98).aspx](https://msdn.microsoft.com/library/dd381609(v=vs.98).aspx)).</span><span class="sxs-lookup"><span data-stu-id="b2aff-221">For more information about Custom Action Filters execution order, please visit this MSDN article: ([https://msdn.microsoft.com/library/dd381609(v=vs.98).aspx](https://msdn.microsoft.com/library/dd381609(v=vs.98).aspx)).</span></span>

<a id="Ex2Task1"></a>

<a id="Task_1_Creating_a_new_Custom_Action_Filter"></a>
#### <a name="task-1-creating-a-new-custom-action-filter"></a><span data-ttu-id="b2aff-222">Zadanie 1: Tworzenie nowego niestandardowy filtr akcji</span><span class="sxs-lookup"><span data-stu-id="b2aff-222">Task 1: Creating a new Custom Action Filter</span></span>

<span data-ttu-id="b2aff-223">W ramach tego zadania spowoduje utworzenie nowej niestandardowy filtr akcji do dodania do klasy StoreController dowiedzieć, jak zarządzać kolejność wykonywania filtrów.</span><span class="sxs-lookup"><span data-stu-id="b2aff-223">In this task, you will create a new Custom Action Filter to inject into the StoreController class, learning how to manage the execution order of the filters.</span></span>

1. <span data-ttu-id="b2aff-224">Otwórz **rozpocząć** rozwiązania, znajdujących się na **\Source\Ex02-ManagingMultipleActionFilters\Begin** folderu.</span><span class="sxs-lookup"><span data-stu-id="b2aff-224">Open the **Begin** solution located at **\Source\Ex02-ManagingMultipleActionFilters\Begin** folder.</span></span> <span data-ttu-id="b2aff-225">W przeciwnym razie możesz nadal korzystać **zakończenia** uzyskane rozwiązanie, wykonując poprzednim ćwiczeniu.</span><span class="sxs-lookup"><span data-stu-id="b2aff-225">Otherwise, you might continue using the **End** solution obtained by completing the previous exercise.</span></span>

    1. <span data-ttu-id="b2aff-226">Po otwarciu dostarczonych **rozpocząć** rozwiązania, musisz pobrać niektórych brakujących pakietów NuGet aby kontynuować.</span><span class="sxs-lookup"><span data-stu-id="b2aff-226">If you opened the provided **Begin** solution, you will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="b2aff-227">Aby to zrobić, kliknij przycisk **projektu** menu i wybierz **Zarządzaj pakietami NuGet**.</span><span class="sxs-lookup"><span data-stu-id="b2aff-227">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
    2. <span data-ttu-id="b2aff-228">W **Zarządzaj pakietami NuGet** okna dialogowego, kliknij przycisk **przywrócić** celu pobieranie brakujących pakietów.</span><span class="sxs-lookup"><span data-stu-id="b2aff-228">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
    3. <span data-ttu-id="b2aff-229">Na koniec Skompiluj rozwiązanie, klikając **kompilacji** | **Kompiluj rozwiązanie**.</span><span class="sxs-lookup"><span data-stu-id="b2aff-229">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

        > [!NOTE]
        > <span data-ttu-id="b2aff-230">Jedną z zalet przy użyciu narzędzia NuGet jest, że nie masz do wysłania wszystkich bibliotek w projekcie, zmniejszenie jego rozmiar projektu.</span><span class="sxs-lookup"><span data-stu-id="b2aff-230">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="b2aff-231">Narzędzia Power NuGet określając wersje pakietów w pliku Packages.config, będzie można pobrać wymaganych bibliotek podczas pierwszego uruchomienia projektu.</span><span class="sxs-lookup"><span data-stu-id="b2aff-231">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="b2aff-232">Jest to, dlaczego konieczne będzie wykonanie tych kroków, po otwarciu istniejącego rozwiązania z tego laboratorium.</span><span class="sxs-lookup"><span data-stu-id="b2aff-232">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
        > 
        > <span data-ttu-id="b2aff-233">Aby uzyskać więcej informacji, zobacz ten artykuł: [ http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages ](http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages).</span><span class="sxs-lookup"><span data-stu-id="b2aff-233">For more information, see this article: [http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages](http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages).</span></span>
2. <span data-ttu-id="b2aff-234">Dodaj nową klasę C# do **filtry** folder i nadaj mu nazwę *MyNewCustomActionFilter.cs*</span><span class="sxs-lookup"><span data-stu-id="b2aff-234">Add a new C# class into the **Filters** folder and name it *MyNewCustomActionFilter.cs*</span></span>
3. <span data-ttu-id="b2aff-235">Otwórz **MyNewCustomActionFilter.cs** i Dodaj odwołanie do **System.Web.Mvc** i **MvcMusicStore.Models** przestrzeni nazw:</span><span class="sxs-lookup"><span data-stu-id="b2aff-235">Open **MyNewCustomActionFilter.cs** and add a reference to **System.Web.Mvc** and the **MvcMusicStore.Models** namespace:</span></span>

    <span data-ttu-id="b2aff-236">(Fragment - kodu *filtry akcji niestandardowych platformy ASP.NET MVC 4 - Ex2 MyNewCustomActionFilterNamespaces*)</span><span class="sxs-lookup"><span data-stu-id="b2aff-236">(Code Snippet - *ASP.NET MVC 4 Custom Action Filters - Ex2-MyNewCustomActionFilterNamespaces*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample8.cs)]
4. <span data-ttu-id="b2aff-237">Zastąp domyślną deklaracji klasy następującym kodem.</span><span class="sxs-lookup"><span data-stu-id="b2aff-237">Replace the default class declaration with the following code.</span></span>

    <span data-ttu-id="b2aff-238">(Fragment - kodu *filtry akcji niestandardowych platformy ASP.NET MVC 4 - Ex2 MyNewCustomActionFilterClass*)</span><span class="sxs-lookup"><span data-stu-id="b2aff-238">(Code Snippet - *ASP.NET MVC 4 Custom Action Filters - Ex2-MyNewCustomActionFilterClass*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample9.cs)]

    > [!NOTE]
    > <span data-ttu-id="b2aff-239">Ten niestandardowy filtr akcji jest niemal taki sam, niż ten, który został utworzony w poprzednim ćwiczeniu.</span><span class="sxs-lookup"><span data-stu-id="b2aff-239">This Custom Action Filter is almost the same than the one you created in the previous exercise.</span></span> <span data-ttu-id="b2aff-240">Główną różnicą jest to, że ma *&quot;rejestrowane przez&quot;* zaktualizowany o nazwie ta nowa klasa, aby zidentyfikować filtru upubliczniona atrybut zarejestrowany dziennika.</span><span class="sxs-lookup"><span data-stu-id="b2aff-240">The main difference is that it has the *&quot;Logged By&quot;* attribute updated with this new class' name to identify wich filter registered the log.</span></span>

<a id="Ex2Task2"></a>

<a id="Task_2_Injecting_a_new_Code_Interceptor_into_the_StoreController_Class"></a>
#### <a name="task-2-injecting-a-new-code-interceptor-into-the-storecontroller-class"></a><span data-ttu-id="b2aff-241">Zadanie 2: Wstrzykiwania nowe interceptora kodu do klasy StoreController</span><span class="sxs-lookup"><span data-stu-id="b2aff-241">Task 2: Injecting a new Code Interceptor into the StoreController Class</span></span>

<span data-ttu-id="b2aff-242">W tym zadaniu zostanie dodaje nowy filtr niestandardowy do klasy StoreController i uruchamianie rozwiązania, aby sprawdzić, jak obu filtrów współpracują ze sobą.</span><span class="sxs-lookup"><span data-stu-id="b2aff-242">In this task, you will add a new custom filter into the StoreController Class and run the solution to verify how both filters work together.</span></span>

1. <span data-ttu-id="b2aff-243">Otwórz **StoreController** klasa znajduje się w **MvcMusicStore\Controllers** i Wstaw nowy filtr niestandardowy **MyNewCustomActionFilter** do  **StoreController** klasy, jak przedstawiono w poniższym kodzie.</span><span class="sxs-lookup"><span data-stu-id="b2aff-243">Open the **StoreController** class located at **MvcMusicStore\Controllers** and inject the new custom filter **MyNewCustomActionFilter** into **StoreController** class like is shown in the following code.</span></span>

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample10.cs)]
2. <span data-ttu-id="b2aff-244">Teraz uruchom aplikację, aby zobaczyć, jak działają te dwa filtry akcji niestandardowej.</span><span class="sxs-lookup"><span data-stu-id="b2aff-244">Now, run the application in order to see how these two Custom Action Filters work.</span></span> <span data-ttu-id="b2aff-245">Aby to zrobić, naciśnij klawisz **F5** i poczekaj, aż uruchamiania aplikacji.</span><span class="sxs-lookup"><span data-stu-id="b2aff-245">To do this, press **F5** and wait until the application starts.</span></span>
3. <span data-ttu-id="b2aff-246">Przejdź do **/ActionLog** aby zobaczyć stan początkowy widok dziennika.</span><span class="sxs-lookup"><span data-stu-id="b2aff-246">Browse to **/ActionLog** to see log view initial state.</span></span>

    <span data-ttu-id="b2aff-247">![Rejestrowanie śledzenia stanu przed działania strony](aspnet-mvc-4-custom-action-filters/_static/image5.png "rejestrowania śledzenia stanu przed działania strony")</span><span class="sxs-lookup"><span data-stu-id="b2aff-247">![Log tracker status before page activity](aspnet-mvc-4-custom-action-filters/_static/image5.png "Log tracker status before page activity")</span></span>

    <span data-ttu-id="b2aff-248">*Dziennik śledzenia stanu przed działania strony*</span><span class="sxs-lookup"><span data-stu-id="b2aff-248">*Log tracker status before page activity*</span></span>
4. <span data-ttu-id="b2aff-249">Kliknij jeden z **Genres** z menu i wykonywania pewnych działań, takich jak przeglądanie dostępnych albumu.</span><span class="sxs-lookup"><span data-stu-id="b2aff-249">Click one of the **Genres** from the menu and perform some actions there, like browsing an available album.</span></span>
5. <span data-ttu-id="b2aff-250">Sprawdź, czy ten czas; wizyty zostały śledzone dwa razy: po każdej filtry akcji niestandardowych dodaniu w **StorageController** klasy.</span><span class="sxs-lookup"><span data-stu-id="b2aff-250">Check that this time; your visits were tracked twice: once for each of the Custom Action Filters you added in the **StorageController** class.</span></span>

    <span data-ttu-id="b2aff-251">![Dziennik akcji z działaniem rejestrowane](aspnet-mvc-4-custom-action-filters/_static/image6.png "dziennika akcji z działaniem rejestrowane")</span><span class="sxs-lookup"><span data-stu-id="b2aff-251">![Action log with activity logged](aspnet-mvc-4-custom-action-filters/_static/image6.png "Action log with activity logged")</span></span>

    <span data-ttu-id="b2aff-252">*Dziennik akcji z działaniem rejestrowane*</span><span class="sxs-lookup"><span data-stu-id="b2aff-252">*Action log with activity logged*</span></span>
6. <span data-ttu-id="b2aff-253">Zamknij przeglądarkę.</span><span class="sxs-lookup"><span data-stu-id="b2aff-253">Close the browser.</span></span>

<a id="Ex2Task3"></a>

<a id="Task_3_Managing_Filter_Ordering"></a>
#### <a name="task-3-managing-filter-ordering"></a><span data-ttu-id="b2aff-254">Zadanie 3: Zarządzanie kolejności filtru</span><span class="sxs-lookup"><span data-stu-id="b2aff-254">Task 3: Managing Filter Ordering</span></span>

<span data-ttu-id="b2aff-255">W tym zadaniu dowiesz się, jak zarządzać kolejność wykonywania filtrów za pomocą właściwości kolejności.</span><span class="sxs-lookup"><span data-stu-id="b2aff-255">In this task, you will learn how to manage the filters' execution order by using the Order propery.</span></span>

1. <span data-ttu-id="b2aff-256">Otwórz **StoreController** klasa znajduje się w **MvcMusicStore\Controllers** i określ **kolejności** właściwości w obu filtrów, takich jak pokazano poniżej.</span><span class="sxs-lookup"><span data-stu-id="b2aff-256">Open the **StoreController** class located at **MvcMusicStore\Controllers** and specify the **Order** property in both filters like shown below.</span></span>

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample11.cs)]
2. <span data-ttu-id="b2aff-257">Sprawdź teraz, jak filtry są wykonywane w zależności od wartości jego właściwość kolejności.</span><span class="sxs-lookup"><span data-stu-id="b2aff-257">Now, verify how the filters are executed depending on its Order property's value.</span></span> <span data-ttu-id="b2aff-258">Zostanie stwierdzisz, że filtr mający najmniejszą wartość kolejności (**CustomActionFilter**) jest pierwszą, która jest wykonywana.</span><span class="sxs-lookup"><span data-stu-id="b2aff-258">You will find that the filter with the smallest Order value (**CustomActionFilter**) is the first one that is executed.</span></span> <span data-ttu-id="b2aff-259">Naciśnij klawisz **F5** i poczekaj, aż uruchamiania aplikacji.</span><span class="sxs-lookup"><span data-stu-id="b2aff-259">Press **F5** and wait until the application starts.</span></span>
3. <span data-ttu-id="b2aff-260">Przejdź do **/ActionLog** aby zobaczyć stan początkowy widok dziennika.</span><span class="sxs-lookup"><span data-stu-id="b2aff-260">Browse to **/ActionLog** to see log view initial state.</span></span>

    <span data-ttu-id="b2aff-261">![Rejestrowanie śledzenia stanu przed działania strony](aspnet-mvc-4-custom-action-filters/_static/image7.png "rejestrowania śledzenia stanu przed działania strony")</span><span class="sxs-lookup"><span data-stu-id="b2aff-261">![Log tracker status before page activity](aspnet-mvc-4-custom-action-filters/_static/image7.png "Log tracker status before page activity")</span></span>

    <span data-ttu-id="b2aff-262">*Dziennik śledzenia stanu przed działania strony*</span><span class="sxs-lookup"><span data-stu-id="b2aff-262">*Log tracker status before page activity*</span></span>
4. <span data-ttu-id="b2aff-263">Kliknij jeden z **Genres** z menu i wykonywania pewnych działań, takich jak przeglądanie dostępnych albumu.</span><span class="sxs-lookup"><span data-stu-id="b2aff-263">Click one of the **Genres** from the menu and perform some actions there, like browsing an available album.</span></span>
5. <span data-ttu-id="b2aff-264">Sprawdź, czy ten czas wizyty zostały śledzone uporządkowanych według wartości kolejności filtrów: **CustomActionFilter** dzienniki pierwszy.</span><span class="sxs-lookup"><span data-stu-id="b2aff-264">Check that this time, your visits were tracked ordered by the filters' Order value: **CustomActionFilter** logs' first.</span></span>

    <span data-ttu-id="b2aff-265">![Dziennik akcji z działaniem rejestrowane](aspnet-mvc-4-custom-action-filters/_static/image8.png "dziennika akcji z działaniem rejestrowane")</span><span class="sxs-lookup"><span data-stu-id="b2aff-265">![Action log with activity logged](aspnet-mvc-4-custom-action-filters/_static/image8.png "Action log with activity logged")</span></span>

    <span data-ttu-id="b2aff-266">*Dziennik akcji z działaniem rejestrowane*</span><span class="sxs-lookup"><span data-stu-id="b2aff-266">*Action log with activity logged*</span></span>
6. <span data-ttu-id="b2aff-267">Zostanie teraz zaktualizować wartość kolejności filtrów i sprawdź, jak zmienia kolejność rejestrowania.</span><span class="sxs-lookup"><span data-stu-id="b2aff-267">Now, you will update the Filters' order value and verify how the logging order changes.</span></span> <span data-ttu-id="b2aff-268">W **StoreController** klasy, zaktualizuj wartość kolejności filtrów, jak pokazano poniżej.</span><span class="sxs-lookup"><span data-stu-id="b2aff-268">In the **StoreController** class, update the Filters' Order value like shown below.</span></span>

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample12.cs)]
7. <span data-ttu-id="b2aff-269">Uruchom aplikację ponownie, naciskając klawisz **F5**.</span><span class="sxs-lookup"><span data-stu-id="b2aff-269">Run the application again by pressing **F5**.</span></span>
8. <span data-ttu-id="b2aff-270">Kliknij jeden z **Genres** z menu i wykonywania pewnych działań, takich jak przeglądanie dostępnych albumu.</span><span class="sxs-lookup"><span data-stu-id="b2aff-270">Click one of the **Genres** from the menu and perform some actions there, like browsing an available album.</span></span>
9. <span data-ttu-id="b2aff-271">Sprawdź, czy ten czas dzienniki utworzony przez **MyNewCustomActionFilter** filtru występuje jako pierwszy.</span><span class="sxs-lookup"><span data-stu-id="b2aff-271">Check that this time, the logs created by **MyNewCustomActionFilter** filter appears first.</span></span>

    <span data-ttu-id="b2aff-272">![Dziennik akcji z działaniem rejestrowane](aspnet-mvc-4-custom-action-filters/_static/image9.png "dziennika akcji z działaniem rejestrowane")</span><span class="sxs-lookup"><span data-stu-id="b2aff-272">![Action log with activity logged](aspnet-mvc-4-custom-action-filters/_static/image9.png "Action log with activity logged")</span></span>

    <span data-ttu-id="b2aff-273">*Dziennik akcji z działaniem rejestrowane*</span><span class="sxs-lookup"><span data-stu-id="b2aff-273">*Action log with activity logged*</span></span>

<a id="Ex2Task4"></a>

<a id="Task_4_Registering_Filters_Globally"></a>
#### <a name="task-4-registering-filters-globally"></a><span data-ttu-id="b2aff-274">Zadanie 4: Rejestrowanie globalnie filtry.</span><span class="sxs-lookup"><span data-stu-id="b2aff-274">Task 4: Registering Filters Globally</span></span>

<span data-ttu-id="b2aff-275">To zadanie zaktualizuje rozwiązanie, aby zarejestrować nowego filtru (**MyNewCustomActionFilter**) jako filtr globalny.</span><span class="sxs-lookup"><span data-stu-id="b2aff-275">In this task, you will update the solution to register the new filter (**MyNewCustomActionFilter**) as a global filter.</span></span> <span data-ttu-id="b2aff-276">W ten sposób zostanie wywołane przez wszystkie przeprowadzane akcje w aplikacji i nie tylko StoreController widocznych w poprzednim zadaniu.</span><span class="sxs-lookup"><span data-stu-id="b2aff-276">By doing this, it will be triggered by all the actions perfomed in the application and not only in the StoreController ones as in the previous task.</span></span>

1. <span data-ttu-id="b2aff-277">W **StoreController** klasy, Usuń **[MyNewCustomActionFilter]** atrybutu, a właściwość kolejności z **[CustomActionFilter]**.</span><span class="sxs-lookup"><span data-stu-id="b2aff-277">In **StoreController** class, remove **[MyNewCustomActionFilter]** attribute and the order property from **[CustomActionFilter]**.</span></span> <span data-ttu-id="b2aff-278">Jego wygląd powinien być podobne do poniższych:</span><span class="sxs-lookup"><span data-stu-id="b2aff-278">It should look like the following:</span></span>

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample13.cs)]
2. <span data-ttu-id="b2aff-279">Otwórz **Global.asax** plików i Znajdź **aplikacji\_Start** metody.</span><span class="sxs-lookup"><span data-stu-id="b2aff-279">Open **Global.asax** file and locate the **Application\_Start** method.</span></span> <span data-ttu-id="b2aff-280">Powiadomienie, że każdy czas uruchamiania aplikacji go rejestruje filtry globalne wywołując **RegisterGlobalFilters** metodę **FilterConfig** klasy.</span><span class="sxs-lookup"><span data-stu-id="b2aff-280">Notice that each time the application starts it is registering the global filters by calling **RegisterGlobalFilters** method within **FilterConfig** class.</span></span>

    <span data-ttu-id="b2aff-281">![Rejestrowanie w pliku Global.asax filtry globalne](aspnet-mvc-4-custom-action-filters/_static/image10.png "rejestrowanie filtry globalne w pliku Global.asax")</span><span class="sxs-lookup"><span data-stu-id="b2aff-281">![Registering Global Filters in Global.asax](aspnet-mvc-4-custom-action-filters/_static/image10.png "Registering Global Filters in Global.asax")</span></span>

    <span data-ttu-id="b2aff-282">*Rejestrowanie w pliku Global.asax filtry globalne*</span><span class="sxs-lookup"><span data-stu-id="b2aff-282">*Registering Global Filters in Global.asax*</span></span>
3. <span data-ttu-id="b2aff-283">Otwórz **FilterConfig.cs** pliku w ramach **aplikacji\_Start** folderu.</span><span class="sxs-lookup"><span data-stu-id="b2aff-283">Open **FilterConfig.cs** file within **App\_Start** folder.</span></span>
4. <span data-ttu-id="b2aff-284">Dodaj odwołanie do przy użyciu System.Web.Mvc; przy użyciu MvcMusicStore.Filters; przestrzeń nazw.</span><span class="sxs-lookup"><span data-stu-id="b2aff-284">Add a reference to using System.Web.Mvc; using MvcMusicStore.Filters; namespace.</span></span>

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample14.cs)]
5. <span data-ttu-id="b2aff-285">Aktualizacja **RegisterGlobalFilters** metody dodawania filtru niestandardowego.</span><span class="sxs-lookup"><span data-stu-id="b2aff-285">Update **RegisterGlobalFilters** method adding your custom filter.</span></span> <span data-ttu-id="b2aff-286">Aby to zrobić, Dodaj wyróżniony kod:</span><span class="sxs-lookup"><span data-stu-id="b2aff-286">To do this, add the highlighted code:</span></span>

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample15.cs)]
6. <span data-ttu-id="b2aff-287">Uruchom aplikację, naciskając klawisz **F5**.</span><span class="sxs-lookup"><span data-stu-id="b2aff-287">Run the application by pressing **F5**.</span></span>
7. <span data-ttu-id="b2aff-288">Kliknij jeden z **Genres** z menu i wykonywania pewnych działań, takich jak przeglądanie dostępnych albumu.</span><span class="sxs-lookup"><span data-stu-id="b2aff-288">Click one of the **Genres** from the menu and perform some actions there, like browsing an available album.</span></span>
8. <span data-ttu-id="b2aff-289">Sprawdź, która teraz **[MyNewCustomActionFilter]** jest trwa zbyt wprowadzonym w HomeController i ActionLogController.</span><span class="sxs-lookup"><span data-stu-id="b2aff-289">Check that now **[MyNewCustomActionFilter]** is being injected in HomeController and ActionLogController too.</span></span>

    <span data-ttu-id="b2aff-290">![Dziennik akcji z działaniem rejestrowane](aspnet-mvc-4-custom-action-filters/_static/image11.png "dziennika akcji z działaniem rejestrowane")</span><span class="sxs-lookup"><span data-stu-id="b2aff-290">![Action log with activity logged](aspnet-mvc-4-custom-action-filters/_static/image11.png "Action log with activity logged")</span></span>

    <span data-ttu-id="b2aff-291">*Dziennik akcji z globalnego zarejestrowanego działania*</span><span class="sxs-lookup"><span data-stu-id="b2aff-291">*Action log with global activity logged*</span></span>

> [!NOTE]
> <span data-ttu-id="b2aff-292">Ponadto można wdrożyć tę aplikację systemu Windows Azure Web Sites następujących [dodatek B: publikowania aplikacji ASP.NET MVC 4 przy użyciu narzędzia Web Deploy](#AppendixB).</span><span class="sxs-lookup"><span data-stu-id="b2aff-292">Additionally, you can deploy this application to Windows Azure Web Sites following [Appendix B: Publishing an ASP.NET MVC 4 Application using Web Deploy](#AppendixB).</span></span>


* * *

<a id="Summary"></a>

<a id="Summary"></a>
## <a name="summary"></a><span data-ttu-id="b2aff-293">Podsumowanie</span><span class="sxs-lookup"><span data-stu-id="b2aff-293">Summary</span></span>

<span data-ttu-id="b2aff-294">Wykonując tego laboratorium Hands-On uzyskanych jak rozszerzyć filtr akcji do wykonania akcji niestandardowych.</span><span class="sxs-lookup"><span data-stu-id="b2aff-294">By completing this Hands-On Lab you have learned how to extend an action filter to execute custom actions.</span></span> <span data-ttu-id="b2aff-295">Ma również przedstawiono sposób wstrzyknąć filtr, który ma kontrolerów strony.</span><span class="sxs-lookup"><span data-stu-id="b2aff-295">You have also learned how to inject any filter to your page controllers.</span></span> <span data-ttu-id="b2aff-296">Zostały użyte następujące kwestie:</span><span class="sxs-lookup"><span data-stu-id="b2aff-296">The following concepts were used:</span></span>

- <span data-ttu-id="b2aff-297">Jak utworzyć filtry akcji niestandardowych z klasy ASP.NET MVC ActionFilterAttribute</span><span class="sxs-lookup"><span data-stu-id="b2aff-297">How to create Custom Action filters with the ASP.NET MVC ActionFilterAttribute class</span></span>
- <span data-ttu-id="b2aff-298">Jak można wstawić filtrów do kontrolerów ASP.NET MVC</span><span class="sxs-lookup"><span data-stu-id="b2aff-298">How to inject filters into ASP.NET MVC controllers</span></span>
- <span data-ttu-id="b2aff-299">Jak zarządzać filtru kolejność przy użyciu właściwości Order</span><span class="sxs-lookup"><span data-stu-id="b2aff-299">How to manage filter ordering using the Order property</span></span>
- <span data-ttu-id="b2aff-300">Jak zarejestrować filtry globalny</span><span class="sxs-lookup"><span data-stu-id="b2aff-300">How to register filters globally</span></span>

<a id="AppendixA"></a>

<a id="Appendix_A_Installing_Visual_Studio_Express_2012_for_Web"></a>
## <a name="appendix-a-installing-visual-studio-express-2012-for-web"></a><span data-ttu-id="b2aff-301">Dodatek A: Instalowanie programu Visual Studio Express 2012 for Web</span><span class="sxs-lookup"><span data-stu-id="b2aff-301">Appendix A: Installing Visual Studio Express 2012 for Web</span></span>

<span data-ttu-id="b2aff-302">Można zainstalować **Microsoft Visual Studio Express 2012 for Web** lub innym &quot;Express&quot; przy użyciu wersji **[Instalatora platformy sieci Web firmy Microsoft](https://www.microsoft.com/web/downloads/platform.aspx)**.</span><span class="sxs-lookup"><span data-stu-id="b2aff-302">You can install **Microsoft Visual Studio Express 2012 for Web** or another &quot;Express&quot; version using the **[Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)**.</span></span> <span data-ttu-id="b2aff-303">Poniższe instrukcje przedstawiono czynności wymagane do zainstalowania *programu Visual studio Express 2012 for Web* przy użyciu *Instalatora platformy sieci Web firmy Microsoft*.</span><span class="sxs-lookup"><span data-stu-id="b2aff-303">The following instructions guide you through the steps required to install *Visual studio Express 2012 for Web* using *Microsoft Web Platform Installer*.</span></span>

1. <span data-ttu-id="b2aff-304">Przejdź do [ [ https://go.microsoft.com/? linkid = 9810169](https://go.microsoft.com/?linkid=9810169)](https://go.microsoft.com/?linkid=9810169).</span><span class="sxs-lookup"><span data-stu-id="b2aff-304">Go to [[https://go.microsoft.com/? linkid=9810169](https://go.microsoft.com/?linkid=9810169)](https://go.microsoft.com/?linkid=9810169).</span></span> <span data-ttu-id="b2aff-305">Alternatywnie, jeśli została już zainstalowana Instalatora platformy sieci Web, można otworzyć go i Wyszukaj produkt &quot; <em>programu Visual Studio Express 2012 for Web z zestawem Windows Azure SDK</em>&quot;.</span><span class="sxs-lookup"><span data-stu-id="b2aff-305">Alternatively, if you already have installed Web Platform Installer, you can open it and search for the product &quot;<em>Visual Studio Express 2012 for Web with Windows Azure SDK</em>&quot;.</span></span>
2. <span data-ttu-id="b2aff-306">Polecenie **teraz zainstalować**.</span><span class="sxs-lookup"><span data-stu-id="b2aff-306">Click on **Install Now**.</span></span> <span data-ttu-id="b2aff-307">Jeśli nie masz **Instalatora platformy sieci Web** nastąpi przekierowanie do pobrania i zainstalowania go najpierw.</span><span class="sxs-lookup"><span data-stu-id="b2aff-307">If you do not have **Web Platform Installer** you will be redirected to download and install it first.</span></span>
3. <span data-ttu-id="b2aff-308">Raz **Instalatora platformy sieci Web** jest otwarty, kliknij przycisk **zainstalować** można uruchomić Instalatora.</span><span class="sxs-lookup"><span data-stu-id="b2aff-308">Once **Web Platform Installer** is open, click **Install** to start the setup.</span></span>

    <span data-ttu-id="b2aff-309">![Instalowanie programu Visual Studio Express](aspnet-mvc-4-custom-action-filters/_static/image12.png "instalacji programu Visual Studio Express")</span><span class="sxs-lookup"><span data-stu-id="b2aff-309">![Install Visual Studio Express](aspnet-mvc-4-custom-action-filters/_static/image12.png "Install Visual Studio Express")</span></span>

    <span data-ttu-id="b2aff-310">*Instalowanie programu Visual Studio Express*</span><span class="sxs-lookup"><span data-stu-id="b2aff-310">*Install Visual Studio Express*</span></span>
4. <span data-ttu-id="b2aff-311">Odczytywanie wszystkich produktów licencji i warunków, a następnie kliknij przycisk **akceptuję** aby kontynuować.</span><span class="sxs-lookup"><span data-stu-id="b2aff-311">Read all the products' licenses and terms and click **I Accept** to continue.</span></span>

    ![Akceptowanie umowy licencyjnej](aspnet-mvc-4-custom-action-filters/_static/image13.png)

    <span data-ttu-id="b2aff-313">*Akceptowanie umowy licencyjnej*</span><span class="sxs-lookup"><span data-stu-id="b2aff-313">*Accepting the license terms*</span></span>
5. <span data-ttu-id="b2aff-314">Poczekaj na zakończenie procesu pobierania i instalacji.</span><span class="sxs-lookup"><span data-stu-id="b2aff-314">Wait until the downloading and installation process completes.</span></span>

    ![Postęp instalacji](aspnet-mvc-4-custom-action-filters/_static/image14.png)

    <span data-ttu-id="b2aff-316">*Postęp instalacji*</span><span class="sxs-lookup"><span data-stu-id="b2aff-316">*Installation progress*</span></span>
6. <span data-ttu-id="b2aff-317">Po zakończeniu instalacji kliknij przycisk **Zakończ**.</span><span class="sxs-lookup"><span data-stu-id="b2aff-317">When the installation completes, click **Finish**.</span></span>

    ![Instalacja została zakończona](aspnet-mvc-4-custom-action-filters/_static/image15.png)

    <span data-ttu-id="b2aff-319">*Instalacja została zakończona*</span><span class="sxs-lookup"><span data-stu-id="b2aff-319">*Installation completed*</span></span>
7. <span data-ttu-id="b2aff-320">Kliknij przycisk **zakończenia** aby zamknąć Instalatora platformy sieci Web.</span><span class="sxs-lookup"><span data-stu-id="b2aff-320">Click **Exit** to close Web Platform Installer.</span></span>
8. <span data-ttu-id="b2aff-321">Aby otworzyć program Visual Studio Express for Web, przejdź do **Start** ekranu i zacznij pisać &quot; **VS Express**&quot;, następnie kliknij polecenie **VS Express for Web** Kafelek.</span><span class="sxs-lookup"><span data-stu-id="b2aff-321">To open Visual Studio Express for Web, go to the **Start** screen and start writing &quot;**VS Express**&quot;, then click on the **VS Express for Web** tile.</span></span>

    ![VS Express for Web kafelka](aspnet-mvc-4-custom-action-filters/_static/image16.png)

    <span data-ttu-id="b2aff-323">*VS Express for Web kafelka*</span><span class="sxs-lookup"><span data-stu-id="b2aff-323">*VS Express for Web tile*</span></span>

<a id="AppendixB"></a>

<a id="Appendix_B_Publishing_an_ASPNET_MVC_4_Application_using_Web_Deploy"></a>
## <a name="appendix-b-publishing-an-aspnet-mvc-4-application-using-web-deploy"></a><span data-ttu-id="b2aff-324">Dodatek B: publikowania aplikacji ASP.NET MVC 4 przy użyciu narzędzia Web Deploy</span><span class="sxs-lookup"><span data-stu-id="b2aff-324">Appendix B: Publishing an ASP.NET MVC 4 Application using Web Deploy</span></span>

<span data-ttu-id="b2aff-325">Ten dodatek opisano sposób tworzenia nowej witryny sieci web z portalu zarządzania pakietu Windows Azure i publikowanie aplikacji, uzyskane wykonując laboratorium, korzystając z funkcji publikowania narzędzia Web Deploy dostarczane przez Windows Azure.</span><span class="sxs-lookup"><span data-stu-id="b2aff-325">This appendix will show you how to create a new web site from the Windows Azure Management Portal and publish the application you obtained by following the lab, taking advantage of the Web Deploy publishing feature provided by Windows Azure.</span></span>

<a id="ApxBTask1"></a>

<a id="Task_1_-_Creating_a_New_Web_Site_from_the_Windows_Azure_Portal"></a>
#### <a name="task-1---creating-a-new-web-site-from-the-windows-azure-portal"></a><span data-ttu-id="b2aff-326">Zadanie 1 — Tworzenie nowej witryny sieci Web systemu Windows portalu Azure</span><span class="sxs-lookup"><span data-stu-id="b2aff-326">Task 1 - Creating a New Web Site from the Windows Azure Portal</span></span>

1. <span data-ttu-id="b2aff-327">Przejdź do [portalu zarządzania pakietu Windows Azure](https://manage.windowsazure.com/) i zaloguj się przy użyciu poświadczeń Microsoft skojarzonych z Twoją subskrypcją.</span><span class="sxs-lookup"><span data-stu-id="b2aff-327">Go to the [Windows Azure Management Portal](https://manage.windowsazure.com/) and sign in using the Microsoft credentials associated with your subscription.</span></span>

    > [!NOTE]
    > <span data-ttu-id="b2aff-328">Z systemu Windows Azure można udostępniać 10 witryn sieci Web platformy ASP.NET bezpłatnie i następnie Skaluj w miarę zwiększania się ruchu.</span><span class="sxs-lookup"><span data-stu-id="b2aff-328">With Windows Azure you can host 10 ASP.NET Web Sites for free and then scale as your traffic grows.</span></span> <span data-ttu-id="b2aff-329">Możesz utworzyć konto [tutaj](http://aka.ms/aspnet-hol-azure).</span><span class="sxs-lookup"><span data-stu-id="b2aff-329">You can sign up [here](http://aka.ms/aspnet-hol-azure).</span></span>

    <span data-ttu-id="b2aff-330">![Zaloguj się do portalu Windows Azure](aspnet-mvc-4-custom-action-filters/_static/image17.png "Zaloguj się do portalu Windows Azure")</span><span class="sxs-lookup"><span data-stu-id="b2aff-330">![Log on to Windows Azure portal](aspnet-mvc-4-custom-action-filters/_static/image17.png "Log on to Windows Azure portal")</span></span>

    <span data-ttu-id="b2aff-331">*Zaloguj się do portalu zarządzania platformy Azure z systemem Windows*</span><span class="sxs-lookup"><span data-stu-id="b2aff-331">*Log on to Windows Azure Management Portal*</span></span>
2. <span data-ttu-id="b2aff-332">Kliknij przycisk **nowy** na pasku poleceń.</span><span class="sxs-lookup"><span data-stu-id="b2aff-332">Click **New** on the command bar.</span></span>

    <span data-ttu-id="b2aff-333">![Tworzenie nowej witryny sieci Web](aspnet-mvc-4-custom-action-filters/_static/image18.png "tworzenia nowej witryny sieci Web")</span><span class="sxs-lookup"><span data-stu-id="b2aff-333">![Creating a new Web Site](aspnet-mvc-4-custom-action-filters/_static/image18.png "Creating a new Web Site")</span></span>

    <span data-ttu-id="b2aff-334">*Tworzenie nowej witryny sieci Web*</span><span class="sxs-lookup"><span data-stu-id="b2aff-334">*Creating a new Web Site*</span></span>
3. <span data-ttu-id="b2aff-335">Kliknij przycisk **obliczeniowe** | **witryny sieci Web**.</span><span class="sxs-lookup"><span data-stu-id="b2aff-335">Click **Compute** | **Web Site**.</span></span> <span data-ttu-id="b2aff-336">Następnie wybierz **szybkie tworzenie** opcji.</span><span class="sxs-lookup"><span data-stu-id="b2aff-336">Then select **Quick Create** option.</span></span> <span data-ttu-id="b2aff-337">Podaj dostępny adres URL dla nowej witryny sieci web, a następnie kliknij przycisk **tworzenie witryny sieci Web**.</span><span class="sxs-lookup"><span data-stu-id="b2aff-337">Provide an available URL for the new web site and click **Create Web Site**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="b2aff-338">Witryny sieci Web systemu Windows Azure jest hostem dla aplikacji sieci web w chmurze, które można kontrolować i zarządzanie nimi.</span><span class="sxs-lookup"><span data-stu-id="b2aff-338">A Windows Azure Web Site is the host for a web application running in the cloud that you can control and manage.</span></span> <span data-ttu-id="b2aff-339">Opcja szybkie tworzenie umożliwia wdrażanie ukończonej aplikacji sieci web do systemu Windows Azure witryny internetowej z spoza portalu.</span><span class="sxs-lookup"><span data-stu-id="b2aff-339">The Quick Create option allows you to deploy a completed web application to the Windows Azure Web Site from outside the portal.</span></span> <span data-ttu-id="b2aff-340">Nie obejmuje kroki konfigurowania bazy danych.</span><span class="sxs-lookup"><span data-stu-id="b2aff-340">It does not include steps for setting up a database.</span></span>

    <span data-ttu-id="b2aff-341">![Tworzenie nowej witryny sieci Web przy użyciu szybkie tworzenie](aspnet-mvc-4-custom-action-filters/_static/image19.png "tworzenia nowej witryny sieci Web przy użyciu szybkie tworzenie")</span><span class="sxs-lookup"><span data-stu-id="b2aff-341">![Creating a new Web Site using Quick Create](aspnet-mvc-4-custom-action-filters/_static/image19.png "Creating a new Web Site using Quick Create")</span></span>

    <span data-ttu-id="b2aff-342">*Tworzenie nowej witryny sieci Web przy użyciu szybkie tworzenie*</span><span class="sxs-lookup"><span data-stu-id="b2aff-342">*Creating a new Web Site using Quick Create*</span></span>
4. <span data-ttu-id="b2aff-343">Poczekaj na nowe **witryny sieci Web** jest tworzony.</span><span class="sxs-lookup"><span data-stu-id="b2aff-343">Wait until the new **Web Site** is created.</span></span>
5. <span data-ttu-id="b2aff-344">Po utworzeniu witryny sieci Web kliknij łącze w obszarze **adres URL** kolumny.</span><span class="sxs-lookup"><span data-stu-id="b2aff-344">Once the Web Site is created click the link under the **URL** column.</span></span> <span data-ttu-id="b2aff-345">Sprawdź, czy działa nowej witryny sieci Web.</span><span class="sxs-lookup"><span data-stu-id="b2aff-345">Check that the new Web Site is working.</span></span>

    <span data-ttu-id="b2aff-346">![Przeglądanie do nowej witryny sieci web](aspnet-mvc-4-custom-action-filters/_static/image20.png "przeglądanie do nowej witryny sieci web")</span><span class="sxs-lookup"><span data-stu-id="b2aff-346">![Browsing to the new web site](aspnet-mvc-4-custom-action-filters/_static/image20.png "Browsing to the new web site")</span></span>

    <span data-ttu-id="b2aff-347">*Przeglądanie do nowej witryny sieci web*</span><span class="sxs-lookup"><span data-stu-id="b2aff-347">*Browsing to the new web site*</span></span>

    <span data-ttu-id="b2aff-348">![Witryna sieci Web działa](aspnet-mvc-4-custom-action-filters/_static/image21.png "uruchamiania witryny sieci Web")</span><span class="sxs-lookup"><span data-stu-id="b2aff-348">![Web site running](aspnet-mvc-4-custom-action-filters/_static/image21.png "Web site running")</span></span>

    <span data-ttu-id="b2aff-349">*Witryna sieci Web uruchomiona*</span><span class="sxs-lookup"><span data-stu-id="b2aff-349">*Web site running*</span></span>
6. <span data-ttu-id="b2aff-350">Wróć do portalu i kliknij nazwę witryny sieci web w obszarze **nazwa** kolumny do wyświetlenia strony zarządzania.</span><span class="sxs-lookup"><span data-stu-id="b2aff-350">Go back to the portal and click the name of the web site under the **Name** column to display the management pages.</span></span>

    <span data-ttu-id="b2aff-351">![Otwieranie stron witryny sieci web zarządzania](aspnet-mvc-4-custom-action-filters/_static/image22.png "otwieranie stron zarządzania witryny sieci web")</span><span class="sxs-lookup"><span data-stu-id="b2aff-351">![Opening the web site management pages](aspnet-mvc-4-custom-action-filters/_static/image22.png "Opening the web site management pages")</span></span>

    <span data-ttu-id="b2aff-352">*Otwieranie stron zarządzania witryny sieci Web*</span><span class="sxs-lookup"><span data-stu-id="b2aff-352">*Opening the Web Site management pages*</span></span>
7. <span data-ttu-id="b2aff-353">W **pulpitu nawigacyjnego** w obszarze **szybkiego dostępu** kliknij **pobieranie profilu publikowania** łącza.</span><span class="sxs-lookup"><span data-stu-id="b2aff-353">In the **Dashboard** page, under the **quick glance** section, click the **Download publish profile** link.</span></span>

    > [!NOTE]
    > <span data-ttu-id="b2aff-354">*Profilu publikowania* zawiera wszystkie informacje wymagane do publikowania aplikacji sieci web do witryny sieci Web systemu Windows Azure dla każdej metody włączone publikacji.</span><span class="sxs-lookup"><span data-stu-id="b2aff-354">The *publish profile* contains all of the information required to publish a web application to a Windows Azure website for each enabled publication method.</span></span> <span data-ttu-id="b2aff-355">Profil publikowania zawiera adresy URL, poświadczenia użytkownika i parametry bazy danych wymagane do nawiązania połączenia i uwierzytelniania dla każdego z punktów końcowych, dla których włączono metoda publikacji.</span><span class="sxs-lookup"><span data-stu-id="b2aff-355">The publish profile contains the URLs, user credentials and database strings required to connect to and authenticate against each of the endpoints for which a publication method is enabled.</span></span> <span data-ttu-id="b2aff-356">**Microsoft WebMatrix 2**, **Microsoft Visual Studio Express for Web** i **programu Microsoft Visual Studio 2012** obsługują odczytywanie publikowanie profile do zautomatyzowania te programy Publikowanie aplikacji sieci web do witryn sieci Web systemu Windows Azure.</span><span class="sxs-lookup"><span data-stu-id="b2aff-356">**Microsoft WebMatrix 2**, **Microsoft Visual Studio Express for Web** and **Microsoft Visual Studio 2012** support reading publish profiles to automate configuration of these programs for publishing web applications to Windows Azure websites.</span></span>

    <span data-ttu-id="b2aff-357">![Pobieranie witryny sieci web profilu publikowania](aspnet-mvc-4-custom-action-filters/_static/image23.png "pobierania witryny sieci web profilu publikowania")</span><span class="sxs-lookup"><span data-stu-id="b2aff-357">![Downloading the web site publish profile](aspnet-mvc-4-custom-action-filters/_static/image23.png "Downloading the web site publish profile")</span></span>

    <span data-ttu-id="b2aff-358">*Pobieranie witryny sieci Web profilu publikowania*</span><span class="sxs-lookup"><span data-stu-id="b2aff-358">*Downloading the Web Site publish profile*</span></span>
8. <span data-ttu-id="b2aff-359">Pobierz profil publikowania w znanej lokalizacji.</span><span class="sxs-lookup"><span data-stu-id="b2aff-359">Download the publish profile file to a known location.</span></span> <span data-ttu-id="b2aff-360">Dodatkowo w tym ćwiczeniu zobaczysz jak opublikować aplikację sieci web do witryny sieci Web systemu Windows Azure w programie Visual Studio przy użyciu tego pliku.</span><span class="sxs-lookup"><span data-stu-id="b2aff-360">Further in this exercise you will see how to use this file to publish a web application to a Windows Azure Web Sites from Visual Studio.</span></span>

    <span data-ttu-id="b2aff-361">![Zapisywanie pliku profilu publikowania](aspnet-mvc-4-custom-action-filters/_static/image24.png "zapisywanie profilu publikowania")</span><span class="sxs-lookup"><span data-stu-id="b2aff-361">![Saving the publish profile file](aspnet-mvc-4-custom-action-filters/_static/image24.png "Saving the publish profile")</span></span>

    <span data-ttu-id="b2aff-362">*Zapisywanie pliku profilu publikowania*</span><span class="sxs-lookup"><span data-stu-id="b2aff-362">*Saving the publish profile file*</span></span>

<a id="ApxBTask2"></a>

<a id="Task_2_-_Configuring_the_Database_Server"></a>
#### <a name="task-2---configuring-the-database-server"></a><span data-ttu-id="b2aff-363">Zadanie 2 — Konfigurowanie serwera bazy danych</span><span class="sxs-lookup"><span data-stu-id="b2aff-363">Task 2 - Configuring the Database Server</span></span>

<span data-ttu-id="b2aff-364">Jeśli aplikacja korzysta z programu SQL Server baz danych, należy utworzyć serwer bazy danych SQL.</span><span class="sxs-lookup"><span data-stu-id="b2aff-364">If your application makes use of SQL Server databases you will need to create a SQL Database server.</span></span> <span data-ttu-id="b2aff-365">Jeśli chcesz wdrożyć prostą aplikację, która nie korzysta z programu SQL Server może pominąć to zadanie.</span><span class="sxs-lookup"><span data-stu-id="b2aff-365">If you want to deploy a simple application that does not use SQL Server you might skip this task.</span></span>

1. <span data-ttu-id="b2aff-366">Będzie potrzebny serwer bazy danych SQL do przechowywania bazy danych aplikacji.</span><span class="sxs-lookup"><span data-stu-id="b2aff-366">You will need a SQL Database server for storing the application database.</span></span> <span data-ttu-id="b2aff-367">Można wyświetlić serwery bazy danych SQL z subskrypcji usługi Windows Azure Management Portal pod adresem **baz danych Sql** | **serwerów** | **serwera Pulpit nawigacyjny**.</span><span class="sxs-lookup"><span data-stu-id="b2aff-367">You can view the SQL Database servers from your subscription in the Windows Azure Management portal at **Sql Databases** | **Servers** | **Server's Dashboard**.</span></span> <span data-ttu-id="b2aff-368">Jeśli nie masz serwer, który został utworzony, można utworzyć przy użyciu jednego **Dodaj** przycisk paska poleceń.</span><span class="sxs-lookup"><span data-stu-id="b2aff-368">If you do not have a server created, you can create one using the **Add** button on the command bar.</span></span> <span data-ttu-id="b2aff-369">Zwróć uwagę na **nazwę serwera i adres URL, nazwę logowania administratora i hasła**, jak będą używane w następnego zadania.</span><span class="sxs-lookup"><span data-stu-id="b2aff-369">Take note of the **server name and URL, administrator login name and password**, as you will use them in the next tasks.</span></span> <span data-ttu-id="b2aff-370">Nie należy tworzyć bazy danych jeszcze, jako zostaną utworzone w późniejszym terminie.</span><span class="sxs-lookup"><span data-stu-id="b2aff-370">Do not create the database yet, as it will be created in a later stage.</span></span>

    <span data-ttu-id="b2aff-371">![Pulpit nawigacyjny serwera bazy danych SQL](aspnet-mvc-4-custom-action-filters/_static/image25.png "pulpitu nawigacyjnego serwera bazy danych SQL")</span><span class="sxs-lookup"><span data-stu-id="b2aff-371">![SQL Database Server Dashboard](aspnet-mvc-4-custom-action-filters/_static/image25.png "SQL Database Server Dashboard")</span></span>

    <span data-ttu-id="b2aff-372">*Pulpit nawigacyjny serwera bazy danych SQL*</span><span class="sxs-lookup"><span data-stu-id="b2aff-372">*SQL Database Server Dashboard*</span></span>
2. <span data-ttu-id="b2aff-373">W następnym zadaniem Testuj połączenie z bazą danych z programu Visual Studio z tego powodu należy uwzględnić lokalny adres IP serwera liście **dozwolone adresy IP**.</span><span class="sxs-lookup"><span data-stu-id="b2aff-373">In the next task you will test the database connection from Visual Studio, for that reason you need to include your local IP address in the server's list of **Allowed IP Addresses**.</span></span> <span data-ttu-id="b2aff-374">Aby to zrobić, kliknij przycisk **Konfiguruj**, wybierz adres IP z **bieżącego adresu IP klienta** i wklej go na **początkowy adres IP** i **końcowy adres IP** pola tekstowe i kliknij przycisk ![add-client-ip-address-ok-button](aspnet-mvc-4-custom-action-filters/_static/image26.png) przycisku.</span><span class="sxs-lookup"><span data-stu-id="b2aff-374">To do that, click **Configure**, select the IP address from **Current Client IP Address** and paste it on the **Start IP Address** and **End IP Address** text boxes and click the ![add-client-ip-address-ok-button](aspnet-mvc-4-custom-action-filters/_static/image26.png) button.</span></span>

    ![Dodawanie adresu IP klienta](aspnet-mvc-4-custom-action-filters/_static/image27.png)

    <span data-ttu-id="b2aff-376">*Dodawanie adresu IP klienta*</span><span class="sxs-lookup"><span data-stu-id="b2aff-376">*Adding Client IP Address*</span></span>
3. <span data-ttu-id="b2aff-377">Raz **adres IP klienta** jest dodawany do dozwolonych adresów IP kliknij na **zapisać** o potwierdzenie zmian.</span><span class="sxs-lookup"><span data-stu-id="b2aff-377">Once the **Client IP Address** is added to the allowed IP addresses list, click on **Save** to confirm the changes.</span></span>

    ![Potwierdzenie zmian](aspnet-mvc-4-custom-action-filters/_static/image28.png)

    <span data-ttu-id="b2aff-379">*Potwierdzenie zmian*</span><span class="sxs-lookup"><span data-stu-id="b2aff-379">*Confirm Changes*</span></span>

<a id="ApxBTask3"></a>

<a id="Task_3_-_Publishing_an_ASPNET_MVC_4_Application_using_Web_Deploy"></a>
#### <a name="task-3---publishing-an-aspnet-mvc-4-application-using-web-deploy"></a><span data-ttu-id="b2aff-380">Zadanie 3 - publikowania aplikacji ASP.NET MVC 4 przy użyciu narzędzia Web Deploy</span><span class="sxs-lookup"><span data-stu-id="b2aff-380">Task 3 - Publishing an ASP.NET MVC 4 Application using Web Deploy</span></span>

1. <span data-ttu-id="b2aff-381">Wróć do rozwiązania ASP.NET MVC 4.</span><span class="sxs-lookup"><span data-stu-id="b2aff-381">Go back to the ASP.NET MVC 4 solution.</span></span> <span data-ttu-id="b2aff-382">W **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy projekt witryny sieci web i wybierz **publikowania**.</span><span class="sxs-lookup"><span data-stu-id="b2aff-382">In the **Solution Explorer**, right-click the web site project and select **Publish**.</span></span>

    <span data-ttu-id="b2aff-383">![Publikowanie aplikacji](aspnet-mvc-4-custom-action-filters/_static/image29.png "publikowania aplikacji")</span><span class="sxs-lookup"><span data-stu-id="b2aff-383">![Publishing the Application](aspnet-mvc-4-custom-action-filters/_static/image29.png "Publishing the Application")</span></span>

    <span data-ttu-id="b2aff-384">*Publikowanie witryny sieci web*</span><span class="sxs-lookup"><span data-stu-id="b2aff-384">*Publishing the web site*</span></span>
2. <span data-ttu-id="b2aff-385">Zaimportuj profil publikowania, zapisana w pierwszym zadaniu.</span><span class="sxs-lookup"><span data-stu-id="b2aff-385">Import the publish profile you saved in the first task.</span></span>

    <span data-ttu-id="b2aff-386">![Importowanie profilu publikowania](aspnet-mvc-4-custom-action-filters/_static/image30.png "Importowanie profilu publikowania")</span><span class="sxs-lookup"><span data-stu-id="b2aff-386">![Importing the publish profile](aspnet-mvc-4-custom-action-filters/_static/image30.png "Importing the publish profile")</span></span>

    <span data-ttu-id="b2aff-387">*Importowanie profilu publikowania*</span><span class="sxs-lookup"><span data-stu-id="b2aff-387">*Importing publish profile*</span></span>
3. <span data-ttu-id="b2aff-388">Kliknij przycisk **Weryfikacja połączenia z**.</span><span class="sxs-lookup"><span data-stu-id="b2aff-388">Click **Validate Connection**.</span></span> <span data-ttu-id="b2aff-389">Po zakończeniu sprawdzania kliknij **dalej**.</span><span class="sxs-lookup"><span data-stu-id="b2aff-389">Once Validation is complete click **Next**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="b2aff-390">Zakończeniu sprawdzania poprawności, gdy zostanie wyświetlony zielony znacznik wyboru są wyświetlane obok przycisku sprawdzania poprawności połączenia.</span><span class="sxs-lookup"><span data-stu-id="b2aff-390">Validation is complete once you see a green checkmark appear next to the Validate Connection button.</span></span>

    <span data-ttu-id="b2aff-391">![Sprawdzanie poprawności połączenia](aspnet-mvc-4-custom-action-filters/_static/image31.png "sprawdzanie poprawności połączenia")</span><span class="sxs-lookup"><span data-stu-id="b2aff-391">![Validating connection](aspnet-mvc-4-custom-action-filters/_static/image31.png "Validating connection")</span></span>

    <span data-ttu-id="b2aff-392">*Sprawdzanie poprawności połączenia*</span><span class="sxs-lookup"><span data-stu-id="b2aff-392">*Validating connection*</span></span>
4. <span data-ttu-id="b2aff-393">W **ustawienia** w obszarze **baz danych** sekcji, kliknij przycisk Dalej, aby textbox połączenia bazy danych (tj. **połączenia DefaultConnection**).</span><span class="sxs-lookup"><span data-stu-id="b2aff-393">In the **Settings** page, under the **Databases** section, click the button next to your database connection's textbox (i.e. **DefaultConnection**).</span></span>

    <span data-ttu-id="b2aff-394">![Konfiguracja narzędzia Web deploy](aspnet-mvc-4-custom-action-filters/_static/image32.png "Konfiguracja narzędzia Web deploy")</span><span class="sxs-lookup"><span data-stu-id="b2aff-394">![Web deploy configuration](aspnet-mvc-4-custom-action-filters/_static/image32.png "Web deploy configuration")</span></span>

    <span data-ttu-id="b2aff-395">*Konfiguracja narzędzia Web deploy*</span><span class="sxs-lookup"><span data-stu-id="b2aff-395">*Web deploy configuration*</span></span>
5. <span data-ttu-id="b2aff-396">Skonfiguruj połączenie z bazą danych w następujący sposób:</span><span class="sxs-lookup"><span data-stu-id="b2aff-396">Configure the database connection as follows:</span></span>

   - <span data-ttu-id="b2aff-397">W **nazwy serwera** wpisz swoją bazą danych SQL server adresu URL przy użyciu *tcp:* prefiks.</span><span class="sxs-lookup"><span data-stu-id="b2aff-397">In the **Server name** type your SQL Database server URL using the *tcp:* prefix.</span></span>
   - <span data-ttu-id="b2aff-398">W **nazwy użytkownika** wpisz nazwę logowania administratora serwera.</span><span class="sxs-lookup"><span data-stu-id="b2aff-398">In **User name** type your server administrator login name.</span></span>
   - <span data-ttu-id="b2aff-399">W **hasło** wpisz hasło logowania administratora serwera.</span><span class="sxs-lookup"><span data-stu-id="b2aff-399">In **Password** type your server administrator login password.</span></span>
   - <span data-ttu-id="b2aff-400">Wpisz nazwę nowej bazy danych.</span><span class="sxs-lookup"><span data-stu-id="b2aff-400">Type a new database name.</span></span>

     <span data-ttu-id="b2aff-401">![Konfigurowanie parametrów połączenia z lokalizacją docelową](aspnet-mvc-4-custom-action-filters/_static/image33.png "Konfigurowanie parametrów połączenia z lokalizacją docelową")</span><span class="sxs-lookup"><span data-stu-id="b2aff-401">![Configuring destination connection string](aspnet-mvc-4-custom-action-filters/_static/image33.png "Configuring destination connection string")</span></span>

     <span data-ttu-id="b2aff-402">*Konfigurowanie parametrów połączenia z lokalizacją docelową*</span><span class="sxs-lookup"><span data-stu-id="b2aff-402">*Configuring destination connection string*</span></span>
6. <span data-ttu-id="b2aff-403">Następnie kliknij przycisk **OK**.</span><span class="sxs-lookup"><span data-stu-id="b2aff-403">Then click **OK**.</span></span> <span data-ttu-id="b2aff-404">Po wyświetleniu monitu można utworzyć bazy danych kliknij **tak**.</span><span class="sxs-lookup"><span data-stu-id="b2aff-404">When prompted to create the database click **Yes**.</span></span>

    <span data-ttu-id="b2aff-405">![Tworzenie bazy danych](aspnet-mvc-4-custom-action-filters/_static/image34.png "tworzenie parametry bazy danych")</span><span class="sxs-lookup"><span data-stu-id="b2aff-405">![Creating the database](aspnet-mvc-4-custom-action-filters/_static/image34.png "Creating the database string")</span></span>

    <span data-ttu-id="b2aff-406">*Tworzenie bazy danych*</span><span class="sxs-lookup"><span data-stu-id="b2aff-406">*Creating the database*</span></span>
7. <span data-ttu-id="b2aff-407">Ciągu połączenia używanego do łączenia z bazą danych SQL w systemie Windows Azure jest wyświetlany w pole tekstowe domyślne połączenie.</span><span class="sxs-lookup"><span data-stu-id="b2aff-407">The connection string you will use to connect to SQL Database in Windows Azure is shown within Default Connection textbox.</span></span> <span data-ttu-id="b2aff-408">Następnie kliknij przycisk **Dalej**.</span><span class="sxs-lookup"><span data-stu-id="b2aff-408">Then click **Next**.</span></span>

    <span data-ttu-id="b2aff-409">![Parametry połączenia wskazujące bazę danych SQL](aspnet-mvc-4-custom-action-filters/_static/image35.png "ciąg połączenia wskazujące bazę danych SQL")</span><span class="sxs-lookup"><span data-stu-id="b2aff-409">![Connection string pointing to SQL Database](aspnet-mvc-4-custom-action-filters/_static/image35.png "Connection string pointing to SQL Database")</span></span>

    <span data-ttu-id="b2aff-410">*Parametry połączenia wskazujące bazę danych SQL*</span><span class="sxs-lookup"><span data-stu-id="b2aff-410">*Connection string pointing to SQL Database*</span></span>
8. <span data-ttu-id="b2aff-411">W **Podgląd** kliknij przycisk **publikowania**.</span><span class="sxs-lookup"><span data-stu-id="b2aff-411">In the **Preview** page, click **Publish**.</span></span>

    <span data-ttu-id="b2aff-412">![Publikowanie aplikacji sieci web](aspnet-mvc-4-custom-action-filters/_static/image36.png "publikowania aplikacji sieci web")</span><span class="sxs-lookup"><span data-stu-id="b2aff-412">![Publishing the web application](aspnet-mvc-4-custom-action-filters/_static/image36.png "Publishing the web application")</span></span>

    <span data-ttu-id="b2aff-413">*Publikowanie aplikacji sieci web*</span><span class="sxs-lookup"><span data-stu-id="b2aff-413">*Publishing the web application*</span></span>
9. <span data-ttu-id="b2aff-414">Po zakończeniu procesu publikowania domyślnej przeglądarce otworzy opublikowanej witryny sieci web.</span><span class="sxs-lookup"><span data-stu-id="b2aff-414">Once the publishing process finishes, your default browser will open the published web site.</span></span>

<a id="AppendixC"></a>

<a id="Appendix_C_Using_Code_Snippets"></a>
## <a name="appendix-c-using-code-snippets"></a><span data-ttu-id="b2aff-415">Dodatek C: korzystania z wstawek kodu</span><span class="sxs-lookup"><span data-stu-id="b2aff-415">Appendix C: Using Code Snippets</span></span>

<span data-ttu-id="b2aff-416">Wstawki kodu zapewniają całego kodu, które są potrzebne w zasięgu ręki.</span><span class="sxs-lookup"><span data-stu-id="b2aff-416">With code snippets, you have all the code you need at your fingertips.</span></span> <span data-ttu-id="b2aff-417">Dokument laboratorium informuje o dokładnie po ich użycia, jak pokazano na poniższej ilustracji.</span><span class="sxs-lookup"><span data-stu-id="b2aff-417">The lab document will tell you exactly when you can use them, as shown in the following figure.</span></span>

<span data-ttu-id="b2aff-418">![Aby wstawić kod do projektu przy użyciu wstawki kodu programu Visual Studio](aspnet-mvc-4-custom-action-filters/_static/image37.png "wstawki kodu za pomocą programu Visual Studio, aby wstawić kod do projektu")</span><span class="sxs-lookup"><span data-stu-id="b2aff-418">![Using Visual Studio code snippets to insert code into your project](aspnet-mvc-4-custom-action-filters/_static/image37.png "Using Visual Studio code snippets to insert code into your project")</span></span>

<span data-ttu-id="b2aff-419">*Aby wstawić kod do projektu przy użyciu wstawki kodu programu Visual Studio*</span><span class="sxs-lookup"><span data-stu-id="b2aff-419">*Using Visual Studio code snippets to insert code into your project*</span></span>

<span data-ttu-id="b2aff-420">***Aby dodać fragment kodu za pomocą klawiatury (C# tylko)***</span><span class="sxs-lookup"><span data-stu-id="b2aff-420">***To add a code snippet using the keyboard (C# only)***</span></span>

1. <span data-ttu-id="b2aff-421">Umieść kursor, w którym chcesz wstawić kod.</span><span class="sxs-lookup"><span data-stu-id="b2aff-421">Place the cursor where you would like to insert the code.</span></span>
2. <span data-ttu-id="b2aff-422">Zacznij wpisywać nazwę fragment (bez spacji i łączniki).</span><span class="sxs-lookup"><span data-stu-id="b2aff-422">Start typing the snippet name (without spaces or hyphens).</span></span>
3. <span data-ttu-id="b2aff-423">Obejrzyj jako IntelliSense wyświetla zgodne z nazwami wstawki.</span><span class="sxs-lookup"><span data-stu-id="b2aff-423">Watch as IntelliSense displays matching snippets' names.</span></span>
4. <span data-ttu-id="b2aff-424">Wybierz prawidłowe fragment (lub zachować wpisywanie do momentu wybrania fragment całą nazwę).</span><span class="sxs-lookup"><span data-stu-id="b2aff-424">Select the correct snippet (or keep typing until the entire snippet's name is selected).</span></span>
5. <span data-ttu-id="b2aff-425">Naciśnij klawisz Tab dwa razy, aby wstawić fragment kodu w lokalizacji kursora.</span><span class="sxs-lookup"><span data-stu-id="b2aff-425">Press the Tab key twice to insert the snippet at the cursor location.</span></span>

<span data-ttu-id="b2aff-426">![Rozpocznij wpisywanie nazwy fragment](aspnet-mvc-4-custom-action-filters/_static/image38.png "zacznij wpisywać nazwę wstawki programu")</span><span class="sxs-lookup"><span data-stu-id="b2aff-426">![Start typing the snippet name](aspnet-mvc-4-custom-action-filters/_static/image38.png "Start typing the snippet name")</span></span>

<span data-ttu-id="b2aff-427">*Rozpocznij wpisywanie nazwy fragment kodu*</span><span class="sxs-lookup"><span data-stu-id="b2aff-427">*Start typing the snippet name*</span></span>

<span data-ttu-id="b2aff-428">![Naciśnij klawisz Tab, aby wybrać wyróżnione fragment](aspnet-mvc-4-custom-action-filters/_static/image39.png "naciśnij klawisz Tab, aby wybrać wyróżnione fragment kodu")</span><span class="sxs-lookup"><span data-stu-id="b2aff-428">![Press Tab to select the highlighted snippet](aspnet-mvc-4-custom-action-filters/_static/image39.png "Press Tab to select the highlighted snippet")</span></span>

<span data-ttu-id="b2aff-429">*Naciśnij klawisz Tab, aby wybrać wyróżnione fragment kodu*</span><span class="sxs-lookup"><span data-stu-id="b2aff-429">*Press Tab to select the highlighted snippet*</span></span>

<span data-ttu-id="b2aff-430">![Ponownie naciśnij klawisz Tab i fragment rozwinie](aspnet-mvc-4-custom-action-filters/_static/image40.png "rozwinie ponownie naciśnij klawisz Tab i wstawki kodu")</span><span class="sxs-lookup"><span data-stu-id="b2aff-430">![Press Tab again and the snippet will expand](aspnet-mvc-4-custom-action-filters/_static/image40.png "Press Tab again and the snippet will expand")</span></span>

<span data-ttu-id="b2aff-431">*Rozwinie ponownie naciśnij klawisz Tab i wstawki kodu*</span><span class="sxs-lookup"><span data-stu-id="b2aff-431">*Press Tab again and the snippet will expand*</span></span>

<span data-ttu-id="b2aff-432">***Aby dodać fragment kodu za pomocą myszy (C#, Visual Basic i XML)*** 1.</span><span class="sxs-lookup"><span data-stu-id="b2aff-432">***To add a code snippet using the mouse (C#, Visual Basic and XML)*** 1.</span></span> <span data-ttu-id="b2aff-433">Kliknij prawym przyciskiem myszy, gdy chcesz wstawić fragment kodu.</span><span class="sxs-lookup"><span data-stu-id="b2aff-433">Right-click where you want to insert the code snippet.</span></span>

1. <span data-ttu-id="b2aff-434">Wybierz **wstawić fragment** następuje **Moje wstawki kodu**.</span><span class="sxs-lookup"><span data-stu-id="b2aff-434">Select **Insert Snippet** followed by **My Code Snippets**.</span></span>
2. <span data-ttu-id="b2aff-435">Wybierz odpowiedni fragment z listy, klikając go.</span><span class="sxs-lookup"><span data-stu-id="b2aff-435">Pick the relevant snippet from the list, by clicking on it.</span></span>

<span data-ttu-id="b2aff-436">![Kliknij prawym przyciskiem myszy, gdy chcesz wstawić fragment kodu i wybierz wstawić fragment](aspnet-mvc-4-custom-action-filters/_static/image41.png "kliknij prawym przyciskiem myszy, gdy chcesz wstawić fragment kodu i wybierz wstawić fragment kodu")</span><span class="sxs-lookup"><span data-stu-id="b2aff-436">![Right-click where you want to insert the code snippet and select Insert Snippet](aspnet-mvc-4-custom-action-filters/_static/image41.png "Right-click where you want to insert the code snippet and select Insert Snippet")</span></span>

<span data-ttu-id="b2aff-437">*Kliknij prawym przyciskiem myszy, gdy chcesz wstawić fragment kodu i wybierz wstawić fragment kodu*</span><span class="sxs-lookup"><span data-stu-id="b2aff-437">*Right-click where you want to insert the code snippet and select Insert Snippet*</span></span>

<span data-ttu-id="b2aff-438">![Wybierz odpowiedni fragment z listy, klikając go](aspnet-mvc-4-custom-action-filters/_static/image42.png "wybierz odpowiedni fragment z listy, klikając go")</span><span class="sxs-lookup"><span data-stu-id="b2aff-438">![Pick the relevant snippet from the list, by clicking on it](aspnet-mvc-4-custom-action-filters/_static/image42.png "Pick the relevant snippet from the list, by clicking on it")</span></span>

<span data-ttu-id="b2aff-439">*Wybierz odpowiedni fragment z listy, klikając go*</span><span class="sxs-lookup"><span data-stu-id="b2aff-439">*Pick the relevant snippet from the list, by clicking on it*</span></span>
