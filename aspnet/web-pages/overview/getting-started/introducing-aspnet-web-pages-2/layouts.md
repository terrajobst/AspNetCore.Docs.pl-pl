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
ms.locfileid: "30899817"
---
<a name="introducing-aspnet-web-pages---creating-a-consistent-layout"></a>Wprowadzenie do stron sieci Web ASP.NET - tworzenie spójnego układu
====================
przez [FitzMacken niestandardowy](https://github.com/tfitzmac)

> Ten samouczek przedstawia sposób użycia *układów* utworzyć spójny wygląd stron w lokacji, która używa strony sieci Web ASP.NET. Przyjęto założenie, że zostały wykonane serii za pomocą [usuwanie danych z baz danych w programie ASP.NET Web Pages](https://go.microsoft.com/fwlink/?LinkId=251584).
> 
> Zawartość:
> 
> - Strona układu jest.
> - Jak połączyć układ strony z zawartością dynamiczną.
> - Jak przekazać wartości do strony układu.


## <a name="about-layouts"></a>Układy — informacje

Strony po utworzeniu wykonanej do tej pory wszystkie zostały zakończone, stron autonomicznych. Wszystkie one należą do tej samej lokacji, ale nie ma wszystkie wspólne elementy lub wygląd normalny.

W większości witryn ma spójny wygląd i układu. Na przykład, jeśli przejdziesz do [Microsoft.com/web](https://www.microsoft.com/web/) lokacji i szukać, zobaczysz, że wszystkie strony przestrzegają ogólny układ i motyw programu visual:

![Strona witryny Microsoft.com/Web przedstawiający układ nagłówka, obszar nawigacji, obszar zawartości i stopki](layouts/_static/image1.png)

*Nieefektywne* sposobem tworzenia tego układu byłoby zdefiniować nagłówek, na pasku nawigacyjnym i stopka osobno dla poszczególnych stron sieci. Użytkownik będzie można duplikowania znacznika tej samej zawsze. Jeśli chcesz zmienić (na przykład aktualizacja stopki), należy zmienić oddzielnie każdej strony.

To miejsce *strony układu* się. W składniku ASP.NET Web Pages można zdefiniować stronę układu, która zapewnia ogólną kontener dla stron w witrynie. Na przykład strony układu może zawierać nagłówek, obszar nawigacji i stopce strony. Strona układu zawiera symbol zastępczy, gdzie należy wstawić zawartość głównego.

Można zdefiniować poszczególnych strony zawartości, które zawierają kod znaczników i kodu tylko na tej stronie. Strony z zawartością nie muszą być ukończone stron HTML. nawet nie mają mieć `<body>` elementu. Mają one również wiersza kodu, który informuje, jakie układ strony, aby wyświetlić zawartość w ASP.NET. Oto obraz, który zawiera około, jak działa ta relacja:

![Diagram koncepcyjny przedstawiający dwie strony z zawartością i stronę układu, w którym mieściły się](layouts/_static/image2.png)

Ta interakcja jest łatwy do zrozumienia, gdy zostanie wyświetlony w akcji. W tym samouczku zostanie zmieniona stronach filmy zastosować układ.

## <a name="adding-a-layout-page"></a>Dodawanie strony układu

Będzie rozpoczyna się od utworzenia strony układu, który definiuje układ strony typowe nagłówek, stopka i obszaru głównego zawartości. W witrynie WebPagesMovies Dodaj stronę CSHTML o nazwie  *\_Layout.cshtml*.

Wiodące znak podkreślenia ( `_` ) znak jest znacząca. Jeśli na stronie nazwa rozpoczyna się od znaku podkreślenia, program ASP.NET nie będzie wysyłać bezpośrednio tej strony w przeglądarce. Tę Konwencję pozwala zdefiniować stron, które są wymagane dla witryny, ale aby użytkownicy nie powinien móc żądać bezpośrednio.

Zamień zawartość na stronie następujące czynności:

[!code-html[Main](layouts/samples/sample1.html)]

Jak widać, ten kod znaczników jest po prostu HTML, który używa `<div>` elementy, aby zdefiniować trzy sekcje w stronie plus jeden więcej `<div>` element, aby pomieścić trzy sekcje. Stopki zawiera bitowego kodu Razor: `@DateTime.Now.Year`, który będzie renderowany w bieżącym roku w tej lokalizacji na stronie.

Zwróć uwagę, że istnieje łącze do arkusza stylów o nazwie *Movies.css*. Arkusz stylów jest, gdzie można zdefiniować szczegóły fizycznego układu elementów. Utworzysz którego za chwilę.

Tylko nietypowe funkcji w tym  *\_Layout.cshtml* strona jest `@Render.Body()` wiersza. To symbol zastępczy, gdzie zawartość, gdy ten układ jest scalany z innej strony.

## <a name="adding-a-css-file"></a>Dodawanie pliku CSS

Preferowany sposób definiowania rzeczywisty układ (to znaczy wygląd) elementów na stronie jest Użyj reguł stylu kaskadowych w arkuszu (CSS). Dlatego należy utworzyć *.css* pliku reguł dla nowego układu.

W programie WebMatrix wybierz katalog główny witryny. Następnie w **pliki** kartę na wstążce kliknij strzałkę w obszarze **nowy** przycisk, a następnie kliknij przycisk **nowy Folder**.

![Opcja "Nowy Folder" obszarze Nowy na Wstążce.](layouts/_static/image3.png)

Nazwa nowego folderu *style*.

![Nazewnictwo nowy folder "Style"](layouts/_static/image4.png)

Wewnątrz nowe *style* folderu, Utwórz plik o nazwie *Movies.css*.

![Tworzenie nowego pliku Movies.css](layouts/_static/image5.png)

Zastąp zawartość nowego *.css* pliku następującym kodem:

[!code-css[Main](layouts/samples/sample2.css)]

Firma Microsoft nie będzie, że te informacje te reguły CSS, z wyjątkiem dwie czynności Pamiętaj, aby. Jeden jest poza ustawieniem czcionki oraz rozmiary, reguły wykorzystania bezwzględny ustanowienie lokalizacji nagłówek, stopka i głównym obszarem zawartości. Jeśli jesteś nowym użytkownikiem pozycjonowanie w kodzie CSS, możesz przeczytać [pozycjonowania CSS](http://www.w3schools.com/css/css_positioning.asp) samouczek w witrynie W3Schools.

Jeszcze inną czynność należy pamiętać, zdefiniowano że u dołu, możemy skopiowany reguły stylu, które pierwotnie indywidualnie w *Movies.cshtml* pliku. Zasady te były używane w [wprowadzenie do wyświetlania danych przy użyciu stron ASP.NET Web Pages](https://go.microsoft.com/fwlink/?LinkId=251580) samouczkiem, aby wprowadzić `WebGrid` pomocnika renderowania kodu znaczników, który rozkłada został dodany do tabeli. (Jeśli ma być używana *.css* pliku definicji stylu reguły stylu dla całej witryny może również umieścić w tym polu.)

## <a name="updating-the-movies-file-to-use-the-layout"></a>Aktualizowanie pliku filmów do użycia w układzie

Należy teraz zaktualizować istniejące pliki w tej witryny używanie nowego układu. Otwórz *Movies.cshtml* pliku. U góry w pierwszym wierszu kodu, Dodaj następujące informacje:

[!code-csharp[Main](layouts/samples/sample3.cs)]

Strona teraz rozpoczyna się w ten sposób:

[!code-cshtml[Main](layouts/samples/sample4.cshtml?highlight=2)]

Jeden wiersz kodu informuje ASP.NET że w przypadku *filmy* uruchomieniu strony, powinny zostać scalone z  *\_Layout.cshtml* pliku.

Ponieważ *Movies.cshtml* użyte strony układu, możesz usunąć znacznika z *Movies.cshtml* strona, która ma poświęcony na obsługę przez  *\_Layout.cshtml*pliku. Wyjmij `<!DOCTYPE>`, `<html>`, i `<body>` otwierające i zamykające znaczniki. Wyjmij całą `<head>` elementu i jego zawartość, która zawiera reguły stylów siatki, ponieważ masz teraz te reguły *.css* pliku. Gdy jesteś jego Zmień istniejącą `<h1>` elementu `<h2>` element; istnieje `<h1>` elementu na stronie układu już. Zmień `<h2>` tekst "Listy filmów".

Zwykle nie trzeba wprowadzać tego rodzaju zmiany na stronie zawartości. Po uruchomieniu witryny ze stroną układu tworzenia stron zawartości bez tych elementów rozpoczynać się od znaku. W takim przypadku jednak konwertowania strony autonomicznych na taki, który używa układ, dlatego nieco oczyszczania.

Po zakończeniu, *Movies.cshtml* strona będzie wyglądała podobnie do następującej:

[!code-cshtml[Main](layouts/samples/sample5.cshtml)]

### <a name="testing-the-layout"></a>Testowanie układu

Teraz widać, jak wygląda układu. W programie WebMatrix, kliknij prawym przyciskiem myszy *Movies.cshtml* i wybrać opcję **Uruchom w przeglądarce**. Gdy w przeglądarce zostanie wyświetlona strona, wygląda tej strony:

![Renderowany przy użyciu układ strony filmy](layouts/_static/image6.png)

ASP.NET został scalony zawartości strony Movies.cshtml do  *\_Layout.cshtml* miejsca z prawej strony `RenderBody` jest metoda. I oczywiście  *\_Layout.cshtml* odwołuje się do strony *.css* pliku, która definiuje wygląd strony.

## <a name="updating-the-addmovie-page-to-use-the-layout"></a>Aktualizowanie strony AddMovie do użycia w układzie

Rzeczywistych zaletą układów jest, której można je dla wszystkich stron w witrynie. Otwórz *AddMovie.cshtml* strony.

Może być Pamiętaj, że *AddMovie.cshtml* strony pierwotnie była niektóre reguły CSS w nim zdefiniować wygląd komunikatów o błędach weryfikacji. Ponieważ masz *.css* pliku witryny teraz, można przenieść te reguły do *.css* pliku. Usuń je z *AddMovie.cshtml* plik i dodać je do dołu *Movies.css* pliku. Przenosisz następujące reguły:

[!code-css[Main](layouts/samples/sample6.css)]

Teraz należy tego samego rodzaju zmiany w *AddMovie.cshtml* jak w *Movies.cshtml* — Dodaj `Layout="~/_Layout.cshtml;` i Usuń kod znaczników HTML, która jest teraz nadmiarowe. Zmień `<h1>` elementu `<h2>`. Gdy wszystko będzie gotowe, strona będzie wyglądała w tym przykładzie:

[!code-cshtml[Main](layouts/samples/sample7.cshtml)]

Uruchom strony. Teraz wygląda na tej ilustracji:

![Strona "Dodaj filmy" renderowany przy użyciu układu](layouts/_static/image7.png)

Chcesz wprowadzić zmiany podobne do stron w witrynie — *EditMovie.cshtml* i *DeleteMovie.cshtml*. Jednak zanim to zrobisz, umożliwia inna zmiana układu, który ułatwia nieco bardziej elastyczne.

## <a name="passing-title-information-to-the-layout-page"></a>Przekazywanie informacji o tytule do strony układu

 *\_Layout.cshtml* strona utworzony ma `<title>` element, który ma ustawioną wartość "Moja witryna filmu". W większości przeglądarek wyświetlenie zawartości tego elementu jako tekst, na karcie:

![Strony &lt;tytuł&gt; elementu wyświetlane na karcie przeglądarki](layouts/_static/image8.png)

Te informacje tytuł jest rodzajowa. Załóżmy, że chcesz tekstu tytułu dokładniej do bieżącej strony. (Tekst tytułu jest używany również przez aparaty wyszukiwania można określić strony o.) Można przekazać informacje ze strony zawartości, takich jak *Movies.cshtml* lub *AddMovie.cshtml* do strony układu, a następnie użyć tych informacji w celu dostosowania strony układu renderuje.

Otwórz *Movies.cshtml* strony ponownie. W kodzie u góry Dodaj następujący wiersz:

[!code-csharp[Main](layouts/samples/sample8.cs)]

`Page` Obiekt jest dostępny na wszystkich *.cshtml* stron i jest do tego celu, to znaczy do udostępniania informacji między stroną i jego układu.

Otwórz<em>\_Layout.cshtml</em> strony. Zmień `<title>` elementu, tak że wygląda ten kod znaczników:

[!code-html[Main](layouts/samples/sample9.html)]

Ten kod renderuje niezależnie od znajduje się w `Page.Title` właściwości w tej lokalizacji na stronie.

Uruchom *Movies.cshtml* strony. Pokazuje kartę przeglądarki teraz przekazany jako wartość `Page.Title`:

![Na karcie przeglądarki pokazywany tytuł utworzony dynamicznie](layouts/_static/image9.png)

Jeśli chcesz, Wyświetl źródło strony w przeglądarce. Można stwierdzić, że `<title>` element jest renderowany jako `<title>List Movies</title>`.

> [!TIP] 
> 
> **Obiekt strony**
> 
> Przydatną cechą `Page` to obiekt dynamiczny — `Title` właściwości nie jest nazwą fixed lub zastrzeżone. Można użyć *żadnych* nazwę wartości `Page` obiektu. Na przykład można łatwo przekazanego tytuł przy użyciu właściwości o nazwie `Page.CurrentName` lub `Page.MyPage`. Tylko ograniczenie jest nazwa do normalnej reguły dla właściwości, które mogą mieć nazwę. (Na przykład nazwa nie może zawierać spacji).
> 
> Można przekazać dowolną liczbę wartości przy użyciu `Page` obiektu. Jeśli chcesz przekazać informacje do strony układu, można przekazać wartości przy użyciu przypominać `Page.MovieTitle` i `Page.Genre` i `Page.MovieYear`. (Lub inne nazwy, które wynaleźli do przechowywania informacji.) Jedynym wymaganiem — która jest prawdopodobnie widocznych — jest trzeba użyć tych samych nazw na stronie zawartości i strony układu.
> 
> Informacje przekazywane za pomocą `Page` obiekt nie jest ograniczona do tylko tekst do wyświetlania na stronie układu. Można przekazać wartość na stronę układu, a następnie kodu na stronie układu można użyć wartości zdecydować, czy mają być wyświetlane części strony, co *.css* plików do użycia i tak dalej. Wartości przekazywane w `Page` obiektu są takie jak wszelkie inne wartości użycia w kodzie. Jest po prostu czy wartości pochodzą z strony zawartości i są przekazywane do strony układu.


Otwórz *AddMovie.cshtml* strony i dodać wiersz do góry kod, który zawiera tytuł *AddMovie.cshtml* strony:

[!code-csharp[Main](layouts/samples/sample10.cs)]

Uruchom *AddMovie.cshtml* strony. Zostanie wyświetlony nowy tytuł:

![Na karcie przeglądarki pokazywany tytuł "Dodaj filmy" utworzony dynamicznie](layouts/_static/image10.png)

## <a name="updating-the-remaining-pages-to-use-the-layout"></a>Uaktualnianie pozostałych stron do użycia w układzie

Teraz możesz ukończyć pozostałych stron w witrynie tak, aby używały nowy układ. Otwórz *EditMovie.cshtml* i *DeleteMovie.cshtml* z kolei i wprowadzenie identycznych zmian w każdym.

Dodaj wiersz kodu, który stanowi łącze do strony układu:

[!code-csharp[Main](layouts/samples/sample11.cs)]

Dodaj linię, aby ustawić tytuł strony:

[!code-csharp[Main](layouts/samples/sample12.cs)]

lub:

[!code-csharp[Main](layouts/samples/sample13.cs)]

Usuń wszystkie nadmiarowe kod znaczników HTML — zasadniczo Pozostaw tylko bity, które znajdują się wewnątrz `<body>` elementu (oraz u góry bloku kodu).

Zmień `<h1>` element ma być `<h2>` elementu.

Po wprowadzeniu tych zmian, każdy test i upewnij się, że są wyświetlane poprawnie i czy tytuł jest poprawna.

## <a name="parting-thoughts-about-layout-pages"></a>Towarzyszącą pomysły dotyczące strony układu

W tym samouczku został utworzony  *\_Layout.cshtml* strony i używać `RenderBody` sposób scalania zawartości z innej strony. To podstawowy wzorzec do używania układy stron sieci Web.

Układ strony mają dodatkowe funkcje, które firma Microsoft nie obejmują tutaj. Na przykład można zagnieżdżać strony układu — jedną stronę układu mogą z kolei odwoływać się do innego. Układy zagnieżdżonych mogą być przydatne, jeśli pracujesz z podpunkty lokacji wymagające układów. Można także używać dodatkowych metod (na przykład `RenderSection`) można skonfigurować konta o nazwie części strony układu.

Kombinacja strony układu i *.css* plików to zaawansowane. Jak można zauważyć w następnej serii samouczek, w programie WebMatrix można utworzyć witryny na podstawie *szablonu*, które zapewnia witryny, która ma wbudowane funkcje w nim. Szablony umożliwiają pełne wykorzystanie strony układu i CSS do tworzenia witryn, który wygląda świetnie i mają funkcje, takie jak menu. Poniżej przedstawiono zrzut ekranu strony głównej z lokacji, na podstawie szablonu, przedstawiający funkcji używających strony układu i CSS:

![Układ utworzony przez nagłówek, obszar nawigacji, obszaru zawartości, sekcja opcjonalna oraz łącza logowania szablonu witryny programu WebMatrix](layouts/_static/image11.png)

## <a name="complete-listing-for-movie-page-updated-to-use-a-layout-page"></a>Pełna lista strony Movie (zaktualizowane do strony układu)

[!code-cshtml[Main](layouts/samples/sample14.cshtml)]

## <a name="complete-page-listing-for-add-movie-page-updated-for-layout"></a>Strona ukończyć dla Dodaj stronę Movie (zaktualizowane dla układu)

[!code-cshtml[Main](layouts/samples/sample15.cshtml)]

## <a name="complete-page-listing-for-delete-movie-page-updated-for-layout"></a>Strona pełną listę strony filmu usuwania (zaktualizowane dla układu)

[!code-cshtml[Main](layouts/samples/sample16.cshtml)]

## <a name="complete-page-listing-for-edit-movie-page-updated-for-layout"></a>Strona pełną listę edycji strony Movie (zaktualizowane dla układu)

[!code-cshtml[Main](layouts/samples/sample17.cshtml)]

## <a name="coming-up-next"></a>Powtarzający się dalej

W następnym samouczku nauczysz się, jak opublikować witryny z Internetem, więc każdy może zobaczyć.

## <a name="additional-resources"></a>Dodatkowe zasoby

- [Tworzenie spójny wygląd](https://go.microsoft.com/fwlink/?LinkID=202891) — artykułu, który zawiera niektóre więcej szczegółów na temat pracy z układów. On również opis przekazać wartości do strony układu, który pokazuje lub ukrywa części zawartości.
- [Zagnieżdżone strony układu ze składnią Razor](http://www.mikesdotnetting.com/Article/164/Nested-Layout-Pages-with-Razor) — blogi Jan Brind przykładem zagnieździć strony układu. (Dotyczy również pobierania strony).

> [!div class="step-by-step"]
> [Poprzednie](deleting-data.md)
> [dalej](publishing.md)
