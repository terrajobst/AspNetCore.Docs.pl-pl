---
uid: web-forms/overview/older-versions-security/admin/unlocking-and-approving-user-accounts-cs
title: Odblokowywanie i zatwierdzanie kont użytkowników (C#) | Dokumentacja firmy Microsoft
author: rick-anderson
description: W tym samouczku przedstawiono sposób tworzenia strony sieci web dla administratorów do zarządzania zablokowane i zatwierdzone stanów użytkowników. Firma Microsoft pojawi się także nowych użytkowników o zatwierdzaniu...
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/01/2008
ms.topic: article
ms.assetid: 5346aab1-9974-489f-a065-ae3883b8a350
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/older-versions-security/admin/unlocking-and-approving-user-accounts-cs
msc.type: authoredcontent
ms.openlocfilehash: f6bf76579898a8ff36b18380100ce4ab2e74fc8d
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/03/2018
ms.locfileid: "37392717"
---
<a name="unlocking-and-approving-user-accounts-c"></a>Odblokowywanie i zatwierdzanie kont użytkowników (C#)
====================
przez [Bento Scott](https://twitter.com/ScottOnWriting)

[Pobierz program Code](http://download.microsoft.com/download/6/0/e/60e1bd94-e5f9-4d5a-a079-f23c98f4f67d/CS.14.zip) lub [Pobierz plik PDF](http://download.microsoft.com/download/6/0/e/60e1bd94-e5f9-4d5a-a079-f23c98f4f67d/aspnet_tutorial14_UnlockAndApprove_cs.pdf)

> W tym samouczku przedstawiono sposób tworzenia strony sieci web dla administratorów do zarządzania zablokowane i zatwierdzone stanów użytkowników. Firma Microsoft pojawi się także sposób zatwierdzenia nowych użytkowników tylko wtedy, gdy ich zweryfikowaniu swojego adresu e-mail.


## <a name="introduction"></a>Wprowadzenie

Wraz z nazwy użytkownika, hasło i adres e-mail, każde konto użytkownika ma dwa pola stanu, które określają, czy użytkownik może zalogować się do witryny: zablokowane i zatwierdzone. Użytkownik jest zablokowane automatycznie, jeśli zapewniają one nieprawidłowe poświadczenia określoną liczbę razy w ciągu określonej liczby minut (ustawienie domyślne zablokowania użytkownika po 5 nieudanych prób logowania w ciągu 10 minut). Stan zatwierdzone jest przydatne w scenariuszach, gdzie niektóre akcje muszą orzeczona zanim nowy użytkownik będzie mógł zalogować się do witryny. Na przykład użytkownik może być konieczne najpierw sprawdź swój adres e-mail lub być zatwierdzone przez administratora, zanim będzie mógł logowania.

Ponieważ zablokowany out lub niezatwierdzonych użytkownika nie mogą się zalogować, jest tylko naturalnego, aby dowiedzieć się, jak można zresetować te stany. Program ASP.NET nie zawiera żadnych funkcji wbudowanych lub kontrolki sieci Web do zarządzania użytkowników zablokowane i częściowo zatwierdzone stany, ponieważ te decyzje muszą być obsługiwane na podstawie lokacji przez. Niektóre witryny może automatycznie zatwierdzaj wszystkich nowych kont użytkowników (domyślnie). Inne osoby mają uprawnienia administratora zatwierdzić nowych kont lub nie zostanie zatwierdzona użytkowników do momentu, gdy odwiedzą łącze wysłane na adres e-mail podany przy wywołaniu metody zarejestrowali. Podobnie niektóre witryny może zablokować użytkowników, aż administrator resetuje ich stan, podczas gdy inne witryny, Wyślij wiadomość e-mail do blokady użytkownika przy użyciu adresu URL, które mogą odwiedzać, aby odblokować konto.

W tym samouczku przedstawiono sposób tworzenia strony sieci web dla administratorów do zarządzania zablokowane i zatwierdzone stanów użytkowników. Firma Microsoft pojawi się także sposób zatwierdzenia nowych użytkowników tylko wtedy, gdy ich zweryfikowaniu swojego adresu e-mail.

## <a name="step-1-managing-users-locked-out-and-approved-statuses"></a>Krok 1: Zarządzanie użytkownikami zablokowane i zatwierdzone Stany

W <a id="Tutorial12"> </a> [ *kompilowanie interfejsu służącego wybierz jedno konto użytkownika z wielu* ](building-an-interface-to-select-one-user-account-from-many-cs.md) samouczka będziemy skonstruowany strona, która każdego konta użytkownika w stronicowane, na liście filtrowane GridView. Siatka wyświetla nazwę każdego użytkownika i poczty e-mail, ich stan zatwierdzone i zablokowane, czy są one obecnie w trybie online i wszelkie komentarze dotyczące użytkownika. Do zarządzania użytkownikami, zatwierdzona i zablokowane stany, firma Microsoft może spowodować, że tej siatki edytowalnych. Aby zmienić stan zatwierdzone przez użytkownika, administrator będzie w pierwszej kolejności zlokalizować konta użytkownika, a następnie edytuj odpowiedni wiersz GridView, sprawdzania lub usuwając zaznaczenie pola wyboru zatwierdzone. Alternatywnie można zarządzać stany zatwierdzone i zablokowane, za pośrednictwem osobnej stronie ASP.NET.

W tym samouczku użyjemy dwóch stron ASP.NET: `ManageUsers.aspx` i `UserInformation.aspx`. W tym miejscu chodzi o to, że `ManageUsers.aspx` Wyświetla listę kont użytkowników w systemie, podczas gdy `UserInformation.aspx` umożliwia administratorowi Zarządzanie stany zatwierdzone i blokady dla określonego użytkownika. Naszym pierwszym zadaniem biznesowym jest rozszerzyć GridView w `ManageUsers.aspx` obejmujący pole hiperłącza HyperLinkField, która renderuje kolumną łącza. Chcemy, aby każdy link, aby wskazywał `UserInformation.aspx?user=UserName`, gdzie *UserName* to nazwa użytkownika do edycji.

> [!NOTE]
> Jeśli pobrano kod <a id="Tutorial13"> </a> [ *odzyskiwanie i zmienianie haseł* ](recovering-and-changing-passwords-cs.md) samouczek, być może zauważono, że `ManageUsers.aspx` strony już zawiera zbiór " Zarządzanie"linki i `UserInformation.aspx` strony udostępnia interfejs dla zmiany hasła wybranego użytkownika. Czy mogę zrezygnowała z replikowanie tej funkcji w kodzie, skojarzone z tym samouczkiem, ponieważ poszło obejściu API członkostwa i operacyjnego bezpośrednio z bazą danych programu SQL Server, aby zmienić hasło użytkownika. Ten samouczek rozpoczyna się od podstaw przy użyciu `UserInformation.aspx` strony.


### <a name="adding-manage-links-to-theuseraccountsgridview"></a>Dodawanie "Manage" linki do`UserAccounts`GridView

Otwórz `ManageUsers.aspx` strony, a następnie dodaj pole hiperłącza HyperLinkField do `UserAccounts` GridView. Ustaw pole hiperłącza HyperLinkField `Text` właściwość "Manage" i jego `DataNavigateUrlFields` i `DataNavigateUrlFormatString` właściwości `UserName` i "UserInformation.aspx?user={0}", odpowiednio. Te ustawienia skonfigurować pole hiperłącza HyperLinkField w taki sposób, że wszystkie łącza wyświetlania tekstu "Manage", ale każde połączenie, przekazuje w odpowiedniej *UserName* wartość w zmiennej querystring.

Po dodaniu pole hiperłącza HyperLinkField do kontrolki GridView, Poświęć chwilę na wyświetlanie `ManageUsers.aspx` strony za pośrednictwem przeglądarki. Jak pokazano na rysunku 1, każdy wiersz GridView obejmuje teraz link "Zarządzaj". Link "Zarządzaj", aby Bruce wskazuje `UserInformation.aspx?user=Bruce`, a link "Zarządzaj" Dave wskazuje `UserInformation.aspx?user=Dave`.


[![Dodaje pole hiperłącza HyperLinkField](unlocking-and-approving-user-accounts-cs/_static/image2.png)](unlocking-and-approving-user-accounts-cs/_static/image1.png)

**Rysunek 1**: pole hiperłącza HyperLinkField dodaje Link "Zarządzaj" dla każdego konta użytkownika ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](unlocking-and-approving-user-accounts-cs/_static/image3.png))


Firma Microsoft tworzymy interfejs użytkownika i kodu dla `UserInformation.aspx` strony w tej chwili, ale pierwszym teraz rozmawiać o jak programowo zmienić użytkownika zablokowane i zatwierdzone stanów. [ `MembershipUser` Klasy](https://msdn.microsoft.com/library/system.web.security.membershipuser.aspx) ma [ `IsLockedOut` ](https://msdn.microsoft.com/library/system.web.security.membershipuser.islockedout.aspx) i [ `IsApproved` właściwości](https://msdn.microsoft.com/library/system.web.security.membershipuser.isapproved.aspx). `IsLockedOut` Właściwość jest tylko do odczytu. Nie ma mechanizmu programowo blokowania użytkownikowi. Aby odblokować użytkownika, należy użyć `MembershipUser` klasy [ `UnlockUser` metoda](https://msdn.microsoft.com/library/system.web.security.membershipuser.unlockuser.aspx). `IsApproved` Właściwość jest Odczytywalny i zapisywalny. Aby zapisać zmiany tej właściwości, należy wywołać `Membership` klasy [ `UpdateUser` metoda](https://msdn.microsoft.com/library/system.web.security.membership.updateuser.aspx), przekazując zmodyfikowanego `MembershipUser` obiektu.

Ponieważ `IsApproved` właściwość jest Odczytywalny i zapisywalny, formant pola wyboru jest prawdopodobnie najlepsze elementu interfejsu użytkownika dotyczące konfigurowania tej właściwości. Jednakże, pole wyboru nie będzie działać dla `IsLockedOut` właściwość, ponieważ administrator nie może zablokować się użytkownika, użytkownik może tylko odblokowanie użytkownika. Interfejs użytkownika odpowiedni `IsLockedOut` właściwość jest przycisku, po kliknięciu odblokowuje konto użytkownika. Kliknięcie tego przycisku powinno być włączone, jeśli użytkownik jest zablokowany.

### <a name="creating-theuserinformationaspxpage"></a>Tworzenie`UserInformation.aspx`strony

Teraz jesteśmy gotowi do zaimplementowania interfejsu użytkownika w `UserInformation.aspx`. Otwórz tę stronę i dodaj następujące kontrolki sieci Web:

- Hiperłącze kontrolkę, która, po kliknięciu, zwraca administratorowi `ManageUsers.aspx` strony.
- Kontrolka etykiety w sieci Web do wyświetlania nazwy wybranego użytkownika. Ustaw tę etykietę `ID` do `UserNameLabel` i wyczyszczenie jego `Text` właściwości.
- Formant pola wyboru o nazwie `IsApproved`. Ustaw jego `AutoPostBack` właściwość `true`.
- Kontrolkę typu etykieta do wyświetlania użytkownikowi przez ostatnie zablokowane daty. Nazwa tej etykiety `LastLockedOutDateLabel` i wyczyszczenie jego `Text` właściwości.
- Przycisk odblokowanie użytkownika. Nazwa tego przycisku `UnlockUserButton` i ustaw jego `Text` właściwość "Odblokować użytkownika".
- Kontrolkę typu etykieta do wyświetlania komunikatów o stanie, takie jak "został zaktualizowany stan zatwierdzone użytkownika". Nadaj nazwę tej kontrolki `StatusMessage`, wyczyść się jego `Text` właściwości, a następnie ustaw jego `CssClass` właściwość `Important`. ( `Important` CSS, klasa jest zdefiniowana w `Styles.css` pliku arkusza stylów; wyświetla odpowiedni tekst czcionką dużych, czerwony.)

Po dodaniu tych kontrolek, widok projektu w programie Visual Studio powinien wyglądać podobnie do ekranu zrzut na rysunku 2.


[![Tworzenie interfejsu użytkownika dla UserInformation.aspx](unlocking-and-approving-user-accounts-cs/_static/image5.png)](unlocking-and-approving-user-accounts-cs/_static/image4.png)

**Rysunek 2**: Tworzenie interfejsu użytkownika dla `UserInformation.aspx` ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](unlocking-and-approving-user-accounts-cs/_static/image6.png))


Pełny interfejs użytkownika naszym kolejnym krokiem jest ustawiony `IsApproved` pola wyboru i inne kontrolki na podstawie wybranego użytkownika informacji. Utwórz procedurę obsługi zdarzeń dla strony `Load` zdarzeń i Dodaj następujący kod:

[!code-csharp[Main](unlocking-and-approving-user-accounts-cs/samples/sample1.cs)]

Powyższy kod uruchamia, zapewniając, że jest to pierwsza wizyta na stronie i nie kolejnych ogłaszania zwrotnego. Następnie odczytuje username przekazywane `user` pole querystring i pobiera informacje dotyczące tego konta użytkownika, za pośrednictwem `Membership.GetUser(username)` metody. Jeśli username nie został dostarczony przez ciąg zapytania lub jeśli nie można odnaleźć określonego użytkownika, administratora jest wysyłane z powrotem do `ManageUsers.aspx` strony.

`MembershipUser` Obiektu `UserName` wartość jest następnie wyświetlana w `UserNameLabel` i `IsApproved` zaznaczono pole wyboru na podstawie `IsApproved` wartości właściwości.

`MembershipUser` Obiektu [ `LastLockoutDate` właściwość](https://msdn.microsoft.com/library/system.web.security.membershipuser.lastlockoutdate.aspx) zwraca `DateTime` wartość wskazująca, kiedy użytkownik był ostatni zablokowane. Jeśli użytkownik nigdy nie zostały zablokowane, wartość zwracana jest zależna od dostawcy członkostwa. Po utworzeniu nowego konta `SqlMembershipProvider` ustawia `aspnet_Membership` tabeli `LastLockoutDate` pole `1754-01-01 12:00:00 AM`. Powyższy kod wyświetla pusty ciąg w `LastLockoutDateLabel` Jeśli `LastLockoutDate` właściwość występuje przed rokiem 2000; w przeciwnym razie daty część `LastLockoutDate` właściwości jest wyświetlany w etykiecie. `UnlockUserButton'` s `Enabled` zostaje ustalona dla użytkownika zablokowany stan, co oznacza, że ten przycisk, będzie można włączyć tylko po użytkownik jest zablokowany.

Poświęć chwilę, aby przetestować `UserInformation.aspx` strony za pośrednictwem przeglądarki. Będzie oczywiście, należy uruchomić na `ManageUsers.aspx` i wybierz konto użytkownika do zarządzania. Po otrzymywanych `UserInformation.aspx`, należy pamiętać, że `IsApproved` tylko po zaznaczeniu pola wyboru, jeśli użytkownik jest zatwierdzony. Jeśli użytkownik nigdy nie zostały zablokowane, ostatnią zablokowane Data jest wyświetlana. Przycisk odblokować użytkownika jest włączona tylko wtedy, gdy użytkownik jest obecnie zablokowane. Zaznaczenie lub usunięcie zaznaczenia `IsApproved` pola wyboru lub przycisku odblokować użytkownika powoduje odświeżenie strony, ale nie zmiany zostaną wprowadzone do konta użytkownika, ponieważ mamy jeszcze do tworzenia programów obsługi zdarzeń dla tych zdarzeń.

Wróć do programu Visual Studio i utworzyć procedury obsługi zdarzeń dla `IsApproved` składnika CheckBox `CheckedChanged` zdarzeń i `UnlockUser` przycisku `Click` zdarzeń. W `CheckedChanged` procedura obsługi zdarzeń, Ustaw użytkownika `IsApproved` właściwości `Checked` właściwości, pola wyboru, a następnie zapisz zmiany przez wywołanie `Membership.UpdateUser`. W `Click` programu obsługi zdarzeń, po prostu wywołanie `MembershipUser` obiektu `UnlockUser` metody. W obu programów obsługi zdarzeń, wyświetlenia odpowiedniego komunikatu w `StatusMessage` etykiety.

[!code-csharp[Main](unlocking-and-approving-user-accounts-cs/samples/sample2.cs)]

### <a name="testing-theuserinformationaspxpage"></a>Testowanie`UserInformation.aspx`strony

Za pomocą tych procedur obsługi zdarzeń w miejscu, należy ponownie stronę i niezatwierdzone przez użytkownika. Jak pokazano na rysunku 3, powinny zostać wyświetlone krótkie wiadomości na stronie, co oznacza, że użytkownik `IsApproved` właściwość została pomyślnie zmodyfikowana.


[![Chris został niezatwierdzone](unlocking-and-approving-user-accounts-cs/_static/image8.png)](unlocking-and-approving-user-accounts-cs/_static/image7.png)

**Rysunek 3**: Chris został niezatwierdzone ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](unlocking-and-approving-user-accounts-cs/_static/image9.png))


Następnie wylogowania i spróbuj zalogować się jako użytkownik, którego konto zostało właśnie niezatwierdzonych. Ponieważ użytkownik nie zostanie zatwierdzone, nie można zalogować. Domyślnie kontrolka Login wyświetla ten sam komunikat, jeśli użytkownik nie może się zalogować, niezależnie od przyczyny. Ale w <a id="Tutorial6"> </a> [ *sprawdzania poprawności użytkownika poświadczeń względem członkostwa użytkownika Store* ](../membership/validating-user-credentials-against-the-membership-user-store-cs.md) samouczek przyjrzeliśmy się udoskonalanie formantu logowania, aby wyświetlić bardziej odpowiedni komunikat. Jak pokazano na rysunku 4, Chris jest wyświetlany komunikat wyjaśniający, że on nie można się zalogować, ponieważ jego konto nie jest jeszcze zatwierdzone.


[![Chris nie ponieważ jego konto logowania jest niezatwierdzone](unlocking-and-approving-user-accounts-cs/_static/image11.png)](unlocking-and-approving-user-accounts-cs/_static/image10.png)

**Rysunek 4**: Chris nie ponieważ jego konto logowania jest niezatwierdzone ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](unlocking-and-approving-user-accounts-cs/_static/image12.png))


Aby przetestować funkcje blokady, spróbuj zalogować się jako użytkownik zatwierdzone, ale użyć niepoprawnego hasła. Powtórz ten proces niezbędne liczbę razy dopóki konto użytkownika zostało zablokowane. Kontrolka Login został także zaktualizowany i będzie zawierał niestandardowego komunikatu, jeśli próba logowania z blokady konta. Wiesz, czy konto zostało zablokowane po uruchomieniu następujący komunikat na stronie logowania wyświetlany: "Twoje konto zostało zablokowane z powodu zbyt wielu nieudanych prób logowania. Skontaktuj się z administratorem, aby Twoje konto zostało odblokowane."

Wróć do `ManageUsers.aspx` strony, a następnie kliknij łącze Zarządzanie blokady użytkownika. Jak pokazano na rysunku 5, powinien zostać wyświetlony z wartością w `LastLockedOutDateLabel` przycisku odblokować użytkownika powinno być włączone. Kliknij przycisk odblokować użytkownika, aby odblokować konto użytkownika. Gdy masz odblokować użytkownika, będą mogli logować się ponownie.


[![Dave zostało zablokowane z systemu](unlocking-and-approving-user-accounts-cs/_static/image14.png)](unlocking-and-approving-user-accounts-cs/_static/image13.png)

**Rysunek 5**: Dave ma zostało zablokowane z systemu ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](unlocking-and-approving-user-accounts-cs/_static/image15.png))


## <a name="step-2-specifying-new-users-approved-status"></a>Krok 2: Określenie nowych użytkowników zatwierdzone stanu

Stan zatwierdzone jest przydatne w scenariuszach, w której chcesz niektóre akcje do wykonania, zanim nowy użytkownik będzie mógł zalogować się i uzyskania dostępu do funkcji specyficzne dla użytkownika w witrynie. Na przykład być może używasz prywatnych witryny internetowej, gdzie wszystkie strony, z wyjątkiem stron logowania i rejestracji, są dostępne tylko dla uwierzytelnionych użytkowników. Ale co się stanie, jeśli obcej osoby osiągnie witryny sieci Web, znajduje się na stronie tworzenia konta i tworzy konto usługi? Aby temu zapobiec, można przenieść strony rejestracji, aby `Administration` folderu i wymagają, że administrator ręcznie tworzyć każdego konta. Można też mogła zadzwonić do rejestracji, ale uniemożliwiają dostęp do witryn, aż administrator zatwierdzi konta użytkownika.

Domyślnie formant CreateUserWizard zatwierdza nowe konta. Można skonfigurować ten problem, używając formantu [ `DisableCreatedUser` właściwość](https://msdn.microsoft.com/en-gb/library/system.web.ui.webcontrols.createuserwizard.disablecreateduser.aspx). Ustaw tę właściwość na `true` nie chcesz zatwierdzać nowych kont użytkowników.

> [!NOTE]
> Domyślnie formant CreateUserWizard loguje się automatycznie nowego konta użytkownika. To zachowanie jest zależna od formantu [ `LoginCreatedUser` właściwość](https://msdn.microsoft.com/en-gb/library/system.web.ui.webcontrols.createuserwizard.logincreateduser.aspx). Ponieważ niezatwierdzonych użytkowników nie można zalogować się do witryny, gdy `DisableCreatedUser` jest `true` nowe konto użytkownika nie jest zalogowany do witryny, niezależnie od wartości `LoginCreatedUser` właściwości.


Jeśli są programowe tworzenie nowych kont użytkowników za pośrednictwem `Membership.CreateUser` metod w celu utworzenia konta użytkownika niezatwierdzonych Użyj jednego z przeciążeń, które akceptują nowego użytkownika `IsApproved` wartości właściwości jako parametr wejściowy.

## <a name="step-3-approving-users-by-verifying-their-email-address"></a>Krok 3: Zatwierdzanie użytkowników, sprawdzając swój adres E-mail

Wiele witryn sieci Web, które obsługują konta użytkowników nie zostanie zatwierdzona nowych użytkowników do momentu ich weryfikacji adresu e-mail, które są dostarczane podczas rejestrowania. Ten proces sprawdzania poprawności jest najczęściej używany do udaremnić botów, spamerów i innych ne'er-do-wells wymaga adresu e-mail z unikatowym, zweryfikowaną, i dodaje dodatkowego kroku w procesie rejestracji. W tym modelu po zarejestrowaniu nowego użytkownika te są wysyłane wiadomości e-mail, która zawiera link do strony weryfikacji. Przechodząc do łącza użytkownika przebiegło czy osoby te otrzymały wiadomości e-mail, i w związku z tym, że podany adres e-mail jest prawidłowy. Na stronie weryfikacji jest odpowiedzialny za zatwierdzanie użytkownika. Ten problem może wystąpić automatycznie, a tym samym zatwierdzanie, użytkownik osiągnie tej strony lub tylko wtedy, gdy użytkownik udostępnia dodatkowe informacje, takie jak [CAPTCHA](http://en.wikipedia.org/wiki/Captcha).

Aby uwzględnić ten przepływ pracy, należy najpierw zaktualizować strony tworzenia konta, dzięki czemu nowe niezatwierdzonych. Otwórz `EnhancedCreateUserWizard.aspx` strony w `Membership` folder i ustaw kontrolkę CreateUserWizard `DisableCreatedUser` właściwość `true`.

Następnie należy skonfigurować kontrolę CreateUserWizard, aby wysłać wiadomość e-mail do nowego użytkownika z instrukcjami zweryfikować swoje konto. W szczególności firma Microsoft będzie zawierać link w wiadomości e-mail do `Verification.aspx` strony (które zostały wykonane następujące kroki jeszcze do tworzenia), przekazując nowego użytkownika `UserId` za pośrednictwem ciąg zapytania. `Verification.aspx` Strony będzie wyszukać określonego użytkownika i oznacz je zatwierdzone.

### <a name="sending-a-verification-email-to-new-users"></a>Wysyłanie weryfikacyjnej wiadomości E-mail do nowych użytkowników

Aby wysłać wiadomość e-mail z formantu CreateUserWizard, skonfiguruj jego `MailDefinition` właściwość odpowiednio. Zgodnie z opisem w <a id="Tutorial13"> </a> [poprzedniego samouczka](recovering-and-changing-passwords-cs.md), obejmują kontrolki ChangePassword i PasswordRecovery [ `MailDefinition` właściwość](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.maildefinition.aspx) działający w taki sam sposób jak CreateUserWizard formantu.

> [!NOTE]
> Aby użyć `MailDefinition` właściwość należy określić dostarczanie poczty opcji na liście `Web.config`. Aby uzyskać więcej informacji, zobacz [wysyłania wiadomości E-mail w programie ASP.NET:](http://aspnet.4guysfromrolla.com/articles/072606-1.aspx).


Rozpocznij od utworzenia nowego szablonu wiadomości e-mail o nazwie `CreateUserWizard.txt` w `EmailTemplates` folderu. Dla szablonu, należy użyć następującego tekstu:

[!code-aspx[Main](unlocking-and-approving-user-accounts-cs/samples/sample3.aspx)]

Ustaw `MailDefinition'` s `BodyFileName` właściwość "~ / EmailTemplates/CreateUserWizard.txt" i jego `Subject` właściwość "Witaj Moja witryna sieci Web! Aktywuj swoje konta."

Należy pamiętać, że `CreateUserWizard.txt` szablon wiadomości e-mail zawiera `<%VerificationUrl%>` symbol zastępczy. To jest, gdy adres URL `Verification.aspx` strony zostaną umieszczone. CreateUserWizard automatycznie zastępuje `<%UserName%>` i `<%Password%>` symbole zastępcze przy użyciu nazwy użytkownika nowego konta i hasła, ale nie ma żadnych wbudowanych `<%VerificationUrl%>` symbol zastępczy. Należy ręcznie zastąpić adres URL odpowiedniej weryfikacji.

W tym celu należy utworzyć procedurę obsługi zdarzeń dla CreateUserWizard [ `SendingMail` zdarzeń](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.sendingmail.aspx) i Dodaj następujący kod:

[!code-csharp[Main](unlocking-and-approving-user-accounts-cs/samples/sample4.cs)]

`SendingMail` Zdarzenia są generowane po `CreatedUser` zdarzeń, co oznacza, że według czasu powyższych programu obsługi zdarzeń uruchamia nowy użytkownik konta został już utworzony. Firma Microsoft mogą uzyskiwać dostęp do nowego użytkownika `UserId` wartość przez wywołanie metody `Membership.GetUser` metody, przekazując `UserName` w formancie CreateUserWizard. Następnie jest sformułowany adres URL weryfikacji. Wykonywanie instrukcji `Request.Url.GetLeftPart(UriPartial.Authority)` zwraca `http://yourserver.com` część adresu URL; `Request.ApplicationPath` zwraca ścieżkę, gdzie aplikacja. Weryfikacja adresu URL jest następnie zdefiniowany jako `Verification.aspx?ID=userId`. Te dwa ciągi są następnie łączone w celu utworzenia pełnego adresu URL. Na koniec treść wiadomości e-mail (`e.Message.Body`) zawiera wszystkie wystąpienia `<%VerificationUrl%>` zastąpione pełny adres URL.

Efektem sieciowym jest nowych użytkowników czy niezatwierdzonych, co oznacza, że nie można zalogować do witryny. Ponadto one są automatycznie wysyłane wiadomości e-mail z linkiem do weryfikacji adresu URL (patrz rysunek 6).


[![Nowy użytkownik otrzymuje wiadomość E-mail z Linkiem do weryfikacji adresu URL](unlocking-and-approving-user-accounts-cs/_static/image17.png)](unlocking-and-approving-user-accounts-cs/_static/image16.png)

**Rysunek 6**: nowy użytkownik otrzymuje wiadomość E-mail z Linkiem do weryfikacji adresu URL ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](unlocking-and-approving-user-accounts-cs/_static/image18.png))


> [!NOTE]
> Krok CreateUserWizard domyślny formant CreateUserWizard zostanie wyświetlony komunikat informujący użytkownika, jego konta został utworzony i wyświetla przycisk Kontynuuj. Kliknięcie powoduje otwarcie na adres URL określony przez funkcję Kontrola `ContinueDestinationPageUrl` właściwości. CreateUserWizard w `EnhancedCreateUserWizard.aspx` jest skonfigurowana do wysyłania nowych użytkowników do `~/Membership/AdditionalUserInfo.aspx`, który monituje użytkownika o ich hometown, adres URL strony głównej i podpis. Ponieważ te informacje mogą być dodawane tylko przy zalogowanych użytkownikach, dobrym pomysłem będzie zaktualizować tę właściwość, aby wysłać do użytkowników powrót do strony głównej witryny (`~/Default.aspx`). Ponadto `EnhancedCreateUserWizard.aspx` strony lub krok CreateUserWizard powinny zostać rozszerzony informować użytkownika, że masz została wysłana weryfikacyjna wiadomość e-mail i ich konta pozostaną nieaktywne do momentu ich postępuj zgodnie z instrukcjami w tej wiadomości e-mail. Czy mogę Pozostaw te modyfikacje w charakterze ćwiczenia czytelnika.


### <a name="creating-the-verification-page"></a>Tworzenie strony weryfikacji

Nasze ostatnim zadaniem jest utworzenie `Verification.aspx` strony. Dodaj tę stronę do folderu głównego, kojarząc ją z `Site.master` strony wzorcowej. Podobnie jak z większością poprzedniej strony zawartości, dodane do witryny, należy usunąć formant zawartości, który odwołuje się do `LoginContent` ContentPlaceHolder tak, aby korzysta z poziomu strony zawartości strony wzorcowej domyślnej zawartości.

Dodaj kontrolkę etykieta w sieci Web do `Verification.aspx` ustaw jego `ID` do `StatusMessage` i wyczyszczenie jego właściwość text. Następnie należy utworzyć `Page_Load` program obsługi zdarzeń i Dodaj następujący kod:

[!code-csharp[Main](unlocking-and-approving-user-accounts-cs/samples/sample5.cs)]

Duża część powyższego kodu sprawdza, czy `UserId` dostarczony przez ciąg zapytania istnieje i że jest prawidłowym `Guid` wartości oraz że odwołuje się do istniejącego konta użytkownika. Jeśli wszystkie te sprawdzenia zakończy się pomyślnie, konto użytkownika zostało zatwierdzone; w przeciwnym razie jest wyświetlany komunikat o stanie odpowiednie.

Rysunek nr 7 przedstawia `Verification.aspx` strony po odwiedzeniu za pośrednictwem przeglądarki.


[![Konto nowego użytkownika są teraz zatwierdzane](unlocking-and-approving-user-accounts-cs/_static/image20.png)](unlocking-and-approving-user-accounts-cs/_static/image19.png)

**Rysunek 7**: nowe konto użytkownika jest teraz zatwierdzane ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](unlocking-and-approving-user-accounts-cs/_static/image21.png))


## <a name="summary"></a>Podsumowanie

Wszystkie konta użytkowników członkostwa mieć dwa stany, które określają, czy użytkownik może zalogować się do witryny: `IsLockedOut` i `IsApproved`. Obie te właściwości musi być `true` dla użytkownika do logowania.

Zablokowane przez użytkownika stanu jest używany ze względów bezpieczeństwa, aby zmniejszyć prawdopodobieństwo haker podzielenie na lokację za pomocą metod atak siłowy. W szczególności użytkownika jest zablokowane w przypadku określonej liczbie nieudanych prób logowania w przedziale czasu. Te granice są konfigurowane za pomocą ustawień dostawcy członkostwa w `Web.config`.

Zatwierdzone stanu jest najczęściej używany jako środek do uniemożliwić logowanie, dopóki nie upłynął czas niektóre akcje, nowi użytkownicy. Być może lokacji wymaga, że nowe konta najpierw zostać zatwierdzone przez administratora lub, jako widzieliśmy w kroku 3, sprawdzając swój adres e-mail.

Wszystkiego najlepszego programowania!

### <a name="about-the-author"></a>Informacje o autorze

Pracował nad Bento Scottem, autor wiele książek ASP/ASP.NET i założyciel 4GuysFromRolla.com, przy użyciu technologii Microsoft Web od 1998 r. Scott działa jako niezależny Konsultant, trainer i składnika zapisywania. Jego najnowszą książkę Stephena  *[Sams uczyć się ASP.NET 2.0 w ciągu 24 godzin](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)*. Scott można z Tobą skontaktować w [ mitchell@4guysfromrolla.com ](mailto:mitchell@4guysfromrolla.com) lub za pośrednictwem jego blog znajduje się na [ http://ScottOnWriting.NET ](http://scottonwriting.net/).

### <a name="special-thanks-to"></a>Specjalne podziękowania dla...

W tej serii samouczków został zrecenzowany przez wielu recenzentów pomocne. Zainteresowani zapoznaniem Moje kolejnych artykułów MSDN? Jeśli tak, Porzuć mnie linii w [mitchell@4GuysFromRolla.com](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Poprzednie](recovering-and-changing-passwords-cs.md)
> [dalej](building-an-interface-to-select-one-user-account-from-many-vb.md)
