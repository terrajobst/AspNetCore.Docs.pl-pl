---
uid: web-forms/overview/data-access/working-with-binary-files/uploading-files-vb
title: Przekazywanie plików (VB) | Dokumentacja firmy Microsoft
author: rick-anderson
description: Dowiedz się, jak Zezwalaj użytkownikom na przekazywanie plików binarnych (takich jak dokumenty programu Word lub PDF) do witryny sieci Web, w którym mogą być przechowywane w systemu plików na serwerze...
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/27/2007
ms.topic: article
ms.assetid: f7c00fbd-652c-433d-8ed3-0e5168a4d4df
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/working-with-binary-files/uploading-files-vb
msc.type: authoredcontent
ms.openlocfilehash: fbc4aaf80ac7e0f960e140b492055fe35cd2b6ce
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/10/2018
---
<a name="uploading-files-vb"></a>Przekazywanie plików (VB)
====================
przez [Bento Scott](https://twitter.com/ScottOnWriting)

[Pobierz przykładową aplikację](http://download.microsoft.com/download/4/a/7/4a7a3b18-d80e-4014-8e53-a6a2427f0d93/ASPNET_Data_Tutorial_54_VB.exe) lub [pobierania plików PDF](uploading-files-vb/_static/datatutorial54vb1.pdf)

> Dowiedz się, jak Zezwalaj użytkownikom na przekazywanie plików binarnych (takich jak dokumenty programu Word lub PDF) do witryny sieci Web, w którym mogą być przechowywane w systemie plików serwera lub bazy danych.


## <a name="introduction"></a>Wprowadzenie

Wszystkie samouczków możemy stawienia zbadać wykonanej do tej pory pracy wyłącznie z danych tekstowych. Jednak wiele aplikacji ma modeli danych, które przechwytują tekst i danych binarnych. Witryny dating online mogą zezwalać użytkownikom na przekazywanie obrazu do skojarzenia z swój profil. Rekrutacji witryny sieci Web mogą zezwolić użytkownikom na przekazywanie ich wznowienia jako dokument programu Microsoft Word czy PDF.

Praca z danych binarnych dodaje nowy zestaw wyzwania. Firma Microsoft należy określić sposób przechowywania danych binarnych w aplikacji. Interfejs używany do wstawianie nowych rekordów ma zostać zaktualizowany do Zezwalaj użytkownikom na przekazywanie pliku z komputera i dodatkowych czynności musi być przełączona do wyświetlania lub zapewniają sposób pobierania rekordu s skojarzone dane binarne. W tym samouczku i dalej trzech firma Microsoft będzie Poznaj jak hurdle te problemy. Na końcu tego samouczka będziesz nawiązaliśmy funkcjonalnej aplikacji, które kojarzy obrazów i plików PDF broszura z każdej kategorii. W tym samouczku określonego firma Microsoft będzie Spójrz na różnych technik do przechowywania danych binarnych i Eksploruj jak umożliwić użytkownikom na przekazywanie plików z komputera i zostały zapisane w systemie plików s serwera sieci web.

> [!NOTE]
> Dane binarne, który jest częścią modelu danych s aplikacji jest czasami nazywany [obiektu BLOB](http://en.wikipedia.org/wiki/Binary_large_object), akronim dużego obiektu binarnego. W tych samouczkach I wybranego do użycia danych binarnych terminologii, mimo że termin obiektu BLOB jest równoważna.


## <a name="step-1-creating-the-working-with-binary-data-web-pages"></a>Krok 1: Tworzenie pracy ze stronami sieci Web dane binarne

Zanim zaczniemy Eksploruj wyzwania skojarzone z dodanie obsługi danych binarnych, umożliwiają s najpierw Poświęć chwilę, aby utworzyć w naszym projekt witryny sieci Web, które będą potrzebne dla tego samouczka i dalej trzy stron ASP.NET. Rozpocznij od dodania nowy folder o nazwie `BinaryData`. Następnie dodaj do tego folderu, upewniając się skojarzyć każdą stronę z następujących stron ASP.NET `Site.master` strony głównej:

- `Default.aspx`
- `FileUpload.aspx`
- `DisplayOrDownloadData.aspx`
- `UploadInDetailsView.aspx`
- `UpdatingAndDeleting.aspx`


![Dodawanie stron ASP.NET binarne samouczki związanych z danymi](uploading-files-vb/_static/image1.gif)

**Rysunek 1**: Dodawanie stron ASP.NET binarne samouczki związanych z danymi


Podobnie jak w innych folderach `Default.aspx` w `BinaryData` folderu spowoduje wyświetlenie listy samouczków w sekcji. Odwołania, który `SectionLevelTutorialListing.ascx` kontrola użytkownika zapewnia tę funkcję. W związku z tym Dodaj formant użytkownika `Default.aspx` przeciągając je z Eksploratora rozwiązań na stronę s widoku Projekt.


[![Dodaj kontrolkę użytkownika SectionLevelTutorialListing.ascx do Default.aspx](uploading-files-vb/_static/image2.gif)](uploading-files-vb/_static/image1.png)

**Rysunek 2**: Dodaj `SectionLevelTutorialListing.ascx` formantu użytkownika `Default.aspx` ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](uploading-files-vb/_static/image2.png))


Na koniec Dodaj te strony jako wpisów `Web.sitemap` pliku. W szczególności, Dodaj następujący kod po Enhancing widoku GridView `<siteMapNode>`:


[!code-xml[Main](uploading-files-vb/samples/sample1.xml)]

Po zaktualizowaniu `Web.sitemap`, Poświęć chwilę, aby wyświetlić witrynę sieci Web samouczki, za pośrednictwem przeglądarki. Menu po lewej stronie zawiera teraz elementów pracy z samouczkami danych binarnych.


![Mapy witryny zawiera teraz wpisy dotyczące pracy z samouczkami dane binarne](uploading-files-vb/_static/image3.gif)

**Rysunek 3**: mapy witryny zawiera teraz wpisy dotyczące pracy z samouczkami dane binarne


## <a name="step-2-deciding-where-to-store-the-binary-data"></a>Krok 2: Określanie miejsca do przechowywania danych binarnych

Dane binarne skojarzone z modelem danych s aplikacji mogą być przechowywane w jednym z dwóch miejscach: w systemie plików serwera s sieci web z odwołaniem do plików przechowywanych w bazie danych. lub bezpośrednio z poziomu bazy danych (patrz rysunek 4). Każdy ma własny zestaw zalet i wad i uzasadnia szczegółowe omówienie.


[![Dane binarne mogą być przechowywane w systemie plików lub bezpośrednio w bazie danych](uploading-files-vb/_static/image4.gif)](uploading-files-vb/_static/image3.png)

**Rysunek 4**: dane binarne mogą być przechowywane w systemie plików lub bezpośrednio w bazie danych ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](uploading-files-vb/_static/image4.png))


Załóżmy, firma chce rozszerzyć bazę danych Northwind, aby skojarzyć obrazu z każdego produktu. Jedną z opcji będzie przechowywania plików obrazu w systemie plików serwera s sieci web i rejestrowania ścieżki w `Products` tabeli. Z tej metody, d dodamy `ImagePath` kolumny `Products` tabeli typu `varchar(200)`, np. Gdy użytkownik przekazać obraz dla Chai, obraz mogą być przechowywane w systemie plików serwera s sieci web w `~/Images/Tea.jpg`, gdzie `~` reprezentuje ścieżkę fizyczną aplikacji s. Oznacza to, że jeśli na ścieżkę fizyczną katalogu głównego witryny sieci web `C:\Websites\Northwind\`, `~/Images/Tea.jpg` jest odpowiednikiem `C:\Websites\Northwind\Images\Tea.jpg`. Po przekazaniu pliku obrazu, d modyfikacjom w rekordzie Chai `Products` tabeli, aby jego `ImagePath` ścieżka nowy obraz odwołania do kolumny. Firma Microsoft może używać `~/Images/Tea.jpg` lub po prostu `Tea.jpg` Jeśli zdecydowaliśmy się, że wszystkie obrazy produktu zostanie umieszczony w aplikacji s `Images` folderu.

Główne zalety przechowywania danych binarnych w systemie plików są następujące:

- **Łatwość wdrażania** jako zajmiemy się wkrótce, przechowywania i pobierania danych binarnych przechowywane bezpośrednio w bazie danych wymaga nieco większej ilości kodu niż podczas pracy z danymi za pomocą systemu plików. Ponadto w kolejności dla użytkownika wyświetlić lub pobrać dane binarne muszą one być przedstawione przy użyciu adresu URL do tych danych. Jeśli dane znajdują się w systemie plików s serwera sieci web, adres URL jest prosta. Jeśli dane są przechowywane w bazie danych, jednak strony sieci web potrzeb do utworzenia, które pobiera i zwracać dane z bazy danych.
- **Szerszy dostęp do danych binarnych** danych binarnych mogą muszą być dostępne dla innych usług lub aplikacji, te, które nie pobierają dane z bazy danych. Na przykład obrazów skojarzonych z każdego produktu może również muszą być dostępne dla użytkowników za pomocą [FTP](http://en.wikipedia.org/wiki/File_Transfer_Protocol), w którym to przypadku d chcemy, aby przechowywać dane binarne w systemie plików.
- **Wydajność** Jeśli danych binarnych jest przechowywana w systemie plików, żądanie i sieci przeciążona między serwerem bazy danych i serwera sieci web będzie mniejsza niż Jeśli bezpośrednio w bazie danych są przechowywane dane binarne.

Główną wadą przechowywania danych binarnych w systemie plików jest, że zapewnia oddzielenie danych z bazy danych. Usunięcie rekordu z `Products` tabeli, skojarzony plik w systemie plików serwera s sieci web nie zostanie automatycznie usunięta. Firma Microsoft musi zapisu dodatkowego kodu do usuwania pliku lub systemu plików będzie zapełnić nieużywane, oddzielone pliki. Ponadto podczas tworzenia kopii zapasowej bazy danych, firma Microsoft musi upewnij się, że tworzyć kopie zapasowe skojarzone dane binarne w systemie plików, a także. Przenoszenie bazy danych do innej lokacji lub serwera stwarza podobne wyzwania.

Alternatywnie dane binarne mogą być przechowywane bezpośrednio w bazie danych programu Microsoft SQL Server 2005, tworząc kolumny typu `varbinary`. Podobnie jak z innymi typami danych o zmiennej długości, można określić maksymalną długość danych binarnych, jakie mogą być przechowywane w tej kolumnie. Na przykład zarezerwować maksymalnie 5000 bajtów użyć `varbinary(5000)`; `varbinary(MAX)` pozwala uzyskać maksymalny rozmiar około 2 GB.

Główną zaletą przechowywania danych binarnych bezpośrednio w bazie danych jest ścisłej sprzężenie dane binarne rekordów bazy danych. To znacznie upraszcza zadania administracyjne bazy danych, takich jak tworzenie kopii zapasowych lub przenoszenie bazy danych do innej lokacji lub serwera. Ponadto usunięcie rekordu automatycznie usuwa odpowiednie dane binarne. Istnieje więcej niewielkie zalet przechowywania danych binarnych w bazie danych. Zobacz [przechowywania binarne pliki bezpośrednio w bazie danych przy użyciu składnika ASP.NET 2.0](http://aspnet.4guysfromrolla.com/articles/120606-1.aspx) dla bardziej szczegółowym omówieniem.

> [!NOTE]
> W programie Microsoft SQL Server 2000 i wcześniejszych wersjach `varbinary` maksymalny limit 8000 bajtów ma typ danych. Aby przechowywać maksymalnie 2 GB danych binarnych [ `image` — typ danych](https://msdn.microsoft.com/library/ms187993.aspx) należy zamiast tego należy użyć. Z dodatkiem `MAX` w programie SQL Server 2005, jednak `image` — typ danych jest przestarzała. Go s nadal obsługiwane dla zapewnienia zgodności, ale firma Microsoft poinformowała, który `image` — typ danych zostanie usunięta w przyszłych wersjach programu SQL Server.


Jeśli pracujesz z starsze modelu danych może zostać wyświetlony `image` — typ danych. Bazy danych Northwind s `Categories` tabela ma `Picture` kolumny, która może służyć do przechowywania danych binarnych z pliku obrazu dla kategorii. Ponieważ bazy danych Northwind ma jego katalogów głównych programu Microsoft Access i wcześniejszych wersjach programu SQL Server, ta kolumna jest typu `image`.

Dla tego samouczka i dalej trzech użyjemy obu podejść. `Categories` Tabela ma już `Picture` kolumny do przechowywania zawartości binarnej obrazu dla kategorii. Dodamy dodatkowe kolumny `BrochurePath`, do przechowywania w systemie plików serwera s sieci web, który może służyć do zapewnienia jakość wydruku, dopracowany Omówienie kategorii ścieżka do pliku PDF.

## <a name="step-3-adding-thebrochurepathcolumn-to-thecategoriestable"></a>Krok 3: Dodawanie`BrochurePath`kolumnę`Categories`tabeli

Obecnie ma tylko cztery kolumny w tabeli Kategorie: `CategoryID`, `CategoryName`, `Description`, i `Picture`. Oprócz tych pól należy dodać nowy wskazującą broszura kategorii s (jeśli istnieje). Aby dodać tę kolumnę, przejdź do Eksploratora serwera na przechodzenie do tabel, kliknij prawym przyciskiem myszy `Categories` tabeli i wybierz polecenie Otwórz definicji tabeli (patrz rysunek 5). Jeśli nie ma Eksploratora serwera, wywołaj, wybierając opcję Eksploratora serwera z menu Widok lub kliknij przycisk Ctrl + Alt + S.

Dodaj nową `varchar(200)` kolumny `Categories` tabeli o nazwie `BrochurePath` i umożliwia `NULL` s i kliknij ikonę Zapisz (lub kliknij przycisk Ctrl + S).


[![Dodaj kolumnę BrochurePath do tabeli Kategorie](uploading-files-vb/_static/image5.gif)](uploading-files-vb/_static/image5.png)

**Rysunek 5**: Dodaj `BrochurePath` kolumny `Categories` tabeli ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](uploading-files-vb/_static/image6.png))


## <a name="step-4-updating-the-architecture-to-use-thepictureandbrochurepathcolumns"></a>Krok 4: Aktualizowanie architektury do użycia`Picture`i`BrochurePath`kolumn

`CategoriesDataTable` w warstwie dostępu do danych (DAL) aktualnie ma cztery `DataColumn` s zdefiniowany: `CategoryID`, `CategoryName`, `Description`, i `NumberOfProducts`. Gdy firma Microsoft zaprojektowanych tego elementu DataTable w [tworzenie Warstwa dostępu do danych](../introduction/creating-a-data-access-layer-vb.md) samouczek, `CategoriesDataTable` miał tylko pierwsze trzy kolumny; `NumberOfProducts` kolumna została dodana w [wzorzec/szczegół za pomocą punktowane Lista rekordów wzorca z DataList szczegóły](../filtering-scenarios-with-the-datalist-and-repeater/master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb.md) samouczka.

Zgodnie z opisem w *tworzenie Warstwa dostępu do danych*, DataTables w zestawie danych wpisane tworzą obiektów biznesowych. TableAdapters są zobowiązani do komunikowania się z bazą danych i wypełniania obiektów biznesowych z wynikami zapytania. `CategoriesDataTable` Jest wypełniana `CategoriesTableAdapter`, który ma trzy metody pobierania danych:

- `GetCategories()` wykonuje zapytanie głównego s TableAdapter i zwraca `CategoryID`, `CategoryName`, i `Description` pól wszystkich rekordów w `Categories` tabeli. Główne zapytanie jest używana przez generowanych automatycznie `Insert` i `Update` metody.
- `GetCategoryByCategoryID(categoryID)` Zwraca `CategoryID`, `CategoryName`, i `Description` pola kategorii, których `CategoryID` jest równe *categoryID*.
- `GetCategoriesAndNumberOfProducts()` -Zwraca `CategoryID`, `CategoryName`, i `Description` pól dla wszystkich rekordów w `Categories` tabeli. Używa również podzapytania w celu Zwróć liczbę skojarzone z każdej kategorii produktów.

Powiadomienie, że żaden z tych zapytań powrotu `Categories` tabeli s `Picture` lub `BrochurePath` kolumn; ani ma `CategoriesDataTable` podaj `DataColumn` s dla tych pól. Aby pracować z obrazu i `BrochurePath` właściwości, należy najpierw dodać je do `CategoriesDataTable` , a następnie zaktualizować `CategoriesTableAdapter` służącą do zwracania tych kolumn.

## <a name="adding-thepictureandbrochurepathdatacolumn-s"></a>Dodawanie`Picture`i`BrochurePath``DataColumn` s

Rozpocznij od dodania tych dwóch kolumn, aby `CategoriesDataTable`. Kliknij prawym przyciskiem myszy `CategoriesDataTable` nagłówka s, z menu kontekstowego wybierz polecenie Dodaj, a następnie wybierz opcję kolumny. Spowoduje to utworzenie nowego `DataColumn` w elemencie DataTable o nazwie `Column1`. Zmień nazwę tej kolumny `Picture`. W oknie właściwości ustaw `DataColumn` s `DataType` właściwości `System.Byte[]` (nie jest to opcja na liście rozwijanej; należy go wpisać).


[![Tworzenie obrazu o nazwie DataColumn, których typ danych jest System.Byte]](uploading-files-vb/_static/image6.gif)](uploading-files-vb/_static/image7.png)

**Rysunek 6**: tworzenie `DataColumn` nazwane `Picture` których `DataType` jest `System.Byte[]` ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](uploading-files-vb/_static/image8.png))


Dodaj inny `DataColumn` do elementu DataTable, nazw `BrochurePath` przy użyciu domyślnego `DataType` wartość (`System.String`).

## <a name="returning-thepictureandbrochurepathvalues-from-the-tableadapter"></a>Zwracanie`Picture`i`BrochurePath`wartości z TableAdapter

Z tych dwóch `DataColumn` s dodane do `CategoriesDataTable`, możemy re gotowy do aktualizacji `CategoriesTableAdapter`. Firma Microsoft może mieć obie te wartości kolumny zwracane w głównym zapytaniu TableAdapter, ale to będzie przywrócić dane binarne zawsze `GetCategories()` wywołano metodę. Zamiast tego let s zaktualizować główne zapytanie TableAdapter, aby przywrócić `BrochurePath` i Utwórz metodę pobierania dodatkowych danych, która zwraca określonej kategorii s `Picture` kolumny.

Aby zaktualizować główne zapytanie TableAdapter, kliknij prawym przyciskiem myszy `CategoriesTableAdapter` s nagłówka i wybierz opcję Konfiguruj z menu kontekstowego. Spowoduje to wyświetlenie Kreatora konfiguracji adaptera tabeli, które firma Microsoft Zapisz w liczbę ostatnich samouczki. Zaktualizuj zapytanie, aby przywrócić `BrochurePath` i kliknij przycisk Zakończ.


[![Aktualizacja listy kolumn w instrukcji SELECT również zwraca BrochurePath](uploading-files-vb/_static/image7.gif)](uploading-files-vb/_static/image9.png)

**Rysunek 7**: Lista kolumn w aktualizacji `SELECT` instrukcji również zwraca `BrochurePath` ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](uploading-files-vb/_static/image10.png))


Korzystając z instrukcji SQL ad hoc TableAdapter, aktualizowania listy kolumn w głównym zapytaniu aktualizuje listy kolumn dla wszystkich `SELECT` zapytania metody TableAdapter. Oznacza to, że `GetCategoryByCategoryID(categoryID)` metoda została zaktualizowana, aby zwrócić `BrochurePath` kolumny, która może być firma Microsoft przeznaczone. Jednak również zaktualizować listy kolumn w `GetCategoriesAndNumberOfProducts()` metody usunięcie podzapytania, która zwraca liczbę produktów dla każdej kategorii! W związku z tym należy zaktualizować tę metodę s `SELECT` zapytania. Kliknij prawym przyciskiem myszy `GetCategoriesAndNumberOfProducts()` metody, wybierz opcję Konfiguruj i przywrócić `SELECT` zapytania do oryginalnej wartości:


[!code-sql[Main](uploading-files-vb/samples/sample2.sql)]

Następnie należy utworzyć nowej metody TableAdapter, która zwraca wartość określonej kategorii s `Picture` wartość w kolumnie. Kliknij prawym przyciskiem myszy `CategoriesTableAdapter` s nagłówka i wybierz opcję Dodaj zapytanie, aby uruchomić Kreatora konfiguracji zapytania TableAdapter. Pierwszym krokiem ten kreator zapyta, nam Jeśli chcemy danych zapytania przy użyciu instrukcji SQL ad hoc nowy przechowywane procedury lub jednego z istniejących. Wybierz instrukcji SQL użyj, a następnie kliknij przycisk Dalej. Ponieważ firma Microsoft będzie zwracać wiersza, wybierz polecenie SELECT, która zwraca wiersze opcja w drugim kroku.


[![Wybierz instrukcji SQL użyj opcji](uploading-files-vb/_static/image8.gif)](uploading-files-vb/_static/image11.png)

**Rysunek 8**: Wybierz instrukcji SQL użyj opcji ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](uploading-files-vb/_static/image12.png))


[![Ponieważ zwraca rekord z tabeli Kategorie, wybierz OPCJĘ wybierz, która zwraca wiersze](uploading-files-vb/_static/image9.gif)](uploading-files-vb/_static/image13.png)

**Rysunek 9**: ponieważ zwraca rekord z tabeli Kategorie, wybierz pozycję Wybierz, która zwraca wiersze ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](uploading-files-vb/_static/image14.png))


W trzecim kroku wprowadź następujące zapytanie SQL, a następnie kliknij przycisk Dalej:


[!code-sql[Main](uploading-files-vb/samples/sample3.sql)]

Ostatnim krokiem jest wybranie nazwy dla nowej metody. Użyj `FillCategoryWithBinaryDataByCategoryID` i `GetCategoryWithBinaryDataByCategoryID` wypełnienia DataTable i przywrócenie elementu DataTable wzorców, odpowiednio. Kliknij przycisk Zakończ, aby zakończyć pracę kreatora.


[![Wybierz nazwy dla metody TableAdapter s](uploading-files-vb/_static/image10.gif)](uploading-files-vb/_static/image15.png)

**Na rysunku nr 10**: Wybierz nazwy dla Obiekt TableAdapter s metod ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](uploading-files-vb/_static/image16.png))


> [!NOTE]
> Po zakończeniu pracy Kreatora konfiguracji zapytania karty tabeli może wyświetlić okno dialogowe informujące, że nowy tekst polecenia zwraca dane ze schematem inny niż schemat głównego zapytania. Krótko mówiąc, Kreator jest zauważyć, że główne zapytanie TableAdapter s `GetCategories()` zwraca innym schematem niż ten, który został właśnie utworzony. Ale jest firma Microsoft ma, dlatego możesz pominąć ten komunikat.


Ponadto należy pamiętać, że jeśli używasz ad hoc instrukcji SQL i użyj kreatora, aby zmienić TableAdapter s główne zapytanie w pewnym momencie nowsze w czasie, zmodyfikuje ona `GetCategoryWithBinaryDataByCategoryID` metody s `SELECT` instrukcja s listy kolumn, aby zawierało tylko te kolumny z główne zapytanie (to znaczy spowoduje usunięcie `Picture` kolumny z kwerendy). Należy ręcznie zaktualizować listę kolumny, aby przywrócić `Picture` kolumny, podobnie jak robiliśmy z `GetCategoriesAndNumberOfProducts()` metody wcześniej w tym kroku.

Po dodaniu dwóch `DataColumn` s, aby `CategoriesDataTable` i `GetCategoryWithBinaryDataByCategoryID` metodę `CategoriesTableAdapter`, te klasy w Projektancie obiektów DataSet wpisane powinna wyglądać zrzut ekranu w rysunek 11.


![Projektant obiektów DataSet zawiera nowe kolumny i — metoda](uploading-files-vb/_static/image11.gif)

**Rysunek 11**: Projektant obiektów DataSet zawiera nowe kolumny i — metoda


## <a name="updating-the-business-logic-layer-bll"></a>Aktualizowanie warstwy logiki biznesowej (logiki warstwy Biznesowej)

W przypadku DAL zaktualizowane, wszystkie te pozostaje jest rozszerzyć firm logiki warstwy (logiki warstwy Biznesowej) do uwzględnienia metody dla nowego `CategoriesTableAdapter` metody. Dodaj następującą metodę do `CategoriesBLL` klasy:


[!code-vb[Main](uploading-files-vb/samples/sample4.vb)]

## <a name="step-5-uploading-a-file-from-the-client-to-the-web-server"></a>Krok 5: Przekazywania plików z klienta do serwera sieci Web

Podczas zbierania danych binarnych, często to dane są dostarczane przez użytkownika końcowego. Do przechwytywania te informacje, użytkownik musi mieć możliwość przekazywania plików z komputera z serwerem sieci web. Następnie dane przekazane musi można zintegrować z modelu danych, co może oznaczać zapisać plik w systemie plików s serwera sieci web i dodanie ścieżki do pliku w bazie danych lub zapisywanie zawartości binarnej bezpośrednio do bazy danych. W tym kroku wyjaśniono, jak zezwolić użytkownikowi na przekazywanie plików z komputera z serwerem. W następnym samouczku firma Microsoft będzie Włącz wymagające uwagi do integracji przekazanego pliku modelu danych.

Platforma ASP.NET 2.0 nowego [kontrolka sieci Web z przekazywaniem plików](https://msdn.microsoft.com/library/ms227677(VS.80).aspx) udostępnia mechanizm dla użytkowników wysłać plik z komputera z serwerem sieci web. Renderuje kontroli przekazywaniem plików jako `<input>` element którego `type` atrybut jest ustawiony w pliku, który przeglądarki wyświetlane jako pole tekstowe z przycisku Przeglądaj. Kliknięcie przycisku Przeglądaj wywołuje okno dialogowe, z którego użytkownik może wybrać plik. Gdy formularz jest przesyłana z powrotem, zawartość wybranego pliku s są wysyłane wraz z ogłaszania zwrotnego. Po stronie serwera informacje o przekazanego pliku jest dostępna za pośrednictwem właściwości s przekazywaniem plików.

Aby zademonstrować przekazywania plików, otwórz `FileUpload.aspx` strony `BinaryData` , przeciągnij formant przekazywaniem plików z przybornika do projektanta i ustaw kontrolę s `ID` właściwości `UploadTest`. Następnie dodaj kontrolkę przycisku Web ustawienie jej `ID` i `Text` właściwości, aby `UploadButton` i przekazywanie pliku wybrane, odpowiednio. Na koniec umieścić formantu etykiety Web poniżej przycisk Wyczyść jego `Text` właściwości i zestaw jej `ID` właściwości `UploadDetails`.


[![Dodawanie formantu przekazywaniem plików do strony ASP.NET](uploading-files-vb/_static/image12.gif)](uploading-files-vb/_static/image17.png)

**Rysunek 12**: Dodawanie formantu przekazywaniem plików do strony ASP.NET ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](uploading-files-vb/_static/image18.png))


Rysunek 13 przedstawiono tę stronę, podczas wyświetlania za pośrednictwem przeglądarki. Należy pamiętać, że kliknięcie przycisku Przeglądaj wywołuje okno dialogowe wyboru pliku, co pozwala na pobranie plików z komputera. Po wybraniu pliku kliknięcie przycisku przekazać wybranego pliku powoduje odświeżenie strony, która wysyła zawartość binarną s wybrany plik do serwera sieci web.


[![Użytkownik może wybrać plik do przekazania z komputera z serwerem](uploading-files-vb/_static/image13.gif)](uploading-files-vb/_static/image19.png)

**Rysunek 13**: użytkownik może wybrać plik do przekazania z komputera z serwerem ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](uploading-files-vb/_static/image20.png))


Strony przekazany plik można zapisać w systemie plików lub jego dane binarne mogą pracować z bezpośrednio za pomocą strumienia. Na przykład umożliwić tworzenie s `~/Brochures` folder i zapisać rozmiar przesłanego pliku. Rozpocznij od dodania `Brochures` folderu do lokacji jako podfolder katalogu głównego. Następnie należy utworzyć programu obsługi zdarzeń dla `UploadButton` s `Click` zdarzeń i Dodaj następujący kod:


[!code-vb[Main](uploading-files-vb/samples/sample5.vb)]

Kontrola przekazywaniem plików zapewnia szereg właściwości przekazywane dane. Na przykład [ `HasFile` właściwości](https://msdn.microsoft.com/library/system.web.ui.webcontrols.fileupload.hasfile.aspx) wskazuje, czy plik został przekazany przez użytkownika, gdy [ `FileBytes` właściwość](https://msdn.microsoft.com/library/system.web.ui.webcontrols.fileupload.filebytes.aspx) zapewnia dostęp do danych binarnych przekazane jako tablicę bajtów. `Click` Uruchamia program obsługi zdarzeń przez zapewnienie, że plik został przekazany. Jeśli plik został przekazany, etykieta zawiera nazwę przekazanego pliku, jego rozmiar w bajtach i jej typ zawartości.

> [!NOTE]
> Aby upewnić się, że użytkownik przekazuje plik można sprawdzić `HasFile` właściwości i wyświetlić ostrzeżenie, jeśli jego s `False`, lub może używać [kontroli RequiredFieldValidator](https://quickstarts.asp.net/QuickStartv20/aspnet/doc/validation/default.aspx) zamiast tego.


Przekazywaniem plików s `SaveAs(filePath)` zapisuje przekazanego pliku do określonego *filePath*. *filePath* musi być *ścieżka fizyczna* (`C:\Websites\Brochures\SomeFile.pdf`) zamiast *wirtualnego* *ścieżki* (`/Brochures/SomeFile.pdf`). [ `Server.MapPath(virtPath)` Metody](https://msdn.microsoft.com/library/system.web.httpserverutility.mappath.aspx) przyjmuje ścieżkę wirtualną i zwraca jego odpowiedniego ścieżkę fizyczną. Ścieżka wirtualna jest w tym miejscu `~/Brochures/fileName`, gdzie *fileName* jest nazwą przekazanego pliku. Zobacz [przy użyciu Server.MapPath](http://www.4guysfromrolla.com/webtech/121799-1.shtml) Aby uzyskać więcej informacji na temat ścieżek wirtualnych i fizycznych `Server.MapPath`.

Po zakończeniu `Click` program obsługi zdarzeń, Poświęć chwilę, aby przetestować strony w przeglądarce. Kliknij przycisk Przeglądaj i wybierz plik z dysku twardego, a następnie kliknij przycisk Przekaż plik zaznaczony. Wysyła zawartość wybranego pliku ogłaszania zwrotnego do serwera sieci web, który następnie spowoduje wyświetlenie informacji o pliku przed zapisaniem do `~/Brochures` folderu. Po przekazaniu pliku, wróć do programu Visual Studio i kliknij przycisk Odśwież w Eksploratorze rozwiązań. Plik, który właśnie został przekazany w folderze ~/Brochures powinien być widoczny!


[![EvolutionValley.jpg plik został przekazany na serwer sieci Web](uploading-files-vb/_static/image14.gif)](uploading-files-vb/_static/image21.png)

**Rysunek 14**: plik `EvolutionValley.jpg` został przekazany na serwer sieci Web ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](uploading-files-vb/_static/image22.png))


![EvolutionValley.jpg został zapisany w folderze ~/Brochures](uploading-files-vb/_static/image15.gif)

**Rysunek 15**: `EvolutionValley.jpg` zapisano `~/Brochures` folderu


## <a name="subtleties-with-saving-uploaded-files-to-the-file-system"></a>Precyzyjnie z zapisywaniem przekazanych plików do systemu plików

Istnieje kilka precyzyjnie, które muszą być kierowane podczas zapisywania przekazywania plików do systemu plików s serwera sieci web. Pierwszy, że s problem zabezpieczeń. Aby zapisać plik w systemie plików, kontekstu zabezpieczeń, w którym jest wykonywany strony ASP.NET musi mieć uprawnienia do zapisu. Sieci Web ASP.NET Development Server działa w kontekście konta użytkownika. Jeśli używasz s Microsoft Internet Information Services (IIS) jako serwera sieci web, kontekst zabezpieczeń zależy od wersji usług IIS i jego konfiguracji.

Zapisywanie plików w systemie plików innym wyzwaniem problemem nazw plików. Obecnie naszą stronę zapisuje pliki przekazane do `~/Brochures` katalogu przy użyciu takiej samej nazwie jak plik na komputerze klienckim s. Jeśli użytkownik A prześle broszurę o nazwie `Brochure.pdf`, plik zostanie zapisany jako `~/Brochure/Brochure.pdf`. Ale co zrobić, jeśli jakimś nowsze B użytkownika przekazuje plik broszura różnych, która ma taką samą nazwę (`Brochure.pdf`)? Kod mamy teraz użytkownika s pliku zostaną zastąpione przekazywania s B użytkownika.

Istnieje szereg technik rozwiązywania konfliktów nazw plików. Jedną z opcji jest zakazać przekazywania pliku, jeśli istnieje już jeden o takiej samej nazwie. Z tej metody, podczas próby przekazania pliku o nazwie użytkownika B `Brochure.pdf`, system nie będzie Zapisz plik i wyświetlany komunikat informujący użytkownika B, Zmień nazwę pliku i spróbuj ponownie. Innym rozwiązaniem jest zapisanie pliku przy użyciu unikatowej nazwy pliku, który może być [Unikatowy identyfikator globalny (GUID)](http://en.wikipedia.org/wiki/Globally_Unique_Identifier) lub wartość z odpowiedniej bazy danych kolumny klucza podstawowego s rekordów (przy założeniu, że przekazywanie jest skojarzony z określonego wiersza w modelu danych). W następnym samouczku firma Microsoft będzie Poznaj szczegółowo te opcje.

## <a name="challenges-involved-with-very-large-amounts-of-binary-data"></a>Wyzwania związane z bardzo dużych ilości danych binarnych

Te samouczki zakładać, że dane binarne przechwycone jest niewielkie rozmiar. Praca z dużymi ilościami danych binarnych plików, które są kilka MB lub większy wprowadza wyzwania, które wykraczają poza zakres tego samouczka. Na przykład domyślnie ASP.NET odrzuci przekazywania więcej niż 4 MB, mimo że można również skonfigurować za pomocą [ `<httpRuntime>` elementu](https://msdn.microsoft.com/library/e1f13641.aspx) w `Web.config`. IIS nakłada za własną ograniczenia rozmiaru przekazywania plików. Zobacz [rozmiar pliku Przekaż IIS](http://vandamme.typepad.com/development/2005/09/iis_upload_file.html) Aby uzyskać więcej informacji. Ponadto czas potrzebny na przekazanie dużych plików może przekroczyć domyślny 110 sekund, przez program ASP.NET będzie czekać na żądanie. Istnieją problemy z pamięcią i wydajnością nasuwające się podczas pracy z dużymi plikami.

Formant przekazywaniem plików jest niepraktyczne przekazywania dużych plików. Jak zawartość pliku s są ogłaszany z serwerem, użytkownik końcowy patiently oczekiwania bez żadnych potwierdzenia postępuje ich przekazywania. Nie jest zbyt duża, wystąpił problem podczas pracy nad mniejsze pliki, które mogą być przekazywane w ciągu kilku sekund, ale może być przyczyną problemów podczas pracy nad większych plików, które może potrwać do przekazania. Istnieje wiele innych firm pliku przekazywania formantów, które są lepiej dostosowane do obsługi dużych przekazywania i wiele z tych dostawców zapewniają wskaźniki postępu i ActiveX Przekaż menedżerów istnieje profesjonalny bardziej środowisko użytkownika.

Jeśli aplikacja wymaga do obsługi dużych plików, należy dokładnie zbadać wyzwania i Znajdź odpowiedniego rozwiązania do potrzeb.

## <a name="summary"></a>Podsumowanie

Tworzenie aplikacji, która potrzebuje do przechwytywania danych binarnych zawiera wiele wyzwań. W tym samouczku będziemy przedstawione dwa pierwsze: Określanie miejsca do przechowywania danych binarnych, dzięki czemu użytkownika można przekazać zawartość binarną za pośrednictwem strony sieci web. W trzech kolejnych samouczki możemy Zobacz sposobu kojarzenia przekazywane dane z rekord w bazie danych, a także sposób wyświetlania danych binarnych obok pola danych jego tekstu.

Programowanie przyjemność!

## <a name="further-reading"></a>Dalsze informacje

Więcej informacji dotyczących tematów omówionych w tym samouczku można znaleźć w następujących zasobach:

- [Używanie typów danych dużych wartości](https://msdn.microsoft.com/library/ms178158.aspx)
- [Przewodniki Szybki Start przekazywaniem plików sterowania](https://quickstarts.asp.net/QuickStartv20/aspnet/doc/ctrlref/standard/fileupload.aspx)
- [Kontrolka serwerowa ASP.NET 2.0 przekazywaniem plików](http://www.wrox.com/WileyCDA/Section/id-292158.html)
- [Ciemny stronie przekazywania plików](http://www.aspnetresources.com/articles/dark_side_of_file_uploads.aspx)

## <a name="about-the-author"></a>Informacje o autorze

[Scott Bento](http://www.4guysfromrolla.com/ScottMitchell.shtml), autora siedmiu książek ASP/ASP.NET i twórcę z [4GuysFromRolla.com](http://www.4guysfromrolla.com), pracuje z technologii Microsoft Web od 1998. Scott działa jako niezależnego konsultanta trainer i składnika zapisywania. Jest jego najnowszej książki [ *Sams nauczyć się ASP.NET 2.0 w ciągu 24 godzin*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Piotr można uzyskać pod adresem [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) lub za pośrednictwem jego blog, który znajduje się w temacie [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Specjalne podziękowania dla

Ten samouczek serii zostało sprawdzone przez wiele recenzentów przydatne. Prowadzić osób dokonujących przeglądu, w tym samouczku zostały Teresa Murphy i Bernadette Leigh. Zainteresowani recenzowania Moje nadchodzących artykuły MSDN? Jeśli tak, Porzuć mnie linii w [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Poprzednie](updating-and-deleting-existing-binary-data-cs.md)
> [dalej](displaying-binary-data-in-the-data-web-controls-vb.md)
