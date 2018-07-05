---
uid: web-forms/overview/data-access/working-with-batched-data/wrapping-database-modifications-within-a-transaction-vb
title: OPAKOWYWANIE modyfikacji bazy danych w ramach transakcji (VB) | Dokumentacja firmy Microsoft
author: rick-anderson
description: Ten samouczek jest pierwszą czterech, który sprawdza, aktualizowanie, usuwanie i wstawianie partie danych. W tym samouczku będziemy Dowiedz się, jak zezwolić transakcji bazy danych...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/26/2007
ms.topic: article
ms.assetid: 7d821db5-6cbb-4b38-af14-198f9155fc82
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/data-access/working-with-batched-data/wrapping-database-modifications-within-a-transaction-vb
msc.type: authoredcontent
ms.openlocfilehash: cf87dd0e677a73dc81cc430157be22b01ff0f7a8
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/03/2018
ms.locfileid: "37390165"
---
<a name="wrapping-database-modifications-within-a-transaction-vb"></a>OPAKOWYWANIE modyfikacji bazy danych w ramach transakcji (VB)
====================
przez [Bento Scott](https://twitter.com/ScottOnWriting)

[Pobierz program Code](http://download.microsoft.com/download/3/9/f/39f92b37-e92e-4ab3-909e-b4ef23d01aa3/ASPNET_Data_Tutorial_63_VB.zip) lub [Pobierz plik PDF](wrapping-database-modifications-within-a-transaction-vb/_static/datatutorial63vb1.pdf)

> Ten samouczek jest pierwszą czterech, który sprawdza, aktualizowanie, usuwanie i wstawianie partie danych. W tym samouczku dowie się, jak zezwolić zmian usługi batch należy przeprowadzić jako operacją niepodzielną gwarantuje, że wszystkie kroki powodzenie lub niepowodzenie wszystkich kroków w transakcji bazy danych.


## <a name="introduction"></a>Wprowadzenie

Jak widzieliśmy, począwszy od [Przegląd Wstawianie, aktualizowanie i usuwanie danych](../editing-inserting-and-deleting-data/an-overview-of-inserting-updating-and-deleting-data-vb.md) samouczku widoku GridView udostępnia wbudowaną obsługę na poziomie wiersza, edytowania i usuwania. Za pomocą kilku kliknięć myszą jest możliwe utworzenie interfejsu modyfikacji danych bez konieczności pisania nawet wiersza kodu, tak długo, jak się zawartość edytowania i usuwania na zasadzie na wiersz. Jednak w niektórych scenariuszach to za mało, więc chcemy zapewnić użytkownikom możliwość edytowania lub usunięcia partii rekordów.

Na przykład najbardziej opartego na sieci web klientów poczty e-mail umożliwia siatki listy każdy komunikat gdzie każdy wiersz zawiera pola wyboru wraz z informacjami s poczty e-mail (temat, nadawca i tak dalej). Ten interfejs umożliwia użytkownikowi usuwanie wielu komunikatów, zaznaczając je, a następnie klikając przycisk Usuń zaznaczone komunikaty o. Edytowanie interfejsu usługi batch jest idealne w sytuacji, w którym użytkownicy często edytować wiele różnych rekordów. Zamiast zmuszania użytkownika, kliknij pozycję Edytuj, wprowadzić swoje zmiany i następnie kliknij przycisk Aktualizuj dla każdego rekordu, który ma zostać zmodyfikowana, partii edytowanie interfejsu renderuje każdy wiersz kończy jego edycji interfejsu. Użytkownika można szybko zmodyfikować zestawu wierszy, które muszą zostać zmienione, a następnie zapisz te zmiany, klikając przycisk Aktualizuj wszystkie. W tym zestawie samouczków zajmiemy się, jak utworzyć interfejsy do wstawiania, edytowanie i usuwanie partii danych.

Podczas wykonywania operacji wsadowych jego ważne określić, czy powinno być możliwe w przypadku niektórych operacji w usłudze batch zakończyło się sukcesem, podczas gdy inne się nie powieść. Należy wziąć pod uwagę partii usuwanie interfejs — co ma się zdarzyć, jeśli pierwszy wybrany rekord został pomyślnie usunięty, ale drugi kończy się niepowodzeniem, powiedz, ze względu na naruszenie ograniczenia klucza obcego? Najpierw usuń rekord s konieczność wycofania, czy jest to dopuszczalne dla pierwszego rekordu zachować usuniętych?

Jeśli chcesz, aby operacji zbiorczej powinien być traktowany jako [niepodzielna operacja](http://en.wikipedia.org/wiki/Atomic_operation), jeden gdzie albo wszystkich kroków powiedzie się lub wszystkie kroki zakończyć się niepowodzeniem, a następnie warstwy dostępu do danych musi zostać rozszerzony o obsługę [bazy danych Transakcje](http://en.wikipedia.org/wiki/Database_transaction). Transakcje bazy danych gwarantuje niepodzielność zestawu `INSERT`, `UPDATE`, i `DELETE` instrukcje wykonywane w obszarze parasola transakcji i są obsługiwane przez większość systemów nowoczesnych bazy danych wszystkich funkcji.

W tym samouczku Zapoznamy się jak rozszerzyć warstwę DAL, aby móc używać transakcji bazy danych. Kolejne samouczki zbada Implementowanie strony sieci web dla usługi batch, wstawianie, aktualizowanie i usuwanie interfejsów. Rozpocznij pracę dzięki s!

> [!NOTE]
> Podczas modyfikowania danych w transakcji usługi batch, niepodzielność nie jest zawsze potrzebny. W niektórych scenariuszach może być możliwe do zaakceptowania mają pewne modyfikacje danych powiodło się i inni użytkownicy w tej samej partii nie powiedzie się, takie jak czas usuwania zestawu wiadomości e-mail z klienta e-mail opartego na sieci web. Jeśli występują s midway błąd bazy danych, poprzez usunięcie procesu jej s prawdopodobnie dopuszczalne usunięto pozostawienie tych rekordów przetwarzania bez błędów. W takich przypadkach warstwy DAL nie musi zostać zmodyfikowane w celu obsługi transakcji bazy danych. Brak innych operacji scenariuszach wsadowych, jednak gdzie niepodzielność jest istotne. Jeśli klient jej środków jest przenoszony z jednego konta bankowego do innego, należy wykonać dwie operacje: środków należy odjąć od pierwszego konta i następnie dodawane do drugiego. Bank może nie mieć nic przeciwko po pierwszym krokiem powiedzie się, ale drugi etap się nie powieść, swoim klientom understandably byłaby zła, ponieważ. Zachęcam Cię do pracy za pomocą tego samouczka i wdrożenie rozszerzenia z warstwą dal do obsługi transakcji bazy danych, nawet jeśli nie planujesz używania ich w partii, wstawianie, aktualizowanie i usuwanie interfejsów, które będziemy tworzyć w następujących trzech samouczków.


## <a name="an-overview-of-transactions"></a>Przegląd transakcji

Większość baz danych obejmują obsługę *transakcji*, która umożliwia wielu poleceń bazy danych można podzielić na pojedynczą jednostką logiczną pracy. Polecenia bazy danych, wchodzące w skład transakcji jest gwarantowana niepodzielnych, co oznacza, że wszystkie polecenia zakończy się niepowodzeniem lub wszystkie zakończy się powodzeniem.

Ogólnie rzecz biorąc transakcje są implementowane za pomocą instrukcji języka SQL przy użyciu następującego wzorca:

1. Wskazuje początek transakcji.
2. Wykonaj instrukcje SQL, wchodzące w skład transakcji.
3. Jeśli występuje błąd w jednej instrukcji z kroku nr 2 wycofywania transakcji.
4. Jeśli wszystkie instrukcje z kroku nr 2 zakończy się bez błędów, należy zatwierdzić transakcji.

Instrukcje SQL używane do tworzenia, zatwierdzania i wycofać transakcji, można wprowadzić ręcznie podczas pisania skryptów SQL lub tworzenie procedur składowanych lub za pośrednictwem programowe oznacza, że za pomocą ADO.NET lub klas w [ `System.Transactions` przestrzeń nazw](https://msdn.microsoft.com/library/system.transactions.aspx). W tym samouczku będziemy tylko sprawdzać Zarządzanie transakcji za pomocą ADO.NET. W przyszłości samouczku przedstawiony zostanie sposób użycia procedur składowanych w warstwie dostępu do danych, które omówimy następujące zagadnienia instrukcji SQL do tworzenia, wycofywanie i zatwierdzania transakcji. W międzyczasie, zapoznaj się z [zarządzania transakcji w procedur składowanych serwera SQL](http://www.4guysfromrolla.com/webtech/080305-1.shtml) Aby uzyskać więcej informacji.

> [!NOTE]
> [ `TransactionScope` Klasy](https://msdn.microsoft.com/library/system.transactions.transactionscope.aspx) w `System.Transactions` przestrzeń nazw umożliwia deweloperom programowo zawinąć serię instrukcji w zakresie transakcji i obejmuje obsługę złożonych transakcji, które obejmują wiele źródeł, takich jak dwóch różnych bazach danych lub nawet heterogenicznych typów magazynów danych, takich jak bazy danych programu Microsoft SQL Server, Oracle database oraz usługi sieci Web. Czy mogę ve zdecydowała się na potrzeby tego samouczka, a nie przy użyciu transakcji ADO.NET `TransactionScope` klasy ADO.NET jest bardziej szczegółowe dla transakcji bazy danych, a w wielu przypadkach jest znacznie mniej dużej ilości zasobów. Ponadto w pewnych scenariuszach `TransactionScope` klasa używa transakcji Koordynator MSDTC (Microsoft Distributed). Problemy konfiguracji, wdrażania i wydajności otaczającego MSDTC sprawia, że zamiast wyspecjalizowanych i zaawansowane tematu i poza zakres tego samouczka.


Podczas pracy z dostawcy SqlClient w ADO.NET, transakcje są inicjowane za pomocą wywołania [ `SqlConnection` klasy](https://msdn.microsoft.com/library/system.data.sqlclient.sqlconnection.aspx) s [ `BeginTransaction` metoda](https://msdn.microsoft.com/library/system.data.sqlclient.sqlconnection.begintransaction.aspx), co powoduje zwrócenie [ `SqlTransaction` obiektu](https://msdn.microsoft.com/library/system.data.sqlclient.sqltransaction.aspx). Instrukcje modyfikacji danych, które korzeń transakcji są umieszczane w ramach `try...catch` bloku. Jeśli wystąpi błąd w instrukcji w `try` block przenosi wykonanie do `catch` bloku, w którym transakcji można wycofać za pośrednictwem `SqlTransaction` obiektu s [ `Rollback` metoda](https://msdn.microsoft.com/library/system.data.sqlclient.sqltransaction.rollback.aspx). Jeśli wszystkie instrukcje zakończy się pomyślnie, wywołanie `SqlTransaction` obiektu s [ `Commit` metoda](https://msdn.microsoft.com/library/system.data.sqlclient.sqltransaction.commit.aspx) na końcu `try` bloku zatwierdzeń transakcji. Poniższy fragment kodu ilustruje ten wzorzec. Zobacz [utrzymania spójności bazy danych z transakcjami](http://aspnet.4guysfromrolla.com/articles/072705-1.aspx) dodatkowej składni i przykłady za pomocą transakcji za pomocą narzędzia ADO.NET.


[!code-vb[Main](wrapping-database-modifications-within-a-transaction-vb/samples/sample1.vb)]

Domyślnie adapterów TableAdapter w zestawie danych wpisane nie należy używać transakcji. Aby zapewnić obsługę dla transakcji należy rozszerzyć klasy TableAdapter, aby uwzględnić dodatkowe metody, za pomocą wzorca powyżej szereg instrukcji modyfikacji danych w zakresie transakcji. W kroku 2 Zobaczymy się, jak używać klas częściowych można dodać te metody.

## <a name="step-1-creating-the-working-with-batched-data-web-pages"></a>Krok 1: Tworzenie pracy ze stronami sieci Web partiami danych

Zanim zaczniemy, eksplorowanie sposób rozszerzyć warstwę DAL do obsługi transakcji bazy danych, niech s najpierw Poświęć chwilę, do tworzenia strony sieci web ASP.NET, które są wymagane na potrzeby tego samouczka i trzech, które należy wykonać. Rozpocznij od dodania nowy folder o nazwie `BatchData` , a następnie dodaj następujące strony ASP.NET, kojarzenie każdej strony zawierającej `Site.master` strony wzorcowej.

- `Default.aspx`
- `Transactions.aspx`
- `BatchUpdate.aspx`
- `BatchDelete.aspx`
- `BatchInsert.aspx`


![Dodawanie stron ASP.NET związane z kontrolką SqlDataSource samouczki](wrapping-database-modifications-within-a-transaction-vb/_static/image1.gif)

**Rysunek 1**: Dodawanie stron ASP.NET związane z kontrolką SqlDataSource samouczki


Podobnie jak w przypadku innych folderów `Default.aspx` użyje `SectionLevelTutorialListing.ascx` kontrolki użytkownika, aby wyświetlić listę samouczków w obrębie sekcji. W związku z tym, Dodaj ten formant użytkownika do `Default.aspx` , przeciągając go z poziomu Eksploratora rozwiązań na stronę s widoku projektu.


[![Dodaj formant użytkownika SectionLevelTutorialListing.ascx na Default.aspx](wrapping-database-modifications-within-a-transaction-vb/_static/image2.gif)](wrapping-database-modifications-within-a-transaction-vb/_static/image1.png)

**Rysunek 2**: Dodaj `SectionLevelTutorialListing.ascx` kontrolki użytkownika do `Default.aspx` ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](wrapping-database-modifications-within-a-transaction-vb/_static/image2.png))


Wreszcie, Dodaj te cztery strony jako wpisy, aby `Web.sitemap` pliku. W szczególności należy dodać następujące znaczniki po Dostosowywanie mapy witryny `<siteMapNode>`:


[!code-xml[Main](wrapping-database-modifications-within-a-transaction-vb/samples/sample2.xml)]

Po zaktualizowaniu `Web.sitemap`, Poświęć chwilę, aby wyświetlić witrynę sieci Web w samouczkach, za pośrednictwem przeglądarki. Menu po lewej stronie zawiera teraz elementów do pracy z samouczkami partiami danych.


![Mapa witryny zawiera teraz wpisy dotyczące pracy z samouczkami partiami danych](wrapping-database-modifications-within-a-transaction-vb/_static/image3.gif)

**Rysunek 3**: mapy witryny zawiera teraz wpisy dotyczące pracy z samouczkami partiami danych


## <a name="step-2-updating-the-data-access-layer-to-support-database-transactions"></a>Krok 2: Trwa aktualizowanie warstwy dostępu do danych do obsługi transakcji bazy danych

Jak wspomniano w pierwszym samouczku [Tworzenie warstwy dostępu do danych](../introduction/creating-a-data-access-layer-vb.md), wpisana zestawu danych w naszym DAL składa się z DataTable i TableAdapters. DataTable przechowywania danych, podczas gdy elementów TableAdapter udostępniają funkcje, które można odczytać danych z bazy danych do DataTable, aby zaktualizować bazę danych ze zmianami wprowadzonymi do DataTable i tak dalej. Pamiętaj, że elementów TableAdapter zapewnić dwa wzorce dla uaktualnienie danych, którego mogę nazywa się aktualizacji usługi Batch i bazy danych bezpośrednio. Za pomocą wzorca aktualizacji wsadowych TableAdapter jest przekazywany w DataSet, DataTable lub kolekcji wierszy danych. Te dane są wyliczane i dla każdego wstawiania, zmodyfikować lub usunąć wiersza, `InsertCommand`, `UpdateCommand`, lub `DeleteCommand` jest wykonywany. Za pomocą wzorca bazy danych bezpośrednio TableAdapter jest przekazywany zamiast wartości kolumn, które są niezbędne do wstawiania, aktualizowania lub usuwania pojedynczy rekord. Metody bezpośredniej DB wzorzec następnie używa tych wartości przekazywane w wykonać odpowiednie `InsertCommand`, `UpdateCommand`, lub `DeleteCommand` instrukcji.

Niezależnie od tego wzorca aktualizacji używane metody automatycznego generowania adapterów TableAdapter nie należy używać transakcji. Domyślnie każdy insert, update lub delete wykonywanych przez TableAdapter jest traktowany jako pojedyncza operacja dyskretnych. Na przykład załóżmy, że wzorzec bezpośrednio z bazy danych jest używany przez jakiś kod w LOGIKI można wstawić dziesięć rekordów do bazy danych. Ten kod może wywołać metodę TableAdapter `Insert` metoda dziesięć razy. Jeśli pięciu pierwszych wstawia powiedzie się, ale szóstego co spowodowało wyjątek, pierwsze pięć rekordów wstawiono pozostanie w bazie danych. Podobnie, jeśli wzorzec aktualizacji usługi Batch jest używany do wykonywania operacji wstawiania, aktualizacji i usuwania do wstawionego, modyfikacji i usunięto wierszy w DataTable, jeśli pierwsze kilka zmian zakończyło się pomyślnie, ale późniejszą napotkał błąd, te wcześniejszych zmian, Ukończono pozostanie w bazie danych.

W niektórych scenariuszach chcemy zapewnić niepodzielność w szereg zmian. Firma Microsoft musi ręcznie rozszerzyć TableAdapter, dodając nowe metody, które są wykonywane w tym celu `InsertCommand`, `UpdateCommand`, i `DeleteCommand` s w obszarze parasola transakcji. W [Tworzenie warstwy dostępu do danych](../introduction/creating-a-data-access-layer-vb.md) przyjrzeliśmy się przy użyciu [klas częściowych](http://en.wikipedia.org/wiki/Partial_type) do rozszerzenia funkcji DataTable w DataSet wpisane. Tej techniki można również za pomocą adapterów TableAdapter.

Wpisany zestaw danych `Northwind.xsd` znajduje się w `App_Code` folderu s `DAL` podfolderu. Utwórz podfolder w `DAL` folder o nazwie `TransactionSupport` i Dodaj plik klasy o nazwie `ProductsTableAdapter.TransactionSupport.vb` (zobacz rysunek 4). Ten plik będzie zawierać wdrożony częściowo `ProductsTableAdapter` zawierającej metody służące do wykonywania modyfikacji danych przy użyciu transakcji.


![Dodaj Folder o nazwie TransactionSupport i plik klasy o nazwie ProductsTableAdapter.TransactionSupport.vb](wrapping-database-modifications-within-a-transaction-vb/_static/image4.gif)

**Rysunek 4**: Dodaj Folder o nazwie `TransactionSupport` i plik klasy o nazwie `ProductsTableAdapter.TransactionSupport.vb`


Wprowadź następujący kod do `ProductsTableAdapter.TransactionSupport.vb` pliku:


[!code-vb[Main](wrapping-database-modifications-within-a-transaction-vb/samples/sample3.vb)]

`Partial` Wskazuje — słowo kluczowe w deklaracji klasy, w tym miejscu, aby kompilator, że dodane w obrębie elementów członkowskich mają być dodawane do `ProductsTableAdapter` klasy w `NorthwindTableAdapters` przestrzeni nazw. Uwaga `Imports System.Data.SqlClient` instrukcji w górnej części pliku. Ponieważ TableAdapter został skonfigurowany do używania dostawcy SqlClient, wewnętrznie używa [ `SqlDataAdapter` ](https://msdn.microsoft.com/library/system.data.sqlclient.sqldataadapter.aspx) obiekt do wysyłania poleceń do bazy danych. W związku z tym, należy wykonać `SqlTransaction` klasy, aby rozpocząć transakcji, a następnie przekazać go ani go wycofać. Jeśli używasz magazynu danych innych niż Microsoft SQL Server, należy użyć odpowiedniego dostawcy.

Metody te zapewniają bloki konstrukcyjne potrzebne do uruchomienia, wycofywania i zatwierdzić transakcji. Są one oznaczone `Public`, umożliwiając im można używać z poziomu `ProductsTableAdapter`z innej klasy w warstwy DAL i z innej warstwy w architekturze, takich jak LOGIKI. `BeginTransaction` Otwiera wewnętrzny s TableAdapter `SqlConnection` (jeśli jest to konieczne), rozpoczyna się transakcji, a następnie przypisuje go do `Transaction` właściwości i dołącza transakcji do wewnętrznego `SqlDataAdapter` s `SqlCommand` obiektów. `CommitTransaction` i `RollbackTransaction` wywołania `Transaction` obiektu s `Commit` i `Rollback` metod, odpowiednio, przed zamknięciem wewnętrzny `Connection` obiektu.

## <a name="step-3-adding-methods-to-update-and-delete-data-under-the-umbrella-of-a-transaction"></a>Krok 3: Dodawanie metod do aktualizowania i usuwania danych w obszarze parasola transakcji

Za pomocą tych metod pełną, możemy ponownie gotowa do Dodaj metody do `ProductsDataTable` lub LOGIKI, które wykonują serię poleceń w obszarze parasola transakcji. Poniższa metoda używa wzorca aktualizacji usługi Batch do aktualizacji `ProductsDataTable` przy użyciu transakcji. Rozpoczyna transakcji wywoływania `BeginTransaction` metody, a następnie używa `Try...Catch` bloku, aby wydać instrukcji modyfikacji danych. Jeśli wywołanie `Adapter` obiektu s `Update` wykonanie metody powoduje wyjątek, zostaną przeniesione do `catch` blok, gdy transakcja zostanie wycofana i ponownie zgłoszony wyjątek. Pamiętamy `Update` metoda implementuje wzorzec aktualizacji usługi Batch, wyliczając wiersze podane `ProductsDataTable` i wykonując niezbędne `InsertCommand`, `UpdateCommand`, i `DeleteCommand` s. Jeśli jeden z tych poleceń będzie skutkowało błędem, transakcja zostanie wycofana, cofnięcie wcześniejszych zmian wprowadzonych w okresie istnienia s transakcji. Należy `Update` instrukcji zakończenia bez błędów, transakcja zostaje zatwierdzona w całości.


[!code-vb[Main](wrapping-database-modifications-within-a-transaction-vb/samples/sample4.vb)]

Dodaj `UpdateWithTransaction` metody `ProductsTableAdapter` klasy do klasy częściowej w `ProductsTableAdapter.TransactionSupport.vb`. Alternatywnie, można dodać tej metody w s warstwy logiki biznesowej `ProductsBLL` klasy za pomocą kilku drobne zmiany syntaktyczne. To znaczy, słowo kluczowe `Me` w `Me.BeginTransaction()`, `Me.CommitTransaction()`, i `Me.RollbackTransaction()` musi zostać zamieniona `Adapter` (pamiętamy `Adapter` jest nazwą właściwości w `ProductsBLL` typu `ProductsTableAdapter`).

`UpdateWithTransaction` Metoda używa wzorca aktualizacji usługi Batch, ale może również służyć szereg wywołań bazy danych bezpośrednio w zakresie transakcji, co pokazuje poniższa metoda. `DeleteProductsWithTransaction` Metoda przyjmuje jako dane wejściowe `List(Of T)` typu `Integer`, które są `ProductID` s do usunięcia. Metoda inicjuje transakcji za pośrednictwem wywołania `BeginTransaction` a następnie w `Try` zablokować, iterację na liście podany wzorzec DB bezpośrednie wywołanie `Delete` metody dla każdego `ProductID` wartość. Jeśli dowolny z wywołania `Delete` zakończy się niepowodzeniem, kontrola jest przekazywana do `Catch` blok, gdy transakcja zostanie wycofana i ponownie zgłoszony wyjątek. Jeśli wszystkie wywołania `Delete` powiedzie się, a następnie transakcja została zatwierdzona. Dodaj tę metodę w celu `ProductsBLL` klasy.


[!code-vb[Main](wrapping-database-modifications-within-a-transaction-vb/samples/sample5.vb)]

## <a name="applying-transactions-across-multiple-tableadapters"></a>Stosowanie transakcji w przypadku wielu TableAdapters

Umożliwia użycie wielu instrukcji względem transakcji powiązanych z kodu, w tym samouczku `ProductsTableAdapter` powinien być traktowany jako operację niepodzielną. Ale co zrobić, jeśli wprowadzanie wielu modyfikacji innych tabelach bazy danych muszą być wykonywane atomowo? Na przykład podczas usuwania kategorii, firma Microsoft może najpierw chcesz ponownie przypisać jej aktualne produkty do niektórych innych kategorii. Następujące dwa kroki ponowne przypisywanie produktów i usuwania kategorii powinna zostać wykonana jako operację niepodzielną. Ale `ProductsTableAdapter` obejmuje tylko metody modyfikowania `Products` tabeli i `CategoriesTableAdapter` obejmuje tylko metody modyfikowania `Categories` tabeli. W jaki sposób transakcji może obejmować zarówno TableAdapters

Dodaj metodę, która jest jedną z opcji `CategoriesTableAdapter` o nazwie `DeleteCategoryAndReassignProducts(categoryIDtoDelete, reassignToCategoryID)` i ma tę metodę wywołać procedurę składowaną, ponownie przypisuje produktów i usuwa kategorii w zakresie transakcji zdefiniowanych w ramach procedury składowanej. Omówimy sposób rozpoczęcia, zatwierdzanie i wycofywania transakcji w procedurach składowanych w przyszłości zapoznać się z samouczkiem.

Innym rozwiązaniem jest utworzenie klasę pomocy w DAL, który zawiera `DeleteCategoryAndReassignProducts(categoryIDtoDelete, reassignToCategoryID)` metody. Ta metoda może utworzyć wystąpienia `CategoriesTableAdapter` i `ProductsTableAdapter` , a następnie ustaw te dwa TableAdapters `Connection` właściwości do tej samej `SqlConnection` wystąpienia. At tego punktu, jeden z dwóch elementów TableAdapter może zainicjować transakcji przy użyciu wywołania do `BeginTransaction`. Czy można wywołać metody TableAdapter, ponowne przypisywanie produktów i usuwania kategorii w `Try...Catch` blok z transakcja przekazana lub wycofana ponownie zgodnie z potrzebami.

## <a name="step-4-adding-theupdatewithtransactionmethod-to-the-business-logic-layer"></a>Krok 4. Dodawanie`UpdateWithTransaction`metody warstwy logiki biznesowej

W kroku 3 dodaliśmy `UpdateWithTransaction` metody `ProductsTableAdapter` w warstwy DAL. Możemy dodać do LOGIKI odpowiadającą im metodę. Podczas gdy Warstwa prezentacji może wywołać bezpośrednio do warstwy DAL do wywołania `UpdateWithTransaction` metody, w tych samouczkach mają strived do definiowania architektury warstwowej, który powoduje, że warstwy DAL z warstwy prezentacji. W związku z tym jego behooves nam to podejście.

Otwórz `ProductsBLL` klasy plików i Dodaj metodę o nazwie `UpdateWithTransaction` który po prostu wywołuje metodę do odpowiedniej metody warstwy DAL. Teraz należy dwóch nowych metod w `ProductsBLL`: `UpdateWithTransaction`, który właśnie został dodany, i `DeleteProductsWithTransaction`, który został dodany w kroku 3.


[!code-vb[Main](wrapping-database-modifications-within-a-transaction-vb/samples/sample6.vb)]

> [!NOTE]
> Te metody nie obejmują `DataObjectMethodAttribute` atrybut przypisany do większości metod w `ProductsBLL` klasy, ponieważ firma Microsoft będzie wywoływanie tych metod bezpośrednio z klasy CodeBehind stron ASP.NET. Pamiętamy `DataObjectMethodAttribute` służy do flagi, jakie metody powinna zostać wyświetlona na ObjectDataSource s Konfigurowanie źródła danych pracę kreatora i jakie karcie (SELECT, UPDATE, INSERT lub DELETE). Ponieważ w widoku GridView nie posiada wbudowanej pomocy technicznej dla usługi batch, edytowania lub usuwania, będziemy musieli wywoływanie tych metod programowo, zamiast używać podejścia deklaratywnego niekorzystające z kodu.


## <a name="step-5-atomically-updating-database-data-from-the-presentation-layer"></a>Krok 5: Niepodzielne aktualizowanie danych bazy danych z warstwy prezentacji

Aby zilustrować wpływ transakcji podczas aktualizowania partii rekordów, umożliwiają s utworzyć interfejs użytkownika, który zawiera wszystkie produkty z GridView i Web przycisk kontrolkę, która, po kliknięciu ponownie przypisuje produktów `CategoryID` wartości. W szczególności, ponowne przypisanie kategorii przyszłego postępu tak, aby pierwsze kilka produktów są przypisane prawidłowe `CategoryID` nieistniejącej przypisano jej wartości, a inne są celowo `CategoryID` wartość. Jeśli próby aktualizacji bazy danych z produktem, którego `CategoryID` jest niezgodna z istniejącą kategorię s `CategoryID`, nastąpi naruszenie ograniczenia klucza obcego i będzie zgłaszany wyjątek. Zobaczymy w tym przykładzie jest to, że przy użyciu transakcji wyjątek zgłoszony z naruszenie ograniczenia klucza obcego spowoduje, że poprzednie prawidłowe `CategoryID` zmiany można wycofać. Bez korzystania z transakcji, jednak będą nadal modyfikacje do początkowego kategorii.

Zacznij od otwarcia `Transactions.aspx` stronie `BatchData` folder i przeciągnij GridView z przybornika do projektanta. Ustaw jego `ID` do `Products` i z jego tag inteligentny powiązać go do nowego elementu ObjectDataSource, o nazwie `ProductsDataSource`. Konfigurowanie kontrolki ObjectDataSource swoich danych z `ProductsBLL` klasy s `GetProducts` metody. Zostanie można GridView tylko do odczytu, więc zestawu list rozwijanych w UPDATE, INSERT i usuwanie kart (Brak) i kliknij przycisk Zakończ.


[![Konfigurowanie kontrolki ObjectDataSource przy użyciu metody GetProducts ProductsBLL klasy s](wrapping-database-modifications-within-a-transaction-vb/_static/image5.gif)](wrapping-database-modifications-within-a-transaction-vb/_static/image3.png)

**Rysunek 5**: Konfigurowanie kontrolki ObjectDataSource do użycia `ProductsBLL` klasy s `GetProducts` — metoda ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](wrapping-database-modifications-within-a-transaction-vb/_static/image4.png))


[![Ustaw list rozwijanych w UPDATE, INSERT i usuwanie kart (Brak)](wrapping-database-modifications-within-a-transaction-vb/_static/image6.gif)](wrapping-database-modifications-within-a-transaction-vb/_static/image5.png)

**Rysunek 6**: Ustaw listy rozwijane w aktualizacji, WSTAWIANIA i usuwania karty (Brak) ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](wrapping-database-modifications-within-a-transaction-vb/_static/image6.png))


Po zakończeniu pracy kreatora Konfigurowanie źródła danych, program Visual Studio utworzy BoundFields i CheckBoxField dla pól danych produktu. Usuń wszystkie te pola z wyjątkiem `ProductID`, `ProductName`, `CategoryID`, i `CategoryName` i Zmień nazwę `ProductName` i `CategoryName` BoundFields `HeaderText` właściwości do produktu i kategorii, odpowiednio. Za pomocą tagu inteligentnego zaznacz opcję włączenia stronicowania. Po wprowadzeniu tych zmian, kontrolkami GridView i kontrolki ObjectDataSource s oznaczeniu deklaracyjnym powinien wyglądać następująco:


[!code-aspx[Main](wrapping-database-modifications-within-a-transaction-vb/samples/sample7.aspx)]

Następnie dodaj trzy kontrolki przycisku w sieci Web powyżej widoku GridView. Wartość pierwszy przycisk s właściwości Text Odśwież siatki, drugi s, aby zmodyfikować kategorie (przy użyciu transakcji), a trzeci jeden s, aby zmodyfikować kategorie (bez transakcji).


[!code-aspx[Main](wrapping-database-modifications-within-a-transaction-vb/samples/sample8.aspx)]

W tym momencie widok projektu w programie Visual Studio, powinny wyglądać podobnie do ekranu zrzut, jak pokazano na rysunku 7.


[![Ta strona zawiera GridView i trzech przycisków umieszczonych w sieci Web](wrapping-database-modifications-within-a-transaction-vb/_static/image7.gif)](wrapping-database-modifications-within-a-transaction-vb/_static/image7.png)

**Rysunek 7**: strona zawiera GridView i trzy kontrolki sieci Web przycisku ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](wrapping-database-modifications-within-a-transaction-vb/_static/image8.png))


Tworzenie obsługi zdarzeń dla każdego z trzech przycisk s `Click` zdarzenia i użyj następującego kodu:


[!code-vb[Main](wrapping-database-modifications-within-a-transaction-vb/samples/sample9.vb)]

Odświeżanie przycisk s `Click` programu obsługi zdarzeń po prostu rebinds dane do widoku GridView przez wywołanie metody `Products` GridView s `DataBind` metody.

Drugi program obsługi zdarzeń ponownie przypisuje produktów `CategoryID` s i używa nowej metody transakcji od LOGIKI do wydajna niż baza danych aktualizacji w obszarze parasola transakcji. Należy pamiętać, że każdy produkt s `CategoryID` arbitralnie jest ustawiona na taką samą wartość jak jego `ProductID`. To będzie działać prawidłowo w pierwszym kilka produktów, ponieważ te produkty mają `ProductID` wartości, które zdarzają się do mapowania na nieprawidłowy `CategoryID` s. Ale raz `ProductID` start s wprowadzenie zbyt duży, to przypadkowe nachodzące na siebie `ProductID` s i `CategoryID` s nie ma już zastosowania.

Trzeci `Click` programu obsługi zdarzeń aktualizacji produktów `CategoryID` s w taki sam sposób, ale wysyła aktualizacji do bazy danych przy użyciu `ProductsTableAdapter` domyślne s `Update` metody. To `Update` metoda nie jest zawijany serię poleceń w ramach transakcji, aby zmiany zostały wprowadzone przed pierwszym błędzie naruszenie napotkano ograniczenie klucza obcego utrwali.

Aby zademonstrować to zachowanie, odwiedź tę stronę za pośrednictwem przeglądarki. Początkowo pierwszej strony danych powinny Zobacz, jak pokazano na rysunku 8. Następnie kliknij przycisk zmodyfikować kategorie (przy użyciu transakcji). Spowoduje to powoduje odświeżenie strony i próba aktualizacji wszystkich produktów `CategoryID` wartości, ale będą powodować naruszenie ograniczenia klucza obcego (patrz rysunek 9).


[![Produkty są wyświetlane w stronicowanej widoku GridView](wrapping-database-modifications-within-a-transaction-vb/_static/image8.gif)](wrapping-database-modifications-within-a-transaction-vb/_static/image9.png)

**Rysunek 8**: produkty są wyświetlane w stronicowanej kontrolki GridView ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](wrapping-database-modifications-within-a-transaction-vb/_static/image10.png))


[![Ponowne przypisywanie kategorii skutkuje naruszenie ograniczenia klucza obcego](wrapping-database-modifications-within-a-transaction-vb/_static/image9.gif)](wrapping-database-modifications-within-a-transaction-vb/_static/image11.png)

**Rysunek 9**: ponowne przypisywanie kategorii skutkuje naruszenie ograniczenia klucza obcego ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](wrapping-database-modifications-within-a-transaction-vb/_static/image12.png))


Teraz, kliknij przycisk Wstecz Twojej przeglądarki s, a następnie kliknij przycisk Odśwież siatki. Po odświeżeniu danych powinny być widoczne dokładnie ten sam wynik, jak pokazano na rysunku 8. Oznacza to, nawet jeśli niektóre produkty `CategoryID` s zostały zmienione na prawne wartości i zaktualizowane w bazie danych, ich zostały wycofane po wystąpiło naruszenie ograniczenia klucza obcego.

Teraz spróbuj, kliknięcie przycisku Modyfikuj kategorii (bez transakcji). Spowoduje to ten sam błąd naruszenie ograniczenia klucza obcego (patrz rysunek 9), ale tym razem tych produktów którego `CategoryID` wartości zostały zmienione na prawnych i wartość będą nie można wycofać. Wprowadzam polecenie przeglądarki s przycisku Wstecz, a następnie przycisk Odśwież siatki. Jak pokazano na rysunku nr 10, `CategoryID` ponownie przypisane s produktów pierwsze osiem. Na przykład na rysunku 8 zmian centralnych miał `CategoryID` 1, ale w rysunek 10 it s przypisane do 2.


[![Niektóre wartości CategoryID produktów zostały zaktualizowane podczas gdy inne osoby zostały nie](wrapping-database-modifications-within-a-transaction-vb/_static/image10.gif)](wrapping-database-modifications-within-a-transaction-vb/_static/image13.png)

**Na rysunku nr 10**: niektóre produkty `CategoryID` wartości zostały zaktualizowane podczas gdy inne osoby zostały nie ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](wrapping-database-modifications-within-a-transaction-vb/_static/image14.png))


## <a name="summary"></a>Podsumowanie

Domyślnie metody s TableAdapter nie jest zawijany instrukcji wykonanych bazy danych w zakresie transakcji, ale z niewielką ilością pracy możemy dodać metody, które spowoduje utworzenie, zatwierdzenia i wycofać transakcji. W tym samouczku utworzyliśmy tych trzech metod w `ProductsTableAdapter` klasy: `BeginTransaction`, `CommitTransaction`, i `RollbackTransaction`. Widzieliśmy sposób użycia tych metod wraz z `Try...Catch` bloku, aby upewnić atomic szereg instrukcji modyfikacji danych. W szczególności utworzyliśmy `UpdateWithTransaction` method in Class metoda `ProductsTableAdapter`, który używa wzorca aktualizacji usługi Batch do wykonania niezbędnych zmian w wierszach, podane `ProductsDataTable`. Dodaliśmy również `DeleteProductsWithTransaction` metody `ProductsBLL` klasy w LOGIKI, która przyjmuje `List` z `ProductID` wartości jako dane wejściowe, a następnie wywołuje metodę wzorca bazy danych bezpośrednio `Delete` dla każdego `ProductID`. Obie metody start tworzenia transakcji, a następnie wykonywania instrukcji modyfikacji danych w ramach `Try...Catch` bloku. Jeśli wystąpi wyjątek, transakcja zostanie wycofana, w przeciwnym razie zostanie zatwierdzony.

Krok 5 ilustruje efekt aktualizacji partii transakcji, jak i aktualizacje usługi batch, które zwrócenia przez niego użyć transakcji. W ramach trzech kolejnych samouczków Rozpoczniemy bazują na podstawę, w tym samouczku i twórz interfejsy użytkownika umożliwiające wykonywanie aktualizacji wsadowych, usunięcia i wstawienia.

Wszystkiego najlepszego programowania!

## <a name="further-reading"></a>Dalsze informacje

Więcej informacji na tematów omówionych w tym samouczku można znaleźć w następujących zasobach:

- [Utrzymywanie spójności bazy danych za pomocą transakcji](http://aspnet.4guysfromrolla.com/articles/072705-1.aspx)
- [Zarządzanie transakcjami w programie SQL Server procedur składowanych](http://www.4guysfromrolla.com/webtech/080305-1.shtml)
- [Transakcje łatwe: `System.Transactions`](https://blogs.msdn.com/florinlazar/archive/2004/07/23/192239.aspx)
- [TransactionScope i elementów DataAdapters](http://andyclymer.blogspot.com/2007/01/transactionscope-and-dataadapters.html)
- [Za pomocą transakcji bazy danych Oracle na platformie .NET](http://www.oracle.com/technology/pub/articles/price_dbtrans_dotnet.html)

## <a name="about-the-author"></a>Informacje o autorze

[Scott Bento](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor siedem ASP/ASP.NET książek i założycielem [4GuysFromRolla.com](http://www.4guysfromrolla.com), pracował nad przy użyciu technologii Microsoft Web od 1998 r. Scott działa jako niezależny Konsultant, trainer i składnika zapisywania. Jego najnowszą książkę Stephena [ *Sams uczyć się ASP.NET 2.0 w ciągu 24 godzin*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). ADAM można z Tobą skontaktować w [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) lub za pośrednictwem jego blogu, który znajduje się w temacie [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Specjalne podziękowania dla

W tej serii samouczków został zrecenzowany przez wielu recenzentów pomocne. Wiodące osób dokonujących przeglądu, w tym samouczku zostały Dave Gardner Hilton Giesenow i Teresa Murphy. Zainteresowani zapoznaniem Moje kolejnych artykułów MSDN? Jeśli tak, Porzuć mnie linii w [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Poprzednie](batch-inserting-cs.md)
> [dalej](batch-updating-vb.md)
