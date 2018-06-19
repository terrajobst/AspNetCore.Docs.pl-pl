---
uid: web-forms/overview/data-access/working-with-batched-data/wrapping-database-modifications-within-a-transaction-vb
title: Zawijanie modyfikacje bazy danych w transakcji (VB) | Dokumentacja firmy Microsoft
author: rick-anderson
description: W tym samouczku jest pierwszy czterech, która wygląda na aktualizowanie, usuwanie i wstawianie partie danych. W tym samouczku będziemy Dowiedz się, jak umożliwić transakcji bazy danych...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/26/2007
ms.topic: article
ms.assetid: 7d821db5-6cbb-4b38-af14-198f9155fc82
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/working-with-batched-data/wrapping-database-modifications-within-a-transaction-vb
msc.type: authoredcontent
ms.openlocfilehash: 2005561755b22f5811d011bd3146853f6cd184af
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/06/2018
ms.locfileid: "30888708"
---
<a name="wrapping-database-modifications-within-a-transaction-vb"></a>Zawijanie modyfikacje bazy danych w transakcji (VB)
====================
przez [Bento Scott](https://twitter.com/ScottOnWriting)

[Pobierz kod](http://download.microsoft.com/download/3/9/f/39f92b37-e92e-4ab3-909e-b4ef23d01aa3/ASPNET_Data_Tutorial_63_VB.zip) lub [pobierania plików PDF](wrapping-database-modifications-within-a-transaction-vb/_static/datatutorial63vb1.pdf)

> W tym samouczku jest pierwszy czterech, która wygląda na aktualizowanie, usuwanie i wstawianie partie danych. W tym samouczku będziemy Dowiedz się, jak transakcji bazy danych Zezwól na modyfikacje partii przeprowadzanych jako operacją niepodzielną gwarantuje, że wszystkie kroki powiedzie się lub wszystkie kroki zakończyć się niepowodzeniem.


## <a name="introduction"></a>Wprowadzenie

Jak widzieliśmy począwszy [omówienie Wstawianie, aktualizowanie i usuwanie danych](../editing-inserting-and-deleting-data/an-overview-of-inserting-updating-and-deleting-data-vb.md) samouczka widoku GridView udostępnia wbudowaną obsługę na poziomie wiersza, edytowanie i usuwanie. Za pomocą kilku kliknięć myszą jest można utworzyć interfejs modyfikacji zaawansowanych danych bez pisania wiersz kodu, tak długo, jak jest zawartości z edytowanie i usuwanie na podstawie na wiersz. Jednak w pewnych sytuacjach są niewystarczające, więc chcemy zapewnić użytkownikom możliwość edytować lub usunąć partii rekordów.

Na przykład większość opartych na sieci web klientów poczty e-mail umożliwia siatki listy każdy komunikat której każdy wiersz zawiera pole wyboru wraz z informacjami s poczty e-mail (podmiotu, nadawca i tak dalej). Ten interfejs umożliwia użytkownikowi na usuwanie komunikatów wielu sprawdzanie je, a następnie klikając przycisk Usuń zaznaczone komunikaty. Edytowanie interfejsu partii jest idealny w sytuacjach, gdy użytkownicy często edytować wiele rekordów. Zamiast wymuszania użytkownikowi kliknij przycisk Edytuj, wprowadzić swoje zmiany i następnie kliknij przycisk Aktualizuj dla każdego rekordu, który ma zostać zmodyfikowana, partii edycji interfejsu renderuje każdego wiersza z jego edycji interfejsu. Użytkownik może szybkiej modyfikacji zestawu wierszy, które muszą zostać zmienione, a następnie zapisz zmiany, klikając przycisk Aktualizuj wszystkie. W tym zestawie samouczki zajmiemy się, jak tworzyć interfejsy Wstawianie, edytowania i usuwania partie danych.

Podczas wykonywania operacji wsadowych go s ważne jest określenie, czy powinno być możliwe dla niektórych operacji w partii powiodło się podczas innych zakończyć się niepowodzeniem. Należy wziąć pod uwagę partii Usuwanie interfejsu — co powinno się zdarzyć, jeśli pierwszy wybranego rekordu została usunięta pomyślnie, a drugi nie powiedzie się, że, ze względu na naruszenie ograniczenia klucza obcego? Należy najpierw usuń rekord s wycofana, czy jest możliwa do pierwszy rekord pozostaje usunięto?

Jeśli chcesz, aby operacja partii powinien być traktowany jako [niepodzielną operację](http://en.wikipedia.org/wiki/Atomic_operation), co gdzie albo wszystkich kroków powiedzie się lub wszystkich kroków zakończyć się niepowodzeniem, a następnie Warstwa dostępu do danych musi zostać rozszerzony w celu uwzględnienia obsługi [bazy danych Transakcje](http://en.wikipedia.org/wiki/Database_transaction). Transakcji bazy danych zagwarantowania niepodzielności zestawu `INSERT`, `UPDATE`, i `DELETE` instrukcje wykonywane w obszarze parasola transakcji i są obsługiwane przez większość systemów nowoczesnych bazy danych wszystkich funkcji.

W tym samouczku wyjaśniono, jak rozszerzyć warstwę DAL do użycia transakcji bazy danych. Kolejnych samouczkach zbada implementującej stron sieci web dla partii, wstawianie, aktualizowanie i usuwanie interfejsów. Rozpoczynanie pracy dzięki s!

> [!NOTE]
> Podczas modyfikowania danych w transakcji partii, niepodzielność nie jest zawsze wymagana. W niektórych scenariuszach może być akceptowane ma pewne modyfikacje danych powiodło się i innych użytkowników w tej samej partii zakończą się niepowodzeniem, takie jak kiedy usuwanie zestawu wiadomości e-mail z klienta poczty e-mail hostowanego w sieci web. Jeśli występują s połowie błąd bazy danych, za pomocą usunięcia przetwarzania go s prawdopodobnie dopuszczalne pozostawienie usunięte rekordy przetwarzane bez błędów. W takich przypadkach warstwy DAL nie musi zostać zmodyfikowane w celu obsługi transakcji bazy danych. Istnieją inne wsadowe operacji zastosowaniach, jednak niepodzielność istotne. Gdy klient jej fundusze są przenoszone z jednego konta bankowego do innego, należy wykonać dwie operacje: fundusze musi być odjęta od pierwszego konta i dodawane do drugiego. Bank może nie stanowi problemu po pierwszym krokiem powiedzie się, ale drugim kroku nie powiedzie, klientom understandably będzie zły. I zachęca do pracy z tym samouczkiem i wdrożenie rozszerzenia do obsługi transakcji bazy danych, nawet jeśli nie planujesz na ich użycie w Wstawianie wsadowe, aktualizowanie i usuwanie interfejsów, które firma Microsoft będzie kompilowania w następujących samouczkach trzy warstwy DAL.


## <a name="an-overview-of-transactions"></a>Omówienie transakcji

Większość baz danych obsługują *transakcji*, umożliwiające wielu poleceń bazy danych mają być grupowane w pojedynczą jednostkę logiczną pracy. Polecenia bazy danych, które obejmują transakcji dotrą do niepodzielnych, co oznacza, że wszystkie polecenia zakończy się niepowodzeniem lub wszystkie powiedzie się.

Ogólnie rzecz biorąc transakcje są implementowane za pośrednictwem instrukcji SQL przy użyciu następującego wzorca:

1. Wskazuje początek transakcji.
2. Wykonywanie instrukcji SQL, które obejmują transakcji.
3. Jeśli występuje błąd w jednej instrukcji z kroku nr 2 wycofywania transakcji.
4. Jeśli wszystkie instrukcje z kroku nr 2 ukończone bez błędów, zatwierdzić transakcji.

Instrukcje SQL używane do tworzenia, zatwierdzania i wycofać transakcji można wprowadzić ręcznie podczas pisania skryptów SQL lub tworzenie procedur składowanych lub za pośrednictwem programowe oznacza za pomocą ADO.NET lub klas w [ `System.Transactions` przestrzeń nazw](https://msdn.microsoft.com/library/system.transactions.aspx). W tym samouczku, które zostaną omówione tylko Zarządzanie transakcji za pomocą ADO.NET. W przyszłości samouczku przedstawiono sposób użycia procedury składowane w warstwie dostępu do danych, które firma Microsoft Eksploruj instrukcji SQL do tworzenia, wycofywanie i zatwierdzania transakcji. W międzyczasie, zapoznaj się [Zarządzanie transakcji w procedur składowanych serwera SQL](http://www.4guysfromrolla.com/webtech/080305-1.shtml) Aby uzyskać więcej informacji.

> [!NOTE]
> [ `TransactionScope` Klasy](https://msdn.microsoft.com/library/system.transactions.transactionscope.aspx) w `System.Transactions` przestrzeni nazw umożliwia deweloperom programowo zawijać serię instrukcji w zakresie transakcji i zapewnia obsługę złożonych transakcje, które obejmują wiele źródeł, takich jak dwóch różnych baz danych lub nawet heterogenicznych typów magazynów danych, takich jak bazy danych programu Microsoft SQL Server, bazy danych Oracle i usługi sieci Web. I Zapisz będę używać ADO.NET transakcji na potrzeby tego samouczka, zamiast `TransactionScope` klasy ADO.NET jest bardziej specyficzne dla transakcji bazy danych, a w wielu przypadkach jest znacznie mniej ilości zasobów. Ponadto, w niektórych scenariuszach `TransactionScope` klasy używa MSDTC Microsoft Distributed Transaction Coordinator (). Problemy konfiguracji, wdrażania i wydajności otaczającego MSDTC ułatwia tematu raczej specjalistyczne i zaawansowane i poza zakres tego samouczka.


Podczas pracy z dostawca SqlClient w ADO.NET, transakcje są inicjowane przez wywołanie [ `SqlConnection` klasy](https://msdn.microsoft.com/library/system.data.sqlclient.sqlconnection.aspx) s [ `BeginTransaction` metody](https://msdn.microsoft.com/library/system.data.sqlclient.sqlconnection.begintransaction.aspx), która zwraca [ `SqlTransaction` obiektu](https://msdn.microsoft.com/library/system.data.sqlclient.sqltransaction.aspx). Instrukcje modyfikacji danych umieszczonych w skład transakcji w obrębie `try...catch` bloku. Jeśli wystąpi błąd w instrukcji w `try` blokowanie wykonywania transferu do `catch` bloku, w którym transakcji można wycofać za pośrednictwem `SqlTransaction` obiektu s [ `Rollback` — metoda](https://msdn.microsoft.com/library/system.data.sqlclient.sqltransaction.rollback.aspx). Jeśli wszystkie instrukcje zakończy się pomyślnie, wywołanie `SqlTransaction` obiektu s [ `Commit` metody](https://msdn.microsoft.com/library/system.data.sqlclient.sqltransaction.commit.aspx) na końcu `try` bloku zatwierdza transakcji. Poniższy fragment kodu przedstawia tego wzorca. Zobacz [zachowaniu spójności bazy danych z transakcjami](http://aspnet.4guysfromrolla.com/articles/072705-1.aspx) dodatkowe składni i przykłady przy użyciu transakcji z ADO.NET.


[!code-vb[Main](wrapping-database-modifications-within-a-transaction-vb/samples/sample1.vb)]

Domyślnie TableAdapters w zestawie danych wpisana nie używaj transakcji. Aby zapewnić obsługę dla transakcji należy rozszerzyć klasy TableAdapter uwzględnienie dodatkowych metod korzystających ze wzorca powyżej, aby wykonać szereg instrukcji modyfikacji danych w zakresie transakcji. W kroku 2 będzie przedstawiono sposób użycia klasy częściowe do dodania tych metod.

## <a name="step-1-creating-the-working-with-batched-data-web-pages"></a>Krok 1: Tworzenie pracy ze stronami sieci Web danych wsadów

Zanim możemy rozpocząć eksplorowanie jak rozszerzyć warstwę DAL do obsługi transakcji bazy danych, chętnie s najpierw Poświęć chwilę, aby utworzyć stron sieci web ASP.NET, które są wymagane dla tego samouczka i trzech, które należy wykonać. Rozpocznij od dodania nowy folder o nazwie `BatchData` , a następnie dodaj następujące strony ASP.NET kojarzenie każdej strony zawierającej `Site.master` strony wzorcowej.

- `Default.aspx`
- `Transactions.aspx`
- `BatchUpdate.aspx`
- `BatchDelete.aspx`
- `BatchInsert.aspx`


![Dodawanie stron ASP.NET samouczki dotyczące SqlDataSource](wrapping-database-modifications-within-a-transaction-vb/_static/image1.gif)

**Rysunek 1**: Dodawanie stron ASP.NET samouczki dotyczące SqlDataSource


Podobnie jak w przypadku innych folderach `Default.aspx` użyje `SectionLevelTutorialListing.ascx` kontrolki użytkownika, aby wyświetlić listę samouczków w obrębie sekcji. W związku z tym Dodaj formant użytkownika `Default.aspx` przeciągając je z Eksploratora rozwiązań na stronę s widoku Projekt.


[![Dodaj kontrolkę użytkownika SectionLevelTutorialListing.ascx do Default.aspx](wrapping-database-modifications-within-a-transaction-vb/_static/image2.gif)](wrapping-database-modifications-within-a-transaction-vb/_static/image1.png)

**Rysunek 2**: Dodaj `SectionLevelTutorialListing.ascx` formantu użytkownika `Default.aspx` ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](wrapping-database-modifications-within-a-transaction-vb/_static/image2.png))


Na koniec Dodaj następujące cztery strony jako wpisów `Web.sitemap` pliku. W szczególności, Dodaj następujący kod po Dostosowywanie mapy witryny `<siteMapNode>`:


[!code-xml[Main](wrapping-database-modifications-within-a-transaction-vb/samples/sample2.xml)]

Po zaktualizowaniu `Web.sitemap`, Poświęć chwilę, aby wyświetlić witrynę sieci Web samouczki, za pośrednictwem przeglądarki. Menu po lewej stronie zawiera teraz elementów pracy z samouczki wsadów danych.


![Mapy witryny zawiera teraz wpisy dotyczące pracy z samouczki wsadów danych](wrapping-database-modifications-within-a-transaction-vb/_static/image3.gif)

**Rysunek 3**: mapy witryny zawiera teraz wpisy dotyczące pracy z samouczki wsadów danych


## <a name="step-2-updating-the-data-access-layer-to-support-database-transactions"></a>Krok 2: Aktualizowanie Warstwa dostępu do danych do obsługi transakcji bazy danych

Jak wspomniano w pierwszym samouczku [tworzenie Warstwa dostępu do danych](../introduction/creating-a-data-access-layer-vb.md), zestaw danych wpisane w naszym DAL składa się z DataTables i TableAdapters. DataTables przechowywania danych, podczas gdy TableAdapters zapewniać funkcje, które można odczytać danych z bazy danych do DataTables, aby zaktualizować bazę danych ze zmianami wprowadzonymi do DataTables i tak dalej. Odwołaj się, że TableAdapters zapewnia dwa wzorce dla aktualizacji danych, które I określone jako aktualizacja partii i bazy danych bezpośrednio. Za pomocą wzorca aktualizacji wsadowych TableAdapter jest przekazywany zestawu danych, DataTable lub kolekcji wierszy danych. Te dane wyliczeniu i dla każdego wstawiania, zmodyfikować lub usunąć wiersza, `InsertCommand`, `UpdateCommand`, lub `DeleteCommand` jest wykonywana. Za pomocą wzorca bazy danych bezpośrednio TableAdapter zamiast tego są przekazywane wartości kolumn, które są niezbędne do Wstawianie, aktualizowanie lub usuwanie pojedynczego rekordu. Metoda bezpośrednia DB wzorzec następnie wykorzystuje te wartości przekazywane w wykonać odpowiednie `InsertCommand`, `UpdateCommand`, lub `DeleteCommand` instrukcji.

Niezależnie od wzorzec aktualizacji używany metody automatycznego generowania TableAdapters nie używaj transakcji. Domyślnie każdego insert, update lub delete wykonywanych przez TableAdapter jest traktowana jako odrębny jednej operacji. Na przykład załóżmy, że wzorzec bezpośrednio bazy danych jest używane przez kod w logiki warstwy Biznesowej można umieścić dziesięć rekordów w bazie danych. Ten kod może wywołać metodę TableAdapter `Insert` metody dziesięć razy. Jeśli pierwsze pięć wstawia powiedzie się, ale szóstego co spowodowało wyjątek, pierwsze pięć wstawianych rekordów pozostanie w bazie danych. Podobnie, jeśli wzorzec aktualizacji wsadowych jest używana do wykonywania operacji wstawiania, aktualizacji i usunięć do wstawiony, modyfikować i usunąć wierszy w DataTable, jeśli pierwsze kilka zmian zakończyło się pomyślnie, ale późniejszą napotkał błąd, te modyfikacje wcześniejsze który Ukończono pozostanie w bazie danych.

W niektórych scenariuszach chcemy zapewnić niepodzielność na wielu zmian. Należy ręcznie rozbudowujemy TableAdapter przez dodanie nowych metod, które są wykonywane w tym celu `InsertCommand`, `UpdateCommand`, i `DeleteCommand` s w obszarze parasola transakcji. W [tworzenie Warstwa dostępu do danych](../introduction/creating-a-data-access-layer-vb.md) analizujemy przy użyciu [klasy częściowe](http://en.wikipedia.org/wiki/Partial_type) mogą rozszerzyć funkcjonalność DataTables wewnątrz elementu DataSet wpisany. Ta technika może również z TableAdapters.

Zestaw danych typu `Northwind.xsd` znajduje się w `App_Code` folderu s `DAL` podfolderu. Utwórz podfolder w `DAL` folder o nazwie `TransactionSupport` i Dodaj plik klasy o nazwie `ProductsTableAdapter.TransactionSupport.vb` (patrz rysunek 4). Ten plik będzie zawierać częściowa implementacja `ProductsTableAdapter` zawierającą metod wykonywania modyfikacji danych przy użyciu transakcji.


![Dodaj Folder o nazwie TransactionSupport i plik klasy o nazwie ProductsTableAdapter.TransactionSupport.vb](wrapping-database-modifications-within-a-transaction-vb/_static/image4.gif)

**Rysunek 4**: Dodaj Folder o nazwie `TransactionSupport` i plik klasy o nazwie `ProductsTableAdapter.TransactionSupport.vb`


Wprowadź następujący kod do `ProductsTableAdapter.TransactionSupport.vb` pliku:


[!code-vb[Main](wrapping-database-modifications-within-a-transaction-vb/samples/sample3.vb)]

`Partial` — Słowo kluczowe w deklaracji klasy, w tym miejscu wskazuje kompilatora, które są dodawani członkowie mają zostać dodane do `ProductsTableAdapter` klasy w `NorthwindTableAdapters` przestrzeni nazw. Uwaga `Imports System.Data.SqlClient` instrukcji w górnej części pliku. Ponieważ TableAdapter został skonfigurowany do używania dostawcy SqlClient, wewnętrznie używa [ `SqlDataAdapter` ](https://msdn.microsoft.com/library/system.data.sqlclient.sqldataadapter.aspx) obiekt do wysyłania polecenia do bazy danych. W związku z tym, należy użyć `SqlTransaction` klasy, aby rozpocząć transakcji, a następnie przekazać go lub go wycofać. Jeśli korzystasz z magazynem danych innych niż Microsoft SQL Server, należy użyć odpowiedniego dostawcę.

Te metody Podaj bloków konstrukcyjnych potrzebne do uruchomienia, wycofywania i przekazać transakcji. Są one oznaczone `Public`, włączanie ich do użycia z poziomu `ProductsTableAdapter`z innej klasy w warstwy DAL i z innej warstwy architektury, takich jak logiki warstwy Biznesowej. `BeginTransaction` Otwiera wewnętrzny s TableAdapter `SqlConnection` (w razie potrzeby), rozpoczyna się transakcji i przypisuje go do `Transaction` właściwości oraz dołącza transakcji do wewnętrznej `SqlDataAdapter` s `SqlCommand` obiektów. `CommitTransaction` i `RollbackTransaction` wywołać `Transaction` obiektu s `Commit` i `Rollback` metod, odpowiednio, przed jego zamknięciem wewnętrznej `Connection` obiektu.

## <a name="step-3-adding-methods-to-update-and-delete-data-under-the-umbrella-of-a-transaction"></a>Krok 3: Dodawanie metody do aktualizacji i usuwania danych w obszarze parasola transakcji

Z tych metod pełną możemy re gotowe do dodawania metod `ProductsDataTable` lub logiki warstwy Biznesowej, które wykonać kilka poleceń w obszarze parasola transakcji. Następująca metoda korzysta ze wzorca wsadowe aktualizacji można zaktualizować `ProductsDataTable` wystąpienia przy użyciu transakcji. Rozpoczyna transakcją wywołując `BeginTransaction` metody, a następnie używa `Try...Catch` bloku do wystawiania oświadczeń modyfikacji danych. Jeśli wywołanie `Adapter` obiektu s `Update` metoda powoduje wyjątek, wykonanie zostaną przeniesione do `catch` blok, gdy transakcja zostanie wycofana i ponownie zgłoszony wyjątek. Odwołania, który `Update` metoda implementuje wzorzec aktualizacji wsadowych wyliczając wiersze podane `ProductsDataTable` do wykonania niezbędnych `InsertCommand`, `UpdateCommand`, i `DeleteCommand` s. Jeśli jeden z tych poleceń powoduje błąd, transakcja zostanie wycofana, cofanie poprzedniej modyfikacje okres istnienia s transakcji. Należy `Update` instrukcji ukończone bez błędów, transakcja zostaje zatwierdzona w całości.


[!code-vb[Main](wrapping-database-modifications-within-a-transaction-vb/samples/sample4.vb)]

Dodaj `UpdateWithTransaction` metodę `ProductsTableAdapter` klasy do klasy częściowej w `ProductsTableAdapter.TransactionSupport.vb`. Alternatywnie tej metody można dodać do warstwy logiki biznesowej s `ProductsBLL` klasy z kilku drobne zmiany syntaktycznych. To znaczy, słowo kluczowe `Me` w `Me.BeginTransaction()`, `Me.CommitTransaction()`, i `Me.RollbackTransaction()` musi zostać zastąpiony `Adapter` (odwołania, który `Adapter` to nazwa właściwości w `ProductsBLL` typu `ProductsTableAdapter`).

`UpdateWithTransaction` Metoda korzysta ze wzorca wsadowe aktualizacji, ale szereg DB bezpośrednie wywołania można również w zakresie transakcji, jak przedstawiono na poniższym metody. `DeleteProductsWithTransaction` Metoda przyjmuje jako dane wejściowe `List(Of T)` typu `Integer`, które są `ProductID` s do usunięcia. Metoda inicjuje transakcję za pośrednictwem wywołania `BeginTransaction` , a następnie w `Try` bloków, iteracji podanej listy wywoływania wzorca bazy danych bezpośrednio `Delete` metody dla każdego `ProductID` wartość. Jeśli dowolny z wywołań `Delete` kończy się niepowodzeniem, sterowanie jest przekazywane na `Catch` blok, gdy transakcja zostanie wycofana i ponownie zgłoszony wyjątek. Jeśli wszystkie wywołania do `Delete` powiedzie się, a następnie transakcja została przekazana. Dodaj tę metodę w celu `ProductsBLL` klasy.


[!code-vb[Main](wrapping-database-modifications-within-a-transaction-vb/samples/sample5.vb)]

## <a name="applying-transactions-across-multiple-tableadapters"></a>Stosowanie transakcje w wielu TableAdapters

Związane z transakcji kodu, w tym samouczku umożliwia użycie wielu instrukcji przed `ProductsTableAdapter` powinien być traktowany jako niepodzielną operację. Ale co zrobić, jeśli wiele zmian do innych tabelach bazy danych muszą być wykonywane automatycznie? Na przykład podczas usuwania kategorii, firma Microsoft może najpierw chcesz ponownie przypisać jej bieżącego produktów do niektórych innych kategorii. Następujące dwa kroki ponowne przypisywanie produktów i usuwanie kategorii powinien być wykonywany jako operacją niepodzielną. Ale `ProductsTableAdapter` obejmuje tylko metody modyfikowania `Products` tabeli i `CategoriesTableAdapter` obejmuje tylko metody modyfikowania `Categories` tabeli. W jaki sposób transakcji obejmować zarówno TableAdapters

Jedną z opcji jest dodanie metody, aby `CategoriesTableAdapter` o nazwie `DeleteCategoryAndReassignProducts(categoryIDtoDelete, reassignToCategoryID)` i wywołanie procedury przechowywanej, która zarówno następuje zmiana przypisania produktów i usuwa kategorii w zakresie transakcji zdefiniowanych w ramach procedury składowanej tej metody. Wyjaśniono, jak rozpocząć, zatwierdzenia i wycofywania transakcji w procedur składowanych w przyszłości samouczka.

Innym rozwiązaniem jest utworzenie klasę pomocy w DAL, który zawiera `DeleteCategoryAndReassignProducts(categoryIDtoDelete, reassignToCategoryID)` metody. Ta metoda może utworzyć wystąpienia `CategoriesTableAdapter` i `ProductsTableAdapter` , a następnie ustaw te dwie TableAdapters `Connection` właściwości do tej samej `SqlConnection` wystąpienia. At tego punktu, albo jeden z dwóch TableAdapters może inicjować transakcji z wywołaniem do `BeginTransaction`. Czy można wywołać metody TableAdapters ponowne przypisywanie produktów i usuwanie kategorii w `Try...Catch` bloku z transakcją zatwierdzona lub wycofana ponownie zgodnie z potrzebami.

## <a name="step-4-adding-theupdatewithtransactionmethod-to-the-business-logic-layer"></a>Krok 4: Dodawanie`UpdateWithTransaction`metody do warstwy logiki biznesowej

W kroku 3 dodaliśmy `UpdateWithTransaction` metodę `ProductsTableAdapter` w warstwy DAL. Należy dodać odpowiedniej metody do logiki warstwy Biznesowej. Gdy warstwę prezentacji można wywołać bezpośrednio do warstwy DAL do wywołania `UpdateWithTransaction` metody te samouczki ma strived do definiowania warstwowego architekturę, która powoduje DAL z warstwy prezentacji. W związku z tym go behooves firmie Microsoft w celu kontynuowania tej metody.

Otwórz `ProductsBLL` klasy plików i Dodaj metodę o nazwie `UpdateWithTransaction` tego wywołania po prostu do odpowiedniej metody DAL. Teraz należy dwóch nowych metod w `ProductsBLL`: `UpdateWithTransaction`, który właśnie został dodany, i `DeleteProductsWithTransaction`, który został dodany w kroku 3.


[!code-vb[Main](wrapping-database-modifications-within-a-transaction-vb/samples/sample6.vb)]

> [!NOTE]
> Nie dołączaj tych metod `DataObjectMethodAttribute` atrybutu przypisane do większości metod w `ProductsBLL` klasy, ponieważ firma Microsoft będzie wywołania tych metod bezpośrednio z klasy związane z kodem stron ASP.NET. Odwołania, który `DataObjectMethodAttribute` służy do Flaga, jakie metody powinny być wyświetlane w ObjectDataSource s Konfigurowanie źródła danych kreatora i jakie karcie (SELECT, UPDATE, INSERT lub DELETE). Ponieważ w widoku GridView nie ma żadnych wbudowaną obsługę partii, edytowania lub usuwania, firma Microsoft będzie konieczne wywołania tych metod programowo, zamiast używać niekorzystające z kodu deklaratywne podejście.


## <a name="step-5-atomically-updating-database-data-from-the-presentation-layer"></a>Krok 5: Atomowo aktualizowania bazy danych z warstwy prezentacji

Aby zilustrować wpływ transakcji podczas aktualizowania partii rekordów, umożliwiają s Tworzenie interfejsu użytkownika, który zawiera listę wszystkich produktów w widoku GridView oraz Web przycisk kontrolować, po kliknięciu następuje zmiana przypisania produktów `CategoryID` wartości. W szczególności, ponowne przypisanie kategorii będzie postęp tak, aby pierwszy kilku produktów są przypisane prawidłowe `CategoryID` wartości, podczas gdy inne celowo są przypisane do nieistniejącej `CategoryID` wartość. Jeśli firma Microsoft próbę aktualizacji bazy danych za pomocą produktu do których `CategoryID` jest niezgodna z istniejącą kategorię s `CategoryID`, nastąpi naruszenie ograniczenia klucza obcego i zostanie zgłoszony wyjątek. Co zajmiemy się tym, w tym przykładzie jest fakt, że przy użyciu transakcji wyjątek zgłoszony z naruszenie ograniczenia klucza obcego spowoduje poprzedniej prawidłową `CategoryID` zmiany wycofana. Jeśli nie przy użyciu transakcji, jednak modyfikacje do początkowej kategorii pozostaną.

Uruchamianie przez otwarcie `Transactions.aspx` strony `BatchData` folder i przeciągnij element GridView z przybornika do projektanta. Ustaw jego `ID` do `Products` i z jego tagów inteligentnych powiązać go z nowego elementu ObjectDataSource o nazwie `ProductsDataSource`. Skonfiguruj ObjectDataSource jego dane z `ProductsBLL` klasy s `GetProducts` metody. To jest tylko do odczytu widoku GridView, tak zestawu list rozwijanych w UPDATE, INSERT i usuwanie kart na (Brak) i kliknij przycisk Zakończ.


[![Skonfiguruj element ObjectDataSource przy użyciu metody GetProducts klasy ProductsBLL s](wrapping-database-modifications-within-a-transaction-vb/_static/image5.gif)](wrapping-database-modifications-within-a-transaction-vb/_static/image3.png)

**Rysunek 5**: Konfigurowanie ObjectDataSource użyć `ProductsBLL` klasy s `GetProducts` — metoda ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](wrapping-database-modifications-within-a-transaction-vb/_static/image4.png))


[![Ustawianie list rozwijanych w UPDATE, INSERT i usuwanie kart na (Brak)](wrapping-database-modifications-within-a-transaction-vb/_static/image6.gif)](wrapping-database-modifications-within-a-transaction-vb/_static/image5.png)

**Rysunek 6**: wartość listy rozwijane w AKTUALIZOWANIA, WSTAWIANIA i usuwanie kart (None) ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](wrapping-database-modifications-within-a-transaction-vb/_static/image6.png))


Po zakończeniu pracy Kreatora konfigurowania źródła danych programu Visual Studio utworzy BoundFields i CheckBoxField dla pól danych produktu. Usuń wszystkie te pola z wyjątkiem `ProductID`, `ProductName`, `CategoryID`, i `CategoryName` i zmienić nazwę `ProductName` i `CategoryName` BoundFields `HeaderText` właściwości do produktu i kategorii, odpowiednio. Za pomocą tagu inteligentnego zaznacz opcję Włącz stronicowanie. Po wprowadzeniu tych zmian, znaczników deklaratywne s GridView i ObjectDataSource powinna wyglądać następująco:


[!code-aspx[Main](wrapping-database-modifications-within-a-transaction-vb/samples/sample7.aspx)]

Następnie dodaj trzy formantów sieci Web przycisk powyżej widoku GridView. Wartość s właściwość tekst przycisku pierwszej Odśwież siatki, drugi s, aby zmodyfikować kategorii (z transakcji), a trzeci jeden s do modyfikowania kategorii (bez transakcji).


[!code-aspx[Main](wrapping-database-modifications-within-a-transaction-vb/samples/sample8.aspx)]

W tym momencie widoku Projekt w programie Visual Studio powinien wyglądać podobnie do ekranu zrzut pokazano na rysunku 7.


[![Strona zawiera element GridView i trzy formantów sieci Web przycisku](wrapping-database-modifications-within-a-transaction-vb/_static/image7.gif)](wrapping-database-modifications-within-a-transaction-vb/_static/image7.png)

**Rysunek 7**: strona zawiera GridView i trzy formantów sieci Web przycisk ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](wrapping-database-modifications-within-a-transaction-vb/_static/image8.png))


Tworzenie obsługi zdarzeń dla każdego z trzech przycisk s `Click` zdarzenia i użyj następującego kodu:


[!code-vb[Main](wrapping-database-modifications-within-a-transaction-vb/samples/sample9.vb)]

Odświeżanie przycisk s `Click` obsługi zdarzeń po prostu rebinds dane do widoku GridView wywołując `Products` GridView s `DataBind` — metoda.

Druga procedura obsługi zdarzeń następuje zmiana przypisania produktów `CategoryID` s i używa nowej metody transakcji z logiki warstwy Biznesowej do wykonania na bazie aktualizacji w obszarze parasola transakcji. Należy pamiętać, że każdy z produktów s `CategoryID` arbitralnie ustawioną taką samą wartość jak jego `ProductID`. To będzie działać prawidłowo w pierwszym kilku produktów, ponieważ zawierają one te produkty `ProductID` wartości, które wystąpić do mapowania na nieprawidłowy `CategoryID` s. Ale raz `ProductID` s rozpoczęcia pobierania zbyt duży, to przypadkowe nakładanie się `ProductID` s i `CategoryID` s nie ma już zastosowania.

Trzeci `Click` obsługi zdarzeń aktualizacji produktów `CategoryID` s w taki sam sposób, ale wysyła aktualizacji do bazy danych przy użyciu `ProductsTableAdapter` domyślne s `Update` metody. To `Update` metoda nie jest zawijany serii poleceń w ramach transakcji, więc tych zmian przed pierwszym błędzie naruszenie napotkano ograniczenie klucza obcego zostanie utrzymana.

Aby demonstrują takie zachowanie, odwiedź stronę tej strony za pośrednictwem przeglądarki. Początkowo powinien zostać wyświetlony na pierwszej stronie danych jak pokazano na rysunku 8. Następnie kliknij przycisk Modyfikuj kategorie (z transakcji). Będzie to powodować odświeżenie strony i próba aktualizacji wszystkich produktów `CategoryID` wartości, ale spowoduje naruszenie ograniczenia klucza obcego (patrz rysunek 9).


[![Te produkty zostaną wyświetlone w stronicowalnej widoku GridView](wrapping-database-modifications-within-a-transaction-vb/_static/image8.gif)](wrapping-database-modifications-within-a-transaction-vb/_static/image9.png)

**Rysunek 8**: produkty są wyświetlane w widoku GridView stronicowalnej ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](wrapping-database-modifications-within-a-transaction-vb/_static/image10.png))


[![Ponowne przypisywanie kategorii spowoduje naruszenie ograniczenia klucza obcego](wrapping-database-modifications-within-a-transaction-vb/_static/image9.gif)](wrapping-database-modifications-within-a-transaction-vb/_static/image11.png)

**Rysunek 9**: ponowne przypisywanie kategorii spowoduje naruszenie ograniczenia klucza obcego ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](wrapping-database-modifications-within-a-transaction-vb/_static/image12.png))


Teraz kliknij przycisk Wstecz Twojej przeglądarki s, a następnie kliknij przycisk Odśwież siatki. Po odświeżeniu danych powinny zostać wyświetlone dokładne dane wyjściowe tego samego jak pokazano na rysunku 8. Oznacza to, nawet jeśli niektóre produkty `CategoryID` s zostały zmienione na prawne wartości i zaktualizowane w bazie danych, ich zostały wycofane po wystąpiło naruszenie ograniczenia klucza obcego.

Spróbuj teraz kliknięcie przycisku Modyfikuj kategorii (bez transakcji). Spowoduje to ten sam błąd naruszenie ograniczenia klucza obcego (patrz rysunek 9), ale tym razem tych produktów których `CategoryID` wartości zostały zmienione na prawnych i wartość będą nie można wycofać. Kliknij przycisk z przycisku Wstecz w przeglądarce s, a następnie przycisk Odśwież siatki. Jak pokazano na rysunku nr 10, `CategoryID` ponownie przypisywane s pierwszych osiem produktów. Na przykład na rysunku 8 zmian miał `CategoryID` 1, ale w rysunek 10 it s przypisane do 2.


[![Niektóre produkty CategoryID wartości aktualizowane podczas innych zostały nie zostały](wrapping-database-modifications-within-a-transaction-vb/_static/image10.gif)](wrapping-database-modifications-within-a-transaction-vb/_static/image13.png)

**Na rysunku nr 10**: niektóre produkty `CategoryID` wartości nie jest aktualizowane podczas innych zostały ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](wrapping-database-modifications-within-a-transaction-vb/_static/image14.png))


## <a name="summary"></a>Podsumowanie

Domyślnie metody TableAdapter s nie jest zawijany instrukcje wykonywane bazy danych w zakresie transakcji, ale z małego wysiłku dodamy metody, które spowoduje utworzenie, przekazywania i wycofywania transakcji. W tym samouczku utworzyliśmy tych trzech metod w `ProductsTableAdapter` klasy: `BeginTransaction`, `CommitTransaction`, i `RollbackTransaction`. Widzieliśmy sposób użycia tych metod, wraz z `Try...Catch` bloku dokonanie atomic szereg instrukcji modyfikacji danych. W szczególności, utworzyliśmy `UpdateWithTransaction` metody w `ProductsTableAdapter`, który korzysta ze wzorca aktualizacji usługi partia zadań do wykonania niezbędnych zmian w wierszach podany `ProductsDataTable`. Dodaliśmy również `DeleteProductsWithTransaction` metodę `ProductsBLL` klasy w logiki warstwy Biznesowej, które akceptuje `List` z `ProductID` wartości jako dane wejściowe i wywołuje metodę wzorca bazy danych bezpośrednio `Delete` dla każdego `ProductID`. Obie metody start tworząc transakcji, a następnie wykonywania instrukcji modyfikacji danych w ramach `Try...Catch` bloku. Jeśli wystąpi wyjątek, transakcja zostanie wycofana, w przeciwnym razie zatwierdzone.

Krok 5 ilustruje efekt aktualizacji partii transakcji, jak i aktualizacje wsadowe, które nie zwrócił używać transakcji. W trzech kolejnych samouczkach nasz zależą od foundation, w tym samouczku i Tworzenie interfejsów użytkownika dla przeprowadzania aktualizacji wsadowych, usuwa i wstawia.

Programowanie przyjemność!

## <a name="further-reading"></a>Dalsze informacje

Więcej informacji dotyczących tematów omówionych w tym samouczku można znaleźć w następujących zasobach:

- [Zachowaniu spójności bazy danych z transakcji](http://aspnet.4guysfromrolla.com/articles/072705-1.aspx)
- [Zarządzanie transakcji w programie SQL Server procedury składowane](http://www.4guysfromrolla.com/webtech/080305-1.shtml)
- [Transakcje prosty: `System.Transactions`](https://blogs.msdn.com/florinlazar/archive/2004/07/23/192239.aspx)
- [Element TransactionScope i obiektów DataAdapter](http://andyclymer.blogspot.com/2007/01/transactionscope-and-dataadapters.html)
- [Używanie transakcji bazy danych Oracle w .NET](http://www.oracle.com/technology/pub/articles/price_dbtrans_dotnet.html)

## <a name="about-the-author"></a>Informacje o autorze

[Scott Bento](http://www.4guysfromrolla.com/ScottMitchell.shtml), autora siedmiu książek ASP/ASP.NET i twórcę z [4GuysFromRolla.com](http://www.4guysfromrolla.com), pracuje z technologii Microsoft Web od 1998. Scott działa jako niezależnego konsultanta trainer i składnika zapisywania. Jest jego najnowszej książki [ *Sams nauczyć się ASP.NET 2.0 w ciągu 24 godzin*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Piotr można uzyskać pod adresem [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) lub za pośrednictwem jego blog, który znajduje się w temacie [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Specjalne podziękowania dla

Ten samouczek serii zostało sprawdzone przez wiele recenzentów przydatne. Prowadzić osób dokonujących przeglądu, w tym samouczku zostały Dave Gardner, Hilton Giesenow i Teresa Murphy. Zainteresowani recenzowania Moje nadchodzących artykuły MSDN? Jeśli tak, Porzuć mnie linii w [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Poprzednie](batch-inserting-cs.md)
> [dalej](batch-updating-vb.md)
