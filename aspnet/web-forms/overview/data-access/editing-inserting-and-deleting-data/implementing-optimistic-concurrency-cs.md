---
uid: web-forms/overview/data-access/editing-inserting-and-deleting-data/implementing-optimistic-concurrency-cs
title: "Implementowanie optymistycznej współbieżności (C#) | Dokumentacja firmy Microsoft"
author: rick-anderson
description: "Dla aplikacji sieci web, która umożliwia wielu użytkownikom edytowanie danych istnieje ryzyko, że dwóch użytkowników może edytować tych samych danych w tym samym czasie. W tym tutori..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/17/2006
ms.topic: article
ms.assetid: 56e15b33-93b8-43ad-8e19-44c6647ea05c
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/editing-inserting-and-deleting-data/implementing-optimistic-concurrency-cs
msc.type: authoredcontent
ms.openlocfilehash: a19e6c320838849e10d2aa397a23a0ee906bac22
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/24/2018
---
<a name="implementing-optimistic-concurrency-c"></a>Implementowanie optymistycznej współbieżności (C#)
====================
przez [Bento Scott](https://twitter.com/ScottOnWriting)

[Pobierz przykładową aplikację](http://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_21_CS.exe) lub [pobierania plików PDF](implementing-optimistic-concurrency-cs/_static/datatutorial21cs1.pdf)

> Dla aplikacji sieci web, która umożliwia wielu użytkownikom edytowanie danych istnieje ryzyko, że dwóch użytkowników może edytować tych samych danych w tym samym czasie. W tym samouczku będziesz wprowadzania optymistyczne sterowanie współbieżnością do obsługi tego ryzyka.


## <a name="introduction"></a>Wprowadzenie

Dla aplikacji sieci web, w których tylko użytkownicy mogą wyświetlać dane, lub takie, które zawierają tylko jeden użytkownik, który można modyfikować danych jest nie zagrożeń dwóch równoczesnych użytkowników przypadkowemu zastąpieniu zmiany siebie nawzajem. Dla aplikacji sieci web, które umożliwiają wielu użytkownikom aktualizować lub usuwać dane jednak istnieje możliwość modyfikacji jednego użytkownika mogą powodować konfliktów z współbieżnych przez innego użytkownika. Bez dowolne zasady współbieżność w miejscu dwóch użytkowników jednocześnie edycji pojedynczego rekordu użytkownika, który potwierdza swoje zmiany ostatnio zastępują zmiany wprowadzone przez pierwszy.

Na przykład załóżmy, że dwóch użytkowników, Jisun i Sam, zostały zarówno odwiedzania strony w naszej aplikacji, która dozwolone osoby odwiedzające aktualizować i usuwać produktów za pomocą kontrolki widoku siatki. Zarówno kliknij przycisk Edytuj w widoku GridView zbliżonym do czasu. Jisun zmiany nazwy produktu "Chai Zepołowy" i klika przycisk Aktualizuj. Wynikiem jest `UPDATE` instrukcji, która jest wysyłane do bazy danych, która ustawia *wszystkie* pól można aktualizować produktu (nawet jeśli Jisun aktualizowana tylko jedno pole `ProductName`). W tym momencie baza danych ma wartość "Chai Zepołowy", kategoria Napoje, dostawca egzotycznych cieczy i tak dalej dla tego konkretnego produktu. Jednak na ekranie Sam w widoku GridView nadal zawiera nazwę produktu w można edytować wiersza elementu GridView jako "Chai". Kilka sekund po jego Jisun zmiany zostały przekazane, Sam aktualizacje kategorii przyprawy i klika aktualizacji. Powoduje to `UPDATE` instrukcji wysyłane do bazy danych, który określa nazwę produktu do "Chai," `CategoryID` na odpowiedni identyfikator kategorii napoje i tak dalej. Zostały zastąpione przez Jisun zmiany nazwy produktu. Rysunek 1 przedstawia graficznie ta sekwencja zdarzeń.


[![Gdy dwóch użytkowników jednocześnie zaktualizowania rekordu, potencjalnych s s jeden użytkownik zmienia się na zastąpienie innych s](implementing-optimistic-concurrency-cs/_static/image2.png)](implementing-optimistic-concurrency-cs/_static/image1.png)

**Rysunek 1**: gdy dwóch użytkowników jednocześnie aktualizacja istnieje rekord s potencjalne dla jednego użytkownika s zmiany Zastąp innych s ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](implementing-optimistic-concurrency-cs/_static/image3.png))


Podobnie gdy dwóch użytkowników odwiedzających stronę, co użytkownik może być pośrodku aktualizowania rekordu, gdy zostanie usunięta przez innego użytkownika. Lub między załadowanie użytkownika na stronie, a następnie klikając przycisk Usuń, może być zmodyfikowany przez innego użytkownika zawartość tego rekordu.

Istnieją trzy [sterowania współbieżnością](http://en.wikipedia.org/wiki/Concurrency_control) strategie dostępności:

- **Nic nie rób** — liczba równoczesnych użytkowników modyfikowania ten sam rekord, let ostatniego zatwierdzenia win (domyślnie)
- [**Optymistycznej współbieżności** ](http://en.wikipedia.org/wiki/Optimistic_concurrency_control) -założono, który może być współbieżności powoduje konflikt every teraz, a następnie, gdy przeważająca większość czasu takie konflikty nie będą występować; w związku z tym w przypadku wystąpienia konfliktu, po prostu Poinformuj użytkownika, który swoje zmiany Nie można zapisać, ponieważ inny użytkownik zmodyfikował tych samych danych
- **Pesymistyczne współbieżności** -założono, że konfliktom współbieżności są typowe i że użytkownicy nie będą tolerować trwa informację, ich zmiany nie zostały zapisane z powodu równoczesnych działania innego użytkownika; w związku z tym podczas uruchamiania jednego użytkownika aktualizowania rekordu go zablokować , zapobiegając w ten sposób innych użytkowników lub usunięcie tego rekordu do momentu ich zmian zatwierdza użytkownika

Wszystkich naszych samouczków dotychczasowych używanych strategię rozpoznawania współbieżności domyślny — mianowicie, zostały let ostatniego zapisu win. W tym samouczku zajmiemy się implementowania optymistyczne sterowanie współbieżnością.

> [!NOTE]
> Firma Microsoft nie będzie przyjrzeć się przykłady pesymistyczne współbieżności, w tym samouczku. Pesymistyczne współbieżności jest rzadko używana, ponieważ takie blokady, jeśli nie prawidłowo zwalniana, może uniemożliwić innym użytkownikom aktualizowanie danych. Na przykład jeśli użytkownik blokuje rekordu do edycji i pozostawia dzień przed jego odblokowaniem, żaden inny użytkownik będzie można zaktualizować tego rekordu do momentu pierwotnego użytkownika zwraca i zakończeniu jego aktualizacji. W związku z tym w sytuacjach, w których jest używany pesymistyczne współbieżności, jest zwykle limit czasu anulujące, jeśli osiągnięto, blokady. Bilet sprzedaży witryn sieci Web, które zablokować lokalizacji konkretnego miejsca krótki okres, gdy użytkownik kończy proces kolejności, jest przykładem formant pesymistyczne współbieżności.


## <a name="step-1-looking-at-how-optimistic-concurrency-is-implemented"></a>Krok 1: Patrzeć jak optymistycznej współbieżności jest zaimplementowana.

Optymistyczne sterowanie współbieżnością działania przez zapewnienie im rekordu są zaktualizowane lub usunięte ma takie same wartości, jak podczas aktualizowania lub usuwania procesu uruchamiania. Na przykład po kliknięciu przycisku edycji w GridView edytowalny, wartości rekordu są do odczytu z bazy danych i wyświetlane w pola tekstowe i inne formanty sieci Web. Te oryginalne wartości są zapisywane w widoku GridView. Później po użytkownik wprowadza swoje zmiany i kliknie przycisk Aktualizuj, oryginalnych wartości oraz nowych wartości są wysyłane do warstwy logiki biznesowej, a następnie do warstwy dostępu do danych. Warstwa dostępu do danych, należy wygenerować instrukcji SQL, która będzie tylko zaktualizowania rekordu, jeśli oryginalne wartości, które użytkownik rozpoczął edycję są takie same jak wartości nadal w bazie danych. Rysunek 2 przedstawia to sekwencja zdarzeń.


[![Dla aktualizacji lub usunięcia, które było poprawne oryginalnych wartości musi być równa wartości bieżącej bazy danych](implementing-optimistic-concurrency-cs/_static/image5.png)](implementing-optimistic-concurrency-cs/_static/image4.png)

**Rysunek 2**: For Update lub Delete pomyślnie oryginalnej wartości musi być równa wartości bieżącej bazy danych ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](implementing-optimistic-concurrency-cs/_static/image6.png))


Istnieją różne podejścia do wdrażania optymistycznej współbieżności (zobacz [Bromberg A. Peterowi](http://peterbromberg.net/)w [logikę aktualizowania współbieżności Optmistic](http://www.eggheadcafe.com/articles/20050719.asp) dla krótki opis wiele opcji). DataSet wpisane ADO.NET zapewnia jeden implementacji, który można skonfigurować za pomocą tylko znaczników wyboru. Włączanie optymistycznej współbieżności dla TableAdapter w zestawie danych wpisane wspomaga TableAdapter `UPDATE` i `DELETE` instrukcje, aby uwzględnić porównanie wszystkich oryginalnych wartości w `WHERE` klauzuli. Następujące `UPDATE` instrukcji, na przykład aktualizacje nazwy i cena produktu tylko wtedy, gdy bieżące wartości bazy danych są takie same wartości, które pierwotnie zostały pobrane podczas aktualizacji rekordu w widoku GridView. `@ProductName` i `@UnitPrice` parametrów zawiera nowe wartości wprowadzonej przez użytkownika, podczas gdy `@original_ProductName` i `@original_UnitPrice` zawierają wartości, które pierwotnie zostały załadowane w widoku GridView, gdy kliknięto przycisk Edytuj:


[!code-sql[Main](implementing-optimistic-concurrency-cs/samples/sample1.sql)]

> [!NOTE]
> To `UPDATE` uproszczono instrukcji dla czytelności. W praktyce `UnitPrice` zaewidencjonować `WHERE` klauzuli będzie więcej wysiłku od `UnitPrice` może zawierać `NULL` s i sprawdzanie, czy `NULL = NULL` zawsze zwraca wartość False (zamiast tego należy użyć `IS NULL`).


Oprócz używania innego podstawowego `UPDATE` instrukcji, konfigurowanie TableAdapter, aby użyć optymistycznej współbieżności także modyfikuje podpis jego DB bezpośrednie metody. Wycofaj z naszym pierwszym samouczku [ *tworzenie Warstwa dostępu do danych*](../introduction/creating-a-data-access-layer-cs.md), metody bezpośredniego bazy danych zostały te, które akceptuje listę skalarną wartości parametrów wejściowych jako (zamiast DataRow jednoznacznie lub Wystąpienie elementu DataTable). Korzystając z optymistycznej współbieżności, bezpośrednie DB `Update()` i `Delete()` metod uwzględnić parametry wejściowe dla wartości początkowe. Ponadto kod w logiki warstwy Biznesowej dla przy użyciu partii zaktualizować wzorca ( `Update()` przeciążenia metody, które akceptują wierszy danych i DataTables zamiast wartości skalarnych) można także zmienić.

Zamiast niż rozszerzyć naszych istniejących TableAdapters DAL w celu możemy użyć optymistycznej współbieżności (które wymagałyby zmiany logiki warstwy Biznesowej w celu uwzględnienia), zamiast tego utworzyć nowe wpisane zestawu danych o nazwie `NorthwindOptimisticConcurrency`, do którego dodamy `Products` TableAdapter który używa optymistycznej współbieżności. Następujące, utworzymy `ProductsOptimisticConcurrencyBLL` klasy warstwy logiki biznesowej, która ma odpowiednie modyfikacje do obsługi optymistycznej współbieżności DAL. Po tym przygotowuje został utworzony, firma Microsoft będzie gotowy do utworzenia strony ASP.NET.

## <a name="step-2-creating-a-data-access-layer-that-supports-optimistic-concurrency"></a>Krok 2: Tworzenie Warstwa dostępu do danych, które obsługuje optymistycznej współbieżności

Aby utworzyć nowy zestaw danych o typie określonym, kliknij prawym przyciskiem myszy `DAL` folder w `App_Code` folderu i Dodaj nowy zestaw danych o nazwie `NorthwindOptimisticConcurrency`. Jak widzieliśmy w pierwszym samouczku spowoduje tak doda nowy obiekt TableAdapter wpisane DataSet, automatyczne uruchamianie Kreatora konfiguracji TableAdapter. Na pierwszym ekranie monit możemy bazy danych do nawiązania połączenia — Połącz na tym samym Northwind bazy danych za pomocą `NORTHWNDConnectionString` z `Web.config`.


[![Łączenie w tej samej bazy danych Northwind](implementing-optimistic-concurrency-cs/_static/image8.png)](implementing-optimistic-concurrency-cs/_static/image7.png)

**Rysunek 3**: nawiązać połączenia z tej samej bazy danych Northwind ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](implementing-optimistic-concurrency-cs/_static/image9.png))


Następnie możemy monit określające, jak wykonać zapytanie dotyczące danych: za pomocą instrukcji SQL ad hoc nową procedurę składowaną lub istniejącą procedurę składowaną. Ponieważ użyliśmy zapytań SQL ad hoc w naszym oryginalnego DAL również za pomocą tej opcji w tym miejscu.


[![Określ dane, aby pobrać za pomocą instrukcji SQL Ad Hoc](implementing-optimistic-concurrency-cs/_static/image11.png)](implementing-optimistic-concurrency-cs/_static/image10.png)

**Rysunek 4**: Określ dane do pobrania przy użyciu instrukcji SQL Ad-Hoc ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](implementing-optimistic-concurrency-cs/_static/image12.png))


Na poniższym ekranie wprowadź zapytanie SQL do użycia w celu pobrania informacji o produkcie. Można użyć dokładnie tego samego zapytania SQL używane do `Products` TableAdapter z naszych DAL oryginalnej, która zwraca wszystkie `Product` kolumn oraz nazwy dostawcy i kategorii produktów:


[!code-sql[Main](implementing-optimistic-concurrency-cs/samples/sample2.sql)]


[![Użyj tego samego zapytania SQL z TableAdapter produktów w oryginalnym DAL](implementing-optimistic-concurrency-cs/_static/image14.png)](implementing-optimistic-concurrency-cs/_static/image13.png)

**Rysunek 5**: Użyj tego samego zapytania SQL z `Products` TableAdapter w oryginalnym DAL ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](implementing-optimistic-concurrency-cs/_static/image15.png))


Przed przeniesieniem na następnym ekranie, kliknij przycisk Opcje zaawansowane. Aby tego formantu optymistycznej współbieżności u TableAdapter, po prostu zaznacz pole wyboru "Użyj optymistycznej współbieżności".


[![Włącz optymistyczne sterowanie współbieżnością za sprawdzanie &quot;użyć optymistycznej współbieżności&quot; wyboru](implementing-optimistic-concurrency-cs/_static/image17.png)](implementing-optimistic-concurrency-cs/_static/image16.png)

**Rysunek 6**: Włącz optymistyczne sterowanie współbieżności, zaznaczając pole wyboru "Użyj optymistycznej współbieżności" ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](implementing-optimistic-concurrency-cs/_static/image18.png))


Ponadto wskazują, że element TableAdapter powinien używać wzorce dostępu do danych, które zarówno wypełnienia DataTable i zwracać element DataTable; również wskazywać, należy utworzyć metody bezpośredniego bazy danych. Zmień nazwę metody zwrotu wzorzec DataTable z GetData na GetProducts, w celu utworzenia duplikatów konwencje nazewnictwa użytymi w naszym oryginalnego DAL.


[![Ma TableAdapter wykorzystywać wszystkie wzorce dostępu do danych](implementing-optimistic-concurrency-cs/_static/image20.png)](implementing-optimistic-concurrency-cs/_static/image19.png)

**Rysunek 7**: ma TableAdapter wykorzystywać wszystkie dane wzorce dostępu ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](implementing-optimistic-concurrency-cs/_static/image21.png))


Po zakończeniu pracy kreatora, Projektant obiektów DataSet zawiera silnie typizowanego `Products` DataTable i TableAdapter. Poświęć chwilę, aby zmienić nazwy elementu DataTable z `Products` do `ProductsOptimisticConcurrency`, co można zrobić, klikając prawym przyciskiem myszy na pasku tytułu tabeli DataTable i wybierając polecenie zmiany nazwy z menu kontekstowego.


[![Element DataTable i TableAdapter zostały dodane do Typizowanego obiektu DataSet](implementing-optimistic-concurrency-cs/_static/image23.png)](implementing-optimistic-concurrency-cs/_static/image22.png)

**Rysunek 8**: element DataTable i DataSet wpisane zostały dodane TableAdapter ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](implementing-optimistic-concurrency-cs/_static/image24.png))


Aby wyświetlić różnice między `UPDATE` i `DELETE` zapytań między `ProductsOptimisticConcurrency` TableAdapter (używający optymistycznej współbieżności) i produkty TableAdapter (który nie), kliknij TableAdapter i przejdź do okna właściwości. W `DeleteCommand` i `UpdateCommand` właściwości `CommandText` podrzędnych widać rzeczywistej składni SQL, który jest wysyłany do bazy danych, jeśli DAL aktualizacji lub usuń związane z metody są wywoływane. Aby uzyskać `ProductsOptimisticConcurrency` TableAdapter `DELETE` jest używana instrukcja:


[!code-sql[Main](implementing-optimistic-concurrency-cs/samples/sample3.sql)]

Natomiast `DELETE` instrukcji dla TableAdapter produktu w naszym oryginalnego DAL jest znacznie prostsza:


[!code-sql[Main](implementing-optimistic-concurrency-cs/samples/sample4.sql)]

Jak widać, `WHERE` w klauzuli `DELETE` instrukcja dla Obiekt TableAdapter, która używa optymistycznej współbieżności zawiera porównanie między poszczególnymi `Product` tabeli do istniejących w chwili oryginalnej wartości i wartości w kolumnie Ostatnio wypełniono GridView (lub widoku DetailsView lub FormView). Ponieważ wszystkie pola innego niż `ProductID`, `ProductName`, i `Discontinued` może mieć `NULL` wartości, parametrów i kontroli są dołączone do porównania poprawnie `NULL` wartości w `WHERE` klauzuli.

Firma Microsoft nie będzie można Dodawanie wszelkie dodatkowe DataTables optymistycznej współbieżności włączoną DataSet w tym samouczku jako strony ASP.NET zapewnia tylko aktualizowanie i usuwanie informacji o produkcie. Jednak nadal trzeba dodać `GetProductByProductID(productID)` metody `ProductsOptimisticConcurrency` TableAdapter.

W tym celu kliknij prawym przyciskiem myszy pasek tytułu TableAdapter (prawo obszaru powyżej `Fill` i `GetProducts` nazwy metod) i wybierz polecenie Dodaj zapytanie z menu kontekstowego. Zostanie uruchomiony Kreator konfiguracji zapytania TableAdapter. Zgodnie z naszym TableAdapter konfiguracji początkowej, zdecydować się na tworzenie `GetProductByProductID(productID)` metody za pomocą instrukcji SQL ad-hoc (patrz rysunek 4). Ponieważ `GetProductByProductID(productID)` metoda zwraca informacje dotyczące konkretnego produktu, wskazuje, że jest to zapytanie `SELECT` zapytanie zwraca wiersze.


[![Oznacz typ zapytania jako &quot;SELECT, która zwraca wiersze&quot;](implementing-optimistic-concurrency-cs/_static/image26.png)](implementing-optimistic-concurrency-cs/_static/image25.png)

**Rysunek 9**: Oznacz typ zapytania jako "`SELECT` która zwraca wiersze" ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](implementing-optimistic-concurrency-cs/_static/image27.png))


Na następnym ekranie możemy wyświetlony monit o zapytanie SQL do użycia z wstępnie załadowane TableAdapter domyślne zapytanie. Rozszerzyć istniejącą kwerendę, aby uwzględnić w klauzuli `WHERE ProductID = @ProductID`, jak pokazano na rysunku nr 10.


[![Dodaj WHERE klauzuli wstępnie załadowane zapytania w celu zwrócenia rekordu określonego produktu](implementing-optimistic-concurrency-cs/_static/image29.png)](implementing-optimistic-concurrency-cs/_static/image28.png)

**Na rysunku nr 10**: Dodaj `WHERE` klauzuli Pre-Loaded zapytania do zwrócenia określonego rekordu produktu ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](implementing-optimistic-concurrency-cs/_static/image30.png))


Na koniec zmień nazwy metody wygenerowanego `FillByProductID` i `GetProductByProductID`.


[![Zmień nazwę metody FillByProductID i GetProductByProductID](implementing-optimistic-concurrency-cs/_static/image32.png)](implementing-optimistic-concurrency-cs/_static/image31.png)

**Rysunek 11**: Zmień nazwę metody `FillByProductID` i `GetProductByProductID` ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](implementing-optimistic-concurrency-cs/_static/image33.png))


Za pomocą tego kreatora pełną TableAdapter zawiera teraz dwie metody do pobierania danych: `GetProducts()`, która zwraca *wszystkie* produktów; i `GetProductByProductID(productID)`, która zwraca wartość określonego produktu.

## <a name="step-3-creating-a-business-logic-layer-for-the-optimistic-concurrency-enabled-dal"></a>Krok 3: Tworzenie warstwy logiki biznesowej dla optymistycznej współbieżności włączoną DAL

Nasze istniejące `ProductsBLL` klasa ma przykłady przy użyciu aktualizacji partii i wzorce bezpośredniego bazy danych. `AddProduct` — Metoda i `UpdateProduct` przeciążenia obu Użyj wzorca aktualizacji partii, przekazując `ProductRow` wystąpienia metody aktualizacji TableAdapter. `DeleteProduct` Metoda, z drugiej strony, korzysta ze wzorca bezpośredniego bazy danych, wywoływania TableAdapter `Delete(productID)` metody.

Z nowym `ProductsOptimisticConcurrency` TableAdapter, bazę danych bezpośrednie metody wymagają teraz, czy oryginalne wartości również być przekazany. Na przykład `Delete` metoda oczekuje teraz dziesięciu parametry wejściowe: oryginalny `ProductID`, `ProductName`, `SupplierID`, `CategoryID`, `QuantityPerUnit`, `UnitPrice`, `UnitsInStock`, `UnitsOnOrder`, `ReorderLevel`, i `Discontinued`. Używa wartości tych parametrów wejściowych dodatkowe w `WHERE` klauzuli `DELETE` instrukcji wysłanych do bazy danych tylko do usuwania określonego rekordu, jeśli mapowanie wartości bieżącej bazy danych do oryginalnych.

Podczas podpis metody dla Obiekt TableAdapter `Update` metodę we wzorcu wsadowe aktualizacji nie zmieniła się, ma kod niezbędne do rejestrowania oryginalnego i nowej wartości. Dlatego zamiast próbują użyć optymistycznej współbieżności włączoną DAL z naszych istniejącym `ProductsBLL` klasy, Utwórz nową klasę warstwy logiki biznesowej do pracy z naszej nowej warstwy DAL.

Dodaj klasę o nazwie `ProductsOptimisticConcurrencyBLL` do `BLL` folderze `App_Code` folderu.


![Dodaj klasę ProductsOptimisticConcurrencyBLL do folderu logiki warstwy Biznesowej](implementing-optimistic-concurrency-cs/_static/image34.png)

**Rysunek 12**: Dodaj `ProductsOptimisticConcurrencyBLL` klasy do folderu logiki warstwy Biznesowej


Następnie dodaj następujący kod, aby `ProductsOptimisticConcurrencyBLL` klasy:


[!code-csharp[Main](implementing-optimistic-concurrency-cs/samples/sample5.cs)]

Należy zwrócić uwagę przy użyciu `NorthwindOptimisticConcurrencyTableAdapters` instrukcji powyżej początku deklaracji klasy. `NorthwindOptimisticConcurrencyTableAdapters` Przestrzeń nazw zawiera `ProductsOptimisticConcurrencyTableAdapter` klasy, która udostępnia metody DAL. Również przed deklaracją klasy można znaleźć `System.ComponentModel.DataObject` atrybut, który powoduje, że Visual Studio, aby uwzględnić tej klasy w Kreatorze ObjectDataSource listy rozwijanej.

`ProductsOptimisticConcurrencyBLL`w `Adapter` właściwości zapewnia szybki dostęp do wystąpienia `ProductsOptimisticConcurrencyTableAdapter` klasy i jest zgodny ze wzorcem używane w naszych oryginalnej klasy logiki warstwy Biznesowej (`ProductsBLL`, `CategoriesBLL`i tak dalej). Na koniec `GetProducts()` metoda wywołuje po prostu DAL `GetProducts()` metodę i zwraca `ProductsOptimisticConcurrencyDataTable` obiektu wypełniane przy użyciu `ProductsOptimisticConcurrencyRow` wystąpienia dla każdego rekordu produktu w bazie danych.

## <a name="deleting-a-product-using-the-db-direct-pattern-with-optimistic-concurrency"></a>Usuwanie produktu przy użyciu wzorca bezpośredniego bazy danych z optymistycznej współbieżności

Podczas korzystania z wzorca bezpośredniego bazy danych przed DAL, która używa optymistycznej współbieżności, metody muszą być przekazywane nowy i oryginalnych wartości. Do usuwania, nie znajdują się nowych wartości, więc oryginalnych wartości muszą być przekazywane w. W naszym logiki warstwy Biznesowej następnie możemy zaakceptować wszystkie parametry oryginalnego jako parametry wejściowe. Ta funkcja pozwala mieć `DeleteProduct` metody w `ProductsOptimisticConcurrencyBLL` klasy należy użyć metody bezpośredniego bazy danych. Oznacza to, że ta metoda musi wykonać we wszystkich polach danych dziesięć produktu jako parametry wejściowe i przekazać je do warstwy DAL, jak pokazano w poniższym kodzie:


[!code-csharp[Main](implementing-optimistic-concurrency-cs/samples/sample6.cs)]

Jeśli oryginalne wartości - tych wartości, które zostały ostatnio załadowane w widoku GridView (lub widoku DetailsView lub FormView) — różnią się od wartości w bazie danych, gdy użytkownik kliknie przycisk Usuń `WHERE` klauzuli nie będzie zgodny z dowolnego rekordu bazy danych i rekordów będzie to miało wpływu. W związku z tym TableAdapter `Delete` metoda zwróci `0` i logiki warstwy Biznesowej `DeleteProduct` metoda zwróci `false`.

## <a name="updating-a-product-using-the-batch-update-pattern-with-optimistic-concurrency"></a>Aktualizowanie produktu przy użyciu wzorca aktualizacji partii z optymistycznej współbieżności

Jak wspomniano wcześniej, TableAdapter `Update` metoda dla wzorca aktualizacji wsadowych ma taką samą sygnaturę metody niezależnie od tego, czy jest stosowane optymistycznej współbieżności. To znaczy `Update` metoda oczekuje DataRow, tablicę elementów DataRows, DataTable lub wpisany zestawu danych. Nie ma żadnych dodatkowych parametrów wejściowych służącą do oryginalnych wartości. Jest to możliwe, ponieważ element DataTable przechowuje informacje o wartości oryginalnej i zmodyfikowanych dla jego DataRow(s). Jeśli problemy z warstwy DAL jego `UPDATE` instrukcji, `@original_ColumnName` parametry są wypełniane przy użyciu oryginalnych wartości DataRow, podczas gdy `@ColumnName` parametry są wypełniane przy użyciu wartości zmodyfikowanych DataRow.

W `ProductsBLL` klasy (który używa naszego oryginalne, optymistycznej współbieżności DAL), podczas używania wzorca aktualizacji wsadowych można zaktualizować informacji o produkcie naszego kodu wykonuje następująca sekwencja zdarzeń:

1. Przeczytaj informacje bieżącego produktu bazy danych do `ProductRow` wystąpienie za pomocą TableAdapter `GetProductByProductID(productID)` — metoda
2. Przypisanie nowych wartości `ProductRow` wystąpienia z kroku 1
3. Wywołaj element TableAdapter `Update` metody, przekazując `ProductRow` wystąpienia

Ta sekwencja kroków, jednak nie będzie poprawnie obsługuje optymistycznej współbieżności, ponieważ `ProductRow` w kroku 1 jest wypełniana bezpośrednio z bazy danych, co oznacza, czy oryginalne wartości używane przez element DataRow to takie, które istnieje obecnie w bazy danych, a nie te, które były powiązane z widoku GridView na początku procesu edycji. Zamiast tego po użyciu optymistycznej współbieżności obsługą DAL, musimy alter `UpdateProduct` przeciążenia metody do, wykonaj następujące kroki:

1. Przeczytaj informacje bieżącego produktu bazy danych do `ProductsOptimisticConcurrencyRow` wystąpienie za pomocą TableAdapter `GetProductByProductID(productID)` — metoda
2. Przypisz *oryginalnego* wartości do `ProductsOptimisticConcurrencyRow` wystąpienia z kroku 1
3. Wywołanie `ProductsOptimisticConcurrencyRow` wystąpienia `AcceptChanges()` metody, która powoduje obiektu DataRow, że jego bieżącymi wartościami są "oryginalnych"
4. Przypisz *nowe* wartości do `ProductsOptimisticConcurrencyRow` wystąpienia
5. Wywołaj element TableAdapter `Update` metody, przekazując `ProductsOptimisticConcurrencyRow` wystąpienia

Krok 1 odczyty we wszystkich wartości bieżącej bazy danych dla określonego produktu rekordu. Ten krok jest zbędny w `UpdateProduct` przeciążenia, które aktualizuje *wszystkie* kolumn produktu (jak te wartości zostaną zastąpione w kroku 2), ale ma zasadnicze znaczenie dla tych przeciążenia, gdy tylko podzestaw wartości w kolumnach są przekazywane w jako Parametry wejściowe. Gdy oryginalne wartości zostały przypisane do `ProductsOptimisticConcurrencyRow` wystąpienia, `AcceptChanges()` wywoływana jest metoda, która oznacza bieżące wartości DataRow jako oryginalnych wartości, które mają być używane w `@original_ColumnName` parametrów w `UPDATE` instrukcji. Następnie nowe wartości parametrów są przypisane do `ProductsOptimisticConcurrencyRow` , a na koniec `Update` wywoływana jest metoda, przekazując obiektu DataRow.

Poniższy kod przedstawia `UpdateProduct` parametrów wejściowych jako pola przeciążenia, które akceptuje wszystkie dane produktu. Gdy nie są wyświetlane w tym miejscu `ProductsOptimisticConcurrencyBLL` klasy pobrany plik zawiera również znajduje się w tym samouczku `UpdateProduct` przeciążenia, które akceptuje tylko produktu nazwę i cen jako parametry wejściowe.


[!code-csharp[Main](implementing-optimistic-concurrency-cs/samples/sample7.cs)]

## <a name="step-4-passing-the-original-and-new-values-from-the-aspnet-page-to-the-bll-methods"></a>Krok 4: Przekazywanie oryginalnego i nowej wartości ze strony ASP.NET do metod logiki warstwy Biznesowej

DAL i logiki warstwy Biznesowej pełne czy pozostaje jest utworzenie strony platformy ASP.NET, która może korzystać z logiką optymistycznej współbieżności wbudowanej w system. W szczególności danych formantu sieci Web (GridView, widoku DetailsView lub FormView) musi należy pamiętać, że jego oryginalnej wartości i ObjectDataSource sprawdzania oba zestawy wartości do warstwy logiki biznesowej. Ponadto strony ASP.NET musi być skonfigurowany, aby bezpiecznie obsłużyć naruszenia współbieżności.

Uruchamianie przez otwarcie `OptimisticConcurrency.aspx` strony `EditInsertDelete` folderu i dodaniu Element GridView do projektanta, ustawienie jej `ID` właściwości `ProductsGrid`. Z tagu w widoku GridView wybrać do utworzenia nowego elementu ObjectDataSource o nazwie `ProductsOptimisticConcurrencyDataSource`. Ponieważ chcemy, aby ten element ObjectDataSource do użycia warstwy DAL, który obsługuje optymistycznej współbieżności, jest skonfigurowana do używania `ProductsOptimisticConcurrencyBLL` obiektu.


[![Mają zastosowanie ObjectDataSource obiektu ProductsOptimisticConcurrencyBLL](implementing-optimistic-concurrency-cs/_static/image36.png)](implementing-optimistic-concurrency-cs/_static/image35.png)

**Rysunek 13**: mogą używać elementu ObjectDataSource `ProductsOptimisticConcurrencyBLL` obiektu ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](implementing-optimistic-concurrency-cs/_static/image37.png))


Wybierz `GetProducts`, `UpdateProduct`, i `DeleteProduct` metody z list rozwijanych w kreatorze. W przypadku metody UpdateProduct Użyj przeciążenia, które akceptuje wszystkie pola danych tego produktu.

## <a name="configuring-the-objectdatasource-controls-properties"></a>Konfigurowanie właściwości kontrolki ObjectDataSource

Po zakończeniu działania kreatora znaczników deklaracyjne element ObjectDataSource powinna wyglądać następująco:


[!code-aspx[Main](implementing-optimistic-concurrency-cs/samples/sample8.aspx)]

Jak widać, `DeleteParameters` kolekcja zawiera `Parameter` wystąpienia dla każdego dziesięć parametry wejściowe w `ProductsOptimisticConcurrencyBLL` klasy `DeleteProduct` metody. Podobnie `UpdateParameters` kolekcja zawiera `Parameter` wystąpienia dla każdego parametry wejściowe w `UpdateProduct`.

Te samouczki poprzedniej związane modyfikacji danych usuniemy ObjectDataSource `OldValuesParameterFormatString` właściwości na tym etapie, ponieważ ta właściwość wskazuje, że metoda logiki warstwy Biznesowej oczekuje wartości starego (lub oryginalnego), należy przesłać, jak również nowe wartości. Ponadto wartość tej właściwości wskazuje nazwy parametru wejściowego dla oryginalnych wartości. Ponieważ firma Microsoft przekazywanie w oryginalnych wartości do logiki warstwy Biznesowej czy *nie* Usuń tę właściwość.

> [!NOTE]
> Wartość `OldValuesParameterFormatString` właściwości musi być zamapowany na nazwy parametru wejściowego w logiki warstwy Biznesowej, które oczekują oryginalnych wartości. Ponieważ firma Microsoft o nazwie te parametry `original_productName`, `original_supplierID`i tak dalej, można pozostawić `OldValuesParameterFormatString` wartości właściwości jako `original_{0}`. Jeśli jednak parametry wejściowe metody logiki warstwy Biznesowej miał nazwy, jak `old_productName`, `old_supplierID`i tak dalej konieczne zaktualizowanie `OldValuesParameterFormatString` właściwości `old_{0}`.


Brak jednego ustawienie właściwości końcowego, który musi być dokonywane w kolejności dla elementu ObjectDataSource poprawnie przekazywane oryginalnych wartości do metod logiki warstwy Biznesowej. Element ObjectDataSource ma [właściwości wartość ConflictDetection](https://msdn.microsoft.com/library/system.web.ui.webcontrols.objectdatasource.conflictdetection.aspx) mogą być przypisane do [jedną z dwóch wartości](https://msdn.microsoft.com/library/system.web.ui.conflictoptions.aspx):

- `OverwriteChanges`— Wartość domyślna; nie przesyłają oryginalnych wartości parametrów wejściowych oryginalnej metody logiki warstwy Biznesowej
- `CompareAllValues`— Wyślij oryginalne wartości do metod logiki warstwy Biznesowej; Wybierz tę opcję, używając optymistycznej współbieżności

Poświęć chwilę, aby ustawić `ConflictDetection` właściwości `CompareAllValues`.

## <a name="configuring-the-gridviews-properties-and-fields"></a>Konfigurowanie właściwości i pola w widoku GridView

Właściwości w elemencie ObjectDataSource prawidłowo skonfigurowane skupimy konfigurowanie widoku GridView. Najpierw ponieważ chcemy GridView do obsługi, edytowanie i usuwanie kliknij pozycję Włącz edytowanie i usuwanie włączyć pól wyboru w widoku GridView tagów inteligentnych. Spowoduje to dodanie CommandField których `ShowEditButton` i `ShowDeleteButton` są ustawione na `true`.

Podczas wiązania z `ProductsOptimisticConcurrencyDataSource` ObjectDataSource, widoku GridView zawiera pole dla każdego pola danych tego produktu. Gdy taki widok siatki mogą być edytowane, wszystko, ale dopuszczalne jest środowisko użytkownika. `CategoryID` i `SupplierID` BoundFields spowoduje, że jako pola tekstowe, gdyż użytkownik musi wprowadzić odpowiednią kategorię i dostawcy jako identyfikatorów. Nie będzie bez formatowania dla pól liczbowych i nie formanty walidacji zapewnienie podano nazwę produktu i cenie jednostkowej jednostki w magazynie, jednostki w kolejności i wartości poziomu zmiany kolejności to odpowiednie wartości liczbowych i są większe niż lub równe od zera.

Jak wspomniano w *dodawanie formantów weryfikacji do edycji i wstawianie interfejsy* i *Dostosowywanie interfejs modyfikacji danych* samouczki, interfejs użytkownika może zostać dostosowane przez zastępując BoundFields TemplateFields. Po modyfikacji tego widoku GridView i jego edycji interfejsu w następujący sposób:

- Usunięte `ProductID`, `SupplierName`, i `CategoryName` BoundFields
- Przekonwertować `ProductName` elementu BoundField na pole TemplateField i dodać RequiredFieldValidation formantu.
- Przekonwertować `CategoryID` i `SupplierID` BoundFields do TemplateFields i dostosować interfejsu edycji, aby DropDownLists zamiast pól tekstowych. W tych TemplateFields `ItemTemplates`, `CategoryName` i `SupplierName` wyświetlania pól danych.
- Przekonwertować `UnitPrice`, `UnitsInStock`, `UnitsOnOrder`, i `ReorderLevel` BoundFields TemplateFields i dodany CompareValidator kontrolek.

Ponieważ sposobu wykonywania tych zadań w poprzednim samouczki mamy już sprawdzane I będzie tylko listę tutaj końcowego składni deklaratywnej i pozostaw implementacji jako rozwiązaniem.


[!code-aspx[Main](implementing-optimistic-concurrency-cs/samples/sample9.aspx)]

Bardzo jesteśmy zbliża się o przykład pełni pracy. Istnieją jednak kilka precyzyjnie, które będą pełzanie się i spowodować rozwiązywaniu problemów. Ponadto wciąż potrzebujemy niektórych interfejs, który powiadamia użytkownika, gdy nastąpiło naruszenie współbieżności.

> [!NOTE]
> Aby danych formantu sieci Web poprawnie przekazywane oryginalnych wartości do elementu ObjectDataSource, (które są następnie przekazywane do logiki warstwy Biznesowej), koniecznie który w widoku GridView `EnableViewState` właściwość jest ustawiona na `true` (ustawienie domyślne). Jeśli wyłączysz stan widoku, oryginalne wartości są utracone na ogłaszania zwrotnego.


## <a name="passing-the-correct-original-values-to-the-objectdatasource"></a>Przekazywanie poprawne oryginalnych wartości do elementu ObjectDataSource

Istnieje kilka problemów ze sposobem, w widoku GridView zostały skonfigurowane. Jeśli element ObjectDataSource `ConflictDetection` właściwość jest ustawiona na `CompareAllValues` (tak jak to nasza), gdy element ObjectDataSource `Update()` lub `Delete()` metody są wywoływane przez GridView (lub widoku DetailsView lub FormView), próbuje skopiować ObjectDataSource W widoku GridView oryginalnych wartości w jego odpowiednie `Parameter` wystąpień. Odwołują się do rysunku 2 dla graficzną reprezentację tego procesu.

W szczególności w widoku GridView oryginalne wartości są przypisane wartości w instrukcji dwukierunkowego wiązania z danymi zawsze danych jest powiązany z widoku GridView. W związku z tym ważne jest przechwytywania wymagane oryginalnych wartości wszystkich za pośrednictwem dwukierunkowego wiązania z danymi i że są one udostępniane w formacie do przekonwertowania.

Aby zobaczyć, dlaczego jest to ważne, odwiedź naszą stronę w przeglądarce chwilę potrwać. Zgodnie z oczekiwaniami, w widoku GridView zawiera listę każdego produktu z edytowanie i usuwanie przycisku w kolumnie z lewej strony.


[![Te produkty są wyświetlane w widoku GridView](implementing-optimistic-concurrency-cs/_static/image39.png)](implementing-optimistic-concurrency-cs/_static/image38.png)

**Rysunek 14**: produkty są wyświetlane w widoku GridView ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](implementing-optimistic-concurrency-cs/_static/image40.png))


Jeśli klikniesz przycisk Usuń dla wszystkich produktów `FormatException` jest generowany.


[![Próba usunięcia żadnych wyników z produktu w FormatException](implementing-optimistic-concurrency-cs/_static/image42.png)](implementing-optimistic-concurrency-cs/_static/image41.png)

**Rysunek 15**: Podjęto próbę usunięcia wszystkich wyników produktu w `FormatException` ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](implementing-optimistic-concurrency-cs/_static/image43.png))


`FormatException` Jest wywoływane, gdy element ObjectDataSource próbuje odczytać w oryginalnym `UnitPrice` wartość. Ponieważ `ItemTemplate` ma `UnitPrice` w formacie waluty (`<%# Bind("UnitPrice", "{0:C}") %>`), zawiera symbol waluty, takich jak cenie od 19,95 USD. `FormatException` Występuje jako element ObjectDataSource próbuje przekonwertować tego ciągu do `decimal`. Aby obejść ten problem, mamy kilka opcji:

- Usuń waluty formatowania `ItemTemplate`. Oznacza to, że zamiast `<%# Bind("UnitPrice", "{0:C}") %>`, po prostu użyć `<%# Bind("UnitPrice") %>`. Wadą interfejsu tego jest, że cena jest już sformatowany.
- Wyświetl `UnitPrice` waluty w `ItemTemplate`, ale użyj `Eval` — słowo kluczowe, w tym celu. Odwołania, który `Eval` wykonuje jednokierunkowe wiązania z danymi. Nadal trzeba podać `UnitPrice` wartości oryginalnych wartości, więc wciąż potrzebujemy instrukcję dwukierunkowego wiązania z danymi w `ItemTemplate`, ale to można umieścić w sieci Web etykiety formantu którego `Visible` właściwość jest ustawiona na `false`. Firma Microsoft może używać następujący kod w szablonie ItemTemplate:


[!code-aspx[Main](implementing-optimistic-concurrency-cs/samples/sample10.aspx)]

- Usuń waluty formatowanie `ItemTemplate`za pomocą `<%# Bind("UnitPrice") %>`. W widoku GridView `RowDataBound` obsługi zdarzeń, programowego dostępu do sieci Web etykiety kontroli w ramach którego `UnitPrice` wartość jest wyświetlana i ustawić jej `Text` właściwości do sformatowanego wersji.
- Pozostaw `UnitPrice` w formacie waluty. W widoku GridView `RowDeleting` program obsługi zdarzeń, Zastąp istniejące oryginalnej `UnitPrice` wartości (cenie od 19,95 USD) przy użyciu rzeczywistej wartości dziesiętnej `Decimal.Parse`. Widzieliśmy sposobu wykonywania podobny w `RowUpdating` obsługi zdarzeń w [ *obsługi logiki warstwy Biznesowej i wyjątków DAL na poziomie strony ASP.NET* ](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-cs.md) samouczka.

Np. Moje wybrano z drugiej metody Dodawanie ukryte Web etykiety kontroli którego `Text` właściwość jest dwukierunkowe danymi powiązanymi niesformatowany `UnitPrice` wartość.

Po rozwiązaniu tego problemu, spróbuj ponownie kliknięcie przycisku Usuń dla każdego produktu. Teraz otrzymasz `InvalidOperationException` próba wywołania logiki warstwy Biznesowej ObjectDataSource `UpdateProduct` metody.


[![Element ObjectDataSource nie można odnaleźć metody o parametry wejściowe chce wysyłania](implementing-optimistic-concurrency-cs/_static/image45.png)](implementing-optimistic-concurrency-cs/_static/image44.png)

**Rysunek 16**: element ObjectDataSource nie można odnaleźć metody o parametry wejściowe chce wysyłania ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](implementing-optimistic-concurrency-cs/_static/image46.png))


Spojrzenie na komunikat o wyjątku, jest jasne, czy ObjectDataSource chce wywołania logiki warstwy Biznesowej `DeleteProduct` metodę, która obejmuje `original_CategoryName` i `original_SupplierName` parametrów wejściowych. Jest to spowodowane `ItemTemplate` s dla `CategoryID` i `SupplierID` TemplateFields zawiera obecnie dwukierunkowe instrukcje powiązania z `CategoryName` i `SupplierName` pola danych. Zamiast tego należy uwzględnić `Bind` instrukcji z `CategoryID` i `SupplierID` pola danych. W tym celu Zastąp istniejące instrukcje powiązania z `Eval` instrukcje, a następnie dodaj ukryta etykieta kontrolki, których `Text` właściwości są powiązane z `CategoryID` i `SupplierID` pola danych przy użyciu dwukierunkowego wiązania danych, jak pokazano poniżej:


[!code-aspx[Main](implementing-optimistic-concurrency-cs/samples/sample11.aspx)]

Wprowadzone zmiany możemy teraz pomyślnie usuwać i edytować informacje o produkcie! W kroku 5 wyjaśniono, jak sprawdzić, czy są wykrywane naruszenia współbieżności. Jednak obecnie potrwać kilka minut, spróbuj zaktualizować i usuwanie kilka rekordów, aby upewnić się, że aktualizowanie i usuwanie dla pojedynczego użytkownika działa zgodnie z oczekiwaniami.

## <a name="step-5-testing-the-optimistic-concurrency-support"></a>Krok 5: Testowanie obsługi optymistycznej współbieżności

Aby zweryfikować naruszenia współbieżności zostanie wykryte (zamiast wynikowe w danych ślepo zastąpieniem), należy otworzyć dwa okna przeglądarki do tej strony. W obu przypadkach przeglądarki kliknij przycisk Edytuj Chai. Następnie tylko w jednej z przeglądarek, Zmień nazwę na "Chai Zepołowy" i kliknij przycisk Aktualizuj. Aktualizacja powinna powiedzie się i przywrócić stan wstępnie edycji, z "Chai Zepołowy" nową nazwę produktu widoku GridView.

W innych przeglądarki okna wystąpień, nazwa produktu pole tekstowe pozostanie "Chai". W tym drugim okno przeglądarki, należy zaktualizować `UnitPrice` do `25.00`. Bez obsługi optymistycznej współbieżności klikając polecenie update w drugiego wystąpienia przeglądarki zmieniłby nazwę produktu do "Chai", a tym samym zastępowanie zmiany wprowadzone przez pierwsze wystąpienie przeglądarki. Z zatrudnionych optymistycznej współbieżności, jednak kliknięcie przycisku Aktualizuj na drugie wystąpienie przeglądarki powoduje [DBConcurrencyException](https://msdn.microsoft.com/library/system.data.dbconcurrencyexception.aspx).


[![Po wykryciu naruszenia współbieżności, jest zgłaszany DBConcurrencyException](implementing-optimistic-concurrency-cs/_static/image48.png)](implementing-optimistic-concurrency-cs/_static/image47.png)

**Rysunek 17**: Wykryto naruszenie współbieżności podczas `DBConcurrencyException` zgłoszono ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](implementing-optimistic-concurrency-cs/_static/image49.png))


`DBConcurrencyException` Jest generowany tylko, gdy jest wykorzystywany DAL wsadowe aktualizacji wzorca. Wzorzec bezpośredniego DB nie zgłaszał wyjątku, jedynie wskazuje, że żadne wiersze nie zostały zainfekowane. Na przykład przywrócić GridView oba wystąpienia przeglądarki ich stan wstępnie edycji. Następnie w pierwszym wystąpieniu przeglądarki, kliknij przycisk Edytuj i Zmień nazwę produktu z "Chai Zepołowy" do "Chai" i kliknij przycisk Aktualizuj. W drugim oknie przeglądarki kliknij przycisk Usuń dla Chai.

Po kliknięciu Delete, strony ogłoszeń Wstecz, widoku GridView wywołuje element ObjectDataSource `Delete()` — metoda i ObjectDataSource wywołuje w dół do `ProductsOptimisticConcurrencyBLL` klasy `DeleteProduct` jest metoda wzdłuż oryginalnych wartości. Oryginalna `ProductName` wartość dla drugiego wystąpienia przeglądarki jest "Zepołowy Chai", która nie jest zgodna z bieżącą `ProductName` wartość w bazie danych. W związku z tym `DELETE` instrukcji wystawiony dla bazy danych wpływa na żadnych wierszy, ponieważ nie ma rekordów w bazie danych który `WHERE` spełnia klauzuli. `DeleteProduct` Metoda zwraca `false` i ObjectDataSource danych jest odbitych do widoku GridView.

Z perspektywy użytkownika końcowego kliknij przycisk Usuń dla Zepołowy Chai w oknie przeglądarki drugi spowodowany ekran miga i, po powracające, produkt jest nadal istnieje, mimo że teraz jest wyświetlany jako "Chai" (zmiana nazwy produktu wprowadzone przez pierwszy przeglądarki wystąpienie). Gdy użytkownik kliknie przycisk Usuń ponownie, Usuń powiedzie się, jak oryginalny widoku GridView `ProductName` wartość ("Chai") teraz dopasowania z wartością w bazie danych.

W obu przypadkach środowisko użytkownika jest daleko od stanu idealnego. Wyraźnie nie chcemy Pokaż szczegóły nitty-gritty użytkownika `DBConcurrencyException` wyjątek podczas używania wzorca aktualizacji wsadowych. A zachowaniem podczas używania wzorca bezpośredniego bazy danych jest nieco mylące polecenie użytkowników nie powiodło się, ale nie znaleziono żadnego wskazania dokładne, dlaczego.

Aby rozwiązać te problemy dwa, możemy utworzyć formantów sieci Web etykiety na stronie, które zapewniają wyjaśnienie, dlaczego aktualizacji lub usunięcia nie powiodła się. Dla partii wzorca aktualizacji, można określić, czy `DBConcurrencyException` Wystąpił wyjątek w obsłudze zdarzeń po poziomu w widoku GridView, Wyświetlanie etykiety ostrzeżenie w razie potrzeby. W przypadku metody bezpośredniego DB omówione można wartość zwracaną przez metodę logiki warstwy Biznesowej (czyli `true` Jeśli wywarła jeden wiersz, `false` w przeciwnym razie) i wyświetlić tylko komunikat informacyjny, zgodnie z potrzebami.

## <a name="step-6-adding-informational-messages-and-displaying-them-in-the-face-of-a-concurrency-violation"></a>Krok 6: Dodawanie komunikaty informacyjne i wyświetlania ich w wypadku Naruszenie współbieżności

W przypadku naruszenia współbieżności działanie, wystawiane jest zależna od czy użyto DAL aktualizację partii lub bezpośredniego wzorca bazy danych. Naszym samouczku oba wzorce z wzorcem aktualizacji wsadowych używana do aktualizowania i wzorzec bezpośredniego bazy danych używane do usuwania. Aby rozpocząć, Dodajmy dwóch formantów etykiet w sieci Web do strony, który objaśnia, czy naruszenie współbieżności wystąpił podczas próby usunięcia lub aktualizowania danych. Ustawianie formantu etykiety `Visible` i `EnableViewState` właściwości `false`; to spowoduje ich ukryta na każdym odwiedź stronę z wyjątkiem tych określonej strony odwiedza where ich `Visible` programowo ustawioną właściwość `true`.


[!code-aspx[Main](implementing-optimistic-concurrency-cs/samples/sample12.aspx)]

Oprócz ustawienia ich `Visible`, `EnabledViewState`, i `Text` właściwości, można również ustawiono `CssClass` właściwości `Warning`, co powoduje, że etykieta obiektu do wyświetlenia dużych czerwony, kursywa pogrubioną czcionką. Ta CSS `Warning` klasa została zdefiniowana i dodane do Styles.css w *badanie zdarzeń skojarzonych z Wstawianie, aktualizowanie i usuwanie* samouczka.

Po dodaniu tych etykiet, Projektant w programie Visual Studio powinien wyglądać podobnie do ilustracji 18.


[![Dodano dwa formantów etykiet do strony](implementing-optimistic-concurrency-cs/_static/image51.png)](implementing-optimistic-concurrency-cs/_static/image50.png)

**Rysunek 18**: dwie etykiety formantów zostały dodane do strony ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](implementing-optimistic-concurrency-cs/_static/image52.png))


Z tych formantów sieci Web etykiety w miejscu już wszystko gotowe do sprawdzenia jak ustalić, kiedy Naruszenie współbieżności wystąpił, jaką odpowiednie etykiety punktu `Visible` ustawioną właściwość `true`, wyświetlanie komunikat informacyjny.

## <a name="handling-concurrency-violations-when-updating"></a>Obsługa naruszenia współbieżności podczas aktualizacji

Najpierw Przyjrzyjmy się jak obsłużyć naruszenia współbieżności podczas używania wzorca aktualizacji wsadowych. Ponieważ takich naruszeń z instancją zaktualizować Przyczyna wzorzec `DBConcurrencyException` wyjątków, należy dodać kod do strony ASP.NET, aby określić, czy `DBConcurrencyException` Wystąpił wyjątek podczas procesu aktualizacji. Jeśli tak, powinien być wyświetlany komunikat do wyjaśniający użytkownika, który ich zmiany nie zostały zapisane, ponieważ inny użytkownik miał zmodyfikował tych samych danych między podczas ich uruchamiania, edytowania rekordu i kliknięcie przycisku Aktualizuj.

Jak widzieliśmy w *obsługi logiki warstwy Biznesowej i wyjątków DAL na poziomie strony ASP.NET* samouczka takie wyjątki mogą być wykryte i pomijane w obsłudze zdarzeń po poziom kontroli danych w sieci Web. W związku z tym należy utworzyć program obsługi zdarzeń dla w widoku GridView `RowUpdated` zdarzeń, który umożliwia sprawdzenie, czy `DBConcurrencyException` zgłosił wyjątek. Ten program obsługi zdarzeń jest przekazywany odwołania do dowolnego wyjątek zgłoszony podczas procesu aktualizacji, jak pokazano w przypadku obsługi kodu poniżej:


[!code-csharp[Main](implementing-optimistic-concurrency-cs/samples/sample13.cs)]

Face z `DBConcurrencyException` wyświetla ten program obsługi zdarzeń wyjątku `UpdateConflictMessage` etykiety formantu i wskazuje, czy wyjątek został obsłużony. Z tego kodu w miejscu, po wystąpieniu Naruszenie współbieżności podczas aktualizacji rekordu, użytkownika zmiany zostaną utracone, ponieważ spowoduje zastąpienie modyfikacje innego użytkownika w tym samym czasie. W szczególności widoku GridView jest zwracana do stanu przed edycji i powiązany z bieżącym danych w bazie danych. Spowoduje to zaktualizowanie wiersza elementu GridView o zmiany wprowadzone przez użytkownika, które były wcześniej nie są widoczne. Ponadto `UpdateConflictMessage` formantu etykiety wyjaśni użytkownikowi, co wystąpił. Ta sekwencja zdarzeń została szczegółowo opisana w rysunek 19.


[![Użytkownik s aktualizacje zostaną utracone w kroju Naruszenie współbieżności](implementing-optimistic-concurrency-cs/_static/image54.png)](implementing-optimistic-concurrency-cs/_static/image53.png)

**Rysunek 19**: s użytkownik aktualizacje zostaną utracone w kroju Naruszenie współbieżności ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](implementing-optimistic-concurrency-cs/_static/image55.png))


> [!NOTE]
> Możesz też zamiast zwracać widoku GridView wstępnie edycji stan, firma Microsoft może być widoku GridView w stanie edycji przez ustawienie `KeepInEditMode` właściwość przekazany do `GridViewUpdatedEventArgs` obiektu na wartość true. W przypadku zastosowania tego podejścia jednak mieć pewność ponownie powiązać dane do widoku GridView (za pomocą jego `DataBind()` metody) tak, aby wartości przez innego użytkownika są ładowane do edycji interfejsu. Dostępny do pobrania z tego samouczka kod ma następujące dwa wiersze kodu w `RowUpdated` obsługi zdarzeń w komentarzach; Usuń komentarz te wiersze kodu do widoku GridView ma pozostać w trybie edycji po Naruszenie współbieżności.


## <a name="responding-to-concurrency-violations-when-deleting"></a>Odpowiada na naruszenia współbieżności podczas usuwania

Przy użyciu wzorca bezpośredniego bazy danych nie istnieje żaden wyjątek zgłoszony w wypadku Naruszenie współbieżności. Zamiast tego instrukcji bazy danych po prostu wpływa na żadne rekordy jako klauzuli WHERE nie jest zgodny z rekordu. Wszystkie metody modyfikacji danych utworzonych w logiki warstwy Biznesowej zostały zaprojektowane tak, aby zwracały wartość logiczną wskazującą, czy wpływ na dokładnie jeden rekord. W związku z tym, aby ustalić, czy naruszenie współbieżności wystąpił podczas usuwania rekordu, można omówione wartość zwracaną logiki warstwy Biznesowej `DeleteProduct` metody.

Wartość zwracana dla metody logiki warstwy Biznesowej można zbadać w obsłudze zdarzeń po poziom ObjectDataSource za pośrednictwem `ReturnValue` właściwość `ObjectDataSourceStatusEventArgs` obiekt przekazany do obsługi zdarzeń. Ponieważ Dbamy o określającą wartość zwrotną z elementu `DeleteProduct` metody, należy utworzyć program obsługi zdarzeń dla elementu ObjectDataSource `Deleted` zdarzeń. `ReturnValue` Właściwość jest typu `object` i może być `null` Jeśli zgłoszono wyjątek i metody zostało przerwane przed może zwracać wartości. W związku z tym możemy należy najpierw upewnij się, że `ReturnValue` właściwość nie jest `null` i jest wartością logiczną. Zakładając, że weryfikacja przebiegnie pomyślnie, zostanie przedstawiony `DeleteConflictMessage` etykiety formantu, jeśli `ReturnValue` jest `false`. Można to zrobić przy użyciu następującego kodu:


[!code-csharp[Main](implementing-optimistic-concurrency-cs/samples/sample14.cs)]

Naruszenie współbieżności w wypadku żądanie usunięcia użytkownika zostało anulowane. Widoku GridView jest odświeżany, pokazujący, że zmiany, które wystąpiły dla tego rekordu między czasem użytkownika załadowanych strony i po jego kliknięciu przycisk Usuń. Jeśli takie naruszenie wynika, `DeleteConflictMessage` jest wyświetlana etykieta wyjaśniający, co tak się stało (patrz rysunek 20).


[![Użytkownik s Delete jest anulowane w wypadku Naruszenie współbieżności](implementing-optimistic-concurrency-cs/_static/image57.png)](implementing-optimistic-concurrency-cs/_static/image56.png)

**Rysunek 20**: s użytkownik Usuń jest anulowane w wypadku Naruszenie współbieżności ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](implementing-optimistic-concurrency-cs/_static/image58.png))


## <a name="summary"></a>Podsumowanie

Możliwości naruszenia współbieżności istnieje w każdej aplikacji, która zezwala na wiele równoczesnych użytkowników, aby aktualizować lub usuwać dane. Jeśli takie naruszenie nie uwzględnia dla dwóch użytkowników jednocześnie aktualizacji tych samych danych, kto z ostatniego zapisu "wins", zastępowanie inny użytkownik zmieni się zmiany. Alternatywnie deweloperów można zaimplementować formantów optymistycznej lub pesymistyczne współbieżności. Optymistyczne sterowanie współbieżnością założeniu, że naruszenia współbieżności są rzadko i po prostu uniemożliwia aktualizacji lub usuwania polecenia, który będzie stanowić naruszenie współbieżności. Formant współbieżności pesymistyczne przyjęto założenie, że tego współbieżności naruszeń są często i po prostu odrzucenia jeden użytkownik zaktualizować lub usunąć polecenie nie jest akceptowane. Za pomocą formantu pesymistyczne współbieżności aktualizowania rekordu obejmuje blokowania, zapobiegając w ten sposób wszyscy inni użytkownicy z modyfikowania i usuwania rekordu, gdy jest on zablokowany.

Wpisane zestawu danych w środowisku .NET zapewnia funkcję obsługi optymistyczne sterowanie współbieżnością. W szczególności `UPDATE` i `DELETE` instrukcje wystawiony dla bazy danych zawiera wszystkich kolumn w tabeli, zapewniając, że update lub delete tylko wówczas, gdy dane bieżącego rekordu jest zgodny z danymi użytkownika kiedy miał wykonywanie ich update lub delete. Po warstwy DAL została skonfigurowana do obsługi optymistycznej współbieżności, muszą zostać zaktualizowane metody logiki warstwy Biznesowej. Ponadto strona ASP.NET, który odwołuje się do logiki warstwy Biznesowej w dół musi być skonfigurowany w taki sposób, że element ObjectDataSource pobiera oryginalnych wartości z kontroli danych w sieci Web i przekazuje je w dół do logiki warstwy Biznesowej.

Jak widzieliśmy w tym samouczku, implementacja optymistyczne sterowanie współbieżnością w aplikacji sieci web ASP.NET polega na aktualizowanie warstwy DAL i logiki warstwy Biznesowej i dodanie obsługi na stronie platformy ASP.NET. Określa, czy dodano prac rozsądnego inwestycji czas i wysiłek zależy od aplikacji. Jeśli masz rzadko użytkownikom jednoczesne aktualizowanie danych lub dane, które aktualizują różni się od siebie, następnie sterowania współbieżnością nie jest podstawowym elementem. Jeśli jednak rutynowo masz wielu użytkowników w swojej witrynie, Praca z tych samych danych, sterowania współbieżnością mogą pomagać w zapobieganiu jeden użytkownik aktualizacji lub usuwania nieświadomie zastępowaniu innej firmy.

Programowanie przyjemność!

## <a name="about-the-author"></a>Informacje o autorze

[Scott Bento](http://www.4guysfromrolla.com/ScottMitchell.shtml), autora siedmiu książek ASP/ASP.NET i twórcę z [4GuysFromRolla.com](http://www.4guysfromrolla.com), pracuje z technologii Microsoft Web od 1998. Scott działa jako niezależnego konsultanta trainer i składnika zapisywania. Jest jego najnowszej książki [ *Sams nauczyć się ASP.NET 2.0 w ciągu 24 godzin*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Piotr można uzyskać pod adresem [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) lub za pośrednictwem jego blog, który znajduje się w temacie [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

>[!div class="step-by-step"]
[Poprzednie](customizing-the-data-modification-interface-cs.md)
[dalej](adding-client-side-confirmation-when-deleting-cs.md)
