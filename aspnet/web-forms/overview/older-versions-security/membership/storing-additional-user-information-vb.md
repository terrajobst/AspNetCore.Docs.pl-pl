---
uid: web-forms/overview/older-versions-security/membership/storing-additional-user-information-vb
title: Przechowywanie dodatkowe informacje dotyczące użytkownika (VB) | Dokumentacja firmy Microsoft
author: rick-anderson
description: W tym samouczku odpowiemy na to pytanie przez tworzenie aplikacji bardzo proste księgi gości. W ten sposób zajmiemy różnych opcji dla modeli...
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/18/2008
ms.topic: article
ms.assetid: ee4b924e-8002-4dc3-819f-695fca1ff867
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-security/membership/storing-additional-user-information-vb
msc.type: authoredcontent
ms.openlocfilehash: 9a8673e764ae94b12fbc01f81ef12ea4c133b7d5
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/06/2018
---
<a name="storing-additional-user-information-vb"></a>Przechowywanie dodatkowe informacje dotyczące użytkownika (VB)
====================
przez [Bento Scott](https://twitter.com/ScottOnWriting)

[Pobierz kod](http://download.microsoft.com/download/3/f/5/3f5a8605-c526-4b34-b3fd-a34167117633/ASPNET_Security_Tutorial_08_VB.zip) lub [pobierania plików PDF](http://download.microsoft.com/download/3/f/5/3f5a8605-c526-4b34-b3fd-a34167117633/aspnet_tutorial08_ExtraUserInfo_vb.pdf)

> W tym samouczku odpowiemy na to pytanie przez tworzenie aplikacji bardzo proste księgi gości. W ten sposób firma Microsoft będzie przyjrzeć się różne opcje modelowania informacje o użytkowniku w bazie danych, a następnie zobacz jak skojarzyć te dane z kontami użytkownika utworzone przez platformę członkostwa.


## <a name="introduction"></a>Wprowadzenie

ŚRODOWISKO ASP. W NET framework członkostwa oferuje elastyczne interfejsu do zarządzania użytkownikami. Interfejs API członkostwa zawiera metody sprawdzanie poprawności poświadczeń, trwa pobieranie informacji na temat aktualnie zalogowanego użytkownika, tworzenie nowego konta użytkownika i usunięcie konta użytkownika, między innymi. Każde konto użytkownika w ramach członkostwa zawiera tylko właściwości potrzebne sprawdzanie poprawności poświadczeń i wykonywanie zadań związanych z kontem użytkownika podstawowych. To jest dowodem metody i właściwości [ `MembershipUser` klasy](https://msdn.microsoft.com/library/system.web.security.membershipuser.aspx), które modele konta użytkownika w ramach członkostwa. Ta klasa ma właściwości, takie jak [ `UserName` ](https://msdn.microsoft.com/library/system.web.security.membershipuser.username.aspx), [ `Email` ](https://msdn.microsoft.com/library/system.web.security.membershipuser.email.aspx), i [ `IsLockedOut` ](https://msdn.microsoft.com/library/system.web.security.membershipuser.islockedout.aspx), i metod, takich jak [ `GetPassword` ](https://msdn.microsoft.com/library/system.web.security.membershipuser.getpassword.aspx) i [ `UnlockUser` ](https://msdn.microsoft.com/library/system.web.security.membershipuser.unlockuser.aspx).

Często należy zachować dodatkowe informacje dotyczące użytkownika nie jest uwzględniony w ramach członkostwa aplikacji. Na przykład detalicznej online może być konieczne let każdego użytkownika, przechowywać swoje adresy wysyłki i rozliczeń, informacje dotyczące płatności, a preferencje dostarczania i numer telefonu. Ponadto każdy kolejności w systemie jest skojarzony z określonego konta użytkownika.

`MembershipUser` Klasa nie ma właściwości, takie jak `PhoneNumber` lub `DeliveryPreferences` lub `PastOrders`. W jaki sposób firma Microsoft śledzenia informacji użytkownika wymagane przez aplikację i ich integracji z architekturą członkostwa? W tym samouczku odpowiemy na to pytanie przez tworzenie aplikacji bardzo proste księgi gości. W ten sposób firma Microsoft będzie przyjrzeć się różne opcje modelowania informacje o użytkowniku w bazie danych, a następnie zobacz jak skojarzyć te dane z kontami użytkownika utworzone przez platformę członkostwa. Dzieła!

## <a name="step-1-creating-the-guestbook-applications-data-model"></a>Krok 1: Tworzenie modelu danych aplikacji księgi gości

Istnieją różne techniki, które można zastosować do przechwycenia informacji użytkownika w bazie danych i powiązać ją z kontami użytkownika utworzone przez platformę członkostwa. Aby zilustrować te techniki, potrzebujemy rozszerzyć aplikacji sieci web samouczka, dzięki czemu znajdują się jakieś dane dotyczące użytkowników. (Aktualnie modelu danych aplikacji zawiera tylko aplikację usługi tabel wymaganych przez `SqlMembershipProvider`.)

Umożliwia tworzenie aplikacji bardzo proste księgi gości, gdzie uwierzytelniony użytkownik może narazić komentarz. Oprócz przechowywania komentarzy księgi gości, umożliwia pozwolić każdemu użytkownikowi na przechowywanie jego miejscowość macierzystego, strony głównej i podpis. Jeśli zostanie podana, miasta macierzysty użytkownika, strony głównej i podpis będą wyświetlane na każdy komunikat, który opuścił on w księdze gości.

### <a name="adding-theguestbookcommentstable"></a>Dodawanie`GuestbookComments`tabeli

Aby przechwycić komentarze księgi gości, należy utworzyć tabeli bazy danych o nazwie `GuestbookComments` mający kolumny, tak jak `CommentId`, `Subject`, `Body`, i `CommentDate`. Możemy również musi mieć każdy rekord w `GuestbookComments` odwołania do tabeli użytkownika, który pozostanie komentarz.

Aby dodać tej tabeli do naszej bazie danych, przejdź do Eksploratora bazy danych w programie Visual Studio i przejdź do szczegółów `SecurityTutorials` bazy danych. Kliknij prawym przyciskiem folder Tabele i wybierz polecenie Dodaj nową tabelę. Spowoduje to wyświetlenie interfejs, który pozwala zdefiniować kolumny w nowej tabeli.


[![Dodaj nową tabelę w bazie danych SecurityTutorials](storing-additional-user-information-vb/_static/image2.png)](storing-additional-user-information-vb/_static/image1.png)

**Rysunek 1**: Dodaj nową tabelę do `SecurityTutorials` bazy danych ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](storing-additional-user-information-vb/_static/image3.png))


Następnie określ `GuestbookComments`dla kolumn. Rozpocznij od dodania kolumny o nazwie `CommentId` typu `uniqueidentifier`. Ta kolumna zostanie jednoznacznie zidentyfikować każdy komentarz w księdze gości, więc nie zezwalaj na `NULL` s i oznacz ją jako klucz podstawowy tabeli. Zamiast podawania wartości dla `CommentId` pole na każdym `INSERT`, firma Microsoft może oznaczać, że nowy `uniqueidentifier` wartości powinny być generowane automatycznie dla tego pola na `INSERT` się przez ustawienie wartości domyślnej kolumny `NEWID()`. Po dodaniu tego pierwsze pole, oznaczając je jako klucz podstawowy, a ustawienia wartość domyślną ekranie powinien wyglądać podobnie do ekranu zrzut pokazano na rysunku 2.


[![Dodaj kolumnę podstawowego o nazwie CommentId](storing-additional-user-information-vb/_static/image5.png)](storing-additional-user-information-vb/_static/image4.png)

**Rysunek 2**: Dodawanie podstawowego o nazwie kolumny `CommentId` ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](storing-additional-user-information-vb/_static/image6.png))


Następnie dodaj kolumnę o nazwie `Subject` typu `nvarchar(50)` i kolumna o nazwie `Body` typu `nvarchar(MAX)`, niezezwalające `NULL` s w kolumnach. Następujące, Dodaj kolumnę o nazwie `CommentDate` typu `datetime`. Nie zezwalaj na `NULL` s i zestaw `CommentDate` wartości domyślnej kolumny do `getdate()`.

Wszystkie opcje, które pozostaje jest dodanie kolumny, która kojarzy konto użytkownika z każdego komentarza księgi gości. Jedną z opcji byłoby dodać kolumnę o nazwie `UserName` typu `nvarchar(256)`. Jest to odpowiedni wybór w przypadku innych niż przy użyciu dostawcy członkostwa `SqlMembershipProvider`. Jednak przy użyciu `SqlMembershipProvider`, ponieważ nie jesteśmy w tym samouczku, `UserName` kolumny w `aspnet_Users` tabeli nie musi być unikatowy. `aspnet_Users` Jest klucz podstawowy tabeli `UserId` i jest typu `uniqueidentifier`. W związku z tym `GuestbookComments` tabela musi kolumny o nazwie `UserId` typu `uniqueidentifier` (Brak zezwolenia `NULL` wartości). Przejdź dalej i dodać tę kolumnę.

> [!NOTE]
> Jak wspomniano w [ *tworzenie schematu członkostwa w programie SQL Server* ](creating-the-membership-schema-in-sql-server-vb.md) samouczek, w ramach członkostwa zaprojektowana w celu umożliwienia wiele aplikacji sieci web z różnych kont użytkowników w identyczny Magazyn użytkowników. Jest to możliwe dzięki partycjonowanie konta użytkowników do różnych aplikacji. I gdy nazwy użytkownika jest musi być unikatowy w obrębie aplikacji, tej samej nazwy użytkownika mogą być używane w różnych aplikacji przy użyciu tego samego magazynu użytkownika. Brak złożonym `UNIQUE` ograniczeniem `aspnet_Users` tabeli na `UserName` i `ApplicationId` pól, ale nie jest elementem na tylko `UserName` pola. W związku z tym, istnieje możliwość aspnet\_tabeli użytkowników (co najmniej dwa) rekordy z takimi samymi `UserName` wartość. Jest jednak `UNIQUE` ograniczenie `aspnet_Users` tabeli `UserId` pola (ponieważ jest to klucz podstawowy). A `UNIQUE` ograniczenie jest ważne, ponieważ bez niego firma Microsoft nie może ustanowić ograniczenie klucza obcego między `GuestbookComments` i `aspnet_Users` tabel.


Po dodaniu `UserId` kolumny, Zapisz tabeli, klikając ikonę Zapisz na pasku narzędzi. Nazwa nowej tabeli `GuestbookComments`.

Mamy jednego ostatniego problemu, aby zająć się z `GuestbookComments` tabeli: należy utworzyć [ograniczenie klucza obcego](https://msdn.microsoft.com/library/ms175464.aspx) między `GuestbookComments.UserId` kolumny i `aspnet_Users.UserId` kolumny. Aby to osiągnąć, kliknij ikonę relacji w pasku narzędzi, aby uruchomić okno dialogowe relacje klucza obcego. (Można również będzie można uruchomić tego okna dialogowego, przechodząc do menu projektanta tabel i wybierając relacje.)

Kliknij przycisk Dodaj w lewym dolnym rogu okna dialogowego relacje klucza obcego. Spowoduje to dodanie nowego ograniczenie klucza obcego, mimo że nadal trzeba zdefiniować tabel uczestniczących w relacji.


[![Użyj okna dialogowego relacje klucza obcego do zarządzania ograniczeń klucza obcego w tabeli](storing-additional-user-information-vb/_static/image8.png)](storing-additional-user-information-vb/_static/image7.png)

**Rysunek 3**: umożliwia zarządzanie ograniczeń klucza obcego w tabeli w oknie dialogowym relacji klucza obcego ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](storing-additional-user-information-vb/_static/image9.png))


Następnie kliknij ikonę elipsy w wierszu "I kolumny specyfikacje tabeli" po prawej stronie. Spowoduje to uruchomienie tabele i kolumny, okno dialogowe, w którym można określić tabeli klucza podstawowego i kolumny i kolumny klucza obcego z `GuestbookComments` tabeli. W szczególności należy wybrać `aspnet_Users` i `UserId` jako tabeli klucza podstawowego i kolumny, a `UserId` z `GuestbookComments` tabeli co kolumna klucza obcego (patrz rysunek 4). Po zdefiniowaniu podstawowe i obce klucza tabel i kolumn, kliknij przycisk OK, aby powrócić do okna dialogowego relacje klucza obcego.


[![Tworzą i obcego klucza ograniczenia między aspnet_Users GuesbookComments tabel](storing-additional-user-information-vb/_static/image11.png)](storing-additional-user-information-vb/_static/image10.png)

**Rysunek 4**: ustanowić obcego klucza ograniczenia między `aspnet_Users` i `GuesbookComments` tabele ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](storing-additional-user-information-vb/_static/image12.png))


W tym momencie została ustanowiona ograniczenie klucza obcego. Zapewnia obecności tego ograniczenia [relacyjna integralność](http://en.wikipedia.org/wiki/Referential_integrity) między dwiema tabelami, gwarantując, że nigdy nie będzie wpisu księgi gości, odwołujący się do konta użytkownika nie istnieje. Domyślnie ograniczenie klucza obcego uniemożliwi rekord nadrzędny ma zostać usunięty, jeśli istnieją odpowiadające im rekordów podrzędnych. Oznacza to, że użytkownik tworzy co najmniej jeden komentarz księgi gości, a następnie spróbuj możemy można usunąć tego konta użytkownika, Usuń zakończy się niepowodzeniem, chyba że najpierw zostaną usunięte jego komentarze księgi gości.

Ograniczenia klucza obcego można skonfigurować do automatycznego usuwania rekordów podrzędnych usunięcie rekordu nadrzędnego. Innymi słowy będziemy konfigurować to ograniczenie klucza obcego, dzięki czemu wpisów księgi gości są automatycznie usuwane, gdy konto użytkownika zostanie usunięta. Aby to zrobić, rozwiń sekcję "INSERT i UPDATE specyfikacji" i ustaw dla właściwości "Usuń regułę" Cascade.


[![Konfiguruj ograniczenie klucza obcego do kaskadowo](storing-additional-user-information-vb/_static/image14.png)](storing-additional-user-information-vb/_static/image13.png)

**Rysunek 5**: skonfigurować ograniczenie klucza obcego do kaskadowe ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](storing-additional-user-information-vb/_static/image15.png))


Aby zapisać ograniczenie klucza obcego, kliknij przycisk Zamknij, aby zakończyć poza relacje klucza obcego. Kliknij ikonę Zapisz w pasku narzędzi, aby zapisać tabeli i jego relacji.

### <a name="storing-the-users-home-town-homepage-and-signature"></a>Przechowywanie miejscowość głównej, strony głównej i podpis użytkownika

`GuestbookComments` Tabeli przedstawiono sposób przechowywania informacji, która udostępnia relacji jeden do wielu kont użytkowników. Ponieważ wszystkie konta użytkowników mogą mieć dowolną liczbę skojarzonych z nimi komentarzy, ta relacja jest modelowana, tworząc tabelę do przechowywania zestaw komentarz, który zawiera kolumnę, że linki kopię każdego komentarza dla określonego użytkownika. Korzystając z `SqlMembershipProvider`, tworząc kolumny o nazwie ustanowieniu tego połączenia będzie najlepiej `UserId` typu `uniqueidentifier` i ograniczenie klucza obcego między tej kolumny i `aspnet_Users.UserId`.

Teraz musisz skojarzyć trzy kolumny z każdego konta użytkownika do przechowywania podpisu, który pojawi się w jego komentarze księgi gości, strony głównej i miejscowość macierzystego użytkownika. Istnieją różne sposoby, w tym celu — liczba:

- <strong>Dodaj nowe kolumny</strong><strong>`aspnet_Users`</strong><strong>lub</strong><strong>`aspnet_Membership`</strong><strong>tabel.</strong> I czy zaleca takie podejście, ponieważ modyfikuje Schemat używany przez `SqlMembershipProvider`. Ta decyzja może wrócić do mogły spowodować szkód występujących. Na przykład, co w przypadku przyszłych wersjach programu ASP.NET używa innej `SqlMembershipProvider` schematu. Microsoft może obejmować narzędzia do migracji programu ASP.NET 2.0 `SqlMembershipProvider` danych do nowego schematu, ale jeśli zmodyfikowano ASP.NET 2.0 `SqlMembershipProvider` schematu, takie Konwersja może nie być możliwe.

- **Użyj ASP. NET w profilu platformy, definiowanie właściwości profilu miejscowość macierzystego, strony głównej i podpis.** Program ASP.NET zawiera framework profil, który służy do przechowywania dodatkowe dane specyficzne dla użytkownika. Jak framework członkostwa w ramach profilu została stworzona z modelu dostawcy. .NET Framework jest dostarczany z `SqlProfileProvider` czy magazyny profilu danych w bazie danych programu SQL Server. W rzeczywistości w naszej bazie danych ma już tabeli używanej przez `SqlProfileProvider` (`aspnet_Profile`), ponieważ zostało dodane, gdy dodaliśmy usług aplikacji ponownie [ *tworzenie schematu członkostwa w programie SQL Server* ](creating-the-membership-schema-in-sql-server-vb.md)samouczka.   
  Największą zaletą framework profilu jest umożliwia deweloperom do definiowania właściwości profilu `Web.config` — żaden kod nie trzeba napisać do serializowania danych profilu do i z odpowiedni magazyn danych. Krótko mówiąc jest bardzo łatwe do zdefiniowania zestawu właściwości profilu i pracować z nimi w kodzie. Jednak system profilu opuszcza się konieczne, jeśli chodzi o wersji, tak więc jeśli korzystasz z aplikacji, w którym oczekujesz nowe właściwości specyficzne dla użytkownika do dodania w późniejszym czasie lub istniejące ma zostać usunięta lub zmodyfikowana, a następnie framework profilu nie może być  najlepszym rozwiązaniem. Ponadto `SqlProfileProvider` przechowuje właściwości profilu w sposób wysokiej nieznormalizowany, uniemożliwiając dalej do wykonywania kwerend bezpośrednio do profilu danych (takich jak, ilu użytkowników mają macierzysty miejscowość z nowego Jorku).   
  Aby uzyskać więcej informacji w ramach profilu zapoznaj się w sekcji "Dalsze odczytów" na końcu tego samouczka.

- <strong>Dodaj te trzy kolumny do nowej tabeli w bazie danych i ustanowienia relacją między tej tabeli i</strong><strong>`aspnet_Users`</strong><strong>.</strong> Ta metoda wymaga nieco więcej pracy niż Framework profilu, ale oferuje maksymalną elastyczność w sposób właściwości użytkownika dodatkowe są modelowane w bazie danych. Jest to opcja, które będą używane w tym samouczku.

Utworzymy nową tabelę o nazwie `UserProfiles` zapisać miejscowość macierzystego, strony głównej i podpis dla każdego użytkownika. Kliknij prawym przyciskiem folder tabel w oknie Eksploratora bazy danych i wybrać opcję utworzenia nowej tabeli. Nazwa pierwszej kolumny `UserId` i Ustaw typ `uniqueidentifier`. Nie zezwalaj na `NULL` wartości i oznacz kolumnę jako klucz podstawowy. Następnie należy dodać kolumny o nazwie: `HomeTown` typu `nvarchar(50)`; `HomepageUrl` typu `nvarchar(100)`; i podpis typu `nvarchar(500)`. Każda z tych trzech kolumn może akceptować `NULL` wartość.


[![Utwórz tabelę UserProfiles](storing-additional-user-information-vb/_static/image17.png)](storing-additional-user-information-vb/_static/image16.png)

**Rysunek 6**: tworzenie `UserProfiles` tabeli ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](storing-additional-user-information-vb/_static/image18.png))


Zapisywanie tabeli i nadaj mu nazwę `UserProfiles`. Ponadto ustanowić ograniczenie klucza obcego między `UserProfiles` tabeli `UserId` pola i `aspnet_Users.UserId` pola. Jak robiliśmy z ograniczenie klucza obcego między `GuestbookComments` i `aspnet_Users` tabele, mieć to ograniczenie kaskadowo usuwa. Ponieważ `UserId` w `UserProfiles` jest serwerem podstawowym klucza, gwarantuje to, że będą istnieć co najwyżej jeden rekord w `UserProfiles` tabeli dla każdego konta użytkownika. Ten typ relacji jest określana jako jeden.

Teraz, gdy mamy modelu danych tworzone są nam będzie go. Kroki 2 i 3 przedstawiono, jak wyświetlić i edytować macierzystego informacji o ich miejscowość, strony głównej i podpis aktualnie zalogowanego użytkownika. Interfejs dla uwierzytelnionych użytkowników przesłać nowe komentarze do księgi gości i wyświetlić istniejące zostanie utworzony w kroku 4.

## <a name="step-2-displaying-the-users-home-town-homepage-and-signature"></a>Krok 2: Wyświetlanie miejscowość głównej, strony głównej i podpis użytkownika

Istnieje wiele sposobów, aby umożliwić aktualnie zalogowanego użytkownika wyświetlić i edytować jego macierzystego informacje miejscowość, strony głównej i podpis. Można ręcznie utworzyć interfejsu użytkownika z polem tekstowym i formantów etykiet lub firma Microsoft może użyć jednej z danych formantów sieci Web, takich jak kontrola widoku DetailsView. Do wykonania w bazie danych `SELECT` i `UPDATE` instrukcje firma Microsoft może zapisać ADO.NET kod w klasie związanej z kodem naszą stronę lub można również stosować deklaratywne podejścia z SqlDataSource. W idealnym przypadku naszej aplikacji może zawierać architektury warstwowych, firma Microsoft może to wywołać programowo z klasy związane z kodem strony lub deklaratywnie za pomocą kontrolki ObjectDataSource.

Ponieważ ta seria samouczek koncentruje się na uwierzytelnianie formularzy, autoryzacji, kont użytkowników i role, nie zostanie dogłębną dyskusję te opcje dostępu do danych lub dlaczego architektura warstwowego jest preferowane bezpośrednio wykonywania instrukcji SQL na stronie platformy ASP.NET. Użyjemy przeprowadzenie przy użyciu widoku DetailsView i SqlDataSource — opcja najszybszym i najprostszym —, ale kwestie omówione pewnością można zastosować do alternatywnego logiki sieci Web formantów i dane dostępu. Aby uzyskać więcej informacji na temat pracy z danymi w programie ASP.NET odwoływać się do mojej *[Praca z danymi w programie ASP.NET 2.0](../../data-access/index.md)* samouczka serii.

Otwórz `AdditionalUserInfo.aspx` strony `Membership` folderu i Dodaj do strony, ustawienie dla jego właściwości ID formantu widoku DetailsView `UserProfile` i wyczyszczenie jego `Width` i `Height` właściwości. Rozwiń tagów inteligentnych DetailsView i wybierz powiązać go z formantem źródła danych. Zostanie uruchomiony Kreator konfiguracji źródła danych (patrz rysunek 7). Pierwszym krokiem prośba o Określ typ źródła danych. Ponieważ zamierzamy Połącz bezpośrednio do `SecurityTutorials` bazy danych, wybierz ikonę bazy danych, określając `ID` jako `UserProfileDataSource`.


[![Dodaj formant SqlDataSource o nazwie UserProfileDataSource](storing-additional-user-information-vb/_static/image20.png)](storing-additional-user-information-vb/_static/image19.png)

**Rysunek 7**: Dodaj nowe kontrolki SqlDataSource, o nazwie `UserProfileDataSource` ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](storing-additional-user-information-vb/_static/image21.png))


Następnym ekranie wyświetli monit o bazy danych. Parametry połączenia w został już zdefiniowany `Web.config` dla `SecurityTutorials` bazy danych. Nazwa parametrów połączenia — `SecurityTutorialsConnectionString` — powinien znajdować się na liście rozwijanej. Wybierz tę opcję i kliknij przycisk Dalej.


[![Z listy rozwijanej wybierz SecurityTutorialsConnectionString](storing-additional-user-information-vb/_static/image23.png)](storing-additional-user-information-vb/_static/image22.png)

**Rysunek 8**: Wybierz `SecurityTutorialsConnectionString` z listy rozwijanej ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](storing-additional-user-information-vb/_static/image24.png))


Kolejne ekranu zapyta firmie Microsoft w celu określenia tabel i kolumn w zapytaniu. Wybierz `UserProfiles` tabeli z listy rozwijanej i sprawdź wszystkie kolumny.


[![Przełącz kopię wszystkich kolumn z tabeli UserProfiles](storing-additional-user-information-vb/_static/image26.png)](storing-additional-user-information-vb/_static/image25.png)

**Rysunek 9**: przełącz ponownie wszystkie kolumny `UserProfiles` tabeli ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](storing-additional-user-information-vb/_static/image27.png))


Bieżącego zapytania w zwraca na rysunku nr 9 *wszystkie* rekordów `UserProfiles`, ale opiszemy tylko w rekordzie aktualnie zalogowanego użytkownika. Aby dodać `WHERE` klauzuli, kliknij przycisk `WHERE` przycisk, aby wyświetlić Dodaj `WHERE` klauzuli dialogowym (zobacz rysunek 10). Tutaj możesz wybierać kolumny do filtrowania, operator i źródła parametru filtru. Wybierz `UserId` jako kolumny i "=" jako operatora.

Niestety nie istnieje źródło parametru wbudowanych do zwrócenia aktualnie zalogowanego użytkownika `UserId` wartość. Musimy programowo pobrania tej wartości. W związku z tym Ustaw listy rozwijanej źródła na "Brak," kliknij przycisk Dodaj przycisk, aby dodać parametr, a następnie kliknij przycisk OK.


[![Dodaj parametr filtru w kolumnie identyfikator użytkownika](storing-additional-user-information-vb/_static/image29.png)](storing-additional-user-information-vb/_static/image28.png)

**Na rysunku nr 10**: Dodaj parametr filtru na `UserId` kolumny ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](storing-additional-user-information-vb/_static/image30.png))


Po kliknięciu przycisku OK powrócisz do ekranu pokazano na rysunku 9. Teraz, jednak zapytanie SQL w dolnej części ekranu powinny zawierać `WHERE` klauzuli. Kliknij przycisk Dalej, aby przejść do ekranu "Test zapytania". W tym miejscu można uruchomić zapytanie i wyświetlić wyniki. Kliknij przycisk Zakończ, aby zakończyć pracę kreatora.

Po zakończeniu pracy Kreatora konfiguracji źródła danych programu Visual Studio tworzy kontrolkę SqlDataSource, na podstawie ustawień określonych w kreatorze. Ponadto ręcznie dodaje BoundFields do widoku DetailsView dla każdej kolumny zwracane przez SqlDataSource `SelectCommand`. Nie istnieje potrzeba pokazanie `UserId` pola w widoku DetailsView, ponieważ użytkownik nie musi znać tej wartości. Można usunąć to pole bezpośrednio z poziomu znacznika deklaratywne kontrolki widoku szczegółów lub kliknij "Edytuj pola" łącze z jego tagów inteligentnych.

W tym momencie znaczników deklaratywne ze strony powinien wyglądać podobnie do następującego:

[!code-aspx[Main](storing-additional-user-information-vb/samples/sample1.aspx)]

Musimy programowane Ustawianie formantu SqlDataSource `UserId` parametru aktualnie zalogowanego użytkownika `UserId` przed danych jest zaznaczone. Można to zrobić, tworząc program obsługi zdarzeń dla SqlDataSource `Selecting` zdarzenia i dodanie poniższego kodu istnieje:

[!code-vb[Main](storing-additional-user-information-vb/samples/sample2.vb)]

Powyższy kod uruchamia, uzyskując odwołanie do aktualnie zalogowanego użytkownika, wywołując `Membership` klasy `GetUser` metody. To polecenie zwróci `MembershipUser` obiektu, którego `ProviderUserKey` zawiera właściwość `UserId`. `UserId` Następnie przypisać wartości do SqlDataSource `@UserId` parametru.

> [!NOTE]
> `Membership.GetUser()` Metoda zwraca informacje o aktualnie zalogowanego użytkownika. Jeśli użytkownik anonimowy odwiedzania strony, zwróci wartość `Nothing`. W takim przypadku będzie to prowadzić do `NullReferenceException` na następujący wiersz kodu podczas próby odczytu `ProviderUserKey` właściwości. Oczywiście nie trzeba martwić się o `Membership.GetUser()` zwracanie postanowienia `AdditionalUserInfo.aspx` strony, ponieważ został skonfigurowany Autoryzacja adresów URL z poprzedniego samouczka, dzięki czemu tylko uwierzytelnieni użytkownicy mogą uzyskiwać dostęp do zasobów ASP.NET w tym folderze. Jeśli potrzebujesz dostępu do informacji o aktualnie zalogowanego użytkownika na stronie, gdy dostęp anonimowy jest dozwolone, upewnij się sprawdzić, czy `MembershipUser` obiektu zwróconego z `GetUser()` metoda nie ma nic przed jego właściwości odwołania.


W przypadku odwiedzenia `AdditionalUserInfo.aspx` strony za pośrednictwem przeglądarki zostanie wyświetlona strona puste, ponieważ mamy jeszcze żadnych wierszy, aby dodać `UserProfiles` tabeli. W kroku 6 przedstawiono, jak dostosować formancie CreateUserWizard można automatycznie dodać nowy wiersz do `UserProfiles` tabeli po utworzeniu nowego konta użytkownika. Jednak obecnie będzie należy ręcznie utworzyć rekord w tabeli.

Przejdź do Eksploratora bazy danych w programie Visual Studio, a następnie rozwiń folder tabel. Kliknij prawym przyciskiem myszy `aspnet_Users` tabeli i wybierz opcję "Pokaż tabeli danych" Aby wyświetlić rekordy w tabeli, tak samo postąpić w `UserProfiles` tabeli. Rysunek 11 zawiera te wyniki podczas sąsiadująco w pionie. W bazie danych są obecnie `aspnet_Users` rekordy Bruce, Krzysztof i Tito, ale żadne rekordy w `UserProfiles` tabeli.


[![Zawartość aspnet_Users i tabele UserProfiles są wyświetlane.](storing-additional-user-information-vb/_static/image32.png)](storing-additional-user-information-vb/_static/image31.png)

**Rysunek 11**: zawartość `aspnet_Users` i `UserProfiles` tabele są wyświetlane ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](storing-additional-user-information-vb/_static/image33.png))


Dodaj nowy rekord do `UserProfiles` tabeli, ręcznie wpisując wartości `HomeTown`, `HomepageUrl`, i `Signature` pól. Najprostszym sposobem, aby uzyskać prawidłową `UserId` wartość w nowym `UserProfiles` rekord jest wybranie `UserId` pole z określonego konta użytkownika w `aspnet_Users` tabeli i skopiuj i wklej ją do `UserId` w `UserProfiles`. Przedstawia rysunek 12 `UserProfiles` tabeli po dodaniu nowego rekordu dla Bruce.


[![Rekord został dodany do UserProfiles dla Bruce](storing-additional-user-information-vb/_static/image35.png)](storing-additional-user-information-vb/_static/image34.png)

**Rysunek 12**: rekord został dodany do `UserProfiles` dla Bruce ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](storing-additional-user-information-vb/_static/image36.png))


Wróć do `AdditionalUserInfo.aspx page`, zalogowany jako Bruce. Jak pokazano na rysunku 13, Bruce jego ustawienia są wyświetlane.


[![Obecnie odwiedzający użytkownik jest wyświetlany jego ustawienia](storing-additional-user-information-vb/_static/image38.png)](storing-additional-user-information-vb/_static/image37.png)

**Rysunek 13**: obecnie odwiedzający użytkownika jest wyświetlany jego ustawienia ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](storing-additional-user-information-vb/_static/image39.png))


> [!NOTE]
> Przejdź dalej i ręcznie dodaj rekordy w `UserProfiles` tabeli dla każdego użytkownika członkostwa. W kroku 6 przedstawiono, jak dostosować formancie CreateUserWizard można automatycznie dodać nowy wiersz do `UserProfiles` tabeli po utworzeniu nowego konta użytkownika.


## <a name="step-3-allowing-the-user-to-edit-his-home-town-homepage-and-signature"></a>Krok 3: Umożliwiający użytkownikowi edytowanie jego miejscowość głównej, strony głównej i podpisu

W tym momencie aktualnie zalogowanego użytkownika można wyświetlić ich miastem macierzystego, strony głównej i ustawienie podpisu, ale jeszcze nie mogą modyfikować je. Teraz należy zaktualizować formantu widoku DetailsView, tak aby dane mogą być edytowane.

Najpierw konieczne jest dodać `UpdateCommand` dla SqlDataSource, określając `UPDATE` instrukcji do wykonania i odpowiednie parametry. Wybierz SqlDataSource i w oknie właściwości, kliknij przycisk wielokropka obok właściwości UpdateQuery, aby wyświetlić okno dialogowe Edytor poleceń i parametrów. Wprowadź następujące `UPDATE` instrukcji w polu tekstowym:

[!code-sql[Main](storing-additional-user-information-vb/samples/sample3.sql)]

Następnie kliknij przycisk "Odśwież parametry", co spowoduje utworzenie parametru w formancie SqlDataSource `UpdateParameters` kolekcji dla każdego z parametrów w `UPDATE` instrukcji. Pozostaw źródła dla wszystkich zestawu parametrów None, a następnie kliknij przycisk OK, aby zakończyć okno dialogowe.


[![Określ UpdateCommand SqlDataSource i UpdateParameters](storing-additional-user-information-vb/_static/image41.png)](storing-additional-user-information-vb/_static/image40.png)

**Rysunek 14**: Określ SqlDataSource `UpdateCommand` i `UpdateParameters` ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](storing-additional-user-information-vb/_static/image42.png))


Z powodu dodatków wprowadziliśmy z formantem SqlDataSource, widoku DetailsView formantu można teraz obsługuje edycji. Z tagów inteligentnych DetailsView zaznacz pole wyboru "Włącz edytowanie". To działanie powoduje dodanie CommandField do formantu `Fields` kolekcji z jego `ShowEditButton` właściwość, ustaw wartość True. Renderuje przycisk Edytuj, gdy widoku DetailsView jest wyświetlany w trybie tylko do odczytu i aktualizacji i przyciski Anuluj podczas wyświetlania w trybie edycji. Zamiast konieczności przez użytkownika kliknij przycisk Edytuj, jednak firma Microsoft może mieć renderowania widoku DetailsView w stanie "zawsze można edytować" przez ustawienie formantu widoku DetailsView [ `DefaultMode` właściwości](https://msdn.microsoft.com/library/system.web.ui.webcontrols.detailsview.defaultmode.aspx) do `Edit`.

Wprowadzone zmiany znaczników deklaratywne formantu widoku DetailsView powinien wyglądać podobny do następującego:

[!code-aspx[Main](storing-additional-user-information-vb/samples/sample4.aspx)]

Należy pamiętać, dodanie CommandField i `DefaultMode` właściwości.

Przejdź dalej i przetestować tę stronę za pośrednictwem przeglądarki. Podczas odwiedzania z użytkownikiem, który ma odpowiadający mu rekord w `UserProfiles`, ustawienia użytkownika są wyświetlane w interfejsie można edytować.


[![Można edytować interfejs renderuje widoku DetailsView](storing-additional-user-information-vb/_static/image44.png)](storing-additional-user-information-vb/_static/image43.png)

**Rysunek 15**: widoku DetailsView renderuje można edytować interfejsu ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](storing-additional-user-information-vb/_static/image45.png))


Spróbuj zmianę wartości i klikając przycisk Aktualizuj. Wygląda na to, jak gdyby nic się nie dzieje. Brak odświeżania strony i wartości są zapisywane w bazie danych, ale nie istnieje żadne wizualny, który wystąpił podczas zapisywania.

Aby rozwiązać ten problem, wróć do programu Visual Studio i Dodaj formant etykiety powyżej widoku DetailsView. Ustaw jego `ID` do `SettingsUpdatedMessage`, jego `Text` dla właściwości "Twoje ustawienia zostały zaktualizowane," i jego `Visible` i `EnableViewState` właściwości `False`.

[!code-aspx[Main](storing-additional-user-information-vb/samples/sample5.aspx)]

Potrzebujemy wyświetlić `SettingsUpdatedMessage` etykiety przy każdej aktualizacji widoku DetailsView. W tym celu należy utworzyć program obsługi zdarzeń dla DetailsView `ItemUpdated` zdarzeń i Dodaj następujący kod:

[!code-vb[Main](storing-additional-user-information-vb/samples/sample6.vb)]

Wróć do `AdditionalUserInfo.aspx` strony za pośrednictwem przeglądarki i aktualizacji danych. Teraz, zostanie wyświetlony komunikat stanu przydatne.


[![Krótki komunikat jest wyświetlany podczas aktualizowanie ustawień](storing-additional-user-information-vb/_static/image47.png)](storing-additional-user-information-vb/_static/image46.png)

**Rysunek 16**: krótki komunikat jest wyświetlany po zaktualizowaniu ustawień ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](storing-additional-user-information-vb/_static/image48.png))


> [!NOTE]
> Kontrolki widoku szczegółów elementu edycji pozostawia interfejsu się być potrzebne. Używa standardowych wymiarach pola tekstowe, ale pole podpisu prawdopodobnie powinien być wielowierszowego pola tekstowego. RegularExpressionValidator mają służyć do zapewnienia, że adres URL strony głównej, czy wprowadzona, rozpoczyna się od "http://" lub "https://". Ponadto, ponieważ widoku DetailsView formant ma jego `DefaultMode` ustawioną właściwość `Edit`, przycisku Anuluj wykonywać żadnych czynności. Go albo należy usunąć lub, po kliknięciu przekieruje użytkownika do innej strony (takie jak `~/Default.aspx`). Te ulepszenia wykonywania I pozostaw dla czytnika.


### <a name="adding-a-link-to-theadditionaluserinfoaspxpage-in-the-master-page"></a>Dodawanie Linku do`AdditionalUserInfo.aspx`strony w strony wzorcowej

Witryna sieci Web nie ma obecnie łącza do `AdditionalUserInfo.aspx` strony. Wprowadź adres URL strony bezpośrednio na pasku adresu przeglądarki jest jedynym sposobem, aby uzyskać do niej dostęp. Możemy dodać łącze do tej strony w `Site.master` strony wzorcowej.

Odwołaj się, że strony wzorcowej zawiera formantu LoginView sieci Web w jego `LoginContent` ContentPlaceHolder wyświetlający różny kod znaczników dla użytkowników uwierzytelnionych i anonimowych. Zaktualizuj formantu LoginView `LoggedInTemplate` aby uwzględnić łącze do `AdditionalUserInfo.aspx` strony. Po wprowadzeniu tych zmian LoginView znaczników deklaratywne kontrolki powinien wyglądać podobnie do poniższej:

[!code-aspx[Main](storing-additional-user-information-vb/samples/sample7.aspx)]

Należy pamiętać, dodanie `lnkUpdateSettings` formant hiperłącza do `LoggedInTemplate`. Z tego łącza w miejscu uwierzytelnieni użytkownicy mogą szybko przejść do strony do wyświetlania i modyfikowania ustawień macierzystego miejscowość, strony głównej i podpis.

## <a name="step-4-adding-new-guestbook-comments"></a>Krok 4: Dodawanie nowe komentarze księgi gości

`Guestbook.aspx` Jest strony, gdy uwierzytelnieni użytkownicy mogą wyświetlać księgi gości i zostaw komentarz. Zacznijmy Tworzenie interfejsu, aby dodać nowe komentarze księgi gości.

Otwórz `Guestbook.aspx` strony w programie Visual Studio i utworzyć interfejsu użytkownika, składające się z dwóch pól tekstowych, jeden dla podmiotu nowy komentarz i jeden dla jego treści. Ustaw pierwszą kontrolkę TextBox `ID` właściwości `Subject` i jego `Columns` właściwości do 40; Ustaw sekundę `ID` do `Body`, jego `TextMode` do `MultiLine`i jego `Width` i `Rows` właściwości "95%" i 8, odpowiednio. Aby ukończyć interfejsu użytkownika, należy dodać kontrolkę przycisku sieci Web o nazwie `PostCommentButton` i ustawić jej `Text` dla właściwości "Comment Your" Post".

Ponieważ każdy komentarz księgi gości wymaga temat i treść, Dodaj RequiredFieldValidator dla poszczególnych pól tekstowych. Ustaw `ValidationGroup` właściwości tych kontrolek do "EnterComment"; Podobnie, ustaw `PostCommentButton` formantu `ValidationGroup` dla właściwości "EnterComment". Aby uzyskać więcej informacji na temat ASP. Formanty walidacji w sieci, wyewidencjonowanie [weryfikacji formularza w programie ASP.NET](http://www.4guysfromrolla.com/webtech/090200-1.shtml), [pincety formanty walidacji w programie ASP.NET 2.0](http://aspnet.4guysfromrolla.com/articles/112305-1.aspx)i [samouczek kontrolek serwera sprawdzania poprawności](http://www.w3schools.com/aspnet/aspnet_refvalidationcontrols.asp) na [W3Schools](http://www.w3schools.com/).

Po obsługuje tworzenie interfejsu użytkownika strony ma deklaratywne znaczników powinien wyglądać jak poniżej:

[!code-aspx[Main](storing-additional-user-information-vb/samples/sample8.aspx)]

Z pełną interfejsu użytkownika ma wstawić nowy rekord w naszym następne zadanie `GuestbookComments` tabeli, gdy `PostCommentButton` zostanie kliknięty. Można to zrobić na kilka sposobów: Firma Microsoft można napisać kod ADO.NET w przycisku `Click` obsługi zdarzeń; można dodać kontrolkę SqlDataSource ze stroną, skonfiguruj jego `InsertCommand`, a następnie wywołać jej `Insert` metody z `Click` zdarzeń Program obsługi; lub firma Microsoft może kompilacji warstwy środkowej, odpowiedzialna za wstawianie nowe komentarze księgi gości, a następnie wywołaj tę funkcję z `Click` obsługi zdarzeń. Ponieważ analizujemy przy użyciu SqlDataSource w kroku 3, Użyjmy ADO.NET kodu w tym miejscu.

> [!NOTE]
> Klasy ADO.NET używana do uzyskania programowego dostępu do danych z bazy danych programu Microsoft SQL Server znajdują się w `System.Data.SqlClient` przestrzeni nazw. Może być konieczne do importowania tej przestrzeni nazw do klasy związane z kodem ze strony (tj. `Imports System.Data.SqlClient`).


Tworzenie procedury obsługi zdarzeń dla `PostCommentButton`w `Click` zdarzeń i Dodaj następujący kod:

[!code-vb[Main](storing-additional-user-information-vb/samples/sample9.vb)]

`Click` Uruchamia program obsługi zdarzeń przez sprawdzenie, czy dane dostarczone przez użytkownika są prawidłowe. Jeśli nie jest dostępne, przed wstawieniem rekordu kończy działanie programu obsługi zdarzeń. Zakładając, że podane dane są prawidłowe, aktualnie zalogowanego użytkownika `UserId` wartość jest pobierana i przechowywane w `currentUserId` zmiennej lokalnej. Ta wartość jest potrzebna, ponieważ firma Microsoft dostarcza `UserId` wartości podczas wstawiania rekordów do `GuestbookComments`.

Następujące, parametrów połączenia dla `SecurityTutorials` bazy danych są pobierane z `Web.config` i `INSERT` jest określona w instrukcji SQL. A `SqlConnection` obiekt jest tworzony i otwierany. Następnie `SqlCommand` obiekt jest tworzony i wartości dla parametrów używane w `INSERT` zapytania są przypisane. `INSERT` Następnie wykonaniu instrukcji i zamknięcie połączenia. Po zakończeniu obsługi zdarzeń `Subject` i `Body` pól tekstowych `Text` właściwości zostały wyczyszczone tak, aby wartości użytkownika nie są zachowywane między ogłaszania zwrotnego.

Przejdź dalej i przetestować tę stronę w przeglądarce. Ponieważ ta strona jest `Membership` folder nie jest on dostępny dla osób odwiedzających anonimowy. W związku z tym należy najpierw zalogować się na (jeśli jeszcze tego nie zrobiono). Wprowadź wartość do `Subject` i `Body` pola tekstowe i kliknij przycisk `PostCommentButton` przycisku. To spowoduje, że mają zostać dodane do nowego rekordu `GuestbookComments`. Strony tematu i treści podane są czyszczone z pól tekstowych.

Po kliknięciu przycisku `PostCommentButton` przycisk istnieje nie jest brak wizualne komentarz dodany do księgi gości. Nadal trzeba zaktualizować tę stronę, aby wyświetlić istniejące komentarze księgi gości, które spowoduje wykonanie w kroku 5. Po możemy realizacji tego komentarza po prostu dodane będą wyświetlane w komentarza, zapewnienie odpowiedniej wizualne. Teraz należy potwierdzić, że komentarz księgi gości został zapisany, sprawdzając zawartość `GuestbookComments` tabeli.

Rysunek 17 wyświetlana jest zawartość `GuestbookComments` tabeli po dwóch komentarze zostały wystawione.


[![Można wyświetlić komentarze księgi gości w tabeli GuestbookComments](storing-additional-user-information-vb/_static/image50.png)](storing-additional-user-information-vb/_static/image49.png)

**Rysunek 17**: można zapoznać się z komentarzami księgi gości w `GuestbookComments` tabeli ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](storing-additional-user-information-vb/_static/image51.png))


> [!NOTE]
> Jeśli użytkownik próbuje Wstaw komentarz księgi gości, który zawiera potencjalnie niebezpiecznych znaczników — takich jak HTML — ASP.NET zgłosi `HttpRequestValidationException`. Aby dowiedzieć się więcej na temat tego wyjątku, dlaczego jest zgłaszany, oraz sposób zezwolić użytkownikom na przesłanie potencjalnie niebezpiecznych wartości, należy zajrzeć [żądania oficjalny dokument weryfikacji](../../../../whitepapers/request-validation.md).


## <a name="step-5-listing-the-existing-guestbook-comments"></a>Krok 5: Lista komentarze księgi gości

Oprócz pozostawienie komentarze, użytkownik odwiedzający `Guestbook.aspx` strony powinny również móc wyświetlić komentarze księgi gości. W tym celu Dodaj formant ListView o nazwie `CommentList` do dolnej części strony.

> [!NOTE]
> ListView — formant jest nowym składnikiem programu ASP.NET w wersji 3.5. Jest on przeznaczony do wyświetlania listy elementów w bardzo można dostosowywać i elastyczny układ, ale nadal oferuje wbudowane edytowanie, wstawianie, usuwanie, stronicowania i sortowania funkcji takich jak widoku GridView. Jeśli używasz programu ASP.NET 2.0, należy zamiast tego użyj formant DataList lub elementu powtarzanego. Aby uzyskać więcej informacji na temat używania element ListView, zobacz [Scott Guthrie](https://weblogs.asp.net/scottgu/)w wpis w blogu [asp: ListView kontroli](https://weblogs.asp.net/scottgu/archive/2007/08/10/the-asp-listview-control-part-1-building-a-product-listing-page-with-clean-css-ui.aspx)i Moje artykułu [wyświetlanie danych z formantem ListView](http://aspnet.4guysfromrolla.com/articles/122607-1.aspx).


Otwórz element ListView tagów inteligentnych i z listy rozwijanej wybierz źródło danych, należy powiązać formant do nowego źródła danych. Jak widzieliśmy w kroku 2, zostanie uruchomiony Kreator konfiguracji źródła danych. Wybierz ikonę bazy danych, nazwę wynikowy SqlDataSource `CommentsDataSource`i kliknij przycisk OK. Następnie wybierz pozycję `SecurityTutorialsConnectionString` połączenia ciąg z listy rozwijanej i kliknij przycisk Dalej.

W tym momencie w kroku 2 Firma Microsoft określi dane do zapytania pobrania `UserProfiles` tabeli z listy rozwijanej i Wybieranie kolumn do zwrócenia (odwołują się do na rysunku nr 9). Teraz, jednak chcemy sformułować instrukcję SQL ponownie ściągający rekordy nie tylko z `GuestbookComments`, ale także osoby wystawiającej komentarz miejscowość macierzystego, strony głównej podpisu i nazwy użytkownika. W związku z tym wybierz przycisk radiowy "Określ niestandardową instrukcję SQL lub procedurę składowaną" i kliknij przycisk Dalej.

Zostaną wyświetlone na ekranie "Zdefiniuj niestandardowe instrukcji lub procedury przechowywane". Kliknij przycisk konstruktora zapytań do postaci graficznej tworzenia zapytania. Konstruktor kwerend rozpoczyna się od nas o określić tabelę, którą chcemy udostępnić zapytania z monitowania. Wybierz `GuestbookComments`, `UserProfiles`, i `aspnet_Users` tabele i kliknij przycisk OK. Spowoduje to dodanie wszystkie trzy tabele na powierzchnię projektu. Ponieważ ma ograniczeń klucza obcego między `GuestbookComments`, `UserProfiles`, i `aspnet_Users` tabel, konstruktora zapytań automatycznie `JOIN` s tych tabel.

Całą zawartość pozostaje do określania kolumn do zwrócenia. Z `GuestbookComments` tabeli wybierz `Subject`, `Body`, i `CommentDate` kolumn; return `HomeTown`, `HomepageUrl`, i `Signature` kolumny z `UserProfiles` tabeli; i zwracać `UserName` z `aspnet_Users`. Ponadto Dodaj "`ORDER BY CommentDate DESC`" na końcu `SELECT` zapytania, aby najpierw zwracane są najnowsze wpisy. Po wprowadzeniu tych opcji, interfejsu konstruktora zapytań powinien wyglądać podobnie do zrzut na rysunku 18 ekranu.


[![Zapytanie zbudowanych sprzężenia GuestbookComments, UserProfiles i tabele aspnet_Users](storing-additional-user-information-vb/_static/image53.png)](storing-additional-user-information-vb/_static/image52.png)

**Rysunek 18**: zapytanie wykonane `JOIN` s `GuestbookComments`, `UserProfiles`, i `aspnet_Users` tabele ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](storing-additional-user-information-vb/_static/image54.png))


Kliknij przycisk OK, aby zamknąć okno konstruktora zapytań i powrócić do ekranu "Zdefiniuj niestandardowe instrukcji lub procedury przechowywane". Kliknij przycisk Dalej wcześniejszym na ekranie "Zapytania Test", gdzie może wyświetlić wyniki zapytania, klikając przycisk zapytanie testu. Gdy wszystko będzie gotowe, kliknij przycisk Zakończ, aby zakończyć pracę Kreatora konfigurowania źródła danych.

Gdy firma Microsoft ukończono pracę Kreatora konfigurowania źródła danych w kroku 2, skojarzony formant widoku DetailsView `Fields` kolekcji został zaktualizowany do uwzględnienia elementu BoundField dla każdej kolumny zwracane przez `SelectCommand`. Element ListView, jednak pozostanie bez zmian; nadal trzeba zdefiniować jego układ. Układ elementu ListView można utworzyć ręcznie przy użyciu jego deklaratywne znaczników lub z opcją "Konfiguruj element ListView" w tagu inteligentnego. I zwykle woli ręcznie definiujący kod znaczników, jednak użyć dowolnej metody jest najbardziej fizyczne do Ciebie.

I zakończył z następującym `LayoutTemplate`, `ItemTemplate`, i `ItemSeparatorTemplate` formantu ListView:

[!code-aspx[Main](storing-additional-user-information-vb/samples/sample10.aspx)]

`LayoutTemplate` Definiuje znaczników emitowane przez formant, podczas gdy `ItemTemplate` renderuje każdego elementu zwrócony przez SqlDataSource. `ItemTemplate`Dla znaczników wynikowy jest umieszczany w `LayoutTemplate`w `itemPlaceholder` formantu. Oprócz `itemPlaceholder`, `LayoutTemplate` zawiera formant DataPager, co ogranicza element ListView do wyświetlania tylko 10 komentarze księgi gości na stronie (ustawienie domyślne) i renderuje interfejsu stronicowania.

Moje `ItemTemplate` temat każdy komentarz księgi gości w `<h4>` element z treści znajdujących się poniżej podmiotu. Składnia służąca do wyświetlania treści ma danych zwróconych przez `Eval("Body")` instrukcji wiązania danych konwertuje go na ciąg i zamienia podziały wierszy z `<br />` elementu. Ta konwersja wymaga Pokaż podziały wprowadzana w trakcie przesyłania komentarza, ponieważ odstęp jest ignorowana przez HTML. Podpis użytkownika jest wyświetlana poniżej treści kursywą, po którym następuje przez użytkownika miejscowość macierzystego, link do jego głównej, daty i godziny wprowadzone komentarz oraz nazwy użytkownika osoby, która pozostanie komentarz.

Poświęć chwilę, aby wyświetlić stronę za pośrednictwem przeglądarki. Powinny pojawić się komentarze, dodawane do księgi gości w kroku 5 tutaj wyświetlane.


[![Teraz Guestbook.aspx Wyświetla komentarze księgi gości](storing-additional-user-information-vb/_static/image56.png)](storing-additional-user-information-vb/_static/image55.png)

**Rysunek 19**: `Guestbook.aspx` komentarze księgi gości są obecnie wyświetlane ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](storing-additional-user-information-vb/_static/image57.png))


Spróbuj dodać nowy komentarz do księgi gości. Po kliknięciu `PostCommentButton` przycisk Strona ogłoszeń Wstecz i komentarz został dodany do bazy danych, ale ListView — formant nie jest aktualizowana w celu wyświetlenia nowy komentarz. Można to naprawić przez:

- Aktualizowanie `PostCommentButton` przycisku `Click` obsługi zdarzeń, tak że wywołuje formant ListView `DataBind()` metody po wstawieniu nowy komentarz do bazy danych, lub
- Ustawienia formantu ListView `EnableViewState` właściwości `False`. Takie podejście działa, ponieważ po wyłączeniu formantu widoku stanu, należy ponownie powiązać w danych źródłowych, na każdym ogłaszania zwrotnego.

Samouczek witryny sieci Web do pobrania z tego samouczka przedstawiono obie techniki. ListView — formant `EnableViewState` właściwości `False` kodu potrzebne do programowego ponownie powiązać danych do elementu ListView jest obecna w `Click` obsługi zdarzeń, ale jest oznaczone jako komentarz.

> [!NOTE]
> Obecnie `AdditionalUserInfo.aspx` strona umożliwia wyświetlanie i edytowanie ustawień macierzystego miejscowość, strony głównej i podpis. Może być dobry zaktualizować `AdditionalUserInfo.aspx` do wyświetlenia zalogowanego w komentarzach księgi gości użytkownika. Oznacza to, oprócz badanie i modyfikowania jej informacji, użytkownik może odwiedzić `AdditionalUserInfo.aspx` stronę, aby zobaczyć, jakie księgi gości komentarzy użytkownik znajduje się w przeszłości. I pozostaw to wykonywania zainteresowanych czytnika.


## <a name="step-6-customizing-the-createuserwizard-control-to-include-an-interface-for-the-home-town-homepage-and-signature"></a>Krok 6: Dostosowywanie formancie CreateUserWizard uwzględnienie interfejs miejscowość głównej, strony głównej i podpisu

`SELECT` Zapytania używanego przez `Guestbook.aspx` strona używa `INNER JOIN` połączyć powiązane rekordy między `GuestbookComments`, `UserProfiles`, i `aspnet_Users` tabel. Jeśli użytkownik, który nie ma rekordu w `UserProfiles` komentarz nawiązuje księgi gości komentarz nie będą wyświetlane w elemencie ListView, ponieważ `INNER JOIN` zwraca tylko `GuestbookComments` rekordów, gdy ma pasującego rekordów w `UserProfiles` i `aspnet_Users`. I jak widzieliśmy w kroku 3, jeśli użytkownik nie ma rekordu `UserProfiles` klika nie można wyświetlić ani edytować jej ustawienia w `AdditionalUserInfo.aspx` strony.

Needless oznacza, ze względu na naszych projektu decyzji jest ważne, czy każde konto użytkownika w systemu członkostwa ma zgodnego rejestrowane w `UserProfiles` tabeli. Co chcemy jest odpowiedni rekord, który ma zostać dodany do `UserProfiles` zawsze, gdy tworzone jest nowe konto użytkownika członkostwa, za pośrednictwem CreateUserWizard.

Zgodnie z opisem w [ *tworzenia kont użytkowników* ](creating-user-accounts-vb.md) zgłasza samouczek, po utworzeniu nowego konta użytkownika członkostwa w formancie CreateUserWizard jego [ `CreatedUser` zdarzenia](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.createduser.aspx). Możemy utworzyć program obsługi zdarzeń dla tego zdarzenia, uzyskać identyfikator UserId dla użytkownika właśnie utworzony, a następnie Wstawianie rekordu do `UserProfiles` tabeli z wartościami domyślnymi dla `HomeTown`, `HomepageUrl`, i `Signature` kolumn. Co więcej istnieje możliwość monit o podanie wartości tych dostosowując interfejsu w formancie CreateUserWizard uwzględnienie dodatkowych pól tekstowych.

Najpierw Zobaczmy, jak dodać nowy wiersz do `UserProfiles` w tabeli `CreatedUser` obsługi zdarzeń z wartościami domyślnymi. Następujące, przedstawiono sposób dostosowywania interfejsu użytkownika w formancie CreateUserWizard uwzględnienie dodatkowe pola do zbierania miejscowość macierzystego, strony głównej i podpis nowego użytkownika.

### <a name="adding-a-default-row-touserprofiles"></a>Dodawanie wiersza do domyślnego`UserProfiles`

W [ *tworzenia kont użytkowników* ](creating-user-accounts-vb.md) samouczek dodaliśmy formancie CreateUserWizard do `CreatingUserAccounts.aspx` strony `Membership` folderu. Aby przypisać CreateUserWizard kontrola dodać rekordu do `UserProfiles` tabeli po utworzeniu konta użytkownika, należy zaktualizować funkcjonalność w formancie CreateUserWizard. Zamiast wprowadzania tych zmian do `CreatingUserAccounts.aspx` pozycję Dodajmy zamiast tego nowego formancie CreateUserWizard do `EnhancedCreateUserWizard.aspx` i wprowadzić te zmiany w tym samouczku strony.

Otwórz `EnhancedCreateUserWizard.aspx` strony w programie Visual Studio i przeciągnij z przybornika na stronę formancie CreateUserWizard. Ustaw w formancie CreateUserWizard `ID` właściwości `NewUserWizard`. Jak wspomniano w [ *tworzenia kont użytkowników* ](creating-user-accounts-vb.md) samouczka CreateUserWizard domyślny interfejs użytkownika wyświetla monit o niezbędne informacje. Po te informacje zostały podane, formantu wewnętrznie tworzy nowe konto użytkownika w ramach członkostwa, wszystko to bez nam konieczności pisania pojedynczy wiersz kodu.

W formancie CreateUserWizard podnosi liczbę zdarzeń podczas jego przepływu pracy. Obiekt odwiedzający dostarcza informacje o żądaniu i przesyła formularz, formancie CreateUserWizard początkowo generowane jego [ `CreatingUser` zdarzeń](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.creatinguser.aspx). Jeśli występuje problem podczas procesu tworzenia [ `CreateUserError` zdarzeń](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.createusererror.aspx) jest uruchamiany; jednak, jeśli użytkownik została pomyślnie utworzona, a następnie [ `CreatedUser` zdarzeń](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.createduser.aspx) jest wywoływane. W [ *tworzenia kont użytkowników* ](creating-user-accounts-vb.md) samouczek utworzyliśmy programu obsługi zdarzeń dla `CreatingUser` zdarzeń, aby upewnić się, że podana nazwa użytkownika nie zawiera żadnych wiodące lub końcowe spacje i który Nazwa użytkownika nie pojawił się dowolne miejsce w haśle.

Aby dodać wiersz w `UserProfiles` tabeli po prostu utworzyć użytkownika, należy utworzyć programu obsługi zdarzeń dla `CreatedUser` zdarzeń. W czasie `CreatedUser` zdarzenie jest wywoływane, już utworzono konto użytkownika w ramach członkostwa, pozwala nam można pobrać wartości identyfikatora użytkownika dla konta.

Tworzenie procedury obsługi zdarzeń dla `NewUserWizard`w `CreatedUser` zdarzeń i Dodaj następujący kod:

[!code-vb[Main](storing-additional-user-information-vb/samples/sample11.vb)]

Powyżej istot kodu przez pobranie nazwa użytkownika konta użytkownika po prostu dodane. Jest to zrobić za pomocą `Membership.GetUser(username)` metoda zwraca informacje dotyczące określonego użytkownika, a następnie użyć `ProviderUserKey` właściwości do pobrania ich identyfikator użytkownika. Nazwa użytkownika wprowadzona przez użytkownika w formancie CreateUserWizard jest dostępny za pośrednictwem jego [ `UserName` właściwości](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.username.aspx).

Następnie parametry połączenia są pobierane z `Web.config` i `INSERT` określono instrukcji. Niezbędne ADO.NET— obiekty są tworzone i wykonania polecenia. Przypisuje kod [ `DBNull` ](https://msdn.microsoft.com/library/system.dbnull.aspx) wystąpienie do `@HomeTown`, `@HomepageUrl`, i `@Signature` parametrów, które powoduje wstawienie bazy danych `NULL` wartości `HomeTown`, `HomepageUrl`, i `Signature` pól.

Odwiedź stronę `EnhancedCreateUserWizard.aspx` stronie za pośrednictwem przeglądarki, a następnie utwórz nowe konto użytkownika. Po wykonaniu tej czynności, wróć do programu Visual Studio i sprawdź zawartość `aspnet_Users` i `UserProfiles` tabele (takich jak robiliśmy na rysunku 12). Powinny pojawić się nowego konta użytkownika w `aspnet_Users` i odpowiadające mu `UserProfiles` wiersza (z `NULL` wartości `HomeTown`, `HomepageUrl`, i `Signature`).


[![Dodano nowe konto użytkownika i UserProfiles rekordu](storing-additional-user-information-vb/_static/image59.png)](storing-additional-user-information-vb/_static/image58.png)

**Rysunek 20**: A nowego konta użytkownika i `UserProfiles` dodano rekord ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](storing-additional-user-information-vb/_static/image60.png))


Po użytkownik ma podany jego nowe informacje o koncie i kliknięty przycisk "Utwórz użytkownika", utworzono konto użytkownika i dodać wiersz do `UserProfiles` tabeli. Następnie wyświetla CreateUserWizard jego `CompleteWizardStep`, która wyświetla komunikat z potwierdzeniem i przycisku Kontynuuj. Kliknięcie przycisku Kontynuuj powoduje odświeżenie strony, ale nie podjęto żadnej akcji, pozostawiając użytkownik zablokował na `EnhancedCreateUserWizard.aspx` strony.

Można określić adres URL wysłać do użytkownika, aby po kliknięciu przycisku Kontynuuj za pośrednictwem formancie CreateUserWizard [ `ContinueDestinationPageUrl` właściwości](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.continuedestinationpageurl.aspx). Ustaw `ContinueDestinationPageUrl` dla właściwości "~ / Membership/AdditionalUserInfo.aspx". Trwa to nowemu użytkownikowi `AdditionalUserInfo.aspx`, gdzie mogą wyświetlać i aktualizować ich ustawienia.

### <a name="customizing-the-createuserwizards-interface-to-prompt-for-the-new-users-home-town-homepage-and-signature"></a>Dostosowywanie interfejsu CreateUserWizard Monituj miejscowość głównej, strony głównej i podpis nowego użytkownika

Interfejs domyślny formancie CreateUserWizard jest wystarczająca dla scenariuszy tworzenia prostego gdzie muszą być zbierane tylko podstawowe informacje o koncie użytkownika, takie jak nazwa użytkownika, hasło i adres e-mail. Ale co zrobić, jeśli trzeba Monituj obiektu odwiedzającego, aby wprowadzić swoje miejscowość macierzystego, strony głównej i podpis podczas tworzenia swoje konto? Istnieje możliwość dostosowania interfejsu w formancie CreateUserWizard do gromadzenia dodatkowych informacji podczas rejestracji, a te informacje mogą być używane w `CreatedUser` obsługi zdarzeń, aby wstawić dodatkowe rekordy do podstawowej bazy danych.

W formancie CreateUserWizard rozszerza formantu kreatora ASP.NET, który jest formant, który umożliwia dewelopera strony do definiowania serii uporządkowanych `WizardSteps`. Formant kreatora renderuje aktywnego kroku i udostępnia interfejsem nawigacji, który umożliwia obiektu odwiedzającego przenieść te kroki. Kreator formantu jest idealny dla podziału długich zadań w kilku krótkich krokach. Aby uzyskać więcej informacji na temat formantu kreatora, zobacz [Tworzenie interfejsu użytkownika krok po kroku z formantu kreatora ASP.NET 2.0](http://aspnet.4guysfromrolla.com/articles/061406-1.aspx).

Znaczników domyślne w formancie CreateUserWizard definiuje dwie `WizardSteps`: `CreateUserWizardStep` i `CompleteWizardStep`.

[!code-aspx[Main](storing-additional-user-information-vb/samples/sample12.aspx)]

Pierwszy `WizardStep`, `CreateUserWizardStep`, renderuje interfejs, który wyświetla monit o nazwę użytkownika, hasło, poczty e-mail i tak dalej. Po osoby odwiedzającej jest możliwe będzie przekazywać te informacje i kliknie przycisk "Utwórz użytkownika", jest ona wyświetlana `CompleteWizardStep`, którym będzie wyświetlany komunikat z potwierdzeniem i przycisku Kontynuuj.

Aby dostosować interfejs formancie CreateUserWizard uwzględnienie dodatkowe pola, możemy:

- <strong>Utwórz co najmniej jeden nowy</strong><strong>`WizardStep`</strong><strong>s zawierają dodatkowe elementy interfejsu użytkownika</strong>. Aby dodać nowy `WizardStep` aby CreateUserWizard, kliknij przycisk "Dodaj lub usuń `WizardStep` s" link z jego tagów inteligentnych można uruchomić `WizardStep` edytora kolekcji. Z tego miejsca można dodać, usunąć lub zmienić kolejność kroków w kreatorze. To podejście, które będą używane na potrzeby tego samouczka.

- <strong>Konwertuj</strong><strong>`CreateUserWizardStep`</strong><strong>do edycji</strong><strong>`WizardStep`</strong><strong>.</strong> Spowoduje to zastąpienie `CreateUserWizardStep` z równoważną `WizardStep` których znaczników definiuje interfejs użytkownika, który odpowiada `CreateUserWizardStep`"s. Konwertując `CreateUserWizardStep` do `WizardStep` możemy zmiana położenia kontrolki, lub Dodaj dodatkowe elementy interfejsu użytkownika do tego kroku. Aby przekonwertować `CreateUserWizardStep` lub `CompleteWizardStep` do edycji `WizardStep`, kliknij przycisk "Krok Dostosuj tworzenia użytkownika" lub "Dostosować wykonaj krok" link z tagu formantu.

- **Użyj kombinacji powyższych dwóch opcji.**

Jest istotnym aspektem należy pamiętać, że w formancie CreateUserWizard wykonuje procesu tworzenia konta użytkownika, po kliknięciu przycisku "Tworzenie użytkownika" z poziomu jego `CreateUserWizardStep`. Nie ma znaczenia, jeśli istnieją dodatkowe `WizardStep` s po `CreateUserWizardStep` lub nie.

Podczas dodawania niestandardowego `WizardStep` na formancie CreateUserWizard służąca do gromadzenia dodatkowych wprowadzania danych przez użytkownika niestandardowego `WizardStep` mogą być umieszczane przed lub po `CreateUserWizardStep`. Jeśli znajduje się przed `CreateUserWizardStep` następnie danych wejściowych użytkownika dodatkowe są zbierane z niestandardowego `WizardStep` jest dostępna dla `CreatedUser` obsługi zdarzeń. Jednak jeśli niestandardowa `WizardStep` po `CreateUserWizardStep` następnie przez czas niestandardowego `WizardStep` jest wyświetlany już utworzono nowe konto użytkownika i `CreatedUser` zdarzenie zostało już uruchomione.

21 rysunku przedstawiono przepływ pracy po dodany `WizardStep` poprzedza `CreateUserWizardStep`. Ponieważ informacje o dodatkowych użytkownika została pobrana przez czas `CreatedUser` aktualizacja jest generowane zdarzenie, wszystkie mamy czy `CreatedUser` obsługi zdarzeń, aby pobrać te dane wejściowe i ich użyć do `INSERT` wartości parametrów w instrukcji (zamiast `DBNull.Value`).


[![Przepływ pracy CreateUserWizard, gdy element CreateUserWizardStep poprzedza dodatkowe WizardStep](storing-additional-user-information-vb/_static/image62.png)](storing-additional-user-information-vb/_static/image61.png)

**Rysunek 21**: CreateUserWizard przepływu pracy podczas dodatkowe `WizardStep` Precedes `CreateUserWizardStep` ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](storing-additional-user-information-vb/_static/image63.png))


Jeśli niestandardowa `WizardStep` znajduje się *po* `CreateUserWizardStep`, jednak proces tworzenia konta użytkownika występuje, zanim użytkownik miał możliwość wprowadź jej Miasto macierzystego, strony głównej lub podpisu. W takim przypadku te informacje dodatkowe potrzebuje do wstawienia do bazy danych po utworzeniu konta użytkownika, jak pokazano na rysunku 22.


[![CreateUserWizard przepływu pracy, gdy po element CreateUserWizardStep dodatkowe WizardStep](storing-additional-user-information-vb/_static/image65.png)](storing-additional-user-information-vb/_static/image64.png)

**Rysunek 22**: CreateUserWizard przepływu pracy podczas dodatkowe `WizardStep` pochodzi po `CreateUserWizardStep` ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](storing-additional-user-information-vb/_static/image66.png))


Przepływ pracy pokazano na rysunku 22 czeka do wstawienia rekordu do `UserProfiles` tabeli dopiero po ukończeniu kroku 2. Obiekt odwiedzający zamknięcie przeglądarki jej po wykonaniu kroku 1, jednak firma Microsoft będzie osiągnięto stanu, w którym utworzono konto użytkownika, ale żaden rekord został dodany do `UserProfiles`. Jednym z rozwiązań ma rekord z `NULL` lub wstawione do wartości domyślnych `UserProfiles` w `CreatedUser` program obsługi zdarzeń (który uruchamia się po wykonaniu kroku 1), a następnie aktualizacji to rekord po ukończeniu kroku 2. Gwarantuje to, że `UserProfiles` rekord zostanie dodany do konta użytkownika nawet wtedy, gdy użytkownik zamyka połowie procesu rejestracji za pomocą.

W tym samouczku Utwórzmy nowy `WizardStep` występuje po `CreateUserWizardStep` lecz przed `CompleteWizardStep`. Pierwszy get WizardStep w umieścić i skonfigurowany, a następnie firma Microsoft będzie Przyjrzyjmy się kodu.

W formancie CreateUserWizard tagów inteligentnych, wybierz opcję "Dodaj lub usuń `WizardStep` s", co spowoduje uruchomienie `WizardStep` okno dialogowe Edytor kolekcji. Dodaj nową `WizardStep`, ustawienie jej `ID` do `UserSettings`, jego `Title` "Twoje ustawienia" i jego `StepType` do `Step`. Umieść go tak, aby po przejściu do `CreateUserWizardStep` ("zamówić Twoje nowe konto"), a przed `CompleteWizardStep` ("ukończone"), jak pokazano na rysunku 23.


[![Dodaj nowy element WizardStep na formancie CreateUserWizard](storing-additional-user-information-vb/_static/image68.png)](storing-additional-user-information-vb/_static/image67.png)

**Rysunek 23**: Dodaj nowy `WizardStep` na formancie CreateUserWizard ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](storing-additional-user-information-vb/_static/image69.png))


Kliknij przycisk OK, aby zamknąć `WizardStep` okno dialogowe Edytor kolekcji. Nowe `WizardStep` świadczy formancie CreateUserWizard zaktualizowane deklaratywne znaczników:

[!code-aspx[Main](storing-additional-user-information-vb/samples/sample13.aspx)]

Należy pamiętać, nowe `<asp:WizardStep>` elementu. Musimy dodać interfejs użytkownika do gromadzenia miejscowość macierzystego, strony głównej i podpis w tym miejscu nowego użytkownika. W składni deklaratywnej lub poprzez projektanta, można wprowadzić tej zawartości. Aby użyć projektanta, wybierz w kroku "Ustawień" z listy rozwijanej w tagu inteligentnego, aby zobaczyć krok w projektancie.

> [!NOTE]
> Wybranie kroku za pomocą listy rozwijanej tagów inteligentnych aktualizacji w formancie CreateUserWizard [ `ActiveStepIndex` właściwości](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.activestepindex.aspx), który określa indeks kroku początkowego. W związku z tym Jeśli używasz tej listy rozwijanej do edycji danego kroku "Ustawień" w projektancie, należy go ustawić ponownie na "Logowania w tym nowe konto", aby ten krok jest wyświetlany, jeśli najpierw odwiedzać użytkownicy `EnhancedCreateUserWizard.aspx` strony.


Tworzenie interfejsu użytkownika w ramach krok "Twoje ustawienia", który zawiera trzy kontrolki TextBox o nazwie `HomeTown`, `HomepageUrl`, i `Signature`. Po konstruowania ten interfejs, znaczników deklaratywne CreateUserWizard powinien wyglądać podobny do następującego:

[!code-aspx[Main](storing-additional-user-information-vb/samples/sample14.aspx)]

Przejdź dalej i stronie za pośrednictwem przeglądarki i utworzyć nowe konto użytkownika, określania wartości dla miejscowość macierzystego, strony głównej i podpis. Po zakończeniu `CreateUserWizardStep` utworzono konto użytkownika w ramach członkostwa i `CreatedUser` uruchamia program obsługi zdarzeń, które dodaje nowy wiersz do `UserProfiles`, ale z bazą danych `NULL` wartość `HomeTown`, `HomepageUrl`, i `Signature`. Wartości podane w domu miejscowość, strony głównej i podpis nigdy nie są używane. Wynikiem jest konto użytkownika z `UserProfiles` rekordów, których `HomeTown`, `HomepageUrl`, i `Signature` pól nie zostały jeszcze można określić.

Musimy wykonanie kodu po wykonaniu kroku "Twoje ustawienia", przyjmuje macierzystego miejscowość, honepage i podpis wartości wprowadzonej przez użytkownika, który aktualizuje odpowiednie `UserProfiles` rekordu. Kontrolowanie każdym razem, użytkownik przechodzi między krokami w kreatorze, Kreator [ `ActiveStepChanged` zdarzeń](https://msdn.microsoft.com/library/system.web.ui.webcontrols.wizard.activestepchanged.aspx) uruchamiany. Można utworzyć program obsługi zdarzeń dla tego zdarzenia i aktualizację `UserProfiles` tabeli po ukończeniu tego kroku "Ustawień".

Dodawanie obsługi zdarzeń dla CreateUserWizard `ActiveStepChanged` zdarzeń i Dodaj następujący kod:

[!code-vb[Main](storing-additional-user-information-vb/samples/sample15.vb)]

Powyższy kod uruchamia się za określenie, czy po prostu osiągnęliśmy kroku "Zakończ". Ponieważ krok "Zakończ" występuje natychmiast po wykonaniu kroku "Twoje ustawienia", następnie po osiągnięciu przez obiekt odwiedzający "Ukończone" krok oznaczający, że użytkownik właśnie zakończono kroku "Ustawień".

W takim przypadku należy programowo odwołują się do formantów TextBox w `UserSettings WizardStep`. Odbywa się przy pierwszym użyciu `FindControl` programowo odwołuje się do metody `UserSettings WizardStep`, a następnie ponownie, aby odwołać się do pól tekstowych z poziomu `WizardStep`. Po odwołaniu pól tekstowych już wszystko gotowe do wykonania `UPDATE` instrukcji. `UPDATE` Instrukcji ma taką samą liczbę parametrów jako `INSERT` instrukcji w `CreatedUser` program obsługi zdarzeń, ale w tym miejscu używamy domową miejscowość, strony głównej i podpis wartości podane przez użytkownika.

Z tej obsługi zdarzeń w miejscu, odwiedź stronę `EnhancedCreateUserWizard.aspx` stronie za pośrednictwem przeglądarki, a następnie utwórz nowe konto użytkownika, określania wartości dla miejscowość macierzystego, strony głównej i podpis. Po utworzeniu nowego konta powinno nastąpić przekierowanie do `AdditionalUserInfo.aspx` strony, gdzie dokładnie wprowadzane macierzystego miejscowość, strony głównej i podpis informacje są wyświetlane.

> [!NOTE]
> Nasze witryny sieci Web ma obecnie dwie strony, z których użytkownik może utworzyć nowe konto: `CreatingUserAccounts.aspx` i `EnhancedCreateUserWizard.aspx`. Mapy witryny sieci Web i strony logowania, wskaż `CreatingUserAccounts.aspx` strony, ale `CreatingUserAccounts.aspx` strony nie Monituj użytkownika o ich macierzystego miejscowość, strony głównej i podpis informacji i nie dodaje odpowiedni wiersz do `UserProfiles`. W związku z tym zaktualizować `CreatingUserAccounts.aspx` strony, dzięki czemu oferuje tej funkcji lub aktualizowanie strony mapy witryny i zaloguj się do odwołania `EnhancedCreateUserWizard.aspx` zamiast `CreatingUserAccounts.aspx`. Jeśli wybierzesz tę druga opcję należy zaktualizować `Membership` folderu `Web.config` pliku tak, aby zezwolić na dostęp do użytkowników anonimowych `EnhancedCreateUserWizard.aspx` strony.


## <a name="summary"></a>Podsumowanie

W tym samouczku analizujemy techniki modelowania danych, które jest powiązane z kont użytkowników w ramach członkostwa. W szczególności analizujemy modelowania podmiotom udostępniać relacji jeden do wielu kont użytkowników, a także dane, które współużytkują relacją. Ponadto widzieliśmy, jak to powiązane informacje można są wyświetlane, wstawiane i aktualizowane z przykładami za pomocą formantu SqlDataSource i inne przy użyciu kodu ADO.NET.

W tym samouczku wykonuje naszych przyjrzeć się konta użytkowników. Począwszy od w następnym samouczku będziemy spowoduje wyłączenie wymagające uwagi do ról. W następnych kilku samouczkach przedstawiono w ramach ról Zobacz Tworzenie nowych ról sposobu przypisywania ról użytkowników, w jaki sposób można określić, jakie role użytkownika należy do oraz w jaki sposób ma być stosowana autoryzacja oparta na rolach.

Programowanie przyjemność!

### <a name="further-reading"></a>Dalsze informacje

Więcej informacji dotyczących tematów omówionych w tym samouczku można znaleźć w następujących zasobach:

- [Uzyskiwanie dostępu i aktualizowanie danych w programie ASP.NET 2.0](http://aspnet.4guysfromrolla.com/articles/011106-1.aspx)
- [Formant ASP.NET 2.0 Kreatora](https://weblogs.asp.net/scottgu/archive/2006/02/21/438732.aspx)
- [Tworzenie za pomocą programu ASP.NET 2.0 Kreator formantu interfejsu użytkownika krok po kroku](http://aspnet.4guysfromrolla.com/articles/061406-1.aspx)
- [Tworzenie parametrów sterujących niestandardowe źródło danych](http://aspnet.4guysfromrolla.com/articles/110106-1.aspx)
- [Dostosowywanie formancie CreateUserWizard](http://aspnet.4guysfromrolla.com/articles/070506-1.aspx)
- [Przewodniki Szybki Start formantu widoku DetailsView](https://quickstarts.asp.net/QuickStartv20/aspnet/doc/ctrlref/data/detailsview.aspx)
- [Wyświetlanie danych za pomocą formantu ListView](http://aspnet.4guysfromrolla.com/articles/122607-1.aspx)
- [Pincety formanty walidacji w programie ASP.NET 2.0](http://aspnet.4guysfromrolla.com/articles/112305-1.aspx)
- [Wstaw edytowanie i usuwanie danych](../../data-access/editing-inserting-and-deleting-data/an-overview-of-inserting-updating-and-deleting-data-cs.md)
- [Sprawdzanie poprawności formularz w programie ASP.NET](http://www.4guysfromrolla.com/webtech/090200-1.shtml)
- [Zbieranie informacji o rejestracji użytkownika niestandardowego](https://weblogs.asp.net/scottgu/archive/2006/07/05/Tip_2F00_Trick_3A00_-Gathering-Custom-User-Registration-Information.aspx)
- [Profilów w programie ASP.NET 2.0](http://www.odetocode.com/Articles/440.aspx)
- [Formant asp: ListView](https://weblogs.asp.net/scottgu/archive/2007/08/10/the-asp-listview-control-part-1-building-a-product-listing-page-with-clean-css-ui.aspx)
- [Szybki Start profilów użytkownika](https://quickstarts.asp.net/QuickStartv20/aspnet/doc/profile/default.aspx)

### <a name="about-the-author"></a>Informacje o autorze

Scott Bento, Utwórz wiele książek ASP/ASP.NET i twórcę 4GuysFromRolla.com, pracuje z technologii Microsoft Web od 1998. Scott działa jako niezależnego konsultanta trainer i składnika zapisywania. Jest jego najnowszej książki  *[Sams nauczyć się ASP.NET 2.0 w ciągu 24 godzin](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)*. Scott jest osiągalny w [ mitchell@4guysfromrolla.com ](mailto:mitchell@4guysfromrolla.com) lub za pośrednictwem jego blogu w [ http://ScottOnWriting.NET ](http://scottonwriting.net/).

### <a name="special-thanks-to"></a>Specjalne podziękowania dla...

Ten samouczek serii zostało sprawdzone przez wiele recenzentów przydatne. Zainteresowani recenzowania Moje nadchodzących artykuły MSDN? Jeśli tak, Porzuć mnie linii w [ mitchell@4GuysFromRolla.com ](mailto:mitchell@4GuysFromRolla.com).

> [!div class="step-by-step"]
> [Poprzednie](user-based-authorization-vb.md)
