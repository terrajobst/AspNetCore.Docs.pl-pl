---
uid: web-forms/overview/data-access/advanced-data-access-scenarios/using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb
title: Korzystanie z istniejących procedur składowanych dla elementów TableAdapter Typizowanego zestawu danych (VB) | Dokumentacja firmy Microsoft
author: rick-anderson
description: W poprzednim samouczku dowiedzieliśmy, jak wygenerować nowe procedury składowane za pomocą Kreatora TableAdapter. W tym samouczku będziemy Dowiedz się, jak ten sam obiekt TableAdapter...
ms.author: aspnetcontent
ms.date: 07/18/2007
ms.assetid: 2da25f6a-757e-4e7b-a812-1575288d8f7a
msc.legacyurl: /web-forms/overview/data-access/advanced-data-access-scenarios/using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb
msc.type: authoredcontent
ms.openlocfilehash: 586b46acf1df19cb1bdd56c4242e92d2019c7b8a
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/05/2018
ms.locfileid: "37805629"
---
<a name="using-existing-stored-procedures-for-the-typed-datasets-tableadapters-vb"></a>Korzystanie z istniejących procedur składowanych dla elementów TableAdapter Typizowanego zestawu danych (VB)
====================
przez [Bento Scott](https://twitter.com/ScottOnWriting)

[Pobierz program Code](http://download.microsoft.com/download/3/9/f/39f92b37-e92e-4ab3-909e-b4ef23d01aa3/ASPNET_Data_Tutorial_68_VB.zip) lub [Pobierz plik PDF](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/datatutorial68vb1.pdf)

> W poprzednim samouczku dowiedzieliśmy, jak wygenerować nowe procedury składowane za pomocą Kreatora TableAdapter. W tym samouczku dowie się, jak tym samym Kreatorze TableAdapter mogą współdziałać za pomocą istniejących procedur składowanych. Możemy również dowiedzieć się, jak można ręcznie dodać nowe procedury składowane do naszej bazie danych.


## <a name="introduction"></a>Wprowadzenie

W [poprzedni Samouczek](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb.md) widzieliśmy jak s wpisany zestaw danych TableAdapters może zostać skonfigurowany w taki sposób, aby użyć procedur składowanych na potrzeby dostępu do danych, a nie w trybie ad-hoc instrukcji języka SQL. W szczególności zbadaliśmy sposób automatycznego tworzenia tych procedurach składowanych Kreator TableAdapter. Podczas przenoszenia starszych aplikacji na platformie ASP.NET 2.0 lub podczas tworzenia witryny sieci Web programu ASP.NET 2.0 wokół istniejącego modelu danych, jest szansa, że baza danych zawiera już procedur składowanych, której potrzebujesz. Alternatywnie można utworzyć przechowywanych procedur, ręcznie lub za pomocą narzędzia do niektórych innych niż kreatora TableAdapter, która automatycznie generuje przechowywanych procedur.

W tym samouczku przedstawiony zostanie sposób konfigurowania TableAdapter używać istniejących procedur składowanych. Ponieważ bazy danych Northwind jest tylko niewielki zestaw wbudowanych procedur składowanych, również przyjrzymy się czynności, aby ręcznie dodać nowe procedury składowane w bazie danych za pośrednictwem środowiska programu Visual Studio. Rozpocznij pracę dzięki s!

> [!NOTE]
> W [opakowywanie modyfikacji bazy danych w ramach transakcji](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb.md) samouczek dodaliśmy metody do TableAdapter do obsługi transakcji (`BeginTransaction`, `CommitTransaction`i tak dalej). Alternatywnie transakcji można zarządzać wyłącznie w obrębie procedury składowanej, który nie wymaga żadnych modyfikacji kodu warstwy dostępu do danych. W tym samouczku przyjrzymy się polecenia T-SQL służącego do wykonania procedury składowanej s instrukcji w zakresie transakcji.


## <a name="step-1-adding-stored-procedures-to-the-northwind-database"></a>Krok 1: Dodawanie procedur składowanych w bazie danych Northwind

Programu Visual Studio ułatwia dodawanie nowych procedur składowanych do bazy danych. Umożliwiają s Dodaj nową procedurę składowaną z bazą danych Northwind, która zwraca wszystkie kolumny z `Products` tabeli dla tych, które mają konkretny `CategoryID` wartość. W oknie Eksploratora serwera rozwiń węzeł bazy danych Northwind, tak, aby jego folderów — diagramy baz danych, tabele, widoki i tak dalej — są wyświetlane. Jak widać widzieliśmy w poprzednim samouczku folderu procedur składowanych zawiera bazy danych s istniejących procedur składowanych. Aby dodać nową procedurę składowaną, kliknij prawym przyciskiem myszy folder procedur składowanych i wybierz opcję Dodaj nową procedurę przechowywaną z menu kontekstowego.


[![Kliknij prawym przyciskiem myszy Folder procedur składowanych i Dodaj nową procedurę składowaną](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image2.png)](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image1.png)

**Rysunek 1**: kliknij prawym przyciskiem myszy Folder procedur przechowywanych i Dodaj nową procedurę przechowywaną ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image3.png))


Jak pokazano na rysunku 1, wybierając opcję Dodaj nową procedurę przechowywaną spowoduje otwarcie okna skryptu w programie Visual Studio z konturem skrypt SQL potrzebne do utworzenia procedury składowanej. To nasze zadania do określania się ten skrypt i uruchomić go w tym momencie procedury składowanej zostaną dodane do bazy danych.

Wprowadź następujący skrypt:


[!code-sql[Main](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/samples/sample1.sql)]

Ten skrypt po wykonaniu doda nową procedurę składowaną w bazie danych Northwind, o nazwie `Products_SelectByCategoryID`. Tę procedurę składowaną przyjmuje jeden parametr wejściowy (`@CategoryID`, typu `int`) i zwraca wszystkie pola dla tych produktów z pasującą `CategoryID` wartość.

Do wykonania tej operacji `CREATE PROCEDURE` skryptu i dodać procedurę składowaną w bazie danych, kliknij ikonę Zapisz na pasku narzędzi lub naciśnij klawisze Ctrl + S. Po wykonaniu tej czynności odświeżania folderu procedur składowanych, procedurę składowaną przedstawiający nowo utworzony. Ponadto skryptów w oknie zmieni subtlety z `CREATE PROCEDURE dbo.Products_SelectProductByCategoryID` do `ALTER PROCEDURE` `dbo.Products_SelectProductByCategoryID`. `CREATE PROCEDURE` Dodaje nową procedurę składowaną w bazie danych, podczas gdy `ALTER PROCEDURE` aktualizuje istniejący zestaw. Od początku skryptu został zmieniony na `ALTER PROCEDURE`zmiany procedur przechowywanych danych wejściowych w instrukcji SQL lub parametrów i klikając ikonę Zapisz spowoduje zaktualizowanie procedury składowanej za pomocą tych zmian.

Na rysunku 2 przedstawiono program Visual Studio po `Products_SelectByCategoryID` procedura składowana została zapisana.


[![Products_SelectByCategoryID procedura składowana została dodana do bazy danych](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image5.png)](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image4.png)

**Rysunek 2**: procedura składowana `Products_SelectByCategoryID` został dodany do bazy danych ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image6.png))


## <a name="step-2-configuring-the-tableadapter-to-use-an-existing-stored-procedure"></a>Krok 2: Konfigurowanie TableAdapter do użycia istniejącą procedurę składowaną

Teraz, gdy `Products_SelectByCategoryID` procedura składowana została dodana do bazy danych, możemy skonfigurować nasz warstwy dostępu do danych, aby użyć tej procedury składowanej, gdy jeden z jego metod jest wywoływany. W szczególności firma Microsoft doda `GetProducstByCategoryID(<_i22_>categoryID)<!--_i22_-->` metody `ProductsTableAdapter` w `NorthwindWithSprocs` wpisany zestaw danych, który wywołuje `Products_SelectByCategoryID` przechowywane procedury, którą właśnie utworzyliśmy.

Zacznij od otwarcia `NorthwindWithSprocs` zestawu danych. Kliknij prawym przyciskiem myszy `ProductsTableAdapter` i wybierz polecenie Dodaj zapytanie, aby uruchomić Kreatora konfiguracji zapytania TableAdapter. W [poprzedni Samouczek](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb.md) postanowiliśmy mają TableAdapter, Utwórz nową procedurę składowaną dla nas. Na potrzeby tego samouczka, chcemy, aby powiązać nową metodę TableAdapter do istniejących `Products_SelectByCategoryID` procedury składowanej. W związku z tym opcję użycia istniejącej procedury składowanej z pierwszego kroku s kreatora, a następnie kliknij przycisk Dalej.


[![Wybierz opcję użycia istniejącej procedury składowanej — opcja](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image8.png)](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image7.png)

**Rysunek 3**: wybierz opcję Użyj istniejącej przechowywane procedury opcji ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image9.png))


Następujący ekran zawiera listy rozwijanej wypełniony bazy danych s procedur składowanych. Wybieranie procedury składowanej wyświetla jego parametrów wejściowych po lewej i pola danych zwracane (jeśli istnieją) po prawej stronie. Wybierz `Products_SelectByCategoryID` przechowywane procedury z listy i kliknij przycisk Dalej.


[![Wybierz Products_SelectByCategoryID procedury składowanej](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image11.png)](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image10.png)

**Rysunek 4**: Wybierz `Products_SelectByCategoryID` Stored Procedure ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image12.png))


Następny ekran najpierw zadaje nam jakiego rodzaju dane zwracane przez procedurę składowaną, a nasze odpowiedzi w tym miejscu określa typ zwracany przez metodę TableAdapter s. Na przykład, jeśli firma Microsoft wskazują, że dane tabelaryczne są zwracane, metoda zwróci `ProductsDataTable` wystąpienie wypełnione przy użyciu rekordów zwracanych przez procedurę składowaną. Z kolei jeśli wskazaliśmy, że ta procedura składowana zwraca pojedynczą wartość TableAdapter zwróci `Object` , jest przypisywana wartość w pierwszej kolumnie pierwszy rekord zwrócone przez procedurę składowaną.

Ponieważ `Products_SelectByCategoryID` procedura składowana ma zwracać wszystkie produkty, które należą do określonej kategorii, wybierz pierwszą odpowiedź — dane tabelaryczne — i kliknij przycisk Dalej.


[![Wskazuje, że procedura składowana ma zwracać dane tabelaryczne](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image14.png)](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image13.png)

**Rysunek 5**: wskazują, że procedura składowana zwraca dane tabelaryczne ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image15.png))


Pozostaje tylko do wskazania, co metoda wzorce do użycia oraz nazwy dotyczącej tych metod. Pozostaw zarówno wypełnienia DataTable i zwrócenia opcje DataTable zaznaczone, ale Zmień nazwę metody służące do `FillByCategoryID` i `GetProductsByCategoryID`. Następnie kliknij przycisk Dalej, aby przejrzeć podsumowanie zadań, które wykona Kreator. Jeśli wszystko jest poprawne, kliknij przycisk Zakończ.


[![Nazwa FillByCategoryID metod i GetProductsByCategoryID](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image17.png)](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image16.png)

**Rysunek 6**: Nazwa metody `FillByCategoryID` i `GetProductsByCategoryID` ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image18.png))


> [!NOTE]
> Metody TableAdapter, którą właśnie utworzyliśmy, `FillByCategoryID` i `GetProductsByCategoryID`, oczekiwany parametr wejściowy typu `Integer`. Wartość tego parametru wejściowego jest przekazywany do procedury składowanej za pośrednictwem jego `@CategoryID` parametru. Jeśli zmodyfikujesz `Products_SelectByCategory` przechowywane parametry procedury s, musisz również zaktualizować parametry dla tych metod TableAdapter. Zgodnie z opisem w [poprzedniego samouczka](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb.md), można to zrobić na dwa sposoby: przez ręczne dodawanie i usuwanie parametrów z kolekcji parametrów lub uruchamiając ponownie kreatora TableAdapter.


## <a name="step-3-adding-agetproductsbycategoryidcategoryidmethod-to-the-bll"></a>Krok 3. Dodawanie`GetProductsByCategoryID(categoryID)`metody LOGIKI

Za pomocą `GetProductsByCategoryID` metody DAL pełną, następnym krokiem jest zapewnienie dostępu do tej metody w warstwy logiki biznesowej. Otwórz `ProductsBLLWithSprocs` klasy plików i dodaj następującą metodę:


[!code-vb[Main](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/samples/sample2.vb)]

Ta metoda LOGIKI po prostu zwraca `ProductsDataTable` zwróciło `ProductsTableAdapter` s `GetProductsByCategoryID` metody. `DataObjectMethodAttribute` Atrybut zawiera metadane używane przez kreatora ObjectDataSource s Konfigurowanie źródła danych. W szczególności ta metoda pojawi się na liście rozwijanej wybierz kartę s.

## <a name="step-4-displaying-products-by-category"></a>Krok 4: Wyświetlanie produktów według kategorii

Aby przetestować nowo dodanych `Products_SelectByCategoryID` procedury składowanej oraz odpowiednich metod DAL i LOGIKI, umożliwiają tworzenie strony ASP.NET, która zawiera kontrolki DropDownList i GridView s. Metody DropDownList spowoduje wyświetlenie listy wszystkich kategorii w bazie danych podczas widoku GridView wyświetli produktów należących do wybranej kategorii.

> [!NOTE]
> Firma Microsoft interfejsy wzorzec/szczegół ve utworzone za pomocą kontrolek DROPDOWNLIST w poprzednich samouczkach. Aby uzyskać bardziej przyjrzeć się raportem wzorzec/szczegół implementacji, zobacz [wzorzec/szczegół filtrowanie przy użyciu kontrolki DropDownList](../masterdetail/master-detail-filtering-with-a-dropdownlist-vb.md) samouczka.


Otwórz `ExistingSprocs.aspx` stronie `AdvancedDAL` folder i przeciągnij kontrolki DropDownList z przybornika do projektanta. Ustaw DropDownList s `ID` właściwości `Categories` i jego `AutoPostBack` właściwość `True`. Następnie z jej tagu inteligentnego powiązać metody DropDownList nowe kontrolki ObjectDataSource, o nazwie `CategoriesDataSource`. Skonfigurować kontrolki ObjectDataSource pobiera dane z `CategoriesBLL` klasy s `GetCategories` metody. Ustawianie list rozwijanych w UPDATE, INSERT i usuwanie kart (Brak).


[![Pobieranie danych z metody GetCategories CategoriesBLL klasy s](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image20.png)](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image19.png)

**Rysunek 7**: pobieranie danych z `CategoriesBLL` klasy s `GetCategories` — metoda ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image21.png))


[![Ustaw list rozwijanych w UPDATE, INSERT i usuwanie kart (Brak)](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image23.png)](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image22.png)

**Rysunek 8**: Ustaw listy rozwijane w aktualizacji, WSTAWIANIA i usuwania karty (Brak) ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image24.png))


Po zakończeniu pracy Kreatora ObjectDataSource, skonfiguruj kontrolki DropDownList, aby wyświetlić `CategoryName` pola danych i korzystać z `CategoryID` pola jako `Value` dla każdego `ListItem`.

W tym momencie DropDownList i kontrolki ObjectDataSource s oznaczeniu deklaracyjnym powinien podobny do następującego:


[!code-aspx[Main](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/samples/sample3.aspx)]

Następnie przeciągnij GridView do konstruktora, umieszczając go poniżej metody DropDownList. Ustaw GridView s `ID` do `ProductsByCategory` i z jego tag inteligentny powiązać go do nowego elementu ObjectDataSource, o nazwie `ProductsByCategoryDataSource`. Konfigurowanie `ProductsByCategoryDataSource` ObjectDataSource do użycia `ProductsBLLWithSprocs` klasy go pobrać jego dane za pomocą `GetProductsByCategoryID(categoryID)` metody. Ponieważ ten GridView będą używane tylko do wyświetlania danych, ustaw list rozwijanych w UPDATE, INSERT i usuwanie kart (Brak) i kliknij przycisk Dalej.


[![Konfigurowanie kontrolki ObjectDataSource na korzystanie z klasy ProductsBLLWithSprocs](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image26.png)](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image25.png)

**Rysunek 9**: Konfigurowanie kontrolki ObjectDataSource do użycia `ProductsBLLWithSprocs` klasy ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image27.png))


[![Pobieranie danych z metody GetProductsByCategoryID(categoryID)](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image29.png)](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image28.png)

**Na rysunku nr 10**: pobieranie danych z `GetProductsByCategoryID(categoryID)` — metoda ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image30.png))


Metody wybranej na karcie Wybierz oczekuje parametru, więc w ostatnim kroku kreatora nam wyświetla monit o podanie parametru źródła s. Zmień wartość na liście rozwijanej źródła parametru do kontroli, a następnie wybierz `Categories` formantu z listy rozwijanej ControlID. Kliknij przycisk Zakończ, aby zakończyć działanie kreatora.


[![Użyj metody DropDownList kategorie jako źródło categoryID parametru](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image32.png)](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image31.png)

**Rysunek 11**: Użyj `Categories` DropDownList jako źródło `categoryID` parametru ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image33.png))


Po zakończeniu pracy Kreatora ObjectDataSource programu Visual Studio spowoduje to dodanie BoundFields i CheckBoxField dla każdego pola danych produktu. Możesz dostosować te pola, zgodnie z potrzebami.

Odwiedź stronę za pośrednictwem przeglądarki. Przy wybranej kategorii Beverages strony i odpowiadających im produktów, które są wyświetlane w siatce. Zmiana listy rozwijanej do alternatywnego kategorii, jak rysunek 12 przedstawiono powoduje odświeżenie strony i ponownie ładuje siatki z produktami nowo wybranej kategorii.


[![Produkty z kategorii generuje są wyświetlane.](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image35.png)](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image34.png)

**Rysunek 12**: produkty z kategorii generuje są wyświetlane ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image36.png))


## <a name="step-5-wrapping-a-stored-procedure-s-statements-within-the-scope-of-a-transaction"></a>Krok 5: Zawijania instrukcji s procedurę składowaną w ramach zakresu transakcji

W [opakowywanie modyfikacji bazy danych w ramach transakcji](../working-with-batched-data/wrapping-database-modifications-within-a-transaction-vb.md) samouczku Omówiliśmy techniki serię instrukcji modyfikacji bazy danych w zakresie transakcji. Odwołania, który modyfikacje w obszarze parasola transakcji albo wszystkie powiedzie się lub wszystkie zakończone niepowodzeniem, gwarantując niepodzielność. Techniki za pomocą transakcji obejmują:

- Korzystanie z klas w `System.Transactions` przestrzeni nazw
- O warstwie dostępu do danych użycia klas ADO.NET, takich jak `SqlTransaction`, i
- Dodawanie poleceń języka T-SQL transakcji, bezpośrednio w ramach procedury składowanej

*Opakowywanie modyfikacji bazy danych w ramach transakcji* samouczek używane klasy ADO.NET w warstwy DAL. W pozostałej części tego samouczka analizuje sposób zarządzania transakcji za pomocą polecenia języka T-SQL z w procedurze składowanej.

Są trzy polecenia SQL klucza ręczny, zatwierdzanie i wycofywania transakcji `BEGIN TRANSACTION`, `COMMIT TRANSACTION`, i `ROLLBACK TRANSACTION`, odpowiednio. Jak podejście ADO.NET, podczas korzystania z transakcji z w ramach procedury składowanej, musimy się z następującym wzorcem:

1. Wskazuje początek transakcji.
2. Wykonaj instrukcje SQL, wchodzące w skład transakcji.
3. Jeśli występuje błąd w jednym z instrukcji z kroku nr 2 wycofywania transakcji.
4. Jeśli wszystkie instrukcje z kroku nr 2 zakończy się bez błędów, należy zatwierdzić transakcji.

Ten wzorzec może być implementowany w składni języka T-SQL przy użyciu następującego szablonu:


[!code-sql[Main](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/samples/sample4.sql)]

Szablon, który rozpoczyna się przez definiowanie `TRY...CATCH` block konstrukcję jesteś nowym użytkownikiem programu SQL Server 2005. Za pomocą `Try...Catch` bloków w języku Visual Basic SQL `TRY...CATCH` blok jest wykonywany instrukcji w `TRY` bloku. Jeśli każda instrukcja zgłasza błąd, natychmiast przekazaniu kontroli do `CATCH` bloku.

Jeśli nie ma żadnych błędów, wykonywania instrukcji SQL tej transakcji, korzeń `COMMIT TRANSACTION` instrukcji zatwierdza zmiany i wykonuje transakcję. Jeśli jednak jedno z oświadczeń powoduje wystąpienie błędu `ROLLBACK TRANSACTION` w `CATCH` bloku przywraca bazę danych do stanu przed rozpoczęciem transakcji. Procedura składowana także zgłasza błąd za pomocą [polecenie RAISERROR](https://msdn.microsoft.com/library/ms178592.aspx), co powoduje, że `SqlException` do wywołania w aplikacji.

> [!NOTE]
> Ponieważ `TRY...CATCH` bloku jest nowym składnikiem programu SQL Server 2005, powyższego szablonu nie będzie działać, jeśli używasz starszej wersji programu Microsoft SQL Server. Jeśli nie używasz programu SQL Server 2005, zapoznaj się z [zarządzania transakcji w procedur składowanych serwera SQL](http://www.4guysfromrolla.com/webtech/080305-1.shtml) dla szablonu, który będzie działać z innymi wersjami programu SQL Server.


Pozwól, s, spójrz na konkretny przykład. Ograniczenie klucza obcego istnieje między `Categories` i `Products` tabel, co oznacza, że każdy `CategoryID` pole `Products` tabeli musi być mapowane `CategoryID` wartość w `Categories` tabeli. Dowolna akcja, którą naruszyłoby to ograniczenie, np. podjęto próbę usunięcia kategorii, która jest skojarzona produktów, powoduje naruszenie ograniczenia klucza obcego. Aby to sprawdzić, ponownie przykład aktualizowanie i usuwanie istniejących danych binarnych w pracy z sekcji danych binarnych (`~/BinaryData/UpdatingAndDeleting.aspx`). Ta strona zawiera listę poszczególnych kategorii w systemie, wraz z przyciski edytowania i usuwania (zobacz rysunek 13), ale Jeśli spróbujesz usunąć kategorię, która jest skojarzona produktów — takich jak Beverages — usunięcie nie powiodło się z powodu naruszenia ograniczenia klucza obcego (zobacz rysunek 14).


[![Każda kategoria jest wyświetlany w widoku GridView z funkcją Edytuj i usuń przyciski](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image38.png)](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image37.png)

**Rysunek 13**: każdej kategorii są wyświetlane w widoku GridView z funkcją Edytuj i usuń przyciski ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image39.png))


[![Nie można usunąć kategorii, której istniejących produktów](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image41.png)](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image40.png)

**Rysunek 14**: nie można usunąć kategorii, której istniejących produktów ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image42.png))


Wyobraź sobie, jednak, że chcemy zezwolić na kategorie w celu usunięcia niezależnie od tego, czy mają jednak powiązane z produktami. Należy usunąć kategorię z produktami, Wyobraź sobie, że chcemy także usunięcie jego istniejących produktów (mimo że innym rozwiązaniem byłoby wystarczy ustawić dla swoich produktów `CategoryID` wartości `NULL`). Ta funkcja może być implementowane za pomocą reguł cascade ograniczenie klucza obcego. Alternatywnie można utworzyć procedurę składowaną, która akceptuje `@CategoryID` parametr wejściowy i wywołana jawnie usuwa wszystkie skojarzone produkty, a następnie określonej kategorii.

Nasze pierwsza próba procedury składowanej może wyglądać następująco:


[!code-sql[Main](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/samples/sample5.sql)]

Gdy ostatecznie spowoduje to usunięcie skojarzone produkty i kategorii, go nie jest to w obszarze parasola transakcji. Wyobraź sobie, że istnieje kilka innych ograniczenie klucza obcego na `Categories` zezwolili spowoduje usunięcie określonego `@CategoryID` wartość. Problem polega na tym, że w takim przypadku wszystkie produkty zostaną usunięte przed staramy się usunąć kategorię. Wynikiem jest, że dla kategorii, tę procedurę składowaną spowoduje usunięcie wszystkich jej produktów podczas gdy pozostała kategorii, ponieważ nadal ma powiązane rekordy w innej tabeli.

Jeśli procedura składowana zostały opakowane w zakresie transakcji, jednak usuwa do `Products` tabeli będzie można wycofać w przypadku nie można usunąć `Categories`. Poniższy skrypt procedury składowanej używa transakcji, aby zapewnić niepodzielność między tymi dwoma `DELETE` instrukcji:


[!code-sql[Main](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/samples/sample6.sql)]

Poświęć chwilę, aby dodać `Categories_Delete` przechowywane procedury z bazą danych Northwind. Odwołaj się do kroku 1 instrukcje dotyczące dodawania procedur składowanych do bazy danych.

## <a name="step-6-updating-thecategoriestableadapter"></a>Krok 6: Aktualizowanie`CategoriesTableAdapter`

Podczas gdy my będziemy dodano `Categories_Delete` procedurę składowaną w bazie danych, warstwy DAL jest obecnie skonfigurowany do używania instrukcji SQL zapytań ad-hoc przeprowadzić delete. Musimy zaktualizować `CategoriesTableAdapter` i poinformuj go do korzystania ze `Categories_Delete` procedurę składowaną w zamian.

> [!NOTE]
> Wcześniej w tym samouczku pracy przy użyciu `NorthwindWithSprocs` zestawu danych. Jednak, że zestaw danych zawiera tylko pojedynczy element `ProductsDataTable`, więc chcemy Praca z elementami roboczymi. W związku z tym, w pozostałej części tego samouczka I mówiąc o m Data Access Layer I odwołujące się do `Northwind` zestawu danych, który utworzyliśmy najpierw w [Tworzenie warstwy dostępu do danych](../introduction/creating-a-data-access-layer-vb.md) samouczka.


Otwórz zestaw danych Northwind, wybierz `CategoriesTableAdapter`, a następnie przejdź do okna właściwości. Wyświetla okno właściwości `InsertCommand`, `UpdateCommand`, `DeleteCommand`, i `SelectCommand` posługują się TableAdapter, a także jej nazwę oraz informacje o połączeniu. Rozwiń `DeleteCommand` właściwości, aby wyświetlić jego szczegóły. Jak pokazano na rysunku 15, `DeleteCommand` s `ComamndType` właściwość jest ustawiona na tekst, który powoduje, że będzie można wysyłać tekst `CommandText` właściwość jako kwerendy SQL zapytań ad-hoc.


![W Projektancie Aby wyświetlić jego właściwości w oknie dialogowym właściwości wybierz CategoriesTableAdapter](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image43.png)

**Rysunek 15**: Wybierz `CategoriesTableAdapter` w Projektancie Aby wyświetlić jego właściwości w oknie dialogowym właściwości


Aby zmienić te ustawienia, zaznacz tekst, (element DeleteCommand) w oknie dialogowym właściwości, a następnie wybierz (nowość) z listy rozwijanej. Spowoduje to wyczyszczenie ustawienia `CommandText`, `CommandType`, i `Parameters` właściwości. Następnym etapem jest skonfigurowanie `CommandType` właściwości `StoredProcedure` , a następnie wpisz nazwę procedury składowanej dla `CommandText` (`dbo.Categories_Delete`). W przypadku wprowadzenia się upewnić, że właściwości w tej kolejności — najpierw `CommandType` i następnie `CommandText` — Visual Studio będzie automatycznie wypełnić kolekcję parametrów. Jeśli nie podasz tych właściwości w podanej kolejności, należy ręcznie dodać parametry przez Edytor kolekcji parametrów. W obu przypadkach go s, aby kliknąć wielokropek we właściwości parametrów, aby wyświetlić się Edytor kolekcji parametrów, aby sprawdzić, czy zostały wprowadzone zmiany w ustawieniach odpowiedni parametr (zobacz rysunek 16). Jeśli nie widzisz żadnych parametrów w oknie dialogowym Dodaj `@CategoryID` parametru ręcznie (nie trzeba dodawać `@RETURN_VALUE` parametru).


![Upewnij się, że ustawienia parametry są poprawne](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image44.png)

**Rysunek 16**: Upewnij się, że ustawienia parametry są poprawne


Po zaktualizowaniu warstwy DAL usuwania kategorii spowoduje usunięcie wszystkich jej skojarzone produkty i automatycznie w tym celu w obszarze parasola transakcji. Aby to sprawdzić, wróć do strony aktualizowanie i usuwanie istniejących danych binarnych, a następnie kliknij przycisk Usuń jeden z tych kategorii. Jeden jednym kliknięciem myszy kategorię i wszystkich jego skojarzone produkty zostaną usunięte.

> [!NOTE]
> Przed testowaniem `Categories_Delete` procedury składowanej, co spowoduje usunięcie wiele produktów wraz z wybranej kategorii, może być rozważenie wykonanie kopii zapasowej bazy danych. Jeśli używasz `NORTHWND.MDF` bazy danych w `App_Data`, po prostu zamknąć program Visual Studio i skopiuj pliki MDF i LDF w `App_Data` do niektórych innego folderu. Po zakończeniu testowania funkcji, można przywrócić bazy danych przez zamknięcie programu Visual Studio i zastąpienie bieżącego plików MDF i LDF pliki `App_Data` z kopią zapasową.


## <a name="summary"></a>Podsumowanie

TableAdapter Kreator s będą automatycznie generować procedur składowanych dla nas, istnieją czasy, kiedy możemy być może już tych procedur składowanych, które zostały utworzone lub chcesz je utworzyć ręcznie lub za pomocą innych narzędzi zamiast tego. Aby uwzględnić takich scenariuszy, można również skonfigurować wskaż istniejącą procedurę składowaną TableAdapter. W tym samouczku zobaczyliśmy, jak ręcznie dodać procedur składowanych do bazy danych za pośrednictwem środowiska programu Visual Studio oraz sposób połączenie metody TableAdapter s do tych procedur składowanych. Możemy również zbadane polecenia języka T-SQL i wzorzec skrypt używany dla początkowego, zatwierdzanie i wycofywanie transakcji z w procedurze składowanej.

Wszystkiego najlepszego programowania!

## <a name="about-the-author"></a>Informacje o autorze

[Scott Bento](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor siedem ASP/ASP.NET książek i założycielem [4GuysFromRolla.com](http://www.4guysfromrolla.com), pracował nad przy użyciu technologii Microsoft Web od 1998 r. Scott działa jako niezależny Konsultant, trainer i składnika zapisywania. Jego najnowszą książkę Stephena [ *Sams uczyć się ASP.NET 2.0 w ciągu 24 godzin*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). ADAM można z Tobą skontaktować w [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) lub za pośrednictwem jego blogu, który znajduje się w temacie [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Specjalne podziękowania dla

W tej serii samouczków został zrecenzowany przez wielu recenzentów pomocne. Wiodące osób dokonujących przeglądu, w tym samouczku zostały Hilton Geisenow, S ren Jacob Lauritsen i Teresa Murphy. Zainteresowani zapoznaniem Moje kolejnych artykułów MSDN? Jeśli tak, Porzuć mnie linii w [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Poprzednie](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb.md)
> [dalej](updating-the-tableadapter-to-use-joins-vb.md)
