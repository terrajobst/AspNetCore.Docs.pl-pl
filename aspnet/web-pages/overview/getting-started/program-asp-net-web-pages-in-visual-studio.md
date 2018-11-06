---
uid: aspnet/web-pages/overview/getting-started/program-asp-net-web-pages-in-visual-studio
title: Programowanie ASP.NET Web Pages (Razor) przy użyciu programu Visual Studio | Dokumentacja firmy Microsoft
author: Rick-Anderson
description: Niniejszym załączniku opisano, jak można użyć Visual Studio 2010 ani Visual Web Developer 2010 Express do programu ASP.NET Web Pages o składni Razor.
ms.author: riande
ms.date: 02/13/2014
ms.assetid: 0acfec5a-48f2-4766-a801-a0f426966f0a
msc.legacyurl: /web-pages/overview/getting-started/program-asp-net-web-pages-in-visual-studio
msc.type: authoredcontent
ms.openlocfilehash: 5b8df17ec1021d133579e23cb4f5b0d0f67d4c7c
ms.sourcegitcommit: 2d3e5422d530203efdaf2014d1d7df31f88d08d0
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/05/2018
ms.locfileid: "51020458"
---
<a name="programming-aspnet-web-pages-razor-using-visual-studio"></a><span data-ttu-id="f17d0-103">Programowanie wzorca ASP.NET Web Pages (Razor) przy użyciu programu Visual Studio</span><span class="sxs-lookup"><span data-stu-id="f17d0-103">Programming ASP.NET Web Pages (Razor) Using Visual Studio</span></span>
====================
<span data-ttu-id="f17d0-104">przez [Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="f17d0-104">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="f17d0-105">W tym artykule wyjaśniono, jak można użyć programu Visual Studio lub Visual Web Developer Express do programu ASP.NET Web Pages (Razor) witryn sieci Web.</span><span class="sxs-lookup"><span data-stu-id="f17d0-105">This article explains how you can use Visual Studio or Visual Web Developer Express to program ASP.NET Web Pages (Razor) websites.</span></span>
>
> <span data-ttu-id="f17d0-106">Dowiesz się</span><span class="sxs-lookup"><span data-stu-id="f17d0-106">What you'll learn</span></span>
>
> - <span data-ttu-id="f17d0-107">Co jest potrzebne do zainstalowania (jeśli nic) do pracy przy użyciu stron ASP.NET Web Pages w używanej wersji programu Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="f17d0-107">What you need to install (if anything) to work with ASP.NET Web Pages in your version of Visual Studio.</span></span>
> - <span data-ttu-id="f17d0-108">Jak dodać obsługę dla stron sieci Web platformy ASP.NET do programu Visual Web Developer 2010 Express.</span><span class="sxs-lookup"><span data-stu-id="f17d0-108">How to add support for ASP.NET Web Pages to Visual Web Developer 2010 Express.</span></span>
> - <span data-ttu-id="f17d0-109">Jak używać funkcji w programie Visual Studio do pracy ze stronami Razor aplikacji ASP.NET, w tym funkcji IntelliSense i debugowania.</span><span class="sxs-lookup"><span data-stu-id="f17d0-109">How to use features in Visual Studio to work with ASP.NET Razor pages, including IntelliSense and the debugger.</span></span>
>
>
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="f17d0-110">Wersje oprogramowania używanego w tym samouczku</span><span class="sxs-lookup"><span data-stu-id="f17d0-110">Software versions used in the tutorial</span></span>
>
>
> - <span data-ttu-id="f17d0-111">ASP.NET Web Pages (Razor) 3</span><span class="sxs-lookup"><span data-stu-id="f17d0-111">ASP.NET Web Pages (Razor) 3</span></span>
> - <span data-ttu-id="f17d0-112">Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="f17d0-112">Visual Studio 2013</span></span>
> - <span data-ttu-id="f17d0-113">Program WebMatrix 3</span><span class="sxs-lookup"><span data-stu-id="f17d0-113">WebMatrix 3</span></span>
>
>
> <span data-ttu-id="f17d0-114">W tym samouczku współpracuje również z wzorca ASP.NET Web Pages 2 programu Visual Studio 2012, Visual Studio 2010 i programu WebMatrix 2.</span><span class="sxs-lookup"><span data-stu-id="f17d0-114">This tutorial also works with ASP.NET Web Pages 2, Visual Studio 2012, Visual Studio 2010, and WebMatrix 2.</span></span>


<span data-ttu-id="f17d0-115">Można programować wzorca ASP.NET Web pages o składni Razor za pomocą programu WebMatrix lub innych edytorów kodu.</span><span class="sxs-lookup"><span data-stu-id="f17d0-115">You can program ASP.NET Web pages with Razor syntax using WebMatrix or many other code editors.</span></span> <span data-ttu-id="f17d0-116">Można również użyć programu Microsoft Visual Studio, który jest w pełni funkcjonalne zintegrowanym środowisku programistycznym (IDE), która udostępnia zaawansowany zestaw narzędzi do tworzenia wielu rodzajach aplikacji (nie tylko witryn sieci Web).</span><span class="sxs-lookup"><span data-stu-id="f17d0-116">You can also use Microsoft Visual Studio which is a full-featured integrated development environment (IDE) that provides a powerful set of tools for creating many types of applications (not just websites).</span></span> <span data-ttu-id="f17d0-117">Aby pracować ze stronami Razor aplikacji ASP.NET, możesz użyć [programu Visual Studio 2017](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=button+cta&utm_content=download+vs2017).</span><span class="sxs-lookup"><span data-stu-id="f17d0-117">To work with ASP.NET Razor pages, you can use [Visual Studio 2017](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=button+cta&utm_content=download+vs2017).</span></span>

<span data-ttu-id="f17d0-118">Dostępne są następujące dwie szczególnie przydatne funkcje, które program Visual Studio udostępnia do programowania w języku ASP.NET Razor strony sieci web:</span><span class="sxs-lookup"><span data-stu-id="f17d0-118">Two particularly useful features that Visual Studio provides for programming with ASP.NET Razor web pages are:</span></span>

- <span data-ttu-id="f17d0-119">*Funkcja IntelliSense*.</span><span class="sxs-lookup"><span data-stu-id="f17d0-119">*IntelliSense*.</span></span> <span data-ttu-id="f17d0-120">Funkcja IntelliSense, wbudowany w program Visual Studio jest większe niż funkcja IntelliSense w programie WebMatrix.</span><span class="sxs-lookup"><span data-stu-id="f17d0-120">The IntelliSense feature built into Visual Studio is more comprehensive than IntelliSense in WebMatrix.</span></span>
- <span data-ttu-id="f17d0-121">*Debuger*.</span><span class="sxs-lookup"><span data-stu-id="f17d0-121">*Debugger*.</span></span> <span data-ttu-id="f17d0-122">Debuger umożliwia rozwiązywanie problemów z kodu przez zatrzymanie programu, gdy jest uruchomiona, badanie zmiennych i krokowe wykonywanie kodu wiersz po wierszu.</span><span class="sxs-lookup"><span data-stu-id="f17d0-122">The debugger lets you troubleshoot your code by stopping a program while it's running, examining variables, and stepping through the code line by line.</span></span>

## <a name="using-visual-studio-with-different-versions-of-aspnet-web-pages"></a><span data-ttu-id="f17d0-123">Za pomocą programu Visual Studio z użyciem różnych wersji wzorca ASP.NET Web Pages</span><span class="sxs-lookup"><span data-stu-id="f17d0-123">Using Visual Studio with Different Versions of ASP.NET Web Pages</span></span>

<span data-ttu-id="f17d0-124">Aby tworzyć aplikacje sieci web ASP.NET w programie Visual Studio 2017, należy zainstalować **ASP.NET i tworzenie aplikacji internetowych** obciążenia.</span><span class="sxs-lookup"><span data-stu-id="f17d0-124">To develop ASP.NET web apps in Visual Studio 2017, install the **ASP.NET and web development** workload.</span></span>

<span data-ttu-id="f17d0-125">Visual Studio 2012 i Visual Studio 2013 obejmują obsługę składnika ASP.NET Web Pages.</span><span class="sxs-lookup"><span data-stu-id="f17d0-125">Visual Studio 2012 and Visual Studio 2013 include support for ASP.NET Web Pages.</span></span> <span data-ttu-id="f17d0-126">(Pakiety, które są wymagane do obsługi stron ASP.NET Web Pages są instalowane podczas instalowania programu Visual Studio).</span><span class="sxs-lookup"><span data-stu-id="f17d0-126">(The packages that are required in order to support ASP.NET Web Pages are installed when you install Visual Studio.)</span></span>

<span data-ttu-id="f17d0-127">Program Visual Studio 2010 nie obejmuje pomocy technicznej domyślnie dla stron ASP.NET Web Pages.</span><span class="sxs-lookup"><span data-stu-id="f17d0-127">Visual Studio 2010 does not include support by default for ASP.NET Web Pages.</span></span> <span data-ttu-id="f17d0-128">Strony sieci Web ASP.NET za pomocą programu Visual Studio 2010, należy zainstalować pakiet platformy ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="f17d0-128">To use ASP.NET Web Pages with Visual Studio 2010, you must install the ASP.NET MVC package.</span></span> <span data-ttu-id="f17d0-129">Aby uzyskać ASP.NET Web Pages 2, należy zainstalować platformy ASP.NET MVC 4.</span><span class="sxs-lookup"><span data-stu-id="f17d0-129">To get ASP.NET Web Pages 2, you install ASP.NET MVC 4.</span></span>

<span data-ttu-id="f17d0-130">Poniższa tabela zawiera podsumowanie obsługi dla stron ASP.NET Web Pages w różnych wersjach programu Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="f17d0-130">The following table summarizes the support for ASP.NET Web Pages in different versions of Visual Studio.</span></span>

|  | <span data-ttu-id="f17d0-131">Visual Studio 2010</span><span class="sxs-lookup"><span data-stu-id="f17d0-131">Visual Studio 2010</span></span> | <span data-ttu-id="f17d0-132">Visual Studio 2012</span><span class="sxs-lookup"><span data-stu-id="f17d0-132">Visual Studio 2012</span></span> | <span data-ttu-id="f17d0-133">Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="f17d0-133">Visual Studio 2013</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="f17d0-134">**ASP.NET Web Pages 2**</span><span class="sxs-lookup"><span data-stu-id="f17d0-134">**ASP.NET Web Pages 2**</span></span> | <span data-ttu-id="f17d0-135">Instalowanie platformy ASP.NET MVC 4</span><span class="sxs-lookup"><span data-stu-id="f17d0-135">Install ASP.NET MVC 4</span></span> | <span data-ttu-id="f17d0-136">(Dołączone)</span><span class="sxs-lookup"><span data-stu-id="f17d0-136">(Included)</span></span> | <span data-ttu-id="f17d0-137">(Dołączone)</span><span class="sxs-lookup"><span data-stu-id="f17d0-137">(Included)</span></span> |
| <span data-ttu-id="f17d0-138">**ASP.NET Web Pages 3**</span><span class="sxs-lookup"><span data-stu-id="f17d0-138">**ASP.NET Web Pages 3**</span></span> |  | <span data-ttu-id="f17d0-139">Aktualizacja sieci Web platformy ASP.NET strony 3 za pomocą NuGet</span><span class="sxs-lookup"><span data-stu-id="f17d0-139">Update to ASP.NET Web Pages 3 through NuGet</span></span> | <span data-ttu-id="f17d0-140">(Dołączone)</span><span class="sxs-lookup"><span data-stu-id="f17d0-140">(Included)</span></span> |

<span data-ttu-id="f17d0-141">Aby pracować z usługą Visual Studio 2010, zobacz [instalowania obsługi dla stron ASP.NET Web Pages w programie Visual Studio 2010](#vs2010support).</span><span class="sxs-lookup"><span data-stu-id="f17d0-141">To work with Visual Studio 2010, see [Installing Support for ASP.NET Web Pages in Visual Studio 2010](#vs2010support).</span></span>

## <a name="launching-visual-studio-from-webmatrix"></a><span data-ttu-id="f17d0-142">Uruchamianie programu Visual Studio z programu WebMatrix</span><span class="sxs-lookup"><span data-stu-id="f17d0-142">Launching Visual Studio from WebMatrix</span></span>

<span data-ttu-id="f17d0-143">Jeśli rozpoczęto projektu w programie WebMatrix i chcesz przełączyć się do programu Visual Studio, program WebMatrix udostępnia przycisku umożliwiającego łatwe otwieranie projektu w programie Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="f17d0-143">If you have started a project in WebMatrix and want to switch to Visual Studio, WebMatrix provides a button to easily open the project in Visual Studio.</span></span> <span data-ttu-id="f17d0-144">Konieczne jest posiadanie zainstalowanego programu Visual Studio na komputerze przez ten przycisk, aby włączyć.</span><span class="sxs-lookup"><span data-stu-id="f17d0-144">You must have Visual Studio installed on your computer for this button to be enabled.</span></span> <span data-ttu-id="f17d0-145">Na poniższej ilustracji przedstawiono przycisku w programie WebMatrix.</span><span class="sxs-lookup"><span data-stu-id="f17d0-145">The following image shows the button in WebMatrix.</span></span>

![Uruchom program Visual Studio](program-asp-net-web-pages-in-visual-studio/_static/image1.png)

<span data-ttu-id="f17d0-147">Po kliknięciu przycisku, projekt jest otwierany w programie Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="f17d0-147">When you click the button, the project is opened in Visual Studio.</span></span> <span data-ttu-id="f17d0-148">Możesz przełączać się i z powrotem między programu WebMatrix i Visual Studio bez problemów.</span><span class="sxs-lookup"><span data-stu-id="f17d0-148">You can switch back and forth between WebMatrix and Visual Studio without any problems.</span></span> <span data-ttu-id="f17d0-149">Użytkownik będzie powiadamiany, jeśli pliki zostały zmienione w innym środowisku, a trzeba będzie ponownie załadować, aby pobrać najnowsze zmiany.</span><span class="sxs-lookup"><span data-stu-id="f17d0-149">You will be notified if any files have changed in the other environment and need to be reloaded to get the latest changes.</span></span>

## <a name="creating-aspnet-razor-site-in-visual-studio"></a><span data-ttu-id="f17d0-150">Tworzenie witryny ASP.NET Razor w programie Visual Studio</span><span class="sxs-lookup"><span data-stu-id="f17d0-150">Creating ASP.NET Razor Site in Visual Studio</span></span>

<span data-ttu-id="f17d0-151">Aby utworzyć witrynę sieci Web ASP.NET Razor, w programie Visual Studio:</span><span class="sxs-lookup"><span data-stu-id="f17d0-151">To create an ASP.NET Razor website in Visual Studio:</span></span>

1. <span data-ttu-id="f17d0-152">Otwórz program Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="f17d0-152">Open Visual Studio.</span></span>
2. <span data-ttu-id="f17d0-153">W **pliku** menu, kliknij przycisk **nową witrynę sieci Web**.</span><span class="sxs-lookup"><span data-stu-id="f17d0-153">In the **File** menu, click **New Web Site**.</span></span>

    ![Utwórz nową witrynę sieci web](program-asp-net-web-pages-in-visual-studio/_static/image2.png)
3. <span data-ttu-id="f17d0-155">W **nową witrynę sieci Web** okna dialogowego Wybierz język (Visual C# lub Visual Basic).</span><span class="sxs-lookup"><span data-stu-id="f17d0-155">In the **New Web Site** dialog box, select the language to use (Visual C# or Visual Basic).</span></span>
4. <span data-ttu-id="f17d0-156">Wybierz **witryny sieci Web platformy ASP.NET (Razor)** szablonu.</span><span class="sxs-lookup"><span data-stu-id="f17d0-156">Select the **ASP.NET Web Site (Razor)** template.</span></span>

    ![Witryna razor](program-asp-net-web-pages-in-visual-studio/_static/image3.png)
5. <span data-ttu-id="f17d0-158">Kliknij przycisk **OK**.</span><span class="sxs-lookup"><span data-stu-id="f17d0-158">Click **OK**.</span></span>

<span data-ttu-id="f17d0-159">Nowy projekt istnieje i jest wypełniana przy użyciu niektóre domyślne strony sieci web, aby pomóc Ci rozpocząć pracę.</span><span class="sxs-lookup"><span data-stu-id="f17d0-159">Your new project exists and is populated with some default web pages to help you get started.</span></span>

### <a name="using-intellisense"></a><span data-ttu-id="f17d0-160">Korzystanie z IntelliSense</span><span class="sxs-lookup"><span data-stu-id="f17d0-160">Using IntelliSense</span></span>

<span data-ttu-id="f17d0-161">Teraz, po utworzeniu witryny, możesz zobaczyć, jak technologia IntelliSense działa w programie Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="f17d0-161">Now that you've created a site, you can see how IntelliSense works in Visual Studio.</span></span>

1. <span data-ttu-id="f17d0-162">W witrynie sieci Web został utworzony, otwórz *Default.cshtml* strony.</span><span class="sxs-lookup"><span data-stu-id="f17d0-162">In the website you just created, open the *Default.cshtml* page.</span></span>
2. <span data-ttu-id="f17d0-163">Po `<h3>` tagów na stronie, wpisz `@ServerInfo.` (w tym kropki (.)).</span><span class="sxs-lookup"><span data-stu-id="f17d0-163">After the `<h3>` tags in the page, type `@ServerInfo.` (including the dot).</span></span> <span data-ttu-id="f17d0-164">Zwróć uwagę, jak technologia IntelliSense wyświetla dostępne metody `ServerInfo` pomocnika na liście rozwijanej.</span><span class="sxs-lookup"><span data-stu-id="f17d0-164">Notice how IntelliSense displays the available methods for the `ServerInfo` helper in a drop-down list.</span></span>

    ![funkcja IntelliSense](program-asp-net-web-pages-in-visual-studio/_static/image4.png)
3. <span data-ttu-id="f17d0-166">Wybierz `GetHtml` metodę z listy, a następnie naciśnij klawisz Enter.</span><span class="sxs-lookup"><span data-stu-id="f17d0-166">Select the `GetHtml` method from the list and then press Enter.</span></span> <span data-ttu-id="f17d0-167">Funkcja IntelliSense automatycznie wypełnia metody.</span><span class="sxs-lookup"><span data-stu-id="f17d0-167">IntelliSense automatically fills in the method.</span></span> <span data-ttu-id="f17d0-168">(Przy użyciu dowolnej metody w języku C#, podczas dodawania `()` znaków po metodzie.) Kompletny kod dla `GetHtml` metoda wygląda następująco:</span><span class="sxs-lookup"><span data-stu-id="f17d0-168">(As with any method in C#, you must add `()` characters after the method.) The completed code for the `GetHtml` method looks like the following example:</span></span>

    [!code-cshtml[Main](program-asp-net-web-pages-in-visual-studio/samples/sample1.cshtml)]
4. <span data-ttu-id="f17d0-169">Naciśnij klawisze Ctrl + F5, aby uruchomić stronę.</span><span class="sxs-lookup"><span data-stu-id="f17d0-169">Press Ctrl+F5 to run the page.</span></span> <span data-ttu-id="f17d0-170">Jest to, jak wygląda podczas wyświetlania w przeglądarce strony:</span><span class="sxs-lookup"><span data-stu-id="f17d0-170">This is what the page looks like when displayed in a browser:</span></span>

    ![domyślną stronę w przeglądarce](program-asp-net-web-pages-in-visual-studio/_static/image5.png)
5. <span data-ttu-id="f17d0-172">Zamknij przeglądarkę.</span><span class="sxs-lookup"><span data-stu-id="f17d0-172">Close the browser.</span></span>

### <a name="using-the-debugger"></a><span data-ttu-id="f17d0-173">Za pomocą debugera</span><span class="sxs-lookup"><span data-stu-id="f17d0-173">Using the Debugger</span></span>

1. <span data-ttu-id="f17d0-174">W górnej części *Default.cshtml* strony po wierszu, który rozpoczyna się od `Page.Title`, Dodaj następujący wiersz kodu:</span><span class="sxs-lookup"><span data-stu-id="f17d0-174">At the top of the *Default.cshtml* page, after the line that begins with `Page.Title`, add the following line of code:</span></span>

    [!code-csharp[Main](program-asp-net-web-pages-in-visual-studio/samples/sample2.cs)]
2. <span data-ttu-id="f17d0-175">Szare marginesie po lewej stronie kodu w edytorze, kliknij przycisk Dalej, ten nowy wiersz. Aby można było dodać *punktu przerwania*.</span><span class="sxs-lookup"><span data-stu-id="f17d0-175">In the gray margin of the editor to the left of the code, click next to this new line in order to add a *breakpoint*.</span></span> <span data-ttu-id="f17d0-176">Punkt przerwania jest znacznik, który informuje debuger, aby zatrzymać program w tym momencie, dzięki czemu można zobaczyć, co się dzieje.</span><span class="sxs-lookup"><span data-stu-id="f17d0-176">A breakpoint is a marker that tells the debugger to stop running the program at that point so you can see what's happening.</span></span>

    ![Ustaw punkt przerwania](program-asp-net-web-pages-in-visual-studio/_static/image6.png)
3. <span data-ttu-id="f17d0-178">Usuń wywołanie funkcji `ServerInfo.GetHtml` metodę i dodaj wywołanie do `@myTime` zmiennej w tym miejscu.</span><span class="sxs-lookup"><span data-stu-id="f17d0-178">Remove the call to the `ServerInfo.GetHtml` method, and add a call to the `@myTime` variable in its place.</span></span> <span data-ttu-id="f17d0-179">To wywołanie powoduje wyświetlenie bieżącej wartości godziny, który jest zwracany przez nowy wiersz kodu.</span><span class="sxs-lookup"><span data-stu-id="f17d0-179">This call displays the current time value that's returned by the new line of code.</span></span>
4. <span data-ttu-id="f17d0-180">Naciśnij klawisz F5, aby uruchomić stronę w debugerze.</span><span class="sxs-lookup"><span data-stu-id="f17d0-180">Press F5 to run the page in the debugger.</span></span> <span data-ttu-id="f17d0-181">Strona zatrzymuje się na punkcie przerwania, który został ustawiony.</span><span class="sxs-lookup"><span data-stu-id="f17d0-181">The page stops on the breakpoint that you set.</span></span> <span data-ttu-id="f17d0-182">Na poniższej ilustracji przedstawiono, jak strona wygląda w edytorze za pomocą punktu przerwania (w kolorze żółtym).</span><span class="sxs-lookup"><span data-stu-id="f17d0-182">The following image shows what the page looks like in the editor with the breakpoint (in yellow).</span></span>

    ![punkt przerwania debugowania](program-asp-net-web-pages-in-visual-studio/_static/image7.png)
5. <span data-ttu-id="f17d0-184">Na pasku narzędzi debugowania kliknij **Step Into** przycisk (lub naciśnij klawisz F11) do uruchamiania następnego wiersza kodu.</span><span class="sxs-lookup"><span data-stu-id="f17d0-184">In the Debug toolbar, click the **Step Into** button (or press F11) to run the next line of code.</span></span> <span data-ttu-id="f17d0-185">Każde kliknięcie tego przycisku, wykonanie przejdź do następnego wiersza kodu.</span><span class="sxs-lookup"><span data-stu-id="f17d0-185">Each time you click this button, you advance the execution to the next line of code.</span></span>

    ![Wejdź do przycisku](program-asp-net-web-pages-in-visual-studio/_static/image8.png)
6. <span data-ttu-id="f17d0-187">Sprawdź wartość `myTime` zmiennej, przytrzymując wskaźnik myszy nad nim lub sprawdzając wartości wyświetlane w **lokalne** i **stos wywołań** systemu windows.</span><span class="sxs-lookup"><span data-stu-id="f17d0-187">Examine the value of the `myTime` variable by holding your mouse pointer over it or by inspecting the values displayed in the **Locals** and **Call Stack** windows.</span></span> <span data-ttu-id="f17d0-188">Program Visual Studio wyświetlić wartość zmiennej.</span><span class="sxs-lookup"><span data-stu-id="f17d0-188">Visual Studio display the value of the variable.</span></span>

    ![wartość czasu show](program-asp-net-web-pages-in-visual-studio/_static/image9.png)
7. <span data-ttu-id="f17d0-190">Gdy skończysz, sprawdzając zmienną i krokowe wykonywanie kodu, naciśnij klawisz F5, aby kontynuować wykonywanie strony bez zatrzymywania w każdym wierszu.</span><span class="sxs-lookup"><span data-stu-id="f17d0-190">When you're done examining the variable and stepping through code, press F5 to continue running the page without stopping at each line.</span></span> <span data-ttu-id="f17d0-191">Po zakończeniu przechodzeniu przez cały kod w przeglądarce pojawi się strona.</span><span class="sxs-lookup"><span data-stu-id="f17d0-191">When you've finished stepping through all the code, the browser displays the page.</span></span>

<span data-ttu-id="f17d0-192">Aby dowiedzieć się więcej na temat debugera i o tym, jak można debugować kodu w programie Visual Studio, zobacz [wskazówki: debugowanie stron sieci Web w Visual Web Developer](https://msdn.microsoft.com/library/z9e7w6cs.aspx).</span><span class="sxs-lookup"><span data-stu-id="f17d0-192">To learn more about the debugger and about how to debug code in Visual Studio, see [Walkthrough: Debugging Web Pages in Visual Web Developer](https://msdn.microsoft.com/library/z9e7w6cs.aspx).</span></span>

## <a name="using-razor-in-aspnet-mvc-projects-with-visual-studio"></a><span data-ttu-id="f17d0-193">Korzystanie z aparatu Razor w projektach ASP.NET MVC z programem Visual Studio</span><span class="sxs-lookup"><span data-stu-id="f17d0-193">Using Razor in ASP.NET MVC projects with Visual Studio</span></span>

<span data-ttu-id="f17d0-194">Składnia Razor jest również używany często w projektach ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="f17d0-194">The Razor syntax is also used extensively in ASP.NET MVC projects.</span></span> <span data-ttu-id="f17d0-195">MVC to zaawansowany, bazujący na wzorcach sposób tworzenia dynamicznych witryn sieci Web.</span><span class="sxs-lookup"><span data-stu-id="f17d0-195">MVC is a powerful, patterns-based way to build dynamic websites.</span></span> <span data-ttu-id="f17d0-196">Witryna ASP.NET Web Pages staje się trudne w utrzymaniu, warto wziąć pod uwagę podczas konwertowania go do aplikacji ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="f17d0-196">If your ASP.NET Web Pages site becomes difficult to maintain, you might want to consider converting it to an ASP.NET MVC application.</span></span> <span data-ttu-id="f17d0-197">Aby uzyskać przykład tworzenia aplikacji MVC, zobacz [wprowadzenie do ASP.NET MVC 5](../../../mvc/overview/getting-started/introduction/getting-started.md).</span><span class="sxs-lookup"><span data-stu-id="f17d0-197">For an example of creating an MVC application, see [Getting Started with ASP.NET MVC 5](../../../mvc/overview/getting-started/introduction/getting-started.md).</span></span>

<a id="vs2010support"></a>
## <a name="installing-support-for-aspnet-web-pages-in-visual-studio-2010"></a><span data-ttu-id="f17d0-198">Instalowanie obsługi dla stron sieci Web platformy ASP.NET w programie Visual Studio 2010</span><span class="sxs-lookup"><span data-stu-id="f17d0-198">Installing Support for ASP.NET Web Pages in Visual Studio 2010</span></span>

<span data-ttu-id="f17d0-199">W tej sekcji przedstawiono sposób instalowania narzędzia stron sieci Web platformy ASP.NET i Visual Web Developer Express 2010 dla programu Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="f17d0-199">This section shows how to install Visual Web Developer Express 2010 and the ASP.NET Web Pages Tools for Visual Studio.</span></span>

1. <span data-ttu-id="f17d0-200">Jeśli nie masz jeszcze Instalatora platformy sieci Web, należy ją pobrać z następującego adresu URL:</span><span class="sxs-lookup"><span data-stu-id="f17d0-200">If you don't already have the Web Platform Installer, download it from the following URL:</span></span>

    [https://www.microsoft.com/web/downloads/platform.aspx](https://www.microsoft.com/web/downloads/platform.aspx)
2. <span data-ttu-id="f17d0-201">Uruchom Instalatora platformy sieci Web.</span><span class="sxs-lookup"><span data-stu-id="f17d0-201">Run the Web Platform Installer.</span></span>
3. <span data-ttu-id="f17d0-202">Kliknij przycisk **produktów** kartę.</span><span class="sxs-lookup"><span data-stu-id="f17d0-202">Click the **Products** tab.</span></span>

    ![Karta produktów Instalatora WebPI](program-asp-net-web-pages-in-visual-studio/_static/image10.png)
4. <span data-ttu-id="f17d0-204">Wyszukaj **platformy ASP.NET MVC 4** (dla programu ASP.NET Web Pages 2) a następnie kliknij przycisk **Dodaj**.</span><span class="sxs-lookup"><span data-stu-id="f17d0-204">Search for **ASP.NET MVC 4** (for ASP.NET Web Pages 2) and then click **Add**.</span></span> <span data-ttu-id="f17d0-205">Produkty te obejmują narzędzia programu Visual Studio do tworzenia witryn sieci Web ASP.NET Razor.</span><span class="sxs-lookup"><span data-stu-id="f17d0-205">These products include Visual Studio tools for building ASP.NET Razor websites.</span></span>

    ![Opcje instalacji Instalatora WebPi](program-asp-net-web-pages-in-visual-studio/_static/image11.png)
5. <span data-ttu-id="f17d0-207">Kliknij przycisk **zainstalować** do ukończenia instalacji.</span><span class="sxs-lookup"><span data-stu-id="f17d0-207">Click **Install** to complete the installation.</span></span>
