---
uid: web-pages/overview/ui-layouts-and-themes/creating-and-using-a-helper-in-an-aspnet-web-pages-site
title: "Tworzenie i używanie pomocnika w sieci Web ASP.NET stron witryny (Razor) | Dokumentacja firmy Microsoft"
author: tfitzmac
description: "W tym artykule opisano sposób tworzenia pomocnika w witrynie sieci Web platformy ASP.NET Web Pages (Razor). Pomocnik jest składnikiem wielokrotnego użytku, który zawiera kod i znaczników do wydajności..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/17/2014
ms.topic: article
ms.assetid: 46bff772-01e0-40f0-9ae6-9e18c5442ee6
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/overview/ui-layouts-and-themes/creating-and-using-a-helper-in-an-aspnet-web-pages-site
msc.type: authoredcontent
ms.openlocfilehash: 5d0c1ae09d8fbc91ff76cd4045d439abafee7736
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/10/2017
---
<a name="creating-and-using-a-helper-in-an-aspnet-web-pages-razor-site"></a><span data-ttu-id="2f1d4-104">Tworzenie i używanie pomocnika w witryny ASP.NET Web Pages (Razor)</span><span class="sxs-lookup"><span data-stu-id="2f1d4-104">Creating and Using a Helper in an ASP.NET Web Pages (Razor) Site</span></span>
====================
<span data-ttu-id="2f1d4-105">przez [FitzMacken niestandardowy](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="2f1d4-105">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="2f1d4-106">W tym artykule opisano sposób tworzenia pomocnika w witrynie sieci Web platformy ASP.NET Web Pages (Razor).</span><span class="sxs-lookup"><span data-stu-id="2f1d4-106">This article describes how to create a helper in an ASP.NET Web Pages (Razor) website.</span></span> <span data-ttu-id="2f1d4-107">A *Pomocnika* jest składnikiem wielokrotnego użytku, który zawiera kod i znaczników do wykonania zadania, które mogą być niewygodny lub złożonych.</span><span class="sxs-lookup"><span data-stu-id="2f1d4-107">A *helper* is a reusable component that includes code and markup to perform a task that might be tedious or complex.</span></span>
> 
> <span data-ttu-id="2f1d4-108">**Zawartość:**</span><span class="sxs-lookup"><span data-stu-id="2f1d4-108">**What you'll learn:**</span></span> 
> 
> - <span data-ttu-id="2f1d4-109">Sposób tworzenia i używania prostego pomocnika.</span><span class="sxs-lookup"><span data-stu-id="2f1d4-109">How to create and use a simple helper.</span></span>
> 
> <span data-ttu-id="2f1d4-110">Poniżej przedstawiono funkcje platformy ASP.NET, wprowadzone w artykule:</span><span class="sxs-lookup"><span data-stu-id="2f1d4-110">These are the ASP.NET features introduced in the article:</span></span>
> 
> - <span data-ttu-id="2f1d4-111">`@helper` Składni.</span><span class="sxs-lookup"><span data-stu-id="2f1d4-111">The `@helper` syntax.</span></span>
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="2f1d4-112">Używane w samouczku wersje oprogramowania</span><span class="sxs-lookup"><span data-stu-id="2f1d4-112">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="2f1d4-113">Strony sieci Web platformy ASP.NET (Razor) 3</span><span class="sxs-lookup"><span data-stu-id="2f1d4-113">ASP.NET Web Pages (Razor) 3</span></span>
>   
> 
> <span data-ttu-id="2f1d4-114">W tym samouczku współdziała również z programu ASP.NET Web Pages 2.</span><span class="sxs-lookup"><span data-stu-id="2f1d4-114">This tutorial also works with ASP.NET Web Pages 2.</span></span>


## <a name="overview-of-helpers"></a><span data-ttu-id="2f1d4-115">Omówienie wątków</span><span class="sxs-lookup"><span data-stu-id="2f1d4-115">Overview of Helpers</span></span>

<span data-ttu-id="2f1d4-116">Jeśli trzeba wykonać te same zadania na różnych stronach w swojej witrynie, można użyć pomocnika.</span><span class="sxs-lookup"><span data-stu-id="2f1d4-116">If you need to perform the same tasks on different pages in your site, you can use a helper.</span></span> <span data-ttu-id="2f1d4-117">Strony sieci Web ASP.NET zawiera pewną liczbę wątków, a istnieje wiele innych, które można pobrać i zainstalować.</span><span class="sxs-lookup"><span data-stu-id="2f1d4-117">ASP.NET Web Pages includes a number of helpers, and there are many more that you can download and install.</span></span> <span data-ttu-id="2f1d4-118">(Listę wbudowanych pomocników stron ASP.NET Web Pages ma na liście [interfejsu API platformy ASP.NET — dokumentacja szybkie](https://go.microsoft.com/fwlink/?LinkId=202907).) Jeśli żadna istniejących pomocników nie spełnia Twoje potrzeby, można utworzyć własne pomocnika.</span><span class="sxs-lookup"><span data-stu-id="2f1d4-118">(A list of the built-in helpers in ASP.NET Web Pages is listed in the [ASP.NET API Quick Reference](https://go.microsoft.com/fwlink/?LinkId=202907).) If none of the existing helpers meet your needs, you can create your own helper.</span></span>

<span data-ttu-id="2f1d4-119">Obiekt pomocnika umożliwia używanie wspólnych bloku kodu na wielu stronach.</span><span class="sxs-lookup"><span data-stu-id="2f1d4-119">A helper lets you use a common block of code across multiple pages.</span></span> <span data-ttu-id="2f1d4-120">Załóżmy, że na stronie często zachodzi potrzeba utworzenia elementu Uwaga ustawiono oprócz normalne akapity.</span><span class="sxs-lookup"><span data-stu-id="2f1d4-120">Suppose that in your page you often want to create a note item that's set apart from normal paragraphs.</span></span> <span data-ttu-id="2f1d4-121">Prawdopodobnie uwagi zostanie utworzona jako `<div>` elementu który został wstawiony jako pole z obramowaniem.</span><span class="sxs-lookup"><span data-stu-id="2f1d4-121">Perhaps the note is created as a `<div>` element that's styled as a box with a border.</span></span> <span data-ttu-id="2f1d4-122">Zamiast za każdym razem, gdy ma być wyświetlana Uwaga, Dodaj ten sam kod znaczników do strony, można spakować znaczników jako pomocnika.</span><span class="sxs-lookup"><span data-stu-id="2f1d4-122">Rather than add this same markup to a page every time you want to display a note, you can package the markup as a helper.</span></span> <span data-ttu-id="2f1d4-123">Można wstawić Notatka w jednym wierszu kodu muszą dowolnego miejsca.</span><span class="sxs-lookup"><span data-stu-id="2f1d4-123">You can then insert the note with a single line of code anywhere you need it.</span></span>

<span data-ttu-id="2f1d4-124">Przy użyciu pomocnika, jak to sprawia, że kod w każdej strony prostszy i bardziej czytelne.</span><span class="sxs-lookup"><span data-stu-id="2f1d4-124">Using a helper like this makes the code in each of your pages simpler and easier to read.</span></span> <span data-ttu-id="2f1d4-125">On również ułatwia Obsługa witryny, ponieważ musisz zmienić wygląd uwagi, można zmienić znaczników w jednym miejscu.</span><span class="sxs-lookup"><span data-stu-id="2f1d4-125">It also makes it easier to maintain your site, because if you need to change how the notes look, you can change the markup in one place.</span></span>

## <a name="creating-a-helper"></a><span data-ttu-id="2f1d4-126">Tworzenie pomocnika</span><span class="sxs-lookup"><span data-stu-id="2f1d4-126">Creating a Helper</span></span>

<span data-ttu-id="2f1d4-127">Tej procedury przedstawiono sposób tworzenia pomocnika tworzącą Uwaga, zgodnie z opisem tylko.</span><span class="sxs-lookup"><span data-stu-id="2f1d4-127">This procedure shows you how to create the helper that creates the note, as just described.</span></span> <span data-ttu-id="2f1d4-128">Jest to prosty przykład, ale niestandardowe Pomocnik może zawierać żadnych znaczników i kodu platformy ASP.NET, które są potrzebne.</span><span class="sxs-lookup"><span data-stu-id="2f1d4-128">This is a simple example, but the custom helper can include any markup and ASP.NET code that you need.</span></span>

1. <span data-ttu-id="2f1d4-129">W folderze głównym witryny sieci Web utwórz folder o nazwie *aplikacji\_kod*.</span><span class="sxs-lookup"><span data-stu-id="2f1d4-129">In the root folder of the website, create a folder named *App\_Code*.</span></span> <span data-ttu-id="2f1d4-130">Jest to zarezerwowana nazwa folderu w programie ASP.NET, których można umieścić kod dla składników, takich jak pomocników.</span><span class="sxs-lookup"><span data-stu-id="2f1d4-130">This is a reserved folder name in ASP.NET where you can put code for components like helpers.</span></span>
2. <span data-ttu-id="2f1d4-131">W *aplikacji\_kod* Utwórz nowy folder *.cshtml* plik i nadaj mu nazwę *MyHelpers.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="2f1d4-131">In the *App\_Code* folder create a new *.cshtml* file and name it *MyHelpers.cshtml*.</span></span>
3. <span data-ttu-id="2f1d4-132">Zastąp istniejącą zawartość następujących czynności:</span><span class="sxs-lookup"><span data-stu-id="2f1d4-132">Replace the existing content with the following:</span></span>

    [!code-cshtml[Main](creating-and-using-a-helper-in-an-aspnet-web-pages-site/samples/sample1.cshtml)]

    <span data-ttu-id="2f1d4-133">W kodzie użyto `@helper` składni, aby zadeklarować nowy Pomocnik o nazwie `MakeNote`.</span><span class="sxs-lookup"><span data-stu-id="2f1d4-133">The code uses the `@helper` syntax to declare a new helper named `MakeNote`.</span></span> <span data-ttu-id="2f1d4-134">Tego konkretnego pomocnika umożliwia przekazywanie parametru o nazwie `content` zawierające kombinację tekst i znacznik.</span><span class="sxs-lookup"><span data-stu-id="2f1d4-134">This particular helper lets you pass a parameter named `content` that can contain a combination of text and markup.</span></span> <span data-ttu-id="2f1d4-135">Pomocnik wstawia ciąg do przy użyciu treści Uwaga `@content` zmiennej.</span><span class="sxs-lookup"><span data-stu-id="2f1d4-135">The helper inserts the string into the note body using the `@content` variable.</span></span>

    <span data-ttu-id="2f1d4-136">Zwróć uwagę, że plik ma nazwę *MyHelpers.cshtml*, ale nosi nazwę Pomocnika `MakeNote`.</span><span class="sxs-lookup"><span data-stu-id="2f1d4-136">Notice that the file is named *MyHelpers.cshtml*, but the helper is named `MakeNote`.</span></span> <span data-ttu-id="2f1d4-137">Wiele wątków niestandardowych można umieścić w jednym pliku.</span><span class="sxs-lookup"><span data-stu-id="2f1d4-137">You can put multiple custom helpers into a single file.</span></span>
4. <span data-ttu-id="2f1d4-138">Zapisz i zamknij plik.</span><span class="sxs-lookup"><span data-stu-id="2f1d4-138">Save and close the file.</span></span>

## <a name="using-the-helper-in-a-page"></a><span data-ttu-id="2f1d4-139">Przy użyciu pomocnika, na stronie</span><span class="sxs-lookup"><span data-stu-id="2f1d4-139">Using the Helper in a Page</span></span>

1. <span data-ttu-id="2f1d4-140">W folderze głównym, należy utworzyć nowy pusty plik o nazwie *TestHelper.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="2f1d4-140">In the root folder, create a new blank file called *TestHelper.cshtml*.</span></span>
2. <span data-ttu-id="2f1d4-141">Dodaj następujący kod do pliku:</span><span class="sxs-lookup"><span data-stu-id="2f1d4-141">Add the following code to the file:</span></span>

    [!code-html[Main](creating-and-using-a-helper-in-an-aspnet-web-pages-site/samples/sample2.html)]

    <span data-ttu-id="2f1d4-142">Aby wywołać pomocnika został utworzony, należy użyć `@` następuje nazwę pliku, w którym jest pomocnika, kropki, a następnie nazwę pomocnika.</span><span class="sxs-lookup"><span data-stu-id="2f1d4-142">To call the helper you created, use `@` followed by the file name where the helper is, a dot, and then the helper name.</span></span> <span data-ttu-id="2f1d4-143">(Jeśli masz wiele folderów *aplikacji\_kod* folderu, można użyć składni `@FolderName.FileName.HelperName` do wywołania Pomocnik żadnego zagnieżdżone na poziomie folderu).</span><span class="sxs-lookup"><span data-stu-id="2f1d4-143">(If you had multiple folders in the *App\_Code* folder, you could use the syntax `@FolderName.FileName.HelperName` to call your helper within any nested folder level).</span></span> <span data-ttu-id="2f1d4-144">Tekst, który należy dodać w znaki cudzysłowu w nawiasach jest tekst, który pomocnika będą wyświetlane jako część Uwaga na stronie sieci web.</span><span class="sxs-lookup"><span data-stu-id="2f1d4-144">The text that you add in quotation marks within the parentheses is the text that the helper will display as part of the note in the web page.</span></span>
3. <span data-ttu-id="2f1d4-145">Strony i uruchom go w przeglądarce.</span><span class="sxs-lookup"><span data-stu-id="2f1d4-145">Save the page and run it in a browser.</span></span> <span data-ttu-id="2f1d4-146">Pomocnik generuje elementu notatki prawo gdzie wywoływany pomocnika: między dwoma akapitów.</span><span class="sxs-lookup"><span data-stu-id="2f1d4-146">The helper generates the note item right where you called the helper: between the two paragraphs.</span></span>

    ![Zrzut ekranu przedstawiający stronę w przeglądarce i jak pomocnika generowany kod znaczników, który umieszcza pole wokół określony tekst.](creating-and-using-a-helper-in-an-aspnet-web-pages-site/_static/image1.jpg)

## <a name="additional-resources"></a><span data-ttu-id="2f1d4-148">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="2f1d4-148">Additional Resources</span></span>


<span data-ttu-id="2f1d4-149">[Poziomy menu jako pomocnika Razor](http://mikepope.com/blog/DisplayBlog.aspx?permalink=2341).</span><span class="sxs-lookup"><span data-stu-id="2f1d4-149">[Horizontal menu as a Razor helper](http://mikepope.com/blog/DisplayBlog.aspx?permalink=2341).</span></span> <span data-ttu-id="2f1d4-150">Ten wpis w blogu przez Pope Jan przedstawiono sposób tworzenia poziomy menu jako pomocnika przy użyciu znaczników, CSS i kod.</span><span class="sxs-lookup"><span data-stu-id="2f1d4-150">This blog entry by Mike Pope shows how to create a horizontal menu as a helper using markup, CSS, and code.</span></span>

<span data-ttu-id="2f1d4-151">[Wykorzystanie HTML5 w sieci Web ASP.NET strony pomocników dla programu WebMatrix i ASP.NET MVC 3](http://geekswithblogs.net/wildturtle/archive/2010/11/08/html5-in-asp.net-web-pages-helpers-for-webmatrix-and_aspnet_mvc3.aspx).</span><span class="sxs-lookup"><span data-stu-id="2f1d4-151">[Leveraging HTML5 in ASP.NET Web Pages Helpers for WebMatrix and ASP.NET MVC3](http://geekswithblogs.net/wildturtle/archive/2010/11/08/html5-in-asp.net-web-pages-helpers-for-webmatrix-and_aspnet_mvc3.aspx).</span></span> <span data-ttu-id="2f1d4-152">Ten wpis w blogu przez Sam Abraham zawiera pomocnika, który renderuje HTML5 `Canvas` elementu.</span><span class="sxs-lookup"><span data-stu-id="2f1d4-152">This blog entry by Sam Abraham shows a helper that renders an HTML5 `Canvas` element.</span></span>

<span data-ttu-id="2f1d4-153">[Różnica między @Helpers i @Functions w programie WebMatrix](http://www.mikesdotnetting.com/Article/173/The-Difference-Between-@Helpers-and-@Functions-In-WebMatrix).</span><span class="sxs-lookup"><span data-stu-id="2f1d4-153">[The Difference Between @Helpers and @Functions in WebMatrix](http://www.mikesdotnetting.com/Article/173/The-Difference-Between-@Helpers-and-@Functions-In-WebMatrix).</span></span> <span data-ttu-id="2f1d4-154">W tym artykule opisano ten wpis w blogu przez Brind Jan `@helper` składni i `@function` składni i kiedy należy używać każdego.</span><span class="sxs-lookup"><span data-stu-id="2f1d4-154">This blog entry by Mike Brind describes `@helper` syntax and `@function` syntax and when to use each.</span></span>
