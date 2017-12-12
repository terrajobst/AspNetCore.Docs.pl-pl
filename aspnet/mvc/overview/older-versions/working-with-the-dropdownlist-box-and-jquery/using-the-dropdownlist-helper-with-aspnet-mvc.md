---
uid: mvc/overview/older-versions/working-with-the-dropdownlist-box-and-jquery/using-the-dropdownlist-helper-with-aspnet-mvc
title: "Używanie Pomocnika lista DropDownList na platformie ASP.NET MVC | Dokumentacja firmy Microsoft"
author: Rick-Anderson
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/12/2012
ms.topic: article
ms.assetid: 53767e05-c8ab-42e1-a94b-22d906195200
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/working-with-the-dropdownlist-box-and-jquery/using-the-dropdownlist-helper-with-aspnet-mvc
msc.type: authoredcontent
ms.openlocfilehash: b8393e1503cb562a46a00f49b51c0cb64ff2cfdc
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/10/2017
---
<a name="using-the-dropdownlist-helper-with-aspnet-mvc"></a>Używanie Pomocnika lista DropDownList na platformie ASP.NET MVC
====================
Przez [Rick Anderson](https://github.com/Rick-Anderson)

W tym samouczku udzieli Ci podstawy pracy z [DropDownList](https://msdn.microsoft.com/en-us/library/dd492948.aspx) pomocnika i [ListBox](https://msdn.microsoft.com/en-us/library/system.web.mvc.html.selectextensions.listbox.aspx) pomocy w aplikacji sieci Web programu ASP.NET MVC. Korzystając z Microsoft Visual Web Developer 2010 Express Service Pack 1, która jest bezpłatna wersja programu Microsoft Visual Studio, które należy wykonać w samouczku. Przed rozpoczęciem upewnij się, że po zainstalowaniu wymagania wstępne wymienione poniżej. Można zainstalować wszystkie z nich, klikając poniższe łącze: [Instalatora platformy sieci Web](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack). Alternatywnie można zainstalować oddzielnie wymagania wstępne, korzystając z następujących linków:

- [Visual Studio Web Developer Express z dodatkiem SP1 wymagania wstępne](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack)<a id="post"></a>
- [Aktualizacji narzędzi programu ASP.NET MVC 3](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=MVC3)
- [SQL Server Compact 4.0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCE;SQLCEVSTools_4_0)(środowisko uruchomieniowe + narzędzia pomocy technicznej)

Jeśli używasz programu Visual Studio 2010, zamiast Visual Web Developer 2010, zainstaluj wymagania wstępne, klikając poniższe łącze: [wymagania wstępne programu Visual Studio 2010](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack). Ten samouczek zakłada, zostały ukończone [wprowadzenie do platformy ASP.NET MVC](../getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) samouczek lub[ASP.NET MVC utworów muzycznych magazynu](../mvc-music-store/mvc-music-store-part-1.md) samouczek lub znasz rozwoju platformy ASP.NET MVC. W tym samouczku rozpoczyna się od zmodyfikowany projekt z [ASP.NET MVC utworów muzycznych magazynu](../mvc-music-store/mvc-music-store-part-1.md) samouczka. Możesz pobrać projekt starter, korzystając z następującego łącza [pobrać wersję języka C#](https://archive.msdn.microsoft.com/Project/Download/FileDownload.aspx?ProjectName=aspnetmvcsamples&amp;DownloadId=15829).

Projekt Visual Web Developer z samouczka ukończone kodu źródłowego C# jest dostępna powiązany z tym tematem. [Pobierz](https://code.msdn.microsoft.com/Using-the-DropDownList-67f9367d).

### <a name="what-youll-build"></a>Jakie będzie kompilacji

Utworzysz metody akcji i widoki, które używają [DropDownList](https://msdn.microsoft.com/en-us/library/system.web.mvc.html.selectextensions.dropdownlist.aspx) pomocnika wybierz kategorię. Ponadto **jQuery** można dodać okna dialogowego kategorii wstawiania, który może służyć, gdy potrzebny jest nową kategorię (takie jak genre lub wykonawcy). Poniżej przedstawiono zrzut ekranu przedstawiający łącza do dodawania nowych genre i dodać nowe wykonawcy widoku Utwórz.

![](using-the-dropdownlist-helper-with-aspnet-mvc/_static/image1.png)

### <a name="skills-youll-learn"></a>Dowiesz się umiejętności

Oto dowiesz się:

- Jak używać [DropDownList](https://msdn.microsoft.com/en-us/library/system.web.mvc.html.selectextensions.dropdownlist.aspx) pomocnika, aby wybrać kategorię danych.
- Jak dodać **jQuery** okna dialogowego, aby dodać nowe kategorie.

### <a name="getting-started"></a>Wprowadzenie

Uruchom pobierając projektu starter z następującego łącza [Pobierz](https://archive.msdn.microsoft.com/Project/Download/FileDownload.aspx?ProjectName=aspnetmvcsamples&amp;DownloadId=15829). W Eksploratorze Windows, kliknij prawym przyciskiem myszy *DDL\_Starter.zip* plik i wybierz polecenie Właściwości. W **DDL\_właściwości Starter.zip** okno dialogowe, wybierz opcję odblokowania.

![](using-the-dropdownlist-helper-with-aspnet-mvc/_static/image2.png)

Kliknij prawym przyciskiem myszy DDL\_Starter.zip plik i wybierz **Wyodrębnij wszystkie** aby rozpakować plik. Otwórz *StartMusicStore.sln* plików z programu Visual Web Developer 2010 Express ("Visual Web Developer" lub "VWD" skrócie) lub programu Visual Studio 2010.

Naciśnij klawisze CTRL + F5, aby uruchomić aplikację, a następnie kliknij przycisk **testu** łącza.

![](using-the-dropdownlist-helper-with-aspnet-mvc/_static/image3.png)

Wybierz **wybierz kategorię Movie (proste)** łącza. Wybierz typ filmu zostanie wyświetlona lista, z Komedia wybranej wartości.

![](using-the-dropdownlist-helper-with-aspnet-mvc/_static/image4.png)

Kliknij prawym przyciskiem myszy w przeglądarce i wybierz pozycję Wyświetl źródło. Zostanie wyświetlony kod HTML dla strony. Poniższy kod HTML dla elementu select.

[!code-html[Main](using-the-dropdownlist-helper-with-aspnet-mvc/samples/sample1.html)]

Widać, że każdy element na liście wyboru ma wartość (0 dla akcji, 1 dla teatralne, Komedia 2 i 3 dla fikcja nauki) i nazwę wyświetlaną (akcji, teatralne i Komedia fikcja nauki). Powyższy kod jest standardowe HTML na liście wyboru.

Zmień lista wyboru teatralne i naciśnij klawisz **przesyłania** przycisku. Adres URL w przeglądarce jest `http://localhost:2468/Home/CategoryChosen?MovieType=1` i zostaje wyświetlona strona **wybrano: 1**.

![](using-the-dropdownlist-helper-with-aspnet-mvc/_static/image5.png)

Otwórz *Controllers\HomeController.cs* pliku i sprawdź, czy `SelectCategory` metody.

[!code-csharp[Main](using-the-dropdownlist-helper-with-aspnet-mvc/samples/sample2.cs)]

[DropDownList](https://msdn.microsoft.com/en-us/library/dd492738.aspx) wymaga pomocnika używany do tworzenia listy wyboru HTML **IEnumerable&lt;SelectListItem &gt;** , jawnie lub niejawnie. Oznacza to, że przekazujesz **IEnumerable&lt;SelectListItem &gt;**  jawnie na [DropDownList](https://msdn.microsoft.com/en-us/library/dd492738.aspx) pomocnika można także dodać **IEnumerable&lt; SelectListItem &gt;**  do [obiekt ViewBag](https://blogs.msdn.com/b/rickandy/archive/2011/01/28/dynamic-v-strongly-typed-views.aspx) przy użyciu takiej samej nazwy **SelectListItem** jako wartość właściwości modelu. Przekazując **SelectListItem** niejawnie i jawnie została opisana w następnej części samouczka. Powyższy kod przedstawia najprostszy sposób możliwych do utworzenia **IEnumerable&lt;SelectListItem &gt;**  i wypełnić ją tekstu i wartości. Uwaga `Comedy` [SelectListItem](https://msdn.microsoft.com/en-us/library/system.web.mvc.selectlistitem.aspx) ma [wybrane](https://msdn.microsoft.com/en-us/library/system.web.mvc.selectlistitem.selected.aspx) ustawioną właściwość **true;** spowoduje renderowanych listy wybierz, aby wyświetlić **Komedia** jako wybrany element na liście.

**IEnumerable&lt;SelectListItem &gt;**  utworzone powyżej jest dodawany do [obiekt ViewBag](https://blogs.msdn.com/b/rickandy/archive/2011/01/28/dynamic-v-strongly-typed-views.aspx) o nazwie MovieType. Jest to, jak jest przekazywana **IEnumerable&lt;SelectListItem &gt;**  niejawnie do [DropDownList](https://msdn.microsoft.com/en-us/library/dd492738.aspx) pomocnika pokazano poniżej.

Otwórz *Views\Home\SelectCategory.cshtml* pliku i sprawdź, czy kod znaczników.

[!code-cshtml[Main](using-the-dropdownlist-helper-with-aspnet-mvc/samples/sample3.cshtml)]

Na trzeci wiersz, możemy ustawić układ widoków/Shared/\_proste\_Layout.cshtml, czyli uproszczonej wersji pliku układu standardowego. Firma Microsoft takie rozwiązanie pozwala uniknąć wyświetlania i renderowania kodu HTML proste.

W tym przykładzie firma Microsoft nie zmienia się stan aplikacji, dlatego firma Microsoft będzie przesyłania danych przy użyciu **HTTP GET**, a nie **HTTP POST**. Zobacz sekcję W3C [szybkie Lista kontrolna dotycząca Wybieranie HTTP GET lub POST](http://www.w3.org/2001/tag/doc/whenToUseGet.html#checklist). Ponieważ firma Microsoft nie są Zmienianie aplikacji i przesyłanie formularza, używamy [Html.BeginForm](https://msdn.microsoft.com/en-us/library/dd460344.aspx) przeciążenia, które pozwala określić metody akcji, kontrolera i formularz — metoda (**HTTP POST** lub **HTTP GET**). Zwykle zawierają widoki [Html.BeginForm](https://msdn.microsoft.com/en-us/library/dd505244.aspx) przeciążenia, które nie przyjmuje żadnych parametrów. Brak wersji parametru domyślnie wysyłania danych formularza do wersji POST w tej samej metody akcji i kontrolera.

Następujący wiersz

[!code-cshtml[Main](using-the-dropdownlist-helper-with-aspnet-mvc/samples/sample4.cshtml)]

argument ciągu przekazuje **DropDownList** pomocnika. Ten ciąg "MovieType" w tym przykładzie wykonuje dwie czynności:

- Udostępnia klucz dla **DropDownList** pomocnika można znaleźć **IEnumerable&lt;SelectListItem &gt;**  w **obiekt ViewBag**.
- Jego jest powiązany z danymi elementu form MovieType. Jeśli metoda przesyłania jest **HTTP GET**, `MovieType` będzie ciągu zapytania. Jeśli metoda przesyłania jest **HTTP POST**, `MovieType` zostaną dodane w treści wiadomości. Na poniższej ilustracji przedstawiono ciągu zapytania o wartości 1.

![](using-the-dropdownlist-helper-with-aspnet-mvc/_static/image6.png)

Poniższy kod przedstawia `CategoryChosen` formularz został przesłany do metody.

[!code-csharp[Main](using-the-dropdownlist-helper-with-aspnet-mvc/samples/sample5.cs)]

Przejdź do strony testu i wybierz **HTML SelectList** łącza. Strony HTML renderuje select element podobny do prostych strona testowa ASP.NET MVC. Kliknij prawym przyciskiem myszy okno przeglądarki i wybierz **Wyświetl źródło**. Kod znaczników HTML dla listy wyboru jest zasadniczo identyczne. Strona testowa HTML, działa jak metoda akcji ASP.NET MVC i widoku, który przetestowaliśmy wcześniej.

### <a name="improving-the-movie-select-list-with-enums"></a>Poprawa listy wyboru film z wyliczenia

Jeśli kategorii w aplikacji są stałe i że nie spowoduje zmiany, należy skorzystać z wyliczenia, aby kod był bardziej niezawodne i łatwiejsze do rozszerzenia. Po dodaniu nowej kategorii jest generowany wartość odpowiedniej kategorii. Pozwala uniknąć błędów kopiowania i wklejania, Dodaj nową kategorię, ale pamiętaj, aby zaktualizować wartości kategorii.

Otwórz *Controllers\HomeController.cs* pliku i sprawdź, czy następujący kod:

[!code-csharp[Main](using-the-dropdownlist-helper-with-aspnet-mvc/samples/sample6.cs)]

[Wyliczenia](https://msdn.microsoft.com/en-us/library/sbbt4032(VS.80).aspx) `eMovieCategories` przechwytuje filmu cztery typy. `SetViewBagMovieType` Metoda tworzy **IEnumerable&lt;SelectListItem &gt;**  z `eMovieCategories` **wyliczenia**i ustawia `Selected` właściwość z `selectedMovie` parametru. `SelectCategoryEnum` Metody akcji używa tego samego widoku jako `SelectCategory` metody akcji.

Przejdź do strony testowej i kliknij na `Select Movie Category (Enum)` łącza. Teraz, zamiast wartości (numer) będzie wyświetlany, ciąg reprezentujący wyliczenia jest wyświetlany.

### <a name="posting-enum-values"></a>Zamieszczając wartości wyliczenia

Formularze HTML są zwykle używane do publikowania danych do serwera. Poniższy kod przedstawia `HTTP GET` i `HTTP POST` wersji `SelectCategoryEnumPost` metody.

[!code-csharp[Main](using-the-dropdownlist-helper-with-aspnet-mvc/samples/sample7.cs)]

Przez przekazanie `eMovieCategories` enum `POST` metody, firma Microsoft może wyodrębnić zarówno wartość enum, jak i ciąg wyliczenia. Uruchom próbkę i przejdź do strony testowej. Polecenie `Select Movie Category(Enum Post)` łącza. Wybierz typ film, a następnie kliknij przycisk przesyłania. Pokazuje zarówno wartość, jak i nazwę typu filmu.

![](using-the-dropdownlist-helper-with-aspnet-mvc/_static/image7.png)

### <a name="creating-a-multiple-section-select-element"></a>Tworzenie wielu Element Select sekcji

[ListBox](https://msdn.microsoft.com/en-us/library/system.web.mvc.html.selectextensions.listbox.aspx) pomocnika kodu HTML, renderowania kodu HTML `<select>` element z `multiple` atrybut, który umożliwia użytkownikom dokonanie wielokrotnego wyboru. Przejdź do łącza testowego, a następnie wybierz **Multi wybierz kraj** łącza. Renderowany interfejsu użytkownika służy do wybierania wielu krajach. Na poniższej ilustracji Kanada i Chin są zaznaczone.

![](using-the-dropdownlist-helper-with-aspnet-mvc/_static/image8.png)

### <a name="examining-the-multiselectcountry-code"></a>Badanie kodu MultiSelectCountry

Przejrzyj następujący kod z *Controllers\HomeController.cs* pliku.

[!code-csharp[Main](using-the-dropdownlist-helper-with-aspnet-mvc/samples/sample8.cs)]

`GetCountries` Metoda tworzy listę krajów, a następnie przekazuje go do `MultiSelectList` konstruktora. `MultiSelectList` Przeładowania konstruktora używane w `GetCountries` powyżej metoda przyjmuje cztery parametry:

[!code-csharp[Main](using-the-dropdownlist-helper-with-aspnet-mvc/samples/sample9.cs)]

1. *elementy*: [IEnumerable](https://msdn.microsoft.com/en-us/library/system.collections.ienumerable.aspx) zawierających elementy na liście. W powyższym przykładzie, lista krajów.
2. *dataValueField*: Nazwa właściwości w **IEnumerable** listy, która zawiera wartość. W powyższym przykładzie `ID` właściwości.
3. *dataTextField*: Nazwa właściwości w **IEnumerable** listy, który zawiera informacje do wyświetlania. W powyższym przykładzie `name` właściwości.
4. *selectedValues*: Lista wybranych wartości.

W powyższym przykładzie `MultiSelectCountry` metoda przekazuje `null` wartość dla wybranych krajów, więc nie krajów nie wybrano wyświetlania interfejsu użytkownika. Poniższy kod przedstawia znaczników Razor używany do renderowania `MultiSelectCountry` widoku.

[!code-cshtml[Main](using-the-dropdownlist-helper-with-aspnet-mvc/samples/sample10.cshtml)]

Pomocnik kodu HTML [ListBox](https://msdn.microsoft.com/en-us/library/dd470200.aspx) metodę powyżej wykonaj dwa parametry, nazwa właściwości do wiązania modelu i [MultiSelectList](https://msdn.microsoft.com/en-us/library/system.web.mvc.multiselectlist.aspx) zawierający wybierz opcje i wartości. `ViewBag.YouSelected` Powyższym kodzie służy do wyświetlania wartości z krajów wybranej podczas przesyłania formularza. Sprawdź przeciążenia HTTP POST `MultiSelectCountry` metody.

[!code-csharp[Main](using-the-dropdownlist-helper-with-aspnet-mvc/samples/sample11.cs)]

`ViewBag.YouSelected` Właściwości dynamicznych zawiera określonych krajów, uzyskać `Countries` wpis w kolekcji formularza. W tej wersji metody GetCountries jest przekazywany listę krajów, w takim przypadku `MultiSelectCountry` zostanie wyświetlony widok, wybranych krajów są wybrane w interfejsie użytkownika.

### <a name="making-a-select-element-friendly-with-the-harvest-chosen-jquery-plugin"></a>Tworzenie, wybierz Element Friendly z jQuery zbioru wybranych wtyczki

Zbiór [wybrane](http://harvesthq.github.com/chosen/) jQuery wtyczki mogą być dodawane do kodu HTML &lt;wybierz&gt; element, aby utworzyć użytkownika przyjazną interfejsu użytkownika. Obrazy poniżej pokazują zbiorów [wybrane](http://harvesthq.github.com/chosen/) jQuery wtyczki o `MultiSelectCountry` widoku.

![](using-the-dropdownlist-helper-with-aspnet-mvc/_static/image9.png)

W poniższych dwóch obrazów **Kanada** jest zaznaczone.

![](using-the-dropdownlist-helper-with-aspnet-mvc/_static/image10.png)

![](using-the-dropdownlist-helper-with-aspnet-mvc/_static/image11.png)

Na ilustracji powyżej wybrano Kanada i zawiera **x** można kliknąć, aby usunąć zaznaczenie. Na poniższym obrazie pokazano Kanada, Chiny, i Japonia zaznaczona.

![](using-the-dropdownlist-helper-with-aspnet-mvc/_static/image12.png)

### <a name="hooking-up-the-harvest-chosen-jquery-plugin"></a>Podłączanie jQuery zbioru wybranych wtyczki

Poniższa sekcja jest łatwiejsze, jeśli użytkownik ma pewne doświadczenie z jQuery. Jeśli nie znasz jQuery przed, możesz Wypróbuj jedną z następujących samouczków jQuery.

- [Jak działa jQuery](http://docs.jquery.com/Tutorials:How_jQuery_Works) przez [Resig Jan](http://ejohn.org/)
- [Wprowadzenie do korzystania z jQuery](http://docs.jquery.com/Tutorials:Getting_Started_with_jQuery) przez [Jörn Zaefferer](http://bassistance.de/)
- [Przykłady jQuery Live](http://codylindley.com/blogstuff/js/jquery/#) przez [Cody Lindley](http://codylindley.com/)

Wybrany dodatek jest dostępny w starter i ukończone przykładowych projektach, dołączone do tego samouczka. W tym samouczku wystarczy umożliwia utworzenie punktu zaczepienia do interfejsu użytkownika jQuery. Aby użyć wtyczki jQuery zbioru wybranych w projekcie platformy ASP.NET MVC, należy:

1. Pobierz wtyczkę wybrane z [github](https://github.com/harvesthq/chosen/). W tym kroku zostało wykonane dla Ciebie.
2. Dodaj wybrany folder do projektu programu ASP.NET MVC. Dodaj zasoby z wtyczki wybrana pobranego w poprzednim kroku do wybranego folderu. W tym kroku zostało wykonane dla Ciebie.
3. Podłączanie wybrany dodatek plug-in, aby **DropDownList** lub **ListBox** pomocnika kodu HTML.

### <a name="hooking-up-the-chosen-plugin-to-the-multiselectcountry-view"></a>Podłączanie wtyczki wybranego widoku MultiSelectCountry.

Otwórz *Views\Home\MultiSelectCountry.cshtml* plik i dodać `htmlAttributes` parametr `Html.ListBox`. Parametr spowoduje dodanie zawiera nazwę klasy dla listy wyboru (`@class = "chzn-select"`). Kompletny kod jest pokazany poniżej:

[!code-cshtml[Main](using-the-dropdownlist-helper-with-aspnet-mvc/samples/sample12.cshtml)]

W powyższym kodzie dodajemy atrybutu HTML i wartości atrybutu `class = "chzn-select"`. @-Znak poprzedzających klasa nie ma nic wspólnego z aparatu widoku Razor. `class`jest [— słowo kluczowe języka C#](https://msdn.microsoft.com/en-us/library/x53a06bb.aspx). Słowa kluczowe języka C# nie można użyć jako identyfikatory, jeśli nie obejmują one jako prefiksu. W powyższym przykładzie `@class` jest prawidłowym identyfikatorem, ale **klasy** jest ponieważ **klasy** jest słowem kluczowym.

Dodaj odwołania do *Chosen/chosen.jquery.js* i *Chosen/chosen.css* plików. *Chosen/chosen.jquery.js* i implementuje funkcjonalnie z wtyczki wybrana. *Chosen/chosen.css* plik zawiera style. Dodaj te odwołania do dołu *Views\Home\MultiSelectCountry.cshtml* pliku. Poniższy kod przedstawia sposób wtyczki wybranego odwołania.

[!code-cshtml[Main](using-the-dropdownlist-helper-with-aspnet-mvc/samples/sample13.cshtml)]

Aktywowanie wtyczki wybrana nazwa klasy używane w **Html.ListBox** kodu. W powyższym przykładzie nazwa klasy jest `chzn-select`. Dodaj następujący wiersz do dołu *Views\Home\MultiSelectCountry.cshtml* widoku pliku. Ten wiersz aktywuje wybrane wtyczki.

[!code-html[Main](using-the-dropdownlist-helper-with-aspnet-mvc/samples/sample14.html)]

Następujący wiersz jest składnia służąca do wywołania funkcji gotowe jQuery, który wybiera element modelu DOM z nazwą klasy `chzn-select`.

[!code-powershell[Main](using-the-dropdownlist-helper-with-aspnet-mvc/samples/sample15.ps1)]

Opakowana wartość zwrócony z powyższego wywołania, to ma zastosowanie wybranej metody (`.chosen();`), który przechwytuje się wtyczki wybrana.

Poniższy kod przedstawia ukończonej *Views\Home\MultiSelectCountry.cshtml* widoku pliku.

[!code-cshtml[Main](using-the-dropdownlist-helper-with-aspnet-mvc/samples/sample16.cshtml)]

Uruchom aplikację i przejdź do `MultiSelectCountry` widoku. Spróbuj dodawania i usuwania krajach. Pobieranie próbki podane zawiera także `MultiCountryVM` — metoda i widoku, która implementuje funkcje MultiSelectCountry przy użyciu widoku modelu zamiast **obiekt ViewBag**.

W następnej sekcji zobaczysz, jak działa mechanizm szkieletów ASP.NET MVC z **DropDownList** pomocnika.

>[!div class="step-by-step"]
[Dalej](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper.md)
