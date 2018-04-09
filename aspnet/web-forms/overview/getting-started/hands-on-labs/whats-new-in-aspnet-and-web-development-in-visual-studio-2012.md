---
uid: web-forms/overview/getting-started/hands-on-labs/whats-new-in-aspnet-and-web-development-in-visual-studio-2012
title: What's New in ASP.NET i aplikacji sieci Web w programie Visual Studio 2012 | Dokumentacja firmy Microsoft
author: rick-anderson
description: Nowa wersja programu Visual Studio wprowadzono wiele ulepszeń skoncentrowane na ułatwieniu środowisko i wydajności podczas pracy z technologii sieci Web...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/18/2013
ms.topic: article
ms.assetid: 6d40d276-1642-4a77-b6c9-02ac914f6805
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/getting-started/hands-on-labs/whats-new-in-aspnet-and-web-development-in-visual-studio-2012
msc.type: authoredcontent
ms.openlocfilehash: 00b43cc548df44edded925521991a095ed856494
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/06/2018
---
<a name="whats-new-in-aspnet-and-web-development-in-visual-studio-2012"></a><span data-ttu-id="dd1f0-103">What's New in ASP.NET i aplikacji sieci Web w programie Visual Studio 2012</span><span class="sxs-lookup"><span data-stu-id="dd1f0-103">What's New in ASP.NET and Web Development in Visual Studio 2012</span></span>
====================
<span data-ttu-id="dd1f0-104">Przez [obozów sieci Web Team](https://twitter.com/webcamps)</span><span class="sxs-lookup"><span data-stu-id="dd1f0-104">by [Web Camps Team](https://twitter.com/webcamps)</span></span>

> <span data-ttu-id="dd1f0-105">Nowa wersja programu Visual Studio wprowadzono wiele ulepszeń skoncentrowane na ułatwieniu środowisko i wydajności podczas pracy z technologii sieci Web.</span><span class="sxs-lookup"><span data-stu-id="dd1f0-105">The new version of Visual Studio introduces a number of enhancements focused on improving the experience and performance when working with Web technologies.</span></span> <span data-ttu-id="dd1f0-106">Visual Studio edytory CSS, JavaScript i HTML ma została utworzona całkowicie od nowa do obejmuje wiele najbardziej w żądanie pomocy kodu, takie jak IntelliSense i automatyczne wcięcia.</span><span class="sxs-lookup"><span data-stu-id="dd1f0-106">Visual Studio Editors for CSS, JavaScript and HTML have been completely revamped to include many of the most in-demand code aids, such as IntelliSense and automatic indentation.</span></span> <span data-ttu-id="dd1f0-107">Dotyczące wydajności tworzenie pakietów i minimalizowanie są teraz zintegrowane jak czas ładowania wbudowanych funkcji, aby łatwo ograniczyć strony.</span><span class="sxs-lookup"><span data-stu-id="dd1f0-107">Regarding performance, bundling and minification are now integrated as built-in features to easily reduce page load time.</span></span>
> 
> <span data-ttu-id="dd1f0-108">Program Visual Studio umożliwia pracę z najnowszych technologii witryny sieci Web.</span><span class="sxs-lookup"><span data-stu-id="dd1f0-108">Visual Studio enables you to work with the latest website technologies.</span></span> <span data-ttu-id="dd1f0-109">Fragmenty kodu CSS3 różnych przeglądarkach umożliwia upewnij się, że witryna działa bez względu na platformę klienta również wykorzystując nowe elementy HTML5 i funkcje.</span><span class="sxs-lookup"><span data-stu-id="dd1f0-109">You can use cross-browser CSS3 Snippets to make sure your site works regardless of the client platform while also taking advantage of the new HTML5 elements and features.</span></span>
> 
> <span data-ttu-id="dd1f0-110">Zapisywanie i profilowanie kodu JavaScript powinny być łatwiejsze w przypadku tej wersji programu Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="dd1f0-110">Writing and profiling JavaScript code should be easier with this Visual Studio version.</span></span> <span data-ttu-id="dd1f0-111">Listy IntelliSense, zintegrowane funkcje dokumentacji i nawigacja XML są teraz dostępne dla kodu JavaScript.</span><span class="sxs-lookup"><span data-stu-id="dd1f0-111">IntelliSense lists, integrated XML documentation and navigation features are now available for JavaScript code.</span></span> <span data-ttu-id="dd1f0-112">Masz teraz katalogu JavaScript w zasięgu ręki.</span><span class="sxs-lookup"><span data-stu-id="dd1f0-112">You now have the JavaScript catalog at your fingertips.</span></span> <span data-ttu-id="dd1f0-113">Ponadto można sprawdzić zgodność ECMAScript5 za pomocą tych skryptów i wykrywania błędów składni na wczesnym etapie.</span><span class="sxs-lookup"><span data-stu-id="dd1f0-113">Additionally, you can check ECMAScript5 compliance with your scripts and detect syntax errors at an early stage.</span></span>
> 
> <span data-ttu-id="dd1f0-114">Ostatnio, ale nie najmniej tej wersji programu Visual Studio implementuje wbudowanych tworzenie pakietów i minimalizowanie.</span><span class="sxs-lookup"><span data-stu-id="dd1f0-114">Last but not least, this Visual Studio version implements built-in bundling and minification.</span></span> <span data-ttu-id="dd1f0-115">Pliki skryptów i arkusze stylów zostanie spakowane i skompresować tak, aby lokacji wykonuje szybciej.</span><span class="sxs-lookup"><span data-stu-id="dd1f0-115">Your script files and style sheets will be packed and compressed so that the site performs faster.</span></span>
> 
> <span data-ttu-id="dd1f0-116">W tym laboratorium przeprowadzi Cię przez rozszerzenia oraz nowe funkcje opisane wcześniej przez zastosowanie drobne zmiany do przykładowej aplikacji sieci Web w folderze źródłowym.</span><span class="sxs-lookup"><span data-stu-id="dd1f0-116">This lab walks you through the enhancements and new features previously described by applying minor changes to a sample Web application provided in the Source folder.</span></span>
> 
> <span data-ttu-id="dd1f0-117">Wszystkie przykładowy kod i fragmenty kodu są uwzględnione w sieci Web obozów zestaw szkoleniowy, dostępne pod adresem [ https://go.microsoft.com/fwlink/?LinkID=248297&clcid=0x409 ](https://go.microsoft.com/fwlink/?LinkID=248297&clcid=0x409).</span><span class="sxs-lookup"><span data-stu-id="dd1f0-117">All sample code and snippets are included in the Web Camps Training Kit, available at [https://go.microsoft.com/fwlink/?LinkID=248297&clcid=0x409](https://go.microsoft.com/fwlink/?LinkID=248297&clcid=0x409).</span></span>


<a id="Objectives"></a>

<a id="Objectives"></a>
### <a name="objectives"></a><span data-ttu-id="dd1f0-118">Cele</span><span class="sxs-lookup"><span data-stu-id="dd1f0-118">Objectives</span></span>

<span data-ttu-id="dd1f0-119">W tym ręce w laboratorium dowiesz się jak:</span><span class="sxs-lookup"><span data-stu-id="dd1f0-119">In this hands on lab, you will learn how to:</span></span>

- <span data-ttu-id="dd1f0-120">Użyj nowe funkcje i ulepszenia w edytorze CSS</span><span class="sxs-lookup"><span data-stu-id="dd1f0-120">Use the new features and improvements in the CSS editor</span></span>
- <span data-ttu-id="dd1f0-121">Użyj nowe funkcje i ulepszenia w edytorze HTML</span><span class="sxs-lookup"><span data-stu-id="dd1f0-121">Use the new features and improvements in the HTML editor</span></span>
- <span data-ttu-id="dd1f0-122">Użyj nowe funkcje i ulepszenia w edytorze kodu JavaScript</span><span class="sxs-lookup"><span data-stu-id="dd1f0-122">Use the new features and improvements in the JavaScript editor</span></span>
- <span data-ttu-id="dd1f0-123">Konfigurowanie i używanie tworzenie pakietów i minimalizowanie</span><span class="sxs-lookup"><span data-stu-id="dd1f0-123">Configure and use bundling and minification</span></span>

<a id="Prerequisites"></a>

<a id="Prerequisites"></a>
### <a name="prerequisites"></a><span data-ttu-id="dd1f0-124">Wymagania wstępne</span><span class="sxs-lookup"><span data-stu-id="dd1f0-124">Prerequisites</span></span>

- <span data-ttu-id="dd1f0-125">[Microsoft Visual Studio Express 2012 for Web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web) lub wyższego poziomu (odczytu [dodatek a.](#AppendixA) instrukcje dotyczące sposobu jego instalacji).</span><span class="sxs-lookup"><span data-stu-id="dd1f0-125">[Microsoft Visual Studio Express 2012 for Web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web) or superior (read [Appendix A](#AppendixA) for instructions on how to install it).</span></span>
- <span data-ttu-id="dd1f0-126">[Środowisko Windows PowerShell](https://support.microsoft.com/kb/968930/) (w przypadku skryptów instalacji — już zainstalowanym systemem Windows 8 i Windows Server 2008 R2)</span><span class="sxs-lookup"><span data-stu-id="dd1f0-126">[Windows PowerShell](https://support.microsoft.com/kb/968930/) (for setup scripts - already installed on Windows 8 and Windows Server 2008 R2)</span></span>
- <span data-ttu-id="dd1f0-127">[Internet Explorer 10](https://windows.microsoft.com/internet-explorer/products/ie/home) - lub HTML5 przeglądarki zgodne</span><span class="sxs-lookup"><span data-stu-id="dd1f0-127">[Internet Explorer 10](https://windows.microsoft.com/internet-explorer/products/ie/home) - or an HTML5 compliant browser</span></span>

<a id="Exercises"></a>

<a id="Exercises"></a>
## <a name="exercises"></a><span data-ttu-id="dd1f0-128">Ćwiczenia</span><span class="sxs-lookup"><span data-stu-id="dd1f0-128">Exercises</span></span>

<span data-ttu-id="dd1f0-129">Wskazówki tego laboratorium obejmuje następujących czynnościach:</span><span class="sxs-lookup"><span data-stu-id="dd1f0-129">This hands on lab includes the following exercises:</span></span>

1. [<span data-ttu-id="dd1f0-130">Ćwiczenie 1: Nowości w edytorze CSS</span><span class="sxs-lookup"><span data-stu-id="dd1f0-130">Exercise 1: What's New in the CSS Editor</span></span>](#Exercise1)
2. [<span data-ttu-id="dd1f0-131">Ćwiczenie 2: Nowości w edytorze HTML</span><span class="sxs-lookup"><span data-stu-id="dd1f0-131">Exercise 2: What's New in the HTML Editor</span></span>](#Exercise2)
3. [<span data-ttu-id="dd1f0-132">Ćwiczenie 3: Nowości w edytorze kodu JavaScript</span><span class="sxs-lookup"><span data-stu-id="dd1f0-132">Exercise 3: What's New in the JavaScript Editor</span></span>](#Exercise3)
4. [<span data-ttu-id="dd1f0-133">Ćwiczenie 4: Tworzenie pakietów i minimalizowanie</span><span class="sxs-lookup"><span data-stu-id="dd1f0-133">Exercise 4: Bundling and Minification</span></span>](#Exercise4)

<span data-ttu-id="dd1f0-134">Szacowany czas trwania tego laboratorium: **60 minut**.</span><span class="sxs-lookup"><span data-stu-id="dd1f0-134">Estimated time to complete this lab: **60 minutes**.</span></span>

<a id="Exercise1"></a>

<a id="Exercise_1_Whats_New_in_the_CSS_Editor"></a>
### <a name="exercise-1-whats-new-in-the-css-editor"></a><span data-ttu-id="dd1f0-135">Ćwiczenie 1: Nowości w edytorze CSS</span><span class="sxs-lookup"><span data-stu-id="dd1f0-135">Exercise 1: What's New in the CSS Editor</span></span>

<span data-ttu-id="dd1f0-136">Deweloperzy sieci Web należy zapoznać się z wielu trudności związane z CSS edycji.</span><span class="sxs-lookup"><span data-stu-id="dd1f0-136">Web developers should be familiar with many of the difficulties related to CSS editing.</span></span> <span data-ttu-id="dd1f0-137">Jedną z największych problemy style CSS jest pomiędzy przeglądarkami.</span><span class="sxs-lookup"><span data-stu-id="dd1f0-137">One of the biggest issues of CSS styling is cross-browser compatibility.</span></span> <span data-ttu-id="dd1f0-138">Często zdarza się, że po zastosowaniu stylów do swojej witryny, można zauważyć, że wygląda inaczej po otwarciu w innej przeglądarki lub urządzenia.</span><span class="sxs-lookup"><span data-stu-id="dd1f0-138">It often happens that, after applying styles to your site, you notice that it looks different if you open it in another browser or device.</span></span> <span data-ttu-id="dd1f0-139">W związku z tym może poświęcać przez dłuższy czas na rozwiązywania tych problemów visual sobie sprawę, że koniec powoduje ona działać w jednej przeglądarki, jest on uszkodzony na innych.</span><span class="sxs-lookup"><span data-stu-id="dd1f0-139">Therefore, you may spend a considerable time on fixing those visual issues to realize that, when you finally make it work in one browser, it is broken in the others.</span></span>

<span data-ttu-id="dd1f0-140">Visual Studio teraz obejmuje funkcje, które ułatwiają deweloperom dostęp, praca i efektywnie zorganizować arkusze stylów CSS.</span><span class="sxs-lookup"><span data-stu-id="dd1f0-140">Visual Studio now includes features that help developers access, work and organize CSS style sheets effectively.</span></span> <span data-ttu-id="dd1f0-141">W ramach tego ćwiczenia będzie spełniać nowych funkcji dla wersji i skuteczne organizacji, a także fragmenty kodu CSS3 zgodności różnych przeglądarkach.</span><span class="sxs-lookup"><span data-stu-id="dd1f0-141">Throughout this exercise, you will meet the new features for an effective organization and edition, as well as the CSS3 Code Snippets for cross-browser compatibility.</span></span>

<a id="Ex1Task1"></a>

<a id="Task_1_-_New_Editor_Features"></a>
#### <a name="task-1---new-editor-features"></a><span data-ttu-id="dd1f0-142">Zadanie 1 — nowe funkcje edytora</span><span class="sxs-lookup"><span data-stu-id="dd1f0-142">Task 1 - New Editor Features</span></span>

<span data-ttu-id="dd1f0-143">W tym zadaniu możesz zauważyć nowe funkcje Edytor CSS.</span><span class="sxs-lookup"><span data-stu-id="dd1f0-143">In this task, you will discover the new features of the CSS Editor.</span></span> <span data-ttu-id="dd1f0-144">Ten nowy edytor pomoże Ci zwiększyć produktywność dzięki wykorzystaniu nowych inteligentne wcięcie, komentarze w kodzie ulepszone i rozszerzone listy IntelliSense.</span><span class="sxs-lookup"><span data-stu-id="dd1f0-144">This new editor will help you increase your productivity by taking advantage of the new smart indentation, the improved code comments and the enhanced IntelliSense list.</span></span>

1. <span data-ttu-id="dd1f0-145">Uruchom **programu Visual Studio** , a następnie otwórz **WhatsNewASPNET.sln** rozwiązania, znajdujących się w **Source\WhatsNewASPNET** folder tego laboratorium.</span><span class="sxs-lookup"><span data-stu-id="dd1f0-145">Start **Visual Studio** and open the **WhatsNewASPNET.sln** solution located in the **Source\WhatsNewASPNET** folder of this lab.</span></span>
2. <span data-ttu-id="dd1f0-146">W Eksploratorze rozwiązań Otwórz **Site.css** plik znajdujący się w obszarze **style** folderu.</span><span class="sxs-lookup"><span data-stu-id="dd1f0-146">In Solution Explorer, open the **Site.css** file located under the **Styles** folder.</span></span> <span data-ttu-id="dd1f0-147">Upewnij się, że **Edytor tekstu** narzędzia są widoczne na pasku narzędzi.</span><span class="sxs-lookup"><span data-stu-id="dd1f0-147">Make sure the **Text Editor** tools are visible on the toolbar.</span></span> <span data-ttu-id="dd1f0-148">W tym celu wybierz **widoku** | **paski narzędzi** opcji menu, a także sprawdź **Edytor tekstu** opcje.</span><span class="sxs-lookup"><span data-stu-id="dd1f0-148">To do that, select the **View** | **Toolbars** menu option, and check the **Text Editor** options.</span></span> <span data-ttu-id="dd1f0-149">Można zauważyć, że od tej nowej wersji **komentarz** przycisk (![przycisk komentarz](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image1.png) ) i **Usuń komentarz** przycisk (![przycisk Usuń znaczniki komentarza](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image2.png)) również są włączone dla Edytor CSS.</span><span class="sxs-lookup"><span data-stu-id="dd1f0-149">You will notice that, since this new version, the **Comment** button (![comment-button](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image1.png) ) and the **Uncomment** button (![uncomment-button](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image2.png) ) are also enabled for the CSS editor.</span></span>

    <span data-ttu-id="dd1f0-150">![Włączanie edytora i narzędzia CSS](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image3.png "włączenie edytora i narzędzia CSS")</span><span class="sxs-lookup"><span data-stu-id="dd1f0-150">![Enabling Editor and CSS Tools](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image3.png "Enabling Editor and CSS Tools")</span></span>

    <span data-ttu-id="dd1f0-151">*Włączanie edytora i narzędzia CSS*</span><span class="sxs-lookup"><span data-stu-id="dd1f0-151">*Enabling Editor and CSS Tools*</span></span>
3. <span data-ttu-id="dd1f0-152">Przewiń w kodzie i wybierz żadnych definicji klas CSS.</span><span class="sxs-lookup"><span data-stu-id="dd1f0-152">Scroll the code and select any CSS class definition.</span></span> <span data-ttu-id="dd1f0-153">Kliknij przycisk **komentarz** (![przycisk komentarz](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image4.png) ) przycisk, aby komentarz dotyczący wybranych wierszy.</span><span class="sxs-lookup"><span data-stu-id="dd1f0-153">Click the **Comment** (![comment-button](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image4.png) ) button to comment the selected lines.</span></span> <span data-ttu-id="dd1f0-154">Następnie kliknij przycisk **Usuń komentarz** (![przycisk Usuń znaczniki komentarza](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image5.png) ) przycisk, aby cofnąć zmiany.</span><span class="sxs-lookup"><span data-stu-id="dd1f0-154">Then, click the **Uncomment** (![uncomment-button](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image5.png) ) button to undo the changes.</span></span>
4. <span data-ttu-id="dd1f0-155">Kliknij przycisk **Zwiń** (![Zwiń](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image6.png) ) i **rozwiń** (![rozwiń](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image7.png) ) przycisków znajdujących się na lewym marginesie tekstu.</span><span class="sxs-lookup"><span data-stu-id="dd1f0-155">Click the **Collapse** (![collapse](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image6.png) ) and **Expand** (![expand](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image7.png) ) buttons located on the left margin of the text.</span></span> <span data-ttu-id="dd1f0-156">Należy zauważyć, że teraz można ukryć style, które nie jest używany widok oczyszczarki.</span><span class="sxs-lookup"><span data-stu-id="dd1f0-156">Notice that you can now hide the styles you don't use to have a cleaner view.</span></span>

    <span data-ttu-id="dd1f0-157">![Zwijanie klas CSS](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image8.png "klas CSS zwijanie")</span><span class="sxs-lookup"><span data-stu-id="dd1f0-157">![Collapsing CSS classes](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image8.png "Collapsing CSS classes")</span></span>

    <span data-ttu-id="dd1f0-158">*Zwijanie klas CSS*</span><span class="sxs-lookup"><span data-stu-id="dd1f0-158">*Collapsing CSS classes*</span></span>
5. <span data-ttu-id="dd1f0-159">Upewnij się, że włączona jest funkcja inteligentnego wcięcia.</span><span class="sxs-lookup"><span data-stu-id="dd1f0-159">Make sure that the smart indentation feature is enabled.</span></span> <span data-ttu-id="dd1f0-160">Wybierz **narzędzia** | **opcje** opcji menu, a następnie wybierz **Edytor tekstu** | **CSS**  |  **Formatowanie** strony w okienku po lewej stronie ekranu.</span><span class="sxs-lookup"><span data-stu-id="dd1f0-160">Select the **Tools** | **Options** menu option, and then select the **Text Editor** | **CSS** | **Formatting** page in the left pane of the screen.</span></span> <span data-ttu-id="dd1f0-161">Sprawdź **hierarchiczna wcięcia** opcji.</span><span class="sxs-lookup"><span data-stu-id="dd1f0-161">Check the **Hierarchical indentation** option.</span></span>

    <span data-ttu-id="dd1f0-162">![Włączanie hierarchiczna wcięcia](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image9.png "włączenie wcięcia hierarchiczne")</span><span class="sxs-lookup"><span data-stu-id="dd1f0-162">![Enabling hierarchical indentation](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image9.png "Enabling hierarchical indentation")</span></span>

    <span data-ttu-id="dd1f0-163">*Włączanie wcięcia hierarchiczne*</span><span class="sxs-lookup"><span data-stu-id="dd1f0-163">*Enabling hierarchical indentation*</span></span>
6. <span data-ttu-id="dd1f0-164">Znajdź definicji klasy głównym (.main) i Dołącz stylu do elementów div.</span><span class="sxs-lookup"><span data-stu-id="dd1f0-164">Locate the main class definition (.main) and append a style to the div elements.</span></span> <span data-ttu-id="dd1f0-165">Można zauważyć kod wyrównuje automatycznie, myśl użytkowników można znaleźć klasy nadrzędnej jeden rzut oka.</span><span class="sxs-lookup"><span data-stu-id="dd1f0-165">You will notice that the code aligns automatically, helping users to find the parent classes at a glance.</span></span>

    <span data-ttu-id="dd1f0-166">CSS</span><span class="sxs-lookup"><span data-stu-id="dd1f0-166">CSS</span></span>

    [!code-css[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample1.css)]

    <span data-ttu-id="dd1f0-167">![Hierarchiczna wyrównania w kodzie CSS](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image10.png "hierarchiczna wyrównania w kodzie CSS")</span><span class="sxs-lookup"><span data-stu-id="dd1f0-167">![Hierarchical alignment in CSS](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image10.png "Hierarchical alignment in CSS")</span></span>

    <span data-ttu-id="dd1f0-168">*Hierarchiczna wyrównania w kodzie CSS*</span><span class="sxs-lookup"><span data-stu-id="dd1f0-168">*Hierarchical alignment in CSS*</span></span>
7. <span data-ttu-id="dd1f0-169">Wewnątrz **.main div** klasy, Znajdź kursor na końcu **obramowania: 0px;** i naciśnij klawisz **Enter** do wyświetlenia na liście IntelliSense.</span><span class="sxs-lookup"><span data-stu-id="dd1f0-169">Inside **.main div** class, locate the cursor at the end of **border: 0px;** and press **Enter** to display the IntelliSense list.</span></span> <span data-ttu-id="dd1f0-170">Zacznij pisać **górnej** i zwróć uwagę, jak lista jest przefiltrowana podczas pisania.</span><span class="sxs-lookup"><span data-stu-id="dd1f0-170">Start typing **top** and notice how the list is filtered as you type.</span></span> <span data-ttu-id="dd1f0-171">Lista będzie zawierać elementów, które zawierają **górnej** w jakimkolwiek Word (w poprzednich wersjach programu Visual Studio, listy są filtrowane według elementy który *rozpocząć* z terminem).</span><span class="sxs-lookup"><span data-stu-id="dd1f0-171">The list will display the elements that contain **top** at any part of the word (In prior versions of Visual Studio, the list is filtered by the items that *begin* with the term).</span></span>

    <span data-ttu-id="dd1f0-172">![Ulepszenia IntelliSense w kodzie CSS](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image11.png "ulepszeń funkcji IntelliSense w kodzie CSS")</span><span class="sxs-lookup"><span data-stu-id="dd1f0-172">![IntelliSense enhancements in CSS](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image11.png "IntelliSense enhancements in CSS")</span></span>

    <span data-ttu-id="dd1f0-173">*Ulepszenia IntelliSense w kodzie CSS*</span><span class="sxs-lookup"><span data-stu-id="dd1f0-173">*IntelliSense enhancements in CSS*</span></span>

<a id="Ex1Task2"></a>

<a id="Task_2_-_The_Color_Picker"></a>
#### <a name="task-2---the-color-picker"></a><span data-ttu-id="dd1f0-174">Zadanie 2 — próbnika kolorów</span><span class="sxs-lookup"><span data-stu-id="dd1f0-174">Task 2 - The Color Picker</span></span>

<span data-ttu-id="dd1f0-175">W tym zadaniu zostanie wykryty próbnika kolorów CSS włączona w programie Visual Studio IntelliSense.</span><span class="sxs-lookup"><span data-stu-id="dd1f0-175">In this task, you will discover the new CSS Color Picker integrated into Visual Studio IntelliSense.</span></span>

1. <span data-ttu-id="dd1f0-176">W **Site.css,** zlokalizować definicji klasy nagłówka (.header), umieść kursor w polu **kolor tła** atrybutu między &quot;:&quot; i &quot; # &quot; znaków w tym wierszu kodu **.**</span><span class="sxs-lookup"><span data-stu-id="dd1f0-176">In **Site.css,** locate the header class definition (.header) and place the cursor next to **background-color** attribute, between the &quot;:&quot; and &quot;#&quot; characters on that line of code **.**</span></span>

    <span data-ttu-id="dd1f0-177">![Lokalizowanie kursor](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image12.png "lokalizowanie kursora")</span><span class="sxs-lookup"><span data-stu-id="dd1f0-177">![Locating the cursor](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image12.png "Locating the cursor")</span></span>

    <span data-ttu-id="dd1f0-178">*Lokalizowanie kursora*</span><span class="sxs-lookup"><span data-stu-id="dd1f0-178">*Locating the cursor*</span></span>
2. <span data-ttu-id="dd1f0-179">Usuń **dwukropek** (:) i zapisać go ponownie, aby wyświetlić próbnika kolorów.</span><span class="sxs-lookup"><span data-stu-id="dd1f0-179">Delete the **colon** (:) and write it again to display the color picker.</span></span> <span data-ttu-id="dd1f0-180">Zwróć uwagę, czy pierwszy kolorów, które będą wyświetlane są najczęściej kolory witryny.</span><span class="sxs-lookup"><span data-stu-id="dd1f0-180">Notice that the first colors you will see are the most frequent colors of your site.</span></span> <span data-ttu-id="dd1f0-181">Jeśli klikniesz przycisk kolor biały, kod HTML (#fff) spowoduje zastąpienie bieżącego kodu kolorów w arkuszu stylów.</span><span class="sxs-lookup"><span data-stu-id="dd1f0-181">If you click the white color, its HTML color code (#fff) will replace the current color code in the stylesheet.</span></span>

    <span data-ttu-id="dd1f0-182">![Selektor kolorów](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image13.png "próbnika kolorów")</span><span class="sxs-lookup"><span data-stu-id="dd1f0-182">![Color picker](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image13.png "Color picker")</span></span>

    <span data-ttu-id="dd1f0-183">*Selektor kolorów*</span><span class="sxs-lookup"><span data-stu-id="dd1f0-183">*Color picker*</span></span>
3. <span data-ttu-id="dd1f0-184">Naciśnij klawisz **rozwiń** (![com](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image14.png) ) przycisk na próbnika kolorów, aby wyświetlić kolor gradientu, a następnie przeciągnij kursor gradientu, aby wybrać inny kolor.</span><span class="sxs-lookup"><span data-stu-id="dd1f0-184">Press the **Expand** (![com](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image14.png) ) button on the color picker to display the color gradient, and then drag the gradient cursor to select a different color.</span></span> <span data-ttu-id="dd1f0-185">Następnie kliknij przycisk **Kroplomierz** i wybrać dowolny kolor na ekranie.</span><span class="sxs-lookup"><span data-stu-id="dd1f0-185">After that, click the **Eyedropper** button and select any color from the screen.</span></span> <span data-ttu-id="dd1f0-186">Zwróć uwagę, że wartość koloru tła zmienia dynamicznie, gdy kursor jest.</span><span class="sxs-lookup"><span data-stu-id="dd1f0-186">Notice that background color value changes dynamically while you move the cursor.</span></span>

    <span data-ttu-id="dd1f0-187">![Kolor gradientu selektora](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image15.png "gradientu próbnika kolorów")</span><span class="sxs-lookup"><span data-stu-id="dd1f0-187">![Color picker gradient](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image15.png "Color picker gradient")</span></span>

    <span data-ttu-id="dd1f0-188">*Kolor gradientu selektora*</span><span class="sxs-lookup"><span data-stu-id="dd1f0-188">*Color picker gradient*</span></span>
4. <span data-ttu-id="dd1f0-189">W **nieprzezroczystość** suwaka, przesuń selektor środek pasek, aby zmniejszyć przezroczystość.</span><span class="sxs-lookup"><span data-stu-id="dd1f0-189">In the **Opacity** slider, move the selector to the center of the bar to reduce the opacity.</span></span> <span data-ttu-id="dd1f0-190">Zwróć uwagę, że wartość koloru tła teraz zmienia jego skali do RGBA.</span><span class="sxs-lookup"><span data-stu-id="dd1f0-190">Notice that background-color value now changes its scale to RGBA.</span></span>

    <span data-ttu-id="dd1f0-191">![Selektor kolorów nieprzezroczystość](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image16.png "próbnika kolorów nieprzezroczystość.")</span><span class="sxs-lookup"><span data-stu-id="dd1f0-191">![Color picker Opacity](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image16.png "Color picker Opacity")</span></span>

    <span data-ttu-id="dd1f0-192">*Selektor kolorów nieprzezroczystość.*</span><span class="sxs-lookup"><span data-stu-id="dd1f0-192">*Color picker Opacity*</span></span>

    > [!NOTE]
    > <span data-ttu-id="dd1f0-193">Definicja kolor RGBA (czerwony, zielony, niebieski, alfa) w standardzie CSS3 umożliwia zdefiniowanie wartości Przezroczystość koloru dla pojedynczego elementu.</span><span class="sxs-lookup"><span data-stu-id="dd1f0-193">The RGBA (Red, Green, Blue, Alpha) color definition in CSS3 enables you to define the color opacity value for a single item.</span></span> <span data-ttu-id="dd1f0-194">W odróżnieniu od **nieprzezroczystość -** podobne atrybut CSS **-** kolory RGBA również są zgodne z najnowszych przeglądarkach.</span><span class="sxs-lookup"><span data-stu-id="dd1f0-194">Unlike **opacity -** a similar CSS attribute **-** RGBA colors are also compatible with the latest browsers.</span></span>

<a id="Ex1Task3"></a>

<a id="Task_3_-_CSS_Compatible_Code_Snippets"></a>
#### <a name="task-3---css-compatible-code-snippets"></a><span data-ttu-id="dd1f0-195">Zadanie 3 — wstawki kodu zgodne CSS</span><span class="sxs-lookup"><span data-stu-id="dd1f0-195">Task 3 - CSS Compatible Code Snippets</span></span>

<span data-ttu-id="dd1f0-196">W tym zadaniu dowiesz się, jak używać różnych przeglądarkach zgodne CSS3 fragmentów w celu wdrożenia niektóre funkcje w witrynie sieci Web.</span><span class="sxs-lookup"><span data-stu-id="dd1f0-196">In this task, you will learn how to use cross-browser compatible CSS3 snippets in order to implement some features in your website.</span></span>

1. <span data-ttu-id="dd1f0-197">W **Site.css** pliku, Znajdź **nagłówka** CSS klasy definicji (.header) i umieść kursor poniżej **/ \*radius obramowania\* /** symbolu zastępczego, aby dodać nowy fragment kodu.</span><span class="sxs-lookup"><span data-stu-id="dd1f0-197">In the **Site.css** file, locate the **header** CSS class definition (.header) and place the cursor below the **/\*border radius\*/** placeholder to add a new snippet.</span></span> <span data-ttu-id="dd1f0-198">Naciśnij klawisz **Enter** do wyświetlenia na liście IntelliSense i typ **radius** Aby filtrować listę.</span><span class="sxs-lookup"><span data-stu-id="dd1f0-198">Press **Enter** to display the IntelliSense list and type **radius** to filter the list.</span></span> <span data-ttu-id="dd1f0-199">Wybierz **border-radius** opcję z listy za pomocą podwójnego kliknięcia, a następnie naciśnij klawisz **kartę** klucz do wstawiania wstawki kodu.</span><span class="sxs-lookup"><span data-stu-id="dd1f0-199">Select the **border-radius** option from the list with a double click, and then press the **TAB** key to insert the snippet.</span></span> <span data-ttu-id="dd1f0-200">Następnie wpisz rozmiar radius w pikselach i naciśnij klawisz **Enter**.</span><span class="sxs-lookup"><span data-stu-id="dd1f0-200">Then, type a radius size in pixels and press **Enter**.</span></span> <span data-ttu-id="dd1f0-201">Na przykład wpisz **15px**.</span><span class="sxs-lookup"><span data-stu-id="dd1f0-201">For instance, type **15px**.</span></span>

    <span data-ttu-id="dd1f0-202">Atrybuty CSS3, dodawane przez fragment kodu spowoduje, że zaokrąglony obramowań w większości przeglądarek zgodności HTML5, w tym Mozilla i na podstawie WebKit przeglądarki.</span><span class="sxs-lookup"><span data-stu-id="dd1f0-202">The CSS3 attributes added by the snippet will render rounded borders in most HTML5 compliance browsers, including Mozilla and WebKit-based browsers.</span></span>

    <span data-ttu-id="dd1f0-203">![Przy użyciu fragment border-radius](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image17.png "przy użyciu fragment border-radius")</span><span class="sxs-lookup"><span data-stu-id="dd1f0-203">![Using a border-radius snippet](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image17.png "Using a border-radius snippet")</span></span>

    <span data-ttu-id="dd1f0-204">*Przy użyciu fragment border-radius*</span><span class="sxs-lookup"><span data-stu-id="dd1f0-204">*Using a border-radius snippet*</span></span>
2. <span data-ttu-id="dd1f0-205">Zastosować te same **obramowania** fragmentów w stylu strony (.page).</span><span class="sxs-lookup"><span data-stu-id="dd1f0-205">Apply the same **border** snippets in the page style (.page).</span></span>

    <span data-ttu-id="dd1f0-206">CSS</span><span class="sxs-lookup"><span data-stu-id="dd1f0-206">CSS</span></span>

    [!code-css[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample2.css)]
3. <span data-ttu-id="dd1f0-207">Naciśnij klawisz **F5** Aby uruchomić rozwiązanie.</span><span class="sxs-lookup"><span data-stu-id="dd1f0-207">Press **F5** to run the solution.</span></span> <span data-ttu-id="dd1f0-208">Zwróć uwagę, że każdej stronie teraz ma zostać zaokrąglona obramowań.</span><span class="sxs-lookup"><span data-stu-id="dd1f0-208">Notice that each page now has rounded borders.</span></span>

    <span data-ttu-id="dd1f0-209">![Zaokrąglone narożniki](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image18.png "zaokrąglone narożniki")</span><span class="sxs-lookup"><span data-stu-id="dd1f0-209">![Rounded corners](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image18.png "Rounded corners")</span></span>

    <span data-ttu-id="dd1f0-210">*Zaokrąglone narożniki*</span><span class="sxs-lookup"><span data-stu-id="dd1f0-210">*Rounded corners*</span></span>
4. <span data-ttu-id="dd1f0-211">Zamknij przeglądarkę i powrócić do programu Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="dd1f0-211">Close the browser and return to Visual Studio.</span></span>
5. <span data-ttu-id="dd1f0-212">Otwórz **Custome.CSS** plik znajdujący się w obszarze **style** folderu, umieść kursor w **div.images ul li img** definicji klasy.</span><span class="sxs-lookup"><span data-stu-id="dd1f0-212">Open the **Custom.css** file located under the **Styles** folder and place the cursor inside **div.images ul li img** class definition.</span></span>
6. <span data-ttu-id="dd1f0-213">Naciśnij klawisz enter, aby wyświetlić listę funkcji IntelliSense, typ **tle pole** i naciśnij klawisz **kartę** klucza dwa razy, aby wstawić fragment kodu tle domyślne wewnątrz definicji klasy.</span><span class="sxs-lookup"><span data-stu-id="dd1f0-213">Press enter to display the IntelliSense list, type **box-shadow** and press the **TAB** key twice to insert the default shadow code snippet inside the class definition.</span></span> <span data-ttu-id="dd1f0-214">Ustaw wartości cienia **10px 10px 5px #888**.</span><span class="sxs-lookup"><span data-stu-id="dd1f0-214">Set the shadow values to **10px 10px 5px #888**.</span></span> <span data-ttu-id="dd1f0-215">Następnie wpisz **border-radius** i wstawianie fragmentu kodu.</span><span class="sxs-lookup"><span data-stu-id="dd1f0-215">Then, type **border-radius** and insert the code snippet.</span></span> <span data-ttu-id="dd1f0-216">Typ **15px** ustawienie rozmiaru usługi radius i naciśnij klawisz **ENTER**.</span><span class="sxs-lookup"><span data-stu-id="dd1f0-216">Type **15px** to set radius size and press **ENTER**.</span></span>

    <span data-ttu-id="dd1f0-217">![Zaokrąglone narożniki z tle](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image19.png "zaokrąglone narożniki z cień")</span><span class="sxs-lookup"><span data-stu-id="dd1f0-217">![Rounded corners with shadow](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image19.png "Rounded corners with shadow")</span></span>

    <span data-ttu-id="dd1f0-218">*Zaokrąglone narożniki z cień*</span><span class="sxs-lookup"><span data-stu-id="dd1f0-218">*Rounded corners with shadow*</span></span>

    > [!NOTE]
    > <span data-ttu-id="dd1f0-219">W tej chwili atrybutu cienia są wstawiane z odpowiedniego prefiksu (moz, webkit, o) do obsługi Mozilla i przeglądarki Webkit (Chrome, Safari, Konkeror).</span><span class="sxs-lookup"><span data-stu-id="dd1f0-219">At this moment, the shadow attribute is inserted with the corresponding prefix (moz, webkit, o) to support Mozilla and Webkit (Chrome, Safari, Konkeror) browsers.</span></span>
7. <span data-ttu-id="dd1f0-220">Utwórz nową klasę **div.images ul li img:hover** poniżej **div.images ul li img** definicji klasy, umieść kursor w nawiasie **.**</span><span class="sxs-lookup"><span data-stu-id="dd1f0-220">Create a new class **div.images ul li img:hover** below the **div.images ul li img** class definition and place the cursor inside the brackets **.**</span></span>

    <span data-ttu-id="dd1f0-221">CSS</span><span class="sxs-lookup"><span data-stu-id="dd1f0-221">CSS</span></span>

    [!code-css[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample3.css)]
8. <span data-ttu-id="dd1f0-222">Typ **przekształcenie** i naciśnij klawisz **kartę** klucza dwa razy, aby wstawić fragment kodu transformacji.</span><span class="sxs-lookup"><span data-stu-id="dd1f0-222">Type **transform** and press the **TAB** key twice in order to insert the transform snippet.</span></span> <span data-ttu-id="dd1f0-223">Następnie wprowadź **rotate(-15deg)** Aby zmienić wartość kąta obrotu, gdy obrazy są aktywowany.</span><span class="sxs-lookup"><span data-stu-id="dd1f0-223">Then, enter **rotate(-15deg)** to change the rotation angle value when images are hovered.</span></span>

    <span data-ttu-id="dd1f0-224">CSS</span><span class="sxs-lookup"><span data-stu-id="dd1f0-224">CSS</span></span>

    [!code-css[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample4.css)]
9. <span data-ttu-id="dd1f0-225">Naciśnij klawisz **F5** uruchomić rozwiązanie, a następnie przejdź do strony CSS3.</span><span class="sxs-lookup"><span data-stu-id="dd1f0-225">Press **F5** to run the solution and browse to the CSS3 page.</span></span> <span data-ttu-id="dd1f0-226">Zauważyć, że obrazy mają zaokrąglone narożniki, a pole cieni.</span><span class="sxs-lookup"><span data-stu-id="dd1f0-226">Notice that the images have rounded corners and box shadows.</span></span> <span data-ttu-id="dd1f0-227">Umieść kursor myszy nad obrazów i oglądanie Obróć.</span><span class="sxs-lookup"><span data-stu-id="dd1f0-227">Hover the mouse over the images and watch them rotate.</span></span>

    <span data-ttu-id="dd1f0-228">![Przekształć fragment obracanie obrazów](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image20.png "fragment przekształcenia obracanie obrazów")</span><span class="sxs-lookup"><span data-stu-id="dd1f0-228">![Transform snippet rotating an image](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image20.png "Transform snippet rotating an image")</span></span>

    <span data-ttu-id="dd1f0-229">*Przekształć fragment obracanie obrazów*</span><span class="sxs-lookup"><span data-stu-id="dd1f0-229">*Transform snippet rotating an image*</span></span>

    > [!NOTE]
    > <span data-ttu-id="dd1f0-230">Jeśli używasz programu Internet Explorer 10 i cieni nie jest wyświetlane, upewnij się, że ustawienie trybu dokumentu Standardy programu IE10.</span><span class="sxs-lookup"><span data-stu-id="dd1f0-230">If you are using Internet Explorer 10 and cannot see the shadows, make sure the document mode is set to IE10 standards.</span></span> <span data-ttu-id="dd1f0-231">Naciśnij klawisz **F12** Otwórz narzędzi deweloperskich programu Internet Explorer, a następnie kliknij przycisk **tryb dokumentu** można zmienić na standardy programu IE10.</span><span class="sxs-lookup"><span data-stu-id="dd1f0-231">Press **F12** to open Internet Explorer developer tools and click **Document Mode** to change to IE10 standards.</span></span>

    ![o-us](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image21.png)

<a id="Exercise2"></a>

<a id="Exercise_2_Whats_New_in_the_HTML_Editor"></a>
### <a name="exercise-2-whats-new-in-the-html-editor"></a><span data-ttu-id="dd1f0-233">Ćwiczenie 2: Nowości w edytorze HTML</span><span class="sxs-lookup"><span data-stu-id="dd1f0-233">Exercise 2: What's New in the HTML Editor</span></span>

<span data-ttu-id="dd1f0-234">Visual Studio ma ulepszone edytora HTML.</span><span class="sxs-lookup"><span data-stu-id="dd1f0-234">Visual Studio has an improved HTML editor.</span></span> <span data-ttu-id="dd1f0-235">Oto niektóre ulepszenia uwzględnione w tej wersji są inteligentne wcięcie w dokumentach HTML, fragmenty kodu HTML5, HTML start i dopasowania tagu końcowego i weryfikacji HTML.</span><span class="sxs-lookup"><span data-stu-id="dd1f0-235">Some of the enhancements included in this version are smart indentation in HTML documents, HTML5 snippets, HTML start and end tag matching, and HTML validation.</span></span> <span data-ttu-id="dd1f0-236">W ramach tego ćwiczenia zobaczysz, jak te zmiany ulepszyć Twoje fluency podczas pracy w znaczniku witryny sieci Web.</span><span class="sxs-lookup"><span data-stu-id="dd1f0-236">Throughout this exercise, you will see how these changes improve your fluency when working in the website markup.</span></span>

<span data-ttu-id="dd1f0-237">Podobnie jak edytor CSS poprawiła się również edytora HTML.</span><span class="sxs-lookup"><span data-stu-id="dd1f0-237">Like the CSS editor, the HTML editor was also improved.</span></span> <span data-ttu-id="dd1f0-238">Większość z nich to małych sieci, które ułatwić życia deweloperów sieci Web.</span><span class="sxs-lookup"><span data-stu-id="dd1f0-238">Most of these improvements are small ones that make the Web developer's life easier.</span></span> <span data-ttu-id="dd1f0-239">Takie operacje, jak więcej wstawki kodu HTML5, inteligentne wcięcie dopasowania tagiem początkowym i końcowym podczas edycji i sprawdzania poprawności przeznaczonych dla dokumentu HTML DOCTYPE są niektóre z nich.</span><span class="sxs-lookup"><span data-stu-id="dd1f0-239">Things like more code snippets for HTML5, smart indentation, matching start and end tags when editing and validation targeting the HTML document DOCTYPE are some of these improvements.</span></span>

<a id="Ex2Task1"></a>

<a id="Task_1_-_Improved_DOCTYPE_Validation"></a>
#### <a name="task-1---improved-doctype-validation"></a><span data-ttu-id="dd1f0-240">Zadanie 1 - ulepszone DOCTYPE weryfikacji</span><span class="sxs-lookup"><span data-stu-id="dd1f0-240">Task 1 - Improved DOCTYPE Validation</span></span>

<span data-ttu-id="dd1f0-241">Edytora HTML ma teraz możliwość DOCTYPE strony, nawet jeśli definicja może być na stronie głównej.</span><span class="sxs-lookup"><span data-stu-id="dd1f0-241">The HTML editor now has the ability to check the DOCTYPE of your page, even though the definition might be in the master page.</span></span> <span data-ttu-id="dd1f0-242">W zależności od typu dokumentu, strony edytora HTML będzie weryfikowane z poprawny zestaw reguł i spowoduje odfiltrowanie listy IntelliSense, biorąc pod uwagę elementy DOCTYPE.</span><span class="sxs-lookup"><span data-stu-id="dd1f0-242">Depending on the DOCTYPE of your page, the HTML editor will validate with the correct set of rules and will filter the IntelliSense list considering the DOCTYPE elements.</span></span>

<span data-ttu-id="dd1f0-243">W tym zadaniu zostanie zmieniony DOCTYPE strony, aby zobaczyć, jak zachowanie edytora HTML zmienia się odpowiednio.</span><span class="sxs-lookup"><span data-stu-id="dd1f0-243">In this task, you will change the DOCTYPE of a page to see how the HTML editor behavior changes accordingly.</span></span>

1. <span data-ttu-id="dd1f0-244">Jeśli nie jest jeszcze otwarty, uruchom **programu Visual Studio** , a następnie otwórz **WhatsNewASPNET.sln** rozwiązania, znajdujących się w **Source\WhatsNewASPNET** folder tego laboratorium.</span><span class="sxs-lookup"><span data-stu-id="dd1f0-244">If not already opened, start **Visual Studio** and open the **WhatsNewASPNET.sln** solution located in the **Source\WhatsNewASPNET** folder of this lab.</span></span>
2. <span data-ttu-id="dd1f0-245">Otwórz **Site.Master** strony.</span><span class="sxs-lookup"><span data-stu-id="dd1f0-245">Open the **Site.Master** page.</span></span>
3. <span data-ttu-id="dd1f0-246">Zwróć uwagę schemat docelowy dla narzędzi do sprawdzania poprawności.</span><span class="sxs-lookup"><span data-stu-id="dd1f0-246">Notice the Target Schema for Validation Toolbar.</span></span> <span data-ttu-id="dd1f0-247">Dopasuj Doctype wybrane sposób zachowania edytora HTML (Sprawdzanie poprawności, IntelliSense, itp.) prawidłowo zostanie zmieniona.</span><span class="sxs-lookup"><span data-stu-id="dd1f0-247">The way the HTML editor behaves (Validation, IntelliSense, etc.) will properly change to fit the Doctype selected.</span></span>

    <span data-ttu-id="dd1f0-248">![Użyj narzędzi edycji HTML źródła Doctype](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image22.png "Doctype użyj narzędzi edycji źródła HTML")</span><span class="sxs-lookup"><span data-stu-id="dd1f0-248">![Use Doctype in HTML Source Editing toolbar](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image22.png "Use Doctype in HTML Source Editing toolbar")</span></span>

    <span data-ttu-id="dd1f0-249">*Użyj Doctype pakietu na pasku narzędzi*</span><span class="sxs-lookup"><span data-stu-id="dd1f0-249">*Use Doctype in HTML Source Editing toolbar*</span></span>
4. <span data-ttu-id="dd1f0-250">Zmień schemat docelowej w formacie HTML 4.01.</span><span class="sxs-lookup"><span data-stu-id="dd1f0-250">Change the Target Schema to HTML 4.01.</span></span>

    <span data-ttu-id="dd1f0-251">![Zmiana typu dokumentu pakietu narzędzi](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image23.png "zmiana typu dokumentu pakietu na pasku narzędzi")</span><span class="sxs-lookup"><span data-stu-id="dd1f0-251">![Changing Doctype in HTML Source Editing toolbar](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image23.png "Changing Doctype in HTML Source Editing toolbar")</span></span>

    <span data-ttu-id="dd1f0-252">*Zmiana typu dokumentu pakietu na pasku narzędzi*</span><span class="sxs-lookup"><span data-stu-id="dd1f0-252">*Changing Doctype in HTML Source Editing toolbar*</span></span>
5. <span data-ttu-id="dd1f0-253">Umieść kursor w obszarze **treści** elementu, a następnie wpisz nazwę elementu HTML5 (na przykład **wideo**).</span><span class="sxs-lookup"><span data-stu-id="dd1f0-253">Place the cursor under the **body** element, and start typing the name of an HTML5 element (for example, **video**).</span></span> <span data-ttu-id="dd1f0-254">Zwróć uwagę, że element nie jest dostępny na liście IntelliSense.</span><span class="sxs-lookup"><span data-stu-id="dd1f0-254">Notice that the element is not available in the IntelliSense list.</span></span>

    <span data-ttu-id="dd1f0-255">![Elementy HTML5 niewymienione](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image24.png "elementy HTML5 niewymienione")</span><span class="sxs-lookup"><span data-stu-id="dd1f0-255">![HTML5 elements not listed](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image24.png "HTML5 elements not listed")</span></span>

    <span data-ttu-id="dd1f0-256">*Elementy HTML5 niewymienione*</span><span class="sxs-lookup"><span data-stu-id="dd1f0-256">*HTML5 elements not listed*</span></span>
6. <span data-ttu-id="dd1f0-257">Cofnij zmiany schematu docelowej dla narzędzi sprawdzania poprawności pobrania DOCTYPE: XHTML5 z listy rozwijanej.</span><span class="sxs-lookup"><span data-stu-id="dd1f0-257">Undo the changes to the Target Schema for Validation Toolbar, picking DOCTYPE: XHTML5 from the dropdown list.</span></span>

    <span data-ttu-id="dd1f0-258">![Użyj narzędzi edycji HTML źródła Doctype](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image25.png "Doctype użyj narzędzi edycji źródła HTML")</span><span class="sxs-lookup"><span data-stu-id="dd1f0-258">![Use Doctype in HTML Source Editing toolbar](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image25.png "Use Doctype in HTML Source Editing toolbar")</span></span>

    <span data-ttu-id="dd1f0-259">*Resetowanie Doctype w narzędzi edycji źródła HTML*</span><span class="sxs-lookup"><span data-stu-id="dd1f0-259">*Reset Doctype in HTML Source Editing toolbar*</span></span>
7. <span data-ttu-id="dd1f0-260">Umieść kursor w obszarze **treści** element i wpisz HTML5 element ponownie (na przykład, takich jak **wideo**).</span><span class="sxs-lookup"><span data-stu-id="dd1f0-260">Place the cursor under the **body** element and start typing an HTML5 element again (For example, like **video**).</span></span> <span data-ttu-id="dd1f0-261">Zwróć uwagę, elementy HTML5 są teraz dostępne na liście IntelliSense.</span><span class="sxs-lookup"><span data-stu-id="dd1f0-261">Notice that the HTML5 elements are now available in the IntelliSense list.</span></span>

    <span data-ttu-id="dd1f0-262">![Elementy HTML5 są wymienione](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image26.png "HTML5 elementy są wymienione")</span><span class="sxs-lookup"><span data-stu-id="dd1f0-262">![HTML5 elements being listed](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image26.png "HTML5 elements being listed")</span></span>

    <span data-ttu-id="dd1f0-263">*Elementy HTML5 są wymienione*</span><span class="sxs-lookup"><span data-stu-id="dd1f0-263">*HTML5 elements being listed*</span></span>

<a id="Ex2Task2"></a>

<a id="Task_2_-_StartEnd_Tags_Automatic_Update"></a>
#### <a name="task-2---startend-tags-automatic-update"></a><span data-ttu-id="dd1f0-264">Zadanie 2 — rozpoczęcia i zakończenia znaczniki aktualizacji automatycznych</span><span class="sxs-lookup"><span data-stu-id="dd1f0-264">Task 2 - Start/End Tags Automatic Update</span></span>

<span data-ttu-id="dd1f0-265">Visual Studio teraz aktualizuje HTML otwierania lub zamykania znaczniki elementu, którą edytujesz, aby były zgodne ze sobą.</span><span class="sxs-lookup"><span data-stu-id="dd1f0-265">Visual Studio now updates the HTML opening or closing tags of the element that you are editing to match each other.</span></span> <span data-ttu-id="dd1f0-266">Ta nowa funkcja zwiększa produktywność podczas edytowania tagów HTML.</span><span class="sxs-lookup"><span data-stu-id="dd1f0-266">This new feature will improve your productivity when editing HTML tags.</span></span>

1. <span data-ttu-id="dd1f0-267">Na **Default.aspx** Dodaj **H3** element z tytułem (na przykład programu Visual Studio 2012 skały!).</span><span class="sxs-lookup"><span data-stu-id="dd1f0-267">On the **Default.aspx** page, add an **H3** element with a title (for example, Visual Studio 2012 Rocks!).</span></span>


~~~
[!code-aspx[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample5.aspx)]
~~~
2. <span data-ttu-id="dd1f0-268">Zmień **H3** tagu i typ **H2** lub **H1.**</span><span class="sxs-lookup"><span data-stu-id="dd1f0-268">Change the **H3** tag and type **H2** or **H1.**</span></span>

    <span data-ttu-id="dd1f0-269">Należy zauważyć, że tag końcowy automatycznie aktualizacji.</span><span class="sxs-lookup"><span data-stu-id="dd1f0-269">Notice that the end tag automatically updates.</span></span> <span data-ttu-id="dd1f0-270">Można również zmodyfikować tagu końcowego, aby zobaczyć, że tag początkowy odpowiednio aktualizowany za.</span><span class="sxs-lookup"><span data-stu-id="dd1f0-270">You can also modify the end tag to see that the start tag updates accordingly too.</span></span>

    <span data-ttu-id="dd1f0-271">![Automatyczna aktualizacja tagu końcowego](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image27.png "automatyczną aktualizację do tagu końcowego")</span><span class="sxs-lookup"><span data-stu-id="dd1f0-271">![Automatic update of the end tag](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image27.png "Automatic update of the end tag")</span></span>

    <span data-ttu-id="dd1f0-272">*Automatyczna aktualizacja tagu końcowego*</span><span class="sxs-lookup"><span data-stu-id="dd1f0-272">*Automatic update of the end tag*</span></span>

<a id="Ex2Task3"></a>

<a id="Task_3_-_New_HTML5_Code_Snippets"></a>
#### <a name="task-3---new-html5-code-snippets"></a><span data-ttu-id="dd1f0-273">Zadanie 3 - nowych fragmentów kodu HTML5</span><span class="sxs-lookup"><span data-stu-id="dd1f0-273">Task 3 - New HTML5 Code Snippets</span></span>

<span data-ttu-id="dd1f0-274">Visual Studio teraz obejmuje kilka fragmentów kodu HTML5.</span><span class="sxs-lookup"><span data-stu-id="dd1f0-274">Visual Studio now includes several HTML5 code snippets.</span></span> <span data-ttu-id="dd1f0-275">W tym zadaniu użyjesz niektóre z tych fragmentów.</span><span class="sxs-lookup"><span data-stu-id="dd1f0-275">In this task, you will use some of these snippets.</span></span>

1. <span data-ttu-id="dd1f0-276">Dodaj nowy folder o nazwie **audio** do głównego folderu witryny sieci web.</span><span class="sxs-lookup"><span data-stu-id="dd1f0-276">Add a new folder named **audio** to the root of the web site folder.</span></span> <span data-ttu-id="dd1f0-277">Otwórz Eksploratora Windows i skopiuj wszystkie plik dźwiękowy do **audio** folderu **WhatsNewASPNET.sln** rozwiązania.</span><span class="sxs-lookup"><span data-stu-id="dd1f0-277">Open Windows Explorer and copy any audio file into the **audio** folder of the **WhatsNewASPNET.sln** solution.</span></span>
2. <span data-ttu-id="dd1f0-278">W **Default.aspx** strony, odszukaj pozycję kursora w sieci Web 11 skały!!</span><span class="sxs-lookup"><span data-stu-id="dd1f0-278">In the **Default.aspx** page, locate the cursor under the Web11 Rocks!!</span></span> <span data-ttu-id="dd1f0-279">Nagłówek.</span><span class="sxs-lookup"><span data-stu-id="dd1f0-279">Header.</span></span> <span data-ttu-id="dd1f0-280">Typ **audio** i naciśnij klawisz TAB.</span><span class="sxs-lookup"><span data-stu-id="dd1f0-280">Type **audio** and press the TAB key.</span></span>

    <span data-ttu-id="dd1f0-281">Nowe edytora HTML zawiera wstawki kodu HTML5 zawartości.</span><span class="sxs-lookup"><span data-stu-id="dd1f0-281">The new HTML editor includes code snippets for HTML5 content.</span></span> <span data-ttu-id="dd1f0-282">Pamiętaj, aby Użyj właściwej definicji typu dokumentu, aby włączyć fragmenty kodu HTML5.</span><span class="sxs-lookup"><span data-stu-id="dd1f0-282">Remember to use the proper DOCTYPE definition to enable the HTML5 snippets.</span></span>

    <span data-ttu-id="dd1f0-283">![Wstawianie wstawek kodu HTML5](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image28.png "Wstawianie wstawek kodu HTML5")</span><span class="sxs-lookup"><span data-stu-id="dd1f0-283">![Inserting HTML5 Code Snippets](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image28.png "Inserting HTML5 Code Snippets")</span></span>

    <span data-ttu-id="dd1f0-284">*Wstawianie wstawek kodu HTML5*</span><span class="sxs-lookup"><span data-stu-id="dd1f0-284">*Inserting HTML5 Code Snippets*</span></span>
3. <span data-ttu-id="dd1f0-285">Aktualizowanie źródła audio, aby wskazywał istniejącego pliku audio.</span><span class="sxs-lookup"><span data-stu-id="dd1f0-285">Update the audio source to point to an existing audio file.</span></span>


~~~
[!code-aspx[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample6.aspx)]

> [!NOTE]
> You will need to add the audio file to the solution.
~~~
4. <span data-ttu-id="dd1f0-286">Naciśnij klawisz **F5** uruchamiania witryny i odtwarzanie dźwięku.</span><span class="sxs-lookup"><span data-stu-id="dd1f0-286">Press **F5** to run the site and play the audio.</span></span>

    <span data-ttu-id="dd1f0-287">![Uruchomienie formantu audio](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image29.png "systemem kontroli audio")</span><span class="sxs-lookup"><span data-stu-id="dd1f0-287">![Running the audio control](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image29.png "Running the audio control")</span></span>

    <span data-ttu-id="dd1f0-288">*Uruchomienie formantu audio*</span><span class="sxs-lookup"><span data-stu-id="dd1f0-288">*Running the audio control*</span></span>

    > [!NOTE]
    > <span data-ttu-id="dd1f0-289">Można też spróbować więcej wstawki uwzględnione w programie Visual Studio ilustracji wideo, np.</span><span class="sxs-lookup"><span data-stu-id="dd1f0-289">You can also try more snippets included in Visual Studio, such as video, figure, etc.</span></span>
5. <span data-ttu-id="dd1f0-290">Teraz spróbuj Wstawianie formantu w niektórych części strony.</span><span class="sxs-lookup"><span data-stu-id="dd1f0-290">Now, try to insert a control in some part of the page.</span></span> <span data-ttu-id="dd1f0-291">Na przykład próba wstawienia **widoku GridView** kontroli, ale zamiast wpisywać  **&lt;linie,** zacznij wpisywać ciąg  **&lt;GV**.</span><span class="sxs-lookup"><span data-stu-id="dd1f0-291">For example, try to insert a **GridView** control, but instead of typing **&lt;Gri,** start typing **&lt;GV**.</span></span> <span data-ttu-id="dd1f0-292">Należy zauważyć, że lista IntelliSense zawiera **asp: GridView** formantu.</span><span class="sxs-lookup"><span data-stu-id="dd1f0-292">Notice that the IntelliSense list shows the **asp:GridView** control.</span></span>

    <span data-ttu-id="dd1f0-293">IntelliSense w edytorze HTML udostępnia teraz wyszukiwania wielkości liter tytułu, a także częściowego dopasowania (pobieranie wszystkie elementy, które zawiera wyrażenie).</span><span class="sxs-lookup"><span data-stu-id="dd1f0-293">IntelliSense in the HTML Editor now provides title-casing search, as well as partial matching (retrieving all elements that contains the term).</span></span>

    <span data-ttu-id="dd1f0-294">![Wstawianie GridView z listami IntelliSense](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image30.png "Wstawianie GridView z listami IntelliSense")</span><span class="sxs-lookup"><span data-stu-id="dd1f0-294">![Inserting a GridView with IntelliSense lists](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image30.png "Inserting a GridView with IntelliSense lists")</span></span>

    <span data-ttu-id="dd1f0-295">*Wstawianie GridView z listami IntelliSense*</span><span class="sxs-lookup"><span data-stu-id="dd1f0-295">*Inserting a GridView with IntelliSense lists*</span></span>

    <span data-ttu-id="dd1f0-296">W przypadku wpisania  **&lt;siatki** otrzymasz wszystkich elementów, które odpowiadają termin, ale sugeruje Visual Studio **gridview** sterowania:</span><span class="sxs-lookup"><span data-stu-id="dd1f0-296">If you type **&lt;grid** you will get all the items that match the term, but Visual Studio will suggest the **gridview** control:</span></span>

    <span data-ttu-id="dd1f0-297">![Wstawianie Element GridView z listy IntelliSense i częściowe dopasowanie](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image31.png "Wstawianie Element GridView z listy IntelliSense i częściowe dopasowania")</span><span class="sxs-lookup"><span data-stu-id="dd1f0-297">![Inserting a GridView with IntelliSense lists and partial matching](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image31.png "Inserting a GridView with IntelliSense lists and partial matching")</span></span>

    <span data-ttu-id="dd1f0-298">*Wstawianie Element GridView z listy IntelliSense i częściowe dopasowania*</span><span class="sxs-lookup"><span data-stu-id="dd1f0-298">*Inserting a GridView with IntelliSense lists and partial matching*</span></span>

<a id="Ex2Task4"></a>

<a id="Task_4_-_HTML_Editor_Smart_Tags"></a>
#### <a name="task-4---html-editor-smart-tags"></a><span data-ttu-id="dd1f0-299">Zadanie 4 — tagów inteligentnych edytora HTML</span><span class="sxs-lookup"><span data-stu-id="dd1f0-299">Task 4 - HTML Editor Smart Tags</span></span>

<span data-ttu-id="dd1f0-300">Poprawa innego edytora HTML jest funkcji tagów inteligentnych.</span><span class="sxs-lookup"><span data-stu-id="dd1f0-300">Another improvement in the HTML Editor is the Smart Tags feature.</span></span> <span data-ttu-id="dd1f0-301">Tagi inteligentne ułatwiają do wykonywania zadań związanych z projektowaniem wspólnych lub powtarzających się na zasadzie-control.</span><span class="sxs-lookup"><span data-stu-id="dd1f0-301">Smart tags make it easy to perform common or repetitive development tasks on a per-control basis.</span></span> <span data-ttu-id="dd1f0-302">Ta funkcja została już dostępne w Projektancie HTML, ale nie w edytorze HTML.</span><span class="sxs-lookup"><span data-stu-id="dd1f0-302">This feature was already available in the HTML Designer, but not in the HTML Editor.</span></span>

1. <span data-ttu-id="dd1f0-303">Otwórz **Site.Master** i Znajdź **asp: Menu** elementu.</span><span class="sxs-lookup"><span data-stu-id="dd1f0-303">Open **Site.Master** and locate the **asp:Menu** element.</span></span> <span data-ttu-id="dd1f0-304">Umieść kursor w tagu początkowym i zwróć uwagę, czy małych symbolu, wyświetlane w dolnej części elementu — kliknij go, aby otworzyć menu panelu inteligentnego.</span><span class="sxs-lookup"><span data-stu-id="dd1f0-304">Place the cursor on the start tag and notice that the small glyph displayed at the bottom of the element - click it to open the smart tasks menu.</span></span> <span data-ttu-id="dd1f0-305">Zwróć uwagę, że masz dostęp do niektórych zadań związanych z Menu kontroli.</span><span class="sxs-lookup"><span data-stu-id="dd1f0-305">Notice that you have quick access to some tasks related to the Menu control.</span></span>

    <span data-ttu-id="dd1f0-306">![Inteligentne zadań kontrolki Menu](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image32.png "inteligentne zadań kontrolki Menu")</span><span class="sxs-lookup"><span data-stu-id="dd1f0-306">![Smart tasks for the Menu control](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image32.png "Smart tasks for the Menu control")</span></span>

    <span data-ttu-id="dd1f0-307">*Inteligentne zadań kontrolki Menu*</span><span class="sxs-lookup"><span data-stu-id="dd1f0-307">*Smart tasks for the Menu control*</span></span>

<a id="Ex2Task5"></a>

<a id="Task_5_-_Smart_Indentation"></a>
#### <a name="task-5---smart-indentation"></a><span data-ttu-id="dd1f0-308">Zadanie 5 - inteligentne wcięcie</span><span class="sxs-lookup"><span data-stu-id="dd1f0-308">Task 5 - Smart Indentation</span></span>

<span data-ttu-id="dd1f0-309">Jeden z najlepszych rozwiązań w formacie HTML jest wcięcia elementów zagnieżdżonych, aby zachować kod do odczytu.</span><span class="sxs-lookup"><span data-stu-id="dd1f0-309">One of the best practices in HTML is indenting the nested elements to keep the code readable.</span></span> <span data-ttu-id="dd1f0-310">W programie Visual Studio 2012 można zauważyć, że edytor automatycznie wcięcia elementów podczas pisania kodu.</span><span class="sxs-lookup"><span data-stu-id="dd1f0-310">In Visual Studio 2012, you will notice that the editor automatically indents the elements while you are writing the code.</span></span>

> [!NOTE]
> <span data-ttu-id="dd1f0-311">W poprzedniej wersji programu Visual Studio, inteligentne wcięcie była dostępna w edytorze XML, ale nie w edytorze HTML.</span><span class="sxs-lookup"><span data-stu-id="dd1f0-311">In previous version of Visual Studio, smart indentation was available in the XML editor but not in the HTML editor.</span></span>


1. <span data-ttu-id="dd1f0-312">Upewnij się, że konfiguracja Indenting w edytorze HTML ma ustawioną wartość inteligentne wcięcie.</span><span class="sxs-lookup"><span data-stu-id="dd1f0-312">Make sure that the Indenting configuration on the HTML Editor is set to Smart Indentation.</span></span> <span data-ttu-id="dd1f0-313">W tym celu wybierz **narzędzia | Opcje** , a następnie wybierz opcję menu **Edytor tekstu | HTML | Karty** strony w okienku po lewej stronie ekranu.</span><span class="sxs-lookup"><span data-stu-id="dd1f0-313">To do that, select the **Tools | Options** menu option and then select the **Text Editor | HTML | Tabs** page in the left pane of the screen.</span></span> <span data-ttu-id="dd1f0-314">Wybierz opcję inteligentne wcięcie.</span><span class="sxs-lookup"><span data-stu-id="dd1f0-314">Select the Smart indentation option.</span></span>

    <span data-ttu-id="dd1f0-315">![Ustawienia edytora HTML](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image33.png "ustawienia edytora HTML")</span><span class="sxs-lookup"><span data-stu-id="dd1f0-315">![HTML Editor settings](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image33.png "HTML Editor settings")</span></span>

    <span data-ttu-id="dd1f0-316">*Ustawienia edytora HTML*</span><span class="sxs-lookup"><span data-stu-id="dd1f0-316">*HTML Editor settings*</span></span>
2. <span data-ttu-id="dd1f0-317">Na **Default.aspx** pozycję Usuń całą zawartość w elemencie audio.</span><span class="sxs-lookup"><span data-stu-id="dd1f0-317">On the **Default.aspx** page, remove all the content under the audio element.</span></span>
3. <span data-ttu-id="dd1f0-318">Umieść kursor na końcu otwarcia **audio** element i naciśnij klawisz **ENTER**.</span><span class="sxs-lookup"><span data-stu-id="dd1f0-318">Place the cursor at the end of the opening **audio** element and hit **ENTER**.</span></span>

    <span data-ttu-id="dd1f0-319">Zwróć uwagę, że nowe położenie kursora ma poziom dodatkowego wcięcia.</span><span class="sxs-lookup"><span data-stu-id="dd1f0-319">Notice that the new position of cursor has an additional indentation level.</span></span>

    <span data-ttu-id="dd1f0-320">![Inteligentne wcięcie w edytorze HTML](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image34.png "inteligentne wcięcie w edytorze HTML")</span><span class="sxs-lookup"><span data-stu-id="dd1f0-320">![Smart indentation in the HTML Editor](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image34.png "Smart indentation in the HTML Editor")</span></span>

    <span data-ttu-id="dd1f0-321">*Inteligentne wcięcie w edytorze HTML*</span><span class="sxs-lookup"><span data-stu-id="dd1f0-321">*Smart indentation in the HTML Editor*</span></span>
4. <span data-ttu-id="dd1f0-322">Przywróć audio tagu z zawartością zostały usunięte lub Zamknij **Default.aspx** bez zapisywania zmian.</span><span class="sxs-lookup"><span data-stu-id="dd1f0-322">Restore the audio tag with the content you have removed, or close **Default.aspx** without saving the changes.</span></span>

<a id="Ex2Task6"></a>

<a id="Task_6_-_Extract_to_User_Control"></a>
#### <a name="task-6---extract-to-user-control"></a><span data-ttu-id="dd1f0-323">Zadanie 6 - Wyodrębnij do kontrolki użytkownika</span><span class="sxs-lookup"><span data-stu-id="dd1f0-323">Task 6 - Extract to User Control</span></span>

<span data-ttu-id="dd1f0-324">Narzędzia Refactoring uwzględnione w programie Visual Studio, takich jak wyodrębnianie fragmentu kodu do funkcji, są wspaniałymi funkcjami, które ułatwiają poprawy i refaktoryzacji istniejącego kodu.</span><span class="sxs-lookup"><span data-stu-id="dd1f0-324">The Refactoring tools included in Visual Studio, such as extracting a portion of code to a function, are great features that facilitate the improvement and the refactoring the existing code.</span></span> <span data-ttu-id="dd1f0-325">Odpowiednikiem stron ASP.NET jest wyodrębniania kodu HTML do kontrolki użytkownika.</span><span class="sxs-lookup"><span data-stu-id="dd1f0-325">The counterpart for ASP.NET pages would be the extraction of HTML code to a User Control.</span></span> <span data-ttu-id="dd1f0-326">To zrobić ręcznie wymagałoby kilku czynności, takie jak tworzenie nowej kontrolki użytkownika, przenoszenie sekcji kodu do kontrolki użytkownika, rejestrowanie prefiksu tagu kontrolki użytkownika i, koniec uruchamianiu formantu użytkownika na stronach.</span><span class="sxs-lookup"><span data-stu-id="dd1f0-326">Doing it manually would involve several steps, like creating a new User Control, moving the code section to the User Control, registering a tag prefix for the User Control, and, finally, instantiating the User Control on the pages.</span></span> <span data-ttu-id="dd1f0-327">Teraz, nowe *Wyodrębnij do kontrolki użytkownika* narzędzie automatycznie wykonuje te kroki dla Ciebie.</span><span class="sxs-lookup"><span data-stu-id="dd1f0-327">Now, the new *Extract to User Control* tool automatically performs all those steps for you.</span></span>

<span data-ttu-id="dd1f0-328">To zadanie będzie używał nowego wyodrębniania operacji kontekstowe kontrolki użytkownika do generowania nową kontrolkę użytkownika z zaznaczonym kodzie.</span><span class="sxs-lookup"><span data-stu-id="dd1f0-328">In this task, you will use the new Extract to User Control contextual operation to generate a new user control from the selected code.</span></span>

1. <span data-ttu-id="dd1f0-329">Na **Default.aspx** wybierz pozycję **H2** i **audio** elementów.</span><span class="sxs-lookup"><span data-stu-id="dd1f0-329">On the **Default.aspx** page, select the **H2** and **audio** elements.</span></span>
2. <span data-ttu-id="dd1f0-330">Kliknij prawym przyciskiem myszy i wybierz **Wyodrębnij do kontrolki użytkownika**.</span><span class="sxs-lookup"><span data-stu-id="dd1f0-330">Right click and select **Extract to User Control**.</span></span>

    <span data-ttu-id="dd1f0-331">![Wyodrębnij do kontrolki użytkownika menu opcji](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image35.png "Wyodrębnij do kontrolki użytkownika opcji menu")</span><span class="sxs-lookup"><span data-stu-id="dd1f0-331">![Extract to User Control menu option](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image35.png "Extract to User Control menu option")</span></span>

    <span data-ttu-id="dd1f0-332">*Wyodrębnij do kontrolki użytkownika opcji menu*</span><span class="sxs-lookup"><span data-stu-id="dd1f0-332">*Extract to User Control menu option*</span></span>
3. <span data-ttu-id="dd1f0-333">Wpisz nazwę nowego formantu użytkownika.</span><span class="sxs-lookup"><span data-stu-id="dd1f0-333">Type a name for the new user control.</span></span> <span data-ttu-id="dd1f0-334">Na przykład **Jukebox.ascx**, a następnie kliknij przycisk **OK**.</span><span class="sxs-lookup"><span data-stu-id="dd1f0-334">For instance, **Jukebox.ascx**, and then click **OK**.</span></span>

    <span data-ttu-id="dd1f0-335">![Zapisywanie kontrolki użytkownika wyodrębnionego](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image36.png "zapisywania kontrolki wyodrębnionego użytkownika")</span><span class="sxs-lookup"><span data-stu-id="dd1f0-335">![Saving the extracted user control](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image36.png "Saving the extracted user control")</span></span>

    <span data-ttu-id="dd1f0-336">*Zapisywanie kontrolki wyodrębnionego użytkownika*</span><span class="sxs-lookup"><span data-stu-id="dd1f0-336">*Saving the extracted user control*</span></span>
4. <span data-ttu-id="dd1f0-337">Zauważyć, że zaznaczony kod został wyodrębniony do kontrolki użytkownika, a Pierwotna lokalizacja zaznaczony kod został zastąpiony wystąpienia nowe kontrolki użytkownika.</span><span class="sxs-lookup"><span data-stu-id="dd1f0-337">Notice that the selected code was extracted to a user control and the original location of the selected code was replaced with an instance of the new user control.</span></span>

    <span data-ttu-id="dd1f0-338">![Strona automatycznie aktualizowane do użycia nowej kontrolki użytkownika](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image37.png "strony są automatycznie aktualizowane do użycia nowej kontrolki użytkownika")</span><span class="sxs-lookup"><span data-stu-id="dd1f0-338">![Page automatically updated to use the new user control](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image37.png "Page automatically updated to use the new user control")</span></span>

    <span data-ttu-id="dd1f0-339">*Strona automatycznie aktualizowane do użycia nowej kontrolki użytkownika*</span><span class="sxs-lookup"><span data-stu-id="dd1f0-339">*Page automatically updated to use the new user control*</span></span>
5. <span data-ttu-id="dd1f0-340">Naciśnij klawisz **F5** do uruchomienia strony i sprawdź, czy formant działa.</span><span class="sxs-lookup"><span data-stu-id="dd1f0-340">Press **F5** to run the page and verify that the control works.</span></span>

<a id="Exercise3"></a>

<a id="Exercise_3_Whats_New_in_the_JavaScript_Editor"></a>
### <a name="exercise-3-whats-new-in-the-javascript-editor"></a><span data-ttu-id="dd1f0-341">Ćwiczenie 3: Nowości w edytorze kodu JavaScript</span><span class="sxs-lookup"><span data-stu-id="dd1f0-341">Exercise 3: What's New in the JavaScript Editor</span></span>

<span data-ttu-id="dd1f0-342">Zapisywanie lub edycji kodu JavaScript nie jest łatwym zadaniem, szczególnie, gdy aplikacja rozpoczyna się zwiększa się rozmiar i okaże się, że zajmowanie długie pliki oraz setki funkcji.</span><span class="sxs-lookup"><span data-stu-id="dd1f0-342">Writing or editing JavaScript code is not an easy task, especially when your application starts to grow in size and you find yourself dealing with long files and hundreds of functions.</span></span> <span data-ttu-id="dd1f0-343">Zwykle programiści skryptu muszą wykonywania dodatkowej pracy, aby zachować czytelność kodu i przechodzić między plików.</span><span class="sxs-lookup"><span data-stu-id="dd1f0-343">Script developers usually have to do some extra work to maintain code legibility and navigate across files.</span></span> <span data-ttu-id="dd1f0-344">Obejmująca biblioteki języka JavaScript, takich jak jQuery nawigacji skryptu stał się żądanie się z powodu długość kodu.</span><span class="sxs-lookup"><span data-stu-id="dd1f0-344">With the inclusion of JavaScript libraries like jQuery, script navigation has become a challenge itself because of the code length.</span></span>

<span data-ttu-id="dd1f0-345">Visual Studio ma odnowiony edytora kodu JavaScript z promise dokonanie tryb kodu, dostępnych i organizowane.</span><span class="sxs-lookup"><span data-stu-id="dd1f0-345">Visual Studio has renewed the JavaScript editor with the promise to make the code mode accessible and organized.</span></span> <span data-ttu-id="dd1f0-346">Wiele funkcji programu Visual Studio, które istniały w językach C# i VB edytory są teraz zaimplementowane w edytorze kodu JavaScript: Przejdź do definicji, automatyczne wcięcia, dokumentacji i sprawdzania poprawności podczas pisania.</span><span class="sxs-lookup"><span data-stu-id="dd1f0-346">Many Visual Studio features that already existed in C# or VB editors are now implemented in the JavaScript editor: Go To Definition, automatic indentation, documentation and validation when you are writing.</span></span> <span data-ttu-id="dd1f0-347">Z listy IntelliSense odnowionego będziesz mieć katalogu funkcji JavaScript w zasięgu ręki.</span><span class="sxs-lookup"><span data-stu-id="dd1f0-347">With the renewed IntelliSense list you will have the JavaScript function catalog at your fingertips.</span></span>

<span data-ttu-id="dd1f0-348">W tym ćwiczeniu dowiesz się, niektóre nowe funkcje i ulepszenia edytora kodu JavaScript.</span><span class="sxs-lookup"><span data-stu-id="dd1f0-348">In this exercise, you will learn some of the new features and improvements of JavaScript editor.</span></span> <span data-ttu-id="dd1f0-349">Zostanie przeglądanie przykładowych plików i odnajdywanie wszystkich nowych cech, które spowoduje, że Twoje programowania w języku JavaScript większą wydajność w programie Visual Studio 2012.</span><span class="sxs-lookup"><span data-stu-id="dd1f0-349">You will browse sample files and discover each of the new characteristics that will make your JavaScript programming more efficient within Visual Studio 2012.</span></span>

<a id="Ex3Task1"></a>

<a id="Task_1_-_JavaScript_Editor_New_Features"></a>
#### <a name="task-1---javascript-editor-new-features"></a><span data-ttu-id="dd1f0-350">Zadanie 1 — nowe funkcje edytora JavaScript</span><span class="sxs-lookup"><span data-stu-id="dd1f0-350">Task 1 - JavaScript Editor New Features</span></span>

<span data-ttu-id="dd1f0-351">To zadanie przedstawiono niektóre nowe funkcje edytora JavaScript, które skupić się na przechowywanie kodu i dostarczają lepsze środowisko pracy użytkownika.</span><span class="sxs-lookup"><span data-stu-id="dd1f0-351">This task will introduce you to some of the new JavaScript editor features, which focus on organizing your code and bringing a better user experience.</span></span>

1. <span data-ttu-id="dd1f0-352">Jeśli nie jest jeszcze otwarty, uruchom **programu Visual Studio** , a następnie otwórz **WhatsNewASPNET.sln** rozwiązania, znajdujących się w **Source\WhatsNewASPNET** folder tego laboratorium.</span><span class="sxs-lookup"><span data-stu-id="dd1f0-352">If not already opened, start **Visual Studio** and open the **WhatsNewASPNET.sln** solution located in the **Source\WhatsNewASPNET** folder of this lab.</span></span>
2. <span data-ttu-id="dd1f0-353">Naciśnij klawisz **F5** do uruchomienia aplikacji, a następnie kliknij łącze JavaScript na pasku nawigacyjnym.</span><span class="sxs-lookup"><span data-stu-id="dd1f0-353">Press **F5** to run the application, then click the JavaScript link in the navigation bar.</span></span> <span data-ttu-id="dd1f0-354">Odśwież stronę kilka razy i sprawdź, jak zwiększa licznik.</span><span class="sxs-lookup"><span data-stu-id="dd1f0-354">Refresh the page several times and check how the counter increments.</span></span>

    <span data-ttu-id="dd1f0-355">![Licznik strony](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image38.png "licznika na stronie")</span><span class="sxs-lookup"><span data-stu-id="dd1f0-355">![Page counter](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image38.png "Page counter")</span></span>

    <span data-ttu-id="dd1f0-356">*Licznik na stronie*</span><span class="sxs-lookup"><span data-stu-id="dd1f0-356">*Page counter*</span></span>
3. <span data-ttu-id="dd1f0-357">Zamknij przeglądarkę i przejdź wstecz do programu Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="dd1f0-357">Close the browser and go back to Visual Studio.</span></span>
4. <span data-ttu-id="dd1f0-358">Otwórz **JavaScript.aspx** strony i Znajdź **&lt;skryptu&gt;** bloku (pokazana poniżej).</span><span class="sxs-lookup"><span data-stu-id="dd1f0-358">Open the **JavaScript.aspx** page and locate the **&lt;script&gt;** block (shown below).</span></span>

    <span data-ttu-id="dd1f0-359">W poniższym kodzie użyto HTML5 lokalnego magazynu do przechowywania *pageLoadCount* zmiennej, która przechowuje numer odwiedza strony przez bieżącego użytkownika.</span><span class="sxs-lookup"><span data-stu-id="dd1f0-359">The following code uses HTML5 local storage to store a *pageLoadCount* variable that stores the number of times the page has been visited by the current user.</span></span> <span data-ttu-id="dd1f0-360">Magazyn lokalny jest bazą danych klucz wartość po stronie klienta wprowadzone ze standardem HTML5.</span><span class="sxs-lookup"><span data-stu-id="dd1f0-360">Local Storage is a client-side key-value database introduced with the HTML5 standard.</span></span> <span data-ttu-id="dd1f0-361">Dane są zapisywane na komputerze lokalnym, w przeglądarce.</span><span class="sxs-lookup"><span data-stu-id="dd1f0-361">The data is saved on the local machine, inside the user's browser.</span></span>

    [!code-html[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample7.html)]

    > [!NOTE]
    > <span data-ttu-id="dd1f0-362">Upewnij się, że DOCTYPE ustawiono XHTML5 przed wykonaniem dalszych kroków.</span><span class="sxs-lookup"><span data-stu-id="dd1f0-362">Ensure the DOCTYPE is set to XHTML5 before proceeding with the next steps.</span></span>
5. <span data-ttu-id="dd1f0-363">Edytuj kod i zwróć uwagę, że IntelliSense dla JavaScript zawiera funkcje HTML5, takie jak i ich metod wewnętrzny magazyn lokalny.</span><span class="sxs-lookup"><span data-stu-id="dd1f0-363">Edit the code and notice that IntelliSense for JavaScript includes HTML5 features, like local storage, and their inner methods.</span></span>

    <span data-ttu-id="dd1f0-364">![Funkcje HTML5 JavaScript w języku JavaScript](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image39.png "funkcji HTML5 JavaScript w języku JavaScript")</span><span class="sxs-lookup"><span data-stu-id="dd1f0-364">![HTML5 JavaScript features in JavaScript](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image39.png "HTML5 JavaScript features in JavaScript")</span></span>

    <span data-ttu-id="dd1f0-365">*Funkcje HTML5 JavaScript w języku JavaScript*</span><span class="sxs-lookup"><span data-stu-id="dd1f0-365">*HTML5 JavaScript features in JavaScript*</span></span>
6. <span data-ttu-id="dd1f0-366">Kliknij żadnych nawiasu otwierającego (**{**) z skryptów kodu i zwróć uwagę, że nawiasy są wyróżnione.</span><span class="sxs-lookup"><span data-stu-id="dd1f0-366">Click any opening bracket (**{**) from the scripting code and notice that the brackets are highlighted.</span></span>

    <span data-ttu-id="dd1f0-367">![Nawiasy kwadratowe są wyróżnione](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image40.png "nawiasy kwadratowe są wyróżnione")</span><span class="sxs-lookup"><span data-stu-id="dd1f0-367">![Brackets are highlighted](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image40.png "Brackets are highlighted")</span></span>

    <span data-ttu-id="dd1f0-368">*Nawiasy kwadratowe są wyróżnione*</span><span class="sxs-lookup"><span data-stu-id="dd1f0-368">*Brackets are highlighted*</span></span>
7. <span data-ttu-id="dd1f0-369">Usuń znaczniki komentarza funkcji **testAutoAlign()** (Wybierz trzy wiersze i można użyć **CTRL** + **K**; **CTRL** + **U**) i Znajdź kursora wewnątrz kodu funkcji.</span><span class="sxs-lookup"><span data-stu-id="dd1f0-369">Uncomment the function **testAutoAlign()** (select the three lines and you can use **CTRL** + **K**; **CTRL** + **U**) and locate the cursor inside the function code.</span></span> <span data-ttu-id="dd1f0-370">Naciśnij klawisz enter, aby można było dodać drugi wiersz.</span><span class="sxs-lookup"><span data-stu-id="dd1f0-370">Press enter to append a second line.</span></span> <span data-ttu-id="dd1f0-371">Należy zauważyć, że kod jest teraz **wyrównane** i **wcięty automatycznie**.</span><span class="sxs-lookup"><span data-stu-id="dd1f0-371">Notice that the code is now **aligned** and **auto-indented**.</span></span>

    <span data-ttu-id="dd1f0-372">![Kod JavaScript jest automatycznie wyrównane](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image41.png "kod JavaScript jest automatycznie wyrównane")</span><span class="sxs-lookup"><span data-stu-id="dd1f0-372">![JavaScript code is auto aligned](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image41.png "JavaScript code is auto aligned")</span></span>

    <span data-ttu-id="dd1f0-373">*Kod JavaScript jest automatycznie wyrównane*</span><span class="sxs-lookup"><span data-stu-id="dd1f0-373">*JavaScript code is auto aligned*</span></span>

<a id="Ex3Task2"></a>

<a id="Task_2_-_Validating_JavaScript"></a>
#### <a name="task-2---validating-javascript"></a><span data-ttu-id="dd1f0-374">Zadanie 2 — Sprawdzanie poprawności kodu JavaScript</span><span class="sxs-lookup"><span data-stu-id="dd1f0-374">Task 2 - Validating JavaScript</span></span>

<span data-ttu-id="dd1f0-375">W tym zadaniu odnajdzie nowy walidacji JavaScript dla standardowej ECMAScript5.</span><span class="sxs-lookup"><span data-stu-id="dd1f0-375">In this task, you will discover the new JavaScript validation for the ECMAScript5 standard.</span></span> <span data-ttu-id="dd1f0-376">Ta funkcja będzie ułatwia pisanie zgodne kod JavaScript, podczas zapobiega zagadnienia dotyczące skryptów przed wdrożeniem witryny.</span><span class="sxs-lookup"><span data-stu-id="dd1f0-376">This feature will help you to write compliant JavaScript code, while preventing scripting issues before site deployment.</span></span>

> [!NOTE]
> <span data-ttu-id="dd1f0-377">Program Visual Studio 2010 zaimplementowana zgodności ECMAStript3 programu Visual Studio 2012 zapewnia zgodność ECMAScript5 jednocześnie.</span><span class="sxs-lookup"><span data-stu-id="dd1f0-377">Visual Studio 2010 implemented ECMAStript3 compliance, while Visual Studio 2012 provides ECMAScript5 compliance.</span></span>


1. <span data-ttu-id="dd1f0-378">Otwórz **ECMA5script5.js** znajduje się w obszarze **Scripts\custom** folderu projektu.</span><span class="sxs-lookup"><span data-stu-id="dd1f0-378">Open **ECMA5script5.js** located under the **Scripts\custom** project folder.</span></span> <span data-ttu-id="dd1f0-379">Teraz zostanie testów sprawdzania poprawności dla ECMAScript5 standardowe.</span><span class="sxs-lookup"><span data-stu-id="dd1f0-379">You will now test validation for ECMAScript5 standard.</span></span>

    [!code-html[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample8.html)]

    <span data-ttu-id="dd1f0-380">Można wyewidencjonować &quot; **używaj z ograniczeniami** &quot; kierunek, w pierwszym wierszu pliku, który umożliwia ECMAScript5 **tryb z ograniczeniami**.</span><span class="sxs-lookup"><span data-stu-id="dd1f0-380">You can check out the &quot; **use strict** &quot; direction in the first line of the file, which enables ECMAScript5 **strict mode**.</span></span> <span data-ttu-id="dd1f0-381">W tym trybie składa się z podzbioru języka, który wyjaśnia niejednoznaczności z poprzednich wersji i dodaje nowe funkcje, takie jak metody pobierające i ustawiające, obsługa bibliotek dla formatu JSON, a dokładniejsza odbicia właściwości obiektu.</span><span class="sxs-lookup"><span data-stu-id="dd1f0-381">This mode consists in a subset of the language that clarifies ambiguities from the past edition, and adds some new features, such as getters and setters, library support for JSON, and more complete reflection on object properties.</span></span>
2. <span data-ttu-id="dd1f0-382">Otwórz **listy błędów** Jeśli nie jest jeszcze otwarty (**widoku** menu | **Listy błędów**).</span><span class="sxs-lookup"><span data-stu-id="dd1f0-382">Open the **Error List** if not already opened (**View** menu | **Error List**).</span></span> <span data-ttu-id="dd1f0-383">Powiadomienie **funkcja** deklaracji jest podkreślony.</span><span class="sxs-lookup"><span data-stu-id="dd1f0-383">Notice the **function** declaration is underlined.</span></span> <span data-ttu-id="dd1f0-384">Jest to spowodowane w ECMA5 standardowych funkcji nie mogą być zagnieżdżone wewnątrz struktury języka.</span><span class="sxs-lookup"><span data-stu-id="dd1f0-384">This is because in ECMA5 standard functions cannot be nested inside language structures.</span></span> <span data-ttu-id="dd1f0-385">W dzienniku błędów poniżej możesz pojawią się szczegóły ostrzeżenie.</span><span class="sxs-lookup"><span data-stu-id="dd1f0-385">In the error list below you will see the warning details.</span></span>

    <span data-ttu-id="dd1f0-386">![Komunikat o błędzie weryfikacji JavaScript](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image42.png "komunikat o błędzie weryfikacji JavaScript")</span><span class="sxs-lookup"><span data-stu-id="dd1f0-386">![JavaScript validation error message](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image42.png "JavaScript validation error message")</span></span>

    <span data-ttu-id="dd1f0-387">*Komunikat o błędzie weryfikacji JavaScript*</span><span class="sxs-lookup"><span data-stu-id="dd1f0-387">*JavaScript validation error message*</span></span>
3. <span data-ttu-id="dd1f0-388">Komentarz **&quot;używaj z ograniczeniami&quot;** kierunek i zwróć uwagę, że błędy znikają, przy zachowaniu ostrzeżenia.</span><span class="sxs-lookup"><span data-stu-id="dd1f0-388">Comment out the **&quot;use strict&quot;** direction and notice that errors disappear, but the warnings remain.</span></span>
4. <span data-ttu-id="dd1f0-389">W ostatnim wierszu pliku, należy zapisać dowolny ciąg, takich jak **&quot;test&quot;** (należy używać znaków cudzysłowu, aby wskazać ją jako ciąg).</span><span class="sxs-lookup"><span data-stu-id="dd1f0-389">In the last line of the file, write any string like **&quot;test&quot;** (include the quotation marks to indicate it is as string).</span></span> <span data-ttu-id="dd1f0-390">Zapis okresu obok ciąg do wyświetlania na liście IntelliSense i wybierz **trim** opcji.</span><span class="sxs-lookup"><span data-stu-id="dd1f0-390">Write a period next to the string to display the IntelliSense list, and select the **trim** option.</span></span>

    <span data-ttu-id="dd1f0-391">W standardzie ECMAScript5 wartości parametrów i zmiennych również mieć zdefiniowane, takich jak trim, wielkie litery, wyszukiwania i zamieniania metod ciągów.</span><span class="sxs-lookup"><span data-stu-id="dd1f0-391">In ECMAScript5 standard, string values and variables also have string methods defined, like trim, uppercase, search and replace.</span></span>

    <span data-ttu-id="dd1f0-392">![Lista IntelliSense w języku JavaScript](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image43.png "listy IntelliSense w języku JavaScript")</span><span class="sxs-lookup"><span data-stu-id="dd1f0-392">![IntelliSense list in JavaScript](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image43.png "IntelliSense list in JavaScript")</span></span>

    <span data-ttu-id="dd1f0-393">*Lista IntelliSense w języku JavaScript*</span><span class="sxs-lookup"><span data-stu-id="dd1f0-393">*IntelliSense list in JavaScript*</span></span>

<a id="Ex3Task3"></a>

<a id="Task_3_-_XML_Documentation_for_JavaScript"></a>
#### <a name="task-3---xml-documentation-for-javascript"></a><span data-ttu-id="dd1f0-394">Zadanie 3. plik dokumentacji XML JavaScript</span><span class="sxs-lookup"><span data-stu-id="dd1f0-394">Task 3 - XML Documentation for JavaScript</span></span>

<span data-ttu-id="dd1f0-395">W tym zadaniu zostanie Poznaj funkcje programu Visual Studio dla dokumentacji XML w języku JavaScript.</span><span class="sxs-lookup"><span data-stu-id="dd1f0-395">In this task, you will explore Visual Studio features for XML documentation in JavaScript.</span></span> <span data-ttu-id="dd1f0-396">Zobaczysz, że lista IntelliSense dla JavaScript zawiera teraz dokumentacji XML każdej funkcji.</span><span class="sxs-lookup"><span data-stu-id="dd1f0-396">You will see the JavaScript IntelliSense list now shows the XML documentation of each function.</span></span> <span data-ttu-id="dd1f0-397">Ponadto możesz zauważyć funkcję nawigacji w języku JavaScript.</span><span class="sxs-lookup"><span data-stu-id="dd1f0-397">Additionally, you will discover the navigation feature in JavaScript.</span></span>

1. <span data-ttu-id="dd1f0-398">Otwórz **XMLDoc.js** plik znajdujący się w **skryptów/niestandardowa** folderu projektu.</span><span class="sxs-lookup"><span data-stu-id="dd1f0-398">Open **XMLDoc.js** file located in **Scripts/custom** project folder.</span></span> <span data-ttu-id="dd1f0-399">Ten plik zawiera dokumentacja XML na każdej z tych funkcji JavaScript.</span><span class="sxs-lookup"><span data-stu-id="dd1f0-399">This file contains XML documentation on each of the JavaScript functions.</span></span>

    <span data-ttu-id="dd1f0-400">![Plik dokumentacji JavaScript XML zintegrowany z IntelliSense](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image44.png "dokumentacji JavaScript XML zintegrowany z IntelliSense")</span><span class="sxs-lookup"><span data-stu-id="dd1f0-400">![JavaScript XML documentation integrated to IntelliSense](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image44.png "JavaScript XML documentation integrated to IntelliSense")</span></span>

    <span data-ttu-id="dd1f0-401">*Plik dokumentacji JavaScript XML zintegrowany z IntelliSense*</span><span class="sxs-lookup"><span data-stu-id="dd1f0-401">*JavaScript XML documentation integrated to IntelliSense*</span></span>
2. <span data-ttu-id="dd1f0-402">Poniżej **dodać** działać w **XMLDoc.js** plików, Utwórz nową funkcję o nazwie **testu**.</span><span class="sxs-lookup"><span data-stu-id="dd1f0-402">Below **add** function in **XMLDoc.js** file, create a new function named **test**.</span></span>
3. <span data-ttu-id="dd1f0-403">W **test** funkcji, należy wywołać **mnożenia** funkcja, która otrzymuje dwa parametry.</span><span class="sxs-lookup"><span data-stu-id="dd1f0-403">In the **test** function, call the **multiply** function that receives two parameters.</span></span> <span data-ttu-id="dd1f0-404">Zwróć uwagę, pole tooltip jest pokazywany **mnożenia** funkcji dokumentacji.</span><span class="sxs-lookup"><span data-stu-id="dd1f0-404">Notice the tooltip box is showing the **multiply** function documentation.</span></span>

    [!code-javascript[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample9.js)]

    <span data-ttu-id="dd1f0-405">![Plik dokumentacji XML dla funkcji JavaScript](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image45.png "dokumentacji XML dla funkcji JavaScript")</span><span class="sxs-lookup"><span data-stu-id="dd1f0-405">![XML documentation for JavaScript functions](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image45.png "XML documentation for JavaScript functions")</span></span>

    <span data-ttu-id="dd1f0-406">*Plik dokumentacji XML dla funkcji JavaScript*</span><span class="sxs-lookup"><span data-stu-id="dd1f0-406">*XML documentation for JavaScript functions*</span></span>
4. <span data-ttu-id="dd1f0-407">Zakończenie instrukcji call funkcji i typ *kropka* aby otworzyć listę IntelliSense w zwracanej wartości.</span><span class="sxs-lookup"><span data-stu-id="dd1f0-407">Complete the function call statement and type a *dot* to open the IntelliSense list on the returned value.</span></span> <span data-ttu-id="dd1f0-408">Należy zauważyć, że program Visual Studio jest wykrywanie **zwracać** wartość w dokumentacji, traktując wartości jako liczby.</span><span class="sxs-lookup"><span data-stu-id="dd1f0-408">Notice that Visual Studio is detecting the **return** value in the documentation, treating the value as a number.</span></span>

    <span data-ttu-id="dd1f0-409">![Plik dokumentacji XML dla zwracanych typów](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image46.png "dokumentacji XML dla zwracane typy")</span><span class="sxs-lookup"><span data-stu-id="dd1f0-409">![XML documentation for return types](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image46.png "XML documentation for return types")</span></span>

    <span data-ttu-id="dd1f0-410">*Plik dokumentacji XML dla zwracane typy*</span><span class="sxs-lookup"><span data-stu-id="dd1f0-410">*XML documentation for return types*</span></span>
5. <span data-ttu-id="dd1f0-411">Teraz Wstawianie wywołania do add — funkcja.</span><span class="sxs-lookup"><span data-stu-id="dd1f0-411">Now, insert a call to add function.</span></span> <span data-ttu-id="dd1f0-412">Zwróć uwagę, że edytor JavaScript obsługuje teraz przeciążenia funkcji.</span><span class="sxs-lookup"><span data-stu-id="dd1f0-412">Notice that the JavaScript editor now supports function overloads.</span></span> <span data-ttu-id="dd1f0-413">Podczas pisania nazwę funkcji, można wybrać dowolny z dostępnych przeciążeń określony w dokumentacji.</span><span class="sxs-lookup"><span data-stu-id="dd1f0-413">When you write a function name, you will be able to select any of the available overloads specified in the documentation.</span></span>

    <span data-ttu-id="dd1f0-414">![Dokumentacja XML do przeciążenia](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image47.png "dokumentacji XML dla przeciążenia")</span><span class="sxs-lookup"><span data-stu-id="dd1f0-414">![XML documentation for overloads](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image47.png "XML documentation for overloads")</span></span>

    <span data-ttu-id="dd1f0-415">*Dokumentacja XML do przeciążenia*</span><span class="sxs-lookup"><span data-stu-id="dd1f0-415">*XML documentation for overloads*</span></span>
6. <span data-ttu-id="dd1f0-416">Otwórz **GotoDefinition.js** plików i Znajdź **$().html()** wywołania funkcji.</span><span class="sxs-lookup"><span data-stu-id="dd1f0-416">Open **GotoDefinition.js** file and locate the **$().html()** function call.</span></span> <span data-ttu-id="dd1f0-417">Zlokalizuj kursor na **html**.</span><span class="sxs-lookup"><span data-stu-id="dd1f0-417">Locate the cursor on **html**.</span></span>
7. <span data-ttu-id="dd1f0-418">Naciśnij klawisz **F12** i przejdź do definicji.</span><span class="sxs-lookup"><span data-stu-id="dd1f0-418">Press **F12** and navigate to the definition.</span></span> <span data-ttu-id="dd1f0-419">Powiadomienia można teraz uzyskać dostępu i przeglądać kod JavaScript bez użycia **znaleźć** narzędzia.</span><span class="sxs-lookup"><span data-stu-id="dd1f0-419">Notice you can now access and browse your JavaScript code without using the **Find** tool.</span></span>
8. <span data-ttu-id="dd1f0-420">Zlokalizuj kursor w instrukcji jQuery przed blok podpisu w dolnej części pliku kodu.</span><span class="sxs-lookup"><span data-stu-id="dd1f0-420">Locate the cursor on the jQuery instruction prior to the signature block at the bottom of the code file.</span></span> <span data-ttu-id="dd1f0-421">Naciśnij klawisz **F12**.</span><span class="sxs-lookup"><span data-stu-id="dd1f0-421">Press **F12**.</span></span> <span data-ttu-id="dd1f0-422">Nastąpi przeniesienie do pliku biblioteki jQuery.</span><span class="sxs-lookup"><span data-stu-id="dd1f0-422">You will navigate to the jQuery library file.</span></span> <span data-ttu-id="dd1f0-423">Zwróć uwagę, można także przechodzić między pliki jQuery przy użyciu **F12**.</span><span class="sxs-lookup"><span data-stu-id="dd1f0-423">Notice you can also navigate across the jQuery files using **F12**.</span></span>

    <span data-ttu-id="dd1f0-424">![Przechodzenie do definicji jQuery](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image48.png "przejść do definicji jQuery")</span><span class="sxs-lookup"><span data-stu-id="dd1f0-424">![Navigating to jQuery definitions](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image48.png "Navigating to jQuery definitions")</span></span>

    <span data-ttu-id="dd1f0-425">*Przechodzenie do definicji jQuery*</span><span class="sxs-lookup"><span data-stu-id="dd1f0-425">*Navigating to jQuery definitions*</span></span>

> [!NOTE]
> <span data-ttu-id="dd1f0-426">Upewnij się, że GotoDefinition.js nie zawiera składnię błędów przed zapisaniem pliku.</span><span class="sxs-lookup"><span data-stu-id="dd1f0-426">Make sure that GotoDefinition.js has no syntax errors before saving the file.</span></span>


<a id="Exercise4"></a>

<a id="Exercise_4_Bundling_and_Minification"></a>
### <a name="exercise-4-bundling-and-minification"></a><span data-ttu-id="dd1f0-427">Ćwiczenie 4: Tworzenie pakietów i minimalizowanie</span><span class="sxs-lookup"><span data-stu-id="dd1f0-427">Exercise 4: Bundling and Minification</span></span>

<span data-ttu-id="dd1f0-428">Ile razy witryny sieci Web ma więcej niż jednego języka JavaScript lub CSS pliku?</span><span class="sxs-lookup"><span data-stu-id="dd1f0-428">How many times do your websites include more than one JavaScript or CSS file?</span></span> <span data-ttu-id="dd1f0-429">Jest to bardzo typowy scenariusz, w którym tworzenie pakietów i minimalizowanie może pomóc zmniejszyć rozmiar pliku i witryna szybsze.</span><span class="sxs-lookup"><span data-stu-id="dd1f0-429">This is a very common scenario where bundling and minification can help to reduce the file size and make the site perform faster.</span></span> <span data-ttu-id="dd1f0-430">Nowa funkcja powiązanego w programie ASP.NET 4.5 pakiety zestawu plików JS i CSS do pojedynczego elementu i zmniejsza rozmiar przez zminimalizowania liczby zawartości (tj. usuwanie spacje nie są wymagane, usuwanie komentarzy, zmniejszając identyfikatory).</span><span class="sxs-lookup"><span data-stu-id="dd1f0-430">The new bundling feature in ASP.NET 4.5 packs a set of JS or CSS files into a single element, and reduces its size by minifying the content ( i.e. removing not required blank spaces, removing comments, reducing identifiers ).</span></span>

<span data-ttu-id="dd1f0-431">Tworzenie pakietów i minimalizowanie w programie ASP.NET 4.5 jest wykonywane w czasie wykonywania, aby proces identyfikowania agent użytkownika (na przykład programu Internet Explorer, Mozilla itp.), a w związku z tym poprawy kompresji, wybierając w przeglądarce użytkownika (na przykład usuwania rzeczy będący Mozilla określonych Jeśli żądanie pochodzi z programu Internet Explorer).</span><span class="sxs-lookup"><span data-stu-id="dd1f0-431">Bundling and minification in ASP.NET 4.5 is performed at runtime, so that the process can identify the user agent (for example IE, Mozilla, etc), and thus, improve the compression by targeting the user browser (for instance, removing stuff that is Mozilla specific when the request comes from IE).</span></span>

<span data-ttu-id="dd1f0-432">W tym ćwiczeniu dowiesz się, jak włączyć i korzystać z różnego rodzaju tworzenie pakietów i minimalizowanie w programie ASP.NET 4.5.</span><span class="sxs-lookup"><span data-stu-id="dd1f0-432">In this exercise, you will learn how to enable and use the different types of bundling and minification in ASP.NET 4.5.</span></span>

<a id="Ex4Task1"></a>

<a id="Task_1_-_Installing_the_Bundling_and_Minification_Package_from_NuGet"></a>
#### <a name="task-1---installing-the-bundling-and-minification-package-from-nuget"></a><span data-ttu-id="dd1f0-433">Zadanie 1 — Instalowanie tworzenie pakietów i minimalizowanie pakiet NuGet</span><span class="sxs-lookup"><span data-stu-id="dd1f0-433">Task 1 - Installing the Bundling and Minification Package from NuGet</span></span>

1. <span data-ttu-id="dd1f0-434">Jeśli nie jest jeszcze otwarty, uruchom **programu Visual Studio** , a następnie otwórz **WhatsNewASPNET.sln** rozwiązania, znajdujących się w **Source\WhatsNewASPNET** folder tego laboratorium.</span><span class="sxs-lookup"><span data-stu-id="dd1f0-434">If not already opened, start **Visual Studio** and open the **WhatsNewASPNET.sln** solution located in the **Source\WhatsNewASPNET** folder of this lab.</span></span>
2. <span data-ttu-id="dd1f0-435">Otwórz konsolę Menedżera pakietów NuGet.</span><span class="sxs-lookup"><span data-stu-id="dd1f0-435">Open the NuGet Package Manager Console.</span></span> <span data-ttu-id="dd1f0-436">Aby to zrobić, użyj menu **widoku** | **inne okna** | **Konsola Menedżera pakietów**.</span><span class="sxs-lookup"><span data-stu-id="dd1f0-436">To do this, use the menu **View** | **Other Windows** | **Package Manager Console**.</span></span>

    <span data-ttu-id="dd1f0-437">![Otwieranie pakietu file:///C:/Users/User/AppData/Local/Temp/Marker3744//media/44462/Multiple-Stylesheets-and-JavaScript-files-in-the-application.pngconsole Menedżera](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image49.png "otwarcie konsoli Menedżera pakietów")</span><span class="sxs-lookup"><span data-stu-id="dd1f0-437">![Opening the package manager file:///C:/Users/User/AppData/Local/Temp/Marker3744//media/44462/Multiple-Stylesheets-and-JavaScript-files-in-the-application.pngconsole](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image49.png "Opening the package manager console")</span></span>

    <span data-ttu-id="dd1f0-438">*Otwieranie konsoli Menedżera pakietów*</span><span class="sxs-lookup"><span data-stu-id="dd1f0-438">*Opening the package manager console*</span></span>
3. <span data-ttu-id="dd1f0-439">W **Konsola Menedżera pakietów,** typu **Microsoft.Web.Optimization Install-Package** i naciśnij klawisz **ENTER**.</span><span class="sxs-lookup"><span data-stu-id="dd1f0-439">In the **Package Manager Console,** type **Install-Package Microsoft.Web.Optimization** and press **ENTER**.</span></span>

<a id="Ex4Task2"></a>

<a id="Task_2_-_Default_Bundles"></a>
#### <a name="task-2---default-bundles"></a><span data-ttu-id="dd1f0-440">Zadanie 2 — domyślne pakiety</span><span class="sxs-lookup"><span data-stu-id="dd1f0-440">Task 2 - Default Bundles</span></span>

<span data-ttu-id="dd1f0-441">Najprostszym sposobem użycia tworzenie pakietów i minimalizowanie jest umożliwienie pakiety z domyślnej.</span><span class="sxs-lookup"><span data-stu-id="dd1f0-441">The simplest way to use bundling and minification is to enable the default bundles.</span></span> <span data-ttu-id="dd1f0-442">Ta metoda używa konwencji, aby można było odwołać powiązane i zminimalizowany wersji JS i CSS pliki w folderze.</span><span class="sxs-lookup"><span data-stu-id="dd1f0-442">This method uses conventions to let you reference the bundled and minified version for the JS and CSS files in a folder.</span></span>

<span data-ttu-id="dd1f0-443">W tym zadaniu dowiesz się, jak włączyć i powiązane i zminimalizowany plików JS i CSS i wyświetlić dane wyjściowe.</span><span class="sxs-lookup"><span data-stu-id="dd1f0-443">In this task, you will learn how to enable and reference the bundled and minified JS and CSS files and view the resulting output.</span></span>

1. <span data-ttu-id="dd1f0-444">Jeśli nie jest jeszcze otwarty, uruchom **programu Visual Studio** , a następnie otwórz **WhatsNewASPNET.sln** rozwiązania, znajdujących się w **Source\WhatsNewASPNET** folder tego laboratorium.</span><span class="sxs-lookup"><span data-stu-id="dd1f0-444">If not already opened, start **Visual Studio** and open the **WhatsNewASPNET.sln** solution located in the **Source\WhatsNewASPNET** folder of this lab.</span></span>
2. <span data-ttu-id="dd1f0-445">W **Eksploratora rozwiązań**, rozwiń węzeł **style**, **Scripts\custom** i **Scripts\bundle** folderów.</span><span class="sxs-lookup"><span data-stu-id="dd1f0-445">In the **Solution Explorer**, expand the **Styles**, **Scripts\custom** and **Scripts\bundle** folders.</span></span>

    <span data-ttu-id="dd1f0-446">Zwróć uwagę, że aplikacja używa pliku więcej niż jeden CSS i JS.</span><span class="sxs-lookup"><span data-stu-id="dd1f0-446">Notice that the application is using more than one CSS and JS file.</span></span>

    <span data-ttu-id="dd1f0-447">![Pliki wielu arkusze stylów i JavaScript w aplikacji](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image50.png "pliki wielu arkusze stylów i JavaScript w aplikacji")</span><span class="sxs-lookup"><span data-stu-id="dd1f0-447">![Multiple Stylesheets and JavaScript files in the application](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image50.png "Multiple Stylesheets and JavaScript files in the application")</span></span>

    <span data-ttu-id="dd1f0-448">*Wiele plików arkusze stylów i JavaScript w aplikacji*</span><span class="sxs-lookup"><span data-stu-id="dd1f0-448">*Multiple Stylesheets and JavaScript files in the application*</span></span>
3. <span data-ttu-id="dd1f0-449">Otwórz **Global.asax.cs** pliku.</span><span class="sxs-lookup"><span data-stu-id="dd1f0-449">Open the **Global.asax.cs** file.</span></span>

    <span data-ttu-id="dd1f0-450">Zwróć uwagę, że nowe **Microsoft.Web.Optimization** przestrzeni nazw jest oznaczone jako komentarz na początku pliku.</span><span class="sxs-lookup"><span data-stu-id="dd1f0-450">Notice that the new **Microsoft.Web.Optimization** namespace is commented out at the beginning of the file.</span></span> <span data-ttu-id="dd1f0-451">Usuń znaczniki komentarza użycie dyrektywy w celu włączenia funkcji Tworzenie pakietów i minimalizowanie.</span><span class="sxs-lookup"><span data-stu-id="dd1f0-451">Uncomment the using directive to include the bundling and minification features.</span></span>


~~~
[!code-csharp[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample10.cs)]
~~~
4. <span data-ttu-id="dd1f0-452">Zlokalizuj **aplikacji\_Start** metody.</span><span class="sxs-lookup"><span data-stu-id="dd1f0-452">Locate the **Application\_Start** method.</span></span>

    <span data-ttu-id="dd1f0-453">W przypadku tej metody usuń znaczniki komentarza wywołania EnableDefaultBundles, jak pokazano w poniższy fragment.</span><span class="sxs-lookup"><span data-stu-id="dd1f0-453">In this method, uncomment the EnableDefaultBundles call as shown in the snippet below.</span></span> <span data-ttu-id="dd1f0-454">Pozwala na odwołanie zbiór powiązane pliki CSS w folderze przy użyciu ścieżki do tego folderu i &quot;CSS&quot; lub &quot;JS&quot; sufiks.</span><span class="sxs-lookup"><span data-stu-id="dd1f0-454">This enables us to reference a bundled collection of CSS files in a folder by using the path to that folder, plus the &quot;CSS&quot; or the &quot;JS&quot; suffix.</span></span>


~~~
[!code-csharp[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample11.cs)]
~~~
5. <span data-ttu-id="dd1f0-455">Otwórz **Optimization.aspx** pliku, a następnie zlokalizuj formant zawartości dla **HeadContent**.</span><span class="sxs-lookup"><span data-stu-id="dd1f0-455">Open the **Optimization.aspx** file and locate the content control for **HeadContent**.</span></span>

    <span data-ttu-id="dd1f0-456">Zwróć uwagę, pliki CSS i JS zawierać jeden tag do którego istnieje odwołanie.</span><span class="sxs-lookup"><span data-stu-id="dd1f0-456">Notice the CSS files and the JS files have a single referenced tag.</span></span>


~~~
[!code-aspx[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample12.aspx)]

> [!NOTE]
> This code is for demo purposes. Ideally, you will reference the bundles in the Site.Master file. In this sample code, you will find that some of the bundled files are also being referenced by the Site.Master file, making this last reference redundant.
~~~
6. <span data-ttu-id="dd1f0-457">Należy zauważyć, że łącza z powiązanego konwencje w **href** atrybutu Pobierz pliki CSS i Javascript z style i Scripts\custom folderu odpowiednio.</span><span class="sxs-lookup"><span data-stu-id="dd1f0-457">Notice that the links are using the bundling conventions in the **href** attribute to get all the CSS or JS files from the Styles and Scripts\custom folder respectively.</span></span>

    <span data-ttu-id="dd1f0-458">Można użyć ścieżki **skryptów/niestandardowe/JS** przedstawioną poniżej połączyć w paczkę i zminimalizowania wszystkie pliki JS wewnątrz **skryptów/niestandardowa** folderu.</span><span class="sxs-lookup"><span data-stu-id="dd1f0-458">You can use the path **Scripts/custom/JS** as shown below to bundle and minify all the JS files inside a **Scripts/custom** folder.</span></span> <span data-ttu-id="dd1f0-459">Jest to domyślne zachowanie z pakietami domyślne.</span><span class="sxs-lookup"><span data-stu-id="dd1f0-459">This is the default behavior with the default bundles.</span></span>


~~~
[!code-aspx[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample13.aspx)]
~~~
7. <span data-ttu-id="dd1f0-460">Otwórz **Styles\Site.css** pliku.</span><span class="sxs-lookup"><span data-stu-id="dd1f0-460">Open the **Styles\Site.css** file.</span></span>

    <span data-ttu-id="dd1f0-461">Zwróć uwagę, że oryginalnego pliku CSS zawiera kod wcięta, spacje i komentarze, które powiększyć plik.</span><span class="sxs-lookup"><span data-stu-id="dd1f0-461">Notice that the original CSS file contains indented code, blank spaces and comments that enlarge the file.</span></span> <span data-ttu-id="dd1f0-462">(Również JavaScript zawiera spacje i komentarze).</span><span class="sxs-lookup"><span data-stu-id="dd1f0-462">(Also the JavaScript file contains blank spaces and comments).</span></span>

    <span data-ttu-id="dd1f0-463">![Jeden z oryginalnego CSS pliki w folderze skryptów](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image51.png "jedną z oryginalnego CSS pliki w folderze skryptów")</span><span class="sxs-lookup"><span data-stu-id="dd1f0-463">![One of the original CSS files in the Scripts folder](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image51.png "One of the original CSS files in the Scripts folder")</span></span>

    <span data-ttu-id="dd1f0-464">*Jeden z oryginalnych plików CSS w folderze skryptów*</span><span class="sxs-lookup"><span data-stu-id="dd1f0-464">*One of the original CSS files in the Scripts folder*</span></span>
8. <span data-ttu-id="dd1f0-465">Naciśnij klawisz **F5** do uruchamiania aplikacji i przejdź do **optymalizacji** strony.</span><span class="sxs-lookup"><span data-stu-id="dd1f0-465">Press **F5** to run the application and navigate to the **Optimization** page.</span></span>
9. <span data-ttu-id="dd1f0-466">Polecenie **pakietu CSS** łącze aby pobrać i otworzyć plik.</span><span class="sxs-lookup"><span data-stu-id="dd1f0-466">Click on the **CSS Bundle** link to download and open the file.</span></span>

    <span data-ttu-id="dd1f0-467">Wyewidencjonuj plik powiązane zminimalizowany.</span><span class="sxs-lookup"><span data-stu-id="dd1f0-467">Check out the minified bundled file.</span></span> <span data-ttu-id="dd1f0-468">Można zauważyć, że wszystkie spacje, komentarze i wcięcia znaków zostały usunięte, generowanie mniejszy plik.</span><span class="sxs-lookup"><span data-stu-id="dd1f0-468">You will notice that all the blank spaces, comments and indentation characters have been removed, generating a smaller file.</span></span>

    <span data-ttu-id="dd1f0-469">![Powiązane pliki CSS](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image52.png "pliki CSS powiązane")</span><span class="sxs-lookup"><span data-stu-id="dd1f0-469">![Bundled CSS files](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image52.png "Bundled CSS files")</span></span>

    <span data-ttu-id="dd1f0-470">*Powiązane pliki CSS*</span><span class="sxs-lookup"><span data-stu-id="dd1f0-470">*Bundled CSS files*</span></span>
10. <span data-ttu-id="dd1f0-471">Teraz kliknij **pakietu JS** łącze, aby otworzyć plik JavaScript powiązane.</span><span class="sxs-lookup"><span data-stu-id="dd1f0-471">Now click the **JS Bundle** link to open the JavaScript bundled file.</span></span> <span data-ttu-id="dd1f0-472">Można bezpiecznie zignorować explorer ostrzeżenie.</span><span class="sxs-lookup"><span data-stu-id="dd1f0-472">You can safely disregard the explorer warning.</span></span> <span data-ttu-id="dd1f0-473">Zwróć uwagę, pliki JavaScript w **niestandardowych** folderu są również powiązane i zminimalizowany.</span><span class="sxs-lookup"><span data-stu-id="dd1f0-473">Notice the JavaScript files under the **custom** folder are also bundled and minified.</span></span>

    <span data-ttu-id="dd1f0-474">![Powiązane pliki JavaScript](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image53.png "pliki pakietu języka JavaScript")</span><span class="sxs-lookup"><span data-stu-id="dd1f0-474">![Bundled JavaScript files](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image53.png "Bundled JavaScript files")</span></span>

    <span data-ttu-id="dd1f0-475">*Powiązane pliki JavaScript*</span><span class="sxs-lookup"><span data-stu-id="dd1f0-475">*Bundled JavaScript files*</span></span>

    <span data-ttu-id="dd1f0-476">Włączanie kompresji plików CSS i Javascript w poprzedniej wersji programu ASP.NET została znacznie bardziej skomplikowane.</span><span class="sxs-lookup"><span data-stu-id="dd1f0-476">Enabling compression for CSS or JS files was much more complicated in previous ASP.NET version.</span></span> <span data-ttu-id="dd1f0-477">Teraz, jak już wspomniano, wystarczy dodać jeden wiersz w *Global.asax* plików, aby włączyć funkcję tworzenia pakietów i odwoływanie powiązane pliki z witryny.</span><span class="sxs-lookup"><span data-stu-id="dd1f0-477">Now, as you have seen, you just need to add one line in the *Global.asax* file to enable bundling, and then reference the bundled files from your site.</span></span>

<a id="Ex4Task3"></a>

<a id="Task_3_-_Static_Bundles"></a>
#### <a name="task-3---static-bundles"></a><span data-ttu-id="dd1f0-478">Zadanie 3 — statyczne pakietów</span><span class="sxs-lookup"><span data-stu-id="dd1f0-478">Task 3 - Static Bundles</span></span>

<span data-ttu-id="dd1f0-479">Metody statyczne pakietu umożliwia dostosowanie zestawu plików do pakietu, odwołanie i metody minimalizację, który będzie używany.</span><span class="sxs-lookup"><span data-stu-id="dd1f0-479">The static bundle approach allows you to customize the set of files to bundle, the reference and the minification method that will be used.</span></span>

<span data-ttu-id="dd1f0-480">W tym zadaniu skonfiguruj statyczny pakietu do definiowania określonego zestawu plików pakietu do zminimalizowania.</span><span class="sxs-lookup"><span data-stu-id="dd1f0-480">In this task, you will configure a static bundle to define a specific set of files to bundle and minify.</span></span>

1. <span data-ttu-id="dd1f0-481">Zamknij przeglądarkę.</span><span class="sxs-lookup"><span data-stu-id="dd1f0-481">Close the browser.</span></span>
2. <span data-ttu-id="dd1f0-482">Otwórz **Global.asax.cs** plików i Znajdź **aplikacji\_Start** metody.</span><span class="sxs-lookup"><span data-stu-id="dd1f0-482">Open the **Global.asax.cs** file and locate the **Application\_Start** method.</span></span>
3. <span data-ttu-id="dd1f0-483">Usuń komentarz kodu pakietu statycznych, jak pokazano w poniższym kodzie.</span><span class="sxs-lookup"><span data-stu-id="dd1f0-483">Uncomment the static bundle code as shown in the code below.</span></span>

    <span data-ttu-id="dd1f0-484">Definiowania statycznego pakiet, który zostanie dodane odwołanie z &quot; **~/StaticBundle** &quot; ścieżki wirtualnej i użyj **JsMinify** dla minimalizację wszystkie określone pliki z **AddFile** metody.</span><span class="sxs-lookup"><span data-stu-id="dd1f0-484">You are defining a static bundle that will be referenced with the &quot;**~/StaticBundle**&quot; virtual path and use **JsMinify** for minification of all the specified files with the **AddFile** method.</span></span> <span data-ttu-id="dd1f0-485">Ponadto w przypadku dodawania statycznych pakietu do **BundleTable** i włączenie go.</span><span class="sxs-lookup"><span data-stu-id="dd1f0-485">Finally, you are adding the static bundle to the **BundleTable** and enabling it.</span></span>

    <span data-ttu-id="dd1f0-486">Zwróć uwagę, że pliki nie znajdują się w tym samym miejscu; jest to możliwości innego niż domyślny, tworzenie pakietów.</span><span class="sxs-lookup"><span data-stu-id="dd1f0-486">Notice that the files are not located in the same place; this is another advantage over the default bundling.</span></span>


~~~
[!code-csharp[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample14.cs)]
~~~
4. <span data-ttu-id="dd1f0-487">Otwórz **Optimization.aspx** pliku.</span><span class="sxs-lookup"><span data-stu-id="dd1f0-487">Open the **Optimization.aspx** file.</span></span>

    <span data-ttu-id="dd1f0-488">Zwróć uwagę, że łącze do **statycznych pakietu JS** jest przy użyciu ścieżki zadeklarowaniu podczas konfigurowania statycznego pakietu w pliku Global.asax.cs: **/StaticBundle**.</span><span class="sxs-lookup"><span data-stu-id="dd1f0-488">Notice that the link to **Static JS Bundle** is using the path you have declared when you configured the static bundle in the Global.asax.cs file: **/StaticBundle**.</span></span>


~~~
[!code-aspx[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample15.aspx)]
~~~
5. <span data-ttu-id="dd1f0-489">Naciśnij klawisz **F5** do uruchomienia aplikacji, a następnie przejdź do **optymalizacji** strony.</span><span class="sxs-lookup"><span data-stu-id="dd1f0-489">Press **F5** to run the application, and then navigate to the **Optimization** page.</span></span>
6. <span data-ttu-id="dd1f0-490">Polecenie **statycznych pakietu JS** łącze, aby otworzyć plik.</span><span class="sxs-lookup"><span data-stu-id="dd1f0-490">Click on the **Static JS Bundle** link to open the file.</span></span>

    <span data-ttu-id="dd1f0-491">Powiadomienie zminimalizowany powiązane plik JavaScript jest wyjściem dla wszystkich plików JavaScript skonfigurowany w pliku statycznego pakietu w ścieżce &quot;/StaticBundle&quot;.</span><span class="sxs-lookup"><span data-stu-id="dd1f0-491">Notice that the minified bundled JavaScript file is the output for all the JavaScript files configured in the static bundle file under the path &quot;/StaticBundle&quot;.</span></span>

    <span data-ttu-id="dd1f0-492">![Statyczne pakietu plików JavaScript](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image54.png "pakietu plików statycznych JavaScript")</span><span class="sxs-lookup"><span data-stu-id="dd1f0-492">![Static JavaScript files bundle](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image54.png "Static JavaScript files bundle")</span></span>

    <span data-ttu-id="dd1f0-493">*Pliki statyczne JavaScript pakietu*</span><span class="sxs-lookup"><span data-stu-id="dd1f0-493">*Static JavaScript files bundle*</span></span>
7. <span data-ttu-id="dd1f0-494">Zamknij przeglądarkę i powrócić do programu Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="dd1f0-494">Close the browser and return to Visual Studio.</span></span>

<a id="Ex4Task4"></a>

<a id="Task_4_-_Dynamic_Folder_Bundles"></a>
#### <a name="task-4---dynamic-folder-bundles"></a><span data-ttu-id="dd1f0-495">Zadanie 4 — pakietów dynamicznych folderu</span><span class="sxs-lookup"><span data-stu-id="dd1f0-495">Task 4 - Dynamic Folder Bundles</span></span>

<span data-ttu-id="dd1f0-496">W tym zadaniu dowiesz się, sposobu konfigurowania pakietów dynamicznych folderu.</span><span class="sxs-lookup"><span data-stu-id="dd1f0-496">In this task, you will learn how to configure dynamic folder bundles.</span></span> <span data-ttu-id="dd1f0-497">Moc dynamiczne tworzenie pakietów jest obejmują statycznych JavaScript, a także inne pliki w językach, które kompiluje na język JavaScript, a w związku z tym wymagają niektórych przetwarzania przed wykonaniem tworzenie pakietów.</span><span class="sxs-lookup"><span data-stu-id="dd1f0-497">The power of dynamic bundling is that you can include static JavaScript, as well as other files in languages that compiles into JavaScript, and thus, require some processing before the bundling is executed.</span></span>

<span data-ttu-id="dd1f0-498">W w tym przykładzie przedstawiono sposób użycia **DynamicFolderBundle** klasa do tworzenia dynamicznych pakietu plików napisana CofeeScript.</span><span class="sxs-lookup"><span data-stu-id="dd1f0-498">In this example, you will learn how to use the **DynamicFolderBundle** class to create a dynamic bundle for files written in CofeeScript.</span></span> <span data-ttu-id="dd1f0-499">CofeeScript jest język programowania, który kompiluje się na język JavaScript i zapewnia prostszy składni pisanie kodu JavaScript, pominiemy i czytelność języka JavaScript.</span><span class="sxs-lookup"><span data-stu-id="dd1f0-499">CofeeScript is a programming language that compiles into JavaScript and provides a simpler syntax for writing JavaScript code, enhancing JavaScript's brevity and readability.</span></span>

1. <span data-ttu-id="dd1f0-500">Otwórz **Global.asax.cs** plików i Znajdź **aplikacji\_Start** metody.</span><span class="sxs-lookup"><span data-stu-id="dd1f0-500">Open the **Global.asax.cs** file and locate the **Application\_Start** method.</span></span>
2. <span data-ttu-id="dd1f0-501">Usuń komentarz kodu dynamiczne pakietu, jak pokazano w poniższym kodzie.</span><span class="sxs-lookup"><span data-stu-id="dd1f0-501">Uncomment the dynamic bundle code as shown in the code below.</span></span>

    <span data-ttu-id="dd1f0-502">Definiujesz pakietu dynamiczne folderu, który będzie używany przez **CoffeeMinify** procesora minimalizację niestandardowego, który będzie dotyczyć tylko pliki z &quot; **.coffee** &quot; (rozszerzenia Pliki języka CoffeeScript).</span><span class="sxs-lookup"><span data-stu-id="dd1f0-502">You are defining a dynamic folder bundle that will use the **CoffeeMinify** custom minification processor that will only apply to the files with the &quot;**.coffee**&quot; extension (CoffeeScript files).</span></span> <span data-ttu-id="dd1f0-503">Powiadomienia, której można wzorzec wyszukiwania, aby wybrać pliki pakietów w folderze, takie jak "\*.coffee".</span><span class="sxs-lookup"><span data-stu-id="dd1f0-503">Notice that you can use a search pattern to select the files to bundle within a folder, like '\*.coffee'.</span></span>


~~~
[!code-csharp[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample16.cs)]
~~~
3. <span data-ttu-id="dd1f0-504">Otwórz konsolę Menedżera pakietów NuGet.</span><span class="sxs-lookup"><span data-stu-id="dd1f0-504">Open the NuGet Package Manager Console.</span></span> <span data-ttu-id="dd1f0-505">Aby to zrobić, użyj menu **widoku** | **inne okna** | **Konsola Menedżera pakietów**.</span><span class="sxs-lookup"><span data-stu-id="dd1f0-505">To do this, use the menu **View** | **Other Windows** | **Package Manager Console**.</span></span>
4. <span data-ttu-id="dd1f0-506">W **Konsola Menedżera pakietów,** typu **CoffeeSharp Install-Package** i naciśnij klawisz **ENTER**.</span><span class="sxs-lookup"><span data-stu-id="dd1f0-506">In the **Package Manager Console,** type **Install-Package CoffeeSharp** and press **ENTER**.</span></span>
5. <span data-ttu-id="dd1f0-507">Kliknij przycisk **Pokaż wszystkie pliki** przycisk **Eksploratora rozwiązań** okna</span><span class="sxs-lookup"><span data-stu-id="dd1f0-507">Click the **Show All Files** button in the **Solution Explorer** window</span></span>

    <span data-ttu-id="dd1f0-508">![Wyświetlanie wszystkich plików](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image55.png "wyświetlanie wszystkich plików")</span><span class="sxs-lookup"><span data-stu-id="dd1f0-508">![Showing all files](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image55.png "Showing all files")</span></span>

    <span data-ttu-id="dd1f0-509">*Wyświetlanie wszystkich plików*</span><span class="sxs-lookup"><span data-stu-id="dd1f0-509">*Showing all files*</span></span>
6. <span data-ttu-id="dd1f0-510">Kliknij prawym przyciskiem myszy **CoffeeMinify.cs** w pliku **Eksploratora rozwiązań** i wybierz **Include w projekcie**</span><span class="sxs-lookup"><span data-stu-id="dd1f0-510">Right click the **CoffeeMinify.cs** file in the **Solution Explorer** and select **Include in Project**</span></span>

    <span data-ttu-id="dd1f0-511">![W projekcie umieścić plik CoffeeMinify.cs](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image56.png "dołączyć plik CoffeeMinify.cs w projekcie")</span><span class="sxs-lookup"><span data-stu-id="dd1f0-511">![Include the CoffeeMinify.cs file in the project](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image56.png "Include the CoffeeMinify.cs file in the project")</span></span>

    <span data-ttu-id="dd1f0-512">*Dołączenie pliku CoffeeMinify.cs w projekcie*</span><span class="sxs-lookup"><span data-stu-id="dd1f0-512">*Include the CoffeeMinify.cs file in the project*</span></span>
7. <span data-ttu-id="dd1f0-513">Otwórz **CoffeeMinify.cs** pliku.</span><span class="sxs-lookup"><span data-stu-id="dd1f0-513">Open the **CoffeeMinify.cs** file.</span></span>

    <span data-ttu-id="dd1f0-514">Ta klasa dziedziczy JsMinify do zminimalizowania dane wyjściowe JavaScript wynikające z CoffeeScript kompilacji kodu.</span><span class="sxs-lookup"><span data-stu-id="dd1f0-514">This class inherits from JsMinify to minify the JavaScript output resulting from the CoffeeScript code compilation.</span></span> <span data-ttu-id="dd1f0-515">Wywołuje kompilatora języka CoffeeScript do generowania kodu JavaScript najpierw, a następnie wysyła go do metody JsMinify.Process w celu zminimalizowania wynikowy kod.</span><span class="sxs-lookup"><span data-stu-id="dd1f0-515">It calls the CoffeeScript compiler to generate the JavaScript code first, and then it sends it to the JsMinify.Process method to minify the resulting code.</span></span>


~~~
[!code-csharp[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample17.cs)]
~~~
8. <span data-ttu-id="dd1f0-516">Otwórz **Script1.coffee** i **Script2.coffee** plików ze **skryptów/pakietu** folderu.</span><span class="sxs-lookup"><span data-stu-id="dd1f0-516">Open the **Script1.coffee** and **Script2.coffee** files from the **Scripts/bundle** folder.</span></span>

    <span data-ttu-id="dd1f0-517">Te pliki zostaną uwzględnione kodu CoffeScript ma być kompilowana podczas wykonywania, tworzenie pakietów przy użyciu klasy CoffeeMinify.</span><span class="sxs-lookup"><span data-stu-id="dd1f0-517">These files will include the CoffeScript code to be compiled while performing the bundling with the CoffeeMinify class.</span></span>

    <span data-ttu-id="dd1f0-518">Dla uproszczenia CoffeeScript pliki udostępniane są tylko tym CoffeeScript przykładowy kod.</span><span class="sxs-lookup"><span data-stu-id="dd1f0-518">For simplicity purposes, the CoffeeScript files provided are only including CoffeeScript sample code.</span></span> <span data-ttu-id="dd1f0-519">Komentarze są wyłączone przez proces JsMinify.</span><span class="sxs-lookup"><span data-stu-id="dd1f0-519">The comments are excluded by the JsMinify process.</span></span>

    <span data-ttu-id="dd1f0-520">![Pliki języka CoffeeScript](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image57.png "pliki języka CoffeeScript")</span><span class="sxs-lookup"><span data-stu-id="dd1f0-520">![CoffeeScript files](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image57.png "CoffeeScript files")</span></span>

    <span data-ttu-id="dd1f0-521">*Pliki języka CoffeeScript*</span><span class="sxs-lookup"><span data-stu-id="dd1f0-521">*CoffeeScript files*</span></span>

    > [!NOTE]
    > <span data-ttu-id="dd1f0-522">[CofeeScript](https://github.com/jashkenas/coffeescript/) zapewnia prostszy składni pisanie kodu JavaScript, skrócenia języka JavaScript i czytelność, a także dodawanie innych funkcji, takich jak tablicy zrozumienia i dopasowywanie do wzorca.</span><span class="sxs-lookup"><span data-stu-id="dd1f0-522">[CofeeScript](https://github.com/jashkenas/coffeescript/) provides a simpler syntax for writing JavaScript code, enhancing JavaScript's brevity and readability, as well as adding other features like array comprehension and pattern matching.</span></span>
9. <span data-ttu-id="dd1f0-523">Otwórz **Optimization.aspx** plików i Znajdź łącza pakietu.</span><span class="sxs-lookup"><span data-stu-id="dd1f0-523">Open the **Optimization.aspx** file and locate the bundle links.</span></span>

    <span data-ttu-id="dd1f0-524">Zwróć uwagę, że łącze do **dynamiczne pakietu JS** odwołuje się do **skryptów/pakietu** folder przy użyciu **/kawy** sufiks skonfigurowany dla pakietu folderu dynamicznych.</span><span class="sxs-lookup"><span data-stu-id="dd1f0-524">Notice that the link to **Dynamic JS Bundle** is referencing the **Scripts/bundle** folder by using the **/Coffee** suffix you configured for the dynamic folder bundle.</span></span>


~~~
[!code-aspx[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample18.aspx)]
~~~
10. <span data-ttu-id="dd1f0-525">Naciśnij klawisz **F5** do uruchomienia aplikacji, a następnie przejdź do **optymalizacji** strony.</span><span class="sxs-lookup"><span data-stu-id="dd1f0-525">Press **F5** to run the application, and then navigate to the **Optimization** page.</span></span>
11. <span data-ttu-id="dd1f0-526">Polecenie **dynamiczne pakietu JS** łącze, aby otworzyć wygenerowanego pliku.</span><span class="sxs-lookup"><span data-stu-id="dd1f0-526">Click on the **Dynamic JS Bundle** link to open the generated file.</span></span>

    <span data-ttu-id="dd1f0-527">Należy zauważyć, że zawiera tylko zawartość, która została uwzględniona w tym pakiecie **.coffee** plików.</span><span class="sxs-lookup"><span data-stu-id="dd1f0-527">Notice that the content that was included in this bundle only contains **.coffee** files.</span></span> <span data-ttu-id="dd1f0-528">Można również sprawdzić, czy kod języka CoffeeScript został skompilowany dla JavaScript i linie poza komentarzem został usunięty.</span><span class="sxs-lookup"><span data-stu-id="dd1f0-528">You can also see that the CoffeeScript code was compiled to JavaScript and the commented-out lines has been removed.</span></span>

    <span data-ttu-id="dd1f0-529">![Dynamiczne pliki JS pakietu](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image58.png "JS dynamiczne pliki pakietu")</span><span class="sxs-lookup"><span data-stu-id="dd1f0-529">![Dynamic JS files bundle](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image58.png "Dynamic JS files bundle")</span></span>

    <span data-ttu-id="dd1f0-530">*Dynamiczne pakietu plików JS*</span><span class="sxs-lookup"><span data-stu-id="dd1f0-530">*Dynamic JS files bundle*</span></span>

> [!NOTE]
> <span data-ttu-id="dd1f0-531">Ponadto można wdrożyć tę aplikację systemu Windows Azure Web Sites następujących [dodatek B: publikowania aplikacji ASP.NET MVC 4 przy użyciu narzędzia Web Deploy](#AppendixB).</span><span class="sxs-lookup"><span data-stu-id="dd1f0-531">Additionally, you can deploy this application to Windows Azure Web Sites following [Appendix B: Publishing an ASP.NET MVC 4 Application using Web Deploy](#AppendixB).</span></span>


<a id="Summary"></a>
## <a name="summary"></a><span data-ttu-id="dd1f0-532">Podsumowanie</span><span class="sxs-lookup"><span data-stu-id="dd1f0-532">Summary</span></span>

<span data-ttu-id="dd1f0-533">W tym laboratorium pomaga zrozumieć, co jest nowego w programie ASP.NET i aplikacji sieci Web w programie Visual Studio 2012 oraz sposób korzystać z różnego rodzaju rozszerzeń w programie Visual Studio 2012.</span><span class="sxs-lookup"><span data-stu-id="dd1f0-533">This lab helps you to understand what New in ASP.NET and Web Development in Visual Studio 2012 is and how to take advantage of the variety of enhancements in Visual Studio 2012.</span></span>

<span data-ttu-id="dd1f0-534">Wykonując tego laboratorium Hands-On ma doświadczeń sposób użycia nowe funkcje i ulepszenia w Visual Studio 2012 edytory CSS, JavaScript i HTML.</span><span class="sxs-lookup"><span data-stu-id="dd1f0-534">By completing this Hands-On Lab, you have learnt how to use the new features and improvements in Visual Studio 2012 Editors for CSS, JavaScript and HTML.</span></span> <span data-ttu-id="dd1f0-535">Ponadto zostały wcześniej, jak Visual Studio 2012 implementuje wbudowanych tworzenie pakietów i minimalizowanie.</span><span class="sxs-lookup"><span data-stu-id="dd1f0-535">In addition, you have learnt how Visual Studio 2012 implements built-in bundling and minification.</span></span>

<a id="AppendixA"></a>

<a id="Appendix_A_Installing_Visual_Studio_Express_2012_for_Web"></a>
## <a name="appendix-a-installing-visual-studio-express-2012-for-web"></a><span data-ttu-id="dd1f0-536">Dodatek A: Instalowanie programu Visual Studio Express 2012 for Web</span><span class="sxs-lookup"><span data-stu-id="dd1f0-536">Appendix A: Installing Visual Studio Express 2012 for Web</span></span>

<span data-ttu-id="dd1f0-537">Można zainstalować **Microsoft Visual Studio Express 2012 for Web** lub innym &quot;Express&quot; przy użyciu wersji **[Instalatora platformy sieci Web firmy Microsoft](https://www.microsoft.com/web/downloads/platform.aspx)**.</span><span class="sxs-lookup"><span data-stu-id="dd1f0-537">You can install **Microsoft Visual Studio Express 2012 for Web** or another &quot;Express&quot; version using the **[Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)**.</span></span> <span data-ttu-id="dd1f0-538">Poniższe instrukcje przedstawiono czynności wymagane do zainstalowania *programu Visual studio Express 2012 for Web* przy użyciu *Instalatora platformy sieci Web firmy Microsoft*.</span><span class="sxs-lookup"><span data-stu-id="dd1f0-538">The following instructions guide you through the steps required to install *Visual studio Express 2012 for Web* using *Microsoft Web Platform Installer*.</span></span>

1. <span data-ttu-id="dd1f0-539">Przejdź do [ [ https://go.microsoft.com/?linkid=9810169 ](https://go.microsoft.com/?linkid=9810169) ](https://go.microsoft.com/?linkid=9810169).</span><span class="sxs-lookup"><span data-stu-id="dd1f0-539">Go to [[https://go.microsoft.com/?linkid=9810169](https://go.microsoft.com/?linkid=9810169)](https://go.microsoft.com/?linkid=9810169).</span></span> <span data-ttu-id="dd1f0-540">Alternatywnie, jeśli została już zainstalowana Instalatora platformy sieci Web, można otworzyć go i Wyszukaj produkt &quot; <em>programu Visual Studio Express 2012 for Web z zestawem Windows Azure SDK</em>&quot;.</span><span class="sxs-lookup"><span data-stu-id="dd1f0-540">Alternatively, if you already have installed Web Platform Installer, you can open it and search for the product &quot;<em>Visual Studio Express 2012 for Web with Windows Azure SDK</em>&quot;.</span></span>
2. <span data-ttu-id="dd1f0-541">Polecenie **teraz zainstalować**.</span><span class="sxs-lookup"><span data-stu-id="dd1f0-541">Click on **Install Now**.</span></span> <span data-ttu-id="dd1f0-542">Jeśli nie masz **Instalatora platformy sieci Web** nastąpi przekierowanie do pobrania i zainstalowania go najpierw.</span><span class="sxs-lookup"><span data-stu-id="dd1f0-542">If you do not have **Web Platform Installer** you will be redirected to download and install it first.</span></span>
3. <span data-ttu-id="dd1f0-543">Raz **Instalatora platformy sieci Web** jest otwarty, kliknij przycisk **zainstalować** można uruchomić Instalatora.</span><span class="sxs-lookup"><span data-stu-id="dd1f0-543">Once **Web Platform Installer** is open, click **Install** to start the setup.</span></span>

    <span data-ttu-id="dd1f0-544">![Instalowanie programu Visual Studio Express](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image59.png "instalacji programu Visual Studio Express")</span><span class="sxs-lookup"><span data-stu-id="dd1f0-544">![Install Visual Studio Express](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image59.png "Install Visual Studio Express")</span></span>

    <span data-ttu-id="dd1f0-545">*Instalowanie programu Visual Studio Express*</span><span class="sxs-lookup"><span data-stu-id="dd1f0-545">*Install Visual Studio Express*</span></span>
4. <span data-ttu-id="dd1f0-546">Odczytywanie wszystkich produktów licencji i warunków, a następnie kliknij przycisk **akceptuję** aby kontynuować.</span><span class="sxs-lookup"><span data-stu-id="dd1f0-546">Read all the products' licenses and terms and click **I Accept** to continue.</span></span>

    ![Akceptowanie umowy licencyjnej](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image60.png)

    <span data-ttu-id="dd1f0-548">*Akceptowanie umowy licencyjnej*</span><span class="sxs-lookup"><span data-stu-id="dd1f0-548">*Accepting the license terms*</span></span>
5. <span data-ttu-id="dd1f0-549">Poczekaj na zakończenie procesu pobierania i instalacji.</span><span class="sxs-lookup"><span data-stu-id="dd1f0-549">Wait until the downloading and installation process completes.</span></span>

    ![Postęp instalacji](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image61.png)

    <span data-ttu-id="dd1f0-551">*Postęp instalacji*</span><span class="sxs-lookup"><span data-stu-id="dd1f0-551">*Installation progress*</span></span>
6. <span data-ttu-id="dd1f0-552">Po zakończeniu instalacji kliknij przycisk **Zakończ**.</span><span class="sxs-lookup"><span data-stu-id="dd1f0-552">When the installation completes, click **Finish**.</span></span>

    ![Instalacja została zakończona](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image62.png)

    <span data-ttu-id="dd1f0-554">*Instalacja została zakończona*</span><span class="sxs-lookup"><span data-stu-id="dd1f0-554">*Installation completed*</span></span>
7. <span data-ttu-id="dd1f0-555">Kliknij przycisk **zakończenia** aby zamknąć Instalatora platformy sieci Web.</span><span class="sxs-lookup"><span data-stu-id="dd1f0-555">Click **Exit** to close Web Platform Installer.</span></span>
8. <span data-ttu-id="dd1f0-556">Aby otworzyć program Visual Studio Express for Web, przejdź do **Start** ekranu i zacznij pisać &quot; **VS Express**&quot;, następnie kliknij polecenie **VS Express for Web** Kafelek.</span><span class="sxs-lookup"><span data-stu-id="dd1f0-556">To open Visual Studio Express for Web, go to the **Start** screen and start writing &quot;**VS Express**&quot;, then click on the **VS Express for Web** tile.</span></span>

    ![VS Express for Web kafelka](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image63.png)

    <span data-ttu-id="dd1f0-558">*VS Express for Web kafelka*</span><span class="sxs-lookup"><span data-stu-id="dd1f0-558">*VS Express for Web tile*</span></span>

* * *

<a id="AppendixB"></a>

<a id="Appendix_B_Publishing_an_ASPNET_MVC_4_Application_using_Web_Deploy"></a>
## <a name="appendix-b-publishing-an-aspnet-mvc-4-application-using-web-deploy"></a><span data-ttu-id="dd1f0-559">Dodatek B: publikowania aplikacji ASP.NET MVC 4 przy użyciu narzędzia Web Deploy</span><span class="sxs-lookup"><span data-stu-id="dd1f0-559">Appendix B: Publishing an ASP.NET MVC 4 Application using Web Deploy</span></span>

<span data-ttu-id="dd1f0-560">Ten dodatek opisano sposób tworzenia nowej witryny sieci web z portalu zarządzania pakietu Windows Azure i publikowanie aplikacji, uzyskane wykonując laboratorium, korzystając z funkcji publikowania narzędzia Web Deploy dostarczane przez Windows Azure.</span><span class="sxs-lookup"><span data-stu-id="dd1f0-560">This appendix will show you how to create a new web site from the Windows Azure Management Portal and publish the application you obtained by following the lab, taking advantage of the Web Deploy publishing feature provided by Windows Azure.</span></span>

<a id="Ex1Task1"></a>

<a id="Task_1_-_Creating_a_New_Web_Site_from_the_Windows_Azure_Portal"></a>
#### <a name="task-1---creating-a-new-web-site-from-the-windows-azure-portal"></a><span data-ttu-id="dd1f0-561">Zadanie 1 — Tworzenie nowej witryny sieci Web systemu Windows portalu Azure</span><span class="sxs-lookup"><span data-stu-id="dd1f0-561">Task 1 - Creating a New Web Site from the Windows Azure Portal</span></span>

1. <span data-ttu-id="dd1f0-562">Przejdź do [portalu zarządzania pakietu Windows Azure](https://manage.windowsazure.com/) i zaloguj się przy użyciu poświadczeń Microsoft skojarzonych z Twoją subskrypcją.</span><span class="sxs-lookup"><span data-stu-id="dd1f0-562">Go to the [Windows Azure Management Portal](https://manage.windowsazure.com/) and sign in using the Microsoft credentials associated with your subscription.</span></span>

    > [!NOTE]
    > <span data-ttu-id="dd1f0-563">Z systemu Windows Azure można udostępniać 10 witryn sieci Web platformy ASP.NET bezpłatnie i następnie Skaluj w miarę zwiększania się ruchu.</span><span class="sxs-lookup"><span data-stu-id="dd1f0-563">With Windows Azure you can host 10 ASP.NET Web Sites for free and then scale as your traffic grows.</span></span> <span data-ttu-id="dd1f0-564">Możesz utworzyć konto [tutaj](http://aka.ms/aspnet-hol-azure).</span><span class="sxs-lookup"><span data-stu-id="dd1f0-564">You can sign up [here](http://aka.ms/aspnet-hol-azure).</span></span>

    <span data-ttu-id="dd1f0-565">![Zaloguj się do portalu Windows Azure](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image64.png "Zaloguj się do portalu Windows Azure")</span><span class="sxs-lookup"><span data-stu-id="dd1f0-565">![Log on to Windows Azure portal](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image64.png "Log on to Windows Azure portal")</span></span>

    <span data-ttu-id="dd1f0-566">*Zaloguj się do portalu zarządzania platformy Azure z systemem Windows*</span><span class="sxs-lookup"><span data-stu-id="dd1f0-566">*Log on to Windows Azure Management Portal*</span></span>
2. <span data-ttu-id="dd1f0-567">Kliknij przycisk **nowy** na pasku poleceń.</span><span class="sxs-lookup"><span data-stu-id="dd1f0-567">Click **New** on the command bar.</span></span>

    <span data-ttu-id="dd1f0-568">![Tworzenie nowej witryny sieci Web](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image65.png "tworzenia nowej witryny sieci Web")</span><span class="sxs-lookup"><span data-stu-id="dd1f0-568">![Creating a new Web Site](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image65.png "Creating a new Web Site")</span></span>

    <span data-ttu-id="dd1f0-569">*Tworzenie nowej witryny sieci Web*</span><span class="sxs-lookup"><span data-stu-id="dd1f0-569">*Creating a new Web Site*</span></span>
3. <span data-ttu-id="dd1f0-570">Kliknij przycisk **obliczeniowe** | **witryny sieci Web**.</span><span class="sxs-lookup"><span data-stu-id="dd1f0-570">Click **Compute** | **Web Site**.</span></span> <span data-ttu-id="dd1f0-571">Następnie wybierz **szybkie tworzenie** opcji.</span><span class="sxs-lookup"><span data-stu-id="dd1f0-571">Then select **Quick Create** option.</span></span> <span data-ttu-id="dd1f0-572">Podaj dostępny adres URL dla nowej witryny sieci web, a następnie kliknij przycisk **tworzenie witryny sieci Web**.</span><span class="sxs-lookup"><span data-stu-id="dd1f0-572">Provide an available URL for the new web site and click **Create Web Site**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="dd1f0-573">Witryny sieci Web systemu Windows Azure jest hostem dla aplikacji sieci web w chmurze, które można kontrolować i zarządzanie nimi.</span><span class="sxs-lookup"><span data-stu-id="dd1f0-573">A Windows Azure Web Site is the host for a web application running in the cloud that you can control and manage.</span></span> <span data-ttu-id="dd1f0-574">Opcja szybkie tworzenie umożliwia wdrażanie ukończonej aplikacji sieci web do systemu Windows Azure witryny internetowej z spoza portalu.</span><span class="sxs-lookup"><span data-stu-id="dd1f0-574">The Quick Create option allows you to deploy a completed web application to the Windows Azure Web Site from outside the portal.</span></span> <span data-ttu-id="dd1f0-575">Nie obejmuje kroki konfigurowania bazy danych.</span><span class="sxs-lookup"><span data-stu-id="dd1f0-575">It does not include steps for setting up a database.</span></span>

    <span data-ttu-id="dd1f0-576">![Tworzenie nowej witryny sieci Web przy użyciu szybkie tworzenie](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image66.png "tworzenia nowej witryny sieci Web przy użyciu szybkie tworzenie")</span><span class="sxs-lookup"><span data-stu-id="dd1f0-576">![Creating a new Web Site using Quick Create](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image66.png "Creating a new Web Site using Quick Create")</span></span>

    <span data-ttu-id="dd1f0-577">*Tworzenie nowej witryny sieci Web przy użyciu szybkie tworzenie*</span><span class="sxs-lookup"><span data-stu-id="dd1f0-577">*Creating a new Web Site using Quick Create*</span></span>
4. <span data-ttu-id="dd1f0-578">Poczekaj na nowe **witryny sieci Web** jest tworzony.</span><span class="sxs-lookup"><span data-stu-id="dd1f0-578">Wait until the new **Web Site** is created.</span></span>
5. <span data-ttu-id="dd1f0-579">Po utworzeniu witryny sieci Web kliknij łącze w obszarze **adres URL** kolumny.</span><span class="sxs-lookup"><span data-stu-id="dd1f0-579">Once the Web Site is created click the link under the **URL** column.</span></span> <span data-ttu-id="dd1f0-580">Sprawdź, czy działa nowej witryny sieci Web.</span><span class="sxs-lookup"><span data-stu-id="dd1f0-580">Check that the new Web Site is working.</span></span>

    <span data-ttu-id="dd1f0-581">![Przeglądanie do nowej witryny sieci web](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image67.png "przeglądanie do nowej witryny sieci web")</span><span class="sxs-lookup"><span data-stu-id="dd1f0-581">![Browsing to the new web site](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image67.png "Browsing to the new web site")</span></span>

    <span data-ttu-id="dd1f0-582">*Przeglądanie do nowej witryny sieci web*</span><span class="sxs-lookup"><span data-stu-id="dd1f0-582">*Browsing to the new web site*</span></span>

    <span data-ttu-id="dd1f0-583">![Witryna sieci Web działa](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image68.png "uruchamiania witryny sieci Web")</span><span class="sxs-lookup"><span data-stu-id="dd1f0-583">![Web site running](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image68.png "Web site running")</span></span>

    <span data-ttu-id="dd1f0-584">*Witryna sieci Web uruchomiona*</span><span class="sxs-lookup"><span data-stu-id="dd1f0-584">*Web site running*</span></span>
6. <span data-ttu-id="dd1f0-585">Wróć do portalu i kliknij nazwę witryny sieci web w obszarze **nazwa** kolumny do wyświetlenia strony zarządzania.</span><span class="sxs-lookup"><span data-stu-id="dd1f0-585">Go back to the portal and click the name of the web site under the **Name** column to display the management pages.</span></span>

    <span data-ttu-id="dd1f0-586">![Otwieranie stron witryny sieci web zarządzania](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image69.png "otwieranie stron zarządzania witryny sieci web")</span><span class="sxs-lookup"><span data-stu-id="dd1f0-586">![Opening the web site management pages](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image69.png "Opening the web site management pages")</span></span>

    <span data-ttu-id="dd1f0-587">*Otwieranie stron zarządzania witryny sieci Web*</span><span class="sxs-lookup"><span data-stu-id="dd1f0-587">*Opening the Web Site management pages*</span></span>
7. <span data-ttu-id="dd1f0-588">W **pulpitu nawigacyjnego** w obszarze **szybkiego dostępu** kliknij **pobieranie profilu publikowania** łącza.</span><span class="sxs-lookup"><span data-stu-id="dd1f0-588">In the **Dashboard** page, under the **quick glance** section, click the **Download publish profile** link.</span></span>

    > [!NOTE]
    > <span data-ttu-id="dd1f0-589">*Profilu publikowania* zawiera wszystkie informacje wymagane do publikowania aplikacji sieci web do witryny sieci Web systemu Windows Azure dla każdej metody włączone publikacji.</span><span class="sxs-lookup"><span data-stu-id="dd1f0-589">The *publish profile* contains all of the information required to publish a web application to a Windows Azure website for each enabled publication method.</span></span> <span data-ttu-id="dd1f0-590">Profil publikowania zawiera adresy URL, poświadczenia użytkownika i parametry bazy danych wymagane do nawiązania połączenia i uwierzytelniania dla każdego z punktów końcowych, dla których włączono metoda publikacji.</span><span class="sxs-lookup"><span data-stu-id="dd1f0-590">The publish profile contains the URLs, user credentials and database strings required to connect to and authenticate against each of the endpoints for which a publication method is enabled.</span></span> <span data-ttu-id="dd1f0-591">**Microsoft WebMatrix 2**, **Microsoft Visual Studio Express for Web** i **programu Microsoft Visual Studio 2012** obsługują odczytywanie publikowanie profile do zautomatyzowania te programy Publikowanie aplikacji sieci web do witryn sieci Web systemu Windows Azure.</span><span class="sxs-lookup"><span data-stu-id="dd1f0-591">**Microsoft WebMatrix 2**, **Microsoft Visual Studio Express for Web** and **Microsoft Visual Studio 2012** support reading publish profiles to automate configuration of these programs for publishing web applications to Windows Azure websites.</span></span>

    <span data-ttu-id="dd1f0-592">![Pobieranie witryny sieci web profilu publikowania](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image70.png "pobierania witryny sieci web profilu publikowania")</span><span class="sxs-lookup"><span data-stu-id="dd1f0-592">![Downloading the web site publish profile](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image70.png "Downloading the web site publish profile")</span></span>

    <span data-ttu-id="dd1f0-593">*Pobieranie witryny sieci Web profilu publikowania*</span><span class="sxs-lookup"><span data-stu-id="dd1f0-593">*Downloading the Web Site publish profile*</span></span>
8. <span data-ttu-id="dd1f0-594">Pobierz profil publikowania w znanej lokalizacji.</span><span class="sxs-lookup"><span data-stu-id="dd1f0-594">Download the publish profile file to a known location.</span></span> <span data-ttu-id="dd1f0-595">Dodatkowo w tym ćwiczeniu zobaczysz jak opublikować aplikację sieci web do witryny sieci Web systemu Windows Azure w programie Visual Studio przy użyciu tego pliku.</span><span class="sxs-lookup"><span data-stu-id="dd1f0-595">Further in this exercise you will see how to use this file to publish a web application to a Windows Azure Web Sites from Visual Studio.</span></span>

    <span data-ttu-id="dd1f0-596">![Zapisywanie pliku profilu publikowania](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image71.png "zapisywanie profilu publikowania")</span><span class="sxs-lookup"><span data-stu-id="dd1f0-596">![Saving the publish profile file](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image71.png "Saving the publish profile")</span></span>

    <span data-ttu-id="dd1f0-597">*Zapisywanie pliku profilu publikowania*</span><span class="sxs-lookup"><span data-stu-id="dd1f0-597">*Saving the publish profile file*</span></span>

<a id="Ex1Task2"></a>

<a id="Task_2_-_Configuring_the_Database_Server"></a>
#### <a name="task-2---configuring-the-database-server"></a><span data-ttu-id="dd1f0-598">Zadanie 2 — Konfigurowanie serwera bazy danych</span><span class="sxs-lookup"><span data-stu-id="dd1f0-598">Task 2 - Configuring the Database Server</span></span>

<span data-ttu-id="dd1f0-599">Jeśli aplikacja korzysta z programu SQL Server baz danych, należy utworzyć serwer bazy danych SQL.</span><span class="sxs-lookup"><span data-stu-id="dd1f0-599">If your application makes use of SQL Server databases you will need to create a SQL Database server.</span></span> <span data-ttu-id="dd1f0-600">Jeśli chcesz wdrożyć prostą aplikację, która nie korzysta z programu SQL Server może pominąć to zadanie.</span><span class="sxs-lookup"><span data-stu-id="dd1f0-600">If you want to deploy a simple application that does not use SQL Server you might skip this task.</span></span>

1. <span data-ttu-id="dd1f0-601">Będzie potrzebny serwer bazy danych SQL do przechowywania bazy danych aplikacji.</span><span class="sxs-lookup"><span data-stu-id="dd1f0-601">You will need a SQL Database server for storing the application database.</span></span> <span data-ttu-id="dd1f0-602">Można wyświetlić serwery bazy danych SQL z subskrypcji usługi Windows Azure Management Portal pod adresem **baz danych Sql** | **serwerów** | **serwera Pulpit nawigacyjny**.</span><span class="sxs-lookup"><span data-stu-id="dd1f0-602">You can view the SQL Database servers from your subscription in the Windows Azure Management portal at **Sql Databases** | **Servers** | **Server's Dashboard**.</span></span> <span data-ttu-id="dd1f0-603">Jeśli nie masz serwer, który został utworzony, można utworzyć przy użyciu jednego **Dodaj** przycisk paska poleceń.</span><span class="sxs-lookup"><span data-stu-id="dd1f0-603">If you do not have a server created, you can create one using the **Add** button on the command bar.</span></span> <span data-ttu-id="dd1f0-604">Zwróć uwagę na **nazwę serwera i adres URL, nazwę logowania administratora i hasła**, jak będą używane w następnego zadania.</span><span class="sxs-lookup"><span data-stu-id="dd1f0-604">Take note of the **server name and URL, administrator login name and password**, as you will use them in the next tasks.</span></span> <span data-ttu-id="dd1f0-605">Nie należy tworzyć bazy danych jeszcze, jako zostaną utworzone w późniejszym terminie.</span><span class="sxs-lookup"><span data-stu-id="dd1f0-605">Do not create the database yet, as it will be created in a later stage.</span></span>

    <span data-ttu-id="dd1f0-606">![Pulpit nawigacyjny serwera bazy danych SQL](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image72.png "pulpitu nawigacyjnego serwera bazy danych SQL")</span><span class="sxs-lookup"><span data-stu-id="dd1f0-606">![SQL Database Server Dashboard](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image72.png "SQL Database Server Dashboard")</span></span>

    <span data-ttu-id="dd1f0-607">*Pulpit nawigacyjny serwera bazy danych SQL*</span><span class="sxs-lookup"><span data-stu-id="dd1f0-607">*SQL Database Server Dashboard*</span></span>
2. <span data-ttu-id="dd1f0-608">W następnym zadaniem Testuj połączenie z bazą danych z programu Visual Studio z tego powodu należy uwzględnić lokalny adres IP serwera liście **dozwolone adresy IP**.</span><span class="sxs-lookup"><span data-stu-id="dd1f0-608">In the next task you will test the database connection from Visual Studio, for that reason you need to include your local IP address in the server's list of **Allowed IP Addresses**.</span></span> <span data-ttu-id="dd1f0-609">Aby to zrobić, kliknij przycisk **Konfiguruj**, wybierz adres IP z **bieżącego adresu IP klienta** i wklej go na **początkowy adres IP** i **końcowy adres IP** pól tekstowych.</span><span class="sxs-lookup"><span data-stu-id="dd1f0-609">To do that, click **Configure**, select the IP address from **Current Client IP Address** and paste it on the **Start IP Address** and **End IP Address** text boxes.</span></span> <span data-ttu-id="dd1f0-610">Wprowadź nazwę reguły, a następnie kliknij przycisk ![add-client-ip-address-ok-button](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image73.png) przycisku.</span><span class="sxs-lookup"><span data-stu-id="dd1f0-610">Enter a name for the rule and click the ![add-client-ip-address-ok-button](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image73.png) button.</span></span>

    ![Dodawanie adresu IP klienta](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image74.png)

    <span data-ttu-id="dd1f0-612">*Dodawanie adresu IP klienta*</span><span class="sxs-lookup"><span data-stu-id="dd1f0-612">*Adding Client IP Address*</span></span>
3. <span data-ttu-id="dd1f0-613">Raz **adres IP klienta** jest dodawany do dozwolonych adresów IP kliknij na **zapisać** o potwierdzenie zmian.</span><span class="sxs-lookup"><span data-stu-id="dd1f0-613">Once the **Client IP Address** is added to the allowed IP addresses list, click on **Save** to confirm the changes.</span></span>

    ![Potwierdzenie zmian](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image75.png)

    <span data-ttu-id="dd1f0-615">*Potwierdzenie zmian*</span><span class="sxs-lookup"><span data-stu-id="dd1f0-615">*Confirm Changes*</span></span>

<a id="Ex1Task3"></a>

<a id="Task_3_-_Publishing_an_ASPNET_MVC_4_Application_using_Web_Deploy"></a>
#### <a name="task-3---publishing-an-aspnet-mvc-4-application-using-web-deploy"></a><span data-ttu-id="dd1f0-616">Zadanie 3 - publikowania aplikacji ASP.NET MVC 4 przy użyciu narzędzia Web Deploy</span><span class="sxs-lookup"><span data-stu-id="dd1f0-616">Task 3 - Publishing an ASP.NET MVC 4 Application using Web Deploy</span></span>

1. <span data-ttu-id="dd1f0-617">Wróć do rozwiązania ASP.NET MVC 4.</span><span class="sxs-lookup"><span data-stu-id="dd1f0-617">Go back to the ASP.NET MVC 4 solution.</span></span> <span data-ttu-id="dd1f0-618">W **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy projekt witryny sieci web i wybierz **publikowania**.</span><span class="sxs-lookup"><span data-stu-id="dd1f0-618">In the **Solution Explorer**, right-click the web site project and select **Publish**.</span></span>

    <span data-ttu-id="dd1f0-619">![Publikowanie aplikacji](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image76.png "publikowania aplikacji")</span><span class="sxs-lookup"><span data-stu-id="dd1f0-619">![Publishing the Application](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image76.png "Publishing the Application")</span></span>

    <span data-ttu-id="dd1f0-620">*Publikowanie witryny sieci web*</span><span class="sxs-lookup"><span data-stu-id="dd1f0-620">*Publishing the web site*</span></span>
2. <span data-ttu-id="dd1f0-621">Zaimportuj profil publikowania, zapisana w pierwszym zadaniu.</span><span class="sxs-lookup"><span data-stu-id="dd1f0-621">Import the publish profile you saved in the first task.</span></span>

    <span data-ttu-id="dd1f0-622">![Importowanie profilu publikowania](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image77.png "Importowanie profilu publikowania")</span><span class="sxs-lookup"><span data-stu-id="dd1f0-622">![Importing the publish profile](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image77.png "Importing the publish profile")</span></span>

    <span data-ttu-id="dd1f0-623">*Importowanie profilu publikowania*</span><span class="sxs-lookup"><span data-stu-id="dd1f0-623">*Importing publish profile*</span></span>
3. <span data-ttu-id="dd1f0-624">Kliknij przycisk **Weryfikacja połączenia z**.</span><span class="sxs-lookup"><span data-stu-id="dd1f0-624">Click **Validate Connection**.</span></span> <span data-ttu-id="dd1f0-625">Po zakończeniu sprawdzania kliknij **dalej**.</span><span class="sxs-lookup"><span data-stu-id="dd1f0-625">Once Validation is complete click **Next**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="dd1f0-626">Zakończeniu sprawdzania poprawności, gdy zostanie wyświetlony zielony znacznik wyboru są wyświetlane obok przycisku sprawdzania poprawności połączenia.</span><span class="sxs-lookup"><span data-stu-id="dd1f0-626">Validation is complete once you see a green checkmark appear next to the Validate Connection button.</span></span>

    <span data-ttu-id="dd1f0-627">![Sprawdzanie poprawności połączenia](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image78.png "sprawdzanie poprawności połączenia")</span><span class="sxs-lookup"><span data-stu-id="dd1f0-627">![Validating connection](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image78.png "Validating connection")</span></span>

    <span data-ttu-id="dd1f0-628">*Sprawdzanie poprawności połączenia*</span><span class="sxs-lookup"><span data-stu-id="dd1f0-628">*Validating connection*</span></span>
4. <span data-ttu-id="dd1f0-629">W **ustawienia** w obszarze **baz danych** sekcji, kliknij przycisk Dalej, aby textbox połączenia bazy danych (tj. **połączenia DefaultConnection**).</span><span class="sxs-lookup"><span data-stu-id="dd1f0-629">In the **Settings** page, under the **Databases** section, click the button next to your database connection's textbox (i.e. **DefaultConnection**).</span></span>

    <span data-ttu-id="dd1f0-630">![Konfiguracja narzędzia Web deploy](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image79.png "Konfiguracja narzędzia Web deploy")</span><span class="sxs-lookup"><span data-stu-id="dd1f0-630">![Web deploy configuration](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image79.png "Web deploy configuration")</span></span>

    <span data-ttu-id="dd1f0-631">*Konfiguracja narzędzia Web deploy*</span><span class="sxs-lookup"><span data-stu-id="dd1f0-631">*Web deploy configuration*</span></span>
5. <span data-ttu-id="dd1f0-632">Skonfiguruj połączenie z bazą danych w następujący sposób:</span><span class="sxs-lookup"><span data-stu-id="dd1f0-632">Configure the database connection as follows:</span></span>

   - <span data-ttu-id="dd1f0-633">W **nazwy serwera** wpisz swoją bazą danych SQL server adresu URL przy użyciu *tcp:* prefiks.</span><span class="sxs-lookup"><span data-stu-id="dd1f0-633">In the **Server name** type your SQL Database server URL using the *tcp:* prefix.</span></span>
   - <span data-ttu-id="dd1f0-634">W **nazwy użytkownika** wpisz nazwę logowania administratora serwera.</span><span class="sxs-lookup"><span data-stu-id="dd1f0-634">In **User name** type your server administrator login name.</span></span>
   - <span data-ttu-id="dd1f0-635">W **hasło** wpisz hasło logowania administratora serwera.</span><span class="sxs-lookup"><span data-stu-id="dd1f0-635">In **Password** type your server administrator login password.</span></span>
   - <span data-ttu-id="dd1f0-636">Wpisz nazwę nowej bazy danych, na przykład: *MVC4SampleDB*.</span><span class="sxs-lookup"><span data-stu-id="dd1f0-636">Type a new database name, for example: *MVC4SampleDB*.</span></span>

     <span data-ttu-id="dd1f0-637">![Konfigurowanie parametrów połączenia z lokalizacją docelową](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image80.png "Konfigurowanie parametrów połączenia z lokalizacją docelową")</span><span class="sxs-lookup"><span data-stu-id="dd1f0-637">![Configuring destination connection string](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image80.png "Configuring destination connection string")</span></span>

     <span data-ttu-id="dd1f0-638">*Konfigurowanie parametrów połączenia z lokalizacją docelową*</span><span class="sxs-lookup"><span data-stu-id="dd1f0-638">*Configuring destination connection string*</span></span>
6. <span data-ttu-id="dd1f0-639">Następnie kliknij przycisk **OK**.</span><span class="sxs-lookup"><span data-stu-id="dd1f0-639">Then click **OK**.</span></span> <span data-ttu-id="dd1f0-640">Po wyświetleniu monitu można utworzyć bazy danych kliknij **tak**.</span><span class="sxs-lookup"><span data-stu-id="dd1f0-640">When prompted to create the database click **Yes**.</span></span>

    <span data-ttu-id="dd1f0-641">![Tworzenie bazy danych](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image81.png "tworzenie parametry bazy danych")</span><span class="sxs-lookup"><span data-stu-id="dd1f0-641">![Creating the database](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image81.png "Creating the database string")</span></span>

    <span data-ttu-id="dd1f0-642">*Tworzenie bazy danych*</span><span class="sxs-lookup"><span data-stu-id="dd1f0-642">*Creating the database*</span></span>
7. <span data-ttu-id="dd1f0-643">Ciągu połączenia używanego do łączenia z bazą danych SQL w systemie Windows Azure jest wyświetlany w pole tekstowe domyślne połączenie.</span><span class="sxs-lookup"><span data-stu-id="dd1f0-643">The connection string you will use to connect to SQL Database in Windows Azure is shown within Default Connection textbox.</span></span> <span data-ttu-id="dd1f0-644">Następnie kliknij przycisk **Dalej**.</span><span class="sxs-lookup"><span data-stu-id="dd1f0-644">Then click **Next**.</span></span>

    <span data-ttu-id="dd1f0-645">![Parametry połączenia wskazujące bazę danych SQL](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image82.png "ciąg połączenia wskazujące bazę danych SQL")</span><span class="sxs-lookup"><span data-stu-id="dd1f0-645">![Connection string pointing to SQL Database](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image82.png "Connection string pointing to SQL Database")</span></span>

    <span data-ttu-id="dd1f0-646">*Parametry połączenia wskazujące bazę danych SQL*</span><span class="sxs-lookup"><span data-stu-id="dd1f0-646">*Connection string pointing to SQL Database*</span></span>
8. <span data-ttu-id="dd1f0-647">W **Podgląd** kliknij przycisk **publikowania**.</span><span class="sxs-lookup"><span data-stu-id="dd1f0-647">In the **Preview** page, click **Publish**.</span></span>

    <span data-ttu-id="dd1f0-648">![Publikowanie aplikacji sieci web](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image83.png "publikowania aplikacji sieci web")</span><span class="sxs-lookup"><span data-stu-id="dd1f0-648">![Publishing the web application](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image83.png "Publishing the web application")</span></span>

    <span data-ttu-id="dd1f0-649">*Publikowanie aplikacji sieci web*</span><span class="sxs-lookup"><span data-stu-id="dd1f0-649">*Publishing the web application*</span></span>
9. <span data-ttu-id="dd1f0-650">Po zakończeniu procesu publikowania domyślnej przeglądarce otworzy opublikowanej witryny sieci web.</span><span class="sxs-lookup"><span data-stu-id="dd1f0-650">Once the publishing process finishes, your default browser will open the published web site.</span></span>

    <span data-ttu-id="dd1f0-651">![Aplikacja opublikowana w systemie Windows Azure](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image84.png "aplikacji publikowanych w systemie Windows Azure")</span><span class="sxs-lookup"><span data-stu-id="dd1f0-651">![Application published to Windows Azure](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image84.png "Application published to Windows Azure")</span></span>

    <span data-ttu-id="dd1f0-652">*Aplikacji publikowanych w systemie Windows Azure*</span><span class="sxs-lookup"><span data-stu-id="dd1f0-652">*Application published to Windows Azure*</span></span>
