---
uid: web-pages/overview/getting-started/introducing-aspnet-web-pages-2/layouts
title: Wprowadzenie do składnika ASP.NET Web Pages — tworzenie spójnego układu | Dokumentacja firmy Microsoft
author: tfitzmac
description: Ten samouczek pokazuje, jak używać układów do tworzenia spójny wygląd stron w lokacji, która używa strony sieci Web ASP.NET. Przyjęto założenie, że zostały wykonane...
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/28/2015
ms.topic: article
ms.assetid: c85ec591-f8d7-4882-b763-de6ab9f3df7a
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/overview/getting-started/introducing-aspnet-web-pages-2/layouts
msc.type: authoredcontent
ms.openlocfilehash: c2d5c4d8ed8a71979c16d484ab90d283a45de537
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/06/2018
---
<a name="introducing-aspnet-web-pages---creating-a-consistent-layout"></a><span data-ttu-id="cc432-104">Wprowadzenie do stron sieci Web ASP.NET - tworzenie spójnego układu</span><span class="sxs-lookup"><span data-stu-id="cc432-104">Introducing ASP.NET Web Pages - Creating a Consistent Layout</span></span>
====================
<span data-ttu-id="cc432-105">przez [FitzMacken niestandardowy](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="cc432-105">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="cc432-106">Ten samouczek przedstawia sposób użycia *układów* utworzyć spójny wygląd stron w lokacji, która używa strony sieci Web ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="cc432-106">This tutorial shows you how to use *layouts* to create a consistent look for the pages on a site that uses ASP.NET Web Pages.</span></span> <span data-ttu-id="cc432-107">Przyjęto założenie, że zostały wykonane serii za pomocą [usuwanie danych z baz danych w programie ASP.NET Web Pages](https://go.microsoft.com/fwlink/?LinkId=251584).</span><span class="sxs-lookup"><span data-stu-id="cc432-107">It assumes you have completed the series through [Deleting Database Data in ASP.NET Web Pages](https://go.microsoft.com/fwlink/?LinkId=251584).</span></span>
> 
> <span data-ttu-id="cc432-108">Zawartość:</span><span class="sxs-lookup"><span data-stu-id="cc432-108">What you'll learn:</span></span>
> 
> - <span data-ttu-id="cc432-109">Strona układu jest.</span><span class="sxs-lookup"><span data-stu-id="cc432-109">What a layout page is.</span></span>
> - <span data-ttu-id="cc432-110">Jak połączyć układ strony z zawartością dynamiczną.</span><span class="sxs-lookup"><span data-stu-id="cc432-110">How to combine layout pages with dynamic content.</span></span>
> - <span data-ttu-id="cc432-111">Jak przekazać wartości do strony układu.</span><span class="sxs-lookup"><span data-stu-id="cc432-111">How to pass values to a layout page.</span></span>


## <a name="about-layouts"></a><span data-ttu-id="cc432-112">Układy — informacje</span><span class="sxs-lookup"><span data-stu-id="cc432-112">About Layouts</span></span>

<span data-ttu-id="cc432-113">Strony po utworzeniu wykonanej do tej pory wszystkie zostały zakończone, stron autonomicznych.</span><span class="sxs-lookup"><span data-stu-id="cc432-113">The pages you've created so far have all been complete, standalone pages.</span></span> <span data-ttu-id="cc432-114">Wszystkie one należą do tej samej lokacji, ale nie ma wszystkie wspólne elementy lub wygląd normalny.</span><span class="sxs-lookup"><span data-stu-id="cc432-114">They all belong to the same site, but they don't have any common elements or a standard look.</span></span>

<span data-ttu-id="cc432-115">W większości witryn ma spójny wygląd i układu.</span><span class="sxs-lookup"><span data-stu-id="cc432-115">Most sites do have a consistent look and layout.</span></span> <span data-ttu-id="cc432-116">Na przykład, jeśli przejdziesz do [Microsoft.com/web](https://www.microsoft.com/web/) lokacji i szukać, zobaczysz, że wszystkie strony przestrzegają ogólny układ i motyw programu visual:</span><span class="sxs-lookup"><span data-stu-id="cc432-116">For example, if you go to the [Microsoft.com/web](https://www.microsoft.com/web/) site and look around, you see that the pages all adhere to an overall layout and to a visual theme:</span></span>

![Strona witryny Microsoft.com/Web przedstawiający układ nagłówka, obszar nawigacji, obszar zawartości i stopki](layouts/_static/image1.png)

<span data-ttu-id="cc432-118">*Nieefektywne* sposobem tworzenia tego układu byłoby zdefiniować nagłówek, na pasku nawigacyjnym i stopka osobno dla poszczególnych stron sieci.</span><span class="sxs-lookup"><span data-stu-id="cc432-118">An *inefficient* way to create this layout would be to define a header, navigation bar, and footer separately on each of your pages.</span></span> <span data-ttu-id="cc432-119">Użytkownik będzie można duplikowania znacznika tej samej zawsze.</span><span class="sxs-lookup"><span data-stu-id="cc432-119">You'd be duplicating the same markup each time.</span></span> <span data-ttu-id="cc432-120">Jeśli chcesz zmienić (na przykład aktualizacja stopki), należy zmienić oddzielnie każdej strony.</span><span class="sxs-lookup"><span data-stu-id="cc432-120">If you wanted to change something (for example, update the footer), you'd have to change each page separately.</span></span>

<span data-ttu-id="cc432-121">To miejsce *strony układu* się.</span><span class="sxs-lookup"><span data-stu-id="cc432-121">That's where *layout pages* come in.</span></span> <span data-ttu-id="cc432-122">W składniku ASP.NET Web Pages można zdefiniować stronę układu, która zapewnia ogólną kontener dla stron w witrynie.</span><span class="sxs-lookup"><span data-stu-id="cc432-122">In ASP.NET Web Pages, you can define a layout page that provides an overall container for pages on your site.</span></span> <span data-ttu-id="cc432-123">Na przykład strony układu może zawierać nagłówek, obszar nawigacji i stopce strony.</span><span class="sxs-lookup"><span data-stu-id="cc432-123">For example, the layout page can contain the header, navigation area, and footer.</span></span> <span data-ttu-id="cc432-124">Strona układu zawiera symbol zastępczy, gdzie należy wstawić zawartość głównego.</span><span class="sxs-lookup"><span data-stu-id="cc432-124">The layout page includes a placeholder where the main content goes.</span></span>

<span data-ttu-id="cc432-125">Można zdefiniować poszczególnych strony zawartości, które zawierają kod znaczników i kodu tylko na tej stronie.</span><span class="sxs-lookup"><span data-stu-id="cc432-125">You can then define individual content pages that contain the markup and the code for only that page.</span></span> <span data-ttu-id="cc432-126">Strony z zawartością nie muszą być ukończone stron HTML. nawet nie mają mieć `<body>` elementu.</span><span class="sxs-lookup"><span data-stu-id="cc432-126">Content pages don't have to be complete HTML pages; they don't even have to have a `<body>` element.</span></span> <span data-ttu-id="cc432-127">Mają one również wiersza kodu, który informuje, jakie układ strony, aby wyświetlić zawartość w ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="cc432-127">They also have a line of code that tells ASP.NET what layout page you want to display the content in.</span></span> <span data-ttu-id="cc432-128">Oto obraz, który zawiera około, jak działa ta relacja:</span><span class="sxs-lookup"><span data-stu-id="cc432-128">Here's a picture that shows roughly how this relationship works:</span></span>

![Diagram koncepcyjny przedstawiający dwie strony z zawartością i stronę układu, w którym mieściły się](layouts/_static/image2.png)

<span data-ttu-id="cc432-130">Ta interakcja jest łatwy do zrozumienia, gdy zostanie wyświetlony w akcji.</span><span class="sxs-lookup"><span data-stu-id="cc432-130">This interaction is easy to understand when you see it in action.</span></span> <span data-ttu-id="cc432-131">W tym samouczku zostanie zmieniona stronach filmy zastosować układ.</span><span class="sxs-lookup"><span data-stu-id="cc432-131">In this tutorial, you'll change your movies pages to use a layout.</span></span>

## <a name="adding-a-layout-page"></a><span data-ttu-id="cc432-132">Dodawanie strony układu</span><span class="sxs-lookup"><span data-stu-id="cc432-132">Adding a Layout Page</span></span>

<span data-ttu-id="cc432-133">Będzie rozpoczyna się od utworzenia strony układu, który definiuje układ strony typowe nagłówek, stopka i obszaru głównego zawartości.</span><span class="sxs-lookup"><span data-stu-id="cc432-133">You'll start by creating a layout page that defines a typical page layout with a header, footer, and an area for the main content.</span></span> <span data-ttu-id="cc432-134">W witrynie WebPagesMovies Dodaj stronę CSHTML o nazwie  *\_Layout.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="cc432-134">In the WebPagesMovies site, add a CSHTML page named *\_Layout.cshtml*.</span></span>

<span data-ttu-id="cc432-135">Wiodące znak podkreślenia ( `_` ) znak jest znacząca.</span><span class="sxs-lookup"><span data-stu-id="cc432-135">The leading underscore ( `_` ) character is significant.</span></span> <span data-ttu-id="cc432-136">Jeśli na stronie nazwa rozpoczyna się od znaku podkreślenia, program ASP.NET nie będzie wysyłać bezpośrednio tej strony w przeglądarce.</span><span class="sxs-lookup"><span data-stu-id="cc432-136">If a page's name starts with an underscore, ASP.NET won't directly send that page to the browser.</span></span> <span data-ttu-id="cc432-137">Tę Konwencję pozwala zdefiniować stron, które są wymagane dla witryny, ale aby użytkownicy nie powinien móc żądać bezpośrednio.</span><span class="sxs-lookup"><span data-stu-id="cc432-137">This convention lets you define pages that are required for your site but that users shouldn't be able to request directly.</span></span>

<span data-ttu-id="cc432-138">Zamień zawartość na stronie następujące czynności:</span><span class="sxs-lookup"><span data-stu-id="cc432-138">Replace the content in the page with the following:</span></span>

[!code-html[Main](layouts/samples/sample1.html)]

<span data-ttu-id="cc432-139">Jak widać, ten kod znaczników jest po prostu HTML, który używa `<div>` elementy, aby zdefiniować trzy sekcje w stronie plus jeden więcej `<div>` element, aby pomieścić trzy sekcje.</span><span class="sxs-lookup"><span data-stu-id="cc432-139">As you can see, this markup is just HTML that uses `<div>` elements to define three sections in the page plus one more `<div>` element to hold the three sections.</span></span> <span data-ttu-id="cc432-140">Stopki zawiera bitowego kodu Razor: `@DateTime.Now.Year`, który będzie renderowany w bieżącym roku w tej lokalizacji na stronie.</span><span class="sxs-lookup"><span data-stu-id="cc432-140">The footer contains a bit of Razor code: `@DateTime.Now.Year`, which will render the current year at that location in the page.</span></span>

<span data-ttu-id="cc432-141">Zwróć uwagę, że istnieje łącze do arkusza stylów o nazwie *Movies.css*.</span><span class="sxs-lookup"><span data-stu-id="cc432-141">Notice that there's a link to a style sheet named *Movies.css*.</span></span> <span data-ttu-id="cc432-142">Arkusz stylów jest, gdzie można zdefiniować szczegóły fizycznego układu elementów.</span><span class="sxs-lookup"><span data-stu-id="cc432-142">The style sheet is where the details of the physical layout of the elements will be defined.</span></span> <span data-ttu-id="cc432-143">Utworzysz którego za chwilę.</span><span class="sxs-lookup"><span data-stu-id="cc432-143">You'll create that in a moment.</span></span>

<span data-ttu-id="cc432-144">Tylko nietypowe funkcji w tym  *\_Layout.cshtml* strona jest `@Render.Body()` wiersza.</span><span class="sxs-lookup"><span data-stu-id="cc432-144">The only unusual feature in this *\_Layout.cshtml* page is the `@Render.Body()` line.</span></span> <span data-ttu-id="cc432-145">To symbol zastępczy, gdzie zawartość, gdy ten układ jest scalany z innej strony.</span><span class="sxs-lookup"><span data-stu-id="cc432-145">That's the placeholder where the content will go when this layout is merged with another page.</span></span>

## <a name="adding-a-css-file"></a><span data-ttu-id="cc432-146">Dodawanie pliku CSS</span><span class="sxs-lookup"><span data-stu-id="cc432-146">Adding a .css File</span></span>

<span data-ttu-id="cc432-147">Preferowany sposób definiowania rzeczywisty układ (to znaczy wygląd) elementów na stronie jest Użyj reguł stylu kaskadowych w arkuszu (CSS).</span><span class="sxs-lookup"><span data-stu-id="cc432-147">The preferred way to define the actual arrangement (that is, appearance) of elements on the page is to use cascading style sheet (CSS) rules.</span></span> <span data-ttu-id="cc432-148">Dlatego należy utworzyć *.css* pliku reguł dla nowego układu.</span><span class="sxs-lookup"><span data-stu-id="cc432-148">So you'll create a *.css* file that has the rules for your new layout.</span></span>

<span data-ttu-id="cc432-149">W programie WebMatrix wybierz katalog główny witryny.</span><span class="sxs-lookup"><span data-stu-id="cc432-149">In WebMatrix, select the root of your site.</span></span> <span data-ttu-id="cc432-150">Następnie w **pliki** kartę na wstążce kliknij strzałkę w obszarze **nowy** przycisk, a następnie kliknij przycisk **nowy Folder**.</span><span class="sxs-lookup"><span data-stu-id="cc432-150">Then in the **Files** tab of the ribbon, click the arrow under the **New** button and then click **New Folder**.</span></span>

![Opcja "Nowy Folder" obszarze Nowy na Wstążce.](layouts/_static/image3.png)

<span data-ttu-id="cc432-152">Nazwa nowego folderu *style*.</span><span class="sxs-lookup"><span data-stu-id="cc432-152">Name the new folder *Styles*.</span></span>

![Nazewnictwo nowy folder "Style"](layouts/_static/image4.png)

<span data-ttu-id="cc432-154">Wewnątrz nowe *style* folderu, Utwórz plik o nazwie *Movies.css*.</span><span class="sxs-lookup"><span data-stu-id="cc432-154">Inside the new *Styles* folder, create a file named *Movies.css*.</span></span>

![Tworzenie nowego pliku Movies.css](layouts/_static/image5.png)

<span data-ttu-id="cc432-156">Zastąp zawartość nowego *.css* pliku następującym kodem:</span><span class="sxs-lookup"><span data-stu-id="cc432-156">Replace the contents of the new *.css* file with the following:</span></span>

[!code-css[Main](layouts/samples/sample2.css)]

<span data-ttu-id="cc432-157">Firma Microsoft nie będzie, że te informacje te reguły CSS, z wyjątkiem dwie czynności Pamiętaj, aby.</span><span class="sxs-lookup"><span data-stu-id="cc432-157">We won't say much about these CSS rules, except to note two things.</span></span> <span data-ttu-id="cc432-158">Jeden jest poza ustawieniem czcionki oraz rozmiary, reguły wykorzystania bezwzględny ustanowienie lokalizacji nagłówek, stopka i głównym obszarem zawartości.</span><span class="sxs-lookup"><span data-stu-id="cc432-158">One is that in addition to setting fonts and sizes, the rules use absolute positioning to establish the location of the header, footer, and main content area.</span></span> <span data-ttu-id="cc432-159">Jeśli jesteś nowym użytkownikiem pozycjonowanie w kodzie CSS, możesz przeczytać [pozycjonowania CSS](http://www.w3schools.com/css/css_positioning.asp) samouczek w witrynie W3Schools.</span><span class="sxs-lookup"><span data-stu-id="cc432-159">If you're new to positioning in CSS, you can read the [CSS Positioning](http://www.w3schools.com/css/css_positioning.asp) tutorial at the W3Schools site.</span></span>

<span data-ttu-id="cc432-160">Jeszcze inną czynność należy pamiętać, zdefiniowano że u dołu, możemy skopiowany reguły stylu, które pierwotnie indywidualnie w *Movies.cshtml* pliku.</span><span class="sxs-lookup"><span data-stu-id="cc432-160">The other thing to note is that at the bottom, we've copied the style rules that were originally defined individually in the *Movies.cshtml* file.</span></span> <span data-ttu-id="cc432-161">Zasady te były używane w [wprowadzenie do wyświetlania danych przy użyciu stron ASP.NET Web Pages](https://go.microsoft.com/fwlink/?LinkId=251580) samouczkiem, aby wprowadzić `WebGrid` pomocnika renderowania kodu znaczników, który rozkłada został dodany do tabeli.</span><span class="sxs-lookup"><span data-stu-id="cc432-161">These rules were used in the [Introduction to Displaying Data by Using ASP.NET Web Pages](https://go.microsoft.com/fwlink/?LinkId=251580) tutorial to make the `WebGrid` helper render markup that added stripes to the table.</span></span> <span data-ttu-id="cc432-162">(Jeśli ma być używana *.css* pliku definicji stylu reguły stylu dla całej witryny może również umieścić w tym polu.)</span><span class="sxs-lookup"><span data-stu-id="cc432-162">(If you're going to use a *.css* file for style definitions, you might as well put the style rules for the whole site in it.)</span></span>

## <a name="updating-the-movies-file-to-use-the-layout"></a><span data-ttu-id="cc432-163">Aktualizowanie pliku filmów do użycia w układzie</span><span class="sxs-lookup"><span data-stu-id="cc432-163">Updating the Movies File to Use the Layout</span></span>

<span data-ttu-id="cc432-164">Należy teraz zaktualizować istniejące pliki w tej witryny używanie nowego układu.</span><span class="sxs-lookup"><span data-stu-id="cc432-164">Now you can update the existing files in your site to use the new layout.</span></span> <span data-ttu-id="cc432-165">Otwórz *Movies.cshtml* pliku.</span><span class="sxs-lookup"><span data-stu-id="cc432-165">Open the *Movies.cshtml* file.</span></span> <span data-ttu-id="cc432-166">U góry w pierwszym wierszu kodu, Dodaj następujące informacje:</span><span class="sxs-lookup"><span data-stu-id="cc432-166">At the top, as the first line of code, add the following:</span></span>

[!code-csharp[Main](layouts/samples/sample3.cs)]

<span data-ttu-id="cc432-167">Strona teraz rozpoczyna się w ten sposób:</span><span class="sxs-lookup"><span data-stu-id="cc432-167">The page now starts out this way:</span></span>

[!code-cshtml[Main](layouts/samples/sample4.cshtml?highlight=2)]

<span data-ttu-id="cc432-168">Jeden wiersz kodu informuje ASP.NET że w przypadku *filmy* uruchomieniu strony, powinny zostać scalone z  *\_Layout.cshtml* pliku.</span><span class="sxs-lookup"><span data-stu-id="cc432-168">This one line of code tells ASP.NET that when the *Movies* page runs, it should be merged with the *\_Layout.cshtml* file.</span></span>

<span data-ttu-id="cc432-169">Ponieważ *Movies.cshtml* użyte strony układu, możesz usunąć znacznika z *Movies.cshtml* strona, która ma poświęcony na obsługę przez  *\_Layout.cshtml*pliku.</span><span class="sxs-lookup"><span data-stu-id="cc432-169">Since the *Movies.cshtml* file now uses a layout page, you can remove the markup from the *Movies.cshtml* page that's taken care of by the *\_Layout.cshtml* file.</span></span> <span data-ttu-id="cc432-170">Wyjmij `<!DOCTYPE>`, `<html>`, i `<body>` otwierające i zamykające znaczniki.</span><span class="sxs-lookup"><span data-stu-id="cc432-170">Take out the `<!DOCTYPE>`, `<html>`, and `<body>` opening and closing tags.</span></span> <span data-ttu-id="cc432-171">Wyjmij całą `<head>` elementu i jego zawartość, która zawiera reguły stylów siatki, ponieważ masz teraz te reguły *.css* pliku.</span><span class="sxs-lookup"><span data-stu-id="cc432-171">Take out the entire `<head>` element and its contents, which includes the style rules for the grid, since you've now got those rules in a *.css* file.</span></span> <span data-ttu-id="cc432-172">Gdy jesteś jego Zmień istniejącą `<h1>` elementu `<h2>` element; istnieje `<h1>` elementu na stronie układu już.</span><span class="sxs-lookup"><span data-stu-id="cc432-172">While you're at it, change the existing `<h1>` element to an `<h2>` element; you have an `<h1>` element in the layout page already.</span></span> <span data-ttu-id="cc432-173">Zmień `<h2>` tekst "Listy filmów".</span><span class="sxs-lookup"><span data-stu-id="cc432-173">Change the `<h2>` text to "List Movies".</span></span>

<span data-ttu-id="cc432-174">Zwykle nie trzeba wprowadzać tego rodzaju zmiany na stronie zawartości.</span><span class="sxs-lookup"><span data-stu-id="cc432-174">Normally you wouldn't have to make these sorts of changes in a content page.</span></span> <span data-ttu-id="cc432-175">Po uruchomieniu witryny ze stroną układu tworzenia stron zawartości bez tych elementów rozpoczynać się od znaku.</span><span class="sxs-lookup"><span data-stu-id="cc432-175">When you start your site out with a layout page, you create content pages without all these elements to begin with.</span></span> <span data-ttu-id="cc432-176">W takim przypadku jednak konwertowania strony autonomicznych na taki, który używa układ, dlatego nieco oczyszczania.</span><span class="sxs-lookup"><span data-stu-id="cc432-176">In this case, though, you're converting a standalone page to one that uses a layout, so there's a bit of cleanup.</span></span>

<span data-ttu-id="cc432-177">Po zakończeniu, *Movies.cshtml* strona będzie wyglądała podobnie do następującej:</span><span class="sxs-lookup"><span data-stu-id="cc432-177">When you're finished, the *Movies.cshtml* page will look like the following:</span></span>

[!code-cshtml[Main](layouts/samples/sample5.cshtml)]

### <a name="testing-the-layout"></a><span data-ttu-id="cc432-178">Testowanie układu</span><span class="sxs-lookup"><span data-stu-id="cc432-178">Testing the Layout</span></span>

<span data-ttu-id="cc432-179">Teraz widać, jak wygląda układu.</span><span class="sxs-lookup"><span data-stu-id="cc432-179">Now you can see what the layout looks like.</span></span> <span data-ttu-id="cc432-180">W programie WebMatrix, kliknij prawym przyciskiem myszy *Movies.cshtml* i wybrać opcję **Uruchom w przeglądarce**.</span><span class="sxs-lookup"><span data-stu-id="cc432-180">In WebMatrix, right-click the *Movies.cshtml* page and select **Launch in browser**.</span></span> <span data-ttu-id="cc432-181">Gdy w przeglądarce zostanie wyświetlona strona, wygląda tej strony:</span><span class="sxs-lookup"><span data-stu-id="cc432-181">When the browser displays the page, it looks like this page:</span></span>

![Renderowany przy użyciu układ strony filmy](layouts/_static/image6.png)

<span data-ttu-id="cc432-183">ASP.NET został scalony zawartości strony Movies.cshtml do  *\_Layout.cshtml* miejsca z prawej strony `RenderBody` jest metoda.</span><span class="sxs-lookup"><span data-stu-id="cc432-183">ASP.NET has merged the content of the Movies.cshtml page into the *\_Layout.cshtml* page right where the `RenderBody` method is.</span></span> <span data-ttu-id="cc432-184">I oczywiście  *\_Layout.cshtml* odwołuje się do strony *.css* pliku, która definiuje wygląd strony.</span><span class="sxs-lookup"><span data-stu-id="cc432-184">And of course the *\_Layout.cshtml* page references a *.css* file that defines the look of the page.</span></span>

## <a name="updating-the-addmovie-page-to-use-the-layout"></a><span data-ttu-id="cc432-185">Aktualizowanie strony AddMovie do użycia w układzie</span><span class="sxs-lookup"><span data-stu-id="cc432-185">Updating the AddMovie Page to Use the Layout</span></span>

<span data-ttu-id="cc432-186">Rzeczywistych zaletą układów jest, której można je dla wszystkich stron w witrynie.</span><span class="sxs-lookup"><span data-stu-id="cc432-186">The real benefit of layouts is that you can use them for all the pages in your site.</span></span> <span data-ttu-id="cc432-187">Otwórz *AddMovie.cshtml* strony.</span><span class="sxs-lookup"><span data-stu-id="cc432-187">Open the *AddMovie.cshtml* page.</span></span>

<span data-ttu-id="cc432-188">Może być Pamiętaj, że *AddMovie.cshtml* strony pierwotnie była niektóre reguły CSS w nim zdefiniować wygląd komunikatów o błędach weryfikacji.</span><span class="sxs-lookup"><span data-stu-id="cc432-188">You might remember that the *AddMovie.cshtml* page originally had some CSS rules in it to define the look of validation error messages.</span></span> <span data-ttu-id="cc432-189">Ponieważ masz *.css* pliku witryny teraz, można przenieść te reguły do *.css* pliku.</span><span class="sxs-lookup"><span data-stu-id="cc432-189">Since you have a *.css* file for your site now, you can move those rules to the *.css* file.</span></span> <span data-ttu-id="cc432-190">Usuń je z *AddMovie.cshtml* plik i dodać je do dołu *Movies.css* pliku.</span><span class="sxs-lookup"><span data-stu-id="cc432-190">Remove them from the *AddMovie.cshtml* file and add them to the bottom of the *Movies.css* file.</span></span> <span data-ttu-id="cc432-191">Przenosisz następujące reguły:</span><span class="sxs-lookup"><span data-stu-id="cc432-191">You are moving the following rules:</span></span>

[!code-css[Main](layouts/samples/sample6.css)]

<span data-ttu-id="cc432-192">Teraz należy tego samego rodzaju zmiany w *AddMovie.cshtml* jak w *Movies.cshtml* — Dodaj `Layout="~/_Layout.cshtml;` i Usuń kod znaczników HTML, która jest teraz nadmiarowe.</span><span class="sxs-lookup"><span data-stu-id="cc432-192">Now make the same sorts of changes in *AddMovie.cshtml* that you did for *Movies.cshtml* — add `Layout="~/_Layout.cshtml;` and remove the HTML markup that's now extraneous.</span></span> <span data-ttu-id="cc432-193">Zmień `<h1>` elementu `<h2>`.</span><span class="sxs-lookup"><span data-stu-id="cc432-193">Change the `<h1>` element to `<h2>`.</span></span> <span data-ttu-id="cc432-194">Gdy wszystko będzie gotowe, strona będzie wyglądała w tym przykładzie:</span><span class="sxs-lookup"><span data-stu-id="cc432-194">When you're done, the page will look like this example:</span></span>

[!code-cshtml[Main](layouts/samples/sample7.cshtml)]

<span data-ttu-id="cc432-195">Uruchom strony.</span><span class="sxs-lookup"><span data-stu-id="cc432-195">Run the page.</span></span> <span data-ttu-id="cc432-196">Teraz wygląda na tej ilustracji:</span><span class="sxs-lookup"><span data-stu-id="cc432-196">Now it looks like this illustration:</span></span>

![Strona "Dodaj filmy" renderowany przy użyciu układu](layouts/_static/image7.png)

<span data-ttu-id="cc432-198">Chcesz wprowadzić zmiany podobne do stron w witrynie — *EditMovie.cshtml* i *DeleteMovie.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="cc432-198">You want to make similar changes to the pages in the site — *EditMovie.cshtml* and *DeleteMovie.cshtml*.</span></span> <span data-ttu-id="cc432-199">Jednak zanim to zrobisz, umożliwia inna zmiana układu, który ułatwia nieco bardziej elastyczne.</span><span class="sxs-lookup"><span data-stu-id="cc432-199">However, before you do, you can make another change to the layout that makes it a little more flexible.</span></span>

## <a name="passing-title-information-to-the-layout-page"></a><span data-ttu-id="cc432-200">Przekazywanie informacji o tytule do strony układu</span><span class="sxs-lookup"><span data-stu-id="cc432-200">Passing Title Information to the Layout Page</span></span>

<span data-ttu-id="cc432-201"> *\_Layout.cshtml* strona utworzony ma `<title>` element, który ma ustawioną wartość "Moja witryna filmu".</span><span class="sxs-lookup"><span data-stu-id="cc432-201">The *\_Layout.cshtml* page that you created has a `<title>` element that's set to "My Movie Site".</span></span> <span data-ttu-id="cc432-202">W większości przeglądarek wyświetlenie zawartości tego elementu jako tekst, na karcie:</span><span class="sxs-lookup"><span data-stu-id="cc432-202">Most browsers display the content of this element as the text on a tab:</span></span>

![Strony &lt;tytuł&gt; elementu wyświetlane na karcie przeglądarki](layouts/_static/image8.png)

<span data-ttu-id="cc432-204">Te informacje tytuł jest rodzajowa.</span><span class="sxs-lookup"><span data-stu-id="cc432-204">This title information is generic.</span></span> <span data-ttu-id="cc432-205">Załóżmy, że chcesz tekstu tytułu dokładniej do bieżącej strony.</span><span class="sxs-lookup"><span data-stu-id="cc432-205">Suppose that you want the title text to be more specific to the current page.</span></span> <span data-ttu-id="cc432-206">(Tekst tytułu jest używany również przez aparaty wyszukiwania można określić strony o.) Można przekazać informacje ze strony zawartości, takich jak *Movies.cshtml* lub *AddMovie.cshtml* do strony układu, a następnie użyć tych informacji w celu dostosowania strony układu renderuje.</span><span class="sxs-lookup"><span data-stu-id="cc432-206">(The title text is also used by search engines to determine what your page is about.) You can pass information from a content page like *Movies.cshtml* or *AddMovie.cshtml* to the layout page, and then use that information to customize what the layout page renders.</span></span>

<span data-ttu-id="cc432-207">Otwórz *Movies.cshtml* strony ponownie.</span><span class="sxs-lookup"><span data-stu-id="cc432-207">Open the *Movies.cshtml* page again.</span></span> <span data-ttu-id="cc432-208">W kodzie u góry Dodaj następujący wiersz:</span><span class="sxs-lookup"><span data-stu-id="cc432-208">In the code at the top, add the following line:</span></span>

[!code-csharp[Main](layouts/samples/sample8.cs)]

<span data-ttu-id="cc432-209">`Page` Obiekt jest dostępny na wszystkich *.cshtml* stron i jest do tego celu, to znaczy do udostępniania informacji między stroną i jego układu.</span><span class="sxs-lookup"><span data-stu-id="cc432-209">The `Page` object is available on all *.cshtml* pages and is for this purpose, namely to share information between a page and its layout.</span></span>

<span data-ttu-id="cc432-210">Otwórz<em>\_Layout.cshtml</em> strony.</span><span class="sxs-lookup"><span data-stu-id="cc432-210">Open the<em>\_Layout.cshtml</em> page.</span></span> <span data-ttu-id="cc432-211">Zmień `<title>` elementu, tak że wygląda ten kod znaczników:</span><span class="sxs-lookup"><span data-stu-id="cc432-211">Change the `<title>` element so that it looks like this markup:</span></span>

[!code-html[Main](layouts/samples/sample9.html)]

<span data-ttu-id="cc432-212">Ten kod renderuje niezależnie od znajduje się w `Page.Title` właściwości w tej lokalizacji na stronie.</span><span class="sxs-lookup"><span data-stu-id="cc432-212">This code renders whatever is in the `Page.Title` property right at that location in the page.</span></span>

<span data-ttu-id="cc432-213">Uruchom *Movies.cshtml* strony.</span><span class="sxs-lookup"><span data-stu-id="cc432-213">Run the *Movies.cshtml* page.</span></span> <span data-ttu-id="cc432-214">Pokazuje kartę przeglądarki teraz przekazany jako wartość `Page.Title`:</span><span class="sxs-lookup"><span data-stu-id="cc432-214">This time the browser tab shows what you passed as the value of `Page.Title`:</span></span>

![Na karcie przeglądarki pokazywany tytuł utworzony dynamicznie](layouts/_static/image9.png)

<span data-ttu-id="cc432-216">Jeśli chcesz, Wyświetl źródło strony w przeglądarce.</span><span class="sxs-lookup"><span data-stu-id="cc432-216">If you want, view the page source in the browser.</span></span> <span data-ttu-id="cc432-217">Można stwierdzić, że `<title>` element jest renderowany jako `<title>List Movies</title>`.</span><span class="sxs-lookup"><span data-stu-id="cc432-217">You can see that the `<title>` element is rendered as `<title>List Movies</title>`.</span></span>

> [!TIP] 
> 
> <span data-ttu-id="cc432-218">**Obiekt strony**</span><span class="sxs-lookup"><span data-stu-id="cc432-218">**The Page Object**</span></span>
> 
> <span data-ttu-id="cc432-219">Przydatną cechą `Page` to obiekt dynamiczny — `Title` właściwości nie jest nazwą fixed lub zastrzeżone.</span><span class="sxs-lookup"><span data-stu-id="cc432-219">A useful feature of `Page` is that it's a dynamic object — the `Title` property is not a fixed or reserved name.</span></span> <span data-ttu-id="cc432-220">Można użyć *żadnych* nazwę wartości `Page` obiektu.</span><span class="sxs-lookup"><span data-stu-id="cc432-220">You can use *any* name for a value of the `Page` object.</span></span> <span data-ttu-id="cc432-221">Na przykład można łatwo przekazanego tytuł przy użyciu właściwości o nazwie `Page.CurrentName` lub `Page.MyPage`.</span><span class="sxs-lookup"><span data-stu-id="cc432-221">For example, you could as easily have passed the title by using a property named `Page.CurrentName` or `Page.MyPage`.</span></span> <span data-ttu-id="cc432-222">Tylko ograniczenie jest nazwa do normalnej reguły dla właściwości, które mogą mieć nazwę.</span><span class="sxs-lookup"><span data-stu-id="cc432-222">The only restriction is that the name has to follow the normal rules for what properties can be named.</span></span> <span data-ttu-id="cc432-223">(Na przykład nazwa nie może zawierać spacji).</span><span class="sxs-lookup"><span data-stu-id="cc432-223">(For example, the name can't contain a space.)</span></span>
> 
> <span data-ttu-id="cc432-224">Można przekazać dowolną liczbę wartości przy użyciu `Page` obiektu.</span><span class="sxs-lookup"><span data-stu-id="cc432-224">You can pass any number of values by using the `Page` object.</span></span> <span data-ttu-id="cc432-225">Jeśli chcesz przekazać informacje do strony układu, można przekazać wartości przy użyciu przypominać `Page.MovieTitle` i `Page.Genre` i `Page.MovieYear`.</span><span class="sxs-lookup"><span data-stu-id="cc432-225">If you wanted to pass movie information to the layout page, you could pass values by using something like `Page.MovieTitle` and `Page.Genre` and `Page.MovieYear`.</span></span> <span data-ttu-id="cc432-226">(Lub inne nazwy, które wynaleźli do przechowywania informacji.) Jedynym wymaganiem — która jest prawdopodobnie widocznych — jest trzeba użyć tych samych nazw na stronie zawartości i strony układu.</span><span class="sxs-lookup"><span data-stu-id="cc432-226">(Or any other names that you invented to store the information.) The only requirement — which is probably obvious — is that you have to use the same names in the content page and the layout page.</span></span>
> 
> <span data-ttu-id="cc432-227">Informacje przekazywane za pomocą `Page` obiekt nie jest ograniczona do tylko tekst do wyświetlania na stronie układu.</span><span class="sxs-lookup"><span data-stu-id="cc432-227">The information you pass by using the `Page` object isn't limited to just text to display on the layout page.</span></span> <span data-ttu-id="cc432-228">Można przekazać wartość na stronę układu, a następnie kodu na stronie układu można użyć wartości zdecydować, czy mają być wyświetlane części strony, co *.css* plików do użycia i tak dalej.</span><span class="sxs-lookup"><span data-stu-id="cc432-228">You can pass a value to the layout page, and then code in the layout page can use the value to decide whether to display a section of the page, what *.css* file to use, and so on.</span></span> <span data-ttu-id="cc432-229">Wartości przekazywane w `Page` obiektu są takie jak wszelkie inne wartości użycia w kodzie.</span><span class="sxs-lookup"><span data-stu-id="cc432-229">The values you pass in the `Page` object are like any other values that you use in code.</span></span> <span data-ttu-id="cc432-230">Jest po prostu czy wartości pochodzą z strony zawartości i są przekazywane do strony układu.</span><span class="sxs-lookup"><span data-stu-id="cc432-230">It's just that the values originate in the content page and are passed to the layout page.</span></span>


<span data-ttu-id="cc432-231">Otwórz *AddMovie.cshtml* strony i dodać wiersz do góry kod, który zawiera tytuł *AddMovie.cshtml* strony:</span><span class="sxs-lookup"><span data-stu-id="cc432-231">Open the *AddMovie.cshtml* page and add a line to the top of the code that provides a title for the *AddMovie.cshtml* page:</span></span>

[!code-csharp[Main](layouts/samples/sample10.cs)]

<span data-ttu-id="cc432-232">Uruchom *AddMovie.cshtml* strony.</span><span class="sxs-lookup"><span data-stu-id="cc432-232">Run the *AddMovie.cshtml* page.</span></span> <span data-ttu-id="cc432-233">Zostanie wyświetlony nowy tytuł:</span><span class="sxs-lookup"><span data-stu-id="cc432-233">You see the new title there:</span></span>

![Na karcie przeglądarki pokazywany tytuł "Dodaj filmy" utworzony dynamicznie](layouts/_static/image10.png)

## <a name="updating-the-remaining-pages-to-use-the-layout"></a><span data-ttu-id="cc432-235">Uaktualnianie pozostałych stron do użycia w układzie</span><span class="sxs-lookup"><span data-stu-id="cc432-235">Updating the Remaining Pages to Use the Layout</span></span>

<span data-ttu-id="cc432-236">Teraz możesz ukończyć pozostałych stron w witrynie tak, aby używały nowy układ.</span><span class="sxs-lookup"><span data-stu-id="cc432-236">Now you can finish the remaining pages in your site so that they use the new layout.</span></span> <span data-ttu-id="cc432-237">Otwórz *EditMovie.cshtml* i *DeleteMovie.cshtml* z kolei i wprowadzenie identycznych zmian w każdym.</span><span class="sxs-lookup"><span data-stu-id="cc432-237">Open *EditMovie.cshtml* and *DeleteMovie.cshtml* in turn and make the same changes in each.</span></span>

<span data-ttu-id="cc432-238">Dodaj wiersz kodu, który stanowi łącze do strony układu:</span><span class="sxs-lookup"><span data-stu-id="cc432-238">Add the line of code that links to the layout page:</span></span>

[!code-csharp[Main](layouts/samples/sample11.cs)]

<span data-ttu-id="cc432-239">Dodaj linię, aby ustawić tytuł strony:</span><span class="sxs-lookup"><span data-stu-id="cc432-239">Add a line to set the title of the page:</span></span>

[!code-csharp[Main](layouts/samples/sample12.cs)]

<span data-ttu-id="cc432-240">lub:</span><span class="sxs-lookup"><span data-stu-id="cc432-240">or:</span></span>

[!code-csharp[Main](layouts/samples/sample13.cs)]

<span data-ttu-id="cc432-241">Usuń wszystkie nadmiarowe kod znaczników HTML — zasadniczo Pozostaw tylko bity, które znajdują się wewnątrz `<body>` elementu (oraz u góry bloku kodu).</span><span class="sxs-lookup"><span data-stu-id="cc432-241">Remove all the extraneous HTML markup — basically, leave only the bits that are inside the `<body>` element (plus the code block at the top).</span></span>

<span data-ttu-id="cc432-242">Zmień `<h1>` element ma być `<h2>` elementu.</span><span class="sxs-lookup"><span data-stu-id="cc432-242">Change the `<h1>` element to be an `<h2>` element.</span></span>

<span data-ttu-id="cc432-243">Po wprowadzeniu tych zmian, każdy test i upewnij się, że są wyświetlane poprawnie i czy tytuł jest poprawna.</span><span class="sxs-lookup"><span data-stu-id="cc432-243">When you've made these changes, test each and make sure that it's displaying properly and that the title is correct.</span></span>

## <a name="parting-thoughts-about-layout-pages"></a><span data-ttu-id="cc432-244">Towarzyszącą pomysły dotyczące strony układu</span><span class="sxs-lookup"><span data-stu-id="cc432-244">Parting Thoughts About Layout Pages</span></span>

<span data-ttu-id="cc432-245">W tym samouczku został utworzony  *\_Layout.cshtml* strony i używać `RenderBody` sposób scalania zawartości z innej strony.</span><span class="sxs-lookup"><span data-stu-id="cc432-245">In this tutorial you created a *\_Layout.cshtml* page and used the `RenderBody` method to merge content from another page.</span></span> <span data-ttu-id="cc432-246">To podstawowy wzorzec do używania układy stron sieci Web.</span><span class="sxs-lookup"><span data-stu-id="cc432-246">That's the basic pattern for using layouts in Web Pages.</span></span>

<span data-ttu-id="cc432-247">Układ strony mają dodatkowe funkcje, które firma Microsoft nie obejmują tutaj.</span><span class="sxs-lookup"><span data-stu-id="cc432-247">Layout pages have additional features that we didn't cover here.</span></span> <span data-ttu-id="cc432-248">Na przykład można zagnieżdżać strony układu — jedną stronę układu mogą z kolei odwoływać się do innego.</span><span class="sxs-lookup"><span data-stu-id="cc432-248">For example, you can nest layout pages — one layout page can in turn reference another.</span></span> <span data-ttu-id="cc432-249">Układy zagnieżdżonych mogą być przydatne, jeśli pracujesz z podpunkty lokacji wymagające układów.</span><span class="sxs-lookup"><span data-stu-id="cc432-249">Nested layouts can be useful if you're working with subsections of a site that require different layouts.</span></span> <span data-ttu-id="cc432-250">Można także używać dodatkowych metod (na przykład `RenderSection`) można skonfigurować konta o nazwie części strony układu.</span><span class="sxs-lookup"><span data-stu-id="cc432-250">You can also use additional methods (for example, `RenderSection`) to set up named sections in the layout page.</span></span>

<span data-ttu-id="cc432-251">Kombinacja strony układu i *.css* plików to zaawansowane.</span><span class="sxs-lookup"><span data-stu-id="cc432-251">The combination of layout pages and *.css* files is powerful.</span></span> <span data-ttu-id="cc432-252">Jak można zauważyć w następnej serii samouczek, w programie WebMatrix można utworzyć witryny na podstawie *szablonu*, które zapewnia witryny, która ma wbudowane funkcje w nim.</span><span class="sxs-lookup"><span data-stu-id="cc432-252">As you'll see in the next tutorial series, in WebMatrix you can create a site based on a *template*, which gives you a site that has prebuilt functionality in it.</span></span> <span data-ttu-id="cc432-253">Szablony umożliwiają pełne wykorzystanie strony układu i CSS do tworzenia witryn, który wygląda świetnie i mają funkcje, takie jak menu.</span><span class="sxs-lookup"><span data-stu-id="cc432-253">The templates make good use of layout pages and CSS to create sites that look great and that have features like menus.</span></span> <span data-ttu-id="cc432-254">Poniżej przedstawiono zrzut ekranu strony głównej z lokacji, na podstawie szablonu, przedstawiający funkcji używających strony układu i CSS:</span><span class="sxs-lookup"><span data-stu-id="cc432-254">Here's a screenshot of the home page from a site based on a template, showing features that use layout pages and CSS:</span></span>

![Układ utworzony przez nagłówek, obszar nawigacji, obszaru zawartości, sekcja opcjonalna oraz łącza logowania szablonu witryny programu WebMatrix](layouts/_static/image11.png)

## <a name="complete-listing-for-movie-page-updated-to-use-a-layout-page"></a><span data-ttu-id="cc432-256">Pełna lista strony Movie (zaktualizowane do strony układu)</span><span class="sxs-lookup"><span data-stu-id="cc432-256">Complete Listing for Movie Page (Updated to Use a Layout Page)</span></span>

[!code-cshtml[Main](layouts/samples/sample14.cshtml)]

## <a name="complete-page-listing-for-add-movie-page-updated-for-layout"></a><span data-ttu-id="cc432-257">Strona ukończyć dla Dodaj stronę Movie (zaktualizowane dla układu)</span><span class="sxs-lookup"><span data-stu-id="cc432-257">Complete Page Listing for Add Movie Page (Updated for Layout)</span></span>

[!code-cshtml[Main](layouts/samples/sample15.cshtml)]

## <a name="complete-page-listing-for-delete-movie-page-updated-for-layout"></a><span data-ttu-id="cc432-258">Strona pełną listę strony filmu usuwania (zaktualizowane dla układu)</span><span class="sxs-lookup"><span data-stu-id="cc432-258">Complete Page Listing for Delete Movie Page (Updated for Layout)</span></span>

[!code-cshtml[Main](layouts/samples/sample16.cshtml)]

## <a name="complete-page-listing-for-edit-movie-page-updated-for-layout"></a><span data-ttu-id="cc432-259">Strona pełną listę edycji strony Movie (zaktualizowane dla układu)</span><span class="sxs-lookup"><span data-stu-id="cc432-259">Complete Page Listing for Edit Movie Page (Updated for Layout)</span></span>

[!code-cshtml[Main](layouts/samples/sample17.cshtml)]

## <a name="coming-up-next"></a><span data-ttu-id="cc432-260">Powtarzający się dalej</span><span class="sxs-lookup"><span data-stu-id="cc432-260">Coming Up Next</span></span>

<span data-ttu-id="cc432-261">W następnym samouczku nauczysz się, jak opublikować witryny z Internetem, więc każdy może zobaczyć.</span><span class="sxs-lookup"><span data-stu-id="cc432-261">In the next tutorial, you'll learn how to publish your site to the Internet so everyone can see it.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="cc432-262">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="cc432-262">Additional Resources</span></span>

- <span data-ttu-id="cc432-263">[Tworzenie spójny wygląd](https://go.microsoft.com/fwlink/?LinkID=202891) — artykułu, który zawiera niektóre więcej szczegółów na temat pracy z układów.</span><span class="sxs-lookup"><span data-stu-id="cc432-263">[Creating a Consistent Look](https://go.microsoft.com/fwlink/?LinkID=202891) — An article that provides some more detail on working with layouts.</span></span> <span data-ttu-id="cc432-264">On również opis przekazać wartości do strony układu, który pokazuje lub ukrywa części zawartości.</span><span class="sxs-lookup"><span data-stu-id="cc432-264">It also describes how to pass a value to a layout page that shows or hides some of the content.</span></span>
- <span data-ttu-id="cc432-265">[Zagnieżdżone strony układu ze składnią Razor](http://www.mikesdotnetting.com/Article/164/Nested-Layout-Pages-with-Razor) — blogi Jan Brind przykładem zagnieździć strony układu.</span><span class="sxs-lookup"><span data-stu-id="cc432-265">[Nested Layout Pages with Razor](http://www.mikesdotnetting.com/Article/164/Nested-Layout-Pages-with-Razor) — Mike Brind blogs an example of how to nest layout pages.</span></span> <span data-ttu-id="cc432-266">(Dotyczy również pobierania strony).</span><span class="sxs-lookup"><span data-stu-id="cc432-266">(Includes a download of the pages.)</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="cc432-267">[Poprzednie](deleting-data.md)
> [dalej](publishing.md)</span><span class="sxs-lookup"><span data-stu-id="cc432-267">[Previous](deleting-data.md)
[Next](publishing.md)</span></span>
