---
uid: web-forms/overview/older-versions-security/introduction/security-basics-and-asp-net-support-vb
title: Podstawowe informacje o zabezpieczeniach i pomoc techniczna platformy ASP.NET (VB) | Dokumentacja firmy Microsoft
author: rick-anderson
description: To jest pierwszy samouczek z serii samouczków przedstawiających technik w celu uwierzytelniania osoby odwiedzające za pomocą formularza sieci web autoryzowania dostępu do partic...
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/13/2008
ms.topic: article
ms.assetid: ab68a92b-fc81-40a4-a7dc-406625d2c5d4
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-security/introduction/security-basics-and-asp-net-support-vb
msc.type: authoredcontent
ms.openlocfilehash: e62bb865e211a279b60f3120162ffc3c49cbdcc5
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/06/2018
---
<a name="security-basics-and-aspnet-support-vb"></a>Podstawowe informacje o zabezpieczeniach i pomoc techniczna platformy ASP.NET (VB)
====================
przez [Bento Scott](https://twitter.com/ScottOnWriting)

[Pobierz plik PDF](http://download.microsoft.com/download/2/F/7/2F705A34-F9DE-4112-BBDE-60098089645E/aspnet_tutorial01_Basics_vb.pdf)

> To jest pierwszy samouczek z serii samouczków przedstawiających techniki odwiedzających za pomocą formularza sieci web uwierzytelniania, autoryzacji dostępu do konkretnej strony i funkcjonalność i zarządzanie kontami użytkowników w aplikacji ASP.NET.


## <a name="introduction"></a>Wprowadzenie

Co to jest rzecz forów, witryn handlu elektronicznego, wiadomości e-mail witryn sieci Web, witryn sieci Web portalu i witrynami sieci społecznościowych, które wszystkie mają wspólną? Oferują one wszystkich *kont użytkowników*. Lokacje, które oferują kont użytkowników, musisz podać numer usług. Co najmniej nowej osoby odwiedzające muszą mieć możliwość tworzenia konta i zwracająca odwiedzający musi być w stanie się zalogować. Takich aplikacji sieci web mogą podejmować decyzje na podstawie zalogowanego użytkownika: niektóre strony lub akcji mogą być ograniczone do tylko zarejestrowane w użytkowników lub do określonej podgrupy użytkowników. inne strony mogą być wyświetlane informacje o określonych zalogowanego użytkownika lub mogą być wyświetlane mniej więcej informacji, w zależności od tego, jakie użytkownik wyświetla stronę.

To jest pierwszy samouczek z serii samouczków przedstawiających techniki odwiedzających za pomocą formularza sieci web uwierzytelniania, autoryzacji dostępu do konkretnej strony i funkcjonalność i zarządzanie kontami użytkowników w aplikacji ASP.NET. W trakcie tego samouczka, zostaną omówione jak:

- Identyfikowanie i logowania użytkowników w witrynie sieci Web
- Użyj ASP. W NET framework członkostwa do zarządzania kontami użytkowników
- Tworzenie, aktualizowanie i usuwanie kont użytkowników
- Ograniczanie dostępu do strony sieci web, katalogu lub określonych funkcji, na podstawie zalogowanego użytkownika
- Użyj ASP. W NET framework role do skojarzenia kont użytkowników z rolami
- Zarządzanie rolami użytkowników
- Ograniczanie dostępu do strony sieci web, katalogu lub określonych funkcji, na podstawie roli zalogowanego użytkownika
- Dostosowywanie i rozszerzanie ASP. Określa, zabezpieczeń sieci w sieci Web

Te samouczki są dostosowane do zwięzły i zawierają instrukcje krok po kroku z dużą ilością zrzuty ekranu wizualnie prowadzą użytkownika przez proces. Każdego samouczka jest dostępna w C# i Visual Basic wersji i obejmuje pobieranie pełny kod używany. (Ten pierwszy samouczek koncentruje się na koncepcji zabezpieczeń z punktu widzenia wysokiego poziomu i w związku z tym nie zawiera żadnych skojarzonego kodu).

W tym samouczku omówimy zabezpieczeń ważne pojęcia i jakie urządzenia są dostępne w programie ASP.NET do pomocy w przypadku implementowania uwierzytelniania formularzy, autoryzacji, kont użytkowników i ról. Dzieła!

> [!NOTE]
> Zabezpieczenia są istotnym elementem z dowolnej aplikacji, która obejmuje fizyczne, technologicznych, a zasady decyzji i wymaga wysoki stopień wiedzy planowania i domeny. Tego samouczka serii nie jest przeznaczony do tworzenia aplikacji sieci web bezpiecznego jako przewodnika. Zamiast zespoły w szczególności na uwierzytelnianie formularzy, autoryzacji, kont użytkowników i ról. Gdy niektóre pojęcia dotyczące zabezpieczeń, obracanie wokół tych problemów są szczegółowo opisane w tej serii, inne są pozostawiane niezbadanego.


## <a name="authentication-authorization-user-accounts-and-roles"></a>Uwierzytelniania, autoryzacji, kont użytkowników i ról

Role uwierzytelniania, autoryzacji, kont użytkowników i są cztery warunki, które będą używane rzadko w całym tego samouczka serii tak, chcę szybkie chwilę do definiowania tych warunków w kontekście zabezpieczeń sieci web. W modelu klient serwer, na przykład Internetem istnieje wiele scenariuszy, w których serwer musi zidentyfikować klienta zgłoszenia żądania. *Uwierzytelnianie* to proces ustalenia tożsamości klienta. Klient, który został zidentyfikowany pomyślnie jest określany jako *uwierzytelniony*. Niezidentyfikowane klienta jest określany jako *nieuwierzytelnione* lub *anonimowych*.

Systemy bezpiecznego uwierzytelniania obejmować co najmniej jeden z następujących trzech aspektami: [coś wiesz, coś masz lub coś są](http://www.cs.cornell.edu/Courses/cs513/2005fa/NNLauthPeople.html). Większość aplikacji sieci web polegają na coś, co klient zna, takie jak hasła lub numeru PIN. Informacje używane do identyfikacji użytkownika - swojej nazwy użytkownika i hasła, na przykład - są określane jako *poświadczenia*. Ten samouczek seria skupia się wokół *uwierzytelnianie formularzy*, która jest model uwierzytelniania, gdy użytkownicy logują się do witryny przez podanie poświadczeń w formie strony sieci web. Firma Microsoft wszystkie wystąpił ten typ uwierzytelniania przed. Przejdź do dowolnej lokacji handlu elektronicznego. Gdy wszystko będzie gotowe do wyewidencjonowania zostanie wyświetlona prośba o zalogowanie się przez podanie nazwy użytkownika i hasła do pól tekstowych na stronie sieci web.

Oprócz Identyfikacja klientów, serwer może być konieczne ograniczenie, jakie zasoby lub funkcje są dostępne w zależności od klienta zgłoszenia żądania. *Autoryzacji* to proces określania, czy dany użytkownik ma uprawnienia do dostępu do określonych zasobów lub funkcji.

A *konta użytkownika* jest magazynem trwałych informacji na temat konkretnego użytkownika. Konta użytkowników muszą zawierać minimalny zestaw informacje, które jednoznacznie identyfikuje użytkownika, takie jak nazwa logowania użytkownika i hasło. Wraz z tym ważne informacje konta użytkowników mogą zawierać elementów, jak: adres e-mail użytkownika; Data i godzina utworzenia konta; Data i godzina ostatniego logowania, w; Imię i nazwisko; numer telefonu; i adres wysyłkowy. Podczas korzystania z uwierzytelniania formularzy, informacje o koncie użytkownika jest zazwyczaj przechowywane w relacyjnej bazie danych, takich jak Microsoft SQL Server.

Aplikacje sieci Web, które obsługują kont użytkowników może opcjonalnie grupy użytkowników do *ról*. Rola to po prostu etykietę, która jest stosowana do użytkownika i udostępnia abstrakcję do definiowania reguł autoryzacji i funkcje na poziomie strony. Na przykład witryna sieci Web może obejmować rolę administratora przy użyciu reguł autoryzacji, które uniemożliwiają odbiorców administratorem, aby uzyskać dostępu do określonego zestawu stron sieci web. Ponadto różne strony, które są dostępne dla wszystkich użytkowników (w tym użytkowników niebędących administratorami) może wyświetlić dodatkowe dane lub oferują dodatkowe funkcje, gdy odwiedzanych przez użytkowników w roli administratora. Przy użyciu ról, możemy zdefiniować te reguły autoryzacji na podstawie roli roli zamiast przez użytkownika.

## <a name="authenticating-users-in-an-aspnet-application"></a>Uwierzytelnianie użytkowników w aplikacji ASP.NET

Gdy użytkownik wpisze adres URL do okna adresu w przeglądarce lub kliknięć łącze, przeglądarki zapewnia [protokołu HTTP (Hypertext Transfer)](http://en.wikipedia.org/wiki/HTTP) żądanie do serwera sieci web dla określonej zawartości, można go ASP.NET strony obrazu, JavaScript plik, lub typ zawartości. Serwer sieci web jest zlecił ze zwracaniem żądanej zawartości. W ten sposób go określania liczby kwestii dotyczących żądania, w tym, który zgłosił żądanie i określa, czy tożsamość jest autoryzowany do pobierania żądanej zawartości.

Domyślnie przeglądarki wysyłanie żądania HTTP, których brakuje dowolny rodzaj informacji identyfikacyjnych. A jeśli przeglądarka zawierają informacje dotyczące uwierzytelniania następnie serwera sieci web uruchamia się przepływ pracy uwierzytelniania, która podejmuje próbę identyfikacji klienta zgłoszenia żądania. Czynności przepływu pracy uwierzytelniania są zależne od typu uwierzytelniania używany przez aplikację sieci web. Program ASP.NET obsługuje trzy typy uwierzytelniania: systemu Windows, usługa Passport i formularze. Ta seria samouczek koncentruje się na uwierzytelnianie formularzy, ale umożliwia zająć kilka minut, aby porównać i kontrastu magazyny użytkowników uwierzytelniania systemu Windows i przepływ pracy.

### <a name="authentication-via-windows-authentication"></a>Uwierzytelnianie za pomocą uwierzytelniania systemu Windows

Przepływ uwierzytelniania systemu Windows korzysta z jednego z następujących metod uwierzytelniania:

- Uwierzytelnianie podstawowe
- Uwierzytelnianie szyfrowane
- Zintegrowane uwierzytelnianie systemu Windows

Wszystkie trzy metody pracy mniej więcej tak samo jak: podczas nieautoryzowanego dociera do żądania od użytkowników anonimowych, serwer sieci web odsyła odpowiedź HTTP, która wskazuje, że autoryzacja jest wymagana, aby kontynuować. Następnie w przeglądarce pojawi się okno modalne okno dialogowe, które monituje użytkownika o swoją nazwę i hasło (zobacz rysunek 1). Te informacje jest następnie wysyłane do serwera sieci web za pośrednictwem nagłówka HTTP.


![Modalne okno dialogowe użytkownik monituje o podanie swoich poświadczeń](security-basics-and-asp-net-support-vb/_static/image1.png)

**Rysunek 1**: modalne okno dialogowe użytkownik monituje o podanie swoich poświadczeń


Podane poświadczenia są sprawdzane w magazynie użytkownika systemu Windows serwer sieci web. Oznacza to, że każdy użytkownik uwierzytelniony w aplikacji sieci web musi mieć konto systemu Windows w Twojej organizacji. To jest typowe w intranecie. W rzeczywistości ustawienia sieci intranet przy użyciu zintegrowanego uwierzytelniania systemu Windows, przeglądarka automatycznie zapewnia serwer sieci web z poświadczenia używane do logowania się do sieci, a tym samym pomijanie okno dialogowe pokazany na rysunku 1. Podczas uwierzytelniania systemu Windows jest doskonały dla aplikacji sieci intranet, jest zazwyczaj będzie niemożliwe dla aplikacji internetowych, ponieważ nie chcesz utworzyć konta systemu Windows dla każdego użytkownika, która zarejestruje się w danej lokacji.

### <a name="authentication-via-forms-authentication"></a>Uwierzytelnianie za pomocą uwierzytelniania formularzy

Z drugiej strony, uwierzytelnianie formularzy, jest odpowiedni dla aplikacji sieci web internetowych. Odwołania, który wchodzi w skład uwierzytelniania identyfikuje użytkownika za pomocą wyświetlania monitu o wprowadzenie poświadczeń za pomocą formularza sieci web. W związku z tym gdy użytkownik próbuje uzyskać dostęp do zasobu nieautoryzowanego, zostanie automatycznie przekierowany do strony logowania, w którym mogą oni wprowadzić swoje poświadczenia. Przesłane poświadczenia są następnie sprawdzane magazynu niestandardowego użytkowników — zwykle bazy danych.

Po zweryfikowaniu poświadczeń przesłanych *biletu uwierzytelniania formularzy* jest tworzony dla użytkownika. Ten bilet wskazuje, czy użytkownik został uwierzytelniony i zawiera informacje identyfikacyjne, takie jak nazwa użytkownika. Biletu uwierzytelniania formularzy (zwykle) jest przechowywana jako plik cookie na komputerze klienckim. W związku z tym kolejnych wizytach witryny sieci Web obejmują biletu uwierzytelniania formularzy w żądaniu HTTP, umożliwiając w ten sposób identyfikowania użytkownika, gdy są zalogowani aplikacji sieci web.

Rysunek 2 przedstawia przepływ pracy uwierzytelniania formularzy z punktu firmę vantage wysokiego poziomu. Zwróć uwagę, jak elementy uwierzytelnianie i autoryzacja w programie ASP.NET działa jako dwa osobne jednostki. System uwierzytelniania formularzy identyfikuje użytkownika (lub zgłasza, że są one anonimowego). System autoryzacji jest to, co określa, czy użytkownik ma dostęp do żądanego zasobu. Jeśli użytkownik nie ma autoryzacji (jak znajdują się na rysunku 2, próbując odwiedzić anonimowo ProtectedPage.aspx), system autoryzacji zgłasza, że użytkownik ma zabroniony, powoduje formularzy systemu uwierzytelniania automatyczne przekierowanie do strony logowania użytkownika.

Po pomyślnym zalogowaniu się użytkownika, kolejne żądania HTTP obejmują biletu uwierzytelniania formularzy. System uwierzytelniania formularzy jedynie identyfikuje użytkownika — jest to system autoryzacji, która określa, czy użytkownik może uzyskiwać dostęp do żądanego zasobu.


![Przepływ uwierzytelniania formularzy](security-basics-and-asp-net-support-vb/_static/image2.png)

**Rysunek 2**: przepływ uwierzytelniania formularzy


Firma Microsoft będzie odnajdywać do uwierzytelniania formularzy znacznie bardziej szczegółowo w dwóch następnych samouczków[omówienie uwierzytelniania formularzy](an-overview-of-forms-authentication-vb.md) i [konfiguracji uwierzytelniania formularzy i Tematy zaawansowane](forms-authentication-configuration-and-advanced-topics-vb.md). Aby uzyskać więcej informacji na temat ASP. Opcje uwierzytelniania w sieci, zobacz [Uwierzytelnianie ASP.NET](https://msdn.microsoft.com/library/eeyk640h.aspx).

## <a name="limiting-access-to-web-pages-directories-and-page-functionality"></a>Ograniczanie dostępu do stron sieci Web, katalogów i funkcji strony

Program ASP.NET udostępnia dwa sposoby ustalić, czy dany użytkownik ma uprawnienia dostępu do określonego pliku lub katalogu:

- **Plik autoryzacji** — ponieważ stron ASP.NET i usługi sieci web są zaimplementowane jako pliki, które znajdują się w systemie plików serwera sieci web, dostęp do tych plików można określić za pomocą listy kontroli dostępu (ACL). Autoryzacja pliku jest najczęściej używane z uwierzytelnianiem systemu Windows, ponieważ listy ACL są uprawnienia, które są stosowane do kont systemu Windows. Podczas korzystania z uwierzytelniania formularzy, wszystkie żądania systemowe systemu operacyjnego i plików są wykonywane przez to samo konto systemu Windows, niezależnie od użytkownika, w tej witrynie.
- **Autoryzacja adresów URL**— z Autoryzacja adresów URL developer strony określa reguły autoryzacji w pliku Web.config. Te reguły autoryzacji Określ, co użytkownicy lub role mogą uzyskać dostęp lub odmówiono dostępu do określonych stron lub katalogów w aplikacji.

Plik uwierzytelnianie i Autoryzacja adresów URL należy zdefiniować reguły autoryzacji do uzyskiwania dostępu do określonej strony ASP.NET dla wszystkich stron ASP.NET z określonego katalogu. Przy użyciu tych metod możemy poinstruuj ASP.NET odrzucanie żądań do określonej strony dla danego użytkownika, lub zezwolić na dostęp do zbioru użytkowników i odmowa dostępu do osoby. Co sytuacji, gdy wszyscy użytkownicy mogą uzyskiwać dostęp do strony, ale funkcjonalność strony zależy od użytkownika? Na przykład wiele witryn, które obsługują konta użytkowników mają stron z inną zawartością lub dane dla uwierzytelnionych użytkowników w zależności od użytkowników anonimowych. Użytkownik anonimowy napotkać łącze do logowania się w lokacji, podczas gdy uwierzytelniony użytkownik zobaczy zamiast tego komunikatu, takich jak, Witamy z powrotem, *Username* wraz z linkiem się wylogować. Inny przykład: podczas wyświetlania elementu na witryną Zobacz różne informacje w zależności od tego, czy oferta najwyższa lub jeden system aukcji elementu.

Takie dostosowania na poziomie strony można osiągnąć deklaratywnie lub programowo. Aby wyświetlić różne zawartości dla anonimowego niż uwierzytelnionych użytkowników, po prostu przeciągania [formantu LoginView](https://msdn.microsoft.com/library/system.web.ui.webcontrols.loginview.aspx) na stronę, a następnie wprowadź odpowiednią zawartość do jego AnonymousTemplate i LoggedInTemplate szablonów. Alternatywnie, można programowo określić, czy bieżące żądanie jest uwierzytelniane, kto jest użytkownika oraz role należą one do (jeśli istnieje). Tych informacji umożliwia następnie Pokazywanie lub ukrywanie kolumn w siatce lub panele na stronie.

Ta seria zawiera trzy samouczków, z którymi się skupić na autoryzacji. ***Autoryzacji na podstawie użytkownika***sprawdza jak ograniczyć dostęp do stron w katalogu lub strony na określonych kont użytkowników; ***Autoryzacji na podstawie roli*** analizuje dostarczanie reguły autoryzacji w ramach roli poziomu; Ponadto ***wyświetlanie zawartości na podstawie obecnie zalogowany w użytkownika*** w tym samouczku przedstawiono modyfikacja szczególności zawartość i funkcje na podstawie użytkownika, odwiedzając stronę strony. Aby uzyskać więcej informacji na temat ASP. Opcje autoryzacji w sieci, zobacz [autoryzacji programu ASP.NET](https://msdn.microsoft.com/library/wce3kxhd.aspx).

## <a name="user-accounts-and-roles"></a>Konta użytkowników i ról

ŚRODOWISKO ASP. W sieci, uwierzytelnianie formularzy zapewnia infrastrukturę użytkownikom zalogować się do witryny i ich stanu uwierzytelnionego zapamiętanych między wizyt strony. I Autoryzacja adresów URL oferuje framework ograniczania dostępu do określonych plików lub folderów w aplikacji ASP.NET. Żadna funkcja dostarcza jednak sposób przechowywania informacji o koncie użytkownika lub zarządzania rolami.

Przed składnika ASP.NET 2.0 deweloperzy zostały odpowiedzialną za tworzenie własnych magazynów użytkownika i roli. Znajdowały się również w haku projektowanie interfejsów użytkownika i pisanie kodu dla użytkownika niezbędne stron związanych z kontem, takich jak strony logowania i stronę, aby utworzyć nowe konto, między innymi. Bez żadnych framework konto wbudowane użytkownika w programie ASP.NET, każdy Deweloper implementującej kont użytkowników musiały osiągnąć własnej decyzji projektowych na pytania, jak przechowywać hasła lub inne poufne informacje? i jakie wytyczne powinna nałożyć dotyczące długość hasła i siły?

Obecnie implementacja kont użytkowników w aplikacji ASP.NET jest znacznie prostsze dzięki *framework członkostwa* i wbudowane formanty sieci Web logowania. W ramach członkostwa jest kilka klas w [System.Web.Security przestrzeni nazw](https://msdn.microsoft.com/library/system.web.security.aspx) które zapewniają funkcje do wykonywania zadań związanych z kontem użytkownika podstawowych. Klasa klucza w ramach członkostwa jest [klasy członkostwa](https://msdn.microsoft.com/library/system.web.security.membership.aspx), która zawiera metody, takie jak:

- CreateUser
- DeleteUser
- GetAllUsers
- GetUser
- UpdateUser
- ValidateUser

Platforma członkostwa korzysta z [modelu dostawcy](http://aspnet.4guysfromrolla.com/articles/101905-1.aspx), który prawidłowo oddziela interfejsu API w ramach członkostwa od jego wykonania. Umożliwia deweloperom korzystanie ze wspólnego interfejsu API, ale upoważnia je, aby korzystać z implementacji zaspokoi potrzeby niestandardowych ich aplikacji. Krótko mówiąc klasa członkostwa definiuje podstawowych funkcji platformy (metody, właściwości i zdarzeń), ale faktycznie nie dostarcza żadnych szczegółów implementacji. Zamiast tego metody klasy członkostwa wywołania skonfigurowanego dostawcy, czyli co wykonuje całą pracę. Na przykład po wywołaniu metody włączenie klasa członkowska klasy członkostwa nie może ustalić szczegółowe informacje o magazynie użytkownika. Nie wiadomo, jeśli użytkownicy są obsługiwane w bazie danych lub w pliku XML lub w niektórych innym magazynie. Klasa członkowska sprawdza konfigurację aplikacji sieci web, aby ustalić, jakie dostawcy, aby delegować wywołanie i klasy dostawcy jest odpowiedzialny za faktycznie Tworzenie nowego konta użytkownika w magazynie odpowiedniego użytkownika. Rysunek 3 przedstawia interakcji.

Microsoft dwie klasy dostawcy członkostwa jest dostarczany w programie .NET Framework:

- [ActiveDirectoryMembershipProvider](https://msdn.microsoft.com/library/system.web.security.activedirectorymembershipprovider.aspx) -implementuje interfejs API członkostwa w usłudze Active Directory i tryb aplikacji usługi Active Directory (ADAM) serwerów.
- [SqlMembershipProvider](https://msdn.microsoft.com/library/system.web.security.sqlmembershipprovider.aspx) -implementuje interfejs API członkostwa w bazie danych programu SQL Server.

Ta seria samouczek koncentruje się wyłącznie na SqlMembershipProvider.


[![Dostawca modelu umożliwia różnych implementacje można bezproblemowo podłączony w ramach](security-basics-and-asp-net-support-vb/_static/image4.png)](security-basics-and-asp-net-support-vb/_static/image3.png)

**Rysunek 03**: Dostawca modelu umożliwia różnych implementacji można bezproblemowo podłączony w ramach ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](security-basics-and-asp-net-support-vb/_static/image5.png))


Zaletą modelu dostawcy jest alternatywnych implementacji może być opracowane przez firmę Microsoft, dostawców innych firm lub indywidualnych deweloperów i bezproblemowo podłączony w ramach członkostwa. Na przykład firma Microsoft wydała [dostawcę członkostwa dla baz danych programu Microsoft Access](https://download.microsoft.com/download/5/5/b/55bc291f-4316-4fd7-9269-dbf9edbaada8/sampleaccessproviders.vsi). Aby uzyskać więcej informacji o dostawcach członkostwa, zapoznaj się [Toolkit dostawcy](https://msdn.microsoft.com/asp.net/aa336558.aspx), w tym instruktażu dostawców członkostwa, przykładowe dostawców niestandardowych, ponad 100 stron dokumentacji modelu dostawcy i uzupełnianie kodu źródłowego dla wbudowanego dostawcy członkostwa (to znaczy, ActiveDirectoryMembershipProvider i SqlMembershipProvider).

Platforma ASP.NET 2.0 wprowadzono również framework ról. Jak framework członkostwa w ramach ról została stworzona z modelu dostawcy. Jego interfejsu API jest udostępniane za pośrednictwem funkcji [klasy ról](https://msdn.microsoft.com/library/system.web.security.roles.aspx) i .NET Framework jest dostarczany z trzech klas dostawcy:

- [Element AuthorizationStoreRoleProvider](https://msdn.microsoft.com/library/system.web.security.authorizationstoreroleprovider.aspx) -zarządza informacje o rolach w magazynie zasad Menedżera autoryzacji, takich jak usługa Active Directory lub ADAM.
- [SqlRoleProvider](https://msdn.microsoft.com/library/system.web.security.sqlroleprovider.aspx) -implementuje ról w bazie danych programu SQL Server.
- [WindowsTokenRoleProvider](https://msdn.microsoft.com/library/system.web.security.windowstokenroleprovider.aspx) -kojarzy informacje o rolach oparte na odwiedzającego grupy systemu Windows. Ta metoda jest zwykle używana z uwierzytelnianiem systemu Windows.

Ta seria samouczek koncentruje się wyłącznie na elementu SqlRoleProvider.

Ponieważ model dostawcy zawiera jeden interfejs API skierowany do przodu (klasy członkostwo i role), jest możliwe utworzenie funkcji wokół tego interfejsu API, nie martwiąc się o szczegóły implementacji — te są obsługiwane przez dostawców wybranych przez strony Developer. Ten ujednolicony interfejs API umożliwia do formantów sieci Web interfejsu z członkostwa oraz ról platformy firmy Microsoft i innych dostawców. Program ASP.NET jest dostarczany z liczbą [formanty sieci Web logowania](https://msdn.microsoft.com/library/ms178329.aspx) stosowania wspólnych interfejsów użytkownika konta użytkownika. Na przykład [formantu logowania](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.aspx) monituje użytkownika o podanie poświadczeń, sprawdza je i rejestruje je za pomocą uwierzytelniania formularzy. [Formantu LoginView](https://msdn.microsoft.com/library/system.web.ui.webcontrols.loginview.aspx) udostępnia szablony do wyświetlania różnych znaczników do użytkowników anonimowych, a użytkownicy uwierzytelnieni lub różnych znaczników na podstawie roli użytkownika. I [formancie CreateUserWizard](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.aspx) udostępnia interfejs użytkownika krok po kroku dotyczące tworzenia nowego konta użytkownika.

Poniżej obejmuje różne kontrolek logowania interakcji z platform członkostwa i ról. Większość kontrolek logowania można zaimplementować bez konieczności pisania pojedynczy wiersz kodu. Tych kontrolek bardziej szczegółowo omówione zostaną w przyszłości samouczki, w tym techniki rozszerzanie i dostosowywanie ich funkcje.

## <a name="summary"></a>Podsumowanie

Wszystkie aplikacje sieci web, które obsługują kont użytkowników wymagają podobne funkcje, takie jak: przez użytkowników do logowania i ich dziennika w stan zapamiętanych między wizyty strony. strony sieci web dla nowych użytkowników do tworzenia konta; i umożliwia określenie, jakie zasobów, dane i funkcje są dostępne do jakiego użytkowników lub ról deweloperowi strony. Zadania, uwierzytelniania i autoryzacji użytkowników i zarządzanie kontami użytkowników i ról jest niezwykle łatwo wykonać w aplikacjach ASP.NET dzięki użyciu uwierzytelniania formularzy, autoryzacja adresu URL i platform członkostwa i ról.

W ciągu następnej samouczki kilka zostaną omówione aspektami przez utworzenie działającą aplikację sieci web od podstaw w sposób krok po kroku. Następne dwa kroki samouczka przeanalizujemy uwierzytelnianie formularzy szczegółowo. Zostanie wyświetlone przepływu pracy uwierzytelniania formularzy w akcji, dissect biletu uwierzytelniania formularzy, omówimy zagadnienia dotyczące zabezpieczeń i zobacz, jak skonfigurować system uwierzytelniania formularzy — wszystkie podczas kompilowania aplikacji sieci web, która umożliwia zalogować się i wylogowania.

Programowanie przyjemność!

### <a name="further-reading"></a>Dalsze informacje

Więcej informacji dotyczących tematów omówionych w tym samouczku można znaleźć w następujących zasobach:

- [Platforma ASP.NET 2.0 członkostwo, role, uwierzytelnianie formularzy i zabezpieczeń zasobów](https://weblogs.asp.net/scottgu/ASP.NET-2.0-Membership_2C00_-Roles_2C00_-Forms-Authentication_2C00_-and-Security-Resources-)
- [Wytyczne dotyczące zabezpieczeń 2.0 ASP.NET](https://msdn.microsoft.com/library/ms998258.aspx)
- [Uwierzytelnianie ASP.NET](https://msdn.microsoft.com/library/eeyk640h.aspx)
- [Autoryzacja w programie ASP.NET](https://msdn.microsoft.com/library/wce3kxhd.aspx)
- [Omówienie kontrolki logowania programu ASP.NET](https://msdn.microsoft.com/library/ms178329.aspx)
- [Badanie ASP.NET 2.0 na członkostwo, role i profilu](http://aspnet.4guysfromrolla.com/articles/120705-1.aspx)
- [Jak I: zabezpieczyć przy użyciu członkostwo i role lokacji](https://asp.net/learn/videos/video-45.aspx) (Klip wideo)
- [Wprowadzenie do członkostwa](https://msdn.microsoft.com/library/yh26yfzy.aspx)
- [Centrum deweloperów zabezpieczeń MSDN](https://msdn.microsoft.com/security/default.aspx)
- [Professional ASP.NET 2.0 zabezpieczeń, członkostwo i zarządzanie rolami](http://www.wrox.com/WileyCDA/WroxTitle/productCd-0764596985.html) (ISBN: 978-0-7645-9698-8)
- [Zestaw narzędzi do dostawcy](https://msdn.microsoft.com/asp.net/aa336558.aspx)

## <a name="about-the-author"></a>Informacje o autorze

[Scott Bento](http://www.4guysfromrolla.com/ScottMitchell.shtml), autora siedmiu książek ASP/ASP.NET i twórcę z [4GuysFromRolla.com](http://www.4guysfromrolla.com), pracuje z technologii Microsoft Web od 1998. Scott działa jako niezależnego konsultanta trainer i składnika zapisywania. Jest jego najnowszej książki [ *Sams nauczyć się ASP.NET 2.0 w ciągu 24 godzin*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Piotr można uzyskać pod adresem [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) lub za pośrednictwem jego blog, który znajduje się w temacie [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Specjalne podziękowania dla

Ten samouczek serii zostało sprawdzone przez wiele recenzentów przydatne. Recenzenta realizacji w tym samouczku został w tym samouczku, który serii zostało sprawdzone przez wiele recenzentów przydatne. Prowadzić osób dokonujących przeglądu, w tym samouczku obejmują Alicja Maziarz, Jan Suru i Teresa Murphy. Zainteresowani recenzowania Moje nadchodzących artykuły MSDN? Jeśli tak, Porzuć mnie linii w [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Poprzednie](forms-authentication-configuration-and-advanced-topics-cs.md)
> [dalej](an-overview-of-forms-authentication-vb.md)
