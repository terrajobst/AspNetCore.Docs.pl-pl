---
uid: web-forms/overview/getting-started/hands-on-labs/whats-new-in-aspnet-and-web-development-in-visual-studio-2012
title: What's New in ASP.NET i aplikacji sieci Web w programie Visual Studio 2012 | Dokumentacja firmy Microsoft
author: rick-anderson
description: "Nowa wersja programu Visual Studio wprowadzono wiele ulepszeń skoncentrowane na ułatwieniu środowisko i wydajności podczas pracy z technologii sieci Web..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/18/2013
ms.topic: article
ms.assetid: 6d40d276-1642-4a77-b6c9-02ac914f6805
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/getting-started/hands-on-labs/whats-new-in-aspnet-and-web-development-in-visual-studio-2012
msc.type: authoredcontent
ms.openlocfilehash: f0818cce2a82ede80556b3471cec9d965c3e987f
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/24/2018
---
<a name="whats-new-in-aspnet-and-web-development-in-visual-studio-2012"></a><span data-ttu-id="1cec9-103">What's New in ASP.NET i aplikacji sieci Web w programie Visual Studio 2012</span><span class="sxs-lookup"><span data-stu-id="1cec9-103">What's New in ASP.NET and Web Development in Visual Studio 2012</span></span>
====================
<span data-ttu-id="1cec9-104">przez [obozów sieci Web Team](https://twitter.com/webcamps)</span><span class="sxs-lookup"><span data-stu-id="1cec9-104">by [Web Camps Team](https://twitter.com/webcamps)</span></span>

> <span data-ttu-id="1cec9-105">Nowa wersja programu Visual Studio wprowadzono wiele ulepszeń skoncentrowane na ułatwieniu środowisko i wydajności podczas pracy z technologii sieci Web.</span><span class="sxs-lookup"><span data-stu-id="1cec9-105">The new version of Visual Studio introduces a number of enhancements focused on improving the experience and performance when working with Web technologies.</span></span> <span data-ttu-id="1cec9-106">Visual Studio edytory CSS, JavaScript i HTML ma została utworzona całkowicie od nowa do obejmuje wiele najbardziej w żądanie pomocy kodu, takie jak IntelliSense i automatyczne wcięcia.</span><span class="sxs-lookup"><span data-stu-id="1cec9-106">Visual Studio Editors for CSS, JavaScript and HTML have been completely revamped to include many of the most in-demand code aids, such as IntelliSense and automatic indentation.</span></span> <span data-ttu-id="1cec9-107">Dotyczące wydajności tworzenie pakietów i minimalizowanie są teraz zintegrowane jak czas ładowania wbudowanych funkcji, aby łatwo ograniczyć strony.</span><span class="sxs-lookup"><span data-stu-id="1cec9-107">Regarding performance, bundling and minification are now integrated as built-in features to easily reduce page load time.</span></span>
> 
> <span data-ttu-id="1cec9-108">Program Visual Studio umożliwia pracę z najnowszych technologii witryny sieci Web.</span><span class="sxs-lookup"><span data-stu-id="1cec9-108">Visual Studio enables you to work with the latest website technologies.</span></span> <span data-ttu-id="1cec9-109">Fragmenty kodu CSS3 różnych przeglądarkach umożliwia upewnij się, że witryna działa bez względu na platformę klienta również wykorzystując nowe elementy HTML5 i funkcje.</span><span class="sxs-lookup"><span data-stu-id="1cec9-109">You can use cross-browser CSS3 Snippets to make sure your site works regardless of the client platform while also taking advantage of the new HTML5 elements and features.</span></span>
> 
> <span data-ttu-id="1cec9-110">Zapisywanie i profilowanie kodu JavaScript powinny być łatwiejsze w przypadku tej wersji programu Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="1cec9-110">Writing and profiling JavaScript code should be easier with this Visual Studio version.</span></span> <span data-ttu-id="1cec9-111">Listy IntelliSense, zintegrowane funkcje dokumentacji i nawigacja XML są teraz dostępne dla kodu JavaScript.</span><span class="sxs-lookup"><span data-stu-id="1cec9-111">IntelliSense lists, integrated XML documentation and navigation features are now available for JavaScript code.</span></span> <span data-ttu-id="1cec9-112">Masz teraz katalogu JavaScript w zasięgu ręki.</span><span class="sxs-lookup"><span data-stu-id="1cec9-112">You now have the JavaScript catalog at your fingertips.</span></span> <span data-ttu-id="1cec9-113">Ponadto można sprawdzić zgodność ECMAScript5 za pomocą tych skryptów i wykrywania błędów składni na wczesnym etapie.</span><span class="sxs-lookup"><span data-stu-id="1cec9-113">Additionally, you can check ECMAScript5 compliance with your scripts and detect syntax errors at an early stage.</span></span>
> 
> <span data-ttu-id="1cec9-114">Ostatnio, ale nie najmniej tej wersji programu Visual Studio implementuje wbudowanych tworzenie pakietów i minimalizowanie.</span><span class="sxs-lookup"><span data-stu-id="1cec9-114">Last but not least, this Visual Studio version implements built-in bundling and minification.</span></span> <span data-ttu-id="1cec9-115">Pliki skryptów i arkusze stylów zostanie spakowane i skompresować tak, aby lokacji wykonuje szybciej.</span><span class="sxs-lookup"><span data-stu-id="1cec9-115">Your script files and style sheets will be packed and compressed so that the site performs faster.</span></span>
> 
> <span data-ttu-id="1cec9-116">W tym laboratorium przeprowadzi Cię przez rozszerzenia oraz nowe funkcje opisane wcześniej przez zastosowanie drobne zmiany do przykładowej aplikacji sieci Web w folderze źródłowym.</span><span class="sxs-lookup"><span data-stu-id="1cec9-116">This lab walks you through the enhancements and new features previously described by applying minor changes to a sample Web application provided in the Source folder.</span></span>
> 
> <span data-ttu-id="1cec9-117">Wszystkie przykładowy kod i fragmenty kodu są uwzględnione w sieci Web obozów zestaw szkoleniowy, dostępne pod adresem [https://go.microsoft.com/fwlink/?LinkID=248297&clcid=0x409](https://go.microsoft.com/fwlink/?LinkID=248297&clcid=0x409).</span><span class="sxs-lookup"><span data-stu-id="1cec9-117">All sample code and snippets are included in the Web Camps Training Kit, available at [https://go.microsoft.com/fwlink/?LinkID=248297&clcid=0x409](https://go.microsoft.com/fwlink/?LinkID=248297&clcid=0x409).</span></span>


<a id="Objectives"></a>

<a id="Objectives"></a>
### <a name="objectives"></a><span data-ttu-id="1cec9-118">Cele</span><span class="sxs-lookup"><span data-stu-id="1cec9-118">Objectives</span></span>

<span data-ttu-id="1cec9-119">W tym ręce w laboratorium dowiesz się jak:</span><span class="sxs-lookup"><span data-stu-id="1cec9-119">In this hands on lab, you will learn how to:</span></span>

- <span data-ttu-id="1cec9-120">Użyj nowe funkcje i ulepszenia w edytorze CSS</span><span class="sxs-lookup"><span data-stu-id="1cec9-120">Use the new features and improvements in the CSS editor</span></span>
- <span data-ttu-id="1cec9-121">Użyj nowe funkcje i ulepszenia w edytorze HTML</span><span class="sxs-lookup"><span data-stu-id="1cec9-121">Use the new features and improvements in the HTML editor</span></span>
- <span data-ttu-id="1cec9-122">Użyj nowe funkcje i ulepszenia w edytorze kodu JavaScript</span><span class="sxs-lookup"><span data-stu-id="1cec9-122">Use the new features and improvements in the JavaScript editor</span></span>
- <span data-ttu-id="1cec9-123">Konfigurowanie i używanie tworzenie pakietów i minimalizowanie</span><span class="sxs-lookup"><span data-stu-id="1cec9-123">Configure and use bundling and minification</span></span>

<a id="Prerequisites"></a>

<a id="Prerequisites"></a>
### <a name="prerequisites"></a><span data-ttu-id="1cec9-124">Wymagania wstępne</span><span class="sxs-lookup"><span data-stu-id="1cec9-124">Prerequisites</span></span>

- <span data-ttu-id="1cec9-125">[Microsoft Visual Studio Express 2012 for Web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web) lub wyższego poziomu (odczytu [dodatek a.](#AppendixA) instrukcje dotyczące sposobu jego instalacji).</span><span class="sxs-lookup"><span data-stu-id="1cec9-125">[Microsoft Visual Studio Express 2012 for Web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web) or superior (read [Appendix A](#AppendixA) for instructions on how to install it).</span></span>
- <span data-ttu-id="1cec9-126">[Środowisko Windows PowerShell](https://support.microsoft.com/kb/968930/) (w przypadku skryptów instalacji — już zainstalowanym systemem Windows 8 i Windows Server 2008 R2)</span><span class="sxs-lookup"><span data-stu-id="1cec9-126">[Windows PowerShell](https://support.microsoft.com/kb/968930/) (for setup scripts - already installed on Windows 8 and Windows Server 2008 R2)</span></span>
- <span data-ttu-id="1cec9-127">[Internet Explorer 10](https://windows.microsoft.com/internet-explorer/products/ie/home) - lub HTML5 przeglądarki zgodne</span><span class="sxs-lookup"><span data-stu-id="1cec9-127">[Internet Explorer 10](https://windows.microsoft.com/internet-explorer/products/ie/home) - or an HTML5 compliant browser</span></span>

<a id="Exercises"></a>

<a id="Exercises"></a>
## <a name="exercises"></a><span data-ttu-id="1cec9-128">Ćwiczenia</span><span class="sxs-lookup"><span data-stu-id="1cec9-128">Exercises</span></span>

<span data-ttu-id="1cec9-129">Wskazówki tego laboratorium obejmuje następujących czynnościach:</span><span class="sxs-lookup"><span data-stu-id="1cec9-129">This hands on lab includes the following exercises:</span></span>

1. [<span data-ttu-id="1cec9-130">Ćwiczenie 1: Nowości w edytorze CSS</span><span class="sxs-lookup"><span data-stu-id="1cec9-130">Exercise 1: What's New in the CSS Editor</span></span>](#Exercise1)
2. [<span data-ttu-id="1cec9-131">Ćwiczenie 2: Nowości w edytorze HTML</span><span class="sxs-lookup"><span data-stu-id="1cec9-131">Exercise 2: What's New in the HTML Editor</span></span>](#Exercise2)
3. [<span data-ttu-id="1cec9-132">Ćwiczenie 3: Nowości w edytorze kodu JavaScript</span><span class="sxs-lookup"><span data-stu-id="1cec9-132">Exercise 3: What's New in the JavaScript Editor</span></span>](#Exercise3)
4. [<span data-ttu-id="1cec9-133">Ćwiczenie 4: Tworzenie pakietów i minimalizowanie</span><span class="sxs-lookup"><span data-stu-id="1cec9-133">Exercise 4: Bundling and Minification</span></span>](#Exercise4)

<span data-ttu-id="1cec9-134">Szacowany czas trwania tego laboratorium: **60 minut**.</span><span class="sxs-lookup"><span data-stu-id="1cec9-134">Estimated time to complete this lab: **60 minutes**.</span></span>

<a id="Exercise1"></a>

<a id="Exercise_1_Whats_New_in_the_CSS_Editor"></a>
### <a name="exercise-1-whats-new-in-the-css-editor"></a><span data-ttu-id="1cec9-135">Ćwiczenie 1: Nowości w edytorze CSS</span><span class="sxs-lookup"><span data-stu-id="1cec9-135">Exercise 1: What's New in the CSS Editor</span></span>

<span data-ttu-id="1cec9-136">Deweloperzy sieci Web należy zapoznać się z wielu trudności związane z CSS edycji.</span><span class="sxs-lookup"><span data-stu-id="1cec9-136">Web developers should be familiar with many of the difficulties related to CSS editing.</span></span> <span data-ttu-id="1cec9-137">Jedną z największych problemy style CSS jest pomiędzy przeglądarkami.</span><span class="sxs-lookup"><span data-stu-id="1cec9-137">One of the biggest issues of CSS styling is cross-browser compatibility.</span></span> <span data-ttu-id="1cec9-138">Często zdarza się, że po zastosowaniu stylów do swojej witryny, można zauważyć, że wygląda inaczej po otwarciu w innej przeglądarki lub urządzenia.</span><span class="sxs-lookup"><span data-stu-id="1cec9-138">It often happens that, after applying styles to your site, you notice that it looks different if you open it in another browser or device.</span></span> <span data-ttu-id="1cec9-139">W związku z tym może poświęcać przez dłuższy czas na rozwiązywania tych problemów visual sobie sprawę, że koniec powoduje ona działać w jednej przeglądarki, jest on uszkodzony na innych.</span><span class="sxs-lookup"><span data-stu-id="1cec9-139">Therefore, you may spend a considerable time on fixing those visual issues to realize that, when you finally make it work in one browser, it is broken in the others.</span></span>

<span data-ttu-id="1cec9-140">Visual Studio teraz obejmuje funkcje, które ułatwiają deweloperom dostęp, praca i efektywnie zorganizować arkusze stylów CSS.</span><span class="sxs-lookup"><span data-stu-id="1cec9-140">Visual Studio now includes features that help developers access, work and organize CSS style sheets effectively.</span></span> <span data-ttu-id="1cec9-141">W ramach tego ćwiczenia będzie spełniać nowych funkcji dla wersji i skuteczne organizacji, a także fragmenty kodu CSS3 zgodności różnych przeglądarkach.</span><span class="sxs-lookup"><span data-stu-id="1cec9-141">Throughout this exercise, you will meet the new features for an effective organization and edition, as well as the CSS3 Code Snippets for cross-browser compatibility.</span></span>

<a id="Ex1Task1"></a>

<a id="Task_1_-_New_Editor_Features"></a>
#### <a name="task-1---new-editor-features"></a><span data-ttu-id="1cec9-142">Zadanie 1 — nowe funkcje edytora</span><span class="sxs-lookup"><span data-stu-id="1cec9-142">Task 1 - New Editor Features</span></span>

<span data-ttu-id="1cec9-143">W tym zadaniu możesz zauważyć nowe funkcje Edytor CSS.</span><span class="sxs-lookup"><span data-stu-id="1cec9-143">In this task, you will discover the new features of the CSS Editor.</span></span> <span data-ttu-id="1cec9-144">Ten nowy edytor pomoże Ci zwiększyć produktywność dzięki wykorzystaniu nowych inteligentne wcięcie, komentarze w kodzie ulepszone i rozszerzone listy IntelliSense.</span><span class="sxs-lookup"><span data-stu-id="1cec9-144">This new editor will help you increase your productivity by taking advantage of the new smart indentation, the improved code comments and the enhanced IntelliSense list.</span></span>

1. <span data-ttu-id="1cec9-145">Uruchom **programu Visual Studio** , a następnie otwórz **WhatsNewASPNET.sln** rozwiązania, znajdujących się w **Source\WhatsNewASPNET** folder tego laboratorium.</span><span class="sxs-lookup"><span data-stu-id="1cec9-145">Start **Visual Studio** and open the **WhatsNewASPNET.sln** solution located in the **Source\WhatsNewASPNET** folder of this lab.</span></span>
2. <span data-ttu-id="1cec9-146">W Eksploratorze rozwiązań Otwórz **Site.css** plik znajdujący się w obszarze **style** folderu.</span><span class="sxs-lookup"><span data-stu-id="1cec9-146">In Solution Explorer, open the **Site.css** file located under the **Styles** folder.</span></span> <span data-ttu-id="1cec9-147">Upewnij się, że **Edytor tekstu** narzędzia są widoczne na pasku narzędzi.</span><span class="sxs-lookup"><span data-stu-id="1cec9-147">Make sure the **Text Editor** tools are visible on the toolbar.</span></span> <span data-ttu-id="1cec9-148">W tym celu wybierz **widoku** | **paski narzędzi** opcji menu, a także sprawdź **Edytor tekstu** opcje.</span><span class="sxs-lookup"><span data-stu-id="1cec9-148">To do that, select the **View** | **Toolbars** menu option, and check the **Text Editor** options.</span></span> <span data-ttu-id="1cec9-149">Można zauważyć, że od tej nowej wersji **komentarz** przycisk (![przycisk komentarz](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image1.png) ) i **Usuń komentarz** przycisk (![przycisk Usuń znaczniki komentarza](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image2.png)) również są włączone dla Edytor CSS.</span><span class="sxs-lookup"><span data-stu-id="1cec9-149">You will notice that, since this new version, the **Comment** button (![comment-button](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image1.png) ) and the **Uncomment** button (![uncomment-button](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image2.png) ) are also enabled for the CSS editor.</span></span>

    <span data-ttu-id="1cec9-150">![Włączanie edytora i narzędzia CSS](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image3.png "włączenie edytora i narzędzia CSS")</span><span class="sxs-lookup"><span data-stu-id="1cec9-150">![Enabling Editor and CSS Tools](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image3.png "Enabling Editor and CSS Tools")</span></span>

    <span data-ttu-id="1cec9-151">*Włączanie edytora i narzędzia CSS*</span><span class="sxs-lookup"><span data-stu-id="1cec9-151">*Enabling Editor and CSS Tools*</span></span>
3. <span data-ttu-id="1cec9-152">Przewiń w kodzie i wybierz żadnych definicji klas CSS.</span><span class="sxs-lookup"><span data-stu-id="1cec9-152">Scroll the code and select any CSS class definition.</span></span> <span data-ttu-id="1cec9-153">Kliknij przycisk **komentarz** (![przycisk komentarz](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image4.png) ) przycisk, aby komentarz dotyczący wybranych wierszy.</span><span class="sxs-lookup"><span data-stu-id="1cec9-153">Click the **Comment** (![comment-button](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image4.png) ) button to comment the selected lines.</span></span> <span data-ttu-id="1cec9-154">Następnie kliknij przycisk **Usuń komentarz** (![przycisk Usuń znaczniki komentarza](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image5.png) ) przycisk, aby cofnąć zmiany.</span><span class="sxs-lookup"><span data-stu-id="1cec9-154">Then, click the **Uncomment** (![uncomment-button](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image5.png) ) button to undo the changes.</span></span>
4. <span data-ttu-id="1cec9-155">Kliknij przycisk **Zwiń** (![Zwiń](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image6.png) ) i **rozwiń** (![rozwiń](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image7.png) ) przycisków znajdujących się na lewym marginesie tekstu.</span><span class="sxs-lookup"><span data-stu-id="1cec9-155">Click the **Collapse** (![collapse](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image6.png) ) and **Expand** (![expand](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image7.png) ) buttons located on the left margin of the text.</span></span> <span data-ttu-id="1cec9-156">Należy zauważyć, że teraz można ukryć style, które nie jest używany widok oczyszczarki.</span><span class="sxs-lookup"><span data-stu-id="1cec9-156">Notice that you can now hide the styles you don't use to have a cleaner view.</span></span>

    <span data-ttu-id="1cec9-157">![Zwijanie klas CSS](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image8.png "klas CSS zwijanie")</span><span class="sxs-lookup"><span data-stu-id="1cec9-157">![Collapsing CSS classes](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image8.png "Collapsing CSS classes")</span></span>

    <span data-ttu-id="1cec9-158">*Zwijanie klas CSS*</span><span class="sxs-lookup"><span data-stu-id="1cec9-158">*Collapsing CSS classes*</span></span>
5. <span data-ttu-id="1cec9-159">Upewnij się, że włączona jest funkcja inteligentnego wcięcia.</span><span class="sxs-lookup"><span data-stu-id="1cec9-159">Make sure that the smart indentation feature is enabled.</span></span> <span data-ttu-id="1cec9-160">Wybierz **narzędzia** | **opcje** opcji menu, a następnie wybierz **Edytor tekstu** | **CSS**  |  **Formatowanie** strony w okienku po lewej stronie ekranu.</span><span class="sxs-lookup"><span data-stu-id="1cec9-160">Select the **Tools** | **Options** menu option, and then select the **Text Editor** | **CSS** | **Formatting** page in the left pane of the screen.</span></span> <span data-ttu-id="1cec9-161">Sprawdź **hierarchiczna wcięcia** opcji.</span><span class="sxs-lookup"><span data-stu-id="1cec9-161">Check the **Hierarchical indentation** option.</span></span>

    <span data-ttu-id="1cec9-162">![Włączanie hierarchiczna wcięcia](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image9.png "włączenie wcięcia hierarchiczne")</span><span class="sxs-lookup"><span data-stu-id="1cec9-162">![Enabling hierarchical indentation](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image9.png "Enabling hierarchical indentation")</span></span>

    <span data-ttu-id="1cec9-163">*Włączanie wcięcia hierarchiczne*</span><span class="sxs-lookup"><span data-stu-id="1cec9-163">*Enabling hierarchical indentation*</span></span>
6. <span data-ttu-id="1cec9-164">Znajdź definicji klasy głównym (.main) i Dołącz stylu do elementów div.</span><span class="sxs-lookup"><span data-stu-id="1cec9-164">Locate the main class definition (.main) and append a style to the div elements.</span></span> <span data-ttu-id="1cec9-165">Można zauważyć kod wyrównuje automatycznie, myśl użytkowników można znaleźć klasy nadrzędnej jeden rzut oka.</span><span class="sxs-lookup"><span data-stu-id="1cec9-165">You will notice that the code aligns automatically, helping users to find the parent classes at a glance.</span></span>

    <span data-ttu-id="1cec9-166">CSS</span><span class="sxs-lookup"><span data-stu-id="1cec9-166">CSS</span></span>

    [!code-css[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample1.css)]

    <span data-ttu-id="1cec9-167">![Hierarchiczna wyrównania w kodzie CSS](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image10.png "hierarchiczna wyrównania w kodzie CSS")</span><span class="sxs-lookup"><span data-stu-id="1cec9-167">![Hierarchical alignment in CSS](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image10.png "Hierarchical alignment in CSS")</span></span>

    <span data-ttu-id="1cec9-168">*Hierarchiczna wyrównania w kodzie CSS*</span><span class="sxs-lookup"><span data-stu-id="1cec9-168">*Hierarchical alignment in CSS*</span></span>
7. <span data-ttu-id="1cec9-169">Wewnątrz **.main div** klasy, Znajdź kursor na końcu **obramowania: 0px;** i naciśnij klawisz **Enter** do wyświetlenia na liście IntelliSense.</span><span class="sxs-lookup"><span data-stu-id="1cec9-169">Inside **.main div** class, locate the cursor at the end of **border: 0px;** and press **Enter** to display the IntelliSense list.</span></span> <span data-ttu-id="1cec9-170">Zacznij pisać **górnej** i zwróć uwagę, jak lista jest przefiltrowana podczas pisania.</span><span class="sxs-lookup"><span data-stu-id="1cec9-170">Start typing **top** and notice how the list is filtered as you type.</span></span> <span data-ttu-id="1cec9-171">Lista będzie zawierać elementów, które zawierają **górnej** w jakimkolwiek Word (w poprzednich wersjach programu Visual Studio, listy są filtrowane według elementy który *rozpocząć* z terminem).</span><span class="sxs-lookup"><span data-stu-id="1cec9-171">The list will display the elements that contain **top** at any part of the word (In prior versions of Visual Studio, the list is filtered by the items that *begin* with the term).</span></span>

    <span data-ttu-id="1cec9-172">![Ulepszenia IntelliSense w kodzie CSS](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image11.png "ulepszeń funkcji IntelliSense w kodzie CSS")</span><span class="sxs-lookup"><span data-stu-id="1cec9-172">![IntelliSense enhancements in CSS](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image11.png "IntelliSense enhancements in CSS")</span></span>

    <span data-ttu-id="1cec9-173">*Ulepszenia IntelliSense w kodzie CSS*</span><span class="sxs-lookup"><span data-stu-id="1cec9-173">*IntelliSense enhancements in CSS*</span></span>

<a id="Ex1Task2"></a>

<a id="Task_2_-_The_Color_Picker"></a>
#### <a name="task-2---the-color-picker"></a><span data-ttu-id="1cec9-174">Zadanie 2 — próbnika kolorów</span><span class="sxs-lookup"><span data-stu-id="1cec9-174">Task 2 - The Color Picker</span></span>

<span data-ttu-id="1cec9-175">W tym zadaniu zostanie wykryty próbnika kolorów CSS włączona w programie Visual Studio IntelliSense.</span><span class="sxs-lookup"><span data-stu-id="1cec9-175">In this task, you will discover the new CSS Color Picker integrated into Visual Studio IntelliSense.</span></span>

1. <span data-ttu-id="1cec9-176">W **Site.css,** zlokalizować definicji klasy nagłówka (.header), umieść kursor w polu **kolor tła** atrybutu między &quot;:&quot; i &quot; # &quot; znaków w tym wierszu kodu **.**</span><span class="sxs-lookup"><span data-stu-id="1cec9-176">In **Site.css,** locate the header class definition (.header) and place the cursor next to **background-color** attribute, between the &quot;:&quot; and &quot;#&quot; characters on that line of code **.**</span></span>

    <span data-ttu-id="1cec9-177">![Lokalizowanie kursor](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image12.png "lokalizowanie kursora")</span><span class="sxs-lookup"><span data-stu-id="1cec9-177">![Locating the cursor](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image12.png "Locating the cursor")</span></span>

    <span data-ttu-id="1cec9-178">*Lokalizowanie kursora*</span><span class="sxs-lookup"><span data-stu-id="1cec9-178">*Locating the cursor*</span></span>
2. <span data-ttu-id="1cec9-179">Usuń **dwukropek** (:) i zapisać go ponownie, aby wyświetlić próbnika kolorów.</span><span class="sxs-lookup"><span data-stu-id="1cec9-179">Delete the **colon** (:) and write it again to display the color picker.</span></span> <span data-ttu-id="1cec9-180">Zwróć uwagę, czy pierwszy kolorów, które będą wyświetlane są najczęściej kolory witryny.</span><span class="sxs-lookup"><span data-stu-id="1cec9-180">Notice that the first colors you will see are the most frequent colors of your site.</span></span> <span data-ttu-id="1cec9-181">Jeśli klikniesz przycisk kolor biały, kod HTML (#fff) spowoduje zastąpienie bieżącego kodu kolorów w arkuszu stylów.</span><span class="sxs-lookup"><span data-stu-id="1cec9-181">If you click the white color, its HTML color code (#fff) will replace the current color code in the stylesheet.</span></span>

    <span data-ttu-id="1cec9-182">![Selektor kolorów](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image13.png "próbnika kolorów")</span><span class="sxs-lookup"><span data-stu-id="1cec9-182">![Color picker](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image13.png "Color picker")</span></span>

    <span data-ttu-id="1cec9-183">*Selektor kolorów*</span><span class="sxs-lookup"><span data-stu-id="1cec9-183">*Color picker*</span></span>
3. <span data-ttu-id="1cec9-184">Naciśnij klawisz **rozwiń** (![com](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image14.png) ) przycisk na próbnika kolorów, aby wyświetlić kolor gradientu, a następnie przeciągnij kursor gradientu, aby wybrać inny kolor.</span><span class="sxs-lookup"><span data-stu-id="1cec9-184">Press the **Expand** (![com](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image14.png) ) button on the color picker to display the color gradient, and then drag the gradient cursor to select a different color.</span></span> <span data-ttu-id="1cec9-185">Następnie kliknij przycisk **Kroplomierz** i wybrać dowolny kolor na ekranie.</span><span class="sxs-lookup"><span data-stu-id="1cec9-185">After that, click the **Eyedropper** button and select any color from the screen.</span></span> <span data-ttu-id="1cec9-186">Zwróć uwagę, że wartość koloru tła zmienia dynamicznie, gdy kursor jest.</span><span class="sxs-lookup"><span data-stu-id="1cec9-186">Notice that background color value changes dynamically while you move the cursor.</span></span>

    <span data-ttu-id="1cec9-187">![Kolor gradientu selektora](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image15.png "gradientu próbnika kolorów")</span><span class="sxs-lookup"><span data-stu-id="1cec9-187">![Color picker gradient](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image15.png "Color picker gradient")</span></span>

    <span data-ttu-id="1cec9-188">*Kolor gradientu selektora*</span><span class="sxs-lookup"><span data-stu-id="1cec9-188">*Color picker gradient*</span></span>
4. <span data-ttu-id="1cec9-189">W **nieprzezroczystość** suwaka, przesuń selektor środek pasek, aby zmniejszyć przezroczystość.</span><span class="sxs-lookup"><span data-stu-id="1cec9-189">In the **Opacity** slider, move the selector to the center of the bar to reduce the opacity.</span></span> <span data-ttu-id="1cec9-190">Zwróć uwagę, że wartość koloru tła teraz zmienia jego skali do RGBA.</span><span class="sxs-lookup"><span data-stu-id="1cec9-190">Notice that background-color value now changes its scale to RGBA.</span></span>

    <span data-ttu-id="1cec9-191">![Selektor kolorów nieprzezroczystość](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image16.png "próbnika kolorów nieprzezroczystość.")</span><span class="sxs-lookup"><span data-stu-id="1cec9-191">![Color picker Opacity](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image16.png "Color picker Opacity")</span></span>

    <span data-ttu-id="1cec9-192">*Selektor kolorów nieprzezroczystość.*</span><span class="sxs-lookup"><span data-stu-id="1cec9-192">*Color picker Opacity*</span></span>

    > [!NOTE]
    > <span data-ttu-id="1cec9-193">Definicja kolor RGBA (czerwony, zielony, niebieski, alfa) w standardzie CSS3 umożliwia zdefiniowanie wartości Przezroczystość koloru dla pojedynczego elementu.</span><span class="sxs-lookup"><span data-stu-id="1cec9-193">The RGBA (Red, Green, Blue, Alpha) color definition in CSS3 enables you to define the color opacity value for a single item.</span></span> <span data-ttu-id="1cec9-194">W odróżnieniu od **nieprzezroczystość -** podobne atrybut CSS  **-**  kolory RGBA również są zgodne z najnowszych przeglądarkach.</span><span class="sxs-lookup"><span data-stu-id="1cec9-194">Unlike **opacity -** a similar CSS attribute **-** RGBA colors are also compatible with the latest browsers.</span></span>

<a id="Ex1Task3"></a>

<a id="Task_3_-_CSS_Compatible_Code_Snippets"></a>
#### <a name="task-3---css-compatible-code-snippets"></a><span data-ttu-id="1cec9-195">Zadanie 3 — wstawki kodu zgodne CSS</span><span class="sxs-lookup"><span data-stu-id="1cec9-195">Task 3 - CSS Compatible Code Snippets</span></span>

<span data-ttu-id="1cec9-196">W tym zadaniu dowiesz się, jak używać różnych przeglądarkach zgodne CSS3 fragmentów w celu wdrożenia niektóre funkcje w witrynie sieci Web.</span><span class="sxs-lookup"><span data-stu-id="1cec9-196">In this task, you will learn how to use cross-browser compatible CSS3 snippets in order to implement some features in your website.</span></span>

1. <span data-ttu-id="1cec9-197">W **Site.css** pliku, Znajdź **nagłówka** CSS klasy definicji (.header) i umieść kursor poniżej  **/ \*radius obramowania\* /**  symbolu zastępczego, aby dodać nowy fragment kodu.</span><span class="sxs-lookup"><span data-stu-id="1cec9-197">In the **Site.css** file, locate the **header** CSS class definition (.header) and place the cursor below the **/\*border radius\*/** placeholder to add a new snippet.</span></span> <span data-ttu-id="1cec9-198">Naciśnij klawisz **Enter** do wyświetlenia na liście IntelliSense i typ **radius** Aby filtrować listę.</span><span class="sxs-lookup"><span data-stu-id="1cec9-198">Press **Enter** to display the IntelliSense list and type **radius** to filter the list.</span></span> <span data-ttu-id="1cec9-199">Wybierz **border-radius** opcję z listy za pomocą podwójnego kliknięcia, a następnie naciśnij klawisz **kartę** klucz do wstawiania wstawki kodu.</span><span class="sxs-lookup"><span data-stu-id="1cec9-199">Select the **border-radius** option from the list with a double click, and then press the **TAB** key to insert the snippet.</span></span> <span data-ttu-id="1cec9-200">Następnie wpisz rozmiar radius w pikselach i naciśnij klawisz **Enter**.</span><span class="sxs-lookup"><span data-stu-id="1cec9-200">Then, type a radius size in pixels and press **Enter**.</span></span> <span data-ttu-id="1cec9-201">Na przykład wpisz **15px**.</span><span class="sxs-lookup"><span data-stu-id="1cec9-201">For instance, type **15px**.</span></span>

    <span data-ttu-id="1cec9-202">Atrybuty CSS3, dodawane przez fragment kodu spowoduje, że zaokrąglony obramowań w większości przeglądarek zgodności HTML5, w tym Mozilla i na podstawie WebKit przeglądarki.</span><span class="sxs-lookup"><span data-stu-id="1cec9-202">The CSS3 attributes added by the snippet will render rounded borders in most HTML5 compliance browsers, including Mozilla and WebKit-based browsers.</span></span>

    <span data-ttu-id="1cec9-203">![Przy użyciu fragment border-radius](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image17.png "przy użyciu fragment border-radius")</span><span class="sxs-lookup"><span data-stu-id="1cec9-203">![Using a border-radius snippet](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image17.png "Using a border-radius snippet")</span></span>

    <span data-ttu-id="1cec9-204">*Przy użyciu fragment border-radius*</span><span class="sxs-lookup"><span data-stu-id="1cec9-204">*Using a border-radius snippet*</span></span>
2. <span data-ttu-id="1cec9-205">Zastosować te same **obramowania** fragmentów w stylu strony (.page).</span><span class="sxs-lookup"><span data-stu-id="1cec9-205">Apply the same **border** snippets in the page style (.page).</span></span>

    <span data-ttu-id="1cec9-206">CSS</span><span class="sxs-lookup"><span data-stu-id="1cec9-206">CSS</span></span>

    [!code-css[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample2.css)]
3. <span data-ttu-id="1cec9-207">Naciśnij klawisz **F5** Aby uruchomić rozwiązanie.</span><span class="sxs-lookup"><span data-stu-id="1cec9-207">Press **F5** to run the solution.</span></span> <span data-ttu-id="1cec9-208">Zwróć uwagę, że każdej stronie teraz ma zostać zaokrąglona obramowań.</span><span class="sxs-lookup"><span data-stu-id="1cec9-208">Notice that each page now has rounded borders.</span></span>

    <span data-ttu-id="1cec9-209">![Zaokrąglone narożniki](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image18.png "zaokrąglone narożniki")</span><span class="sxs-lookup"><span data-stu-id="1cec9-209">![Rounded corners](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image18.png "Rounded corners")</span></span>

    <span data-ttu-id="1cec9-210">*Zaokrąglone narożniki*</span><span class="sxs-lookup"><span data-stu-id="1cec9-210">*Rounded corners*</span></span>
4. <span data-ttu-id="1cec9-211">Zamknij przeglądarkę i powrócić do programu Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="1cec9-211">Close the browser and return to Visual Studio.</span></span>
5. <span data-ttu-id="1cec9-212">Otwórz **Custome.CSS** plik znajdujący się w obszarze **style** folderu, umieść kursor w **div.images ul li img** definicji klasy.</span><span class="sxs-lookup"><span data-stu-id="1cec9-212">Open the **Custom.css** file located under the **Styles** folder and place the cursor inside **div.images ul li img** class definition.</span></span>
6. <span data-ttu-id="1cec9-213">Naciśnij klawisz enter, aby wyświetlić listę funkcji IntelliSense, typ **tle pole** i naciśnij klawisz **kartę** klucza dwa razy, aby wstawić fragment kodu tle domyślne wewnątrz definicji klasy.</span><span class="sxs-lookup"><span data-stu-id="1cec9-213">Press enter to display the IntelliSense list, type **box-shadow** and press the **TAB** key twice to insert the default shadow code snippet inside the class definition.</span></span> <span data-ttu-id="1cec9-214">Ustaw wartości cienia **10px 10px 5px #888**.</span><span class="sxs-lookup"><span data-stu-id="1cec9-214">Set the shadow values to **10px 10px 5px #888**.</span></span> <span data-ttu-id="1cec9-215">Następnie wpisz **border-radius** i wstawianie fragmentu kodu.</span><span class="sxs-lookup"><span data-stu-id="1cec9-215">Then, type **border-radius** and insert the code snippet.</span></span> <span data-ttu-id="1cec9-216">Typ **15px** ustawienie rozmiaru usługi radius i naciśnij klawisz **ENTER**.</span><span class="sxs-lookup"><span data-stu-id="1cec9-216">Type **15px** to set radius size and press **ENTER**.</span></span>

    <span data-ttu-id="1cec9-217">![Zaokrąglone narożniki z tle](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image19.png "zaokrąglone narożniki z cień")</span><span class="sxs-lookup"><span data-stu-id="1cec9-217">![Rounded corners with shadow](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image19.png "Rounded corners with shadow")</span></span>

    <span data-ttu-id="1cec9-218">*Zaokrąglone narożniki z cień*</span><span class="sxs-lookup"><span data-stu-id="1cec9-218">*Rounded corners with shadow*</span></span>

    > [!NOTE]
    > <span data-ttu-id="1cec9-219">W tej chwili atrybutu cienia są wstawiane z odpowiedniego prefiksu (moz, webkit, o) do obsługi Mozilla i przeglądarki Webkit (Chrome, Safari, Konkeror).</span><span class="sxs-lookup"><span data-stu-id="1cec9-219">At this moment, the shadow attribute is inserted with the corresponding prefix (moz, webkit, o) to support Mozilla and Webkit (Chrome, Safari, Konkeror) browsers.</span></span>
7. <span data-ttu-id="1cec9-220">Utwórz nową klasę **div.images ul li img:hover** poniżej **div.images ul li img** definicji klasy, umieść kursor w nawiasie **.**</span><span class="sxs-lookup"><span data-stu-id="1cec9-220">Create a new class **div.images ul li img:hover** below the **div.images ul li img** class definition and place the cursor inside the brackets **.**</span></span>

    <span data-ttu-id="1cec9-221">CSS</span><span class="sxs-lookup"><span data-stu-id="1cec9-221">CSS</span></span>

    [!code-css[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample3.css)]
8. <span data-ttu-id="1cec9-222">Typ **przekształcenie** i naciśnij klawisz **kartę** klucza dwa razy, aby wstawić fragment kodu transformacji.</span><span class="sxs-lookup"><span data-stu-id="1cec9-222">Type **transform** and press the **TAB** key twice in order to insert the transform snippet.</span></span> <span data-ttu-id="1cec9-223">Następnie wprowadź **rotate(-15deg)** Aby zmienić wartość kąta obrotu, gdy obrazy są aktywowany.</span><span class="sxs-lookup"><span data-stu-id="1cec9-223">Then, enter **rotate(-15deg)** to change the rotation angle value when images are hovered.</span></span>

    <span data-ttu-id="1cec9-224">CSS</span><span class="sxs-lookup"><span data-stu-id="1cec9-224">CSS</span></span>

    [!code-css[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample4.css)]
9. <span data-ttu-id="1cec9-225">Naciśnij klawisz **F5** uruchomić rozwiązanie, a następnie przejdź do strony CSS3.</span><span class="sxs-lookup"><span data-stu-id="1cec9-225">Press **F5** to run the solution and browse to the CSS3 page.</span></span> <span data-ttu-id="1cec9-226">Zauważyć, że obrazy mają zaokrąglone narożniki, a pole cieni.</span><span class="sxs-lookup"><span data-stu-id="1cec9-226">Notice that the images have rounded corners and box shadows.</span></span> <span data-ttu-id="1cec9-227">Umieść kursor myszy nad obrazów i oglądanie Obróć.</span><span class="sxs-lookup"><span data-stu-id="1cec9-227">Hover the mouse over the images and watch them rotate.</span></span>

    <span data-ttu-id="1cec9-228">![Przekształć fragment obracanie obrazów](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image20.png "fragment przekształcenia obracanie obrazów")</span><span class="sxs-lookup"><span data-stu-id="1cec9-228">![Transform snippet rotating an image](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image20.png "Transform snippet rotating an image")</span></span>

    <span data-ttu-id="1cec9-229">*Przekształć fragment obracanie obrazów*</span><span class="sxs-lookup"><span data-stu-id="1cec9-229">*Transform snippet rotating an image*</span></span>

    > [!NOTE]
    > <span data-ttu-id="1cec9-230">Jeśli używasz programu Internet Explorer 10 i cieni nie jest wyświetlane, upewnij się, że ustawienie trybu dokumentu Standardy programu IE10.</span><span class="sxs-lookup"><span data-stu-id="1cec9-230">If you are using Internet Explorer 10 and cannot see the shadows, make sure the document mode is set to IE10 standards.</span></span> <span data-ttu-id="1cec9-231">Naciśnij klawisz **F12** Otwórz narzędzi deweloperskich programu Internet Explorer, a następnie kliknij przycisk **tryb dokumentu** można zmienić na standardy programu IE10.</span><span class="sxs-lookup"><span data-stu-id="1cec9-231">Press **F12** to open Internet Explorer developer tools and click **Document Mode** to change to IE10 standards.</span></span>

    ![o-us](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image21.png)

<a id="Exercise2"></a>

<a id="Exercise_2_Whats_New_in_the_HTML_Editor"></a>
### <a name="exercise-2-whats-new-in-the-html-editor"></a><span data-ttu-id="1cec9-233">Ćwiczenie 2: Nowości w edytorze HTML</span><span class="sxs-lookup"><span data-stu-id="1cec9-233">Exercise 2: What's New in the HTML Editor</span></span>

<span data-ttu-id="1cec9-234">Visual Studio ma ulepszone edytora HTML.</span><span class="sxs-lookup"><span data-stu-id="1cec9-234">Visual Studio has an improved HTML editor.</span></span> <span data-ttu-id="1cec9-235">Oto niektóre ulepszenia uwzględnione w tej wersji są inteligentne wcięcie w dokumentach HTML, fragmenty kodu HTML5, HTML start i dopasowania tagu końcowego i weryfikacji HTML.</span><span class="sxs-lookup"><span data-stu-id="1cec9-235">Some of the enhancements included in this version are smart indentation in HTML documents, HTML5 snippets, HTML start and end tag matching, and HTML validation.</span></span> <span data-ttu-id="1cec9-236">W ramach tego ćwiczenia zobaczysz, jak te zmiany ulepszyć Twoje fluency podczas pracy w znaczniku witryny sieci Web.</span><span class="sxs-lookup"><span data-stu-id="1cec9-236">Throughout this exercise, you will see how these changes improve your fluency when working in the website markup.</span></span>

<span data-ttu-id="1cec9-237">Podobnie jak edytor CSS poprawiła się również edytora HTML.</span><span class="sxs-lookup"><span data-stu-id="1cec9-237">Like the CSS editor, the HTML editor was also improved.</span></span> <span data-ttu-id="1cec9-238">Większość z nich to małych sieci, które ułatwić życia deweloperów sieci Web.</span><span class="sxs-lookup"><span data-stu-id="1cec9-238">Most of these improvements are small ones that make the Web developer's life easier.</span></span> <span data-ttu-id="1cec9-239">Takie operacje, jak więcej wstawki kodu HTML5, inteligentne wcięcie dopasowania tagiem początkowym i końcowym podczas edycji i sprawdzania poprawności przeznaczonych dla dokumentu HTML DOCTYPE są niektóre z nich.</span><span class="sxs-lookup"><span data-stu-id="1cec9-239">Things like more code snippets for HTML5, smart indentation, matching start and end tags when editing and validation targeting the HTML document DOCTYPE are some of these improvements.</span></span>

<a id="Ex2Task1"></a>

<a id="Task_1_-_Improved_DOCTYPE_Validation"></a>
#### <a name="task-1---improved-doctype-validation"></a><span data-ttu-id="1cec9-240">Zadanie 1 - ulepszone DOCTYPE weryfikacji</span><span class="sxs-lookup"><span data-stu-id="1cec9-240">Task 1 - Improved DOCTYPE Validation</span></span>

<span data-ttu-id="1cec9-241">Edytora HTML ma teraz możliwość DOCTYPE strony, nawet jeśli definicja może być na stronie głównej.</span><span class="sxs-lookup"><span data-stu-id="1cec9-241">The HTML editor now has the ability to check the DOCTYPE of your page, even though the definition might be in the master page.</span></span> <span data-ttu-id="1cec9-242">W zależności od typu dokumentu, strony edytora HTML będzie weryfikowane z poprawny zestaw reguł i spowoduje odfiltrowanie listy IntelliSense, biorąc pod uwagę elementy DOCTYPE.</span><span class="sxs-lookup"><span data-stu-id="1cec9-242">Depending on the DOCTYPE of your page, the HTML editor will validate with the correct set of rules and will filter the IntelliSense list considering the DOCTYPE elements.</span></span>

<span data-ttu-id="1cec9-243">W tym zadaniu zostanie zmieniony DOCTYPE strony, aby zobaczyć, jak zachowanie edytora HTML zmienia się odpowiednio.</span><span class="sxs-lookup"><span data-stu-id="1cec9-243">In this task, you will change the DOCTYPE of a page to see how the HTML editor behavior changes accordingly.</span></span>

1. <span data-ttu-id="1cec9-244">Jeśli nie jest jeszcze otwarty, uruchom **programu Visual Studio** , a następnie otwórz **WhatsNewASPNET.sln** rozwiązania, znajdujących się w **Source\WhatsNewASPNET** folder tego laboratorium.</span><span class="sxs-lookup"><span data-stu-id="1cec9-244">If not already opened, start **Visual Studio** and open the **WhatsNewASPNET.sln** solution located in the **Source\WhatsNewASPNET** folder of this lab.</span></span>
2. <span data-ttu-id="1cec9-245">Otwórz **Site.Master** strony.</span><span class="sxs-lookup"><span data-stu-id="1cec9-245">Open the **Site.Master** page.</span></span>
3. <span data-ttu-id="1cec9-246">Zwróć uwagę schemat docelowy dla narzędzi do sprawdzania poprawności.</span><span class="sxs-lookup"><span data-stu-id="1cec9-246">Notice the Target Schema for Validation Toolbar.</span></span> <span data-ttu-id="1cec9-247">Dopasuj Doctype wybrane sposób zachowania edytora HTML (Sprawdzanie poprawności, IntelliSense, itp.) prawidłowo zostanie zmieniona.</span><span class="sxs-lookup"><span data-stu-id="1cec9-247">The way the HTML editor behaves (Validation, IntelliSense, etc.) will properly change to fit the Doctype selected.</span></span>

    <span data-ttu-id="1cec9-248">![Użyj narzędzi edycji HTML źródła Doctype](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image22.png "Doctype użyj narzędzi edycji źródła HTML")</span><span class="sxs-lookup"><span data-stu-id="1cec9-248">![Use Doctype in HTML Source Editing toolbar](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image22.png "Use Doctype in HTML Source Editing toolbar")</span></span>

    <span data-ttu-id="1cec9-249">*Użyj Doctype pakietu na pasku narzędzi*</span><span class="sxs-lookup"><span data-stu-id="1cec9-249">*Use Doctype in HTML Source Editing toolbar*</span></span>
4. <span data-ttu-id="1cec9-250">Zmień schemat docelowej w formacie HTML 4.01.</span><span class="sxs-lookup"><span data-stu-id="1cec9-250">Change the Target Schema to HTML 4.01.</span></span>

    <span data-ttu-id="1cec9-251">![Zmiana typu dokumentu pakietu narzędzi](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image23.png "zmiana typu dokumentu pakietu na pasku narzędzi")</span><span class="sxs-lookup"><span data-stu-id="1cec9-251">![Changing Doctype in HTML Source Editing toolbar](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image23.png "Changing Doctype in HTML Source Editing toolbar")</span></span>

    <span data-ttu-id="1cec9-252">*Zmiana typu dokumentu pakietu na pasku narzędzi*</span><span class="sxs-lookup"><span data-stu-id="1cec9-252">*Changing Doctype in HTML Source Editing toolbar*</span></span>
5. <span data-ttu-id="1cec9-253">Umieść kursor w obszarze **treści** elementu, a następnie wpisz nazwę elementu HTML5 (na przykład **wideo**).</span><span class="sxs-lookup"><span data-stu-id="1cec9-253">Place the cursor under the **body** element, and start typing the name of an HTML5 element (for example, **video**).</span></span> <span data-ttu-id="1cec9-254">Zwróć uwagę, że element nie jest dostępny na liście IntelliSense.</span><span class="sxs-lookup"><span data-stu-id="1cec9-254">Notice that the element is not available in the IntelliSense list.</span></span>

    <span data-ttu-id="1cec9-255">![Elementy HTML5 niewymienione](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image24.png "elementy HTML5 niewymienione")</span><span class="sxs-lookup"><span data-stu-id="1cec9-255">![HTML5 elements not listed](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image24.png "HTML5 elements not listed")</span></span>

    <span data-ttu-id="1cec9-256">*Elementy HTML5 niewymienione*</span><span class="sxs-lookup"><span data-stu-id="1cec9-256">*HTML5 elements not listed*</span></span>
6. <span data-ttu-id="1cec9-257">Cofnij zmiany schematu docelowej dla narzędzi sprawdzania poprawności pobrania DOCTYPE: XHTML5 z listy rozwijanej.</span><span class="sxs-lookup"><span data-stu-id="1cec9-257">Undo the changes to the Target Schema for Validation Toolbar, picking DOCTYPE: XHTML5 from the dropdown list.</span></span>

    <span data-ttu-id="1cec9-258">![Użyj narzędzi edycji HTML źródła Doctype](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image25.png "Doctype użyj narzędzi edycji źródła HTML")</span><span class="sxs-lookup"><span data-stu-id="1cec9-258">![Use Doctype in HTML Source Editing toolbar](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image25.png "Use Doctype in HTML Source Editing toolbar")</span></span>

    <span data-ttu-id="1cec9-259">*Resetowanie Doctype w narzędzi edycji źródła HTML*</span><span class="sxs-lookup"><span data-stu-id="1cec9-259">*Reset Doctype in HTML Source Editing toolbar*</span></span>
7. <span data-ttu-id="1cec9-260">Umieść kursor w obszarze **treści** element i wpisz HTML5 element ponownie (na przykład, takich jak **wideo**).</span><span class="sxs-lookup"><span data-stu-id="1cec9-260">Place the cursor under the **body** element and start typing an HTML5 element again (For example, like **video**).</span></span> <span data-ttu-id="1cec9-261">Zwróć uwagę, elementy HTML5 są teraz dostępne na liście IntelliSense.</span><span class="sxs-lookup"><span data-stu-id="1cec9-261">Notice that the HTML5 elements are now available in the IntelliSense list.</span></span>

    <span data-ttu-id="1cec9-262">![Elementy HTML5 są wymienione](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image26.png "HTML5 elementy są wymienione")</span><span class="sxs-lookup"><span data-stu-id="1cec9-262">![HTML5 elements being listed](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image26.png "HTML5 elements being listed")</span></span>

    <span data-ttu-id="1cec9-263">*Elementy HTML5 są wymienione*</span><span class="sxs-lookup"><span data-stu-id="1cec9-263">*HTML5 elements being listed*</span></span>

<a id="Ex2Task2"></a>

<a id="Task_2_-_StartEnd_Tags_Automatic_Update"></a>
#### <a name="task-2---startend-tags-automatic-update"></a><span data-ttu-id="1cec9-264">Zadanie 2 — rozpoczęcia i zakończenia znaczniki aktualizacji automatycznych</span><span class="sxs-lookup"><span data-stu-id="1cec9-264">Task 2 - Start/End Tags Automatic Update</span></span>

<span data-ttu-id="1cec9-265">Visual Studio teraz aktualizuje HTML otwierania lub zamykania znaczniki elementu, którą edytujesz, aby były zgodne ze sobą.</span><span class="sxs-lookup"><span data-stu-id="1cec9-265">Visual Studio now updates the HTML opening or closing tags of the element that you are editing to match each other.</span></span> <span data-ttu-id="1cec9-266">Ta nowa funkcja zwiększa produktywność podczas edytowania tagów HTML.</span><span class="sxs-lookup"><span data-stu-id="1cec9-266">This new feature will improve your productivity when editing HTML tags.</span></span>

1. <span data-ttu-id="1cec9-267">Na **Default.aspx** Dodaj **H3** element z tytułem (na przykład programu Visual Studio 2012 skały!).</span><span class="sxs-lookup"><span data-stu-id="1cec9-267">On the **Default.aspx** page, add an **H3** element with a title (for example, Visual Studio 2012 Rocks!).</span></span>


    [!code-aspx[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample5.aspx)]
2. <span data-ttu-id="1cec9-268">Zmień **H3** tagu i typ **H2** lub **H1.**</span><span class="sxs-lookup"><span data-stu-id="1cec9-268">Change the **H3** tag and type **H2** or **H1.**</span></span>

    <span data-ttu-id="1cec9-269">Należy zauważyć, że tag końcowy automatycznie aktualizacji.</span><span class="sxs-lookup"><span data-stu-id="1cec9-269">Notice that the end tag automatically updates.</span></span> <span data-ttu-id="1cec9-270">Można również zmodyfikować tagu końcowego, aby zobaczyć, że tag początkowy odpowiednio aktualizowany za.</span><span class="sxs-lookup"><span data-stu-id="1cec9-270">You can also modify the end tag to see that the start tag updates accordingly too.</span></span>

    <span data-ttu-id="1cec9-271">![Automatyczna aktualizacja tagu końcowego](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image27.png "automatyczną aktualizację do tagu końcowego")</span><span class="sxs-lookup"><span data-stu-id="1cec9-271">![Automatic update of the end tag](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image27.png "Automatic update of the end tag")</span></span>

    <span data-ttu-id="1cec9-272">*Automatyczna aktualizacja tagu końcowego*</span><span class="sxs-lookup"><span data-stu-id="1cec9-272">*Automatic update of the end tag*</span></span>

<a id="Ex2Task3"></a>

<a id="Task_3_-_New_HTML5_Code_Snippets"></a>
#### <a name="task-3---new-html5-code-snippets"></a><span data-ttu-id="1cec9-273">Zadanie 3 - nowych fragmentów kodu HTML5</span><span class="sxs-lookup"><span data-stu-id="1cec9-273">Task 3 - New HTML5 Code Snippets</span></span>

<span data-ttu-id="1cec9-274">Visual Studio teraz obejmuje kilka fragmentów kodu HTML5.</span><span class="sxs-lookup"><span data-stu-id="1cec9-274">Visual Studio now includes several HTML5 code snippets.</span></span> <span data-ttu-id="1cec9-275">W tym zadaniu użyjesz niektóre z tych fragmentów.</span><span class="sxs-lookup"><span data-stu-id="1cec9-275">In this task, you will use some of these snippets.</span></span>

1. <span data-ttu-id="1cec9-276">Dodaj nowy folder o nazwie **audio** do głównego folderu witryny sieci web.</span><span class="sxs-lookup"><span data-stu-id="1cec9-276">Add a new folder named **audio** to the root of the web site folder.</span></span> <span data-ttu-id="1cec9-277">Otwórz Eksploratora Windows i skopiuj wszystkie plik dźwiękowy do **audio** folderu **WhatsNewASPNET.sln** rozwiązania.</span><span class="sxs-lookup"><span data-stu-id="1cec9-277">Open Windows Explorer and copy any audio file into the **audio** folder of the **WhatsNewASPNET.sln** solution.</span></span>
2. <span data-ttu-id="1cec9-278">W **Default.aspx** strony, odszukaj pozycję kursora w sieci Web 11 skały!!</span><span class="sxs-lookup"><span data-stu-id="1cec9-278">In the **Default.aspx** page, locate the cursor under the Web11 Rocks!!</span></span> <span data-ttu-id="1cec9-279">Nagłówek.</span><span class="sxs-lookup"><span data-stu-id="1cec9-279">Header.</span></span> <span data-ttu-id="1cec9-280">Typ **audio** i naciśnij klawisz TAB.</span><span class="sxs-lookup"><span data-stu-id="1cec9-280">Type **audio** and press the TAB key.</span></span>

    <span data-ttu-id="1cec9-281">Nowe edytora HTML zawiera wstawki kodu HTML5 zawartości.</span><span class="sxs-lookup"><span data-stu-id="1cec9-281">The new HTML editor includes code snippets for HTML5 content.</span></span> <span data-ttu-id="1cec9-282">Pamiętaj, aby Użyj właściwej definicji typu dokumentu, aby włączyć fragmenty kodu HTML5.</span><span class="sxs-lookup"><span data-stu-id="1cec9-282">Remember to use the proper DOCTYPE definition to enable the HTML5 snippets.</span></span>

    <span data-ttu-id="1cec9-283">![Wstawianie wstawek kodu HTML5](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image28.png "Wstawianie wstawek kodu HTML5")</span><span class="sxs-lookup"><span data-stu-id="1cec9-283">![Inserting HTML5 Code Snippets](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image28.png "Inserting HTML5 Code Snippets")</span></span>

    <span data-ttu-id="1cec9-284">*Wstawianie wstawek kodu HTML5*</span><span class="sxs-lookup"><span data-stu-id="1cec9-284">*Inserting HTML5 Code Snippets*</span></span>
3. <span data-ttu-id="1cec9-285">Aktualizowanie źródła audio, aby wskazywał istniejącego pliku audio.</span><span class="sxs-lookup"><span data-stu-id="1cec9-285">Update the audio source to point to an existing audio file.</span></span>


    [!code-aspx[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample6.aspx)]

    > [!NOTE]
    > <span data-ttu-id="1cec9-286">Musisz dodać plik dźwiękowy do rozwiązania.</span><span class="sxs-lookup"><span data-stu-id="1cec9-286">You will need to add the audio file to the solution.</span></span>
4. <span data-ttu-id="1cec9-287">Naciśnij klawisz **F5** uruchamiania witryny i odtwarzanie dźwięku.</span><span class="sxs-lookup"><span data-stu-id="1cec9-287">Press **F5** to run the site and play the audio.</span></span>

    <span data-ttu-id="1cec9-288">![Uruchomienie formantu audio](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image29.png "systemem kontroli audio")</span><span class="sxs-lookup"><span data-stu-id="1cec9-288">![Running the audio control](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image29.png "Running the audio control")</span></span>

    <span data-ttu-id="1cec9-289">*Uruchomienie formantu audio*</span><span class="sxs-lookup"><span data-stu-id="1cec9-289">*Running the audio control*</span></span>

    > [!NOTE]
    > <span data-ttu-id="1cec9-290">Można też spróbować więcej wstawki uwzględnione w programie Visual Studio ilustracji wideo, np.</span><span class="sxs-lookup"><span data-stu-id="1cec9-290">You can also try more snippets included in Visual Studio, such as video, figure, etc.</span></span>
5. <span data-ttu-id="1cec9-291">Teraz spróbuj Wstawianie formantu w niektórych części strony.</span><span class="sxs-lookup"><span data-stu-id="1cec9-291">Now, try to insert a control in some part of the page.</span></span> <span data-ttu-id="1cec9-292">Na przykład próba wstawienia **widoku GridView** kontroli, ale zamiast wpisywać  **&lt;linie,** zacznij wpisywać ciąg  **&lt;GV**.</span><span class="sxs-lookup"><span data-stu-id="1cec9-292">For example, try to insert a **GridView** control, but instead of typing **&lt;Gri,** start typing **&lt;GV**.</span></span> <span data-ttu-id="1cec9-293">Należy zauważyć, że lista IntelliSense zawiera **asp: GridView** formantu.</span><span class="sxs-lookup"><span data-stu-id="1cec9-293">Notice that the IntelliSense list shows the **asp:GridView** control.</span></span>

    <span data-ttu-id="1cec9-294">IntelliSense w edytorze HTML udostępnia teraz wyszukiwania wielkości liter tytułu, a także częściowego dopasowania (pobieranie wszystkie elementy, które zawiera wyrażenie).</span><span class="sxs-lookup"><span data-stu-id="1cec9-294">IntelliSense in the HTML Editor now provides title-casing search, as well as partial matching (retrieving all elements that contains the term).</span></span>

    <span data-ttu-id="1cec9-295">![Wstawianie GridView z listami IntelliSense](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image30.png "Wstawianie GridView z listami IntelliSense")</span><span class="sxs-lookup"><span data-stu-id="1cec9-295">![Inserting a GridView with IntelliSense lists](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image30.png "Inserting a GridView with IntelliSense lists")</span></span>

    <span data-ttu-id="1cec9-296">*Wstawianie GridView z listami IntelliSense*</span><span class="sxs-lookup"><span data-stu-id="1cec9-296">*Inserting a GridView with IntelliSense lists*</span></span>

    <span data-ttu-id="1cec9-297">W przypadku wpisania  **&lt;siatki** otrzymasz wszystkich elementów, które odpowiadają termin, ale sugeruje Visual Studio **gridview** sterowania:</span><span class="sxs-lookup"><span data-stu-id="1cec9-297">If you type **&lt;grid** you will get all the items that match the term, but Visual Studio will suggest the **gridview** control:</span></span>

    <span data-ttu-id="1cec9-298">![Wstawianie Element GridView z listy IntelliSense i częściowe dopasowanie](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image31.png "Wstawianie Element GridView z listy IntelliSense i częściowe dopasowania")</span><span class="sxs-lookup"><span data-stu-id="1cec9-298">![Inserting a GridView with IntelliSense lists and partial matching](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image31.png "Inserting a GridView with IntelliSense lists and partial matching")</span></span>

    <span data-ttu-id="1cec9-299">*Wstawianie Element GridView z listy IntelliSense i częściowe dopasowania*</span><span class="sxs-lookup"><span data-stu-id="1cec9-299">*Inserting a GridView with IntelliSense lists and partial matching*</span></span>

<a id="Ex2Task4"></a>

<a id="Task_4_-_HTML_Editor_Smart_Tags"></a>
#### <a name="task-4---html-editor-smart-tags"></a><span data-ttu-id="1cec9-300">Zadanie 4 — tagów inteligentnych edytora HTML</span><span class="sxs-lookup"><span data-stu-id="1cec9-300">Task 4 - HTML Editor Smart Tags</span></span>

<span data-ttu-id="1cec9-301">Poprawa innego edytora HTML jest funkcji tagów inteligentnych.</span><span class="sxs-lookup"><span data-stu-id="1cec9-301">Another improvement in the HTML Editor is the Smart Tags feature.</span></span> <span data-ttu-id="1cec9-302">Tagi inteligentne ułatwiają do wykonywania zadań związanych z projektowaniem wspólnych lub powtarzających się na zasadzie-control.</span><span class="sxs-lookup"><span data-stu-id="1cec9-302">Smart tags make it easy to perform common or repetitive development tasks on a per-control basis.</span></span> <span data-ttu-id="1cec9-303">Ta funkcja została już dostępne w Projektancie HTML, ale nie w edytorze HTML.</span><span class="sxs-lookup"><span data-stu-id="1cec9-303">This feature was already available in the HTML Designer, but not in the HTML Editor.</span></span>

1. <span data-ttu-id="1cec9-304">Otwórz **Site.Master** i Znajdź **asp: Menu** elementu.</span><span class="sxs-lookup"><span data-stu-id="1cec9-304">Open **Site.Master** and locate the **asp:Menu** element.</span></span> <span data-ttu-id="1cec9-305">Umieść kursor w tagu początkowym i zwróć uwagę, czy małych symbolu, wyświetlane w dolnej części elementu — kliknij go, aby otworzyć menu panelu inteligentnego.</span><span class="sxs-lookup"><span data-stu-id="1cec9-305">Place the cursor on the start tag and notice that the small glyph displayed at the bottom of the element - click it to open the smart tasks menu.</span></span> <span data-ttu-id="1cec9-306">Zwróć uwagę, że masz dostęp do niektórych zadań związanych z Menu kontroli.</span><span class="sxs-lookup"><span data-stu-id="1cec9-306">Notice that you have quick access to some tasks related to the Menu control.</span></span>

    <span data-ttu-id="1cec9-307">![Inteligentne zadań kontrolki Menu](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image32.png "inteligentne zadań kontrolki Menu")</span><span class="sxs-lookup"><span data-stu-id="1cec9-307">![Smart tasks for the Menu control](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image32.png "Smart tasks for the Menu control")</span></span>

    <span data-ttu-id="1cec9-308">*Inteligentne zadań kontrolki Menu*</span><span class="sxs-lookup"><span data-stu-id="1cec9-308">*Smart tasks for the Menu control*</span></span>

<a id="Ex2Task5"></a>

<a id="Task_5_-_Smart_Indentation"></a>
#### <a name="task-5---smart-indentation"></a><span data-ttu-id="1cec9-309">Zadanie 5 - inteligentne wcięcie</span><span class="sxs-lookup"><span data-stu-id="1cec9-309">Task 5 - Smart Indentation</span></span>

<span data-ttu-id="1cec9-310">Jeden z najlepszych rozwiązań w formacie HTML jest wcięcia elementów zagnieżdżonych, aby zachować kod do odczytu.</span><span class="sxs-lookup"><span data-stu-id="1cec9-310">One of the best practices in HTML is indenting the nested elements to keep the code readable.</span></span> <span data-ttu-id="1cec9-311">W programie Visual Studio 2012 można zauważyć, że edytor automatycznie wcięcia elementów podczas pisania kodu.</span><span class="sxs-lookup"><span data-stu-id="1cec9-311">In Visual Studio 2012, you will notice that the editor automatically indents the elements while you are writing the code.</span></span>

> [!NOTE]
> <span data-ttu-id="1cec9-312">W poprzedniej wersji programu Visual Studio, inteligentne wcięcie była dostępna w edytorze XML, ale nie w edytorze HTML.</span><span class="sxs-lookup"><span data-stu-id="1cec9-312">In previous version of Visual Studio, smart indentation was available in the XML editor but not in the HTML editor.</span></span>


1. <span data-ttu-id="1cec9-313">Upewnij się, że konfiguracja Indenting w edytorze HTML ma ustawioną wartość inteligentne wcięcie.</span><span class="sxs-lookup"><span data-stu-id="1cec9-313">Make sure that the Indenting configuration on the HTML Editor is set to Smart Indentation.</span></span> <span data-ttu-id="1cec9-314">W tym celu wybierz **narzędzia | Opcje** , a następnie wybierz opcję menu **Edytor tekstu | HTML | Karty** strony w okienku po lewej stronie ekranu.</span><span class="sxs-lookup"><span data-stu-id="1cec9-314">To do that, select the **Tools | Options** menu option and then select the **Text Editor | HTML | Tabs** page in the left pane of the screen.</span></span> <span data-ttu-id="1cec9-315">Wybierz opcję inteligentne wcięcie.</span><span class="sxs-lookup"><span data-stu-id="1cec9-315">Select the Smart indentation option.</span></span>

    <span data-ttu-id="1cec9-316">![Ustawienia edytora HTML](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image33.png "ustawienia edytora HTML")</span><span class="sxs-lookup"><span data-stu-id="1cec9-316">![HTML Editor settings](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image33.png "HTML Editor settings")</span></span>

    <span data-ttu-id="1cec9-317">*Ustawienia edytora HTML*</span><span class="sxs-lookup"><span data-stu-id="1cec9-317">*HTML Editor settings*</span></span>
2. <span data-ttu-id="1cec9-318">Na **Default.aspx** pozycję Usuń całą zawartość w elemencie audio.</span><span class="sxs-lookup"><span data-stu-id="1cec9-318">On the **Default.aspx** page, remove all the content under the audio element.</span></span>
3. <span data-ttu-id="1cec9-319">Umieść kursor na końcu otwarcia **audio** element i naciśnij klawisz **ENTER**.</span><span class="sxs-lookup"><span data-stu-id="1cec9-319">Place the cursor at the end of the opening **audio** element and hit **ENTER**.</span></span>

    <span data-ttu-id="1cec9-320">Zwróć uwagę, że nowe położenie kursora ma poziom dodatkowego wcięcia.</span><span class="sxs-lookup"><span data-stu-id="1cec9-320">Notice that the new position of cursor has an additional indentation level.</span></span>

    <span data-ttu-id="1cec9-321">![Inteligentne wcięcie w edytorze HTML](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image34.png "inteligentne wcięcie w edytorze HTML")</span><span class="sxs-lookup"><span data-stu-id="1cec9-321">![Smart indentation in the HTML Editor](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image34.png "Smart indentation in the HTML Editor")</span></span>

    <span data-ttu-id="1cec9-322">*Inteligentne wcięcie w edytorze HTML*</span><span class="sxs-lookup"><span data-stu-id="1cec9-322">*Smart indentation in the HTML Editor*</span></span>
4. <span data-ttu-id="1cec9-323">Przywróć audio tagu z zawartością zostały usunięte lub Zamknij **Default.aspx** bez zapisywania zmian.</span><span class="sxs-lookup"><span data-stu-id="1cec9-323">Restore the audio tag with the content you have removed, or close **Default.aspx** without saving the changes.</span></span>

<a id="Ex2Task6"></a>

<a id="Task_6_-_Extract_to_User_Control"></a>
#### <a name="task-6---extract-to-user-control"></a><span data-ttu-id="1cec9-324">Zadanie 6 - Wyodrębnij do kontrolki użytkownika</span><span class="sxs-lookup"><span data-stu-id="1cec9-324">Task 6 - Extract to User Control</span></span>

<span data-ttu-id="1cec9-325">Narzędzia Refactoring uwzględnione w programie Visual Studio, takich jak wyodrębnianie fragmentu kodu do funkcji, są wspaniałymi funkcjami, które ułatwiają poprawy i refaktoryzacji istniejącego kodu.</span><span class="sxs-lookup"><span data-stu-id="1cec9-325">The Refactoring tools included in Visual Studio, such as extracting a portion of code to a function, are great features that facilitate the improvement and the refactoring the existing code.</span></span> <span data-ttu-id="1cec9-326">Odpowiednikiem stron ASP.NET jest wyodrębniania kodu HTML do kontrolki użytkownika.</span><span class="sxs-lookup"><span data-stu-id="1cec9-326">The counterpart for ASP.NET pages would be the extraction of HTML code to a User Control.</span></span> <span data-ttu-id="1cec9-327">To zrobić ręcznie wymagałoby kilku czynności, takie jak tworzenie nowej kontrolki użytkownika, przenoszenie sekcji kodu do kontrolki użytkownika, rejestrowanie prefiksu tagu kontrolki użytkownika i, koniec uruchamianiu formantu użytkownika na stronach.</span><span class="sxs-lookup"><span data-stu-id="1cec9-327">Doing it manually would involve several steps, like creating a new User Control, moving the code section to the User Control, registering a tag prefix for the User Control, and, finally, instantiating the User Control on the pages.</span></span> <span data-ttu-id="1cec9-328">Teraz, nowe *Wyodrębnij do kontrolki użytkownika* narzędzie automatycznie wykonuje te kroki dla Ciebie.</span><span class="sxs-lookup"><span data-stu-id="1cec9-328">Now, the new *Extract to User Control* tool automatically performs all those steps for you.</span></span>

<span data-ttu-id="1cec9-329">To zadanie będzie używał nowego wyodrębniania operacji kontekstowe kontrolki użytkownika do generowania nową kontrolkę użytkownika z zaznaczonym kodzie.</span><span class="sxs-lookup"><span data-stu-id="1cec9-329">In this task, you will use the new Extract to User Control contextual operation to generate a new user control from the selected code.</span></span>

1. <span data-ttu-id="1cec9-330">Na **Default.aspx** wybierz pozycję **H2** i **audio** elementów.</span><span class="sxs-lookup"><span data-stu-id="1cec9-330">On the **Default.aspx** page, select the **H2** and **audio** elements.</span></span>
2. <span data-ttu-id="1cec9-331">Kliknij prawym przyciskiem myszy i wybierz **Wyodrębnij do kontrolki użytkownika**.</span><span class="sxs-lookup"><span data-stu-id="1cec9-331">Right click and select **Extract to User Control**.</span></span>

    <span data-ttu-id="1cec9-332">![Wyodrębnij do kontrolki użytkownika menu opcji](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image35.png "Wyodrębnij do kontrolki użytkownika opcji menu")</span><span class="sxs-lookup"><span data-stu-id="1cec9-332">![Extract to User Control menu option](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image35.png "Extract to User Control menu option")</span></span>

    <span data-ttu-id="1cec9-333">*Wyodrębnij do kontrolki użytkownika opcji menu*</span><span class="sxs-lookup"><span data-stu-id="1cec9-333">*Extract to User Control menu option*</span></span>
3. <span data-ttu-id="1cec9-334">Wpisz nazwę nowego formantu użytkownika.</span><span class="sxs-lookup"><span data-stu-id="1cec9-334">Type a name for the new user control.</span></span> <span data-ttu-id="1cec9-335">Na przykład **Jukebox.ascx**, a następnie kliknij przycisk **OK**.</span><span class="sxs-lookup"><span data-stu-id="1cec9-335">For instance, **Jukebox.ascx**, and then click **OK**.</span></span>

    <span data-ttu-id="1cec9-336">![Zapisywanie kontrolki użytkownika wyodrębnionego](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image36.png "zapisywania kontrolki wyodrębnionego użytkownika")</span><span class="sxs-lookup"><span data-stu-id="1cec9-336">![Saving the extracted user control](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image36.png "Saving the extracted user control")</span></span>

    <span data-ttu-id="1cec9-337">*Zapisywanie kontrolki wyodrębnionego użytkownika*</span><span class="sxs-lookup"><span data-stu-id="1cec9-337">*Saving the extracted user control*</span></span>
4. <span data-ttu-id="1cec9-338">Zauważyć, że zaznaczony kod został wyodrębniony do kontrolki użytkownika, a Pierwotna lokalizacja zaznaczony kod został zastąpiony wystąpienia nowe kontrolki użytkownika.</span><span class="sxs-lookup"><span data-stu-id="1cec9-338">Notice that the selected code was extracted to a user control and the original location of the selected code was replaced with an instance of the new user control.</span></span>

    <span data-ttu-id="1cec9-339">![Strona automatycznie aktualizowane do użycia nowej kontrolki użytkownika](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image37.png "strony są automatycznie aktualizowane do użycia nowej kontrolki użytkownika")</span><span class="sxs-lookup"><span data-stu-id="1cec9-339">![Page automatically updated to use the new user control](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image37.png "Page automatically updated to use the new user control")</span></span>

    <span data-ttu-id="1cec9-340">*Strona automatycznie aktualizowane do użycia nowej kontrolki użytkownika*</span><span class="sxs-lookup"><span data-stu-id="1cec9-340">*Page automatically updated to use the new user control*</span></span>
5. <span data-ttu-id="1cec9-341">Naciśnij klawisz **F5** do uruchomienia strony i sprawdź, czy formant działa.</span><span class="sxs-lookup"><span data-stu-id="1cec9-341">Press **F5** to run the page and verify that the control works.</span></span>

<a id="Exercise3"></a>

<a id="Exercise_3_Whats_New_in_the_JavaScript_Editor"></a>
### <a name="exercise-3-whats-new-in-the-javascript-editor"></a><span data-ttu-id="1cec9-342">Ćwiczenie 3: Nowości w edytorze kodu JavaScript</span><span class="sxs-lookup"><span data-stu-id="1cec9-342">Exercise 3: What's New in the JavaScript Editor</span></span>

<span data-ttu-id="1cec9-343">Zapisywanie lub edycji kodu JavaScript nie jest łatwym zadaniem, szczególnie, gdy aplikacja rozpoczyna się zwiększa się rozmiar i okaże się, że zajmowanie długie pliki oraz setki funkcji.</span><span class="sxs-lookup"><span data-stu-id="1cec9-343">Writing or editing JavaScript code is not an easy task, especially when your application starts to grow in size and you find yourself dealing with long files and hundreds of functions.</span></span> <span data-ttu-id="1cec9-344">Zwykle programiści skryptu muszą wykonywania dodatkowej pracy, aby zachować czytelność kodu i przechodzić między plików.</span><span class="sxs-lookup"><span data-stu-id="1cec9-344">Script developers usually have to do some extra work to maintain code legibility and navigate across files.</span></span> <span data-ttu-id="1cec9-345">Obejmująca biblioteki języka JavaScript, takich jak jQuery nawigacji skryptu stał się żądanie się z powodu długość kodu.</span><span class="sxs-lookup"><span data-stu-id="1cec9-345">With the inclusion of JavaScript libraries like jQuery, script navigation has become a challenge itself because of the code length.</span></span>

<span data-ttu-id="1cec9-346">Visual Studio ma odnowiony edytora kodu JavaScript z promise dokonanie tryb kodu, dostępnych i organizowane.</span><span class="sxs-lookup"><span data-stu-id="1cec9-346">Visual Studio has renewed the JavaScript editor with the promise to make the code mode accessible and organized.</span></span> <span data-ttu-id="1cec9-347">Wiele funkcji programu Visual Studio, które istniały w językach C# i VB edytory są teraz zaimplementowane w edytorze kodu JavaScript: Przejdź do definicji, automatyczne wcięcia, dokumentacji i sprawdzania poprawności podczas pisania.</span><span class="sxs-lookup"><span data-stu-id="1cec9-347">Many Visual Studio features that already existed in C# or VB editors are now implemented in the JavaScript editor: Go To Definition, automatic indentation, documentation and validation when you are writing.</span></span> <span data-ttu-id="1cec9-348">Z listy IntelliSense odnowionego będziesz mieć katalogu funkcji JavaScript w zasięgu ręki.</span><span class="sxs-lookup"><span data-stu-id="1cec9-348">With the renewed IntelliSense list you will have the JavaScript function catalog at your fingertips.</span></span>

<span data-ttu-id="1cec9-349">W tym ćwiczeniu dowiesz się, niektóre nowe funkcje i ulepszenia edytora kodu JavaScript.</span><span class="sxs-lookup"><span data-stu-id="1cec9-349">In this exercise, you will learn some of the new features and improvements of JavaScript editor.</span></span> <span data-ttu-id="1cec9-350">Zostanie przeglądanie przykładowych plików i odnajdywanie wszystkich nowych cech, które spowoduje, że Twoje programowania w języku JavaScript większą wydajność w programie Visual Studio 2012.</span><span class="sxs-lookup"><span data-stu-id="1cec9-350">You will browse sample files and discover each of the new characteristics that will make your JavaScript programming more efficient within Visual Studio 2012.</span></span>

<a id="Ex3Task1"></a>

<a id="Task_1_-_JavaScript_Editor_New_Features"></a>
#### <a name="task-1---javascript-editor-new-features"></a><span data-ttu-id="1cec9-351">Zadanie 1 — nowe funkcje edytora JavaScript</span><span class="sxs-lookup"><span data-stu-id="1cec9-351">Task 1 - JavaScript Editor New Features</span></span>

<span data-ttu-id="1cec9-352">To zadanie przedstawiono niektóre nowe funkcje edytora JavaScript, które skupić się na przechowywanie kodu i dostarczają lepsze środowisko pracy użytkownika.</span><span class="sxs-lookup"><span data-stu-id="1cec9-352">This task will introduce you to some of the new JavaScript editor features, which focus on organizing your code and bringing a better user experience.</span></span>

1. <span data-ttu-id="1cec9-353">Jeśli nie jest jeszcze otwarty, uruchom **programu Visual Studio** , a następnie otwórz **WhatsNewASPNET.sln** rozwiązania, znajdujących się w **Source\WhatsNewASPNET** folder tego laboratorium.</span><span class="sxs-lookup"><span data-stu-id="1cec9-353">If not already opened, start **Visual Studio** and open the **WhatsNewASPNET.sln** solution located in the **Source\WhatsNewASPNET** folder of this lab.</span></span>
2. <span data-ttu-id="1cec9-354">Naciśnij klawisz **F5** do uruchomienia aplikacji, a następnie kliknij łącze JavaScript na pasku nawigacyjnym.</span><span class="sxs-lookup"><span data-stu-id="1cec9-354">Press **F5** to run the application, then click the JavaScript link in the navigation bar.</span></span> <span data-ttu-id="1cec9-355">Odśwież stronę kilka razy i sprawdź, jak zwiększa licznik.</span><span class="sxs-lookup"><span data-stu-id="1cec9-355">Refresh the page several times and check how the counter increments.</span></span>

    <span data-ttu-id="1cec9-356">![Licznik strony](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image38.png "licznika na stronie")</span><span class="sxs-lookup"><span data-stu-id="1cec9-356">![Page counter](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image38.png "Page counter")</span></span>

    <span data-ttu-id="1cec9-357">*Licznik na stronie*</span><span class="sxs-lookup"><span data-stu-id="1cec9-357">*Page counter*</span></span>
3. <span data-ttu-id="1cec9-358">Zamknij przeglądarkę i przejdź wstecz do programu Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="1cec9-358">Close the browser and go back to Visual Studio.</span></span>
4. <span data-ttu-id="1cec9-359">Otwórz **JavaScript.aspx** strony i Znajdź  **&lt;skryptu&gt;**  bloku (pokazana poniżej).</span><span class="sxs-lookup"><span data-stu-id="1cec9-359">Open the **JavaScript.aspx** page and locate the **&lt;script&gt;** block (shown below).</span></span>

    <span data-ttu-id="1cec9-360">W poniższym kodzie użyto HTML5 lokalnego magazynu do przechowywania *pageLoadCount* zmiennej, która przechowuje numer odwiedza strony przez bieżącego użytkownika.</span><span class="sxs-lookup"><span data-stu-id="1cec9-360">The following code uses HTML5 local storage to store a *pageLoadCount* variable that stores the number of times the page has been visited by the current user.</span></span> <span data-ttu-id="1cec9-361">Magazyn lokalny jest bazą danych klucz wartość po stronie klienta wprowadzone ze standardem HTML5.</span><span class="sxs-lookup"><span data-stu-id="1cec9-361">Local Storage is a client-side key-value database introduced with the HTML5 standard.</span></span> <span data-ttu-id="1cec9-362">Dane są zapisywane na komputerze lokalnym, w przeglądarce.</span><span class="sxs-lookup"><span data-stu-id="1cec9-362">The data is saved on the local machine, inside the user's browser.</span></span>

    [!code-html[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample7.html)]

    > [!NOTE]
    > <span data-ttu-id="1cec9-363">Upewnij się, że DOCTYPE ustawiono XHTML5 przed wykonaniem dalszych kroków.</span><span class="sxs-lookup"><span data-stu-id="1cec9-363">Ensure the DOCTYPE is set to XHTML5 before proceeding with the next steps.</span></span>
5. <span data-ttu-id="1cec9-364">Edytuj kod i zwróć uwagę, że IntelliSense dla JavaScript zawiera funkcje HTML5, takie jak i ich metod wewnętrzny magazyn lokalny.</span><span class="sxs-lookup"><span data-stu-id="1cec9-364">Edit the code and notice that IntelliSense for JavaScript includes HTML5 features, like local storage, and their inner methods.</span></span>

    <span data-ttu-id="1cec9-365">![Funkcje HTML5 JavaScript w języku JavaScript](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image39.png "funkcji HTML5 JavaScript w języku JavaScript")</span><span class="sxs-lookup"><span data-stu-id="1cec9-365">![HTML5 JavaScript features in JavaScript](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image39.png "HTML5 JavaScript features in JavaScript")</span></span>

    <span data-ttu-id="1cec9-366">*Funkcje HTML5 JavaScript w języku JavaScript*</span><span class="sxs-lookup"><span data-stu-id="1cec9-366">*HTML5 JavaScript features in JavaScript*</span></span>
6. <span data-ttu-id="1cec9-367">Kliknij żadnych nawiasu otwierającego (**{**) z skryptów kodu i zwróć uwagę, że nawiasy są wyróżnione.</span><span class="sxs-lookup"><span data-stu-id="1cec9-367">Click any opening bracket (**{**) from the scripting code and notice that the brackets are highlighted.</span></span>

    <span data-ttu-id="1cec9-368">![Nawiasy kwadratowe są wyróżnione](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image40.png "nawiasy kwadratowe są wyróżnione")</span><span class="sxs-lookup"><span data-stu-id="1cec9-368">![Brackets are highlighted](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image40.png "Brackets are highlighted")</span></span>

    <span data-ttu-id="1cec9-369">*Nawiasy kwadratowe są wyróżnione*</span><span class="sxs-lookup"><span data-stu-id="1cec9-369">*Brackets are highlighted*</span></span>
7. <span data-ttu-id="1cec9-370">Usuń znaczniki komentarza funkcji **testAutoAlign()** (Wybierz trzy wiersze i można użyć **CTRL** + **K**; **CTRL** + **U**) i Znajdź kursora wewnątrz kodu funkcji.</span><span class="sxs-lookup"><span data-stu-id="1cec9-370">Uncomment the function **testAutoAlign()** (select the three lines and you can use **CTRL** + **K**; **CTRL** + **U**) and locate the cursor inside the function code.</span></span> <span data-ttu-id="1cec9-371">Naciśnij klawisz enter, aby można było dodać drugi wiersz.</span><span class="sxs-lookup"><span data-stu-id="1cec9-371">Press enter to append a second line.</span></span> <span data-ttu-id="1cec9-372">Należy zauważyć, że kod jest teraz **wyrównane** i **wcięty automatycznie**.</span><span class="sxs-lookup"><span data-stu-id="1cec9-372">Notice that the code is now **aligned** and **auto-indented**.</span></span>

    <span data-ttu-id="1cec9-373">![Kod JavaScript jest automatycznie wyrównane](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image41.png "kod JavaScript jest automatycznie wyrównane")</span><span class="sxs-lookup"><span data-stu-id="1cec9-373">![JavaScript code is auto aligned](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image41.png "JavaScript code is auto aligned")</span></span>

    <span data-ttu-id="1cec9-374">*Kod JavaScript jest automatycznie wyrównane*</span><span class="sxs-lookup"><span data-stu-id="1cec9-374">*JavaScript code is auto aligned*</span></span>

<a id="Ex3Task2"></a>

<a id="Task_2_-_Validating_JavaScript"></a>
#### <a name="task-2---validating-javascript"></a><span data-ttu-id="1cec9-375">Zadanie 2 — Sprawdzanie poprawności kodu JavaScript</span><span class="sxs-lookup"><span data-stu-id="1cec9-375">Task 2 - Validating JavaScript</span></span>

<span data-ttu-id="1cec9-376">W tym zadaniu odnajdzie nowy walidacji JavaScript dla standardowej ECMAScript5.</span><span class="sxs-lookup"><span data-stu-id="1cec9-376">In this task, you will discover the new JavaScript validation for the ECMAScript5 standard.</span></span> <span data-ttu-id="1cec9-377">Ta funkcja będzie ułatwia pisanie zgodne kod JavaScript, podczas zapobiega zagadnienia dotyczące skryptów przed wdrożeniem witryny.</span><span class="sxs-lookup"><span data-stu-id="1cec9-377">This feature will help you to write compliant JavaScript code, while preventing scripting issues before site deployment.</span></span>

> [!NOTE]
> <span data-ttu-id="1cec9-378">Program Visual Studio 2010 zaimplementowana zgodności ECMAStript3 programu Visual Studio 2012 zapewnia zgodność ECMAScript5 jednocześnie.</span><span class="sxs-lookup"><span data-stu-id="1cec9-378">Visual Studio 2010 implemented ECMAStript3 compliance, while Visual Studio 2012 provides ECMAScript5 compliance.</span></span>


1. <span data-ttu-id="1cec9-379">Otwórz **ECMA5script5.js** znajduje się w obszarze **Scripts\custom** folderu projektu.</span><span class="sxs-lookup"><span data-stu-id="1cec9-379">Open **ECMA5script5.js** located under the **Scripts\custom** project folder.</span></span> <span data-ttu-id="1cec9-380">Teraz zostanie testów sprawdzania poprawności dla ECMAScript5 standardowe.</span><span class="sxs-lookup"><span data-stu-id="1cec9-380">You will now test validation for ECMAScript5 standard.</span></span>

    [!code-html[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample8.html)]

    <span data-ttu-id="1cec9-381">Można wyewidencjonować &quot; **używaj z ograniczeniami** &quot; kierunek, w pierwszym wierszu pliku, który umożliwia ECMAScript5 **tryb z ograniczeniami**.</span><span class="sxs-lookup"><span data-stu-id="1cec9-381">You can check out the &quot; **use strict** &quot; direction in the first line of the file, which enables ECMAScript5 **strict mode**.</span></span> <span data-ttu-id="1cec9-382">W tym trybie składa się z podzbioru języka, który wyjaśnia niejednoznaczności z poprzednich wersji i dodaje nowe funkcje, takie jak metody pobierające i ustawiające, obsługa bibliotek dla formatu JSON, a dokładniejsza odbicia właściwości obiektu.</span><span class="sxs-lookup"><span data-stu-id="1cec9-382">This mode consists in a subset of the language that clarifies ambiguities from the past edition, and adds some new features, such as getters and setters, library support for JSON, and more complete reflection on object properties.</span></span>
2. <span data-ttu-id="1cec9-383">Otwórz **listy błędów** Jeśli nie jest jeszcze otwarty (**widoku** menu | **Listy błędów**).</span><span class="sxs-lookup"><span data-stu-id="1cec9-383">Open the **Error List** if not already opened (**View** menu | **Error List**).</span></span> <span data-ttu-id="1cec9-384">Powiadomienie **funkcja** deklaracji jest podkreślony.</span><span class="sxs-lookup"><span data-stu-id="1cec9-384">Notice the **function** declaration is underlined.</span></span> <span data-ttu-id="1cec9-385">Jest to spowodowane w ECMA5 standardowych funkcji nie mogą być zagnieżdżone wewnątrz struktury języka.</span><span class="sxs-lookup"><span data-stu-id="1cec9-385">This is because in ECMA5 standard functions cannot be nested inside language structures.</span></span> <span data-ttu-id="1cec9-386">W dzienniku błędów poniżej możesz pojawią się szczegóły ostrzeżenie.</span><span class="sxs-lookup"><span data-stu-id="1cec9-386">In the error list below you will see the warning details.</span></span>

    <span data-ttu-id="1cec9-387">![Komunikat o błędzie weryfikacji JavaScript](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image42.png "komunikat o błędzie weryfikacji JavaScript")</span><span class="sxs-lookup"><span data-stu-id="1cec9-387">![JavaScript validation error message](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image42.png "JavaScript validation error message")</span></span>

    <span data-ttu-id="1cec9-388">*Komunikat o błędzie weryfikacji JavaScript*</span><span class="sxs-lookup"><span data-stu-id="1cec9-388">*JavaScript validation error message*</span></span>
3. <span data-ttu-id="1cec9-389">Komentarz  **&quot;używaj z ograniczeniami&quot;**  kierunek i zwróć uwagę, że błędy znikają, przy zachowaniu ostrzeżenia.</span><span class="sxs-lookup"><span data-stu-id="1cec9-389">Comment out the **&quot;use strict&quot;** direction and notice that errors disappear, but the warnings remain.</span></span>
4. <span data-ttu-id="1cec9-390">W ostatnim wierszu pliku, należy zapisać dowolny ciąg, takich jak  **&quot;test&quot;**  (należy używać znaków cudzysłowu, aby wskazać ją jako ciąg).</span><span class="sxs-lookup"><span data-stu-id="1cec9-390">In the last line of the file, write any string like **&quot;test&quot;** (include the quotation marks to indicate it is as string).</span></span> <span data-ttu-id="1cec9-391">Zapis okresu obok ciąg do wyświetlania na liście IntelliSense i wybierz **trim** opcji.</span><span class="sxs-lookup"><span data-stu-id="1cec9-391">Write a period next to the string to display the IntelliSense list, and select the **trim** option.</span></span>

    <span data-ttu-id="1cec9-392">W standardzie ECMAScript5 wartości parametrów i zmiennych również mieć zdefiniowane, takich jak trim, wielkie litery, wyszukiwania i zamieniania metod ciągów.</span><span class="sxs-lookup"><span data-stu-id="1cec9-392">In ECMAScript5 standard, string values and variables also have string methods defined, like trim, uppercase, search and replace.</span></span>

    <span data-ttu-id="1cec9-393">![Lista IntelliSense w języku JavaScript](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image43.png "listy IntelliSense w języku JavaScript")</span><span class="sxs-lookup"><span data-stu-id="1cec9-393">![IntelliSense list in JavaScript](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image43.png "IntelliSense list in JavaScript")</span></span>

    <span data-ttu-id="1cec9-394">*Lista IntelliSense w języku JavaScript*</span><span class="sxs-lookup"><span data-stu-id="1cec9-394">*IntelliSense list in JavaScript*</span></span>

<a id="Ex3Task3"></a>

<a id="Task_3_-_XML_Documentation_for_JavaScript"></a>
#### <a name="task-3---xml-documentation-for-javascript"></a><span data-ttu-id="1cec9-395">Zadanie 3. plik dokumentacji XML JavaScript</span><span class="sxs-lookup"><span data-stu-id="1cec9-395">Task 3 - XML Documentation for JavaScript</span></span>

<span data-ttu-id="1cec9-396">W tym zadaniu zostanie Poznaj funkcje programu Visual Studio dla dokumentacji XML w języku JavaScript.</span><span class="sxs-lookup"><span data-stu-id="1cec9-396">In this task, you will explore Visual Studio features for XML documentation in JavaScript.</span></span> <span data-ttu-id="1cec9-397">Zobaczysz, że lista IntelliSense dla JavaScript zawiera teraz dokumentacji XML każdej funkcji.</span><span class="sxs-lookup"><span data-stu-id="1cec9-397">You will see the JavaScript IntelliSense list now shows the XML documentation of each function.</span></span> <span data-ttu-id="1cec9-398">Ponadto możesz zauważyć funkcję nawigacji w języku JavaScript.</span><span class="sxs-lookup"><span data-stu-id="1cec9-398">Additionally, you will discover the navigation feature in JavaScript.</span></span>

1. <span data-ttu-id="1cec9-399">Otwórz **XMLDoc.js** plik znajdujący się w **skryptów/niestandardowa** folderu projektu.</span><span class="sxs-lookup"><span data-stu-id="1cec9-399">Open **XMLDoc.js** file located in **Scripts/custom** project folder.</span></span> <span data-ttu-id="1cec9-400">Ten plik zawiera dokumentacja XML na każdej z tych funkcji JavaScript.</span><span class="sxs-lookup"><span data-stu-id="1cec9-400">This file contains XML documentation on each of the JavaScript functions.</span></span>

    <span data-ttu-id="1cec9-401">![Plik dokumentacji JavaScript XML zintegrowany z IntelliSense](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image44.png "dokumentacji JavaScript XML zintegrowany z IntelliSense")</span><span class="sxs-lookup"><span data-stu-id="1cec9-401">![JavaScript XML documentation integrated to IntelliSense](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image44.png "JavaScript XML documentation integrated to IntelliSense")</span></span>

    <span data-ttu-id="1cec9-402">*Plik dokumentacji JavaScript XML zintegrowany z IntelliSense*</span><span class="sxs-lookup"><span data-stu-id="1cec9-402">*JavaScript XML documentation integrated to IntelliSense*</span></span>
2. <span data-ttu-id="1cec9-403">Poniżej **dodać** działać w **XMLDoc.js** plików, Utwórz nową funkcję o nazwie **testu**.</span><span class="sxs-lookup"><span data-stu-id="1cec9-403">Below **add** function in **XMLDoc.js** file, create a new function named **test**.</span></span>
3. <span data-ttu-id="1cec9-404">W **test** funkcji, należy wywołać **mnożenia** funkcja, która otrzymuje dwa parametry.</span><span class="sxs-lookup"><span data-stu-id="1cec9-404">In the **test** function, call the **multiply** function that receives two parameters.</span></span> <span data-ttu-id="1cec9-405">Zwróć uwagę, pole tooltip jest pokazywany **mnożenia** funkcji dokumentacji.</span><span class="sxs-lookup"><span data-stu-id="1cec9-405">Notice the tooltip box is showing the **multiply** function documentation.</span></span>

    [!code-javascript[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample9.js)]

    <span data-ttu-id="1cec9-406">![Plik dokumentacji XML dla funkcji JavaScript](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image45.png "dokumentacji XML dla funkcji JavaScript")</span><span class="sxs-lookup"><span data-stu-id="1cec9-406">![XML documentation for JavaScript functions](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image45.png "XML documentation for JavaScript functions")</span></span>

    <span data-ttu-id="1cec9-407">*Plik dokumentacji XML dla funkcji JavaScript*</span><span class="sxs-lookup"><span data-stu-id="1cec9-407">*XML documentation for JavaScript functions*</span></span>
4. <span data-ttu-id="1cec9-408">Zakończenie instrukcji call funkcji i typ *kropka* aby otworzyć listę IntelliSense w zwracanej wartości.</span><span class="sxs-lookup"><span data-stu-id="1cec9-408">Complete the function call statement and type a *dot* to open the IntelliSense list on the returned value.</span></span> <span data-ttu-id="1cec9-409">Należy zauważyć, że program Visual Studio jest wykrywanie **zwracać** wartość w dokumentacji, traktując wartości jako liczby.</span><span class="sxs-lookup"><span data-stu-id="1cec9-409">Notice that Visual Studio is detecting the **return** value in the documentation, treating the value as a number.</span></span>

    <span data-ttu-id="1cec9-410">![Plik dokumentacji XML dla zwracanych typów](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image46.png "dokumentacji XML dla zwracane typy")</span><span class="sxs-lookup"><span data-stu-id="1cec9-410">![XML documentation for return types](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image46.png "XML documentation for return types")</span></span>

    <span data-ttu-id="1cec9-411">*Plik dokumentacji XML dla zwracane typy*</span><span class="sxs-lookup"><span data-stu-id="1cec9-411">*XML documentation for return types*</span></span>
5. <span data-ttu-id="1cec9-412">Teraz Wstawianie wywołania do add — funkcja.</span><span class="sxs-lookup"><span data-stu-id="1cec9-412">Now, insert a call to add function.</span></span> <span data-ttu-id="1cec9-413">Zwróć uwagę, że edytor JavaScript obsługuje teraz przeciążenia funkcji.</span><span class="sxs-lookup"><span data-stu-id="1cec9-413">Notice that the JavaScript editor now supports function overloads.</span></span> <span data-ttu-id="1cec9-414">Podczas pisania nazwę funkcji, można wybrać dowolny z dostępnych przeciążeń określony w dokumentacji.</span><span class="sxs-lookup"><span data-stu-id="1cec9-414">When you write a function name, you will be able to select any of the available overloads specified in the documentation.</span></span>

    <span data-ttu-id="1cec9-415">![Dokumentacja XML do przeciążenia](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image47.png "dokumentacji XML dla przeciążenia")</span><span class="sxs-lookup"><span data-stu-id="1cec9-415">![XML documentation for overloads](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image47.png "XML documentation for overloads")</span></span>

    <span data-ttu-id="1cec9-416">*Dokumentacja XML do przeciążenia*</span><span class="sxs-lookup"><span data-stu-id="1cec9-416">*XML documentation for overloads*</span></span>
6. <span data-ttu-id="1cec9-417">Otwórz **GotoDefinition.js** plików i Znajdź **$().html()** wywołania funkcji.</span><span class="sxs-lookup"><span data-stu-id="1cec9-417">Open **GotoDefinition.js** file and locate the **$().html()** function call.</span></span> <span data-ttu-id="1cec9-418">Zlokalizuj kursor na **html**.</span><span class="sxs-lookup"><span data-stu-id="1cec9-418">Locate the cursor on **html**.</span></span>
7. <span data-ttu-id="1cec9-419">Naciśnij klawisz **F12** i przejdź do definicji.</span><span class="sxs-lookup"><span data-stu-id="1cec9-419">Press **F12** and navigate to the definition.</span></span> <span data-ttu-id="1cec9-420">Powiadomienia można teraz uzyskać dostępu i przeglądać kod JavaScript bez użycia **znaleźć** narzędzia.</span><span class="sxs-lookup"><span data-stu-id="1cec9-420">Notice you can now access and browse your JavaScript code without using the **Find** tool.</span></span>
8. <span data-ttu-id="1cec9-421">Zlokalizuj kursor w instrukcji jQuery przed blok podpisu w dolnej części pliku kodu.</span><span class="sxs-lookup"><span data-stu-id="1cec9-421">Locate the cursor on the jQuery instruction prior to the signature block at the bottom of the code file.</span></span> <span data-ttu-id="1cec9-422">Naciśnij klawisz **F12**.</span><span class="sxs-lookup"><span data-stu-id="1cec9-422">Press **F12**.</span></span> <span data-ttu-id="1cec9-423">Nastąpi przeniesienie do pliku biblioteki jQuery.</span><span class="sxs-lookup"><span data-stu-id="1cec9-423">You will navigate to the jQuery library file.</span></span> <span data-ttu-id="1cec9-424">Zwróć uwagę, można także przechodzić między pliki jQuery przy użyciu **F12**.</span><span class="sxs-lookup"><span data-stu-id="1cec9-424">Notice you can also navigate across the jQuery files using **F12**.</span></span>

    <span data-ttu-id="1cec9-425">![Przechodzenie do definicji jQuery](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image48.png "przejść do definicji jQuery")</span><span class="sxs-lookup"><span data-stu-id="1cec9-425">![Navigating to jQuery definitions](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image48.png "Navigating to jQuery definitions")</span></span>

    <span data-ttu-id="1cec9-426">*Przechodzenie do definicji jQuery*</span><span class="sxs-lookup"><span data-stu-id="1cec9-426">*Navigating to jQuery definitions*</span></span>

> [!NOTE]
> <span data-ttu-id="1cec9-427">Upewnij się, że GotoDefinition.js nie zawiera składnię błędów przed zapisaniem pliku.</span><span class="sxs-lookup"><span data-stu-id="1cec9-427">Make sure that GotoDefinition.js has no syntax errors before saving the file.</span></span>


<a id="Exercise4"></a>

<a id="Exercise_4_Bundling_and_Minification"></a>
### <a name="exercise-4-bundling-and-minification"></a><span data-ttu-id="1cec9-428">Ćwiczenie 4: Tworzenie pakietów i minimalizowanie</span><span class="sxs-lookup"><span data-stu-id="1cec9-428">Exercise 4: Bundling and Minification</span></span>

<span data-ttu-id="1cec9-429">Ile razy witryny sieci Web ma więcej niż jednego języka JavaScript lub CSS pliku?</span><span class="sxs-lookup"><span data-stu-id="1cec9-429">How many times do your websites include more than one JavaScript or CSS file?</span></span> <span data-ttu-id="1cec9-430">Jest to bardzo typowy scenariusz, w którym tworzenie pakietów i minimalizowanie może pomóc zmniejszyć rozmiar pliku i witryna szybsze.</span><span class="sxs-lookup"><span data-stu-id="1cec9-430">This is a very common scenario where bundling and minification can help to reduce the file size and make the site perform faster.</span></span> <span data-ttu-id="1cec9-431">Nowa funkcja powiązanego w programie ASP.NET 4.5 pakiety zestawu plików JS i CSS do pojedynczego elementu i zmniejsza rozmiar przez zminimalizowania liczby zawartości (tj. usuwanie spacje nie są wymagane, usuwanie komentarzy, zmniejszając identyfikatory).</span><span class="sxs-lookup"><span data-stu-id="1cec9-431">The new bundling feature in ASP.NET 4.5 packs a set of JS or CSS files into a single element, and reduces its size by minifying the content ( i.e. removing not required blank spaces, removing comments, reducing identifiers ).</span></span>

<span data-ttu-id="1cec9-432">Tworzenie pakietów i minimalizowanie w programie ASP.NET 4.5 jest wykonywane w czasie wykonywania, aby proces identyfikowania agent użytkownika (na przykład programu Internet Explorer, Mozilla itp.), a w związku z tym poprawy kompresji, wybierając w przeglądarce użytkownika (na przykład usuwania rzeczy będący Mozilla określonych Jeśli żądanie pochodzi z programu Internet Explorer).</span><span class="sxs-lookup"><span data-stu-id="1cec9-432">Bundling and minification in ASP.NET 4.5 is performed at runtime, so that the process can identify the user agent (for example IE, Mozilla, etc), and thus, improve the compression by targeting the user browser (for instance, removing stuff that is Mozilla specific when the request comes from IE).</span></span>

<span data-ttu-id="1cec9-433">W tym ćwiczeniu dowiesz się, jak włączyć i korzystać z różnego rodzaju tworzenie pakietów i minimalizowanie w programie ASP.NET 4.5.</span><span class="sxs-lookup"><span data-stu-id="1cec9-433">In this exercise, you will learn how to enable and use the different types of bundling and minification in ASP.NET 4.5.</span></span>

<a id="Ex4Task1"></a>

<a id="Task_1_-_Installing_the_Bundling_and_Minification_Package_from_NuGet"></a>
#### <a name="task-1---installing-the-bundling-and-minification-package-from-nuget"></a><span data-ttu-id="1cec9-434">Zadanie 1 — Instalowanie tworzenie pakietów i minimalizowanie pakiet NuGet</span><span class="sxs-lookup"><span data-stu-id="1cec9-434">Task 1 - Installing the Bundling and Minification Package from NuGet</span></span>

1. <span data-ttu-id="1cec9-435">Jeśli nie jest jeszcze otwarty, uruchom **programu Visual Studio** , a następnie otwórz **WhatsNewASPNET.sln** rozwiązania, znajdujących się w **Source\WhatsNewASPNET** folder tego laboratorium.</span><span class="sxs-lookup"><span data-stu-id="1cec9-435">If not already opened, start **Visual Studio** and open the **WhatsNewASPNET.sln** solution located in the **Source\WhatsNewASPNET** folder of this lab.</span></span>
2. <span data-ttu-id="1cec9-436">Otwórz konsolę Menedżera pakietów NuGet.</span><span class="sxs-lookup"><span data-stu-id="1cec9-436">Open the NuGet Package Manager Console.</span></span> <span data-ttu-id="1cec9-437">Aby to zrobić, użyj menu **widoku** | **inne okna** | **Konsola Menedżera pakietów**.</span><span class="sxs-lookup"><span data-stu-id="1cec9-437">To do this, use the menu **View** | **Other Windows** | **Package Manager Console**.</span></span>

    <span data-ttu-id="1cec9-438">![Otwieranie pakietu file:///C:/Users/User/AppData/Local/Temp/Marker3744//media/44462/Multiple-Stylesheets-and-JavaScript-files-in-the-application.pngconsole Menedżera](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image49.png "otwarcie konsoli Menedżera pakietów")</span><span class="sxs-lookup"><span data-stu-id="1cec9-438">![Opening the package manager file:///C:/Users/User/AppData/Local/Temp/Marker3744//media/44462/Multiple-Stylesheets-and-JavaScript-files-in-the-application.pngconsole](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image49.png "Opening the package manager console")</span></span>

    <span data-ttu-id="1cec9-439">*Otwieranie konsoli Menedżera pakietów*</span><span class="sxs-lookup"><span data-stu-id="1cec9-439">*Opening the package manager console*</span></span>
3. <span data-ttu-id="1cec9-440">W **Konsola Menedżera pakietów,** typu **Microsoft.Web.Optimization Install-Package** i naciśnij klawisz **ENTER**.</span><span class="sxs-lookup"><span data-stu-id="1cec9-440">In the **Package Manager Console,** type **Install-Package Microsoft.Web.Optimization** and press **ENTER**.</span></span>

<a id="Ex4Task2"></a>

<a id="Task_2_-_Default_Bundles"></a>
#### <a name="task-2---default-bundles"></a><span data-ttu-id="1cec9-441">Zadanie 2 — domyślne pakiety</span><span class="sxs-lookup"><span data-stu-id="1cec9-441">Task 2 - Default Bundles</span></span>

<span data-ttu-id="1cec9-442">Najprostszym sposobem użycia tworzenie pakietów i minimalizowanie jest umożliwienie pakiety z domyślnej.</span><span class="sxs-lookup"><span data-stu-id="1cec9-442">The simplest way to use bundling and minification is to enable the default bundles.</span></span> <span data-ttu-id="1cec9-443">Ta metoda używa konwencji, aby można było odwołać powiązane i zminimalizowany wersji JS i CSS pliki w folderze.</span><span class="sxs-lookup"><span data-stu-id="1cec9-443">This method uses conventions to let you reference the bundled and minified version for the JS and CSS files in a folder.</span></span>

<span data-ttu-id="1cec9-444">W tym zadaniu dowiesz się, jak włączyć i powiązane i zminimalizowany plików JS i CSS i wyświetlić dane wyjściowe.</span><span class="sxs-lookup"><span data-stu-id="1cec9-444">In this task, you will learn how to enable and reference the bundled and minified JS and CSS files and view the resulting output.</span></span>

1. <span data-ttu-id="1cec9-445">Jeśli nie jest jeszcze otwarty, uruchom **programu Visual Studio** , a następnie otwórz **WhatsNewASPNET.sln** rozwiązania, znajdujących się w **Source\WhatsNewASPNET** folder tego laboratorium.</span><span class="sxs-lookup"><span data-stu-id="1cec9-445">If not already opened, start **Visual Studio** and open the **WhatsNewASPNET.sln** solution located in the **Source\WhatsNewASPNET** folder of this lab.</span></span>
2. <span data-ttu-id="1cec9-446">W **Eksploratora rozwiązań**, rozwiń węzeł **style**, **Scripts\custom** i **Scripts\bundle** folderów.</span><span class="sxs-lookup"><span data-stu-id="1cec9-446">In the **Solution Explorer**, expand the **Styles**, **Scripts\custom** and **Scripts\bundle** folders.</span></span>

    <span data-ttu-id="1cec9-447">Zwróć uwagę, że aplikacja używa pliku więcej niż jeden CSS i JS.</span><span class="sxs-lookup"><span data-stu-id="1cec9-447">Notice that the application is using more than one CSS and JS file.</span></span>

    <span data-ttu-id="1cec9-448">![Pliki wielu arkusze stylów i JavaScript w aplikacji](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image50.png "pliki wielu arkusze stylów i JavaScript w aplikacji")</span><span class="sxs-lookup"><span data-stu-id="1cec9-448">![Multiple Stylesheets and JavaScript files in the application](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image50.png "Multiple Stylesheets and JavaScript files in the application")</span></span>

    <span data-ttu-id="1cec9-449">*Wiele plików arkusze stylów i JavaScript w aplikacji*</span><span class="sxs-lookup"><span data-stu-id="1cec9-449">*Multiple Stylesheets and JavaScript files in the application*</span></span>
3. <span data-ttu-id="1cec9-450">Otwórz **Global.asax.cs** pliku.</span><span class="sxs-lookup"><span data-stu-id="1cec9-450">Open the **Global.asax.cs** file.</span></span>

    <span data-ttu-id="1cec9-451">Zwróć uwagę, że nowe **Microsoft.Web.Optimization** przestrzeni nazw jest oznaczone jako komentarz na początku pliku.</span><span class="sxs-lookup"><span data-stu-id="1cec9-451">Notice that the new **Microsoft.Web.Optimization** namespace is commented out at the beginning of the file.</span></span> <span data-ttu-id="1cec9-452">Usuń znaczniki komentarza użycie dyrektywy w celu włączenia funkcji Tworzenie pakietów i minimalizowanie.</span><span class="sxs-lookup"><span data-stu-id="1cec9-452">Uncomment the using directive to include the bundling and minification features.</span></span>


    [!code-csharp[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample10.cs)]
4. <span data-ttu-id="1cec9-453">Zlokalizuj **aplikacji\_Start** metody.</span><span class="sxs-lookup"><span data-stu-id="1cec9-453">Locate the **Application\_Start** method.</span></span>

    <span data-ttu-id="1cec9-454">W przypadku tej metody usuń znaczniki komentarza wywołania EnableDefaultBundles, jak pokazano w poniższy fragment.</span><span class="sxs-lookup"><span data-stu-id="1cec9-454">In this method, uncomment the EnableDefaultBundles call as shown in the snippet below.</span></span> <span data-ttu-id="1cec9-455">Pozwala na odwołanie zbiór powiązane pliki CSS w folderze przy użyciu ścieżki do tego folderu i &quot;CSS&quot; lub &quot;JS&quot; sufiks.</span><span class="sxs-lookup"><span data-stu-id="1cec9-455">This enables us to reference a bundled collection of CSS files in a folder by using the path to that folder, plus the &quot;CSS&quot; or the &quot;JS&quot; suffix.</span></span>


    [!code-csharp[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample11.cs)]
5. <span data-ttu-id="1cec9-456">Otwórz **Optimization.aspx** pliku, a następnie zlokalizuj formant zawartości dla **HeadContent**.</span><span class="sxs-lookup"><span data-stu-id="1cec9-456">Open the **Optimization.aspx** file and locate the content control for **HeadContent**.</span></span>

    <span data-ttu-id="1cec9-457">Zwróć uwagę, pliki CSS i JS zawierać jeden tag do którego istnieje odwołanie.</span><span class="sxs-lookup"><span data-stu-id="1cec9-457">Notice the CSS files and the JS files have a single referenced tag.</span></span>


    [!code-aspx[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample12.aspx)]

    > [!NOTE]
    > <span data-ttu-id="1cec9-458">Ten kod jest dla celów demonstracyjnych.</span><span class="sxs-lookup"><span data-stu-id="1cec9-458">This code is for demo purposes.</span></span> <span data-ttu-id="1cec9-459">W idealnym przypadku będzie odwoływać pakietów w pliku Site.Master.</span><span class="sxs-lookup"><span data-stu-id="1cec9-459">Ideally, you will reference the bundles in the Site.Master file.</span></span> <span data-ttu-id="1cec9-460">W ten przykładowy kod można znaleźć czy niektóre pliki w pakiecie są również odwołuje pliku Site.Master odniesienia tego ostatniego nadmiarowe.</span><span class="sxs-lookup"><span data-stu-id="1cec9-460">In this sample code, you will find that some of the bundled files are also being referenced by the Site.Master file, making this last reference redundant.</span></span>
6. <span data-ttu-id="1cec9-461">Należy zauważyć, że łącza z powiązanego konwencje w **href** atrybutu Pobierz pliki CSS i Javascript z style i Scripts\custom folderu odpowiednio.</span><span class="sxs-lookup"><span data-stu-id="1cec9-461">Notice that the links are using the bundling conventions in the **href** attribute to get all the CSS or JS files from the Styles and Scripts\custom folder respectively.</span></span>

    <span data-ttu-id="1cec9-462">Można użyć ścieżki **skryptów/niestandardowe/JS** przedstawioną poniżej połączyć w paczkę i zminimalizowania wszystkie pliki JS wewnątrz **skryptów/niestandardowa** folderu.</span><span class="sxs-lookup"><span data-stu-id="1cec9-462">You can use the path **Scripts/custom/JS** as shown below to bundle and minify all the JS files inside a **Scripts/custom** folder.</span></span> <span data-ttu-id="1cec9-463">Jest to domyślne zachowanie z pakietami domyślne.</span><span class="sxs-lookup"><span data-stu-id="1cec9-463">This is the default behavior with the default bundles.</span></span>


    [!code-aspx[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample13.aspx)]
7. <span data-ttu-id="1cec9-464">Otwórz **Styles\Site.css** pliku.</span><span class="sxs-lookup"><span data-stu-id="1cec9-464">Open the **Styles\Site.css** file.</span></span>

    <span data-ttu-id="1cec9-465">Zwróć uwagę, że oryginalnego pliku CSS zawiera kod wcięta, spacje i komentarze, które powiększyć plik.</span><span class="sxs-lookup"><span data-stu-id="1cec9-465">Notice that the original CSS file contains indented code, blank spaces and comments that enlarge the file.</span></span> <span data-ttu-id="1cec9-466">(Również JavaScript zawiera spacje i komentarze).</span><span class="sxs-lookup"><span data-stu-id="1cec9-466">(Also the JavaScript file contains blank spaces and comments).</span></span>

    <span data-ttu-id="1cec9-467">![Jeden z oryginalnego CSS pliki w folderze skryptów](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image51.png "jedną z oryginalnego CSS pliki w folderze skryptów")</span><span class="sxs-lookup"><span data-stu-id="1cec9-467">![One of the original CSS files in the Scripts folder](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image51.png "One of the original CSS files in the Scripts folder")</span></span>

    <span data-ttu-id="1cec9-468">*Jeden z oryginalnych plików CSS w folderze skryptów*</span><span class="sxs-lookup"><span data-stu-id="1cec9-468">*One of the original CSS files in the Scripts folder*</span></span>
8. <span data-ttu-id="1cec9-469">Naciśnij klawisz **F5** do uruchamiania aplikacji i przejdź do **optymalizacji** strony.</span><span class="sxs-lookup"><span data-stu-id="1cec9-469">Press **F5** to run the application and navigate to the **Optimization** page.</span></span>
9. <span data-ttu-id="1cec9-470">Polecenie **pakietu CSS** łącze aby pobrać i otworzyć plik.</span><span class="sxs-lookup"><span data-stu-id="1cec9-470">Click on the **CSS Bundle** link to download and open the file.</span></span>

    <span data-ttu-id="1cec9-471">Wyewidencjonuj plik powiązane zminimalizowany.</span><span class="sxs-lookup"><span data-stu-id="1cec9-471">Check out the minified bundled file.</span></span> <span data-ttu-id="1cec9-472">Można zauważyć, że wszystkie spacje, komentarze i wcięcia znaków zostały usunięte, generowanie mniejszy plik.</span><span class="sxs-lookup"><span data-stu-id="1cec9-472">You will notice that all the blank spaces, comments and indentation characters have been removed, generating a smaller file.</span></span>

    <span data-ttu-id="1cec9-473">![Powiązane pliki CSS](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image52.png "pliki CSS powiązane")</span><span class="sxs-lookup"><span data-stu-id="1cec9-473">![Bundled CSS files](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image52.png "Bundled CSS files")</span></span>

    <span data-ttu-id="1cec9-474">*Powiązane pliki CSS*</span><span class="sxs-lookup"><span data-stu-id="1cec9-474">*Bundled CSS files*</span></span>
10. <span data-ttu-id="1cec9-475">Teraz kliknij **pakietu JS** łącze, aby otworzyć plik JavaScript powiązane.</span><span class="sxs-lookup"><span data-stu-id="1cec9-475">Now click the **JS Bundle** link to open the JavaScript bundled file.</span></span> <span data-ttu-id="1cec9-476">Można bezpiecznie zignorować explorer ostrzeżenie.</span><span class="sxs-lookup"><span data-stu-id="1cec9-476">You can safely disregard the explorer warning.</span></span> <span data-ttu-id="1cec9-477">Zwróć uwagę, pliki JavaScript w **niestandardowych** folderu są również powiązane i zminimalizowany.</span><span class="sxs-lookup"><span data-stu-id="1cec9-477">Notice the JavaScript files under the **custom** folder are also bundled and minified.</span></span>

    <span data-ttu-id="1cec9-478">![Powiązane pliki JavaScript](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image53.png "pliki pakietu języka JavaScript")</span><span class="sxs-lookup"><span data-stu-id="1cec9-478">![Bundled JavaScript files](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image53.png "Bundled JavaScript files")</span></span>

    <span data-ttu-id="1cec9-479">*Powiązane pliki JavaScript*</span><span class="sxs-lookup"><span data-stu-id="1cec9-479">*Bundled JavaScript files*</span></span>

    <span data-ttu-id="1cec9-480">Włączanie kompresji plików CSS i Javascript w poprzedniej wersji programu ASP.NET została znacznie bardziej skomplikowane.</span><span class="sxs-lookup"><span data-stu-id="1cec9-480">Enabling compression for CSS or JS files was much more complicated in previous ASP.NET version.</span></span> <span data-ttu-id="1cec9-481">Teraz, jak już wspomniano, wystarczy dodać jeden wiersz w *Global.asax* plików, aby włączyć funkcję tworzenia pakietów i odwoływanie powiązane pliki z witryny.</span><span class="sxs-lookup"><span data-stu-id="1cec9-481">Now, as you have seen, you just need to add one line in the *Global.asax* file to enable bundling, and then reference the bundled files from your site.</span></span>

<a id="Ex4Task3"></a>

<a id="Task_3_-_Static_Bundles"></a>
#### <a name="task-3---static-bundles"></a><span data-ttu-id="1cec9-482">Zadanie 3 — statyczne pakietów</span><span class="sxs-lookup"><span data-stu-id="1cec9-482">Task 3 - Static Bundles</span></span>

<span data-ttu-id="1cec9-483">Metody statyczne pakietu umożliwia dostosowanie zestawu plików do pakietu, odwołanie i metody minimalizację, który będzie używany.</span><span class="sxs-lookup"><span data-stu-id="1cec9-483">The static bundle approach allows you to customize the set of files to bundle, the reference and the minification method that will be used.</span></span>

<span data-ttu-id="1cec9-484">W tym zadaniu skonfiguruj statyczny pakietu do definiowania określonego zestawu plików pakietu do zminimalizowania.</span><span class="sxs-lookup"><span data-stu-id="1cec9-484">In this task, you will configure a static bundle to define a specific set of files to bundle and minify.</span></span>

1. <span data-ttu-id="1cec9-485">Zamknij przeglądarkę.</span><span class="sxs-lookup"><span data-stu-id="1cec9-485">Close the browser.</span></span>
2. <span data-ttu-id="1cec9-486">Otwórz **Global.asax.cs** plików i Znajdź **aplikacji\_Start** metody.</span><span class="sxs-lookup"><span data-stu-id="1cec9-486">Open the **Global.asax.cs** file and locate the **Application\_Start** method.</span></span>
3. <span data-ttu-id="1cec9-487">Usuń komentarz kodu pakietu statycznych, jak pokazano w poniższym kodzie.</span><span class="sxs-lookup"><span data-stu-id="1cec9-487">Uncomment the static bundle code as shown in the code below.</span></span>

    <span data-ttu-id="1cec9-488">Definiowania statycznego pakiet, który zostanie dodane odwołanie z &quot; **~/StaticBundle** &quot; ścieżki wirtualnej i użyj **JsMinify** dla minimalizację wszystkie określone pliki z **AddFile** metody.</span><span class="sxs-lookup"><span data-stu-id="1cec9-488">You are defining a static bundle that will be referenced with the &quot;**~/StaticBundle**&quot; virtual path and use **JsMinify** for minification of all the specified files with the **AddFile** method.</span></span> <span data-ttu-id="1cec9-489">Ponadto w przypadku dodawania statycznych pakietu do **BundleTable** i włączenie go.</span><span class="sxs-lookup"><span data-stu-id="1cec9-489">Finally, you are adding the static bundle to the **BundleTable** and enabling it.</span></span>

    <span data-ttu-id="1cec9-490">Zwróć uwagę, że pliki nie znajdują się w tym samym miejscu; jest to możliwości innego niż domyślny, tworzenie pakietów.</span><span class="sxs-lookup"><span data-stu-id="1cec9-490">Notice that the files are not located in the same place; this is another advantage over the default bundling.</span></span>


    [!code-csharp[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample14.cs)]
4. <span data-ttu-id="1cec9-491">Otwórz **Optimization.aspx** pliku.</span><span class="sxs-lookup"><span data-stu-id="1cec9-491">Open the **Optimization.aspx** file.</span></span>

    <span data-ttu-id="1cec9-492">Zwróć uwagę, że łącze do **statycznych pakietu JS** jest przy użyciu ścieżki zadeklarowaniu podczas konfigurowania statycznego pakietu w pliku Global.asax.cs: **/StaticBundle**.</span><span class="sxs-lookup"><span data-stu-id="1cec9-492">Notice that the link to **Static JS Bundle** is using the path you have declared when you configured the static bundle in the Global.asax.cs file: **/StaticBundle**.</span></span>


    [!code-aspx[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample15.aspx)]
5. <span data-ttu-id="1cec9-493">Naciśnij klawisz **F5** do uruchomienia aplikacji, a następnie przejdź do **optymalizacji** strony.</span><span class="sxs-lookup"><span data-stu-id="1cec9-493">Press **F5** to run the application, and then navigate to the **Optimization** page.</span></span>
6. <span data-ttu-id="1cec9-494">Polecenie **statycznych pakietu JS** łącze, aby otworzyć plik.</span><span class="sxs-lookup"><span data-stu-id="1cec9-494">Click on the **Static JS Bundle** link to open the file.</span></span>

    <span data-ttu-id="1cec9-495">Powiadomienie zminimalizowany powiązane plik JavaScript jest wyjściem dla wszystkich plików JavaScript skonfigurowany w pliku statycznego pakietu w ścieżce &quot;/StaticBundle&quot;.</span><span class="sxs-lookup"><span data-stu-id="1cec9-495">Notice that the minified bundled JavaScript file is the output for all the JavaScript files configured in the static bundle file under the path &quot;/StaticBundle&quot;.</span></span>

    <span data-ttu-id="1cec9-496">![Statyczne pakietu plików JavaScript](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image54.png "pakietu plików statycznych JavaScript")</span><span class="sxs-lookup"><span data-stu-id="1cec9-496">![Static JavaScript files bundle](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image54.png "Static JavaScript files bundle")</span></span>

    <span data-ttu-id="1cec9-497">*Pliki statyczne JavaScript pakietu*</span><span class="sxs-lookup"><span data-stu-id="1cec9-497">*Static JavaScript files bundle*</span></span>
7. <span data-ttu-id="1cec9-498">Zamknij przeglądarkę i powrócić do programu Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="1cec9-498">Close the browser and return to Visual Studio.</span></span>

<a id="Ex4Task4"></a>

<a id="Task_4_-_Dynamic_Folder_Bundles"></a>
#### <a name="task-4---dynamic-folder-bundles"></a><span data-ttu-id="1cec9-499">Zadanie 4 — pakietów dynamicznych folderu</span><span class="sxs-lookup"><span data-stu-id="1cec9-499">Task 4 - Dynamic Folder Bundles</span></span>

<span data-ttu-id="1cec9-500">W tym zadaniu dowiesz się, sposobu konfigurowania pakietów dynamicznych folderu.</span><span class="sxs-lookup"><span data-stu-id="1cec9-500">In this task, you will learn how to configure dynamic folder bundles.</span></span> <span data-ttu-id="1cec9-501">Moc dynamiczne tworzenie pakietów jest obejmują statycznych JavaScript, a także inne pliki w językach, które kompiluje na język JavaScript, a w związku z tym wymagają niektórych przetwarzania przed wykonaniem tworzenie pakietów.</span><span class="sxs-lookup"><span data-stu-id="1cec9-501">The power of dynamic bundling is that you can include static JavaScript, as well as other files in languages that compiles into JavaScript, and thus, require some processing before the bundling is executed.</span></span>

<span data-ttu-id="1cec9-502">W w tym przykładzie przedstawiono sposób użycia **DynamicFolderBundle** klasa do tworzenia dynamicznych pakietu plików napisana CofeeScript.</span><span class="sxs-lookup"><span data-stu-id="1cec9-502">In this example, you will learn how to use the **DynamicFolderBundle** class to create a dynamic bundle for files written in CofeeScript.</span></span> <span data-ttu-id="1cec9-503">CofeeScript jest język programowania, który kompiluje się na język JavaScript i zapewnia prostszy składni pisanie kodu JavaScript, pominiemy i czytelność języka JavaScript.</span><span class="sxs-lookup"><span data-stu-id="1cec9-503">CofeeScript is a programming language that compiles into JavaScript and provides a simpler syntax for writing JavaScript code, enhancing JavaScript's brevity and readability.</span></span>

1. <span data-ttu-id="1cec9-504">Otwórz **Global.asax.cs** plików i Znajdź **aplikacji\_Start** metody.</span><span class="sxs-lookup"><span data-stu-id="1cec9-504">Open the **Global.asax.cs** file and locate the **Application\_Start** method.</span></span>
2. <span data-ttu-id="1cec9-505">Usuń komentarz kodu dynamiczne pakietu, jak pokazano w poniższym kodzie.</span><span class="sxs-lookup"><span data-stu-id="1cec9-505">Uncomment the dynamic bundle code as shown in the code below.</span></span>

    <span data-ttu-id="1cec9-506">Definiujesz pakietu dynamiczne folderu, który będzie używany przez **CoffeeMinify** procesora minimalizację niestandardowego, który będzie dotyczyć tylko pliki z &quot; **.coffee** &quot; (rozszerzenia Pliki języka CoffeeScript).</span><span class="sxs-lookup"><span data-stu-id="1cec9-506">You are defining a dynamic folder bundle that will use the **CoffeeMinify** custom minification processor that will only apply to the files with the &quot;**.coffee**&quot; extension (CoffeeScript files).</span></span> <span data-ttu-id="1cec9-507">Powiadomienia, której można wzorzec wyszukiwania, aby wybrać pliki pakietów w folderze, takie jak "\*.coffee".</span><span class="sxs-lookup"><span data-stu-id="1cec9-507">Notice that you can use a search pattern to select the files to bundle within a folder, like '\*.coffee'.</span></span>


    [!code-csharp[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample16.cs)]
3. <span data-ttu-id="1cec9-508">Otwórz konsolę Menedżera pakietów NuGet.</span><span class="sxs-lookup"><span data-stu-id="1cec9-508">Open the NuGet Package Manager Console.</span></span> <span data-ttu-id="1cec9-509">Aby to zrobić, użyj menu **widoku** | **inne okna** | **Konsola Menedżera pakietów**.</span><span class="sxs-lookup"><span data-stu-id="1cec9-509">To do this, use the menu **View** | **Other Windows** | **Package Manager Console**.</span></span>
4. <span data-ttu-id="1cec9-510">W **Konsola Menedżera pakietów,** typu **CoffeeSharp Install-Package** i naciśnij klawisz **ENTER**.</span><span class="sxs-lookup"><span data-stu-id="1cec9-510">In the **Package Manager Console,** type **Install-Package CoffeeSharp** and press **ENTER**.</span></span>
5. <span data-ttu-id="1cec9-511">Kliknij przycisk **Pokaż wszystkie pliki** przycisk **Eksploratora rozwiązań** okna</span><span class="sxs-lookup"><span data-stu-id="1cec9-511">Click the **Show All Files** button in the **Solution Explorer** window</span></span>

    <span data-ttu-id="1cec9-512">![Wyświetlanie wszystkich plików](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image55.png "wyświetlanie wszystkich plików")</span><span class="sxs-lookup"><span data-stu-id="1cec9-512">![Showing all files](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image55.png "Showing all files")</span></span>

    <span data-ttu-id="1cec9-513">*Wyświetlanie wszystkich plików*</span><span class="sxs-lookup"><span data-stu-id="1cec9-513">*Showing all files*</span></span>
6. <span data-ttu-id="1cec9-514">Kliknij prawym przyciskiem myszy **CoffeeMinify.cs** w pliku **Eksploratora rozwiązań** i wybierz **Include w projekcie**</span><span class="sxs-lookup"><span data-stu-id="1cec9-514">Right click the **CoffeeMinify.cs** file in the **Solution Explorer** and select **Include in Project**</span></span>

    <span data-ttu-id="1cec9-515">![W projekcie umieścić plik CoffeeMinify.cs](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image56.png "dołączyć plik CoffeeMinify.cs w projekcie")</span><span class="sxs-lookup"><span data-stu-id="1cec9-515">![Include the CoffeeMinify.cs file in the project](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image56.png "Include the CoffeeMinify.cs file in the project")</span></span>

    <span data-ttu-id="1cec9-516">*Dołączenie pliku CoffeeMinify.cs w projekcie*</span><span class="sxs-lookup"><span data-stu-id="1cec9-516">*Include the CoffeeMinify.cs file in the project*</span></span>
7. <span data-ttu-id="1cec9-517">Otwórz **CoffeeMinify.cs** pliku.</span><span class="sxs-lookup"><span data-stu-id="1cec9-517">Open the **CoffeeMinify.cs** file.</span></span>

    <span data-ttu-id="1cec9-518">Ta klasa dziedziczy JsMinify do zminimalizowania dane wyjściowe JavaScript wynikające z CoffeeScript kompilacji kodu.</span><span class="sxs-lookup"><span data-stu-id="1cec9-518">This class inherits from JsMinify to minify the JavaScript output resulting from the CoffeeScript code compilation.</span></span> <span data-ttu-id="1cec9-519">Wywołuje kompilatora języka CoffeeScript do generowania kodu JavaScript najpierw, a następnie wysyła go do metody JsMinify.Process w celu zminimalizowania wynikowy kod.</span><span class="sxs-lookup"><span data-stu-id="1cec9-519">It calls the CoffeeScript compiler to generate the JavaScript code first, and then it sends it to the JsMinify.Process method to minify the resulting code.</span></span>


    [!code-csharp[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample17.cs)]
8. <span data-ttu-id="1cec9-520">Otwórz **Script1.coffee** i **Script2.coffee** plików ze **skryptów/pakietu** folderu.</span><span class="sxs-lookup"><span data-stu-id="1cec9-520">Open the **Script1.coffee** and **Script2.coffee** files from the **Scripts/bundle** folder.</span></span>

    <span data-ttu-id="1cec9-521">Te pliki zostaną uwzględnione kodu CoffeScript ma być kompilowana podczas wykonywania, tworzenie pakietów przy użyciu klasy CoffeeMinify.</span><span class="sxs-lookup"><span data-stu-id="1cec9-521">These files will include the CoffeScript code to be compiled while performing the bundling with the CoffeeMinify class.</span></span>

    <span data-ttu-id="1cec9-522">Dla uproszczenia CoffeeScript pliki udostępniane są tylko tym CoffeeScript przykładowy kod.</span><span class="sxs-lookup"><span data-stu-id="1cec9-522">For simplicity purposes, the CoffeeScript files provided are only including CoffeeScript sample code.</span></span> <span data-ttu-id="1cec9-523">Komentarze są wyłączone przez proces JsMinify.</span><span class="sxs-lookup"><span data-stu-id="1cec9-523">The comments are excluded by the JsMinify process.</span></span>

    <span data-ttu-id="1cec9-524">![Pliki języka CoffeeScript](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image57.png "pliki języka CoffeeScript")</span><span class="sxs-lookup"><span data-stu-id="1cec9-524">![CoffeeScript files](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image57.png "CoffeeScript files")</span></span>

    <span data-ttu-id="1cec9-525">*Pliki języka CoffeeScript*</span><span class="sxs-lookup"><span data-stu-id="1cec9-525">*CoffeeScript files*</span></span>

    > [!NOTE]
    > <span data-ttu-id="1cec9-526">[CofeeScript](https://github.com/jashkenas/coffeescript/) zapewnia prostszy składni pisanie kodu JavaScript, skrócenia języka JavaScript i czytelność, a także dodawanie innych funkcji, takich jak tablicy zrozumienia i dopasowywanie do wzorca.</span><span class="sxs-lookup"><span data-stu-id="1cec9-526">[CofeeScript](https://github.com/jashkenas/coffeescript/) provides a simpler syntax for writing JavaScript code, enhancing JavaScript's brevity and readability, as well as adding other features like array comprehension and pattern matching.</span></span>
9. <span data-ttu-id="1cec9-527">Otwórz **Optimization.aspx** plików i Znajdź łącza pakietu.</span><span class="sxs-lookup"><span data-stu-id="1cec9-527">Open the **Optimization.aspx** file and locate the bundle links.</span></span>

    <span data-ttu-id="1cec9-528">Zwróć uwagę, że łącze do **dynamiczne pakietu JS** odwołuje się do **skryptów/pakietu** folder przy użyciu **/kawy** sufiks skonfigurowany dla pakietu folderu dynamicznych.</span><span class="sxs-lookup"><span data-stu-id="1cec9-528">Notice that the link to **Dynamic JS Bundle** is referencing the **Scripts/bundle** folder by using the **/Coffee** suffix you configured for the dynamic folder bundle.</span></span>


    [!code-aspx[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample18.aspx)]
10. <span data-ttu-id="1cec9-529">Naciśnij klawisz **F5** do uruchomienia aplikacji, a następnie przejdź do **optymalizacji** strony.</span><span class="sxs-lookup"><span data-stu-id="1cec9-529">Press **F5** to run the application, and then navigate to the **Optimization** page.</span></span>
11. <span data-ttu-id="1cec9-530">Polecenie **dynamiczne pakietu JS** łącze, aby otworzyć wygenerowanego pliku.</span><span class="sxs-lookup"><span data-stu-id="1cec9-530">Click on the **Dynamic JS Bundle** link to open the generated file.</span></span>

    <span data-ttu-id="1cec9-531">Należy zauważyć, że zawiera tylko zawartość, która została uwzględniona w tym pakiecie **.coffee** plików.</span><span class="sxs-lookup"><span data-stu-id="1cec9-531">Notice that the content that was included in this bundle only contains **.coffee** files.</span></span> <span data-ttu-id="1cec9-532">Można również sprawdzić, czy kod języka CoffeeScript został skompilowany dla JavaScript i linie poza komentarzem został usunięty.</span><span class="sxs-lookup"><span data-stu-id="1cec9-532">You can also see that the CoffeeScript code was compiled to JavaScript and the commented-out lines has been removed.</span></span>

    <span data-ttu-id="1cec9-533">![Dynamiczne pliki JS pakietu](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image58.png "JS dynamiczne pliki pakietu")</span><span class="sxs-lookup"><span data-stu-id="1cec9-533">![Dynamic JS files bundle](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image58.png "Dynamic JS files bundle")</span></span>

    <span data-ttu-id="1cec9-534">*Dynamiczne pakietu plików JS*</span><span class="sxs-lookup"><span data-stu-id="1cec9-534">*Dynamic JS files bundle*</span></span>

> [!NOTE]
> <span data-ttu-id="1cec9-535">Ponadto można wdrożyć tę aplikację systemu Windows Azure Web Sites następujących [dodatek B: publikowania aplikacji ASP.NET MVC 4 przy użyciu narzędzia Web Deploy](#AppendixB).</span><span class="sxs-lookup"><span data-stu-id="1cec9-535">Additionally, you can deploy this application to Windows Azure Web Sites following [Appendix B: Publishing an ASP.NET MVC 4 Application using Web Deploy](#AppendixB).</span></span>


<a id="Summary"></a>
## <a name="summary"></a><span data-ttu-id="1cec9-536">Podsumowanie</span><span class="sxs-lookup"><span data-stu-id="1cec9-536">Summary</span></span>

<span data-ttu-id="1cec9-537">W tym laboratorium pomaga zrozumieć, co jest nowego w programie ASP.NET i aplikacji sieci Web w programie Visual Studio 2012 oraz sposób korzystać z różnego rodzaju rozszerzeń w programie Visual Studio 2012.</span><span class="sxs-lookup"><span data-stu-id="1cec9-537">This lab helps you to understand what New in ASP.NET and Web Development in Visual Studio 2012 is and how to take advantage of the variety of enhancements in Visual Studio 2012.</span></span>

<span data-ttu-id="1cec9-538">Wykonując tego laboratorium Hands-On ma doświadczeń sposób użycia nowe funkcje i ulepszenia w Visual Studio 2012 edytory CSS, JavaScript i HTML.</span><span class="sxs-lookup"><span data-stu-id="1cec9-538">By completing this Hands-On Lab, you have learnt how to use the new features and improvements in Visual Studio 2012 Editors for CSS, JavaScript and HTML.</span></span> <span data-ttu-id="1cec9-539">Ponadto zostały wcześniej, jak Visual Studio 2012 implementuje wbudowanych tworzenie pakietów i minimalizowanie.</span><span class="sxs-lookup"><span data-stu-id="1cec9-539">In addition, you have learnt how Visual Studio 2012 implements built-in bundling and minification.</span></span>

<a id="AppendixA"></a>

<a id="Appendix_A_Installing_Visual_Studio_Express_2012_for_Web"></a>
## <a name="appendix-a-installing-visual-studio-express-2012-for-web"></a><span data-ttu-id="1cec9-540">Dodatek A: Instalowanie programu Visual Studio Express 2012 for Web</span><span class="sxs-lookup"><span data-stu-id="1cec9-540">Appendix A: Installing Visual Studio Express 2012 for Web</span></span>

<span data-ttu-id="1cec9-541">Można zainstalować **Microsoft Visual Studio Express 2012 for Web** lub innym &quot;Express&quot; przy użyciu wersji  **[Instalatora platformy sieci Web firmy Microsoft](https://www.microsoft.com/web/downloads/platform.aspx)** .</span><span class="sxs-lookup"><span data-stu-id="1cec9-541">You can install **Microsoft Visual Studio Express 2012 for Web** or another &quot;Express&quot; version using the **[Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)**.</span></span> <span data-ttu-id="1cec9-542">Poniższe instrukcje przedstawiono czynności wymagane do zainstalowania *programu Visual studio Express 2012 for Web* przy użyciu *Instalatora platformy sieci Web firmy Microsoft*.</span><span class="sxs-lookup"><span data-stu-id="1cec9-542">The following instructions guide you through the steps required to install *Visual studio Express 2012 for Web* using *Microsoft Web Platform Installer*.</span></span>

1. <span data-ttu-id="1cec9-543">Przejdź do [ [https://go.microsoft.com/?linkid=9810169](https://go.microsoft.com/?linkid=9810169)](https://go.microsoft.com/?linkid=9810169).</span><span class="sxs-lookup"><span data-stu-id="1cec9-543">Go to [[https://go.microsoft.com/?linkid=9810169](https://go.microsoft.com/?linkid=9810169)](https://go.microsoft.com/?linkid=9810169).</span></span> <span data-ttu-id="1cec9-544">Alternatywnie, jeśli została już zainstalowana Instalatora platformy sieci Web, można otworzyć go i Wyszukaj produkt &quot; *programu Visual Studio Express 2012 for Web z zestawem Windows Azure SDK*&quot;.</span><span class="sxs-lookup"><span data-stu-id="1cec9-544">Alternatively, if you already have installed Web Platform Installer, you can open it and search for the product &quot;*Visual Studio Express 2012 for Web with Windows Azure SDK*&quot;.</span></span>
2. <span data-ttu-id="1cec9-545">Polecenie **teraz zainstalować**.</span><span class="sxs-lookup"><span data-stu-id="1cec9-545">Click on **Install Now**.</span></span> <span data-ttu-id="1cec9-546">Jeśli nie masz **Instalatora platformy sieci Web** nastąpi przekierowanie do pobrania i zainstalowania go najpierw.</span><span class="sxs-lookup"><span data-stu-id="1cec9-546">If you do not have **Web Platform Installer** you will be redirected to download and install it first.</span></span>
3. <span data-ttu-id="1cec9-547">Raz **Instalatora platformy sieci Web** jest otwarty, kliknij przycisk **zainstalować** można uruchomić Instalatora.</span><span class="sxs-lookup"><span data-stu-id="1cec9-547">Once **Web Platform Installer** is open, click **Install** to start the setup.</span></span>

    <span data-ttu-id="1cec9-548">![Instalowanie programu Visual Studio Express](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image59.png "instalacji programu Visual Studio Express")</span><span class="sxs-lookup"><span data-stu-id="1cec9-548">![Install Visual Studio Express](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image59.png "Install Visual Studio Express")</span></span>

    <span data-ttu-id="1cec9-549">*Instalowanie programu Visual Studio Express*</span><span class="sxs-lookup"><span data-stu-id="1cec9-549">*Install Visual Studio Express*</span></span>
4. <span data-ttu-id="1cec9-550">Odczytywanie wszystkich produktów licencji i warunków, a następnie kliknij przycisk **akceptuję** aby kontynuować.</span><span class="sxs-lookup"><span data-stu-id="1cec9-550">Read all the products' licenses and terms and click **I Accept** to continue.</span></span>

    ![Akceptowanie umowy licencyjnej](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image60.png)

    <span data-ttu-id="1cec9-552">*Akceptowanie umowy licencyjnej*</span><span class="sxs-lookup"><span data-stu-id="1cec9-552">*Accepting the license terms*</span></span>
5. <span data-ttu-id="1cec9-553">Poczekaj na zakończenie procesu pobierania i instalacji.</span><span class="sxs-lookup"><span data-stu-id="1cec9-553">Wait until the downloading and installation process completes.</span></span>

    ![Postęp instalacji](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image61.png)

    <span data-ttu-id="1cec9-555">*Postęp instalacji*</span><span class="sxs-lookup"><span data-stu-id="1cec9-555">*Installation progress*</span></span>
6. <span data-ttu-id="1cec9-556">Po zakończeniu instalacji kliknij przycisk **Zakończ**.</span><span class="sxs-lookup"><span data-stu-id="1cec9-556">When the installation completes, click **Finish**.</span></span>

    ![Instalacja została zakończona](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image62.png)

    <span data-ttu-id="1cec9-558">*Instalacja została zakończona*</span><span class="sxs-lookup"><span data-stu-id="1cec9-558">*Installation completed*</span></span>
7. <span data-ttu-id="1cec9-559">Kliknij przycisk **zakończenia** aby zamknąć Instalatora platformy sieci Web.</span><span class="sxs-lookup"><span data-stu-id="1cec9-559">Click **Exit** to close Web Platform Installer.</span></span>
8. <span data-ttu-id="1cec9-560">Aby otworzyć program Visual Studio Express for Web, przejdź do **Start** ekranu i zacznij pisać &quot; **VS Express**&quot;, następnie kliknij polecenie **VS Express for Web** Kafelek.</span><span class="sxs-lookup"><span data-stu-id="1cec9-560">To open Visual Studio Express for Web, go to the **Start** screen and start writing &quot;**VS Express**&quot;, then click on the **VS Express for Web** tile.</span></span>

    ![VS Express for Web kafelka](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image63.png)

    <span data-ttu-id="1cec9-562">*VS Express for Web kafelka*</span><span class="sxs-lookup"><span data-stu-id="1cec9-562">*VS Express for Web tile*</span></span>

* * *

<a id="AppendixB"></a>

<a id="Appendix_B_Publishing_an_ASPNET_MVC_4_Application_using_Web_Deploy"></a>
## <a name="appendix-b-publishing-an-aspnet-mvc-4-application-using-web-deploy"></a><span data-ttu-id="1cec9-563">Dodatek B: publikowania aplikacji ASP.NET MVC 4 przy użyciu narzędzia Web Deploy</span><span class="sxs-lookup"><span data-stu-id="1cec9-563">Appendix B: Publishing an ASP.NET MVC 4 Application using Web Deploy</span></span>

<span data-ttu-id="1cec9-564">Ten dodatek opisano sposób tworzenia nowej witryny sieci web z portalu zarządzania pakietu Windows Azure i publikowanie aplikacji, uzyskane wykonując laboratorium, korzystając z funkcji publikowania narzędzia Web Deploy dostarczane przez Windows Azure.</span><span class="sxs-lookup"><span data-stu-id="1cec9-564">This appendix will show you how to create a new web site from the Windows Azure Management Portal and publish the application you obtained by following the lab, taking advantage of the Web Deploy publishing feature provided by Windows Azure.</span></span>

<a id="Ex1Task1"></a>

<a id="Task_1_-_Creating_a_New_Web_Site_from_the_Windows_Azure_Portal"></a>
#### <a name="task-1---creating-a-new-web-site-from-the-windows-azure-portal"></a><span data-ttu-id="1cec9-565">Zadanie 1 — Tworzenie nowej witryny sieci Web systemu Windows portalu Azure</span><span class="sxs-lookup"><span data-stu-id="1cec9-565">Task 1 - Creating a New Web Site from the Windows Azure Portal</span></span>

1. <span data-ttu-id="1cec9-566">Przejdź do [portalu zarządzania pakietu Windows Azure](https://manage.windowsazure.com/) i zaloguj się przy użyciu poświadczeń Microsoft skojarzonych z Twoją subskrypcją.</span><span class="sxs-lookup"><span data-stu-id="1cec9-566">Go to the [Windows Azure Management Portal](https://manage.windowsazure.com/) and sign in using the Microsoft credentials associated with your subscription.</span></span>

    > [!NOTE]
    > <span data-ttu-id="1cec9-567">Z systemu Windows Azure można udostępniać 10 witryn sieci Web platformy ASP.NET bezpłatnie i następnie Skaluj w miarę zwiększania się ruchu.</span><span class="sxs-lookup"><span data-stu-id="1cec9-567">With Windows Azure you can host 10 ASP.NET Web Sites for free and then scale as your traffic grows.</span></span> <span data-ttu-id="1cec9-568">Możesz utworzyć konto [tutaj](http://aka.ms/aspnet-hol-azure).</span><span class="sxs-lookup"><span data-stu-id="1cec9-568">You can sign up [here](http://aka.ms/aspnet-hol-azure).</span></span>

    <span data-ttu-id="1cec9-569">![Zaloguj się do portalu Windows Azure](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image64.png "Zaloguj się do portalu Windows Azure")</span><span class="sxs-lookup"><span data-stu-id="1cec9-569">![Log on to Windows Azure portal](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image64.png "Log on to Windows Azure portal")</span></span>

    <span data-ttu-id="1cec9-570">*Zaloguj się do portalu zarządzania platformy Azure z systemem Windows*</span><span class="sxs-lookup"><span data-stu-id="1cec9-570">*Log on to Windows Azure Management Portal*</span></span>
2. <span data-ttu-id="1cec9-571">Kliknij przycisk **nowy** na pasku poleceń.</span><span class="sxs-lookup"><span data-stu-id="1cec9-571">Click **New** on the command bar.</span></span>

    <span data-ttu-id="1cec9-572">![Tworzenie nowej witryny sieci Web](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image65.png "tworzenia nowej witryny sieci Web")</span><span class="sxs-lookup"><span data-stu-id="1cec9-572">![Creating a new Web Site](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image65.png "Creating a new Web Site")</span></span>

    <span data-ttu-id="1cec9-573">*Tworzenie nowej witryny sieci Web*</span><span class="sxs-lookup"><span data-stu-id="1cec9-573">*Creating a new Web Site*</span></span>
3. <span data-ttu-id="1cec9-574">Kliknij przycisk **obliczeniowe** | **witryny sieci Web**.</span><span class="sxs-lookup"><span data-stu-id="1cec9-574">Click **Compute** | **Web Site**.</span></span> <span data-ttu-id="1cec9-575">Następnie wybierz **szybkie tworzenie** opcji.</span><span class="sxs-lookup"><span data-stu-id="1cec9-575">Then select **Quick Create** option.</span></span> <span data-ttu-id="1cec9-576">Podaj dostępny adres URL dla nowej witryny sieci web, a następnie kliknij przycisk **tworzenie witryny sieci Web**.</span><span class="sxs-lookup"><span data-stu-id="1cec9-576">Provide an available URL for the new web site and click **Create Web Site**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="1cec9-577">Witryny sieci Web systemu Windows Azure jest hostem dla aplikacji sieci web w chmurze, które można kontrolować i zarządzanie nimi.</span><span class="sxs-lookup"><span data-stu-id="1cec9-577">A Windows Azure Web Site is the host for a web application running in the cloud that you can control and manage.</span></span> <span data-ttu-id="1cec9-578">Opcja szybkie tworzenie umożliwia wdrażanie ukończonej aplikacji sieci web do systemu Windows Azure witryny internetowej z spoza portalu.</span><span class="sxs-lookup"><span data-stu-id="1cec9-578">The Quick Create option allows you to deploy a completed web application to the Windows Azure Web Site from outside the portal.</span></span> <span data-ttu-id="1cec9-579">Nie obejmuje kroki konfigurowania bazy danych.</span><span class="sxs-lookup"><span data-stu-id="1cec9-579">It does not include steps for setting up a database.</span></span>

    <span data-ttu-id="1cec9-580">![Tworzenie nowej witryny sieci Web przy użyciu szybkie tworzenie](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image66.png "tworzenia nowej witryny sieci Web przy użyciu szybkie tworzenie")</span><span class="sxs-lookup"><span data-stu-id="1cec9-580">![Creating a new Web Site using Quick Create](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image66.png "Creating a new Web Site using Quick Create")</span></span>

    <span data-ttu-id="1cec9-581">*Tworzenie nowej witryny sieci Web przy użyciu szybkie tworzenie*</span><span class="sxs-lookup"><span data-stu-id="1cec9-581">*Creating a new Web Site using Quick Create*</span></span>
4. <span data-ttu-id="1cec9-582">Poczekaj na nowe **witryny sieci Web** jest tworzony.</span><span class="sxs-lookup"><span data-stu-id="1cec9-582">Wait until the new **Web Site** is created.</span></span>
5. <span data-ttu-id="1cec9-583">Po utworzeniu witryny sieci Web kliknij łącze w obszarze **adres URL** kolumny.</span><span class="sxs-lookup"><span data-stu-id="1cec9-583">Once the Web Site is created click the link under the **URL** column.</span></span> <span data-ttu-id="1cec9-584">Sprawdź, czy działa nowej witryny sieci Web.</span><span class="sxs-lookup"><span data-stu-id="1cec9-584">Check that the new Web Site is working.</span></span>

    <span data-ttu-id="1cec9-585">![Przeglądanie do nowej witryny sieci web](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image67.png "przeglądanie do nowej witryny sieci web")</span><span class="sxs-lookup"><span data-stu-id="1cec9-585">![Browsing to the new web site](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image67.png "Browsing to the new web site")</span></span>

    <span data-ttu-id="1cec9-586">*Przeglądanie do nowej witryny sieci web*</span><span class="sxs-lookup"><span data-stu-id="1cec9-586">*Browsing to the new web site*</span></span>

    <span data-ttu-id="1cec9-587">![Witryna sieci Web działa](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image68.png "uruchamiania witryny sieci Web")</span><span class="sxs-lookup"><span data-stu-id="1cec9-587">![Web site running](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image68.png "Web site running")</span></span>

    <span data-ttu-id="1cec9-588">*Witryna sieci Web uruchomiona*</span><span class="sxs-lookup"><span data-stu-id="1cec9-588">*Web site running*</span></span>
6. <span data-ttu-id="1cec9-589">Wróć do portalu i kliknij nazwę witryny sieci web w obszarze **nazwa** kolumny do wyświetlenia strony zarządzania.</span><span class="sxs-lookup"><span data-stu-id="1cec9-589">Go back to the portal and click the name of the web site under the **Name** column to display the management pages.</span></span>

    <span data-ttu-id="1cec9-590">![Otwieranie stron witryny sieci web zarządzania](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image69.png "otwieranie stron zarządzania witryny sieci web")</span><span class="sxs-lookup"><span data-stu-id="1cec9-590">![Opening the web site management pages](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image69.png "Opening the web site management pages")</span></span>

    <span data-ttu-id="1cec9-591">*Otwieranie stron zarządzania witryny sieci Web*</span><span class="sxs-lookup"><span data-stu-id="1cec9-591">*Opening the Web Site management pages*</span></span>
7. <span data-ttu-id="1cec9-592">W **pulpitu nawigacyjnego** w obszarze **szybkiego dostępu** kliknij **pobieranie profilu publikowania** łącza.</span><span class="sxs-lookup"><span data-stu-id="1cec9-592">In the **Dashboard** page, under the **quick glance** section, click the **Download publish profile** link.</span></span>

    > [!NOTE]
    > <span data-ttu-id="1cec9-593">*Profilu publikowania* zawiera wszystkie informacje wymagane do publikowania aplikacji sieci web do witryny sieci Web systemu Windows Azure dla każdej metody włączone publikacji.</span><span class="sxs-lookup"><span data-stu-id="1cec9-593">The *publish profile* contains all of the information required to publish a web application to a Windows Azure website for each enabled publication method.</span></span> <span data-ttu-id="1cec9-594">Profil publikowania zawiera adresy URL, poświadczenia użytkownika i parametry bazy danych wymagane do nawiązania połączenia i uwierzytelniania dla każdego z punktów końcowych, dla których włączono metoda publikacji.</span><span class="sxs-lookup"><span data-stu-id="1cec9-594">The publish profile contains the URLs, user credentials and database strings required to connect to and authenticate against each of the endpoints for which a publication method is enabled.</span></span> <span data-ttu-id="1cec9-595">**Microsoft WebMatrix 2**, **Microsoft Visual Studio Express for Web** i **programu Microsoft Visual Studio 2012** obsługują odczytywanie publikowanie profile do zautomatyzowania te programy Publikowanie aplikacji sieci web do witryn sieci Web systemu Windows Azure.</span><span class="sxs-lookup"><span data-stu-id="1cec9-595">**Microsoft WebMatrix 2**, **Microsoft Visual Studio Express for Web** and **Microsoft Visual Studio 2012** support reading publish profiles to automate configuration of these programs for publishing web applications to Windows Azure websites.</span></span>

    <span data-ttu-id="1cec9-596">![Pobieranie witryny sieci web profilu publikowania](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image70.png "pobierania witryny sieci web profilu publikowania")</span><span class="sxs-lookup"><span data-stu-id="1cec9-596">![Downloading the web site publish profile](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image70.png "Downloading the web site publish profile")</span></span>

    <span data-ttu-id="1cec9-597">*Pobieranie witryny sieci Web profilu publikowania*</span><span class="sxs-lookup"><span data-stu-id="1cec9-597">*Downloading the Web Site publish profile*</span></span>
8. <span data-ttu-id="1cec9-598">Pobierz profil publikowania w znanej lokalizacji.</span><span class="sxs-lookup"><span data-stu-id="1cec9-598">Download the publish profile file to a known location.</span></span> <span data-ttu-id="1cec9-599">Dodatkowo w tym ćwiczeniu zobaczysz jak opublikować aplikację sieci web do witryny sieci Web systemu Windows Azure w programie Visual Studio przy użyciu tego pliku.</span><span class="sxs-lookup"><span data-stu-id="1cec9-599">Further in this exercise you will see how to use this file to publish a web application to a Windows Azure Web Sites from Visual Studio.</span></span>

    <span data-ttu-id="1cec9-600">![Zapisywanie pliku profilu publikowania](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image71.png "zapisywanie profilu publikowania")</span><span class="sxs-lookup"><span data-stu-id="1cec9-600">![Saving the publish profile file](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image71.png "Saving the publish profile")</span></span>

    <span data-ttu-id="1cec9-601">*Zapisywanie pliku profilu publikowania*</span><span class="sxs-lookup"><span data-stu-id="1cec9-601">*Saving the publish profile file*</span></span>

<a id="Ex1Task2"></a>

<a id="Task_2_-_Configuring_the_Database_Server"></a>
#### <a name="task-2---configuring-the-database-server"></a><span data-ttu-id="1cec9-602">Zadanie 2 — Konfigurowanie serwera bazy danych</span><span class="sxs-lookup"><span data-stu-id="1cec9-602">Task 2 - Configuring the Database Server</span></span>

<span data-ttu-id="1cec9-603">Jeśli aplikacja korzysta z programu SQL Server baz danych, należy utworzyć serwer bazy danych SQL.</span><span class="sxs-lookup"><span data-stu-id="1cec9-603">If your application makes use of SQL Server databases you will need to create a SQL Database server.</span></span> <span data-ttu-id="1cec9-604">Jeśli chcesz wdrożyć prostą aplikację, która nie korzysta z programu SQL Server może pominąć to zadanie.</span><span class="sxs-lookup"><span data-stu-id="1cec9-604">If you want to deploy a simple application that does not use SQL Server you might skip this task.</span></span>

1. <span data-ttu-id="1cec9-605">Będzie potrzebny serwer bazy danych SQL do przechowywania bazy danych aplikacji.</span><span class="sxs-lookup"><span data-stu-id="1cec9-605">You will need a SQL Database server for storing the application database.</span></span> <span data-ttu-id="1cec9-606">Można wyświetlić serwery bazy danych SQL z subskrypcji usługi Windows Azure Management Portal pod adresem **baz danych Sql** | **serwerów** | **serwera Pulpit nawigacyjny**.</span><span class="sxs-lookup"><span data-stu-id="1cec9-606">You can view the SQL Database servers from your subscription in the Windows Azure Management portal at **Sql Databases** | **Servers** | **Server's Dashboard**.</span></span> <span data-ttu-id="1cec9-607">Jeśli nie masz serwer, który został utworzony, można utworzyć przy użyciu jednego **Dodaj** przycisk paska poleceń.</span><span class="sxs-lookup"><span data-stu-id="1cec9-607">If you do not have a server created, you can create one using the **Add** button on the command bar.</span></span> <span data-ttu-id="1cec9-608">Zwróć uwagę na **nazwę serwera i adres URL, nazwę logowania administratora i hasła**, jak będą używane w następnego zadania.</span><span class="sxs-lookup"><span data-stu-id="1cec9-608">Take note of the **server name and URL, administrator login name and password**, as you will use them in the next tasks.</span></span> <span data-ttu-id="1cec9-609">Nie należy tworzyć bazy danych jeszcze, jako zostaną utworzone w późniejszym terminie.</span><span class="sxs-lookup"><span data-stu-id="1cec9-609">Do not create the database yet, as it will be created in a later stage.</span></span>

    <span data-ttu-id="1cec9-610">![Pulpit nawigacyjny serwera bazy danych SQL](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image72.png "pulpitu nawigacyjnego serwera bazy danych SQL")</span><span class="sxs-lookup"><span data-stu-id="1cec9-610">![SQL Database Server Dashboard](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image72.png "SQL Database Server Dashboard")</span></span>

    <span data-ttu-id="1cec9-611">*Pulpit nawigacyjny serwera bazy danych SQL*</span><span class="sxs-lookup"><span data-stu-id="1cec9-611">*SQL Database Server Dashboard*</span></span>
2. <span data-ttu-id="1cec9-612">W następnym zadaniem Testuj połączenie z bazą danych z programu Visual Studio z tego powodu należy uwzględnić lokalny adres IP serwera liście **dozwolone adresy IP**.</span><span class="sxs-lookup"><span data-stu-id="1cec9-612">In the next task you will test the database connection from Visual Studio, for that reason you need to include your local IP address in the server's list of **Allowed IP Addresses**.</span></span> <span data-ttu-id="1cec9-613">Aby to zrobić, kliknij przycisk **Konfiguruj**, wybierz adres IP z **bieżącego adresu IP klienta** i wklej go na **początkowy adres IP** i **końcowy adres IP** pól tekstowych.</span><span class="sxs-lookup"><span data-stu-id="1cec9-613">To do that, click **Configure**, select the IP address from **Current Client IP Address** and paste it on the **Start IP Address** and **End IP Address** text boxes.</span></span> <span data-ttu-id="1cec9-614">Wprowadź nazwę reguły, a następnie kliknij przycisk ![add-client-ip-address-ok-button](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image73.png) przycisku.</span><span class="sxs-lookup"><span data-stu-id="1cec9-614">Enter a name for the rule and click the ![add-client-ip-address-ok-button](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image73.png) button.</span></span>

    ![Dodawanie adresu IP klienta](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image74.png)

    <span data-ttu-id="1cec9-616">*Dodawanie adresu IP klienta*</span><span class="sxs-lookup"><span data-stu-id="1cec9-616">*Adding Client IP Address*</span></span>
3. <span data-ttu-id="1cec9-617">Raz **adres IP klienta** jest dodawany do dozwolonych adresów IP kliknij na **zapisać** o potwierdzenie zmian.</span><span class="sxs-lookup"><span data-stu-id="1cec9-617">Once the **Client IP Address** is added to the allowed IP addresses list, click on **Save** to confirm the changes.</span></span>

    ![Potwierdzenie zmian](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image75.png)

    <span data-ttu-id="1cec9-619">*Potwierdzenie zmian*</span><span class="sxs-lookup"><span data-stu-id="1cec9-619">*Confirm Changes*</span></span>

<a id="Ex1Task3"></a>

<a id="Task_3_-_Publishing_an_ASPNET_MVC_4_Application_using_Web_Deploy"></a>
#### <a name="task-3---publishing-an-aspnet-mvc-4-application-using-web-deploy"></a><span data-ttu-id="1cec9-620">Zadanie 3 - publikowania aplikacji ASP.NET MVC 4 przy użyciu narzędzia Web Deploy</span><span class="sxs-lookup"><span data-stu-id="1cec9-620">Task 3 - Publishing an ASP.NET MVC 4 Application using Web Deploy</span></span>

1. <span data-ttu-id="1cec9-621">Wróć do rozwiązania ASP.NET MVC 4.</span><span class="sxs-lookup"><span data-stu-id="1cec9-621">Go back to the ASP.NET MVC 4 solution.</span></span> <span data-ttu-id="1cec9-622">W **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy projekt witryny sieci web i wybierz **publikowania**.</span><span class="sxs-lookup"><span data-stu-id="1cec9-622">In the **Solution Explorer**, right-click the web site project and select **Publish**.</span></span>

    <span data-ttu-id="1cec9-623">![Publikowanie aplikacji](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image76.png "publikowania aplikacji")</span><span class="sxs-lookup"><span data-stu-id="1cec9-623">![Publishing the Application](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image76.png "Publishing the Application")</span></span>

    <span data-ttu-id="1cec9-624">*Publikowanie witryny sieci web*</span><span class="sxs-lookup"><span data-stu-id="1cec9-624">*Publishing the web site*</span></span>
2. <span data-ttu-id="1cec9-625">Zaimportuj profil publikowania, zapisana w pierwszym zadaniu.</span><span class="sxs-lookup"><span data-stu-id="1cec9-625">Import the publish profile you saved in the first task.</span></span>

    <span data-ttu-id="1cec9-626">![Importowanie profilu publikowania](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image77.png "Importowanie profilu publikowania")</span><span class="sxs-lookup"><span data-stu-id="1cec9-626">![Importing the publish profile](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image77.png "Importing the publish profile")</span></span>

    <span data-ttu-id="1cec9-627">*Importowanie profilu publikowania*</span><span class="sxs-lookup"><span data-stu-id="1cec9-627">*Importing publish profile*</span></span>
3. <span data-ttu-id="1cec9-628">Kliknij przycisk **Weryfikacja połączenia z**.</span><span class="sxs-lookup"><span data-stu-id="1cec9-628">Click **Validate Connection**.</span></span> <span data-ttu-id="1cec9-629">Po zakończeniu sprawdzania kliknij **dalej**.</span><span class="sxs-lookup"><span data-stu-id="1cec9-629">Once Validation is complete click **Next**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="1cec9-630">Zakończeniu sprawdzania poprawności, gdy zostanie wyświetlony zielony znacznik wyboru są wyświetlane obok przycisku sprawdzania poprawności połączenia.</span><span class="sxs-lookup"><span data-stu-id="1cec9-630">Validation is complete once you see a green checkmark appear next to the Validate Connection button.</span></span>

    <span data-ttu-id="1cec9-631">![Sprawdzanie poprawności połączenia](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image78.png "sprawdzanie poprawności połączenia")</span><span class="sxs-lookup"><span data-stu-id="1cec9-631">![Validating connection](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image78.png "Validating connection")</span></span>

    <span data-ttu-id="1cec9-632">*Sprawdzanie poprawności połączenia*</span><span class="sxs-lookup"><span data-stu-id="1cec9-632">*Validating connection*</span></span>
4. <span data-ttu-id="1cec9-633">W **ustawienia** w obszarze **baz danych** sekcji, kliknij przycisk Dalej, aby textbox połączenia bazy danych (tj. **połączenia DefaultConnection**).</span><span class="sxs-lookup"><span data-stu-id="1cec9-633">In the **Settings** page, under the **Databases** section, click the button next to your database connection's textbox (i.e. **DefaultConnection**).</span></span>

    <span data-ttu-id="1cec9-634">![Konfiguracja narzędzia Web deploy](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image79.png "Konfiguracja narzędzia Web deploy")</span><span class="sxs-lookup"><span data-stu-id="1cec9-634">![Web deploy configuration](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image79.png "Web deploy configuration")</span></span>

    <span data-ttu-id="1cec9-635">*Konfiguracja narzędzia Web deploy*</span><span class="sxs-lookup"><span data-stu-id="1cec9-635">*Web deploy configuration*</span></span>
5. <span data-ttu-id="1cec9-636">Skonfiguruj połączenie z bazą danych w następujący sposób:</span><span class="sxs-lookup"><span data-stu-id="1cec9-636">Configure the database connection as follows:</span></span>

    - <span data-ttu-id="1cec9-637">W **nazwy serwera** wpisz swoją bazą danych SQL server adresu URL przy użyciu *tcp:* prefiks.</span><span class="sxs-lookup"><span data-stu-id="1cec9-637">In the **Server name** type your SQL Database server URL using the *tcp:* prefix.</span></span>
    - <span data-ttu-id="1cec9-638">W **nazwy użytkownika** wpisz nazwę logowania administratora serwera.</span><span class="sxs-lookup"><span data-stu-id="1cec9-638">In **User name** type your server administrator login name.</span></span>
    - <span data-ttu-id="1cec9-639">W **hasło** wpisz hasło logowania administratora serwera.</span><span class="sxs-lookup"><span data-stu-id="1cec9-639">In **Password** type your server administrator login password.</span></span>
    - <span data-ttu-id="1cec9-640">Wpisz nazwę nowej bazy danych, na przykład: *MVC4SampleDB*.</span><span class="sxs-lookup"><span data-stu-id="1cec9-640">Type a new database name, for example: *MVC4SampleDB*.</span></span>

    <span data-ttu-id="1cec9-641">![Konfigurowanie parametrów połączenia z lokalizacją docelową](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image80.png "Konfigurowanie parametrów połączenia z lokalizacją docelową")</span><span class="sxs-lookup"><span data-stu-id="1cec9-641">![Configuring destination connection string](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image80.png "Configuring destination connection string")</span></span>

    <span data-ttu-id="1cec9-642">*Konfigurowanie parametrów połączenia z lokalizacją docelową*</span><span class="sxs-lookup"><span data-stu-id="1cec9-642">*Configuring destination connection string*</span></span>
6. <span data-ttu-id="1cec9-643">Następnie kliknij przycisk **OK**.</span><span class="sxs-lookup"><span data-stu-id="1cec9-643">Then click **OK**.</span></span> <span data-ttu-id="1cec9-644">Po wyświetleniu monitu można utworzyć bazy danych kliknij **tak**.</span><span class="sxs-lookup"><span data-stu-id="1cec9-644">When prompted to create the database click **Yes**.</span></span>

    <span data-ttu-id="1cec9-645">![Tworzenie bazy danych](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image81.png "tworzenie parametry bazy danych")</span><span class="sxs-lookup"><span data-stu-id="1cec9-645">![Creating the database](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image81.png "Creating the database string")</span></span>

    <span data-ttu-id="1cec9-646">*Tworzenie bazy danych*</span><span class="sxs-lookup"><span data-stu-id="1cec9-646">*Creating the database*</span></span>
7. <span data-ttu-id="1cec9-647">Ciągu połączenia używanego do łączenia z bazą danych SQL w systemie Windows Azure jest wyświetlany w pole tekstowe domyślne połączenie.</span><span class="sxs-lookup"><span data-stu-id="1cec9-647">The connection string you will use to connect to SQL Database in Windows Azure is shown within Default Connection textbox.</span></span> <span data-ttu-id="1cec9-648">Następnie kliknij przycisk **Dalej**.</span><span class="sxs-lookup"><span data-stu-id="1cec9-648">Then click **Next**.</span></span>

    <span data-ttu-id="1cec9-649">![Parametry połączenia wskazujące bazę danych SQL](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image82.png "ciąg połączenia wskazujące bazę danych SQL")</span><span class="sxs-lookup"><span data-stu-id="1cec9-649">![Connection string pointing to SQL Database](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image82.png "Connection string pointing to SQL Database")</span></span>

    <span data-ttu-id="1cec9-650">*Parametry połączenia wskazujące bazę danych SQL*</span><span class="sxs-lookup"><span data-stu-id="1cec9-650">*Connection string pointing to SQL Database*</span></span>
8. <span data-ttu-id="1cec9-651">W **Podgląd** kliknij przycisk **publikowania**.</span><span class="sxs-lookup"><span data-stu-id="1cec9-651">In the **Preview** page, click **Publish**.</span></span>

    <span data-ttu-id="1cec9-652">![Publikowanie aplikacji sieci web](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image83.png "publikowania aplikacji sieci web")</span><span class="sxs-lookup"><span data-stu-id="1cec9-652">![Publishing the web application](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image83.png "Publishing the web application")</span></span>

    <span data-ttu-id="1cec9-653">*Publikowanie aplikacji sieci web*</span><span class="sxs-lookup"><span data-stu-id="1cec9-653">*Publishing the web application*</span></span>
9. <span data-ttu-id="1cec9-654">Po zakończeniu procesu publikowania domyślnej przeglądarce otworzy opublikowanej witryny sieci web.</span><span class="sxs-lookup"><span data-stu-id="1cec9-654">Once the publishing process finishes, your default browser will open the published web site.</span></span>

    <span data-ttu-id="1cec9-655">![Aplikacja opublikowana w systemie Windows Azure](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image84.png "aplikacji publikowanych w systemie Windows Azure")</span><span class="sxs-lookup"><span data-stu-id="1cec9-655">![Application published to Windows Azure](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image84.png "Application published to Windows Azure")</span></span>

    <span data-ttu-id="1cec9-656">*Aplikacji publikowanych w systemie Windows Azure*</span><span class="sxs-lookup"><span data-stu-id="1cec9-656">*Application published to Windows Azure*</span></span>
