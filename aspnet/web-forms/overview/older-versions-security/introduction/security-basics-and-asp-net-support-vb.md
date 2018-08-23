---
uid: web-forms/overview/older-versions-security/introduction/security-basics-and-asp-net-support-vb
title: Podstawy zabezpieczeń i Obsługa platformy ASP.NET (VB) | Dokumentacja firmy Microsoft
author: rick-anderson
description: To jest pierwszy samouczek z serii samouczków, przedstawiających technik w celu uwierzytelniania za pomocą formularza sieci web odwiedzających, autoryzowanie dostępu do partic...
ms.author: riande
ms.date: 01/13/2008
ms.assetid: ab68a92b-fc81-40a4-a7dc-406625d2c5d4
msc.legacyurl: /web-forms/overview/older-versions-security/introduction/security-basics-and-asp-net-support-vb
msc.type: authoredcontent
ms.openlocfilehash: c293f4946001d0940c7e12b67f4fbc63cf5ca18c
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/16/2018
ms.locfileid: "41751971"
---
<a name="security-basics-and-aspnet-support-vb"></a>Podstawy zabezpieczeń i Obsługa platformy ASP.NET (VB)
====================
przez [Bento Scott](https://twitter.com/ScottOnWriting)

[Pobierz plik PDF](http://download.microsoft.com/download/2/F/7/2F705A34-F9DE-4112-BBDE-60098089645E/aspnet_tutorial01_Basics_vb.pdf)

> To jest pierwszy samouczek z serii samouczków, przedstawiających techniki gości za pomocą formularza sieci web uwierzytelniania, autoryzacji dostępu do konkretnych stron oraz funkcji i zarządzanie kontami użytkowników w aplikacji ASP.NET.


## <a name="introduction"></a>Wprowadzenie

Co to jest jedno forów, witrynami handlu elektronicznego, e-mail online witryn sieci Web, witryn sieci Web w portalu i witryn sieci społecznościowych, w którym wszystkie mają wspólną? Oferują one wszystkie *kont użytkowników*. Witryn, które oferują kont użytkowników, należy podać liczbę usług. Jako minimum nowi odwiedzający muszą mieć możliwość tworzenia konta usługi i zwracanie odwiedzających musi być w stanie się zalogować. Takie aplikacje sieci web podejmować decyzje na podstawie zalogowanego użytkownika: niektóre strony lub akcji mogą być ograniczone do tylko zarejestrowane użytkowników lub do określonej podgrupy użytkowników. inne strony może wyświetlać informacje o określonych dla zalogowanego użytkownika lub może być pokazując więcej lub mniej informacji, w zależności od tego, jakie użytkownik wyświetla stronę.

To jest pierwszy samouczek z serii samouczków, przedstawiających techniki gości za pomocą formularza sieci web uwierzytelniania, autoryzacji dostępu do konkretnych stron oraz funkcji i zarządzanie kontami użytkowników w aplikacji ASP.NET. W trakcie tego samouczka zostanie omówiony sposób:

- Identyfikowanie i logowania użytkowników w witrynie sieci Web
- Użyj ASP. W .NET framework członkostwa do zarządzania kontami użytkowników
- Tworzenie, aktualizowanie i usuwanie kont użytkowników
- Ograniczanie dostępu do strony sieci web, katalogu lub określonych funkcji na podstawie zalogowanego użytkownika
- Użyj ASP. W .NET framework role do skojarzenia kont użytkowników z rolami
- Zarządzanie rolami użytkowników
- Ograniczanie dostępu do strony sieci web, katalogu lub określonych funkcji na podstawie roli zalogowanego użytkownika
- Dostosowywanie i rozszerzanie ASP. Określa, zabezpieczeń sieci w sieci Web

Te samouczki są dostosowane do być zwięzłe i zapewnić instrukcje krok po kroku z dużą ilością zrzuty ekranu wizualnie przeprowadzi Cię przez proces. Każdy samouczek jest dostępny w C# i Visual Basic wersji i obejmuje pobieranie kompletny kod używany. (Ten pierwszy samouczek koncentruje się na pojęcia dotyczące zabezpieczeń z punktu widzenia wysokiego poziomu i w związku z tym nie zawiera żadnych skojarzonego kodu)

W tym samouczku omówimy zabezpieczeń ważne pojęcia i jakie urządzenia są dostępne na platformie ASP.NET, aby pomóc we wdrażaniu uwierzytelniania formularzy, autoryzacji, konta użytkowników i ról. Zaczynajmy!

> [!NOTE]
> Zabezpieczenia są istotnym elementem każdej aplikacji, która obejmuje fizycznego, technologicznego i zasad decyzji i wymaga wysokiego stopnia wiedzę na temat planowania i domeny. W tej serii samouczków nie jest przeznaczony jako wskazówki dotyczące tworzenia zabezpieczaniu aplikacji internetowych. Zamiast koncentruje się ona szczegółowe informacje dotyczące uwierzytelniania formularzy, autoryzacji, konta użytkowników i ról. Podczas gdy niektóre pojęcia dotyczące zabezpieczeń obrotowymi te problemy zostały omówione w tej serii, inne są pozostawiane nieeksplorowanych.


## <a name="authentication-authorization-user-accounts-and-roles"></a>Uwierzytelnianie, autoryzacja, kont użytkowników i ról

Są cztery terminów, które będą używane zbyt często w tej serii samouczków, więc chcę Poświęć chwilę szybki, aby zdefiniować te warunki w kontekście zabezpieczeń sieci web, uwierzytelnianie, autoryzacja, kont użytkowników i ról. W modelu klient serwer, na przykład Internet istnieje wiele scenariuszy, w których serwer musi zidentyfikować klienta wysyłającego żądanie. *Uwierzytelnianie* to proces ustalenia tożsamości klienta. Klient, który został pomyślnie zidentyfikowało jest nazywany *uwierzytelniony*. Niezidentyfikowany klienta jest nazywany *nieuwierzytelnione* lub *anonimowe*.

Systemy bezpiecznego uwierzytelniania obejmują co najmniej jeden z następujących trzech zestawy reguł: [coś, co wiesz, coś, co masz lub coś są](http://www.cs.cornell.edu/Courses/cs513/2005fa/NNLauthPeople.html). Większość aplikacji sieci web opierają się na coś, co klient zna, takie jak hasła lub numeru PIN. Informacje używane do identyfikowania użytkownika — swojej nazwy użytkownika i hasła, na przykład — są określane jako *poświadczenia*. W tej serii samouczków skupia się na *uwierzytelnianie formularzy*, który jest model uwierzytelniania, w których użytkownicy logują się do witryny, podając ich poświadczenia w postaci strony sieci web. Wszystkie wystąpił ten typ uwierzytelniania przed. Przejdź do dowolnej witryny handlu elektronicznego. Gdy wszystko jest gotowe do wyewidencjonowania zostanie wyświetlony monit do logowania, wprowadzając nazwę użytkownika i hasło do pól tekstowych na stronie sieci web.

Oprócz identyfikowania klientów, serwer może być konieczne ograniczyć, jakie zasoby lub funkcje są dostępne w zależności od klienta wysyłającego żądanie. *Autoryzacja* jest proces określania, czy dany użytkownik ma uprawnienia dostępu do określonego zasobu lub funkcji.

A *konta użytkownika* magazyn na potrzeby utrwalanie informacji na temat określonego użytkownika. Konta użytkowników, minimalny zestaw musi zawierać informacje, które jednoznacznie identyfikuje użytkownika, takie jak nazwa logowania użytkownika i hasło. Wraz z tym istotnymi informacjami, konta użytkowników mogą zawierać elementów, takich jak: adres e-mail użytkownika; Data i godzina utworzenia konta; Data i godzina ostatniego logowania, w; Imię i nazwisko; numer telefonu; i adres wysyłkowy. Korzystając z uwierzytelniania formularzy, informacje o koncie użytkownika są zazwyczaj przechowywane w relacyjnej bazy danych, takich jak Microsoft SQL Server.

Aplikacje sieci Web, które obsługują konta użytkowników mogą opcjonalnie grupie użytkowników do *role*. Rola to po prostu etykiety, która jest stosowana do użytkownika i udostępnia abstrakcję do definiowania reguł autoryzacji i funkcje na poziomie strony. Na przykład witryny sieci Web może obejmować rolę administratora przy użyciu reguł autoryzacji, które uniemożliwiają osób administratorowi na dostęp do określonego zestawu stron sieci web. Ponadto różne strony, które są dostępne dla wszystkich użytkowników (w tym użytkowników niebędących administratorami) mogą wyświetlać dodatkowe dane lub oferują dodatkowe funkcje, gdy odwiedzanych przez użytkowników w roli administratora. Przy użyciu ról, możemy zdefiniować te reguły autoryzacji na podstawie roli roli zamiast użytkownika według.

## <a name="authenticating-users-in-an-aspnet-application"></a>Uwierzytelnianie użytkowników w aplikacji ASP.NET

Kiedy użytkownik wprowadzi adres URL do okna adres lub kliknięć ich przeglądarki przez łącze, przeglądarka sprawia, że [protokołu HTTP (Hypertext Transfer)](http://en.wikipedia.org/wiki/HTTP) żądanie do serwera sieci web dla określonej zawartości, są to platformy ASP.NET stronie obrazu, JavaScript plik lub innego typu zawartości. Serwer sieci web jest nadzorowania ze zwracaniem żądanej zawartości. W ten sposób go określić kilka kwestii, o żądaniu, który wysłał żądanie i tego, czy tożsamość jest autoryzowany do pobierania żądanej zawartości.

Domyślnie przeglądarek wysyłania żądań HTTP, które nie mają dowolny rodzaj informacje identyfikacyjne. Ale jeśli przeglądarka zawierają informacje o uwierzytelnianiu następnie serwer sieci web uruchamia przepływ pracy uwierzytelniania, która podejmuje próbę identyfikacji klienta wysyłającego żądanie. Kroki przepływu pracy uwierzytelniania zależą od typu uwierzytelniania używany przez aplikację sieci web. Program ASP.NET obsługuje trzy typy uwierzytelniania: Windows, usługi Passport i formularze. W tej serii samouczków skupia się na uwierzytelnianie formularzy, ale poświęćmy chwilę na Porównaj i zestaw Windows uwierzytelniania użytkownika magazynów i przepływ pracy.

### <a name="authentication-via-windows-authentication"></a>Uwierzytelnianie za pośrednictwem uwierzytelniania Windows

Przepływ pracy uwierzytelniania Windows używa jednego z następujących metod uwierzytelniania:

- Uwierzytelnianie podstawowe
- Uwierzytelnianie szyfrowane
- Zintegrowane uwierzytelnianie Windows

Wszystkie trzy metody działają w przybliżeniu taki sam sposób: nieautoryzowanych nadejściu żądania od użytkowników anonimowych, serwer sieci web odsyła odpowiedź HTTP, która wskazuje, że autoryzacja jest wymagana, aby kontynuować. Następnie w przeglądarce pojawi się okno modalne okno dialogowe, który monituje użytkownika dla nazwy użytkownika i hasła (patrz rysunek 1). Te informacje są następnie wysyłane do serwera sieci web za pomocą nagłówka HTTP.


![Modalne okno dialogowe monituje użytkownika o jego poświadczenia](security-basics-and-asp-net-support-vb/_static/image1.png)

**Rysunek 1**: modalne okno dialogowe monituje użytkownika o jego poświadczenia


Podane poświadczenia są weryfikowane względem Store użytkownika Windows serwer sieci web. Oznacza to, że każdy uwierzytelniony użytkownik w aplikacji sieci web, musisz mieć konto Windows w Twojej organizacji. To jest typowe w intranetowych. W rzeczywistości przy użyciu zintegrowanego uwierzytelniania Windows w środowisku sieci intranet, przeglądarka automatycznie zapewnia serwer sieci web przy użyciu poświadczeń, używane do logowania do sieci, w tym samym pominięcie okna dialogowego przedstawionej na rysunku 1. Podczas uwierzytelniania Windows to doskonałe rozwiązanie dla aplikacji intranetowych, jest zazwyczaj niecelowe dla aplikacji internetowych, ponieważ nie chcesz tworzyć konta Windows dla każdego użytkownika, która zarejestruje się w lokacji.

### <a name="authentication-via-forms-authentication"></a>Uwierzytelnianie za pomocą uwierzytelniania formularzy

Z drugiej strony, uwierzytelnianie formularzy jest idealny dla aplikacji sieci web w Internecie. Odwołania, który wchodzi w skład uwierzytelniania identyfikuje użytkownika przez monitowania o wprowadzenie poświadczeń za pomocą formularza sieci web. W związku z tym gdy użytkownik próbuje uzyskać dostęp do zasobu nieautoryzowany, zostanie automatycznie przekierowany do strony logowania, w którym można wprowadzić swoje poświadczenia. Poświadczenia przesłane następnie są weryfikowane względem magazynu użytkowników niestandardowe — zwykle bazy danych.

Po zweryfikowaniu poświadczeń przesłanych *biletu uwierzytelniania formularzy* jest tworzony dla użytkownika. Ten bilet wskazuje, że użytkownik został uwierzytelniony i zawiera informacje identyfikacyjne, takie jak nazwa użytkownika. Bilet uwierzytelniania formularzy (zazwyczaj) jest przechowywany jako plik cookie na komputerze klienckim. W związku z tym kolejne wizyty w witrynie sieci Web obejmują biletu uwierzytelniania formularzy w żądaniu HTTP, umożliwiając w ten sposób aplikacji sieci web zidentyfikować użytkownika, gdy wcześniejszego zalogowania.

Na rysunku 2 przedstawiono przepływ pracy uwierzytelniania formularzy z punktu firmę vantage wysokiego poziomu. Zwróć uwagę, jak elementy uwierzytelnianie i autoryzacja w programie ASP.NET: działa jako dwa osobne jednostki. System uwierzytelniania formularzy identyfikację użytkownika lub zgłasza, że są anonimowe. System autoryzacji jest to, co określa, czy użytkownik ma dostęp do żądanego zasobu. Jeśli użytkownik nie ma autoryzacji (ponieważ znajdują się na rysunku 2, podczas próby anonimowo odwiedź ProtectedPage.aspx), system autoryzacji zgłasza, że użytkownik ma zabroniony, powodując formularzy systemu uwierzytelniania automatyczne przekierowanie użytkownika do strony logowania.

Po pomyślnym zalogowaniu się użytkownika kolejnych żądań HTTP obejmują biletu uwierzytelniania formularzy. System uwierzytelniania formularzy jedynie identyfikuje użytkownika — jest system autoryzacji, który określa, czy użytkownik może uzyskać dostęp do żądanego zasobu.


![Przepływ pracy uwierzytelniania formularzy](security-basics-and-asp-net-support-vb/_static/image2.png)

**Rysunek 2**: przepływ pracy uwierzytelniania formularzy


Omawiania do uwierzytelniania formularzy znacznie bardziej szczegółowo w dwóch następnych samouczków[omówienie uwierzytelniania formularzy](an-overview-of-forms-authentication-vb.md) i [Konfiguracja uwierzytelniania formularzy i Tematy zaawansowane](forms-authentication-configuration-and-advanced-topics-vb.md). Aby uzyskać więcej informacji na stronach ASP. Opcje uwierzytelniania w sieci, zobacz [Uwierzytelnianie ASP.NET](https://msdn.microsoft.com/library/eeyk640h.aspx).

## <a name="limiting-access-to-web-pages-directories-and-page-functionality"></a>Ograniczanie dostępu do stron sieci Web, katalogów i funkcji strony

Program ASP.NET zawiera dwa sposoby, aby ustalić, czy dany użytkownik ma uprawnienia dostępu do określonego pliku lub katalogu:

- **Plik autoryzacji** — od strony ASP.NET i usług sieci web są implementowane jako pliki, które znajdują się w systemie plików serwera sieci web, dostęp do tych plików można określić za pomocą list kontroli dostępu (ACL). Plik autoryzacji jest najczęściej używana z uwierzytelnianiem Windows, ponieważ listy ACL są uprawnienia, które są stosowane do kont Windows. Korzystając z uwierzytelniania formularzy, wszystkie żądania systemu na poziomie systemu operacyjnego i pliku są wykonywane przez tego samego konta Windows, niezależnie od użytkownika w witrynie.
- **Autoryzacja adresów URL**— z Autoryzacja adresów URL dla deweloperów strony określa reguły autoryzacji w pliku Web.config. Te reguły autoryzacji określają, co użytkowników lub ról mają zezwolenie na dostęp lub odmówiono dostępu do niektórych stron lub katalogów w aplikacji.

Autoryzacja adresów URL i autoryzacji plików można zdefiniować reguły autoryzacji do uzyskiwania dostępu do określonej strony ASP.NET lub dla wszystkich stron ASP.NET w określonym katalogu. Korzystanie z tych technik firma Microsoft może wydać ASP.NET odmowę realizacji żądań na danej stronie dany użytkownik lub zezwolić na dostęp do zbioru użytkowników i nie zezwoli na dostęp wszystkim osobom. Jak wygląda sytuacjach, gdy wszyscy użytkownicy mogą uzyskać dostęp do strony, ale funkcjonalność strony zależy od użytkownika? Na przykład wiele witryn, które obsługują konta użytkowników mają stron, które wyświetlają różną zawartość lub dane uwierzytelnionych użytkowników, a użytkownicy anonimowi. Użytkownik anonimowy napotkać łącze, aby zalogować się do witryny, podczas gdy uwierzytelniony użytkownik będzie zamiast tego zostanie wyświetlony komunikat, na przykład, Witamy z powrotem, *Username* wraz z linkiem do wylogowania. Inny przykład: podczas wyświetlania elementu na witryną zobaczyć różne informacje w zależności od tego, czy jesteś oferta najwyższa lub jeden system aukcji elementu.

Deklaratywne lub programowo, można wykonywać takie dostosowania na poziomie strony. Aby wyświetlić różną zawartość w przypadku anonimowych niż uwierzytelnionych użytkowników, po prostu przeciągnij [kontrolki widoku logowania](https://msdn.microsoft.com/library/system.web.ui.webcontrols.loginview.aspx) na stronę i wprowadź odpowiednią zawartość do jego szablony AnonymousTemplate i LoggedInTemplate. Alternatywnie, można programowo określić czy bieżące żądanie jest uwierzytelniane, kim jest użytkownik i jakie role należą one do (jeśli istnieje). Następnie pokazać lub ukryć kolumny w siatce lub paneli, na stronie, można użyć tych informacji.

Ta seria zawiera trzy samouczków, które koncentrują się na autoryzacji. ***Autoryzacja oparta na użytkowniku***zbadamy, jak ograniczyć dostęp do stron w katalogu lub strony na określone konta użytkowników; ***Autoryzacji na podstawie roli*** patrzy na dostarczenie reguły autoryzacji w roli, poziom; Ponadto ***wyświetlanie zawartości na podstawie obecnie rejestrowane w użytkownika*** w tym samouczku przedstawiono, modyfikując określonego na stronie zawartość i funkcje na podstawie użytkownika, odwiedzając stronę. Aby uzyskać więcej informacji na stronach ASP. Opcje autoryzacji w sieci, zobacz [autoryzacji programu ASP.NET](https://msdn.microsoft.com/library/wce3kxhd.aspx).

## <a name="user-accounts-and-roles"></a>Konta użytkowników i ról

ASP. W sieci, uwierzytelnianie formularzy zapewnia infrastrukturę dla użytkowników, zaloguj się do witryny i ich stan uwierzytelnionego zapamiętanych między odwiedzin strony. I Autoryzacja adresów URL oferuje umożliwiająca ograniczenie dostępu do określonych plików lub folderów w aplikacji ASP.NET. Żadna funkcja dostarcza jednak oznacza, że do przechowywania informacji o koncie użytkownika lub Zarządzanie rolami.

Przed ASP.NET 2.0 deweloperzy były odpowiedzialne za tworzenie własnych magazynów użytkownika i roli. Również znajdowały się w haku projektowanie interfejsów użytkownika i pisanie kodu dla użytkownika niezbędne stron związanych z kontem, takich jak strony logowania i strony Aby utworzyć nowe konto, między innymi. Bez dowolnej architektury konta wbudowanego użytkownika w programie ASP.NET: Każdy deweloper implementującej kont użytkowników było osiągnąć własnej decyzji projektowych na takie pytania, jak, jak przechowywać hasła lub inne poufne informacje? i jakie wytyczne powinny nakładać dotyczące długość hasła i siły?

Obecnie Implementowanie kont użytkowników w aplikacji ASP.NET jest znacznie prostsze dzięki *framework członkostwa* i wbudowanej kontrolki sieci Web logowania. W ramach członkostwa jest kilka klas w [przestrzeń nazw System.Web.Security](https://msdn.microsoft.com/library/system.web.security.aspx) dostarczające funkcje do wykonywania zadań związanych z kontem użytkownika niezbędne. Klasa klucza w ramach członkostwa jest [klasa członkowska](https://msdn.microsoft.com/library/system.web.security.membership.aspx), który ma metod, takich jak:

- Tworzenie użytkownika
- DeleteUser
- GetAllUsers
- GetUser
- UpdateUser
- ValidateUser

Zastosowano w ramach członkostwa [modelu dostawca](http://aspnet.4guysfromrolla.com/articles/101905-1.aspx), której nie pozostawia żadnych śladów oddziela interfejsu API w ramach członkostwa od implementacji. Umożliwia deweloperom korzystanie wspólny interfejs API, ale upoważnia do użycia z implementacją, który spełnia wymagania niestandardowe swoich aplikacji. Krótko mówiąc klasa członkowska określa podstawowe funkcje framework (metody, właściwości i zdarzenia), ale faktycznie nie dostarcza żadnych szczegółów implementacji. Zamiast tego metody klasy członkostwa wywołać skonfigurowany dostawca to, co wykonuje rzeczywistą pracę. Na przykład gdy wywoływana jest metoda Tworzenie użytkownika klasy członkostwa, klasa członkowska nie wie, szczegółowe informacje o magazynie użytkownika. Nie wie, jeśli użytkownicy są obsługiwane w bazie danych lub w pliku XML lub niektórych innych magazynu. Klasa członkostwa sprawdza konfigurację aplikacji sieci web, aby określić, jakie dostawcę, aby delegować wywołanie, a tej klasy dostawcy jest odpowiedzialny za faktycznie tworzenia nowego konta użytkownika w magazynie odpowiedniego użytkownika. Ta interakcja jest przedstawione na rysunku 3.

Microsoft jest dostarczany dwie klasy dostawcy członkostwa w programie .NET Framework:

- [ActiveDirectoryMembershipProvider](https://msdn.microsoft.com/library/system.web.security.activedirectorymembershipprovider.aspx) — implementuje interfejs API członkostwa w serwerów usługi Active Directory i trybu aplikacji usługi Active Directory (ADAM).
- [SqlMembershipProvider](https://msdn.microsoft.com/library/system.web.security.sqlmembershipprovider.aspx) — implementuje interfejs API członkostwa w bazie danych programu SQL Server.

W tej serii samouczków skupia się wyłącznie na SqlMembershipProvider.


[![Dostawca modelu umożliwia różne implementacje się bezproblemowo podłączone do Framework](security-basics-and-asp-net-support-vb/_static/image4.png)](security-basics-and-asp-net-support-vb/_static/image3.png)

**Rysunek 03**: Dostawca modelu umożliwia różne implementacje się bezproblemowo podłączone w ramach ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](security-basics-and-asp-net-support-vb/_static/image5.png))


Zaletą modelu dostawca jest, że alternatywnych implementacji może być opracowane przez firmę Microsoft, dostawców innych firm lub indywidualnych deweloperów i bezproblemowo podłączony do członkostwa w ramach. Na przykład firma Microsoft wydała [dostawcy członkostwa dla baz danych Microsoft Access](https://download.microsoft.com/download/5/5/b/55bc291f-4316-4fd7-9269-dbf9edbaada8/sampleaccessproviders.vsi). Aby uzyskać więcej informacji o dostawcach członkostwa, zobacz [Toolkit dostawcy](https://msdn.microsoft.com/asp.net/aa336558.aspx), który zawiera wskazówki dotyczące dostawców członkostwa, przykładowe dostawców niestandardowych, ponad 100 strony dokumentacji na podstawie modelu dostawcy i Wbudowani dostawcy członkostwa (to znaczy, ActiveDirectoryMembershipProvider i SqlMembershipProvider) należy wykonać kod źródłowy.

ASP.NET 2.0 wprowadzono także w ramach ról. Jak w ramach członkostwa w ramach ról jest kompilowanych na podstawie modelu dostawców. Jego interfejs API jest udostępniany za pośrednictwem [klasy ról](https://msdn.microsoft.com/library/system.web.security.roles.aspx) i .NET Framework, który jest dostarczany z trzech klas dostawcy:

- [Element AuthorizationStoreRoleProvider](https://msdn.microsoft.com/library/system.web.security.authorizationstoreroleprovider.aspx) — zarządza informacje o rolach w magazynie zasad Menedżera autoryzacji, takich jak usługi Active Directory lub ADAM.
- [SqlRoleProvider](https://msdn.microsoft.com/library/system.web.security.sqlroleprovider.aspx) — implementuje role w bazie danych programu SQL Server.
- [WindowsTokenRoleProvider](https://msdn.microsoft.com/library/system.web.security.windowstokenroleprovider.aspx) -kojarzy informacje o rolach oparte na grupie Windows gości. Ta metoda jest zwykle używany z uwierzytelnianiem Windows.

W tej serii samouczków skupia się wyłącznie na SqlRoleProvider.

Ponieważ model dostawcy zawiera pojedynczy interfejs API przedni (grupy członkostwa i ról), jest możliwe utworzenie funkcji wokół tego interfejsu API bez konieczności martwienia się o szczegóły implementacji — te są obsługiwane przez dostawców wybranych przez strony dla deweloperów. Ten ujednolicony interfejs API umożliwia firmy Microsoft i innych dostawców, do tworzenia kontrolki sieci Web, ten interfejs, za pomocą platform członkostwa i ról. ASP.NET jest dostarczana z liczbą [kontrolki sieci Web logowania](https://msdn.microsoft.com/library/ms178329.aspx) implementacji typowych interfejsów użytkownika konta użytkownika. Na przykład [kontrolka Login](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.aspx) monituje użytkownika o podanie poświadczeń, sprawdza poprawność ich i rejestruje je za pomocą uwierzytelniania formularzy. [Kontrolki widoku logowania](https://msdn.microsoft.com/library/system.web.ui.webcontrols.loginview.aspx) oferuje szablony na potrzeby wyświetlania różny kod znaczników dla użytkowników anonimowych, a użytkownicy uwierzytelnieni lub różny kod znaczników na podstawie roli użytkownika. I [kontroli CreateUserWizard](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.aspx) udostępnia interfejs użytkownika krok po kroku dotyczące tworzenia nowego konta użytkownika.

Wewnętrznie różnych kontrolek logowania wchodzić w interakcje ze strukturami członkostwa i ról. Większość kontrolek logowania może być implementowany bez konieczności pisania nawet wiersza kodu. Będziemy sprawdzać te kontrolki większej liczby szczegółów przyszłych samouczków, w tym techniki rozszerzanie i dostosowywanie ich funkcje.

## <a name="summary"></a>Podsumowanie

Wszystkie aplikacje sieci web, które obsługują konta użytkowników wymagają podobne funkcje, takie jak: możliwości dla użytkowników, zaloguj się i ich dziennika stanu zapamiętanych między odwiedzin strony; strony sieci web nowi odwiedzający utworzyć konto usługi; i możliwość określenia, jakie zasoby, danych i funkcje są dostępne do jakich użytkowników lub ról do deweloperów strony. Zadania, uwierzytelniania i autoryzowania użytkowników i zarządzanie kontami użytkowników i ról jest niezwykle łatwe w aplikacjach ASP.NET dzięki uwierzytelniania formularzy, Autoryzacja adresów URL i struktury członkostwa i ról.

W ciągu następnej kilka samouczków będziemy sprawdzać te aspekty przez utworzenie działającą aplikację sieci web od podstaw w sposób krok po kroku. W dwóch następnych samouczku omówimy uwierzytelniania formularzy szczegółowo. Zobaczymy, przepływ pracy uwierzytelniania formularzy w akcji, przeanalizujemy biletu uwierzytelniania formularzy, omówić problemy dotyczące zabezpieczeń i zobacz, jak skonfigurować system uwierzytelniania formularzy — wszystkie podczas tworzenia aplikacji sieci web, która umożliwia Zaloguj się i wylogowania.

Wszystkiego najlepszego programowania!

### <a name="further-reading"></a>Dalsze informacje

Więcej informacji na tematów omówionych w tym samouczku można znaleźć w następujących zasobach:

- [Program ASP.NET 2.0 członkostwo, role, uwierzytelnianie formularzy i zasoby dotyczące zabezpieczeń](https://weblogs.asp.net/scottgu/ASP.NET-2.0-Membership_2C00_-Roles_2C00_-Forms-Authentication_2C00_-and-Security-Resources-)
- [Wytyczne dotyczące zabezpieczeń 2.0 platformy ASP.NET](https://msdn.microsoft.com/library/ms998258.aspx)
- [Uwierzytelnianie ASP.NET](https://msdn.microsoft.com/library/eeyk640h.aspx)
- [Autoryzacja w programie ASP.NET](https://msdn.microsoft.com/library/wce3kxhd.aspx)
- [Logowanie do platformy ASP.NET określa — omówienie](https://msdn.microsoft.com/library/ms178329.aspx)
- [Badanie programu ASP.NET 2.0 na członkostwo, role i profilu](http://aspnet.4guysfromrolla.com/articles/120705-1.aspx)
- [Jak I: Zabezpieczanie witryny przy użyciu członkostwa i ról](https://asp.net/learn/videos/video-45.aspx) (Wideo)
- [Wprowadzenie do członkostwa](https://msdn.microsoft.com/library/yh26yfzy.aspx)
- [Centrum deweloperów zabezpieczeń w witrynie MSDN](https://msdn.microsoft.com/security/default.aspx)
- [Profesjonalne platformy ASP.NET 2.0 zabezpieczeń, członkostwo i zarządzanie rolami](http://www.wrox.com/WileyCDA/WroxTitle/productCd-0764596985.html) (ISBN: 978-0-7645-9698-8)
- [Zestaw narzędzi do dostawcy](https://msdn.microsoft.com/asp.net/aa336558.aspx)

## <a name="about-the-author"></a>Informacje o autorze

[Scott Bento](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor siedem ASP/ASP.NET książek i założycielem [4GuysFromRolla.com](http://www.4guysfromrolla.com), pracował nad przy użyciu technologii Microsoft Web od 1998 r. Scott działa jako niezależny Konsultant, trainer i składnika zapisywania. Jego najnowszą książkę Stephena [ *Sams uczyć się ASP.NET 2.0 w ciągu 24 godzin*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). ADAM można z Tobą skontaktować w [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) lub za pośrednictwem jego blogu, który znajduje się w temacie [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Specjalne podziękowania dla

W tej serii samouczków został zrecenzowany przez wielu recenzentów pomocne. Weryfikacja potencjalnych klientów w ramach tego samouczka został w tym samouczku, który serii został zrecenzowany przez wielu recenzentów pomocne. Wiodące osób dokonujących przeglądu, w tym samouczku obejmują Alicja Maziarz, Jan Suru i Teresa Murphy. Zainteresowani zapoznaniem Moje kolejnych artykułów MSDN? Jeśli tak, Porzuć mnie linii w [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Poprzednie](forms-authentication-configuration-and-advanced-topics-cs.md)
> [dalej](an-overview-of-forms-authentication-vb.md)
