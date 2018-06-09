---
uid: mvc/overview/views/using-page-inspector-in-aspnet-mvc
title: W programie ASP.NET MVC przy użyciu narzędzia Page Inspector | Dokumentacja firmy Microsoft
author: rick-anderson
description: Narzędzie Page Inspector w programie Visual Studio 2012 to narzędzie do projektowania sieci web za pomocą zintegrowanego przeglądarki. Wybierz dowolny element w zintegrowanej przeglądarki i narzędzie Page Inspector i...
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/15/2012
ms.topic: article
ms.assetid: c7e4e1ab-4932-4614-9f53-aaf7c706d498
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/views/using-page-inspector-in-aspnet-mvc
msc.type: authoredcontent
ms.openlocfilehash: 5b443963a089f96a9dab11b7db4a25451075d6be
ms.sourcegitcommit: 6784510cfb589308c3875ccb5113eb31031766b4
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/08/2018
ms.locfileid: "28034519"
---
<a name="using-page-inspector-in-aspnet-mvc"></a><span data-ttu-id="b6e7d-104">Za pomocą narzędzia Page Inspector na platformie ASP.NET MVC</span><span class="sxs-lookup"><span data-stu-id="b6e7d-104">Using Page Inspector in ASP.NET MVC</span></span>
====================
<span data-ttu-id="b6e7d-105">przez Ammann Timowi</span><span class="sxs-lookup"><span data-stu-id="b6e7d-105">by Tim Ammann</span></span>

> <span data-ttu-id="b6e7d-106">Narzędzie Page Inspector w programie Visual Studio 2012 to narzędzie do projektowania sieci web za pomocą zintegrowanego przeglądarki.</span><span class="sxs-lookup"><span data-stu-id="b6e7d-106">Page Inspector in Visual Studio 2012 is a web development tool with an integrated browser.</span></span> <span data-ttu-id="b6e7d-107">Wybierz dowolny element w zintegrowanej przeglądarki i narzędzie Page Inspector natychmiast wyróżnia elementu źródłowego i CSS.</span><span class="sxs-lookup"><span data-stu-id="b6e7d-107">Select any element in the integrated browser, and Page Inspector instantly highlights the element's source and CSS.</span></span> <span data-ttu-id="b6e7d-108">Można wybrać dowolny widok MVC, szybkie znajdowanie źródeł renderowanego kodu znaczników i użyj narzędzia przeglądarki w środowisku Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="b6e7d-108">You can browse any MVC view, quickly find the sources of rendered markup, and use browser tools right within the Visual Studio environment.</span></span>
> 
> [<span data-ttu-id="b6e7d-109">Obejrzyj film</span><span class="sxs-lookup"><span data-stu-id="b6e7d-109">Watch the Video</span></span>](../../videos/mvc-4/using-page-inspector-in-aspnet-mvc.md)
> 
> <span data-ttu-id="b6e7d-110">W tym samouczku pokazano, jak włączyć tryb inspekcji, a następnie szybko Znajdź i edytowanie znaczników i CSS w ramach projektu sieci web.</span><span class="sxs-lookup"><span data-stu-id="b6e7d-110">This tutorial shows how to enable Inspection Mode, and then quickly locate and edit markup and CSS within your web project.</span></span> <span data-ttu-id="b6e7d-111">W samouczku projektu MVC, ale można również użyć narzędzia Page Inspector dla [formularzy sieci Web](https://go.microsoft.com/?linkid=9802001) i innych aplikacji ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="b6e7d-111">The tutorial uses an MVC Project, but you can also use Page Inspector for [Web Forms](https://go.microsoft.com/?linkid=9802001) and other ASP.NET applications.</span></span>
> 
> <span data-ttu-id="b6e7d-112">Samouczek zawiera następujące sekcje:</span><span class="sxs-lookup"><span data-stu-id="b6e7d-112">The tutorial has the following sections:</span></span>
> 
> - [<span data-ttu-id="b6e7d-113">Wymagania wstępne</span><span class="sxs-lookup"><span data-stu-id="b6e7d-113">Prerequisites</span></span>](#_1_prerequisites)
> - [<span data-ttu-id="b6e7d-114">Tworzenie aplikacji sieci Web</span><span class="sxs-lookup"><span data-stu-id="b6e7d-114">Create a Web Application</span></span>](#_2_creating_a)
> - [<span data-ttu-id="b6e7d-115">Użyj narzędzia Page Inspector do przejdź do widoku</span><span class="sxs-lookup"><span data-stu-id="b6e7d-115">Use Page Inspector to Browse to a View</span></span>](#_3_using_page)
> - [<span data-ttu-id="b6e7d-116">Włącz tryb inspekcji</span><span class="sxs-lookup"><span data-stu-id="b6e7d-116">Enable Inspection Mode</span></span>](#_4_inspection_mode)
> - [<span data-ttu-id="b6e7d-117">Narzędzie Page Inspector umożliwia zmianę znaczników</span><span class="sxs-lookup"><span data-stu-id="b6e7d-117">Use Page Inspector to Make Changes to Markup</span></span>](#_5_using_page)
> - [<span data-ttu-id="b6e7d-118">Tryb inspekcji i okno HTML</span><span class="sxs-lookup"><span data-stu-id="b6e7d-118">Inspection Mode and the HTML Window</span></span>](#_6_inspection_mode)
> - [<span data-ttu-id="b6e7d-119">Podgląd zmian CSS w oknie style</span><span class="sxs-lookup"><span data-stu-id="b6e7d-119">Preview CSS Changes in the Styles window</span></span>](#_7_previewing_css)
> - [<span data-ttu-id="b6e7d-120">Automatyczna synchronizacja z CSS</span><span class="sxs-lookup"><span data-stu-id="b6e7d-120">CSS Auto Sync</span></span>](#css_auto_sync)
> - [<span data-ttu-id="b6e7d-121">Za pomocą selektora kolorów CSS</span><span class="sxs-lookup"><span data-stu-id="b6e7d-121">Using the CSS Color Picker</span></span>](#css_color_picker)
> - [<span data-ttu-id="b6e7d-122">Elementy strony dynamiczne mapowania JavaScript</span><span class="sxs-lookup"><span data-stu-id="b6e7d-122">Mapping Dynamic Page Elements to JavaScript</span></span>](#map_dynamic_elements)


<a id="_prerequisites"></a><a id="_1_prerequisites"></a>

## <a name="prerequisites"></a><span data-ttu-id="b6e7d-123">Wymagania wstępne</span><span class="sxs-lookup"><span data-stu-id="b6e7d-123">Prerequisites</span></span>

- <span data-ttu-id="b6e7d-124">[Program Visual Studio 2012](https://www.microsoft.com/visualstudio/11) lub [programu Visual Studio Express 2012 for Web](https://www.microsoft.com/visualstudio/11/downloads#express-web).</span><span class="sxs-lookup"><span data-stu-id="b6e7d-124">[Visual Studio 2012](https://www.microsoft.com/visualstudio/11) or [Visual Studio Express 2012 for Web](https://www.microsoft.com/visualstudio/11/downloads#express-web).</span></span>

> [!NOTE]
> <span data-ttu-id="b6e7d-125">Aby uzyskać najnowszą wersję narzędzia Page Inspector, należy użyć [Instalatora platformy sieci Web](https://go.microsoft.com/fwlink/?LinkId=255386) zainstalować zestaw Windows Azure SDK dla platformy .NET 2.0.</span><span class="sxs-lookup"><span data-stu-id="b6e7d-125">To get the latest version of Page Inspector, use [Web Platform Installer](https://go.microsoft.com/fwlink/?LinkId=255386) to install the Windows Azure SDK for .NET 2.0.</span></span>


<span data-ttu-id="b6e7d-126">Narzędzie Page Inspector jest dołączany do narzędzia Microsoft Web Developer Tools.</span><span class="sxs-lookup"><span data-stu-id="b6e7d-126">Page Inspector is bundled with Microsoft Web Developer Tools.</span></span> <span data-ttu-id="b6e7d-127">Najnowsza wersja to 1.3.</span><span class="sxs-lookup"><span data-stu-id="b6e7d-127">The latest version is 1.3.</span></span> <span data-ttu-id="b6e7d-128">Aby sprawdzić, która wersja ma, uruchom Visual Studio i wybierz **Microsoft Visual Studio** z **pomocy** menu.</span><span class="sxs-lookup"><span data-stu-id="b6e7d-128">To check which version you have, run Visual Studio and select **About Microsoft Visual Studio** from the **Help** menu.</span></span>

<a id="_creating_a_web"></a><a id="_2_creating_a"></a>

## <a name="create-a-web-application"></a><span data-ttu-id="b6e7d-129">Tworzenie aplikacji sieci Web</span><span class="sxs-lookup"><span data-stu-id="b6e7d-129">Create a Web Application</span></span>

<span data-ttu-id="b6e7d-130">Najpierw należy utworzyć aplikacji sieci web, która będzie używana narzędzie Page Inspector z.</span><span class="sxs-lookup"><span data-stu-id="b6e7d-130">First, create a web application that you will use Page Inspector with.</span></span> <span data-ttu-id="b6e7d-131">W programie Visual Studio, wybierz **pliku** &gt; **nowy projekt**.</span><span class="sxs-lookup"><span data-stu-id="b6e7d-131">In Visual Studio, choose **File** &gt; **New Project**.</span></span> <span data-ttu-id="b6e7d-132">Po lewej stronie rozwiń węzeł **Visual C#**, wybierz pozycję **Web**, a następnie wybierz **aplikacji sieci Web ASP.NET MVC4**.</span><span class="sxs-lookup"><span data-stu-id="b6e7d-132">On the left, expand **Visual C#**, select **Web**, and then select **ASP.NET MVC4 Web Application**.</span></span>

![Nowa aplikacja platformy ASP.NET MVC](using-page-inspector-in-aspnet-mvc/_static/image2.png)

<span data-ttu-id="b6e7d-134">Kliknij przycisk **OK**.</span><span class="sxs-lookup"><span data-stu-id="b6e7d-134">Click **OK**.</span></span>

<span data-ttu-id="b6e7d-135">W **nowy projekt programu ASP.NET MVC 4** okno dialogowe, wybierz opcję **aplikacji internetowej**.</span><span class="sxs-lookup"><span data-stu-id="b6e7d-135">In the **New ASP.NET MVC 4 Project** dialog box, select **Internet Application**.</span></span> <span data-ttu-id="b6e7d-136">Pozostaw **Razor** jako domyślny aparat widoku.</span><span class="sxs-lookup"><span data-stu-id="b6e7d-136">Leave **Razor** as the default view engine.</span></span>

![Nowy projekt ASP.NET MVC — aplikacji internetowej](using-page-inspector-in-aspnet-mvc/_static/image4.png)

<span data-ttu-id="b6e7d-138">Aplikacja zostanie otwarta w **źródła** widoku.</span><span class="sxs-lookup"><span data-stu-id="b6e7d-138">The application opens in **Source** view.</span></span>

![Nowa aplikacja platformy ASP.NET MVC w widoku źródła](using-page-inspector-in-aspnet-mvc/_static/image6.png)

<span data-ttu-id="b6e7d-140">Teraz, po aplikacji do pracy z służy narzędzie Page Inspector do zbadania i zmodyfikowania go.</span><span class="sxs-lookup"><span data-stu-id="b6e7d-140">Now that you have an application to work with, you can use Page Inspector to examine and modify it.</span></span>

<a id="_starting_page_inspector"></a><a id="_3_using_page"></a>

## <a name="use-page-inspector-to-browse-to-a-view"></a><span data-ttu-id="b6e7d-141">Użyj narzędzia Page Inspector do przejdź do widoku</span><span class="sxs-lookup"><span data-stu-id="b6e7d-141">Use Page Inspector to Browse to a View</span></span>

<span data-ttu-id="b6e7d-142">W programie Visual Studio 2012, należy kliknąć prawym przyciskiem myszy dowolny widok w projekcie, wybierz opcję **widoku w narzędzie Page Inspector**, i narzędzie Page Inspector będzie ustalić trasy i wyświetli tej strony.</span><span class="sxs-lookup"><span data-stu-id="b6e7d-142">In Visual Studio 2012, you can right-click any view in your project, select **View in Page Inspector**, and Page Inspector will figure out the route and display the page.</span></span>

<span data-ttu-id="b6e7d-143">W **Eksploratora rozwiązań**, rozwiń węzeł **widoków** folder, a następnie **Home** folderu.</span><span class="sxs-lookup"><span data-stu-id="b6e7d-143">In **Solution Explorer**, expand the **Views** folder and then the **Home** folder.</span></span> <span data-ttu-id="b6e7d-144">Kliknij prawym przyciskiem myszy plik Index.cshtml i wybierz polecenie **widoku w narzędzie Page Inspector**.</span><span class="sxs-lookup"><span data-stu-id="b6e7d-144">Right click the Index.cshtml file and choose **View in Page Inspector**.</span></span>

![Wyświetl Index.cshtml w narzędzie Page Inspector](using-page-inspector-in-aspnet-mvc/_static/image8.png)

<span data-ttu-id="b6e7d-146">Domyślnie narzędzie Page Inspector jest zadokowany jako okno po lewej stronie środowiska Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="b6e7d-146">By default, Page Inspector is docked as a window on the left side of the Visual Studio environment.</span></span> <span data-ttu-id="b6e7d-147">Jeśli wolisz, możesz dock go w innym miejscu lub Oddokuj okna.</span><span class="sxs-lookup"><span data-stu-id="b6e7d-147">If you prefer, you can dock it elsewhere, or undock the window.</span></span> <span data-ttu-id="b6e7d-148">Zobacz [porady: Aranżowanie i dokowanie okien](https://msdn.microsoft.com/library/z4y0hsax.aspx).</span><span class="sxs-lookup"><span data-stu-id="b6e7d-148">See [How to: Arrange and Dock Windows](https://msdn.microsoft.com/library/z4y0hsax.aspx).</span></span>

<span data-ttu-id="b6e7d-149">Górne okienko okna narzędzia Page Inspector pokazuje bieżącą stronę w przeglądarce.</span><span class="sxs-lookup"><span data-stu-id="b6e7d-149">The top pane of the Page Inspector window shows the current page in a browser window.</span></span> <span data-ttu-id="b6e7d-150">Dolne okienko zawiera strony w kod znaczników HTML, wraz z niektórych kart, które pozwalają sprawdzić różnych aspektów strony.</span><span class="sxs-lookup"><span data-stu-id="b6e7d-150">The bottom pane shows the page in HTML markup, along with some tabs that let you inspect different aspects of the page.</span></span> <span data-ttu-id="b6e7d-151">Dolne okienko jest podobny do [F12 Developer Tools](https://msdn.microsoft.com/ie/aa740478) w programie Internet Explorer.</span><span class="sxs-lookup"><span data-stu-id="b6e7d-151">The bottom pane is similar to the [F12 Developer Tools](https://msdn.microsoft.com/ie/aa740478) in Internet Explorer.</span></span>

![Aplikacji ASP.NET MVC w narzędzie Page Inspector](using-page-inspector-in-aspnet-mvc/_static/image10.png)

<span data-ttu-id="b6e7d-153">W tym samouczku użyjesz **HTML** i **style** karty, aby szybko przejść i wprowadzenia zmian w aplikacji.</span><span class="sxs-lookup"><span data-stu-id="b6e7d-153">In this tutorial, you will use the **HTML** and **Styles** tabs to navigate quickly and make changes to the application.</span></span>

<a id="_examining_(&quot;decomposing&quot;)_the"></a><a id="_inspection_mode_and"></a><a id="_4_inspection_mode"></a>

## <a name="enableinspection-mode"></a><span data-ttu-id="b6e7d-154">Tryb EnableInspection</span><span class="sxs-lookup"><span data-stu-id="b6e7d-154">EnableInspection Mode</span></span>

<span data-ttu-id="b6e7d-155">Aby uruchomić narzędzie Page Inspector w trybie inspekcji, kliknij przycisk **inspekcję** przycisku.</span><span class="sxs-lookup"><span data-stu-id="b6e7d-155">To put Page Inspector into Inspection Mode, click the **Inspect** button.</span></span> <span data-ttu-id="b6e7d-156">W trybie inspekcji gdy kursora myszy nad dowolną część renderowanej strony odpowiedni kod źródłowy lub kod zostanie wyróżniona.</span><span class="sxs-lookup"><span data-stu-id="b6e7d-156">In Inspection Mode, when you hold the mouse pointer over any part of the rendered page, the corresponding source markup or code is highlighted.</span></span>

![Przełącz tryb inspekcji](using-page-inspector-in-aspnet-mvc/_static/image12.png)

<span data-ttu-id="b6e7d-158">Teraz myszą różnych części strony w narzędzie Page Inspector.</span><span class="sxs-lookup"><span data-stu-id="b6e7d-158">Now move your mouse over different parts of the page within Page Inspector.</span></span> <span data-ttu-id="b6e7d-159">Jak, kursor zmienia się na znak plus dużych i jest wyróżniony element poniżej:</span><span class="sxs-lookup"><span data-stu-id="b6e7d-159">As you do, the mouse pointer changes to a large plus sign, and the element underneath is highlighted:</span></span>

![Ustawiając kursor nad div.content otoki](using-page-inspector-in-aspnet-mvc/_static/image14.png)

<span data-ttu-id="b6e7d-161">Podczas przesuwania wskaźnika myszy Visual Studio prezentuje odpowiedniej składni Razor w pliku źródłowym.</span><span class="sxs-lookup"><span data-stu-id="b6e7d-161">As you move the mouse pointer, Visual Studio highlights the corresponding Razor syntax in the source file.</span></span> <span data-ttu-id="b6e7d-162">Jeśli HTML element pochodzą z innego pliku źródłowego, Visual Studio automatycznie otwiera plik.</span><span class="sxs-lookup"><span data-stu-id="b6e7d-162">If the HTML element comes from another source file, Visual Studio automatically opens the file.</span></span>

<span data-ttu-id="b6e7d-163">W narzędzie Page Inspector **HTML** karta zawiera kod HTML, który został wygenerowany z użyciem składni Razor.</span><span class="sxs-lookup"><span data-stu-id="b6e7d-163">In Page Inspector, the **HTML** tab shows the HTML that was generated from the Razor syntax.</span></span> <span data-ttu-id="b6e7d-164">Podczas przesuwania wskaźnika myszy, są wyróżnione elementów HTML.</span><span class="sxs-lookup"><span data-stu-id="b6e7d-164">As you move the mouse pointer, the HTML elements are highlighted.</span></span> <span data-ttu-id="b6e7d-165">**Style** karta zawiera reguły CSS dla elementu.</span><span class="sxs-lookup"><span data-stu-id="b6e7d-165">The **Styles** tab shows the CSS rules for the element.</span></span>

<a id="_5_using_page"></a>

## <a name="use-page-inspector-to-make-changes-to-markup"></a><span data-ttu-id="b6e7d-166">Narzędzie Page Inspector umożliwia zmianę znaczników</span><span class="sxs-lookup"><span data-stu-id="b6e7d-166">Use Page Inspector to Make Changes to Markup</span></span>

<span data-ttu-id="b6e7d-167">Narzędzie Page Inspector umożliwia znaleźć znaczników, którego lokalizacja może nie być oczywista.</span><span class="sxs-lookup"><span data-stu-id="b6e7d-167">Page Inspector lets you find markup whose location might not be obvious.</span></span> <span data-ttu-id="b6e7d-168">Następnie można zmodyfikować kod znaczników i wyświetlić wynikowe zmiany.</span><span class="sxs-lookup"><span data-stu-id="b6e7d-168">Then you can modify the markup and see the resulting changes.</span></span>

<span data-ttu-id="b6e7d-169">Aby zobaczyć, kliknij przycisk **inspekcję** , a następnie przewiń w dół strony w oknie narzędzia Page Inspector.</span><span class="sxs-lookup"><span data-stu-id="b6e7d-169">To see this, click **Inspect** and then scroll to the bottom of the page in the Page Inspector window.</span></span>

<span data-ttu-id="b6e7d-170">Podczas przesuwania wskaźnika myszy do obszaru stopki zostanie otwarte narzędzie Page Inspector \_plik Layout.cshtml i zaznacza sekcji wybranej strony układu.</span><span class="sxs-lookup"><span data-stu-id="b6e7d-170">When you move the mouse pointer into the footer area, Page Inspector opens the \_Layout.cshtml file and highlights the section of the layout page that you have selected.</span></span> <span data-ttu-id="b6e7d-171">Jak widać, czy stopka jest zdefiniowany w pliku układu, a nie samego widoku.</span><span class="sxs-lookup"><span data-stu-id="b6e7d-171">As you can see, the footer are is defined in the layout file, and not the view itself.</span></span>

![Stopki](using-page-inspector-in-aspnet-mvc/_static/image16.png)

<span data-ttu-id="b6e7d-173">Teraz umieść wskaźnik myszy na linii o prawach autorskich <a id="a"> </a>zauważyć.</span><span class="sxs-lookup"><span data-stu-id="b6e7d-173">Now move your mouse pointer over the line with the copyright <a id="a"></a>notice.</span></span> <span data-ttu-id="b6e7d-174">W \_zostanie wyróżniona Layout.cshtml strony, odpowiednim wierszu.</span><span class="sxs-lookup"><span data-stu-id="b6e7d-174">In the \_Layout.cshtml page, the corresponding line is highlighted.</span></span>

![Stopka wiersza o prawach autorskich wyróżnione](using-page-inspector-in-aspnet-mvc/_static/image18.png)

<span data-ttu-id="b6e7d-176">Dodaj tekst do końca wiersza w \_plik Layout.cshtml.</span><span class="sxs-lookup"><span data-stu-id="b6e7d-176">Add some text to the end of the line in the \_Layout.cshtml file.</span></span>

<span data-ttu-id="b6e7d-177">&lt;p&gt;&amp;kopiowania; @DateTime.Now.Year — Moja aplikacja platformy ASP.NET MVC zmieni! &lt;/p&gt;</span><span class="sxs-lookup"><span data-stu-id="b6e7d-177">&lt;p&gt;&amp;copy; @DateTime.Now.Year - My ASP.NET MVC Application Rocks!&lt;/p&gt;</span></span>

<span data-ttu-id="b6e7d-178">Teraz naciśnij kombinację klawiszy Ctrl + Alt + Enter lub kliknij przycisk paska aktualizacji, aby wyświetlić wyniki w oknie przeglądarki narzędzie Page Inspector.</span><span class="sxs-lookup"><span data-stu-id="b6e7d-178">Now, press Ctrl+Alt+Enter or click the Update Bar to see the results in the Page Inspector browser window.</span></span>

![Moje skały aplikacji ASP.NET!](using-page-inspector-in-aspnet-mvc/_static/image20.png)

<span data-ttu-id="b6e7d-180">Może mieć uważasz stopki zdefiniowane w Index.cshtml, że włączone, w \_Layout.cshtml i narzędzie Page Inspector odnaleziona automatycznie.</span><span class="sxs-lookup"><span data-stu-id="b6e7d-180">You might have thought that the footer defined in Index.cshtml, but it turned out to be in the \_Layout.cshtml, and Page Inspector found it for you.</span></span>

<a id="_inspection_mode_and_1"></a><a id="_6_inspection_mode"></a>

## <a name="inspection-mode-and-the-html-window"></a><span data-ttu-id="b6e7d-181">Tryb inspekcji i okno HTML</span><span class="sxs-lookup"><span data-stu-id="b6e7d-181">Inspection Mode and the HTML Window</span></span>

<span data-ttu-id="b6e7d-182">Następnie należy krótki przegląd okno HTML i jak mapuje elementy dla Ciebie.</span><span class="sxs-lookup"><span data-stu-id="b6e7d-182">Next, you will have a quick look at the HTML window and how it maps elements for you.</span></span>

<span data-ttu-id="b6e7d-183">Kliknij przycisk **inspekcję** chcesz włączyć tryb inspekcji narzędzie Page Inspector.</span><span class="sxs-lookup"><span data-stu-id="b6e7d-183">Click **Inspect** to put Page Inspector in Inspection Mode.</span></span>

<span data-ttu-id="b6e7d-184">Kliknij w górnej części strony, stwierdzający "logohere".</span><span class="sxs-lookup"><span data-stu-id="b6e7d-184">Click the top part of the page that says "Your logohere".</span></span> <span data-ttu-id="b6e7d-185">Są badanie dany element bardziej szczegółowo dzięki wyświetlana w oknie przeglądarki już zmiany podczas przesuwania wskaźnika myszy.</span><span class="sxs-lookup"><span data-stu-id="b6e7d-185">You are examining a particular element in more detail, so the display in the browser window no longer changes as you move the mouse pointer.</span></span>

<span data-ttu-id="b6e7d-186">Teraz umieść wskaźnik myszy **HTML** okna.</span><span class="sxs-lookup"><span data-stu-id="b6e7d-186">Now move the mouse pointer to the **HTML** window.</span></span> <span data-ttu-id="b6e7d-187">Podczas przesuwania wskaźnika myszy narzędzie Page Inspector przedstawiono w elemencie **HTML** okna i zaznacza odpowiadający mu element w oknie przeglądarki.</span><span class="sxs-lookup"><span data-stu-id="b6e7d-187">As you move the mouse pointer, Page Inspector outlines the element within the **HTML** window and highlights the corresponding element in the browser window.</span></span>

![Okno HTML](using-page-inspector-in-aspnet-mvc/_static/image22.png)

<span data-ttu-id="b6e7d-189">Jak wcześniej, zostanie otwarte narzędzie Page Inspector \_plik Layout.cshtml dla Ciebie na karcie tymczasowego. Kliknij przycisk \_Layout.cshtml kartę tymczasowego i odpowiednie znaczników zostanie podświetlony w &lt;nagłówka&gt; sekcji można:</span><span class="sxs-lookup"><span data-stu-id="b6e7d-189">As before, Page Inspector opens the \_Layout.cshtml file for you in a temporary tab. Click the \_Layout.cshtml temporary tab, and the corresponding markup will be highlighted in the &lt;header&gt; section for you:</span></span>

![Wyróżnione znaczników](using-page-inspector-in-aspnet-mvc/_static/image24.png)

<a id="_using_page_inspector"></a><a id="_7_previewing_css"></a>

## <a name="preview-css-changes-in-the-styles-window"></a><span data-ttu-id="b6e7d-191">Podgląd zmian CSS w oknie style</span><span class="sxs-lookup"><span data-stu-id="b6e7d-191">Preview CSS Changes in the Styles Window</span></span>

<span data-ttu-id="b6e7d-192">Następnie użyje narzędzie Page Inspector **style** okno, aby wyświetlić podgląd zmian do arkusza CSS.</span><span class="sxs-lookup"><span data-stu-id="b6e7d-192">Next, you will use the Page Inspector **Styles** window to preview changes to CSS.</span></span>

<span data-ttu-id="b6e7d-193">Kliknij przycisk **inspekcję** chcesz włączyć tryb inspekcji narzędzie Page Inspector.</span><span class="sxs-lookup"><span data-stu-id="b6e7d-193">Click **Inspect** to put Page Inspector in Inspection Mode.</span></span>

<span data-ttu-id="b6e7d-194">W oknie przeglądarki narzędzie Page Inspector, przenieś wskaźnik myszy w sekcji "Strona główna" do **div.content otoki** jest wyświetlana etykieta.</span><span class="sxs-lookup"><span data-stu-id="b6e7d-194">In the Page Inspector browser window, move the mouse pointer over the "Home Page" section until the **div.content-wrapper** label appears.</span></span>

![Ustawiając kursor nad div.content otoki](using-page-inspector-in-aspnet-mvc/_static/image26.png)

<span data-ttu-id="b6e7d-196">Kliknij wewnątrz sekcji otoki div.content, a następnie przesuń wskaźnik myszy do **style** okna.</span><span class="sxs-lookup"><span data-stu-id="b6e7d-196">Click within the div.content-wrapper section once, and then move the mouse pointer to the **Styles** window.</span></span> <span data-ttu-id="b6e7d-197">**Syles** oknie wyświetlane są wszystkie reguły CSS dla tego elementu.</span><span class="sxs-lookup"><span data-stu-id="b6e7d-197">The **Syles** window shows all of the CSS rules for this element.</span></span> <span data-ttu-id="b6e7d-198">Przewiń w dół do selektora klasy Znajdź .featured .content otoki.</span><span class="sxs-lookup"><span data-stu-id="b6e7d-198">Scroll down to find the .featured .content-wrapper class selector.</span></span> <span data-ttu-id="b6e7d-199">Teraz, wyczyść pole wyboru dla właściwości kolor tła.</span><span class="sxs-lookup"><span data-stu-id="b6e7d-199">Now clear the checkbox for the background-color property.</span></span>

![Kolor tła wyczyść](using-page-inspector-in-aspnet-mvc/_static/image28.png)

<span data-ttu-id="b6e7d-201">Zwróć uwagę, jak zmiana Wyświetla podgląd natychmiast w oknie przeglądarki narzędzie Page Inspector.</span><span class="sxs-lookup"><span data-stu-id="b6e7d-201">Notice how the change previews instantly in the Page Inspector browser window.</span></span>

<span data-ttu-id="b6e7d-202">Zaznacz pole wyboru ponownie, kliknij dwukrotnie wartość właściwości i zmień ją na czerwony.</span><span class="sxs-lookup"><span data-stu-id="b6e7d-202">Select the checkbox again, then double-click the property value and change it to red.</span></span> <span data-ttu-id="b6e7d-203">Zmiana przedstawia natychmiast:</span><span class="sxs-lookup"><span data-stu-id="b6e7d-203">The change shows immediately:</span></span>

![Kolor tła czerwony](using-page-inspector-in-aspnet-mvc/_static/image30.png)

<span data-ttu-id="b6e7d-205">**Style** sprawia, że okno łatwe do testowania i Podgląd CSS zmienia przed dokonaniem zmiany stylu arkusza samej siebie.</span><span class="sxs-lookup"><span data-stu-id="b6e7d-205">The **Styles** window makes it easy to test and preview CSS changes before you commit the changes to the style sheet itself.</span></span>

<a id="css_auto_sync"></a>
## <a name="css-auto-sync"></a><span data-ttu-id="b6e7d-206">Automatyczna synchronizacja z CSS</span><span class="sxs-lookup"><span data-stu-id="b6e7d-206">CSS Auto Sync</span></span>

> [!NOTE]
> <span data-ttu-id="b6e7d-207">Ta funkcja wymaga wersji 1.3 narzędzie Page Inspector.</span><span class="sxs-lookup"><span data-stu-id="b6e7d-207">This feature requires version 1.3 of Page Inspector.</span></span>


<span data-ttu-id="b6e7d-208">Funkcja automatycznej synchronizacji CSS służy do bezpośredniego edytowania pliku CSS i zobaczyć zmiany bezpośrednio w przeglądarce narzędzie Page Inspector.</span><span class="sxs-lookup"><span data-stu-id="b6e7d-208">The CSS Auto-Sync feature allows you to edit a CSS file directly, and see the changes immediately in the Page Inspector browser.</span></span>

<span data-ttu-id="b6e7d-209">Kliknij przycisk **inspekcję** chcesz włączyć tryb inspekcji narzędzie Page Inspector.</span><span class="sxs-lookup"><span data-stu-id="b6e7d-209">Click **Inspect** to put Page Inspector in Inspection Mode.</span></span>

<span data-ttu-id="b6e7d-210">W przeglądarce narzędzie Page Inspector, przenieś wskaźnik myszy w sekcji "Strona główna" do **div.content otoki** jest wyświetlana etykieta.</span><span class="sxs-lookup"><span data-stu-id="b6e7d-210">In the Page Inspector browser, move the mouse pointer over the "Home Page" section until the **div.content-wrapper** label appears.</span></span> <span data-ttu-id="b6e7d-211">Kliknij raz, aby wybrać tego elementu.</span><span class="sxs-lookup"><span data-stu-id="b6e7d-211">Click once to select this element.</span></span>

<span data-ttu-id="b6e7d-212">**Syles** oknie wyświetlane są wszystkie reguły CSS dla tego elementu.</span><span class="sxs-lookup"><span data-stu-id="b6e7d-212">The **Syles** window shows all of the CSS rules for this element.</span></span> <span data-ttu-id="b6e7d-213">Przewiń w dół do selektora klasy Znajdź .featured .content otoki.</span><span class="sxs-lookup"><span data-stu-id="b6e7d-213">Scroll down to find the .featured .content-wrapper class selector.</span></span> <span data-ttu-id="b6e7d-214">Kliknij pozycję ".featured .content otoki".</span><span class="sxs-lookup"><span data-stu-id="b6e7d-214">Click on ".featured .content-wrapper".</span></span> <span data-ttu-id="b6e7d-215">Narzędzie Page Inspector otwiera plik kodu CSS, która definiuje ten styl (Site.css) i zaznacza odpowiednie stylu CSS.</span><span class="sxs-lookup"><span data-stu-id="b6e7d-215">Page Inspector opens the CSS file that defines this style (Site.css) and highlights the corresponding CSS style.</span></span>

![](using-page-inspector-in-aspnet-mvc/_static/image32.png)

<span data-ttu-id="b6e7d-216">Teraz Zmień wartość atrybutu `background-color` do "red".</span><span class="sxs-lookup"><span data-stu-id="b6e7d-216">Now change the value for `background-color` to "red".</span></span> <span data-ttu-id="b6e7d-217">W przeglądarce narzędzie Page Inspector zmiana pojawi się natychmiast.</span><span class="sxs-lookup"><span data-stu-id="b6e7d-217">The change appears immediately in the Page Inspector browser.</span></span>

![](using-page-inspector-in-aspnet-mvc/_static/image34.png)

<a id="css_color_picker"></a>
## <a name="using-the-css-color-picker"></a><span data-ttu-id="b6e7d-218">Za pomocą selektora kolorów CSS</span><span class="sxs-lookup"><span data-stu-id="b6e7d-218">Using the CSS Color Picker</span></span>

<span data-ttu-id="b6e7d-219">Edytor CSS w programie Visual Studio 2012 zawiera próbnika kolorów, który można łatwo wybrać i Wstaw kolorów.</span><span class="sxs-lookup"><span data-stu-id="b6e7d-219">The CSS editor in Visual Studio 2012 has a color picker that makes it easy to choose and insert colors.</span></span> <span data-ttu-id="b6e7d-220">Próbnika kolorów zawiera standardowe paletę kolorów, obsługuje standardowe kolor nazwy i skrótu, kolorów RGB, RGBA HSL i HSLA i przechowuje listę kolorów używanych ostatnio w dokumencie.</span><span class="sxs-lookup"><span data-stu-id="b6e7d-220">The color picker includes a standard palette of colors, supports standard color names, hash codes, RGB, RGBA, HSL, and HSLA colors, and maintains a list of the colors you've used most recently in the document.</span></span>

<span data-ttu-id="b6e7d-221">W poprzedniej sekcji, należy zmienić wartość `background-color` właściwości.</span><span class="sxs-lookup"><span data-stu-id="b6e7d-221">In the previous section, you changed the value of the `background-color` property.</span></span> <span data-ttu-id="b6e7d-222">Aby wywołać próbnika kolorów, umieść kursor po nazwie właściwości i typ **#** lub **rgb (**.</span><span class="sxs-lookup"><span data-stu-id="b6e7d-222">To invoke the color picker, place the insertion point after the property name and type **#** or **rgb(**.</span></span>

![Na pasku próbnika kolorów CSS](using-page-inspector-in-aspnet-mvc/_static/image36.png)

<span data-ttu-id="b6e7d-224">Kliknij kolor, który będzie zaznacz ją, a następnie naciśnij klawisz strzałki w dół, a następnie użyj klawiszy strzałek w lewo i w prawo do przechodzenia kolorów.</span><span class="sxs-lookup"><span data-stu-id="b6e7d-224">Click on a color to select it, or press the down arrow key and then use the left and right arrow keys to traverse the colors.</span></span> <span data-ttu-id="b6e7d-225">Podczas odwiedzania kolor jest przeglądany odpowiedniej wartości szesnastkowych:</span><span class="sxs-lookup"><span data-stu-id="b6e7d-225">When you visit a color, the corresponding hex value is previewed:</span></span>

![wartość właściwości kolor tła podglądu](using-page-inspector-in-aspnet-mvc/_static/image38.png)

<span data-ttu-id="b6e7d-227">Jeśli pasek koloru nie ma dokładnie kolor, który ma, można użyć selektora kolorów w dół pop.</span><span class="sxs-lookup"><span data-stu-id="b6e7d-227">If the color bar doesn't have the exact color you want, you can use the color picker pop-down.</span></span> <span data-ttu-id="b6e7d-228">Aby go otworzyć, kliknij przycisk ostrokątny na prawym końcu paska koloru, lub jeden lub dwa razy klawisz strzałki w dół na klawiaturze.</span><span class="sxs-lookup"><span data-stu-id="b6e7d-228">To open it, click the double chevron at the right end of the color bar, or press the Down Arrow once or twice on the keyboard.</span></span>

![Selektor koloru CSS Pop dół](using-page-inspector-in-aspnet-mvc/_static/image40.png)

<span data-ttu-id="b6e7d-230">Kliknij przycisk koloru z pionowy pasek po prawej stronie.</span><span class="sxs-lookup"><span data-stu-id="b6e7d-230">Click a color from the vertical bar on the right.</span></span> <span data-ttu-id="b6e7d-231">W oknie głównym wskazuje gradientu dla tego koloru.</span><span class="sxs-lookup"><span data-stu-id="b6e7d-231">This shows a gradient for that color in the main window.</span></span> <span data-ttu-id="b6e7d-232">Wybierz kolor bezpośrednio z pionowy pasek, naciskając klawisz Enter lub kliknij przycisk dowolnego punktu w oknie głównym, aby wybrać o większej dokładności.</span><span class="sxs-lookup"><span data-stu-id="b6e7d-232">Choose a color directly from the vertical bar by pressing Enter, or click any point in the main window to choose with greater precision.</span></span>

<span data-ttu-id="b6e7d-233">Jeśli na ekranie komputera, który ma być używany jest kolor (nie musi on być w interfejsie użytkownika programu Visual Studio), jego wartość można przechwycić przy użyciu narzędzia służącego do pobierania w prawej dolnej.</span><span class="sxs-lookup"><span data-stu-id="b6e7d-233">If there is a color on your computer screen that you want to use (it doesn't have to be inside the Visual Studio user interface), you can capture its value by using the eyedropper tool on the lower right.</span></span>

<span data-ttu-id="b6e7d-234">Możesz również zmienić przezroczystość koloru za pomocą suwaka w dolnej części próbnika kolorów.</span><span class="sxs-lookup"><span data-stu-id="b6e7d-234">You also can change the opacity of a color by moving the slider at the bottom of the color picker.</span></span> <span data-ttu-id="b6e7d-235">Grozi to zmiany kolor wartości z RGBA, ponieważ RGBA format może reprezentować nieprzezroczystość.</span><span class="sxs-lookup"><span data-stu-id="b6e7d-235">Doing so changes color values to RGBA values, because the RGBA format can represent opacity.</span></span>

<span data-ttu-id="b6e7d-236">Po wybraniu opcji koloru, naciśnij klawisz Enter, a następnie wpisz średnik przeprowadzenie wpisu kolor tła w *Site.css* pliku.</span><span class="sxs-lookup"><span data-stu-id="b6e7d-236">After you have chosen a color, press Enter, and then type a semicolon to complete the background-color entry in the *Site.css* file.</span></span>

<a id="_the_update_bar"></a>

### <a name="the-page-inspector-update-bar"></a><span data-ttu-id="b6e7d-237">Na pasku aktualizacji inspektora strony</span><span class="sxs-lookup"><span data-stu-id="b6e7d-237">The Page Inspector Update Bar</span></span>

<span data-ttu-id="b6e7d-238">Narzędzie Page Inspector natychmiast wykrywa zmiany *Site.css* plików i wyświetla alert w pasku aktualizacji.</span><span class="sxs-lookup"><span data-stu-id="b6e7d-238">Page Inspector immediately detects the change to the *Site.css* file and displays an alert in an update bar.</span></span>

![Pasek aktualizacji](using-page-inspector-in-aspnet-mvc/_static/image42.png)

<span data-ttu-id="b6e7d-240">Aby zapisać wszystkie pliki i odświeżyć przeglądarkę narzędzia Page Inspector, naciśnij kombinację klawiszy Ctrl + Alt + Enter lub kliknij przycisk paska aktualizacji.</span><span class="sxs-lookup"><span data-stu-id="b6e7d-240">To save all your files and refresh the Page Inspector browser, press Ctrl+Alt+Enter or click the update bar.</span></span> <span data-ttu-id="b6e7d-241">Zmiana koloru wyróżnienia zostanie wyświetlona w przeglądarce.</span><span class="sxs-lookup"><span data-stu-id="b6e7d-241">The change in the highlight color appears in the browser.</span></span>

<a id="map_dynamic_elements"></a>
## <a name="mapping-dynamic-page-elements-to-javascript"></a><span data-ttu-id="b6e7d-242">Elementy strony dynamiczne mapowania JavaScript</span><span class="sxs-lookup"><span data-stu-id="b6e7d-242">Mapping Dynamic Page Elements to JavaScript</span></span>

<span data-ttu-id="b6e7d-243">W nowoczesnych aplikacji sieci web elementy na stronie są często generowane dynamicznie z użyciem języka JavaScript.</span><span class="sxs-lookup"><span data-stu-id="b6e7d-243">In modern web applications, elements in the page are often generated dynamically with JavaScript.</span></span> <span data-ttu-id="b6e7d-244">Oznacza to, że nie istnieje żadne statycznych kod znaczników (HTML lub Razor) umożliwiająca te elementy na stronie.</span><span class="sxs-lookup"><span data-stu-id="b6e7d-244">That means there is no static markup (either HTML or Razor) that corresponds to these page elements.</span></span>

<span data-ttu-id="b6e7d-245">W wersji 1.3 narzędzie Page Inspector można teraz mapować elementy, które były dodawane dynamicznie do strony do odpowiedniego kodu JavaScript.</span><span class="sxs-lookup"><span data-stu-id="b6e7d-245">With version 1.3, Page Inspector can now map items that were dynamically added to the page back to the corresponding JavaScript code.</span></span> <span data-ttu-id="b6e7d-246">Aby zademonstrować tę funkcję, użyjemy [szablonu jednej strony aplikacji JEDNOSTRONICOWEJ](../../../single-page-application/overview/introduction/knockoutjs-template.md).</span><span class="sxs-lookup"><span data-stu-id="b6e7d-246">To demonstrate this feature, we'll use the [Single Page Application (SPA) template](../../../single-page-application/overview/introduction/knockoutjs-template.md).</span></span>

> [!NOTE]
> <span data-ttu-id="b6e7d-247">Szablon SPA wymaga [ASP.NET i 2012.2 narzędzia sieci Web](https://go.microsoft.com/fwlink/?LinkId=282650) aktualizacji.</span><span class="sxs-lookup"><span data-stu-id="b6e7d-247">The SPA template requires the [ASP.NET and Web Tools 2012.2](https://go.microsoft.com/fwlink/?LinkId=282650) update.</span></span>


<span data-ttu-id="b6e7d-248">W programie Visual Studio, wybierz **pliku** &gt; **nowy projekt**.</span><span class="sxs-lookup"><span data-stu-id="b6e7d-248">In Visual Studio, choose **File** &gt; **New Project**.</span></span> <span data-ttu-id="b6e7d-249">Po lewej stronie rozwiń węzeł **Visual C#**, wybierz pozycję **Web**, a następnie wybierz **aplikacji sieci Web ASP.NET MVC4**.</span><span class="sxs-lookup"><span data-stu-id="b6e7d-249">On the left, expand **Visual C#**, select **Web**, and then select **ASP.NET MVC4 Web Application**.</span></span> <span data-ttu-id="b6e7d-250">Kliknij przycisk **OK**.</span><span class="sxs-lookup"><span data-stu-id="b6e7d-250">Click **OK**.</span></span>

<span data-ttu-id="b6e7d-251">W **nowy projekt programu ASP.NET MVC 4** okno dialogowe, wybierz opcję **jednej strony aplikacji**.</span><span class="sxs-lookup"><span data-stu-id="b6e7d-251">In the **New ASP.NET MVC 4 Project** dialog, select **Single Page Application**.</span></span>

<span data-ttu-id="b6e7d-252">W Eksploratorze rozwiązań rozwiń **widoków** folder, a następnie **Home** folderu.</span><span class="sxs-lookup"><span data-stu-id="b6e7d-252">In Solution Explorer, expand the **Views** folder and then the **Home** folder.</span></span> <span data-ttu-id="b6e7d-253">Kliknij prawym przyciskiem myszy plik Index.cshtml i wybierz polecenie **widoku w narzędzie Page Inspector**.</span><span class="sxs-lookup"><span data-stu-id="b6e7d-253">Right click the Index.cshtml file and choose **View in Page Inspector**.</span></span>

<span data-ttu-id="b6e7d-254">Po pierwsze jest wyświetlany w przeglądarce narzędzie Page Inspector jest strony logowania.</span><span class="sxs-lookup"><span data-stu-id="b6e7d-254">The first thing that is displayed in the Page Inspector browser is a login page.</span></span> <span data-ttu-id="b6e7d-255">Kliknij przycisk "Utwórz konto", a następnie utwórz nazwę użytkownika i hasło.</span><span class="sxs-lookup"><span data-stu-id="b6e7d-255">Click "Sign Up" and create a user name and password.</span></span> <span data-ttu-id="b6e7d-256">Po zarejestrowaniu się aplikacja loguje się użytkownik i tworzy listę zadań do wykonania z niektórych przykładowych elementów.</span><span class="sxs-lookup"><span data-stu-id="b6e7d-256">Once you sign up, the application logs you in and creates a to-do list with some sample items.</span></span>

<span data-ttu-id="b6e7d-257">Kliknij przycisk **inspekcję** chcesz włączyć tryb inspekcji narzędzie Page Inspector.</span><span class="sxs-lookup"><span data-stu-id="b6e7d-257">Click **Inspect** to put Page Inspector in Inspection Mode.</span></span> <span data-ttu-id="b6e7d-258">W przeglądarce narzędzie Page Inspector kliknij jeden z elementów do wykonania.</span><span class="sxs-lookup"><span data-stu-id="b6e7d-258">In the Page Inspector browser, click on one of the to-do items.</span></span> <span data-ttu-id="b6e7d-259">Zwróć uwagę, że zamiast są zaznaczone na niebiesko, element jest wyróżniona w kolorze pomarańczowym z "JS" obok nazwy elementu.</span><span class="sxs-lookup"><span data-stu-id="b6e7d-259">Notice that instead of being highlighted in blue, the element is highlighted in orange, with "JS" next to the element name.</span></span> <span data-ttu-id="b6e7d-260">Oznacza to, że element został utworzony dynamicznie za pośrednictwem skryptu.</span><span class="sxs-lookup"><span data-stu-id="b6e7d-260">This indicates that the element was created dynamically through script.</span></span>

![](using-page-inspector-in-aspnet-mvc/_static/image44.png)

<span data-ttu-id="b6e7d-261">Ponadto pomarańczowy podkreślenie pojawia się na **stos wywołań** kartę. Oznacza to, że **stos wywołań** okienko zawiera więcej informacji o elemencie.</span><span class="sxs-lookup"><span data-stu-id="b6e7d-261">In addition, an orange underline appears on the **Call Stack** tab. This indicates that the **Call Stack** pane has more information about the element.</span></span>

<span data-ttu-id="b6e7d-262">Polecenie **stos wywołań** kartę. **Stos wywołań** w okienku zostaną wyświetlone stosu wywołań utworzony element wywołania języka JavaScript.</span><span class="sxs-lookup"><span data-stu-id="b6e7d-262">Click on the **Call Stack** tab. The **Call Stack** pane shows the call stack for the JavaScript call that created the element.</span></span> <span data-ttu-id="b6e7d-263">Wywołuje do zewnętrznej biblioteki np. jQuery jest zwinięte, dzięki czemu można łatwo Zobacz wywołania do skryptu aplikacji.</span><span class="sxs-lookup"><span data-stu-id="b6e7d-263">Calls to external libraries such as jQuery are collapsed, so that you can easily see the calls to your application script.</span></span>

![](using-page-inspector-in-aspnet-mvc/_static/image46.png)

<span data-ttu-id="b6e7d-264">Aby wyświetlić pełną stosu, w tym wywołania do zewnętrznej biblioteki, można rozwinąć węzły z etykietą "Biblioteki zewnętrznej":</span><span class="sxs-lookup"><span data-stu-id="b6e7d-264">To see the full stack, including calls to external libraries, you can expand the nodes labeled "External Libraries":</span></span>

![](using-page-inspector-in-aspnet-mvc/_static/image48.png)

<span data-ttu-id="b6e7d-265">Po kliknięciu elementu w stosie wywołań Visual Studio otworzy plik kodu i zaznacza odpowiadający jej skrypt.</span><span class="sxs-lookup"><span data-stu-id="b6e7d-265">If you click an item in the call stack, Visual Studio opens the code file and highlights the corresponding script.</span></span>

![](using-page-inspector-in-aspnet-mvc/_static/image50.png)

## <a name="see-also"></a><span data-ttu-id="b6e7d-266">Zobacz też</span><span class="sxs-lookup"><span data-stu-id="b6e7d-266">See Also</span></span>

<span data-ttu-id="b6e7d-267">[Wprowadzenie do platformy ASP.NET MVC 4 z programem Visual Studio](../older-versions/getting-started-with-aspnet-mvc4/intro-to-aspnet-mvc-4.md) (witryny sieci Web programu ASP.net)</span><span class="sxs-lookup"><span data-stu-id="b6e7d-267">[Intro to ASP.NET MVC 4 with Visual Studio](../older-versions/getting-started-with-aspnet-mvc4/intro-to-aspnet-mvc-4.md) (ASP.net website)</span></span>

<span data-ttu-id="b6e7d-268">[Wprowadzenie do narzędzia Page Inspector](https://channel9.msdn.com/posts/visual-studio-vnext-introducing-page-inspector/) (wideo z witryny Channel 9)</span><span class="sxs-lookup"><span data-stu-id="b6e7d-268">[Introducing Page Inspector](https://channel9.msdn.com/posts/visual-studio-vnext-introducing-page-inspector/) (Channel 9 video)</span></span>

<span data-ttu-id="b6e7d-269">[Komunikaty o błędach narzędzia Page Inspector](https://go.microsoft.com/?linkid=9813062) (MSDN)</span><span class="sxs-lookup"><span data-stu-id="b6e7d-269">[Page Inspector Error Messages](https://go.microsoft.com/?linkid=9813062) (MSDN)</span></span>
