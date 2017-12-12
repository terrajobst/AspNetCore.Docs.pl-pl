---
uid: web-forms/overview/data-access/basic-reporting/programmatically-setting-the-objectdatasource-s-parameter-values-cs
title: "Programowo Trwa ustawianie wartości parametrów elementu ObjectDataSource (C#) | Dokumentacja firmy Microsoft"
author: rick-anderson
description: "W ramach tego samouczka przyjrzymy Dodawanie metody do warstwy DAL i logiki warstwy Biznesowej, która przyjmuje jeden parametr wejściowy i zwraca dane firmy Microsoft. Ten parametr zostanie ustawiony przykładzie..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/31/2010
ms.topic: article
ms.assetid: 1c4588bb-255d-4088-b319-5208da756f4d
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/basic-reporting/programmatically-setting-the-objectdatasource-s-parameter-values-cs
msc.type: authoredcontent
ms.openlocfilehash: 7a009d57f97838feb5b4a3253c6de9a872a9e9ee
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/10/2017
---
<a name="programmatically-setting-the-objectdatasources-parameter-values-c"></a>Programowo Trwa ustawianie wartości parametrów elementu ObjectDataSource (C#)
====================
przez [Bento Scott](https://twitter.com/ScottOnWriting)

[Pobierz przykładową aplikację](http://download.microsoft.com/download/4/6/3/463cf87c-4724-4cbc-b7b5-3f866f43ba50/ASPNET_Data_Tutorial_6_CS.exe) lub [pobierania plików PDF](programmatically-setting-the-objectdatasource-s-parameter-values-cs/_static/datatutorial06cs1.pdf)

> W ramach tego samouczka przyjrzymy Dodawanie metody do warstwy DAL i logiki warstwy Biznesowej, która przyjmuje jeden parametr wejściowy i zwraca dane firmy Microsoft. Przykład ustawi programowo tego parametru.


## <a name="introduction"></a>Wprowadzenie

Jak widzieliśmy w [poprzedniego samouczek](declarative-parameters-cs.md), liczba opcji dostępnych dla deklaratywnie przekazanie wartości parametru do metody ObjectDataSource. Jeśli wartość parametru jest ustalony, pochodzi z formantu na stronie sieci Web lub w inne źródło, który jest możliwy do odczytu przez źródło danych `Parameter` obiektu, na przykład, że wartość może być powiązana z parametru wejściowego bez pisania wiersz kodu.

Może to być sytuacji, gdy wartość parametru pochodzi z niektórych źródła nie jest jeszcze uwzględnione przez jednego źródła danych wbudowanych `Parameter` obiektów. Jeśli naszej witrynie obsługiwane konta użytkowników mogą chcemy ustaw dla parametru oparte na aktualnie zalogowanego użytkownika nazwę użytkownika. Lub musimy dostosować wartość parametru przed wysłaniem wzdłuż metody ObjectDataSource obiektu źródłowego.

Zawsze, gdy element ObjectDataSource `Select` wywoływana jest metoda ObjectDataSource najpierw zgłasza jego [zdarzenia Selecting](https://msdn.microsoft.com/en-US/library/system.web.ui.webcontrols.objectdatasource.selecting%28VS.80%29.aspx). Następnie wywoływana jest metoda ObjectDataSource obiektu źródłowego. Po zakończeniu który ObjectDataSource [wybrane zdarzenie](https://msdn.microsoft.com/en-US/library/system.web.ui.webcontrols.objectdatasource.selected%28VS.80%29.aspx) uruchamiany (rysunek 1 pokazuje, jak ta sekwencja zdarzeń). Wartości parametrów przekazane do metody ObjectDataSource podstawowego obiektu można ustawić lub dostosować w obsłudze zdarzeń dla `Selecting` zdarzeń.


[![Jest wywoływany przez element ObjectDataSource wybrane i wybierając Fire zdarzenia przed i po jego podstawowego obiektu — metoda](programmatically-setting-the-objectdatasource-s-parameter-values-cs/_static/image2.png)](programmatically-setting-the-objectdatasource-s-parameter-values-cs/_static/image1.png)

**Rysunek 1**: element ObjectDataSource `Selected` i `Selecting` wywoływana jest metoda Fire zdarzenia przed i po jego podstawowego obiektu ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](programmatically-setting-the-objectdatasource-s-parameter-values-cs/_static/image3.png))


W ramach tego samouczka przyjrzymy Dodawanie metody do naszych DAL i logiki warstwy Biznesowej, który przyjmuje jeden parametr wejściowy `Month`, typu `int` i zwraca `EmployeesDataTable` wypełniane przy użyciu tych pracowników, którzy mają ich zatrudniania rozliczenia w określonym obiekcie `Month`. Naszym przykładzie zostanie Ustaw ten parametr programowo na podstawie bieżącego miesiąca, przedstawiający listę "Rocznice pracowników w tym miesiącu."

Dzieła!

## <a name="step-1-adding-a-method-toemployeestableadapter"></a>Krok 1: Dodawanie metody do`EmployeesTableAdapter`

W naszym przykładzie pierwsze musimy dodać sposób pobrać pracowników, których `HireDate` w określonym miesiącu. Do tej funkcji, zgodnie z naszej architektury, należy najpierw utworzyć metody w `EmployeesTableAdapter` która jest mapowana do właściwego instrukcji SQL. W tym celu uruchom przez otwarcie zestawu Northwind typu danych. Kliknij prawym przyciskiem myszy `EmployeesTableAdapter` etykiety i wybierz polecenie Dodaj zapytanie.


[![Dodaj nowe zapytanie do EmployeesTableAdapter](programmatically-setting-the-objectdatasource-s-parameter-values-cs/_static/image5.png)](programmatically-setting-the-objectdatasource-s-parameter-values-cs/_static/image4.png)

**Rysunek 2**: Dodaj nowe zapytanie w celu `EmployeesTableAdapter` ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](programmatically-setting-the-objectdatasource-s-parameter-values-cs/_static/image6.png))


Wybierz dodać instrukcję SQL, która zwraca wiersze. Po przejściu określ `SELECT` instrukcji ekranu domyślnie `SELECT` instrukcji dla `EmployeesTableAdapter` już zostanie załadowany. Po prostu Dodaj w `WHERE` klauzuli: `WHERE DATEPART(m, HireDate) = @Month`. [DATEPART](https://msdn.microsoft.com/en-us/library/ms174420.aspx) jest funkcja T-SQL, która zwraca określoną datę część `datetime` typu; w takim przypadku używamy `DATEPART` do zwrócenia miesiąc `HireDate` kolumny.


[![Zwracane tylko tych wierszy gdzie rekrutacji kolumny jest mniejsze niż lub równe @HiredBeforeDate parametru](programmatically-setting-the-objectdatasource-s-parameter-values-cs/_static/image8.png)](programmatically-setting-the-objectdatasource-s-parameter-values-cs/_static/image7.png)

**Rysunek 3**: zwraca tylko te wiersze, dla których `HireDate` kolumn jest mniejsza lub równa `@HiredBeforeDate` parametr ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](programmatically-setting-the-objectdatasource-s-parameter-values-cs/_static/image9.png))


Na koniec zmień `FillBy` i `GetDataBy` nazwy metody `FillByHiredDateMonth` i `GetEmployeesByHiredDateMonth`odpowiednio.


[![Wybierz odpowiednie nazwy metod niż FillBy i GetDataBy](programmatically-setting-the-objectdatasource-s-parameter-values-cs/_static/image11.png)](programmatically-setting-the-objectdatasource-s-parameter-values-cs/_static/image10.png)

**Rysunek 4**: Wybierz bardziej odpowiednie metody nazw niż `FillBy` i `GetDataBy` ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](programmatically-setting-the-objectdatasource-s-parameter-values-cs/_static/image12.png))


Kliknij przycisk Zakończ, aby zakończyć pracę kreatora i powrócić do powierzchni projektu DataSet. `EmployeesTableAdapter` Teraz powinien zawierać nowy zestaw metod dostępu do pracowników zatrudnionych w określonym miesiącu.


[![Nowych metod są wyświetlane w powierzchnię zestawu danych](programmatically-setting-the-objectdatasource-s-parameter-values-cs/_static/image14.png)](programmatically-setting-the-objectdatasource-s-parameter-values-cs/_static/image13.png)

**Rysunek 5**: nowych metod są wyświetlane w powierzchnię projektu DataSet ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](programmatically-setting-the-objectdatasource-s-parameter-values-cs/_static/image15.png))


## <a name="step-2-adding-thegetemployeesbyhireddatemonthmonthmethod-to-the-business-logic-layer"></a>Krok 2: Dodawanie`GetEmployeesByHiredDateMonth(month)`metody do warstwy logiki biznesowej

Ponieważ naszej aplikacji architektura używa oddzielnej warstwy logiki biznesowej i danych dostęp logiki, należy dodać metodę do naszej logiki warstwy Biznesowej wywołania do warstwy DAL do pobrania pracowników zatrudnionych przed upływem określonego terminu. Otwórz `EmployeesBLL.cs` pliku i dodaj następującą metodę:


[!code-csharp[Main](programmatically-setting-the-objectdatasource-s-parameter-values-cs/samples/sample1.cs)]

Podobnie jak w przypadku naszego inne metody w tej klasie `GetEmployeesByHiredDateMonth(month)` po prostu wywołuje w dół do warstwy DAL i zwraca wyniki.

## <a name="step-3-displaying-employees-whose-hiring-anniversary-is-this-month"></a>Krok 3: Wyświetlanie pracowników, których zatrudniania dzień tego miesiąca

Ostatnią czynnością w tym przykładzie jest do wyświetlenia tych pracowników, których zatrudniania dzień tego miesiąca. Rozpocznij od dodania GridView do `ProgrammaticParams.aspx` strony `BasicReporting` folderu i Dodaj nowy element ObjectDataSource jako źródła danych. Element ObjectDataSource umożliwia konfigurowanie `EmployeesBLL` klasy z `SelectMethod` ustawioną `GetEmployeesByHiredDateMonth(month)`.


[![Klasa EmployeesBLL](programmatically-setting-the-objectdatasource-s-parameter-values-cs/_static/image17.png)](programmatically-setting-the-objectdatasource-s-parameter-values-cs/_static/image16.png)

**Rysunek 6**: Użyj `EmployeesBLL` klasy ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](programmatically-setting-the-objectdatasource-s-parameter-values-cs/_static/image18.png))


[![Wybierz z GetEmployeesByHiredDateMonth(month) — metoda](programmatically-setting-the-objectdatasource-s-parameter-values-cs/_static/image20.png)](programmatically-setting-the-objectdatasource-s-parameter-values-cs/_static/image19.png)

**Rysunek 7**: Wybierz z `GetEmployeesByHiredDateMonth(month)` — metoda ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](programmatically-setting-the-objectdatasource-s-parameter-values-cs/_static/image21.png))


Na ekranie końcowym zapyta przekazać nam `month` źródło wartości parametru. Ponieważ programowo zaplanujemy tę wartość, pozostaw źródło parametru domyślną wartość None opcję i kliknij przycisk Zakończ.


[![Pozostaw zestaw parametrów źródła None](programmatically-setting-the-objectdatasource-s-parameter-values-cs/_static/image23.png)](programmatically-setting-the-objectdatasource-s-parameter-values-cs/_static/image22.png)

**Rysunek 8**: pozostaw ustawić parametr źródła None ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](programmatically-setting-the-objectdatasource-s-parameter-values-cs/_static/image24.png))


Spowoduje to utworzenie `Parameter` obiektu w elemencie ObjectDataSource `SelectParameters` kolekcji, która ma określoną wartość.


[!code-aspx[Main](programmatically-setting-the-objectdatasource-s-parameter-values-cs/samples/sample2.aspx)]

Aby ustawić tę wartość programowo, należy utworzyć program obsługi zdarzeń dla elementu ObjectDataSource `Selecting` zdarzeń. W tym celu przejdź do widoku projektu, a następnie kliknij dwukrotnie element ObjectDataSource. Można również wybrać element ObjectDataSource, przejdź do okna właściwości i kliknij ikonę bolt. Następnie, albo kliknij dwukrotnie w polu tekstowym obok pozycji `Selecting` zdarzenia lub wpisz nazwę programu obsługi zdarzeń, którego chcesz użyć.


![Kliknij ikonę błyskawicy w oknie właściwości, aby wyświetlić listę zdarzeń formantu sieci Web](programmatically-setting-the-objectdatasource-s-parameter-values-cs/_static/image25.png)

**Rysunek 9**: kliknij ikonę błyskawicy w oknie właściwości, aby wyświetlić listę zdarzeń formantu sieci Web


W obu przypadkach efekt Dodaj nowy program obsługi zdarzeń dla elementu ObjectDataSource `Selecting` zdarzeń do klasy związane z kodem strony. W tej obsłudze zdarzeń możemy odczytu i zapisu do wartości parametrów za pomocą `e.InputParameters[parameterName]`, gdzie  *`parameterName`*  jest wartością `Name` atrybutu w `<asp:Parameter>` tag ( `InputParameters` można też kolekcji indeksowane ordinally, podobnie jak w `e.InputParameters[index]`). Aby ustawić `month` parametru do bieżącego miesiąca, Dodaj następujący kod do `Selecting` obsługi zdarzeń:


[!code-csharp[Main](programmatically-setting-the-objectdatasource-s-parameter-values-cs/samples/sample3.cs)]

Podczas odwiedzania tej strony za pośrednictwem przeglądarki widać tylko jednego pracownika został dzierżawione w tym miesiącu (marzec) Kowalski Laura, którzy byli od 1994 r. w firmie.


[![Pracowników, których Rocznice są wyświetlane w tym miesiącu](programmatically-setting-the-objectdatasource-s-parameter-values-cs/_static/image27.png)](programmatically-setting-the-objectdatasource-s-parameter-values-cs/_static/image26.png)

**Na rysunku nr 10**: tych pracowników Whose rocznice ten miesiąc są wyświetlane ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](programmatically-setting-the-objectdatasource-s-parameter-values-cs/_static/image28.png))


## <a name="summary"></a>Podsumowanie

Gdy wartości parametrów elementu ObjectDataSource zwykle można ustawić deklaratywnie, bez konieczności wiersz kodu, jest bardzo proste programowane Ustawianie wartości parametrów. Konieczne jest tworzenie procedury obsługi zdarzeń dla elementu ObjectDataSource `Selecting` zdarzenie, które są generowane przed wywoływane metody obiektu źródłowego i ręcznie ustawić wartości dla jednego lub więcej parametrów za pomocą `InputParameters` kolekcji.

Ten samouczek zawiera sekcji podstawowe raportowania. [Następny samouczek](../masterdetail/master-detail-filtering-with-a-dropdownlist-cs.md) dotyczącego sekcji Filtrowanie i danych głównych scenariuszy, w którym firma Microsoft będzie przyjrzeć się techniki stosowanie obiektu odwiedzającego, aby odfiltrować dane i przejść z głównego raportu do raportu szczegółów.

Programowanie przyjemność!

## <a name="about-the-author"></a>Informacje o autorze

[Scott Bento](http://www.4guysfromrolla.com/ScottMitchell.shtml), autora siedmiu książek ASP/ASP.NET i twórcę z [4GuysFromRolla.com](http://www.4guysfromrolla.com), pracuje z technologii Microsoft Web od 1998. Scott działa jako niezależnego konsultanta trainer i składnika zapisywania. Jest jego najnowszej książki [ *Sams nauczyć się ASP.NET 2.0 w ciągu 24 godzin*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Piotr można uzyskać pod adresem [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) lub za pośrednictwem jego blog, który znajduje się w temacie [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Specjalne podziękowania dla

Ten samouczek serii zostało sprawdzone przez wiele recenzentów przydatne. Recenzenta realizacji w tym samouczku został Hilton Giesenow. Zainteresowani recenzowania Moje nadchodzących artykuły MSDN? Jeśli tak, Porzuć mnie linii w [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

>[!div class="step-by-step"]
[Poprzednie](declarative-parameters-cs.md)
[dalej](displaying-data-with-the-objectdatasource-vb.md)
