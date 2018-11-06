---
uid: web-pages/overview/getting-started/introducing-aspnet-web-pages-2/layouts
title: Wprowadzenie do składnika ASP.NET Web Pages — tworzenie spójnego układu | Dokumentacja firmy Microsoft
author: Rick-Anderson
description: W tym samouczku dowiesz się, jak za pomocą układów tworzenie spójnego wyglądu dla stron w lokacji korzystającej z ASP.NET Web Pages. Przyjęto założenie, że zostały wykonane...
ms.author: riande
ms.date: 05/28/2015
ms.assetid: c85ec591-f8d7-4882-b763-de6ab9f3df7a
msc.legacyurl: /web-pages/overview/getting-started/introducing-aspnet-web-pages-2/layouts
msc.type: authoredcontent
ms.openlocfilehash: a6a007678d58547e9987ebda46bd08ae8aea66f7
ms.sourcegitcommit: 2d3e5422d530203efdaf2014d1d7df31f88d08d0
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/05/2018
ms.locfileid: "51021186"
---
<a name="introducing-aspnet-web-pages---creating-a-consistent-layout"></a><span data-ttu-id="fa231-104">Wprowadzenie do wzorca ASP.NET Web Pages — tworzenie spójnego układu</span><span class="sxs-lookup"><span data-stu-id="fa231-104">Introducing ASP.NET Web Pages - Creating a Consistent Layout</span></span>
====================
<span data-ttu-id="fa231-105">przez [Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="fa231-105">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="fa231-106">W tym samouczku dowiesz się, jak używać *układy* do tworzenia spójnego wyglądu dla stron w lokacji korzystającej z ASP.NET Web Pages.</span><span class="sxs-lookup"><span data-stu-id="fa231-106">This tutorial shows you how to use *layouts* to create a consistent look for the pages on a site that uses ASP.NET Web Pages.</span></span> <span data-ttu-id="fa231-107">Przyjęto założenie, że zostały wykonane serii za pośrednictwem [usuwanie danych z baz danych w programie ASP.NET Web Pages](https://go.microsoft.com/fwlink/?LinkId=251584).</span><span class="sxs-lookup"><span data-stu-id="fa231-107">It assumes you have completed the series through [Deleting Database Data in ASP.NET Web Pages](https://go.microsoft.com/fwlink/?LinkId=251584).</span></span>
> 
> <span data-ttu-id="fa231-108">Zawartość:</span><span class="sxs-lookup"><span data-stu-id="fa231-108">What you'll learn:</span></span>
> 
> - <span data-ttu-id="fa231-109">Strona układu jest.</span><span class="sxs-lookup"><span data-stu-id="fa231-109">What a layout page is.</span></span>
> - <span data-ttu-id="fa231-110">Jak połączyć układu strony z zawartością dynamiczną.</span><span class="sxs-lookup"><span data-stu-id="fa231-110">How to combine layout pages with dynamic content.</span></span>
> - <span data-ttu-id="fa231-111">Jak przekazać wartości do strony układu.</span><span class="sxs-lookup"><span data-stu-id="fa231-111">How to pass values to a layout page.</span></span>


## <a name="about-layouts"></a><span data-ttu-id="fa231-112">Układy — informacje</span><span class="sxs-lookup"><span data-stu-id="fa231-112">About Layouts</span></span>

<span data-ttu-id="fa231-113">Strony utworzone do tej pory wszystkie zostały zakończone, stron autonomicznych.</span><span class="sxs-lookup"><span data-stu-id="fa231-113">The pages you've created so far have all been complete, standalone pages.</span></span> <span data-ttu-id="fa231-114">Wszystkie one należeć do tej samej lokacji, ale nie mają żadnych wspólnych elementów lub standardowy wygląd.</span><span class="sxs-lookup"><span data-stu-id="fa231-114">They all belong to the same site, but they don't have any common elements or a standard look.</span></span>

<span data-ttu-id="fa231-115">Większość witryn mają spójnego wyglądu i układ.</span><span class="sxs-lookup"><span data-stu-id="fa231-115">Most sites do have a consistent look and layout.</span></span> <span data-ttu-id="fa231-116">Na przykład, jeśli przejdziesz do [Microsoft.com/web](https://www.microsoft.com/web/) lokacji i poszukaj, zobaczysz, że wszystkie strony stosować ogólny układ i motyw wizualny:</span><span class="sxs-lookup"><span data-stu-id="fa231-116">For example, if you go to the [Microsoft.com/web](https://www.microsoft.com/web/) site and look around, you see that the pages all adhere to an overall layout and to a visual theme:</span></span>

![Strona witryny Microsoft.com/Web przedstawiający układ nagłówka, obszar nawigacji, obszarem zawartości i stopka](layouts/_static/image1.png)

<span data-ttu-id="fa231-118">*Nieefektywne* sposób, aby utworzyć ten układ jest zdefiniować nagłówek, pasek nawigacyjny i stopka osobno dla poszczególnych stron sieci.</span><span class="sxs-lookup"><span data-stu-id="fa231-118">An *inefficient* way to create this layout would be to define a header, navigation bar, and footer separately on each of your pages.</span></span> <span data-ttu-id="fa231-119">Można będzie się duplikowania ten sam kod znaczników każdorazowo.</span><span class="sxs-lookup"><span data-stu-id="fa231-119">You'd be duplicating the same markup each time.</span></span> <span data-ttu-id="fa231-120">Jeśli chcesz coś zmienić (na przykład aktualizacja stopki), trzeba zmienić każdą stronę oddzielnie.</span><span class="sxs-lookup"><span data-stu-id="fa231-120">If you wanted to change something (for example, update the footer), you'd have to change each page separately.</span></span>

<span data-ttu-id="fa231-121">To tutaj *strony układu* pochodzą.</span><span class="sxs-lookup"><span data-stu-id="fa231-121">That's where *layout pages* come in.</span></span> <span data-ttu-id="fa231-122">W składniku ASP.NET Web Pages można zdefiniować stronę układu, oferująca pojemniku ogólnego stron w witrynie.</span><span class="sxs-lookup"><span data-stu-id="fa231-122">In ASP.NET Web Pages, you can define a layout page that provides an overall container for pages on your site.</span></span> <span data-ttu-id="fa231-123">Na przykład strony układu może zawierać nagłówek, obszar nawigacji i stopki.</span><span class="sxs-lookup"><span data-stu-id="fa231-123">For example, the layout page can contain the header, navigation area, and footer.</span></span> <span data-ttu-id="fa231-124">Strona układu zawiera symbol zastępczy, gdzie należy wstawić zawartość głównego.</span><span class="sxs-lookup"><span data-stu-id="fa231-124">The layout page includes a placeholder where the main content goes.</span></span>

<span data-ttu-id="fa231-125">Następnie można zdefiniować poszczególne strony z zawartością, które zawierają znaczników i kodu tylko na tej stronie.</span><span class="sxs-lookup"><span data-stu-id="fa231-125">You can then define individual content pages that contain the markup and the code for only that page.</span></span> <span data-ttu-id="fa231-126">Strony z zawartością nie muszą być ukończone stron HTML. jeszcze nie mają mieć `<body>` elementu.</span><span class="sxs-lookup"><span data-stu-id="fa231-126">Content pages don't have to be complete HTML pages; they don't even have to have a `<body>` element.</span></span> <span data-ttu-id="fa231-127">Mają one linię kodu, który informuje, jakie stronę układu, w której chcesz wyświetlać zawartość w ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="fa231-127">They also have a line of code that tells ASP.NET what layout page you want to display the content in.</span></span> <span data-ttu-id="fa231-128">Oto obraz, który około pokazuje, jak działa ta relacja:</span><span class="sxs-lookup"><span data-stu-id="fa231-128">Here's a picture that shows roughly how this relationship works:</span></span>

![Diagram koncepcyjny przedstawiający dwóch stronach zawartości i stronę układu, w którym mieściły się](layouts/_static/image2.png)

<span data-ttu-id="fa231-130">Ta interakcja jest łatwa do zrozumienia, kiedy zostanie wyświetlony w działaniu.</span><span class="sxs-lookup"><span data-stu-id="fa231-130">This interaction is easy to understand when you see it in action.</span></span> <span data-ttu-id="fa231-131">W tym samouczku zmienisz stron sieci filmy, aby używać układu.</span><span class="sxs-lookup"><span data-stu-id="fa231-131">In this tutorial, you'll change your movies pages to use a layout.</span></span>

## <a name="adding-a-layout-page"></a><span data-ttu-id="fa231-132">Dodawanie strony układu</span><span class="sxs-lookup"><span data-stu-id="fa231-132">Adding a Layout Page</span></span>

<span data-ttu-id="fa231-133">Będziesz rozpoczyna się od utworzenia strony układu, który definiuje układ strony typowego nagłówka, stopki i obszar do głównej zawartości.</span><span class="sxs-lookup"><span data-stu-id="fa231-133">You'll start by creating a layout page that defines a typical page layout with a header, footer, and an area for the main content.</span></span> <span data-ttu-id="fa231-134">W witrynie WebPagesMovies Dodaj stronę CSHTML o nazwie  *\_Layout.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="fa231-134">In the WebPagesMovies site, add a CSHTML page named *\_Layout.cshtml*.</span></span>

<span data-ttu-id="fa231-135">Wiodący znak podkreślenia ( `_` ) znak jest znacząca.</span><span class="sxs-lookup"><span data-stu-id="fa231-135">The leading underscore ( `_` ) character is significant.</span></span> <span data-ttu-id="fa231-136">Jeśli na stronie nazwa zaczyna się od znaku podkreślenia, ASP.NET nie bezpośrednio wysłać tę stronę w przeglądarce.</span><span class="sxs-lookup"><span data-stu-id="fa231-136">If a page's name starts with an underscore, ASP.NET won't directly send that page to the browser.</span></span> <span data-ttu-id="fa231-137">Ta konwencja pozwala zdefiniować stron, które są wymagane dla danej witryny, jednak, że użytkownicy nie powinien móc żądać bezpośrednio.</span><span class="sxs-lookup"><span data-stu-id="fa231-137">This convention lets you define pages that are required for your site but that users shouldn't be able to request directly.</span></span>

<span data-ttu-id="fa231-138">Zastąp zawartość strony następujących czynności:</span><span class="sxs-lookup"><span data-stu-id="fa231-138">Replace the content in the page with the following:</span></span>

[!code-html[Main](layouts/samples/sample1.html)]

<span data-ttu-id="fa231-139">Jak widać, ten kod znaczników jest po prostu HTML, który używa `<div>` elementy, aby zdefiniować trzy sekcje w stronę plus 1 bardziej `<div>` elementu do przechowywania trzy sekcje.</span><span class="sxs-lookup"><span data-stu-id="fa231-139">As you can see, this markup is just HTML that uses `<div>` elements to define three sections in the page plus one more `<div>` element to hold the three sections.</span></span> <span data-ttu-id="fa231-140">Stopka zawiera ilość kodu Razor: `@DateTime.Now.Year`, który będzie renderowany w bieżącym roku w tej lokalizacji, na stronie.</span><span class="sxs-lookup"><span data-stu-id="fa231-140">The footer contains a bit of Razor code: `@DateTime.Now.Year`, which will render the current year at that location in the page.</span></span>

<span data-ttu-id="fa231-141">Należy zauważyć, że znajduje się link do arkusza stylów, o nazwie *Movies.css*.</span><span class="sxs-lookup"><span data-stu-id="fa231-141">Notice that there's a link to a style sheet named *Movies.css*.</span></span> <span data-ttu-id="fa231-142">Arkusz stylów jest, gdzie można zdefiniować szczegóły fizycznego układu elementów.</span><span class="sxs-lookup"><span data-stu-id="fa231-142">The style sheet is where the details of the physical layout of the elements will be defined.</span></span> <span data-ttu-id="fa231-143">Instrukcje dotyczące tworzenia, za chwilę.</span><span class="sxs-lookup"><span data-stu-id="fa231-143">You'll create that in a moment.</span></span>

<span data-ttu-id="fa231-144">Tylko niezwykłych funkcji, w tym  *\_Layout.cshtml* strona jest `@Render.Body()` wiersza.</span><span class="sxs-lookup"><span data-stu-id="fa231-144">The only unusual feature in this *\_Layout.cshtml* page is the `@Render.Body()` line.</span></span> <span data-ttu-id="fa231-145">To symbol zastępczy, gdzie zawartość, gdy ten układ jest scalany z innej strony.</span><span class="sxs-lookup"><span data-stu-id="fa231-145">That's the placeholder where the content will go when this layout is merged with another page.</span></span>

## <a name="adding-a-css-file"></a><span data-ttu-id="fa231-146">Dodawanie pliku .css</span><span class="sxs-lookup"><span data-stu-id="fa231-146">Adding a .css File</span></span>

<span data-ttu-id="fa231-147">Preferowanym sposobem zdefiniowania układowi (czyli wygląd) elementów na stronie jest użyć reguły stylów kaskadowych w arkusz (CSS).</span><span class="sxs-lookup"><span data-stu-id="fa231-147">The preferred way to define the actual arrangement (that is, appearance) of elements on the page is to use cascading style sheet (CSS) rules.</span></span> <span data-ttu-id="fa231-148">Dlatego należy utworzyć *.css* pliku, który ma reguły dla nowego układu.</span><span class="sxs-lookup"><span data-stu-id="fa231-148">So you'll create a *.css* file that has the rules for your new layout.</span></span>

<span data-ttu-id="fa231-149">W programie WebMatrix wybierz katalog główny witryny.</span><span class="sxs-lookup"><span data-stu-id="fa231-149">In WebMatrix, select the root of your site.</span></span> <span data-ttu-id="fa231-150">Następnie w **pliki** karty na wstążce kliknij strzałkę w obszarze **New** przycisk, a następnie kliknij przycisk **nowy Folder**.</span><span class="sxs-lookup"><span data-stu-id="fa231-150">Then in the **Files** tab of the ribbon, click the arrow under the **New** button and then click **New Folder**.</span></span>

![Opcję "Nowy Folder" w obszarze nowe na Wstążce.](layouts/_static/image3.png)

<span data-ttu-id="fa231-152">Nadaj nazwę nowego folderu *style*.</span><span class="sxs-lookup"><span data-stu-id="fa231-152">Name the new folder *Styles*.</span></span>

![Nazewnictwo nowy folder "Style"](layouts/_static/image4.png)

<span data-ttu-id="fa231-154">W nowym *style* folderze utwórz plik o nazwie *Movies.css*.</span><span class="sxs-lookup"><span data-stu-id="fa231-154">Inside the new *Styles* folder, create a file named *Movies.css*.</span></span>

![Tworzenie nowego pliku Movies.css](layouts/_static/image5.png)

<span data-ttu-id="fa231-156">Zastąp zawartość nowego *.css* pliku następującym kodem:</span><span class="sxs-lookup"><span data-stu-id="fa231-156">Replace the contents of the new *.css* file with the following:</span></span>

[!code-css[Main](layouts/samples/sample2.css)]

<span data-ttu-id="fa231-157">Firma Microsoft nie będzie, że te informacje, te reguły CSS, z wyjątkiem dwie czynności Pamiętaj, aby.</span><span class="sxs-lookup"><span data-stu-id="fa231-157">We won't say much about these CSS rules, except to note two things.</span></span> <span data-ttu-id="fa231-158">Jeden jest oprócz ustawienia czcionek i rozmiarów, reguły pozycjonowanie absolutne ustanowienie lokalizacji nagłówek, stopka i główny obszar zawartości.</span><span class="sxs-lookup"><span data-stu-id="fa231-158">One is that in addition to setting fonts and sizes, the rules use absolute positioning to establish the location of the header, footer, and main content area.</span></span> <span data-ttu-id="fa231-159">Jeśli jesteś nowym użytkownikiem pozycjonowania w kodzie CSS, możesz przeczytać [pozycjonowania CSS](http://www.w3schools.com/css/css_positioning.asp) samouczek w witrynie W3Schools.</span><span class="sxs-lookup"><span data-stu-id="fa231-159">If you're new to positioning in CSS, you can read the [CSS Positioning](http://www.w3schools.com/css/css_positioning.asp) tutorial at the W3Schools site.</span></span>

<span data-ttu-id="fa231-160">Jeszcze inną czynność należy pamiętać, zdefiniowano, u dołu skopiowano możemy reguły stylów, które były pierwotnie indywidualnie w *Movies.cshtml* pliku.</span><span class="sxs-lookup"><span data-stu-id="fa231-160">The other thing to note is that at the bottom, we've copied the style rules that were originally defined individually in the *Movies.cshtml* file.</span></span> <span data-ttu-id="fa231-161">Te reguły zostały użyte w [wprowadzenie do wyświetlania danych przy użyciu stron ASP.NET Web Pages](https://go.microsoft.com/fwlink/?LinkId=251580) samouczka, aby wprowadzić `WebGrid` pomocnika renderowania kodu znaczników dodaną rozkłada do tabeli.</span><span class="sxs-lookup"><span data-stu-id="fa231-161">These rules were used in the [Introduction to Displaying Data by Using ASP.NET Web Pages](https://go.microsoft.com/fwlink/?LinkId=251580) tutorial to make the `WebGrid` helper render markup that added stripes to the table.</span></span> <span data-ttu-id="fa231-162">(Jeśli zamierzasz używać *.css* pliku definicji stylów może także stanowić reguł stylu dla całej witryny w nim.)</span><span class="sxs-lookup"><span data-stu-id="fa231-162">(If you're going to use a *.css* file for style definitions, you might as well put the style rules for the whole site in it.)</span></span>

## <a name="updating-the-movies-file-to-use-the-layout"></a><span data-ttu-id="fa231-163">Aktualizowanie pliku filmy do użycia w układzie</span><span class="sxs-lookup"><span data-stu-id="fa231-163">Updating the Movies File to Use the Layout</span></span>

<span data-ttu-id="fa231-164">Można teraz zaktualizować istniejące pliki w tej witryny używanie nowego układu.</span><span class="sxs-lookup"><span data-stu-id="fa231-164">Now you can update the existing files in your site to use the new layout.</span></span> <span data-ttu-id="fa231-165">Otwórz *Movies.cshtml* pliku.</span><span class="sxs-lookup"><span data-stu-id="fa231-165">Open the *Movies.cshtml* file.</span></span> <span data-ttu-id="fa231-166">U góry strony w pierwszym wierszu kodu, Dodaj następujący fragment kodu:</span><span class="sxs-lookup"><span data-stu-id="fa231-166">At the top, as the first line of code, add the following:</span></span>

[!code-csharp[Main](layouts/samples/sample3.cs)]

<span data-ttu-id="fa231-167">Strona teraz rozpoczyna się w ten sposób:</span><span class="sxs-lookup"><span data-stu-id="fa231-167">The page now starts out this way:</span></span>

[!code-cshtml[Main](layouts/samples/sample4.cshtml?highlight=2)]

<span data-ttu-id="fa231-168">To jeden wiersz kodu informuje ASP.NET że w przypadku *filmy* uruchamia stronę, powinny zostać scalone z  *\_Layout.cshtml* pliku.</span><span class="sxs-lookup"><span data-stu-id="fa231-168">This one line of code tells ASP.NET that when the *Movies* page runs, it should be merged with the *\_Layout.cshtml* file.</span></span>

<span data-ttu-id="fa231-169">Ponieważ *Movies.cshtml* plik używa teraz stronę układu, możesz usunąć znaczników z *Movies.cshtml* strona, która jest wykonywane przez  *\_Layout.cshtml*pliku.</span><span class="sxs-lookup"><span data-stu-id="fa231-169">Since the *Movies.cshtml* file now uses a layout page, you can remove the markup from the *Movies.cshtml* page that's taken care of by the *\_Layout.cshtml* file.</span></span> <span data-ttu-id="fa231-170">Opróżnij `<!DOCTYPE>`, `<html>`, i `<body>` znaczników otwierających i zamykających.</span><span class="sxs-lookup"><span data-stu-id="fa231-170">Take out the `<!DOCTYPE>`, `<html>`, and `<body>` opening and closing tags.</span></span> <span data-ttu-id="fa231-171">W działaniu całej `<head>` elementu i jego zawartości, które obejmują reguły stylów siatki, ponieważ masz teraz tych zasad w *.css* pliku.</span><span class="sxs-lookup"><span data-stu-id="fa231-171">Take out the entire `<head>` element and its contents, which includes the style rules for the grid, since you've now got those rules in a *.css* file.</span></span> <span data-ttu-id="fa231-172">Podczas pracy w tym zmienić istniejące `<h1>` elementu `<h2>` element; ma `<h1>` element już strony układu.</span><span class="sxs-lookup"><span data-stu-id="fa231-172">While you're at it, change the existing `<h1>` element to an `<h2>` element; you have an `<h1>` element in the layout page already.</span></span> <span data-ttu-id="fa231-173">Zmiana `<h2>` tekst "Listy filmów".</span><span class="sxs-lookup"><span data-stu-id="fa231-173">Change the `<h2>` text to "List Movies".</span></span>

<span data-ttu-id="fa231-174">Zazwyczaj nie trzeba wprowadzać tego rodzaju zmian w zawartości strony.</span><span class="sxs-lookup"><span data-stu-id="fa231-174">Normally you wouldn't have to make these sorts of changes in a content page.</span></span> <span data-ttu-id="fa231-175">Po uruchomieniu witryny ze stroną układu tworzenia stron zawartości bez tych elementów rozpoczynać się.</span><span class="sxs-lookup"><span data-stu-id="fa231-175">When you start your site out with a layout page, you create content pages without all these elements to begin with.</span></span> <span data-ttu-id="fa231-176">W takim jednak konwertujesz strony autonomicznego na taki, który używa układu, więc ma nieco oczyszczania.</span><span class="sxs-lookup"><span data-stu-id="fa231-176">In this case, though, you're converting a standalone page to one that uses a layout, so there's a bit of cleanup.</span></span>

<span data-ttu-id="fa231-177">Gdy skończysz, *Movies.cshtml* strona będzie wyglądała podobnie do poniższego:</span><span class="sxs-lookup"><span data-stu-id="fa231-177">When you're finished, the *Movies.cshtml* page will look like the following:</span></span>

[!code-cshtml[Main](layouts/samples/sample5.cshtml)]

### <a name="testing-the-layout"></a><span data-ttu-id="fa231-178">Testowanie układu</span><span class="sxs-lookup"><span data-stu-id="fa231-178">Testing the Layout</span></span>

<span data-ttu-id="fa231-179">Teraz możesz zobaczyć, jak wygląda układu.</span><span class="sxs-lookup"><span data-stu-id="fa231-179">Now you can see what the layout looks like.</span></span> <span data-ttu-id="fa231-180">W programie WebMatrix, kliknij prawym przyciskiem myszy *Movies.cshtml* strony i wybierz **Uruchom w przeglądarce**.</span><span class="sxs-lookup"><span data-stu-id="fa231-180">In WebMatrix, right-click the *Movies.cshtml* page and select **Launch in browser**.</span></span> <span data-ttu-id="fa231-181">Jeśli przeglądarka wyświetla stronę, wygląda na tej stronie:</span><span class="sxs-lookup"><span data-stu-id="fa231-181">When the browser displays the page, it looks like this page:</span></span>

![Strona filmy renderowany przy użyciu układu](layouts/_static/image6.png)

<span data-ttu-id="fa231-183">ASP.NET został scalony zawartość strony Movies.cshtml do  *\_Layout.cshtml* stronie odpowiednie miejsce `RenderBody` jest metoda.</span><span class="sxs-lookup"><span data-stu-id="fa231-183">ASP.NET has merged the content of the Movies.cshtml page into the *\_Layout.cshtml* page right where the `RenderBody` method is.</span></span> <span data-ttu-id="fa231-184">I oczywiście  *\_Layout.cshtml* stronie odwołania *.css* pliku, która definiuje wygląd strony.</span><span class="sxs-lookup"><span data-stu-id="fa231-184">And of course the *\_Layout.cshtml* page references a *.css* file that defines the look of the page.</span></span>

## <a name="updating-the-addmovie-page-to-use-the-layout"></a><span data-ttu-id="fa231-185">Aktualizowanie strony AddMovie do użycia w układzie</span><span class="sxs-lookup"><span data-stu-id="fa231-185">Updating the AddMovie Page to Use the Layout</span></span>

<span data-ttu-id="fa231-186">Rzeczywiste zaletą układów jest, której można je na wszystkich stronach w witrynie.</span><span class="sxs-lookup"><span data-stu-id="fa231-186">The real benefit of layouts is that you can use them for all the pages in your site.</span></span> <span data-ttu-id="fa231-187">Otwórz *AddMovie.cshtml* strony.</span><span class="sxs-lookup"><span data-stu-id="fa231-187">Open the *AddMovie.cshtml* page.</span></span>

<span data-ttu-id="fa231-188">Może być Pamiętaj, że *AddMovie.cshtml* strony pierwotnie miały niektóre reguły CSS w nim zdefiniować wygląd komunikatów o błędach weryfikacji.</span><span class="sxs-lookup"><span data-stu-id="fa231-188">You might remember that the *AddMovie.cshtml* page originally had some CSS rules in it to define the look of validation error messages.</span></span> <span data-ttu-id="fa231-189">Ponieważ masz *.css* pliku teraz witryny, można przenieść te reguły *.css* pliku.</span><span class="sxs-lookup"><span data-stu-id="fa231-189">Since you have a *.css* file for your site now, you can move those rules to the *.css* file.</span></span> <span data-ttu-id="fa231-190">Usuń je z *AddMovie.cshtml* plik i dodać je do dolnej części *Movies.css* pliku.</span><span class="sxs-lookup"><span data-stu-id="fa231-190">Remove them from the *AddMovie.cshtml* file and add them to the bottom of the *Movies.css* file.</span></span> <span data-ttu-id="fa231-191">Chcesz przenieść następujące reguły:</span><span class="sxs-lookup"><span data-stu-id="fa231-191">You are moving the following rules:</span></span>

[!code-css[Main](layouts/samples/sample6.css)]

<span data-ttu-id="fa231-192">Teraz wprowadzić tej samej różne rodzaje zmian w *AddMovie.cshtml* wykonanego *Movies.cshtml* — Dodaj `Layout="~/_Layout.cshtml;` i Usuń kod znaczników HTML, która jest teraz nadmiarowe.</span><span class="sxs-lookup"><span data-stu-id="fa231-192">Now make the same sorts of changes in *AddMovie.cshtml* that you did for *Movies.cshtml* — add `Layout="~/_Layout.cshtml;` and remove the HTML markup that's now extraneous.</span></span> <span data-ttu-id="fa231-193">Zmiana `<h1>` elementu `<h2>`.</span><span class="sxs-lookup"><span data-stu-id="fa231-193">Change the `<h1>` element to `<h2>`.</span></span> <span data-ttu-id="fa231-194">Gdy wszystko będzie gotowe, strony będzie wyglądać następująco:</span><span class="sxs-lookup"><span data-stu-id="fa231-194">When you're done, the page will look like this example:</span></span>

[!code-cshtml[Main](layouts/samples/sample7.cshtml)]

<span data-ttu-id="fa231-195">Uruchom stronę.</span><span class="sxs-lookup"><span data-stu-id="fa231-195">Run the page.</span></span> <span data-ttu-id="fa231-196">Teraz wygląda na tej ilustracji:</span><span class="sxs-lookup"><span data-stu-id="fa231-196">Now it looks like this illustration:</span></span>

!["Dodaj filmy" strony renderowany przy użyciu układu](layouts/_static/image7.png)

<span data-ttu-id="fa231-198">Aby wprowadzić zmiany podobne do strony w witrynie — *EditMovie.cshtml* i *DeleteMovie.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="fa231-198">You want to make similar changes to the pages in the site — *EditMovie.cshtml* and *DeleteMovie.cshtml*.</span></span> <span data-ttu-id="fa231-199">Jednak zanim to zrobisz, możesz wprowadzenie innej zmiany do układu, który sprawia, że to nieco bardziej elastyczne.</span><span class="sxs-lookup"><span data-stu-id="fa231-199">However, before you do, you can make another change to the layout that makes it a little more flexible.</span></span>

## <a name="passing-title-information-to-the-layout-page"></a><span data-ttu-id="fa231-200">Informacje o tytułach Przechodzenie do strony układ</span><span class="sxs-lookup"><span data-stu-id="fa231-200">Passing Title Information to the Layout Page</span></span>

<span data-ttu-id="fa231-201">*\_Layout.cshtml* zawiera strony, który został utworzony `<title>` element, który jest ustawiony na "Moja witryna filmu".</span><span class="sxs-lookup"><span data-stu-id="fa231-201">The *\_Layout.cshtml* page that you created has a `<title>` element that's set to "My Movie Site".</span></span> <span data-ttu-id="fa231-202">W większości przeglądarek wyświetlenie zawartości tego elementu jako tekst, na karcie:</span><span class="sxs-lookup"><span data-stu-id="fa231-202">Most browsers display the content of this element as the text on a tab:</span></span>

![Strony &lt;tytuł&gt; element wyświetlane na karcie przeglądarki](layouts/_static/image8.png)

<span data-ttu-id="fa231-204">Informacje o tytułach jest ogólny.</span><span class="sxs-lookup"><span data-stu-id="fa231-204">This title information is generic.</span></span> <span data-ttu-id="fa231-205">Załóżmy, tekst tytułu, aby dokładniej do bieżącej strony.</span><span class="sxs-lookup"><span data-stu-id="fa231-205">Suppose that you want the title text to be more specific to the current page.</span></span> <span data-ttu-id="fa231-206">(Tekst tytułu również służy przez aparaty wyszukiwania do określania strona jest o.) Możesz przekazać informacje ze strony zawartości, takich jak *Movies.cshtml* lub *AddMovie.cshtml* do strony układu i następnie użyć tych informacji, aby dostosować stronę układu renderuje.</span><span class="sxs-lookup"><span data-stu-id="fa231-206">(The title text is also used by search engines to determine what your page is about.) You can pass information from a content page like *Movies.cshtml* or *AddMovie.cshtml* to the layout page, and then use that information to customize what the layout page renders.</span></span>

<span data-ttu-id="fa231-207">Otwórz *Movies.cshtml* strony.</span><span class="sxs-lookup"><span data-stu-id="fa231-207">Open the *Movies.cshtml* page again.</span></span> <span data-ttu-id="fa231-208">W kodzie u góry Dodaj następujący wiersz:</span><span class="sxs-lookup"><span data-stu-id="fa231-208">In the code at the top, add the following line:</span></span>

[!code-csharp[Main](layouts/samples/sample8.cs)]

<span data-ttu-id="fa231-209">`Page` Obiekt jest dostępny na wszystkich *.cshtml* strony i jest w tym celu, a mianowicie udostępnianie informacji między stroną i jego układu.</span><span class="sxs-lookup"><span data-stu-id="fa231-209">The `Page` object is available on all *.cshtml* pages and is for this purpose, namely to share information between a page and its layout.</span></span>

<span data-ttu-id="fa231-210">Otwórz<em>\_Layout.cshtml</em> strony.</span><span class="sxs-lookup"><span data-stu-id="fa231-210">Open the<em>\_Layout.cshtml</em> page.</span></span> <span data-ttu-id="fa231-211">Zmiana `<title>` elementu, tak że wygląda ten kod znaczników:</span><span class="sxs-lookup"><span data-stu-id="fa231-211">Change the `<title>` element so that it looks like this markup:</span></span>

[!code-html[Main](layouts/samples/sample9.html)]

<span data-ttu-id="fa231-212">Ten kod renderuje, niezależnie od rodzaju znajduje się w `Page.Title` właściwość bezpośrednio w tej lokalizacji, na stronie.</span><span class="sxs-lookup"><span data-stu-id="fa231-212">This code renders whatever is in the `Page.Title` property right at that location in the page.</span></span>

<span data-ttu-id="fa231-213">Uruchom *Movies.cshtml* strony.</span><span class="sxs-lookup"><span data-stu-id="fa231-213">Run the *Movies.cshtml* page.</span></span> <span data-ttu-id="fa231-214">Tym razem karty przeglądarki pokazuje przekazany jako wartość `Page.Title`:</span><span class="sxs-lookup"><span data-stu-id="fa231-214">This time the browser tab shows what you passed as the value of `Page.Title`:</span></span>

![Na karcie przeglądarki, przedstawiający tytuł tworzone dynamicznie](layouts/_static/image9.png)

<span data-ttu-id="fa231-216">Jeśli chcesz, wyświetlić źródło strony w przeglądarce.</span><span class="sxs-lookup"><span data-stu-id="fa231-216">If you want, view the page source in the browser.</span></span> <span data-ttu-id="fa231-217">Możesz zobaczyć, że `<title>` element jest renderowany jako `<title>List Movies</title>`.</span><span class="sxs-lookup"><span data-stu-id="fa231-217">You can see that the `<title>` element is rendered as `<title>List Movies</title>`.</span></span>

> [!TIP] 
> 
> <span data-ttu-id="fa231-218">**Obiekt strony**</span><span class="sxs-lookup"><span data-stu-id="fa231-218">**The Page Object**</span></span>
> 
> <span data-ttu-id="fa231-219">Przydatną cechą `Page` jest to obiekt dynamiczny — `Title` właściwość nie jest nazwą fixed lub zastrzeżonych.</span><span class="sxs-lookup"><span data-stu-id="fa231-219">A useful feature of `Page` is that it's a dynamic object — the `Title` property is not a fixed or reserved name.</span></span> <span data-ttu-id="fa231-220">Możesz użyć *wszelkie* nazwę wartości `Page` obiektu.</span><span class="sxs-lookup"><span data-stu-id="fa231-220">You can use *any* name for a value of the `Page` object.</span></span> <span data-ttu-id="fa231-221">Na przykład, można łatwo przekazana tytuł przy użyciu właściwości o nazwie `Page.CurrentName` lub `Page.MyPage`.</span><span class="sxs-lookup"><span data-stu-id="fa231-221">For example, you could as easily have passed the title by using a property named `Page.CurrentName` or `Page.MyPage`.</span></span> <span data-ttu-id="fa231-222">Jedynym ograniczeniem jest, że nazwa ta musi postępuj zgodnie z normalnym zasady mogą mieć nazwę właściwości.</span><span class="sxs-lookup"><span data-stu-id="fa231-222">The only restriction is that the name has to follow the normal rules for what properties can be named.</span></span> <span data-ttu-id="fa231-223">(Na przykład nazwa nie może zawierać spacji).</span><span class="sxs-lookup"><span data-stu-id="fa231-223">(For example, the name can't contain a space.)</span></span>
> 
> <span data-ttu-id="fa231-224">Można przekazać dowolną liczbę wartości za pomocą `Page` obiektu.</span><span class="sxs-lookup"><span data-stu-id="fa231-224">You can pass any number of values by using the `Page` object.</span></span> <span data-ttu-id="fa231-225">Jeśli chcesz przekazać informacje o filmach do strony układu można przekazać wartości, przy użyciu polecenia podobnego `Page.MovieTitle` i `Page.Genre` i `Page.MovieYear`.</span><span class="sxs-lookup"><span data-stu-id="fa231-225">If you wanted to pass movie information to the layout page, you could pass values by using something like `Page.MovieTitle` and `Page.Genre` and `Page.MovieYear`.</span></span> <span data-ttu-id="fa231-226">(Lub innych nazw, które opracowali do przechowywania informacji.) Jedynym wymaganiem — czyli prawdopodobnie oczywiste — jest konieczne przy użyciu tych samych nazw w zawartości strony i strony układu.</span><span class="sxs-lookup"><span data-stu-id="fa231-226">(Or any other names that you invented to store the information.) The only requirement — which is probably obvious — is that you have to use the same names in the content page and the layout page.</span></span>
> 
> <span data-ttu-id="fa231-227">Informacje przekazywane za pomocą `Page` obiekt nie jest ograniczona do po prostu tekst do wyświetlenia na stronie układu.</span><span class="sxs-lookup"><span data-stu-id="fa231-227">The information you pass by using the `Page` object isn't limited to just text to display on the layout page.</span></span> <span data-ttu-id="fa231-228">Można przekazać wartość do strony układu, a następnie kod w stronę układu można użyć wartości zdecydować, czy mają być wyświetlane w sekcji strony, co *.css* plików do użycia i tak dalej.</span><span class="sxs-lookup"><span data-stu-id="fa231-228">You can pass a value to the layout page, and then code in the layout page can use the value to decide whether to display a section of the page, what *.css* file to use, and so on.</span></span> <span data-ttu-id="fa231-229">Wartości są przekazywane w `Page` obiektu są tak jak inne wartości użycia w kodzie.</span><span class="sxs-lookup"><span data-stu-id="fa231-229">The values you pass in the `Page` object are like any other values that you use in code.</span></span> <span data-ttu-id="fa231-230">Jest po prostu czy wartości pochodzą z poziomu strony zawartości i są przekazywane do strony układu.</span><span class="sxs-lookup"><span data-stu-id="fa231-230">It's just that the values originate in the content page and are passed to the layout page.</span></span>


<span data-ttu-id="fa231-231">Otwórz *AddMovie.cshtml* strony, a następnie dodaj wiersz na początku kod, który zawiera tytuł *AddMovie.cshtml* strony:</span><span class="sxs-lookup"><span data-stu-id="fa231-231">Open the *AddMovie.cshtml* page and add a line to the top of the code that provides a title for the *AddMovie.cshtml* page:</span></span>

[!code-csharp[Main](layouts/samples/sample10.cs)]

<span data-ttu-id="fa231-232">Uruchom *AddMovie.cshtml* strony.</span><span class="sxs-lookup"><span data-stu-id="fa231-232">Run the *AddMovie.cshtml* page.</span></span> <span data-ttu-id="fa231-233">Zostanie wyświetlony nowy tytuł istnieje:</span><span class="sxs-lookup"><span data-stu-id="fa231-233">You see the new title there:</span></span>

![Na karcie przeglądarki, przedstawiający tytuł "Dodaj filmy" tworzone dynamicznie](layouts/_static/image10.png)

## <a name="updating-the-remaining-pages-to-use-the-layout"></a><span data-ttu-id="fa231-235">Aktualizowanie pozostałe strony do użycia w układzie</span><span class="sxs-lookup"><span data-stu-id="fa231-235">Updating the Remaining Pages to Use the Layout</span></span>

<span data-ttu-id="fa231-236">Teraz można dokończyć pozostałe strony w witrynie, tak aby używały nowego układu.</span><span class="sxs-lookup"><span data-stu-id="fa231-236">Now you can finish the remaining pages in your site so that they use the new layout.</span></span> <span data-ttu-id="fa231-237">Otwórz *EditMovie.cshtml* i *DeleteMovie.cshtml* z kolei i wprowadzenie identycznych zmian w każdym.</span><span class="sxs-lookup"><span data-stu-id="fa231-237">Open *EditMovie.cshtml* and *DeleteMovie.cshtml* in turn and make the same changes in each.</span></span>

<span data-ttu-id="fa231-238">Dodaj wiersz kodu, który stanowi łącze do strony układu:</span><span class="sxs-lookup"><span data-stu-id="fa231-238">Add the line of code that links to the layout page:</span></span>

[!code-csharp[Main](layouts/samples/sample11.cs)]

<span data-ttu-id="fa231-239">Dodaj wiersz, aby ustawić tytuł strony:</span><span class="sxs-lookup"><span data-stu-id="fa231-239">Add a line to set the title of the page:</span></span>

[!code-csharp[Main](layouts/samples/sample12.cs)]

<span data-ttu-id="fa231-240">lub:</span><span class="sxs-lookup"><span data-stu-id="fa231-240">or:</span></span>

[!code-csharp[Main](layouts/samples/sample13.cs)]

<span data-ttu-id="fa231-241">Usuń wszystkie nadmiarowe kod znaczników HTML — po prostu Pozostaw tylko bity, które znajdują się wewnątrz `<body>` — element (plus blok kodu, u góry).</span><span class="sxs-lookup"><span data-stu-id="fa231-241">Remove all the extraneous HTML markup — basically, leave only the bits that are inside the `<body>` element (plus the code block at the top).</span></span>

<span data-ttu-id="fa231-242">Zmiana `<h1>` element ma być `<h2>` elementu.</span><span class="sxs-lookup"><span data-stu-id="fa231-242">Change the `<h1>` element to be an `<h2>` element.</span></span>

<span data-ttu-id="fa231-243">Po wprowadzeniu tych zmian, każdy test i upewnij się, że są wyświetlane prawidłowo i czy tytuł jest poprawna.</span><span class="sxs-lookup"><span data-stu-id="fa231-243">When you've made these changes, test each and make sure that it's displaying properly and that the title is correct.</span></span>

## <a name="parting-thoughts-about-layout-pages"></a><span data-ttu-id="fa231-244">Towarzyszącą pomysł, strony układu</span><span class="sxs-lookup"><span data-stu-id="fa231-244">Parting Thoughts About Layout Pages</span></span>

<span data-ttu-id="fa231-245">W tym samouczku utworzono  *\_Layout.cshtml* strony, a następnie użyć `RenderBody` metodę, aby scalić zawartości z innej strony.</span><span class="sxs-lookup"><span data-stu-id="fa231-245">In this tutorial you created a *\_Layout.cshtml* page and used the `RenderBody` method to merge content from another page.</span></span> <span data-ttu-id="fa231-246">To podstawowy wzorzec na stronach sieci Web za pomocą układów.</span><span class="sxs-lookup"><span data-stu-id="fa231-246">That's the basic pattern for using layouts in Web Pages.</span></span>

<span data-ttu-id="fa231-247">Układ strony mają dodatkowe funkcje, które nie zostały omówione w tym miejscu.</span><span class="sxs-lookup"><span data-stu-id="fa231-247">Layout pages have additional features that we didn't cover here.</span></span> <span data-ttu-id="fa231-248">Na przykład, można zagnieżdżać strony układu — jedną stronę układu można z kolei odwoływać się do innego.</span><span class="sxs-lookup"><span data-stu-id="fa231-248">For example, you can nest layout pages — one layout page can in turn reference another.</span></span> <span data-ttu-id="fa231-249">Układy zagnieżdżonych mogą być przydatne, jeśli pracujesz z podsekcje lokacji, które wymagają różnych układów.</span><span class="sxs-lookup"><span data-stu-id="fa231-249">Nested layouts can be useful if you're working with subsections of a site that require different layouts.</span></span> <span data-ttu-id="fa231-250">Można również użyć dodatkowych metod (na przykład `RenderSection`) do skonfigurowania o nazwie sekcjach strony układu.</span><span class="sxs-lookup"><span data-stu-id="fa231-250">You can also use additional methods (for example, `RenderSection`) to set up named sections in the layout page.</span></span>

<span data-ttu-id="fa231-251">Połączenie strony układu i *.css* plików to zaawansowane.</span><span class="sxs-lookup"><span data-stu-id="fa231-251">The combination of layout pages and *.css* files is powerful.</span></span> <span data-ttu-id="fa231-252">Jak zobaczysz w następnej serii samouczków, w programie WebMatrix można utworzyć witrynę na podstawie *szablonu*, które zapewnia witryny, która ma wbudowanych funkcji w nim.</span><span class="sxs-lookup"><span data-stu-id="fa231-252">As you'll see in the next tutorial series, in WebMatrix you can create a site based on a *template*, which gives you a site that has prebuilt functionality in it.</span></span> <span data-ttu-id="fa231-253">Szablony umożliwiają pełne wykorzystanie strony układu i arkusze CSS do tworzenia witryn, który wyglądał świetnie, które mają funkcje, takie jak menu.</span><span class="sxs-lookup"><span data-stu-id="fa231-253">The templates make good use of layout pages and CSS to create sites that look great and that have features like menus.</span></span> <span data-ttu-id="fa231-254">Poniżej przedstawiono zrzut ekranu strony głównej z lokacji, na podstawie szablonu, przedstawiający funkcji używających strony układu i arkusze CSS:</span><span class="sxs-lookup"><span data-stu-id="fa231-254">Here's a screenshot of the home page from a site based on a template, showing features that use layout pages and CSS:</span></span>

![Układ utworzone przez wyświetlanie nagłówka, obszar nawigacji, obszaru zawartości, opcjonalna sekcja i łącza logowania szablonu witryny programu WebMatrix](layouts/_static/image11.png)

## <a name="complete-listing-for-movie-page-updated-to-use-a-layout-page"></a><span data-ttu-id="fa231-256">Kompletna lista strony filmu (zaktualizowana w celu używania strony układu)</span><span class="sxs-lookup"><span data-stu-id="fa231-256">Complete Listing for Movie Page (Updated to Use a Layout Page)</span></span>

[!code-cshtml[Main](layouts/samples/sample14.cshtml)]

## <a name="complete-page-listing-for-add-movie-page-updated-for-layout"></a><span data-ttu-id="fa231-257">Strona ukończone dla dodać strony filmu (Aktualizacja dla układu)</span><span class="sxs-lookup"><span data-stu-id="fa231-257">Complete Page Listing for Add Movie Page (Updated for Layout)</span></span>

[!code-cshtml[Main](layouts/samples/sample15.cshtml)]

## <a name="complete-page-listing-for-delete-movie-page-updated-for-layout"></a><span data-ttu-id="fa231-258">Kompletna lista strony dla strony filmu Delete (Aktualizacja dla układu)</span><span class="sxs-lookup"><span data-stu-id="fa231-258">Complete Page Listing for Delete Movie Page (Updated for Layout)</span></span>

[!code-cshtml[Main](layouts/samples/sample16.cshtml)]

## <a name="complete-page-listing-for-edit-movie-page-updated-for-layout"></a><span data-ttu-id="fa231-259">Kompletna lista strony dla strony filmu edycji (Aktualizacja dla układu)</span><span class="sxs-lookup"><span data-stu-id="fa231-259">Complete Page Listing for Edit Movie Page (Updated for Layout)</span></span>

[!code-cshtml[Main](layouts/samples/sample17.cshtml)]

## <a name="coming-up-next"></a><span data-ttu-id="fa231-260">Pojawi się dalej</span><span class="sxs-lookup"><span data-stu-id="fa231-260">Coming Up Next</span></span>

<span data-ttu-id="fa231-261">W następnym samouczku dowiesz się, jak opublikowanie witryny z Internetem, dzięki czemu wszyscy mogą oglądać.</span><span class="sxs-lookup"><span data-stu-id="fa231-261">In the next tutorial, you'll learn how to publish your site to the Internet so everyone can see it.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="fa231-262">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="fa231-262">Additional Resources</span></span>

- <span data-ttu-id="fa231-263">[Tworzenie spójnego Szukaj](https://go.microsoft.com/fwlink/?LinkID=202891) — artykułu, który zawiera więcej szczegółów na temat pracy z układów.</span><span class="sxs-lookup"><span data-stu-id="fa231-263">[Creating a Consistent Look](https://go.microsoft.com/fwlink/?LinkID=202891) — An article that provides some more detail on working with layouts.</span></span> <span data-ttu-id="fa231-264">Zawiera opis również sposób przekazywania wartości do strony układu, który pokazuje lub ukrywa część zawartości.</span><span class="sxs-lookup"><span data-stu-id="fa231-264">It also describes how to pass a value to a layout page that shows or hides some of the content.</span></span>
- <span data-ttu-id="fa231-265">[Zagnieżdżone strony układu ze składnią Razor](http://www.mikesdotnetting.com/Article/164/Nested-Layout-Pages-with-Razor) — blogi Mike Brind przykładowy sposób zagnieździć strony układu.</span><span class="sxs-lookup"><span data-stu-id="fa231-265">[Nested Layout Pages with Razor](http://www.mikesdotnetting.com/Article/164/Nested-Layout-Pages-with-Razor) — Mike Brind blogs an example of how to nest layout pages.</span></span> <span data-ttu-id="fa231-266">(Obejmuje pobierania stron).</span><span class="sxs-lookup"><span data-stu-id="fa231-266">(Includes a download of the pages.)</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="fa231-267">[Poprzednie](deleting-data.md)
> [dalej](publishing.md)</span><span class="sxs-lookup"><span data-stu-id="fa231-267">[Previous](deleting-data.md)
[Next](publishing.md)</span></span>
