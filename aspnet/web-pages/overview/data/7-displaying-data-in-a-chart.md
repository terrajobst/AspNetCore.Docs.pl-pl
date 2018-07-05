---
uid: web-pages/overview/data/7-displaying-data-in-a-chart
title: Wyświetlanie danych na wykresie przy użyciu stron ASP.NET Web Pages (Razor) | Dokumentacja firmy Microsoft
author: microsoft
description: W tym rozdziale wyjaśniono sposób wyświetlania danych na wykresie. W poprzednich akapitach pokazaliśmy ci, jak wyświetlać dane ręcznie, jak i w siatce. W tym rozdziale wyjaśniono...
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/22/2012
ms.topic: article
ms.assetid: f889fd46-4dac-4ecb-83d8-60e64c22036e
ms.technology: dotnet-webpages
msc.legacyurl: /web-pages/overview/data/7-displaying-data-in-a-chart
msc.type: authoredcontent
ms.openlocfilehash: 41af8ac4dba0ba5ad478df7075628186a96f48c4
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/03/2018
ms.locfileid: "37393759"
---
<a name="displaying-data-in-a-chart-with-aspnet-web-pages-razor"></a>Wyświetlanie danych na wykresie przy użyciu wzorca ASP.NET Web Pages (Razor)
====================
przez [firmy Microsoft](https://github.com/microsoft)

> W tym artykule wyjaśniono, jak używać wykresu do wyświetlania danych w witrynie internetowej ASP.NET Web Pages (Razor) przy użyciu `Chart` pomocnika.
> 
> **Dowiesz się**:
> 
> - Jak wyświetlać dane na wykresie.
> - Jak do nadawania stylu wykresów przy użyciu wbudowanych motywów.
> - Jak zapisywać wykresy oraz jak je skonfigurować buforowanie lepszą wydajność.
> 
> Poniżej przedstawiono funkcje wprowadzone w artykule programowania programu ASP.NET:
> 
> - `Chart` Pomocnika.
> 
> > [!NOTE]
> > Informacje przedstawione w tym artykule dotyczy 1.0 stron sieci Web platformy ASP.NET i Web Pages 2.


<a id="The_Chart_Helper"></a>
## <a name="the-chart-helper"></a>Pomocnik wykresu

Jeśli chcesz wyświetlać dane w formie graficznej, można użyć `Chart` pomocnika. `Chart` Pomocnika można renderować obraz, który wyświetla dane w różnych typów wykresów. Obsługuje ona wiele opcji formatowania i etykietowania. `Chart` Pomocnika umożliwiający renderowanie ponad 30 typów wykresów, w tym wszystkich typów wykresów, które mogą być znane z programu Microsoft Excel lub innych narzędzi &#8212; wykresy warstwowe, wykresy słupkowe, wykresy kolumnowe, wykresów liniowych i wykresy kołowe, wraz z więcej Wykresy specjalne, takie jak podstawowe wykresy.

| **Wykres warstwowy** ![opis: obraz typu obszaru wykresu](7-displaying-data-in-a-chart/_static/image1.jpg) | **Wykres słupkowy** ![opis: obraz typu wykresu słupkowego](7-displaying-data-in-a-chart/_static/image2.jpg) |
| --- | --- |
| **Wykres kolumnowy** ![opis: obraz wykresu kolumnowego](7-displaying-data-in-a-chart/_static/image3.jpg) | **Wykres liniowy** ![opis: obraz typu wykresu wiersza](7-displaying-data-in-a-chart/_static/image4.jpg) |
| **Wykres kołowy** ![opis: obraz typu wykresu kołowego](7-displaying-data-in-a-chart/_static/image5.jpg) | **Wykres giełdowy** ![opis: obraz Stock typ wykresu](7-displaying-data-in-a-chart/_static/image6.jpg) |

### <a name="chart-elements"></a>Elementy wykresu

Wykresy pokazują dane i dodatkowe elementy, takie jak legendami, osiami, serii i tak dalej. Na poniższej ilustracji przedstawiono wiele elementów wykresu, które można dostosować, korzystając z `Chart` pomocnika. W tym artykule pokazano, jak ustawić niektóre (nie wszystkie) z tych elementów.

![Opis: Obraz przedstawiający elementy wykresu](7-displaying-data-in-a-chart/_static/image7.jpg)

<a id="Creating_a_Chart"></a>
## <a name="creating-a-chart-from-data"></a>Tworzenie wykresu na podstawie danych

Dane, które są wyświetlane na wykresie może być z tablicy, na liście wyników zwrócony z bazy danych lub danych, który znajduje się w pliku XML.

### <a name="using-an-array"></a>Przy użyciu tablicy

Jak wyjaśniono w [wprowadzenie do platformy ASP.NET Web Pages programowania z użyciem składni Razor](https://go.microsoft.com/fwlink/?LinkId=202890), Tablica umożliwia przechowywanie kolekcji podobnych elementów w zmiennej. Tablice umożliwia zawierają dane, które mają zostać uwzględnione w planie.

W tej procedurze pokazano, jak utworzyć wykres z danymi w macierzy, przy użyciu domyślnego typu wykresu. Pokazano również, jak wyświetlić wykres na stronie.

1. Utwórz nowy plik o nazwie *ChartArrayBasic.cshtml*.
2. Zastąp istniejącą zawartość następujących czynności: 

    [!code-cshtml[Main](7-displaying-data-in-a-chart/samples/sample1.cshtml)]

    Ten kod najpierw tworzy nowy wykres i ustawia jego szerokość i wysokość. Należy określić tytuł wykresu za pomocą `AddTitle` metody. Aby dodać dane, należy użyć `AddSeries` metody. W tym przykładzie używamy `name`, `xValue`, i `yValues` parametry `AddSeries` metody. `name` Parametru jest wyświetlany w legendzie wykresu. `xValue` Parametr zawiera tablicę danych wyświetlanych na osi poziomej wykresu. `yValues` Parametr zawiera tablicę danych służącą do nakreślenia pionowych punktów wykresu.

    `Write` Metoda renderuje faktycznie wykresu. W tym przypadku, ponieważ nie określono typu wykresu `Chart` pomocnika powoduje wyświetlenie jego wykresu domyślny, który jest wykres kolumnowy.
3. Uruchom stronę w przeglądarce. W przeglądarce pojawi się wykres. 

    ![](7-displaying-data-in-a-chart/_static/image8.jpg)

### <a name="using-a-database-query-for-chart-data"></a>Za pomocą zapytania do bazy danych dla danych wykresu

Jeśli informacje, które chcesz wykresu znajduje się w bazie danych, można uruchomić zapytania do bazy danych i następnie użyć danych z wyników, aby utworzyć wykres. Ta procedura dowiesz się, jak czytać i wyświetlić dane z bazy danych utworzonej w artykule [wprowadzenie do pracy z bazą danych w witrynach stron sieci Web platformy ASP.NET](https://go.microsoft.com/fwlink/?LinkId=202893).

1. Dodaj *aplikacji\_danych* folder w katalogu głównym witryny sieci Web, jeśli folder nie istnieje.
2. W *aplikacji\_danych* folderu, Dodaj plik bazy danych o nazwie *SmallBakery.sdf* opisanym w [wprowadzenie do pracy z bazą danych w witrynach stron sieci Web platformy ASP.NET](https://go.microsoft.com/fwlink/?LinkId=202893).
3. Utwórz nowy plik o nazwie *ChartDataQuery.cshtml*.
4. Zastąp istniejącą zawartość następujących czynności:   

    [!code-cshtml[Main](7-displaying-data-in-a-chart/samples/sample2.cshtml)]

    Ten kod najpierw otwarcie bazy danych, SmallBakery i przypisuje go do zmiennej o nazwie `db`. Ta zmienna reprezentuje `Database` obiekt, który może służyć do odczytu i zapisu w bazie danych. Następnie kod uruchamia zapytanie SQL w celu uzyskania nazwy i cenę każdego produktu. Ten kod tworzy nowy wykres i przekazuje zapytanie bazy danych przez wywołanie metody na wykresie `DataBindTable` metody. Ta metoda przyjmuje dwa parametry: `dataSource` parametr jest używany dane z zapytania, a `xField` parametr umożliwia określenie, która kolumna danych jest używany dla osi x wykresu.

    Jako alternatywa dla użycia `DataBindTable` metody, można użyć `AddSeries` metody `Chart` pomocnika. `AddSeries` Metoda pozwala na ustalanie `xValue` i `yValues` parametrów. Na przykład, zamiast `DataBindTable` metoda następująco:

    [!code-css[Main](7-displaying-data-in-a-chart/samples/sample3.css)]

    Możesz użyć `AddSeries` metoda następująco:

    [!code-html[Main](7-displaying-data-in-a-chart/samples/sample4.html)]

    Renderuj oba te same wyniki. `AddSeries` Metody jest bardziej elastyczna, ponieważ można określić typ wykresu i danych bardziej jawny sposób, ale `DataBindTable` metodą jest łatwiejszy w obsłudze, jeśli jest potrzebna dodatkowa elastyczność.
5. Uruchom stronę w przeglądarce. 

    ![](7-displaying-data-in-a-chart/_static/image9.jpg)

### <a name="using-xml-data"></a>Korzystanie z danych XML

Trzecia opcja dla wykresów jest używany jest plik XML jako źródło danych dla wykresu. Wymaga to, że plik XML również mieć plik schematu (*XSD* pliku), który opisuje strukturze XML. Ta procedura pokazuje, jak odczytywać dane z pliku XML.

1. W *aplikacji\_danych* folderu, Utwórz nowy plik XML o nazwie *data.xml*.
2. Zastąp istniejący plik XML przy użyciu poniższych składników, które to dane XML o pracownikach w fikcyjnej firmie. 

    [!code-xml[Main](7-displaying-data-in-a-chart/samples/sample5.xml)]
3. W *aplikacji\_danych* folderu, Utwórz nowy plik XML o nazwie *data.xsd*. (Należy pamiętać, że rozszerzenie jest w tej chwili *XSD*.)
4. Zastąp istniejący kod XML następujących czynności: 

    [!code-xml[Main](7-displaying-data-in-a-chart/samples/sample6.xml)]
5. W katalogu głównym witryny sieci Web, Utwórz nowy plik o nazwie *ChartDataXML.cshtml*.
6. Zastąp istniejącą zawartość następujących czynności: 

    [!code-cshtml[Main](7-displaying-data-in-a-chart/samples/sample7.cshtml)]

    Kod najpierw tworzy `DataSet` obiektu. Ten obiekt jest używany do zarządzania danymi, które są odczytywane z pliku XML i organizowania go zgodnie z informacjami w pliku schematu. (Zwróć uwagę, że górnej części kodu zawiera instrukcję `using SystemData`. Jest to wymagane, aby można było pracować z `DataSet` obiektu. Aby uzyskać więcej informacji, zobacz [ &quot;Using&quot; instrukcji i w pełni kwalifikowane nazwy](#SB_UsingStatements) w dalszej części tego artykułu.)

    Następnie kod tworzy `DataView` obiektu na podstawie zestawu danych. Widok danych zawiera obiekt, który można powiązać wykres &#8212; oznacza to, Odczyt i wykreślania. Wykres, który tworzy powiązanie danych przy użyciu `AddSeries` metody, jak był wyświetlany w przypadku wykresów danych, chyba że teraz `xValue` i `yValues` parametry są ustawione na `DataView` obiektu.

    W tym przykładzie przedstawiono również sposób określania typu wykresu określonego Gdy dane są dodawane w `AddSeries` metody `chartType` parametru jest również ustawiona na wyświetlania wykresu kołowego.
7. Uruchom stronę w przeglądarce. 

    ![](7-displaying-data-in-a-chart/_static/image10.jpg)

> [!TIP]
> 
> <a id="SB_UsingStatements"></a>
> ### <a name="using-statements-and-fully-qualified-names"></a>Instrukcje "Za pomocą" i w pełni kwalifikowane nazwy
> 
> .NET Framework, ASP.NET Web Pages o składni Razor oparty na składa się z wielu tysięcy składników (grupy). Aby umożliwić łatwe w zarządzaniu do pracy z tych klas, są zorganizowane w *przestrzenie nazw*, które są nieco, takich jak biblioteki. Na przykład `System.Web` przestrzeń nazw zawiera klasy, które obsługują komunikację przeglądarką a serwerem `System.Xml` przestrzeń nazw zawiera klasy, które są używane do tworzenia i odczytywania plików XML i `System.Data` przestrzeń nazw zawiera klasy, które pozwalają pracować z danymi.
> 
> Aby uzyskać dostęp do danej klasy w .NET Framework, kod musi wiedzieć, nie tylko nazwę klasy, ale także przestrzeni nazw, który znajduje się w klasie. Na przykład, aby można było używać `Chart` pomocnika, kod musi znaleźć `System.Web.Helpers.Chart` klasy, która łączy przestrzeni nazw (`System.Web.Helpers`) o nazwie klasy (`Chart`). Jest to nazywane klasy *pełną* nazwa &#8212; pełną, jednoznaczną lokalizacji w ramach vastness programu .NET Framework. W kodzie to będzie wyglądać następująco:
> 
> `var myChart = new System.Web.Helpers.Chart(width: 600, height: 400) // etc.`
> 
> Jednak jest kłopotliwe (i podatne) musi używać tych nazw długie, w pełni kwalifikowanej, za każdym razem, gdy chcesz odwołać się do klasy lub pomocnika. W związku z tym, aby ułatwić nazwy klas, możesz *zaimportować* przestrzenie nazw interesuje Cię, który zazwyczaj jest to tylko kilka spośród wielu przestrzeniach nazw programu .NET Framework. Jeśli zaimportowane przestrzeni nazw, można użyć tylko nazwy klasy (`Chart`) zamiast z w pełni kwalifikowana nazwa (`System.Web.Helpers.Chart`). Kod uruchamia i napotka nazwy klasy, można wygląda w właśnie przestrzeni nazw zaimportowanymi można znaleźć tej klasy.
> 
> Korzystając z ASP.NET Web Pages o składni Razor do tworzenia stron sieci web, zazwyczaj używasz tego samego zestawu klas za każdym razem w tym `WebPage` klasy, różnych wątków i tak dalej. Aby zapisać pracę importowania odpowiednie przestrzenie nazw, za każdym razem, gdy tworzysz witrynę sieci Web, ASP.NET jest skonfigurowany tak automatycznie importuje zestaw podstawowej przestrzeni nazw, dla każdej witryny sieci Web. Dlatego nie ma do czynienia z przestrzeni nazw lub zaimportowanie do chwili obecnej; wszystkie klasy, do których się znajdują się w przestrzeni nazw, które zostały już zaimportowane dla Ciebie.
> 
> Jednak czasami musisz pracować z klasą, która nie znajduje się w przestrzeni nazw, który jest importowany automatycznie dla Ciebie. W takim przypadku możesz użyć tej klasy w pełni kwalifikowaną nazwę lub można ręcznie zaimportować obszar nazw, który zawiera klasę. Aby zaimportować przestrzeni nazw, należy użyć `using` — instrukcja (`import` w języku Visual Basic), jak pokazano w przykładzie wcześniej tego artykułu.
> 
> Na przykład `DataSet` klasa się zebrała `System.Data` przestrzeni nazw. `System.Data` Przestrzeni nazw nie jest automatycznie dostępny dla stron Razor programu ASP.NET. W związku z tym aby pracować z `DataSet` przy użyciu jego w pełni kwalifikowana nazwa, możesz użyć kodu w następujący sposób:
> 
> `var dataSet = new System.Data.DataSet();`
> 
> Jeśli trzeba użyć `DataSet` wielokrotnie klasy można importować przestrzeni nazw w następujący sposób i następnie użyć tylko nazwę klasy w kodzie:
> 
> [!code-cshtml[Main](7-displaying-data-in-a-chart/samples/sample8.cshtml)]
> 
> Możesz dodać `using` instrukcji dla żadnych innych .NET Framework przestrzenie nazw, które ma dotyczyć odwołanie. Jak wspomniano, nie będzie potrzebny, w tym celu często, ponieważ większość klas, którą będziesz pracować, które znajdują się w przestrzeni nazw, które są automatycznie importowane przez platformę ASP.NET do użycia w *.cshtml* i *.vbhtml* stron.


<a id="Displaying_Charts"></a>
## <a name="displaying-charts-inside-a-web-page"></a>Wyświetlanie wykresów na stronie sieci Web

W przykładach przedstawiono do tej pory utworzyć wykres, a następnie renderowania wykresu bezpośrednio do przeglądarki jako grafika. W wielu przypadkach jednak chcesz wyświetlić wykres jako część strony, nie tylko przez siebie w przeglądarce. Aby to zrobić wymaga procesem dwuetapowym. Pierwszym krokiem jest utworzyć stronę, która generuje wykres, zgodnie z wyświetlonymi już wcześniej.

Drugim krokiem jest do wyświetlenia obraz wynikowy w innej strony. Aby wyświetlić obraz, należy użyć kodu HTML `<img>` elementu w taki sam sposób, w jaki do wyświetlania dowolnego obrazu. Jednak zamiast odwoływać się do *.jpg* lub *.png* pliku `<img>` odwołania do elementu *.cshtml* pliku, który zawiera `Chart` pomocnika, Tworzy wykres. Po uruchomieniu strony ekranu, `<img>` element pobiera dane wyjściowe `Chart` pomocnika i renderuje wykresu.

![](7-displaying-data-in-a-chart/_static/image11.jpg)

1. Utwórz plik o nazwie *ShowChart.cshtml*.
2. Zastąp istniejącą zawartość następujących czynności: 

    [!code-html[Main](7-displaying-data-in-a-chart/samples/sample9.html)]

    Kod używa `<img>` element, aby wyświetlić wykres, który został wcześniej utworzony w *ChartArrayBasic.cshtml* pliku.
3. Uruchom stronę sieci web w przeglądarce. *ShowChart.cshtml* plik zawiera obraz wykresu, w oparciu o kod zawarty w *ChartArrayBasic.cshtml* pliku.

<a id="Styling_a_Chart"></a>
## <a name="styling-a-chart"></a>Ustawianie stylów na wykresie

`Chart` Pomocnika obsługuje dużą liczbę opcji, które umożliwiają dostosowanie wyglądu wykresu. Można ustawić kolory, czcionki, obramowania i tak dalej. Łatwe dostosowywanie wyglądu wykresu jest użycie *motyw*. Motywy to kolekcje informacji określających sposób renderowania wykresu przy użyciu czcionek, kolorów, etykiet, palet, obramowania i efektów. (Zwróć uwagę, że styl wykresu nie wskazuje typ wykresu).

Poniższa tabela zawiera listę wbudowanych motywów.

| Motyw | Opis |
| --- | --- |
| `Vanilla` | Wyświetla czerwony kolumn na białym tle. |
| `Blue` | Wyświetla niebieskie kolumn na niebieskim tle gradientu. |
| `Green` | Wyświetla niebieski kolumn znajdujących się zielony gradientu tła. |
| `Yellow` | Wyświetla pomarańczowy kolumn w żółte tło gradientu. |
| `Vanilla3D` | Wyświetla 3-czerwony kolumn na białym tle. |

Można określić motywu, który ma używać podczas tworzenia nowego wykresu.

1. Utwórz nowy plik o nazwie *ChartStyleGreen.cshtml*.
2. Zastąp istniejącą zawartość na stronie następujących czynności:

    [!code-cshtml[Main](7-displaying-data-in-a-chart/samples/sample10.cshtml)]

    Ten kod jest taki sam jak poprzedniego przykładu, który korzysta z bazy danych dla danych, ale dodaje `theme` parametru podczas tworzenia `Chart` obiektu. Poniżej przedstawiono zmiany kodu:

    [!code-csharp[Main](7-displaying-data-in-a-chart/samples/sample11.cs)]
3. Uruchom stronę w przeglądarce. Zobacz te same dane, co przed, ale bardziej dopracowane wyglądu wykresu: 

    ![](7-displaying-data-in-a-chart/_static/image12.jpg)

<a id="Saving_a_Chart"></a>
## <a name="saving-a-chart"></a>Zapisanie wykresu

Kiedy używasz `Chart` pomocnika, jak w tym samouczku do tej pory w tym artykule pomocnika ponownie tworzy wykres od podstaw każdym razem jest wywoływana. Jeśli to konieczne, kod dla wykresu również ponownie zapytania bazy danych lub ponownie odczytuje plik XML, aby pobrać dane. W niektórych przypadkach, w ten sposób może być złożonych operacji, takich jak jeśli zadajesz zapytanie do bazy danych jest duży, czy plik XML zawiera dużą ilość danych. Nawet jeśli wykres nie wymaga dużej ilości danych, proces dynamicznie Tworzenie obrazu zajmuje zasoby serwera, a jeśli wiele osób żądanie strony, które wyświetlić wykres, mogą być wpływ na wydajność witryny sieci Web.

Pozwala zmniejszyć potencjalne obniżenie wydajności podczas tworzenia wykresu, można utworzyć wykres przy jego pierwszym czas potrzebny, a następnie zapisz go. Gdy wykres jest ponownie potrzebny, zamiast ponownego generowania, możesz po prostu pobrać zapisaną wersję i renderować.

Wykres można zapisać w następujący sposób:

- Wykres w pamięci komputera (na serwerze) w pamięci podręcznej.
- Zapisz wykres jako plik obrazu.
- Zapisz wykres jako plik XML. Ta opcja pozwala zmodyfikować wykres, przed jej zapisaniem.

### <a name="caching-a-chart"></a>Buforowanie wykresu

Po utworzeniu wykresu możesz pamięci podręcznej. Buforowanie wykres oznacza, że nie ma zostać utworzony ponownie, jeśli należy go ponownie wyświetlone. Podczas zapisywania wykresu w pamięci podręcznej, możesz nadać mu kluczem, który musi być unikatowa dla tego wykresu.

Wykresy zapisane w pamięci podręcznej może zostać usunięty, jeśli serwer jest za mało pamięci. Ponadto pamięć podręczna jest czyszczona, jeśli aplikacja uruchamia ponownie jakiegokolwiek powodu. W związku z tym standardowy sposób pracy z pamięci podręcznej wykresu jest zawsze najpierw sprawdzić, czy jest dostępna w pamięci podręcznej, a w przeciwnym razie, a następnie utwórz lub utwórz go ponownie.

1. W katalogu głównym witryny sieci Web, Utwórz plik o nazwie *ShowCachedChart.cshtml*.
2. Zastąp istniejącą zawartość następujących czynności: 

    [!code-html[Main](7-displaying-data-in-a-chart/samples/sample12.html)]

    `<img>` Znacznik obejmuje `src` atrybut, który wskazuje na *ChartSaveToCache.cshtml* plik i przekazuje klucz do strony jako ciąg zapytania. Klucza zawiera wartość &quot;myChartKey&quot;. *ChartSaveToCache.cshtml* plik zawiera `Chart` pomocnika, która tworzy wykres. Utworzysz tę stronę za chwilę.

    Na końcu strony znajduje się link do strony o nazwie *ClearCache.cshtml*. To strona, która utworzysz też wkrótce. Potrzebujesz *ClearCache.cshtml* tylko w celu testowania buforowania w tym przykładzie — nie jest łącze lub strona, która zwykle należy dołączyć podczas pracy z wykresami pamięci podręcznej.
3. W katalogu głównym witryny sieci Web, Utwórz nowy plik o nazwie *ChartSaveToCache.cshtml*.
4. Zastąp istniejącą zawartość następujących czynności:

    [!code-cshtml[Main](7-displaying-data-in-a-chart/samples/sample13.cshtml)]

    Ten kod najpierw sprawdza, czy przekazano wszystko jako wartości klucza w ciągu zapytania. Jeśli tak, kod próbuje odczytać wykres z pamięci podręcznej przez wywołanie metody `GetFromCache` metody i przekazanie do niej klucza. Okazuje się, że nie ma nic w pamięci podręcznej, w tym kluczu (co się stanie po raz pierwszy wykres jest wymagane), ten kod tworzy wykres w zwykły sposób. Po zakończeniu wykresu kodu zapisuje je w pamięci podręcznej przez wywołanie metody `SaveToCache`. Ta metoda wymaga klucza (tak, aby później można żądać wykres) i czas, który chcesz zapisać wykres w pamięci podręcznej. (Dokładny czas będzie buforować wykres będzie zależeć od jak często użytkownik uważa, że dane, które reprezentuje mogą ulec zmianie). `SaveToCache` Wymaga również metoda `slidingExpiration` parametru &#8212; Jeśli ta opcja jest ustawiona na wartość true, limit czasu licznik jest resetowany każdorazowo wykresu jest dostępny. W tym przypadku w praktyce oznacza to, że wpis pamięci podręcznej wykresu wygaśnie 2 minuty po ostatnim ktoś uzyskać dostęp do wykresu. (Zamiast przedłużanie ważności jest bezwzględna wygaśnięcia, co oznacza, że wpis pamięci podręcznej upływa dokładnie 2 minuty po został umieszczony w pamięci podręcznej, niezależnie od tego, jak często ma były używane).

    Na koniec kod używa `WriteFromCache` metodę, aby pobrać i renderować wykres z pamięci podręcznej. Należy zauważyć, że ta metoda jest poza `if` blok, który sprawdza, czy pamięć podręczna, ponieważ pobierze wykres z pamięci podręcznej czy wykres został ma rozpoczynać się, czy ma zostać wygenerowany i zapisane w pamięci podręcznej.

    Należy zauważyć, że w tym przykładzie `AddTitle` metoda zawiera sygnaturę czasową. (Dodaje bieżącą datę i godzinę &#8212; `DateTime.Now` &#8212; tytuł.)
5. Utwórz nową stronę o nazwie *ClearCache.cshtml* i zastąp jego zawartość następującym kodem:

    [!code-cshtml[Main](7-displaying-data-in-a-chart/samples/sample14.cshtml)]

    Ta strona używa `WebCache` pomocnika wykres, który jest buforowany w *ChartSaveToCache.cshtml*. Jak wspomniano wcześniej, zwykle nie trzeba mieć stronę następująco. Tworzysz tu tylko po to, aby ułatwić testowanie buforowania.
6. Uruchom *ShowCachedChart.cshtml* strony sieci web w przeglądarce. Zostanie wyświetlona strona obraz wykresu, w oparciu o kod zawarty w *ChartSaveToCache.cshtml* pliku. Zanotuj sygnaturę czasową mówi w tytuł wykresu. 

    ![Opis: Obraz podstawowego wykresu z sygnaturą czasową w tytuł wykresu](7-displaying-data-in-a-chart/_static/image13.jpg)
7. Zamknij przeglądarkę.
8. Uruchom *ShowCachedChart.cshtml* ponownie. Należy zauważyć, że sygnatura czasowa jest taka sama jak wcześniej, co oznacza, że wykres nie został ponownie wygenerowany, ale zamiast tego została odczytana z pamięci podręcznej.
9. W *ShowCachedChart.cshtml*, kliknij przycisk **wyczyścić pamięć podręczną** łącza. Spowoduje to przejście do *ClearCache.cshtml*, który zgłasza, że pamięć podręczna została wyczyszczona.
10. Kliknij przycisk **powrócić do ShowCachedChart.cshtml** połączyć lub uruchomić ponownie *ShowCachedChart.cshtml* z programu WebMatrix. Zwróć uwagę, że teraz sygnaturę czasową została zmieniona, ponieważ pamięć podręczna została wyczyszczona. W związku z tym w tym celu należy wygenerować ponownie wykresu, a następnie przestawić je ponownie w pamięci podręcznej musiały kodu.

### <a name="saving-a-chart-as-an-image-file"></a>Zapisanie wykresu jako pliku obrazu

Można również zapisać wykres jako plik obrazu (na przykład, jako *.jpg* pliku) na serwerze. Można następnie użyć pliku obrazu sposób, w jaki dowolny obraz. Zaletą jest to plik jest przechowywany zamiast zapisane w tymczasowej pamięci podręcznej. Można zapisać nowego obrazu wykresu w różnym czasie (na przykład co godzinę) i następnie rejestrować je trwałych zmian, które występują w czasie. Pamiętaj, że należy się upewnić, że aplikacja sieci web ma uprawnienia do zapisania pliku do folderu na serwerze, na którym chcesz umieścić plik obrazu.

1. W katalogu głównym witryny sieci Web utwórz folder o nazwie  *\_ChartFiles* Jeśli jeszcze nie istnieje.
2. W katalogu głównym witryny sieci Web, Utwórz nowy plik o nazwie *ChartSave.cshtml*.
3. Zastąp istniejącą zawartość następujących czynności:

    [!code-cshtml[Main](7-displaying-data-in-a-chart/samples/sample15.cshtml)]

    Ten kod najpierw sprawdza, czy *.jpg* plik istnieje, wywołując `File.Exists` metody. Jeśli plik nie istnieje, kod tworzy nową `Chart` z tablicy. Tym razem, kod wywołuje `Save` metody i przekazuje `path` parametru, aby określić ścieżkę i nazwę pliku, gdzie zapisać wykres. W treści strony `<img>` element używa ścieżki, aby wskazywał *.jpg* plik, aby wyświetlić.
4. Uruchom *ChartSave.cshtml* pliku.
5. Wróć do programu WebMatrix. Należy zauważyć, że plik obrazu o nazwie *chart01.jpg* został zapisany w  *\_ChartFiles* folderu.

### <a name="saving-a-chart-as-an-xml-file"></a>Zapisywanie wykres jako plik XML

Na koniec można zapisać wykres jako plik XML na serwerze. Zaletą tej metody w pamięci podręcznej wykresu lub zapisywania wykresu do pliku jest, czy można zmodyfikować plik XML, przed wyświetleniem wykresu, jeśli chce się. Aplikacja musi mieć uprawnienia odczytu/zapisu do folderu na serwerze, w którym chcesz umieścić plik obrazu.

1. W katalogu głównym witryny sieci Web, Utwórz nowy plik o nazwie *ChartSaveXml.cshtml*.
2. Zastąp istniejącą zawartość następujących czynności:

    [!code-cshtml[Main](7-displaying-data-in-a-chart/samples/sample16.cshtml)]

    Ten kod jest podobny do kodu, który był wyświetlany zapisywania wykresu w pamięci podręcznej, z tą różnicą, że używa pliku XML. Ten kod najpierw sprawdza, czy plik XML istnieje, wywołując `File.Exists` metody. Jeśli plik istnieje, kod tworzy nową `Chart` obiektu i przekazuje nazwę pliku jako `themePath` parametru. Spowoduje to utworzenie wykresu oparte na dowolnie znajduje się w pliku XML. Jeśli jeszcze nie istnieje w pliku XML, ten kod tworzy wykres, takich jak normalne, a następnie wywołuje `SaveXml` go zapisać. Wykres jest renderowany przy użyciu `Write` metody, jak przedstawiono przed.

    Podobnie jak w przypadku strony, która wykazała, buforowanie, ten kod zawiera sygnaturę czasową tytuł wykresu.
3. Utwórz nową stronę o nazwie *ChartDisplayXMLChart.cshtml* i Dodaj następujący kod do niego: 

    [!code-html[Main](7-displaying-data-in-a-chart/samples/sample17.html)]
4. Uruchom *ChartDisplayXMLChart.cshtml* strony. Wykres zostanie wyświetlony. Zanotuj sygnaturę czasową tytuł wykresu.
5. Zamknij przeglądarkę.
6. W programie WebMatrix, kliknij prawym przyciskiem myszy  *\_ChartFiles* folderu, kliknij przycisk **Odśwież**, a następnie otwórz folder. *XMLChart.xml* plik w tym folderze został utworzony przez `Chart` pomocnika. 

    ![Opis: Folder _ChartFiles przedstawiający plik XMLChart.xml utworzony przez pomocnika wykresu.](7-displaying-data-in-a-chart/_static/image14.jpg)
7. Uruchom *ChartDisplayXMLChart.cshtml* strony. Wykres ten przedstawia tego samego znacznika czasu jako pierwszego uruchomienia strony. To, ponieważ wykresu jest generowany z pliku XML został wcześniej zapisany.
8. W programie WebMatrix Otwórz  *\_ChartFiles* folder i Usuń *XMLChart.xml* pliku.
9. Uruchom *ChartDisplayXMLChart.cshtml* stronie jeszcze raz. Tym razem zaktualizować sygnaturę czasową, ponieważ `Chart` pomocnika musieli ponownie utworzyć plik XML. Jeśli chcesz, sprawdź  *\_ChartFiles* folder i zwróć uwagę, że plik XML jest ponownie.

<a id="Additional_Resources"></a>
## <a name="additional-resources"></a>Dodatkowe zasoby

- [Wprowadzenie do pracy z bazą danych we wzorcu ASP.NET Web Pages witryn](https://go.microsoft.com/fwlink/?LinkId=202893)
- [Przy użyciu buforowania we wzorcu ASP.NET Web Pages lokacjach w celu zwiększenia wydajności](https://go.microsoft.com/fwlink/?LinkId=202903)
- [Klasa wykresu](https://msdn.microsoft.com/library/system.web.helpers.chart(v=vs.99)) (Dokumentacja interfejsu API platformy ASP.NET Web Pages w witrynie MSDN)
