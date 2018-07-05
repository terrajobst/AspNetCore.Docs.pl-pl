---
uid: web-pages/overview/ui-layouts-and-themes/creating-and-using-a-helper-in-an-aspnet-web-pages-site
title: Tworzenie i używanie Pomocnika we wzorcu ASP.NET Web Pages (Razor) lokacji | Dokumentacja firmy Microsoft
author: tfitzmac
description: W tym artykule opisano sposób tworzenia pomocnika w witrynie internetowej ASP.NET Web Pages (Razor). Pomocnik jest komponentów wielokrotnego użytku, obejmującą kodu i znaczników w celu wydajności...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/17/2014
ms.topic: article
ms.assetid: 46bff772-01e0-40f0-9ae6-9e18c5442ee6
ms.technology: dotnet-webpages
msc.legacyurl: /web-pages/overview/ui-layouts-and-themes/creating-and-using-a-helper-in-an-aspnet-web-pages-site
msc.type: authoredcontent
ms.openlocfilehash: 5e73beabf973a20335112a0d7dc5ecb6b8817744
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/03/2018
ms.locfileid: "37394615"
---
<a name="creating-and-using-a-helper-in-an-aspnet-web-pages-razor-site"></a><span data-ttu-id="891ed-104">Tworzenie i używanie pomocnika w witrynie ASP.NET Web Pages (Razor)</span><span class="sxs-lookup"><span data-stu-id="891ed-104">Creating and Using a Helper in an ASP.NET Web Pages (Razor) Site</span></span>
====================
<span data-ttu-id="891ed-105">przez [Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="891ed-105">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="891ed-106">W tym artykule opisano sposób tworzenia pomocnika w witrynie internetowej ASP.NET Web Pages (Razor).</span><span class="sxs-lookup"><span data-stu-id="891ed-106">This article describes how to create a helper in an ASP.NET Web Pages (Razor) website.</span></span> <span data-ttu-id="891ed-107">A *Pomocnika* to składnik wielokrotnego użytku, który zawiera kod i znaczników w celu wykonania zadania, które mogą być uciążliwe lub złożonych.</span><span class="sxs-lookup"><span data-stu-id="891ed-107">A *helper* is a reusable component that includes code and markup to perform a task that might be tedious or complex.</span></span>
> 
> <span data-ttu-id="891ed-108">**Zawartość:**</span><span class="sxs-lookup"><span data-stu-id="891ed-108">**What you'll learn:**</span></span> 
> 
> - <span data-ttu-id="891ed-109">Jak utworzyć i używać prostych pomocnika.</span><span class="sxs-lookup"><span data-stu-id="891ed-109">How to create and use a simple helper.</span></span>
> 
> <span data-ttu-id="891ed-110">Poniżej przedstawiono funkcje platformy ASP.NET, wprowadzona w artykule:</span><span class="sxs-lookup"><span data-stu-id="891ed-110">These are the ASP.NET features introduced in the article:</span></span>
> 
> - <span data-ttu-id="891ed-111">`@helper` Składni.</span><span class="sxs-lookup"><span data-stu-id="891ed-111">The `@helper` syntax.</span></span>
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="891ed-112">Wersje oprogramowania używanego w tym samouczku</span><span class="sxs-lookup"><span data-stu-id="891ed-112">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="891ed-113">ASP.NET Web Pages (Razor) 3</span><span class="sxs-lookup"><span data-stu-id="891ed-113">ASP.NET Web Pages (Razor) 3</span></span>
>   
> 
> <span data-ttu-id="891ed-114">W tym samouczku współpracuje również z wzorca ASP.NET Web Pages 2.</span><span class="sxs-lookup"><span data-stu-id="891ed-114">This tutorial also works with ASP.NET Web Pages 2.</span></span>


## <a name="overview-of-helpers"></a><span data-ttu-id="891ed-115">Omówienie wątków</span><span class="sxs-lookup"><span data-stu-id="891ed-115">Overview of Helpers</span></span>

<span data-ttu-id="891ed-116">Jeśli trzeba wykonać te same zadania na różnych stronach w witrynie, można użyć pomocnika.</span><span class="sxs-lookup"><span data-stu-id="891ed-116">If you need to perform the same tasks on different pages in your site, you can use a helper.</span></span> <span data-ttu-id="891ed-117">ASP.NET Web Pages obejmuje pewną liczbę wątków, wiąże się z wielu innych, które można pobrać i zainstalować.</span><span class="sxs-lookup"><span data-stu-id="891ed-117">ASP.NET Web Pages includes a number of helpers, and there are many more that you can download and install.</span></span> <span data-ttu-id="891ed-118">(Lista wbudowanej pomocników stron ASP.NET Web Pages znajduje się w [interfejsu API platformy ASP.NET — krótki przewodnik](https://go.microsoft.com/fwlink/?LinkId=202907).) Jeśli żadne z istniejących pomocników odpowiada Twoim potrzebom, możesz utworzyć własne pomocnika.</span><span class="sxs-lookup"><span data-stu-id="891ed-118">(A list of the built-in helpers in ASP.NET Web Pages is listed in the [ASP.NET API Quick Reference](https://go.microsoft.com/fwlink/?LinkId=202907).) If none of the existing helpers meet your needs, you can create your own helper.</span></span>

<span data-ttu-id="891ed-119">Obiekt pomocnika umożliwia używanie typowych bloku kodu na wielu stronach.</span><span class="sxs-lookup"><span data-stu-id="891ed-119">A helper lets you use a common block of code across multiple pages.</span></span> <span data-ttu-id="891ed-120">Załóżmy, że na stronie często chcesz utworzyć element Uwaga, który jest ustawiony, oprócz normalnych akapitach.</span><span class="sxs-lookup"><span data-stu-id="891ed-120">Suppose that in your page you often want to create a note item that's set apart from normal paragraphs.</span></span> <span data-ttu-id="891ed-121">Przykład uwagi są tworzone jako `<div>` elementu, został wstawiony jako pole z obramowaniem.</span><span class="sxs-lookup"><span data-stu-id="891ed-121">Perhaps the note is created as a `<div>` element that's styled as a box with a border.</span></span> <span data-ttu-id="891ed-122">Zamiast dodać ten sam kod znaczników do strony za każdym razem, gdy chcesz wyświetlić uwagi, można spakować znaczników jako obiekt pomocnika.</span><span class="sxs-lookup"><span data-stu-id="891ed-122">Rather than add this same markup to a page every time you want to display a note, you can package the markup as a helper.</span></span> <span data-ttu-id="891ed-123">Następnie można wstawić notatki z jednego wiersza kodu potrzebne dowolne miejsce.</span><span class="sxs-lookup"><span data-stu-id="891ed-123">You can then insert the note with a single line of code anywhere you need it.</span></span>

<span data-ttu-id="891ed-124">Przy użyciu pomocnika, np. to sprawia, że kod w poszczególnych stron sieci prostsze i łatwiejsze do odczytania.</span><span class="sxs-lookup"><span data-stu-id="891ed-124">Using a helper like this makes the code in each of your pages simpler and easier to read.</span></span> <span data-ttu-id="891ed-125">Również ułatwia utrzymanie witryny, ponieważ jeśli trzeba zmienić wygląd uwagi, można zmienić znaczników w jednym miejscu.</span><span class="sxs-lookup"><span data-stu-id="891ed-125">It also makes it easier to maintain your site, because if you need to change how the notes look, you can change the markup in one place.</span></span>

## <a name="creating-a-helper"></a><span data-ttu-id="891ed-126">Tworzenie pomocnika</span><span class="sxs-lookup"><span data-stu-id="891ed-126">Creating a Helper</span></span>

<span data-ttu-id="891ed-127">Ta procedura pokazuje, jak utworzyć pomocnika, która tworzy Uwaga, jak opisano powyżej.</span><span class="sxs-lookup"><span data-stu-id="891ed-127">This procedure shows you how to create the helper that creates the note, as just described.</span></span> <span data-ttu-id="891ed-128">Jest to prosty przykład, ale niestandardowego elementu pomocniczego może zawierać żadnych znaczników i kodu platformy ASP.NET, które są potrzebne.</span><span class="sxs-lookup"><span data-stu-id="891ed-128">This is a simple example, but the custom helper can include any markup and ASP.NET code that you need.</span></span>

1. <span data-ttu-id="891ed-129">W folderze głównym w witrynie sieci Web utwórz folder o nazwie *aplikacji\_kodu*.</span><span class="sxs-lookup"><span data-stu-id="891ed-129">In the root folder of the website, create a folder named *App\_Code*.</span></span> <span data-ttu-id="891ed-130">Jest to zarezerwowana nazwa folderu w programie ASP.NET, gdzie można umieścić kod dla składników, takich jak pomocników.</span><span class="sxs-lookup"><span data-stu-id="891ed-130">This is a reserved folder name in ASP.NET where you can put code for components like helpers.</span></span>
2. <span data-ttu-id="891ed-131">W *aplikacji\_kodu* folderze utwórz nową *.cshtml* plik i nadaj mu nazwę *MyHelpers.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="891ed-131">In the *App\_Code* folder create a new *.cshtml* file and name it *MyHelpers.cshtml*.</span></span>
3. <span data-ttu-id="891ed-132">Zastąp istniejącą zawartość następujących czynności:</span><span class="sxs-lookup"><span data-stu-id="891ed-132">Replace the existing content with the following:</span></span>

    [!code-cshtml[Main](creating-and-using-a-helper-in-an-aspnet-web-pages-site/samples/sample1.cshtml)]

    <span data-ttu-id="891ed-133">Kod używa `@helper` składnia do deklarowania nowego pomocnika o nazwie `MakeNote`.</span><span class="sxs-lookup"><span data-stu-id="891ed-133">The code uses the `@helper` syntax to declare a new helper named `MakeNote`.</span></span> <span data-ttu-id="891ed-134">Tego konkretnego pomocnika pozwala przekazać parametr o nazwie `content` , może zawierać kombinację tekstu i znaczników.</span><span class="sxs-lookup"><span data-stu-id="891ed-134">This particular helper lets you pass a parameter named `content` that can contain a combination of text and markup.</span></span> <span data-ttu-id="891ed-135">Pomocnik wstawia ciąg znaków do treść notatki przy użyciu `@content` zmiennej.</span><span class="sxs-lookup"><span data-stu-id="891ed-135">The helper inserts the string into the note body using the `@content` variable.</span></span>

    <span data-ttu-id="891ed-136">Należy zauważyć, że plik ma nazwę *MyHelpers.cshtml*, ale nosi nazwę Pomocnika `MakeNote`.</span><span class="sxs-lookup"><span data-stu-id="891ed-136">Notice that the file is named *MyHelpers.cshtml*, but the helper is named `MakeNote`.</span></span> <span data-ttu-id="891ed-137">Wiele niestandardowych pomocników można umieścić w jednym pliku.</span><span class="sxs-lookup"><span data-stu-id="891ed-137">You can put multiple custom helpers into a single file.</span></span>
4. <span data-ttu-id="891ed-138">Zapisz i zamknij plik.</span><span class="sxs-lookup"><span data-stu-id="891ed-138">Save and close the file.</span></span>

## <a name="using-the-helper-in-a-page"></a><span data-ttu-id="891ed-139">Na stronie przy użyciu Pomocnika</span><span class="sxs-lookup"><span data-stu-id="891ed-139">Using the Helper in a Page</span></span>

1. <span data-ttu-id="891ed-140">W folderze głównym Utwórz nowy pusty plik o nazwie *TestHelper.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="891ed-140">In the root folder, create a new blank file called *TestHelper.cshtml*.</span></span>
2. <span data-ttu-id="891ed-141">Dodaj następujący kod do pliku:</span><span class="sxs-lookup"><span data-stu-id="891ed-141">Add the following code to the file:</span></span>

    [!code-html[Main](creating-and-using-a-helper-in-an-aspnet-web-pages-site/samples/sample2.html)]

    <span data-ttu-id="891ed-142">Aby wywołać pomocnika został utworzony, należy użyć `@` nazwę pliku, gdzie jest pomocnika, kropka, a następnie nazwa pomocnika.</span><span class="sxs-lookup"><span data-stu-id="891ed-142">To call the helper you created, use `@` followed by the file name where the helper is, a dot, and then the helper name.</span></span> <span data-ttu-id="891ed-143">(Jeśli masz wiele folderów *aplikacji\_kodu* folderu, można użyć składni `@FolderName.FileName.HelperName` wywołać Pomocnik żadnej zagnieżdżony poziom folderów).</span><span class="sxs-lookup"><span data-stu-id="891ed-143">(If you had multiple folders in the *App\_Code* folder, you could use the syntax `@FolderName.FileName.HelperName` to call your helper within any nested folder level).</span></span> <span data-ttu-id="891ed-144">Tekst, który możesz dodać w znaki cudzysłowu w nawiasach jest tekst, który pomocnika będą wyświetlane jako część notatki na stronie sieci web.</span><span class="sxs-lookup"><span data-stu-id="891ed-144">The text that you add in quotation marks within the parentheses is the text that the helper will display as part of the note in the web page.</span></span>
3. <span data-ttu-id="891ed-145">Zapisz stronę i uruchom go w przeglądarce.</span><span class="sxs-lookup"><span data-stu-id="891ed-145">Save the page and run it in a browser.</span></span> <span data-ttu-id="891ed-146">Pomocnik generuje elementu notatki bezpośrednio gdzie wywoływana pomocnika: między dwoma akapitami.</span><span class="sxs-lookup"><span data-stu-id="891ed-146">The helper generates the note item right where you called the helper: between the two paragraphs.</span></span>

    ![Zrzut ekranu przedstawiający stronę w przeglądarce i jak Pomocnik wygenerowany kod znaczników, który umieszcza otoczkę wokół pozycji określony tekst.](creating-and-using-a-helper-in-an-aspnet-web-pages-site/_static/image1.jpg)

## <a name="additional-resources"></a><span data-ttu-id="891ed-148">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="891ed-148">Additional Resources</span></span>


<span data-ttu-id="891ed-149">[Poziomy menu jako pomocnika Razor](http://mikepope.com/blog/DisplayBlog.aspx?permalink=2341).</span><span class="sxs-lookup"><span data-stu-id="891ed-149">[Horizontal menu as a Razor helper](http://mikepope.com/blog/DisplayBlog.aspx?permalink=2341).</span></span> <span data-ttu-id="891ed-150">Ten wpis w blogu przez Mike'a Pope przedstawia sposób tworzenia menu poziomie jako pomoc przy użyciu znaczników, CSS i kod.</span><span class="sxs-lookup"><span data-stu-id="891ed-150">This blog entry by Mike Pope shows how to create a horizontal menu as a helper using markup, CSS, and code.</span></span>

<span data-ttu-id="891ed-151">[Korzystanie z języka HTML5 we wzorcu ASP.NET Web Pages pomocników dla programu WebMatrix i wzorca ASP.NET MVC 3](http://geekswithblogs.net/wildturtle/archive/2010/11/08/html5-in-asp.net-web-pages-helpers-for-webmatrix-and_aspnet_mvc3.aspx).</span><span class="sxs-lookup"><span data-stu-id="891ed-151">[Leveraging HTML5 in ASP.NET Web Pages Helpers for WebMatrix and ASP.NET MVC3](http://geekswithblogs.net/wildturtle/archive/2010/11/08/html5-in-asp.net-web-pages-helpers-for-webmatrix-and_aspnet_mvc3.aspx).</span></span> <span data-ttu-id="891ed-152">Ten wpis w blogu przez Sam Abraham pokazuje pomocnika, który renderuje HTML5 `Canvas` elementu.</span><span class="sxs-lookup"><span data-stu-id="891ed-152">This blog entry by Sam Abraham shows a helper that renders an HTML5 `Canvas` element.</span></span>

<span data-ttu-id="891ed-153">[Różnica między @Helpers i @Functions w programie WebMatrix](http://www.mikesdotnetting.com/Article/173/The-Difference-Between-@Helpers-and-@Functions-In-WebMatrix).</span><span class="sxs-lookup"><span data-stu-id="891ed-153">[The Difference Between @Helpers and @Functions in WebMatrix](http://www.mikesdotnetting.com/Article/173/The-Difference-Between-@Helpers-and-@Functions-In-WebMatrix).</span></span> <span data-ttu-id="891ed-154">W tym artykule opisano ten wpis w blogu przez Mike'a Brind `@helper` składni i `@function` składni i kiedy należy używać każdego.</span><span class="sxs-lookup"><span data-stu-id="891ed-154">This blog entry by Mike Brind describes `@helper` syntax and `@function` syntax and when to use each.</span></span>
