---
uid: web-forms/overview/data-access/basic-reporting/programmatically-setting-the-objectdatasource-s-parameter-values-vb
title: Programowe Ustawianie wartości parametrów elementu ObjectDataSource (VB) | Dokumentacja firmy Microsoft
author: rick-anderson
description: W tym samouczku Zapoznamy się dodanie metody do naszych DAL i LOGIKI, która przyjmuje jeden parametr wejściowy i zwraca dane. Ten parametr zostanie ustawiony przykład...
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/31/2010
ms.topic: article
ms.assetid: 0ecb03b6-52a0-4731-8c7a-436391d36838
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/data-access/basic-reporting/programmatically-setting-the-objectdatasource-s-parameter-values-vb
msc.type: authoredcontent
ms.openlocfilehash: e2d0afa6616d936d2c8a2c76ca51ee1995040644
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: HT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/03/2018
ms.locfileid: "37396271"
---
<a name="programmatically-setting-the-objectdatasources-parameter-values-vb"></a>Programowe Ustawianie wartości parametrów elementu ObjectDataSource (VB)
====================
przez [Bento Scott](https://twitter.com/ScottOnWriting)

[Pobierz przykładową aplikację](http://download.microsoft.com/download/5/d/7/5d7571fc-d0b7-4798-ad4a-c976c02363ce/ASPNET_Data_Tutorial_6_VB.exe) lub [Pobierz plik PDF](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/datatutorial06vb1.pdf)

> W tym samouczku Zapoznamy się dodanie metody do naszych DAL i LOGIKI, która przyjmuje jeden parametr wejściowy i zwraca dane. Przykład będzie programowo ustawić ten parametr.


## <a name="introduction"></a>Wprowadzenie

Jak widzieliśmy w [poprzedniego samouczka](declarative-parameters-vb.md), dla deklaratywne przekazywania wartości parametrów do metod ObjectDataSource dostępnych kilka opcji. Jeśli wartość tego parametru jest ustalony, pochodzi z formantu sieci Web, na stronie lub znajduje się w innych źródeł, który odczyta przez źródło danych `Parameter` obiektu, na przykład, że wartość może być powiązana z parametru wejściowego, bez konieczności pisania nawet wiersza kodu.

Może to być sytuacji, gdy wartość parametru pochodzi z niektóre źródła nie jest już uwzględniony przez jedno źródło danych wbudowane `Parameter` obiektów. Jeśli naszą witrynę obsługiwane konta użytkowników mogą chcemy ustawić parametr oparte na aktualnie zalogowanego użytkownika identyfikatora użytkownika. Lub firma Microsoft może być konieczne dostosowanie wartość parametru przed wysłaniem ich wzdłuż do obiektu źródłowego kontrolki ObjectDataSource metody.

Zawsze, gdy ObjectDataSource `Select` wywoływana jest metoda kontrolki ObjectDataSource najpierw wywołuje jego [wybranie event](https://msdn.microsoft.com/library/system.web.ui.webcontrols.objectdatasource.selecting%28VS.80%29.aspx). Następnie wywoływana jest metoda ObjectDataSource obiektu źródłowego. Gdy służący do ukończenia ObjectDataSource [wybrane zdarzenia](https://msdn.microsoft.com/library/system.web.ui.webcontrols.objectdatasource.selected%28VS.80%29.aspx) uruchamiany (rysunek 1 przedstawia następująca sekwencja zdarzeń). Wartości parametrów przekazywany do metody obiektu źródłowego kontrolki ObjectDataSource można ustawić lub dostosować w obsłudze zdarzeń dla `Selecting` zdarzeń.


[![Wybrane ObjectDataSource i wybierając Fire zdarzenia przed i po jego podstawowego obiektu — metoda jest wywoływana.](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image2.png)](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image1.png)

**Rysunek 1**: ObjectDataSource `Selected` i `Selecting` wywoływana jest metoda zdarzenia Fire przed i po jego podstawowego obiektu ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image3.png))


W tym samouczku Zapoznamy się dodanie metody do naszych DAL i LOGIKI, która przyjmuje jeden parametr wejściowy `Month`, typu `Integer` i zwraca `EmployeesDataTable` obiektu wypełniane przy użyciu tych pracowników, które mają ich rocznicy zatrudniania w określonym `Month`. Nasz przykład ustawi tego parametru, programowo na podstawie bieżącego miesiąca pokazanie listy "Rocznice pracowników, w tym miesiącu."

Zaczynajmy!

## <a name="step-1-adding-a-method-toemployeestableadapter"></a>Krok 1: Dodawanie metody do`EmployeesTableAdapter`

W tym przykładzie pierwsze musimy dodać oznacza, że można pobrać pracowników, których `HireDate` wystąpił w określonym miesiącu. Do tej funkcji, zgodnie z naszej architektury, musimy najpierw utworzyć metody w `EmployeesTableAdapter` która jest mapowana do właściwego instrukcji SQL. W tym celu należy uruchomić, otwierając zestaw Northwind wpisanych danych. Kliknij prawym przyciskiem myszy `EmployeesTableAdapter` etykiety, a następnie wybierz polecenie Dodaj zapytanie.


[![Dodaj nowe zapytanie do EmployeesTableAdapter](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image5.png)](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image4.png)

**Rysunek 2**: Dodaj nowe zapytanie w celu `EmployeesTableAdapter` ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image6.png))


Wybierz dodać instrukcję SQL, która zwraca wiersze. Po przejściu określ `SELECT` instrukcji ekran domyślny `SELECT` poufności informacji dotyczące `EmployeesTableAdapter` już zostanie załadowany. Po prostu Dodaj w `WHERE` klauzuli: `WHERE DATEPART(m, HireDate) = @Month`. [DATEPART](https://msdn.microsoft.com/library/ms174420.aspx) jest funkcją języka T-SQL, która zwraca określoną datę część `datetime` typu; w tym przypadku używamy `DATEPART` do zwrócenia w miesiącu `HireDate` kolumny.


[![Zwracany tylko te wiersze gdzie rekrutacji kolumna jest mniejsza niż lub równa @HiredBeforeDate parametru](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image8.png)](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image7.png)

**Rysunek 3**: zwracać tylko te wiersze, dla których `HireDate` kolumna jest mniejsza niż lub równa `@HiredBeforeDate` parametru ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image9.png))


Na koniec zmień `FillBy` i `GetDataBy` nazwy metody `FillByHiredDateMonth` i `GetEmployeesByHiredDateMonth`, odpowiednio.


[![Wybierz bardziej odpowiednie nazwy metod niż FillBy i GetDataBy](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image11.png)](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image10.png)

**Rysunek 4**: Wybierz bardziej odpowiednie metody nazw niż `FillBy` i `GetDataBy` ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image12.png))


Kliknij przycisk Zakończ, aby zakończyć pracę kreatora i powrócić do powierzchni projektowej typizowanego zestawu danych. `EmployeesTableAdapter` Powinny znajdować się teraz nowy zestaw metod dostępu do pracowników zatrudnionych w określonym miesiącu.


[![Nowe metody są wyświetlane w powierzchni projektowej typizowanego zestawu danych](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image14.png)](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image13.png)

**Rysunek 5**: nowe metody są wyświetlane w powierzchni projektowej typizowanego zestawu danych ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image15.png))


## <a name="step-2-adding-thegetemployeesbyhireddatemonthmonthmethod-to-the-business-logic-layer"></a>Krok 2. Dodawanie`GetEmployeesByHiredDateMonth(month)`metody warstwy logiki biznesowej

Ponieważ naszej aplikacji architektura używa oddzielnego warstwy logiki biznesowej i danych uzyskać dostęp do logiki, musimy dodać metodę do naszych LOGIKI, która wywołania do warstwy DAL do pobrania pracowników zatrudnionych przed upływem określonego terminu. Otwórz `EmployeesBLL.vb` pliku i dodaj następującą metodę:


[!code-vb[Main](programmatically-setting-the-objectdatasource-s-parameter-values-vb/samples/sample1.vb)]

Podobnie jak w przypadku naszych innych metod, w tej klasie `GetEmployeesByHiredDateMonth(month)` po prostu wywołuje widok w dół do warstwy DAL i zwraca wyniki.

## <a name="step-3-displaying-employees-whose-hiring-anniversary-is-this-month"></a>Krok 3: Wyświetlanie pracowników, których rocznicy zatrudniania w tym miesiącu

Ostatnią czynnością, w tym przykładzie jest do wyświetlenia tych pracowników, których rocznicy zatrudniania w tym miesiącu. Rozpocznij od dodania GridView do `ProgrammaticParams.aspx` stronie `BasicReporting` folder i dodać nowe kontrolki ObjectDataSource jako źródło danych. Konfigurowanie kontrolki ObjectDataSource używać `EmployeesBLL` klasy `SelectMethod` równa `GetEmployeesByHiredDateMonth(month)`.


[![Korzystanie z klasy EmployeesBLL](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image17.png)](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image16.png)

**Rysunek 6**: Użyj `EmployeesBLL` klasy ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image18.png))


[![Wybierz z GetEmployeesByHiredDateMonth(month) — metoda](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image20.png)](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image19.png)

**Rysunek 7**: Wybierz z `GetEmployeesByHiredDateMonth(month)` — metoda ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image21.png))


Ekran końcowy prosi nam zapewnić `month` źródła wartości parametru. Ponieważ firma Microsoft będzie programowo ustawić tę wartość, pozostaw źródło parametru ustawionej na wartość domyślna Brak opcję i kliknij przycisk Zakończ.


[![Pozostaw Parametr źródłowy zestaw None](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image23.png)](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image22.png)

**Rysunek 8**: pozostaw Parametr źródłowy zestaw None ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image24.png))


Spowoduje to utworzenie `Parameter` obiektu w ObjectDataSource `SelectParameters` kolekcji, które nie mają określonej wartości.


[!code-aspx[Main](programmatically-setting-the-objectdatasource-s-parameter-values-vb/samples/sample2.aspx)]

Aby programowo ustawić tę wartość, należy utworzyć procedurę obsługi zdarzeń dla elementu ObjectDataSource `Selecting` zdarzeń. Aby to zrobić, przejdź do widoku projektu, a następnie kliknij dwukrotnie ikonę kontrolki ObjectDataSource. Alternatywnie zaznacz kontrolki ObjectDataSource, przejdź do okna właściwości i kliknij ikonę pioruna. Następnie albo kliknij dwukrotnie pole tekstowe obok `Selecting` zdarzenia lub wpisz nazwę programu obsługi zdarzeń, którego chcesz użyć. Trzecią możliwością, można utworzyć programu obsługi zdarzeń, wybierając kontrolki ObjectDataSource i jego `Selecting` zdarzenie z dwóch list rozwijanych w górnej części strony osobna klasa kodu.


![Kliknij ikonę pioruna w oknie dialogowym właściwości, aby wyświetlić listę zdarzeń formantu sieci Web](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image25.png)

**Rysunek 9**: kliknij ikonę pioruna w oknie dialogowym właściwości, aby wyświetlić listę zdarzeń formantu sieci Web


Wszystkie trzy metody dodać nowy program obsługi zdarzeń dla ObjectDataSource `Selecting` zdarzeń do klasy CodeBehind strony. Ta procedura obsługi zdarzeń firma Microsoft odczytu i zapisu wartości parametru za pomocą `e.InputParameters(parameterName)`, gdzie *`parameterName`* jest wartością `Name` atrybutu w `<asp:Parameter>` tag ( `InputParameters` kolekcji może być również indeksowane ordinally, podobnie jak w `e.InputParameters(index)`). Aby ustawić `month` parametr do bieżącego miesiąca, Dodaj następujący kod do `Selecting` program obsługi zdarzeń:


[!code-vb[Main](programmatically-setting-the-objectdatasource-s-parameter-values-vb/samples/sample3.vb)]

Gdy użytkownik odwiedzi tę stronę za pośrednictwem przeglądarki widać tylko jednego pracownika został zatrudniony (marca) w tym miesiącu Kowalski Laura, kto pracuje w firmie od 1994 r.


[![Pracowników, u którego Rocznice są wyświetlane w tym miesiącu](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image27.png)](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image26.png)

**Na rysunku nr 10**: tych pracowników Whose rocznice ten miesiąc są wyświetlane ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image28.png))


## <a name="summary"></a>Podsumowanie

Podczas gdy wartości parametrów elementu ObjectDataSource można zwykle ustawić deklaratywne, bez konieczności wiersz kodu, jest łatwe programowe Ustawianie wartości parametrów. Musimy wystarczy utworzyć program obsługi zdarzeń dla elementu ObjectDataSource `Selecting` zdarzenie, które jest uruchamiany przed wywołana metoda obiektu źródłowego i ręcznie ustawić wartości dla jednego lub więcej parametrów za pomocą `InputParameters` kolekcji.

W tym samouczku kończy się w sekcji podstawowe raportowanie. [Następnego samouczka](../masterdetail/master-detail-filtering-with-a-dropdownlist-vb.md) dotyczącego sekcji Filtrowanie i głównych scenariuszy, w którym utworzymy Przyjrzyj się techniki umożliwiające obiektu odwiedzającego, aby filtrować dane i przejść do szczegółów z głównego raportu do raportu szczegółowe informacje.

Wszystkiego najlepszego programowania!

## <a name="about-the-author"></a>Informacje o autorze

[Scott Bento](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor siedem ASP/ASP.NET książek i założycielem [4GuysFromRolla.com](http://www.4guysfromrolla.com), pracował nad przy użyciu technologii Microsoft Web od 1998 r. Scott działa jako niezależny Konsultant, trainer i składnika zapisywania. Jego najnowszą książkę Stephena [ *Sams uczyć się ASP.NET 2.0 w ciągu 24 godzin*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). ADAM można z Tobą skontaktować w [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) lub za pośrednictwem jego blogu, który znajduje się w temacie [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Specjalne podziękowania dla

W tej serii samouczków został zrecenzowany przez wielu recenzentów pomocne. Weryfikacja potencjalnych klientów w ramach tego samouczka został Hilton Giesenow. Zainteresowani zapoznaniem Moje kolejnych artykułów MSDN? Jeśli tak, Porzuć mnie linii w [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Poprzednie](declarative-parameters-vb.md)
