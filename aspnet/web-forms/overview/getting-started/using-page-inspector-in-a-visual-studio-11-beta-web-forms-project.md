---
uid: web-forms/overview/getting-started/using-page-inspector-in-a-visual-studio-11-beta-web-forms-project
title: "Za pomocą narzędzia Page Inspector dla formularzy programu Visual Studio 2012 w sieci Web programu ASP.NET | Dokumentacja firmy Microsoft"
author: rick-anderson
description: "Narzędzie Page Inspector dla programu Visual Studio 2012 to narzędzie do projektowania sieci web za pomocą zintegrowanego przeglądarki. Wybierz dowolny element w zintegrowanej przeglądarki i narzędzie Page Inspector..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/15/2012
ms.topic: article
ms.assetid: 2ece0bf4-aae5-4ff4-8f62-28e0819d4f86
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/getting-started/using-page-inspector-in-a-visual-studio-11-beta-web-forms-project
msc.type: authoredcontent
ms.openlocfilehash: a2ac8334e62e6ab7af7042572cfd5950c687001b
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/10/2017
---
<a name="using-page-inspector-for-visual-studio-2012-in-aspnet-web-forms"></a><span data-ttu-id="7bb41-104">Za pomocą narzędzia Page Inspector dla programu Visual Studio 2012 w formularzach sieci Web ASP.NET</span><span class="sxs-lookup"><span data-stu-id="7bb41-104">Using Page Inspector for Visual Studio 2012 in ASP.NET Web Forms</span></span>
====================
<span data-ttu-id="7bb41-105">przez Ammann Timowi</span><span class="sxs-lookup"><span data-stu-id="7bb41-105">by Tim Ammann</span></span>

> <span data-ttu-id="7bb41-106">Narzędzie Page Inspector dla programu Visual Studio 2012 to narzędzie do projektowania sieci web za pomocą zintegrowanego przeglądarki.</span><span class="sxs-lookup"><span data-stu-id="7bb41-106">Page Inspector for Visual Studio 2012 is a web development tool with an integrated browser.</span></span> <span data-ttu-id="7bb41-107">Wybierz dowolny element w zintegrowanej przeglądarki i narzędzie Page Inspector natychmiast wyróżnia elementu źródłowego i CSS.</span><span class="sxs-lookup"><span data-stu-id="7bb41-107">Select any element in the integrated browser, and Page Inspector instantly highlights the element's source and CSS.</span></span> <span data-ttu-id="7bb41-108">Można wybrać dowolną stronę w aplikacji, szybkie znajdowanie źródeł renderowanego kodu znaczników i użyj narzędzia przeglądarki w środowisku Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="7bb41-108">You can browse any page in your application, quickly find the sources of rendered markup, and use browser tools right within the Visual Studio environment.</span></span>
> 
> <span data-ttu-id="7bb41-109">Ten samouczek shwos jak włączyć tryb inspekcji, a następnie szybko zlokalizować i edytować reguły CSS i tekst w ramach projektu sieci web.</span><span class="sxs-lookup"><span data-stu-id="7bb41-109">This tutorial shwos how to enable Inspection Mode and then quickly locate and edit CSS rules and text within your web project.</span></span> <span data-ttu-id="7bb41-110">W samouczku projekt aplikacji formularzy sieci Web, ale można również użyć narzędzie Page Inspector dla projektów witryny sieci Web i [MVC](https://go.microsoft.com/?linkid=9802002) aplikacji.</span><span class="sxs-lookup"><span data-stu-id="7bb41-110">The tutorial uses a Web Forms Application Project, but you can also use Page Inspector for Web Site projects and [MVC](https://go.microsoft.com/?linkid=9802002) applications.</span></span>
> 
> <span data-ttu-id="7bb41-111">Samouczek zawiera następujące sekcje:</span><span class="sxs-lookup"><span data-stu-id="7bb41-111">The tutorial has the following sections:</span></span>
> 
> [<span data-ttu-id="7bb41-112">Wymagania wstępne</span><span class="sxs-lookup"><span data-stu-id="7bb41-112">Prerequisites</span></span>](#_1_prerequisites)
> 
> [<span data-ttu-id="7bb41-113">Tworzenie aplikacji sieci Web</span><span class="sxs-lookup"><span data-stu-id="7bb41-113">Create a Web Application</span></span>](#_2_creating_a)
> 
> [<span data-ttu-id="7bb41-114">Narzędzie Page Inspector umożliwia wyświetlanie aplikacji</span><span class="sxs-lookup"><span data-stu-id="7bb41-114">Use Page Inspector to View the Application</span></span>](#_3_using_page)
> 
> [<span data-ttu-id="7bb41-115">Włącz tryb inspekcji</span><span class="sxs-lookup"><span data-stu-id="7bb41-115">Enable Inspection Mode</span></span>](#_4_inspection_mode)
> 
> [<span data-ttu-id="7bb41-116">Narzędzie Page Inspector umożliwia zmianę znaczników</span><span class="sxs-lookup"><span data-stu-id="7bb41-116">Use Page Inspector to Make Changes to Markup</span></span>](#_5_using_page)
> 
> [<span data-ttu-id="7bb41-117">Tryb inspekcji i okno HTML</span><span class="sxs-lookup"><span data-stu-id="7bb41-117">Inspection Mode and the HTML Window</span></span>](#_6_inspection_mode)
> 
> [<span data-ttu-id="7bb41-118">Podgląd zmian CSS w oknie style</span><span class="sxs-lookup"><span data-stu-id="7bb41-118">Preview CSS Changes in the Styles Window</span></span>](#_7_previewing_css)
> 
> [<span data-ttu-id="7bb41-119">Automatyczna synchronizacja z CSS</span><span class="sxs-lookup"><span data-stu-id="7bb41-119">CSS Auto Sync</span></span>](#css_auto_sync)
> 
> [<span data-ttu-id="7bb41-120">Za pomocą selektora kolorów CSS</span><span class="sxs-lookup"><span data-stu-id="7bb41-120">Using the CSS Color Picker</span></span>](#css_color_picker)


<a id="_prerequisites"></a><a id="_1_prerequisites"></a>

## <a name="prerequisites"></a><span data-ttu-id="7bb41-121">Wymagania wstępne</span><span class="sxs-lookup"><span data-stu-id="7bb41-121">Prerequisites</span></span>

- <span data-ttu-id="7bb41-122">[Program Visual Studio 2012](https://www.microsoft.com/visualstudio/11/en-us) lub [programu Visual Studio Express 2012 for Web](https://www.microsoft.com/visualstudio/11/en-us/downloads#express-web).</span><span class="sxs-lookup"><span data-stu-id="7bb41-122">[Visual Studio 2012](https://www.microsoft.com/visualstudio/11/en-us) or [Visual Studio Express 2012 for Web](https://www.microsoft.com/visualstudio/11/en-us/downloads#express-web).</span></span>

> [!NOTE]
> <span data-ttu-id="7bb41-123">Aby uzyskać najnowszą wersję narzędzia Page Inspector, należy użyć [Instalatora platformy sieci Web](https://go.microsoft.com/fwlink/?LinkId=255386) zainstalować zestaw Azure SDK dla .NET 2.0.</span><span class="sxs-lookup"><span data-stu-id="7bb41-123">To get the latest version of Page Inspector, use [Web Platform Installer](https://go.microsoft.com/fwlink/?LinkId=255386) to install the Azure SDK for .NET 2.0.</span></span>


<span data-ttu-id="7bb41-124">Narzędzie Page Inspector jest dołączany do narzędzia Microsoft Web Developer Tools.</span><span class="sxs-lookup"><span data-stu-id="7bb41-124">Page Inspector is bundled with Microsoft Web Developer Tools.</span></span> <span data-ttu-id="7bb41-125">Najnowsza wersja to 1.3.</span><span class="sxs-lookup"><span data-stu-id="7bb41-125">The latest version is 1.3.</span></span> <span data-ttu-id="7bb41-126">Aby sprawdzić, która wersja ma, uruchom Visual Studio i wybierz **Microsoft Visual Studio** z **pomocy** menu.</span><span class="sxs-lookup"><span data-stu-id="7bb41-126">To check which version you have, run Visual Studio and select **About Microsoft Visual Studio** from the **Help** menu.</span></span>

<a id="_creating_a_web"></a><a id="_2_creating_a"></a>

## <a name="create-a-web-application"></a><span data-ttu-id="7bb41-127">Tworzenie aplikacji sieci Web</span><span class="sxs-lookup"><span data-stu-id="7bb41-127">Create a Web Application</span></span>

<span data-ttu-id="7bb41-128">Najpierw należy utworzyć aplikacji sieci web, która będzie używana narzędzie Page Inspector z.</span><span class="sxs-lookup"><span data-stu-id="7bb41-128">First, you will create a web application that you will use Page Inspector with.</span></span> <span data-ttu-id="7bb41-129">W programie Visual Studio, wybierz **pliku** &gt; **nowy projekt**.</span><span class="sxs-lookup"><span data-stu-id="7bb41-129">In Visual Studio, choose **File** &gt; **New Project**.</span></span> <span data-ttu-id="7bb41-130">Po lewej stronie rozwiń węzeł **Visual C#**, wybierz pozycję **Web**, a następnie wybierz **aplikacji formularzy sieci Web ASP.NET**.</span><span class="sxs-lookup"><span data-stu-id="7bb41-130">On the left, expand **Visual C#**, select **Web**, and then select **ASP.NET Web Forms Application**.</span></span>

![Nowa aplikacja formularzy sieci Web](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image1.png)

<span data-ttu-id="7bb41-132">Kliknij przycisk **OK**.</span><span class="sxs-lookup"><span data-stu-id="7bb41-132">Click **OK**.</span></span>

<span data-ttu-id="7bb41-133">Aplikacja zostanie otwarta w **źródła** widoku.</span><span class="sxs-lookup"><span data-stu-id="7bb41-133">The application opens in **Source** view.</span></span>

![Nowej aplikacji formularzy sieci Web w widoku źródła](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image2.png)

<span data-ttu-id="7bb41-135">Teraz, po aplikacji do pracy z służy narzędzie Page Inspector do zbadania i zmodyfikowania go.</span><span class="sxs-lookup"><span data-stu-id="7bb41-135">Now that you have an application to work with, you can use Page Inspector to examine and modify it.</span></span>

<a id="_starting_page_inspector"></a><a id="_3_starting_page"></a><a id="_3_using_page"></a>

## <a name="use-page-inspector-to-view-the-application"></a><span data-ttu-id="7bb41-136">Narzędzie Page Inspector umożliwia wyświetlanie aplikacji</span><span class="sxs-lookup"><span data-stu-id="7bb41-136">Use Page Inspector to View the Application</span></span>

<span data-ttu-id="7bb41-137">Następnie zostaną wyświetlone aplikacji z narzędziem Page Inspector.</span><span class="sxs-lookup"><span data-stu-id="7bb41-137">Next, you will view the application with Page Inspector.</span></span> <span data-ttu-id="7bb41-138">W **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy projekt, a następnie wybierz pozycję **widoku w narzędzie Page Inspector**.</span><span class="sxs-lookup"><span data-stu-id="7bb41-138">In **Solution Explorer**, right click the project, and then choose **View in Page Inspector**.</span></span>

![Widok w narzędzie Page Inspector](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image3.png)

<span data-ttu-id="7bb41-140">Domyślnie gdy narzędzie Page Inspector uruchamia się po raz pierwszy jest zadokowany wąskie okna po lewej stronie środowiska Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="7bb41-140">By default, when Page Inspector launches for the first time, it is docked as a narrow window on the left side of the Visual Studio environment.</span></span> <span data-ttu-id="7bb41-141">Pozostaw zadokowane po lewej stronie i Ustaw szerokość jest wygodne dla Ciebie lub dock w jednym z obszarów narzędzia na górze, dołu lub prawej:</span><span class="sxs-lookup"><span data-stu-id="7bb41-141">Leave it docked on the left side and set it to a width that is comfortable for you, or dock it in one of the tool areas on the top, bottom, or right:</span></span>

![Narzędzie Page Inspector pozycjach](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image4.png)

<span data-ttu-id="7bb41-143">Jeśli Oddokuj okna narzędzia Page Inspector, możesz go umieścić poza programem Visual Studio lub nawet na drugim monitorze Jeśli masz.</span><span class="sxs-lookup"><span data-stu-id="7bb41-143">If you undock the Page Inspector window, you can place it outside Visual Studio, or even on a second monitor if you have one.</span></span> <span data-ttu-id="7bb41-144">Jednak aby klawisze ALT + TAB między narzędzie Page Inspector i Visual Studio podczas okna narzędzia Page Inspector jest zadokowany, przejdź do **narzędzia** &gt; **opcje** &gt;  **Środowiska** &gt; **zakładek i okien**i w obszarze **kartę również**, wyczyść pole wyboru o nazwie **zawsze nad przestawne okna narzędzi Okno główne**:</span><span class="sxs-lookup"><span data-stu-id="7bb41-144">However, in order to ALT+TAB between Page Inspector and Visual Studio when the Page Inspector window is undocked, go to **Tools** &gt; **Options** &gt; **Environment** &gt; **Tabs and Windows**, and under **Tab Well**, clear the check box called **Floating tool windows always stay on top of the main window**:</span></span>

![Wyczyść pole wyboru przestawne windows narzędzia do ALT + TAB między Visual Studio i niezadokowane okna narzędzia Page Inspector](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image5.png)

<span data-ttu-id="7bb41-146">Górne okienko okna narzędzia Page Inspector pokazuje bieżącą stronę w przeglądarce.</span><span class="sxs-lookup"><span data-stu-id="7bb41-146">The top pane of the Page Inspector window shows the current page in a browser window.</span></span> <span data-ttu-id="7bb41-147">Dolne okienko zawiera strony w kod znaczników HTML po lewej stronie, a niektóre karty po prawej stronie, które pozwalają sprawdzić różnych aspektów strony.</span><span class="sxs-lookup"><span data-stu-id="7bb41-147">The bottom pane shows the page in HTML markup on the left, and some tabs on the right that let you inspect different aspects of the page.</span></span> <span data-ttu-id="7bb41-148">Dolne okienko jest podobny do [F12 Developer Tools](https://msdn.microsoft.com/en-us/ie/aa740478) w programie Internet Explorer.</span><span class="sxs-lookup"><span data-stu-id="7bb41-148">The bottom pane is similar to the [F12 Developer Tools](https://msdn.microsoft.com/en-us/ie/aa740478) in Internet Explorer.</span></span> <span data-ttu-id="7bb41-149">(Jednak w przeciwieństwie do narzędzia developer służy narzędzie Page Inspector bezpośrednio w programie Visual Studio.)</span><span class="sxs-lookup"><span data-stu-id="7bb41-149">(However, unlike the developer tools, you can use Page Inspector right within Visual Studio.)</span></span>

![Inspektor strony](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image6.png)

<span data-ttu-id="7bb41-151">W tym samouczku użyjesz okienku przeglądarkę narzędzia Page Inspector i **HTML** i **style** karty, aby szybko ułatwiają nawigowanie i wprowadzenia zmian w aplikacji.</span><span class="sxs-lookup"><span data-stu-id="7bb41-151">In this tutorial, you will use the Page Inspector browser pane, and the **HTML** and **Styles** tabs to help you rapidly navigate and make changes to the application.</span></span>

<a id="_4_inspection_mode"></a>
## <a name="enable-inspection-mode"></a><span data-ttu-id="7bb41-152">Włącz tryb inspekcji</span><span class="sxs-lookup"><span data-stu-id="7bb41-152">Enable Inspection Mode</span></span>

<span data-ttu-id="7bb41-153">Następnie pojawi się, jak działa tryb inspekcji Page Inspector.</span><span class="sxs-lookup"><span data-stu-id="7bb41-153">Next, you will see how Page Inspector's Inspection Mode works.</span></span> <span data-ttu-id="7bb41-154">Kliknij w oknie narzędzia Page Inspector **inspekcję** przycisku.</span><span class="sxs-lookup"><span data-stu-id="7bb41-154">In the Page Inspector window, click the **Inspect** button.</span></span>

![Sprawdź Element](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image7.png)

<span data-ttu-id="7bb41-156">Aby zobaczyć, tryb inspekcji w akcji, myszą różnych części strony w oknie przeglądarki narzędzie Page Inspector.</span><span class="sxs-lookup"><span data-stu-id="7bb41-156">To see inspection mode in action, move your mouse over different parts of the page within the Page Inspector browser window.</span></span> <span data-ttu-id="7bb41-157">Jak, kursor zmienia się na znak plus dużych i jest wyróżniony element poniżej:</span><span class="sxs-lookup"><span data-stu-id="7bb41-157">As you do, the mouse pointer changes to a large plus sign, and the element underneath is highlighted:</span></span>

![Ustawiając kursor nad div.content otoki](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image8.png)

<span data-ttu-id="7bb41-159">Pamiętaj, że podczas przesuwania wskaźnika myszy</span><span class="sxs-lookup"><span data-stu-id="7bb41-159">As you move the mouse pointer, note that</span></span>

- <span data-ttu-id="7bb41-160">Zawartość **źródła** zmian do wyświetlenia znaczników odpowiadający wybranego elementu na stronie widoku.</span><span class="sxs-lookup"><span data-stu-id="7bb41-160">The content in **Source** view changes to show the markup corresponding to the selected element on the page.</span></span> <span data-ttu-id="7bb41-161">Zostanie wyróżniona odpowiednich znaczników.</span><span class="sxs-lookup"><span data-stu-id="7bb41-161">The relevant markup is highlighted.</span></span> <span data-ttu-id="7bb41-162">Jeśli źródło jest w innym pliku, czy plik jest otwarty w widoku źródła z odpowiednich wyróżniony kod znaczników.</span><span class="sxs-lookup"><span data-stu-id="7bb41-162">If the source is in another file, that file is opened in Source view with the relevant markup highlighted.</span></span>

- <span data-ttu-id="7bb41-163">Kod znaczników wyświetlanych w **HTML** kartę w narzędzie Page Inspector zmienia także odpowiadają wybranego elementu na stronie.</span><span class="sxs-lookup"><span data-stu-id="7bb41-163">The markup displayed in the **HTML** tab in Page Inspector also changes to correspond to the selected element on the page.</span></span> <span data-ttu-id="7bb41-164">W **HTML** karcie przedstawiono istotne znaczników.</span><span class="sxs-lookup"><span data-stu-id="7bb41-164">In the **HTML** tab, the relevant markup is outlined.</span></span>

- <span data-ttu-id="7bb41-165">**Style** karta zawiera reguły CSS odpowiednie do bieżącego zaznaczenia.</span><span class="sxs-lookup"><span data-stu-id="7bb41-165">The **Styles** tab shows the CSS rules relevant to the current selection.</span></span>

<a id="_5_using_page"></a>

## <a name="use-page-inspector-to-make-changes-to-markup"></a><span data-ttu-id="7bb41-166">Narzędzie Page Inspector umożliwia zmianę znaczników</span><span class="sxs-lookup"><span data-stu-id="7bb41-166">Use Page Inspector to Make Changes to Markup</span></span>

<span data-ttu-id="7bb41-167">Teraz będą widzieć, jak można użyć narzędzia Page Inspector można znaleźć i wprowadzać zmiany znaczników lub tekst, którego lokalizacja może nie być od razu widoczne.</span><span class="sxs-lookup"><span data-stu-id="7bb41-167">Now you will see how you can use Page Inspector to find and make changes to markup or text whose location might not be immediately obvious.</span></span>

<span data-ttu-id="7bb41-168">Umieść narzędzie Page Inspector w trybie inspekcji, a następnie przewiń w dół strony głównej.</span><span class="sxs-lookup"><span data-stu-id="7bb41-168">Put Page Inspector in Inspection Mode and then scroll to the bottom of the home page.</span></span>

<span data-ttu-id="7bb41-169">Jak wprowadzasz obszaru stopki zostanie otwarte narzędzie Page Inspector *Site.Master* pliku układu **źródła** widok na karcie tymczasowego na prawo od drugiej karty i zaznacza sekcji wzorca strony, które zostały wybrane.</span><span class="sxs-lookup"><span data-stu-id="7bb41-169">As soon as you enter the footer area, Page Inspector opens the *Site.Master* layout file in **Source** view in a temporary tab to the right of the other tabs and highlights the section of the master page that you have selected.</span></span> <span data-ttu-id="7bb41-170">To pokazuje sposób narzędzie Page Inspector można znaleźć i wyświetlić zawartość na stronie, która może faktycznie pochodzą z innego pliku niż ten, który je otworzył.</span><span class="sxs-lookup"><span data-stu-id="7bb41-170">This shows you how Page Inspector can find and display content on a page that might actually come from a different file than the one you originally opened.</span></span>

![Najważniejsze funkcje stopki w trybie inspekcji](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image9.png)

<span data-ttu-id="7bb41-172">W oknie przeglądarki narzędzie Page Inspector Przesuń wskaźnik myszy nad wiersza o prawach autorskich <a id="a"> </a>zauważyć.</span><span class="sxs-lookup"><span data-stu-id="7bb41-172">In the Page Inspector browser window, move your mouse pointer over the line with the copyright <a id="a"></a>notice.</span></span>

<span data-ttu-id="7bb41-173">W *Site.Master* zostanie wyróżniona strony, odpowiednim wierszu.</span><span class="sxs-lookup"><span data-stu-id="7bb41-173">In the *Site.Master* page, the corresponding line is highlighted.</span></span>

![Stopka wiersza o prawach autorskich wyróżnione](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image10.png)

<span data-ttu-id="7bb41-175">Dodaj tekst do końca wiersza w *Site.Master* pliku.</span><span class="sxs-lookup"><span data-stu-id="7bb41-175">Add some text to the end of the line in the *Site.Master* file.</span></span>

<span data-ttu-id="7bb41-176">&lt;p&gt;&amp;kopiowania; &lt;%: DateTime.Now.Year %&gt; -Moje skały aplikacji ASP.NET!&lt; /p&gt;</span><span class="sxs-lookup"><span data-stu-id="7bb41-176">&lt;p&gt;&amp;copy; &lt;%: DateTime.Now.Year %&gt; - My ASP.NET Application Rocks!&lt;/p&gt;</span></span>

<span data-ttu-id="7bb41-177">Teraz naciśnij kombinację klawiszy Ctrl + Alt + Enter lub kliknij przycisk paska aktualizacji, aby wyświetlić wyniki w oknie przeglądarki narzędzie Page Inspector.</span><span class="sxs-lookup"><span data-stu-id="7bb41-177">Now, press Ctrl+Alt+Enter or click the Update Bar to see the results in the Page Inspector browser window.</span></span>

![Moje skały aplikacji ASP.NET!](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image11.png)

<span data-ttu-id="7bb41-179">Może mieć uważasz, że stopki został na *Default.aspx* strony, ale wyłączone w strony wzorcowej układu i narzędzie Page Inspector uznała, że dla Ciebie.</span><span class="sxs-lookup"><span data-stu-id="7bb41-179">You might have thought that the footer was on the *Default.aspx* page, but it turned out to be in the master layout page, and Page Inspector found it for you.</span></span>

<a id="_6_inspection_mode"></a>

## <a name="inspection-mode-and-the-html-window"></a><span data-ttu-id="7bb41-180">Tryb inspekcji i okno HTML</span><span class="sxs-lookup"><span data-stu-id="7bb41-180">Inspection Mode and the HTML Window</span></span>

<span data-ttu-id="7bb41-181">Następnie należy krótki przegląd okno HTML i jak mapuje elementy dla Ciebie.</span><span class="sxs-lookup"><span data-stu-id="7bb41-181">Next, you will have a quick look at the HTML window and how it maps elements for you.</span></span>

<span data-ttu-id="7bb41-182">Narzędzie Page Inspector należy umieścić w trybie inspekcji.</span><span class="sxs-lookup"><span data-stu-id="7bb41-182">Put Page Inspector in Inspection Mode.</span></span>

![Sprawdź Element](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image12.png)

<span data-ttu-id="7bb41-184">Kliknij w górnej części strony, stwierdzający "tutaj znak logo".</span><span class="sxs-lookup"><span data-stu-id="7bb41-184">Click the top part of the page that says "your logo here".</span></span> <span data-ttu-id="7bb41-185">Są badanie dany element bardziej szczegółowo dzięki wyświetlana w oknie przeglądarki już zmiany podczas przesuwania wskaźnika myszy.</span><span class="sxs-lookup"><span data-stu-id="7bb41-185">You are examining a particular element in more detail, so the display in the browser window no longer changes as you move the mouse pointer.</span></span>

<span data-ttu-id="7bb41-186">Teraz umieść wskaźnik myszy **HTML** okna.</span><span class="sxs-lookup"><span data-stu-id="7bb41-186">Now move the mouse pointer to the **HTML** window.</span></span> <span data-ttu-id="7bb41-187">Podczas przesuwania wskaźnika myszy narzędzie Page Inspector przedstawiono w elemencie **HTML** okna i zaznacza odpowiadający mu element w oknie przeglądarki.</span><span class="sxs-lookup"><span data-stu-id="7bb41-187">As you move the mouse pointer, Page Inspector outlines the element within the **HTML** window and highlights the corresponding element in the browser window.</span></span>

![Okno HTML](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image13.png)

<span data-ttu-id="7bb41-189">Jak wcześniej, zostanie otwarte narzędzie Page Inspector *Site.Master* pliku dla Ciebie na karcie tymczasowego. Kliknij kartę Site.Master i odpowiedni kod znaczników jest wyróżniona w &lt;nagłówka&gt; sekcji:</span><span class="sxs-lookup"><span data-stu-id="7bb41-189">As before, Page Inspector opens the *Site.Master* file for you in a temporary tab. Click the Site.Master tab, and the corresponding markup is highlighted in the &lt;header&gt; section:</span></span>

![Wyróżnione znaczników](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image14.png)

<a id="_using_page_inspector"></a><a id="_7_previewing_css"></a>

## <a name="preview-css-changes-in-the-styles-window"></a><span data-ttu-id="7bb41-191">Podgląd zmian CSS w oknie style</span><span class="sxs-lookup"><span data-stu-id="7bb41-191">Preview CSS Changes in the Styles Window</span></span>

<span data-ttu-id="7bb41-192">Następnie zostanie wyświetlony, jak używasz narzędzia Page Inspector **style** okno, aby wyświetlić podgląd zmian do arkusza CSS.</span><span class="sxs-lookup"><span data-stu-id="7bb41-192">Next, you will see how you can use the Page Inspector **Styles** window to preview changes to CSS.</span></span>

<span data-ttu-id="7bb41-193">Kliknij przycisk **inspekcję** przycisk, aby włączyć tryb inspekcji narzędzie Page Inspector.</span><span class="sxs-lookup"><span data-stu-id="7bb41-193">Click the **Inspect** button to put Page Inspector in Inspection Mode.</span></span>

<span data-ttu-id="7bb41-194">W oknie przeglądarki narzędzie Page Inspector, przenieś wskaźnik myszy w sekcji "Strona główna" do **div.content otoki** jest wyświetlana etykieta.</span><span class="sxs-lookup"><span data-stu-id="7bb41-194">In the Page Inspector browser window, move the mouse pointer over the "Home Page" section until the **div.content-wrapper** label appears.</span></span>

![Ustawiając kursor nad elementów](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image15.png)

<span data-ttu-id="7bb41-196">Kliknij wewnątrz sekcji otoki div.content, a następnie przesuń wskaźnik myszy do **style** okna.</span><span class="sxs-lookup"><span data-stu-id="7bb41-196">Click within the div.content-wrapper section once, and then move the mouse pointer to the **Styles** window.</span></span> <span data-ttu-id="7bb41-197">W obszarze selektor klasy otoki .content .featured wyczyść i zaznacz pole wyboru dla właściwości kolor tła.</span><span class="sxs-lookup"><span data-stu-id="7bb41-197">Under the .featured .content-wrapper class selector, clear and select the checkbox for the background-color property.</span></span>

![Kolor tła wyczyść](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image16.png)

<span data-ttu-id="7bb41-199">Zwróć uwagę, jak zmiana Wyświetla podgląd natychmiast w oknie przeglądarki narzędzie Page Inspector.</span><span class="sxs-lookup"><span data-stu-id="7bb41-199">Notice how the change previews instantly in the Page Inspector browser window.</span></span>

<span data-ttu-id="7bb41-200">Zaznacz pole wyboru ponownie, kliknij dwukrotnie wartość właściwości i zmień go na `red`.</span><span class="sxs-lookup"><span data-stu-id="7bb41-200">Select the checkbox again, then double-click the property value and change it to `red`.</span></span> <span data-ttu-id="7bb41-201">Zmiana przedstawia natychmiast:</span><span class="sxs-lookup"><span data-stu-id="7bb41-201">The change shows immediately:</span></span>

![Kolor tła czerwony](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image17.png)

<span data-ttu-id="7bb41-203">**Style** sprawia, że okno łatwe do testowania i Podgląd CSS zmienia przed dokonaniem zmiany stylu arkusza samej siebie.</span><span class="sxs-lookup"><span data-stu-id="7bb41-203">The **Styles** window makes it easy to test and preview CSS changes before you commit the changes to the style sheet itself.</span></span>

<a id="css_auto_sync"></a>
## <a name="css-auto-sync"></a><span data-ttu-id="7bb41-204">Automatyczna synchronizacja z CSS</span><span class="sxs-lookup"><span data-stu-id="7bb41-204">CSS Auto Sync</span></span>

> [!NOTE]
> <span data-ttu-id="7bb41-205">Ta funkcja wymaga wersji 1.3 narzędzie Page Inspector.</span><span class="sxs-lookup"><span data-stu-id="7bb41-205">This feature requires version 1.3 of Page Inspector.</span></span>


<span data-ttu-id="7bb41-206">Funkcja automatycznej synchronizacji CSS służy do bezpośredniego edytowania pliku CSS i zobaczyć zmiany bezpośrednio w przeglądarce narzędzie Page Inspector.</span><span class="sxs-lookup"><span data-stu-id="7bb41-206">The CSS Auto-Sync feature allows you to edit a CSS file directly, and see the changes immediately in the Page Inspector browser.</span></span>

<span data-ttu-id="7bb41-207">Kliknij przycisk **inspekcję** chcesz włączyć tryb inspekcji narzędzie Page Inspector.</span><span class="sxs-lookup"><span data-stu-id="7bb41-207">Click **Inspect** to put Page Inspector in Inspection Mode.</span></span>

<span data-ttu-id="7bb41-208">W przeglądarce narzędzie Page Inspector, przenieś wskaźnik myszy w sekcji "Strona główna" do **div.content otoki** jest wyświetlana etykieta.</span><span class="sxs-lookup"><span data-stu-id="7bb41-208">In the Page Inspector browser, move the mouse pointer over the "Home Page" section until the **div.content-wrapper** label appears.</span></span> <span data-ttu-id="7bb41-209">Kliknij raz, aby wybrać tego elementu.</span><span class="sxs-lookup"><span data-stu-id="7bb41-209">Click once to select this element.</span></span>

<span data-ttu-id="7bb41-210">**Syles** oknie wyświetlane są wszystkie reguły CSS dla tego elementu.</span><span class="sxs-lookup"><span data-stu-id="7bb41-210">The **Syles** window shows all of the CSS rules for this element.</span></span> <span data-ttu-id="7bb41-211">Przewiń w dół do selektora klasy Znajdź .featured .content otoki.</span><span class="sxs-lookup"><span data-stu-id="7bb41-211">Scroll down to find the .featured .content-wrapper class selector.</span></span> <span data-ttu-id="7bb41-212">Kliknij pozycję ".featured .content otoki".</span><span class="sxs-lookup"><span data-stu-id="7bb41-212">Click on ".featured .content-wrapper".</span></span> <span data-ttu-id="7bb41-213">Narzędzie Page Inspector otwiera plik kodu CSS, która definiuje ten styl (Site.css) i zaznacza odpowiednie stylu CSS.</span><span class="sxs-lookup"><span data-stu-id="7bb41-213">Page Inspector opens the CSS file that defines this style (Site.css) and highlights the corresponding CSS style.</span></span>

![Pliku CSS](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image18.png)

<span data-ttu-id="7bb41-215">Teraz Zmień wartość atrybutu `background-color` do "red".</span><span class="sxs-lookup"><span data-stu-id="7bb41-215">Now change the value for `background-color` to "red".</span></span> <span data-ttu-id="7bb41-216">W przeglądarce narzędzie Page Inspector zmiana pojawi się natychmiast.</span><span class="sxs-lookup"><span data-stu-id="7bb41-216">The change appears immediately in the Page Inspector browser.</span></span>

![Narzędzie Page Inspector przeglądarki](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image19.png)

<a id="css_color_picker"></a>

## <a name="using-the-css-color-picker"></a><span data-ttu-id="7bb41-218">Za pomocą selektora kolorów CSS</span><span class="sxs-lookup"><span data-stu-id="7bb41-218">Using the CSS Color Picker</span></span>

<span data-ttu-id="7bb41-219">Następnie dowiesz się, jak używać narzędzia Page Inspector można szybko znaleźć i zmienić CSS dla wyróżnionego tekstu w domyślnej aplikacji.</span><span class="sxs-lookup"><span data-stu-id="7bb41-219">Next, you'll learn how to use Page Inspector to quickly find and change the CSS for highlighted text in the default application.</span></span> <span data-ttu-id="7bb41-220">W tym przykładzie decydujesz się nie niebieskie wyróżnienie, takich jak i chcesz je zmienić kolor na inny.</span><span class="sxs-lookup"><span data-stu-id="7bb41-220">In this example, you've decided that you don't like the blue highlighting and want to change it to another color.</span></span>

<span data-ttu-id="7bb41-221">Kliknij przycisk **inspekcję** przycisku.</span><span class="sxs-lookup"><span data-stu-id="7bb41-221">Click the **Inspect** button.</span></span>

![Sprawdź Element](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image20.png)

<span data-ttu-id="7bb41-223">W oknie przeglądarki narzędzie Page Inspector Przesuń wskaźnik myszy nad wyróżnionego "wideo, samouczki i przykłady" tak, aby CSS "znak" etykiety tekstu.</span><span class="sxs-lookup"><span data-stu-id="7bb41-223">In the Page Inspector browser window, move the mouse pointer over the highlighted "videos, tutorials, and samples" text so that the CSS "mark" label appears.</span></span>

![Kursor myszy nad elementem znaku](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image21.png)

<span data-ttu-id="7bb41-225">Kliknij tekst, aby go wybrać.</span><span class="sxs-lookup"><span data-stu-id="7bb41-225">Click the text to select it.</span></span> <span data-ttu-id="7bb41-226">Zostanie wyświetlony odpowiedni selektor CSS w znaku w dolnej części **style** okna.</span><span class="sxs-lookup"><span data-stu-id="7bb41-226">The corresponding CSS mark selector appears at the bottom of the **Styles** window.</span></span>

![Selektor znacznika w oknie style](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image22.png)

<span data-ttu-id="7bb41-228">Kliknij znacznik wyboru.</span><span class="sxs-lookup"><span data-stu-id="7bb41-228">Click the mark selector.</span></span> <span data-ttu-id="7bb41-229">Spowoduje to otwarcie *Site.css* plików dla aplikacji sieci web.</span><span class="sxs-lookup"><span data-stu-id="7bb41-229">This opens the *Site.css* file for the web application.</span></span> <span data-ttu-id="7bb41-230">Kliknij kartę Site.css i odpowiednie CSS selektora zostanie wyróżniona:</span><span class="sxs-lookup"><span data-stu-id="7bb41-230">Click the Site.css tab, and the corresponding CSS for the selector is highlighted:</span></span>

![Selektor znacznika w arkuszu stylów](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image23.png)

<span data-ttu-id="7bb41-232">Wybierz i Usuń wiersz z właściwością kolor tła.</span><span class="sxs-lookup"><span data-stu-id="7bb41-232">Select and remove the line with the background-color property.</span></span>

<span data-ttu-id="7bb41-233">Teraz użyje próbnika kolorów programu Visual Studio 2012 CSS, aby wybrać nowy kolor **oznaczyć** właściwości kolor tła.</span><span class="sxs-lookup"><span data-stu-id="7bb41-233">You will now use the new Visual Studio 2012 CSS color picker to choose a new color for the **mark** background-color property.</span></span>

<a id="_using_the_visual"></a>

### <a name="using-the-visual-studio-2012-css-color-picker"></a><span data-ttu-id="7bb41-234">Za pomocą programu Visual Studio 2012 CSS próbnika</span><span class="sxs-lookup"><span data-stu-id="7bb41-234">Using the Visual Studio 2012 CSS Color Picker</span></span>

<span data-ttu-id="7bb41-235">Edytor CSS w programie Visual Studio 2012 zawiera próbnika kolorów, który można łatwo wybrać i Wstaw kolorów.</span><span class="sxs-lookup"><span data-stu-id="7bb41-235">The CSS editor in Visual Studio 2012 has a color picker that makes it easy to choose and insert colors.</span></span> <span data-ttu-id="7bb41-236">Ma prosty pasek koloru i selektor "pop rozwijanej", który zapewnia bardziej precyzyjną kontrolę.</span><span class="sxs-lookup"><span data-stu-id="7bb41-236">It has a simple color bar and a "pop-down" picker that offers finer control.</span></span>

<span data-ttu-id="7bb41-237">Próbnika kolorów zawiera standardowe paletę kolorów, obsługuje standardowe kolor nazwy i skrótu, kolorów RGB, RGBA HSL i HSLA i przechowuje listę kolorów używanych ostatnio w dokumencie.</span><span class="sxs-lookup"><span data-stu-id="7bb41-237">The color picker includes a standard palette of colors, supports standard color names, hash codes, RGB, RGBA, HSL, and HSLA colors, and maintains a list of the colors you've used most recently in the document.</span></span>

<span data-ttu-id="7bb41-238">Gdy właściwość kolor tła była wierszu "bc" i naciśnij klawisz strzałki w dół raz.</span><span class="sxs-lookup"><span data-stu-id="7bb41-238">On the line where the background-color property was, type "bc" and press the down arrow once.</span></span>

<span data-ttu-id="7bb41-239">Po wpisaniu pierwszych znaków każdego wyrazu we właściwości rozdzielone łącznikiem, takich jak "background-color" IntelliSense filtry na liście pokazywać tylko właściwości, które odpowiadają:</span><span class="sxs-lookup"><span data-stu-id="7bb41-239">When you type the first character of each word in a hyphen-separated property like "background-color", IntelliSense filters the list for you to show only the properties that match:</span></span>

![IntelliSense filtrowane wartości](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image24.png)

<span data-ttu-id="7bb41-241">Teraz wpisz dwukropkiem.</span><span class="sxs-lookup"><span data-stu-id="7bb41-241">Now type a colon.</span></span> <span data-ttu-id="7bb41-242">Po wykonaniu czynności są wstawiane nazwę właściwości pełny kolor tła.</span><span class="sxs-lookup"><span data-stu-id="7bb41-242">When you do, the full background-color property name is inserted.</span></span> <span data-ttu-id="7bb41-243">Typ  **#**  lub **rgb (**, i zostanie wyświetlony pasek selektora kolorów:</span><span class="sxs-lookup"><span data-stu-id="7bb41-243">Type **#** or **rgb(**, and the color picker bar appears:</span></span>

![Na pasku próbnika kolorów CSS](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image25.png)

<span data-ttu-id="7bb41-245">Aby zobaczyć, jak działa na pasku próbnika kolorów, kliknij jego kolorów przy użyciu wskaźnika myszy lub naciśnij klawisz strzałki w dół, a następnie użyj klawiszy strzałek w lewo i prawo przechodzenia kolorów.</span><span class="sxs-lookup"><span data-stu-id="7bb41-245">To see how the color picker bar works, click its colors with the mouse pointer, or press the down arrow key and then use the left and right arrow keys to traverse the colors.</span></span> <span data-ttu-id="7bb41-246">Podczas odwiedzania kolor odpowiednie wartości dla właściwości kolor tła jest przeglądany:</span><span class="sxs-lookup"><span data-stu-id="7bb41-246">When you visit a color, the corresponding value for the background-color property is previewed:</span></span>

![wartość właściwości kolor tła podglądu](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image26.png)

<span data-ttu-id="7bb41-248">W tym momencie można nacisnąć klawisz Enter Wybierz wartość, a następnie średnikiem (;) do ukończenia wpis CSS.</span><span class="sxs-lookup"><span data-stu-id="7bb41-248">At this point, you could press Enter to select the value and then a semicolon (;) to complete the CSS entry.</span></span> <span data-ttu-id="7bb41-249">Teraz przejdź do następnej sekcji, aby mogli przeglądać, jak działa próbnika kolorów w dół pop.</span><span class="sxs-lookup"><span data-stu-id="7bb41-249">For now, go on to the next section so that you can see how the color picker pop-down works.</span></span>

#### <a name="using-the-color-picker-pop-down"></a><span data-ttu-id="7bb41-250">Za pomocą selektora kolorów w dół Pop</span><span class="sxs-lookup"><span data-stu-id="7bb41-250">Using the Color Picker Pop-Down</span></span>

<span data-ttu-id="7bb41-251">Przy pasku kolorów nie ma dokładnie kolor, którego szukasz, można użyć pop rozwijanej próbnika kolorów.</span><span class="sxs-lookup"><span data-stu-id="7bb41-251">When the color bar doesn't have the exact color that you're looking for, you can use the color picker pop-down.</span></span>

<span data-ttu-id="7bb41-252">Aby go otworzyć, kliknij przycisk ostrokątny na prawym końcu paska koloru, lub jeden lub dwa razy klawisz strzałki w dół na klawiaturze.</span><span class="sxs-lookup"><span data-stu-id="7bb41-252">To open it, click the double chevron at the right end of the color bar, or press the Down Arrow once or twice on the keyboard.</span></span>

![Selektor koloru CSS Pop dół](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image27.png)

<span data-ttu-id="7bb41-254">Kliknij przycisk koloru z pionowy pasek po prawej stronie.</span><span class="sxs-lookup"><span data-stu-id="7bb41-254">Click a color from the vertical bar on the right.</span></span> <span data-ttu-id="7bb41-255">W oknie głównym wskazuje gradientu dla tego koloru.</span><span class="sxs-lookup"><span data-stu-id="7bb41-255">This shows a gradient for that color in the main window.</span></span> <span data-ttu-id="7bb41-256">Wybierz kolor bezpośrednio z pionowy pasek, naciskając klawisz Enter lub kliknij przycisk dowolnego punktu w oknie głównym, aby wybrać o większej dokładności.</span><span class="sxs-lookup"><span data-stu-id="7bb41-256">Choose a color directly from the vertical bar by pressing Enter, or click any point in the main window to choose with greater precision.</span></span>

<span data-ttu-id="7bb41-257">Jeśli na ekranie komputera, który ma być używany jest kolor (nie musi on być w interfejsie użytkownika programu Visual Studio), jego wartość można przechwycić przy użyciu narzędzia służącego do pobierania w prawej dolnej.</span><span class="sxs-lookup"><span data-stu-id="7bb41-257">If there is a color on your computer screen that you want to use (it doesn't have to be inside the Visual Studio user interface), you can capture its value by using the eyedropper tool on the lower right.</span></span>

<span data-ttu-id="7bb41-258">Możesz również zmienić przezroczystość koloru za pomocą suwaka w dolnej części próbnika kolorów.</span><span class="sxs-lookup"><span data-stu-id="7bb41-258">You also can change the opacity of a color by moving the slider at the bottom of the color picker.</span></span> <span data-ttu-id="7bb41-259">Grozi to zmiany wartości z RGBA kolorów, ponieważ RGBA format może reprezentować nieprzezroczystość.</span><span class="sxs-lookup"><span data-stu-id="7bb41-259">Doing so changes color values to RGBA values because the RGBA format can represent opacity.</span></span>

<span data-ttu-id="7bb41-260">Po wybraniu opcji koloru, naciśnij klawisz Enter, a następnie wpisz średnik przeprowadzenie wpisu kolor tła w *Site.css* pliku.</span><span class="sxs-lookup"><span data-stu-id="7bb41-260">After you have chosen a color, press Enter, and then type a semicolon to complete the background-color entry in the *Site.css* file.</span></span>

<a id="_the_update_bar"></a>

### <a name="the-page-inspector-update-bar"></a><span data-ttu-id="7bb41-261">Na pasku aktualizacji inspektora strony</span><span class="sxs-lookup"><span data-stu-id="7bb41-261">The Page Inspector Update Bar</span></span>

<span data-ttu-id="7bb41-262">Narzędzie Page Inspector natychmiast wykrywa zmiany *Site.css* pliku (lub dowolny plik w aplikacji) i wyświetla alert w pasku aktualizacji.</span><span class="sxs-lookup"><span data-stu-id="7bb41-262">Page Inspector immediately detects the change to the *Site.css* file (or to any file in the application) and displays an alert in an update bar.</span></span>

![Pasek aktualizacji](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image28.png)

<span data-ttu-id="7bb41-264">Aby zapisać wszystkie pliki i odświeżyć przeglądarkę narzędzia Page Inspector, naciśnij kombinację klawiszy Ctrl + Alt + Enter lub kliknij przycisk paska aktualizacji.</span><span class="sxs-lookup"><span data-stu-id="7bb41-264">To save all your files and refresh the Page Inspector browser, press Ctrl+Alt+Enter or click the update bar.</span></span> <span data-ttu-id="7bb41-265">Zmiana koloru wyróżnienia zostanie wyświetlona w przeglądarce:</span><span class="sxs-lookup"><span data-stu-id="7bb41-265">The change in the highlight color appears in the browser:</span></span>

![Zmienić kolor zaznaczenia](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image29.png)

<a id="_using_page_inspector_1"></a><span data-ttu-id="7bb41-267">Zwróć uwagę, wygodnie odświeżyć przeglądarkę narzędzia Page Inspector bezpośrednio w w środowisku Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="7bb41-267">Notice that you conveniently refreshed the Page Inspector browser right from within the Visual Studio environment.</span></span> <span data-ttu-id="7bb41-268">Za pomocą narzędzia Page Inspector zamiast zewnętrznej przeglądarki umożliwia pozostanie w edytorze podczas opracowywania aplikacji sieci web.</span><span class="sxs-lookup"><span data-stu-id="7bb41-268">Using Page Inspector instead of an external browser lets you stay in the editor when you develop your web applications.</span></span>

## <a name="see-also"></a><span data-ttu-id="7bb41-269">Zobacz też</span><span class="sxs-lookup"><span data-stu-id="7bb41-269">See Also</span></span>

<span data-ttu-id="7bb41-270">[Wprowadzenie do narzędzia Page Inspector](https://channel9.msdn.com/posts/visual-studio-vnext-introducing-page-inspector) (wideo z witryny Channel 9)</span><span class="sxs-lookup"><span data-stu-id="7bb41-270">[Introducing Page Inspector](https://channel9.msdn.com/posts/visual-studio-vnext-introducing-page-inspector) (Channel 9 video)</span></span>

<span data-ttu-id="7bb41-271">[Komunikaty o błędach narzędzia Page Inspector](https://go.microsoft.com/?linkid=9813062) (MSDN)</span><span class="sxs-lookup"><span data-stu-id="7bb41-271">[Page Inspector Error Messages](https://go.microsoft.com/?linkid=9813062) (MSDN)</span></span>
