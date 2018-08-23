---
uid: mvc/overview/views/using-page-inspector-in-aspnet-mvc
title: Za pomocą narzędzia Page Inspector we wzorcu ASP.NET MVC | Dokumentacja firmy Microsoft
author: rick-anderson
description: Narzędzie Page Inspector w programie Visual Studio 2012 jest narzędziem do projektowania sieci web za pomocą zintegrowanej przeglądarki. Wybierz dowolny element w zintegrowanej przeglądarki i narzędzie Page Inspector i...
ms.author: riande
ms.date: 08/15/2012
ms.assetid: c7e4e1ab-4932-4614-9f53-aaf7c706d498
msc.legacyurl: /mvc/overview/views/using-page-inspector-in-aspnet-mvc
msc.type: authoredcontent
ms.openlocfilehash: c465b0bac9af90a892d6e62a327ba36977d08d4a
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/16/2018
ms.locfileid: "41757187"
---
<a name="using-page-inspector-in-aspnet-mvc"></a><span data-ttu-id="776ed-104">Za pomocą narzędzia Page Inspector we wzorcu ASP.NET MVC</span><span class="sxs-lookup"><span data-stu-id="776ed-104">Using Page Inspector in ASP.NET MVC</span></span>
====================
<span data-ttu-id="776ed-105">przez Tim Ammann</span><span class="sxs-lookup"><span data-stu-id="776ed-105">by Tim Ammann</span></span>

> <span data-ttu-id="776ed-106">Narzędzie Page Inspector w programie Visual Studio 2012 jest narzędziem do projektowania sieci web za pomocą zintegrowanej przeglądarki.</span><span class="sxs-lookup"><span data-stu-id="776ed-106">Page Inspector in Visual Studio 2012 is a web development tool with an integrated browser.</span></span> <span data-ttu-id="776ed-107">Wybranie dowolnego elementu w zintegrowanej przeglądarki i narzędzie Page Inspector natychmiast wyróżnia elementu źródłowego i arkusze CSS.</span><span class="sxs-lookup"><span data-stu-id="776ed-107">Select any element in the integrated browser, and Page Inspector instantly highlights the element's source and CSS.</span></span> <span data-ttu-id="776ed-108">Można przeglądać dowolny widok MVC, szybkie znalezienie źródła renderowanego kodu znaczników i za pomocą narzędzi przeglądarki bezpośrednio w środowisku Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="776ed-108">You can browse any MVC view, quickly find the sources of rendered markup, and use browser tools right within the Visual Studio environment.</span></span>
> 
> [<span data-ttu-id="776ed-109">Obejrzyj wideo</span><span class="sxs-lookup"><span data-stu-id="776ed-109">Watch the Video</span></span>](../../videos/mvc-4/using-page-inspector-in-aspnet-mvc.md)
> 
> <span data-ttu-id="776ed-110">W tym samouczku pokazano, jak włączyć tryb inspekcji, a następnie szybko Znajdź i Edytuj znaczniki i CSS w projekcie sieci web.</span><span class="sxs-lookup"><span data-stu-id="776ed-110">This tutorial shows how to enable Inspection Mode, and then quickly locate and edit markup and CSS within your web project.</span></span> <span data-ttu-id="776ed-111">W tym samouczku użyto projektu MVC, ale można również użyć narzędzia Page Inspector dla [formularzy sieci Web](https://go.microsoft.com/?linkid=9802001) i inne aplikacje platformy ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="776ed-111">The tutorial uses an MVC Project, but you can also use Page Inspector for [Web Forms](https://go.microsoft.com/?linkid=9802001) and other ASP.NET applications.</span></span>
> 
> <span data-ttu-id="776ed-112">Samouczek zawiera następujące sekcje:</span><span class="sxs-lookup"><span data-stu-id="776ed-112">The tutorial has the following sections:</span></span>
> 
> - [<span data-ttu-id="776ed-113">Wymagania wstępne</span><span class="sxs-lookup"><span data-stu-id="776ed-113">Prerequisites</span></span>](#_1_prerequisites)
> - [<span data-ttu-id="776ed-114">Tworzenie aplikacji sieci Web</span><span class="sxs-lookup"><span data-stu-id="776ed-114">Create a Web Application</span></span>](#_2_creating_a)
> - [<span data-ttu-id="776ed-115">Inspektor stron do przejdź do widoku</span><span class="sxs-lookup"><span data-stu-id="776ed-115">Use Page Inspector to Browse to a View</span></span>](#_3_using_page)
> - [<span data-ttu-id="776ed-116">Włącz tryb inspekcji</span><span class="sxs-lookup"><span data-stu-id="776ed-116">Enable Inspection Mode</span></span>](#_4_inspection_mode)
> - [<span data-ttu-id="776ed-117">Inspektor stron umożliwia wprowadzać zmiany znaczników</span><span class="sxs-lookup"><span data-stu-id="776ed-117">Use Page Inspector to Make Changes to Markup</span></span>](#_5_using_page)
> - [<span data-ttu-id="776ed-118">Tryb inspekcji i w oknie kodu HTML</span><span class="sxs-lookup"><span data-stu-id="776ed-118">Inspection Mode and the HTML Window</span></span>](#_6_inspection_mode)
> - [<span data-ttu-id="776ed-119">Zmiany arkusza CSS (wersja zapoznawcza) w oknie style</span><span class="sxs-lookup"><span data-stu-id="776ed-119">Preview CSS Changes in the Styles window</span></span>](#_7_previewing_css)
> - [<span data-ttu-id="776ed-120">Automatyczna synchronizacja z CSS</span><span class="sxs-lookup"><span data-stu-id="776ed-120">CSS Auto Sync</span></span>](#css_auto_sync)
> - [<span data-ttu-id="776ed-121">Za pomocą selektora kolorów CSS</span><span class="sxs-lookup"><span data-stu-id="776ed-121">Using the CSS Color Picker</span></span>](#css_color_picker)
> - [<span data-ttu-id="776ed-122">Mapowanie elementów strony dynamicznej języka JavaScript</span><span class="sxs-lookup"><span data-stu-id="776ed-122">Mapping Dynamic Page Elements to JavaScript</span></span>](#map_dynamic_elements)


<a id="_prerequisites"></a><a id="_1_prerequisites"></a>

## <a name="prerequisites"></a><span data-ttu-id="776ed-123">Wymagania wstępne</span><span class="sxs-lookup"><span data-stu-id="776ed-123">Prerequisites</span></span>

- <span data-ttu-id="776ed-124">[Program Visual Studio 2012](https://www.microsoft.com/visualstudio/11) lub [programu Visual Studio Express 2012 for Web](https://www.microsoft.com/visualstudio/11/downloads#express-web).</span><span class="sxs-lookup"><span data-stu-id="776ed-124">[Visual Studio 2012](https://www.microsoft.com/visualstudio/11) or [Visual Studio Express 2012 for Web](https://www.microsoft.com/visualstudio/11/downloads#express-web).</span></span>

> [!NOTE]
> <span data-ttu-id="776ed-125">Aby uzyskać najnowszą wersję narzędzia Page Inspector, użyj [Instalatora platformy sieci Web](https://go.microsoft.com/fwlink/?LinkId=255386) zainstalować zestaw Windows Azure SDK dla platformy .NET w wersji 2.0.</span><span class="sxs-lookup"><span data-stu-id="776ed-125">To get the latest version of Page Inspector, use [Web Platform Installer](https://go.microsoft.com/fwlink/?LinkId=255386) to install the Windows Azure SDK for .NET 2.0.</span></span>


<span data-ttu-id="776ed-126">Narzędzie Page Inspector jest umieszczany w pakietach za pomocą narzędzia Microsoft Web Developer Tools.</span><span class="sxs-lookup"><span data-stu-id="776ed-126">Page Inspector is bundled with Microsoft Web Developer Tools.</span></span> <span data-ttu-id="776ed-127">Najnowsza wersja to 1.3.</span><span class="sxs-lookup"><span data-stu-id="776ed-127">The latest version is 1.3.</span></span> <span data-ttu-id="776ed-128">Aby sprawdzić, która wersja mają, uruchom program Visual Studio i wybierz **Microsoft Visual Studio** z **pomocy** menu.</span><span class="sxs-lookup"><span data-stu-id="776ed-128">To check which version you have, run Visual Studio and select **About Microsoft Visual Studio** from the **Help** menu.</span></span>

<a id="_creating_a_web"></a><a id="_2_creating_a"></a>

## <a name="create-a-web-application"></a><span data-ttu-id="776ed-129">Tworzenie aplikacji sieci Web</span><span class="sxs-lookup"><span data-stu-id="776ed-129">Create a Web Application</span></span>

<span data-ttu-id="776ed-130">Najpierw należy utworzyć aplikacji sieci web, którego będziesz używać narzędzia Page Inspector za pomocą.</span><span class="sxs-lookup"><span data-stu-id="776ed-130">First, create a web application that you will use Page Inspector with.</span></span> <span data-ttu-id="776ed-131">W programie Visual Studio, wybierz **pliku** &gt; **nowy projekt**.</span><span class="sxs-lookup"><span data-stu-id="776ed-131">In Visual Studio, choose **File** &gt; **New Project**.</span></span> <span data-ttu-id="776ed-132">Po lewej stronie, rozwiń węzeł **Visual C#**, wybierz opcję **Web**, a następnie wybierz pozycję **aplikacji sieci Web programu ASP.NET MVC4**.</span><span class="sxs-lookup"><span data-stu-id="776ed-132">On the left, expand **Visual C#**, select **Web**, and then select **ASP.NET MVC4 Web Application**.</span></span>

![Nowa aplikacja platformy ASP.NET MVC](using-page-inspector-in-aspnet-mvc/_static/image2.png)

<span data-ttu-id="776ed-134">Kliknij przycisk **OK**.</span><span class="sxs-lookup"><span data-stu-id="776ed-134">Click **OK**.</span></span>

<span data-ttu-id="776ed-135">W **nowego projektu programu ASP.NET MVC 4** okno dialogowe, wybierz opcję **aplikacji internetowej**.</span><span class="sxs-lookup"><span data-stu-id="776ed-135">In the **New ASP.NET MVC 4 Project** dialog box, select **Internet Application**.</span></span> <span data-ttu-id="776ed-136">Pozostaw **Razor** jako domyślny aparat widoku.</span><span class="sxs-lookup"><span data-stu-id="776ed-136">Leave **Razor** as the default view engine.</span></span>

![Nowy projekt ASP.NET MVC — aplikacji internetowej](using-page-inspector-in-aspnet-mvc/_static/image4.png)

<span data-ttu-id="776ed-138">Aplikacja zostanie otwarta w **źródła** widoku.</span><span class="sxs-lookup"><span data-stu-id="776ed-138">The application opens in **Source** view.</span></span>

![Nowa aplikacja platformy ASP.NET MVC w widoku źródła](using-page-inspector-in-aspnet-mvc/_static/image6.png)

<span data-ttu-id="776ed-140">Teraz, gdy masz już aplikację, chcesz pracować, można użyć narzędzia Page Inspector do zbadania i zmodyfikowania go.</span><span class="sxs-lookup"><span data-stu-id="776ed-140">Now that you have an application to work with, you can use Page Inspector to examine and modify it.</span></span>

<a id="_starting_page_inspector"></a><a id="_3_using_page"></a>

## <a name="use-page-inspector-to-browse-to-a-view"></a><span data-ttu-id="776ed-141">Inspektor stron do przejdź do widoku</span><span class="sxs-lookup"><span data-stu-id="776ed-141">Use Page Inspector to Browse to a View</span></span>

<span data-ttu-id="776ed-142">W programie Visual Studio 2012, możesz kliknąć prawym przyciskiem myszy dowolny widok w projekcie, wybierz opcję **widoku w narzędzia Page Inspector**, i narzędzie Page Inspector będzie ustalenie trasy i wyświetlić stronę.</span><span class="sxs-lookup"><span data-stu-id="776ed-142">In Visual Studio 2012, you can right-click any view in your project, select **View in Page Inspector**, and Page Inspector will figure out the route and display the page.</span></span>

<span data-ttu-id="776ed-143">W **Eksploratora rozwiązań**, rozwiń węzeł **widoków** folder i następnie **Home** folderu.</span><span class="sxs-lookup"><span data-stu-id="776ed-143">In **Solution Explorer**, expand the **Views** folder and then the **Home** folder.</span></span> <span data-ttu-id="776ed-144">Plik Index.cshtml kliknij prawym przyciskiem myszy i wybierz polecenie **widoku w narzędzia Page Inspector**.</span><span class="sxs-lookup"><span data-stu-id="776ed-144">Right click the Index.cshtml file and choose **View in Page Inspector**.</span></span>

![Wyświetl Index.cshtml w narzędzia Page Inspector](using-page-inspector-in-aspnet-mvc/_static/image8.png)

<span data-ttu-id="776ed-146">Domyślnie narzędzie Page Inspector jest zadokowany jako okno po lewej stronie w środowisku Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="776ed-146">By default, Page Inspector is docked as a window on the left side of the Visual Studio environment.</span></span> <span data-ttu-id="776ed-147">Jeśli wolisz, możesz go zadokować w innym miejscu lub oddokować okno.</span><span class="sxs-lookup"><span data-stu-id="776ed-147">If you prefer, you can dock it elsewhere, or undock the window.</span></span> <span data-ttu-id="776ed-148">Zobacz [porady: Aranżowanie i dokowanie Windows](https://msdn.microsoft.com/library/z4y0hsax.aspx).</span><span class="sxs-lookup"><span data-stu-id="776ed-148">See [How to: Arrange and Dock Windows](https://msdn.microsoft.com/library/z4y0hsax.aspx).</span></span>

<span data-ttu-id="776ed-149">Górne okienko okna narzędzia Page Inspector pokazuje bieżącej strony w oknie przeglądarki.</span><span class="sxs-lookup"><span data-stu-id="776ed-149">The top pane of the Page Inspector window shows the current page in a browser window.</span></span> <span data-ttu-id="776ed-150">Dolne okienko wyświetla stronę w kod znaczników HTML, wraz z niektórych kart, które pozwalają przeprowadzać inspekcje różne aspekty strony.</span><span class="sxs-lookup"><span data-stu-id="776ed-150">The bottom pane shows the page in HTML markup, along with some tabs that let you inspect different aspects of the page.</span></span> <span data-ttu-id="776ed-151">Dolne okienko jest podobny do [narzędzi deweloperskich F12](https://msdn.microsoft.com/ie/aa740478) w programie Internet Explorer.</span><span class="sxs-lookup"><span data-stu-id="776ed-151">The bottom pane is similar to the [F12 Developer Tools](https://msdn.microsoft.com/ie/aa740478) in Internet Explorer.</span></span>

![Aplikacja platformy ASP.NET MVC w narzędzia Page Inspector](using-page-inspector-in-aspnet-mvc/_static/image10.png)

<span data-ttu-id="776ed-153">W tym samouczku użyjesz **HTML** i **style** karty, aby szybko i wprowadzać zmiany w aplikacji.</span><span class="sxs-lookup"><span data-stu-id="776ed-153">In this tutorial, you will use the **HTML** and **Styles** tabs to navigate quickly and make changes to the application.</span></span>

<a id="_examining_(&quot;decomposing&quot;)_the"></a><a id="_inspection_mode_and"></a><a id="_4_inspection_mode"></a>

## <a name="enableinspection-mode"></a><span data-ttu-id="776ed-154">Tryb EnableInspection</span><span class="sxs-lookup"><span data-stu-id="776ed-154">EnableInspection Mode</span></span>

<span data-ttu-id="776ed-155">Aby ustawić narzędzia Page Inspector trybu inspekcji, kliknij przycisk **Sprawdź** przycisku.</span><span class="sxs-lookup"><span data-stu-id="776ed-155">To put Page Inspector into Inspection Mode, click the **Inspect** button.</span></span> <span data-ttu-id="776ed-156">W trybie inspekcji Jeśli przytrzymasz wskaźnik myszy nad dowolną część renderowanej strony odpowiedni kod źródłowy lub kod zostanie wyróżniona.</span><span class="sxs-lookup"><span data-stu-id="776ed-156">In Inspection Mode, when you hold the mouse pointer over any part of the rendered page, the corresponding source markup or code is highlighted.</span></span>

![Przełącz tryb inspekcji](using-page-inspector-in-aspnet-mvc/_static/image12.png)

<span data-ttu-id="776ed-158">Teraz umieść wskaźnik myszy nad różnych części strony, w ramach narzędzia Page Inspector.</span><span class="sxs-lookup"><span data-stu-id="776ed-158">Now move your mouse over different parts of the page within Page Inspector.</span></span> <span data-ttu-id="776ed-159">To zrobisz, kursor zmienia się na znak plus w dużych, a jest wyróżniony element poniżej:</span><span class="sxs-lookup"><span data-stu-id="776ed-159">As you do, the mouse pointer changes to a large plus sign, and the element underneath is highlighted:</span></span>

![Kursor div.content otoki](using-page-inspector-in-aspnet-mvc/_static/image14.png)

<span data-ttu-id="776ed-161">Podczas przesuwania wskaźnika myszy, Visual Studio wyróżnia odpowiedniej składni Razor w pliku źródłowym.</span><span class="sxs-lookup"><span data-stu-id="776ed-161">As you move the mouse pointer, Visual Studio highlights the corresponding Razor syntax in the source file.</span></span> <span data-ttu-id="776ed-162">Jeśli HTML element pochodzi z innego pliku źródłowego, Visual Studio automatycznie otwiera plik.</span><span class="sxs-lookup"><span data-stu-id="776ed-162">If the HTML element comes from another source file, Visual Studio automatically opens the file.</span></span>

<span data-ttu-id="776ed-163">W narzędzia Page Inspector **HTML** karta przedstawia HTML, który został wygenerowany z użyciem składni Razor.</span><span class="sxs-lookup"><span data-stu-id="776ed-163">In Page Inspector, the **HTML** tab shows the HTML that was generated from the Razor syntax.</span></span> <span data-ttu-id="776ed-164">Podczas przesuwania wskaźnika myszy, są wyróżnione elementów HTML.</span><span class="sxs-lookup"><span data-stu-id="776ed-164">As you move the mouse pointer, the HTML elements are highlighted.</span></span> <span data-ttu-id="776ed-165">**Style** karta zawiera reguły CSS dla elementu.</span><span class="sxs-lookup"><span data-stu-id="776ed-165">The **Styles** tab shows the CSS rules for the element.</span></span>

<a id="_5_using_page"></a>

## <a name="use-page-inspector-to-make-changes-to-markup"></a><span data-ttu-id="776ed-166">Inspektor stron umożliwia wprowadzać zmiany znaczników</span><span class="sxs-lookup"><span data-stu-id="776ed-166">Use Page Inspector to Make Changes to Markup</span></span>

<span data-ttu-id="776ed-167">Narzędzie Page Inspector umożliwia znajdowanie znaczników, którego lokalizacja może nie być oczywista.</span><span class="sxs-lookup"><span data-stu-id="776ed-167">Page Inspector lets you find markup whose location might not be obvious.</span></span> <span data-ttu-id="776ed-168">Następnie można zmodyfikować znaczników i wyświetlić wynikowe zmiany.</span><span class="sxs-lookup"><span data-stu-id="776ed-168">Then you can modify the markup and see the resulting changes.</span></span>

<span data-ttu-id="776ed-169">Aby to zobaczyć, kliknij przycisk **Sprawdź** a następnie przewiń do dołu strony, w oknie narzędzia Page Inspector.</span><span class="sxs-lookup"><span data-stu-id="776ed-169">To see this, click **Inspect** and then scroll to the bottom of the page in the Page Inspector window.</span></span>

<span data-ttu-id="776ed-170">Gdy przesuniesz wskaźnik myszy w obszarze stopki, zostanie otwarte narzędzie Page Inspector \_plik Layout.cshtml i wyróżnienie części strony układu, który wybrano.</span><span class="sxs-lookup"><span data-stu-id="776ed-170">When you move the mouse pointer into the footer area, Page Inspector opens the \_Layout.cshtml file and highlights the section of the layout page that you have selected.</span></span> <span data-ttu-id="776ed-171">Jak widać, czy stopka jest zdefiniowana w pliku układu, a nie samego widoku.</span><span class="sxs-lookup"><span data-stu-id="776ed-171">As you can see, the footer are is defined in the layout file, and not the view itself.</span></span>

![Stopka](using-page-inspector-in-aspnet-mvc/_static/image16.png)

<span data-ttu-id="776ed-173">Teraz umieść wskaźnik myszy nad wierszem o prawach autorskich <a id="a"> </a>Zwróć uwagę.</span><span class="sxs-lookup"><span data-stu-id="776ed-173">Now move your mouse pointer over the line with the copyright <a id="a"></a>notice.</span></span> <span data-ttu-id="776ed-174">W \_Layout.cshtml stronie odpowiednim wierszu zostanie wyróżniony.</span><span class="sxs-lookup"><span data-stu-id="776ed-174">In the \_Layout.cshtml page, the corresponding line is highlighted.</span></span>

![Stopka wiersz o prawach autorskich wyróżniony](using-page-inspector-in-aspnet-mvc/_static/image18.png)

<span data-ttu-id="776ed-176">Dodaj jakiś tekst do końca wiersza w \_plik Layout.cshtml.</span><span class="sxs-lookup"><span data-stu-id="776ed-176">Add some text to the end of the line in the \_Layout.cshtml file.</span></span>

<span data-ttu-id="776ed-177">&lt;p&gt;&amp;kopiowania; @DateTime.Now.Year — Moja aplikacja platformy ASP.NET MVC zmieni! &lt;/p&gt;</span><span class="sxs-lookup"><span data-stu-id="776ed-177">&lt;p&gt;&amp;copy; @DateTime.Now.Year - My ASP.NET MVC Application Rocks!&lt;/p&gt;</span></span>

<span data-ttu-id="776ed-178">Teraz naciśnij klawisze Ctrl + Alt + Enter lub kliknij pasek aktualizacji, aby wyświetlić wyniki w oknie przeglądarki, narzędzie Page Inspector.</span><span class="sxs-lookup"><span data-stu-id="776ed-178">Now, press Ctrl+Alt+Enter or click the Update Bar to see the results in the Page Inspector browser window.</span></span>

![Moje ASP.NET aplikacji Rocks!](using-page-inspector-in-aspnet-mvc/_static/image20.png)

<span data-ttu-id="776ed-180">Może mieć uważasz, czy stopka zdefiniowany w Index.cshtml, ale okazała się on znajdować się w \_Layout.cshtml i narzędzie Page Inspector odnaleziona automatycznie.</span><span class="sxs-lookup"><span data-stu-id="776ed-180">You might have thought that the footer defined in Index.cshtml, but it turned out to be in the \_Layout.cshtml, and Page Inspector found it for you.</span></span>

<a id="_inspection_mode_and_1"></a><a id="_6_inspection_mode"></a>

## <a name="inspection-mode-and-the-html-window"></a><span data-ttu-id="776ed-181">Tryb inspekcji i w oknie kodu HTML</span><span class="sxs-lookup"><span data-stu-id="776ed-181">Inspection Mode and the HTML Window</span></span>

<span data-ttu-id="776ed-182">Następnie będziesz mieć rzut oka na oknie HTML oraz sposób mapowania elementów dla Ciebie.</span><span class="sxs-lookup"><span data-stu-id="776ed-182">Next, you will have a quick look at the HTML window and how it maps elements for you.</span></span>

<span data-ttu-id="776ed-183">Kliknij przycisk **Sprawdź** umieścić narzędzie Page Inspector w trybie inspekcji.</span><span class="sxs-lookup"><span data-stu-id="776ed-183">Click **Inspect** to put Page Inspector in Inspection Mode.</span></span>

<span data-ttu-id="776ed-184">Kliknij w górnej części strony, który jest wyświetlany komunikat "logohere".</span><span class="sxs-lookup"><span data-stu-id="776ed-184">Click the top part of the page that says "Your logohere".</span></span> <span data-ttu-id="776ed-185">To badanie konkretnego elementu bardziej szczegółowo, aby wyświetlana w oknie przeglądarki nie jest już zmienia się po przesunięciu wskaźnika myszy.</span><span class="sxs-lookup"><span data-stu-id="776ed-185">You are examining a particular element in more detail, so the display in the browser window no longer changes as you move the mouse pointer.</span></span>

<span data-ttu-id="776ed-186">Teraz umieść kursor myszy na **HTML** okna.</span><span class="sxs-lookup"><span data-stu-id="776ed-186">Now move the mouse pointer to the **HTML** window.</span></span> <span data-ttu-id="776ed-187">Podczas przesuwania wskaźnika myszy, narzędzie Page Inspector przedstawia element w obrębie **HTML** okna i odpowiadający mu element w oknie przeglądarki.</span><span class="sxs-lookup"><span data-stu-id="776ed-187">As you move the mouse pointer, Page Inspector outlines the element within the **HTML** window and highlights the corresponding element in the browser window.</span></span>

![Okno w formacie HTML](using-page-inspector-in-aspnet-mvc/_static/image22.png)

<span data-ttu-id="776ed-189">Jak wcześniej, zostanie otwarte narzędzie Page Inspector \_plik Layout.cshtml dla Ciebie w karcie tymczasowej. Kliknij przycisk \_Layout.cshtml tymczasowe kartę, a odpowiedni kod znaczników zostaną wyróżnione w &lt;nagłówka&gt; sekcji możesz:</span><span class="sxs-lookup"><span data-stu-id="776ed-189">As before, Page Inspector opens the \_Layout.cshtml file for you in a temporary tab. Click the \_Layout.cshtml temporary tab, and the corresponding markup will be highlighted in the &lt;header&gt; section for you:</span></span>

![Wyróżnione znaczników](using-page-inspector-in-aspnet-mvc/_static/image24.png)

<a id="_using_page_inspector"></a><a id="_7_previewing_css"></a>

## <a name="preview-css-changes-in-the-styles-window"></a><span data-ttu-id="776ed-191">Zmiany arkusza CSS (wersja zapoznawcza) w oknie style</span><span class="sxs-lookup"><span data-stu-id="776ed-191">Preview CSS Changes in the Styles Window</span></span>

<span data-ttu-id="776ed-192">Następnie, użyjesz narzędzia Page Inspector **style** okna, aby wyświetlić podgląd zmian do arkusza CSS.</span><span class="sxs-lookup"><span data-stu-id="776ed-192">Next, you will use the Page Inspector **Styles** window to preview changes to CSS.</span></span>

<span data-ttu-id="776ed-193">Kliknij przycisk **Sprawdź** umieścić narzędzie Page Inspector w trybie inspekcji.</span><span class="sxs-lookup"><span data-stu-id="776ed-193">Click **Inspect** to put Page Inspector in Inspection Mode.</span></span>

<span data-ttu-id="776ed-194">W oknie przeglądarki, narzędzie Page Inspector, umieść kursor myszy na sekcję "Home Page" do momentu **div.content otoki** etykieta jest wyświetlana.</span><span class="sxs-lookup"><span data-stu-id="776ed-194">In the Page Inspector browser window, move the mouse pointer over the "Home Page" section until the **div.content-wrapper** label appears.</span></span>

![Kursor div.content otoki](using-page-inspector-in-aspnet-mvc/_static/image26.png)

<span data-ttu-id="776ed-196">Kliknij wewnątrz sekcji otoki div.content jeden raz, a następnie przesuń wskaźnik myszy na **style** okna.</span><span class="sxs-lookup"><span data-stu-id="776ed-196">Click within the div.content-wrapper section once, and then move the mouse pointer to the **Styles** window.</span></span> <span data-ttu-id="776ed-197">**— Style** okno pokazuje wszystkie reguły CSS dla tego elementu.</span><span class="sxs-lookup"><span data-stu-id="776ed-197">The **Syles** window shows all of the CSS rules for this element.</span></span> <span data-ttu-id="776ed-198">Przewiń w dół do selektora klasy Znajdź .featured .content otoki.</span><span class="sxs-lookup"><span data-stu-id="776ed-198">Scroll down to find the .featured .content-wrapper class selector.</span></span> <span data-ttu-id="776ed-199">Teraz usuń zaznaczenie pola wyboru dla właściwości kolor tła.</span><span class="sxs-lookup"><span data-stu-id="776ed-199">Now clear the checkbox for the background-color property.</span></span>

![Kolor tła wyczyść](using-page-inspector-in-aspnet-mvc/_static/image28.png)

<span data-ttu-id="776ed-201">Zwróć uwagę, jak zmiany zawiera wersje zapoznawcze natychmiast w oknie przeglądarki, narzędzie Page Inspector.</span><span class="sxs-lookup"><span data-stu-id="776ed-201">Notice how the change previews instantly in the Page Inspector browser window.</span></span>

<span data-ttu-id="776ed-202">Ponownie zaznacz pole wyboru, a następnie kliknij dwukrotnie wartość właściwości i zmienić ją na czerwono.</span><span class="sxs-lookup"><span data-stu-id="776ed-202">Select the checkbox again, then double-click the property value and change it to red.</span></span> <span data-ttu-id="776ed-203">Zmiana pokazuje natychmiast:</span><span class="sxs-lookup"><span data-stu-id="776ed-203">The change shows immediately:</span></span>

![Kolor tła czerwony](using-page-inspector-in-aspnet-mvc/_static/image30.png)

<span data-ttu-id="776ed-205">**Style** sprawia, że okna ułatwiają przetestuj i Wyświetl podgląd CSS zmian, zanim zatwierdzenia zmian w stylu arkusz sam.</span><span class="sxs-lookup"><span data-stu-id="776ed-205">The **Styles** window makes it easy to test and preview CSS changes before you commit the changes to the style sheet itself.</span></span>

<a id="css_auto_sync"></a>
## <a name="css-auto-sync"></a><span data-ttu-id="776ed-206">Automatyczna synchronizacja z CSS</span><span class="sxs-lookup"><span data-stu-id="776ed-206">CSS Auto Sync</span></span>

> [!NOTE]
> <span data-ttu-id="776ed-207">Ta funkcja wymaga w wersji 1.3 narzędzia Page Inspector.</span><span class="sxs-lookup"><span data-stu-id="776ed-207">This feature requires version 1.3 of Page Inspector.</span></span>


<span data-ttu-id="776ed-208">Funkcja automatycznej synchronizacji CSS służy do bezpośredniego edytowania pliku CSS i zobaczyć zmiany bezpośrednio w przeglądarce narzędzia Page Inspector.</span><span class="sxs-lookup"><span data-stu-id="776ed-208">The CSS Auto-Sync feature allows you to edit a CSS file directly, and see the changes immediately in the Page Inspector browser.</span></span>

<span data-ttu-id="776ed-209">Kliknij przycisk **Sprawdź** umieścić narzędzie Page Inspector w trybie inspekcji.</span><span class="sxs-lookup"><span data-stu-id="776ed-209">Click **Inspect** to put Page Inspector in Inspection Mode.</span></span>

<span data-ttu-id="776ed-210">W przeglądarce narzędzia Page Inspector, umieść kursor myszy na sekcję "Home Page" do momentu **div.content otoki** etykieta jest wyświetlana.</span><span class="sxs-lookup"><span data-stu-id="776ed-210">In the Page Inspector browser, move the mouse pointer over the "Home Page" section until the **div.content-wrapper** label appears.</span></span> <span data-ttu-id="776ed-211">Kliknij raz, aby wybrać ten element.</span><span class="sxs-lookup"><span data-stu-id="776ed-211">Click once to select this element.</span></span>

<span data-ttu-id="776ed-212">**— Style** okno pokazuje wszystkie reguły CSS dla tego elementu.</span><span class="sxs-lookup"><span data-stu-id="776ed-212">The **Syles** window shows all of the CSS rules for this element.</span></span> <span data-ttu-id="776ed-213">Przewiń w dół do selektora klasy Znajdź .featured .content otoki.</span><span class="sxs-lookup"><span data-stu-id="776ed-213">Scroll down to find the .featured .content-wrapper class selector.</span></span> <span data-ttu-id="776ed-214">Kliknij pozycję "opakowanie .content .featured".</span><span class="sxs-lookup"><span data-stu-id="776ed-214">Click on ".featured .content-wrapper".</span></span> <span data-ttu-id="776ed-215">Narzędzie Page Inspector otwiera się w pliku CSS, która definiuje ten styl (Site.css) i zaznacza odpowiednie style CSS.</span><span class="sxs-lookup"><span data-stu-id="776ed-215">Page Inspector opens the CSS file that defines this style (Site.css) and highlights the corresponding CSS style.</span></span>

![](using-page-inspector-in-aspnet-mvc/_static/image32.png)

<span data-ttu-id="776ed-216">Teraz Zmień wartość `background-color` do "red".</span><span class="sxs-lookup"><span data-stu-id="776ed-216">Now change the value for `background-color` to "red".</span></span> <span data-ttu-id="776ed-217">Zmiana pojawi się natychmiast w przeglądarce narzędzia Page Inspector.</span><span class="sxs-lookup"><span data-stu-id="776ed-217">The change appears immediately in the Page Inspector browser.</span></span>

![](using-page-inspector-in-aspnet-mvc/_static/image34.png)

<a id="css_color_picker"></a>
## <a name="using-the-css-color-picker"></a><span data-ttu-id="776ed-218">Za pomocą selektora kolorów CSS</span><span class="sxs-lookup"><span data-stu-id="776ed-218">Using the CSS Color Picker</span></span>

<span data-ttu-id="776ed-219">Edytor CSS w programie Visual Studio 2012 zawiera selektor kolorów, która pozwala łatwo wybrać i Wstaw kolorów.</span><span class="sxs-lookup"><span data-stu-id="776ed-219">The CSS editor in Visual Studio 2012 has a color picker that makes it easy to choose and insert colors.</span></span> <span data-ttu-id="776ed-220">Selektor kolorów zawiera standardowe paletę kolorów, obsługuje nazwy kolor standardowy, kody skrótów, kolorów RGB, RGBA, HSL i HSLA i utrzymuje listę kolorów, które zostały ostatnio użyte w dokumencie.</span><span class="sxs-lookup"><span data-stu-id="776ed-220">The color picker includes a standard palette of colors, supports standard color names, hash codes, RGB, RGBA, HSL, and HSLA colors, and maintains a list of the colors you've used most recently in the document.</span></span>

<span data-ttu-id="776ed-221">W poprzedniej sekcji, możesz zmienić wartość `background-color` właściwości.</span><span class="sxs-lookup"><span data-stu-id="776ed-221">In the previous section, you changed the value of the `background-color` property.</span></span> <span data-ttu-id="776ed-222">Aby wywołać selektor kolorów, umieść kursor po danej nazwy właściwości oraz typ **#** lub **rgb (**.</span><span class="sxs-lookup"><span data-stu-id="776ed-222">To invoke the color picker, place the insertion point after the property name and type **#** or **rgb(**.</span></span>

![Pasek wyboru koloru CSS](using-page-inspector-in-aspnet-mvc/_static/image36.png)

<span data-ttu-id="776ed-224">Kliknij kolor, aby go zaznaczyć, lub naciśnij klawisz strzałki w dół, a następnie użyj klawiszy strzałek w lewo i w prawo do przechodzenia kolorów.</span><span class="sxs-lookup"><span data-stu-id="776ed-224">Click on a color to select it, or press the down arrow key and then use the left and right arrow keys to traverse the colors.</span></span> <span data-ttu-id="776ed-225">Gdy użytkownik odwiedza kolor, jest przeglądany odpowiadająca wartość szesnastkowa:</span><span class="sxs-lookup"><span data-stu-id="776ed-225">When you visit a color, the corresponding hex value is previewed:</span></span>

![wartość właściwości kolor tła podglądu](using-page-inspector-in-aspnet-mvc/_static/image38.png)

<span data-ttu-id="776ed-227">Jeśli pasek koloru nie ma dokładnie odpowiedni kolor, można użyć selektora kolorów w dół pop.</span><span class="sxs-lookup"><span data-stu-id="776ed-227">If the color bar doesn't have the exact color you want, you can use the color picker pop-down.</span></span> <span data-ttu-id="776ed-228">Aby go otworzyć, kliknij cudzysłów ostrokątny double na prawym końcu paska koloru, lub jeden lub dwa razy klawisz Strzałka w dół na klawiaturze.</span><span class="sxs-lookup"><span data-stu-id="776ed-228">To open it, click the double chevron at the right end of the color bar, or press the Down Arrow once or twice on the keyboard.</span></span>

![Selektor koloru CSS Pop — szczegółów](using-page-inspector-in-aspnet-mvc/_static/image40.png)

<span data-ttu-id="776ed-230">Kliknij przycisk koloru z pionowy pasek po prawej stronie.</span><span class="sxs-lookup"><span data-stu-id="776ed-230">Click a color from the vertical bar on the right.</span></span> <span data-ttu-id="776ed-231">W oknie głównym ten pokazuje gradientu dla tego koloru.</span><span class="sxs-lookup"><span data-stu-id="776ed-231">This shows a gradient for that color in the main window.</span></span> <span data-ttu-id="776ed-232">Wybierz kolor, bezpośrednio z kreska pionowa, naciskając klawisz Enter lub kliknij dowolnego punktu w głównym oknie, aby wybrać z większą dokładnością.</span><span class="sxs-lookup"><span data-stu-id="776ed-232">Choose a color directly from the vertical bar by pressing Enter, or click any point in the main window to choose with greater precision.</span></span>

<span data-ttu-id="776ed-233">Jeśli jest kolorem na ekranie komputera, którego chcesz używać (nie musi być w interfejsie użytkownika programu Visual Studio), jego wartość można przechwycić za pomocą narzędzia służącego do pobierania w prawym dolnym rogu.</span><span class="sxs-lookup"><span data-stu-id="776ed-233">If there is a color on your computer screen that you want to use (it doesn't have to be inside the Visual Studio user interface), you can capture its value by using the eyedropper tool on the lower right.</span></span>

<span data-ttu-id="776ed-234">Możesz również zmienić przezroczystość koloru za pomocą suwaka u dołu selektora kolorów.</span><span class="sxs-lookup"><span data-stu-id="776ed-234">You also can change the opacity of a color by moving the slider at the bottom of the color picker.</span></span> <span data-ttu-id="776ed-235">Robi zmiany kolor wartości do wartości RGBA, ponieważ RGBA format może reprezentować nieprzezroczystości.</span><span class="sxs-lookup"><span data-stu-id="776ed-235">Doing so changes color values to RGBA values, because the RGBA format can represent opacity.</span></span>

<span data-ttu-id="776ed-236">Po wybraniu koloru, naciśnij klawisz Enter, a następnie wpisz średnikiem, aby ukończyć kolor tła wejścia w *Site.css* pliku.</span><span class="sxs-lookup"><span data-stu-id="776ed-236">After you have chosen a color, press Enter, and then type a semicolon to complete the background-color entry in the *Site.css* file.</span></span>

<a id="_the_update_bar"></a>

### <a name="the-page-inspector-update-bar"></a><span data-ttu-id="776ed-237">Na pasku aktualizacji Inspektor strony</span><span class="sxs-lookup"><span data-stu-id="776ed-237">The Page Inspector Update Bar</span></span>

<span data-ttu-id="776ed-238">Narzędzie Page Inspector natychmiast wykrywa zmiany *Site.css* pliku i wyświetla alert w pasku aktualizacji.</span><span class="sxs-lookup"><span data-stu-id="776ed-238">Page Inspector immediately detects the change to the *Site.css* file and displays an alert in an update bar.</span></span>

![Pasek aktualizacji](using-page-inspector-in-aspnet-mvc/_static/image42.png)

<span data-ttu-id="776ed-240">Aby zapisać wszystkie pliki i odświeżyć przeglądarkę, narzędzie Page Inspector, naciśnij klawisze Ctrl + Alt + Enter lub kliknij przycisk na pasku aktualizacji.</span><span class="sxs-lookup"><span data-stu-id="776ed-240">To save all your files and refresh the Page Inspector browser, press Ctrl+Alt+Enter or click the update bar.</span></span> <span data-ttu-id="776ed-241">Zmień kolor wyróżnienia pojawia się w przeglądarce.</span><span class="sxs-lookup"><span data-stu-id="776ed-241">The change in the highlight color appears in the browser.</span></span>

<a id="map_dynamic_elements"></a>
## <a name="mapping-dynamic-page-elements-to-javascript"></a><span data-ttu-id="776ed-242">Mapowanie elementów strony dynamicznej języka JavaScript</span><span class="sxs-lookup"><span data-stu-id="776ed-242">Mapping Dynamic Page Elements to JavaScript</span></span>

<span data-ttu-id="776ed-243">W nowoczesnych aplikacji sieci web elementy na stronie są często generowane dynamicznie przy użyciu języka JavaScript.</span><span class="sxs-lookup"><span data-stu-id="776ed-243">In modern web applications, elements in the page are often generated dynamically with JavaScript.</span></span> <span data-ttu-id="776ed-244">Oznacza to, że nie ma żadnych znaczników statyczne (HTML lub Razor), która odnosi się do tych elementów strony.</span><span class="sxs-lookup"><span data-stu-id="776ed-244">That means there is no static markup (either HTML or Razor) that corresponds to these page elements.</span></span>

<span data-ttu-id="776ed-245">W wersji 1.3 narzędzie Page Inspector można teraz Mapowanie elementów, które zostały dynamicznie dodawane do strony do odpowiedniego kodu JavaScript.</span><span class="sxs-lookup"><span data-stu-id="776ed-245">With version 1.3, Page Inspector can now map items that were dynamically added to the page back to the corresponding JavaScript code.</span></span> <span data-ttu-id="776ed-246">Aby zademonstrować tę funkcję, użyjemy [szablonu jednej strony aplikacji (SPA)](../../../single-page-application/overview/introduction/knockoutjs-template.md).</span><span class="sxs-lookup"><span data-stu-id="776ed-246">To demonstrate this feature, we'll use the [Single Page Application (SPA) template](../../../single-page-application/overview/introduction/knockoutjs-template.md).</span></span>

> [!NOTE]
> <span data-ttu-id="776ed-247">Szablon SPA wymaga [platformy ASP.NET i Web Tools 2012.2](https://go.microsoft.com/fwlink/?LinkId=282650) aktualizacji.</span><span class="sxs-lookup"><span data-stu-id="776ed-247">The SPA template requires the [ASP.NET and Web Tools 2012.2](https://go.microsoft.com/fwlink/?LinkId=282650) update.</span></span>


<span data-ttu-id="776ed-248">W programie Visual Studio, wybierz **pliku** &gt; **nowy projekt**.</span><span class="sxs-lookup"><span data-stu-id="776ed-248">In Visual Studio, choose **File** &gt; **New Project**.</span></span> <span data-ttu-id="776ed-249">Po lewej stronie, rozwiń węzeł **Visual C#**, wybierz opcję **Web**, a następnie wybierz pozycję **aplikacji sieci Web programu ASP.NET MVC4**.</span><span class="sxs-lookup"><span data-stu-id="776ed-249">On the left, expand **Visual C#**, select **Web**, and then select **ASP.NET MVC4 Web Application**.</span></span> <span data-ttu-id="776ed-250">Kliknij przycisk **OK**.</span><span class="sxs-lookup"><span data-stu-id="776ed-250">Click **OK**.</span></span>

<span data-ttu-id="776ed-251">W **nowego projektu programu ASP.NET MVC 4** okno dialogowe, wybierz opcję **aplikacji jednostronicowej**.</span><span class="sxs-lookup"><span data-stu-id="776ed-251">In the **New ASP.NET MVC 4 Project** dialog, select **Single Page Application**.</span></span>

<span data-ttu-id="776ed-252">W Eksploratorze rozwiązań rozwiń **widoków** folder i następnie **Home** folderu.</span><span class="sxs-lookup"><span data-stu-id="776ed-252">In Solution Explorer, expand the **Views** folder and then the **Home** folder.</span></span> <span data-ttu-id="776ed-253">Plik Index.cshtml kliknij prawym przyciskiem myszy i wybierz polecenie **widoku w narzędzia Page Inspector**.</span><span class="sxs-lookup"><span data-stu-id="776ed-253">Right click the Index.cshtml file and choose **View in Page Inspector**.</span></span>

<span data-ttu-id="776ed-254">Po pierwsze, czyli wyświetlonej w przeglądarce narzędzia Page Inspector jest strona logowania.</span><span class="sxs-lookup"><span data-stu-id="776ed-254">The first thing that is displayed in the Page Inspector browser is a login page.</span></span> <span data-ttu-id="776ed-255">Kliknij przycisk "Zarejestruj", a następnie utwórz nazwę użytkownika i hasło.</span><span class="sxs-lookup"><span data-stu-id="776ed-255">Click "Sign Up" and create a user name and password.</span></span> <span data-ttu-id="776ed-256">Po zarejestrowaniu się aplikacji loguje się użytkownik i tworzy listę zadań do wykonania z niektórymi elementami próbki.</span><span class="sxs-lookup"><span data-stu-id="776ed-256">Once you sign up, the application logs you in and creates a to-do list with some sample items.</span></span>

<span data-ttu-id="776ed-257">Kliknij przycisk **Sprawdź** umieścić narzędzie Page Inspector w trybie inspekcji.</span><span class="sxs-lookup"><span data-stu-id="776ed-257">Click **Inspect** to put Page Inspector in Inspection Mode.</span></span> <span data-ttu-id="776ed-258">W przeglądarce narzędzia Page Inspector kliknij jeden z elementów do wykonania.</span><span class="sxs-lookup"><span data-stu-id="776ed-258">In the Page Inspector browser, click on one of the to-do items.</span></span> <span data-ttu-id="776ed-259">Należy zauważyć, że zamiast są wyróżnione na niebiesko, elementu jest wyróżniony kolorem pomarańczowym za pomocą "JS" obok nazwy elementu.</span><span class="sxs-lookup"><span data-stu-id="776ed-259">Notice that instead of being highlighted in blue, the element is highlighted in orange, with "JS" next to the element name.</span></span> <span data-ttu-id="776ed-260">Oznacza to, że element został utworzony dynamicznie za pośrednictwem skryptu.</span><span class="sxs-lookup"><span data-stu-id="776ed-260">This indicates that the element was created dynamically through script.</span></span>

![](using-page-inspector-in-aspnet-mvc/_static/image44.png)

<span data-ttu-id="776ed-261">Ponadto pomarańczowy podkreślenie pojawia się na **stos wywołań** kartę. Oznacza to, że **stos wywołań** okienko zawiera więcej informacji na temat elementu.</span><span class="sxs-lookup"><span data-stu-id="776ed-261">In addition, an orange underline appears on the **Call Stack** tab. This indicates that the **Call Stack** pane has more information about the element.</span></span>

<span data-ttu-id="776ed-262">Kliknij pozycję **stos wywołań** kartę. **Stos wywołań** okienko zawiera stos wywołań dla wywołania języka JavaScript, utworzony element.</span><span class="sxs-lookup"><span data-stu-id="776ed-262">Click on the **Call Stack** tab. The **Call Stack** pane shows the call stack for the JavaScript call that created the element.</span></span> <span data-ttu-id="776ed-263">Wywołuje w zewnętrznych bibliotekach np. jQuery są zwinięte, dzięki czemu można łatwo zobaczyć, wywołuje skrypt aplikacji.</span><span class="sxs-lookup"><span data-stu-id="776ed-263">Calls to external libraries such as jQuery are collapsed, so that you can easily see the calls to your application script.</span></span>

![](using-page-inspector-in-aspnet-mvc/_static/image46.png)

<span data-ttu-id="776ed-264">Aby wyświetlić pełny stos, w tym wywołania zewnętrznych bibliotek można rozwinąć węzły z etykietą "Biblioteki zewnętrznej":</span><span class="sxs-lookup"><span data-stu-id="776ed-264">To see the full stack, including calls to external libraries, you can expand the nodes labeled "External Libraries":</span></span>

![](using-page-inspector-in-aspnet-mvc/_static/image48.png)

<span data-ttu-id="776ed-265">Kliknięcie elementu w stosie wywołań programu Visual Studio otwiera plik kodu i wyróżnienie odpowiadający jej skrypt.</span><span class="sxs-lookup"><span data-stu-id="776ed-265">If you click an item in the call stack, Visual Studio opens the code file and highlights the corresponding script.</span></span>

![](using-page-inspector-in-aspnet-mvc/_static/image50.png)

## <a name="see-also"></a><span data-ttu-id="776ed-266">Zobacz też</span><span class="sxs-lookup"><span data-stu-id="776ed-266">See Also</span></span>

<span data-ttu-id="776ed-267">[Wprowadzenie do platformy ASP.NET MVC 4 w programie Visual Studio](../older-versions/getting-started-with-aspnet-mvc4/intro-to-aspnet-mvc-4.md) (witryny sieci Web platformy ASP.net)</span><span class="sxs-lookup"><span data-stu-id="776ed-267">[Intro to ASP.NET MVC 4 with Visual Studio](../older-versions/getting-started-with-aspnet-mvc4/intro-to-aspnet-mvc-4.md) (ASP.net website)</span></span>

<span data-ttu-id="776ed-268">[Wprowadzenie do narzędzia Page Inspector](https://channel9.msdn.com/posts/visual-studio-vnext-introducing-page-inspector/) (wideo Channel 9)</span><span class="sxs-lookup"><span data-stu-id="776ed-268">[Introducing Page Inspector](https://channel9.msdn.com/posts/visual-studio-vnext-introducing-page-inspector/) (Channel 9 video)</span></span>

<span data-ttu-id="776ed-269">[Komunikaty o błędach narzędzia Page Inspector](https://go.microsoft.com/?linkid=9813062) (MSDN)</span><span class="sxs-lookup"><span data-stu-id="776ed-269">[Page Inspector Error Messages](https://go.microsoft.com/?linkid=9813062) (MSDN)</span></span>
