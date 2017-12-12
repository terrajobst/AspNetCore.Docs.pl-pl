---
uid: web-forms/overview/data-access/advanced-data-access-scenarios/using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-cs
title: "Za pomocą istniejących procedur składowanych do TableAdapters Typizowanego obiektu DataSet (C#) | Dokumentacja firmy Microsoft"
author: rick-anderson
description: "W poprzednich instrukcji firma Microsoft przedstawiono sposób generować nowe procedury składowane za pomocą Kreatora TableAdapter. W tym samouczku będziemy Dowiedz się, jak ten sam obiekt TableAdapter..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/18/2007
ms.topic: article
ms.assetid: 440bef2a-1641-4238-99e3-8e2d44e7d94c
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/advanced-data-access-scenarios/using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-cs
msc.type: authoredcontent
ms.openlocfilehash: 58b76f0ac07051496c6f34be41dcf20154e34674
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/10/2017
---
<a name="using-existing-stored-procedures-for-the-typed-datasets-tableadapters-c"></a>Za pomocą istniejących procedur składowanych do TableAdapters Typizowanego obiektu DataSet (C#)
====================
przez [Bento Scott](https://twitter.com/ScottOnWriting)

[Pobierz kod](http://download.microsoft.com/download/3/9/f/39f92b37-e92e-4ab3-909e-b4ef23d01aa3/ASPNET_Data_Tutorial_68_CS.zip) lub [pobierania plików PDF](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/datatutorial68cs1.pdf)

> W poprzednich instrukcji firma Microsoft przedstawiono sposób generować nowe procedury składowane za pomocą Kreatora TableAdapter. W tym samouczku będziemy Dowiedz się, jak sam TableAdapter Kreator może współpracować z istniejącymi procedurami składowanymi. Możemy również Dowiedz się, jak ręcznie dodawać nowe procedury składowane do naszej bazie danych.


## <a name="introduction"></a>Wprowadzenie

W [poprzedniego samouczek](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs.md) widzieliśmy jak s wpisane DataSet TableAdapters może zostać skonfigurowany w taki sposób, aby użyć procedury składowane w celu dostępu do danych, a nie w trybie ad hoc instrukcji SQL. W szczególności badane możemy jak Kreator TableAdapter automatycznego tworzenia tych procedur składowanych. Jeśli przenoszenie starszych aplikacji na platformie ASP.NET 2.0 lub tworzenia witryny sieci Web programu ASP.NET 2.0 wokół istniejącego modelu danych, prawdopodobnie czy baza danych już zawiera procedury składowane, potrzebne. Alternatywnie możesz utworzyć z procedur składowanych ręcznie lub za pomocą narzędzia niektóre inne niż TableAdapter, Kreator automatycznie generuje z procedur składowanych.

W tym samouczku przedstawiono sposób konfigurowania TableAdapter do używania istniejących procedur składowanych. Ponieważ bazy danych Northwind ma tylko niewielki zestaw wbudowanych procedur składowanych, także przedstawiono kroki, aby ręcznie dodać nowe procedury składowane w bazie danych za pośrednictwem środowiska Visual Studio. Rozpoczynanie pracy dzięki s!

> [!NOTE]
> W [zawijania modyfikacje bazy danych w ramach transakcji](../working-with-batched-data/wrapping-database-modifications-within-a-transaction-cs.md) samouczek dodaliśmy metod do TableAdapter obsługi transakcji (`BeginTransaction`, `CommitTransaction`i tak dalej). Alternatywnie można zarządzać transakcji całkowicie w procedurze składowanej, która wymaga żadnych zmian w kodzie Warstwa dostępu do danych. W tym samouczku będziemy odkryć polecenia T-SQL używane do wykonywania procedury składowanej s instrukcji w zakresie transakcji.


## <a name="step-1-adding-stored-procedures-to-the-northwind-database"></a>Krok 1: Dodawanie procedur składowanych do bazy danych Northwind

Visual Studio można łatwo dodawać nowe procedury składowane do bazy danych. Let s dodać nową procedurę składowaną z bazą danych Northwind, która zwraca wszystkie kolumny z `Products` tabeli dla tych, które mają określonego `CategoryID` wartość. W oknie Eksploratora serwera rozwiń węzeł bazy danych Northwind, aby jej folderów - diagramach baz danych, tabel, widoków i tak dalej - są wyświetlane. Folder procedur składowanych widzieliśmy w poprzednim samouczek zawiera bazy danych s istniejących procedur składowanych. Aby dodać nową procedurę składowaną, kliknij prawym przyciskiem myszy folder procedur składowanych i wybierz opcję Dodaj nowe procedury składowanej z menu kontekstowego.


[![Kliknij prawym przyciskiem myszy Folder procedur składowanych i Dodaj nową procedurę składowaną](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image2.png)](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image1.png)

**Rysunek 1**: kliknij prawym przyciskiem myszy Folder procedury przechowywane i dodać nowe procedury składowanej ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image3.png))


Jak pokazano na rysunku 1, wybranie opcji Dodaj nowe procedury składowanej otwiera okno skryptu w programie Visual Studio konspektu skryptu SQL potrzebne do utworzenia procedury składowanej. Jest nasze zadania do określania się tego skryptu i wykonaj go w takim przypadku procedurę składowaną zostaną dodane do bazy danych.

Wprowadź następujący skrypt:


[!code-sql[Main](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/samples/sample1.sql)]

Ten skrypt po wykonaniu doda nową procedurę składowaną z bazą danych Northwind o nazwie `Products_SelectByCategoryID`. Tę procedurę składowaną przyjmuje jeden parametr wejściowy (`@CategoryID`, typu `int`) i zwraca wszystkie pola dla tych produktów z odpowiadającego mu `CategoryID` wartość.

Do wykonania tej operacji `CREATE PROCEDURE` skryptów i dodać procedury składowanej do bazy danych, kliknij ikonę Zapisz na pasku narzędzi lub kliknij przycisk Ctrl + S. Po wykonaniu tej czynności odświeżanie folderu procedur składowanych, procedury składowanej przedstawiający nowo utworzony. Ponadto skryptu w oknie zmieni subtlety z `CREATE PROCEDURE dbo.Products_SelectProductByCategoryID` do `ALTER PROCEDURE` `dbo.Products_SelectProductByCategoryID`. `CREATE PROCEDURE`Dodaje nową procedurę składowaną do bazy danych, podczas gdy `ALTER PROCEDURE` aktualizuje istniejący zestaw. Od czasu uruchomienia skryptu został zmieniony na `ALTER PROCEDURE`, zmiana procedur przechowywanych danych wejściowych parametrów lub instrukcji SQL i klikając ikonę Zapisz zaktualizuje procedurę składowaną z tych zmian.

Na rysunku 2 przedstawiono Visual Studio po `Products_SelectByCategoryID` procedury składowanej został zapisany.


[![Procedura składowana Products_SelectByCategoryID został dodany do bazy danych](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image5.png)](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image4.png)

**Rysunek 2**: procedura składowana `Products_SelectByCategoryID` został dodany do bazy danych ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image6.png))


## <a name="step-2-configuring-the-tableadapter-to-use-an-existing-stored-procedure"></a>Krok 2: Konfigurowanie TableAdapter, aby użyć istniejącą procedurę składowaną

Teraz, gdy `Products_SelectByCategoryID` procedury składowanej został dodany do bazy danych, możemy concigure naszych Warstwa dostępu do danych, aby użyć tej procedury składowanej po jednej z metod jej wywołaniu. W szczególności firma Microsoft doda `GetProducstByCategoryID(categoryID)` metodę `ProductsTableAdapter` w `NorthwindWithSprocs` wpisane zestawu danych, który wywołuje `Products_SelectByCategoryID` właśnie utworzyliśmy procedury składowanej.

Uruchamianie przez otwarcie `NorthwindWithSprocs` zestawu danych. Kliknij prawym przyciskiem myszy `ProductsTableAdapter` i wybierz polecenie Dodaj zapytanie, aby uruchomić Kreatora konfiguracji zapytania TableAdapter. W [poprzedniego samouczek](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs.md) postanowiliśmy ma TableAdapter, Utwórz nową procedurę składowaną firmie Microsoft. W tym samouczku, jednak chcemy okablować nowej metody TableAdapter do istniejącej `Products_SelectByCategoryID` procedury składowanej. W związku z tym wybierz opcję Użyj istniejącego procedury składowanej z pierwszym krokiem s kreatora, a następnie kliknij przycisk Dalej.


[![Wybierz istniejącą procedurę składowaną opcja użycia](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image8.png)](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image7.png)

**Rysunek 3**: wybierz opcję użycia istniejącej przechowywane procedury opcji ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image9.png))


Następujący ekran zawiera listy rozwijanej wypełniane przy użyciu bazy danych s procedur składowanych. Wybieranie procedury składowanej wyświetla jego parametrów wejściowych po lewej i zwracane (jeśli istnieją) po prawej stronie pola danych. Wybierz `Products_SelectByCategoryID` przechowywane procedury z listy i kliknij przycisk Dalej.


[![Wybierz Products_SelectByCategoryID procedury składowanej](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image11.png)](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image10.png)

**Rysunek 4**: pobranie `Products_SelectByCategoryID` procedury składowanej ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image12.png))


Następnym ekranie prosi o nas jakiego rodzaju dane zwracane przez procedurę składowaną i naszych tutaj odpowiedzi określa typ zwracany przez metodę TableAdapter s. Na przykład, jeśli firma Microsoft wskazują, że tabelaryczne dane są zwracane, metoda zwróci `ProductsDataTable` wystąpienie wypełnione rekordów zwróconych przez procedurę składowaną. Z kolei, jeśli firma Microsoft wskazują, że ta procedura składowana zwraca pojedynczą wartość TableAdapter zwróci `object` przypisany wartość w pierwszej kolumnie pierwszy rekord zwrócone przez procedurę składowaną.

Ponieważ `Products_SelectByCategoryID` procedury składowanej zwraca wszystkie produkty, które należą do określonej kategorii, wybierz pierwszej odpowiedzi - dane tabelaryczne — i kliknij przycisk Dalej.


[![Wskazuje, że procedura składowana ma zwracać dane tabelaryczne](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image14.png)](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image13.png)

**Rysunek 5**: wskazuje, że procedura składowana zwraca dane tabelaryczne ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image15.png))


Wszystkie opcje, które pozostaje jest wskazanie co metoda wzorce do użycia oraz nazwy dla tych metod. Pozostaw zarówno wypełnienia DataTable i wróć opcje elementu DataTable sprawdzić, ale Zmień nazwę metody `FillByCategoryID` i `GetProductsByCategoryID`. Następnie kliknij przycisk Dalej, aby przejrzeć podsumowanie zadań, które wykona Kreator. Jeśli wszystko jest poprawny, kliknij przycisk Zakończ.


[![Nazwa metody FillByCategoryID i GetProductsByCategoryID](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image17.png)](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image16.png)

**Rysunek 6**: Nazwa metody `FillByCategoryID` i `GetProductsByCategoryID` ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image18.png))


> [!NOTE]
> Metody TableAdapter właśnie utworzyliśmy, `FillByCategoryID` i `GetProductsByCategoryID`, oczekuje parametru wejściowego typu `int`. Wartość tego parametru wejściowego jest przekazywany do procedury przechowywanej za pośrednictwem jego `@CategoryID` parametru. Jeśli zmodyfikujesz `Products_SelectByCategory` przechowywane parametry procedury s, musisz również zaktualizować parametry te metody TableAdapter. Zgodnie z opisem w [poprzedniego samouczek](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs.md), można to zrobić w jeden z dwóch sposobów: przez ręczne dodanie lub usunięcie parametrów z kolekcji parametrów lub uruchamiając ponownie kreatora TableAdapter.


## <a name="step-3-adding-agetproductsbycategoryidcategoryidmethod-to-the-bll"></a>Krok 3: Dodawanie`GetProductsByCategoryID(categoryID)`metodę logiki warstwy Biznesowej

Z `GetProductsByCategoryID` metody DAL pełną, następnym krokiem jest zapewnienie dostępu do tej metody, warstwy logiki biznesowej. Otwórz `ProductsBLLWithSprocs` klasy plików i dodaj następującą metodę:


[!code-csharp[Main](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/samples/sample2.cs)]

Ta metoda logiki warstwy Biznesowej po prostu zwraca `ProductsDataTable` zwrócony z `ProductsTableAdapter` s `GetProductsByCategoryID` metody. `DataObjectMethodAttribute` Atrybut udostępnia metadane używane przez element ObjectDataSource kreatora Konfigurowanie źródła danych s. W szczególności ta metoda zostanie wyświetlony na liście rozwijanej wybierz kartę s.

## <a name="step-4-displaying-products-by-category"></a>Krok 4: Wyświetlenie produktów według kategorii

Aby przetestować nowo dodanego `Products_SelectByCategoryID` procedur składowanych i odpowiednich metod DAL i logiki warstwy Biznesowej, let s Tworzenie strony ASP.NET, która zawiera DropDownList i Element GridView. Lista DropDownList spowoduje wyświetlenie listy wszystkich kategorii w bazie danych podczas widoku GridView będzie wyświetlana produktów należących do wybranej kategorii.

> [!NOTE]
> Firma Microsoft interfejsy wzorzec/szczegół kolejnych utworzone przy użyciu DropDownLists w poprzednim samouczki. Aby uzyskać więcej informacji na temat przyjrzeć się implementacja raportu wzorzec/szczegół, zobacz [wzorzec/szczegół filtrowania z DropDownList](../masterdetail/master-detail-filtering-with-a-dropdownlist-cs.md) samouczka.


Otwórz `ExistingSprocs.aspx` strony `AdvancedDAL` folder i przeciągnij DropDownList z przybornika do projektanta. Ustaw DropDownList s `ID` właściwości `Categories` i jego `AutoPostBack` właściwości `true`. Następnie w tagu inteligentnego powiązać DropDownList nowy element ObjectDataSource o nazwie `CategoriesDataSource`. Skonfiguruj ObjectDataSource, dzięki czemu jego pobiera dane z `CategoriesBLL` klasy s `GetCategories` metody. Ustawianie list rozwijanych w UPDATE, INSERT i usuwanie kart na (Brak).


[![Pobieranie danych z metody GetCategories klasy CategoriesBLL s](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image20.png)](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image19.png)

**Rysunek 7**: pobieranie danych z `CategoriesBLL` klasy s `GetCategories` — metoda ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image21.png))


[![Ustawianie list rozwijanych w UPDATE, INSERT i usuwanie kart na (Brak)](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image23.png)](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image22.png)

**Rysunek 8**: wartość listy rozwijane w AKTUALIZOWANIA, WSTAWIANIA i usuwanie kart (None) ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image24.png))


Po zakończeniu pracy Kreatora ObjectDataSource, skonfiguruj DropDownList do wyświetlenia `CategoryName` pola danych i korzystać z `CategoryID` pole jako `Value` dla każdego `ListItem`.

W tym momencie znaczników deklaratywne s DropDownList i ObjectDataSource powinien podobny do następującego:


[!code-aspx[Main](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/samples/sample3.aspx)]

Następnie przeciągnij element GridView projektanta umieszczenia go pod DropDownList. Ustaw GridView s `ID` do `ProductsByCategory` i z jego tagów inteligentnych powiązać go z nowego elementu ObjectDataSource o nazwie `ProductsByCategoryDataSource`. Skonfiguruj `ProductsByCategoryDataSource` ObjectDataSource do użycia `ProductsBLLWithSprocs` klasy go pobrać jego danych przy użyciu `GetProductsByCategoryID(categoryID)` metody. Ponieważ tego widoku GridView będzie używana tylko do wyświetlania danych, ustaw list rozwijanych w UPDATE, INSERT i usuwanie kart na (Brak) i kliknij przycisk Dalej.


[![Skonfiguruj ObjectDataSource do użycia klasy ProductsBLLWithSprocs](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image26.png)](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image25.png)

**Rysunek 9**: Konfigurowanie ObjectDataSource użyć `ProductsBLLWithSprocs` klasy ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image27.png))


[![Pobieranie danych z metody GetProductsByCategoryID(categoryID)](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image29.png)](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image28.png)

**Na rysunku nr 10**: pobieranie danych z `GetProductsByCategoryID(categoryID)` — metoda ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image30.png))


Metody wybranej na karcie Wybierz oczekuje parametru, więc ostatniego kroku kreatora nam monituje o podanie źródła s parametru. Wartość na liście rozwijanej źródła parametru do formantu i wybierz polecenie `Categories` formantu z listy rozwijanej ControlID. Kliknij przycisk Zakończ, aby zakończyć pracę kreatora.


[![Użyj kategorii DropDownList jako źródło categoryID parametru](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image32.png)](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image31.png)

**Rysunek 11**: Użyj `Categories` DropDownList jako źródło `categoryID` parametr ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image33.png))


Po zakończeniu pracy Kreatora ObjectDataSource Visual Studio spowoduje dodanie BoundFields i CheckBoxField dla każdego pola danych produktu. Możesz dostosować tych pól, zgodnie z własnymi potrzebami.

Odwiedź stronę za pośrednictwem przeglądarki. Podczas odwiedzania strony, którą wybrano należące do tej kategorii i odpowiadających im produktów wymieniona w siatce. Zmiana listy rozwijanej do kategorii alternatywną, na rysunku 12 przedstawiono, powoduje odświeżenie strony i ponowne załadowanie siatkę z produktów nowo wybranej kategorii.


[![Produkty utworzyć kategorii są wyświetlane.](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image35.png)](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image34.png)

**Rysunek 12**: utworzyć kategorii produktów są wyświetlane ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image36.png))


## <a name="step-5-wrapping-a-stored-procedure-s-statements-within-the-scope-of-a-transaction"></a>Krok 5: Zawijanie instrukcje s procedury składowanej w zakresie transakcji

W [zawijania modyfikacje bazy danych w ramach transakcji](../working-with-batched-data/wrapping-database-modifications-within-a-transaction-cs.md) samouczku omówiono techniki wykonanie serię instrukcji modyfikacji bazy danych w zakresie transakcji. Lub wszystkich błędów, gwarantując niepodzielność odwołania, który zmiany wykonane w obszarze parasola transakcji albo wszystkie powiodło się. Techniki przy użyciu transakcji obejmują:

- Korzystanie z klas w `System.Transactions` przestrzeni nazw
- Warstwa dostępu do danych o używać klas ADO.NET, takich jak `SqlTransaction`, i
- Dodawanie poleceń transakcji T-SQL bezpośrednio w ramach procedury składowanej

*Zawijania modyfikacje bazy danych w ramach transakcji* samouczek używane klasy ADO.NET w warstwy DAL. W pozostałej części tego samouczka sprawdza, czy zarządzanie transakcji za pomocą polecenia T-SQL z w procedurze składowanej.

Są trzy polecenia SQL klucza ręcznego uruchamiania, zatwierdzania i wycofywania transakcji `BEGIN TRANSACTION`, `COMMIT TRANSACTION`, i `ROLLBACK TRANSACTION`odpowiednio. Jak podejście ADO.NET podczas korzystania z transakcji od w procedurze składowanej należy zastosować następującego wzorca:

1. Wskazuje początek transakcji.
2. Wykonywanie instrukcji SQL, które obejmują transakcji.
3. Jeśli występuje błąd w jednym z instrukcji z kroku nr 2 wycofywania transakcji
4. Jeśli wszystkie instrukcje z kroku nr 2 ukończone bez błędów, zatwierdzić transakcji.

Ten wzorzec może być wdrożonych w składni T-SQL przy użyciu następującego szablonu:


[!code-sql[Main](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/samples/sample4.sql)]

Szablon, który rozpoczyna się przez zdefiniowanie `TRY...CATCH` zablokować konstrukcję jesteś nowym użytkownikiem programu SQL Server 2005. Jak `try...catch` blokuje w języku C#, SQL `TRY...CATCH` bloku wykonuje instrukcje w `TRY` bloku. Jeśli każda instrukcja zgłasza błąd, sterowanie natychmiast jest przekazywane na `CATCH` bloku.

Jeśli nie ma żadnych błędów wykonywania instrukcji SQL, że w skład transakcji, `COMMIT TRANSACTION` instrukcji zatwierdza zmiany i wykonuje transakcję. Jeśli jednak jeden z oświadczeń powoduje wystąpienie błędu `ROLLBACK TRANSACTION` w `CATCH` bloku przywraca bazę danych do stanu przed rozpoczęciem transakcji. Procedura składowana również zgłasza błąd przy użyciu [polecenie RAISERROR](https://msdn.microsoft.com/en-us/library/ms178592.aspx), co powoduje, że `SqlException` się w aplikacji.

> [!NOTE]
> Ponieważ `TRY...CATCH` bloku jest nowym składnikiem programu SQL Server 2005, powyższego szablonu nie będzie działać, jeśli używasz starszej wersji programu Microsoft SQL Server. Jeśli nie używasz programu SQL Server 2005, należy skontaktować się [Zarządzanie transakcji w procedur składowanych serwera SQL](http://www.4guysfromrolla.com/webtech/080305-1.shtml) dla szablonu, który będzie działać w innych wersjach programu SQL Server.


Pozwolić, aby przyjrzeć się konkretny przykład s. Istnieje ograniczenie klucza obcego między `Categories` i `Products` tabel, co oznacza, że każdy `CategoryID` w `Products` tabeli musi być zamapowany `CategoryID` wartość w `Categories` tabeli. Dowolną akcję, która naruszyłoby to ograniczenie, takich jak próba usunięcia kategorię, która jest skojarzona produktów, wynikiem jest naruszenie ograniczenia klucza obcego. Aby to sprawdzić, należy ponownie przykład aktualizowania i usuwania istniejących danych binarnych w pracy z części danych binarnych (`~/BinaryData/UpdatingAndDeleting.aspx`). Ta strona zawiera listę poszczególnych kategorii w systemie, wraz z edytowanie i usuwanie przycisków (patrz rysunek 13), ale jeśli próba usunięcia kategorię, która ma skojarzone produkty — takie jak napoje - Usuń zakończy się niepowodzeniem z powodu naruszenia ograniczenia klucza obcego (patrz rysunek 14).


[![Każda kategoria są wyświetlane w Element GridView z właściwością edytowanie i usuwanie przycisków](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image38.png)](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image37.png)

**Rysunek 13**: każdej kategorii jest wyświetlany w Element GridView z właściwością edytowanie i usuwanie przycisków ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image39.png))


[![Nie można usunąć kategorię, która ma istniejących produktów](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image41.png)](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image40.png)

**Rysunek 14**: nie można usunąć kategorię, która ma istniejących produktów ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image42.png))


Załóżmy, jednak chęć Zezwalaj kategorii do usunięcia niezależnie od tego, czy mają jednak powiązane z produktów. Można usunąć kategorii produktów, Wyobraź sobie chęć również usunięcie ich istniejących produktów (chociaż inną opcją jest po prostu ustawienie jej produktów `CategoryID` wartości do `NULL`). Ta funkcja może realizowane za pośrednictwem zasad cascade ograniczenie klucza obcego. Alternatywnie można utworzyć procedury przechowywanej, która akceptuje `@CategoryID` parametr wejściowy i, gdy została wywołana, jawnie powoduje usunięcie wszystkich skojarzonych produktów, a następnie określonej kategorii.

Nasze pierwsza próba procedury składowanej może wyglądać następująco:


[!code-sql[Main](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/samples/sample5.sql)]

Podczas skojarzone produktów oraz kategorii ostatecznie zostaną usunięte, nie robi to w obszarze parasola transakcji. Załóżmy, że istnieje niektóre inne ograniczenia klucza obcego na `Categories` zezwolili spowoduje usunięcie określonego `@CategoryID` wartość. Problem polega na tym, że w takim przypadku wszystkie produkty zostaną usunięte przed próbą możemy usunąć kategorii. Wynikiem jest to, że dla tych kategorii tej procedury składowanej spowoduje usunięcie wszystkich jego produktów podczas gdy pozostała kategorii, ponieważ nadal ma powiązane rekordy w innej tabeli.

Jeśli procedura składowana zostały opakowane w zakresie transakcji, jednak usuwa do `Products` tabeli będzie można wycofać w wypadku niepowodzenia usuwania `Categories`. Poniższy skrypt procedury składowanej używa transakcji do zapewnienia niepodzielność między tymi dwoma `DELETE` instrukcji:


[!code-sql[Main](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/samples/sample6.sql)]

Poświęć chwilę, aby dodać `Categories_Delete` przechowywane procedury do bazy danych Northwind. Odwołują się do kroku 1 instrukcje dotyczące dodawania procedur składowanych do bazy danych.

## <a name="step-6-updating-thecategoriestableadapter"></a>Krok 6: Aktualizowanie`CategoriesTableAdapter`

Podczas możemy dodać kolejnych `Categories_Delete` procedury składowanej do bazy danych, warstwy DAL jest obecnie skonfigurowany do używania instrukcji SQL ad hoc do wykonania delete. Należy zaktualizować `CategoriesTableAdapter` oraz poinstruuj go do użycia `Categories_Delete` przechowywane procedury.

> [!NOTE]
> Wcześniej w tym samouczku będziemy pracy z `NorthwindWithSprocs` zestawu danych. Ale ten zestaw danych zawiera tylko pojedynczy element `ProductsDataTable`, więc chcemy pracować z kategorii. W związku z tym w pozostałej części tego samouczka I mówiąc o m danych warstwy dostępu I odwołujących się do `Northwind` zestawu danych, który najpierw utworzone [tworzenie Warstwa dostępu do danych](../introduction/creating-a-data-access-layer-cs.md) samouczka.


Wybierz zestaw danych Northwind, otwórz `CategoriesTableAdapter`, a następnie przejdź do okna właściwości. Wyświetla okno właściwości `InsertCommand`, `UpdateCommand`, `DeleteCommand`, i `SelectCommand` używany przez element TableAdapter oraz jej nazwę oraz informacje o połączeniu. Rozwiń węzeł `DeleteCommand` właściwości, aby wyświetlić jego szczegóły. Jak pokazano na rys. 15, `DeleteCommand` s `ComamndType` właściwość jest ustawiona na tekst, który powoduje, że wysłać tekst `CommandText` właściwości w trybie ad hoc SQL.


![Wybierz CategoriesTableAdapter w projektancie, aby wyświetlić jego właściwości w oknie właściwości](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image43.png)

**Rysunek 15**: Wybierz `CategoriesTableAdapter` w projektancie, aby wyświetlić jego właściwości w oknie właściwości


Aby zmienić te ustawienia, zaznacz tekst (elementu DeleteCommand) w oknie właściwości, a następnie wybierz z listy rozwijanej (nowy). Spowoduje to wyczyszczenie ustawienia `CommandText`, `CommandType`, i `Parameters` właściwości. Następnie należy ustawić `CommandType` właściwości `StoredProcedure` , a następnie wpisz nazwę procedury składowanej dla `CommandText` (`dbo.Categories_Delete`). Jeśli należy upewnić się, najpierw wprowadź właściwości w następującej kolejności - `CommandType` , a następnie `CommandText` — Visual Studio będzie automatycznie wypełnić kolekcji parametrów. Jeśli nie podasz tych właściwości w tej kolejności, należy ręcznie dodać parametry przez Edytor kolekcji parametrów. W obu przypadkach go s rozsądne kliknij wielokropek we właściwości parametrów można wyświetlić się Edytor kolekcji parametrów, aby sprawdzić, czy zostały wprowadzone zmiany w ustawieniach odpowiedni parametr (patrz rysunek 16). Jeśli nie ma żadnych parametrów w oknie dialogowym, Dodaj `@CategoryID` parametru ręcznie (nie trzeba dodać `@RETURN_VALUE` parametru).


![Upewnij się, że ustawienia parametrów są Popraw](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image44.png)

**Rysunek 16**: Upewnij się, że ustawienia parametrów są Popraw


Po zaktualizowaniu warstwy DAL usuwanie kategorii spowoduje usunięcie wszystkich jego skojarzony produktów i automatycznie zrobić w obszarze parasola transakcji. Aby to sprawdzić, wróć do strony aktualizowania i usuwania istniejących danych binarnych, a następnie kliknij przycisk Usuń dla jednej z następujących kategorii. Jednym kliknięciem jednym kliknięciem kategorii i wszystkich jego skojarzony produktów zostaną usunięte.

> [!NOTE]
> Przed testowaniem `Categories_Delete` procedury składowanej, co spowoduje usunięcie wiele produktów wraz z wybranej kategorii, może być rozsądne wykonanie kopii zapasowej bazy danych. Jeśli używasz `NORTHWND.MDF` bazy danych w `App_Data`, wystarczy zamknąć program Visual Studio i skopiuj pliki MDF i LDF w `App_Data` do niektórych inny folder. Po zakończeniu testowania funkcji, można przywrócić bazy danych przez zamknięcie programu Visual Studio i zastąpienie bieżącego plików MDF i LDF, pliki w `App_Data` z kopią zapasową.


## <a name="summary"></a>Podsumowanie

TableAdapter Kreator s automatycznie wygeneruje procedur składowanych firmie Microsoft, są razy, gdy firma Microsoft może już ma tworzyć takie procedury składowane lub chcesz utworzyć je ręcznie lub przy użyciu innych narzędzi zamiast tego. W przypadku takich scenariuszy, można również skonfigurować aby wskazywała istniejącą procedurę składowaną TableAdapter. W tym samouczku Analizujemy sposób ręcznie dodać procedur składowanych do bazy danych za pośrednictwem środowiska Visual Studio i okablować metody TableAdapter s do tych procedur składowanych. Firma Microsoft uwzględniła polecenia T-SQL i wzorzec skryptu używane do uruchamiania, zatwierdzania i wycofywanie transakcji od w procedurze składowanej.

Programowanie przyjemność!

## <a name="about-the-author"></a>Informacje o autorze

[Scott Bento](http://www.4guysfromrolla.com/ScottMitchell.shtml), autora siedmiu książek ASP/ASP.NET i twórcę z [4GuysFromRolla.com](http://www.4guysfromrolla.com), pracuje z technologii Microsoft Web od 1998. Scott działa jako niezależnego konsultanta trainer i składnika zapisywania. Jest jego najnowszej książki [ *Sams nauczyć się ASP.NET 2.0 w ciągu 24 godzin*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Piotr można uzyskać pod adresem [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) lub za pośrednictwem jego blog, który znajduje się w temacie [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Specjalne podziękowania dla

Ten samouczek serii zostało sprawdzone przez wiele recenzentów przydatne. Prowadzić osób dokonujących przeglądu, w tym samouczku zostały Hilton Geisenow S ren Lauritsen Jacoba i Teresa Murphy. Zainteresowani recenzowania Moje nadchodzących artykuły MSDN? Jeśli tak, Porzuć mnie linii w [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

>[!div class="step-by-step"]
[Poprzednie](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs.md)
[dalej](updating-the-tableadapter-to-use-joins-cs.md)
