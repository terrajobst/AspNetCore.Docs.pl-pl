---
uid: web-forms/overview/moving-to-aspnet-20/membership
title: Członkostwo | Dokumentacja firmy Microsoft
author: microsoft
description: Członkostwo ASP.NET jest oparta na powodzenie modelu uwierzytelniania formularzy z platformy ASP.NET 1.x. Uwierzytelnianie formularzy programu ASP.NET zapewnia wygodny sposób incorp...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/20/2005
ms.topic: article
ms.assetid: f2339485-5d78-4c5e-8c0a-dc9b8a315345
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/moving-to-aspnet-20/membership
msc.type: authoredcontent
ms.openlocfilehash: ebba0c25fd3c7d6de7182c14559add7902cafe83
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/03/2018
ms.locfileid: "37388399"
---
<a name="membership"></a>Członkostwo
====================
przez [firmy Microsoft](https://github.com/microsoft)

> Członkostwo ASP.NET jest oparta na powodzenie modelu uwierzytelniania formularzy z platformy ASP.NET 1.x. Uwierzytelnianie formularzy programu ASP.NET zapewnia wygodny sposób uwzględnienie formularz logowania w aplikacji ASP.NET i weryfikowania użytkowników przed bazę danych lub w innym magazynie danych.


Członkostwo ASP.NET jest oparta na powodzenie modelu uwierzytelniania formularzy z platformy ASP.NET 1.x. Uwierzytelnianie formularzy programu ASP.NET zapewnia wygodny sposób uwzględnienie formularz logowania w aplikacji ASP.NET i weryfikowania użytkowników przed bazę danych lub w innym magazynie danych. Elementy członkowskie klasy uwierzytelniania formularzy umożliwiają obsługę plików cookie uwierzytelniania, sprawdź, czy są dostępne prawidłowe nazwy logowania, logowania użytkownika w poziomie itp. Implementowanie uwierzytelniania formularzy w aplikacji ASP.NET 1.x mogą jednak wymagać ilość kodu.

Członkostwo w programie ASP.NET 2.0 jest główne zawodowego za pośrednictwem za pomocą uwierzytelniania formularzy, które samodzielnie. (Członkostwo jest najlepszy w taki sposób, w połączeniu z uwierzytelniania formularzy, ale za pomocą uwierzytelniania formularzy nie jest to wymagane). Jak szybko zobaczysz, służy członkostwa ASP.NET i kontrolek logowania programu ASP.NET 2.0 do implementowania systemu członkostwa zaawansowanego bez konieczności pisania większej ilości kodu w ogóle.

## <a name="implementing-membership-in-aspnet-20"></a>Implementowanie członkostwa w programie ASP.NET 2.0

Członkostwo jest implementowany przez następujące cztery kroki. Należy pamiętać o tym, istnieje wiele podrzędnych kroków, które są zaangażowane oraz opcjonalnym, który może być również implementowana. Te kroki są przeznaczone do zilustrowania szerszej perspektywie konfigurowania członkostwa.

1. Tworzenie bazy danych członkostwa (Jeśli program SQL Server jest używany jako magazyn członkostwa).
2. Określ opcje członkostwa w plikach konfiguracji aplikacji. (Członkostwa jest włączona domyślnie).
3. Określanie typu magazynu członkostwa, którego chcesz użyć. Dostępne są następujące opcje: 

    - Microsoft SQL Server (w wersji 7.0 lub nowszy)
    - Active Directory Store
    - Niestandardowego dostawcy członkostwa
4. Skonfiguruj aplikację uwierzytelniania formularzy programu ASP.NET. Jeszcze raz członkostwa jest przeznaczona do korzystać z uwierzytelniania formularzy, ale za pomocą uwierzytelniania formularzy nie jest wymagane.
5. Zdefiniuj kontami użytkowników w ramach członkostwa i konfigurowania ról, jeśli to konieczne.

## <a name="creating-the-membership-database"></a>Tworzenie bazy danych członkostwa

Jeśli swój przy użyciu programu SQL Server 7.0 lub później, jako magazyn członkostwa, można użyć aspnet\_narzędzie regsql (dostępne najłatwiej z programu Visual Studio .NET 2005 wiersza polecenia) do konfiguracji bazy danych. Aspnet\_regsql narzędzie może służyć jako narzędzie wiersza polecenia lub przy użyciu Kreatora graficznego interfejsu użytkownika. Metoda Kreator jest najprostszym sposobem skonfigurowania bazy danych. Aby uzyskać dostęp do kreatora, po prostu uruchom następujące polecenie:

`aspnet_regsql W`

Po uruchomieniu tego polecenia, zostanie wyświetlona przy użyciu Kreatora instalacji serwera SQL programu ASP.NET jak pokazano poniżej.


![](membership/_static/image1.jpg)

**Rysunek 1.**


Kreator instalacji serwera SQL platformy ASP.NET tworzy witrynę sieci Web w wystąpieniu, które są określone w kreatorze. Jednak ASP.NET użyje parametry połączenia w pliku machine.config nawiązywania połączenia z bazą danych. Domyślnie wskaże te parametry połączenia do wystąpienia programu SQL Server 2005, dlatego jeśli używasz wystąpienia programu SQL Server 2000 lub SQL Server 7.0, należy zmodyfikować parametry połączenia w pliku machine.config. Te parametry połączenia można znaleźć tutaj:

[!code-xml[Main](membership/samples/sample1.xml)]

Niestety Jeśli nie zmodyfikujesz parametry połączenia programu ASP.NET nie zapewni opisem błędu. Nadal będzie po prostu zaczną zgłaszać skargi powiedzenie, że nie utworzono bazy danych. W przypadku powyższych zmodyfikowano I parametry połączenia, aby wskazywał Moje lokalne wystąpienie programu SQL Server 2000.

## <a name="specifying-configuration-and-adding-users-and-roles"></a>Określanie konfiguracji i dodawanie użytkowników i ról

Następnym etapem konfigurowania członkostwa jest dodawanie informacji niezbędnych do pliku web.config aplikacji. W programie ASP.NET: 1.x, modyfikując plik web.config czasami trudno było ze względu na użycie lowerCamelCase i braku Intellisense. Visual Studio .NET w wersji 2005 sprawia, że zadanie jest znacznie łatwiejsze za pomocą funkcji Intellisense dla plików konfiguracyjnych, ale ASP.NET 2.0 pójść o krok dalej, zapewniając interfejs sieci Web dla edycję plików konfiguracyjnych.

Interfejs sieci Web można uruchomić, klikając przycisk Konfiguracja platformy ASP.NET na pasku narzędzi Eksploratora rozwiązań, jak pokazano poniżej. Można również uruchomić interfejs sieci Web za pomocą okienka wyskakujące, które są wyświetlane, gdy są wstawiane kontrolek logowania.


![](membership/_static/image2.jpg)

**Rysunek 2**


Spowoduje to uruchomienie narzędzie Administratorskie witryny sieci Web ASP.NET, pokazano poniżej. Administrowanie witryną sieci Web platformy ASP.NET to interfejs cztery karty, która ułatwia zarządzanie ustawieniami aplikacji. Dostępne są następujące karty:

- **Strona główna**
- **Zabezpieczenia** użytkowników, ról i dostępu.
- **Aplikacja** konfigurowania ustawień aplikacji.
- **Dostawca** Konfiguracja i testowanie aplikacji dostawcy członkostwa.

Narzędzie Administratorskie witryny sieci Web pozwala łatwo utworzyć nowych użytkowników, tworzenie nowych ról, a także do zarządzania użytkownikami i rolami. Ta możliwość nie jest dostępne w interfejsie Windows. Interfejs Windows pozwala łatwo definiować ustawienia autoryzacji i dodawanie, usuwanie i zarządzać dostawcami, funkcje, które nie znajdują się w narzędzia do administrowania witryną sieci Web.

Aby uruchomić interfejs Windows, otwórz przystawkę Internetowe usługi informacyjne, kliknij prawym przyciskiem myszy w swojej aplikacji i wybierz polecenie Właściwości. Kliknij kartę programu ASP.NET, a następnie kliknij przycisk Edytuj konfigurację. (Aplikacji musi być uruchomiona w ramach programu ASP.NET 2.0 dla przycisku edycji konfiguracji do włączenia. Można skonfigurować verze technologie ASP.NET w oknie dialogowym aplikacji ASP.NET.) Zostanie wyświetlone okno dialogowe Ustawienia konfiguracji programu ASP.NET, jak pokazano poniżej.


![](membership/_static/image3.jpg)

**Rysunek 3.**


Na karcie Ogólne parametry połączenia i ustawienia aplikacji są wymienione. Wszystkie ustawienia kursywą są zdefiniowane w pliku konfiguracji nadrzędnej (w pliku machine.config lub web.config na wyższym poziomie), a ustawienia nie kursywą pochodzą z pliku konfiguracji aplikacji. Jeśli ustawienie zostanie dodany, usunąć lub edytować na poziomie aplikacji ASP.NET będzie dodać, usunąć lub zmodyfikować ustawienia w pliku web.config poziomy aplikacji zamiast usuwania ustawienia z pliku konfiguracji, z którego jest dziedziczona.

Poniżej pokazano kartę uwierzytelnianie. Jest to, gdy skonfigurujesz ustawienia członkostwa. Tworzy ustawienia uwierzytelniania, dostawców członkostwa i dostawców ról można skonfigurować w tym miejscu.


![](membership/_static/image4.jpg)

**Rysunek 4**


## <a name="implementing-membership-in-your-application"></a>Implementowanie członkostwa w aplikacji

Najprostszy sposób implementowania członkostwa ASP.NET w wersji 2.0 w aplikacji jest użycie podana kontroli logowania. Ta metoda umożliwia Implementowanie podstawowe informacje dotyczące członkostwa programu ASP.NET 2.0 bez pisania kodu w ogóle.

Następujące elementy sterujące logowania są dostępne w programie ASP.NET 2.0:

## <a name="login-control"></a>Kontrolka Login

Kontrolka Login udostępnia interfejs dla kogoś zalogować się do systemu członkostwa. Udostępnia przycisk logowania i textboxt nazwy użytkownika i hasła. Wiele innych typowych funkcji np. łącze do zarejestrowania dla osób, które nie zostały jeszcze wykonane, dzięki czemu pole wyboru, która umożliwia użytkownikom automatyczne logowanie przy następnych wizytach, łącza do monitu o hasło itp. Wszystkie funkcje kontroli logowania są możliwe do dostosowania przy użyciu właściwości formantu.

W programie ASP.NET: 1.x, deweloperzy musieli napisać ilość kodu w celu wyszukiwania, korzystając z uwierzytelniania formularzy. Za pomocą członkostwa programu ASP.NET 2.0 można sprawdzić poprawność użytkowników bez pisania kodu w ogóle. Program ASP.NET automatycznie wykona wyszukiwania użytkownika. (Jeśli używasz kontrolki logowania bez użycia członkostwa ASP.NET, możesz użyć **OnAuthenticate** metody do weryfikacji użytkownika.)

## <a name="loginview-control"></a>Kontrolki widoku logowania

Kontrolki widoku logowania jest formant z szablonem, który zawiera dwa szablony domyślne; AnonymousTemplate i LoggedInTemplate. Szablon, który jest wyświetlany jest określany przez Określa, czy użytkownik jest zalogowany do systemu członkostwa. Ten formant zazwyczaj służy do wyświetlania kontrolki logowania, gdy użytkownik nie ma jeszcze zalogowany i kontrolki stanu logowania i/lub innych kontrolek logowania, gdy użytkownik jest zalogowany. Jeśli używasz zarządzania rolami w aplikacji programu ASP.NET, kontrolki widoku logowania można wyświetlić określonego szablonu na podstawie ról użytkowników. (Więcej na platformie ASP.NET Zarządzanie rolami zostały omówione później.)

## <a name="passwordrecovery-control"></a>Kontrolka PasswordRecovery

PasswordRecovery temu użytkownicy będą otrzymywać wiadomości e-mail przy użyciu bieżącego hasła lub zresetować swoje hasło. Zwykły tekst i zaszyfrowane hasła można odzyskać, a pocztą e-mail do użytkowników. Jeśli hasło jest mieszany, nie można odzyskać. Zamiast tego użytkownik będzie musiał przeprowadzić resetowanie hasła.

## <a name="loginstatus-control"></a>Kontrolki stanu logowania

Kontrolki stanu logowania jest używany do wyświetlania wskaźnik logowania dla użytkowników, którzy nie są rejestrowane w i wskaźnik wylogowania dla użytkowników, którzy są aktualnie zalogowany. Właściwość Request.IsAuthenticated jest używana ustalenie, który wskaźnik do wyświetlenia. Wskaźnik wyświetlany przez kontrolkę stanu logowania mogą być tekstem (wdrożone za pośrednictwem **LoginText** i **LogoutText** właściwości) lub obrazów (wdrożone za pośrednictwem **LoginImageUrl**i **LogoutImageUrl** właściwości.)

Gdy użytkownik zaloguje za pomocą kontrolki stanu logowania, użytkownik jest przekierowywany do adresu URL określonego przez **LogoutPageUrl** właściwości. Jeśli ta właściwość nie jest ustawiona, są odświeżane bieżącej strony. Ponieważ lokacji prawdopodobnie jest chroniony przez uwierzytelnianie formularzy, odświeżanie w bieżącej stronie spowoduje przekierowanie użytkownika do strony logowania dla witryny.

## <a name="loginname-control"></a>Kontrolki nazwy logowania

Kontrolki nazwy logowania jest wyświetlana nazwa użytkownika zalogowanego obecnie do witryny.

## <a name="createuserwizard-control"></a>Kontrolka CreateUserWizard

Kontrolka CreateUserWizard zapewnia użytkownikom wygodny sposób rejestrowania w systemie członkostwa. Możesz dodać kroki (zaimplementowane jako zbiór WizardSteps) za pośrednictwem interfejsu, pokazano poniżej.


![](membership/_static/image5.jpg)

**Rysunek 5**


CreateUserWizard jest oparte na szablonach formant, który pochodzi od klasy kreatora i zawiera następujące szablony:

- **HeaderTemplate** ten szablon określa wygląd nagłówka kreatora.
- **SidebarTemplate** ten szablon określa wygląd paska bocznego w kreatorze.
- **StartNavigationTemplate** tej kontrolki szablonu wygląd nawigacji są kreatora na etapie rozpoczęcia.
- **StepNavigationTemplate** ten szablon określa wygląd obszar nawigacji nie w kroku rozpoczęcia lub zakończenia.
- **FinishNavigationTemplate** ten szablon określa wygląd obszar nawigacji, gdy w kroku kończenie.

Ponadto dla każdego kroku dodasz do kreatora ASP.NET spowoduje utworzenie szablonu niestandardowego, który zawiera zarówno ContentTemplate, jak i CustomNavigationTemplate dla tego kroku. Aby uzyskać szczegółowe informacje o dostosowywaniu CreateUserWizard zobacz dokumentację VS.NET 2005:

## <a name="changepassword-control"></a>Kontrolka ChangePassword

Kontrolka ChangePassword umożliwia użytkownikom zmianę swojego hasła. Jeśli właściwość nazwy użytkownika ma wartość true, (jest to wartość false, domyślnie), użytkownik może zmienić swoje hasło, gdy nie są rejestrowane. Jeśli użytkownik *jest* jest zalogowany i właściwości nazwy użytkownika jest spełniony, użytkownik będzie mógł zmienić hasło innego użytkownika, który nie jest rejestrowana w zapewnienie wiedzieli, identyfikator użytkownika, ten użytkownik.

Należy pamiętać o tym, jeśli chcesz, aby użytkownicy mogli zmieniać hasła bez konieczności zalogowania, konieczne będzie upewnij się, że strona, na którym jest wyświetlany formantu ChangePassword zezwala na dostęp anonimowy. Oczywiście użytkownicy będą musieli podać swoje stare hasło, aby zmienić swoje hasło.

## <a name="role-management"></a>Zarządzanie rolami

Zarządzanie rolami umożliwia przypisywanie użytkowników do określonej roli, a następnie ograniczyć dostęp do niektórych plików lub folderów na podstawie tej roli. Zarządzanie rolami również zapewnia interfejs API, dzięki czemu można programowo określić ktoś roli lub określić wszyscy użytkownicy w określonej roli i reagować odpowiednio.

Zarządzanie rolami nie jest wymagane członkostwo ASP.NET nie jest to wymagane, aby można było używać Zarządzanie rolami członkostwo w grupie. Jednak dwa stanowią uzupełnienie sobą dobrze i jest prawdopodobne, że deweloperzy będą ich używać w połączeniu ze sobą.

Aby włączyć zarządzanie rolami w aplikacji, wprowadź następującą zmianę w pliku web.config:

[!code-xml[Main](membership/samples/sample2.xml)]

Gdy **cacheRolesInCookie** atrybut jest ustawiony na wartość true, ASP.NET buforuje członkostwo w roli użytkowników w pliku cookie na komputerze klienckim. Dzięki temu roli wyszukiwania bez wywołania do elementu RoleProvider. Korzystając z tego atrybutu, deweloperzy są zachęcani do upewnij się, że **cookieProtection** atrybut jest ustawiony na wszystkie. (Jest to ustawienie domyślne). Gwarantuje to, że dane pliku cookie są szyfrowane i pomaga zapewnić, że zawartość plików cookie nie zostały zmienione. Role można dodać za pomocą narzędzia do administrowania witryną sieci Web. Umożliwia on łatwe Definiowanie ról, skonfiguruj dostęp do części witryny na podstawie tych ról i przypisywania użytkowników do ról.


![](membership/_static/image6.jpg)

**Rysunek 6.**


Jak wspomniano powyżej, można dodać nowe role po prostu wprowadź nazwę roli, a następnie klikając polecenie Dodaj rolę. Istniejące role mogą zarządzane lub usunięty przez kliknąć odpowiednie łącze na liście istniejące role.

Podczas zarządzania roli można dodać lub usunąć użytkowników, jak pokazano poniżej.


![](membership/_static/image7.jpg)

**Rysunek 7**


Zaznaczając pole wyboru jest w roli użytkownika, można łatwo dodać użytkownika do określonej roli. ASP.NET automatycznie zaktualizuje bazę danych członkostwa odpowiednie pozycje. Należy również skonfigurować reguły dostępu do aplikacji. Deweloperzy 1.x ASP.NET zaczynasz temu za pośrednictwem &lt;autoryzacji&gt; elementu w pliku web.config, a ta opcja jest nadal dostępny w programie ASP.NET 2.0. Jednak łatwiej zarządzać dostępem reguł za pomocą witryny sieci Web administracji narzędzia jak pokazano poniżej.


![](membership/_static/image8.jpg)

**Rysunek 8**


W takim przypadku zostanie wyróżniona folderu administracji (jego trudno będą widoczne, ponieważ narzędzie wyróżnia ją w kolorze szarym światła) i roli Administratorzy przyznano dostęp. Wszyscy pozostali użytkownicy są odrzucane. Możesz kliknąć ikonę głównego, wybierz regułę, a następnie użyć przycisków Przenieś w górę i Przenieś w dół, aby uzgodnić zasady. Podobnie jak w przypadku platformy ASP.NET &lt;autoryzacji&gt; elementu reguły są przetwarzane w kolejności, w jakiej są wyświetlane. Oznacza to jeśli kolejność reguł na powyższym zrzucie zostały cofnięte, nikt będzie mieć dostęp do folderu administracji ponieważ pierwszą regułę, która napotyka ASP.NET będzie regułę, która nie zezwala na wszystkich użytkowników do folderu.

Program ASP.NET 2.0 dodaje plik web.config do folderu, dla którego określasz reguły dostępu. Reguły dostępu można edytować za pomocą pliku konfiguracji lub przy użyciu narzędzia do administrowania witryną sieci Web. Innymi słowy narzędzie Administratorskie witryny sieci Web jest po prostu interfejs, za pomocą którego można edytować plik konfiguracji w środowisku przyjazny dla użytkownika.

## <a name="using-roles-in-code"></a>Przy użyciu ról w kodzie

Interfejs API do zarządzania rolami nie zmienił się od wersji 1.x. **IsInRole** metoda jest używana do określenia, czy użytkownik ma określoną rolę.

[!code-csharp[Main](membership/samples/sample3.cs)]

ASP.NET również tworzy wystąpienie RolePrincipal jako członek bieżącego kontekstu. Obiekt RolePrincipal może służyć do uzyskania wszystkich ról, do których należy użytkownik w następujący sposób:

[!code-csharp[Main](membership/samples/sample4.cs)]

## <a name="using-rolegroups-with-the-loginview-control"></a>Kolekci RoleGroups przy użyciu kontrolki widoku logowania

Teraz, gdy masz zrozumienia zarządzania rolami i członkostwa, umożliwia krótko omówiono, jak kontrolki widoku logowania korzysta z tej funkcji w programie ASP.NET 2.0. Jak już wspomniano kontrolki widoku logowania jest formant z szablonem, który zawiera dwa szablony domyślne; AnonymousTemplate i LoggedInTemplate. W widoku logowania zadania, które okno dialogowe jest link (poniżej) umożliwiający upravit kolekci RoleGroups.


![](membership/_static/image9.jpg)

**Rysunek 9**


Każdy obiekt RoleGroup zawiera tablicę ciągów, który definiuje role, których dotyczy RoleGroup. Aby dodać nowe RoleGroup kontrolki widoku logowania, kliknij link Edytuj RoleGroups. Na powyższej ilustracji widać, że po dodaniu nowego RoleGroup dla administratorów. Wybierając ten RoleGroup (RoleGroup[0]) z listy rozwijanej widoki, można skonfigurować szablon, który będzie wyświetlany tylko członkowie roli administratora. Na poniższej ilustracji I zostały dodane nowe RoleGroup, która ma zastosowanie do elementów członkowskich w roli Sprzedaż i rola dystrybucji. Spowoduje to dodanie drugiego RoleGroup widoków listy rozwijanej w oknie dialogowym zadania widoku logowania i nic dodane do szablonu, którego będzie widoczny przez dowolnego użytkownika w sprzedaży lub dystrybucji roli.


![](membership/_static/image10.jpg)

**Na rysunku nr 10**


## <a name="overriding-the-existing-membership-provider"></a>Zastępowanie istniejącego dostawcy członkostwa

Istnieje kilka sposobów rozszerzyć funkcjonalność członkostwa ASP.NET. Po pierwsze oczywiście można zmienić istniejących funkcji klasy SqlMembershipProvider z niego dziedziczącego i przesłanianie jej metody. Na przykład jeśli chcesz zaimplementować swoje własne funkcje, podczas tworzenia użytkowników można utworzyć własne klasy, która dziedziczy SqlMembershipProvider w następujący sposób:

[!code-csharp[Main](membership/samples/sample5.cs)]

Jeśli z drugiej strony, użytkownik chce utworzyć własnego dostawcę (na przykład przechowywać informacje o członkostwie w bazie danych programu Access), można utworzyć własnego dostawcę.

## <a name="creating-your-own-membership-provider"></a>Tworzenie dostawcy członkostwa

Do utworzenia własnego dostawcy członkostwa, należy najpierw utworzyć klasę, która dziedziczy z klasy MembershipProvider. Jeśli używasz VB.NET, Visual Studio 2005 doda wycinków dla wszystkich metod, które chcesz zmienić. Jeśli używasz języka C#, jego maksymalnie można dodać wycinki.

Należy zastąpić następujące czynności:

- Właściwość ApplicationName
- Funkcja ChangePassword
- ChangePasswordQuestionAndAnswer — funkcja
- Tworzenie użytkownika — funkcja
- DeleteUser — funkcja
- EnablePasswordReset właściwości
- Właściwość EnablePasswordRetrieval
- FindUsersByEmail — funkcja
- FindUsersByName — funkcja
- GetAllUsers — funkcja
- GetNumberOfUsersOnline — funkcja
- GetPassword — funkcja
- GetUser — funkcja
- GetUserNameByEmail — funkcja
- Właściwość MaxInvalidPasswordAttempts
- Wartość MinRequiredNonAlphanumericCharacters właściwości
- Wartość MinRequiredPasswordLength właściwości
- Właściwość PasswordAttemptWindow
- Właściwość PasswordFormat
- Właściwość PasswordStrengthRegularExpression
- RequiresQuestionAndAnswer właściwości
- RequiresUniqueEmail właściwości
- ResetPassword — funkcja
- Odblokuj użytkownika — funkcja
- UpdateUser — funkcja
- Funkcja ValidateUser

Thats dość listy do zaimplementowania dewelopera języka C#. Może się okazać je łatwiej tworzyć klasy w VB.NET bez implementacji, a następnie użyj odblaskowego .NET lub podobnego narzędzia, aby przekonwertować kod w języku C#.

Parametry połączenia i inne właściwości, należy ustawić wartości domyślne metody inicjowania. (Metoda inicjowania uruchamiane, gdy dostawca jest ładowany w czasie wykonywania.) Drugi parametr do metody inicjowania typu System.Collections.Specialized.NameValueCollection i jest odwołaniem do &lt;Dodaj&gt; element, który jest skojarzony z niestandardowego dostawcy w pliku web.config. Ten wpis wygląda podobnie do poniższego:

[!code-xml[Main](membership/samples/sample6.xml)]

Oto przykład metody inicjowania.

[!code-csharp[Main](membership/samples/sample7.cs)]

W celu potwierdzenia tożsamości użytkownika, gdy przesyła formularz logowania, należy użyć metody ValidateUser. Ta metoda jest uruchamiana, gdy użytkownik kliknie przycisk Zaloguj się w kontrolce logowania. Umieści Twoje kod, który obsługuje wyszukiwanie użytkowników w przypadku tej metody.

Jak widać, pisanie własnego dostawcy członkostwa jest trudne i umożliwia rozszerzenie to zaawansowane funkcje platformy ASP.NET w wersji 2.0.
