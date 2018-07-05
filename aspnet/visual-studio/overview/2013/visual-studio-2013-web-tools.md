---
uid: visual-studio/overview/2013/visual-studio-2013-web-tools
title: 'Warsztaty: Visual Studio 2013 w sieci Web Tools | Dokumentacja firmy Microsoft'
author: rick-anderson
description: Visual Studio to środowisko doskonałymi metodami tworzenia oprogramowania. Na podstawie NET Windows oraz projekty sieci web. Zawiera on edytorem tekstu zaawansowane, która może być bez problemów używany na...
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/16/2014
ms.topic: article
ms.assetid: 09e82351-816b-402d-acd1-0f9ac6901d16
ms.technology: ''
msc.legacyurl: /visual-studio/overview/2013/visual-studio-2013-web-tools
msc.type: authoredcontent
ms.openlocfilehash: 8f0f19a184d50cdc6e8e5a2da0e55a0e8ca44dc5
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/03/2018
ms.locfileid: "37399403"
---
<a name="hands-on-lab-visual-studio-2013-web-tools"></a><span data-ttu-id="c8182-104">Warsztaty: Visual Studio 2013 w sieci Web Tools</span><span class="sxs-lookup"><span data-stu-id="c8182-104">Hands On Lab: Visual Studio 2013 Web Tools</span></span>
====================
<span data-ttu-id="c8182-105">Przez [Camp w sieci Web zespołu](https://twitter.com/webcamps)</span><span class="sxs-lookup"><span data-stu-id="c8182-105">by [Web Camps Team](https://twitter.com/webcamps)</span></span>

[<span data-ttu-id="c8182-106">Pobierz Camp Web szkolenia Kit</span><span class="sxs-lookup"><span data-stu-id="c8182-106">Download Web Camps Training Kit</span></span>](http://aka.ms/webcamps-training-kit)

> <span data-ttu-id="c8182-107">Visual Studio to środowisko doskonałymi metodami tworzenia oprogramowania. Na podstawie NET Windows oraz projekty sieci web.</span><span class="sxs-lookup"><span data-stu-id="c8182-107">Visual Studio is an excellent development environment for .NET-based Windows and web projects.</span></span> <span data-ttu-id="c8182-108">Obejmuje ona edytorem tekstu zaawansowane, która może być bez problemów używany do edytowania plików autonomicznych bez projektu.</span><span class="sxs-lookup"><span data-stu-id="c8182-108">It includes a powerful text editor that can easily be used to edit standalone files without a project.</span></span>
> 
> <span data-ttu-id="c8182-109">Program Visual Studio przechowuje drzewo analizy w pełni funkcjonalne w trakcie edycji każdego pliku.</span><span class="sxs-lookup"><span data-stu-id="c8182-109">Visual Studio maintains a full-featured parse tree as you edit each file.</span></span> <span data-ttu-id="c8182-110">Dzięki temu program Visual Studio zapewniają niezrównaną automatycznego uzupełniania i czynności na podstawie dokumentu podczas dokonywania środowisko programistyczne, znacznie szybszy i bardziej przyjemny.</span><span class="sxs-lookup"><span data-stu-id="c8182-110">This allows Visual Studio to provide unparalleled auto-completion and document-based actions while making the development experience much faster and more pleasant.</span></span> <span data-ttu-id="c8182-111">Funkcje te są szczególnie wydajne w dokumentach HTML i CSS.</span><span class="sxs-lookup"><span data-stu-id="c8182-111">These features are especially powerful in HTML and CSS documents.</span></span>
> 
> <span data-ttu-id="c8182-112">Wszystkie te możliwości jest również dostępna dla rozszerzenia, dzięki czemu można łatwo rozszerzyć edytorów oraz zaawansowanych nowych funkcji, zgodnie z potrzebami.</span><span class="sxs-lookup"><span data-stu-id="c8182-112">All of this power is also available for extensions, making it simple to extend the editors with powerful new features to suit your needs.</span></span> <span data-ttu-id="c8182-113">Web Essentials to kolekcja rozszerzeń programu Visual Studio (najczęściej) związane z sieci web.</span><span class="sxs-lookup"><span data-stu-id="c8182-113">Web Essentials is a collection of (mostly) web-related enhancements to Visual Studio.</span></span> <span data-ttu-id="c8182-114">Zawiera on też wiele nowych uzupełnianiu IntelliSense (szczególnie w przypadku CSS), nowe funkcje łączność z przeglądarkami, automatyczne plików JSHint dla języka JavaScript, nowe ostrzeżenia dla HTML, CSS i wiele innych funkcji, które są niezbędne do tworzenia nowoczesnej sieci web.</span><span class="sxs-lookup"><span data-stu-id="c8182-114">It includes lots of new IntelliSense completions (especially for CSS), new Browser Link features, automatic JSHint for JavaScript files, new warnings for HTML and CSS, and many other features that are essential to modern web development.</span></span>
> 
> <span data-ttu-id="c8182-115">Wszystkie przykładowy kod i fragmenty kodu są uwzględnione w sieci Web Camp zestaw szkoleniowy, dostępne pod adresem [ http://aka.ms/webcamps-training-kit ](http://aka.ms/webcamps-training-kit).</span><span class="sxs-lookup"><span data-stu-id="c8182-115">All sample code and snippets are included in the Web Camps Training Kit, available at [http://aka.ms/webcamps-training-kit](http://aka.ms/webcamps-training-kit).</span></span>


<a id="Overview"></a>
## <a name="overview"></a><span data-ttu-id="c8182-116">Omówienie</span><span class="sxs-lookup"><span data-stu-id="c8182-116">Overview</span></span>

<a id="Objectives"></a>
### <a name="objectives"></a><span data-ttu-id="c8182-117">Cele</span><span class="sxs-lookup"><span data-stu-id="c8182-117">Objectives</span></span>

<span data-ttu-id="c8182-118">W tym praktyczne laboratorium dowiesz się jak:</span><span class="sxs-lookup"><span data-stu-id="c8182-118">In this hands-on lab, you will learn how to:</span></span>

- <span data-ttu-id="c8182-119">Korzystać z nowych funkcji edytora HTML objęte Web Essentials, takich jak rozbudowanych wstawek kodu HTML5 i Zen kodowania</span><span class="sxs-lookup"><span data-stu-id="c8182-119">Use new HTML editor features included in Web Essentials such as rich HTML5 code snippets and Zen coding</span></span>
- <span data-ttu-id="c8182-120">Nowe funkcje edytora CSS uwzględnione w Web Essentials, takich jak selektor kolorów i etykietkę narzędzia macierzy przeglądarki</span><span class="sxs-lookup"><span data-stu-id="c8182-120">Use new CSS editor features included in Web Essentials such as the Color picker and Browser matrix tooltip</span></span>
- <span data-ttu-id="c8182-121">Korzystać z nowych funkcji edytora JavaScript zawartych w Web Essentials, takich jak Wyodrębnianie pliku, a funkcja IntelliSense dla wszystkich elementów HTML</span><span class="sxs-lookup"><span data-stu-id="c8182-121">Use new JavaScript editor features included in Web Essentials such as Extract to File and IntelliSense for all HTML elements</span></span>
- <span data-ttu-id="c8182-122">Dane programu Exchange między przeglądarką i Visual Studio za pomocą łączność z przeglądarkami</span><span class="sxs-lookup"><span data-stu-id="c8182-122">Exchange data between your browser and Visual Studio using Browser Link</span></span>

<a id="Prerequisites"></a>
### <a name="prerequisites"></a><span data-ttu-id="c8182-123">Wymagania wstępne</span><span class="sxs-lookup"><span data-stu-id="c8182-123">Prerequisites</span></span>

<span data-ttu-id="c8182-124">Do ukończenia tego laboratorium praktycznego niezbędne jest, następujące elementy:</span><span class="sxs-lookup"><span data-stu-id="c8182-124">The following is required to complete this hands-on lab:</span></span>

- <span data-ttu-id="c8182-125">[Microsoft Visual Studio Professional 2013](https://www.microsoft.com/visualstudio/) lub nowszej</span><span class="sxs-lookup"><span data-stu-id="c8182-125">[Microsoft Visual Studio Professional 2013](https://www.microsoft.com/visualstudio/) or greater</span></span>
- [<span data-ttu-id="c8182-126">Web Essentials 2013</span><span class="sxs-lookup"><span data-stu-id="c8182-126">Web Essentials 2013</span></span>](http://vswebessentials.com/)
- [<span data-ttu-id="c8182-127">Google Chrome</span><span class="sxs-lookup"><span data-stu-id="c8182-127">Google Chrome</span></span>](https://www.google.com/chrome/)

<a id="Setup"></a>
### <a name="setup"></a><span data-ttu-id="c8182-128">Konfiguracja</span><span class="sxs-lookup"><span data-stu-id="c8182-128">Setup</span></span>

<span data-ttu-id="c8182-129">Aby można było uruchomić ćwiczeń opisanych w tym praktyczne laboratorium, należy najpierw skonfigurować swoje środowisko.</span><span class="sxs-lookup"><span data-stu-id="c8182-129">In order to run the exercises in this hands-on lab, you will need to set up your environment first.</span></span>

1. <span data-ttu-id="c8182-130">Otwórz okno Eksploratora Windows i przejdź do laboratorium **źródła** folderu.</span><span class="sxs-lookup"><span data-stu-id="c8182-130">Open a Windows Explorer window and browse to the lab's **Source** folder.</span></span>
2. <span data-ttu-id="c8182-131">Kliknij prawym przyciskiem myszy **plik Setup.cmd** i wybierz **Uruchom jako administrator** do uruchamiania procesu instalacji, który będzie skonfigurować środowisko i zainstalować fragmenty kodu programu Visual Studio, w tym środowisku laboratoryjnym.</span><span class="sxs-lookup"><span data-stu-id="c8182-131">Right-click **Setup.cmd** and select **Run as administrator** to launch the setup process that will configure your environment and install the Visual Studio code snippets for this lab.</span></span>
3. <span data-ttu-id="c8182-132">Jeśli zostanie wyświetlone okno dialogowe kontroli konta użytkownika, upewnij się, działania, aby kontynuować.</span><span class="sxs-lookup"><span data-stu-id="c8182-132">If the User Account Control dialog box is shown, confirm the action to proceed.</span></span>

> [!NOTE]
> <span data-ttu-id="c8182-133">Upewnij się, że wszystkie zależności w tym środowisku laboratoryjnym sprawdzeniu przed uruchomieniem Instalatora.</span><span class="sxs-lookup"><span data-stu-id="c8182-133">Make sure you have checked all the dependencies for this lab before running the setup.</span></span>


<a id="CodeSnippets"></a>
### <a name="using-the-code-snippets"></a><span data-ttu-id="c8182-134">Za pomocą fragmentów kodu</span><span class="sxs-lookup"><span data-stu-id="c8182-134">Using the Code Snippets</span></span>

<span data-ttu-id="c8182-135">W dokumencie laboratorium należy poinstruować można wstawiać bloki kodu.</span><span class="sxs-lookup"><span data-stu-id="c8182-135">Throughout the lab document, you will be instructed to insert code blocks.</span></span> <span data-ttu-id="c8182-136">Dla wygody większość ten kod jest dostarczany jako Visual Studio fragmenty kodu, które są dostępne w Visual Studio 2013, aby uniknąć konieczności Dodaj ją ręcznie.</span><span class="sxs-lookup"><span data-stu-id="c8182-136">For your convenience, most of this code is provided as Visual Studio Code Snippets, which you can access from within Visual Studio 2013 to avoid having to add it manually.</span></span>

> [!NOTE]
> <span data-ttu-id="c8182-137">Każdy wykonywania towarzyszy początkowy rozwiązanie znajduje się w **rozpocząć** folderu ćwiczeniu, która umożliwia wykonanie każdego wykonywania niezależnie od innych.</span><span class="sxs-lookup"><span data-stu-id="c8182-137">Each exercise is accompanied by a starting solution located in the **Begin** folder of the exercise that allows you to follow each exercise independently of the others.</span></span> <span data-ttu-id="c8182-138">Należy pamiętać, że fragmenty kodu, które są dodawane podczas wykonywania brakuje te uruchamianie rozwiązań i może nie działać, dopóki nie zakończysz wykonywania.</span><span class="sxs-lookup"><span data-stu-id="c8182-138">Please be aware that the code snippets that are added during an exercise are missing from these starting solutions and may not work until you have completed the exercise.</span></span> <span data-ttu-id="c8182-139">Wewnątrz kodu źródłowego dla ćwiczenia, można również znaleźć **zakończenia** folderu zawierającego rozwiązania programu Visual Studio z kodem, który powstały na skutek wykonaniu kroków w odpowiedniej wykonywania.</span><span class="sxs-lookup"><span data-stu-id="c8182-139">Inside the source code for an exercise, you will also find an **End** folder containing a Visual Studio solution with the code that results from completing the steps in the corresponding exercise.</span></span> <span data-ttu-id="c8182-140">Jeśli potrzebujesz dodatkowej pomocy, gdy pracujesz za pośrednictwem tego laboratorium praktycznego, można użyć jako wskazówki dotyczące tych rozwiązań.</span><span class="sxs-lookup"><span data-stu-id="c8182-140">You can use these solutions as guidance if you need additional help as you work through this hands-on lab.</span></span>


* * *

<a id="Exercises"></a>
## <a name="exercises"></a><span data-ttu-id="c8182-141">Ćwiczenia</span><span class="sxs-lookup"><span data-stu-id="c8182-141">Exercises</span></span>

<span data-ttu-id="c8182-142">To ćwiczenie praktyczne obejmuje następujących czynnościach:</span><span class="sxs-lookup"><span data-stu-id="c8182-142">This hands-on lab includes the following exercises:</span></span>

1. [<span data-ttu-id="c8182-143">Praca z przeglądarkami i Web Essentials</span><span class="sxs-lookup"><span data-stu-id="c8182-143">Working with Browser Link and Web Essentials</span></span>](#Exercise1)
2. [<span data-ttu-id="c8182-144">Korzystając z zalet IntelliSense i fragmentów kodu</span><span class="sxs-lookup"><span data-stu-id="c8182-144">Taking Advantage of Code Snippets and IntelliSense</span></span>](#Exercise2)

> [!NOTE]
> <span data-ttu-id="c8182-145">Przy pierwszym uruchomieniu programu Visual Studio, należy wybrać jedną z kolekcji wstępnie zdefiniowanych ustawień.</span><span class="sxs-lookup"><span data-stu-id="c8182-145">When you first start Visual Studio, you must select one of the predefined settings collections.</span></span> <span data-ttu-id="c8182-146">Każda kolekcja wstępnie zdefiniowanych służy do dopasowywania style rozwoju i określa układy okna, zachowanie edytora, fragmenty kodu IntelliSense i opcje w oknach dialogowych.</span><span class="sxs-lookup"><span data-stu-id="c8182-146">Each predefined collection is designed to match a particular development style and determines window layouts, editor behavior, IntelliSense code snippets, and dialog box options.</span></span> <span data-ttu-id="c8182-147">Procedury przedstawione w tym środowisku laboratoryjnym opisano czynności niezbędnych do wykonywania danego zadania w programie Visual Studio, korzystając z **ogólnych ustawieniach projektowych** kolekcji.</span><span class="sxs-lookup"><span data-stu-id="c8182-147">The procedures in this lab describe the actions necessary to accomplish a given task in Visual Studio when using the **General Development Settings** collection.</span></span> <span data-ttu-id="c8182-148">Jeśli wybierzesz kolekcji różne ustawienia dla swojego środowiska programowania, może być różnice w krokach, które należy wziąć pod uwagę.</span><span class="sxs-lookup"><span data-stu-id="c8182-148">If you choose a different settings collection for your development environment, there may be differences in the steps that you should take into account.</span></span>


<a id="Exercise1"></a>
### <a name="exercise-1-working-with-browser-link-and-web-essentials"></a><span data-ttu-id="c8182-149">Ćwiczenie 1: Praca z przeglądarkami i Web Essentials</span><span class="sxs-lookup"><span data-stu-id="c8182-149">Exercise 1: Working with Browser Link and Web Essentials</span></span>

<span data-ttu-id="c8182-150">**Web Essentials** to rozszerzenie programu Visual Studio, który dodaje różne funkcje przydatne do tworzenia nowoczesnej sieci web, koncentrując się głównie na środowisko programistyczne w sieci web, które znacznie szybszy i bardziej przyjemny.</span><span class="sxs-lookup"><span data-stu-id="c8182-150">**Web Essentials** is a Visual Studio extension that adds a variety of useful features for modern web development, mostly focused on making the web development experience much faster and more pleasant.</span></span> <span data-ttu-id="c8182-151">Web Essentials można zainstalować z galerii rozszerzeń w programie Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="c8182-151">You can install Web Essentials from the Extension Gallery in Visual Studio.</span></span>

<span data-ttu-id="c8182-152">**Łączność z przeglądarkami** to nowa funkcja zawarte w Visual Studio 2013, która udostępnia kanał między środowiska IDE programu Visual Studio oraz otwartą przeglądarką wymiany danych między aplikacją sieci web i programu Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="c8182-152">**Browser Link** is a new feature included in Visual Studio 2013 that provides a channel between the Visual Studio IDE and any open browser to exchange data between your web application and Visual Studio.</span></span> <span data-ttu-id="c8182-153">Rozszerzenia Web Essentials wzbogacają łączność z przeglądarkami za pomocą narzędzi do manipulowania model obiektu modelu DOM i style CSS stron sieci web bezpośrednio z przeglądarki.</span><span class="sxs-lookup"><span data-stu-id="c8182-153">Web Essentials extends Browser Link with tools to manipulate the DOM object model and the CSS styles of your web pages directly from the browser.</span></span>

<span data-ttu-id="c8182-154">W tym ćwiczeniu zostanie Poznaj niektóre z funkcji obsługiwanych przez **Web Essentials** i **łączność z przeglądarkami** Aby ulepszyć stronę prosty test.</span><span class="sxs-lookup"><span data-stu-id="c8182-154">In this exercise, you will explore some of the features supported by **Web Essentials** and **Browser Link** to enhance a simple quiz page.</span></span>

<a id="Ex1Task1"></a>
#### <a name="task-1---running-the-project-in-multiple-browsers"></a><span data-ttu-id="c8182-155">Zadanie 1 — Uruchamianie projektu w różnych przeglądarkach</span><span class="sxs-lookup"><span data-stu-id="c8182-155">Task 1 - Running the Project in Multiple Browsers</span></span>

<span data-ttu-id="c8182-156">W tym zadaniu skonfigurujesz aplikację sieci web do uruchamiania w wielu przeglądarkach jednocześnie, która jest przydatna przy testowaniu przeglądarek.</span><span class="sxs-lookup"><span data-stu-id="c8182-156">In this task, you will configure your web application to run in multiple browsers at once, which is useful for cross-browser testing.</span></span>

1. <span data-ttu-id="c8182-157">Otwórz **programu Microsoft Visual Studio**.</span><span class="sxs-lookup"><span data-stu-id="c8182-157">Open **Microsoft Visual Studio**.</span></span>
2. <span data-ttu-id="c8182-158">W **pliku** menu, wybierz opcję **Open | Projekt/rozwiązanie...**  i przejdź do **Ex1 WorkingwithBrowserLinkandWebEssentials\Begin** w **źródła** folderu laboratorium (C:\WebCampsTK\HOL\VSWebTooling\Source).</span><span class="sxs-lookup"><span data-stu-id="c8182-158">In the **File** menu, select **Open | Project/Solution...** and browse to **Ex1-WorkingwithBrowserLinkandWebEssentials\Begin** in the **Source** folder of the lab (C:\WebCampsTK\HOL\VSWebTooling\Source).</span></span> <span data-ttu-id="c8182-159">Wybierz **Begin.sln** i kliknij przycisk **Otwórz**.</span><span class="sxs-lookup"><span data-stu-id="c8182-159">Select **Begin.sln** and click **Open**.</span></span>
3. <span data-ttu-id="c8182-160">Na pasku narzędzi programu Visual Studio rozwiń menu przeglądarki, a następnie wybierz **przeglądanie za pomocą...** .</span><span class="sxs-lookup"><span data-stu-id="c8182-160">In the Visual Studio toolbar, expand the browser menu and select **Browse With...**.</span></span>

    <span data-ttu-id="c8182-161">![Przeglądanie za pomocą opcji menu](visual-studio-2013-web-tools/_static/image1.png "Przeglądaj za pomocą menu przeglądarki")</span><span class="sxs-lookup"><span data-stu-id="c8182-161">![Browse With menu option](visual-studio-2013-web-tools/_static/image1.png "Browse with... in browser menu")</span></span>

    <span data-ttu-id="c8182-162">*Przeglądanie za pomocą opcji menu*</span><span class="sxs-lookup"><span data-stu-id="c8182-162">*Browse With menu option*</span></span>
4. <span data-ttu-id="c8182-163">W **przeglądanie za pomocą** okno dialogowe, wybierz **Google Chrome** i **programu Internet Explorer** , przytrzymując **CTRL** klucza, a następnie kliknij przycisk  **Ustaw jako domyślny**.</span><span class="sxs-lookup"><span data-stu-id="c8182-163">In the **Browse With** dialog box, select both **Google Chrome** and **Internet Explorer** by holding down the **CTRL** key and click **Set as Default**.</span></span>

    <span data-ttu-id="c8182-164">![Przeglądaj okna dialogowego](visual-studio-2013-web-tools/_static/image2.png "Przeglądaj okna dialogowego")</span><span class="sxs-lookup"><span data-stu-id="c8182-164">![Browse with dialog box](visual-studio-2013-web-tools/_static/image2.png "Browse with dialog box")</span></span>

    <span data-ttu-id="c8182-165">*Wybieranie wielu domyślnych przeglądarek*</span><span class="sxs-lookup"><span data-stu-id="c8182-165">*Selecting multiple default browsers*</span></span>
5. <span data-ttu-id="c8182-166">Program Internet Explorer i Google Chrome teraz powinny się wyświetlać jako domyślną przeglądarką.</span><span class="sxs-lookup"><span data-stu-id="c8182-166">Both Google Chrome and Internet Explorer should now appear as the default browsers.</span></span> <span data-ttu-id="c8182-167">Kliknij przycisk **anulować** aby zamknąć okno dialogowe.</span><span class="sxs-lookup"><span data-stu-id="c8182-167">Click **Cancel** to close the dialog box.</span></span>

    <span data-ttu-id="c8182-168">![Google Chrome i przeglądarki Internet Explorer jako domyślnej przeglądarki](visual-studio-2013-web-tools/_static/image3.png "domyślnych przeglądarek Google Chrome i przeglądarki Internet Explorer")</span><span class="sxs-lookup"><span data-stu-id="c8182-168">![Google Chrome and Internet Explorer as default browsers](visual-studio-2013-web-tools/_static/image3.png "Google Chrome and Internet Explorer default browsers")</span></span>

    <span data-ttu-id="c8182-169">*Google Chrome i przeglądarki Internet Explorer jako domyślnej przeglądarki*</span><span class="sxs-lookup"><span data-stu-id="c8182-169">*Google Chrome and Internet Explorer as default browsers*</span></span>

    > [!NOTE]
    > <span data-ttu-id="c8182-170">Po skonfigurowaniu domyślną przeglądarką **wiele przeglądarek** wybrano opcję w menu przeglądarki.</span><span class="sxs-lookup"><span data-stu-id="c8182-170">After configuring the default browsers, the **Multiple Browsers** option is selected in the browser menu.</span></span>
    > 
    > <span data-ttu-id="c8182-171">![Wiele przeglądarek](visual-studio-2013-web-tools/_static/image4.png "wiele przeglądarek")</span><span class="sxs-lookup"><span data-stu-id="c8182-171">![Multiple browsers](visual-studio-2013-web-tools/_static/image4.png "Multiple browsers")</span></span>
6. <span data-ttu-id="c8182-172">Naciśnij klawisz **CTRL** + **F5** Aby uruchomić aplikację bez debugowania.</span><span class="sxs-lookup"><span data-stu-id="c8182-172">Press **CTRL** + **F5** to run the application without debugging.</span></span>
7. <span data-ttu-id="c8182-173">Po otwarciu oba okna przeglądarki, umieść jeden z nich nad drugim umożliwiający zobaczenie aktualizacje w przeglądarkach dla obu jednocześnie.</span><span class="sxs-lookup"><span data-stu-id="c8182-173">When both browser windows open, place one of them above the other in order to see the updates on both browsers simultaneously.</span></span> <span data-ttu-id="c8182-174">Przeglądarki powinien być wyświetlany na stronie sieci web z obrazie, gdzie jasnoniebieski prostokąt.</span><span class="sxs-lookup"><span data-stu-id="c8182-174">The browsers should display a web page with a light-blue rectangle.</span></span>

    <span data-ttu-id="c8182-175">![Wprowadzenie do przeglądarki jeden nad drugim](visual-studio-2013-web-tools/_static/image5.png "umieszczenie przeglądarkę jeden nad drugim")</span><span class="sxs-lookup"><span data-stu-id="c8182-175">![Placing one browser above the other](visual-studio-2013-web-tools/_static/image5.png "Placing one browser above the other")</span></span>

    <span data-ttu-id="c8182-176">*Wprowadzenie do przeglądarki jeden nad drugim*</span><span class="sxs-lookup"><span data-stu-id="c8182-176">*Placing one browser above the other*</span></span>
8. <span data-ttu-id="c8182-177">Nie zamykaj przeglądarki.</span><span class="sxs-lookup"><span data-stu-id="c8182-177">Do not close the browsers.</span></span> <span data-ttu-id="c8182-178">Użyjesz ich w ramach następnego zadania.</span><span class="sxs-lookup"><span data-stu-id="c8182-178">You will use them in the next task.</span></span>

<a id="Ex1Task2"></a>
#### <a name="task-2---using-zen-coding-to-create-html-elements"></a><span data-ttu-id="c8182-179">Zadanie 2 — przy użyciu Zen programowania do tworzenia elementów HTML</span><span class="sxs-lookup"><span data-stu-id="c8182-179">Task 2 - Using Zen Coding to Create HTML Elements</span></span>

<span data-ttu-id="c8182-180">**Kodowanie Zen** jest edytorem kodowania wtyczki dla szybkich HTML, XML, XSL (lub innych formatów kodu ze strukturą) i edytowania.</span><span class="sxs-lookup"><span data-stu-id="c8182-180">**Zen Coding** is an editor plugin for high-speed HTML, XML, XSL (or any other structured code format) coding and editing.</span></span> <span data-ttu-id="c8182-181">Podstawowe Ta wtyczka jest aparat zaawansowane skrót, który umożliwia rozwinięcie wyrażeń — podobnie jak selektorów CSS - kod HTML.</span><span class="sxs-lookup"><span data-stu-id="c8182-181">The core of this plugin is a powerful abbreviation engine which allows you to expand expressions -similar to CSS selectors- into HTML code.</span></span> <span data-ttu-id="c8182-182">Kodowanie Zen jest możliwość szybkiego zapisu HTML za pomocą CSS stylu selektor składni.</span><span class="sxs-lookup"><span data-stu-id="c8182-182">Zen Coding is a fast way to write HTML using a CSS style selector syntax.</span></span>

<span data-ttu-id="c8182-183">W tym ćwiczeniu użyje funkcji Zen kodowania, dostarczone przez Web Essentials do generowania przyciski HTML, które reprezentują opcje pytania.</span><span class="sxs-lookup"><span data-stu-id="c8182-183">In this exercise, you will use the Zen Coding feature provided by Web Essentials to generate the HTML buttons that represent the options of the question.</span></span>

1. <span data-ttu-id="c8182-184">Przełącz się do programu Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="c8182-184">Switch back to Visual Studio.</span></span>
2. <span data-ttu-id="c8182-185">Otwórz **Index.cshtml** plik znajdujący się w **widoków** | **Home** folderu.</span><span class="sxs-lookup"><span data-stu-id="c8182-185">Open the **Index.cshtml** file located in the **Views** | **Home** folder.</span></span>
3. <span data-ttu-id="c8182-186">Zastąp **&lt;!--TODO: Dodaj tutaj — opcje&gt;** komentarz z poniższej kod i naciśnij klawisz **kartę**.</span><span class="sxs-lookup"><span data-stu-id="c8182-186">Replace the **&lt;!-- TODO: add options here--&gt;** comment with the following code, and press **TAB**.</span></span>

    [!code-css[Main](visual-studio-2013-web-tools/samples/sample1.css)]
4. <span data-ttu-id="c8182-187">Kod powinien rozwinięte w formacie HTML.</span><span class="sxs-lookup"><span data-stu-id="c8182-187">The code should be expanded to HTML.</span></span>

    <span data-ttu-id="c8182-188">![Rozwinięte HTML](visual-studio-2013-web-tools/_static/image6.png "rozwinięte HTML")</span><span class="sxs-lookup"><span data-stu-id="c8182-188">![Expanded HTML](visual-studio-2013-web-tools/_static/image6.png "Expanded HTML")</span></span>

    <span data-ttu-id="c8182-189">*Rozwinięty HTML*</span><span class="sxs-lookup"><span data-stu-id="c8182-189">*Expanded HTML*</span></span>

    > [!NOTE]
    > <span data-ttu-id="c8182-190">Aby dowiedzieć się więcej na temat kodowania Zen składni, zapoznaj się z poniższymi [artykułu](http://www.johnpapa.net/zen-coding-in-visual-studio-2012/).</span><span class="sxs-lookup"><span data-stu-id="c8182-190">To learn more about Zen Coding syntax, see the following [article](http://www.johnpapa.net/zen-coding-in-visual-studio-2012/).</span></span>
5. <span data-ttu-id="c8182-191">Kliknij przycisk **Odśwież połączone przeglądarki** przycisk, aby zaktualizować obie przeglądarki.</span><span class="sxs-lookup"><span data-stu-id="c8182-191">Click the **Refresh linked browsers** button to update both browsers.</span></span>

    <span data-ttu-id="c8182-192">![Odśwież połączone przeglądarki](visual-studio-2013-web-tools/_static/image7.png "Odśwież połączone przeglądarki")</span><span class="sxs-lookup"><span data-stu-id="c8182-192">![Refresh linked browsers](visual-studio-2013-web-tools/_static/image7.png "Refresh linked browsers")</span></span>

    <span data-ttu-id="c8182-193">*Odśwież połączone przeglądarki*</span><span class="sxs-lookup"><span data-stu-id="c8182-193">*Refresh linked browsers*</span></span>

    <span data-ttu-id="c8182-194">![Internet Explorer — strona aktualizowana z czterema przyciskami](visual-studio-2013-web-tools/_static/image8.png "programu Internet Explorer — strona aktualizowana z czterema przyciskami")</span><span class="sxs-lookup"><span data-stu-id="c8182-194">![Internet Explorer - Page updated with four buttons](visual-studio-2013-web-tools/_static/image8.png "Internet Explorer - Page updated with four buttons")</span></span>

    <span data-ttu-id="c8182-195">*Internet Explorer — strona aktualizowana z czterema przyciskami*</span><span class="sxs-lookup"><span data-stu-id="c8182-195">*Internet Explorer - Page updated with four buttons*</span></span>

    <span data-ttu-id="c8182-196">![Google Chrome — strona aktualizowana z czterema przyciskami](visual-studio-2013-web-tools/_static/image9.png "Google Chrome — strona aktualizowana z czterema przyciskami")</span><span class="sxs-lookup"><span data-stu-id="c8182-196">![Google Chrome - Page updated with four buttons](visual-studio-2013-web-tools/_static/image9.png "Google Chrome - Page updated with four buttons")</span></span>

    <span data-ttu-id="c8182-197">*Google Chrome — strona aktualizowana z czterema przyciskami*</span><span class="sxs-lookup"><span data-stu-id="c8182-197">*Google Chrome - Page updated with four buttons*</span></span>
6. <span data-ttu-id="c8182-198">Przełącz się do programu Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="c8182-198">Switch back to Visual Studio.</span></span>
7. <span data-ttu-id="c8182-199">Przyciski zostały dodane do strony, ale nadal musisz dodać pytanie szablonu.</span><span class="sxs-lookup"><span data-stu-id="c8182-199">You have added the buttons to the page, but you still need to add a template question.</span></span> <span data-ttu-id="c8182-200">Aby to zrobić, użyjesz nową funkcją w sieci Web Essentials o nazwie **generator Lorem Ipsum**.</span><span class="sxs-lookup"><span data-stu-id="c8182-200">To do so, you will use a new feature in Web Essentials called **Lorem Ipsum generator**.</span></span> <span data-ttu-id="c8182-201">Znajdź **div** element z **klasy** atrybut **front**.</span><span class="sxs-lookup"><span data-stu-id="c8182-201">Locate the **div** element with the **class** attribute **front**.</span></span>
8. <span data-ttu-id="c8182-202">Dodaj następujący kod jako pierwszy element podrzędny elementu **div**i naciśnij klawisz **kartę**.</span><span class="sxs-lookup"><span data-stu-id="c8182-202">Add the following code as the first child element of the **div**, and press **TAB**.</span></span>

    [!code-css[Main](visual-studio-2013-web-tools/samples/sample2.css)]
9. <span data-ttu-id="c8182-203">Kod powinien rozwinięte w formacie HTML.</span><span class="sxs-lookup"><span data-stu-id="c8182-203">The code should be expanded to HTML.</span></span>

    <span data-ttu-id="c8182-204">![Lorem Ipsum wygenerowany automatycznie](visual-studio-2013-web-tools/_static/image10.png "Lorem Ipsum wygenerowany automatycznie")</span><span class="sxs-lookup"><span data-stu-id="c8182-204">![Lorem Ipsum autogenerated](visual-studio-2013-web-tools/_static/image10.png "Lorem Ipsum autogenerated")</span></span>

    <span data-ttu-id="c8182-205">*Lorem Ipsum wygenerowany automatycznie*</span><span class="sxs-lookup"><span data-stu-id="c8182-205">*Lorem Ipsum autogenerated*</span></span>

    > [!NOTE]
    > <span data-ttu-id="c8182-206">W ramach Zen kodowania można teraz wygenerować Lorem Ipsum kodu bezpośrednio w edytorze HTML.</span><span class="sxs-lookup"><span data-stu-id="c8182-206">As part of Zen Coding, you can now generate Lorem Ipsum code directly in the HTML editor.</span></span> <span data-ttu-id="c8182-207">Po prostu wpisz **lorem** i kliknij przycisk **kartę** i 30 word Lorem Ipsum, tekst zostanie wstawiony.</span><span class="sxs-lookup"><span data-stu-id="c8182-207">Simply type **lorem** and hit **TAB** and a 30 word Lorem Ipsum text will be inserted.</span></span> <span data-ttu-id="c8182-208">Np.</span><span class="sxs-lookup"><span data-stu-id="c8182-208">E.g.</span></span> <span data-ttu-id="c8182-209">*lorem10* wstawia 10 słów Lorem Ipsum.</span><span class="sxs-lookup"><span data-stu-id="c8182-209">*lorem10* inserts 10 Lorem Ipsum words.</span></span>
10. <span data-ttu-id="c8182-210">W górnej części zapytania dodasz logo przy użyciu kolejną nową funkcją w sieci Web Essentials o nazwie **generator pikseli Lorem**.</span><span class="sxs-lookup"><span data-stu-id="c8182-210">You will add a logo at the top of the question by using another new feature in Web Essentials called **Lorem Pixel generator**.</span></span> <span data-ttu-id="c8182-211">Dodaj następujący kod jako pierwszy element podrzędny elementu **div** element z **kontenera** jako **klasy** wartości, a następnie naciśnij klawisz **kartę**.</span><span class="sxs-lookup"><span data-stu-id="c8182-211">Add the following code as the first child element of the **div** element with **container** as **class** value, and press **TAB**.</span></span>

    [!code-css[Main](visual-studio-2013-web-tools/samples/sample3.css)]
11. <span data-ttu-id="c8182-212">Kod powinni rozwinąć pozycję w formacie HTML.</span><span class="sxs-lookup"><span data-stu-id="c8182-212">The code should expand to HTML.</span></span>

    <span data-ttu-id="c8182-213">![Wygenerowany automatycznie pikseli Lorem](visual-studio-2013-web-tools/_static/image11.png "wygenerowany automatycznie Lorem pikseli")</span><span class="sxs-lookup"><span data-stu-id="c8182-213">![Lorem Pixel autogenerated](visual-studio-2013-web-tools/_static/image11.png "Lorem Pixel autogenerated")</span></span>

    <span data-ttu-id="c8182-214">*Wygenerowany automatycznie Lorem pikseli*</span><span class="sxs-lookup"><span data-stu-id="c8182-214">*Lorem Pixel autogenerated*</span></span>

    > [!NOTE]
    > <span data-ttu-id="c8182-215">W ramach Zen kodowania można również wygenerować pikseli Lorem kodu bezpośrednio w edytorze HTML.</span><span class="sxs-lookup"><span data-stu-id="c8182-215">As part of Zen Coding, you can also generate Lorem Pixel code directly in the HTML editor.</span></span> <span data-ttu-id="c8182-216">Po prostu wpisz **programu pix-200 x 200-zwierzęta** i kliknij przycisk **kartę** i **img** tag z obrazem 200 x 200 zwierzęcia zostanie wstawiony.</span><span class="sxs-lookup"><span data-stu-id="c8182-216">Simply type **pix-200x200-animals** and hit **TAB** and an **img** tag with a 200x200 image of an animal will be inserted.</span></span> <span data-ttu-id="c8182-217">Aby uzyskać więcej informacji, zobacz [pikseli Lorem](http://www.lorempixel.com).</span><span class="sxs-lookup"><span data-stu-id="c8182-217">For more information, refer to [Lorem Pixel](http://www.lorempixel.com).</span></span>
12. <span data-ttu-id="c8182-218">Kliknij przycisk **Odśwież połączone przeglądarki** przycisk, aby zaktualizować obie przeglądarki.</span><span class="sxs-lookup"><span data-stu-id="c8182-218">Click the **Refresh linked browsers** button to update both browsers.</span></span>

    <span data-ttu-id="c8182-219">![Internet Explorer — automatycznie wygenerowany obraz i tekst](visual-studio-2013-web-tools/_static/image12.png "programu Internet Explorer — automatycznie wygenerowany obraz i tekst")</span><span class="sxs-lookup"><span data-stu-id="c8182-219">![Internet Explorer - Autogenerated image and text](visual-studio-2013-web-tools/_static/image12.png "Internet Explorer - Autogenerated image and text")</span></span>

    <span data-ttu-id="c8182-220">*Internet Explorer — automatycznie wygenerowany obraz i tekst*</span><span class="sxs-lookup"><span data-stu-id="c8182-220">*Internet Explorer - Autogenerated image and text*</span></span>

    <span data-ttu-id="c8182-221">![Google Chrome — automatycznie wygenerowany obraz i tekst](visual-studio-2013-web-tools/_static/image13.png "Google Chrome — automatycznie wygenerowany obraz i tekst")</span><span class="sxs-lookup"><span data-stu-id="c8182-221">![Google Chrome - Autogenerated image and text](visual-studio-2013-web-tools/_static/image13.png "Google Chrome - Autogenerated image and text")</span></span>

    <span data-ttu-id="c8182-222">*Google Chrome — automatycznie wygenerowany obraz i tekst*</span><span class="sxs-lookup"><span data-stu-id="c8182-222">*Google Chrome - Autogenerated image and text*</span></span>

    > [!NOTE]
    > <span data-ttu-id="c8182-223">Ponieważ obraz jest wybranych losowo podczas dodawania fragment kodu, obraz wyświetlany w przeglądarkach mogą się różnić.</span><span class="sxs-lookup"><span data-stu-id="c8182-223">Because the image is selected randomly when adding the code snippet, the image shown in the browsers may differ.</span></span>
13. <span data-ttu-id="c8182-224">Nie zamykaj przeglądarki.</span><span class="sxs-lookup"><span data-stu-id="c8182-224">Do not close the browsers.</span></span> <span data-ttu-id="c8182-225">Użyjesz ich w ramach następnego zadania.</span><span class="sxs-lookup"><span data-stu-id="c8182-225">You will use them in the next task.</span></span>

<a id="Ex1Task3"></a>
#### <a name="task-3---updating-a-style-property"></a><span data-ttu-id="c8182-226">Zadanie 3 — aktualizowanie właściwości stylu</span><span class="sxs-lookup"><span data-stu-id="c8182-226">Task 3 - Updating a Style Property</span></span>

<span data-ttu-id="c8182-227">W tym zadaniu użyje Browser Link **sprawdzić tryb** funkcję, aby wykryć dokładną lokalizację, gdzie jest generowany określonego elementu DOM, a następnie zaktualizuj właściwość kolor elementu przy użyciu selektora kolorów, dostarczone przez sieci Web Podstawy.</span><span class="sxs-lookup"><span data-stu-id="c8182-227">In this task, you will use the Browser Link's **Inspect Mode** feature to detect the exact location where the specific DOM element is generated and then update the color property of that element using a color picker provided by Web Essentials.</span></span>

1. <span data-ttu-id="c8182-228">W przeglądarce Internet Explorer, naciśnij klawisz **CTRL** + **ALT** + **I** Aby włączyć tryb inspekcji.</span><span class="sxs-lookup"><span data-stu-id="c8182-228">In the Internet Explorer browser, press **CTRL** + **ALT** + **I** to enable Inspect Mode.</span></span>
2. <span data-ttu-id="c8182-229">Przesuń wskaźnik nad światła niebieskie obramowanie, a następnie kliknij przycisk.</span><span class="sxs-lookup"><span data-stu-id="c8182-229">Move the pointer over the light blue border and click.</span></span>

    <span data-ttu-id="c8182-230">![Przesunięcie kursora nad światła niebieskie obramowanie](visual-studio-2013-web-tools/_static/image14.png "przesunięcie wskaźnika nad światła niebieskie obramowanie")</span><span class="sxs-lookup"><span data-stu-id="c8182-230">![Moving the pointer over the light blue border](visual-studio-2013-web-tools/_static/image14.png "Moving the pointer over the light blue border")</span></span>

    <span data-ttu-id="c8182-231">*Przesunięcie kursora nad światła niebieskie obramowanie*</span><span class="sxs-lookup"><span data-stu-id="c8182-231">*Moving the pointer over the light blue border*</span></span>
3. <span data-ttu-id="c8182-232">Przełącz się do programu Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="c8182-232">Switch back to Visual Studio.</span></span> <span data-ttu-id="c8182-233">Zwróć uwagę, jak element HTML, który został wybrany w przeglądarce zostanie również wybrane w edytorze programu Visual Studio HTML.</span><span class="sxs-lookup"><span data-stu-id="c8182-233">Notice how the HTML element that you selected in the browser is also selected in the Visual Studio HTML editor.</span></span>

    <span data-ttu-id="c8182-234">![Element HTML zaznaczonego w edytorze programu Visual Studio HTML](visual-studio-2013-web-tools/_static/image15.png "HTML elementu zaznaczonego w edytorze programu Visual Studio HTML")</span><span class="sxs-lookup"><span data-stu-id="c8182-234">![HTML element selected in the Visual Studio HTML editor](visual-studio-2013-web-tools/_static/image15.png "HTML element selected in the Visual Studio HTML editor")</span></span>

    <span data-ttu-id="c8182-235">*Element HTML zaznaczonego w edytorze programu Visual Studio HTML*</span><span class="sxs-lookup"><span data-stu-id="c8182-235">*HTML element selected in the Visual Studio HTML editor*</span></span>
4. <span data-ttu-id="c8182-236">Teraz zmodyfikujemy **front** klasy CSS, aby zmienić stylu wybranego elementu.</span><span class="sxs-lookup"><span data-stu-id="c8182-236">You will now update the **front** CSS class in order to change the styling of the selected element.</span></span> <span data-ttu-id="c8182-237">Aby to zrobić, naciśnij klawisz **CTRL** + **,** otworzyć **przejdź do** pola wyszukiwania.</span><span class="sxs-lookup"><span data-stu-id="c8182-237">To do so, press **CTRL** + **,** to open the **Navigate To** search box.</span></span> <span data-ttu-id="c8182-238">Typ **site.css** i naciśnij klawisz **ENTER** można otworzyć pliku.</span><span class="sxs-lookup"><span data-stu-id="c8182-238">Type **site.css** and press **ENTER** to open the file.</span></span>

    <span data-ttu-id="c8182-239">![Otwieranie pliku Site.css](visual-studio-2013-web-tools/_static/image16.png "otwierania pliku Site.css")</span><span class="sxs-lookup"><span data-stu-id="c8182-239">![Opening file Site.css](visual-studio-2013-web-tools/_static/image16.png "Opening file Site.css")</span></span>

    <span data-ttu-id="c8182-240">*Otwieranie pliku Site.css*</span><span class="sxs-lookup"><span data-stu-id="c8182-240">*Opening file Site.css*</span></span>
5. <span data-ttu-id="c8182-241">Naciśnij klawisz **CTRL** + **F** i typ **.front .flip container** można znaleźć selektora CSS.</span><span class="sxs-lookup"><span data-stu-id="c8182-241">Press **CTRL** + **F** and type **.flip-containter .front** to find the CSS selector.</span></span>
6. <span data-ttu-id="c8182-242">Kliknij przycisk światła niebieskim kwadratem we właściwości klasy, aby otworzyć selektor kolorów obramowania.</span><span class="sxs-lookup"><span data-stu-id="c8182-242">Click the light blue square in the border property of the class to open the Color Picker.</span></span>

    <span data-ttu-id="c8182-243">![Otwieranie selektor kolorów](visual-studio-2013-web-tools/_static/image17.png "otworzyć selektor kolorów")</span><span class="sxs-lookup"><span data-stu-id="c8182-243">![Opening the Color Picker](visual-studio-2013-web-tools/_static/image17.png "Opening the Color Picker")</span></span>

    <span data-ttu-id="c8182-244">*Otwieranie selektor kolorów*</span><span class="sxs-lookup"><span data-stu-id="c8182-244">*Opening the Color Picker*</span></span>
7. <span data-ttu-id="c8182-245">Rozwiń selektor kolorów, klikając przycisk z podwójną strzałką i wybierz nowy kolor.</span><span class="sxs-lookup"><span data-stu-id="c8182-245">Expand the Color Picker by clicking the chevron button and select a new color.</span></span>

    <span data-ttu-id="c8182-246">![Rozwijanie selektor kolorów](visual-studio-2013-web-tools/_static/image18.png "rozwijanie selektor kolorów")</span><span class="sxs-lookup"><span data-stu-id="c8182-246">![Expanding the Color Picker](visual-studio-2013-web-tools/_static/image18.png "Expanding the Color Picker")</span></span>

    <span data-ttu-id="c8182-247">*Rozwijanie selektor kolorów*</span><span class="sxs-lookup"><span data-stu-id="c8182-247">*Expanding the Color Picker*</span></span>
8. <span data-ttu-id="c8182-248">Naciśnij klawisz **CTRL** + **ALT** + **ENTER** odświeżania podłączonych przeglądarek.</span><span class="sxs-lookup"><span data-stu-id="c8182-248">Press **CTRL** + **ALT** + **ENTER** to refresh linked browsers.</span></span>
9. <span data-ttu-id="c8182-249">Przełącz się do programu Internet Explorer i zwróć uwagę, jak zmienił kolor obramowania.</span><span class="sxs-lookup"><span data-stu-id="c8182-249">Switch to Internet Explorer and notice how the color of the border has changed.</span></span>

    <span data-ttu-id="c8182-250">![Internet Explorer — kolor obramowania zaktualizowane](visual-studio-2013-web-tools/_static/image19.png "programu Internet Explorer — kolor obramowania zaktualizowane")</span><span class="sxs-lookup"><span data-stu-id="c8182-250">![Internet Explorer - Border color updated](visual-studio-2013-web-tools/_static/image19.png "Internet Explorer - Border color updated")</span></span>

    <span data-ttu-id="c8182-251">*Internet Explorer — kolor obramowania zaktualizowane*</span><span class="sxs-lookup"><span data-stu-id="c8182-251">*Internet Explorer - Border color updated*</span></span>
10. <span data-ttu-id="c8182-252">Przełącz się do przeglądarki Google Chrome i zwróć uwagę, jak zmienił kolor obramowania.</span><span class="sxs-lookup"><span data-stu-id="c8182-252">Switch to Google Chrome and notice how the color of the border has changed.</span></span>

    <span data-ttu-id="c8182-253">![Google Chrome — kolor obramowania zaktualizowane](visual-studio-2013-web-tools/_static/image20.png "Google Chrome — kolor obramowania zaktualizowane")</span><span class="sxs-lookup"><span data-stu-id="c8182-253">![Google Chrome - Border color updated](visual-studio-2013-web-tools/_static/image20.png "Google Chrome - Border color updated")</span></span>

    <span data-ttu-id="c8182-254">*Google Chrome — kolor obramowania zaktualizowane*</span><span class="sxs-lookup"><span data-stu-id="c8182-254">*Google Chrome - Border color updated*</span></span>
11. <span data-ttu-id="c8182-255">Przełącz się do programu Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="c8182-255">Switch back to Visual Studio.</span></span>
12. <span data-ttu-id="c8182-256">Przejdź na koniec **Site.css** pliku i naciśnij klawisz **CTRL** + **F** zlokalizować **.btn** selektora.</span><span class="sxs-lookup"><span data-stu-id="c8182-256">Go to the end of the **Site.css** file and press **CTRL** + **F** to locate the **.btn** selector.</span></span>
13. <span data-ttu-id="c8182-257">Należy zauważyć, że **- webkit-border-radius** właściwość jest podkreślone w kolorze zielonym.</span><span class="sxs-lookup"><span data-stu-id="c8182-257">Notice that the **-webkit-border-radius** property is underlined in green.</span></span>

    <span data-ttu-id="c8182-258">![Właściwość selektor btn - webkit-border-radius](visual-studio-2013-web-tools/_static/image21.png "właściwość selektor btn - webkit-border-radius")</span><span class="sxs-lookup"><span data-stu-id="c8182-258">![-webkit-border-radius property of the btn selector](visual-studio-2013-web-tools/_static/image21.png "-webkit-border-radius property of the btn selector")</span></span>

    <span data-ttu-id="c8182-259">*Właściwość selektor btn - webkit-border-radius*</span><span class="sxs-lookup"><span data-stu-id="c8182-259">*-webkit-border-radius property of the btn selector*</span></span>
14. <span data-ttu-id="c8182-260">Umieść punkt wstawiania w **- webkit-border-radius** właściwości.</span><span class="sxs-lookup"><span data-stu-id="c8182-260">Place the caret in the **-webkit-border-radius** property.</span></span> <span data-ttu-id="c8182-261">Niebieska linia powinna zostać wyświetlona w obszarze pierwszą literę wyrazu pierwszej właściwości.</span><span class="sxs-lookup"><span data-stu-id="c8182-261">A blue line should appear under the first letter of the first word of the property.</span></span> <span data-ttu-id="c8182-262">Jest to **tagów inteligentnych**.</span><span class="sxs-lookup"><span data-stu-id="c8182-262">This is the **smart tag**.</span></span>
15. <span data-ttu-id="c8182-263">Naciśnij klawisz **CTRL** + **.**</span><span class="sxs-lookup"><span data-stu-id="c8182-263">Press **CTRL** + **.**</span></span> <span data-ttu-id="c8182-264">Aby otworzyć menu sugestie, a następnie kliknij przycisk **Dodawanie właściwości standardowych (border-radius)**.</span><span class="sxs-lookup"><span data-stu-id="c8182-264">to open the suggestions menu and click **Add missing standard property (border-radius)**.</span></span>

    <span data-ttu-id="c8182-265">![Dodaj brakujące sugestii standardowe właściwości](visual-studio-2013-web-tools/_static/image22.png "Dodaj brakujące sugestii właściwości standardowych")</span><span class="sxs-lookup"><span data-stu-id="c8182-265">![Add missing standard property suggestion](visual-studio-2013-web-tools/_static/image22.png "Add missing standard property suggestion")</span></span>

    <span data-ttu-id="c8182-266">*Dodaj brakujące sugestii właściwości standardowe*</span><span class="sxs-lookup"><span data-stu-id="c8182-266">*Add missing standard property suggestion*</span></span>
16. <span data-ttu-id="c8182-267">**Promień obramowania** reguły CSS jest automatycznie dodawana właściwość.</span><span class="sxs-lookup"><span data-stu-id="c8182-267">The **border-radius** property is automatically added to the CSS rule.</span></span>

    <span data-ttu-id="c8182-268">![Brak standardowych dodanie właściwości](visual-studio-2013-web-tools/_static/image23.png "Brak dodanie właściwości standardowe")</span><span class="sxs-lookup"><span data-stu-id="c8182-268">![Missing standard property added](visual-studio-2013-web-tools/_static/image23.png "Missing standard property added")</span></span>

    <span data-ttu-id="c8182-269">*Brak dodanie właściwości standardowe*</span><span class="sxs-lookup"><span data-stu-id="c8182-269">*Missing standard property added*</span></span>
17. <span data-ttu-id="c8182-270">Przesuń wskaźnik nad **promień obramowania** właściwość do wyświetlenia **etykietki narzędzia macierzy przeglądarki**.</span><span class="sxs-lookup"><span data-stu-id="c8182-270">Move the pointer over the **border-radius** property to display the **Browser matrix tooltip**.</span></span> <span data-ttu-id="c8182-271">**Etykietki narzędzia macierzy przeglądarki** przedstawia dostępność właściwości w każdej przeglądarce.</span><span class="sxs-lookup"><span data-stu-id="c8182-271">The **Browser matrix tooltip** shows the availability of the property in each browser.</span></span>

    <span data-ttu-id="c8182-272">![Etykietka narzędzia macierzy przeglądarki](visual-studio-2013-web-tools/_static/image24.png "etykietki narzędzia macierzy przeglądarki")</span><span class="sxs-lookup"><span data-stu-id="c8182-272">![Browser matrix tooltip](visual-studio-2013-web-tools/_static/image24.png "Browser matrix tooltip")</span></span>

    <span data-ttu-id="c8182-273">*Etykietka narzędzia macierzy przeglądarki*</span><span class="sxs-lookup"><span data-stu-id="c8182-273">*Browser matrix tooltip*</span></span>
18. <span data-ttu-id="c8182-274">Należy zauważyć, że wartość **promień obramowania** właściwość jest nadal podkreślony.</span><span class="sxs-lookup"><span data-stu-id="c8182-274">Notice that the value of the **border-radius** property is still underlined.</span></span> <span data-ttu-id="c8182-275">Przesuń wskaźnik nad wartość, aby zobaczyć komunikat ostrzegawczy.</span><span class="sxs-lookup"><span data-stu-id="c8182-275">Move the pointer over the value to see the warning message.</span></span>

    <span data-ttu-id="c8182-276">![Ostrzeżenie wartość właściwości promień obramowania](visual-studio-2013-web-tools/_static/image25.png "ostrzeżenie wartość właściwości promień obramowania")</span><span class="sxs-lookup"><span data-stu-id="c8182-276">![Border-radius property value warning](visual-studio-2013-web-tools/_static/image25.png "Border-radius property value warning")</span></span>

    <span data-ttu-id="c8182-277">*Ostrzeżenie wartość właściwości promień obramowania*</span><span class="sxs-lookup"><span data-stu-id="c8182-277">*Border-radius property value warning*</span></span>
19. <span data-ttu-id="c8182-278">Usuń jednostkę **promień obramowania** wartość właściwości zgodnie z sugestią podaną w etykietce narzędzia.</span><span class="sxs-lookup"><span data-stu-id="c8182-278">Remove the unit of the **border-radius** property value as suggested by the tooltip.</span></span>
20. <span data-ttu-id="c8182-279">Jako **promień obramowania** jest właściwością standardowego do definiowania obramowania jak zaokrąglone rogi są, możesz usunąć **- webkit-border-radius** właściwości i wartości z reguły CSS.</span><span class="sxs-lookup"><span data-stu-id="c8182-279">As **border-radius** is the standard property for defining how rounded border corners are, you can remove the **-webkit-border-radius** property and value from the CSS rule.</span></span>
21. <span data-ttu-id="c8182-280">Umieść punkt wstawiania w **zawijanie wyrazów** właściwość i zwróć uwagę, że tagu inteligentnego również pojawia się poniżej.</span><span class="sxs-lookup"><span data-stu-id="c8182-280">Place the caret in the **word-wrap** property and notice that the smart tag also appears below.</span></span>
22. <span data-ttu-id="c8182-281">Otwórz menu, a następnie kliknij przycisk **Dodaj brakujące szczegóły dostawcy**.</span><span class="sxs-lookup"><span data-stu-id="c8182-281">Open the menu and click **Add missing vendor specifics**.</span></span>

    <span data-ttu-id="c8182-282">![Dodaj brakujące sugestii szczegóły dostawcy](visual-studio-2013-web-tools/_static/image26.png "Dodaj brakujące sugestii szczegóły dostawcy")</span><span class="sxs-lookup"><span data-stu-id="c8182-282">![Add missing vendor specifics suggestion](visual-studio-2013-web-tools/_static/image26.png "Add missing vendor specifics suggestion")</span></span>

    <span data-ttu-id="c8182-283">*Dodaj brakujące sugestii szczegóły dostawcy*</span><span class="sxs-lookup"><span data-stu-id="c8182-283">*Add missing vendor specifics suggestion*</span></span>
23. <span data-ttu-id="c8182-284">**-Ms-zawijanie** reguły CSS jest automatycznie dodawana właściwość.</span><span class="sxs-lookup"><span data-stu-id="c8182-284">The **-ms-word-wrap** property is automatically added to the CSS rule.</span></span>

    <span data-ttu-id="c8182-285">![Dodanie właściwości określonych dostawców](visual-studio-2013-web-tools/_static/image27.png "dodanie właściwości określonych dostawców")</span><span class="sxs-lookup"><span data-stu-id="c8182-285">![Vendor specific property added](visual-studio-2013-web-tools/_static/image27.png "Vendor specific property added")</span></span>

    <span data-ttu-id="c8182-286">*Dodanie właściwości określonych dostawców*</span><span class="sxs-lookup"><span data-stu-id="c8182-286">*Vendor specific property added*</span></span>

<a id="Ex1Task4"></a>
#### <a name="task-4---updating-the-html-code-from-the-browser"></a><span data-ttu-id="c8182-287">Zadanie 4. aktualizowanie kodu HTML w przeglądarce</span><span class="sxs-lookup"><span data-stu-id="c8182-287">Task 4 - Updating the HTML Code from the Browser</span></span>

<span data-ttu-id="c8182-288">W tym zadaniu użyje Browser Link **trybu projektowania** funkcji do edycji obiektu DOM w przeglądarce i przesłać zmiany do pliku źródła HTML w programie Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="c8182-288">In this task, you will use the Browser Link's **Design Mode** feature to edit the DOM object from the browser and transfer the changes to the HTML source file in Visual Studio.</span></span>

1. <span data-ttu-id="c8182-289">Google Chrome, naciśnij **CTRL** + **ALT** + **D** Aby włączyć tryb projektowania.</span><span class="sxs-lookup"><span data-stu-id="c8182-289">In Google Chrome, press **CTRL** + **ALT** + **D** to enable Design Mode.</span></span>
2. <span data-ttu-id="c8182-290">Przesuń wskaźnik nad **Lorem Ipsum dolor sit amet** etykiety, a następnie kliknij przycisk.</span><span class="sxs-lookup"><span data-stu-id="c8182-290">Move the pointer over the **Lorem Ipsum dolor sit amet** label and click.</span></span>

    <span data-ttu-id="c8182-291">![Edytowanie pytania](visual-studio-2013-web-tools/_static/image28.png "Edytowanie pytania")</span><span class="sxs-lookup"><span data-stu-id="c8182-291">![Editing the question](visual-studio-2013-web-tools/_static/image28.png "Editing the question")</span></span>

    <span data-ttu-id="c8182-292">*Edytowanie pytania*</span><span class="sxs-lookup"><span data-stu-id="c8182-292">*Editing the question*</span></span>
3. <span data-ttu-id="c8182-293">Kursor powinien zostać wyświetlony.</span><span class="sxs-lookup"><span data-stu-id="c8182-293">A cursor should appear.</span></span> <span data-ttu-id="c8182-294">Zastąp oryginalny tekst z *jak ona wygląda podczas zapisu czy dłużej pytanie?*, a następnie naciśnij klawisz **ESC** aby wyjść z trybu projektowania.</span><span class="sxs-lookup"><span data-stu-id="c8182-294">Replace the original text with *What does it look like when I write a longer question?*, and then press **ESC** to exit Design Mode.</span></span>

    <span data-ttu-id="c8182-295">![Pytanie, edytować](visual-studio-2013-web-tools/_static/image29.png "edytować pytanie")</span><span class="sxs-lookup"><span data-stu-id="c8182-295">![Question edited](visual-studio-2013-web-tools/_static/image29.png "Question edited")</span></span>

    <span data-ttu-id="c8182-296">*Edytować pytanie*</span><span class="sxs-lookup"><span data-stu-id="c8182-296">*Question edited*</span></span>
4. <span data-ttu-id="c8182-297">Przełącz do programu Visual Studio i Otwórz **Index.cshtml**, jeśli nie jest jeszcze otwarty.</span><span class="sxs-lookup"><span data-stu-id="c8182-297">Switch back to Visual Studio and open **Index.cshtml**, if not already opened.</span></span> <span data-ttu-id="c8182-298">Należy zauważyć, że tekst zawarty wewnątrz **&lt;p&gt;** element został zaktualizowany.</span><span class="sxs-lookup"><span data-stu-id="c8182-298">Notice that the inner text of the **&lt;p&gt;** element has been updated.</span></span>

    <span data-ttu-id="c8182-299">![Zaktualizowano pytanie, na stronie HTML](visual-studio-2013-web-tools/_static/image30.png "pytanie zaktualizowane strony HTML")</span><span class="sxs-lookup"><span data-stu-id="c8182-299">![Updated question in the HTML page](visual-studio-2013-web-tools/_static/image30.png "Updated question in the HTML page")</span></span>

    <span data-ttu-id="c8182-300">*Zaktualizowano pytanie, na stronie HTML*</span><span class="sxs-lookup"><span data-stu-id="c8182-300">*Updated question in the HTML page*</span></span>

<a id="Ex1Task5"></a>
#### <a name="task-5---reviewing-seo-related-warnings"></a><span data-ttu-id="c8182-301">Zadanie 5 - recenzowania optymalizacji dla aparatów wyszukiwania związane z ostrzeżeniami</span><span class="sxs-lookup"><span data-stu-id="c8182-301">Task 5 - Reviewing SEO Related Warnings</span></span>

<span data-ttu-id="c8182-302">**Optymalizacja wyszukiwarek** (SEO) to proces polegający na wprowadzaniu wyższe rangę witryny sieci Web na liście wyników z wyszukiwarki.</span><span class="sxs-lookup"><span data-stu-id="c8182-302">**Search Engine Optimization** (SEO) is the process of making a website rank higher on a search engine's list of results.</span></span> <span data-ttu-id="c8182-303">Im większa szereguje lokacji i skuteczniejsze ta opcja jest wyświetlana, więcej odwiedzających lokacji zostanie uzyskiwanie tego aparatu wyszukiwania.</span><span class="sxs-lookup"><span data-stu-id="c8182-303">The higher the site ranks and the more consistently it is listed, the more visitors the site will get from that search engine.</span></span> <span data-ttu-id="c8182-304">Web Essentials zawiera narzędzia analitycznego, który analizuje HTML, raporty problemy znalezione i zapewnia pomoc, aby to naprawić.</span><span class="sxs-lookup"><span data-stu-id="c8182-304">Web Essentials incorporates an analytical tool that examines HTML, reports the issues found and provides assistance to fix them.</span></span>

1. <span data-ttu-id="c8182-305">Przejdź do **widoku** menu i kliknij przycisk **lista błędów** otworzyć **lista błędów** okna.</span><span class="sxs-lookup"><span data-stu-id="c8182-305">Go to the **View** menu and click **Error List** to open the **Error List** window.</span></span>

    <span data-ttu-id="c8182-306">![Błąd w widoku listy menu](visual-studio-2013-web-tools/_static/image31.png "lista błędów w menu Widok")</span><span class="sxs-lookup"><span data-stu-id="c8182-306">![Error List in View menu](visual-studio-2013-web-tools/_static/image31.png "Error List in View menu")</span></span>

    <span data-ttu-id="c8182-307">*Błąd w widoku listy menu*</span><span class="sxs-lookup"><span data-stu-id="c8182-307">*Error List in View menu*</span></span>
2. <span data-ttu-id="c8182-308">Należy zauważyć, że istnieje ostrzeżenie optymalizacji dla aparatów wyszukiwania z informacją, że **&lt;meta&gt;** tagu dla Brak opisu strony.</span><span class="sxs-lookup"><span data-stu-id="c8182-308">Notice that there is an SEO warning notifying that a **&lt;meta&gt;** tag for the page description is missing.</span></span> <span data-ttu-id="c8182-309">Kliknij dwukrotnie wpis ostrzeżenie optymalizacji dla aparatów wyszukiwania, aby rozwiązać ten problem.</span><span class="sxs-lookup"><span data-stu-id="c8182-309">Double-click the SEO warning entry to fix it.</span></span>

    <span data-ttu-id="c8182-310">![Okno listy błędów](visual-studio-2013-web-tools/_static/image32.png "okno Lista błędów")</span><span class="sxs-lookup"><span data-stu-id="c8182-310">![Error List window](visual-studio-2013-web-tools/_static/image32.png "Error List window")</span></span>

    <span data-ttu-id="c8182-311">*Okno listy błędów*</span><span class="sxs-lookup"><span data-stu-id="c8182-311">*Error List window*</span></span>
3. <span data-ttu-id="c8182-312">W **Web Essentials** okno dialogowe, kliknij przycisk **tak** do wstawienia opis &lt;meta&gt; tagu.</span><span class="sxs-lookup"><span data-stu-id="c8182-312">In the **Web Essentials** dialog box, click **Yes** to insert a description &lt;meta&gt; tag.</span></span>

    <span data-ttu-id="c8182-313">![Okno dialogowe usługi sieci Web Essentials](visual-studio-2013-web-tools/_static/image33.png "Web Essentials, okno dialogowe")</span><span class="sxs-lookup"><span data-stu-id="c8182-313">![Web Essentials dialog box](visual-studio-2013-web-tools/_static/image33.png "Web Essentials dialog box")</span></span>

    <span data-ttu-id="c8182-314">*Okno dialogowe usługi sieci Web Essentials*</span><span class="sxs-lookup"><span data-stu-id="c8182-314">*Web Essentials dialog box*</span></span>
4. <span data-ttu-id="c8182-315">Edytor dla  **\_Layout.cshtml** otwiera i **&lt;meta&gt;** tag jest automatycznie dodawany do **head** sekcji Plik HTML.</span><span class="sxs-lookup"><span data-stu-id="c8182-315">The editor for **\_Layout.cshtml** opens and the **&lt;meta&gt;** tag is automatically added to the **head** section of the HTML file.</span></span>

    <span data-ttu-id="c8182-316">![Tag meta automatycznie dodawane na stronie _układ](visual-studio-2013-web-tools/_static/image34.png "metatag automatycznie dodawane na stronie _układ")</span><span class="sxs-lookup"><span data-stu-id="c8182-316">![Meta tag automatically added in _Layout page](visual-studio-2013-web-tools/_static/image34.png "Meta tag automatically added in _Layout page")</span></span>

    <span data-ttu-id="c8182-317">*Tag meta automatycznie dodawane do \_stronę układu*</span><span class="sxs-lookup"><span data-stu-id="c8182-317">*Meta tag automatically added to \_Layout page*</span></span>
5. <span data-ttu-id="c8182-318">Zmień wartość właściwości **zawartości** atrybutu *GeekQuiz* i Zapisz plik.</span><span class="sxs-lookup"><span data-stu-id="c8182-318">Change the value of the **content** attribute to *GeekQuiz* and save the file.</span></span>

<a id="Exercise2"></a>
### <a name="exercise-2-taking-advantage-of-code-snippets-and-intellisense"></a><span data-ttu-id="c8182-319">Ćwiczenie 2: Wykorzystanie IntelliSense i fragmentów kodu</span><span class="sxs-lookup"><span data-stu-id="c8182-319">Exercise 2: Taking Advantage of Code Snippets and IntelliSense</span></span>

<span data-ttu-id="c8182-320">Przy użyciu Web Essentials edytora HTML została rozszerzona o dodatkowe funkcje.</span><span class="sxs-lookup"><span data-stu-id="c8182-320">With Web Essentials, the HTML editor has been extended with extra functionality.</span></span> <span data-ttu-id="c8182-321">W tym ćwiczeniu zobaczysz niektóre nowe funkcje, które są przydatne podczas tworzenia aplikacji sieci web.</span><span class="sxs-lookup"><span data-stu-id="c8182-321">In this exercise, you will see some new features that are helpful when developing web applications.</span></span>

<a id="Ex2Task1"></a>
#### <a name="task-1---using-intellisense-in-html-documents"></a><span data-ttu-id="c8182-322">Zadanie 1 — za pomocą funkcji IntelliSense w dokumentach HTML</span><span class="sxs-lookup"><span data-stu-id="c8182-322">Task 1 - Using IntelliSense in HTML Documents</span></span>

<span data-ttu-id="c8182-323">Pierwszy nowej funkcji, zostanie wyświetlony w ramach tego zadania jest nazywany **dynamiczne IntelliSense**.</span><span class="sxs-lookup"><span data-stu-id="c8182-323">The first new feature you will see in this task is called **Dynamic IntelliSense**.</span></span> <span data-ttu-id="c8182-324">Dynamiczne IntelliSense odczytuje inne znaczniki i atrybuty w celu możliwych identyfikatorów, które będą używane.</span><span class="sxs-lookup"><span data-stu-id="c8182-324">Dynamic IntelliSense reads other tags and attributes to infer the possible ids you will use.</span></span>

<span data-ttu-id="c8182-325">W tym zadaniu zostanie utworzony nowy element formularza HTML, który zawiera etykiety i pola wejściowego.</span><span class="sxs-lookup"><span data-stu-id="c8182-325">In this task, you will create a new HTML form element which contains a label and an input field.</span></span> <span data-ttu-id="c8182-326">A następnie dodasz **dla** atrybutu do etykiety, aby powiązać go z danych wejściowych, a zobaczysz sugestie funkcji IntelliSense, w oparciu o identyfikatorach danych wejściowych w zakresie.</span><span class="sxs-lookup"><span data-stu-id="c8182-326">Then you will add a **for** attribute to the label to bind it to the input, and you will see IntelliSense suggestions based on the ids of the inputs in scope.</span></span>

1. <span data-ttu-id="c8182-327">Otwórz **Visual Studio Express 2013 for Web** i **Begin.sln** rozwiązanie znajduje się w **/Ex2-TakingAdvantageofCodeSnippetsandIntelliSense/początkowy w źródle** folderu.</span><span class="sxs-lookup"><span data-stu-id="c8182-327">Open **Visual Studio Express 2013 for Web** and the **Begin.sln** solution located in the **Source/Ex2-TakingAdvantageofCodeSnippetsandIntelliSense/Begin** folder.</span></span> <span data-ttu-id="c8182-328">Alternatywnie można kontynuować z rozwiązaniem uzyskanym w poprzednim ćwiczeniu.</span><span class="sxs-lookup"><span data-stu-id="c8182-328">Alternatively, you can continue with the solution that you obtained in the previous exercise.</span></span>
2. <span data-ttu-id="c8182-329">W **Eksploratora rozwiązań**, otwórz **Index.cshtml** plik znajdujący się w **widoków** | **Home** folderu.</span><span class="sxs-lookup"><span data-stu-id="c8182-329">In **Solution Explorer**, open the **Index.cshtml** file located in the **Views** | **Home** folder.</span></span>
3. <span data-ttu-id="c8182-330">Dodaj następującą postać wewnątrz **&lt;sekcji&gt;** elementu.</span><span class="sxs-lookup"><span data-stu-id="c8182-330">Add the following form inside the **&lt;section&gt;** element.</span></span>

    <span data-ttu-id="c8182-331">(Code Snippet — *VisualStudio2013WebTooling* - *Ex2* - *formularza*)</span><span class="sxs-lookup"><span data-stu-id="c8182-331">(Code Snippet - *VisualStudio2013WebTooling* - *Ex2* - *Form*)</span></span>

    [!code-html[Main](visual-studio-2013-web-tools/samples/sample4.html)]
4. <span data-ttu-id="c8182-332">Tagu wejściowego powinien być poprzedzony etykiety z niektórych opis pola.</span><span class="sxs-lookup"><span data-stu-id="c8182-332">The input tag should be preceded by a label with some description of the field.</span></span> <span data-ttu-id="c8182-333">Dodaj następujące etykietę przed tagu wejściowego.</span><span class="sxs-lookup"><span data-stu-id="c8182-333">Add the following label before the input tag.</span></span>

    [!code-html[Main](visual-studio-2013-web-tools/samples/sample5.html)]
5. <span data-ttu-id="c8182-334">**Dla** atrybutu **&lt;etykiety&gt;** Określa, który element formularza, a etykiety jest powiązany z.</span><span class="sxs-lookup"><span data-stu-id="c8182-334">The **for** attribute of a **&lt;label&gt;** specifies which form element a label is bound to.</span></span> <span data-ttu-id="c8182-335">Wartość atrybutu powinna być równa identyfikator elementu powiązane.</span><span class="sxs-lookup"><span data-stu-id="c8182-335">The attribute's value should be equal to the id of the related element.</span></span> <span data-ttu-id="c8182-336">Dodaj **dla** atrybutu **&lt;etykiety&gt;** elementu.</span><span class="sxs-lookup"><span data-stu-id="c8182-336">Add the **for** attribute to the **&lt;label&gt;** element.</span></span> <span data-ttu-id="c8182-337">Jak pokazano na poniższej ilustracji &quot;nazwa&quot; wartość pojawia się w oknie funkcji IntelliSense, na podstawie identyfikatora elementów w obrębie tego samego zakresu (otaczający  **&lt;formularza&gt;**).</span><span class="sxs-lookup"><span data-stu-id="c8182-337">As shown in the following figure, the &quot;name&quot; value pops up in the IntelliSense box, based on the id of the elements within the same scope (the enclosing **&lt;form&gt;**).</span></span>

    <span data-ttu-id="c8182-338">![Wyświetlanie identyfikatora w technologii IntelliSense](visual-studio-2013-web-tools/_static/image35.png "zawierające identyfikator w technologii IntelliSense")</span><span class="sxs-lookup"><span data-stu-id="c8182-338">![Showing the id in IntelliSense](visual-studio-2013-web-tools/_static/image35.png "Showing the id in IntelliSense")</span></span>

    <span data-ttu-id="c8182-339">*Wyświetlanie identyfikatora w technologii IntelliSense*</span><span class="sxs-lookup"><span data-stu-id="c8182-339">*Showing the id in IntelliSense*</span></span>
6. <span data-ttu-id="c8182-340">Usuń ostatnio dodane **&lt;formularza&gt;** elementu i jego zawartości.</span><span class="sxs-lookup"><span data-stu-id="c8182-340">Delete the recently added **&lt;form&gt;** element and its content.</span></span>

<a id="Ex2Task2"></a>
#### <a name="task-2---using-html-code-snippets"></a><span data-ttu-id="c8182-341">Zadanie 2 — przy użyciu fragmentów kodu HTML</span><span class="sxs-lookup"><span data-stu-id="c8182-341">Task 2 - Using HTML Code Snippets</span></span>

<span data-ttu-id="c8182-342">HTML5 wprowadzono więcej niż 25 nowe znaczniki semantyczne.</span><span class="sxs-lookup"><span data-stu-id="c8182-342">HTML5 introduced more than 25 new semantic tags.</span></span> <span data-ttu-id="c8182-343">Program Visual Studio ma już obsługę funkcji IntelliSense na te tagi, ale Visual Studio 2013 umożliwia szybsze i prostsze do zapisania znaczników przez dodanie nowych fragmentów kodu.</span><span class="sxs-lookup"><span data-stu-id="c8182-343">Visual Studio already had IntelliSense support for these tags, but Visual Studio 2013 makes it faster and easier to write markup by adding new code snippets.</span></span> <span data-ttu-id="c8182-344">Chociaż te tagi nie są skomplikowane, pochodzą z kilku małych precyzyjnie, takie jak dodawanie planów awaryjnych poprawnego kodera-dekodera dla *audio* tagu.</span><span class="sxs-lookup"><span data-stu-id="c8182-344">Though these tags are not complicated, they come with a few small subtleties, such as adding the correct codec fallbacks for the *audio* tag.</span></span> <span data-ttu-id="c8182-345">W tym zadaniu zostanie wyświetlony fragmenty kodu HTML dla tagu audio.</span><span class="sxs-lookup"><span data-stu-id="c8182-345">In this task, you will see the HTML code snippets for the audio tag.</span></span>

1. <span data-ttu-id="c8182-346">W **Index.cshtml** pliku, wpisz  **&lt;aud** wewnątrz **&lt;sekcji&gt;** elementu, jak pokazano na poniższej ilustracji.</span><span class="sxs-lookup"><span data-stu-id="c8182-346">In the **Index.cshtml** file, type **&lt;aud** inside the **&lt;section&gt;** element as shown in the following figure.</span></span>

    <span data-ttu-id="c8182-347">![Wstawianie elementu audio](visual-studio-2013-web-tools/_static/image36.png "Wstawianie elementu audio")</span><span class="sxs-lookup"><span data-stu-id="c8182-347">![Inserting an audio element](visual-studio-2013-web-tools/_static/image36.png "Inserting an audio element")</span></span>

    <span data-ttu-id="c8182-348">*Wstawianie elementu audio*</span><span class="sxs-lookup"><span data-stu-id="c8182-348">*Inserting an audio element*</span></span>
2. <span data-ttu-id="c8182-349">Naciśnij klawisz **kartę** dwa razy i zwróć uwagę, jak poniższy kod dodaje się na stronie, a znajduje się kursor na **src** atrybut pierwsze źródło.</span><span class="sxs-lookup"><span data-stu-id="c8182-349">Press **TAB** twice and notice how the following code is added on the page and the cursor is placed on the **src** attribute of the first source.</span></span>

    [!code-html[Main](visual-studio-2013-web-tools/samples/sample6.html)]

    > [!NOTE]
    > <span data-ttu-id="c8182-350">Naciskając **kartę** klucza dwukrotnie wstawieniu fragmentu kodu.</span><span class="sxs-lookup"><span data-stu-id="c8182-350">By pressing the **TAB** key twice, the code snippet is inserted.</span></span> <span data-ttu-id="c8182-351">Audio fragment kodu przedstawia użycie standardowego *audio* tag z dwóch plików źródłowych dla Ulepszona obsługa.</span><span class="sxs-lookup"><span data-stu-id="c8182-351">The audio snippet shows the standard usage of the *audio* tag, with two source files for improved support.</span></span>
3. <span data-ttu-id="c8182-352">Usuwanie drugi wiersz i aktualizowanie źródła pierwszy wiersz z następującego linku do pokazania WebCampsTV Katana: [ http://media.ch9.ms/ch9/11d8/604b8163-fad3-4f12-9607-b404201211d8/KatanaProject.mp3 ](http://media.ch9.ms/ch9/11d8/604b8163-fad3-4f12-9607-b404201211d8/KatanaProject.mp3).</span><span class="sxs-lookup"><span data-stu-id="c8182-352">Delete the second line and update the source of the first line with the following link to the WebCampsTV Katana show: [http://media.ch9.ms/ch9/11d8/604b8163-fad3-4f12-9607-b404201211d8/KatanaProject.mp3](http://media.ch9.ms/ch9/11d8/604b8163-fad3-4f12-9607-b404201211d8/KatanaProject.mp3).</span></span> <span data-ttu-id="c8182-353">Następnie kod wynikowy znajdują się poniżej.</span><span class="sxs-lookup"><span data-stu-id="c8182-353">The resulting code is shown below.</span></span>

    [!code-html[Main](visual-studio-2013-web-tools/samples/sample7.html)]

    > [!NOTE]
    > <span data-ttu-id="c8182-354">Plik źródłowy *KatanaProject.mp3* służy jako przykład.</span><span class="sxs-lookup"><span data-stu-id="c8182-354">The source file *KatanaProject.mp3* is used as an example.</span></span> <span data-ttu-id="c8182-355">Można użyć innego źródła, jeśli użytkownik sobie tego życzy.</span><span class="sxs-lookup"><span data-stu-id="c8182-355">You can use another source if you prefer.</span></span>
4. <span data-ttu-id="c8182-356">Naciśnij klawisz **CTRL** + **S** można zapisać pliku.</span><span class="sxs-lookup"><span data-stu-id="c8182-356">Press **CTRL** + **S** to save the file.</span></span>
5. <span data-ttu-id="c8182-357">Naciśnij klawisz **CTRL** + **F5** do uruchomienia aplikacji.</span><span class="sxs-lookup"><span data-stu-id="c8182-357">Press **CTRL** + **F5** to start the application.</span></span>
6. <span data-ttu-id="c8182-358">Należy zauważyć, że odtwarzacz audio została dodana do aplikacji.</span><span class="sxs-lookup"><span data-stu-id="c8182-358">Notice that an audio player was added to the application.</span></span>

    <span data-ttu-id="c8182-359">![Odtwarzacz audio w programie Internet Explorer](visual-studio-2013-web-tools/_static/image37.png "odtwarzacz Audio w programie Internet Explorer")</span><span class="sxs-lookup"><span data-stu-id="c8182-359">![Audio player in Internet Explorer](visual-studio-2013-web-tools/_static/image37.png "Audio player in Internet Explorer")</span></span>

    <span data-ttu-id="c8182-360">*Odtwarzacz audio w programie Internet Explorer*</span><span class="sxs-lookup"><span data-stu-id="c8182-360">*Audio player in Internet Explorer*</span></span>

    <span data-ttu-id="c8182-361">![Odtwarzacz audio w przeglądarce Google Chrome](visual-studio-2013-web-tools/_static/image38.png "odtwarzacz Audio w przeglądarce Google Chrome")</span><span class="sxs-lookup"><span data-stu-id="c8182-361">![Audio player in Google Chrome](visual-studio-2013-web-tools/_static/image38.png "Audio player in Google Chrome")</span></span>

    <span data-ttu-id="c8182-362">*Odtwarzacz audio w przeglądarce Google Chrome*</span><span class="sxs-lookup"><span data-stu-id="c8182-362">*Audio player in Google Chrome*</span></span>
7. <span data-ttu-id="c8182-363">Nie zamykaj przeglądarki.</span><span class="sxs-lookup"><span data-stu-id="c8182-363">Do not close the browsers.</span></span> <span data-ttu-id="c8182-364">Użyjesz ich w ramach następnego zadania.</span><span class="sxs-lookup"><span data-stu-id="c8182-364">You will use them in the next task.</span></span>

<a id="Ex2Task3"></a>
#### <a name="task-3---using-intellisense-in-javascript-documents"></a><span data-ttu-id="c8182-365">Zadanie 3 — za pomocą funkcji IntelliSense w dokumentach JavaScript</span><span class="sxs-lookup"><span data-stu-id="c8182-365">Task 3 - Using IntelliSense in JavaScript Documents</span></span>

<span data-ttu-id="c8182-366">Za pomocą sieci Web Essentials 2013 arkusze stylów i strony HTML przedstawić listę identyfikatorów i nazw klas.</span><span class="sxs-lookup"><span data-stu-id="c8182-366">With Web Essentials 2013, style sheets and HTML pages produce a list of IDs and class names.</span></span> <span data-ttu-id="c8182-367">W tym zadaniu dowiesz się, jak te listy poprawić obsługę funkcji IntelliSense języka JavaScript, w sieci Web Essentials 2013.</span><span class="sxs-lookup"><span data-stu-id="c8182-367">In this task, you will learn how those lists improve JavaScript IntelliSense support in Web Essentials 2013.</span></span>

1. <span data-ttu-id="c8182-368">W **Index.cshtml** plików, Dodaj następujący kod, aby zdefiniować **skryptu** tag w przypadku kodu JavaScript.</span><span class="sxs-lookup"><span data-stu-id="c8182-368">In the **Index.cshtml** file, add the following code to define a **script** tag for JavaScript code.</span></span>

    [!code-cshtml[Main](visual-studio-2013-web-tools/samples/sample8.cshtml)]
2. <span data-ttu-id="c8182-369">Dodaj następujący kod wewnątrz **skryptu** tag, aby zdefiniować funkcję wywołania zwrotnego gotowe.</span><span class="sxs-lookup"><span data-stu-id="c8182-369">Add the following code inside the **script** tag to define the ready callback function.</span></span>

    <span data-ttu-id="c8182-370">(Code Snippet — *VisualStudio2013WebTooling* - *Ex2* - *ReadyFunction*)</span><span class="sxs-lookup"><span data-stu-id="c8182-370">(Code Snippet - *VisualStudio2013WebTooling* - *Ex2* - *ReadyFunction*)</span></span>

    [!code-javascript[Main](visual-studio-2013-web-tools/samples/sample9.js)]
3. <span data-ttu-id="c8182-371">Umieść punkt wstawiania w **skryptu** tagu, a następnie naciśnij klawisz **CTRL** + **.**</span><span class="sxs-lookup"><span data-stu-id="c8182-371">Place the caret in the **script** tag and press **CTRL** + **.**</span></span> <span data-ttu-id="c8182-372">Aby otworzyć menu sugestię.</span><span class="sxs-lookup"><span data-stu-id="c8182-372">to open the suggestion menu.</span></span>
4. <span data-ttu-id="c8182-373">Kliknij przycisk **wyodrębnić pliku**.</span><span class="sxs-lookup"><span data-stu-id="c8182-373">Click **Extract To File**.</span></span>

    <span data-ttu-id="c8182-374">![JavaScript Wyodrębnij plik sugestią](visual-studio-2013-web-tools/_static/image39.png "JavaScript wyodrębnić pliku sugestii")</span><span class="sxs-lookup"><span data-stu-id="c8182-374">![JavaScript extract to file suggestion](visual-studio-2013-web-tools/_static/image39.png "JavaScript extract to file suggestion")</span></span>

    <span data-ttu-id="c8182-375">*JavaScript wyodrębnić pliku sugestii*</span><span class="sxs-lookup"><span data-stu-id="c8182-375">*JavaScript extract to file suggestion*</span></span>
5. <span data-ttu-id="c8182-376">W **Zapisz jako** wybierz **skrypty** folder i nazwę pliku **init.js** i kliknij przycisk **Zapisz**.</span><span class="sxs-lookup"><span data-stu-id="c8182-376">In the **Save As** window, select the **Scripts** folder, name the file **init.js** and click **Save**.</span></span>

    <span data-ttu-id="c8182-377">![Zapisz jako okno](visual-studio-2013-web-tools/_static/image40.png "okno Zapisz jako")</span><span class="sxs-lookup"><span data-stu-id="c8182-377">![Save As window](visual-studio-2013-web-tools/_static/image40.png "Save As window")</span></span>

    <span data-ttu-id="c8182-378">*Okno Zapisz jako*</span><span class="sxs-lookup"><span data-stu-id="c8182-378">*Save As window*</span></span>

    > [!NOTE]
    > <span data-ttu-id="c8182-379">**Init.js** plik zostanie utworzony i zawartość skryptu zostanie przeniesiony do pliku.</span><span class="sxs-lookup"><span data-stu-id="c8182-379">The **init.js** file is created and the content of the script is moved to the file.</span></span>
    > 
    > <span data-ttu-id="c8182-380">![Plik init.js utworzony za pomocą dołączonej zawartości](visual-studio-2013-web-tools/_static/image41.png "Init.js plik utworzony za pomocą dołączonej zawartości")</span><span class="sxs-lookup"><span data-stu-id="c8182-380">![Init.js file created with the content included](visual-studio-2013-web-tools/_static/image41.png "Init.js file created with the content included")</span></span>
    > 
    > <span data-ttu-id="c8182-381">*Plik init.js utworzony za pomocą dołączonej zawartości*</span><span class="sxs-lookup"><span data-stu-id="c8182-381">*Init.js file created with the content included*</span></span>
6. <span data-ttu-id="c8182-382">Otwórz **Index.cshtml** pliku i sprawdź, czy tag skryptu został zastąpiony w odniesieniu do **init.js** pliku.</span><span class="sxs-lookup"><span data-stu-id="c8182-382">Open the **Index.cshtml** file and check that the script tag was replaced with a reference to the **init.js** file.</span></span>

    <span data-ttu-id="c8182-383">![Odwołania html init.js](visual-studio-2013-web-tools/_static/image42.png "Init.js html odwołania")</span><span class="sxs-lookup"><span data-stu-id="c8182-383">![Init.js html reference](visual-studio-2013-web-tools/_static/image42.png "Init.js html reference")</span></span>

    <span data-ttu-id="c8182-384">*Odwołania html init.js*</span><span class="sxs-lookup"><span data-stu-id="c8182-384">*Init.js html reference*</span></span>
7. <span data-ttu-id="c8182-385">Przejdź do **Eksploratora rozwiązań** i zwróć uwagę, że **init.js** plik został automatycznie dołączony w rozwiązaniu.</span><span class="sxs-lookup"><span data-stu-id="c8182-385">Go to the **Solution Explorer** and notice that the **init.js** file was included automatically in the solution.</span></span>

    <span data-ttu-id="c8182-386">![Plik init.js zawartych w rozwiązaniu](visual-studio-2013-web-tools/_static/image43.png "Init.js plików zawarte w rozwiązaniu")</span><span class="sxs-lookup"><span data-stu-id="c8182-386">![Init.js file included in solution](visual-studio-2013-web-tools/_static/image43.png "Init.js file included in solution")</span></span>

    <span data-ttu-id="c8182-387">*Plik init.js zawartych w rozwiązaniu*</span><span class="sxs-lookup"><span data-stu-id="c8182-387">*Init.js file included in solution*</span></span>
8. <span data-ttu-id="c8182-388">Przejdź z powrotem do **init.js** pliku do zaktualizowania **gotowe** funkcji wywołania zwrotnego.</span><span class="sxs-lookup"><span data-stu-id="c8182-388">Switch back to the **init.js** file to update the **ready** function callback.</span></span>
9. <span data-ttu-id="c8182-389">Wewnątrz definicji funkcji wywołania zwrotnego, który jest przekazywany do *gotowe*, Dodaj następujący kod, aby pobrać wszystkie elementy w atrybucie określonej klasy.</span><span class="sxs-lookup"><span data-stu-id="c8182-389">Inside the function callback definition that is passed to *ready*, add the following code to get all the elements by a specific class attribute.</span></span>

    [!code-javascript[Main](visual-studio-2013-web-tools/samples/sample10.js)]
10. <span data-ttu-id="c8182-390">Naciśnij klawisz **CTRL** + **miejsca** w cudzysłowie wewnątrz **getElementsByClassName** wywołania funkcji.</span><span class="sxs-lookup"><span data-stu-id="c8182-390">Press **CTRL** + **Space** between the quotes inside the **getElementsByClassName** function call.</span></span>

    <span data-ttu-id="c8182-391">![Wyświetlanie funkcji IntelliSense dla funkcji getElementsByClassName](visual-studio-2013-web-tools/_static/image44.png "pokazywanie IntelliSense dla funkcji getElementsByClassName")</span><span class="sxs-lookup"><span data-stu-id="c8182-391">![Showing IntelliSense for the getElementsByClassName function](visual-studio-2013-web-tools/_static/image44.png "Showing IntelliSense for the getElementsByClassName function")</span></span>

    <span data-ttu-id="c8182-392">*Wyświetlanie funkcji IntelliSense dla funkcji getElementsByClassName*</span><span class="sxs-lookup"><span data-stu-id="c8182-392">*Showing IntelliSense for the getElementsByClassName function*</span></span>

    > [!NOTE]
    > <span data-ttu-id="c8182-393">Należy zauważyć, że funkcja IntelliSense wyświetla klas zdefiniowanych w arkuszach stylów projektu.</span><span class="sxs-lookup"><span data-stu-id="c8182-393">Notice that IntelliSense shows the classes defined in the project style sheets.</span></span>
11. <span data-ttu-id="c8182-394">Zastąp wiersz, utworzony za pomocą następującego kodu.</span><span class="sxs-lookup"><span data-stu-id="c8182-394">Replace the line that you have created with the following code.</span></span>

    [!code-javascript[Main](visual-studio-2013-web-tools/samples/sample11.js)]
12. <span data-ttu-id="c8182-395">Umieść kursor po **au** w cudzysłowie w **getElementsByTagName** funkcji, a następnie naciśnij klawisz **CTRL** + **miejsca**.</span><span class="sxs-lookup"><span data-stu-id="c8182-395">Position the cursor after **au** inside the quotes in the **getElementsByTagName** function and press **CTRL** + **Space**.</span></span>

    <span data-ttu-id="c8182-396">![Wyświetlanie funkcji IntelliSense dla metody getElementByTagName](visual-studio-2013-web-tools/_static/image45.png "pokazywanie IntelliSense dla metody getElementByTagName")</span><span class="sxs-lookup"><span data-stu-id="c8182-396">![Showing IntelliSense for the getElementByTagName method](visual-studio-2013-web-tools/_static/image45.png "Showing IntelliSense for the getElementByTagName method")</span></span>

    <span data-ttu-id="c8182-397">*Wyświetlanie funkcji IntelliSense dla metody getElementsByTagName*</span><span class="sxs-lookup"><span data-stu-id="c8182-397">*Showing IntelliSense for the getElementsByTagName method*</span></span>
13. <span data-ttu-id="c8182-398">Wybierz **&quot;audio&quot;** z listy i kliknij **ENTER**.</span><span class="sxs-lookup"><span data-stu-id="c8182-398">Select **&quot;audio&quot;** from the list and press **ENTER**.</span></span> <span data-ttu-id="c8182-399">Wynik jest wyświetlany na poniższej ilustracji.</span><span class="sxs-lookup"><span data-stu-id="c8182-399">The result is shown in the following figure.</span></span>

    <span data-ttu-id="c8182-400">![Trwa pobieranie elementów Audio](visual-studio-2013-web-tools/_static/image46.png "podczas pobierania elementów Audio")</span><span class="sxs-lookup"><span data-stu-id="c8182-400">![Retrieving Audio Elements](visual-studio-2013-web-tools/_static/image46.png "Retrieving Audio Elements")</span></span>

    <span data-ttu-id="c8182-401">*Trwa pobieranie elementów Audio*</span><span class="sxs-lookup"><span data-stu-id="c8182-401">*Retrieving Audio Elements*</span></span>
14. <span data-ttu-id="c8182-402">W **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy **init.js** w pliku **skrypty** i wybierz polecenie **pliki JavaScript zmniejszenie** z **Web Essentials** menu.</span><span class="sxs-lookup"><span data-stu-id="c8182-402">In **Solution Explorer**, right-click the **init.js** file in the **Scripts** folder and select **Minify JavaScript file(s)** from the **Web Essentials** menu.</span></span>

    <span data-ttu-id="c8182-403">![Minimalizowanie pliki JavaScript](visual-studio-2013-web-tools/_static/image47.png "pliki zmniejszenie JavaScript")</span><span class="sxs-lookup"><span data-stu-id="c8182-403">![Minify JavaScript file(s)](visual-studio-2013-web-tools/_static/image47.png "Minify JavaScript files")</span></span>

    <span data-ttu-id="c8182-404">*Minimalizowanie pliki JavaScript*</span><span class="sxs-lookup"><span data-stu-id="c8182-404">*Minify JavaScript file(s)*</span></span>
15. <span data-ttu-id="c8182-405">Po wyświetleniu monitu, aby włączyć automatyczne minimalizowanie, po kliknięciu zmiany pliku źródłowego **tak**.</span><span class="sxs-lookup"><span data-stu-id="c8182-405">When prompted to enable automatic minification when the source file changes click **Yes**.</span></span>

    <span data-ttu-id="c8182-406">![Włączanie automatycznego minimalizację ostrzeżenie](visual-studio-2013-web-tools/_static/image48.png "włączenie automatycznego minimalizację ostrzeżenie")</span><span class="sxs-lookup"><span data-stu-id="c8182-406">![Enabling automatic minification warning](visual-studio-2013-web-tools/_static/image48.png "Enabling automatic minification warning")</span></span>

    <span data-ttu-id="c8182-407">*Włączanie automatycznego minimalizację ostrzeżenie*</span><span class="sxs-lookup"><span data-stu-id="c8182-407">*Enabling automatic minification warning*</span></span>

    > [!NOTE]
    > <span data-ttu-id="c8182-408">**Init.min.js** jest tworzony i zostanie dodany jako zależność z **init.js** pliku.</span><span class="sxs-lookup"><span data-stu-id="c8182-408">The **init.min.js** is created and is added as a dependency of the **init.js** file.</span></span>
    > 
    > <span data-ttu-id="c8182-409">![Utworzony plik init.min.js](visual-studio-2013-web-tools/_static/image49.png "utworzony plik Init.min.js")</span><span class="sxs-lookup"><span data-stu-id="c8182-409">![Init.min.js file created](visual-studio-2013-web-tools/_static/image49.png "Init.min.js file created")</span></span>
    > 
    > <span data-ttu-id="c8182-410">*Utworzony plik init.min.js*</span><span class="sxs-lookup"><span data-stu-id="c8182-410">*Init.min.js file created*</span></span>
16. <span data-ttu-id="c8182-411">Otwórz **init.min.js** pliku i zwróć uwagę, że plik jest zminimalizowany.</span><span class="sxs-lookup"><span data-stu-id="c8182-411">Open the **init.min.js** file and notice that the file is minified.</span></span>

    <span data-ttu-id="c8182-412">![Zawartość pliku init.min.js](visual-studio-2013-web-tools/_static/image50.png "Init.min.js zawartość pliku")</span><span class="sxs-lookup"><span data-stu-id="c8182-412">![Init.min.js file content](visual-studio-2013-web-tools/_static/image50.png "Init.min.js file content")</span></span>

    <span data-ttu-id="c8182-413">*Zawartość pliku init.min.js*</span><span class="sxs-lookup"><span data-stu-id="c8182-413">*Init.min.js file content*</span></span>
17. <span data-ttu-id="c8182-414">W **init.js** plików, Dodaj następujący kod poniżej **getElementsByTagName** wywołania funkcji do odtwarzania dźwięku elementów.</span><span class="sxs-lookup"><span data-stu-id="c8182-414">In the **init.js** file, add the following code below the **getElementsByTagName** function call to play all the audio elements.</span></span>

    <span data-ttu-id="c8182-415">(Code Snippet — *VisualStudio2013WebTooling* - *Ex2* - *PlayAudioElements*)</span><span class="sxs-lookup"><span data-stu-id="c8182-415">(Code Snippet - *VisualStudio2013WebTooling* - *Ex2* - *PlayAudioElements*)</span></span>

    [!code-csharp[Main](visual-studio-2013-web-tools/samples/sample12.cs)]
18. <span data-ttu-id="c8182-416">Kliknij przycisk **CTRL** + **S** można zapisać pliku.</span><span class="sxs-lookup"><span data-stu-id="c8182-416">Click **CTRL** + **S** to save the file.</span></span> <span data-ttu-id="c8182-417">Ponieważ zminimalizowanego pliku jest już otwarty, zobaczysz okno dialogowe z informacją, że plik został zmodyfikowany poza edytorem źródła.</span><span class="sxs-lookup"><span data-stu-id="c8182-417">Since the minified file is already opened, you will see a dialog box saying that the file was modified outside of the source editor.</span></span> <span data-ttu-id="c8182-418">Kliknij przycisk **Tak**.</span><span class="sxs-lookup"><span data-stu-id="c8182-418">Click **Yes**.</span></span>

    <span data-ttu-id="c8182-419">![Ostrzeżenie programu Microsoft Visual Studio](visual-studio-2013-web-tools/_static/image51.png "ostrzeżenie programu Microsoft Visual Studio")</span><span class="sxs-lookup"><span data-stu-id="c8182-419">![Microsoft Visual Studio warning](visual-studio-2013-web-tools/_static/image51.png "Microsoft Visual Studio warning")</span></span>

    <span data-ttu-id="c8182-420">*Ostrzeżenie programu Microsoft Visual Studio*</span><span class="sxs-lookup"><span data-stu-id="c8182-420">*Microsoft Visual Studio warning*</span></span>
19. <span data-ttu-id="c8182-421">Przejdź z powrotem do **init.min.js** plik, aby sprawdzić, czy plik został zaktualizowany o nowy kod.</span><span class="sxs-lookup"><span data-stu-id="c8182-421">Switch back to the **init.min.js** file to verify that the file was updated with the new code.</span></span>

    <span data-ttu-id="c8182-422">![Zaktualizowano plik init.min.js](visual-studio-2013-web-tools/_static/image52.png "zaktualizowanego pliku Init.min.js")</span><span class="sxs-lookup"><span data-stu-id="c8182-422">![Init.min.js file updated](visual-studio-2013-web-tools/_static/image52.png "Init.min.js file updated")</span></span>

    <span data-ttu-id="c8182-423">*Zaktualizowano plik init.min.js*</span><span class="sxs-lookup"><span data-stu-id="c8182-423">*Init.min.js file updated*</span></span>
20. <span data-ttu-id="c8182-424">Kliknij przycisk **Odśwież łącze przeglądarki** przycisku.</span><span class="sxs-lookup"><span data-stu-id="c8182-424">Click the **Browser Link Refresh** button.</span></span>
21. <span data-ttu-id="c8182-425">Gdy obie przeglądarki są odświeżane odtwarzacze audio, które zostały użyte w poprzednim zadaniu rozpocznie się automatycznie odtwarzania.</span><span class="sxs-lookup"><span data-stu-id="c8182-425">Once both browsers are refreshed the audio players you saw in the previous task will start playing automatically.</span></span>

    <span data-ttu-id="c8182-426">![Odtwarzacz audio uwzględnione w widoku](visual-studio-2013-web-tools/_static/image53.png "odtwarzacz Audio uwzględnione w widoku")</span><span class="sxs-lookup"><span data-stu-id="c8182-426">![Audio player included in view](visual-studio-2013-web-tools/_static/image53.png "Audio player included in view")</span></span>

    <span data-ttu-id="c8182-427">*Odtwarzacz audio uwzględnione w widoku*</span><span class="sxs-lookup"><span data-stu-id="c8182-427">*Audio player included in view*</span></span>

* * *

<a id="Summary"></a>
## <a name="summary"></a><span data-ttu-id="c8182-428">Podsumowanie</span><span class="sxs-lookup"><span data-stu-id="c8182-428">Summary</span></span>

<span data-ttu-id="c8182-429">Przez ukończenie tego laboratorium praktycznego wiesz już, jak:</span><span class="sxs-lookup"><span data-stu-id="c8182-429">By completing this hands-on lab you have learned how to:</span></span>

- <span data-ttu-id="c8182-430">Korzystać z nowych funkcji edytora HTML objęte Web Essentials, takich jak rozbudowanych wstawek kodu HTML5 i Zen kodowania</span><span class="sxs-lookup"><span data-stu-id="c8182-430">Use new HTML editor features included in Web Essentials such as rich HTML5 code snippets and Zen coding</span></span>
- <span data-ttu-id="c8182-431">Nowe funkcje edytora CSS uwzględnione w Web Essentials, takich jak selektor kolorów i etykietkę narzędzia macierzy przeglądarki</span><span class="sxs-lookup"><span data-stu-id="c8182-431">Use new CSS editor features included in Web Essentials such as the Color picker and Browser matrix tooltip</span></span>
- <span data-ttu-id="c8182-432">Korzystać z nowych funkcji edytora JavaScript zawartych w Web Essentials, takich jak Wyodrębnianie pliku, a funkcja IntelliSense dla wszystkich elementów HTML</span><span class="sxs-lookup"><span data-stu-id="c8182-432">Use new JavaScript editor features included in Web Essentials such as Extract to File and IntelliSense for all HTML elements</span></span>
- <span data-ttu-id="c8182-433">Dane programu Exchange między przeglądarką i Visual Studio za pomocą łączność z przeglądarkami</span><span class="sxs-lookup"><span data-stu-id="c8182-433">Exchange data between your browser and Visual Studio using Browser Link</span></span>
