---
uid: visual-studio/overview/2013/one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api
title: 'Warsztaty: One ASP.NET: integrowanie wzorca ASP.NET Web Forms, MVC i interfejs API sieci Web | Dokumentacja firmy Microsoft'
author: rick-anderson
description: ASP.NET to architektura służąca do tworzenia witryn sieci Web, aplikacji i usług przy użyciu technologii specjalne, np. MVC, interfejs API sieci Web i innych. Przy użyciu rozszerzeń ASP.NET h...
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/16/2014
ms.topic: article
ms.assetid: 4fe2558d-67cc-4d12-a5c1-6fb9f6f16137
ms.technology: ''
msc.legacyurl: /visual-studio/overview/2013/one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api
msc.type: authoredcontent
ms.openlocfilehash: 7dec4daffa66621acaee1c76fda7b2e7550ad925
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: HT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/03/2018
ms.locfileid: "37382955"
---
<a name="hands-on-lab-one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api"></a><span data-ttu-id="2c878-104">Warsztaty: One ASP.NET: integrowanie wzorca ASP.NET Web Forms, MVC i Web API</span><span class="sxs-lookup"><span data-stu-id="2c878-104">Hands On Lab: One ASP.NET: Integrating ASP.NET Web Forms, MVC and Web API</span></span>
====================
<span data-ttu-id="2c878-105">Przez [Camp w sieci Web zespołu](https://twitter.com/webcamps)</span><span class="sxs-lookup"><span data-stu-id="2c878-105">by [Web Camps Team](https://twitter.com/webcamps)</span></span>

[<span data-ttu-id="2c878-106">Pobierz Camp Web szkolenia Kit</span><span class="sxs-lookup"><span data-stu-id="2c878-106">Download Web Camps Training Kit</span></span>](http://aka.ms/webcamps-training-kit)

> <span data-ttu-id="2c878-107">ASP.NET to architektura służąca do tworzenia witryn sieci Web, aplikacji i usług przy użyciu technologii specjalne, np. MVC, interfejs API sieci Web i innych.</span><span class="sxs-lookup"><span data-stu-id="2c878-107">ASP.NET is a framework for building Web sites, apps and services using specialized technologies such as MVC, Web API and others.</span></span> <span data-ttu-id="2c878-108">Z rozszerzeniem ASP.NET obserwowała, jak od jego utworzenia i zaakceptowania muszą być tych technologii zintegrowane, działania w kierunku miały miejsce ostatnie wysiłków **One ASP.NET**.</span><span class="sxs-lookup"><span data-stu-id="2c878-108">With the expansion ASP.NET has seen since its creation and the expressed need to have these technologies integrated, there have been recent efforts in working towards **One ASP.NET**.</span></span>
> 
> <span data-ttu-id="2c878-109">Visual Studio 2013 wprowadza nowy jednolity system, który umożliwia tworzenie aplikacji i korzystać z technologii ASP.NET w jednym projekcie.</span><span class="sxs-lookup"><span data-stu-id="2c878-109">Visual Studio 2013 introduces a new unified project system which lets you build an application and use all the ASP.NET technologies in one project.</span></span> <span data-ttu-id="2c878-110">Ta funkcja eliminuje konieczność pobrania jedna technologia na początku projektu i hokejowego z nim, a zamiast tego zaleca się korzystanie z wielu platform ASP.NET, w ramach jednego projektu.</span><span class="sxs-lookup"><span data-stu-id="2c878-110">This feature eliminates the need to pick one technology at the start of a project and stick with it, and instead encourages the use of multiple ASP.NET frameworks within one project.</span></span>
> 
> <span data-ttu-id="2c878-111">Wszystkie przykładowy kod i fragmenty kodu są uwzględnione w sieci Web Camp zestaw szkoleniowy, dostępne pod adresem [ http://aka.ms/webcamps-training-kit ](http://aka.ms/webcamps-training-kit).</span><span class="sxs-lookup"><span data-stu-id="2c878-111">All sample code and snippets are included in the Web Camps Training Kit, available at [http://aka.ms/webcamps-training-kit](http://aka.ms/webcamps-training-kit).</span></span>


<a id="Overview"></a>
## <a name="overview"></a><span data-ttu-id="2c878-112">Omówienie</span><span class="sxs-lookup"><span data-stu-id="2c878-112">Overview</span></span>

<a id="Objectives"></a>
### <a name="objectives"></a><span data-ttu-id="2c878-113">Cele</span><span class="sxs-lookup"><span data-stu-id="2c878-113">Objectives</span></span>

<span data-ttu-id="2c878-114">W tym praktyczne laboratorium dowiesz się jak:</span><span class="sxs-lookup"><span data-stu-id="2c878-114">In this hands-on lab, you will learn how to:</span></span>

- <span data-ttu-id="2c878-115">Tworzenie witryny sieci Web, na podstawie **One ASP.NET** typ projektu</span><span class="sxs-lookup"><span data-stu-id="2c878-115">Create a Web site based on the **One ASP.NET** project type</span></span>
- <span data-ttu-id="2c878-116">Użyj innych **ASP.NET** platform, na przykład **MVC** i **interfejsu API sieci Web** w tym samym projekcie</span><span class="sxs-lookup"><span data-stu-id="2c878-116">Use different **ASP.NET** frameworks like **MVC** and **Web API** in the same project</span></span>
- <span data-ttu-id="2c878-117">Identyfikowanie główne składniki **ASP.NET** aplikacji</span><span class="sxs-lookup"><span data-stu-id="2c878-117">Identify the main components of an **ASP.NET** application</span></span>
- <span data-ttu-id="2c878-118">Skorzystaj z zalet **funkcja tworzenia szkieletu ASP.NET** framework, automatyczne tworzenie widoków i kontrolerów w celu wykonywania operacji CRUD, oparte na klasach modeli</span><span class="sxs-lookup"><span data-stu-id="2c878-118">Take advantage of the **ASP.NET Scaffolding** framework to automatically create Controllers and Views to perform CRUD operations based on your model classes</span></span>
- <span data-ttu-id="2c878-119">Udostępnianie tego samego zestawu danych w formatach machine - i czytelny dla człowieka, za pomocą właściwych narzędzi dla każdego zadania</span><span class="sxs-lookup"><span data-stu-id="2c878-119">Expose the same set of information in machine- and human-readable formats using the right tool for each job</span></span>

<a id="Prerequisites"></a>
### <a name="prerequisites"></a><span data-ttu-id="2c878-120">Wymagania wstępne</span><span class="sxs-lookup"><span data-stu-id="2c878-120">Prerequisites</span></span>

<span data-ttu-id="2c878-121">Do ukończenia tego laboratorium praktycznego niezbędne jest, następujące elementy:</span><span class="sxs-lookup"><span data-stu-id="2c878-121">The following is required to complete this hands-on lab:</span></span>

- <span data-ttu-id="2c878-122">[Visual Studio Express 2013 for Web](https://www.microsoft.com/visualstudio/) lub nowszej</span><span class="sxs-lookup"><span data-stu-id="2c878-122">[Visual Studio Express 2013 for Web](https://www.microsoft.com/visualstudio/) or greater</span></span>
- [<span data-ttu-id="2c878-123">Visual Studio 2013 Update 1</span><span class="sxs-lookup"><span data-stu-id="2c878-123">Visual Studio 2013 Update 1</span></span>](https://go.microsoft.com/fwlink/?LinkId=301714)

<a id="Setup"></a>
### <a name="setup"></a><span data-ttu-id="2c878-124">Konfiguracja</span><span class="sxs-lookup"><span data-stu-id="2c878-124">Setup</span></span>

<span data-ttu-id="2c878-125">Aby można było uruchomić ćwiczeń opisanych w tym praktyczne laboratorium, należy najpierw skonfigurować swoje środowisko.</span><span class="sxs-lookup"><span data-stu-id="2c878-125">In order to run the exercises in this hands-on lab, you will need to set up your environment first.</span></span>

1. <span data-ttu-id="2c878-126">Otwórz Eksploratora Windows i przejdź do laboratorium **źródła** folderu.</span><span class="sxs-lookup"><span data-stu-id="2c878-126">Open Windows Explorer and browse to the lab's **Source** folder.</span></span>
2. <span data-ttu-id="2c878-127">Kliknij prawym przyciskiem myszy **plik Setup.cmd** i wybierz **Uruchom jako administrator** do uruchamiania procesu instalacji, który będzie skonfigurować środowisko i zainstalować fragmenty kodu programu Visual Studio, w tym środowisku laboratoryjnym.</span><span class="sxs-lookup"><span data-stu-id="2c878-127">Right-click on **Setup.cmd** and select **Run as administrator** to launch the setup process that will configure your environment and install the Visual Studio code snippets for this lab.</span></span>
3. <span data-ttu-id="2c878-128">Jeśli zostanie wyświetlone okno dialogowe kontroli konta użytkownika, upewnij się, działania, aby kontynuować.</span><span class="sxs-lookup"><span data-stu-id="2c878-128">If the User Account Control dialog box is shown, confirm the action to proceed.</span></span>

> [!NOTE]
> <span data-ttu-id="2c878-129">Upewnij się, że wszystkie zależności w tym środowisku laboratoryjnym sprawdzeniu przed uruchomieniem Instalatora.</span><span class="sxs-lookup"><span data-stu-id="2c878-129">Make sure you have checked all the dependencies for this lab before running the setup.</span></span>


<a id="CodeSnippets"></a>
### <a name="using-the-code-snippets"></a><span data-ttu-id="2c878-130">Za pomocą fragmentów kodu</span><span class="sxs-lookup"><span data-stu-id="2c878-130">Using the Code Snippets</span></span>

<span data-ttu-id="2c878-131">W dokumencie laboratorium należy poinstruować można wstawiać bloki kodu.</span><span class="sxs-lookup"><span data-stu-id="2c878-131">Throughout the lab document, you will be instructed to insert code blocks.</span></span> <span data-ttu-id="2c878-132">Dla wygody większość ten kod jest dostarczany jako Visual Studio fragmenty kodu, które są dostępne w Visual Studio 2013, aby uniknąć konieczności Dodaj ją ręcznie.</span><span class="sxs-lookup"><span data-stu-id="2c878-132">For your convenience, most of this code is provided as Visual Studio Code Snippets, which you can access from within Visual Studio 2013 to avoid having to add it manually.</span></span>

> [!NOTE]
> <span data-ttu-id="2c878-133">Każdy wykonywania towarzyszy początkowy rozwiązanie znajduje się w **rozpocząć** folderu ćwiczeniu, która umożliwia wykonanie każdego wykonywania niezależnie od innych.</span><span class="sxs-lookup"><span data-stu-id="2c878-133">Each exercise is accompanied by a starting solution located in the **Begin** folder of the exercise that allows you to follow each exercise independently of the others.</span></span> <span data-ttu-id="2c878-134">Należy pamiętać, że fragmenty kodu, które są dodawane podczas wykonywania brakuje te uruchamianie rozwiązań i może nie działać, dopóki nie zakończysz wykonywania.</span><span class="sxs-lookup"><span data-stu-id="2c878-134">Please be aware that the code snippets that are added during an exercise are missing from these starting solutions and may not work until you have completed the exercise.</span></span> <span data-ttu-id="2c878-135">Wewnątrz kodu źródłowego dla ćwiczenia, można również znaleźć **zakończenia** folderu zawierającego rozwiązania programu Visual Studio z kodem, który powstały na skutek wykonaniu kroków w odpowiedniej wykonywania.</span><span class="sxs-lookup"><span data-stu-id="2c878-135">Inside the source code for an exercise, you will also find an **End** folder containing a Visual Studio solution with the code that results from completing the steps in the corresponding exercise.</span></span> <span data-ttu-id="2c878-136">Jeśli potrzebujesz dodatkowej pomocy, gdy pracujesz za pośrednictwem tego laboratorium praktycznego, można użyć jako wskazówki dotyczące tych rozwiązań.</span><span class="sxs-lookup"><span data-stu-id="2c878-136">You can use these solutions as guidance if you need additional help as you work through this hands-on lab.</span></span>


* * *

<a id="Exercises"></a>
## <a name="exercises"></a><span data-ttu-id="2c878-137">Ćwiczenia</span><span class="sxs-lookup"><span data-stu-id="2c878-137">Exercises</span></span>

<span data-ttu-id="2c878-138">To ćwiczenie praktyczne obejmuje następujących czynnościach:</span><span class="sxs-lookup"><span data-stu-id="2c878-138">This hands-on lab includes the following exercises:</span></span>

1. [<span data-ttu-id="2c878-139">Tworzenie nowego projektu formularzy sieci Web</span><span class="sxs-lookup"><span data-stu-id="2c878-139">Creating a New Web Forms Project</span></span>](#Exercise1)
2. [<span data-ttu-id="2c878-140">Tworzenie szkieletów kontrolera MVC</span><span class="sxs-lookup"><span data-stu-id="2c878-140">Creating an MVC Controller Using Scaffolding</span></span>](#Exercise2)
3. [<span data-ttu-id="2c878-141">Tworzenie szkieletów kontrolera interfejsu API sieci Web</span><span class="sxs-lookup"><span data-stu-id="2c878-141">Creating a Web API Controller Using Scaffolding</span></span>](#Exercise3)

<span data-ttu-id="2c878-142">Szacowany czas do ukończenia tego laboratorium: **60 minut**</span><span class="sxs-lookup"><span data-stu-id="2c878-142">Estimated time to complete this lab: **60 minutes**</span></span>

> [!NOTE]
> <span data-ttu-id="2c878-143">Przy pierwszym uruchomieniu programu Visual Studio, należy wybrać jedną z kolekcji wstępnie zdefiniowanych ustawień.</span><span class="sxs-lookup"><span data-stu-id="2c878-143">When you first start Visual Studio, you must select one of the predefined settings collections.</span></span> <span data-ttu-id="2c878-144">Każda kolekcja wstępnie zdefiniowanych służy do dopasowywania style rozwoju i określa układy okna, zachowanie edytora, fragmenty kodu IntelliSense i opcje w oknach dialogowych.</span><span class="sxs-lookup"><span data-stu-id="2c878-144">Each predefined collection is designed to match a particular development style and determines window layouts, editor behavior, IntelliSense code snippets, and dialog box options.</span></span> <span data-ttu-id="2c878-145">Procedury przedstawione w tym środowisku laboratoryjnym opisano czynności niezbędnych do wykonywania danego zadania w programie Visual Studio, korzystając z **ogólnych ustawieniach projektowych** kolekcji.</span><span class="sxs-lookup"><span data-stu-id="2c878-145">The procedures in this lab describe the actions necessary to accomplish a given task in Visual Studio when using the **General Development Settings** collection.</span></span> <span data-ttu-id="2c878-146">Jeśli wybierzesz kolekcji różne ustawienia dla swojego środowiska programowania, może być różnice w krokach, które należy wziąć pod uwagę.</span><span class="sxs-lookup"><span data-stu-id="2c878-146">If you choose a different settings collection for your development environment, there may be differences in the steps that you should take into account.</span></span>


<a id="Exercise1"></a>
### <a name="exercise-1-creating-a-new-web-forms-project"></a><span data-ttu-id="2c878-147">Ćwiczenie 1: Tworzenie nowego projektu formularzy sieci Web</span><span class="sxs-lookup"><span data-stu-id="2c878-147">Exercise 1: Creating a New Web Forms Project</span></span>

<span data-ttu-id="2c878-148">W tym ćwiczeniu utworzysz nową lokację formularzy sieci Web przy użyciu programu Visual Studio 2013 **One ASP.NET** ujednolicone środowisko projektu, który pozwoli łatwo zintegrować składniki formularzy sieci Web, MVC i interfejs API sieci Web w tej samej aplikacji.</span><span class="sxs-lookup"><span data-stu-id="2c878-148">In this exercise you will create a new Web Forms site in Visual Studio 2013 using the **One ASP.NET** unified project experience, which will allow you to easily integrate Web Forms, MVC and Web API components in the same application.</span></span> <span data-ttu-id="2c878-149">Będzie Poznaj rozwiązanie wygenerowany i zidentyfikować jej części, a na koniec zobaczysz witryny sieci Web w działaniu.</span><span class="sxs-lookup"><span data-stu-id="2c878-149">You will then explore the generated solution and identify its parts, and finally you will see the Web site in action.</span></span>

<a id="Ex1Task1"></a>
#### <a name="task-1--creating-a-new-site-using-the-one-aspnet-experience"></a><span data-ttu-id="2c878-150">Zadanie 1 — Tworzenie nowej lokacji za pomocą jednego środowiska ASP.NET</span><span class="sxs-lookup"><span data-stu-id="2c878-150">Task 1 – Creating a New Site Using the One ASP.NET Experience</span></span>

<span data-ttu-id="2c878-151">W ramach tego zadania, użytkownik rozpocznie Tworzenie nowej witryny sieci Web w programie Visual Studio na podstawie **One ASP.NET** typ projektu.</span><span class="sxs-lookup"><span data-stu-id="2c878-151">In this task you will start creating a new Web site in Visual Studio based on the **One ASP.NET** project type.</span></span> <span data-ttu-id="2c878-152">**One ASP.NET** łączy wszystkie technologie ASP.NET i daje możliwość mieszać i dopasowywać je zgodnie z potrzebami.</span><span class="sxs-lookup"><span data-stu-id="2c878-152">**One ASP.NET** unifies all ASP.NET technologies and gives you the option to mix and match them as desired.</span></span> <span data-ttu-id="2c878-153">Następnie rozpoznają różnych składników Web Forms, MVC i interfejs API sieci Web, znajdujące się obok siebie w ramach aplikacji.</span><span class="sxs-lookup"><span data-stu-id="2c878-153">You will then recognize the different components of Web Forms, MVC and Web API that live side by side within your application.</span></span>

1. <span data-ttu-id="2c878-154">Otwórz **Visual Studio Express 2013 for Web** i wybierz **pliku | Nowy projekt...**  można uruchomić nowego rozwiązania.</span><span class="sxs-lookup"><span data-stu-id="2c878-154">Open **Visual Studio Express 2013 for Web** and select **File | New Project...** to start a new solution.</span></span>

    ![Tworzenie nowego projektu](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image1.png)

    <span data-ttu-id="2c878-156">*Tworzenie nowego projektu*</span><span class="sxs-lookup"><span data-stu-id="2c878-156">*Creating a New Project*</span></span>
2. <span data-ttu-id="2c878-157">W **nowy projekt** okno dialogowe, wybierz opcję **aplikacji sieci Web ASP.NET** w obszarze **Visual C# | Web** kartę i upewnij się, **.NET Framework 4.5** jest zaznaczone.</span><span class="sxs-lookup"><span data-stu-id="2c878-157">In the **New Project** dialog box, select **ASP.NET Web Application** under the **Visual C# | Web** tab, and make sure **.NET Framework 4.5** is selected.</span></span> <span data-ttu-id="2c878-158">Nadaj projektowi nazwę *MyHybridSite*, wybierz **lokalizacji** i kliknij przycisk **OK**.</span><span class="sxs-lookup"><span data-stu-id="2c878-158">Name the project *MyHybridSite*, choose a **Location** and click **OK**.</span></span>

    ![Nowy projekt aplikacji sieci Web ASP.NET](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image2.png)

    <span data-ttu-id="2c878-160">*Tworzenie nowego projektu aplikacji sieci Web ASP.NET*</span><span class="sxs-lookup"><span data-stu-id="2c878-160">*Creating a new ASP.NET Web Application project*</span></span>
3. <span data-ttu-id="2c878-161">W **nowy projekt ASP.NET** okno dialogowe, wybierz opcję **formularzy sieci Web** szablonu, a następnie wybierz **MVC** i **interfejsu API sieci Web** opcje.</span><span class="sxs-lookup"><span data-stu-id="2c878-161">In the **New ASP.NET Project** dialog box, select the **Web Forms** template and select the **MVC** and **Web API** options.</span></span> <span data-ttu-id="2c878-162">Ponadto upewnij się, że **uwierzytelniania** ustawiono opcję **indywidualne konta użytkowników**.</span><span class="sxs-lookup"><span data-stu-id="2c878-162">Also, make sure that the **Authentication** option is set to **Individual User Accounts**.</span></span> <span data-ttu-id="2c878-163">Kliknij przycisk **OK** aby kontynuować.</span><span class="sxs-lookup"><span data-stu-id="2c878-163">Click **OK** to continue.</span></span>

    ![Tworzenie nowego projektu za pomocą szablonu formularzy sieci Web, w tym składników Web API i MVC](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image3.png)

    <span data-ttu-id="2c878-165">*Tworzenie nowego projektu za pomocą szablonu formularzy sieci Web, w tym składników Web API i MVC*</span><span class="sxs-lookup"><span data-stu-id="2c878-165">*Creating a new project with the Web Forms template, including Web API and MVC components*</span></span>
4. <span data-ttu-id="2c878-166">Teraz możesz eksplorować strukturę wygenerowanego rozwiązania.</span><span class="sxs-lookup"><span data-stu-id="2c878-166">You can now explore the structure of the generated solution.</span></span>

    ![Eksplorowanie wygenerowanej rozwiązania](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image4.png)

    <span data-ttu-id="2c878-168">*Eksplorowanie wygenerowanej rozwiązania*</span><span class="sxs-lookup"><span data-stu-id="2c878-168">*Exploring the generated solution*</span></span>

    1. <span data-ttu-id="2c878-169">**Konto:** ten folder zawiera strony formularzy sieci Web do rejestracji, zaloguj się do i zarządzanie kontami użytkowników w aplikacji.</span><span class="sxs-lookup"><span data-stu-id="2c878-169">**Account:** This folder contains the Web Form pages to register, log in to and manage the application's user accounts.</span></span> <span data-ttu-id="2c878-170">Ten folder jest dodawany, gdy **indywidualne konta użytkowników** wybrano opcję uwierzytelniania podczas konfigurowania szablonu projektu formularzy sieci Web.</span><span class="sxs-lookup"><span data-stu-id="2c878-170">This folder is added when the **Individual User Accounts** authentication option is selected during the configuration of the Web Forms project template.</span></span>
    2. <span data-ttu-id="2c878-171">**Modele:** ten folder zawiera klasy reprezentujące dane aplikacji.</span><span class="sxs-lookup"><span data-stu-id="2c878-171">**Models:** This folder will contain the classes that represent your application data.</span></span>
    3. <span data-ttu-id="2c878-172">**Kontrolery** i **widoków**: te foldery są wymagane dla **platformy ASP.NET MVC** i **ASP.NET Web API** składników.</span><span class="sxs-lookup"><span data-stu-id="2c878-172">**Controllers** and **Views**: These folders are required for the **ASP.NET MVC** and **ASP.NET Web API** components.</span></span> <span data-ttu-id="2c878-173">Przedstawimy technologii MVC i interfejs API sieci Web w następnym ćwiczeniach.</span><span class="sxs-lookup"><span data-stu-id="2c878-173">You will explore the MVC and Web API technologies in the next exercises.</span></span>
    4. <span data-ttu-id="2c878-174">**Default.aspx**, **Contact.aspx** i **About.aspx** pliki są wstępnie zdefiniowane stron formularzy sieci Web, które służy jako punkt wyjścia do tworzenia stron, które są specyficzne dla użytkownika aplikacja.</span><span class="sxs-lookup"><span data-stu-id="2c878-174">The **Default.aspx**, **Contact.aspx** and **About.aspx** files are pre-defined Web Form pages that you can use as starting points to build the pages specific to your application.</span></span> <span data-ttu-id="2c878-175">Logikę programistyczną tych plików znajduje się w oddzielnym pliku, nazywane &quot;związanym z kodem&quot; pliku, który ma &quot;. aspx.vb&quot; lub &quot;. aspx.cs&quot; rozszerzenia (w zależności od język używany).</span><span class="sxs-lookup"><span data-stu-id="2c878-175">The programming logic of those files resides in a separate file referred to as the &quot;code-behind&quot; file, which has an &quot;.aspx.vb&quot; or &quot;.aspx.cs&quot; extension (depending on the language used).</span></span> <span data-ttu-id="2c878-176">Logika związanym z kodem działa na serwerze i dynamicznie generuje dane wyjściowe HTML dla strony.</span><span class="sxs-lookup"><span data-stu-id="2c878-176">The code-behind logic runs on the server and dynamically produces the HTML output for your page.</span></span>
    5. <span data-ttu-id="2c878-177">**Site.Master** i **Site.Mobile.Master** stron zdefiniować wygląd i działanie i standardowe zachowanie wszystkich stron w aplikacji.</span><span class="sxs-lookup"><span data-stu-id="2c878-177">The **Site.Master** and **Site.Mobile.Master** pages define the look and feel and the standard behavior of all the pages in the application.</span></span>
5. <span data-ttu-id="2c878-178">Kliknij dwukrotnie **Default.aspx** plik, aby eksplorować zawartość strony.</span><span class="sxs-lookup"><span data-stu-id="2c878-178">Double-click the **Default.aspx** file to explore the content of the page.</span></span>

    ![Eksplorowanie strony Default.aspx](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image5.png)

    <span data-ttu-id="2c878-180">*Eksplorowanie strony Default.aspx*</span><span class="sxs-lookup"><span data-stu-id="2c878-180">*Exploring the Default.aspx page*</span></span>

    > [!NOTE]
    > <span data-ttu-id="2c878-181">**Strony** dyrektywę w górnej części pliku definiuje atrybuty strony formularzy sieci Web.</span><span class="sxs-lookup"><span data-stu-id="2c878-181">The **Page** directive at the top of the file defines the attributes of the Web Forms page.</span></span> <span data-ttu-id="2c878-182">Na przykład **MasterPageFile** atrybut określa ścieżkę do poziomu głównego stronie — w tym przypadku *Site.Master* stronie — i **Inherits** definiuje atrybut osobna klasa kodu strony dziedziczenia.</span><span class="sxs-lookup"><span data-stu-id="2c878-182">For example, the **MasterPageFile** attribute specifies the path to the master page -in this case, the *Site.Master* page- and the **Inherits** attribute defines the code-behind class for the page to inherit.</span></span> <span data-ttu-id="2c878-183">Ta klasa znajduje się w pliku ustalany na podstawie **CodeBehind** atrybutu.</span><span class="sxs-lookup"><span data-stu-id="2c878-183">This class is located in the file determined by the **CodeBehind** attribute.</span></span>
    > 
    > <span data-ttu-id="2c878-184">**Asp: Content** kontroli rzeczywiste zawartością strony (tekst, znaczników i kontrolki) i jest mapowany na **asp: ContentPlaceHolder** kontrolki na stronie głównej.</span><span class="sxs-lookup"><span data-stu-id="2c878-184">The **asp:Content** control holds the actual content of the page (text, markup and controls) and is mapped to a **asp:ContentPlaceHolder** control on the master page.</span></span> <span data-ttu-id="2c878-185">W tym przypadku będzie renderowana zawartość strony wewnątrz *MainContent* kontroli *Site.Master* strony.</span><span class="sxs-lookup"><span data-stu-id="2c878-185">In this case, the page content will be rendered inside the *MainContent* control defined in the *Site.Master* page.</span></span>
6. <span data-ttu-id="2c878-186">Rozwiń **aplikacji\_Start** folderu i zwróć uwagę, **WebApiConfig.cs** pliku.</span><span class="sxs-lookup"><span data-stu-id="2c878-186">Expand the **App\_Start** folder and notice the **WebApiConfig.cs** file.</span></span> <span data-ttu-id="2c878-187">Programu Visual Studio tego pliku są zawarte w wygenerowanym rozwiązaniu, ponieważ dołączył interfejsu API sieci Web podczas konfigurowania projektu z szablonu aplikacji One ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="2c878-187">Visual Studio included that file in the generated solution because you included Web API when configuring your project with the One ASP.NET template.</span></span>
7. <span data-ttu-id="2c878-188">Otwórz **WebApiConfig.cs** pliku.</span><span class="sxs-lookup"><span data-stu-id="2c878-188">Open the **WebApiConfig.cs** file.</span></span> <span data-ttu-id="2c878-189">W *WebApiConfig* klasy można znaleźć konfiguracji skojarzone z internetowego interfejsu API, który mapuje HTTP kieruje do **kontrolerów internetowych interfejsów API**.</span><span class="sxs-lookup"><span data-stu-id="2c878-189">In the *WebApiConfig* class you will find the configuration associated with Web API, which maps HTTP routes to **Web API controllers**.</span></span>

    [!code-csharp[Main](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/samples/sample1.cs)]
8. <span data-ttu-id="2c878-190">Otwórz **RouteConfig.cs** pliku.</span><span class="sxs-lookup"><span data-stu-id="2c878-190">Open the **RouteConfig.cs** file.</span></span> <span data-ttu-id="2c878-191">Wewnątrz *RegisterRoutes* metody, można znaleźć konfiguracji skojarzone z MVC, która mapuje trasy HTTP **kontrolerów MVC**.</span><span class="sxs-lookup"><span data-stu-id="2c878-191">Inside the *RegisterRoutes* method you will find the configuration associated with MVC, which maps HTTP routes to **MVC controllers**.</span></span>

    [!code-csharp[Main](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/samples/sample2.cs)]

<a id="Ex1Task2"></a>
#### <a name="task-2--running-the-solution"></a><span data-ttu-id="2c878-192">Zadanie 2 — uruchamianie rozwiązania</span><span class="sxs-lookup"><span data-stu-id="2c878-192">Task 2 – Running the Solution</span></span>

<span data-ttu-id="2c878-193">W ramach tego zadania będzie uruchomić wygenerowanego rozwiązanie, zapoznaj się z aplikacji i część jej dostępnych funkcji, takich jak ponownego zapisywania adresów URL i wbudowanego uwierzytelniania za pomocą.</span><span class="sxs-lookup"><span data-stu-id="2c878-193">In this task you will run the generated solution, explore the app and some of its features, like URL rewriting and built-in authentication.</span></span>

1. <span data-ttu-id="2c878-194">Aby uruchomić rozwiązanie, naciśnij klawisz **F5** lub kliknij przycisk **Start** znajdujący się na pasku narzędzi.</span><span class="sxs-lookup"><span data-stu-id="2c878-194">To run the solution, press **F5** or click the **Start** button located on the toolbar.</span></span> <span data-ttu-id="2c878-195">Strona główna aplikacji powinna zostać otwarta w przeglądarce.</span><span class="sxs-lookup"><span data-stu-id="2c878-195">The application home page should open in the browser.</span></span>

    ![Uruchamianie rozwiązania](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image6.png)
2. <span data-ttu-id="2c878-197">Sprawdź, czy strony formularzy sieci Web są wywoływane.</span><span class="sxs-lookup"><span data-stu-id="2c878-197">Verify that the Web Forms pages are being invoked.</span></span> <span data-ttu-id="2c878-198">Aby to zrobić, należy dołączyć **/contact.aspx** do adresu URL w pasku adresu i naciśnij klawisz **Enter**.</span><span class="sxs-lookup"><span data-stu-id="2c878-198">To do this, append **/contact.aspx** to the URL in the address bar and press **Enter**.</span></span>

    ![Przyjazne adresy URL](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image7.png)

    <span data-ttu-id="2c878-200">*Przyjazne adresy URL*</span><span class="sxs-lookup"><span data-stu-id="2c878-200">*Friendly URLs*</span></span>

    > [!NOTE]
    > <span data-ttu-id="2c878-201">Jak widać, zmiana adresu URL do **/skontaktuj się z pomocą**.</span><span class="sxs-lookup"><span data-stu-id="2c878-201">As you can see, the URL changes to **/contact**.</span></span> <span data-ttu-id="2c878-202">Począwszy od **platformy ASP.NET 4**możliwości routingu adresów URL zostały dodane do formularzy sieci Web, dzięki czemu można napisać adresy URL, takich jak *[ http://www.mysite.com/products/software ](http://www.mysite.com/products/software)* zamiast  *[http://www.mysite.com/products.aspx?category=software](http://www.mysite.com/products.aspx?category=software)*.</span><span class="sxs-lookup"><span data-stu-id="2c878-202">Starting from **ASP.NET 4**, URL routing capabilities were added to Web Forms, so you can write URLs like *[http://www.mysite.com/products/software](http://www.mysite.com/products/software)* instead of *[http://www.mysite.com/products.aspx?category=software](http://www.mysite.com/products.aspx?category=software)*.</span></span> <span data-ttu-id="2c878-203">Aby uzyskać więcej informacji zobacz [routingu adresów URL](../../../web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/url-routing.md).</span><span class="sxs-lookup"><span data-stu-id="2c878-203">For more information refer to [URL Routing](../../../web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/url-routing.md).</span></span>
3. <span data-ttu-id="2c878-204">Może teraz zapoznać się przepływ uwierzytelniania w aplikacji.</span><span class="sxs-lookup"><span data-stu-id="2c878-204">You will now explore the authentication flow integrated into the application.</span></span> <span data-ttu-id="2c878-205">Aby to zrobić, kliknij przycisk **zarejestrować** w prawym górnym rogu strony.</span><span class="sxs-lookup"><span data-stu-id="2c878-205">To do this, click **Register** in the upper-right corner of the page.</span></span>

    ![Rejestrowanie nowego użytkownika](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image8.png)

    <span data-ttu-id="2c878-207">*Rejestrowanie nowego użytkownika*</span><span class="sxs-lookup"><span data-stu-id="2c878-207">*Registering a new user*</span></span>
4. <span data-ttu-id="2c878-208">W **zarejestrować** wpisz **nazwa_użytkownika** i **hasło**, a następnie kliknij przycisk **zarejestrować**.</span><span class="sxs-lookup"><span data-stu-id="2c878-208">In the **Register** page, enter a **User name** and **Password**, and then click **Register**.</span></span>

    ![Strona rejestracji](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image9.png)

    <span data-ttu-id="2c878-210">*Strona rejestracji*</span><span class="sxs-lookup"><span data-stu-id="2c878-210">*Register page*</span></span>
5. <span data-ttu-id="2c878-211">Aplikacja rejestruje nowe konto, a użytkownik jest uwierzytelniony.</span><span class="sxs-lookup"><span data-stu-id="2c878-211">The application registers the new account, and the user is authenticated.</span></span>

    ![Użytkownik uwierzytelniony](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image10.png)

    <span data-ttu-id="2c878-213">*Użytkownik uwierzytelniony*</span><span class="sxs-lookup"><span data-stu-id="2c878-213">*User authenticated*</span></span>
6. <span data-ttu-id="2c878-214">Wróć do programu Visual Studio, a następnie naciśnij klawisz **SHIFT + F5** Aby zatrzymać debugowanie.</span><span class="sxs-lookup"><span data-stu-id="2c878-214">Go back to Visual Studio and press **SHIFT + F5** to stop debugging.</span></span>

<a id="Exercise2"></a>
### <a name="exercise-2-creating-an-mvc-controller-using-scaffolding"></a><span data-ttu-id="2c878-215">Ćwiczenie 2: Tworzenie szkieletów kontrolera MVC</span><span class="sxs-lookup"><span data-stu-id="2c878-215">Exercise 2: Creating an MVC Controller Using Scaffolding</span></span>

<span data-ttu-id="2c878-216">W tym ćwiczeniu będzie korzystać z platformę ASP.NET tworzenie szkieletów udostępniane przez program Visual Studio, aby utworzyć kontroler składnika ASP.NET MVC 5 z akcjami i widokami Razor do wykonywania operacji CRUD, bez konieczności pisania nawet jednego wiersza kodu.</span><span class="sxs-lookup"><span data-stu-id="2c878-216">In this exercise you will take advantage of the ASP.NET Scaffolding framework provided by Visual Studio to create an ASP.NET MVC 5 controller with actions and Razor views to perform CRUD operations, without writing a single line of code.</span></span> <span data-ttu-id="2c878-217">Proces tworzenia szkieletów użyje Entity Framework Code First generowanie kontekstu danych i schemat bazy danych w bazie danych SQL.</span><span class="sxs-lookup"><span data-stu-id="2c878-217">The scaffolding process will use Entity Framework Code First to generate the data context and the database schema in the SQL database.</span></span>

<span data-ttu-id="2c878-218">**O programu Entity Framework Code najpierw**</span><span class="sxs-lookup"><span data-stu-id="2c878-218">**About Entity Framework Code First**</span></span>

<span data-ttu-id="2c878-219">Entity Framework (EF) to maper obiektowo relacyjny (ORM), który pozwala na tworzenie aplikacji dostęp do danych przez programowania, korzystając z modelu koncepcyjnego aplikacji zamiast programowanie bezpośrednio przy użyciu schematu magazyn relacyjny.</span><span class="sxs-lookup"><span data-stu-id="2c878-219">Entity Framework (EF) is an object-relational mapper (ORM) that enables you to create data access applications by programming with a conceptual application model instead of programming directly using a relational storage schema.</span></span>

<span data-ttu-id="2c878-220">Entity Framework Code First modelowania przepływu pracy pozwala na używanie własnych klas domeny do reprezentowania model, który EF opiera się na podczas wykonywania zapytania, funkcji śledzenia zmian i aktualizacji.</span><span class="sxs-lookup"><span data-stu-id="2c878-220">The Entity Framework Code First modeling workflow allows you to use your own domain classes to represent the model that EF relies on when performing querying, change-tracking and updating functions.</span></span> <span data-ttu-id="2c878-221">Za pomocą Code First przepływ pracy tworzenia oprogramowania, nie trzeba zacząć aplikację przez utworzenie bazy danych lub określenie schematu.</span><span class="sxs-lookup"><span data-stu-id="2c878-221">Using the Code First development workflow, you do not need to begin your application by creating a database or specifying a schema.</span></span> <span data-ttu-id="2c878-222">Zamiast tego można napisać standardowych klas platformy .NET, które określają najbardziej odpowiednie obiekty modelu domeny dla swojej aplikacji i Entity Framework spowoduje utworzenie bazy danych dla Ciebie.</span><span class="sxs-lookup"><span data-stu-id="2c878-222">Instead, you can write standard .NET classes that define the most appropriate domain model objects for your application, and Entity Framework will create the database for you.</span></span>

> [!NOTE]
> <span data-ttu-id="2c878-223">Dowiedz się więcej na temat platformy Entity Framework [tutaj](../../../entity-framework.md).</span><span class="sxs-lookup"><span data-stu-id="2c878-223">You can learn more about Entity Framework [here](../../../entity-framework.md).</span></span>


<a id="Ex2Task1"></a>
#### <a name="task-1--creating-a-new-model"></a><span data-ttu-id="2c878-224">Zadanie 1 — Tworzenie nowego modelu</span><span class="sxs-lookup"><span data-stu-id="2c878-224">Task 1 – Creating a New Model</span></span>

<span data-ttu-id="2c878-225">Teraz określi **osoby** klasy, która będzie model używany przez proces tworzenia szkieletu do tworzenia widoków i kontrolerów MVC.</span><span class="sxs-lookup"><span data-stu-id="2c878-225">You will now define a **Person** class, which will be the model used by the scaffolding process to create the MVC controller and the views.</span></span> <span data-ttu-id="2c878-226">Rozpocznie się przez utworzenie **osoby** klasy modelu, a operacje CRUD na kontrolerze zostaną automatycznie utworzone przy użyciu funkcji tworzenia szkieletów.</span><span class="sxs-lookup"><span data-stu-id="2c878-226">You will start by creating a **Person** model class, and the CRUD operations in the controller will be automatically created using scaffolding features.</span></span>

1. <span data-ttu-id="2c878-227">Otwórz **Visual Studio Express 2013 for Web** i **MyHybridSite.sln** rozwiązanie znajduje się w **/Ex2-MvcScaffolding/początkowy w źródle** folderu.</span><span class="sxs-lookup"><span data-stu-id="2c878-227">Open **Visual Studio Express 2013 for Web** and the **MyHybridSite.sln** solution located in the **Source/Ex2-MvcScaffolding/Begin** folder.</span></span> <span data-ttu-id="2c878-228">Alternatywnie można kontynuować z rozwiązaniem uzyskanym w poprzednim ćwiczeniu.</span><span class="sxs-lookup"><span data-stu-id="2c878-228">Alternatively, you can continue with the solution that you obtained in the previous exercise.</span></span>
2. <span data-ttu-id="2c878-229">W **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy **modeli** folderu **MyHybridSite** projektu, a następnie wybierz **Dodaj | Klasa...** .</span><span class="sxs-lookup"><span data-stu-id="2c878-229">In **Solution Explorer**, right-click the **Models** folder of the **MyHybridSite** project and select **Add | Class...**.</span></span>

    ![Dodawanie klasy modelu osoby](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image11.png)

    <span data-ttu-id="2c878-231">*Dodawanie klasy modelu osoby*</span><span class="sxs-lookup"><span data-stu-id="2c878-231">*Adding the Person model class*</span></span>
3. <span data-ttu-id="2c878-232">W **Dodaj nowy element** okno dialogowe, nazwij plik *osoba.cs* i kliknij przycisk **Dodaj**.</span><span class="sxs-lookup"><span data-stu-id="2c878-232">In the **Add New Item** dialog box, name the file *Person.cs* and click **Add**.</span></span>

    ![Tworzenie klasy modelu osoby](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image12.png)

    <span data-ttu-id="2c878-234">*Tworzenie klasy modelu osoby*</span><span class="sxs-lookup"><span data-stu-id="2c878-234">*Creating the Person model class*</span></span>
4. <span data-ttu-id="2c878-235">Zastąp zawartość **osoba.cs** pliku następującym kodem.</span><span class="sxs-lookup"><span data-stu-id="2c878-235">Replace the content of the **Person.cs** file with the following code.</span></span> <span data-ttu-id="2c878-236">Naciśnij klawisz **CTRL + S** Aby zapisać zmiany.</span><span class="sxs-lookup"><span data-stu-id="2c878-236">Press **CTRL + S** to save the changes.</span></span>

    <span data-ttu-id="2c878-237">(Code Snippet — *PersonClass BringingTogetherOneAspNet - Ex2 -*)</span><span class="sxs-lookup"><span data-stu-id="2c878-237">(Code Snippet - *BringingTogetherOneAspNet - Ex2 - PersonClass*)</span></span>

    [!code-csharp[Main](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/samples/sample3.cs)]
5. <span data-ttu-id="2c878-238">W **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy **MyHybridSite** projektu, a następnie wybierz **kompilacji**, lub naciśnij **CTRL + SHIFT + B** do skompilowania projektu.</span><span class="sxs-lookup"><span data-stu-id="2c878-238">In **Solution Explorer**, right-click the **MyHybridSite** project and select **Build**, or press **CTRL + SHIFT + B** to build the project.</span></span>

<a id="Ex2Task2"></a>
#### <a name="task-2--creating-an-mvc-controller"></a><span data-ttu-id="2c878-239">Zadanie 2 — Tworzenie kontrolera MVC</span><span class="sxs-lookup"><span data-stu-id="2c878-239">Task 2 – Creating an MVC Controller</span></span>

<span data-ttu-id="2c878-240">Teraz, gdy **osoby** model został utworzony, ponieważ będziesz korzystać scaffolding ASP.NET MVC za pomocą platformy Entity Framework do tworzenia akcji kontrolera CRUD i widoków **osoby**.</span><span class="sxs-lookup"><span data-stu-id="2c878-240">Now that the **Person** model is created, you will use ASP.NET MVC scaffolding with Entity Framework to create the CRUD controller actions and views for **Person**.</span></span>

1. <span data-ttu-id="2c878-241">W **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy **kontrolerów** folderu **MyHybridSite** projektu, a następnie wybierz **Dodaj | Nowy element szkieletowy...** .</span><span class="sxs-lookup"><span data-stu-id="2c878-241">In **Solution Explorer**, right-click the **Controllers** folder of the **MyHybridSite** project and select **Add | New Scaffolded Item...**.</span></span>

    ![Tworzenie nowego szkieletu kontrolera](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image13.png)

    <span data-ttu-id="2c878-243">*Tworzenie nowego kontrolera szkielet*</span><span class="sxs-lookup"><span data-stu-id="2c878-243">*Creating a new Scaffolded Controller*</span></span>
2. <span data-ttu-id="2c878-244">W **Dodawanie szkieletu** okno dialogowe, wybierz opcję **kontroler MVC 5 z widokami używający narzędzia Entity Framework** a następnie kliknij przycisk **Dodaj.**</span><span class="sxs-lookup"><span data-stu-id="2c878-244">In the **Add Scaffold** dialog box, select **MVC 5 Controller with views, using Entity Framework** and then click **Add.**</span></span>

    ![Wybieranie kontroler MVC 5 z widokami i Entity Framework](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image14.png)

    <span data-ttu-id="2c878-246">*Wybieranie kontroler MVC 5 z widokami i Entity Framework*</span><span class="sxs-lookup"><span data-stu-id="2c878-246">*Selecting MVC 5 Controller with views and Entity Framework*</span></span>
3. <span data-ttu-id="2c878-247">Ustaw *MvcPersonController* jako **nazwy kontrolera**, wybierz opcję **używać asynchronicznych akcji kontrolera** opcji, a następnie wybierz pozycję **osoby (MyHybridSite.Models)**  jako **klasa modelu**.</span><span class="sxs-lookup"><span data-stu-id="2c878-247">Set *MvcPersonController* as the **Controller name**, select the **Use async controller actions** option and select **Person (MyHybridSite.Models)** as the **Model class**.</span></span>

    ![Dodawanie kontrolera MVC za pomocą tworzenia szkieletów](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image15.png)

    <span data-ttu-id="2c878-249">*Dodawanie kontrolera MVC za pomocą tworzenia szkieletów*</span><span class="sxs-lookup"><span data-stu-id="2c878-249">*Adding an MVC controller with scaffolding*</span></span>
4. <span data-ttu-id="2c878-250">W obszarze **klasa kontekstu danych**, kliknij przycisk **nowy kontekst danych...** .</span><span class="sxs-lookup"><span data-stu-id="2c878-250">Under **Data context class**, click **New data context...**.</span></span>

    ![Tworząc nowy kontekst danych](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image16.png)

    <span data-ttu-id="2c878-252">*Tworząc nowy kontekst danych*</span><span class="sxs-lookup"><span data-stu-id="2c878-252">*Creating a new data context*</span></span>
5. <span data-ttu-id="2c878-253">W **nowy kontekst danych** okno dialogowe, nazwa nowy kontekst danych *PersonContext* i kliknij przycisk **Dodaj**.</span><span class="sxs-lookup"><span data-stu-id="2c878-253">In the **New Data Context** dialog box, name the new data context *PersonContext* and click **Add**.</span></span>

    ![Tworzenie nowego PersonContext](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image17.png)

    <span data-ttu-id="2c878-255">*Tworzenie nowego typu PersonContext*</span><span class="sxs-lookup"><span data-stu-id="2c878-255">*Creating the new PersonContext type*</span></span>
6. <span data-ttu-id="2c878-256">Kliknij przycisk **Dodaj** do utworzenia nowego kontrolera dla **osoby** z tworzenia szkieletów.</span><span class="sxs-lookup"><span data-stu-id="2c878-256">Click **Add** to create the new controller for **Person** with scaffolding.</span></span> <span data-ttu-id="2c878-257">Program Visual Studio wygeneruje wtedy akcji kontrolera, kontekst danych osoby i widokami Razor.</span><span class="sxs-lookup"><span data-stu-id="2c878-257">Visual Studio will then generate the controller actions, the Person data context and the Razor views.</span></span>

    ![Po utworzeniu kontroler MVC z tworzenia szkieletów](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image18.png)

    <span data-ttu-id="2c878-259">*Po utworzeniu kontroler MVC z tworzenia szkieletów*</span><span class="sxs-lookup"><span data-stu-id="2c878-259">*After creating the MVC controller with scaffolding*</span></span>
7. <span data-ttu-id="2c878-260">Otwórz **MvcPersonController.cs** w pliku **kontrolerów** folderu.</span><span class="sxs-lookup"><span data-stu-id="2c878-260">Open the **MvcPersonController.cs** file in the **Controllers** folder.</span></span> <span data-ttu-id="2c878-261">Należy zauważyć, że metody akcji CRUD został wygenerowany automatycznie.</span><span class="sxs-lookup"><span data-stu-id="2c878-261">Notice that the CRUD action methods have been generated automatically.</span></span>

    [!code-csharp[Main](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/samples/sample4.cs)]

    > [!NOTE]
    > <span data-ttu-id="2c878-262">Wybierając **używać asynchronicznych akcji kontrolera** pole wyboru szkieletu opcje w poprzednich krokach, program Visual Studio generuje metod asynchronicznych akcji dla wszystkich akcji, które obejmują dostęp do kontekstu danych osoby.</span><span class="sxs-lookup"><span data-stu-id="2c878-262">By selecting the **Use async controller actions** check box from the scaffolding options in the previous steps, Visual Studio generates asynchronous action methods for all actions that involve access to the Person data context.</span></span> <span data-ttu-id="2c878-263">Zalecane jest, że używasz metod asynchronicznych akcji dla długotrwałych, innego niż procesor CPU powiązane żądania w celu unikania blokowania serwera sieci Web z wykonywania pracy, podczas przetwarzania żądania.</span><span class="sxs-lookup"><span data-stu-id="2c878-263">It is recommended that you use asynchronous action methods for long-running, non-CPU bound requests to avoid blocking the Web server from performing work while the request is being processed.</span></span>

<a id="Ex2Task3"></a>
#### <a name="task-3--running-the-solution"></a><span data-ttu-id="2c878-264">Zadanie 3 — uruchamianie rozwiązania</span><span class="sxs-lookup"><span data-stu-id="2c878-264">Task 3 – Running the Solution</span></span>

<span data-ttu-id="2c878-265">W tym zadaniu uruchomisz rozwiązanie ponownie, aby sprawdzić, czy widoków dla **osoby** działają zgodnie z oczekiwaniami.</span><span class="sxs-lookup"><span data-stu-id="2c878-265">In this task, you will run the solution again to verify that the views for **Person** are working as expected.</span></span> <span data-ttu-id="2c878-266">Należy dodać nową osobę, aby sprawdzić, czy zostaje pomyślnie zapisane w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="2c878-266">You will add a new person to verify that it is successfully saved to the database.</span></span>

1. <span data-ttu-id="2c878-267">Naciśnij klawisz **F5** Aby uruchomić rozwiązanie.</span><span class="sxs-lookup"><span data-stu-id="2c878-267">Press **F5** to run the solution.</span></span>
2. <span data-ttu-id="2c878-268">Przejdź do **/MvcPerson**.</span><span class="sxs-lookup"><span data-stu-id="2c878-268">Navigate to **/MvcPerson**.</span></span> <span data-ttu-id="2c878-269">Widok szkieletu, który pokazuje listę osób, powinny być wyświetlane.</span><span class="sxs-lookup"><span data-stu-id="2c878-269">The scaffolded view that shows the list of people should appear.</span></span>
3. <span data-ttu-id="2c878-270">Kliknij przycisk **Utwórz nową** na dodawanie nowej osoby.</span><span class="sxs-lookup"><span data-stu-id="2c878-270">Click **Create New** to add a new person.</span></span>

    ![Przejdź do szkieletu widoków MVC](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image19.png)

    <span data-ttu-id="2c878-272">*Przejdź do szkieletu widoków MVC*</span><span class="sxs-lookup"><span data-stu-id="2c878-272">*Navigating to the scaffolded MVC views*</span></span>
4. <span data-ttu-id="2c878-273">W **Utwórz** wyświetlanie, podaj **nazwa** i **wiek** osoby, a następnie kliknij przycisk **Utwórz**.</span><span class="sxs-lookup"><span data-stu-id="2c878-273">In the **Create** view, provide a **Name** and an **Age** for the person, and click **Create**.</span></span>

    ![Dodawanie nowej osoby](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image20.png)

    <span data-ttu-id="2c878-275">*Dodawanie nowej osoby*</span><span class="sxs-lookup"><span data-stu-id="2c878-275">*Adding a new person*</span></span>
5. <span data-ttu-id="2c878-276">Nowej osoby zostanie dodany do listy.</span><span class="sxs-lookup"><span data-stu-id="2c878-276">The new person is added to the list.</span></span> <span data-ttu-id="2c878-277">Na liście elementów kliknij **szczegóły** do wyświetlenia widoku szczegółów osoby.</span><span class="sxs-lookup"><span data-stu-id="2c878-277">In the element list, click **Details** to display the person's details view.</span></span> <span data-ttu-id="2c878-278">Następnie w **szczegóły** wyświetlić, kliknij przycisk **powrót do listy** aby powrócić do widoku listy.</span><span class="sxs-lookup"><span data-stu-id="2c878-278">Then, in the **Details** view, click **Back to List** to go back to the list view.</span></span>

    ![Widok szczegółów osoby](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image21.png)

    <span data-ttu-id="2c878-280">*Widok szczegółów osoby*</span><span class="sxs-lookup"><span data-stu-id="2c878-280">*Person's details view*</span></span>
6. <span data-ttu-id="2c878-281">Kliknij przycisk **Usuń** łącze, aby usunąć osoby.</span><span class="sxs-lookup"><span data-stu-id="2c878-281">Click the **Delete** link to delete the person.</span></span> <span data-ttu-id="2c878-282">W **Usuń** wyświetlić, kliknij przycisk **Usuń** o potwierdzenie operacji.</span><span class="sxs-lookup"><span data-stu-id="2c878-282">In the **Delete** view, click **Delete** to confirm the operation.</span></span>

    ![Usuwanie osoby](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image22.png)

    <span data-ttu-id="2c878-284">*Usuwanie osoby*</span><span class="sxs-lookup"><span data-stu-id="2c878-284">*Deleting a person*</span></span>
7. <span data-ttu-id="2c878-285">Wróć do programu Visual Studio, a następnie naciśnij klawisz **SHIFT + F5** Aby zatrzymać debugowanie.</span><span class="sxs-lookup"><span data-stu-id="2c878-285">Go back to Visual Studio and press **SHIFT + F5** to stop debugging.</span></span>

<a id="Exercise3"></a>
### <a name="exercise-3-creating-a-web-api-controller-using-scaffolding"></a><span data-ttu-id="2c878-286">Ćwiczenie 3: Tworzenie szkieletów kontrolera interfejsu API sieci Web</span><span class="sxs-lookup"><span data-stu-id="2c878-286">Exercise 3: Creating a Web API Controller Using Scaffolding</span></span>

<span data-ttu-id="2c878-287">Strukturę interfejsu API sieci Web jest częścią stosu ASP.NET i mają ułatwić Implementowanie usług HTTP, zwykle wysyłania i odbierania danych w formacie JSON lub XML za pomocą interfejsu API RESTful.</span><span class="sxs-lookup"><span data-stu-id="2c878-287">The Web API framework is part of the ASP.NET Stack and designed to make implementing HTTP services easier, generally sending and receiving JSON- or XML-formatted data through a RESTful API.</span></span>

<span data-ttu-id="2c878-288">W tym ćwiczeniu funkcja tworzenia szkieletu ASP.NET będzie użyć ponownie, aby wygenerować kontroler Web API.</span><span class="sxs-lookup"><span data-stu-id="2c878-288">In this exercise, you will use ASP.NET Scaffolding again to generate a Web API controller.</span></span> <span data-ttu-id="2c878-289">Będzie używać tego samego **osoby** i **PersonContext** klas z poprzednim ćwiczeniu, aby zapewnić te same dane osoby w formacie JSON.</span><span class="sxs-lookup"><span data-stu-id="2c878-289">You will use the same **Person** and **PersonContext** classes from the previous exercise to provide the same person data in JSON format.</span></span> <span data-ttu-id="2c878-290">Zobaczysz, jak udostępnić te same zasoby na różne sposoby, w ramach tej samej aplikacji ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="2c878-290">You will see how you can expose the same resources in different ways within the same ASP.NET application.</span></span>

<a id="Ex3Task1"></a>
#### <a name="task-1--creating-a-web-api-controller"></a><span data-ttu-id="2c878-291">Zadanie 1 — Tworzenie kontrolera interfejsu API sieci Web</span><span class="sxs-lookup"><span data-stu-id="2c878-291">Task 1 – Creating a Web API Controller</span></span>

<span data-ttu-id="2c878-292">W tym zadaniu zostanie utworzony nowy **kontroler internetowego interfejsu API** , spowodują ujawnienie danych osobowych w formacie usprawnia maszyny, takich jak JSON.</span><span class="sxs-lookup"><span data-stu-id="2c878-292">In this task you will create a new **Web API Controller** that will expose the person data in a machine-consumable format like JSON.</span></span>

1. <span data-ttu-id="2c878-293">Jeśli nie jest jeszcze otwarty, otwórz **Visual Studio Express 2013 for Web** , a następnie otwórz **MyHybridSite.sln** rozwiązanie znajduje się w **/Ex3-WebAPI/początkowy w źródle** folderu.</span><span class="sxs-lookup"><span data-stu-id="2c878-293">If not already opened, open **Visual Studio Express 2013 for Web** and open the **MyHybridSite.sln** solution located in the **Source/Ex3-WebAPI/Begin** folder.</span></span> <span data-ttu-id="2c878-294">Alternatywnie można kontynuować z rozwiązaniem uzyskanym w poprzednim ćwiczeniu.</span><span class="sxs-lookup"><span data-stu-id="2c878-294">Alternatively, you can continue with the solution that you obtained in the previous exercise.</span></span>

    > [!NOTE]
    > <span data-ttu-id="2c878-295">W przypadku uruchomienia za pomocą rozwiązania Rozpocznij od 3 do wykonywania, naciśnij klawisz **CTRL + SHIFT + B** do skompilowania rozwiązania.</span><span class="sxs-lookup"><span data-stu-id="2c878-295">If you start with the Begin solution from Exercise 3, press **CTRL + SHIFT + B** to build the solution.</span></span>
2. <span data-ttu-id="2c878-296">W **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy **kontrolerów** folderu **MyHybridSite** projektu, a następnie wybierz **Dodaj | Nowy element szkieletowy...** .</span><span class="sxs-lookup"><span data-stu-id="2c878-296">In **Solution Explorer**, right-click the **Controllers** folder of the **MyHybridSite** project and select **Add | New Scaffolded Item...**.</span></span>

    ![Tworzenie nowego szkieletu kontrolera](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image23.png)

    <span data-ttu-id="2c878-298">*Tworzenie nowego szkieletu kontrolera*</span><span class="sxs-lookup"><span data-stu-id="2c878-298">*Creating a new scaffolded Controller*</span></span>
3. <span data-ttu-id="2c878-299">W **Dodawanie szkieletu** okno dialogowe, wybierz opcję **interfejsu API sieci Web** w lewym okienku, a następnie **kontroler internetowego interfejsu API 2 z akcjami używający narzędzia Entity Framework** w środkowym okienku, a następnie kliknij przycisk  **Dodaj.**</span><span class="sxs-lookup"><span data-stu-id="2c878-299">In the **Add Scaffold** dialog box, select **Web API** in the left pane, then **Web API 2 Controller with actions, using Entity Framework** in the middle pane and then click **Add.**</span></span>

    <span data-ttu-id="2c878-300">![Wybieranie kontroler 2 internetowego interfejsu API z akcjami i Entity Framework](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image24.png "wybierając kontroler internetowego interfejsu API 2 z akcjami i Entity Framework")</span><span class="sxs-lookup"><span data-stu-id="2c878-300">![Selecting Web API 2 Controller with actions and Entity Framework](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image24.png "Selecting Web API 2 Controller with actions and Entity Framework")</span></span>

    <span data-ttu-id="2c878-301">*Wybieranie kontroler internetowego interfejsu API 2 z akcjami i Entity Framework*</span><span class="sxs-lookup"><span data-stu-id="2c878-301">*Selecting Web API 2 Controller with actions and Entity Framework*</span></span>
4. <span data-ttu-id="2c878-302">Ustaw *ApiPersonController* jako **nazwy kontrolera**, wybierz opcję **używać asynchronicznych akcji kontrolera** opcji, a następnie wybierz pozycję **osoby (MyHybridSite.Models)**  i **PersonContext (MyHybridSite.Models)** jako **modelu** i **kontekstu danych** klasy odpowiednio.</span><span class="sxs-lookup"><span data-stu-id="2c878-302">Set *ApiPersonController* as the **Controller name**, select the **Use async controller actions** option and select **Person (MyHybridSite.Models)** and **PersonContext (MyHybridSite.Models)** as the **Model** and **Data context** classes respectively.</span></span> <span data-ttu-id="2c878-303">Następnie kliknij przycisk **Dodaj**.</span><span class="sxs-lookup"><span data-stu-id="2c878-303">Then click **Add**.</span></span>

    <span data-ttu-id="2c878-304">![Dodawanie kontrolera interfejsu API sieci Web za pomocą tworzenia szkieletów](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image25.png "Dodawanie kontrolera interfejsu API sieci Web za pomocą tworzenia szkieletów")</span><span class="sxs-lookup"><span data-stu-id="2c878-304">![Adding a Web API Controller with scaffolding](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image25.png "Adding a Web API controller with scaffolding")</span></span>

    <span data-ttu-id="2c878-305">*Dodawanie kontrolera interfejsu API sieci Web za pomocą tworzenia szkieletów*</span><span class="sxs-lookup"><span data-stu-id="2c878-305">*Adding a Web API controller with scaffolding*</span></span>
5. <span data-ttu-id="2c878-306">Program Visual Studio wygeneruje wtedy **ApiPersonController** klasie z atrybutem cztery akcje CRUD do pracy z danymi.</span><span class="sxs-lookup"><span data-stu-id="2c878-306">Visual Studio will then generate the **ApiPersonController** class with the four CRUD actions to work with your data.</span></span>

    <span data-ttu-id="2c878-307">![Po utworzeniu kontroler Web API za pomocą tworzenia szkieletów](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image26.png "po utworzeniu kontroler Web API za pomocą tworzenia szkieletów")</span><span class="sxs-lookup"><span data-stu-id="2c878-307">![After creating the Web API controller with scaffolding](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image26.png "After creating the Web API controller with scaffolding")</span></span>

    <span data-ttu-id="2c878-308">*Po utworzeniu kontroler Web API za pomocą tworzenia szkieletów*</span><span class="sxs-lookup"><span data-stu-id="2c878-308">*After creating the Web API controller with scaffolding*</span></span>
6. <span data-ttu-id="2c878-309">Otwórz **ApiPersonController.cs** plik i sprawdzić *GetPeople* metody akcji.</span><span class="sxs-lookup"><span data-stu-id="2c878-309">Open the **ApiPersonController.cs** file and inspect the *GetPeople* action method.</span></span> <span data-ttu-id="2c878-310">Ta metoda wysyła zapytanie pola db **PersonContext** typu, aby pobrać dane osoby.</span><span class="sxs-lookup"><span data-stu-id="2c878-310">This method queries the db field of **PersonContext** type in order to get the people data.</span></span>

    [!code-csharp[Main](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/samples/sample5.cs)]
7. <span data-ttu-id="2c878-311">Teraz zauważyć komentarz powyżej definicję metody.</span><span class="sxs-lookup"><span data-stu-id="2c878-311">Now notice the comment above the method definition.</span></span> <span data-ttu-id="2c878-312">Zawiera identyfikator URI, który udostępnia tę akcję, który będzie używany w ramach następnego zadania.</span><span class="sxs-lookup"><span data-stu-id="2c878-312">It provides the URI that exposes this action which you will use in the next task.</span></span>

    [!code-csharp[Main](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/samples/sample6.cs)]

    > [!NOTE]
    > <span data-ttu-id="2c878-313">Domyślnie interfejs API sieci Web jest skonfigurowany do zapytań, aby przechwycić */interfejs API* ścieżki w celu uniknięcia kolizji z kontrolerów MVC.</span><span class="sxs-lookup"><span data-stu-id="2c878-313">By default, Web API is configured to catch the queries to the */api* path to avoid collisions with MVC controllers.</span></span> <span data-ttu-id="2c878-314">Jeśli potrzebujesz zmienić to ustawienie, zapoznaj się [routingu ASP.NET Web API](../../../web-api/overview/web-api-routing-and-actions/routing-in-aspnet-web-api.md).</span><span class="sxs-lookup"><span data-stu-id="2c878-314">If you need to change this setting, refer to [Routing in ASP.NET Web API](../../../web-api/overview/web-api-routing-and-actions/routing-in-aspnet-web-api.md).</span></span>

<a id="Ex3Task2"></a>
#### <a name="task-2--running-the-solution"></a><span data-ttu-id="2c878-315">Zadanie 2 — uruchamianie rozwiązania</span><span class="sxs-lookup"><span data-stu-id="2c878-315">Task 2 – Running the Solution</span></span>

<span data-ttu-id="2c878-316">W ramach tego zadania będzie używać programu Internet Explorer **narzędzi deweloperskich F12** Aby sprawdzić pełną odpowiedź z kontrolera interfejsu API sieci Web.</span><span class="sxs-lookup"><span data-stu-id="2c878-316">In this task you will use the Internet Explorer **F12 developer tools** to inspect the full response from the Web API controller.</span></span> <span data-ttu-id="2c878-317">Zobaczysz, jak przechwytywać ruch sieciowy, aby uzyskać lepszy wgląd w dane aplikacji.</span><span class="sxs-lookup"><span data-stu-id="2c878-317">You will see how you can capture network traffic to get more insight into your application data.</span></span>

> [!NOTE]
> <span data-ttu-id="2c878-318">Upewnij się, że **programu Internet Explorer** wybrano **Start** znajdujący się na pasku narzędzi programu Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="2c878-318">Make sure that **Internet Explorer** is selected in the **Start** button located on the Visual Studio toolbar.</span></span>
> 
> ![Opcja program Internet Explorer](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image27.png)
> 
> <span data-ttu-id="2c878-320">**Narzędzi deweloperskich F12** mają szeroki zestaw funkcji, która nie została uwzględniona w tym hands-na-laboratorium.</span><span class="sxs-lookup"><span data-stu-id="2c878-320">The **F12 developer tools** have a wide set of functionality that is not covered in this hands-on-lab.</span></span> <span data-ttu-id="2c878-321">Aby dowiedzieć się więcej na ten temat, zapoznaj się [przy użyciu narzędzi deweloperskich F12](https://msdn.microsoft.com/library/ie/bg182326(v=vs.85)).</span><span class="sxs-lookup"><span data-stu-id="2c878-321">If you want to learn more about it, refer to [Using the F12 developer tools](https://msdn.microsoft.com/library/ie/bg182326(v=vs.85)).</span></span>


1. <span data-ttu-id="2c878-322">Naciśnij klawisz **F5** Aby uruchomić rozwiązanie.</span><span class="sxs-lookup"><span data-stu-id="2c878-322">Press **F5** to run the solution.</span></span>

    > [!NOTE]
    > <span data-ttu-id="2c878-323">Aby można było wykonać to zadanie prawidłowo, aplikacja musi mieć danych.</span><span class="sxs-lookup"><span data-stu-id="2c878-323">In order to follow this task correctly, your application needs to have data.</span></span> <span data-ttu-id="2c878-324">Jeśli baza danych jest pusta, można wrócić do zadanie 3 w wersji 2 wykonywania i postępuj zgodnie z instrukcjami na temat tworzenia nowej osoby za pomocą widoków MVC.</span><span class="sxs-lookup"><span data-stu-id="2c878-324">If your database is empty, you can go back to Task 3 in Exercise 2 and follow the steps on how to create a new person using the MVC views.</span></span>
2. <span data-ttu-id="2c878-325">W przeglądarce naciśnij **F12** otworzyć **narzędzi deweloperskich** panelu.</span><span class="sxs-lookup"><span data-stu-id="2c878-325">In the browser, press **F12** to open the **Developer Tools** panel.</span></span> <span data-ttu-id="2c878-326">Naciśnij klawisz **CTRL** + **4** lub kliknij przycisk **sieci** ikonę, a następnie kliknij przycisk zieloną strzałkę, aby rozpocząć przechwytywanie ruchu sieciowego.</span><span class="sxs-lookup"><span data-stu-id="2c878-326">Press **CTRL** + **4** or click the **Network** icon, and then click the green arrow button to begin capturing network traffic.</span></span>

    <span data-ttu-id="2c878-327">![Inicjowanie interfejsu API sieci Web, przechwytywanie sieci](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image28.png "przechwytywania sieci Inicjowanie interfejsu API sieci Web")</span><span class="sxs-lookup"><span data-stu-id="2c878-327">![Initiating Web API network capture](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image28.png "Initiating Web API network capture")</span></span>

    <span data-ttu-id="2c878-328">*Inicjuje Przechwytywanie sieci interfejsu API sieci Web*</span><span class="sxs-lookup"><span data-stu-id="2c878-328">*Initiating Web API network capture*</span></span>
3. <span data-ttu-id="2c878-329">Dołącz **interfejsu api/ApiPerson** do adresu URL w pasku adresu przeglądarki.</span><span class="sxs-lookup"><span data-stu-id="2c878-329">Append **api/ApiPerson** to the URL in the browser's address bar.</span></span> <span data-ttu-id="2c878-330">Teraz Sprawdź szczegóły odpowiedzi z **ApiPersonController**.</span><span class="sxs-lookup"><span data-stu-id="2c878-330">You will now inspect the details of the response from the **ApiPersonController**.</span></span>

    <span data-ttu-id="2c878-331">![Trwa pobieranie danych osobowych za pomocą interfejsu API sieci Web](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image29.png "podczas pobierania danych osobowych za pomocą interfejsu API sieci Web")</span><span class="sxs-lookup"><span data-stu-id="2c878-331">![Retrieving person data through Web API](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image29.png "Retrieving person data through Web API")</span></span>

    <span data-ttu-id="2c878-332">*Trwa pobieranie danych osobowych za pomocą interfejsu API sieci Web*</span><span class="sxs-lookup"><span data-stu-id="2c878-332">*Retrieving person data through Web API*</span></span>

    > [!NOTE]
    > <span data-ttu-id="2c878-333">Po zakończeniu pobierania, pojawi się akcję przy użyciu pobranego pliku.</span><span class="sxs-lookup"><span data-stu-id="2c878-333">Once the download finishes, you will be prompted to make an action with the downloaded file.</span></span> <span data-ttu-id="2c878-334">Pozostaw to okno dialogowe otwarte, aby móc obejrzeć zawartość odpowiedzi za pomocą okna narzędzia dla deweloperów.</span><span class="sxs-lookup"><span data-stu-id="2c878-334">Leave the dialog box open in order to be able to watch the response content through the Developers Tool window.</span></span>
4. <span data-ttu-id="2c878-335">Teraz Sprawdź treść odpowiedzi.</span><span class="sxs-lookup"><span data-stu-id="2c878-335">Now you will inspect the body of the response.</span></span> <span data-ttu-id="2c878-336">Aby to zrobić, kliknij przycisk **szczegóły** kartę, a następnie kliknij przycisk **treść odpowiedzi**.</span><span class="sxs-lookup"><span data-stu-id="2c878-336">To do this, click the **Details** tab and then click **Response body**.</span></span> <span data-ttu-id="2c878-337">Możesz sprawdzić, czy pobrane dane znajduje się lista obiektów z właściwościami **identyfikator**, **nazwa** i **wiek** odpowiadające **osoby** Klasa.</span><span class="sxs-lookup"><span data-stu-id="2c878-337">You can check that the downloaded data is a list of objects with the properties **Id**, **Name** and **Age** that correspond to the **Person** class.</span></span>

    <span data-ttu-id="2c878-338">![Przeglądanie sieci Web w treści odpowiedzi interfejsu API](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image30.png "przeglądania sieci Web w treści odpowiedzi interfejsu API")</span><span class="sxs-lookup"><span data-stu-id="2c878-338">![Viewing Web API Response Body](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image30.png "Viewing Web API Response Body")</span></span>

    <span data-ttu-id="2c878-339">*Treść odpowiedzi interfejsu API sieci Web wyświetlania*</span><span class="sxs-lookup"><span data-stu-id="2c878-339">*Viewing Web API Response Body*</span></span>

<a id="Ex3Task3"></a>
#### <a name="task-3--adding-web-api-help-pages"></a><span data-ttu-id="2c878-340">Zadanie 3 — Dodawanie stron pomocy interfejsu API sieci Web</span><span class="sxs-lookup"><span data-stu-id="2c878-340">Task 3 – Adding Web API Help Pages</span></span>

<span data-ttu-id="2c878-341">Podczas tworzenia internetowego interfejsu API, warto utworzyć stronę pomocy, aby inni deweloperzy powinni wiedzieć, jak wywołać interfejs API.</span><span class="sxs-lookup"><span data-stu-id="2c878-341">When you create a Web API, it is useful to create a help page so that other developers will know how to call your API.</span></span> <span data-ttu-id="2c878-342">Należy utworzyć i ręcznie zaktualizuj strony dokumentacji, ale zaleca się automatyczne generowanie je, aby uniknąć konieczności wykonaj prac konserwacyjnych.</span><span class="sxs-lookup"><span data-stu-id="2c878-342">You could create and update the documentation pages manually, but it is better to auto-generate them to avoid having to do maintenance work.</span></span> <span data-ttu-id="2c878-343">W tym zadaniu użyjesz pakietu Nuget do automatycznego generowania strony pomocy internetowego interfejsu API do rozwiązania.</span><span class="sxs-lookup"><span data-stu-id="2c878-343">In this task you will use a Nuget package to automatically generate Web API help pages to the solution.</span></span>

1. <span data-ttu-id="2c878-344">Z **narzędzia** menu w programie Visual Studio, wybierz **Menedżer pakietów biblioteki**, a następnie kliknij przycisk **Konsola Menedżera pakietów**.</span><span class="sxs-lookup"><span data-stu-id="2c878-344">From the **Tools** menu in Visual Studio, select **Library Package Manager**, and then click **Package Manager Console**.</span></span>
2. <span data-ttu-id="2c878-345">W **Konsola Menedżera pakietów** okna, wykonaj następujące polecenie:</span><span class="sxs-lookup"><span data-stu-id="2c878-345">In the **Package Manager Console** window, execute the following command:</span></span>

    [!code-powershell[Main](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/samples/sample7.ps1)]

    > [!NOTE]
    > <span data-ttu-id="2c878-346">**Microsoft.AspNet.WebApi.HelpPage** pakiet instaluje konieczne zestawy i dodaje widoków MVC dla stron pomocy, w obszarze **obszarów/HelpPage** folderu.</span><span class="sxs-lookup"><span data-stu-id="2c878-346">The **Microsoft.AspNet.WebApi.HelpPage** package installs the necessary assemblies and adds MVC Views for the help pages under the **Areas/HelpPage** folder.</span></span>

    <span data-ttu-id="2c878-347">![Obszar HelpPage](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image31.png "HelpPage obszaru")</span><span class="sxs-lookup"><span data-stu-id="2c878-347">![HelpPage Area](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image31.png "HelpPage Area")</span></span>

    <span data-ttu-id="2c878-348">*Obszar HelpPage*</span><span class="sxs-lookup"><span data-stu-id="2c878-348">*HelpPage Area*</span></span>
3. <span data-ttu-id="2c878-349">Domyślnie pomocy strony zawierają symbol zastępczy parametrów dla dokumentacji.</span><span class="sxs-lookup"><span data-stu-id="2c878-349">By default, the help pages have placeholder strings for documentation.</span></span> <span data-ttu-id="2c878-350">Komentarze dokumentacji XML umożliwia tworzenie w dokumentacji.</span><span class="sxs-lookup"><span data-stu-id="2c878-350">You can use XML documentation comments to create the documentation.</span></span> <span data-ttu-id="2c878-351">Aby włączyć tę funkcję, otwórz **HelpPageConfig.cs** plik znajdujący się w **obszarów/HelpPage/aplikacji\_Start** folder i usuń znaczniki komentarza następujący wiersz:</span><span class="sxs-lookup"><span data-stu-id="2c878-351">To enable this feature, open the **HelpPageConfig.cs** file located in the **Areas/HelpPage/App\_Start** folder and uncomment the following line:</span></span>

    [!code-javascript[Main](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/samples/sample8.js)]
4. <span data-ttu-id="2c878-352">W **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy projekt **MyHybridSite**, wybierz opcję **właściwości** i kliknij przycisk **kompilacji** kartę.</span><span class="sxs-lookup"><span data-stu-id="2c878-352">In **Solution Explorer**, right-click the project **MyHybridSite**, select **Properties** and click the **Build** tab.</span></span>

    <span data-ttu-id="2c878-353">![Tworzenie karty](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image32.png "Tworzenie sekcji")</span><span class="sxs-lookup"><span data-stu-id="2c878-353">![Build tab](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image32.png "Build section")</span></span>

    <span data-ttu-id="2c878-354">*Tworzenie karty*</span><span class="sxs-lookup"><span data-stu-id="2c878-354">*Build tab*</span></span>
5. <span data-ttu-id="2c878-355">W obszarze **dane wyjściowe**, wybierz opcję **pliku dokumentacji XML**.</span><span class="sxs-lookup"><span data-stu-id="2c878-355">Under **Output**, select **XML documentation file**.</span></span> <span data-ttu-id="2c878-356">Wpisz w polu edycji **aplikacji\_Data/XmlDocument.xml**.</span><span class="sxs-lookup"><span data-stu-id="2c878-356">In the edit box, type **App\_Data/XmlDocument.xml**.</span></span>

    <span data-ttu-id="2c878-357">![Dane wyjściowe sekcji na karcie kompilacja](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image33.png "danych wyjściowych sekcji na karcie kompilacja")</span><span class="sxs-lookup"><span data-stu-id="2c878-357">![Output section in Build tab](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image33.png "Output section in build tab")</span></span>

    <span data-ttu-id="2c878-358">*Sekcja danych wyjściowych na karcie kompilacja*</span><span class="sxs-lookup"><span data-stu-id="2c878-358">*Output section in Build tab*</span></span>
6. <span data-ttu-id="2c878-359">Naciśnij klawisz **CTRL** + **S** Aby zapisać zmiany.</span><span class="sxs-lookup"><span data-stu-id="2c878-359">Press **CTRL** + **S** to save the changes.</span></span>
7. <span data-ttu-id="2c878-360">Otwórz **ApiPersonController.cs** plik wchodzącej w skład **kontrolerów** folderu.</span><span class="sxs-lookup"><span data-stu-id="2c878-360">Open the **ApiPersonController.cs** file from the **Controllers** folder.</span></span>
8. <span data-ttu-id="2c878-361">Wprowadź nowy wiersz między *GetPeople* podpis metody i */ / GET interfejsu api/ApiPerson* komentarz, a następnie wpisz trzy ukośników.</span><span class="sxs-lookup"><span data-stu-id="2c878-361">Enter a new line between the *GetPeople* method signature and the *// GET api/ApiPerson* comment, and then type three forward slashes.</span></span>

    > [!NOTE]
    > <span data-ttu-id="2c878-362">Program Visual Studio automatycznie wstawia elementy XML, które definiują w dokumentacji metody.</span><span class="sxs-lookup"><span data-stu-id="2c878-362">Visual Studio automatically inserts the XML elements which define the method documentation.</span></span>
9. <span data-ttu-id="2c878-363">Dodawanie tekstu podsumowania i wartość zwracana dla *GetPeople* metody.</span><span class="sxs-lookup"><span data-stu-id="2c878-363">Add a summary text and the return value for the *GetPeople* method.</span></span> <span data-ttu-id="2c878-364">Powinno to wyglądać następująco.</span><span class="sxs-lookup"><span data-stu-id="2c878-364">It should look like the following.</span></span>

    [!code-csharp[Main](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/samples/sample9.cs)]
10. <span data-ttu-id="2c878-365">Naciśnij klawisz **F5** Aby uruchomić rozwiązanie.</span><span class="sxs-lookup"><span data-stu-id="2c878-365">Press **F5** to run the solution.</span></span>
11. <span data-ttu-id="2c878-366">Dołącz **/help** do adresu URL w pasku adresu, aby przejść do strony pomocy.</span><span class="sxs-lookup"><span data-stu-id="2c878-366">Append **/help** to the URL in the address bar to browse to the help page.</span></span>

    <span data-ttu-id="2c878-367">![Strona pomocy interfejsu API sieci Web platformy ASP.NET](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image34.png "strona pomocy interfejsu API sieci Web platformy ASP.NET")</span><span class="sxs-lookup"><span data-stu-id="2c878-367">![ASP.NET Web API Help Page](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image34.png "ASP.NET Web API Help Page")</span></span>

    <span data-ttu-id="2c878-368">*Strona pomocy interfejsu API sieci Web platformy ASP.NET*</span><span class="sxs-lookup"><span data-stu-id="2c878-368">*ASP.NET Web API Help Page*</span></span>

    > [!NOTE]
    > <span data-ttu-id="2c878-369">Główną zawartością strony znajduje się tabela interfejsów API, pogrupowane według kontrolera.</span><span class="sxs-lookup"><span data-stu-id="2c878-369">The main content of the page is a table of APIs, grouped by controller.</span></span> <span data-ttu-id="2c878-370">Wpisy tabeli są generowane dynamicznie przy użyciu **IApiExplorer** interfejsu.</span><span class="sxs-lookup"><span data-stu-id="2c878-370">The table entries are generated dynamically, using the **IApiExplorer** interface.</span></span> <span data-ttu-id="2c878-371">Jeśli dodasz lub zaktualizować Kontroler interfejsu API tabeli zostaną automatycznie zaktualizowane przy następnej kompilacji aplikacji.</span><span class="sxs-lookup"><span data-stu-id="2c878-371">If you add or update an API controller, the table will be automatically updated the next time you build the application.</span></span>
    > 
    > <span data-ttu-id="2c878-372">**API** kolumna zawiera metodę HTTP oraz względnego identyfikatora URI.</span><span class="sxs-lookup"><span data-stu-id="2c878-372">The **API** column lists the HTTP method and relative URI.</span></span> <span data-ttu-id="2c878-373">**Opis** kolumna zawierała informacje, który został wyodrębniony z dokumentacji metody.</span><span class="sxs-lookup"><span data-stu-id="2c878-373">The **Description** column contains information that has been extracted from the method's documentation.</span></span>
12. <span data-ttu-id="2c878-374">Należy zauważyć, że opis, który dodaje powyżej definicję metody jest wyświetlana w kolumnie Opis.</span><span class="sxs-lookup"><span data-stu-id="2c878-374">Note that the description you added above the method definition is displayed in the description column.</span></span>

    <span data-ttu-id="2c878-375">![Opis metody interfejsu API](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image35.png "opis metody interfejsu API")</span><span class="sxs-lookup"><span data-stu-id="2c878-375">![API method description](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image35.png "API method description")</span></span>

    <span data-ttu-id="2c878-376">*Opis metody interfejsu API*</span><span class="sxs-lookup"><span data-stu-id="2c878-376">*API method description*</span></span>
13. <span data-ttu-id="2c878-377">Kliknij jeden z metody interfejsu API, aby przejść do strony z bardziej szczegółowe informacje, w tym przykładowe ciała odpowiedzi.</span><span class="sxs-lookup"><span data-stu-id="2c878-377">Click one of the API methods to navigate to a page with more detailed information, including sample response bodies.</span></span>

    <span data-ttu-id="2c878-378">![Strona informacji o szczegółów](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image36.png "szczegółowe informacje o stronie")</span><span class="sxs-lookup"><span data-stu-id="2c878-378">![Detail Information page](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image36.png "Detail Information page")</span></span>

    <span data-ttu-id="2c878-379">*Szczegółowe informacje na stronie*</span><span class="sxs-lookup"><span data-stu-id="2c878-379">*Detailed information page*</span></span>

* * *

<a id="Summary"></a>
## <a name="summary"></a><span data-ttu-id="2c878-380">Podsumowanie</span><span class="sxs-lookup"><span data-stu-id="2c878-380">Summary</span></span>

<span data-ttu-id="2c878-381">Przez ukończenie tego laboratorium praktycznego wiesz już, jak:</span><span class="sxs-lookup"><span data-stu-id="2c878-381">By completing this hands-on lab you have learned how to:</span></span>

- <span data-ttu-id="2c878-382">Utwórz nową aplikację sieci Web za pomocą jednego środowiska ASP.NET w programie Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="2c878-382">Create a new Web application using the One ASP.NET Experience in Visual Studio 2013</span></span>
- <span data-ttu-id="2c878-383">Integrowanie wiele technologii ASP.NET z jednego pojedynczego projektu</span><span class="sxs-lookup"><span data-stu-id="2c878-383">Integrate multiple ASP.NET technologies into one single project</span></span>
- <span data-ttu-id="2c878-384">Generowanie widoków i kontrolerów MVC z klas modelu za pomocą programu ASP.NET tworzenie szkieletów</span><span class="sxs-lookup"><span data-stu-id="2c878-384">Generate MVC controllers and views from your model classes using ASP.NET Scaffolding</span></span>
- <span data-ttu-id="2c878-385">Generowanie kontrolerów internetowych interfejsów API, które używają funkcji, takich jak programowanie Async oraz dostęp do danych za pomocą platformy Entity Framework</span><span class="sxs-lookup"><span data-stu-id="2c878-385">Generate Web API controllers, which use features such as Async Programming and data access through Entity Framework</span></span>
- <span data-ttu-id="2c878-386">Automatyczne generowanie stron pomocy interfejsu API sieci Web dla kontrolerów</span><span class="sxs-lookup"><span data-stu-id="2c878-386">Automatically generate Web API Help Pages for your controllers</span></span>
