---
uid: web-forms/overview/older-versions-security/membership/storing-additional-user-information-cs
title: Przechowywanie dodatkowych informacji dotyczących użytkowników (C#) | Dokumentacja firmy Microsoft
author: rick-anderson
description: W tym samouczku odpowiemy na to pytanie, tworząc aplikację bardzo podstawowe księgi gości. W ten sposób przyjrzymy się różnych opcji dla modeli...
ms.author: riande
ms.date: 01/18/2008
ms.assetid: 1642132a-1ca5-4872-983f-ab59fc8865d3
msc.legacyurl: /web-forms/overview/older-versions-security/membership/storing-additional-user-information-cs
msc.type: authoredcontent
ms.openlocfilehash: 76e6cd1ec290cf572023aef35e349b1146b2b432
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/16/2018
ms.locfileid: "41757147"
---
<a name="storing-additional-user-information-c"></a>Przechowywanie dodatkowych informacji dotyczących użytkowników (C#)
====================
przez [Bento Scott](https://twitter.com/ScottOnWriting)

[Pobierz program Code](http://download.microsoft.com/download/3/f/5/3f5a8605-c526-4b34-b3fd-a34167117633/ASPNET_Security_Tutorial_08_CS.zip) lub [Pobierz plik PDF](http://download.microsoft.com/download/3/f/5/3f5a8605-c526-4b34-b3fd-a34167117633/aspnet_tutorial08_ExtraUserInfo_cs.pdf)

> W tym samouczku odpowiemy na to pytanie, tworząc aplikację bardzo podstawowe księgi gości. W ten sposób firma Microsoft będzie Przyjrzyj się różne opcje modelowania informacje o użytkowniku w bazie danych, a następnie zobacz, jak skojarzyć te dane z konta użytkowników utworzone przez platformę członkostwa.


## <a name="introduction"></a>Wprowadzenie

ASP. Framework członkostwo firmy NET oferuje elastyczny interfejs do zarządzania użytkownikami. Interfejs API członkostwa obejmuje metody sprawdzania poprawności poświadczeń, odzyskiwania informacji o aktualnie zalogowanego użytkownika, tworzenie nowego konta użytkownika i usunięcie konta użytkownika, między innymi. Każde konto użytkownika w ramach członkostwa zawiera tylko właściwości potrzebne dla sprawdzanie poprawności poświadczeń i wykonywanie zadań związanych z kontem użytkownika niezbędne. Jest to dowodem metod i właściwości [ `MembershipUser` klasy](https://msdn.microsoft.com/library/system.web.security.membershipuser.aspx), który modeluje konta użytkownika w ramach członkostwa. Ta klasa posiada właściwości, takie jak [ `UserName` ](https://msdn.microsoft.com/library/system.web.security.membershipuser.username.aspx), [ `Email` ](https://msdn.microsoft.com/library/system.web.security.membershipuser.email.aspx), i [ `IsLockedOut` ](https://msdn.microsoft.com/library/system.web.security.membershipuser.islockedout.aspx), i metod, takich jak [ `GetPassword` ](https://msdn.microsoft.com/library/system.web.security.membershipuser.getpassword.aspx) i [ `UnlockUser` ](https://msdn.microsoft.com/library/system.web.security.membershipuser.unlockuser.aspx).

Aplikacje muszą się często, do przechowywania dodatkowych informacji dotyczących użytkowników nie są uwzględnione w ramach członkostwa. Na przykład sklepie internetowym może podać tę informację każdy użytkownik, przechowywać swoje adresy wysyłki i rozliczeń, informacje o płatności, preferencje dostarczania, a numer telefonu kontaktowego. Ponadto każde zamówienie w systemie jest skojarzony z określonego konta użytkownika.

`MembershipUser` Klasa nie ma właściwości, takie jak `PhoneNumber` lub `DeliveryPreferences` lub `PastOrders`. W jaki sposób firma Microsoft śledzić informacje o użytkowniku, wymagane przez aplikację i jego Integracja z usługą w ramach członkostwa? W tym samouczku odpowiemy na to pytanie, tworząc aplikację bardzo podstawowe księgi gości. W ten sposób firma Microsoft będzie Przyjrzyj się różne opcje modelowania informacje o użytkowniku w bazie danych, a następnie zobacz, jak skojarzyć te dane z konta użytkowników utworzone przez platformę członkostwa. Zaczynajmy!

## <a name="step-1-creating-the-guestbook-applications-data-model"></a>Krok 1: Tworzenie modelu danych aplikacji księgi gości

Istnieją różne techniki, które można zastosować do przechwytywania informacji o użytkowniku w bazie danych i skojarzyć go z konta użytkowników utworzone przez platformę członkostwa. Aby zilustrować tych metod, potrzebujemy Wzbogać samouczek sieci web tak, aby przechwytuje jakieś dane dotyczące użytkowników. (Obecnie aplikacji model danych zawiera tylko tabele wymagane przez usługi aplikacji `SqlMembershipProvider`.)

Utwórzmy aplikacji bardzo proste księgi gości, gdy uwierzytelniony użytkownik może komentarz. Oprócz przechowywania komentarzy księgi gości, zezwolimy na każdego użytkownika, do przechowywania jego macierzystego miejscowości, strony głównej i podpis. Jeśli podano użytkownika głównego miejscowości, strony głównej i podpis pojawi się na każdy komunikat jest on niedostępny w księdze gości.

### <a name="adding-theguestbookcommentstable"></a>Dodawanie`GuestbookComments`tabeli

Aby przechwycić komentarze księgi gości, należy utworzyć tabelę bazy danych o nazwie `GuestbookComments` zawierający kolumny, takie jak `CommentId`, `Subject`, `Body`, i `CommentDate`. Firma Microsoft musi być także każdy rekord `GuestbookComments` odwołania do tabeli użytkownika, który wystawił komentarz.

Aby dodać w tej tabeli do naszych bazy danych, przejdź do Eksploratora bazy danych w programie Visual Studio i przejść do szczegółów `SecurityTutorials` bazy danych. Kliknij prawym przyciskiem myszy w folderze tabel i wybierz polecenie Dodaj nową tabelę. Wywołuje interfejs, który pozwala na określenie kolumny w nowej tabeli.


[![Dodaj nową tabelę w bazie danych SecurityTutorials](storing-additional-user-information-cs/_static/image2.png)](storing-additional-user-information-cs/_static/image1.png)

**Rysunek 1**: Dodawanie nowej tabeli do `SecurityTutorials` bazy danych ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](storing-additional-user-information-cs/_static/image3.png))


Następnie zdefiniuj `GuestbookComments`firmy kolumn. Rozpocznij, dodając kolumnę o nazwie `CommentId` typu `uniqueidentifier`. Ta kolumna będzie jednoznacznie identyfikują każdego komentarza w księdze gości, więc nie zezwalaj na `NULL` s i oznacz go jako klucza podstawowego tabeli. Zamiast podanie wartości dla `CommentId` pola w każdym `INSERT`, firma Microsoft może wskazać, że nowy `uniqueidentifier` wartości powinny być generowane automatycznie dla tego pola na `INSERT` ustawiając wartość domyślna w kolumnie `NEWID()`. Po dodaniu tego pierwszego pola, oznaczając je jako klucz podstawowy i ustawienia jej wartości domyślnej ekran powinien wyglądać podobnie do ekranu zrzut, jak pokazano na rysunku 2.


[![Dodaj kolumnę głównej o nazwie CommentId](storing-additional-user-information-cs/_static/image5.png)](storing-additional-user-information-cs/_static/image4.png)

**Rysunek 2**: Dodawanie podstawowej kolumny o nazwie `CommentId` ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](storing-additional-user-information-cs/_static/image6.png))


Następnie dodaj kolumnę o nazwie `Subject` typu `nvarchar(50)` i kolumnę o nazwie `Body` typu `nvarchar(MAX)`, niezezwalające `NULL` s w tych kolumnach. Poniżej Dodaj kolumnę o nazwie `CommentDate` typu `datetime`. Nie zezwalaj na `NULL` s i ustaw `CommentDate` wartości domyślnej kolumny do `getdate()`.

Pozostaje tylko można dodać kolumny, które kojarzy konto użytkownika z każdego komentarza księgi gości. Jedną z opcji byłoby Dodaj kolumnę o nazwie `UserName` typu `nvarchar(256)`. Jest to odpowiedni wybór, gdy inne niż przy użyciu dostawcy członkostwa `SqlMembershipProvider`. Ale w przypadku, gdy za pomocą `SqlMembershipProvider`, jak możemy w tej serii samouczków `UserName` kolumny w `aspnet_Users` tabeli nie musi być unikatowy. `aspnet_Users` Jest klucz podstawowy tabeli `UserId` i jest typu `uniqueidentifier`. W związku z tym `GuestbookComments` tabela musi kolumnę o nazwie `UserId` typu `uniqueidentifier` (Brak zezwolenia `NULL` wartości). Przejdź dalej i Dodaj tę kolumnę.

> [!NOTE]
> Tak jak Omówiliśmy to w [ *tworzenie schematu członkostwa w programie SQL Server* ](creating-the-membership-schema-in-sql-server-cs.md) samouczek, w ramach członkostwa zaprojektowana w celu umożliwienia wielu aplikacji sieci web przy użyciu różnych kont użytkowników można współużytkować ten sam Magazyn użytkowników. Robi to za pomocą partycjonowania kont użytkowników w różnych aplikacjach. I gdy każda nazwa użytkownika jest musi być unikatowy w obrębie aplikacji, nazwę użytkownika mogą być używane w innych aplikacjach, przy użyciu tego samego magazynu użytkownika. Brak złożonego `UNIQUE` ograniczenie w `aspnet_Users` tabeli na `UserName` i `ApplicationId` pola, ale nie jeden na tylko `UserName` pola. W związku z tym, istnieje możliwość aspnet\_użytkowników tabeli rekordów dwie (lub więcej) z takimi samymi `UserName` wartość. Jest jednak `UNIQUE` ograniczenie `aspnet_Users` tabeli `UserId` pola (ponieważ jest to klucz podstawowy). A `UNIQUE` ważne jest ograniczenie, ponieważ bez niego firma Microsoft nie może ustanowić ograniczenie klucza obcego między `GuestbookComments` i `aspnet_Users` tabel.


Po dodaniu `UserId` kolumny, Zapisz tabelę, klikając ikonę Zapisz na pasku narzędzi. Nadaj nazwę nowej tabeli `GuestbookComments`.

Mamy jeden problem ostatniego zająć się za pomocą `GuestbookComments` tabeli: musimy utworzyć [ograniczenie klucza obcego](https://msdn.microsoft.com/library/ms175464.aspx) między `GuestbookComments.UserId` kolumny i `aspnet_Users.UserId` kolumny. Aby to osiągnąć, kliknij ikonę relacji na pasku narzędzi, aby uruchomić okno dialogowe relacje klucza obcego. (Ewentualnie można uruchomić tego okna dialogowego, przechodząc do menu projektanta tabel i wybierając pozycję relacje.)

Kliknij przycisk Dodaj, w lewym dolnym rogu okna dialogowego relacje klucza obcego. Spowoduje to dodanie nowego ograniczenia klucza obcego, mimo że wciąż potrzebujemy do definiowania tabel, które uczestniczą w relacji.


[![Umożliwia zarządzanie ograniczeń klucza obcego z tabeli przez okno dialogowe relacje klucza obcego](storing-additional-user-information-cs/_static/image8.png)](storing-additional-user-information-cs/_static/image7.png)

**Rysunek 3**: okno dialogowe relacje klucza obcego umożliwia zarządzanie ograniczeń klucza obcego dla tabeli ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](storing-additional-user-information-cs/_static/image9.png))


Następnie kliknij ikonę elipsy, w wierszu "Tabel i kolumn specyfikacji" po prawej stronie. Spowoduje to uruchomienie okno dialogowe tabele i kolumny, z której można określić tabeli klucza podstawowego i kolumny i kolumny klucza obcego z `GuestbookComments` tabeli. W szczególności należy wybrać `aspnet_Users` i `UserId` jako tabeli klucza podstawowego i kolumny, a `UserId` z `GuestbookComments` tabeli jako kolumna klucza obcego (zobacz rysunek 4). Po zdefiniowaniu podstawowe i obce klucza tabele i kolumny, kliknij przycisk OK, aby powrócić do okna dialogowego relacje klucza obcego.


[![Ustanów i obcego klucza ograniczenie między aspnet_Users GuesbookComments tabel](storing-additional-user-information-cs/_static/image11.png)](storing-additional-user-information-cs/_static/image10.png)

**Rysunek 4**: ustanowić obcego klucza ograniczenie między `aspnet_Users` i `GuesbookComments` tabel ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](storing-additional-user-information-cs/_static/image12.png))


W tym momencie została ustanowiona ograniczenie klucza obcego. Obecność tego ograniczenia zapewnia [relacyjna integralność](http://en.wikipedia.org/wiki/Referential_integrity) między dwiema tabelami, gwarantując, że nigdy nie będzie wpisu księgi gości, odnoszące się do konta użytkownika nie istnieje. Domyślnie ograniczenie klucza obcego uniemożliwi nadrzędnego rekordu do usunięcia, jeśli istnieją odpowiednie rekordy podrzędne. Oznacza to jeśli użytkownika sprawia, że co najmniej jeden komentarz księgi gości, a następnie próby usunięcia konta użytkownika, Usuń zakończy się niepowodzeniem, chyba że najpierw zostaną usunięte swoje komentarze księgi gości.

Ograniczenia klucza obcego można skonfigurować do automatycznego usuwania rekordów podrzędnych, po usunięciu rekordu nadrzędnego. Innymi słowy firma Microsoft można skonfigurować tego ograniczenia klucza obcego tak, aby wpisów księgi gości są automatycznie usuwane po usunięciu jego konta użytkownika. Aby to osiągnąć, rozwiń sekcję "INSERT i UPDATE Specyfikacja" i ustawić dla właściwości "Usuń regułę" Cascade.


[![Skonfiguruj ograniczenia klucza obcego się kaskadowo](storing-additional-user-information-cs/_static/image14.png)](storing-additional-user-information-cs/_static/image13.png)

**Rysunek 5**: Konfigurowanie ograniczenie klucza obcego w celu kaskadowe ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](storing-additional-user-information-cs/_static/image15.png))


Aby zapisać ograniczenie klucza obcego, kliknij przycisk Zamknij, aby wyjść poza relacje klucza obcego. Następnie kliknij przycisk Zapisz w pasku narzędzi, aby zapisać w tabeli i tej relacji.

### <a name="storing-the-users-home-town-homepage-and-signature"></a>Przechowywanie Scootney, strony głównej i podpis użytkownika

`GuestbookComments` Tabeli przedstawiono sposób przechowywania informacji, które udostępnia relacji jeden do wielu kont użytkowników. Ponieważ każde konto użytkownika może mieć dowolną liczbę skojarzonych z nimi komentarzy, ta relacja jest modelowane, tworząc tabelę zawierającą zestaw komentarze, który zawiera kolumny z linkiem każdego komentarza do konkretnego użytkownika. Korzystając z `SqlMembershipProvider`, tworząc kolumnę o nazwie ustanowieniu tego połączenia będzie najlepiej `UserId` typu `uniqueidentifier` i ograniczenie klucza obcego między tej kolumny i `aspnet_Users.UserId`.

Teraz musisz skojarzyć trzy kolumny z każdego konta użytkownika, do przechowywania głównego miejscowości, strony głównej i podpisu, który pojawi się w komentarzach księgi gości do jego użytkownika. Istnieją liczby różnych sposobów, w tym celu:

- <strong>Dodaj nowe kolumny w celu</strong><strong>`aspnet_Users`</strong><strong>lub</strong><strong>`aspnet_Membership`</strong><strong>tabel.</strong> Nie będzie najlepiej to podejście ponieważ modyfikuje Schemat używany przez `SqlMembershipProvider`. Ta decyzja może wrócić do mogły spowodować szkód w dół po drodze. Na przykład, co w przypadku przyszłych wersjach programu ASP.NET używa innego `SqlMembershipProvider` schematu. Microsoft może obejmować narzędzia do migracji programu ASP.NET 2.0 `SqlMembershipProvider` danych do nowego schematu, ale jeśli zmodyfikowano ASP.NET 2.0 `SqlMembershipProvider` schematu, taka Konwersja może nie być możliwe.

- **Użyj ASP. Struktura profilu firmy NET, definiowanie właściwości profilu dla głównego miejscowości, strony głównej i podpis.** Program ASP.NET zawiera strukturę profilu, która służy do przechowywania dodatkowych danych specyficzne dla użytkownika. Jak w ramach członkostwa w ramach profilu jest kompilowanych na podstawie modelu dostawców. .NET Framework, który jest dostarczany z `SqlProfileProvider` sthat dane profilu są przechowywane w bazie danych programu SQL Server. W rzeczywistości w naszej bazie danych ma już tabeli używanej przez `SqlProfileProvider` (`aspnet_Profile`), jak zostało dodane, gdy dodaliśmy usługi aplikacji z powrotem w <a id="_msoanchor_2"> </a> [ *tworzenie schematu członkostwa w programie SQL Serwer* ](creating-the-membership-schema-in-sql-server-cs.md) samouczka.   
  Główną zaletą framework profilu jest możliwość dla deweloperów zdefiniować właściwości profilu w `Web.config` — żaden kod nie musi być napisany do serializowania danych profilu do i z magazynu danych. Krótko mówiąc jest bardzo łatwe do definiowania zestawu właściwości profilu i pracować z nimi w kodzie. Jednak system profilu pozostawia znacznie pożądane, jeśli chodzi o przechowywanie wersji, więc jeśli masz aplikację, których oczekujesz, że nowe właściwości specyficzne dla użytkownika mają zostać dodane w późniejszym czasie lub istniejące ma zostać usunięta lub zmodyfikowana, a następnie w ramach profilu nie może być  najlepszym rozwiązaniem. Ponadto `SqlProfileProvider` właściwości profilu są przechowywane w wysokim stopniu zdenormalizowane sposób, uniemożliwiając dalej na do uruchamiania zapytań bezpośrednio w odniesieniu do danych profilu (na przykład, jak wielu użytkowników korzysta z miastem głównego elementu New York).   
  Aby uzyskać więcej informacji w ramach profilu zapoznaj się w sekcji "Dalsze odczytów" na końcu tego samouczka.

- <strong>Dodaj te trzy kolumny do nowej tabeli w bazie danych i ustanowić relację jeden do jednego między tej tabeli i</strong><strong>`aspnet_Users`</strong><strong>.</strong> Takie podejście wymaga nieco więcej pracy niż przy użyciu framework profilu, ale zapewnia maksymalną elastyczność jak właściwości dodatkowych użytkowników są modelowane w bazie danych. Jest to opcja, które zostaną użyte w tym samouczku.

Utworzymy nową tabelę o nazwie `UserProfiles` można zapisać macierzystego miejscowości, strony głównej i podpisu dla każdego użytkownika. Kliknij prawym przyciskiem myszy w folderze tabelami w oknie Eksplorator bazy danych i wybrać opcję utworzenia nowej tabeli. Nazwa pierwszej kolumny `UserId` i ustaw jej typ `uniqueidentifier`. Nie zezwalaj na `NULL` wartości i oznacz kolumnę jako klucz podstawowy. Następnie dodaj kolumn o nazwach: `HomeTown` typu `nvarchar(50)`; `HomepageUrl` typu `nvarchar(100)`; i podpis typu `nvarchar(500)`. Każda z tych trzech kolumn może akceptować `NULL` wartość.


[![Utwórz tabelę UserProfiles](storing-additional-user-information-cs/_static/image17.png)](storing-additional-user-information-cs/_static/image16.png)

**Rysunek 6**: tworzenie `UserProfiles` tabeli ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](storing-additional-user-information-cs/_static/image18.png))


Zapisz tabelę i nadaj mu nazwę `UserProfiles`. Na koniec ustanowić ograniczenie klucza obcego między `UserProfiles` tabeli `UserId` pola i `aspnet_Users.UserId` pola. Ile My mieliśmy z ograniczenie klucza obcego między `GuestbookComments` i `aspnet_Users` tabele, mają to ograniczenie kaskadowo usuwa. Ponieważ `UserId` pole `UserProfiles` jest serwerem podstawowym klucza, gwarantuje to, że będą istnieć nie więcej niż jednego rekordu w `UserProfiles` tabeli dla każdego konta użytkownika. Ten typ relacji jest określany jako jeden do jednego.

Teraz, gdy model danych utworzone, jesteśmy gotowi do jej używania. W krokach 2 i 3 przedstawiony zostanie sposób wyświetlić i edytować macierzystego miejscowości, strony głównej i podpis informacjami o aktualnie zalogowanego użytkownika. W kroku 4 zostanie utworzony interfejs dla uwierzytelnionych użytkowników przesłać nowe komentarze do księgi gości i wyświetlać już istniejące.

## <a name="step-2-displaying-the-users-home-town-homepage-and-signature"></a>Krok 2: Wyświetlanie Scootney, strony głównej i podpis użytkownika

Istnieją różne sposoby, aby zezwolić na aktualnie zalogowanego użytkownika wyświetlić i edytować jego macierzystego informacje miejscowości, strony głównej i podpis. Firma Microsoft może ręczne tworzenie interfejsu użytkownika przy użyciu pole tekstowe i formanty etykiet lub firma mogą użyć jednego z danych kontrolki sieci Web, takich jak kontrolce DetailsView. Do wykonania w bazie danych `SELECT` i `UPDATE` instrukcji napiszemy ADO.NET kod w klasie CodeBehind naszą stronę lub też zatrudniać podejścia deklaratywnego z kontrolką SqlDataSource. Najlepiej naszej aplikacji będzie zawierać architektury warstwowej, firma Microsoft może albo wywołać programowo z klasy CodeBehind strony, lub deklaratywne za pomocą kontrolki ObjectDataSource.

Ponieważ w tej serii samouczków skupia się na uwierzytelnianie formularzy, autoryzacji, konta użytkowników i ról, nie będzie dogłębną dyskusję te opcje dostępu do danych lub dlaczego architekturę warstwową jest preferowany w porównaniu do wykonywania instrukcji SQL bezpośrednio na stronie ASP.NET. Zamierzam wzięte, przy użyciu DetailsView i użyciu kontrolki SqlDataSource — opcja najszybszym i najprostszym —, ale kwestie omówione bez obaw można zastosować do alternatywnych sieci Web formanty i danych logika do dostępu. Aby uzyskać więcej informacji na temat pracy z danymi w programie ASP.NET: Zobacz Mój *[Praca z danymi w programie ASP.NET 2.0](../../data-access/index.md)* serii samouczków.

Otwórz `AdditionalUserInfo.aspx` strony w `Membership` folder i dodać kontrolki widoku szczegółów do strony, ustawiając jego `ID` właściwości `UserProfile` i wyczyszczenie jego `Width` i `Height` właściwości. Rozwiń DetailsView tagu inteligentnego, a następnie wybierz powiązać formant źródła danych. Spowoduje to uruchomienie Kreatora konfiguracji źródła danych (zobacz rysunek 7). Pierwszym krokiem pyta, aby określić typ źródła danych. Ponieważ firma Microsoft zamierza łącz się bezpośrednio z `SecurityTutorials` bazy danych, wybierz ikonę bazy danych, określając `ID` jako `UserProfileDataSource`.


[![Dodaj kontrolkę kontrolką SqlDataSource o nazwie UserProfileDataSource](storing-additional-user-information-cs/_static/image20.png)](storing-additional-user-information-cs/_static/image19.png)

**Rysunek 7**: Dodaj nowe kontrolki SqlDataSource, o nazwie `UserProfileDataSource` ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](storing-additional-user-information-cs/_static/image21.png))


Następny ekran wyświetla monit dotyczący bazy danych do użycia. Firma Microsoft już zdefiniowane parametry połączenia w `Web.config` dla `SecurityTutorials` bazy danych. Ta nazwa parametrów połączenia — `SecurityTutorialsConnectionString` — powinien znajdować się na liście rozwijanej. Wybierz tę opcję, a następnie kliknij przycisk Dalej.


[![Z listy rozwijanej wybierz SecurityTutorialsConnectionString](storing-additional-user-information-cs/_static/image23.png)](storing-additional-user-information-cs/_static/image22.png)

**Rysunek 8**: Wybierz `SecurityTutorialsConnectionString` z listy rozwijanej ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](storing-additional-user-information-cs/_static/image24.png))


Kolejne ekranu pyta, czy nam określić tabeli i kolumn do zapytań. Wybierz `UserProfiles` tabeli z listy rozwijanej i zaznacz wszystkie kolumny.


[![Przenieś kopii wszystkich kolumn z tabeli UserProfiles](storing-additional-user-information-cs/_static/image26.png)](storing-additional-user-information-cs/_static/image25.png)

**Rysunek 9**: Przenieś ponownie wszystkich kolumn z `UserProfiles` tabeli ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](storing-additional-user-information-cs/_static/image27.png))


Bieżące zapytanie zwraca rysunek 9 *wszystkich* rekordów w `UserProfiles`, ale firma Microsoft jest zainteresowany wyłącznie rekordu aktualnie zalogowanego użytkownika. Aby dodać `WHERE` klauzuli kliknij `WHERE` przycisk, aby wyświetlić Dodaj `WHERE` klauzuli dialogowego (zobacz rysunek 10). W tym miejscu można wybrać kolumny do filtrowania, operator i źródło parametru filtru. Wybierz `UserId` jako kolumny i "=" jako operatora.

Niestety nie istnieje źródło wbudowanych parametru do zwrócenia aktualnie zalogowanego użytkownika `UserId` wartość. Konieczne będzie programowo uzyskać tę wartość. W związku z tym Ustaw listy rozwijanej źródła na "None," kliknij pozycję Dodaj przycisk, aby dodać parametr, a następnie kliknij przycisk OK.


[![Dodaj parametr filtru w kolumnie Nazwa użytkownika](storing-additional-user-information-cs/_static/image29.png)](storing-additional-user-information-cs/_static/image28.png)

**Na rysunku nr 10**: Dodaj parametr filtru na `UserId` kolumny ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](storing-additional-user-information-cs/_static/image30.png))


Po kliknięciu przycisku OK nastąpi powrót do ekranu pokazano na rysunku 9. Tym razem jednak zapytanie SQL w dolnej części ekranu powinna zawierać `WHERE` klauzuli. Kliknij przycisk Dalej, aby przejść do ekranu "Testuj zapytanie". W tym miejscu można uruchomić zapytanie i wyświetlić wyniki. Kliknij przycisk Zakończ, aby zakończyć działanie kreatora.

Po ukończeniu pracy Kreatora konfiguracji źródła danych, program Visual Studio tworzy kontrolki SqlDataSource na podstawie ustawień określonych w kreatorze. Ponadto, ręcznie dodaje BoundFields do DetailsView dla każdej kolumny zwracane przez SqlDataSource `SelectCommand`. Nie ma potrzeby pokazanie `UserId` pole DetailsView, ponieważ użytkownik nie musi wiedzieć, ta wartość. Możesz usunąć to pole bezpośrednio w kontrolce DetailsView oznaczeniu deklaracyjnym lub przez kliknięcie przycisku "Edytuj pola" link z jego tagu inteligentnego.

W tym momencie oznaczeniu deklaracyjnym strony powinien wyglądać podobnie do poniższej:

[!code-aspx[Main](storing-additional-user-information-cs/samples/sample1.aspx)]

Potrzebujemy programowo ustawić kontrolki SqlDataSource `UserId` parametr aktualnie zalogowanego użytkownika `UserId` przed danych jest zaznaczone. Można to osiągnąć, tworząc program obsługi zdarzeń dla SqlDataSource `Selecting` zdarzenia i dodanie poniższego kodu istnieje:

[!code-csharp[Main](storing-additional-user-information-cs/samples/sample2.cs)]

Powyższy kod, który rozpoczyna się dzięki uzyskaniu odwołania do aktualnie zalogowanego użytkownika, wywołując `Membership` klasy `GetUser` metody. Spowoduje to zwrócenie `MembershipUser` obiektu, którego `ProviderUserKey` właściwość zawiera `UserId`. `UserId` Wartość jest następnie przypisywana do SqlDataSource `@UserId` parametru.

> [!NOTE]
> `Membership.GetUser()` Metoda zwraca informacje o aktualnie zalogowanego użytkownika. Jeśli użytkownik anonimowy odwiedzania strony, zwróci wartość `null`. W takim przypadku doprowadzi to do `NullReferenceException` na następujący wiersz kodu, podczas próby odczytu `ProviderUserKey` właściwości. Oczywiście nie mamy już martwić się o `Membership.GetUser()` zwracanie `null` wartość w `AdditionalUserInfo.aspx` strony, ponieważ został skonfigurowany Autoryzacja adresów URL w poprzednim samouczku, aby tylko uwierzytelnionym użytkownikom można uzyskać dostępu do zasobów platformy ASP.NET, w tym folderze. Jeśli potrzebujesz dostępu do informacji o aktualnie zalogowanego użytkownika na stronie, gdzie dostęp anonimowy jest dozwolone, upewnij się sprawdzić, czy innej niż`null MembershipUser` obiekt jest zwracany z `GetUser()` metoda przed odwołaniem się do jego właściwości.


W przypadku odwiedzenia `AdditionalUserInfo.aspx` strony za pośrednictwem przeglądarki zostanie wyświetlona strona puste, ponieważ trzeba jeszcze żadnych wierszy, aby dodać `UserProfiles` tabeli. W kroku 6 przedstawiony zostanie sposób dostosowywania kontroli CreateUserWizard do automatycznego dodawania nowego wiersza do `UserProfiles` tabeli po utworzeniu nowego konta użytkownika. Na razie jednak konieczne będzie ręczne tworzenie rekordu w tabeli.

Przejdź do Eksploratora bazy danych w programie Visual Studio, a następnie rozwiń folder tabel. Kliknij prawym przyciskiem myszy `aspnet_Users` tabeli i wybierz pozycję "Pokaż dane tabeli" Aby wyświetlić rekordy w tabeli; zrobić to samo `UserProfiles` tabeli. Na ilustracji 11 pokazano tych wyników, gdy sąsiadująco w pionie. W bazie danych istnieją obecnie `aspnet_Users` rekordy Bruce Fred i Tito, ale nie rekordów w `UserProfiles` tabeli.


[![Zawartość aspnet_Users i UserProfiles tabele są wyświetlane.](storing-additional-user-information-cs/_static/image32.png)](storing-additional-user-information-cs/_static/image31.png)

**Rysunek 11**: zawartość `aspnet_Users` i `UserProfiles` tabele są wyświetlane ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](storing-additional-user-information-cs/_static/image33.png))


Dodaj nowy rekord do `UserProfiles` tabeli ręcznie wpisując wartości dla `HomeTown`, `HomepageUrl`, i `Signature` pola. Najprostszym sposobem, aby uzyskać prawidłową `UserId` wartość w nowym `UserProfiles` rekordu jest wybranie `UserId` pola z określonego konta użytkownika w `aspnet_Users` tabeli i skopiuj i wklej go do `UserId` pole `UserProfiles`. Przedstawia rysunek 12 `UserProfiles` tabeli po dodaniu nowego rekordu dla Bruce.


[![Rekord został dodany do UserProfiles dla Bruce](storing-additional-user-information-cs/_static/image35.png)](storing-additional-user-information-cs/_static/image34.png)

**Rysunek 12**: dodano rekord A `UserProfiles` dla Bruce ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](storing-additional-user-information-cs/_static/image36.png))


Wróć do `AdditionalUserInfo.aspx` strony i zalogować się jako Bruce. Jak pokazano na rysunku 13, zostaną wyświetlone ustawienia Bruce firmy.


[![Obecnie odwiedzający użytkownik, jest wyświetlany jego ustawienia](storing-additional-user-information-cs/_static/image38.png)](storing-additional-user-information-cs/_static/image37.png)

**Rysunek 13**: obecnie odwiedzający użytkownika jest wyświetlana jego ustawienia ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](storing-additional-user-information-cs/_static/image39.png))


> [!NOTE]
> Przejdź dalej i ręcznie dodaj rekordy w `UserProfiles` tabeli dla każdego użytkownika członkostwa. W kroku 6 przedstawiony zostanie sposób dostosowywania kontroli CreateUserWizard do automatycznego dodawania nowego wiersza do `UserProfiles` tabeli po utworzeniu nowego konta użytkownika.


## <a name="step-3-allowing-the-user-to-edit-his-home-town-homepage-and-signature"></a>Krok 3: Umożliwienie użytkownikowi edytować jego Scootney, strony głównej i podpisu

W tym momencie aktualnie zalogowanego użytkownika można wyświetlić ich głównego miejscowości, strony głównej i ustawienie podpisu, ale jeszcze nie mogą ich modyfikować. Zaktualizujmy kontrolce DetailsView, dzięki czemu dane mogą być edytowane.

Najpierw musimy to dodanie `UpdateCommand` dla SqlDataSource, określając `UPDATE` instrukcję do wykonania i jego odpowiednich parametrów. Wybierz SqlDataSource, a następnie w oknie dialogowym właściwości kliknij wielokropek obok właściwości UpdateQuery, aby wyświetlić okno dialogowe Edytor poleceń i parametrów. Wprowadź następujące `UPDATE` instrukcji w polu tekstowym:

[!code-sql[Main](storing-additional-user-information-cs/samples/sample3.sql)]

Następnie kliknij przycisk "Odśwież parametry", który zostanie utworzony parametr w kontrolki SqlDataSource `UpdateParameters` kolekcji dla każdego z parametrów w `UPDATE` instrukcji. Pozostaw źródła dla każdego zestawu parametrów None, a następnie kliknij przycisk OK, aby wypełnić okno dialogowe.


[![Określ elementu UpdateCommand i UpdateParameters SqlDataSource](storing-additional-user-information-cs/_static/image41.png)](storing-additional-user-information-cs/_static/image40.png)

**Rysunek 14**: Określ SqlDataSource `UpdateCommand` i `UpdateParameters` ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](storing-additional-user-information-cs/_static/image42.png))


Ze względu na dodatki wprowadziliśmy do kontrolki SqlDataSource DetailsView kontroli może teraz obsługiwać edycji. W tagu inteligentnego DetailsView zaznacz pole wyboru "Włącz edytowanie". Spowoduje to dodanie CommandField formantu `Fields` kolekcji z jego `ShowEditButton` właściwość ustawioną na wartość True. Renderuje przycisk edycji, gdy DetailsView jest wyświetlany w trybie tylko do odczytu i aktualizacji i przyciski Anuluj po wyświetleniu w trybie edycji. Zamiast konieczności przez użytkownika, kliknij przycisk Edytuj, jednak firma Microsoft może mieć renderowania DetailsView w stanie "zawsze można edytować", ustawiając kontrolce DetailsView [ `DefaultMode` właściwość](https://msdn.microsoft.com/library/system.web.ui.webcontrols.detailsview.defaultmode.aspx) do `Edit`.

Za pomocą tych zmian swojej kontrolce DetailsView oznaczeniu deklaracyjnym powinien wyglądać podobnie do poniższej:

[!code-aspx[Main](storing-additional-user-information-cs/samples/sample4.aspx)]

Należy pamiętać, dodanie CommandField i `DefaultMode` właściwości.

Przejdź dalej i przetestować tę stronę za pośrednictwem przeglądarki. Gdy użytkownik odwiedzi z użytkownikiem, który ma odpowiedni rekord w `UserProfiles`, ustawienia użytkownika są wyświetlane w interfejsie można edytować.


[![DetailsView renderuje interfejsu można edytować](storing-additional-user-information-cs/_static/image44.png)](storing-additional-user-information-cs/_static/image43.png)

**Rysunek 15**: DetailsView renderuje interfejsu można edytować ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](storing-additional-user-information-cs/_static/image45.png))


Spróbuj zmianę wartości, a następnie klikając przycisk Aktualizuj. Wydaje się, jak gdyby nic się nie dzieje. Brak odświeżenie strony i wartości są zapisywane w bazie danych, ale nie ma żadnych wizualną opinię, który wystąpił podczas zapisywania.

Aby rozwiązać ten problem, wróć do programu Visual Studio i Dodaj kontrolkę typu etykieta powyżej DetailsView. Ustaw jego `ID` do `SettingsUpdatedMessage`, jego `Text` właściwość "Twoje ustawienia zostały zaktualizowane," i jego `Visible` i `EnableViewState` właściwości `false`.

[!code-aspx[Main](storing-additional-user-information-cs/samples/sample5.aspx)]

Potrzebujemy wyświetlić `SettingsUpdatedMessage` etykiety przy każdej aktualizacji DetailsView. W tym celu należy utworzyć procedurę obsługi zdarzeń dla DetailsView `ItemUpdated` zdarzeń i Dodaj następujący kod:

[!code-csharp[Main](storing-additional-user-information-cs/samples/sample6.cs)]

Wróć do `AdditionalUserInfo.aspx` strony za pośrednictwem przeglądarki i zaktualizować dane. Tym razem jest wyświetlany komunikat o stanie pomocne.


[![Krótką wiadomość jest wyświetlana podczas ustawienia zostały zaktualizowane](storing-additional-user-information-cs/_static/image47.png)](storing-additional-user-information-cs/_static/image46.png)

**Rysunek 16**: krótką wiadomość jest wyświetlana, gdy ustawienia są aktualne ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](storing-additional-user-information-cs/_static/image48.png))


> [!NOTE]
> W kontrolce DetailsView przez edytowanie pozostawia interfejsu znacznie być wskazane. Używa ona standardowych wymiarach pól tekstowych, ale pole podpisu powinien być najprawdopodobniej wielowierszowym polu tekstowym. RegularExpressionValidator należy używać, aby upewnić się, że adres URL strony głównej, jeśli wprowadzono, rozpoczyna się od "http://" lub "https://". Ponadto, ponieważ DetailsView formantem i jego `DefaultMode` ustawioną na `Edit`, przycisk Anuluj nie działa. Jego powinny albo zostać usunięte, lub po kliknięciu przekierowanie użytkownika do innej strony (takie jak `~/Default.aspx`). Te ulepszenia w charakterze ćwiczenia opuścić dla czytnika.


### <a name="adding-a-link-to-theadditionaluserinfoaspxpage-in-the-master-page"></a>Dodawanie Linku do`AdditionalUserInfo.aspx`strony na stronie wzorcowej

Obecnie usługa witryny sieci Web nie zapewnia łącza do `AdditionalUserInfo.aspx` strony. Jedynym sposobem, aby przejść do niego jest wprowadź adres URL strony bezpośrednio w pasku adresu przeglądarki. Możemy dodać link do tej strony w `Site.master` strony wzorcowej.

Pamiętaj, że strona główna zawiera kontrolki widoku logowania sieci Web w jego `LoginContent` ContentPlaceHolder wyświetlającą różny kod znaczników dla użytkowników uwierzytelnionych i anonimowych. Zaktualizuj kontrolki widoku logowania `LoggedInTemplate` aby uwzględnić łącze do `AdditionalUserInfo.aspx` strony. Po wprowadzeniu tych zmian widoku logowania oznaczeniu deklaracyjnym kontrolki powinien wyglądać podobny do następującego:

[!code-aspx[Main](storing-additional-user-information-cs/samples/sample7.aspx)]

Należy pamiętać, dodanie `lnkUpdateSettings` kontrolki Hiperlinku `LoggedInTemplate`. Z tego linku w miejscu uwierzytelnionych użytkowników można szybko przechodzić do strony, aby wyświetlać i modyfikować ustawień głównego miejscowości, strony głównej i podpis.

## <a name="step-4-adding-new-guestbook-comments"></a>Krok 4: Dodanie nowych komentarzy księgi gości

`Guestbook.aspx` Strona jest którym uwierzytelnionym użytkownikom można wyświetlać księgi gości i Dodaj komentarz. Zacznijmy od utworzenia interfejs służący do dodawania nowych komentarzy księgi gości.

Otwórz `Guestbook.aspx` strony w programie Visual Studio i utworzyć interfejs użytkownika składający się z dwóch pól tekstowych: jeden dla podmiotu nowy komentarz, a drugi dla jej treści. Ustaw pierwszą kontrolkę pola tekstowego `ID` właściwości `Subject` i jego `Columns` właściwość 40; Ustaw sekundę `ID` do `Body`, jego `TextMode` do `MultiLine`i jego `Width` i `Rows` właściwości "95%" i 8, odpowiednio. Aby ukończyć interfejsu użytkownika, należy dodać formant przycisku w sieci Web o nazwie `PostCommentButton` i ustaw jego `Text` właściwość "Comment Your" Post".

Ponieważ każdy komentarz księgi gości wymaga temat i treść, należy dodać RequiredFieldValidator dla wszystkich pól tekstowych. Ustaw `ValidationGroup` właściwości tych kontrolek, aby "EnterComment"; Podobnie, ustaw `PostCommentButton` kontrolki `ValidationGroup` właściwość "EnterComment". Aby uzyskać więcej informacji na temat ASP. Kontrolkami walidacji w sieci, zapoznaj się [weryfikacji formularza w programie ASP.NET:](http://www.4guysfromrolla.com/webtech/090200-1.shtml), [analiza formanty sprawdzania poprawności w programie ASP.NET 2.0](http://aspnet.4guysfromrolla.com/articles/112305-1.aspx)i [samouczek formantów serwera sprawdzania poprawności](http://www.w3schools.com/aspnet/aspnet_refvalidationcontrols.asp) na [W3Schools](http://www.w3schools.com/).

Po tworzeniu interfejsu użytkownika oznaczeniu deklaracyjnym strony powinien wyglądać następująco:

[!code-aspx[Main](storing-additional-user-information-cs/samples/sample8.aspx)]

Pełny interfejs użytkownika naszym kolejnym krokiem jest wstawić nowy rekord do `GuestbookComments` tabeli, gdy `PostCommentButton` kliknięciu. Można to zrobić na wiele sposobów: napiszemy kod ADO.NET przycisku `Click` programu obsługi zdarzeń; firma Microsoft można dodać kontrolki SqlDataSource do strony, skonfiguruj jego `InsertCommand`, a następnie wywołać jej `Insert` metody z `Click` zdarzeń Program obsługi; lub można tworzyć warstwy środkowej, który był odpowiedzialny za wstawianie nowych komentarzy księgi gości i wywołać tę funkcję z `Click` programu obsługi zdarzeń. Ponieważ przyjrzeliśmy się przy użyciu SqlDataSource w kroku 3, użyjemy kodu ADO.NET w tym miejscu.

> [!NOTE]
> Klas ADO.NET, używanych do programowego dostępu do danych z bazy danych programu Microsoft SQL Server znajdują się w `System.Data.SqlClient` przestrzeni nazw. Może być konieczne do importowania tej przestrzeni nazw do strony swojego osobna klasa kodu (czyli `using System.Data.SqlClient;`).


Utwórz procedurę obsługi zdarzeń dla `PostCommentButton`firmy `Click` zdarzeń i Dodaj następujący kod:

[!code-csharp[Main](storing-additional-user-information-cs/samples/sample9.cs)]

`Click` Program obsługi zdarzeń uruchamia, sprawdzając, czy dane dostarczone przez użytkownika są prawidłowe. Jeśli nie jest dostępne, przed wstawieniem rekord kończy działanie programu obsługi zdarzeń. Zakładając, że podane dane są prawidłowe, aktualnie zalogowanego użytkownika `UserId` wartość jest pobierana i przechowywane w `currentUserId` zmiennej lokalnej. Ta wartość jest potrzebna, ponieważ firma Microsoft należy podać `UserId` wartości podczas wstawiania rekordów do `GuestbookComments`.

Poniżej parametrów połączenia Aby uzyskać `SecurityTutorials` bazy danych jest pobierana z `Web.config` i `INSERT` jest określona żadna instrukcja SQL. A `SqlConnection` obiekt zostanie utworzony i otwarty. Następnie `SqlCommand` obiekt jest konstruowany i wartości dla parametrów używane w `INSERT` zapytania są przypisane. `INSERT` Następnie zostaje wykonana instrukcja i zamknięcie połączenia. Na koniec obsługi zdarzeń `Subject` i `Body` pól tekstowych `Text` właściwości są wyczyszczone, aby wartości przez użytkownika nie są zachowywane między zwrotu.

Przejdź dalej i przetestowania tę stronę w przeglądarce. Ponieważ na tej stronie znajduje się w `Membership` folder nie jest on dostępny anonimowych gościom. W związku z tym należy najpierw zalogować się na (jeśli jeszcze tego nie zrobiono). Wprowadź wartość w `Subject` i `Body` pól tekstowych i kliknij przycisk `PostCommentButton` przycisku. Spowoduje to nowy rekord ma zostać dodany do `GuestbookComments`. Na odświeżenie strony temat i treść, podane są czyszczone z pól tekstowych.

Po kliknięciu przycisku `PostCommentButton` przycisk nie jest brak wizualną opinię dodaną komentarz do księgi gości. Firma Microsoft nadal należy zaktualizować tę stronę, aby wyświetlić istniejące komentarze księgi gości, które robimy w kroku 5. Po możemy to zrobić, po prostu dodać komentarz zostanie wyświetlony na liście Komentarze, podając odpowiednie wizualną opinię. Teraz, upewnij się, że Twój komentarz księgi gości została zapisana, sprawdzając zawartość `GuestbookComments` tabeli.

Rysunek 17 pokazuje zawartość `GuestbookComments` tabeli po dwóch komentarze zostały wystawione.


[![Może wyświetlać komentarze księgi gości w tabeli GuestbookComments](storing-additional-user-information-cs/_static/image50.png)](storing-additional-user-information-cs/_static/image49.png)

**Rysunek 17**: może wyświetlać komentarze księgi gości w `GuestbookComments` tabeli ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](storing-additional-user-information-cs/_static/image51.png))


> [!NOTE]
> Jeśli użytkownik próbuje Wstaw komentarz księgi gości, który zawiera potencjalnie niebezpiecznych znaczników — takich jak HTML, ASP.NET będzie sygnalizować `HttpRequestValidationException`. Aby dowiedzieć się więcej na temat tego wyjątku, dlaczego jest zgłaszany, i jak można zezwolić użytkownikom na przesłanie potencjalnie niebezpiecznych wartości, zapoznaj się z [oficjalny dokument dotyczący sprawdzania poprawności żądań](../../../../whitepapers/request-validation.md).


## <a name="step-5-listing-the-existing-guestbook-comments"></a>Krok 5: Lista komentarze księgi gości

Oprócz umieszczania komentarzy, których użytkownik odwiedzający `Guestbook.aspx` strony powinny również mieć możliwość wyświetlania komentarze księgi gości. Aby to osiągnąć, należy dodać kontrolki ListView o nazwie `CommentList` do dolnej części strony.

> [!NOTE]
> Kontrolka ListView jest nowym składnikiem programu ASP.NET w wersji 3.5. Służy do wyświetlania listy elementów w bardzo możliwe do dostosowania i elastyczny układ, ale nadal oferuje wbudowane edytowanie, wstawianie, usuwanie, stronicowanie i sortowanie funkcji, takich jak kontrolki GridView. Jeśli używasz programu ASP.NET 2.0, należy zamiast tego użyj kontrolki DataList lub Repeater. Aby uzyskać więcej informacji na temat korzystania z ListView, zobacz [Scott Guthrie](https://weblogs.asp.net/scottgu/)firmy wpis w blogu [asp: ListView kontroli](https://weblogs.asp.net/scottgu/archive/2007/08/10/the-asp-listview-control-part-1-building-a-product-listing-page-with-clean-css-ui.aspx)i Moje artykułu [wyświetlanie danych za pomocą kontrolki ListView](http://aspnet.4guysfromrolla.com/articles/122607-1.aspx).


Otwórz ListView tagu inteligentnego, a następnie z listy rozwijanej wybierz źródło danych, należy powiązać formant z nowego źródła danych. Jak widzieliśmy w kroku 2, spowoduje to uruchomienie Kreatora konfiguracji źródła danych. Wybierz ikonę bazy danych, nazwę wynikowy SqlDataSource `CommentsDataSource`i kliknij przycisk OK. Następnie wybierz pozycję `SecurityTutorialsConnectionString` połączenia ciągu z listy rozwijanej i kliknij przycisk Dalej.

W tym momencie w kroku 2 określonej danych do wykonywania zapytań przez pobrania `UserProfiles` tabeli z listy rozwijanej, a następnie wybierając kolumny do zwrócenia (odnoszą się do rysunek 9). Tym razem jednak chcemy, aby sformułować instrukcję SQL, która ściąga ponownie nie tylko rekordy z `GuestbookComments`, ale również twórca komentarza usługi macierzystego miejscowości, strony głównej, podpis i nazwy użytkownika. W związku z tym wybierz przycisk radiowy "Określ niestandardową instrukcję SQL lub procedury składowanej" i kliknij przycisk Dalej.

Spowoduje to wyświetlenie na ekranie "Zdefiniować niestandardowe instrukcji lub procedur składowanych". Kliknij przycisk konstruktora zapytań graficznie tworzyć zapytania. Konstruktor zapytań rozpoczyna się od prośbą o Określ tabele, które chcemy powstają kwerendy. Wybierz `GuestbookComments`, `UserProfiles`, i `aspnet_Users` tabele, a następnie kliknij przycisk OK. Spowoduje to dodanie wszystkie trzy tabele do powierzchni projektowej. Ponieważ istnieją ograniczenia klucza obcego między `GuestbookComments`, `UserProfiles`, i `aspnet_Users` tabel, Konstruktor zapytań automatycznie `JOIN` s tych tabel.

Pozostaje tylko określone kolumny do zwrócenia. Z `GuestbookComments` tabeli wybierz `Subject`, `Body`, i `CommentDate` kolumny; return `HomeTown`, `HomepageUrl`, i `Signature` kolumny z `UserProfiles` tabeli; i zwracają `UserName` z `aspnet_Users`. Ponadto Dodaj "`ORDER BY CommentDate DESC`" na końcu `SELECT` zapytanie tak, aby najpierw zwracane są najnowsze wpisy. Po wprowadzeniu tych opcji, interfejsu konstruktora zapytań powinna wyglądać zrzut na rysunku 18 ekranu.


[![Zapytanie zbudowanych sprzęga GuestbookComments UserProfiles i aspnet_Users tabel](storing-additional-user-information-cs/_static/image53.png)](storing-additional-user-information-cs/_static/image52.png)

**Rysunek 18**: zapytanie wykonane `JOIN` s `GuestbookComments`, `UserProfiles`, i `aspnet_Users` tabel ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](storing-additional-user-information-cs/_static/image54.png))


Kliknij przycisk OK, aby zamknąć okno konstruktora zapytań i powrócić do ekranu "Zdefiniować niestandardowe instrukcji lub procedur składowanych". Kliknij obok przejdź do ekranu "Testuj zapytanie", w którym mogą wyświetlać wyniki zapytania, klikając przycisk Testuj zapytanie. Gdy wszystko będzie gotowe, kliknij przycisk Zakończ, aby zakończyć działanie kreatora Konfigurowanie źródła danych.

Firma Microsoft ukończenia kreatora Konfigurowanie źródła danych, w kroku 2, skojarzone kontrolce DetailsView `Fields` Kolekcja została zaktualizowana do uwzględnienia elementu BoundField dla każdej kolumny zwracane przez `SelectCommand`. ListView, jednak pozostaje bez zmian; wciąż potrzebujemy do definiowania jego układu. Układ ListView można konstruować ręcznie za pośrednictwem jego oznaczeniu deklaracyjnym lub opcję "Konfiguruj ListView" w jego tagu inteligentnego. Czy mogę zwykle Preferuj Definiowanie znaczników ręcznie, ale użyć dowolnej metody jest najbardziej naturalna.

Czy mogę zakończył za pomocą następujących `LayoutTemplate`, `ItemTemplate`, i `ItemSeparatorTemplate` formantu ListView:

[!code-aspx[Main](storing-additional-user-information-cs/samples/sample10.aspx)]

`LayoutTemplate` Definiuje znaczniki emitowane przez kontrolkę, podczas gdy `ItemTemplate` renderuje każdego elementu zwróconego przy użyciu kontrolki SqlDataSource. `ItemTemplate`Firmy wynikowy znaczników jest umieszczany w `LayoutTemplate`firmy `itemPlaceholder` kontroli. Oprócz `itemPlaceholder`, `LayoutTemplate` obejmuje kontrolka DataPager, co ogranicza ListView do wyświetlania tylko 10 komentarze księgi gości na stronie (ustawienie domyślne) i renderuje interfejsu stronicowania.

Moje `ItemTemplate` temat każdy komentarz księgi gości w `<h4>` element z treści znajdujących się poniżej podmiotu. Składnia służąca do wyświetlania treść ma dane zwrócone przez `Eval("Body")` instrukcji wiązania danych, konwertuje ją na ciąg i zamienia podziały wierszy z `<br />` elementu. Ta konwersja jest konieczna, aby pokazać podziały wprowadzone podczas przesyłania komentarza, ponieważ odstęp jest ignorowana przez HTML. Podpis użytkownika jest wyświetlana poniżej treści kursywą, a następnie według miejscowości macierzystego użytkownika, link do jego strony głównej, daty i godziny wprowadzone komentarz oraz nazwa użytkownika osoby, który wystawił komentarz.

Poświęć chwilę, aby wyświetlić stronę za pośrednictwem przeglądarki. Powinien zostać wyświetlony komentarze, które zostały dodane do księgi gości w kroku 5 wyświetlane w tym miejscu.


[![Teraz Guestbook.aspx Wyświetla komentarze księgi gości](storing-additional-user-information-cs/_static/image56.png)](storing-additional-user-information-cs/_static/image55.png)

**Rysunek 19**: `Guestbook.aspx` komentarze księgi gości są obecnie wyświetlane ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](storing-additional-user-information-cs/_static/image57.png))


Spróbuj dodać nowy komentarz do księgi gości. Po kliknięciu `PostCommentButton` przycisk strony publikuje Wstecz i komentarz zostanie dodany do bazy danych, ale kontrolki ListView nie jest aktualizowana, aby wyświetlić nowy komentarz. Można to naprawić, albo:

- Aktualizowanie `PostCommentButton` przycisku `Click` programu obsługi zdarzeń, tak że wywołuje kontrolki ListView `DataBind()` metoda po wstawieniu nowy komentarz do bazy danych, lub
- Ustawienia formantu ListView `EnableViewState` właściwość `false`. Ta metoda działa, ponieważ po wyłączeniu stan wyświetlania kontrolki, należy ponownie powiązać z danymi źródłowymi w każdym zwrotu.

Samouczek witryny sieci Web do pobrania z tego samouczka przedstawiono obu tych technik. Kontrolki ListView `EnableViewState` właściwości `false` i kodu programowo ponownie powiązać dane ListView znajduje się w `Click` programu obsługi zdarzeń, ale są oznaczone jako komentarz.

> [!NOTE]
> Obecnie `AdditionalUserInfo.aspx` strony umożliwia użytkownikowi wyświetlanie i edytowanie ustawień głównego miejscowości, strony głównej i podpis. Może być przydatne do zaktualizowania `AdditionalUserInfo.aspx` do wyświetlenia zalogowanego w komentarzach księgi gości użytkownika. Oznacza to, oprócz badanie i modyfikowanie jej informacje, użytkownik może odwiedzić `AdditionalUserInfo.aspx` strony, aby zobaczyć, jakie księgi gości komentarze podjęła w przeszłości. Można pozostawić to w charakterze ćwiczenia zainteresowanych czytnika.


## <a name="step-6-customizing-the-createuserwizard-control-to-include-an-interface-for-the-home-town-homepage-and-signature"></a>Krok 6: Dostosowywanie formantu CreateUserWizard, aby uwzględnić interfejsu Scootney, strony głównej i podpis

`SELECT` Zapytania używanego przez `Guestbook.aspx` stronie używa `INNER JOIN` połączyć powiązanych rekordów spośród `GuestbookComments`, `UserProfiles`, i `aspnet_Users` tabel. Jeśli użytkownik, który nie ma rekordu w `UserProfiles` komentarz nawiązuje księgi gości, komentarz nie będą wyświetlane w ListView, ponieważ `INNER JOIN` zwraca tylko `GuestbookComments` rekordów, gdy istnieją rekordy w `UserProfiles` i `aspnet_Users`. I które widzieliśmy w kroku 3, jeśli użytkownik nie ma rekordu `UserProfiles` ona nie można wyświetlić lub edytować jej ustawienia w `AdditionalUserInfo.aspx` strony.

Needless, że ze względu na nasze projektowania decyzje, które jest ważne, że każde konto użytkownika w systemie członkostwa mają pasujący obiekt typu rekord w `UserProfiles` tabeli. Chcemy jest odpowiedni rekord, który ma zostać dodany do `UserProfiles` zawsze, gdy konto nowego użytkownika członkostwa jest tworzony za pomocą CreateUserWizard.

Zgodnie z opisem w [ *tworzenie kont użytkowników* ](creating-user-accounts-cs.md) samouczek, po utworzeniu nowego konta użytkownika członkostwa kontroli CreateUserWizard wywołuje jego [ `CreatedUser` zdarzeń](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.createduser.aspx). Możemy utworzyć program obsługi zdarzeń dla tego zdarzenia, Uzyskaj identyfikator UserId dla użytkownika nowo utworzoną i następnie wstawienia rekordu do `UserProfiles` tabelę z wartościami domyślnymi dla `HomeTown`, `HomepageUrl`, i `Signature` kolumn. Co więcej istnieje możliwość monit o podanie tych wartości, dostosowując interfejsu kontroli CreateUserWizard uwzględnienie dodatkowych pól tekstowych.

Najpierw Przyjrzyjmy się jak dodać nowy wiersz do `UserProfiles` tabelę `CreatedUser` programu obsługi zdarzeń z wartościami domyślnymi. Poniżej zobaczymy sposobu dostosowywania interfejsu użytkownika formantu CreateUserWizard obejmujący dodatkowe pola do zbierania macierzystego miejscowości, strony głównej i podpis nowego użytkownika.

### <a name="adding-a-default-row-touserprofiles"></a>Dodawanie domyślnego wiersza do`UserProfiles`

W [ *tworzenie kont użytkowników* ](creating-user-accounts-cs.md) samouczek dodaliśmy formancie CreateUserWizard do `CreatingUserAccounts.aspx` stronie `Membership` folderu. Aby mogła mieć CreateUserWizard kontroli dodać rekord do `UserProfiles` tabeli po utworzeniu konta użytkownika, należy zaktualizować funkcjonalność formantu CreateUserWizard. Zamiast wprowadzania tych zmian do `CreatingUserAccounts.aspx` strony, zamiast Dodajmy formancie nowe CreateUserWizard do `EnhancedCreateUserWizard.aspx` strony, a następnie wprowadź zmiany w tym samouczku.

Otwórz `EnhancedCreateUserWizard.aspx` strony w programie Visual Studio, a następnie przeciągnij formancie CreateUserWizard z przybornika na stronę. Ustawianie kontroli CreateUserWizard `ID` właściwość `NewUserWizard`. Tak jak Omówiliśmy to w <a id="_msoanchor_5"> </a> [ *tworzenie kont użytkowników* ](creating-user-accounts-cs.md) samouczek CreateUserWizard domyślnym interfejsem użytkownika monituje o niezbędne informacje. Po podano te informacje, formant wewnętrznie tworzy nowe konto użytkownika w ramach członkostwa, wszystko to bez konieczności nam konieczności pisania nawet wiersza kodu.

Kontrolka CreateUserWizard podnosi liczbę zdarzeń, podczas jego przepływu pracy. Po użytkownik dostarcza informacje o żądaniu i przesyła formularz, formant CreateUserWizard początkowo generowane jego [ `CreatingUser` zdarzeń](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.creatinguser.aspx). Jeśli występuje problem podczas procesu tworzenia [ `CreateUserError` zdarzeń](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.createusererror.aspx) wyzwoleniu; jednak, jeśli użytkownik została pomyślnie utworzona, a następnie [ `CreatedUser` zdarzeń](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.createduser.aspx) jest wywoływane. W <a id="_msoanchor_6"> </a> [ *tworzenie kont użytkowników* ](creating-user-accounts-cs.md) samouczku utworzyliśmy program obsługi zdarzeń dla `CreatingUser` zdarzeń, aby upewnić się, czy podana nazwa użytkownika nie zawierała wszystkie wiodące lub spacje końcowe i że nazwa użytkownika nie pojawiły się dowolnym miejscu w haśle.

Aby można było dodać wiersz w `UserProfiles` tabeli dla nowo utworzoną użytkownika, należy utworzyć program obsługi zdarzeń dla `CreatedUser` zdarzeń. Do czasu `CreatedUser` zdarzenie jest wywoływane, konto użytkownika został już utworzony w ramach członkostwa, dzięki czemu możemy pobrać wartość identyfikatora użytkownika dla konta.

Utwórz procedurę obsługi zdarzeń dla `NewUserWizard`firmy `CreatedUser` zdarzeń i Dodaj następujący kod:

[!code-csharp[Main](storing-additional-user-information-cs/samples/sample11.cs)]

Powyższe żywych kodu przez pobranie identyfikatora konta użytkownika po prostu dodanego przez producenta. Jest to realizowane przy użyciu `Membership.GetUser(username)` zwrócone informacje na temat określonego użytkownika, a następnie za pomocą metody `ProviderUserKey` właściwość służąca do pobierania swojego identyfikatora użytkownika. Nazwa użytkownika wprowadzona przez użytkownika w CreateUserWizard jest dostępny za pośrednictwem jego [ `UserName` właściwość](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.username.aspx).

Następnie parametry połączenia są pobierane z `Web.config` i `INSERT` określona żadna instrukcja. Niezbędne obiekty ADO.NET są tworzone i wykonywane polecenia. Kod przypisuje [ `DBNull` ](https://msdn.microsoft.com/library/system.dbnull.aspx) wystąpienia do `@HomeTown`, `@HomepageUrl`, i `@Signature` parametry, które ma Wstawianie bazy danych `NULL` wartości `HomeTown`, `HomepageUrl`, i `Signature` pola.

Odwiedź stronę `EnhancedCreateUserWizard.aspx` strony za pośrednictwem przeglądarki, a następnie utwórz nowe konto użytkownika. Po wykonaniu tej czynności, wróć do programu Visual Studio i sprawdź zawartość `aspnet_Users` i `UserProfiles` tabele (takich jak Robiliśmy to ponownie rysunek 12). Powinien zostać wyświetlony nowemu kontu użytkownika w `aspnet_Users` i odpowiadający mu `UserProfiles` wiersza (przy użyciu `NULL` wartości `HomeTown`, `HomepageUrl`, i `Signature`).


[![Dodano nowe konto użytkownika i UserProfiles rekordu](storing-additional-user-information-cs/_static/image59.png)](storing-additional-user-information-cs/_static/image58.png)

**Rysunek 20**: nowe konto użytkownika i `UserProfiles` dodano rekord ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](storing-additional-user-information-cs/_static/image60.png))


Po odwiedzający został dostarczony jego informacjami o nowym koncie kliknięto przycisk "Utwórz użytkownika", utworzono konto użytkownika i dodać wiersz do `UserProfiles` tabeli. Następnie wyświetla CreateUserWizard jego `CompleteWizardStep`, która wyświetla komunikat o powodzeniu i przycisk Kontynuuj. Kliknięcie przycisku Kontynuuj powoduje odświeżenie strony, ale nie podjęto żadnej akcji, pozostawiając użytkownika została zablokowana na `EnhancedCreateUserWizard.aspx` strony.

Można określić adres URL, aby wysłać użytkownika do po kliknięciu przycisku Kontynuuj za pomocą kontrolki CreateUserWizard [ `ContinueDestinationPageUrl` właściwość](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.continuedestinationpageurl.aspx). Ustaw `ContinueDestinationPageUrl` właściwość "~ / Membership/AdditionalUserInfo.aspx". Spowoduje to przejście nowemu użytkownikowi `AdditionalUserInfo.aspx`, gdzie mogą wyświetlać i aktualizować ustawień.

### <a name="customizing-the-createuserwizards-interface-to-prompt-for-the-new-users-home-town-homepage-and-signature"></a>Dostosowywanie interfejsu CreateUserWizard wybór opcji Monituj o Scootney nowego użytkownika, strony głównej i podpis

Kontrolka CreateUserWizard domyślny interfejs jest wystarczająca dla scenariuszy tworzenia prostego, gdzie muszą być zbierane tylko podstawowe informacje o koncie użytkownika, takie jak nazwa użytkownika, hasło i adres e-mail. Ale co zrobić, jeśli chcemy Monituj odwiedzający wprowadzenia jej głównego miejscowości, strony głównej i podpis podczas tworzenia swojego konta? Istnieje możliwość dostosowania interfejsu kontroli CreateUserWizard, aby zbierać dodatkowe informacje podczas rejestracji, a te informacje mogą być używane w `CreatedUser` programu obsługi zdarzeń, aby wstawić dodatkowe rekordy do podstawowej bazy danych.

Formant CreateUserWizard rozszerza formant kreatora ASP.NET, który jest formant, który umożliwia dewelopera strony do definiowania serii uporządkowane `WizardSteps`. Formant kreatora renderuje krok aktywny i udostępnia interfejs nawigacji, który umożliwia obiektu odwiedzającego przejść przez następujące kroki. Kreator kontrolki jest idealny dla potężne długich zadań w kilku krótkich krokach. Aby uzyskać więcej informacji na temat formantu kreatora, zobacz [Tworzenie interfejsu użytkownika krok po kroku za pomocą programu ASP.NET 2.0 kreatora formantu](http://aspnet.4guysfromrolla.com/articles/061406-1.aspx).

Formant CreateUserWizard domyślne znaczników definiuje dwa `WizardSteps`: `CreateUserWizardStep` i `CompleteWizardStep`.

[!code-aspx[Main](storing-additional-user-information-cs/samples/sample12.aspx)]

Pierwszy `WizardStep`, `CreateUserWizardStep`, powoduje wyświetlenie interfejs, który monituje o podanie nazwy użytkownika, hasła, wiadomości e-mail i tak dalej. Po odwiedzający będzie przekazywać te informacje i kliknie przycisk "Create User", jest ona wyświetlana `CompleteWizardStep`, które wyświetla komunikat o powodzeniu i przycisk Kontynuuj.

Aby dostosować interfejsu kontroli CreateUserWizard, aby uwzględnić dodatkowe pola, możemy wykonywać następujące czynności:

- <strong>Utwórz co najmniej jeden nowy</strong><strong>`WizardStep`</strong><strong>s, aby zawierać dodatkowe elementy interfejsu użytkownika</strong>. Aby dodać nowy `WizardStep` aby CreateUserWizard, kliknij przycisk "Dodaj lub usuń `WizardSteps`" link z jego tagu inteligentnego, aby uruchomić `WizardStep` — Edytor kolekcji. W tym miejscu możesz dodać, usunąć lub zmienić kolejność kroków kreatora. To podejście, które zostaną użyte na potrzeby tego samouczka.

- <strong>Konwertuj</strong><strong>`CreateUserWizardStep`</strong><strong>do edycji</strong><strong>`WizardStep`</strong><strong>.</strong> Spowoduje to zastąpienie `CreateUserWizardStep` ekwiwalent `WizardStep` którego znaczników definiuje interfejs użytkownika, który odpowiada `CreateUserWizardStep`"s. Za pomocą konwersji `CreateUserWizardStep` do `WizardStep` możemy zmienić położenie kontrolki lub dodać dodatkowe elementy interfejsu użytkownika do tego kroku. Aby przekonwertować `CreateUserWizardStep` lub `CompleteWizardStep` do edycji `WizardStep`, kliknij przycisk "Dostosuj Utwórz użytkownika krok" lub "Dostosowywanie wykonaj krok" link z tagu kontrolki.

- **Użyj kombinacji powyższych dwóch opcji.**

Istotnym aspektem na uwadze jest, że kontroli CreateUserWizard wykonuje jej proces tworzenia konta użytkownika, po kliknięciu przycisku "Utwórz użytkownika" z poziomu jej `CreateUserWizardStep`. Nie ma znaczenia, jeśli istnieją dodatkowe `WizardStep` s po `CreateUserWizardStep` czy nie.

Podczas dodawania niestandardowego `WizardStep` do kontroli CreateUserWizard, aby zbierać dodatkowe wprowadzania danych przez użytkownika niestandardowego `WizardStep` można umieścić przed lub po `CreateUserWizardStep`. Jeśli znajduje się przed `CreateUserWizardStep` następnie danych wejściowych użytkownika dodatkowe są zbierane z niestandardowego `WizardStep` jest dostępna dla `CreatedUser` programu obsługi zdarzeń. Jednak jeśli niestandardowa `WizardStep` przychodzi po `CreateUserWizardStep` następnie przez czas niestandardowego `WizardStep` jest wyświetlany nowemu kontu użytkownika już istnieje i `CreatedUser` zdarzenie zostało już uruchomione.

21 rysunku przedstawiono przepływ pracy po dodany `WizardStep` poprzedza `CreateUserWizardStep`. Ponieważ informacje o użytkowniku dodatkowe został zebrany przez czas `CreatedUser` generowane zdarzenie, wszystkie musimy to zrobić to aktualizacja `CreatedUser` programu obsługi zdarzeń w celu pobrania tych danych wejściowych i je wykorzystać do `INSERT` wartości parametrów instrukcji (zamiast `DBNull.Value`).


[![CreateUserWizard przepływu pracy, gdy dodatkowe WizardStep poprzedza element CreateUserWizardStep](storing-additional-user-information-cs/_static/image62.png)](storing-additional-user-information-cs/_static/image61.png)

**Rysunek 21**: CreateUserWizard przepływu pracy podczas dodatkowe `WizardStep` Precedes `CreateUserWizardStep` ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](storing-additional-user-information-cs/_static/image63.png))


Jeśli niestandardowa `WizardStep` jest umieszczany *po* `CreateUserWizardStep`, jednak proces tworzenia konta użytkownika występuje przed użytkownika miała szansę, wprowadź jej głównego miejscowości, strony głównej lub podpisu. W takim przypadku są to informacje dodatkowe musi zostać wstawiony do bazy danych po utworzeniu konta użytkownika, tak jak pokazano na rysunku 22.


[![CreateUserWizard przepływu pracy, gdy po element CreateUserWizardStep dodatkowe WizardStep](storing-additional-user-information-cs/_static/image65.png)](storing-additional-user-information-cs/_static/image64.png)

**Rysunek 22**: CreateUserWizard przepływu pracy podczas dodatkowe `WizardStep` pochodzi od `CreateUserWizardStep` ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](storing-additional-user-information-cs/_static/image66.png))


Przepływ pracy pokazano na rysunku 22 czeka do wstawienia rekordu do `UserProfiles` tabeli do czasu, po ukończeniu kroku 2. Jeśli odwiedzający zostanie zamknięte w swojej przeglądarce po wykonaniu kroku 1, jednak firma Microsoft będzie osiągnięto stanu, w którym utworzono konto użytkownika, ale żaden rekord został dodany do `UserProfiles`. Jednym z rozwiązań jest rekord z `NULL` lub wstawione do wartości domyślnych `UserProfiles` w `CreatedUser` programu obsługi zdarzeń (który jest uruchamiany po wykonaniu kroku 1), a następnie zaktualizuj to rejestrowania po ukończeniu kroku 2. Gwarantuje to, że `UserProfiles` rekord zostanie dodany do konta użytkownika nawet wtedy, gdy użytkownik zamyka midway procesu rejestracji za pośrednictwem.

W tym samouczku utworzymy nową `WizardStep` występuje po `CreateUserWizardStep` lecz przed `CompleteWizardStep`. Pierwszy get WizardStep w miejscu i skonfigurowany, a następnie wysyłamy będzie Spójrzmy na kod.

Z tagu inteligentnego sterowania CreateUserWizard, wybierz opcję "Dodaj lub usuń `WizardStep` s", co spowoduje uruchomienie `WizardStep` okno dialogowe Edytor kolekcji. Dodaj nową `WizardStep`, ustawiając jego `ID` do `UserSettings`, jego `Title` do "Your Settings" i jego `StepType` do `Step`. Umieść ją tak, aby nastąpi po `CreateUserWizardStep` ("Utwórz konto dla nowego konta"), a przed `CompleteWizardStep` ("ukończony"), jak pokazano na rysunku 23.


[![Dodaj nowy element WizardStep do kontroli CreateUserWizard](storing-additional-user-information-cs/_static/image68.png)](storing-additional-user-information-cs/_static/image67.png)

**Ilustracja 23**: Dodaj nowy `WizardStep` do kontroli CreateUserWizard ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](storing-additional-user-information-cs/_static/image69.png))


Kliknij przycisk OK, aby zamknąć `WizardStep` okno dialogowe Edytor kolekcji. Nowy `WizardStep` świadczy kontroli CreateUserWizard zaktualizowane oznaczeniu deklaracyjnym:

[!code-aspx[Main](storing-additional-user-information-cs/samples/sample13.aspx)]

Należy pamiętać, nowe `<asp:WizardStep>` elementu. Musimy dodać interfejs użytkownika do gromadzenia macierzystego miejscowości, strony głównej i podpis w tym miejscu nowego użytkownika. Można wprowadzić tę zawartość, w składni deklaratywnej lub za pomocą projektanta. Aby korzystać z projektanta, wybierz w kroku "Your Settings" z listy rozwijanej w tagu inteligentnego, aby zobaczyć krok w projektancie.

> [!NOTE]
> Wybranie kroku za pomocą listy rozwijanej tagu inteligentnego aktualizuje kontroli CreateUserWizard [ `ActiveStepIndex` właściwość](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.activestepindex.aspx), która określa indeks kroku początkowego. W związku z tym, jeśli używasz tej listy rozwijanej do edycji danego kroku "Your Settings" w projektancie, pamiętaj go ustawić ponownie na "Logowania konto nowego konta", aby ten krok jest wyświetlany, gdy najpierw podczas odwiedzin `EnhancedCreateUserWizard.aspx` strony.


Tworzenie interfejsu użytkownika w ramach tego kroku "Your Settings", który zawiera trzy kontrolki TextBox o nazwie `HomeTown`, `HomepageUrl`, i `Signature`. Po konstruowanie ten interfejs, CreateUserWizard oznaczeniu deklaracyjnym powinien wyglądać podobnie do poniższej:

[!code-aspx[Main](storing-additional-user-information-cs/samples/sample14.aspx)]

Przejdź dalej i odwiedź tę stronę za pośrednictwem przeglądarki i Utwórz nowe konto użytkownika, określania wartości dla głównego miejscowości, strony głównej i podpis. Po zakończeniu `CreateUserWizardStep` konto użytkownika zostanie utworzone w ramach członkostwa i `CreatedUser` działa program obsługi zdarzeń, które dodaje nowy wiersz do `UserProfiles`, z bazą danych, ale `NULL` wartość `HomeTown`, `HomepageUrl`, i `Signature`. Nigdy nie są używane wartości wprowadzone dla głównego miejscowości, strony głównej i podpis. Wynikiem jest konto użytkownika z `UserProfiles` rejestrowania, którego `HomeTown`, `HomepageUrl`, i `Signature` pola jeszcze nie można określić.

Musimy wykonanie kodu po wykonaniu kroku "Your Settings", która przyjmuje macierzystego miejscowości, honepage i podpis wartości wprowadzonej przez użytkownika i aktualizuje odpowiedni `UserProfiles` rekordu. Każdorazowo, użytkownik przechodzi między krokami w Kreatorze kontrolek, Kreator [ `ActiveStepChanged` zdarzeń](https://msdn.microsoft.com/library/system.web.ui.webcontrols.wizard.activestepchanged.aspx) uruchamiany. Można utworzyć program obsługi zdarzeń dla tego zdarzenia i aktualizację `UserProfiles` tabeli po ukończeniu kroku "Your Settings".

Dodaj program obsługi zdarzeń dla CreateUserWizard `ActiveStepChanged` zdarzeń i Dodaj następujący kod:

[!code-csharp[Main](storing-additional-user-information-cs/samples/sample15.cs)]

Powyższy kod uruchamia się przez określenie, czy po prostu osiągnęliśmy kroku "Zakończono". Ponieważ w kroku "Zakończono", występuje natychmiast po kroku "Your Settings", następnie po osiągnięciu przez obiekt odwiedzający "Ukończony" krok, co oznacza, że użytkownik właśnie został kroku "Your Settings".

W takim przypadku należy programowo odwoływać się do kontrolki pola tekstowego w ramach `UserSettings WizardStep`. Jest to realizowane przy pierwszym użyciu `FindControl` metody programowe odwoływanie się do `UserSettings WizardStep`, a następnie ponownie, aby odwoływać się do pól tekstowych z poziomu `WizardStep`. Gdy istnieją odwołania pól tekstowych, jesteśmy gotowi do wykonania `UPDATE` instrukcji. `UPDATE` Instrukcji ma taką samą liczbę parametrów jak `INSERT` instrukcji w `CreatedUser` programu obsługi zdarzeń, ale w tym miejscu użyjemy macierzystego miejscowości, strony głównej i podpis wartości dostarczone przez użytkownika.

Z tej obsługi zdarzeń w miejscu, odwiedź stronę `EnhancedCreateUserWizard.aspx` strony za pośrednictwem przeglądarki, a następnie utwórz nowe konto użytkownika, określania wartości dla głównego miejscowości, strony głównej i podpis. Po utworzeniu nowego konta powinno nastąpić przekierowanie do `AdditionalUserInfo.aspx` strony, gdzie dokładnie wprowadzane macierzystego miejscowości, strony głównej i podpis informacje są wyświetlane.

> [!NOTE]
> Naszą witrynę sieci Web ma obecnie dwie strony, z których użytkownik może utworzyć nowe konto: `CreatingUserAccounts.aspx` i `EnhancedCreateUserWizard.aspx`. Mapy witryny dla witryny sieci Web i strony logowania wskazują `CreatingUserAccounts.aspx` strony, ale `CreatingUserAccounts.aspx` strony nie Monituj użytkownika o informacjami macierzystego miejscowości, strony głównej i podpis i nie powoduje dodania odpowiedni wiersz do `UserProfiles`. W związku z tym, albo zaktualizować `CreatingUserAccounts.aspx` strony, czemu oferuje tę funkcję, lub zaktualizuj strony mapy witryny i zaloguj się, aby odwołać się do `EnhancedCreateUserWizard.aspx` zamiast `CreatingUserAccounts.aspx`. Jeśli wybierzesz tę druga opcję, należy zaktualizować `Membership` folderu `Web.config` pliku tak, aby zezwolić anonimowym użytkownikom dostępu do `EnhancedCreateUserWizard.aspx` strony.


## <a name="summary"></a>Podsumowanie

W tym samouczku przyjrzeliśmy się technik modelowania danych, który jest powiązany z kont użytkowników w ramach członkostwa. W szczególności przyjrzeliśmy się modelowania jednostek, które mają relację jeden do wielu przy użyciu konta użytkownika, a także danych, która udostępnia relacja jeden do jednego. Ponadto widzieliśmy, jak to powiązane informacje można są wyświetlane, dodaje i aktualizowane przy użyciu kilka przykładów przy użyciu kontrolki SqlDataSource i innym osobom za pomocą kodu ADO.NET.

W tym samouczku kończy naszych przyjrzeć się konta użytkowników. Począwszy od następnego samouczka funkcję naszej uwagi do ról. Ciągu kolejnych kilka samouczków, omówimy framework ról, zobacz temat Tworzenie nowych ról, jak przypisać role do użytkowników, w jaki sposób można określić, jakie role użytkownika należy do oraz w jaki sposób zastosować autoryzacji opartej na rolach.

Wszystkiego najlepszego programowania!

### <a name="further-reading"></a>Dalsze informacje

Więcej informacji na tematów omówionych w tym samouczku można znaleźć w następujących zasobach:

- [Uzyskiwanie dostępu do i aktualizowanie danych w programie ASP.NET 2.0](http://aspnet.4guysfromrolla.com/articles/011106-1.aspx)
- [Kontrolka 2.0 kreatora ASP.NET](https://weblogs.asp.net/scottgu/archive/2006/02/21/438732.aspx)
- [Tworzenie interfejsu użytkownika krok po kroku za pomocą kontrolki ASP.NET 2.0 Kreatora](http://aspnet.4guysfromrolla.com/articles/061406-1.aspx)
- [Tworzenie niestandardowego źródła danych, parametry sterujące](http://aspnet.4guysfromrolla.com/articles/110106-1.aspx)
- [Dostosowywanie formantu CreateUserWizard](http://aspnet.4guysfromrolla.com/articles/070506-1.aspx)
- [Przewodniki Szybki Start w kontrolce DetailsView](https://quickstarts.asp.net/QuickStartv20/aspnet/doc/ctrlref/data/detailsview.aspx)
- [Wyświetlanie danych za pomocą formantu ListView](http://aspnet.4guysfromrolla.com/articles/122607-1.aspx)
- [Analiza formanty sprawdzania poprawności w programie ASP.NET 2.0](http://aspnet.4guysfromrolla.com/articles/112305-1.aspx)
- [Edytowanie, wstawianie i usuwanie danych](../../data-access/editing-inserting-and-deleting-data/an-overview-of-inserting-updating-and-deleting-data-cs.md)
- [Weryfikacji formularza w programie ASP.NET:](http://www.4guysfromrolla.com/webtech/090200-1.shtml)
- [Zbieranie informacji o rejestracji użytkownika niestandardowego](https://weblogs.asp.net/scottgu/archive/2006/07/05/Tip_2F00_Trick_3A00_-Gathering-Custom-User-Registration-Information.aspx)
- [Profile na platformie ASP.NET 2.0](http://www.odetocode.com/Articles/440.aspx)
- [Formant asp: ListView](https://weblogs.asp.net/scottgu/archive/2007/08/10/the-asp-listview-control-part-1-building-a-product-listing-page-with-clean-css-ui.aspx)
- [Szybki Start profilów użytkownika](https://quickstarts.asp.net/QuickStartv20/aspnet/doc/profile/default.aspx)

### <a name="about-the-author"></a>Informacje o autorze

Pracował nad Bento Scottem, autor wiele książek ASP/ASP.NET i założyciel 4GuysFromRolla.com, przy użyciu technologii Microsoft Web od 1998 r. Scott działa jako niezależny Konsultant, trainer i składnika zapisywania. Jego najnowszą książkę Stephena  *[Sams uczyć się ASP.NET 2.0 w ciągu 24 godzin](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)*. Scott można z Tobą skontaktować w [ mitchell@4guysfromrolla.com ](mailto:mitchell@4guysfromrolla.com) lub za pośrednictwem jego blog znajduje się na [ http://ScottOnWriting.NET ](http://scottonwriting.net/).

### <a name="special-thanks-to"></a>Specjalne podziękowania dla...

W tej serii samouczków został zrecenzowany przez wielu recenzentów pomocne. Zainteresowani zapoznaniem Moje kolejnych artykułów MSDN? Jeśli tak, Porzuć mnie linii w [ mitchell@4GuysFromRolla.com ](mailto:mitchell@4GuysFromRolla.com).

> [!div class="step-by-step"]
> [Poprzednie](user-based-authorization-cs.md)
> [dalej](creating-the-membership-schema-in-sql-server-vb.md)
