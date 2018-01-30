---
uid: web-forms/overview/moving-to-aspnet-20/membership
title: "Członkostwo | Dokumentacja firmy Microsoft"
author: microsoft
description: "Członkostwo ASP.NET opiera się na powodzenie modelu uwierzytelniania formularzy z platformy ASP.NET 1.x. Uwierzytelnianie formularzy ASP.NET oferuje wygodny sposób incorp..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/20/2005
ms.topic: article
ms.assetid: f2339485-5d78-4c5e-8c0a-dc9b8a315345
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/moving-to-aspnet-20/membership
msc.type: authoredcontent
ms.openlocfilehash: 1a5a495845b60f9aac51c9776311af67f5dc8767
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/30/2018
---
<a name="membership"></a>Członkostwo
====================
przez [firmy Microsoft](https://github.com/microsoft)

> Członkostwo ASP.NET opiera się na powodzenie modelu uwierzytelniania formularzy z platformy ASP.NET 1.x. Uwierzytelnianie formularzy ASP.NET oferuje wygodny sposób włączają formularz logowania do aplikacji platformy ASP.NET i weryfikowania użytkowników przed bazy danych lub innym magazynie danych.


Członkostwo ASP.NET opiera się na powodzenie modelu uwierzytelniania formularzy z platformy ASP.NET 1.x. Uwierzytelnianie formularzy ASP.NET oferuje wygodny sposób włączają formularz logowania do aplikacji platformy ASP.NET i weryfikowania użytkowników przed bazy danych lub innym magazynie danych. Elementy członkowskie klasy FormsAuthentication umożliwiają obsługę plików cookie uwierzytelniania, sprawdź, czy prawidłową nazwą logowania, logowania użytkownika limit itp. Implementowanie uwierzytelniania formularzy w aplikacji ASP.NET 1.x można jednak wymaga odpowiedniej ilości kodu.

Członkostwa programu ASP.NET 2.0 jest głównych przejścia w przypadku uwierzytelniania formularzy samodzielnie. (Członkostwo jest najbardziej niezawodna w połączeniu z uwierzytelniania formularzy, ale przy użyciu uwierzytelniania formularzy nie jest to wymagane). Jak szybko można zauważyć, służy członkostwa ASP.NET i kontrolek logowania programu ASP.NET 2.0 do zaimplementowania systemu członkostwa zaawansowanego bez pisania większej ilości kodu w ogóle.

## <a name="implementing-membership-in-aspnet-20"></a>Implementowanie członkostwa w programie ASP.NET 2.0

Członkostwo jest implementowany przez następujące cztery kroki. Należy pamiętać o tym, czy wiele podrzędnych kroki, które są związane z a także opcjonalnym, który można także wdrożyć. Te kroki są przeznaczone do zilustrowania szerszej konfigurowania członkostwa.

1. Tworzenie bazy danych członkostwa (Jeśli program SQL Server jest używany do przechowywania członkostwa).
2. Określ opcje członkostwa w plikach konfiguracji aplikacji. (Członkostwa jest włączona domyślnie).
3. Określ typ członkostwa magazynu, którego chcesz użyć. Dostępne są następujące opcje: 

    - Microsoft SQL Server (w wersji 7.0 lub nowszy)
    - Magazyn usługi Active Directory
    - Dostawca członkostwa niestandardowych
4. Konfigurowanie aplikacji uwierzytelniania formularzy ASP.NET. Ponownie członkostwo ma korzystać z uwierzytelniania formularzy, ale przy użyciu uwierzytelniania formularzy nie jest wymagane.
5. Zdefiniuj kont użytkowników do członkostwa oraz skonfigurować role, w razie potrzeby.

## <a name="creating-the-membership-database"></a>Tworzenie bazy danych członkostwa

Jeśli youre przy użyciu programu SQL Server 7.0 lub nowszym jako magazyn członkostwa, można użyć aspnet\_narzędzie regsql (dostępne najłatwiej z programu Visual Studio .NET 2005 wiersza polecenia) do konfigurowania bazy danych. Aspnet\_regsql narzędzia mogą służyć jako narzędzie wiersza polecenia lub przy użyciu graficznego interfejsu użytkownika kreatora. Metoda kreatora jest najprostszym sposobem konfiguracji bazy danych. Aby uzyskać dostęp do kreatora, wystarczy uruchomić następujące polecenie:

`aspnet_regsql W`

Po uruchomieniu tego polecenia, użytkownik zobaczy Kreator konfiguracji ASP.NET SQL Server w sposób przedstawiony poniżej.


![](membership/_static/image1.jpg)

**Rysunek 1**


Kreator konfiguracji ASP.NET SQL Server tworzy witrynę sieci Web w wystąpieniu, które określisz w kreatorze. Jednak ASP.NET użyje parametry połączenia w pliku machine.config do połączenia z bazą danych. Domyślnie ten ciąg połączenia wskaże wystąpienia programu SQL Server 2005, dlatego jeśli używasz wystąpienia programu SQL Server 2000 lub SQL Server 7.0, należy zmodyfikować parametrów połączenia w pliku machine.config. Tego ciągu połączenia można znaleźć tutaj:

[!code-xml[Main](membership/samples/sample1.xml)]

Niestety Jeśli nie zmodyfikujesz parametry połączenia, program ASP.NET nie zapewni opisem błędu. Nadal będzie tylko skarżą się, że nie utworzono bazy danych zawierający komunikat. W przypadku powyżej I zostały zmodyfikowane parametry połączenia, aby wskazywał Moje lokalne wystąpienie programu SQL Server 2000.

## <a name="specifying-configuration-and-adding-users-and-roles"></a>Określenie konfiguracji oraz dodanie użytkowników i ról

Następnym etapem konfigurowania członkostwa jest dodać informacje niezbędne do pliku web.config aplikacji. W programie ASP.NET 1.x zmodyfikowanie pliku web.config czasami było trudne ze względu na użycie lowerCamelCase i braku Intellisense. Visual Studio .NET 2005 sprawia, że zadanie jest znacznie łatwiejsze z funkcji Intellisense dla plików konfiguracyjnych, ale ASP.NET 2.0 przechodzi krok dalej, podając interfejs sieci Web edycję plików konfiguracyjnych.

Interfejs sieci Web można uruchomić przez kliknięcie przycisku Konfiguracja platformy ASP.NET na pasku narzędzi Eksplorator rozwiązań, jak pokazano poniżej. Można również uruchomić interfejs sieci Web za pośrednictwem okna podręczne, które są wyświetlane, gdy są wstawiane kontrolek logowania.


![](membership/_static/image2.jpg)

**Rysunek 2**


Spowoduje to uruchomienie narzędzia administrowania witryną sieci Web programu ASP.NET, pokazano poniżej. Administracja witryny sieci Web programu ASP.NET jest interfejsem cztery karty, który ułatwia zarządzanie ustawieniami aplikacji. Dostępne są następujące karty:

- **Strona główna**
- **Zabezpieczenia** skonfigurować użytkowników, ról i dostępu.
- **Aplikacja** skonfigurować ustawienia aplikacji.
- **Dostawca** Konfiguracja i testowanie aplikacji dostawcy członkostwa.

Narzędzie do administrowania witryną sieci Web umożliwia łatwe tworzenie nowych użytkowników, tworzenie nowych ról oraz do zarządzania użytkownikami i rolami. Ta możliwość nie jest dostępna w interfejsie systemu Windows. Interfejsu systemu Windows umożliwia łatwe określenie ustawień autoryzacji i dodawanie, usuwanie i zarządzanie dostawcami, możliwości, które nie należą do narzędzia administrowania witryną sieci Web.

Aby uruchomić interfejs systemu Windows, otwórz przystawkę Internetowe usługi informacyjne, kliknij prawym przyciskiem myszy w swojej aplikacji i wybierz polecenie Właściwości. Kliknij kartę platformy ASP.NET, a następnie kliknij przycisk Edytuj konfigurację. (Aplikacji musi być uruchomiony system programu ASP.NET 2.0 dla przycisku Edycja konfiguracji włączenia. Można skonfigurować wersję platformy ASP.NET w oknie dialogowym ASP.NET.) Zostanie wyświetlone okno dialogowe Ustawienia konfiguracji programu ASP.NET, jak pokazano poniżej.


![](membership/_static/image3.jpg)

**Rysunek 3**


Na karcie Ogólne parametry połączenia i ustawienia aplikacji są wymienione. Wszystkie ustawienia kursywą są zdefiniowane w pliku konfiguracji nadrzędnej (w pliku machine.config lub web.config na wyższym poziomie) i ustawienia kursywą nie pochodzą z pliku konfiguracji aplikacji. Jeśli ustawienie zostanie dodany, usunięte lub edytować na poziomie aplikacji ASP.NET będzie dodać, usunąć lub zmodyfikować ustawienia w pliku web.config poziomy aplikacji zamiast usuwania ustawienie z pliku konfiguracji, z którego został on odziedziczony.

Poniżej przedstawiono kartę uwierzytelnianie. Jest to, gdy skonfigurujesz ustawienia członkostwa. Tworzy ustawienia uwierzytelniania, dostawców członkostwa i dostawców ról można skonfigurować w tym miejscu.


![](membership/_static/image4.jpg)

**Rysunek 4**


## <a name="implementing-membership-in-your-application"></a>Implementowanie członkostwa w aplikacji

Najprostszym sposobem implementowania członkostwa programu ASP.NET 2.0 w aplikacji jest użycie podana kontroli logowania. Ta metoda umożliwia wdrożenie podstawowe informacje dotyczące członkostwa programu ASP.NET 2.0 bez pisania żadnego kodu w ogóle.

Następujące formanty logowania są dostępne w programie ASP.NET 2.0:

## <a name="login-control"></a>Formant logowania

Kontrolka logowania udostępnia interfejs dla kogoś o logowaniu do systemu członkostwa. Umożliwia textboxt nazwy użytkownika i hasła i przycisku Zaloguj. Wielu innych typowych funkcji takich jak łącze do zarejestrowania dla osób, które jeszcze nie zostało zrobione, pole wyboru, która zezwala użytkownikowi na automatyczne logowanie przy kolejnych wizytach, łącze monitu hasła itp. Można dostosować za pomocą właściwości formantu są wszystkie funkcje kontroli logowania.

W programie ASP.NET 1.x, deweloperzy musiał zapisać odpowiedniej ilości kodu w celu wyszukiwania, korzystając z uwierzytelniania formularzy. Członkostwo ASP.NET 2.0 można sprawdzić poprawność użytkowników bez pisania żadnego kodu w ogóle. Program ASP.NET automatycznie wykona wyszukiwanie nazwy użytkownika. (Jeśli używasz kontrolki logowania bez użycia członkostwa ASP.NET, możesz użyć **OnAuthenticate** metody do weryfikacji użytkownika.)

## <a name="loginview-control"></a>Kontrolki widoku logowania

Kontrolki widoku logowania jest szablonem formant, który udostępnia dwa szablony domyślne; AnonymousTemplate i LoggedInTemplate. Szablon, który jest wyświetlany jest określana przez Określa, czy użytkownik jest zalogowany do systemu członkostwa. Ten formant jest zwykle używanych do wyświetlania formantu logowania, gdy użytkownik nie ma jeszcze zalogowany i kontrolki stanu logowania i/lub innych kontrolek logowania po zalogowaniu się użytkownika. Jeśli używasz zarządzania rolami w aplikacji platformy ASP.NET, formantu LoginView można wyświetlić określonego szablonu, zależnie od roli użytkowników. (Więcej na platformie ASP.NET zarządzania rolami zostanie omówiona później.)

## <a name="passwordrecovery-control"></a>Formant PasswordRecovery

Formant PasswordRecovery umożliwia użytkownikom otrzymasz wiadomość e-mail z bieżącego hasła lub zresetować swoje hasło. Zwykły tekst i hasła szyfrowane można odzyskać, a pocztą e-mail do użytkowników. Jeśli hasło jest wartość skrótu, nie można odzyskać. Zamiast tego użytkownik będzie musiał przeprowadzić resetowanie hasła.

## <a name="loginstatus-control"></a>Kontrolki stanu logowania

Kontrolki stanu logowania jest używany do wyświetlania wskaźnik logowania do użytkowników, którzy nie są rejestrowane w i wskaźnika wylogowanie do użytkowników, którzy są aktualnie zalogowany. Właściwość Request.IsAuthenticated jest używana do określenia który wskaźnik do wyświetlenia. Wskaźnik wyświetlany przez formant stanu logowania może być tekstem (implementacja za pośrednictwem **LoginText** i **LogoutText** właściwości) lub obrazów (implementowany za pośrednictwem **LoginImageUrl**i **LogoutImageUrl** właściwości.)

Podczas logowania za pomocą kontrolki stanu logowania, użytkownik zostanie przekierowany adres URL określony przez **LogoutPageUrl** właściwości. Jeśli nie ustawiono tej właściwości, jest odświeżany bieżącej strony. Ponieważ lokacji prawdopodobnie jest chroniony przez uwierzytelnianie oparte na formularzach, Odśwież bieżącej strony przekierowuje użytkownika do strony logowania dla lokacji.

## <a name="loginname-control"></a>Kontrolki nazwy logowania

Kontrolki nazwy logowania Wyświetla nazwy użytkownika użytkownika aktualnie zalogowany do witryny.

## <a name="createuserwizard-control"></a>Formancie CreateUserWizard

W formancie CreateUserWizard zapewnia użytkownikom wygodny sposób rejestrowania systemu członkostwa. Możesz dodać kroki (zaimplementowane jako kolekcji WizardSteps) za pośrednictwem interfejsu pokazano poniżej.


![](membership/_static/image5.jpg)

**Rysunek 5**


CreateUserWizard jest szablonem formant pochodzi od klasy kreatora, który udostępnia następujące szablony:

- **Właściwość HeaderTemplate** ten szablon określa wygląd nagłówka kreatora.
- **Element SidebarTemplate** ten szablon określa wygląd paska bocznego kreatora.
- **StartNavigationTemplate** tej kontrolki szablonu wygląd nawigacji są kreatora w kroku początkowego.
- **StepNavigationTemplate** ten szablon określa wygląd obszar nawigacji nie w kroku rozpoczęcia lub zakończenia.
- **FinishNavigationTemplate** ten szablon określa wygląd obszar nawigacji, gdy w kroku.

Ponadto dla każdego kroku należy dodać do kreatora, ASP.NET spowoduje utworzenie szablonu niestandardowego, który zawiera elementu ContentTemplate i CustomNavigationTemplate dla tego kroku. Aby uzyskać szczegółowe informacje dotyczące dostosowywania CreateUserWizard zobacz dokumentację VS.NET 2005:

## <a name="changepassword-control"></a>Element ChangePassword formantu

Element ChangePassword formantu umożliwia użytkownikom zmianę hasła. Jeśli właściwość DisplayUserName ma wartość true, (jest domyślnie false), użytkownik może zmienić swoje hasło, gdy nie są rejestrowane. Jeśli użytkownik *jest* już zalogowany i właściwość DisplayUserName ma wartość true, użytkownik będzie mógł zmienić hasło innego użytkownika, który nie jest zalogowany w zapewnianiu wiedzieli, identyfikator użytkownika tego użytkownika.

Należy pamiętać, że jeśli chcesz, aby użytkownicy mogli zmieniać hasła bez konieczności logowania, konieczne będzie upewnij się, że strony, na którym jest wyświetlany formant Element ChangePassword zezwala na dostęp anonimowy. Oczywiście użytkownicy będą musieli podać swoje stare hasło w celu zmiany hasła.

## <a name="role-management"></a>Zarządzanie rolami

Zarządzanie rolami służy do przypisywania użytkowników do określonej roli, a następnie ograniczyć dostęp do niektórych plików lub folderów na podstawie tej roli. Zarządzanie rolami także interfejsu API można programowo określić rolę someones lub określić wszyscy użytkownicy w określonej roli i odpowiednio odpowiadać.

Zarządzanie rolami nie jest wymagane w członkostwie w ASP.NET nie jest członkostwo w grupie wymaganie, aby można było używać zarządzania rolami. Jednak dwa uzupełnić wzajemnie dobrze i istnieje duże prawdopodobieństwo, że deweloperzy będą używać ich w połączeniu ze sobą.

Aby włączyć zarządzanie rolami w aplikacji, wprowadź następujące zmiany w pliku web.config:

[!code-xml[Main](membership/samples/sample2.xml)]

Gdy **cacheRolesInCookie** atrybut ma ustawioną wartość true, ASP.NET buforuje członkostwo roli użytkowników, w pliku cookie na komputerze klienckim. Dzięki temu wystąpią, unikając wywołuje element RoleProvider wyszukiwania w roli. Korzystając z tego atrybutu, deweloperzy są zachęcani do upewnij się, że **cookieProtection** atrybut jest ustawiony na wszystkie. (To ustawienie domyślne). Dzięki temu, że dane pliku cookie są szyfrowane i można mieć pewność, że zawartość plików cookie nie zostały zmienione. Role można dodać za pomocą narzędzia administrowania witryną sieci Web. Dzięki temu można łatwo zdefiniować role, konfigurowanie dostępu do części witryny na podstawie tych ról i przypisywania użytkowników do ról.


![](membership/_static/image6.jpg)

**Rysunek 6.**


Jak pokazano powyżej, wystarczy wprowadzić nazwę roli, a następnie klikając przycisk Dodaj rolę można dodawać nowych ról. Istniejące role mogą zarządzane lub usunięty przez kliknąć odpowiednie łącze na liście istniejących ról.

W przypadku zarządzania roli, można dodać lub usunąć użytkowników, jak pokazano poniżej.


![](membership/_static/image7.jpg)

**Rysunek 7**


Zaznaczając pole wyboru jest w roli użytkownika, można łatwo dodać użytkownika do konkretnej roli. Program ASP.NET automatycznie zaktualizuje bazę danych członkostwa odpowiednie wpisy. Należy również skonfigurować zasady dostępu dla aplikacji. Deweloperów platformy ASP.NET 1.x zapoznali się z temu za pośrednictwem &lt;autoryzacji&gt; elementu w pliku web.config, a ta opcja jest nadal dostępny w programie ASP.NET 2.0. Jednak łatwiej zarządzać dostępem zasady przy użyciu witryny sieci Web narzędzia administracyjnego jak pokazano poniżej.


![](membership/_static/image8.jpg)

**Rysunek 8**


W takim przypadku zostanie wyróżniona folderu administracyjnej (jego trudno ponieważ narzędzie wyróżnia ją w kolorze szarym światła) i roli Administratorzy przyznano dostęp. Wszyscy użytkownicy są odrzucane. Możesz kliknąć ikonę head wybierz regułę, a następnie użyj przycisków Przenieś w górę i Przenieś w dół ułożyć reguły. Za pomocą programu ASP.NET &lt;autoryzacji&gt; elementu reguły są przetwarzane w kolejności ich występowania. Innymi słowy Jeśli kolejność reguł w powyższym zrzut zostały wycofane, nikt nie musi dostępu do folderu administracji ponieważ pierwszej reguły, które napotyka ASP.NET będzie regułę, która nie zezwala wszystkim użytkownikom folderu.

Platforma ASP.NET 2.0 dodaje plik web.config do folderu, dla którego określono reguły dostępu. Reguły dostępu mogą być edytowane za pomocą pliku konfiguracji lub za pomocą narzędzia administrowania witryną sieci Web. Innymi słowy narzędzie do administrowania witryną sieci Web jest po prostu interfejs, za pomocą którego można edytować plik konfiguracji w środowisku przyjazną dla użytkownika.

## <a name="using-roles-in-code"></a>Przy użyciu ról w kodzie

Interfejs API do zarządzania rolami nie zmienił się od wersji 1.x. **IsInRole** metoda jest używana do określenia, czy użytkownik ma określoną rolę.

[!code-csharp[Main](membership/samples/sample3.cs)]

ASP.NET również tworzy wystąpienie RolePrincipal jako element członkowski w bieżącym kontekście. Obiekt RolePrincipal może zostać użyty do uzyskania wszystkich ról, do których należy użytkownik w następujący sposób:

[!code-csharp[Main](membership/samples/sample4.cs)]

## <a name="using-rolegroups-with-the-loginview-control"></a>Przy użyciu RoleGroups za pomocą formantu LoginView

Teraz, gdy masz zrozumienia zarządzania rolami i członkostwa umożliwia pokrótce omówiono sposób formantu LoginView korzysta z tej funkcji w programie ASP.NET 2.0. Jak już wspomniano formantu LoginView jest szablonem formant, który zawiera dwa szablony domyślne; AnonymousTemplate i LoggedInTemplate. W ramach LoginView zadania, które okno dialogowe jest link (pokazana poniżej), który służy do edytowania RoleGroups.


![](membership/_static/image9.jpg)

**Rysunek 9**


Każdy obiekt RoleGroup zawiera tablicę ciągów, który definiuje, które role, których dotyczy RoleGroup. Aby dodać nowy RoleGroup do formantu LoginView, kliknij łącze edycji RoleGroups. Na ilustracji powyżej widać, po dodaniu nowego RoleGroup dla administratorów. Po wybraniu tego RoleGroup (RoleGroup[0]) z listy rozwijanej widoki, można skonfigurować szablon, który będzie wyświetlany tylko dla członków roli administratora. Na poniższej ilustracji po dodaniu nowego RoleGroup, która ma zastosowanie do członków roli Sprzedaż i roli dystrybucji. Służy do dodania drugiego RoleGroup widoków listy rozwijanej w oknie dialogowym LoginView zadania i niczego dodane do tego szablonu będą widoczne dla wszystkich użytkowników w sprzedaży lub dystrybucji roli.


![](membership/_static/image10.jpg)

**Na rysunku nr 10**


## <a name="overriding-the-existing-membership-provider"></a>Zastępowanie istniejącego dostawcy członkostwa

Istnieje kilka sposobów rozszerzyć funkcjonalność członkostwa ASP.NET. Po pierwsze oczywiście można zmienić istniejące funkcje klasy SqlMembershipProvider dziedziczenie z tego i przesłanianie jej metod. Na przykład jeśli chcesz wdrożyć własnej funkcji podczas tworzenia użytkowników, można utworzyć własne klasy, która dziedziczy SqlMembershipProvider w następujący sposób:

[!code-csharp[Main](membership/samples/sample5.cs)]

Chcąc, z drugiej strony utworzyć własnego dostawcę (na przykład przechowywać informacje o członkostwie w bazie danych programu Access), można utworzyć własnego dostawcę.

## <a name="creating-your-own-membership-provider"></a>Tworzenie własnego dostawcę członkostwa

Aby utworzyć własnego dostawcę członkostwa, należy najpierw utworzyć klasę, która dziedziczy po klasie MembershipProvider. Jeśli używasz VB.NET programu Visual Studio 2005 doda klas zastępczych dla wszystkich metod, które należy zastąpić. Jeśli używasz C#, jego maksymalnie można dodać klas zastępczych.

Należy zastąpić następujące czynności:

- Właściwość ApplicationName
- Element ChangePassword — funkcja
- Funkcja ChangePasswordQuestionAndAnswer
- Włączenie funkcji
- Funkcja DeleteUser
- Właściwości EnablePasswordReset
- Właściwości EnablePasswordRetrieval
- Funkcja FindUsersByEmail
- Funkcja FindUsersByName
- Funkcja GetAllUsers
- Funkcja GetNumberOfUsersOnline
- Funkcja GetPassword
- Funkcja GetUser
- Funkcja GetUserNameByEmail
- Właściwość MaxInvalidPasswordAttempts
- Wartość MinRequiredNonAlphanumericCharacters właściwości
- Wartość MinRequiredPasswordLength właściwości
- Właściwość PasswordAttemptWindow
- Właściwość PasswordFormat
- Właściwość PasswordStrengthRegularExpression
- Właściwość RequiresQuestionAndAnswer
- Właściwość RequiresUniqueEmail
- Funkcja ResetPassword
- Odblokuj użytkownika — funkcja
- Funkcja UpdateUser
- Funkcja ValidateUser — funkcja

Thats dość listę, aby zaimplementować jako deweloper C#. Użytkownik może ułatwić utworzyć klasę w VB.NET bez implementacji, a następnie użyć reflektora .NET lub podobnego narzędzia, aby przekonwertować kod C#.

Ciąg połączenia i inne właściwości powinna być równa wartości domyślnych w metodę Initialize. (Metoda inicjowania jest uruchamiane, gdy dostawca został załadowany w czasie wykonywania.) Metoda inicjowania drugi parametr jest typu System.Collections.Specialized.NameValueCollection i odwołań do &lt;dodać&gt; element, który jest skojarzony z niestandardowego dostawcy w pliku web.config. Ten wpis wygląda następująco:

[!code-xml[Main](membership/samples/sample6.xml)]

Oto przykład metodę Initialize.

[!code-csharp[Main](membership/samples/sample7.cs)]

W celu potwierdzenia tożsamości użytkownika podczas przesyłania formularza logowania, należy użyć metody funkcja ValidateUser. Ta metoda generowane, gdy użytkownik kliknie przycisk logowania w formancie logowania. Zostanie umieść swój kod, który jest wyszukiwania użytkownika w ramach tej metody.

Jak widać, zapisywanie Dostawca członkostwa nie jest skomplikowane i umożliwia rozszerzenie tego zaawansowanych funkcji programu ASP.NET 2.0.
