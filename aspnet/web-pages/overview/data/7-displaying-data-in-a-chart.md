---
uid: web-pages/overview/data/7-displaying-data-in-a-chart
title: Wyświetlanie danych na wykresie ze stronami sieci Web platformy ASP.NET (Razor) | Dokumentacja firmy Microsoft
author: microsoft
description: W tym rozdziale opisano sposób wyświetlania danych na wykresie. W poprzednich rozdziałach przedstawiono sposób wyświetlania danych ręcznie, jak i w siatce. W tym rozdziale opisano...
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/22/2012
ms.topic: article
ms.assetid: f889fd46-4dac-4ecb-83d8-60e64c22036e
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/overview/data/7-displaying-data-in-a-chart
msc.type: authoredcontent
ms.openlocfilehash: 5cf17e54408d585e9a375b302b61b4e28d9b022a
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/06/2018
---
<a name="displaying-data-in-a-chart-with-aspnet-web-pages-razor"></a>Wyświetlanie danych na wykresie ze stronami sieci Web platformy ASP.NET (Razor)
====================
przez [firmy Microsoft](https://github.com/microsoft)

> W tym artykule opisano sposób użycia wykresu do wyświetlania danych w witrynie sieci Web platformy ASP.NET Web Pages (Razor) przy użyciu `Chart` pomocnika.
> 
> **Dowiesz się**:
> 
> - Sposób wyświetlania danych na wykresie.
> - Jak styl wykresów przy użyciu wbudowanych motywów.
> - Aby zapisać wykresy oraz sposobu buforowania ich w celu zapewnienia lepszej wydajności.
> 
> Są to programowania funkcje dodane w artykule programu ASP.NET:
> 
> - `Chart` Pomocnika.
> 
> > [!NOTE]
> > Informacje przedstawione w tym artykule dotyczą 1.0 stron sieci Web ASP.NET i 2 stron sieci Web.


<a id="The_Chart_Helper"></a>
## <a name="the-chart-helper"></a>Pomocnik wykresu

Jeśli chcesz wyświetlać dane w formie graficznej, możesz użyć `Chart` pomocnika. `Chart` Pomocnika może renderować obraz, który wyświetla dane w różnych typów wykresów. Obsługuje wiele opcji formatowania i etykietowania. `Chart` Pomocnika umożliwiający renderowanie ponad 30 typów wykresów, w tym wszystkie typy wykresów, które mogą być znane z programu Microsoft Excel lub innych narzędzi &#8212; wykresy, wykresy słupkowe, wykresy kolumnowe, wykresy liniowe i wykresy kołowe wraz z więcej Wykresy specjalnych, takich jak wykresy standardowych.

| **Wykres warstwowy** ![opis: obraz typu obszaru wykresu](7-displaying-data-in-a-chart/_static/image1.jpg) | **Wykres słupkowy** ![opis: obraz wykresu słupkowego](7-displaying-data-in-a-chart/_static/image2.jpg) |
| --- | --- |
| **Wykres kolumnowy** ![opis: obraz wykresu kolumnowego](7-displaying-data-in-a-chart/_static/image3.jpg) | **Wykres liniowy** ![opis: obraz typu wykresu wiersza](7-displaying-data-in-a-chart/_static/image4.jpg) |
| **Wykres kołowy** ![opis: obraz typ wykresu kołowego](7-displaying-data-in-a-chart/_static/image5.jpg) | **Wykres giełdowy** ![opis: obraz wykresu typu zasobu](7-displaying-data-in-a-chart/_static/image6.jpg) |

### <a name="chart-elements"></a>Elementy wykresu

Na wykresach danych i dodatkowe elementy, takie jak legendy, osi serii i tak dalej. Na poniższej ilustracji pokazano wiele elementów wykresu, które można dostosować, korzystając z `Chart` pomocnika. W tym artykule przedstawiono sposób ustawiania niektórych (nie wszystkie) z tych elementów.

![Opis: Obraz przedstawiający elementów wykresu](7-displaying-data-in-a-chart/_static/image7.jpg)

<a id="Creating_a_Chart"></a>
## <a name="creating-a-chart-from-data"></a>Tworzenie wykresu ze źródła danych

Dane wyświetlane na wykresie można z tablicy, od wyników zwróconych z bazy danych lub danych, który znajduje się w pliku XML.

### <a name="using-an-array"></a>Przy użyciu tablicy

Zgodnie z objaśnieniem w [wprowadzenie do platformy ASP.NET Web Pages programowania przy użyciu składni Razor](https://go.microsoft.com/fwlink/?LinkId=202890), tablicy umożliwia przechowywanie kolekcji podobnych elementów w zmiennej. Tablice umożliwia zawierają dane, które chcesz uwzględnić w planie.

W tej procedurze pokazano sposób tworzenia wykresu z danych w tablicach, przy użyciu domyślnego typu wykresu. Widoczny jest również sposób wyświetlania wykresu wewnątrz strony.

1. Utwórz nowy plik o nazwie *ChartArrayBasic.cshtml*.
2. Zastąp istniejącą zawartość następujących czynności: 

    [!code-cshtml[Main](7-displaying-data-in-a-chart/samples/sample1.cshtml)]

    Kod najpierw tworzy nowy wykres i ustawia szerokości i wysokości. Określ tytuł wykresu przy użyciu `AddTitle` metody. Aby dodać dane, należy użyć `AddSeries` metody. W tym przykładzie używamy `name`, `xValue`, i `yValues` parametry `AddSeries` metody. `name` Parametru jest wyświetlany w legendzie wykresu. `xValue` Parametr zawiera tablicę danych wyświetlanych na osi poziomej wykresu. `yValues` Parametr zawiera tablicę danych służącą do nakreślenia pionowych punktów wykresu.

    `Write` Metoda renderuje faktycznie wykresu. W takim przypadku, ponieważ nie określono typu wykresu `Chart` pomocnika renderowania wykresu jego domyślny, który jest wykresu kolumnowego.
3. Uruchom strony w przeglądarce. Przeglądarka wyświetla wykres. 

    ![](7-displaying-data-in-a-chart/_static/image8.jpg)

### <a name="using-a-database-query-for-chart-data"></a>Za pomocą kwerendy bazy danych dla danych wykresu

Jeśli informacje, które chcesz wykresu znajduje się w bazie danych, można uruchomić kwerendy bazy danych i następnie użyć do utworzenia wykresu danych z wyników. W tej procedurze przedstawiono sposób odczytu i wyświetlanie danych z bazy danych utworzone w artykule [wprowadzenie do pracy z bazą danych w witrynach stron sieci Web platformy ASP.NET](https://go.microsoft.com/fwlink/?LinkId=202893).

1. Dodaj *aplikacji\_danych* folder w katalogu głównym witryny sieci Web, jeśli folder już nie istnieje.
2. W *aplikacji\_danych* folderu, Dodaj plik bazy danych o nazwie *SmallBakery.sdf* opisanym w [wprowadzenie do pracy z bazą danych w witrynach stron sieci Web platformy ASP.NET](https://go.microsoft.com/fwlink/?LinkId=202893).
3. Utwórz nowy plik o nazwie *ChartDataQuery.cshtml*.
4. Zastąp istniejącą zawartość następujących czynności:   

    [!code-cshtml[Main](7-displaying-data-in-a-chart/samples/sample2.cshtml)]

    Kod najpierw otwarcie bazy danych, SmallBakery i przypisuje go do zmiennej o nazwie `db`. Ta zmienna reprezentuje `Database` obiekt, który może służyć do odczytu i zapisu w bazie danych. Następnie kod uruchamia zapytanie SQL, aby uzyskać nazwę i ceny każdego produktu. Kod pozwala utworzyć nowy wykres i przekazuje zapytanie bazy danych przez wywołanie metody na wykresie `DataBindTable` metody. Ta metoda przyjmuje dwa parametry: `dataSource` parametr jest danych z zapytania i `xField` parametr umożliwia określenie, która kolumna danych jest używana dla osi x wykresu.

    Alternatywą wobec przy użyciu `DataBindTable` metody, można użyć `AddSeries` metody `Chart` pomocnika. `AddSeries` Metoda pozwala ustawić `xValue` i `yValues` parametrów. Na przykład zamiast `DataBindTable` metody następująco:

    [!code-css[Main](7-displaying-data-in-a-chart/samples/sample3.css)]

    Można użyć `AddSeries` metody następująco:

    [!code-html[Main](7-displaying-data-in-a-chart/samples/sample4.html)]

    Zarówno renderowania takie same wyniki. `AddSeries` Metoda jest bardziej elastyczne, ponieważ można określić typ wykresu i dane więcej jawnie, ale `DataBindTable` metoda jest łatwiejsze do użycia, jeśli nie jest potrzebny dodatkowy elastyczność.
5. Uruchom strony w przeglądarce. 

    ![](7-displaying-data-in-a-chart/_static/image9.jpg)

### <a name="using-xml-data"></a>Przy użyciu danych XML

Trzecia opcja dla wykresów jest użyć pliku XML jako danych wykresu. Plik XML również mieć plik schematu (*XSD* pliku), który opisuje struktury XML. Ta procedura przedstawia sposób odczytywania danych z pliku XML.

1. W *aplikacji\_danych* folderu, Utwórz nowy plik XML o nazwie *data.xml*.
2. Zastąp istniejący plik XML następujące dane, które są niektóre dane XML o pracownikach w fikcyjnej firmy. 

    [!code-xml[Main](7-displaying-data-in-a-chart/samples/sample5.xml)]
3. W *aplikacji\_danych* folderu, Utwórz nowy plik XML o nazwie *data.xsd*. (Należy pamiętać, że rozszerzenie jest teraz *XSD*.)
4. Zastąp istniejący plik XML następujące czynności: 

    [!code-xml[Main](7-displaying-data-in-a-chart/samples/sample6.xml)]
5. W katalogu głównym witryny sieci Web, Utwórz nowy plik o nazwie *ChartDataXML.cshtml*.
6. Zastąp istniejącą zawartość następujących czynności: 

    [!code-cshtml[Main](7-displaying-data-in-a-chart/samples/sample7.cshtml)]

    Najpierw tworzy kod `DataSet` obiektu. Ten obiekt jest używany do zarządzania danymi, które zostanie odczytany z pliku XML i organizowanie go zgodnie z informacjami w pliku schematu. (Powiadomienie, że na początku kodu zawiera instrukcję `using SystemData`. Jest to wymagane, aby można było pracować z `DataSet` obiektu. Aby uzyskać więcej informacji, zobacz [ &quot;Using&quot; instrukcje i w pełni kwalifikowane nazwy](#SB_UsingStatements) dalszej części tego artykułu.)

    Następnie kod tworzy `DataView` obiektu oparte na zestawie danych. Widok danych zawiera obiekt, który można powiązać z wykresu &#8212; oznacza to, Odczyt i kreślenia. Wiąże wykres danych przy użyciu `AddSeries` metody, jako użytkownik był wyświetlany podczas wykresów tablicy danych, z wyjątkiem teraz `xValue` i `yValues` parametry są ustawione na `DataView` obiektu.

    Ten przykład przedstawia również sposób określania typu konkretnego. Gdy dane są dodawane w `AddSeries` metody `chartType` również ustawiona jest wyświetlania wykresu kołowego.
7. Uruchom strony w przeglądarce. 

    ![](7-displaying-data-in-a-chart/_static/image10.jpg)

> [!TIP]
> 
> <a id="SB_UsingStatements"></a>
> ### <a name="using-statements-and-fully-qualified-names"></a>Instrukcje "Using" i w pełni kwalifikowane nazwy
> 
> .NET Framework, na podstawie stron ASP.NET Web Pages o składni Razor składa się z wielu tysięcy składników (klasy). Aby umożliwić łatwe w zarządzaniu do pracy z tych klas, są podzielone na *przestrzeni nazw*, które przypominają do biblioteki. Na przykład `System.Web` przestrzeń nazw zawiera klasy obsługujące komunikacji z serwerem przeglądarki `System.Xml` przestrzeń nazw zawiera klasy służące do tworzenia i odczytywać pliki XML i `System.Data` przestrzeń nazw zawiera klasy, które pozwalają pracować z danymi.
> 
> Aby uzyskać dostęp do danej klasy w programie .NET Framework, kod musi wiedzieć, nie tylko nazwę klasy, ale klasa znajduje się w obszarze nazw. Na przykład, aby można było używać `Chart` pomocnika, kod musi znaleźć `System.Web.Helpers.Chart` klasy, która łączy obszaru nazw (`System.Web.Helpers`) z nazwą klasy (`Chart`). Jest to nazywane klasy *pełną* nazwa &#8212; pełną, jednoznacznej lokalizacji w ramach vastness programu .NET Framework. W kodzie to będzie wyglądać następująco:
> 
> `var myChart = new System.Web.Helpers.Chart(width: 600, height: 400) // etc.`
> 
> Jednak jest skomplikowane (i podatna) do użycia tych długie, w pełni kwalifikowanej nazwy za każdym razem, gdy chcesz odwołuje się do klasy lub pomocnika. W związku z tym, aby ułatwić nazwy klas, możesz *zaimportować* przestrzenie nazw są zainteresowani, zazwyczaj jest to jest tylko kilka spośród wiele przestrzeni nazw w programie .NET Framework. Jeśli zaimportowane przestrzeni nazw, można użyć tylko nazwę klasy (`Chart`) zamiast w pełni kwalifikowana nazwa (`System.Web.Helpers.Chart`). Gdy kod napotka nazwy klasy, mogą zobaczyć w właśnie przestrzeniach nazw zaimportowany do znalezienia tej klasy.
> 
> Użycie składnika ASP.NET Web Pages o składni Razor do tworzenia stron sieci web, zwykle korzysta ten sam zestaw klas, w tym `WebPage` klasy, różnych pomocników i tak dalej. Aby zapisać pracy importowania odpowiednie przestrzenie nazw w każdym utworzeniu witryny sieci Web, ASP.NET jest skonfigurowany tak automatycznie importuje zestaw przestrzeni nazw core dla każdej witryny sieci Web. Dlatego nie mam radzenia sobie z przestrzeni nazw lub importowanie do chwili; wszystkie klasy, z którymi się znajdują się w przestrzeni nazw, które już są importowane automatycznie.
> 
> Czasami potrzebny pracować z klasy, która nie znajduje się w przestrzeni nazw, która jest automatycznie importowane automatycznie. W takim przypadku możesz użyć tej klasy, w pełni kwalifikowaną nazwę lub przestrzeń nazw zawiera klasy można ręcznie zaimportować. Aby zaimportować przestrzeni nazw, należy użyć `using` instrukcji (`import` w języku Visual Basic), jak przedstawiono w przykładzie wcześniej tego artykułu.
> 
> Na przykład `DataSet` klasa znajduje się w `System.Data` przestrzeni nazw. `System.Data` Przestrzeni nazw nie jest automatycznie dostępny dla stron ASP.NET Razor. W związku z tym aby pracować z `DataSet` przy użyciu w pełni kwalifikowanej nazwy, można użyć kodu następująco:
> 
> `var dataSet = new System.Data.DataSet();`
> 
> Jeśli użytkownik musi używać `DataSet` wielokrotnie klasy mogą być importowane przestrzeni nazw, jak i następnie użyć tylko nazwę klasy w kodzie:
> 
> [!code-cshtml[Main](7-displaying-data-in-a-chart/samples/sample8.cshtml)]
> 
> Możesz dodać `using` instrukcji dla innych nazw .NET Framework, która ma dotyczyć odwołanie. Jak wspomniano, nie będzie potrzebny, w tym celu często, ponieważ większość klas, które będzie działać z znajdują się w przestrzeni nazw, które są automatycznie importowane przez program ASP.NET do użycia w *.cshtml* i *.vbhtml* stron.


<a id="Displaying_Charts"></a>
## <a name="displaying-charts-inside-a-web-page"></a>Wyświetlanie wykresów na stronie sieci Web

W przykładach przedstawiono dotychczas tworzenie wykresu, a następnie renderowania wykresu bezpośrednio w przeglądarce jako grafiki. W wielu przypadkach jednak chcesz wyświetlić wykres jako części strony, nie tylko przez samego siebie w przeglądarce. W tym celu wymaga dwuetapowego procesu. Pierwszym krokiem jest do utworzenia strony, która generuje wykresu, ponieważ już w tym samouczku.

Drugim krokiem jest Wynikowy obraz był wyświetlany na innej stronie. Aby wyświetlić obraz, należy użyć kodu HTML `<img>` elementu w taki sam sposób, w jaki można wyświetlić żadnego obrazu. Jednak zamiast odwołania do *.jpg* lub *.png* pliku `<img>` odwołania do elementu *.cshtml* plik zawierający `Chart` pomocnika który Tworzy wykres. Po uruchomieniu strony wyświetlania, `<img>` element pobiera dane wyjściowe `Chart` pomocnika i renderowania wykresu.

![](7-displaying-data-in-a-chart/_static/image11.jpg)

1. Utwórz plik o nazwie *ShowChart.cshtml*.
2. Zastąp istniejącą zawartość następujących czynności: 

    [!code-html[Main](7-displaying-data-in-a-chart/samples/sample9.html)]

    W kodzie użyto `<img>` element, aby wyświetlić wykresu, który został wcześniej utworzony w *ChartArrayBasic.cshtml* pliku.
3. Uruchom strony sieci web w przeglądarce. *ShowChart.cshtml* plik zawiera obraz wykresu, na podstawie kodu zawarte w *ChartArrayBasic.cshtml* pliku.

<a id="Styling_a_Chart"></a>
## <a name="styling-a-chart"></a>Ustawianie stylów na wykresie

`Chart` Pomocnika obsługuje wiele opcji, które umożliwiają dostosowanie wyglądu wykresu. Można ustawić kolory, czcionki, obramowania i tak dalej. Prosty sposób na dostosowanie wyglądu wykresu jest użycie *motyw*. Motywy to kolekcje informacji określających sposób renderowania wykresu przy użyciu czcionek, kolorów, etykiet, palet, obramowania i efekty. (Należy pamiętać, że styl wykresu nie wskazuje typ wykresu).

W poniższej tabeli wymieniono motywów wbudowanych.

| Motyw | Opis |
| --- | --- |
| `Vanilla` | Wyświetla red kolumny na białe tło. |
| `Blue` | Wyświetla niebieskie kolumn na niebieskim gradientu tła. |
| `Green` | Wyświetla niebieskie kolumn na zielonym gradientu tła. |
| `Yellow` | Wyświetla pomarańczowy kolumny na żółty gradientu tła. |
| `Vanilla3D` | Wyświetla 3-czerwony kolumny na białe tło. |

Można określić motyw do użycia podczas tworzenia nowego wykresu.

1. Utwórz nowy plik o nazwie *ChartStyleGreen.cshtml*.
2. Zastąp istniejącą zawartość na stronie następujące czynności:

    [!code-cshtml[Main](7-displaying-data-in-a-chart/samples/sample10.cshtml)]

    Ten kod jest taka sama jak poprzedniego przykładu, który używa bazy danych, ale nie dodaje `theme` parametru podczas tworzenia `Chart` obiektu. Poniżej przedstawiono zmienionego kodu:

    [!code-csharp[Main](7-displaying-data-in-a-chart/samples/sample11.cs)]
3. Uruchom strony w przeglądarce. Możesz zobaczyć te same dane jako przed, ale bardziej profesjonalny wygląd wykresu: 

    ![](7-displaying-data-in-a-chart/_static/image12.jpg)

<a id="Saving_a_Chart"></a>
## <a name="saving-a-chart"></a>Zapisywanie wykresu

Jeśli używasz `Chart` pomocnika jako użytkownik w tym samouczku wykonanej do tej pory w tym artykule pomocnika ponownie tworzy wykres od początku każdej jest wywoływana. W razie potrzeby kod wykresu również ponownie wysyła zapytanie do bazy danych lub ponownie odczytuje plik XML można pobrać danych. W niektórych przypadkach w ten sposób można złożonych operacji, takich jak bazy danych, która jest wykonywana kwerenda jest duży, lub plik XML zawiera dużą ilość danych. Nawet jeśli wykresu nie zawiera dużą ilość danych, proces dynamicznego tworzenia obrazu zajmuje zasobów serwera, a jeśli wiele osób żądanie strony, które wyświetlić wykresu, może istnieć wpływ na wydajność witryny sieci Web.

Pozwala zmniejszyć potencjalne obniżenie wydajności podczas tworzenia wykresu, można utworzyć wykres pierwszy raz potrzebny, a następnie zapisz go. Wykres jest ponownie potrzebny, zamiast ponownego generowania, możesz po prostu pobrać zapisaną wersję i renderować.

Wykres można zapisać w następujący sposób:

- Wykres w pamięci komputera (na serwerze) w pamięci podręcznej.
- Wykres można zapisać jako plik obrazu.
- Wykres można zapisać jako plik XML. Ta opcja pozwala zmodyfikować wykres przed jej zapisaniem.

### <a name="caching-a-chart"></a>Buforowanie wykresu

Po utworzeniu wykresu można buforować. Buforowanie wykresu oznacza, że nie ma zostać utworzony ponownie, jeśli muszą być ponownie wyświetlone. Podczas zapisywania wykresu w pamięci podręcznej, należy nadać mu klucz, który musi być unikatowa dla tego wykresu.

Wykresy zapisane w pamięci podręcznej mogą zostać usunięte, jeśli serwer ma za mało pamięci. Ponadto pamięci podręcznej jest wyczyszczone, jeśli aplikacja zostanie uruchomiony ponownie w dowolnej przyczyny. W związku z tym standardowy sposób pracy z pamięci podręcznej wykresu jest zawsze najpierw sprawdź, czy jest dostępna w pamięci podręcznej, a jeśli nie, a następnie do tworzenia lub utwórz go ponownie.

1. W katalogu głównym witryny sieci Web, Utwórz plik o nazwie *ShowCachedChart.cshtml*.
2. Zastąp istniejącą zawartość następujących czynności: 

    [!code-html[Main](7-displaying-data-in-a-chart/samples/sample12.html)]

    `<img>` Zawiera tag `src` atrybut, który wskazuje *ChartSaveToCache.cshtml* i przekazuje klucz do strony jako ciąg zapytania. Klucz zawiera wartość &quot;myChartKey&quot;. *ChartSaveToCache.cshtml* plik zawiera `Chart` pomocnika, który tworzy wykres. Za chwilę zostaną utworzone tej strony.

    Na końcu strony jest łączem do strony o nazwie *ClearCache.cshtml*. To strona, która zostanie również utworzony wkrótce. Należy *ClearCache.cshtml* tylko do testowania buforowania w tym przykładzie — nie jest łącze lub strona, która zwykle można dołączyć podczas pracy z pamięci podręcznej schematów.
3. W katalogu głównym witryny sieci Web, Utwórz nowy plik o nazwie *ChartSaveToCache.cshtml*.
4. Zastąp istniejącą zawartość następujących czynności:

    [!code-cshtml[Main](7-displaying-data-in-a-chart/samples/sample13.cshtml)]

    Ten kod najpierw sprawdza, czy przekazano niczego jako wartość klucza w ciągu zapytania. Jeśli tak, kod próbuje odczytać wykres z pamięci podręcznej przez wywołanie metody `GetFromCache` — metoda i przekazanie jej klucza. Jeśli okaże się, że nie ma w pamięci podręcznej, w tym kluczu (co się stanie po raz pierwszy zażądano wykresu), kod tworzy wykres w zwykły sposób. Po zakończeniu operacji wykresu kod zapisuje go w pamięci podręcznej przez wywołanie metody `SaveToCache`. Ta metoda wymaga klucza (Aby później można żądać wykresu) i ilość czasu, który ma zostać zapisany wykresu w pamięci podręcznej. (Czas będzie pamięci podręcznej wykresu będzie zależeć od częstotliwość traktować danych, który reprezentuje może zmienić). `SaveToCache` Wymaga również metody `slidingExpiration` parametru &#8212; Jeśli ta opcja jest ustawiona na wartość true, limit czasu licznik jest resetowany zawsze wykresu jest dostępny. W takim przypadku obowiązują oznacza to, że wpis pamięci podręcznej wykresu wygaśnie 2 minut od czasu ostatniego ktoś uzyskać dostęp do wykresu. (Zamiast przedłużanie ważności jest wygaśnięcia bezwzględne, co oznacza, że wpis pamięci podręcznej upływa dokładnie 2 minuty po został umieszczony w pamięci podręcznej, niezależnie od tego, jak często ma uzyskano).

    Ponadto w kodzie użyto `WriteFromCache` metodę, aby pobrać i renderować wykres z pamięci podręcznej. Należy pamiętać, że ta metoda jest poza `if` bloku, który sprawdza pamięci podręcznej, ponieważ pobierze wykres z pamięci podręcznej wykresu były rozpoczynać się od znaku lub były generowane i zapisać w pamięci podręcznej.

    Zwróć uwagę, że w tym przykładzie `AddTitle` metoda zawiera sygnaturę czasową. (Dodaje bieżącą datę i godzinę &#8212; `DateTime.Now` &#8212; do tytułu.)
5. Utwórz nową stronę o nazwie *ClearCache.cshtml* i zastąp jego zawartość następującym kodem:

    [!code-cshtml[Main](7-displaying-data-in-a-chart/samples/sample14.cshtml)]

    Ta strona używa `WebCache` pomocnika wykresu, który jest buforowana w *ChartSaveToCache.cshtml*. Jak wspomniano wcześniej, zwykle nie muszą mieć strony następująco. Tworzysz tu tylko po to, aby ułatwić testowanie buforowania.
6. Uruchom *ShowCachedChart.cshtml* strony sieci web w przeglądarce. Na stronie są wyświetlane obraz wykresu, na podstawie kodu zawarte w *ChartSaveToCache.cshtml* pliku. Zanotuj sygnaturę czasową mówi tytuł wykresu. 

    ![Opis: Obraz podstawowy wykres z sygnaturą czasową w tytuł wykresu](7-displaying-data-in-a-chart/_static/image13.jpg)
7. Zamknij przeglądarkę.
8. Uruchom *ShowCachedChart.cshtml* ponownie. Zwróć uwagę, że sygnatura czasowa jest taka sama jak poprzednio, co oznacza, że nie wygenerowano wykresu, ale zamiast tego została odczytana z pamięci podręcznej.
9. W *ShowCachedChart.cshtml*, kliknij przycisk **wyczyścić pamięć podręczną** łącza. Powoduje to przejście do *ClearCache.cshtml*, które raporty, że pamięć podręczna została wyczyszczona.
10. Kliknij przycisk **powrócić do ShowCachedChart.cshtml** łącze lub uruchom ponownie *ShowCachedChart.cshtml* z programu WebMatrix. Należy zauważyć, że teraz znacznik czasu został zmieniony, ponieważ pamięć podręczna została wyczyszczona. W związku z tym kod musiał ponownie wygenerować wykresu i umieść je z powrotem do pamięci podręcznej.

### <a name="saving-a-chart-as-an-image-file"></a>Zapisywanie jako plik obrazu wykresu

Wykres można także zapisać jako plik obrazu (na przykład jako *.jpg* pliku) na serwerze. Można następnie użyć pliku obrazu sposób, w jaki żadnego obrazu. Zaletą jest to plik jest przechowywany zamiast zapisane do tymczasowego pamięci podręcznej. Można zapisać nowy obraz wykresu o różnych porach (na przykład co godzinę) i następnie rejestrować je trwałych zmian, które występują w czasie. Należy pamiętać, że należy upewnić się, że aplikację sieci web ma uprawnienia do zapisania pliku do folderu na serwerze, na którym ma zostać umieszczony plik obrazu.

1. W katalogu głównym witryny sieci Web, Utwórz folder o nazwie  *\_ChartFiles* Jeśli jeszcze nie istnieje.
2. W katalogu głównym witryny sieci Web, Utwórz nowy plik o nazwie *ChartSave.cshtml*.
3. Zastąp istniejącą zawartość następujących czynności:

    [!code-cshtml[Main](7-displaying-data-in-a-chart/samples/sample15.cshtml)]

    Kod najpierw sprawdza, czy *.jpg* plik istnieje, wywołując `File.Exists` metody. Jeśli plik nie istnieje, kod tworzy nową `Chart` z tablicy. Teraz, kod wywołuje `Save` — metoda i przekazuje `path` parametr, aby określić ścieżkę i nazwę pliku z lokalizacji zapisu na wykresie. W treści strony `<img>` elementu użyto ścieżki, aby wskazywał *.jpg* plików do wyświetlenia.
4. Uruchom *ChartSave.cshtml* pliku.
5. Wróć do programu WebMatrix. Należy zauważyć, że plik obrazu o nazwie *chart01.jpg* został zapisany w  *\_ChartFiles* folderu.

### <a name="saving-a-chart-as-an-xml-file"></a>Zapisywanie wykresu jako pliku XML

Na koniec wykres można zapisać jako plik XML na serwerze. Zaletą tej metody za pośrednictwem buforowanie wykresu lub zapisywania wykresu do pliku jest, czy można zmodyfikować pliku XML przed wyświetleniem wykresu, jeśli chcesz. Aplikacja musi mieć uprawnienia odczytu/zapisu do folderu na serwerze, w którym ma zostać umieszczony plik obrazu.

1. W katalogu głównym witryny sieci Web, Utwórz nowy plik o nazwie *ChartSaveXml.cshtml*.
2. Zastąp istniejącą zawartość następujących czynności:

    [!code-cshtml[Main](7-displaying-data-in-a-chart/samples/sample16.cshtml)]

    Ten kod jest podobny do kodu, który był wyświetlany poprzednio do zapisywania wykresu w pamięci podręcznej, z wyjątkiem tego, że używa pliku XML. Kod najpierw sprawdza, czy plik XML istnieje przez wywołanie metody `File.Exists` metody. Jeśli plik istnieje, kod tworzy nową `Chart` obiektu i przekazuje nazwę pliku jako `themePath` parametru. Spowoduje to utworzenie wykresu oparte na dowolnym znajduje się w pliku XML. Jeśli plik XML nie istnieje, tworzy wykres, takich jak normalne kod, a następnie wywołuje `SaveXml` go zapisać. Wykres jest renderowany przy użyciu `Write` metody, jak przedstawiono przed.

    Podobnie jak w przypadku strony, którą wykazało buforowania, ten kod zawiera sygnaturę czasową w tytuł wykresu.
3. Utwórz nową stronę o nazwie *ChartDisplayXMLChart.cshtml* i Dodaj do niej następujący kod: 

    [!code-html[Main](7-displaying-data-in-a-chart/samples/sample17.html)]
4. Uruchom *ChartDisplayXMLChart.cshtml* strony. Wykres zostanie wyświetlony. Zanotuj sygnaturę czasową tytuł wykresu.
5. Zamknij przeglądarkę.
6. W programie WebMatrix, kliknij prawym przyciskiem myszy  *\_ChartFiles* folderu, kliknij przycisk **Odśwież**, a następnie otwórz folder. *XMLChart.xml* plik w tym folderze został utworzony przez `Chart` pomocnika. 

    ![Opis: Folder _ChartFiles przedstawiający XMLChart.xml plik utworzony przez pomocnika wykresu.](7-displaying-data-in-a-chart/_static/image14.jpg)
7. Uruchom *ChartDisplayXMLChart.cshtml* strony ponownie. Wykres przedstawia tą samą sygnaturą czasową jako podczas pierwszego uruchomienia strony. To ponieważ wykresu są generowane z pliku XML został wcześniej zapisany.
8. W programie WebMatrix, otwórz  *\_ChartFiles* folder i Usuń *XMLChart.xml* pliku.
9. Uruchom *ChartDisplayXMLChart.cshtml* strony ponownie. Teraz, zaktualizować sygnaturę czasową, ponieważ `Chart` pomocnika musiał ponownie utworzyć plik XML. Jeśli chcesz, sprawdź  *\_ChartFiles* folderu i zwróć uwagę, że plik XML została przywrócona.

<a id="Additional_Resources"></a>
## <a name="additional-resources"></a>Dodatkowe zasoby

- [Wprowadzenie do pracy z bazą danych w sieci Web ASP.NET stron witryny](https://go.microsoft.com/fwlink/?LinkId=202893)
- [Przy użyciu pamięci podręcznej w sieci Web ASP.NET strony lokacjach w celu zwiększenia wydajności](https://go.microsoft.com/fwlink/?LinkId=202903)
- [Klasa wykresu](https://msdn.microsoft.com/library/system.web.helpers.chart(v=vs.99)) (odwołanie do API stron sieci Web ASP.NET w witrynie MSDN)
