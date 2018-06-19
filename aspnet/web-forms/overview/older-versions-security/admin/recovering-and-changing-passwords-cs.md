---
uid: web-forms/overview/older-versions-security/admin/recovering-and-changing-passwords-cs
title: Odzyskiwanie i zmiana haseł (C#) | Dokumentacja firmy Microsoft
author: rick-anderson
description: Program ASP.NET zawiera dwa formanty sieci Web z odzyskiwanie i zmiana haseł. Formant PasswordRecovery umożliwia odwiedzającego odzyskać jego utracone pa...
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/01/2008
ms.topic: article
ms.assetid: 19c4d042-4e34-4b44-9f1d-6bf2253ba366
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-security/admin/recovering-and-changing-passwords-cs
msc.type: authoredcontent
ms.openlocfilehash: 7f8b019631eff4840bf1759f8e2752946abcaf80
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/06/2018
ms.locfileid: "30891006"
---
<a name="recovering-and-changing-passwords-c"></a>Odzyskiwanie i zmiana haseł (C#)
====================
przez [Bento Scott](https://twitter.com/ScottOnWriting)

[Pobierz kod](http://download.microsoft.com/download/6/0/e/60e1bd94-e5f9-4d5a-a079-f23c98f4f67d/CS.13.zip) lub [pobierania plików PDF](http://download.microsoft.com/download/6/0/e/60e1bd94-e5f9-4d5a-a079-f23c98f4f67d/aspnet_tutorial13_ChangingPasswords_cs.pdf)

> Program ASP.NET zawiera dwa formanty sieci Web z odzyskiwanie i zmiana haseł. Formant PasswordRecovery umożliwia odwiedzającego odzyskać jego utraconego hasła. Element ChangePassword kontroli umożliwia użytkownikowi zaktualizuj jego hasło. Podobnie jak inne formanty związane z logowaniem Web możemy przedstawiono w tej serii samouczek PasswordRecovery i Element ChangePassword kontroluje współpracują z framework członkostwa w tle do resetowania lub modyfikowania haseł użytkowników.


## <a name="introduction"></a>Wprowadzenie

Między witrynami sieci Web Moje bank, narzędzie firmy, telefonicznej, konta e-mail i portali sieci web spersonalizowane, większość użytkowników mam dziesiątki różnych haseł do zapamiętania. Z tylu poświadczenia, aby te dni pamiętania nie jest często użytkownicy zapomni swoje hasło. Aby to uwzględniać, witryn sieci Web, które oferują kont użytkowników musi obejmują sposób na odzyskiwanie hasła użytkownika. Ten proces zwykle obejmuje Generowanie nowy, losowy hasła i wysyłać wiadomości e-mail na adres e-mail użytkownika w pliku. Po otrzymaniu nowego hasła większość użytkowników wróć do witryny i zmiany hasła z losowo generowany jeden do jednego łatwiej zapamiętać.

Program ASP.NET zawiera dwa formanty sieci Web z odzyskiwanie i zmiana haseł. Formant PasswordRecovery umożliwia odwiedzającego odzyskać jego utraconego hasła. Element ChangePassword kontroli umożliwia użytkownikowi zaktualizuj jego hasło. Podobnie jak inne formanty związane z logowaniem Web możemy przedstawiono w tej serii samouczek PasswordRecovery i Element ChangePassword kontroluje współpracują z framework członkostwa w tle do resetowania lub modyfikowania haseł użytkowników.

W tym samouczku omówione zostanie za pomocą tych dwóch formantów. Przedstawiono również sposób programowo zmienić i zresetować hasło użytkownika za pośrednictwem `MembershipUser` klasy `ChangePassword` i `ResetPassword` metody.

## <a name="step-1-helping-users-recover-lost-passwords"></a>Krok 1: Pomaganie użytkownikom odzyskania hasła

Wszystkie witryny sieci Web, które obsługują kont użytkowników należy zapewnić użytkownikom mechanizmu zapomniane hasła odzyskiwania. Dobre wieści jest wdrażanie tych funkcji w programie ASP.NET błyskawicznie dzięki użyciu formantu PasswordRecovery sieci Web. Formant PasswordRecovery renderuje interfejsu, które monituje użytkownika o swoją nazwę użytkownika i, jeśli to konieczne, odpowiedź na swoje pytanie zabezpieczające. Wiadomości następnie użytkownik swoje hasło.

> [!NOTE]
> Ponieważ wiadomości e-mail są przesyłane przez sieć jako zwykły tekst istnieją zagrożenia bezpieczeństwa związane z wysyłaniem hasło użytkownika za pośrednictwem poczty e-mail.


Kontrola PasswordRecovery składa się z trzech widoków:

- **Nazwa użytkownika** -wyświetla monit o swoją nazwę użytkownika. Jest to widok początkowy.
- **Pytanie**-wyświetla pytanie nazwy użytkownika i zabezpieczeń użytkownika w postaci tekstu, wraz z pola tekstowego do wpisz odpowiedź na swoje pytanie zabezpieczeń użytkownika.
- **Powodzenie**-wyświetla komunikat informujący użytkownika, że pocztą e-mail hasła.

Widoki wyświetlane i akcje wykonywane przez formant PasswordRecovery zależą od następujących ustawień konfiguracji członkostwa:

- `RequiresQuestionAndAnswer`
- `EnablePasswordRetrieval`
- `EnablePasswordReset`

Framework członkostwa `RequiresQuestionAndAnswer` ustawienie wskazuje, czy użytkownicy muszą określić pytanie i odpowiedź zabezpieczeń podczas rejestrowania dla konta. Jak wspomniano w <a id="_msoanchor_1"> </a> [ *tworzenia kont użytkowników* ](../membership/creating-user-accounts-cs.md) samouczek, jeśli `RequiresQuestionAndAnswer` ma wartość True (ustawienie domyślne), a następnie CreateUserWizard interfejs zawiera pole tekstowe Formanty dla nowego użytkownika pytanie i odpowiedź zabezpieczeń; Jeśli `RequiresQuestionAndAnswer` ma wartość False, żadne takie informacje są zbierane. Podobnie jeśli `RequiresQuestionAndAnswer` jest wartość PRAWDA, a następnie wyświetla formant PasswordRecovery pytanie wyświetlić po użytkownik wprowadził swoją nazwę użytkownika, hasło jest odzyskać tylko wtedy, gdy użytkownik wprowadza odpowiedzi poprawnych zabezpieczeń. Jeśli `RequiresQuestionAndAnswer` ma wartość False, jednak formant PasswordRecovery przemieszcza się bezpośrednio z widoku nazwy użytkownika do widoku powodzenia.

Po użytkownik udostępnił jego nazwa użytkownika - lub jego nazwa użytkownika i zabezpieczeń odpowiedzi, jeśli `RequiresQuestionAndAnswer` ma wartość True - PasswordRecovery wysłanie wiadomości e-mail użytkownika i hasła. Jeśli `EnablePasswordRetrieval` opcja jest ustawiona na wartość True, a następnie użytkownik wysyłany pocztą e-mail ich bieżące hasło. Jeśli jest ustawiona na False i `EnablePasswordReset` ma wartość True, następnie kontroli PasswordRecovery generuje nowy, losowy hasło dla użytkownika i wiadomości e-mail to nowe hasło, aby je. Jeśli oba `EnablePasswordRetrieval` i `EnablePasswordReset` mają wartość FAŁSZ, kontroli PasswordRecovery zgłasza wyjątek.

> [!NOTE]
> Odwołania, który `SqlMembershipProvider` przechowuje hasło użytkownika w jednym z trzech formatów: wyczyść, Hashed (ustawienie domyślne) lub szyfrowane. Używany mechanizm magazynu zależy od ustawień konfiguracji członkostwa; Wersja demonstracyjna aplikację Hashed format hasła. Korzystając z formatu hasła Hashed `EnablePasswordRetrieval` opcja musi mieć wartość false, ponieważ system nie można określić rzeczywistego hasła z mieszaną wersję w bazie danych.


Rysunek 1 pokazuje, jak interfejs i zachowanie PasswordRecovery ma wpływ konfiguracji członkostwa.


[![RequiresQuestionAndAnswer, EnablePasswordRetrieval i EnablePasswordReset wpływ wygląd i zachowanie kontroli PasswordRecovery](recovering-and-changing-passwords-cs/_static/image2.png)](recovering-and-changing-passwords-cs/_static/image1.png)

**Rysunek 1**: `RequiresQuestionAndAnswer`, `EnablePasswordRetrieval`, i `EnablePasswordReset` wpływ wygląd i zachowanie kontroli PasswordRecovery ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](recovering-and-changing-passwords-cs/_static/image3.png))


> [!NOTE]
> W <a id="_msoanchor_2"> </a> [ *tworzenie schematu członkostwa w programie SQL Server* ](../membership/creating-the-membership-schema-in-sql-server-cs.md) samouczka został skonfigurowany dostawca członkostwa przez ustawienie `RequiresQuestionAndAnswer` na wartość True, `EnablePasswordRetrieval` do Wartość false, a `EnablePasswordReset` na wartość True.


### <a name="using-the-passwordrecovery-control"></a>Używanie formantu PasswordRecovery

Przyjrzyjmy się przy użyciu formantu PasswordRecovery strony ASP.NET. Otwórz `RecoverPassword.aspx` i przeciągnij i upuść kontrolkę PasswordRecovery z przybornika do projektanta; ustaw jego `ID` do `RecoverPwd`. Podobnie jak kontrolek logowania i CreateUserWizard w sieci Web widoki kontrolne PasswordRecovery renderowania sformatowanego interfejsu złożony, który zawiera etykiet pól tekstowych, przycisków i formanty walidacji. Można dostosować wygląd widoków za pomocą właściwości stylu formantu lub konwertując widoków do szablonów. I pozostaw to wykonywania zainteresowanych czytnika.

Gdy użytkownik odwiedzi tę stronę ona wprowadź jego nazwę użytkownika i kliknij przycisk Zatwierdź. Ponieważ firma Microsoft ustawiono `RequiresQuestionAndAnswer` właściwości na wartość True w ustawieniach konfiguracji naszych członkostwa, PasswordRecovery będzie kontrolować, a następnie Wyświetl widok zapytania. Po użytkownik wprowadza swoje odpowiedzi poprawnych zabezpieczeń i klika przycisk Prześlij, kontroli PasswordRecovery aktualizacji hasła do jeden losowo wygenerowany i wiadomości e-mail tego hasła na adres e-mail w pliku. Wszystko to było możliwe bez nam konieczności pisania pojedynczy wiersz kodu!

Aby przetestować tę stronę, jest jeden ostatni element konfiguracji do często: należy określić ustawienia dostarczania poczty w `Web.config`. Kontroli PasswordRecovery korzysta z tych ustawień do wysyłania wiadomości e-mail.

Konfiguracja dostarczania poczty jest określany za pośrednictwem [ `<system.net>` elementu](https://msdn.microsoft.com/library/6484zdc1.aspx)w [ `<mailSettings>` elementu](https://msdn.microsoft.com/library/w355a94k.aspx). Użyj [ `<smtp>` elementu](https://msdn.microsoft.com/library/ms164240.aspx) oznacza metodę dostarczania i domyślny adres nadawcy. Następujący kod konfiguruje ustawienia poczty do korzystania z serwera SMTP sieci o nazwie `smtp.example.com` na porcie 25 i poświadczenia nazwy użytkownika i hasła użytkownika i hasło.

> [!NOTE]
> `<system.net>` jest elementem podrzędnym głównego `<configuration>` elementu i elementem równorzędnym `<system.web>`. W związku z tym nie należy umieszczać `<system.net>` w elemencie `<system.web>` element; zamiast tego należy umieścić na tym samym poziomie.


[!code-xml[Main](recovering-and-changing-passwords-cs/samples/sample1.xml)]

Oprócz przy użyciu serwera SMTP w sieci, można alternatywnie określić katalog podnoszenia, w których powinny być nadawane wiadomości e-mail do wysłania.

Po skonfigurowaniu ustawienia SMTP, odwiedź stronę `RecoverPassword.aspx` strony za pośrednictwem przeglądarki. Najpierw spróbuj wprowadzić nazwę użytkownika, który nie istnieje w magazynie użytkownika. Jak pokazano na rysunku 2, kontroli PasswordRecovery wyświetla komunikat informujący, że informacje o użytkowniku nie jest dostępny. Tekst komunikatu można dostosować za pomocą formantu [ `UserNameFailureText` właściwości](https://msdn.microsoft.com/library/system.web.ui.webcontrols.passwordrecovery.usernamefailuretext.aspx).


[![Komunikat o błędzie jest wyświetlany, jeśli podano nieprawidłową nazwę użytkownika](recovering-and-changing-passwords-cs/_static/image5.png)](recovering-and-changing-passwords-cs/_static/image4.png)

**Rysunek 2**: komunikat o błędzie jest wyświetlany, jeśli podano nieprawidłową nazwę użytkownika ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](recovering-and-changing-passwords-cs/_static/image6.png))


Teraz należy wprowadzić nazwę użytkownika. Użyj nazwy użytkownika konta w systemie przy użyciu adresu e-mail można uzyskać dostęp, a których zabezpieczeń odpowiedzi, należy znać. Po wprowadzeniu nazwy użytkownika, a następnie klikając przycisk Prześlij, kontrola PasswordRecovery wyświetla jego widoku pytania. Jako z widoku nazwy użytkownika, po wprowadzeniu nieprawidłowych odpowiedzi Wyświetla formant PasswordRecovery komunikatu o błędzie (patrz rysunek 3). Użyj [ `QuestionFailureText` właściwości](https://msdn.microsoft.com/library/system.web.ui.webcontrols.passwordrecovery.questionfailuretext.aspx) Aby dostosować ten komunikat o błędzie.


[![Jeśli użytkownik wprowadzi nieprawidłowe odpowiedź zabezpieczeń jest wyświetlany komunikat o błędzie](recovering-and-changing-passwords-cs/_static/image8.png)](recovering-and-changing-passwords-cs/_static/image7.png)

**Rysunek 3**: komunikat o błędzie jest wyświetlany, jeśli użytkownik wprowadzi nieprawidłowe odpowiedź zabezpieczeń ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](recovering-and-changing-passwords-cs/_static/image9.png))


Na koniec Wprowadź poprawne zabezpieczeń i kliknij przycisk Prześlij. W tle formantu PasswordRecovery generuje losowe hasło, przypisuje go do konta użytkownika, wysyła wiadomość e-mail informujące użytkownika swoje nowe hasło (patrz rysunek 4), a następnie wyświetla widoku powodzenia.


[![Użytkownik jest wysyłana wiadomość E-mail z jego nowego hasła](recovering-and-changing-passwords-cs/_static/image11.png)](recovering-and-changing-passwords-cs/_static/image10.png)

**Rysunek 4**: użytkownika jest wysyłana wiadomość E-mail z jego nowego hasła ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](recovering-and-changing-passwords-cs/_static/image12.png))


### <a name="customizing-the-email"></a>Dostosowywanie wiadomości E-mail

Domyślne wiadomości e-mail wysyłane przez formant PasswordRecovery jest raczej niekontrastowy obraz (patrz rysunek 4). Komunikat jest wysyłany z konta określonego w `<smtp>` elementu `from` atrybut z podmiotem hasła i treści zwykłego tekstu:

Wróć do witryny i zaloguj się za pomocą poniższych informacji.

Nazwa użytkownika: *nazwy użytkownika*

Hasło: *hasła*

Ten komunikat można dostosować programowo przez program obsługi zdarzeń dla formantu PasswordRecovery [ `SendingMail` zdarzeń](https://msdn.microsoft.com/library/system.web.ui.webcontrols.passwordrecovery.sendingmail.aspx), lub deklaratywnie za pomocą [ `MailDefinition` właściwości](https://msdn.microsoft.com/library/system.web.ui.webcontrols.passwordrecovery.maildefinition.aspx). Przyjrzyjmy się obu tych opcji.

`SendingMail` Zdarzenie jest generowane tuż przed wiadomości e-mail są wysyłane i jest naszych ostatnia możliwość programowo dostosować wiadomości e-mail. Jeśli to zdarzenie jest wywoływane, program obsługi zdarzeń jest przekazywany obiekt typu [ `MailMessageEventArgs` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.mailmessageeventargs.aspx), których `Message` właściwość zawiera odwołanie do wiadomości e-mail wysłane.

Tworzenie procedury obsługi zdarzeń dla `SendingMail` zdarzeń i Dodaj następujący kod, który dodaje programowo `webmaster@example.com` do listy DW.

[!code-csharp[Main](recovering-and-changing-passwords-cs/samples/sample2.cs)]

Wiadomości e-mail można również skonfigurować za pośrednictwem deklaratywne środków. PasswordRecovery `MailDefinition` właściwość jest typu obiektu [ `MailDefinition` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.maildefinition.aspx). `MailDefinition` Klasa zapewnia szereg właściwości związanych z pocztą e-mail, w tym `From`, `CC`, `Priority`, `Subject`, `IsBodyHtml`, `BodyFileName`i inne. Po pierwsze, ustaw [ `Subject` właściwości](https://msdn.microsoft.com/library/system.web.ui.webcontrols.maildefinition.subject.aspx) inny opisową więcej niż jeden używany domyślnie (hasło), takie jak Twoje hasło zostało zresetowane...

Aby dostosować treść wiadomości e-mail, którą należy utworzyć plik szablonu osobne wiadomości e-mail, który zawiera zawartość treści. Rozpocznij od utworzenia nowy folder w witrynie sieci Web o nazwie `EmailTemplates`. Następnie dodaj nowy plik tekstowy do tego folderu o nazwie `PasswordRecovery.txt` i dodaj następującą zawartość:

[!code-aspx[Main](recovering-and-changing-passwords-cs/samples/sample3.aspx)]

Zwróć uwagę na użycie symboli zastępczych `<%UserName%>` i `<%Password%>`. Formant PasswordRecovery zastępuje automatycznie tych dwóch symbole zastępcze nazwy użytkownika i hasła odzyskane przed wysłaniem wiadomości e-mail użytkownika.

Ponadto punkt `MailDefinition`w [ `BodyFileName` właściwości](https://msdn.microsoft.com/library/system.web.ui.webcontrols.maildefinition.bodyfilename.aspx) do właśnie utworzony szablon wiadomości e-mail (`~/EmailTemplates/PasswordRecovery.txt`).

Po wprowadzania tych zmian ponownie `RecoverPassword.aspx` strony, a następnie wprowadź nazwę użytkownika i zabezpieczeń odpowiedzi. Pojawi się powinien wiadomości e-mail, która wygląda jak przedstawiony na rysunku 5. Należy pamiętać, że `webmaster@example.com` została czy DW i że zaktualizowano temat i treść.


[![Zaktualizowano tematu, treści i listy DW](recovering-and-changing-passwords-cs/_static/image14.png)](recovering-and-changing-passwords-cs/_static/image13.png)

**Rysunek 5**: tematu, treści i DW listy zostały zaktualizowane ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](recovering-and-changing-passwords-cs/_static/image15.png))


Do wysyłania wiadomości e-mail w formacie HTML ustawić [ `IsBodyHtml` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.maildefinition.isbodyhtml.aspx) True (wartość domyślna to False), a aktualizacji szablonu wiadomości e-mail, aby uwzględnić HTML.

`MailDefinition` Właściwość nie jest unikatowa dla klasy PasswordRecovery. Jak zostanie wyświetlone w kroku 2, kontroli Element ChangePassword oferuje również `MailDefinition` właściwości. Ponadto w formancie CreateUserWizard obejmuje takie właściwości można skonfigurować do automatycznego wysyłania wiadomości powitalnej wiadomości e-mail do nowych użytkowników.

> [!NOTE]
> Obecnie nie ma żadnych łączy w obszarze nawigacji po lewej stronie osiągnięcia `RecoverPassword.aspx` strony. Użytkownik może tylko być zainteresowani odwiedzenie tej strony, jeśli użytkownik nie mógł pomyślnie Zaloguj się do witryny. W związku z tym zaktualizować `Login.aspx` stronę, aby uwzględnić łącze do `RecoverPassword.aspx` strony.


### <a name="programmatically-resetting-a-users-password"></a>Programowo resetowania hasła użytkownika

Podczas resetowania hasła użytkownika PasswordRecovery kontrolować wywołania `MembershipUser` obiektu [ `ResetPassword` metody](https://msdn.microsoft.com/library/system.web.security.membershipuser.resetpassword.aspx). Ta metoda ma dwa przeciążenia:

- **[`ResetPassword`](https://msdn.microsoft.com/library/d94bdzz2.aspx)** -Resetuje hasło użytkownika. Użyj tego przeciążenia, jeśli `RequiresQuestionAndAnswer` ma wartość False.
- **[`ResetPassword(securityAnswer)`](https://msdn.microsoft.com/library/d90zte4w.aspx)** -Resetuje hasło użytkownika tylko wtedy, gdy podane *securityAnswer* jest poprawna. Użyj tego przeciążenia, jeśli `RequiresQuestionAndAnswer` ma wartość True.

Zarówno przeciążenia zwraca nowy, losowo wygenerowane hasło.

Za pomocą innych metod w ramach członkostwa, takich jak `ResetPassword` delegatów metody do skonfigurowanego dostawcy. `SqlMembershipProvider` Wywołuje `aspnet_Membership_ResetPassword` przekazywanie przechowywane procedury, nazwa użytkownika, nowe hasło i odpowiedź podane hasło, wśród innych pól. Procedura składowana gwarantuje, że odpowiedź hasła jest zgodny i następnie aktualizuje hasło użytkownika.

Kilka uwagi dotyczące implementacji niskiego poziomu:

- Blokady użytkownika nie można zresetować swoje hasło. Może jednak niezatwierdzonych użytkownika. Firma Microsoft będzie omawiać zablokowanym wychodzących i zatwierdzone stanów bardziej szczegółowo w <a id="_msoanchor_3"> </a> [ *Unlocking i zatwierdzania użytkownika* ](unlocking-and-approving-user-accounts-cs.md) samouczek kont.
- Jeśli odpowiedź hasła jest nieprawidłowa, jest zwiększany liczba prób odpowiedzi hasła użytkownika. Po wystąpieniu określonej liczby prób odpowiedzi nieprawidłowy zabezpieczeń w przedziale czasu, użytkownik jest zablokowany.

### <a name="a-word-on-how-the-random-passwords-are-generated"></a>Wyraz w sposób losowego hasła są generowane

Losowo generowany hasła wyświetlany w wiadomości e-mail w rysunki 4 i 5 są tworzone przez klasę członkostwa [ `GeneratePassword` metody](https://msdn.microsoft.com/library/system.web.security.membership.generatepassword.aspx). Ta metoda przyjmuje dwóch parametrów wejściowych całkowitą - *długość* i *numberOfNonAlphanumericCharacters* - i zwraca wartość typu ciąg, co najmniej *długość* znaków długości w najmniej *numberOfNonAlphanumericCharacters* liczba znaków innych niż alfanumeryczne. Gdy ta metoda jest wywoływana z klasy członkostwa lub kontrolki związane z logowaniem sieci Web, wartości tych parametrów są określane przez konfigurację członkostwa `MinRequiredPasswordLength` i `MinRequiredNonalphanumericCharacters` właściwości, które firma Microsoft 7 i 1, odpowiednio.

`GeneratePassword` — Metoda korzysta silną kryptograficznie generatora liczb losowych, aby upewnić się, nie jest odchylenia nie są wybrane jakie losowo wybranych znaków. Ponadto `GeneratePassword` jest `public`, co oznacza, że umożliwia bezpośrednio z poziomu aplikacji ASP.NET aby generować losową ciągów lub hasła.

> [!NOTE]
> `SqlMembershipProvider` Klasy zawsze generuje losowe hasło co najmniej 14 znaków, więc jeśli `MinRequiredPasswordLength` to mniej niż 14, a następnie jego wartość jest ignorowana.


## <a name="step-2-changing-passwords"></a>Krok 2: Zmienianie haseł

Losowo generowany hasła są trudne do zapamiętania. Należy wziąć pod uwagę hasła pokazano na rysunku 4: `WWGUZv(f2yM:Bd`. Sprawdź, który zobowiązuje się do pamięci! Needless, że po wysłaniu losowo wygenerowane hasło tego rodzaju użytkownika, użytkownik będzie Zmień hasło na łatwiejszą do zapamiętania.

Formant Element ChangePassword służy do tworzenia interfejsu użytkownika zmienić swoje hasło. Znacznie formantu PasswordRecovery kontroli Element ChangePassword składa się z dwóch widoków: Zmień hasło i Powodzenie. Zmień hasło widoku monituje użytkownika o starego i nowego hasła. Na dostarczenie poprawne stare hasło i nowe hasło, które spełnia minimalnej długości i wymagania dotyczące znaków innych niż alfanumeryczne, kontroli Element ChangePassword aktualizacji hasła użytkownika i wyświetla widoku powodzenia.

> [!NOTE]
> Element ChangePassword kontroli modyfikuje hasło użytkownika za pomocą `MembershipUser` obiektu [ `ChangePassword` metody](https://msdn.microsoft.com/library/system.web.security.membershipuser.changepassword.aspx). Metody ChangePassword akceptuje dwa `string` parametrów - wejściowych *Stare_hasło* i *NoweHasło*- i aktualizuje konto użytkownika z *NoweHasło*, Zakładając, że podana *Stare_hasło* jest poprawna.


Otwórz `ChangePassword.aspx` strony i Dodaj formant Element ChangePassword ze stroną, nazw `ChangePwd`. W tym momencie Pokaż hasło zmiany w widoku Projekt wyświetlania (patrz rysunek 6). Podobnie jak z formantem PasswordRecovery, można przełączyć między widokami za pomocą tagów inteligentnych formantu. Ponadto można dostosować za pomocą właściwości stylu różne lub konwertując je do szablonu są wystąpień tych widoków.


[![Dodawanie formantu Element ChangePassword ze stroną](recovering-and-changing-passwords-cs/_static/image17.png)](recovering-and-changing-passwords-cs/_static/image16.png)

**Rysunek 6**: Dodawanie formantu Element ChangePassword ze stroną ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](recovering-and-changing-passwords-cs/_static/image18.png))


Element ChangePassword formantu można zaktualizować hasła aktualnie zalogowanego użytkownika *lub* hasła innym, określonego użytkownika. Jak pokazano na rysunku 6, Zmień hasło widok domyślny renderuje trzech pól tekstowych: jeden dla starego hasła i dwie nowe hasło. Ten interfejs domyślny jest używany można zaktualizować hasła aktualnie zalogowanego użytkownika.

Aby użyć formantu Element ChangePassword do zaktualizowania hasła innego użytkownika, ustaw dla formantu [ `DisplayUserName` właściwość](https://msdn.microsoft.com/library/system.web.ui.webcontrols.changepassword.displayusername.aspx) na wartość True. W ten sposób dodaje czwarty pole tekstowe do strony monitowanie o nazwa użytkownika, którego hasło, aby zmienić.

Ustawienie `DisplayUserName` na wartość True jest przydatne, jeśli chcesz umożliwić użytkownikowi zarejestrowane się zmienić swoje hasła bez konieczności logowania się. Osobiście myślę, że nie ma problem z konieczności użytkownikowi na logowanie przed umożliwieniem jej zmienić swoje hasło. W związku z tym pozostaw `DisplayUserName` ustawiony na wartość False (domyślnej). W podjęciem decyzji, jednak firma Microsoft zasadniczo są połączeń użytkowników anonimowych z osiągnięcia tej strony. Aktualizowanie reguł autoryzacji adresów URL witryny tak, aby odmówić użytkowników anonimowych z wizytę `ChangePassword.aspx`. Jeśli potrzebujesz odświeżanie pamięci na składni reguł autoryzacji adresów URL, odwołaj się do <a id="_msoanchor_4"> </a> [ *autoryzacji na podstawie użytkownika* ](../membership/user-based-authorization-cs.md) samouczka.

> [!NOTE]
> Wydaje się, że `DisplayUserName` właściwość jest przydatna do pozwala administratorom na zmiany hasła innym użytkownikom. Jednak nawet wtedy, gdy `DisplayUserName` ma wartość True, musi być znane i wprowadzona poprawna stare hasło. Zostanie omawianiu techniki pozwala administratorom na zmiany haseł użytkowników w kroku 3.


Odwiedź stronę `ChangePassword.aspx` strony za pośrednictwem przeglądarki, a następnie zmiany hasła. Należy pamiętać, że jest wyświetlany komunikat o błędzie, jeśli wprowadź nowe hasło, które nie spełniają długość hasła i wymagania znak niealfanumeryczny określony w konfiguracji członkostwa (patrz rysunek 7).


[![Dodawanie formantu Element ChangePassword ze stroną](recovering-and-changing-passwords-cs/_static/image20.png)](recovering-and-changing-passwords-cs/_static/image19.png)

**Rysunek 7**: Dodawanie formantu Element ChangePassword ze stroną ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](recovering-and-changing-passwords-cs/_static/image21.png))


Po wprowadzeniu poprawne stare hasło i prawidłowe hasło nowe zalogowanego na użytkownika hasło jest zmieniane i wyświetlane w widoku powodzenia.

### <a name="sending-a-confirmation-email"></a>Wysyła wiadomość E-mail z potwierdzeniem

Domyślnie kontroli Element ChangePassword nie wysyła wiadomość e-mail do użytkownika, którego hasło właśnie zostało zaktualizowane. Jeśli chcesz wysłać wiadomość e-mail, wystarczy skonfigurować formantu `MailDefinition` właściwości. Skonfigurujmy kontroli Element ChangePassword tak, aby użytkownik są wysyłane w formacie HTML wiadomość e-mail zawierającą swoje nowe hasło.

Rozpocznij od utworzenia nowego pliku w `EmailTemplates` folder o nazwie `ChangePassword.htm`. Dodaj następujący kod:

[!code-html[Main](recovering-and-changing-passwords-cs/samples/sample4.html)]

Następnie ustawianie formantu Element ChangePassword `MailDefinition` właściwości `BodyFileName`, `IsBodyHtml`, i `Subject` właściwości ~ / EmailTemplates/ChangePassword.htm, True i hasła został zmieniony! odpowiednio.

Po wprowadzeniu tych zmian, ponownie strony i zmienić hasło ponownie. Tym razem kontroli Element ChangePassword wysyła niestandardowego, w formacie HTML wiadomość e-mail na adres e-mail użytkownika w pliku (patrz rysunek 8).


[![Wiadomość E-mail informuje że ich hasła użytkownika został zmieniony.](recovering-and-changing-passwords-cs/_static/image23.png)](recovering-and-changing-passwords-cs/_static/image22.png)

**Rysunek 8**: E-mail wiadomość informuje o tym że ich hasła użytkownika został zmieniony ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](recovering-and-changing-passwords-cs/_static/image24.png))


## <a name="step-3-allowing-administrators-to-change-users-passwords"></a>Krok 3: Pozwala administratorom na zmiany haseł użytkowników

Typową funkcją w aplikacjach obsługujących konta użytkownika jest możliwość przez użytkownika administracyjnego zmienić hasła innych użytkowników. Czasami ta funkcja jest wymagana, ponieważ system nie ma możliwości zmiany haseł przez użytkowników. W takim przypadku jedynym sposobem, aby użytkownik mógł odzyskać zapomniane hasło będzie ich przypisać nowe hasło administratora. PasswordRecovery i Element ChangePassword kontroli jednak użytkownicy administracyjni muszą nie zajęty się ze zmianą hasła użytkowników, jak użytkownicy są w stanie wykonać tego same.

Ale co zrobić, jeśli klient sobie, że użytkownicy administracyjni powinna być możliwość zmiany hasła innym użytkownikom? Niestety dodanie tej funkcji może być nieco pracy. Aby zmienić hasło użytkownika, należy podać stare i nowe hasło do `MembershipUser` obiektu `ChangePassword` metody, ale administrator nie musi znać hasło użytkownika Aby zmodyfikować go.

Jednym z rozwiązań jest najpierw resetowania hasła użytkownika, a następnie zmień go do nowego hasła przy użyciu kodu podobne do poniższych:

[!code-aspx[Main](recovering-and-changing-passwords-cs/samples/sample5.aspx)]

Ten kod rozpoczyna się od sieci pobiera informacje o *username*, czyli użytkownika, którego administrator chce zmienić hasło. Następnie `ResetPassword` wywoływana jest metoda, która przypisuje i użytkownika, hasło nowy, losowy. Losowo wygenerowane hasło jest zwracany przez metodę i przechowywane w zmiennej `resetPwd`. Teraz, wiemy, hasło użytkownika, możemy można zmienić po za pośrednictwem wywołania `ChangePassword`.

Problem jest, że ten kod działa tylko, jeśli konfiguracja systemu członkostwa jest ustawiona tak, aby `RequiresQuestionAndAnswer` ma wartość False. Jeśli `RequiresQuestionAndAnswer` ma wartość PRAWDA, jak w przypadku aplikacji, a następnie `ResetPassword` — metoda musi zostać przekazane odpowiedź zabezpieczeń, w przeciwnym razie spowoduje zgłoszenie wyjątku.

Jeśli w ramach członkostwa jest skonfigurowany do żądania pytanie zabezpieczające i odpowiedzi, a jeszcze klienckie sobie, że administratorzy można zmienić hasła użytkowników, dostępne są trzy opcje:

- Throw dłoni niebu z i poinformuj klienta, że jest to tylko jeden element, którego nie można wykonać.
- Ustaw `RequiresQuestionAndAnswer` o wartości False. Powoduje to mniej bezpieczna opcja aplikacji. Załóżmy, że nefarious użytkownika ma uzyska dostęp do skrzynki odbiorczej poczty e-mail innego użytkownika. Możliwe, że użytkownik, którego bezpieczeństwo zostało naruszone opuścił ich technicznej, aby przejść do obiad i nie zablokować stacji roboczej lub może uzyskać dostępu do poczty e-mail z publicznego terminalu i nie Wyloguj. W obu przypadkach nefarious użytkownik może odwiedzić `RecoverPassword.aspx` strony, a następnie wprowadź nazwy użytkownika. System będzie poczty e-mail odzyskane hasła bez monitowania o odpowiedź zabezpieczeń.
- Obejście warstwy abstrakcji utworzony przez framework członkostwa i pracować bezpośrednio z bazy danych programu SQL Server. Schemat członkostwo obejmuje procedury składowanej o nazwie `aspnet_Membership_SetPassword` który ustawia hasło użytkownika i nie wymaga w celu wykonania zadania, jego odpowiedź zabezpieczeń lub stare hasło.

Żaden z tych opcji nie jest szczególnie atrakcyjne, ale jest to jak czasem życia dewelopera przejdzie.

I dalej poszło i zaimplementowana trzeci podejście, pisania kodu, które pomija `Membership` i `MembershipUser` klas i działa bezpośrednio przed `SecurityTutorials` bazy danych.

> [!NOTE]
> Praca bezpośrednio z bazą danych, jest shattered hermetyzacji dostarczane przez platformę członkostwa. Ta decyzja wiąże nam `SqlMembershipProvider`, co naszego kodu mniej przenośny. Ponadto ten kod może nie działać zgodnie z oczekiwaniami w przyszłych wersjach programu ASP.NET w przypadku zmiany schematu członkostwa. Ta metoda jest obejście tego problemu i jak większość rozwiązania nie jest przykładem najlepsze rozwiązania.


Kod niektórych nieatrakcyjnych bitów i jest bardzo długi. W związku z tym nie chcę zajmowały tego samouczka z szczegółowe badanie. Jeśli chcesz dowiedzieć się więcej, Pobierz kod dla tego samouczka i odwiedziny `~/Administration/ManageUsers.aspx` strony. Ta strona, której utworzono w <a id="_msoanchor_5"> </a> [poprzedniego samouczek](building-an-interface-to-select-one-user-account-from-many-cs.md), listy poszczególnych użytkowników. Po aktualizacji widoku GridView, aby uwzględnić łącze do `UserInformation.aspx` strony przekazywanie wybranego użytkownika za pośrednictwem ciąg zapytania. `UserInformation.aspx` Stronie wyświetlane są informacje o wybrany użytkownik i pól tekstowych do zmiany hasła (patrz rysunek 9).

Po wprowadzić nowe hasło, potwierdzenie, w drugim polu tekstowym i kliknięcie przycisku Aktualizuj użytkownika, ensues odświeżania strony i `aspnet_Membership_SetPassword` jest wywoływana procedura składowana aktualizowania hasła użytkownika. I zachęca tych czytników zainteresowane w tej funkcji w celu zapoznanie się ze kod i spróbuj rozszerzania funkcji, aby uwzględnić wysyłanie wiadomości e-mail do użytkownika, którego hasło zostało zmienione.


[![Administrator może zmienić hasło użytkownika](recovering-and-changing-passwords-cs/_static/image26.png)](recovering-and-changing-passwords-cs/_static/image25.png)

**Rysunek 9**: Administrator może zmienić hasło użytkownika ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](recovering-and-changing-passwords-cs/_static/image27.png))


> [!NOTE]
> `UserInformation.aspx` Strony obecnie działa tylko, jeśli w ramach członkostwa jest skonfigurowany do przechowywania haseł w formacie zwykłego lub Hashed. Brak kodu do szyfrowania nowe hasło, mimo że zaproszono Cię do dodać tę funkcję. Sposób najlepiej Dodawanie kodu konieczne jest użycie decompiler jak [reflektora](http://www.aisto.com/roeder/dotnet/) zbadanie kodu źródłowego dla metod w programie .NET Framework; start, sprawdzając `SqlMembershipProvider` klasy `ChangePassword` metody. Jest to technika, używanych do pisania kodu do tworzenia skrótu hasła.


## <a name="summary"></a>Podsumowanie

Program ASP.NET oferuje dwóch formantów, aby ułatwić użytkownikom zarządzanie swoje hasło. Formant PasswordRecovery jest przydatne dla użytkowników, którzy pamiętasz hasła. W zależności od konfiguracji framework członkostwa użytkownika jest albo pocztą e-mail powiadomienia o ich istniejące hasło lub nowe, losowo wygenerowane hasło. Element ChangePassword kontroli umożliwia użytkownikowi zaktualizować swoje hasło.

Tak, jak logowanie i CreateUserWizard formanty formanty PasswordRecovery i Element ChangePassword renderowania interfejsu użytkownika sformatowanego bez konieczności pisania lizawki deklaratywne znaczników lub wiersz kodu. Jeśli domyślny interfejs użytkownika nie odpowiada potrzebom użytkownika, można dostosować go za pomocą różnych właściwości stylu. Alternatywnie interfejsów formantów może zostać przekształcone szablonów dla nawet dokładniejszą kontrolę. W tle tych kontrolek za pomocą interfejsu API członkostwa, wywoływania `MembershipUser` obiektu `ResetPassword` i `ChangePassword` metody.

Programowanie przyjemność!

### <a name="further-reading"></a>Dalsze informacje

Więcej informacji dotyczących tematów omówionych w tym samouczku można znaleźć w następujących zasobach:

- [Element ChangePassword kontroli poradniki Szybki Start](https://quickstarts.asp.net/QuickStartv20/aspnet/doc/ctrlref/login/changepassword.aspx)
- [Przewodniki Szybki Start PasswordRecovery formantu](https://quickstarts.asp.net/QuickStartv20/aspnet/doc/ctrlref/login/passwordrecovery.aspx)
- [Wysyłanie wiadomości E-mail w programie ASP.NET](http://aspnet.4guysfromrolla.com/articles/072606-1.aspx)
- [`System.Net.Mail` Często zadawane pytania](http://www.systemnetmail.com/)

### <a name="about-the-author"></a>Informacje o autorze

Scott Bento, Utwórz wiele książek ASP/ASP.NET i twórcę 4GuysFromRolla.com, pracuje z technologii Microsoft Web od 1998. Scott działa jako niezależnego konsultanta trainer i składnika zapisywania. Jest jego najnowszej książki  *[Sams nauczyć się ASP.NET 2.0 w ciągu 24 godzin](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)*. Scott jest osiągalny w [ mitchell@4guysfromrolla.com ](mailto:mitchell@4guysfromrolla.com) lub za pośrednictwem jego blogu w [ http://ScottOnWriting.NET ](http://scottonwriting.net/).

### <a name="special-thanks-to"></a>Specjalne podziękowania dla

Ten samouczek serii zostało sprawdzone przez wiele recenzentów przydatne. Prowadzić osób dokonujących przeglądu, w tym samouczku obejmują Michael Emmings i Suchi Banerjee. Zainteresowani recenzowania Moje nadchodzących artykuły MSDN? Jeśli tak, Porzuć mnie linii w [mitchell@4GuysFromRolla.com](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Poprzednie](building-an-interface-to-select-one-user-account-from-many-cs.md)
> [dalej](unlocking-and-approving-user-accounts-cs.md)
