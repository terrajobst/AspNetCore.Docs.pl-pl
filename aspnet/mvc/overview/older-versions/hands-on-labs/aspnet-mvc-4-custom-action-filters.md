---
uid: mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-custom-action-filters
title: Filtry akcji niestandardowych platformy ASP.NET MVC 4 | Dokumentacja firmy Microsoft
author: rick-anderson
description: ASP.NET MVC zawiera filtry akcji do wykonania logikę filtrowania przed lub po nosi nazwę metody akcji. Filtry akcji są tha atrybuty niestandardowe...
ms.author: aspnetcontent
ms.date: 02/18/2013
ms.assetid: 969ab824-1b98-4552-81fe-b60ef5fc6887
msc.legacyurl: /mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-custom-action-filters
msc.type: authoredcontent
ms.openlocfilehash: c435fef624d526ceb01dbc370c5df52e2a1e8350
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/05/2018
ms.locfileid: "37807938"
---
# <a name="aspnet-mvc-4-custom-action-filters"></a><span data-ttu-id="e5461-104">Platforma ASP.NET MVC 4 — niestandardowe filtry akcji</span><span class="sxs-lookup"><span data-stu-id="e5461-104">ASP.NET MVC 4 Custom Action Filters</span></span>

<span data-ttu-id="e5461-105">Przez [Camp w sieci Web zespołu](https://twitter.com/webcamps)</span><span class="sxs-lookup"><span data-stu-id="e5461-105">By [Web Camps Team](https://twitter.com/webcamps)</span></span>

[<span data-ttu-id="e5461-106">Pobierz Camp Web szkolenia Kit</span><span class="sxs-lookup"><span data-stu-id="e5461-106">Download Web Camps Training Kit</span></span>](https://aka.ms/webcamps-training-kit)

<span data-ttu-id="e5461-107">ASP.NET MVC zawiera filtry akcji do wykonania logikę filtrowania przed lub po nosi nazwę metody akcji.</span><span class="sxs-lookup"><span data-stu-id="e5461-107">ASP.NET MVC provides Action Filters for executing filtering logic either before or after an action method is called.</span></span> <span data-ttu-id="e5461-108">Filtry akcji są atrybutów niestandardowych, które zapewniają deklaracyjne oznacza, że można dodać zachowanie akcja przed i po działaniu do metody akcji kontrolera.</span><span class="sxs-lookup"><span data-stu-id="e5461-108">Action Filters are custom attributes that provide declarative means to add pre-action and post-action behavior to the controller's action methods.</span></span>

<span data-ttu-id="e5461-109">W tym warsztatów utworzysz atrybutu filtru akcji niestandardowej w rozwiązaniu MvcMusicStore dziennika aktywności lokację do tabeli bazy danych i przechwytywać żądań kontrolera.</span><span class="sxs-lookup"><span data-stu-id="e5461-109">In this Hands-on Lab you will create a custom action filter attribute into MvcMusicStore solution to catch controller's requests and log the activity of a site into a database table.</span></span> <span data-ttu-id="e5461-110">Można dodać filtr rejestrowania przez iniekcję do dowolnego kontrolera lub akcji.</span><span class="sxs-lookup"><span data-stu-id="e5461-110">You will be able to add your logging filter by injection to any controller or action.</span></span> <span data-ttu-id="e5461-111">Na koniec zostanie wyświetlony widok dziennika, który wyświetla listę użytkowników.</span><span class="sxs-lookup"><span data-stu-id="e5461-111">Finally, you will see the log view that shows the list of visitors.</span></span>

<span data-ttu-id="e5461-112">W tym laboratorium praktyczne przyjęto założenie, masz podstawową wiedzę na temat **platformy ASP.NET MVC**.</span><span class="sxs-lookup"><span data-stu-id="e5461-112">This Hands-on Lab assumes you have basic knowledge of **ASP.NET MVC**.</span></span> <span data-ttu-id="e5461-113">Jeśli nie używasz **platformy ASP.NET MVC** , zalecamy zapoznać się z **podstawowe informacje dotyczące programu ASP.NET MVC 4** warsztatów.</span><span class="sxs-lookup"><span data-stu-id="e5461-113">If you have not used **ASP.NET MVC** before, we recommend you to go over **ASP.NET MVC 4 Fundamentals** Hands-on Lab.</span></span>

> [!NOTE]
> <span data-ttu-id="e5461-114">Wszystkie przykładowy kod i fragmenty są uwzględnione w sieci Web Camp zestaw szkoleniowy, pod [wersji Microsoft — w sieci Web/WebCampTrainingKit](https://aka.ms/webcamps-training-kit).</span><span class="sxs-lookup"><span data-stu-id="e5461-114">All sample code and snippets are included in the Web Camps Training Kit, available from at [Microsoft-Web/WebCampTrainingKit Releases](https://aka.ms/webcamps-training-kit).</span></span> <span data-ttu-id="e5461-115">Projekt specyficzne dla tego laboratorium znajduje się w temacie [filtry akcji programu ASP.NET MVC 4 niestandardowe](https://github.com/Microsoft-Web/HOL-MVC4CustomActionFilters).</span><span class="sxs-lookup"><span data-stu-id="e5461-115">The project specific to this lab is available at [ASP.NET MVC 4 Custom Action Filters](https://github.com/Microsoft-Web/HOL-MVC4CustomActionFilters).</span></span>

<a id="Objectives"></a>
### <a name="objectives"></a><span data-ttu-id="e5461-116">Cele</span><span class="sxs-lookup"><span data-stu-id="e5461-116">Objectives</span></span>

<span data-ttu-id="e5461-117">W tym laboratorium praktyczne dowiesz się jak:</span><span class="sxs-lookup"><span data-stu-id="e5461-117">In this Hands-On Lab, you will learn how to:</span></span>

- <span data-ttu-id="e5461-118">Tworzenie akcji niestandardowej atrybutu filtru, aby rozszerzyć możliwości filtrowania</span><span class="sxs-lookup"><span data-stu-id="e5461-118">Create a custom action filter attribute to extend filtering capabilities</span></span>
- <span data-ttu-id="e5461-119">Zastosuj atrybut niestandardowy filtr przez iniekcję do określonego poziomu</span><span class="sxs-lookup"><span data-stu-id="e5461-119">Apply a custom filter attribute by injection to a specific level</span></span>
- <span data-ttu-id="e5461-120">Zarejestruj filtry akcji niestandardowej globalnie</span><span class="sxs-lookup"><span data-stu-id="e5461-120">Register a custom action filters globally</span></span>

<a id="Prerequisites"></a>

<a id="Prerequisites"></a>
### <a name="prerequisites"></a><span data-ttu-id="e5461-121">Wymagania wstępne</span><span class="sxs-lookup"><span data-stu-id="e5461-121">Prerequisites</span></span>

<span data-ttu-id="e5461-122">Należy dysponować następującymi elementami do przygotowania tego laboratorium:</span><span class="sxs-lookup"><span data-stu-id="e5461-122">You must have the following items to complete this lab:</span></span>

- <span data-ttu-id="e5461-123">[Microsoft Visual Studio Express 2012 for Web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web) lub wyższego poziomu (odczyt [dodatek a.](#AppendixA) instrukcje dotyczące sposobu jego instalacji).</span><span class="sxs-lookup"><span data-stu-id="e5461-123">[Microsoft Visual Studio Express 2012 for Web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web) or superior (read [Appendix A](#AppendixA) for instructions on how to install it).</span></span>

<a id="Setup"></a>

<a id="Setup"></a>
### <a name="setup"></a><span data-ttu-id="e5461-124">Konfiguracja</span><span class="sxs-lookup"><span data-stu-id="e5461-124">Setup</span></span>

<span data-ttu-id="e5461-125">**Instalowanie fragmentów kodu**</span><span class="sxs-lookup"><span data-stu-id="e5461-125">**Installing Code Snippets**</span></span>

<span data-ttu-id="e5461-126">Dla wygody większość kodu, który będzie zarządzany wzdłuż tego laboratorium jest dostępna jako fragmenty kodu programu Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="e5461-126">For convenience, much of the code you will be managing along this lab is available as Visual Studio code snippets.</span></span> <span data-ttu-id="e5461-127">Aby zainstalować fragmenty kodu, uruchom **.\Source\Setup\CodeSnippets.vsi** pliku.</span><span class="sxs-lookup"><span data-stu-id="e5461-127">To install the code snippets run **.\Source\Setup\CodeSnippets.vsi** file.</span></span>

<span data-ttu-id="e5461-128">Jeśli nie jesteś zaznajomiony z fragmentów kodu w usłudze Visual Studio i chcesz dowiedzieć się, jak z nich korzystać, możesz zapoznać się z dodatku z tego dokumentu &quot; [dodatek C: za pomocą wstawek kodu programu](#AppendixC)&quot;.</span><span class="sxs-lookup"><span data-stu-id="e5461-128">If you are not familiar with the Visual Studio Code Snippets, and want to learn how to use them, you can refer to the appendix from this document &quot;[Appendix C: Using Code Snippets](#AppendixC)&quot;.</span></span>

* * *

<a id="Exercises"></a>

<a id="Exercises"></a>
## <a name="exercises"></a><span data-ttu-id="e5461-129">Ćwiczenia</span><span class="sxs-lookup"><span data-stu-id="e5461-129">Exercises</span></span>

<span data-ttu-id="e5461-130">W tym laboratorium praktyczne składa się przez następujących czynnościach:</span><span class="sxs-lookup"><span data-stu-id="e5461-130">This Hands-On Lab is comprised by the following exercises:</span></span>

1. [<span data-ttu-id="e5461-131">Ćwiczenie 1: Rejestrowanie akcji</span><span class="sxs-lookup"><span data-stu-id="e5461-131">Exercise 1: Logging actions</span></span>](#Exercise1)
2. [<span data-ttu-id="e5461-132">Ćwiczenie 2: Zarządzanie wieloma filtry akcji</span><span class="sxs-lookup"><span data-stu-id="e5461-132">Exercise 2: Managing Multiple Action Filters</span></span>](#Exercise2)

<span data-ttu-id="e5461-133">Szacowany czas do ukończenia tego laboratorium: **30 minut**.</span><span class="sxs-lookup"><span data-stu-id="e5461-133">Estimated time to complete this lab: **30 minutes**.</span></span>

> [!NOTE]
> <span data-ttu-id="e5461-134">Towarzyszy każdego wykonywania **zakończenia** folderu zawierającego wynikowy rozwiązania, należy uzyskać po ukończeniu ćwiczenia.</span><span class="sxs-lookup"><span data-stu-id="e5461-134">Each exercise is accompanied by an **End** folder containing the resulting solution you should obtain after completing the exercises.</span></span> <span data-ttu-id="e5461-135">Jeśli potrzebujesz dodatkowej pomocy ćwiczeń opisanych w dalszej, można użyć tego rozwiązania jako wskazówki.</span><span class="sxs-lookup"><span data-stu-id="e5461-135">You can use this solution as a guide if you need additional help working through the exercises.</span></span>


<a id="Exercise1"></a>

<a id="Exercise_1_Logging_Actions"></a>
### <a name="exercise-1-logging-actions"></a><span data-ttu-id="e5461-136">Ćwiczenie 1: Rejestrowanie akcji</span><span class="sxs-lookup"><span data-stu-id="e5461-136">Exercise 1: Logging Actions</span></span>

<span data-ttu-id="e5461-137">W tym ćwiczeniu dowiesz się, jak utworzyć filtr dziennika akcji niestandardowej za pomocą programu ASP.NET MVC 4 dostawców filtrów.</span><span class="sxs-lookup"><span data-stu-id="e5461-137">In this exercise, you will learn how to create a custom action log filter by using ASP.NET MVC 4 Filter Providers.</span></span> <span data-ttu-id="e5461-138">W tym celu filtr rejestrowania będą dotyczyć witryny MusicStore, które będą rejestrowane wszystkie działania w wybrane kontrolerów.</span><span class="sxs-lookup"><span data-stu-id="e5461-138">For that purpose you will apply a logging filter to the MusicStore site that will record all the activities in the selected controllers.</span></span>

<span data-ttu-id="e5461-139">Filtr będzie rozszerzać **ActionFilterAttributeClass** i zastąpić **OnActionExecuting** metodę, aby przechwycić każdego żądania, a następnie wykonać akcje rejestrowania.</span><span class="sxs-lookup"><span data-stu-id="e5461-139">The filter will extend **ActionFilterAttributeClass** and override **OnActionExecuting** method to catch each request and then perform the logging actions.</span></span> <span data-ttu-id="e5461-140">Informacje o kontekście żądania HTTP, wykonywanie metod, wyniki i parametry będzie świadczona przez platformy ASP.NET MVC **ActionExecutingContext** klasy **.**</span><span class="sxs-lookup"><span data-stu-id="e5461-140">The context information about HTTP requests, executing methods, results and parameters will be provided by ASP.NET MVC **ActionExecutingContext** class **.**</span></span>

> [!NOTE]
> <span data-ttu-id="e5461-141">ASP.NET MVC 4 ma również domyślnych dostawców filtrów, których można używać bez konieczności tworzenia niestandardowego filtru.</span><span class="sxs-lookup"><span data-stu-id="e5461-141">ASP.NET MVC 4 also has default filters providers you can use without creating a custom filter.</span></span> <span data-ttu-id="e5461-142">ASP.NET MVC 4 zawiera następujące typy filtrów:</span><span class="sxs-lookup"><span data-stu-id="e5461-142">ASP.NET MVC 4 provides the following types of filters:</span></span>
> 
> - <span data-ttu-id="e5461-143">**Autoryzacja** filtrowania, co sprawia, że decyzje dotyczące bezpieczeństwa o tym, czy można wykonać metody akcji, takich jak wykonanie uwierzytelniania lub sprawdzania poprawności właściwości żądania.</span><span class="sxs-lookup"><span data-stu-id="e5461-143">**Authorization** filter, which makes security decisions about whether to execute an action method, such as performing authentication or validating properties of the request.</span></span>
> - <span data-ttu-id="e5461-144">**Akcja** filtr, który opakowuje wykonanie metody akcji.</span><span class="sxs-lookup"><span data-stu-id="e5461-144">**Action** filter, which wraps the action method execution.</span></span> <span data-ttu-id="e5461-145">Ten filtr, można wykonać dodatkowe przetwarzania, takich jak dostarcza dodatkowe dane do metody akcji, kontroli wartość zwracaną lub który anulował wykonanie metody akcji</span><span class="sxs-lookup"><span data-stu-id="e5461-145">This filter can perform additional processing, such as providing extra data to the action method, inspecting the return value, or canceling execution of the action method</span></span>
> - <span data-ttu-id="e5461-146">**Wynik** filtr, który opakowuje wykonanie obiektu ActionResult.</span><span class="sxs-lookup"><span data-stu-id="e5461-146">**Result** filter, which wraps execution of the ActionResult object.</span></span> <span data-ttu-id="e5461-147">Ten filtr, można wykonać dodatkowego przetwarzania wyniku, takie jak modyfikowanie odpowiedzi HTTP.</span><span class="sxs-lookup"><span data-stu-id="e5461-147">This filter can perform additional processing of the result, such as modifying the HTTP response.</span></span>
> - <span data-ttu-id="e5461-148">**Wyjątek** filtr, który jest wykonywany, jeśli ma nieobsługiwany wyjątek spowodowany gdzieś w metodzie akcji, począwszy od filtry autoryzacji, a kończąc na wykonanie wynik.</span><span class="sxs-lookup"><span data-stu-id="e5461-148">**Exception** filter, which executes if there is an unhandled exception thrown somewhere in action method, starting with the authorization filters and ending with the execution of the result.</span></span> <span data-ttu-id="e5461-149">Filtry wyjątków może służyć do zadań, takich jak rejestrowanie lub wyświetlanie strony błędu.</span><span class="sxs-lookup"><span data-stu-id="e5461-149">Exception filters can be used for tasks such as logging or displaying an error page.</span></span>
> 
> <span data-ttu-id="e5461-150">Aby uzyskać więcej informacji o dostawcach filtrów można znaleźć ten link MSDN: ([https://msdn.microsoft.com/library/dd410209.aspx](https://msdn.microsoft.com/library/dd410209.aspx)).</span><span class="sxs-lookup"><span data-stu-id="e5461-150">For more information about Filters Providers please visit this MSDN link: ([https://msdn.microsoft.com/library/dd410209.aspx](https://msdn.microsoft.com/library/dd410209.aspx)) .</span></span>


<a id="AboutLoggingFeature"></a>

<a id="About_MVC_Music_Store_Application_logging_feature"></a>
#### <a name="about-mvc-music-store-application-logging-feature"></a><span data-ttu-id="e5461-151">Funkcja rejestrowania aplikacji programu MVC Music Store — informacje</span><span class="sxs-lookup"><span data-stu-id="e5461-151">About MVC Music Store Application logging feature</span></span>

<span data-ttu-id="e5461-152">To rozwiązanie Music Store ma nowej tabeli modelu danych do rejestrowania witryny **ActionLog**, przy użyciu następujących pól: Nazwa kontrolera, który odebrał żądanie, wywoływana akcja, adres IP klienta i sygnaturę czasową.</span><span class="sxs-lookup"><span data-stu-id="e5461-152">This Music Store solution has a new data model table for site logging, **ActionLog**, with the following fields: Name of the controller that received a request, Called action, Client IP and Time stamp.</span></span>

<span data-ttu-id="e5461-153">![Model danych. Tabela ActionLog. ](aspnet-mvc-4-custom-action-filters/_static/image1.png "Modelu danych. Tabela ActionLog.")</span><span class="sxs-lookup"><span data-stu-id="e5461-153">![Data model. ActionLog table.](aspnet-mvc-4-custom-action-filters/_static/image1.png "Data model. ActionLog table.")</span></span>

<span data-ttu-id="e5461-154">*Model danych — ActionLog tabeli*</span><span class="sxs-lookup"><span data-stu-id="e5461-154">*Data model - ActionLog table*</span></span>

<span data-ttu-id="e5461-155">Rozwiązanie udostępnia widok ASP.NET MVC dla dziennika akcji, która znajduje się w temacie **MvcMusicStores/widoków/ActionLog**:</span><span class="sxs-lookup"><span data-stu-id="e5461-155">The solution provides an ASP.NET MVC View for the Action log that can be found at **MvcMusicStores/Views/ActionLog**:</span></span>

<span data-ttu-id="e5461-156">![Widok dziennika akcji](aspnet-mvc-4-custom-action-filters/_static/image2.png "widok dziennika akcji")</span><span class="sxs-lookup"><span data-stu-id="e5461-156">![Action Log view](aspnet-mvc-4-custom-action-filters/_static/image2.png "Action Log view")</span></span>

<span data-ttu-id="e5461-157">*Widok dziennika akcji*</span><span class="sxs-lookup"><span data-stu-id="e5461-157">*Action Log view*</span></span>

<span data-ttu-id="e5461-158">Dzięki temu podane struktury całą pracę koncentrować się na przerywania kontrolera żądań i wykonywania rejestrowanie za pomocą niestandardowych filtrowania.</span><span class="sxs-lookup"><span data-stu-id="e5461-158">With this given structure, all the work will be focused on interrupting controller's request and performing the logging by using custom filtering.</span></span>

<a id="Ex1Task1"></a>

<a id="Task_1_-_Creating_a_Custom_Filter_to_Catch_a_Controllers_Request"></a>
#### <a name="task-1---creating-a-custom-filter-to-catch-a-controllers-request"></a><span data-ttu-id="e5461-159">Zadanie 1. Tworzenie niestandardowych filtr, aby przechwytywać żądania poziomu kontrolera</span><span class="sxs-lookup"><span data-stu-id="e5461-159">Task 1 - Creating a Custom Filter to Catch a Controller's Request</span></span>

<span data-ttu-id="e5461-160">W tym zadaniu utworzysz klasę atrybutów niestandardowego filtru, która będzie zawierać logiki rejestrowania.</span><span class="sxs-lookup"><span data-stu-id="e5461-160">In this task you will create a custom filter attribute class that will contain the logging logic.</span></span> <span data-ttu-id="e5461-161">W tym celu możesz rozszerzyć platformy ASP.NET MVC **ActionFilterAttribute** klasy i zaimplementuj interfejs **IActionFilter**.</span><span class="sxs-lookup"><span data-stu-id="e5461-161">For that purpose you will extend ASP.NET MVC **ActionFilterAttribute** Class and implement the interface **IActionFilter**.</span></span>

> [!NOTE]
> <span data-ttu-id="e5461-162">**ActionFilterAttribute** jest klasą bazową dla wszystkich filtrów atrybutu.</span><span class="sxs-lookup"><span data-stu-id="e5461-162">The **ActionFilterAttribute** is the base class for all the attribute filters.</span></span> <span data-ttu-id="e5461-163">Oferuje ono następujące metody umożliwiające wykonywanie logiki określonych po, a także przed wykonaniem akcji kontrolera:</span><span class="sxs-lookup"><span data-stu-id="e5461-163">It provides the following methods to execute a specific logic after and before controller action's execution:</span></span>
> 
> - <span data-ttu-id="e5461-164">**OnActionExecuting**(ActionExecutingContext filterContext): tuż przed akcji jest wywoływana metoda.</span><span class="sxs-lookup"><span data-stu-id="e5461-164">**OnActionExecuting**(ActionExecutingContext filterContext): Just before the action method is called.</span></span>
> - <span data-ttu-id="e5461-165">**OnActionExecuted**(ActionExecutedContext filterContext): po wywołaniu metody akcji i zanim wynik zostanie wykonany (przed renderowania widoku).</span><span class="sxs-lookup"><span data-stu-id="e5461-165">**OnActionExecuted**(ActionExecutedContext filterContext): After the action method is called and before the result is executed (before view render).</span></span>
> - <span data-ttu-id="e5461-166">**OnResultExecuting**(ResultExecutingContext filterContext): tak, zanim wynik zostanie wykonany (przed renderowania widoku).</span><span class="sxs-lookup"><span data-stu-id="e5461-166">**OnResultExecuting**(ResultExecutingContext filterContext): Just before the result is executed (before view render).</span></span>
> - <span data-ttu-id="e5461-167">**OnResultExecuted**(ResultExecutedContext filterContext): po wykonaniu wyniku (po renderowania widoku).</span><span class="sxs-lookup"><span data-stu-id="e5461-167">**OnResultExecuted**(ResultExecutedContext filterContext): After the result is executed (after the view is rendered).</span></span>
> 
> <span data-ttu-id="e5461-168">Przez zastąpienie dowolnego z tych metod w klasie pochodnej, można wykonać filtrowania kodu.</span><span class="sxs-lookup"><span data-stu-id="e5461-168">By overriding any of these methods into a derived class, you can execute your own filtering code.</span></span>


1. <span data-ttu-id="e5461-169">Otwórz **rozpocząć** rozwiązania znajdujący się w **\Source\Ex01-LoggingActions\Begin** folderu.</span><span class="sxs-lookup"><span data-stu-id="e5461-169">Open the **Begin** solution located at **\Source\Ex01-LoggingActions\Begin** folder.</span></span>

   1. <span data-ttu-id="e5461-170">Należy pobrać niektóre brakujące pakiety NuGet, przed kontynuowaniem.</span><span class="sxs-lookup"><span data-stu-id="e5461-170">You will need to download some missing NuGet packages before you continue.</span></span> <span data-ttu-id="e5461-171">Aby to zrobić, kliknij przycisk **projektu** menu, a następnie wybierz **Zarządzaj pakietami NuGet**.</span><span class="sxs-lookup"><span data-stu-id="e5461-171">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
   2. <span data-ttu-id="e5461-172">W **Zarządzaj pakietami NuGet** okno dialogowe, kliknij przycisk **przywrócić** Aby pobrać brakujące pakiety.</span><span class="sxs-lookup"><span data-stu-id="e5461-172">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
   3. <span data-ttu-id="e5461-173">Na koniec Skompiluj rozwiązanie, klikając **kompilacji** | **Kompiluj rozwiązanie**.</span><span class="sxs-lookup"><span data-stu-id="e5461-173">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

      > [!NOTE]
      > <span data-ttu-id="e5461-174">Jedną z zalet za pomocą narzędzia NuGet jest, że nie masz do wysłania wszystkie biblioteki w projekcie, zmniejszenie rozmiaru projektu.</span><span class="sxs-lookup"><span data-stu-id="e5461-174">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="e5461-175">Za pomocą narzędzi NuGet Power Tools, określając wersji pakietu w pliku Packages.config można pobrać wymaganych bibliotek podczas pierwszego uruchomienia projektu.</span><span class="sxs-lookup"><span data-stu-id="e5461-175">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="e5461-176">Jest to, dlaczego należy uruchomić następujące kroki, po otwarciu istniejącego rozwiązania, w tym środowisku laboratoryjnym.</span><span class="sxs-lookup"><span data-stu-id="e5461-176">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
      > 
      > <span data-ttu-id="e5461-177">Aby uzyskać więcej informacji znajduje się w artykule: [ http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages ](http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages).</span><span class="sxs-lookup"><span data-stu-id="e5461-177">For more information, see this article: [http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages](http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages).</span></span>
2. <span data-ttu-id="e5461-178">Dodaj nową klasę C# do **filtry** folder i nadaj mu nazwę *CustomActionFilter.cs*.</span><span class="sxs-lookup"><span data-stu-id="e5461-178">Add a new C# class into the **Filters** folder and name it *CustomActionFilter.cs*.</span></span> <span data-ttu-id="e5461-179">Ten folder będzie przechowywać wszystkie filtry niestandardowe.</span><span class="sxs-lookup"><span data-stu-id="e5461-179">This folder will store all the custom filters.</span></span>
3. <span data-ttu-id="e5461-180">Otwórz **CustomActionFilter.cs** i Dodaj odwołanie do **System.Web.Mvc** i **MvcMusicStore.Models** przestrzeni nazw:</span><span class="sxs-lookup"><span data-stu-id="e5461-180">Open **CustomActionFilter.cs** and add a reference to **System.Web.Mvc** and **MvcMusicStore.Models** namespaces:</span></span>

    <span data-ttu-id="e5461-181">(Code Snippet — *filtry akcji niestandardowej platformy ASP.NET MVC 4 — Ex1 CustomActionFilterNamespaces*)</span><span class="sxs-lookup"><span data-stu-id="e5461-181">(Code Snippet - *ASP.NET MVC 4 Custom Action Filters - Ex1-CustomActionFilterNamespaces*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample1.cs)]
4. <span data-ttu-id="e5461-182">Dziedziczenie **CustomActionFilter** klasy z **ActionFilterAttribute** , a następnie **CustomActionFilter** implementacji klasy **IActionFilter** interfejsu.</span><span class="sxs-lookup"><span data-stu-id="e5461-182">Inherit the **CustomActionFilter** class from **ActionFilterAttribute** and then make **CustomActionFilter** class implement **IActionFilter** interface.</span></span>

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample2.cs)]
5. <span data-ttu-id="e5461-183">Wprowadź **CustomActionFilter** klasie zastąpić metodę **OnActionExecuting** i dodać logikę potrzebną do dziennika wykonywania filtru.</span><span class="sxs-lookup"><span data-stu-id="e5461-183">Make **CustomActionFilter** class override the method **OnActionExecuting** and add the necessary logic to log the filter's execution.</span></span> <span data-ttu-id="e5461-184">Aby to zrobić, Dodaj następujący wyróżniony kod w ramach **CustomActionFilter** klasy.</span><span class="sxs-lookup"><span data-stu-id="e5461-184">To do this, add the following highlighted code within **CustomActionFilter** class.</span></span>

    <span data-ttu-id="e5461-185">(Code Snippet — *filtry akcji niestandardowej platformy ASP.NET MVC 4 — Ex1 LoggingActions*)</span><span class="sxs-lookup"><span data-stu-id="e5461-185">(Code Snippet - *ASP.NET MVC 4 Custom Action Filters - Ex1-LoggingActions*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample3.cs#Highlight)]

    > [!NOTE]
    > <span data-ttu-id="e5461-186">**OnActionExecuting** metoda używa **Entity Framework** można dodać nowego rejestru ActionLog.</span><span class="sxs-lookup"><span data-stu-id="e5461-186">**OnActionExecuting** method is using **Entity Framework** to add a new ActionLog register.</span></span> <span data-ttu-id="e5461-187">Tworzy i wypełnia nowe wystąpienie jednostki informacje o kontekście z **filterContext**.</span><span class="sxs-lookup"><span data-stu-id="e5461-187">It creates and fills a new entity instance with the context information from **filterContext**.</span></span>
    > 
    > <span data-ttu-id="e5461-188">Przeczytaj więcej o **kontekstem ControllerContext** klasy na [msdn](https://msdn.microsoft.com/library/system.web.mvc.controllercontext.aspx).</span><span class="sxs-lookup"><span data-stu-id="e5461-188">You can read more about **ControllerContext** class at [msdn](https://msdn.microsoft.com/library/system.web.mvc.controllercontext.aspx).</span></span>

<a id="Ex1Task2"></a>

<a id="Task_2_-_Injecting_a_Code_Interceptor_into_the_Store_Controller_Class"></a>
#### <a name="task-2---injecting-a-code-interceptor-into-the-store-controller-class"></a><span data-ttu-id="e5461-189">Zadanie 2 — wprowadzanie Interceptor kod do klasy kontrolera Store</span><span class="sxs-lookup"><span data-stu-id="e5461-189">Task 2 - Injecting a Code Interceptor into the Store Controller Class</span></span>

<span data-ttu-id="e5461-190">W tym zadaniu należy dodać filtr niestandardowy przez iniekcję go do wszystkich klas kontrolera i akcji kontrolera, które będą rejestrowane.</span><span class="sxs-lookup"><span data-stu-id="e5461-190">In this task you will add the custom filter by injecting it to all controller classes and controller actions that will be logged.</span></span> <span data-ttu-id="e5461-191">Na potrzeby tego ćwiczenia klasy kontrolera Store będzie prowadzić dziennik.</span><span class="sxs-lookup"><span data-stu-id="e5461-191">For the purpose of this exercise, the Store Controller class will have a log.</span></span>

<span data-ttu-id="e5461-192">Metoda **OnActionExecuting** z **ActionLogFilterAttribute** filtr niestandardowy jest uruchamiany, gdy element wprowadzonego jest wywoływana.</span><span class="sxs-lookup"><span data-stu-id="e5461-192">The method **OnActionExecuting** from **ActionLogFilterAttribute** custom filter runs when an injected element is called.</span></span>

<span data-ttu-id="e5461-193">Istnieje również możliwość przechwytuje metodę określonego kontrolera.</span><span class="sxs-lookup"><span data-stu-id="e5461-193">It is also possible to intercept a specific controller method.</span></span>

1. <span data-ttu-id="e5461-194">Otwórz **StoreController** na **MvcMusicStore\Controllers** i Dodaj odwołanie do **filtry** przestrzeni nazw:</span><span class="sxs-lookup"><span data-stu-id="e5461-194">Open the **StoreController** at **MvcMusicStore\Controllers** and add a reference to the **Filters** namespace:</span></span>

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample4.cs)]
2. <span data-ttu-id="e5461-195">Filtr niestandardowy wstrzyknąć **CustomActionFilter** do **StoreController** klasy, dodając **[CustomActionFilter]** atrybutu przed deklaracją klasy.</span><span class="sxs-lookup"><span data-stu-id="e5461-195">Inject the custom filter **CustomActionFilter** into **StoreController** class by adding **[CustomActionFilter]** attribute before the class declaration.</span></span>

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample5.cs)]

   > [!NOTE]
   > <span data-ttu-id="e5461-196">Jeśli filtr są wstrzykiwane do klasy kontrolera, jego akcje również są wprowadzane.</span><span class="sxs-lookup"><span data-stu-id="e5461-196">When a filter is injected into a controller class, all its actions are also injected.</span></span> <span data-ttu-id="e5461-197">Jeśli chcesz zastosować filtr tylko dla działań, trzeba będzie wstawić **[CustomActionFilter]** do każdego z nich z nich:</span><span class="sxs-lookup"><span data-stu-id="e5461-197">If you would like to apply the filter only for a set of actions, you would have to inject **[CustomActionFilter]** to each one of them:</span></span>
   > 
   > [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample6.cs)]

<a id="Ex1Task3"></a>

<a id="Task_3_-_Running_the_Application"></a>
#### <a name="task-3---running-the-application"></a><span data-ttu-id="e5461-198">Zadanie 3. uruchamianie aplikacji</span><span class="sxs-lookup"><span data-stu-id="e5461-198">Task 3 - Running the Application</span></span>

<span data-ttu-id="e5461-199">W tym zadaniu przetestujesz działa filtr rejestrowania.</span><span class="sxs-lookup"><span data-stu-id="e5461-199">In this task, you will test that the logging filter is working.</span></span> <span data-ttu-id="e5461-200">Należy uruchomić aplikację i odwiedź Sklep, a następnie będzie sprawdzać zarejestrowanych działań.</span><span class="sxs-lookup"><span data-stu-id="e5461-200">You will start the application and visit the store, and then you will check logged activities.</span></span>

1. <span data-ttu-id="e5461-201">Naciśnij klawisz **F5** do uruchomienia aplikacji.</span><span class="sxs-lookup"><span data-stu-id="e5461-201">Press **F5** to run the application.</span></span>
2. <span data-ttu-id="e5461-202">Przejdź do **/ActionLog** aby zobaczyć stan początkowy widok dziennika:</span><span class="sxs-lookup"><span data-stu-id="e5461-202">Browse to **/ActionLog** to see log view initial state:</span></span>

    <span data-ttu-id="e5461-203">![Rejestrowanie śledzenia stanu przed działaniem strony](aspnet-mvc-4-custom-action-filters/_static/image3.png "rejestrowania śledzenia stanu przed na pager")</span><span class="sxs-lookup"><span data-stu-id="e5461-203">![Log tracker status before page activity](aspnet-mvc-4-custom-action-filters/_static/image3.png "Log tracker status before page activity")</span></span>

    <span data-ttu-id="e5461-204">*Dziennika śledzenia stanu przed na pager*</span><span class="sxs-lookup"><span data-stu-id="e5461-204">*Log tracker status before page activity*</span></span>

   > [!NOTE]
   > <span data-ttu-id="e5461-205">Domyślnie będzie zawsze wyświetlany jeden element, który jest generowany podczas pobierania istniejących gatunki menu.</span><span class="sxs-lookup"><span data-stu-id="e5461-205">By default, it will always show one item that is generated when retrieving the existing genres for the menu.</span></span>
   > 
   > <span data-ttu-id="e5461-206">Dla uproszczenia czyścimy **ActionLog** tabeli przy każdym uruchomieniu aplikacji, widoczna będzie tylko dzienniki weryfikacji każdego określonego zadania.</span><span class="sxs-lookup"><span data-stu-id="e5461-206">For simplicity purposes we're cleaning up the **ActionLog** table each time the application runs so it will only show the logs of each particular task's verification.</span></span>
   > 
   > <span data-ttu-id="e5461-207">Może być konieczne usunięcie następujący kod z **sesji\_Start** — metoda (w **Global.asax** klasy), aby zapisać dziennik historyczny akcje wykonywane w ramach Store Kontroler.</span><span class="sxs-lookup"><span data-stu-id="e5461-207">You might need to remove the following code from the **Session\_Start** method (in the **Global.asax** class), in order to save an historical log for all the actions executed within the Store Controller.</span></span>
   > 
   > [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample7.cs)]
3. <span data-ttu-id="e5461-208">Kliknij jedną z **gatunki** z menu i wykonywać niektórych akcji, takich jak przeglądanie dostępnych albumu.</span><span class="sxs-lookup"><span data-stu-id="e5461-208">Click one of the **Genres** from the menu and perform some actions there, like browsing an available album.</span></span>
4. <span data-ttu-id="e5461-209">Przejdź do **/ActionLog** i jeśli dziennik jest pusty naciśnij **F5** Aby odświeżyć stronę.</span><span class="sxs-lookup"><span data-stu-id="e5461-209">Browse to **/ActionLog** and if the log is empty press **F5** to refresh the page.</span></span> <span data-ttu-id="e5461-210">Sprawdź, czy Twoje odwiedziny były śledzone:</span><span class="sxs-lookup"><span data-stu-id="e5461-210">Check that your visits were tracked:</span></span>

    <span data-ttu-id="e5461-211">![Dziennik akcji za pomocą działania rejestrowane](aspnet-mvc-4-custom-action-filters/_static/image4.png "dziennik akcji za pomocą działania rejestrowane")</span><span class="sxs-lookup"><span data-stu-id="e5461-211">![Action log with activity logged](aspnet-mvc-4-custom-action-filters/_static/image4.png "Action log with activity logged")</span></span>

    <span data-ttu-id="e5461-212">*Dziennik akcji za pomocą działania rejestrowane*</span><span class="sxs-lookup"><span data-stu-id="e5461-212">*Action log with activity logged*</span></span>

<a id="Exercise2"></a>

<a id="Exercise_2_Managing_Multiple_Action_Filters"></a>
### <a name="exercise-2-managing-multiple-action-filters"></a><span data-ttu-id="e5461-213">Ćwiczenie 2: Zarządzanie wieloma filtry akcji</span><span class="sxs-lookup"><span data-stu-id="e5461-213">Exercise 2: Managing Multiple Action Filters</span></span>

<span data-ttu-id="e5461-214">W tym ćwiczeniu możesz dodać drugi niestandardowy filtr akcji do klasy StoreController i zdefiniuj określonej kolejności, w którym będą wykonywane obu filtrów.</span><span class="sxs-lookup"><span data-stu-id="e5461-214">In this exercise you will add a second Custom Action Filter to the StoreController class and define the specific order in which both filters will be executed.</span></span> <span data-ttu-id="e5461-215">Następnie zmodyfikujemy kod, aby zarejestrować filtr globalny.</span><span class="sxs-lookup"><span data-stu-id="e5461-215">Then you will update the code to register the filter Globally.</span></span>

<span data-ttu-id="e5461-216">Istnieją różne opcje, aby wziąć pod uwagę podczas definiowania kolejność wykonywania filtrów.</span><span class="sxs-lookup"><span data-stu-id="e5461-216">There are different options to take into account when defining the Filters' execution order.</span></span> <span data-ttu-id="e5461-217">Na przykład właściwość kolejności i te filtry zakresu:</span><span class="sxs-lookup"><span data-stu-id="e5461-217">For example, the Order property and the Filters' scope:</span></span>

<span data-ttu-id="e5461-218">Można zdefiniować **zakres** dla każdego z filtrów, na przykład przykład możesz ograniczyć zakres wszystkie filtry akcji do działania w ramach **zakresem kontrolera**i wszystkie filtry autoryzacji do uruchamiania w **zakresie globalnym** .</span><span class="sxs-lookup"><span data-stu-id="e5461-218">You can define a **Scope** for each of the Filters, for example, you could scope all the Action Filters to run within the **Controller Scope**, and all Authorization Filters to run in **Global scope**.</span></span> <span data-ttu-id="e5461-219">Zakresy mają kolejność wykonywania zdefiniowane.</span><span class="sxs-lookup"><span data-stu-id="e5461-219">The scopes have a defined execution order.</span></span>

<span data-ttu-id="e5461-220">Ponadto każdy filtr akcji ma właściwość zlecenia służy do określania kolejności wykonywania w zakresie filtru.</span><span class="sxs-lookup"><span data-stu-id="e5461-220">Additionally, each action filter has an Order property which is used to determine the execution order in the scope of the filter.</span></span>

<span data-ttu-id="e5461-221">Więcej informacji na temat kolejność wykonywania filtrów akcji niestandardowych można znaleźć w artykule MSDN: ([https://msdn.microsoft.com/library/dd381609(v=vs.98).aspx](https://msdn.microsoft.com/library/dd381609(v=vs.98).aspx)).</span><span class="sxs-lookup"><span data-stu-id="e5461-221">For more information about Custom Action Filters execution order, please visit this MSDN article: ([https://msdn.microsoft.com/library/dd381609(v=vs.98).aspx](https://msdn.microsoft.com/library/dd381609(v=vs.98).aspx)).</span></span>

<a id="Ex2Task1"></a>

<a id="Task_1_Creating_a_new_Custom_Action_Filter"></a>
#### <a name="task-1-creating-a-new-custom-action-filter"></a><span data-ttu-id="e5461-222">Zadanie 1: Tworzenie nowego niestandardowy filtr akcji</span><span class="sxs-lookup"><span data-stu-id="e5461-222">Task 1: Creating a new Custom Action Filter</span></span>

<span data-ttu-id="e5461-223">W tym zadaniu utworzysz nowy niestandardowy filtr akcji można wstawić do klasy StoreController, jak zarządzać kolejność wykonywania filtrów.</span><span class="sxs-lookup"><span data-stu-id="e5461-223">In this task, you will create a new Custom Action Filter to inject into the StoreController class, learning how to manage the execution order of the filters.</span></span>

1. <span data-ttu-id="e5461-224">Otwórz **rozpocząć** rozwiązania znajdujący się w **\Source\Ex02-ManagingMultipleActionFilters\Begin** folderu.</span><span class="sxs-lookup"><span data-stu-id="e5461-224">Open the **Begin** solution located at **\Source\Ex02-ManagingMultipleActionFilters\Begin** folder.</span></span> <span data-ttu-id="e5461-225">W przeciwnym razie będziesz nadal korzystać z **zakończenia** rozwiązania uzyskać, wykonując poprzednim ćwiczeniu.</span><span class="sxs-lookup"><span data-stu-id="e5461-225">Otherwise, you might continue using the **End** solution obtained by completing the previous exercise.</span></span>

    1. <span data-ttu-id="e5461-226">Jeśli został otwarty dołączonym **rozpocząć** rozwiązania, należy pobrać niektóre brakujące pakiety NuGet przed kontynuowaniem.</span><span class="sxs-lookup"><span data-stu-id="e5461-226">If you opened the provided **Begin** solution, you will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="e5461-227">Aby to zrobić, kliknij przycisk **projektu** menu, a następnie wybierz **Zarządzaj pakietami NuGet**.</span><span class="sxs-lookup"><span data-stu-id="e5461-227">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
    2. <span data-ttu-id="e5461-228">W **Zarządzaj pakietami NuGet** okno dialogowe, kliknij przycisk **przywrócić** Aby pobrać brakujące pakiety.</span><span class="sxs-lookup"><span data-stu-id="e5461-228">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
    3. <span data-ttu-id="e5461-229">Na koniec Skompiluj rozwiązanie, klikając **kompilacji** | **Kompiluj rozwiązanie**.</span><span class="sxs-lookup"><span data-stu-id="e5461-229">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

        > [!NOTE]
        > <span data-ttu-id="e5461-230">Jedną z zalet za pomocą narzędzia NuGet jest, że nie masz do wysłania wszystkie biblioteki w projekcie, zmniejszenie rozmiaru projektu.</span><span class="sxs-lookup"><span data-stu-id="e5461-230">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="e5461-231">Za pomocą narzędzi NuGet Power Tools, określając wersji pakietu w pliku Packages.config można pobrać wymaganych bibliotek podczas pierwszego uruchomienia projektu.</span><span class="sxs-lookup"><span data-stu-id="e5461-231">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="e5461-232">Jest to, dlaczego należy uruchomić następujące kroki, po otwarciu istniejącego rozwiązania, w tym środowisku laboratoryjnym.</span><span class="sxs-lookup"><span data-stu-id="e5461-232">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
        > 
        > <span data-ttu-id="e5461-233">Aby uzyskać więcej informacji znajduje się w artykule: [ http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages ](http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages).</span><span class="sxs-lookup"><span data-stu-id="e5461-233">For more information, see this article: [http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages](http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages).</span></span>
2. <span data-ttu-id="e5461-234">Dodaj nową klasę C# do **filtry** folder i nadaj mu nazwę *MyNewCustomActionFilter.cs*</span><span class="sxs-lookup"><span data-stu-id="e5461-234">Add a new C# class into the **Filters** folder and name it *MyNewCustomActionFilter.cs*</span></span>
3. <span data-ttu-id="e5461-235">Otwórz **MyNewCustomActionFilter.cs** i Dodaj odwołanie do **System.Web.Mvc** i **MvcMusicStore.Models** przestrzeni nazw:</span><span class="sxs-lookup"><span data-stu-id="e5461-235">Open **MyNewCustomActionFilter.cs** and add a reference to **System.Web.Mvc** and the **MvcMusicStore.Models** namespace:</span></span>

    <span data-ttu-id="e5461-236">(Code Snippet — *filtry akcji niestandardowej platformy ASP.NET MVC 4 — Ex2 MyNewCustomActionFilterNamespaces*)</span><span class="sxs-lookup"><span data-stu-id="e5461-236">(Code Snippet - *ASP.NET MVC 4 Custom Action Filters - Ex2-MyNewCustomActionFilterNamespaces*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample8.cs)]
4. <span data-ttu-id="e5461-237">Zastąp domyślną deklaracji klasy z następującym kodem.</span><span class="sxs-lookup"><span data-stu-id="e5461-237">Replace the default class declaration with the following code.</span></span>

    <span data-ttu-id="e5461-238">(Code Snippet — *filtry akcji niestandardowej platformy ASP.NET MVC 4 — Ex2 MyNewCustomActionFilterClass*)</span><span class="sxs-lookup"><span data-stu-id="e5461-238">(Code Snippet - *ASP.NET MVC 4 Custom Action Filters - Ex2-MyNewCustomActionFilterClass*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample9.cs)]

    > [!NOTE]
    > <span data-ttu-id="e5461-239">Ten niestandardowy filtr akcji jest prawie taki sam, niż ten, który został utworzony w poprzednim ćwiczeniu.</span><span class="sxs-lookup"><span data-stu-id="e5461-239">This Custom Action Filter is almost the same than the one you created in the previous exercise.</span></span> <span data-ttu-id="e5461-240">Główną różnicą jest to, że ma on *&quot;rejestrowane przez&quot;* zaktualizowany o nazwie tej nowej klasy, aby zidentyfikować filtr wystąpiły atrybut zarejestrowany dziennika.</span><span class="sxs-lookup"><span data-stu-id="e5461-240">The main difference is that it has the *&quot;Logged By&quot;* attribute updated with this new class' name to identify wich filter registered the log.</span></span>

<a id="Ex2Task2"></a>

<a id="Task_2_Injecting_a_new_Code_Interceptor_into_the_StoreController_Class"></a>
#### <a name="task-2-injecting-a-new-code-interceptor-into-the-storecontroller-class"></a><span data-ttu-id="e5461-241">Zadanie 2: Wprowadzanie nowych Interceptor kod do klasy StoreController</span><span class="sxs-lookup"><span data-stu-id="e5461-241">Task 2: Injecting a new Code Interceptor into the StoreController Class</span></span>

<span data-ttu-id="e5461-242">W tym zadaniu zostanie dodaje nowy filtr niestandardowy do klasy StoreController i uruchomić rozwiązanie, aby sprawdzić, jak w przypadku obu filtrów współpracują ze sobą.</span><span class="sxs-lookup"><span data-stu-id="e5461-242">In this task, you will add a new custom filter into the StoreController Class and run the solution to verify how both filters work together.</span></span>

1. <span data-ttu-id="e5461-243">Otwórz **StoreController** klasy znajdujący się w **MvcMusicStore\Controllers** i wstawić nowy filtr niestandardowy **MyNewCustomActionFilter** do  **StoreController** klasy, jak przedstawiono w poniższym kodzie.</span><span class="sxs-lookup"><span data-stu-id="e5461-243">Open the **StoreController** class located at **MvcMusicStore\Controllers** and inject the new custom filter **MyNewCustomActionFilter** into **StoreController** class like is shown in the following code.</span></span>

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample10.cs)]
2. <span data-ttu-id="e5461-244">Teraz uruchom aplikację, aby zobaczyć, jak działają te dwa filtry akcji niestandardowych.</span><span class="sxs-lookup"><span data-stu-id="e5461-244">Now, run the application in order to see how these two Custom Action Filters work.</span></span> <span data-ttu-id="e5461-245">Aby to zrobić, naciśnij klawisz **F5** i zaczekaj, aż uruchamiania aplikacji.</span><span class="sxs-lookup"><span data-stu-id="e5461-245">To do this, press **F5** and wait until the application starts.</span></span>
3. <span data-ttu-id="e5461-246">Przejdź do **/ActionLog** aby zobaczyć stan początkowy widok dziennika.</span><span class="sxs-lookup"><span data-stu-id="e5461-246">Browse to **/ActionLog** to see log view initial state.</span></span>

    <span data-ttu-id="e5461-247">![Rejestrowanie śledzenia stanu przed działaniem strony](aspnet-mvc-4-custom-action-filters/_static/image5.png "rejestrowania śledzenia stanu przed na pager")</span><span class="sxs-lookup"><span data-stu-id="e5461-247">![Log tracker status before page activity](aspnet-mvc-4-custom-action-filters/_static/image5.png "Log tracker status before page activity")</span></span>

    <span data-ttu-id="e5461-248">*Dziennika śledzenia stanu przed na pager*</span><span class="sxs-lookup"><span data-stu-id="e5461-248">*Log tracker status before page activity*</span></span>
4. <span data-ttu-id="e5461-249">Kliknij jedną z **gatunki** z menu i wykonywać niektórych akcji, takich jak przeglądanie dostępnych albumu.</span><span class="sxs-lookup"><span data-stu-id="e5461-249">Click one of the **Genres** from the menu and perform some actions there, like browsing an available album.</span></span>
5. <span data-ttu-id="e5461-250">Sprawdź, czy ten czas; wizyty były śledzone dwa razy: po każdej niestandardowe filtry akcji dodane w **kontroler magazynu** klasy.</span><span class="sxs-lookup"><span data-stu-id="e5461-250">Check that this time; your visits were tracked twice: once for each of the Custom Action Filters you added in the **StorageController** class.</span></span>

    <span data-ttu-id="e5461-251">![Dziennik akcji za pomocą działania rejestrowane](aspnet-mvc-4-custom-action-filters/_static/image6.png "dziennik akcji za pomocą działania rejestrowane")</span><span class="sxs-lookup"><span data-stu-id="e5461-251">![Action log with activity logged](aspnet-mvc-4-custom-action-filters/_static/image6.png "Action log with activity logged")</span></span>

    <span data-ttu-id="e5461-252">*Dziennik akcji za pomocą działania rejestrowane*</span><span class="sxs-lookup"><span data-stu-id="e5461-252">*Action log with activity logged*</span></span>
6. <span data-ttu-id="e5461-253">Zamknij przeglądarkę.</span><span class="sxs-lookup"><span data-stu-id="e5461-253">Close the browser.</span></span>

<a id="Ex2Task3"></a>

<a id="Task_3_Managing_Filter_Ordering"></a>
#### <a name="task-3-managing-filter-ordering"></a><span data-ttu-id="e5461-254">Zadanie 3: Zarządzanie, kolejności filtru</span><span class="sxs-lookup"><span data-stu-id="e5461-254">Task 3: Managing Filter Ordering</span></span>

<span data-ttu-id="e5461-255">W tym zadaniu dowiesz się, jak zarządzać kolejność wykonywania filtrów przy użyciu podawany zamówienia.</span><span class="sxs-lookup"><span data-stu-id="e5461-255">In this task, you will learn how to manage the filters' execution order by using the Order propery.</span></span>

1. <span data-ttu-id="e5461-256">Otwórz **StoreController** klasy znajdujący się w **MvcMusicStore\Controllers** i określ **kolejności** właściwości w obu filtrów, takich jak pokazano poniżej.</span><span class="sxs-lookup"><span data-stu-id="e5461-256">Open the **StoreController** class located at **MvcMusicStore\Controllers** and specify the **Order** property in both filters like shown below.</span></span>

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample11.cs)]
2. <span data-ttu-id="e5461-257">Teraz Sprawdź, jak filtry są wykonywane w zależności od wartości właściwości jego zamówienia.</span><span class="sxs-lookup"><span data-stu-id="e5461-257">Now, verify how the filters are executed depending on its Order property's value.</span></span> <span data-ttu-id="e5461-258">Będzie zauważysz, że filtr mający najmniejszą wartość kolejności (**CustomActionFilter**) jest to pierwsza, który jest wykonywany.</span><span class="sxs-lookup"><span data-stu-id="e5461-258">You will find that the filter with the smallest Order value (**CustomActionFilter**) is the first one that is executed.</span></span> <span data-ttu-id="e5461-259">Naciśnij klawisz **F5** i zaczekaj, aż uruchamiania aplikacji.</span><span class="sxs-lookup"><span data-stu-id="e5461-259">Press **F5** and wait until the application starts.</span></span>
3. <span data-ttu-id="e5461-260">Przejdź do **/ActionLog** aby zobaczyć stan początkowy widok dziennika.</span><span class="sxs-lookup"><span data-stu-id="e5461-260">Browse to **/ActionLog** to see log view initial state.</span></span>

    <span data-ttu-id="e5461-261">![Rejestrowanie śledzenia stanu przed działaniem strony](aspnet-mvc-4-custom-action-filters/_static/image7.png "rejestrowania śledzenia stanu przed na pager")</span><span class="sxs-lookup"><span data-stu-id="e5461-261">![Log tracker status before page activity](aspnet-mvc-4-custom-action-filters/_static/image7.png "Log tracker status before page activity")</span></span>

    <span data-ttu-id="e5461-262">*Dziennika śledzenia stanu przed na pager*</span><span class="sxs-lookup"><span data-stu-id="e5461-262">*Log tracker status before page activity*</span></span>
4. <span data-ttu-id="e5461-263">Kliknij jedną z **gatunki** z menu i wykonywać niektórych akcji, takich jak przeglądanie dostępnych albumu.</span><span class="sxs-lookup"><span data-stu-id="e5461-263">Click one of the **Genres** from the menu and perform some actions there, like browsing an available album.</span></span>
5. <span data-ttu-id="e5461-264">Sprawdź, czy ten czas wizyty były śledzone uporządkowane według wartości kolejności filtrów: **CustomActionFilter** dzienniki pierwszy.</span><span class="sxs-lookup"><span data-stu-id="e5461-264">Check that this time, your visits were tracked ordered by the filters' Order value: **CustomActionFilter** logs' first.</span></span>

    <span data-ttu-id="e5461-265">![Dziennik akcji za pomocą działania rejestrowane](aspnet-mvc-4-custom-action-filters/_static/image8.png "dziennik akcji za pomocą działania rejestrowane")</span><span class="sxs-lookup"><span data-stu-id="e5461-265">![Action log with activity logged](aspnet-mvc-4-custom-action-filters/_static/image8.png "Action log with activity logged")</span></span>

    <span data-ttu-id="e5461-266">*Dziennik akcji za pomocą działania rejestrowane*</span><span class="sxs-lookup"><span data-stu-id="e5461-266">*Action log with activity logged*</span></span>
6. <span data-ttu-id="e5461-267">Będzie teraz zaktualizować wartość kolejności filtrów i sprawdź, jak zmienia kolejność rejestrowania.</span><span class="sxs-lookup"><span data-stu-id="e5461-267">Now, you will update the Filters' order value and verify how the logging order changes.</span></span> <span data-ttu-id="e5461-268">W **StoreController** klasy, zaktualizuj wartość kolejności filtrów, jak pokazano poniżej.</span><span class="sxs-lookup"><span data-stu-id="e5461-268">In the **StoreController** class, update the Filters' Order value like shown below.</span></span>

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample12.cs)]
7. <span data-ttu-id="e5461-269">Uruchom aplikację ponownie, naciskając klawisz **F5**.</span><span class="sxs-lookup"><span data-stu-id="e5461-269">Run the application again by pressing **F5**.</span></span>
8. <span data-ttu-id="e5461-270">Kliknij jedną z **gatunki** z menu i wykonywać niektórych akcji, takich jak przeglądanie dostępnych albumu.</span><span class="sxs-lookup"><span data-stu-id="e5461-270">Click one of the **Genres** from the menu and perform some actions there, like browsing an available album.</span></span>
9. <span data-ttu-id="e5461-271">Sprawdź, czy ten czas dzienniki utworzone w wyniku **MyNewCustomActionFilter** występuje jako pierwszy filtr.</span><span class="sxs-lookup"><span data-stu-id="e5461-271">Check that this time, the logs created by **MyNewCustomActionFilter** filter appears first.</span></span>

    <span data-ttu-id="e5461-272">![Dziennik akcji za pomocą działania rejestrowane](aspnet-mvc-4-custom-action-filters/_static/image9.png "dziennik akcji za pomocą działania rejestrowane")</span><span class="sxs-lookup"><span data-stu-id="e5461-272">![Action log with activity logged](aspnet-mvc-4-custom-action-filters/_static/image9.png "Action log with activity logged")</span></span>

    <span data-ttu-id="e5461-273">*Dziennik akcji za pomocą działania rejestrowane*</span><span class="sxs-lookup"><span data-stu-id="e5461-273">*Action log with activity logged*</span></span>

<a id="Ex2Task4"></a>

<a id="Task_4_Registering_Filters_Globally"></a>
#### <a name="task-4-registering-filters-globally"></a><span data-ttu-id="e5461-274">Zadanie 4: Rejestrowanie filtry globalnie</span><span class="sxs-lookup"><span data-stu-id="e5461-274">Task 4: Registering Filters Globally</span></span>

<span data-ttu-id="e5461-275">To zadanie zaktualizuje rozwiązanie, aby zarejestrować nowy filtr (**MyNewCustomActionFilter**) jako filtrów globalnych.</span><span class="sxs-lookup"><span data-stu-id="e5461-275">In this task, you will update the solution to register the new filter (**MyNewCustomActionFilter**) as a global filter.</span></span> <span data-ttu-id="e5461-276">W ten sposób zostanie wyzwolone przez wszystkie zestawem akcji w aplikacji i nie tylko w StoreController z nich, tak jak w poprzednim zadaniu.</span><span class="sxs-lookup"><span data-stu-id="e5461-276">By doing this, it will be triggered by all the actions perfomed in the application and not only in the StoreController ones as in the previous task.</span></span>

1. <span data-ttu-id="e5461-277">W **StoreController** klasy, należy usunąć **[MyNewCustomActionFilter]** atrybut i właściwość kolejności **[CustomActionFilter]**.</span><span class="sxs-lookup"><span data-stu-id="e5461-277">In **StoreController** class, remove **[MyNewCustomActionFilter]** attribute and the order property from **[CustomActionFilter]**.</span></span> <span data-ttu-id="e5461-278">Jego powinien wyglądać następująco:</span><span class="sxs-lookup"><span data-stu-id="e5461-278">It should look like the following:</span></span>

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample13.cs)]
2. <span data-ttu-id="e5461-279">Otwórz **Global.asax** pliku, a następnie zlokalizuj **aplikacji\_Start** metody.</span><span class="sxs-lookup"><span data-stu-id="e5461-279">Open **Global.asax** file and locate the **Application\_Start** method.</span></span> <span data-ttu-id="e5461-280">Zwróć uwagę, że każdorazowo aplikacja uruchamia go rejestruje filtry globalne, wywołując **RegisterGlobalFilters** metodę w ramach **FilterConfig** klasy.</span><span class="sxs-lookup"><span data-stu-id="e5461-280">Notice that each time the application starts it is registering the global filters by calling **RegisterGlobalFilters** method within **FilterConfig** class.</span></span>

    <span data-ttu-id="e5461-281">![Rejestrowanie filtry globalne w pliku Global.asax](aspnet-mvc-4-custom-action-filters/_static/image10.png "rejestrowanie filtry globalne w pliku Global.asax")</span><span class="sxs-lookup"><span data-stu-id="e5461-281">![Registering Global Filters in Global.asax](aspnet-mvc-4-custom-action-filters/_static/image10.png "Registering Global Filters in Global.asax")</span></span>

    <span data-ttu-id="e5461-282">*Rejestrowanie filtry globalne w pliku Global.asax*</span><span class="sxs-lookup"><span data-stu-id="e5461-282">*Registering Global Filters in Global.asax*</span></span>
3. <span data-ttu-id="e5461-283">Otwórz **FilterConfig.cs** plików w ramach **aplikacji\_Start** folderu.</span><span class="sxs-lookup"><span data-stu-id="e5461-283">Open **FilterConfig.cs** file within **App\_Start** folder.</span></span>
4. <span data-ttu-id="e5461-284">Dodaj odwołanie do przy użyciu System.Web.Mvc; za pomocą MvcMusicStore.Filters; przestrzeń nazw.</span><span class="sxs-lookup"><span data-stu-id="e5461-284">Add a reference to using System.Web.Mvc; using MvcMusicStore.Filters; namespace.</span></span>

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample14.cs)]
5. <span data-ttu-id="e5461-285">Aktualizacja **RegisterGlobalFilters** metody dodawania niestandardowego filtru.</span><span class="sxs-lookup"><span data-stu-id="e5461-285">Update **RegisterGlobalFilters** method adding your custom filter.</span></span> <span data-ttu-id="e5461-286">Aby to zrobić, Dodaj wyróżniony kod:</span><span class="sxs-lookup"><span data-stu-id="e5461-286">To do this, add the highlighted code:</span></span>

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample15.cs)]
6. <span data-ttu-id="e5461-287">Uruchom aplikację, naciskając klawisz **F5**.</span><span class="sxs-lookup"><span data-stu-id="e5461-287">Run the application by pressing **F5**.</span></span>
7. <span data-ttu-id="e5461-288">Kliknij jedną z **gatunki** z menu i wykonywać niektórych akcji, takich jak przeglądanie dostępnych albumu.</span><span class="sxs-lookup"><span data-stu-id="e5461-288">Click one of the **Genres** from the menu and perform some actions there, like browsing an available album.</span></span>
8. <span data-ttu-id="e5461-289">Sprawdź, że teraz **[MyNewCustomActionFilter]** jest on zbyt wprowadzony w HomeController i ActionLogController.</span><span class="sxs-lookup"><span data-stu-id="e5461-289">Check that now **[MyNewCustomActionFilter]** is being injected in HomeController and ActionLogController too.</span></span>

    <span data-ttu-id="e5461-290">![Dziennik akcji za pomocą działania rejestrowane](aspnet-mvc-4-custom-action-filters/_static/image11.png "dziennik akcji za pomocą działania rejestrowane")</span><span class="sxs-lookup"><span data-stu-id="e5461-290">![Action log with activity logged](aspnet-mvc-4-custom-action-filters/_static/image11.png "Action log with activity logged")</span></span>

    <span data-ttu-id="e5461-291">*Dziennik akcji za pomocą działania globalnego rejestrowane*</span><span class="sxs-lookup"><span data-stu-id="e5461-291">*Action log with global activity logged*</span></span>

> [!NOTE]
> <span data-ttu-id="e5461-292">Ponadto można wdrożyć tę aplikację do witryny sieci Web systemu Windows Azure następujące [dodatek B: publikowania aplikacji ASP.NET MVC 4 za pomocą narzędzia Web Deploy](#AppendixB).</span><span class="sxs-lookup"><span data-stu-id="e5461-292">Additionally, you can deploy this application to Windows Azure Web Sites following [Appendix B: Publishing an ASP.NET MVC 4 Application using Web Deploy](#AppendixB).</span></span>


* * *

<a id="Summary"></a>

<a id="Summary"></a>
## <a name="summary"></a><span data-ttu-id="e5461-293">Podsumowanie</span><span class="sxs-lookup"><span data-stu-id="e5461-293">Summary</span></span>

<span data-ttu-id="e5461-294">Przez ukończenie tego laboratorium praktyczne mają pokazaliśmy, jak rozszerzyć filtr akcji do wykonania akcji niestandardowej.</span><span class="sxs-lookup"><span data-stu-id="e5461-294">By completing this Hands-On Lab you have learned how to extend an action filter to execute custom actions.</span></span> <span data-ttu-id="e5461-295">Ma również pokazaliśmy, jak wprowadzić dowolny filtr z kontrolerami strony.</span><span class="sxs-lookup"><span data-stu-id="e5461-295">You have also learned how to inject any filter to your page controllers.</span></span> <span data-ttu-id="e5461-296">Były używane następujące pojęcia:</span><span class="sxs-lookup"><span data-stu-id="e5461-296">The following concepts were used:</span></span>

- <span data-ttu-id="e5461-297">Jak utworzyć filtry akcji niestandardowej z klasy ASP.NET MVC ActionFilterAttribute</span><span class="sxs-lookup"><span data-stu-id="e5461-297">How to create Custom Action filters with the ASP.NET MVC ActionFilterAttribute class</span></span>
- <span data-ttu-id="e5461-298">Jak wstawić filtry do kontrolery ASP.NET MVC</span><span class="sxs-lookup"><span data-stu-id="e5461-298">How to inject filters into ASP.NET MVC controllers</span></span>
- <span data-ttu-id="e5461-299">Jak zarządzać filtru kolejność przy użyciu właściwości Order</span><span class="sxs-lookup"><span data-stu-id="e5461-299">How to manage filter ordering using the Order property</span></span>
- <span data-ttu-id="e5461-300">Jak zarejestrować filtry globalnie</span><span class="sxs-lookup"><span data-stu-id="e5461-300">How to register filters globally</span></span>

<a id="AppendixA"></a>

<a id="Appendix_A_Installing_Visual_Studio_Express_2012_for_Web"></a>
## <a name="appendix-a-installing-visual-studio-express-2012-for-web"></a><span data-ttu-id="e5461-301">Dodatek A: Instalowanie programu Visual Studio Express 2012 for Web</span><span class="sxs-lookup"><span data-stu-id="e5461-301">Appendix A: Installing Visual Studio Express 2012 for Web</span></span>

<span data-ttu-id="e5461-302">Możesz zainstalować **programu Microsoft Visual Studio Express 2012 for Web** lub inne &quot;Express&quot; przy użyciu wersji **[Instalatora platformy sieci Web firmy Microsoft](https://www.microsoft.com/web/downloads/platform.aspx)**.</span><span class="sxs-lookup"><span data-stu-id="e5461-302">You can install **Microsoft Visual Studio Express 2012 for Web** or another &quot;Express&quot; version using the **[Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)**.</span></span> <span data-ttu-id="e5461-303">Poniższe wskazówki ułatwiają kroki wymagane do zainstalowania *programu Visual studio Express 2012 for Web* przy użyciu *Instalatora platformy sieci Web firmy Microsoft*.</span><span class="sxs-lookup"><span data-stu-id="e5461-303">The following instructions guide you through the steps required to install *Visual studio Express 2012 for Web* using *Microsoft Web Platform Installer*.</span></span>

1. <span data-ttu-id="e5461-304">Przejdź do [ [ https://go.microsoft.com/? linkid = 9810169](https://go.microsoft.com/?linkid=9810169)](https://go.microsoft.com/?linkid=9810169).</span><span class="sxs-lookup"><span data-stu-id="e5461-304">Go to [[https://go.microsoft.com/? linkid=9810169](https://go.microsoft.com/?linkid=9810169)](https://go.microsoft.com/?linkid=9810169).</span></span> <span data-ttu-id="e5461-305">Alternatywnie, jeśli już zainstalowano Instalatora platformy sieci Web, możesz otworzyć go i Wyszukaj produkt &quot; <em>Visual Studio Express 2012 for Web z zestawem Windows Azure SDK</em>&quot;.</span><span class="sxs-lookup"><span data-stu-id="e5461-305">Alternatively, if you already have installed Web Platform Installer, you can open it and search for the product &quot;<em>Visual Studio Express 2012 for Web with Windows Azure SDK</em>&quot;.</span></span>
2. <span data-ttu-id="e5461-306">Kliknij pozycję **Zainstaluj teraz**.</span><span class="sxs-lookup"><span data-stu-id="e5461-306">Click on **Install Now**.</span></span> <span data-ttu-id="e5461-307">Jeśli nie masz **Instalatora platformy sieci Web** nastąpi przekierowanie do pobrania i zainstalowania go najpierw.</span><span class="sxs-lookup"><span data-stu-id="e5461-307">If you do not have **Web Platform Installer** you will be redirected to download and install it first.</span></span>
3. <span data-ttu-id="e5461-308">Raz **Instalatora platformy sieci Web** jest otwarty, kliknij przycisk **zainstalować** można uruchomić Instalatora.</span><span class="sxs-lookup"><span data-stu-id="e5461-308">Once **Web Platform Installer** is open, click **Install** to start the setup.</span></span>

    <span data-ttu-id="e5461-309">![Zainstaluj program Visual Studio Express](aspnet-mvc-4-custom-action-filters/_static/image12.png "zainstalować program Visual Studio Express")</span><span class="sxs-lookup"><span data-stu-id="e5461-309">![Install Visual Studio Express](aspnet-mvc-4-custom-action-filters/_static/image12.png "Install Visual Studio Express")</span></span>

    <span data-ttu-id="e5461-310">*Zainstaluj program Visual Studio Express*</span><span class="sxs-lookup"><span data-stu-id="e5461-310">*Install Visual Studio Express*</span></span>
4. <span data-ttu-id="e5461-311">Odczytywanie wszystkich produktów licencji oraz warunki, a następnie kliknij przycisk **akceptuję** aby kontynuować.</span><span class="sxs-lookup"><span data-stu-id="e5461-311">Read all the products' licenses and terms and click **I Accept** to continue.</span></span>

    ![Akceptowanie umowy licencyjnej](aspnet-mvc-4-custom-action-filters/_static/image13.png)

    <span data-ttu-id="e5461-313">*Akceptowanie umowy licencyjnej*</span><span class="sxs-lookup"><span data-stu-id="e5461-313">*Accepting the license terms*</span></span>
5. <span data-ttu-id="e5461-314">Zaczekaj, aż do zakończenia procesu pobierania i instalacji.</span><span class="sxs-lookup"><span data-stu-id="e5461-314">Wait until the downloading and installation process completes.</span></span>

    ![Postęp instalacji](aspnet-mvc-4-custom-action-filters/_static/image14.png)

    <span data-ttu-id="e5461-316">*Postęp instalacji*</span><span class="sxs-lookup"><span data-stu-id="e5461-316">*Installation progress*</span></span>
6. <span data-ttu-id="e5461-317">Po zakończeniu instalacji kliknij przycisk **Zakończ**.</span><span class="sxs-lookup"><span data-stu-id="e5461-317">When the installation completes, click **Finish**.</span></span>

    ![Instalacja została zakończona](aspnet-mvc-4-custom-action-filters/_static/image15.png)

    <span data-ttu-id="e5461-319">*Instalacja została zakończona*</span><span class="sxs-lookup"><span data-stu-id="e5461-319">*Installation completed*</span></span>
7. <span data-ttu-id="e5461-320">Kliknij przycisk **zakończenia** zamknąć Instalatora platformy sieci Web.</span><span class="sxs-lookup"><span data-stu-id="e5461-320">Click **Exit** to close Web Platform Installer.</span></span>
8. <span data-ttu-id="e5461-321">Aby otworzyć program Visual Studio Express for Web, przejdź do **Start** ekranu, a następnie zacznij pisać &quot; **VS Express**&quot;, następnie kliknij pozycję **VS Express for Web** Kafelek.</span><span class="sxs-lookup"><span data-stu-id="e5461-321">To open Visual Studio Express for Web, go to the **Start** screen and start writing &quot;**VS Express**&quot;, then click on the **VS Express for Web** tile.</span></span>

    ![VS Express for Web kafelka](aspnet-mvc-4-custom-action-filters/_static/image16.png)

    <span data-ttu-id="e5461-323">*VS Express for Web kafelka*</span><span class="sxs-lookup"><span data-stu-id="e5461-323">*VS Express for Web tile*</span></span>

<a id="AppendixB"></a>

<a id="Appendix_B_Publishing_an_ASPNET_MVC_4_Application_using_Web_Deploy"></a>
## <a name="appendix-b-publishing-an-aspnet-mvc-4-application-using-web-deploy"></a><span data-ttu-id="e5461-324">Dodatek B: publikowania aplikacji ASP.NET MVC 4 za pomocą narzędzia Web Deploy</span><span class="sxs-lookup"><span data-stu-id="e5461-324">Appendix B: Publishing an ASP.NET MVC 4 Application using Web Deploy</span></span>

<span data-ttu-id="e5461-325">Ten dodatek będzie pokazują, jak utworzyć nową witrynę sieci web w portalu zarządzania pakietu Windows Azure i publikowanie aplikacji, uzyskany postępując zgodnie z laboratorium, korzystając z zalet funkcji publikowania narzędzia Web Deploy oferowanego przez system Windows Azure.</span><span class="sxs-lookup"><span data-stu-id="e5461-325">This appendix will show you how to create a new web site from the Windows Azure Management Portal and publish the application you obtained by following the lab, taking advantage of the Web Deploy publishing feature provided by Windows Azure.</span></span>

<a id="ApxBTask1"></a>

<a id="Task_1_-_Creating_a_New_Web_Site_from_the_Windows_Azure_Portal"></a>
#### <a name="task-1---creating-a-new-web-site-from-the-windows-azure-portal"></a><span data-ttu-id="e5461-326">Zadanie 1. Tworzenie nowej witryny sieci Web z Windows Azure Portal</span><span class="sxs-lookup"><span data-stu-id="e5461-326">Task 1 - Creating a New Web Site from the Windows Azure Portal</span></span>

1. <span data-ttu-id="e5461-327">Przejdź do [portalu zarządzania pakietu Windows Azure](https://manage.windowsazure.com/) i zaloguj się przy użyciu poświadczeń firmy Microsoft, powiązaną z Twoją subskrypcją.</span><span class="sxs-lookup"><span data-stu-id="e5461-327">Go to the [Windows Azure Management Portal](https://manage.windowsazure.com/) and sign in using the Microsoft credentials associated with your subscription.</span></span>

    > [!NOTE]
    > <span data-ttu-id="e5461-328">Platforma Windows Azure można bezpłatny hosting 10 witryn sieci Web platformy ASP.NET i skalowanie w miarę wzrostu ruchu.</span><span class="sxs-lookup"><span data-stu-id="e5461-328">With Windows Azure you can host 10 ASP.NET Web Sites for free and then scale as your traffic grows.</span></span> <span data-ttu-id="e5461-329">Możesz zarejestrować się [tutaj](http://aka.ms/aspnet-hol-azure).</span><span class="sxs-lookup"><span data-stu-id="e5461-329">You can sign up [here](http://aka.ms/aspnet-hol-azure).</span></span>

    <span data-ttu-id="e5461-330">![Zaloguj się do portalu usługi Windows Azure](aspnet-mvc-4-custom-action-filters/_static/image17.png "Zaloguj się do portalu usługi Windows Azure")</span><span class="sxs-lookup"><span data-stu-id="e5461-330">![Log on to Windows Azure portal](aspnet-mvc-4-custom-action-filters/_static/image17.png "Log on to Windows Azure portal")</span></span>

    <span data-ttu-id="e5461-331">*Zaloguj się do portalu zarządzania systemu Azure Windows*</span><span class="sxs-lookup"><span data-stu-id="e5461-331">*Log on to Windows Azure Management Portal*</span></span>
2. <span data-ttu-id="e5461-332">Kliknij przycisk **New** na pasku poleceń.</span><span class="sxs-lookup"><span data-stu-id="e5461-332">Click **New** on the command bar.</span></span>

    <span data-ttu-id="e5461-333">![Tworzenie nowej witryny sieci Web](aspnet-mvc-4-custom-action-filters/_static/image18.png "tworzenia nowej witryny sieci Web")</span><span class="sxs-lookup"><span data-stu-id="e5461-333">![Creating a new Web Site](aspnet-mvc-4-custom-action-filters/_static/image18.png "Creating a new Web Site")</span></span>

    <span data-ttu-id="e5461-334">*Tworzenie nowej witryny sieci Web*</span><span class="sxs-lookup"><span data-stu-id="e5461-334">*Creating a new Web Site*</span></span>
3. <span data-ttu-id="e5461-335">Kliknij przycisk **obliczenia** | **witryny sieci Web**.</span><span class="sxs-lookup"><span data-stu-id="e5461-335">Click **Compute** | **Web Site**.</span></span> <span data-ttu-id="e5461-336">Następnie wybierz pozycję **szybkie tworzenie** opcji.</span><span class="sxs-lookup"><span data-stu-id="e5461-336">Then select **Quick Create** option.</span></span> <span data-ttu-id="e5461-337">Podaj adres URL dostępny dla nowej witryny sieci web, a następnie kliknij przycisk **Utwórz witrynę sieci Web**.</span><span class="sxs-lookup"><span data-stu-id="e5461-337">Provide an available URL for the new web site and click **Create Web Site**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="e5461-338">Witryny sieci Web Windows Azure jest hostem dla aplikacji sieci web działające w chmurze, które można kontrolować i zarządzać nimi.</span><span class="sxs-lookup"><span data-stu-id="e5461-338">A Windows Azure Web Site is the host for a web application running in the cloud that you can control and manage.</span></span> <span data-ttu-id="e5461-339">Opcja szybkie tworzenie umożliwia wdrażanie ukończonej aplikacji sieci web do Windows Azure witryny sieci Web z poza portalem.</span><span class="sxs-lookup"><span data-stu-id="e5461-339">The Quick Create option allows you to deploy a completed web application to the Windows Azure Web Site from outside the portal.</span></span> <span data-ttu-id="e5461-340">Nie obejmuje kroki konfigurowania bazy danych.</span><span class="sxs-lookup"><span data-stu-id="e5461-340">It does not include steps for setting up a database.</span></span>

    <span data-ttu-id="e5461-341">![Tworzenie nowej witryny sieci Web przy użyciu szybkiego tworzenia](aspnet-mvc-4-custom-action-filters/_static/image19.png "tworzenia nowej witryny sieci Web przy użyciu szybkie tworzenie")</span><span class="sxs-lookup"><span data-stu-id="e5461-341">![Creating a new Web Site using Quick Create](aspnet-mvc-4-custom-action-filters/_static/image19.png "Creating a new Web Site using Quick Create")</span></span>

    <span data-ttu-id="e5461-342">*Tworzenie nowej witryny sieci Web przy użyciu szybkie tworzenie*</span><span class="sxs-lookup"><span data-stu-id="e5461-342">*Creating a new Web Site using Quick Create*</span></span>
4. <span data-ttu-id="e5461-343">Poczekaj, aż nowe **witryny sieci Web** zostanie utworzony.</span><span class="sxs-lookup"><span data-stu-id="e5461-343">Wait until the new **Web Site** is created.</span></span>
5. <span data-ttu-id="e5461-344">Po utworzeniu witryny sieci Web kliknij link w obszarze **adresu URL** kolumny.</span><span class="sxs-lookup"><span data-stu-id="e5461-344">Once the Web Site is created click the link under the **URL** column.</span></span> <span data-ttu-id="e5461-345">Sprawdź, czy działa nową witrynę sieci Web.</span><span class="sxs-lookup"><span data-stu-id="e5461-345">Check that the new Web Site is working.</span></span>

    <span data-ttu-id="e5461-346">![Przejście do nowej witryny sieci web](aspnet-mvc-4-custom-action-filters/_static/image20.png "przejście do nowej witryny sieci web")</span><span class="sxs-lookup"><span data-stu-id="e5461-346">![Browsing to the new web site](aspnet-mvc-4-custom-action-filters/_static/image20.png "Browsing to the new web site")</span></span>

    <span data-ttu-id="e5461-347">*Przejście do nowej witryny sieci web*</span><span class="sxs-lookup"><span data-stu-id="e5461-347">*Browsing to the new web site*</span></span>

    <span data-ttu-id="e5461-348">![Witryna sieci Web działa](aspnet-mvc-4-custom-action-filters/_static/image21.png "witryna sieci Web działa")</span><span class="sxs-lookup"><span data-stu-id="e5461-348">![Web site running](aspnet-mvc-4-custom-action-filters/_static/image21.png "Web site running")</span></span>

    <span data-ttu-id="e5461-349">*Witryna sieci Web działa*</span><span class="sxs-lookup"><span data-stu-id="e5461-349">*Web site running*</span></span>
6. <span data-ttu-id="e5461-350">Wróć do portalu i kliknij nazwę witryny sieci web w obszarze **nazwa** kolumny do wyświetlenia strony zarządzania.</span><span class="sxs-lookup"><span data-stu-id="e5461-350">Go back to the portal and click the name of the web site under the **Name** column to display the management pages.</span></span>

    <span data-ttu-id="e5461-351">![Otwieranie strony zarządzania witryny sieci web](aspnet-mvc-4-custom-action-filters/_static/image22.png "otwieranie stron zarządzania witryny sieci web")</span><span class="sxs-lookup"><span data-stu-id="e5461-351">![Opening the web site management pages](aspnet-mvc-4-custom-action-filters/_static/image22.png "Opening the web site management pages")</span></span>

    <span data-ttu-id="e5461-352">*Otwieranie strony zarządzania witryny sieci Web*</span><span class="sxs-lookup"><span data-stu-id="e5461-352">*Opening the Web Site management pages*</span></span>
7. <span data-ttu-id="e5461-353">W **pulpit nawigacyjny** w obszarze **Przegląd** kliknij **Pobierz profil publikowania** łącza.</span><span class="sxs-lookup"><span data-stu-id="e5461-353">In the **Dashboard** page, under the **quick glance** section, click the **Download publish profile** link.</span></span>

    > [!NOTE]
    > <span data-ttu-id="e5461-354">*Profil publikowania* zawiera wszystkie informacje wymagane do publikowania aplikacji sieci web w witrynie sieci Web usługi Windows Azure dla poszczególnych metod włączone publikacji.</span><span class="sxs-lookup"><span data-stu-id="e5461-354">The *publish profile* contains all of the information required to publish a web application to a Windows Azure website for each enabled publication method.</span></span> <span data-ttu-id="e5461-355">Profil publikowania zawiera adresy URL, poświadczenia użytkownika i parametry bazy danych wymagane do łączenia się i uwierzytelnianie w odniesieniu do każdego z punktów końcowych, dla których włączono metoda publikacji.</span><span class="sxs-lookup"><span data-stu-id="e5461-355">The publish profile contains the URLs, user credentials and database strings required to connect to and authenticate against each of the endpoints for which a publication method is enabled.</span></span> <span data-ttu-id="e5461-356">**Microsoft WebMatrix 2**, **Microsoft Visual Studio Express for Web** i **Microsoft Visual Studio 2012** Obsługa odczytywania profili publikowania do automatyzowania konfiguracji tych programów dla Publikowanie aplikacji sieci web w usłudze Windows Azure websites.</span><span class="sxs-lookup"><span data-stu-id="e5461-356">**Microsoft WebMatrix 2**, **Microsoft Visual Studio Express for Web** and **Microsoft Visual Studio 2012** support reading publish profiles to automate configuration of these programs for publishing web applications to Windows Azure websites.</span></span>

    <span data-ttu-id="e5461-357">![Pobieranie witryny sieci web profil publikowania](aspnet-mvc-4-custom-action-filters/_static/image23.png "pobierania witryny sieci web profilu publikowania")</span><span class="sxs-lookup"><span data-stu-id="e5461-357">![Downloading the web site publish profile](aspnet-mvc-4-custom-action-filters/_static/image23.png "Downloading the web site publish profile")</span></span>

    <span data-ttu-id="e5461-358">*Pobieranie witryny sieci Web profilu publikowania*</span><span class="sxs-lookup"><span data-stu-id="e5461-358">*Downloading the Web Site publish profile*</span></span>
8. <span data-ttu-id="e5461-359">Pobierz plik profilu publikowania w znanej lokalizacji.</span><span class="sxs-lookup"><span data-stu-id="e5461-359">Download the publish profile file to a known location.</span></span> <span data-ttu-id="e5461-360">Dodatkowo w tym ćwiczeniu zobaczysz jak opublikować aplikację sieci web do Windows Azure Web Sites z programu Visual Studio za pomocą tego pliku.</span><span class="sxs-lookup"><span data-stu-id="e5461-360">Further in this exercise you will see how to use this file to publish a web application to a Windows Azure Web Sites from Visual Studio.</span></span>

    <span data-ttu-id="e5461-361">![Zapisywanie pliku profilu publikowania](aspnet-mvc-4-custom-action-filters/_static/image24.png "zapisywanie profilu publikowania")</span><span class="sxs-lookup"><span data-stu-id="e5461-361">![Saving the publish profile file](aspnet-mvc-4-custom-action-filters/_static/image24.png "Saving the publish profile")</span></span>

    <span data-ttu-id="e5461-362">*Zapisywanie pliku profilu publikowania*</span><span class="sxs-lookup"><span data-stu-id="e5461-362">*Saving the publish profile file*</span></span>

<a id="ApxBTask2"></a>

<a id="Task_2_-_Configuring_the_Database_Server"></a>
#### <a name="task-2---configuring-the-database-server"></a><span data-ttu-id="e5461-363">Zadanie 2 — Konfigurowanie serwera bazy danych</span><span class="sxs-lookup"><span data-stu-id="e5461-363">Task 2 - Configuring the Database Server</span></span>

<span data-ttu-id="e5461-364">Jeśli aplikacja korzysta z programu SQL Server baz danych, musisz utworzyć serwer bazy danych SQL.</span><span class="sxs-lookup"><span data-stu-id="e5461-364">If your application makes use of SQL Server databases you will need to create a SQL Database server.</span></span> <span data-ttu-id="e5461-365">Jeśli chcesz wdrożyć prostą aplikację, która nie korzysta z programu SQL Server może pominąć to zadanie.</span><span class="sxs-lookup"><span data-stu-id="e5461-365">If you want to deploy a simple application that does not use SQL Server you might skip this task.</span></span>

1. <span data-ttu-id="e5461-366">Należy serwera usługi SQL Database do przechowywania bazy danych aplikacji.</span><span class="sxs-lookup"><span data-stu-id="e5461-366">You will need a SQL Database server for storing the application database.</span></span> <span data-ttu-id="e5461-367">Możesz wyświetlić serwery baz danych SQL z subskrypcji w portalu zarządzania Azure Windows pod **baz danych Sql** | **serwerów** | **serwera Pulpit nawigacyjny**.</span><span class="sxs-lookup"><span data-stu-id="e5461-367">You can view the SQL Database servers from your subscription in the Windows Azure Management portal at **Sql Databases** | **Servers** | **Server's Dashboard**.</span></span> <span data-ttu-id="e5461-368">Jeśli nie masz serwer, który został utworzony, można utworzyć ją przy użyciu **Dodaj** przycisk na pasku poleceń.</span><span class="sxs-lookup"><span data-stu-id="e5461-368">If you do not have a server created, you can create one using the **Add** button on the command bar.</span></span> <span data-ttu-id="e5461-369">Zwróć uwagę na **nazwę serwera i adres URL, nazwę logowania administratora i hasła**, ponieważ będziesz ich używać w kolejne zadania podrzędne.</span><span class="sxs-lookup"><span data-stu-id="e5461-369">Take note of the **server name and URL, administrator login name and password**, as you will use them in the next tasks.</span></span> <span data-ttu-id="e5461-370">Nie należy tworzyć bazy danych, ponieważ zostanie on utworzony w późniejszym terminie.</span><span class="sxs-lookup"><span data-stu-id="e5461-370">Do not create the database yet, as it will be created in a later stage.</span></span>

    <span data-ttu-id="e5461-371">![Pulpit nawigacyjny z serwera bazy danych SQL](aspnet-mvc-4-custom-action-filters/_static/image25.png "pulpitu nawigacyjnego serwera bazy danych SQL")</span><span class="sxs-lookup"><span data-stu-id="e5461-371">![SQL Database Server Dashboard](aspnet-mvc-4-custom-action-filters/_static/image25.png "SQL Database Server Dashboard")</span></span>

    <span data-ttu-id="e5461-372">*Pulpit nawigacyjny z serwera bazy danych SQL*</span><span class="sxs-lookup"><span data-stu-id="e5461-372">*SQL Database Server Dashboard*</span></span>
2. <span data-ttu-id="e5461-373">W ramach następnego zadania spowoduje przetestowanie połączenia z bazą danych z programu Visual Studio, dlatego trzeba uwzględnić adres IP lokalnego serwera liście **dozwolone adresy IP**.</span><span class="sxs-lookup"><span data-stu-id="e5461-373">In the next task you will test the database connection from Visual Studio, for that reason you need to include your local IP address in the server's list of **Allowed IP Addresses**.</span></span> <span data-ttu-id="e5461-374">Aby to zrobić, kliknij przycisk **Konfiguruj**, wybierz adres IP z **bieżący adres IP klienta** i wklej go na **początkowy adres IP** i **końcowy adres IP** pola tekstowe i kliknij przycisk ![add-client-ip-address-ok-button](aspnet-mvc-4-custom-action-filters/_static/image26.png) przycisku.</span><span class="sxs-lookup"><span data-stu-id="e5461-374">To do that, click **Configure**, select the IP address from **Current Client IP Address** and paste it on the **Start IP Address** and **End IP Address** text boxes and click the ![add-client-ip-address-ok-button](aspnet-mvc-4-custom-action-filters/_static/image26.png) button.</span></span>

    ![Dodawanie adresu IP klienta](aspnet-mvc-4-custom-action-filters/_static/image27.png)

    <span data-ttu-id="e5461-376">*Dodawanie adresu IP klienta*</span><span class="sxs-lookup"><span data-stu-id="e5461-376">*Adding Client IP Address*</span></span>
3. <span data-ttu-id="e5461-377">Raz **adres IP klienta** jest dodawany do dozwolonych adresów IP listy, kliknij pozycję **Zapisz** aby potwierdzić zmiany.</span><span class="sxs-lookup"><span data-stu-id="e5461-377">Once the **Client IP Address** is added to the allowed IP addresses list, click on **Save** to confirm the changes.</span></span>

    ![Potwierdź zmiany](aspnet-mvc-4-custom-action-filters/_static/image28.png)

    <span data-ttu-id="e5461-379">*Potwierdź zmiany*</span><span class="sxs-lookup"><span data-stu-id="e5461-379">*Confirm Changes*</span></span>

<a id="ApxBTask3"></a>

<a id="Task_3_-_Publishing_an_ASPNET_MVC_4_Application_using_Web_Deploy"></a>
#### <a name="task-3---publishing-an-aspnet-mvc-4-application-using-web-deploy"></a><span data-ttu-id="e5461-380">Zadanie 3 — Publikowanie aplikacji ASP.NET MVC 4 za pomocą narzędzia Web Deploy</span><span class="sxs-lookup"><span data-stu-id="e5461-380">Task 3 - Publishing an ASP.NET MVC 4 Application using Web Deploy</span></span>

1. <span data-ttu-id="e5461-381">Wróć do rozwiązania ASP.NET MVC 4.</span><span class="sxs-lookup"><span data-stu-id="e5461-381">Go back to the ASP.NET MVC 4 solution.</span></span> <span data-ttu-id="e5461-382">W **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy projekt witryny sieci web i wybierz **Publikuj**.</span><span class="sxs-lookup"><span data-stu-id="e5461-382">In the **Solution Explorer**, right-click the web site project and select **Publish**.</span></span>

    <span data-ttu-id="e5461-383">![Publikowanie aplikacji](aspnet-mvc-4-custom-action-filters/_static/image29.png "publikowania aplikacji")</span><span class="sxs-lookup"><span data-stu-id="e5461-383">![Publishing the Application](aspnet-mvc-4-custom-action-filters/_static/image29.png "Publishing the Application")</span></span>

    <span data-ttu-id="e5461-384">*Publikowanie witryny sieci web*</span><span class="sxs-lookup"><span data-stu-id="e5461-384">*Publishing the web site*</span></span>
2. <span data-ttu-id="e5461-385">Importuj profil publikowania, zapisaną w pierwszym zadaniem.</span><span class="sxs-lookup"><span data-stu-id="e5461-385">Import the publish profile you saved in the first task.</span></span>

    <span data-ttu-id="e5461-386">![Trwa importowanie profilu publikowania](aspnet-mvc-4-custom-action-filters/_static/image30.png "importowania profilu publikowania")</span><span class="sxs-lookup"><span data-stu-id="e5461-386">![Importing the publish profile](aspnet-mvc-4-custom-action-filters/_static/image30.png "Importing the publish profile")</span></span>

    <span data-ttu-id="e5461-387">*Importowanie profilu publikowania*</span><span class="sxs-lookup"><span data-stu-id="e5461-387">*Importing publish profile*</span></span>
3. <span data-ttu-id="e5461-388">Kliknij przycisk **sprawdzanie poprawności połączenia**.</span><span class="sxs-lookup"><span data-stu-id="e5461-388">Click **Validate Connection**.</span></span> <span data-ttu-id="e5461-389">Po zakończeniu sprawdzania kliknij **dalej**.</span><span class="sxs-lookup"><span data-stu-id="e5461-389">Once Validation is complete click **Next**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="e5461-390">Sprawdzanie poprawności zostało ukończone, gdy zostanie wyświetlony z zielonym znacznikiem wyboru pojawiają się obok przycisku Waliduj połączenie.</span><span class="sxs-lookup"><span data-stu-id="e5461-390">Validation is complete once you see a green checkmark appear next to the Validate Connection button.</span></span>

    <span data-ttu-id="e5461-391">![Sprawdzanie poprawności połączenia](aspnet-mvc-4-custom-action-filters/_static/image31.png "sprawdzanie poprawności połączenia")</span><span class="sxs-lookup"><span data-stu-id="e5461-391">![Validating connection](aspnet-mvc-4-custom-action-filters/_static/image31.png "Validating connection")</span></span>

    <span data-ttu-id="e5461-392">*Sprawdzanie poprawności połączenia*</span><span class="sxs-lookup"><span data-stu-id="e5461-392">*Validating connection*</span></span>
4. <span data-ttu-id="e5461-393">W **ustawienia** w obszarze **baz danych** sekcji, kliknij przycisk obok pola tekstowego połączenia bazy danych (czyli **DefaultConnection**).</span><span class="sxs-lookup"><span data-stu-id="e5461-393">In the **Settings** page, under the **Databases** section, click the button next to your database connection's textbox (i.e. **DefaultConnection**).</span></span>

    <span data-ttu-id="e5461-394">![Konfiguracja narzędzia Web deploy](aspnet-mvc-4-custom-action-filters/_static/image32.png "Konfiguracja narzędzia Web deploy")</span><span class="sxs-lookup"><span data-stu-id="e5461-394">![Web deploy configuration](aspnet-mvc-4-custom-action-filters/_static/image32.png "Web deploy configuration")</span></span>

    <span data-ttu-id="e5461-395">*Konfiguracja narzędzia Web deploy*</span><span class="sxs-lookup"><span data-stu-id="e5461-395">*Web deploy configuration*</span></span>
5. <span data-ttu-id="e5461-396">Skonfiguruj połączenie z bazą danych w następujący sposób:</span><span class="sxs-lookup"><span data-stu-id="e5461-396">Configure the database connection as follows:</span></span>

   - <span data-ttu-id="e5461-397">W **nazwy serwera** wpisz swoją bazę danych SQL server adresu URL przy użyciu *tcp:* prefiks.</span><span class="sxs-lookup"><span data-stu-id="e5461-397">In the **Server name** type your SQL Database server URL using the *tcp:* prefix.</span></span>
   - <span data-ttu-id="e5461-398">W **nazwa_użytkownika** wpisz nazwę logowania administratora serwera.</span><span class="sxs-lookup"><span data-stu-id="e5461-398">In **User name** type your server administrator login name.</span></span>
   - <span data-ttu-id="e5461-399">W **hasło** wpisz hasło logowania administratora serwera.</span><span class="sxs-lookup"><span data-stu-id="e5461-399">In **Password** type your server administrator login password.</span></span>
   - <span data-ttu-id="e5461-400">Wpisz nazwę nowej bazy danych.</span><span class="sxs-lookup"><span data-stu-id="e5461-400">Type a new database name.</span></span>

     <span data-ttu-id="e5461-401">![Konfigurowanie parametrów połączenia z docelowym](aspnet-mvc-4-custom-action-filters/_static/image33.png "Konfigurowanie parametrów połączenia z docelowym")</span><span class="sxs-lookup"><span data-stu-id="e5461-401">![Configuring destination connection string](aspnet-mvc-4-custom-action-filters/_static/image33.png "Configuring destination connection string")</span></span>

     <span data-ttu-id="e5461-402">*Konfigurowanie parametrów połączenia z docelowym*</span><span class="sxs-lookup"><span data-stu-id="e5461-402">*Configuring destination connection string*</span></span>
6. <span data-ttu-id="e5461-403">Następnie kliknij przycisk **OK**.</span><span class="sxs-lookup"><span data-stu-id="e5461-403">Then click **OK**.</span></span> <span data-ttu-id="e5461-404">Po wyświetleniu monitu, aby utworzyć kliknij bazę danych **tak**.</span><span class="sxs-lookup"><span data-stu-id="e5461-404">When prompted to create the database click **Yes**.</span></span>

    <span data-ttu-id="e5461-405">![Tworzenie bazy danych](aspnet-mvc-4-custom-action-filters/_static/image34.png "Tworzenie parametrów bazy danych")</span><span class="sxs-lookup"><span data-stu-id="e5461-405">![Creating the database](aspnet-mvc-4-custom-action-filters/_static/image34.png "Creating the database string")</span></span>

    <span data-ttu-id="e5461-406">*Tworzenie bazy danych*</span><span class="sxs-lookup"><span data-stu-id="e5461-406">*Creating the database*</span></span>
7. <span data-ttu-id="e5461-407">Parametry połączenia, które będą używane do łączenia z bazą danych SQL na platformie Windows Azure jest wyświetlana w polu tekstowym połączenia domyślne.</span><span class="sxs-lookup"><span data-stu-id="e5461-407">The connection string you will use to connect to SQL Database in Windows Azure is shown within Default Connection textbox.</span></span> <span data-ttu-id="e5461-408">Następnie kliknij przycisk **Dalej**.</span><span class="sxs-lookup"><span data-stu-id="e5461-408">Then click **Next**.</span></span>

    <span data-ttu-id="e5461-409">![Ciąg połączenia wskazujący bazę danych SQL](aspnet-mvc-4-custom-action-filters/_static/image35.png "ciąg połączenia wskazujący bazę danych SQL")</span><span class="sxs-lookup"><span data-stu-id="e5461-409">![Connection string pointing to SQL Database](aspnet-mvc-4-custom-action-filters/_static/image35.png "Connection string pointing to SQL Database")</span></span>

    <span data-ttu-id="e5461-410">*Ciąg połączenia wskazujący bazę danych SQL*</span><span class="sxs-lookup"><span data-stu-id="e5461-410">*Connection string pointing to SQL Database*</span></span>
8. <span data-ttu-id="e5461-411">W **Podgląd** kliknij **Publikuj**.</span><span class="sxs-lookup"><span data-stu-id="e5461-411">In the **Preview** page, click **Publish**.</span></span>

    <span data-ttu-id="e5461-412">![Publikowanie aplikacji sieci web](aspnet-mvc-4-custom-action-filters/_static/image36.png "publikowania aplikacji sieci web")</span><span class="sxs-lookup"><span data-stu-id="e5461-412">![Publishing the web application](aspnet-mvc-4-custom-action-filters/_static/image36.png "Publishing the web application")</span></span>

    <span data-ttu-id="e5461-413">*Publikowanie aplikacji sieci web*</span><span class="sxs-lookup"><span data-stu-id="e5461-413">*Publishing the web application*</span></span>
9. <span data-ttu-id="e5461-414">Po zakończeniu procesu publikowania domyślnej przeglądarce otworzy opublikowanej witryny sieci web.</span><span class="sxs-lookup"><span data-stu-id="e5461-414">Once the publishing process finishes, your default browser will open the published web site.</span></span>

<a id="AppendixC"></a>

<a id="Appendix_C_Using_Code_Snippets"></a>
## <a name="appendix-c-using-code-snippets"></a><span data-ttu-id="e5461-415">Dodatek C: korzystania z fragmentów kodu</span><span class="sxs-lookup"><span data-stu-id="e5461-415">Appendix C: Using Code Snippets</span></span>

<span data-ttu-id="e5461-416">Za pomocą wstawek kodu masz kod, czego potrzebujesz w zasięgu ręki.</span><span class="sxs-lookup"><span data-stu-id="e5461-416">With code snippets, you have all the code you need at your fingertips.</span></span> <span data-ttu-id="e5461-417">Dokument laboratorium informuje o dokładnie podczas korzystania z nich, jak pokazano na poniższej ilustracji.</span><span class="sxs-lookup"><span data-stu-id="e5461-417">The lab document will tell you exactly when you can use them, as shown in the following figure.</span></span>

<span data-ttu-id="e5461-418">![Za pomocą wstawek kodu programu Visual Studio, aby wstawić kod do projektu](aspnet-mvc-4-custom-action-filters/_static/image37.png "fragmenty kodu za pomocą programu Visual Studio, aby wstawić kod do projektu")</span><span class="sxs-lookup"><span data-stu-id="e5461-418">![Using Visual Studio code snippets to insert code into your project](aspnet-mvc-4-custom-action-filters/_static/image37.png "Using Visual Studio code snippets to insert code into your project")</span></span>

<span data-ttu-id="e5461-419">*Za pomocą wstawek kodu programu Visual Studio, aby wstawić kod do projektu*</span><span class="sxs-lookup"><span data-stu-id="e5461-419">*Using Visual Studio code snippets to insert code into your project*</span></span>

<span data-ttu-id="e5461-420">***Aby dodać wstawki kodu za pomocą klawiatury (tylko język C#)***</span><span class="sxs-lookup"><span data-stu-id="e5461-420">***To add a code snippet using the keyboard (C# only)***</span></span>

1. <span data-ttu-id="e5461-421">Umieść kursor, w którym chcesz wstawić kod.</span><span class="sxs-lookup"><span data-stu-id="e5461-421">Place the cursor where you would like to insert the code.</span></span>
2. <span data-ttu-id="e5461-422">Rozpocznij wpisywanie nazwy fragmentu kodu (bez spacji i łączniki).</span><span class="sxs-lookup"><span data-stu-id="e5461-422">Start typing the snippet name (without spaces or hyphens).</span></span>
3. <span data-ttu-id="e5461-423">Obejrzyj, jak technologia IntelliSense wyświetla zgodne z nazwami fragmenty kodu.</span><span class="sxs-lookup"><span data-stu-id="e5461-423">Watch as IntelliSense displays matching snippets' names.</span></span>
4. <span data-ttu-id="e5461-424">Wybierz prawidłowy fragment kodu lub pisz dalej do momentu wybrania debugowanej nazwy całego fragmentu kodu.</span><span class="sxs-lookup"><span data-stu-id="e5461-424">Select the correct snippet (or keep typing until the entire snippet's name is selected).</span></span>
5. <span data-ttu-id="e5461-425">Naciśnij klawisz Tab dwa razy, aby wstawić fragment kodu w lokalizacji kursora.</span><span class="sxs-lookup"><span data-stu-id="e5461-425">Press the Tab key twice to insert the snippet at the cursor location.</span></span>

<span data-ttu-id="e5461-426">![Rozpocznij wpisywanie nazwy fragmentu kodu](aspnet-mvc-4-custom-action-filters/_static/image38.png "Rozpocznij wpisywanie nazwy fragmentu kodu")</span><span class="sxs-lookup"><span data-stu-id="e5461-426">![Start typing the snippet name](aspnet-mvc-4-custom-action-filters/_static/image38.png "Start typing the snippet name")</span></span>

<span data-ttu-id="e5461-427">*Rozpocznij wpisywanie nazwy fragmentu kodu*</span><span class="sxs-lookup"><span data-stu-id="e5461-427">*Start typing the snippet name*</span></span>

<span data-ttu-id="e5461-428">![Naciśnij klawisz Tab, aby zaznaczyć fragment wyróżnione](aspnet-mvc-4-custom-action-filters/_static/image39.png "naciśnij klawisz Tab, aby wybrać wyróżnione fragmentu kodu")</span><span class="sxs-lookup"><span data-stu-id="e5461-428">![Press Tab to select the highlighted snippet](aspnet-mvc-4-custom-action-filters/_static/image39.png "Press Tab to select the highlighted snippet")</span></span>

<span data-ttu-id="e5461-429">*Naciśnij klawisz Tab, aby wybrać wyróżnione fragment kodu*</span><span class="sxs-lookup"><span data-stu-id="e5461-429">*Press Tab to select the highlighted snippet*</span></span>

<span data-ttu-id="e5461-430">![Ponownie naciśnij klawisz Tab i fragment kodu zostanie rozwinięte](aspnet-mvc-4-custom-action-filters/_static/image40.png "rozwinie ponownie naciśnij klawisz Tab i fragment kodu")</span><span class="sxs-lookup"><span data-stu-id="e5461-430">![Press Tab again and the snippet will expand](aspnet-mvc-4-custom-action-filters/_static/image40.png "Press Tab again and the snippet will expand")</span></span>

<span data-ttu-id="e5461-431">*Ponownie naciśnij klawisz Tab i fragment kodu zostanie rozwinięte.*</span><span class="sxs-lookup"><span data-stu-id="e5461-431">*Press Tab again and the snippet will expand*</span></span>

<span data-ttu-id="e5461-432">***Aby dodać wstawki kodu za pomocą myszy (C#, Visual Basic i XML)*** 1.</span><span class="sxs-lookup"><span data-stu-id="e5461-432">***To add a code snippet using the mouse (C#, Visual Basic and XML)*** 1.</span></span> <span data-ttu-id="e5461-433">Kliknij prawym przyciskiem myszy, gdy chcesz wstawić fragment kodu.</span><span class="sxs-lookup"><span data-stu-id="e5461-433">Right-click where you want to insert the code snippet.</span></span>

1. <span data-ttu-id="e5461-434">Wybierz **Wstaw fragment kodu** następuje **Moje fragmenty kodu**.</span><span class="sxs-lookup"><span data-stu-id="e5461-434">Select **Insert Snippet** followed by **My Code Snippets**.</span></span>
2. <span data-ttu-id="e5461-435">Wybierz odpowiedni fragment z listy, klikając go.</span><span class="sxs-lookup"><span data-stu-id="e5461-435">Pick the relevant snippet from the list, by clicking on it.</span></span>

<span data-ttu-id="e5461-436">![Kliknij prawym przyciskiem myszy, które chcesz wstawić fragment kodu i wybierz opcję Wstaw fragment kodu](aspnet-mvc-4-custom-action-filters/_static/image41.png "kliknij prawym przyciskiem myszy, które chcesz wstawić fragment kodu i wybierz opcję Wstaw fragment kodu")</span><span class="sxs-lookup"><span data-stu-id="e5461-436">![Right-click where you want to insert the code snippet and select Insert Snippet](aspnet-mvc-4-custom-action-filters/_static/image41.png "Right-click where you want to insert the code snippet and select Insert Snippet")</span></span>

<span data-ttu-id="e5461-437">*Kliknij prawym przyciskiem myszy, które chcesz wstawić fragment kodu i wybierz opcję Wstaw fragment kodu*</span><span class="sxs-lookup"><span data-stu-id="e5461-437">*Right-click where you want to insert the code snippet and select Insert Snippet*</span></span>

<span data-ttu-id="e5461-438">![Wybierz odpowiedni fragment z listy, klikając go](aspnet-mvc-4-custom-action-filters/_static/image42.png "wybierz odpowiedni fragment z listy, klikając go")</span><span class="sxs-lookup"><span data-stu-id="e5461-438">![Pick the relevant snippet from the list, by clicking on it](aspnet-mvc-4-custom-action-filters/_static/image42.png "Pick the relevant snippet from the list, by clicking on it")</span></span>

<span data-ttu-id="e5461-439">*Wybierz odpowiedni fragment z listy, klikając go*</span><span class="sxs-lookup"><span data-stu-id="e5461-439">*Pick the relevant snippet from the list, by clicking on it*</span></span>
