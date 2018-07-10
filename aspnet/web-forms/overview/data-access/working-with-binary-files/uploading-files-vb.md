---
uid: web-forms/overview/data-access/working-with-binary-files/uploading-files-vb
title: Przekazywanie plików (VB) | Dokumentacja firmy Microsoft
author: rick-anderson
description: Dowiedz się, jak Zezwalaj użytkownikom na przekazywanie plików binarnych (takich jak dokumenty programu Word lub w formacie PDF) do witryny sieci Web, w których mogą być przechowywane w systemu plików na serwerze...
ms.author: aspnetcontent
ms.date: 03/27/2007
ms.assetid: f7c00fbd-652c-433d-8ed3-0e5168a4d4df
msc.legacyurl: /web-forms/overview/data-access/working-with-binary-files/uploading-files-vb
msc.type: authoredcontent
ms.openlocfilehash: 412a809de63f22fd3fddf99ad34d06c079f73fd0
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/05/2018
ms.locfileid: "37802724"
---
<a name="uploading-files-vb"></a>Przekazywanie plików (VB)
====================
przez [Bento Scott](https://twitter.com/ScottOnWriting)

[Pobierz przykładową aplikację](http://download.microsoft.com/download/4/a/7/4a7a3b18-d80e-4014-8e53-a6a2427f0d93/ASPNET_Data_Tutorial_54_VB.exe) lub [Pobierz plik PDF](uploading-files-vb/_static/datatutorial54vb1.pdf)

> Dowiedz się, jak umożliwić użytkownikom przekazywanie plików binarnych (takich jak dokumenty programu Word lub w formacie PDF) do witryny sieci Web, w których mogą być przechowywane w systemie plików serwera lub bazy danych.


## <a name="introduction"></a>Wprowadzenie

Wszystkie z samouczków firma ve badania do tej pory pracy wyłącznie z danych tekstowych. Jednak wiele aplikacji ma modeli danych, które przechwytują dane binarne i tekstowe. Witryna dating online może być Zezwalaj użytkownikom na przekazywanie obrazu do skojarzenia z swój profil. Rekrutacji witryny sieci Web mogą umożliwić użytkownikom przekazywanie ich wznowienia jako dokument programu Microsoft Word lub w formacie PDF.

Praca z danymi binarnymi dodaje nowy zestaw wyzwania. Firma Microsoft należy określić sposób przechowywania danych binarnych w aplikacji. Interfejs umożliwiający wstawianie nowych rekordów musi zostać zaktualizowane, aby umożliwić użytkownikowi do przekazywania plików z komputera, a dodatkowe kroki należy podjąć, aby wyświetlić lub udostępniają sposób pobierania rekord s skojarzone dane binarne. W tym samouczku i trzech dalej pokażemy, jak hurdle te wyzwania. Na końcu tego samouczka będziesz mieć dostosowaliśmy w pełni funkcjonalnych aplikacji, która kojarzy obrazów i plików PDF broszura z każdej kategorii. W tym samouczku określonego utworzymy Spójrz na różnych technik do przechowywania danych binarnych i Zbadaj, jak umożliwić użytkownikom przekazywanie plików z komputera i zostały zapisane w systemie plików s serwera sieci web.

> [!NOTE]
> Dane binarne, który jest częścią modelu danych s aplikacji jest czasami określane jako [BLOB](http://en.wikipedia.org/wiki/Binary_large_object), akronim duży obiekt binarny. W tych samouczkach I wybranego do użycia terminologii dane binarne, chociaż terminem jest równoznaczny obiektu BLOB.


## <a name="step-1-creating-the-working-with-binary-data-web-pages"></a>Krok 1: Tworzenie pracy ze stronami sieci Web danych binarnych

Zanim zaczniemy Poznaj wyzwania związane z dodaniem obsługi dla danych binarnych, niech s najpierw Poświęć chwilę, do tworzenia stron ASP.NET w naszym projektu witryny sieci Web, który będziemy potrzebować dla tego samouczka i trzy dalej. Rozpocznij od dodania nowy folder o nazwie `BinaryData`. Następnie dodaj następujące strony ASP.NET do tego folderu, upewniając się skojarzyć każdą stronę z `Site.master` strona główna:

- `Default.aspx`
- `FileUpload.aspx`
- `DisplayOrDownloadData.aspx`
- `UploadInDetailsView.aspx`
- `UpdatingAndDeleting.aspx`


![Dodawanie stron ASP.NET binarne samouczki związanych z danymi](uploading-files-vb/_static/image1.gif)

**Rysunek 1**: Dodawanie stron ASP.NET binarne samouczki związanych z danymi


Podobnie jak w przypadku innych folderów `Default.aspx` w `BinaryData` folderu wyświetli listę samouczków w jego sekcji. Pamiętamy `SectionLevelTutorialListing.ascx` kontrolki użytkownika oferuje tę funkcję. W związku z tym, Dodaj ten formant użytkownika do `Default.aspx` , przeciągając go z poziomu Eksploratora rozwiązań na stronę s widoku projektu.


[![Dodaj formant użytkownika SectionLevelTutorialListing.ascx na Default.aspx](uploading-files-vb/_static/image2.gif)](uploading-files-vb/_static/image1.png)

**Rysunek 2**: Dodaj `SectionLevelTutorialListing.ascx` kontrolki użytkownika do `Default.aspx` ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](uploading-files-vb/_static/image2.png))


Wreszcie, Dodaj te strony jako wpisy, aby `Web.sitemap` pliku. W szczególności należy dodać następujące znaczniki po Enhancing widoku GridView `<siteMapNode>`:


[!code-xml[Main](uploading-files-vb/samples/sample1.xml)]

Po zaktualizowaniu `Web.sitemap`, Poświęć chwilę, aby wyświetlić witrynę sieci Web w samouczkach, za pośrednictwem przeglądarki. Menu po lewej stronie zawiera teraz elementów do pracy z samouczkami danych binarnych.


![Mapa witryny zawiera teraz wpisy dotyczące pracy z samouczkami danych binarnych](uploading-files-vb/_static/image3.gif)

**Rysunek 3**: mapy witryny zawiera teraz wpisy dotyczące pracy z samouczkami danych binarnych


## <a name="step-2-deciding-where-to-store-the-binary-data"></a>Krok 2: Decydowanie o Store danych binarnych

Dane binarne, który jest skojarzony z modelem danych s aplikacji mogą być przechowywane w jednym z dwóch miejsc: w systemie plików serwera s sieci web z odwołaniem do plików przechowywanych w bazie danych. lub bezpośrednio z poziomu samej bazy danych (zobacz rysunek 4). Każde podejście ma swój własny zestaw zalet i wad i uzasadnia uzyskać bardziej szczegółowe informacje.


[![Dane binarne mogą być przechowywane w systemie plików lub bezpośrednio w bazie danych](uploading-files-vb/_static/image4.gif)](uploading-files-vb/_static/image3.png)

**Rysunek 4**: dane binarne mogą być przechowywane w systemie plików lub bezpośrednio w bazie danych ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](uploading-files-vb/_static/image4.png))


Wyobraź sobie, że Chcieliśmy, aby rozszerzyć bazy danych Northwind, aby skojarzyć obrazu z każdego produktu. Jedną z opcji będzie przechowywać te pliki obrazów w systemie plików serwera s sieci web i rejestrowania ścieżki w `Products` tabeli. W przypadku tej metody, d dodamy `ImagePath` kolumny `Products` tabelę typu `varchar(200)`, być może. Po użytkownik przekazany obraz dla Chai, ten obraz może być przechowywany w systemie plików serwera s sieci web w `~/Images/Tea.jpg`, gdzie `~` reprezentuje ścieżkę fizyczną s aplikacji. Oznacza to jeśli witryna sieci web jest ścieżką w ścieżce fizycznej `C:\Websites\Northwind\`, `~/Images/Tea.jpg` byłaby równoważna `C:\Websites\Northwind\Images\Tea.jpg`. Po przekazaniu pliku obrazu, d aktualizujemy rekordu Chai w `Products` tabeli, aby jego `ImagePath` ścieżkę obraz nowe odwołanie do kolumny. Moglibyśmy użyć `~/Images/Tea.jpg` lub po prostu `Tea.jpg` Jeśli zdecydowaliśmy się, że wszystkie obrazy produktów zostanie umieszczony w tym s aplikacji `Images` folderu.

Dostępne są następujące główne zalety przechowywania danych binarnych w systemie plików:

- **Łatwość implementacji** jako zajmiemy się wkrótce, przechowywania i pobierania danych binarnych, które są przechowywane bezpośrednio w bazie danych wymaga nieco więcej kodu niż gdy praca z danymi za pomocą systemu plików. Ponadto w kolejności dla użytkownika wyświetlić lub pobrać dane binarne muszą one być przedstawione za pomocą adresu URL, do tych danych. Jeśli dane znajdują się w systemie plików s serwera sieci web, adres URL jest bardzo proste. Jeśli dane są przechowywane w bazie danych, jednak strony sieci web musi odbywać się, które pobierze i zwrócić dane z bazy danych.
- **Szerszy dostęp do danych binarnych** danych binarnych mogą muszą być dostępne dla innych usług lub aplikacji, te, które nie może pobrać dane z bazy danych. Na przykład obrazów skojarzonych z każdego produktu może również muszą być dostępne dla użytkowników za pomocą [FTP](http://en.wikipedia.org/wiki/File_Transfer_Protocol), w którym to przypadku d chcemy przechowywanie danych binarnych w systemie plików.
- **Wydajność** Jeśli danych binarnych jest przechowywany w systemie plików, przeciążenie żądanie, jak i sieci między serwerem bazy danych i serwera sieci web będzie mniejsza niż Jeśli dane binarne są przechowywane bezpośrednio w bazie danych.

Główną wadą przechowywanie danych binarnych w systemie plików jest, że są oddzieleni dane z bazy danych. Jeśli rekord zostanie usunięty z `Products` tabeli, skojarzony plik w systemie plików serwera s sieci web nie zostanie automatycznie usunięta. Firma Microsoft musi zapisać dodatkowego kodu można usunąć pliku lub systemu plików zostanie zapełnić nieużywane, oddzielone pliki. Ponadto podczas tworzenia kopii zapasowej bazy danych, firma Microsoft musi upewnij się utworzyć kopie zapasowe skojarzone dane binarne w systemie plików, a także. Przenoszenie bazy danych do innej lokacji lub serwera stwarza podobne problemy.

Alternatywnie danych binarnych mogą być przechowywane bezpośrednio w bazie danych programu Microsoft SQL Server 2005, tworząc kolumny typu `varbinary`. Podobnie jak przy użyciu innych typów danych o zmiennej długości, można określić maksymalną długość danych binarnych, które mogą być przechowywane w tej kolumnie. Na przykład aby zarezerwować maksymalnie 5000 bajtów użyć `varbinary(5000)`; `varbinary(MAX)` pozwala uzyskać maksymalny rozmiar około 2 GB.

Główną zaletą przechowywanie danych binarnych bezpośrednio w bazie danych jest ścisłego sprzężenia między danych binarnych i rekordu bazy danych. To znacznie upraszcza zadania administracyjne bazy danych, takich jak tworzenie kopii zapasowych, lub przenoszenia bazy danych do innej lokacji lub serwera. Ponadto usunięcie rekordu automatycznie usuwa odpowiadające im dane binarne. Istnieją również więcej subtelne zalety przechowywania danych binarnych w bazie danych. Zobacz [przechowywania binarnych plików bezpośrednio w bazie danych za pomocą programu ASP.NET 2.0](http://aspnet.4guysfromrolla.com/articles/120606-1.aspx) bardziej szczegółowe omówienie.

> [!NOTE]
> W programie Microsoft SQL Server 2000 i wcześniejszych wersji `varbinary` — typ danych ma maksymalny limit 8000 bajtów. Do przechowywania maksymalnie 2 GB danych binarnych [ `image` — typ danych](https://msdn.microsoft.com/library/ms187993.aspx) musi być używane zamiast tego. Z dodatkiem `MAX` w programie SQL Server 2005, jednak `image` — typ danych jest przestarzała. Jego s nadal obsługiwane dla zapewnienia zgodności, ale firma Microsoft poinformowała o który `image` — typ danych zostanie usunięta w przyszłych wersjach programu SQL Server.


Jeśli pracujesz z starszego modelu danych może zostać wyświetlony `image` typu danych. Bazy danych Northwind s `Categories` tabela ma `Picture` kolumny, która może służyć do przechowywania danych binarnych plik obrazu dla kategorii. Ponieważ bazy danych Northwind ma jego katalogów głównych programu Microsoft Access i wcześniejszych wersjach programu SQL Server, ta kolumna jest typu `image`.

W tym samouczku i dalej trzech użyjemy oba podejścia. `Categories` Tabela ma już `Picture` kolumny do przechowywania zawartości binarnej obrazu dla kategorii. Dodamy dodatkowe kolumny `BrochurePath`w celu przechowywania ścieżkę do pliku PDF w systemie plików serwera s sieci web, który może służyć do zapewnienia jakość wydruku, dopracowane Przegląd kategorii.

## <a name="step-3-adding-thebrochurepathcolumn-to-thecategoriestable"></a>Krok 3. Dodawanie`BrochurePath`kolumny`Categories`tabeli

Obecnie ma tylko cztery kolumny w tabeli Kategorie: `CategoryID`, `CategoryName`, `Description`, i `Picture`. Oprócz tych pól należy dodać nowy, który będzie wskazywać broszura kategorii s (jeśli istnieje). Aby dodać tę kolumnę, przejdź do Eksploratora serwera, przejdź do szczegółów tabel, kliknij prawym przyciskiem myszy `Categories` tabeli, a następnie wybierz polecenie Otwórz definicję tabeli (zobacz rysunek 5). Jeśli Eksplorator serwera nie jest widoczny, wyświetlenie go, wybierając opcję Server Explorer z menu Widok lub naciśnij klawisze Ctrl + Alt + S.

Dodaj nową `varchar(200)` kolumny `Categories` tabelę o nazwie `BrochurePath` i umożliwia `NULL` s i kliknij ikonę Zapisz (lub naciśnij klawisze Ctrl + S).


[![Dodaj kolumnę BrochurePath do tabeli Kategorie](uploading-files-vb/_static/image5.gif)](uploading-files-vb/_static/image5.png)

**Rysunek 5**: Dodaj `BrochurePath` kolumny `Categories` tabeli ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](uploading-files-vb/_static/image6.png))


## <a name="step-4-updating-the-architecture-to-use-thepictureandbrochurepathcolumns"></a>Krok 4: Aktualizowanie architektury w celu używania`Picture`i`BrochurePath`kolumn

`CategoriesDataTable` w warstwie dostępu do danych (DAL) obecnie ma cztery `DataColumn` s zdefiniowany: `CategoryID`, `CategoryName`, `Description`, i `NumberOfProducts`. Jeśli pierwotnie został zaprojektowany tego elementu DataTable w [Tworzenie warstwy dostępu do danych](../introduction/creating-a-data-access-layer-vb.md) samouczku `CategoriesDataTable` ustawiono tylko pierwsze trzy kolumny; `NumberOfProducts` została dodana kolumna w [wzorzec/szczegół za pomocą punktowane Lista rekordów głównych z kontrolką DataList szczegółów](../filtering-scenarios-with-the-datalist-and-repeater/master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb.md) samouczka.

Zgodnie z opisem w *Tworzenie warstwy dostępu do danych*, DataTable w DataSet wpisane tworzą obiekty biznesowe. Elementów TableAdapter są odpowiedzialne za komunikację z bazą danych i wypełnianie obiektów biznesowych z wynikami zapytania. `CategoriesDataTable` Jest wypełniana przez `CategoriesTableAdapter`, która ma trzy metody pobierania danych:

- `GetCategories()` wykonuje TableAdapter s główne zapytanie i zwraca `CategoryID`, `CategoryName`, i `Description` pola wszystkie rekordy w `Categories` tabeli. Główne zapytanie to, co jest używany przez wygenerowane automatycznie `Insert` i `Update` metody.
- `GetCategoryByCategoryID(categoryID)` Zwraca `CategoryID`, `CategoryName`, i `Description` pola kategorii, której `CategoryID` jest równa *categoryID*.
- `GetCategoriesAndNumberOfProducts()` -Zwraca `CategoryID`, `CategoryName`, i `Description` pól dla wszystkich rekordów w `Categories` tabeli. Używa również podzapytania do zwracają liczbę produktów skojarzonych z każdej kategorii.

Zwróć uwagę, że żaden z tych zapytań powrotu `Categories` tabeli s `Picture` lub `BrochurePath` kolumny; ani ma `CategoriesDataTable` zapewniają `DataColumn` s dla tych pól. Aby pracować z obrazu i `BrochurePath` właściwości, należy najpierw dodać je do `CategoriesDataTable` , a następnie zaktualizuj `CategoriesTableAdapter` klasy w celu zwracania tych kolumn.

## <a name="adding-thepictureandbrochurepathdatacolumn-s"></a>Dodawanie`Picture`i`BrochurePath``DataColumn` s

Rozpocznij od dodania tych dwóch kolumn, aby `CategoriesDataTable`. Kliknij prawym przyciskiem myszy `CategoriesDataTable` s nagłówka, z menu kontekstowego wybierz polecenie Dodaj, a następnie wybierz opcję kolumny. Spowoduje to utworzenie nowego `DataColumn` w elemencie DataTable o nazwie `Column1`. Zmień nazwę tej kolumny na `Picture`. W oknie właściwości ustaw `DataColumn` s `DataType` właściwość `System.Byte[]` (nie jest to opcja na liście rozwijanej; należy go wpisać).


[![Tworzenie obrazu o nazwie DataColumn, którego typem danych jest System.Byte](uploading-files-vb/_static/image6.gif)](uploading-files-vb/_static/image7.png)

**Rysunek 6**: tworzenie `DataColumn` nazwanych `Picture` którego `DataType` jest `System.Byte[]` ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](uploading-files-vb/_static/image8.png))


Dodaj kolejną `DataColumn` do elementu DataTable, nadając mu nazwę `BrochurePath` przy użyciu domyślnego `DataType` wartość (`System.String`).

## <a name="returning-thepictureandbrochurepathvalues-from-the-tableadapter"></a>Zwracanie`Picture`i`BrochurePath`wartości z elementu TableAdapter

Za pomocą tych dwóch `DataColumn` s dodane do `CategoriesDataTable`, możemy ponownie gotowe do aktualizacji `CategoriesTableAdapter`. Firma Microsoft może mieć obie te wartości kolumny zwracane w główne zapytanie TableAdapter, ale to może przywrócić dane binarne każdym `GetCategories()` metoda została wywołana. Zamiast tego umożliwiają s Zaktualizuj główne zapytanie TableAdapter przywrócić `BrochurePath` i Utwórz metodę pobierania dodatkowych danych, która zwraca określoną kategorię s `Picture` kolumny.

Aby zaktualizować główne zapytanie TableAdapter, kliknij prawym przyciskiem myszy `CategoriesTableAdapter` nagłówka s i wybierz opcję Konfiguruj z menu kontekstowego. Otwarte Kreatora konfiguracji adaptera tabeli, które firma Microsoft ve widoczne w numerze z poprzednich samouczków. Aktualizuj zapytanie, aby przywrócić `BrochurePath` i kliknij przycisk Zakończ.


[![Aktualizowanie listy kolumn w instrukcji SELECT również zwraca BrochurePath](uploading-files-vb/_static/image7.gif)](uploading-files-vb/_static/image9.png)

**Rysunek 7**: aktualizowanie listy kolumn w `SELECT` instrukcji, aby również zwrócić `BrochurePath` ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](uploading-files-vb/_static/image10.png))


Korzystając z instrukcji SQL zapytań ad-hoc do TableAdapter, aktualizowanie listy kolumn w głównym zapytaniu aktualizuje listy kolumn dla wszystkich `SELECT` zapytania metody TableAdapter. Oznacza to, że `GetCategoryByCategoryID(categoryID)` metoda został zaktualizowany, aby zwrócić `BrochurePath` kolumny, która może być firma Microsoft przeznaczony. Jednak również zaktualizowane listy kolumn w `GetCategoriesAndNumberOfProducts()` metody, usunięcie podzapytania, która zwraca liczbę produktów dla każdej kategorii! W związku z tym, należy zaktualizować tę metodę s `SELECT` zapytania. Kliknij prawym przyciskiem myszy `GetCategoriesAndNumberOfProducts()` metody, wybierz opcję Konfiguruj i przywrócić `SELECT` zapytania do oryginalnej wartości:


[!code-sql[Main](uploading-files-vb/samples/sample2.sql)]

Następnie należy utworzyć nową metodę TableAdapter, która zwraca określoną kategorię s `Picture` wartości w kolumnie. Kliknij prawym przyciskiem myszy `CategoriesTableAdapter` nagłówka s i wybierz opcję Dodaj zapytanie, aby uruchomić Kreatora konfiguracji zapytań TableAdapter. Pierwszym krokiem tego kreatora pyta, czy nam chcemy zapytania o dane za pomocą instrukcji SQL zapytań ad-hoc nową przechowywaną procedurę lub istniejącą grupę. Użyj instrukcji SQL zaznaczyć, a następnie kliknij przycisk Dalej. Ponieważ firma Microsoft będzie zwracać wiersz, wybierz polecenie SELECT, która zwraca wiersze opcji w drugim kroku.


[![Wybierz opcję Użyj instrukcji SQL — opcja](uploading-files-vb/_static/image8.gif)](uploading-files-vb/_static/image11.png)

**Rysunek 8**: wybierz opcję Użyj instrukcji SQL opcji ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](uploading-files-vb/_static/image12.png))


[![Ponieważ zapytanie zwróci rekord z tabeli Kategorie, wybierz pozycję Wybierz, która zwraca wiersze](uploading-files-vb/_static/image9.gif)](uploading-files-vb/_static/image13.png)

**Rysunek 9**: ponieważ zapytanie zwraca rekord z tabeli Kategorie, wybierz pozycję Wybierz, która zwraca wiersze ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](uploading-files-vb/_static/image14.png))


W trzecim kroku wprowadź następujące zapytanie SQL, a następnie kliknij przycisk Dalej:


[!code-sql[Main](uploading-files-vb/samples/sample3.sql)]

Ostatnim krokiem jest wybranie nazwy dla nowej metody. Użyj `FillCategoryWithBinaryDataByCategoryID` i `GetCategoryWithBinaryDataByCategoryID` wypełnienia DataTable i zwrócenia DataTable wzorców, odpowiednio. Kliknij przycisk Zakończ, aby zakończyć działanie kreatora.


[![Wybierz nazwy dla metody s TableAdapter](uploading-files-vb/_static/image10.gif)](uploading-files-vb/_static/image15.png)

**Na rysunku nr 10**: Wybierz nazwy s TableAdapter metody ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](uploading-files-vb/_static/image16.png))


> [!NOTE]
> Po zakończeniu pracy Kreatora konfiguracji zapytań adaptera tabeli zobaczysz okno dialogowe informujące o tym, że nowy tekst polecenia zwraca dane przy użyciu schematu inny niż schemat głównego zapytania. Krótko mówiąc, Kreator jest biorąc pod uwagę, że główne zapytanie TableAdapter s `GetCategories()` zwraca schemat innej niż ta, którą właśnie utworzyliśmy. Ale to chcemy, aby pominąć ten komunikat.


Ponadto należy pamiętać, że jeśli używasz instrukcji SQL zapytań ad-hoc, użyj kreatora, aby zmienić TableAdapter s główne zapytanie w pewnym momencie później w czasie jego zmodyfikuje `GetCategoryWithBinaryDataByCategoryID` metoda s `SELECT` instrukcja s listy kolumn, aby uwzględnić tylko te kolumny z główne zapytanie (czyli spowoduje usunięcie `Picture` kolumn z zapytania). Trzeba będzie ręcznie zaktualizować listę kolumn, aby przywrócić `Picture` kolumny, podobnie jak postępowanie z `GetCategoriesAndNumberOfProducts()` metoda wcześniej w tym kroku.

Po dodaniu dwóch `DataColumn` s `CategoriesDataTable` i `GetCategoryWithBinaryDataByCategoryID` metody `CategoriesTableAdapter`, te klasy w Projektancie obiektów DataSet wpisane powinien wyglądać zrzut ekranu w rysunek 11.


![Projektant obiektów DataSet zawiera nowe kolumny i — metoda](uploading-files-vb/_static/image11.gif)

**Rysunek 11**: Projektanta obiektów DataSet zawiera nowe kolumny i — metoda


## <a name="updating-the-business-logic-layer-bll"></a>Aktualizowanie warstwy logiki biznesowej (LOGIKI)

Za pomocą warstwy DAL zaktualizowane pozostaje rozszerzyć warstwę logiki biznesowej (LOGIKI) do uwzględnienia metody dla nowego `CategoriesTableAdapter` metody. Dodaj następującą metodę do `CategoriesBLL` klasy:


[!code-vb[Main](uploading-files-vb/samples/sample4.vb)]

## <a name="step-5-uploading-a-file-from-the-client-to-the-web-server"></a>Krok 5: Przekazywanie plików z klienta do serwera sieci Web

Podczas zbierania danych binarnych, często tych danych jest dostarczany przez użytkownika końcowego. Aby przechwytywać te informacje, użytkownik musi mieć możliwość przekazywania plików z komputera z serwerem sieci web. Przekazane dane musi być zintegrowane z modelu danych, który może oznaczać, że zapisywania pliku w systemie plików s serwera sieci web i dodając ścieżkę do pliku w bazie danych lub zapisując treść binarnych bezpośrednio do bazy danych. W tym kroku omówimy sposób zezwalający użytkownikowi na przekazywanie plików z komputera z serwerem. W następnym samouczku firma Microsoft będzie włączyć naszej uwagi do integracji przekazanego pliku przy użyciu modelu danych.

Program ASP.NET 2.0 nowego [formantu sieci Web FileUpload](https://msdn.microsoft.com/library/ms227677(VS.80).aspx) udostępnia mechanizm dla użytkowników wysłać plik z komputera z serwerem sieci web. Kontrolka FileUpload renderowany jako `<input>` elementu którego `type` atrybut jest ustawiony na plik, który przeglądarek wyświetlane jako pole tekstowe za pomocą przycisku przeglądania. Kliknięcie przycisku Przeglądaj otwiera okno dialogowe, w którym użytkownik może wybrać plik. Ponownie opublikowania formularza zawartość s wybranego pliku są wysyłane wraz z zwrotu. Po stronie serwera informacje o przekazany plik jest dostępny za pośrednictwem właściwości s FileUpload.

Aby zademonstrować przekazywanie plików, należy otworzyć `FileUpload.aspx` strony w `BinaryData` folderu, przeciągnij formant FileUpload z przybornika do projektanta i ustawić s `ID` właściwość `UploadTest`. Następnie dodaj kontrolkę przycisk w sieci Web, ustawianie jego `ID` i `Text` właściwości w celu `UploadButton` i przekaż plik wybrane odpowiednio. Na koniec, umieść formantu etykiety w sieci Web, poniżej przycisku wyczyścić jej `Text` właściwości i ustaw jego `ID` właściwość `UploadDetails`.


[![Dodaj kontrolkę FileUpload do strony ASP.NET](uploading-files-vb/_static/image12.gif)](uploading-files-vb/_static/image17.png)

**Rysunek 12**: Dodawanie formantu FileUpload do strony ASP.NET ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](uploading-files-vb/_static/image18.png))


Rysunek 13 przedstawiono tej strony, podczas wyświetlania za pośrednictwem przeglądarki. Należy pamiętać, że kliknięcie przycisku Przeglądaj wywołuje okno dialogowe wyboru plików, co pozwala na pobranie pliku z komputera. Po wybraniu pliku kliknięcie przycisku przekazywania wybranego pliku powoduje odświeżenie strony, który przesyła zawartość binarną s wybrany plik do serwera sieci web.


[![Użytkownik może wybrać plik do przekazania z komputera z serwerem](uploading-files-vb/_static/image13.gif)](uploading-files-vb/_static/image19.png)

**Rysunek 13**: użytkownik może wybrać plik do przekazania z komputera z serwerem ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](uploading-files-vb/_static/image20.png))


Odświeżenie strony przekazany plik można zapisać do systemu plików lub jego danych binarnych można by pracować bezpośrednio za pomocą Stream. W tym przykładzie umożliwiają tworzenie s `~/Brochures` folder i zapisać przekazany plik istnieje. Rozpocznij od dodania `Brochures` folderu do lokacji jako podfolder katalogu głównego. Następnie należy utworzyć program obsługi zdarzeń dla `UploadButton` s `Click` zdarzeń i Dodaj następujący kod:


[!code-vb[Main](uploading-files-vb/samples/sample5.vb)]

Formant FileUpload zapewnia szereg właściwości przekazane dane. Na przykład [ `HasFile` właściwość](https://msdn.microsoft.com/library/system.web.ui.webcontrols.fileupload.hasfile.aspx) wskazuje, czy plik został przekazany przez użytkownika, gdy [ `FileBytes` właściwość](https://msdn.microsoft.com/library/system.web.ui.webcontrols.fileupload.filebytes.aspx) zapewnia dostęp do danych binarnych przekazanych jako tablicę bajtów. `Click` Program obsługi zdarzeń, który rozpoczyna się poprzez zapewnienie, że plik został przekazany. Jeśli plik został przekazany, etykieta jest wyświetlana nazwa przekazanego pliku, jego rozmiar w bajtach i typu zawartości.

> [!NOTE]
> Aby upewnić się, że użytkownik służy do przekazywania pliku możesz sprawdzić `HasFile` właściwości i wyświetlić ostrzeżenie, jeśli jego s `False`, lub mogą używać [kontroli RequiredFieldValidator](https://quickstarts.asp.net/QuickStartv20/aspnet/doc/validation/default.aspx) zamiast tego.


FileUpload s `SaveAs(filePath)` zapisuje przekazany plik do określonego *filePath*. *filePath* musi być *ścieżkę fizyczną* (`C:\Websites\Brochures\SomeFile.pdf`) zamiast *wirtualnego* *ścieżki* (`/Brochures/SomeFile.pdf`). [ `Server.MapPath(virtPath)` Metoda](https://msdn.microsoft.com/library/system.web.httpserverutility.mappath.aspx) przyjmuje ścieżkę wirtualną i zwraca jego odpowiedniego ścieżka fizyczna. W tym miejscu, ścieżka wirtualna jest `~/Brochures/fileName`, gdzie *fileName* nazywa się przekazanego pliku. Zobacz [przy użyciu Server.MapPath](http://www.4guysfromrolla.com/webtech/121799-1.shtml) uzyskać więcej informacji na temat ścieżek wirtualnych i fizycznych i przy użyciu `Server.MapPath`.

Po zakończeniu `Click` procedura obsługi zdarzeń, Poświęć chwilę, aby przetestować stronę w przeglądarce. Kliknij przycisk Przeglądaj i wybierz plik z dysku twardego, a następnie kliknij przycisk Przekaż wybrany plik. Zwrot wyśle zawartość wybranego pliku do serwera sieci web, który następnie zostaną wyświetlone informacje o pliku przed jego zapisaniem `~/Brochures` folderu. Po przekazaniu pliku, wróć do programu Visual Studio, a następnie kliknij przycisk Odśwież w Eksploratorze rozwiązań. Powinien zostać wyświetlony plik, który właśnie został przekazany w folderze ~/Brochures!


[![EvolutionValley.jpg plik został przekazany do serwera sieci Web](uploading-files-vb/_static/image14.gif)](uploading-files-vb/_static/image21.png)

**Rysunek 14**: plik `EvolutionValley.jpg` został przekazany do serwera sieci Web ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](uploading-files-vb/_static/image22.png))


![EvolutionValley.jpg został zapisany w folderze ~/Brochures](uploading-files-vb/_static/image15.gif)

**Rysunek 15**: `EvolutionValley.jpg` został zapisany w `~/Brochures` folderu


## <a name="subtleties-with-saving-uploaded-files-to-the-file-system"></a>Precyzyjnie z zapisywaniem przekazywanych plików do systemu plików

Istnieje kilka precyzyjnie, które muszą być kierowane przy zapisywaniu przekazywania plików do systemu plików s serwera sieci web. Po pierwsze, miejsca s problem zabezpieczeń. Aby zapisać plik w systemie plików, kontekstu zabezpieczeń, w którym jest wykonywany strony ASP.NET musi mieć uprawnienia do zapisu. Serwer sieci Web programu ASP.NET Development jest uruchamiany w kontekście bieżącego konta użytkownika. Jeśli używasz s Microsoft Internet Information Services (IIS) jako serwera sieci web, kontekstu zabezpieczeń zależy od wersji usług IIS i jego konfiguracji.

Inne wyzwanie zapisywania plików w systemie plików problemem nazewnictwa plików. Obecnie nasza strona zapisuje przekazywanych plików do `~/Brochures` katalogu przy użyciu takiej samej nazwie jak plik na komputerze klienckim s. Jeśli użytkownik A prześle broszura o nazwie `Brochure.pdf`, plik zostanie zapisany jako `~/Brochure/Brochure.pdf`. Ale co zrobić, jeśli jakiś czas nowsze użytkownikowi B służy do przekazywania pliku broszura różnych zdarzeń ma taką samą nazwę (`Brochure.pdf`)? Z kodem mamy teraz użytkownika pliku s zostanie zastąpiony użytkownikowi B s przekazywania.

Istnieje kilka technik rozwiązywania konfliktów nazw plików. Jedną z opcji jest ze Stanów Zjednoczonych zabraniają przekazywania pliku, jeśli istnieje już jeden o takiej samej nazwie. W przypadku tej metody, gdy użytkownik B podejmuje próbę przekazania pliku o nazwie `Brochure.pdf`, system nie będzie zapisać ich plik i zamiast tego wyświetli się komunikat z informacją, użytkownikowi B, aby zmienić nazwę pliku i spróbuj ponownie. Innym rozwiązaniem jest zapisanie pliku przy użyciu unikatową nazwę pliku, który może być [Unikatowy identyfikator globalny (GUID)](http://en.wikipedia.org/wiki/Globally_Unique_Identifier) lub wartość od odpowiadających im bazy danych rekordu s podstawowej kolumny klucza (przy założeniu, że przekazywanie jest skojarzony z określonego wiersza w modelu danych). W następnym samouczku przedstawimy tych opcji bardziej szczegółowo.

## <a name="challenges-involved-with-very-large-amounts-of-binary-data"></a>Wyzwania związane z bardzo dużymi ilościami danych binarnych

Tych samouczków założono, że przechwycone dane binarne jest niewielkim rozmiarze. Korzysta z bardzo dużymi ilościami pliki danych binarnych, które są kilku megabajtów lub większych wprowadza wyzwania, które wykraczają poza zakres tego samouczka. Na przykład domyślnie ASP.NET odrzuci przekazywania więcej niż 4 MB, mimo że można również skonfigurować za pomocą [ `<httpRuntime>` elementu](https://msdn.microsoft.com/library/e1f13641.aspx) w `Web.config`. IIS nakłada ograniczenia rozmiaru przekazywania swój własny plik zbyt. Zobacz [rozmiar pliku przekazywania IIS](http://vandamme.typepad.com/development/2005/09/iis_upload_file.html) Aby uzyskać więcej informacji. Ponadto czas poświęcony na przekazywanie dużych plików może przekraczać domyślny 110 sekund, przez program ASP.NET będzie czekać na żądanie. Dostępne są także pamięć i wydajność problemy, które występują podczas pracy z dużymi plikami.

Kontrolka FileUpload jest niepraktyczne przekazywania plików o dużym rozmiarze. Jako zawartość pliku s są są ogłaszane na serwerze, użytkownik końcowy cierpliwie oczekiwania bez żadnych potwierdzeń postępuje ich przekazywania. Nie jest zbyt duża, wystąpił problem podczas pracy z mniejszych plików, które można przekazać w ciągu kilku sekund, ale może być problemem podczas zajmowania się większe pliki, które może potrwać do przekazania. Istnieje wiele innych firm pliku formantów przekazywania, które są lepiej dostosowane do obsługi przekazywania dużych i wiele z tych dostawców zapewniają wskaźniki postępu i ActiveX przekazywanie menedżerów, są one znacznie bardziej dopracowane środowiska użytkownika.

Jeśli Twoja aplikacja potrzebuje do obsługi dużych plików, należy dokładnie zbadać wyzwań i Znajdź odpowiednie rozwiązania dla Twoich konkretnych potrzeb.

## <a name="summary"></a>Podsumowanie

Tworzenie aplikacji, która ma przechwytywać dane binarne wprowadzono szereg wyzwań. W tym samouczku rozważyliśmy dwa pierwsze: Decydowanie o przechowywanie danych binarnych i pozwalając użytkownikom na przekazywanie zawartości binarnej za pośrednictwem strony sieci web. W ciągu trzech kolejnych samouczków zobaczymy, jak skojarzyć przekazane dane przy użyciu rekordu w bazie danych, a także sposób wyświetlania danych binarnych wraz z jej pól danych tekstowych.

Wszystkiego najlepszego programowania!

## <a name="further-reading"></a>Dalsze informacje

Więcej informacji na tematów omówionych w tym samouczku można znaleźć w następujących zasobach:

- [Używanie typów danych dużych wartości](https://msdn.microsoft.com/library/ms178158.aspx)
- [Przewodniki Szybki Start fileUpload kontroli](https://quickstarts.asp.net/QuickStartv20/aspnet/doc/ctrlref/standard/fileupload.aspx)
- [Formant serwera ASP.NET 2.0 FileUpload](http://www.wrox.com/WileyCDA/Section/id-292158.html)
- [Ciemny boku przekazywania plików](http://www.aspnetresources.com/articles/dark_side_of_file_uploads.aspx)

## <a name="about-the-author"></a>Informacje o autorze

[Scott Bento](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor siedem ASP/ASP.NET książek i założycielem [4GuysFromRolla.com](http://www.4guysfromrolla.com), pracował nad przy użyciu technologii Microsoft Web od 1998 r. Scott działa jako niezależny Konsultant, trainer i składnika zapisywania. Jego najnowszą książkę Stephena [ *Sams uczyć się ASP.NET 2.0 w ciągu 24 godzin*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). ADAM można z Tobą skontaktować w [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) lub za pośrednictwem jego blogu, który znajduje się w temacie [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Specjalne podziękowania dla

W tej serii samouczków został zrecenzowany przez wielu recenzentów pomocne. Wiodące osób dokonujących przeglądu, w tym samouczku zostały Teresa Murphy i Bernadette Leigh. Zainteresowani zapoznaniem Moje kolejnych artykułów MSDN? Jeśli tak, Porzuć mnie linii w [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Poprzednie](updating-and-deleting-existing-binary-data-cs.md)
> [dalej](displaying-binary-data-in-the-data-web-controls-vb.md)
