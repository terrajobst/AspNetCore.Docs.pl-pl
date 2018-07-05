---
uid: web-pages/overview/getting-started/introducing-aspnet-web-pages-2/layouts
title: Wprowadzenie do składnika ASP.NET Web Pages — tworzenie spójnego układu | Dokumentacja firmy Microsoft
author: tfitzmac
description: W tym samouczku dowiesz się, jak za pomocą układów tworzenie spójnego wyglądu dla stron w lokacji korzystającej z ASP.NET Web Pages. Przyjęto założenie, że zostały wykonane...
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/28/2015
ms.topic: article
ms.assetid: c85ec591-f8d7-4882-b763-de6ab9f3df7a
ms.technology: dotnet-webpages
msc.legacyurl: /web-pages/overview/getting-started/introducing-aspnet-web-pages-2/layouts
msc.type: authoredcontent
ms.openlocfilehash: 3368a3e3c9dc56b98ca0adddf4ffb181c7b34863
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/03/2018
ms.locfileid: "37379450"
---
<a name="introducing-aspnet-web-pages---creating-a-consistent-layout"></a>Wprowadzenie do wzorca ASP.NET Web Pages — tworzenie spójnego układu
====================
przez [Tom FitzMacken](https://github.com/tfitzmac)

> W tym samouczku dowiesz się, jak używać *układy* do tworzenia spójnego wyglądu dla stron w lokacji korzystającej z ASP.NET Web Pages. Przyjęto założenie, że zostały wykonane serii za pośrednictwem [usuwanie danych z baz danych w programie ASP.NET Web Pages](https://go.microsoft.com/fwlink/?LinkId=251584).
> 
> Zawartość:
> 
> - Strona układu jest.
> - Jak połączyć układu strony z zawartością dynamiczną.
> - Jak przekazać wartości do strony układu.


## <a name="about-layouts"></a>Układy — informacje

Strony utworzone do tej pory wszystkie zostały zakończone, stron autonomicznych. Wszystkie one należeć do tej samej lokacji, ale nie mają żadnych wspólnych elementów lub standardowy wygląd.

Większość witryn mają spójnego wyglądu i układ. Na przykład, jeśli przejdziesz do [Microsoft.com/web](https://www.microsoft.com/web/) lokacji i poszukaj, zobaczysz, że wszystkie strony stosować ogólny układ i motyw wizualny:

![Strona witryny Microsoft.com/Web przedstawiający układ nagłówka, obszar nawigacji, obszarem zawartości i stopka](layouts/_static/image1.png)

*Nieefektywne* sposób, aby utworzyć ten układ jest zdefiniować nagłówek, pasek nawigacyjny i stopka osobno dla poszczególnych stron sieci. Można będzie się duplikowania ten sam kod znaczników każdorazowo. Jeśli chcesz coś zmienić (na przykład aktualizacja stopki), trzeba zmienić każdą stronę oddzielnie.

To tutaj *strony układu* pochodzą. W składniku ASP.NET Web Pages można zdefiniować stronę układu, oferująca pojemniku ogólnego stron w witrynie. Na przykład strony układu może zawierać nagłówek, obszar nawigacji i stopki. Strona układu zawiera symbol zastępczy, gdzie należy wstawić zawartość głównego.

Następnie można zdefiniować poszczególne strony z zawartością, które zawierają znaczników i kodu tylko na tej stronie. Strony z zawartością nie muszą być ukończone stron HTML. jeszcze nie mają mieć `<body>` elementu. Mają one linię kodu, który informuje, jakie stronę układu, w której chcesz wyświetlać zawartość w ASP.NET. Oto obraz, który około pokazuje, jak działa ta relacja:

![Diagram koncepcyjny przedstawiający dwóch stronach zawartości i stronę układu, w którym mieściły się](layouts/_static/image2.png)

Ta interakcja jest łatwa do zrozumienia, kiedy zostanie wyświetlony w działaniu. W tym samouczku zmienisz stron sieci filmy, aby używać układu.

## <a name="adding-a-layout-page"></a>Dodawanie strony układu

Będziesz rozpoczyna się od utworzenia strony układu, który definiuje układ strony typowego nagłówka, stopki i obszar do głównej zawartości. W witrynie WebPagesMovies Dodaj stronę CSHTML o nazwie  *\_Layout.cshtml*.

Wiodący znak podkreślenia ( `_` ) znak jest znacząca. Jeśli na stronie nazwa zaczyna się od znaku podkreślenia, ASP.NET nie bezpośrednio wysłać tę stronę w przeglądarce. Ta konwencja pozwala zdefiniować stron, które są wymagane dla danej witryny, jednak, że użytkownicy nie powinien móc żądać bezpośrednio.

Zastąp zawartość strony następujących czynności:

[!code-html[Main](layouts/samples/sample1.html)]

Jak widać, ten kod znaczników jest po prostu HTML, który używa `<div>` elementy, aby zdefiniować trzy sekcje w stronę plus 1 bardziej `<div>` elementu do przechowywania trzy sekcje. Stopka zawiera ilość kodu Razor: `@DateTime.Now.Year`, który będzie renderowany w bieżącym roku w tej lokalizacji, na stronie.

Należy zauważyć, że znajduje się link do arkusza stylów, o nazwie *Movies.css*. Arkusz stylów jest, gdzie można zdefiniować szczegóły fizycznego układu elementów. Instrukcje dotyczące tworzenia, za chwilę.

Tylko niezwykłych funkcji, w tym  *\_Layout.cshtml* strona jest `@Render.Body()` wiersza. To symbol zastępczy, gdzie zawartość, gdy ten układ jest scalany z innej strony.

## <a name="adding-a-css-file"></a>Dodawanie pliku .css

Preferowanym sposobem zdefiniowania układowi (czyli wygląd) elementów na stronie jest użyć reguły stylów kaskadowych w arkusz (CSS). Dlatego należy utworzyć *.css* pliku, który ma reguły dla nowego układu.

W programie WebMatrix wybierz katalog główny witryny. Następnie w **pliki** karty na wstążce kliknij strzałkę w obszarze **New** przycisk, a następnie kliknij przycisk **nowy Folder**.

![Opcję "Nowy Folder" w obszarze nowe na Wstążce.](layouts/_static/image3.png)

Nadaj nazwę nowego folderu *style*.

![Nazewnictwo nowy folder "Style"](layouts/_static/image4.png)

W nowym *style* folderze utwórz plik o nazwie *Movies.css*.

![Tworzenie nowego pliku Movies.css](layouts/_static/image5.png)

Zastąp zawartość nowego *.css* pliku następującym kodem:

[!code-css[Main](layouts/samples/sample2.css)]

Firma Microsoft nie będzie, że te informacje, te reguły CSS, z wyjątkiem dwie czynności Pamiętaj, aby. Jeden jest oprócz ustawienia czcionek i rozmiarów, reguły pozycjonowanie absolutne ustanowienie lokalizacji nagłówek, stopka i główny obszar zawartości. Jeśli jesteś nowym użytkownikiem pozycjonowania w kodzie CSS, możesz przeczytać [pozycjonowania CSS](http://www.w3schools.com/css/css_positioning.asp) samouczek w witrynie W3Schools.

Jeszcze inną czynność należy pamiętać, zdefiniowano, u dołu skopiowano możemy reguły stylów, które były pierwotnie indywidualnie w *Movies.cshtml* pliku. Te reguły zostały użyte w [wprowadzenie do wyświetlania danych przy użyciu stron ASP.NET Web Pages](https://go.microsoft.com/fwlink/?LinkId=251580) samouczka, aby wprowadzić `WebGrid` pomocnika renderowania kodu znaczników dodaną rozkłada do tabeli. (Jeśli zamierzasz używać *.css* pliku definicji stylów może także stanowić reguł stylu dla całej witryny w nim.)

## <a name="updating-the-movies-file-to-use-the-layout"></a>Aktualizowanie pliku filmy do użycia w układzie

Można teraz zaktualizować istniejące pliki w tej witryny używanie nowego układu. Otwórz *Movies.cshtml* pliku. U góry strony w pierwszym wierszu kodu, Dodaj następujący fragment kodu:

[!code-csharp[Main](layouts/samples/sample3.cs)]

Strona teraz rozpoczyna się w ten sposób:

[!code-cshtml[Main](layouts/samples/sample4.cshtml?highlight=2)]

To jeden wiersz kodu informuje ASP.NET że w przypadku *filmy* uruchamia stronę, powinny zostać scalone z  *\_Layout.cshtml* pliku.

Ponieważ *Movies.cshtml* plik używa teraz stronę układu, możesz usunąć znaczników z *Movies.cshtml* strona, która jest wykonywane przez  *\_Layout.cshtml*pliku. Opróżnij `<!DOCTYPE>`, `<html>`, i `<body>` znaczników otwierających i zamykających. W działaniu całej `<head>` elementu i jego zawartości, które obejmują reguły stylów siatki, ponieważ masz teraz tych zasad w *.css* pliku. Podczas pracy w tym zmienić istniejące `<h1>` elementu `<h2>` element; ma `<h1>` element już strony układu. Zmiana `<h2>` tekst "Listy filmów".

Zazwyczaj nie trzeba wprowadzać tego rodzaju zmian w zawartości strony. Po uruchomieniu witryny ze stroną układu tworzenia stron zawartości bez tych elementów rozpoczynać się. W takim jednak konwertujesz strony autonomicznego na taki, który używa układu, więc ma nieco oczyszczania.

Gdy skończysz, *Movies.cshtml* strona będzie wyglądała podobnie do poniższego:

[!code-cshtml[Main](layouts/samples/sample5.cshtml)]

### <a name="testing-the-layout"></a>Testowanie układu

Teraz możesz zobaczyć, jak wygląda układu. W programie WebMatrix, kliknij prawym przyciskiem myszy *Movies.cshtml* strony i wybierz **Uruchom w przeglądarce**. Jeśli przeglądarka wyświetla stronę, wygląda na tej stronie:

![Strona filmy renderowany przy użyciu układu](layouts/_static/image6.png)

ASP.NET został scalony zawartość strony Movies.cshtml do  *\_Layout.cshtml* stronie odpowiednie miejsce `RenderBody` jest metoda. I oczywiście  *\_Layout.cshtml* stronie odwołania *.css* pliku, która definiuje wygląd strony.

## <a name="updating-the-addmovie-page-to-use-the-layout"></a>Aktualizowanie strony AddMovie do użycia w układzie

Rzeczywiste zaletą układów jest, której można je na wszystkich stronach w witrynie. Otwórz *AddMovie.cshtml* strony.

Może być Pamiętaj, że *AddMovie.cshtml* strony pierwotnie miały niektóre reguły CSS w nim zdefiniować wygląd komunikatów o błędach weryfikacji. Ponieważ masz *.css* pliku teraz witryny, można przenieść te reguły *.css* pliku. Usuń je z *AddMovie.cshtml* plik i dodać je do dolnej części *Movies.css* pliku. Chcesz przenieść następujące reguły:

[!code-css[Main](layouts/samples/sample6.css)]

Teraz wprowadzić tej samej różne rodzaje zmian w *AddMovie.cshtml* wykonanego *Movies.cshtml* — Dodaj `Layout="~/_Layout.cshtml;` i Usuń kod znaczników HTML, która jest teraz nadmiarowe. Zmiana `<h1>` elementu `<h2>`. Gdy wszystko będzie gotowe, strony będzie wyglądać następująco:

[!code-cshtml[Main](layouts/samples/sample7.cshtml)]

Uruchom stronę. Teraz wygląda na tej ilustracji:

!["Dodaj filmy" strony renderowany przy użyciu układu](layouts/_static/image7.png)

Aby wprowadzić zmiany podobne do strony w witrynie — *EditMovie.cshtml* i *DeleteMovie.cshtml*. Jednak zanim to zrobisz, możesz wprowadzenie innej zmiany do układu, który sprawia, że to nieco bardziej elastyczne.

## <a name="passing-title-information-to-the-layout-page"></a>Informacje o tytułach Przechodzenie do strony układ

*\_Layout.cshtml* zawiera strony, który został utworzony `<title>` element, który jest ustawiony na "Moja witryna filmu". W większości przeglądarek wyświetlenie zawartości tego elementu jako tekst, na karcie:

![Strony &lt;tytuł&gt; element wyświetlane na karcie przeglądarki](layouts/_static/image8.png)

Informacje o tytułach jest ogólny. Załóżmy, tekst tytułu, aby dokładniej do bieżącej strony. (Tekst tytułu również służy przez aparaty wyszukiwania do określania strona jest o.) Możesz przekazać informacje ze strony zawartości, takich jak *Movies.cshtml* lub *AddMovie.cshtml* do strony układu i następnie użyć tych informacji, aby dostosować stronę układu renderuje.

Otwórz *Movies.cshtml* strony. W kodzie u góry Dodaj następujący wiersz:

[!code-csharp[Main](layouts/samples/sample8.cs)]

`Page` Obiekt jest dostępny na wszystkich *.cshtml* strony i jest w tym celu, a mianowicie udostępnianie informacji między stroną i jego układu.

Otwórz<em>\_Layout.cshtml</em> strony. Zmiana `<title>` elementu, tak że wygląda ten kod znaczników:

[!code-html[Main](layouts/samples/sample9.html)]

Ten kod renderuje, niezależnie od rodzaju znajduje się w `Page.Title` właściwość bezpośrednio w tej lokalizacji, na stronie.

Uruchom *Movies.cshtml* strony. Tym razem karty przeglądarki pokazuje przekazany jako wartość `Page.Title`:

![Na karcie przeglądarki, przedstawiający tytuł tworzone dynamicznie](layouts/_static/image9.png)

Jeśli chcesz, wyświetlić źródło strony w przeglądarce. Możesz zobaczyć, że `<title>` element jest renderowany jako `<title>List Movies</title>`.

> [!TIP] 
> 
> **Obiekt strony**
> 
> Przydatną cechą `Page` jest to obiekt dynamiczny — `Title` właściwość nie jest nazwą fixed lub zastrzeżonych. Możesz użyć *wszelkie* nazwę wartości `Page` obiektu. Na przykład, można łatwo przekazana tytuł przy użyciu właściwości o nazwie `Page.CurrentName` lub `Page.MyPage`. Jedynym ograniczeniem jest, że nazwa ta musi postępuj zgodnie z normalnym zasady mogą mieć nazwę właściwości. (Na przykład nazwa nie może zawierać spacji).
> 
> Można przekazać dowolną liczbę wartości za pomocą `Page` obiektu. Jeśli chcesz przekazać informacje o filmach do strony układu można przekazać wartości, przy użyciu polecenia podobnego `Page.MovieTitle` i `Page.Genre` i `Page.MovieYear`. (Lub innych nazw, które opracowali do przechowywania informacji.) Jedynym wymaganiem — czyli prawdopodobnie oczywiste — jest konieczne przy użyciu tych samych nazw w zawartości strony i strony układu.
> 
> Informacje przekazywane za pomocą `Page` obiekt nie jest ograniczona do po prostu tekst do wyświetlenia na stronie układu. Można przekazać wartość do strony układu, a następnie kod w stronę układu można użyć wartości zdecydować, czy mają być wyświetlane w sekcji strony, co *.css* plików do użycia i tak dalej. Wartości są przekazywane w `Page` obiektu są tak jak inne wartości użycia w kodzie. Jest po prostu czy wartości pochodzą z poziomu strony zawartości i są przekazywane do strony układu.


Otwórz *AddMovie.cshtml* strony, a następnie dodaj wiersz na początku kod, który zawiera tytuł *AddMovie.cshtml* strony:

[!code-csharp[Main](layouts/samples/sample10.cs)]

Uruchom *AddMovie.cshtml* strony. Zostanie wyświetlony nowy tytuł istnieje:

![Na karcie przeglądarki, przedstawiający tytuł "Dodaj filmy" tworzone dynamicznie](layouts/_static/image10.png)

## <a name="updating-the-remaining-pages-to-use-the-layout"></a>Aktualizowanie pozostałe strony do użycia w układzie

Teraz można dokończyć pozostałe strony w witrynie, tak aby używały nowego układu. Otwórz *EditMovie.cshtml* i *DeleteMovie.cshtml* z kolei i wprowadzenie identycznych zmian w każdym.

Dodaj wiersz kodu, który stanowi łącze do strony układu:

[!code-csharp[Main](layouts/samples/sample11.cs)]

Dodaj wiersz, aby ustawić tytuł strony:

[!code-csharp[Main](layouts/samples/sample12.cs)]

lub:

[!code-csharp[Main](layouts/samples/sample13.cs)]

Usuń wszystkie nadmiarowe kod znaczników HTML — po prostu Pozostaw tylko bity, które znajdują się wewnątrz `<body>` — element (plus blok kodu, u góry).

Zmiana `<h1>` element ma być `<h2>` elementu.

Po wprowadzeniu tych zmian, każdy test i upewnij się, że są wyświetlane prawidłowo i czy tytuł jest poprawna.

## <a name="parting-thoughts-about-layout-pages"></a>Towarzyszącą pomysł, strony układu

W tym samouczku utworzono  *\_Layout.cshtml* strony, a następnie użyć `RenderBody` metodę, aby scalić zawartości z innej strony. To podstawowy wzorzec na stronach sieci Web za pomocą układów.

Układ strony mają dodatkowe funkcje, które nie zostały omówione w tym miejscu. Na przykład, można zagnieżdżać strony układu — jedną stronę układu można z kolei odwoływać się do innego. Układy zagnieżdżonych mogą być przydatne, jeśli pracujesz z podsekcje lokacji, które wymagają różnych układów. Można również użyć dodatkowych metod (na przykład `RenderSection`) do skonfigurowania o nazwie sekcjach strony układu.

Połączenie strony układu i *.css* plików to zaawansowane. Jak zobaczysz w następnej serii samouczków, w programie WebMatrix można utworzyć witrynę na podstawie *szablonu*, które zapewnia witryny, która ma wbudowanych funkcji w nim. Szablony umożliwiają pełne wykorzystanie strony układu i arkusze CSS do tworzenia witryn, który wyglądał świetnie, które mają funkcje, takie jak menu. Poniżej przedstawiono zrzut ekranu strony głównej z lokacji, na podstawie szablonu, przedstawiający funkcji używających strony układu i arkusze CSS:

![Układ utworzone przez wyświetlanie nagłówka, obszar nawigacji, obszaru zawartości, opcjonalna sekcja i łącza logowania szablonu witryny programu WebMatrix](layouts/_static/image11.png)

## <a name="complete-listing-for-movie-page-updated-to-use-a-layout-page"></a>Kompletna lista strony filmu (zaktualizowana w celu używania strony układu)

[!code-cshtml[Main](layouts/samples/sample14.cshtml)]

## <a name="complete-page-listing-for-add-movie-page-updated-for-layout"></a>Strona ukończone dla dodać strony filmu (Aktualizacja dla układu)

[!code-cshtml[Main](layouts/samples/sample15.cshtml)]

## <a name="complete-page-listing-for-delete-movie-page-updated-for-layout"></a>Kompletna lista strony dla strony filmu Delete (Aktualizacja dla układu)

[!code-cshtml[Main](layouts/samples/sample16.cshtml)]

## <a name="complete-page-listing-for-edit-movie-page-updated-for-layout"></a>Kompletna lista strony dla strony filmu edycji (Aktualizacja dla układu)

[!code-cshtml[Main](layouts/samples/sample17.cshtml)]

## <a name="coming-up-next"></a>Pojawi się dalej

W następnym samouczku dowiesz się, jak opublikowanie witryny z Internetem, dzięki czemu wszyscy mogą oglądać.

## <a name="additional-resources"></a>Dodatkowe zasoby

- [Tworzenie spójnego Szukaj](https://go.microsoft.com/fwlink/?LinkID=202891) — artykułu, który zawiera więcej szczegółów na temat pracy z układów. Zawiera opis również sposób przekazywania wartości do strony układu, który pokazuje lub ukrywa część zawartości.
- [Zagnieżdżone strony układu ze składnią Razor](http://www.mikesdotnetting.com/Article/164/Nested-Layout-Pages-with-Razor) — blogi Mike Brind przykładowy sposób zagnieździć strony układu. (Obejmuje pobierania stron).

> [!div class="step-by-step"]
> [Poprzednie](deleting-data.md)
> [dalej](publishing.md)
