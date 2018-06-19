---
uid: visual-studio/overview/2013/visual-studio-2013-web-tools
title: 'Wskazówki laboratorium: Narzędzia sieci Web 2013 Visual Studio | Dokumentacja firmy Microsoft'
author: rick-anderson
description: Visual Studio to środowisko rozwoju znakomity. SIEĆ systemu Windows i projekty sieci web. Obejmuje on łatwo służący do edytora tekstu zaawansowane...
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/16/2014
ms.topic: article
ms.assetid: 09e82351-816b-402d-acd1-0f9ac6901d16
ms.technology: ''
ms.prod: .net-framework
msc.legacyurl: /visual-studio/overview/2013/visual-studio-2013-web-tools
msc.type: authoredcontent
ms.openlocfilehash: ef8ab82f9043ef9da3a3e6a146a97f083149534d
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/10/2018
ms.locfileid: "26566444"
---
<a name="hands-on-lab-visual-studio-2013-web-tools"></a><span data-ttu-id="ec55a-104">Wskazówki laboratorium: Narzędzia sieci Web 2013 Visual Studio</span><span class="sxs-lookup"><span data-stu-id="ec55a-104">Hands On Lab: Visual Studio 2013 Web Tools</span></span>
====================
<span data-ttu-id="ec55a-105">Przez [obozów sieci Web Team](https://twitter.com/webcamps)</span><span class="sxs-lookup"><span data-stu-id="ec55a-105">by [Web Camps Team](https://twitter.com/webcamps)</span></span>

[<span data-ttu-id="ec55a-106">Pobierz obozów sieci Web uczenie Kit</span><span class="sxs-lookup"><span data-stu-id="ec55a-106">Download Web Camps Training Kit</span></span>](http://aka.ms/webcamps-training-kit)

> <span data-ttu-id="ec55a-107">Visual Studio to środowisko rozwoju znakomity. SIEĆ systemu Windows i projekty sieci web.</span><span class="sxs-lookup"><span data-stu-id="ec55a-107">Visual Studio is an excellent development environment for .NET-based Windows and web projects.</span></span> <span data-ttu-id="ec55a-108">Obejmuje on edytora tekstów zaawansowanych łatwo służący do edytowania plików autonomicznych bez projektu.</span><span class="sxs-lookup"><span data-stu-id="ec55a-108">It includes a powerful text editor that can easily be used to edit standalone files without a project.</span></span>
> 
> <span data-ttu-id="ec55a-109">Visual Studio zachowuje drzewo analizy oferujący wszystkie funkcje edycji każdego pliku.</span><span class="sxs-lookup"><span data-stu-id="ec55a-109">Visual Studio maintains a full-featured parse tree as you edit each file.</span></span> <span data-ttu-id="ec55a-110">Dzięki temu Visual Studio, aby zapewnić bezkonkurencyjne autouzupełniania i czynności na podstawie dokumentu jednocześnie środowisko programistyczne znacznie szybsze i bardziej przyjemne.</span><span class="sxs-lookup"><span data-stu-id="ec55a-110">This allows Visual Studio to provide unparalleled auto-completion and document-based actions while making the development experience much faster and more pleasant.</span></span> <span data-ttu-id="ec55a-111">Te funkcje są szczególnie zaawansowanych w dokumentach HTML i CSS.</span><span class="sxs-lookup"><span data-stu-id="ec55a-111">These features are especially powerful in HTML and CSS documents.</span></span>
> 
> <span data-ttu-id="ec55a-112">Wszystkie tego uprawnienia jest również dostępny do rozszerzenia, dzięki czemu można łatwo rozszerzyć edytory nowe zaawansowane funkcje, w zależności od potrzeb.</span><span class="sxs-lookup"><span data-stu-id="ec55a-112">All of this power is also available for extensions, making it simple to extend the editors with powerful new features to suit your needs.</span></span> <span data-ttu-id="ec55a-113">Podstawowe informacje dotyczące sieci Web jest kolekcją rozszerzeń programu Visual Studio (przeważnie) związanych z sieci web.</span><span class="sxs-lookup"><span data-stu-id="ec55a-113">Web Essentials is a collection of (mostly) web-related enhancements to Visual Studio.</span></span> <span data-ttu-id="ec55a-114">Zawiera wiele nowych funkcji IntelliSense zakończeń (szczególnie w przypadku CSS), nowe funkcje łącze przeglądarki, automatyczne JSHint JavaScript pliki nowego ostrzeżenia dla HTML, CSS i wiele innych funkcji, które są niezbędne do tworzenia nowoczesnych witryn sieci web.</span><span class="sxs-lookup"><span data-stu-id="ec55a-114">It includes lots of new IntelliSense completions (especially for CSS), new Browser Link features, automatic JSHint for JavaScript files, new warnings for HTML and CSS, and many other features that are essential to modern web development.</span></span>
> 
> <span data-ttu-id="ec55a-115">Wszystkie przykładowy kod i fragmenty kodu są uwzględnione w sieci Web obozów zestaw szkoleniowy, dostępne pod adresem [ http://aka.ms/webcamps-training-kit ](http://aka.ms/webcamps-training-kit).</span><span class="sxs-lookup"><span data-stu-id="ec55a-115">All sample code and snippets are included in the Web Camps Training Kit, available at [http://aka.ms/webcamps-training-kit](http://aka.ms/webcamps-training-kit).</span></span>


<a id="Overview"></a>
## <a name="overview"></a><span data-ttu-id="ec55a-116">Omówienie</span><span class="sxs-lookup"><span data-stu-id="ec55a-116">Overview</span></span>

<a id="Objectives"></a>
### <a name="objectives"></a><span data-ttu-id="ec55a-117">Cele</span><span class="sxs-lookup"><span data-stu-id="ec55a-117">Objectives</span></span>

<span data-ttu-id="ec55a-118">W tym laboratorium praktycznego przedstawiono sposób:</span><span class="sxs-lookup"><span data-stu-id="ec55a-118">In this hands-on lab, you will learn how to:</span></span>

- <span data-ttu-id="ec55a-119">Korzystać z nowych funkcji edytora HTML zawarte w Essentials sieci Web, takich jak rozbudowanych wstawek kodu HTML5 i Zen kodowania</span><span class="sxs-lookup"><span data-stu-id="ec55a-119">Use new HTML editor features included in Web Essentials such as rich HTML5 code snippets and Zen coding</span></span>
- <span data-ttu-id="ec55a-120">Korzystać z nowych funkcji Edytor CSS zawarte w Essentials sieci Web, takich jak próbnika kolorów i etykietka narzędzia macierzy przeglądarki</span><span class="sxs-lookup"><span data-stu-id="ec55a-120">Use new CSS editor features included in Web Essentials such as the Color picker and Browser matrix tooltip</span></span>
- <span data-ttu-id="ec55a-121">Korzystać z nowych funkcji edytora JavaScript zawarte w sieci Web Essentials, takich jak wyodrębniania do pliku i technologii IntelliSense dla wszystkich elementów HTML</span><span class="sxs-lookup"><span data-stu-id="ec55a-121">Use new JavaScript editor features included in Web Essentials such as Extract to File and IntelliSense for all HTML elements</span></span>
- <span data-ttu-id="ec55a-122">Wymiany danych między przeglądarką a Visual Studio przy użyciu Browser Link</span><span class="sxs-lookup"><span data-stu-id="ec55a-122">Exchange data between your browser and Visual Studio using Browser Link</span></span>

<a id="Prerequisites"></a>
### <a name="prerequisites"></a><span data-ttu-id="ec55a-123">Wymagania wstępne</span><span class="sxs-lookup"><span data-stu-id="ec55a-123">Prerequisites</span></span>

<span data-ttu-id="ec55a-124">Poniżej jest wymagany do ukończenia tego laboratorium praktycznego:</span><span class="sxs-lookup"><span data-stu-id="ec55a-124">The following is required to complete this hands-on lab:</span></span>

- <span data-ttu-id="ec55a-125">[Microsoft Visual Studio Professional 2013](https://www.microsoft.com/visualstudio/) or greater</span><span class="sxs-lookup"><span data-stu-id="ec55a-125">[Microsoft Visual Studio Professional 2013](https://www.microsoft.com/visualstudio/) or greater</span></span>
- [<span data-ttu-id="ec55a-126">Web Essentials 2013</span><span class="sxs-lookup"><span data-stu-id="ec55a-126">Web Essentials 2013</span></span>](http://vswebessentials.com/)
- [<span data-ttu-id="ec55a-127">Google Chrome</span><span class="sxs-lookup"><span data-stu-id="ec55a-127">Google Chrome</span></span>](https://www.google.com/chrome/)

<a id="Setup"></a>
### <a name="setup"></a><span data-ttu-id="ec55a-128">Konfiguracja</span><span class="sxs-lookup"><span data-stu-id="ec55a-128">Setup</span></span>

<span data-ttu-id="ec55a-129">Aby można było uruchomić ćwiczeń opisanych w tym laboratorium praktycznego, należy najpierw skonfigurować środowiska.</span><span class="sxs-lookup"><span data-stu-id="ec55a-129">In order to run the exercises in this hands-on lab, you will need to set up your environment first.</span></span>

1. <span data-ttu-id="ec55a-130">Otwórz okno Eksploratora Windows i przejdź do laboratorium **źródła** folderu.</span><span class="sxs-lookup"><span data-stu-id="ec55a-130">Open a Windows Explorer window and browse to the lab's **Source** folder.</span></span>
2. <span data-ttu-id="ec55a-131">Kliknij prawym przyciskiem myszy **Setup.cmd** i wybierz **Uruchom jako administrator** do uruchamiania procesu instalacji, który skonfigurujesz środowisko i zainstalować wstawki kodu programu Visual Studio dla tego laboratorium.</span><span class="sxs-lookup"><span data-stu-id="ec55a-131">Right-click **Setup.cmd** and select **Run as administrator** to launch the setup process that will configure your environment and install the Visual Studio code snippets for this lab.</span></span>
3. <span data-ttu-id="ec55a-132">Jeśli wyświetlane jest okno dialogowe kontroli konta użytkownika, potwierdź tę akcję, aby kontynuować.</span><span class="sxs-lookup"><span data-stu-id="ec55a-132">If the User Account Control dialog box is shown, confirm the action to proceed.</span></span>

> [!NOTE]
> <span data-ttu-id="ec55a-133">Upewnij się, że zaznaczono wszystkie zależności dla tego laboratorium przed uruchomieniem Instalatora.</span><span class="sxs-lookup"><span data-stu-id="ec55a-133">Make sure you have checked all the dependencies for this lab before running the setup.</span></span>


<a id="CodeSnippets"></a>
### <a name="using-the-code-snippets"></a><span data-ttu-id="ec55a-134">Korzystania z wstawek kodu</span><span class="sxs-lookup"><span data-stu-id="ec55a-134">Using the Code Snippets</span></span>

<span data-ttu-id="ec55a-135">W dokumencie laboratorium należy poinstruować do wstawienia bloków kodu.</span><span class="sxs-lookup"><span data-stu-id="ec55a-135">Throughout the lab document, you will be instructed to insert code blocks.</span></span> <span data-ttu-id="ec55a-136">Dla wygody większość tego kodu jest dostępna w Visual Studio wstawki kodu, które są dostępne w Visual Studio 2013, aby uniknąć konieczności Dodaj ją ręcznie.</span><span class="sxs-lookup"><span data-stu-id="ec55a-136">For your convenience, most of this code is provided as Visual Studio Code Snippets, which you can access from within Visual Studio 2013 to avoid having to add it manually.</span></span>

> [!NOTE]
> <span data-ttu-id="ec55a-137">Każdy wykonywania towarzyszy początkowy rozwiązania, znajdujących się w **rozpocząć** folderu ćwiczeniu, która umożliwia wykonanie każdej wykonywania niezależnie od innych.</span><span class="sxs-lookup"><span data-stu-id="ec55a-137">Each exercise is accompanied by a starting solution located in the **Begin** folder of the exercise that allows you to follow each exercise independently of the others.</span></span> <span data-ttu-id="ec55a-138">Należy pamiętać, że fragmenty kodu, które są dodawane podczas wykonywania brakuje te uruchamianie rozwiązań i może nie działać do czasu wykonywania.</span><span class="sxs-lookup"><span data-stu-id="ec55a-138">Please be aware that the code snippets that are added during an exercise are missing from these starting solutions and may not work until you have completed the exercise.</span></span> <span data-ttu-id="ec55a-139">W kodzie źródłowym wykonywania można również znaleźć **zakończenia** folderu zawierającego rozwiązanie Visual Studio z kodem, który powoduje wykonanie czynności opisane w odpowiedniej wykonywania.</span><span class="sxs-lookup"><span data-stu-id="ec55a-139">Inside the source code for an exercise, you will also find an **End** folder containing a Visual Studio solution with the code that results from completing the steps in the corresponding exercise.</span></span> <span data-ttu-id="ec55a-140">Jeśli potrzebujesz dodatkowej pomocy w pracy za pośrednictwem tego laboratorium praktycznego, można użyć tych rozwiązań jako wskazówki.</span><span class="sxs-lookup"><span data-stu-id="ec55a-140">You can use these solutions as guidance if you need additional help as you work through this hands-on lab.</span></span>


* * *

<a id="Exercises"></a>
## <a name="exercises"></a><span data-ttu-id="ec55a-141">Ćwiczenia</span><span class="sxs-lookup"><span data-stu-id="ec55a-141">Exercises</span></span>

<span data-ttu-id="ec55a-142">To laboratorium praktycznego obejmuje następujących czynnościach:</span><span class="sxs-lookup"><span data-stu-id="ec55a-142">This hands-on lab includes the following exercises:</span></span>

1. [<span data-ttu-id="ec55a-143">Praca z łącze przeglądarki i Essentials sieci Web</span><span class="sxs-lookup"><span data-stu-id="ec55a-143">Working with Browser Link and Web Essentials</span></span>](#Exercise1)
2. [<span data-ttu-id="ec55a-144">Dzięki funkcji IntelliSense i wstawki kodu</span><span class="sxs-lookup"><span data-stu-id="ec55a-144">Taking Advantage of Code Snippets and IntelliSense</span></span>](#Exercise2)

> [!NOTE]
> <span data-ttu-id="ec55a-145">Przy pierwszym uruchomieniu programu Visual Studio, musisz wybrać jeden z wstępnie zdefiniowanych ustawień kolekcji.</span><span class="sxs-lookup"><span data-stu-id="ec55a-145">When you first start Visual Studio, you must select one of the predefined settings collections.</span></span> <span data-ttu-id="ec55a-146">Każda kolekcja wstępnie zdefiniowanych zaprojektowano w celu stylem rozwoju i określa układów okien, zachowanie edytora wstawki kodu IntelliSense i opcje w oknach dialogowych.</span><span class="sxs-lookup"><span data-stu-id="ec55a-146">Each predefined collection is designed to match a particular development style and determines window layouts, editor behavior, IntelliSense code snippets, and dialog box options.</span></span> <span data-ttu-id="ec55a-147">Procedury przedstawione w tym laboratorium opisano czynności niezbędnych do wykonywania danego zadania w programie Visual Studio, korzystając z **ogólne ustawienia środowiska deweloperskiego** kolekcji.</span><span class="sxs-lookup"><span data-stu-id="ec55a-147">The procedures in this lab describe the actions necessary to accomplish a given task in Visual Studio when using the **General Development Settings** collection.</span></span> <span data-ttu-id="ec55a-148">Jeśli wybierzesz kolekcji różne ustawienia dla swojego środowiska programowania, może być różnice w krokach, które należy wziąć pod uwagę.</span><span class="sxs-lookup"><span data-stu-id="ec55a-148">If you choose a different settings collection for your development environment, there may be differences in the steps that you should take into account.</span></span>


<a id="Exercise1"></a>
### <a name="exercise-1-working-with-browser-link-and-web-essentials"></a><span data-ttu-id="ec55a-149">Ćwiczenie 1: Praca z łącze przeglądarki i Essentials sieci Web</span><span class="sxs-lookup"><span data-stu-id="ec55a-149">Exercise 1: Working with Browser Link and Web Essentials</span></span>

<span data-ttu-id="ec55a-150">**Sieci Web Essentials** to rozszerzenie programu Visual Studio, które dodaje różne funkcje przydatne do tworzenia nowoczesnych witryn sieci web, przede wszystkim koncentruje się na wprowadzenie środowisko programistyczne web znacznie szybsze i bardziej przyjemne.</span><span class="sxs-lookup"><span data-stu-id="ec55a-150">**Web Essentials** is a Visual Studio extension that adds a variety of useful features for modern web development, mostly focused on making the web development experience much faster and more pleasant.</span></span> <span data-ttu-id="ec55a-151">Podstawowe informacje dotyczące sieci Web można zainstalować z galerii rozszerzeń programu Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="ec55a-151">You can install Web Essentials from the Extension Gallery in Visual Studio.</span></span>

<span data-ttu-id="ec55a-152">**Łącze przeglądarki** to nowa funkcja uwzględnione w programie Visual Studio 2013, która udostępnia kanał między środowiska IDE programu Visual Studio i otwórz przeglądarki do wymiany danych między aplikacji sieci web i Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="ec55a-152">**Browser Link** is a new feature included in Visual Studio 2013 that provides a channel between the Visual Studio IDE and any open browser to exchange data between your web application and Visual Studio.</span></span> <span data-ttu-id="ec55a-153">Podstawowe informacje dotyczące sieci Web rozszerza łączy przeglądarki z narzędziami do modyfikowania obiektu modelu DOM i style CSS stron sieci web bezpośrednio z przeglądarki.</span><span class="sxs-lookup"><span data-stu-id="ec55a-153">Web Essentials extends Browser Link with tools to manipulate the DOM object model and the CSS styles of your web pages directly from the browser.</span></span>

<span data-ttu-id="ec55a-154">W tym ćwiczeniu zostanie Eksploruj niektóre funkcje obsługiwane przez **Web Essentials** i **łącze przeglądarki** ulepszyć stronę kwizu proste.</span><span class="sxs-lookup"><span data-stu-id="ec55a-154">In this exercise, you will explore some of the features supported by **Web Essentials** and **Browser Link** to enhance a simple quiz page.</span></span>

<a id="Ex1Task1"></a>
#### <a name="task-1---running-the-project-in-multiple-browsers"></a><span data-ttu-id="ec55a-155">Zadanie 1 - uruchamiania projektu w wielu przeglądarkach</span><span class="sxs-lookup"><span data-stu-id="ec55a-155">Task 1 - Running the Project in Multiple Browsers</span></span>

<span data-ttu-id="ec55a-156">W tym zadaniu skonfiguruj aplikację sieci web do uruchamiania w różnych przeglądarkach, które jest przydatna przy testowaniu różnych przeglądarkach.</span><span class="sxs-lookup"><span data-stu-id="ec55a-156">In this task, you will configure your web application to run in multiple browsers at once, which is useful for cross-browser testing.</span></span>

1. <span data-ttu-id="ec55a-157">Otwórz **programu Microsoft Visual Studio**.</span><span class="sxs-lookup"><span data-stu-id="ec55a-157">Open **Microsoft Visual Studio**.</span></span>
2. <span data-ttu-id="ec55a-158">W **pliku** menu, wybierz opcję **Otwórz | Projekt/rozwiązanie...**  i przejdź do **Ex1 WorkingwithBrowserLinkandWebEssentials\Begin** w **źródła** folderu laboratorium (C:\WebCampsTK\HOL\VSWebTooling\Source).</span><span class="sxs-lookup"><span data-stu-id="ec55a-158">In the **File** menu, select **Open | Project/Solution...** and browse to **Ex1-WorkingwithBrowserLinkandWebEssentials\Begin** in the **Source** folder of the lab (C:\WebCampsTK\HOL\VSWebTooling\Source).</span></span> <span data-ttu-id="ec55a-159">Wybierz **Begin.sln** i kliknij przycisk **Otwórz**.</span><span class="sxs-lookup"><span data-stu-id="ec55a-159">Select **Begin.sln** and click **Open**.</span></span>
3. <span data-ttu-id="ec55a-160">Na pasku narzędzi programu Visual Studio rozwiń menu przeglądarki i wybierz **przeglądanie za pomocą...** .</span><span class="sxs-lookup"><span data-stu-id="ec55a-160">In the Visual Studio toolbar, expand the browser menu and select **Browse With...**.</span></span>

    <span data-ttu-id="ec55a-161">![Przeglądanie za pomocą opcji menu](visual-studio-2013-web-tools/_static/image1.png "Przeglądaj z menu przeglądarki")</span><span class="sxs-lookup"><span data-stu-id="ec55a-161">![Browse With menu option](visual-studio-2013-web-tools/_static/image1.png "Browse with... in browser menu")</span></span>

    <span data-ttu-id="ec55a-162">*Przeglądanie za pomocą opcji menu*</span><span class="sxs-lookup"><span data-stu-id="ec55a-162">*Browse With menu option*</span></span>
4. <span data-ttu-id="ec55a-163">W **przeglądanie za pomocą** okno dialogowe, wybierz **Google Chrome** i **programu Internet Explorer** , przytrzymując **CTRL** klucza, a następnie kliknij przycisk  **Ustaw jako domyślny**.</span><span class="sxs-lookup"><span data-stu-id="ec55a-163">In the **Browse With** dialog box, select both **Google Chrome** and **Internet Explorer** by holding down the **CTRL** key and click **Set as Default**.</span></span>

    <span data-ttu-id="ec55a-164">![Przeglądaj okna dialogowego](visual-studio-2013-web-tools/_static/image2.png "Przeglądaj okna dialogowego")</span><span class="sxs-lookup"><span data-stu-id="ec55a-164">![Browse with dialog box](visual-studio-2013-web-tools/_static/image2.png "Browse with dialog box")</span></span>

    <span data-ttu-id="ec55a-165">*Wybieranie wielu domyślną przeglądarką*</span><span class="sxs-lookup"><span data-stu-id="ec55a-165">*Selecting multiple default browsers*</span></span>
5. <span data-ttu-id="ec55a-166">Zarówno Google Chrome, jak i przeglądarki Internet Explorer powinien zostać wyświetlony jako domyślnej przeglądarki.</span><span class="sxs-lookup"><span data-stu-id="ec55a-166">Both Google Chrome and Internet Explorer should now appear as the default browsers.</span></span> <span data-ttu-id="ec55a-167">Kliknij przycisk **anulować** aby zamknąć okno dialogowe.</span><span class="sxs-lookup"><span data-stu-id="ec55a-167">Click **Cancel** to close the dialog box.</span></span>

    <span data-ttu-id="ec55a-168">![Google Chrome i przeglądarki Internet Explorer jako domyślnej przeglądarki](visual-studio-2013-web-tools/_static/image3.png "domyślnej przeglądarce Google Chrome i przeglądarki Internet Explorer")</span><span class="sxs-lookup"><span data-stu-id="ec55a-168">![Google Chrome and Internet Explorer as default browsers](visual-studio-2013-web-tools/_static/image3.png "Google Chrome and Internet Explorer default browsers")</span></span>

    <span data-ttu-id="ec55a-169">*Google Chrome i przeglądarki Internet Explorer jako domyślnej przeglądarki*</span><span class="sxs-lookup"><span data-stu-id="ec55a-169">*Google Chrome and Internet Explorer as default browsers*</span></span>

    > [!NOTE]
    > <span data-ttu-id="ec55a-170">Po skonfigurowaniu domyślną przeglądarką **wielu przeglądarek** wybrano opcję w menu przeglądarki.</span><span class="sxs-lookup"><span data-stu-id="ec55a-170">After configuring the default browsers, the **Multiple Browsers** option is selected in the browser menu.</span></span>
    > 
    > <span data-ttu-id="ec55a-171">![Wiele przeglądarek](visual-studio-2013-web-tools/_static/image4.png "wielu przeglądarek")</span><span class="sxs-lookup"><span data-stu-id="ec55a-171">![Multiple browsers](visual-studio-2013-web-tools/_static/image4.png "Multiple browsers")</span></span>
6. <span data-ttu-id="ec55a-172">Naciśnij klawisz **CTRL** + **F5** do uruchomienia aplikacji bez debugowania.</span><span class="sxs-lookup"><span data-stu-id="ec55a-172">Press **CTRL** + **F5** to run the application without debugging.</span></span>
7. <span data-ttu-id="ec55a-173">Po otwarciu oba okna przeglądarki, umieść jeden z nich nad drugim w celu wyświetlenia aktualizacji w przeglądarkach obu jednocześnie.</span><span class="sxs-lookup"><span data-stu-id="ec55a-173">When both browser windows open, place one of them above the other in order to see the updates on both browsers simultaneously.</span></span> <span data-ttu-id="ec55a-174">Strony sieci web z prostokąt blue jasny powinien być wyświetlany przeglądarek.</span><span class="sxs-lookup"><span data-stu-id="ec55a-174">The browsers should display a web page with a light-blue rectangle.</span></span>

    <span data-ttu-id="ec55a-175">![Wprowadzenie do przeglądarki jeden nad drugim](visual-studio-2013-web-tools/_static/image5.png "umieszczenie przeglądarki jeden nad drugim")</span><span class="sxs-lookup"><span data-stu-id="ec55a-175">![Placing one browser above the other](visual-studio-2013-web-tools/_static/image5.png "Placing one browser above the other")</span></span>

    <span data-ttu-id="ec55a-176">*Wprowadzenie do przeglądarki jeden nad drugim*</span><span class="sxs-lookup"><span data-stu-id="ec55a-176">*Placing one browser above the other*</span></span>
8. <span data-ttu-id="ec55a-177">Nie zamykaj przeglądarek.</span><span class="sxs-lookup"><span data-stu-id="ec55a-177">Do not close the browsers.</span></span> <span data-ttu-id="ec55a-178">Użyjesz ich w następnego zadania.</span><span class="sxs-lookup"><span data-stu-id="ec55a-178">You will use them in the next task.</span></span>

<a id="Ex1Task2"></a>
#### <a name="task-2---using-zen-coding-to-create-html-elements"></a><span data-ttu-id="ec55a-179">Zadanie 2 — Zen przy użyciu kodowania do tworzenia elementów HTML</span><span class="sxs-lookup"><span data-stu-id="ec55a-179">Task 2 - Using Zen Coding to Create HTML Elements</span></span>

<span data-ttu-id="ec55a-180">**Kodowanie Zen** jest edytorem kodowania dodatek plug-in dla szybkich HTML, XML, XSL (lub innego formatu strukturalny) i edytowania.</span><span class="sxs-lookup"><span data-stu-id="ec55a-180">**Zen Coding** is an editor plugin for high-speed HTML, XML, XSL (or any other structured code format) coding and editing.</span></span> <span data-ttu-id="ec55a-181">Podstawowe ten dodatek jest aparat skrót zaawansowanych, dzięki czemu można rozwinąć wyrażeń — podobnie jak selektory CSS — kod HTML.</span><span class="sxs-lookup"><span data-stu-id="ec55a-181">The core of this plugin is a powerful abbreviation engine which allows you to expand expressions -similar to CSS selectors- into HTML code.</span></span> <span data-ttu-id="ec55a-182">Kodowanie Zen jest szybkim sposobem zapisu składni selektor stylu HTML za pomocą CSS.</span><span class="sxs-lookup"><span data-stu-id="ec55a-182">Zen Coding is a fast way to write HTML using a CSS style selector syntax.</span></span>

<span data-ttu-id="ec55a-183">W tym ćwiczeniu funkcję kodowania Zen podał Essentials sieci Web użyje do określenia przyciski HTML, które reprezentują opcje pytania.</span><span class="sxs-lookup"><span data-stu-id="ec55a-183">In this exercise, you will use the Zen Coding feature provided by Web Essentials to generate the HTML buttons that represent the options of the question.</span></span>

1. <span data-ttu-id="ec55a-184">Przełącz się do programu Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="ec55a-184">Switch back to Visual Studio.</span></span>
2. <span data-ttu-id="ec55a-185">Otwórz **Index.cshtml** plik znajdujący się w **widoków** | **Home** folderu.</span><span class="sxs-lookup"><span data-stu-id="ec55a-185">Open the **Index.cshtml** file located in the **Views** | **Home** folder.</span></span>
3. <span data-ttu-id="ec55a-186">Zastąp **&lt;!--TODO: Dodaj tutaj--opcje&gt;** komentarza z następującego kodu i naciśnij klawisz **kartę**.</span><span class="sxs-lookup"><span data-stu-id="ec55a-186">Replace the **&lt;!-- TODO: add options here--&gt;** comment with the following code, and press **TAB**.</span></span>

    [!code-css[Main](visual-studio-2013-web-tools/samples/sample1.css)]
4. <span data-ttu-id="ec55a-187">Kod powinny być rozwijane w formacie HTML.</span><span class="sxs-lookup"><span data-stu-id="ec55a-187">The code should be expanded to HTML.</span></span>

    <span data-ttu-id="ec55a-188">![Expanded HTML](visual-studio-2013-web-tools/_static/image6.png "Expanded HTML")</span><span class="sxs-lookup"><span data-stu-id="ec55a-188">![Expanded HTML](visual-studio-2013-web-tools/_static/image6.png "Expanded HTML")</span></span>

    <span data-ttu-id="ec55a-189">*Expanded HTML*</span><span class="sxs-lookup"><span data-stu-id="ec55a-189">*Expanded HTML*</span></span>

    > [!NOTE]
    > <span data-ttu-id="ec55a-190">Aby dowiedzieć się więcej o składni Zen kodowania, zobacz następujące tematy [artykułu](http://www.johnpapa.net/zen-coding-in-visual-studio-2012/).</span><span class="sxs-lookup"><span data-stu-id="ec55a-190">To learn more about Zen Coding syntax, see the following [article](http://www.johnpapa.net/zen-coding-in-visual-studio-2012/).</span></span>
5. <span data-ttu-id="ec55a-191">Kliknij przycisk **odświeżyć połączonej przeglądarki** przycisk, aby zaktualizować obie przeglądarki.</span><span class="sxs-lookup"><span data-stu-id="ec55a-191">Click the **Refresh linked browsers** button to update both browsers.</span></span>

    <span data-ttu-id="ec55a-192">![Odśwież połączonej przeglądarki](visual-studio-2013-web-tools/_static/image7.png "odświeżyć połączonej przeglądarki")</span><span class="sxs-lookup"><span data-stu-id="ec55a-192">![Refresh linked browsers](visual-studio-2013-web-tools/_static/image7.png "Refresh linked browsers")</span></span>

    <span data-ttu-id="ec55a-193">*Odśwież połączonej przeglądarki*</span><span class="sxs-lookup"><span data-stu-id="ec55a-193">*Refresh linked browsers*</span></span>

    <span data-ttu-id="ec55a-194">![Internet Explorer - aktualizacji strony z czterech przycisków](visual-studio-2013-web-tools/_static/image8.png "programu Internet Explorer — strona aktualizowana z czterech przycisków")</span><span class="sxs-lookup"><span data-stu-id="ec55a-194">![Internet Explorer - Page updated with four buttons](visual-studio-2013-web-tools/_static/image8.png "Internet Explorer - Page updated with four buttons")</span></span>

    <span data-ttu-id="ec55a-195">*Internet Explorer - aktualizacji strony z czterech przycisków*</span><span class="sxs-lookup"><span data-stu-id="ec55a-195">*Internet Explorer - Page updated with four buttons*</span></span>

    <span data-ttu-id="ec55a-196">![Google Chrome — strona aktualizowana z czterech przycisków](visual-studio-2013-web-tools/_static/image9.png "Google Chrome — strona aktualizowana z czterech przycisków")</span><span class="sxs-lookup"><span data-stu-id="ec55a-196">![Google Chrome - Page updated with four buttons](visual-studio-2013-web-tools/_static/image9.png "Google Chrome - Page updated with four buttons")</span></span>

    <span data-ttu-id="ec55a-197">*Google Chrome — strona aktualizowana z czterech przycisków*</span><span class="sxs-lookup"><span data-stu-id="ec55a-197">*Google Chrome - Page updated with four buttons*</span></span>
6. <span data-ttu-id="ec55a-198">Przełącz się do programu Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="ec55a-198">Switch back to Visual Studio.</span></span>
7. <span data-ttu-id="ec55a-199">Przyciski ma być dodany do strony, ale nadal konieczne jest dodanie pytania szablonu.</span><span class="sxs-lookup"><span data-stu-id="ec55a-199">You have added the buttons to the page, but you still need to add a template question.</span></span> <span data-ttu-id="ec55a-200">Aby to zrobić, użyjesz nową funkcją w Essentials sieci Web o nazwie **generator Lorem Ipsum**.</span><span class="sxs-lookup"><span data-stu-id="ec55a-200">To do so, you will use a new feature in Web Essentials called **Lorem Ipsum generator**.</span></span> <span data-ttu-id="ec55a-201">Zlokalizuj **div** element z **klasy** atrybutu **przodu**.</span><span class="sxs-lookup"><span data-stu-id="ec55a-201">Locate the **div** element with the **class** attribute **front**.</span></span>
8. <span data-ttu-id="ec55a-202">Dodaj następujący kod jako pierwszy element podrzędny **div**i naciśnij klawisz **kartę**.</span><span class="sxs-lookup"><span data-stu-id="ec55a-202">Add the following code as the first child element of the **div**, and press **TAB**.</span></span>

    [!code-css[Main](visual-studio-2013-web-tools/samples/sample2.css)]
9. <span data-ttu-id="ec55a-203">Kod powinny być rozwijane w formacie HTML.</span><span class="sxs-lookup"><span data-stu-id="ec55a-203">The code should be expanded to HTML.</span></span>

    <span data-ttu-id="ec55a-204">![Automatycznie wygenerowany Lorem Ipsum](visual-studio-2013-web-tools/_static/image10.png "Lorem Ipsum wygenerowana automatycznie")</span><span class="sxs-lookup"><span data-stu-id="ec55a-204">![Lorem Ipsum autogenerated](visual-studio-2013-web-tools/_static/image10.png "Lorem Ipsum autogenerated")</span></span>

    <span data-ttu-id="ec55a-205">*Automatycznie wygenerowany Lorem Ipsum*</span><span class="sxs-lookup"><span data-stu-id="ec55a-205">*Lorem Ipsum autogenerated*</span></span>

    > [!NOTE]
    > <span data-ttu-id="ec55a-206">W ramach Zen kodowania można teraz wygenerować kod Lorem Ipsum bezpośrednio w edytorze HTML.</span><span class="sxs-lookup"><span data-stu-id="ec55a-206">As part of Zen Coding, you can now generate Lorem Ipsum code directly in the HTML editor.</span></span> <span data-ttu-id="ec55a-207">Po prostu wpisz **lorem** i trafień **kartę** i 30 word Lorem Ipsum zostanie wstawiony tekst.</span><span class="sxs-lookup"><span data-stu-id="ec55a-207">Simply type **lorem** and hit **TAB** and a 30 word Lorem Ipsum text will be inserted.</span></span> <span data-ttu-id="ec55a-208">Np.</span><span class="sxs-lookup"><span data-stu-id="ec55a-208">E.g.</span></span> <span data-ttu-id="ec55a-209">*lorem10* wstawia 10 Lorem Ipsum słów.</span><span class="sxs-lookup"><span data-stu-id="ec55a-209">*lorem10* inserts 10 Lorem Ipsum words.</span></span>
10. <span data-ttu-id="ec55a-210">Logo, które zostaną dodane w górnej części zapytania przy użyciu kolejną nową funkcją w Essentials sieci Web o nazwie **generator pikseli Lorem**.</span><span class="sxs-lookup"><span data-stu-id="ec55a-210">You will add a logo at the top of the question by using another new feature in Web Essentials called **Lorem Pixel generator**.</span></span> <span data-ttu-id="ec55a-211">Dodaj następujący kod jako pierwszy element podrzędny **div** element z **kontenera** jako **klasy** wartości, a następnie naciśnij klawisz **kartę**.</span><span class="sxs-lookup"><span data-stu-id="ec55a-211">Add the following code as the first child element of the **div** element with **container** as **class** value, and press **TAB**.</span></span>

    [!code-css[Main](visual-studio-2013-web-tools/samples/sample3.css)]
11. <span data-ttu-id="ec55a-212">Kod należy rozszerzyć w formacie HTML.</span><span class="sxs-lookup"><span data-stu-id="ec55a-212">The code should expand to HTML.</span></span>

    <span data-ttu-id="ec55a-213">![Wygenerowany automatycznie pikseli Lorem](visual-studio-2013-web-tools/_static/image11.png "wygenerowana automatycznie Lorem pikseli")</span><span class="sxs-lookup"><span data-stu-id="ec55a-213">![Lorem Pixel autogenerated](visual-studio-2013-web-tools/_static/image11.png "Lorem Pixel autogenerated")</span></span>

    <span data-ttu-id="ec55a-214">*Automatycznie wygenerowany Lorem pikseli*</span><span class="sxs-lookup"><span data-stu-id="ec55a-214">*Lorem Pixel autogenerated*</span></span>

    > [!NOTE]
    > <span data-ttu-id="ec55a-215">W ramach Zen kodowania można również generować kod pikseli Lorem bezpośrednio w edytorze HTML.</span><span class="sxs-lookup"><span data-stu-id="ec55a-215">As part of Zen Coding, you can also generate Lorem Pixel code directly in the HTML editor.</span></span> <span data-ttu-id="ec55a-216">Po prostu wpisz **pix-200 x 200-zwierząt** i trafień **kartę** i **img** tagu z obrazem 200 x 200 zwierzęcia zostanie wstawiony.</span><span class="sxs-lookup"><span data-stu-id="ec55a-216">Simply type **pix-200x200-animals** and hit **TAB** and an **img** tag with a 200x200 image of an animal will be inserted.</span></span> <span data-ttu-id="ec55a-217">Aby uzyskać więcej informacji, zapoznaj się [pikseli Lorem](http://www.lorempixel.com).</span><span class="sxs-lookup"><span data-stu-id="ec55a-217">For more information, refer to [Lorem Pixel](http://www.lorempixel.com).</span></span>
12. <span data-ttu-id="ec55a-218">Kliknij przycisk **odświeżyć połączonej przeglądarki** przycisk, aby zaktualizować obie przeglądarki.</span><span class="sxs-lookup"><span data-stu-id="ec55a-218">Click the **Refresh linked browsers** button to update both browsers.</span></span>

    <span data-ttu-id="ec55a-219">![Internet Explorer - automatycznie wygenerowany obraz i tekst](visual-studio-2013-web-tools/_static/image12.png "programu Internet Explorer - automatycznie wygenerowany obraz i tekst")</span><span class="sxs-lookup"><span data-stu-id="ec55a-219">![Internet Explorer - Autogenerated image and text](visual-studio-2013-web-tools/_static/image12.png "Internet Explorer - Autogenerated image and text")</span></span>

    <span data-ttu-id="ec55a-220">*Internet Explorer - automatycznie wygenerowany obraz i tekst*</span><span class="sxs-lookup"><span data-stu-id="ec55a-220">*Internet Explorer - Autogenerated image and text*</span></span>

    <span data-ttu-id="ec55a-221">![Google Chrome — automatycznie wygenerowany obraz i tekst](visual-studio-2013-web-tools/_static/image13.png "Google Chrome — automatycznie wygenerowany obraz i tekst")</span><span class="sxs-lookup"><span data-stu-id="ec55a-221">![Google Chrome - Autogenerated image and text](visual-studio-2013-web-tools/_static/image13.png "Google Chrome - Autogenerated image and text")</span></span>

    <span data-ttu-id="ec55a-222">*Google Chrome — automatycznie wygenerowany obraz i tekst*</span><span class="sxs-lookup"><span data-stu-id="ec55a-222">*Google Chrome - Autogenerated image and text*</span></span>

    > [!NOTE]
    > <span data-ttu-id="ec55a-223">Ponieważ obraz jest wybierane losowo podczas dodawania fragment kodu, obraz wyświetlany w przeglądarkach mogą się różnić.</span><span class="sxs-lookup"><span data-stu-id="ec55a-223">Because the image is selected randomly when adding the code snippet, the image shown in the browsers may differ.</span></span>
13. <span data-ttu-id="ec55a-224">Nie zamykaj przeglądarek.</span><span class="sxs-lookup"><span data-stu-id="ec55a-224">Do not close the browsers.</span></span> <span data-ttu-id="ec55a-225">Użyjesz ich w następnego zadania.</span><span class="sxs-lookup"><span data-stu-id="ec55a-225">You will use them in the next task.</span></span>

<a id="Ex1Task3"></a>
#### <a name="task-3---updating-a-style-property"></a><span data-ttu-id="ec55a-226">Zadanie 3 — aktualizowanie właściwości stylu</span><span class="sxs-lookup"><span data-stu-id="ec55a-226">Task 3 - Updating a Style Property</span></span>

<span data-ttu-id="ec55a-227">W tym zadaniu użyje łącze przeglądarki **trybu inspekcji** funkcji wykrywania dokładnej lokalizacji, w którym jest generowany określonego elementu DOM, a następnie zaktualizuj właściwości kolor tego elementu próbnika kolorów pochodzącymi z sieci Web Essentials.</span><span class="sxs-lookup"><span data-stu-id="ec55a-227">In this task, you will use the Browser Link's **Inspect Mode** feature to detect the exact location where the specific DOM element is generated and then update the color property of that element using a color picker provided by Web Essentials.</span></span>

1. <span data-ttu-id="ec55a-228">W przeglądarce Internet Explorer, naciśnij klawisz **CTRL** + **ALT** + **I** Aby włączyć tryb inspekcji.</span><span class="sxs-lookup"><span data-stu-id="ec55a-228">In the Internet Explorer browser, press **CTRL** + **ALT** + **I** to enable Inspect Mode.</span></span>
2. <span data-ttu-id="ec55a-229">Wskaźnika na światła niebieskie obramowanie, a następnie kliknij przycisk.</span><span class="sxs-lookup"><span data-stu-id="ec55a-229">Move the pointer over the light blue border and click.</span></span>

    <span data-ttu-id="ec55a-230">![Przesunięcie kursora nad światła niebieskie obramowanie](visual-studio-2013-web-tools/_static/image14.png "przesunięciu wskaźnika na granicy światła niebieski")</span><span class="sxs-lookup"><span data-stu-id="ec55a-230">![Moving the pointer over the light blue border](visual-studio-2013-web-tools/_static/image14.png "Moving the pointer over the light blue border")</span></span>

    <span data-ttu-id="ec55a-231">*Przesunięcie kursora nad światła niebieskie obramowanie*</span><span class="sxs-lookup"><span data-stu-id="ec55a-231">*Moving the pointer over the light blue border*</span></span>
3. <span data-ttu-id="ec55a-232">Przełącz się do programu Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="ec55a-232">Switch back to Visual Studio.</span></span> <span data-ttu-id="ec55a-233">Zwróć uwagę, jak również wybrano element HTML, który wybrano w przeglądarce w edytorze programu Visual Studio HTML.</span><span class="sxs-lookup"><span data-stu-id="ec55a-233">Notice how the HTML element that you selected in the browser is also selected in the Visual Studio HTML editor.</span></span>

    <span data-ttu-id="ec55a-234">![Element HTML zaznaczonego w edytorze programu Visual Studio HTML](visual-studio-2013-web-tools/_static/image15.png "zaznaczonego w edytorze programu Visual Studio HTML elementu HTML")</span><span class="sxs-lookup"><span data-stu-id="ec55a-234">![HTML element selected in the Visual Studio HTML editor](visual-studio-2013-web-tools/_static/image15.png "HTML element selected in the Visual Studio HTML editor")</span></span>

    <span data-ttu-id="ec55a-235">*Element HTML zaznaczonego w edytorze programu Visual Studio HTML*</span><span class="sxs-lookup"><span data-stu-id="ec55a-235">*HTML element selected in the Visual Studio HTML editor*</span></span>
4. <span data-ttu-id="ec55a-236">Teraz zaktualizuje **przodu** klasy CSS, aby można było zmienić style wybranego elementu.</span><span class="sxs-lookup"><span data-stu-id="ec55a-236">You will now update the **front** CSS class in order to change the styling of the selected element.</span></span> <span data-ttu-id="ec55a-237">Aby to zrobić, naciśnij klawisz **CTRL** + **,** otworzyć **przejdź do** pola wyszukiwania.</span><span class="sxs-lookup"><span data-stu-id="ec55a-237">To do so, press **CTRL** + **,** to open the **Navigate To** search box.</span></span> <span data-ttu-id="ec55a-238">Typ **site.css** i naciśnij klawisz **ENTER** można otworzyć pliku.</span><span class="sxs-lookup"><span data-stu-id="ec55a-238">Type **site.css** and press **ENTER** to open the file.</span></span>

    <span data-ttu-id="ec55a-239">![Otwieranie pliku Site.css](visual-studio-2013-web-tools/_static/image16.png "otwierania pliku Site.css")</span><span class="sxs-lookup"><span data-stu-id="ec55a-239">![Opening file Site.css](visual-studio-2013-web-tools/_static/image16.png "Opening file Site.css")</span></span>

    <span data-ttu-id="ec55a-240">*Otwieranie pliku Site.css*</span><span class="sxs-lookup"><span data-stu-id="ec55a-240">*Opening file Site.css*</span></span>
5. <span data-ttu-id="ec55a-241">Naciśnij klawisz **CTRL** + **F** i typ **.front .flip container** można znaleźć selektora CSS.</span><span class="sxs-lookup"><span data-stu-id="ec55a-241">Press **CTRL** + **F** and type **.flip-containter .front** to find the CSS selector.</span></span>
6. <span data-ttu-id="ec55a-242">Kliknij światła niebieski kwadrat we właściwości obramowania klasy, aby otworzyć próbnika kolorów.</span><span class="sxs-lookup"><span data-stu-id="ec55a-242">Click the light blue square in the border property of the class to open the Color Picker.</span></span>

    <span data-ttu-id="ec55a-243">![Otwieranie próbnika kolorów](visual-studio-2013-web-tools/_static/image17.png "otwierania próbnika kolorów")</span><span class="sxs-lookup"><span data-stu-id="ec55a-243">![Opening the Color Picker](visual-studio-2013-web-tools/_static/image17.png "Opening the Color Picker")</span></span>

    <span data-ttu-id="ec55a-244">*Otwieranie próbnika kolorów*</span><span class="sxs-lookup"><span data-stu-id="ec55a-244">*Opening the Color Picker*</span></span>
7. <span data-ttu-id="ec55a-245">Rozwiń próbnika kolorów, klikając przycisk i wybierz nowy kolor.</span><span class="sxs-lookup"><span data-stu-id="ec55a-245">Expand the Color Picker by clicking the chevron button and select a new color.</span></span>

    <span data-ttu-id="ec55a-246">![Rozszerzanie próbnika kolorów](visual-studio-2013-web-tools/_static/image18.png "rozszerzania próbnika kolorów")</span><span class="sxs-lookup"><span data-stu-id="ec55a-246">![Expanding the Color Picker](visual-studio-2013-web-tools/_static/image18.png "Expanding the Color Picker")</span></span>

    <span data-ttu-id="ec55a-247">*Rozszerzanie próbnika kolorów*</span><span class="sxs-lookup"><span data-stu-id="ec55a-247">*Expanding the Color Picker*</span></span>
8. <span data-ttu-id="ec55a-248">Naciśnij klawisz **CTRL** + **ALT** + **ENTER** można odświeżyć połączonej przeglądarki.</span><span class="sxs-lookup"><span data-stu-id="ec55a-248">Press **CTRL** + **ALT** + **ENTER** to refresh linked browsers.</span></span>
9. <span data-ttu-id="ec55a-249">Przełącz się do programu Internet Explorer i zwróć uwagę, jak został zmieniony kolor obramowania.</span><span class="sxs-lookup"><span data-stu-id="ec55a-249">Switch to Internet Explorer and notice how the color of the border has changed.</span></span>

    <span data-ttu-id="ec55a-250">![Internet Explorer - kolor obramowania zaktualizowane](visual-studio-2013-web-tools/_static/image19.png "programu Internet Explorer — kolor obramowania zaktualizowane")</span><span class="sxs-lookup"><span data-stu-id="ec55a-250">![Internet Explorer - Border color updated](visual-studio-2013-web-tools/_static/image19.png "Internet Explorer - Border color updated")</span></span>

    <span data-ttu-id="ec55a-251">*Internet Explorer - kolor obramowania zaktualizowane*</span><span class="sxs-lookup"><span data-stu-id="ec55a-251">*Internet Explorer - Border color updated*</span></span>
10. <span data-ttu-id="ec55a-252">Przełącz do Google Chrome i zwróć uwagę, jak został zmieniony kolor obramowania.</span><span class="sxs-lookup"><span data-stu-id="ec55a-252">Switch to Google Chrome and notice how the color of the border has changed.</span></span>

    <span data-ttu-id="ec55a-253">![Google Chrome — kolor obramowania zaktualizowane](visual-studio-2013-web-tools/_static/image20.png "Google Chrome — kolor obramowania zaktualizowane")</span><span class="sxs-lookup"><span data-stu-id="ec55a-253">![Google Chrome - Border color updated](visual-studio-2013-web-tools/_static/image20.png "Google Chrome - Border color updated")</span></span>

    <span data-ttu-id="ec55a-254">*Google Chrome — kolor obramowania zaktualizowane*</span><span class="sxs-lookup"><span data-stu-id="ec55a-254">*Google Chrome - Border color updated*</span></span>
11. <span data-ttu-id="ec55a-255">Przełącz się do programu Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="ec55a-255">Switch back to Visual Studio.</span></span>
12. <span data-ttu-id="ec55a-256">Przejdź do końca **Site.css** plik i naciśnij przycisk **CTRL** + **F** zlokalizować **.btn** selektora.</span><span class="sxs-lookup"><span data-stu-id="ec55a-256">Go to the end of the **Site.css** file and press **CTRL** + **F** to locate the **.btn** selector.</span></span>
13. <span data-ttu-id="ec55a-257">Zwróć uwagę, że **- webkit-border-radius** właściwości jest podkreślone na zielono.</span><span class="sxs-lookup"><span data-stu-id="ec55a-257">Notice that the **-webkit-border-radius** property is underlined in green.</span></span>

    <span data-ttu-id="ec55a-258">![Właściwość - webkit-border-radius selektora btn](visual-studio-2013-web-tools/_static/image21.png "- webkit-border-radius właściwości selektora btn")</span><span class="sxs-lookup"><span data-stu-id="ec55a-258">![-webkit-border-radius property of the btn selector](visual-studio-2013-web-tools/_static/image21.png "-webkit-border-radius property of the btn selector")</span></span>

    <span data-ttu-id="ec55a-259">*Właściwość - webkit-border-radius selektora btn*</span><span class="sxs-lookup"><span data-stu-id="ec55a-259">*-webkit-border-radius property of the btn selector*</span></span>
14. <span data-ttu-id="ec55a-260">Umieść punkt wstawiania w **- webkit-border-radius** właściwości.</span><span class="sxs-lookup"><span data-stu-id="ec55a-260">Place the caret in the **-webkit-border-radius** property.</span></span> <span data-ttu-id="ec55a-261">Niebieski linii powinny być wyświetlane w obszarze pierwszą literę słowa pierwszej właściwości.</span><span class="sxs-lookup"><span data-stu-id="ec55a-261">A blue line should appear under the first letter of the first word of the property.</span></span> <span data-ttu-id="ec55a-262">Jest to **tagów inteligentnych**.</span><span class="sxs-lookup"><span data-stu-id="ec55a-262">This is the **smart tag**.</span></span>
15. <span data-ttu-id="ec55a-263">Naciśnij klawisz **CTRL** + **.**</span><span class="sxs-lookup"><span data-stu-id="ec55a-263">Press **CTRL** + **.**</span></span> <span data-ttu-id="ec55a-264">Aby otworzyć menu sugestie i kliknij przycisk **Dodawanie właściwości standardowych (border-radius)**.</span><span class="sxs-lookup"><span data-stu-id="ec55a-264">to open the suggestions menu and click **Add missing standard property (border-radius)**.</span></span>

    <span data-ttu-id="ec55a-265">![Dodaj brakujące sugestię standardowe właściwości](visual-studio-2013-web-tools/_static/image22.png "Dodaj Brak sugestii właściwości standardowych")</span><span class="sxs-lookup"><span data-stu-id="ec55a-265">![Add missing standard property suggestion](visual-studio-2013-web-tools/_static/image22.png "Add missing standard property suggestion")</span></span>

    <span data-ttu-id="ec55a-266">*Dodaj brakujące sugestię właściwości standardowych*</span><span class="sxs-lookup"><span data-stu-id="ec55a-266">*Add missing standard property suggestion*</span></span>
16. <span data-ttu-id="ec55a-267">**Border-radius** właściwość jest automatycznie dodawany do reguły CSS.</span><span class="sxs-lookup"><span data-stu-id="ec55a-267">The **border-radius** property is automatically added to the CSS rule.</span></span>

    <span data-ttu-id="ec55a-268">![Brak właściwości standardowe dodane](visual-studio-2013-web-tools/_static/image23.png "dodane Brak właściwości standardowych")</span><span class="sxs-lookup"><span data-stu-id="ec55a-268">![Missing standard property added](visual-studio-2013-web-tools/_static/image23.png "Missing standard property added")</span></span>

    <span data-ttu-id="ec55a-269">*Brak właściwości standardowe dodane*</span><span class="sxs-lookup"><span data-stu-id="ec55a-269">*Missing standard property added*</span></span>
17. <span data-ttu-id="ec55a-270">Przesuń wskaźnik myszy nad **border-radius** właściwość, aby wyświetlić **tooltip macierzy przeglądarki**.</span><span class="sxs-lookup"><span data-stu-id="ec55a-270">Move the pointer over the **border-radius** property to display the **Browser matrix tooltip**.</span></span> <span data-ttu-id="ec55a-271">**Tooltip macierzy przeglądarki** przedstawia dostępność właściwości w każdej przeglądarki.</span><span class="sxs-lookup"><span data-stu-id="ec55a-271">The **Browser matrix tooltip** shows the availability of the property in each browser.</span></span>

    <span data-ttu-id="ec55a-272">![Etykietka narzędzia macierzy przeglądarki](visual-studio-2013-web-tools/_static/image24.png "tooltip macierzy przeglądarki")</span><span class="sxs-lookup"><span data-stu-id="ec55a-272">![Browser matrix tooltip](visual-studio-2013-web-tools/_static/image24.png "Browser matrix tooltip")</span></span>

    <span data-ttu-id="ec55a-273">*Etykietka narzędzia macierzy przeglądarki*</span><span class="sxs-lookup"><span data-stu-id="ec55a-273">*Browser matrix tooltip*</span></span>
18. <span data-ttu-id="ec55a-274">Zwróć uwagę, że wartość **border-radius** właściwość jest nadal podkreślony.</span><span class="sxs-lookup"><span data-stu-id="ec55a-274">Notice that the value of the **border-radius** property is still underlined.</span></span> <span data-ttu-id="ec55a-275">Wskaźnika na wartość, aby wyświetlić komunikat ostrzegawczy.</span><span class="sxs-lookup"><span data-stu-id="ec55a-275">Move the pointer over the value to see the warning message.</span></span>

    <span data-ttu-id="ec55a-276">![Ostrzeżenie wartość właściwości border-radius](visual-studio-2013-web-tools/_static/image25.png "ostrzeżenie wartość właściwości Border-radius")</span><span class="sxs-lookup"><span data-stu-id="ec55a-276">![Border-radius property value warning](visual-studio-2013-web-tools/_static/image25.png "Border-radius property value warning")</span></span>

    <span data-ttu-id="ec55a-277">*Ostrzeżenie wartość właściwości border-radius*</span><span class="sxs-lookup"><span data-stu-id="ec55a-277">*Border-radius property value warning*</span></span>
19. <span data-ttu-id="ec55a-278">Usuń jednostki **border-radius** wartość właściwości zgodnie z sugestią podaną przez etykietkę narzędzia.</span><span class="sxs-lookup"><span data-stu-id="ec55a-278">Remove the unit of the **border-radius** property value as suggested by the tooltip.</span></span>
20. <span data-ttu-id="ec55a-279">Jako **border-radius** jest standardowe właściwości do definiowania obramowania jak zaokrąglone narożniki są, możesz usunąć **- webkit-border-radius** właściwości i wartości z reguły CSS.</span><span class="sxs-lookup"><span data-stu-id="ec55a-279">As **border-radius** is the standard property for defining how rounded border corners are, you can remove the **-webkit-border-radius** property and value from the CSS rule.</span></span>
21. <span data-ttu-id="ec55a-280">Umieść punkt wstawiania w **zawijanie** właściwości i zwróć uwagę, że tagów inteligentnych również pojawia się poniżej.</span><span class="sxs-lookup"><span data-stu-id="ec55a-280">Place the caret in the **word-wrap** property and notice that the smart tag also appears below.</span></span>
22. <span data-ttu-id="ec55a-281">Otworzyć menu, a następnie kliknij przycisk **Podaj szczegóły dostawcy Brak**.</span><span class="sxs-lookup"><span data-stu-id="ec55a-281">Open the menu and click **Add missing vendor specifics**.</span></span>

    <span data-ttu-id="ec55a-282">![Dodaj brakujące sugestię szczegóły dostawcy](visual-studio-2013-web-tools/_static/image26.png "Dodaj brakujące sugestię szczegóły dostawcy")</span><span class="sxs-lookup"><span data-stu-id="ec55a-282">![Add missing vendor specifics suggestion](visual-studio-2013-web-tools/_static/image26.png "Add missing vendor specifics suggestion")</span></span>

    <span data-ttu-id="ec55a-283">*Dodaj brakujące sugestię szczegóły dostawcy*</span><span class="sxs-lookup"><span data-stu-id="ec55a-283">*Add missing vendor specifics suggestion*</span></span>
23. <span data-ttu-id="ec55a-284">**-Ms-zawijanie** właściwość jest automatycznie dodawany do reguły CSS.</span><span class="sxs-lookup"><span data-stu-id="ec55a-284">The **-ms-word-wrap** property is automatically added to the CSS rule.</span></span>

    <span data-ttu-id="ec55a-285">![Dostawcy określoną właściwość dodane](visual-studio-2013-web-tools/_static/image27.png "dostawcy dodać określoną właściwość.")</span><span class="sxs-lookup"><span data-stu-id="ec55a-285">![Vendor specific property added](visual-studio-2013-web-tools/_static/image27.png "Vendor specific property added")</span></span>

    <span data-ttu-id="ec55a-286">*Właściwość określonego dostawcy dodane*</span><span class="sxs-lookup"><span data-stu-id="ec55a-286">*Vendor specific property added*</span></span>

<a id="Ex1Task4"></a>
#### <a name="task-4---updating-the-html-code-from-the-browser"></a><span data-ttu-id="ec55a-287">Zadanie 4. aktualizowanie kodu HTML z przeglądarki</span><span class="sxs-lookup"><span data-stu-id="ec55a-287">Task 4 - Updating the HTML Code from the Browser</span></span>

<span data-ttu-id="ec55a-288">W tym zadaniu użyje łącze przeglądarki **tryb projektowania** funkcji do edycji obiektu DOM z przeglądarki i przetransferować zmiany wprowadzone do pliku źródła HTML programu Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="ec55a-288">In this task, you will use the Browser Link's **Design Mode** feature to edit the DOM object from the browser and transfer the changes to the HTML source file in Visual Studio.</span></span>

1. <span data-ttu-id="ec55a-289">Google Chrome, naciśnij klawisz **CTRL** + **ALT** + **D** Aby włączyć tryb projektowania.</span><span class="sxs-lookup"><span data-stu-id="ec55a-289">In Google Chrome, press **CTRL** + **ALT** + **D** to enable Design Mode.</span></span>
2. <span data-ttu-id="ec55a-290">Przesuń wskaźnik myszy nad **Lorem Ipsum dolor sit amet** etykiety, a następnie kliknij przycisk.</span><span class="sxs-lookup"><span data-stu-id="ec55a-290">Move the pointer over the **Lorem Ipsum dolor sit amet** label and click.</span></span>

    <span data-ttu-id="ec55a-291">![Edytowanie pytania](visual-studio-2013-web-tools/_static/image28.png "Edytowanie pytania")</span><span class="sxs-lookup"><span data-stu-id="ec55a-291">![Editing the question](visual-studio-2013-web-tools/_static/image28.png "Editing the question")</span></span>

    <span data-ttu-id="ec55a-292">*Edytowanie pytania*</span><span class="sxs-lookup"><span data-stu-id="ec55a-292">*Editing the question*</span></span>
3. <span data-ttu-id="ec55a-293">Powinna zostać wyświetlona kursora.</span><span class="sxs-lookup"><span data-stu-id="ec55a-293">A cursor should appear.</span></span> <span data-ttu-id="ec55a-294">Zastąp oryginalny tekst z *jak go wygląda po napisać dłużej pytanie?*, a następnie naciśnij klawisz **ESC** aby zakończyć tryb projektowania.</span><span class="sxs-lookup"><span data-stu-id="ec55a-294">Replace the original text with *What does it look like when I write a longer question?*, and then press **ESC** to exit Design Mode.</span></span>

    <span data-ttu-id="ec55a-295">![Pytanie edytować](visual-studio-2013-web-tools/_static/image29.png "edytować pytanie")</span><span class="sxs-lookup"><span data-stu-id="ec55a-295">![Question edited](visual-studio-2013-web-tools/_static/image29.png "Question edited")</span></span>

    <span data-ttu-id="ec55a-296">*Pytanie edytować*</span><span class="sxs-lookup"><span data-stu-id="ec55a-296">*Question edited*</span></span>
4. <span data-ttu-id="ec55a-297">Przełącz do programu Visual Studio i Otwórz **Index.cshtml**, jeśli nie jest jeszcze otwarty.</span><span class="sxs-lookup"><span data-stu-id="ec55a-297">Switch back to Visual Studio and open **Index.cshtml**, if not already opened.</span></span> <span data-ttu-id="ec55a-298">Należy zauważyć, że tekst wewnętrzny z **&lt;p&gt;** element został zaktualizowany.</span><span class="sxs-lookup"><span data-stu-id="ec55a-298">Notice that the inner text of the **&lt;p&gt;** element has been updated.</span></span>

    <span data-ttu-id="ec55a-299">![Pytanie zaktualizowane na stronie HTML](visual-studio-2013-web-tools/_static/image30.png "pytanie zaktualizowane strony HTML")</span><span class="sxs-lookup"><span data-stu-id="ec55a-299">![Updated question in the HTML page](visual-studio-2013-web-tools/_static/image30.png "Updated question in the HTML page")</span></span>

    <span data-ttu-id="ec55a-300">*Zaktualizowano pytanie na stronie HTML*</span><span class="sxs-lookup"><span data-stu-id="ec55a-300">*Updated question in the HTML page*</span></span>

<a id="Ex1Task5"></a>
#### <a name="task-5---reviewing-seo-related-warnings"></a><span data-ttu-id="ec55a-301">Zadanie 5 - recenzowania optymalizacji dla aparatów wyszukiwania związane z ostrzeżeniami</span><span class="sxs-lookup"><span data-stu-id="ec55a-301">Task 5 - Reviewing SEO Related Warnings</span></span>

<span data-ttu-id="ec55a-302">**Optymalizacji dla aparatów wyszukiwania** (SEO) to proces polegający na wprowadzaniu wyższej pozycję witryny sieci Web na liście wyników z wyszukiwarki.</span><span class="sxs-lookup"><span data-stu-id="ec55a-302">**Search Engine Optimization** (SEO) is the process of making a website rank higher on a search engine's list of results.</span></span> <span data-ttu-id="ec55a-303">Im większa jest lokacji i bardziej spójnie to wymienione, więcej odwiedzających lokacji pobierze z tego aparatu wyszukiwania.</span><span class="sxs-lookup"><span data-stu-id="ec55a-303">The higher the site ranks and the more consistently it is listed, the more visitors the site will get from that search engine.</span></span> <span data-ttu-id="ec55a-304">Essentials sieci Web zawiera narzędzie analityczne, który sprawdza HTML, znaleziono raporty problemów i zapewnia pomoc, aby je rozwiązać.</span><span class="sxs-lookup"><span data-stu-id="ec55a-304">Web Essentials incorporates an analytical tool that examines HTML, reports the issues found and provides assistance to fix them.</span></span>

1. <span data-ttu-id="ec55a-305">Przejdź do **widoku** menu i kliknij przycisk **listy błędów** otworzyć **listy błędów** okna.</span><span class="sxs-lookup"><span data-stu-id="ec55a-305">Go to the **View** menu and click **Error List** to open the **Error List** window.</span></span>

    <span data-ttu-id="ec55a-306">![Błąd w widoku listy menu](visual-studio-2013-web-tools/_static/image31.png "listy błędów w menu Widok")</span><span class="sxs-lookup"><span data-stu-id="ec55a-306">![Error List in View menu](visual-studio-2013-web-tools/_static/image31.png "Error List in View menu")</span></span>

    <span data-ttu-id="ec55a-307">*Błąd w widoku listy menu*</span><span class="sxs-lookup"><span data-stu-id="ec55a-307">*Error List in View menu*</span></span>
2. <span data-ttu-id="ec55a-308">Zwróć uwagę, że istnieje ostrzeżenie optymalizacji dla aparatów wyszukiwania z informacją, że **&lt;meta&gt;** tagu dla Brak opisu strony.</span><span class="sxs-lookup"><span data-stu-id="ec55a-308">Notice that there is an SEO warning notifying that a **&lt;meta&gt;** tag for the page description is missing.</span></span> <span data-ttu-id="ec55a-309">Kliknij dwukrotnie wpis ostrzeżenie optymalizacji dla aparatów wyszukiwania, aby go rozwiązać.</span><span class="sxs-lookup"><span data-stu-id="ec55a-309">Double-click the SEO warning entry to fix it.</span></span>

    <span data-ttu-id="ec55a-310">![Okno listy błędów](visual-studio-2013-web-tools/_static/image32.png "w oknie Lista błędów")</span><span class="sxs-lookup"><span data-stu-id="ec55a-310">![Error List window](visual-studio-2013-web-tools/_static/image32.png "Error List window")</span></span>

    <span data-ttu-id="ec55a-311">*Okno listy błędów*</span><span class="sxs-lookup"><span data-stu-id="ec55a-311">*Error List window*</span></span>
3. <span data-ttu-id="ec55a-312">W **Web Essentials** okno dialogowe, kliknij przycisk **tak** do wstawienia opis &lt;meta&gt; tagu.</span><span class="sxs-lookup"><span data-stu-id="ec55a-312">In the **Web Essentials** dialog box, click **Yes** to insert a description &lt;meta&gt; tag.</span></span>

    <span data-ttu-id="ec55a-313">![Okno dialogowe Essentials Web](visual-studio-2013-web-tools/_static/image33.png "Essentials sieci Web — okno dialogowe")</span><span class="sxs-lookup"><span data-stu-id="ec55a-313">![Web Essentials dialog box](visual-studio-2013-web-tools/_static/image33.png "Web Essentials dialog box")</span></span>

    <span data-ttu-id="ec55a-314">*Okno dialogowe Essentials sieci Web*</span><span class="sxs-lookup"><span data-stu-id="ec55a-314">*Web Essentials dialog box*</span></span>
4. <span data-ttu-id="ec55a-315">Edytor dla  **\_Layout.cshtml** otwiera i **&lt;meta&gt;** tag jest automatycznie dodawany do **head** sekcji Plik HTML.</span><span class="sxs-lookup"><span data-stu-id="ec55a-315">The editor for **\_Layout.cshtml** opens and the **&lt;meta&gt;** tag is automatically added to the **head** section of the HTML file.</span></span>

    <span data-ttu-id="ec55a-316">![Tag meta automatycznie dodane na stronie _Layout](visual-studio-2013-web-tools/_static/image34.png "metatag automatycznie dodane w _Layout strony")</span><span class="sxs-lookup"><span data-stu-id="ec55a-316">![Meta tag automatically added in _Layout page](visual-studio-2013-web-tools/_static/image34.png "Meta tag automatically added in _Layout page")</span></span>

    <span data-ttu-id="ec55a-317">*Tag meta automatycznie dodawane do \_układ strony*</span><span class="sxs-lookup"><span data-stu-id="ec55a-317">*Meta tag automatically added to \_Layout page*</span></span>
5. <span data-ttu-id="ec55a-318">Zmień wartość **zawartości** atrybutu *GeekQuiz* i Zapisz plik.</span><span class="sxs-lookup"><span data-stu-id="ec55a-318">Change the value of the **content** attribute to *GeekQuiz* and save the file.</span></span>

<a id="Exercise2"></a>
### <a name="exercise-2-taking-advantage-of-code-snippets-and-intellisense"></a><span data-ttu-id="ec55a-319">Ćwiczenie 2: Korzystanie z IntelliSense i wstawki kodu</span><span class="sxs-lookup"><span data-stu-id="ec55a-319">Exercise 2: Taking Advantage of Code Snippets and IntelliSense</span></span>

<span data-ttu-id="ec55a-320">Z sieci Web Essentials edytora HTML został rozszerzony o dodatkowe funkcje.</span><span class="sxs-lookup"><span data-stu-id="ec55a-320">With Web Essentials, the HTML editor has been extended with extra functionality.</span></span> <span data-ttu-id="ec55a-321">W tym ćwiczeniu zostanie wyświetlona niektóre nowe funkcje, które są przydatne podczas tworzenia aplikacji sieci web.</span><span class="sxs-lookup"><span data-stu-id="ec55a-321">In this exercise, you will see some new features that are helpful when developing web applications.</span></span>

<a id="Ex2Task1"></a>
#### <a name="task-1---using-intellisense-in-html-documents"></a><span data-ttu-id="ec55a-322">Zadanie 1 — za pomocą funkcji IntelliSense w dokumentach HTML</span><span class="sxs-lookup"><span data-stu-id="ec55a-322">Task 1 - Using IntelliSense in HTML Documents</span></span>

<span data-ttu-id="ec55a-323">Pierwszy nowa funkcja pojawi się w ramach tego zadania jest nazywany **dynamiczne IntelliSense**.</span><span class="sxs-lookup"><span data-stu-id="ec55a-323">The first new feature you will see in this task is called **Dynamic IntelliSense**.</span></span> <span data-ttu-id="ec55a-324">Dynamiczne IntelliSense odczytuje inne znaczniki i atrybuty do wywnioskowania możliwych identyfikatorów, które będą używane.</span><span class="sxs-lookup"><span data-stu-id="ec55a-324">Dynamic IntelliSense reads other tags and attributes to infer the possible ids you will use.</span></span>

<span data-ttu-id="ec55a-325">W ramach tego zadania spowoduje utworzenie nowego elementu formularza HTML, który zawiera etykiety i pola wejściowego.</span><span class="sxs-lookup"><span data-stu-id="ec55a-325">In this task, you will create a new HTML form element which contains a label and an input field.</span></span> <span data-ttu-id="ec55a-326">Następnie dodasz **dla** atrybutu do etykiety, aby powiązać go z danych wejściowych, i zobaczysz sugestie IntelliSense oparte na identyfikatory danych wejściowych w zakresie.</span><span class="sxs-lookup"><span data-stu-id="ec55a-326">Then you will add a **for** attribute to the label to bind it to the input, and you will see IntelliSense suggestions based on the ids of the inputs in scope.</span></span>

1. <span data-ttu-id="ec55a-327">Otwórz **programu Visual Studio Express 2013 for Web** i **Begin.sln** rozwiązania, znajdujących się w **źródło/Ex2-TakingAdvantageofCodeSnippetsandIntelliSense/Begin** folderu.</span><span class="sxs-lookup"><span data-stu-id="ec55a-327">Open **Visual Studio Express 2013 for Web** and the **Begin.sln** solution located in the **Source/Ex2-TakingAdvantageofCodeSnippetsandIntelliSense/Begin** folder.</span></span> <span data-ttu-id="ec55a-328">Alternatywnie możesz kontynuować z rozwiązaniem uzyskanymi w poprzednim ćwiczeniu.</span><span class="sxs-lookup"><span data-stu-id="ec55a-328">Alternatively, you can continue with the solution that you obtained in the previous exercise.</span></span>
2. <span data-ttu-id="ec55a-329">W **Eksploratora rozwiązań**, otwórz **Index.cshtml** plik znajdujący się w **widoków** | **Home** folderu.</span><span class="sxs-lookup"><span data-stu-id="ec55a-329">In **Solution Explorer**, open the **Index.cshtml** file located in the **Views** | **Home** folder.</span></span>
3. <span data-ttu-id="ec55a-330">Dodaj następującą postać wewnątrz **&lt;sekcji&gt;** elementu.</span><span class="sxs-lookup"><span data-stu-id="ec55a-330">Add the following form inside the **&lt;section&gt;** element.</span></span>

    <span data-ttu-id="ec55a-331">(Fragment - kodu *VisualStudio2013WebTooling* - *Ex2* - *formularz*)</span><span class="sxs-lookup"><span data-stu-id="ec55a-331">(Code Snippet - *VisualStudio2013WebTooling* - *Ex2* - *Form*)</span></span>

    [!code-html[Main](visual-studio-2013-web-tools/samples/sample4.html)]
4. <span data-ttu-id="ec55a-332">Etykieta z niektórych opis pola powinien być poprzedzony tagu wejściowego.</span><span class="sxs-lookup"><span data-stu-id="ec55a-332">The input tag should be preceded by a label with some description of the field.</span></span> <span data-ttu-id="ec55a-333">Dodaj następujące etykiety przed tagu wejściowego.</span><span class="sxs-lookup"><span data-stu-id="ec55a-333">Add the following label before the input tag.</span></span>

    [!code-html[Main](visual-studio-2013-web-tools/samples/sample5.html)]
5. <span data-ttu-id="ec55a-334">**Dla** atrybutu **&lt;etykiety&gt;** Określa, który element formularza etykietę jest powiązany.</span><span class="sxs-lookup"><span data-stu-id="ec55a-334">The **for** attribute of a **&lt;label&gt;** specifies which form element a label is bound to.</span></span> <span data-ttu-id="ec55a-335">Wartość atrybutu powinna być równa identyfikator elementu pokrewne.</span><span class="sxs-lookup"><span data-stu-id="ec55a-335">The attribute's value should be equal to the id of the related element.</span></span> <span data-ttu-id="ec55a-336">Dodaj **dla** atrybutu **&lt;etykiety&gt;** elementu.</span><span class="sxs-lookup"><span data-stu-id="ec55a-336">Add the **for** attribute to the **&lt;label&gt;** element.</span></span> <span data-ttu-id="ec55a-337">Jak pokazano na poniższej ilustracji, &quot;nazwa&quot; wartość pojawia się w polu IntelliSense, na podstawie identyfikatora elementów w tym samym zakresie (otaczający  **&lt;formularza&gt;**).</span><span class="sxs-lookup"><span data-stu-id="ec55a-337">As shown in the following figure, the &quot;name&quot; value pops up in the IntelliSense box, based on the id of the elements within the same scope (the enclosing **&lt;form&gt;**).</span></span>

    <span data-ttu-id="ec55a-338">![Wyświetlanie w IntelliSense identyfikator](visual-studio-2013-web-tools/_static/image35.png "zawierające identyfikator w IntelliSense")</span><span class="sxs-lookup"><span data-stu-id="ec55a-338">![Showing the id in IntelliSense](visual-studio-2013-web-tools/_static/image35.png "Showing the id in IntelliSense")</span></span>

    <span data-ttu-id="ec55a-339">*Wyświetlanie w IntelliSense identyfikator*</span><span class="sxs-lookup"><span data-stu-id="ec55a-339">*Showing the id in IntelliSense*</span></span>
6. <span data-ttu-id="ec55a-340">Usuń ostatnio dodany **&lt;formularza&gt;** elementu i jego zawartości.</span><span class="sxs-lookup"><span data-stu-id="ec55a-340">Delete the recently added **&lt;form&gt;** element and its content.</span></span>

<a id="Ex2Task2"></a>
#### <a name="task-2---using-html-code-snippets"></a><span data-ttu-id="ec55a-341">Zadanie 2 — przy użyciu fragmentów kodu HTML</span><span class="sxs-lookup"><span data-stu-id="ec55a-341">Task 2 - Using HTML Code Snippets</span></span>

<span data-ttu-id="ec55a-342">HTML5 wprowadzić więcej niż 25 nowych znaczników semantyki.</span><span class="sxs-lookup"><span data-stu-id="ec55a-342">HTML5 introduced more than 25 new semantic tags.</span></span> <span data-ttu-id="ec55a-343">Visual Studio już obsługę funkcji IntelliSense na te tagi, ale programu Visual Studio 2013 pozwala szybciej i łatwiej można zapisać znacznika przez dodanie nowych fragmentów kodu.</span><span class="sxs-lookup"><span data-stu-id="ec55a-343">Visual Studio already had IntelliSense support for these tags, but Visual Studio 2013 makes it faster and easier to write markup by adding new code snippets.</span></span> <span data-ttu-id="ec55a-344">Chociaż te tagi nie są skomplikowane, pochodzą z kilku precyzyjnie małych, takie jak dodanie przejścia poprawnego kodera-dekodera dla *audio* tagu.</span><span class="sxs-lookup"><span data-stu-id="ec55a-344">Though these tags are not complicated, they come with a few small subtleties, such as adding the correct codec fallbacks for the *audio* tag.</span></span> <span data-ttu-id="ec55a-345">W tym zadaniu zobaczysz wstawki kodu HTML dla tagu audio.</span><span class="sxs-lookup"><span data-stu-id="ec55a-345">In this task, you will see the HTML code snippets for the audio tag.</span></span>

1. <span data-ttu-id="ec55a-346">W **Index.cshtml** plików, wpisz  **&lt;lub** wewnątrz **&lt;sekcji&gt;** element, jak pokazano na poniższej ilustracji.</span><span class="sxs-lookup"><span data-stu-id="ec55a-346">In the **Index.cshtml** file, type **&lt;aud** inside the **&lt;section&gt;** element as shown in the following figure.</span></span>

    <span data-ttu-id="ec55a-347">![Wstawianie audio element](visual-studio-2013-web-tools/_static/image36.png "Wstawianie audio element")</span><span class="sxs-lookup"><span data-stu-id="ec55a-347">![Inserting an audio element](visual-studio-2013-web-tools/_static/image36.png "Inserting an audio element")</span></span>

    <span data-ttu-id="ec55a-348">*Wstawianie audio element*</span><span class="sxs-lookup"><span data-stu-id="ec55a-348">*Inserting an audio element*</span></span>
2. <span data-ttu-id="ec55a-349">Naciśnij klawisz **kartę** dwa razy i zwróć uwagę, jak poniższy kod zostanie dodany na stronie i znajduje się kursor na **src** atrybutu pierwszego źródła.</span><span class="sxs-lookup"><span data-stu-id="ec55a-349">Press **TAB** twice and notice how the following code is added on the page and the cursor is placed on the **src** attribute of the first source.</span></span>

    [!code-html[Main](visual-studio-2013-web-tools/samples/sample6.html)]

    > [!NOTE]
    > <span data-ttu-id="ec55a-350">Naciskając **kartę** klucza dwukrotnie wstawieniu fragmentu kodu.</span><span class="sxs-lookup"><span data-stu-id="ec55a-350">By pressing the **TAB** key twice, the code snippet is inserted.</span></span> <span data-ttu-id="ec55a-351">Audio fragment kodu przedstawia standardowego użycia *audio* tag o dwa pliki źródłowe ulepszoną obsługę.</span><span class="sxs-lookup"><span data-stu-id="ec55a-351">The audio snippet shows the standard usage of the *audio* tag, with two source files for improved support.</span></span>
3. <span data-ttu-id="ec55a-352">Usuń drugi wiersz i aktualizowanie źródła pierwszego wiersza następujące łącze, aby WebCampsTV Katana Pokaz: [ http://media.ch9.ms/ch9/11d8/604b8163-fad3-4f12-9607-b404201211d8/KatanaProject.mp3 ](http://media.ch9.ms/ch9/11d8/604b8163-fad3-4f12-9607-b404201211d8/KatanaProject.mp3).</span><span class="sxs-lookup"><span data-stu-id="ec55a-352">Delete the second line and update the source of the first line with the following link to the WebCampsTV Katana show: [http://media.ch9.ms/ch9/11d8/604b8163-fad3-4f12-9607-b404201211d8/KatanaProject.mp3](http://media.ch9.ms/ch9/11d8/604b8163-fad3-4f12-9607-b404201211d8/KatanaProject.mp3).</span></span> <span data-ttu-id="ec55a-353">Poniżej przedstawiono wynikowy kod.</span><span class="sxs-lookup"><span data-stu-id="ec55a-353">The resulting code is shown below.</span></span>

    [!code-html[Main](visual-studio-2013-web-tools/samples/sample7.html)]

    > [!NOTE]
    > <span data-ttu-id="ec55a-354">Plik źródłowy *KatanaProject.mp3* służy jako przykład.</span><span class="sxs-lookup"><span data-stu-id="ec55a-354">The source file *KatanaProject.mp3* is used as an example.</span></span> <span data-ttu-id="ec55a-355">Jeśli wolisz, możesz użyć innego źródła.</span><span class="sxs-lookup"><span data-stu-id="ec55a-355">You can use another source if you prefer.</span></span>
4. <span data-ttu-id="ec55a-356">Naciśnij klawisz **CTRL** + **S** można zapisać pliku.</span><span class="sxs-lookup"><span data-stu-id="ec55a-356">Press **CTRL** + **S** to save the file.</span></span>
5. <span data-ttu-id="ec55a-357">Naciśnij klawisz **CTRL** + **F5** do uruchomienia aplikacji.</span><span class="sxs-lookup"><span data-stu-id="ec55a-357">Press **CTRL** + **F5** to start the application.</span></span>
6. <span data-ttu-id="ec55a-358">Należy zauważyć, że odtwarzacz audio została dodana do aplikacji.</span><span class="sxs-lookup"><span data-stu-id="ec55a-358">Notice that an audio player was added to the application.</span></span>

    <span data-ttu-id="ec55a-359">![Odtwarzacz audio w programie Internet Explorer](visual-studio-2013-web-tools/_static/image37.png "Audio player w programie Internet Explorer")</span><span class="sxs-lookup"><span data-stu-id="ec55a-359">![Audio player in Internet Explorer](visual-studio-2013-web-tools/_static/image37.png "Audio player in Internet Explorer")</span></span>

    <span data-ttu-id="ec55a-360">*Odtwarzacz audio w programie Internet Explorer*</span><span class="sxs-lookup"><span data-stu-id="ec55a-360">*Audio player in Internet Explorer*</span></span>

    <span data-ttu-id="ec55a-361">![Odtwarzacz audio w przeglądarce Google Chrome](visual-studio-2013-web-tools/_static/image38.png "odtwarzacz Audio w przeglądarce Google Chrome")</span><span class="sxs-lookup"><span data-stu-id="ec55a-361">![Audio player in Google Chrome](visual-studio-2013-web-tools/_static/image38.png "Audio player in Google Chrome")</span></span>

    <span data-ttu-id="ec55a-362">*Odtwarzacz audio w przeglądarce Google Chrome*</span><span class="sxs-lookup"><span data-stu-id="ec55a-362">*Audio player in Google Chrome*</span></span>
7. <span data-ttu-id="ec55a-363">Nie zamykaj przeglądarek.</span><span class="sxs-lookup"><span data-stu-id="ec55a-363">Do not close the browsers.</span></span> <span data-ttu-id="ec55a-364">Użyjesz ich w następnego zadania.</span><span class="sxs-lookup"><span data-stu-id="ec55a-364">You will use them in the next task.</span></span>

<a id="Ex2Task3"></a>
#### <a name="task-3---using-intellisense-in-javascript-documents"></a><span data-ttu-id="ec55a-365">Zadanie 3 — za pomocą funkcji IntelliSense w dokumentach JavaScript</span><span class="sxs-lookup"><span data-stu-id="ec55a-365">Task 3 - Using IntelliSense in JavaScript Documents</span></span>

<span data-ttu-id="ec55a-366">W przypadku sieci Web 2013 Essentials HTML strony i arkusze stylów drukować listę identyfikatorów i nazw klas.</span><span class="sxs-lookup"><span data-stu-id="ec55a-366">With Web Essentials 2013, style sheets and HTML pages produce a list of IDs and class names.</span></span> <span data-ttu-id="ec55a-367">W tym zadaniu dowiesz się, jak te listy poprawić obsługę funkcji IntelliSense języka JavaScript w sieci Web 2013 Essentials.</span><span class="sxs-lookup"><span data-stu-id="ec55a-367">In this task, you will learn how those lists improve JavaScript IntelliSense support in Web Essentials 2013.</span></span>

1. <span data-ttu-id="ec55a-368">W **Index.cshtml** pliku, Dodaj następujący kod, aby zdefiniować **skryptu** tag dla kodu JavaScript.</span><span class="sxs-lookup"><span data-stu-id="ec55a-368">In the **Index.cshtml** file, add the following code to define a **script** tag for JavaScript code.</span></span>

    [!code-cshtml[Main](visual-studio-2013-web-tools/samples/sample8.cshtml)]
2. <span data-ttu-id="ec55a-369">Dodaj następujący kod wewnątrz **skryptu** tag, aby zdefiniować funkcję wywołania zwrotnego gotowe.</span><span class="sxs-lookup"><span data-stu-id="ec55a-369">Add the following code inside the **script** tag to define the ready callback function.</span></span>

    <span data-ttu-id="ec55a-370">(Fragment - kodu *VisualStudio2013WebTooling* - *Ex2* - *ReadyFunction*)</span><span class="sxs-lookup"><span data-stu-id="ec55a-370">(Code Snippet - *VisualStudio2013WebTooling* - *Ex2* - *ReadyFunction*)</span></span>

    [!code-javascript[Main](visual-studio-2013-web-tools/samples/sample9.js)]
3. <span data-ttu-id="ec55a-371">Umieść punkt wstawiania w **skryptu** tagu i naciśnij klawisz **CTRL** + **.**</span><span class="sxs-lookup"><span data-stu-id="ec55a-371">Place the caret in the **script** tag and press **CTRL** + **.**</span></span> <span data-ttu-id="ec55a-372">Aby otworzyć menu uwag.</span><span class="sxs-lookup"><span data-stu-id="ec55a-372">to open the suggestion menu.</span></span>
4. <span data-ttu-id="ec55a-373">Kliknij przycisk **wyodrębnić pliku**.</span><span class="sxs-lookup"><span data-stu-id="ec55a-373">Click **Extract To File**.</span></span>

    <span data-ttu-id="ec55a-374">![Wyodrębnij JavaScript do pliku sugestii](visual-studio-2013-web-tools/_static/image39.png "JavaScript wyodrębnić sugestią pliku")</span><span class="sxs-lookup"><span data-stu-id="ec55a-374">![JavaScript extract to file suggestion](visual-studio-2013-web-tools/_static/image39.png "JavaScript extract to file suggestion")</span></span>

    <span data-ttu-id="ec55a-375">*Wyodrębnij JavaScript do pliku sugestii*</span><span class="sxs-lookup"><span data-stu-id="ec55a-375">*JavaScript extract to file suggestion*</span></span>
5. <span data-ttu-id="ec55a-376">W **Zapisz jako** wybierz **skryptów** folderu, nazwa pliku **init.js** i kliknij przycisk **zapisać**.</span><span class="sxs-lookup"><span data-stu-id="ec55a-376">In the **Save As** window, select the **Scripts** folder, name the file **init.js** and click **Save**.</span></span>

    <span data-ttu-id="ec55a-377">![Zapisz jako okno](visual-studio-2013-web-tools/_static/image40.png "okno Zapisz jako")</span><span class="sxs-lookup"><span data-stu-id="ec55a-377">![Save As window](visual-studio-2013-web-tools/_static/image40.png "Save As window")</span></span>

    <span data-ttu-id="ec55a-378">*Okno Zapisz jako*</span><span class="sxs-lookup"><span data-stu-id="ec55a-378">*Save As window*</span></span>

    > [!NOTE]
    > <span data-ttu-id="ec55a-379">**Init.js** plik jest tworzony i zawartość skryptu zostanie przeniesiony do pliku.</span><span class="sxs-lookup"><span data-stu-id="ec55a-379">The **init.js** file is created and the content of the script is moved to the file.</span></span>
    > 
    > <span data-ttu-id="ec55a-380">![Utworzone za pomocą zawartość pliku init.js](visual-studio-2013-web-tools/_static/image41.png "Init.js pliku utworzonego za pomocą zawartość")</span><span class="sxs-lookup"><span data-stu-id="ec55a-380">![Init.js file created with the content included](visual-studio-2013-web-tools/_static/image41.png "Init.js file created with the content included")</span></span>
    > 
    > <span data-ttu-id="ec55a-381">*Init.js pliku utworzonego za pomocą zawartość*</span><span class="sxs-lookup"><span data-stu-id="ec55a-381">*Init.js file created with the content included*</span></span>
6. <span data-ttu-id="ec55a-382">Otwórz **Index.cshtml** plik i sprawdź, czy tag skryptu został zastąpiony w odniesieniu do **init.js** pliku.</span><span class="sxs-lookup"><span data-stu-id="ec55a-382">Open the **Index.cshtml** file and check that the script tag was replaced with a reference to the **init.js** file.</span></span>

    <span data-ttu-id="ec55a-383">![Odwołanie html init.js](visual-studio-2013-web-tools/_static/image42.png "Init.js odwołania html")</span><span class="sxs-lookup"><span data-stu-id="ec55a-383">![Init.js html reference](visual-studio-2013-web-tools/_static/image42.png "Init.js html reference")</span></span>

    <span data-ttu-id="ec55a-384">*Init.js html reference*</span><span class="sxs-lookup"><span data-stu-id="ec55a-384">*Init.js html reference*</span></span>
7. <span data-ttu-id="ec55a-385">Przejdź do **Eksploratora rozwiązań** i zwróć uwagę, że **init.js** pliku została automatycznie uwzględniona w rozwiązaniu.</span><span class="sxs-lookup"><span data-stu-id="ec55a-385">Go to the **Solution Explorer** and notice that the **init.js** file was included automatically in the solution.</span></span>

    <span data-ttu-id="ec55a-386">![Plik init.js zawartych w rozwiązaniu](visual-studio-2013-web-tools/_static/image43.png "pliku Init.js zawartych w rozwiązaniu")</span><span class="sxs-lookup"><span data-stu-id="ec55a-386">![Init.js file included in solution](visual-studio-2013-web-tools/_static/image43.png "Init.js file included in solution")</span></span>

    <span data-ttu-id="ec55a-387">*Plik init.js zawartych w rozwiązaniu*</span><span class="sxs-lookup"><span data-stu-id="ec55a-387">*Init.js file included in solution*</span></span>
8. <span data-ttu-id="ec55a-388">Przywraca **init.js** plik, aby zaktualizować **gotowe** funkcji wywołania zwrotnego.</span><span class="sxs-lookup"><span data-stu-id="ec55a-388">Switch back to the **init.js** file to update the **ready** function callback.</span></span>
9. <span data-ttu-id="ec55a-389">W definicji funkcji wywołania zwrotnego, przekazywany do *gotowe*, Dodaj następujący kod, aby pobrać wszystkie elementy przez atrybut określonej klasy.</span><span class="sxs-lookup"><span data-stu-id="ec55a-389">Inside the function callback definition that is passed to *ready*, add the following code to get all the elements by a specific class attribute.</span></span>

    [!code-javascript[Main](visual-studio-2013-web-tools/samples/sample10.js)]
10. <span data-ttu-id="ec55a-390">Naciśnij klawisz **CTRL** + **miejsca** ująć w cudzysłów wewnątrz **getElementsByClassName** wywołania funkcji.</span><span class="sxs-lookup"><span data-stu-id="ec55a-390">Press **CTRL** + **Space** between the quotes inside the **getElementsByClassName** function call.</span></span>

    <span data-ttu-id="ec55a-391">![Wyświetlanie funkcji IntelliSense dla funkcji getElementsByClassName](visual-studio-2013-web-tools/_static/image44.png "przedstawiający IntelliSense dla funkcji getElementsByClassName")</span><span class="sxs-lookup"><span data-stu-id="ec55a-391">![Showing IntelliSense for the getElementsByClassName function](visual-studio-2013-web-tools/_static/image44.png "Showing IntelliSense for the getElementsByClassName function")</span></span>

    <span data-ttu-id="ec55a-392">*Wyświetlanie funkcji IntelliSense dla funkcji getElementsByClassName*</span><span class="sxs-lookup"><span data-stu-id="ec55a-392">*Showing IntelliSense for the getElementsByClassName function*</span></span>

    > [!NOTE]
    > <span data-ttu-id="ec55a-393">Należy zauważyć, że IntelliSense pokazuje klas zdefiniowanych w arkuszach stylów projektu.</span><span class="sxs-lookup"><span data-stu-id="ec55a-393">Notice that IntelliSense shows the classes defined in the project style sheets.</span></span>
11. <span data-ttu-id="ec55a-394">Zastąp wiersz utworzony w następującym kodem.</span><span class="sxs-lookup"><span data-stu-id="ec55a-394">Replace the line that you have created with the following code.</span></span>

    [!code-javascript[Main](visual-studio-2013-web-tools/samples/sample11.js)]
12. <span data-ttu-id="ec55a-395">Umieść kursor po **au** wewnątrz cudzysłowów w **getElementsByTagName** funkcji i naciśnij klawisz **CTRL** + **miejsca**.</span><span class="sxs-lookup"><span data-stu-id="ec55a-395">Position the cursor after **au** inside the quotes in the **getElementsByTagName** function and press **CTRL** + **Space**.</span></span>

    <span data-ttu-id="ec55a-396">![Wyświetlanie funkcji IntelliSense dla metody getElementByTagName](visual-studio-2013-web-tools/_static/image45.png "przedstawiający IntelliSense dla metody getElementByTagName")</span><span class="sxs-lookup"><span data-stu-id="ec55a-396">![Showing IntelliSense for the getElementByTagName method](visual-studio-2013-web-tools/_static/image45.png "Showing IntelliSense for the getElementByTagName method")</span></span>

    <span data-ttu-id="ec55a-397">*Wyświetlanie funkcji IntelliSense dla metody getElementsByTagName*</span><span class="sxs-lookup"><span data-stu-id="ec55a-397">*Showing IntelliSense for the getElementsByTagName method*</span></span>
13. <span data-ttu-id="ec55a-398">Wybierz **&quot;audio&quot;** z listy i kliknij **ENTER**.</span><span class="sxs-lookup"><span data-stu-id="ec55a-398">Select **&quot;audio&quot;** from the list and press **ENTER**.</span></span> <span data-ttu-id="ec55a-399">Wynik jest wyświetlany na poniższej ilustracji.</span><span class="sxs-lookup"><span data-stu-id="ec55a-399">The result is shown in the following figure.</span></span>

    <span data-ttu-id="ec55a-400">![Pobieranie elementów Audio](visual-studio-2013-web-tools/_static/image46.png "pobierania elementów Audio")</span><span class="sxs-lookup"><span data-stu-id="ec55a-400">![Retrieving Audio Elements](visual-studio-2013-web-tools/_static/image46.png "Retrieving Audio Elements")</span></span>

    <span data-ttu-id="ec55a-401">*Pobieranie elementów Audio*</span><span class="sxs-lookup"><span data-stu-id="ec55a-401">*Retrieving Audio Elements*</span></span>
14. <span data-ttu-id="ec55a-402">W **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy **init.js** w pliku **skryptów** i wybierz polecenie **pliki JavaScript zminimalizowania** z **Web Essentials** menu.</span><span class="sxs-lookup"><span data-stu-id="ec55a-402">In **Solution Explorer**, right-click the **init.js** file in the **Scripts** folder and select **Minify JavaScript file(s)** from the **Web Essentials** menu.</span></span>

    <span data-ttu-id="ec55a-403">![Zminimalizowania pliki JavaScript](visual-studio-2013-web-tools/_static/image47.png "pliki zminimalizowania JavaScript")</span><span class="sxs-lookup"><span data-stu-id="ec55a-403">![Minify JavaScript file(s)](visual-studio-2013-web-tools/_static/image47.png "Minify JavaScript files")</span></span>

    <span data-ttu-id="ec55a-404">*Minify JavaScript file(s)*</span><span class="sxs-lookup"><span data-stu-id="ec55a-404">*Minify JavaScript file(s)*</span></span>
15. <span data-ttu-id="ec55a-405">Po wyświetleniu monitu, aby włączyć automatyczne minimalizowanie kliknięcie zmiany pliku źródłowego **tak**.</span><span class="sxs-lookup"><span data-stu-id="ec55a-405">When prompted to enable automatic minification when the source file changes click **Yes**.</span></span>

    <span data-ttu-id="ec55a-406">![Włączanie automatycznego minimalizację ostrzeżenie](visual-studio-2013-web-tools/_static/image48.png "włączenie automatycznego minimalizację ostrzeżenie")</span><span class="sxs-lookup"><span data-stu-id="ec55a-406">![Enabling automatic minification warning](visual-studio-2013-web-tools/_static/image48.png "Enabling automatic minification warning")</span></span>

    <span data-ttu-id="ec55a-407">*Włączanie automatycznego minimalizację ostrzeżenie*</span><span class="sxs-lookup"><span data-stu-id="ec55a-407">*Enabling automatic minification warning*</span></span>

    > [!NOTE]
    > <span data-ttu-id="ec55a-408">**Init.min.js** zostanie utworzona i nie zostanie dodany jako zależność z **init.js** pliku.</span><span class="sxs-lookup"><span data-stu-id="ec55a-408">The **init.min.js** is created and is added as a dependency of the **init.js** file.</span></span>
    > 
    > <span data-ttu-id="ec55a-409">![Utworzony plik init.min.js](visual-studio-2013-web-tools/_static/image49.png "Init.min.js plik utworzony")</span><span class="sxs-lookup"><span data-stu-id="ec55a-409">![Init.min.js file created](visual-studio-2013-web-tools/_static/image49.png "Init.min.js file created")</span></span>
    > 
    > <span data-ttu-id="ec55a-410">*Utworzony plik init.min.js*</span><span class="sxs-lookup"><span data-stu-id="ec55a-410">*Init.min.js file created*</span></span>
16. <span data-ttu-id="ec55a-411">Otwórz **init.min.js** pliku i zwróć uwagę, że plik jest zminimalizowany.</span><span class="sxs-lookup"><span data-stu-id="ec55a-411">Open the **init.min.js** file and notice that the file is minified.</span></span>

    <span data-ttu-id="ec55a-412">![Zawartość pliku init.min.js](visual-studio-2013-web-tools/_static/image50.png "Init.min.js pliku zawartości")</span><span class="sxs-lookup"><span data-stu-id="ec55a-412">![Init.min.js file content](visual-studio-2013-web-tools/_static/image50.png "Init.min.js file content")</span></span>

    <span data-ttu-id="ec55a-413">*Zawartość pliku init.min.js*</span><span class="sxs-lookup"><span data-stu-id="ec55a-413">*Init.min.js file content*</span></span>
17. <span data-ttu-id="ec55a-414">W **init.js** pliku, Dodaj następujący kod poniżej **getElementsByTagName** wywołania funkcji do odtwarzania audio elementów.</span><span class="sxs-lookup"><span data-stu-id="ec55a-414">In the **init.js** file, add the following code below the **getElementsByTagName** function call to play all the audio elements.</span></span>

    <span data-ttu-id="ec55a-415">(Fragment - kodu *VisualStudio2013WebTooling* - *Ex2* - *PlayAudioElements*)</span><span class="sxs-lookup"><span data-stu-id="ec55a-415">(Code Snippet - *VisualStudio2013WebTooling* - *Ex2* - *PlayAudioElements*)</span></span>

    [!code-csharp[Main](visual-studio-2013-web-tools/samples/sample12.cs)]
18. <span data-ttu-id="ec55a-416">Kliknij przycisk **CTRL** + **S** można zapisać pliku.</span><span class="sxs-lookup"><span data-stu-id="ec55a-416">Click **CTRL** + **S** to save the file.</span></span> <span data-ttu-id="ec55a-417">Ponieważ zminimalizowany plik jest już otwarty, pojawi się okno dialogowe z informacją, że plik został zmodyfikowany poza edytorem źródła.</span><span class="sxs-lookup"><span data-stu-id="ec55a-417">Since the minified file is already opened, you will see a dialog box saying that the file was modified outside of the source editor.</span></span> <span data-ttu-id="ec55a-418">Kliknij przycisk **Tak**.</span><span class="sxs-lookup"><span data-stu-id="ec55a-418">Click **Yes**.</span></span>

    <span data-ttu-id="ec55a-419">![Ostrzeżenie programu Microsoft Visual Studio](visual-studio-2013-web-tools/_static/image51.png "ostrzeżenie programu Microsoft Visual Studio")</span><span class="sxs-lookup"><span data-stu-id="ec55a-419">![Microsoft Visual Studio warning](visual-studio-2013-web-tools/_static/image51.png "Microsoft Visual Studio warning")</span></span>

    <span data-ttu-id="ec55a-420">*Ostrzeżenie programu Microsoft Visual Studio*</span><span class="sxs-lookup"><span data-stu-id="ec55a-420">*Microsoft Visual Studio warning*</span></span>
19. <span data-ttu-id="ec55a-421">Przywraca **init.min.js** plik, aby sprawdzić, czy plik został zaktualizowany o nowy kod.</span><span class="sxs-lookup"><span data-stu-id="ec55a-421">Switch back to the **init.min.js** file to verify that the file was updated with the new code.</span></span>

    <span data-ttu-id="ec55a-422">![Zaktualizowany plik init.min.js](visual-studio-2013-web-tools/_static/image52.png "zaktualizowanego pliku Init.min.js")</span><span class="sxs-lookup"><span data-stu-id="ec55a-422">![Init.min.js file updated](visual-studio-2013-web-tools/_static/image52.png "Init.min.js file updated")</span></span>

    <span data-ttu-id="ec55a-423">*Zaktualizowany plik init.min.js*</span><span class="sxs-lookup"><span data-stu-id="ec55a-423">*Init.min.js file updated*</span></span>
20. <span data-ttu-id="ec55a-424">Kliknij przycisk **Odśwież łącze przeglądarki** przycisku.</span><span class="sxs-lookup"><span data-stu-id="ec55a-424">Click the **Browser Link Refresh** button.</span></span>
21. <span data-ttu-id="ec55a-425">Gdy obie przeglądarki są odświeżane odtwarzacze audio, który został wyświetlony w poprzednim zadaniu rozpocznie się automatyczne odtwarzanie.</span><span class="sxs-lookup"><span data-stu-id="ec55a-425">Once both browsers are refreshed the audio players you saw in the previous task will start playing automatically.</span></span>

    <span data-ttu-id="ec55a-426">![Odtwarzacz audio zawarte w widoku](visual-studio-2013-web-tools/_static/image53.png "odtwarzacz Audio zawarte w widoku")</span><span class="sxs-lookup"><span data-stu-id="ec55a-426">![Audio player included in view](visual-studio-2013-web-tools/_static/image53.png "Audio player included in view")</span></span>

    <span data-ttu-id="ec55a-427">*Odtwarzacz audio zawarte w widoku*</span><span class="sxs-lookup"><span data-stu-id="ec55a-427">*Audio player included in view*</span></span>

* * *

<a id="Summary"></a>
## <a name="summary"></a><span data-ttu-id="ec55a-428">Podsumowanie</span><span class="sxs-lookup"><span data-stu-id="ec55a-428">Summary</span></span>

<span data-ttu-id="ec55a-429">Wykonując tego laboratorium praktycznego wiesz już, jak:</span><span class="sxs-lookup"><span data-stu-id="ec55a-429">By completing this hands-on lab you have learned how to:</span></span>

- <span data-ttu-id="ec55a-430">Korzystać z nowych funkcji edytora HTML zawarte w Essentials sieci Web, takich jak rozbudowanych wstawek kodu HTML5 i Zen kodowania</span><span class="sxs-lookup"><span data-stu-id="ec55a-430">Use new HTML editor features included in Web Essentials such as rich HTML5 code snippets and Zen coding</span></span>
- <span data-ttu-id="ec55a-431">Korzystać z nowych funkcji Edytor CSS zawarte w Essentials sieci Web, takich jak próbnika kolorów i etykietka narzędzia macierzy przeglądarki</span><span class="sxs-lookup"><span data-stu-id="ec55a-431">Use new CSS editor features included in Web Essentials such as the Color picker and Browser matrix tooltip</span></span>
- <span data-ttu-id="ec55a-432">Korzystać z nowych funkcji edytora JavaScript zawarte w sieci Web Essentials, takich jak wyodrębniania do pliku i technologii IntelliSense dla wszystkich elementów HTML</span><span class="sxs-lookup"><span data-stu-id="ec55a-432">Use new JavaScript editor features included in Web Essentials such as Extract to File and IntelliSense for all HTML elements</span></span>
- <span data-ttu-id="ec55a-433">Wymiany danych między przeglądarką a Visual Studio przy użyciu Browser Link</span><span class="sxs-lookup"><span data-stu-id="ec55a-433">Exchange data between your browser and Visual Studio using Browser Link</span></span>
