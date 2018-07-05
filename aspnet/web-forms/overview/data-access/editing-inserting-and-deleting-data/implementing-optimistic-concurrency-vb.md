---
uid: web-forms/overview/data-access/editing-inserting-and-deleting-data/implementing-optimistic-concurrency-vb
title: Implementowanie optymistycznej współbieżności (VB) | Dokumentacja firmy Microsoft
author: rick-anderson
description: Dla aplikacji sieci web, która umożliwia wielu użytkownikom edytowanie danych istnieje ryzyko, że dwaj użytkownicy mogą edytować tych samych danych w tym samym czasie. W tym tutori...
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/17/2006
ms.topic: article
ms.assetid: 2646968c-2826-4418-b1d0-62610ed177e3
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/data-access/editing-inserting-and-deleting-data/implementing-optimistic-concurrency-vb
msc.type: authoredcontent
ms.openlocfilehash: 8bda9799661d3a2285e1a32f6d300831d8ca92e1
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/03/2018
ms.locfileid: "37395944"
---
<a name="implementing-optimistic-concurrency-vb"></a>Implementowanie optymistycznej współbieżności (VB)
====================
przez [Bento Scott](https://twitter.com/ScottOnWriting)

[Pobierz przykładową aplikację](http://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_21_VB.exe) lub [Pobierz plik PDF](implementing-optimistic-concurrency-vb/_static/datatutorial21vb1.pdf)

> Dla aplikacji sieci web, która umożliwia wielu użytkownikom edytowanie danych istnieje ryzyko, że dwaj użytkownicy mogą edytować tych samych danych w tym samym czasie. W tym samouczku będziesz wdrażamy mechanizmu kontroli optymistycznej współbieżności do obsługi tego ryzyka.


## <a name="introduction"></a>Wprowadzenie

Dla aplikacji sieci web, które Zezwalaj tylko użytkownikom na wyświetlanie danych, lub takie, które zawierają tylko pojedynczego użytkownika, kto może modyfikować danych nie ma żadnych zagrożeń dwóch jednoczesnych użytkowników przypadkowemu zastąpieniu zmiany siebie nawzajem. Dla aplikacji sieci web, które umożliwiają wielu użytkownikom aktualizować lub usuwać dane jednak istnieje możliwość modyfikacji jednego użytkownika mogą powodować konfliktów z jednoczesnych przez innego użytkownika. Bez żadnych zasad współbieżności w miejscu gdy dwóch użytkowników będzie jednocześnie edytować pojedynczy rekord użytkownika, który potwierdza jej zmiany ostatnio spowoduje zastąpienie zmian wprowadzonych przez pierwszy.

Na przykład załóżmy, że dwóch użytkowników, Jisun i Sam, zostały oba odwiedzając strony w naszej aplikacji, które dozwolone osoby odwiedzające aktualizowanie i usuwanie produktów za pomocą kontrolki GridView. Zarówno kliknij przycisk edycji, w widoku GridView w tym samym czasie. Jisun zmiany nazwy produktu "Chai herbaty" i klika przycisk Aktualizuj. Wynikiem jest `UPDATE` instrukcji, które są wysyłane do bazy danych, która ustawia *wszystkich* pól można aktualizować produktu (mimo że Jisun aktualizowane tylko jedno pole `ProductName`). W tym momencie baza danych ma wartość "Chai herbaty," kategorii Beverages, dostawca egzotycznych płynów, i tak dalej dla tego konkretnego produktu. Jednak GridView na ekranie przez Sam nadal zawiera nazwy produktu w można edytować wiersza w widoku GridView jako "Chai". Kilka sekund po jego Jisun zmiany zostały zatwierdzone, Sam aktualizacje kategorii Condiments i klika aktualizacji. Skutkuje to `UPDATE` instrukcji wysyłane do bazy danych, która ustawia nazwę produktu, aby "Chai," `CategoryID` do odpowiedniego Identyfikatora kategorii Beverages i tak dalej. Zostały zastąpione przez Jisun zmiany nazwy produktu. Rysunek 1 przedstawia graficznie w tej serii zdarzeń.


[![Gdy dwóch użytkowników jednoczesne aktualizowanie rekordów s tam potencjalne zmiany jednego użytkownika do zastąpienia jej](implementing-optimistic-concurrency-vb/_static/image2.png)](implementing-optimistic-concurrency-vb/_static/image1.png)

**Rysunek 1**: gdy dwóch użytkowników jednoczesne aktualizowanie istnieje rekord s potencjał dla jednego użytkownika zmieni się do zastąpienia jej ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](implementing-optimistic-concurrency-vb/_static/image3.png))


Podobnie gdy dwóch użytkowników odwiedzających stronę, jeden użytkownik może być podczas aktualizowania rekordu, gdy zostanie usunięty przez innego użytkownika. Lub między po użytkownik załaduje stronę, a po kliknięciu przycisku Usuń, inny użytkownik zmienił zawartość tego rekordu.

Dostępne są trzy [kontroli współbieżności](http://en.wikipedia.org/wiki/Concurrency_control) strategie dostępności:

- **Nic nie rób** -równoczesnych użytkowników modyfikowania ten sam rekord, umożliwić ostatniego zatwierdzenia, wygrać (zachowanie domyślne)
- [**Optymistyczna współbieżność** ](http://en.wikipedia.org/wiki/Optimistic_concurrency_control) -przyjęto założenie, który może być współbieżności jest w konflikcie every teraz, a następnie, większość czasu nie będą występować takie konflikty; w związku z tym, jeśli wystąpić konflikt, po prostu informować użytkownika, ich zmiany Nie można zapisać, ponieważ inny użytkownik zmodyfikował te same dane
- **Współbieżność pesymistyczna** -założono, że konfliktów współbieżności są powszechnie znanych i że użytkownicy nie będą tolerować zwrócenia informację, ich zmiany nie zostały zapisane, ze względu na jednoczesną aktywność innego użytkownika; w związku z tym, po uruchomieniu jednemu użytkownikowi aktualizowania rekordu je zablokować , zapobiegając w ten sposób inni użytkownicy, edycji lub usuwania tego rekordu, dopóki użytkownik zobowiązuje się ich modyfikacji

Pory ma wszystkich naszych samouczków używany strategię rozpoznawania współbieżności domyślne — to znaczy, Otrzymaliśmy opinie zapisana jako ostatnia wygrać. W tym samouczku zajmiemy się, jak zaimplementować mechanizmu kontroli optymistycznej współbieżności.

> [!NOTE]
> Nie będzie przyjrzymy się Współbieżność pesymistyczna przykładów w tej serii samouczków. Współbieżność pesymistyczna jest rzadko używana, ponieważ takie blokady, w przeciwnym razie poprawnie opuści, może uniemożliwić innym użytkownikom aktualizowanie danych. Na przykład jeśli użytkownik blokuje rekord do edycji i pozostawia dzień przed trwa odblokowywanie, żaden inny użytkownik będzie na zaktualizowanie rekordu do momentu użytkownika oryginalnego zwraca i zakończeniu jego aktualizacji. Dlatego w sytuacjach, w których jest używana funkcja pesymistycznej współbieżności, ma zwykle limit czasu, który, jeśli osiągnięty, anuluje blokady. Bilet sprzedaży witryn sieci Web, których zablokować lokalizacji miejsc siedzących określonego przez krótki okres, gdy użytkownik kończy proces kolejności, jest przykładem mechanizm kontroli pesymistycznej współbieżności.


## <a name="step-1-looking-at-how-optimistic-concurrency-is-implemented"></a>Krok 1: Patrząc jak Optymistyczna współbieżność jest zaimplementowana.

Mechanizmu kontroli optymistycznej współbieżności działa przez zapewnienie im rekordu są zaktualizowane lub usunięte ma takie same wartości, tak jak podczas aktualizowania lub usuwania procesu uruchamiania. Na przykład po kliknięciu przycisku edycji w edycji kontrolki GridView wartości rekordu są odczytu z bazy danych i wyświetlane w polach tekstowych i innych formantów sieci Web. Te oryginalne wartości są zapisywane w widoku GridView. Później po użytkownik wprowadza swoje zmiany i kliknie przycisk Aktualizuj, oryginalne wartości, a także nowe wartości są wysyłane do warstwy logiki biznesowej, a następnie w dół do warstwy dostępu do danych. Warstwa dostępu do danych należy wydać instrukcji SQL, który będzie aktualizować jedynie rekord Jeśli oryginalne wartości, które użytkownik rozpoczął edycję są identyczne do wartości w bazie danych. Rysunek 2 przedstawia następująca sekwencja zdarzeń.


[![Update lub Delete, które zakończyło się sukcesem oryginalne wartości, musi być równa wartości bieżącej bazy danych](implementing-optimistic-concurrency-vb/_static/image5.png)](implementing-optimistic-concurrency-vb/_static/image4.png)

**Rysunek 2**: For Update lub Delete, aby odnieść sukces, oryginalnym wartości musi być równa wartości bieżącej bazy danych ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](implementing-optimistic-concurrency-vb/_static/image6.png))


Istnieją różne metody Implementowanie optymistycznej współbieżności (zobacz [Peter A. Bromberg](http://peterbromberg.net/)firmy [Optmistic współbieżności aktualizowanie logiki](http://www.eggheadcafe.com/articles/20050719.asp) dla krótki przegląd szereg opcji). ADO.NET DataSet wpisane zapewnia co wdrożenia, które można skonfigurować tylko znaczników pola wyboru. Włączanie optymistycznej współbieżności dla TableAdapter w zestawie danych wpisane rozszerzają TableAdapter `UPDATE` i `DELETE` porównanie wszystkich oryginalnej wartości w instrukcji `WHERE` klauzuli. Następujące `UPDATE` instrukcji, na przykład aktualizuje nazwę i cena produktu tylko wtedy, gdy wartości bieżącej bazy danych są równe wartości, które zostały pierwotnie pobrany podczas aktualizowania rekordu w widoku GridView. `@ProductName` i `@UnitPrice` parametrów zawiera nowe wartości wprowadzonej przez użytkownika, natomiast `@original_ProductName` i `@original_UnitPrice` zawierają wartości, które zostały pierwotnie załadowane do kontrolki GridView kliknięcie przycisku Edytuj:


[!code-sql[Main](implementing-optimistic-concurrency-vb/samples/sample1.sql)]

> [!NOTE]
> To `UPDATE` instrukcji został uproszczony dla czytelności. W praktyce `UnitPrice` zaewidencjonować `WHERE` klauzuli byłoby bardziej skomplikowane, ponieważ `UnitPrice` może zawierać `NULL` s i sprawdzanie, czy gdy `NULL = NULL` zawsze zwraca wartość FAŁSZ (zamiast tego należy użyć `IS NULL`).


Oprócz używania różnych podstawowych `UPDATE` metody bezpośrednie instrukcji konfigurowania TableAdapter, aby użyć optymistycznej współbieżności również modyfikowanie podpis jego bazy danych. Odwoływanie się z naszym pierwszym samouczku [ *Tworzenie warstwy dostępu do danych*](../introduction/creating-a-data-access-layer-cs.md), metod bezpośrednich DB były te, które akceptuje listę skalarną wartości parametrów jako danych wejściowych (zamiast elementu DataRow silnie typizowane lub Wystąpienie elementu DataTable). Gdy optymistycznej współbieżności, bezpośrednie DB `Update()` i `Delete()` metody obejmują parametry wejściowe dla wartości oryginalne. Ponadto kod LOGIKI dotyczące korzystania z usługi batch aktualizacji wzorca ( `Update()` przeciążenia metody, które akceptują dotyczy to również utworzeń i DataTable, a nie wartości skalarnych) można także zmienić.

Raczej niż rozszerzyć nasze istniejące TableAdapters przez warstwę DAL do możemy użyć optymistycznej współbieżności, (które wymagałyby zmiany LOGIKI w celu uwzględnienia), zamiast tego utworzyć nowy wpisany zestaw danych o nazwie `NorthwindOptimisticConcurrency`, do którego dodamy `Products` TableAdapter, używa optymistycznej współbieżności. Poniżej, utworzymy `ProductsOptimisticConcurrencyBLL` klasy warstwy logiki biznesowej, która ma odpowiednie modyfikacje Obsługa optymistycznej współbieżności warstwy DAL. Po tym podwaliny pod Projekt wdrożenia został utworzony, będziemy gotowi do utworzenia strony ASP.NET.

## <a name="step-2-creating-a-data-access-layer-that-supports-optimistic-concurrency"></a>Krok 2: Tworzenie warstwy dostępu do danych, który obsługuje optymistycznej współbieżności

Aby utworzyć nowy zestaw danych wpisane, kliknij prawym przyciskiem myszy `DAL` folder wewnątrz `App_Code` folderze i Dodaj nowy zestaw danych o nazwie `NorthwindOptimisticConcurrency`. Jak widzieliśmy w pierwszym samouczku spowoduje więc doda nowy obiekt TableAdapter do wpisany zestaw danych, automatyczne uruchamianie Kreatora konfiguracji TableAdapter. Na pierwszym ekranie możemy monit Określ bazę danych, można połączyć się — connect do tego samego Northwind bazy danych przy użyciu `NORTHWNDConnectionString` z `Web.config`.


[![Nawiązać połączenie z tej samej bazy danych Northwind](implementing-optimistic-concurrency-vb/_static/image8.png)](implementing-optimistic-concurrency-vb/_static/image7.png)

**Rysunek 3**: nawiązać połączenie z tej samej bazy danych Northwind ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](implementing-optimistic-concurrency-vb/_static/image9.png))


Następnie możemy monit sposobów wysłać zapytanie dotyczące danych: za pomocą instrukcji SQL zapytań ad-hoc nową procedurę składowaną lub istniejącej procedury składowanej. Ponieważ użyliśmy zapytań SQL ad hoc w naszym oryginalnego DAL Użyj tej opcji w tym miejscu także.


[![Określ dane, które można pobrać przy użyciu instrukcji SQL zapytań Ad-Hoc](implementing-optimistic-concurrency-vb/_static/image11.png)](implementing-optimistic-concurrency-vb/_static/image10.png)

**Rysunek 4**: określenie danych, które można pobrać przy użyciu instrukcji SQL zapytań Ad-Hoc ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](implementing-optimistic-concurrency-vb/_static/image12.png))


Na poniższym ekranie wprowadź zapytanie SQL, aby pobrać informacje o produkcie. Można użyć dokładnie tego samego zapytania SQL używany do `Products` TableAdapter z naszych oryginalnego warstwy DAL, które zwraca wszystkie `Product` kolumn wraz z nazwy dostawcy i Kategoria produktu:


[!code-sql[Main](implementing-optimistic-concurrency-vb/samples/sample2.sql)]


[![Użyj tego samego zapytania SQL z TableAdapter produktów w oryginalnym warstwy DAL](implementing-optimistic-concurrency-vb/_static/image14.png)](implementing-optimistic-concurrency-vb/_static/image13.png)

**Rysunek 5**: Użyj tego samego zapytania SQL z `Products` TableAdapter w oryginalnym DAL ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](implementing-optimistic-concurrency-vb/_static/image15.png))


Przed przejściem do następnego ekranu, kliknij przycisk Opcje zaawansowane. Aby tego mechanizmu kontroli optymistycznej współbieżności stosują TableAdapter, po prostu zaznacz pole wyboru "Użyj optymistycznej współbieżności".


[![Włącz kontrolę optymistycznej współbieżności, przez sprawdzanie &quot;używaj optymistycznej współbieżności&quot; pola wyboru](implementing-optimistic-concurrency-vb/_static/image17.png)](implementing-optimistic-concurrency-vb/_static/image16.png)

**Rysunek 6**: Włącz optymistyczne sterowanie współbieżnością, zaznaczając pole wyboru "Użyj optymistycznej współbieżności" ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](implementing-optimistic-concurrency-vb/_static/image18.png))


Na koniec wskazują, że wzorców dostępu do danych, które zwraca DataTable; i Wypełnij tabelę danych powinien używać TableAdapter również wskazywać, że metod bezpośrednich bazy danych powinny być tworzone. Zmień nazwę metody dotyczące zwracania wzorzec DataTable z GetData na GetProducts, w celu utworzenia duplikatów konwencji nazewnictwa użytymi w naszym oryginalnego warstwy DAL.


[![Masz TableAdapter wykorzystywać wszystkie wzorce dostępu do danych](implementing-optimistic-concurrency-vb/_static/image20.png)](implementing-optimistic-concurrency-vb/_static/image19.png)

**Rysunek 7**: TableAdapter korzystanie z wszystkich danych programu Access wzorców ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](implementing-optimistic-concurrency-vb/_static/image21.png))


Po ukończeniu kreatora, Projektant obiektów DataSet będzie zawierać silnie typizowanego `Products` DataTable i TableAdapter. Poświęć chwilę, aby zmienić nazwę elementu DataTable z `Products` do `ProductsOptimisticConcurrency`, co można zrobić, klikając prawym przyciskiem myszy pasek tytułu tabeli DataTable i wybierając zmiany nazwy z menu kontekstowego.


[![DataTable i TableAdapter zostały dodane do zestawu danych](implementing-optimistic-concurrency-vb/_static/image23.png)](implementing-optimistic-concurrency-vb/_static/image22.png)

**Rysunek 8**: DataTable i TableAdapter zostały dodane do zestawu danych wpisane ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](implementing-optimistic-concurrency-vb/_static/image24.png))


Aby zobaczyć różnice między `UPDATE` i `DELETE` zapytań między `ProductsOptimisticConcurrency` TableAdapter (używający optymistycznej współbieżności) i TableAdapter produktów, (które nie), kliknij na obiekt TableAdapter i przejdź do okna właściwości. W `DeleteCommand` i `UpdateCommand` właściwości `CommandText` właściwości podrzędnych widać rzeczywistej składni SQL, które są wysyłane do bazy danych, wywołana aktualizację DAL lub metody dotyczące usuwania. Aby uzyskać `ProductsOptimisticConcurrency` TableAdapter `DELETE` jest używana instrukcja:


[!code-sql[Main](implementing-optimistic-concurrency-vb/samples/sample3.sql)]

Natomiast `DELETE` poufności informacji dotyczące TableAdapter produktu w naszym oryginalnego DAL jest znacznie prostsza:


[!code-sql[Main](implementing-optimistic-concurrency-vb/samples/sample4.sql)]

Jak widać, `WHERE` w klauzuli `DELETE` poufności informacji dotyczące TableAdapter, która używa optymistycznej współbieżności zawiera porównanie między poszczególnymi `Product` tabeli istniejącej wartości w kolumnie i oryginalne wartości w czasie Ostatniej chwili wypełniania kontrolki GridView (lub DetailsView lub FormView). Od wszystkich pól innych niż `ProductID`, `ProductName`, i `Discontinued` może mieć `NULL` wartości, dodatkowe parametry i kontrole są dołączone do porównania poprawnie `NULL` wartości w `WHERE` klauzuli.

Firma Microsoft nie będzie dodawać żadnych dodatkowych DataTable optymistycznej współbieżności, włączone DataSet w tym samouczku jako tylko zapewnia strony ASP.NET, aktualizowanie i usuwanie informacji o produkcie. Jednak nadal musimy dodać `GetProductByProductID(productID)` metody `ProductsOptimisticConcurrency` TableAdapter.

W tym celu kliknij prawym przyciskiem myszy pasek tytułu TableAdapter (po prawej stronie obszaru powyżej `Fill` i `GetProducts` nazwy metod) i wybierz polecenie Dodaj zapytanie z menu kontekstowego. Spowoduje to uruchomienie Kreatora konfiguracji zapytań TableAdapter. Zgodnie z naszym TableAdapter wstępną konfigurację, wybrać opcję utworzenia `GetProductByProductID(productID)` metody za pomocą instrukcji SQL zapytań ad-hoc (zobacz rysunek 4). Ponieważ `GetProductByProductID(productID)` metoda zwraca informacje o konkretnym produktem, wskazują, że to zapytanie jest `SELECT` zapytania typu, która zwraca wiersze.


[![Oznacz typ zapytania jako &quot;SELECT, która zwraca wiersze&quot;](implementing-optimistic-concurrency-vb/_static/image26.png)](implementing-optimistic-concurrency-vb/_static/image25.png)

**Rysunek 9**: Oznacz typ zapytania jako "`SELECT` która zwraca wiersze" ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](implementing-optimistic-concurrency-vb/_static/image27.png))


Na następnym ekranie możemy monit o podanie zapytanie SQL do użycia przy użyciu domyślnego zapytania TableAdapter wstępnie załadowane. Rozszerzaj istniejące zapytanie, aby uwzględnić w klauzuli `WHERE ProductID = @ProductID`, jak pokazano na rysunku nr 10.


[![Dodaj WHERE — klauzula zapytania wstępnie załadowane w celu zwrócenia rekordu określonego produktu](implementing-optimistic-concurrency-vb/_static/image29.png)](implementing-optimistic-concurrency-vb/_static/image28.png)

**Na rysunku nr 10**: Dodaj `WHERE` klauzula zapytania Pre-Loaded w celu zwrócenia określonego rekordu produktu ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](implementing-optimistic-concurrency-vb/_static/image30.png))


Na koniec zmień nazwy wygenerowana metoda `FillByProductID` i `GetProductByProductID`.


[![Zmień nazwę metody FillByProductID i GetProductByProductID](implementing-optimistic-concurrency-vb/_static/image32.png)](implementing-optimistic-concurrency-vb/_static/image31.png)

**Rysunek 11**: Zmień nazwę metody służące do `FillByProductID` i `GetProductByProductID` ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](implementing-optimistic-concurrency-vb/_static/image33.png))


Za pomocą tego kreatora pełną TableAdapter zawiera teraz dwie metody do pobierania danych: `GetProducts()`, co powoduje zwrócenie *wszystkich* produktów; i `GetProductByProductID(productID)`, która zwraca określony produkt.

## <a name="step-3-creating-a-business-logic-layer-for-the-optimistic-concurrency-enabled-dal"></a>Krok 3: Tworzenie warstwy logiki biznesowej dla optymistycznej współbieżności, włączone DAL

Nasze istniejące `ProductsBLL` klasa zawiera przykłady przy użyciu aktualizacji usługi batch i wzorce bezpośrednie bazy danych. `AddProduct` Metody i `UpdateProduct` przeciążenia obu użyć wzorca aktualizacji usługi batch, przekazując `ProductRow` wystąpienia metody aktualizacji TableAdapter. `DeleteProduct` Metody, z drugiej strony, używa wzorca bezpośrednie bazy danych, wywoływania TableAdapter `Delete(productID)` metody.

Przy użyciu nowego `ProductsOptimisticConcurrency` TableAdapter, baza danych bezpośrednie metody teraz wymagają, że oryginalne wartości zostać przekazany w. Na przykład `Delete` metoda oczekuje teraz dziesięciu parametry wejściowe: oryginalna `ProductID`, `ProductName`, `SupplierID`, `CategoryID`, `QuantityPerUnit`, `UnitPrice`, `UnitsInStock`, `UnitsOnOrder`, `ReorderLevel`, i `Discontinued`. Używa wartości te dodatkowe parametry wejściowe `WHERE` klauzuli `DELETE` instrukcji wysyłane do bazy danych tylko do usuwania określonego rekordu, jeśli mapowania wartości bieżącej bazy danych do oryginalnych.

Podczas gdy podpis metody dla TableAdapter `Update` metody używane we wzorcu aktualizacji wsadowych nie zmienił, zawiera kod potrzebny do rejestrowania wartości oryginalne i nowe. W związku z tym a nie próbują użyć optymistycznej współbieżności, włączone DAL z naszym istniejącym `ProductsBLL` klasy, Utwórz nową klasę warstwy logiki biznesowej do pracy z naszej nowej warstwy DAL.

Dodaj klasę o nazwie `ProductsOptimisticConcurrencyBLL` do `BLL` folder wewnątrz `App_Code` folderu.


![Dodaj klasę ProductsOptimisticConcurrencyBLL do folderu LOGIKI](implementing-optimistic-concurrency-vb/_static/image34.png)

**Rysunek 12**: Dodaj `ProductsOptimisticConcurrencyBLL` klasy do folderu LOGIKI


Następnie dodaj następujący kod, aby `ProductsOptimisticConcurrencyBLL` klasy:


[!code-vb[Main](implementing-optimistic-concurrency-vb/samples/sample5.vb)]

Należy pamiętać używając `NorthwindOptimisticConcurrencyTableAdapters` instrukcji powyżej początku deklaracji klasy. `NorthwindOptimisticConcurrencyTableAdapters` Przestrzeń nazw zawiera `ProductsOptimisticConcurrencyTableAdapter` klasy, która zapewnia metody DAL. Ponadto przed deklaracją klasy znajdziesz `System.ComponentModel.DataObject` atrybut, który powoduje, że Visual Studio, rozszerzając tej klasy w Kreatorze ObjectDataSource listy rozwijanej.

`ProductsOptimisticConcurrencyBLL`Firmy `Adapter` właściwości zapewnia szybki dostęp do wystąpienia `ProductsOptimisticConcurrencyTableAdapter` klasy i jest zgodna z wzorcem, używane w naszych oryginalnej klasy LOGIKI (`ProductsBLL`, `CategoriesBLL`i tak dalej). Na koniec `GetProducts()` metoda wywołuje po prostu DAL `GetProducts()` metodę i zwraca `ProductsOptimisticConcurrencyDataTable` object wypełniany `ProductsOptimisticConcurrencyRow` wystąpienia dla każdego rekordu produktu w bazie danych.

## <a name="deleting-a-product-using-the-db-direct-pattern-with-optimistic-concurrency"></a>Usuwanie produktu przy użyciu wzorca bezpośrednie bazy danych za pomocą optymistycznej współbieżności

Korzystając z wzorca bezpośrednie bazy danych dla warstwy, która używa optymistycznej współbieżności, metody muszą być przekazywane wartości nowymi i oryginalnymi. Związanych z usuwaniem, istnieją nowe wartości, więc należy przekazać oryginalnych wartości. W naszym LOGIKI następnie firma Microsoft musi zaakceptować wszystkie parametry oryginalnego jako parametry wejściowe. Przyjrzyjmy się `DeleteProduct` method in Class metoda `ProductsOptimisticConcurrencyBLL` klasy należy użyć metody bezpośrednie bazy danych. Oznacza to, że ta metoda musi wykonać we wszystkich polach danych dziesięć produktu jako parametry wejściowe i przekazać je do warstwy DAL, jak pokazano w poniższym kodzie:


[!code-vb[Main](implementing-optimistic-concurrency-vb/samples/sample6.vb)]

Jeżeli oryginalne wartości - tych wartości, które zostały ostatnio załadowane do kontrolki GridView (lub DetailsView lub FormView) — różnią się od wartości w bazie danych, gdy użytkownik kliknie przycisk Usuń `WHERE` klauzuli nie będzie zgodny z dowolnego rekordu bazy danych i żadnych rekordów będzie to miało wpływu. Dzięki temu TableAdapter `Delete` metoda zwróci `0` i LOGIKI `DeleteProduct` metoda zwróci `false`.

## <a name="updating-a-product-using-the-batch-update-pattern-with-optimistic-concurrency"></a>Aktualizowanie produktu przy użyciu wzorca aktualizacji usługi Batch za pomocą optymistycznej współbieżności

Jak wspomniano wcześniej, TableAdapter `Update` metody wzorca aktualizacji usługi batch ma taki sam podpis metody, niezależnie od tego, czy jest zatrudniony optymistycznej współbieżności. To znaczy `Update` metoda oczekuje elementu DataRow, tablica dotyczy to również utworzeń, DataTable lub DataSet z kontrolą typów. Nie istnieją żadne dodatkowe parametry wejściowe do określania oryginalnych wartości. Jest to możliwe, ponieważ DataTable śledzi informacje o wartości oryginalnego i modyfikacji dla jego DataRow(s). Jeśli problemy z warstwy DAL jego `UPDATE` instrukcji, `@original_ColumnName` parametry są wypełniane przy użyciu oryginalnych wartości DataRow, natomiast `@ColumnName` parametry są wypełniane przy użyciu zmodyfikowane wartości DataRow.

W `ProductsBLL` klasy (używający naszych oryginalny, optymistycznej współbieżności DAL), korzystając z wzorca aktualizacji usługi batch można zaktualizować informacji o produkcie, nasz kod wykonuje następująca sekwencja zdarzeń:

1. Bieżące informacje o produkcie bazy danych do odczytu `ProductRow` przy użyciu TableAdapter `GetProductByProductID(productID)` — metoda
2. Przypisz nowe wartości do `ProductRow` wystąpienia z kroku 1
3. Wywołaj TableAdapter `Update` metody, przekazując `ProductRow` wystąpienia

Tej sekwencji kroki, jednak nie będzie poprawnie obsługują optymistyczną współbieżność, ponieważ `ProductRow` wypełnione w kroku 1 jest wypełniana bezpośrednio z bazy danych, co oznacza, że oryginalne wartości, które są używane przez klasy DataRow te, które obecnie istnieje w bazy danych, a nie te, które były powiązane z GridView na początku procesu edycji. Zamiast tego po użyciu optymistycznej współbieżności obsługą warstwy DAL, musimy alter `UpdateProduct` przeciążeń metody, wykonaj następujące kroki:

1. Bieżące informacje o produkcie bazy danych do odczytu `ProductsOptimisticConcurrencyRow` przy użyciu TableAdapter `GetProductByProductID(productID)` — metoda
2. Przypisz *oryginalnego* wartości `ProductsOptimisticConcurrencyRow` wystąpienia z kroku 1
3. Wywołaj `ProductsOptimisticConcurrencyRow` wystąpienia `AcceptChanges()` metody, która instruuje klasy DataRow, czy jego bieżące wartości są tymi, które "oryginalne"
4. Przypisz *nowe* wartości `ProductsOptimisticConcurrencyRow` wystąpienia
5. Wywołaj TableAdapter `Update` metody, przekazując `ProductsOptimisticConcurrencyRow` wystąpienia

Krok 1 odczyty we wszystkich wartości bieżącej bazy danych dla rekordu określony produkt. Ten krok jest zbędny w `UpdateProduct` przeciążenia, które aktualizuje *wszystkich* kolumn produktu (jako wartości te zostaną zastąpione w kroku 2), ale ma zasadnicze znaczenie dla tych przeciążeń, gdzie podzbiorem wartości w kolumnach są przekazywane w jako Parametry wejściowe. Gdy oryginalne wartości zostały przypisane do `ProductsOptimisticConcurrencyRow` wypadku `AcceptChanges()` wywoływana jest metoda, która oznacza bieżące wartości DataRow jako oryginalnych wartości, które zostaną użyte w `@original_ColumnName` parametrów w `UPDATE` instrukcji. Następnie nowe wartości parametrów są przypisane do `ProductsOptimisticConcurrencyRow` i na koniec `Update` metoda jest wywoływana, przekazując klasy DataRow.

Poniższy kod przedstawia `UpdateProduct` przeciążenie, które akceptuje wszystkie dane produktu pól parametrów jako danych wejściowych. Gdy nie są wyświetlane w tym miejscu `ProductsOptimisticConcurrencyBLL` oferowanego w pakiecie do pobrania dla ten samouczek zawiera również klasy `UpdateProduct` przeciążenia, które akceptuje tylko w produkcie nazwa i cena jako parametry wejściowe.


[!code-vb[Main](implementing-optimistic-concurrency-vb/samples/sample7.vb)]

## <a name="step-4-passing-the-original-and-new-values-from-the-aspnet-page-to-the-bll-methods"></a>Krok 4: Przekazywanie oryginalnego i nowych wartości ze strony programu ASP.NET do metod LOGIKI

DAL i LOGIKI pełną pozostaje tylko do tworzenia strony ASP.NET, która może korzystać z logiką optymistycznej współbieżności wbudowanej w system. W szczególności danych formantu sieci Web (GridView, DetailsView lub FormView) musi Pamiętaj, że jego oryginalnej wartości i kontrolki ObjectDataSource musi przekazać oba zestawy wartości do warstwy logiki biznesowej. Ponadto strona ASP.NET musi być skonfigurowany do poprawnego działania obsługi naruszeń współbieżności.

Zacznij od otwarcia `OptimisticConcurrency.aspx` strony w `EditInsertDelete` folderu i dodaniu GridView do projektanta, ustawiając jego `ID` właściwość `ProductsGrid`. Z GridView tagu inteligentnego, wybrać opcję utworzenia nowego elementu ObjectDataSource, o nazwie `ProductsOptimisticConcurrencyDataSource`. Ponieważ chcemy, aby ta ObjectDataSource używać warstwy DAL, która obsługuje optymistycznej współbieżności, należy skonfigurować tak, aby użyć `ProductsOptimisticConcurrencyBLL` obiektu.


[![Mogą używać kontrolki ObjectDataSource obiektu ProductsOptimisticConcurrencyBLL](implementing-optimistic-concurrency-vb/_static/image36.png)](implementing-optimistic-concurrency-vb/_static/image35.png)

**Rysunek 13**: mogą używać kontrolki ObjectDataSource `ProductsOptimisticConcurrencyBLL` obiektu ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](implementing-optimistic-concurrency-vb/_static/image37.png))


Wybierz `GetProducts`, `UpdateProduct`, i `DeleteProduct` metody z listy rozwijanej w kreatorze. W przypadku metody UpdateProduct Użyj przeciążenia, które akceptuje wszystkie pola danych produktu.

## <a name="configuring-the-objectdatasource-controls-properties"></a>Konfigurowanie właściwości formantu ObjectDataSource

Po zakończeniu działania kreatora ObjectDataSource oznaczeniu deklaracyjnym powinien wyglądać następująco:


[!code-aspx[Main](implementing-optimistic-concurrency-vb/samples/sample8.aspx)]

Jak widać, `DeleteParameters` kolekcja zawiera `Parameter` wystąpienia dla każdej z dziesięciu parametrów wejściowych w `ProductsOptimisticConcurrencyBLL` klasy `DeleteProduct` metody. Podobnie `UpdateParameters` kolekcja zawiera `Parameter` wystąpienia dla każdego z parametrów wejściowych w `UpdateProduct`.

Dla tych, które zaangażowane modyfikacji danych poprzednich samouczkach usuniemy ObjectDataSource `OldValuesParameterFormatString` właściwość na tym etapie, ponieważ ta właściwość wskazuje, że metoda LOGIKI oczekuje wartości starego (lub oryginalnego) przekazać, a także nowe wartości. Ponadto wartość tej właściwości oznacza nazwy parametru wejściowego oryginalnych wartości. Ponieważ firma Microsoft przekazywania w oryginalnych wartości do LOGIKI tak *nie* Usuń tę właściwość.

> [!NOTE]
> Wartość `OldValuesParameterFormatString` właściwość musi być mapowane na nazwy parametru wejściowego w LOGIKI, które oczekują oryginalnych wartości. Ponieważ firma Microsoft o nazwie te parametry `original_productName`, `original_supplierID`i tak dalej, można pozostawić `OldValuesParameterFormatString` wartości właściwości jako `original_{0}`. Jeśli jednak z metodami LOGIKI wejściowych parametrów miały nazwy, jak `old_productName`, `old_supplierID`i tak dalej konieczne zaktualizowanie `OldValuesParameterFormatString` właściwość `old_{0}`.


Istnieje jeden ustawienie właściwości końcowego musi być dokonywane w kolejności dla elementu ObjectDataSource poprawnie przekazywane oryginalnych wartości do metod LOGIKI. Zawiera kontrolki ObjectDataSource [właściwość ConflictDetection](https://msdn.microsoft.com/library/system.web.ui.webcontrols.objectdatasource.conflictdetection.aspx) mogą być przypisane do [jedną z dwóch wartości](https://msdn.microsoft.com/library/system.web.ui.conflictoptions.aspx):

- `OverwriteChanges` — wartość domyślną; nie będzie przesyłał oryginalnych wartości do metody LOGIKI oryginalnego parametry wejściowe
- `CompareAllValues` — wysłać oryginalnych wartości do metod LOGIKI; Wybierz tę opcję, gdy optymistycznej współbieżności

Poświęć chwilę, aby ustawić `ConflictDetection` właściwość `CompareAllValues`.

## <a name="configuring-the-gridviews-properties-and-fields"></a>Konfigurowanie właściwości i pola w widoku GridView

Za pomocą właściwości ObjectDataSource prawidłowo skonfigurowane skupimy z konfigurowaniem widoku GridView. Najpierw ponieważ chcemy, aby GridView do obsługi, edytowania i usuwania, kliknij pozycję Włącz edytowanie i Włącz usuwanie pól wyboru z GridView tagu inteligentnego. Spowoduje to dodanie CommandField którego `ShowEditButton` i `ShowDeleteButton` są ustawione na `true`.

Podczas wiązania z `ProductsOptimisticConcurrencyDataSource` ObjectDataSource, widoku GridView zawiera pole dla każdego pola danych produktu. Podczas edycji kontrolki GridView wszystko, ale dopuszczalne jest środowisko użytkownika. `CategoryID` i `SupplierID` BoundFields będą renderowane jako pola tekstowe, użytkownik musi wprowadzić odpowiednią kategorię i dostawcy, jako numery identyfikatorów. Nie będzie żadnych formatowania pól liczbowych i nie kontrolek weryfikacji, aby upewnić się, podano nazwę produktu i cenę jednostkową jednostek w magazynie, jednostki w kolejności, a której kolejność chcesz zmienić wartości poziomu obu odpowiednie wartości numeryczne i są większe niż lub równe od zera.

Tak jak Omówiliśmy to w *Dodawanie kontrolek weryfikacji do edycji i wstawiania interfejsy* i *Dostosowywanie interfejsu modyfikacji danych* samouczki i interfejsu użytkownika mogą być dostosowywane przez zastępując BoundFields kontrolek TemplateField. Po modyfikacji tego widoku GridView i jego edycji interfejs w następujący sposób:

- Usunięte `ProductID`, `SupplierName`, i `CategoryName` BoundFields
- Przekonwertowany `ProductName` elementu BoundField do TemplateField i dodana kontrolka RequiredFieldValidation.
- Przekonwertowany `CategoryID` i `SupplierID` BoundFields do kontrolek TemplateField i dostosowana interfejsu edycji, aby kontrolek DropDownList, a nie pola tekstowe. W tych kontrolek TemplateField `ItemTemplates`, `CategoryName` i `SupplierName` wyświetlania pól danych.
- Przekonwertowany `UnitPrice`, `UnitsInStock`, `UnitsOnOrder`, i `ReorderLevel` BoundFields kontrolek TemplateField i dodano CompareValidator kontrolki.

Ponieważ sposobu wykonywania tych zadań w poprzednich samouczkach mamy już sprawdzane I będzie tylko listy w tym miejscu ostatecznego składni deklaratywnej i pozostaw implementacji rozwiązaniem.


[!code-aspx[Main](implementing-optimistic-concurrency-vb/samples/sample9.aspx)]

Jesteśmy bardzo blisko o w pełni praktyczny przykład. Istnieje jednak kilka precyzyjnie, które będą pełzanie się i spowodować, że nas problemów. Ponadto wciąż potrzebujemy niektórych interfejs, który powiadamia użytkownika, gdy wystąpiło naruszenie współbieżności.

> [!NOTE]
> Aby dane formantu sieci Web poprawnie przekazywane oryginalnych wartości do kontrolki ObjectDataSource (które są następnie przekazywane do LOGIKI), koniecznie, GridView `EnableViewState` właściwość jest ustawiona na `true` (ustawienie domyślne). Jeśli wyłączysz stan widoku, oryginalne wartości zostaną utracone na zwrot.


## <a name="passing-the-correct-original-values-to-the-objectdatasource"></a>Przekazywanie poprawne oryginalnych wartości do kontrolki ObjectDataSource

Istnieje kilka problemów ze sposobem, w widoku GridView zostały skonfigurowane. Jeśli ObjectDataSource `ConflictDetection` właściwość jest ustawiona na `CompareAllValues` (podobnie jak nasza jest), gdy ObjectDataSource `Update()` lub `Delete()` metody są wywoływane przez kontrolki GridView (lub DetailsView lub FormView), kontrolki ObjectDataSource próbuje skopiować GridView oryginalnych wartości w jego odpowiedniego `Parameter` wystąpień. Odwołaj się do rysunku 2 dla graficzną reprezentację tego procesu.

W szczególności GridView oryginalne wartości są przypisane wartości w instrukcjach dwukierunkowego wiązania danych, w każdym danych jest powiązany z kontrolki GridView. Dlatego jest istotne, że wymagane oryginalnych wartości wszystkich są przechwytywane za pośrednictwem dwukierunkowego wiązania danych i że są one udostępniane w postaci konwertowany.

Aby zobaczyć, dlaczego jest to ważne, Poświęć chwilę, aby odwiedzić naszą stronę w przeglądarce. Zgodnie z oczekiwaniami, w widoku GridView zawiera listę każdego produktu z przyciskiem edytowania i usuwania w skrajnej lewej kolumnie.


[![Produkty są wymienione w widoku GridView](implementing-optimistic-concurrency-vb/_static/image39.png)](implementing-optimistic-concurrency-vb/_static/image38.png)

**Rysunek 14**: produkty są wymienione w kontrolce GridView ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](implementing-optimistic-concurrency-vb/_static/image40.png))


Jeśli klikniesz przycisk usuwania dla wszystkich produktów `FormatException` zgłaszany.


[![Podjęto próbę usunięcia wyniki dla dowolnego produktu w wyjątek FormatException](implementing-optimistic-concurrency-vb/_static/image42.png)](implementing-optimistic-concurrency-vb/_static/image41.png)

**Rysunek 15**: Podjęto próbę usunięcia wszystkie wyniki dla produktu w `FormatException` ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](implementing-optimistic-concurrency-vb/_static/image43.png))


`FormatException` Jest zgłaszane w przypadku kontrolki ObjectDataSource podejmuje próbę odczytu w oryginalnym `UnitPrice` wartość. Ponieważ `ItemTemplate` ma `UnitPrice` w formacie waluty (`<%# Bind("UnitPrice", "{0:C}") %>`), zawiera symbol waluty, takich jak cenie od 19,95 USD. `FormatException` Wypada kontrolki ObjectDataSource stara się przekonwertować tego ciągu w `decimal`. Aby obejść ten problem, mamy kilka opcji:

- Usuń formatowanie z waluty `ItemTemplate`. Oznacza to, że zamiast `<%# Bind("UnitPrice", "{0:C}") %>`, po prostu użyć `<%# Bind("UnitPrice") %>`. Wadą tego jest, że cena jest już sformatowany.
- Wyświetlanie `UnitPrice` w formacie waluty w `ItemTemplate`, ale `Eval` — słowo kluczowe, w tym celu. Pamiętamy `Eval` wykonuje jednokierunkowe wiązania danych. Nadal należy podać `UnitPrice` wartość oryginalne wartości, dlatego nadal należy do instrukcji dwukierunkowego wiązania z danymi w `ItemTemplate`, ale to można umieścić w kontrolce etykiety Web którego `Visible` właściwość jest ustawiona na `false`. Moglibyśmy użyć następujące znaczniki ItemTemplate:


[!code-aspx[Main](implementing-optimistic-concurrency-vb/samples/sample10.aspx)]

- Usuń formatowanie z waluty `ItemTemplate`przy użyciu `<%# Bind("UnitPrice") %>`. W widoku GridView `RowDataBound` programu obsługi zdarzeń, programowo dostęp Web etykiety kontroli w ramach którego `UnitPrice` wartość jest wyświetlana i ustaw jego `Text` właściwości do wersji sformatowany.
- Pozostaw `UnitPrice` sformatowane jako walutę. W widoku GridView `RowDeleting` procedura obsługi zdarzeń, zastąpić nim pierwotny istniejących `UnitPrice` wartość (cenie od 19,95 USD) z usługi za pomocą rzeczywistych wartości dziesiętnej `Decimal.Parse`. Widzieliśmy sposób wykonania coś podobnego na elemencie `RowUpdating` programu obsługi zdarzeń w [ *obsługi LOGIKI i wyjątki DAL na poziomie strony ASP.NET* ](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-cs.md) samouczka.

Mój przykład został wybrany do korzystania z drugiego podejścia formant dodanie ukrytego Web etykiety, którego `Text` właściwość jest dwukierunkowe danymi powiązanymi niesformatowany `UnitPrice` wartość.

Po rozwiązaniu tego problemu, spróbuj ponowne kliknięcie przycisku usuwania dla każdego produktu. Teraz otrzymasz `InvalidOperationException` podczas próby wywołania LOGIKI kontrolki ObjectDataSource `UpdateProduct` metody.


[![Kontrolki ObjectDataSource nie można odnaleźć metody o parametry wejściowe chce wysyłania](implementing-optimistic-concurrency-vb/_static/image45.png)](implementing-optimistic-concurrency-vb/_static/image44.png)

**Rysunek 16**: kontrolki ObjectDataSource nie można odnaleźć metody o parametry wejściowe chce wysyłania ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](implementing-optimistic-concurrency-vb/_static/image46.png))


Patrząc komunikat o wyjątku, to oczywiste, że kontrolki ObjectDataSource chce do wywołania LOGIKI `DeleteProduct` metodę, która obejmuje `original_CategoryName` i `original_SupplierName` parametrów wejściowych. Jest to spowodowane `ItemTemplate` pod kątem `CategoryID` i `SupplierID` kontrolek TemplateField aktualnie zawierają instrukcje powiązanie dwukierunkowe `CategoryName` i `SupplierName` pola danych. Zamiast tego należy dołączyć `Bind` instrukcje `CategoryID` i `SupplierID` pola danych. W tym celu Zastąp istniejące instrukcje powiązania z `Eval` instrukcji, a następnie dodaj ukryta etykieta kontrolki, których `Text` właściwości są powiązane z `CategoryID` i `SupplierID` pola danych za pomocą dwukierunkowego wiązania danych, jak pokazano poniżej:


[!code-aspx[Main](implementing-optimistic-concurrency-vb/samples/sample11.aspx)]

Za pomocą tych zmian możemy teraz pomyślnie usuwać i edytować informacje o produkcie! W kroku 5 omówimy sposób weryfikacji wykrycia naruszenia współbieżności. Ale teraz, po kilku minutach aktualizacji i usuwania rekordów kilka, aby upewnić się, że aktualizowanie i usuwanie dla pojedynczego użytkownika działa zgodnie z oczekiwaniami.

## <a name="step-5-testing-the-optimistic-concurrency-support"></a>Krok 5: Testowanie Obsługa optymistycznej współbieżności

Aby sprawdzić, czy ich naruszenia współbieżności wykryte (zamiast wynikowe dane skutkować nieświadomym zastąpieniem), należy otworzyć dwa okna przeglądarki do tej strony. W obu przypadkach przeglądarki kliknij przycisk Edytuj, aby Chai. Następnie tylko w jednej z przeglądarki, Zmień nazwę na "Chai herbaty" i kliknij przycisk Aktualizuj. Aktualizacja powinna powiedzie się i przywrócić stan wstępnie edycji, za pomocą "Chai herbaty" nową nazwę produktu widoku GridView.

W innych okien wystąpieniu przeglądarki jednak nazwa produktu TextBox nadal będzie widoczny "Chai". W tym drugim okno przeglądarki, należy zaktualizować `UnitPrice` do `25.00`. Bez obsługi optymistycznej współbieżności klikając polecenie update w drugim wystąpieniu przeglądarki zmieniłby się nazwa produktu do "Chai", a tym samym zastępowanie zmian wprowadzonych przez pierwsze wystąpienie przeglądarki. Za pomocą zatrudnionych optymistycznej współbieżności, jednak, klikając przycisk Aktualizuj w drugim wystąpieniu przeglądarki skutkuje [DBConcurrencyException](https://msdn.microsoft.com/library/system.data.dbconcurrencyexception.aspx).


[![Po wykryciu naruszenia współbieżności zgłaszany DBConcurrencyException](implementing-optimistic-concurrency-vb/_static/image48.png)](implementing-optimistic-concurrency-vb/_static/image47.png)

**Rysunek 17**: Wykryto naruszenie współbieżności podczas `DBConcurrencyException` zgłaszany ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](implementing-optimistic-concurrency-vb/_static/image49.png))


`DBConcurrencyException` Tylko jest generowany, gdy wzorzec aktualizacji wsadowych DAL jest wykorzystywany. Wzorzec bezpośrednie bazy danych nie zgłaszała wyjątek, tylko wskazuje, że żadne wiersze nie zostały zainfekowane. Na przykład zwraca GridView oba wystąpienia przeglądarki do stanu wstępnie edycji. Następnie w pierwszym wystąpieniu przeglądarki, kliknij przycisk Edytuj i zmienić nazwę produktu z "Chai herbaty" do "Chai" i kliknij przycisk Aktualizuj. W drugim oknie przeglądarki kliknij przycisk Usuń, aby Chai.

Po kliknięciu Delete, strony publikuje Wstecz, widoku GridView wywołuje ObjectDataSource `Delete()` metoda i kontrolki ObjectDataSource wywoła `ProductsOptimisticConcurrencyBLL` klasy `DeleteProduct` jest metoda wzdłuż oryginalnych wartości. Oryginalny `ProductName` wartość dla drugiego wystąpienia przeglądarki jest "Herbaty Chai", który jest niezgodny z bieżącym `ProductName` wartość w bazie danych. W związku z tym `DELETE` oświadczenie wydane w bazie danych wpływa na zero wierszy, ponieważ nie ma rekordów w bazie danych, `WHERE` spełnia klauzuli. `DeleteProduct` Metoda zwraca `false` i ObjectDataSource danych jest odbitych do kontrolki GridView.

Z perspektywy użytkownika końcowego kliknięcie na przycisk usuwania dla herbaty Chai w drugim oknie przeglądarki spowodowane ekranu, aby flash oraz od powracające, produkt jest nadal istnieje, mimo że teraz jest ona wyświetlana jako "Chai" (zmiana nazwy produktu przez pierwsze przeglądarki wystąpienie). Jeśli użytkownik kliknie przycisk Usuń ponownie, usuwania powiedzie się, jak oryginalny widoku GridView `ProductName` wartość ("Chai") teraz dopasowania z wartością w bazie danych.

W obu przypadkach środowisko użytkownika jest idealne rozwiązanie. Wyraźnie nie chcemy pokazać użytkownikowi szczegóły nitty-gritty `DBConcurrencyException` wyjątek podczas używania wzorca aktualizacji usługi batch. I zachowanie, gdy za pomocą wzorca bezpośrednie bazy danych jest nieco mylące jako polecenie użytkowników nie powiodło się, ale nie było żadnych dokładnie określić dlaczego.

Aby rozwiązać te dwa problemy, możemy utworzyć formantów etykiet w sieci Web, na stronie, zapewniające wyjaśnienie, dlaczego aktualizacji lub usuwania nie powiodła się. Wzorzec aktualizacji usługi batch, określimy, czy `DBConcurrencyException` Wystąpił wyjątek w obsłudze zdarzeń po poziom GridView wyświetlania etykiety ostrzeżenie, zgodnie z potrzebami. Metody bezpośredniej bazy danych, firma Microsoft można sprawdzić wartość zwracaną metody LOGIKI (czyli `true` Jeśli miała wpływu na jeden wiersz, `false` inaczej) i wyświetlić komunikat informacyjny, zgodnie z potrzebami.

## <a name="step-6-adding-informational-messages-and-displaying-them-in-the-face-of-a-concurrency-violation"></a>Krok 6: Dodawanie komunikaty informacyjne i wyświetlania ich w przypadku naruszenia współbieżności

W przypadku naruszenia współbieżności działanie, wystawiane jest zależna od czy DAL aktualizacji usługi batch lub wzorzec bezpośrednie DB użyto. Nasz samouczek korzysta z obu wzorców, za pomocą wzorca aktualizacji usługi batch używanych na potrzeby wzorzec bezpośrednie bazy danych używane do usuwania i aktualizowania. Aby rozpocząć pracę, możemy dodać dwie kontrolki etykiety w sieci Web do strony, która wyjaśnia, czy naruszenie współbieżności podczas próby usunięcia lub aktualizacji danych. Ustaw formant etykiety `Visible` i `EnableViewState` właściwości w celu `false`; spowoduje ich ukryte na każdym odwiedź stronę z wyjątkiem tych określonej strony odwiedza miejsce ich `Visible` programowo ustawiono właściwość `true`.


[!code-aspx[Main](implementing-optimistic-concurrency-vb/samples/sample12.aspx)]

Oprócz ustawienia ich `Visible`, `EnabledViewState`, i `Text` właściwości, czy też ustawiono `CssClass` właściwości `Warning`, co powoduje, że etykieta użytkownika będzie wyświetlana w dużych, czerwony, kursywy, pogrubioną czcionką. Ta CSS `Warning` klasy została zdefiniowana i dodane do Styles.css w *badanie zdarzeń skojarzonych z Wstawianie, aktualizowanie i usuwanie* samouczka.

Po dodaniu tych etykiet, Projektant w programie Visual Studio powinien wyglądać podobnie jak rysunek 18.


[![Dwie kontrolki etykiety zostały dodane do strony](implementing-optimistic-concurrency-vb/_static/image51.png)](implementing-optimistic-concurrency-vb/_static/image50.png)

**Rysunek 18**: dwie etykiety formantów zostały dodane do strony ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](implementing-optimistic-concurrency-vb/_static/image52.png))


Z tych formantów etykiet w sieci Web w miejscu, jesteśmy gotowi do sprawdzenia, jak ustalić, kiedy współbieżności nastąpiło naruszenie, jaką punktu odpowiednią etykietę `Visible` właściwość może być ustawiona na `true`, wyświetlając komunikat informacyjny.

## <a name="handling-concurrency-violations-when-updating"></a>Obsługa współbieżności naruszeń podczas aktualizacji

Najpierw Spójrzmy na sposób obsługi naruszeń współbieżności, korzystając z wzorca aktualizacji usługi batch. Od czasu takiego naruszenia, za pomocą usługi batch aktualizacji Przyczyna wzorzec `DBConcurrencyException` zgłoszenie wyjątku, należy dodać kod do strony ASP.NET, aby określić, czy `DBConcurrencyException` Wystąpił wyjątek podczas procesu aktualizacji. Jeśli tak, powinien zostać wyświetlony komunikat wyjaśniający użytkownika, ich zmiany nie zostały zapisane, ponieważ ma zmodyfikowany przez innego użytkownika te same dane między podczas ich uruchamiania, edytowania rekordu i kliknięcie przycisku aktualizacji.

Jak widzieliśmy w *obsługi LOGIKI i wyjątki DAL na poziomie strony ASP.NET* samouczek takie wyjątki mogą być wykryte i pomijane w procedurze obsługi zdarzeń po poziomu danych kontrolki sieci Web. W związku z tym, należy utworzyć procedurę obsługi zdarzeń dla GridView `RowUpdated` zdarzenia, które sprawdza, czy `DBConcurrencyException` został zgłoszony wyjątek. Ta procedura obsługi zdarzeń jest przekazywany odwołanie do każdego wyjątku, który został zgłoszony podczas procesu aktualizacji, jak pokazano w poniższym kodzie procedury obsługi zdarzeń:


[!code-vb[Main](implementing-optimistic-concurrency-vb/samples/sample13.vb)]

Face z `DBConcurrencyException` wyjątku, ta procedura obsługi zdarzeń wyświetla `UpdateConflictMessage` kontrolka etykiety i wskazuje, czy wyjątek został obsłużony. Przy użyciu tego kodu w miejscu po Naruszenie współbieżności występuje podczas aktualizowania rekordu, zmiany wprowadzone przez użytkownika zostaną utracone, ponieważ będzie zastąpione modyfikacji przez innego użytkownika w tym samym czasie. W szczególności widoku GridView jest zwracany stan wstępnie edycji i powiązane z bieżącym danych w bazie danych. Spowoduje to zaktualizowanie wiersza w widoku GridView zmian przez innych użytkowników, które były wcześniej nie są widoczne. Ponadto `UpdateConflictMessage` formant etykiety wyjaśnią użytkownikowi, co stało. Następująca sekwencja zdarzeń została szczegółowo opisana w rysunek 19.


[![Użytkownik s aktualizacje zostaną utracone w twarz Naruszenie współbieżności](implementing-optimistic-concurrency-vb/_static/image54.png)](implementing-optimistic-concurrency-vb/_static/image53.png)

**Rysunek 19**: s użytkownik aktualizacje zostaną utracone w twarz Naruszenie współbieżności ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](implementing-optimistic-concurrency-vb/_static/image55.png))


> [!NOTE]
> Alternatywnie zamiast zwracać widoku GridView wstępnie edycji stanu, można pozostawimy widoku GridView w stanie edycji, ustawiając `KeepInEditMode` właściwość przekazany do `GridViewUpdatedEventArgs` obiektu na wartość true. W przypadku zastosowania tego podejścia jednak mieć pewność ponownie powiązać dane do kontrolki GridView (przez wywołanie jego `DataBind()` metoda) tak, aby wartości przez innego użytkownika są ładowane do interfejsu edycji. Kod można pobrać za pomocą tego samouczka ma następujące dwa wiersze kodu w `RowUpdated` komentarzami programu obsługi zdarzeń; po prostu Usuń komentarz następujące wiersze kodu, widoku GridView po Naruszenie współbieżności pozostać w trybie edycji.


## <a name="responding-to-concurrency-violations-when-deleting"></a>Reagowanie na naruszenie współbieżności, podczas usuwania

Za pomocą wzorca bezpośrednie bazy danych nie istnieje żaden wyjątek zgłaszany w przypadku naruszenia współbieżności. Zamiast tego instrukcja bazy danych po prostu ma wpływ na żadne rekordy, jako klauzuli WHERE nie jest zgodny z dowolnego rekordu. Wszystkie metody modyfikacji danych utworzonych w LOGIKI zostały zaprojektowane tak, aby zwracają wartość logiczną wskazującą, czy problem dotyczy dokładnie jeden rekord. W związku z tym, aby ustalić, czy naruszenie współbieżności wystąpił podczas usuwania rekordu, firma Microsoft można sprawdzić wartość zwracaną LOGIKI `DeleteProduct` metody.

Wartość zwracana dla metody LOGIKI można zbadać w obsłudze zdarzeń po poziomu ObjectDataSource za pośrednictwem `ReturnValue` właściwość `ObjectDataSourceStatusEventArgs` obiekt przekazany do procedury obsługi zdarzeń. Ponieważ jesteśmy zainteresowani określająca wartość zwrotną z elementu `DeleteProduct` metody, należy utworzyć procedurę obsługi zdarzeń dla elementu ObjectDataSource `Deleted` zdarzeń. `ReturnValue` Właściwość jest typu `object` i może być `null` Jeśli wystąpił wyjątek i metoda zostało przerwane przed jego może zwracać wartości. W związku z tym, możemy najpierw upewnij się, że `ReturnValue` właściwość nie jest `null` i jest wartością logiczną. Zakładając, że przekazuje ten test, pokazujemy `DeleteConflictMessage` kontrolka etykiety, jeśli `ReturnValue` jest `false`. Można to osiągnąć, używając następującego kodu:


[!code-vb[Main](implementing-optimistic-concurrency-vb/samples/sample14.vb)]

W przypadku naruszenia współbieżności żądanie usunięcia użytkownika zostało anulowane. Są odświeżane widoku GridView wskazuje, że zmiany, które wystąpiły dla tego rekordu w czasie między użytkownika załadować stronę i po jego kliknięciu przycisk Usuń. Jeśli takie naruszenie wynika, `DeleteConflictMessage` jest wyświetlana etykieta wyjaśniający, co właśnie wydarzyło się (zobacz rysunek 20).


[![S usuwania użytkownika zostało anulowane w przypadku naruszenia współbieżności](implementing-optimistic-concurrency-vb/_static/image57.png)](implementing-optimistic-concurrency-vb/_static/image56.png)

**Rysunek 20**: s użytkownik, Usuń zostało anulowane w przypadku naruszenia współbieżności ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](implementing-optimistic-concurrency-vb/_static/image58.png))


## <a name="summary"></a>Podsumowanie

Możliwości naruszeń współbieżności istnieje w każdej aplikacji, która zezwala na wiele równoczesnych użytkowników, aby aktualizować lub usuwać dane. Jeśli takie naruszenie nie uwzględnia dla dwóch użytkowników jednocześnie aktualizacji tych samych danych, kto pobiera ostatniego zapisu "wins", zastępując to drugi użytkownik zmieni się zmiany. Alternatywnie deweloperzy mogą zaimplementować albo kontroli współbieżności optymistycznego lub pesymistycznego. Mechanizmu kontroli optymistycznej współbieżności przyjęto założenie, że współbieżności naruszeń są rzadkie i po prostu nie zezwala na aktualizację lub usunąć polecenie, które będzie stanowić naruszenie współbieżności. Mechanizm kontroli pesymistycznej współbieżności przyjęto założenie, że ten współbieżności naruszeń są często i po prostu odrzuca jednego użytkownika. Zaktualizuj lub Usuń polecenie nie jest akceptowane. Za pomocą kontroli pesymistycznej współbieżności aktualizowania rekordu obejmuje blokowania, zapobiegając w ten sposób inni użytkownicy, zmodyfikowanie lub usunięcie rekordu, gdy jest on zablokowany.

Wpisany zestaw danych na platformie .NET zapewnia funkcję obsługi mechanizmu kontroli optymistycznej współbieżności. W szczególności `UPDATE` i `DELETE` instrukcji wystawiony dla bazy danych obejmuje wszystkie kolumny tabeli, zapewniając, że update lub delete tylko wówczas, gdy dane bieżącego rekordu jest zgodna z oryginalnymi danymi użytkownik miał kiedy wykonywanie ich update lub delete. Gdy warstwy DAL została skonfigurowana obsługa optymistycznej współbieżności, należy zaktualizować metody LOGIKI. Ponadto strony ASP.NET, która wywołuje w dół do LOGIKI musi być skonfigurowany w taki sposób, że kontrolki ObjectDataSource pobiera oryginalnych wartości z formantu sieci Web swoje dane i przekazuje je w dół do LOGIKI.

Jak widzieliśmy w ramach tego samouczka, implementacja mechanizmu kontroli optymistycznej współbieżności w aplikacji sieci web ASP.NET obejmuje aktualizację DAL i logiki warstwy Biznesowej i dodanie obsługi na stronie ASP.NET. Określa, czy dodano prac jest poddanie inwestycji, czas i nakład pracy, zależy od aplikacji. Jeśli masz rzadko równoczesnych użytkowników, aktualizowanie danych lub dane, które aktualizują różni się od siebie nawzajem, następnie kontrola współbieżności nie jest kluczową kwestią. Jeśli jednak rutynowo masz wielu użytkowników w swojej witrynie stosowaniu tych samych danych, kontroli współbieżności może zapobiec jednego użytkownika, aktualizacji lub usuwania wirus zastępowaniu kogoś innego.

Wszystkiego najlepszego programowania!

## <a name="about-the-author"></a>Informacje o autorze

[Scott Bento](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor siedem ASP/ASP.NET książek i założycielem [4GuysFromRolla.com](http://www.4guysfromrolla.com), pracował nad przy użyciu technologii Microsoft Web od 1998 r. Scott działa jako niezależny Konsultant, trainer i składnika zapisywania. Jego najnowszą książkę Stephena [ *Sams uczyć się ASP.NET 2.0 w ciągu 24 godzin*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). ADAM można z Tobą skontaktować w [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) lub za pośrednictwem jego blogu, który znajduje się w temacie [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

> [!div class="step-by-step"]
> [Poprzednie](customizing-the-data-modification-interface-vb.md)
> [dalej](adding-client-side-confirmation-when-deleting-vb.md)
