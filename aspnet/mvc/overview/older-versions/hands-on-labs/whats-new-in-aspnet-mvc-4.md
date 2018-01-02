---
uid: mvc/overview/older-versions/hands-on-labs/whats-new-in-aspnet-mvc-4
title: Co to jest nowe w programie ASP.NET MVC 4 | Dokumentacja firmy Microsoft
author: rick-anderson
description: "ASP.NET MVC 4 to struktura służąca do tworzenia aplikacji sieci web skalowalnych, opartych na standardach za pomocą często używanych wzorów projektów oraz funkcji platform ASP.NET i..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/18/2013
ms.topic: article
ms.assetid: 48f7feb3-872f-485d-b96f-e30011ff8c4a
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/hands-on-labs/whats-new-in-aspnet-mvc-4
msc.type: authoredcontent
ms.openlocfilehash: 3de952224e23eed29f90ed0e8c662e4ee3f531ce
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/10/2017
---
<a name="whats-new-in-aspnet-mvc-4"></a><span data-ttu-id="d0549-103">Co to jest nowe w programie ASP.NET MVC 4</span><span class="sxs-lookup"><span data-stu-id="d0549-103">What's New in ASP.NET MVC 4</span></span>
====================
<span data-ttu-id="d0549-104">przez [obozów sieci Web Team](https://twitter.com/webcamps)</span><span class="sxs-lookup"><span data-stu-id="d0549-104">by [Web Camps Team](https://twitter.com/webcamps)</span></span>

[<span data-ttu-id="d0549-105">Pobierz obozów sieci Web uczenie Kit</span><span class="sxs-lookup"><span data-stu-id="d0549-105">Download Web Camps Training Kit</span></span>](http://www.microsoft.com/en-us/download/29843)

> <span data-ttu-id="d0549-106">ASP.NET MVC 4 to struktura służąca do tworzenia aplikacji sieci web skalowalnych, opartych na standardach za pomocą często używanych wzorów projektów oraz funkcji platform ASP.NET i .NET framework.</span><span class="sxs-lookup"><span data-stu-id="d0549-106">ASP.NET MVC 4 is a framework for building scalable, standards-based web applications using well-established design patterns and the power of the ASP.NET and the .NET framework.</span></span> <span data-ttu-id="d0549-107">Nowy, czwarty wersji platformy koncentruje się na ułatwiając tworzenie aplikacji sieci web urządzeń przenośnych.</span><span class="sxs-lookup"><span data-stu-id="d0549-107">This new, fourth version of the framework focuses on making mobile web application development easier.</span></span>
> 
> <span data-ttu-id="d0549-108">Najpierw utwórz nowy projekt ASP.NET MVC 4 ma teraz szablon projektu aplikacji mobilnej, który umożliwia tworzenie aplikacji autonomicznej specjalnie dla urządzeń przenośnych.</span><span class="sxs-lookup"><span data-stu-id="d0549-108">To begin with, when you create a new ASP.NET MVC 4 project there is now a mobile application project template you can use to build a standalone app specifically for mobile devices.</span></span> <span data-ttu-id="d0549-109">Ponadto program ASP.NET MVC 4 integruje się z jQuery Mobile za pośrednictwem pakietu NuGet jQuery.Mobile.MVC.</span><span class="sxs-lookup"><span data-stu-id="d0549-109">Additionally, ASP.NET MVC 4 integrates with jQuery Mobile through a jQuery.Mobile.MVC NuGet package.</span></span> <span data-ttu-id="d0549-110">jQuery Mobile to struktura na podstawie HTML5 umożliwiający projektowanie aplikacji sieci web, które są zgodne z wszystkich platform popularnym urządzeniu przenośnym, w tym Windows Phone, iPhone, Android i tak dalej.</span><span class="sxs-lookup"><span data-stu-id="d0549-110">jQuery Mobile is an HTML5-based framework for developing web apps that are compatible with all popular mobile device platforms, including Windows Phone, iPhone, Android and so on.</span></span> <span data-ttu-id="d0549-111">Jednak specjalizacji, należy ASP.NET MVC 4 umożliwia również obsługiwać różne widoki dla różnych urządzeń i podaj optymalizacje specyficzne dla urządzenia.</span><span class="sxs-lookup"><span data-stu-id="d0549-111">However, if you need specialization, ASP.NET MVC 4 also enables you to serve different views for different devices and provide device-specific optimizations.</span></span>
> 
> <span data-ttu-id="d0549-112">W tym laboratorium praktycznego rozpocznie z platformy ASP.NET MVC 4 &quot;aplikacji internetowej&quot; szablon projektu do tworzenia aplikacji galerii fotografii.</span><span class="sxs-lookup"><span data-stu-id="d0549-112">In this hands-on lab, you will start with the ASP.NET MVC 4 &quot;Internet Application&quot; project template to create a Photo Gallery application.</span></span> <span data-ttu-id="d0549-113">Stopniowo umożliwiających ulepszenie aplikacji przy użyciu technologii jQuery Mobile i nowe funkcje platformy ASP.NET MVC 4, aby był zgodny z różnych urządzeń przenośnych i przeglądarki sieci web.</span><span class="sxs-lookup"><span data-stu-id="d0549-113">You will progressively enhance the app using jQuery Mobile and ASP.NET MVC 4's new features to make it compatible with different mobile devices and desktop web browsers.</span></span> <span data-ttu-id="d0549-114">Możesz także informacje dotyczące nowych przepisami kodu dla generowania kodu i jak ASP.NET MVC 4 pozwala na łatwiejsze do pisania metod asynchronicznych akcji przez zadanie obsługi&lt;ActionResult&gt; zwracanych typów.</span><span class="sxs-lookup"><span data-stu-id="d0549-114">You will also learn about new code recipes for code generation and how ASP.NET MVC 4 makes it easier for you to write asynchronous action methods by supporting Task&lt;ActionResult&gt; return types.</span></span>
> 
> <span data-ttu-id="d0549-115">Wszystkie przykładowy kod i fragmenty kodu są uwzględnione w sieci Web obozów zestaw szkoleniowy, dostępne pod adresem [https://www.microsoft.com/en-us/download/29843](https://www.microsoft.com/en-us/download/29843).</span><span class="sxs-lookup"><span data-stu-id="d0549-115">All sample code and snippets are included in the Web Camps Training Kit, available at [https://www.microsoft.com/en-us/download/29843](https://www.microsoft.com/en-us/download/29843).</span></span>


<a id="Objectives"></a>

<a id="Objectives"></a>
### <a name="objectives"></a><span data-ttu-id="d0549-116">Cele</span><span class="sxs-lookup"><span data-stu-id="d0549-116">Objectives</span></span>

<span data-ttu-id="d0549-117">W tym laboratorium praktycznego przedstawiono sposób:</span><span class="sxs-lookup"><span data-stu-id="d0549-117">In this hands-on lab, you will learn how to:</span></span>

- <span data-ttu-id="d0549-118">Zawiera ulepszenia do platformy ASP.NET MVC projektu szablony tym szablonie projektu aplikacji mobilnej</span><span class="sxs-lookup"><span data-stu-id="d0549-118">Take advantage of the enhancements to the ASP.NET MVC project templates-including the new mobile application project template</span></span>
- <span data-ttu-id="d0549-119">Za pomocą atrybutu okienka ekranu HTML5 i zapytaniami multimediów CSS zwiększyć wyświetlania na urządzeniach przenośnych</span><span class="sxs-lookup"><span data-stu-id="d0549-119">Use the HTML5 viewport attribute and CSS media queries to improve the display on mobile devices</span></span>
- <span data-ttu-id="d0549-120">Ulepszenia progresywnego oraz tworzenie zoptymalizowanych pod kątem touch interfejsu użytkownika sieci web mieć jQuery Mobile</span><span class="sxs-lookup"><span data-stu-id="d0549-120">Use jQuery Mobile for progressive enhancements and for building touch-optimized web UI</span></span>
- <span data-ttu-id="d0549-121">Utwórz widoki specyficzne dla mobile</span><span class="sxs-lookup"><span data-stu-id="d0549-121">Create mobile-specific views</span></span>
- <span data-ttu-id="d0549-122">Korzystanie ze składnika tym przełącznikiem widoku do przełączania się między widokami aplikacji mobilnych i klasycznych</span><span class="sxs-lookup"><span data-stu-id="d0549-122">Use the view-switcher component to toggle between mobile and desktop views in the application</span></span>
- <span data-ttu-id="d0549-123">Utwórz asynchroniczne kontrolerów przy użyciu zadań pomocy technicznej</span><span class="sxs-lookup"><span data-stu-id="d0549-123">Create asynchronous controllers using task support</span></span>

<a id="Prerequisites"></a>

<a id="Prerequisites"></a>
### <a name="prerequisites"></a><span data-ttu-id="d0549-124">Wymagania wstępne</span><span class="sxs-lookup"><span data-stu-id="d0549-124">Prerequisites</span></span>

<span data-ttu-id="d0549-125">Musi mieć następujące elementy do przygotowania tego laboratorium:</span><span class="sxs-lookup"><span data-stu-id="d0549-125">You must have the following items to complete this lab:</span></span>

- <span data-ttu-id="d0549-126">[Microsoft Visual Studio Express 2012 for Web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web) lub wyższego poziomu (odczytu [dodatek B](#AppendixB) instrukcje dotyczące sposobu jego instalacji).</span><span class="sxs-lookup"><span data-stu-id="d0549-126">[Microsoft Visual Studio Express 2012 for Web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web) or superior (read [Appendix B](#AppendixB) for instructions on how to install it).</span></span>
- <span data-ttu-id="d0549-127">[ASP.NET MVC 4](../../../mvc4.md) (dołączone do instalacji programu Microsoft Visual Studio 2012)</span><span class="sxs-lookup"><span data-stu-id="d0549-127">[ASP.NET MVC 4](../../../mvc4.md) (included in the Microsoft Visual Studio 2012 installation)</span></span>
- <span data-ttu-id="d0549-128">Emulator Windows Phone (objęte [Windows Phone 7.1.1 SDK](https://www.microsoft.com/en-us/download/details.aspx?id=29233))</span><span class="sxs-lookup"><span data-stu-id="d0549-128">Windows Phone Emulator (included in the [Windows Phone 7.1.1 SDK](https://www.microsoft.com/en-us/download/details.aspx?id=29233))</span></span>
- <span data-ttu-id="d0549-129">Opcjonalne - [programu WebMatrix 2](https://www.microsoft.com/web/webmatrix/) z **iPhone Electric Plum symulatora** (tylko w przypadku wykonywania 3 używane do przeglądania aplikacji sieci web przy użyciu symulatora telefonu iPhone)</span><span class="sxs-lookup"><span data-stu-id="d0549-129">Optional - [WebMatrix 2](https://www.microsoft.com/web/webmatrix/) with **Electric Plum iPhone Simulator** extension (only for Exercise 3 used to browse the web application with an iPhone simulator)</span></span>

<a id="Setup"></a>

<a id="Setup"></a>
### <a name="setup"></a><span data-ttu-id="d0549-130">Konfiguracja</span><span class="sxs-lookup"><span data-stu-id="d0549-130">Setup</span></span>

<span data-ttu-id="d0549-131">W dokumencie laboratorium należy poinstruować do wstawienia bloków kodu.</span><span class="sxs-lookup"><span data-stu-id="d0549-131">Throughout the lab document, you will be instructed to insert code blocks.</span></span> <span data-ttu-id="d0549-132">Dla wygody większość tego kodu jest dostępna w Visual Studio wstawki kodu, którym można z poziomu programu Visual Studio Aby uniknąć konieczności Dodaj ją ręcznie.</span><span class="sxs-lookup"><span data-stu-id="d0549-132">For your convenience, most of that code is provided as Visual Studio Code Snippets, which you can use from within Visual Studio to avoid having to add it manually.</span></span>

<span data-ttu-id="d0549-133">Aby zainstalować wstawki kodu:</span><span class="sxs-lookup"><span data-stu-id="d0549-133">To install the code snippets:</span></span>

1. <span data-ttu-id="d0549-134">Otwórz okno Eksploratora Windows i przejdź do laboratorium **Source\Setup** folderu.</span><span class="sxs-lookup"><span data-stu-id="d0549-134">Open a Windows Explorer window and browse to the lab's **Source\Setup** folder.</span></span>
2. <span data-ttu-id="d0549-135">Kliknij dwukrotnie **Setup.cmd** plik w tym folderze, aby zainstalować wstawki kodu programu Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="d0549-135">Double-click the **Setup.cmd** file in this folder to install the Visual Studio code snippets.</span></span>

<span data-ttu-id="d0549-136">Jeśli nie masz doświadczenia z wstawki programu Visual Studio i chcesz dowiedzieć się, jak ich używać, można odwołać się do dodatku z tego dokumentu &quot; [wstawki kodu za pomocą programu dodatek A:](#AppendixA)&quot;.</span><span class="sxs-lookup"><span data-stu-id="d0549-136">If you are not familiar with the Visual Studio Code Snippets, and want to learn how to use them, you can refer to the appendix from this document &quot;[Appendix A: Using Code Snippets](#AppendixA)&quot;.</span></span>

<a id="Exercises"></a>

<a id="Exercises"></a>
## <a name="exercises"></a><span data-ttu-id="d0549-137">Ćwiczenia</span><span class="sxs-lookup"><span data-stu-id="d0549-137">Exercises</span></span>

<span data-ttu-id="d0549-138">To laboratorium praktycznego obejmuje następujących czynnościach:</span><span class="sxs-lookup"><span data-stu-id="d0549-138">This hands-on lab includes the following exercises:</span></span>

1. [<span data-ttu-id="d0549-139">Nowe szablony projektów programu ASP.NET MVC 4</span><span class="sxs-lookup"><span data-stu-id="d0549-139">New ASP.NET MVC 4 Project Templates</span></span>](#Exercise1)
2. [<span data-ttu-id="d0549-140">Tworzenie aplikacji sieci Web galerii fotografii</span><span class="sxs-lookup"><span data-stu-id="d0549-140">Creating the Photo Gallery Web Application</span></span>](#Exercise2)
3. [<span data-ttu-id="d0549-141">Dodawanie obsługi urządzeń przenośnych</span><span class="sxs-lookup"><span data-stu-id="d0549-141">Adding Support for Mobile Devices</span></span>](#Exercise3)
4. [<span data-ttu-id="d0549-142">Za pomocą kontrolerów asynchroniczne</span><span class="sxs-lookup"><span data-stu-id="d0549-142">Using Asynchronous Controllers</span></span>](#Exercise4)

> [!NOTE]
> <span data-ttu-id="d0549-143">Towarzyszy każdego wykonywania **zakończenia** folderu zawierającego wynikowy rozwiązanie, należy uzyskać po wykonaniu ćwiczeniach.</span><span class="sxs-lookup"><span data-stu-id="d0549-143">Each exercise is accompanied by an **End** folder containing the resulting solution you should obtain after completing the exercises.</span></span> <span data-ttu-id="d0549-144">Jeśli potrzebujesz dodatkowej pomocy pracuje nad ćwiczeniami, można użyć tego rozwiązania jako przewodnika.</span><span class="sxs-lookup"><span data-stu-id="d0549-144">You can use this solution as a guide if you need additional help working through the exercises.</span></span>


<span data-ttu-id="d0549-145">Szacowany czas trwania tego laboratorium: **60 minut**.</span><span class="sxs-lookup"><span data-stu-id="d0549-145">Estimated time to complete this lab: **60 minutes**.</span></span>

<a id="Exercise1"></a>

<a id="Exercise_1_New_ASPNET_MVC_4_Project_Templates"></a>
### <a name="exercise-1-new-aspnet-mvc-4-project-templates"></a><span data-ttu-id="d0549-146">Ćwiczenie 1: Nowe szablony projektów programu ASP.NET MVC 4</span><span class="sxs-lookup"><span data-stu-id="d0549-146">Exercise 1: New ASP.NET MVC 4 Project Templates</span></span>

<span data-ttu-id="d0549-147">W tym ćwiczeniu zostanie Poznaj ulepszenia w szablonach projektu programu ASP.NET MVC 4.</span><span class="sxs-lookup"><span data-stu-id="d0549-147">In this exercise, you will explore the enhancements in the ASP.NET MVC 4 Project templates.</span></span> <span data-ttu-id="d0549-148">Oprócz szablonu aplikacji internetowej znajduje się już w MVC 3, ta wersja teraz obejmuje oddzielne szablonu dla aplikacji dla urządzeń przenośnych.</span><span class="sxs-lookup"><span data-stu-id="d0549-148">In addition to the Internet Application template, already present in MVC 3, this version now includes a separate template for Mobile applications.</span></span> <span data-ttu-id="d0549-149">Po pierwsze będzie wyglądać na niektóre istotne cechy każdego z szablonów.</span><span class="sxs-lookup"><span data-stu-id="d0549-149">First, you will look at some relevant features of each of the templates.</span></span> <span data-ttu-id="d0549-150">Następnie będzie działać na renderowanie strony prawidłowo na różnych platformach przy użyciu podejście.</span><span class="sxs-lookup"><span data-stu-id="d0549-150">Then, you will work on rendering your page properly on the different platforms by using the right approach.</span></span>

<a id="Task_1_-_Exploring_the_Internet_Application_Template"></a>
#### <a name="task-1---exploring-the-internet-application-template"></a><span data-ttu-id="d0549-151">Zadanie 1 - eksploracji szablon aplikacji internetowych</span><span class="sxs-lookup"><span data-stu-id="d0549-151">Task 1 - Exploring the Internet Application Template</span></span>

1. <span data-ttu-id="d0549-152">Otwórz **programu Visual Studio**.</span><span class="sxs-lookup"><span data-stu-id="d0549-152">Open **Visual Studio**.</span></span>
2. <span data-ttu-id="d0549-153">Wybierz **pliku | Nowy | Projekt** polecenia menu.</span><span class="sxs-lookup"><span data-stu-id="d0549-153">Select the **File | New | Project** menu command.</span></span> <span data-ttu-id="d0549-154">W **nowy projekt** okno dialogowe, wybierz opcję **Visual C# | Web** szablonu w lewym okienku drzewa, a następnie wybierz pozycję **aplikacji sieci Web programu ASP.NET MVC 4.**</span><span class="sxs-lookup"><span data-stu-id="d0549-154">In the **New Project** dialog, select the **Visual C# | Web** template on the left pane tree, and choose **ASP.NET MVC 4 Web Application.**</span></span> <span data-ttu-id="d0549-155">Nazwij projekt **PhotoGallery**, wybierz lokalizację (lub pozostaw wartość domyślną) i kliknij przycisk **OK**.</span><span class="sxs-lookup"><span data-stu-id="d0549-155">Name the project **PhotoGallery**, select a location (or leave the default) and click **OK**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="d0549-156">Później będzie można dostosować rozwiązania PhotoGallery ASP.NET MVC 4, które teraz tworzysz.</span><span class="sxs-lookup"><span data-stu-id="d0549-156">You will later customize the PhotoGallery ASP.NET MVC 4 solution you are now creating.</span></span>

    <span data-ttu-id="d0549-157">![Tworzenie nowego projektu](whats-new-in-aspnet-mvc-4/_static/image1.png "tworzenia nowego projektu")</span><span class="sxs-lookup"><span data-stu-id="d0549-157">![Creating a new project](whats-new-in-aspnet-mvc-4/_static/image1.png "Creating a new project")</span></span>

    <span data-ttu-id="d0549-158">*Tworzenie nowego projektu*</span><span class="sxs-lookup"><span data-stu-id="d0549-158">*Creating a new project*</span></span>
3. <span data-ttu-id="d0549-159">W **nowy projekt programu ASP.NET MVC 4** okno dialogowe, wybierz opcję **aplikacji internetowej** szablonu projektu i kliknij przycisk **OK**.</span><span class="sxs-lookup"><span data-stu-id="d0549-159">In the **New ASP.NET MVC 4 Project** dialog, select the **Internet Application** project template and click **OK**.</span></span> <span data-ttu-id="d0549-160">Upewnij się, że został wybrany aparat widoku Razor.</span><span class="sxs-lookup"><span data-stu-id="d0549-160">Make sure you have selected Razor as the view engine.</span></span>

    <span data-ttu-id="d0549-161">![Tworzenie nowej aplikacji internetowych 4 ASP.NET MVC](whats-new-in-aspnet-mvc-4/_static/image2.png "Tworzenie nowej aplikacji internetowych 4 ASP.NET MVC")</span><span class="sxs-lookup"><span data-stu-id="d0549-161">![Creating a new ASP.NET MVC 4 Internet Application](whats-new-in-aspnet-mvc-4/_static/image2.png "Creating a new ASP.NET MVC 4 Internet Application")</span></span>

    <span data-ttu-id="d0549-162">*Tworzenie nowej aplikacji internetowych 4 ASP.NET MVC*</span><span class="sxs-lookup"><span data-stu-id="d0549-162">*Creating a new ASP.NET MVC 4 Internet Application*</span></span>

    > [!NOTE]
    > <span data-ttu-id="d0549-163">Składnia razor została wprowadzona w programie ASP.NET MVC 3.</span><span class="sxs-lookup"><span data-stu-id="d0549-163">Razor syntax has been introduced in ASP.NET MVC 3.</span></span> <span data-ttu-id="d0549-164">Jego celem jest zminimalizowanie liczby znaków i naciśnięcia klawiszy wymagane w pliku, umożliwiające szybkie oraz płynu kodowania przepływu pracy.</span><span class="sxs-lookup"><span data-stu-id="d0549-164">Its goal is to minimize the number of characters and keystrokes required in a file, enabling a fast and fluid coding workflow.</span></span> <span data-ttu-id="d0549-165">Razor wykorzystuje istniejące C# / VB (lub innych) umiejętności język, a następnie dostarcza składnia znacznika szablonu, umożliwiającą świetny przepływu pracy konstrukcji kodu HTML.</span><span class="sxs-lookup"><span data-stu-id="d0549-165">Razor leverages existing C# / VB (or other) language skills and delivers a template markup syntax that enables an awesome HTML construction workflow.</span></span>
4. <span data-ttu-id="d0549-166">Naciśnij klawisz **F5** uruchomić rozwiązanie i zobaczyć odnowionego szablonów.</span><span class="sxs-lookup"><span data-stu-id="d0549-166">Press **F5** to run the solution and see the renewed templates.</span></span> <span data-ttu-id="d0549-167">Można wyewidencjonować następujące funkcje:</span><span class="sxs-lookup"><span data-stu-id="d0549-167">You can check out the following features:</span></span>

    <span data-ttu-id="d0549-168">**Szablonów w stylu Modern**</span><span class="sxs-lookup"><span data-stu-id="d0549-168">**Modern-style templates**</span></span>

    <span data-ttu-id="d0549-169">Szablony odnowienia, zapewniając więcej stylów nowoczesny wygląd.</span><span class="sxs-lookup"><span data-stu-id="d0549-169">The templates have been renewed, providing more modern-looking styles.</span></span>

    <span data-ttu-id="d0549-170">![Szablony ASP.NET MVC 4 restyled](whats-new-in-aspnet-mvc-4/_static/image3.png "MVC 4 restyled szablonów")</span><span class="sxs-lookup"><span data-stu-id="d0549-170">![ASP.NET MVC 4 restyled templates](whats-new-in-aspnet-mvc-4/_static/image3.png "MVC 4 restyled templates")</span></span>

    <span data-ttu-id="d0549-171">*Szablony ASP.NET MVC 4 restyled*</span><span class="sxs-lookup"><span data-stu-id="d0549-171">*ASP.NET MVC 4 restyled templates*</span></span>

    <span data-ttu-id="d0549-172">![Nowa strona Skontaktuj się z](whats-new-in-aspnet-mvc-4/_static/image4.png "nowy kontakt strony")</span><span class="sxs-lookup"><span data-stu-id="d0549-172">![New Contact page](whats-new-in-aspnet-mvc-4/_static/image4.png "New Contact page")</span></span>

    <span data-ttu-id="d0549-173">*Nowa strona kontaktu*</span><span class="sxs-lookup"><span data-stu-id="d0549-173">*New Contact page*</span></span>

    <span data-ttu-id="d0549-174">**Renderowanie adaptacyjną**</span><span class="sxs-lookup"><span data-stu-id="d0549-174">**Adaptive Rendering**</span></span>

    <span data-ttu-id="d0549-175">Zapoznaj się z zmiany rozmiaru okna przeglądarki i zwróć uwagę, jak dynamicznie dostosowuje układ strony do nowego rozmiaru okna.</span><span class="sxs-lookup"><span data-stu-id="d0549-175">Check out resizing the browser window and notice how the page layout dynamically adapts to the new window size.</span></span> <span data-ttu-id="d0549-176">Te szablony używać techniki adaptacyjną renderowania mają być renderowane poprawnie w zarówno wersje desktop i mobile platform bez potrzeby dostosowywania.</span><span class="sxs-lookup"><span data-stu-id="d0549-176">These templates use the adaptive rendering technique to render properly in both desktop and mobile platforms without any customization.</span></span>

    <span data-ttu-id="d0549-177">![Szablon projektu programu ASP.NET MVC 4 w innej przeglądarce rozmiary](whats-new-in-aspnet-mvc-4/_static/image5.png "szablonu projektu programu ASP.NET MVC 4 w innej przeglądarce rozmiarach")</span><span class="sxs-lookup"><span data-stu-id="d0549-177">![ASP.NET MVC 4 project template in different browser sizes](whats-new-in-aspnet-mvc-4/_static/image5.png "ASP.NET MVC 4 project template in different browser sizes")</span></span>

    <span data-ttu-id="d0549-178">*Szablon projektu programu ASP.NET MVC 4 w innej przeglądarce rozmiarach*</span><span class="sxs-lookup"><span data-stu-id="d0549-178">*ASP.NET MVC 4 project template in different browser sizes*</span></span>

    <span data-ttu-id="d0549-179">**Bardziej zaawansowane funkcje interfejsu użytkownika w języku JavaScript**</span><span class="sxs-lookup"><span data-stu-id="d0549-179">**Richer UI with JavaScript**</span></span>

    <span data-ttu-id="d0549-180">Inny rozszerzenie domyślnych szablonów projektu jest użycie JavaScript w celu zapewnienia większej liczby interaktywnych JavaScript.</span><span class="sxs-lookup"><span data-stu-id="d0549-180">Another enhancement to default project templates is the use of JavaScript to provide a more interactive JavaScript.</span></span> <span data-ttu-id="d0549-181">Łącza logowania i rejestrowanie, używane w szablonie spróbujemy sposób użycia jQuery operacji sprawdzania poprawności do sprawdzania poprawności pól wejściowych od klienta.</span><span class="sxs-lookup"><span data-stu-id="d0549-181">The Login and Register links used in the template exemplify how to use the jQuery Validations to validate the input fields from client-side.</span></span>

    ![Sprawdzanie poprawności jQuery](whats-new-in-aspnet-mvc-4/_static/image6.png)

    <span data-ttu-id="d0549-183">*Sprawdzanie poprawności jQuery*</span><span class="sxs-lookup"><span data-stu-id="d0549-183">*jQuery Validation*</span></span>

    > [!NOTE]
    > <span data-ttu-id="d0549-184">Powiadomienie, że dwa Zaloguj się w sekcji, w pierwszej sekcji można zalogować się przy użyciu konta SuperScript z lokacji i w drugiej sekcji można altenativelly logowanie się za pomocą innej usługi uwierzytelniania, takich jak google (domyślnie wyłączone).</span><span class="sxs-lookup"><span data-stu-id="d0549-184">Notice the two log in sections, in the first section you can log in using a registerd account from the site and in the second section you can altenativelly log in using another authentication service like google (disabled by default).</span></span>
5. <span data-ttu-id="d0549-185">Zamknij przeglądarkę, aby zatrzymać debuger i powrócić do programu Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="d0549-185">Close the browser to stop the debugger and return to Visual Studio.</span></span>
6. <span data-ttu-id="d0549-186">Otwórz plik **AuthConfig.cs** znajduje się w obszarze **aplikacji\_Start** folderu.</span><span class="sxs-lookup"><span data-stu-id="d0549-186">Open the file **AuthConfig.cs** located under the **App\_Start** folder.</span></span>
7. <span data-ttu-id="d0549-187">Usuń komentarz z ostatniego wiersza, aby zarejestrować klienta Google *OAuth* uwierzytelniania.</span><span class="sxs-lookup"><span data-stu-id="d0549-187">Remove the comment from the last line to register Google client for *OAuth* authentication.</span></span>


    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample1.cs)]

    > [!NOTE]
    > <span data-ttu-id="d0549-188">Należy zauważyć, że można łatwo włączyć uwierzytelnianie przy użyciu dowolnej usługi OpenID lub OAuth, takich jak Facebook, Twitter, Microsoft itd.</span><span class="sxs-lookup"><span data-stu-id="d0549-188">Notice you can easily enable authentication using any OpenID or OAuth service like Facebook, Twitter, Microsoft, etc.</span></span>
8. <span data-ttu-id="d0549-189">Naciśnij klawisz **F5** Aby uruchomić rozwiązanie i przejdź do strony logowania.</span><span class="sxs-lookup"><span data-stu-id="d0549-189">Press **F5** to run the solution and navigate to the login page.</span></span>
9. <span data-ttu-id="d0549-190">Wybierz **Google** usługi, aby się zalogować.</span><span class="sxs-lookup"><span data-stu-id="d0549-190">Select **Google** service to log in.</span></span>

    ![Wybieranie dziennika w usłudze](whats-new-in-aspnet-mvc-4/_static/image7.png)

    <span data-ttu-id="d0549-192">*Wybieranie dziennika w usłudze*</span><span class="sxs-lookup"><span data-stu-id="d0549-192">*Selecting the log in service*</span></span>
10. <span data-ttu-id="d0549-193">Zaloguj się za pomocą konta Google.</span><span class="sxs-lookup"><span data-stu-id="d0549-193">Log in using your Google account.</span></span>
11. <span data-ttu-id="d0549-194">Zezwalaj na lokacji (localhost) pobrać informacje z konta Google.</span><span class="sxs-lookup"><span data-stu-id="d0549-194">Allow the site (localhost) to retrieve information from Google account.</span></span>
12. <span data-ttu-id="d0549-195">Ponadto należy zarejestrować się w lokacji, aby skojarzyć konto Google.</span><span class="sxs-lookup"><span data-stu-id="d0549-195">Finally, you will have to register in the site to associate the Google account.</span></span>

    ![Skojarz konto Google](whats-new-in-aspnet-mvc-4/_static/image8.png)

    <span data-ttu-id="d0549-197">*Kojarzenie konto Google*</span><span class="sxs-lookup"><span data-stu-id="d0549-197">*Associating your Google account*</span></span>
13. <span data-ttu-id="d0549-198">Zamknij przeglądarkę, aby zatrzymać debuger i powrócić do programu Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="d0549-198">Close the browser to stop the debugger and return to Visual Studio.</span></span>
14. <span data-ttu-id="d0549-199">Teraz Poznaj rozwiązania do wyewidencjonowania niektórych innych nowych funkcji wprowadzonych przez program ASP.NET MVC 4 w szablonie projektu.</span><span class="sxs-lookup"><span data-stu-id="d0549-199">Now explore the solution to check out some other new features introduced by ASP.NET MVC 4 in the project template.</span></span>

    <span data-ttu-id="d0549-200">![Szablon projektu aplikacji internetowych platformy ASP.NET MVC 4](whats-new-in-aspnet-mvc-4/_static/image9.png "szablonu projektu aplikacji internetowych platformy ASP.NET MVC 4")</span><span class="sxs-lookup"><span data-stu-id="d0549-200">![The ASP.NET MVC 4 Internet Application Project Template](whats-new-in-aspnet-mvc-4/_static/image9.png "The ASP.NET MVC 4 Internet Application Project Template")</span></span>

    <span data-ttu-id="d0549-201">*Szablon projektu aplikacji internetowych platformy ASP.NET MVC 4*</span><span class="sxs-lookup"><span data-stu-id="d0549-201">*The ASP.NET MVC 4 Internet Application Project Template*</span></span>

    - <span data-ttu-id="d0549-202">**HTML 5 znaczników**</span><span class="sxs-lookup"><span data-stu-id="d0549-202">**HTML 5 Markup**</span></span>

        <span data-ttu-id="d0549-203">Przeglądaj widoków szablonu, aby dowiedzieć się nowego znacznika motywu.</span><span class="sxs-lookup"><span data-stu-id="d0549-203">Browse template views to find out the new theme markup.</span></span>

        <span data-ttu-id="d0549-204">![Nowy szablon, za pomocą znacznika About.cshtml Razor i HTML5. ] (whats-new-in-aspnet-mvc-4/_static/image10.png "Nowego szablonu, za pomocą znacznika About.cshtml Razor i HTML5.")</span><span class="sxs-lookup"><span data-stu-id="d0549-204">![New template, using Razor and HTML5 markup About.cshtml.](whats-new-in-aspnet-mvc-4/_static/image10.png "New template, using Razor and HTML5 markup About.cshtml.")</span></span>

        <span data-ttu-id="d0549-205">*Nowy szablon, przy użyciu znaczników Razor i HTML5 (About.cshtml).*</span><span class="sxs-lookup"><span data-stu-id="d0549-205">*New template, using Razor and HTML5 markup (About.cshtml).*</span></span>
    - <span data-ttu-id="d0549-206">**Zaktualizowano biblioteki JavaScript**</span><span class="sxs-lookup"><span data-stu-id="d0549-206">**Updated JavaScript libraries**</span></span>

        <span data-ttu-id="d0549-207">Domyślny szablon platformy ASP.NET MVC 4 zawiera teraz elementami KnockoutJS i platforma JavaScript MVVM, która umożliwia tworzenie zaawansowanych aplikacji szybkiego sieci web przy użyciu języka JavaScript i HTML.</span><span class="sxs-lookup"><span data-stu-id="d0549-207">The ASP.NET MVC 4 default template now includes KnockoutJS, a JavaScript MVVM framework that lets you create rich and highly responsive web applications using JavaScript and HTML.</span></span> <span data-ttu-id="d0549-208">Podobnie jak w MVC3, jQuery i jQuery biblioteki interfejsu użytkownika znajdują się również w technologii ASP.NET MVC 4.</span><span class="sxs-lookup"><span data-stu-id="d0549-208">Like in MVC3, jQuery and jQuery UI libraries are also included in ASP.NET MVC 4.</span></span>

        > [!NOTE]
        > <span data-ttu-id="d0549-209">Możesz uzyskać więcej informacji na temat biblioteki elementami KnockOutJS w to łącze: [ [http://learn.knockoutjs.com/](http://learn.knockoutjs.com/)](http://learn.knockoutjs.com/).</span><span class="sxs-lookup"><span data-stu-id="d0549-209">You can get more information about KnockOutJS library in this link: [[http://learn.knockoutjs.com/](http://learn.knockoutjs.com/)](http://learn.knockoutjs.com/).</span></span> <span data-ttu-id="d0549-210">Ponadto informacje na temat jQuery i jQuery interfejsu użytkownika w [ [http://docs.jquery.com/](http://docs.jquery.com/)](http://docs.jquery.com/).</span><span class="sxs-lookup"><span data-stu-id="d0549-210">Additionally, you can learn about jQuery and jQuery UI in [[http://docs.jquery.com/](http://docs.jquery.com/)](http://docs.jquery.com/).</span></span>

<a id="Task_2_-_Exploring_the_Mobile_Application_Template"></a>
#### <a name="task-2---exploring-the-mobile-application-template"></a><span data-ttu-id="d0549-211">Zadanie 2 — poznawanie szablonu aplikacji mobilnej</span><span class="sxs-lookup"><span data-stu-id="d0549-211">Task 2 - Exploring the Mobile Application Template</span></span>

<span data-ttu-id="d0549-212">ASP.NET MVC 4 ułatwia projektowanie witryn sieci Web dla urządzeń przenośnych i przeglądarek typu tablet.</span><span class="sxs-lookup"><span data-stu-id="d0549-212">ASP.NET MVC 4 facilitates the development of websites for mobile and tablet browsers.</span></span> <span data-ttu-id="d0549-213">Ten szablon ma taką samą strukturę aplikacji jako szablonu aplikację internetową (powiadomienie, że kod kontrolera jest praktycznie identyczny), ale jego styl został zmodyfikowany mają być renderowane poprawnie w urządzeń przenośnych z systemem touch.</span><span class="sxs-lookup"><span data-stu-id="d0549-213">This template has the same application structure as the Internet Application template (notice that the controller code is practically identical), but its style was modified to render properly in touch-based mobile devices.</span></span>

1. <span data-ttu-id="d0549-214">Wybierz **pliku | Nowy | Projekt** polecenia menu.</span><span class="sxs-lookup"><span data-stu-id="d0549-214">Select the **File | New | Project** menu command.</span></span> <span data-ttu-id="d0549-215">W **nowy projekt** okno dialogowe, wybierz opcję **Visual C# | Web** szablonu w lewym okienku drzewa, a następnie wybierz pozycję **aplikacji sieci Web programu ASP.NET MVC 4.**</span><span class="sxs-lookup"><span data-stu-id="d0549-215">In the **New Project** dialog, select the **Visual C# | Web** template on the left pane tree, and choose the **ASP.NET MVC 4 Web Application.**</span></span> <span data-ttu-id="d0549-216">Nazwij projekt **PhotoGallery.Mobile**, wybierz lokalizację (lub pozostaw wartość domyślną), wybierz &quot;dodać do rozwiązania&quot; i kliknij przycisk **OK**.</span><span class="sxs-lookup"><span data-stu-id="d0549-216">Name the project **PhotoGallery.Mobile**, select a location (or leave the default), select &quot;Add to solution&quot; and click **OK**.</span></span>
2. <span data-ttu-id="d0549-217">W **nowy projekt programu ASP.NET MVC 4** okno dialogowe, wybierz opcję **aplikacji Mobile** szablonu projektu i kliknij przycisk **OK**.</span><span class="sxs-lookup"><span data-stu-id="d0549-217">In the **New ASP.NET MVC 4 Project** dialog, select the **Mobile Application** project template and click **OK**.</span></span> <span data-ttu-id="d0549-218">Upewnij się, że został wybrany aparat widoku Razor.</span><span class="sxs-lookup"><span data-stu-id="d0549-218">Make sure you have selected Razor as the view engine.</span></span>

    <span data-ttu-id="d0549-219">![Tworzenie nowej aplikacji mobilnej 4 ASP.NET MVC](whats-new-in-aspnet-mvc-4/_static/image11.png "Tworzenie nowej aplikacji mobilnej 4 ASP.NET MVC")</span><span class="sxs-lookup"><span data-stu-id="d0549-219">![Creating a new ASP.NET MVC 4 Mobile Application](whats-new-in-aspnet-mvc-4/_static/image11.png "Creating a new ASP.NET MVC 4 Mobile Application")</span></span>

    <span data-ttu-id="d0549-220">*Tworzenie nowej aplikacji mobilnej 4 ASP.NET MVC*</span><span class="sxs-lookup"><span data-stu-id="d0549-220">*Creating a new ASP.NET MVC 4 Mobile Application*</span></span>
3. <span data-ttu-id="d0549-221">Teraz masz możliwość Eksploruj rozwiązania i zapoznaj się z nowych funkcji wprowadzonych w szablonie rozwiązania ASP.NET MVC 4 dla urządzeń przenośnych:</span><span class="sxs-lookup"><span data-stu-id="d0549-221">Now you are able to explore the solution and check out some of the new features introduced by the ASP.NET MVC 4 solution template for mobile:</span></span>

    - <span data-ttu-id="d0549-222">**jQuery Mobile biblioteki**</span><span class="sxs-lookup"><span data-stu-id="d0549-222">**jQuery Mobile Library**</span></span>

        <span data-ttu-id="d0549-223">Szablon projektu aplikacji mobilnych zawiera przenośnych biblioteki jQuery, która jest biblioteki typu open source dla przenośnych przeglądarkami.</span><span class="sxs-lookup"><span data-stu-id="d0549-223">The Mobile Application project template includes the jQuery Mobile library, which is an open source library for mobile browser compatibility.</span></span> <span data-ttu-id="d0549-224">jQuery Mobile dotyczy stopniowym rozszerzaniu przeglądarek urządzeń przenośnych, które obsługują CSS i JavaScript.</span><span class="sxs-lookup"><span data-stu-id="d0549-224">jQuery Mobile applies progressive enhancement to mobile browsers that support CSS and JavaScript.</span></span> <span data-ttu-id="d0549-225">Stopniowym rozszerzaniu włącza wszystkie przeglądarki wyświetlić podstawowe elementy strony sieci web podczas tylko umożliwia korzystanie z najbardziej zaawansowanych przeglądarki do wyświetlania zawartości sformatowanego.</span><span class="sxs-lookup"><span data-stu-id="d0549-225">Progressive enhancement enables all browsers to display the basic content of a web page, while it only enables the most powerful browsers to display the rich content.</span></span> <span data-ttu-id="d0549-226">Pliki obsługi języka JavaScript i CSS, uwzględnione w jQuery przenośnych styl pomocy przeglądarek urządzeń przenośnych w celu dopasowania do zawartości na ekranie bez wprowadzania żadnych zmian w znaczniku strony.</span><span class="sxs-lookup"><span data-stu-id="d0549-226">The JavaScript and CSS files, included in the jQuery Mobile style, help mobile browsers to fit the content in the screen without making any change in the page markup.</span></span>

        ![jQuery-mobile-library-included-in-the-template](whats-new-in-aspnet-mvc-4/_static/image12.png)

        <span data-ttu-id="d0549-228">*Biblioteka przenośnych jQuery zawarte w szablonie*</span><span class="sxs-lookup"><span data-stu-id="d0549-228">*jQuery mobile library included in the template*</span></span>
    - <span data-ttu-id="d0549-229">**HTML5 na podstawie znaczników**</span><span class="sxs-lookup"><span data-stu-id="d0549-229">**HTML5 based markup**</span></span>

        ![Mobile-application-template-using-HTML5-markup](whats-new-in-aspnet-mvc-4/_static/image13.png)

        <span data-ttu-id="d0549-231">*Szablon aplikacji mobilnej przy użyciu znaczników HTML5, (Login.cshtml i index.cshtml)*</span><span class="sxs-lookup"><span data-stu-id="d0549-231">*Mobile application template using HTML5 markup, (Login.cshtml and index.cshtml)*</span></span>
4. <span data-ttu-id="d0549-232">Naciśnij klawisz **F5** Aby uruchomić rozwiązanie.</span><span class="sxs-lookup"><span data-stu-id="d0549-232">Press **F5** to run the solution.</span></span>
5. <span data-ttu-id="d0549-233">Otwórz **Windows Phone 7 Emulator**.</span><span class="sxs-lookup"><span data-stu-id="d0549-233">Open the **Windows Phone 7 Emulator**.</span></span>
6. <span data-ttu-id="d0549-234">Na ekranie startowym telefonu Otwórz program Internet Explorer.</span><span class="sxs-lookup"><span data-stu-id="d0549-234">In the phone start screen, open Internet Explorer.</span></span> <span data-ttu-id="d0549-235">Zapoznaj się z adresu URL, gdzie uruchomić aplikacji klasycznych i przejść do tego adresu URL z telefonu (np. `http://localhost:[PortNumber]/`).</span><span class="sxs-lookup"><span data-stu-id="d0549-235">Check out the URL where the desktop application started and browse to that URL from the phone (e.g. `http://localhost:[PortNumber]/`).</span></span>
7. <span data-ttu-id="d0549-236">Teraz możesz wprowadzić strony logowania lub zapoznaj się z o stronie.</span><span class="sxs-lookup"><span data-stu-id="d0549-236">Now you are able to enter the login page or check out the about page.</span></span> <span data-ttu-id="d0549-237">Należy zauważyć, że styl witryny sieci Web jest oparty na nową aplikację Metro dla urządzeń przenośnych.</span><span class="sxs-lookup"><span data-stu-id="d0549-237">Notice that the style of the website is based on the new Metro app for mobile.</span></span> <span data-ttu-id="d0549-238">Szablon projektu programu ASP.NET MVC 4 jest prawidłowo wyświetlany na urządzeniach przenośnych, upewniając się, że wszystkie elementy na stronie są widoczne i włączone.</span><span class="sxs-lookup"><span data-stu-id="d0549-238">The ASP.NET MVC 4 project template is correctly displayed on mobile devices, making sure all the elements of the page are visible and enabled.</span></span> <span data-ttu-id="d0549-239">Zwróć uwagę, czy wystarczająco duży, aby być kliknięte lub dotknięciu łącza w nagłówku.</span><span class="sxs-lookup"><span data-stu-id="d0549-239">Notice that the links on the header are big enough to be clicked or tapped.</span></span>

    <span data-ttu-id="d0549-240">![Projekt strony szablonu w urządzeniu przenośnym](whats-new-in-aspnet-mvc-4/_static/image14.png "projektu szablonu stron w urządzeniu przenośnym")</span><span class="sxs-lookup"><span data-stu-id="d0549-240">![Project template pages in a mobile device](whats-new-in-aspnet-mvc-4/_static/image14.png "Project template pages in a mobile device")</span></span>

    <span data-ttu-id="d0549-241">*Strony szablonu projektu w urządzeniu przenośnym*</span><span class="sxs-lookup"><span data-stu-id="d0549-241">*Project template pages in a mobile device*</span></span>
8. <span data-ttu-id="d0549-242">Nowy szablon używa również **okienka ekranu metatag**.</span><span class="sxs-lookup"><span data-stu-id="d0549-242">The new template also uses the **Viewport meta tag**.</span></span> <span data-ttu-id="d0549-243">Szerokość okna przeglądarki wirtualnego zdefiniuj przeglądarki dla większości urządzeń przenośnych lub &quot;okienka ekranu&quot;, które jest większe niż rzeczywista szerokość urządzenia przenośnego.</span><span class="sxs-lookup"><span data-stu-id="d0549-243">Most mobile browsers define a width for a virtual browser window or &quot;viewport&quot;, which is larger than the actual width of the mobile device.</span></span> <span data-ttu-id="d0549-244">Dzięki temu przeglądarek urządzeń przenośnych wyświetlić całą stronę sieci web wewnątrz wirtualnych wyświetlania.</span><span class="sxs-lookup"><span data-stu-id="d0549-244">This enables mobile browsers to display the entire web page inside the virtual display.</span></span> <span data-ttu-id="d0549-245">**Okienka ekranu metatag** umożliwia deweloperom sieci web ustawianie szerokości, wysokości i skali obszaru przeglądarki na urządzeniach przenośnych **.**</span><span class="sxs-lookup"><span data-stu-id="d0549-245">The **Viewport meta tag** allows web developers to set the width, height and scale of the browser area on mobile devices **.**</span></span> <span data-ttu-id="d0549-246">Szablonu platformy ASP.NET MVC 4 dla aplikacji mobilnych Ustawia szerokość urządzenia okienka ekranu (&quot;szerokość = szerokość urządzenia&quot;) w szablonie układu (*Views\Shared\_Layout.cshtml*), dzięki czemu wszystkie strony mają ich okienka ekranu, Ustaw szerokość ekranu urządzenia.</span><span class="sxs-lookup"><span data-stu-id="d0549-246">The ASP.NET MVC 4 template for Mobile Applications sets the viewport to the device width (&quot;width=device-width&quot;) in the layout template (*Views\Shared\_Layout.cshtml*), so that all the pages will have their viewport set to the device screen width.</span></span> <span data-ttu-id="d0549-247">Zwróć uwagę, metatag okienka ekranu nie zmieni domyślny widok przeglądarki.</span><span class="sxs-lookup"><span data-stu-id="d0549-247">Notice that the Viewport meta tag will not change the default browser view.</span></span>
9. <span data-ttu-id="d0549-248">Otwórz  **\_Layout.cshtml**, który znajduje się w **widoków | Udostępnione** folder i komentarza metatag okienka ekranu.</span><span class="sxs-lookup"><span data-stu-id="d0549-248">Open **\_Layout.cshtml**, located in the **Views | Shared** folder, and comment the Viewport meta tag.</span></span> <span data-ttu-id="d0549-249">Uruchom aplikację, jeśli nie już otwarty i zapoznaj się z różnic.</span><span class="sxs-lookup"><span data-stu-id="d0549-249">Run the application, if not already opened, and check out the differences.</span></span>


    [!code-cshtml[Main](whats-new-in-aspnet-mvc-4/samples/sample2.cshtml)]

    <span data-ttu-id="d0549-250">![Lokacji po komentowania metatag okienka ekranu](whats-new-in-aspnet-mvc-4/_static/image15.png "lokacji po komentowania metatag okienka ekranu")</span><span class="sxs-lookup"><span data-stu-id="d0549-250">![The site after commenting the viewport meta tag](whats-new-in-aspnet-mvc-4/_static/image15.png "The site after commenting the viewport meta tag")</span></span>

    <span data-ttu-id="d0549-251">*Lokacji po komentowania metatag okienka ekranu*</span><span class="sxs-lookup"><span data-stu-id="d0549-251">*The site after commenting the viewport meta tag*</span></span>
10. <span data-ttu-id="d0549-252">W programie Visual Studio, naciśnij klawisz **SHIFT** + **F5** można zatrzymać debugowania aplikacji.</span><span class="sxs-lookup"><span data-stu-id="d0549-252">In Visual Studio, press **SHIFT** + **F5** to stop debugging the application.</span></span>
11. <span data-ttu-id="d0549-253">Usuń znaczniki komentarza metatag okienka ekranu.</span><span class="sxs-lookup"><span data-stu-id="d0549-253">Uncomment the viewport meta tag.</span></span>


    [!code-cshtml[Main](whats-new-in-aspnet-mvc-4/samples/sample3.cshtml)]

<a id="Task_3_-_Using_Adaptive_Rendering"></a>
#### <a name="task-3---using-adaptive-rendering"></a><span data-ttu-id="d0549-254">Zadanie 3 - adaptacyjnych renderowaniem</span><span class="sxs-lookup"><span data-stu-id="d0549-254">Task 3 - Using Adaptive Rendering</span></span>

<span data-ttu-id="d0549-255">W tym zadaniu dowiesz się innej metody do renderowania strony sieci Web prawidłowo na urządzeniach przenośnych i przeglądarki sieci Web w tym samym czasie, bez potrzeby dostosowywania.</span><span class="sxs-lookup"><span data-stu-id="d0549-255">In this task, you will learn another method to render a Web page correctly on mobile devices and Web browsers at the same time without any customization.</span></span> <span data-ttu-id="d0549-256">Używano już okienka ekranu metatag o podobnych celów.</span><span class="sxs-lookup"><span data-stu-id="d0549-256">You have already used Viewport meta tag with a similar purpose.</span></span> <span data-ttu-id="d0549-257">Teraz będzie spełniać innej metody zaawansowane: *adaptacyjną renderowania*.</span><span class="sxs-lookup"><span data-stu-id="d0549-257">Now you will meet another powerful method: *adaptive rendering*.</span></span>

<span data-ttu-id="d0549-258">Renderowanie adaptacyjną jest technika, która używa **zapytaniami multimediów CSS3** Aby dostosować styl stosowany do strony.</span><span class="sxs-lookup"><span data-stu-id="d0549-258">Adaptive rendering is a technique that uses **CSS3 media queries** to customize the style applied to a page.</span></span> <span data-ttu-id="d0549-259">Zapytaniami multimediów Zdefiniuj warunki wewnątrz arkusza stylów, grupowanie stylów CSS w obszarze określonego warunku.</span><span class="sxs-lookup"><span data-stu-id="d0549-259">Media queries define conditions inside a style sheet, grouping CSS styles under a certain condition.</span></span> <span data-ttu-id="d0549-260">Tylko wtedy, gdy warunek jest spełniony, styl jest stosowany do obiektów zadeklarowane.</span><span class="sxs-lookup"><span data-stu-id="d0549-260">Only when the condition is true, the style is applied to the declared objects.</span></span>

<span data-ttu-id="d0549-261">Elastyczności techniką adaptacyjną renderowania umożliwia dostosowywanie wyświetlania lokacji na różnych urządzeniach.</span><span class="sxs-lookup"><span data-stu-id="d0549-261">The flexibility provided by the adaptive rendering technique enables any customization for displaying the site on different devices.</span></span> <span data-ttu-id="d0549-262">Można określić dowolną liczbę style, w których chcesz na arkusz stylów pojedynczego bez pisania kodu logiki wybierz styl.</span><span class="sxs-lookup"><span data-stu-id="d0549-262">You can define as many styles as you want on a single style sheet without writing logic code to choose the style.</span></span> <span data-ttu-id="d0549-263">W związku z tym jest bardzo przejrzysty sposób dostosowania strony style, ponieważ zmniejsza ilość zduplikowanych kodu i logikę do celów renderowania.</span><span class="sxs-lookup"><span data-stu-id="d0549-263">Therefore, it is a very neat way of adapting page styles, as it reduces the amount of duplicated code and logic for rendering purposes.</span></span> <span data-ttu-id="d0549-264">Z drugiej strony zwiększyć będą zużycie przepustowości, wzrostem rozmiaru plików CSS mogą się nieznacznie.</span><span class="sxs-lookup"><span data-stu-id="d0549-264">On the other hand, bandwidth consumption would increase, as the size of your CSS files could grow marginally.</span></span>

<span data-ttu-id="d0549-265">Przy użyciu techniki adaptacyjną renderowania, zostanie wyświetlona witryna **wyświetlane prawidłowo, niezależnie od przeglądarki.**</span><span class="sxs-lookup"><span data-stu-id="d0549-265">By using the adaptive rendering technique, your site will be **displayed properly, regardless of the browser.**</span></span> <span data-ttu-id="d0549-266">Jednak należy rozważyć, jest istotny, jeśli przepustowość dodatkowego obciążenia.</span><span class="sxs-lookup"><span data-stu-id="d0549-266">However, you should consider if the bandwidth extra load is a concern.</span></span>

> [!NOTE]
> <span data-ttu-id="d0549-267">Jest podstawowy format zapytanie o multimedia: @media \[zakres: wszystkie | handheld | wydruku | projekcji | ekranu\] ([właściwość: wartość] i... [właściwość: wartość])</span><span class="sxs-lookup"><span data-stu-id="d0549-267">The basic format of a media query is: @media \[Scope: all | handheld | print | projection | screen\] ([property:value] and ... [property:value])</span></span>


<span data-ttu-id="d0549-268">Przykłady z zapytaniami multimediów: &gt;  **@media wszystkich i (szerokość maksymalna: 1000px) i (szerokość minimalna: 700px) {}:** dla wszystkich rozdzielczości między 700px i 1000px.</span><span class="sxs-lookup"><span data-stu-id="d0549-268">Examples of media queries: &gt;**@media all and (max-width: 1000px) and (min-width: 700px) {}:** For all the resolutions between 700px and 1000px.</span></span>

> <span data-ttu-id="d0549-269">**@mediaekran i (szerokość minimalna: 400 piks.) i (szerokość maksymalna: 700px) {...}:** tylko na ekranach.</span><span class="sxs-lookup"><span data-stu-id="d0549-269">**@media screen and (min-width: 400px) and (max-width: 700px) { ... }:** Only for screens.</span></span> <span data-ttu-id="d0549-270">Rozdzielczość musi wynosić od 400 do 700px.</span><span class="sxs-lookup"><span data-stu-id="d0549-270">The resolution must be between 400 and 700px.</span></span>
> 
> <span data-ttu-id="d0549-271">**@mediaurządzenia przenośnego i (minimalną szerokość: 20em), ekranu i (szerokość minimalna: 20em) {...}:** urządzenia podręczne (przenośne i urządzenia) i ekranów.</span><span class="sxs-lookup"><span data-stu-id="d0549-271">**@media handheld and (min-width: 20em), screen and (min-width: 20em) { ... }:** For handhelds(mobile and devices) and screens.</span></span> <span data-ttu-id="d0549-272">Minimalna szerokość musi być większa niż 20em.</span><span class="sxs-lookup"><span data-stu-id="d0549-272">The minimum width must be greater than 20em.</span></span>
> 
> <span data-ttu-id="d0549-273">Więcej informacji na ten temat można znaleźć na [lokacji W3C](http://www.w3.org/TR/css3-mediaqueries/).</span><span class="sxs-lookup"><span data-stu-id="d0549-273">You can find more information about this on the [W3C site](http://www.w3.org/TR/css3-mediaqueries/).</span></span>


<span data-ttu-id="d0549-274">Może teraz zapoznać, jak działa adaptacyjną renderowania, poprawa czytelności platformy ASP.NET MVC 4 domyślny szablon witryny sieci Web.</span><span class="sxs-lookup"><span data-stu-id="d0549-274">You will now explore how the adaptive rendering works, improving the readability of the ASP.NET MVC 4 default website template.</span></span>

1. <span data-ttu-id="d0549-275">Otwórz **PhotoGallery.sln** rozwiązania, które zostały utworzone w zadaniu 1 i wybierz **PhotoGallery** projektu.</span><span class="sxs-lookup"><span data-stu-id="d0549-275">Open the **PhotoGallery.sln** solution you have created at Task 1 and select the **PhotoGallery** project.</span></span> <span data-ttu-id="d0549-276">Naciśnij klawisz **F5** Aby uruchomić rozwiązanie.</span><span class="sxs-lookup"><span data-stu-id="d0549-276">Press **F5** to run the solution.</span></span>
2. <span data-ttu-id="d0549-277">Zmienia szerokość w przeglądarce, ustawienia systemu windows do połowę lub mniej niż kwartału oryginalnego rozmiaru.</span><span class="sxs-lookup"><span data-stu-id="d0549-277">Resize the browser's width, setting the windows to half or to less than a quarter of its original size.</span></span> <span data-ttu-id="d0549-278">Zwróć uwagę, co się dzieje z elementami w nagłówku: niektóre elementy nie będą widoczne w widocznym obszarze nagłówka.</span><span class="sxs-lookup"><span data-stu-id="d0549-278">Notice what happens with the items in the header: Some elements will not appear in the visible area of the header.</span></span>
3. <span data-ttu-id="d0549-279">Otwórz **Site.css** plików w Eksploratorze rozwiązania Visual Studio, znajduje się w **zawartości** folderu projektu.</span><span class="sxs-lookup"><span data-stu-id="d0549-279">Open **Site.css** file from the Visual Studio Solution explorer, located in **Content** project folder.</span></span> <span data-ttu-id="d0549-280">Naciśnij klawisz **CTRL + F** Otwórz wyszukiwanie zintegrowane Visual Studio i zapisać  **@media**  zlokalizować **zapytanie o multimedia CSS**.</span><span class="sxs-lookup"><span data-stu-id="d0549-280">Press **CTRL + F** to open Visual Studio integrated search, and write **@media** to locate the **CSS media query**.</span></span>

    <span data-ttu-id="d0549-281">Warunek kwerendy nośnika zdefiniowane w tym szablonie działa w ten sposób: gdy rozmiar okna przeglądarki jest poniżej **850 pikseli**, reguły CSS, stosowane są tymi zdefiniowany w tym bloku nośnika.</span><span class="sxs-lookup"><span data-stu-id="d0549-281">The media query condition defined in this template works in this way: When the browser's window size is below **850 px**, the CSS rules applied are the ones defined inside this media block.</span></span>

    <span data-ttu-id="d0549-282">![Lokalizowanie zapytanie o multimedia](whats-new-in-aspnet-mvc-4/_static/image16.png "lokalizowanie zapytanie o multimedia")</span><span class="sxs-lookup"><span data-stu-id="d0549-282">![Locating the media query](whats-new-in-aspnet-mvc-4/_static/image16.png "Locating the media query")</span></span>

    <span data-ttu-id="d0549-283">*Lokalizowanie zapytanie o multimedia*</span><span class="sxs-lookup"><span data-stu-id="d0549-283">*Locating the media query*</span></span>
4. <span data-ttu-id="d0549-284">Zastąp wartość atrybutu maksymalna szerokość 850 pikseli z **10px**, aby wyłączyć adaptacyjną renderowania i naciśnij klawisz **CTRL + S** Aby zapisać zmiany.</span><span class="sxs-lookup"><span data-stu-id="d0549-284">Replace the max-width attribute value set in 850 px with **10px**, in order to disable the adaptive rendering, and press **CTRL + S** to save the changes.</span></span> <span data-ttu-id="d0549-285">Wróć do przeglądarki i naciśnij klawisz **CTRL + F5** odświeżenie strony z wprowadzone zmiany.</span><span class="sxs-lookup"><span data-stu-id="d0549-285">Return to the browser and press **CTRL + F5** to refresh the page with the changes you have made.</span></span> <span data-ttu-id="d0549-286">Zwróć uwagę różnice w obu stron, zmieniając szerokości okna.</span><span class="sxs-lookup"><span data-stu-id="d0549-286">Notice the differences in both pages when adjusting the width of the window.</span></span>

    <span data-ttu-id="d0549-287">![W lewej strony jest stosowania @media pominięcia stylu w prawo, styl](whats-new-in-aspnet-mvc-4/_static/image17.png "w lewej strony jest stosowania @media stylu w prawo, styl zostanie pominięty")</span><span class="sxs-lookup"><span data-stu-id="d0549-287">![In the left, the page is applying the @media style, in the right, the style is omitted](whats-new-in-aspnet-mvc-4/_static/image17.png "In the left, the page is applying the @media style, in the right, the style is omitted")</span></span>

    <span data-ttu-id="d0549-288">*W lewej strony jest stosowania @media stylu w prawo, styl zostanie pominięty.*</span><span class="sxs-lookup"><span data-stu-id="d0549-288">*In the left, the page is applying the @media style, in the right, the style is omitted*</span></span>

    <span data-ttu-id="d0549-289">Teraz załóżmy wyewidencjonować co się dzieje na urządzeniach przenośnych:</span><span class="sxs-lookup"><span data-stu-id="d0549-289">Now, let's check out what happens on mobile devices:</span></span>

    <span data-ttu-id="d0549-290">![W lewej strony jest stosowania @media pominięcia stylu w prawo, styl](whats-new-in-aspnet-mvc-4/_static/image18.png "w lewej strony jest stosowania @media stylu w prawo, styl zostanie pominięty")</span><span class="sxs-lookup"><span data-stu-id="d0549-290">![In the left, the page is applying the @media style, in the right, the style is omitted](whats-new-in-aspnet-mvc-4/_static/image18.png "In the left, the page is applying the @media style, in the right, the style is omitted")</span></span>

    <span data-ttu-id="d0549-291">*W lewej strony jest stosowania @media stylu w prawo, styl zostanie pominięty.*</span><span class="sxs-lookup"><span data-stu-id="d0549-291">*In the left, the page is applying the @media style, in the right, the style is omitted*</span></span>

    <span data-ttu-id="d0549-292">Mimo że można zauważyć, że zmiany podczas renderowania strony w przeglądarce sieci Web nie są bardzo istotne, gdy na urządzeniu przenośnym, różnice staną się bardziej oczywistymi.</span><span class="sxs-lookup"><span data-stu-id="d0549-292">Although you will notice that the changes when the page is rendered in a Web browser are not very significant, when using a mobile device the differences become more obvious.</span></span> <span data-ttu-id="d0549-293">Po lewej stronie obrazu możemy stwierdzić, czy styl niestandardowy poprawia czytelność.</span><span class="sxs-lookup"><span data-stu-id="d0549-293">On the left side of the image, we can see that the custom style improved the readability.</span></span>

    <span data-ttu-id="d0549-294">W wielu scenariuszach więcej, co ułatwia zastosować warunkowego stylów do witryny sieci Web i rozwiązywania typowych problemów styl porządek podejścia przy rozwiązywaniu służy adaptacyjną renderowania.</span><span class="sxs-lookup"><span data-stu-id="d0549-294">Adaptive rendering can be used in many more scenarios, making it easier to apply conditional styling to a Web site and solving common style issues with a neat approach.</span></span>

    <span data-ttu-id="d0549-295">Tag meta okienka ekranu i zapytaniami multimediów CSS nie są specyficzne dla platformy ASP.NET MVC 4, dzięki czemu można korzystać z tych funkcji w dowolnej aplikacji sieci web.</span><span class="sxs-lookup"><span data-stu-id="d0549-295">The Viewport meta tag and CSS media queries are not specific to ASP.NET MVC 4, so you can take advantage of these features in any web application.</span></span>
5. <span data-ttu-id="d0549-296">W programie Visual Studio, naciśnij klawisz **SHIFT** + **F5** można zatrzymać debugowania aplikacji.</span><span class="sxs-lookup"><span data-stu-id="d0549-296">In Visual Studio, press **SHIFT** + **F5** to stop debugging the application.</span></span>

<a id="Exercise2"></a>

<a id="Exercise_2_Creating_the_Photo_Gallery_Web_Application"></a>
### <a name="exercise-2-creating-the-photo-gallery-web-application"></a><span data-ttu-id="d0549-297">Ćwiczenie 2: Tworzenie aplikacji sieci Web galerii fotografii</span><span class="sxs-lookup"><span data-stu-id="d0549-297">Exercise 2: Creating the Photo Gallery Web Application</span></span>

<span data-ttu-id="d0549-298">W tym ćwiczeniu będzie działać w Galerii fotografii aplikacji do wyświetlenia zdjęcia.</span><span class="sxs-lookup"><span data-stu-id="d0549-298">In this exercise, you will work on a Photo Gallery application to display photos.</span></span> <span data-ttu-id="d0549-299">Rozpocznie się przy użyciu szablonu projektu programu ASP.NET MVC 4, a następnie doda funkcji można pobrać zdjęcia z usługą i wyświetlać na stronie głównej.</span><span class="sxs-lookup"><span data-stu-id="d0549-299">You will start with the ASP.NET MVC 4 project template, and then you will add a feature to retrieve photos from a service and display them in the home page.</span></span>

<span data-ttu-id="d0549-300">W poniższym ćwiczeniu zostanie zaktualizowana to rozwiązanie w celu zwiększenia sposób wyświetlania na urządzeniach przenośnych.</span><span class="sxs-lookup"><span data-stu-id="d0549-300">In the following exercise, you will update this solution to enhance the way it is displayed on mobile devices.</span></span>

<a id="Task_1_-_Creating_a_Mock_Photo_Service"></a>
#### <a name="task-1---creating-a-mock-photo-service"></a><span data-ttu-id="d0549-301">Zadanie 1 — Tworzenie usługi zasymulować zdjęcia</span><span class="sxs-lookup"><span data-stu-id="d0549-301">Task 1 - Creating a Mock Photo Service</span></span>

<span data-ttu-id="d0549-302">W ramach tego zadania spowoduje utworzenie makiety usługi fotografii można pobrać zawartości, który będzie wyświetlany w galerii.</span><span class="sxs-lookup"><span data-stu-id="d0549-302">In this task, you will create a mock of the photo service to retrieve the content that will be displayed in the gallery.</span></span> <span data-ttu-id="d0549-303">Aby to zrobić, zostaną dodane nowego kontrolera, która po prostu zwróci pliku JSON z danymi każdej fotografii.</span><span class="sxs-lookup"><span data-stu-id="d0549-303">To do this, you will add a new controller that will simply return a JSON file with the data of each photo.</span></span>

1. <span data-ttu-id="d0549-304">Otwórz **programu Visual Studio** Jeśli nie jest jeszcze otwarty.</span><span class="sxs-lookup"><span data-stu-id="d0549-304">Open **Visual Studio** if not already opened.</span></span>
2. <span data-ttu-id="d0549-305">Wybierz **pliku | Nowy | Projekt** polecenia menu.</span><span class="sxs-lookup"><span data-stu-id="d0549-305">Select the **File | New | Project** menu command.</span></span> <span data-ttu-id="d0549-306">W **nowy projekt** okno dialogowe, wybierz opcję **Visual C# | Web** szablonu w lewym okienku drzewa, a następnie wybierz pozycję **aplikacji sieci Web programu ASP.NET MVC 4.**</span><span class="sxs-lookup"><span data-stu-id="d0549-306">In the **New Project** dialog, select the **Visual C# | Web** template on the left pane tree, and choose **ASP.NET MVC 4 Web Application.**</span></span> <span data-ttu-id="d0549-307">Nazwij projekt **PhotoGallery**, wybierz lokalizację (lub pozostaw wartość domyślną) i kliknij przycisk **OK**.</span><span class="sxs-lookup"><span data-stu-id="d0549-307">Name the project **PhotoGallery**, select a location (or leave the default) and click **OK**.</span></span> <span data-ttu-id="d0549-308">Alternatywnie można kontynuować pracę z istniejących MVC 4 ASP.NET **aplikacji internetowej** rozwiązania z **ćwiczenie 1** i przejść do następnego kroku.</span><span class="sxs-lookup"><span data-stu-id="d0549-308">Alternatively, you can continue working from your existing ASP.NET MVC 4 **Internet Application** solution from **Exercise 1** and skip the next step.</span></span>
3. <span data-ttu-id="d0549-309">W **nowy projekt programu ASP.NET MVC 4** okno dialogowe, wybierz opcję **aplikacji internetowej** szablonu projektu i kliknij przycisk **OK**.</span><span class="sxs-lookup"><span data-stu-id="d0549-309">In the **New ASP.NET MVC 4 Project** dialog box, select the **Internet Application** project template and click **OK**.</span></span> <span data-ttu-id="d0549-310">Upewnij się, że masz wybrany jako aparat widoku Razor.</span><span class="sxs-lookup"><span data-stu-id="d0549-310">Make sure you have Razor selected as the View Engine.</span></span>
4. <span data-ttu-id="d0549-311">W **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy **aplikacji\_danych** folderu projektu i wybierz **Dodaj | Istniejący element**.</span><span class="sxs-lookup"><span data-stu-id="d0549-311">In the **Solution Explorer**, right-click the **App\_Data** folder of your project, and select **Add | Existing Item**.</span></span> <span data-ttu-id="d0549-312">Przejdź do **Source\Assets\App\_danych** folder tego laboratorium i Dodaj **Photos.json** pliku.</span><span class="sxs-lookup"><span data-stu-id="d0549-312">Browse to the **Source\Assets\App\_Data** folder of this lab and add the **Photos.json** file.</span></span>
5. <span data-ttu-id="d0549-313">Tworzenie nowego kontrolera o nazwie **PhotoController**.</span><span class="sxs-lookup"><span data-stu-id="d0549-313">Create a new controller with the name **PhotoController**.</span></span> <span data-ttu-id="d0549-314">Aby to zrobić, kliknij prawym przyciskiem myszy **kontrolerów** folderu, przejdź do **Dodaj** i wybierz **kontrolera.**</span><span class="sxs-lookup"><span data-stu-id="d0549-314">To do this, right-click on the **Controllers** folder, go to **Add** and select **Controller.**</span></span> <span data-ttu-id="d0549-315">Pełna nazwa kontrolera, pozostaw **kontroler MVC pusty** szablon i kliknij przycisk **Dodaj**.</span><span class="sxs-lookup"><span data-stu-id="d0549-315">Complete the controller name, leave the **Empty MVC controller** template and click **Add**.</span></span>

    <span data-ttu-id="d0549-316">![Dodawanie PhotoController](whats-new-in-aspnet-mvc-4/_static/image19.png "Dodawanie PhotoController")</span><span class="sxs-lookup"><span data-stu-id="d0549-316">![Adding the PhotoController](whats-new-in-aspnet-mvc-4/_static/image19.png "Adding the PhotoController")</span></span>

    <span data-ttu-id="d0549-317">*Dodawanie PhotoController*</span><span class="sxs-lookup"><span data-stu-id="d0549-317">*Adding the PhotoController*</span></span>
6. <span data-ttu-id="d0549-318">Zastąp **indeksu** metodę z następującym **galerii** akcji i powrocie zawartość z pliku JSON ostatnio dodane do projektu.</span><span class="sxs-lookup"><span data-stu-id="d0549-318">Replace the **Index** method with the following **Gallery** action, and return the content from the JSON file you have recently added to the project.</span></span>

    <span data-ttu-id="d0549-319">(Fragment - kodu *platformy ASP.NET MVC 4 - Ex02 - laboratorium galerii akcji*)</span><span class="sxs-lookup"><span data-stu-id="d0549-319">(Code Snippet - *ASP.NET MVC 4 Lab - Ex02 - Gallery Action*)</span></span>


    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample4.cs)]
7. <span data-ttu-id="d0549-320">Naciśnij klawisz **F5** Aby uruchomić rozwiązanie, a następnie przejdź do następującego adresu URL w celu przetestowania usługi mocked zdjęcie: `http://localhost:[port]/photo/gallery` (wartość [port] zależy od bieżącego portu, na którym aplikacja została uruchomiona).</span><span class="sxs-lookup"><span data-stu-id="d0549-320">Press **F5** to run the solution, and then browse to the following URL in order to test the mocked photo service: `http://localhost:[port]/photo/gallery` (the [port] value depends on the current port where the application was launched).</span></span> <span data-ttu-id="d0549-321">Żądanie do tego adresu URL powinien pobierać zawartość z **Photos.json** pliku.</span><span class="sxs-lookup"><span data-stu-id="d0549-321">The request to this URL should retrieve the content of the **Photos.json** file.</span></span>

    <span data-ttu-id="d0549-322">![Testowanie usługi fotografii mocked](whats-new-in-aspnet-mvc-4/_static/image20.png "testowania usługi mocked zdjęcia")</span><span class="sxs-lookup"><span data-stu-id="d0549-322">![Testing the mocked photo service](whats-new-in-aspnet-mvc-4/_static/image20.png "Testing the mocked photo service")</span></span>

    <span data-ttu-id="d0549-323">*Testowanie usługi mocked zdjęcia*</span><span class="sxs-lookup"><span data-stu-id="d0549-323">*Testing the mocked photo service*</span></span>

<span data-ttu-id="d0549-324">W implementacji rzeczywistych można użyć [ASP.NET Web API](../../../../web-api/index.md) do implementacji usługi galerii fotografii.</span><span class="sxs-lookup"><span data-stu-id="d0549-324">In a real implementation you could use [ASP.NET Web API](../../../../web-api/index.md) to implement the Photo Gallery service.</span></span> <span data-ttu-id="d0549-325">Interfejs API sieci Web platformy ASP.NET to platforma, która ułatwia tworzenie usług HTTP, które są używane przez szeroki wachlarz klientów, w tym przeglądarki i urządzenia przenośne.</span><span class="sxs-lookup"><span data-stu-id="d0549-325">ASP.NET Web API is a framework that makes it easy to build HTTP services that reach a broad range of clients, including browsers and mobile devices.</span></span> <span data-ttu-id="d0549-326">Interfejs API sieci Web ASP.NET jest idealną platformą do tworzenia RESTful aplikacji w programie .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="d0549-326">ASP.NET Web API is an ideal platform for building RESTful applications on the .NET Framework.</span></span>

<a id="Task_2_-_Displaying_the_Photo_Gallery"></a>
#### <a name="task-2---displaying-the-photo-gallery"></a><span data-ttu-id="d0549-327">Zadanie 2 — Wyświetlanie galerii fotografii</span><span class="sxs-lookup"><span data-stu-id="d0549-327">Task 2 - Displaying the Photo Gallery</span></span>

<span data-ttu-id="d0549-328">To zadanie zaktualizuje strony głównej można wyświetlić za pomocą usługi mocked utworzonego w pierwszym zadaniu tego ćwiczenia galerii fotografii.</span><span class="sxs-lookup"><span data-stu-id="d0549-328">In this task, you will update the Home page to show the photo gallery by using the mocked service you created in the first task of this exercise.</span></span> <span data-ttu-id="d0549-329">Spowoduje dodanie plików modelu i zaktualizować widoki galerii.</span><span class="sxs-lookup"><span data-stu-id="d0549-329">You will add model files and update the gallery views.</span></span>

1. <span data-ttu-id="d0549-330">W programie Visual Studio, naciśnij klawisz **SHIFT** + **F5** można zatrzymać debugowania aplikacji.</span><span class="sxs-lookup"><span data-stu-id="d0549-330">In Visual Studio, press **SHIFT** + **F5** to stop debugging the application.</span></span>
2. <span data-ttu-id="d0549-331">Utwórz **fotografii** klasy w **modele** folderu.</span><span class="sxs-lookup"><span data-stu-id="d0549-331">Create the **Photo** class in the **Models** folder.</span></span> <span data-ttu-id="d0549-332">Aby to zrobić, kliknij prawym przyciskiem myszy **modele** folderu, wybierz opcję **Dodaj** i kliknij przycisk **klasy**.</span><span class="sxs-lookup"><span data-stu-id="d0549-332">To do this, right-click on the **Models** folder, select **Add** and click **Class**.</span></span> <span data-ttu-id="d0549-333">Następnie ustaw nazwę **Photo.cs** i kliknij przycisk **Dodaj**.</span><span class="sxs-lookup"><span data-stu-id="d0549-333">Then, set the name to **Photo.cs** and click **Add**.</span></span>
3. <span data-ttu-id="d0549-334">Dodaj następujące elementy członkowskie do **fotografii** klasy.</span><span class="sxs-lookup"><span data-stu-id="d0549-334">Add the following members to the **Photo** class.</span></span>

    <span data-ttu-id="d0549-335">(Fragment - kodu *modelu fotografii ASP.NET MVC 4 laboratorium - Ex02 -*)</span><span class="sxs-lookup"><span data-stu-id="d0549-335">(Code Snippet - *ASP.NET MVC 4 Lab - Ex02 - Photo model*)</span></span>


    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample5.cs)]
4. <span data-ttu-id="d0549-336">Otwórz **HomeController.cs** plik z **kontrolerów** folderu.</span><span class="sxs-lookup"><span data-stu-id="d0549-336">Open the **HomeController.cs** file from the **Controllers** folder.</span></span>
5. <span data-ttu-id="d0549-337">Dodaj następujące instrukcje using.</span><span class="sxs-lookup"><span data-stu-id="d0549-337">Add the following using statements.</span></span>

    <span data-ttu-id="d0549-338">(Fragment - kodu *deklaracje Using HomeController laboratorium - Ex02 - platformy ASP.NET MVC 4*)</span><span class="sxs-lookup"><span data-stu-id="d0549-338">(Code Snippet - *ASP.NET MVC 4 Lab - Ex02 - HomeController Usings*)</span></span>


    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample6.cs)]
6. <span data-ttu-id="d0549-339">Aktualizacja **indeksu** akcję do użycia **HttpClient** pobierania danych galerii, a następnie użyć **JavaScriptSerializer** do deserializacji do modelu widoku.</span><span class="sxs-lookup"><span data-stu-id="d0549-339">Update the **Index** action to use **HttpClient** to retrieve the gallery data, and then use the **JavaScriptSerializer** to deserialize it to the view model.</span></span>

    <span data-ttu-id="d0549-340">(Fragment - kodu *akcji indeksu laboratorium - Ex02 - platformy ASP.NET MVC 4*)</span><span class="sxs-lookup"><span data-stu-id="d0549-340">(Code Snippet - *ASP.NET MVC 4 Lab - Ex02 - Index Action*)</span></span>


    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample7.cs)]
7. <span data-ttu-id="d0549-341">Otwórz **Index.cshtml** plik znajdujący się w obszarze **Views\Home** folderu i Zastąp całą zawartość następującym kodem.</span><span class="sxs-lookup"><span data-stu-id="d0549-341">Open the **Index.cshtml** file located under the **Views\Home** folder and replace all the content with the following code.</span></span>

    <span data-ttu-id="d0549-342">Ten kod w pętli wszystkich zdjęć, które są pobierane z usługi i wyświetla je do listy nieuporządkowanej.</span><span class="sxs-lookup"><span data-stu-id="d0549-342">This code loops through all the photos retrieved from the service and displays them into an unordered list.</span></span>

    <span data-ttu-id="d0549-343">(Fragment - kodu *listy fotografii laboratorium - Ex02 - platformy ASP.NET MVC 4*)</span><span class="sxs-lookup"><span data-stu-id="d0549-343">(Code Snippet - *ASP.NET MVC 4 Lab - Ex02 - Photo List*)</span></span>


    [!code-cshtml[Main](whats-new-in-aspnet-mvc-4/samples/sample8.cshtml)]
8. <span data-ttu-id="d0549-344">W **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy **zawartości** folderu projektu i wybierz **Dodaj | Istniejący element**.</span><span class="sxs-lookup"><span data-stu-id="d0549-344">In the **Solution Explorer**, right-click the **Content** folder of your project, and select **Add | Existing Item**.</span></span> <span data-ttu-id="d0549-345">Przejdź do **Source\Assets\Content** folder tego laboratorium i Dodaj **Site.css** pliku.</span><span class="sxs-lookup"><span data-stu-id="d0549-345">Browse to the **Source\Assets\Content** folder of this lab and add the **Site.css** file.</span></span> <span data-ttu-id="d0549-346">Należy potwierdzić jego zastąpienie.</span><span class="sxs-lookup"><span data-stu-id="d0549-346">You will have to confirm its replacement.</span></span> <span data-ttu-id="d0549-347">Jeśli masz **Site.css** plik otwarty, musisz potwierdzić również ponownie załadować plik.</span><span class="sxs-lookup"><span data-stu-id="d0549-347">If you have the **Site.css** file open, you will have to confirm to reload the file also.</span></span>
9. <span data-ttu-id="d0549-348">Otwórz Eksplorator plików i skopiować całą **zdjęć** folder znajduje się w obszarze **Source\Assets** folder tego laboratorium do folderu głównego projektu w Eksploratorze rozwiązań.</span><span class="sxs-lookup"><span data-stu-id="d0549-348">Open File Explorer and copy the entire **Photos** folder located under the **Source\Assets** folder of this lab to the root folder of your project in Solution Explorer.</span></span>
10. <span data-ttu-id="d0549-349">Uruchom aplikację.</span><span class="sxs-lookup"><span data-stu-id="d0549-349">Run the application.</span></span> <span data-ttu-id="d0549-350">Powinna zostać wyświetlona strona główna wyświetlanie zdjęć w galerii.</span><span class="sxs-lookup"><span data-stu-id="d0549-350">You should now see the home page displaying the photos in the gallery.</span></span>

    <span data-ttu-id="d0549-351">![Galeria fotografii](whats-new-in-aspnet-mvc-4/_static/image21.png "galerii fotografii")</span><span class="sxs-lookup"><span data-stu-id="d0549-351">![Photo Gallery](whats-new-in-aspnet-mvc-4/_static/image21.png "Photo Gallery")</span></span>

    <span data-ttu-id="d0549-352">*Galeria fotografii*</span><span class="sxs-lookup"><span data-stu-id="d0549-352">*Photo Gallery*</span></span>
11. <span data-ttu-id="d0549-353">W programie Visual Studio, naciśnij klawisz **SHIFT** + **F5** można zatrzymać debugowania aplikacji.</span><span class="sxs-lookup"><span data-stu-id="d0549-353">In Visual Studio, press **SHIFT** + **F5** to stop debugging the application.</span></span>

<a id="Exercise3"></a>

<a id="Exercise_3_Adding_support_for_mobile_devices"></a>
### <a name="exercise-3-adding-support-for-mobile-devices"></a><span data-ttu-id="d0549-354">Ćwiczenie 3: Dodawanie obsługi urządzeń przenośnych</span><span class="sxs-lookup"><span data-stu-id="d0549-354">Exercise 3: Adding support for mobile devices</span></span>

<span data-ttu-id="d0549-355">Jeden z kluczy aktualizacji w programie ASP.NET MVC 4 jest obsługiwane do tworzenia aplikacji mobilnych.</span><span class="sxs-lookup"><span data-stu-id="d0549-355">One of the key updates in ASP.NET MVC 4 is the support for mobile development.</span></span> <span data-ttu-id="d0549-356">W tym ćwiczeniu zostanie Poznaj nowe funkcje platformy ASP.NET MVC 4 dla aplikacji dla urządzeń przenośnych przez rozszerzenie rozwiązanie PhotoGallery, które zostały utworzone w poprzednim ćwiczeniu.</span><span class="sxs-lookup"><span data-stu-id="d0549-356">In this exercise, you will explore ASP.NET MVC 4 new features for mobile applications by extending the PhotoGallery solution you have created in the previous exercise.</span></span>

<a id="Task_1_-_Installing_jQuery_Mobile_in_an_ASPNET_MVC_4_Application"></a>
#### <a name="task-1---installing-jquery-mobile-in-an-aspnet-mvc-4-application"></a><span data-ttu-id="d0549-357">Zadanie 1 - instalowania jQuery Mobile w aplikacji ASP.NET MVC 4</span><span class="sxs-lookup"><span data-stu-id="d0549-357">Task 1 - Installing jQuery Mobile in an ASP.NET MVC 4 Application</span></span>

1. <span data-ttu-id="d0549-358">Otwórz **rozpocząć** rozwiązania, znajdujących się na **źródło/Ex3-MobileSupport/Begin/** folderu.</span><span class="sxs-lookup"><span data-stu-id="d0549-358">Open the **Begin** solution located at **Source/Ex3-MobileSupport/Begin/** folder.</span></span> <span data-ttu-id="d0549-359">W przeciwnym razie możesz nadal korzystać **zakończenia** uzyskane rozwiązanie, wykonując poprzednim ćwiczeniu.</span><span class="sxs-lookup"><span data-stu-id="d0549-359">Otherwise, you might continue using the **End** solution obtained by completing the previous exercise.</span></span>

    1. <span data-ttu-id="d0549-360">Po otwarciu dostarczonych **rozpocząć** rozwiązania, musisz pobrać niektórych brakujących pakietów NuGet aby kontynuować.</span><span class="sxs-lookup"><span data-stu-id="d0549-360">If you opened the provided **Begin** solution, you will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="d0549-361">Aby to zrobić, kliknij przycisk **projektu** menu i wybierz **Zarządzaj pakietami NuGet**.</span><span class="sxs-lookup"><span data-stu-id="d0549-361">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
    2. <span data-ttu-id="d0549-362">W **Zarządzaj pakietami NuGet** okna dialogowego, kliknij przycisk **przywrócić** celu pobieranie brakujących pakietów.</span><span class="sxs-lookup"><span data-stu-id="d0549-362">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
    3. <span data-ttu-id="d0549-363">Na koniec Skompiluj rozwiązanie, klikając **kompilacji** | **Kompiluj rozwiązanie**.</span><span class="sxs-lookup"><span data-stu-id="d0549-363">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="d0549-364">Jedną z zalet przy użyciu narzędzia NuGet jest, że nie masz do wysłania wszystkich bibliotek w projekcie, zmniejszenie jego rozmiar projektu.</span><span class="sxs-lookup"><span data-stu-id="d0549-364">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="d0549-365">Narzędzia Power NuGet określając wersje pakietów w pliku Packages.config, będzie można pobrać wymaganych bibliotek podczas pierwszego uruchomienia projektu.</span><span class="sxs-lookup"><span data-stu-id="d0549-365">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="d0549-366">Jest to, dlaczego konieczne będzie wykonanie tych kroków, po otwarciu istniejącego rozwiązania z tego laboratorium.</span><span class="sxs-lookup"><span data-stu-id="d0549-366">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
2. <span data-ttu-id="d0549-367">Otwórz **Konsola Menedżera pakietów** klikając **narzędzia** &gt; **Menedżer pakietów biblioteki** &gt; **Menedżera pakietów Konsola** opcji menu.</span><span class="sxs-lookup"><span data-stu-id="d0549-367">Open the **Package Manager Console** by clicking the **Tools** &gt; **Library Package Manager** &gt; **Package Manager Console** menu option.</span></span>

    <span data-ttu-id="d0549-368">![Otwieranie konsoli Menedżera pakietów NuGet](whats-new-in-aspnet-mvc-4/_static/image22.png "otwieranie konsoli Menedżera pakietów NuGet")</span><span class="sxs-lookup"><span data-stu-id="d0549-368">![Opening the NuGet Package Manager Console](whats-new-in-aspnet-mvc-4/_static/image22.png "Opening the NuGet Package Manager Console")</span></span>

    <span data-ttu-id="d0549-369">*Otwieranie konsoli Menedżera pakietów NuGet*</span><span class="sxs-lookup"><span data-stu-id="d0549-369">*Opening the NuGet Package Manager Console*</span></span>
3. <span data-ttu-id="d0549-370">W konsoli Menedżera pakietów, uruchom następujące polecenie, aby zainstalować **jQuery.Mobile.MVC** pakietu.</span><span class="sxs-lookup"><span data-stu-id="d0549-370">In the Package Manager Console run the following command to install the **jQuery.Mobile.MVC** package.</span></span>

    <span data-ttu-id="d0549-371">jQuery Mobile to biblioteki typu open source do tworzenia zoptymalizowanych pod kątem touch interfejsu użytkownika sieci web.</span><span class="sxs-lookup"><span data-stu-id="d0549-371">jQuery Mobile is an open source library for building touch-optimized web UI.</span></span> <span data-ttu-id="d0549-372">Pakiet NuGet jQuery.Mobile.MVC zawiera pomocników do użycia z aplikacją ASP.NET MVC 4 jQuery Mobile.</span><span class="sxs-lookup"><span data-stu-id="d0549-372">The jQuery.Mobile.MVC NuGet package includes helpers to use jQuery Mobile with an ASP.NET MVC 4 application.</span></span>

    > [!NOTE]
    > <span data-ttu-id="d0549-373">Uruchamiając następujące polecenie, będzie można pobierania biblioteki jQuery.Mobile.MVC z pakietu Nuget.</span><span class="sxs-lookup"><span data-stu-id="d0549-373">By running the following command, you will be downloading the jQuery.Mobile.MVC library from Nuget.</span></span>

    <span data-ttu-id="d0549-374">PM.</span><span class="sxs-lookup"><span data-stu-id="d0549-374">PM</span></span>

    [!code-powershell[Main](whats-new-in-aspnet-mvc-4/samples/sample9.ps1)]

    <span data-ttu-id="d0549-375">To polecenie instaluje jQuery Mobile, a niektóre pliki pomocnicze, takie jak następujące:</span><span class="sxs-lookup"><span data-stu-id="d0549-375">This command installs jQuery Mobile and some helper files, including the following:</span></span>

    - <span data-ttu-id="d0549-376">**Widoki/Shared/\_Layout.Mobile.cshtml**: jest zoptymalizowana pod kątem mniejszego ekranu jQuery Mobile na podstawie układu.</span><span class="sxs-lookup"><span data-stu-id="d0549-376">**Views/Shared/\_Layout.Mobile.cshtml**: is a jQuery Mobile-based layout optimized for a smaller screen.</span></span> <span data-ttu-id="d0549-377">Gdy witryna sieci Web otrzymuje żądanie z przenośnymi przeglądarki, spowoduje zastąpienie oryginalnego układu (\_Layout.cshtml) z tym kontem.</span><span class="sxs-lookup"><span data-stu-id="d0549-377">When the website receives a request from a mobile browser, it will replace the original layout (\_Layout.cshtml) with this one.</span></span>
    - <span data-ttu-id="d0549-378">Składnik tym przełącznikiem widoku: składa się z **widoków/Shared/\_ViewSwitcher.cshtml** widok częściowy i **ViewSwitcherController.cs** kontrolera.</span><span class="sxs-lookup"><span data-stu-id="d0549-378">A view-switcher component: consists of the **Views/Shared/\_ViewSwitcher.cshtml** partial view and the **ViewSwitcherController.cs** controller.</span></span> <span data-ttu-id="d0549-379">Ten składnik będą wyświetlane łącza na przeglądarek urządzeń przenośnych, aby umożliwić użytkownikom przełączyć się do tej wersji strony.</span><span class="sxs-lookup"><span data-stu-id="d0549-379">This component will show a link on mobile browsers to enable users to switch to the desktop version of the page.</span></span>  
        <span data-ttu-id="d0549-380">![Galeria fotografii projektu z obsługą przenośnych](whats-new-in-aspnet-mvc-4/_static/image23.png "galerii fotografii projektu z obsługą przenośnych")</span><span class="sxs-lookup"><span data-stu-id="d0549-380">![Photo Gallery project with mobile support](whats-new-in-aspnet-mvc-4/_static/image23.png "Photo Gallery project with mobile support")</span></span>

        <span data-ttu-id="d0549-381">*Galeria fotografii projektu z obsługą przenośnych*</span><span class="sxs-lookup"><span data-stu-id="d0549-381">*Photo Gallery project with mobile support*</span></span>
4. <span data-ttu-id="d0549-382">Zarejestruj pakiety przenośnych.</span><span class="sxs-lookup"><span data-stu-id="d0549-382">Register the Mobile bundles.</span></span> <span data-ttu-id="d0549-383">Aby to zrobić, otwórz **Global.asax.cs** pliku i Dodaj następujący wiersz.</span><span class="sxs-lookup"><span data-stu-id="d0549-383">To do this, open the **Global.asax.cs** file and add the following line.</span></span>

    <span data-ttu-id="d0549-384">(Fragment - kodu *platformy ASP.NET MVC 4 - Ex03 - laboratorium rejestru przenośnych pakiety*)</span><span class="sxs-lookup"><span data-stu-id="d0549-384">(Code Snippet - *ASP.NET MVC 4 Lab - Ex03 - Register Mobile Bundles*)</span></span>


    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample10.cs)]
5. <span data-ttu-id="d0549-385">Uruchom aplikację za pomocą przeglądarki sieci web.</span><span class="sxs-lookup"><span data-stu-id="d0549-385">Run the application using a desktop web browser.</span></span>
6. <span data-ttu-id="d0549-386">Otwórz **Windows Phone 7 Emulator,** znajduje się w **Start Menu | Wszystkie programy | Windows Phone SDK 7.1 | Emulator Windows Phone.**</span><span class="sxs-lookup"><span data-stu-id="d0549-386">Open the **Windows Phone 7 Emulator,** located in **Start Menu | All Programs | Windows Phone SDK 7.1 | Windows Phone Emulator.**</span></span>
7. <span data-ttu-id="d0549-387">Na ekranie startowym telefonu Otwórz program Internet Explorer.</span><span class="sxs-lookup"><span data-stu-id="d0549-387">In the phone start screen, open Internet Explorer.</span></span> <span data-ttu-id="d0549-388">Zapoznaj się z adresu URL, w której aplikacja jest uruchomiona, a następnie przejdź do tego adresu URL za pośrednictwem przeglądarki telefonu (np. `http://localhost:[PortNumber]/`).</span><span class="sxs-lookup"><span data-stu-id="d0549-388">Check out the URL where the application started and browse to that URL with the phone browser (e.g. `http://localhost:[PortNumber]/`).</span></span>

    <span data-ttu-id="d0549-389">Można zauważyć, że aplikacja będzie wyglądać inaczej w emulator Windows Phone jQuery.Mobile.MVC ma tworzenia nowych zasobów w projekcie Pokaż widoki zoptymalizowane dla urządzeń przenośnych.</span><span class="sxs-lookup"><span data-stu-id="d0549-389">You will notice that your application will look different in the Windows Phone emulator, as the jQuery.Mobile.MVC has created new assets in your project that show views optimized for mobile devices.</span></span>

    <span data-ttu-id="d0549-390">Zwróć uwagę, wiadomości w górnej części przez telefon, przedstawiający link zmienia się do widoku pulpitu.</span><span class="sxs-lookup"><span data-stu-id="d0549-390">Notice the message at the top of the phone, showing the link that switches to the Desktop view.</span></span> <span data-ttu-id="d0549-391">Ponadto  **\_Layout.Mobile.cshtml** układu, który został utworzony przez pakiet został zainstalowany inny układ jest w tym w aplikacji.</span><span class="sxs-lookup"><span data-stu-id="d0549-391">Additionally, the **\_Layout.Mobile.cshtml** layout that was created by the package you have installed is including a different layout in the application.</span></span>

    > [!NOTE]
    > <span data-ttu-id="d0549-392">Do tej pory nie ma żadnego linku, aby wrócić do widokiem dla urządzeń przenośnych.</span><span class="sxs-lookup"><span data-stu-id="d0549-392">So far, there is no link to get back to mobile view.</span></span> <span data-ttu-id="d0549-393">Są uwzględniane w nowszych wersjach.</span><span class="sxs-lookup"><span data-stu-id="d0549-393">It will be included in later versions.</span></span>

    <span data-ttu-id="d0549-394">![Widok strony głównej Galeria fotografii dla urządzeń przenośnych](whats-new-in-aspnet-mvc-4/_static/image24.png "widok strony głównej Galeria fotografii dla urządzeń przenośnych")</span><span class="sxs-lookup"><span data-stu-id="d0549-394">![Mobile view of the Photo Gallery Home page](whats-new-in-aspnet-mvc-4/_static/image24.png "Mobile view of the Photo Gallery Home page")</span></span>

    <span data-ttu-id="d0549-395">*Widok strony głównej Galeria fotografii dla urządzeń przenośnych*</span><span class="sxs-lookup"><span data-stu-id="d0549-395">*Mobile view of the Photo Gallery Home page*</span></span>
8. <span data-ttu-id="d0549-396">W programie Visual Studio, naciśnij klawisz **SHIFT** + **F5** można zatrzymać debugowania aplikacji.</span><span class="sxs-lookup"><span data-stu-id="d0549-396">In Visual Studio, press **SHIFT** + **F5** to stop debugging the application.</span></span>

<a id="Task_2_-_Creating_Mobile_Views"></a>
#### <a name="task-2---creating-mobile-views"></a><span data-ttu-id="d0549-397">Zadanie 2 — Tworzenie widokach dla urządzeń przenośnych</span><span class="sxs-lookup"><span data-stu-id="d0549-397">Task 2 - Creating Mobile Views</span></span>

<span data-ttu-id="d0549-398">W ramach tego zadania spowoduje utworzenie mobilnej wersji widoku indeksu z zawartością dostosowane do lepszego appareance na urządzeniach przenośnych.</span><span class="sxs-lookup"><span data-stu-id="d0549-398">In this task, you will create a mobile version of the index view with content adapted for better appareance in mobile devices.</span></span>

1. <span data-ttu-id="d0549-399">Kopiuj **Views\Home\Index.cshtml** wyświetlanie i wklej go, aby utworzyć kopię, Zmień nazwę nowego pliku **Index.Mobile.cshtml**.</span><span class="sxs-lookup"><span data-stu-id="d0549-399">Copy the **Views\Home\Index.cshtml** view and paste it to create a copy, rename the new file to **Index.Mobile.cshtml**.</span></span>
2. <span data-ttu-id="d0549-400">Otwórz utworzony nowy **Index.Mobile.cshtml** wyświetlanie i Zastąp istniejące &lt;ul&gt; tag o tym kodzie.</span><span class="sxs-lookup"><span data-stu-id="d0549-400">Open the new created **Index.Mobile.cshtml** view and replace the existing &lt;ul&gt; tag with this code.</span></span> <span data-ttu-id="d0549-401">Dzięki temu będzie aktualizowanie &lt;ul&gt; tag przy użyciu adnotacji danych mobilnych jQuery używać przenośnych motywów z jQuery.</span><span class="sxs-lookup"><span data-stu-id="d0549-401">By doing this, you will be updating the &lt;ul&gt; tag with jQuery Mobile data annotations to use the mobile themes from jQuery.</span></span>


    [!code-html[Main](whats-new-in-aspnet-mvc-4/samples/sample11.html)]

    > [!NOTE] 
    > 
    > <span data-ttu-id="d0549-402">Zwróć uwagę, że:</span><span class="sxs-lookup"><span data-stu-id="d0549-402">Notice that:</span></span>
    > 
    > - <span data-ttu-id="d0549-403">**Danych roli** ustawić atrybutu **listview** spowoduje, że listy przy użyciu stylów elementu listview.</span><span class="sxs-lookup"><span data-stu-id="d0549-403">The **data-role** attribute set to **listview** will render the list using the listview styles.</span></span>
    > 
    > - <span data-ttu-id="d0549-404">**Danych wstawki** pokazuje listę zaokrąglony obramowania i margines atrybut ma wartość true.</span><span class="sxs-lookup"><span data-stu-id="d0549-404">The **data-inset** attribute set to true will show the list with rounded border and margin.</span></span>
    > 
    > - <span data-ttu-id="d0549-405">**Filtr danych** ustawić atrybutu **true** wygeneruje pola wyszukiwania.</span><span class="sxs-lookup"><span data-stu-id="d0549-405">The **data-filter** attribute set to **true** will generate a search box.</span></span>
    > 
    > <span data-ttu-id="d0549-406">Dowiedz się więcej o konwencje przenośnych dostępne w dokumentacji projektu: [ [http://jquerymobile.com/demos/1.1.1/](http://jquerymobile.com/demos/1.1.1/)](http://jquerymobile.com/demos/1.1.1/)</span><span class="sxs-lookup"><span data-stu-id="d0549-406">You can learn more about jQuery Mobile conventions in the project documentation: [[http://jquerymobile.com/demos/1.1.1/](http://jquerymobile.com/demos/1.1.1/)](http://jquerymobile.com/demos/1.1.1/)</span></span>
3. <span data-ttu-id="d0549-407">Naciśnij klawisz **CTRL + S** Aby zapisać zmiany.</span><span class="sxs-lookup"><span data-stu-id="d0549-407">Press **CTRL + S** to save the changes.</span></span>
4. <span data-ttu-id="d0549-408">Przełącz się do **Emulator Windows Phone** i Odśwież lokacji.</span><span class="sxs-lookup"><span data-stu-id="d0549-408">Switch to the **Windows Phone Emulator** and refresh the site.</span></span> <span data-ttu-id="d0549-409">Zwróć uwagę, nowe wygląd listy galerii, a także pole wyszukiwania znajduje się na górze.</span><span class="sxs-lookup"><span data-stu-id="d0549-409">Notice the new look and feel of the gallery list, as well as the new search box located on the top.</span></span> <span data-ttu-id="d0549-410">Następnie wpisz wyraz w polu wyszukiwania (na przykład **Tulipany**), aby rozpocząć wyszukiwanie w Galerii fotografii.</span><span class="sxs-lookup"><span data-stu-id="d0549-410">Then, type a word in the search box (for instance, **Tulips**) to start a search in the photo gallery.</span></span>

    <span data-ttu-id="d0549-411">![Galeria przy użyciu stylu listview z filtrowania](whats-new-in-aspnet-mvc-4/_static/image25.png "galerii przy użyciu stylu listview z filtrowania")</span><span class="sxs-lookup"><span data-stu-id="d0549-411">![Gallery using listview style with filtering](whats-new-in-aspnet-mvc-4/_static/image25.png "Gallery using listview style with filtering")</span></span>

    <span data-ttu-id="d0549-412">*Galeria przy użyciu stylu listview z filtrowania*</span><span class="sxs-lookup"><span data-stu-id="d0549-412">*Gallery using listview style with filtering*</span></span>

    <span data-ttu-id="d0549-413">Podsumowując, aby utworzyć kopię widoku indeksu z zostały użyte przepisu Mobilizer widoku &quot;przenośnych&quot; sufiks.</span><span class="sxs-lookup"><span data-stu-id="d0549-413">To summarize, you have used the View Mobilizer recipe to create a copy of the Index view with the &quot;mobile&quot; suffix.</span></span> <span data-ttu-id="d0549-414">Ten sufiks wskazuje ASP.NET MVC 4, że każde żądanie generowane z urządzenia przenośnego będzie używał kopii tego indeksu.</span><span class="sxs-lookup"><span data-stu-id="d0549-414">This suffix indicates to ASP.NET MVC 4 that every request generated from a mobile device will use this copy of the index.</span></span> <span data-ttu-id="d0549-415">Ponadto zaktualizowano wersja urządzenia przenośne widoku indeksu na jQuery Mobile wzmocnienia lokacji wygląd i działanie na urządzeniach przenośnych.</span><span class="sxs-lookup"><span data-stu-id="d0549-415">Additionally, you have updated the mobile version of the Index view to use jQuery Mobile for enhancing the site look and feel in mobile devices.</span></span>
5. <span data-ttu-id="d0549-416">Wróć do programu Visual Studio i Otwórz **Site.Mobile.css** znajduje się w obszarze **zawartości** folderu.</span><span class="sxs-lookup"><span data-stu-id="d0549-416">Go back to Visual Studio and open **Site.Mobile.css** located under the **Content** folder.</span></span>
6. <span data-ttu-id="d0549-417">Napraw pozycjonowanie tytuł fotografii, aby wyświetlić je po prawej stronie obrazu.</span><span class="sxs-lookup"><span data-stu-id="d0549-417">Fix the positioning of the photo title to make it show at the right side of the image.</span></span> <span data-ttu-id="d0549-418">Aby to zrobić, Dodaj następujący kod, aby **Site.Mobile.css** pliku.</span><span class="sxs-lookup"><span data-stu-id="d0549-418">To do this, add the following code to the **Site.Mobile.css** file.</span></span>

    <span data-ttu-id="d0549-419">CSS</span><span class="sxs-lookup"><span data-stu-id="d0549-419">CSS</span></span>

    [!code-css[Main](whats-new-in-aspnet-mvc-4/samples/sample12.css)]
7. <span data-ttu-id="d0549-420">Naciśnij klawisz **CTRL + S** Aby zapisać zmiany.</span><span class="sxs-lookup"><span data-stu-id="d0549-420">Press **CTRL + S** to save the changes.</span></span>
8. <span data-ttu-id="d0549-421">Przywraca **Emulator Windows Phone** i Odśwież lokacji.</span><span class="sxs-lookup"><span data-stu-id="d0549-421">Switch back to the **Windows Phone Emulator** and refresh the site.</span></span> <span data-ttu-id="d0549-422">Należy zauważyć, że tytuł fotografii prawidłowo znajduje się teraz.</span><span class="sxs-lookup"><span data-stu-id="d0549-422">Notice the photo title is properly positioned now.</span></span>

    <span data-ttu-id="d0549-423">![Tytuł znajduje się po prawej stronie obrazu](whats-new-in-aspnet-mvc-4/_static/image26.png "tytuł znajduje się po prawej stronie obrazu")</span><span class="sxs-lookup"><span data-stu-id="d0549-423">![Title positioned on the right side of the image](whats-new-in-aspnet-mvc-4/_static/image26.png "Title positioned on the right side of the image")</span></span>

    <span data-ttu-id="d0549-424">*Tytuł znajduje się po prawej stronie obrazu*</span><span class="sxs-lookup"><span data-stu-id="d0549-424">*Title positioned on the right side of the image*</span></span>

<a id="Task_3_-_jQuery_Mobile_Themes"></a>
#### <a name="task-3---jquery-mobile-themes"></a><span data-ttu-id="d0549-425">Zadanie 3 - jQuery Mobile motywów</span><span class="sxs-lookup"><span data-stu-id="d0549-425">Task 3 - jQuery Mobile Themes</span></span>

<span data-ttu-id="d0549-426">Każdy układ i elementu widget w jQuery Mobile jest zaprojektowana dla nowego zorientowane obiektowo CSS platforma, która pozwala na zastosowanie motywu pełną ujednoliconego projekt visual do witryn i aplikacji.</span><span class="sxs-lookup"><span data-stu-id="d0549-426">Every layout and widget in jQuery Mobile is designed around a new object-oriented CSS framework that makes it possible to apply a complete unified visual design theme to sites and applications.</span></span>

<span data-ttu-id="d0549-427">jQuery Mobile domyślny motyw obejmuje 5 próbki, które są podane litery (, b, c, d, e) krótkimi opisami.</span><span class="sxs-lookup"><span data-stu-id="d0549-427">jQuery Mobile's default Theme includes 5 swatches that are given letters (a, b, c, d, e) for quick reference.</span></span>

<span data-ttu-id="d0549-428">To zadanie zaktualizuje przenośnych układ na kompozycję inną niż domyślna.</span><span class="sxs-lookup"><span data-stu-id="d0549-428">In this task, you will update the mobile layout to use a different theme than the default.</span></span>

1. <span data-ttu-id="d0549-429">Przełącz się do programu Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="d0549-429">Switch back to Visual Studio.</span></span>
2. <span data-ttu-id="d0549-430">Otwórz  **\_Layout.Mobile.cshtml** plik znajdujący się w **Views\Shared**.</span><span class="sxs-lookup"><span data-stu-id="d0549-430">Open the **\_Layout.Mobile.cshtml** file located in **Views\Shared**.</span></span>
3. <span data-ttu-id="d0549-431">Znajdź div element z rolą danych ustawioną &quot;strony&quot; i zaktualizuj **data-theme** do &quot; **e**&quot;.</span><span class="sxs-lookup"><span data-stu-id="d0549-431">Find the div element with the data-role set to &quot;page&quot; and update the **data-theme** to &quot;**e**&quot;.</span></span>


    [!code-html[Main](whats-new-in-aspnet-mvc-4/samples/sample13.html)]
4. <span data-ttu-id="d0549-432">Naciśnij klawisz **CTRL + S** Aby zapisać zmiany.</span><span class="sxs-lookup"><span data-stu-id="d0549-432">Press **CTRL + S** to save the changes.</span></span>
5. <span data-ttu-id="d0549-433">Odśwież lokację w **Emulator Windows Phone** i zwróć uwagę, nowy schemat kolorów.</span><span class="sxs-lookup"><span data-stu-id="d0549-433">Refresh the site in the **Windows Phone Emulator** and notice the new colors scheme.</span></span>

    <span data-ttu-id="d0549-434">![Przenośne układ z inny schemat kolorów](whats-new-in-aspnet-mvc-4/_static/image27.png "przenośnych układ z innego schematu kolorów")</span><span class="sxs-lookup"><span data-stu-id="d0549-434">![Mobile layout with a different color scheme](whats-new-in-aspnet-mvc-4/_static/image27.png "Mobile layout with a different color scheme")</span></span>

    <span data-ttu-id="d0549-435">*Przenośne układ z innego schematu kolorów*</span><span class="sxs-lookup"><span data-stu-id="d0549-435">*Mobile layout with a different color scheme*</span></span>

<a id="Task_4_-_Using_the_View-Switcher_Component_and_the_Browser_Overriding_Features"></a>
#### <a name="task-4---using-the-view-switcher-component-and-the-browser-overriding-features"></a><span data-ttu-id="d0549-436">Zadanie 4 — przy użyciu składnika tym przełącznikiem widoku i zastępowanie funkcje przeglądarki</span><span class="sxs-lookup"><span data-stu-id="d0549-436">Task 4 - Using the View-Switcher Component and the Browser Overriding Features</span></span>

<span data-ttu-id="d0549-437">Konwencja dla stron sieci web zoptymalizowanych pod kątem mobile jest dodać łącze, którego tekst jest coś jak widok pulpitu lub tryb witrynę w trybie pełnym, który umożliwia użytkownikom pulpitu wersję strony.</span><span class="sxs-lookup"><span data-stu-id="d0549-437">A convention for mobile-optimized web pages is to add a link whose text is something like Desktop view or Full site mode that lets users switch to a desktop version of the page.</span></span> <span data-ttu-id="d0549-438">Pakiet jQuery.Mobile.MVC zawiera przykładowe **tym przełącznikiem widoku** składnika w tym celu używana w  **\_Layout.Mobile.cshtml** widoku.</span><span class="sxs-lookup"><span data-stu-id="d0549-438">The jQuery.Mobile.MVC package includes a sample **view-switcher** component for this purpose used in the **\_Layout.Mobile.cshtml** view.</span></span>

<span data-ttu-id="d0549-439">![Łącze, aby przełączyć do widoku pulpitu](whats-new-in-aspnet-mvc-4/_static/image28.png "łącze, aby przełączyć do widoku pulpitu")</span><span class="sxs-lookup"><span data-stu-id="d0549-439">![Link to switch to Desktop View](whats-new-in-aspnet-mvc-4/_static/image28.png "Link to switch to Desktop View")</span></span>

<span data-ttu-id="d0549-440">*Łącze, aby przełączyć do widoku pulpitu*</span><span class="sxs-lookup"><span data-stu-id="d0549-440">*Link to switch to Desktop View*</span></span>

<span data-ttu-id="d0549-441">Korzysta z tym przełącznikiem widoku nową funkcję o nazwie **zastępowania przeglądarki**.</span><span class="sxs-lookup"><span data-stu-id="d0549-441">The view switcher uses a new feature called **Browser Overriding**.</span></span> <span data-ttu-id="d0549-442">Ta funkcja umożliwia aplikacji taką obsługę żądań, tak jakby pochodziły one z innej przeglądarki (agenta użytkownika) niż ten, który faktycznie pochodzą.</span><span class="sxs-lookup"><span data-stu-id="d0549-442">This feature lets your application treat requests as if they were coming from a different browser (user agent) than the one they are actually coming from.</span></span>

<span data-ttu-id="d0549-443">W tym zadaniu zostanie Poznaj Przykładowa implementacja dodane przez jQuery.Mobile.MVC i Nowa przeglądarka zastępowanie funkcji w programie ASP.NET MVC 4 przełącznik widoku.</span><span class="sxs-lookup"><span data-stu-id="d0549-443">In this task, you will explore the sample implementation of a view-switcher added by jQuery.Mobile.MVC and the new browser overriding features in ASP.NET MVC 4.</span></span>

1. <span data-ttu-id="d0549-444">Przełącz się do programu Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="d0549-444">Switch back to Visual Studio.</span></span>
2. <span data-ttu-id="d0549-445">Otwórz  **\_Layout.Mobile.cshtml** widoku znajduje się w obszarze **Views\Shared** folderu i zwróć uwagę, składnik tym przełącznikiem widoku jest określany jako widok częściowy.</span><span class="sxs-lookup"><span data-stu-id="d0549-445">Open the **\_Layout.Mobile.cshtml** view located under the **Views\Shared** folder and notice the view-switcher component being referenced as a partial view.</span></span>

    <span data-ttu-id="d0549-446">![Układ przenośnych za pomocą składnika z tym przełącznikiem widoku](whats-new-in-aspnet-mvc-4/_static/image29.png "układu przenośnych za pomocą składnika z tym przełącznikiem widoku")</span><span class="sxs-lookup"><span data-stu-id="d0549-446">![Mobile layout using View-Switcher component](whats-new-in-aspnet-mvc-4/_static/image29.png "Mobile layout using View-Switcher component")</span></span>

    <span data-ttu-id="d0549-447">*Układ przenośnych za pomocą składnika z tym przełącznikiem widoku*</span><span class="sxs-lookup"><span data-stu-id="d0549-447">*Mobile layout using View-Switcher component*</span></span>
3. <span data-ttu-id="d0549-448">Otwórz  **\_ViewSwitcher.cshtml** widoku częściowego.</span><span class="sxs-lookup"><span data-stu-id="d0549-448">Open the **\_ViewSwitcher.cshtml** partial view.</span></span>

    <span data-ttu-id="d0549-449">Nowa metoda używa widoku częściowego **ViewContext.HttpContext.GetOverriddenBrowser()** do określenia pochodzenia żądania sieci web i wyświetlić odpowiednie łącze, aby przełączyć się do widoków Desktop lub Mobile.</span><span class="sxs-lookup"><span data-stu-id="d0549-449">The partial view uses the new method **ViewContext.HttpContext.GetOverriddenBrowser()** to determine the origin of the web request and show the corresponding link to switch either to the Desktop or Mobile views.</span></span>

    <span data-ttu-id="d0549-450">**GetOverridenBrowser** metoda zwraca **HttpBrowserCapabilitiesBase** wystąpienie, które odpowiada agenta użytkownika aktualnie ustawione na żądanie (rzeczywista lub przesłonięte).</span><span class="sxs-lookup"><span data-stu-id="d0549-450">The **GetOverridenBrowser** method returns an **HttpBrowserCapabilitiesBase** instance that corresponds to the user agent currently set for the request (actual or overridden).</span></span> <span data-ttu-id="d0549-451">Możesz użyć tej wartości można pobrać właściwości, takie jak **IsMobileDevice**.</span><span class="sxs-lookup"><span data-stu-id="d0549-451">You can use this value to get properties such as **IsMobileDevice**.</span></span>

    <span data-ttu-id="d0549-452">![Widok częściowy ViewSwitcher](whats-new-in-aspnet-mvc-4/_static/image30.png "ViewSwitcher widok częściowy")</span><span class="sxs-lookup"><span data-stu-id="d0549-452">![ViewSwitcher partial view](whats-new-in-aspnet-mvc-4/_static/image30.png "ViewSwitcher partial view")</span></span>

    <span data-ttu-id="d0549-453">*Widok częściowy ViewSwitcher*</span><span class="sxs-lookup"><span data-stu-id="d0549-453">*ViewSwitcher partial view*</span></span>
4. <span data-ttu-id="d0549-454">Otwórz **ViewSwitcherController.cs** klasa znajduje się w **kontrolerów** folderu.</span><span class="sxs-lookup"><span data-stu-id="d0549-454">Open the **ViewSwitcherController.cs** class located in the **Controllers** folder.</span></span> <span data-ttu-id="d0549-455">Zapoznaj się z tym SwitchView akcji jest wywoływana przez łącze w składniku ViewSwitcher i zwróć uwagę, nowych metod element HttpContext.</span><span class="sxs-lookup"><span data-stu-id="d0549-455">Check out that SwitchView action is called by the link in the ViewSwitcher component, and notice the new HttpContext methods.</span></span>

    - <span data-ttu-id="d0549-456">**HttpContext.ClearOverridenBrowser()** metoda usuwa wszelkich przesłoniętych agentów użytkownika dla bieżącego żądania.</span><span class="sxs-lookup"><span data-stu-id="d0549-456">The **HttpContext.ClearOverridenBrowser()** method removes any overridden user agent for the current request.</span></span>
    - <span data-ttu-id="d0549-457">**HttpContext.SetOverridenBrowser()** metody przesłania żądania rzeczywistą wartość agenta użytkownika przy użyciu określonego agenta użytkownika.</span><span class="sxs-lookup"><span data-stu-id="d0549-457">The **HttpContext.SetOverridenBrowser()** method overrides the request's actual user agent value using the specified user agent.</span></span>  
        <span data-ttu-id="d0549-458">![Kontroler ViewSwitcher](whats-new-in-aspnet-mvc-4/_static/image31.png "ViewSwitcher kontrolera")</span><span class="sxs-lookup"><span data-stu-id="d0549-458">![ViewSwitcher Controller](whats-new-in-aspnet-mvc-4/_static/image31.png "ViewSwitcher Controller")</span></span>  
<span data-ttu-id="d0549-459">*Kontroler ViewSwitcher*</span><span class="sxs-lookup"><span data-stu-id="d0549-459">*ViewSwitcher Controller*</span></span>

        <span data-ttu-id="d0549-460">Przesłanianie przeglądarki jest funkcją podstawowej platformy ASP.NET MVC 4, który jest również dostępny nawet wtedy, gdy pakiet jQuery.Mobile.MVC nie jest instalowany.</span><span class="sxs-lookup"><span data-stu-id="d0549-460">Browser Overriding is a core feature of ASP.NET MVC 4, which is also available even if you do not install the jQuery.Mobile.MVC package.</span></span> <span data-ttu-id="d0549-461">Jednak ta funkcja dotyczy tylko widoku, układu i widoku częściowego, a nie dotyczy żadnych funkcji, które są zależne od obiektu Request.Browser.</span><span class="sxs-lookup"><span data-stu-id="d0549-461">However, this feature affects only view, layout, and partial-view, and it does not affect any of the features that depend on the Request.Browser object.</span></span>

<a id="Task_5_-_Adding_the_View-Switcher_in_the_Desktop_View"></a>
#### <a name="task-5---adding-the-view-switcher-in-the-desktop-view"></a><span data-ttu-id="d0549-462">Zadanie 5 dodawanie z tym przełącznikiem widoku w widoku pulpitu.</span><span class="sxs-lookup"><span data-stu-id="d0549-462">Task 5 - Adding the View-Switcher in the Desktop View</span></span>

<span data-ttu-id="d0549-463">To zadanie zaktualizuje układ pulpitu do uwzględnienia w tym przełącznikiem widoku.</span><span class="sxs-lookup"><span data-stu-id="d0549-463">In this task, you will update the desktop layout to include the view-switcher.</span></span> <span data-ttu-id="d0549-464">Dzięki temu użytkowników mobilnych powrócić do widoku przenośnych podczas przeglądania widok pulpitu.</span><span class="sxs-lookup"><span data-stu-id="d0549-464">This will allow mobile users to go back to the mobile view when browsing the desktop view.</span></span>

1. <span data-ttu-id="d0549-465">Odśwież lokację w **Emulator Windows Phone**.</span><span class="sxs-lookup"><span data-stu-id="d0549-465">Refresh the site in the **Windows Phone Emulator**.</span></span>
2. <span data-ttu-id="d0549-466">Polecenie **widok pulpitu** łącze w górnej części galerii.</span><span class="sxs-lookup"><span data-stu-id="d0549-466">Click on the **Desktop view** link at the top of the gallery.</span></span> <span data-ttu-id="d0549-467">Należy zauważyć, że istnieje widok przełącznik w widoku pulpitu do zezwalania powróć do widoku przenośnych.</span><span class="sxs-lookup"><span data-stu-id="d0549-467">Notice there is no view-switcher in the desktop view to allow you return to the mobile view.</span></span>
3. <span data-ttu-id="d0549-468">Wróć do programu Visual Studio i Otwórz  **\_Layout.cshtml** widoku.</span><span class="sxs-lookup"><span data-stu-id="d0549-468">Go back to Visual Studio and open the **\_Layout.cshtml** view.</span></span>
4. <span data-ttu-id="d0549-469">Znajdź sekcję logowania i wstawianie wywołania do renderowania  **\_ViewSwitcher** widoku częściowego poniżej  **\_LogOnPartial** widoku częściowego.</span><span class="sxs-lookup"><span data-stu-id="d0549-469">Find the login section and insert a call to render the **\_ViewSwitcher** partial view below the **\_LogOnPartial** partial view.</span></span> <span data-ttu-id="d0549-470">Naciśnij klawisz **CTRL + S** Aby zapisać zmiany.</span><span class="sxs-lookup"><span data-stu-id="d0549-470">Then, press **CTRL + S** to save the changes.</span></span>


    [!code-cshtml[Main](whats-new-in-aspnet-mvc-4/samples/sample14.cshtml)]
5. <span data-ttu-id="d0549-471">Naciśnij klawisz **CTRL + S** Aby zapisać zmiany.</span><span class="sxs-lookup"><span data-stu-id="d0549-471">Press **CTRL + S** to save the changes.</span></span>
6. <span data-ttu-id="d0549-472">Odśwież stronę w Emulator Windows Phone, a następnie kliknij dwukrotnie ikonę ekranu, aby powiększyć.</span><span class="sxs-lookup"><span data-stu-id="d0549-472">Refresh the page in the Windows Phone Emulator and double-click the screen to zoom in.</span></span> <span data-ttu-id="d0549-473">Należy zauważyć, że strona główna zawiera obecnie **widokiem dla urządzeń przenośnych** łącze, które zmienia z przenośnymi na widok pulpitu.</span><span class="sxs-lookup"><span data-stu-id="d0549-473">Notice that the home page now shows the **Mobile view** link that switches from mobile to desktop view.</span></span>

    <span data-ttu-id="d0549-474">![Wyświetl przełącznik renderowane w widoku pulpitu](whats-new-in-aspnet-mvc-4/_static/image32.png "przełącznik widok renderowany w widoku pulpitu")</span><span class="sxs-lookup"><span data-stu-id="d0549-474">![View Switcher rendered in desktop view](whats-new-in-aspnet-mvc-4/_static/image32.png "View Switcher rendered in desktop view")</span></span>

    <span data-ttu-id="d0549-475">*Przełącznik widok renderowany w widoku pulpitu*</span><span class="sxs-lookup"><span data-stu-id="d0549-475">*View Switcher rendered in desktop view*</span></span>
7. <span data-ttu-id="d0549-476">Przełącz do widoku przenośnych ponownie, a następnie przejdź do **o** strony (http://localhost [portu] / Home/o).</span><span class="sxs-lookup"><span data-stu-id="d0549-476">Switch to the Mobile view again and browse to **About** page (http://localhost[port]/Home/About).</span></span> <span data-ttu-id="d0549-477">Należy zauważyć, że nawet jeśli nie utworzono widok About.Mobile.cshtml informacje strona jest wyświetlana w układzie przenośnych (\_Layout.Mobile.cshtml).</span><span class="sxs-lookup"><span data-stu-id="d0549-477">Notice that, even if you haven't created an About.Mobile.cshtml view, the About page is displayed using the mobile layout (\_Layout.Mobile.cshtml).</span></span>

    <span data-ttu-id="d0549-478">![Strona informacje](whats-new-in-aspnet-mvc-4/_static/image33.png "strona — informacje")</span><span class="sxs-lookup"><span data-stu-id="d0549-478">![About page](whats-new-in-aspnet-mvc-4/_static/image33.png "About page")</span></span>

    <span data-ttu-id="d0549-479">*Strona — informacje*</span><span class="sxs-lookup"><span data-stu-id="d0549-479">*About page*</span></span>
8. <span data-ttu-id="d0549-480">Na koniec Otwórz witrynę w przeglądarce sieci Web pulpitu.</span><span class="sxs-lookup"><span data-stu-id="d0549-480">Finally, open the site in a desktop Web browser.</span></span> <span data-ttu-id="d0549-481">Należy zauważyć, że żaden z poprzednich aktualizacji ma wpływ widok pulpitu.</span><span class="sxs-lookup"><span data-stu-id="d0549-481">Notice that none of the previous updates has affected the desktop view.</span></span>

    <span data-ttu-id="d0549-482">![Widok pulpitu PhotoGallery](whats-new-in-aspnet-mvc-4/_static/image34.png "PhotoGallery widok pulpitu")</span><span class="sxs-lookup"><span data-stu-id="d0549-482">![PhotoGallery desktop view](whats-new-in-aspnet-mvc-4/_static/image34.png "PhotoGallery desktop view")</span></span>

    <span data-ttu-id="d0549-483">*Widok pulpitu PhotoGallery*</span><span class="sxs-lookup"><span data-stu-id="d0549-483">*PhotoGallery desktop view*</span></span>

<a id="Task_6_-_Creating_New_Display_Modes"></a>
#### <a name="task-6---creating-new-display-modes"></a><span data-ttu-id="d0549-484">Zadanie 6 — utworzenie nowego trybów wyświetlania</span><span class="sxs-lookup"><span data-stu-id="d0549-484">Task 6 - Creating New Display Modes</span></span>

<span data-ttu-id="d0549-485">Nowa funkcja trybów wyświetlania umożliwia aplikacji wybierz widoki, w zależności od przeglądarki, która generuje żądanie.</span><span class="sxs-lookup"><span data-stu-id="d0549-485">The new display modes feature lets an application select views depending on the browser that is generating the request.</span></span> <span data-ttu-id="d0549-486">Na przykład jeśli przeglądarka pulpitu zażąda strony głównej, aplikacja zwróci **Views\Home\Index.cshtml** szablonu.</span><span class="sxs-lookup"><span data-stu-id="d0549-486">For example, if a desktop browser requests the Home page, the application will return the **Views\Home\Index.cshtml** template.</span></span> <span data-ttu-id="d0549-487">Następnie, jeśli przeglądarkę dla telefonów zażąda strony głównej, aplikacja zwróci **Views\Home\Index.mobile.cshtml** szablonu.</span><span class="sxs-lookup"><span data-stu-id="d0549-487">Then, if a mobile browser requests the Home page, the application will return the **Views\Home\Index.mobile.cshtml** template.</span></span>

<span data-ttu-id="d0549-488">W ramach tego zadania spowoduje utworzenie układu dostosowanego dla urządzenia iPhone i konieczne będzie symulować żądania z urządzenia iPhone.</span><span class="sxs-lookup"><span data-stu-id="d0549-488">In this task, you will create a customized layout for iPhone devices, and you will have to simulate requests from iPhone devices.</span></span> <span data-ttu-id="d0549-489">Aby to zrobić, można użyć albo iPhone emulatorze/symulatorze (takich jak [Electric symulatora Mobile](http://www.electricplum.com/)) lub przeglądarki z dodatkami, które modyfikują agenta użytkownika.</span><span class="sxs-lookup"><span data-stu-id="d0549-489">To do this, you can use either an iPhone emulator/simulator (like [Electric Mobile Simulator](http://www.electricplum.com/)) or a browser with add-ons that modify the user agent.</span></span> <span data-ttu-id="d0549-490">Aby uzyskać instrukcje dotyczące ustawiania ciąg agenta użytkownika przeglądarki Safari emulować iPhone, zobacz [jak umożliwić Safari podawać jest IE](http://www.davidalison.com/2008/05/how-to-let-safari-pretend-its-ie.html) w blogu Alison Dominika.</span><span class="sxs-lookup"><span data-stu-id="d0549-490">For instructions on how to set the user agent string in an Safari browser to emulate an iPhone, see [How to let Safari pretend it's IE](http://www.davidalison.com/2008/05/how-to-let-safari-pretend-its-ie.html) in David Alison's blog.</span></span>

<span data-ttu-id="d0549-491">**Zwróć uwagę, że to zadanie jest opcjonalne i można kontynuować bez jej wykonanie w laboratorium.**</span><span class="sxs-lookup"><span data-stu-id="d0549-491">**Notice that this task is optional and you can continue throughout the lab without executing it.**</span></span>

1. <span data-ttu-id="d0549-492">W programie Visual Studio, naciśnij klawisz **SHIFT** + **F5** można zatrzymać debugowania aplikacji.</span><span class="sxs-lookup"><span data-stu-id="d0549-492">In Visual Studio, press **SHIFT** + **F5** to stop debugging the application.</span></span>
2. <span data-ttu-id="d0549-493">Otwórz **Global.asax.cs** i dodaj następującą instrukcję using.</span><span class="sxs-lookup"><span data-stu-id="d0549-493">Open **Global.asax.cs** and add the following using statement.</span></span>


    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample15.cs)]
3. <span data-ttu-id="d0549-494">Dodaj następujący wyróżniony kod do aplikacji\_Start — metoda.</span><span class="sxs-lookup"><span data-stu-id="d0549-494">Add the following highlighted code into the Application\_Start method.</span></span>

    <span data-ttu-id="d0549-495">(Fragment - kodu *ASP.NET MVC 4 laboratorium - Ex03 — iPhone DisplayMode*)</span><span class="sxs-lookup"><span data-stu-id="d0549-495">(Code Snippet - *ASP.NET MVC 4 Lab - Ex03 - iPhone DisplayMode*)</span></span>


    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample16.cs)]

    <span data-ttu-id="d0549-496">Zarejestrowano nowe **DefaultDisplayMode o nazwie &quot;iPhone&quot;**, w ramach statycznych **DisplayModeProvider.Instance.Modes** listy statycznej pasujących przed każdego żądania przychodzącego.</span><span class="sxs-lookup"><span data-stu-id="d0549-496">You have registered a new **DefaultDisplayMode named &quot;iPhone&quot;**, within the static **DisplayModeProvider.Instance.Modes** static list, that will be matched against each incoming request.</span></span> <span data-ttu-id="d0549-497">Jeśli żądania przychodzącego, jeśli zawiera ciąg &quot;iPhone&quot;, ASP.NET MVC zostanie ustalone, widoki, których nazwa zawiera &quot;iPhone&quot; sufiks.</span><span class="sxs-lookup"><span data-stu-id="d0549-497">If the incoming request contains the string &quot;iPhone&quot;, ASP.NET MVC will find the views whose name contain the &quot;iPhone&quot; suffix.</span></span> <span data-ttu-id="d0549-498">Parametr 0 wskazuje, jak określone jest nowy tryb; na przykład, w tym widoku jest bardziej szczegółowy niż ogólne &quot;.mobile&quot; regułę, która odpowiada na żądania z urządzeń przenośnych.</span><span class="sxs-lookup"><span data-stu-id="d0549-498">The 0 parameter indicates how specific is the new mode; for instance, this view is more specific than the general &quot;.mobile&quot; rule that matches requests from mobile devices.</span></span>

    <span data-ttu-id="d0549-499">Po uruchomieniu tego kodu, jeśli iPhone przeglądarka generuje żądanie, aplikacja będzie korzystać z **Views\Shared\\_Layout.iPhone.cshtml** układu utworzysz w następnych krokach.</span><span class="sxs-lookup"><span data-stu-id="d0549-499">After this code runs, when an iPhone browser generates a request, your application will use the **Views\Shared\\_Layout.iPhone.cshtml** layout you will create in the next steps.</span></span>

    > [!NOTE]
    > <span data-ttu-id="d0549-500">Dzięki temu testowania żądania dla telefonów iPhone, zostały uproszczone dla celów demonstracyjnych i może nie działać zgodnie z oczekiwaniami dotyczącymi każdego iPhone ciąg agenta użytkownika (na przykład testu jest uwzględniana wielkość liter).</span><span class="sxs-lookup"><span data-stu-id="d0549-500">This way of testing the request for iPhone has been simplified for demo purposes and might not work as expected for every iPhone user agent string (for example test is case sensitive).</span></span>
4. <span data-ttu-id="d0549-501">Utwórz kopię  **\_Layout.Mobile.cshtml** w pliku **Views\Shared** folder i Zmień nazwę kopii do &quot;  **\_Layout.iPhone.csthml** &quot;.</span><span class="sxs-lookup"><span data-stu-id="d0549-501">Create a copy of the **\_Layout.Mobile.cshtml** file in the **Views\Shared** folder and rename the copy to &quot;**\_Layout.iPhone.csthml**&quot;.</span></span>
5. <span data-ttu-id="d0549-502">Otwórz  **\_Layout.iPhone.csthml** utworzony w poprzednim kroku.</span><span class="sxs-lookup"><span data-stu-id="d0549-502">Open **\_Layout.iPhone.csthml** you created in the previous step.</span></span>
6. <span data-ttu-id="d0549-503">Znajdź div element, używając atrybutu data-role ustawioną **strony** i zmienić **data-theme** atrybutu &quot; **a**&quot;.</span><span class="sxs-lookup"><span data-stu-id="d0549-503">Find the div element with the data-role attribute set to **page** and change the **data-theme** attribute to &quot;**a**&quot;.</span></span>


    [!code-cshtml[Main](whats-new-in-aspnet-mvc-4/samples/sample17.cshtml)]

    <span data-ttu-id="d0549-504">Teraz masz 3 układów w aplikacji ASP.NET MVC 4:</span><span class="sxs-lookup"><span data-stu-id="d0549-504">Now you have 3 layouts in your ASP.NET MVC 4 application:</span></span>

    1. <span data-ttu-id="d0549-505">**\_Layout.cshtml**: układ domyślny używany w przypadku przeglądarek komputerowych.</span><span class="sxs-lookup"><span data-stu-id="d0549-505">**\_Layout.cshtml**: default layout used for desktop browsers.</span></span>
    2. <span data-ttu-id="d0549-506">**\_Layout.Mobile.cshtml**: układ domyślny używany dla urządzeń przenośnych.</span><span class="sxs-lookup"><span data-stu-id="d0549-506">**\_Layout.mobile.cshtml**: default layout used for mobile devices.</span></span>
    3. <span data-ttu-id="d0549-507">**\_Layout.iPhone.cshtml**: układ określonych dla urządzenia iPhone, przy użyciu innego schematu kolorów do odróżnienia od \_Layout.mobile.cshtml.</span><span class="sxs-lookup"><span data-stu-id="d0549-507">**\_Layout.iPhone.cshtml**: specific layout for iPhone devices, using a different color scheme to differentiate from \_Layout.mobile.cshtml.</span></span>
7. <span data-ttu-id="d0549-508">Naciśnij klawisz **F5** uruchomić aplikację, a następnie przejdź do lokacji w **Emulator Windows Phone**.</span><span class="sxs-lookup"><span data-stu-id="d0549-508">Press **F5** to run the application and browse the site in the **Windows Phone Emulator**.</span></span>
8. <span data-ttu-id="d0549-509">Otwórz **symulatora telefonu iPhone** (zobacz [dodatku C](#AppendixC) instrukcje na temat instalowania i konfigurowania symulatora telefonu iPhone) i przejdź do witryny za.</span><span class="sxs-lookup"><span data-stu-id="d0549-509">Open an **iPhone simulator** (see [Appendix C](#AppendixC) for instructions on how to install and configure an iPhone simulator), and browse to the site too.</span></span> <span data-ttu-id="d0549-510">Zwróć uwagę, że każdy telefon przy użyciu określonego szablonu.</span><span class="sxs-lookup"><span data-stu-id="d0549-510">Notice that each phone is using the specific template.</span></span>

    ![Using-different-views-for-each-Mobile-device2](whats-new-in-aspnet-mvc-4/_static/image35.png)

    <span data-ttu-id="d0549-512">*Przy użyciu różnych widoków dla każdego urządzenia przenośnego*</span><span class="sxs-lookup"><span data-stu-id="d0549-512">*Using different views for each mobile device*</span></span>

<a id="Exercise4"></a>

<a id="Exercise_4_Using_Asynchronous_Controllers"></a>
### <a name="exercise-4-using-asynchronous-controllers"></a><span data-ttu-id="d0549-513">Ćwiczenie 4: Za pomocą kontrolerów asynchroniczne</span><span class="sxs-lookup"><span data-stu-id="d0549-513">Exercise 4: Using Asynchronous Controllers</span></span>

<span data-ttu-id="d0549-514">Microsoft .NET Framework 4.5 wprowadza nowe funkcje języka C# i Visual Basic zapewnia nowy podstawę dla asynchrony w programowaniu .NET.</span><span class="sxs-lookup"><span data-stu-id="d0549-514">Microsoft .NET Framework 4.5 introduces new language features in C# and Visual Basic to provide a new foundation for asynchrony in .NET programming.</span></span> <span data-ttu-id="d0549-515">Ten nowy foundation sprawia, że programowanie asynchroniczne podobne do - i około równie proste jak - synchroniczne programowania.</span><span class="sxs-lookup"><span data-stu-id="d0549-515">This new foundation makes asynchronous programming similar to - and about as straightforward as - synchronous programming.</span></span> <span data-ttu-id="d0549-516">Jesteś teraz możliwość zapisywania akcji asynchronicznej metody w technologii ASP.NET MVC 4 przy użyciu **AsyncController** klasy.</span><span class="sxs-lookup"><span data-stu-id="d0549-516">You are now able to write asynchronous action methods in ASP.NET MVC 4 by using the **AsyncController** class.</span></span> <span data-ttu-id="d0549-517">Za pomocą metod asynchronicznych akcji dla długotrwałe, niezwiązane z procesorem powiązany żądania.</span><span class="sxs-lookup"><span data-stu-id="d0549-517">You can use asynchronous action methods for long-running, non-CPU bound requests.</span></span> <span data-ttu-id="d0549-518">Dzięki temu można uniknąć blokowanie serwera sieci Web z wykonywania pracy podczas przetwarzania żądania.</span><span class="sxs-lookup"><span data-stu-id="d0549-518">This avoids blocking the Web server from performing work while the request is being processed.</span></span> <span data-ttu-id="d0549-519">Klasa AsyncController zazwyczaj jest używany dla wywołań usług sieci Web długotrwałe.</span><span class="sxs-lookup"><span data-stu-id="d0549-519">The AsyncController class is typically used for long-running Web service calls.</span></span>

<span data-ttu-id="d0549-520">Tego ćwiczenia przedstawiono podstawowe operację asynchroniczną na platformie ASP.NET MVC 4.</span><span class="sxs-lookup"><span data-stu-id="d0549-520">This exercise explains the basics of asynchronous operation in ASP.NET MVC 4.</span></span> <span data-ttu-id="d0549-521">Jeśli chcesz bardziej zgłębić temat, można wyewidencjonować artykule: [ [https://msdn.microsoft.com/en-us/library/ee728598%28v=vs.100%29.aspx](https://msdn.microsoft.com/en-us/library/ee728598%28v=vs.100%29.aspx)](https://msdn.microsoft.com/en-us/library/ee728598%28v=vs.100%29.aspx)</span><span class="sxs-lookup"><span data-stu-id="d0549-521">If you want a deeper dive, you can check out the following article: [[https://msdn.microsoft.com/en-us/library/ee728598%28v=vs.100%29.aspx](https://msdn.microsoft.com/en-us/library/ee728598%28v=vs.100%29.aspx)](https://msdn.microsoft.com/en-us/library/ee728598%28v=vs.100%29.aspx)</span></span>

<a id="Task_1_-_Implementing_an_Asynchronous_Controller"></a>
#### <a name="task-1---implementing-an-asynchronous-controller"></a><span data-ttu-id="d0549-522">Zadanie 1 - wdrażanie kontrolera asynchronicznego</span><span class="sxs-lookup"><span data-stu-id="d0549-522">Task 1 - Implementing an Asynchronous Controller</span></span>

1. <span data-ttu-id="d0549-523">Otwórz **rozpocząć** rozwiązania, znajdujących się na **źródło/Ex4-Async/Begin/** folderu.</span><span class="sxs-lookup"><span data-stu-id="d0549-523">Open the **Begin** solution located at **Source/Ex4-Async/Begin/** folder.</span></span> <span data-ttu-id="d0549-524">W przeciwnym razie możesz nadal korzystać **zakończenia** uzyskane rozwiązanie, wykonując poprzednim ćwiczeniu.</span><span class="sxs-lookup"><span data-stu-id="d0549-524">Otherwise, you might continue using the **End** solution obtained by completing the previous exercise.</span></span>

    1. <span data-ttu-id="d0549-525">Po otwarciu dostarczonych **rozpocząć** rozwiązania, musisz pobrać niektórych brakujących pakietów NuGet aby kontynuować.</span><span class="sxs-lookup"><span data-stu-id="d0549-525">If you opened the provided **Begin** solution, you will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="d0549-526">Aby to zrobić, kliknij przycisk **projektu** menu i wybierz **Zarządzaj pakietami NuGet**.</span><span class="sxs-lookup"><span data-stu-id="d0549-526">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
    2. <span data-ttu-id="d0549-527">W **Zarządzaj pakietami NuGet** okna dialogowego, kliknij przycisk **przywrócić** celu pobieranie brakujących pakietów.</span><span class="sxs-lookup"><span data-stu-id="d0549-527">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
    3. <span data-ttu-id="d0549-528">Na koniec Skompiluj rozwiązanie, klikając **kompilacji** | **Kompiluj rozwiązanie**.</span><span class="sxs-lookup"><span data-stu-id="d0549-528">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="d0549-529">Jedną z zalet przy użyciu narzędzia NuGet jest, że nie masz do wysłania wszystkich bibliotek w projekcie, zmniejszenie jego rozmiar projektu.</span><span class="sxs-lookup"><span data-stu-id="d0549-529">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="d0549-530">Narzędzia Power NuGet określając wersje pakietów w pliku Packages.config, będzie można pobrać wymaganych bibliotek podczas pierwszego uruchomienia projektu.</span><span class="sxs-lookup"><span data-stu-id="d0549-530">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="d0549-531">Jest to, dlaczego konieczne będzie wykonanie tych kroków, po otwarciu istniejącego rozwiązania z tego laboratorium.</span><span class="sxs-lookup"><span data-stu-id="d0549-531">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
2. <span data-ttu-id="d0549-532">Otwórz **HomeController.cs** klasę z **kontrolerów** folderu.</span><span class="sxs-lookup"><span data-stu-id="d0549-532">Open the **HomeController.cs** class from the **Controllers** folder.</span></span>
3. <span data-ttu-id="d0549-533">Dodaj następującą instrukcję using.</span><span class="sxs-lookup"><span data-stu-id="d0549-533">Add the following using statement.</span></span>


    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample18.cs)]
4. <span data-ttu-id="d0549-534">Aktualizacja **HomeController** klasa dziedziczy po **AsyncController**.</span><span class="sxs-lookup"><span data-stu-id="d0549-534">Update the **HomeController** class to inherit from **AsyncController**.</span></span> <span data-ttu-id="d0549-535">Kontrolery, które pochodzą z AsyncController włączyć platformę ASP.NET do przetwarzania żądań asynchronicznych i może nadal metod synchronicznych akcji usługi.</span><span class="sxs-lookup"><span data-stu-id="d0549-535">Controllers that derive from AsyncController enable ASP.NET to process asynchronous requests, and they can still service synchronous action methods.</span></span>


    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample19.cs)]
5. <span data-ttu-id="d0549-536">Dodaj **async** słowa kluczowego **indeksu** — metoda i przydzielić mu typ zwracany **zadań&lt;ActionResult&gt;**.</span><span class="sxs-lookup"><span data-stu-id="d0549-536">Add the **async** keyword to the **Index** method and make it return the type **Task&lt;ActionResult&gt;**.</span></span>


    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample20.cs)]

    > [!NOTE]
    > <span data-ttu-id="d0549-537">**Async** — słowo kluczowe jest jedną z nowych słów kluczowych, .NET Framework 4.5 zapewnia; go informuje kompilator, że ta metoda zawiera kod asynchroniczny.</span><span class="sxs-lookup"><span data-stu-id="d0549-537">The **async** keyword is one of the new keywords the .NET Framework 4.5 provides; it tells the compiler that this method contains asynchronous code.</span></span> <span data-ttu-id="d0549-538">A **zadań** obiekt reprezentuje operację asynchroniczną, która może zakończyć w pewnym momencie w przyszłości.</span><span class="sxs-lookup"><span data-stu-id="d0549-538">A **Task** object represents an asynchronous operation that may complete at some point in the future.</span></span>
6. <span data-ttu-id="d0549-539">Zastąp **klienta. GetAsync()** wywołanie za pomocą wersji pełnej async — słowo kluczowe await, jak pokazano poniżej.</span><span class="sxs-lookup"><span data-stu-id="d0549-539">Replace the **client.GetAsync()** call with the full async version using await keyword as shown below.</span></span>

    <span data-ttu-id="d0549-540">(Fragment - kodu *platformy ASP.NET MVC 4 laboratorium - Ex04 - GetAsync*)</span><span class="sxs-lookup"><span data-stu-id="d0549-540">(Code Snippet - *ASP.NET MVC 4 Lab - Ex04 - GetAsync*)</span></span>


    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample21.cs)]

    > [!NOTE]
    > <span data-ttu-id="d0549-541">W poprzedniej wersji, były używane **wynik** właściwość z **zadań** obiekt, aby zablokować wątek, dopóki wynik zostanie zwrócony (wersję usługi synchronizacji).</span><span class="sxs-lookup"><span data-stu-id="d0549-541">In the previous version, you were using the **Result** property from the **Task** object to block the thread until the result is returned (sync version).</span></span>
    > 
    > <span data-ttu-id="d0549-542">Dodawanie **await** — słowo kluczowe informuje kompilator, aby asynchronicznie poczekaj, aż zadanie zwrócone przez wywołanie metody.</span><span class="sxs-lookup"><span data-stu-id="d0549-542">Adding the **await** keyword tells the compiler to asynchronously wait for the task returned from the method call.</span></span> <span data-ttu-id="d0549-543">Oznacza to, że dalszej części kodu zostanie wykonany jako wywołanie zwrotne dopiero po zakończeniu Oczekiwano metody.</span><span class="sxs-lookup"><span data-stu-id="d0549-543">This means that the rest of the code will be executed as a callback only after the awaited method completes.</span></span> <span data-ttu-id="d0549-544">Jest innym czynnikiem, który należy zauważyć, że nie trzeba zmienić z bloku try-catch, aby umożliwić użycie tych wartości: wyjątki, które się tak zdarzyć w tle lub na pierwszym planie nadal zostanie przechwycony bez konieczności wykonywania dodatkowych działań za pomocą obsługi dostarczane przez platformę.</span><span class="sxs-lookup"><span data-stu-id="d0549-544">Another thing to notice is that you do not need to change your try-catch block in order to make this work: the exceptions that happen in background or in foreground will still be caught without any extra work using a handler provided by the framework.</span></span>
7. <span data-ttu-id="d0549-545">Zmień kod, aby kontynuować z implementacją asynchroniczne przez zamianę wiersze nowy kod w sposób przedstawiony poniżej</span><span class="sxs-lookup"><span data-stu-id="d0549-545">Change the code to continue with the asynchronous implementation by replacing the lines with the new code as shown below</span></span>

    <span data-ttu-id="d0549-546">(Fragment - kodu *platformy ASP.NET MVC 4 laboratorium - Ex04 - ReadAsStringAsync*)</span><span class="sxs-lookup"><span data-stu-id="d0549-546">(Code Snippet - *ASP.NET MVC 4 Lab - Ex04 - ReadAsStringAsync*)</span></span>


    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample22.cs)]
8. <span data-ttu-id="d0549-547">Uruchom aplikację.</span><span class="sxs-lookup"><span data-stu-id="d0549-547">Run the application.</span></span> <span data-ttu-id="d0549-548">Można zauważyć nie większych zmian, ale kodu nie powoduje blokowania wątków z puli wątków wprowadzania czy lepsze wykorzystanie zasobów serwera i poprawia wydajność.</span><span class="sxs-lookup"><span data-stu-id="d0549-548">You will notice no major changes, but your code will not block a thread from the thread pool making a better usage of the server resources and improving performance.</span></span>

    > [!NOTE]
    > <span data-ttu-id="d0549-549">Dowiedz się więcej o nowe funkcje programowania asynchronicznego w laboratorium &quot; **programowania asynchronicznego w programie .NET 4.5 z C# i Visual Basic** &quot; zawarte w zestawie szkolenia programu Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="d0549-549">You can learn more about the new asynchronous programming features in the lab &quot;**Asynchronous Programming in .NET 4.5 with C# and Visual Basic**&quot; included in the Visual Studio Training Kit.</span></span>

<a id="Task_2_-_Handling_Time-Outs_with_Cancellation_Tokens"></a>
#### <a name="task-2---handling-time-outs-with-cancellation-tokens"></a><span data-ttu-id="d0549-550">Zadanie 2 — limity czasu obsługi o anulowanie tokenów</span><span class="sxs-lookup"><span data-stu-id="d0549-550">Task 2 - Handling Time-Outs with Cancellation Tokens</span></span>

<span data-ttu-id="d0549-551">Metody asynchroniczne akcji, które zwracają wystąpienia obiektów Task może również obsługiwać przekroczeń limitu czasu.</span><span class="sxs-lookup"><span data-stu-id="d0549-551">Asynchronous action methods that return Task instances can also support time-outs.</span></span> <span data-ttu-id="d0549-552">To zadanie zaktualizuje indeksu kodu metody do obsługi scenariusza limitu czasu przy użyciu token anulowania.</span><span class="sxs-lookup"><span data-stu-id="d0549-552">In this task, you will update the Index method code to handle a time-out scenario using a cancellation token.</span></span>

1. <span data-ttu-id="d0549-553">Wróć do programu Visual Studio i naciśnij klawisz **SHIFT + F5** można zatrzymać debugowania.</span><span class="sxs-lookup"><span data-stu-id="d0549-553">Go back to Visual Studio and press **SHIFT + F5** to stop debugging.</span></span>
2. <span data-ttu-id="d0549-554">Dodaj następujący kod za pomocą instrukcji do **HomeController.cs** pliku.</span><span class="sxs-lookup"><span data-stu-id="d0549-554">Add the following using statement to the **HomeController.cs** file.</span></span>


    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample23.cs)]
3. <span data-ttu-id="d0549-555">Akcja indeksu do otrzymywania aktualizacji **CancellationToken** argumentu.</span><span class="sxs-lookup"><span data-stu-id="d0549-555">Update the Index action to receive a **CancellationToken** argument.</span></span>


    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample24.cs)]
4. <span data-ttu-id="d0549-556">Aktualizacja **GetAsync** wywołania do przekazania token anulowania.</span><span class="sxs-lookup"><span data-stu-id="d0549-556">Update the **GetAsync** call to pass the cancellation token.</span></span>

    <span data-ttu-id="d0549-557">(Fragment - kodu *SendAsync laboratorium - Ex04 - platformy ASP.NET MVC 4 z CancellationToken*)</span><span class="sxs-lookup"><span data-stu-id="d0549-557">(Code Snippet - *ASP.NET MVC 4 Lab - Ex04 - SendAsync with CancellationToken*)</span></span>


    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample25.cs)]
5. <span data-ttu-id="d0549-558">Dekoracji *indeksu* metody z **Wartość AsyncTimeout** atrybutu ustawić milisekund 500 i **HandleError** atrybutu skonfigurowane do obsługi  **TaskCanceledException** przez przekierowywanie do **upłynął limit czasu** widoku.</span><span class="sxs-lookup"><span data-stu-id="d0549-558">Decorate the *Index* method with an **AsyncTimeout** attribute set to 500 milliseconds and a **HandleError** attribute configured to handle **TaskCanceledException** by redirecting to a **TimedOut** view.</span></span>

    <span data-ttu-id="d0549-559">(Fragment - kodu *atrybuty laboratorium - Ex04 - platformy ASP.NET MVC 4*)</span><span class="sxs-lookup"><span data-stu-id="d0549-559">(Code Snippet - *ASP.NET MVC 4 Lab - Ex04 - Attributes*)</span></span>


    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample26.cs)]
6. <span data-ttu-id="d0549-560">Otwórz **PhotoController** klasy i aktualizacji **galerii** metody opóźnienia miliseconds wykonywania 1000 (1 sekunda), aby symulować długotrwałych zadań.</span><span class="sxs-lookup"><span data-stu-id="d0549-560">Open the **PhotoController** class and update the **Gallery** method to delay the execution 1000 miliseconds (1 second) to simulate a long running task.</span></span>


    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample27.cs)]
7. <span data-ttu-id="d0549-561">Otwórz **Web.config** plików i włączyć błędów niestandardowych, dodając następujący element.</span><span class="sxs-lookup"><span data-stu-id="d0549-561">Open the **Web.config** file and enable custom errors by adding the following element.</span></span>


    [!code-xml[Main](whats-new-in-aspnet-mvc-4/samples/sample28.xml)]
8. <span data-ttu-id="d0549-562">Utwórz nowy widok w **Views\Shared** o nazwie **upłynął limit czasu** i używać domyślnego układu.</span><span class="sxs-lookup"><span data-stu-id="d0549-562">Create a new view in **Views\Shared** named **TimedOut** and use the default layout.</span></span> <span data-ttu-id="d0549-563">W Eksploratorze rozwiązań kliknij prawym przyciskiem myszy **Views\Shared** i wybierz polecenie **Dodaj | Widok**.</span><span class="sxs-lookup"><span data-stu-id="d0549-563">In the Solution Explorer, right-click the **Views\Shared** folder and select **Add | View**.</span></span>

    <span data-ttu-id="d0549-564">![Na każdym urządzeniu przenośnym, przy użyciu różnych widoków](whats-new-in-aspnet-mvc-4/_static/image36.png "przy użyciu różnych widoków dla każdego urządzenia przenośnego")</span><span class="sxs-lookup"><span data-stu-id="d0549-564">![Using different views for each mobile device](whats-new-in-aspnet-mvc-4/_static/image36.png "Using different views for each mobile device")</span></span>

    <span data-ttu-id="d0549-565">*Przy użyciu różnych widoków dla każdego urządzenia przenośnego*</span><span class="sxs-lookup"><span data-stu-id="d0549-565">*Using different views for each mobile device*</span></span>
9. <span data-ttu-id="d0549-566">Aktualizacja **upłynął limit czasu** wyświetlania zawartości, jak pokazano poniżej.</span><span class="sxs-lookup"><span data-stu-id="d0549-566">Update the **TimedOut** view content as shown below.</span></span>


    [!code-cshtml[Main](whats-new-in-aspnet-mvc-4/samples/sample29.cshtml)]
10. <span data-ttu-id="d0549-567">Uruchom aplikację i przejdź do adresu URL katalogu głównego.</span><span class="sxs-lookup"><span data-stu-id="d0549-567">Run the application and navigate to the root URL.</span></span> <span data-ttu-id="d0549-568">Jak dodano **Thread.Sleep** 1000 milisekund, wystąpi błąd limitu czasu, generowane przez **Wartość AsyncTimeout** atrybutu i catch przez **HandleError** atrybut.</span><span class="sxs-lookup"><span data-stu-id="d0549-568">As you have added a **Thread.Sleep** of 1000 milliseconds, you will get a time-out error, generated by the **AsyncTimeout** attribute and catch by the **HandleError** attribute.</span></span>

    <span data-ttu-id="d0549-569">![Wyjątek limitu czasu obsługi](whats-new-in-aspnet-mvc-4/_static/image37.png "wyjątek limitu czasu obsługi")</span><span class="sxs-lookup"><span data-stu-id="d0549-569">![Time-out exception handled](whats-new-in-aspnet-mvc-4/_static/image37.png "Time-out exception handled")</span></span>

    <span data-ttu-id="d0549-570">*Wyjątek limitu czasu obsługi*</span><span class="sxs-lookup"><span data-stu-id="d0549-570">*Time-out exception handled*</span></span>

> [!NOTE]
> <span data-ttu-id="d0549-571">Ponadto można wdrożyć tę aplikację systemu Windows Azure Web Sites następujących [dodatek D: publikowania aplikacji ASP.NET MVC 4 przy użyciu narzędzia Web Deploy](#AppendixD).</span><span class="sxs-lookup"><span data-stu-id="d0549-571">Additionally, you can deploy this application to Windows Azure Web Sites following [Appendix D: Publishing an ASP.NET MVC 4 Application using Web Deploy](#AppendixD).</span></span>


<a id="Summary"></a>

<a id="Summary"></a>
## <a name="summary"></a><span data-ttu-id="d0549-572">Podsumowanie</span><span class="sxs-lookup"><span data-stu-id="d0549-572">Summary</span></span>

<span data-ttu-id="d0549-573">W tym hands-na-laboratorium możesz już przestrzegane niektóre z nowych funkcji znajdują się w technologii ASP.NET MVC 4.</span><span class="sxs-lookup"><span data-stu-id="d0549-573">In this hands-on-lab, you've observed some of the new features resident in ASP.NET MVC 4.</span></span> <span data-ttu-id="d0549-574">Zostały poddane dyskusji następujące kwestie:</span><span class="sxs-lookup"><span data-stu-id="d0549-574">The following concepts have been discussed:</span></span>

- <span data-ttu-id="d0549-575">Zawiera ulepszenia do platformy ASP.NET MVC projektu szablony tym szablonie projektu aplikacji mobilnej</span><span class="sxs-lookup"><span data-stu-id="d0549-575">Take advantage of the enhancements to the ASP.NET MVC project templates-including the new mobile application project template</span></span>
- <span data-ttu-id="d0549-576">Za pomocą atrybutu okienka ekranu HTML5 i zapytaniami multimediów CSS zwiększyć wyświetlania na urządzeniach przenośnych</span><span class="sxs-lookup"><span data-stu-id="d0549-576">Use the HTML5 viewport attribute and CSS media queries to improve the display on mobile devices</span></span>
- <span data-ttu-id="d0549-577">Ulepszenia progresywnego oraz tworzenie zoptymalizowanych pod kątem touch interfejsu użytkownika sieci web mieć jQuery Mobile</span><span class="sxs-lookup"><span data-stu-id="d0549-577">Use jQuery Mobile for progressive enhancements and for building touch-optimized web UI</span></span>
- <span data-ttu-id="d0549-578">Utwórz widoki specyficzne dla mobile</span><span class="sxs-lookup"><span data-stu-id="d0549-578">Create mobile-specific views</span></span>
- <span data-ttu-id="d0549-579">Korzystanie ze składnika tym przełącznikiem widoku do przełączania się między widokami aplikacji mobilnych i klasycznych</span><span class="sxs-lookup"><span data-stu-id="d0549-579">Use the view-switcher component to toggle between mobile and desktop views in the application</span></span>
- <span data-ttu-id="d0549-580">Utwórz asynchroniczne kontrolerów przy użyciu zadań pomocy technicznej</span><span class="sxs-lookup"><span data-stu-id="d0549-580">Create asynchronous controllers using task support</span></span>

<a id="AppendixA"></a>

<a id="Appendix_A_Using_Code_Snippets"></a>
## <a name="appendix-a-using-code-snippets"></a><span data-ttu-id="d0549-581">Dodatek A: korzystania z wstawek kodu</span><span class="sxs-lookup"><span data-stu-id="d0549-581">Appendix A: Using Code Snippets</span></span>

<span data-ttu-id="d0549-582">Wstawki kodu zapewniają całego kodu, które są potrzebne w zasięgu ręki.</span><span class="sxs-lookup"><span data-stu-id="d0549-582">With code snippets, you have all the code you need at your fingertips.</span></span> <span data-ttu-id="d0549-583">Dokument laboratorium informuje o dokładnie po ich użycia, jak pokazano na poniższej ilustracji.</span><span class="sxs-lookup"><span data-stu-id="d0549-583">The lab document will tell you exactly when you can use them, as shown in the following figure.</span></span>

<span data-ttu-id="d0549-584">![Aby wstawić kod do projektu przy użyciu wstawki kodu programu Visual Studio](whats-new-in-aspnet-mvc-4/_static/image38.png "wstawki kodu za pomocą programu Visual Studio, aby wstawić kod do projektu")</span><span class="sxs-lookup"><span data-stu-id="d0549-584">![Using Visual Studio code snippets to insert code into your project](whats-new-in-aspnet-mvc-4/_static/image38.png "Using Visual Studio code snippets to insert code into your project")</span></span>

<span data-ttu-id="d0549-585">*Aby wstawić kod do projektu przy użyciu wstawki kodu programu Visual Studio*</span><span class="sxs-lookup"><span data-stu-id="d0549-585">*Using Visual Studio code snippets to insert code into your project*</span></span>

<span data-ttu-id="d0549-586">***Aby dodać fragment kodu za pomocą klawiatury (C# tylko)***</span><span class="sxs-lookup"><span data-stu-id="d0549-586">***To add a code snippet using the keyboard (C# only)***</span></span>

1. <span data-ttu-id="d0549-587">Umieść kursor, w którym chcesz wstawić kod.</span><span class="sxs-lookup"><span data-stu-id="d0549-587">Place the cursor where you would like to insert the code.</span></span>
2. <span data-ttu-id="d0549-588">Zacznij wpisywać nazwę fragment (bez spacji i łączniki).</span><span class="sxs-lookup"><span data-stu-id="d0549-588">Start typing the snippet name (without spaces or hyphens).</span></span>
3. <span data-ttu-id="d0549-589">Obejrzyj jako IntelliSense wyświetla zgodne z nazwami wstawki.</span><span class="sxs-lookup"><span data-stu-id="d0549-589">Watch as IntelliSense displays matching snippets' names.</span></span>
4. <span data-ttu-id="d0549-590">Wybierz prawidłowe fragment (lub zachować wpisywanie do momentu wybrania fragment całą nazwę).</span><span class="sxs-lookup"><span data-stu-id="d0549-590">Select the correct snippet (or keep typing until the entire snippet's name is selected).</span></span>
5. <span data-ttu-id="d0549-591">Naciśnij klawisz Tab dwa razy, aby wstawić fragment kodu w lokalizacji kursora.</span><span class="sxs-lookup"><span data-stu-id="d0549-591">Press the Tab key twice to insert the snippet at the cursor location.</span></span>

<span data-ttu-id="d0549-592">![Rozpocznij wpisywanie nazwy fragment](whats-new-in-aspnet-mvc-4/_static/image39.png "zacznij wpisywać nazwę wstawki programu")</span><span class="sxs-lookup"><span data-stu-id="d0549-592">![Start typing the snippet name](whats-new-in-aspnet-mvc-4/_static/image39.png "Start typing the snippet name")</span></span>

<span data-ttu-id="d0549-593">*Rozpocznij wpisywanie nazwy fragment kodu*</span><span class="sxs-lookup"><span data-stu-id="d0549-593">*Start typing the snippet name*</span></span>

<span data-ttu-id="d0549-594">![Naciśnij klawisz Tab, aby wybrać wyróżnione fragment](whats-new-in-aspnet-mvc-4/_static/image40.png "naciśnij klawisz Tab, aby wybrać wyróżnione fragment kodu")</span><span class="sxs-lookup"><span data-stu-id="d0549-594">![Press Tab to select the highlighted snippet](whats-new-in-aspnet-mvc-4/_static/image40.png "Press Tab to select the highlighted snippet")</span></span>

<span data-ttu-id="d0549-595">*Naciśnij klawisz Tab, aby wybrać wyróżnione fragment kodu*</span><span class="sxs-lookup"><span data-stu-id="d0549-595">*Press Tab to select the highlighted snippet*</span></span>

<span data-ttu-id="d0549-596">![Ponownie naciśnij klawisz Tab i fragment rozwinie](whats-new-in-aspnet-mvc-4/_static/image41.png "rozwinie ponownie naciśnij klawisz Tab i wstawki kodu")</span><span class="sxs-lookup"><span data-stu-id="d0549-596">![Press Tab again and the snippet will expand](whats-new-in-aspnet-mvc-4/_static/image41.png "Press Tab again and the snippet will expand")</span></span>

<span data-ttu-id="d0549-597">*Rozwinie ponownie naciśnij klawisz Tab i wstawki kodu*</span><span class="sxs-lookup"><span data-stu-id="d0549-597">*Press Tab again and the snippet will expand*</span></span>

<span data-ttu-id="d0549-598">***Aby dodać fragment kodu za pomocą myszy (C#, Visual Basic i XML)***</span><span class="sxs-lookup"><span data-stu-id="d0549-598">***To add a code snippet using the mouse (C#, Visual Basic and XML)***</span></span>

1. <span data-ttu-id="d0549-599">Kliknij prawym przyciskiem myszy, gdy chcesz wstawić fragment kodu.</span><span class="sxs-lookup"><span data-stu-id="d0549-599">Right-click where you want to insert the code snippet.</span></span>
2. <span data-ttu-id="d0549-600">Wybierz **wstawić fragment** następuje **Moje wstawki kodu**.</span><span class="sxs-lookup"><span data-stu-id="d0549-600">Select **Insert Snippet** followed by **My Code Snippets**.</span></span>
3. <span data-ttu-id="d0549-601">Wybierz odpowiedni fragment z listy, klikając go.</span><span class="sxs-lookup"><span data-stu-id="d0549-601">Pick the relevant snippet from the list, by clicking on it.</span></span>

<span data-ttu-id="d0549-602">![Kliknij prawym przyciskiem myszy, gdy chcesz wstawić fragment kodu i wybierz wstawić fragment](whats-new-in-aspnet-mvc-4/_static/image42.png "kliknij prawym przyciskiem myszy, gdy chcesz wstawić fragment kodu i wybierz wstawić fragment kodu")</span><span class="sxs-lookup"><span data-stu-id="d0549-602">![Right-click where you want to insert the code snippet and select Insert Snippet](whats-new-in-aspnet-mvc-4/_static/image42.png "Right-click where you want to insert the code snippet and select Insert Snippet")</span></span>

<span data-ttu-id="d0549-603">*Kliknij prawym przyciskiem myszy, gdy chcesz wstawić fragment kodu i wybierz wstawić fragment kodu*</span><span class="sxs-lookup"><span data-stu-id="d0549-603">*Right-click where you want to insert the code snippet and select Insert Snippet*</span></span>

<span data-ttu-id="d0549-604">![Wybierz odpowiedni fragment z listy, klikając go](whats-new-in-aspnet-mvc-4/_static/image43.png "wybierz odpowiedni fragment z listy, klikając go")</span><span class="sxs-lookup"><span data-stu-id="d0549-604">![Pick the relevant snippet from the list, by clicking on it](whats-new-in-aspnet-mvc-4/_static/image43.png "Pick the relevant snippet from the list, by clicking on it")</span></span>

<span data-ttu-id="d0549-605">*Wybierz odpowiedni fragment z listy, klikając go*</span><span class="sxs-lookup"><span data-stu-id="d0549-605">*Pick the relevant snippet from the list, by clicking on it*</span></span>

<a id="AppendixB"></a>

<a id="Appendix_B_Installing_Visual_Studio_Express_2012_for_Web"></a>
## <a name="appendix-b-installing-visual-studio-express-2012-for-web"></a><span data-ttu-id="d0549-606">Dodatek B: Instalowanie programu Visual Studio Express 2012 for Web</span><span class="sxs-lookup"><span data-stu-id="d0549-606">Appendix B: Installing Visual Studio Express 2012 for Web</span></span>

<span data-ttu-id="d0549-607">Można zainstalować **Microsoft Visual Studio Express 2012 for Web** lub innym &quot;Express&quot; przy użyciu wersji  **[Instalatora platformy sieci Web firmy Microsoft](https://www.microsoft.com/web/downloads/platform.aspx)** .</span><span class="sxs-lookup"><span data-stu-id="d0549-607">You can install **Microsoft Visual Studio Express 2012 for Web** or another &quot;Express&quot; version using the **[Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)**.</span></span> <span data-ttu-id="d0549-608">Poniższe instrukcje przedstawiono czynności wymagane do zainstalowania *programu Visual studio Express 2012 for Web* przy użyciu *Instalatora platformy sieci Web firmy Microsoft*.</span><span class="sxs-lookup"><span data-stu-id="d0549-608">The following instructions guide you through the steps required to install *Visual studio Express 2012 for Web* using *Microsoft Web Platform Installer*.</span></span>

1. <span data-ttu-id="d0549-609">Przejdź do [ [https://go.microsoft.com/?linkid=9810169](https://go.microsoft.com/?linkid=9810169)](https://go.microsoft.com/?linkid=9810169).</span><span class="sxs-lookup"><span data-stu-id="d0549-609">Go to [[https://go.microsoft.com/?linkid=9810169](https://go.microsoft.com/?linkid=9810169)](https://go.microsoft.com/?linkid=9810169).</span></span> <span data-ttu-id="d0549-610">Alternatywnie, jeśli została już zainstalowana Instalatora platformy sieci Web, można otworzyć go i Wyszukaj produkt &quot; *programu Visual Studio Express 2012 for Web z zestawem Windows Azure SDK*&quot;.</span><span class="sxs-lookup"><span data-stu-id="d0549-610">Alternatively, if you already have installed Web Platform Installer, you can open it and search for the product &quot;*Visual Studio Express 2012 for Web with Windows Azure SDK*&quot;.</span></span>
2. <span data-ttu-id="d0549-611">Polecenie **teraz zainstalować**.</span><span class="sxs-lookup"><span data-stu-id="d0549-611">Click on **Install Now**.</span></span> <span data-ttu-id="d0549-612">Jeśli nie masz **Instalatora platformy sieci Web** nastąpi przekierowanie do pobrania i zainstalowania go najpierw.</span><span class="sxs-lookup"><span data-stu-id="d0549-612">If you do not have **Web Platform Installer** you will be redirected to download and install it first.</span></span>
3. <span data-ttu-id="d0549-613">Raz **Instalatora platformy sieci Web** jest otwarty, kliknij przycisk **zainstalować** można uruchomić Instalatora.</span><span class="sxs-lookup"><span data-stu-id="d0549-613">Once **Web Platform Installer** is open, click **Install** to start the setup.</span></span>

    <span data-ttu-id="d0549-614">![Instalowanie programu Visual Studio Express](whats-new-in-aspnet-mvc-4/_static/image44.png "instalacji programu Visual Studio Express")</span><span class="sxs-lookup"><span data-stu-id="d0549-614">![Install Visual Studio Express](whats-new-in-aspnet-mvc-4/_static/image44.png "Install Visual Studio Express")</span></span>

    <span data-ttu-id="d0549-615">*Instalowanie programu Visual Studio Express*</span><span class="sxs-lookup"><span data-stu-id="d0549-615">*Install Visual Studio Express*</span></span>
4. <span data-ttu-id="d0549-616">Odczytywanie wszystkich produktów licencji i warunków, a następnie kliknij przycisk **akceptuję** aby kontynuować.</span><span class="sxs-lookup"><span data-stu-id="d0549-616">Read all the products' licenses and terms and click **I Accept** to continue.</span></span>

    ![Akceptowanie umowy licencyjnej](whats-new-in-aspnet-mvc-4/_static/image45.png)

    <span data-ttu-id="d0549-618">*Akceptowanie umowy licencyjnej*</span><span class="sxs-lookup"><span data-stu-id="d0549-618">*Accepting the license terms*</span></span>
5. <span data-ttu-id="d0549-619">Poczekaj na zakończenie procesu pobierania i instalacji.</span><span class="sxs-lookup"><span data-stu-id="d0549-619">Wait until the downloading and installation process completes.</span></span>

    ![Postęp instalacji](whats-new-in-aspnet-mvc-4/_static/image46.png)

    <span data-ttu-id="d0549-621">*Postęp instalacji*</span><span class="sxs-lookup"><span data-stu-id="d0549-621">*Installation progress*</span></span>
6. <span data-ttu-id="d0549-622">Po zakończeniu instalacji kliknij przycisk **Zakończ**.</span><span class="sxs-lookup"><span data-stu-id="d0549-622">When the installation completes, click **Finish**.</span></span>

    ![Instalacja została zakończona](whats-new-in-aspnet-mvc-4/_static/image47.png)

    <span data-ttu-id="d0549-624">*Instalacja została zakończona*</span><span class="sxs-lookup"><span data-stu-id="d0549-624">*Installation completed*</span></span>
7. <span data-ttu-id="d0549-625">Kliknij przycisk **zakończenia** aby zamknąć Instalatora platformy sieci Web.</span><span class="sxs-lookup"><span data-stu-id="d0549-625">Click **Exit** to close Web Platform Installer.</span></span>
8. <span data-ttu-id="d0549-626">Aby otworzyć program Visual Studio Express for Web, przejdź do **Start** ekranu i zacznij pisać &quot; **VS Express**&quot;, następnie kliknij polecenie **VS Express for Web** Kafelek.</span><span class="sxs-lookup"><span data-stu-id="d0549-626">To open Visual Studio Express for Web, go to the **Start** screen and start writing &quot;**VS Express**&quot;, then click on the **VS Express for Web** tile.</span></span>

    ![VS Express for Web kafelka](whats-new-in-aspnet-mvc-4/_static/image48.png)

    <span data-ttu-id="d0549-628">*VS Express for Web kafelka*</span><span class="sxs-lookup"><span data-stu-id="d0549-628">*VS Express for Web tile*</span></span>

<a id="AppendixC"></a>

<a id="Appendix_C_Installing_WebMatrix_2_and_iPhone_Simulator"></a>
## <a name="appendix-c-installing-webmatrix-2-and-iphone-simulator"></a><span data-ttu-id="d0549-629">Dodatek C: Instalowanie programu WebMatrix 2 i iPhone symulatora</span><span class="sxs-lookup"><span data-stu-id="d0549-629">Appendix C: Installing WebMatrix 2 and iPhone Simulator</span></span>

<span data-ttu-id="d0549-630">Działanie witryny na urządzeniu iPhone symulowane można użyć rozszerzenia programu WebMatrix &quot;Electric symulatora mobilnych dla telefonów iPhone&quot;.</span><span class="sxs-lookup"><span data-stu-id="d0549-630">To run your site in a simulated iPhone device you can use the WebMatrix extension &quot;Electric Mobile Simulator for the iPhone&quot;.</span></span> <span data-ttu-id="d0549-631">Ponadto można skonfigurować te same rozszerzeń do uruchamiania symulatora z programu Visual Studio 2012.</span><span class="sxs-lookup"><span data-stu-id="d0549-631">Also, you can configure the same extension to run the simulator from Visual Studio 2012.</span></span>

<a id="ApxCTask1"></a>

<a id="Task_1_-_Installing_WebMatrix_2"></a>
#### <a name="task-1---installing-webmatrix-2"></a><span data-ttu-id="d0549-632">Zadanie 1 — Instalowanie programu WebMatrix 2</span><span class="sxs-lookup"><span data-stu-id="d0549-632">Task 1 - Installing WebMatrix 2</span></span>

1. <span data-ttu-id="d0549-633">Przejdź do [ [https://go.microsoft.com/?linkid=9809776](https://go.microsoft.com/?linkid=9809776)](https://go.microsoft.com/?linkid=9810169).</span><span class="sxs-lookup"><span data-stu-id="d0549-633">Go to [[https://go.microsoft.com/?linkid=9809776](https://go.microsoft.com/?linkid=9809776)](https://go.microsoft.com/?linkid=9810169).</span></span> <span data-ttu-id="d0549-634">Alternatywnie, jeśli została już zainstalowana Instalatora platformy sieci Web, można otworzyć go i Wyszukaj produkt &quot; *programu WebMatrix 2*&quot;.</span><span class="sxs-lookup"><span data-stu-id="d0549-634">Alternatively, if you already have installed Web Platform Installer, you can open it and search for the product &quot;*WebMatrix 2*&quot;.</span></span>
2. <span data-ttu-id="d0549-635">Polecenie **teraz zainstalować**.</span><span class="sxs-lookup"><span data-stu-id="d0549-635">Click on **Install Now**.</span></span> <span data-ttu-id="d0549-636">Jeśli nie masz **Instalatora platformy sieci Web** nastąpi przekierowanie do pobrania i zainstalowania go najpierw.</span><span class="sxs-lookup"><span data-stu-id="d0549-636">If you do not have **Web Platform Installer** you will be redirected to download and install it first.</span></span>
3. <span data-ttu-id="d0549-637">Raz **Instalatora platformy sieci Web** jest otwarty, kliknij przycisk **zainstalować** można uruchomić Instalatora.</span><span class="sxs-lookup"><span data-stu-id="d0549-637">Once **Web Platform Installer** is open, click **Install** to start the setup.</span></span>

    <span data-ttu-id="d0549-638">![Zainstaluj program WebMatrix 2](whats-new-in-aspnet-mvc-4/_static/image49.png "zainstalować program WebMatrix 2")</span><span class="sxs-lookup"><span data-stu-id="d0549-638">![Install WebMatrix 2](whats-new-in-aspnet-mvc-4/_static/image49.png "Install WebMatrix 2")</span></span>

    <span data-ttu-id="d0549-639">*Instalowanie programu WebMatrix 2*</span><span class="sxs-lookup"><span data-stu-id="d0549-639">*Install WebMatrix 2*</span></span>
4. <span data-ttu-id="d0549-640">Odczytywanie wszystkich produktów licencji i warunków, a następnie kliknij przycisk **akceptuję** aby kontynuować.</span><span class="sxs-lookup"><span data-stu-id="d0549-640">Read all the products' licenses and terms and click **I Accept** to continue.</span></span>

    <span data-ttu-id="d0549-641">![Akceptowanie umowy licencyjnej](whats-new-in-aspnet-mvc-4/_static/image50.png "akceptowanie umowy licencyjnej")</span><span class="sxs-lookup"><span data-stu-id="d0549-641">![Accepting the license terms](whats-new-in-aspnet-mvc-4/_static/image50.png "Accepting the license terms")</span></span>

    <span data-ttu-id="d0549-642">*Akceptowanie umowy licencyjnej*</span><span class="sxs-lookup"><span data-stu-id="d0549-642">*Accepting the license terms*</span></span>
5. <span data-ttu-id="d0549-643">Poczekaj na zakończenie procesu pobierania i instalacji.</span><span class="sxs-lookup"><span data-stu-id="d0549-643">Wait until the downloading and installation process completes.</span></span>

    <span data-ttu-id="d0549-644">![Postęp instalacji](whats-new-in-aspnet-mvc-4/_static/image51.png "postęp instalacji")</span><span class="sxs-lookup"><span data-stu-id="d0549-644">![Installation progress](whats-new-in-aspnet-mvc-4/_static/image51.png "Installation progress")</span></span>

    <span data-ttu-id="d0549-645">*Postęp instalacji*</span><span class="sxs-lookup"><span data-stu-id="d0549-645">*Installation progress*</span></span>
6. <span data-ttu-id="d0549-646">Po zakończeniu instalacji kliknij przycisk **Zakończ**.</span><span class="sxs-lookup"><span data-stu-id="d0549-646">When the installation completes, click **Finish**.</span></span>

    <span data-ttu-id="d0549-647">![Instalacja została zakończona](whats-new-in-aspnet-mvc-4/_static/image52.png "instalacja została zakończona")</span><span class="sxs-lookup"><span data-stu-id="d0549-647">![Installation completed](whats-new-in-aspnet-mvc-4/_static/image52.png "Installation completed")</span></span>

    <span data-ttu-id="d0549-648">*Instalacja została zakończona*</span><span class="sxs-lookup"><span data-stu-id="d0549-648">*Installation completed*</span></span>
7. <span data-ttu-id="d0549-649">Kliknij przycisk **zakończenia** aby zamknąć Instalatora platformy sieci Web.</span><span class="sxs-lookup"><span data-stu-id="d0549-649">Click **Exit** to close Web Platform Installer.</span></span>

<a id="ApxCTask2"></a>

<a id="Task_2_-_Installing_the_iPhone_Simulator_Extension"></a>
#### <a name="task-2---installing-the-iphone-simulator-extension"></a><span data-ttu-id="d0549-650">Zadanie 2 — Instalowanie iPhone symulatora rozszerzenia</span><span class="sxs-lookup"><span data-stu-id="d0549-650">Task 2 - Installing the iPhone Simulator Extension</span></span>

1. <span data-ttu-id="d0549-651">Uruchom **WebMatrix** i Otwórz istniejącą witrynę sieci Web lub Utwórz nową.</span><span class="sxs-lookup"><span data-stu-id="d0549-651">Run **WebMatrix** and open any existing Web site or create a new one.</span></span>
2. <span data-ttu-id="d0549-652">Kliknij przycisk **Uruchom** przycisk z **Home** Wstążki i wybierz **Dodaj nowe**.</span><span class="sxs-lookup"><span data-stu-id="d0549-652">Click the **Run** button from the **Home** ribbon and select **Add new**.</span></span>

    <span data-ttu-id="d0549-653">![Dodawanie nowego rozszerzenia programu WebMatrix](whats-new-in-aspnet-mvc-4/_static/image53.png "dodanie nowego rozszerzenia programu WebMatrix")</span><span class="sxs-lookup"><span data-stu-id="d0549-653">![Adding new WebMatrix extension](whats-new-in-aspnet-mvc-4/_static/image53.png "Adding new WebMatrix extension")</span></span>

    <span data-ttu-id="d0549-654">*Dodawanie nowego rozszerzenia programu WebMatrix*</span><span class="sxs-lookup"><span data-stu-id="d0549-654">*Adding new WebMatrix extension*</span></span>
3. <span data-ttu-id="d0549-655">Wybierz **iPhone symulatora** i kliknij przycisk **zainstalować**.</span><span class="sxs-lookup"><span data-stu-id="d0549-655">Select **iPhone Simulator** and click **Install**.</span></span>

    <span data-ttu-id="d0549-656">![Przeglądanie rozszerzenia programu WebMatrix](whats-new-in-aspnet-mvc-4/_static/image54.png "rozszerzenia przeglądania programu WebMatrix")</span><span class="sxs-lookup"><span data-stu-id="d0549-656">![Browsing WebMatrix extensions](whats-new-in-aspnet-mvc-4/_static/image54.png "Browsing WebMatrix extensions")</span></span>

    <span data-ttu-id="d0549-657">*Przeglądanie rozszerzenia programu WebMatrix*</span><span class="sxs-lookup"><span data-stu-id="d0549-657">*Browsing WebMatrix extensions*</span></span>
4. <span data-ttu-id="d0549-658">Informacje dotyczące pakietu, kliknij przycisk **zainstalować** do kontynuowania instalacji rozszerzenia.</span><span class="sxs-lookup"><span data-stu-id="d0549-658">In the package details, click **Install** to continue with the extension installation.</span></span>

    <span data-ttu-id="d0549-659">![rozszerzenie symulatora telefonu iPhone](whats-new-in-aspnet-mvc-4/_static/image55.png "rozszerzenia symulatora telefonu iPhone")</span><span class="sxs-lookup"><span data-stu-id="d0549-659">![iPhone Simulator extension](whats-new-in-aspnet-mvc-4/_static/image55.png "iPhone Simulator extension")</span></span>

    <span data-ttu-id="d0549-660">*rozszerzenie symulatora telefonu iPhone*</span><span class="sxs-lookup"><span data-stu-id="d0549-660">*iPhone Simulator extension*</span></span>
5. <span data-ttu-id="d0549-661">Przeczytaj i zaakceptuj rozszerzenia umowy licencyjnej.</span><span class="sxs-lookup"><span data-stu-id="d0549-661">Read and accept the extension EULA.</span></span>

    <span data-ttu-id="d0549-662">![Rozszerzenia programu WebMatrix umowy licencyjnej](whats-new-in-aspnet-mvc-4/_static/image56.png "rozszerzenia programu WebMatrix umowy licencyjnej")</span><span class="sxs-lookup"><span data-stu-id="d0549-662">![WebMatrix extension EULA](whats-new-in-aspnet-mvc-4/_static/image56.png "WebMatrix extension EULA")</span></span>

    <span data-ttu-id="d0549-663">*Rozszerzenia programu WebMatrix umowy licencyjnej*</span><span class="sxs-lookup"><span data-stu-id="d0549-663">*WebMatrix extension EULA*</span></span>
6. <span data-ttu-id="d0549-664">Teraz można uruchomić witryny sieci Web z programu WebMatrix za pomocą iPhone opcja symulatora.</span><span class="sxs-lookup"><span data-stu-id="d0549-664">Now, you can run your Web site from WebMatrix using the iPhone Simulator option.</span></span>

    <span data-ttu-id="d0549-665">![Uruchom przy użyciu iPhone](whats-new-in-aspnet-mvc-4/_static/image57.png "uruchomić przy użyciu iPhone")</span><span class="sxs-lookup"><span data-stu-id="d0549-665">![Run using iPhone](whats-new-in-aspnet-mvc-4/_static/image57.png "Run using iPhone")</span></span>

    <span data-ttu-id="d0549-666">*Uruchom przy użyciu iPhone*</span><span class="sxs-lookup"><span data-stu-id="d0549-666">*Run using iPhone*</span></span>

<a id="ApxCTask3"></a>

<a id="Task_3_-_Configuring_Visual_Studio_2012_to_run_iPhone_Simulator"></a>
#### <a name="task-3---configuring-visual-studio-2012-to-run-iphone-simulator"></a><span data-ttu-id="d0549-667">Zadanie 3 — Konfigurowanie programu Visual Studio 2012 do uruchamiania symulatora telefonu iPhone</span><span class="sxs-lookup"><span data-stu-id="d0549-667">Task 3 - Configuring Visual Studio 2012 to run iPhone Simulator</span></span>

1. <span data-ttu-id="d0549-668">Otwórz **programu Visual Studio 2012** i otwarcie każdej witryny sieci Web lub Utwórz nowy projekt.</span><span class="sxs-lookup"><span data-stu-id="d0549-668">Open **Visual Studio 2012** and open any Web site or create a new project.</span></span>
2. <span data-ttu-id="d0549-669">Kliknij strzałkę w dół od przycisk Uruchom, a następnie wybierz **przeglądania przy użyciu**.</span><span class="sxs-lookup"><span data-stu-id="d0549-669">Click the down arrow from the Run button and select **Browse with**.</span></span>

    <span data-ttu-id="d0549-670">![Przeglądaj z](whats-new-in-aspnet-mvc-4/_static/image58.png "przeglądania przy użyciu")</span><span class="sxs-lookup"><span data-stu-id="d0549-670">![Browse with](whats-new-in-aspnet-mvc-4/_static/image58.png "Browse with")</span></span>

    <span data-ttu-id="d0549-671">*Przeglądaj z*</span><span class="sxs-lookup"><span data-stu-id="d0549-671">*Browse with*</span></span>
3. <span data-ttu-id="d0549-672">W &quot;przeglądanie za pomocą&quot; okna dialogowego, kliknij przycisk **Dodaj**.</span><span class="sxs-lookup"><span data-stu-id="d0549-672">In the &quot;Browse With&quot; dialog, click **Add**.</span></span>
4. <span data-ttu-id="d0549-673">W &quot;Dodaj Program&quot; okna dialogowego, użyj następujących wartości:</span><span class="sxs-lookup"><span data-stu-id="d0549-673">In the &quot;Add Program&quot; dialog, use the following values:</span></span>

    - <span data-ttu-id="d0549-674">**Program**: C:\Users\*{CurrentUser}*\AppData\Local\Microsoft\WebMatrix\Extensions\20\iPhoneSimulator\ElectricMobileSim\ElectricMobileSim.exe *(zaktualizować odpowiednio ścieżkę)*</span><span class="sxs-lookup"><span data-stu-id="d0549-674">**Program**: C:\Users\*{CurrentUser}*\AppData\Local\Microsoft\WebMatrix\Extensions\20\iPhoneSimulator\ElectricMobileSim\ElectricMobileSim.exe *(update the path accordingly)*</span></span>
    - <span data-ttu-id="d0549-675">**Argumenty**: &quot;1&quot;</span><span class="sxs-lookup"><span data-stu-id="d0549-675">**Arguments**: &quot;1&quot;</span></span>
    - <span data-ttu-id="d0549-676">**Przyjazna nazwa**: symulatora telefonu iPhone</span><span class="sxs-lookup"><span data-stu-id="d0549-676">**Friendly name**: iPhone Simulator</span></span>

    <span data-ttu-id="d0549-677">![Dodaj program](whats-new-in-aspnet-mvc-4/_static/image59.png "Dodaj program")</span><span class="sxs-lookup"><span data-stu-id="d0549-677">![Add program](whats-new-in-aspnet-mvc-4/_static/image59.png "Add program")</span></span>

    <span data-ttu-id="d0549-678">*Dodaj program do przeglądania przy użyciu*</span><span class="sxs-lookup"><span data-stu-id="d0549-678">*Add program to browse with*</span></span>
5. <span data-ttu-id="d0549-679">Kliknij przycisk **OK** i zamknąć okna dialogowe.</span><span class="sxs-lookup"><span data-stu-id="d0549-679">Click **OK** and close the dialogs.</span></span>
6. <span data-ttu-id="d0549-680">Jesteś teraz można uruchamiać aplikacje sieci Web w symulatorze iPhone z programu Visual Studio 2012.</span><span class="sxs-lookup"><span data-stu-id="d0549-680">Now you are able to run your Web applications in the iPhone simulator from Visual Studio 2012.</span></span>

    <span data-ttu-id="d0549-681">![Przeglądaj z iPhone symulatora](whats-new-in-aspnet-mvc-4/_static/image60.png "przeglądania przy użyciu symulatora telefonu iPhone")</span><span class="sxs-lookup"><span data-stu-id="d0549-681">![Browse with iPhone Simulator](whats-new-in-aspnet-mvc-4/_static/image60.png "Browse with iPhone Simulator")</span></span>

    <span data-ttu-id="d0549-682">*Przeglądania przy użyciu symulatora telefonu iPhone*</span><span class="sxs-lookup"><span data-stu-id="d0549-682">*Browse with iPhone Simulator*</span></span>

<a id="AppendixD"></a>

<a id="Appendix_D_Publishing_an_ASPNET_MVC_4_Application_using_Web_Deploy"></a>
## <a name="appendix-d-publishing-an-aspnet-mvc-4-application-using-web-deploy"></a><span data-ttu-id="d0549-683">Dodatek D: publikowania aplikacji ASP.NET MVC 4 przy użyciu narzędzia Web Deploy</span><span class="sxs-lookup"><span data-stu-id="d0549-683">Appendix D: Publishing an ASP.NET MVC 4 Application using Web Deploy</span></span>

<span data-ttu-id="d0549-684">Ten dodatek opisano sposób tworzenia nowej witryny sieci web z portalu zarządzania pakietu Windows Azure i publikowanie aplikacji, uzyskane wykonując laboratorium, korzystając z funkcji publikowania narzędzia Web Deploy dostarczane przez Windows Azure.</span><span class="sxs-lookup"><span data-stu-id="d0549-684">This appendix will show you how to create a new web site from the Windows Azure Management Portal and publish the application you obtained by following the lab, taking advantage of the Web Deploy publishing feature provided by Windows Azure.</span></span>

<a id="ApxDTask1"></a>

<a id="Task_1_-_Creating_a_New_Web_Site_from_the_Windows_Azure_Portal"></a>
#### <a name="task-1---creating-a-new-web-site-from-the-windows-azure-portal"></a><span data-ttu-id="d0549-685">Zadanie 1 — Tworzenie nowej witryny sieci Web systemu Windows portalu Azure</span><span class="sxs-lookup"><span data-stu-id="d0549-685">Task 1 - Creating a New Web Site from the Windows Azure Portal</span></span>

1. <span data-ttu-id="d0549-686">Przejdź do [portalu zarządzania pakietu Windows Azure](https://manage.windowsazure.com/) i zaloguj się przy użyciu poświadczeń Microsoft skojarzonych z Twoją subskrypcją.</span><span class="sxs-lookup"><span data-stu-id="d0549-686">Go to the [Windows Azure Management Portal](https://manage.windowsazure.com/) and sign in using the Microsoft credentials associated with your subscription.</span></span>

    > [!NOTE]
    > <span data-ttu-id="d0549-687">Z systemu Windows Azure można udostępniać 10 witryn sieci Web platformy ASP.NET bezpłatnie i następnie Skaluj w miarę zwiększania się ruchu.</span><span class="sxs-lookup"><span data-stu-id="d0549-687">With Windows Azure you can host 10 ASP.NET Web Sites for free and then scale as your traffic grows.</span></span> <span data-ttu-id="d0549-688">Możesz utworzyć konto [tutaj](http://aka.ms/aspnet-hol-azure).</span><span class="sxs-lookup"><span data-stu-id="d0549-688">You can sign up [here](http://aka.ms/aspnet-hol-azure).</span></span>

    <span data-ttu-id="d0549-689">![Zaloguj się do portalu Windows Azure](whats-new-in-aspnet-mvc-4/_static/image61.png "Zaloguj się do portalu Windows Azure")</span><span class="sxs-lookup"><span data-stu-id="d0549-689">![Log on to Windows Azure portal](whats-new-in-aspnet-mvc-4/_static/image61.png "Log on to Windows Azure portal")</span></span>

    <span data-ttu-id="d0549-690">*Zaloguj się do portalu zarządzania platformy Azure z systemem Windows*</span><span class="sxs-lookup"><span data-stu-id="d0549-690">*Log on to Windows Azure Management Portal*</span></span>
2. <span data-ttu-id="d0549-691">Kliknij przycisk **nowy** na pasku poleceń.</span><span class="sxs-lookup"><span data-stu-id="d0549-691">Click **New** on the command bar.</span></span>

    <span data-ttu-id="d0549-692">![Tworzenie nowej witryny sieci Web](whats-new-in-aspnet-mvc-4/_static/image62.png "tworzenia nowej witryny sieci Web")</span><span class="sxs-lookup"><span data-stu-id="d0549-692">![Creating a new Web Site](whats-new-in-aspnet-mvc-4/_static/image62.png "Creating a new Web Site")</span></span>

    <span data-ttu-id="d0549-693">*Tworzenie nowej witryny sieci Web*</span><span class="sxs-lookup"><span data-stu-id="d0549-693">*Creating a new Web Site*</span></span>
3. <span data-ttu-id="d0549-694">Kliknij przycisk **obliczeniowe** | **witryny sieci Web**.</span><span class="sxs-lookup"><span data-stu-id="d0549-694">Click **Compute** | **Web Site**.</span></span> <span data-ttu-id="d0549-695">Następnie wybierz **szybkie tworzenie** opcji.</span><span class="sxs-lookup"><span data-stu-id="d0549-695">Then select **Quick Create** option.</span></span> <span data-ttu-id="d0549-696">Podaj dostępny adres URL dla nowej witryny sieci web, a następnie kliknij przycisk **tworzenie witryny sieci Web**.</span><span class="sxs-lookup"><span data-stu-id="d0549-696">Provide an available URL for the new web site and click **Create Web Site**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="d0549-697">Witryny sieci Web systemu Windows Azure jest hostem dla aplikacji sieci web w chmurze, które można kontrolować i zarządzanie nimi.</span><span class="sxs-lookup"><span data-stu-id="d0549-697">A Windows Azure Web Site is the host for a web application running in the cloud that you can control and manage.</span></span> <span data-ttu-id="d0549-698">Opcja szybkie tworzenie umożliwia wdrażanie ukończonej aplikacji sieci web do systemu Windows Azure witryny internetowej z spoza portalu.</span><span class="sxs-lookup"><span data-stu-id="d0549-698">The Quick Create option allows you to deploy a completed web application to the Windows Azure Web Site from outside the portal.</span></span> <span data-ttu-id="d0549-699">Nie obejmuje kroki konfigurowania bazy danych.</span><span class="sxs-lookup"><span data-stu-id="d0549-699">It does not include steps for setting up a database.</span></span>

    <span data-ttu-id="d0549-700">![Tworzenie nowej witryny sieci Web przy użyciu szybkie tworzenie](whats-new-in-aspnet-mvc-4/_static/image63.png "tworzenia nowej witryny sieci Web przy użyciu szybkie tworzenie")</span><span class="sxs-lookup"><span data-stu-id="d0549-700">![Creating a new Web Site using Quick Create](whats-new-in-aspnet-mvc-4/_static/image63.png "Creating a new Web Site using Quick Create")</span></span>

    <span data-ttu-id="d0549-701">*Tworzenie nowej witryny sieci Web przy użyciu szybkie tworzenie*</span><span class="sxs-lookup"><span data-stu-id="d0549-701">*Creating a new Web Site using Quick Create*</span></span>
4. <span data-ttu-id="d0549-702">Poczekaj na nowe **witryny sieci Web** jest tworzony.</span><span class="sxs-lookup"><span data-stu-id="d0549-702">Wait until the new **Web Site** is created.</span></span>
5. <span data-ttu-id="d0549-703">Po utworzeniu witryny sieci Web kliknij łącze w obszarze **adres URL** kolumny.</span><span class="sxs-lookup"><span data-stu-id="d0549-703">Once the Web Site is created click the link under the **URL** column.</span></span> <span data-ttu-id="d0549-704">Sprawdź, czy działa nowej witryny sieci Web.</span><span class="sxs-lookup"><span data-stu-id="d0549-704">Check that the new Web Site is working.</span></span>

    <span data-ttu-id="d0549-705">![Przeglądanie do nowej witryny sieci web](whats-new-in-aspnet-mvc-4/_static/image64.png "przeglądanie do nowej witryny sieci web")</span><span class="sxs-lookup"><span data-stu-id="d0549-705">![Browsing to the new web site](whats-new-in-aspnet-mvc-4/_static/image64.png "Browsing to the new web site")</span></span>

    <span data-ttu-id="d0549-706">*Przeglądanie do nowej witryny sieci web*</span><span class="sxs-lookup"><span data-stu-id="d0549-706">*Browsing to the new web site*</span></span>

    <span data-ttu-id="d0549-707">![Witryna sieci Web działa](whats-new-in-aspnet-mvc-4/_static/image65.png "uruchamiania witryny sieci Web")</span><span class="sxs-lookup"><span data-stu-id="d0549-707">![Web site running](whats-new-in-aspnet-mvc-4/_static/image65.png "Web site running")</span></span>

    <span data-ttu-id="d0549-708">*Witryna sieci Web uruchomiona*</span><span class="sxs-lookup"><span data-stu-id="d0549-708">*Web site running*</span></span>
6. <span data-ttu-id="d0549-709">Wróć do portalu i kliknij nazwę witryny sieci web w obszarze **nazwa** kolumny do wyświetlenia strony zarządzania.</span><span class="sxs-lookup"><span data-stu-id="d0549-709">Go back to the portal and click the name of the web site under the **Name** column to display the management pages.</span></span>

    <span data-ttu-id="d0549-710">![Otwieranie stron witryny sieci web zarządzania](whats-new-in-aspnet-mvc-4/_static/image66.png "otwieranie stron zarządzania witryny sieci web")</span><span class="sxs-lookup"><span data-stu-id="d0549-710">![Opening the web site management pages](whats-new-in-aspnet-mvc-4/_static/image66.png "Opening the web site management pages")</span></span>

    <span data-ttu-id="d0549-711">*Otwieranie stron zarządzania witryny sieci Web*</span><span class="sxs-lookup"><span data-stu-id="d0549-711">*Opening the Web Site management pages*</span></span>
7. <span data-ttu-id="d0549-712">W **pulpitu nawigacyjnego** w obszarze **szybkiego dostępu** kliknij **pobieranie profilu publikowania** łącza.</span><span class="sxs-lookup"><span data-stu-id="d0549-712">In the **Dashboard** page, under the **quick glance** section, click the **Download publish profile** link.</span></span>

    > [!NOTE]
    > <span data-ttu-id="d0549-713">*Profilu publikowania* zawiera wszystkie informacje wymagane do publikowania aplikacji sieci web do witryny sieci Web systemu Windows Azure dla każdej metody włączone publikacji.</span><span class="sxs-lookup"><span data-stu-id="d0549-713">The *publish profile* contains all of the information required to publish a web application to a Windows Azure website for each enabled publication method.</span></span> <span data-ttu-id="d0549-714">Profil publikowania zawiera adresy URL, poświadczenia użytkownika i parametry bazy danych wymagane do nawiązania połączenia i uwierzytelniania dla każdego z punktów końcowych, dla których włączono metoda publikacji.</span><span class="sxs-lookup"><span data-stu-id="d0549-714">The publish profile contains the URLs, user credentials and database strings required to connect to and authenticate against each of the endpoints for which a publication method is enabled.</span></span> <span data-ttu-id="d0549-715">**Microsoft WebMatrix 2**, **Microsoft Visual Studio Express for Web** i **programu Microsoft Visual Studio 2012** obsługują odczytywanie publikowanie profile do zautomatyzowania te programy Publikowanie aplikacji sieci web do witryn sieci Web systemu Windows Azure.</span><span class="sxs-lookup"><span data-stu-id="d0549-715">**Microsoft WebMatrix 2**, **Microsoft Visual Studio Express for Web** and **Microsoft Visual Studio 2012** support reading publish profiles to automate configuration of these programs for publishing web applications to Windows Azure websites.</span></span>

    <span data-ttu-id="d0549-716">![Pobieranie witryny sieci web profilu publikowania](whats-new-in-aspnet-mvc-4/_static/image67.png "pobierania witryny sieci web profilu publikowania")</span><span class="sxs-lookup"><span data-stu-id="d0549-716">![Downloading the web site publish profile](whats-new-in-aspnet-mvc-4/_static/image67.png "Downloading the web site publish profile")</span></span>

    <span data-ttu-id="d0549-717">*Pobieranie witryny sieci Web profilu publikowania*</span><span class="sxs-lookup"><span data-stu-id="d0549-717">*Downloading the Web Site publish profile*</span></span>
8. <span data-ttu-id="d0549-718">Pobierz profil publikowania w znanej lokalizacji.</span><span class="sxs-lookup"><span data-stu-id="d0549-718">Download the publish profile file to a known location.</span></span> <span data-ttu-id="d0549-719">Dodatkowo w tym ćwiczeniu zobaczysz jak opublikować aplikację sieci web do witryny sieci Web systemu Windows Azure w programie Visual Studio przy użyciu tego pliku.</span><span class="sxs-lookup"><span data-stu-id="d0549-719">Further in this exercise you will see how to use this file to publish a web application to a Windows Azure Web Sites from Visual Studio.</span></span>

    <span data-ttu-id="d0549-720">![Zapisywanie pliku profilu publikowania](whats-new-in-aspnet-mvc-4/_static/image68.png "zapisywanie profilu publikowania")</span><span class="sxs-lookup"><span data-stu-id="d0549-720">![Saving the publish profile file](whats-new-in-aspnet-mvc-4/_static/image68.png "Saving the publish profile")</span></span>

    <span data-ttu-id="d0549-721">*Zapisywanie pliku profilu publikowania*</span><span class="sxs-lookup"><span data-stu-id="d0549-721">*Saving the publish profile file*</span></span>

<a id="ApxDTask2"></a>

<a id="Task_2_-_Configuring_the_Database_Server"></a>
#### <a name="task-2---configuring-the-database-server"></a><span data-ttu-id="d0549-722">Zadanie 2 — Konfigurowanie serwera bazy danych</span><span class="sxs-lookup"><span data-stu-id="d0549-722">Task 2 - Configuring the Database Server</span></span>

<span data-ttu-id="d0549-723">Jeśli aplikacja korzysta z programu SQL Server baz danych, należy utworzyć serwer bazy danych SQL.</span><span class="sxs-lookup"><span data-stu-id="d0549-723">If your application makes use of SQL Server databases you will need to create a SQL Database server.</span></span> <span data-ttu-id="d0549-724">Jeśli chcesz wdrożyć prostą aplikację, która nie korzysta z programu SQL Server może pominąć to zadanie.</span><span class="sxs-lookup"><span data-stu-id="d0549-724">If you want to deploy a simple application that does not use SQL Server you might skip this task.</span></span>

1. <span data-ttu-id="d0549-725">Będzie potrzebny serwer bazy danych SQL do przechowywania bazy danych aplikacji.</span><span class="sxs-lookup"><span data-stu-id="d0549-725">You will need a SQL Database server for storing the application database.</span></span> <span data-ttu-id="d0549-726">Można wyświetlić serwery bazy danych SQL z subskrypcji usługi Windows Azure Management Portal pod adresem **baz danych Sql** | **serwerów** | **serwera Pulpit nawigacyjny**.</span><span class="sxs-lookup"><span data-stu-id="d0549-726">You can view the SQL Database servers from your subscription in the Windows Azure Management portal at **Sql Databases** | **Servers** | **Server's Dashboard**.</span></span> <span data-ttu-id="d0549-727">Jeśli nie masz serwer, który został utworzony, można utworzyć przy użyciu jednego **Dodaj** przycisk paska poleceń.</span><span class="sxs-lookup"><span data-stu-id="d0549-727">If you do not have a server created, you can create one using the **Add** button on the command bar.</span></span> <span data-ttu-id="d0549-728">Zwróć uwagę na **nazwę serwera i adres URL, nazwę logowania administratora i hasła**, jak będą używane w następnego zadania.</span><span class="sxs-lookup"><span data-stu-id="d0549-728">Take note of the **server name and URL, administrator login name and password**, as you will use them in the next tasks.</span></span> <span data-ttu-id="d0549-729">Nie należy tworzyć bazy danych jeszcze, jako zostaną utworzone w późniejszym terminie.</span><span class="sxs-lookup"><span data-stu-id="d0549-729">Do not create the database yet, as it will be created in a later stage.</span></span>

    <span data-ttu-id="d0549-730">![Pulpit nawigacyjny serwera bazy danych SQL](whats-new-in-aspnet-mvc-4/_static/image69.png "pulpitu nawigacyjnego serwera bazy danych SQL")</span><span class="sxs-lookup"><span data-stu-id="d0549-730">![SQL Database Server Dashboard](whats-new-in-aspnet-mvc-4/_static/image69.png "SQL Database Server Dashboard")</span></span>

    <span data-ttu-id="d0549-731">*Pulpit nawigacyjny serwera bazy danych SQL*</span><span class="sxs-lookup"><span data-stu-id="d0549-731">*SQL Database Server Dashboard*</span></span>
2. <span data-ttu-id="d0549-732">W następnym zadaniem Testuj połączenie z bazą danych z programu Visual Studio z tego powodu należy uwzględnić lokalny adres IP serwera liście **dozwolone adresy IP**.</span><span class="sxs-lookup"><span data-stu-id="d0549-732">In the next task you will test the database connection from Visual Studio, for that reason you need to include your local IP address in the server's list of **Allowed IP Addresses**.</span></span> <span data-ttu-id="d0549-733">Aby to zrobić, kliknij przycisk **Konfiguruj**, wybierz adres IP z **bieżącego adresu IP klienta** i wklej go na **początkowy adres IP** i **końcowy adres IP** pola tekstowe i kliknij przycisk ![add-client-ip-address-ok-button](whats-new-in-aspnet-mvc-4/_static/image70.png) przycisku.</span><span class="sxs-lookup"><span data-stu-id="d0549-733">To do that, click **Configure**, select the IP address from **Current Client IP Address** and paste it on the **Start IP Address** and **End IP Address** text boxes and click the ![add-client-ip-address-ok-button](whats-new-in-aspnet-mvc-4/_static/image70.png) button.</span></span>

    ![Dodawanie adresu IP klienta](whats-new-in-aspnet-mvc-4/_static/image71.png)

    <span data-ttu-id="d0549-735">*Dodawanie adresu IP klienta*</span><span class="sxs-lookup"><span data-stu-id="d0549-735">*Adding Client IP Address*</span></span>
3. <span data-ttu-id="d0549-736">Raz **adres IP klienta** jest dodawany do dozwolonych adresów IP kliknij na **zapisać** o potwierdzenie zmian.</span><span class="sxs-lookup"><span data-stu-id="d0549-736">Once the **Client IP Address** is added to the allowed IP addresses list, click on **Save** to confirm the changes.</span></span>

    ![Potwierdzenie zmian](whats-new-in-aspnet-mvc-4/_static/image72.png)

    <span data-ttu-id="d0549-738">*Potwierdzenie zmian*</span><span class="sxs-lookup"><span data-stu-id="d0549-738">*Confirm Changes*</span></span>

<a id="ApxDTask3"></a>

<a id="Task_3_-_Publishing_an_ASPNET_MVC_4_Application_using_Web_Deploy"></a>
#### <a name="task-3---publishing-an-aspnet-mvc-4-application-using-web-deploy"></a><span data-ttu-id="d0549-739">Zadanie 3 - publikowania aplikacji ASP.NET MVC 4 przy użyciu narzędzia Web Deploy</span><span class="sxs-lookup"><span data-stu-id="d0549-739">Task 3 - Publishing an ASP.NET MVC 4 Application using Web Deploy</span></span>

1. <span data-ttu-id="d0549-740">Wróć do rozwiązania ASP.NET MVC 4.</span><span class="sxs-lookup"><span data-stu-id="d0549-740">Go back to the ASP.NET MVC 4 solution.</span></span> <span data-ttu-id="d0549-741">W **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy projekt witryny sieci web i wybierz **publikowania**.</span><span class="sxs-lookup"><span data-stu-id="d0549-741">In the **Solution Explorer**, right-click the web site project and select **Publish**.</span></span>

    <span data-ttu-id="d0549-742">![Publikowanie aplikacji](whats-new-in-aspnet-mvc-4/_static/image73.png "publikowania aplikacji")</span><span class="sxs-lookup"><span data-stu-id="d0549-742">![Publishing the Application](whats-new-in-aspnet-mvc-4/_static/image73.png "Publishing the Application")</span></span>

    <span data-ttu-id="d0549-743">*Publikowanie witryny sieci web*</span><span class="sxs-lookup"><span data-stu-id="d0549-743">*Publishing the web site*</span></span>
2. <span data-ttu-id="d0549-744">Zaimportuj profil publikowania, zapisana w pierwszym zadaniu.</span><span class="sxs-lookup"><span data-stu-id="d0549-744">Import the publish profile you saved in the first task.</span></span>

    <span data-ttu-id="d0549-745">![Importowanie profilu publikowania](whats-new-in-aspnet-mvc-4/_static/image74.png "Importowanie profilu publikowania")</span><span class="sxs-lookup"><span data-stu-id="d0549-745">![Importing the publish profile](whats-new-in-aspnet-mvc-4/_static/image74.png "Importing the publish profile")</span></span>

    <span data-ttu-id="d0549-746">*Importowanie profilu publikowania*</span><span class="sxs-lookup"><span data-stu-id="d0549-746">*Importing publish profile*</span></span>
3. <span data-ttu-id="d0549-747">Kliknij przycisk **Weryfikacja połączenia z**.</span><span class="sxs-lookup"><span data-stu-id="d0549-747">Click **Validate Connection**.</span></span> <span data-ttu-id="d0549-748">Po zakończeniu sprawdzania kliknij **dalej**.</span><span class="sxs-lookup"><span data-stu-id="d0549-748">Once Validation is complete click **Next**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="d0549-749">Zakończeniu sprawdzania poprawności, gdy zostanie wyświetlony zielony znacznik wyboru są wyświetlane obok przycisku sprawdzania poprawności połączenia.</span><span class="sxs-lookup"><span data-stu-id="d0549-749">Validation is complete once you see a green checkmark appear next to the Validate Connection button.</span></span>

    <span data-ttu-id="d0549-750">![Sprawdzanie poprawności połączenia](whats-new-in-aspnet-mvc-4/_static/image75.png "sprawdzanie poprawności połączenia")</span><span class="sxs-lookup"><span data-stu-id="d0549-750">![Validating connection](whats-new-in-aspnet-mvc-4/_static/image75.png "Validating connection")</span></span>

    <span data-ttu-id="d0549-751">*Sprawdzanie poprawności połączenia*</span><span class="sxs-lookup"><span data-stu-id="d0549-751">*Validating connection*</span></span>
4. <span data-ttu-id="d0549-752">W **ustawienia** w obszarze **baz danych** sekcji, kliknij przycisk Dalej, aby textbox połączenia bazy danych (tj. **połączenia DefaultConnection**).</span><span class="sxs-lookup"><span data-stu-id="d0549-752">In the **Settings** page, under the **Databases** section, click the button next to your database connection's textbox (i.e. **DefaultConnection**).</span></span>

    <span data-ttu-id="d0549-753">![Konfiguracja narzędzia Web deploy](whats-new-in-aspnet-mvc-4/_static/image76.png "Konfiguracja narzędzia Web deploy")</span><span class="sxs-lookup"><span data-stu-id="d0549-753">![Web deploy configuration](whats-new-in-aspnet-mvc-4/_static/image76.png "Web deploy configuration")</span></span>

    <span data-ttu-id="d0549-754">*Konfiguracja narzędzia Web deploy*</span><span class="sxs-lookup"><span data-stu-id="d0549-754">*Web deploy configuration*</span></span>
5. <span data-ttu-id="d0549-755">Skonfiguruj połączenie z bazą danych w następujący sposób:</span><span class="sxs-lookup"><span data-stu-id="d0549-755">Configure the database connection as follows:</span></span>

    - <span data-ttu-id="d0549-756">W **nazwy serwera** wpisz swoją bazą danych SQL server adresu URL przy użyciu *tcp:* prefiks.</span><span class="sxs-lookup"><span data-stu-id="d0549-756">In the **Server name** type your SQL Database server URL using the *tcp:* prefix.</span></span>
    - <span data-ttu-id="d0549-757">W **nazwy użytkownika** wpisz nazwę logowania administratora serwera.</span><span class="sxs-lookup"><span data-stu-id="d0549-757">In **User name** type your server administrator login name.</span></span>
    - <span data-ttu-id="d0549-758">W **hasło** wpisz hasło logowania administratora serwera.</span><span class="sxs-lookup"><span data-stu-id="d0549-758">In **Password** type your server administrator login password.</span></span>
    - <span data-ttu-id="d0549-759">Wpisz nazwę nowej bazy danych, na przykład: *MVC4SampleDB*.</span><span class="sxs-lookup"><span data-stu-id="d0549-759">Type a new database name, for example: *MVC4SampleDB*.</span></span>

    <span data-ttu-id="d0549-760">![Konfigurowanie parametrów połączenia z lokalizacją docelową](whats-new-in-aspnet-mvc-4/_static/image77.png "Konfigurowanie parametrów połączenia z lokalizacją docelową")</span><span class="sxs-lookup"><span data-stu-id="d0549-760">![Configuring destination connection string](whats-new-in-aspnet-mvc-4/_static/image77.png "Configuring destination connection string")</span></span>

    <span data-ttu-id="d0549-761">*Konfigurowanie parametrów połączenia z lokalizacją docelową*</span><span class="sxs-lookup"><span data-stu-id="d0549-761">*Configuring destination connection string*</span></span>
6. <span data-ttu-id="d0549-762">Następnie kliknij przycisk **OK**.</span><span class="sxs-lookup"><span data-stu-id="d0549-762">Then click **OK**.</span></span> <span data-ttu-id="d0549-763">Po wyświetleniu monitu można utworzyć bazy danych kliknij **tak**.</span><span class="sxs-lookup"><span data-stu-id="d0549-763">When prompted to create the database click **Yes**.</span></span>

    <span data-ttu-id="d0549-764">![Tworzenie bazy danych](whats-new-in-aspnet-mvc-4/_static/image78.png "tworzenie parametry bazy danych")</span><span class="sxs-lookup"><span data-stu-id="d0549-764">![Creating the database](whats-new-in-aspnet-mvc-4/_static/image78.png "Creating the database string")</span></span>

    <span data-ttu-id="d0549-765">*Tworzenie bazy danych*</span><span class="sxs-lookup"><span data-stu-id="d0549-765">*Creating the database*</span></span>
7. <span data-ttu-id="d0549-766">Ciągu połączenia używanego do łączenia z bazą danych SQL w systemie Windows Azure jest wyświetlany w pole tekstowe domyślne połączenie.</span><span class="sxs-lookup"><span data-stu-id="d0549-766">The connection string you will use to connect to SQL Database in Windows Azure is shown within Default Connection textbox.</span></span> <span data-ttu-id="d0549-767">Następnie kliknij przycisk **Dalej**.</span><span class="sxs-lookup"><span data-stu-id="d0549-767">Then click **Next**.</span></span>

    <span data-ttu-id="d0549-768">![Parametry połączenia wskazujące bazę danych SQL](whats-new-in-aspnet-mvc-4/_static/image79.png "ciąg połączenia wskazujące bazę danych SQL")</span><span class="sxs-lookup"><span data-stu-id="d0549-768">![Connection string pointing to SQL Database](whats-new-in-aspnet-mvc-4/_static/image79.png "Connection string pointing to SQL Database")</span></span>

    <span data-ttu-id="d0549-769">*Parametry połączenia wskazujące bazę danych SQL*</span><span class="sxs-lookup"><span data-stu-id="d0549-769">*Connection string pointing to SQL Database*</span></span>
8. <span data-ttu-id="d0549-770">W **Podgląd** kliknij przycisk **publikowania**.</span><span class="sxs-lookup"><span data-stu-id="d0549-770">In the **Preview** page, click **Publish**.</span></span>

    <span data-ttu-id="d0549-771">![Publikowanie aplikacji sieci web](whats-new-in-aspnet-mvc-4/_static/image80.png "publikowania aplikacji sieci web")</span><span class="sxs-lookup"><span data-stu-id="d0549-771">![Publishing the web application](whats-new-in-aspnet-mvc-4/_static/image80.png "Publishing the web application")</span></span>

    <span data-ttu-id="d0549-772">*Publikowanie aplikacji sieci web*</span><span class="sxs-lookup"><span data-stu-id="d0549-772">*Publishing the web application*</span></span>
9. <span data-ttu-id="d0549-773">Po zakończeniu procesu publikowania domyślnej przeglądarce otworzy opublikowanej witryny sieci web.</span><span class="sxs-lookup"><span data-stu-id="d0549-773">Once the publishing process finishes, your default browser will open the published web site.</span></span>

    <span data-ttu-id="d0549-774">![Aplikacja opublikowana w systemie Windows Azure](whats-new-in-aspnet-mvc-4/_static/image81.png "aplikacji publikowanych w systemie Windows Azure")</span><span class="sxs-lookup"><span data-stu-id="d0549-774">![Application published to Windows Azure](whats-new-in-aspnet-mvc-4/_static/image81.png "Application published to Windows Azure")</span></span>

    <span data-ttu-id="d0549-775">*Aplikacji publikowanych w systemie Windows Azure*</span><span class="sxs-lookup"><span data-stu-id="d0549-775">*Application published to Windows Azure*</span></span>
