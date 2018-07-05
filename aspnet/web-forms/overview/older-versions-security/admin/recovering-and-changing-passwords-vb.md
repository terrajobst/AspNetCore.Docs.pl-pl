---
uid: web-forms/overview/older-versions-security/admin/recovering-and-changing-passwords-vb
title: Odzyskiwanie i zmienianie haseł (VB) | Dokumentacja firmy Microsoft
author: rick-anderson
description: Program ASP.NET zawiera dwie kontrolki sieci Web ułatwiających Odzyskiwanie i zmienianie haseł. Formant PasswordRecovery umożliwia obiekt odwiedzający do odzyskania jego zgubienia pa...
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/01/2008
ms.topic: article
ms.assetid: f9adcb5d-6d70-4885-a3bf-ed95efb4da1a
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/older-versions-security/admin/recovering-and-changing-passwords-vb
msc.type: authoredcontent
ms.openlocfilehash: 68b70708904a46e31639331bbfd0dd31240ea421
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/03/2018
ms.locfileid: "37393999"
---
<a name="recovering-and-changing-passwords-vb"></a>Odzyskiwanie i zmienianie haseł (VB)
====================
przez [Bento Scott](https://twitter.com/ScottOnWriting)

[Pobierz program Code](http://download.microsoft.com/download/6/0/e/60e1bd94-e5f9-4d5a-a079-f23c98f4f67d/VB.13.zip) lub [Pobierz plik PDF](http://download.microsoft.com/download/6/0/e/60e1bd94-e5f9-4d5a-a079-f23c98f4f67d/aspnet_tutorial13_ChangingPasswords_vb.pdf)

> Program ASP.NET zawiera dwie kontrolki sieci Web ułatwiających Odzyskiwanie i zmienianie haseł. Formant PasswordRecovery umożliwia obiekt odwiedzający do odzyskiwania hasła do jego zgubienia. Kontrolka ChangePassword pozwala użytkownikowi na aktualizowanie własnego hasła. Podobnie jak inne formanty Web skojarzone z logowaniem widzieliśmy w całej tej serii samouczków PasswordRecovery i ChangePassword kontroluje pracy przy użyciu framework członkostwa w tle zresetować lub zmodyfikować haseł użytkowników.


## <a name="introduction"></a>Wprowadzenie

Między witrynami sieci Web dla Moje bank, prądu, telefonicznej, konta e-mail i portali sieci web spersonalizowane, takich jak większość osób, masz wielu różnych haseł do zapamiętania. Przy użyciu tak wielu poświadczeń do pamiętania te dni nie jest niczym niezwykłym osobom zapomni swoje hasło. Zostało to, witryn sieci Web, które oferują kont użytkowników musi obejmować sposób na odzyskiwanie hasła użytkownika. Ten proces zwykle obejmuje Generowanie nowy, losowy hasła i wysyłanie wiadomości e-mail na adres e-mail użytkownika w pliku. Po otrzymaniu nowego hasła większość użytkowników, wróć do witryny i zmiany hasła z losowo wygenerowaną łatwiej zapamiętać niż ten, który.

Program ASP.NET zawiera dwie kontrolki sieci Web ułatwiających Odzyskiwanie i zmienianie haseł. Formant PasswordRecovery umożliwia obiekt odwiedzający do odzyskiwania hasła do jego zgubienia. Kontrolka ChangePassword pozwala użytkownikowi na aktualizowanie własnego hasła. Podobnie jak inne formanty Web skojarzone z logowaniem widzieliśmy w całej tej serii samouczków PasswordRecovery i ChangePassword kontroluje pracy przy użyciu framework członkostwa w tle zresetować lub zmodyfikować haseł użytkowników.

W tym samouczku będziemy sprawdzać za pomocą tych dwóch kontrolek. Możemy również pokazano, jak programowo zmienić i zresetować hasło użytkownika za pośrednictwem `MembershipUser` klasy `ChangePassword` i `ResetPassword` metody.

## <a name="step-1-helping-users-recover-lost-passwords"></a>Krok 1: Pomaganie użytkownikom odzyskania hasła

Wszystkie witryny sieci Web, które obsługują konta użytkowników muszą zapewnić użytkownikom mechanizmu odzyskiwania zapomnianych haseł. Dobra wiadomość jest implementacji tych funkcji w programie ASP.NET: szybka i bezproblemowa dzięki kontrolki PasswordRecovery sieci Web. Kontrolka PasswordRecovery renderuje interfejs, który monituje użytkownika o swoją nazwę użytkownika i, jeśli to konieczne, odpowiedź na swoje pytanie zabezpieczające. Go następnie wysłany w wiadomości e-mail użytkownik swoje hasło.

> [!NOTE]
> Ponieważ wiadomości e-mail są przesyłane przez sieć w postaci zwykłego tekstu są zagrożenia bezpieczeństwa związane z wysyłaniem hasło użytkownika za pośrednictwem poczty e-mail.


Kontrola PasswordRecovery składa się z trzech widoków:

- **Nazwa użytkownika** -wyświetla monit o ich nazwy użytkownika. Jest to widok początkowy.
- **Pytanie**-Wyświetla nazwę użytkownika i zabezpieczeń pytanie użytkownika jako tekst, oraz pole tekstowe dla użytkownika o podanie odpowiedzi na swoje pytanie zabezpieczające.
- **Powodzenie**— zostanie wyświetlony komunikat informujący użytkownika, że pocztą e-mail jego hasło.

Widoki wyświetlane i akcje wykonywane przez kontrolkę PasswordRecovery zależą od następujących ustawień konfiguracji członkostwa:

- `RequiresQuestionAndAnswer`
- `EnablePasswordRetrieval`
- `EnablePasswordReset`

Framework członkostwa `RequiresQuestionAndAnswer` ustawienie wskazuje, czy użytkownicy muszą określić pytanie zabezpieczające i odpowiedzi podczas rejestrowania konta. Tak jak Omówiliśmy to w <a id="_msoanchor_1"> </a> [ *tworzenie kont użytkowników* ](../membership/creating-user-accounts-vb.md) samouczków, jeśli `RequiresQuestionAndAnswer` ma wartość True (ustawienie domyślne), a następnie interfejs CreateUserWizard zawiera pole tekstowe Formanty pytanie zabezpieczające i odpowiedzi, nowy użytkownik Jeśli `RequiresQuestionAndAnswer` ma wartość FAŁSZ, są zbierane żadne takie informacje. Podobnie jeśli `RequiresQuestionAndAnswer` jest wartość True, a następnie wyświetla formant PasswordRecovery pytanie wyświetlić po użytkownik wprowadził swoją nazwę użytkownika, ponieważ hasło jest odzyskać tylko wtedy, gdy użytkownik wprowadzi prawidłowy zabezpieczającą. Jeśli `RequiresQuestionAndAnswer` ma wartość FAŁSZ, jednak kontrola PasswordRecovery przechodzi bezpośrednio z widoku nazwy użytkownika do widoku sukces.

Po użytkownik udostępnił jego nazwa użytkownika — lub jego nazwa użytkownika i zabezpieczeń w odpowiedzi, jeśli `RequiresQuestionAndAnswer` to True - PasswordRecovery wysłanie wiadomości e-mail użytkownika i jego hasło. Jeśli `EnablePasswordRetrieval` opcja jest ustawiona na wartość True, a następnie użytkownik jest wysyłany pocztą e-mail ich bieżące hasło. Jeśli jest ustawiona na False i `EnablePasswordReset` jest ustawiona na wartość True, formant PasswordRecovery generuje nowy, losowy hasło dla użytkownika i wiadomości e-mail to nowe hasło do nich. Jeśli oba `EnablePasswordRetrieval` i `EnablePasswordReset` mają wartość FAŁSZ, kontrola PasswordRecovery zgłasza wyjątek.

> [!NOTE]
> Pamiętamy `SqlMembershipProvider` hasła użytkowników są przechowywane w jednej z trzech formatów: wyczyść, Hashed (ustawienie domyślne) lub szyfrowane. Używany mechanizm magazynu zależy od ustawienia konfiguracji członkostwa; aplikacji pokazowej używa formatu hasła Hashed. Korzystając z formatu hasła Hashed `EnablePasswordRetrieval` opcja musi być ustawiona na wartość False, ponieważ system nie może określić rzeczywiste hasło użytkownika z wersji skrótu, przechowywane w bazie danych.


Rysunek 1 przedstawia, jak interfejs i zachowanie PasswordRecovery ma wpływ konfiguracji członkostwa.


[![RequiresQuestionAndAnswer EnablePasswordRetrieval i EnablePasswordReset mają wpływ na wygląd i zachowanie kontrolki PasswordRecovery](recovering-and-changing-passwords-vb/_static/image2.png)](recovering-and-changing-passwords-vb/_static/image1.png)

**Rysunek 1**: `RequiresQuestionAndAnswer`, `EnablePasswordRetrieval`, i `EnablePasswordReset` mają wpływ na wygląd i zachowanie kontrolki PasswordRecovery ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](recovering-and-changing-passwords-vb/_static/image3.png))


> [!NOTE]
> W <a id="_msoanchor_2"> </a> [ *tworzenie schematu członkostwa w programie SQL Server* ](../membership/creating-the-membership-schema-in-sql-server-vb.md) samouczek skonfigurowaliśmy dostawcy członkostwa, ustawiając `RequiresQuestionAndAnswer` na wartość True, `EnablePasswordRetrieval` do Wartość false, a `EnablePasswordReset` na wartość True.


### <a name="using-the-passwordrecovery-control"></a>Używanie formantu PasswordRecovery

Przyjrzyjmy się za pomocą kontroli PasswordRecovery na stronie ASP.NET. Otwórz `RecoverPassword.aspx` i przeciągnij i upuść PasswordRecovery kontrolki z przybornika w Projektancie; ustaw jego `ID` do `RecoverPwd`. Podobnie jak kontrolek logowania i CreateUserWizard w sieci Web widoki kontrolne PasswordRecovery renderowania bogaty interfejs złożony, który zawiera etykiety, pola tekstowe, przyciski i kontrolkami walidacji. Można dostosować wygląd widoków, za pomocą właściwości stylu formantu lub za pomocą konwersji widoków do szablonów. Można pozostawić to w charakterze ćwiczenia zainteresowanych czytnika.

Gdy użytkownik odwiedzi tę stronę ona wprowadź swoją nazwę użytkownika i kliknij przycisk Prześlij. Ponieważ firma Microsoft ma ustawiony `RequiresQuestionAndAnswer` właściwości na wartość True w naszej konfiguracji ustawienia członkostwa PasswordRecovery będzie kontrolować, a następnie Wyświetl widok zapytania. Po użytkownik wprowadza odpowiedź jej poprawna zabezpieczeń i klika przycisk Prześlij, formant PasswordRecovery zaktualizować hasło użytkownika do jeden losowo generowany i wiadomości e-mail tego hasła na adres e-mail podany w pliku. Wszystko to było możliwe bez nam konieczności pisania nawet wiersza kodu!

Przed testowaniem na tej stronie jest jeden ostatni element konfiguracji mają tendencję do: trzeba określić ustawienia dostarczania poczty w `Web.config`. Kontrolki PasswordRecovery opiera się na temat tych ustawień do wysyłania wiadomości e-mail.

Konfiguracja dostarczania poczty jest określony za pomocą [ `<system.net>` elementu](https://msdn.microsoft.com/library/6484zdc1.aspx)firmy [ `<mailSettings>` elementu](https://msdn.microsoft.com/library/w355a94k.aspx). Użyj [ `<smtp>` elementu](https://msdn.microsoft.com/library/ms164240.aspx) oznacza metodę dostarczania i domyślny adres nadawcy. Następujące znaczniki konfiguruje ustawienia poczty do korzystania z serwera SMTP sieci o nazwie `smtp.example.com` na porcie 25 i przy użyciu nazwy użytkownika i hasła poświadczeń użytkownika i hasło.

> [!NOTE]
> `<system.net>` jest elementem podrzędnym elementu głównego `<configuration>` elementu i elementem równorzędnym węzła `<system.web>`. W związku z tym, nie umieszczaj `<system.net>` elemencie `<system.web>` element; zamiast tego należy umieścić na tym samym poziomie.


[!code-xml[Main](recovering-and-changing-passwords-vb/samples/sample1.xml)]

Oprócz korzystania z serwera SMTP w sieci, można alternatywnie określić katalog podnoszenia, w którym złożone wiadomości e-mail do wysłania.

Po skonfigurowaniu ustawień SMTP, odwiedź witrynę `RecoverPassword.aspx` strony za pośrednictwem przeglądarki. Najpierw wprowadź nazwę użytkownika, który nie istnieje w magazynie użytkownika. Jak pokazano na rysunku 2, kontrola PasswordRecovery wyświetla komunikat informujący, że informacje o użytkowniku nie jest dostępny. Tekst komunikatu można dostosować za pomocą formantu [ `UserNameFailureText` właściwość](https://msdn.microsoft.com/library/system.web.ui.webcontrols.passwordrecovery.usernamefailuretext.aspx).


[![Komunikat o błędzie jest wyświetlany, jeśli podano nieprawidłową nazwę użytkownika](recovering-and-changing-passwords-vb/_static/image5.png)](recovering-and-changing-passwords-vb/_static/image4.png)

**Rysunek 2**: komunikat o błędzie jest wyświetlany, jeśli podano nieprawidłową nazwę użytkownika ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](recovering-and-changing-passwords-vb/_static/image6.png))


Teraz wprowadź nazwę użytkownika. Korzystanie z nazwą użytkownika konta w systemie przy użyciu adresu e-mail, że możesz uzyskać dostęp i których zabezpieczeń odpowiedzi, należy znać. Po wprowadzeniu nazwy użytkownika i kliknięcie polecenia przesłania, formant PasswordRecovery wyświetla jej widok zapytania. Jako widok nazwy użytkownika, po wprowadzeniu nieprawidłowe odpowiedzi Wyświetla formant PasswordRecovery komunikatu o błędzie (zobacz rysunek 3). Użyj [ `QuestionFailureText` właściwość](https://msdn.microsoft.com/library/system.web.ui.webcontrols.passwordrecovery.questionfailuretext.aspx) dostosować ten komunikat o błędzie.


[![Komunikat o błędzie jest wyświetlany, jeśli użytkownik wprowadzi nieprawidłowe odpowiedzi zabezpieczeń](recovering-and-changing-passwords-vb/_static/image8.png)](recovering-and-changing-passwords-vb/_static/image7.png)

**Rysunek 3**: komunikat o błędzie jest wyświetlany, jeśli użytkownik wprowadzi nieprawidłowe odpowiedzi zabezpieczeń ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](recovering-and-changing-passwords-vb/_static/image9.png))


Na koniec Wprowadź poprawne zabezpieczającą i kliknij przycisk Prześlij. W tle formantu PasswordRecovery generuje losowe hasło, następnie przypisuje go do konta użytkownika, wysyła wiadomość e-mail z informacją o tym użytkownika nowego hasła (zobacz rysunek 4), a następnie wyświetla widoku powodzenia.


[![Użytkownik otrzymuje wiadomość E-mail z jego nowe hasło](recovering-and-changing-passwords-vb/_static/image11.png)](recovering-and-changing-passwords-vb/_static/image10.png)

**Rysunek 4**: użytkownika jest wysyłana wiadomość E-mail z jego nowe hasło ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](recovering-and-changing-passwords-vb/_static/image12.png))


### <a name="customizing-the-email"></a>Dostosowywanie wiadomości E-mail

Domyślny adres e-mail, wysyłany przez kontrolkę PasswordRecovery jest raczej niekontrastowy obraz (zobacz rysunek 4). Komunikat jest wysyłany z konta określonego w `<smtp>` elementu `from` atrybut z tematem hasła i treść jako zwykły tekst:

Wróć do witryny i zaloguj się przy użyciu następujących informacji.

Nazwa użytkownika: *nazwy użytkownika*

hasło: *hasła*

Ten komunikat można dostosować programowo za pośrednictwem program obsługi zdarzeń dla formantu PasswordRecovery [ `SendingMail` zdarzeń](https://msdn.microsoft.com/library/system.web.ui.webcontrols.passwordrecovery.sendingmail.aspx), lub deklaratywnego za pomocą [ `MailDefinition` właściwość](https://msdn.microsoft.com/library/system.web.ui.webcontrols.passwordrecovery.maildefinition.aspx). Przyjrzyjmy się obu tych opcji.

`SendingMail` Zdarzenie jest wywoływane tuż przed wiadomość e-mail jest wysyłana i jest naszym ostatnia możliwość programowego Dostosuj wiadomości e-mail. Jeśli to zdarzenie jest wywoływane, program obsługi zdarzeń jest przekazywany obiekt typu [ `MailMessageEventArgs` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.mailmessageeventargs.aspx), którego `Message` właściwość zawiera odwołanie do wiadomości e-mail wysłane.

Utwórz procedurę obsługi zdarzeń dla `SendingMail` zdarzeń i Dodaj następujący kod, który programowo dodaje `webmaster@example.com` do listy DW.

[!code-vb[Main](recovering-and-changing-passwords-vb/samples/sample2.vb)]

Wiadomości e-mail można skonfigurować w taki sposób, w sposób deklaratywny. PasswordRecovery `MailDefinition` właściwości jest obiektem typu [ `MailDefinition` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.maildefinition.aspx). `MailDefinition` Klasa oferuje szereg właściwości związanych z pocztą e-mail, w tym `From`, `CC`, `Priority`, `Subject`, `IsBodyHtml`, `BodyFileName`i innym osobom. Po pierwsze, ustaw [ `Subject` właściwość](https://msdn.microsoft.com/library/system.web.ui.webcontrols.maildefinition.subject.aspx) na coś bardziej opisowe niż używany domyślnie (hasło), takie jak Twoje hasło zostało zresetowane...

Aby dostosować treść wiadomości e-mail, które należy utworzyć plik szablonu osobnego adresu e-mail, która zawiera zawartość treści. Najpierw utwórz nowy folder w witrynie sieci Web o nazwie `EmailTemplates`. Następnie dodaj nowy plik tekstowy do tego folderu o nazwie `PasswordRecovery.txt` i dodaj następującą zawartość:

[!code-aspx[Main](recovering-and-changing-passwords-vb/samples/sample3.aspx)]

Zwróć uwagę na użycie symboli zastępczych `<%UserName%>` i `<%Password%>`. Kontrolka PasswordRecovery automatycznie zastępuje te dwa symbole zastępcze z nazwą użytkownika i hasło odzyskane przed wysłaniem wiadomości e-mail przez użytkownika.

Na koniec punktu `MailDefinition`firmy [ `BodyFileName` właściwość](https://msdn.microsoft.com/library/system.web.ui.webcontrols.maildefinition.bodyfilename.aspx) do szablonu wiadomości e-mail, którą właśnie utworzyliśmy (`~/EmailTemplates/PasswordRecovery.txt`).

Po wprowadzisz te zmiany wracać `RecoverPassword.aspx` stronie, a następnie wprowadź nazwę użytkownika i zabezpieczeń odpowiedzi. Otrzymasz wiadomość e-mail, która wygląda podobnie do pokazanego na rysunku 5 powinny. Należy pamiętać, że `webmaster@example.com` została DW będzie i że temat i treść zostały zaktualizowane.


[![Zaktualizowano tematu, treści i DW List](recovering-and-changing-passwords-vb/_static/image14.png)](recovering-and-changing-passwords-vb/_static/image13.png)

**Rysunek 5**: tematu, treści i DW listy zostały zaktualizowane ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](recovering-and-changing-passwords-vb/_static/image15.png))


Aby wysłać wiadomość e-mail w formacie HTML ustaw [ `IsBodyHtml` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.maildefinition.isbodyhtml.aspx) True (wartość domyślna to False), a aktualizacji szablonu wiadomości e-mail, aby uwzględnić HTML.

`MailDefinition` Właściwość nie jest unikatowy dla klasy PasswordRecovery. Jak widać w kroku 2, kontrolka ChangePassword oferuje również `MailDefinition` właściwości. Ponadto, kontrola CreateUserWizard obejmuje takie właściwości można skonfigurować automatyczne wysyłanie wiadomość powitalna wiadomość e-mail do nowych użytkowników.

> [!NOTE]
> Obecnie nie ma żadnych łączy w nawigacji po lewej stronie w celu połączenia się `RecoverPassword.aspx` strony. Użytkownik tylko będą zainteresowani odwiedzenie tej strony, jeśli użytkownik nie mógł pomyślnie zalogować się do witryny. W związku z tym, zaktualizuj `Login.aspx` strony, aby uwzględnić łącze do `RecoverPassword.aspx` strony.


### <a name="programmatically-resetting-a-users-password"></a>Programowe Resetowanie hasła użytkownika

Podczas resetowania hasła użytkownika PasswordRecovery kontrolować wywołania `MembershipUser` obiektu [ `ResetPassword` metoda](https://msdn.microsoft.com/library/system.web.security.membershipuser.resetpassword.aspx). Ta metoda ma dwa przeciążenia:

- **[`ResetPassword`](https://msdn.microsoft.com/library/d94bdzz2.aspx)** -Resetuje hasło użytkownika. Użyj tego przeciążenia, jeśli `RequiresQuestionAndAnswer` ma wartość False.
- **[`ResetPassword(securityAnswer)`](https://msdn.microsoft.com/library/d90zte4w.aspx)** -Resetuje hasło użytkownika tylko wtedy, gdy podane *securityAnswer* jest poprawna. Użyj tego przeciążenia, jeśli `RequiresQuestionAndAnswer` ma wartość True.

Oba przeciążenia zwraca nowy, losowo wygenerowane hasło.

Za pomocą innych metod, w ramach członkostwa `ResetPassword` metody delegatów do skonfigurowanego dostawcy. `SqlMembershipProvider` Wywołuje `aspnet_Membership_ResetPassword` procedurą składowaną, zakończone powodzeniem w nazwy użytkownika, nowe hasło i odpowiedź podane hasło, wśród innych pól. Procedury składowanej zapewnia, że odpowiedź hasła jest zgodny, a następnie aktualizuje hasło użytkownika.

Kilka uwagi dotyczące implementacji niskiego poziomu:

- Blokady użytkownika nie można zresetować swoje hasło. Jednak może być użytkownikiem niezatwierdzonych. Firma Microsoft będzie omawiać zablokowany limit i zatwierdzone stany bardziej szczegółowo w <a id="_msoanchor_3"> </a> [ *Unlocking i zatwierdzanie użytkownika* ](unlocking-and-approving-user-accounts-vb.md) samouczek kont.
- Jeśli odpowiedź hasła jest niepoprawny, jest zwiększana liczba prób odpowiedź hasła użytkownika. Jeśli wystąpią określonej liczby prób odpowiedzi nieprawidłowe zabezpieczenia w określonym przedziale czasu, użytkownik jest zablokowany.

### <a name="a-word-on-how-the-random-passwords-are-generated"></a>Word, w jaki sposób losowych haseł są generowane.

Generowany losowo hasła, wyświetlane w wiadomościach e-mail rysunki 4 i 5 są tworzone przez klasę członkostwa [ `GeneratePassword` metoda](https://msdn.microsoft.com/library/system.web.security.membership.generatepassword.aspx). Ta metoda przyjmuje dwa parametry wejściowe całkowitą - *długość* i *numberOfNonAlphanumericCharacters* — i zwraca ciąg w co najmniej *długość* znaków długości w co najmniej *numberOfNonAlphanumericCharacters* liczba znaków innych niż alfanumeryczne. Gdy ta metoda jest wywoływana z w obrębie klasy członkostwa lub kontrolki skojarzone z logowaniem sieci Web, wartości tych parametrów są określane przez konfigurację członkostwa `MinRequiredPasswordLength` i `MinRequiredNonalphanumericCharacters` właściwości, które możemy ustawić 7 i 1, odpowiednio.

`GeneratePassword` Metoda używa silną kryptograficznie generator liczb losowych, aby upewnić się, że nie jest brak odchylenie jakie losowo wybranych znaków są zaznaczone. Ponadto `GeneratePassword` jest `Public`, co oznacza, której można je bezpośrednio z poziomu aplikacji ASP.NET Jeśli potrzebujesz do wygenerowania losowych ciągów lub hasła.

> [!NOTE]
> `SqlMembershipProvider` Zawsze generuje losowe hasło co najmniej 14 znaków, więc jeśli `MinRequiredPasswordLength` jest mniejsza niż 14, a następnie jego wartość jest ignorowana.


## <a name="step-2-changing-passwords"></a>Krok 2: Zmienianie haseł

Generowany losowo hasła są trudne do zapamiętania. Należy wziąć pod uwagę hasło pokazano na rysunku 4: `WWGUZv(f2yM:Bd`. Wypróbuj, zatwierdzania, który pamięci! Needless, że po użytkownik zostanie wysłany losowo wygenerowane hasło tego rodzaju, ona będziesz chciał Zmień hasło na łatwiejszą do zapamiętania.

Kontrolka ChangePassword umożliwia utworzenie interfejsu użytkownika zmienić swoje hasło. Podobnie jak kontrolka PasswordRecovery kontrolka ChangePassword składa się z dwóch widoków: zmiana hasła i Powodzenie. Wyświetl zmiany hasła monituje użytkownika o ich stare i nowe hasło. Na dostarczenie poprawne hasło stare i nowe hasło, które spełnia minimalnej długości i wymagania dotyczące znaków innych niż alfanumeryczne, kontrolka ChangePassword aktualizacji hasła użytkownika i wyświetla widok sukces.

> [!NOTE]
> Kontrolka ChangePassword modyfikuje hasło użytkownika, wywołując `MembershipUser` obiektu [ `ChangePassword` metoda](https://msdn.microsoft.com/library/system.web.security.membershipuser.changepassword.aspx). Metoda ChangePassword akceptuje dwa `String` parametrów - wejściowych *oldPassword* i *newPassword*— i aktualizuje konto użytkownika z *newPassword*, Zakładając, że podane *oldPassword* jest poprawna.


Otwórz `ChangePassword.aspx` strony, a następnie dodaj kontrolka ChangePassword do strony, nadając mu nazwę `ChangePwd`. W tym momencie Pokaż hasło zmiany w widoku Projekt wyświetlenia (patrz rysunek 6). Podobnie jak za pomocą kontrolki PasswordRecovery można przełączać się między widoków przy użyciu tagu kontrolki. Ponadto wystąpienia tych widoków są dostosowywane do ponownego obliczenia właściwości stylu asortymencie lub konwertowania go do szablonu.


[![Na stronie Dodaj kontrolka ChangePassword](recovering-and-changing-passwords-vb/_static/image17.png)](recovering-and-changing-passwords-vb/_static/image16.png)

**Rysunek 6**: Dodawanie formantu ChangePassword do strony ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](recovering-and-changing-passwords-vb/_static/image18.png))


Kontrolka ChangePassword można zaktualizować hasła aktualnie zalogowanego użytkownika *lub* hasło innego, określonego użytkownika. Jak pokazano na rysunku 6, domyślny widok zmiany haseł powoduje wyświetlenie tylko trzy kontrolki TextBox: jeden dla stare hasło, a dwa nowe hasło. Ten interfejs domyślny jest używany do zaktualizowania hasła aktualnie zalogowanego użytkownika.

Aby zaktualizować hasło innego użytkownika, należy użyć kontrolka ChangePassword, ustaw formantu [ `DisplayUserName` właściwość](https://msdn.microsoft.com/library/system.web.ui.webcontrols.changepassword.displayusername.aspx) na wartość True. Spowoduje to dodanie czwarte pole tekstowe do strony, należy podać nazwa użytkownika, którego hasło, aby zmienić.

Ustawienie `DisplayUserName` na wartość True jest przydatne, jeśli chcesz powiadomić użytkownika zalogowanego się zmienić swoje hasła bez konieczności logowania. Osobiście myślę, że nie ma problem z konieczności użytkownikowi na logowanie przed zezwoleniem na jej zmienić swoje hasło. Dlatego należy pozostawić `DisplayUserName` ustawiony na wartość False (domyślnej). Dokonując tej decyzji, jednak firma Microsoft zasadniczo są blokując użytkowników anonimowych z osiągnięcia tej strony. Aktualizowanie reguł autoryzacji adresów URL lokacji tak, aby odmówić użytkownikom anonimowym z odwiedzając `ChangePassword.aspx`. Jeśli musisz odświeżyć sobie pamięć na składni reguł autoryzacji adresów URL, odwołaj się do <a id="_msoanchor_4"> </a> [ *autoryzacja na podstawie użytkownika* ](../membership/user-based-authorization-vb.md) samouczka.

> [!NOTE]
> Wydaje się, że `DisplayUserName` właściwość jest przydatne w przypadku umożliwiające administratorom zmieniać hasła innych użytkowników. Jednak nawet wtedy, gdy `DisplayUserName` ma wartość True, musi być znane i wprowadzono poprawny stare hasło. Zostaną omówione techniki umożliwiające administratorom konieczności zmiany haseł użytkowników w kroku 3.


Odwiedź stronę `ChangePassword.aspx` stronie za pośrednictwem przeglądarki i Zmień hasło. Należy pamiętać, że jest wyświetlany komunikat o błędzie, jeśli wprowadzisz nowe hasło, które nie spełniają długość hasła i wymagania dotyczące znaków innych niż alfanumeryczne, określona w konfiguracji członkostwa (zobacz rysunek 7).


[![Na stronie Dodaj kontrolka ChangePassword](recovering-and-changing-passwords-vb/_static/image20.png)](recovering-and-changing-passwords-vb/_static/image19.png)

**Rysunek 7**: Dodawanie formantu ChangePassword do strony ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](recovering-and-changing-passwords-vb/_static/image21.png))


Po wprowadzeniu poprawne stare hasło i prawidłowe hasło nowego zalogowanego na użytkownika zostanie zmienione hasło, a następnie wyświetlany widok sukces.

### <a name="sending-a-confirmation-email"></a>Wysyłanie wiadomości E-mail z potwierdzeniem

Domyślnie kontrolka ChangePassword nie wysyła wiadomość e-mail do użytkownika, którego hasło zostało właśnie został zaktualizowany. Jeśli chcesz wysłać wiadomość e-mail, po prostu skonfigurować formantu `MailDefinition` właściwości. Skonfigurujmy kontrolka ChangePassword, tak aby użytkownik otrzymuje wiadomość e-mail formacie HTML, która zawiera nowego hasła.

Rozpocznij od utworzenia nowego pliku w `EmailTemplates` folder o nazwie `ChangePassword.htm`. Dodaj następujący kod:

[!code-html[Main](recovering-and-changing-passwords-vb/samples/sample4.html)]

Następnym etapem jest skonfigurowanie kontrolka ChangePassword `MailDefinition` właściwości `BodyFileName`, `IsBodyHtml`, i `Subject` właściwości ~ / EmailTemplates/ChangePassword.htm, PRAWDA, a Twoje hasło zmieniono! odpowiednio.

Po wprowadzeniu tych zmian, ponownie stronę i Zmień hasło ponownie. Tym razem kontrolka ChangePassword wysyła wiadomość e-mail niestandardowego, w formacie HTML na adres e-mail użytkownika w pliku (zobacz rysunek 8).


[![Wiadomość E-mail informuje że ich hasło użytkownika został zmieniony.](recovering-and-changing-passwords-vb/_static/image23.png)](recovering-and-changing-passwords-vb/_static/image22.png)

**Rysunek 8**: wiadomości E-mail wyświetlony komunikat z informacją, ich hasło użytkownika został zmieniony ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](recovering-and-changing-passwords-vb/_static/image24.png))


## <a name="step-3-allowing-administrators-to-change-users-passwords"></a>Krok 3: Umożliwiające administratorom konieczności zmiany haseł użytkowników

Typową funkcją w aplikacjach, które obsługują konta użytkowników polega na tym użytkownikiem administracyjnym, można zmienić hasła innych użytkowników. Czasami ta funkcja jest wymagana, ponieważ system nie umożliwia użytkownikom zmiany swoich haseł. W takim przypadku jedynym sposobem, aby użytkownik mógł odzyskać zapomniane hasła będzie dla administratora przypisać je nowe hasło. Za pomocą kontrolek PasswordRecovery i ChangePassword jednak użytkownicy administracyjni muszą nie zajęty samodzielnie zmienianie haseł użytkowników, jak użytkownicy są w stanie wykonać to samodzielnie.

Ale co zrobić, jeśli Twój klient sobie, że użytkownicy administracyjni powinien móc zmiany hasła innym użytkownikom? Niestety dodanie tej funkcji może być nieco pracy. Aby zmienić hasło użytkownika, należy podać stare i nowe hasło do `MembershipUser` obiektu `ChangePassword` metody, ale administrator nie powinien trzeba znać hasło użytkownika, aby go zmodyfikować.

Jednym z rozwiązań jest najpierw zresetować hasło użytkownika, a następnie zmień go na nowe hasło przy użyciu kodu, jak pokazano poniżej:

[!code-vb[Main](recovering-and-changing-passwords-vb/samples/sample5.vb)]

Ten kod uruchamia przez pobieranie informacji o *username*, czyli użytkownika, którego hasło administrator chce zmienić. Następnie `ResetPassword` wywoływana jest metoda, która przypisuje i użytkownika, hasło nowy, losowy. To losowo wygenerowane hasło jest zwracany przez metodę i przechowywane w zmiennej `resetPwd`. Teraz, gdy wiemy hasło użytkownika, możemy zmienić ją za pośrednictwem wywołania `ChangePassword`.

Problem polega na to, że ten kod działa tylko, jeśli konfiguracja systemu członkostwa jest ustawiona tak, aby `RequiresQuestionAndAnswer` ma wartość False. Jeśli `RequiresQuestionAndAnswer` ma wartość PRAWDA, jak w przypadku naszej aplikacji, a następnie `ResetPassword` metoda musi zostać przekazane z odpowiedzią zabezpieczającą, w przeciwnym razie spowoduje zgłoszenie wyjątku.

Jeśli w ramach członkostwa jest skonfigurowany do żądania pytanie zabezpieczające i odpowiedzi oraz jeszcze klienta sobie, że administratorzy mogli zmieniać hasła użytkowników są trzy opcje:

- Throw opracowanymi w powietrzu i poinformuj klienta, to po prostu jedną z rzeczy, nie można wykonać.
- Ustaw `RequiresQuestionAndAnswer` o wartości False. Skutkuje to mniej bezpieczna opcja aplikacji. Wyobraź sobie, że szkodliwa użytkownika uzyskało dostęp do skrzynki odbiorczej poczty e-mail przez innego użytkownika. Być może użytkownik, którego bezpieczeństwo zostało naruszone opuścił własnym biurku, aby przejść do coś zjeść i nie zablokować stacji roboczej lub może uzyskać dostępu do poczty e-mail, z poziomu terminalu publicznego i nie wylogują się. W obu przypadkach szkodliwa użytkownik może odwiedzić `RecoverPassword.aspx` stronie, a następnie wprowadź nazwy użytkownika. System będzie poczty e-mail odzyskane hasła bez monitowania o odpowiedź na pytanie zabezpieczeń.
- Obejście warstwy abstrakcji, utworzonych przez członkostwo framework oraz pracy bezpośrednio z bazą danych programu SQL Server. Schemat członkostwo obejmuje procedury składowanej o nazwie `aspnet_Membership_SetPassword` , ustawia hasło użytkownika i nie wymaga zabezpieczającą lub starego hasła w celu wykonania swojego zadania.

Żadna z tych opcji są szczególnie atrakcyjne, ale to, jak czasem życia dewelopera przechodzi.

Błąd z wyprzedzeniem i zaimplementować podejście trzeci, pisanie kodu, który pomija `Membership` i `MembershipUser` klasy i działa bezpośrednio w odniesieniu `SecurityTutorials` bazy danych.

> [!NOTE]
> Pracując bezpośrednio z bazą danych jest shattered hermetyzacji dostarczanych przez szablon członkostwa. Ta decyzja więzi nam `SqlMembershipProvider`, dzięki czemu naszego kodu mniej przenośna. Ponadto ten kod może nie działać zgodnie z oczekiwaniami w przyszłych wersjach programu ASP.NET, jeśli zmiany schematu członkostwa. To podejście jest obejście tego problemu i jak większość obejścia problemu, nie jest przykładem najlepszych rozwiązań.


Kod zawiera kilka bitów nieatrakcyjnych i jest bardzo długi. W związku z tym nie chcę tego samouczka przy użyciu szczegółowe badanie zbliżyć do siebie te. Jeśli chcesz dowiedzieć się więcej, Pobierz kod dla tego samouczka i odwiedź stronę `~/Administration/ManageUsers.aspx` strony. Tej strony, które utworzyliśmy w <a id="_msoanchor_5"> </a> [poprzedni Samouczek](building-an-interface-to-select-one-user-account-from-many-vb.md), zawiera listę poszczególnych użytkowników. Po aktualizacji GridView, aby uwzględnić łącze do `UserInformation.aspx` strony, przekazując wybranego użytkownika za pośrednictwem ciąg zapytania. `UserInformation.aspx` Strony wyświetla informacje dotyczące wybranego użytkownika i pola tekstowe do zmiany hasła (patrz rysunek 9).

Po wprowadzeniu nowego hasła, potwierdzenie go w drugim polu tekstowym i kliknięcie przycisku użytkownika aktualizacji, ensues odświeżenie strony i `aspnet_Membership_SetPassword` zostanie wywołana procedura składowana, aktualizowanie hasła użytkownika. Zachęcam tych czytelnikom interesuje ta funkcjonalność dokładniej zapoznać się z kodem, a następnie spróbuj rozszerzania funkcji obejmujący wysłanie wiadomości e-mail do użytkownika, którego hasło zostało zmienione.


[![Administrator może zmienić hasła użytkownika](recovering-and-changing-passwords-vb/_static/image26.png)](recovering-and-changing-passwords-vb/_static/image25.png)

**Rysunek 9**: Administrator może zmienić hasło użytkownika ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](recovering-and-changing-passwords-vb/_static/image27.png))


> [!NOTE]
> `UserInformation.aspx` Strony obecnie działa tylko, jeśli w ramach członkostwa jest skonfigurowany do przechowywania haseł w formacie zwykłego lub Hashed. Mimo że zaproszono Cię do dodać tę funkcjonalność, brakuje kod, aby zaszyfrować nowe hasło. Sposób zalecam, dodając kod niezbędne jest użycie decompiler, takich jak [odblaskowego](http://www.aisto.com/roeder/dotnet/) zbadanie kodu źródłowego dla metod .NET Framework; start, sprawdzając `SqlMembershipProvider` klasy `ChangePassword` metody. Jest to technika, używane do pisania kodu do tworzenia skrótów haseł.


## <a name="summary"></a>Podsumowanie

Program ASP.NET oferuje dwie kontrolki, aby ułatwić użytkownikom zarządzanie swoje hasło. Kontrolka PasswordRecovery jest przydatne dla osób, który zapomniał hasła. W zależności od konfiguracji w ramach członkostwa użytkownika jest albo pocztą e-mail powiadomienia o ich istniejące hasło lub nowy, losowo wygenerowane hasło. Kontrolka ChangePassword umożliwia użytkownikowi zaktualizowanie hasła.

Tak, jak nazwa logowania i CreateUserWizard formanty kontrolki PasswordRecovery i ChangePassword renderowane w zaawansowanym interfejsie użytkownika bez konieczności pisania lizawki oznaczeniu deklaracyjnym lub wiersza kodu. Jeśli domyślny interfejs użytkownika nie odpowiada Twoim potrzebom, możesz dostosować ją przy użyciu różnych właściwości stylu. Alternatywnie kontrolek interfejsy mogą być konwertowane do szablonów, aby uzyskać jeszcze lepszą kontrolę. Za kulisami tych kontrolek za pomocą interfejsu API członkostwa, wywołując `MembershipUser` obiektu `ResetPassword` i `ChangePassword` metody.

Wszystkiego najlepszego programowania!

### <a name="further-reading"></a>Dalsze informacje

Więcej informacji na tematów omówionych w tym samouczku można znaleźć w następujących zasobach:

- [Kontrolka ChangePassword przewodników Szybki Start](https://quickstarts.asp.net/QuickStartv20/aspnet/doc/ctrlref/login/changepassword.aspx)
- [Kontrolka PasswordRecovery przewodników Szybki Start](https://quickstarts.asp.net/QuickStartv20/aspnet/doc/ctrlref/login/passwordrecovery.aspx)
- [Wysyłanie wiadomości E-mail w programie ASP.NET:](http://aspnet.4guysfromrolla.com/articles/072606-1.aspx)
- [`System.Net.Mail` Często zadawane pytania](http://www.systemnetmail.com/)

### <a name="about-the-author"></a>Informacje o autorze

Pracował nad Bento Scottem, autor wiele książek ASP/ASP.NET i założyciel 4GuysFromRolla.com, przy użyciu technologii Microsoft Web od 1998 r. Scott działa jako niezależny Konsultant, trainer i składnika zapisywania. Jego najnowszą książkę Stephena  *[Sams uczyć się ASP.NET 2.0 w ciągu 24 godzin](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)*. Scott można z Tobą skontaktować w [ mitchell@4guysfromrolla.com ](mailto:mitchell@4guysfromrolla.com) lub za pośrednictwem jego blog znajduje się na [ http://ScottOnWriting.NET ](http://scottonwriting.net/).

### <a name="special-thanks-to"></a>Specjalne podziękowania dla

W tej serii samouczków został zrecenzowany przez wielu recenzentów pomocne. Wiodące osób dokonujących przeglądu, w tym samouczku obejmują Michael Emmings i Suchi Banerjee. Zainteresowani zapoznaniem Moje kolejnych artykułów MSDN? Jeśli tak, Porzuć mnie linii w [mitchell@4GuysFromRolla.com](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Poprzednie](building-an-interface-to-select-one-user-account-from-many-vb.md)
> [dalej](unlocking-and-approving-user-accounts-vb.md)
