---
uid: aspnet/web-pages/overview/getting-started/program-asp-net-web-pages-in-visual-studio
title: "Programowania w języku ASP.NET Web Pages (Razor) przy użyciu programu Visual Studio | Dokumentacja firmy Microsoft"
author: tfitzmac
description: "Ten dodatek wyjaśniono, jak można użyć programu Visual Studio 2010 lub Visual Web Developer 2010 Express programu ASP.NET Web Pages o składni Razor."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/13/2014
ms.topic: article
ms.assetid: 0acfec5a-48f2-4766-a801-a0f426966f0a
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/overview/getting-started/program-asp-net-web-pages-in-visual-studio
msc.type: authoredcontent
ms.openlocfilehash: 5cfeda206eda8fb3fd769d34fb40bae2c3b65093
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/10/2017
---
<a name="programming-aspnet-web-pages-razor-using-visual-studio"></a><span data-ttu-id="ad665-103">Programowanie stron sieci Web platformy ASP.NET (Razor) za pomocą programu Visual Studio</span><span class="sxs-lookup"><span data-stu-id="ad665-103">Programming ASP.NET Web Pages (Razor) Using Visual Studio</span></span>
====================
<span data-ttu-id="ad665-104">przez [FitzMacken niestandardowy](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="ad665-104">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="ad665-105">W tym artykule opisano, jak można użyć programu Visual Studio lub Visual Web Developer Express programu ASP.NET Web Pages (Razor) witryn sieci Web.</span><span class="sxs-lookup"><span data-stu-id="ad665-105">This article explains how you can use Visual Studio or Visual Web Developer Express to program ASP.NET Web Pages (Razor) websites.</span></span>
> 
> <span data-ttu-id="ad665-106">Dowiesz się</span><span class="sxs-lookup"><span data-stu-id="ad665-106">What you'll learn</span></span>
> 
> - <span data-ttu-id="ad665-107">Co jest potrzebne do zainstalowania (jeśli nic) do pracy z stron sieci Web ASP.NET w wersji programu Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="ad665-107">What you need to install (if anything) to work with ASP.NET Web Pages in your version of Visual Studio.</span></span>
> - <span data-ttu-id="ad665-108">Jak dodać obsługę dla stron sieci Web ASP.NET do programu Visual Web Developer 2010 Express.</span><span class="sxs-lookup"><span data-stu-id="ad665-108">How to add support for ASP.NET Web Pages to Visual Web Developer 2010 Express.</span></span>
> - <span data-ttu-id="ad665-109">Jak używać funkcji w programie Visual Studio, aby pracować ze stronami ASP.NET Razor, w tym IntelliSense i debuger.</span><span class="sxs-lookup"><span data-stu-id="ad665-109">How to use features in Visual Studio to work with ASP.NET Razor pages, including IntelliSense and the debugger.</span></span>
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="ad665-110">Używane w samouczku wersje oprogramowania</span><span class="sxs-lookup"><span data-stu-id="ad665-110">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="ad665-111">Strony sieci Web platformy ASP.NET (Razor) 3</span><span class="sxs-lookup"><span data-stu-id="ad665-111">ASP.NET Web Pages (Razor) 3</span></span>
> - <span data-ttu-id="ad665-112">Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="ad665-112">Visual Studio 2013</span></span>
> - <span data-ttu-id="ad665-113">Program WebMatrix 3</span><span class="sxs-lookup"><span data-stu-id="ad665-113">WebMatrix 3</span></span>
>   
> 
> <span data-ttu-id="ad665-114">W tym samouczku współdziała również z ASP.NET Web Pages 2 programu Visual Studio 2012, Visual Studio 2010 i programu WebMatrix 2.</span><span class="sxs-lookup"><span data-stu-id="ad665-114">This tutorial also works with ASP.NET Web Pages 2, Visual Studio 2012, Visual Studio 2010, and WebMatrix 2.</span></span>


<span data-ttu-id="ad665-115">Można programu stron ASP.NET Web pages o składni Razor przy użyciu programu WebMatrix lub wiele edytorów kodu.</span><span class="sxs-lookup"><span data-stu-id="ad665-115">You can program ASP.NET Web pages with Razor syntax using WebMatrix or many other code editors.</span></span> <span data-ttu-id="ad665-116">Można również użyć programu Microsoft Visual Studio, czyli kompletne zintegrowane środowisko programistyczne (IDE), który zapewnia zaawansowany zestaw narzędzi do tworzenia wielu typów aplikacji (nie tylko witryn sieci Web).</span><span class="sxs-lookup"><span data-stu-id="ad665-116">You can also use Microsoft Visual Studio which is a full-featured integrated development environment (IDE) that provides a powerful set of tools for creating many types of applications (not just websites).</span></span> <span data-ttu-id="ad665-117">Aby pracować ze stronami ASP.NET Razor, możesz albo użyj jednego z pełnej wersji programu Visual Studio lub wolnych [programu Visual Studio Express for Web](https://www.visualstudio.com/downloads/download-visual-studio-vs#d-2013-express) edition.</span><span class="sxs-lookup"><span data-stu-id="ad665-117">To work with ASP.NET Razor pages, you can either use one of the full editions of Visual Studio or the free [Visual Studio Express for Web](https://www.visualstudio.com/downloads/download-visual-studio-vs#d-2013-express) edition.</span></span>

<span data-ttu-id="ad665-118">Dostępne są następujące dwie szczególnie przydatne funkcje, które program Visual Studio udostępnia do programowania w języku ASP.NET Razor strony sieci web:</span><span class="sxs-lookup"><span data-stu-id="ad665-118">Two particularly useful features that Visual Studio provides for programming with ASP.NET Razor web pages are:</span></span>

- <span data-ttu-id="ad665-119">*IntelliSense*.</span><span class="sxs-lookup"><span data-stu-id="ad665-119">*IntelliSense*.</span></span> <span data-ttu-id="ad665-120">Funkcja IntelliSense wbudowanych w programie Visual Studio jest bardziej zaawansowane niż IntelliSense w programie WebMatrix.</span><span class="sxs-lookup"><span data-stu-id="ad665-120">The IntelliSense feature built into Visual Studio is more comprehensive than IntelliSense in WebMatrix.</span></span>
- <span data-ttu-id="ad665-121">*Debuger*.</span><span class="sxs-lookup"><span data-stu-id="ad665-121">*Debugger*.</span></span> <span data-ttu-id="ad665-122">Debuger umożliwia rozwiązywanie problemów z kodu przez zatrzymanie programu, gdy jest uruchomiona, sprawdzając zmiennych i krokowe wykonywanie kodu wiersz po wierszu.</span><span class="sxs-lookup"><span data-stu-id="ad665-122">The debugger lets you troubleshoot your code by stopping a program while it's running, examining variables, and stepping through the code line by line.</span></span>

## <a name="using-visual-studio-with-different-versions-of-aspnet-web-pages"></a><span data-ttu-id="ad665-123">Za pomocą programu Visual Studio z różnymi wersjami strony sieci Web ASP.NET</span><span class="sxs-lookup"><span data-stu-id="ad665-123">Using Visual Studio with Different Versions of ASP.NET Web Pages</span></span>

<span data-ttu-id="ad665-124">Visual Studio 2012 i Visual Studio 2013 obsługują dla stron sieci Web programu ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="ad665-124">Visual Studio 2012 and Visual Studio 2013 include support for ASP.NET Web Pages.</span></span> <span data-ttu-id="ad665-125">(Pakiety, które są wymagane w celu obsługi stron ASP.NET Web Pages są instalowane podczas instalowania programu Visual Studio).</span><span class="sxs-lookup"><span data-stu-id="ad665-125">(The packages that are required in order to support ASP.NET Web Pages are installed when you install Visual Studio.)</span></span>

<span data-ttu-id="ad665-126">Program Visual Studio 2010 nie obsługują domyślnie dla stron sieci Web programu ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="ad665-126">Visual Studio 2010 does not include support by default for ASP.NET Web Pages.</span></span> <span data-ttu-id="ad665-127">Aby strony sieci Web ASP.NET za pomocą programu Visual Studio 2010, należy zainstalować pakiet programu ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="ad665-127">To use ASP.NET Web Pages with Visual Studio 2010, you must install the ASP.NET MVC package.</span></span> <span data-ttu-id="ad665-128">Aby uzyskać ASP.NET Web Pages 2, należy zainstalować program ASP.NET MVC 4.</span><span class="sxs-lookup"><span data-stu-id="ad665-128">To get ASP.NET Web Pages 2, you install ASP.NET MVC 4.</span></span>

<span data-ttu-id="ad665-129">Poniższa tabela zawiera podsumowanie obsługi dla stron sieci Web ASP.NET w różnych wersjach programu Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="ad665-129">The following table summarizes the support for ASP.NET Web Pages in different versions of Visual Studio.</span></span>

|  | <span data-ttu-id="ad665-130">Visual Studio 2010</span><span class="sxs-lookup"><span data-stu-id="ad665-130">Visual Studio 2010</span></span> | <span data-ttu-id="ad665-131">Visual Studio 2012</span><span class="sxs-lookup"><span data-stu-id="ad665-131">Visual Studio 2012</span></span> | <span data-ttu-id="ad665-132">Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="ad665-132">Visual Studio 2013</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="ad665-133">**Strony ASP.NET Web Pages 2**</span><span class="sxs-lookup"><span data-stu-id="ad665-133">**ASP.NET Web Pages 2**</span></span> | <span data-ttu-id="ad665-134">Zainstaluj program ASP.NET MVC 4</span><span class="sxs-lookup"><span data-stu-id="ad665-134">Install ASP.NET MVC 4</span></span> | <span data-ttu-id="ad665-135">(Włączone)</span><span class="sxs-lookup"><span data-stu-id="ad665-135">(Included)</span></span> | <span data-ttu-id="ad665-136">(Włączone)</span><span class="sxs-lookup"><span data-stu-id="ad665-136">(Included)</span></span> |
| <span data-ttu-id="ad665-137">**Strony ASP.NET Web Pages 3**</span><span class="sxs-lookup"><span data-stu-id="ad665-137">**ASP.NET Web Pages 3**</span></span> |  | <span data-ttu-id="ad665-138">Aktualizacja do sieci Web ASP.NET strony 3 za pomocą NuGet</span><span class="sxs-lookup"><span data-stu-id="ad665-138">Update to ASP.NET Web Pages 3 through NuGet</span></span> | <span data-ttu-id="ad665-139">(Włączone)</span><span class="sxs-lookup"><span data-stu-id="ad665-139">(Included)</span></span> |

<span data-ttu-id="ad665-140">Aby pracować z programu Visual Studio 2010, zobacz [Instalowanie obsługi dla stron sieci Web ASP.NET w programie Visual Studio 2010](#vs2010support).</span><span class="sxs-lookup"><span data-stu-id="ad665-140">To work with Visual Studio 2010, see [Installing Support for ASP.NET Web Pages in Visual Studio 2010](#vs2010support).</span></span>

## <a name="launching-visual-studio-from-webmatrix"></a><span data-ttu-id="ad665-141">Uruchamianie programu Visual Studio za pomocą programu WebMatrix</span><span class="sxs-lookup"><span data-stu-id="ad665-141">Launching Visual Studio from WebMatrix</span></span>

<span data-ttu-id="ad665-142">Jeśli rozpoczęto projekt w programie WebMatrix i chcesz przełączyć się do programu Visual Studio, program WebMatrix udostępnia przycisk, aby łatwo Otwórz projekt w programie Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="ad665-142">If you have started a project in WebMatrix and want to switch to Visual Studio, WebMatrix provides a button to easily open the project in Visual Studio.</span></span> <span data-ttu-id="ad665-143">Musi mieć zainstalowanego programu Visual Studio na komputerze na tym przycisku włączenia.</span><span class="sxs-lookup"><span data-stu-id="ad665-143">You must have Visual Studio installed on your computer for this button to be enabled.</span></span> <span data-ttu-id="ad665-144">Na poniższej ilustracji przedstawiono przycisku w programie WebMatrix.</span><span class="sxs-lookup"><span data-stu-id="ad665-144">The following image shows the button in WebMatrix.</span></span>

![Uruchom program Visual Studio](program-asp-net-web-pages-in-visual-studio/_static/image1.png)

<span data-ttu-id="ad665-146">Po kliknięciu przycisku projektu jest otwarty w programie Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="ad665-146">When you click the button, the project is opened in Visual Studio.</span></span> <span data-ttu-id="ad665-147">Możesz można przełączać się między WebMatrix i Visual Studio bez problemów.</span><span class="sxs-lookup"><span data-stu-id="ad665-147">You can switch back and forth between WebMatrix and Visual Studio without any problems.</span></span> <span data-ttu-id="ad665-148">Jeśli wszystkie pliki zostały zmienione w innym środowisku i musi być załadowany ponownie, aby pobrać najnowsze zmiany, otrzymasz powiadomienie.</span><span class="sxs-lookup"><span data-stu-id="ad665-148">You will be notified if any files have changed in the other environment and need to be reloaded to get the latest changes.</span></span>

## <a name="creating-aspnet-razor-site-in-visual-studio"></a><span data-ttu-id="ad665-149">Tworzenie witryny ASP.NET Razor w programie Visual Studio</span><span class="sxs-lookup"><span data-stu-id="ad665-149">Creating ASP.NET Razor Site in Visual Studio</span></span>

<span data-ttu-id="ad665-150">Aby utworzyć witryny sieci Web platformy ASP.NET Razor w programie Visual Studio:</span><span class="sxs-lookup"><span data-stu-id="ad665-150">To create an ASP.NET Razor website in Visual Studio:</span></span>

1. <span data-ttu-id="ad665-151">Uruchom program Visual Studio lub Visual Web Developer.</span><span class="sxs-lookup"><span data-stu-id="ad665-151">Start Visual Studio or Visual Web Developer.</span></span>
2. <span data-ttu-id="ad665-152">W **pliku** menu, kliknij przycisk **nowej witryny sieci Web**.</span><span class="sxs-lookup"><span data-stu-id="ad665-152">In the **File** menu, click **New Web Site**.</span></span>

    ![Utwórz nową witrynę sieci web](program-asp-net-web-pages-in-visual-studio/_static/image2.png)
3. <span data-ttu-id="ad665-154">W **nowej witryny sieci Web** oknie dialogowym Wybierz język (Visual C# lub Visual Basic).</span><span class="sxs-lookup"><span data-stu-id="ad665-154">In the **New Web Site** dialog box, select the language to use (Visual C# or Visual Basic).</span></span>
4. <span data-ttu-id="ad665-155">Wybierz **witryny sieci Web platformy ASP.NET (Razor)** szablonu.</span><span class="sxs-lookup"><span data-stu-id="ad665-155">Select the **ASP.NET Web Site (Razor)** template.</span></span>

    ![razor lokacji](program-asp-net-web-pages-in-visual-studio/_static/image3.png)
5. <span data-ttu-id="ad665-157">Kliknij przycisk **OK**.</span><span class="sxs-lookup"><span data-stu-id="ad665-157">Click **OK**.</span></span>

<span data-ttu-id="ad665-158">Twój nowy projekt istnieje i jest wypełniana niektóre domyślne strony sieci web do ułatwiające rozpoczęcie pracy.</span><span class="sxs-lookup"><span data-stu-id="ad665-158">Your new project exists and is populated with some default web pages to help you get started.</span></span>

### <a name="using-intellisense"></a><span data-ttu-id="ad665-159">Korzystanie z IntelliSense</span><span class="sxs-lookup"><span data-stu-id="ad665-159">Using IntelliSense</span></span>

<span data-ttu-id="ad665-160">Teraz, po utworzeniu witryny widać, jak działa funkcja IntelliSense w Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="ad665-160">Now that you've created a site, you can see how IntelliSense works in Visual Studio.</span></span>

1. <span data-ttu-id="ad665-161">Witryna sieci Web został utworzony, otwórz *Default.cshtml* strony.</span><span class="sxs-lookup"><span data-stu-id="ad665-161">In the website you just created, open the *Default.cshtml* page.</span></span>
2. <span data-ttu-id="ad665-162">Po `<h3>` znaczników na stronie typu `@ServerInfo.` (w tym kropki (.)).</span><span class="sxs-lookup"><span data-stu-id="ad665-162">After the `<h3>` tags in the page, type `@ServerInfo.` (including the dot).</span></span> <span data-ttu-id="ad665-163">Zwróć uwagę, jak IntelliSense wyświetla dostępne metody `ServerInfo` pomocnika na liście rozwijanej.</span><span class="sxs-lookup"><span data-stu-id="ad665-163">Notice how IntelliSense displays the available methods for the `ServerInfo` helper in a drop-down list.</span></span> 

    ![IntelliSense](program-asp-net-web-pages-in-visual-studio/_static/image4.png)
3. <span data-ttu-id="ad665-165">Wybierz `GetHtml` metodę z listy, a następnie naciśnij klawisz Enter.</span><span class="sxs-lookup"><span data-stu-id="ad665-165">Select the `GetHtml` method from the list and then press Enter.</span></span> <span data-ttu-id="ad665-166">IntelliSense automatycznie wypełnia metody.</span><span class="sxs-lookup"><span data-stu-id="ad665-166">IntelliSense automatically fills in the method.</span></span> <span data-ttu-id="ad665-167">(Przy użyciu dowolnej metody w języku C#, podczas dodawania `()` po metodzie znaki.)</span><span class="sxs-lookup"><span data-stu-id="ad665-167">(As with any method in C#, you must add `()` characters after the method.)</span></span>  
 <span data-ttu-id="ad665-168">Kompletny kod dla `GetHtml` metody wygląda następująco:</span><span class="sxs-lookup"><span data-stu-id="ad665-168">The completed code for the `GetHtml` method looks like the following example:</span></span>  

    [!code-cshtml[Main](program-asp-net-web-pages-in-visual-studio/samples/sample1.cshtml)]
4. <span data-ttu-id="ad665-169">Naciśnij klawisze Ctrl + F5, aby uruchomić strony.</span><span class="sxs-lookup"><span data-stu-id="ad665-169">Press Ctrl+F5 to run the page.</span></span> <span data-ttu-id="ad665-170">Jest to, jak podczas wyświetlania w przeglądarce wygląda strony:</span><span class="sxs-lookup"><span data-stu-id="ad665-170">This is what the page looks like when displayed in a browser:</span></span> 

    ![domyślną stronę w przeglądarce](program-asp-net-web-pages-in-visual-studio/_static/image5.png)
5. <span data-ttu-id="ad665-172">Zamknij przeglądarkę.</span><span class="sxs-lookup"><span data-stu-id="ad665-172">Close the browser.</span></span>

### <a name="using-the-debugger"></a><span data-ttu-id="ad665-173">Korzystanie z debugera</span><span class="sxs-lookup"><span data-stu-id="ad665-173">Using the Debugger</span></span>

1. <span data-ttu-id="ad665-174">W górnej części *Default.cshtml* strony po wierszu, który rozpoczyna się od `Page.Title`, Dodaj następujący wiersz kodu:</span><span class="sxs-lookup"><span data-stu-id="ad665-174">At the top of the *Default.cshtml* page, after the line that begins with `Page.Title`, add the following line of code:</span></span> 

    [!code-csharp[Main](program-asp-net-web-pages-in-visual-studio/samples/sample2.cs)]
2. <span data-ttu-id="ad665-175">Szara marginesie edytora kodu w lewo, kliknij przycisk Dalej, nowego wiersza w celu dodania *punktu przerwania*.</span><span class="sxs-lookup"><span data-stu-id="ad665-175">In the gray margin of the editor to the left of the code, click next to this new line in order to add a *breakpoint*.</span></span> <span data-ttu-id="ad665-176">Punkt przerwania jest znacznik informuje debugera przestanie działać program w tym momencie, tak aby było widać, co dzieje się.</span><span class="sxs-lookup"><span data-stu-id="ad665-176">A breakpoint is a marker that tells the debugger to stop running the program at that point so you can see what's happening.</span></span>

    ![Ustaw punkt przerwania](program-asp-net-web-pages-in-visual-studio/_static/image6.png)
3. <span data-ttu-id="ad665-178">Usuń wywołanie `ServerInfo.GetHtml` metody i dodaj wywołanie do `@myTime` zmiennej w jego miejscu.</span><span class="sxs-lookup"><span data-stu-id="ad665-178">Remove the call to the `ServerInfo.GetHtml` method, and add a call to the `@myTime` variable in its place.</span></span> <span data-ttu-id="ad665-179">To wywołanie Wyświetla bieżącą wartość czasu zwróconego przez nowy wiersz kodu.</span><span class="sxs-lookup"><span data-stu-id="ad665-179">This call displays the current time value that's returned by the new line of code.</span></span>
4. <span data-ttu-id="ad665-180">Naciśnij klawisz F5, aby uruchomić strony w debugerze.</span><span class="sxs-lookup"><span data-stu-id="ad665-180">Press F5 to run the page in the debugger.</span></span> <span data-ttu-id="ad665-181">Strona zatrzymuje punkt przerwania, które można ustawić.</span><span class="sxs-lookup"><span data-stu-id="ad665-181">The page stops on the breakpoint that you set.</span></span> <span data-ttu-id="ad665-182">Na poniższej ilustracji przedstawiono strony jak wygląda w edytorze z punktu przerwania (w żółty).</span><span class="sxs-lookup"><span data-stu-id="ad665-182">The following image shows what the page looks like in the editor with the breakpoint (in yellow).</span></span> 

    ![punkt przerwania debugowania](program-asp-net-web-pages-in-visual-studio/_static/image7.png)
5. <span data-ttu-id="ad665-184">Na pasku narzędzi debugowania, kliknij przycisk **Step Into** przycisku (lub naciśnij klawisz F11) do uruchomienia w następnym wierszu kodu.</span><span class="sxs-lookup"><span data-stu-id="ad665-184">In the Debug toolbar, click the **Step Into** button (or press F11) to run the next line of code.</span></span> <span data-ttu-id="ad665-185">Każdorazowo po kliknięciu tego przycisku wykonywanie przejście do następnego wiersza kodu.</span><span class="sxs-lookup"><span data-stu-id="ad665-185">Each time you click this button, you advance the execution to the next line of code.</span></span>

    ![Krok do przycisku](program-asp-net-web-pages-in-visual-studio/_static/image8.png)
6. <span data-ttu-id="ad665-187">Sprawdź wartość `myTime` zmiennej przez umieszczenie wskaźnika myszy nad nim lub sprawdzając wartości wyświetlane w **zmiennych lokalnych** i **stos wywołań** systemu windows.</span><span class="sxs-lookup"><span data-stu-id="ad665-187">Examine the value of the `myTime` variable by holding your mouse pointer over it or by inspecting the values displayed in the **Locals** and **Call Stack** windows.</span></span> <span data-ttu-id="ad665-188">Visual Studio wyświetlona wartość zmiennej.</span><span class="sxs-lookup"><span data-stu-id="ad665-188">Visual Studio display the value of the variable.</span></span>

    ![wartość czasu Pokaż](program-asp-net-web-pages-in-visual-studio/_static/image9.png)
7. <span data-ttu-id="ad665-190">Gdy wszystko będzie gotowe, sprawdzając zmiennej i krokowe wykonywanie kodu, naciśnij klawisz F5, aby kontynuować wykonywanie strony bez zatrzymywania w każdym wierszu.</span><span class="sxs-lookup"><span data-stu-id="ad665-190">When you're done examining the variable and stepping through code, press F5 to continue running the page without stopping at each line.</span></span> <span data-ttu-id="ad665-191">Po zakończeniu krokowe całego kodu, w przeglądarce zostanie wyświetlona strona.</span><span class="sxs-lookup"><span data-stu-id="ad665-191">When you've finished stepping through all the code, the browser displays the page.</span></span>

<span data-ttu-id="ad665-192">Aby dowiedzieć się więcej o debugera i informacje dotyczące debugowania kodu w programie Visual Studio, zobacz [wskazówki: debugowanie stron sieci Web w programie Visual Web Developer](https://msdn.microsoft.com/library/z9e7w6cs.aspx).</span><span class="sxs-lookup"><span data-stu-id="ad665-192">To learn more about the debugger and about how to debug code in Visual Studio, see [Walkthrough: Debugging Web Pages in Visual Web Developer](https://msdn.microsoft.com/library/z9e7w6cs.aspx).</span></span>

## <a name="using-razor-in-aspnet-mvc-projects-with-visual-studio"></a><span data-ttu-id="ad665-193">Przy użyciu Razor do projektów platformy ASP.NET MVC programu Visual Studio</span><span class="sxs-lookup"><span data-stu-id="ad665-193">Using Razor in ASP.NET MVC projects with Visual Studio</span></span>

<span data-ttu-id="ad665-194">Składnia Razor jest również używany często w projekty składnika ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="ad665-194">The Razor syntax is also used extensively in ASP.NET MVC projects.</span></span> <span data-ttu-id="ad665-195">MVC to zaawansowane, na podstawie wzorców sposób tworzenia dynamicznych witryn sieci Web.</span><span class="sxs-lookup"><span data-stu-id="ad665-195">MVC is a powerful, patterns-based way to build dynamic websites.</span></span> <span data-ttu-id="ad665-196">Witryny ASP.NET Web Pages staje się trudne w utrzymaniu, warto wziąć pod uwagę konwersją do aplikacji platformy ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="ad665-196">If your ASP.NET Web Pages site becomes difficult to maintain, you might want to consider converting it to an ASP.NET MVC application.</span></span> <span data-ttu-id="ad665-197">Na przykład tworzenia aplikacji MVC, zobacz [wprowadzenie do platformy ASP.NET MVC 5](../../../mvc/overview/getting-started/introduction/getting-started.md).</span><span class="sxs-lookup"><span data-stu-id="ad665-197">For an example of creating an MVC application, see [Getting Started with ASP.NET MVC 5](../../../mvc/overview/getting-started/introduction/getting-started.md).</span></span>

<a id="vs2010support"></a>
## <a name="installing-support-for-aspnet-web-pages-in-visual-studio-2010"></a><span data-ttu-id="ad665-198">Instalowanie obsługi dla stron sieci Web ASP.NET w programie Visual Studio 2010</span><span class="sxs-lookup"><span data-stu-id="ad665-198">Installing Support for ASP.NET Web Pages in Visual Studio 2010</span></span>

<span data-ttu-id="ad665-199">W tej sekcji przedstawiono sposób instalowania narzędzia stron sieci Web ASP.NET i Visual Web Developer Express 2010 dla programu Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="ad665-199">This section shows how to install Visual Web Developer Express 2010 and the ASP.NET Web Pages Tools for Visual Studio.</span></span>

1. <span data-ttu-id="ad665-200">Jeśli nie masz jeszcze Instalatora platformy sieci Web, należy go pobrać z następującego adresu URL:</span><span class="sxs-lookup"><span data-stu-id="ad665-200">If you don't already have the Web Platform Installer, download it from the following URL:</span></span>

    [<span data-ttu-id="ad665-201">https://www.microsoft.com/Web/downloads/platform.aspx</span><span class="sxs-lookup"><span data-stu-id="ad665-201">https://www.microsoft.com/web/downloads/platform.aspx</span></span>](https://www.microsoft.com/web/downloads/platform.aspx)
2. <span data-ttu-id="ad665-202">Uruchom Instalatora platformy sieci Web.</span><span class="sxs-lookup"><span data-stu-id="ad665-202">Run the Web Platform Installer.</span></span>
3. <span data-ttu-id="ad665-203">Kliknij przycisk **produktów** kartę.</span><span class="sxs-lookup"><span data-stu-id="ad665-203">Click the **Products** tab.</span></span>

    ![Karta WebPI produktów](program-asp-net-web-pages-in-visual-studio/_static/image10.png)
4. <span data-ttu-id="ad665-205">Wyszukaj **ASP.NET MVC 4** (dla programu ASP.NET Web Pages 2), a następnie kliknij przycisk **Dodaj**.</span><span class="sxs-lookup"><span data-stu-id="ad665-205">Search for **ASP.NET MVC 4** (for ASP.NET Web Pages 2) and then click **Add**.</span></span> <span data-ttu-id="ad665-206">Produkty te obejmują narzędzia programu Visual Studio do tworzenia witryn sieci Web ASP.NET Razor.</span><span class="sxs-lookup"><span data-stu-id="ad665-206">These products include Visual Studio tools for building ASP.NET Razor websites.</span></span>

    ![Opcje instalacji WebPi](program-asp-net-web-pages-in-visual-studio/_static/image11.png)
5. <span data-ttu-id="ad665-208">Kliknij przycisk **zainstalować** do ukończenia instalacji.</span><span class="sxs-lookup"><span data-stu-id="ad665-208">Click **Install** to complete the installation.</span></span>
