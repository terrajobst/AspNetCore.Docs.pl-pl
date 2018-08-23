---
uid: mvc/overview/older-versions/working-with-the-dropdownlist-box-and-jquery/using-the-dropdownlist-helper-with-aspnet-mvc
title: Używanie Pomocnika DropDownList na platformie ASP.NET MVC | Dokumentacja firmy Microsoft
author: Rick-Anderson
description: ''
ms.author: riande
ms.date: 01/12/2012
ms.assetid: 53767e05-c8ab-42e1-a94b-22d906195200
msc.legacyurl: /mvc/overview/older-versions/working-with-the-dropdownlist-box-and-jquery/using-the-dropdownlist-helper-with-aspnet-mvc
msc.type: authoredcontent
ms.openlocfilehash: 600a8ad409d55f0b01eedc8168eaec2b39c88a3b
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/16/2018
ms.locfileid: "41753407"
---
<a name="using-the-dropdownlist-helper-with-aspnet-mvc"></a>Używanie Pomocnika DropDownList na platformie ASP.NET MVC
====================
Przez [Rick Anderson](https://github.com/Rick-Anderson)

Ta seria samouczków obejmuje podstawy pracy z [DropDownList](https://msdn.microsoft.com/library/dd492948.aspx) pomocnika oraz [ListBox](https://msdn.microsoft.com/library/system.web.mvc.html.selectextensions.listbox.aspx) pomocy w aplikacji sieci Web platformy ASP.NET MVC. Microsoft Visual Web Developer 2010 Express Service Pack 1, która jest bezpłatna wersja programu Microsoft Visual Studio, aby wykonać kroki samouczka można użyć. Przed rozpoczęciem upewnij się, że po zainstalowaniu wymagań wstępnych wymienionych poniżej. Możesz zainstalować wszystkie z nich, klikając poniższe łącze: [Instalatora platformy sieci Web](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack). Alternatywnie można indywidualnie zainstalować wymagania wstępne, korzystając z następujących linków:

- [Visual Studio Web Developer Express z dodatkiem SP1 wymagań wstępnych.](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack) <a id="post"></a>
- [Program ASP.NET MVC 3 Tools Update](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=MVC3)
- [SQL Server Compact 4.0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCE;SQLCEVSTools_4_0)(Obsługa środowiska uruchomieniowego i narzędzi)

Jeśli używasz programu Visual Studio 2010 zamiast Visual Web Developer 2010, zainstaluj wymagania wstępne, klikając poniższe łącze: [wymagania wstępne programu Visual Studio 2010](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack). W tym samouczku założono, że zostały wykonane [wprowadzenie do platformy ASP.NET MVC](../getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) samouczek lub[platformy ASP.NET MVC Music Store](../mvc-music-store/mvc-music-store-part-1.md) samouczek lub znasz rozwoju platformy ASP.NET MVC. Ten samouczek rozpoczyna się od modyfikacji projektu z [platformy ASP.NET MVC Music Store](../mvc-music-store/mvc-music-store-part-1.md) samouczka. Możesz pobrać projekt startowy z następującego linku [pobrać wersję języka C#](https://archive.msdn.microsoft.com/Project/Download/FileDownload.aspx?ProjectName=aspnetmvcsamples&amp;DownloadId=15829).

Projekt Visual Web Developer z samouczkiem dotyczącym ukończone kod źródłowy języka C# jest dostępny powiązany z tym tematem. [Pobierz](https://code.msdn.microsoft.com/Using-the-DropDownList-67f9367d).

### <a name="what-youll-build"></a>Jakie będziesz tworzyć

Utworzysz metody akcji i widoki, które używają [DropDownList](https://msdn.microsoft.com/library/system.web.mvc.html.selectextensions.dropdownlist.aspx) pomocnika o wybranie kategorii. Ponadto **jQuery** dodać okno dialogowe kategorii insert, które mogą być używane podczas nowej kategorii (na przykład gatunku lub wykonawcy) jest wymagana. Poniżej przedstawiono zrzut ekranu widoku Create wyświetlane łącza Dodaj nowe gatunek i dodać nowe wykonawcy.

![](using-the-dropdownlist-helper-with-aspnet-mvc/_static/image1.png)

### <a name="skills-youll-learn"></a>Umiejętności, których dowiesz się

Oto, dowiesz się:

- Jak używać [DropDownList](https://msdn.microsoft.com/library/system.web.mvc.html.selectextensions.dropdownlist.aspx) pomocnika, aby wybrać kategorię danych.
- Jak dodać **jQuery** okno dialogowe, aby dodać nowe kategorie.

### <a name="getting-started"></a>Wprowadzenie

Zacznij od pobrania projekt startowy z następującego linku [Pobierz](https://archive.msdn.microsoft.com/Project/Download/FileDownload.aspx?ProjectName=aspnetmvcsamples&amp;DownloadId=15829). W Eksploratorze Windows, kliknij prawym przyciskiem myszy *DDL\_Starter.zip* plik i wybierz polecenie Właściwości. W **DDL\_właściwości Starter.zip** okno dialogowe, wybierz opcję odblokowania.

![](using-the-dropdownlist-helper-with-aspnet-mvc/_static/image2.png)

Kliknij prawym przyciskiem myszy DDL\_Starter.zip plik i wybierz pozycję **Wyodrębnij wszystkie** aby rozpakować plik. Otwórz *StartMusicStore.sln* plików za pomocą programu Visual Web Developer 2010 Express ("Visual Web Developer" lub "VWD" skrócie) lub Visual Studio 2010.

Naciśnij klawisze CTRL + F5, aby uruchomić aplikację, a następnie kliknij przycisk **testu** łącza.

![](using-the-dropdownlist-helper-with-aspnet-mvc/_static/image3.png)

Wybierz **wybierz kategorię Movie (proste)** łącza. Wybierz typ film zostanie wyświetlona lista, z Komedia wybranej wartości.

![](using-the-dropdownlist-helper-with-aspnet-mvc/_static/image4.png)

Kliknij prawym przyciskiem myszy w przeglądarce i wybierz Wyświetl źródło. Zostanie wyświetlony kod HTML dla strony. Poniższy kod HTML elementu select.

[!code-html[Main](using-the-dropdownlist-helper-with-aspnet-mvc/samples/sample1.html)]

Widać, że każdy element na liście wyboru ma wartość (0 dla akcji, dramat 1, 2-dokument i 3-Science-Fiction) i nazwę wyświetlaną (działania, dramat, Komedia oraz Science-Fiction). Powyższy kod jest standardowy kod HTML dla listy wyboru.

Zmień listy wyboru dramat i naciśnij klawisz **przesyłania** przycisku. Adres URL w przeglądarce jest `http://localhost:2468/Home/CategoryChosen?MovieType=1` i zostanie wyświetlona strona **wybrano: 1**.

![](using-the-dropdownlist-helper-with-aspnet-mvc/_static/image5.png)

Otwórz *Controllers\HomeController.cs* plików i zbadaj `SelectCategory` metody.

[!code-csharp[Main](using-the-dropdownlist-helper-with-aspnet-mvc/samples/sample2.cs)]

[DropDownList](https://msdn.microsoft.com/library/dd492738.aspx) wymaga pomocnika używany do tworzenia listy wyboru HTML **IEnumerable&lt;SelectListItem &gt;** , jawnie lub niejawnie. Oznacza to, że przekazujesz **IEnumerable&lt;SelectListItem &gt;**  jawnie na [DropDownList](https://msdn.microsoft.com/library/dd492738.aspx) pomocnika lub można dodać **IEnumerable&lt; SelectListItem &gt;**  do [obiekt ViewBag](https://blogs.msdn.com/b/rickandy/archive/2011/01/28/dynamic-v-strongly-typed-views.aspx) przy użyciu takiej samej nazwie **SelectListItem** jako właściwości modelu. Przekazując **SelectListItem** niejawne i jawne są omówione w następnej części samouczka. Powyższy kod pokazuje najprostszy sposób możliwych do utworzenia **IEnumerable&lt;SelectListItem &gt;**  i wypełnić ją za pomocą tekstu i wartości. Uwaga `Comedy` [SelectListItem](https://msdn.microsoft.com/library/system.web.mvc.selectlistitem.aspx) ma [wybrane](https://msdn.microsoft.com/library/system.web.mvc.selectlistitem.selected.aspx) właściwością **true;** spowoduje renderowanych listy wyboru pokazać **Komedia** wybranego elementu na liście.

**IEnumerable&lt;SelectListItem &gt;**  utworzone powyżej jest dodawany do [obiekt ViewBag](https://blogs.msdn.com/b/rickandy/archive/2011/01/28/dynamic-v-strongly-typed-views.aspx) o nazwie MovieType. Jest to, jak możemy przekazać **IEnumerable&lt;SelectListItem &gt;**  niejawnie do [DropDownList](https://msdn.microsoft.com/library/dd492738.aspx) pomocnika, pokazano poniżej.

Otwórz *Views\Home\SelectCategory.cshtml* plików i zbadaj znaczników.

[!code-cshtml[Main](using-the-dropdownlist-helper-with-aspnet-mvc/samples/sample3.cshtml)]

W trzecim wierszu ustawimy układu do widoków/Shared/\_proste\_Layout.cshtml, który jest uproszczoną wersją pliku standardowego układu. Możemy to zrobić, aby zapewnić wyświetlanie i renderowania kodu HTML proste.

W tym przykładzie firma Microsoft nie zmienia się stan aplikacji, dzięki czemu firma Microsoft będzie przesłać dane za pomocą **HTTP GET**, a nie **żądania HTTP POST**. Zobacz sekcję W3C [Szybka lista kontrolna dotycząca wybierając HTTP GET lub POST](http://www.w3.org/2001/tag/doc/whenToUseGet.html#checklist). Ponieważ nie Zmieniamy aplikacji i publikowanie formularza, używamy [Html.BeginForm](https://msdn.microsoft.com/library/dd460344.aspx) przeciążenia, które pozwala na określenie metody akcji, kontrolera i formularz, metoda (**żądania HTTP POST** lub **HTTP GET**). Zwykle zawierają widoki [Html.BeginForm](https://msdn.microsoft.com/library/dd505244.aspx) przeciążenia, które nie przyjmuje żadnych parametrów. Brak parametru wersji domyślnie wysyłania danych formularza WPIS wersję tej samej metody akcji i kontrolera.

Następujący wiersz

[!code-cshtml[Main](using-the-dropdownlist-helper-with-aspnet-mvc/samples/sample4.cshtml)]

przekazuje argument ciągu **DropDownList** pomocnika. Ten ciąg "MovieType", w tym przykładzie wykonuje dwie czynności:

- Zawiera klucz **DropDownList** pomocnika, aby znaleźć **IEnumerable&lt;SelectListItem &gt;**  w **obiekt ViewBag**.
- Jego jest powiązany z danymi MovieType element formularza. Jeśli metoda przesyłania jest **HTTP GET**, `MovieType` będzie ciąg zapytania. Jeśli metoda przesyłania jest **żądania HTTP POST**, `MovieType` zostanie dodany do treści wiadomości. Na poniższej ilustracji przedstawiono ciągu zapytania o wartości 1.

![](using-the-dropdownlist-helper-with-aspnet-mvc/_static/image6.png)

Poniższy kod przedstawia `CategoryChosen` formularz został przesłany do metody.

[!code-csharp[Main](using-the-dropdownlist-helper-with-aspnet-mvc/samples/sample5.cs)]

Przejdź z powrotem do strony testowej, a następnie wybierz pozycję **HTML SelectList** łącza. Strony HTML renderuje podobne do prostych strona testowa platformy ASP.NET MVC wybierz element. Kliknij prawym przyciskiem myszy okno przeglądarki, a następnie wybierz pozycję **Wyświetl źródło**. Kod znaczników HTML dla listy wyboru jest zasadniczo identyczne. Strona testowa HTML, działa jak metody akcji ASP.NET MVC i widok, który przetestowaliśmy wcześniej.

### <a name="improving-the-movie-select-list-with-enums"></a>Poprawianie listy wybierz filmów wyliczeniowych

Jeśli kategorie w aplikacji są rozwiązywane i nie ulegnie zmianie, możesz skorzystać z wyliczenia, aby sprawić, że kod bardziej niezawodne i łatwiejsze do rozszerzenia. Po dodaniu nowej kategorii, jest generowany wartość prawidłową kategorię. Pozwala uniknąć błędów kopiowania i wklejania, gdy Dodaj nową kategorię, ale zapomnij zaktualizować wartości kategorii.

Otwórz *Controllers\HomeController.cs* pliku i sprawdź następujący kod:

[!code-csharp[Main](using-the-dropdownlist-helper-with-aspnet-mvc/samples/sample6.cs)]

[Wyliczenia](https://msdn.microsoft.com/library/sbbt4032(VS.80).aspx) `eMovieCategories` przechwytuje typy cztery filmu. `SetViewBagMovieType` Metoda tworzy **IEnumerable&lt;SelectListItem &gt;**  z `eMovieCategories` **wyliczenia**i ustawia `Selected` właściwość `selectedMovie` parametru. `SelectCategoryEnum` Metody akcji używa tego samego widoku jako `SelectCategory` metody akcji.

Przejdź do strony testowej, a następnie kliknij przycisk na `Select Movie Category (Enum)` łącza. Tym razem, zamiast wartości (liczba) są wyświetlane, ciąg reprezentujący wyliczenia jest wyświetlany.

### <a name="posting-enum-values"></a>Publikowanie wartości wyliczenia

Formularze HTML zwykle są używane do publikowania danych do serwera. Poniższy kod przedstawia `HTTP GET` i `HTTP POST` wersje `SelectCategoryEnumPost` metody.

[!code-csharp[Main](using-the-dropdownlist-helper-with-aspnet-mvc/samples/sample7.cs)]

Przekazując `eMovieCategories` enum `POST` metody, firma Microsoft może wyodrębnić wartość wyliczenia i ciąg typu wyliczeniowego. Uruchamianie aplikacji przykładowej i przejdź do strony testowej. Kliknij pozycję `Select Movie Category(Enum Post)` łącza. Wybierz typ filmu, a następnie kliknij przycisk Prześlij. Wyświetlacz pokazuje zarówno wartość, jak i nazwę typu filmu.

![](using-the-dropdownlist-helper-with-aspnet-mvc/_static/image7.png)

### <a name="creating-a-multiple-section-select-element"></a>Tworzenie wielu elementu wybierz sekcję

[ListBox](https://msdn.microsoft.com/library/system.web.mvc.html.selectextensions.listbox.aspx) pomocnika kodu HTML renderowany kod HTML `<select>` element z `multiple` atrybut, który pozwala użytkownikom na dokonanie wielokrotny wybór. Przejdź do testu łącza, a następnie wybierz **Multi Wybieranie kraju** łącza. Wyrenderowany interfejsu użytkownika można wybrać wielu krajach. Na poniższej ilustracji wybrano Kanada i Chiny.

![](using-the-dropdownlist-helper-with-aspnet-mvc/_static/image8.png)

### <a name="examining-the-multiselectcountry-code"></a>Badanie kodu MultiSelectCountry

Sprawdź następujący kod z *Controllers\HomeController.cs* pliku.

[!code-csharp[Main](using-the-dropdownlist-helper-with-aspnet-mvc/samples/sample8.cs)]

`GetCountries` Metoda tworzy listę krajów, a następnie przekazuje go do `MultiSelectList` konstruktora. `MultiSelectList` Przeciążenia konstruktora, używane w `GetCountries` powyższych metod przyjmuje cztery parametry:

[!code-csharp[Main](using-the-dropdownlist-helper-with-aspnet-mvc/samples/sample9.cs)]

1. *elementy*: [IEnumerable](https://msdn.microsoft.com/library/system.collections.ienumerable.aspx) zawierający elementy na liście. W przykładzie powyżej listy krajów.
2. *dataValueField*: Nazwa właściwości w **IEnumerable** listy, który zawiera wartość. W powyższym przykładzie `ID` właściwości.
3. *dataTextField*: Nazwa właściwości w **IEnumerable** listę zawierającą informacje do wyświetlenia. W powyższym przykładzie `name` właściwości.
4. *selectedValues*: Lista wybranych wartości.

W powyższym przykładzie `MultiSelectCountry` metoda przekazuje `null` wartość dla wybranych krajach, dzięki czemu nie kraje są zaznaczone, gdy jest wyświetlana w Interfejsie użytkownika. Poniższy kod przedstawia używany do renderowania kodu znaczników Razor `MultiSelectCountry` widoku.

[!code-cshtml[Main](using-the-dropdownlist-helper-with-aspnet-mvc/samples/sample10.cshtml)]

Pomocnik kodu HTML [ListBox](https://msdn.microsoft.com/library/dd470200.aspx) metoda powyżej wykonaj dwa parametry, nazwa właściwości do wiązania modelu i [MultiSelectList](https://msdn.microsoft.com/library/system.web.mvc.multiselectlist.aspx) zawierający wybierz opcje i wartości. `ViewBag.YouSelected` Powyższy kod służy do wyświetlania wartości krajów wybrania podczas przesyłania formularza. Sprawdź przeciążenia metody POST protokołu HTTP `MultiSelectCountry` metody.

[!code-csharp[Main](using-the-dropdownlist-helper-with-aspnet-mvc/samples/sample11.cs)]

`ViewBag.YouSelected` Właściwość dynamiczna zawiera wybranych krajach, uzyskać `Countries` wpisu w kolekcji formularza. W tej wersji metoda GetCountries przechodzi przez listę wybranych krajach, więc kiedy `MultiSelectCountry` wyświetlany jest widok, w wybranych krajach są zaznaczone w interfejsie użytkownika.

### <a name="making-a-select-element-friendly-with-the-harvest-chosen-jquery-plugin"></a>Tworzenie wybierz Element Friendly przy użyciu jQuery wybrane Harvest wtyczki

Zbiór [wybrane](http://harvesthq.github.com/chosen/) jQuery wtyczki mogą być dodawane do kodu HTML &lt;wybierz&gt; element, aby utworzyć użytkownika przyjazna interfejsu użytkownika. Poniższe obrazy pokazują zbiorów [wybrane](http://harvesthq.github.com/chosen/) jQuery wtyczkę przy użyciu `MultiSelectCountry` widoku.

![](using-the-dropdownlist-helper-with-aspnet-mvc/_static/image9.png)

W poniższych dwóch obrazów **Kanada** jest zaznaczone.

![](using-the-dropdownlist-helper-with-aspnet-mvc/_static/image10.png)

![](using-the-dropdownlist-helper-with-aspnet-mvc/_static/image11.png)

Na powyższej ilustracji wybrano Kanada i zawiera **x** można kliknąć, aby usunąć zaznaczenie. Na poniższym obrazie przedstawiono Kanadzie, Chiny, i Japonia wybrane.

![](using-the-dropdownlist-helper-with-aspnet-mvc/_static/image12.png)

### <a name="hooking-up-the-harvest-chosen-jquery-plugin"></a>Podłączanie jQuery wybrane Harvest wtyczki

Poniższa sekcja to łatwiejszej do obserwowania, jeśli użytkownik ma pewne doświadczenie z jQuery. Jeśli po raz pierwszy używasz jQuery przed, możesz chcieć Wypróbuj jedną z następujących samouczków jQuery.

- [Jak działa jQuery](http://docs.jquery.com/Tutorials:How_jQuery_Works) przez [Resig Jan](http://ejohn.org/)
- [Wprowadzenie do technologii jQuery](http://docs.jquery.com/Tutorials:Getting_Started_with_jQuery) przez [Jörn Zaefferer](http://bassistance.de/)
- [Na żywo przykłady jQuery](http://codylindley.com/blogstuff/js/jquery/#) przez [Cody Lindley](http://codylindley.com/)

Wtyczka wybrane znajduje się w starter i ukończone przykładowych projektów, dołączone do tego samouczka. Na potrzeby tego samouczka wystarczy podłączyć ją do interfejsu użytkownika przy użyciu jQuery. Aby użyć wtyczki jQuery Harvest wybrane w projekcie ASP.NET MVC, musisz mieć:

1. Pobierz dodatek wybrane [github](https://github.com/harvesthq/chosen/). W tym kroku zostały wykonane dla Ciebie.
2. Dodaj folder wybrany do projektu programu ASP.NET MVC. Dodaj zasoby z wtyczki wybrane pobrany w poprzednim kroku do wybranego folderu. W tym kroku zostały wykonane dla Ciebie.
3. Podpinanie wybrany dodatek plug-in, aby **DropDownList** lub **ListBox** pomocnika kodu HTML.

### <a name="hooking-up-the-chosen-plugin-to-the-multiselectcountry-view"></a>Podłączanie wtyczki wybrane do widoku MultiSelectCountry.

Otwórz *Views\Home\MultiSelectCountry.cshtml* pliku i Dodaj `htmlAttributes` parametr `Html.ListBox`. Parametr spowoduje dodanie zawiera nazwę klasy do listy wyboru (`@class = "chzn-select"`). Kompletny kod jest pokazany poniżej:

[!code-cshtml[Main](using-the-dropdownlist-helper-with-aspnet-mvc/samples/sample12.cshtml)]

W powyższym kodzie dodajemy atrybutu w kodzie HTML i wartością atrybutu `class = "chzn-select"`. \@ Znak poprzedniego klasa ma nic wspólnego z aparatu widoku Razor. `class` jest [— słowo kluczowe języka C#](https://msdn.microsoft.com/library/x53a06bb.aspx). Słowa kluczowe języka C# nie można używać jako identyfikatorów, o ile nie obejmują one \@ jako prefiksu. W powyższym przykładzie `@class` jest prawidłowym identyfikatorem, ale **klasy** jest niezgodny, ponieważ **klasy** jest słowem kluczowym.

Dodaj odwołania do *Chosen/chosen.jquery.js* i *Chosen/chosen.css* plików. *Chosen/chosen.jquery.js* i implementuje funkcjonalnie z wtyczki wybrane. *Chosen/chosen.css* plik zawiera stylu. Dodaj te odwołania do dołu *Views\Home\MultiSelectCountry.cshtml* pliku. Poniższy kod pokazuje, jak odwoływać się do wtyczki wybrane.

[!code-cshtml[Main](using-the-dropdownlist-helper-with-aspnet-mvc/samples/sample13.cshtml)]

Aktywowanie wtyczki wybrane przy użyciu nazwy klasy używane w **Html.ListBox** kodu. W powyższym przykładzie nazwa klasy jest `chzn-select`. Dodaj następujący wiersz do dołu *Views\Home\MultiSelectCountry.cshtml* plik widoku. Ten wiersz aktywuje dodatek wybrane.

[!code-html[Main](using-the-dropdownlist-helper-with-aspnet-mvc/samples/sample14.html)]

Następujący wiersz jest składnia do wywoływania funkcji gotowości jQuery, który wybiera element DOM, z nazwą klasy `chzn-select`.

[!code-powershell[Main](using-the-dropdownlist-helper-with-aspnet-mvc/samples/sample15.ps1)]

Opakowana wartość zwrócony z powyższego wywołania to dotyczy wybranej metody (`.chosen();`), która przechwytuje się wtyczka wybrane.

Poniższy kod przedstawia ukończoną *Views\Home\MultiSelectCountry.cshtml* plik widoku.

[!code-cshtml[Main](using-the-dropdownlist-helper-with-aspnet-mvc/samples/sample16.cshtml)]

Uruchom aplikację, a następnie przejdź do `MultiSelectCountry` widoku. Spróbuj dodać i usuwanie krajach. Zawiera także udostępniona do pobrania próbki `MultiCountryVM` metody i widok, który implementuje funkcje MultiSelectCountry przy użyciu widoku modelu zamiast **obiekt ViewBag**.

W następnej sekcji zobaczysz, jak działa mechanizm tworzenia szkieletu ASP.NET MVC z **DropDownList** pomocnika.

> [!div class="step-by-step"]
> [Next](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper.md)
