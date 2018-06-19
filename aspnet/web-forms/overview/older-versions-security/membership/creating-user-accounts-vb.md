---
uid: web-forms/overview/older-versions-security/membership/creating-user-accounts-vb
title: Tworzenie kont użytkowników (VB) | Dokumentacja firmy Microsoft
author: rick-anderson
description: W tym samouczku przeanalizujemy, tworzenie nowych kont użytkowników przy użyciu członkostwa w ramach (za pośrednictwem SqlMembershipProvider). Należy sprawdzić, jak utworzyć nowe nam...
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/18/2008
ms.topic: article
ms.assetid: 9ef3e893-bebe-4b13-9fe5-8b71720dd85e
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-security/membership/creating-user-accounts-vb
msc.type: authoredcontent
ms.openlocfilehash: d665e7ba43401da76a88a904c10a587aa4576d4b
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/06/2018
ms.locfileid: "30892072"
---
<a name="creating-user-accounts-vb"></a>Tworzenie kont użytkowników (VB)
====================
przez [Bento Scott](https://twitter.com/ScottOnWriting)

[Pobierz kod](http://download.microsoft.com/download/3/f/5/3f5a8605-c526-4b34-b3fd-a34167117633/ASPNET_Security_Tutorial_05_VB.zip) lub [pobierania plików PDF](http://download.microsoft.com/download/3/f/5/3f5a8605-c526-4b34-b3fd-a34167117633/aspnet_tutorial05_CreatingUsers_vb.pdf)

> W tym samouczku przeanalizujemy, tworzenie nowych kont użytkowników przy użyciu członkostwa w ramach (za pośrednictwem SqlMembershipProvider). Przedstawiono sposób tworzenia nowych użytkowników programowo i za pośrednictwem ASP. NET przez wbudowane formancie CreateUserWizard.


## <a name="introduction"></a>Wprowadzenie

W <a id="_msoanchor_1"> </a> [poprzedniego samouczek](creating-the-membership-schema-in-sql-server-vb.md) zainstalowano schemat usług aplikacji w bazie danych, co dodać tabel, widoków i wymagane przez procedury składowane `SqlMembershipProvider` i `SqlRoleProvider`. To utworzyć infrastruktury, które są wymagane w pozostałej części w tej serii samouczków. W tym samouczku przeanalizujemy, przy użyciu platformy członkostwa (za pośrednictwem `SqlMembershipProvider`) do tworzenia nowych kont użytkowników. Przedstawiono sposób tworzenia nowych użytkowników programowo i za pośrednictwem ASP. NET przez wbudowane formancie CreateUserWizard.

Oprócz dowiedzieć, jak utworzyć nowe konta użytkowników, będą również należy zaktualizować pokaz witryny sieci Web, utworzyliśmy najpierw w *<a id="_msoanchor_2"> </a> [omówienie uwierzytelniania formularzy](../introduction/an-overview-of-forms-authentication-vb.md)* samouczek, a następnie rozszerzone w *<a id="_msoanchor_3"> </a> [konfiguracji uwierzytelniania formularzy i Tematy zaawansowane](../introduction/forms-authentication-configuration-and-advanced-topics-vb.md)* samouczka. Pokaz aplikacji sieci web ma strony logowania, która weryfikuje poświadczenia użytkowników przed pary ustalony nazwy użytkownika i hasła. Ponadto `Global.asax` zawiera kod, który tworzy niestandardowe `IPrincipal` i `IIdentity` obiektów dla uwierzytelnionych użytkowników. Aktualizujemy strony logowania do zweryfikowania poświadczeń użytkowników przed framework członkostwa i usunąć niestandardowej logiki podmiot zabezpieczeń i tożsamości.

Dzieła!

## <a name="the-forms-authentication-and-membership-checklist"></a>Uwierzytelnianie formularzy i Lista kontrolna członkostwa

Zanim zaczniemy pracy z członkostwa w ramach wytłumaczone przeczytaj ważne czynności, które przekierowaliśmy osiągnąć tego punktu. Korzystając z członkostwa w ramach `SqlMembershipProvider` w przypadku uwierzytelniania formularzy, należy wykonać przed Implementowanie funkcjonalności członkostwa w aplikacji sieci web następujące czynności:

1. **Włącz uwierzytelnianie oparte na formularzach.** Jak wspomniano w  *<a id="_msoanchor_4"> </a> [omówienie uwierzytelniania formularzy](../introduction/an-overview-of-forms-authentication-vb.md)*, uwierzytelnianie formularzy jest włączone, edytując `Web.config` i ustawienie `<authentication>` elementu `mode` atrybutu `Forms`. Z włączone uwierzytelnianie formularzy, każde żądanie przychodzące jest pod kątem *biletu uwierzytelniania formularzy*, jeśli jest obecny, identyfikuje obiekt żądający.
2. **Dodaj schemat usług aplikacji z odpowiednią bazą danych.** Korzystając z `SqlMembershipProvider` musimy zainstalować schemat usług aplikacji do bazy danych. Zazwyczaj ten schemat zostanie dodany do tej samej bazy danych, który zawiera model danych aplikacji. *<a id="_msoanchor_5"> </a> [Tworzenie schematu członkostwa w programie SQL Server](creating-the-membership-schema-in-sql-server-vb.md)* samouczek przeglądał przy użyciu `aspnet_regsql.exe` narzędzia w tym celu.
3. **Dostosowywanie ustawień aplikacji sieci Web, aby odwoływać się do bazy danych z kroku 2.** *Tworzenie schematu członkostwa w programie SQL Server* samouczku przedstawiono dwa sposoby konfigurowania aplikacji sieci web, aby `SqlMembershipProvider` użyje bazy danych wybranej w kroku 2: modyfikując `LocalSqlServer` Nazwa ciągu połączenia; lub dodanie nowego dostawcę zarejestrowanych do listy dostawców członkostwa framework i dostosowując tego nowego dostawcy na potrzeby używania bazy danych z kroku 2.

Podczas tworzenia aplikacji sieci web, która używa `SqlMembershipProvider` i uwierzytelnianie oparte na formularzach, konieczne będzie wykonywać te trzy kroki przed użyciem `Membership` klasy lub kontrolek logowania programu ASP.NET w sieci Web. Ponieważ już wykonane następujące kroki w poprzednim samouczki, możemy gotowe do rozpoczęcia przy użyciu platformy członkostwa!

## <a name="step-1-adding-new-aspnet-pages"></a>Krok 1: Dodawanie nowych stron ASP.NET

W tym samouczku i dalej trzech firma Microsoft będzie można badanie różne funkcje dotyczące członkostwa i możliwości. Szereg stron ASP.NET do zaimplementowania tematy zbadać w całym te samouczki są wymagane. Utwórz tych stron, a następnie plik mapy witryny `(Web.sitemap)`.

Rozpocznij od utworzenia nowego folderu do projektu o nazwie `Membership`. Następnie dodaj pięć nowych stron ASP.NET do `Membership` folderu łączenie każdej strony zawierającej `Site.master` strony wzorcowej. Nazwa strony:

- `CreatingUserAccounts.aspx`
- `UserBasedAuthorization.aspx`
- `EnhancedCreateUserWizard.aspx`
- `AdditionalUserInfo.aspx`
- `Guestbook.aspx`

W tym momencie projektu Eksploratora rozwiązań powinien wyglądać podobnie do ekranu zrzut pokazano na rysunku 1.


[![Dodano pięć nowych stron w folderze członkostwa](creating-user-accounts-vb/_static/image2.png)](creating-user-accounts-vb/_static/image1.png)

**Rysunek 1**: pięć nowych stron zostały dodane do `Membership` Folder ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](creating-user-accounts-vb/_static/image3.png))


Każdej strony w tym momencie powinna mieć dwóch formantów zawartości, jeden dla każdej strony wzorcowej Elementy ContentPlaceHolders: `MainContent` i `LoginContent`.

[!code-aspx[Main](creating-user-accounts-vb/samples/sample1.aspx)]

Odwołania, który `LoginContent` znaczników domyślnego elementu ContentPlaceHolder na zawiera łącze do logowania się lub wylogowywania lokacji, w zależności od tego, czy użytkownik jest uwierzytelniony. Obecność `Content2` formantu zawartości, jednak zastępuje znaczników domyślnej strony wzorcowej. Jak wspomniano w *<a id="_msoanchor_6"> </a> [omówienie uwierzytelniania formularzy](../introduction/an-overview-of-forms-authentication-vb.md)* samouczek, jest to przydatne w którym firma Microsoft nie ma być wyświetlane opcje związane z logowaniem w lewej kolumnie strony.

Dla tych pięć stron, jednak chęć pokazania znaczników domyślnej strony wzorcowej dla `LoginContent` ContentPlaceHolder. W związku z tym usunąć deklaratywne kod znaczników dla `Content2` zawartości formantu. Po wykonaniu tej czynności, każdej strony pięć znacznika powinna zawierać tylko jeden formant zawartości.

## <a name="step-2-creating-the-site-map"></a>Krok 2: Tworzenie mapy witryny

Wszystkie, oprócz najbardziej trivial witryn sieci Web muszą implementować jakiegoś interfejs użytkownika nawigacji. Interfejs użytkownika nawigacji może być to prosta lista łącza do różnych sekcji witryny. Alternatywnie można organizować te linki do menu lub widok drzewa. Jako strony deweloperów tworzenie interfejs użytkownika nawigacji jest tylko połowę wątku. Należy także niektóre oznacza, że do definiowania struktury logicznej witryny w sposób łatwy w obsłudze i można aktualizować. Nowe strony są dodawane lub usuwane istniejących stron, chcemy można było zaktualizować jednego źródła - mapy witryny - i mają te zmiany zostaną uwzględnione przez interfejs użytkownika nawigacji witryny.

Te dwa zadania — Definiowanie mapy witryny i implementującej interfejs użytkownika nawigacji, oparte na mapie witryny — można łatwo osiągnąć dzięki użyciu framework mapy witryny i sieci Web nawigacji formanty dodane w platformę ASP.NET w wersji 2.0. Umożliwia framework mapy witryny dla deweloperów zdefiniować mapy witryny sieci Web, a następnie do niej dostęp za pośrednictwem programowe interfejsu API ( [ `SiteMap` klasy](https://msdn.microsoft.com/library/system.web.sitemap.aspx)). Wbudowane formanty nawigacji w sieci Web obejmują [Menu sterowania](https://msdn.microsoft.com/library/bz09dy46.aspx), [formantu TreeView](https://msdn.microsoft.com/library/3eafky27.aspx)i [kontrolki ścieżki mapy witryny](https://msdn.microsoft.com/library/3eafky27.aspx).

Takie jak członkostwo i role struktur, została stworzona framework mapy witryny [modelu dostawcy](http://aspnet.4guysfromrolla.com/articles/101905-1.aspx). Zadanie klasa dostawcy mapy witryny polega na generowaniu struktury w pamięci używane przez `SiteMap` klasy z magazynu danych, takich jak plik XML lub tabeli bazy danych. .NET Framework jest dostarczany z domyślnego dostawcy mapy witryny, która odczytuje dane mapy witryny z pliku XML ([`XmlSiteMapProvider`](https://msdn.microsoft.com/library/system.web.xmlsitemapprovider.aspx)), a jest to dostawca użyjemy w tym samouczku. Niektóre alternatywnych implementacji dostawcy mapy witryny zapoznaj się z rozdziałem dalsze odczyty na końcu tego samouczka.

Domyślny dostawca mapy witryny oczekuje poprawnie sformatowany plik XML o nazwie `Web.sitemap` istnieje katalog główny. Ponieważ używamy tego domyślnego dostawcę, należy dodać taki plik i definiowania struktury mapy witryny w odpowiednim formacie XML. Aby dodać plik, kliknij prawym przyciskiem myszy nazwę projektu w Eksploratorze rozwiązań i wybierz polecenie Dodaj nowy element. W oknie dialogowym opt można dodać pliku mapy witryny o nazwie typu `Web.sitemap`.


[![Dodaj plik o nazwie Web.sitemap do katalogu głównego projektu](creating-user-accounts-vb/_static/image5.png)](creating-user-accounts-vb/_static/image4.png)

**Rysunek 2**: Dodaj plik o nazwie `Web.sitemap` do katalogu głównego projektu ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](creating-user-accounts-vb/_static/image6.png))


Plik mapy witryny XML definiuje strukturę witryny sieci Web jako hierarchia. Ta hierarchiczną relację w modelu w pliku XML za pomocą pochodzenia z `<siteMapNode>` elementów. `Web.sitemap` Musi rozpoczynać się od `<siteMap>` węzła nadrzędnego, który ma dokładnie jedno `<siteMapNode>` podrzędnych. Tym najwyższego poziomu `<siteMapNode>` — element reprezentuje element główny hierarchii i może mieć dowolną liczbę węzłów podrzędnych. Każdy `<siteMapNode>` musi zawierać element `title` atrybutu i opcjonalnie mogą obejmować `url` i `description` atrybuty, między innymi; każdego niepustym `url` atrybutu musi być unikatowa.

Wprowadź następujący kod XML w `Web.sitemap` pliku:

[!code-xml[Main](creating-user-accounts-vb/samples/sample2.xml)]

Powyżej znaczników mapy witryny definiuje hierarchii pokazany na rysunku 3.


[![Mapy witryny reprezentuje strukturę hierarchiczną nawigacji](creating-user-accounts-vb/_static/image8.png)](creating-user-accounts-vb/_static/image7.png)

**Rysunek 3**: mapy witryny reprezentuje strukturę hierarchiczną nawigacji ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](creating-user-accounts-vb/_static/image9.png))


## <a name="step-3-updating-the-master-page-to-include-a-navigational-user-interface"></a>Krok 3: Aktualizowanie strony wzorcowej, aby uwzględnić interfejs użytkownika nawigacji

Program ASP.NET zawiera szereg związanych z nawigacją formantów sieci Web podczas projektowania interfejsu użytkownika. Obejmują one Menu, TreeView i kontrolki ścieżki mapy witryny. Formanty TreeView i Menu renderowania struktury mapy witryny w menu lub drzewo, odpowiednio, natomiast ścieżki mapy witryny Wyświetla ślad, który pokazuje bieżący węzeł odwiedzana oraz jego obiektów nadrzędnych. Dane mapy witryny może być powiązana z innymi danymi formanty sieci Web przy użyciu SiteMapDataSource i można uzyskać programistycznie przy użyciu `SiteMap` klasy.

Ponieważ dogłębną dyskusję framework mapy witryny i formantów nawigacji wykracza poza zakres tego samouczka serii, zamiast niż poświęcić czas teraz obsługuje tworzenie własnej interfejs użytkownika nawigacji zamiast przekazania używaną w mojej *[ Praca z danymi w programie ASP.NET 2.0](../../data-access/index.md)* samouczka serii, używanym w kontrolce elementu powtarzanego do wyświetlania listy punktowanej dwa bezpośrednich łączy nawigacji, jak pokazano na rysunku 4.

### <a name="adding-a-two-level-list-of-links-in-the-left-column"></a>Dodawanie listy dwupoziomowej łączy w lewej kolumnie

Aby utworzyć ten interfejs, Dodaj następujący znacznik deklaratywne `Site.master` lewej strony wzorcowej formantu kolumny gdzie tekst TODO: Menu zostanie w tym miejscu... się teraz znajduje.

[!code-aspx[Main](creating-user-accounts-vb/samples/sample3.aspx)]

Powyżej znaczników wiąże kontrolce elementu powtarzanego o nazwie `menu` do SiteMapDataSource, która zwraca zdefiniowany w hierarchii mapy witryny `Web.sitemap`. Ponieważ formant SiteMapDataSource [ `ShowStartingNode` właściwość](https://msdn.microsoft.com/library/system.web.ui.webcontrols.sitemapdatasource.showstartingnode.aspx) ma wartość False, uruchamia, zwracając hierarchii mapy witryny, począwszy od elementów podrzędnych węzła głównej. Wyświetla powtarzanego każdego z tych węzłów (obecnie tylko członkostwa) w `<li>` elementu. Inny, wewnętrzny elementu powtarzanego. następnie wyświetla elementy podrzędne bieżącego węzła w nieuporządkowaną listę zagnieżdżoną.

Na rysunku 4 przedstawiono powyżej znaczników przetworzonych wyników ze strukturą mapy witryny utworzone w kroku 2. Powtarzanego renderuje podstawowego nieuporządkowaną listę znaczników; reguły arkusza stylów kaskadowych zdefiniowane w `Styles.css` są odpowiedzialne za aesthetically czytelnych układu. Aby uzyskać bardziej szczegółowy opis działania powyżej znaczników, zapoznaj się [stron wzorcowych i nawigacji w witrynie](https://asp.net/learn/data-access/tutorial-03-vb.aspx) samouczka.


[![Nawigacyjne interfejs użytkownika jest renderowany przy użyciu zagnieżdżone nieuporządkowaną listy](creating-user-accounts-vb/_static/image11.png)](creating-user-accounts-vb/_static/image10.png)

**Rysunek 4**: interfejs użytkownika nawigacji jest renderowany przy użyciu zagnieżdżone nieuporządkowaną wymieniono ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](creating-user-accounts-vb/_static/image12.png))


### <a name="adding-breadcrumb-navigation"></a>Dodawanie nadrzędnych

Oprócz listy łącza w lewej kolumnie, umożliwia również mieć wyświetlania każdej strony [stron nadrzędnych](http://en.wikipedia.org/wiki/Breadcrumb_%28navigation%29). Ślad jest elementem interfejsu użytkownika nawigacji, pokazujący szybko użytkownikom ich bieżącą pozycję w hierarchii lokacji. Kontrolki ścieżki mapy witryny używa framework mapy witryny w celu określenia lokalizacji bieżącej strony mapy witryny, a następnie wyświetla ślad na podstawie tych informacji.

W szczególności dodać `<span>` element do nagłówka strony wzorcowej `<div>` element i ustaw nowy `<span>` elementu `class` atrybutu stron nadrzędnych. ( `Styles.css` Klasa zawiera reguły dla klas nadrzędnych.) Następnie dodaj ścieżki mapy witryny w tym nowych `<span>` elementu.

[!code-aspx[Main](creating-user-accounts-vb/samples/sample4.aspx)]

Rysunek 5. pokazuje wyniki ścieżki mapy witryny podczas odwiedzania `~/Membership/CreatingUserAccounts.aspx`.


[![Łączach Wyświetla bieżącą stronę i zamapuj jego elementy nadrzędne w lokacji](creating-user-accounts-vb/_static/image14.png)](creating-user-accounts-vb/_static/image13.png)

**Rysunek 5**: łączach Wyświetla bieżącą stronę i jego elementy nadrzędne mapy witryny ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](creating-user-accounts-vb/_static/image15.png))


## <a name="step-4-removing-the-custom-principal-and-identity-logic"></a>Krok 4: Usunięcie niestandardowego podmiotu i logiki tożsamości

W *<a id="_msoanchor_7"> </a> [konfiguracji uwierzytelniania formularzy i Tematy zaawansowane](../introduction/forms-authentication-configuration-and-advanced-topics-vb.md)* samouczek widzieliśmy jak skojarzyć niestandardowe obiekty podmiot zabezpieczeń i tożsamość uwierzytelnionego użytkownika. Firma Microsoft to realizowane przez tworzenie obsługi zdarzeń w `Global.asax` dla aplikacji `PostAuthenticateRequest` zdarzenie, które są generowane po `FormsAuthenticationModule` uwierzytelnieniu użytkownika. W tej obsłudze zdarzeń możemy zastąpić `GenericPrincipal` i `FormsIdentity` obiektów dodanych przez `FormsAuthenticationModule` z `CustomPrincipal` i `CustomIdentity` obiekty utworzyliśmy w tym samouczku.

Niestandardowe obiekty Principal role i tożsamości są przydatne w niektórych scenariuszach, w większości przypadków `GenericPrincipal` i `FormsIdentity` obiekty są wystarczające. W rezultacie myślę, że będzie zastanowić, aby powrócić do domyślnego zachowania. To zrobić przez usunięcie lub komentarzy limit `PostAuthenticateRequest` obsługi zdarzeń lub usuwając `Global.asax` całkowicie pliku.

> [!NOTE]
> Po zostały oznaczone jako lub usunięty przez kod `Global.asax`, należy przekształcić w komentarz kod w `Default.aspx's` związane z kodem klasę, która rzutuje `User.Identity` właściwości `CustomIdentity` wystąpienia.


## <a name="step-5-programmatically-creating-a-new-user"></a>Krok 5: Programowe tworzenie nowego użytkownika

Aby utworzyć nowe konto użytkownika przy użyciu framework członkostwa `Membership` klasy [ `CreateUser` metody](https://msdn.microsoft.com/library/system.web.security.membership.createuser.aspx). Ta metoda ma wejściowych parametry dla nazwy użytkownika, hasła i inne pola dotyczące użytkowników. Na wywołanie, deleguje Tworzenie nowego konta użytkownika do skonfigurowanego dostawcy członkostwa, a następnie zwraca [ `MembershipUser` obiektu](https://msdn.microsoft.com/library/system.web.security.membershipuser.aspx) reprezentujący właśnie utworzonego konta.

`CreateUser` Metoda ma cztery przeciążenia każdego akceptowanie różne liczby parametrów wejściowych:

- [`CreateUser(username, password)`](https://msdn.microsoft.com/library/d8t4h2es.aspx)
- [`CreateUser(username, password, email)`](https://msdn.microsoft.com/library/t8yy6w3h.aspx)
- [`CreateUser(username, password, email, passwordQuestion, passwordAnswer, isApproved, MembershipCreateStatus)`](https://msdn.microsoft.com/library/82xx2e62.aspx)
- [`CreateUser(username, password, email, passwordQuestion, passwordAnswer, isApproved, providerUserKey, MembershipCreateStatus)`](https://msdn.microsoft.com/library/ms152012.aspx)

Te cztery przeciążenia różnią się ilości zbieranych informacji. Pierwszy przeciążenia wymaga na przykład tylko nazwę użytkownika i hasło dla nowego konta użytkownika, a drugi wymaga również adres e-mail użytkownika.

Te przeciążenia istnieje, ponieważ informacje wymagane do utworzenia nowego konta użytkownika zależy od ustawień konfiguracji dostawcy członkostwa. W *<a id="_msoanchor_8"> </a> [tworzenie schematu członkostwa w programie SQL Server](creating-the-membership-schema-in-sql-server-vb.md)* możemy zbadać określania ustawień konfiguracji dostawcy członkostwa w samouczku `Web.config`. Tabela 2 uwzględnione pełną listę ustawień konfiguracyjnych.

Jeden taki członkostwa dostawcy ustawienie konfiguracji, które ma wpływ na co `CreateUser` przeciążenia, które mogą być używane jest `requiresQuestionAndAnswer` ustawienie. Jeśli `requiresQuestionAndAnswer` ma ustawioną wartość `true` (ustawienie domyślne), a następnie podczas tworzenia nowego konta użytkownika firma Microsoft Wprowadź pytanie zabezpieczające i odpowiedzi. Te informacje są używane później, jeśli użytkownik musi zresetować lub zmienić swoje hasło. W szczególności w tym czasie są wyświetlane na pytanie zabezpieczające i wprowadzić poprawną odpowiedź w celu resetowania lub zmiany hasła. W związku z tym jeśli `requiresQuestionAndAnswer` ustawiono `true` , a następnie podczas wywoływania jednej z dwóch pierwszych `CreateUser` overloads powoduje wyjątek, ponieważ brakuje pytanie zabezpieczające i odpowiedzi. Ponieważ naszej aplikacji jest obecnie skonfigurowany do wymagają pytanie zabezpieczające i odpowiedzi, możemy należy użyć jednej z ostatnie dwa przeciążenia programowe tworzenie użytkownika.

Aby zilustrować przy użyciu `CreateUser` metody, Utwórzmy interfejsu użytkownika, w którym firma Microsoft monit o podanie nazwy, hasła, poczty e-mail i odpowiedzi na pytanie zabezpieczające wstępnie zdefiniowane. Otwórz `CreatingUserAccounts.aspx` strony `Membership` folderu i dodaj następujące formantów sieci Web do kontroli zawartości:

- Pole tekstowe o nazwie `Username`
- Pole tekstowe o nazwie `Password`, których `TextMode` ma ustawioną wartość właściwości `Password`
- Pole tekstowe o nazwie `Email`
- Etykieta o nazwie `SecurityQuestion` z jego `Text` wyczyścić właściwości
- Pole tekstowe o nazwie `SecurityAnswer`
- Przycisk o nazwie `CreateAccountButton` którego `Text` właściwość jest ustawiona na tworzenie konta użytkownika
- Etykieta o nazwie `CreateAccountResults` z jego `Text` wyczyścić właściwości

W tym momencie ekranie powinien wyglądać podobnie do ekranu zrzut pokazano na rysunku 6.


[![Na stronie CreatingUserAccounts.aspx Dodaj różnych formantów sieci Web](creating-user-accounts-vb/_static/image17.png)](creating-user-accounts-vb/_static/image16.png)

**Rysunek 6**: Dodaj formanty sieci Web różnych `CreatingUserAccounts.aspx Page` ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](creating-user-accounts-vb/_static/image18.png))


`SecurityQuestion` Etykiety i `SecurityAnswer` pole tekstowe są przeznaczone do wyświetlenia pytanie zabezpieczające wstępnie zdefiniowane i zbieranie odpowiedzi użytkownika. Należy pamiętać, że zarówno pytanie i odpowiedź zabezpieczeń są przechowywane na zasadzie użytkownika według, można pozwolić każdemu użytkownikowi na definiowanie własnych pytanie zabezpieczające. Jednak w tym przykładzie, I zdecydował się użyć pytanie zabezpieczające uniwersalnego, to znaczy: co to jest Twoje ulubione kolor?

Aby zaimplementować to pytanie zabezpieczeń wstępnie zdefiniowane, należy dodać stałą do strony związane z kodem klasę o nazwie `passwordQuestion`, przypisywania go na pytanie zabezpieczające. Następnie w `Page_Load` obsługi zdarzeń, przypisz tym stała przeznaczona do `SecurityQuestion` etykiety `Text` właściwości:

[!code-vb[Main](creating-user-accounts-vb/samples/sample5.vb)]

Następnie należy utworzyć programu obsługi zdarzeń dla `CreateAccountButton'` s `Click` zdarzeń i Dodaj następujący kod:

[!code-vb[Main](creating-user-accounts-vb/samples/sample6.vb)]

`Click` Uruchamia program obsługi zdarzeń, definiując zmiennej o nazwie `createStatus` typu [ `MembershipCreateStatus` ](https://msdn.microsoft.com/library/system.web.security.membershipcreatestatus.aspx). `MembershipCreateStatus` to wyliczenie, które wskazuje stan `CreateUser` operacji. Na przykład, jeśli konto użytkownika jest została pomyślnie utworzona, powstałe w ten sposób `MembershipCreateStatus` wystąpienie zostanie ustawiona na wartość `Success;` z drugiej strony, jeśli operacja nie powiedzie się, ponieważ już istnieje użytkownik o tej samej nazwy użytkownika, zostanie ustawiona na wartość `DuplicateUserName`. W `CreateUser` przeciążenia używamy, należy przekazać `MembershipCreateStatus` wystąpienie do metody. Ten parametr ma wartość odpowiednią wartość w `CreateUser` — metoda i firma Microsoft, można sprawdzić jego wartość po wywołaniu metody do określenia, czy konto użytkownika zostało pomyślnie utworzone.

Po wywołaniu `CreateUser`, przekazując `createStatus`, `Select Case` instrukcji jest używany do wypełniania wyjściowego odpowiedni komunikat w zależności od wartość przypisana do `createStatus`. Rysunki 7 przedstawiono dane wyjściowe, gdy nowy użytkownik został pomyślnie utworzony. Rysunki 8 do 9 wyświetlić dane wyjściowe, gdy konto użytkownika nie jest tworzone. Na rysunku 8 obiekt odwiedzający wprowadzone hasło pięciu litery, który nie spełnia wymagania dotyczące siły hasła, które zostały zapisane w ustawienia konfiguracji dostawcy członkostwa. Na rysunku nr 9 użytkownik próbuje utworzyć konto użytkownika z istniejącą nazwą użytkownika (utworzonym na rysunku 7).


[![Nowe konto użytkownika jest pomyślnie utworzony](creating-user-accounts-vb/_static/image20.png)](creating-user-accounts-vb/_static/image19.png)

**Rysunek 7**: A nowego konta użytkownika jest pomyślnie utworzony ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](creating-user-accounts-vb/_static/image21.png))


[![Nie utworzono konta użytkownika, ponieważ podane hasło jest zbyt słabe](creating-user-accounts-vb/_static/image23.png)](creating-user-accounts-vb/_static/image22.png)

**Rysunek 8**: nie utworzono konto użytkownika, ponieważ podane hasło jest zbyt słabe ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](creating-user-accounts-vb/_static/image24.png))


[![To konto użytkownika nie utworzone, ponieważ nazwa użytkownika jest już w użyciu](creating-user-accounts-vb/_static/image26.png)](creating-user-accounts-vb/_static/image25.png)

**Rysunek 9**: konto użytkownika jest nie utworzone, ponieważ nazwa użytkownika jest już w użyciu ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](creating-user-accounts-vb/_static/image27.png))


> [!NOTE]
> Możesz się zastanawiać, jak do ustalenia powodzenia lub niepowodzenia przy użyciu jednej z dwóch pierwszych `CreateUser` metoda przeciąża ani z którego ma parametr typu `MembershipCreateStatus`. Throw tych dwóch pierwszych przeciążenia [ `MembershipCreateUserException` wyjątek](https://msdn.microsoft.com/library/system.web.security.membershipcreateuserexception.aspx) w wypadku awarii, która obejmuje [ `StatusCode` właściwości](https://msdn.microsoft.com/library/system.web.security.membershipcreateuserexception.statuscode.aspx) typu `MembershipCreateStatus`.


Po utworzeniu kilka kont użytkowników, sprawdź, czy konta zostały utworzone przez wyświetlania zawartości `aspnet_Users` i `aspnet_Membership` tabelach `SecurityTutorials.mdf` bazy danych. Jak pokazano na rysunku nr 10, po dodaniu dwóch użytkowników za pośrednictwem `CreatingUserAccounts.aspx` strony: Tito i Bruce.


[![Istnieją dwa użytkowników w magazynie użytkownika członkostwa: Tito i Bruce](creating-user-accounts-vb/_static/image29.png)](creating-user-accounts-vb/_static/image28.png)

**Na rysunku nr 10**: Brak dwóch użytkowników w magazynie użytkownika członkostwa: Tito i Bruce ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](creating-user-accounts-vb/_static/image30.png))


Gdy magazynie członkostwa użytkownika zawiera teraz informacje o koncie Bruce i jego Tito, musimy jeszcze implementuje funkcje, które umożliwia Bruce lub Tito do logowania się do witryny. Obecnie `Login.aspx` sprawdza poprawność poświadczeń użytkownika przed ustalony zbiór par nazwa użytkownika i hasło — robi *nie* sprawdzanie poprawności podanych poświadczeń przed framework członkostwa. Przy przeglądaniu teraz nowych kont użytkowników w `aspnet_Users` i `aspnet_Membership` tabel będzie musiał wystarczające. W następnym samouczku  *<a id="_msoanchor_9"> </a> [sprawdzania poprawności użytkownika poświadczeń przed członkostwa użytkownika przechowywania](validating-user-credentials-against-the-membership-user-store-vb.md)*, firma Microsoft będzie aktualizować strony logowania na potrzeby sprawdzania poprawności magazynu członkostwa.

> [!NOTE]
> Jeśli nie ma żadnych użytkowników w Twojej `SecurityTutorials.mdf` bazy danych, może to być spowodowane aplikacji sieci web używa domyślnego dostawcę członkostwa `AspNetSqlMembershipProvider`, który korzysta z `ASPNETDB.mdf` bazy danych jako jego magazynu użytkownika. Aby ustalić, czy na tym polega problem, kliknij przycisk Odśwież w Eksploratorze rozwiązań. Jeśli w bazie danych o nazwie `ASPNETDB.mdf` został dodany do `App_Data` folderu, na tym polega problem. Wróć do kroku 4 *<a id="_msoanchor_10"> </a> [tworzenie schematu członkostwa w programie SQL Server](creating-the-membership-schema-in-sql-server-vb.md)* samouczek, w jaki sposób poprawnie skonfigurować dostawcę członkostwa.


W większości Utwórz użytkownika scenariusze konta, użytkownik zobaczy niektórych interfejsu, aby wprowadzić swoją nazwę użytkownika, hasło, poczty e-mail i inne ważne informacje, w którym utworzono nowe konto. W tym kroku będziemy przeglądał ręcznego tworzenia takiego interfejsu i następnie pokazano, jak używać `Membership.CreateUser` metoda programowo dodać nowe konto użytkownika oparta na danych wprowadzonych przez użytkownika. Naszego kodu, jednak tylko utworzyć nowe konto użytkownika. Nie wykonał wszelkich dalszych akcji, takich jak logowanie użytkownika do witryny przy użyciu konta użytkownika właśnie utworzony lub wysyłania do użytkownika wiadomość e-mail z potwierdzeniem. Te dodatkowe kroki wymagają dodatkowy kod w przycisku `Click` obsługi zdarzeń.

Program ASP.NET jest dostarczany z formancie CreateUserWizard, przeznaczoną do obsługi procesu tworzenia konta użytkownika, z renderowania interfejs użytkownika służący do tworzenia nowych kont użytkowników, do tworzenia konta w ramach członkostwa do wykonania po konta Tworzenie zadań, takich jak wysyła wiadomość e-mail z potwierdzeniem i logowania użytkownika właśnie utworzony w lokacji. Używanie w formancie CreateUserWizard jest tak proste, jak przeciąganie formancie CreateUserWizard z przybornika do strony, a następnie ustawić kilka właściwości. W większości przypadków nie musisz zapisywać pojedynczy wiersz kodu. Przeanalizujemy tego formantu pomysłowych szczegółowo w kroku 6.

Jeśli nowe konta użytkowników są tworzone tylko za pośrednictwem typowej strony sieci web Utwórz konto, jest mało prawdopodobne, że będzie trzeba napisać kod, który używa `CreateUser` metody jako formancie CreateUserWizard będzie prawdopodobnie odpowiadają Twoim potrzebom. Jednak `CreateUser` metoda jest przydatne w scenariuszach wymagających wysoce dostosowane środowisko użytkownika Utwórz konto lub konieczność programowane tworzenie nowych kont użytkowników za pomocą interfejsu alternatywnych. Na przykład może być strona, która umożliwia użytkownikom przekazywanie pliku XML, który zawiera informacje o użytkowniku z innej aplikacji. Strona może analizuje zawartość XML przekazanego pliku i utworzyć nowe konto dla każdego użytkownika reprezentowane w kodzie XML przez wywołanie metody `CreateUser` metody.

## <a name="step-6-creating-a-new-user-with-the-createuserwizard-control"></a>Krok 6: Tworzenie nowego użytkownika z formancie CreateUserWizard

Program ASP.NET jest dostarczany z kilku formantów sieci Web logowania. Formanty pomocy w wielu typowych użytkownika dotyczące kont i logowania scenariuszy. [Formancie CreateUserWizard](https://quickstarts.asp.net/QuickStartv20/aspnet/doc/ctrlref/login/createuserwizard.aspx) jest jeden formant, przeznaczony do prezentowania interfejs użytkownika umożliwiający dodanie nowego konta użytkownika do struktury członkostwa.

Podobnie jak wiele innych kontrolek związane z logowaniem sieci Web można CreateUserWizard bez pisania pojedynczy wiersz kodu. Intuicyjnie udostępnia interfejs użytkownika na podstawie dostawcy członkostwa ustawienia konfiguracji i wewnętrznie wywołania `Membership` klasy `CreateUser` metody po użytkownik wprowadza niezbędne informacje i kliknięciu przycisku tworzenia użytkownika. W formancie CreateUserWizard jest bardzo można dostosowywać. Brak hosta zdarzeń uruchamianych na różnych etapach procesu tworzenia konta. Można utworzyć procedury obsługi zdarzeń, w razie potrzeby można wstawić niestandardowej logiki do przepływu pracy tworzenia konta. Ponadto CreateUserWizard wygląd jest bardzo elastyczny. Istnieje wiele właściwości, które definiują wygląd domyślny interfejs; w razie potrzeby formantu można przekonwertować szablon lub mogą być dodawane dodatkowe kroki rejestracji.

Zacznijmy od przyjrzeć się przy użyciu domyślnego interfejsu i zachowanie formancie CreateUserWizard. Firma Microsoft będzie następnie eksplorować jak dostosować wygląd za pośrednictwem właściwości i zdarzeń formantu.

### <a name="examining-the-createuserwizards-default-interface-and-behavior"></a>Badanie CreateUserWizard domyślnego interfejsu i zachowania

Wróć do `CreatingUserAccounts.aspx` strony `Membership` folderu, Przełącz do trybu projektowania lub podziału, a następnie dodaj formancie CreateUserWizard do górnej części strony. W sekcji Formanty logowania przybornika usterki w formancie CreateUserWizard. Po dodaniu formantu, ustaw jej `ID` właściwości `RegisterUser`. Jak zrzut ekranu w przedstawia rysunek 11, CreateUserWizard renderuje interfejs z pól tekstowych o nazwę użytkownika, hasło nowego użytkownika, adres e-mail i pytanie zabezpieczające i odpowiedzi.


[![Renderuje formancie CreateUserWizard ogólnego Tworzenie interfejsu użytkownika](creating-user-accounts-vb/_static/image32.png)](creating-user-accounts-vb/_static/image31.png)

**Rysunek 11**: formancie CreateUserWizard renderuje ogólne Tworzenie interfejsu użytkownika ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](creating-user-accounts-vb/_static/image33.png))


Załóżmy Poświęć chwilę, aby porównać domyślny interfejs użytkownika generowane w formancie CreateUserWizard z interfejsem utworzonego w kroku 5. Po pierwsze w formancie CreateUserWizard umożliwia obiektu odwiedzającego określić pytanie zabezpieczające i odpowiedzi, naszych interfejsu utworzone ręcznie stosować pytanie zabezpieczające wstępnie zdefiniowane. Interfejs w formancie CreateUserWizard także formanty walidacji konieczne było jeszcze do implementacji sprawdzania poprawności na naszych interfejsu pól formularza. I interfejsie kontroli CreateUserWizard zawiera pole tekstowe Potwierdź hasło (wraz z CompareValidator do upewnij się, że hasło wprowadzony tekst i pola tekstowe porównywanie hasła są takie same).

Interesujące jest to, że w formancie CreateUserWizard sprawdza ustawienia konfiguracji dostawcy członkostwa, podczas renderowania interfejs użytkownika. Na przykład pól tekstowych pytanie i odpowiedź zabezpieczeń są wyświetlane tylko wtedy, jeśli `requiresQuestionAndAnswer` ma wartość True. Podobnie, CreateUserWizard automatycznie dodaje kontrolkę RegularExpressionValidator, aby upewnić się, że spełniono wymagania dotyczące siły hasła i ustawia jej `ErrorMessage` i `ValidationExpression` na podstawie właściwości `minRequiredPasswordLength`, `minRequiredNonalphanumericCharacters`i `passwordStrengthRegularExpression` ustawienia konfiguracji.

Formancie CreateUserWizard, jak jego nazwa wskazuje, jest określana na podstawie [formantu kreatora](https://msdn.microsoft.com/library/s2etd1ek.aspx). Kreator formantów są przeznaczone do zapewniają interfejs do wykonywania zadań wieloetapowych. Kreator formantu może mieć dowolną liczbę `WizardSteps`, z których każdy jest szablon, który definiuje HTML oraz formanty sieci Web dla tego kroku. Kontrola Kreator wyświetla początkowo pierwszy `WizardStep`, wraz z kontrolki, które umożliwiają użytkownikowi przejść od jednego kroku do następnej lub wrócić do poprzednich kroków.

Jak pokazano na deklaratywne znaczników w rysunek 11, formancie CreateUserWizard domyślny interfejs zawiera dwie `WizardStep` s:

- [`CreateUserWizardStep`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizardstep.aspx) ? renderuje interfejsu zbierać informacje dotyczące tworzenia nowego konta użytkownika. Jest to krok pokazano na rysunku nr 11.
- [`CompleteWizardStep`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.completewizardstep.aspx) ? Wyświetla komunikat informujący, że konto zostało pomyślnie utworzone.

Wygląd i zachowanie CreateUserWizard można modyfikować, konwertując jeden z tych kroków do szablonów lub przez dodanie własnych `WizardStep` s. Zajmiemy Dodawanie `WizardStep` interfejsu rejestracji w *przechowywania dodatkowych informacji użytkownika* samouczka.

Zobaczmy formancie CreateUserWizard w akcji. Odwiedź stronę `CreatingUserAccounts.aspx` strony za pośrednictwem przeglądarki. Najpierw wprowadź niektóre nieprawidłowe wartości w interfejsie CreateUserWizard. Spróbuj wprowadzić hasło, które są niezgodne do wymagania dotyczące siły haseł lub pozostawiając puste pole tekstowe nazwy użytkownika. CreateUserWizard wyświetli odpowiedni komunikat o błędzie. Rysunek 12 przedstawiono dane wyjściowe podczas próby utworzenia użytkownika z niewystarczająco silne hasło.


[![CreateUserWizard automatycznie Injects formanty walidacji](creating-user-accounts-vb/_static/image35.png)](creating-user-accounts-vb/_static/image34.png)

**Rysunek 12**: CreateUserWizard automatycznie wprowadza formanty walidacji ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](creating-user-accounts-vb/_static/image36.png))


Następnie wprowadź odpowiednie wartości w CreateUserWizard i kliknij przycisk Utwórz użytkownika. Zakładając, że wymagane pola zostały wprowadzone i siły hasła jest wystarczająca, CreateUserWizard utworzy nowe konto użytkownika za pośrednictwem członkostwa w ramach i wyświetl `CompleteWizardStep`obiektu interfejsu (patrz rysunek 13). W tle wywołuje CreateUserWizard `Membership.CreateUser` — metoda, podobnie jak robiliśmy w kroku 5.


[![Nowe konto użytkownika ma pomyślnie utworzone](creating-user-accounts-vb/_static/image38.png)](creating-user-accounts-vb/_static/image37.png)

**Rysunek 13**: A nowego konta użytkownika została pomyślnie zostały utworzone ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](creating-user-accounts-vb/_static/image39.png))


> [!NOTE]
> Jak pokazano na rysunku 13, `CompleteWizardStep`przez interfejs zawiera przycisku Kontynuuj. Jednak w tym momencie kliknięcie tak powoduje wykonanie odświeżania strony, pozostawiając obiekt odwiedzający na tej samej stronie. W Dostosowywanie wyglądu i zachowania za pośrednictwem jej właściwości CreateUserWizard sekcji firma Microsoft będzie wyglądać w sposób masz ten przycisk wysłać użytkownika `Default.aspx` (lub innej strony).


Po utworzeniu nowego konta użytkownika, wróć do programu Visual Studio i sprawdź, czy `aspnet_Users` i `aspnet_Membership` tabel, takich jak robiliśmy na rysunku nr 10, aby sprawdzić, czy konto została pomyślnie utworzona.

### <a name="customizing-the-createuserwizards-behavior-and-appearance-through-its-properties"></a>Dostosowywanie zachowania i wyglądu w jego właściwościach CreateUserWizard

Na różne sposoby, za pomocą właściwości, można dostosować CreateUserWizard `WizardStep` s i procedury obsługi zdarzeń. W tej sekcji przedstawiono, jak dostosować wygląd formantu za pośrednictwem jej właściwości; Następna sekcja analizuje rozszerzanie zachowanie formantu za pomocą programów obsługi zdarzeń.

Praktycznie cały tekst wyświetlany w formancie CreateUserWizard domyślnego interfejsu użytkownika można dostosować za pomocą jego nadmiarem właściwości. Na przykład można dostosować nazwę użytkownika, hasło, potwierdź hasło, poczty E-mail, pytanie zabezpieczające i odpowiedź zabezpieczeń etykiety wyświetlane z lewej strony pól tekstowych przez [ `UserNameLabelText` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.usernamelabeltext.aspx), [ `PasswordLabelText` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.passwordlabeltext.aspx), [ `ConfirmPasswordLabelText` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.confirmpasswordlabeltext.aspx), [ `EmailLabelText` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.emaillabeltext.aspx), [ `QuestionLabelText` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.questionlabeltext.aspx), i [ `AnswerLabelText` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.answerlabeltext.aspx) właściwości, odpowiednio. Podobnie, nie ma właściwości określająca tekst Tworzenie użytkownika i Kontynuuj przycisków w `CreateUserWizardStep` i `CompleteWizardStep`, jak również tak, jakby tych przycisków są renderowane jako przyciski, LinkButtons lub ImageButtons.

Kolory, czcionki, obramowania i inne elementy wizualne mogą być konfigurowane przez hosta właściwości stylu. Wspólne właściwości stylu formantu sieci Web - ma CreateUserWizard formancie `BackColor`, `BorderStyle`, `CssClass`, `Font`i tak dalej - i kilka właściwości stylu do definiowania wyglądu dla poszczególnych sekcji Interfejs w CreateUserWizard. [ `TextBoxStyle` Właściwości](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.textboxstyle.aspx), na przykład określa style dla pól tekstowych w `CreateUserWizardStep`, gdy [ `TitleTextStyle` właściwości](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.titletextstyle.aspx) definiuje styl tytułu (zamówić Twoje nowe Konto).

Oprócz właściwości związanych z wygląd istnieje wiele właściwości, które wpływają na działanie w formancie CreateUserWizard. [ `DisplayCancelButton` Właściwości](https://msdn.microsoft.com/library/system.web.ui.webcontrols.wizard.displaycancelbutton.aspx), jeśli jest ustawiona na wartość True, wskazuje przycisk Anuluj, obok przycisku tworzenia użytkownika (wartość domyślna to False). Po wyświetleniu przycisk Anuluj, należy również ustawić [ `CancelDestinationPageUrl` właściwości](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.continuedestinationpageurl.aspx), który określa stronę użytkownika są wysyłane do po kliknięciu przycisku Anuluj. Jak wspomniano w poprzedniej sekcji, przycisku Kontynuuj w `CompleteWizardStep`przez interfejs powoduje odświeżenie strony, ale pozostawia obiekt odwiedzający na tej samej stronie. Aby wysłać obiekt odwiedzający do innej strony po kliknięciu przycisku Kontynuuj, wystarczy określić adres URL w [ `ContinueDestinationPageUrl` właściwości](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.continuedestinationpageurl.aspx).

Ta funkcja pozwala zaktualizować `RegisterUser` formancie CreateUserWizard przycisk Anuluj i wysłać użytkownika `Default.aspx` po kliknięciu przycisku Anuluj lub Kontynuuj. Aby to zrobić, ustaw `DisplayCancelButton` właściwości na wartość True, a oba `CancelDestinationPageUrl` i `ContinueDestinationPageUrl` właściwości ~ / Default.aspx. Rysunek 14 zawiera zaktualizowane CreateUserWizard widzianego za pośrednictwem przeglądarki.


[![Element CreateUserWizardStep zawiera przycisk Anuluj](creating-user-accounts-vb/_static/image41.png)](creating-user-accounts-vb/_static/image40.png)

**Rysunek 14**: `CreateUserWizardStep` zawiera przycisk Anuluj ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](creating-user-accounts-vb/_static/image42.png))


Gdy użytkownik wprowadza nazwę użytkownika, hasła, adres e-mail i pytanie zabezpieczające i odpowiedzi i klika pozycję Utwórz użytkownika, tworzone jest nowe konto użytkownika, a użytkownik jest zalogowany jako nowo utworzony użytkownik. Przy założeniu, że osoby, odwiedzając stronę tworzy nowe konto dla siebie, prawdopodobnie jest zamierzone zachowanie. Można jednak administratorzy mogą dodawać nowych kont użytkowników. W ten sposób może zostać utworzone konto użytkownika, ale Administrator pozostanie zalogowany jako Administrator (a nie nowo utworzonych kont). To zachowanie można modyfikować za pomocą typu Boolean [ `LoginCreatedUser` właściwości](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.logincreateduser.aspx).

Konta użytkowników w ramach członkostwa zawierają zatwierdzonych flagi; Nie można zalogować się do witryny są użytkownicy, którzy nie zostały zatwierdzone. Domyślnie nowo utworzonego konta jest oznaczony jako zatwierdzone, umożliwiając użytkownikowi natychmiast zalogować się do witryny. Jest to możliwe, jednak mieć oznaczenie niezatwierdzonych nowych kont użytkowników. Możesz określić ma administratorowi ręcznie zatwierdzić nowych użytkowników, zanim będzie mógł logować lub może być, aby sprawdzić, czy przed pozwalające użytkownikowi na logowanie jest prawidłowy adres e-mail wprowadzony podczas rejestracji. Niezależnie od przypadku, może mieć nowo utworzonego konta użytkownika oznaczona jako niezatwierdzonych przez ustawienie w formancie CreateUserWizard [ `DisableCreatedUser` właściwość](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.disablecreateduser.aspx) na wartość True (wartość domyślna to False).

Inne właściwości związane z zachowaniem notatki obejmują `AutoGeneratePassword` i `MailDefinition`. Jeśli [ `AutoGeneratePassword` właściwości](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.autogeneratepassword.aspx) ma wartość True, `CreateUserWizardStep` nie są wyświetlane pola tekstowe hasło i potwierdzenie hasła; zamiast tego nowo utworzone hasło użytkownika jest generowana automatycznie przy użyciu `Membership` klasy [ `GeneratePassword` metody](https://msdn.microsoft.com/library/system.web.security.membership.generatepassword.aspx). `GeneratePassword` Metoda tworzy hasło o określonej długości i z wystarczającą liczbą znaków innych niż alfanumeryczne, aby spełniać wymagania dotyczące siły haseł skonfigurowane.

[ `MailDefinition` Właściwości](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.maildefinition.aspx) jest przydatne, jeśli chcesz wysłać wiadomość e-mail na adres e-mail określony w trakcie procesu tworzenia konta. `MailDefinition` Właściwość zawiera szereg właściwości podrzędnych do definiowania informacji na temat wiadomości e-mail skonstruowane. Te właściwości obejmują opcje, takie jak `Subject`, `Priority`, `IsBodyHtml`, `From`, `CC`, i `BodyFileName`. [ `BodyFileName` Właściwości](https://msdn.microsoft.com/library/system.web.ui.webcontrols.maildefinition.bodyfilename.aspx) wskazuje tekst lub pliku HTML, który zawiera treść wiadomości e-mail. Treść obsługuje dwa wstępnie zdefiniowane symbole zastępcze: `<%UserName%>` i `<%Password%>`. Te symbole, jeśli jest obecny w `BodyFileName` pliku, zostanie zastąpiony nazwę i hasło użytkownika właśnie utworzony.

> [!NOTE]
> `CreateUserWizard` Formantu `MailDefinition` właściwość określa tylko szczegóły dotyczące wiadomości e-mail, który jest wysyłany, gdy tworzone jest nowe konto. Nie ma żadnych szczegółów w sposób faktycznie zostanie wysłana wiadomość e-mail (oznacza to, czy używany jest katalog serwera lub przechowywania poczty usługi SMTP, wszystkie informacje dotyczące uwierzytelniania i tak dalej). Te informacje niskiego poziomu, które muszą być zdefiniowane w `<system.net>` sekcji `Web.config`. Więcej informacji na temat tych ustawień konfiguracji i wysyłania wiadomości e-mail z programu ASP.NET 2.0, ogólnie rzecz biorąc, można znaleźć w [— często zadawane pytania na SystemNetMail.com](http://www.systemnetmail.com/) i Moje artykułu [wysyłania poczty E-mail w programie ASP.NET 2.0](http://aspnet.4guysfromrolla.com/articles/072606-1.aspx).


### <a name="extending-the-createuserwizards-behavior-using-event-handlers"></a>Rozszerzanie zachowanie CreateUserWizard przy użyciu programów obsługi zdarzeń

W formancie CreateUserWizard podnosi liczbę zdarzeń podczas jego przepływu pracy. Na przykład po użytkownik wprowadza swoją nazwę użytkownika, hasło i inne istotne informacje i kliknięciu przycisku Utwórz użytkownika, zgłasza formancie CreateUserWizard jego [ `CreatingUser` zdarzeń](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.creatinguser.aspx). Jeśli występuje problem podczas procesu tworzenia [ `CreateUserError` zdarzeń](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.createusererror.aspx) jest uruchamiany; jednak, jeśli użytkownik została pomyślnie utworzona, a następnie [ `CreatedUser` zdarzeń](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.createduser.aspx) jest wywoływane. Istnieją dodatkowe zdarzenia kontroli CreateUserWizard, które uzyskać zgłoszone, ale są to te trzy najbardziej istotnego znaczenia.

W pewnych sytuacjach może chcemy naciśnij w przepływie pracy CreateUserWizard, którego możemy tworząc program obsługi zdarzeń dla odpowiedniego zdarzenia. Na przykład możemy ulepszyć `RegisterUser` formancie CreateUserWizard w celu uwzględnienia niektórych niestandardowego sprawdzania poprawności nazwy użytkownika i hasła. W szczególności umożliwia także poprawiania komfortu naszych CreateUserWizard tak, aby nazwy użytkowników nie może zawierać spacji wiodących i końcowych i nazwa użytkownika nie może występować w dowolnym miejscu w haśle. Krótko mówiąc chcemy uniemożliwić niepowołanym tworzenia nazwę użytkownika, takie jak "Scott" lub kombinacja nazwy użytkownika i hasła, takie jak Scott i Scott.1234 o.

Zostanie utworzony moduł obsługi zdarzeń w tym celu `CreatingUser` zdarzenia do naszej sprawdzana jest poprawność dodatkowe. Podane dane są nieprawidłowe musimy anulować proces tworzenia. Musimy również dodawanie formantu etykiety w sieci Web do strony, aby wyświetlić komunikat wyjaśniający, że nazwa użytkownika lub hasło jest nieprawidłowe. Rozpocznij od dodania formantu etykiety pod formancie CreateUserWizard, ustawienie jej `ID` właściwości `InvalidUserNameOrPasswordMessage` i jego `ForeColor` właściwości `Red`. Czyści jej `Text` właściwości i zestaw jej `EnableViewState` i `Visible` właściwości na wartość False.

[!code-aspx[Main](creating-user-accounts-vb/samples/sample7.aspx)]

Następnie należy utworzyć programu obsługi zdarzeń dla formancie CreateUserWizard `CreatingUser` zdarzeń. Aby utworzyć program obsługi zdarzeń, wybierz kontrolkę w projektancie, a następnie przejdź do okna właściwości. Z tego miejsca kliknij ikonę błyskawicy, a następnie kliknij dwukrotnie odpowiednie zdarzenia w celu utworzenia programu obsługi zdarzeń.

Dodaj następujący kod do `CreatingUser` obsługi zdarzeń:

[!code-vb[Main](creating-user-accounts-vb/samples/sample8.vb)]

Należy pamiętać, że nazwa użytkownika i hasło w formancie CreateUserWizard są dostępne za pośrednictwem jego [ `UserName` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.username.aspx) i [ `Password` właściwości](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.password.aspx)odpowiednio. Używamy tych właściwości w powyższym programu obsługi zdarzeń w celu określenia, czy podana nazwa użytkownika zawiera spacji wiodących i końcowych i określa, czy nazwa użytkownika znajduje się w obrębie hasło. Jeśli jest spełniony jeden z tych warunków, komunikat o błędzie jest wyświetlany w `InvalidUserNameOrPasswordMessage` etykiety i program obsługi zdarzeń `e.Cancel` właściwość jest ustawiona na `True`. Jeśli `e.Cancel` ma ustawioną wartość `True`, CreateUserWizard short-circuits jego przepływu pracy, efektywnie anulowanie procesu tworzenia konta użytkownika.

Zrzut ekranu przedstawia rysunek 15 `CreatingUserAccounts.aspx` gdy użytkownik wprowadza nazwę użytkownika z spacje początkowe.


[![Nazwy użytkowników z wiodące lub końcowe spacje są niedozwolone.](creating-user-accounts-vb/_static/image44.png)](creating-user-accounts-vb/_static/image43.png)

**Rysunek 15**: nazwy użytkowników z wiodące lub końcowe spacje nie są dozwolone ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](creating-user-accounts-vb/_static/image45.png))


> [!NOTE]
> Przykład użycia formancie CreateUserWizard zostanie wyświetlone `CreatedUser` zdarzenia w *<a id="_msoanchor_11"> </a> [przechowywania dodatkowych informacji użytkownika](storing-additional-user-information-vb.md)* samouczka.


## <a name="summary"></a>Podsumowanie

`Membership` Klasy `CreateUser` metoda tworzy nowe konto użytkownika w ramach członkostwa. Robi to przez delegowanie wywołanie skonfigurowanego dostawcy członkostwa. W przypadku liczby `SqlMembershipProvider`, `CreateUser` metoda dodaje rekord `aspnet_Users` i `aspnet_Membership` tabel bazy danych programu.

Gdy programowo (jak widzieliśmy w kroku 5) można utworzyć nowych kont użytkowników, szybciej i łatwiej rozwiązaniem jest użycie w formancie CreateUserWizard. Ten formant renderuje interfejs użytkownika wieloetapowych zbierania informacji o użytkowniku i tworzenia nowego użytkownika w ramach członkostwa. Poniżej obejmuje, ta kontrola korzysta takie same `Membership.CreateUser` metoda w kroku 5, ale formant tworzy interfejs użytkownika, formanty walidacji i odpowiada błędów tworzenia konta użytkownika bez konieczności pisania lizawki kodu.

W tym momencie mamy funkcji w celu tworzenia nowych kont użytkowników. Jednak strony logowania nadal jest sprawdzanie poprawności względem tych poświadczeń ustalony określone w drugim samouczka. W <a id="_msoanchor_12"> </a> [następny samouczek](validating-user-credentials-against-the-membership-user-store-vb.md) modyfikacjom `Login.aspx` sprawdza, czy użytkownik na dostarczone poświadczenia w ramach członkostwa.

Programowanie przyjemność!

### <a name="further-reading"></a>Dalsze informacje

Więcej informacji dotyczących tematów omówionych w tym samouczku można znaleźć w następujących zasobach:

- [`CreateUser` Dokumentacja techniczna](https://msdn.microsoft.com/library/system.web.security.membershipprovider.createuser.aspx)
- [Informacje o formancie CreateUserWizard](https://quickstarts.asp.net/QuickStartv20/aspnet/doc/ctrlref/login/createuserwizard.aspx)
- [Tworzenie dostawcy mapy witryny opartej na systemie plików](http://aspnet.4guysfromrolla.com/articles/020106-1.aspx)
- [Tworzenie za pomocą programu ASP.NET 2.0 Kreator formantu interfejsu użytkownika krok po kroku](http://aspnet.4guysfromrolla.com/articles/061406-1.aspx)
- [Badanie ASP.NET 2.0 do nawigacji w witrynie](http://aspnet.4guysfromrolla.com/articles/111605-1.aspx)
- [Strony wzorcowej i nawigacji w witrynie](https://asp.net/learn/data-access/tutorial-03-vb.aspx)
- [Dostawcy mapy witryny SQL oczekiwania dla](https://msdn.microsoft.com/msdnmag/issues/06/02/WickedCode/default.aspx)

### <a name="about-the-author"></a>Informacje o autorze

Scott Bento, Utwórz wiele książek ASP/ASP.NET i twórcę 4GuysFromRolla.com, pracuje z technologii Microsoft Web od 1998. Scott działa jako niezależnego konsultanta trainer i składnika zapisywania. Jest jego najnowszej książki  *[Sams nauczyć się ASP.NET 2.0 w ciągu 24 godzin](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)*. Scott jest osiągalny w [ mitchell@4guysfromrolla.com ](mailto:mitchell@4guysfromrolla.com) lub za pośrednictwem jego blogu w [ http://ScottOnWriting.NET ](http://scottonwriting.net/).

### <a name="special-thanks-to"></a>Specjalne podziękowania dla

Ten samouczek serii zostało sprawdzone przez wiele recenzentów przydatne. Recenzenta realizacji w tym samouczku został Teresa Murphy. Zainteresowani recenzowania Moje nadchodzących artykuły MSDN? Jeśli tak, Porzuć mnie linii w [ mitchell@4GuysFromRolla.com ](mailto:mitchell@4guysfromrolla.com).

> [!div class="step-by-step"]
> [Poprzednie](creating-the-membership-schema-in-sql-server-vb.md)
> [dalej](validating-user-credentials-against-the-membership-user-store-vb.md)
