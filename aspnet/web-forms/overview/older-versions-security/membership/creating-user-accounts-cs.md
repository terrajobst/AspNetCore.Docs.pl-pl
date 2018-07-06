---
uid: web-forms/overview/older-versions-security/membership/creating-user-accounts-cs
title: Tworzenie kont użytkowników (C#) | Dokumentacja firmy Microsoft
author: rick-anderson
description: W tym samouczku omówimy będzie tworzenie nowych kont użytkowników przy użyciu struktury członkostwa (za pośrednictwem SqlMembershipProvider). Zobaczymy sposobu tworzenia nowych nam...
ms.author: aspnetcontent
ms.date: 01/18/2008
ms.assetid: f175278c-6079-4d91-b9b4-2493ed43d9ec
msc.legacyurl: /web-forms/overview/older-versions-security/membership/creating-user-accounts-cs
msc.type: authoredcontent
ms.openlocfilehash: 7caa6b614bbe6545929a9b201de7f30fe95e6481
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/05/2018
ms.locfileid: "37842627"
---
<a name="creating-user-accounts-c"></a>Tworzenie kont użytkowników (C#)
====================
przez [Bento Scott](https://twitter.com/ScottOnWriting)

[Pobierz program Code](http://download.microsoft.com/download/3/f/5/3f5a8605-c526-4b34-b3fd-a34167117633/ASPNET_Security_Tutorial_05_CS.zip) lub [Pobierz plik PDF](http://download.microsoft.com/download/3/f/5/3f5a8605-c526-4b34-b3fd-a34167117633/aspnet_tutorial05_CreatingUsers_cs.pdf)

> W tym samouczku omówimy będzie tworzenie nowych kont użytkowników przy użyciu struktury członkostwa (za pośrednictwem SqlMembershipProvider). Zobaczymy sposobu tworzenia nowych użytkowników, programowo i za pośrednictwem ASP. Kontrolki CreateUserWizard wbudowanej w sieci.


## <a name="introduction"></a>Wprowadzenie

W <a id="_msoanchor_1"> </a> [poprzedni Samouczek](creating-the-membership-schema-in-sql-server-cs.md) zainstalowanej schemat usług aplikacji w bazie danych, który dodano tabel, widoków i wymagane przez procedury składowane `SqlMembershipProvider` i `SqlRoleProvider`. Spowodowało to infrastruktura, będą potrzebne w pozostałej części samouczków w tej serii. W tym samouczku przeanalizujemy, przy użyciu struktury członkostwa (za pośrednictwem `SqlMembershipProvider`) do tworzenia nowych kont użytkowników. Zobaczymy sposobu tworzenia nowych użytkowników, programowo i za pośrednictwem ASP. Kontrolki CreateUserWizard wbudowanej w sieci.

Oprócz nauka tworzenia nowych kont użytkowników, firma Microsoft będzie również trzeba aktualizacji wersji demonstracyjnej witryny sieci Web, utworzyliśmy najpierw w *<a id="_msoanchor_2"> </a> [omówienie uwierzytelniania formularzy](../introduction/an-overview-of-forms-authentication-cs.md)* samouczek i następnie rozszerzone w  *<a id="https://www.asp.net/learn/security/tutorial-03-cs.aspx"> </a> Konfiguracja uwierzytelniania formularzy i Tematy zaawansowane* samouczka. Nasze demonstracyjnej aplikacji internetowej zawiera stronę logowania, która sprawdza poprawność poświadczeń użytkownika względem pary trwale zakodowane nazwy użytkownika i hasła. Ponadto `Global.asax` zawiera kod, który tworzy niestandardowy `IPrincipal` i `IIdentity` obiektów dla uwierzytelnionych użytkowników. Zaktualizujemy strony logowania do zweryfikowania poświadczeń użytkowników względem framework członkostwa i usunąć logiki niestandardowej jednostki i tożsamości.

Zaczynajmy!

## <a name="the-forms-authentication-and-membership-checklist"></a>Uwierzytelnianie formularzy i listę kontrolną członkostwa

Przed rozpoczęciem pracy z członkostwa w ramach możemy przyjrzeć się Przejrzyj ważne czynności, które przekierowaliśmy osiągnąć tego punktu. Korzystając z członkostwa w ramach `SqlMembershipProvider` w scenariuszu uwierzytelnianie oparte na formularzach, poniższe kroki należy wykonać przed ich wprowadzeniem funkcjonalności członkostwa w aplikacji sieci web:

1. **Włącz uwierzytelnianie oparte na formularzach.** Tak jak Omówiliśmy to w  *<a id="_msoanchor_4"> </a> [omówienie uwierzytelniania formularzy](../introduction/an-overview-of-forms-authentication-cs.md)*, uwierzytelnianie formularzy jest włączone, edytując `Web.config` i ustawienie `<authentication>` elementu `mode` atrybutu `Forms`. Za pomocą uwierzytelniania formularzy, włączone, każdego żądania przychodzącego jest sprawdzany pod kątem *biletu uwierzytelniania formularzy*, jeśli jest obecny, identyfikuje obiekt żądający.
2. **Schemat usług aplikacji należy dodać do odpowiedniej bazy danych.** Korzystając z `SqlMembershipProvider` musimy zainstalować schemat usług aplikacji z bazą danych. Zazwyczaj ten schemat jest dodawany do tej samej bazy danych, który zawiera model danych aplikacji. *<a id="_msoanchor_5"> </a> [Tworzenie schematu członkostwa w programie SQL Server](creating-the-membership-schema-in-sql-server-cs.md)* przyjrzano się przy użyciu samouczka `aspnet_regsql.exe` narzędzie, aby to zrobić.
3. **Dostosuj ustawienia aplikacji sieci Web, aby odwoływać się do bazy danych z kroku 2.** *Tworzenie schematu członkostwa w programie SQL Server* samouczku przedstawiono dwa sposoby konfigurowania aplikacji sieci web tak, aby `SqlMembershipProvider` użyć bazy danych wybrane w kroku 2: modyfikując `LocalSqlServer` nazwa parametrów połączenia; lub, dodając nowe zarejestrowanego dostawcy do listy dostawców w ramach członkostwa i Dostosowywanie tego nowego dostawcę do korzystania z bazy danych z kroku 2.

Podczas tworzenia aplikacji sieci web, która używa `SqlMembershipProvider` i uwierzytelniania opartego na formularzach, konieczne będzie wykonywać te trzy kroki przed użyciem `Membership` klasy lub kontrolek logowania programu ASP.NET w sieci Web. Ponieważ już wykonane następujące kroki w poprzednich samouczkach, jesteśmy gotowi rozpocząć korzystanie z w ramach członkostwa!

## <a name="step-1-adding-new-aspnet-pages"></a>Krok 1: Dodawanie nowych stron ASP.NET

W tym samouczku i dalej trzech firma Microsoft będzie można badanie różnych funkcji związanych z członkostwa i możliwości. Musimy szereg stron ASP.NET do zaimplementowania tematy dotyczące badania w tych samouczkach. Utwórzmy tych stron, a następnie plik mapy witryny `(Web.sitemap)`.

Rozpocznij od utworzenia nowego folderu w projekcie o nazwie `Membership`. Następnie dodaj pięć nowych stron ASP.NET do `Membership` folderze łączenia każdego stronę z `Site.master` strony wzorcowej. Nazwa strony:

- `CreatingUserAccounts.aspx`
- `UserBasedAuthorization.aspx`
- `EnhancedCreateUserWizard.aspx`
- `AdditionalUserInfo.aspx`
- `Guestbook.aspx`

Na tym etapie projektu w Eksploratorze rozwiązań powinny wyglądać podobnie do ekranu zrzut, jak pokazano na rysunku 1.


[![Pięć nowych stron zostały dodane do folderu członkostwa](creating-user-accounts-cs/_static/image2.png)](creating-user-accounts-cs/_static/image1.png)

**Rysunek 1**: pięć nowych stron zostały dodane do `Membership` Folder ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](creating-user-accounts-cs/_static/image3.png))


Każda strona w tym momencie powinna mieć dwóch kontrolek zawartości, jeden dla każdej strony wzorcowej kontrolek ContentPlaceHolder: `MainContent` i `LoginContent`.

[!code-aspx[Main](creating-user-accounts-cs/samples/sample1.aspx)]

Pamiętamy `LoginContent` firmy ContentPlaceHolder domyślne znaczników Wyświetla łącze do logowania się lub wylogowywania z lokacji, w zależności od tego, czy użytkownik jest uwierzytelniony. Obecność `Content2` formantu zawartości, jednak zastępuje znaczników domyślnej strony wzorcowej. Tak jak Omówiliśmy to w *<a id="_msoanchor_6"> </a> [omówienie uwierzytelniania formularzy](../introduction/an-overview-of-forms-authentication-cs.md)* samouczek, jest to przydatne na stronach, gdy nie chcemy wyświetlić opcje związane z logowaniem w lewej kolumnie.

Dla tych pięciu stron, chcemy wyświetlić znaczników domyślną stronę wzorcową dla `LoginContent` ContentPlaceHolder. W związku z tym, Usuń oznaczeniu deklaracyjnym dla `Content2` formantu zawartości. Po wykonaniu tej czynności, wszystkich znaczników pięć strony powinna zawierać tylko jeden formant zawartości.

## <a name="step-2-creating-the-site-map"></a>Krok 2: Tworzenie mapy witryny

Wszystkie z wyjątkiem najbardziej trivial witryn sieci Web konieczne zaimplementowanie pewnego rodzaju interfejs użytkownika nawigacji. Interfejs użytkownika nawigacji może być prosta lista łączy do różnych sekcji witryny. Alternatywnie te linki mogą rozmieszczone w menu lub widok drzewa. Jako programiści strony Tworzenie interfejsu użytkownika nawigacji to dopiero połowa wątku. Należy również niektóre oznacza, że do definiowania struktury logicznej lokacji w sposób łatwego w utrzymaniu i można aktualizować. Nowe strony są dodawane lub usuwane istniejących stron, chcemy można było zaktualizować pojedyncze źródło — Mapa witryny — i mają te modyfikacje zostaną uwzględnione na interfejs użytkownika nawigacji strony.

Te dwa zadania — definiujące mapowanie lokacji i implementującej interfejs użytkownika nawigacji, oparte na mapie witryny — jest łatwe dzięki rozłożeniu w ramach Mapa witryny, a następnie dodać formantów nawigacji w sieci Web na platformie ASP.NET w wersji 2.0. Zezwala na framework mapy witryny dla deweloperów do definiowania mapy witryny sieci Web i dostępu do niego za pomocą programowego interfejsu API ( [ `SiteMap` klasy](https://msdn.microsoft.com/library/system.web.sitemap.aspx)). Wbudowane kontrolki nawigacji w sieci Web obejmują [formant Menu](https://msdn.microsoft.com/library/bz09dy46.aspx), [kontrolki TreeView](https://msdn.microsoft.com/library/3eafky27.aspx)i [kontrolki ścieżki mapy witryny](https://msdn.microsoft.com/library/3eafky27.aspx).

Podobnie jak struktury członkostwa i ról kompilowanych framework mapy witryny [modelu dostawca](http://aspnet.4guysfromrolla.com/articles/101905-1.aspx). Zadanie klasy dostawcy mapy witryny jest do generowania struktury w pamięci używane przez `SiteMap` klasy w trwałym magazynie danych, takich jak plik XML lub tabeli bazy danych. .NET Framework, który jest dostarczany z domyślnego dostawcy mapy witryny, która odczytuje dane mapy witryny z pliku XML ([`XmlSiteMapProvider`](https://msdn.microsoft.com/library/system.web.xmlsitemapprovider.aspx)), i jest to dostawca, firma Microsoft będzie używana w tym samouczku. Niektóre alternatywnych implementacji dostawcy mapy witryny można znaleźć w sekcji dalsze odczyty na końcu tego samouczka.

Domyślnego dostawcy mapy witryny oczekuje, że poprawnie sformatowany plik XML o nazwie `Web.sitemap` istnieć katalogu głównego. Ponieważ używamy tego domyślnego dostawcę, należy dodać takiego pliku oraz zdefiniować strukturę mapy witryny w odpowiednim formacie XML. Aby dodać plik, kliknij prawym przyciskiem myszy nazwę projektu w Eksploratorze rozwiązań i wybierz polecenie Dodaj nowy element. W oknie dialogowym, zoptymalizowany pod kątem można dodać pliku typu mapy witryny o nazwie `Web.sitemap`.


[![Dodaj plik o nazwie Web.sitemap do katalogu głównego projektu](creating-user-accounts-cs/_static/image5.png)](creating-user-accounts-cs/_static/image4.png)

**Rysunek 2**: Dodaj plik o nazwie `Web.sitemap` do katalogu głównego projektu ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](creating-user-accounts-cs/_static/image6.png))


Plik mapy witryny XML definiuje strukturę witryny sieci Web jako hierarchia. To hierarchiczna relacja jest formowana w pliku XML przy użyciu pochodzenie elementu `<siteMapNode>` elementów. `Web.sitemap` Musi zaczynać się `<siteMap>` węzła nadrzędnego, który ma dokładnie jedno `<siteMapNode>` podrzędnych. Tym najwyższego poziomu `<siteMapNode>` element reprezentuje katalog główny hierarchii i może mieć dowolną liczbę węzłów podrzędnych. Każdy `<siteMapNode>` element musi zawierać `title` atrybutu i może opcjonalnie obejmować `url` i `description` atrybutów, między innymi; każdego niepustego `url` atrybutu musi być unikatowa.

Wprowadź następujący kod XML do `Web.sitemap` pliku:

[!code-xml[Main](creating-user-accounts-cs/samples/sample2.xml)]

Powyższe znaczników mapy witryny definiuje hierarchię pokazany na rysunku 3.


[![Mapa witryny reprezentuje hierarchicznej struktury nawigacyjnej](creating-user-accounts-cs/_static/image8.png)](creating-user-accounts-cs/_static/image7.png)

**Rysunek 3**: mapy witryny reprezentuje hierarchicznej struktury nawigacyjnej ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](creating-user-accounts-cs/_static/image9.png))


## <a name="step-3-updating-the-master-page-to-include-a-navigational-user-interface"></a>Krok 3: Aktualizowanie strony wzorcowej, aby uwzględnić interfejs użytkownika nawigacji

Program ASP.NET zawiera szereg związanych z nawigacją kontrolki sieci Web, dotyczące projektowania interfejsu użytkownika. Obejmują one Menu widoku drzewa i formanty SiteMapPath. Kontrolki Menu i TreeView renderowania struktury mapy witryny, w menu lub drzewo, odpowiednio, natomiast SiteMapPath Wyświetla nawigacji, który pokazuje bieżący węzeł odwiedzana oraz jego elementów nadrzędnych. Dane mapy witryny może być powiązana z innymi danymi, kontrolki sieci Web, przy użyciu SiteMapDataSource i można uzyskać programistycznie przy użyciu `SiteMap` klasy.

Ponieważ dogłębną dyskusję framework mapy witryny i formanty nawigacyjne wykracza poza zakres tej serii samouczków, przeciwnie niż poświęcać czasu na tworzeniu własnego interfejsu użytkownika nawigacji umożliwia zamiast tego "pożyczać" komentarzowi użytemu w mojej *[ Praca z danymi w programie ASP.NET 2.0](../../data-access/index.md)* serii samouczków, która korzysta z kontrolką elementu powtarzanego do wyświetlania listy punktowanej dwóch głębokiego łącza nawigacji, jak pokazano na rysunku 4.

### <a name="adding-a-two-level-list-of-links-in-the-left-column"></a>Dodawanie dwupoziomowej listę linków w kolumnie po lewej stronie

Aby utworzyć ten interfejs, Dodaj następujące oznaczeniu deklaracyjnym do `Site.master` strony wzorcowej lewą kolumnę gdzie tekst "TODO: Menu znajdzie się tutaj..." aktualnie znajduje się.

[!code-aspx[Main](creating-user-accounts-cs/samples/sample3.aspx)]

Powyższe znaczników wiąże kontrolką elementu powtarzanego o nazwie `menu` do SiteMapDataSource, która zwraca hierarchii mapy witryny, które są zdefiniowane w `Web.sitemap`. Od kontroli SiteMapDataSource [ `ShowStartingNode` właściwość](https://msdn.microsoft.com/library/system.web.ui.webcontrols.sitemapdatasource.showstartingnode.aspx) ma wartość False, uruchamia, zwracając hierarchii mapy witryny, rozpoczynając od elementów potomnych węzła "Home". Powtarzanego wyświetla każdy z tych węzłów (obecnie tylko "członkostwa") w `<li>` elementu. Inny, wewnętrzny elementu powtarzanego. następnie wyświetla elementy podrzędne bieżącego węzła w zagnieżdżonej listy nieuporządkowane.

Rysunek 4 przedstawia powyżej znaczników wyniku renderowania przy użyciu struktury mapy witryny, utworzonego w kroku 2. Powtarzanego renderuje znacznik wanilii listę nieuporządkowaną kaskadowe reguły arkusz stylów zdefiniowany w `Styles.css` jest odpowiedzialny za aesthetically atrakcyjne układu. Aby uzyskać bardziej szczegółowy opis sposobu działania powyżej znaczników, dotyczą [strony wzorcowe i nawigacja w witrynie](https://asp.net/learn/data-access/tutorial-03-cs.aspx) samouczka.


[![Nawigacyjne interfejsu użytkownika jest renderowany przy użyciu zagnieżdżonych nieuporządkowaną Wyświetla listę](creating-user-accounts-cs/_static/image11.png)](creating-user-accounts-cs/_static/image10.png)

**Rysunek 4**: nawigacyjne interfejs użytkownika jest renderowany przy użyciu zagnieżdżonych nieuporządkowaną Wyświetla listę ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](creating-user-accounts-cs/_static/image12.png))


### <a name="adding-breadcrumb-navigation"></a>Dodawanie do stron nadrzędnych

Oprócz listy linków w kolumnie po lewej stronie umożliwia również mieć każdy wyświetlania strony [łączy do stron nadrzędnych](http://en.wikipedia.org/wiki/Breadcrumb_%28navigation%29). Ślad jest element interfejsu użytkownika nawigacyjne, które szybko ułatwiają użytkownikom ich bieżącą pozycję w hierarchii lokacji. Kontrolki ścieżki mapy witryny używa framework Mapa witryny, aby określić lokalizację bieżącej strony mapy witryny, a następnie wyświetla ślad na podstawie tych informacji.

W szczególności dodać `<span>` elementu nagłówka strony wzorcowej `<div>` element i ustaw nowy `<span>` elementu `class` atrybutu "łączy do stron nadrzędnych". ( `Styles.css` Klasa zawiera regułę dla klasy "łączy do stron nadrzędnych".) Następnie dodaj ścieżki mapy witryny, w tym nowych `<span>` elementu.

[!code-aspx[Main](creating-user-accounts-cs/samples/sample4.aspx)]

Rysunek 5. pokazuje dane wyjściowe SiteMapPath podczas odwiedzania `~/Membership/CreatingUserAccounts.aspx`.


[![Łączy do stron nadrzędnych Wyświetla bieżącą stronę i zamapuj jego elementy nadrzędne w lokacji](creating-user-accounts-cs/_static/image14.png)](creating-user-accounts-cs/_static/image13.png)

**Rysunek 5**: łączach Wyświetla bieżącą stronę i jego elementy nadrzędne mapy witryny ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](creating-user-accounts-cs/_static/image15.png))


## <a name="step-4-removing-the-custom-principal-and-identity-logic"></a>Krok 4: Usuwanie jednostki niestandardowej i logiki tożsamości

W *<a id="_msoanchor_7"> </a> [Konfiguracja uwierzytelniania formularzy i Tematy zaawansowane](../introduction/forms-authentication-configuration-and-advanced-topics-cs.md)* samouczek widzieliśmy jak skojarzyć niestandardowe obiekty jednostki i tożsamość uwierzytelnionego użytkownika. Możemy to osiągnąć, tworząc program obsługi zdarzeń w `Global.asax` aplikacji `PostAuthenticateRequest` zdarzenie, które uruchamia się po `FormsAuthenticationModule` uwierzytelnił użytkownika. W tej obsługi zdarzeń zastąpiliśmy `GenericPrincipal` i `FormsIdentity` obiekty dodane przez `FormsAuthenticationModule` z `CustomPrincipal` i `CustomIdentity` obiektów utworzonych w tym samouczku.

Niestandardowe obiekty jednostki i tożsamość są przydatne w niektórych scenariuszach, w większości przypadków `GenericPrincipal` i `FormsIdentity` obiekty są wystarczające. W związku z tym myślę, że będzie się zastanowić, aby powrócić do domyślnego zachowania. Ta zmiana wprowadzona przez usunięcie lub zakomentowując `PostAuthenticateRequest` programu obsługi zdarzeń lub usuwając `Global.asax` pliku w całości.

## <a name="step-5-programmatically-creating-a-new-user"></a>Krok 5: Programowe tworzenie nowego użytkownika

Aby utworzyć nowe konto użytkownika przy użyciu framework członkostwa `Membership` klasy [ `CreateUser` metoda](https://msdn.microsoft.com/library/system.web.security.membership.createuser.aspx). Ta metoda ma wejściowych parametrów dla nazwy użytkownika, hasła i inne pola związane z użytkownikiem. Na wywołanie, deleguje tworzenia nowego konta użytkownika skonfigurowanego dostawcy członkostwa, a następnie zwraca [ `MembershipUser` obiektu](https://msdn.microsoft.com/library/system.web.security.membershipuser.aspx) reprezentujący konto nowo utworzoną użytkownika.

`CreateUser` Metoda ma cztery przeciążenia każdego akceptowanie innej liczby parametrów wejściowych:

- [`CreateUser(username, password)`](https://msdn.microsoft.com/library/d8t4h2es.aspx)
- [`CreateUser(username, password, email)`](https://msdn.microsoft.com/library/t8yy6w3h.aspx)
- [`CreateUser(username, password, email, passwordQuestion, passwordAnswer, isApproved, MembershipCreateStatus)`](https://msdn.microsoft.com/library/82xx2e62.aspx)
- [`CreateUser(username, password, email, passwordQuestion, passwordAnswer, isApproved, providerUserKey, MembershipCreateStatus)`](https://msdn.microsoft.com/library/ms152012.aspx)

Te cztery przeciążenia różnią się od ilości zbieranych informacji. Pierwsze przeciążenie, na przykład wymaga tylko nazwy użytkownika i hasła dla nowego konta użytkownika, drugi z nich wymaga również adres e-mail użytkownika.

Taka przeciążona funkcja sprawdza istnieje, ponieważ informacje wymagane do utworzenia nowego konta użytkownika zależy od ustawienia konfiguracji dostawcy członkostwa. W *<a id="_msoanchor_8"> </a> [tworzenie schematu członkostwa w programie SQL Server](creating-the-membership-schema-in-sql-server-cs.md)* zbadaliśmy Określanie ustawień konfiguracji dostawcy członkostwa w samouczku `Web.config`. Tabela 2 dołączone pełną listę ustawień konfiguracji.

Jednego takiego członkostwa dostawcy ustawienie konfiguracji, które ma wpływ na co `CreateUser` przeciążenia, które mogą być używane jest `requiresQuestionAndAnswer` ustawienie. Jeśli `requiresQuestionAndAnswer` ustawiono `true` (ustawienie domyślne), a następnie podczas tworzenia nowego konta użytkownika firma Microsoft Określ pytanie zabezpieczające i odpowiedzi. Te informacje są używane później, jeśli użytkownik potrzebuje do Resetowanie lub zmienianie swoich haseł. W szczególności w tym czasie są wyświetlane na pytanie zabezpieczające i ich wprowadzić poprawną odpowiedź, aby Resetowanie lub zmienianie swoich haseł. W związku z tym jeśli `requiresQuestionAndAnswer` ustawiono `true` , a następnie wywoływania jednej z dwóch pierwszych `CreateUser` przeciążenia powoduje wyjątek, ponieważ brakuje pytanie zabezpieczające i odpowiedzi. Ponieważ nasza aplikacja jest obecnie skonfigurowany do wymagają pytanie zabezpieczające i odpowiedzi, musimy użyć jednej z drugim dwa przeciążenia programowe tworzenie użytkownika.

Aby zilustrować, za pomocą `CreateUser` metody, Utwórzmy interfejsu użytkownika, w którym firma Microsoft monit o podanie ich nazwy, hasła, wiadomości e-mail i odpowiedź na pytanie zabezpieczające wstępnie zdefiniowane. Otwórz `CreatingUserAccounts.aspx` stronie `Membership` folderze i dodaj następujące kontrolki sieci Web do formantu zawartości:

- Pole tekstowe o nazwie `Username`
- Pole tekstowe o nazwie `Password`, którego `TextMode` właściwość jest ustawiona na `Password`
- Pole tekstowe o nazwie `Email`
- Etykieta o nazwie `SecurityQuestion` z jego `Text` właściwość wyczyszczone
- Pole tekstowe o nazwie `SecurityAnswer`
- Przycisk o nazwie `CreateAccountButton` którego tekst jest właściwością "Utwórz konto użytkownika"
- Formant etykiety o nazwie `CreateAccountResults` z jego `Text` właściwość wyczyszczone

W tym momencie ekran powinien wyglądać podobnie do ekranu zrzut, jak pokazano na rysunku 6.


[![Na stronie CreatingUserAccounts.aspx Dodaj różnych formantów sieci Web](creating-user-accounts-cs/_static/image17.png)](creating-user-accounts-cs/_static/image16.png)

**Rysunek 6**: dodać różne formanty Web `CreatingUserAccounts.aspx` strony ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](creating-user-accounts-cs/_static/image18.png))


`SecurityQuestion` Etykiety i `SecurityAnswer` pole tekstowe są przeznaczone do wyświetlania na pytanie zabezpieczające wstępnie zdefiniowanych i zbieranie odpowiedzi użytkownika. Należy pamiętać, że pytanie zabezpieczające i odpowiedzi są przechowywane na podstawie użytkownika według, można pozwolić każdemu użytkownikowi na definiowanie własnych pytanie zabezpieczające. Jednak w tym przykładzie, czy zamierzasz używać pytanie zabezpieczające uniwersalne, to znaczy: "Co to jest Twoje ulubione koloru?"

Aby zaimplementować to pytanie zabezpieczeń wstępnie zdefiniowane, należy dodać stałą do strony związanym z kodem klasę o nazwie `passwordQuestion`, przypisując go na pytanie zabezpieczające. Następnie w `Page_Load` programu obsługi zdarzeń, przypisać tym stała przeznaczona do `SecurityQuestion` etykiety `Text` właściwości:

[!code-csharp[Main](creating-user-accounts-cs/samples/sample5.cs)]

Następnie należy utworzyć program obsługi zdarzeń dla `CreateAccountButton`firmy `Click` zdarzeń i Dodaj następujący kod:

[!code-csharp[Main](creating-user-accounts-cs/samples/sample6.cs)]

`Click` Programu obsługi zdarzeń rozpoczyna się od zdefiniowania zmiennej o nazwie `createStatus` typu [ `MembershipCreateStatus` ](https://msdn.microsoft.com/library/system.web.security.membershipcreatestatus.aspx). `MembershipCreateStatus` to wyliczenie, które wskazuje stan `CreateUser` operacji. Na przykład, jeśli konto użytkownika został utworzony pomyślnie, wynikowy `MembershipCreateStatus` wystąpienia zostanie ustawiona na wartość `Success`; na drugim drugiej strony, jeśli operacja zakończy się niepowodzeniem, ponieważ istnieje już użytkownika przy użyciu tej samej nazwy użytkownika, zostanie ustawiona na wartość `DuplicateUserName`. W `CreateUser` przeciążenia używamy, należy przekazać `MembershipCreateStatus` wystąpienia do metody jako `out` parametru. Ten parametr ma wartość odpowiednią wartość w ramach `CreateUser` metody, a firma Microsoft, można sprawdzić jego wartość po wywołaniu metody, aby ustalić, czy konto użytkownika zostało pomyślnie utworzone.

Po wywołaniu `CreateUser`, przekazując `createStatus`, `switch` instrukcja jest używane w danych wyjściowych odpowiedni komunikat i w zależności od wartości przypisanej do `createStatus`. Rysunki 7 przedstawia dane wyjściowe, gdy nowy użytkownik został pomyślnie utworzony. Rysunki 8 i 9 pokazują dane wyjściowe, gdy konto użytkownika nie została utworzona. Na rysunku 8 odwiedzający wprowadzić hasło pięciu literę, który nie spełnia wymagania dotyczące siły hasła, które zostały zapisane w ustawieniach konfiguracji dostawcy członkostwa. Na rysunku 9 odwiedzający próbuje utworzyć konto użytkownika przy użyciu istniejącej nazwy użytkownika (utworzonym na rysunku 7).


[![Nowe konto użytkownika jest pomyślnie utworzony](creating-user-accounts-cs/_static/image20.png)](creating-user-accounts-cs/_static/image19.png)

**Rysunek 7**: nowe konto użytkownika jest pomyślnie utworzony ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](creating-user-accounts-cs/_static/image21.png))


[![Nie utworzono konta użytkownika, ponieważ podane hasło jest za słabe](creating-user-accounts-cs/_static/image23.png)](creating-user-accounts-cs/_static/image22.png)

**Rysunek 8**: nie utworzono konto użytkownika, ponieważ podane hasło jest za słabe ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](creating-user-accounts-cs/_static/image24.png))


[![To konto użytkownika nie utworzone, ponieważ nazwa jest już w użyciu](creating-user-accounts-cs/_static/image26.png)](creating-user-accounts-cs/_static/image25.png)

**Rysunek 9**: konto użytkownika jest nie utworzone, ponieważ nazwa jest już w użyciu ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](creating-user-accounts-cs/_static/image27.png))


> [!NOTE]
> Możesz się zastanawiać, jak określić powodzenie lub niepowodzenie, korzystając z jednej z dwóch pierwszych `CreateUser` ani przeciążenia metod programu, który ma parametr typu `MembershipCreateStatus`. Te pierwsze dwa przeciążenia throw [ `MembershipCreateUserException` wyjątek](https://msdn.microsoft.com/library/system.web.security.membershipcreateuserexception.aspx) także w przypadku awarii, co obejmuje [ `StatusCode` właściwość](https://msdn.microsoft.com/library/system.web.security.membershipcreateuserexception.statuscode.aspx) typu `MembershipCreateStatus`.


Po utworzeniu kilku kontom użytkowników, należy sprawdzić, czy konta zostały utworzone przez wymienienie zawartość `aspnet_Users` i `aspnet_Membership` tabelach `SecurityTutorials.mdf` bazy danych. Jak pokazano na rysunku nr 10, po dodaniu dwóch użytkowników za pośrednictwem `CreatingUserAccounts.aspx` strony: Tito i Bruce.


[![Istnieją dwaj użytkownicy w Store użytkownika członkostwa: Tito i Bruce](creating-user-accounts-cs/_static/image29.png)](creating-user-accounts-cs/_static/image28.png)

**Na rysunku nr 10**: istnieją dwaj użytkownicy w Store użytkownika członkostwa: Tito i Bruce ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](creating-user-accounts-cs/_static/image30.png))


Gdy magazyn użytkownika członkostwa zawiera teraz informacje o koncie Bruce i Tito firmy, mamy jeszcze do implementacji funkcji umożliwiającej Bruce lub Tito, aby zalogować się do witryny. Obecnie `Login.aspx` przeprowadza walidację poświadczeń użytkownika względem stały zestaw par nazwa użytkownika i hasło — robi *nie* sprawdzanie poprawności podanych poświadczeń względem framework członkostwa. Dla nowych kont użytkowników w teraz jest wyświetlany `aspnet_Users` i `aspnet_Membership` tabele będą miały się wystarczające. W następnym samouczku  *<a id="_msoanchor_9"> </a> [sprawdzania poprawności użytkownika poświadczeń względem członkostwa użytkownika Store](validating-user-credentials-against-the-membership-user-store-cs.md)*, zaktualizujemy strony logowania, aby przeprowadzić walidacji względem magazynu członkostwa.

> [!NOTE]
> Jeśli nie widzisz wszystkich użytkowników w Twojej `SecurityTutorials.mdf` bazy danych, może to być spowodowane aplikacją sieci web używa domyślnym dostawcą członkostwa `AspNetSqlMembershipProvider`, wykonującemu `ASPNETDB.mdf` baza danych jako jej magazyn użytkownika. Aby ustalić, czy ten problem, kliknij przycisk Odśwież w Eksploratorze rozwiązań. Jeśli baza danych o nazwie `ASPNETDB.mdf` została dodana do `App_Data` folderu, na tym polega problem. Wróć do kroku 4 *<a id="_msoanchor_10"> </a> [tworzenie schematu członkostwa w programie SQL Server](creating-the-membership-schema-in-sql-server-cs.md)* samouczek, w jaki sposób poprawnie skonfigurować dostawcy członkostwa.


W większości Utwórz użytkownika konta scenariuszy, odwiedzający są prezentowane za pomocą niektórych interfejsu, aby wprowadzić swoją nazwę użytkownika, hasło, adres e-mail i inne istotne informacje, w tym momencie jest tworzone nowe konto. W tym kroku będziemy przyjrzano ręcznego tworzenia takiego interfejsu i następnie pokazano, jak używać `Membership.CreateUser` metodę, aby programowo dodać nowe konto użytkownika oparte na danych wejściowych użytkownika. Nasz kod, jednak po prostu utworzyć nowe konto użytkownika. Nie wykonał wszelkich dalszych działań, takich jak logowanie użytkownika do witryny w ramach konta użytkownika z nowo utworzoną lub wysłanie wiadomości e-mail z potwierdzeniem do użytkownika. Te dodatkowe kroki wymaga dodatkowego kodu w przycisku `Click` programu obsługi zdarzeń.

ASP.NET jest dostarczany za pomocą kontrolki CreateUserWizard, który został zaprojektowany do obsługi procesu tworzenia konta użytkownika, z renderowania interfejsu użytkownika do tworzenia nowych kont użytkowników, do tworzenia konta w ramach członkostwa i wykonywania po konta Tworzenie zadania, takie jak wysyłanie wiadomości e-mail z potwierdzeniem i logowania użytkownika nowo utworzoną w lokacji. Używanie formantu CreateUserWizard jest tak proste, jak przeciągnięcie formantu CreateUserWizard z przybornika na stronie, a następnie ustawić kilka właściwości. W większości przypadków nie musisz pisać jednego wiersza kodu. Przeanalizujemy tej kontrolki pomysłowych szczegółowo w kroku 6.

Jeśli nowe konta użytkowników są tworzone tylko za pośrednictwem typowej stronie sieci web Utwórz konto, jest mało prawdopodobne, że nigdy nie należy napisać kod, który używa `CreateUser` metody jako formant CreateUserWizard będą prawdopodobnie odpowiadać potrzebom użytkownika. Jednak `CreateUser` metodą jest przydatne w scenariuszach, gdzie potrzebujesz wysoce dostosowanego środowiska użytkownika Utwórz konto lub konieczność programowe tworzenie nowych kont użytkowników za pomocą alternatywnych interfejsu. Na przykład Niewykluczone, że strona, która pozwala użytkownikowi na przesłanie pliku XML, który zawiera informacje o użytkowniku z innej aplikacji. Strona może być analizuje zawartość XML przekazano plik i utworzyć nowe konto dla każdego użytkownika reprezentowane w pliku XML, wywołując `CreateUser` metody.

## <a name="step-6-creating-a-new-user-with-the-createuserwizard-control"></a>Krok 6: Tworzenie nowego użytkownika za pomocą kontrolki CreateUserWizard

ASP.NET jest dostarczany z szeregu kontroli, zaloguj się w sieci Web. Te kontrolki pomocy w wielu typowych użytkownika skojarzone z kontem i logowaniem scenariuszy. [Kontroli CreateUserWizard](https://quickstarts.asp.net/QuickStartv20/aspnet/doc/ctrlref/login/createuserwizard.aspx) jest jeden formant, której celem jest prezentowanie interfejsu użytkownika do dodawania nowego konta użytkownika do struktury członkostwa.

Podobnie jak wiele innych formantów Web skojarzone z logowaniem CreateUserWizard można używać bez napisania choćby jednego wiersza kodu. Intuicyjnie udostępnia interfejs użytkownika, w oparciu o dostawcy członkostwa ustawienia konfiguracji i wywołuje wewnętrznie `Membership` klasy `CreateUser` metoda po użytkownik wprowadzi wymagane informacje i kliknie przycisk "Create User". Kontrolka CreateUserWizard jest bardzo można dostosowywać. Ma hosta zdarzeń, które są aktywowane podczas różnych etapach procesu tworzenia konta. Można utworzyć procedury obsługi zdarzeń, zgodnie z potrzebami, aby wstawić niestandardowej logiki do przepływu pracy tworzenia konta. Ponadto CreateUserWizard wygląd jest bardzo elastyczny. Istnieje wiele właściwości, które definiują wygląd domyślny interfejs; Jeśli to konieczne, kontrolki, można przekonwertować na szablon lub rejestracja użytkownika dodatkowe "kroki" mogą być dodawane.

Zacznijmy od przyjrzeć się przy użyciu domyślnego interfejsu i zachowanie kontrolki CreateUserWizard. Następnie pokażemy, jak dostosować wygląd za pośrednictwem właściwości i zdarzenia formantu.

### <a name="examining-the-createuserwizards-default-interface-and-behavior"></a>Badanie CreateUserWizard domyślny interfejs i zachowanie

Wróć do `CreatingUserAccounts.aspx` stronie `Membership` folderu, Przełącz do trybu projektowania lub podziału, a następnie dodaj formancie CreateUserWizard do górnej części strony. Kontrolka CreateUserWizard jest zachowane w sekcji Formanty logowania przybornika. Po dodaniu kontrolki, ustaw jego `ID` właściwość `RegisterUser`. Jak zrzut ekranu w przedstawia rysunek 11, CreateUserWizard renderuje interfejs z pól tekstowych dla nowego użytkownika nazwy użytkownika, hasło, adres e-mail i pytanie zabezpieczające i odpowiedzi.


[![Renderuje CreateUserWizard kontroli ogólnego Tworzenie interfejsu użytkownika](creating-user-accounts-cs/_static/image32.png)](creating-user-accounts-cs/_static/image31.png)

**Rysunek 11**: kontrolka CreateUserWizard renderuje ogólny Tworzenie interfejsu użytkownika ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](creating-user-accounts-cs/_static/image33.png))


Poświęćmy chwilę, aby porównać domyślnym interfejsem użytkownika generowany przez kontrolkę CreateUserWizard przy użyciu interfejsu, utworzonego w kroku 5. Po pierwsze kontroli CreateUserWizard umożliwia obiektu odwiedzającego określić pytanie zabezpieczające i odpowiedzi, interfejsu utworzone ręcznie stosować na pytanie zabezpieczające wstępnie zdefiniowane. Interfejsu kontroli CreateUserWizard obejmuje również kontrolkami walidacji, dlatego musimy jeszcze Implementowanie weryfikacji pól formularza naszego interfejsu. I interfejsu kontroli CreateUserWizard zawiera pole tekstowe "Potwierdź hasło" (wraz z CompareValidator aby upewnić się, że wprowadzony tekst "Password" i "Password porównania" pola tekstowe są takie same).

Ciekawe jest to formant CreateUserWizard powinni konsultować ustawienia konfiguracji dostawcy członkostwa, podczas renderowania interfejsu użytkownika. Na przykład zabezpieczenia pytanie i odpowiedź pola tekstowe są wyświetlane tylko jeśli `requiresQuestionAndAnswer` jest ustawiona na wartość True. Podobnie, CreateUserWizard automatycznie dodaje formant RegularExpressionValidator, aby zagwarantować, że są spełnione wymagania dotyczące siły hasła i ustawia jego `ErrorMessage` i `ValidationExpression` na podstawie właściwości `minRequiredPasswordLength`, `minRequiredNonalphanumericCharacters`i `passwordStrengthRegularExpression` ustawień konfiguracji.

Kontrolka CreateUserWizard jak sugeruje jej nazwa, jest tworzony na podstawie [formantu kreatora](https://msdn.microsoft.com/library/s2etd1ek.aspx). Kreator kontrolki są przeznaczone do zapewniają interfejs do wykonywania zadań z wieloma krokami. Kontrolka Kreator może mieć dowolną liczbę `WizardSteps`, z których każdy jest szablon, który definiuje HTML i formantów sieci Web, dla tego kroku. Kontrolka kreatora początkowo Wyświetla pierwszy `WizardStep`, wraz z formantów nawigacji, które zezwala na użytkownika, aby przejść od jednego kroku do następnego lub aby wrócić do poprzednich kroków.

Jak pokazano w oznaczeniu deklaracyjnym rysunek 11, kontrola CreateUserWizard domyślny interfejs zawiera dwie `WizardSteps:`

- [`CreateUserWizardStep`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizardstep.aspx) — renderuje interfejsu, aby zebrać informacje dotyczące tworzenia nowego konta użytkownika. Jest to krok pokazano na ilustracji 11.
- [`CompleteWizardStep`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.completewizardstep.aspx) — renderuje komunikat wskazujący, że konto zostało pomyślnie utworzone.

Wygląd i zachowanie CreateUserWizard może być modyfikowane, konwertując dowolną z następujących czynności w szablonach, lub przez dodanie własnych `WizardSteps`. Przyjrzymy się dodanie `WizardStep` interfejsu rejestracji w *przechowywanie dodatkowych informacji użytkownika* samouczka.

Zobaczmy, kontrola CreateUserWizard w działaniu. Odwiedź stronę `CreatingUserAccounts.aspx` strony za pośrednictwem przeglądarki. Rozpocznij, wprowadzając niektóre nieprawidłowe wartości do interfejsu CreateUserWizard. Spróbuj wprowadzić hasło, które nie są zgodne, wymagania dotyczące siły hasła lub opuścić "Nazwa użytkownika" pole tekstowe puste. CreateUserWizard wyświetli odpowiedni komunikat o błędzie. Rysunek 12 zawiera dane wyjściowe podczas próby utworzenia użytkownika z niewystarczająco silne hasło.


[![CreateUserWizard automatycznie wprowadza kontrolkami walidacji](creating-user-accounts-cs/_static/image35.png)](creating-user-accounts-cs/_static/image34.png)

**Rysunek 12**: CreateUserWizard automatycznie wprowadza kontrolkami walidacji ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](creating-user-accounts-cs/_static/image36.png))


Następnie wprowadź odpowiednie wartości w CreateUserWizard i kliknij przycisk "Create User". Zakładając, że wymagane pola zostały wprowadzone i siły hasła jest wystarczająca, CreateUserWizard będzie Utwórz nowe konto użytkownika za pośrednictwem framework członkostwa i następnie wyświetlać `CompleteWizardStep`(zobacz rysunek 13) na interfejsie użytkownika. W tle wywołuje CreateUserWizard `Membership.CreateUser` metodę, tak samo, jak Robiliśmy to krok 5.


[![Nowe konto użytkownika została pomyślnie utworzone](creating-user-accounts-cs/_static/image38.png)](creating-user-accounts-cs/_static/image37.png)

**Rysunek 13**: nowego konta użytkownika została pomyślnie utworzone ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](creating-user-accounts-cs/_static/image39.png))


> [!NOTE]
> Jak pokazano na rysunku 13, `CompleteWizardStep`przez interfejs zawiera przycisk Kontynuuj. Jednak w tym momencie klikając po prostu wykonuje zwrotu, pozostawiając odwiedzający na tej samej stronie. W sekcji "Dostosowywanie wyglądu i zachowania za pomocą jego właściwości CreateUserWizard" przedstawiony zostanie sposób mogą mieć tego przycisku wysłać użytkownika `Default.aspx` (lub innej strony).


Po utworzeniu nowego konta użytkownika, wróć do programu Visual Studio i sprawdź `aspnet_Users` i `aspnet_Membership` tabelami, takie jak zrobiliśmy na rysunku nr 10, aby sprawdzić, czy konto zostało pomyślnie utworzone.

### <a name="customizing-the-createuserwizards-behavior-and-appearance-through-its-properties"></a>Dostosowywanie zachowania i wyglądu za pośrednictwem jego właściwości CreateUserWizard

Na różne sposoby, za pomocą właściwości, można dostosować CreateUserWizard `WizardSteps`do programów obsługi zdarzeń. W tej sekcji przyjrzymy się jak dostosować wygląd formantu za pomocą jego właściwości; Następna sekcja patrzy na rozszerzanie zachowania formantu przy użyciu programów obsługi zdarzeń.

Praktycznie wszystkich tekstu wyświetlanego w interfejsie użytkownika domyślny formant CreateUserWizard można dostosować za pomocą jego mnóstwo właściwości. Na przykład "Nazwa użytkownika", "Password", "Potwierdź hasło", "Wiadomość E-mail", "Pytanie zabezpieczające" i "Zabezpieczającą" etykiety wyświetlany z lewej strony pól tekstowych można dostosowywać przez [ `UserNameLabelText` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.usernamelabeltext.aspx), [ `PasswordLabelText` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.passwordlabeltext.aspx), [ `ConfirmPasswordLabelText` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.confirmpasswordlabeltext.aspx), [ `EmailLabelText` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.emaillabeltext.aspx), [ `QuestionLabelText` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.questionlabeltext.aspx), i [ `AnswerLabelText` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.answerlabeltext.aspx)właściwości, odpowiednio. Podobnie, ma właściwości do określania tekstu, przycisków "Create User" i "Kontynuuj" w `CreateUserWizardStep` i `CompleteWizardStep`, jak również tak, jakby przyciski te są renderowane jako przyciski, LinkButtons lub ImageButtons.

Kolory, obramowania, czcionki i inne elementy wizualne mogą być konfigurowane przez host ponownego obliczenia właściwości stylu. Sama kontrolka CreateUserWizard ma typowe właściwości stylu formantu sieci Web — `BackColor`, `BorderStyle`, `CssClass`, `Font`i tak dalej — i kilka do definiowania wyglądu dla poszczególnych sekcji ponownego obliczenia właściwości stylu Interfejs CreateUserWizard firmy. [ `TextBoxStyle` Właściwość](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.textboxstyle.aspx), na przykład określa style dla pola tekstowe w `CreateUserWizardStep`, podczas [ `TitleTextStyle` właściwość](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.titletextstyle.aspx) definiuje styl tytułu ("Utwórz konto dla nowej usługi Konto").

Oprócz właściwości powiązane z wyglądem istnieje kilka właściwości, które mają wpływ na zachowanie kontroli CreateUserWizard. [ `DisplayCancelButton` Właściwości](https://msdn.microsoft.com/library/system.web.ui.webcontrols.wizard.displaycancelbutton.aspx), jeśli ustawiono wartość True, wyświetla przycisk Anuluj, obok przycisku "Utwórz użytkownika" (wartość domyślna to False). Jeśli przycisk Anuluj jest wyświetlany, należy również ustawić [ `CancelDestinationPageUrl` właściwość](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.continuedestinationpageurl.aspx), który określa stronę, użytkownik wysyłanymi do po kliknięciu przycisku Anuluj. Jak wspomniano w poprzedniej sekcji, a przycisk Kontynuuj w `CompleteWizardStep`przez interfejs, który powoduje odświeżenie strony, ale pozostawia odwiedzający na tej samej stronie. Aby wysłać użytkownika do innej strony po kliknięciu przycisku Kontynuuj, wystarczy określić adres URL w [ `ContinueDestinationPageUrl` właściwość](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.continuedestinationpageurl.aspx).

Zaktualizujmy `RegisterUser` kontroli CreateUserWizard Pokaż przycisk Anuluj i wysłać użytkownika `Default.aspx` po kliknięciu przycisku Anuluj, lub Kontynuuj. Aby to zrobić, należy ustawić `DisplayCancelButton` właściwości na wartość True, a oba `CancelDestinationPageUrl` i `ContinueDestinationPageUrl` właściwości "~ / Default.aspx". Rysunek 14 zawiera zaktualizowane CreateUserWizard podczas wyświetlania za pośrednictwem przeglądarki.


[![Element CreateUserWizardStep zawiera przycisk Anuluj](creating-user-accounts-cs/_static/image41.png)](creating-user-accounts-cs/_static/image40.png)

**Rysunek 14**: `CreateUserWizardStep` zawiera przycisk anulowania ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](creating-user-accounts-cs/_static/image42.png))


Po użytkownik wprowadza nazwę użytkownika, hasło, adres e-mail i pytanie zabezpieczające i odpowiedzi i kliknie przycisk "Create User", jest tworzone nowe konto użytkownika i użytkownik jest zalogowany jako nowo utworzone przez tego użytkownika. Przy założeniu, że osoby, odwiedzając stronę tworzy nowe konto dla siebie, to prawdopodobnie żądane zachowanie. Można jednak administratorzy mogą dodawać nowych kont użytkowników. W ten sposób będzie można utworzyć konto użytkownika, ale Administrator pozostanie zalogowany jako Administrator (a nie nowo utworzonych kont). To zachowanie można modyfikować za pomocą typu Boolean [ `LoginCreatedUser` właściwość](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.logincreateduser.aspx).

Konta użytkowników w ramach członkostwa zawierają zatwierdzonych flagę; Nie można zalogować się do witryny są użytkownicy, którzy nie zostały zatwierdzone. Domyślnie nowo utworzonego konta jest oznaczony jako zatwierdzone, umożliwiając użytkownikowi logowanie się do witryny natychmiast. Jest to możliwe, jest jednak mieć nowych kont użytkowników, oznaczone jako niezatwierdzonych. Na przykład możesz Administrator, aby ręcznie zatwierdzić nowych użytkowników, zanim mogą oni się zalogować. lub być może chcesz zweryfikować, że przed pozwalające użytkownikowi na logowanie jest prawidłowy adres e-mail wprowadzony podczas rejestracji. Niezależnie od przyczyny wyświetlenia, możesz mieć konto nowo utworzonego użytkownika, oznaczone jako niezatwierdzonych przez ustawienie sterowania CreateUserWizard [ `DisableCreatedUser` właściwość](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.disablecreateduser.aspx) na wartość True (wartość domyślna to False).

Inne właściwości związane z zachowaniem uwagi obejmują `AutoGeneratePassword` i `MailDefinition`. Jeśli [ `AutoGeneratePassword` właściwość](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.autogeneratepassword.aspx) jest ustawiona na wartość True, `CreateUserWizardStep` nie są wyświetlane pola tekstowe "Password" i "Potwierdź hasło"; zamiast tego nowo utworzone hasło użytkownika jest automatycznie generowana z użyciem `Membership` klasy [ `GeneratePassword` metoda](https://msdn.microsoft.com/library/system.web.security.membership.generatepassword.aspx). `GeneratePassword` Metoda konstrukcje hasła o określonej długości i z wystarczającą liczbą znaków innych niż alfanumeryczne w celu spełnienia wymagań siły skonfigurowanym hasłem.

[ `MailDefinition` Właściwość](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.maildefinition.aspx) jest przydatne, jeśli chcesz wysłać wiadomość e-mail na adres e-mail określony w trakcie procesu tworzenia konta. `MailDefinition` Właściwość zawiera szereg właściwości podrzędnych do definiowania informacji na temat wiadomości e-mail skonstruowany. Te właściwości obejmują opcje, takie jak `Subject`, `Priority`, `IsBodyHtml`, `From`, `CC`, i `BodyFileName`. [ `BodyFileName` Właściwość](https://msdn.microsoft.com/library/system.web.ui.webcontrols.maildefinition.bodyfilename.aspx) wskazuje tekst lub pliku HTML, który zawiera treść wiadomości e-mail. Treść obsługuje dwa wstępnie zdefiniowane symbole zastępcze: `<%UserName%>` i `<%Password%>`. Te symbole zastępcze, jeśli jest obecny w `BodyFileName` pliku, zostanie zastąpiony nowo utworzoną nazwę i hasło użytkownika.

> [!NOTE]
> `CreateUserWizard` Kontrolki `MailDefinition` właściwość określa tylko szczegóły dotyczące wiadomości e-mail, która jest wysyłana, gdy tworzone jest nowe konto. Nie zawiera wszystkie szczegółowe informacje dotyczące sposobu faktycznie zostanie wysłana wiadomość e-mail (oznacza to, czy jest używany katalog serwera lub przechowywania poczty usługi SMTP, wszelkie informacje uwierzytelniania i tak dalej). Te szczegóły niskiego poziomu, które muszą być zdefiniowane w `<system.net>` sekcji `Web.config`. Aby uzyskać więcej informacji na temat tych ustawień konfiguracji i wysyłanie wiadomości e-mail z programu ASP.NET 2.0, ogólnie rzecz biorąc, zobacz [— często zadawane pytania na SystemNetMail.com](http://www.systemnetmail.com/) i Moje artykułu [wysyłania wiadomości E-mail w programie ASP.NET 2.0](http://aspnet.4guysfromrolla.com/articles/072606-1.aspx).


### <a name="extending-the-createuserwizards-behavior-using-event-handlers"></a>Rozszerzanie zachowanie CreateUserWizard przy użyciu programów obsługi zdarzeń

Kontrolka CreateUserWizard podnosi liczbę zdarzeń, podczas jego przepływu pracy. Na przykład po użytkownik wprowadza swoją nazwę użytkownika, hasło i inne istotne informacje i kliknie przycisk "Create User", zgłasza kontroli CreateUserWizard jego [ `CreatingUser` zdarzeń](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.creatinguser.aspx). Jeśli występuje problem podczas procesu tworzenia [ `CreateUserError` zdarzeń](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.createusererror.aspx) wyzwoleniu; jednak, jeśli użytkownik została pomyślnie utworzona, a następnie [ `CreatedUser` zdarzeń](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.createduser.aspx) jest wywoływane. Istnieją dodatkowe zdarzenia kontroli CreateUserWizard, które pobieranie, ale są to trzy najbardziej istotnego znaczenia dla nich.

W niektórych scenariuszach może być chcemy akademickiej CreateUserWizard przepływu pracy, którą firma Microsoft może zrobić, tworząc program obsługi zdarzeń dla odpowiedniego zdarzenia. Na przykład możemy poprawić `RegisterUser` kontroli CreateUserWizard obejmujący niektóre niestandardowe sprawdzanie poprawności na nazwę użytkownika i hasło. W szczególności umożliwia zwiększenie naszych CreateUserWizard tak, aby nazwy użytkowników nie może zawierać spacji wiodących ani końcowych i nazwa użytkownika nie może występować w dowolnym miejscu w haśle. Krótko mówiąc chcemy zapobiec ktoś z tworzenia nazwę użytkownika, takich jak "Scott" lub masz kombinację nazwy użytkownika/hasła, takie jak "Scott" i "Scott.1234".

W tym utworzymy automatycznie zdarzenia obsługi dla `CreatingUser` zdarzenie, aby wykonać Nasze testy dodatkową walidację. Podane dane są nieprawidłowe potrzebujemy anulować procesu tworzenia. Należy również dodać kontrolkę etykiety w sieci Web do strony, aby wyświetlić komunikat wyjaśniający, że nazwa użytkownika lub hasło jest nieprawidłowe. Rozpocznij, dodając kontrolkę typu etykieta poniżej formantu CreateUserWizard, ustawiając jego `ID` właściwości `InvalidUserNameOrPasswordMessage` i jego `ForeColor` właściwość `Red`. Czyści jej `Text` właściwości i ustaw jego `EnableViewState` i `Visible` właściwości na wartość False.

[!code-aspx[Main](creating-user-accounts-cs/samples/sample7.aspx)]

Następnie należy utworzyć program obsługi zdarzeń dla formantu CreateUserWizard `CreatingUser` zdarzeń. Aby utworzyć program obsługi zdarzeń, wybierz formant w projektancie, a następnie przejdź do okna właściwości. W tym miejscu kliknij ikonę pioruna, a następnie kliknij dwukrotnie odpowiednie zdarzenie, aby utworzyć program obsługi zdarzeń.

Dodaj następujący kod do `CreatingUser` program obsługi zdarzeń:

[!code-csharp[Main](creating-user-accounts-cs/samples/sample8.cs)]

Należy pamiętać, że nazwa użytkownika i hasło wprowadzone w formancie CreateUserWizard są dostępne za pośrednictwem jego [ `UserName` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.username.aspx) i [ `Password` właściwości](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.password.aspx), odpowiednio. Używamy tych właściwości w powyższej procedury obsługi zdarzeń można określić, czy podana nazwa użytkownika zawiera spacji wiodących ani końcowych i tego, czy nazwa użytkownika znajduje się w obrębie hasło. Jeśli jeden z tych warunków jest spełniony, komunikat o błędzie jest wyświetlany w `InvalidUserNameOrPasswordMessage` etykiety i program obsługi zdarzeń `e.Cancel` właściwość jest ustawiona na `true`. Jeśli `e.Cancel` ustawiono `true`, CreateUserWizard short-circuits jego przepływu pracy, efektywnie anulowanie procesu tworzenia konta użytkownika.

Zrzut ekranu pokazuje, rysunek 15 `CreatingUserAccounts.aspx` po użytkownik wprowadza nazwę użytkownika, za pomocą spacji wiodących.


[![Nazwy użytkowników za pomocą wiodące lub końcowe spacje są niedozwolone.](creating-user-accounts-cs/_static/image44.png)](creating-user-accounts-cs/_static/image43.png)

**Rysunek 15**: nazw użytkowników z wiodące lub końcowe spacje nie są dozwolone ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](creating-user-accounts-cs/_static/image45.png))


> [!NOTE]
> Będzie można znaleźć przykład za pomocą kontroli CreateUserWizard `CreatedUser` zdarzenia w *<a id="_msoanchor_11"> </a> [przechowywanie dodatkowych informacji użytkownika](storing-additional-user-information-cs.md)* samouczka.


## <a name="summary"></a>Podsumowanie

`Membership` Klasy `CreateUser` metoda tworzy nowe konto użytkownika w ramach członkostwa. Robi to przez delegowanie wywołań do skonfigurowanego dostawcy członkostwa. W przypadku właściwości `SqlMembershipProvider`, `CreateUser` metoda dodaje rekord do `aspnet_Users` i `aspnet_Membership` bazy danych tabel.

Programowe (jak widzieliśmy w kroku 5) można tworzyć nowych kont użytkowników, szybciej i łatwiej podejścia polega na korzystanie z kontrolki CreateUserWizard. Ta kontrolka renderuje interfejs użytkownika wieloetapowego do zbierania informacji o użytkowniku i tworzenia nowego użytkownika w ramach członkostwa. Wewnętrznie ta kontrolka używa tych samych `Membership.CreateUser` metodzie jak badania w kroku 5, ale formant jest tworzony interfejs użytkownika, formanty sprawdzania poprawności i reaguje na błędy tworzenia konta użytkownika bez konieczności pisania lizawki kodu.

W tym momencie mamy funkcje, w celu tworzenia nowych kont użytkowników. Jednak na stronie logowania nadal trwa sprawdzanie poprawności względem tych zakodowanych poświadczeń, które określonej w drugim samouczku. W <a id="_msoanchor_12"> </a> [następnego samouczka](validating-user-credentials-against-the-membership-user-store-cs.md) zaktualizujemy `Login.aspx` sprawdza, czy użytkownik użytkownika podane poświadczenia w ramach członkostwa.

Wszystkiego najlepszego programowania!

### <a name="further-reading"></a>Dalsze informacje

Więcej informacji na tematów omówionych w tym samouczku można znaleźć w następujących zasobach:

- [`CreateUser` Dokumentacja techniczna](https://msdn.microsoft.com/library/system.web.security.membershipprovider.createuser.aspx)
- [CreateUserWizard informacje o formancie](https://quickstarts.asp.net/QuickStartv20/aspnet/doc/ctrlref/login/createuserwizard.aspx)
- [Tworzenie dostawcy mapy witryny opartej na systemie plików](http://aspnet.4guysfromrolla.com/articles/020106-1.aspx)
- [Tworzenie interfejsu użytkownika krok po kroku za pomocą kontrolki ASP.NET 2.0 Kreatora](http://aspnet.4guysfromrolla.com/articles/061406-1.aspx)
- [Badanie programu ASP.NET 2.0 w nawigacji po witrynie](http://aspnet.4guysfromrolla.com/articles/111605-1.aspx)
- [Strony wzorcowe i nawigacja w witrynie](https://asp.net/learn/data-access/tutorial-03-vb.aspx)
- [Dostawcy mapy witryny SQL oczekiwania dla](https://msdn.microsoft.com/msdnmag/issues/06/02/WickedCode/default.aspx)

### <a name="about-the-author"></a>Informacje o autorze

Pracował nad Bento Scottem, autor wiele książek ASP/ASP.NET i założyciel 4GuysFromRolla.com, przy użyciu technologii Microsoft Web od 1998 r. Scott działa jako niezależny Konsultant, trainer i składnika zapisywania. Jego najnowszą książkę Stephena  *[Sams uczyć się ASP.NET 2.0 w ciągu 24 godzin](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)*. Scott można z Tobą skontaktować w [ mitchell@4guysfromrolla.com ](mailto:mitchell@4guysfromrolla.com) lub za pośrednictwem jego blog znajduje się na [ http://ScottOnWriting.NET ](http://scottonwriting.net/).

### <a name="special-thanks-to"></a>Specjalne podziękowania dla...

W tej serii samouczków został zrecenzowany przez wielu recenzentów pomocne. Weryfikacja potencjalnych klientów w ramach tego samouczka został Teresa Murphy. Zainteresowani zapoznaniem Moje kolejnych artykułów MSDN? Jeśli tak, Porzuć mnie linii w [ mitchell@4GuysFromRolla.com ](mailto:mitchell@4GuysFromRolla.com).

> [!div class="step-by-step"]
> [Poprzednie](creating-the-membership-schema-in-sql-server-cs.md)
> [dalej](validating-user-credentials-against-the-membership-user-store-cs.md)
