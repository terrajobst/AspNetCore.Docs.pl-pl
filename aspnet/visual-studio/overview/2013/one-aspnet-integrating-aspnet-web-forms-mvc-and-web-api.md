---
uid: visual-studio/overview/2013/one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api
title: 'Wskazówki laboratorium: Jeden ASP.NET: integrowanie formularzy sieci Web ASP.NET, MVC i interfejs API sieci Web | Dokumentacja firmy Microsoft'
author: rick-anderson
description: ASP.NET to struktura służąca do tworzenia witryn sieci Web, aplikacji i usług przy użyciu technologii specjalnych, takich jak MVC, Web API i inne. Z rozszerzeniem ASP.NET h...
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/16/2014
ms.topic: article
ms.assetid: 4fe2558d-67cc-4d12-a5c1-6fb9f6f16137
ms.technology: ''
ms.prod: .net-framework
msc.legacyurl: /visual-studio/overview/2013/one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api
msc.type: authoredcontent
ms.openlocfilehash: 55109723e566a9f7c66c1a59414377b05dbec760
ms.sourcegitcommit: 6784510cfb589308c3875ccb5113eb31031766b4
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/08/2018
ms.locfileid: "26566441"
---
<a name="hands-on-lab-one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api"></a><span data-ttu-id="bded3-104">Wskazówki laboratorium: Jeden ASP.NET: integrowanie formularzy sieci Web ASP.NET, MVC i interfejs API sieci Web</span><span class="sxs-lookup"><span data-stu-id="bded3-104">Hands On Lab: One ASP.NET: Integrating ASP.NET Web Forms, MVC and Web API</span></span>
====================
<span data-ttu-id="bded3-105">Przez [obozów sieci Web Team](https://twitter.com/webcamps)</span><span class="sxs-lookup"><span data-stu-id="bded3-105">by [Web Camps Team](https://twitter.com/webcamps)</span></span>

[<span data-ttu-id="bded3-106">Pobierz obozów sieci Web uczenie Kit</span><span class="sxs-lookup"><span data-stu-id="bded3-106">Download Web Camps Training Kit</span></span>](http://aka.ms/webcamps-training-kit)

> <span data-ttu-id="bded3-107">ASP.NET to struktura służąca do tworzenia witryn sieci Web, aplikacji i usług przy użyciu technologii specjalnych, takich jak MVC, Web API i inne.</span><span class="sxs-lookup"><span data-stu-id="bded3-107">ASP.NET is a framework for building Web sites, apps and services using specialized technologies such as MVC, Web API and others.</span></span> <span data-ttu-id="bded3-108">Z rozszerzeniem ASP.NET ma odebrane od czasu jego utworzenia i zaakceptowania muszą mieć te technologie zintegrowane, zaszły ostatnie wysiłków w prace nad **ASP.NET jeden**.</span><span class="sxs-lookup"><span data-stu-id="bded3-108">With the expansion ASP.NET has seen since its creation and the expressed need to have these technologies integrated, there have been recent efforts in working towards **One ASP.NET**.</span></span>
> 
> <span data-ttu-id="bded3-109">Visual Studio 2013 wprowadzenie nowego systemu ujednoliconego projektu, który umożliwia tworzenie aplikacji i korzystać z technologii ASP.NET w jednym projekcie.</span><span class="sxs-lookup"><span data-stu-id="bded3-109">Visual Studio 2013 introduces a new unified project system which lets you build an application and use all the ASP.NET technologies in one project.</span></span> <span data-ttu-id="bded3-110">Ta funkcja eliminuje konieczność pobrania jednej technologii na początku projektu i dysku z nim, a zamiast tego zaleca się korzystanie z wielu platform ASP.NET w jednym projekcie.</span><span class="sxs-lookup"><span data-stu-id="bded3-110">This feature eliminates the need to pick one technology at the start of a project and stick with it, and instead encourages the use of multiple ASP.NET frameworks within one project.</span></span>
> 
> <span data-ttu-id="bded3-111">Wszystkie przykładowy kod i fragmenty kodu są uwzględnione w sieci Web obozów zestaw szkoleniowy, dostępne pod adresem [ http://aka.ms/webcamps-training-kit ](http://aka.ms/webcamps-training-kit).</span><span class="sxs-lookup"><span data-stu-id="bded3-111">All sample code and snippets are included in the Web Camps Training Kit, available at [http://aka.ms/webcamps-training-kit](http://aka.ms/webcamps-training-kit).</span></span>


<a id="Overview"></a>
## <a name="overview"></a><span data-ttu-id="bded3-112">Omówienie</span><span class="sxs-lookup"><span data-stu-id="bded3-112">Overview</span></span>

<a id="Objectives"></a>
### <a name="objectives"></a><span data-ttu-id="bded3-113">Cele</span><span class="sxs-lookup"><span data-stu-id="bded3-113">Objectives</span></span>

<span data-ttu-id="bded3-114">W tym laboratorium praktycznego przedstawiono sposób:</span><span class="sxs-lookup"><span data-stu-id="bded3-114">In this hands-on lab, you will learn how to:</span></span>

- <span data-ttu-id="bded3-115">Tworzenie witryny sieci Web na podstawie **ASP.NET jeden** typ projektu</span><span class="sxs-lookup"><span data-stu-id="bded3-115">Create a Web site based on the **One ASP.NET** project type</span></span>
- <span data-ttu-id="bded3-116">Użyj innego **ASP.NET** platform, takich jak **MVC** i **interfejsu API sieci Web** w tym samym projekcie</span><span class="sxs-lookup"><span data-stu-id="bded3-116">Use different **ASP.NET** frameworks like **MVC** and **Web API** in the same project</span></span>
- <span data-ttu-id="bded3-117">Identyfikacja głównych składników **ASP.NET** aplikacji</span><span class="sxs-lookup"><span data-stu-id="bded3-117">Identify the main components of an **ASP.NET** application</span></span>
- <span data-ttu-id="bded3-118">Korzystać z **szkieletów ASP.NET** framework do automatycznego tworzenia widoków i kontrolerów w celu wykonywania operacji CRUD oparte na klasach modeli</span><span class="sxs-lookup"><span data-stu-id="bded3-118">Take advantage of the **ASP.NET Scaffolding** framework to automatically create Controllers and Views to perform CRUD operations based on your model classes</span></span>
- <span data-ttu-id="bded3-119">Udostępnianie ten sam zestaw informacji w formatach machine - i zrozumiałą dla użytkownika za pomocą właściwych narzędzi do każdego zadania</span><span class="sxs-lookup"><span data-stu-id="bded3-119">Expose the same set of information in machine- and human-readable formats using the right tool for each job</span></span>

<a id="Prerequisites"></a>
### <a name="prerequisites"></a><span data-ttu-id="bded3-120">Wymagania wstępne</span><span class="sxs-lookup"><span data-stu-id="bded3-120">Prerequisites</span></span>

<span data-ttu-id="bded3-121">Poniżej jest wymagany do ukończenia tego laboratorium praktycznego:</span><span class="sxs-lookup"><span data-stu-id="bded3-121">The following is required to complete this hands-on lab:</span></span>

- <span data-ttu-id="bded3-122">[Visual Studio Express 2013 for Web](https://www.microsoft.com/visualstudio/) lub nowszej</span><span class="sxs-lookup"><span data-stu-id="bded3-122">[Visual Studio Express 2013 for Web](https://www.microsoft.com/visualstudio/) or greater</span></span>
- [<span data-ttu-id="bded3-123">Visual Studio 2013 Update 1</span><span class="sxs-lookup"><span data-stu-id="bded3-123">Visual Studio 2013 Update 1</span></span>](https://go.microsoft.com/fwlink/?LinkId=301714)

<a id="Setup"></a>
### <a name="setup"></a><span data-ttu-id="bded3-124">Konfiguracja</span><span class="sxs-lookup"><span data-stu-id="bded3-124">Setup</span></span>

<span data-ttu-id="bded3-125">Aby można było uruchomić ćwiczeń opisanych w tym laboratorium praktycznego, należy najpierw skonfigurować środowiska.</span><span class="sxs-lookup"><span data-stu-id="bded3-125">In order to run the exercises in this hands-on lab, you will need to set up your environment first.</span></span>

1. <span data-ttu-id="bded3-126">Otwórz Eksploratora Windows i przejdź do laboratorium **źródła** folderu.</span><span class="sxs-lookup"><span data-stu-id="bded3-126">Open Windows Explorer and browse to the lab's **Source** folder.</span></span>
2. <span data-ttu-id="bded3-127">Kliknij prawym przyciskiem myszy **Setup.cmd** i wybierz **Uruchom jako administrator** do uruchamiania procesu instalacji, który skonfigurujesz środowisko i zainstalować wstawki kodu programu Visual Studio dla tego laboratorium.</span><span class="sxs-lookup"><span data-stu-id="bded3-127">Right-click on **Setup.cmd** and select **Run as administrator** to launch the setup process that will configure your environment and install the Visual Studio code snippets for this lab.</span></span>
3. <span data-ttu-id="bded3-128">Jeśli wyświetlane jest okno dialogowe kontroli konta użytkownika, potwierdź tę akcję, aby kontynuować.</span><span class="sxs-lookup"><span data-stu-id="bded3-128">If the User Account Control dialog box is shown, confirm the action to proceed.</span></span>

> [!NOTE]
> <span data-ttu-id="bded3-129">Upewnij się, że zaznaczono wszystkie zależności dla tego laboratorium przed uruchomieniem Instalatora.</span><span class="sxs-lookup"><span data-stu-id="bded3-129">Make sure you have checked all the dependencies for this lab before running the setup.</span></span>


<a id="CodeSnippets"></a>
### <a name="using-the-code-snippets"></a><span data-ttu-id="bded3-130">Korzystania z wstawek kodu</span><span class="sxs-lookup"><span data-stu-id="bded3-130">Using the Code Snippets</span></span>

<span data-ttu-id="bded3-131">W dokumencie laboratorium należy poinstruować do wstawienia bloków kodu.</span><span class="sxs-lookup"><span data-stu-id="bded3-131">Throughout the lab document, you will be instructed to insert code blocks.</span></span> <span data-ttu-id="bded3-132">Dla wygody większość tego kodu jest dostępna w Visual Studio wstawki kodu, które są dostępne w Visual Studio 2013, aby uniknąć konieczności Dodaj ją ręcznie.</span><span class="sxs-lookup"><span data-stu-id="bded3-132">For your convenience, most of this code is provided as Visual Studio Code Snippets, which you can access from within Visual Studio 2013 to avoid having to add it manually.</span></span>

> [!NOTE]
> <span data-ttu-id="bded3-133">Każdy wykonywania towarzyszy początkowy rozwiązania, znajdujących się w **rozpocząć** folderu ćwiczeniu, która umożliwia wykonanie każdej wykonywania niezależnie od innych.</span><span class="sxs-lookup"><span data-stu-id="bded3-133">Each exercise is accompanied by a starting solution located in the **Begin** folder of the exercise that allows you to follow each exercise independently of the others.</span></span> <span data-ttu-id="bded3-134">Należy pamiętać, że fragmenty kodu, które są dodawane podczas wykonywania brakuje te uruchamianie rozwiązań i może nie działać do czasu wykonywania.</span><span class="sxs-lookup"><span data-stu-id="bded3-134">Please be aware that the code snippets that are added during an exercise are missing from these starting solutions and may not work until you have completed the exercise.</span></span> <span data-ttu-id="bded3-135">W kodzie źródłowym wykonywania można również znaleźć **zakończenia** folderu zawierającego rozwiązanie Visual Studio z kodem, który powoduje wykonanie czynności opisane w odpowiedniej wykonywania.</span><span class="sxs-lookup"><span data-stu-id="bded3-135">Inside the source code for an exercise, you will also find an **End** folder containing a Visual Studio solution with the code that results from completing the steps in the corresponding exercise.</span></span> <span data-ttu-id="bded3-136">Jeśli potrzebujesz dodatkowej pomocy w pracy za pośrednictwem tego laboratorium praktycznego, można użyć tych rozwiązań jako wskazówki.</span><span class="sxs-lookup"><span data-stu-id="bded3-136">You can use these solutions as guidance if you need additional help as you work through this hands-on lab.</span></span>


* * *

<a id="Exercises"></a>
## <a name="exercises"></a><span data-ttu-id="bded3-137">Ćwiczenia</span><span class="sxs-lookup"><span data-stu-id="bded3-137">Exercises</span></span>

<span data-ttu-id="bded3-138">To laboratorium praktycznego obejmuje następujących czynnościach:</span><span class="sxs-lookup"><span data-stu-id="bded3-138">This hands-on lab includes the following exercises:</span></span>

1. [<span data-ttu-id="bded3-139">Tworzenie nowego projektu formularzy sieci Web</span><span class="sxs-lookup"><span data-stu-id="bded3-139">Creating a New Web Forms Project</span></span>](#Exercise1)
2. [<span data-ttu-id="bded3-140">Tworzenie przy użyciu funkcji szkieletów kontrolera MVC</span><span class="sxs-lookup"><span data-stu-id="bded3-140">Creating an MVC Controller Using Scaffolding</span></span>](#Exercise2)
3. [<span data-ttu-id="bded3-141">Tworzenie kontrolera interfejsu API sieci Web przy użyciu funkcji szkieletów</span><span class="sxs-lookup"><span data-stu-id="bded3-141">Creating a Web API Controller Using Scaffolding</span></span>](#Exercise3)

<span data-ttu-id="bded3-142">Szacowany czas trwania tego laboratorium: **60 minut**</span><span class="sxs-lookup"><span data-stu-id="bded3-142">Estimated time to complete this lab: **60 minutes**</span></span>

> [!NOTE]
> <span data-ttu-id="bded3-143">Przy pierwszym uruchomieniu programu Visual Studio, musisz wybrać jeden z wstępnie zdefiniowanych ustawień kolekcji.</span><span class="sxs-lookup"><span data-stu-id="bded3-143">When you first start Visual Studio, you must select one of the predefined settings collections.</span></span> <span data-ttu-id="bded3-144">Każda kolekcja wstępnie zdefiniowanych zaprojektowano w celu stylem rozwoju i określa układów okien, zachowanie edytora wstawki kodu IntelliSense i opcje w oknach dialogowych.</span><span class="sxs-lookup"><span data-stu-id="bded3-144">Each predefined collection is designed to match a particular development style and determines window layouts, editor behavior, IntelliSense code snippets, and dialog box options.</span></span> <span data-ttu-id="bded3-145">Procedury przedstawione w tym laboratorium opisano czynności niezbędnych do wykonywania danego zadania w programie Visual Studio, korzystając z **ogólne ustawienia środowiska deweloperskiego** kolekcji.</span><span class="sxs-lookup"><span data-stu-id="bded3-145">The procedures in this lab describe the actions necessary to accomplish a given task in Visual Studio when using the **General Development Settings** collection.</span></span> <span data-ttu-id="bded3-146">Jeśli wybierzesz kolekcji różne ustawienia dla swojego środowiska programowania, może być różnice w krokach, które należy wziąć pod uwagę.</span><span class="sxs-lookup"><span data-stu-id="bded3-146">If you choose a different settings collection for your development environment, there may be differences in the steps that you should take into account.</span></span>


<a id="Exercise1"></a>
### <a name="exercise-1-creating-a-new-web-forms-project"></a><span data-ttu-id="bded3-147">Ćwiczenie 1: Tworzenie nowego projektu formularzy sieci Web</span><span class="sxs-lookup"><span data-stu-id="bded3-147">Exercise 1: Creating a New Web Forms Project</span></span>

<span data-ttu-id="bded3-148">W tym ćwiczeniu utworzysz nową lokację formularzy sieci Web w programie Visual Studio 2013 za pomocą **ASP.NET jeden** unified doświadczenie w projekcie, co pozwoli łatwo zintegrować składników Web Forms, MVC i interfejsu API sieci Web w tej samej aplikacji.</span><span class="sxs-lookup"><span data-stu-id="bded3-148">In this exercise you will create a new Web Forms site in Visual Studio 2013 using the **One ASP.NET** unified project experience, which will allow you to easily integrate Web Forms, MVC and Web API components in the same application.</span></span> <span data-ttu-id="bded3-149">Będą Eksploruj wygenerowanego rozwiązania i zidentyfikować jego części, a na koniec zostanie zobacz witrynę sieci Web w akcji.</span><span class="sxs-lookup"><span data-stu-id="bded3-149">You will then explore the generated solution and identify its parts, and finally you will see the Web site in action.</span></span>

<a id="Ex1Task1"></a>
#### <a name="task-1--creating-a-new-site-using-the-one-aspnet-experience"></a><span data-ttu-id="bded3-150">Zadanie 1 — Tworzenie nowej lokacji za pomocą jednego środowiska ASP.NET</span><span class="sxs-lookup"><span data-stu-id="bded3-150">Task 1 – Creating a New Site Using the One ASP.NET Experience</span></span>

<span data-ttu-id="bded3-151">To zadanie będzie rozpocząć tworzenia nowej witryny sieci Web w programie Visual Studio na podstawie **ASP.NET jeden** typu projektu.</span><span class="sxs-lookup"><span data-stu-id="bded3-151">In this task you will start creating a new Web site in Visual Studio based on the **One ASP.NET** project type.</span></span> <span data-ttu-id="bded3-152">**Jeden ASP.NET** łączy wszystkie technologii ASP.NET i daje możliwość mieszać i dopasowywać je zgodnie z potrzebami.</span><span class="sxs-lookup"><span data-stu-id="bded3-152">**One ASP.NET** unifies all ASP.NET technologies and gives you the option to mix and match them as desired.</span></span> <span data-ttu-id="bded3-153">Następnie rozpoznania różnych składników Web Forms, MVC i interfejsu API sieci Web, które na żywo jednocześnie w aplikacji.</span><span class="sxs-lookup"><span data-stu-id="bded3-153">You will then recognize the different components of Web Forms, MVC and Web API that live side by side within your application.</span></span>

1. <span data-ttu-id="bded3-154">Otwórz **programu Visual Studio Express 2013 for Web** i wybierz **pliku | Nowy projekt...**  można uruchomić nowego rozwiązania.</span><span class="sxs-lookup"><span data-stu-id="bded3-154">Open **Visual Studio Express 2013 for Web** and select **File | New Project...** to start a new solution.</span></span>

    ![Tworzenie nowego projektu](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image1.png)

    <span data-ttu-id="bded3-156">*Tworzenie nowego projektu*</span><span class="sxs-lookup"><span data-stu-id="bded3-156">*Creating a New Project*</span></span>
2. <span data-ttu-id="bded3-157">W **nowy projekt** okno dialogowe, wybierz opcję **aplikacji sieci Web ASP.NET** w obszarze **Visual C# | Web** i upewnij się, że **.NET Framework 4.5** jest zaznaczone.</span><span class="sxs-lookup"><span data-stu-id="bded3-157">In the **New Project** dialog box, select **ASP.NET Web Application** under the **Visual C# | Web** tab, and make sure **.NET Framework 4.5** is selected.</span></span> <span data-ttu-id="bded3-158">Nazwij projekt *MyHybridSite*, wybierz **lokalizacji** i kliknij przycisk **OK**.</span><span class="sxs-lookup"><span data-stu-id="bded3-158">Name the project *MyHybridSite*, choose a **Location** and click **OK**.</span></span>

    ![Nowy projekt aplikacji sieci Web ASP.NET](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image2.png)

    <span data-ttu-id="bded3-160">*Tworzenie nowego projektu aplikacji sieci Web ASP.NET*</span><span class="sxs-lookup"><span data-stu-id="bded3-160">*Creating a new ASP.NET Web Application project*</span></span>
3. <span data-ttu-id="bded3-161">W **nowy projekt ASP.NET** okno dialogowe, wybierz opcję **formularzy sieci Web** szablonu, a następnie wybierz **MVC** i **interfejsu API sieci Web** opcje.</span><span class="sxs-lookup"><span data-stu-id="bded3-161">In the **New ASP.NET Project** dialog box, select the **Web Forms** template and select the **MVC** and **Web API** options.</span></span> <span data-ttu-id="bded3-162">Ponadto upewnij się, że **uwierzytelniania** ustawiono opcję **indywidualnych kont użytkowników**.</span><span class="sxs-lookup"><span data-stu-id="bded3-162">Also, make sure that the **Authentication** option is set to **Individual User Accounts**.</span></span> <span data-ttu-id="bded3-163">Kliknij przycisk **OK** aby kontynuować.</span><span class="sxs-lookup"><span data-stu-id="bded3-163">Click **OK** to continue.</span></span>

    ![Tworzenie nowego projektu z szablonu formularzy sieci Web, w tym składniki interfejsu API sieci Web i MVC](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image3.png)

    <span data-ttu-id="bded3-165">*Tworzenie nowego projektu z szablonu formularzy sieci Web, w tym składniki interfejsu API sieci Web i MVC*</span><span class="sxs-lookup"><span data-stu-id="bded3-165">*Creating a new project with the Web Forms template, including Web API and MVC components*</span></span>
4. <span data-ttu-id="bded3-166">Teraz można sprawdzić strukturę wygenerowanego rozwiązania.</span><span class="sxs-lookup"><span data-stu-id="bded3-166">You can now explore the structure of the generated solution.</span></span>

    ![Eksploracja wygenerowanego rozwiązania](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image4.png)

    <span data-ttu-id="bded3-168">*Eksploracja wygenerowanego rozwiązania*</span><span class="sxs-lookup"><span data-stu-id="bded3-168">*Exploring the generated solution*</span></span>

    1. <span data-ttu-id="bded3-169">**Konto:** ten folder zawiera na stronach formularzy sieci Web, aby zarejestrować, zaloguj się i zarządzanie kontami użytkowników aplikacji.</span><span class="sxs-lookup"><span data-stu-id="bded3-169">**Account:** This folder contains the Web Form pages to register, log in to and manage the application's user accounts.</span></span> <span data-ttu-id="bded3-170">Ten folder jest dodana gdy **indywidualnych kont użytkowników** wybrano opcję uwierzytelniania podczas konfigurowania szablonu projektu formularzy sieci Web.</span><span class="sxs-lookup"><span data-stu-id="bded3-170">This folder is added when the **Individual User Accounts** authentication option is selected during the configuration of the Web Forms project template.</span></span>
    2. <span data-ttu-id="bded3-171">**Modele:** ten folder będzie zawierać klasy, które przedstawiają dane aplikacji.</span><span class="sxs-lookup"><span data-stu-id="bded3-171">**Models:** This folder will contain the classes that represent your application data.</span></span>
    3. <span data-ttu-id="bded3-172">**Kontrolery** i **widoków**: te foldery są wymagane dla **ASP.NET MVC** i **ASP.NET Web API** składników.</span><span class="sxs-lookup"><span data-stu-id="bded3-172">**Controllers** and **Views**: These folders are required for the **ASP.NET MVC** and **ASP.NET Web API** components.</span></span> <span data-ttu-id="bded3-173">Użytkownik może zapoznać technologii MVC i interfejsu API sieci Web w następnym ćwiczeniach.</span><span class="sxs-lookup"><span data-stu-id="bded3-173">You will explore the MVC and Web API technologies in the next exercises.</span></span>
    4. <span data-ttu-id="bded3-174">**Default.aspx**, **Contact.aspx** i **About.aspx** pliki są wstępnie zdefiniowane stron formularzy sieci Web, które służy jako punkt wyjścia do tworzenia strony specyficzne dla użytkownika aplikacja.</span><span class="sxs-lookup"><span data-stu-id="bded3-174">The **Default.aspx**, **Contact.aspx** and **About.aspx** files are pre-defined Web Form pages that you can use as starting points to build the pages specific to your application.</span></span> <span data-ttu-id="bded3-175">Programowania logiki tych plików znajduje się w oddzielnym pliku nazywane &quot;CodeBehind&quot; pliku, który ma &quot;. aspx.vb&quot; lub &quot;. aspx.cs&quot; rozszerzenia (w zależności od język używany).</span><span class="sxs-lookup"><span data-stu-id="bded3-175">The programming logic of those files resides in a separate file referred to as the &quot;code-behind&quot; file, which has an &quot;.aspx.vb&quot; or &quot;.aspx.cs&quot; extension (depending on the language used).</span></span> <span data-ttu-id="bded3-176">Logika CodeBehind działa na serwerze i dynamicznie tworzy danych wyjściowych HTML strony.</span><span class="sxs-lookup"><span data-stu-id="bded3-176">The code-behind logic runs on the server and dynamically produces the HTML output for your page.</span></span>
    5. <span data-ttu-id="bded3-177">**Site.Master** i **Site.Mobile.Master** stron zdefiniuj wyglądu i działania i standardowe zachowanie wszystkich stron w aplikacji.</span><span class="sxs-lookup"><span data-stu-id="bded3-177">The **Site.Master** and **Site.Mobile.Master** pages define the look and feel and the standard behavior of all the pages in the application.</span></span>
5. <span data-ttu-id="bded3-178">Kliknij dwukrotnie **Default.aspx** plik, aby zapoznać się z zawartości strony.</span><span class="sxs-lookup"><span data-stu-id="bded3-178">Double-click the **Default.aspx** file to explore the content of the page.</span></span>

    ![Na stronie Default.aspx eksploracji](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image5.png)

    <span data-ttu-id="bded3-180">*Na stronie Default.aspx eksploracji*</span><span class="sxs-lookup"><span data-stu-id="bded3-180">*Exploring the Default.aspx page*</span></span>

    > [!NOTE]
    > <span data-ttu-id="bded3-181">**Strony** dyrektywy w górnej części pliku definiuje atrybuty strony formularzy sieci Web.</span><span class="sxs-lookup"><span data-stu-id="bded3-181">The **Page** directive at the top of the file defines the attributes of the Web Forms page.</span></span> <span data-ttu-id="bded3-182">Na przykład **MasterPageFile** atrybut określa ścieżkę do wzorca strony — w takim przypadku *Site.Master* stronie — i **Inherits** definiuje atrybut klasy związane z kodem strony dziedziczenia.</span><span class="sxs-lookup"><span data-stu-id="bded3-182">For example, the **MasterPageFile** attribute specifies the path to the master page -in this case, the *Site.Master* page- and the **Inherits** attribute defines the code-behind class for the page to inherit.</span></span> <span data-ttu-id="bded3-183">Ta klasa znajduje się w pliku ustaleniami **plik CodeBehind** atrybutu.</span><span class="sxs-lookup"><span data-stu-id="bded3-183">This class is located in the file determined by the **CodeBehind** attribute.</span></span>
    > 
    > <span data-ttu-id="bded3-184">**Asp: Content** formant zawiera rzeczywistej zawartości strony (tekst, znaczniki i kontrolki) i jest mapowany na **asp: ContentPlaceHolder** kontrolki na stronie głównej.</span><span class="sxs-lookup"><span data-stu-id="bded3-184">The **asp:Content** control holds the actual content of the page (text, markup and controls) and is mapped to a **asp:ContentPlaceHolder** control on the master page.</span></span> <span data-ttu-id="bded3-185">W takim przypadku zawartość strony będzie renderowany wewnątrz *znacznika* kontroli *Site.Master* strony.</span><span class="sxs-lookup"><span data-stu-id="bded3-185">In this case, the page content will be rendered inside the *MainContent* control defined in the *Site.Master* page.</span></span>
6. <span data-ttu-id="bded3-186">Rozwiń węzeł **aplikacji\_Start** folderu i zwróć uwagę, **WebApiConfig.cs** pliku.</span><span class="sxs-lookup"><span data-stu-id="bded3-186">Expand the **App\_Start** folder and notice the **WebApiConfig.cs** file.</span></span> <span data-ttu-id="bded3-187">Visual Studio zawarte tego pliku w wygenerowanym rozwiązania, ponieważ dołączony interfejsu API sieci Web podczas konfigurowania projektu przy użyciu szablonu platformy ASP.NET w jednym.</span><span class="sxs-lookup"><span data-stu-id="bded3-187">Visual Studio included that file in the generated solution because you included Web API when configuring your project with the One ASP.NET template.</span></span>
7. <span data-ttu-id="bded3-188">Otwórz **WebApiConfig.cs** pliku.</span><span class="sxs-lookup"><span data-stu-id="bded3-188">Open the **WebApiConfig.cs** file.</span></span> <span data-ttu-id="bded3-189">W *WebApiConfig* klasy można znaleźć konfiguracji skojarzone z sieci Web interfejsu API, który mapuje HTTP kieruje do **kontrolerów interfejsu API sieci Web**.</span><span class="sxs-lookup"><span data-stu-id="bded3-189">In the *WebApiConfig* class you will find the configuration associated with Web API, which maps HTTP routes to **Web API controllers**.</span></span>

    [!code-csharp[Main](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/samples/sample1.cs)]
8. <span data-ttu-id="bded3-190">Otwórz **RouteConfig.cs** pliku.</span><span class="sxs-lookup"><span data-stu-id="bded3-190">Open the **RouteConfig.cs** file.</span></span> <span data-ttu-id="bded3-191">Wewnątrz *RegisterRoutes* można znaleźć konfiguracji skojarzone z MVC, który mapuje tras HTTP do metody **kontrolerów MVC**.</span><span class="sxs-lookup"><span data-stu-id="bded3-191">Inside the *RegisterRoutes* method you will find the configuration associated with MVC, which maps HTTP routes to **MVC controllers**.</span></span>

    [!code-csharp[Main](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/samples/sample2.cs)]

<a id="Ex1Task2"></a>
#### <a name="task-2--running-the-solution"></a><span data-ttu-id="bded3-192">Zadanie 2 — rozwiązanie uruchomiona</span><span class="sxs-lookup"><span data-stu-id="bded3-192">Task 2 – Running the Solution</span></span>

<span data-ttu-id="bded3-193">W tym zadaniu zostanie uruchamianie wygenerowanego rozwiązania, aplikacji i niektóre jej funkcje, takie jak ponowne zapisywanie adresów URL i wbudowane uwierzytelnianie.</span><span class="sxs-lookup"><span data-stu-id="bded3-193">In this task you will run the generated solution, explore the app and some of its features, like URL rewriting and built-in authentication.</span></span>

1. <span data-ttu-id="bded3-194">Aby uruchomić rozwiązanie, naciśnij klawisz **F5** lub kliknij przycisk **Start** przycisk znajdujący się na pasku narzędzi.</span><span class="sxs-lookup"><span data-stu-id="bded3-194">To run the solution, press **F5** or click the **Start** button located on the toolbar.</span></span> <span data-ttu-id="bded3-195">Strona główna aplikacji należy otworzyć w przeglądarce.</span><span class="sxs-lookup"><span data-stu-id="bded3-195">The application home page should open in the browser.</span></span>

    ![Uruchomiona rozwiązania](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image6.png)
2. <span data-ttu-id="bded3-197">Upewnij się, że stron formularzy sieci Web są wywoływane.</span><span class="sxs-lookup"><span data-stu-id="bded3-197">Verify that the Web Forms pages are being invoked.</span></span> <span data-ttu-id="bded3-198">Aby to zrobić, należy dołączyć **/contact.aspx** do adresu URL na pasku adresu i naciśnij klawisz **Enter**.</span><span class="sxs-lookup"><span data-stu-id="bded3-198">To do this, append **/contact.aspx** to the URL in the address bar and press **Enter**.</span></span>

    ![Przyjazne adresy URL](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image7.png)

    <span data-ttu-id="bded3-200">*Przyjazne adresy URL*</span><span class="sxs-lookup"><span data-stu-id="bded3-200">*Friendly URLs*</span></span>

    > [!NOTE]
    > <span data-ttu-id="bded3-201">Jak widać, zmiana adresu URL do **/skontaktuj się z**.</span><span class="sxs-lookup"><span data-stu-id="bded3-201">As you can see, the URL changes to **/contact**.</span></span> <span data-ttu-id="bded3-202">Począwszy od **programu ASP.NET 4**, adres URL funkcji routingu zostały dodane do formularzy sieci Web, więc można napisać adresów URL, takich jak *[ http://www.mysite.com/products/software ](http://www.mysite.com/products/software)* zamiast  *[http://www.mysite.com/products.aspx?category=software](http://www.mysite.com/products.aspx?category=software)*.</span><span class="sxs-lookup"><span data-stu-id="bded3-202">Starting from **ASP.NET 4**, URL routing capabilities were added to Web Forms, so you can write URLs like *[http://www.mysite.com/products/software](http://www.mysite.com/products/software)* instead of *[http://www.mysite.com/products.aspx?category=software](http://www.mysite.com/products.aspx?category=software)*.</span></span> <span data-ttu-id="bded3-203">Więcej informacji można znaleźć w [routingu adresów URL](../../../web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/url-routing.md).</span><span class="sxs-lookup"><span data-stu-id="bded3-203">For more information refer to [URL Routing](../../../web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/url-routing.md).</span></span>
3. <span data-ttu-id="bded3-204">Użytkownik może teraz zapoznać przepływ uwierzytelniania zintegrowane w aplikacji.</span><span class="sxs-lookup"><span data-stu-id="bded3-204">You will now explore the authentication flow integrated into the application.</span></span> <span data-ttu-id="bded3-205">Aby to zrobić, kliknij przycisk **zarejestrować** w prawym górnym rogu strony.</span><span class="sxs-lookup"><span data-stu-id="bded3-205">To do this, click **Register** in the upper-right corner of the page.</span></span>

    ![Rejestrowanie nowego użytkownika](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image8.png)

    <span data-ttu-id="bded3-207">*Rejestrowanie nowego użytkownika*</span><span class="sxs-lookup"><span data-stu-id="bded3-207">*Registering a new user*</span></span>
4. <span data-ttu-id="bded3-208">W **zarejestrować** wprowadź **nazwy użytkownika** i **hasło**, a następnie kliknij przycisk **zarejestrować**.</span><span class="sxs-lookup"><span data-stu-id="bded3-208">In the **Register** page, enter a **User name** and **Password**, and then click **Register**.</span></span>

    ![Zarejestruj strony](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image9.png)

    <span data-ttu-id="bded3-210">*Zarejestruj strony*</span><span class="sxs-lookup"><span data-stu-id="bded3-210">*Register page*</span></span>
5. <span data-ttu-id="bded3-211">Aplikacja rejestruje nowe konto, a użytkownik jest uwierzytelniony.</span><span class="sxs-lookup"><span data-stu-id="bded3-211">The application registers the new account, and the user is authenticated.</span></span>

    ![Użytkownik uwierzytelniony](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image10.png)

    <span data-ttu-id="bded3-213">*Użytkownik uwierzytelniony*</span><span class="sxs-lookup"><span data-stu-id="bded3-213">*User authenticated*</span></span>
6. <span data-ttu-id="bded3-214">Wróć do programu Visual Studio i naciśnij klawisz **SHIFT + F5** można zatrzymać debugowania.</span><span class="sxs-lookup"><span data-stu-id="bded3-214">Go back to Visual Studio and press **SHIFT + F5** to stop debugging.</span></span>

<a id="Exercise2"></a>
### <a name="exercise-2-creating-an-mvc-controller-using-scaffolding"></a><span data-ttu-id="bded3-215">Ćwiczenie 2: Tworzenie kontrolera MVC przy użyciu funkcji szkieletów</span><span class="sxs-lookup"><span data-stu-id="bded3-215">Exercise 2: Creating an MVC Controller Using Scaffolding</span></span>

<span data-ttu-id="bded3-216">W tym ćwiczeniu zostanie korzystanie z platformy ASP.NET szkieletów dostarczane przez program Visual Studio do utworzenia kontrolera ASP.NET MVC 5 z akcjami i widokami Razor do wykonywania operacji CRUD, bez konieczności pisania pojedynczy wiersz kodu.</span><span class="sxs-lookup"><span data-stu-id="bded3-216">In this exercise you will take advantage of the ASP.NET Scaffolding framework provided by Visual Studio to create an ASP.NET MVC 5 controller with actions and Razor views to perform CRUD operations, without writing a single line of code.</span></span> <span data-ttu-id="bded3-217">Proces tworzenia szkieletów użyje Entity Framework Code First można wygenerować kontekstu danych i schemat bazy danych w bazie danych SQL.</span><span class="sxs-lookup"><span data-stu-id="bded3-217">The scaffolding process will use Entity Framework Code First to generate the data context and the database schema in the SQL database.</span></span>

<span data-ttu-id="bded3-218">**O programu Entity Framework Code najpierw**</span><span class="sxs-lookup"><span data-stu-id="bded3-218">**About Entity Framework Code First**</span></span>

<span data-ttu-id="bded3-219">Entity Framework (EF) jest mapowania obiektów relacyjnych (ORM) umożliwiający tworzenie aplikacji dostęp do danych przez programowania za pomocą modelu koncepcyjnego aplikacji zamiast programowanie bezpośrednio przy użyciu schematu magazyn relacyjny.</span><span class="sxs-lookup"><span data-stu-id="bded3-219">Entity Framework (EF) is an object-relational mapper (ORM) that enables you to create data access applications by programming with a conceptual application model instead of programming directly using a relational storage schema.</span></span>

<span data-ttu-id="bded3-220">Entity Framework Code First modelowania przepływu pracy umożliwia przy użyciu własnych klas domeny do reprezentowania EF zależy od podczas wykonywania kwerendy, model funkcji śledzenia zmian i aktualizowanie.</span><span class="sxs-lookup"><span data-stu-id="bded3-220">The Entity Framework Code First modeling workflow allows you to use your own domain classes to represent the model that EF relies on when performing querying, change-tracking and updating functions.</span></span> <span data-ttu-id="bded3-221">Przy użyciu Code First przepływu pracy programowania, nie należy do rozpoczęcia tworzenia bazy danych lub określając schematem aplikacji.</span><span class="sxs-lookup"><span data-stu-id="bded3-221">Using the Code First development workflow, you do not need to begin your application by creating a database or specifying a schema.</span></span> <span data-ttu-id="bded3-222">Zamiast tego można napisać standardowe klasy .NET, które definiują najbardziej odpowiednie obiekty modelu domeny dla aplikacji i programu Entity Framework utworzy bazę danych można.</span><span class="sxs-lookup"><span data-stu-id="bded3-222">Instead, you can write standard .NET classes that define the most appropriate domain model objects for your application, and Entity Framework will create the database for you.</span></span>

> [!NOTE]
> <span data-ttu-id="bded3-223">Dowiedz się więcej na temat programu Entity Framework [tutaj](../../../entity-framework.md).</span><span class="sxs-lookup"><span data-stu-id="bded3-223">You can learn more about Entity Framework [here](../../../entity-framework.md).</span></span>


<a id="Ex2Task1"></a>
#### <a name="task-1--creating-a-new-model"></a><span data-ttu-id="bded3-224">Zadanie 1 — Tworzenie nowego modelu</span><span class="sxs-lookup"><span data-stu-id="bded3-224">Task 1 – Creating a New Model</span></span>

<span data-ttu-id="bded3-225">Teraz określi **osoby** klasy, który będzie używany przez proces tworzenia szkieletów do tworzenia kontrolera MVC i widoki modelu.</span><span class="sxs-lookup"><span data-stu-id="bded3-225">You will now define a **Person** class, which will be the model used by the scaffolding process to create the MVC controller and the views.</span></span> <span data-ttu-id="bded3-226">Rozpocznie się przez utworzenie **osoby** klasy modelu i operacje CRUD na kontrolerze zostanie automatycznie utworzony przy użyciu funkcji szkieletów.</span><span class="sxs-lookup"><span data-stu-id="bded3-226">You will start by creating a **Person** model class, and the CRUD operations in the controller will be automatically created using scaffolding features.</span></span>

1. <span data-ttu-id="bded3-227">Otwórz **programu Visual Studio Express 2013 for Web** i **MyHybridSite.sln** rozwiązania, znajdujących się w **źródło/Ex2-MvcScaffolding/Begin** folderu.</span><span class="sxs-lookup"><span data-stu-id="bded3-227">Open **Visual Studio Express 2013 for Web** and the **MyHybridSite.sln** solution located in the **Source/Ex2-MvcScaffolding/Begin** folder.</span></span> <span data-ttu-id="bded3-228">Alternatywnie możesz kontynuować z rozwiązaniem uzyskanymi w poprzednim ćwiczeniu.</span><span class="sxs-lookup"><span data-stu-id="bded3-228">Alternatively, you can continue with the solution that you obtained in the previous exercise.</span></span>
2. <span data-ttu-id="bded3-229">W **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy **modele** folderu **MyHybridSite** projekt i wybierz **Dodaj | Klasy...** .</span><span class="sxs-lookup"><span data-stu-id="bded3-229">In **Solution Explorer**, right-click the **Models** folder of the **MyHybridSite** project and select **Add | Class...**.</span></span>

    ![Dodawanie klasy modelu osoby](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image11.png)

    <span data-ttu-id="bded3-231">*Dodawanie klasy modelu osoby*</span><span class="sxs-lookup"><span data-stu-id="bded3-231">*Adding the Person model class*</span></span>
3. <span data-ttu-id="bded3-232">W **Dodaj nowy element** okno dialogowe, nazwa pliku *Person.cs* i kliknij przycisk **Dodaj**.</span><span class="sxs-lookup"><span data-stu-id="bded3-232">In the **Add New Item** dialog box, name the file *Person.cs* and click **Add**.</span></span>

    ![Tworzenie klasy modelu osoby](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image12.png)

    <span data-ttu-id="bded3-234">*Tworzenie klasy modelu osoby*</span><span class="sxs-lookup"><span data-stu-id="bded3-234">*Creating the Person model class*</span></span>
4. <span data-ttu-id="bded3-235">Zamień zawartość **Person.cs** pliku następującym kodem.</span><span class="sxs-lookup"><span data-stu-id="bded3-235">Replace the content of the **Person.cs** file with the following code.</span></span> <span data-ttu-id="bded3-236">Naciśnij klawisz **CTRL + S** Aby zapisać zmiany.</span><span class="sxs-lookup"><span data-stu-id="bded3-236">Press **CTRL + S** to save the changes.</span></span>

    <span data-ttu-id="bded3-237">(Fragment - kodu *PersonClass BringingTogetherOneAspNet - Ex2 -*)</span><span class="sxs-lookup"><span data-stu-id="bded3-237">(Code Snippet - *BringingTogetherOneAspNet - Ex2 - PersonClass*)</span></span>

    [!code-csharp[Main](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/samples/sample3.cs)]
5. <span data-ttu-id="bded3-238">W **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy **MyHybridSite** projekt i wybierz **kompilacji**, lub naciśnij klawisz **CTRL + SHIFT + B** Aby skompilować projekt.</span><span class="sxs-lookup"><span data-stu-id="bded3-238">In **Solution Explorer**, right-click the **MyHybridSite** project and select **Build**, or press **CTRL + SHIFT + B** to build the project.</span></span>

<a id="Ex2Task2"></a>
#### <a name="task-2--creating-an-mvc-controller"></a><span data-ttu-id="bded3-239">Zadanie 2 — Tworzenie kontrolera MVC</span><span class="sxs-lookup"><span data-stu-id="bded3-239">Task 2 – Creating an MVC Controller</span></span>

<span data-ttu-id="bded3-240">Teraz, gdy **osoby** modelu jest tworzony, użyje szkieletów ASP.NET MVC z programu Entity Framework do utworzenia CRUD akcji kontrolera oraz widoki dla **osoby**.</span><span class="sxs-lookup"><span data-stu-id="bded3-240">Now that the **Person** model is created, you will use ASP.NET MVC scaffolding with Entity Framework to create the CRUD controller actions and views for **Person**.</span></span>

1. <span data-ttu-id="bded3-241">W **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy **kontrolerów** folderu **MyHybridSite** projekt i wybierz **Dodaj | Nowy element szkieletu...** .</span><span class="sxs-lookup"><span data-stu-id="bded3-241">In **Solution Explorer**, right-click the **Controllers** folder of the **MyHybridSite** project and select **Add | New Scaffolded Item...**.</span></span>

    ![Tworzenie nowego kontrolera szkieletu](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image13.png)

    <span data-ttu-id="bded3-243">*Tworzenie nowego kontrolera szkieletu*</span><span class="sxs-lookup"><span data-stu-id="bded3-243">*Creating a new Scaffolded Controller*</span></span>
2. <span data-ttu-id="bded3-244">W **Dodawanie szkieletu** okno dialogowe, wybierz opcję **kontroler MVC 5 z widokami używający narzędzia Entity Framework** , a następnie kliknij przycisk **Dodaj.**</span><span class="sxs-lookup"><span data-stu-id="bded3-244">In the **Add Scaffold** dialog box, select **MVC 5 Controller with views, using Entity Framework** and then click **Add.**</span></span>

    ![Wybieranie kontroler MVC 5 z widokami i Entity Framework](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image14.png)

    <span data-ttu-id="bded3-246">*Wybieranie kontroler MVC 5 z widokami i Entity Framework*</span><span class="sxs-lookup"><span data-stu-id="bded3-246">*Selecting MVC 5 Controller with views and Entity Framework*</span></span>
3. <span data-ttu-id="bded3-247">Ustaw *MvcPersonController* jako **nazwy kontrolera**, wybierz pozycję **używać asynchronicznych akcji kontrolera** opcję i zaznacz **osoby (MyHybridSite.Models)**  jako **klasa modelu**.</span><span class="sxs-lookup"><span data-stu-id="bded3-247">Set *MvcPersonController* as the **Controller name**, select the **Use async controller actions** option and select **Person (MyHybridSite.Models)** as the **Model class**.</span></span>

    ![Dodawanie kontroler MVC z szkieletów](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image15.png)

    <span data-ttu-id="bded3-249">*Dodawanie kontroler MVC z szkieletów*</span><span class="sxs-lookup"><span data-stu-id="bded3-249">*Adding an MVC controller with scaffolding*</span></span>
4. <span data-ttu-id="bded3-250">W obszarze **klasa kontekstu danych**, kliknij przycisk **nowy kontekst danych...** .</span><span class="sxs-lookup"><span data-stu-id="bded3-250">Under **Data context class**, click **New data context...**.</span></span>

    ![Tworzenie nowego kontekstu danych](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image16.png)

    <span data-ttu-id="bded3-252">*Tworzenie nowego kontekstu danych*</span><span class="sxs-lookup"><span data-stu-id="bded3-252">*Creating a new data context*</span></span>
5. <span data-ttu-id="bded3-253">W **nowy kontekst danych** okno dialogowe, nazwa nowy kontekst danych *PersonContext* i kliknij przycisk **Dodaj**.</span><span class="sxs-lookup"><span data-stu-id="bded3-253">In the **New Data Context** dialog box, name the new data context *PersonContext* and click **Add**.</span></span>

    ![Tworzenie nowego PersonContext](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image17.png)

    <span data-ttu-id="bded3-255">*Tworzenie nowego typu PersonContext*</span><span class="sxs-lookup"><span data-stu-id="bded3-255">*Creating the new PersonContext type*</span></span>
6. <span data-ttu-id="bded3-256">Kliknij przycisk **Dodaj** do utworzenia nowego kontrolera dla **osoby** z szkieletów.</span><span class="sxs-lookup"><span data-stu-id="bded3-256">Click **Add** to create the new controller for **Person** with scaffolding.</span></span> <span data-ttu-id="bded3-257">Program Visual Studio wygeneruje następnie akcji kontrolera, kontekst danych osoby i widokami Razor.</span><span class="sxs-lookup"><span data-stu-id="bded3-257">Visual Studio will then generate the controller actions, the Person data context and the Razor views.</span></span>

    ![Po utworzeniu kontroler MVC z szkieletów](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image18.png)

    <span data-ttu-id="bded3-259">*Po utworzeniu kontroler MVC z szkieletów*</span><span class="sxs-lookup"><span data-stu-id="bded3-259">*After creating the MVC controller with scaffolding*</span></span>
7. <span data-ttu-id="bded3-260">Otwórz **MvcPersonController.cs** w pliku **kontrolerów** folderu.</span><span class="sxs-lookup"><span data-stu-id="bded3-260">Open the **MvcPersonController.cs** file in the **Controllers** folder.</span></span> <span data-ttu-id="bded3-261">Zwróć uwagę, że metody akcji CRUD został wygenerowany automatycznie.</span><span class="sxs-lookup"><span data-stu-id="bded3-261">Notice that the CRUD action methods have been generated automatically.</span></span>

    [!code-csharp[Main](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/samples/sample4.cs)]

    > [!NOTE]
    > <span data-ttu-id="bded3-262">Wybierając **używać asynchronicznych akcji kontrolera** pola wyboru z rusztowania opcje w poprzednich krokach, Visual Studio generuje metod asynchronicznych akcji dla wszystkich akcji, które wymagają dostępu do osoby kontekstu danych.</span><span class="sxs-lookup"><span data-stu-id="bded3-262">By selecting the **Use async controller actions** check box from the scaffolding options in the previous steps, Visual Studio generates asynchronous action methods for all actions that involve access to the Person data context.</span></span> <span data-ttu-id="bded3-263">Zaleca się, że w przypadku użycia metod asynchronicznych akcji dla długotrwałe, niezwiązane z procesorem powiązany żądania w celu unikania blokowania serwera sieci Web z wykonywania pracy podczas przetwarzania żądania.</span><span class="sxs-lookup"><span data-stu-id="bded3-263">It is recommended that you use asynchronous action methods for long-running, non-CPU bound requests to avoid blocking the Web server from performing work while the request is being processed.</span></span>

<a id="Ex2Task3"></a>
#### <a name="task-3--running-the-solution"></a><span data-ttu-id="bded3-264">Zadanie 3 — uruchomiona rozwiązania</span><span class="sxs-lookup"><span data-stu-id="bded3-264">Task 3 – Running the Solution</span></span>

<span data-ttu-id="bded3-265">W ramach tego zadania, należy uruchomić ponownie, aby sprawdzić, czy rozwiązanie widoków **osoby** działają zgodnie z oczekiwaniami.</span><span class="sxs-lookup"><span data-stu-id="bded3-265">In this task, you will run the solution again to verify that the views for **Person** are working as expected.</span></span> <span data-ttu-id="bded3-266">Spowoduje dodanie nowej osoby, aby zweryfikować, że został pomyślnie zapisany w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="bded3-266">You will add a new person to verify that it is successfully saved to the database.</span></span>

1. <span data-ttu-id="bded3-267">Naciśnij klawisz **F5** Aby uruchomić rozwiązanie.</span><span class="sxs-lookup"><span data-stu-id="bded3-267">Press **F5** to run the solution.</span></span>
2. <span data-ttu-id="bded3-268">Przejdź do **/MvcPerson**.</span><span class="sxs-lookup"><span data-stu-id="bded3-268">Navigate to **/MvcPerson**.</span></span> <span data-ttu-id="bded3-269">Widok szkieletu, który zawiera listę osób, powinny być wyświetlane.</span><span class="sxs-lookup"><span data-stu-id="bded3-269">The scaffolded view that shows the list of people should appear.</span></span>
3. <span data-ttu-id="bded3-270">Kliknij przycisk **Utwórz nowy** można dodać nowej osoby.</span><span class="sxs-lookup"><span data-stu-id="bded3-270">Click **Create New** to add a new person.</span></span>

    ![Nawigowanie do szkieletu widoków MVC](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image19.png)

    <span data-ttu-id="bded3-272">*Nawigowanie do szkieletu widoków MVC*</span><span class="sxs-lookup"><span data-stu-id="bded3-272">*Navigating to the scaffolded MVC views*</span></span>
4. <span data-ttu-id="bded3-273">W **Utwórz** wyświetlić, podaj **nazwa** i **wieku** osoby, a następnie kliknij przycisk **Utwórz**.</span><span class="sxs-lookup"><span data-stu-id="bded3-273">In the **Create** view, provide a **Name** and an **Age** for the person, and click **Create**.</span></span>

    ![Dodawanie nowej osoby](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image20.png)

    <span data-ttu-id="bded3-275">*Dodawanie nowej osoby*</span><span class="sxs-lookup"><span data-stu-id="bded3-275">*Adding a new person*</span></span>
5. <span data-ttu-id="bded3-276">Nowej osoby jest dodawany do listy.</span><span class="sxs-lookup"><span data-stu-id="bded3-276">The new person is added to the list.</span></span> <span data-ttu-id="bded3-277">Z listy element, kliknij przycisk **szczegóły** Aby wyświetlić szczegóły danej osoby.</span><span class="sxs-lookup"><span data-stu-id="bded3-277">In the element list, click **Details** to display the person's details view.</span></span> <span data-ttu-id="bded3-278">Następnie w **szczegóły** wyświetlić, kliknij przycisk **powrót do listy** aby powrócić do widoku listy.</span><span class="sxs-lookup"><span data-stu-id="bded3-278">Then, in the **Details** view, click **Back to List** to go back to the list view.</span></span>

    ![Widok szczegółów osoby](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image21.png)

    <span data-ttu-id="bded3-280">*Widok szczegółów osoby*</span><span class="sxs-lookup"><span data-stu-id="bded3-280">*Person's details view*</span></span>
6. <span data-ttu-id="bded3-281">Kliknij przycisk **usunąć** łącze, aby usunąć osoby.</span><span class="sxs-lookup"><span data-stu-id="bded3-281">Click the **Delete** link to delete the person.</span></span> <span data-ttu-id="bded3-282">W **usunąć** wyświetlić, kliknij przycisk **usunąć** o potwierdzenie operacji.</span><span class="sxs-lookup"><span data-stu-id="bded3-282">In the **Delete** view, click **Delete** to confirm the operation.</span></span>

    ![Usuwanie osoby](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image22.png)

    <span data-ttu-id="bded3-284">*Usuwanie osoby*</span><span class="sxs-lookup"><span data-stu-id="bded3-284">*Deleting a person*</span></span>
7. <span data-ttu-id="bded3-285">Wróć do programu Visual Studio i naciśnij klawisz **SHIFT + F5** można zatrzymać debugowania.</span><span class="sxs-lookup"><span data-stu-id="bded3-285">Go back to Visual Studio and press **SHIFT + F5** to stop debugging.</span></span>

<a id="Exercise3"></a>
### <a name="exercise-3-creating-a-web-api-controller-using-scaffolding"></a><span data-ttu-id="bded3-286">Ćwiczenie 3: Tworzenie kontrolera interfejsu API sieci Web przy użyciu funkcji szkieletów</span><span class="sxs-lookup"><span data-stu-id="bded3-286">Exercise 3: Creating a Web API Controller Using Scaffolding</span></span>

<span data-ttu-id="bded3-287">Strukturę interfejsu API sieci Web jest częścią stosu ASP.NET i przeznaczony do ułatwienia wykonawcze usługi HTTP, zazwyczaj wysyłania i odbierania danych w formacie JSON i XML za pomocą interfejsu API RESTful.</span><span class="sxs-lookup"><span data-stu-id="bded3-287">The Web API framework is part of the ASP.NET Stack and designed to make implementing HTTP services easier, generally sending and receiving JSON- or XML-formatted data through a RESTful API.</span></span>

<span data-ttu-id="bded3-288">W tym ćwiczeniu będzie szkieletów ASP.NET ponownie użyć do wygenerowania kontrolera interfejsu API sieci Web.</span><span class="sxs-lookup"><span data-stu-id="bded3-288">In this exercise, you will use ASP.NET Scaffolding again to generate a Web API controller.</span></span> <span data-ttu-id="bded3-289">Będzie używać tego samego **osoby** i **PersonContext** klas z poprzednim ćwiczeniu, aby zapewnić tych samych danych osoby w formacie JSON.</span><span class="sxs-lookup"><span data-stu-id="bded3-289">You will use the same **Person** and **PersonContext** classes from the previous exercise to provide the same person data in JSON format.</span></span> <span data-ttu-id="bded3-290">Zobaczysz, jak mogą uwidaczniać tych samych zasobów w różny sposób w tej samej aplikacji ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="bded3-290">You will see how you can expose the same resources in different ways within the same ASP.NET application.</span></span>

<a id="Ex3Task1"></a>
#### <a name="task-1--creating-a-web-api-controller"></a><span data-ttu-id="bded3-291">Zadanie 1 — Tworzenie kontrolera interfejsu API sieci Web</span><span class="sxs-lookup"><span data-stu-id="bded3-291">Task 1 – Creating a Web API Controller</span></span>

<span data-ttu-id="bded3-292">W ramach tego zadania spowoduje utworzenie nowej **Kontroler interfejsu API sieci Web** który powoduje to udostępnienie danych osoby w niestandardowe maszyny formatu JSON.</span><span class="sxs-lookup"><span data-stu-id="bded3-292">In this task you will create a new **Web API Controller** that will expose the person data in a machine-consumable format like JSON.</span></span>

1. <span data-ttu-id="bded3-293">Jeśli nie jest jeszcze otwarty, otwórz **programu Visual Studio Express 2013 for Web** , a następnie otwórz **MyHybridSite.sln** rozwiązania, znajdujących się w **źródło/Ex3-WebAPI/Begin** folderu.</span><span class="sxs-lookup"><span data-stu-id="bded3-293">If not already opened, open **Visual Studio Express 2013 for Web** and open the **MyHybridSite.sln** solution located in the **Source/Ex3-WebAPI/Begin** folder.</span></span> <span data-ttu-id="bded3-294">Alternatywnie możesz kontynuować z rozwiązaniem uzyskanymi w poprzednim ćwiczeniu.</span><span class="sxs-lookup"><span data-stu-id="bded3-294">Alternatively, you can continue with the solution that you obtained in the previous exercise.</span></span>

    > [!NOTE]
    > <span data-ttu-id="bded3-295">Po uruchomieniu z rozwiązaniem Rozpocznij od 3 do wykonywania, naciśnij klawisz **CTRL + SHIFT + B** celu skompilowania rozwiązania.</span><span class="sxs-lookup"><span data-stu-id="bded3-295">If you start with the Begin solution from Exercise 3, press **CTRL + SHIFT + B** to build the solution.</span></span>
2. <span data-ttu-id="bded3-296">W **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy **kontrolerów** folderu **MyHybridSite** projekt i wybierz **Dodaj | Nowy element szkieletu...** .</span><span class="sxs-lookup"><span data-stu-id="bded3-296">In **Solution Explorer**, right-click the **Controllers** folder of the **MyHybridSite** project and select **Add | New Scaffolded Item...**.</span></span>

    ![Tworzenie nowego kontrolera szkieletu](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image23.png)

    <span data-ttu-id="bded3-298">*Tworzenie nowego kontrolera szkieletu*</span><span class="sxs-lookup"><span data-stu-id="bded3-298">*Creating a new scaffolded Controller*</span></span>
3. <span data-ttu-id="bded3-299">W **Dodawanie szkieletu** okno dialogowe, wybierz opcję **interfejsu API sieci Web** w okienku po lewej stronie, następnie **Web Kontroler interfejsu API 2 z akcjami używający narzędzia Entity Framework** w środkowym okienku, a następnie kliknij przycisk  **Dodaj.**</span><span class="sxs-lookup"><span data-stu-id="bded3-299">In the **Add Scaffold** dialog box, select **Web API** in the left pane, then **Web API 2 Controller with actions, using Entity Framework** in the middle pane and then click **Add.**</span></span>

    <span data-ttu-id="bded3-300">![Wybiera kontroler 2 interfejsu API sieci Web z akcjami i Entity Framework](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image24.png "Wybieranie sieci Web Kontroler interfejsu API 2 z akcjami i Entity Framework")</span><span class="sxs-lookup"><span data-stu-id="bded3-300">![Selecting Web API 2 Controller with actions and Entity Framework](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image24.png "Selecting Web API 2 Controller with actions and Entity Framework")</span></span>

    <span data-ttu-id="bded3-301">*Wybór kontroler Web API 2 z akcjami i Entity Framework*</span><span class="sxs-lookup"><span data-stu-id="bded3-301">*Selecting Web API 2 Controller with actions and Entity Framework*</span></span>
4. <span data-ttu-id="bded3-302">Ustaw *ApiPersonController* jako **nazwy kontrolera**, wybierz pozycję **używać asynchronicznych akcji kontrolera** opcję i zaznacz **osoby (MyHybridSite.Models)**  i **PersonContext (MyHybridSite.Models)** jako **modelu** i **kontekstu danych** odpowiednio klasy.</span><span class="sxs-lookup"><span data-stu-id="bded3-302">Set *ApiPersonController* as the **Controller name**, select the **Use async controller actions** option and select **Person (MyHybridSite.Models)** and **PersonContext (MyHybridSite.Models)** as the **Model** and **Data context** classes respectively.</span></span> <span data-ttu-id="bded3-303">Następnie kliknij przycisk **Dodaj**.</span><span class="sxs-lookup"><span data-stu-id="bded3-303">Then click **Add**.</span></span>

    <span data-ttu-id="bded3-304">![Dodawanie kontrolera interfejsu API sieci Web z szkieletów](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image25.png "dodawania kontrolera interfejsu API sieci Web z szkieletów")</span><span class="sxs-lookup"><span data-stu-id="bded3-304">![Adding a Web API Controller with scaffolding](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image25.png "Adding a Web API controller with scaffolding")</span></span>

    <span data-ttu-id="bded3-305">*Dodawanie kontrolera interfejsu API sieci Web z szkieletów*</span><span class="sxs-lookup"><span data-stu-id="bded3-305">*Adding a Web API controller with scaffolding*</span></span>
5. <span data-ttu-id="bded3-306">Następnie program Visual Studio wygeneruje **ApiPersonController** klasy z czterech akcjami CRUD do pracy z danymi.</span><span class="sxs-lookup"><span data-stu-id="bded3-306">Visual Studio will then generate the **ApiPersonController** class with the four CRUD actions to work with your data.</span></span>

    <span data-ttu-id="bded3-307">![Po utworzeniu kontrolera interfejsu API sieci Web za pomocą szkieletów](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image26.png "po utworzeniu kontrolera interfejsu API sieci Web za pomocą szkieletów")</span><span class="sxs-lookup"><span data-stu-id="bded3-307">![After creating the Web API controller with scaffolding](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image26.png "After creating the Web API controller with scaffolding")</span></span>

    <span data-ttu-id="bded3-308">*Po utworzeniu kontrolera interfejsu API sieci Web za pomocą szkieletów*</span><span class="sxs-lookup"><span data-stu-id="bded3-308">*After creating the Web API controller with scaffolding*</span></span>
6. <span data-ttu-id="bded3-309">Otwórz **ApiPersonController.cs** plików i sprawdzić *GetPeople* metody akcji.</span><span class="sxs-lookup"><span data-stu-id="bded3-309">Open the **ApiPersonController.cs** file and inspect the *GetPeople* action method.</span></span> <span data-ttu-id="bded3-310">Ta metoda odpytuje db zakresie **PersonContext** typu w celu pobrania danych osoby.</span><span class="sxs-lookup"><span data-stu-id="bded3-310">This method queries the db field of **PersonContext** type in order to get the people data.</span></span>

    [!code-csharp[Main](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/samples/sample5.cs)]
7. <span data-ttu-id="bded3-311">Teraz można zauważyć komentarz powyżej definicją metody.</span><span class="sxs-lookup"><span data-stu-id="bded3-311">Now notice the comment above the method definition.</span></span> <span data-ttu-id="bded3-312">Zawiera identyfikator URI, który udostępnia tę akcję, która zostanie użyta w następnym zadaniem.</span><span class="sxs-lookup"><span data-stu-id="bded3-312">It provides the URI that exposes this action which you will use in the next task.</span></span>

    [!code-csharp[Main](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/samples/sample6.cs)]

    > [!NOTE]
    > <span data-ttu-id="bded3-313">Domyślnie skonfigurowano catch zapytań do interfejsu API sieci Web */api* ścieżkę, aby uniknąć kolizji z kontrolerów MVC.</span><span class="sxs-lookup"><span data-stu-id="bded3-313">By default, Web API is configured to catch the queries to the */api* path to avoid collisions with MVC controllers.</span></span> <span data-ttu-id="bded3-314">Jeśli musisz zmienić to ustawienie, można skorzystać z [routingu na platformie ASP.NET Web API](../../../web-api/overview/web-api-routing-and-actions/routing-in-aspnet-web-api.md).</span><span class="sxs-lookup"><span data-stu-id="bded3-314">If you need to change this setting, refer to [Routing in ASP.NET Web API](../../../web-api/overview/web-api-routing-and-actions/routing-in-aspnet-web-api.md).</span></span>

<a id="Ex3Task2"></a>
#### <a name="task-2--running-the-solution"></a><span data-ttu-id="bded3-315">Zadanie 2 — rozwiązanie uruchomiona</span><span class="sxs-lookup"><span data-stu-id="bded3-315">Task 2 – Running the Solution</span></span>

<span data-ttu-id="bded3-316">W tym zadaniu korzystasz z programu Internet Explorer **narzędzi deweloperskich F12** do zbadania pełnego odpowiedzi z kontrolera interfejsu API sieci Web.</span><span class="sxs-lookup"><span data-stu-id="bded3-316">In this task you will use the Internet Explorer **F12 developer tools** to inspect the full response from the Web API controller.</span></span> <span data-ttu-id="bded3-317">Zobaczysz, jak można przechwytywać ruch sieciowy do pobrania uzyskać lepszy wgląd w dane aplikacji.</span><span class="sxs-lookup"><span data-stu-id="bded3-317">You will see how you can capture network traffic to get more insight into your application data.</span></span>

> [!NOTE]
> <span data-ttu-id="bded3-318">Upewnij się, że **programu Internet Explorer** wybrano **Start** przycisk znajdujący się na pasku narzędzi programu Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="bded3-318">Make sure that **Internet Explorer** is selected in the **Start** button located on the Visual Studio toolbar.</span></span>
> 
> ![Opcji programu Internet Explorer](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image27.png)
> 
> <span data-ttu-id="bded3-320">**Narzędzi deweloperskich F12** mają szeroki zestaw funkcji, które nie są ujęte w tym hands-na-laboratorium.</span><span class="sxs-lookup"><span data-stu-id="bded3-320">The **F12 developer tools** have a wide set of functionality that is not covered in this hands-on-lab.</span></span> <span data-ttu-id="bded3-321">Aby uzyskać więcej informacji na ten temat, zapoznaj się [korzystania z narzędzi deweloperskich F12](https://msdn.microsoft.com/library/ie/bg182326(v=vs.85)).</span><span class="sxs-lookup"><span data-stu-id="bded3-321">If you want to learn more about it, refer to [Using the F12 developer tools](https://msdn.microsoft.com/library/ie/bg182326(v=vs.85)).</span></span>


1. <span data-ttu-id="bded3-322">Naciśnij klawisz **F5** Aby uruchomić rozwiązanie.</span><span class="sxs-lookup"><span data-stu-id="bded3-322">Press **F5** to run the solution.</span></span>

    > [!NOTE]
    > <span data-ttu-id="bded3-323">Aby wykonać to zadanie poprawnie, aplikacja musi mieć danych.</span><span class="sxs-lookup"><span data-stu-id="bded3-323">In order to follow this task correctly, your application needs to have data.</span></span> <span data-ttu-id="bded3-324">Jeśli baza danych jest pusta, można powrócić do 3 zadania w 2 wykonywania i wykonaj kroki dotyczące tworzenia nowej osoby za pomocą widoków MVC.</span><span class="sxs-lookup"><span data-stu-id="bded3-324">If your database is empty, you can go back to Task 3 in Exercise 2 and follow the steps on how to create a new person using the MVC views.</span></span>
2. <span data-ttu-id="bded3-325">W przeglądarce, naciśnij klawisz **F12** otworzyć **Developer Tools** panelu.</span><span class="sxs-lookup"><span data-stu-id="bded3-325">In the browser, press **F12** to open the **Developer Tools** panel.</span></span> <span data-ttu-id="bded3-326">Naciśnij klawisz **CTRL** + **4** lub kliknij przycisk **sieci** ikonę, a następnie kliknij przycisk zieloną strzałkę, aby rozpocząć przechwytywanie ruchu sieciowego.</span><span class="sxs-lookup"><span data-stu-id="bded3-326">Press **CTRL** + **4** or click the **Network** icon, and then click the green arrow button to begin capturing network traffic.</span></span>

    <span data-ttu-id="bded3-327">![Inicjowanie przechwytywania ruchu sieciowego interfejsu API sieci Web](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image28.png "przechwytywania ruchu sieciowego Inicjowanie interfejsu API sieci Web")</span><span class="sxs-lookup"><span data-stu-id="bded3-327">![Initiating Web API network capture](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image28.png "Initiating Web API network capture")</span></span>

    <span data-ttu-id="bded3-328">*Inicjowanie przechwytywania ruchu sieciowego interfejsu API sieci Web*</span><span class="sxs-lookup"><span data-stu-id="bded3-328">*Initiating Web API network capture*</span></span>
3. <span data-ttu-id="bded3-329">Dołącz **interfejsu api/ApiPerson** do adresu URL na pasku adresu przeglądarki.</span><span class="sxs-lookup"><span data-stu-id="bded3-329">Append **api/ApiPerson** to the URL in the browser's address bar.</span></span> <span data-ttu-id="bded3-330">Teraz będzie przeprowadzał inspekcję szczegóły odpowiedzi z **ApiPersonController**.</span><span class="sxs-lookup"><span data-stu-id="bded3-330">You will now inspect the details of the response from the **ApiPersonController**.</span></span>

    <span data-ttu-id="bded3-331">![Pobieranie danych osoby za pomocą interfejsu API sieci Web](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image29.png "pobierania danych osoby za pomocą interfejsu API sieci Web")</span><span class="sxs-lookup"><span data-stu-id="bded3-331">![Retrieving person data through Web API](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image29.png "Retrieving person data through Web API")</span></span>

    <span data-ttu-id="bded3-332">*Pobieranie danych osoby za pomocą interfejsu API sieci Web*</span><span class="sxs-lookup"><span data-stu-id="bded3-332">*Retrieving person data through Web API*</span></span>

    > [!NOTE]
    > <span data-ttu-id="bded3-333">Po zakończeniu pobierania pojawi się monit dokonanie akcji z pobranego pliku.</span><span class="sxs-lookup"><span data-stu-id="bded3-333">Once the download finishes, you will be prompted to make an action with the downloaded file.</span></span> <span data-ttu-id="bded3-334">Pozostaw to okno dialogowe otwarte, aby można było oglądać zawartości odpowiedzi, za pomocą okna narzędzia dla deweloperów.</span><span class="sxs-lookup"><span data-stu-id="bded3-334">Leave the dialog box open in order to be able to watch the response content through the Developers Tool window.</span></span>
4. <span data-ttu-id="bded3-335">Teraz będzie przeprowadzał inspekcję treść odpowiedzi.</span><span class="sxs-lookup"><span data-stu-id="bded3-335">Now you will inspect the body of the response.</span></span> <span data-ttu-id="bded3-336">Aby to zrobić, kliknij przycisk **szczegóły** a następnie kliknij pozycję **treść odpowiedzi**.</span><span class="sxs-lookup"><span data-stu-id="bded3-336">To do this, click the **Details** tab and then click **Response body**.</span></span> <span data-ttu-id="bded3-337">Można sprawdzić, czy pobrane dane mają listę obiektów z właściwościami **identyfikator**, **nazwa** i **wieku** odpowiadające **osoby** Klasa.</span><span class="sxs-lookup"><span data-stu-id="bded3-337">You can check that the downloaded data is a list of objects with the properties **Id**, **Name** and **Age** that correspond to the **Person** class.</span></span>

    <span data-ttu-id="bded3-338">![Treść odpowiedzi interfejsu API sieci Web wyświetlanie](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image30.png "przeglądania sieci Web treść odpowiedzi interfejsu API")</span><span class="sxs-lookup"><span data-stu-id="bded3-338">![Viewing Web API Response Body](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image30.png "Viewing Web API Response Body")</span></span>

    <span data-ttu-id="bded3-339">*Treść odpowiedzi interfejsu API sieci Web wyświetlania*</span><span class="sxs-lookup"><span data-stu-id="bded3-339">*Viewing Web API Response Body*</span></span>

<a id="Ex3Task3"></a>
#### <a name="task-3--adding-web-api-help-pages"></a><span data-ttu-id="bded3-340">Zadanie 3 — Dodawanie strony pomocy interfejsu API sieci Web</span><span class="sxs-lookup"><span data-stu-id="bded3-340">Task 3 – Adding Web API Help Pages</span></span>

<span data-ttu-id="bded3-341">Utwórz interfejs API sieci Web, jest przydatne do tworzenia strony pomocy, aby inni deweloperzy będą wiedzieli, sposób wywołania interfejsu API.</span><span class="sxs-lookup"><span data-stu-id="bded3-341">When you create a Web API, it is useful to create a help page so that other developers will know how to call your API.</span></span> <span data-ttu-id="bded3-342">Należy utworzyć i ręcznie zaktualizować stron z dokumentacją, ale zaleca się automatycznego generowania je, aby uniknąć konieczności wykonywać zadania konserwacji.</span><span class="sxs-lookup"><span data-stu-id="bded3-342">You could create and update the documentation pages manually, but it is better to auto-generate them to avoid having to do maintenance work.</span></span> <span data-ttu-id="bded3-343">W tym zadaniu użyjesz pakiet Nuget do automatycznego generowania strony pomocy interfejsu API sieci Web do rozwiązania.</span><span class="sxs-lookup"><span data-stu-id="bded3-343">In this task you will use a Nuget package to automatically generate Web API help pages to the solution.</span></span>

1. <span data-ttu-id="bded3-344">Z **narzędzia** menu w programie Visual Studio, wybierz **Menedżer pakietów biblioteki**, a następnie kliknij przycisk **Konsola Menedżera pakietów**.</span><span class="sxs-lookup"><span data-stu-id="bded3-344">From the **Tools** menu in Visual Studio, select **Library Package Manager**, and then click **Package Manager Console**.</span></span>
2. <span data-ttu-id="bded3-345">W **Konsola Menedżera pakietów** okna, uruchom następujące polecenie:</span><span class="sxs-lookup"><span data-stu-id="bded3-345">In the **Package Manager Console** window, execute the following command:</span></span>

    [!code-powershell[Main](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/samples/sample7.ps1)]

    > [!NOTE]
    > <span data-ttu-id="bded3-346">**Microsoft.AspNet.WebApi.HelpPage** pakiet instaluje konieczne zestawy i dodaje widoków MVC dla strony pomocy w obszarze **obszarów/HelpPage** folderu.</span><span class="sxs-lookup"><span data-stu-id="bded3-346">The **Microsoft.AspNet.WebApi.HelpPage** package installs the necessary assemblies and adds MVC Views for the help pages under the **Areas/HelpPage** folder.</span></span>

    <span data-ttu-id="bded3-347">![Obszar HelpPage](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image31.png "HelpPage obszaru")</span><span class="sxs-lookup"><span data-stu-id="bded3-347">![HelpPage Area](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image31.png "HelpPage Area")</span></span>

    <span data-ttu-id="bded3-348">*Obszar HelpPage*</span><span class="sxs-lookup"><span data-stu-id="bded3-348">*HelpPage Area*</span></span>
3. <span data-ttu-id="bded3-349">Domyślnie pomocy strony mają ciągów symbolu zastępczego dla dokumentacji.</span><span class="sxs-lookup"><span data-stu-id="bded3-349">By default, the help pages have placeholder strings for documentation.</span></span> <span data-ttu-id="bded3-350">Komentarze dokumentacji XML służy do tworzenia w dokumentacji.</span><span class="sxs-lookup"><span data-stu-id="bded3-350">You can use XML documentation comments to create the documentation.</span></span> <span data-ttu-id="bded3-351">Aby włączyć tę funkcję, otwórz **HelpPageConfig.cs** plik znajdujący się w **App-obszarów/HelpPage\_Start** folder i Usuń komentarz następujący wiersz:</span><span class="sxs-lookup"><span data-stu-id="bded3-351">To enable this feature, open the **HelpPageConfig.cs** file located in the **Areas/HelpPage/App\_Start** folder and uncomment the following line:</span></span>

    [!code-javascript[Main](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/samples/sample8.js)]
4. <span data-ttu-id="bded3-352">W **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy projekt **MyHybridSite**, wybierz pozycję **właściwości** i kliknij przycisk **kompilacji** kartę.</span><span class="sxs-lookup"><span data-stu-id="bded3-352">In **Solution Explorer**, right-click the project **MyHybridSite**, select **Properties** and click the **Build** tab.</span></span>

    <span data-ttu-id="bded3-353">![Tworzenie kartę](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image32.png "kompilacji sekcji")</span><span class="sxs-lookup"><span data-stu-id="bded3-353">![Build tab](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image32.png "Build section")</span></span>

    <span data-ttu-id="bded3-354">*Karta kompilacji*</span><span class="sxs-lookup"><span data-stu-id="bded3-354">*Build tab*</span></span>
5. <span data-ttu-id="bded3-355">W obszarze **dane wyjściowe**, wybierz pozycję **pliku dokumentacji XML**.</span><span class="sxs-lookup"><span data-stu-id="bded3-355">Under **Output**, select **XML documentation file**.</span></span> <span data-ttu-id="bded3-356">W polu edycji wpisz **aplikacji\_Data/XmlDocument.xml**.</span><span class="sxs-lookup"><span data-stu-id="bded3-356">In the edit box, type **App\_Data/XmlDocument.xml**.</span></span>

    <span data-ttu-id="bded3-357">![Dane wyjściowe części kart Build](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image33.png "Output sekcji karcie kompilacji")</span><span class="sxs-lookup"><span data-stu-id="bded3-357">![Output section in Build tab](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image33.png "Output section in build tab")</span></span>

    <span data-ttu-id="bded3-358">*Sekcja danych wyjściowych w karcie kompilacji*</span><span class="sxs-lookup"><span data-stu-id="bded3-358">*Output section in Build tab*</span></span>
6. <span data-ttu-id="bded3-359">Naciśnij klawisz **CTRL** + **S** Aby zapisać zmiany.</span><span class="sxs-lookup"><span data-stu-id="bded3-359">Press **CTRL** + **S** to save the changes.</span></span>
7. <span data-ttu-id="bded3-360">Otwórz **ApiPersonController.cs** plik z **kontrolerów** folderu.</span><span class="sxs-lookup"><span data-stu-id="bded3-360">Open the **ApiPersonController.cs** file from the **Controllers** folder.</span></span>
8. <span data-ttu-id="bded3-361">Wprowadź nowy wiersz między *GetPeople* podpis metody i */ / api/ApiPerson GET* komentarz, a następnie wpisz trzy kreskami ukośnymi.</span><span class="sxs-lookup"><span data-stu-id="bded3-361">Enter a new line between the *GetPeople* method signature and the *// GET api/ApiPerson* comment, and then type three forward slashes.</span></span>

    > [!NOTE]
    > <span data-ttu-id="bded3-362">Visual Studio automatycznie wstawia elementy XML, które definiują dokumentacji — metoda.</span><span class="sxs-lookup"><span data-stu-id="bded3-362">Visual Studio automatically inserts the XML elements which define the method documentation.</span></span>
9. <span data-ttu-id="bded3-363">Dodawanie tekstu podsumowania i wartość zwracana *GetPeople* metody.</span><span class="sxs-lookup"><span data-stu-id="bded3-363">Add a summary text and the return value for the *GetPeople* method.</span></span> <span data-ttu-id="bded3-364">Jego wygląd powinien być podobne do następujących.</span><span class="sxs-lookup"><span data-stu-id="bded3-364">It should look like the following.</span></span>

    [!code-csharp[Main](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/samples/sample9.cs)]
10. <span data-ttu-id="bded3-365">Naciśnij klawisz **F5** Aby uruchomić rozwiązanie.</span><span class="sxs-lookup"><span data-stu-id="bded3-365">Press **F5** to run the solution.</span></span>
11. <span data-ttu-id="bded3-366">Dołącz **/help** do adresu URL na pasku adresu, aby przejść do strony pomocy.</span><span class="sxs-lookup"><span data-stu-id="bded3-366">Append **/help** to the URL in the address bar to browse to the help page.</span></span>

    <span data-ttu-id="bded3-367">![Strona pomocy interfejsu API sieci Web ASP.NET](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image34.png "strona pomocy interfejsu API sieci Web ASP.NET")</span><span class="sxs-lookup"><span data-stu-id="bded3-367">![ASP.NET Web API Help Page](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image34.png "ASP.NET Web API Help Page")</span></span>

    <span data-ttu-id="bded3-368">*Strona pomocy interfejsu API sieci Web ASP.NET*</span><span class="sxs-lookup"><span data-stu-id="bded3-368">*ASP.NET Web API Help Page*</span></span>

    > [!NOTE]
    > <span data-ttu-id="bded3-369">Główne zawartość strony znajduje się tabela interfejsów API, które są pogrupowane według kontrolera.</span><span class="sxs-lookup"><span data-stu-id="bded3-369">The main content of the page is a table of APIs, grouped by controller.</span></span> <span data-ttu-id="bded3-370">Wpisy tabeli są generowane dynamicznie, przy użyciu **IApiExplorer** interfejsu.</span><span class="sxs-lookup"><span data-stu-id="bded3-370">The table entries are generated dynamically, using the **IApiExplorer** interface.</span></span> <span data-ttu-id="bded3-371">Jeśli musisz dodać lub zaktualizować Kontroler interfejsu API, tabeli będą automatycznie aktualizowane przy następnym kompilowania aplikacji.</span><span class="sxs-lookup"><span data-stu-id="bded3-371">If you add or update an API controller, the table will be automatically updated the next time you build the application.</span></span>
    > 
    > <span data-ttu-id="bded3-372">**Interfejsu API** kolumna zawiera metody HTTP oraz względnego identyfikatora URI.</span><span class="sxs-lookup"><span data-stu-id="bded3-372">The **API** column lists the HTTP method and relative URI.</span></span> <span data-ttu-id="bded3-373">**Opis** kolumna zawiera informacje, który został wyodrębniony z dokumentacji metody.</span><span class="sxs-lookup"><span data-stu-id="bded3-373">The **Description** column contains information that has been extracted from the method's documentation.</span></span>
12. <span data-ttu-id="bded3-374">Należy pamiętać, że opis dodanych powyżej definicja metody jest wyświetlany w kolumnie Opis.</span><span class="sxs-lookup"><span data-stu-id="bded3-374">Note that the description you added above the method definition is displayed in the description column.</span></span>

    <span data-ttu-id="bded3-375">![Opis metody interfejsu API](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image35.png "opis metody interfejsu API")</span><span class="sxs-lookup"><span data-stu-id="bded3-375">![API method description](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image35.png "API method description")</span></span>

    <span data-ttu-id="bded3-376">*Opis metody interfejsu API*</span><span class="sxs-lookup"><span data-stu-id="bded3-376">*API method description*</span></span>
13. <span data-ttu-id="bded3-377">Kliknij jedną z metod interfejsu API można przejść do strony z bardziej szczegółowe informacje, w tym próbki treści odpowiedzi.</span><span class="sxs-lookup"><span data-stu-id="bded3-377">Click one of the API methods to navigate to a page with more detailed information, including sample response bodies.</span></span>

    <span data-ttu-id="bded3-378">![Strona informacji o szczegółów](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image36.png "szczegółowe informacje o strony")</span><span class="sxs-lookup"><span data-stu-id="bded3-378">![Detail Information page](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image36.png "Detail Information page")</span></span>

    <span data-ttu-id="bded3-379">*Szczegółowe informacje na stronie*</span><span class="sxs-lookup"><span data-stu-id="bded3-379">*Detailed information page*</span></span>

* * *

<a id="Summary"></a>
## <a name="summary"></a><span data-ttu-id="bded3-380">Podsumowanie</span><span class="sxs-lookup"><span data-stu-id="bded3-380">Summary</span></span>

<span data-ttu-id="bded3-381">Wykonując tego laboratorium praktycznego wiesz już, jak:</span><span class="sxs-lookup"><span data-stu-id="bded3-381">By completing this hands-on lab you have learned how to:</span></span>

- <span data-ttu-id="bded3-382">Tworzenie nowej aplikacji sieci Web przy użyciu jednego środowiska ASP.NET w programie Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="bded3-382">Create a new Web application using the One ASP.NET Experience in Visual Studio 2013</span></span>
- <span data-ttu-id="bded3-383">Integracji wiele technologii ASP.NET z jednego pojedynczego projektu</span><span class="sxs-lookup"><span data-stu-id="bded3-383">Integrate multiple ASP.NET technologies into one single project</span></span>
- <span data-ttu-id="bded3-384">Generowanie widoków i kontrolerów MVC z klas modelu przy użyciu funkcji szkieletów ASP.NET</span><span class="sxs-lookup"><span data-stu-id="bded3-384">Generate MVC controllers and views from your model classes using ASP.NET Scaffolding</span></span>
- <span data-ttu-id="bded3-385">Generowanie kontrolerów interfejsu API sieci Web, korzystających z funkcji, takich jak programowania asynchronicznego i dostęp do danych za pośrednictwem programu Entity Framework</span><span class="sxs-lookup"><span data-stu-id="bded3-385">Generate Web API controllers, which use features such as Async Programming and data access through Entity Framework</span></span>
- <span data-ttu-id="bded3-386">Automatycznego generowania strony pomocy interfejsu API sieci Web dla kontrolerów</span><span class="sxs-lookup"><span data-stu-id="bded3-386">Automatically generate Web API Help Pages for your controllers</span></span>
