---
uid: web-forms/overview/older-versions-security/admin/unlocking-and-approving-user-accounts-vb
title: Odblokowywanie i zatwierdzania konta użytkowników (VB) | Dokumentacja firmy Microsoft
author: rick-anderson
description: W tym samouczku przedstawiono sposób tworzenia strony sieci web, Administratorzy mogą skutecznie zarządzać zablokowane i zatwierdzone stanów użytkowników. Zostanie również wyświetlone nowych użytkowników o zatwierdzaniu...
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/01/2008
ms.topic: article
ms.assetid: 041854a5-ea8c-4de0-82f1-121ba6cb2893
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-security/admin/unlocking-and-approving-user-accounts-vb
msc.type: authoredcontent
ms.openlocfilehash: 977ed9d1e68e635eadd7aa8339670f5a5d209a7b
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/06/2018
ms.locfileid: "30891857"
---
<a name="unlocking-and-approving-user-accounts-vb"></a>Odblokowywanie i zatwierdzania konta użytkowników (VB)
====================
przez [Bento Scott](https://twitter.com/ScottOnWriting)

[Pobierz kod](http://download.microsoft.com/download/6/0/e/60e1bd94-e5f9-4d5a-a079-f23c98f4f67d/VB.14.zip) lub [pobierania plików PDF](http://download.microsoft.com/download/6/0/e/60e1bd94-e5f9-4d5a-a079-f23c98f4f67d/aspnet_tutorial14_UnlockAndApprove_vb.pdf)

> W tym samouczku przedstawiono sposób tworzenia strony sieci web, Administratorzy mogą skutecznie zarządzać zablokowane i zatwierdzone stanów użytkowników. Zostanie również wyświetlone zatwierdzaniu nowych użytkowników dopiero po sprawdzeniu swój adres e-mail.


## <a name="introduction"></a>Wprowadzenie

Wraz z nazwą użytkownika, hasło i adres e-mail, każde konto użytkownika ma dwa pola stanu, które wymuszają, czy użytkownik może zalogować się do witryny: zablokowane i zatwierdzone. Użytkownik jest zablokowane automatycznie, jeśli zapewniają nieprawidłowe poświadczenia określoną liczbę razy w ciągu określonej liczby minut (domyślne ustawienia zablokowania użytkownika po 5 nieudanych prób logowania w ciągu 10 minut). Zatwierdzone stanem jest to przydatne w scenariuszach, w której niektóre akcje, muszą orzeczona zanim nowy użytkownik będzie mógł zalogować się do witryny. Na przykład użytkownik może być konieczne najpierw Zweryfikuj swój adres e-mail lub być zatwierdzone przez administratora, aby można było do logowania.

Ponieważ zablokowanym out lub niezatwierdzonych użytkownik nie może się zalogować, jest tylko naturalnego do zastanawiasz się, jak można zresetować te stany. Program ASP.NET nie ma żadnych wbudowanych funkcji lub formantów sieci Web do zarządzania użytkowników zablokowane i częściowo zatwierdzone stany, ponieważ decyzje te muszą być obsługiwani na podstawie lokacji przez. Niektóre witryny może automatycznie zatwierdzać wszystkie nowe konta użytkowników (domyślnie). Inne poproś administratora o zatwierdzić nowych kont lub nie zatwierdzenia użytkowników, dopóki odwiedzeniu łącza wysyłane na adres e-mail podany podczas ich konta. Podobnie niektóre witryny może zablokować użytkowników, dopóki administrator resetuje stan, podczas gdy inne witryny wysłanie wiadomości e-mail do użytkownika zablokowane przy użyciu adresu URL, które mogą odwiedzać Aby odblokować konto.

W tym samouczku przedstawiono sposób tworzenia strony sieci web, Administratorzy mogą skutecznie zarządzać zablokowane i zatwierdzone stanów użytkowników. Zostanie również wyświetlone zatwierdzaniu nowych użytkowników dopiero po sprawdzeniu swój adres e-mail.

## <a name="step-1-managing-users-locked-out-and-approved-statuses"></a>Krok 1: Zarządzanie użytkowników zablokowane i zatwierdzone stanów

W <a id="Tutorial12"> </a> [ *tworzenia interfejsu można wybrać jedno konto użytkownika z wielu* ](building-an-interface-to-select-one-user-account-from-many-vb.md) samouczek możemy skonstruowany strony, znajdującego się na każdym koncie użytkownika w stronicowanej, filtrowane widoku GridView. Siatka wyświetla nazwę każdego użytkownika i poczty e-mail, ich stanów zatwierdzone i zablokowane, czy są one obecnie w trybie online i wszelkie komentarze dotyczące użytkownika. Aby zarządzać użytkowników zatwierdzona i zablokowane stany, firma Microsoft może wprowadzać ta Siatka można edytować. Aby zmienić stan zatwierdzone przez użytkownika, administrator będzie w pierwszej kolejności zlokalizować konta użytkownika, a następnie edytuj odpowiedni wiersz widoku GridView, sprawdzanie lub usunięcie zaznaczenia pola wyboru zatwierdzone. Alternatywnie firma Microsoft może zarządzać stanów zatwierdzone i zablokowane, za pomocą osobnych stron ASP.NET.

W tym samouczku można użyć dwóch stron ASP.NET: `ManageUsers.aspx` i `UserInformation.aspx`. Pomysł, w tym miejscu jest to, że `ManageUsers.aspx` Wyświetla listę kont użytkowników w systemie, podczas gdy `UserInformation.aspx` umożliwia administratorowi Zarządzanie stanów zatwierdzone i zablokowane dla określonego użytkownika. Naszym pierwszym działalnością of kolejności jest rozszerzyć w widoku GridView `ManageUsers.aspx` uwzględnienie pole hiperłącza HyperLinkField, która renderuje kolumną łącza. Chcemy każde łącze, aby wskazywał `UserInformation.aspx?user=UserName`, gdzie *UserName* jest nazwa użytkownika do edycji.

> [!NOTE]
> Jeśli pobrano kod <a id="Tutorial13"> </a> [ *odzyskiwanie i zmiana hasła* ](recovering-and-changing-passwords-vb.md) samouczka można zauważyć, że `ManageUsers.aspx` strona zawiera już zestaw " Zarządzanie"łącza i `UserInformation.aspx` strony udostępnia interfejs zmiany hasła wybranego użytkownika. I zrezygnowała replikować te funkcje w kodzie skojarzone z tego samouczka, ponieważ ci poszło obejścia interfejs API członkostwa i operacyjnego bezpośrednio z bazą danych programu SQL Server, aby zmienić hasło użytkownika. W tym samouczku rozpoczyna się od podstaw z `UserInformation.aspx` strony.


### <a name="adding-manage-links-to-theuseraccountsgridview"></a>Dodawanie "Manage" łącza do`UserAccounts`widoku GridView

Otwórz `ManageUsers.aspx` i dodać pole hiperłącza HyperLinkField do `UserAccounts` widoku GridView. Ustaw pole hiperłącza HyperLinkField `Text` dla właściwości "Manage" i jego `DataNavigateUrlFields` i `DataNavigateUrlFormatString` właściwości `UserName` i "UserInformation.aspx?user={0}", odpowiednio. Tych ustawień Skonfiguruj pole hiperłącza HyperLinkField w taki sposób, aby wyświetlić wszystkie hiperłącza tekst "Manage", ale przekazuje każde łącze w odpowiedniej *UserName* wartość na ciąg zapytania.

Po dodaniu pole hiperłącza HyperLinkField do widoku GridView, Poświęć chwilę, aby wyświetlić `ManageUsers.aspx` strony za pośrednictwem przeglądarki. Jak pokazano na rysunku 1, każdego wiersza w widoku GridView teraz zawiera link "Zarządzaj". Wskazuje link "Zarządzaj" Bruce `UserInformation.aspx?user=Bruce`, podczas gdy wskazuje link "Zarządzaj" Dave `UserInformation.aspx?user=Dave`.


[![Dodaje pole hiperłącza HyperLinkField](unlocking-and-approving-user-accounts-vb/_static/image2.png)](unlocking-and-approving-user-accounts-vb/_static/image1.png)

**Rysunek 1**: pole hiperłącza HyperLinkField dodaje łącze "Manage", dla każdego konta użytkownika ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](unlocking-and-approving-user-accounts-vb/_static/image3.png))


Firma Microsoft utworzy interfejsu użytkownika i kodu dla `UserInformation.aspx` strony w momencie, ale najpierw umożliwia poproś o jak programowo zmienić użytkownika zablokowane i zatwierdzone stanów. [ `MembershipUser` Klasy](https://msdn.microsoft.com/library/system.web.security.membershipuser.aspx) ma [ `IsLockedOut` ](https://msdn.microsoft.com/library/system.web.security.membershipuser.islockedout.aspx) i [ `IsApproved` właściwości](https://msdn.microsoft.com/library/system.web.security.membershipuser.isapproved.aspx). `IsLockedOut` Właściwość jest tylko do odczytu. Nie istnieje mechanizm do programowego zablokowania użytkownika; Aby odblokować użytkownika, należy użyć `MembershipUser` klasy [ `UnlockUser` metody](https://msdn.microsoft.com/library/system.web.security.membershipuser.unlockuser.aspx). `IsApproved` Właściwość jest do odczytu i zapisu. Aby zapisać zmiany w tej właściwości, należy wywołać `Membership` klasy [ `UpdateUser` metody](https://msdn.microsoft.com/library/system.web.security.membership.updateuser.aspx), przekazując zmodyfikowanych `MembershipUser` obiektu.

Ponieważ `IsApproved` właściwość do odczytu i zapisu, pole wyboru jest prawdopodobnie najlepsze elementu interfejsu użytkownika dotyczące konfigurowania tej właściwości. Jednak pole wyboru nie będzie działać dla `IsLockedOut` właściwości, ponieważ administrator nie zablokowania użytkownika, użytkownik może tylko odblokowanie użytkownika. Interfejs użytkownika odpowiedniego `IsLockedOut` właściwości jest przycisk, po kliknięciu odblokowuje konto użytkownika. Ten przycisk powinien być włączony tylko, jeśli użytkownik jest zablokowany.

### <a name="creating-theuserinformationaspxpage"></a>Tworzenie`UserInformation.aspx`strony

Firma Microsoft są teraz gotowe do implementacji interfejsu użytkownika w `UserInformation.aspx`. Otwórz tę stronę i dodaj następujące formantów sieci Web:

- Hiperłącze kontrolować, po kliknięciu zwraca administratorowi `ManageUsers.aspx` strony.
- Kontrolka etykiety w sieci Web do wyświetlania nazwę wybranego użytkownika. Ustaw tę etykietę `ID` do `UserNameLabel` i wyczyszczenie właściwości Text.
- Formant wyboru o nazwie `IsApproved`. Ustaw jego `AutoPostBack` właściwości `True`.
- Etykieta do wyświetlania użytkownika obiektu ostatniego zablokowane Data. Nazwa tej etykiety `LastLockedOutDateLabel` i wyczyszczenie jego `Text` właściwości.
- Przycisk odblokowanie użytkownika. Nazwa tego przycisku `UnlockUserButton` i ustawić jej `Text` dla właściwości "Odblokuj użytkownika".
- Etykieta do wyświetlania komunikatów o stanie, takie jak "został zaktualizowany stan zatwierdzone użytkownika". Nazwa tego formantu `StatusMessage`, wyczyść się jego `Text` właściwości i ustaw jego `CssClass` właściwości `Important`. ( `Important` CSS klasa jest zdefiniowana w `Styles.css` pliku arkusza stylów; wyświetla odpowiedni tekst w dużych, red czcionki.)

Po dodaniu tych kontrolek, widok projektu w programie Visual Studio powinien wyglądać podobnie do ekranu zrzut na rysunku 2.


[![Tworzenie interfejsu użytkownika dla UserInformation.aspx](unlocking-and-approving-user-accounts-vb/_static/image5.png)](unlocking-and-approving-user-accounts-vb/_static/image4.png)

**Rysunek 2**: Tworzenie interfejsu użytkownika dla `UserInformation.aspx` ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](unlocking-and-approving-user-accounts-vb/_static/image6.png))


Pełne interfejsu użytkownika jest skonfigurowanie naszych następne zadanie `IsApproved` wyboru i inne formanty oparte na informacje wybranego użytkownika. Tworzenie procedury obsługi zdarzeń dla strony `Load` zdarzeń i Dodaj następujący kod:

[!code-vb[Main](unlocking-and-approving-user-accounts-vb/samples/sample1.vb)]

Powyższy kod rozpoczyna się przez zapewnienie, że jest to pierwsza wizyta strony i nie kolejnych ogłaszania zwrotnego. Następnie odczytuje username przekazywane `user` pole querystring i pobiera informacje o tym koncie użytkownika za pośrednictwem `Membership.GetUser(username)` metody. Jeśli username nie został dostarczony przez ciąg zapytania, lub jeśli nie można odnaleźć określonego użytkownika, administratora są wysyłane do `ManageUsers.aspx` strony.

`MembershipUser` Obiektu `UserName` są następnie wyświetlane wartości `UserNameLabel` i `IsApproved` jest zaznaczone pole wyboru na podstawie `IsApproved` wartości właściwości.

`MembershipUser` Obiektu [ `LastLockoutDate` właściwości](https://msdn.microsoft.com/library/system.web.security.membershipuser.lastlockoutdate.aspx) zwraca `DateTime` wartość wskazującą, kiedy użytkownik został ostatnio zablokowane. Jeśli użytkownik nigdy nie zostało zablokowane, wartość zwracana jest zależna od dostawcy członkostwa. Po utworzeniu nowego konta `SqlMembershipProvider` ustawia `aspnet_Membership` tabeli `LastLockoutDate` do `1754-01-01 12:00:00 AM`. Powyższy kod wyświetla pusty ciąg `LastLockoutDateLabel` Jeśli `LastLockoutDate` właściwość występuje przed rokiem 2000; w przeciwnym razie daty część `LastLockoutDate` właściwość jest wyświetlana w etykiecie. `UnlockUserButton`w `Enabled` właściwość jest ustawiona na użytkownika zablokowane w stanie, co oznacza, że tego przycisku zostanie tylko włączone, jeśli użytkownik jest zablokowany.

Poświęć chwilę, aby przetestować `UserInformation.aspx` strony za pośrednictwem przeglądarki. Należy rozpocząć od `ManageUsers.aspx` i wybierz konto użytkownika do zarządzania. Po otrzymywanych `UserInformation.aspx`, należy pamiętać, że `IsApproved` tylko jest zaznaczone pole wyboru, jeśli użytkownik jest zatwierdzony. Jeśli użytkownik kiedykolwiek zostało zablokowane, wyświetlany jest ostatnią zablokowane daty. Przycisk Odblokuj użytkownika jest włączona tylko wtedy, gdy użytkownik jest obecnie zablokowane. Zaznaczenie lub usunięcie zaznaczenia `IsApproved` wyboru lub przycisku odblokowanie użytkownika powoduje odświeżenie strony, ale nie modyfikacje są dokonywane na koncie użytkownika, ponieważ jeszcze mamy do tworzenie obsługi zdarzeń dla tych zdarzeń.

Wróć do programu Visual Studio i utworzyć obsługi zdarzeń `IsApproved` składnika CheckBox `CheckedChanged` zdarzeń i `UnlockUser` przycisku `Click` zdarzeń. W `CheckedChanged` program obsługi zdarzeń, Ustaw użytkownika `IsApproved` właściwości `Checked` właściwości pola wyboru, a następnie zapisz zmiany za pomocą wywołania `Membership.UpdateUser`. W `Click` program obsługi zdarzeń, po prostu wywołanie `MembershipUser` obiektu `UnlockUser` metody. W obu programów obsługi zdarzeń wyświetlony odpowiedni komunikat w `StatusMessage` etykiety.

[!code-vb[Main](unlocking-and-approving-user-accounts-vb/samples/sample2.vb)]

### <a name="testing-theuserinformationaspxpage"></a>Testowanie`UserInformation.aspx`strony

Z tych obsługi zdarzeń w miejscu, należy ponownie strony i niezatwierdzonych przez użytkownika. Jak pokazano na rysunku 3, powinny pojawić się krótkie komunikatów na stronie, co oznacza, że użytkownika `IsApproved` właściwość została pomyślnie zmodyfikowana.


[![Krzysztof został niezatwierdzone](unlocking-and-approving-user-accounts-vb/_static/image8.png)](unlocking-and-approving-user-accounts-vb/_static/image7.png)

**Rysunek 3**: Krzysztof został niezatwierdzone ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](unlocking-and-approving-user-accounts-vb/_static/image9.png))


Następnie, wyloguj się i spróbuj zalogować się jako użytkownik, którego konto zostało właśnie niezatwierdzonych. Ponieważ użytkownik nie jest zatwierdzona, nie można zalogować. Domyślnie ten sam komunikat wyświetla formantu logowania, jeśli użytkownik nie może się zalogować, niezależnie od przyczyny. Jednak <a id="Tutorial6"> </a> [ *weryfikowania użytkowników przed członkostwa użytkownika magazynu poświadczeń* ](../membership/validating-user-credentials-against-the-membership-user-store-vb.md) samouczek analizujemy udoskonalanie formantu logowania, aby wyświetlić bardziej odpowiednia wiadomość. Jak pokazano na rysunku 4, Krzysztof jest wyświetlany komunikat z informacjami o tym, że nie można zalogować się on, ponieważ jego konto nie jest jeszcze zatwierdzone.


[![Krzysztof nie ponieważ jego konto logowania jest niezatwierdzone](unlocking-and-approving-user-accounts-vb/_static/image11.png)](unlocking-and-approving-user-accounts-vb/_static/image10.png)

**Rysunek 4**: Krzysztof nie ponieważ jego konto logowania jest niezatwierdzone ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](unlocking-and-approving-user-accounts-vb/_static/image12.png))


Aby przetestować funkcje zablokowane, spróbuj zalogować się jako użytkownik zatwierdzone, ale niepoprawne hasło. Powtórz ten proces niezbędne liczbę razy, aż konto użytkownika zostało zablokowane. Kontrolka logowania Zaktualizowano również Pokaż niestandardowego komunikatu, jeśli próba logowania z blokady konta. Wiadomo, że po uruchomieniu następujący komunikat na stronie logowania wyświetlany zostało zablokowane konta: "Twoje konto zostało zablokowane z powodu zbyt wielu nieudanych prób logowania. Skontaktuj się z administratorem, aby odblokować konto."

Wróć do `ManageUsers.aspx` i kliknij przycisk Zarządzaj link blokady użytkownika. Jak pokazano na rysunku nr 5, powinna zostać wyświetlona wartość w `LastLockedOutDateLabel` przycisku odblokowanie użytkownika powinno być włączone. Kliknij przycisk odblokowanie użytkownika, aby odblokować konto użytkownika. Gdy użytkownik ma odblokowany, będą oni mogli zalogować się ponownie.


[![Dave została ograniczona do systemu](unlocking-and-approving-user-accounts-vb/_static/image14.png)](unlocking-and-approving-user-accounts-vb/_static/image13.png)

**Rysunek 5**: Dave ma zostało zablokowane z systemu ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](unlocking-and-approving-user-accounts-vb/_static/image15.png))


## <a name="step-2-specifying-new-users-approved-status"></a>Krok 2: Określenie nowych użytkowników zatwierdzone stanu

Zatwierdzone stanem jest to przydatne w scenariuszach, w którym ma niektóre akcje do wykonania, zanim nowy użytkownik będzie mógł zalogować się i uzyskiwać dostęp do funkcji specyficzne dla użytkownika witryny. Na przykład być może używasz prywatnej witryny sieci Web gdzie wszystkich stron, z wyjątkiem strony logowania i zapisywania są dostępne tylko dla użytkowników uwierzytelnionych. Ale co się stanie, jeśli obcej osoby osiągnie witryny sieci Web, znajduje stronę rejestracji i tworzy konto? Aby temu zapobiec, można przenieść strony rejestracji, aby `Administration` folderu i wymaga się, że administrator ręcznie tworzyć każdego konta. Alternatywnie można pozwala nikomu rejestracji, ale uniemożliwiają dostęp do witryny, aż administrator zatwierdzi konta użytkownika.

Domyślnie w formancie CreateUserWizard zatwierdza nowe konta. Można skonfigurować tego zachowania, za pomocą formantu [ `DisableCreatedUser` właściwości](https://msdn.microsoft.com/en-gb/library/system.web.ui.webcontrols.createuserwizard.disablecreateduser.aspx). Ta właściwość jest ustawiana `True` nie zatwierdzić nowych kont użytkowników.

> [!NOTE]
> Domyślnie w formancie CreateUserWizard automatyczne logowanie na koncie użytkownika. To zachowanie jest zależna formantu [ `LoginCreatedUser` właściwości](https://msdn.microsoft.com/en-gb/library/system.web.ui.webcontrols.createuserwizard.logincreateduser.aspx). Ponieważ użytkownicy niezatwierdzonych nie może zalogować się do witryny, gdy `DisableCreatedUser` jest `True` nowe konto użytkownika nie jest zalogowany do lokacji, niezależnie od wartości `LoginCreatedUser` właściwości.


Programowo w przypadku tworzenia nowych kont użytkowników za pomocą `Membership.CreateUser` metody, aby utworzyć konto użytkownika niezatwierdzonych Użyj jednego z przeciążeń, które akceptują nowego użytkownika `IsApproved` wartości właściwości jako parametr wejściowy.

## <a name="step-3-approving-users-by-verifying-their-email-address"></a>Krok 3: Zatwierdzanie użytkowników, sprawdzając swój adres E-mail

Wiele witryn sieci Web, które obsługują kont użytkowników nie Akceptuj nowych użytkowników, dopóki ich weryfikacji adresu e-mail, który one dostarczane podczas rejestrowania. Ten proces sprawdzania poprawności jest najczęściej używany do oszustw robotów, nadawcy i innych ne'er-do-wells, ponieważ wymaga adresu e-mail unikatowy, zweryfikowane i dodaje dodatkowego kroku w procesie rejestracji. Z tym modelem po zarejestrowaniu nowego użytkownika są one wysyłane wiadomości e-mail, który zawiera łącze do strony weryfikacji. Odwiedzając łącze użytkownika przebiegło, aby otrzymały one wiadomości e-mail i w związku z tym, że podany adres e-mail jest prawidłowa. Na stronie weryfikacji jest odpowiedzialny za zatwierdzanie użytkownika. Może to mieć miejsce automatycznie, a tym samym zatwierdzanie osiągnie tej strony, użytkownik lub tylko wtedy, gdy użytkownik udostępnia dodatkowe informacje, takie jak [CAPTCHA](http://en.wikipedia.org/wiki/Captcha).

Aby uwzględnić ten przepływ pracy, należy najpierw zaktualizować strony tworzenia konta, tak aby nowi użytkownicy są niezatwierdzonych. Otwórz `EnhancedCreateUserWizard.aspx` strony `Membership` folder i zestaw formancie CreateUserWizard `DisableCreatedUser` właściwości `True`.

Następnie należy skonfigurować w formancie CreateUserWizard do wysyłania wiadomości e-mail do nowego użytkownika z instrukcjami zweryfikować swoje konto. W szczególności firma Microsoft zawiera łącze w wiadomości e-mail, aby `Verification.aspx` strony (który mamy jeszcze do tworzenia), przekazując nowego użytkownika `UserId` za pośrednictwem ciąg zapytania. `Verification.aspx` Strony będzie wyszukać określonego użytkownika i oznacz je zatwierdzone.

### <a name="sending-a-verification-email-to-new-users"></a>Wysyłając wiadomość E-mail z weryfikacji do nowych użytkowników

Aby wysłać wiadomość e-mail w formancie CreateUserWizard, skonfiguruj jego `MailDefinition` właściwości odpowiednio. Zgodnie z opisem w <a id="Tutorial13"> </a> [poprzedniego samouczek](recovering-and-changing-passwords-vb.md), dostępne kontrolki Element ChangePassword i PasswordRecovery [ `MailDefinition` właściwości](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.maildefinition.aspx) działający w taki sam sposób jak CreateUserWizard formantu.

> [!NOTE]
> Aby użyć `MailDefinition` opcje właściwości, należy określić dostarczanie poczty w `Web.config`. Aby uzyskać więcej informacji, zapoznaj się [wysyłania poczty E-mail w programie ASP.NET](http://aspnet.4guysfromrolla.com/articles/072606-1.aspx).


Rozpocznij od utworzenia nowego szablonu wiadomości e-mail o nazwie `CreateUserWizard.txt` w `EmailTemplates` folderu. Używany następujący tekst:

[!code-aspx[Main](unlocking-and-approving-user-accounts-vb/samples/sample3.aspx)]

Ustaw `MailDefinition`w `BodyFileName` dla właściwości "~ / EmailTemplates/CreateUserWizard.txt" i jego `Subject` dla właściwości "Zapraszamy do witryny sieci Web! Aktywuj konta."

Należy pamiętać, że `CreateUserWizard.txt` szablon wiadomości e-mail zawiera `<%VerificationUrl%>` symbolu zastępczego. Jest to, gdy adres URL `Verification.aspx` strony zostaną umieszczone. Automatycznie zastępuje CreateUserWizard `<%UserName%>` i `<%Password%>` symbole zastępcze z nazwy użytkownika i hasła, nowe konto, ale nie istnieje żadne wbudowane `<%VerificationUrl%>` symbolu zastępczego. Należy ręcznie, zastąp ją odpowiednią weryfikacji adresu URL.

W tym celu należy utworzyć program obsługi zdarzeń dla CreateUserWizard [ `SendingMail` zdarzeń](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.sendingmail.aspx) i Dodaj następujący kod:

[!code-vb[Main](unlocking-and-approving-user-accounts-vb/samples/sample4.vb)]

`SendingMail` Zdarzenia są generowane po `CreatedUser` zdarzeń, co oznacza, że w czasie, nowy użytkownik wykonuje powyższe procedury obsługi zdarzeń konto już istnieje. Firma Microsoft mogą uzyskiwać dostęp do nowego użytkownika `UserId` wartość wywołując `Membership.GetUser` metody, przekazując `UserName` w formancie CreateUserWizard. Następnie jest sformatowany adres URL weryfikacji. Instrukcja `Request.Url.GetLeftPart(UriPartial.Authority)` zwraca `http://yourserver.com` część adresu URL; `Request.ApplicationPath` zwraca ścieżkę, gdzie aplikacja. Weryfikacja adresu URL jest następnie zdefiniowany jako `Verification.aspx?ID=userId`. Te dwa ciągi, które następnie są łączone do utworzenia pełny adres URL. Na koniec treść wiadomości e-mail (`e.Message.Body`) zawiera wszystkie wystąpienia `<%VerificationUrl%>` zastąpione pełny adres URL.

Net powoduje, że nowi użytkownicy są niezatwierdzonych, co oznacza, że nie można zalogować do witryny. Ponadto one są automatycznie wysyłane wiadomości e-mail z łączem do weryfikacji adresu URL (patrz rysunek 6).


[![Nowy użytkownik otrzymuje wiadomość E-mail zawierającą łącze do adresu URL weryfikacji](unlocking-and-approving-user-accounts-vb/_static/image17.png)](unlocking-and-approving-user-accounts-vb/_static/image16.png)

**Rysunek 6**: nowy użytkownik otrzymuje wiadomość E-mail zawierającą łącze do adresu URL weryfikacji ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](unlocking-and-approving-user-accounts-vb/_static/image18.png))


> [!NOTE]
> Krok CreateUserWizard domyślne w formancie CreateUserWizard wyświetla komunikat informujący użytkownika swoje konto zostało utworzone i wyświetla przycisku Kontynuuj. Kliknięcie łącza przejście użytkownika na adres URL określony przez formant `ContinueDestinationPageUrl` właściwości. CreateUserWizard w `EnhancedCreateUserWizard.aspx` jest skonfigurowany do wysyłania nowych użytkowników do `~/Membership/AdditionalUserInfo.aspx`, które monituje użytkownika o ich hometown, adres URL strony głównej i podpis. Ponieważ te informacje mogą być dodawane tylko przez zalogowanych użytkowników, warto zaktualizować tę właściwość, aby wysłać użytkowników z powrotem do strony głównej witryny (`~/Default.aspx`). Ponadto `EnhancedCreateUserWizard.aspx` strony lub krok CreateUserWizard powinien zostać rozszerzony, aby poinformować użytkownika, że zostały wysłane e-mail weryfikacji i nie będzie można aktywować swoje konto, aż do ich postępuj zgodnie z instrukcjami w wiadomości e-mail. Te modyfikacje wykonywania I pozostaw dla czytnika.


### <a name="creating-the-verification-page"></a>Tworzenie strony weryfikacji

Nasze ostatnim zadaniem jest tworzenie `Verification.aspx` strony. Dodaj tę stronę do folderu głównego było możliwe skojarzenie go z `Site.master` strony wzorcowej. Jak możemy wykonane z większością poprzedniej strony zawartości dodane do lokacji, usuń formant zawartości, który odwołuje się do `LoginContent` ContentPlaceHolder tak, aby strona zawartości używa strony wzorcowej domyślne zawartości.

Dodawanie formantu etykiety w sieci Web do `Verification.aspx` ustaw jego `ID` do `StatusMessage` i wyczyszczenie właściwości text. Następnie należy utworzyć `Page_Load` obsługi zdarzeń i Dodaj następujący kod:

[!code-vb[Main](unlocking-and-approving-user-accounts-vb/samples/sample5.vb)]

Sprawdza zbiorczego powyższy kod UserId dostarczane za pośrednictwem ciąg zapytania, że istnieje i czy jest prawidłową `Guid` wartość i że jego odwołuje się do istniejącego konta użytkownika. Jeśli wszystkie te kontroli przekazać, konto użytkownika zostało zatwierdzone; w przeciwnym razie zostanie wyświetlony komunikat odpowiedni stan.

Rysunek nr 7 przedstawia `Verification.aspx` strony po odwiedzeniu za pośrednictwem przeglądarki.


[![Nowe konta jest teraz zatwierdzony](unlocking-and-approving-user-accounts-vb/_static/image20.png)](unlocking-and-approving-user-accounts-vb/_static/image19.png)

**Rysunek 7**: nowe konto użytkownika jest teraz zatwierdzone ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](unlocking-and-approving-user-accounts-vb/_static/image21.png))


## <a name="summary"></a>Podsumowanie

Wszystkie konta użytkowników członkostwa mają dwa stany, które określają, czy użytkownik może zalogować się do witryny: `IsLockedOut` i `IsApproved`. Obie te właściwości musi być `True` do logowania użytkownika.

Zablokowane przez użytkownika stanu jest używany ze względów bezpieczeństwa, aby zmniejszyć prawdopodobieństwo haker okazji do witryny za pomocą metod siłowych. W szczególności użytkownika jest zablokowane, jeśli istnieje wiele nieudanych prób logowania w przedziale czasu. Te granice są konfigurowane za pomocą ustawień dostawcy członkostwa w `Web.config`.

Stan zatwierdzone jest najczęściej używany jako środek do uniemożliwić logowanie do momentu upłynął niektóre akcje nowych użytkowników. Prawdopodobnie lokacji wymaga, aby nowe konta najpierw zostać zatwierdzone przez administratora lub jako widzieliśmy w kroku 3, sprawdzając swój adres e-mail.

Programowanie przyjemność!

### <a name="about-the-author"></a>Informacje o autorze

Scott Bento, Utwórz wiele książek ASP/ASP.NET i twórcę 4GuysFromRolla.com, pracuje z technologii Microsoft Web od 1998. Scott działa jako niezależnego konsultanta trainer i składnika zapisywania. Jest jego najnowszej książki  *[Sams nauczyć się ASP.NET 2.0 w ciągu 24 godzin](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)*. Scott jest osiągalny w [ mitchell@4guysfromrolla.com ](mailto:mitchell@4guysfromrolla.com) lub za pośrednictwem jego blogu w [ http://ScottOnWriting.NET ](http://scottonwriting.net/).

### <a name="special-thanks-to"></a>Specjalne podziękowania dla...

Ten samouczek serii zostało sprawdzone przez wiele recenzentów przydatne. Zainteresowani recenzowania Moje nadchodzących artykuły MSDN? Jeśli tak, Porzuć mnie linii w [mitchell@4GuysFromRolla.com](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Poprzednie](recovering-and-changing-passwords-vb.md)
